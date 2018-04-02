---
title: "Sentiment Analysis: Wine Reviews"
author: "Esther Suh"
output: 
    html_document:
        keep_md: True
---

**Load Dataset**


```r
df <- read.csv("https://query.data.world/s/fsz4la3t7cjhixxhg2q2rmvtcu2tzm", header=TRUE, stringsAsFactors=FALSE);
str(df)
```

```
## 'data.frame':	2890 obs. of  32 variables:
##  $ id                  : chr  "AV13ClKCGV-KLJ3akN68" "AV13CsvW-jtxr-f38AQO" "AV13CVI_glJLPUi8O7Po" "AV13CVI_glJLPUi8O7Po" ...
##  $ asins               : chr  "" "" "" "" ...
##  $ brand               : chr  "Gallo" "Fresh Craft Co." "1000 Stories" "1000 Stories" ...
##  $ categories          : chr  "Food & Beverage,Beverages,Wine, Beer & Liquor,Wine" "Food & Beverage,Beverages,Wine, Beer & Liquor,Wine" "Food & Beverage,Beverages,Wine, Beer & Liquor,Wine" "Food & Beverage,Beverages,Wine, Beer & Liquor,Wine" ...
##  $ dateAdded           : chr  "2017-07-24T23:59:11Z" "2017-07-24T23:59:42Z" "2017-07-24T23:58:05Z" "2017-07-24T23:58:05Z" ...
##  $ dateUpdated         : chr  "2018-01-10T18:06:28Z" "2018-01-10T05:38:33Z" "2018-01-10T05:38:31Z" "2018-01-10T05:38:31Z" ...
##  $ descriptions        : chr  "" "[{\"dateSeen\":[\"2017-12-21T05:43:00.000Z\",\"2017-12-16T19:47:00.000Z\",\"2017-12-07T17:57:00.000Z\",\"2017-1"| __truncated__ "" "" ...
##  $ dimension           : chr  "1.0 in x 1.0 in x 1.0 in" "4.25 in x 4.25 in x 5.25 in" "3.3 in x 3.3 in x 11.79 in" "3.3 in x 3.3 in x 11.79 in" ...
##  $ ean                 : chr  "" "" "" "" ...
##  $ flavors             : chr  "" "" "" "" ...
##  $ keys                : chr  "492130001994,gallo/13312834" "freshcraft/50392800,083120003441" "082896001453,1000stories/50399893" "082896001453,1000stories/50399893" ...
##  $ manufacturer        : chr  "" "" "" "" ...
##  $ manufacturerNumber  : chr  "13312834" "50392800" "50399893" "50399893" ...
##  $ name                : chr  "Ecco Domani174 Pinot Grigio - 750ml Bottle" "Fresh Craft174 Mango Citrus - 4pk / 250ml Bottle" "1000 Stories174 Zinfandel - 750ml Bottle" "1000 Stories174 Zinfandel - 750ml Bottle" ...
##  $ reviews.date        : chr  "2017-08-01T21:13:49.000Z" "2017-07-10T04:17:34.000Z" "2017-10-02T12:52:27.000Z" "2016-11-02T10:00:37.000Z" ...
##  $ reviews.dateAdded   : chr  "2018-01-09T13:24:04Z" "2018-01-09T17:31:52Z" "2018-01-09T17:31:51Z" "2017-10-04T18:03:12Z" ...
##  $ reviews.dateSeen    : chr  "2017-12-14T19:41:00.000Z,2017-12-19T19:55:00.000Z,2017-12-06T01:40:00.000Z,2017-11-18T20:09:00.000Z,2017-10-26T"| __truncated__ "2017-12-17T05:32:00.000Z,2017-12-21T14:32:00.000Z,2017-12-08T04:46:00.000Z,2017-08-08T10:33:33.070Z,2017-07-23T"| __truncated__ "2017-12-17T05:30:00.000Z,2017-12-21T14:29:00.000Z,2017-12-08T04:50:00.000Z,2017-10-10T11:32:10.201Z" "2017-09-30T00:52:32.856Z,2017-08-08T10:33:31.624Z,2017-08-06T13:27:29.991Z,2017-08-04T13:22:24.342Z,2017-07-24T"| __truncated__ ...
##  $ reviews.didPurchase : logi  NA NA NA NA NA NA ...
##  $ reviews.doRecommend : logi  TRUE TRUE TRUE TRUE TRUE TRUE ...
##  $ reviews.id          : int  NA NA NA NA NA NA NA NA NA NA ...
##  $ reviews.numHelpful  : int  1 NA NA NA 1 NA 1 1 0 NA ...
##  $ reviews.rating      : int  5 5 5 5 5 5 3 2 5 5 ...
##  $ reviews.sourceURLs  : chr  "https://redsky.target.com/groot-domain-api/v1/reviews/13312834?sort=helpfulness_desc&limit=200&offset=0" "https://redsky.target.com/groot-domain-api/v1/reviews/50392800?sort=helpfulness_desc&limit=200&offset=0" "https://redsky.target.com/groot-domain-api/v1/reviews/50399893?sort=helpfulness_desc&limit=200&offset=0" "https://redsky.target.com/groot-domain-api/v1/reviews/50399893?sort=helpfulness_desc&limit=200&offset=0" ...
##  $ reviews.text        : chr  "This a fantastic white wine for any occasion!" "Tart, not sweet...very refreshing and delicious!" "I was given this wine so it was a delightful surprise to find that it has a flavorful and delicious taste! A new favorite!!!!" "This is a phenomenal wine and my new favorite red." ...
##  $ reviews.title       : chr  "My Favorite White Wine" "Yum!!" "A New Favorite!" "Bold, Flavorful, Aromatic, Delicious" ...
##  $ reviews.userCity    : chr  "" "" "" "" ...
##  $ reviews.userProvince: chr  "" "" "" "" ...
##  $ reviews.username    : chr  "Bjh" "Wino" "Bama Mom" "Av Dub" ...
##  $ sizes               : chr  "" "" "" "" ...
##  $ sourceURLs          : chr  "http://redsky.target.com/v1/plp/search?kwr=y&count=90&offset=0&category=5xsxvZ56dar,http://redsky.target.com/v1"| __truncated__ "http://redsky.target.com/v1/plp/search?kwr=y&count=90&offset=540&category=5xsxv,http://redsky.target.com/v2/pdp"| __truncated__ "http://redsky.target.com/v1/plp/search?kwr=y&count=90&offset=90&category=5xsxv,http://redsky.target.com/v1/plp/"| __truncated__ "http://redsky.target.com/v1/plp/search?kwr=y&count=90&offset=90&category=5xsxv,http://redsky.target.com/v1/plp/"| __truncated__ ...
##  $ upc                 : chr  "4.9213E+11" "83120003441" "82896001453" "82896001453" ...
##  $ weight              : chr  "1.0 lbs" "2.45 lbs" "3.09 lbs" "3.09 lbs" ...
```

