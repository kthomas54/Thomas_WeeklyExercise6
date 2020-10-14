---
title: 'Weekly Exercises #6'
author: "Kalvin Thomas"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(googlesheets4) # for reading googlesheet data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(gifski)        # for creating the gif (don't need to load this library every time,but need it installed)
library(transformr)    # for "tweening" (gganimate)
library(shiny)         # for creating interactive apps
library(patchwork)     # for nicely combining ggplot2 graphs  
library(gt)            # for creating nice tables
library(rvest)         # for scraping data
library(robotstxt)     # for checking if you can scrape data
library(COVID19)
gs4_deauth()           # To not have to authorize each time you knit.
theme_set(theme_minimal())
```


```r
# Lisa's garden data 
garden_harvest <- read_sheet("https://docs.google.com/spreadsheets/d/1DekSazCzKqPS2jnGhKue7tLxRU3GVL1oxi-4bEM5IWw/edit?usp=sharing") %>% 
  mutate(date = ymd(date))

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Your first `shiny` app 

  1. This app will also use the COVID data. Make sure you load that data and all the libraries you need in the `app.R` file you create. Below, you will post a link to the app that you publish on shinyapps.io. You will create an app to compare states' cumulative number of COVID cases over time. The x-axis will be number of days since 20+ cases and the y-axis will be cumulative cases on the log scale (`scale_y_log10()`). We use number of days since 20+ cases on the x-axis so we can make better comparisons of the curve trajectories. You will have an input box where the user can choose which states to compare (`selectInput()`) and have a submit button to click once the user has chosen all states they're interested in comparing. The graph should display a different line for each state, with labels either on the graph or in a legend. Color can be used if needed. 
  
  -[covid_app link here](https://kthomas54.shinyapps.io/covid_app/)
  
## Warm-up exercises from tutorial

  2. Read in the fake garden harvest data. Find the data [here](https://github.com/llendway/scraping_etc/blob/main/2020_harvest.csv) and click on the `Raw` button to get a direct link to the data. 
  

```r
X2020_harvest <- read_csv("https://raw.githubusercontent.com/llendway/scraping_etc/main/2020_harvest.csv", 
    col_types = cols(X1 = col_skip(), 
                     weight = col_number()), 
    na = "MISSING", 
    skip = 2)
X2020_harvest
```

<div data-pagedtable="false">
  <script data-pagedtable-source type="application/json">
{"columns":[{"label":["vegetable"],"name":[1],"type":["chr"],"align":["left"]},{"label":["variety"],"name":[2],"type":["chr"],"align":["left"]},{"label":["date"],"name":[3],"type":["chr"],"align":["left"]},{"label":["weight"],"name":[4],"type":["dbl"],"align":["right"]},{"label":["units"],"name":[5],"type":["chr"],"align":["left"]}],"data":[{"1":"lettuce","2":"reseed","3":"6/6/20","4":"20","5":"grams"},{"1":"radish","2":"Garden Party Mix","3":"6/6/20","4":"36","5":"grams"},{"1":"lettuce","2":"reseed","3":"6/8/20","4":"15","5":"grams"},{"1":"lettuce","2":"reseed","3":"6/9/20","4":"10","5":"grams"},{"1":"radish","2":"Garden Party Mix","3":"6/11/20","4":"67","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"6/11/20","4":"12","5":"grams"},{"1":"spinach","2":"Catalina","3":"6/11/20","4":"9","5":"grams"},{"1":"beets","2":"leaves","3":"6/11/20","4":"8","5":"grams"},{"1":"radish","2":"Garden Party Mix","3":"6/13/20","4":"53","5":"grams"},{"1":"lettuce","2":"NA","3":"6/13/20","4":"19","5":"grams"},{"1":"spinach","2":"Catalina","3":"6/13/20","4":"14","5":"grams"},{"1":"kale","2":"Heirloom Lacinto","3":"6/13/20","4":"10","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"6/17/20","4":"NA","5":"grams"},{"1":"spinach","2":"Catalina","3":"6/17/20","4":"58","5":"grams"},{"1":"peas","2":"Magnolia Blossom","3":"6/17/20","4":"8","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"6/17/20","4":"121","5":"grams"},{"1":"chives","2":"perrenial","3":"6/17/20","4":"8","5":"grams"},{"1":"strawberries","2":"perrenial","3":"6/18/20","4":"40","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"6/18/20","4":"47","5":"grams"},{"1":"spinach","2":"NA","3":"6/18/20","4":"59","5":"grams"},{"1":"beets","2":"leaves","3":"6/18/20","4":"25","5":"grams"},{"1":"spinach","2":"Catalina","3":"6/19/20","4":"58","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"6/19/20","4":"39","5":"grams"},{"1":"beets","2":"leaves","3":"6/19/20","4":"11","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"6/19/20","4":"NA","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"6/20/20","4":"22","5":"grams"},{"1":"spinach","2":"Catalina","3":"6/20/20","4":"25","5":"grams"},{"1":"lettuce","2":"Tatsoi","3":"6/20/20","4":"18","5":"grams"},{"1":"radish","2":"Garden Party Mix","3":"6/20/20","4":"16","5":"grams"},{"1":"peas","2":"Magnolia Blossom","3":"6/20/20","4":"71","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"6/20/20","4":"148","5":"grams"},{"1":"asparagus","2":"asparagus","3":"6/20/20","4":"20","5":"grams"},{"1":"radish","2":"Garden Party Mix","3":"6/21/20","4":"37","5":"grams"},{"1":"Swiss chard","2":"Neon Glow","3":"6/21/20","4":"19","5":"grams"},{"1":"spinach","2":"Catalina","3":"6/21/20","4":"71","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"6/21/20","4":"95","5":"grams"},{"1":"spinach","2":"Catalina","3":"6/21/20","4":"51","5":"grams"},{"1":"Swiss chard","2":"Neon Glow","3":"6/21/20","4":"13","5":"grams"},{"1":"beets","2":"leaves","3":"6/21/20","4":"57","5":"grams"},{"1":"kale","2":"Heirloom Lacinto","3":"6/21/20","4":"60","5":"grams"},{"1":"spinach","2":"Catalina","3":"6/22/20","4":"37","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"6/22/20","4":"52","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"6/22/20","4":"40","5":"grams"},{"1":"peas","2":"Magnolia Blossom","3":"6/22/20","4":"19","5":"grams"},{"1":"strawberries","2":"perrenial","3":"6/22/20","4":"19","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"6/22/20","4":"18","5":"grams"},{"1":"peas","2":"Magnolia Blossom","3":"6/23/20","4":"40","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"6/23/20","4":"165","5":"grams"},{"1":"spinach","2":"Catalina","3":"6/23/20","4":"41","5":"grams"},{"1":"cilantro","2":"cilantro","3":"6/23/20","4":"2","5":"grams"},{"1":"basil","2":"Isle of Naxos","3":"6/23/20","4":"5","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"6/24/20","4":"34","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"6/24/20","4":"122","5":"grams"},{"1":"spinach","2":"Catalina","3":"6/25/20","4":"22","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"6/25/20","4":"30","5":"grams"},{"1":"strawberries","2":"perrenial","3":"6/26/20","4":"17","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"6/26/20","4":"425","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"6/27/20","4":"52","5":"grams"},{"1":"lettuce","2":"Tatsoi","3":"6/27/20","4":"89","5":"grams"},{"1":"spinach","2":"NA","3":"6/27/20","4":"60","5":"grams"},{"1":"peas","2":"Magnolia Blossom","3":"6/27/20","4":"333","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"6/28/20","4":"793","5":"grams"},{"1":"spinach","2":"Catalina","3":"6/28/20","4":"99","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"6/28/20","4":"111","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"6/29/20","4":"58","5":"grams"},{"1":"lettuce","2":"mustard greens","3":"6/29/20","4":"23","5":"grams"},{"1":"peas","2":"Magnolia Blossom","3":"6/29/20","4":"625","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"6/29/20","4":"561","5":"grams"},{"1":"raspberries","2":"perrenial","3":"6/29/20","4":"30","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"6/29/20","4":"82","5":"grams"},{"1":"Swiss chard","2":"Neon Glow","3":"6/30/20","4":"32","5":"grams"},{"1":"spinach","2":"Catalina","3":"6/30/20","4":"80","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"7/1/20","4":"60","5":"grams"},{"1":"lettuce","2":"Tatsoi","3":"7/2/20","4":"144","5":"grams"},{"1":"spinach","2":"Catalina","3":"7/2/20","4":"16","5":"grams"},{"1":"peas","2":"Magnolia Blossom","3":"7/2/20","4":"798","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"7/2/20","4":"743","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"7/3/20","4":"217","5":"grams"},{"1":"lettuce","2":"Tatsoi","3":"7/3/20","4":"216","5":"grams"},{"1":"radish","2":"Garden Party Mix","3":"7/3/20","4":"88","5":"grams"},{"1":"basil","2":"Isle of Naxos","3":"7/3/20","4":"9","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"7/4/20","4":"285","5":"grams"},{"1":"peas","2":"Magnolia Blossom","3":"7/4/20","4":"457","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"7/4/20","4":"147","5":"grams"},{"1":"basil","2":"Isle of Naxos","3":"7/6/20","4":"17","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"7/6/20","4":"175","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"7/6/20","4":"NA","5":"grams"},{"1":"lettuce","2":"Tatsoi","3":"7/6/20","4":"NA","5":"grams"},{"1":"peas","2":"Magnolia Blossom","3":"7/6/20","4":"433","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"7/6/20","4":"48","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"7/7/20","4":"67","5":"grams"},{"1":"beets","2":"Gourmet Golden","3":"7/7/20","4":"62","5":"grams"},{"1":"beets","2":"Sweet Merlin","3":"7/7/20","4":"10","5":"grams"},{"1":"radish","2":"Garden Party Mix","3":"7/7/20","4":"43","5":"grams"},{"1":"basil","2":"Isle of Naxos","3":"7/7/20","4":"11","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"7/7/20","4":"13","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"7/8/20","4":"75","5":"grams"},{"1":"peas","2":"Magnolia Blossom","3":"7/8/20","4":"252","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"7/8/20","4":"178","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"7/8/20","4":"39","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/8/20","4":"181","5":"grams"},{"1":"beets","2":"Gourmet Golden","3":"7/8/20","4":"83","5":"grams"},{"1":"Swiss chard","2":"Neon Glow","3":"7/8/20","4":"96","5":"grams"},{"1":"lettuce","2":"Tatsoi","3":"7/8/20","4":"75","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"7/9/20","4":"61","5":"grams"},{"1":"raspberries","2":"perrenial","3":"7/9/20","4":"131","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"7/9/20","4":"140","5":"grams"},{"1":"beets","2":"Sweet Merlin","3":"7/9/20","4":"69","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/9/20","4":"78","5":"grams"},{"1":"raspberries","2":"perrenial","3":"7/10/20","4":"61","5":"grams"},{"1":"basil","2":"Isle of Naxos","3":"7/10/20","4":"150","5":"grams"},{"1":"raspberries","2":"perrenial","3":"7/11/20","4":"60","5":"grams"},{"1":"strawberries","2":"perrenial","3":"7/11/20","4":"77","5":"grams"},{"1":"spinach","2":"Catalina","3":"7/11/20","4":"19","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"7/11/20","4":"79","5":"grams"},{"1":"raspberries","2":"perrenial","3":"7/11/20","4":"105","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"7/11/20","4":"701","5":"grams"},{"1":"tomatoes","2":"grape","3":"7/11/20","4":"24","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/12/20","4":"130","5":"grams"},{"1":"beets","2":"Sweet Merlin","3":"7/12/20","4":"89","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"7/12/20","4":"492","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"7/12/20","4":"83","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/13/20","4":"47","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"7/13/20","4":"145","5":"grams"},{"1":"radish","2":"Garden Party Mix","3":"7/13/20","4":"50","5":"grams"},{"1":"strawberries","2":"perrenial","3":"7/13/20","4":"85","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"7/13/20","4":"53","5":"grams"},{"1":"lettuce","2":"Tatsoi","3":"7/13/20","4":"137","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"7/13/20","4":"40","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"7/13/20","4":"443","5":"grams"},{"1":"kale","2":"Heirloom Lacinto","3":"7/14/20","4":"128","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/14/20","4":"152","5":"grams"},{"1":"peas","2":"Magnolia Blossom","3":"7/14/20","4":"207","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"7/14/20","4":"526","5":"grams"},{"1":"raspberries","2":"perrenial","3":"7/14/20","4":"152","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"7/15/20","4":"393","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"7/15/20","4":"743","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/15/20","4":"1057","5":"grams"},{"1":"spinach","2":"Catalina","3":"7/15/20","4":"39","5":"grams"},{"1":"Swiss chard","2":"Neon Glow","3":"7/16/20","4":"29","5":"grams"},{"1":"lettuce","2":"Farmer's Market Blend","3":"7/16/20","4":"61","5":"grams"},{"1":"onions","2":"Delicious Duo","3":"7/16/20","4":"50","5":"grams"},{"1":"strawberries","2":"perrenial","3":"7/17/20","4":"88","5":"grams"},{"1":"cilantro","2":"cilantro","3":"7/17/20","4":"33","5":"grams"},{"1":"basil","2":"Isle of Naxos","3":"7/17/20","4":"16","5":"grams"},{"1":"jalapeño","2":"giant","3":"7/17/20","4":"20","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/17/20","4":"347","5":"grams"},{"1":"raspberries","2":"perrenial","3":"7/18/20","4":"77","5":"grams"},{"1":"beets","2":"Sweet Merlin","3":"7/18/20","4":"172","5":"grams"},{"1":"kale","2":"Heirloom Lacinto","3":"7/18/20","4":"61","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"7/18/20","4":"81","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/18/20","4":"294","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"7/18/20","4":"660","5":"grams"},{"1":"kale","2":"Heirloom Lacinto","3":"7/19/20","4":"113","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/19/20","4":"531","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"7/19/20","4":"344","5":"grams"},{"1":"strawberries","2":"perrenial","3":"7/19/20","4":"37","5":"grams"},{"1":"peas","2":"Magnolia Blossom","3":"7/19/20","4":"140","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"7/20/20","4":"134","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/20/20","4":"179","5":"grams"},{"1":"peas","2":"Super Sugar Snap","3":"7/20/20","4":"336","5":"grams"},{"1":"beets","2":"Gourmet Golden","3":"7/20/20","4":"107","5":"grams"},{"1":"kale","2":"Heirloom Lacinto","3":"7/20/20","4":"128","5":"grams"},{"1":"hot peppers","2":"thai","3":"7/20/20","4":"12","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"7/20/20","4":"519","5":"grams"},{"1":"hot peppers","2":"variety","3":"7/20/20","4":"559","5":"grams"},{"1":"jalapeño","2":"giant","3":"7/20/20","4":"197","5":"grams"},{"1":"lettuce","2":"Tatsoi","3":"7/20/20","4":"123","5":"grams"},{"1":"Swiss chard","2":"Neon Glow","3":"7/20/20","4":"178","5":"grams"},{"1":"onions","2":"Long Keeping Rainbow","3":"7/20/20","4":"102","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"7/21/20","4":"110","5":"grams"},{"1":"tomatoes","2":"grape","3":"7/21/20","4":"86","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"7/21/20","4":"137","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"7/21/20","4":"339","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"7/21/20","4":"21","5":"grams"},{"1":"spinach","2":"Catalina","3":"7/21/20","4":"21","5":"grams"},{"1":"basil","2":"Isle of Naxos","3":"7/21/20","4":"7","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"7/22/20","4":"76","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"7/22/20","4":"351","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/22/20","4":"655","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"7/22/20","4":"23","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"7/23/20","4":"129","5":"grams"},{"1":"carrots","2":"King Midas","3":"7/23/20","4":"56","5":"grams"},{"1":"Swiss chard","2":"Neon Glow","3":"7/23/20","4":"466","5":"grams"},{"1":"onions","2":"Long Keeping Rainbow","3":"7/23/20","4":"91","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"7/23/20","4":"130","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/24/20","4":"525","5":"grams"},{"1":"tomatoes","2":"grape","3":"7/24/20","4":"31","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"7/24/20","4":"140","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"7/24/20","4":"247","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"7/24/20","4":"220","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"7/24/20","4":"1321","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"7/24/20","4":"100","5":"grams"},{"1":"raspberries","2":"perrenial","3":"7/24/20","4":"32","5":"grams"},{"1":"strawberries","2":"perrenial","3":"7/24/20","4":"93","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"7/24/20","4":"16","5":"grams"},{"1":"basil","2":"Isle of Naxos","3":"7/24/20","4":"3","5":"grams"},{"1":"peppers","2":"variety","3":"7/24/20","4":"68","5":"grams"},{"1":"carrots","2":"King Midas","3":"7/24/20","4":"178","5":"grams"},{"1":"carrots","2":"Dragon","3":"7/24/20","4":"80","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"7/25/20","4":"463","5":"grams"},{"1":"tomatoes","2":"grape","3":"7/25/20","4":"106","5":"grams"},{"1":"kale","2":"Heirloom Lacinto","3":"7/25/20","4":"121","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/25/20","4":"901","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"7/26/20","4":"81","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"7/26/20","4":"148","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"7/27/20","4":"1542","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"7/27/20","4":"728","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/27/20","4":"785","5":"grams"},{"1":"strawberries","2":"perrenial","3":"7/27/20","4":"113","5":"grams"},{"1":"raspberries","2":"perrenial","3":"7/27/20","4":"29","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"7/27/20","4":"801","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"7/27/20","4":"99","5":"grams"},{"1":"beets","2":"Sweet Merlin","3":"7/27/20","4":"49","5":"grams"},{"1":"beets","2":"Gourmet Golden","3":"7/27/20","4":"149","5":"grams"},{"1":"radish","2":"Garden Party Mix","3":"7/27/20","4":"39","5":"grams"},{"1":"carrots","2":"King Midas","3":"7/27/20","4":"174","5":"grams"},{"1":"onions","2":"Long Keeping Rainbow","3":"7/27/20","4":"129","5":"grams"},{"1":"broccoli","2":"Yod Fah","3":"7/27/20","4":"372","5":"grams"},{"1":"carrots","2":"King Midas","3":"7/28/20","4":"160","5":"grams"},{"1":"tomatoes","2":"Old German","3":"7/28/20","4":"611","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"7/28/20","4":"203","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"7/28/20","4":"312","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"7/28/20","4":"315","5":"grams"},{"1":"tomatoes","2":"grape","3":"7/28/20","4":"131","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"7/28/20","4":"91","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/28/20","4":"76","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"7/29/20","4":"153","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"7/29/20","4":"442","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"7/29/20","4":"240","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"7/29/20","4":"209","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"7/29/20","4":"73","5":"grams"},{"1":"tomatoes","2":"grape","3":"7/29/20","4":"40","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"7/29/20","4":"457","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/29/20","4":"514","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"7/29/20","4":"305","5":"grams"},{"1":"kale","2":"Heirloom Lacinto","3":"7/29/20","4":"280","5":"grams"},{"1":"tomatoes","2":"grape","3":"7/30/20","4":"91","5":"grams"},{"1":"beets","2":"Sweet Merlin","3":"7/30/20","4":"101","5":"grams"},{"1":"onions","2":"Long Keeping Rainbow","3":"7/30/20","4":"19","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"7/30/20","4":"94","5":"grams"},{"1":"carrots","2":"Bolero","3":"7/30/20","4":"116","5":"grams"},{"1":"carrots","2":"King Midas","3":"7/30/20","4":"107","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/30/20","4":"626","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"7/31/20","4":"307","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"7/31/20","4":"197","5":"grams"},{"1":"tomatoes","2":"Old German","3":"7/31/20","4":"633","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"7/31/20","4":"290","5":"grams"},{"1":"tomatoes","2":"grape","3":"7/31/20","4":"100","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"7/31/20","4":"1215","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"7/31/20","4":"592","5":"grams"},{"1":"strawberries","2":"perrenial","3":"7/31/20","4":"23","5":"grams"},{"1":"spinach","2":"Catalina","3":"7/31/20","4":"31","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"7/31/20","4":"107","5":"grams"},{"1":"cucumbers","2":"pickling","3":"7/31/20","4":"174","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/1/20","4":"435","5":"grams"},{"1":"tomatoes","2":"Brandywine","3":"8/1/20","4":"320","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"8/1/20","4":"619","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"8/1/20","4":"97","5":"grams"},{"1":"tomatoes","2":"Black Krim","3":"8/1/20","4":"436","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/1/20","4":"168","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/1/20","4":"164","5":"grams"},{"1":"cucumbers","2":"pickling","3":"8/1/20","4":"1130","5":"grams"},{"1":"basil","2":"Isle of Naxos","3":"8/1/20","4":"137","5":"grams"},{"1":"jalapeño","2":"giant","3":"8/1/20","4":"74","5":"grams"},{"1":"cilantro","2":"cilantro","3":"8/1/20","4":"17","5":"grams"},{"1":"onions","2":"Delicious Duo","3":"8/1/20","4":"182","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/2/20","4":"1175","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"8/2/20","4":"509","5":"grams"},{"1":"tomatoes","2":"Black Krim","3":"8/2/20","4":"857","5":"grams"},{"1":"tomatoes","2":"Old German","3":"8/2/20","4":"336","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/2/20","4":"156","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"8/2/20","4":"211","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/2/20","4":"102","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"8/3/20","4":"308","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/3/20","4":"252","5":"grams"},{"1":"cucumbers","2":"pickling","3":"8/3/20","4":"1155","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"8/3/20","4":"572","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"8/3/20","4":"65","5":"grams"},{"1":"kale","2":"Heirloom Lacinto","3":"8/3/20","4":"383","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/4/20","4":"387","5":"grams"},{"1":"tomatoes","2":"Brandywine","3":"8/4/20","4":"231","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"8/4/20","4":"73","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"8/4/20","4":"339","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/4/20","4":"118","5":"grams"},{"1":"peppers","2":"variety","3":"8/4/20","4":"270","5":"grams"},{"1":"jalapeño","2":"giant","3":"8/4/20","4":"162","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"8/4/20","4":"56","5":"grams"},{"1":"peppers","2":"variety","3":"8/4/20","4":"192","5":"grams"},{"1":"onions","2":"Long Keeping Rainbow","3":"8/4/20","4":"195","5":"grams"},{"1":"peppers","2":"green","3":"8/4/20","4":"81","5":"grams"},{"1":"jalapeño","2":"giant","3":"8/4/20","4":"87","5":"grams"},{"1":"hot peppers","2":"thai","3":"8/4/20","4":"24","5":"grams"},{"1":"hot peppers","2":"variety","3":"8/4/20","4":"40","5":"grams"},{"1":"spinach","2":"Catalina","3":"8/4/20","4":"44","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/4/20","4":"427","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/5/20","4":"563","5":"grams"},{"1":"tomatoes","2":"Brandywine","3":"8/5/20","4":"290","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"8/5/20","4":"781","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"8/5/20","4":"223","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"8/5/20","4":"382","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/5/20","4":"217","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"8/5/20","4":"67","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"8/5/20","4":"41","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"8/5/20","4":"234","5":"grams"},{"1":"tomatoes","2":"Black Krim","3":"8/6/20","4":"393","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"8/6/20","4":"307","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"8/6/20","4":"175","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"8/6/20","4":"303","5":"grams"},{"1":"cucumbers","2":"pickling","3":"8/6/20","4":"127","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"8/6/20","4":"98","5":"grams"},{"1":"carrots","2":"Bolero","3":"8/6/20","4":"164","5":"grams"},{"1":"carrots","2":"Dragon","3":"8/6/20","4":"442","5":"grams"},{"1":"potatoes","2":"purple","3":"8/6/20","4":"317","5":"grams"},{"1":"potatoes","2":"yellow","3":"8/6/20","4":"439","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/7/20","4":"359","5":"grams"},{"1":"tomatoes","2":"Brandywine","3":"8/7/20","4":"356","5":"grams"},{"1":"tomatoes","2":"Old German","3":"8/7/20","4":"233","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"8/7/20","4":"364","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"8/7/20","4":"1045","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"8/7/20","4":"562","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/7/20","4":"292","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/7/20","4":"1219","5":"grams"},{"1":"cucumbers","2":"pickling","3":"8/7/20","4":"1327","5":"grams"},{"1":"carrots","2":"Bolero","3":"8/7/20","4":"255","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"8/7/20","4":"19","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"8/8/20","4":"162","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/8/20","4":"81","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/8/20","4":"564","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"8/8/20","4":"184","5":"grams"},{"1":"beans","2":"Chinese Red Noodle","3":"8/8/20","4":"108","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"8/8/20","4":"122","5":"grams"},{"1":"cucumbers","2":"pickling","3":"8/8/20","4":"1697","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"8/8/20","4":"545","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/8/20","4":"445","5":"grams"},{"1":"kale","2":"Heirloom Lacinto","3":"8/8/20","4":"305","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/9/20","4":"179","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"8/9/20","4":"591","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"8/9/20","4":"1102","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"8/9/20","4":"308","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"8/9/20","4":"54","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/9/20","4":"64","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/9/20","4":"443","5":"grams"},{"1":"onions","2":"Long Keeping Rainbow","3":"8/9/20","4":"118","5":"grams"},{"1":"Swiss chard","2":"Neon Glow","3":"8/9/20","4":"302","5":"grams"},{"1":"basil","2":"Isle of Naxos","3":"8/10/20","4":"13","5":"grams"},{"1":"potatoes","2":"yellow","3":"8/10/20","4":"272","5":"grams"},{"1":"potatoes","2":"purple","3":"8/10/20","4":"168","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"8/10/20","4":"216","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"8/10/20","4":"241","5":"grams"},{"1":"Swiss chard","2":"Neon Glow","3":"8/10/20","4":"309","5":"grams"},{"1":"carrots","2":"Bolero","3":"8/10/20","4":"221","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/11/20","4":"731","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/11/20","4":"302","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/11/20","4":"307","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"8/11/20","4":"160","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"8/11/20","4":"755","5":"grams"},{"1":"cucumbers","2":"pickling","3":"8/11/20","4":"1029","5":"grams"},{"1":"beans","2":"Chinese Red Noodle","3":"8/11/20","4":"78","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"8/11/20","4":"245","5":"grams"},{"1":"tomatoes","2":"Brandywine","3":"8/11/20","4":"218","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"8/11/20","4":"802","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"8/11/20","4":"354","5":"grams"},{"1":"tomatoes","2":"Black Krim","3":"8/11/20","4":"359","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"8/11/20","4":"506","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"8/11/20","4":"92","5":"grams"},{"1":"edamame","2":"edamame","3":"8/11/20","4":"109","5":"grams"},{"1":"corn","2":"Dorinny Sweet","3":"8/11/20","4":"330","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"8/12/20","4":"73","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/13/20","4":"1774","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"8/13/20","4":"468","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"8/13/20","4":"122","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/13/20","4":"421","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/13/20","4":"332","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"8/13/20","4":"727","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"8/13/20","4":"642","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"8/13/20","4":"413","5":"grams"},{"1":"beans","2":"Chinese Red Noodle","3":"8/13/20","4":"65","5":"grams"},{"1":"cucumbers","2":"pickling","3":"8/13/20","4":"599","5":"grams"},{"1":"basil","2":"Isle of Naxos","3":"8/13/20","4":"12","5":"grams"},{"1":"beets","2":"Sweet Merlin","3":"8/13/20","4":"198","5":"grams"},{"1":"beets","2":"Gourmet Golden","3":"8/13/20","4":"308","5":"grams"},{"1":"Swiss chard","2":"Neon Glow","3":"8/13/20","4":"517","5":"grams"},{"1":"beets","2":"Sweet Merlin","3":"8/13/20","4":"2209","5":"grams"},{"1":"beets","2":"Gourmet Golden","3":"8/13/20","4":"2476","5":"grams"},{"1":"corn","2":"Dorinny Sweet","3":"8/14/20","4":"1564","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"8/14/20","4":"80","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/14/20","4":"711","5":"grams"},{"1":"tomatoes","2":"Old German","3":"8/14/20","4":"238","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"8/14/20","4":"525","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"8/14/20","4":"181","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"8/14/20","4":"266","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"8/14/20","4":"490","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/14/20","4":"126","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/14/20","4":"371","5":"grams"},{"1":"corn","2":"Golden Bantam","3":"8/15/20","4":"383","5":"grams"},{"1":"cucumbers","2":"pickling","3":"8/15/20","4":"351","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/15/20","4":"859","5":"grams"},{"1":"basil","2":"Isle of Naxos","3":"8/15/20","4":"25","5":"grams"},{"1":"onions","2":"Long Keeping Rainbow","3":"8/15/20","4":"137","5":"grams"},{"1":"kale","2":"Heirloom Lacinto","3":"8/15/20","4":"71","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"8/15/20","4":"56","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/16/20","4":"477","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"8/16/20","4":"328","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"8/16/20","4":"45","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/16/20","4":"543","5":"grams"},{"1":"tomatoes","2":"Old German","3":"8/16/20","4":"599","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"8/16/20","4":"560","5":"grams"},{"1":"tomatoes","2":"Black Krim","3":"8/16/20","4":"291","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"8/16/20","4":"238","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"8/16/20","4":"397","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/16/20","4":"660","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"8/16/20","4":"693","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/17/20","4":"364","5":"grams"},{"1":"tomatoes","2":"Brandywine","3":"8/17/20","4":"305","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"8/17/20","4":"588","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"8/17/20","4":"764","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/17/20","4":"436","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"8/17/20","4":"306","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"8/17/20","4":"350","5":"grams"},{"1":"beans","2":"Chinese Red Noodle","3":"8/17/20","4":"105","5":"grams"},{"1":"spinach","2":"Catalina","3":"8/17/20","4":"30","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"8/17/20","4":"67","5":"grams"},{"1":"corn","2":"Golden Bantam","3":"8/17/20","4":"344","5":"grams"},{"1":"kale","2":"Heirloom Lacinto","3":"8/17/20","4":"173","5":"grams"},{"1":"basil","2":"Isle of Naxos","3":"8/18/20","4":"27","5":"grams"},{"1":"onions","2":"Long Keeping Rainbow","3":"8/18/20","4":"126","5":"grams"},{"1":"peppers","2":"variety","3":"8/18/20","4":"112","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/18/20","4":"1151","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"8/18/20","4":"225","5":"grams"},{"1":"cucumbers","2":"pickling","3":"8/18/20","4":"2888","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"8/18/20","4":"608","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/18/20","4":"136","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"8/18/20","4":"148","5":"grams"},{"1":"tomatoes","2":"Black Krim","3":"8/18/20","4":"317","5":"grams"},{"1":"tomatoes","2":"Old German","3":"8/18/20","4":"105","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/18/20","4":"271","5":"grams"},{"1":"spinach","2":"Catalina","3":"8/18/20","4":"39","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"8/18/20","4":"87","5":"grams"},{"1":"cucumbers","2":"pickling","3":"8/18/20","4":"233","5":"grams"},{"1":"edamame","2":"edamame","3":"8/18/20","4":"527","5":"grams"},{"1":"potatoes","2":"purple","3":"8/19/20","4":"323","5":"grams"},{"1":"potatoes","2":"yellow","3":"8/19/20","4":"278","5":"grams"},{"1":"hot peppers","2":"thai","3":"8/19/20","4":"31","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"8/19/20","4":"872","5":"grams"},{"1":"tomatoes","2":"Black Krim","3":"8/19/20","4":"579","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"8/19/20","4":"615","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"8/19/20","4":"997","5":"grams"},{"1":"tomatoes","2":"Brandywine","3":"8/19/20","4":"335","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"8/19/20","4":"264","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/19/20","4":"451","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"8/19/20","4":"306","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"8/20/20","4":"99","5":"grams"},{"1":"cucumbers","2":"pickling","3":"8/20/20","4":"70","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"8/20/20","4":"333","5":"grams"},{"1":"tomatoes","2":"Brandywine","3":"8/20/20","4":"483","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/20/20","4":"632","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"8/20/20","4":"360","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"8/20/20","4":"230","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"8/20/20","4":"344","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"8/20/20","4":"1010","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"8/20/20","4":"328","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"8/20/20","4":"287","5":"grams"},{"1":"lettuce","2":"Tatsoi","3":"8/20/20","4":"322","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/20/20","4":"493","5":"grams"},{"1":"peppers","2":"green","3":"8/20/20","4":"252","5":"grams"},{"1":"peppers","2":"variety","3":"8/20/20","4":"70","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/20/20","4":"834","5":"grams"},{"1":"onions","2":"Long Keeping Rainbow","3":"8/20/20","4":"113","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/21/20","4":"1122","5":"grams"},{"1":"basil","2":"Isle of Naxos","3":"8/21/20","4":"34","5":"grams"},{"1":"jalapeño","2":"giant","3":"8/21/20","4":"509","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"8/21/20","4":"1601","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"8/21/20","4":"842","5":"grams"},{"1":"tomatoes","2":"Black Krim","3":"8/21/20","4":"1538","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"8/21/20","4":"428","5":"grams"},{"1":"tomatoes","2":"Old German","3":"8/21/20","4":"243","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/21/20","4":"330","5":"grams"},{"1":"cucumbers","2":"pickling","3":"8/21/20","4":"997","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/21/20","4":"265","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"8/21/20","4":"562","5":"grams"},{"1":"carrots","2":"Dragon","3":"8/21/20","4":"457","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"8/23/20","4":"1542","5":"grams"},{"1":"tomatoes","2":"Old German","3":"8/23/20","4":"801","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/23/20","4":"436","5":"grams"},{"1":"cucumbers","2":"pickling","3":"8/23/20","4":"747","5":"grams"},{"1":"tomatoes","2":"Black Krim","3":"8/23/20","4":"1573","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"8/23/20","4":"704","5":"grams"},{"1":"tomatoes","2":"Brandywine","3":"8/23/20","4":"446","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/23/20","4":"269","5":"grams"},{"1":"corn","2":"Dorinny Sweet","3":"8/23/20","4":"661","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/23/20","4":"2436","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"8/23/20","4":"111","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"8/24/20","4":"134","5":"grams"},{"1":"peppers","2":"green","3":"8/24/20","4":"115","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/24/20","4":"75","5":"grams"},{"1":"kale","2":"Heirloom Lacinto","3":"8/24/20","4":"117","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"8/25/20","4":"578","5":"grams"},{"1":"tomatoes","2":"Brandywine","3":"8/25/20","4":"871","5":"grams"},{"1":"tomatoes","2":"Old German","3":"8/25/20","4":"115","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/25/20","4":"629","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"8/25/20","4":"186","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"8/25/20","4":"320","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"8/25/20","4":"488","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/25/20","4":"506","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/25/20","4":"920","5":"grams"},{"1":"cucumbers","2":"pickling","3":"8/25/20","4":"179","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"8/25/20","4":"1400","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"8/25/20","4":"993","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"8/25/20","4":"1026","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"8/26/20","4":"1886","5":"grams"},{"1":"tomatoes","2":"Old German","3":"8/26/20","4":"666","5":"grams"},{"1":"tomatoes","2":"Brandywine","3":"8/26/20","4":"1042","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"8/26/20","4":"593","5":"grams"},{"1":"tomatoes","2":"Black Krim","3":"8/26/20","4":"216","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"8/26/20","4":"309","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"8/26/20","4":"497","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"8/26/20","4":"261","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/26/20","4":"819","5":"grams"},{"1":"corn","2":"Dorinny Sweet","3":"8/26/20","4":"1607","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"8/27/20","4":"14","5":"grams"},{"1":"raspberries","2":"perrenial","3":"8/28/20","4":"29","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"8/28/20","4":"3244","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"8/28/20","4":"85","5":"grams"},{"1":"basil","2":"Isle of Naxos","3":"8/29/20","4":"24","5":"grams"},{"1":"onions","2":"Long Keeping Rainbow","3":"8/29/20","4":"289","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/29/20","4":"380","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"8/29/20","4":"737","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"8/29/20","4":"1033","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"8/29/20","4":"1097","5":"grams"},{"1":"edamame","2":"edamame","3":"8/29/20","4":"483","5":"grams"},{"1":"peppers","2":"variety","3":"8/29/20","4":"627","5":"grams"},{"1":"jalapeño","2":"giant","3":"8/29/20","4":"352","5":"grams"},{"1":"potatoes","2":"purple","3":"8/29/20","4":"262","5":"grams"},{"1":"potatoes","2":"yellow","3":"8/29/20","4":"716","5":"grams"},{"1":"carrots","2":"Bolero","3":"8/29/20","4":"888","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"8/29/20","4":"566","5":"grams"},{"1":"carrots","2":"greens","3":"8/29/20","4":"169","5":"grams"},{"1":"tomatoes","2":"Old German","3":"8/30/20","4":"861","5":"grams"},{"1":"tomatoes","2":"Brandywine","3":"8/30/20","4":"460","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"8/30/20","4":"2934","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"8/30/20","4":"599","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"8/30/20","4":"155","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"8/30/20","4":"822","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"8/30/20","4":"589","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"8/30/20","4":"393","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"8/30/20","4":"752","5":"grams"},{"1":"tomatoes","2":"grape","3":"8/30/20","4":"833","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"9/1/20","4":"2831","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"9/1/20","4":"1953","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"9/1/20","4":"160","5":"grams"},{"1":"pumpkins","2":"saved","3":"9/1/20","4":"4758","5":"grams"},{"1":"pumpkins","2":"saved","3":"9/1/20","4":"2342","5":"grams"},{"1":"squash","2":"Blue (saved)","3":"9/1/20","4":"3227","5":"grams"},{"1":"squash","2":"Blue (saved)","3":"9/1/20","4":"5150","5":"grams"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"9/1/20","4":"7350","5":"grams"},{"1":"tomatoes","2":"Old German","3":"9/1/20","4":"805","5":"grams"},{"1":"tomatoes","2":"Brandywine","3":"9/1/20","4":"178","5":"grams"},{"1":"tomatoes","2":"Cherokee Purple","3":"9/1/20","4":"201","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"9/1/20","4":"1537","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"9/1/20","4":"773","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"9/1/20","4":"1202","5":"grams"},{"1":"corn","2":"Dorinny Sweet","3":"9/2/20","4":"798","5":"grams"},{"1":"peppers","2":"green","3":"9/2/20","4":"370","5":"grams"},{"1":"jalapeño","2":"giant","3":"9/2/20","4":"43","5":"grams"},{"1":"peppers","2":"variety","3":"9/2/20","4":"60","5":"grams"},{"1":"tomatoes","2":"grape","3":"9/3/20","4":"1131","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"9/3/20","4":"610","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"9/3/20","4":"1265","5":"grams"},{"1":"jalapeño","2":"giant","3":"9/3/20","4":"102","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"9/4/20","4":"2160","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"9/4/20","4":"2899","5":"grams"},{"1":"tomatoes","2":"grape","3":"9/4/20","4":"442","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"9/4/20","4":"1234","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"9/4/20","4":"1178","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"9/4/20","4":"255","5":"grams"},{"1":"tomatoes","2":"Brandywine","3":"9/4/20","4":"430","5":"grams"},{"1":"onions","2":"Delicious Duo","3":"9/4/20","4":"33","5":"grams"},{"1":"Swiss chard","2":"Neon Glow","3":"9/4/20","4":"256","5":"grams"},{"1":"jalapeño","2":"giant","3":"9/4/20","4":"58","5":"grams"},{"1":"corn","2":"Dorinny Sweet","3":"9/5/20","4":"214","5":"grams"},{"1":"edamame","2":"edamame","3":"9/5/20","4":"1644","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"9/6/20","4":"2377","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"9/6/20","4":"710","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"9/6/20","4":"1317","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"9/6/20","4":"1649","5":"grams"},{"1":"tomatoes","2":"grape","3":"9/6/20","4":"615","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"9/7/20","4":"3284","5":"grams"},{"1":"zucchini","2":"Romanesco","3":"9/8/20","4":"1300","5":"grams"},{"1":"potatoes","2":"yellow","3":"9/9/20","4":"843","5":"grams"},{"1":"broccoli","2":"Main Crop Bravado","3":"9/9/20","4":"102","5":"grams"},{"1":"Swiss chard","2":"Neon Glow","3":"9/9/20","4":"228","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"9/10/20","4":"692","5":"grams"},{"1":"tomatoes","2":"Old German","3":"9/10/20","4":"674","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"9/10/20","4":"1392","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"9/10/20","4":"316","5":"grams"},{"1":"tomatoes","2":"Jet Star","3":"9/10/20","4":"754","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"9/10/20","4":"413","5":"grams"},{"1":"tomatoes","2":"grape","3":"9/10/20","4":"509","5":"grams"},{"1":"kale","2":"Heirloom Lacinto","3":"9/12/20","4":"108","5":"grams"},{"1":"tomatoes","2":"grape","3":"9/15/20","4":"258","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"9/15/20","4":"725","5":"grams"},{"1":"potatoes","2":"Russet","3":"9/16/20","4":"629","5":"grams"},{"1":"broccoli","2":"Main Crop Bravado","3":"9/16/20","4":"219","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"9/16/20","4":"8","5":"grams"},{"1":"carrots","2":"King Midas","3":"9/17/20","4":"160","5":"grams"},{"1":"carrots","2":"Bolero","3":"9/17/20","4":"168","5":"grams"},{"1":"kohlrabi","2":"Crispy Colors Duo","3":"9/17/20","4":"191","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"9/17/20","4":"212","5":"grams"},{"1":"tomatoes","2":"Brandywine","3":"9/18/20","4":"714","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"9/18/20","4":"228","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"9/18/20","4":"670","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"9/18/20","4":"1052","5":"grams"},{"1":"tomatoes","2":"Old German","3":"9/18/20","4":"1631","5":"grams"},{"1":"raspberries","2":"perrenial","3":"9/18/20","4":"137","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"9/19/20","4":"2934","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"9/19/20","4":"304","5":"grams"},{"1":"tomatoes","2":"grape","3":"9/19/20","4":"1058","5":"grams"},{"1":"squash","2":"delicata","3":"9/19/20","4":"307","5":"grams"},{"1":"squash","2":"delicata","3":"9/19/20","4":"397","5":"grams"},{"1":"squash","2":"delicata","3":"9/19/20","4":"537","5":"grams"},{"1":"squash","2":"delicata","3":"9/19/20","4":"314","5":"grams"},{"1":"squash","2":"delicata","3":"9/19/20","4":"494","5":"grams"},{"1":"squash","2":"delicata","3":"9/19/20","4":"484","5":"grams"},{"1":"squash","2":"delicata","3":"9/19/20","4":"454","5":"grams"},{"1":"squash","2":"delicata","3":"9/19/20","4":"480","5":"grams"},{"1":"squash","2":"delicata","3":"9/19/20","4":"252","5":"grams"},{"1":"squash","2":"delicata","3":"9/19/20","4":"294","5":"grams"},{"1":"squash","2":"delicata","3":"9/19/20","4":"437","5":"grams"},{"1":"squash","2":"Waltham Butternut","3":"9/19/20","4":"1834","5":"grams"},{"1":"squash","2":"Waltham Butternut","3":"9/19/20","4":"1655","5":"grams"},{"1":"squash","2":"Waltham Butternut","3":"9/19/20","4":"1927","5":"grams"},{"1":"squash","2":"Waltham Butternut","3":"9/19/20","4":"1558","5":"grams"},{"1":"squash","2":"Waltham Butternut","3":"9/19/20","4":"1183","5":"grams"},{"1":"squash","2":"Red Kuri","3":"9/19/20","4":"1178","5":"grams"},{"1":"squash","2":"Red Kuri","3":"9/19/20","4":"706","5":"grams"},{"1":"squash","2":"Red Kuri","3":"9/19/20","4":"1686","5":"grams"},{"1":"squash","2":"Red Kuri","3":"9/19/20","4":"1785","5":"grams"},{"1":"squash","2":"Blue (saved)","3":"9/19/20","4":"1923","5":"grams"},{"1":"squash","2":"Blue (saved)","3":"9/19/20","4":"2120","5":"grams"},{"1":"squash","2":"Blue (saved)","3":"9/19/20","4":"2325","5":"grams"},{"1":"squash","2":"Blue (saved)","3":"9/19/20","4":"1172","5":"grams"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"9/19/20","4":"1311","5":"grams"},{"1":"pumpkins","2":"Cinderella's Carraige","3":"9/19/20","4":"6250","5":"grams"},{"1":"pumpkins","2":"saved","3":"9/19/20","4":"1154","5":"grams"},{"1":"pumpkins","2":"saved","3":"9/19/20","4":"1208","5":"grams"},{"1":"pumpkins","2":"saved","3":"9/19/20","4":"2882","5":"grams"},{"1":"pumpkins","2":"saved","3":"9/19/20","4":"2689","5":"grams"},{"1":"pumpkins","2":"saved","3":"9/19/20","4":"3441","5":"grams"},{"1":"pumpkins","2":"saved","3":"9/19/20","4":"7050","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"9/19/20","4":"1109","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"9/19/20","4":"1028","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"9/19/20","4":"1131","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"9/19/20","4":"1302","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"9/19/20","4":"1570","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"9/19/20","4":"1359","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"9/19/20","4":"1608","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"9/19/20","4":"2277","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"9/19/20","4":"1743","5":"grams"},{"1":"pumpkins","2":"New England Sugar","3":"9/19/20","4":"2931","5":"grams"},{"1":"kale","2":"Heirloom Lacinto","3":"9/20/20","4":"163","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"9/21/20","4":"714","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"9/21/20","4":"95","5":"grams"},{"1":"tomatoes","2":"Bonny Best","3":"9/25/20","4":"477","5":"grams"},{"1":"tomatoes","2":"Amish Paste","3":"9/25/20","4":"2738","5":"grams"},{"1":"tomatoes","2":"Black Krim","3":"9/25/20","4":"236","5":"grams"},{"1":"tomatoes","2":"Old German","3":"9/25/20","4":"1823","5":"grams"},{"1":"tomatoes","2":"grape","3":"9/25/20","4":"819","5":"grams"},{"1":"tomatoes","2":"Mortgage Lifter","3":"9/25/20","4":"2006","5":"grams"},{"1":"tomatoes","2":"Big Beef","3":"9/25/20","4":"659","5":"grams"},{"1":"tomatoes","2":"Better Boy","3":"9/25/20","4":"1239","5":"grams"},{"1":"tomatoes","2":"volunteers","3":"9/25/20","4":"1978","5":"grams"},{"1":"kale","2":"Heirloom Lacinto","3":"9/25/20","4":"28","5":"grams"},{"1":"Swiss chard","2":"Neon Glow","3":"9/25/20","4":"24","5":"grams"},{"1":"broccoli","2":"Main Crop Bravado","3":"9/25/20","4":"75","5":"grams"},{"1":"peppers","2":"variety","3":"9/25/20","4":"84","5":"grams"},{"1":"apple","2":"unknown","3":"9/26/20","4":"156","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"9/26/20","4":"95","5":"grams"},{"1":"beans","2":"Bush Bush Slender","3":"9/27/20","4":"94","5":"grams"},{"1":"beans","2":"Classic Slenderette","3":"9/28/20","4":"81","5":"grams"},{"1":"lettuce","2":"Lettuce Mixture","3":"9/29/20","4":"139","5":"grams"},{"1":"broccoli","2":"Main Crop Bravado","3":"9/30/20","4":"134","5":"grams"},{"1":"carrots","2":"Dragon","3":"10/1/20","4":"883","5":"grams"},{"1":"carrots","2":"Bolero","3":"10/2/20","4":"449","5":"grams"},{"1":"Swiss chard","2":"Neon Glow","3":"10/3/20","4":"232","5":"grams"}],"options":{"columns":{"min":{},"max":[10]},"rows":{"min":[10],"max":[10]},"pages":{}}}
  </script>
</div>


  5. Create a table using `gt` with data from your project or from the `garden_harvest` data if your project data aren't ready.
  

```r
covid_UK_US <- covid19(c("GBR" ,"US"), level = 2) 
```

```
## We have invested a lot of time and effort in creating COVID-19 Data Hub, please cite the following when using it:
## 
##   Guidotti, E., Ardia, D., (2020), "COVID-19 Data Hub", Journal of Open
##   Source Software 5(51):2376, doi: 10.21105/joss.02376.
## 
## A BibTeX entry for LaTeX users is
## 
##   @Article{,
##     title = {COVID-19 Data Hub},
##     year = {2020},
##     doi = {10.21105/joss.02376},
##     author = {Emanuele Guidotti and David Ardia},
##     journal = {Journal of Open Source Software},
##     volume = {5},
##     number = {51},
##     pages = {2376},
##   }
## 
## To retrieve citation and metadata of the data sources see ?covid19cite. To hide this message use 'verbose = FALSE'.
```

```r
covid_presentation_data <- covid_UK_US %>% 
  select(id, administrative_area_level_1 , administrative_area_level_2, date, confirmed, deaths, population, latitude, longitude) %>% 
  rename(country = administrative_area_level_1,
         state = administrative_area_level_2,
         total_cases = confirmed,
         total_deaths = deaths) #

covid_presentation_data[is.na(covid_presentation_data)] <- 0

dailycovid2 <- covid_presentation_data %>% 
  group_by(state) %>% 
  mutate(lag1_cases = lag(total_cases, 1, order_by = date),
         lag1_deaths = lag(total_deaths, 1, order_by = date)
         ) %>% 
  replace_na(list(lag1_cases = 0)) %>%
  replace_na(list(lag1_deaths = 0)) %>% 
  mutate(daily_cases = total_cases - lag1_cases,
         daily_deaths = total_deaths - lag1_deaths,
         cum_cases = cumsum(daily_cases),
         cum_deaths = cumsum(daily_deaths),
         cum_cases_per_10000 = cum_cases/population*10000) 

exercise_data_subset <- dailycovid2 %>% 
  filter(country == "United Kingdom") %>% 
  filter(state  %in% c("South East", "London", "North East", "West Midlands", "South West", "East Midlands")) %>% 
  select(id, country, state, date, population, daily_cases, daily_deaths, cum_cases, cum_deaths, cum_cases_per_10000)

tab <- exercise_data_subset %>% 
  gt(
    groupname_col = "state" 
    )

tab2 <- 
  tab %>% 
  cols_hide(
    columns = vars(daily_deaths, cum_deaths, country)
  )
tab3 <- 
  tab2 %>% 
  fmt_number(vars(cum_cases_per_10000), 
             n_sigfig = 4)
tab4 <-
  tab3 %>% 
  tab_header("United Kingdom COVID-19 Cases Data by Region", subtitle = NULL
  )
tab4
```

<!--html_preserve--><style>html {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Helvetica Neue', 'Fira Sans', 'Droid Sans', Arial, sans-serif;
}

#quqlrglfxi .gt_table {
  display: table;
  border-collapse: collapse;
  margin-left: auto;
  margin-right: auto;
  color: #333333;
  font-size: 16px;
  font-weight: normal;
  font-style: normal;
  background-color: #FFFFFF;
  width: auto;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #A8A8A8;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #A8A8A8;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
}

#quqlrglfxi .gt_heading {
  background-color: #FFFFFF;
  text-align: center;
  border-bottom-color: #FFFFFF;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#quqlrglfxi .gt_title {
  color: #333333;
  font-size: 125%;
  font-weight: initial;
  padding-top: 4px;
  padding-bottom: 4px;
  border-bottom-color: #FFFFFF;
  border-bottom-width: 0;
}

#quqlrglfxi .gt_subtitle {
  color: #333333;
  font-size: 85%;
  font-weight: initial;
  padding-top: 0;
  padding-bottom: 4px;
  border-top-color: #FFFFFF;
  border-top-width: 0;
}

#quqlrglfxi .gt_bottom_border {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#quqlrglfxi .gt_col_headings {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
}

#quqlrglfxi .gt_col_heading {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  padding-left: 5px;
  padding-right: 5px;
  overflow-x: hidden;
}

