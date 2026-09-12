# 📰 Arabic News NLP Project

> A comprehensive, end-to-end Natural Language Processing pipeline for Arabic news articles featuring text classification, extractive summarization, sentiment analysis, topic modeling, and entity extraction.

**Language:** 100% Jupyter Notebook | **Status:** Complete Implementation | **Dataset:** 8,378 Arabic News Articles

---

## 🎯 What This Project Does

This project demonstrates a **production-grade NLP workflow** for Arabic-language news processing. It implements multiple complementary NLP techniques to automatically classify news articles, extract key insights, identify sentiment, discover thematic patterns, and summarize content — all within a unified pipeline.

### Quick Facts
- **Dataset Size:** 8,378 articles across multiple news categories
- **Best Classification Accuracy:** **80.63%** (SVM with TF-IDF word n-grams)
- **Preprocessing Steps:** 8 stages of Arabic-specific text cleaning
- **Models Compared:** 5 different classification approaches
- **NLP Tasks:** Classification | Summarization | Sentiment Analysis | Topic Modeling | Entity Extraction

---

## ⚡ Performance at a Glance

| Approach | Feature Representation | Accuracy |
|----------|----------------------|----------|
| **SVM with Word N-grams** | TF-IDF | **80.63%** ✓ |
| SVM with Character N-grams | TF-IDF | 80.26% |
| SVM with Stemming | TF-IDF | 80.51% |
| Random Forest | TF-IDF | 76.10% |
| CNN with Embeddings | Word Embeddings | 74.30% |

**Text Summarization (Extractive):**
- ROUGE-1: 0.2468
- ROUGE-2: 0.1337
- ROUGE-L: 0.2462

---

## 📂 Project Overview

### Core Components

```
Dataset
   ↓
Data Validation & Cleaning
   ↓
Arabic NLP Preprocessing
   ↓
Exploratory Data Analysis
   ↓
Feature Engineering (TF-IDF, Embeddings)
   ↓
┌─────────────────────────────────────────────────┐
│   Text Classification    │   Summarization       │
│   • SVM                  │   • TextRank          │
│   • Random Forest        │   • ROUGE Evaluation  │
│   • CNN                  │   • Extractive Method │
└─────────────────────────────────────────────────┘
   ↓
Sentiment Analysis (Lexicon-based)
   ↓
Topic Modeling (LDA)
   ↓
Entity Extraction (Rule-based)
   ↓
Unified Text Analysis Pipeline
```

---

## 📊 Dataset Details

### Structure
- **Total Records:** 8,378 Arabic news articles
- **Columns:** 4 (original_text, category, processed_text, summary)
- **Data Quality:**
  - No missing values
  - 298 duplicate rows removed
  - Minimum article length: 12 words
  - Maximum article length: 1,597 words
  - Average length: 112 words

### Statistics
| Metric | Value |
|--------|-------|
| Average Characters | 663.66 |
| Median Characters | 524.50 |
| Average Words | 111.75 |
| Median Words | 89 |

### News Categories
The dataset contains articles from **two primary categories**:
- **Culture** (الثقافة) - Cultural events, arts, and heritage
- **Diverse** - General news, science, technology, and miscellaneous topics

Category distribution shows a focus on cultural content with balanced representation of general interest news.

---

## 🔧 Arabic NLP Preprocessing Pipeline

The project implements an **8-step Arabic-specific text cleaning pipeline**:

### Preprocessing Stages

1. **Remove Diacritics** (الحركات)
   - Eliminates Arabic vowel marks and diacritical signs
   - Pattern: `[\u064B-\u0652]`

2. **Remove Tatweel** (التطويل)
   - Removes Arabic letter elongation character (ـ)
   - Pattern: `\u0640`

3. **Arabic Character Normalization**
   - Unify alef variants: أ, إ, آ → ا
   - Normalize alef maksura: ى → ي
   - Unify teh marbuta: ة → ه
   - Normalize hamza variants: ؤ → و, ئ → ي

