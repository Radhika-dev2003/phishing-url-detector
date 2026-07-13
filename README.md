# Phishing URL Detector

This is a machine learning project I built to detect phishing websites. It uses a Random Forest model trained on a dataset of about 11,000 websites, and can also check any live URL in real time by visiting the page and analyzing it.

# Why I built this

Phishing is one of the most common cyberattacks, and I wanted to combine my interest in machine learning with a real cybersecurity problem instead of doing a purely academic classification project. This project analyzes URL structure and webpage content to flag suspicious sites before a user clicks or enters their data.

# Dataset

I used the UCI Phishing Websites dataset (11,054 websites, 30 features), which is a well known dataset used in phishing detection research. Each website is labeled as either phishing or legitimate.

# Approach

I trained a Random Forest classifier on the full dataset first, which got 97.38% accuracy. But a lot of the original features (like domain age or website traffic rank) need paid APIs or external services that aren't practical to call in real time. So I retrained the model using only the 21 features that can actually be computed instantly, by parsing the URL and scraping the live webpage (things like HTTPS usage, redirects, whether links point off-domain, etc.). That version still got 95.61% accuracy, which was a good tradeoff for something that actually works on any URL you throw at it, not just the ones in the dataset.

# Results

| Metric | Phishing | Legitimate |
|---|---|---|
| Precision | 0.98 | 0.97 |
| Recall | 0.96 | 0.98 |
| F1-Score | 0.97 | 0.98 |

Feature importance — HTTPS usage and anchor tag behavior (whether links on the page point to a different domain) turned out to be the two strongest signals for phishing detection, which lines up with what real security research says.

- [Feature Importance Chart](feature_importance.png)
- [Confusion Matrix](confusion_matrix.png)
- [Accuracy Comparison](accuracy_comparison.png)

# How it works

1. Model is trained on the labeled dataset in `phishing_detector.ipynb`
2. For a new URL, the script visits the actual page and extracts 21 features in real time (HTTPS, redirects, form handling, iframe usage, anchor links, etc.)
3. The trained model predicts Phishing or Legitimate with a confidence score

# Tech used

Python, scikit-learn, Pandas, Requests, BeautifulSoup, Google Colab

# What I'd add next

- A simple Streamlit web app so people can test it without running the notebook
- A browser extension version
- Domain age lookup via WHOIS for extra accuracy

# Dataset source

[UCI Phishing Websites Dataset](https://archive.ics.uci.edu/dataset/327/phishing+websites)
