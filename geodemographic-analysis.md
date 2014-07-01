---
title: "Using R for Geodemographic Analysis"
author: "Nick Bearman"
output: html_document
---

R Basics
---

R began as a statistics program, and is still used as one my many users. At a simple level you can type in "3 + 4", press return, and R will respond "7". The code you type in in this tutorial is shown like this:


```r
3 + 4
```

And R's output is shown like this: 


```
[1] 7
```

R has developed into a GIS as a result of user contributed packages, or Libaries, as R refers to them. We will be using several libaries in this practical, and will load them as necessary. 

_If you are using this worksheet outside of the course, you may need to install the R libaries as well as loading them. To do this, run "install.package("package\_name")._

We won't spend too much time on the basics of using R - if you want to find out more, there are some good tutorials at http://www.social-statistics.org/?p=764 or http://rpubs.com/nickbearman/gettingstartedwithr. 

We are going to use a program called R Studio, which works on top of R and provides a good user interface. I'll talk a little bit about it in the presentation, but the key areas of the window are these:

(Image of R Studio, different bits highlihgted, console, files/plots, environment, R script)

Open up R Studio (Start > All Programs > R Studio > R Studio) and arrange the windows so you can see the instructions along side R Studio. 

Index Scores
---

Index scores are an important part of geodemographic classification as they show how the rate of a particular characteristic is for that particular geogrmographic group compared to the national average. 

For example, the table below shows index scores for broadsheet and tabliod newspaper readership. A score of 100 indicates an OAC SuperGroup where newspaper readership is the same as the national average. The City Living SuperGroup have a score of 144 for broadsheet newspapers, which means the readership of broadsheet newspapers for them is 44% higher than the national average. The Blue Collar Communities SuperGroup have an index score of 73.2, which means their readership of broadsheet newspapers is 21.8% lower (100-73.2 = 21.8) than the national average. 

**Table 1: Index Scores for broadsheet and tabliod newspaper readership**
<!-- html table generated in R 3.0.1 by xtable 1.7-1 package -->
<!-- Fri Jun 27 14:50:17 2014 -->
<TABLE border=1>
<TR> <TH> OAC SuperGroup </TH> <TH> Broadsheet Index Score </TH> <TH> Tabloid Index Score </TH>  </TR>
  <TR> <TD> Blue Collar Communities </TD> <TD align="right"> 73.20 </TD> <TD align="right"> 110.80 </TD> </TR>
  <TR> <TD> City Living </TD> <TD align="right"> 144.00 </TD> <TD align="right"> 82.20 </TD> </TR>
  <TR> <TD> Countryside </TD> <TD align="right"> 103.90 </TD> <TD align="right"> 104.90 </TD> </TR>
  <TR> <TD> Prospering Suburbs </TD> <TD align="right"> 109.10 </TD> <TD align="right"> 94.50 </TD> </TR>
  <TR> <TD> Constrained by Circumstances </TD> <TD align="right"> 78.20 </TD> <TD align="right"> 108.40 </TD> </TR>
  <TR> <TD> Typical Traits </TD> <TD align="right"> 97.10 </TD> <TD align="right"> 96.40 </TD> </TR>
  <TR> <TD> Multicultural </TD> <TD align="right"> 120.20 </TD> <TD align="right"> 96.00 </TD> </TR>
   </TABLE>

We are going to make a graph of these values. We're using the ggplot2 function, but there are many different ways of doing. 

In the R Studio window, click + (img) and choose R Script (use Ctrl-Shift-N).

When writing code for R, as with any programming language, it is good to include comments - these are bits of text that R ignores, but we can use them to explain what is happening. This is useful if we pass our code on to someone else (so they know what the code is doing) and for ourselves, when we come back to a piece of code six months later and can't remember what it was for!

The code below creates a graph of the index scores. We are going to take this code, and alter it to show the Tabloid news paper data. Copy all the code in the section below and paste it into the R script window at the top.