4. **Tokenization**
   - Split text into individual tokens

5. **Stopword Removal**
   - NLTK Arabic stopwords
   - Custom manual stopword list
   - Removes ~95 common Arabic words

6. **Short Token Filtering**
   - Removes tokens with length < 3 characters

7. **ISRI Stemming**
   - Light stemming using NLTK's ISRI Stemmer
   - Reduces words to root forms while maintaining readability

8. **Data Validation**
   - Drop duplicates
   - Remove null values
   - Filter articles with fewer than 10 words

### Before/After Impact
- **Tokens before cleaning:** Original raw text with diacritics and variations
- **Tokens after cleaning:** Normalized, stemmed, stopword-free vocabulary
- Word clouds show significant noise reduction and improved term frequency clarity

---

## 📈 Exploratory Data Analysis

### Character & Word Length Analysis
- **Character Distribution:** Mean 663.66 ± 491.51 (range: 82-9,854)
- **Word Count Distribution:** Mean 111.75 ± 81.59 (range: 12-1,597)
- Most articles cluster around 89-133 words (IQR: Q1=62, Q3=133)

### Category Insights
- Word count patterns vary by category
- Culture articles generally follow similar length distribution
- Diverse category shows comparable word length patterns

### Word Cloud Analysis
- **Pre-cleaning:** Dense with common prepositions, articles, and diacritics
- **Post-cleaning:** Clear focus on meaningful content words
- Top terms align with news content themes

---

## 🎓 Text Classification Models

### Feature Representations Tested

#### 1. **TF-IDF Features**
- **Word-level N-grams:** (1,2) unigrams and bigrams
- **Vectorizer:** `max_features=5000`
- **Sparse representation:** Reduces dimensionality while preserving term importance

#### 2. **Character N-grams**
- **Range:** (2,3) character n-grams
- **Purpose:** Captures morphological patterns in Arabic text
- **Alternative approach:** Language-agnostic features

#### 3. **Word Embeddings**
- **Method:** Word-level tokenization for CNN
- **Embedding dimension:** Learned during training
- **Max sequence length:** Padded/truncated to consistent size

### Classification Models Evaluated

#### Support Vector Machine (SVM) - LinearSVC
- **Best Performer** 🏆
- **TF-IDF with Word N-grams:** 80.63% accuracy
- **TF-IDF with Character N-grams:** 80.26% accuracy
- **With Stemming:** 80.51% accuracy
- **Train/Test Split:** 80/20 with random_state=42
- **Characteristics:** Effective for high-dimensional sparse features

#### Random Forest Classifier
- **Accuracy:** 76.10%
- **Feature Input:** TF-IDF sparse matrix
- **Trade-off:** Good interpretability, lower accuracy than SVM
- **Advantage:** Feature importance analysis

#### Convolutional Neural Network (CNN)
- **Architecture:**
  - Input: Word embeddings (learned)
  - Conv1D layer: 100 filters, kernel size 3
  - GlobalMaxPooling1D
  - Dense layer: 64 units
  - Dropout: 0.5
  - Output: Softmax
- **Accuracy:** 74.30%
- **Framework:** TensorFlow/Keras
- **Training:** EarlyStopping callback
- **Observation:** Deep learning approach underperforms classical methods on this dataset

### Model Comparison

| Model | Feature Type | Parameters | Accuracy |
|-------|--------------|-----------|----------|
| SVM (Word N-grams) | TF-IDF | Kernel='linear' | **80.63%** |
| SVM (Char N-grams) | TF-IDF | Kernel='linear' | 80.26% |
| SVM (Stemmed) | TF-IDF | Kernel='linear' | 80.51% |
| Random Forest | TF-IDF | n_estimators=100 | 76.10% |
| CNN | Embeddings | Conv1D + Dense | 74.30% |

