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

## Symbols

Symbols are similar to strings except that they can't contain any spaces or special characters. They are used primarily as hash keys (see Hashes below). Symbols are created without quotes, just prepend the value with a colon:

```ruby
foo = :bar # this is a symbol
foo.to_s # convert a symbol to a string
# 'bar'
'bar'.to_sym # convert a symbol to a string
# :bar
```

## Currency

There are `dollars` and `cents` methods to help convert numbers to human-readable currency:

```ruby
puts 1234.dollars # dollar sign and commas automatically added
# $1,234.00
puts 1234.cents
# $12.34
puts 1234.dollars(false) # pass false to hide the cents
# $1,234
puts 1234.cents(false)
# $12
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

Similarly, an array fo symbols can be created with `%i()`:

```ruby
symbols = %i(one two three)
# [:one, :two, :three]
```

Arrays are indexed using square brackets, starting at zero:

```ruby
puts "#{names[0]} is the first name and #{names[1]} is the second"
# bobby is the first name and timmy is the second
```

Negative indexes count from the end of the array:

```ruby
puts "#{names[-1]} is the last name and #{names[-2]} is the second to last"
# joey is the last name and timmy is the second to last
```

Or you can use shorthand methods like `.first` and `.last`:

```ruby
puts "#{names.first} is the first name and #{names.last} is the last"
# bobby is the first name and joey is the last
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
Inside the block, the current element gets assigned to whatever variable name is placed between the pipes (`name`, in this case). This can be whatever you want. 

In Clypboard scripts specifically, there is a special `each` method that is at the top level. It works just like `.each` except that it enables a special `puts_count` method to make it easy to see the progress of your loop:

```ruby
each names do |name|
  puts_count name
end
# [1 of 3] bobby
# [2 of 3] timmy
# [3 of 3] joey
```


## Hashes

Hashes are collections of key-value pairs. They are used to associate one thing (the key) with another thing (the value). Hashes are created using curly brackets, with keys and values separated by colons:

```ruby
person = {
  name: 'billy',
  age: 43,
  email: 'billy@example.com'
} # this is a hash
```

Hashes created this way have symbols as keys. Just like arrays, the values can be accessed with square brackets:

```ruby
puts "#{person[:name]} is #{person[:age]} years old"
# billy is 43 years old
person[:age] = 44
puts "his age is now #{person[:age]}"
# his age is now 44
```

You can get all of the keys or all of the values from a hash using the `.keys` and `.values` methods, respectively. Both will return an array.

```ruby
person.keys
# [:name, :age, :email]
person.values
# ['billy', 44, 'billy@example.com']
```

Hashes also have an `.each` method which iterates over it's values, except that the block will also accept the key as the first argument:

```ruby
person.each do |key, value|
  puts "#{key.to_s.titleize}: #{value}"
end
# Name: billy
# Age: 44
# Email: billy@example.com
```

You can also use non-symbols - like strings - as keys for hashes. In this case, you have to use the "fat arrow" syntax:

```ruby
person = {
  "full name" => "Sam Smith",
  "years old" => 26
}
person.keys
# ['full name', 'years old'] 
```

This syntax is used when you need keys that contain special characters like spaces or periods.


## Map

The `.map` method is similar to `.each` except that it collects the results of each iteration and returns them as a separate array. For example, you could take an array of numbers and convert them to formatted strings:

```ruby
numbers = [1, 4, 6]
strings = numbers.map do |num|
  "#{num}*2 is #{num*2}"
end
# ['1*2 is 2', '4*2 is 8', '6*2 is 12']
```

`.map` can also be used to convert strings into other strings:

```ruby
%w(one two three).map{|s| s.titleize}
# ['One', 'Two', 'Three']
```


## & Shorthand

Instead of specifying a block to pass to `.each` or `.map`, you can pass a symbol prefixed with an ampersand to specify a method to call on each element:

```ruby
%w(one two three).map &:titleize # same as above
# ['One', 'Two', 'Three']
[1.2, 5.8, 7.1].map &:round
# [1, 6, 7]
[' one', 'two ', ' three '].map(&:upcase).map(&:strip)
# ['ONE', 'TWO', 'THREE']
```


## Sorting

The simplest way to sort an array is with the `sort` method. This will sort the contents of the array using standard alphanumerical order:

```ruby
['gamma', 'beta', 'alpha'].sort
# ['alpha', 'beta', 'gamma']

[3, 1, 6, 5].sort
# [1, 3, 5, 6]
```

This only works for arrays with strings or numbers, though. To sort an array of arbitrary values, use the `sort_by` method. It takes a block that computes a value to use for sorting:

```ruby
a = [
  {name: 'zeke'},
  {name: 'alice'},
  {name: 'matt'}
]
a.sort_by {|h| h[:name]}
# [{name: 'alice'}, {name: 'matt'}, {name: 'zeke'}]
```

If you're sorting objects and want to sort by the result of one its methods, you can use the & shorthand:

```ruby
strings = %w(longword foo bars)
strings.sort_by(&:size)
# ['foo', 'bars', 'longword']
```


## Filtering

To filter the contents of an array using a certain criteria, use the `select` method:

```ruby
a = [3, 6, 9, 1, 2, 4, 7]
a.select {|n| n < 6}
# [3, 1, 2, 4]
```

