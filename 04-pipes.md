# Pipes

## Piping Introduction

Use `%>%` to emphasise a sequence of actions, rather than the object that the actions are being performed on. 

Avoid using the pipe when:

* You need to manipulate more than one object at a time. Reserve pipes for a 
  sequence of steps applied to one primary object.
 
* There are meaningful intermediate objects that could be given
  informative names.

## Whitespace

`%>%` should always have a space before it, and should usually be followed by a new line. After the first step, each line should be indented by two spaces. This structure makes it easier to add new steps (or rearrange existing steps) and harder to overlook a step.


```r
# Good
iris %>%
  group_by(Species) %>%
  summarize_if(is.numeric, mean) %>%
  ungroup() %>%
  gather(measure, value, -Species) %>%
  arrange(value)

# Bad
iris %>% group_by(Species) %>% summarize_all(mean) %>%
ungroup %>% gather(measure, value, -Species) %>%
arrange(value)
```

## Long lines

If the arguments to a function don't all fit on one line, put each argument on 
its own line and indent:


```r
iris %>%
  group_by(Species) %>%
  summarise(
    Sepal.Length = mean(Sepal.Length),
    Sepal.Width = mean(Sepal.Width),
    Species = n_distinct(Species)
  )
```

## Short pipes

A one-step pipe can stay on one line, but unless you plan to expand it later on, you should consider rewriting it to a regular function call.


```r
# Good
iris %>% arrange(Species)

iris %>% 
  arrange(Species)

arrange(iris, Species)
```

Sometimes it's useful to include a short pipe as an argument to a function in a 
longer pipe. Carefully consider whether the code is more readable with a short 
inline pipe (which doesn't require a lookup elsewhere) or if it's better to move 
the code outside the pipe and give it an evocative name.


```r
# Good
x %>%
  select(a, b, w) %>%
  left_join(y %>% select(a, b, v), by = c("a", "b"))

# Better
x_join <- x %>% select(a, b, w)
y_join <- y %>% select(a, b, v)
left_join(x_join, y_join, by = c("a", "b"))
```

## No arguments

magrittr allows you to omit `()` on functions that don't have arguments. Avoid  this feature.


```r
# Good
x %>% 
  unique() %>%
  sort()

# Bad
x %>% 
  unique %>%
  sort
```

## Assignment

Acceptable forms of assignment include:

*   Variable name and assignment on the same line (preferred method):

    
    ```r
    iris_long <- iris %>%
      gather(measure, value, -Species) %>%
      arrange(-value)
    ```

*   Variable name and assignment on separate lines:

    
    ```r
    iris_long <-
      iris %>%
      gather(measure, value, -Species) %>%
      arrange(-value)
    ```

The magrittr package provides the `%<>%` operator as a shortcut for modifying an object in place. Avoid this operator.


```r
# Good
x <- x %>% 
  abs() %>% 
  sort()
  
# Bad
x %<>%
  abs() %>% 
  sort()
```

Multiple piping steps should be used over repeatedly reassigning a dataset. 


```r
# Good
responses <- responses %>% 
  filter(Q5 == 1) %>% 
  mutate(new_variable = case_when(
    # do something
  ))

# Bad
responses <- responses %>% 
  filter(Q5 == 1)

responses <- responses %>% 
  mutate(new_variable = case_when(
    # do something
  ))
```