**Key Insight:** TF-IDF with SVM proves superior for Arabic news classification, suggesting that explicit term frequency patterns capture more discriminative information than learned embeddings for this dataset size and domain.

---

## 📝 Text Summarization - TextRank Algorithm

### Implementation Details

The project implements **extractive summarization** using TextRank, a graph-based ranking algorithm:

**Algorithm Steps:**
1. **Sentence Splitting:** Divide article into sentences
2. **Sentence Tokenization:** Tokenize and clean each sentence
3. **Sentence Representation:** Build TF-IDF vectors for each sentence (word-level)
4. **Similarity Matrix:** Compute cosine similarity between all sentence pairs
5. **Graph Construction:** Create similarity graph with sentences as nodes
6. **PageRank:** Apply PageRank algorithm to rank sentences
7. **Summary Extraction:** Select top-ranked sentences
8. **Order Preservation:** Maintain original article sentence order

**Why TextRank?**
- Unsupervised approach (no training labels needed)
- Language-agnostic algorithm
- Effective for single-document summarization
- Graph-based ranking captures semantic relationships

### Evaluation Metrics (Evaluated on 300 articles)

| Metric | Score |
|--------|-------|
| ROUGE-1 (Unigram Overlap) | 0.2468 |
| ROUGE-2 (Bigram Overlap) | 0.1337 |
| ROUGE-L (Longest Common Subsequence) | 0.2462 |

**Interpretation:**
- ROUGE-1 of 0.2468 indicates ~24.7% unigram overlap with reference summaries
- Modest scores reflect the challenge of extractive summarization on very brief news articles
- TextRank performs reasonably well for unsupervised extraction

---

## 💭 Sentiment Analysis

### Lexicon-Based Sentiment Classification

The project implements **manual lexicon-based sentiment analysis** using Arabic word lists:

**Components:**
- **Positive Word List:** Manually curated Arabic positive adjectives and descriptive terms
- **Negative Word List:** Manually curated Arabic negative adjectives and descriptive terms
- **Sentiment Scoring:** Count-based scoring with positive/negative word matches
- **Classification:** Three-class output (Positive | Neutral | Negative)

**Scoring Logic:**
1. Tokenize and clean article text
2. Match tokens against positive and negative word lists
3. Calculate sentiment score: `(positive_count - negative_count)`
4. Classify:
   - Score > threshold → Positive
   - Score < -threshold → Negative
   - Otherwise → Neutral

**Important Note:**
This is a **rule-based sentiment analyzer**, not a neural network model. It depends on lexicon coverage and does not learn from training data. Effectiveness limited to vocabulary in the word lists.

---

## 🎯 Topic Modeling with LDA

### Latent Dirichlet Allocation Implementation

**Configuration:**
- **Number of Topics:** 6
- **Library:** Gensim
- **Corpus Type:** Bag-of-words representation

**Pipeline:**
1. **Dictionary Creation:** Build vocabulary dictionary from corpus
2. **Corpus Representation:** Convert documents to BoW corpus
3. **Filtering:** Remove extremes (min_docs, max_docs)
4. **LDA Training:** Fit LDA model on corpus
5. **Topic Extraction:** Extract top words per topic

**Topic Results:**
The model identifies **6 primary latent topics** from the news articles. Each topic is represented by its top contributing words, revealing thematic patterns in the dataset (e.g., cultural events, regional news, organizational activities).

---

## 🔍 Entity Extraction

### Rule-Based Named Entity Recognition

The project implements **keyword/pattern-based entity extraction** for three entity types:

**Entity Categories:**
1. **Locations (Places)**
   - Pattern-based matching against location keywords
   - Examples: Countries, cities, regions (e.g., Tunisia, Morocco, Egypt)

2. **Organizations**
   - Pattern-based matching against organization keywords
   - Examples: Government bodies, cultural institutions, companies

3. **Person Names**
   - Pattern-based matching against known names
   - Examples: Recognized person names in Arabic

