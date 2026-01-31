# Python - Spotify Analysis

### Description 
- In this project, I applied Python-based data collection and natural language processing (NLP) techniques to analyze song lyrics and album artwork from multiple music artists. The project included API-based data scraping, text cleaning, sentiment and emotion analysis, profanity detection, and image analytics. Through visualizations and comparative analysis, I developed skills in transforming unstructured media data into actionable insights for real-world analytical applications.

### Lyrics Text Cleaning and Preprocessing (NLP Preparation)

<b>Code Overview:</b>
- This code performs text cleaning and preprocessing on song lyrics and song titles in preparation for natural language processing (NLP) analysis. It begins by importing essential Python libraries for data handling (pandas), text processing (re), and NLP (nltk). The program reads a CSV file containing scraped song data, converts it into a dictionary structure, and extracts the lyrics and song title fields for further processing.
- The lyrics are cleaned by removing annotations such as text enclosed in parentheses, brackets, and braces, which often represent non-lyrical content (e.g., stage directions or background notes). The text is then standardized by converting all characters to lowercase, removing punctuation, eliminating line breaks, and retaining only alphanumeric characters and whitespace. These steps ensure that the lyrics are in a consistent format suitable for tokenization, frequency analysis, and sentiment modeling.
- Similarly, the song titles are cleaned by removing artist name suffixes, converting text to lowercase, and stripping punctuation. The cleaned song titles and lyrics are stored together in a structured output dictionary, which can later be used for downstream tasks such as word frequency analysis, n-gram generation, sentiment analysis, or visualization.

```python
import pandas
import re
import nltk
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from nltk.util import ngrams
from collections import Counter
import ssl

inputdata={}
# I changed the CSV file to match the CSV file made in example 5 from Q1
inputdata = pandas.read_csv('example5_EBresults.csv', header=[0], index_col=0).to_dict()

inputdata = {k: v for k, v in inputdata.items() if v} 

lyricsdictionary = inputdata.get('Lyrics')

output={"Song":[],"Lyrics":[]}

for lyric in lyricsdictionary.values():
    lyric = re.sub(r'\([^)]*\)', '', lyric)
    lyric = re.sub(r'\[[^\]]*\]', '', lyric)
    lyric = re.sub(r'\{[^}]*\}', '', lyric)
    lyric=lyric.lower()
    lyric = re.sub(r'[^\w\s]','',lyric)
    lyric = lyric.replace('\n', ' ')
    output['Lyrics'].append(lyric)

songtitledictionary = inputdata.get('Song')

for songtitle in songtitledictionary.values():
    songtitle = songtitle.split(" by", 1)[0]
songtitle = songtitle.lower()
    songtitle = re.sub(r'[^\w\s]','',songtitle)
    output['Song'].append(songtitle)
    
results = pandas.DataFrame(output)
# I changed the output name to match with Erykah Badu
results.to_csv('cleaninputdata_EB.csv', index=True, index_label="Index")

print("done")
```
<b> Overall Skills Developed </b>

<ins> Python Programming & Data Manipulation: </ins>
- Strengthened proficiency in Python by working with dictionaries, lists, loops, and conditional filtering to transform raw CSV data into structured formats suitable for analysis.

<ins> CSV Processing & Data Extraction: </ins>
- Gained experience using the Pandas library to read CSV files, convert tabular data into dictionary-based structures, and selectively extract relevant fields such as song titles and lyrics.

<ins> Text Cleaning & Normalization: </ins>
- Developed the ability to clean unstructured text data by removing annotations, punctuation, special characters, and line breaks, as well as standardizing text through lowercasing to ensure consistency.

<ins> Regular Expressions (Regex): </ins>
- Learned how to apply regular expressions to systematically remove unwanted patterns from text, including bracketed metadata and non-alphanumeric characters commonly found in song lyrics.

<ins> Natural Language Processing (NLP) Preparation: </ins>
- Built foundational NLP skills by preparing cleaned lyrics for tokenization, n-gram generation, word frequency analysis, and sentiment modeling in subsequent questions.

<ins> Data Quality & Analytical Readiness: </ins>
- Improved data quality by transforming raw, noisy lyrics into a clean and standardized dataset that enables accurate visualization, sentiment analysis, and comparative analysis across artists.

### Wordcloud Visualization of Cleaned Lyrics

