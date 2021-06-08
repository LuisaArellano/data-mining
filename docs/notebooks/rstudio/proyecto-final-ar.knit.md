---
title: "Proyecto Final (Association Rules)"
author: "Luisa Fernanda Arellano Alvarado"
date: "07 de Junio del 2021"
output:
  pdf_document:
    latex_engine: xelatex
  html_document:
    df_print: paged
---


## Introducción
Este notebook contiene las actividades a realizar como parte del proyecto final de la materia Data Mining del Tecnológico de Monterrey, Campus Estado de México para el ciclo escolar enero/junio de 2021.

Las actividades incluyen ejecución, personalización y codificación de estatutos del lenguaje R con diversas liberarías de reglas de asociación, así como cálculos, análisis, investigación y exposición de conceptos relacionados.

La respuesta a todas y cada una de las actividades (implementación de código, notas y enlace a video) debe quedar registrada en el presente documento en formato R Markdown, exportada como documento PDF y publicada en el repositorio GitHub del alumno.

**Fecha límite de entrega: sábado 5 de junio al cierre del día.**

### Recursos para la realización del proyecto
El proyecto podrá realizarse en un ambiente de desarrollo local o remoto, dependiendo de los recursos que disponga el alumno.

Recursos generales:

- Cuenta de usuario en GitHub.
- GitHub Desktop.
- Fork del [repositorio principal de la clase](https://github.com/marcianomoreno/data-mining.git)
- Conectividad a Internet.
- Dispositivo de grabación de video.
- Cuenta para publicación de video (puede ser privado) como es el caso de YouTube.

Ambiente de desarrollo local:

- Procesaror Intel i5, 8 GB RAM, 50 GB HD.
- R Studio Desktop Open Source Edition.
- Sistema operativo [soportado por R Studio](https://www.rstudio.com/about/platform-support/).
- Proyecto de R Studio con el repositorio GitHub del alumno.

Ambiente de desarrollo remoto:

- Cuenta gratuita en [R Studio Cloud](https://rstudio.cloud/).
- Espacio de trabajo creado a partir del repositorio GitHub del alumno.

Paquetes de R

- [`git2r`](https://cran.r-project.org/web/packages/git2r/index.html)
- [`arulesViz`](https://cran.r-project.org/web/packages/arulesViz/index.html) (incluye [`arules`](https://cran.r-project.org/web/packages/arules/index.html))
- [`dplyr`](https://cran.r-project.org/web/packages/dplyr/index.html)


### Conceptos

**Chunk de código**: región del documento identificada por triple tilde invertida.

```r
#Chunk de código
```

**Chunk de código sin personalización**: El alumno deberá ejecutar el chunk de código sin mayor personalización.

```r
#Chunk de código sin personalización.
print("Este es un chunk de código sin personalización.")
```

```
## [1] "Este es un chunk de código sin personalización."
```

Las actividades de codificación estarán identificadas con la clave **CODE-nn** previo al chunk y contarán con comentarios `#TODO: `.

**CODE-01** Suma 2+3
Implementa la operación de adición de los enteros 2 y 3

```r
#TODO: Implementa la operación de adición con los enteros: 2 y 3
2+3
```

```
## [1] 5
```

**Notas en markdown**: El alumno escribirá las notas requeridas en las secciones anotadas con la clave **NOTE-nn**


Escribe la fecha actual (**NOTE-01**): <FECHA>

## 07/06/2021

**CODE-02** Datos generales
Asigna tus datos a las siguientes variables:

```r
NOMBRE_COMPLETO = "Luisa Arellano"
DIGITOS_MATRICULA = 01377974
```

## Instalación y carga de paquetes base

Instala los paquetes base de este notebook.

Los siguientes paquetes son requeridos para aspectos operativos de los ejercicios, no están relacionados con los temas de la materia. Si los paquetes no están instalados deberás retirar los comentarios de los siguientes estatutos y ejecutar el chunk. Coloca los comentarios nuevamente cuando hayas instalado los paquetes.

**CODE-03** Instala `git2r`

```r
#TODO: Retira los comentarios la primera vez para instalar los paquetes.
#install.packages("git2r")
```

git2r es una interfaz para el sistema de control de versiones Git que usaremos para verificar que el número de commit del proyecto final sea el esperado. El valor esperado del commit será comunicado por separado.

```r
library(git2r)
git2r::revparse_single(repository("."),"HEAD")
```

```
## [c2521b2] 2021-06-08: commit de prueba
```


## Instalación y carga de paquetes para los ejercicios

Instala los paquetes requeridos para este notebook.

En caso que los no tengas será necesario que retires los comentarios y ejecutes los comandos de la siguiente celda.

<!-- El símbolo de comentarios en R es `#` -->

Tip: Coloca nuevamente en comentario las líneas de abajo en cuanto hayas instalado los paquetes.

**CODE-04** Instala `arulesViz`

```r
#TODO: Retira los comentarios la primera vez para instalar los paquetes.
#install.packages("arulesViz")
```


Carga la librería arulesViz (la cual carga automáticamente arules).

```r
library("arulesViz")
```

```
## Loading required package: arules
```

```
## Loading required package: Matrix
```

```
## 
## Attaching package: 'arules'
```

```
## The following objects are masked from 'package:base':
## 
##     abbreviate, write
```



## smallbasket: Fundamentos de Association Rules
Considera a `smallbasket` como la siguiente lista de transacciones, implementa el código necesario y responde a las solicitudes indicadas.

| TID | items |
| :-: | :-: |
| Tr10 | {beer, nuts, diapers} |
| Tr20 | {beer, coffee, diapers} |
| Tr30 | {beer, diapers, eggs} |
| Tr40 | {nuts, eggs, milk} |
| Tr50 | {nuts, coffee, diapers, eggs, milk}|


Responde a las siguientes preguntas de smallbasket:

## NOTE-02: 5 transacciones
## NOTE-03: frecuencia absoluta del item beer --> 3
## NOTE-04: frecuencia absoluta del item nuts --> 3
## NOTE-05: frecuencia absoluta del itemset  {beer, nuts}  --> 1
## NOTE-06: Support del item beer --> 3/5
## NOTE-07: Support del itemset  {beer, nuts} --> 1/5


```r
#Para sacar la frecuencia absoluta de beer
s <- list(c("beer", "nuts", "diapers" , "beer", "coffee", "diapers" , "beer", "diapers", "eggs","nuts", "eggs", "milk" ,"nuts", "coffee", "diapers", "eggs", "milk")) 
table(s)
```

```
## s
##    beer  coffee diapers    eggs    milk    nuts 
##       3       2       4       3       2       3
```


Implementaremos a `smallbasket` en R por medio de `list`.


```r
smallbasket <- list(c("beer", "nuts", "diapers"), 
               c("beer", "coffee", "diapers"), 
               c("beer", "diapers", "eggs"),
               c("nuts", "eggs", "milk"),
               c("nuts", "coffee", "diapers", "eggs", "milk"))

names(smallbasket) <- paste("Tr", seq(from = 10, to = 50, by = 10), sep = "")
```
Crea un objeto `smalltx` de tipo `arules::transactions` a partir de la lista `smallbasket` el cual deberá tener la siguiente estructura lógica:

| TID | beer | nuts | diapers | coffee | eggs | milk |
| --- | :-: | :-: | :-: | :-: | :-: | :-: |
| Tr10 | 1 | 1 | 1 | 0 | 0 | 0 |
| Tr20 | 1 | 0 | 1 | 1 | 0 | 0 |
| Tr30 | 1 | 0 | 1 | 0 | 1 | 0 |
| Tr40 | 0 | 1 | 0 | 0 | 1 | 1 |
| Tr50 | 0 | 1 | 1 | 1 | 1 | 1 |

**CODE-05** Crea una variable `smalltx` que sea un objeto `transactions` a partir de los datos en `smallbasket`.

```r
#TODO: Modifica el código de abajo para transformar smallbaskets a un objeto de tipo transactions
smalltx <- as(smallbasket, "transactions")
```

Verifica que smalltx sea de tipo transactions.

```r
class(smalltx)[1]=="transactions"
```

```
## [1] TRUE
```
Implementa en el chunk de abajo el código para visualizar la información general del objeto `smalltx` por medio de la función `summary()`, visualiza la salida **R Console** y responde en línea en el markdown a las preguntas de abajo.

```r
summary(smalltx)
```

```
## transactions as itemMatrix in sparse format with
##  5 rows (elements/itemsets/transactions) and
##  6 columns (items) and a density of 0.5666667 
## 
## most frequent items:
## diapers    beer    eggs    nuts  coffee (Other) 
##       4       3       3       3       2       2 
## 
## element (itemset/transaction) length distribution:
## sizes
## 3 5 
## 4 1 
## 
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##     3.0     3.0     3.0     3.4     3.0     5.0 
## 
## includes extended item information - examples:
##    labels
## 1    beer
## 2  coffee
## 3 diapers
## 
## includes extended transaction information - examples:
##   transactionID
## 1          Tr10
## 2          Tr20
## 3          Tr30
```

## NOTE-08: Número de renglones = 5
## NOTE-09: Número de columnas = 6
## NOTE-10:Densidad de la matriz= 0.5666667 
## NOTE-11:Media aritmética del número de elementos (itemsets) por transacción = 3.0 

Visualiza la matriz de items y transacciones.

```r
try(
  plot(smalltx, main = paste0("Elaborado por: ", NOMBRE_COMPLETO, " (", DIGITOS_MATRICULA, ")" ))  
)
```

```
## Warning in plot.itemMatrix(smalltx, main = paste0("Elaborado por: ",
## NOMBRE_COMPLETO, : Use image() instead of plot().
```

![](proyecto-final-ar_files/figure-latex/unnamed-chunk-14-1.pdf)<!-- --> 

Visualiza el gráfico de frecuencia de items.

```r
try(
  itemFrequencyPlot(smalltx, topN=10, main = paste0("Elaborado por: ", NOMBRE_COMPLETO, " (", DIGITOS_MATRICULA, ")" ))
)
```

![](proyecto-final-ar_files/figure-latex/unnamed-chunk-15-1.pdf)<!-- --> 


Invoca el algoritmo **apriori** de `arules` con las transacciones de `smalltx` y asignando el resultado a `smallrules`.

```r
smallrules <- apriori(smalltx)
```

```
## Apriori
## 
## Parameter specification:
##  confidence minval smax arem  aval originalSupport maxtime support minlen
##         0.8    0.1    1 none FALSE            TRUE       5     0.1      1
##  maxlen target  ext
##      10  rules TRUE
## 
## Algorithmic control:
##  filter tree heap memopt load sort verbose
##     0.1 TRUE TRUE  FALSE TRUE    2    TRUE
## 
## Absolute minimum support count: 0 
## 
## set item appearances ...[0 item(s)] done [0.00s].
## set transactions ...[6 item(s), 5 transaction(s)] done [0.00s].
## sorting and recoding items ... [6 item(s)] done [0.00s].
## creating transaction tree ... done [0.00s].
## checking subsets of size 1 2 3 4 5 done [0.00s].
## writing ... [46 rule(s)] done [0.00s].
## creating S4 object  ... done [0.00s].
```

```r
#parameter = list(support = 0.01, confidence = 0.01)
```

Menciona los valores por omisión de algoritmo apriori de la librería arules:

## NOTE-12:support (mínimo) -> Por defecto es 0.1
## NOTE-13:confidence (mínimo) ->  Por defecto 0.8
## NOTE-14:items (máximo) -> Por defecto 10
## NOTE-15: tiempo de verificación de subsets (máximo) -> Por defecto 5 segundos.
## Referencia 
## https://www.cienciadedatos.net/documentos/43_reglas_de_asociacion


Invoca `summary()` con `smallrules`, consulta los resultados en **R Console** y responde a las preguntas de abajo. 

Tip: Para mayor legibilidad emplea el comando **Show in new window**


```r
summary(smallrules)
```

```
## set of 46 rules
## 
## rule length distribution (lhs + rhs):sizes
##  1  2  3  4  5 
##  1  4 18 18  5 
## 
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   1.000   3.000   3.500   3.478   4.000   5.000 
## 
## summary of quality measures:
##     support         confidence        coverage           lift      
##  Min.   :0.2000   Min.   :0.8000   Min.   :0.2000   Min.   :1.000  
##  1st Qu.:0.2000   1st Qu.:1.0000   1st Qu.:0.2000   1st Qu.:1.250  
##  Median :0.2000   Median :1.0000   Median :0.2000   Median :1.667  
##  Mean   :0.2478   Mean   :0.9957   Mean   :0.2522   Mean   :1.779  
##  3rd Qu.:0.2000   3rd Qu.:1.0000   3rd Qu.:0.2000   3rd Qu.:2.500  
##  Max.   :0.8000   Max.   :1.0000   Max.   :1.0000   Max.   :2.500  
##      count      
##  Min.   :1.000  
##  1st Qu.:1.000  
##  Median :1.000  
##  Mean   :1.239  
##  3rd Qu.:1.000  
##  Max.   :4.000  
## 
## mining info:
##     data ntransactions support confidence
##  smalltx             5     0.1        0.8
```

## NOTE-16:Cantidad de reglas producidas -> 46
## NOTE-17:Media aritmética de support -> 0.2000
## NOTE-18: Media aritmética de confidence -> 1.0000

Inspecciona `smallrules` y responde a las preguntas a continuación.

```r
inspect(smallrules)
```

```
##      lhs                           rhs       support confidence coverage
## [1]  {}                         => {diapers} 0.8     0.8        1.0     
## [2]  {coffee}                   => {diapers} 0.4     1.0        0.4     
## [3]  {milk}                     => {nuts}    0.4     1.0        0.4     
## [4]  {milk}                     => {eggs}    0.4     1.0        0.4     
## [5]  {beer}                     => {diapers} 0.6     1.0        0.6     
## [6]  {coffee,milk}              => {nuts}    0.2     1.0        0.2     
## [7]  {coffee,nuts}              => {milk}    0.2     1.0        0.2     
## [8]  {coffee,milk}              => {eggs}    0.2     1.0        0.2     
## [9]  {coffee,eggs}              => {milk}    0.2     1.0        0.2     
## [10] {coffee,milk}              => {diapers} 0.2     1.0        0.2     
## [11] {diapers,milk}             => {coffee}  0.2     1.0        0.2     
## [12] {beer,coffee}              => {diapers} 0.2     1.0        0.2     
## [13] {coffee,nuts}              => {eggs}    0.2     1.0        0.2     
## [14] {coffee,eggs}              => {nuts}    0.2     1.0        0.2     
## [15] {coffee,nuts}              => {diapers} 0.2     1.0        0.2     
## [16] {coffee,eggs}              => {diapers} 0.2     1.0        0.2     
## [17] {milk,nuts}                => {eggs}    0.4     1.0        0.4     
## [18] {eggs,milk}                => {nuts}    0.4     1.0        0.4     
## [19] {eggs,nuts}                => {milk}    0.4     1.0        0.4     
## [20] {diapers,milk}             => {nuts}    0.2     1.0        0.2     
## [21] {diapers,milk}             => {eggs}    0.2     1.0        0.2     
## [22] {beer,nuts}                => {diapers} 0.2     1.0        0.2     
## [23] {beer,eggs}                => {diapers} 0.2     1.0        0.2     
## [24] {coffee,milk,nuts}         => {eggs}    0.2     1.0        0.2     
## [25] {coffee,eggs,milk}         => {nuts}    0.2     1.0        0.2     
## [26] {coffee,eggs,nuts}         => {milk}    0.2     1.0        0.2     
## [27] {coffee,milk,nuts}         => {diapers} 0.2     1.0        0.2     
## [28] {coffee,diapers,milk}      => {nuts}    0.2     1.0        0.2     
## [29] {coffee,diapers,nuts}      => {milk}    0.2     1.0        0.2     
## [30] {diapers,milk,nuts}        => {coffee}  0.2     1.0        0.2     
## [31] {coffee,eggs,milk}         => {diapers} 0.2     1.0        0.2     
## [32] {coffee,diapers,milk}      => {eggs}    0.2     1.0        0.2     
## [33] {coffee,diapers,eggs}      => {milk}    0.2     1.0        0.2     
## [34] {diapers,eggs,milk}        => {coffee}  0.2     1.0        0.2     
## [35] {coffee,eggs,nuts}         => {diapers} 0.2     1.0        0.2     
## [36] {coffee,diapers,nuts}      => {eggs}    0.2     1.0        0.2     
## [37] {coffee,diapers,eggs}      => {nuts}    0.2     1.0        0.2     
## [38] {diapers,eggs,nuts}        => {coffee}  0.2     1.0        0.2     
## [39] {diapers,milk,nuts}        => {eggs}    0.2     1.0        0.2     
## [40] {diapers,eggs,milk}        => {nuts}    0.2     1.0        0.2     
## [41] {diapers,eggs,nuts}        => {milk}    0.2     1.0        0.2     
## [42] {coffee,eggs,milk,nuts}    => {diapers} 0.2     1.0        0.2     
## [43] {coffee,diapers,milk,nuts} => {eggs}    0.2     1.0        0.2     
## [44] {coffee,diapers,eggs,milk} => {nuts}    0.2     1.0        0.2     
## [45] {coffee,diapers,eggs,nuts} => {milk}    0.2     1.0        0.2     
## [46] {diapers,eggs,milk,nuts}   => {coffee}  0.2     1.0        0.2     
##      lift     count
## [1]  1.000000 4    
## [2]  1.250000 2    
## [3]  1.666667 2    
## [4]  1.666667 2    
## [5]  1.250000 3    
## [6]  1.666667 1    
## [7]  2.500000 1    
## [8]  1.666667 1    
## [9]  2.500000 1    
## [10] 1.250000 1    
## [11] 2.500000 1    
## [12] 1.250000 1    
## [13] 1.666667 1    
## [14] 1.666667 1    
## [15] 1.250000 1    
## [16] 1.250000 1    
## [17] 1.666667 2    
## [18] 1.666667 2    
## [19] 2.500000 2    
## [20] 1.666667 1    
## [21] 1.666667 1    
## [22] 1.250000 1    
## [23] 1.250000 1    
## [24] 1.666667 1    
## [25] 1.666667 1    
## [26] 2.500000 1    
## [27] 1.250000 1    
## [28] 1.666667 1    
## [29] 2.500000 1    
## [30] 2.500000 1    
## [31] 1.250000 1    
## [32] 1.666667 1    
## [33] 2.500000 1    
## [34] 2.500000 1    
## [35] 1.250000 1    
## [36] 1.666667 1    
## [37] 1.666667 1    
## [38] 2.500000 1    
## [39] 1.666667 1    
## [40] 1.666667 1    
## [41] 2.500000 1    
## [42] 1.250000 1    
## [43] 1.666667 1    
## [44] 1.666667 1    
## [45] 2.500000 1    
## [46] 2.500000 1
```


## NOTE-19:¿Cuál es la interpretación de la regla {} => {diapers} en la que se aprecia la ausencia del LHS?  --- > tiene eñ soporte y la confianza alto , la probalidad de que compren ese producto es de un 80%

## NOTE-20:Calcula el valor de la métrica support para la regla {beer} => {nuts}  --> 0.2

## NOTE-21: Calcula el valor de la métrica confidence para la regla {beer} => {nuts}  --> soporte{A, B} / soporte {A} => 0.33

## NOTE-22: ¿Por qué no aparece listada la regla {beer} => {nuts}? --> porque el minimo para la confianza es de 0.8 , esta regla no cumple con el minimo.



## MSSD: El dataset de sesiones de streaming
[The Music Streaming Sessions Dataset](https://research.atspotify.com/publications/the-music-streaming-sessions-dataset-short-paper/) fue desarrollado por Spotify para promover la investigación en modelado de escucha por parte de usuarios, interacciones en streaming, recuperación de información musical (MIR, por sus siglas en inglés) y recomendaciones con base a sesiones secuenciales.

Analiza una extracción del dataset MSSD por medio de reglas de asociación.

Instala el paquete [dplyr](https://dplyr.tidyverse.org/) para llevar preparar los datos de MSSD de tal forma que puedan ser analizados por medio de  `arules`.

**CODE-06** Instala el paquete dplyr

```r
#TODO: Retira los comentarios la primera vez para instalar los paquetes.
#install.packages("dplyr")
```

Carga la librería dplyr.

```r
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:arules':
## 
##     intersect, recode, setdiff, setequal, union
```

```
## The following object is masked from 'package:git2r':
## 
##     pull
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

Lee el dataset MSSD y verifica su estructura.

```r
mssddf <- read.csv(url("https://storage.googleapis.com/data-mining-202104/mssd-log_mini.csv")) #../../../data/mssd-log_mini.csv
str(mssddf)
```

```
## 'data.frame':	167880 obs. of  21 variables:
##  $ session_id                     : chr  "0_00006f66-33e5-4de7-a324-2d18e439fc1e" "0_00006f66-33e5-4de7-a324-2d18e439fc1e" "0_00006f66-33e5-4de7-a324-2d18e439fc1e" "0_00006f66-33e5-4de7-a324-2d18e439fc1e" ...
##  $ session_position               : int  1 2 3 4 5 6 7 8 9 10 ...
##  $ session_length                 : int  20 20 20 20 20 20 20 20 20 20 ...
##  $ track_id_clean                 : chr  "t_0479f24c-27d2-46d6-a00c-7ec928f2b539" "t_9099cd7b-c238-47b7-9381-f23f2c1d1043" "t_fc5df5ba-5396-49a7-8b29-35d0d28249e0" "t_23cff8d6-d874-4b20-83dc-94e450e8aa20" ...
##  $ skip_1                         : chr  "false" "false" "false" "false" ...
##  $ skip_2                         : chr  "false" "false" "false" "false" ...
##  $ skip_3                         : chr  "false" "false" "false" "false" ...
##  $ not_skipped                    : chr  "true" "true" "true" "true" ...
##  $ context_switch                 : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ no_pause_before_play           : int  0 1 1 1 1 1 1 1 1 1 ...
##  $ short_pause_before_play        : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ long_pause_before_play         : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ hist_user_behavior_n_seekfwd   : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ hist_user_behavior_n_seekback  : int  0 0 0 0 0 0 0 0 0 0 ...
##  $ hist_user_behavior_is_shuffle  : chr  "true" "true" "true" "true" ...
##  $ hour_of_day                    : int  16 16 16 16 16 16 16 16 16 16 ...
##  $ date                           : chr  "2018-07-15" "2018-07-15" "2018-07-15" "2018-07-15" ...
##  $ premium                        : chr  "true" "true" "true" "true" ...
##  $ context_type                   : chr  "editorial_playlist" "editorial_playlist" "editorial_playlist" "editorial_playlist" ...
##  $ hist_user_behavior_reason_start: chr  "trackdone" "trackdone" "trackdone" "trackdone" ...
##  $ hist_user_behavior_reason_end  : chr  "trackdone" "trackdone" "trackdone" "trackdone" ...
```

Inspecciona la columna `session_id`.

```r
summary(mssddf$session_id)
```

```
##    Length     Class      Mode 
##    167880 character character
```

```r
head(mssddf$session_id)
```

```
## [1] "0_00006f66-33e5-4de7-a324-2d18e439fc1e"
## [2] "0_00006f66-33e5-4de7-a324-2d18e439fc1e"
## [3] "0_00006f66-33e5-4de7-a324-2d18e439fc1e"
## [4] "0_00006f66-33e5-4de7-a324-2d18e439fc1e"
## [5] "0_00006f66-33e5-4de7-a324-2d18e439fc1e"
## [6] "0_00006f66-33e5-4de7-a324-2d18e439fc1e"
```

```r
length(unique(mssddf$session_id))
```

```
## [1] 10000
```

Inspecciona la columna `track_id_clean`.

```r
summary(mssddf$track_id_clean)
```

```
##    Length     Class      Mode 
##    167880 character character
```

```r
head(mssddf$track_id_clean)
```

```
## [1] "t_0479f24c-27d2-46d6-a00c-7ec928f2b539"
## [2] "t_9099cd7b-c238-47b7-9381-f23f2c1d1043"
## [3] "t_fc5df5ba-5396-49a7-8b29-35d0d28249e0"
## [4] "t_23cff8d6-d874-4b20-83dc-94e450e8aa20"
## [5] "t_64f3743c-f624-46bb-a579-0f3f9a07a123"
## [6] "t_c815228b-3212-4f9e-9d4f-9cb19b248184"
```

```r
length(unique(mssddf$track_id_clean))
```

```
## [1] 50704
```

Consulta el paper [The Music Streaming Sessions Dataset](https://arxiv.org/abs/1901.09851) y responde a las siguientes preguntas:

## NOTE-23:¿Cuál es el propósito de session id? ->Es un identificador de sesión único

## NOTE-24:¿Cuál es el propósito de track id? -> Es un identificador de pista único

## NOTE-25:¿Consideras que las otras columnas del dataset son aplicables para análisis por medio de reglas de asociación, justifica tu respuesta? -> Sí, estás columnas contienen datos y números como lo hemos visto anteriormente en las canasta de mercado, considero que sería el mismo procedimiento. Quitando los datos booleanos.


Inspecciona la estructura de `y`, el dataset preparado.

```r
x <- mssddf %>% select(session_id, track_id_clean) %>% group_by(session_id)
y <- as.data.frame(x)
str(y)
```

```
## 'data.frame':	167880 obs. of  2 variables:
##  $ session_id    : chr  "0_00006f66-33e5-4de7-a324-2d18e439fc1e" "0_00006f66-33e5-4de7-a324-2d18e439fc1e" "0_00006f66-33e5-4de7-a324-2d18e439fc1e" "0_00006f66-33e5-4de7-a324-2d18e439fc1e" ...
##  $ track_id_clean: chr  "t_0479f24c-27d2-46d6-a00c-7ec928f2b539" "t_9099cd7b-c238-47b7-9381-f23f2c1d1043" "t_fc5df5ba-5396-49a7-8b29-35d0d28249e0" "t_23cff8d6-d874-4b20-83dc-94e450e8aa20" ...
```

Descarga la librería `dplyr`

```r
#Descargamos dplyr porque enmascara otras funciones y no es requerida en lo sucesivo 
detach(package:dplyr)
```

Genera la representación de transacciones para `arules` del dataset MSSD y visualiza la salida **R Console**.

```r
mssdtx <- as(split(y[,"track_id_clean"], y["session_id"]), "transactions")
```

```
## Warning in asMethod(object): removing duplicated items in transactions
```

```r
#Trabajaremos con una muestra del 80% de las transacciones
numSessions <- round(nrow(mssdtx) * 0.8)
set.seed(DIGITOS_MATRICULA)
mssdtx <- sample(mssdtx, numSessions)
summary(mssdtx)
```

```
## transactions as itemMatrix in sparse format with
##  8000 rows (elements/itemsets/transactions) and
##  50704 columns (items) and a density of 0.0002883081 
## 
## most frequent items:
## t_bacf06d3-9185-4183-84ea-ff0db51475ce t_5718ab08-3a15-4d3f-9e63-42b2f6805e31 
##                                    878                                    595 
## t_8c4d29b1-e0bf-464c-88f7-ac19240cbba0 t_77b02acb-1b1f-4b36-b8fc-2c3e01892b9a 
##                                    539                                    499 
## t_a66ea088-b357-449a-8a1e-64dd0b8d6cb5                                (Other) 
##                                    494                                 113942 
## 
## element (itemset/transaction) length distribution:
## sizes
##    2    3    4    5    6    7    8    9   10   11   12   13   14   15   16   17 
##   19   15   38   93  128  182  249  325  658  596  550  444  474  427  493  385 
##   18   19   20 
##  830  514 1580 
## 
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##    2.00   11.00   15.00   14.62   19.00   20.00 
## 
## includes extended item information - examples:
##                                   labels
## 1 t_00007fba-6bd3-449d-85dd-54d4aea397c2
## 2 t_0000dc06-0c00-4a09-9dc6-3bdad9c6f0e8
## 3 t_00020dc1-1b82-43e9-8327-77b074bdf626
## 
## includes extended transaction information - examples:
##                               transactionID
## 5931 0_08aefed7-f24c-4221-979f-1c88ea7e1a79
## 6679 0_09c77550-7c4f-4d78-b4bb-630bbe9d33a8
## 9118 0_0d672e83-e7bf-40cf-a1ed-f904ae297675
```
Responde a las siguientes preguntas:

## NOTE-26: ¿Cuál es el ID y frecuencia absoluta de la sesión más frecuente? -> t_bacf06d3-9185-4183-84ea-ff0db51475ce 
## frec 878 

## NOTE-27:¿Cuántos items tiene el itemset con mayor número de ocurrencias en la lista element length distribution? -> 5

## NOTE-28:  En el contexto de este dataset con información de sesiones de streaming, ¿cuál es la interpretación de la media aritmética? -> una gran parte ocupa streaming



Ejecuta el algoritmo apriori con los parámetros por omisión.

```r
rules <- apriori(mssdtx)
```

```
## Apriori
## 
## Parameter specification:
##  confidence minval smax arem  aval originalSupport maxtime support minlen
##         0.8    0.1    1 none FALSE            TRUE       5     0.1      1
##  maxlen target  ext
##      10  rules TRUE
## 
## Algorithmic control:
##  filter tree heap memopt load sort verbose
##     0.1 TRUE TRUE  FALSE TRUE    2    TRUE
## 
## Absolute minimum support count: 800 
## 
## set item appearances ...[0 item(s)] done [0.00s].
## set transactions ...[43218 item(s), 8000 transaction(s)] done [0.07s].
## sorting and recoding items ... [1 item(s)] done [0.00s].
## creating transaction tree ... done [0.00s].
## checking subsets of size 1 done [0.00s].
## writing ... [0 rule(s)] done [0.00s].
## creating S4 object  ... done [0.00s].
```

```r
summary(rules)
```

```
## set of 0 rules
```

## NOTE-29: 
##  a) ¿Qué factores están asociados con que el algoritmo apriori con parámetros por omisión no haya producido ninguna regla de asociación, justifica tu respuesta? -> entre más alto sea el nivel de soporte más confiable se vuelve y más frecuentes . Pero si el valor da muy alto no entra dentro de las transacciones de datos

## B) ¿Qué riesgos identificas con la asignación de valores muy bajos en los parámetros de support y confidence al invocar el algoritmo apriori? --> Que en estas ocaciones no se considerada un item frecuente y no tiene  la confianza mínima que debe de tener una regla para ser incluida en los resultados. 

**CODE-07** Invoca `apriori`
Configura la invocación a `apriori` para producir al menos 220 reglas de asociación para ello puedes considerar el uso de los parámetros `support` y `confidence` como se muestra a continuación
`rules <- apriori(mssdtx, parameter = list(support = SUPP, confidence = CONF))`


```r
#TODO: Configura las variables SUPP y CONF para generar al menos 250 reglas de asociación
SUPP = 0.01
CONF = 0.01
rules <- apriori(mssdtx, parameter = list(support = SUPP, confidence = CONF))
```

```
## Apriori
## 
## Parameter specification:
##  confidence minval smax arem  aval originalSupport maxtime support minlen
##        0.01    0.1    1 none FALSE            TRUE       5    0.01      1
##  maxlen target  ext
##      10  rules TRUE
## 
## Algorithmic control:
##  filter tree heap memopt load sort verbose
##     0.1 TRUE TRUE  FALSE TRUE    2    TRUE
## 
## Absolute minimum support count: 80 
## 
## set item appearances ...[0 item(s)] done [0.00s].
## set transactions ...[43218 item(s), 8000 transaction(s)] done [0.06s].
## sorting and recoding items ... [115 item(s)] done [0.00s].
## creating transaction tree ... done [0.00s].
## checking subsets of size 1 2 3 4 5 6 7 8 9 10
```

```
## Warning in apriori(mssdtx, parameter = list(support = SUPP, confidence = CONF)):
## Mining stopped (maxlen reached). Only patterns up to a length of 10 returned!
```

```
##  done [0.03s].
## writing ... [298129 rule(s)] done [0.04s].
## creating S4 object  ... done [0.09s].
```

```r
summary(rules)
```

```
## set of 298129 rules
## 
## rule length distribution (lhs + rhs):sizes
##     1     2     3     4     5     6     7     8     9    10 
##   115   812  3918 12128 27545 47160 63028 64648 50085 28690 
## 
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##   1.000   6.000   7.000   7.308   9.000  10.000 
## 
## summary of quality measures:
##     support          confidence        coverage            lift      
##  Min.   :0.01000   Min.   :0.0100   Min.   :0.01000   Min.   : 1.00  
##  1st Qu.:0.01050   1st Qu.:0.9175   1st Qu.:0.01075   1st Qu.:17.15  
##  Median :0.01112   Median :0.9881   Median :0.01175   Median :22.38  
##  Mean   :0.01170   Mean   :0.9443   Mean   :0.01296   Mean   :22.74  
##  3rd Qu.:0.01225   3rd Qu.:1.0000   3rd Qu.:0.01325   3rd Qu.:25.48  
##  Max.   :0.10975   Max.   :1.0000   Max.   :1.00000   Max.   :48.19  
##      count       
##  Min.   : 80.00  
##  1st Qu.: 84.00  
##  Median : 89.00  
##  Mean   : 93.62  
##  3rd Qu.: 98.00  
##  Max.   :878.00  
## 
## mining info:
##    data ntransactions support confidence
##  mssdtx          8000    0.01       0.01
```





```r
try(
  plot(rules, main=paste0("Elaborado por: ", NOMBRE_COMPLETO, " (", DIGITOS_MATRICULA, ")" ))
)
```

```
## To reduce overplotting, jitter is added! Use jitter = 0 to prevent jitter.
```

![](proyecto-final-ar_files/figure-latex/unnamed-chunk-29-1.pdf)<!-- --> 
Inspecciona las reglas con mayor lift

```r
inspect(head(sort(rules, by = "lift")))
```

```
##     lhs                                         rhs                                       support confidence coverage     lift count
## [1] {t_52fc9bcf-ce50-43fc-9498-c2c8421a33e7,                                                                                        
##      t_78b908e3-62db-4021-ac90-2ed146bcea8d,                                                                                        
##      t_d0c22c8c-7969-4d17-bf0d-d4fa3c04a768} => {t_c0de25db-8c01-4122-9991-27132e852a18} 0.010875          1 0.010875 48.19277    87
## [2] {t_32270005-26a1-4763-8ba5-44fcc15f9914,                                                                                        
##      t_52fc9bcf-ce50-43fc-9498-c2c8421a33e7,                                                                                        
##      t_d0c22c8c-7969-4d17-bf0d-d4fa3c04a768} => {t_c0de25db-8c01-4122-9991-27132e852a18} 0.010875          1 0.010875 48.19277    87
## [3] {t_29a3895e-2c91-49a6-9383-6a71c597390d,                                                                                        
##      t_52fc9bcf-ce50-43fc-9498-c2c8421a33e7,                                                                                        
##      t_d0c22c8c-7969-4d17-bf0d-d4fa3c04a768} => {t_c0de25db-8c01-4122-9991-27132e852a18} 0.011250          1 0.011250 48.19277    90
## [4] {t_52fc9bcf-ce50-43fc-9498-c2c8421a33e7,                                                                                        
##      t_a66ea088-b357-449a-8a1e-64dd0b8d6cb5,                                                                                        
##      t_d0c22c8c-7969-4d17-bf0d-d4fa3c04a768} => {t_c0de25db-8c01-4122-9991-27132e852a18} 0.010625          1 0.010625 48.19277    85
## [5] {t_52fc9bcf-ce50-43fc-9498-c2c8421a33e7,                                                                                        
##      t_77b02acb-1b1f-4b36-b8fc-2c3e01892b9a,                                                                                        
##      t_d0c22c8c-7969-4d17-bf0d-d4fa3c04a768} => {t_c0de25db-8c01-4122-9991-27132e852a18} 0.010250          1 0.010250 48.19277    82
## [6] {t_32270005-26a1-4763-8ba5-44fcc15f9914,                                                                                        
##      t_78b908e3-62db-4021-ac90-2ed146bcea8d,                                                                                        
##      t_d0c22c8c-7969-4d17-bf0d-d4fa3c04a768} => {t_c0de25db-8c01-4122-9991-27132e852a18} 0.011000          1 0.011000 48.19277    88
```

## Note 30:https://youtu.be/0CQ6IMV2Nwc

Graba y publica un video (puede ser un video publicado de forma privada en YouTube u otro servicio similar) de 3 a 5 minutos explicando lo siguiente:

- Nombre y carrera.
- Concepto de association rules.
- Diferencia entre los conceptos de transacciones y reglas.
- Estrategia seguida para descubrir las reglas de interés en el dataset MSSD de este proyecto.
- Interpretación de la primera regla listada en el chunk anterior con el código `inspect(head(sort(rules, by = "lift")))` aclarando qué significan los datos listados en lhs, rhs y la interpretación de todas sus métricas.
- Provee el enlace al video aquí: **NOTE-30**

## Entrega del proyecto final

- Verifica que el notebook corra de principio a fin sin errores y produzca los resultados esperados.
- Verifica que hayas realizado todas las actividades de programación indicadas con **CODE-nn**
- Verifica que hayas respondido a todas las preguntas **NOTE-nn** 
- Verifica que hayas producido, publicado y escrito el enlace al video en el notebook.
- Guarda el archivo de markup (rmd) de forma local.
- Genera el PDF del notebook por medio del comando Knit (ya sea directo a PDF o Word y posteriormente Save As/PDF).
- No se recibirán proyectos en formato PDF con texto libre y capturas de pantalla.
- Publica el código de tu notebook a tu respositorio de GitHub por medio de los comandos `commit` y `push`.
- Envía el documento PDF por medio de mensaje personal a @marciano en Slack `naylacommunity` junto con el enlace a tu repositorio de GitHub.

**Fecha límite de entrega: Sábado 5 de junio al cierre del día.**
