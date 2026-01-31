# Python - Spotify Analysis

### Description 
- In this assignment, I applied Python-based data collection and natural language processing (NLP) techniques to analyze song lyrics and album artwork from multiple music artists. The project included API-based data scraping, text cleaning, sentiment and emotion analysis, profanity detection, and image analytics. Through visualizations and comparative analysis, I developed skills in transforming unstructured media data into actionable insights for real-world analytical applications.

## 

<b>Part 2 – Lyrics Text Cleaning and Preprocessing (NLP Preparation)</b>

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