```r
#Load libaries
library(scales)
library(ggplot2)
#set up data and data frame
oac_names <- c("Blue Collar Communities","City Living","Countryside","Prospering Suburbs","Constrained by Circumstances","Typical Traits","Multicultural")
broadsheets <- c(73.2, 144, 103.9, 109.1, 78.2, 97.1, 120.2)
oac_broadsheets <- data.frame(oac_names,broadsheets)
#convert the percentage values (e.g. 144%) to decimal increase or decrease (e.g. 0.44)
oac_broadsheets$broadsheets <-  broadsheets / 100 - 1
#select the colours we are going to use
my_colour <- c("#33A1C9","#FFEC8B","#A2CD5A","#CD7054","#B7B7B7","#9F79EE","#FCC08F")
#plot the graph - this has several bits to it           
  #the first three lines setup the data and type of graph
ggplot(oac_broadsheets, aes(oac_names, broadsheets)) + 
  geom_bar(stat = "identity", fill = my_colour, position="identity") + 
  theme(axis.text.x=element_text(angle=-90,hjust=0,vjust=0.5)) + 
  #this line add the lables to each bar
  geom_text(aes(label = paste(round(broadsheets * 100,digits = 0), "%"), vjust = ifelse(broadsheets >= 0, -0.5, 1.5)), size=3) +
  #these lines as the axis labels
  scale_y_continuous("Difference from national average for Broadsheet Newspapers",labels = percent_format()) +
  scale_x_discrete("OAC SuperGroups")
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 

This code uses the ggplot function to create a nice looking graph of the index scores (above). There is quite a lot going on in this code, but don't worry about trying to understand every command at this point. The key aspects are the `oac_broadsheets` data frame - this is where the data for the graph is stored. You can find out more about any R command by typing `?_command_` into the console. For example, `?ggplot`.

To run the code, either click the 'Run' button (top right) which will run all the code, or highlight the bit you want to run, and then press Ctrl-Enter on the keyboard (hold down Control and press Enter). R should create the graph, as shown in this document. If you get red error messages, check you have copied all of the code and not missed any bits out. 

Now, instead of Broadsheets we will create a graph for Tabloids. Firstly replace this line:


```r
broadsheets <- c(73.2, 144, 103.9, 109.1, 78.2, 97.1, 120.2)
```

with this


```r
tabloids <- c(110.8,82.2,104.9,94.5,108.4,96.4,96.0)
```

As we're doing tabloids now, we need to replace any instance of broadsheets with tabliods. 
Change 


```r
oac_broadsheets <- data.frame(oac_names,broadsheets)
```

to 


```r
oac_tabloids <- data.frame(oac_names,tabloids)
```

And do the same for the rest of the code.

You should end up with a graph like this:

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9.png) 


This is our new graph!

Geodemographic Map of Liverpool
---

2011
Cluster Data from http://www.opendataprofiler.com/2011OAC.aspx
2011 LSOA
https://geoportal.statistics.gov.uk/geoportal/catalog/main/home.page
Output areas (E+W) 2011 Boundaries (Full, Clipped)

2001 
OAC by OA
http://www.ons.gov.uk/ons/guide-method/geography/products/area-classifications/national-statistics-area-classifications/national-statistics-2001-area-classifications/datasets/output-areas/output-areas.html



#load library
library(maptools)
#read in shapefile
liverpool <- readShapeSpatial('geodemographics/liverpool', proj4string = CRS("+init=epsg:27700"))

Download
Read in CSV


```r
OAC <- read.csv("geodemographics/2011_EW_OAC_Clusters.csv",header=TRUE)
```

```
## Warning: cannot open file 'geodemographics/2011_EW_OAC_Clusters.csv': No
## such file or directory
```

```
## Error: cannot open the connection
```

```r
#OAC by OA
OAC <- read.csv("geodemographics/J31A0301_2101_GeoPolicy_NW_OA.csv",header=TRUE,skip=5)


#LSOA# OAC <- read.csv("geodemographics/J30A0301_1938_GeoPolicy.csv",header=TRUE,skip=5)
OAC <- subset(OAC, select = c("LSOA_CODE","LSOA_NAME","DATA_VALUE","DATA_VALUE.1","DATA_VALUE.2","DATA_VALUE.3","DATA_VALUE.4","DATA_VALUE.5"))
colnames(OAC) <- c("LSOA_CODE","LSOA_NAME","Supergroup Name","Supergroup Code","Group Name","Group Code","Subgroup Name","Subgroup Code")
```

Join

The next stage is to join this data onto the data slot of the OA object. This code uses the match() function to examine which rows in the OAC_diabetes_rates match with those in the OA@data. The matches are done using the OAC group name columns in both objects. If you print the first few rows of the data slot on the OA object you will see that this now contains the data you calculated above.


```r
#Join OAC classification on to LSOA shapefile
liverpool@data = data.frame(liverpool@data, OAC[match(liverpool@data[, "LSOA01CD"], OAC[,"LSOA_CODE"]),])
```

```
Error: object 'liverpool' not found
```

```r
#Show head of liverpool
head(liverpool@data)
```

```
Error: object 'liverpool' not found
```

Map of these

1. Blue collar communities
2. City Living
3. Countryside
4. Prospering suburbs
5. Constrained by circumstances
6. Typical traits
7. Multicultural
8. (Misc?)

Map of these
1. Rural Residents
2. Cosmopolitans
3. Ethnic Mix
4. Blue Collar Neighbourhoods
5. Multicultural Metropolitans
6. Suburbanites
7. Hard-Pressed Households
8. Urbanities



```r
#Define a set of colours, one for each of the OAC supergroups
my_colour <- c("#33A1C9","#FFEC8B","#A2CD5A","#CD7054","#B7B7B7","#9F79EE","#FCC08F")
oac_names <- c("Blue collar communities","City Living","Countryside","Prospering suburbs","Constrained by circumstances","Typical traits","Multicultural")

