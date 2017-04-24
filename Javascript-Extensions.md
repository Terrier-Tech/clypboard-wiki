In order to make the language a bit more friendly, there are a number of commonly-used functions that have been added to native Javascript types. The functions are listed with their receiver and return types separated by an arrow.

### abbreviate :: string -> string
Creates an abbreviation using the first two letters in each word.
```coffeescript
'Hello World'.abbreviate() # 'HeWo'
```

### capitalize :: string -> string
Capitalizes only the first letter of the string.
```coffeescript
'hello world'.capitalize() # 'Hello world'
```

### formatCents(showCents=true) :: string|number -> string
Converts a cents value into dollars, adds the dollar sign, and formats the decimal to show two places (or hides it if showCents=false).
```coffeescript
'1234'.formatCents() # '$12.34'
```

### formatPhone :: string -> string
Sanitizes a phone number to have consistent formatting.
```coffeescript
'9525551234'.formatPhone() # '952-555-1234'
```

### formatPhoneAndType :: string -> string
Sanitizes a phone number to have consistent formatting, prefixing with a (titleized) type.
```coffeescript
'9525551234'.formatPhoneAndType('cell') # 'Cell: 952-555-1234'
```

### joinWithCommas :: array -> string
Joins the array using english-y constructs.
```coffeescript
['one', 'two'].joinWithCommas() # 'one and two'
['one', 'two', 'three'].joinWithCommas() # 'one, two, and three'
```

### newlinesToBreaks :: string -> string
Converts all newlines in the string to break tags.
```coffeescript
"hello\nWorld".newlinesToBreaks(1) # 'Hello<br/>World'
```

## pluralize(num) :: string -> string
Pluralizes the string if num is greater than 1.
```coffeescript
'cat'.pluralize(1) # 'cat'
'cat'.pluralize(2) # 'cats'
```

## singularize(num) :: string -> string
Singularizes the string if num is 1.
```coffeescript
'cats'.singularize(1) # 'cat'
'cats'.singularize(2) # 'cats'
```

### thousandize :: string|number -> string
Returns a string representing the value in thousands with a _k_ at the end. Some truncation may occur to keep the result short.
```coffeescript
'324'.thousandize() # '324'
'14324'.thousandize() # '14.3k'
```

### titleize :: string -> string
Capitalizes the first letter of every word.
```coffeescript
'hello world'.titleize() # 'Hello World'
```

### zeroPad(size) :: string|number -> string
Returns a string that is at least _size_ long, padding the left with zeros if the input isn't long enough.
```coffeescript
'12'.zeroPad(4) # '0012'
```

