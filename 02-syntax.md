# Syntax

## Object names

Dataset (or other variable objects) and function names should use only lowercase letters, numbers, and `_`. Use underscores (`_`) (so called snake case) to separate words within a name, don't use a period (`.`) or omit a delimiter between words. 


```r
# Good
day_one
day_1

# Bad
DayOne
day.one
```

Constants (objects that will not change throughout the script) should be named using all uppercase letters, and should be assigned at the top of the script.


```r
CHART_PATH <- "filepath"
```

New column names should follow the convention established in the dataset they belong to. Depending on the source of the dataset, this may be snake_case, CamelCase, ALLCAPS, alllowercase, or another format. 

Generally, variable names should be nouns and function names should be verbs (an exception to this may be functions designed to create charts). Strive for names that are concise and meaningful (this is not easy!). 


```r
# Good
day_one

# Bad
first_day_of_the_month
djm1
```

Where possible, avoid re-using names of common functions and variables. This 
will cause confusion for the readers of your code.


```r
# Bad
T <- FALSE
c <- 10
mean <- function(x) sum(x)
responses$weights <- weights(rake_object)
```

Base R uses dots in function names (`contrib.url()`) and class names 
(`data.frame`), but it's better to reserve dots exclusively for the S3 object 
system. In S3, methods are given the name `function.class`; if you also use 
`.` in function and class names, you end up with confusing methods like
`as.data.frame.data.frame()`. 

## Spacing

### Commas

Always put a space after a comma, never before, just like in regular English.  


```r
# Good
x[, 1]

# Bad
x[,1]
x[ ,1]
x[ , 1]
```

### Parentheses

Do not put spaces inside or outside parentheses for regular function calls.


```r
# Good
mean(x, na.rm = TRUE)

# Bad
mean (x, na.rm = TRUE)
mean( x, na.rm = TRUE )
```

Place a space before and after `()` when used with  `if`, `for`, or `while`.


```r
# Good
if (debug) {
  show(x)
}

# Bad
if(debug){
  show(x)
}
```

Place a space after `()` used for function arguments:


```r
# Good
function(x) {}

# Bad
function (x) {}
function(x){}
```

### Embracing

The embracing operator, `{{ }}`, should always have inner spaces to help emphasise its special behaviour: 


```r
# Good
max_by <- function(data, var, by) {
  data %>%
    group_by({{ by }}) %>%
    summarise(maximum = max({{ var }}, na.rm = TRUE))
}

# Bad
max_by <- function(data, var, by) {
  data %>%
    group_by({{by}}) %>%
    summarise(maximum = max({{var}}, na.rm = TRUE))
}
```


### Infix operators

Most infix operators (`==`, `+`, `-`, `<-`, etc.) should always be surrounded by
spaces:


```r
# Good
height <- (feet * 12) + inches
mean(x, na.rm = TRUE)

# Bad
height<-feet*12+inches
mean(x, na.rm=TRUE)
```

There are a few exceptions, which should never be surrounded by spaces:

*   The operators with [high precedence][syntax]: `::`, `:::`, `$`, `@`, `[`, 
    `[[`, `^`, unary `-`, unary `+`, and `:`.
  
    
    ```r
    # Good
    sqrt(x^2 + y^2)
    df$z
    x <- 1:10
    
    # Bad
    sqrt(x ^ 2 + y ^ 2)
    df $ z
    x <- 1 : 10
    ```
  
*   Single-sided formulas when the right-hand side is a single identifier:

    
    ```r
    # Good
    ~foo
    tribble(
      ~col1, ~col2,
      "a",   "b"
    )
    
    # Bad
    ~ foo
    tribble(
      ~ col1, ~ col2,
      "a", "b"
    )
    ```

    Note that single-sided formulas with a complex right-hand side do need a space:

    
    ```r
    # Good
    ~ .x + .y
    
    # Bad
    ~.x + .y
    ```

*   When used in tidy evaluation `!!` (bang-bang) and `!!!` (bang-bang-bang)
    (because have precedence equivalent to unary `-`/`+`)

    
    ```r
    # Good
    call(!!xyz)
    
    # Bad
    call(!! xyz)
    call( !! xyz)
    call(! !xyz)
    ```

*   The help operator

    
    ```r
    # Good
    package?stats
    ?mean
    
    # Bad
    package ? stats
    ? mean
    ```
    
### Extra spaces

