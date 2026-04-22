# Mental-NLP-Classifiers

Exploring Mental Health Conditions Through Linguistic Patterns in Online Texts

## About the Project

This project investigates Natural Language Processing (NLP) methods for the multi-class classification of mental health conditions from written texts. By combining deep semantic representations with engineered linguistic features, the study explores how language patterns may reflect psychological states and whether these signals can support automatic text classification.

## Dataset

The analysis is based on the **Kanakmi Mental Disorders Dataset**, which originally contains more than **580,000 forum posts**.

### Target Classes
The dataset includes the following diagnostic categories:
- BPD
- Bipolar
- Depression
- Anxiety
- Schizophrenia

### Data Balancing
To address class imbalance and reduce computational cost, **random undersampling** was applied, resulting in a balanced dataset of **39,976 records**.

### Exclusions
The generic **"MentalIllness"** category was excluded in order to focus only on specific diagnostic classes.

## Methodology

### 1. Data Cleaning and Preprocessing

The preprocessing pipeline included:
- removal of non-English texts
- removal of very short texts (under 200 characters)
- removal of extreme outliers (texts longer than approximately 4,443 tokens)
- tokenization, lowercasing, lemmatization, and removal of stop words and punctuation using **spaCy**
- removal of HTML/XML tags using **BeautifulSoup**
- removal of emojis due to their low frequency in the dataset

### 2. Feature Engineering

The models were built on a multidimensional feature representation including:

#### Lexical and Syntactic Features
- Standardized Type-Token Ratio (STTR)
- text length
- average sentence length

#### Discourse Structure
- average sentence coherence score

#### Affective and Emotional Features
- dominant emotions extracted with a **RoBERTa-based GoEmotions classifier**
- sentiment polarity predicted with a **fine-tuned RoBERTa model**

#### Toxicity
- toxicity probability scores obtained with **Detoxify**
- logit transformation applied to the toxicity scores

#### Embeddings
- **DistilBERT** contextual embeddings extracted from the **[CLS] token** (768 dimensions)

## Models and Results

Three supervised models were implemented and evaluated:

### Multi-Layer Perceptron (MLP)
- Test Accuracy: **0.69**
- Weighted F1-score: **0.68**

This model achieved the best overall performance. It used a bottleneck architecture and was optimized with the **AdamW** optimizer and **Early Stopping**.

### Support Vector Machine (SVM)
- Test Accuracy: **0.68**
- Weighted F1-score: **0.68**

A linear kernel SVM produced results close to those of the MLP.

### XGBoost
- Test Accuracy: **0.66**
- Weighted F1-score: **0.66**

This model was optimized through **Bayesian optimization with Optuna**.

### Key Findings
Performance differed across classes. In particular, **Anxiety** and **Schizophrenia** obtained the highest F1-scores across models.

## Explainability

To improve interpretability, two post-hoc explanation methods were applied:

### LIME
Local explanations suggested that the neural model relies on a hybrid decision process: DistilBERT embeddings provide semantic information, while some engineered features, such as toxicity and emotional signals, can become decisive in specific predictions.

### SHAP
Global feature importance analysis showed that contextual embeddings play a major role overall, while some engineered linguistic features appear especially relevant for specific classes. For example:
- **STTR** was particularly important for **BPD**
- **toxicity scores** were strong indicators for **Depression**

## Authors
- Matilde Viti
- Elisa Calabrese
- Francesco Curia
- Alessia Di Gioia
