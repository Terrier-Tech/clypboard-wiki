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

### as_objects

Optionally, you can call the `as_objects' method on a SqlBuilder instance to have it return proper Ruby objects instead of hashes as the result. Attribute types will be automatically inferred from the names, including converting from cents to dollars: 

```ruby
locs = SqlBuilder.new
  .select(%w(id number annual_value geo created_at))
  .from('locations')
  .where("zip = '55124')
  .as_objects
  .exec

locs.each do |loc|
  puts loc.number # and integer
  puts loc.annual_value # a dollars float
  puts loc.created_at # a DateTime
end
```

You can also compute additional columns on the result set. Use the `compute_column` method with the name of the new column and a block that accepts each row: 

```ruby
locs.compute_column 'age' do |row|
  Time.now - row.created_at
end
```

Alternatively, you can use `compute_columns` to compute multiple columns at once: 

```ruby
locs.compute_columns do |row|
  geo = row.geo.parse_geo_point
  {latitude: geo.latitude, longitude: geo.longitude}
end
```




