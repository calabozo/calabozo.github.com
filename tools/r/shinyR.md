---
layout: default
title: Shiny Tricks
---

# Dinamically create elements


```
# UI part of the module
ab_moduleUI <- function(id){
  ns <- NS(id)
  tagList(
    fluidRow(
      actionButton(ns("btn"), paste("ActionButton", id, sep="-")),
      textOutput(ns("txt"))
    )
  )
}

# Server part of the module
ab_module <- function(input, output, session){
  observeEvent(input$btn,{
    output$txt <- renderText("More information shown")
  })
}


# UI
ui <- fluidPage(
  # lapply(paste0("mod", 1:no_btn), ab_moduleUI)
  numericInput("num", "Number of buttons to show" ,value = 5, min = 3, max = 10),
  uiOutput("ui")
)

# Server side
server <- function(input, output, session){
  observeEvent(input$num, {
    output$ui <- renderUI({
      lapply(paste0("mod", 1:input$num), ab_moduleUI)
    })
    
    lapply(paste0("mod", 1:input$num), function(x) callModule(ab_module, x))
  })
  
}


shinyApp(ui, server)
```