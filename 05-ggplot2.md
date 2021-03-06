# ggplot2


## ggplot introduction

Styling for `+` used to separate ggplot2 layers is very similar to that of `%>%` in pipelines.

## Whitespace

`+` should always have a space before it, and should be followed by a new line. This is true even if your plot has only two layers. After the first step, each line should be indented by two spaces.

If you are creating a ggplot off of a dplyr pipeline, there should only be one level of indentation.


```r
# Good
iris %>%
  filter(Species == "setosa") %>%
  ggplot(aes(x = Sepal.Width, y = Sepal.Length)) +
  geom_point()

# Bad
iris %>%
  filter(Species == "setosa") %>%
  ggplot(aes(x = Sepal.Width, y = Sepal.Length)) +
    geom_point()

# Bad
iris %>%
  filter(Species == "setosa") %>%
  ggplot(aes(x = Sepal.Width, y = Sepal.Length)) + geom_point()
```


## Long lines

ggplot2 allows you to do data manipulation, such as filtering or slicing, within the `data` argument. Avoid this whenever possible, and instead do the data manipulation in a pipeline before starting plotting. 


```r
# Good
iris %>%
  filter(Species == "setosa") %>%
  ggplot(aes(x = Sepal.Width, y = Sepal.Length)) +
  geom_point()

# Bad
ggplot(filter(iris, Species == "setosa"), aes(x = Sepal.Width, y = Sepal.Length)) +
  geom_point()
```

## Structure

Generally, `geom` layers should come immediately after the initial `ggplot` layer. If the theme needs to be adjusted, do so at the end of the plot, after all other layers. Try to order layers in a sensible way that is easy to understand, i.e. grouping all geom layers together, grouping all axis manipulation layers together. 

Include the `aes` argument as the first argument in `geom` layers that require it. 
