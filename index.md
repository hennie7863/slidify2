---
title       : Week 4 assignment
subtitle    : Shiny
author      : Hennie de Nooijer
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained    # {standalone, draft, selfcontained}
knit        : slidify::knit2slides
---

## Description assignment
In this assignment the graph is made dynamic by selecting the number of cylinders with checkboxes.

The app can be accessed at https://yellowdolphin.shinyapps.io/MyShinyApp/

---
## Servercode

This is the server code :


```r
library(shiny)
```

```
## Warning: package 'shiny' was built under R version 3.3.3
```

```r
# Define server logic required to draw a histogram
shinyServer(function(input, output) {
   
  output$distPlot <- renderPlot({
    
textOutput(input$fourcyl)
    # generate bins based on input$bins from ui.R
    NumberofCyl<- ifelse(input$fourcyl, 4, ifelse(input$sixcyl,6, 8 ))
    textOutput(input$fourcyl)
    filtermtcars <- mtcars[mtcars$cyl==NumberofCyl, ]
    x    <- filtermtcars[, 5] 
    bins <- seq(min(x), max(x), length.out = input$bins + 1)
    
    # draw the histogram with the specified number of bins
   hist(x, breaks = bins, col = 'darkgray', border = 'white')
    
  })
  
})
```

---
## UI code

This is the ui code code


```r
library(shiny)

# Define UI for application that draws a histogram
shinyUI(fluidPage(
  
  # Application title
  titlePanel("A shiny app about mtcars"),
  
  
  # Sidebar with a slider input for number of bins 
  sidebarLayout(
    sidebarPanel(
      h1("Move the slider!"),
       sliderInput("bins",
                   "Number of bins:",
                   min = 1,
                   max = 50,
                   value = 30),
       checkboxInput("fourcyl", "4 cylinders", value = TRUE),
       checkboxInput("sixcyl", " 6 cylinders", value = TRUE),
       checkboxInput("eightcyl", "8 cylinders", value = TRUE)
       ),
    
    # Show a plot of the generated distribution
    mainPanel(
       plotOutput("distPlot")
    )
  )
))
```

<!--html_preserve--><div class="container-fluid">
<h2>A shiny app about mtcars</h2>
<div class="row">
<div class="col-sm-4">
<form class="well">
<h1>Move the slider!</h1>
<div class="form-group shiny-input-container">
<label class="control-label" for="bins">Number of bins:</label>
<input class="js-range-slider" id="bins" data-min="1" data-max="50" data-from="30" data-step="1" data-grid="true" data-grid-num="9.8" data-grid-snap="false" data-prettify-separator="," data-prettify-enabled="true" data-keyboard="true" data-keyboard-step="2.04081632653061" data-data-type="number"/>
</div>
<div class="form-group shiny-input-container">
<div class="checkbox">
<label>
<input id="fourcyl" type="checkbox" checked="checked"/>
<span>4 cylinders</span>
</label>
</div>
</div>
<div class="form-group shiny-input-container">
<div class="checkbox">
<label>
<input id="sixcyl" type="checkbox" checked="checked"/>
<span> 6 cylinders</span>
</label>
</div>
</div>
<div class="form-group shiny-input-container">
<div class="checkbox">
<label>
<input id="eightcyl" type="checkbox" checked="checked"/>
<span>8 cylinders</span>
</label>
</div>
</div>
</form>
</div>
<div class="col-sm-8">
<div id="distPlot" class="shiny-plot-output" style="width: 100% ; height: 400px"></div>
</div>
</div>
</div><!--/html_preserve-->