Adding extra spaces ok if it improves alignment of `=` or `<-`. 


```r
# Good
list(
  total = a + b + c,
  mean  = (a + b + c) / n
)

# Also fine
list(
  total = a + b + c,
  mean = (a + b + c) / n
)
```

Do not add extra spaces to places where space is not usually allowed.

## Function calls

### Named arguments {#argument-names}

A function's arguments typically fall into two broad categories: one supplies 
the __data__ to compute on; the other controls the __details__ of computation. 
When you call a function, you typically omit the names of data arguments, 
because they are used so commonly. If you override the default value of an 
argument, use the full name:


```r
# Good
mean(1:10, na.rm = TRUE)
mean(x = 1:10, na.rm = TRUE)

# Bad
mean(x = 1:10, , FALSE)
mean(, TRUE, x = c(1:10, NA))
```

Avoid partial matching.

### Assignment

Avoid assignment in function calls:


```r
# Good
x <- complicated_function()
if (nzchar(x) < 1) {
  # do something
}

# Bad
if (nzchar(x <- complicated_function()) < 1) {
  # do something
}
```

The only exception is in functions that capture side-effects:


```r
output <- capture.output(x <- f())
```


## Control flow

### Code blocks {#indenting}

Curly braces, `{}`, define the most important hierarchy of R code. To make this 
hierarchy easy to see:

* `{` should be the last character on the line. Related code (e.g., an `if` 
  clause, a function declaration, a trailing comma, ...) must be on the same
  line as the opening brace.

* The contents should be indented by two spaces.

* `}` should be the first character on the line.


```r
# Good
if (y < 0 && debug) {
  message("y is negative")
}

if (y == 0) {
  if (x > 0) {
    log(x)
  } else {
    message("x is negative or zero")
  }
} else {
  y^x
}

test_that("call1 returns an ordered factor", {
  expect_s3_class(call1(x, y), c("factor", "ordered"))
})

tryCatch(
  {
    x <- scan()
    cat("Total: ", sum(x), "\n", sep = "")
  },
  interrupt = function(e) {
    message("Aborted by user")
  }
)

# Bad
if (y < 0 && debug) {
message("Y is negative")
}

if (y == 0)
{
    if (x > 0) {
      log(x)
    } else {
  message("x is negative or zero")
    }
} else { y ^ x }
```

### Inline statements {#inline-statements}

It's ok to drop the curly braces for very simple statements that fit on one line, as long as they don't have side-effects.


```r
# Good
y <- 10
x <- if (y < 20) "Too low" else "Too high"
```

Function calls that affect control flow (like `return()`, `stop()` or `continue`) should always go in their own `{}` block:


```r
# Good
if (y < 0) {
  stop("Y is negative")
}

find_abs <- function(x) {
  if (x > 0) {
    return(x)
  }
  x * -1
}

# Bad
if (y < 0) stop("Y is negative")

if (y < 0)
  stop("Y is negative")

find_abs <- function(x) {
  if (x > 0) return(x)
  x * -1
}
```

### Implicit type coercion

Avoid implicit type coercion (e.g. from numeric to logical) in `if` statements: 


```r
# Good
if (length(x) > 0) {
  # do something
}

# Bad
if (length(x)) {
  # do something
}
```

### Switch statements

* Avoid position-based `switch()` statements (i.e. prefer names).
* Each element should go on its own line.
* Elements that fall through to the following element should have a space after `=`.
* Provide a fall-through error, unless you have previously validated the input. 


```r
# Good 
switch(x
  a = ,
  b = 1, 
  c = 2,
  stop("Unknown `x`", call. = FALSE)
)

# Bad
switch(x, a = , b = 1, c = 2)
switch(x, a =, b = 1, c = 2)
switch(y, 1, 2, 3)
```

## Long lines

Strive to limit your code and comments to 80 characters per line. This fits comfortably on a printed page with a reasonably sized font. If you find yourself running out of room, this is a good indication that you should encapsulate some of the work in a separate function. File path names and character strings may be exempt as they will often be longer than 80 characters.

You can add a faint overlayed line to the source pane in RStudio to mark the width of 80 characters by clicking on RStudio in the menu bar and navigating to Preferences > Code > Display and selecting Show margin and setting column margin to 80. 

If a function call is too long to fit on a single line, use one line each for 
the function name, each argument, and the closing `)`. 
This makes the code easier to read and to change later. 


