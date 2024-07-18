# PIT-Scoring
Categorization of books written by NYU Alliance scholars by their compatibility to Public Interest Technology (PIT)

## Abstract

The PIT-Scoring project aims to categorize books written by NYU Alliance scholars based on their compatibility with Public Interest Technology (PIT) themes. This involves developing a methodology to evaluate and score book descriptions against a set of PIT keywords. The process includes extracting text from various sources (PDFs, web pages), preprocessing the text by tokenizing, removing stop words and punctuation, and performing part-of-speech tagging to focus on relevant words. Using techniques such as TF-IDF (Term Frequency-Inverse Document Frequency) and cosine similarity, the project calculates a compatibility score for each book description. This structured approach ensures a comprehensive evaluation of the books, providing a reliable measure of their alignment with PIT-related topics. The descriptions of explicitly PIT-related books were utilized to extract relevant keywords, further enhancing the accuracy and relevance of the scoring system.

## Summary and Goal

The goal of this project is to develop a methodology to evaluate and score the compatibility of book descriptions with Public Interest Technology (PIT) keywords. This involves extracting text from various sources (PDFs, web pages), preprocessing the text to focus on relevant words, and calculating a compatibility score using techniques such as TF-IDF and cosine similarity.

## Install Necessary Packages

To perform the necessary text extraction, preprocessing, and analysis, the following packages need to be installed:

```python
!pip install pymupdf requests beautifulsoup4 nltk sklearn rake_nltk spacy
```

## Step 1: Define Functions to Extract Text from PDF and Webpage

### Purpose

These functions are designed to extract raw text data from PDF files and web pages, which will be used as input for further text analysis.

### Function: `extract_text_from_pdf`
- **Purpose**: Extracts text from a PDF file.
- **Parameters**: `pdf_path` (str) - Path to the PDF file.
- **Returns**: Extracted text as a string.
- **Process**: Opens the PDF, iterates through each page, and concatenates the text from each page into a single string.

### Function: `extract_text_from_webpage`
- **Purpose**: Extracts text from a web page.
- **Parameters**: `url` (str) - URL of the web page.
- **Returns**: Extracted text as a string.
- **Process**: Sends a GET request to the URL, parses the HTML content, and extracts the text.

## Step 2: Define a Function to Preprocess the Extracted Text

### Purpose

This function preprocesses the raw text to make it suitable for analysis by tokenizing, removing stop words and punctuation, and performing part-of-speech (PoS) tagging.

### Detailed Explanation of Preprocessing

1. **Tokenization**: This is the process of breaking down text into smaller units called tokens (usually words). For example, the sentence "The quick brown fox" would be tokenized into ["The", "quick", "brown", "fox"].

2. **Stop Words Removal**: Stop words are common words (like "the", "is", "in") that do not carry significant meaning and are often removed to focus on the more important words.

3. **Punctuation Removal**: Punctuation marks (like periods, commas, etc.) are removed as they do not contribute to the meaning in most text analysis tasks.

4. **Part-of-Speech (PoS) Tagging**: This involves labeling each word with its part of speech (noun, verb, adjective, etc.). For this project, we are particularly interested in nouns and adjectives as they are likely to represent the key topics and concepts.

### Function: `preprocess_text`
- **Purpose**: Preprocesses the extracted text.
- **Parameters**: `text` (str) - Extracted text.
- **Returns**: List of preprocessed words.
- **Process**: Tokenizes the text, removes stop words and non-alphanumeric characters, and converts the text to lowercase.

### Function: `extract_nouns_and_adjectives`
- **Purpose**: Extracts nouns and adjectives from the preprocessed text.
- **Parameters**: `text_list` (list) - List of preprocessed words.
- **Returns**: List of extracted nouns and adjectives.
- **Process**: Uses spaCy to process the text and extract lemmas of nouns and adjectives.

### Function: `unique_nouns_and_adjectives`
- **Purpose**: Extracts unique nouns and adjectives from the list of text.
- **Parameters**: `text_list` (list) - List of extracted words.
- **Returns**: List of unique nouns and adjectives.
- **Process**: Counts occurrences of each word and returns only those that appear once.

