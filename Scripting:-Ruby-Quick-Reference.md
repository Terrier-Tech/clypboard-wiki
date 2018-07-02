## Strings

Strings are sequences of characters. Use `puts` to send strings to the output: 

```ruby
name = 'andy' # simple strings use single quotes
puts "hello, my name is #{name}" # use double quotes for interpolation
# 'hello, my name is andy'
```

You can change the case of the whole string with `.upcase` and `.downcase`:

```ruby
puts 'Hello World'.upcase
# HELLO WORLD
puts 'Hello World'.downcase
# hello world
```

More usefully, you can capitalize just the first letter of each word with `.titleize`:

```ruby
'the great gatsby'.titleize
# 'The Great Gatsby'
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


## Loops

Iterate through each element of an array with `.each`, which takes a block as an argument. Blocks can be declared with either `{...}` or `do...end`. The latter is generally only used if the block is a single line.

```ruby
names.each{|name| puts name} 
# is exactly the same as:
names.each do |name|
  puts name
end
# bobby
# timmy
# joey
```
Inside the block, the current element gets assigned to whatever variable name is placed between the pipes (`name`, in this case).


