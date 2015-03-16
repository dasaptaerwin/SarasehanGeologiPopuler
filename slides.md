---
title: 'Geological analysis with open-source software: case Cikapundung River'
author: "Dasapta Erwin Irawan"
date: "16th March 2015"
output:
  beamer_presentation: default
  ioslides_presentation:
    fig_caption: yes
    toc: yes
    toc_depth: 2
  slidy_presentation: default
subtitle: 'Event: Sarasehan Geologi Populer, Badan Geologi Indonesia'
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(cache=TRUE)
```

## Pre-talk quiz

- Pls visit `www.govote.at` enter code 88 74 35, and answer three yes/no questions

# Part one: Introduction

## Slide license 

![Creative Commons](cc.png)

- need attribution (BY)
- for non-commercial purposes only (NC)
- can be copied, modified (SA, share-alike)


## A bit about me

Personal data

![Japan, 2009](foto.jpg)

+ Name: Dasapta Erwin Irawan

+ Job: Lecturer/researcher at Groundwater Engineering Program, ITB

+ Education (Geology, ITB): 1994-1998 (undergrad), 1999-2001 (Master), 2005-2009 (PhD) 

## A bit about me

Visiting program

+ Nov-Dec 2009 

    - Center for Environmental Remote Sensing (CeRES), Chiba University (2009)
    - Supervisor: Prof. Josaphat T.S. Sumantyo
    - Research: remote sensing for hydrological purposes

+ Feb 2014-Feb 2015 

    - Faculty of Agriculture and Environment, University of Sydney
    - Supervisor: Dr. Willem Vervoort
    - Research: hydrological modeling in R 

## A bit about me 

Research

- hydrogeology 
- hydrochemistry
- multivariate [statistical] analysis

## A bit about me

Media social

Website: 
- [R and Linux](http://erwinirawansblog.blogspot.com)
- [Writing](http://onlinewaterbook.wordpress.com)
- [SlideShare](http://slideshare.com/d_erwin_irawan)

Twitter: @dasaptaerwin

Email: d_erwin_irawan[at]yahoo[dot]com

# Skills in Geology

## Essential skills for geologist

![Essential skills for geologist](GeologySkills.png)

## Software skills (not free version)

- office: Microsoft Office (Word, Excel, ppt) -> annual subscription (from USD 10 per month)
- citation and referencing: EndNote -> from USD 250
- statistical: Minitab, SPSS, Statistica, Stata -> basic version from USD 700 (2012)        
- spatial / GIS: ArcGIS, Mapinfo, etc -> annual subscription (basic version from USD 100 per year)

Sources: 

- [Openwetware](http://openwetware.org/wiki/Guide_to_statistics_software)
- [ESRI](http://www.esri.com/software/arcgis/arcgis-for-desktop/pricing)        
        
## Software skills (free equivalent)        

- office: [OpenOffice](www.openoffice.org) or [LibreOffice](www.libreoffice.org)
- citation and referencing: [Zotero](www.zotero.org), [Mendeley](www.mendeley.org), etc
- statistical: [R](www.r-project.org) and [R Studio](www.rstudio.com), [Orange Data Mining](orange.biolab.si/), [PSPP](www.gnu.org/software/pspp), etc
- GIS: [QGIS](www.qgis.org), [GRASSGIS](grass.osgeo.org), R 

## Why open source?

- free as breathing
- mostly cross-platform (Linux, Mac, Win)
- strong community, hence rapid development
- supporting _reproducibility_

## What is _reproducibility_ in science?

Every step can be:

- re-do
- re-analysed and re-evaluate
- re-developed

## What is _reproducibility_ in science?

Those principles are applied to:

- data (items and locations)
- software used in the analyses:
    - each software has distinct feature and algorithm
    - what would happen if not everyone could purchase the software?

## End of part one

# Part two: Cikapundung case

## Slide license 

![Creative Commons](cc.png)

- need attribution (BY)
- for non-commercial purposes only (NC)
- can be copied, modified (SA, share-alike)


## Background

![Kepadatan penduduk di bantaran Cikapundung (panoramio.com)](ckp1.jpg)


## Background

+ Cikapundung has important roles:

  - is one of the major water source for Bandung Basin: WTP Dago Pakar = 40 L/sec
  - electrical generator (since 1923): 
      + PLTA Bengkok = 3 MW
      + PLTA Dago = 0.7 MW 

+ Drainase kota

## Background

![Some facts](Darul1.png)

## Background

Vast growth of settlements + landuse change -> declining water quality (both river and groundwater).

![High density spots (Distarcip, 2014)](Darul2.jpg)


## Background

![BOD and COD (BPLH, 2006)](BOD.png)

## What do we know so far?

+ There types of groundwater and river water interactions (Lubis and Puradimaja 2006)
    - isolated stream at Maribaya area (upstream)
    - effluent stream (or gaining stream) at Maribaya to Viaduct segment (Bandung central)
    - influent stream (or losing stream) from Viaduct to Dayeuhkolot
+ some facts of springs and seepages at isolated segment (Tanuwijaya 2014).

## What do we know so far?

![Model Lubis and Puradimaja, 2006](ModelLubis.jpg)

## What do we know so far?

+ Conceptual model to mimic the interactions (Darul et.al 2014^a^^b^)
+ It confirms the Lubis Model 

## What do we know so far?

![Model 1](Model1.jpg)

## What do we know so far?

![Model 2](Model2.jpg)


## Our question

+ Does water quality reflect the interactions?

## Our tools

+ [R](www.r-project.org)
+ Let's do some [simple] analyses

# Showing `pairs` analysis (`bi`variate analysis)

## Data format

+ variables or measurements in columns
+ cases or samples in rows 
+ no merged columns or rows
+ read also [Data is the new soil](http://goo.gl/F2mej7)

## Why `pairs` analysis 

+ equivalent to `correlation matrix`
+ the fastest way to see correlations between variables
+ pls bear in mind `correlation` does not always mean `causality`

## Our data

![Data points](Capture.JPG)

## Our data

- 295 samples 
- From five years periode (1997, 1998, 2007, 2011, 2012)

## Load
```{r, echo=TRUE, results='hide'}
# load data
data <- as.data.frame(read.csv("BandungData.csv",
                               header = TRUE))
