# Project Jesse: Discriminating Terms for Identifying Employment Skills in Job Listings: A Lexical and Statistical Approach

### **Abstract**  
The ability to accurately identify and extract relevant skills from job listings is essential for job seekers navigating the complex employment marketplace. In this study, we introduce a novel approach for identifying in-demand skills and job-specific terminology using **Discriminating Terms**—words and phrases that statistically differentiate job listings from general internet text. Our method begins with a dataset of texts categorized as "Job Listings" or "General Web Content." We employ Poisson distribution-based statistical analysis to identify terms that occur with significantly higher frequency in job listings. These Discriminating Terms are further enriched through lexical expansion, incorporating synonyms, abbreviations, and domain-specific terminology. The resulting feature set is used to train a machine learning classifier, evaluated across multiple models, including Naïve Bayes, Logistic Regression, Support Vector Machines, and Transformer-based architectures. This approach enables the automatic extraction of skills terminology from job listings, helping job seekers better understand employer requirements and align their qualifications accordingly.

### **Introduction**  
The classification and analysis of job listings is a critical task in career guidance and workforce development, aiding job seekers in filtering relevant opportunities amidst an overwhelming number of postings. One particularly important challenge is identifying **specific skills and qualifications demanded by employers**, as these listings provide essential information on requirements, responsibilities, and preferred attributes. However, manually sorting through job listings to extract this information is both time-consuming and error-prone.

Traditional keyword-based searches often fail due to the **polysemy and variability of terminology** in job descriptions. For example, terms like "communication" or "leadership" appear across multiple contexts, with varying levels of importance. To overcome these limitations, we propose a method based on **Discriminating Terms**—a set of words and phrases that significantly distinguish job listings from general web content.

This study leverages a **curated dataset of job listings and general web content**, classified into **Job Listings** and **Non-Job Content** categories. We employ a **Poisson distribution-based statistical approach** to identify terms that appear disproportionately in job listings, filtering out common but non-discriminatory terms. To enhance classification accuracy, we further **enrich the Discriminating Terms** by incorporating synonyms, industry-specific terminology, abbreviations, and phrase-level variations.

These Discriminating Terms are then used to develop a **machine learning classifier** capable of automatically categorizing text based on its content and extracting relevant skills terminology. We explore multiple classification models, ranging from **probabilistic methods (Naïve Bayes, Logistic Regression) to deep learning approaches (BERT, JobBERT)**. Our approach demonstrates high precision and recall, effectively distinguishing job-related skills and requirements from unrelated content.

The contributions of this study include:  
1. **A novel method for identifying Discriminating Terms** for employment skills and requirements.  
2. **A machine learning-based classifier** for automated job listing categorization and skills extraction.  
3. **A linguistically enriched dataset** that improves accuracy in employment text mining.  

The remainder of this paper details our methodology for extracting Discriminating Terms, constructing the classifier, and evaluating its performance. Our findings suggest that this approach can **enhance automated skills identification**, aiding job seekers in efficiently identifying relevant qualifications and requirements in job listings.

### Methods  

#### Identifying Discriminating Terms for Job Listing Classification  

To classify text as either related to "Job Listings" or not, we developed a methodology based on Discriminating Terms. These terms serve as linguistic markers that differentiate job listings from general web content. Our approach involves extracting key terminology from a curated dataset of labeled texts and using statistical techniques to identify terms that significantly distinguish job-related content from others.

### 1. **Data Collection and Curation**  
To establish a reliable ground truth dataset, we compiled a collection of texts from publicly available sources, including job boards, company career pages, and general web content. These texts were categorized into two groups:

- **Job Listings**: Text explicitly providing details about employment opportunities, including required qualifications, responsibilities, and skills.
- **Non-Job Content**: Randomly selected web content that does not focus on job opportunities, covering unrelated topics from news, blogs, and general information websites.

This curated dataset serves as the foundation for training and validating our classifier.

### 2. **Extracting Candidate Discriminating Terms**  
We define "Discriminating Terms" as words or phrases that appear significantly more frequently in job listings than in general web content. To identify these terms, we:

1. **Tokenize and Normalize the Text**  
   - Convert all text to lowercase.  
   - Remove stopwords (e.g., "the," "and," "of") using a standard stopword list.  
   - Apply lemmatization to normalize word forms (e.g., "requires" → "require").  

2. **Calculate Word Frequencies in Each Category**  
To determine the significance of words in job listings, we compute their frequency in two distinct corpora:

- Count occurrences of each word in the **Job Listings corpus** (\\( C_{JL} \\)).  
- Count occurrences of each word in the **Non-Job Content corpus** (\\( C_{NJ} \\)).  
- Normalize word frequencies by the total word count in each corpus:

