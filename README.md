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
