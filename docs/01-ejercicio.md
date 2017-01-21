---
title: "R - ChickenWeight"
author: "Miguel Ángel Porras Naranjo"
output: 
  html_document:
    toc: true
    toc_depth: 2
runtime: shiny
---


# - `ChickWeight`


## Cargando los datos

Cargamos la librería `ChickWeight`.


```r
data(ChickWeight)
head(ChickWeight)
```

```
##   weight Time Chick Diet
## 1     42    0     1    1
## 2     51    2     1    1
## 3     59    4     1    1
## 4     64    6     1    1
## 5     76    8     1    1
## 6     93   10     1    1
```

Veamos un breve resumen de los datos.


```r
summary(ChickWeight)
```

```
##      weight           Time           Chick     Diet   
##  Min.   : 35.0   Min.   : 0.00   13     : 12   1:220  
##  1st Qu.: 63.0   1st Qu.: 4.00   9      : 12   2:120  
##  Median :103.0   Median :10.00   20     : 12   3:120  
##  Mean   :121.8   Mean   :10.72   10     : 12   4:118  
##  3rd Qu.:163.8   3rd Qu.:16.00   17     : 12          
##  Max.   :373.0   Max.   :21.00   19     : 12          
##                                  (Other):506
```

## Tablas de frencuencia

La tabla de frecuencia absoluta para la variable `weight` dividida en 10 subintervalos es,


```r
table(cut(ChickWeight$weight,breaks = 10))
```

```
## 
## (34.7,68.8]  (68.8,103]   (103,136]   (136,170]   (170,204]   (204,238] 
##         174         111          82          82          51          35 
##   (238,272]   (272,305]   (305,339]   (339,373] 
##          18          14           8           3
```

Para la variable `time` dividida en 7 subintervalos es,


```r
table(cut(ChickWeight$Time,breaks = 7))
```

```
## 
## (-0.021,3]      (3,6]      (6,9]     (9,12]    (12,15]    (15,18] 
##        100         98         49         98         48         94 
##    (18,21] 
##         91
```

Para la variable `Chick`


```r
tablaDes <- table(ChickWeight$Chick)
tablaDes <-tablaDes[order(as.numeric(names(tablaDes)))]
tablaDes
```

```
## 
##  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 
## 12 12 12 12 12 12 12 11 12 12 12 12 12 12  8  7 12  2 12 12 12 12 12 12 12 
## 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 
## 12 12 12 12 12 12 12 12 12 12 12 12 12 12 12 12 12 12 10 12 12 12 12 12 12
```

## Buscando outliers

Calculamos los posibles valores de la variable `Time`


```r
vec.time<-unique(ChickWeight$Time)
vec.time
```

```
##  [1]  0  2  4  6  8 10 12 14 16 18 20 21
```

Programamos el siguiente bucle para ver la lista de los outliers.


```r
for (days in vec.time)
{
  df = subset(ChickWeight, Time==days,select=c(1))
  media.df = mean(df$weight)
  var.df = sd(df$weight)
  int <- c(media.df-2*var.df,media.df+2*var.df)
  out <- which((df < int[1]) | (df >int[2]))
  if (length(out) != 0){
    print(paste(c("Los outliers para la edad", days, "son", out),collapse= " "))
    }
}
```

```
## [1] "Los outliers para la edad 2 son 3 18"
## [1] "Los outliers para la edad 4 son 5 16 42"
## [1] "Los outliers para la edad 6 son 16 42"
## [1] "Los outliers para la edad 8 son 16 20 42"
## [1] "Los outliers para la edad 10 son 16 20 34 42"
## [1] "Los outliers para la edad 12 son 16 20 34"
## [1] "Los outliers para la edad 14 son 19 33"
## [1] "Los outliers para la edad 16 son 13 18 21 32"
## [1] "Los outliers para la edad 18 son 18 21 32"
## [1] "Los outliers para la edad 20 son 21 32"
## [1] "Los outliers para la edad 21 son 20 31"
```

## Tabla de frecuencia conjunta


