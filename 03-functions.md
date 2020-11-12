# Functions

## Naming

As well as following the general advice for object names, strive to use verbs for function names (excepting functions to create charts, in which case the function name can describe the style of chart it creates):


```r
# Good
add_row()
permute()

# Bad
row_adder()
permutation()
```

## Long lines

If a function definition runs over multiple lines, indent the second line to 
where the definition starts.


```r
# Good
long_function_name <- function(a = "a long argument",
                               b = "another argument",
                               c = "another long argument") {
  # As usual code is indented by two spaces.
}

# Bad
long_function_name <- function(a = "a long argument",
  b = "another argument",
  c = "another long argument") {
  # Here it's hard to spot where the definition ends and the
  # code begins
}
```

## `return()`

Only use `return()` for early returns. Otherwise, rely on R to return the result 
of the last evaluated expression.


```r
# Good
find_abs <- function(x) {
  if (x > 0) {
    return(x)
  }
  x * -1
}
add_two <- function(x, y) {
  x + y
}

# Bad
add_two <- function(x, y) {
  return(x + y)
}
```

Return statements should always be on their own line because they have important effects on the control flow. See also [inline statements](#inline-statements).


```r
# Good
find_abs <- function(x) {
  if (x > 0) {
    return(x)
  }
  x * -1
}

# Bad
find_abs <- function(x) {
  if (x > 0) return(x)
  x * -1
}
```

If your function is called primarily for its side-effects (like printing, 
plotting, or saving to disk), it should return the first argument invisibly. 
This makes it possible to use the function as part of a pipe. `print` methods 
should usually do this, like this example from [httr](http://httr.r-lib.org/):


```r
print.url <- function(x, ...) {
  cat("Url: ", build_url(x), "\n", sep = "")
  invisible(x)
}
```

## Comments in functions

In functions intended for publication in a package, comments should be in sentence case, and only end with a full stop if they contain at least two sentences:


```r
# Good

# Objects like data frames are treated as leaves
x <- map_if(x, is_bare_list, recurse)

# Do not use `is.list()`. Objects like data frames must be treated
# as leaves.
x <- map_if(x, is_bare_list, recurse)


# Bad

# objects like data frames are treated as leaves
x <- map_if(x, is_bare_list, recurse)

# Objects like data frames are treated as leaves.
x <- map_if(x, is_bare_list, recurse)
```
