---
title: Country Networks and Flags
author: Matthew Smith
date: '2018-05-19'
slug: country-networks-and-flags
categories:
  - R
tags:
  - R
  - Country Networks
  - Visualisation
subtitle: ''
---

Recently, I was asked whether I could create an international trade network with flags as nodes. Therefore, I thought I would write a post introducing the `ggflags` packages and how to use it in network visualisation.

I am creating this visualisation in R, and relying on a number of packages:

- `ITNr` for the international trade network data that we use in the example. However, you the visualisation steps I will outline can be applied to any country level network (that is a set of countries connect by any links - this could be trade, investment etc.)  

- `ggflags` to visualise the nodes as flags. This package is available on GitHub (currently not on CRAN)  

- `ggnetwork` is used to plot the network

- `countrycode` and `magrittr` to process node names into the correct form to be used by `ggflags`

- I also use `igraph` and `intergraph` as the example network from `ITNr` is a `igraph` object

*Update: An example of a published article using these flag network visualisations can be found [here](https://onlinelibrary.wiley.com/doi/abs/10.1111/twec.13006)*

# Load, clean and process network data
First we load the example data from the `ITNr`, an example International Trade Network (ITN). We also calculate network centrality metrics, as these can be used later in the visualisation - such as setting node size based on centrality. 


```r
library(ITNr)

data("ELEnet16")

## Centrality Measures
ITNcent<-ITNcentrality(ELEnet16)

## Add these as attributes to the igraph object
library(igraph)
V(ELEnet16)$cent<-ITNcent$Weighted.Out.Degree
V(ELEnet16)$centALL<-ITNcent$Weighted.Degree.All
```

We then need to convert this into an object compatible with `ggnetwork` so that we can take advantage of the `ggflags`functionality. 


```r
library(ggnetwork)
library(intergraph)

## Convert igraph object to network object
ITNnet<-asNetwork(ELEnet16)

##Convert this to ggnetwork dataframe
ITN<-ggnetwork(ITNnet)
```

Before we can visualise this data, we need to do some additional cleaning, such as making use the `ggflags` package can recognise the country names. `ggflags` needs the country names to be in lowercase and in the form of 2 digit ISO codes - here is where the `countrycodes` packages is very useful. 


```r
library(countrycode)
# Create new column called country
# Need to make sure the vertex names are characters,
# as ggnetwork may have converted them to factors.

ITN$country<-countrycode(as.character(ITN$vertex.names),
                         "iso3c","iso2c")

# In this example - the vertex anmes are 3 digit ISO codes
# However, countrycode has a wide range of function to 
# convert country names to 2 sigit iso codes

# We then need to make the codes lowercase
library(magrittr)
ITN$country%<>%tolower()
```


# Network Visualisation
THe data is now in a form, where we can plot the network and make use of the `ggflags` package. In this example node size is based on weighted out degree centrality and tie size is based on the level of trade in the network (the edge weight in the `igraph` object)


```r
#First install ggflags
devtools::install_github("rensa/ggflags")
library(ggflags)
# I would recommend opening a tiff file, so that you get a higher quality visualisation
tiff('Example.tiff', units="in", width=6, height=5, res=300)

ggplot(ITN, aes(x = x, y = y, xend = xend, yend = yend,
                country=country,size=cent)) +
  geom_edges(size = E(ELEnet16)$weight*.5,#the edge weight from the igraph object
             color="grey") +
  geom_flag() + 
  scale_size(range = c(2, 13))+ #Node size scale
  guides(colour=FALSE,size=FALSE)+ #Remove the legends
  theme_blank()

dev.off()
```


```
## Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
## ℹ Please use `linewidth` instead.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

```
## Warning: The `<scale>` argument of `guides()` cannot be `FALSE`. Use "none" instead as
## of ggplot2 3.3.4.
## This warning is displayed once every 8 hours.
## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
## generated.
```

<img src="/post/2018-05-19-country-networks-and-flags_files/figure-html/full-1.png" width="672" />

