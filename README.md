## **Exploring Mental Health Conditions Through Linguistic Patterns in Online Texts**

This repository contains the code and resources for this project. 
It was developed for the Text Analytics Course 
at the University of Pisa (A.Y. 2025/2026).

**[Read the full Project Report (PDF)](ProjectReport.pdf)**

## Authors
* Matilde Viti
* Elisa Calabrese
* Francesco Curia
* Alessia Di Gioia

## About the Project
This study evaluates Natural Language Processing (NLP) methods for the multi-class classification of mental health conditions from written text. By integrating deep semantic representations with specific engineered features, the project aims to demonstrate how linguistic patterns can reflect psychological states, providing a foundational step toward NLP-assisted mental health support tools.

## Dataset
The analysis is based on the Kanakmi Mental Disorders Dataset, which originally comprises over 580,000 forum posts. 
* **Target Classes**: The data covers diagnostic categories such as BPD, Bipolar, Depression, Anxiety, and Schizophrenia. 
* **Data Balancing**: To address heavy class imbalance and optimize computational resources, random undersampling was applied, resulting in a balanced corpus of 39,976 records. 
* **Exclusions**: The generic "MentalIllness" category was excluded to focus strictly on specific clinical classes.

## Methodology

### 1. Data Cleaning and Preprocessing
* **Filtering**: Non-English texts, extremely short texts (under 200 characters), and severe outliers (texts exceeding approximately 4,443 tokens) were removed .
* **Normalization**: The spaCy library was used for tokenization, lowercasing, lemmatization, and the removal of stop words and punctuation .
* **Noise Reduction**: Emojis were removed due to sparse prevalence, and HTML/XML tags were stripped using BeautifulSoup .

### 2. Feature Engineering
The models leverage a multidimensional feature representation:
* **Lexical & Syntactic Features**: Standardized Type-Token Ratio (STTR) for lexical diversity, text length, and average sentence length .
* **Discourse Structure**: Semantic consistency was modeled using an average sentence coherence score .
* **Affective & Emotional Features**: A RoBERTa-based GoEmotions classifier extracted dominant emotions, and a fine-tuned RoBERTa model classified sentiment polarity . 
* **Toxicity**: A BERT-based transformer (Detoxify) provided toxicity probability scores, which were logit-transformed .
* **Embeddings**: DistilBERT was utilized to extract 768-dimensional contextual embeddings from the [CLS] token .

## Models and Results
Three supervised learning approaches were implemented and optimized:

* **Multi-Layer Perceptron (MLP)**: The neural network achieved the highest overall performance with a test accuracy of 0.69 and a weighted F1-score of 0.68 . The model used a bottleneck design and was optimized using the AdamW optimizer with Early Stopping .
* **Support Vector Machine (SVM)**: A linear kernel SVM achieved a test accuracy of 0.68 and a weighted F1-score of 0.68 . 
* **XGBoost**: The ensemble tree model, optimized via Bayesian optimization (Optuna), achieved a test accuracy of 0.66 and a weighted F1-score of 0.66 .

**Key Findings**: Performance varied across categories, with Anxiety and Schizophrenia consistently showing the highest F1-scores across models .

## Explainability
To ensure clinical validity and model transparency, post-hoc interpretability techniques were applied :
* **LIME**: Local explanations revealed that neural models use a hybrid decision process . The model relies on the DistilBERT embeddings for semantic context, while specific manual features (e.g., toxicity or emotional markers) act as decisive factors .
* **SHAP**: Global feature importance analysis showed that while contextual embeddings dominate decision-making, specific linguistic markers vary by class . For example, lexical diversity (STTR) is crucial for identifying BPD, whereas toxicity scores strongly signal Depression .
