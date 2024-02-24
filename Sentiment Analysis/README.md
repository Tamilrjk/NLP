# Sentiment Analysis of YouTube Video Comments

This project analyzes the sentiment of comments on YouTube videos, exploring the opinions and emotions expressed by viewers. By using  VADER Sentiment Scoring, it aims to understand audience reception, identify trends, and potentially offer insights for content creators and researchers. This project can be valuable for Data Sciences.


## List the libraries and their roles:
~~~ bash
import pandas as pd
from selenium import webdriver
from selenium.webdriver.common.by import By
import time

~~~

pandas: "Used for data manipulation and analysis of scraped comments."

selenium: "Used to automate web scraping of comments from YouTube videos."

selenium.webdriver.common.by: "Provides methods for locating elements on web pages 

# WebDrives
~~~ bash
# Create a new instance of the Microsoft Edge WebDriver
driver = webdriver.Edge()
~~~

This line creates a new instance of the Microsoft Edge WebDriver (`webdriver.Edge()`) and assigns it to the variable `driver`. This allows our code to interact with and automate an Edge browser window in the background.

Make sure you have the Selenium library installed (pip install selenium) and the Microsoft Edge WebDriver downloaded and properly configured in your system environment variables. You can find more information about the Edge WebDriver here: https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver.

# Breakdown
~~~bash
# Navigate to the YouTube video page
driver.get("https://www.youtube.com/watch?v=HN54x-eShFU")

# Give some time for the video to load
time.sleep(5)
~~~
driver.get("https://www.youtube.com/watch?v=HN54x-eShFU"): This line of code uses the driver object, which is likely a web browser automation tool like Selenium, to navigate to the specified YouTube video URL.

time.sleep(5): This line pauses the execution of the code for 5 seconds, presumably to allow the video to load before proceeding.

~~~bash
# Scroll down to load comments
SCROLL_PAUSE_TIME = 2
last_height = driver.execute_script("return document.documentElement.scrollHeight")

while True:
    driver.execute_script("window.scrollTo(0, document.documentElement.scrollHeight);")
    time.sleep(SCROLL_PAUSE_TIME)
    new_height = driver.execute_script("return document.documentElement.scrollHeight")
    if new_height == last_height:
        break
    last_height = new_height

# Find and extract comments
comments_elements = driver.find_elements(By.CSS_SELECTOR, "#content-text")
comments = [comment.text for comment in comments_elements]

# Close the WebDriver
driver.quit()
~~~
SCROLL_PAUSE_TIME = 2: This line defines a variable SCROLL_PAUSE_TIME with the value 2, representing a 2-second pause between each scroll action.

last_height = driver.execute_script("return document.documentElement.scrollHeight"): This line retrieves the current height of the entire webpage content.

while True:: This starts an infinite loop that continues until a specific condition is met.
driver.execute_script("window.scrollTo(0, document.documentElement.scrollHeight);"): This line scrolls the webpage vertically to the bottom, using JavaScript.

time.sleep(SCROLL_PAUSE_TIME): This line pauses the execution for 2 seconds, allowing the page to load additional content after scrolling.
new_height = driver.execute_script("return document.documentElement.scrollHeight"): This line retrieves the new height of the webpage content after scrolling.

if new_height == last_height:: This condition checks if the webpage height hasn't changed after the scroll, indicating no new content is loaded.

break: If the condition is true, the loop breaks, ending the scrolling process.

comments_elements = driver.find_elements(By.CSS_SELECTOR, "#content-text"): This line finds all HTML elements with the ID "content-text", which likely contain the comment text.

comments = [comment.text for comment in comments_elements]: This line extracts the text content from each comment element and stores it in a list named comments.

driver.quit(): This line closes the WebDriver instance, presumably used for browser automation.

~~~bash
# Create a DataFrame
df = pd.DataFrame({'Comments': comments})

# Print the DataFrame
print(df)

# Optionally, you can export the DataFrame to a CSV file
df.to_csv('comments_dataframe.csv', index=False)
~~~
This line creates a DataFrame named df from the list of comments. The DataFrame has one column named "Comments" and each row contains a comment from the list.
~~~bash
# Separate time and comments
df['Time'] = df['Comments'].str.extract(r'(\d{1,2}:\d{2})')
df['Comment'] = df['Comments'].str.replace(r'\d{1,2}:\d{2}', '').str.strip()
~~~
This line adds a new column named "Time" to the DataFrame.
It uses the str.extract method of the Comments Series to extract all occurrences of a regular expression pattern r'(\d{1,2}:\d{2}).
This pattern matches strings containing 1 or 2 digits, followed by a colon, and then 2 more digits, which likely represent the time format in your comments.

The extracted time values are stored in the newly created "Time" column