```r
# Good
do_something_very_complicated(
  something = "that",
  requires = many,
  arguments = "some of which may be long"
)

# Bad
do_something_very_complicated("that", requires, many, arguments,
                              "some of which may be long"
                              )
```

As described under [Argument names], you can omit the argument names
for very common arguments (i.e. for arguments that are used in almost every 
invocation of the function). Short unnamed arguments can also go on the same 
line as the function name, even if the whole function call spans multiple lines.


```r
map(x, f,
    extra_argument_a = 10,
    extra_argument_b = c(1, 43, 390, 210209)
)
```

You may also place several arguments on the same line if they are closely 
related to each other, e.g., strings in calls to `paste()` or `stop()`. When 
building strings, where possible match one line of code to one line of output. 


```r
# Good
paste0(
  "Requirement: ", requires, "\n",
  "Result: ", result, "\n"
)

# Bad
paste0(
  "Requirement: ", requires,
  "\n", "Result: ",
  result, "\n")
```

There are a variety of acceptable spacing styles for function arguments. While you can choose your personal preference for how to space function arguments, please be sure to use a consistent style throughout a script. 


```r
# Good
responses %>% 
  mutate(new_var = case_when(
    old_var == "This is a good example" ~ "Fewer than 80 characters"
  ))

responses %>% 
  mutate(
    new_var = case_when(
      old_var == "This is a good example" ~ "Fewer than 80 characters"
    )
  )

mod <- lm(formula = response_variable ~ predictor_1 + predictor_2 + predictor_3,
          data = responses)

mod <- lm(formula = response_variable ~ 
            predictor_1 + 
            predictor_2 + 
            predictor_3,
          data = responses)

mod <- lm(
  response_variable ~ 
    predictor_1 + 
    predictor_2 + 
    predictor_3,
  data = responses
)

# Bad
responses %>% 
  mutate(new_var = case_when(old_var == "This is an example of what not to do" ~ "More than 80 characters"))

mod <- lm(formula = response_variable ~ predictor_1 + predictor_2 + predictor_3, data = responses)
```

## Semicolons

Don't put `;` at the end of a line, and don't use `;` to put multiple commands 
on one line.

## Assignment

Use `<-`, not `=` or `%<>%`, for assignment. (The `=` operator is used inside of mutate functions.)


```r
# Good
x <- 5

# Bad
x = 5
x %<>% 5
```



## Data

### Character vectors

Both `"` and `'` may be used for quoting text, but one style of quotes should be used consistently throughout a script. The only exception is when the text already contains double quotes and no single quotes or vice versa. 


```r
# Preferred by the tidyverse style guide
"Text"
'Text with "quotes"'
'<a href="http://style.tidyverse.org">A link</a>'

# Also acceptable
'Text'
'Text with "double" and \'single\' quotes'
```

### Logical vectors

Prefer `TRUE` and `FALSE` over `T` and `F`.

## Comments

In code, use comments to explain the "why" not the "what" or "how". Each line 
of a comment should begin with the comment symbol and a single space: `# `.


```r
# Good

# Objects like data frames are treated as leaves
x <- map_if(x, is_bare_list, recurse)


# Bad

# Recurse only with bare lists
x <- map_if(x, is_bare_list, recurse)
```

In data analysis code, use comments to record important findings and analysis 
decisions. If you need comments to explain what your code is doing, consider 
rewriting your code to be clearer. If you discover that you have more comments 
than code, consider switching to [R Markdown][rmd].

Comments should not exceed 80 characters per line. If a comment requires more space, use multiple lines and/or block commenting. Entering on a comment line starting with `#'` will ensure the next line is also commented out. 


```r
# Good
# Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam pharetra 
# nulla enim, et venenatis neque bibendum ac. 

#' Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam pharetra 
#' nulla enim, et venenatis neque bibendum ac. Sed lorem nisl, blandit at
#' ante ut, luctus euismod lectus. Vivamus interdum ornare massa, condimentum 
#' cursus velit tincidunt vitae. Nunc sit amet quam elementum, consequat elit 
#' quis, consectetur est. Sed in eros ipsum. Etiam sed mattis tortor.


# Bad
# Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nam pharetra nulla enim, et venenatis neque bibendum ac. Sed lorem nisl, blandit at ante ut, luctus euismod lectus. 
```


[syntax]: https://rdrr.io/r/base/Syntax.html
[rmd]:    https://rmarkdown.rstudio.com/
