---
title: Creating R packages, R markdown and blogdown
author: Matthew Smith
date: '2018-04-09'
categories:
  - R
  - R markdown
tags:
  - blogdown
  - CRAN
  - GitHub
  - R Markdown
  - R Package
slug: creating-r-packages-using-r-markdown-and-blogdown
---



As this my very first blog post for this site (created using blogdown) I decided to write some comments/general points on my experience moving from a being a general R user making use of functions to writing R packages, using GitHub, and making use of markdown and blogdown. 

# Packages
Throughout my PhD I had to create networks from international trade data. This involved cleaning the data, removing unnecessary actors (such as territories like “Other Asia not elsewhere classified”), applying a threshold, so the network only contained ties that were some percentage of world trade and finally convert this into a network file. I did this using excel (as at the time I was not fully familiar with R) and found it a very repetitive/laborious task. As I become more familiar with R – I did this in R and wrote a function I could use on any future data. The advantages of using an R function were instantly obvious, the process of data cleaning now took moments. 

I then had a number of functions related to cleaning and analysing trade data scattered across numerous .R files. Therefore, I decided to take the next step and collect all these functions into an R package. It took some time – but it was completely worth it, as creating a package has a number of benefits:  

* All related functions are in one place, and can be easily shared with colleagues. It is much easier to share a package than a set of .R files.  

*  Writing a package provides the opportunity (and in some cases forces you) to improve your code so that it is more streamlined.  

*  Help documentation and examples. Having to write help documentations and examples is a very useful task, as you do not end up coming back to functions where there is little or no explanation of what they do. Furthermore, you can search for the help and guidance on these functions within R in the future.

The most useful guides on how to write an R package are:  

*  http://kbroman.org/pkg_primer/  

*  http://r-pkgs.had.co.nz/

They provide information on how to set up an R package, write documentation, along with how to write vignettes guides to accompany your package.

There are number of very useful packages to use when creating an R package including `devtools`,`usethis` and `roxygen2`.

Another point to make before writing an R package is about using `%>%` (pipes) in R package functions. Unless you set up pipes with `usethis`, more specially the `use_pipe()` function, avoid using pipes.

## GitHub
When writing a package – you can upload this as a repository on GitHub. If, like me, you are not completely comfortable using the command line, then GitHub Desktop (or equivalents such as GitKracken), is an essential tool. 

I also realised that when uploading a package on GitHub a detailed readme is essential to guide user on what exactly the package does and how to properly implement various functions. 

I have created a number of R packages as repositories on GitHub. 

## CRAN
Although you can upload R packages onto GitHub, where others can download and install these packages using `devtools`, packages can also be uploaded to CRAN. I have uploaded one package onto CRAN: `ITNr`. 

I was somewhat apprehensive about submitting a package to CRAN – as the documentation and online guidance often paints this as a daunting task. I did not necessarily find this the case - that as long as you follow the package writing guidance- you should not be discouraged from uploading it to CRAN.

However, uploading a package to CRAN and maintaining is a more time consuming process than simply uploading a package to GitHub, where changes can be made easily and quickly. So there is a need to weigh the benefits of maintaining the CRAN submission with having it on GitHub. 

If you are submitting a package to CRAN, some advice based on the feedback I received includes:  

* Ensure that you thoroughly check your help documentation (make sure there are no typos and that it properly matches the function code).  

* In your R description file, in the Description section, do not start this with “This package”  

* Unwrap examples. I wrote all example with \dontrun{}, and received the following feedback:  

>Your examples are wrapped in \dontrun{}, hence nothing gets tested. Please unwrap the examples if that is feasible and if they can be executed in < 5 sec for each Rd file or create additionally small toy examples”  

Here is an demonstration of an unwrapped example with toy example (taken from `ITNr`):

```r
#' @examples
#' require(igraph)
#' ##Create random International Trade Network (igraph object)
#' ITN<-erdos.renyi.game(75,0.05,directed = TRUE)
#'
#' ##Add edge weights
#' E(ITN)$weight<-runif(ecount(ITN), 0, 1)
#'
#' ##Add vertex names
#' V(ITN)$name<-1:vcount(ITN)
#'
#' ##Calculate the centrality measures
#' ITNCENT<-ITNcentrality(ITN)
#'
```

# R markdown
I started using R markdown to write detailed readme/vignette files to accompany R packages on GitHub. R markdown is great and an essential tool for writing guides to using R, and can even be used to create [presentations](https://rmarkdown.rstudio.com/lesson-11.html). There are a number of resources to help writing R markdown – among them the R markdown cheat sheet and an introduction to r markdown provided by [RStudio](https://rmarkdown.rstudio.com/) 

I would also highly recommend using [RPubs](https://rpubs.com/) to share R markdown documents – either guides for more public use or to share these documents with colleagues (that include R code and output).  

Only issue is that you cannot edit these once uploaded (to my knowledge). So if you notice a mistake in your RPub and want to change it – you would have to delete the RPub, and upload a new one under the same name. In my early use of R markdown, one very simple error I made, was that I forgot to run a spell check (due to familiarity with word/writer), and subsequently had to correct these mistakes in RPubs. 

I have been talking about R markdown, but stand alone markdown has a number of great capabilities, such as [academic writing and technical documents](http://blog.kdheepak.com/writing-papers-with-markdown.html). R markdown is simply markdown that allows you to bring together text, R code and R outputs in a single elegant document. 

Whilst you can write R markdown in RStudio, there a number of editors dedicated just to writing markdown, an overview of these is provided [here](https://www.slant.co/topics/1852/~markdown-editors-for-windows) 

# Blogdown
If you choose to make a site using blogdown it is essential you follow the guidance and instruction provided in: https://bookdown.org/yihui/blogdown/. This guide provide all the details you need to produce a blog using blogdown. 

I am using the beautifulhugo theme – which allows an icon. In order to create the icon from a .png file I made use of imagemagick and the magick R package. I installed imagemagick and then ran the following code:


```r
library(magick)
library(magrittr)

image_read("File.png")%>%
  image_scale("32x32") %>%
  image_write("File.ico",format="ico")
```

This produced a .ico file I used when making the site.  

Another useful resource for using blogdown can be found [here](https://alison.rbind.io/post/up-and-running-with-blogdown/)