<b> Code Overview: </b>
- This code generates a wordcloud visualization to explore the most frequently occurring words in an artist’s cleaned song lyrics. It begins by importing Python libraries for data handling (pandas), visualization (matplotlib), and wordcloud generation (WordCloud).
- The program reads a cleaned CSV file containing preprocessed lyrics data and converts it into a dictionary format. From this dataset, the lyrics column is extracted and transformed into a list, allowing all lyric entries to be combined into a single text corpus.
- The individual lyric entries are concatenated into one continuous string, which serves as the input for the wordcloud generation. The WordCloud object is configured with specific parameters such as image dimensions, background color, and minimum font size to improve readability and visual clarity.
- Finally, the wordcloud is rendered using Matplotlib, with axis labels removed to emphasize the textual patterns. This visualization highlights dominant words within the artist’s lyrics and supports exploratory text analysis by revealing common themes and language usage.

```python
import pandas
import matplotlib.pyplot as plt
from wordcloud import WordCloud

inputdata = {}
# I changed the CSV file to match the one I made in example 2 for Erykah Badu
inputdata = pandas.read_csv('cleaninputdata_EB.csv', header=[0], index_col=0).to_dict()

lyricsdictionary = inputdata.get("Lyrics")

lyricslist = list(lyricsdictionary.values())

lyricsinstring = ""
for eachletter in lyricslist:
    lyricsinstring += ''+ str(eachletter)

wordcloud = WordCloud(width = 500, height = 500,
            background_color ='white',
            min_font_size = 10).generate(lyricsinstring)

plt.figure(figsize = (15, 10), facecolor = None)
plt.imshow(wordcloud)
plt.axis("off")
plt.show()
```

<b> Overall Skills Developed </b>

<ins> Data Ingestion & CSV Handling: </ins>
- Strengthened skills in reading and processing cleaned CSV files using the Pandas library, enabling efficient extraction of lyric text for visualization and analysis.

<ins> Text Aggregation for Visualization: </ins>
- Developed the ability to combine multiple lyric entries into a single text corpus, a necessary step for generating global word frequency representations such as wordclouds.

<ins> Data Visualization with Python: </ins>
- Gained experience using Matplotlib to create and display visual outputs, including configuring figure size, layout, and axis settings for clear and effective presentation.

<ins> Wordcloud Generation & Interpretation: </ins>
- Learned how to generate wordcloud visualizations using the WordCloud library to identify dominant words and patterns within an artist’s lyrics.

<ins> Exploratory Text Analysis: </ins>
- Practiced exploratory data analysis techniques by visually examining word usage trends, supporting comparative analysis of lyrical themes across artists.

<ins> Analytical Storytelling & Insight Communication: </ins>
- Enhanced the ability to communicate insights from unstructured text data through intuitive visual representations that support pattern recognition and high-level interpretation.

### Lemmatization and Wordcloud Visualization of Lyrics

<b> Code Overview: </b>
- This code applies lemmatization to an artist’s cleaned song lyrics and visualizes the results using a wordcloud. The process begins by importing Python libraries for data handling (pandas), visualization (matplotlib), and wordcloud generation (WordCloud).
- The program reads a cleaned CSV file containing Erykah Badu’s lyrics and converts the dataset into a dictionary format. The lyrics are extracted, combined into a single text string, and prepared for natural language processing.
- Using the NLTK library, the lyrics text is tokenized into individual words, allowing each term to be processed independently. The WordNet lemmatizer is then applied to reduce words to their base or dictionary form (e.g., running → run, songs → song).
- After lemmatization, the processed tokens are converted back into a single string. This transformed text is used as the input for generating a wordcloud, which visually highlights the most common lemmatized words in the lyrics.
- Finally, the wordcloud is displayed using Matplotlib with formatting adjustments to improve clarity and readability. Overall, this code demonstrates how lemmatization affects word frequency representation and supports more accurate comparisons of lyrical themes.

```python
# Lemmatization Erykah Badu
import pandas
import matplotlib.pyplot as plt
from wordcloud import WordCloud

inputdata = {}
# I input the cleaned data for Erykah Badu that was made in example 2
inputdata = pandas.read_csv('cleaninputdata_EB.csv', header=[0], index_col=0).to_dict()

lyricsdictionary = inputdata.get("Lyrics")

lyricslist = list(lyricsdictionary.values())

lyricsinstring = ""
for eachletter in lyricslist:
    lyricsinstring += ''+ str(eachletter)

import nltk
from nltk.tokenize import word_tokenize
from nltk.stem import WordNetLemmatizer

tokenized_lyrics = word_tokenize(lyricsinstring)

nltk.download('wordnet')
lemmatizer = WordNetLemmatizer()

lemmatized_lyrics_tokens = [lemmatizer.lemmatize(word) for word in tokenized_lyrics]

convert_tokens_back_to_string = " ".join(lemmatized_lyrics_tokens)

wordcloud = WordCloud(width = 500, height = 500,
            background_color ='white',
min_font_size = 10).generate(convert_tokens_back_to_string)

plt.figure(figsize = (15, 10), facecolor = None)
plt.imshow(wordcloud)
plt.axis("off")
plt.show()
```