```r
summary(df)
```

```
##       id               asins              brand          
##  Length:2890        Length:2890        Length:2890       
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
##                                                          
##   categories         dateAdded         dateUpdated       
##  Length:2890        Length:2890        Length:2890       
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
##                                                          
##  descriptions        dimension             ean           
##  Length:2890        Length:2890        Length:2890       
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
##                                                          
##    flavors              keys           manufacturer      
##  Length:2890        Length:2890        Length:2890       
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
##                                                          
##  manufacturerNumber     name           reviews.date      
##  Length:2890        Length:2890        Length:2890       
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
##                                                          
##  reviews.dateAdded  reviews.dateSeen   reviews.didPurchase
##  Length:2890        Length:2890        Mode :logical      
##  Class :character   Class :character   FALSE:1723         
##  Mode  :character   Mode  :character   TRUE :326          
##                                        NA's :841          
##                                                           
##                                                           
##                                                           
##  reviews.doRecommend   reviews.id        reviews.numHelpful
##  Mode :logical       Min.   :  2188283   Min.   : 0.000    
##  FALSE:90            1st Qu.: 96744793   1st Qu.: 0.000    
##  TRUE :1821          Median :102795588   Median : 0.000    
##  NA's :979           Mean   :101256393   Mean   : 1.283    
##                      3rd Qu.:103820575   3rd Qu.: 1.000    
##                      Max.   :190981111   Max.   :29.000    
##                      NA's   :1005        NA's   :2264      
##  reviews.rating  reviews.sourceURLs reviews.text       reviews.title     
##  Min.   :1.000   Length:2890        Length:2890        Length:2890       
##  1st Qu.:5.000   Class :character   Class :character   Class :character  
##  Median :5.000   Mode  :character   Mode  :character   Mode  :character  
##  Mean   :4.691                                                           
##  3rd Qu.:5.000                                                           
##  Max.   :5.000                                                           
##  NA's   :445                                                             
##  reviews.userCity   reviews.userProvince reviews.username  
##  Length:2890        Length:2890          Length:2890       
##  Class :character   Class :character     Class :character  
##  Mode  :character   Mode  :character     Mode  :character  
##                                                            
##                                                            
##                                                            
##                                                            
##     sizes            sourceURLs            upc           
##  Length:2890        Length:2890        Length:2890       
##  Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character  
##                                                          
##                                                          
##                                                          
##                                                          
##     weight         
##  Length:2890       
##  Class :character  
##  Mode  :character  
##                    
##                    
##                    
## 
```


**Remove rows where review scores are null**


```r
df = df[!is.na(df$reviews.rating),]
table(df$reviews.rating)
```

