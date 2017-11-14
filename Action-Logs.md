## How It Works

The action logging is now a two-step process:
1. When a record is created, updated, or deleted, an action log entry is made into the local database - like it's always been. 
1. A background task periodically sends these records to our cloud datastore. 

**All read operations are performed from the cloud**, so it's possible you may not see a log right after an operation occurs. The task runs every minute and syncs 200 logs at a time, so it's generally no more than a minute behind. This obviously goes out the window for things like invoicing, but even then it should be no more than a day behind. 

## Getting Logs

You can get the action logs for any record using the ActionLogCloud class: 

```ruby
logs = ActionLogCloud.get location
```

If you know the ID of the record but don't have an instance sitting around (i.e. because it's been deleted), you just need to make a temporary one:

```ruby
logs = ActionLogCloud.get LocationCredit.new(id: '4ttaNlfbY1vmwB')
```

Either way, this method will return an array of ActionLog objects.

## Querying

Besides just getting logs by record, you can perform some more arbitrary searches using ActionLogCloud#query:

```ruby
logs = ActionLogCloud.query :_time.gt => '2017-10-01', _action_type: 'delete'
```

In the example above, we're querying on metadata of the logs themselves. The possible metadata fields are: 
* _time
* _action (create|update|delete)
* _user_id
* _user_name
* _record_type (class name)
* _record_id

In addition to the metadata fields, you can also query on a set of special fields that have been indexed for convenience:

```ruby
logs = ActionLogCloud.query location_id: '4ttaNlfbY1vmwB'
```

This will find the _create_ logs for all records associated with the given location id. _NOTE: it will also find update and delete logs created after 2017-11-14._

The available special fields are: 
* location_id
* work_order_id
* invoice_id

## Non-indexed Queries

Technically, you can query by _any_ changed field:

```ruby
logs = ActionLogCloud.query subtotal: 1200
```

This example will find all changes where the record's subtotal value went either two or from 1200. 

The only caveat to non-indexed querying (besides the fact that it might be a bit slow) is that you can only query one field at a time. So, something like this won't work: 

```ruby
logs = ActionLogCloud.query subtotal: 1200, :_time.gt => '2017-01-01' # ERROR: need a compound index
```

If you ever run into the compound index error, I can always create one. I've already made indexes for some standard combinations (_time along with one of: _action, _record_type, _user_id, or one of the special fields).





