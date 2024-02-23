# Sentiment Analysis of YouTube Video Comments

This project analyzes the sentiment of comments on YouTube videos, exploring the opinions and emotions expressed by viewers. By using  VADER Sentiment Scoring, it aims to understand audience reception, identify trends, and potentially offer insights for content creators and researchers. This project can be valuable for Data Sciences.

## List the libraries

## Web derives

## Scrapping the data


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

~~~ bash
# Create a new instance of the Microsoft Edge WebDriver
driver = webdriver.Edge()
~~~
This line creates a new instance of the Microsoft Edge WebDriver (`webdriver.Edge()`) and assigns it to the variable `driver`. This allows our code to interact with and automate an Edge browser window in the background.

Make sure you have the Selenium library installed (pip install selenium) and the Microsoft Edge WebDriver downloaded and properly configured in your system environment variables. You can find more information about the Edge WebDriver here: https://developer.microsoft.com/en-us/microsoft-edge/tools/webdriver
