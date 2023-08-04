---
title: Note on deploying with Netlify
author: Matthew Smith
date: '2023-08-04'
slug: []
categories: []
tags:
  - blogdown
type: ''
subtitle: ''
image: ''
---

This is a post for anyone coming back to update their website after some time and encountering issues. In a [previous post](https://matthewsmith.rbind.io/post/creating-r-packages-using-r-markdown-and-blogdown/) I describe how I created the site using [blogdown](https://bookdown.org/yihui/blogdown/).

One issue I encountered when updating my website then using `blogdown::serve_site()` to view it, was the that the blog posts no longer appeared on the main page. After some searching the solution was to simply update Hugo using `blogdown::install_hugo()`. Then to adjust the Hugo versions listed in the *config.toml* file and *theme.toml* file. 

However, there was an issue deploying my website – which uses [Netlify](https://www.netlify.com/) to deploy. The deploy was repeatedly failing. 
With a *“Command failed with exit code 255”* error message. The solution was to update my *netlify.toml* file, using the guidance outlined [here](https://www.apreshill.com/blog/2019-02-spoonful-netlify-toml/), and to move the *netlify.toml* file to my main directory. 

Hope this helps anyone else who is encountering this issue. 
