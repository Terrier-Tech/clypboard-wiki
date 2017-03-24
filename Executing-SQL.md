SQL queries can be executed directly using: 

```ruby
PostgresBase.raw_sql query
```

The result is an enumerable object that will iterate over a list of hashes containing the query results. For example:

```ruby
PostgresBase.raw_sql('select number from locations where zip = '55124').each do |loc|
  puts loc['number']
end
```

## SQL Builder

Alternatively, you can use the SqlBuilder class to compose queries in pure Ruby. SqlBuilder uses a chainable builder syntax to make constructing queries for flexible than using plain strings. It also automatically handles things like applying prefixes to column names and managing aliases.

```ruby
# sql builder example
locs = SqlBuilder.new
  .select(%w(address1 city state), 'loc', 'location_') # columns, table alias, prefix
  .select(%w(first_name last_name), 'tech', 'technician_') 
  .from('locations', 'loc') # table name, alias
  .left_join('users', 'tech', 'tech.id = loc.primary_technician_id') # table name, alias, clause
  .where("loc.zip = '55124'") # plain string clause
  .exec
# returns an array of hashes with keys location_address1, location_city, location_state, technician_first_name, technician_last_name
```