# Sentiment Analysis
~~~bash
import re
import nltk
from nltk.tokenize import word_tokenize
from nltk.corpus import stopwords
import string
~~~
Imports the re module, which provides functions for working with regular expressions. Regular expressions are patterns that can be used to match, search, and manipulate text.

Imports the nltk (Natural Language Toolkit) library, which is a powerful tool for working with human language data. It offers a wide range of functionalities for tasks like text processing, tokenization, stemming, lemmatization, part-of-speech tagging, parsing, and more.

Specifically imports the word_tokenize function from the nltk.tokenize module. This function is used to split text into individual words or tokens, preparing it for further analysis.

imports the stopwords corpus from the nltk.corpus module. This corpus contains a list of common stop words in a particular language (e.g., "the", "a", "an", "in", "of"). Stop words are often filtered out during text processing as they don't carry much semantic meaning.

Imports the string module, which provides constants for various string operations. It's often used for tasks like removing punctuation from text.
~~~bash
nltk.download('stopwords')
nltk.download('punkt')
~~~
This line downloads the stopwords corpus, which contains a list of common words in English (like "the", "a", "an", "in", "of") that often don't carry much semantic meaning. These words are frequently removed during text processing tasks as they can add noise and reduce the focus on more meaningful content.

This line downloads the punkt model, which is used for sentence tokenization. It helps you split text into individual sentences, allowing for further analysis at the sentence level. This model is particularly useful for tasks like sentiment analysis or topic modeling where understanding the context of words within sentences is important.
~~~bash
# Remove irrelevant characters like emojis, symbols, or special characters
df['Comment'] = df['Comment'].apply(lambda x: re.sub(r'[^\w\s]', '', x))
# Tokenize the comments into individual words or phrases
df['Tokenized_Comments'] = df['Comment'].apply(word_tokenize)
#Normalize the text by converting everything to lowercase
df['Tokenized_Comments'] = df['Tokenized_Comments'].apply(lambda x: [word.lower() for word in x])
# Remove stop words and punctuation
stop_words = set(stopwords.words('english'))  # Assuming English language
df['Tokenized_Comments'] = df['Tokenized_Comments'].apply(lambda x: [word for word in x if word not in stop_words])

# Remove punctuation marks
df['Tokenized_Comments'] = df['Tokenized_Comments'].apply(lambda x: [word for word in x if word.isalpha()])
~~~
## Remove irrelevant characters
This line uses the apply method and a lambda function to iterate through each comment in the "Comment" column.
The regular expression r'[^\w\s]' matches any character that is not a word character (letter, digit, or underscore) or whitespace.
## Tokenize comments
This line also uses the apply method and a lambda function to tokenize each comment.
The word_tokenize function from nltk splits the comments into individual words or phrases based on the punkt model downloaded earlier. This creates a new column named "Tokenized_Comments" containing a list of tokens for each comment.
## Normalize text
This line iterates through each list of tokens in "Tokenized_Comments" and applies a lambda function to convert all words to lowercase.
This step helps standardize the text and potentially improves the accuracy of further processing steps.

The re.sub function replaces all matched characters with an empty string, effectively removing them from the comments.
##  Remove stop words and punctuation
his line creates a set containing stop words for the English language using the stopwords corpus downloaded earlier.
df['Tokenized_Comments'] = df['Tokenized_Comments'].apply(lambda x: [word for word in x if word not in stop_words])
This line filters out stop words from each list of tokens. It iterates through each list and keeps only the words that are not present in the stop_words set.
df['Tokenized_Comments'] = df['Tokenized_Comments'].apply(lambda x: [word for word in x if word.isalpha()])
This line further removes any remaining words that are not alphabetical characters. This step removes punctuation and other non-word characters.
# Word Cloud
~~~bash
from wordcloud import WordCloud
import matplotlib.pyplot as plt
# Concatenate all comments into a single string
all_comments = ' '.join(df['Comment'])

# Generate the word cloud
wordcloud = WordCloud(width=800, height=400, background_color='white').generate(all_comments)

# Visualize the word cloud
plt.figure(figsize=(10, 6))
plt.imshow(wordcloud, interpolation='bilinear')
plt.title('YouTube Comments Word Cloud')
plt.axis('off')  # Hide axes
plt.show()
~~~
from wordcloud import WordCloud: Imports the WordCloud class from the wordcloud library, which is used for creating word clouds.
import matplotlib.pyplot as plt: Imports the pyplot module from matplotlib for visualizing the generated word cloud.

all_comments = ' '.join(df['Comment']): Combines all comments from the "Comment" column in your DataFrame into a single string, creating a continuous text input for the word cloud.
Creates a WordCloud object with specified dimensions (width=800, height=400) and a white background color.
Calls the generate method on the wordcloud object, passing the all_comments string as input. This generates the word cloud, where word frequencies and prominence are visually represented.


![Image description](Sentiment Analysis/download.png)

