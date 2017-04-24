In order to make the language a bit more friendly, there are a number of commonly-used functions that have been added to native Javascript types. The functions are listed with their receiver and return types separated by an arrow. For example, *foo :: string|number -> string* acts on a string or number and returns a string, i.e. _"hello".foo()_.

### capitalize :: string -> string
Capitalizes only the first letter of the string

### formatCents(showCents=true) :: string|number -> string
Converts a cents value into dollars, adds the dollar sign, and formats the decimal to show two places (or hides it if showCents=false)

### titleize :: string -> string
Capitalizes the first letter of every word

### zeroPad :: string|number -> string

