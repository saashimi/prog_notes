# R / Datascience Notes

The advantage of using .rproj files is that working directory is relative to 
the .rproj file. Can export to other machines and you don't have to worry about
absolute paths. 
 
ls() function to list variables. 

Mask function warning when using packages that have namespace conflicts with 
other packages. 

Tidyverse prefers a long format to a wide format (more rows in lieu of more 
columns)

R assignment (ALT - )
Cmd shift M to pipe

Tibbles vs dataframes.
Tibbles do not have row identifiers. Be careful if the row identifier is a 
unique string, etc.

Feather format → binary file format for Python / R data interchange.

Think of tidying data based on the shape of data (wide vs tall)
Spreading vs gathering

Zeallot for unpacking items in a collection similar to python tuple assignment.

library(directlabels) for control over ggplots

Split-Apply-Combine

Travis CI for continuous integration testing. 