<b> Overall Skills Developed </b>

<ins> Text Normalization Through Lemmatization: </ins>
- Developed the ability to apply lemmatization techniques to reduce words to their base forms, improving the accuracy of word frequency analysis in text data.

<ins> Natural Language Processing (NLP) Techniques: </ins>
- Strengthened foundational NLP skills by implementing tokenization and lemmatization using the NLTK library and understanding their role in text preprocessing pipelines.
  
<ins> Comparative Text Analysis Preparation: </ins>
- Learned how linguistic normalization impacts visual outputs such as wordclouds, enabling meaningful comparison between original, stemmed, and lemmatized text representations.

<ins> Data Visualization for NLP Insights: </ins>
- Gained experience generating and interpreting wordclouds that reflect normalized word usage patterns across an artist’s lyrical content.

<ins> Analytical Reasoning & Interpretation: </ins>
- Enhanced the ability to interpret how preprocessing choices influence analytical outcomes and visual patterns, supporting thoughtful evaluation of NLP results.

<ins> End-to-End Text Processing Workflow: </ins>
- Practiced building an end-to-end NLP workflow that integrates data ingestion, text preprocessing, linguistic normalization, and visualization.

### Stemming and Wordcloud Visualization of Lyrics

<b> Code Overview: </b>
- This code applies stemming techniques to an artist’s cleaned song lyrics and visualizes the results using a wordcloud. The process begins by importing Python libraries for data handling (pandas), visualization (matplotlib), and wordcloud generation (WordCloud).
- The program reads a cleaned CSV file containing Erykah Badu’s lyrics and extracts the lyrics column for analysis. All lyric entries are combined into a single text corpus, which serves as the input for natural language processing.
- Using the NLTK library, the lyrics text is tokenized into individual words. The Porter Stemmer is then applied to reduce words to their root forms by removing suffixes (e.g., running → run, happiness → happi). Although both Porter and Snowball stemmers are imported, the Porter Stemmer is used to generate the stemmed tokens in this implementation.
- The stemmed tokens are converted back into a single string and used to generate a wordcloud visualization. This visual output highlights the most frequently occurring stems rather than complete dictionary words.
- Finally, the wordcloud is displayed using Matplotlib with formatting adjustments for readability. This approach demonstrates how stemming alters word representations and affects the interpretation of dominant terms in lyrical content.

```
# Stemming Erykah Badu
import pandas
import matplotlib.pyplot as plt
from wordcloud import WordCloud

inputdata = {}
# I changed the CSV file to match the one for Erykah Badu made in example 2
inputdata = pandas.read_csv('cleaninputdata_EB.csv', header=[0], index_col=0).to_dict()

lyricsdictionary = inputdata.get("Lyrics")

lyricslist = list(lyricsdictionary.values())

lyricsinstring = ""
for eachletter in lyricslist:
    lyricsinstring += ''+ str(eachletter)

import nltk
from nltk.tokenize import word_tokenize
from nltk.stem import PorterStemmer, SnowballStemmer
porter_stemmer = PorterStemmer()
snowball_stemmer = SnowballStemmer(language='english')

tokenized_lyrics = word_tokenize(lyricsinstring)

nltk.download('wordnet')
porter_stems = [porter_stemmer.stem(word) for word in tokenized_lyrics]

convert_tokens_back_to_string = " ".join(porter_stems)

wordcloud = WordCloud(width = 500, height = 500,
            background_color ='white',
            min_font_size = 10).generate(convert_tokens_back_to_string)

plt.figure(figsize = (15, 10), facecolor = None)
plt.imshow(wordcloud)
plt.axis("off")
plt.show()
```

<b> Overall Skills Developed </b>

<ins> Linguistic Normalization via Stemming: </ins>
- Developed the ability to apply stemming techniques to reduce words to their root forms, enabling analysis of word frequency patterns independent of grammatical variations.

<ins> Natural Language Processing (NLP) Tooling: </ins>
- Strengthened familiarity with NLTK stemmers, including Porter and Snowball stemmers, and understood their role in text preprocessing workflows.

<ins> Text Transformation & Token Handling: </ins>
- Gained experience tokenizing large text corpora and transforming token-level outputs back into analyzable text formats.

<ins> Exploratory Text Visualization: </ins>
- Practiced using wordcloud visualizations to explore how stemming impacts dominant word patterns and thematic emphasis in song lyrics.