```
## 
##    1    2    3    4    5 
##   69   43   65  221 2047
```

**Create wine dataframe with 2 columns: reviews.rating, reviews.text**


```r
wine = data.frame(df$reviews.rating, df$reviews.text)
```

**Create "negative" column where the rating score where reviews.rating <=2 is TRUE, and FALSE other for reviews.rating >=3**


```r
wine$negative = as.factor(wine$df.reviews.rating <= 2)
```


**Load libraries: tm, SnowballC**


```r
library(tm)
```

```
## Loading required package: NLP
```

```r
library(SnowballC)
```


**Create Corpus with reviews.text, and look at first review**

```r
corpus = Corpus(VectorSource(wine$df.reviews.text))
corpus[[1]]$content
```

```
## [1] "This a fantastic white wine for any occasion!"
```

**Get data ready for text analysis**


```r
corpus = tm_map(corpus, tolower)
corpus = tm_map(corpus, removePunctuation)
corpus = tm_map(corpus, removeWords, c(stopwords("english")))
corpus = tm_map(corpus, stemDocument)
```

**Review first review after changes**

```r
corpus[[1]]$content
```

```
## [1] "fantast white wine occas"
```

**Review text frequencies (below shows that there are 4,018 distinct terms)**

```r
freq = DocumentTermMatrix(corpus)
freq
```

```
## <<DocumentTermMatrix (documents: 2445, terms: 4018)>>
## Non-/sparse entries: 35054/9788956
## Sparsity           : 100%
## Maximal term length: 24
## Weighting          : term frequency (tf)
```

**Show words that appear more than 100 times**

```r
findFreqTerms(freq, lowfreq=100)
```

```
##  [1] "wine"    "favorit" "find"    "flavor"  "tast"    "bottl"   "get"    
##  [8] "good"    "like"    "price"   "drink"   "great"   "just"    "love"   
## [15] "one"     "also"    "sweet"   "smooth"  "soft"    "make"    "perfect"
## [22] "sinc"    "well"    "will"    "even"    "tri"     "day"     "dri"    
## [29] "dont"    "everi"   "brand"   "best"    "buy"     "need"    "realli" 
## [36] "without" "time"    "work"    "ever"    "mix"     "can"     "now"    
## [43] "keep"    "purchas" "alway"   "cold"    "product" "ive"     "bought" 
## [50] "never"   "better"  "littl"   "long"    "year"    "order"   "origin" 
## [57] "winter"  "use"     "lip"     "feel"    "help"    "jar"     "carmex" 
## [64] "chap"    "heal"    "balm"    "sore"
```

**Narrowing terms that appear .05%+ in text**

```r
sparse = removeSparseTerms(freq, .995)
sparse
```

```
## <<DocumentTermMatrix (documents: 2445, terms: 496)>>
## Non-/sparse entries: 26311/1186409
## Sparsity           : 98%
## Maximal term length: 10
## Weighting          : term frequency (tf)
```

**Convert "sparse" to dataframe**

```r
wineSparse = as.data.frame(as.matrix(sparse))
colnames(wineSparse) = make.names(colnames(wineSparse))
wineSparse$negative = wine$negative
```

*Split data to training/testing sets*

```r
library(caTools)
set.seed(101)
split = sample.split(wineSparse$negative, SplitRatio = .7)
trainWine = subset(wineSparse, split==TRUE)
testWine= subset(wineSparse, split==FALSE)
```

**CART Model on training set**

```r
library(rpart)
library(rpart.plot)
wineCart = rpart(negative ~., data=trainWine, method="class")
prp(wineCart)
```

![](WineReviews_files/figure-html/unnamed-chunk-14-1.png)<!-- -->

**Predict model on testing set**

```r
predictWine = predict(wineCart, newdata=testWine, type="class")
```

**Classfication Matrix**

```r
table(testWine$negative, predictWine)
```

```
##        predictWine
##         FALSE TRUE
##   FALSE   698    2
##   TRUE     31    3
```

**Model accuracy**

```r
(698 + 3) / (698+2+31+3)
```

```
## [1] 0.9550409
```

**Baseline model**

```r
table(testWine$negative)
```

```
## 
## FALSE  TRUE 
##   700    34
```

```r
700/ (700 + 34)
```

```
## [1] 0.9536785
```

**Random Forest Model**

```r
library(randomForest)
```

```
## randomForest 4.6-12
```

```
## Type rfNews() to see new features/changes/bug fixes.
```

```r
set.seed(144)
wineRF = randomForest(negative ~., data=trainWine)
predictRF = predict(wineRF, newdata= testWine)
table(testWine$negative, predictRF)
```

```
##        predictRF
##         FALSE TRUE
##   FALSE   700    0
##   TRUE     31    3
```

```r
(700+3)/(700+31+3)
```

```
## [1] 0.9577657
```


