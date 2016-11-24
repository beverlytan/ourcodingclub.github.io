#R Markdown
*By __John Godlee__ on __November 24, 2016__*

<img src="{{ site.baseurl }}/img/tuthead_rmd.jpg" alt="Img">

##Introduction

###Tutorial Aims:
* Learn what R Markdown is and why it is worth using
* Learn how to construct an R Markdown file
* Export an R Markdown file into many file formats

###Steps:
1. <a href="#download">Download R Markdown </a>
2. <a href="#create">Create an R Markdown (.Rmd) file </a>
3. <a href="#identify">Identify the different parts of a .Rmd file </a>
4. <a href="#insert">Insert code from an R script into a .Rmd file </a>
5. <a href="#reprod">Create a reproducible html record of your code</a>
6. <a href="#pdf">Create a .pdf file from your .Rmd file  </a>
7. <a href="#notebook">R Notebooks (The future of Reproducible code?) </a>

R Markdown allows you to create documents that serve as a neat record of your coding. The documents present your code alongside its output (graphs, tables, etc.) with conventional text to explain it.

R Markdown is useful for:

* Showing off your coding prowess
* Collaborating with other people 
* Creating a coherent log of your analysis, to refer to later on.

To see what R Markdown is capable of, have a look at this ____LINK.pdf____ from an undergraduate dissertation, giving a concise log of their statistical analysis.

