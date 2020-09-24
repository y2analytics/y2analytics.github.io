# Files

## File Names

File names should be concise but meaningful and end in `.R`. Avoid using special characters in file names - stick with numbers, letters, `-`, and `_`. Include your initials at the end of a file name. 

For projects with multiple code files, include a two digit number at the beginning of the file name. This makes it easy to understand the workflow of past projects. If multiple analysts are coding the same step of a project (such as chart creation), each code file should begin with the same number. If it seems likely you'll have more than 10 files, left pad with zero:

    # Good
    01 Modeling MTG.R
    04 - utility_functions XYZ.R
    06 Charts ABC.R
    06 Charts DEF.R

    # Bad
    1fitModels.R
    foo.r
    stuff.r

## Internal structure 

### Sections

Use commented lines of `-` and `=` to break up your file into easily readable 
chunks. The keyboard shortcut for creating sections is `command + shift + r`. Section headers should describe the research objective of the code they contain.


```r
# Load data ---------------------------

# Plot data ---------------------------
```

### Packages

If your script uses add-on packages, load them all at once at the very 
beginning of the file. This is more transparent than sprinkling `library()` 
calls throughout your code or having hidden dependencies that are loaded in a 
startup file, such as `.Rprofile`.

Do not load packages you do not use. Never load the `magrittr` package.

Do not include `install.packages('packagename')` in your code, whether or not the command is commented out. 

If you need to load the `plyr` package, do so before loading `tidyverse` or `dplyr`.

### File paths

Add a project path constant at the top of your code, and use the project path to read in and save out data files and output. This makes it easy to change the project path and avoid errors when the project folder is moved. 


```r
PROJECT_PATH <- "~/Dropbox (Y2 Analytics)/Y2 Analytics Team Folder/Resources/Coding Style Guide/"

responses <- read_sav(str_c(PROJECT_PATH, "data file.sav"))
```

### Data manipulation

New variables that will be referenced more than once should be created in a global mutate section towards the top of the code, close to or in the same section where the data is read in.  If a variable will only be used in one section of analysis then it should be created as close to its use as possible. 

Global variables you create should be given new names rather than replacing the existing variable they are based on. 

Variables should be created using tidyverse functions and syntax where possible. Data manipulation should be done in as few steps as possible-don't use multiple mutate functions where one would suffice. 


```r
# Good
responses <- responses %>% 
    mutate(
        var_example_1 = case_when(
            # do something
        ),
        var_example_2 = case_when(
            # do something
        ),
        var_example_3 = case_when(
            # do something
        ))

# Bad
responses <- responses %>% 
    mutate(
        var_example_1 = case_when(
            # do something
        )
    ) %>% 
    mutate(
        var_example_2 = case_when(
            # do something
        )
    )
# some other code
responses <- responses %>% 
    mutate(var_example_3 = case_when(
        #do something
    ))
```

### Annotation and commented code

Code should be annotated in a way that makes it easy to understand without being intimately familiar with the project. Annotations should be included throughout a code file, and should explain *why* analysis is being done rather than describing *what* analysis is being performed.

Data exploration should not be included unless it is relevant to analysis. If data exploration is useful, annotate why you were checking something and comment out the code.

### Indentation

Code should have consistent indenting through the file. An easy way to make sure your indentation is standardized is to highlight a code chunk and use `command + I`. 

### Function use

Functions should be used in place of copying and pasting code chunks. A good rule of thumb is if you have to reuse code more than 3 times, you should create a function.

### Reproducibility 

After writing a script, ensure that it runs correctly and is reproducible by cleaning R's memory and rerunning the script.