#quqlrglfxi .gt_column_spanner_outer {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: normal;
  text-transform: inherit;
  padding-top: 0;
  padding-bottom: 0;
  padding-left: 4px;
  padding-right: 4px;
}

#quqlrglfxi .gt_column_spanner_outer:first-child {
  padding-left: 0;
}

#quqlrglfxi .gt_column_spanner_outer:last-child {
  padding-right: 0;
}

#quqlrglfxi .gt_column_spanner {
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: bottom;
  padding-top: 5px;
  padding-bottom: 6px;
  overflow-x: hidden;
  display: inline-block;
  width: 100%;
}

#quqlrglfxi .gt_group_heading {
  padding: 8px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
}

#quqlrglfxi .gt_empty_group_heading {
  padding: 0.5px;
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  vertical-align: middle;
}

#quqlrglfxi .gt_from_md > :first-child {
  margin-top: 0;
}

#quqlrglfxi .gt_from_md > :last-child {
  margin-bottom: 0;
}

#quqlrglfxi .gt_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  margin: 10px;
  border-top-style: solid;
  border-top-width: 1px;
  border-top-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 1px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 1px;
  border-right-color: #D3D3D3;
  vertical-align: middle;
  overflow-x: hidden;
}

#quqlrglfxi .gt_stub {
  color: #333333;
  background-color: #FFFFFF;
  font-size: 100%;
  font-weight: initial;
  text-transform: inherit;
  border-right-style: solid;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
  padding-left: 12px;
}

#quqlrglfxi .gt_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#quqlrglfxi .gt_first_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
}

#quqlrglfxi .gt_grand_summary_row {
  color: #333333;
  background-color: #FFFFFF;
  text-transform: inherit;
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
}

#quqlrglfxi .gt_first_grand_summary_row {
  padding-top: 8px;
  padding-bottom: 8px;
  padding-left: 5px;
  padding-right: 5px;
  border-top-style: double;
  border-top-width: 6px;
  border-top-color: #D3D3D3;
}

#quqlrglfxi .gt_striped {
  background-color: rgba(128, 128, 128, 0.05);
}

#quqlrglfxi .gt_table_body {
  border-top-style: solid;
  border-top-width: 2px;
  border-top-color: #D3D3D3;
  border-bottom-style: solid;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
}

#quqlrglfxi .gt_footnotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#quqlrglfxi .gt_footnote {
  margin: 0px;
  font-size: 90%;
  padding: 4px;
}

#quqlrglfxi .gt_sourcenotes {
  color: #333333;
  background-color: #FFFFFF;
  border-bottom-style: none;
  border-bottom-width: 2px;
  border-bottom-color: #D3D3D3;
  border-left-style: none;
  border-left-width: 2px;
  border-left-color: #D3D3D3;
  border-right-style: none;
  border-right-width: 2px;
  border-right-color: #D3D3D3;
}

#quqlrglfxi .gt_sourcenote {
  font-size: 90%;
  padding: 4px;
}

#quqlrglfxi .gt_left {
  text-align: left;
}

#quqlrglfxi .gt_center {
  text-align: center;
}

#quqlrglfxi .gt_right {
  text-align: right;
  font-variant-numeric: tabular-nums;
}

#quqlrglfxi .gt_font_normal {
  font-weight: normal;
}

#quqlrglfxi .gt_font_bold {
  font-weight: bold;
}

#quqlrglfxi .gt_font_italic {
  font-style: italic;
}

#quqlrglfxi .gt_super {
  font-size: 65%;
}