attach(data)
```

## Data structure

```{r, echo=TRUE}
# data structure
str(data)
```

## `pairs` plot 1

```{r, echo=TRUE, results='hide'}
pairs(data)
```

## `pairs` plot 1

+ ugly, too small
+ no legend and axis
+ we need to tweak it: group the variables and change plot code

## `pairs` plot 1

```
pairs(group1,labels=colnames(group1),
      main="Physical parameter", 
      pch=21, bg=c("red", "blue")
      [unclass(data$type)],
      upper.panel=NULL)
legend(x=0.6, y=0.8, levels(data$type), 
        pt.bg=c("red", "blue"), 
        pch=21, bty="n", ncol=2, 
        horiz=F)
```

## Grouping variables

```{r, echo=TRUE}
# group data
# Data group 1: Physical parameters
group1 <- data[,c("x", "y", "elv", "aq", "ec", "ph",
                  "hard", "tds", "temp", "eh", "Q")]

# Data group 2: Cation
group2 <- data[,c("x", "y", "elv", "Ca",
                  "Mg", "Fe", "Mn", "K", "Na")]

## Data group 3: Anions (unit = ppm)
group3 = data[,c("x", "y", "CO3", "HCO3",
                 "CO2", "Cl", "SO4", "NO2",
                 "NO3", "SiO2")]
```



## `Pairs` plot group 1 (physical parameters)

```{r, echo=FALSE, fig.height=5.5, fig.width=5.5}
pairs(group1,labels=colnames(group1),
      main="Physical parameter", 
      pch=21, bg=c("red", "blue")
      [unclass(data$type)],
      upper.panel=NULL)
legend(x=0.6, y=0.8, levels(data$type), 
        pt.bg=c("red", "blue"), 
        pch=21, bty="n", ncol=2, 
        horiz=F)
```

## `Pairs` plot group 2 (cations )

```{r, echo=FALSE, fig.height=5.5, fig.width=5.5}
pairs(group2,labels=colnames(group2),main="Cations", 
      pch=21, bg=c("red", "blue")
      [unclass(data$type)],
      upper.panel=NULL)
legend(x=0.6, y=0.8, levels(data$type), 
       pt.bg=c("red", "blue"), 
       pch=21, bty="n", ncol=2, horiz=F)
