Retrieving the Data
================
John Carlo Maula

## 1. Introduction

This section focuses on retrieving the lyrics to Taylor Swift’s albums
using the genius package, which can be accessed
[here](https://github.com/JosiahParry/genius). I will also be cleaning
the data by removing duplicates, filling in missing values, and
addressing contractions and special characters in the lyrics. Finally, I
will store the dataset into a .csv file to be used in the analyses
section.

**Note: Due to problems with the genius package, the resulting dataset
from this section contains incomplete lyrics.**

### 1.1 Loading Packages

These are the packages I used for this section of the project.

``` r
# Load required libraries
library(genius)
library(tidyverse)
```

### 1.2 Retrieving Lyrics

I used the genius library to pull the lyrics from Genius. I chose deluxe
editions of albums where it is applicable to include any bonus tracks.

``` r
# Create a tribble of inputs for each album
artist_albums <- tribble(
  ~artist, ~album,
  "Taylor Swift", "Taylor Swift",
  "Taylor Swift", "Fearless (Taylor's Version)",
  "Taylor Swift", "Speak Now (Deluxe)",
  "Taylor Swift", "Red (Deluxe Edition)",
  "Taylor Swift", "1989 (Deluxe)",
  "Taylor Swift", "reputation",
  "Taylor Swift", "Lover",
  "Taylor Swift", "folklore (deluxe version)",
  "Taylor Swift", "evermore (deluxe version)"
)

# Retrieve the lyrics for each album using the genius package
lyrics <- artist_albums %>% add_genius(artist, album, type = "album")
```

## 2. Data Cleaning

### 2.1 Examining the Data

After retrieving the lyrics from Genius, I viewed the data and its
structure to determine any necessary cleaning. There are 6 columns and
3,552 rows. The column names are:

1.  **artist** - artist of the song
2.  **album** - album containing the song
3.  **track\_n** - track number of the song in the album
4.  **line** - line number of the lyric in the song
5.  **lyric** - lyric of the song
6.  **track\_title** - name of the song

**Note: Due to an issue with the genius package, the resulting lyrics
are incomplete.**

``` r
# View data
head(lyrics)
```

    ## # A tibble: 6 × 6
    ##   artist       album        track_n  line lyric                      track_title
    ##   <chr>        <chr>          <int> <int> <chr>                      <chr>      
    ## 1 Taylor Swift Taylor Swift       1     1 "He said the way my blue … Tim McGraw 
    ## 2 Taylor Swift Taylor Swift       1     2 "Put those Georgia stars … Tim McGraw 
    ## 3 Taylor Swift Taylor Swift       1     3 "I said, \"That's a lie\"… Tim McGraw 
    ## 4 Taylor Swift Taylor Swift       1     4 "That had a tendency of g… Tim McGraw 
    ## 5 Taylor Swift Taylor Swift       1     5 "On backroads at night"    Tim McGraw 
    ## 6 Taylor Swift Taylor Swift       1     6 "And I was right there be… Tim McGraw

``` r
# Check structure of the data
str(lyrics)
```

    ## tibble [3,552 × 6] (S3: tbl_df/tbl/data.frame)
    ##  $ artist     : chr [1:3552] "Taylor Swift" "Taylor Swift" "Taylor Swift" "Taylor Swift" ...
    ##  $ album      : chr [1:3552] "Taylor Swift" "Taylor Swift" "Taylor Swift" "Taylor Swift" ...
    ##  $ track_n    : int [1:3552] 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ line       : int [1:3552] 1 2 3 4 5 6 7 8 9 10 ...
    ##  $ lyric      : chr [1:3552] "He said the way my blue eyes shined" "Put those Georgia stars to shame that night" "I said, \"That's a lie\"Just a boy in a Chevy truck" "That had a tendency of gettin' stuck" ...
    ##  $ track_title: chr [1:3552] "Tim McGraw" "Tim McGraw" "Tim McGraw" "Tim McGraw" ...

### 2.2 Missing Values

There are two songs with missing values. It appears that the first lines
of the song were mistaken as the second line, resulting in missing
values for the first line. I removed these rows and corrected the line
numbering.

``` r
# Check missing values
lyrics[!complete.cases(lyrics), ]
```

    ## # A tibble: 2 × 6
    ##   artist       album                track_n  line lyric track_title             
    ##   <chr>        <chr>                  <int> <int> <chr> <chr>                   
    ## 1 Taylor Swift Red (Deluxe Edition)       4     1 <NA>  I Knew You Were Trouble.
    ## 2 Taylor Swift 1989 (Deluxe)             12     1 <NA>  I Know Places

``` r
# Check lyrics of tracks with the missing values
lyrics[lyrics$track_title == "I Knew You Were Trouble.", ]
```

    ## # A tibble: 20 × 6
    ##    artist       album                track_n  line lyric           track_title  
    ##    <chr>        <chr>                  <int> <int> <chr>           <chr>        
    ##  1 Taylor Swift Red (Deluxe Edition)       4     1 <NA>            I Knew You W…
    ##  2 Taylor Swift Red (Deluxe Edition)       4     2 Once upon a ti… I Knew You W…
    ##  3 Taylor Swift Red (Deluxe Edition)       4     3 I was in your … I Knew You W…
    ##  4 Taylor Swift Red (Deluxe Edition)       4     4 You found me, … I Knew You W…
    ##  5 Taylor Swift Red (Deluxe Edition)       4     5 I guess you di… I Knew You W…
    ##  6 Taylor Swift Red (Deluxe Edition)       4     6 And when I fel… I Knew You W…
    ##  7 Taylor Swift Red (Deluxe Edition)       4     7 Without me, wi… I Knew You W…
    ##  8 Taylor Swift Red (Deluxe Edition)       4     8 And he's long … I Knew You W…
    ##  9 Taylor Swift Red (Deluxe Edition)       4     9 And I realize … I Knew You W…
    ## 10 Taylor Swift Red (Deluxe Edition)       4    10 'Cause I knew … I Knew You W…
    ## 11 Taylor Swift Red (Deluxe Edition)       4    11 So shame on me… I Knew You W…
    ## 12 Taylor Swift Red (Deluxe Edition)       4    12 Flew me to pla… I Knew You W…
    ## 13 Taylor Swift Red (Deluxe Edition)       4    13 I knew you wer… I Knew You W…
    ## 14 Taylor Swift Red (Deluxe Edition)       4    14 So shame on me… I Knew You W…
    ## 15 Taylor Swift Red (Deluxe Edition)       4    15 Flew me to pla… I Knew You W…
    ## 16 Taylor Swift Red (Deluxe Edition)       4    16 Now I'm lying … I Knew You W…
    ## 17 Taylor Swift Red (Deluxe Edition)       4    17 Oh, oh-oh       I Knew You W…
    ## 18 Taylor Swift Red (Deluxe Edition)       4    18 Trouble, troub… I Knew You W…
    ## 19 Taylor Swift Red (Deluxe Edition)       4    19 Oh, oh-oh       I Knew You W…
    ## 20 Taylor Swift Red (Deluxe Edition)       4    20 Trouble, troub… I Knew You W…

``` r
lyrics[lyrics$track_title == "I Know Places", ]
```

    ## # A tibble: 15 × 6
    ##    artist       album         track_n  line lyric                    track_title
    ##    <chr>        <chr>           <int> <int> <chr>                    <chr>      
    ##  1 Taylor Swift 1989 (Deluxe)      12     1 <NA>                     I Know Pla…
    ##  2 Taylor Swift 1989 (Deluxe)      12     2 I, I, I, I, I, I, I, I-… I Know Pla…
    ##  3 Taylor Swift 1989 (Deluxe)      12     3 You stand with your han… I Know Pla…
    ##  4 Taylor Swift 1989 (Deluxe)      12     4 It's a scene and we're … I Know Pla…
    ##  5 Taylor Swift 1989 (Deluxe)      12     5 I can hear them whisper… I Know Pla…
    ##  6 Taylor Swift 1989 (Deluxe)      12     6 It's a bad sign, bad si… I Know Pla…
    ##  7 Taylor Swift 1989 (Deluxe)      12     7 Something happens when … I Know Pla…
    ##  8 Taylor Swift 1989 (Deluxe)      12     8 See the vultures circli… I Know Pla…
    ##  9 Taylor Swift 1989 (Deluxe)      12     9 Love's a fragile little… I Know Pla…
    ## 10 Taylor Swift 1989 (Deluxe)      12    10 It could burn out        I Know Pla…
    ## 11 Taylor Swift 1989 (Deluxe)      12    11 'Cause they got the cag… I Know Pla…
    ## 12 Taylor Swift 1989 (Deluxe)      12    12 They are the hunters, w… I Know Pla…
    ## 13 Taylor Swift 1989 (Deluxe)      12    13 Baby, I know places we … I Know Pla…
    ## 14 Taylor Swift 1989 (Deluxe)      12    14 And they'll be chasing … I Know Pla…
    ## 15 Taylor Swift 1989 (Deluxe)      12    15 I know places, I know p… I Know Pla…

``` r
# Remove rows with NA values
lyrics <- lyrics[complete.cases(lyrics), ]

# Fix line numbers for affected songs
lyrics[lyrics$track_title == "I Knew You Were Trouble.", ]$line <- lyrics[lyrics$track_title == "I Knew You Were Trouble.", ]$line - 1
lyrics[lyrics$track_title == "I Know Places", ]$line <- lyrics[lyrics$track_title == "I Know Places", ]$line - 1
```

### 2.3 Duplicated Lyrics

I looked at the list of every song in the dataset and verified the
tracklisting. Then, I removed the extra versions of songs that are
already in the album (e.g., *Teardrops on My Guitar (Pop Version)*).

``` r
# Check list of tracks included in the dataset
unique(as.factor(lyrics$track_title))
```

    ##   [1] Tim McGraw                                                            
    ##   [2] Picture to Burn                                                       
    ##   [3] Teardrops On My Guitar                                                
    ##   [4] A Place In This World                                                 
    ##   [5] Cold as You                                                           
    ##   [6] The Outside                                                           
    ##   [7] Tied Together with a Smile                                            
    ##   [8] Stay Beautiful                                                        
    ##   [9] Should've Said No                                                     
    ##  [10] Mary's Song (Oh My My My)                                             
    ##  [11] Our Song                                                              
    ##  [12] I’m Only Me When I’m with You                                         
    ##  [13] Invisible                                                             
    ##  [14] A Perfectly Good Heart                                                
    ##  [15] Teardrops on My Guitar (Pop Version)                                  
    ##  [16] Fearless (Taylor's Version)                                           
    ##  [17] Fifteen (Taylor's Version)                                            
    ##  [18] Love Story (Taylor's Version)                                         
    ##  [19] Hey Stephen (Taylor's Version)                                        
    ##  [20] White Horse (Taylor's Version)                                        
    ##  [21] You Belong With Me (Taylor's Version)                                 
    ##  [22] Breathe (Taylor's Version) (Ft. Colbie Caillat)                       
    ##  [23] Tell Me Why (Taylor's Version)                                        
    ##  [24] You're Not Sorry (Taylor's Version)                                   
    ##  [25] The Way I Loved You (Taylor's Version)                                
    ##  [26] Forever & Always (Taylor's Version)                                   
    ##  [27] The Best Day (Taylor's Version)                                       
    ##  [28] Change (Taylor's Version)                                             
    ##  [29] Jump Then Fall (Taylor's Version)                                     
    ##  [30] Untouchable (Taylor's Version)                                        
    ##  [31] Forever & Always (Piano Version) [Taylor's Version]                   
    ##  [32] Come In With the Rain (Taylor's Version)                              
    ##  [33] Superstar (Taylor's Version)                                          
    ##  [34] The Other Side of the Door (Taylor's Version)                         
    ##  [35] Today Was a Fairytale (Taylor's Version)                              
    ##  [36] You All Over Me (Taylor's Version) [From the Vault] (Ft. Maren Morris)
    ##  [37] Mr. Perfectly Fine (Taylor's Version) [From the Vault]                
    ##  [38] We Were Happy (Taylor's Version) [From the Vault]                     
    ##  [39] That's When (Taylor's Version) [From the Vault] (Ft. Keith Urban)     
    ##  [40] Don't You (Taylor's Version) [From the Vault]                         
    ##  [41] Bye Bye Baby (Taylor's Version) [From the Vault]                      
    ##  [42] Love Story (Taylor's Version) [Elvira Remix]                          
    ##  [43] Mine                                                                  
    ##  [44] Sparks Fly                                                            
    ##  [45] Back to December                                                      
    ##  [46] Speak Now                                                             
    ##  [47] Dear John                                                             
    ##  [48] Mean                                                                  
    ##  [49] The Story of Us                                                       
    ##  [50] Never Grow Up                                                         
    ##  [51] Enchanted                                                             
    ##  [52] Better Than Revenge                                                   
    ##  [53] Innocent                                                              
    ##  [54] Haunted                                                               
    ##  [55] Last Kiss                                                             
    ##  [56] Long Live                                                             
    ##  [57] Ours                                                                  
    ##  [58] If This Was a Movie                                                   
    ##  [59] Superman                                                              
    ##  [60] State of Grace                                                        
    ##  [61] Red                                                                   
    ##  [62] Treacherous                                                           
    ##  [63] I Knew You Were Trouble.                                              
    ##  [64] All Too Well                                                          
    ##  [65] 22                                                                    
    ##  [66] I Almost Do                                                           
    ##  [67] We Are Never Ever Getting Back Together                               
    ##  [68] Stay Stay Stay                                                        
    ##  [69] The Last Time (Ft. Gary Lightbody)                                    
    ##  [70] Holy Ground                                                           
    ##  [71] Sad Beautiful Tragic                                                  
    ##  [72] The Lucky One                                                         
    ##  [73] Everything Has Changed (Ft. Ed Sheeran)                               
    ##  [74] Starlight                                                             
    ##  [75] Begin Again                                                           
    ##  [76] The Moment I Knew                                                     
    ##  [77] Come Back... Be Here                                                  
    ##  [78] Girl at Home                                                          
    ##  [79] Treacherous (Original Demo Recording)                                 
    ##  [80] Red (Original Demo Recording)                                         
    ##  [81] State of Grace (Acoustic Version)                                     
    ##  [82] Welcome to New York                                                   
    ##  [83] Blank Space                                                           
    ##  [84] Style                                                                 
    ##  [85] Out of the Woods                                                      
    ##  [86] All You Had to Do Was Stay                                            
    ##  [87] Shake It Off                                                          
    ##  [88] I Wish You Would                                                      
    ##  [89] Bad Blood                                                             
    ##  [90] Wildest Dreams                                                        
    ##  [91] How You Get the Girl                                                  
    ##  [92] This Love                                                             
    ##  [93] I Know Places                                                         
    ##  [94] Clean                                                                 
    ##  [95] Wonderland                                                            
    ##  [96] You Are in Love                                                       
    ##  [97] New Romantics                                                         
    ##  [98] I Know Places (Voice Memo)                                            
    ##  [99] I Wish You Would (Voice Memo)                                         
    ## [100] Blank Space (Voice Memo)                                              
    ## [101] ...Ready for It?                                                      
    ## [102] End Game (Ft. Ed Sheeran & Future)                                    
    ## [103] I Did Something Bad                                                   
    ## [104] Don't Blame Me                                                        
    ## [105] Delicate                                                              
    ## [106] Look What You Made Me Do                                              
    ## [107] So It Goes...                                                         
    ## [108] Gorgeous                                                              
    ## [109] Getaway Car                                                           
    ## [110] King of My Heart                                                      
    ## [111] Dancing with Our Hands Tied                                           
    ## [112] Dress                                                                 
    ## [113] This Is Why We Can’t Have Nice Things                                 
    ## [114] Call It What You Want                                                 
    ## [115] New Year's Day                                                        
    ## [116] I Forgot That You Existed                                             
    ## [117] Cruel Summer                                                          
    ## [118] Lover                                                                 
    ## [119] The Man                                                               
    ## [120] The Archer                                                            
    ## [121] I Think He Knows                                                      
    ## [122] Miss Americana & The Heartbreak Prince                                
    ## [123] Paper Rings                                                           
    ## [124] Cornelia Street                                                       
    ## [125] Death by a Thousand Cuts                                              
    ## [126] London Boy                                                            
    ## [127] Soon You'll Get Better (Ft. The Chicks)                               
    ## [128] False God                                                             
    ## [129] You Need To Calm Down                                                 
    ## [130] Afterglow                                                             
    ## [131] ME! (Ft. Brendon Urie)                                                
    ## [132] It’s Nice to Have a Friend                                            
    ## [133] Daylight                                                              
    ## [134] ​the 1                                                                 
    ## [135] ​cardigan                                                              
    ## [136] ​the last great american dynasty                                       
    ## [137] ​exile (Ft. Bon Iver)                                                  
    ## [138] ​my tears ricochet                                                     
    ## [139] ​mirrorball                                                            
    ## [140] ​seven                                                                 
    ## [141] ​august                                                                
    ## [142] ​this is me trying                                                     
    ## [143] ​illicit affairs                                                       
    ## [144] ​invisible string                                                      
    ## [145] ​mad woman                                                             
    ## [146] ​epiphany                                                              
    ## [147] ​betty                                                                 
    ## [148] ​peace                                                                 
    ## [149] ​hoax                                                                  
    ## [150] ​the lakes                                                             
    ## [151] ​willow                                                                
    ## [152] ​champagne problems                                                    
    ## [153] ​gold rush                                                             
    ## [154] ​'tis the damn season                                                  
    ## [155] ​tolerate it                                                           
    ## [156] ​no body, no crime (Ft. HAIM)                                          
    ## [157] ​happiness                                                             
    ## [158] ​dorothea                                                              
    ## [159] ​coney island (Ft. The National)                                       
    ## [160] ​ivy                                                                   
    ## [161] ​cowboy like me                                                        
    ## [162] ​l​ong story short                                                      
    ## [163] ​marjorie                                                              
    ## [164] ​closure                                                               
    ## [165] ​evermore (Ft. Bon Iver)                                               
    ## [166] ​r​ight where you left me                                               
    ## [167] ​it’s time to go                                                       
    ## 167 Levels: ...Ready for It? ​'tis the damn season 22 ... You're Not Sorry (Taylor's Version)

``` r
# Remove extra tracks
lyrics <- lyrics[(lyrics$track_title != "Teardrops on My Guitar (Pop Version)") &
                   (lyrics$track_title != "Forever & Always (Piano Version) [Taylor's Version]") &
                   (lyrics$track_title != "Love Story (Taylor's Version) [Elvira Remix]") &
                   (lyrics$track_title != "Treacherous (Original Demo Recording)") &
                   (lyrics$track_title != "Red (Original Demo Recording)") &
                   (lyrics$track_title != "State of Grace (Acoustic Version)") &
                   (lyrics$track_title != "I Know Places (Voice Memo)") &
                   (lyrics$track_title != "I Wish You Would (Voice Memo)") &
                   (lyrics$track_title != "Blank Space (Voice Memo)"), ]
```

### 2.4 Addressing Contractions and Special Characters

Using a
[tutorial](https://www.datacamp.com/community/tutorials/R-nlp-machine-learning)
from DataCamp, I defined a function that will fix contractions and
remove special characters. Then, I applied these functions to the
dataset and stored it into a csv file.

``` r
# Check list of album titles
unique(lyrics$album) 
```

    ## [1] "Taylor Swift"                "Fearless (Taylor's Version)"
    ## [3] "Speak Now (Deluxe)"          "Red (Deluxe Edition)"       
    ## [5] "1989 (Deluxe)"               "reputation"                 
    ## [7] "Lover"                       "folklore (deluxe version)"  
    ## [9] "evermore (deluxe version)"

``` r
# Simplify album titles
lyrics$album <- gsub(" \\(Deluxe\\)", "", lyrics$album)
lyrics$album <- gsub(" \\(Deluxe Edition\\)", "", lyrics$album)
lyrics$album <- gsub(" \\(deluxe version\\)", "", lyrics$album)
```

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
lyrics$lyric <- sapply(lyrics$lyric, tolower)
lyrics$lyric <- sapply(lyrics$lyric, fix.contractions)
lyrics$lyric <- sapply(lyrics$lyric, remove.special.chars)
```

Before storing the dataset into a csv file, I took one final look at the
dataframe.

``` r
# Preview the data and its structure
head(lyrics)
```

    ## # A tibble: 6 × 6
    ##   artist       album        track_n  line lyric                      track_title
    ##   <chr>        <chr>          <int> <int> <chr>                      <chr>      
    ## 1 Taylor Swift Taylor Swift       1     1 he said the way my blue e… Tim McGraw 
    ## 2 Taylor Swift Taylor Swift       1     2 put those georgia stars t… Tim McGraw 
    ## 3 Taylor Swift Taylor Swift       1     3 i said   that a lie just … Tim McGraw 
    ## 4 Taylor Swift Taylor Swift       1     4 that had a tendency of ge… Tim McGraw 
    ## 5 Taylor Swift Taylor Swift       1     5 on backroads at night      Tim McGraw 
    ## 6 Taylor Swift Taylor Swift       1     6 and i was right there bes… Tim McGraw

``` r
# Export dataset into a csv file
write.csv(lyrics, "data/swiftLyrics.csv")
```