#quqlrglfxi .gt_footnote_marks {
  font-style: italic;
  font-size: 65%;
}
</style>
<div id="quqlrglfxi" style="overflow-x:auto;overflow-y:auto;width:auto;height:auto;"><table class="gt_table">
  <thead class="gt_header">
    <tr>
      <th colspan="6" class="gt_heading gt_title gt_font_normal" style>United Kingdom COVID-19 Cases Data by Region</th>
    </tr>
    <tr>
      <th colspan="6" class="gt_heading gt_subtitle gt_font_normal gt_bottom_border" style></th>
    </tr>
  </thead>
  <thead class="gt_col_headings">
    <tr>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">id</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_left" rowspan="1" colspan="1">date</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">population</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">daily_cases</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">cum_cases</th>
      <th class="gt_col_heading gt_columns_bottom_border gt_right" rowspan="1" colspan="1">cum_cases_per_10000</th>
    </tr>
  </thead>
  <tbody class="gt_table_body">
    <tr class="gt_group_heading_row">
      <td colspan="6" class="gt_group_heading">North East</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-02</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.003762</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-03</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.003762</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-04</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">0.007525</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-05</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">4</td>
      <td class="gt_row gt_right">0.01505</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-06</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">5</td>
      <td class="gt_row gt_right">0.01881</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-07</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">7</td>
      <td class="gt_row gt_right">0.02634</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-08</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">7</td>
      <td class="gt_row gt_right">0.02634</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-09</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">3</td>
      <td class="gt_row gt_right">10</td>
      <td class="gt_row gt_right">0.03762</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-10</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">4</td>
      <td class="gt_row gt_right">14</td>
      <td class="gt_row gt_right">0.05267</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-11</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">15</td>
      <td class="gt_row gt_right">0.05644</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-12</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">17</td>
      <td class="gt_row gt_right">0.06396</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-13</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">23</td>
      <td class="gt_row gt_right">0.08653</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-14</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">29</td>
      <td class="gt_row gt_right">0.1091</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-15</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">10</td>
      <td class="gt_row gt_right">39</td>
      <td class="gt_row gt_right">0.1467</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-16</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">7</td>
      <td class="gt_row gt_right">46</td>
      <td class="gt_row gt_right">0.1731</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-17</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">16</td>
      <td class="gt_row gt_right">62</td>
      <td class="gt_row gt_right">0.2333</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-18</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">14</td>
      <td class="gt_row gt_right">76</td>
      <td class="gt_row gt_right">0.2859</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-19</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">19</td>
      <td class="gt_row gt_right">95</td>
      <td class="gt_row gt_right">0.3574</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-20</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">26</td>
      <td class="gt_row gt_right">121</td>
      <td class="gt_row gt_right">0.4552</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-21</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">23</td>
      <td class="gt_row gt_right">144</td>
      <td class="gt_row gt_right">0.5418</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-22</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">20</td>
      <td class="gt_row gt_right">164</td>
      <td class="gt_row gt_right">0.6170</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-23</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">58</td>
      <td class="gt_row gt_right">222</td>
      <td class="gt_row gt_right">0.8352</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-24</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">76</td>
      <td class="gt_row gt_right">298</td>
      <td class="gt_row gt_right">1.121</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-25</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">76</td>
      <td class="gt_row gt_right">374</td>
      <td class="gt_row gt_right">1.407</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-26</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">97</td>
      <td class="gt_row gt_right">471</td>
      <td class="gt_row gt_right">1.772</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-27</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">102</td>
      <td class="gt_row gt_right">573</td>
      <td class="gt_row gt_right">2.156</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-28</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">113</td>
      <td class="gt_row gt_right">686</td>
      <td class="gt_row gt_right">2.581</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-29</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">107</td>
      <td class="gt_row gt_right">793</td>
      <td class="gt_row gt_right">2.984</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-30</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">176</td>
      <td class="gt_row gt_right">969</td>
      <td class="gt_row gt_right">3.646</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-03-31</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">210</td>
      <td class="gt_row gt_right">1179</td>
      <td class="gt_row gt_right">4.436</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-01</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">307</td>
      <td class="gt_row gt_right">1486</td>
      <td class="gt_row gt_right">5.591</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-02</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">219</td>
      <td class="gt_row gt_right">1705</td>
      <td class="gt_row gt_right">6.415</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-03</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">261</td>
      <td class="gt_row gt_right">1966</td>
      <td class="gt_row gt_right">7.397</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-04</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">254</td>
      <td class="gt_row gt_right">2220</td>
      <td class="gt_row gt_right">8.352</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-05</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">221</td>
      <td class="gt_row gt_right">2441</td>
      <td class="gt_row gt_right">9.184</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-06</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">375</td>
      <td class="gt_row gt_right">2816</td>
      <td class="gt_row gt_right">10.59</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-07</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">350</td>
      <td class="gt_row gt_right">3166</td>
      <td class="gt_row gt_right">11.91</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-08</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">292</td>
      <td class="gt_row gt_right">3458</td>
      <td class="gt_row gt_right">13.01</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-09</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">314</td>
      <td class="gt_row gt_right">3772</td>
      <td class="gt_row gt_right">14.19</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-10</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">245</td>
      <td class="gt_row gt_right">4017</td>
      <td class="gt_row gt_right">15.11</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-11</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">267</td>
      <td class="gt_row gt_right">4284</td>
      <td class="gt_row gt_right">16.12</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-12</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">201</td>
      <td class="gt_row gt_right">4485</td>
      <td class="gt_row gt_right">16.87</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-13</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">232</td>
      <td class="gt_row gt_right">4717</td>
      <td class="gt_row gt_right">17.75</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-14</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">256</td>
      <td class="gt_row gt_right">4973</td>
      <td class="gt_row gt_right">18.71</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-15</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">284</td>
      <td class="gt_row gt_right">5257</td>
      <td class="gt_row gt_right">19.78</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-16</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">295</td>
      <td class="gt_row gt_right">5552</td>
      <td class="gt_row gt_right">20.89</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-17</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">292</td>
      <td class="gt_row gt_right">5844</td>
      <td class="gt_row gt_right">21.99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-18</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">379</td>
      <td class="gt_row gt_right">6223</td>
      <td class="gt_row gt_right">23.41</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-19</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">276</td>
      <td class="gt_row gt_right">6499</td>
      <td class="gt_row gt_right">24.45</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-20</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">315</td>
      <td class="gt_row gt_right">6814</td>
      <td class="gt_row gt_right">25.64</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-21</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">288</td>
      <td class="gt_row gt_right">7102</td>
      <td class="gt_row gt_right">26.72</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-22</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">361</td>
      <td class="gt_row gt_right">7463</td>
      <td class="gt_row gt_right">28.08</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-23</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">333</td>
      <td class="gt_row gt_right">7796</td>
      <td class="gt_row gt_right">29.33</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-24</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">332</td>
      <td class="gt_row gt_right">8128</td>
      <td class="gt_row gt_right">30.58</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-25</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">215</td>
      <td class="gt_row gt_right">8343</td>
      <td class="gt_row gt_right">31.39</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-26</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">159</td>
      <td class="gt_row gt_right">8502</td>
      <td class="gt_row gt_right">31.99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-27</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">283</td>
      <td class="gt_row gt_right">8785</td>
      <td class="gt_row gt_right">33.05</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-28</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">360</td>
      <td class="gt_row gt_right">9145</td>
      <td class="gt_row gt_right">34.41</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-29</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">359</td>
      <td class="gt_row gt_right">9504</td>
      <td class="gt_row gt_right">35.76</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-04-30</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">384</td>
      <td class="gt_row gt_right">9888</td>
      <td class="gt_row gt_right">37.20</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-01</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">273</td>
      <td class="gt_row gt_right">10161</td>
      <td class="gt_row gt_right">38.23</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-02</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">183</td>
      <td class="gt_row gt_right">10344</td>
      <td class="gt_row gt_right">38.92</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-03</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">162</td>
      <td class="gt_row gt_right">10506</td>
      <td class="gt_row gt_right">39.53</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-04</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">234</td>
      <td class="gt_row gt_right">10740</td>
      <td class="gt_row gt_right">40.41</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-05</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">252</td>
      <td class="gt_row gt_right">10992</td>
      <td class="gt_row gt_right">41.36</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-06</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">243</td>
      <td class="gt_row gt_right">11235</td>
      <td class="gt_row gt_right">42.27</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-07</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">199</td>
      <td class="gt_row gt_right">11434</td>
      <td class="gt_row gt_right">43.02</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-08</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">158</td>
      <td class="gt_row gt_right">11592</td>
      <td class="gt_row gt_right">43.61</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-09</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">151</td>
      <td class="gt_row gt_right">11743</td>
      <td class="gt_row gt_right">44.18</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-10</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">150</td>
      <td class="gt_row gt_right">11893</td>
      <td class="gt_row gt_right">44.75</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-11</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">229</td>
      <td class="gt_row gt_right">12122</td>
      <td class="gt_row gt_right">45.61</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-12</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">220</td>
      <td class="gt_row gt_right">12342</td>
      <td class="gt_row gt_right">46.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-13</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">143</td>
      <td class="gt_row gt_right">12485</td>
      <td class="gt_row gt_right">46.97</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-14</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">139</td>
      <td class="gt_row gt_right">12624</td>
      <td class="gt_row gt_right">47.50</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-15</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">137</td>
      <td class="gt_row gt_right">12761</td>
      <td class="gt_row gt_right">48.01</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-16</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">126</td>
      <td class="gt_row gt_right">12887</td>
      <td class="gt_row gt_right">48.49</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-17</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">98</td>
      <td class="gt_row gt_right">12985</td>
      <td class="gt_row gt_right">48.85</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-18</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">147</td>
      <td class="gt_row gt_right">13132</td>
      <td class="gt_row gt_right">49.41</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-19</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">124</td>
      <td class="gt_row gt_right">13256</td>
      <td class="gt_row gt_right">49.87</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-20</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">131</td>
      <td class="gt_row gt_right">13387</td>
      <td class="gt_row gt_right">50.37</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-21</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">130</td>
      <td class="gt_row gt_right">13517</td>
      <td class="gt_row gt_right">50.86</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-22</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">95</td>
      <td class="gt_row gt_right">13612</td>
      <td class="gt_row gt_right">51.21</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-23</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">76</td>
      <td class="gt_row gt_right">13688</td>
      <td class="gt_row gt_right">51.50</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-24</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">76</td>
      <td class="gt_row gt_right">13764</td>
      <td class="gt_row gt_right">51.79</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-25</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">59</td>
      <td class="gt_row gt_right">13823</td>
      <td class="gt_row gt_right">52.01</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-26</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">100</td>
      <td class="gt_row gt_right">13923</td>
      <td class="gt_row gt_right">52.38</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-27</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">115</td>
      <td class="gt_row gt_right">14038</td>
      <td class="gt_row gt_right">52.82</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-28</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">49</td>
      <td class="gt_row gt_right">14087</td>
      <td class="gt_row gt_right">53.00</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-29</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">70</td>
      <td class="gt_row gt_right">14157</td>
      <td class="gt_row gt_right">53.26</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-30</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">47</td>
      <td class="gt_row gt_right">14204</td>
      <td class="gt_row gt_right">53.44</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-05-31</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">46</td>
      <td class="gt_row gt_right">14250</td>
      <td class="gt_row gt_right">53.61</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-01</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">70</td>
      <td class="gt_row gt_right">14320</td>
      <td class="gt_row gt_right">53.88</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-02</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">59</td>
      <td class="gt_row gt_right">14379</td>
      <td class="gt_row gt_right">54.10</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-03</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">51</td>
      <td class="gt_row gt_right">14430</td>
      <td class="gt_row gt_right">54.29</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-04</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">59</td>
      <td class="gt_row gt_right">14489</td>
      <td class="gt_row gt_right">54.51</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-05</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">42</td>
      <td class="gt_row gt_right">14531</td>
      <td class="gt_row gt_right">54.67</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-06</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">28</td>
      <td class="gt_row gt_right">14559</td>
      <td class="gt_row gt_right">54.78</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-07</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">17</td>
      <td class="gt_row gt_right">14576</td>
      <td class="gt_row gt_right">54.84</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-08</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">34</td>
      <td class="gt_row gt_right">14610</td>
      <td class="gt_row gt_right">54.97</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-09</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">40</td>
      <td class="gt_row gt_right">14650</td>
      <td class="gt_row gt_right">55.12</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-10</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">36</td>
      <td class="gt_row gt_right">14686</td>
      <td class="gt_row gt_right">55.25</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-11</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">23</td>
      <td class="gt_row gt_right">14709</td>
      <td class="gt_row gt_right">55.34</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-12</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">40</td>
      <td class="gt_row gt_right">14749</td>
      <td class="gt_row gt_right">55.49</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-13</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">32</td>
      <td class="gt_row gt_right">14781</td>
      <td class="gt_row gt_right">55.61</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-14</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">31</td>
      <td class="gt_row gt_right">14812</td>
      <td class="gt_row gt_right">55.73</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-15</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">32</td>
      <td class="gt_row gt_right">14844</td>
      <td class="gt_row gt_right">55.85</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-16</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">18</td>
      <td class="gt_row gt_right">14862</td>
      <td class="gt_row gt_right">55.92</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-17</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">19</td>
      <td class="gt_row gt_right">14881</td>
      <td class="gt_row gt_right">55.99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-18</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">23</td>
      <td class="gt_row gt_right">14904</td>
      <td class="gt_row gt_right">56.07</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-19</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">14</td>
      <td class="gt_row gt_right">14918</td>
      <td class="gt_row gt_right">56.13</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-20</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">17</td>
      <td class="gt_row gt_right">14935</td>
      <td class="gt_row gt_right">56.19</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-21</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">4</td>
      <td class="gt_row gt_right">14939</td>
      <td class="gt_row gt_right">56.21</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-22</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">24</td>
      <td class="gt_row gt_right">14963</td>
      <td class="gt_row gt_right">56.30</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-23</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">26</td>
      <td class="gt_row gt_right">14989</td>
      <td class="gt_row gt_right">56.39</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-24</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">11</td>
      <td class="gt_row gt_right">15000</td>
      <td class="gt_row gt_right">56.44</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-25</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">12</td>
      <td class="gt_row gt_right">15012</td>
      <td class="gt_row gt_right">56.48</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-26</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">8</td>
      <td class="gt_row gt_right">15020</td>
      <td class="gt_row gt_right">56.51</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-27</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">14</td>
      <td class="gt_row gt_right">15034</td>
      <td class="gt_row gt_right">56.56</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-28</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">9</td>
      <td class="gt_row gt_right">15043</td>
      <td class="gt_row gt_right">56.60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-29</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">11</td>
      <td class="gt_row gt_right">15054</td>
      <td class="gt_row gt_right">56.64</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-06-30</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">11</td>
      <td class="gt_row gt_right">15065</td>
      <td class="gt_row gt_right">56.68</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-01</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">7</td>
      <td class="gt_row gt_right">15072</td>
      <td class="gt_row gt_right">56.71</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-02</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">9</td>
      <td class="gt_row gt_right">15081</td>
      <td class="gt_row gt_right">56.74</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-03</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">8</td>
      <td class="gt_row gt_right">15089</td>
      <td class="gt_row gt_right">56.77</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-04</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">9</td>
      <td class="gt_row gt_right">15098</td>
      <td class="gt_row gt_right">56.80</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-05</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">4</td>
      <td class="gt_row gt_right">15102</td>
      <td class="gt_row gt_right">56.82</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-06</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">11</td>
      <td class="gt_row gt_right">15113</td>
      <td class="gt_row gt_right">56.86</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-07</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">12</td>
      <td class="gt_row gt_right">15125</td>
      <td class="gt_row gt_right">56.91</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-08</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">12</td>
      <td class="gt_row gt_right">15137</td>
      <td class="gt_row gt_right">56.95</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-09</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">17</td>
      <td class="gt_row gt_right">15154</td>
      <td class="gt_row gt_right">57.01</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-10</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">10</td>
      <td class="gt_row gt_right">15164</td>
      <td class="gt_row gt_right">57.05</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-11</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">8</td>
      <td class="gt_row gt_right">15172</td>
      <td class="gt_row gt_right">57.08</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-12</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">3</td>
      <td class="gt_row gt_right">15175</td>
      <td class="gt_row gt_right">57.09</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-13</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">9</td>
      <td class="gt_row gt_right">15184</td>
      <td class="gt_row gt_right">57.13</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-14</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">12</td>
      <td class="gt_row gt_right">15196</td>
      <td class="gt_row gt_right">57.17</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-15</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">16</td>
      <td class="gt_row gt_right">15212</td>
      <td class="gt_row gt_right">57.23</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-16</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">15</td>
      <td class="gt_row gt_right">15227</td>
      <td class="gt_row gt_right">57.29</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-17</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">10</td>
      <td class="gt_row gt_right">15237</td>
      <td class="gt_row gt_right">57.33</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-18</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">4</td>
      <td class="gt_row gt_right">15241</td>
      <td class="gt_row gt_right">57.34</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-19</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">7</td>
      <td class="gt_row gt_right">15248</td>
      <td class="gt_row gt_right">57.37</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-20</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">11</td>
      <td class="gt_row gt_right">15259</td>
      <td class="gt_row gt_right">57.41</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-21</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">14</td>
      <td class="gt_row gt_right">15273</td>
      <td class="gt_row gt_right">57.46</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-22</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">16</td>
      <td class="gt_row gt_right">15289</td>
      <td class="gt_row gt_right">57.52</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-23</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">8</td>
      <td class="gt_row gt_right">15297</td>
      <td class="gt_row gt_right">57.55</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-24</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">7</td>
      <td class="gt_row gt_right">15304</td>
      <td class="gt_row gt_right">57.58</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-25</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">5</td>
      <td class="gt_row gt_right">15309</td>
      <td class="gt_row gt_right">57.60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-26</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">15315</td>
      <td class="gt_row gt_right">57.62</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-27</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">9</td>
      <td class="gt_row gt_right">15324</td>
      <td class="gt_row gt_right">57.65</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-28</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">5</td>
      <td class="gt_row gt_right">15329</td>
      <td class="gt_row gt_right">57.67</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-29</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">21</td>
      <td class="gt_row gt_right">15350</td>
      <td class="gt_row gt_right">57.75</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-30</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">15</td>
      <td class="gt_row gt_right">15365</td>
      <td class="gt_row gt_right">57.81</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-07-31</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">12</td>
      <td class="gt_row gt_right">15377</td>
      <td class="gt_row gt_right">57.85</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-01</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">9</td>
      <td class="gt_row gt_right">15386</td>
      <td class="gt_row gt_right">57.89</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-02</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">16</td>
      <td class="gt_row gt_right">15402</td>
      <td class="gt_row gt_right">57.95</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-03</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">19</td>
      <td class="gt_row gt_right">15421</td>
      <td class="gt_row gt_right">58.02</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-04</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">20</td>
      <td class="gt_row gt_right">15441</td>
      <td class="gt_row gt_right">58.09</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-05</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">30</td>
      <td class="gt_row gt_right">15471</td>
      <td class="gt_row gt_right">58.21</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-06</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">24</td>
      <td class="gt_row gt_right">15495</td>
      <td class="gt_row gt_right">58.30</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-07</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">15</td>
      <td class="gt_row gt_right">15510</td>
      <td class="gt_row gt_right">58.35</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-08</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">12</td>
      <td class="gt_row gt_right">15522</td>
      <td class="gt_row gt_right">58.40</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-09</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">15</td>
      <td class="gt_row gt_right">15537</td>
      <td class="gt_row gt_right">58.46</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-10</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">21</td>
      <td class="gt_row gt_right">15558</td>
      <td class="gt_row gt_right">58.53</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-11</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">45</td>
      <td class="gt_row gt_right">15603</td>
      <td class="gt_row gt_right">58.70</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-12</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">39</td>
      <td class="gt_row gt_right">15642</td>
      <td class="gt_row gt_right">58.85</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-13</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">53</td>
      <td class="gt_row gt_right">15695</td>
      <td class="gt_row gt_right">59.05</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-14</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">31</td>
      <td class="gt_row gt_right">15726</td>
      <td class="gt_row gt_right">59.17</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-15</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">18</td>
      <td class="gt_row gt_right">15744</td>
      <td class="gt_row gt_right">59.23</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-16</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">11</td>
      <td class="gt_row gt_right">15755</td>
      <td class="gt_row gt_right">59.28</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-17</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">32</td>
      <td class="gt_row gt_right">15787</td>
      <td class="gt_row gt_right">59.40</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-18</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">36</td>
      <td class="gt_row gt_right">15823</td>
      <td class="gt_row gt_right">59.53</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-19</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">46</td>
      <td class="gt_row gt_right">15869</td>
      <td class="gt_row gt_right">59.70</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-20</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">43</td>
      <td class="gt_row gt_right">15912</td>
      <td class="gt_row gt_right">59.87</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-21</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">42</td>
      <td class="gt_row gt_right">15954</td>
      <td class="gt_row gt_right">60.02</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-22</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">33</td>
      <td class="gt_row gt_right">15987</td>
      <td class="gt_row gt_right">60.15</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-23</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">23</td>
      <td class="gt_row gt_right">16010</td>
      <td class="gt_row gt_right">60.24</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-24</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">39</td>
      <td class="gt_row gt_right">16049</td>
      <td class="gt_row gt_right">60.38</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-25</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">49</td>
      <td class="gt_row gt_right">16098</td>
      <td class="gt_row gt_right">60.57</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-26</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">69</td>
      <td class="gt_row gt_right">16167</td>
      <td class="gt_row gt_right">60.83</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-27</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">66</td>
      <td class="gt_row gt_right">16233</td>
      <td class="gt_row gt_right">61.07</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-28</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">67</td>
      <td class="gt_row gt_right">16300</td>
      <td class="gt_row gt_right">61.33</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-29</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">78</td>
      <td class="gt_row gt_right">16378</td>
      <td class="gt_row gt_right">61.62</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-30</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">61</td>
      <td class="gt_row gt_right">16439</td>
      <td class="gt_row gt_right">61.85</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-08-31</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">91</td>
      <td class="gt_row gt_right">16530</td>
      <td class="gt_row gt_right">62.19</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-01</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">117</td>
      <td class="gt_row gt_right">16647</td>
      <td class="gt_row gt_right">62.63</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-02</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">131</td>
      <td class="gt_row gt_right">16778</td>
      <td class="gt_row gt_right">63.12</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-03</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">203</td>
      <td class="gt_row gt_right">16981</td>
      <td class="gt_row gt_right">63.89</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-04</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">175</td>
      <td class="gt_row gt_right">17156</td>
      <td class="gt_row gt_right">64.55</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-05</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">231</td>
      <td class="gt_row gt_right">17387</td>
      <td class="gt_row gt_right">65.42</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-06</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">173</td>
      <td class="gt_row gt_right">17560</td>
      <td class="gt_row gt_right">66.07</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-07</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">202</td>
      <td class="gt_row gt_right">17762</td>
      <td class="gt_row gt_right">66.83</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-08</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">226</td>
      <td class="gt_row gt_right">17988</td>
      <td class="gt_row gt_right">67.68</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-09</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">236</td>
      <td class="gt_row gt_right">18224</td>
      <td class="gt_row gt_right">68.57</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-10</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">237</td>
      <td class="gt_row gt_right">18461</td>
      <td class="gt_row gt_right">69.46</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-11</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">197</td>
      <td class="gt_row gt_right">18658</td>
      <td class="gt_row gt_right">70.20</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-12</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">154</td>
      <td class="gt_row gt_right">18812</td>
      <td class="gt_row gt_right">70.78</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-13</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">145</td>
      <td class="gt_row gt_right">18957</td>
      <td class="gt_row gt_right">71.32</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-14</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">197</td>
      <td class="gt_row gt_right">19154</td>
      <td class="gt_row gt_right">72.06</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-15</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">262</td>
      <td class="gt_row gt_right">19416</td>
      <td class="gt_row gt_right">73.05</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-16</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">322</td>
      <td class="gt_row gt_right">19738</td>
      <td class="gt_row gt_right">74.26</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-17</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">369</td>
      <td class="gt_row gt_right">20107</td>
      <td class="gt_row gt_right">75.65</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-18</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">462</td>
      <td class="gt_row gt_right">20569</td>
      <td class="gt_row gt_right">77.39</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-19</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">424</td>
      <td class="gt_row gt_right">20993</td>
      <td class="gt_row gt_right">78.98</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-20</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">578</td>
      <td class="gt_row gt_right">21571</td>
      <td class="gt_row gt_right">81.16</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-21</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">603</td>
      <td class="gt_row gt_right">22174</td>
      <td class="gt_row gt_right">83.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-22</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">538</td>
      <td class="gt_row gt_right">22712</td>
      <td class="gt_row gt_right">85.45</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-23</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">624</td>
      <td class="gt_row gt_right">23336</td>
      <td class="gt_row gt_right">87.80</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-24</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">704</td>
      <td class="gt_row gt_right">24040</td>
      <td class="gt_row gt_right">90.45</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-25</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">593</td>
      <td class="gt_row gt_right">24633</td>
      <td class="gt_row gt_right">92.68</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-26</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">713</td>
      <td class="gt_row gt_right">25346</td>
      <td class="gt_row gt_right">95.36</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-27</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">595</td>
      <td class="gt_row gt_right">25941</td>
      <td class="gt_row gt_right">97.60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-28</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">903</td>
      <td class="gt_row gt_right">26844</td>
      <td class="gt_row gt_right">101.0</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-29</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">928</td>
      <td class="gt_row gt_right">27772</td>
      <td class="gt_row gt_right">104.5</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-09-30</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">886</td>
      <td class="gt_row gt_right">28658</td>
      <td class="gt_row gt_right">107.8</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-10-01</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">1052</td>
      <td class="gt_row gt_right">29710</td>
      <td class="gt_row gt_right">111.8</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-10-02</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">1081</td>
      <td class="gt_row gt_right">30791</td>
      <td class="gt_row gt_right">115.8</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-10-03</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">988</td>
      <td class="gt_row gt_right">31779</td>
      <td class="gt_row gt_right">119.6</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-10-04</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">997</td>
      <td class="gt_row gt_right">32776</td>
      <td class="gt_row gt_right">123.3</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-10-05</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">1232</td>
      <td class="gt_row gt_right">34008</td>
      <td class="gt_row gt_right">128.0</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-10-06</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">1259</td>
      <td class="gt_row gt_right">35267</td>
      <td class="gt_row gt_right">132.7</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-10-07</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">1329</td>
      <td class="gt_row gt_right">36596</td>
      <td class="gt_row gt_right">137.7</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-10-08</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">1169</td>
      <td class="gt_row gt_right">37765</td>
      <td class="gt_row gt_right">142.1</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-10-09</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">973</td>
      <td class="gt_row gt_right">38738</td>
      <td class="gt_row gt_right">145.7</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-10-10</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">674</td>
      <td class="gt_row gt_right">39412</td>
      <td class="gt_row gt_right">148.3</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-10-11</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">668</td>
      <td class="gt_row gt_right">40080</td>
      <td class="gt_row gt_right">150.8</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-10-12</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">40086</td>
      <td class="gt_row gt_right">150.8</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0138ef99</td>
      <td class="gt_row gt_left">2020-10-13</td>
      <td class="gt_row gt_right">2657909</td>
      <td class="gt_row gt_right">-40086</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">0</td>
    </tr>
    <tr class="gt_group_heading_row">
      <td colspan="6" class="gt_group_heading">West Midlands</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-01</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">0.003389</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-02</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">4</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">0.01017</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-03</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">7</td>
      <td class="gt_row gt_right">0.01186</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-04</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">3</td>
      <td class="gt_row gt_right">10</td>
      <td class="gt_row gt_right">0.01695</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-05</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">16</td>
      <td class="gt_row gt_right">0.02712</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-06</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">18</td>
      <td class="gt_row gt_right">0.03050</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-07</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">24</td>
      <td class="gt_row gt_right">0.04067</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-08</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">4</td>
      <td class="gt_row gt_right">28</td>
      <td class="gt_row gt_right">0.04745</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-09</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">8</td>
      <td class="gt_row gt_right">36</td>
      <td class="gt_row gt_right">0.06101</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-10</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">17</td>
      <td class="gt_row gt_right">53</td>
      <td class="gt_row gt_right">0.08982</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-11</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">29</td>
      <td class="gt_row gt_right">82</td>
      <td class="gt_row gt_right">0.1390</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-12</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">46</td>
      <td class="gt_row gt_right">128</td>
      <td class="gt_row gt_right">0.2169</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-13</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">40</td>
      <td class="gt_row gt_right">168</td>
      <td class="gt_row gt_right">0.2847</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-14</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">31</td>
      <td class="gt_row gt_right">199</td>
      <td class="gt_row gt_right">0.3372</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-15</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">44</td>
      <td class="gt_row gt_right">243</td>
      <td class="gt_row gt_right">0.4118</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-16</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">56</td>
      <td class="gt_row gt_right">299</td>
      <td class="gt_row gt_right">0.5067</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-17</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">69</td>
      <td class="gt_row gt_right">368</td>
      <td class="gt_row gt_right">0.6236</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-18</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">99</td>
      <td class="gt_row gt_right">467</td>
      <td class="gt_row gt_right">0.7914</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-19</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">123</td>
      <td class="gt_row gt_right">590</td>
      <td class="gt_row gt_right">0.9999</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-20</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">151</td>
      <td class="gt_row gt_right">741</td>
      <td class="gt_row gt_right">1.256</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-21</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">125</td>
      <td class="gt_row gt_right">866</td>
      <td class="gt_row gt_right">1.468</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-22</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">112</td>
      <td class="gt_row gt_right">978</td>
      <td class="gt_row gt_right">1.657</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-23</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">239</td>
      <td class="gt_row gt_right">1217</td>
      <td class="gt_row gt_right">2.062</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-24</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">250</td>
      <td class="gt_row gt_right">1467</td>
      <td class="gt_row gt_right">2.486</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-25</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">240</td>
      <td class="gt_row gt_right">1707</td>
      <td class="gt_row gt_right">2.893</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-26</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">313</td>
      <td class="gt_row gt_right">2020</td>
      <td class="gt_row gt_right">3.423</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-27</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">279</td>
      <td class="gt_row gt_right">2299</td>
      <td class="gt_row gt_right">3.896</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-28</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">323</td>
      <td class="gt_row gt_right">2622</td>
      <td class="gt_row gt_right">4.443</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-29</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">366</td>
      <td class="gt_row gt_right">2988</td>
      <td class="gt_row gt_right">5.064</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-30</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">431</td>
      <td class="gt_row gt_right">3419</td>
      <td class="gt_row gt_right">5.794</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-03-31</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">412</td>
      <td class="gt_row gt_right">3831</td>
      <td class="gt_row gt_right">6.492</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-01</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">410</td>
      <td class="gt_row gt_right">4241</td>
      <td class="gt_row gt_right">7.187</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-02</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">447</td>
      <td class="gt_row gt_right">4688</td>
      <td class="gt_row gt_right">7.945</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-03</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">372</td>
      <td class="gt_row gt_right">5060</td>
      <td class="gt_row gt_right">8.575</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-04</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">346</td>
      <td class="gt_row gt_right">5406</td>
      <td class="gt_row gt_right">9.162</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-05</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">290</td>
      <td class="gt_row gt_right">5696</td>
      <td class="gt_row gt_right">9.653</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-06</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">360</td>
      <td class="gt_row gt_right">6056</td>
      <td class="gt_row gt_right">10.26</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-07</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">387</td>
      <td class="gt_row gt_right">6443</td>
      <td class="gt_row gt_right">10.92</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-08</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">361</td>
      <td class="gt_row gt_right">6804</td>
      <td class="gt_row gt_right">11.53</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-09</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">345</td>
      <td class="gt_row gt_right">7149</td>
      <td class="gt_row gt_right">12.12</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-10</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">362</td>
      <td class="gt_row gt_right">7511</td>
      <td class="gt_row gt_right">12.73</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-11</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">253</td>
      <td class="gt_row gt_right">7764</td>
      <td class="gt_row gt_right">13.16</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-12</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">239</td>
      <td class="gt_row gt_right">8003</td>
      <td class="gt_row gt_right">13.56</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-13</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">281</td>
      <td class="gt_row gt_right">8284</td>
      <td class="gt_row gt_right">14.04</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-14</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">322</td>
      <td class="gt_row gt_right">8606</td>
      <td class="gt_row gt_right">14.58</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-15</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">387</td>
      <td class="gt_row gt_right">8993</td>
      <td class="gt_row gt_right">15.24</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-16</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">511</td>
      <td class="gt_row gt_right">9504</td>
      <td class="gt_row gt_right">16.11</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-17</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">445</td>
      <td class="gt_row gt_right">9949</td>
      <td class="gt_row gt_right">16.86</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-18</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">547</td>
      <td class="gt_row gt_right">10496</td>
      <td class="gt_row gt_right">17.79</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-19</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">340</td>
      <td class="gt_row gt_right">10836</td>
      <td class="gt_row gt_right">18.36</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-20</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">401</td>
      <td class="gt_row gt_right">11237</td>
      <td class="gt_row gt_right">19.04</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-21</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">492</td>
      <td class="gt_row gt_right">11729</td>
      <td class="gt_row gt_right">19.88</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-22</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">578</td>
      <td class="gt_row gt_right">12307</td>
      <td class="gt_row gt_right">20.86</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-23</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">550</td>
      <td class="gt_row gt_right">12857</td>
      <td class="gt_row gt_right">21.79</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-24</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">482</td>
      <td class="gt_row gt_right">13339</td>
      <td class="gt_row gt_right">22.61</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-25</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">386</td>
      <td class="gt_row gt_right">13725</td>
      <td class="gt_row gt_right">23.26</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-26</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">315</td>
      <td class="gt_row gt_right">14040</td>
      <td class="gt_row gt_right">23.79</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-27</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">418</td>
      <td class="gt_row gt_right">14458</td>
      <td class="gt_row gt_right">24.50</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-28</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">376</td>
      <td class="gt_row gt_right">14834</td>
      <td class="gt_row gt_right">25.14</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-29</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">441</td>
      <td class="gt_row gt_right">15275</td>
      <td class="gt_row gt_right">25.89</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-04-30</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">452</td>
      <td class="gt_row gt_right">15727</td>
      <td class="gt_row gt_right">26.65</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-01</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">469</td>
      <td class="gt_row gt_right">16196</td>
      <td class="gt_row gt_right">27.45</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-02</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">318</td>
      <td class="gt_row gt_right">16514</td>
      <td class="gt_row gt_right">27.99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-03</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">297</td>
      <td class="gt_row gt_right">16811</td>
      <td class="gt_row gt_right">28.49</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-04</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">235</td>
      <td class="gt_row gt_right">17046</td>
      <td class="gt_row gt_right">28.89</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-05</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">317</td>
      <td class="gt_row gt_right">17363</td>
      <td class="gt_row gt_right">29.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-06</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">314</td>
      <td class="gt_row gt_right">17677</td>
      <td class="gt_row gt_right">29.96</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-07</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">369</td>
      <td class="gt_row gt_right">18046</td>
      <td class="gt_row gt_right">30.58</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-08</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">341</td>
      <td class="gt_row gt_right">18387</td>
      <td class="gt_row gt_right">31.16</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-09</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">163</td>
      <td class="gt_row gt_right">18550</td>
      <td class="gt_row gt_right">31.44</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-10</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">217</td>
      <td class="gt_row gt_right">18767</td>
      <td class="gt_row gt_right">31.80</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-11</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">248</td>
      <td class="gt_row gt_right">19015</td>
      <td class="gt_row gt_right">32.22</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-12</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">308</td>
      <td class="gt_row gt_right">19323</td>
      <td class="gt_row gt_right">32.75</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-13</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">244</td>
      <td class="gt_row gt_right">19567</td>
      <td class="gt_row gt_right">33.16</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-14</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">258</td>
      <td class="gt_row gt_right">19825</td>
      <td class="gt_row gt_right">33.60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-15</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">266</td>
      <td class="gt_row gt_right">20091</td>
      <td class="gt_row gt_right">34.05</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-16</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">175</td>
      <td class="gt_row gt_right">20266</td>
      <td class="gt_row gt_right">34.34</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-17</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">172</td>
      <td class="gt_row gt_right">20438</td>
      <td class="gt_row gt_right">34.64</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-18</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">208</td>
      <td class="gt_row gt_right">20646</td>
      <td class="gt_row gt_right">34.99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-19</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">278</td>
      <td class="gt_row gt_right">20924</td>
      <td class="gt_row gt_right">35.46</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-20</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">252</td>
      <td class="gt_row gt_right">21176</td>
      <td class="gt_row gt_right">35.89</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-21</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">226</td>
      <td class="gt_row gt_right">21402</td>
      <td class="gt_row gt_right">36.27</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-22</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">196</td>
      <td class="gt_row gt_right">21598</td>
      <td class="gt_row gt_right">36.60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-23</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">148</td>
      <td class="gt_row gt_right">21746</td>
      <td class="gt_row gt_right">36.85</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-24</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">134</td>
      <td class="gt_row gt_right">21880</td>
      <td class="gt_row gt_right">37.08</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-25</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">138</td>
      <td class="gt_row gt_right">22018</td>
      <td class="gt_row gt_right">37.31</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-26</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">152</td>
      <td class="gt_row gt_right">22170</td>
      <td class="gt_row gt_right">37.57</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-27</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">193</td>
      <td class="gt_row gt_right">22363</td>
      <td class="gt_row gt_right">37.90</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-28</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">154</td>
      <td class="gt_row gt_right">22517</td>
      <td class="gt_row gt_right">38.16</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-29</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">155</td>
      <td class="gt_row gt_right">22672</td>
      <td class="gt_row gt_right">38.42</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-30</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">115</td>
      <td class="gt_row gt_right">22787</td>
      <td class="gt_row gt_right">38.62</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-05-31</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">78</td>
      <td class="gt_row gt_right">22865</td>
      <td class="gt_row gt_right">38.75</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-01</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">125</td>
      <td class="gt_row gt_right">22990</td>
      <td class="gt_row gt_right">38.96</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-02</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">125</td>
      <td class="gt_row gt_right">23115</td>
      <td class="gt_row gt_right">39.17</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-03</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">125</td>
      <td class="gt_row gt_right">23240</td>
      <td class="gt_row gt_right">39.38</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-04</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">136</td>
      <td class="gt_row gt_right">23376</td>
      <td class="gt_row gt_right">39.62</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-05</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">103</td>
      <td class="gt_row gt_right">23479</td>
      <td class="gt_row gt_right">39.79</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-06</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">77</td>
      <td class="gt_row gt_right">23556</td>
      <td class="gt_row gt_right">39.92</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-07</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">64</td>
      <td class="gt_row gt_right">23620</td>
      <td class="gt_row gt_right">40.03</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-08</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">120</td>
      <td class="gt_row gt_right">23740</td>
      <td class="gt_row gt_right">40.23</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-09</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">101</td>
      <td class="gt_row gt_right">23841</td>
      <td class="gt_row gt_right">40.40</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-10</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">91</td>
      <td class="gt_row gt_right">23932</td>
      <td class="gt_row gt_right">40.56</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-11</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">91</td>
      <td class="gt_row gt_right">24023</td>
      <td class="gt_row gt_right">40.71</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-12</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">76</td>
      <td class="gt_row gt_right">24099</td>
      <td class="gt_row gt_right">40.84</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-13</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">86</td>
      <td class="gt_row gt_right">24185</td>
      <td class="gt_row gt_right">40.99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-14</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">75</td>
      <td class="gt_row gt_right">24260</td>
      <td class="gt_row gt_right">41.11</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-15</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">102</td>
      <td class="gt_row gt_right">24362</td>
      <td class="gt_row gt_right">41.29</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-16</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">104</td>
      <td class="gt_row gt_right">24466</td>
      <td class="gt_row gt_right">41.46</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-17</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">80</td>
      <td class="gt_row gt_right">24546</td>
      <td class="gt_row gt_right">41.60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-18</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">83</td>
      <td class="gt_row gt_right">24629</td>
      <td class="gt_row gt_right">41.74</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-19</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">51</td>
      <td class="gt_row gt_right">24680</td>
      <td class="gt_row gt_right">41.83</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-20</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">48</td>
      <td class="gt_row gt_right">24728</td>
      <td class="gt_row gt_right">41.91</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-21</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">37</td>
      <td class="gt_row gt_right">24765</td>
      <td class="gt_row gt_right">41.97</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-22</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">67</td>
      <td class="gt_row gt_right">24832</td>
      <td class="gt_row gt_right">42.08</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-23</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">53</td>
      <td class="gt_row gt_right">24885</td>
      <td class="gt_row gt_right">42.17</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-24</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">54</td>
      <td class="gt_row gt_right">24939</td>
      <td class="gt_row gt_right">42.26</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-25</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">56</td>
      <td class="gt_row gt_right">24995</td>
      <td class="gt_row gt_right">42.36</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-26</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">69</td>
      <td class="gt_row gt_right">25064</td>
      <td class="gt_row gt_right">42.48</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-27</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">44</td>
      <td class="gt_row gt_right">25108</td>
      <td class="gt_row gt_right">42.55</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-28</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">25</td>
      <td class="gt_row gt_right">25133</td>
      <td class="gt_row gt_right">42.59</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-29</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">57</td>
      <td class="gt_row gt_right">25190</td>
      <td class="gt_row gt_right">42.69</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-06-30</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">51</td>
      <td class="gt_row gt_right">25241</td>
      <td class="gt_row gt_right">42.78</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-01</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">49</td>
      <td class="gt_row gt_right">25290</td>
      <td class="gt_row gt_right">42.86</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-02</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">55</td>
      <td class="gt_row gt_right">25345</td>
      <td class="gt_row gt_right">42.95</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-03</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">41</td>
      <td class="gt_row gt_right">25386</td>
      <td class="gt_row gt_right">43.02</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-04</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">23</td>
      <td class="gt_row gt_right">25409</td>
      <td class="gt_row gt_right">43.06</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-05</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">36</td>
      <td class="gt_row gt_right">25445</td>
      <td class="gt_row gt_right">43.12</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-06</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">53</td>
      <td class="gt_row gt_right">25498</td>
      <td class="gt_row gt_right">43.21</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-07</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">60</td>
      <td class="gt_row gt_right">25558</td>
      <td class="gt_row gt_right">43.31</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-08</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">66</td>
      <td class="gt_row gt_right">25624</td>
      <td class="gt_row gt_right">43.42</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-09</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">121</td>
      <td class="gt_row gt_right">25745</td>
      <td class="gt_row gt_right">43.63</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-10</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">44</td>
      <td class="gt_row gt_right">25789</td>
      <td class="gt_row gt_right">43.70</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-11</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">40</td>
      <td class="gt_row gt_right">25829</td>
      <td class="gt_row gt_right">43.77</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-12</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">38</td>
      <td class="gt_row gt_right">25867</td>
      <td class="gt_row gt_right">43.84</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-13</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">79</td>
      <td class="gt_row gt_right">25946</td>
      <td class="gt_row gt_right">43.97</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-14</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">72</td>
      <td class="gt_row gt_right">26018</td>
      <td class="gt_row gt_right">44.09</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-15</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">106</td>
      <td class="gt_row gt_right">26124</td>
      <td class="gt_row gt_right">44.27</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-16</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">65</td>
      <td class="gt_row gt_right">26189</td>
      <td class="gt_row gt_right">44.38</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-17</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">69</td>
      <td class="gt_row gt_right">26258</td>
      <td class="gt_row gt_right">44.50</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-18</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">51</td>
      <td class="gt_row gt_right">26309</td>
      <td class="gt_row gt_right">44.59</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-19</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">40</td>
      <td class="gt_row gt_right">26349</td>
      <td class="gt_row gt_right">44.65</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-20</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">83</td>
      <td class="gt_row gt_right">26432</td>
      <td class="gt_row gt_right">44.79</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-21</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">80</td>
      <td class="gt_row gt_right">26512</td>
      <td class="gt_row gt_right">44.93</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-22</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">78</td>
      <td class="gt_row gt_right">26590</td>
      <td class="gt_row gt_right">45.06</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-23</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">89</td>
      <td class="gt_row gt_right">26679</td>
      <td class="gt_row gt_right">45.21</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-24</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">78</td>
      <td class="gt_row gt_right">26757</td>
      <td class="gt_row gt_right">45.35</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-25</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">67</td>
      <td class="gt_row gt_right">26824</td>
      <td class="gt_row gt_right">45.46</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-26</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">65</td>
      <td class="gt_row gt_right">26889</td>
      <td class="gt_row gt_right">45.57</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-27</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">77</td>
      <td class="gt_row gt_right">26966</td>
      <td class="gt_row gt_right">45.70</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-28</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">89</td>
      <td class="gt_row gt_right">27055</td>
      <td class="gt_row gt_right">45.85</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-29</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">91</td>
      <td class="gt_row gt_right">27146</td>
      <td class="gt_row gt_right">46.00</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-30</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">80</td>
      <td class="gt_row gt_right">27226</td>
      <td class="gt_row gt_right">46.14</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-07-31</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">81</td>
      <td class="gt_row gt_right">27307</td>
      <td class="gt_row gt_right">46.28</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-01</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">54</td>
      <td class="gt_row gt_right">27361</td>
      <td class="gt_row gt_right">46.37</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-02</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">42</td>
      <td class="gt_row gt_right">27403</td>
      <td class="gt_row gt_right">46.44</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-03</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">90</td>
      <td class="gt_row gt_right">27493</td>
      <td class="gt_row gt_right">46.59</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-04</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">98</td>
      <td class="gt_row gt_right">27591</td>
      <td class="gt_row gt_right">46.76</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-05</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">95</td>
      <td class="gt_row gt_right">27686</td>
      <td class="gt_row gt_right">46.92</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-06</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">114</td>
      <td class="gt_row gt_right">27800</td>
      <td class="gt_row gt_right">47.11</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-07</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">105</td>
      <td class="gt_row gt_right">27905</td>
      <td class="gt_row gt_right">47.29</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-08</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">74</td>
      <td class="gt_row gt_right">27979</td>
      <td class="gt_row gt_right">47.42</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-09</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">76</td>
      <td class="gt_row gt_right">28055</td>
      <td class="gt_row gt_right">47.54</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-10</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">158</td>
      <td class="gt_row gt_right">28213</td>
      <td class="gt_row gt_right">47.81</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-11</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">164</td>
      <td class="gt_row gt_right">28377</td>
      <td class="gt_row gt_right">48.09</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-12</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">133</td>
      <td class="gt_row gt_right">28510</td>
      <td class="gt_row gt_right">48.32</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-13</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">117</td>
      <td class="gt_row gt_right">28627</td>
      <td class="gt_row gt_right">48.51</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-14</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">172</td>
      <td class="gt_row gt_right">28799</td>
      <td class="gt_row gt_right">48.81</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-15</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">96</td>
      <td class="gt_row gt_right">28895</td>
      <td class="gt_row gt_right">48.97</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-16</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">73</td>
      <td class="gt_row gt_right">28968</td>
      <td class="gt_row gt_right">49.09</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-17</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">138</td>
      <td class="gt_row gt_right">29106</td>
      <td class="gt_row gt_right">49.33</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-18</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">110</td>
      <td class="gt_row gt_right">29216</td>
      <td class="gt_row gt_right">49.51</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-19</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">118</td>
      <td class="gt_row gt_right">29334</td>
      <td class="gt_row gt_right">49.71</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-20</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">163</td>
      <td class="gt_row gt_right">29497</td>
      <td class="gt_row gt_right">49.99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-21</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">105</td>
      <td class="gt_row gt_right">29602</td>
      <td class="gt_row gt_right">50.17</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-22</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">76</td>
      <td class="gt_row gt_right">29678</td>
      <td class="gt_row gt_right">50.30</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-23</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">82</td>
      <td class="gt_row gt_right">29760</td>
      <td class="gt_row gt_right">50.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-24</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">130</td>
      <td class="gt_row gt_right">29890</td>
      <td class="gt_row gt_right">50.65</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-25</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">143</td>
      <td class="gt_row gt_right">30033</td>
      <td class="gt_row gt_right">50.90</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-26</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">125</td>
      <td class="gt_row gt_right">30158</td>
      <td class="gt_row gt_right">51.11</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-27</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">144</td>
      <td class="gt_row gt_right">30302</td>
      <td class="gt_row gt_right">51.35</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-28</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">157</td>
      <td class="gt_row gt_right">30459</td>
      <td class="gt_row gt_right">51.62</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-29</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">114</td>
      <td class="gt_row gt_right">30573</td>
      <td class="gt_row gt_right">51.81</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-30</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">94</td>
      <td class="gt_row gt_right">30667</td>
      <td class="gt_row gt_right">51.97</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-08-31</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">127</td>
      <td class="gt_row gt_right">30794</td>
      <td class="gt_row gt_right">52.19</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-01</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">260</td>
      <td class="gt_row gt_right">31054</td>
      <td class="gt_row gt_right">52.63</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-02</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">336</td>
      <td class="gt_row gt_right">31390</td>
      <td class="gt_row gt_right">53.20</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-03</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">366</td>
      <td class="gt_row gt_right">31756</td>
      <td class="gt_row gt_right">53.82</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-04</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">422</td>
      <td class="gt_row gt_right">32178</td>
      <td class="gt_row gt_right">54.53</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-05</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">314</td>
      <td class="gt_row gt_right">32492</td>
      <td class="gt_row gt_right">55.06</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-06</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">318</td>
      <td class="gt_row gt_right">32810</td>
      <td class="gt_row gt_right">55.60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-07</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">486</td>
      <td class="gt_row gt_right">33296</td>
      <td class="gt_row gt_right">56.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-08</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">425</td>
      <td class="gt_row gt_right">33721</td>
      <td class="gt_row gt_right">57.15</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-09</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">388</td>
      <td class="gt_row gt_right">34109</td>
      <td class="gt_row gt_right">57.80</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-10</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">385</td>
      <td class="gt_row gt_right">34494</td>
      <td class="gt_row gt_right">58.46</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-11</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">351</td>
      <td class="gt_row gt_right">34845</td>
      <td class="gt_row gt_right">59.05</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-12</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">291</td>
      <td class="gt_row gt_right">35136</td>
      <td class="gt_row gt_right">59.54</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-13</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">213</td>
      <td class="gt_row gt_right">35349</td>
      <td class="gt_row gt_right">59.91</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-14</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">319</td>
      <td class="gt_row gt_right">35668</td>
      <td class="gt_row gt_right">60.45</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-15</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">362</td>
      <td class="gt_row gt_right">36030</td>
      <td class="gt_row gt_right">61.06</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-16</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">450</td>
      <td class="gt_row gt_right">36480</td>
      <td class="gt_row gt_right">61.82</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-17</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">508</td>
      <td class="gt_row gt_right">36988</td>
      <td class="gt_row gt_right">62.68</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-18</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">584</td>
      <td class="gt_row gt_right">37572</td>
      <td class="gt_row gt_right">63.67</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-19</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">583</td>
      <td class="gt_row gt_right">38155</td>
      <td class="gt_row gt_right">64.66</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-20</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">591</td>
      <td class="gt_row gt_right">38746</td>
      <td class="gt_row gt_right">65.66</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-21</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">644</td>
      <td class="gt_row gt_right">39390</td>
      <td class="gt_row gt_right">66.75</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-22</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">611</td>
      <td class="gt_row gt_right">40001</td>
      <td class="gt_row gt_right">67.79</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-23</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">695</td>
      <td class="gt_row gt_right">40696</td>
      <td class="gt_row gt_right">68.97</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-24</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">695</td>
      <td class="gt_row gt_right">41391</td>
      <td class="gt_row gt_right">70.15</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-25</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">621</td>
      <td class="gt_row gt_right">42012</td>
      <td class="gt_row gt_right">71.20</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-26</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">581</td>
      <td class="gt_row gt_right">42593</td>
      <td class="gt_row gt_right">72.18</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-27</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">521</td>
      <td class="gt_row gt_right">43114</td>
      <td class="gt_row gt_right">73.07</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-28</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">719</td>
      <td class="gt_row gt_right">43833</td>
      <td class="gt_row gt_right">74.28</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-29</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">747</td>
      <td class="gt_row gt_right">44580</td>
      <td class="gt_row gt_right">75.55</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-09-30</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">971</td>
      <td class="gt_row gt_right">45551</td>
      <td class="gt_row gt_right">77.20</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-10-01</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">897</td>
      <td class="gt_row gt_right">46448</td>
      <td class="gt_row gt_right">78.72</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-10-02</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">929</td>
      <td class="gt_row gt_right">47377</td>
      <td class="gt_row gt_right">80.29</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-10-03</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">756</td>
      <td class="gt_row gt_right">48133</td>
      <td class="gt_row gt_right">81.57</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-10-04</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">685</td>
      <td class="gt_row gt_right">48818</td>
      <td class="gt_row gt_right">82.73</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-10-05</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">1167</td>
      <td class="gt_row gt_right">49985</td>
      <td class="gt_row gt_right">84.71</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-10-06</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">1200</td>
      <td class="gt_row gt_right">51185</td>
      <td class="gt_row gt_right">86.74</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-10-07</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">1176</td>
      <td class="gt_row gt_right">52361</td>
      <td class="gt_row gt_right">88.74</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-10-08</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">1172</td>
      <td class="gt_row gt_right">53533</td>
      <td class="gt_row gt_right">90.72</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-10-09</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">980</td>
      <td class="gt_row gt_right">54513</td>
      <td class="gt_row gt_right">92.38</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-10-10</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">405</td>
      <td class="gt_row gt_right">54918</td>
      <td class="gt_row gt_right">93.07</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-10-11</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">268</td>
      <td class="gt_row gt_right">55186</td>
      <td class="gt_row gt_right">93.52</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-10-12</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">11</td>
      <td class="gt_row gt_right">55197</td>
      <td class="gt_row gt_right">93.54</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">06552b80</td>
      <td class="gt_row gt_left">2020-10-13</td>
      <td class="gt_row gt_right">5900757</td>
      <td class="gt_row gt_right">-55197</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">0</td>
    </tr>
    <tr class="gt_group_heading_row">
      <td colspan="6" class="gt_group_heading">South West</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-02-26</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.001786</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-02-27</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">0.003572</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-02-28</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">0.003572</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-02-29</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">0.003572</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-01</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">3</td>
      <td class="gt_row gt_right">5</td>
      <td class="gt_row gt_right">0.008929</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-02</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">8</td>
      <td class="gt_row gt_right">13</td>
      <td class="gt_row gt_right">0.02322</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-03</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">10</td>
      <td class="gt_row gt_right">23</td>
      <td class="gt_row gt_right">0.04107</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-04</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">29</td>
      <td class="gt_row gt_right">0.05179</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-05</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">4</td>
      <td class="gt_row gt_right">33</td>
      <td class="gt_row gt_right">0.05893</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-06</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">11</td>
      <td class="gt_row gt_right">44</td>
      <td class="gt_row gt_right">0.07858</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-07</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">4</td>
      <td class="gt_row gt_right">48</td>
      <td class="gt_row gt_right">0.08572</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-08</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">3</td>
      <td class="gt_row gt_right">51</td>
      <td class="gt_row gt_right">0.09108</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-09</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">5</td>
      <td class="gt_row gt_right">56</td>
      <td class="gt_row gt_right">0.1000</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-10</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">11</td>
      <td class="gt_row gt_right">67</td>
      <td class="gt_row gt_right">0.1196</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-11</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">14</td>
      <td class="gt_row gt_right">81</td>
      <td class="gt_row gt_right">0.1446</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-12</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">18</td>
      <td class="gt_row gt_right">99</td>
      <td class="gt_row gt_right">0.1768</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-13</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">30</td>
      <td class="gt_row gt_right">129</td>
      <td class="gt_row gt_right">0.2304</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-14</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">10</td>
      <td class="gt_row gt_right">139</td>
      <td class="gt_row gt_right">0.2482</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-15</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">17</td>
      <td class="gt_row gt_right">156</td>
      <td class="gt_row gt_right">0.2786</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-16</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">18</td>
      <td class="gt_row gt_right">174</td>
      <td class="gt_row gt_right">0.3107</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-17</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">21</td>
      <td class="gt_row gt_right">195</td>
      <td class="gt_row gt_right">0.3482</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-18</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">43</td>
      <td class="gt_row gt_right">238</td>
      <td class="gt_row gt_right">0.4250</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-19</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">21</td>
      <td class="gt_row gt_right">259</td>
      <td class="gt_row gt_right">0.4625</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-20</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">43</td>
      <td class="gt_row gt_right">302</td>
      <td class="gt_row gt_right">0.5393</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-21</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">42</td>
      <td class="gt_row gt_right">344</td>
      <td class="gt_row gt_right">0.6143</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-22</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">44</td>
      <td class="gt_row gt_right">388</td>
      <td class="gt_row gt_right">0.6929</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-23</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">62</td>
      <td class="gt_row gt_right">450</td>
      <td class="gt_row gt_right">0.8036</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-24</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">68</td>
      <td class="gt_row gt_right">518</td>
      <td class="gt_row gt_right">0.9250</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-25</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">104</td>
      <td class="gt_row gt_right">622</td>
      <td class="gt_row gt_right">1.111</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-26</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">118</td>
      <td class="gt_row gt_right">740</td>
      <td class="gt_row gt_right">1.321</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-27</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">128</td>
      <td class="gt_row gt_right">868</td>
      <td class="gt_row gt_right">1.550</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-28</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">102</td>
      <td class="gt_row gt_right">970</td>
      <td class="gt_row gt_right">1.732</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-29</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">101</td>
      <td class="gt_row gt_right">1071</td>
      <td class="gt_row gt_right">1.913</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-30</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">163</td>
      <td class="gt_row gt_right">1234</td>
      <td class="gt_row gt_right">2.204</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-03-31</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">141</td>
      <td class="gt_row gt_right">1375</td>
      <td class="gt_row gt_right">2.455</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-01</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">183</td>
      <td class="gt_row gt_right">1558</td>
      <td class="gt_row gt_right">2.782</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-02</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">183</td>
      <td class="gt_row gt_right">1741</td>
      <td class="gt_row gt_right">3.109</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-03</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">187</td>
      <td class="gt_row gt_right">1928</td>
      <td class="gt_row gt_right">3.443</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-04</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">174</td>
      <td class="gt_row gt_right">2102</td>
      <td class="gt_row gt_right">3.754</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-05</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">153</td>
      <td class="gt_row gt_right">2255</td>
      <td class="gt_row gt_right">4.027</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-06</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">186</td>
      <td class="gt_row gt_right">2441</td>
      <td class="gt_row gt_right">4.359</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-07</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">212</td>
      <td class="gt_row gt_right">2653</td>
      <td class="gt_row gt_right">4.738</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-08</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">236</td>
      <td class="gt_row gt_right">2889</td>
      <td class="gt_row gt_right">5.159</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-09</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">246</td>
      <td class="gt_row gt_right">3135</td>
      <td class="gt_row gt_right">5.598</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-10</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">234</td>
      <td class="gt_row gt_right">3369</td>
      <td class="gt_row gt_right">6.016</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-11</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">173</td>
      <td class="gt_row gt_right">3542</td>
      <td class="gt_row gt_right">6.325</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-12</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">178</td>
      <td class="gt_row gt_right">3720</td>
      <td class="gt_row gt_right">6.643</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-13</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">194</td>
      <td class="gt_row gt_right">3914</td>
      <td class="gt_row gt_right">6.990</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-14</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">185</td>
      <td class="gt_row gt_right">4099</td>
      <td class="gt_row gt_right">7.320</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-15</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">211</td>
      <td class="gt_row gt_right">4310</td>
      <td class="gt_row gt_right">7.697</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-16</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">262</td>
      <td class="gt_row gt_right">4572</td>
      <td class="gt_row gt_right">8.165</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-17</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">240</td>
      <td class="gt_row gt_right">4812</td>
      <td class="gt_row gt_right">8.593</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-18</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">168</td>
      <td class="gt_row gt_right">4980</td>
      <td class="gt_row gt_right">8.893</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-19</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">200</td>
      <td class="gt_row gt_right">5180</td>
      <td class="gt_row gt_right">9.250</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-20</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">183</td>
      <td class="gt_row gt_right">5363</td>
      <td class="gt_row gt_right">9.577</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-21</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">227</td>
      <td class="gt_row gt_right">5590</td>
      <td class="gt_row gt_right">9.983</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-22</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">286</td>
      <td class="gt_row gt_right">5876</td>
      <td class="gt_row gt_right">10.49</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-23</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">281</td>
      <td class="gt_row gt_right">6157</td>
      <td class="gt_row gt_right">11.00</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-24</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">250</td>
      <td class="gt_row gt_right">6407</td>
      <td class="gt_row gt_right">11.44</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-25</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">200</td>
      <td class="gt_row gt_right">6607</td>
      <td class="gt_row gt_right">11.80</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-26</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">254</td>
      <td class="gt_row gt_right">6861</td>
      <td class="gt_row gt_right">12.25</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-27</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">321</td>
      <td class="gt_row gt_right">7182</td>
      <td class="gt_row gt_right">12.83</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-28</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">275</td>
      <td class="gt_row gt_right">7457</td>
      <td class="gt_row gt_right">13.32</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-29</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">348</td>
      <td class="gt_row gt_right">7805</td>
      <td class="gt_row gt_right">13.94</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-04-30</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">244</td>
      <td class="gt_row gt_right">8049</td>
      <td class="gt_row gt_right">14.37</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-01</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">263</td>
      <td class="gt_row gt_right">8312</td>
      <td class="gt_row gt_right">14.84</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-02</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">208</td>
      <td class="gt_row gt_right">8520</td>
      <td class="gt_row gt_right">15.22</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-03</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">176</td>
      <td class="gt_row gt_right">8696</td>
      <td class="gt_row gt_right">15.53</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-04</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">165</td>
      <td class="gt_row gt_right">8861</td>
      <td class="gt_row gt_right">15.82</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-05</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">173</td>
      <td class="gt_row gt_right">9034</td>
      <td class="gt_row gt_right">16.13</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-06</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">201</td>
      <td class="gt_row gt_right">9235</td>
      <td class="gt_row gt_right">16.49</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-07</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">180</td>
      <td class="gt_row gt_right">9415</td>
      <td class="gt_row gt_right">16.81</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-08</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">178</td>
      <td class="gt_row gt_right">9593</td>
      <td class="gt_row gt_right">17.13</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-09</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">106</td>
      <td class="gt_row gt_right">9699</td>
      <td class="gt_row gt_right">17.32</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-10</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">135</td>
      <td class="gt_row gt_right">9834</td>
      <td class="gt_row gt_right">17.56</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-11</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">255</td>
      <td class="gt_row gt_right">10089</td>
      <td class="gt_row gt_right">18.02</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-12</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">177</td>
      <td class="gt_row gt_right">10266</td>
      <td class="gt_row gt_right">18.33</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-13</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">176</td>
      <td class="gt_row gt_right">10442</td>
      <td class="gt_row gt_right">18.65</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-14</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">92</td>
      <td class="gt_row gt_right">10534</td>
      <td class="gt_row gt_right">18.81</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-15</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">103</td>
      <td class="gt_row gt_right">10637</td>
      <td class="gt_row gt_right">19.00</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-16</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">90</td>
      <td class="gt_row gt_right">10727</td>
      <td class="gt_row gt_right">19.16</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-17</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">97</td>
      <td class="gt_row gt_right">10824</td>
      <td class="gt_row gt_right">19.33</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-18</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">118</td>
      <td class="gt_row gt_right">10942</td>
      <td class="gt_row gt_right">19.54</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-19</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">108</td>
      <td class="gt_row gt_right">11050</td>
      <td class="gt_row gt_right">19.73</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-20</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">137</td>
      <td class="gt_row gt_right">11187</td>
      <td class="gt_row gt_right">19.98</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-21</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">87</td>
      <td class="gt_row gt_right">11274</td>
      <td class="gt_row gt_right">20.13</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-22</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">82</td>
      <td class="gt_row gt_right">11356</td>
      <td class="gt_row gt_right">20.28</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-23</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">41</td>
      <td class="gt_row gt_right">11397</td>
      <td class="gt_row gt_right">20.35</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-24</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">59</td>
      <td class="gt_row gt_right">11456</td>
      <td class="gt_row gt_right">20.46</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-25</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">61</td>
      <td class="gt_row gt_right">11517</td>
      <td class="gt_row gt_right">20.57</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-26</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">80</td>
      <td class="gt_row gt_right">11597</td>
      <td class="gt_row gt_right">20.71</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-27</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">104</td>
      <td class="gt_row gt_right">11701</td>
      <td class="gt_row gt_right">20.90</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-28</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">115</td>
      <td class="gt_row gt_right">11816</td>
      <td class="gt_row gt_right">21.10</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-29</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">73</td>
      <td class="gt_row gt_right">11889</td>
      <td class="gt_row gt_right">21.23</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-30</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">51</td>
      <td class="gt_row gt_right">11940</td>
      <td class="gt_row gt_right">21.32</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-05-31</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">62</td>
      <td class="gt_row gt_right">12002</td>
      <td class="gt_row gt_right">21.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-01</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">51</td>
      <td class="gt_row gt_right">12053</td>
      <td class="gt_row gt_right">21.52</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-02</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">52</td>
      <td class="gt_row gt_right">12105</td>
      <td class="gt_row gt_right">21.62</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-03</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">69</td>
      <td class="gt_row gt_right">12174</td>
      <td class="gt_row gt_right">21.74</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-04</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">65</td>
      <td class="gt_row gt_right">12239</td>
      <td class="gt_row gt_right">21.86</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-05</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">52</td>
      <td class="gt_row gt_right">12291</td>
      <td class="gt_row gt_right">21.95</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-06</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">23</td>
      <td class="gt_row gt_right">12314</td>
      <td class="gt_row gt_right">21.99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-07</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">18</td>
      <td class="gt_row gt_right">12332</td>
      <td class="gt_row gt_right">22.02</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-08</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">23</td>
      <td class="gt_row gt_right">12355</td>
      <td class="gt_row gt_right">22.06</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-09</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">25</td>
      <td class="gt_row gt_right">12380</td>
      <td class="gt_row gt_right">22.11</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-10</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">31</td>
      <td class="gt_row gt_right">12411</td>
      <td class="gt_row gt_right">22.16</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-11</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">25</td>
      <td class="gt_row gt_right">12436</td>
      <td class="gt_row gt_right">22.21</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-12</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">19</td>
      <td class="gt_row gt_right">12455</td>
      <td class="gt_row gt_right">22.24</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-13</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">14</td>
      <td class="gt_row gt_right">12469</td>
      <td class="gt_row gt_right">22.27</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-14</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">9</td>
      <td class="gt_row gt_right">12478</td>
      <td class="gt_row gt_right">22.28</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-15</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">20</td>
      <td class="gt_row gt_right">12498</td>
      <td class="gt_row gt_right">22.32</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-16</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">15</td>
      <td class="gt_row gt_right">12513</td>
      <td class="gt_row gt_right">22.35</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-17</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">16</td>
      <td class="gt_row gt_right">12529</td>
      <td class="gt_row gt_right">22.37</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-18</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">13</td>
      <td class="gt_row gt_right">12542</td>
      <td class="gt_row gt_right">22.40</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-19</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">24</td>
      <td class="gt_row gt_right">12566</td>
      <td class="gt_row gt_right">22.44</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-20</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">8</td>
      <td class="gt_row gt_right">12574</td>
      <td class="gt_row gt_right">22.45</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-21</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">8</td>
      <td class="gt_row gt_right">12582</td>
      <td class="gt_row gt_right">22.47</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-22</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">19</td>
      <td class="gt_row gt_right">12601</td>
      <td class="gt_row gt_right">22.50</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-23</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">15</td>
      <td class="gt_row gt_right">12616</td>
      <td class="gt_row gt_right">22.53</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-24</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">16</td>
      <td class="gt_row gt_right">12632</td>
      <td class="gt_row gt_right">22.56</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-25</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">8</td>
      <td class="gt_row gt_right">12640</td>
      <td class="gt_row gt_right">22.57</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-26</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">12</td>
      <td class="gt_row gt_right">12652</td>
      <td class="gt_row gt_right">22.59</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-27</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">8</td>
      <td class="gt_row gt_right">12660</td>
      <td class="gt_row gt_right">22.61</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-28</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">11</td>
      <td class="gt_row gt_right">12671</td>
      <td class="gt_row gt_right">22.63</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-29</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">16</td>
      <td class="gt_row gt_right">12687</td>
      <td class="gt_row gt_right">22.66</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-06-30</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">18</td>
      <td class="gt_row gt_right">12705</td>
      <td class="gt_row gt_right">22.69</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-01</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">23</td>
      <td class="gt_row gt_right">12728</td>
      <td class="gt_row gt_right">22.73</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-02</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">11</td>
      <td class="gt_row gt_right">12739</td>
      <td class="gt_row gt_right">22.75</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-03</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">8</td>
      <td class="gt_row gt_right">12747</td>
      <td class="gt_row gt_right">22.76</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-04</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">10</td>
      <td class="gt_row gt_right">12757</td>
      <td class="gt_row gt_right">22.78</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-05</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">17</td>
      <td class="gt_row gt_right">12774</td>
      <td class="gt_row gt_right">22.81</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-06</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">25</td>
      <td class="gt_row gt_right">12799</td>
      <td class="gt_row gt_right">22.86</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-07</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">17</td>
      <td class="gt_row gt_right">12816</td>
      <td class="gt_row gt_right">22.89</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-08</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">15</td>
      <td class="gt_row gt_right">12831</td>
      <td class="gt_row gt_right">22.91</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-09</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">17</td>
      <td class="gt_row gt_right">12848</td>
      <td class="gt_row gt_right">22.94</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-10</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">13</td>
      <td class="gt_row gt_right">12861</td>
      <td class="gt_row gt_right">22.97</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-11</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">12867</td>
      <td class="gt_row gt_right">22.98</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-12</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">12873</td>
      <td class="gt_row gt_right">22.99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-13</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">17</td>
      <td class="gt_row gt_right">12890</td>
      <td class="gt_row gt_right">23.02</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-14</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">19</td>
      <td class="gt_row gt_right">12909</td>
      <td class="gt_row gt_right">23.05</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-15</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">17</td>
      <td class="gt_row gt_right">12926</td>
      <td class="gt_row gt_right">23.08</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-16</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">28</td>
      <td class="gt_row gt_right">12954</td>
      <td class="gt_row gt_right">23.13</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-17</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">20</td>
      <td class="gt_row gt_right">12974</td>
      <td class="gt_row gt_right">23.17</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-18</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">12</td>
      <td class="gt_row gt_right">12986</td>
      <td class="gt_row gt_right">23.19</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-19</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">9</td>
      <td class="gt_row gt_right">12995</td>
      <td class="gt_row gt_right">23.21</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-20</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">23</td>
      <td class="gt_row gt_right">13018</td>
      <td class="gt_row gt_right">23.25</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-21</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">35</td>
      <td class="gt_row gt_right">13053</td>
      <td class="gt_row gt_right">23.31</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-22</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">30</td>
      <td class="gt_row gt_right">13083</td>
      <td class="gt_row gt_right">23.36</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-23</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">39</td>
      <td class="gt_row gt_right">13122</td>
      <td class="gt_row gt_right">23.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-24</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">35</td>
      <td class="gt_row gt_right">13157</td>
      <td class="gt_row gt_right">23.50</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-25</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">16</td>
      <td class="gt_row gt_right">13173</td>
      <td class="gt_row gt_right">23.52</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-26</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">20</td>
      <td class="gt_row gt_right">13193</td>
      <td class="gt_row gt_right">23.56</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-27</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">40</td>
      <td class="gt_row gt_right">13233</td>
      <td class="gt_row gt_right">23.63</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-28</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">35</td>
      <td class="gt_row gt_right">13268</td>
      <td class="gt_row gt_right">23.69</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-29</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">53</td>
      <td class="gt_row gt_right">13321</td>
      <td class="gt_row gt_right">23.79</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-30</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">63</td>
      <td class="gt_row gt_right">13384</td>
      <td class="gt_row gt_right">23.90</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-07-31</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">29</td>
      <td class="gt_row gt_right">13413</td>
      <td class="gt_row gt_right">23.95</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-01</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">9</td>
      <td class="gt_row gt_right">13422</td>
      <td class="gt_row gt_right">23.97</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-02</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">10</td>
      <td class="gt_row gt_right">13432</td>
      <td class="gt_row gt_right">23.99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-03</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">43</td>
      <td class="gt_row gt_right">13475</td>
      <td class="gt_row gt_right">24.06</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-04</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">54</td>
      <td class="gt_row gt_right">13529</td>
      <td class="gt_row gt_right">24.16</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-05</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">40</td>
      <td class="gt_row gt_right">13569</td>
      <td class="gt_row gt_right">24.23</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-06</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">45</td>
      <td class="gt_row gt_right">13614</td>
      <td class="gt_row gt_right">24.31</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-07</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">41</td>
      <td class="gt_row gt_right">13655</td>
      <td class="gt_row gt_right">24.39</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-08</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">25</td>
      <td class="gt_row gt_right">13680</td>
      <td class="gt_row gt_right">24.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-09</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">15</td>
      <td class="gt_row gt_right">13695</td>
      <td class="gt_row gt_right">24.46</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-10</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">41</td>
      <td class="gt_row gt_right">13736</td>
      <td class="gt_row gt_right">24.53</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-11</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">41</td>
      <td class="gt_row gt_right">13777</td>
      <td class="gt_row gt_right">24.60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-12</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">42</td>
      <td class="gt_row gt_right">13819</td>
      <td class="gt_row gt_right">24.68</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-13</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">53</td>
      <td class="gt_row gt_right">13872</td>
      <td class="gt_row gt_right">24.77</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-14</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">55</td>
      <td class="gt_row gt_right">13927</td>
      <td class="gt_row gt_right">24.87</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-15</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">25</td>
      <td class="gt_row gt_right">13952</td>
      <td class="gt_row gt_right">24.92</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-16</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">37</td>
      <td class="gt_row gt_right">13989</td>
      <td class="gt_row gt_right">24.98</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-17</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">48</td>
      <td class="gt_row gt_right">14037</td>
      <td class="gt_row gt_right">25.07</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-18</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">58</td>
      <td class="gt_row gt_right">14095</td>
      <td class="gt_row gt_right">25.17</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-19</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">69</td>
      <td class="gt_row gt_right">14164</td>
      <td class="gt_row gt_right">25.29</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-20</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">85</td>
      <td class="gt_row gt_right">14249</td>
      <td class="gt_row gt_right">25.45</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-21</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">64</td>
      <td class="gt_row gt_right">14313</td>
      <td class="gt_row gt_right">25.56</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-22</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">39</td>
      <td class="gt_row gt_right">14352</td>
      <td class="gt_row gt_right">25.63</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-23</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">41</td>
      <td class="gt_row gt_right">14393</td>
      <td class="gt_row gt_right">25.70</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-24</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">49</td>
      <td class="gt_row gt_right">14442</td>
      <td class="gt_row gt_right">25.79</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-25</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">50</td>
      <td class="gt_row gt_right">14492</td>
      <td class="gt_row gt_right">25.88</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-26</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">62</td>
      <td class="gt_row gt_right">14554</td>
      <td class="gt_row gt_right">25.99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-27</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">57</td>
      <td class="gt_row gt_right">14611</td>
      <td class="gt_row gt_right">26.09</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-28</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">72</td>
      <td class="gt_row gt_right">14683</td>
      <td class="gt_row gt_right">26.22</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-29</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">50</td>
      <td class="gt_row gt_right">14733</td>
      <td class="gt_row gt_right">26.31</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-30</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">34</td>
      <td class="gt_row gt_right">14767</td>
      <td class="gt_row gt_right">26.37</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-08-31</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">57</td>
      <td class="gt_row gt_right">14824</td>
      <td class="gt_row gt_right">26.47</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-01</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">101</td>
      <td class="gt_row gt_right">14925</td>
      <td class="gt_row gt_right">26.65</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-02</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">123</td>
      <td class="gt_row gt_right">15048</td>
      <td class="gt_row gt_right">26.87</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-03</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">151</td>
      <td class="gt_row gt_right">15199</td>
      <td class="gt_row gt_right">27.14</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-04</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">135</td>
      <td class="gt_row gt_right">15334</td>
      <td class="gt_row gt_right">27.38</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-05</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">84</td>
      <td class="gt_row gt_right">15418</td>
      <td class="gt_row gt_right">27.53</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-06</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">78</td>
      <td class="gt_row gt_right">15496</td>
      <td class="gt_row gt_right">27.67</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-07</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">105</td>
      <td class="gt_row gt_right">15601</td>
      <td class="gt_row gt_right">27.86</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-08</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">81</td>
      <td class="gt_row gt_right">15682</td>
      <td class="gt_row gt_right">28.00</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-09</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">75</td>
      <td class="gt_row gt_right">15757</td>
      <td class="gt_row gt_right">28.14</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-10</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">92</td>
      <td class="gt_row gt_right">15849</td>
      <td class="gt_row gt_right">28.30</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-11</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">98</td>
      <td class="gt_row gt_right">15947</td>
      <td class="gt_row gt_right">28.48</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-12</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">63</td>
      <td class="gt_row gt_right">16010</td>
      <td class="gt_row gt_right">28.59</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-13</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">58</td>
      <td class="gt_row gt_right">16068</td>
      <td class="gt_row gt_right">28.69</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-14</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">80</td>
      <td class="gt_row gt_right">16148</td>
      <td class="gt_row gt_right">28.84</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-15</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">89</td>
      <td class="gt_row gt_right">16237</td>
      <td class="gt_row gt_right">29.00</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-16</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">75</td>
      <td class="gt_row gt_right">16312</td>
      <td class="gt_row gt_right">29.13</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-17</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">114</td>
      <td class="gt_row gt_right">16426</td>
      <td class="gt_row gt_right">29.33</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-18</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">122</td>
      <td class="gt_row gt_right">16548</td>
      <td class="gt_row gt_right">29.55</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-19</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">130</td>
      <td class="gt_row gt_right">16678</td>
      <td class="gt_row gt_right">29.78</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-20</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">150</td>
      <td class="gt_row gt_right">16828</td>
      <td class="gt_row gt_right">30.05</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-21</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">153</td>
      <td class="gt_row gt_right">16981</td>
      <td class="gt_row gt_right">30.32</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-22</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">161</td>
      <td class="gt_row gt_right">17142</td>
      <td class="gt_row gt_right">30.61</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-23</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">183</td>
      <td class="gt_row gt_right">17325</td>
      <td class="gt_row gt_right">30.94</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-24</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">221</td>
      <td class="gt_row gt_right">17546</td>
      <td class="gt_row gt_right">31.33</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-25</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">228</td>
      <td class="gt_row gt_right">17774</td>
      <td class="gt_row gt_right">31.74</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-26</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">164</td>
      <td class="gt_row gt_right">17938</td>
      <td class="gt_row gt_right">32.03</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-27</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">143</td>
      <td class="gt_row gt_right">18081</td>
      <td class="gt_row gt_right">32.29</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-28</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">320</td>
      <td class="gt_row gt_right">18401</td>
      <td class="gt_row gt_right">32.86</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-29</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">292</td>
      <td class="gt_row gt_right">18693</td>
      <td class="gt_row gt_right">33.38</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-09-30</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">429</td>
      <td class="gt_row gt_right">19122</td>
      <td class="gt_row gt_right">34.15</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-10-01</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">431</td>
      <td class="gt_row gt_right">19553</td>
      <td class="gt_row gt_right">34.92</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-10-02</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">445</td>
      <td class="gt_row gt_right">19998</td>
      <td class="gt_row gt_right">35.71</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-10-03</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">390</td>
      <td class="gt_row gt_right">20388</td>
      <td class="gt_row gt_right">36.41</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-10-04</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">443</td>
      <td class="gt_row gt_right">20831</td>
      <td class="gt_row gt_right">37.20</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-10-05</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">613</td>
      <td class="gt_row gt_right">21444</td>
      <td class="gt_row gt_right">38.29</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-10-06</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">587</td>
      <td class="gt_row gt_right">22031</td>
      <td class="gt_row gt_right">39.34</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-10-07</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">709</td>
      <td class="gt_row gt_right">22740</td>
      <td class="gt_row gt_right">40.61</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-10-08</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">767</td>
      <td class="gt_row gt_right">23507</td>
      <td class="gt_row gt_right">41.98</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-10-09</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">505</td>
      <td class="gt_row gt_right">24012</td>
      <td class="gt_row gt_right">42.88</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-10-10</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">84</td>
      <td class="gt_row gt_right">24096</td>
      <td class="gt_row gt_right">43.03</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-10-11</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">62</td>
      <td class="gt_row gt_right">24158</td>
      <td class="gt_row gt_right">43.14</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-10-12</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">4</td>
      <td class="gt_row gt_right">24162</td>
      <td class="gt_row gt_right">43.15</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">0fe367ab</td>
      <td class="gt_row gt_left">2020-10-13</td>
      <td class="gt_row gt_right">5599735</td>
      <td class="gt_row gt_right">-24162</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">0</td>
    </tr>
    <tr class="gt_group_heading_row">
      <td colspan="6" class="gt_group_heading">East Midlands</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-02-21</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.002082</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-02-22</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.002082</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-02-23</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.002082</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-02-24</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.002082</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-02-25</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">0.004163</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-02-26</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">0.004163</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-02-27</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">0.004163</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-02-28</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">3</td>
      <td class="gt_row gt_right">0.006245</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-02-29</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">4</td>
      <td class="gt_row gt_right">0.008326</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-01</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">5</td>
      <td class="gt_row gt_right">0.01041</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-02</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">7</td>
      <td class="gt_row gt_right">0.01457</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-03</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">7</td>
      <td class="gt_row gt_right">14</td>
      <td class="gt_row gt_right">0.02914</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-04</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">16</td>
      <td class="gt_row gt_right">0.03330</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-05</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">3</td>
      <td class="gt_row gt_right">19</td>
      <td class="gt_row gt_right">0.03955</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-06</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">7</td>
      <td class="gt_row gt_right">26</td>
      <td class="gt_row gt_right">0.05412</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-07</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">5</td>
      <td class="gt_row gt_right">31</td>
      <td class="gt_row gt_right">0.06453</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-08</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">32</td>
      <td class="gt_row gt_right">0.06661</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-09</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">10</td>
      <td class="gt_row gt_right">42</td>
      <td class="gt_row gt_right">0.08742</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-10</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">12</td>
      <td class="gt_row gt_right">54</td>
      <td class="gt_row gt_right">0.1124</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-11</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">25</td>
      <td class="gt_row gt_right">79</td>
      <td class="gt_row gt_right">0.1644</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-12</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">18</td>
      <td class="gt_row gt_right">97</td>
      <td class="gt_row gt_right">0.2019</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-13</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">22</td>
      <td class="gt_row gt_right">119</td>
      <td class="gt_row gt_right">0.2477</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-14</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">19</td>
      <td class="gt_row gt_right">138</td>
      <td class="gt_row gt_right">0.2873</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-15</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">26</td>
      <td class="gt_row gt_right">164</td>
      <td class="gt_row gt_right">0.3414</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-16</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">34</td>
      <td class="gt_row gt_right">198</td>
      <td class="gt_row gt_right">0.4121</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-17</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">33</td>
      <td class="gt_row gt_right">231</td>
      <td class="gt_row gt_right">0.4808</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-18</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">49</td>
      <td class="gt_row gt_right">280</td>
      <td class="gt_row gt_right">0.5828</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-19</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">66</td>
      <td class="gt_row gt_right">346</td>
      <td class="gt_row gt_right">0.7202</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-20</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">53</td>
      <td class="gt_row gt_right">399</td>
      <td class="gt_row gt_right">0.8305</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-21</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">69</td>
      <td class="gt_row gt_right">468</td>
      <td class="gt_row gt_right">0.9742</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-22</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">99</td>
      <td class="gt_row gt_right">567</td>
      <td class="gt_row gt_right">1.180</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-23</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">116</td>
      <td class="gt_row gt_right">683</td>
      <td class="gt_row gt_right">1.422</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-24</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">136</td>
      <td class="gt_row gt_right">819</td>
      <td class="gt_row gt_right">1.705</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-25</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">116</td>
      <td class="gt_row gt_right">935</td>
      <td class="gt_row gt_right">1.946</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-26</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">154</td>
      <td class="gt_row gt_right">1089</td>
      <td class="gt_row gt_right">2.267</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-27</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">137</td>
      <td class="gt_row gt_right">1226</td>
      <td class="gt_row gt_right">2.552</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-28</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">159</td>
      <td class="gt_row gt_right">1385</td>
      <td class="gt_row gt_right">2.883</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-29</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">129</td>
      <td class="gt_row gt_right">1514</td>
      <td class="gt_row gt_right">3.151</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-30</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">206</td>
      <td class="gt_row gt_right">1720</td>
      <td class="gt_row gt_right">3.580</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-03-31</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">277</td>
      <td class="gt_row gt_right">1997</td>
      <td class="gt_row gt_right">4.157</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-01</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">296</td>
      <td class="gt_row gt_right">2293</td>
      <td class="gt_row gt_right">4.773</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-02</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">213</td>
      <td class="gt_row gt_right">2506</td>
      <td class="gt_row gt_right">5.216</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-03</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">259</td>
      <td class="gt_row gt_right">2765</td>
      <td class="gt_row gt_right">5.755</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-04</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">189</td>
      <td class="gt_row gt_right">2954</td>
      <td class="gt_row gt_right">6.149</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-05</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">148</td>
      <td class="gt_row gt_right">3102</td>
      <td class="gt_row gt_right">6.457</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-06</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">215</td>
      <td class="gt_row gt_right">3317</td>
      <td class="gt_row gt_right">6.904</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-07</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">227</td>
      <td class="gt_row gt_right">3544</td>
      <td class="gt_row gt_right">7.377</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-08</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">188</td>
      <td class="gt_row gt_right">3732</td>
      <td class="gt_row gt_right">7.768</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-09</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">233</td>
      <td class="gt_row gt_right">3965</td>
      <td class="gt_row gt_right">8.253</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-10</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">172</td>
      <td class="gt_row gt_right">4137</td>
      <td class="gt_row gt_right">8.611</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-11</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">174</td>
      <td class="gt_row gt_right">4311</td>
      <td class="gt_row gt_right">8.973</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-12</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">214</td>
      <td class="gt_row gt_right">4525</td>
      <td class="gt_row gt_right">9.419</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-13</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">172</td>
      <td class="gt_row gt_right">4697</td>
      <td class="gt_row gt_right">9.777</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-14</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">210</td>
      <td class="gt_row gt_right">4907</td>
      <td class="gt_row gt_right">10.21</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-15</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">218</td>
      <td class="gt_row gt_right">5125</td>
      <td class="gt_row gt_right">10.67</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-16</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">267</td>
      <td class="gt_row gt_right">5392</td>
      <td class="gt_row gt_right">11.22</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-17</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">255</td>
      <td class="gt_row gt_right">5647</td>
      <td class="gt_row gt_right">11.75</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-18</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">195</td>
      <td class="gt_row gt_right">5842</td>
      <td class="gt_row gt_right">12.16</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-19</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">246</td>
      <td class="gt_row gt_right">6088</td>
      <td class="gt_row gt_right">12.67</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-20</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">187</td>
      <td class="gt_row gt_right">6275</td>
      <td class="gt_row gt_right">13.06</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-21</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">219</td>
      <td class="gt_row gt_right">6494</td>
      <td class="gt_row gt_right">13.52</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-22</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">334</td>
      <td class="gt_row gt_right">6828</td>
      <td class="gt_row gt_right">14.21</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-23</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">270</td>
      <td class="gt_row gt_right">7098</td>
      <td class="gt_row gt_right">14.77</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-24</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">298</td>
      <td class="gt_row gt_right">7396</td>
      <td class="gt_row gt_right">15.40</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-25</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">246</td>
      <td class="gt_row gt_right">7642</td>
      <td class="gt_row gt_right">15.91</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-26</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">242</td>
      <td class="gt_row gt_right">7884</td>
      <td class="gt_row gt_right">16.41</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-27</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">319</td>
      <td class="gt_row gt_right">8203</td>
      <td class="gt_row gt_right">17.07</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-28</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">272</td>
      <td class="gt_row gt_right">8475</td>
      <td class="gt_row gt_right">17.64</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-29</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">378</td>
      <td class="gt_row gt_right">8853</td>
      <td class="gt_row gt_right">18.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-04-30</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">294</td>
      <td class="gt_row gt_right">9147</td>
      <td class="gt_row gt_right">19.04</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-01</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">294</td>
      <td class="gt_row gt_right">9441</td>
      <td class="gt_row gt_right">19.65</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-02</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">198</td>
      <td class="gt_row gt_right">9639</td>
      <td class="gt_row gt_right">20.06</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-03</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">169</td>
      <td class="gt_row gt_right">9808</td>
      <td class="gt_row gt_right">20.42</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-04</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">235</td>
      <td class="gt_row gt_right">10043</td>
      <td class="gt_row gt_right">20.90</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-05</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">284</td>
      <td class="gt_row gt_right">10327</td>
      <td class="gt_row gt_right">21.50</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-06</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">334</td>
      <td class="gt_row gt_right">10661</td>
      <td class="gt_row gt_right">22.19</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-07</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">312</td>
      <td class="gt_row gt_right">10973</td>
      <td class="gt_row gt_right">22.84</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-08</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">269</td>
      <td class="gt_row gt_right">11242</td>
      <td class="gt_row gt_right">23.40</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-09</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">176</td>
      <td class="gt_row gt_right">11418</td>
      <td class="gt_row gt_right">23.77</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-10</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">197</td>
      <td class="gt_row gt_right">11615</td>
      <td class="gt_row gt_right">24.18</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-11</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">308</td>
      <td class="gt_row gt_right">11923</td>
      <td class="gt_row gt_right">24.82</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-12</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">321</td>
      <td class="gt_row gt_right">12244</td>
      <td class="gt_row gt_right">25.49</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-13</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">298</td>
      <td class="gt_row gt_right">12542</td>
      <td class="gt_row gt_right">26.11</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-14</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">226</td>
      <td class="gt_row gt_right">12768</td>
      <td class="gt_row gt_right">26.58</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-15</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">251</td>
      <td class="gt_row gt_right">13019</td>
      <td class="gt_row gt_right">27.10</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-16</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">257</td>
      <td class="gt_row gt_right">13276</td>
      <td class="gt_row gt_right">27.63</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-17</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">203</td>
      <td class="gt_row gt_right">13479</td>
      <td class="gt_row gt_right">28.06</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-18</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">272</td>
      <td class="gt_row gt_right">13751</td>
      <td class="gt_row gt_right">28.62</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-19</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">293</td>
      <td class="gt_row gt_right">14044</td>
      <td class="gt_row gt_right">29.23</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-20</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">274</td>
      <td class="gt_row gt_right">14318</td>
      <td class="gt_row gt_right">29.80</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-21</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">281</td>
      <td class="gt_row gt_right">14599</td>
      <td class="gt_row gt_right">30.39</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-22</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">240</td>
      <td class="gt_row gt_right">14839</td>
      <td class="gt_row gt_right">30.89</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-23</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">164</td>
      <td class="gt_row gt_right">15003</td>
      <td class="gt_row gt_right">31.23</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-24</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">145</td>
      <td class="gt_row gt_right">15148</td>
      <td class="gt_row gt_right">31.53</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-25</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">192</td>
      <td class="gt_row gt_right">15340</td>
      <td class="gt_row gt_right">31.93</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-26</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">160</td>
      <td class="gt_row gt_right">15500</td>
      <td class="gt_row gt_right">32.26</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-27</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">197</td>
      <td class="gt_row gt_right">15697</td>
      <td class="gt_row gt_right">32.67</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-28</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">191</td>
      <td class="gt_row gt_right">15888</td>
      <td class="gt_row gt_right">33.07</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-29</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">170</td>
      <td class="gt_row gt_right">16058</td>
      <td class="gt_row gt_right">33.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-30</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">139</td>
      <td class="gt_row gt_right">16197</td>
      <td class="gt_row gt_right">33.71</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-05-31</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">149</td>
      <td class="gt_row gt_right">16346</td>
      <td class="gt_row gt_right">34.02</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-01</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">217</td>
      <td class="gt_row gt_right">16563</td>
      <td class="gt_row gt_right">34.48</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-02</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">218</td>
      <td class="gt_row gt_right">16781</td>
      <td class="gt_row gt_right">34.93</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-03</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">171</td>
      <td class="gt_row gt_right">16952</td>
      <td class="gt_row gt_right">35.29</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-04</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">143</td>
      <td class="gt_row gt_right">17095</td>
      <td class="gt_row gt_right">35.58</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-05</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">153</td>
      <td class="gt_row gt_right">17248</td>
      <td class="gt_row gt_right">35.90</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-06</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">105</td>
      <td class="gt_row gt_right">17353</td>
      <td class="gt_row gt_right">36.12</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-07</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">127</td>
      <td class="gt_row gt_right">17480</td>
      <td class="gt_row gt_right">36.39</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-08</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">173</td>
      <td class="gt_row gt_right">17653</td>
      <td class="gt_row gt_right">36.75</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-09</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">176</td>
      <td class="gt_row gt_right">17829</td>
      <td class="gt_row gt_right">37.11</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-10</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">167</td>
      <td class="gt_row gt_right">17996</td>
      <td class="gt_row gt_right">37.46</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-11</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">182</td>
      <td class="gt_row gt_right">18178</td>
      <td class="gt_row gt_right">37.84</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-12</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">174</td>
      <td class="gt_row gt_right">18352</td>
      <td class="gt_row gt_right">38.20</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-13</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">133</td>
      <td class="gt_row gt_right">18485</td>
      <td class="gt_row gt_right">38.48</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-14</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">145</td>
      <td class="gt_row gt_right">18630</td>
      <td class="gt_row gt_right">38.78</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-15</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">165</td>
      <td class="gt_row gt_right">18795</td>
      <td class="gt_row gt_right">39.12</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-16</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">161</td>
      <td class="gt_row gt_right">18956</td>
      <td class="gt_row gt_right">39.46</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-17</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">164</td>
      <td class="gt_row gt_right">19120</td>
      <td class="gt_row gt_right">39.80</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-18</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">169</td>
      <td class="gt_row gt_right">19289</td>
      <td class="gt_row gt_right">40.15</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-19</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">153</td>
      <td class="gt_row gt_right">19442</td>
      <td class="gt_row gt_right">40.47</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-20</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">145</td>
      <td class="gt_row gt_right">19587</td>
      <td class="gt_row gt_right">40.77</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-21</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">130</td>
      <td class="gt_row gt_right">19717</td>
      <td class="gt_row gt_right">41.04</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-22</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">177</td>
      <td class="gt_row gt_right">19894</td>
      <td class="gt_row gt_right">41.41</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-23</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">169</td>
      <td class="gt_row gt_right">20063</td>
      <td class="gt_row gt_right">41.76</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-24</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">172</td>
      <td class="gt_row gt_right">20235</td>
      <td class="gt_row gt_right">42.12</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-25</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">125</td>
      <td class="gt_row gt_right">20360</td>
      <td class="gt_row gt_right">42.38</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-26</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">114</td>
      <td class="gt_row gt_right">20474</td>
      <td class="gt_row gt_right">42.62</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-27</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">87</td>
      <td class="gt_row gt_right">20561</td>
      <td class="gt_row gt_right">42.80</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-28</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">78</td>
      <td class="gt_row gt_right">20639</td>
      <td class="gt_row gt_right">42.96</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-29</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">128</td>
      <td class="gt_row gt_right">20767</td>
      <td class="gt_row gt_right">43.23</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-06-30</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">128</td>
      <td class="gt_row gt_right">20895</td>
      <td class="gt_row gt_right">43.49</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-01</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">120</td>
      <td class="gt_row gt_right">21015</td>
      <td class="gt_row gt_right">43.74</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-02</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">105</td>
      <td class="gt_row gt_right">21120</td>
      <td class="gt_row gt_right">43.96</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-03</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">107</td>
      <td class="gt_row gt_right">21227</td>
      <td class="gt_row gt_right">44.18</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-04</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">85</td>
      <td class="gt_row gt_right">21312</td>
      <td class="gt_row gt_right">44.36</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-05</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">127</td>
      <td class="gt_row gt_right">21439</td>
      <td class="gt_row gt_right">44.63</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-06</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">139</td>
      <td class="gt_row gt_right">21578</td>
      <td class="gt_row gt_right">44.92</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-07</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">108</td>
      <td class="gt_row gt_right">21686</td>
      <td class="gt_row gt_right">45.14</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-08</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">110</td>
      <td class="gt_row gt_right">21796</td>
      <td class="gt_row gt_right">45.37</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-09</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">95</td>
      <td class="gt_row gt_right">21891</td>
      <td class="gt_row gt_right">45.57</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-10</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">99</td>
      <td class="gt_row gt_right">21990</td>
      <td class="gt_row gt_right">45.77</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-11</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">96</td>
      <td class="gt_row gt_right">22086</td>
      <td class="gt_row gt_right">45.97</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-12</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">46</td>
      <td class="gt_row gt_right">22132</td>
      <td class="gt_row gt_right">46.07</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-13</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">105</td>
      <td class="gt_row gt_right">22237</td>
      <td class="gt_row gt_right">46.29</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-14</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">89</td>
      <td class="gt_row gt_right">22326</td>
      <td class="gt_row gt_right">46.47</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-15</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">112</td>
      <td class="gt_row gt_right">22438</td>
      <td class="gt_row gt_right">46.71</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-16</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">92</td>
      <td class="gt_row gt_right">22530</td>
      <td class="gt_row gt_right">46.90</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-17</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">80</td>
      <td class="gt_row gt_right">22610</td>
      <td class="gt_row gt_right">47.06</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-18</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">63</td>
      <td class="gt_row gt_right">22673</td>
      <td class="gt_row gt_right">47.19</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-19</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">48</td>
      <td class="gt_row gt_right">22721</td>
      <td class="gt_row gt_right">47.29</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-20</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">83</td>
      <td class="gt_row gt_right">22804</td>
      <td class="gt_row gt_right">47.47</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-21</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">68</td>
      <td class="gt_row gt_right">22872</td>
      <td class="gt_row gt_right">47.61</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-22</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">88</td>
      <td class="gt_row gt_right">22960</td>
      <td class="gt_row gt_right">47.79</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-23</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">94</td>
      <td class="gt_row gt_right">23054</td>
      <td class="gt_row gt_right">47.99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-24</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">83</td>
      <td class="gt_row gt_right">23137</td>
      <td class="gt_row gt_right">48.16</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-25</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">44</td>
      <td class="gt_row gt_right">23181</td>
      <td class="gt_row gt_right">48.25</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-26</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">38</td>
      <td class="gt_row gt_right">23219</td>
      <td class="gt_row gt_right">48.33</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-27</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">92</td>
      <td class="gt_row gt_right">23311</td>
      <td class="gt_row gt_right">48.52</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-28</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">77</td>
      <td class="gt_row gt_right">23388</td>
      <td class="gt_row gt_right">48.68</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-29</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">108</td>
      <td class="gt_row gt_right">23496</td>
      <td class="gt_row gt_right">48.91</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-30</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">110</td>
      <td class="gt_row gt_right">23606</td>
      <td class="gt_row gt_right">49.14</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-07-31</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">75</td>
      <td class="gt_row gt_right">23681</td>
      <td class="gt_row gt_right">49.29</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-01</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">69</td>
      <td class="gt_row gt_right">23750</td>
      <td class="gt_row gt_right">49.44</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-02</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">54</td>
      <td class="gt_row gt_right">23804</td>
      <td class="gt_row gt_right">49.55</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-03</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">108</td>
      <td class="gt_row gt_right">23912</td>
      <td class="gt_row gt_right">49.77</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-04</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">88</td>
      <td class="gt_row gt_right">24000</td>
      <td class="gt_row gt_right">49.96</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-05</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">115</td>
      <td class="gt_row gt_right">24115</td>
      <td class="gt_row gt_right">50.20</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-06</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">113</td>
      <td class="gt_row gt_right">24228</td>
      <td class="gt_row gt_right">50.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-07</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">92</td>
      <td class="gt_row gt_right">24320</td>
      <td class="gt_row gt_right">50.62</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-08</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">74</td>
      <td class="gt_row gt_right">24394</td>
      <td class="gt_row gt_right">50.78</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-09</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">72</td>
      <td class="gt_row gt_right">24466</td>
      <td class="gt_row gt_right">50.93</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-10</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">210</td>
      <td class="gt_row gt_right">24676</td>
      <td class="gt_row gt_right">51.36</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-11</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">176</td>
      <td class="gt_row gt_right">24852</td>
      <td class="gt_row gt_right">51.73</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-12</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">119</td>
      <td class="gt_row gt_right">24971</td>
      <td class="gt_row gt_right">51.98</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-13</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">109</td>
      <td class="gt_row gt_right">25080</td>
      <td class="gt_row gt_right">52.20</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-14</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">87</td>
      <td class="gt_row gt_right">25167</td>
      <td class="gt_row gt_right">52.39</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-15</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">55</td>
      <td class="gt_row gt_right">25222</td>
      <td class="gt_row gt_right">52.50</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-16</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">58</td>
      <td class="gt_row gt_right">25280</td>
      <td class="gt_row gt_right">52.62</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-17</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">138</td>
      <td class="gt_row gt_right">25418</td>
      <td class="gt_row gt_right">52.91</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-18</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">77</td>
      <td class="gt_row gt_right">25495</td>
      <td class="gt_row gt_right">53.07</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-19</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">102</td>
      <td class="gt_row gt_right">25597</td>
      <td class="gt_row gt_right">53.28</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-20</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">100</td>
      <td class="gt_row gt_right">25697</td>
      <td class="gt_row gt_right">53.49</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-21</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">101</td>
      <td class="gt_row gt_right">25798</td>
      <td class="gt_row gt_right">53.70</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-22</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">49</td>
      <td class="gt_row gt_right">25847</td>
      <td class="gt_row gt_right">53.80</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-23</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">52</td>
      <td class="gt_row gt_right">25899</td>
      <td class="gt_row gt_right">53.91</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-24</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">83</td>
      <td class="gt_row gt_right">25982</td>
      <td class="gt_row gt_right">54.08</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-25</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">73</td>
      <td class="gt_row gt_right">26055</td>
      <td class="gt_row gt_right">54.23</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-26</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">91</td>
      <td class="gt_row gt_right">26146</td>
      <td class="gt_row gt_right">54.42</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-27</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">84</td>
      <td class="gt_row gt_right">26230</td>
      <td class="gt_row gt_right">54.60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-28</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">96</td>
      <td class="gt_row gt_right">26326</td>
      <td class="gt_row gt_right">54.80</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-29</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">49</td>
      <td class="gt_row gt_right">26375</td>
      <td class="gt_row gt_right">54.90</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-30</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">61</td>
      <td class="gt_row gt_right">26436</td>
      <td class="gt_row gt_right">55.03</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-08-31</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">87</td>
      <td class="gt_row gt_right">26523</td>
      <td class="gt_row gt_right">55.21</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-01</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">153</td>
      <td class="gt_row gt_right">26676</td>
      <td class="gt_row gt_right">55.53</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-02</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">250</td>
      <td class="gt_row gt_right">26926</td>
      <td class="gt_row gt_right">56.05</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-03</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">239</td>
      <td class="gt_row gt_right">27165</td>
      <td class="gt_row gt_right">56.54</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-04</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">182</td>
      <td class="gt_row gt_right">27347</td>
      <td class="gt_row gt_right">56.92</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-05</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">171</td>
      <td class="gt_row gt_right">27518</td>
      <td class="gt_row gt_right">57.28</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-06</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">139</td>
      <td class="gt_row gt_right">27657</td>
      <td class="gt_row gt_right">57.57</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-07</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">350</td>
      <td class="gt_row gt_right">28007</td>
      <td class="gt_row gt_right">58.30</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-08</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">313</td>
      <td class="gt_row gt_right">28320</td>
      <td class="gt_row gt_right">58.95</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-09</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">262</td>
      <td class="gt_row gt_right">28582</td>
      <td class="gt_row gt_right">59.49</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-10</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">213</td>
      <td class="gt_row gt_right">28795</td>
      <td class="gt_row gt_right">59.94</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-11</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">222</td>
      <td class="gt_row gt_right">29017</td>
      <td class="gt_row gt_right">60.40</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-12</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">147</td>
      <td class="gt_row gt_right">29164</td>
      <td class="gt_row gt_right">60.71</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-13</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">124</td>
      <td class="gt_row gt_right">29288</td>
      <td class="gt_row gt_right">60.96</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-14</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">201</td>
      <td class="gt_row gt_right">29489</td>
      <td class="gt_row gt_right">61.38</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-15</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">208</td>
      <td class="gt_row gt_right">29697</td>
      <td class="gt_row gt_right">61.82</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-16</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">245</td>
      <td class="gt_row gt_right">29942</td>
      <td class="gt_row gt_right">62.33</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-17</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">243</td>
      <td class="gt_row gt_right">30185</td>
      <td class="gt_row gt_right">62.83</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-18</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">261</td>
      <td class="gt_row gt_right">30446</td>
      <td class="gt_row gt_right">63.37</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-19</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">256</td>
      <td class="gt_row gt_right">30702</td>
      <td class="gt_row gt_right">63.91</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-20</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">276</td>
      <td class="gt_row gt_right">30978</td>
      <td class="gt_row gt_right">64.48</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-21</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">308</td>
      <td class="gt_row gt_right">31286</td>
      <td class="gt_row gt_right">65.12</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-22</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">286</td>
      <td class="gt_row gt_right">31572</td>
      <td class="gt_row gt_right">65.72</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-23</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">323</td>
      <td class="gt_row gt_right">31895</td>
      <td class="gt_row gt_right">66.39</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-24</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">350</td>
      <td class="gt_row gt_right">32245</td>
      <td class="gt_row gt_right">67.12</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-25</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">348</td>
      <td class="gt_row gt_right">32593</td>
      <td class="gt_row gt_right">67.84</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-26</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">345</td>
      <td class="gt_row gt_right">32938</td>
      <td class="gt_row gt_right">68.56</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-27</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">399</td>
      <td class="gt_row gt_right">33337</td>
      <td class="gt_row gt_right">69.39</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-28</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">580</td>
      <td class="gt_row gt_right">33917</td>
      <td class="gt_row gt_right">70.60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-29</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">602</td>
      <td class="gt_row gt_right">34519</td>
      <td class="gt_row gt_right">71.85</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-09-30</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">879</td>
      <td class="gt_row gt_right">35398</td>
      <td class="gt_row gt_right">73.68</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-10-01</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">992</td>
      <td class="gt_row gt_right">36390</td>
      <td class="gt_row gt_right">75.75</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-10-02</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">1161</td>
      <td class="gt_row gt_right">37551</td>
      <td class="gt_row gt_right">78.16</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-10-03</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">957</td>
      <td class="gt_row gt_right">38508</td>
      <td class="gt_row gt_right">80.16</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-10-04</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">1065</td>
      <td class="gt_row gt_right">39573</td>
      <td class="gt_row gt_right">82.37</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-10-05</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">1441</td>
      <td class="gt_row gt_right">41014</td>
      <td class="gt_row gt_right">85.37</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-10-06</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">1438</td>
      <td class="gt_row gt_right">42452</td>
      <td class="gt_row gt_right">88.37</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-10-07</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">1540</td>
      <td class="gt_row gt_right">43992</td>
      <td class="gt_row gt_right">91.57</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-10-08</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">1626</td>
      <td class="gt_row gt_right">45618</td>
      <td class="gt_row gt_right">94.96</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-10-09</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">990</td>
      <td class="gt_row gt_right">46608</td>
      <td class="gt_row gt_right">97.02</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-10-10</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">704</td>
      <td class="gt_row gt_right">47312</td>
      <td class="gt_row gt_right">98.48</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-10-11</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">198</td>
      <td class="gt_row gt_right">47510</td>
      <td class="gt_row gt_right">98.89</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-10-12</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">13</td>
      <td class="gt_row gt_right">47523</td>
      <td class="gt_row gt_right">98.92</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">71521b16</td>
      <td class="gt_row gt_left">2020-10-13</td>
      <td class="gt_row gt_right">4804149</td>
      <td class="gt_row gt_right">-47523</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">0</td>
    </tr>
    <tr class="gt_group_heading_row">
      <td colspan="6" class="gt_group_heading">South East</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-05</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.001095</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-06</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.001095</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-07</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.001095</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-08</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">4</td>
      <td class="gt_row gt_right">5</td>
      <td class="gt_row gt_right">0.005474</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-09</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">0.006569</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-10</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">0.006569</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-11</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">0.006569</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-12</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">0.006569</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-13</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">0.006569</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-14</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">0.006569</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-15</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">0.006569</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-16</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">0.006569</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-17</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">0.006569</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-18</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">0.006569</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-19</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">0.006569</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-20</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">0.006569</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-21</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">0.006569</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-22</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">0.006569</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-23</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">7</td>
      <td class="gt_row gt_right">0.007664</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-24</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">8</td>
      <td class="gt_row gt_right">0.008759</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-25</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">9</td>
      <td class="gt_row gt_right">0.009854</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-26</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">9</td>
      <td class="gt_row gt_right">0.009854</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-27</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">11</td>
      <td class="gt_row gt_right">0.01204</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-28</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">13</td>
      <td class="gt_row gt_right">0.01423</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-02-29</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">15</td>
      <td class="gt_row gt_right">0.01642</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-01</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">15</td>
      <td class="gt_row gt_right">0.01642</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-02</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">16</td>
      <td class="gt_row gt_right">0.01752</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-03</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">9</td>
      <td class="gt_row gt_right">25</td>
      <td class="gt_row gt_right">0.02737</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-04</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">7</td>
      <td class="gt_row gt_right">32</td>
      <td class="gt_row gt_right">0.03504</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-05</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">4</td>
      <td class="gt_row gt_right">36</td>
      <td class="gt_row gt_right">0.03941</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-06</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">16</td>
      <td class="gt_row gt_right">52</td>
      <td class="gt_row gt_right">0.05693</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-07</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">5</td>
      <td class="gt_row gt_right">57</td>
      <td class="gt_row gt_right">0.06241</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-08</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">10</td>
      <td class="gt_row gt_right">67</td>
      <td class="gt_row gt_right">0.07336</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-09</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">24</td>
      <td class="gt_row gt_right">91</td>
      <td class="gt_row gt_right">0.09963</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-10</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">35</td>
      <td class="gt_row gt_right">126</td>
      <td class="gt_row gt_right">0.1380</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-11</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">50</td>
      <td class="gt_row gt_right">176</td>
      <td class="gt_row gt_right">0.1927</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-12</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">85</td>
      <td class="gt_row gt_right">261</td>
      <td class="gt_row gt_right">0.2858</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-13</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">53</td>
      <td class="gt_row gt_right">314</td>
      <td class="gt_row gt_right">0.3438</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-14</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">41</td>
      <td class="gt_row gt_right">355</td>
      <td class="gt_row gt_right">0.3887</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-15</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">53</td>
      <td class="gt_row gt_right">408</td>
      <td class="gt_row gt_right">0.4467</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-16</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">78</td>
      <td class="gt_row gt_right">486</td>
      <td class="gt_row gt_right">0.5321</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-17</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">89</td>
      <td class="gt_row gt_right">575</td>
      <td class="gt_row gt_right">0.6295</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-18</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">122</td>
      <td class="gt_row gt_right">697</td>
      <td class="gt_row gt_right">0.7631</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-19</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">127</td>
      <td class="gt_row gt_right">824</td>
      <td class="gt_row gt_right">0.9022</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-20</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">126</td>
      <td class="gt_row gt_right">950</td>
      <td class="gt_row gt_right">1.040</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-21</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">128</td>
      <td class="gt_row gt_right">1078</td>
      <td class="gt_row gt_right">1.180</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-22</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">169</td>
      <td class="gt_row gt_right">1247</td>
      <td class="gt_row gt_right">1.365</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-23</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">281</td>
      <td class="gt_row gt_right">1528</td>
      <td class="gt_row gt_right">1.673</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-24</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">235</td>
      <td class="gt_row gt_right">1763</td>
      <td class="gt_row gt_right">1.930</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-25</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">305</td>
      <td class="gt_row gt_right">2068</td>
      <td class="gt_row gt_right">2.264</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-26</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">320</td>
      <td class="gt_row gt_right">2388</td>
      <td class="gt_row gt_right">2.615</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-27</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">330</td>
      <td class="gt_row gt_right">2718</td>
      <td class="gt_row gt_right">2.976</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-28</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">339</td>
      <td class="gt_row gt_right">3057</td>
      <td class="gt_row gt_right">3.347</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-29</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">295</td>
      <td class="gt_row gt_right">3352</td>
      <td class="gt_row gt_right">3.670</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-30</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">476</td>
      <td class="gt_row gt_right">3828</td>
      <td class="gt_row gt_right">4.191</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-03-31</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">470</td>
      <td class="gt_row gt_right">4298</td>
      <td class="gt_row gt_right">4.706</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-01</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">546</td>
      <td class="gt_row gt_right">4844</td>
      <td class="gt_row gt_right">5.303</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-02</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">485</td>
      <td class="gt_row gt_right">5329</td>
      <td class="gt_row gt_right">5.834</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-03</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">519</td>
      <td class="gt_row gt_right">5848</td>
      <td class="gt_row gt_right">6.403</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-04</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">456</td>
      <td class="gt_row gt_right">6304</td>
      <td class="gt_row gt_right">6.902</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-05</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">424</td>
      <td class="gt_row gt_right">6728</td>
      <td class="gt_row gt_right">7.366</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-06</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">592</td>
      <td class="gt_row gt_right">7320</td>
      <td class="gt_row gt_right">8.014</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-07</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">717</td>
      <td class="gt_row gt_right">8037</td>
      <td class="gt_row gt_right">8.799</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-08</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">566</td>
      <td class="gt_row gt_right">8603</td>
      <td class="gt_row gt_right">9.419</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-09</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">522</td>
      <td class="gt_row gt_right">9125</td>
      <td class="gt_row gt_right">9.991</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-10</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">575</td>
      <td class="gt_row gt_right">9700</td>
      <td class="gt_row gt_right">10.62</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-11</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">382</td>
      <td class="gt_row gt_right">10082</td>
      <td class="gt_row gt_right">11.04</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-12</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">485</td>
      <td class="gt_row gt_right">10567</td>
      <td class="gt_row gt_right">11.57</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-13</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">576</td>
      <td class="gt_row gt_right">11143</td>
      <td class="gt_row gt_right">12.20</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-14</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">554</td>
      <td class="gt_row gt_right">11697</td>
      <td class="gt_row gt_right">12.81</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-15</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">711</td>
      <td class="gt_row gt_right">12408</td>
      <td class="gt_row gt_right">13.58</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-16</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">655</td>
      <td class="gt_row gt_right">13063</td>
      <td class="gt_row gt_right">14.30</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-17</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">704</td>
      <td class="gt_row gt_right">13767</td>
      <td class="gt_row gt_right">15.07</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-18</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">523</td>
      <td class="gt_row gt_right">14290</td>
      <td class="gt_row gt_right">15.65</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-19</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">414</td>
      <td class="gt_row gt_right">14704</td>
      <td class="gt_row gt_right">16.10</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-20</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">489</td>
      <td class="gt_row gt_right">15193</td>
      <td class="gt_row gt_right">16.63</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-21</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">633</td>
      <td class="gt_row gt_right">15826</td>
      <td class="gt_row gt_right">17.33</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-22</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">692</td>
      <td class="gt_row gt_right">16518</td>
      <td class="gt_row gt_right">18.08</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-23</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">684</td>
      <td class="gt_row gt_right">17202</td>
      <td class="gt_row gt_right">18.83</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-24</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">657</td>
      <td class="gt_row gt_right">17859</td>
      <td class="gt_row gt_right">19.55</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-25</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">518</td>
      <td class="gt_row gt_right">18377</td>
      <td class="gt_row gt_right">20.12</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-26</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">501</td>
      <td class="gt_row gt_right">18878</td>
      <td class="gt_row gt_right">20.67</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-27</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">555</td>
      <td class="gt_row gt_right">19433</td>
      <td class="gt_row gt_right">21.28</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-28</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">659</td>
      <td class="gt_row gt_right">20092</td>
      <td class="gt_row gt_right">22.00</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-29</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">691</td>
      <td class="gt_row gt_right">20783</td>
      <td class="gt_row gt_right">22.75</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-04-30</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">650</td>
      <td class="gt_row gt_right">21433</td>
      <td class="gt_row gt_right">23.47</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-01</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">522</td>
      <td class="gt_row gt_right">21955</td>
      <td class="gt_row gt_right">24.04</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-02</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">417</td>
      <td class="gt_row gt_right">22372</td>
      <td class="gt_row gt_right">24.49</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-03</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">363</td>
      <td class="gt_row gt_right">22735</td>
      <td class="gt_row gt_right">24.89</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-04</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">488</td>
      <td class="gt_row gt_right">23223</td>
      <td class="gt_row gt_right">25.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-05</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">475</td>
      <td class="gt_row gt_right">23698</td>
      <td class="gt_row gt_right">25.95</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-06</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">444</td>
      <td class="gt_row gt_right">24142</td>
      <td class="gt_row gt_right">26.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-07</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">420</td>
      <td class="gt_row gt_right">24562</td>
      <td class="gt_row gt_right">26.89</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-08</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">352</td>
      <td class="gt_row gt_right">24914</td>
      <td class="gt_row gt_right">27.28</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-09</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">246</td>
      <td class="gt_row gt_right">25160</td>
      <td class="gt_row gt_right">27.55</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-10</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">308</td>
      <td class="gt_row gt_right">25468</td>
      <td class="gt_row gt_right">27.88</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-11</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">481</td>
      <td class="gt_row gt_right">25949</td>
      <td class="gt_row gt_right">28.41</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-12</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">411</td>
      <td class="gt_row gt_right">26360</td>
      <td class="gt_row gt_right">28.86</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-13</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">482</td>
      <td class="gt_row gt_right">26842</td>
      <td class="gt_row gt_right">29.39</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-14</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">294</td>
      <td class="gt_row gt_right">27136</td>
      <td class="gt_row gt_right">29.71</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-15</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">262</td>
      <td class="gt_row gt_right">27398</td>
      <td class="gt_row gt_right">30.00</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-16</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">193</td>
      <td class="gt_row gt_right">27591</td>
      <td class="gt_row gt_right">30.21</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-17</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">223</td>
      <td class="gt_row gt_right">27814</td>
      <td class="gt_row gt_right">30.45</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-18</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">361</td>
      <td class="gt_row gt_right">28175</td>
      <td class="gt_row gt_right">30.85</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-19</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">385</td>
      <td class="gt_row gt_right">28560</td>
      <td class="gt_row gt_right">31.27</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-20</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">343</td>
      <td class="gt_row gt_right">28903</td>
      <td class="gt_row gt_right">31.64</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-21</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">303</td>
      <td class="gt_row gt_right">29206</td>
      <td class="gt_row gt_right">31.98</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-22</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">246</td>
      <td class="gt_row gt_right">29452</td>
      <td class="gt_row gt_right">32.25</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-23</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">121</td>
      <td class="gt_row gt_right">29573</td>
      <td class="gt_row gt_right">32.38</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-24</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">150</td>
      <td class="gt_row gt_right">29723</td>
      <td class="gt_row gt_right">32.54</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-25</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">186</td>
      <td class="gt_row gt_right">29909</td>
      <td class="gt_row gt_right">32.75</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-26</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">182</td>
      <td class="gt_row gt_right">30091</td>
      <td class="gt_row gt_right">32.95</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-27</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">201</td>
      <td class="gt_row gt_right">30292</td>
      <td class="gt_row gt_right">33.17</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-28</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">202</td>
      <td class="gt_row gt_right">30494</td>
      <td class="gt_row gt_right">33.39</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-29</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">163</td>
      <td class="gt_row gt_right">30657</td>
      <td class="gt_row gt_right">33.56</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-30</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">131</td>
      <td class="gt_row gt_right">30788</td>
      <td class="gt_row gt_right">33.71</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-05-31</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">134</td>
      <td class="gt_row gt_right">30922</td>
      <td class="gt_row gt_right">33.86</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-01</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">185</td>
      <td class="gt_row gt_right">31107</td>
      <td class="gt_row gt_right">34.06</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-02</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">221</td>
      <td class="gt_row gt_right">31328</td>
      <td class="gt_row gt_right">34.30</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-03</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">137</td>
      <td class="gt_row gt_right">31465</td>
      <td class="gt_row gt_right">34.45</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-04</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">158</td>
      <td class="gt_row gt_right">31623</td>
      <td class="gt_row gt_right">34.62</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-05</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">139</td>
      <td class="gt_row gt_right">31762</td>
      <td class="gt_row gt_right">34.77</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-06</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">85</td>
      <td class="gt_row gt_right">31847</td>
      <td class="gt_row gt_right">34.87</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-07</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">61</td>
      <td class="gt_row gt_right">31908</td>
      <td class="gt_row gt_right">34.93</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-08</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">121</td>
      <td class="gt_row gt_right">32029</td>
      <td class="gt_row gt_right">35.07</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-09</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">112</td>
      <td class="gt_row gt_right">32141</td>
      <td class="gt_row gt_right">35.19</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-10</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">127</td>
      <td class="gt_row gt_right">32268</td>
      <td class="gt_row gt_right">35.33</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-11</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">94</td>
      <td class="gt_row gt_right">32362</td>
      <td class="gt_row gt_right">35.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-12</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">89</td>
      <td class="gt_row gt_right">32451</td>
      <td class="gt_row gt_right">35.53</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-13</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">66</td>
      <td class="gt_row gt_right">32517</td>
      <td class="gt_row gt_right">35.60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-14</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">72</td>
      <td class="gt_row gt_right">32589</td>
      <td class="gt_row gt_right">35.68</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-15</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">98</td>
      <td class="gt_row gt_right">32687</td>
      <td class="gt_row gt_right">35.79</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-16</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">98</td>
      <td class="gt_row gt_right">32785</td>
      <td class="gt_row gt_right">35.89</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-17</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">82</td>
      <td class="gt_row gt_right">32867</td>
      <td class="gt_row gt_right">35.98</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-18</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">75</td>
      <td class="gt_row gt_right">32942</td>
      <td class="gt_row gt_right">36.07</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-19</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">81</td>
      <td class="gt_row gt_right">33023</td>
      <td class="gt_row gt_right">36.16</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-20</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">65</td>
      <td class="gt_row gt_right">33088</td>
      <td class="gt_row gt_right">36.23</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-21</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">66</td>
      <td class="gt_row gt_right">33154</td>
      <td class="gt_row gt_right">36.30</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-22</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">78</td>
      <td class="gt_row gt_right">33232</td>
      <td class="gt_row gt_right">36.38</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-23</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">60</td>
      <td class="gt_row gt_right">33292</td>
      <td class="gt_row gt_right">36.45</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-24</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">44</td>
      <td class="gt_row gt_right">33336</td>
      <td class="gt_row gt_right">36.50</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-25</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">62</td>
      <td class="gt_row gt_right">33398</td>
      <td class="gt_row gt_right">36.57</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-26</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">66</td>
      <td class="gt_row gt_right">33464</td>
      <td class="gt_row gt_right">36.64</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-27</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">54</td>
      <td class="gt_row gt_right">33518</td>
      <td class="gt_row gt_right">36.70</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-28</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">45</td>
      <td class="gt_row gt_right">33563</td>
      <td class="gt_row gt_right">36.75</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-29</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">81</td>
      <td class="gt_row gt_right">33644</td>
      <td class="gt_row gt_right">36.84</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-06-30</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">57</td>
      <td class="gt_row gt_right">33701</td>
      <td class="gt_row gt_right">36.90</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-01</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">69</td>
      <td class="gt_row gt_right">33770</td>
      <td class="gt_row gt_right">36.97</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-02</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">61</td>
      <td class="gt_row gt_right">33831</td>
      <td class="gt_row gt_right">37.04</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-03</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">61</td>
      <td class="gt_row gt_right">33892</td>
      <td class="gt_row gt_right">37.11</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-04</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">28</td>
      <td class="gt_row gt_right">33920</td>
      <td class="gt_row gt_right">37.14</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-05</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">58</td>
      <td class="gt_row gt_right">33978</td>
      <td class="gt_row gt_right">37.20</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-06</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">45</td>
      <td class="gt_row gt_right">34023</td>
      <td class="gt_row gt_right">37.25</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-07</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">55</td>
      <td class="gt_row gt_right">34078</td>
      <td class="gt_row gt_right">37.31</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-08</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">82</td>
      <td class="gt_row gt_right">34160</td>
      <td class="gt_row gt_right">37.40</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-09</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">85</td>
      <td class="gt_row gt_right">34245</td>
      <td class="gt_row gt_right">37.49</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-10</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">76</td>
      <td class="gt_row gt_right">34321</td>
      <td class="gt_row gt_right">37.58</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-11</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">34</td>
      <td class="gt_row gt_right">34355</td>
      <td class="gt_row gt_right">37.61</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-12</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">30</td>
      <td class="gt_row gt_right">34385</td>
      <td class="gt_row gt_right">37.65</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-13</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">69</td>
      <td class="gt_row gt_right">34454</td>
      <td class="gt_row gt_right">37.72</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-14</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">58</td>
      <td class="gt_row gt_right">34512</td>
      <td class="gt_row gt_right">37.79</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-15</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">63</td>
      <td class="gt_row gt_right">34575</td>
      <td class="gt_row gt_right">37.85</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-16</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">64</td>
      <td class="gt_row gt_right">34639</td>
      <td class="gt_row gt_right">37.92</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-17</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">50</td>
      <td class="gt_row gt_right">34689</td>
      <td class="gt_row gt_right">37.98</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-18</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">54</td>
      <td class="gt_row gt_right">34743</td>
      <td class="gt_row gt_right">38.04</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-19</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">46</td>
      <td class="gt_row gt_right">34789</td>
      <td class="gt_row gt_right">38.09</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-20</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">87</td>
      <td class="gt_row gt_right">34876</td>
      <td class="gt_row gt_right">38.18</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-21</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">77</td>
      <td class="gt_row gt_right">34953</td>
      <td class="gt_row gt_right">38.27</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-22</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">78</td>
      <td class="gt_row gt_right">35031</td>
      <td class="gt_row gt_right">38.35</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-23</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">76</td>
      <td class="gt_row gt_right">35107</td>
      <td class="gt_row gt_right">38.44</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-24</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">65</td>
      <td class="gt_row gt_right">35172</td>
      <td class="gt_row gt_right">38.51</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-25</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">47</td>
      <td class="gt_row gt_right">35219</td>
      <td class="gt_row gt_right">38.56</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-26</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">37</td>
      <td class="gt_row gt_right">35256</td>
      <td class="gt_row gt_right">38.60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-27</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">85</td>
      <td class="gt_row gt_right">35341</td>
      <td class="gt_row gt_right">38.69</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-28</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">64</td>
      <td class="gt_row gt_right">35405</td>
      <td class="gt_row gt_right">38.76</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-29</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">80</td>
      <td class="gt_row gt_right">35485</td>
      <td class="gt_row gt_right">38.85</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-30</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">63</td>
      <td class="gt_row gt_right">35548</td>
      <td class="gt_row gt_right">38.92</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-07-31</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">41</td>
      <td class="gt_row gt_right">35589</td>
      <td class="gt_row gt_right">38.96</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-01</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">30</td>
      <td class="gt_row gt_right">35619</td>
      <td class="gt_row gt_right">39.00</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-02</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">21</td>
      <td class="gt_row gt_right">35640</td>
      <td class="gt_row gt_right">39.02</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-03</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">77</td>
      <td class="gt_row gt_right">35717</td>
      <td class="gt_row gt_right">39.10</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-04</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">66</td>
      <td class="gt_row gt_right">35783</td>
      <td class="gt_row gt_right">39.18</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-05</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">65</td>
      <td class="gt_row gt_right">35848</td>
      <td class="gt_row gt_right">39.25</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-06</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">61</td>
      <td class="gt_row gt_right">35909</td>
      <td class="gt_row gt_right">39.32</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-07</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">54</td>
      <td class="gt_row gt_right">35963</td>
      <td class="gt_row gt_right">39.37</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-08</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">31</td>
      <td class="gt_row gt_right">35994</td>
      <td class="gt_row gt_right">39.41</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-09</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">29</td>
      <td class="gt_row gt_right">36023</td>
      <td class="gt_row gt_right">39.44</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-10</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">113</td>
      <td class="gt_row gt_right">36136</td>
      <td class="gt_row gt_right">39.56</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-11</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">77</td>
      <td class="gt_row gt_right">36213</td>
      <td class="gt_row gt_right">39.65</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-12</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">83</td>
      <td class="gt_row gt_right">36296</td>
      <td class="gt_row gt_right">39.74</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-13</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">77</td>
      <td class="gt_row gt_right">36373</td>
      <td class="gt_row gt_right">39.82</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-14</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">95</td>
      <td class="gt_row gt_right">36468</td>
      <td class="gt_row gt_right">39.93</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-15</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">45</td>
      <td class="gt_row gt_right">36513</td>
      <td class="gt_row gt_right">39.98</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-16</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">36</td>
      <td class="gt_row gt_right">36549</td>
      <td class="gt_row gt_right">40.02</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-17</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">109</td>
      <td class="gt_row gt_right">36658</td>
      <td class="gt_row gt_right">40.14</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-18</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">77</td>
      <td class="gt_row gt_right">36735</td>
      <td class="gt_row gt_right">40.22</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-19</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">115</td>
      <td class="gt_row gt_right">36850</td>
      <td class="gt_row gt_right">40.35</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-20</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">130</td>
      <td class="gt_row gt_right">36980</td>
      <td class="gt_row gt_right">40.49</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-21</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">110</td>
      <td class="gt_row gt_right">37090</td>
      <td class="gt_row gt_right">40.61</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-22</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">78</td>
      <td class="gt_row gt_right">37168</td>
      <td class="gt_row gt_right">40.69</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-23</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">84</td>
      <td class="gt_row gt_right">37252</td>
      <td class="gt_row gt_right">40.79</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-24</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">94</td>
      <td class="gt_row gt_right">37346</td>
      <td class="gt_row gt_right">40.89</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-25</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">90</td>
      <td class="gt_row gt_right">37436</td>
      <td class="gt_row gt_right">40.99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-26</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">115</td>
      <td class="gt_row gt_right">37551</td>
      <td class="gt_row gt_right">41.11</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-27</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">127</td>
      <td class="gt_row gt_right">37678</td>
      <td class="gt_row gt_right">41.25</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-28</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">116</td>
      <td class="gt_row gt_right">37794</td>
      <td class="gt_row gt_right">41.38</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-29</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">104</td>
      <td class="gt_row gt_right">37898</td>
      <td class="gt_row gt_right">41.49</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-30</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">101</td>
      <td class="gt_row gt_right">37999</td>
      <td class="gt_row gt_right">41.60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-08-31</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">117</td>
      <td class="gt_row gt_right">38116</td>
      <td class="gt_row gt_right">41.73</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-01</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">131</td>
      <td class="gt_row gt_right">38247</td>
      <td class="gt_row gt_right">41.87</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-02</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">209</td>
      <td class="gt_row gt_right">38456</td>
      <td class="gt_row gt_right">42.10</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-03</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">226</td>
      <td class="gt_row gt_right">38682</td>
      <td class="gt_row gt_right">42.35</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-04</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">233</td>
      <td class="gt_row gt_right">38915</td>
      <td class="gt_row gt_right">42.61</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-05</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">162</td>
      <td class="gt_row gt_right">39077</td>
      <td class="gt_row gt_right">42.78</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-06</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">181</td>
      <td class="gt_row gt_right">39258</td>
      <td class="gt_row gt_right">42.98</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-07</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">205</td>
      <td class="gt_row gt_right">39463</td>
      <td class="gt_row gt_right">43.21</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-08</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">191</td>
      <td class="gt_row gt_right">39654</td>
      <td class="gt_row gt_right">43.42</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-09</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">170</td>
      <td class="gt_row gt_right">39824</td>
      <td class="gt_row gt_right">43.60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-10</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">210</td>
      <td class="gt_row gt_right">40034</td>
      <td class="gt_row gt_right">43.83</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-11</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">166</td>
      <td class="gt_row gt_right">40200</td>
      <td class="gt_row gt_right">44.01</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-12</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">141</td>
      <td class="gt_row gt_right">40341</td>
      <td class="gt_row gt_right">44.17</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-13</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">68</td>
      <td class="gt_row gt_right">40409</td>
      <td class="gt_row gt_right">44.24</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-14</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">156</td>
      <td class="gt_row gt_right">40565</td>
      <td class="gt_row gt_right">44.41</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-15</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">142</td>
      <td class="gt_row gt_right">40707</td>
      <td class="gt_row gt_right">44.57</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-16</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">144</td>
      <td class="gt_row gt_right">40851</td>
      <td class="gt_row gt_right">44.73</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-17</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">169</td>
      <td class="gt_row gt_right">41020</td>
      <td class="gt_row gt_right">44.91</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-18</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">196</td>
      <td class="gt_row gt_right">41216</td>
      <td class="gt_row gt_right">45.13</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-19</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">183</td>
      <td class="gt_row gt_right">41399</td>
      <td class="gt_row gt_right">45.33</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-20</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">184</td>
      <td class="gt_row gt_right">41583</td>
      <td class="gt_row gt_right">45.53</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-21</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">236</td>
      <td class="gt_row gt_right">41819</td>
      <td class="gt_row gt_right">45.79</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-22</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">284</td>
      <td class="gt_row gt_right">42103</td>
      <td class="gt_row gt_right">46.10</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-23</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">281</td>
      <td class="gt_row gt_right">42384</td>
      <td class="gt_row gt_right">46.40</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-24</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">332</td>
      <td class="gt_row gt_right">42716</td>
      <td class="gt_row gt_right">46.77</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-25</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">346</td>
      <td class="gt_row gt_right">43062</td>
      <td class="gt_row gt_right">47.15</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-26</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">249</td>
      <td class="gt_row gt_right">43311</td>
      <td class="gt_row gt_right">47.42</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-27</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">315</td>
      <td class="gt_row gt_right">43626</td>
      <td class="gt_row gt_right">47.76</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-28</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">432</td>
      <td class="gt_row gt_right">44058</td>
      <td class="gt_row gt_right">48.24</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-29</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">436</td>
      <td class="gt_row gt_right">44494</td>
      <td class="gt_row gt_right">48.71</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-09-30</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">588</td>
      <td class="gt_row gt_right">45082</td>
      <td class="gt_row gt_right">49.36</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-10-01</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">644</td>
      <td class="gt_row gt_right">45726</td>
      <td class="gt_row gt_right">50.06</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-10-02</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">717</td>
      <td class="gt_row gt_right">46443</td>
      <td class="gt_row gt_right">50.85</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-10-03</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">648</td>
      <td class="gt_row gt_right">47091</td>
      <td class="gt_row gt_right">51.56</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-10-04</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">648</td>
      <td class="gt_row gt_right">47739</td>
      <td class="gt_row gt_right">52.27</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-10-05</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">872</td>
      <td class="gt_row gt_right">48611</td>
      <td class="gt_row gt_right">53.22</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-10-06</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">804</td>
      <td class="gt_row gt_right">49415</td>
      <td class="gt_row gt_right">54.10</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-10-07</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">1015</td>
      <td class="gt_row gt_right">50430</td>
      <td class="gt_row gt_right">55.21</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-10-08</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">1002</td>
      <td class="gt_row gt_right">51432</td>
      <td class="gt_row gt_right">56.31</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-10-09</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">609</td>
      <td class="gt_row gt_right">52041</td>
      <td class="gt_row gt_right">56.98</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-10-10</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">154</td>
      <td class="gt_row gt_right">52195</td>
      <td class="gt_row gt_right">57.15</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-10-11</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">88</td>
      <td class="gt_row gt_right">52283</td>
      <td class="gt_row gt_right">57.24</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-10-12</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">7</td>
      <td class="gt_row gt_right">52290</td>
      <td class="gt_row gt_right">57.25</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">78e4573d</td>
      <td class="gt_row gt_left">2020-10-13</td>
      <td class="gt_row gt_right">9133625</td>
      <td class="gt_row gt_right">-52290</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">0</td>
    </tr>
    <tr class="gt_group_heading_row">
      <td colspan="6" class="gt_group_heading">London</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-11</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.001123</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-12</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.001123</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-13</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.001123</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-14</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.001123</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-15</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.001123</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-16</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.001123</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-17</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.001123</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-18</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.001123</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-19</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.001123</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-20</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.001123</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-21</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.001123</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-22</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.001123</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-23</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">0.001123</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-24</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">0.002245</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-25</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">3</td>
      <td class="gt_row gt_right">5</td>
      <td class="gt_row gt_right">0.005613</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-26</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">7</td>
      <td class="gt_row gt_right">0.007858</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-27</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">2</td>
      <td class="gt_row gt_right">9</td>
      <td class="gt_row gt_right">0.01010</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-28</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">3</td>
      <td class="gt_row gt_right">12</td>
      <td class="gt_row gt_right">0.01347</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-02-29</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">1</td>
      <td class="gt_row gt_right">13</td>
      <td class="gt_row gt_right">0.01459</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-01</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">4</td>
      <td class="gt_row gt_right">17</td>
      <td class="gt_row gt_right">0.01908</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-02</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">12</td>
      <td class="gt_row gt_right">29</td>
      <td class="gt_row gt_right">0.03255</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-03</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">6</td>
      <td class="gt_row gt_right">35</td>
      <td class="gt_row gt_right">0.03929</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-04</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">18</td>
      <td class="gt_row gt_right">53</td>
      <td class="gt_row gt_right">0.05950</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-05</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">14</td>
      <td class="gt_row gt_right">67</td>
      <td class="gt_row gt_right">0.07521</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-06</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">23</td>
      <td class="gt_row gt_right">90</td>
      <td class="gt_row gt_right">0.1010</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-07</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">14</td>
      <td class="gt_row gt_right">104</td>
      <td class="gt_row gt_right">0.1167</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-08</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">22</td>
      <td class="gt_row gt_right">126</td>
      <td class="gt_row gt_right">0.1414</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-09</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">53</td>
      <td class="gt_row gt_right">179</td>
      <td class="gt_row gt_right">0.2009</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-10</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">92</td>
      <td class="gt_row gt_right">271</td>
      <td class="gt_row gt_right">0.3042</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-11</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">183</td>
      <td class="gt_row gt_right">454</td>
      <td class="gt_row gt_right">0.5096</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-12</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">165</td>
      <td class="gt_row gt_right">619</td>
      <td class="gt_row gt_right">0.6949</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-13</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">136</td>
      <td class="gt_row gt_right">755</td>
      <td class="gt_row gt_right">0.8475</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-14</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">149</td>
      <td class="gt_row gt_right">904</td>
      <td class="gt_row gt_right">1.015</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-15</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">165</td>
      <td class="gt_row gt_right">1069</td>
      <td class="gt_row gt_right">1.200</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-16</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">234</td>
      <td class="gt_row gt_right">1303</td>
      <td class="gt_row gt_right">1.463</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-17</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">310</td>
      <td class="gt_row gt_right">1613</td>
      <td class="gt_row gt_right">1.811</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-18</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">356</td>
      <td class="gt_row gt_right">1969</td>
      <td class="gt_row gt_right">2.210</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-19</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">333</td>
      <td class="gt_row gt_right">2302</td>
      <td class="gt_row gt_right">2.584</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-20</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">426</td>
      <td class="gt_row gt_right">2728</td>
      <td class="gt_row gt_right">3.062</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-21</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">351</td>
      <td class="gt_row gt_right">3079</td>
      <td class="gt_row gt_right">3.456</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-22</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">437</td>
      <td class="gt_row gt_right">3516</td>
      <td class="gt_row gt_right">3.947</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-23</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">688</td>
      <td class="gt_row gt_right">4204</td>
      <td class="gt_row gt_right">4.719</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-24</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">634</td>
      <td class="gt_row gt_right">4838</td>
      <td class="gt_row gt_right">5.431</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-25</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">759</td>
      <td class="gt_row gt_right">5597</td>
      <td class="gt_row gt_right">6.283</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-26</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">767</td>
      <td class="gt_row gt_right">6364</td>
      <td class="gt_row gt_right">7.144</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-27</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">819</td>
      <td class="gt_row gt_right">7183</td>
      <td class="gt_row gt_right">8.063</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-28</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">577</td>
      <td class="gt_row gt_right">7760</td>
      <td class="gt_row gt_right">8.711</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-29</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">595</td>
      <td class="gt_row gt_right">8355</td>
      <td class="gt_row gt_right">9.379</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-30</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">868</td>
      <td class="gt_row gt_right">9223</td>
      <td class="gt_row gt_right">10.35</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-03-31</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">935</td>
      <td class="gt_row gt_right">10158</td>
      <td class="gt_row gt_right">11.40</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-01</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">979</td>
      <td class="gt_row gt_right">11137</td>
      <td class="gt_row gt_right">12.50</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-02</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">1071</td>
      <td class="gt_row gt_right">12208</td>
      <td class="gt_row gt_right">13.70</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-03</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">983</td>
      <td class="gt_row gt_right">13191</td>
      <td class="gt_row gt_right">14.81</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-04</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">765</td>
      <td class="gt_row gt_right">13956</td>
      <td class="gt_row gt_right">15.67</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-05</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">634</td>
      <td class="gt_row gt_right">14590</td>
      <td class="gt_row gt_right">16.38</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-06</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">859</td>
      <td class="gt_row gt_right">15449</td>
      <td class="gt_row gt_right">17.34</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-07</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">953</td>
      <td class="gt_row gt_right">16402</td>
      <td class="gt_row gt_right">18.41</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-08</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">875</td>
      <td class="gt_row gt_right">17277</td>
      <td class="gt_row gt_right">19.39</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-09</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">798</td>
      <td class="gt_row gt_right">18075</td>
      <td class="gt_row gt_right">20.29</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-10</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">619</td>
      <td class="gt_row gt_right">18694</td>
      <td class="gt_row gt_right">20.99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-11</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">519</td>
      <td class="gt_row gt_right">19213</td>
      <td class="gt_row gt_right">21.57</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-12</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">500</td>
      <td class="gt_row gt_right">19713</td>
      <td class="gt_row gt_right">22.13</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-13</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">602</td>
      <td class="gt_row gt_right">20315</td>
      <td class="gt_row gt_right">22.81</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-14</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">603</td>
      <td class="gt_row gt_right">20918</td>
      <td class="gt_row gt_right">23.48</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-15</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">659</td>
      <td class="gt_row gt_right">21577</td>
      <td class="gt_row gt_right">24.22</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-16</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">577</td>
      <td class="gt_row gt_right">22154</td>
      <td class="gt_row gt_right">24.87</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-17</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">579</td>
      <td class="gt_row gt_right">22733</td>
      <td class="gt_row gt_right">25.52</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-18</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">403</td>
      <td class="gt_row gt_right">23136</td>
      <td class="gt_row gt_right">25.97</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-19</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">396</td>
      <td class="gt_row gt_right">23532</td>
      <td class="gt_row gt_right">26.42</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-20</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">434</td>
      <td class="gt_row gt_right">23966</td>
      <td class="gt_row gt_right">26.90</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-21</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">466</td>
      <td class="gt_row gt_right">24432</td>
      <td class="gt_row gt_right">27.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-22</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">397</td>
      <td class="gt_row gt_right">24829</td>
      <td class="gt_row gt_right">27.87</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-23</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">522</td>
      <td class="gt_row gt_right">25351</td>
      <td class="gt_row gt_right">28.46</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-24</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">378</td>
      <td class="gt_row gt_right">25729</td>
      <td class="gt_row gt_right">28.88</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-25</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">333</td>
      <td class="gt_row gt_right">26062</td>
      <td class="gt_row gt_right">29.26</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-26</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">310</td>
      <td class="gt_row gt_right">26372</td>
      <td class="gt_row gt_right">29.60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-27</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">418</td>
      <td class="gt_row gt_right">26790</td>
      <td class="gt_row gt_right">30.07</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-28</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">312</td>
      <td class="gt_row gt_right">27102</td>
      <td class="gt_row gt_right">30.42</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-29</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">431</td>
      <td class="gt_row gt_right">27533</td>
      <td class="gt_row gt_right">30.91</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-04-30</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">424</td>
      <td class="gt_row gt_right">27957</td>
      <td class="gt_row gt_right">31.38</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-01</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">356</td>
      <td class="gt_row gt_right">28313</td>
      <td class="gt_row gt_right">31.78</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-02</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">248</td>
      <td class="gt_row gt_right">28561</td>
      <td class="gt_row gt_right">32.06</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-03</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">168</td>
      <td class="gt_row gt_right">28729</td>
      <td class="gt_row gt_right">32.25</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-04</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">246</td>
      <td class="gt_row gt_right">28975</td>
      <td class="gt_row gt_right">32.53</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-05</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">220</td>
      <td class="gt_row gt_right">29195</td>
      <td class="gt_row gt_right">32.77</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-06</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">269</td>
      <td class="gt_row gt_right">29464</td>
      <td class="gt_row gt_right">33.08</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-07</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">271</td>
      <td class="gt_row gt_right">29735</td>
      <td class="gt_row gt_right">33.38</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-08</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">206</td>
      <td class="gt_row gt_right">29941</td>
      <td class="gt_row gt_right">33.61</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-09</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">129</td>
      <td class="gt_row gt_right">30070</td>
      <td class="gt_row gt_right">33.76</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-10</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">125</td>
      <td class="gt_row gt_right">30195</td>
      <td class="gt_row gt_right">33.90</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-11</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">159</td>
      <td class="gt_row gt_right">30354</td>
      <td class="gt_row gt_right">34.07</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-12</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">194</td>
      <td class="gt_row gt_right">30548</td>
      <td class="gt_row gt_right">34.29</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-13</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">203</td>
      <td class="gt_row gt_right">30751</td>
      <td class="gt_row gt_right">34.52</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-14</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">157</td>
      <td class="gt_row gt_right">30908</td>
      <td class="gt_row gt_right">34.70</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-15</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">132</td>
      <td class="gt_row gt_right">31040</td>
      <td class="gt_row gt_right">34.84</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-16</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">73</td>
      <td class="gt_row gt_right">31113</td>
      <td class="gt_row gt_right">34.93</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-17</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">79</td>
      <td class="gt_row gt_right">31192</td>
      <td class="gt_row gt_right">35.02</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-18</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">115</td>
      <td class="gt_row gt_right">31307</td>
      <td class="gt_row gt_right">35.14</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-19</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">131</td>
      <td class="gt_row gt_right">31438</td>
      <td class="gt_row gt_right">35.29</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-20</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">144</td>
      <td class="gt_row gt_right">31582</td>
      <td class="gt_row gt_right">35.45</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-21</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">158</td>
      <td class="gt_row gt_right">31740</td>
      <td class="gt_row gt_right">35.63</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-22</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">84</td>
      <td class="gt_row gt_right">31824</td>
      <td class="gt_row gt_right">35.72</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-23</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">65</td>
      <td class="gt_row gt_right">31889</td>
      <td class="gt_row gt_right">35.80</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-24</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">50</td>
      <td class="gt_row gt_right">31939</td>
      <td class="gt_row gt_right">35.85</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-25</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">58</td>
      <td class="gt_row gt_right">31997</td>
      <td class="gt_row gt_right">35.92</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-26</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">66</td>
      <td class="gt_row gt_right">32063</td>
      <td class="gt_row gt_right">35.99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-27</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">84</td>
      <td class="gt_row gt_right">32147</td>
      <td class="gt_row gt_right">36.09</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-28</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">74</td>
      <td class="gt_row gt_right">32221</td>
      <td class="gt_row gt_right">36.17</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-29</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">56</td>
      <td class="gt_row gt_right">32277</td>
      <td class="gt_row gt_right">36.23</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-30</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">39</td>
      <td class="gt_row gt_right">32316</td>
      <td class="gt_row gt_right">36.28</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-05-31</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">38</td>
      <td class="gt_row gt_right">32354</td>
      <td class="gt_row gt_right">36.32</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-01</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">49</td>
      <td class="gt_row gt_right">32403</td>
      <td class="gt_row gt_right">36.37</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-02</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">49</td>
      <td class="gt_row gt_right">32452</td>
      <td class="gt_row gt_right">36.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-03</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">79</td>
      <td class="gt_row gt_right">32531</td>
      <td class="gt_row gt_right">36.52</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-04</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">57</td>
      <td class="gt_row gt_right">32588</td>
      <td class="gt_row gt_right">36.58</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-05</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">47</td>
      <td class="gt_row gt_right">32635</td>
      <td class="gt_row gt_right">36.64</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-06</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">46</td>
      <td class="gt_row gt_right">32681</td>
      <td class="gt_row gt_right">36.69</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-07</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">45</td>
      <td class="gt_row gt_right">32726</td>
      <td class="gt_row gt_right">36.74</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-08</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">56</td>
      <td class="gt_row gt_right">32782</td>
      <td class="gt_row gt_right">36.80</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-09</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">71</td>
      <td class="gt_row gt_right">32853</td>
      <td class="gt_row gt_right">36.88</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-10</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">53</td>
      <td class="gt_row gt_right">32906</td>
      <td class="gt_row gt_right">36.94</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-11</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">47</td>
      <td class="gt_row gt_right">32953</td>
      <td class="gt_row gt_right">36.99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-12</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">54</td>
      <td class="gt_row gt_right">33007</td>
      <td class="gt_row gt_right">37.05</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-13</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">39</td>
      <td class="gt_row gt_right">33046</td>
      <td class="gt_row gt_right">37.10</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-14</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">31</td>
      <td class="gt_row gt_right">33077</td>
      <td class="gt_row gt_right">37.13</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-15</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">41</td>
      <td class="gt_row gt_right">33118</td>
      <td class="gt_row gt_right">37.18</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-16</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">47</td>
      <td class="gt_row gt_right">33165</td>
      <td class="gt_row gt_right">37.23</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-17</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">43</td>
      <td class="gt_row gt_right">33208</td>
      <td class="gt_row gt_right">37.28</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-18</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">46</td>
      <td class="gt_row gt_right">33254</td>
      <td class="gt_row gt_right">37.33</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-19</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">37</td>
      <td class="gt_row gt_right">33291</td>
      <td class="gt_row gt_right">37.37</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-20</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">28</td>
      <td class="gt_row gt_right">33319</td>
      <td class="gt_row gt_right">37.40</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-21</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">27</td>
      <td class="gt_row gt_right">33346</td>
      <td class="gt_row gt_right">37.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-22</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">51</td>
      <td class="gt_row gt_right">33397</td>
      <td class="gt_row gt_right">37.49</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-23</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">49</td>
      <td class="gt_row gt_right">33446</td>
      <td class="gt_row gt_right">37.55</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-24</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">46</td>
      <td class="gt_row gt_right">33492</td>
      <td class="gt_row gt_right">37.60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-25</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">57</td>
      <td class="gt_row gt_right">33549</td>
      <td class="gt_row gt_right">37.66</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-26</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">48</td>
      <td class="gt_row gt_right">33597</td>
      <td class="gt_row gt_right">37.72</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-27</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">32</td>
      <td class="gt_row gt_right">33629</td>
      <td class="gt_row gt_right">37.75</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-28</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">22</td>
      <td class="gt_row gt_right">33651</td>
      <td class="gt_row gt_right">37.78</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-29</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">58</td>
      <td class="gt_row gt_right">33709</td>
      <td class="gt_row gt_right">37.84</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-06-30</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">52</td>
      <td class="gt_row gt_right">33761</td>
      <td class="gt_row gt_right">37.90</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-01</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">41</td>
      <td class="gt_row gt_right">33802</td>
      <td class="gt_row gt_right">37.95</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-02</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">40</td>
      <td class="gt_row gt_right">33842</td>
      <td class="gt_row gt_right">37.99</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-03</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">38</td>
      <td class="gt_row gt_right">33880</td>
      <td class="gt_row gt_right">38.03</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-04</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">32</td>
      <td class="gt_row gt_right">33912</td>
      <td class="gt_row gt_right">38.07</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-05</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">51</td>
      <td class="gt_row gt_right">33963</td>
      <td class="gt_row gt_right">38.13</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-06</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">53</td>
      <td class="gt_row gt_right">34016</td>
      <td class="gt_row gt_right">38.19</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-07</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">61</td>
      <td class="gt_row gt_right">34077</td>
      <td class="gt_row gt_right">38.25</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-08</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">57</td>
      <td class="gt_row gt_right">34134</td>
      <td class="gt_row gt_right">38.32</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-09</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">54</td>
      <td class="gt_row gt_right">34188</td>
      <td class="gt_row gt_right">38.38</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-10</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">41</td>
      <td class="gt_row gt_right">34229</td>
      <td class="gt_row gt_right">38.42</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-11</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">20</td>
      <td class="gt_row gt_right">34249</td>
      <td class="gt_row gt_right">38.45</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-12</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">37</td>
      <td class="gt_row gt_right">34286</td>
      <td class="gt_row gt_right">38.49</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-13</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">85</td>
      <td class="gt_row gt_right">34371</td>
      <td class="gt_row gt_right">38.58</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-14</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">62</td>
      <td class="gt_row gt_right">34433</td>
      <td class="gt_row gt_right">38.65</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-15</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">76</td>
      <td class="gt_row gt_right">34509</td>
      <td class="gt_row gt_right">38.74</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-16</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">70</td>
      <td class="gt_row gt_right">34579</td>
      <td class="gt_row gt_right">38.82</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-17</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">48</td>
      <td class="gt_row gt_right">34627</td>
      <td class="gt_row gt_right">38.87</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-18</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">48</td>
      <td class="gt_row gt_right">34675</td>
      <td class="gt_row gt_right">38.93</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-19</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">41</td>
      <td class="gt_row gt_right">34716</td>
      <td class="gt_row gt_right">38.97</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-20</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">97</td>
      <td class="gt_row gt_right">34813</td>
      <td class="gt_row gt_right">39.08</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-21</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">82</td>
      <td class="gt_row gt_right">34895</td>
      <td class="gt_row gt_right">39.17</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-22</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">79</td>
      <td class="gt_row gt_right">34974</td>
      <td class="gt_row gt_right">39.26</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-23</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">70</td>
      <td class="gt_row gt_right">35044</td>
      <td class="gt_row gt_right">39.34</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-24</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">83</td>
      <td class="gt_row gt_right">35127</td>
      <td class="gt_row gt_right">39.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-25</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">55</td>
      <td class="gt_row gt_right">35182</td>
      <td class="gt_row gt_right">39.49</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-26</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">65</td>
      <td class="gt_row gt_right">35247</td>
      <td class="gt_row gt_right">39.57</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-27</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">99</td>
      <td class="gt_row gt_right">35346</td>
      <td class="gt_row gt_right">39.68</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-28</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">99</td>
      <td class="gt_row gt_right">35445</td>
      <td class="gt_row gt_right">39.79</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-29</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">99</td>
      <td class="gt_row gt_right">35544</td>
      <td class="gt_row gt_right">39.90</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-30</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">102</td>
      <td class="gt_row gt_right">35646</td>
      <td class="gt_row gt_right">40.02</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-07-31</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">73</td>
      <td class="gt_row gt_right">35719</td>
      <td class="gt_row gt_right">40.10</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-01</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">58</td>
      <td class="gt_row gt_right">35777</td>
      <td class="gt_row gt_right">40.16</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-02</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">76</td>
      <td class="gt_row gt_right">35853</td>
      <td class="gt_row gt_right">40.25</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-03</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">87</td>
      <td class="gt_row gt_right">35940</td>
      <td class="gt_row gt_right">40.35</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-04</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">104</td>
      <td class="gt_row gt_right">36044</td>
      <td class="gt_row gt_right">40.46</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-05</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">114</td>
      <td class="gt_row gt_right">36158</td>
      <td class="gt_row gt_right">40.59</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-06</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">104</td>
      <td class="gt_row gt_right">36262</td>
      <td class="gt_row gt_right">40.71</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-07</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">122</td>
      <td class="gt_row gt_right">36384</td>
      <td class="gt_row gt_right">40.84</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-08</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">55</td>
      <td class="gt_row gt_right">36439</td>
      <td class="gt_row gt_right">40.91</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-09</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">67</td>
      <td class="gt_row gt_right">36506</td>
      <td class="gt_row gt_right">40.98</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-10</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">142</td>
      <td class="gt_row gt_right">36648</td>
      <td class="gt_row gt_right">41.14</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-11</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">149</td>
      <td class="gt_row gt_right">36797</td>
      <td class="gt_row gt_right">41.31</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-12</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">157</td>
      <td class="gt_row gt_right">36954</td>
      <td class="gt_row gt_right">41.48</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-13</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">124</td>
      <td class="gt_row gt_right">37078</td>
      <td class="gt_row gt_right">41.62</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-14</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">126</td>
      <td class="gt_row gt_right">37204</td>
      <td class="gt_row gt_right">41.76</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-15</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">105</td>
      <td class="gt_row gt_right">37309</td>
      <td class="gt_row gt_right">41.88</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-16</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">88</td>
      <td class="gt_row gt_right">37397</td>
      <td class="gt_row gt_right">41.98</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-17</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">189</td>
      <td class="gt_row gt_right">37586</td>
      <td class="gt_row gt_right">42.19</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-18</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">181</td>
      <td class="gt_row gt_right">37767</td>
      <td class="gt_row gt_right">42.40</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-19</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">151</td>
      <td class="gt_row gt_right">37918</td>
      <td class="gt_row gt_right">42.57</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-20</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">206</td>
      <td class="gt_row gt_right">38124</td>
      <td class="gt_row gt_right">42.80</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-21</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">166</td>
      <td class="gt_row gt_right">38290</td>
      <td class="gt_row gt_right">42.98</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-22</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">118</td>
      <td class="gt_row gt_right">38408</td>
      <td class="gt_row gt_right">43.12</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-23</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">144</td>
      <td class="gt_row gt_right">38552</td>
      <td class="gt_row gt_right">43.28</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-24</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">197</td>
      <td class="gt_row gt_right">38749</td>
      <td class="gt_row gt_right">43.50</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-25</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">170</td>
      <td class="gt_row gt_right">38919</td>
      <td class="gt_row gt_right">43.69</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-26</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">164</td>
      <td class="gt_row gt_right">39083</td>
      <td class="gt_row gt_right">43.87</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-27</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">229</td>
      <td class="gt_row gt_right">39312</td>
      <td class="gt_row gt_right">44.13</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-28</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">221</td>
      <td class="gt_row gt_right">39533</td>
      <td class="gt_row gt_right">44.38</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-29</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">138</td>
      <td class="gt_row gt_right">39671</td>
      <td class="gt_row gt_right">44.53</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-30</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">133</td>
      <td class="gt_row gt_right">39804</td>
      <td class="gt_row gt_right">44.68</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-08-31</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">216</td>
      <td class="gt_row gt_right">40020</td>
      <td class="gt_row gt_right">44.93</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-01</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">311</td>
      <td class="gt_row gt_right">40331</td>
      <td class="gt_row gt_right">45.27</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-02</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">333</td>
      <td class="gt_row gt_right">40664</td>
      <td class="gt_row gt_right">45.65</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-03</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">348</td>
      <td class="gt_row gt_right">41012</td>
      <td class="gt_row gt_right">46.04</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-04</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">356</td>
      <td class="gt_row gt_right">41368</td>
      <td class="gt_row gt_right">46.44</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-05</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">271</td>
      <td class="gt_row gt_right">41639</td>
      <td class="gt_row gt_right">46.74</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-06</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">203</td>
      <td class="gt_row gt_right">41842</td>
      <td class="gt_row gt_right">46.97</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-07</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">451</td>
      <td class="gt_row gt_right">42293</td>
      <td class="gt_row gt_right">47.48</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-08</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">333</td>
      <td class="gt_row gt_right">42626</td>
      <td class="gt_row gt_right">47.85</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-09</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">335</td>
      <td class="gt_row gt_right">42961</td>
      <td class="gt_row gt_right">48.23</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-10</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">391</td>
      <td class="gt_row gt_right">43352</td>
      <td class="gt_row gt_right">48.67</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-11</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">322</td>
      <td class="gt_row gt_right">43674</td>
      <td class="gt_row gt_right">49.03</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-12</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">232</td>
      <td class="gt_row gt_right">43906</td>
      <td class="gt_row gt_right">49.29</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-13</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">116</td>
      <td class="gt_row gt_right">44022</td>
      <td class="gt_row gt_right">49.42</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-14</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">299</td>
      <td class="gt_row gt_right">44321</td>
      <td class="gt_row gt_right">49.75</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-15</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">273</td>
      <td class="gt_row gt_right">44594</td>
      <td class="gt_row gt_right">50.06</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-16</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">306</td>
      <td class="gt_row gt_right">44900</td>
      <td class="gt_row gt_right">50.40</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-17</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">324</td>
      <td class="gt_row gt_right">45224</td>
      <td class="gt_row gt_right">50.77</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-18</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">394</td>
      <td class="gt_row gt_right">45618</td>
      <td class="gt_row gt_right">51.21</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-19</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">362</td>
      <td class="gt_row gt_right">45980</td>
      <td class="gt_row gt_right">51.62</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-20</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">421</td>
      <td class="gt_row gt_right">46401</td>
      <td class="gt_row gt_right">52.09</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-21</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">433</td>
      <td class="gt_row gt_right">46834</td>
      <td class="gt_row gt_right">52.57</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-22</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">540</td>
      <td class="gt_row gt_right">47374</td>
      <td class="gt_row gt_right">53.18</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-23</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">646</td>
      <td class="gt_row gt_right">48020</td>
      <td class="gt_row gt_right">53.91</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-24</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">607</td>
      <td class="gt_row gt_right">48627</td>
      <td class="gt_row gt_right">54.59</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-25</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">604</td>
      <td class="gt_row gt_right">49231</td>
      <td class="gt_row gt_right">55.27</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-26</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">573</td>
      <td class="gt_row gt_right">49804</td>
      <td class="gt_row gt_right">55.91</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-27</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">566</td>
      <td class="gt_row gt_right">50370</td>
      <td class="gt_row gt_right">56.54</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-28</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">791</td>
      <td class="gt_row gt_right">51161</td>
      <td class="gt_row gt_right">57.43</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-29</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">769</td>
      <td class="gt_row gt_right">51930</td>
      <td class="gt_row gt_right">58.30</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-09-30</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">1088</td>
      <td class="gt_row gt_right">53018</td>
      <td class="gt_row gt_right">59.52</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-10-01</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">1159</td>
      <td class="gt_row gt_right">54177</td>
      <td class="gt_row gt_right">60.82</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-10-02</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">1137</td>
      <td class="gt_row gt_right">55314</td>
      <td class="gt_row gt_right">62.09</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-10-03</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">847</td>
      <td class="gt_row gt_right">56161</td>
      <td class="gt_row gt_right">63.05</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-10-04</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">909</td>
      <td class="gt_row gt_right">57070</td>
      <td class="gt_row gt_right">64.07</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-10-05</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">1353</td>
      <td class="gt_row gt_right">58423</td>
      <td class="gt_row gt_right">65.58</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-10-06</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">1263</td>
      <td class="gt_row gt_right">59686</td>
      <td class="gt_row gt_right">67.00</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-10-07</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">1484</td>
      <td class="gt_row gt_right">61170</td>
      <td class="gt_row gt_right">68.67</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-10-08</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">1432</td>
      <td class="gt_row gt_right">62602</td>
      <td class="gt_row gt_right">70.28</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-10-09</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">909</td>
      <td class="gt_row gt_right">63511</td>
      <td class="gt_row gt_right">71.30</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-10-10</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">181</td>
      <td class="gt_row gt_right">63692</td>
      <td class="gt_row gt_right">71.50</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-10-11</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">82</td>
      <td class="gt_row gt_right">63774</td>
      <td class="gt_row gt_right">71.59</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-10-12</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">7</td>
      <td class="gt_row gt_right">63781</td>
      <td class="gt_row gt_right">71.60</td>
    </tr>
    <tr>
      <td class="gt_row gt_left">e85b4aac</td>
      <td class="gt_row gt_left">2020-10-13</td>
      <td class="gt_row gt_right">8908081</td>
      <td class="gt_row gt_right">-63781</td>
      <td class="gt_row gt_right">0</td>
      <td class="gt_row gt_right">0</td>
    </tr>
  </tbody>
  
  