```

## `Pairs` plot group 3 (anions )

```{r, echo=FALSE, fig.height=5.5, fig.width=5.5}
pairs(group3,labels=colnames(group3),main="Anions", 
      pch=21, bg=c("red", "blue")
      [unclass(data$type)],
      upper.panel=NULL)
legend(x=0.6, y=0.8, levels(data$type), 
       pt.bg=c("red", "blue"), 
       pch=21, bty="n", ncol=2, horiz=F)
```

# Showing `PCA` analysis (`multi`variate analysis)

## Why PCA (Principle Component Analysis)?

- nature embeds multivariable process
- has been widely used and developed since the 60's
- simple, straighforward, nearest neighbour (cluster) principles
- offers nice visualisation


## [Simple] codes

```
# install library
install.packages("pcaMethods") # for PCA
install.packages("gridExtra")  # for plot lay out

# load library
library(pcaMethods) # for PCA
library(gridExtra)  # for plot lay out

# run PCA
pca1 <- pca(group1,  
               method = "svdImpute", 
               scale = "uv",
               center = T,
               nPcs = 3,
               evalPcs = 1:3)
```

## [Simple] codes

```
# evaluate results
summary(pca1)     # result summary
sDev(pca1)        # extracting eigenvalues
plot(sDev(pca1))  # plotting eigenvalues
loadings(pca1)    # plot loadings
scores(pca1)      # plot scores
```


```{r, echo=FALSE, results='hide'}
# load library
library(pcaMethods) # calling pcaMethods package
library(gridExtra)
library(lattice)
```

```{r, echo=FALSE, results='hide'}
# Group data
group1 <- data[,c("distx", "type", "ec", "elv",  
                   "ph", "hard", "tds", "temp",
                   "eh", "cumrain", "lag1")]

group2 <- data[,c("distx", "type", "ec", "elv",
                  "aq", "Ca", "Mg", 
                  "Fe", "Mn", "K", 
                  "Na", "cumrain", "lag1")]

group3 <- data[,c("distx", "type", "ec", "elv",
                  "aq", "CO3", "HCO3", 
                  "CO2", "Cl", "SO4", 
                  "NO2", "NO3", "SiO2",  
                  "cumrain", "lag1")]

```

```{r, echo=FALSE, results='hide'}
# Run PCA 
## using svdImpute = standard pca, with imputation, standardised, method univariate (uv)
pca1 <- pca(group1, 
               method = "svdImpute", 
               scale = "uv",
               center = T,
               nPcs = 3,
               evalPcs = 1:3)

pca2 <- pca(group2, 
               method = "svdImpute", 
               scale = "uv",
               center = T,
               nPcs = 3,
               evalPcs = 1:3)

pca3 <- pca(group3, 
               method = "svdImpute", 
               scale = "uv",
               center = T,
               nPcs = 3,
               evalPcs = 1:3)

```

## Results: summary PCA1

```{r, echo=FALSE}
## Summary PCA1
summary(pca1)
```

## Results: summary PCA2

```{r, echo=FALSE}
summary(pca2)
```

## Results: summary PCA3

```{r, echo=FALSE}
summary(pca3)
```

## Results: Extract Eigenvalues PCA1

```{r, echo=FALSE, fig.height=5.5, fig.width=5.5}
eigen1 <- sDev(pca1)
eigen2 <- sDev(pca2)
eigen3 <- sDev(pca3)
par(mfrow=c(1,3))
matplot(eigen1, type = 'l', 
        xlab="Principal Component", 
        ylab="Variance", 
        lwd = 3) # plotting eigenvalues
matplot(eigen2, type = 'l', 
        xlab="Principal Component", 
        ylab="Variance", 
        lwd = 3) # plotting eigenvalues
matplot(eigen3, type = 'l', 
        xlab="Principal Component", 
        ylab="Variance", 
        lwd = 3) # plotting eigenvalues

```

## Results: plot PCA Group 1

```{r, echo=FALSE, fig.height=5.5, fig.width=5.5}
plotPcs(pca1, 
        col = group1$type)
legend("topright", 
       legend = c("groundwater", "river water"), 
       pch = "o",
       col = c("black", "red"))
