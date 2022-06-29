# Analyzing Taylor Swift's Lyrics

**Project Description**: In this project, I analyze the lyrics of Taylor Swift's music in R. For more background information, check out my **[blog](https://johncarlomaula.github.io/project2_lyrics)** to see a summary write-up of the project!

**Main Goals**:
- Extract Taylor Swift lyrics data using the `genius` package from CRAN
- Calculate word counts and TF-IDF
- Perform sentiment analysis on the lyrics
- Visualize results using word clouds and other visualization tools

## 

**Project Overview**

| Title| Description | .Rmd File
| :---: | ----------- | :---: |
| **[Data Retrieval](https://github.com/johncarlomaula/taylorswift-lyrics-project/blob/main/swift_data_v2.md)** | I cleaned the dataset Taylor Swift's lyrics from the user [shaynak](https://github.com/shaynak/taylor-swift-lyrics) and prepared it for analysis.  | [View](https://github.com/johncarlomaula/taylorswift-lyrics-project/blob/main/rmd_files/swift_data_v2.Rmd) |
| **[Word Count Analysis](https://github.com/johncarlomaula/taylorswift-lyrics-project/blob/main/swift_words.md)** | In this section, I focused on the analyzing the word frequency and lexical diversity of Taylor Swift's music. | [View](https://github.com/johncarlomaula/taylorswift-lyrics-project/blob/main/rmd_files/swift_words.Rmd) |
| **[Sentiment Analysis](https://github.com/johncarlomaula/taylorswift-lyrics-project/blob/main/swift_sentiments.md)** | In this section, I focused on analyzing the sentiment of Taylor Swift's lyrics based on 3 different lexicons: Bing, AFINN, and NRC. | [View](https://github.com/johncarlomaula/taylorswift-lyrics-project/blob/main/rmd_files/swift_sentiments.Rmd) |
| **[Data Retrieval (Old)](https://github.com/johncarlomaula/taylorswift-lyrics-project/blob/main/swift_data.md)** | I used the `genius` package to retrieve Taylor Swift's lyrics and prepared it for analysis. **Note:** The `genius` package was returning incomplete lyrics and is unsupported as of now. | [View](https://github.com/johncarlomaula/taylorswift-lyrics-project/blob/main/rmd_files/swift_data.Rmd) |

##

**Links**

Here are the resources I used to complete this project.
1. https://github.com/JosiahParry/genius
2. https://github.com/shaynak/taylor-swift-lyrics
3. https://www.datacamp.com/community/tutorials/R-nlp-machine-learning
4. https://www.tidytextmining.com/sentiment.html
5. https://www.r-graph-gallery.com/38-rcolorbrewers-palettes.html