</table></div><!--/html_preserve-->

  
  6. Use `patchwork` operators and functions to combine at least two graphs using your project data or `garden_harvest` data if your project data aren't read.


```r
g1 <- dailycovid2 %>%
  filter(country == "United Kingdom") %>% 
  group_by(date, state) %>% 
  mutate(lag1_cases = lag(total_cases, 1, order_by = date)) %>% 
  replace_na(list(lag1_cases = 0)) %>%
  mutate(daily_cases = total_cases - lag1_cases) %>%    
  ungroup() %>% 
  complete(date, state) %>% 
  arrange(state,date) %>% 
  group_by(state) %>% 
  replace_na(list(daily_cases = 0)) %>% 
  mutate(cum_cases = cumsum(daily_cases),
         cum_cases_per_10000 = cum_cases/population*10000) %>% 
  filter(cum_cases > 0) %>% 
  ggplot(aes(x = date,
             y = cum_cases_per_10000,
             group = state,
             fill = state)) +
  geom_line(aes(x = date,
                y = cum_cases_per_10000,
                color = state)) +
  labs(title = "Cummulative Cases per 10000 People \nfor Each State Over Time in UK",
       x = "",
       y = "") +
  theme(legend.position = "none")

g2 <- dailycovid2 %>%
  filter(country == "United States") %>% 
  group_by(date, state) %>% 
  mutate(lag1_cases = lag(total_cases, 1, order_by = date)) %>% 
  replace_na(list(lag1_cases = 0)) %>%
  mutate(daily_cases = total_cases - lag1_cases) %>%    
  ungroup() %>% 
  complete(date, state) %>% 
  arrange(state,date) %>% 
  group_by(state) %>% 
  replace_na(list(daily_cases = 0)) %>% 
  mutate(cum_cases = cumsum(daily_cases),
         cum_cases_per_10000 = cum_cases/population*10000) %>% 
  filter(cum_cases > 0) %>% 
  ggplot(aes(x = date,
             y = cum_cases_per_10000,
             group = state,
             fill = state)) +
  geom_line(aes(x = date,
                y = cum_cases_per_10000,
                color = state)) +
  labs(title = "Cummulative Cases per 10000 People \nfor Each State Over Time in US",
       x = "",
       y = "") +
  theme(legend.position = "none")
g2 | g1
```

![](06_exercises_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

```r
#The names of the states are not included because there is no room to effectively show the names of the states in the plot. However, the more important message of the plot is still conveyed: there are more people in the US who have contracted COVID, which is likely due to a higher population in the US, but both graphs show very similar trends in the lines meaning the infection rates are similar, just scaled down.
```



  
**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**
