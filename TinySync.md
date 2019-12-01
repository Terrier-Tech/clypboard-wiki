In order to access the database and application session information from the client-side JavaScript we use the TinySync JavaScript API.

This API abstracts the differences between the web and Android TinySync implementations, allowing us to use the same JavaScript code on both platforms.

#### Session

The TinySync session is a key/value store of information particular to the current user's session (such as the user id).
The session information is stored in local storage in the regular browser and shared preferences in Android, although these differences are hidden to the user.

The API is simple, providing just a get() and set() method:

```javascript
tinysync.session.set('key', 'value')

tinysync.session.get('key') // -> 'value'
```

#### Database

Access to the database is provided with a simple CRUD API in JavaScript.
On the web, it uses a set of AJAX web services.
On Android, it uses a custom Java bridge class.

To retrieve objects from the database, use the get() method.
Its arguments are: the name of the collection, the query, and a callback function:

```javascript
tinysync.db.get(
    'location',
    {where: {search_tags: 'main'}, include: 'customer', limit: 10, order: {last_name: 'ASC'}},
    function (result) {
        alert('Found ' + result.records.length + ' locations!')
    }
)
```
    
The query is object with the following (optional) fields:

* _where_: a set of key/value pairs that specify the *where* clause of the query
* _limit_: the number of results to return
* _order_: an object with fields declaring the sort order, i.e. {name: 'ASC'}
* _include_: an array of associations to include, i.e. ['customer'] while getting locations
 
The result object (for all database functions) has the form:

* _status_: either 'success' or 'error', depending on whether the request succeeded
* _collection_: the name of the collection you're accessing, in case you forgot
* _message_: a (hopefully) informative message about what happened
* _records_: the database records returned in the query (or created/updated), always an array
* _errors_: an array of error objects (only for create/update)

If you assume that the query will succeed and are okay with some default error handling if it fails
(i.e. all waiting overlays are cleared and an alert pops with the error message), 
then you can use the _safe_ variant that returns the resulting objects directly:

```javascript
tinysync.db.safeGet(
    'location',
    {where: {search_tags: 'main'}, include: 'customer', limit: 10, order: {last_name: 'ASC'}},
    function (locations) {
        alert('Found ' + locations.length + ' locations!')
    }
)
```

If you know the if of a record and only want that one value, you can use the find() method instead:

```javascript
tinysync.db.safeGet(
    'location',
    '2mrJZKtNbMHoaJ',
    {include: 'customer'},
    function (location) {
        alert('Found location ' + location.display_name)
    }
)
```

find() also takes an options argument where you can specify the same _include_ clause as get().

To count the number of objects returned from a given query, without actually serializing them all (in order to improve performance),
you can use the count() function:

```javascript
tinysync.db.count(
    'location',
    {where: {zip: '55124'}},
    function (result) {
        alert('Found ' + result.count + ' locations!')
    }
)
```

Creating and updating records is done with similar methods, 
except instead of sending a query object you send an object containing the record data.
The response from the server will have the same format as the get() function, 
and will contain the full record if the operation was a success. 
Note that the second argument must be an object with the name of the entity as the key:

```javascript
tinysync.db.create(
    'customer',
    {customer: {number: 1234567, name: 'Demo Customer'}},
    function (result) {
        if (result.status == 'success')
            alert('Created customer ' + result.records[0].id)
        else
            alert('Error creating customer: ' + result.message)
    }
)
```

The update method is nearly identical, except that you must explicitly pass the id of the record you'd like to update.

```javascript
tinysync.db.update(
    'customer',
    '53949ab5caa9d04a6a0000f9',
    {customer: {number: 1234567, name: 'Demo Customer'}},
    function (result) {
        if (result.status == 'success')
            alert('Updated customer ' + result.records[0].id)
        else
            alert('Error updating customer: ' + result.message)
    }
)
```

In order to write code that works for both new and existing records, you can use the upsert() function.
It takes the same arguments as create(), but will update an existing record if the object you're passing has an id:

```javascript
tinysync.db.upsert(
    'customer',
    {customer: someCustomerData},
    function (result) {
        if (result.status == 'success')
            alert('Upserted customer ' + result.records[0].id)
        else
            alert('Error upserting customer: ' + result.message)
    }
)
```

The create, update, upsert, and count methods all have _safe_ variants similar to _safeGet_. 

The destroy method deletes a record from the database, and only requires an id.

```javascript
tinysync.db.destroy(
    'customer',
    '53949ab5caa9d04a6a0000f9',
    function (result) {
        if (result.status == 'success')
            alert('Deleted customer ' + result.records[0].id)
        else
            alert('Error deleting customer: ' + result.message)
    }
)
```