```

## Results: plot PCA Group 2

```{r, echo=FALSE, fig.height=5.5, fig.width=5.5}
plotPcs(pca2, 
        col = group1$type)
legend("topright", 
       legend = c("groundwater", "river water"), 
       pch = "o",
       col = c("black", "red"))
```

## Results: plot PCA Group 3

```{r, echo=FALSE, fig.height=5.5, fig.width=5.5}
plotPcs(pca3, 
        col = group1$type)
legend("topright", 
       legend = c("groundwater", "river water"), 
       pch = "o",
       col = c("black", "red"))
```

## Results: loadings and scores Group1

```{r, echo=FALSE, fig.height=5.3, fig.width=5.3}
par(mfrow=c(1,2))
plot(loadings(pca1), 
     pch = 20,
     main = "Variable loadings",
     sub = "Group1")
text(loadings(pca1), 
     row.names(loadings(pca1)),
     cex=0.6, 
     pos=1, 
     col="red")
abline(v=0, h=0, 
       lty=2, 
       lwd=.1, 
       col = "gray70")

plot(scores(pca1), 
                pch = c(group1$type), 
                col = c(group1$type),
                main = "Case scores",
                sub = "Group1")
legend(x = "topleft", 
       c("Groundwater", "River Water"),
       title = "Water type:",
       cex=0.7, 
       pch = c(1, 2), 
       col = c("black", "red"))

abline(v=0, h=0, 
       lty=2, 
       lwd=.1, 
       col = "gray70")
```

## Results: loadings and scores Group2

```{r, echo=FALSE, fig.height=5.3, fig.width=5.3}
par(mfrow=c(1,2))
plot(loadings(pca2), 
     pch = 20,
     main = "Variable loadings",
     sub = "Group2")
text(loadings(pca2), 
     row.names(loadings(pca2)),
     cex=0.6, 
     pos=1, 
     col="red")
abline(v=0, h=0, 
       lty=2, 
       lwd=.1, 
       col = "gray70")

plot(scores(pca2), 
                pch = c(group2$type), 
                col = c(group2$type),
                main = "Case scores",
                sub = "Group2")
legend(x = "topright", 
       c("Groundwater", "River Water"),
       title = "Water type:",
       cex=0.6, 
       pch = c(1, 2), 
       col = c("black", "red"))

abline(v=0, h=0, 
       lty=2, 
       lwd=.1, 
       col = "gray70")
```

## Results: loadings and scores Group3

```{r, echo=FALSE, fig.height=5.3, fig.width=5.3}
par(mfrow=c(1,2))
plot(loadings(pca3), 
     pch = 20,
     main = "Variable loadings",
     sub = "Group3")
text(loadings(pca3), 
     row.names(loadings(pca3)),
     cex=0.6, 
     pos=1, 
     col="red")
abline(v=0, h=0, 
       lty=2, 
       lwd=.1, 
       col = "gray70")

plot(scores(pca3), 
                pch = c(group3$type), 
                col = c(group3$type),
                main = "Case scores",
                sub = "Group3")
legend(x = "topleft", 
       c("Groundwater", "River Water"),
       title = "Water type:",
       cex=0.7, 
       pch = c(1, 2), 
       col = c("black", "red"))

abline(v=0, h=0, 
       lty=2, 
       lwd=.1, 
       col = "gray70")
```

# spatial analysis (bubble plot)

## why bubble plot?

- shows spatial variation as well as values distribution
- simple and straigtforward visualisation

## [simple] codes

```
# load library (assuming all libraries are installed)
library(gstat)
library(sp)
library(rgdal)
library(latticeExtra)

# open and load data
df <- read.csv("BandungData.csv", header=TRUE)

# convert xy values as coordinates
coordinates(df) <- ~ x + y
```

## [simple] codes

```
# make bubble plot
bubbleCa <- bubble(data, zcol="Ca", 
                   xlab="X coord", ylab="Y coord",
                   main="Bubble plot Ca",
                   col = data$zone, 
                   scales=list(tck=0.5))
print(bubbleCa)
```

## Process

```{r, echo=FALSE, results='hide', fig.height=5.3, fig.width=5.3}
# load library
library(sp)
library(rgdal)
coordinates(data) <- ~ x + y

# bubble plot 
## groundwater end member 
bubbleCa <- bubble(data, zcol="Ca", 
                   xlab="X coord", ylab="Y coord",
                   main="Bubble plot Ca",
                   scales=list(tck=0.5))