$$
f_{JL}(w) = \frac{C_{JL}(w)}{\sum C_{JL}}
$$

$$
f_{NJ}(w) = \frac{C_{NJ}(w)}{\sum C_{NJ}}
$$

Where:  
- \\( f_{JL}(w) \\) is the normalized frequency of word \\( w \\) in the Job Listings corpus.  
- \\( f_{NJ}(w) \\) is the normalized frequency of word \\( w \\) in the Non-Job Content corpus.  
- \\( C_{JL}(w) \\) and \\( C_{NJ}(w) \\) are the raw counts of word \\( w \\) in their respective corpora.  
- \\( \sum C_{JL} \\) and \\( \sum C_{NJ} \\) are the total word counts in each corpus.  

3. **Compute the Poisson Distribution for Term Significance**  
To determine whether a term is significantly overrepresented in job listings, we use the Poisson distribution. Given that term frequencies follow a power-law distribution in natural language, the Poisson distribution helps model rare but meaningful occurrences of specific terms.

## Probability Calculation for Discriminating Terms

The probability of observing \\( n \\) occurrences of a word in job listings, given its expected frequency across all texts, is:

$$
p(n | N, f) = e^{-Nf} \cdot \frac{(Nf)^n}{n!}
$$

Where:  
- \\( n \\) is the observed frequency of the term in job listings.  
- \\( N \\) is the total word count in job listings.  
- \\( f \\) is the expected frequency of the term across all texts, calculated as:  

$$
f = \frac{C_{NJ}}{\sum C_{NJ}}
$$

To avoid floating-point errors, we reformulate the equation in logarithmic terms:

$$
\ln p(n | N, f) = -Nf + n \ln(Nf) - \ln(n!)
$$

For larger values of \\( n \\), Stirling's approximation is used for factorial computation:

$$
\ln(n!) \approx n \ln n - n
$$

Using this method, we rank words based on how significantly they deviate from their expected frequency. Terms with the highest deviation are considered Discriminating Terms.

### 3. **Semantic Enrichment of Discriminating Terms**  
Once candidate Discriminating Terms are identified, we enhance them using linguistic resources:

- **Synonyms & Lexical Variants:** Extracted from WordNet, O*NET (Occupational Information Network), and industry-specific thesauri.
- **Acronyms & Abbreviations:** Identified using regex-based heuristic rules and industry-specific acronym databases.
- **Skills Hierarchies:** Hierarchical relationships (e.g., "Python programming" as a subtype of "Software Development").

This step ensures robustness in capturing domain-specific terminology across different industries and job roles.

### 4. **Building the Job Skills Classifier**  
We integrate Discriminating Terms into a classification model to automatically determine whether text contains job-related skills and requirements.

#### 4.1 Feature Engineering  
We represent each text as a feature vector based on:  
- **Presence of Discriminating Terms** (binary or TF-IDF weighted).  
- **Term Proximity Analysis** (e.g., "experience" near "years" is stronger evidence than alone).  
- **Section Importance** (terms in "Requirements" sections may have different significance).  

#### 4.2 Model Training  
We test multiple classifiers, including:  
- **Naïve Bayes** (leveraging term probabilities).  
- **Logistic Regression** (linear decision boundary based on term presence).  
- **Support Vector Machines (SVMs)** (optimal hyperplane for classification).  
- **Transformer-based Models (BERT, JobBERT)** (for deep semantic analysis).  

The final model is selected based on cross-validation accuracy and F1-score.

### 5. **Evaluation and Validation**  
We evaluate model performance using:  
- **Precision, Recall, and F1-score** on a held-out test set.  
- **Manual Review of Misclassified Texts** to refine the Discriminating Terms list.  
- **Comparison with Existing Text Classifiers** for benchmarking.

### 6. **Application: Degree Selection Guidance**
Named after Jesse B. Davis, a pioneer in vocational guidance who established the first systematic school counseling program in 1907, Project Jesse extends our skills identification approach to help students match their interests with appropriate degree programs. By analyzing:

- Job listings related to various degrees
- Required and preferred skills for different career paths
- Correlation between academic programs and employment opportunities

The system provides personalized degree recommendations based on student interests and career aspirations, continuing Davis's legacy of connecting education with career development.

### 7. **Conclusion**  
By identifying and leveraging Discriminating Terms, we create a classifier that accurately distinguishes job-related skills and requirements. This approach combines statistical rigor, semantic enrichment, and machine learning to improve classification accuracy and support automated skills extraction from job listings. Project Jesse not only aids in career guidance but also helps educational institutions align their programs with employment market demands.

