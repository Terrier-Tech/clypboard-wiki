## Strings

Strings are sequences of characters:

```ruby
name = 'andy' # simple strings use single quotes
greeting = "hello, my name is #{name}" # use double quotes for interpolation
# 'hello, my name is andy'
```

Use `puts` to print output:

```ruby
puts 'this will get output'
```

You can change the case of the whole string with `.upcase` and `.downcase`:

```ruby
puts 'Hello World'.upcase
# HELLO WORLD
puts 'Hello World'.downcase
```

Remove extra whitespace with `.strip`:

```ruby
'  this has extra whitespace  '.strip
# 'this has extra whitespace'
```


## Arrays

Arrays are ordered lists of values. Create them with square brackets:

```ruby
names = ['bobby', 'timmy', 'joey']
puts "names has #{names.count} values"
# names has 3 values
numbers = [1, 5, 2, 84]
```
You can use `%w()` as a shorthand to create an array of single-word strings:

```ruby
names = %w(bobby timmy joey) # this is the same as above
```

Arrays are indexed using square brackets, starting at zero:

```ruby
puts "#{names[0]} is the first name and #{names[1]} is the second"
# bobby is the first name and timmy is the second
```

Or you can use shorthand methods like `.first`:

```ruby
puts "#{names.first} is the first name and #{names.second} is the second"
# bobby is the first name and timmy is the second
```

## Split and Join

Strings can be split by separators - like spaces or commas - into arrays: 

```ruby
comps = 'one,two,three,four'.split ','
# ['one', 'two', 'three', 'four']
```

Conversely, arrays can be joined with separators into strings:

```ruby
comps.join ' and '
# 'one and two and three and four'
```