## Step 3: Define TF-IDF Vectorizer to Extract Keywords

### Purpose

This function uses the TF-IDF (Term Frequency-Inverse Document Frequency) method to identify and extract the most important keywords from the preprocessed text.

### Detailed Explanation of TF-IDF

1. **Term Frequency (TF)**: Measures how frequently a term appears in a document. The assumption is that the more a term appears in a document, the more important it is.

2. **Inverse Document Frequency (IDF)**: Measures how important a term is in the entire corpus of documents. It reduces the weight of terms that appear very frequently across many documents and increases the weight of terms that appear less frequently.

3. **TF-IDF Score**: Combines TF and IDF to calculate the importance of a term in a document relative to the entire corpus. A high TF-IDF score indicates that the term is significant in that document.

### Function: `extract_top_keywords`
- **Purpose**: Extracts top keywords from a list of texts.
- **Parameters**: `texts` (list) - List of texts; `top_n` (int) - Number of top keywords to extract.
- **Returns**: List of top keywords.
- **Process**: Creates a TF-IDF vectorizer, transforms texts into a TF-IDF matrix, and extracts top keywords based on their TF-IDF scores.

## Step 4: Combine the Preprocessed Text Chain

### Purpose

Combines and preprocesses text from various PIT-related documents and sources to build a comprehensive set of keywords.

### Process
- **Extract and preprocess text**: Extract text from PDFs and web pages, preprocess it, and extract nouns and adjectives.
- **Combine and filter unique words**: Combine the unique nouns and adjectives from all sources.

## Step 5: Add Description of Books for PIT-Related Publication

### Purpose

Extracts and preprocesses descriptions of selected books to analyze their compatibility with PIT keywords.

### Process
- **Extract and preprocess text**: Extract text from web pages containing book descriptions, preprocess it, and extract unique nouns and adjectives.

## Step 6: Update Keywords with Additional Text Chain

### Purpose

Enhances the keyword list with additional PIT-related texts and extracts the top 100 keywords using the refined text chain.

### Example Book Descriptions for PIT-Related Publication

The descriptions of these books were used to extract PIT keywords due to their explicit relevance to Public Interest Technology:
- **“Black Software”: The Internet & Racial Justice - Charlton McIlwain (PB001)**
- **Ethical Data Science: Prediction in the Public Interest - Anne Washington (PB002)**
- **Privacy, Big Data, and the Public Good: Frameworks for Engagement - Julia Lane (PB003)**

### Process
- **Combine all preprocessed texts**: Combine all unique words from PIT-related documents and book descriptions.
- **Extract top keywords**: Use the TF-IDF vectorizer to extract the top 100 keywords.

## Step 7: Create a PIT-Scoring Function

### Purpose

Defines a function to score the compatibility of book descriptions with the PIT keywords using cosine similarity.

### Detailed Explanation of Cosine Similarity

1. **Cosine Similarity**: Measures the cosine of the angle between two vectors. It is a measure of similarity between two non-zero vectors. In the context of text analysis, it compares the vector representations of documents (TF-IDF vectors) to determine how similar they are.

2. **Range of Cosine Similarity**:
   - **1**: The documents are identical.
   - **0**: There is no similarity between the documents.
   - **-1**: The documents are completely dissimilar (in text analysis, usually values range between 0 and 1).

### Function: `cosine_similarity_scoring`
- **Purpose**: Scores the PIT-compatibility of a book description.
- **Parameters**: `text_string` (str) - Extracted text of book description; `keywords` (list) - List of PIT keywords.
- **Returns**: Compatibility score (float).
- **Process**: Preprocesses the text, combines it with keywords, transforms it using TF-IDF, and calculates the cosine similarity score.

## Step 8: Import Data and Add Compatibility Score

### Purpose

Applies the PIT-scoring function to a dataset of book descriptions to calculate and add the compatibility scores.

### Process
- **Load dataset**: Load a CSV file containing book descriptions.
- **Calculate and add scores**: Iterate through each book description, calculate the compatibility score using the PIT-scoring function, and add the score to the dataset.

This structured approach ensures a thorough evaluation of book descriptions against PIT keywords, providing a reliable measure of their compatibility with Public Interest Technology themes.