#oac_names <- c("Rural Residents","Cosmopolitans","Ethnic Mix","Blue Collar Neighbourhoods","Multicultural Metropolitans","Suburbanites","Hard-Pressed Households","Urbanities")
#my_colour <- c("darkgreen","red","hotpink","blue","orange","purple","gold","brown")

#Create a basic OAC choropleth map
plot(liverpool, col=my_colour[liverpool@data$Supergroup.Code], axes=FALSE,border = NA)
```

```
Error: object 'liverpool' not found
```

```r
#Add the legend (the oac_names object was created earlier)
legend(x=332210, y=385752, legend=oac_names, fill=my_colour, bty="n", cex=.8, ncol=1)
```

```
Error: plot.new has not been called yet
```

```r
#Add North Arrow
SpatialPolygonsRescale(layout.north.arrow(2), offset= c(332610,385852), scale = 1600, plot.grid=F)
```

```
Error: could not find function "SpatialPolygonsRescale"
```

```r
#Add Scale Bar
SpatialPolygonsRescale(layout.scale.bar(), offset= c(333210,381252), scale= 5000, fill= c("white", "black"), plot.grid= F)
```

```
Error: could not find function "SpatialPolygonsRescale"
```

```r
#Add text to scale bar 
text(333410,380952,"0km", cex=.8)
```

```
Error: plot.new has not been called yet
```

```r
text(333410 + 2500,380952,"2.5km", cex=.8)
```

```
Error: plot.new has not been called yet
```

```r
text(333410 + 5000,380952,"5km", cex=.8)
```

```
Error: plot.new has not been called yet
```

```r
#Add a title
title("OAC Group Map of Liverpool")
```

```
Error: plot.new has not been called yet
```

Lunch

Calc index scores, based on cross tab

include index scores for these. 

Creating an Index Score


Calc example for newspapers? Same as example below. 

Based on numbers of papers sold in diff areas (back calculation)
Avg no of papers 1000000 per day tabloid
100000 per day broadsheet.
 - get rough numbers. 
Variation by OAC group
Calc index scores


Newspapers example 

If we decide to open a corner shop in a particular location (X) then it might be useful to know what newspapers we are likley to sell most of. We know which OAC Group our corner shop falls into, and we know which classifications tend to read which newspapers, so we can combine the two together to work out which newspapers to stock. 

#This code reads in a file called newspapers_summary.csv
newspapers <- read.csv("geodemographics/newspapers_summary.csv",header=TRUE)

setwd("/Users/nickbearman/Dropbox/teaching/2014-liverpool/2014-r-google-workshop/practicals")


END

This is an R Markdown document. Markdown is a simple formatting syntax for authoring HTML, PDF, and MS Word documents. For more details on using R Markdown see <http://rmarkdown.rstudio.com>.

When you click the **Knit** button a document will be generated that includes both content as well as the output of any embedded R code chunks within the document. You can embed an R code chunk like this:


```r
summary(cars)
```

```
##      speed           dist    
##  Min.   : 4.0   Min.   :  2  
##  1st Qu.:12.0   1st Qu.: 26  
##  Median :15.0   Median : 36  
##  Mean   :15.4   Mean   : 43  
##  3rd Qu.:19.0   3rd Qu.: 56  
##  Max.   :25.0   Max.   :120
```

You can also embed plots, for example:

![plot of chunk unnamed-chunk-14](figure/unnamed-chunk-14.png) 

Note that the `echo = FALSE` parameter was added to the code chunk to prevent printing of the R code that generated the plot.