<ins> Comparative NLP Analysis: </ins>
- Learned to compare stemmed text outputs with original and lemmatized versions, supporting deeper analysis of preprocessing choices and their analytical consequences.

<ins> End-to-End NLP Workflow Development: </ins>
- Enhanced skills in building a complete text analytics pipeline that integrates data ingestion, stemming, visualization, and interpretive analysis.

### Sentiment Analysis of Lyrics Using TextBlob and VADER

<b> Code Overview: </b>
- This code performs sentiment analysis on Erykah Badu’s cleaned song lyrics using two different sentiment tools: TextBlob and VADER. It begins by importing libraries for data processing (pandas), sentiment scoring (TextBlob and SentimentIntensityAnalyzer from VADER), and CSV output management.
- The program reads the cleaned lyrics dataset from a CSV file and extracts the lyrics column into a list so that each song’s lyrics can be analyzed individually. Two empty result lists are created to store sentiment outputs—one for TextBlob scores and one for VADER scores.
- For each song’s lyrics, the code uses TextBlob to compute:
    - Polarity (how positive or negative the text is)
    - Subjectivity (how opinion-based vs. factual the text appears)
      These values are stored in a dictionary and appended to a results list.
- The code then applies VADER sentiment analysis to each lyric entry. VADER returns four scores:
    - neg (negative sentiment proportion)
    - neu (neutral sentiment proportion)
    - pos (positive sentiment proportion)
    - compound (overall sentiment score from -1 to +1)
- These scores are stored for each song and later converted into a DataFrame for easy merging.
- After generating sentiment results, the code merges the TextBlob and VADER sentiment scores back into the original lyrics dataset as new columns. The final dataset is then exported to a new CSV file, creating a clean output that can be used for summary statistics, trend plots, and comparisons across artists.

```python
# TextBlob and VADER for Erykah Badu
import pandas
from textblob import TextBlob
from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer
import csv

inputdata={}
# Change CSV file name
inputdata = pandas.read_csv('cleaninputdata_EB.csv', header=[0]).to_dict()

lyricsdictionary = inputdata.get('Lyrics')
lyricslist =  list(lyricsdictionary.values())

textblob_results_list=[]
vader_results_list=[]

for i in range(len(lyricslist)):
    textblob_analyze_polarity = TextBlob(lyricslist[i]).polarity
    textblob_analyze_subjectivity = TextBlob(lyricslist[i]).subjectivity
    
    textblob_result = {"TextBlob Polarity Score":textblob_analyze_polarity,"TextBlob Subjectivity Score": textblob_analyze_subjectivity}
    textblob_results_list.append(textblob_result)

    vader_sentiment_analysis = SentimentIntensityAnalyzer().polarity_scores(lyricslist[i])
    vader_results_list.append(vader_sentiment_analysis)

textblobresults = pandas.DataFrame(textblob_results_list)

vaderresults = pandas.DataFrame(vader_results_list)

# changed CSV file name
file = pandas.read_csv('cleaninputdata_EB.csv')
file['TextBlob Polarity Score'] = textblobresults['TextBlob Polarity Score']
file['TextBlob Subjectivity Score'] = textblobresults['TextBlob Subjectivity Score']
file['Vader Negative Polarity Score'] = vaderresults['neg']
file['Vader Neutral Polarity Score']= vaderresults['neu']
file['Vader Positive Polarity Score'] = vaderresults['pos']
file['Vader Compound Polarity Score'] = vaderresults['compound']

# Changed CSV file name
file.to_csv('example6results_EB.csv', index=True, index_label="Index")

print("done")
```

<b> Overall Skills Developed </b>

<ins> Sentiment Analysis with Multiple Models: </ins>
- Developed the ability to apply and compare two sentiment tools (TextBlob and VADER), strengthening understanding of how different sentiment methods generate different emotional measurements.

<ins> Feature Engineering & Dataset Enrichment: </ins>
- Gained experience creating new analytical variables by generating sentiment scores and adding them as columns to an existing dataset for further analysis.

<ins> NLP Metrics Interpretation: </ins>
- Strengthened the ability to interpret sentiment outputs such as polarity, subjectivity, and compound sentiment scores, supporting meaningful comparison across songs and artists.

<ins> Iterative Text Processing at Scale: </ins>
- Practiced analyzing large collections of text by looping through lyric entries efficiently and storing results in structured formats for downstream use.

<ins> Data Structuring & Results Integration: </ins>
- Improved skills in organizing model outputs into Pandas DataFrames and merging multiple result sets into one unified dataset.

<ins> Reproducible Output & CSV Exporting: </ins>
- Learned how to generate reproducible outputs by exporting an analysis-ready CSV file that contains both original lyrics data and computed sentiment metrics.
