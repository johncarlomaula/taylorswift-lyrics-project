Preparing the Dataset
================

## 1. Introduction

Since retrieving the data using the genius library was returning
incomplete lyrics, I decided to use a dataset, which I found
[here](https://github.com/shaynak/taylor-swift-lyrics), that contains
Taylor Swift lyrics. This section will focus on preparing that dataset
for analysis, as well as addressing any other issues that occur such as
missing lyrics.

### 1.1 Loading Packages

These are the packages I used for this section of the project.

``` r
# Load required libraries
library(tidyverse)
```

### 1.2 Importing Data

``` r
# Import lyrics data
lyrics <- read.csv("data/lyrics.csv")
```

## 2. Data Cleaning

### 2.1 Data Exploration

I previewed and looked at the structure of the data. There are 6 columns
and 9,642 rows. The 6 columns are:

1.  **Song** - the name of the song
2.  **Album** - the album the song belongs to
3.  **Lyric** - a line of the song
4.  **Previous.Lyric** - the previous line of the song
5.  **Next.Lyric** - the next line in the song
6.  **Multiplicity** - the occurence of the lyric in the song

Since I only require the first 3 columns for this analysis, I dropped
the last 3 columns.

``` r
# Preview data
head(lyrics)
```

    ##   Song                Album                                        Lyric
    ## 1   22 Red (Deluxe Edition)                It feels like a perfect night
    ## 2   22 Red (Deluxe Edition)                    To dress up like hipsters
    ## 3   22 Red (Deluxe Edition)       And make fun of our exes, uh-uh, uh-uh
    ## 4   22 Red (Deluxe Edition)                It feels like a perfect night
    ## 5   22 Red (Deluxe Edition)                    For breakfast at midnight
    ## 6   22 Red (Deluxe Edition) To fall in love with strangers, uh-uh, uh-uh
    ##                           Previous.Lyric
    ## 1                                       
    ## 2          It feels like a perfect night
    ## 3              To dress up like hipsters
    ## 4 And make fun of our exes, uh-uh, uh-uh
    ## 5          It feels like a perfect night
    ## 6              For breakfast at midnight
    ##                                     Next.Lyric Multiplicity
    ## 1                    To dress up like hipsters            1
    ## 2       And make fun of our exes, uh-uh, uh-uh            1
    ## 3                It feels like a perfect night            1
    ## 4                    For breakfast at midnight            1
    ## 5 To fall in love with strangers, uh-uh, uh-uh            1
    ## 6                                                         1

``` r
# Check data structure
str(lyrics)
```

    ## 'data.frame':    9642 obs. of  6 variables:
    ##  $ Song          : Factor w/ 241 levels "...Ready for It?",..: 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ Album         : Factor w/ 25 levels "","1989","1989 (Deluxe)",..: 16 16 16 16 16 16 16 16 16 16 ...
    ##  $ Lyric         : Factor w/ 7483 levels " 'Tis the damn season",..: 4000 6213 958 4000 2419 6214 6924 6523 4117 6257 ...
    ##  $ Previous.Lyric: Factor w/ 7395 levels ""," 'Tis the damn season",..: 1 3957 6136 945 3957 2393 6137 6843 6445 4074 ...
    ##  $ Next.Lyric    : Factor w/ 6293 levels ""," 'Tis the damn season",..: 5195 821 3372 2091 5196 1 5466 3472 5237 3498 ...
    ##  $ Multiplicity  : int  1 1 1 1 1 1 1 1 1 1 ...

``` r
# Remove the last three columns
lyrics <- subset(lyrics, select = c("Song", "Album", "Lyric"))
```

For this project, I am only interested in songs that belong to her 9
studio albums: *Taylor Swift*, *Fearless*, *Speak Now*, *Red*, *1989*,
*reputation*, *Lover*, *folklore*, and *evermore*.

Looking at the list of albums, there are 24 different album titles
included in the dataset, such as the Hunger Games soundtrack. There are
also deluxe versions of her studio albums, which contain bonus tracks.
To simplify the album titles, I removed the “deluxe” identifiers in the
album titles.

``` r
# View list of albums
levels(lyrics$Album)
```

    ##  [1] ""                                                    
    ##  [2] "1989"                                                
    ##  [3] "1989 (Deluxe)"                                       
    ##  [4] "2004-2005 Demo CD"                                   
    ##  [5] "Beautiful Eyes - EP"                                 
    ##  [6] "Cats: Highlights From the Motion Picture Soundtrack" 
    ##  [7] "evermore"                                            
    ##  [8] "evermore (deluxe version)"                           
    ##  [9] "Fearless"                                            
    ## [10] "Fearless (Platinum Edition)"                         
    ## [11] "folklore"                                            
    ## [12] "folklore (deluxe version)"                           
    ## [13] "Hannah Montana: The Movie"                           
    ## [14] "Lover"                                               
    ## [15] "One Chance (Original Motion Picture Soundtrack)"     
    ## [16] "Red (Deluxe Edition)"                                
    ## [17] "reputation"                                          
    ## [18] "Speak Now"                                           
    ## [19] "Speak Now (Deluxe)"                                  
    ## [20] "Taylor Swift"                                        
    ## [21] "The Hunger Games: Songs from District 12 and Beyond" 
    ## [22] "The Taylor Swift Holiday Collection - EP"            
    ## [23] "Uncategorized"                                       
    ## [24] "Unreleased Songs"                                    
    ## [25] "Valentine’s Day (Original Motion Picture Soundtrack)"

``` r
# Remove deluxe identifiers in the album titles
lyrics$Album <- gsub(" \\(Deluxe\\)", "", lyrics$Album)
lyrics$Album <- gsub(" \\(Deluxe Edition\\)", "", lyrics$Album)
lyrics$Album <- gsub(" \\(deluxe version\\)", "", lyrics$Album)
lyrics$Album <- gsub(" \\(Platinum Edition\\)", "", lyrics$Album)
```

``` r
# Verify changes and change the album column to a factor variable
lyrics$Album <- factor(lyrics$Album)
levels(lyrics$Album)
```

    ##  [1] ""                                                    
    ##  [2] "1989"                                                
    ##  [3] "2004-2005 Demo CD"                                   
    ##  [4] "Beautiful Eyes - EP"                                 
    ##  [5] "Cats: Highlights From the Motion Picture Soundtrack" 
    ##  [6] "evermore"                                            
    ##  [7] "Fearless"                                            
    ##  [8] "folklore"                                            
    ##  [9] "Hannah Montana: The Movie"                           
    ## [10] "Lover"                                               
    ## [11] "One Chance (Original Motion Picture Soundtrack)"     
    ## [12] "Red"                                                 
    ## [13] "reputation"                                          
    ## [14] "Speak Now"                                           
    ## [15] "Taylor Swift"                                        
    ## [16] "The Hunger Games: Songs from District 12 and Beyond" 
    ## [17] "The Taylor Swift Holiday Collection - EP"            
    ## [18] "Uncategorized"                                       
    ## [19] "Unreleased Songs"                                    
    ## [20] "Valentine’s Day (Original Motion Picture Soundtrack)"

### 2.2 Filtering Songs

I filtered the dataset to only contain lyrics from songs in her studio
albums. After verifying the structure of the dataset, I found that there
are 151 songs in the dataset.

``` r
# Create a list of desired albums
albums <- c("Taylor Swift",
            "Fearless",
            "Speak Now",
            "Red",
            "1989",
            "reputation",
            "Lover",
            "folklore",
            "evermore")

# Filter songs in her nine studio albums
album.lyrics <- filter(lyrics, Album %in% albums) %>% droplevels()
```

``` r
# Verify 
str(album.lyrics)
```

    ## 'data.frame':    6577 obs. of  3 variables:
    ##  $ Song : Factor w/ 151 levels "...Ready for It?",..: 3 3 3 3 3 3 3 3 3 3 ...
    ##  $ Album: Factor w/ 9 levels "1989","evermore",..: 6 6 6 6 6 6 6 6 6 6 ...
    ##  $ Lyric: Factor w/ 5192 levels " 'Tis the damn season",..: 2821 4322 720 2821 1738 4323 4799 4541 2896 4349 ...

There should be a total of 150 unique songs in the dataset. However,
there are extra versions of songs that are included in the dataset
(e.g., *Teardrops on My Guitar (Pop Version)*). Since these versions are
usually indicated by an identifier in parenthesis, I decided to list all
the songs that contain parenthesis in the title to identify the extra
tracks to remove.

``` r
# List songs containing parenthesis in the title
par.songs <- album.lyrics[grep("\\(", album.lyrics$Song), ] %>% droplevels()
unique(par.songs$Song)
```

    ## [1] Red (Original Demo Recording)         State of Grace (Acoustic Version)    
    ## [3] Treacherous (Original Demo Recording) Mary’s Song (Oh My My My)            
    ## [5] Teardrops on My Guitar (Pop Version)  Forever & Always (Piano Version)     
    ## 6 Levels: Forever & Always (Piano Version) ... Treacherous (Original Demo Recording)

``` r
# Remove extra tracks
album.lyrics <- album.lyrics[(album.lyrics$Song != "Teardrops on My Guitar (Pop Version)") &
                   (album.lyrics$Song != "Forever & Always (Piano Version)") &
                   (album.lyrics$Song != "Treacherous (Original Demo Recording)") &
                   (album.lyrics$Song != "Red (Original Demo Recording)") &
                   (album.lyrics$Song != "State of Grace (Acoustic Version)"), ] %>% droplevels()
```

After removing the extra versions of standard songs, there are a total
of 146 songs remaining. Thus, there are a total of 4 missing songs in
the dataset. Fortunately, they were already identified by the person who
originally scraped the lyrics.

The missing songs, which all belong to Taylor Swift’s self-titled album,
are:

1.  Picture To Burn
2.  Cold As You
3.  Tied Together With A Smile
4.  I’m Only Me When I’m With You

``` r
# List songs in the self-titled album
unique(album.lyrics[album.lyrics$Album == "Taylor Swift", ]$Song)
```

    ##  [1] A Perfectly Good Heart    A Place In This World    
    ##  [3] Invisible                 Mary’s Song (Oh My My My)
    ##  [5] Our Song                  Should’ve Said No        
    ##  [7] Stay Beautiful            Teardrops on My Guitar   
    ##  [9] The Outside               Tim McGraw               
    ## 146 Levels: ...Ready for It? ​’tis the damn season 22 ... You’re Not Sorry

### 2.4 Adding Missing Songs

Although I have incomplete lyrics of those songs from the data retrieval
section, I decided to create a .txt file of the complete lyrics for each
song and placed them into a designated folder. Then, I read these text
files and stored them into a dataframe that matches the columns of the
dataset. Finally, I appended the songs into the dataset.

``` r
# Read extra tracks
picture.to.burn <- read.delim("data/missing_songs/picture_to_burn.txt", header = FALSE, col.names = "Lyric")
cold.as.you <- read.delim("data/missing_songs/cold_as_you.txt", header = FALSE, col.names = "Lyric")
tied.together <- read.delim("data/missing_songs/tied_together.txt", header = FALSE, col.names = "Lyric")
only.me <- read.delim("data/missing_songs/only_me.txt", header = FALSE, col.names = "Lyric")
```

``` r
# Add album and song titles
picture.to.burn$Song <- "Picture To Burn"
picture.to.burn$Album <- "Taylor Swift"

cold.as.you$Song <- "Cold As You"
cold.as.you$Album <- "Taylor Swift"

tied.together$Song <- "Tied Together With A Smile"
tied.together$Album <- "Taylor Swift"

only.me$Song <- "I'm Only Me When I'm With You"
only.me$Album <- "Taylor Swift"
```

``` r
# Add songs to the dataset
complete.album.lyrics <- rbind(album.lyrics, picture.to.burn, cold.as.you, tied.together, only.me)
```

### 2.5 Special Characters & Contractions

Using a
[tutorial](https://www.datacamp.com/community/tutorials/R-nlp-machine-learning)
from DataCamp, I defined a function that will fix contractions and
remove special characters. Then, I applied these functions to the
dataset and stored it into a csv file.

``` r
# Define function to fix contractions
fix.contractions <- function(doc) {
  # "won't" is a special case as it does not expand to "wo not"
  doc <- gsub("won't", "will not", doc)
  doc <- gsub("can't", "can not", doc)
  doc <- gsub("n't", " not", doc)
  doc <- gsub("'ll", " will", doc)
  doc <- gsub("'re", " are", doc)
  doc <- gsub("'ve", " have", doc)
  doc <- gsub("'m", " am", doc)
  doc <- gsub("'d", " would", doc)
  # 's could be 'is' or could be possessive: it has no expansion
  doc <- gsub("'s", "", doc)
  return(doc)
}

# Define function to remove special characters
remove.special.chars <- function(x) gsub("[^a-zA-Z0-9 ]", " ", x)
```

``` r
# Apply functions to dataset
complete.album.lyrics$Lyric <- sapply(complete.album.lyrics$Lyric, tolower)
complete.album.lyrics$Lyric <- sapply(complete.album.lyrics$Lyric, fix.contractions)
complete.album.lyrics$Lyric <- sapply(complete.album.lyrics$Lyric, remove.special.chars)
```

Before storing the dataset into a csv file, I took one final look at the
dataframe to make sure the results are as expected.

``` r
# Preview the data 
head(complete.album.lyrics, 10)
```

    ##    Song Album                                                     Lyric
    ## 1    22   Red                             it feels like a perfect night
    ## 2    22   Red                                 to dress up like hipsters
    ## 3    22   Red                    and make fun of our exes  uh uh  uh uh
    ## 4    22   Red                             it feels like a perfect night
    ## 5    22   Red                                 for breakfast at midnight
    ## 6    22   Red              to fall in love with strangers  uh uh  uh uh
    ## 7    22   Red                                                      yeah
    ## 8    22   Red we are happy  free  confused  and lonely at the same time
    ## 9    22   Red                        it miserable and magical  oh  yeah
    ## 10   22   Red      tonight the night when we forget about the deadlines

``` r
# Export dataset into a csv file
write.csv(complete.album.lyrics, "data/swiftLyrics_v2.csv")
```