bubbleMg <- bubble(data, zcol="Mg", 
                   xlab="X coord", ylab="Y coord",
                   main="Bubble plot Mg",
                   scales=list(tck=0.5))

## river water end member 
bubbleNa <- bubble(data, zcol="Na", 
                   xlab="X coord", ylab="Y coord",
                   main="Bubble plot Na",
                   scales=list(tck=0.5))
bubbleK <- bubble(data, zcol="K", 
                  xlab="X coord", ylab="Y coord",
                  main="Bubble plot K",
                  scales=list(tck=0.5))

# contamination signature
bubbleNO3 <- bubble(data, zcol="NO3", 
                    xlab="X coord", ylab="Y coord",
                    main="Bubble plot NO3",
                    scales=list(tck=0.5))

bubbleNO2 <- bubble(data, zcol="NO2", 
                    xlab="X coord", ylab="Y coord",
                    main="Bubble plot NO2",
                    scales=list(tck=0.5))
bubbleSO4 <- bubble(data, zcol="SO4", 
                    xlab="X coord", ylab="Y coord",
                    main="Bubble plot SO4",
                    scales=list(tck=0.5))
bubbleCl <- bubble(data, zcol="Cl", 
                   xlab="X coord", ylab="Y coord",
                   main="Bubble plot Cl",
                   scales=list(tck=0.5))

```

## Results: groundwater end member (Ca)

```{r, echo=FALSE, fig.height=5, fig.width=5}
print(bubbleCa)
```

## Results: groundwater end member (Mg)

```{r, echo=FALSE, fig.height=5, fig.width=5}
print(bubbleMg)
```

## Results: river end member (Na)

```{r, echo=FALSE, fig.height=5, fig.width=5}
print(bubbleNa)
```

## Results: river end member (K)

```{r, echo=FALSE, fig.height=5, fig.width=5}
print(bubbleK)
```


## Results: contamination signature (NO~3~)

```{r, echo=FALSE, fig.height=5, fig.width=5}
print(bubbleNO3)
```

## Results: contamination signature (NO~2~)

```{r, echo=FALSE, fig.height=5, fig.width=5}
print(bubbleNO2)
```


## Results: contamination signature (SO~4~)

```{r, echo=FALSE, fig.height=5, fig.width=5}
print(bubbleSO4)
```

## Results: contamination signature (Cl)

```{r, echo=FALSE, fig.height=5, fig.width=5}
print(bubbleCl)
```

## Remarks

- higher mineral concentration in river water than groundwater should have occured in effluent flow.
- higher mineral concentration in groundwater than river water should have occured in influent flow.
- both natural indications are not detected, except for NO~2~.

## Remarks

- the anomaly is due to dilution effect.
- dilution overides enrichment effect.
- the opposite would happen if sampling is conducted in dry season.
- possibility of different catchment between groundwater and river water. 

## Closing

Future research opportunities:

- to add more data in different locations along river bank, taken in both rain and dry season.
- more exploratory statistical analysis, eg: multiple regression tree to extract data pattern.

## Main references

- Lubis, RF and Puradimaja, DJ, 2006, Hydrodynamic relationsships between groundwater and river water: CIkapundung river stream, West Java, Indonesia
- Darul, A, Irawan, DE, and Trilaksono, NJ, 2014^a^, Groundwater and river water interaction on Cikapundung River: Revisited, International Conference on Math and Natural Sciences, ITB.
- Darul, A, 2014^b^, Model konseptual interaksi air tanah dan air sungai di bantaran S. Cikapundung, Bandung, Jawa Barat, Tesis S2, Supervisor: Dr. Dasapta Erwin Irawan dan Dr. Nurjanna Joko Trilaksono.
- Tanuwijaya, ZAJ, 2014, Identifikasi interaksi air sungai dan air tanah di DAS Cikapundung, Disertasi, Geologi Universitas Padjadjaran.   

## These slides were made using open-source tools

- Ubuntu Linux (14.04)
- R 
- Dia flowcharter
- Gimp image editor

## More resources

- [Me on Wordpress](http://onlinewaterbook.wordpress.com)
- [Me on Blogger](http://erwinirawansblog.blogspot.com)