R Markdown uses [markdown syntax](http://www.markdowntutorial.com), which provides a simple way of creating documents that can be easily converted to .html, .pdf and many other file types, while being able to include pieces of code and the output of that code alongside normal prose.

In this tutorial we will learn the basics of R Markdown and how to create a reproducible record of your R script.

<a name="download"></a>
##Download R Markdown
To get R Markdown working in R Studio the first thing you need is the rmarkdown package, which you can get from [CRAN](https://cran.r-project.org/web/packages/rmarkdown/index.html):

``` r
install.packages("rmarkdown")
library(rmarkdown)
```
<a name="create"></a>
##Create an R Markdown file
To create a new R Markdown file (.Rmd), select _File -> New File -> R Markdown..._ in R Studio, then choose the file type you want to create. For now we will focus on .html, which can be easily converted to other file types later.

The newly created .Rmd file comes with basic instructions but we want to create our own R Markdown script, so go ahead and delete everything.

Now download our demo R script from ([INSERT_LINK](ourcodingclub.github.io)) and use the instructions below to help you convert this script into a coherent story.
You can also use any other R script you have.
<a name="identify"></a>
##R Markdown Syntax

###The YAML Header
At the top of any R Markdown script is a `YAML` header section enclosed by `` --- ``. By default this includes a title, author, date, and the file type you want to output to. Many other options are available for different functions and formatting. Rules in the header section apply to the whole document.

Insert something like this at the top of your .Rmd script:

```{}
---
title: "Edinburgh Biodiversity"
author: John Doe
date: 22/Oct/2016
output: html_document
---
```
By default, the title, author, date and output format are printed at the top of your .html document. This is the minimum you should put in your header section.

<a name="insert"></a>
###Inserting Code
Code that is included in your .Rmd document should be enclosed by three backwards apostrophes ```` ``` ```` (grave accents!):

````
```{r}
norm <- rnorm(100, mean = 0, sd = 1)
```
````
Inside the curly brackets is a space where you can assign rules for that code chunk. The code chunk above says that the code is R code, we'll get onto some other rules later.

Alternatively, click the little green button with the yellow arrow to put a code chunk wherever your cursor is.


<img src="{{ site.baseurl }}/img/Code_Chunk_Screenshot.jpg" alt="Img">

Have a go at copying and pasting some code from an R script into the .Rmd script using the syntax shown above.

###More on Code Chunks
It's important to remember when you are creating an Rmarkdown file that if you want to run code that refers to an object, for example:

````
```{r}
plot(dataframe)
```
````
you must include instructions showing what `` dataframe `` is, just like in a normal R script. 

````
```{r}
A <- c("a", "a", "b", "b")
B <- c(5, 10, 15, 20)
df <- data.frame(A, B)
plot(df)
```
````

Though if you don't want that code to appear in the final document and just include the graph, then you can include `` echo=FALSE `` in the code chunk instructions. 

````
```{r, echo=FALSE}
A <- c("a", "a", "b", "b")
B <- c(5, 10, 15, 20)
df <- data.frame(A, B)
plot(df)
```
````

Similarly, if you want to perform any actions that require a package like ``dplyr()`` you must load the package in the .Rmd file. To stop an entire code chunk and its output from appearing in the final document, use ``include=FALSE``.

e.g.

````
```{r, include=FALSE}
library(dplyr)
```

```{r, echo=FALSE}
richness <- 
  edidiv %>%
    group_by(taxonGroup) %>%
    summarise(Species_richness = n_distinct(taxonName))
```
````
###More Code Chunk Rules

| Rule | Example (default)| Function |
|:------|:-----:|:-------:|
| eval     | eval=TRUE    | Is the code run and the results included in the output?   |
| echo    | echo=TRUE    | Is the code displayed alongside the results?  |
| warning     | warning=TRUE    | Are warning messages displayed?   |
| error | error=FALSE | Are error messages displayed? |
| message | message=TRUE | Are messages displayed? |
|tidy | tidy=FALSE | Is the code reformatted to make it look "tidy"? |
| results | results="markup" | __How are results treated?__ <br> "hide" = no results <br> "asis" = results without formatting <br> "hold" = results only compiled at end of chunk (use if many commands act on one object)|
| cache | cache=FALSE | Are the results cached for future renders|
| comment | comment="##" | What character are comments prefaced with? | 
|fig.width, fig.height | fig.width=7 | What width/height (in inches) are the plots? | 
| fig.align | fig.align="left" | "left" "right" "center" |

##Inserting Figures
Inserting a graph into R Markdown is easy, the difficult bit is adjusting the formatting.

By default, .Rmd will place graphs by maximising their height while keeping them within the margins of the page and maintaining aspect ratio. If you have a particularly tall figure, this can mean a really huge graph. To manually set the figure dimensions, you can insert a rule:

````
```{r, fig.width=2.5, fig.height=7.5}
ggplot(...)
```
````

By default, figures are rendered as .png files by R Markdown, this can lead to loss of quality when your document is rescaled so you can change that to .svg, a vector file format by adding ``dev='svg'`` to the code chunk rule section.

##Inserting Tables
While R Markdown can print the contents of a data frame easily by enclosing the name of the data frame in a code chunk...

````
```{r}
dataframe
```
````
... this can look a bit messy, especially with data frames with lots of columns. Including a formal table requires more effort.

The most aesthetically pleasing and simple table formatting function I have found is ``kable()`` in the ``knitr`` package. The first argument tells kable to make a table out of the object `dataframe` and that numbers should have two significant figures.

````
```{r}
library(knitr)
kable(dataframe, digits = 2)
```
````




You can also manually create small tables using markdown syntax.

For example: 

```
| Plant | Temp. | Growth |
|:------|:-----:|-------:|
| A     | 20    | 0.65   |
| B     | 20    | 0.95   |
| C     | 20    | 0.15   |
```

Will create something that looks like this:

| Plant | Temp. | Growth |
|:------|:-----:|-------:|
| A     | 20    | 0.65   |
| B     | 20    | 0.95   |
| C     | 20    | 0.15   |

The ``:-----:`` tells markdown that the line above should be treated as a header and the lines below should be treated as the body of the table. Text alignment of the columns is set by the position of ``:``:

|Syntax|Alignment| 
|:------|:-----:|
|``:----:``|Centre|
|``:-----``|Left|
|``-----:``|Right|

##Formatting Text
Markdown syntax can be used to change how text appears in your output file, here are a few common formatting commands:

`*Italic*` = *Italic*

`**Bold**` = **Bold**

This is  \`code` in text

This is `code` in text

`#Header 1`
#Header 1

`##Header 2`
##Header 2

`* Unordered list item`

* Unordered list item

`1. Ordered list item`

1. Ordered list item

`[Link](https://www.google.com)`

[Link](https://www.google.com)

`***` - A horizontal line
***

`$A = \pi \times r^{2}$`

<img src="{{ site.baseurl }}/img/Inline_eq_ex.png" alt="Img">

The `$` symbols tell R markdown to use [LaTeX equation syntax](http://reu.dimacs.rutgers.edu/Symbols.pdf).
 
<a name="reprod"></a>
##Compiling an R Markdown file
Once you have finished creating a beautiful .Rmd script, you can make it into a .html file by selecting _Knit HTML_ in R Studio.

<img src="{{ site.baseurl }}/img/Knit_HTML_Screenshot.jpg" alt="Img">

 Not only does a preview appear in the _Viewer_ window in R Studio but it also saves a .html file to your working directory. As ever, set your working directory location using:

```
setwd("~/...")
```
or find out what your working directory is using:

```
getwd()
``` 

<a name="pdf"></a>
##Creating pdf files in Rmarkdown


To create .pdf documents for printing in A4 requires a bit more fiddling around. R Studio uses another document compiling system called [LaTeX] (http://www.latex-project.org).

If you are using windows go to <https://miktex.org/download> and download the appropriate "Complete MikTeX Installer" for your system, either 32-bit or 64-bit.

If you are using a mac go to <https://tug.org/mactex/mactex-download.html> and download the "MacTeX.pkg".

This will install a version of LaTeX onto your system which R will then be able to call to compile the .pdf.

Becoming familiar with LaTeX syntax will give you a lot more options to make your R markdown .pdf look pretty, as LaTeX commands are mostly compatible with R markdown, though some googling is often required.

<a name="notebook"></a>
##R Markdown Notebooks
R Markdown requires you to output to a non-interactive file format like .html or .pdf. When presenting your code, this means you have to make a choice, do you want interactive but messy looking code (.Rmd) or non-interactive but neat looking code (.html, .pdf)?

R notebooks provide a file format that combine the interactivity of a .Rmd file with the attractiveness of .html output.

R notebooks output to the imaginatively named .nb.html format.

.nb.html files can be loaded into a web browser to see the output, or loaded into a code editor like R Studio to see the code.

Notebooks use the same syntax as .Rmd files so it is easy to copy and paste the script from a .Rmd into a Notebook. To create a new R Notebook file (.Rmd), select _File -> New File -> R Notebook_. Try and create a notebook from your newly created .Rmd file by copying and pasting the script.

To output to .nb.html first make sure all your code chunks have been run:

<img src="{{ site.baseurl }}/img/Notebook_run.jpg" alt="Img">

then click _Preview_:

<img src="{{ site.baseurl }}/img/Notebook_preview.jpg" alt="Img">

Notice that with R Notebooks you can still output to .html or .pdf, the same as a .Rmd file.

R notebooks have only been around for about a year so they're not perfect yet, but may replace R markdown in the future for many applications.

__Note: R Notebooks are only available in R Studio 1.0 or higher.__