**Important Note:**
This is a **rule-based/keyword approach**, NOT a pretrained NER model like spaCy or transformers. It relies on static dictionaries and pattern lists. Performance limited to entities in the keyword lists.

**Characteristics:**
- High precision for known entities
- Limited recall (misses entities not in the dictionary)
- No context understanding
- No morphological adaptation for Arabic case/gender variations

---

## 🚀 End-to-End Text Analysis Pipeline

### Unified `analyze_text()` Function

The project culminates in a comprehensive analysis function that processes a raw Arabic news article through the complete pipeline:

```python
def analyze_text(article):
    # Input: Raw Arabic news article (string)
    
    # Step 1: Text Cleaning
    - Remove diacritics, tatweel, normalize characters
    - Tokenize, remove stopwords, stem
    
    # Step 2: Category Prediction
    - Vectorize with trained TF-IDF vectorizer
    - Classify using trained SVM model
    - Output: Predicted category + confidence
    
    # Step 3: Sentiment Analysis
    - Match against positive/negative lexicons
    - Output: Sentiment (Positive | Neutral | Negative)
    
    # Step 4: Entity Extraction
    - Extract locations, organizations, persons
    - Output: List of identified entities by type
    
    # Step 5: Extractive Summarization
    - Apply TextRank algorithm
    - Extract top sentences
    - Output: Summary text
    
    # Result: Comprehensive analysis object with all outputs
```

**Output:**
A complete analysis object containing:
- Cleaned text
- Predicted category
- Sentiment label and score
- Extracted entities (locations, organizations, persons)
- Extractive summary
- All intermediate representations

**Use Case:**
Batch processing of news articles for content indexing, recommendation systems, content filtering, or editorial workflows.

---

## 💾 Technologies & Libraries

### Core Dependencies
| Category | Technologies |
|----------|--------------|
| **Data Processing** | Pandas, NumPy |
| **Text Processing** | NLTK (tokenization, stopwords, stemming) |
| **Feature Extraction** | Scikit-learn (TF-IDF vectorizer) |
| **Classification** | Scikit-learn (SVM, Random Forest) |
| **Deep Learning** | TensorFlow, Keras |
| **NLP Algorithms** | Gensim (LDA), NetworkX (graph operations) |
| **Evaluation** | ROUGE (summarization metrics) |
| **Visualization** | Matplotlib, Seaborn, WordCloud |
| **Arabic Support** | arabic-reshaper, python-bidi |

### Libraries Version Context
- Python 3.x
- TensorFlow 2.x
- Scikit-learn (modern versions)
- Gensim 4.x

---

## 📁 Repository Structure

```
761mario/arabic-news-nlp-project/
├── Final_Project_NLP.ipynb          # Complete implementation notebook
├── summarizdataset.csv               # Dataset: 8,378 Arabic news articles
└── README.md                         # This file
```

**File Descriptions:**
- **Final_Project_NLP.ipynb:** Jupyter notebook containing all code, analysis, visualizations, and results
- **summarizdataset.csv:** CSV dataset with 4 columns (text, type, processed_text, summarizer)
- **README.md:** Comprehensive project documentation

---

## 🚀 Installation & Usage

### Prerequisites
- Python 3.7+
- Jupyter Notebook or Google Colab
- 4GB+ RAM (for model training)

### Setup Instructions

#### 1. Clone the Repository
```bash
git clone https://github.com/761mario/arabic-news-nlp-project.git
cd arabic-news-nlp-project
```

#### 2. Install Dependencies
```bash
pip install pandas numpy matplotlib seaborn scikit-learn nltk tensorflow gensim networkx rouge python-bidi arabic-reshaper wordcloud
```

#### 3. Download NLTK Data
```python
import nltk
nltk.download('stopwords', quiet=True)
```

#### 4. Open the Notebook
```bash
jupyter notebook Final_Project_NLP.ipynb
```