```r
conjuDes <- table(ChickWeight$Chick,ChickWeight$Diet)
conjunta <- conjuDes[order(as.numeric(rownames(conjuDes))), ]
conjunta
```

```
##     
##       1  2  3  4
##   1  12  0  0  0
##   2  12  0  0  0
##   3  12  0  0  0
##   4  12  0  0  0
##   5  12  0  0  0
##   6  12  0  0  0
##   7  12  0  0  0
##   8  11  0  0  0
##   9  12  0  0  0
##   10 12  0  0  0
##   11 12  0  0  0
##   12 12  0  0  0
##   13 12  0  0  0
##   14 12  0  0  0
##   15  8  0  0  0
##   16  7  0  0  0
##   17 12  0  0  0
##   18  2  0  0  0
##   19 12  0  0  0
##   20 12  0  0  0
##   21  0 12  0  0
##   22  0 12  0  0
##   23  0 12  0  0
##   24  0 12  0  0
##   25  0 12  0  0
##   26  0 12  0  0
##   27  0 12  0  0
##   28  0 12  0  0
##   29  0 12  0  0
##   30  0 12  0  0
##   31  0  0 12  0
##   32  0  0 12  0
##   33  0  0 12  0
##   34  0  0 12  0
##   35  0  0 12  0
##   36  0  0 12  0
##   37  0  0 12  0
##   38  0  0 12  0
##   39  0  0 12  0
##   40  0  0 12  0
##   41  0  0  0 12
##   42  0  0  0 12
##   43  0  0  0 12
##   44  0  0  0 10
##   45  0  0  0 12
##   46  0  0  0 12
##   47  0  0  0 12
##   48  0  0  0 12
##   49  0  0  0 12
##   50  0  0  0 12
```

Las tabla de frecuencias marginales son,


```r
marginal.X = margin.table(conjunta,margin=1)
marginal.Y = margin.table(conjunta,margin=2)
marginal.X
```

```
## 
##  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25 
## 12 12 12 12 12 12 12 11 12 12 12 12 12 12  8  7 12  2 12 12 12 12 12 12 12 
## 26 27 28 29 30 31 32 33 34 35 36 37 38 39 40 41 42 43 44 45 46 47 48 49 50 
## 12 12 12 12 12 12 12 12 12 12 12 12 12 12 12 12 12 12 10 12 12 12 12 12 12
```

```r
marginal.Y
```

```
## 
##   1   2   3   4 
## 220 120 120 118
```


```r
tabla <- addmargins(prop.table(conjunta)*100)
tabla
```

