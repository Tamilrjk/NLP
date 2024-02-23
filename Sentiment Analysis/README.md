# Sentiment Analysis of YouTube Video Comments

This project analyzes the sentiment of comments on YouTube videos, exploring the opinions and emotions expressed by viewers. By using  VADER Seniment Scoring, it aims to understand audience reception, identify trends, and potentially offer insights for content creators and researchers. This project can be valuable for Data Sciences

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