### Running the Project

1. **Execute all cells** in sequence to:
   - Load the dataset (summarizdataset.csv)
   - Perform data validation and preprocessing
   - Generate exploratory visualizations
   - Train all classification models
   - Evaluate summarization with ROUGE metrics
   - Run sentiment analysis
   - Build LDA topic model
   - Extract entities

2. **Required Dataset File:**
   - Place `summarizdataset.csv` in the same directory as the notebook

3. **Expected Runtime:**
   - Full notebook execution: ~5-10 minutes on modern hardware
   - CNN model training: ~1-3 minutes with early stopping

### Example Usage (After Running Notebook)

Once models are trained, use the unified analysis pipeline:

```python
# Example: Analyze a new Arabic news article
article = "أخبار ثقافية عن حفل توقيع كتاب جديد..."

# Get comprehensive analysis
results = analyze_text(article)

# Access individual results
print(f"Category: {results['category']}")
print(f"Sentiment: {results['sentiment']}")
print(f"Entities: {results['entities']}")
print(f"Summary: {results['summary']}")
```

---

## 📊 Key Results Summary

### Classification Performance
- **Best Model:** SVM with TF-IDF Word N-grams
- **Accuracy:** 80.63%
- **Test Set Size:** 20% of 8,055 articles
- **Comparison:** Classical ML outperforms CNN on this dataset

### Summarization Quality
- **Algorithm:** TextRank (Unsupervised)
- **ROUGE-1:** 0.2468 (evaluated on 300 articles)
- **Method:** Extractive (selects existing sentences)

### Analysis Scope
- **5 Classification Models** compared
- **2 Feature Representations** evaluated
- **3 Sentiment States** (Positive, Neutral, Negative)
- **6 Latent Topics** discovered
- **3 Entity Types** extracted

---

## 🔎 Limitations & Considerations

### Data Limitations
1. **Category Imbalance:** Dataset shows higher frequency of cultural news
2. **Text Length Variance:** Articles range from 82 to 9,854 characters
3. **Limited Dataset Size:** 8,055 articles post-cleaning may limit deep learning potential
4. **Duplicate Content:** 298 duplicates removed during preprocessing

### Methodological Limitations

**Text Classification:**
- SVM trained on 80/20 split (no cross-validation)
- Hyperparameter tuning not extensively performed
- Feature engineering limited to TF-IDF and embeddings

**Text Summarization:**
- **Extractive only** (not abstractive) - selects existing sentences
- ROUGE scores modest (0.24 avg) due to inherent extractive limitations
- Evaluation on sample of 300 articles
- Does not generate novel summaries

**Sentiment Analysis:**
- **Lexicon-based, not learned** - depends on manual word lists
- Limited vocabulary coverage in positive/negative dictionaries
- No context understanding (e.g., sarcasm, negation)
- No training on labeled sentiment data

**Topic Modeling:**
- **6 topics fixed** (not optimized for coherence)
- LDA assumes bag-of-words (loses word order)
- No external validation of topic quality
- Limited interpretability of latent dimensions

**Entity Extraction:**
- **Rule-based, not neural** - keyword matching only
- No handling of Arabic morphological variations
- Limited to pre-defined entity dictionaries
- High precision, low recall approach

### Preprocessing Trade-offs
- **Stemming:** Reduces vocabulary but may lose nuance
- **Stopword removal:** Removes functional words that might carry meaning
- **Normalization:** Aggressive character normalization may conflate similar-looking letters

---

## 🔮 Future Improvements

These enhancements represent realistic next steps for production deployment:

### Enhanced Classification
- [ ] **Cross-validation:** Implement k-fold CV for robust accuracy estimation
- [ ] **Hyperparameter tuning:** GridSearchCV or RandomizedSearchCV
- [ ] **Transformer models:** Fine-tune AraBERT or other Arabic BERT variants
- [ ] **Class balancing:** Apply SMOTE or weighted loss for imbalanced categories
- [ ] **Ensemble methods:** Combine SVM, Random Forest, and neural models

