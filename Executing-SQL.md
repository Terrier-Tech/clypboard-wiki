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
