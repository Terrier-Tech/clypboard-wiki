## Strings

```ruby
name = 'andy' # simple strings use single quotes
greeting = "hello, my name is #{name}" # use double quotes for interpolation
# 'hello, my name is andy'
```
Use `puts` to print output:

```ruby
puts 'this will get output'
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