### Advanced Summarization
- [ ] **Abstractive summarization:** Seq2seq or transformer-based models
- [ ] **Multi-document summarization:** Combine related articles
- [ ] **Length control:** Adjustable summary length
- [ ] **Hierarchical summarization:** Multi-level summaries
- [ ] **Evaluation:** Human evaluation on reference summaries

### Improved Sentiment Analysis
- [ ] **Supervised learning:** Train on labeled sentiment dataset (e.g., Arabic Twitter)
- [ ] **Context awareness:** Handle negation and intensifiers
- [ ] **Aspect-based:** Sentiment toward specific topics
- [ ] **Fine-grained:** Multi-class sentiment (very positive to very negative)
- [ ] **Transfer learning:** Leverage pretrained Arabic sentiment models

### Deeper Topic Analysis
- [ ] **Topic coherence:** Evaluate and optimize number of topics
- [ ] **Dynamic topics:** Track topic evolution over time
- [ ] **Hierarchical LDA:** Nested topic structures
- [ ] **Topic labeling:** Automatic descriptive labels for topics

### Production NER
- [ ] **Neural NER:** BiLSTM-CRF or transformer-based models
- [ ] **Pretrained models:** spaCy Arabic NER or Arabic BERT NER
- [ ] **More entity types:** Add Person, Date, Event, Location details
- [ ] **Morphological awareness:** Handle Arabic case/gender/number
- [ ] **Entity linking:** Link entities to knowledge bases (Wikipedia, etc.)

### System Enhancements
- [ ] **API deployment:** REST API for text analysis
- [ ] **Web interface:** Interactive dashboard for real-time analysis
- [ ] **Batch processing:** GPU acceleration for large document collections
- [ ] **Monitoring:** Performance tracking and model drift detection
- [ ] **Multilingual:** Extend to Modern Standard Arabic dialects

### Evaluation & Validation
- [ ] **Human evaluation:** Benchmark against expert annotators
- [ ] **A/B testing:** Compare model variants on production data
- [ ] **Error analysis:** Investigate and categorize prediction failures
- [ ] **Ablation studies:** Measure impact of each preprocessing step

---

## 👨‍💻 Project Highlights

### Strengths
✅ **Complete NLP Pipeline:** End-to-end implementation covering 5 distinct NLP tasks  
✅ **Arabic-First Approach:** Specialized preprocessing for Arabic language characteristics  
✅ **Rigorous Preprocessing:** 8-step cleaning pipeline demonstrating NLP best practices  
✅ **Model Comparison:** Systematic evaluation of 5 different classification approaches  
✅ **Strong Results:** 80.63% classification accuracy using classical ML methods  
✅ **Multiple Approaches:** Demonstrates both traditional and deep learning techniques  
✅ **Practical Pipeline:** Unified analysis function for real-world usage  
✅ **Well-Documented:** Comprehensive code comments and markdown explanations  

### Educational Value
- Demonstrates practical NLP workflow from data to deployment
- Shows appropriate model selection (classical vs. deep learning)
- Illustrates Arabic NLP challenges and solutions
- Provides reusable components for Arabic text processing

---

## 📝 License & Attribution

This project is provided as-is for educational and research purposes.

**Dataset Source:** Arabic news articles (dates indicate 2015-2017 timeframe)

**Technologies:** Built with open-source libraries (Scikit-learn, TensorFlow, Gensim, NLTK)

---

## 📧 Contact & Contributions

**Repository:** [761mario/arabic-news-nlp-project](https://github.com/761mario/arabic-news-nlp-project)

**Project Type:** Portfolio Project | Educational | Complete Implementation

---

**Last Updated:** 2024  
**Python Version:** 3.7+  
**Status:** ✅ Complete and Functional
