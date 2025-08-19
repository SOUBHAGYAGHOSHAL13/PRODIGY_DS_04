# PRODIGY_DS_04
Analyze and visualize sentiment patterns on social media data to understand public opinion and attitude towards specific topics or brands

✅ STEP-BY-STEP PROCESS

1. Define the Objective

Clarify:

Topic or Brand: E.g., “Coca-Cola,” “Apple iPhone,” or “climate change”

Platforms: Twitter/X, Instagram, Reddit, etc.

Time Frame: Last week, last month, etc.

2. Data Collection

You can collect data using:

APIs (e.g., Twitter/X API, Reddit API)

Scraping tools (e.g., snscrape, BeautifulSoup, or Scrapy)

Third-party tools: Like Brandwatch, Talkwalker, or Sprout Social.

Data fields to collect:

Post text

Date/time

Username (optional)

Likes/retweets/comments

Hashtags/mentions

3. Preprocess the Data

Clean and prepare the text:
# Python example (using pandas and NLTK)
import pandas as pd
import re
from nltk.corpus import stopwords

def clean_text(text):
    text = re.sub(r"http\S+", "", text)  # remove links
    text = re.sub(r"@\w+", "", text)  # remove mentions
    text = re.sub(r"#\w+", "", text)  # remove hashtags
    text = re.sub(r"[^\w\s]", "", text)  # remove punctuation
    text = text.lower()
    text = " ".join([word for word in text.split() if word not in stopwords.words('english')])
    return text

df['clean_text'] = df['text'].apply(clean_text)

4. Sentiment Analysis

Use tools like:

TextBlob: For basic polarity

VADER (for social media): Rule-based, good for emojis, slang

BERT-based models: For deep contextual analysis

from vaderSentiment.vaderSentiment import SentimentIntensityAnalyzer analyzer = SentimentIntensityAnalyzer() df['sentiment'] = df['clean_text'].apply(lambda x: analyzer.polarity_scores(x)['compound']) df['label'] = df['sentiment'].apply(lambda x: 'Positive' if x > 0.05 else ('Negative' if x < -0.05 else 'Neutral')) 

5. Visualization

📊 Sentiment Distribution

import seaborn as sns import matplotlib.pyplot as plt sns.countplot(x='label', data=df) plt.title('Sentiment Distribution') plt.show() 

📈 Sentiment Over Time

df['date'] = pd.to_datetime(df['date']) df.set_index('date', inplace=True) df.resample('D')['sentiment'].mean().plot(title='Average Sentiment Over Time') plt.show() 

🔥 Word Clouds

from wordcloud import WordCloud positive_text = " ".join(df[df['label'] == 'Positive']['clean_text']) WordCloud(width=800, height=400).generate(positive_text).to_image() 

6. Pattern Recognition

Look for:

Spikes in sentiment on specific days

Correlation with news/events

High-engagement posts with strong sentiment
