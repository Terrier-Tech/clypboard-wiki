In order to make the language a bit more friendly, there are a number of commonly-used functions that have been added to native Javascript types. The functions are listed with their receiver and return types separated by an arrow. For example, `foo :: string|number -> string` acts on a string or number and returns a string, i.e. `"hello".foo()`.

### capitalize :: string -> string
Capitalizes only the first letter of the string

### formatCents(showCents=true) :: string|number -> string
Converts a cents value into dollars, adds the dollar sign, and formats the decimal to show two places (or hides it if showCents=false)

### thousandize :: string|number -> string
Returns a string representing the value in thousands with a _k_ at the end. Some truncation may occur to keep the result short.

```coffeescript
'14324'.thousandize() # '14.3k'
```


### titleize :: string -> string
Capitalizes the first letter of every word

### zeroPad(size) :: string|number -> string
Returns a string that is at least _size_ long, padding the left with zeros if the input isn't long enough.