```
##      
##                 1           2           3           4         Sum
##   1     2.0761246   0.0000000   0.0000000   0.0000000   2.0761246
##   2     2.0761246   0.0000000   0.0000000   0.0000000   2.0761246
##   3     2.0761246   0.0000000   0.0000000   0.0000000   2.0761246
##   4     2.0761246   0.0000000   0.0000000   0.0000000   2.0761246
##   5     2.0761246   0.0000000   0.0000000   0.0000000   2.0761246
##   6     2.0761246   0.0000000   0.0000000   0.0000000   2.0761246
##   7     2.0761246   0.0000000   0.0000000   0.0000000   2.0761246
##   8     1.9031142   0.0000000   0.0000000   0.0000000   1.9031142
##   9     2.0761246   0.0000000   0.0000000   0.0000000   2.0761246
##   10    2.0761246   0.0000000   0.0000000   0.0000000   2.0761246
##   11    2.0761246   0.0000000   0.0000000   0.0000000   2.0761246
##   12    2.0761246   0.0000000   0.0000000   0.0000000   2.0761246
##   13    2.0761246   0.0000000   0.0000000   0.0000000   2.0761246
##   14    2.0761246   0.0000000   0.0000000   0.0000000   2.0761246
##   15    1.3840830   0.0000000   0.0000000   0.0000000   1.3840830
##   16    1.2110727   0.0000000   0.0000000   0.0000000   1.2110727
##   17    2.0761246   0.0000000   0.0000000   0.0000000   2.0761246
##   18    0.3460208   0.0000000   0.0000000   0.0000000   0.3460208
##   19    2.0761246   0.0000000   0.0000000   0.0000000   2.0761246
##   20    2.0761246   0.0000000   0.0000000   0.0000000   2.0761246
##   21    0.0000000   2.0761246   0.0000000   0.0000000   2.0761246
##   22    0.0000000   2.0761246   0.0000000   0.0000000   2.0761246
##   23    0.0000000   2.0761246   0.0000000   0.0000000   2.0761246
##   24    0.0000000   2.0761246   0.0000000   0.0000000   2.0761246
##   25    0.0000000   2.0761246   0.0000000   0.0000000   2.0761246
##   26    0.0000000   2.0761246   0.0000000   0.0000000   2.0761246
##   27    0.0000000   2.0761246   0.0000000   0.0000000   2.0761246
##   28    0.0000000   2.0761246   0.0000000   0.0000000   2.0761246
##   29    0.0000000   2.0761246   0.0000000   0.0000000   2.0761246
##   30    0.0000000   2.0761246   0.0000000   0.0000000   2.0761246
##   31    0.0000000   0.0000000   2.0761246   0.0000000   2.0761246
##   32    0.0000000   0.0000000   2.0761246   0.0000000   2.0761246
##   33    0.0000000   0.0000000   2.0761246   0.0000000   2.0761246
##   34    0.0000000   0.0000000   2.0761246   0.0000000   2.0761246
##   35    0.0000000   0.0000000   2.0761246   0.0000000   2.0761246
##   36    0.0000000   0.0000000   2.0761246   0.0000000   2.0761246
##   37    0.0000000   0.0000000   2.0761246   0.0000000   2.0761246
##   38    0.0000000   0.0000000   2.0761246   0.0000000   2.0761246
##   39    0.0000000   0.0000000   2.0761246   0.0000000   2.0761246
##   40    0.0000000   0.0000000   2.0761246   0.0000000   2.0761246
##   41    0.0000000   0.0000000   0.0000000   2.0761246   2.0761246
##   42    0.0000000   0.0000000   0.0000000   2.0761246   2.0761246
##   43    0.0000000   0.0000000   0.0000000   2.0761246   2.0761246
##   44    0.0000000   0.0000000   0.0000000   1.7301038   1.7301038
##   45    0.0000000   0.0000000   0.0000000   2.0761246   2.0761246
##   46    0.0000000   0.0000000   0.0000000   2.0761246   2.0761246
##   47    0.0000000   0.0000000   0.0000000   2.0761246   2.0761246
##   48    0.0000000   0.0000000   0.0000000   2.0761246   2.0761246
##   49    0.0000000   0.0000000   0.0000000   2.0761246   2.0761246
##   50    0.0000000   0.0000000   0.0000000   2.0761246   2.0761246
##   Sum  38.0622837  20.7612457  20.7612457  20.4152249 100.0000000
```

## Histograma del peso en cada grupo de edad

Vamos a crear una aplicación `Shiny` usando el paquete `ggplot2`.


```r
# library(ggplot2)
# require(shiny)
# shinyApp(
#   
#   ui = fluidPage(    
#   
#   titlePanel("Histograma del peso de los pollos según el tiempo"),
#   
#   sidebarLayout(      
#     
#     sidebarPanel(
#       selectInput("region", "Tiempo (días):", 
#                   choices=unique(ChickWeight$Time)),
#       hr()
#     ),
#     
#     mainPanel(
#       plotOutput("weightPlot")  
#     )
#     
#   )
# ),
# 
#   server = function(input, output) {
#   
#   output$weightPlot <- renderPlot({
#     
#     ggplot(subset(ChickWeight, Time == input$region), aes(x=weight)) +
#             geom_histogram(bins=6, colour="#FE9A2E", fill="#F7BE81") + 
#             labs(x="Peso (g)", y = "Densidad")
#   })
# },
# 
#   options = list(height = 700)
# )
```





