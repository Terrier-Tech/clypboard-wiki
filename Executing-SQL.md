## Raw SQL

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
  .select(%w(city state), 'loc', 'location_') # columns, table alias, prefix
  .select(%w(first_name last_name), 'tech', 'technician_') 
  .from('locations', 'loc') # table name, alias
  .left_join('users', 'tech', 'tech.id = loc.primary_technician_id') # table name, alias, clause
  .where("loc.zip = '55124'") # plain string clause
  .where("tech.first_name = 'Billy'")
  .exec

# returns an array of hashes with keys location_city, location_state, 
# technician_first_name, technician_last_name
```

Since SqlBuilder stores all of the components of the query and is a plain Ruby object, it can also be composed conditionally:

```ruby
builder = SqlBuilder.new
  .select(%w(first_name last_name), 'tech')
  .from('users', 'tech')
  .where('tech._state = 0')
  .where("tech.role = 'technician'")

if branch_id
  builder.where("tech.branch_id = '#{branch_id}'")
end

techs = builder.exec
```


