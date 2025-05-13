---
description: Notes from Stanford Psych 290 by Johannes Eichstaedt
---

# NLP Street Fighting Course

## NLP Research Street Knowledge&#x20;

Conferences:&#x20;

* Top annual conferences:&#x20;
  * [Association for Computational Linguistics](https://2025.aclweb.org/)
  * [Empirical Methods in Natural Language Processing](https://2025.emnlp.org/)&#x20;
  * [North American Chapter of the Association for Computational Linguistics](https://2025.naacl.org/)
* Computational Social Scientists conferences:&#x20;
  * [International Conference on Weblogs and Social Media](https://www.icwsm.org/2025/index.html)
  * [International Conference on Computational Social Science](https://www.ic2s2-2025.org/)
  * [Computational Linguistics and Clinical Psychology](https://clpsych.org/)
  * [CSCW ](https://cscw.acm.org/2025/)

## DLATK&#x20;

### Lingo & Component

* Type: an element of the vocabulary (e.g., "dog fighting dog" has two types)
* Token: an instance of that type in text (e.g., "dog fighting dog" has three tokens)
* case folding: reduce all letters to lower case (sometimes for sentiment analysis, information extraction, machine translation, case sensitive is good)&#x20;
* Lemmatization: Reduce inflections or variant forms to base form (e.g., am, are, is -> be)&#x20;
* Stemming: reduces terms to their root form (See [this blog post](https://www.quora.com/What-is-difference-between-stemming-and-lemmatization) about the difference). In reality, with enough data, this becomes a secondary concern&#x20;

### Language Features

Dictionaries:&#x20;

* Top 5 most frequent words for [GI, DICTION, and LIWC 2015](https://osf.io/qtajf)
* Using dictionaries, we should always see the most frequent words correlate, and annotate for precision.&#x20;
* For LIWC, understand LIWC correlation patterns, read the documentation.&#x20;
* Good Dictionaries:&#x20;
  * Curated: LIWC,&#x20;
  * Annotation-based dictionaries: Warriner's new ANEW (affective), LabMT (they capture how words make people feel)&#x20;
  * Sentiment / ML-based: Sentistrength, VADER, SwissChocolate, NRC (recommended)

### Component of DLATK&#x20;

* Takes in a message table
* Produce a feature table
  * Be careful how this impacts the calculation. For example, if I have 1 gram, and want to know the average percentage of use (the group norm) of a single word, we can't just take the mean (missing all the people who did not use that word)
* Uses lexicon tables to store dictionaries
* Use outcome tables for correlations

### Lexicon tables:&#x20;

* stored in the central dlatk\_lexica database
* contains term (word), category (lexicon), and weight, sparse encoded
* All feature table names created with -add\_lex\_table contain "cat\_"&#x20;

```python

# 1 gram extraction
# Result table breaks the 1 gram stats at the coorel_field level
# The group_norm is the value divided by the total number of features for this group 
# !!!The result is sparse coded, so if a feature does not exist for a user, there is no entry.
 
!dlatkInterface.py \
    --corpdb eich\  #-- database name
    --corptable msgs\ #-- message table
    --correl_field user_id \  #-- group by what
    --add_ngrams -n 1  #-- doing what (do 1 gram) '


# Adding lexicon information to 1 gram 
# It will give you a table that counts each user's different categories (e.g., positive emotion). 
! dlatkInterface.py \
    --corpdb eich \
    --corptable msgs\
    --correl_field user_id \
    --add_lex_table -l LIWC2015 # add --weighted_lexicon to enable weights 
    
# Correlate LIWC against Personality
# --rmatrix produces a correlation matrix in HTML format, 
# --csv produces correlation matrix in csv format 
# --sort append correlations matrix with another one that is sorted by effect size. 
# --group_freq_thresh limits the population (only for correlations and predictions)
!dlatkInterface.py \
    --corpdb eich \
    --corptable msgs\
    --correl_field user_id \
    --group_freq_thresh 500\
    --correlate\
    --rmatrix --csv --sort\
    ---feat_table 'feat$cat_mini_LIWC2015$msgs$user_id$1gra' \
    -- outcome_table blog_outcomes --outcomes age gender \
    -- controls control_var \
    -- output_name ~/mini_liwc_age_gender
    
# Correlation against categorical variables 
!dlatkInterface.py \
    --corpdb {corpdb} \
    --corptable {msgs_table} \
    --correl_field user_id \
    --correlate \
    --rmatrix --csv --sort \
    --feat_table {feat_miniliwc_table} \
    --outcome_table {outcomes_table} \
    --categories_to_binary occu \
    --outcomes occu \
    --output_name ~/mini_liwc_occu

# Creating 1-3 gram clouds 
!dlatkInterface.py \
-d {corpdb} -t {msgs_table} -c user_id \
--correlate --csv\
--feat_table '{feat_1to3gram_table}' \
--outcome_table {outcomes_table} --outcomes age_bins \
--category_to_binary age_bins \
--tagcloud --make_wordclouds \
--output_name {OUTPUT_FOLDER}/1to3gram_ageBuckets

# Topic clouds
!dlatkInterface.py \
-d {corpdb} -t {msgs_table} -c user_id \
--correlate --csv\
--feat_table '{feat_topic_table}' \
--outcome_table {outcomes_table} --outcomes age_bins \
--category_to_binary age_bins \
 --topic_tagcloud --make_topic_wordclouds \
 --topic_lexicon topics_fb2k_freq \
--output_name {OUTPUT_FOLDER}/1to3gram_ageBuckets



```



## Quick NLP Stuff&#x20;

Language is Weird:&#x20;

*   Language follows the Zif's law. The probability of a word with a rank $$r$$ to show up in text is&#x20;

    &#x20;$$p(W_r)= \frac{0.1}{r}$$

Minimum Data Intuition&#x20;

* Minimal: hundreds of words from hundreds of people
* Medium: hundreds of words from thousands of people, or\
  thousands of words from hundreds of people
* Good: thousands of words from thousands of people

A fundamental difficulty of language&#x20;

* Many processes map to a single outcome (e.g., use of a singular pronoun), but knowing the outcome is hard to match to a specific process.&#x20;

## Scientific Storytelling with Language Analyses (for details check Lecture 8)&#x20;

### Testing a priori hypotheses in language&#x20;

Testing specific language correlations/effects. Example paper: [Narcissism and the use of personal pronouns revisited](https://pubmed.ncbi.nlm.nih.gov/25822035/)

### Prediction&#x20;

Predicts X from text, e.g., [What Twitter Profile and Posted Images Reveal about Depression and Anxiety](https://ojs.aaai.org/index.php/ICWSM/article/view/3225)

Imputing estimates where there are none

Using the prediction error as an estimate of something else e.g., : [Authentic self-expression on social media is associated with greater subjective well-being](https://www.nature.com/articles/s41467-020-18539-w)

### Exploratory correlates&#x20;

Papers that show "the language of X"&#x20;

Show "dose response" / "XY, "IV-DV"patterns&#x20;

Construct elaboration and refinement

* Exploring the nomological network of a new construct&#x20;

Construct differentiation through language

* A conceivable avenue for item discovery in survey design&#x20;
* A complement to grounded theory and other idiographic approaches (e.g., C[omparing grounded theory and topic modeling: Extreme divergence or unlikely convergence?](https://asistdl.onlinelibrary.wiley.com/doi/10.1002/asi.23786))
* Differentiating in the language space&#x20;

### Measuring Within-person change&#x20;

e.g., : [The secret life of pronouns: flexibility in writing style and physical health](https://pubmed.ncbi.nlm.nih.gov/12564755/)

### Exploiting semantic distances&#x20;

Given a set of constructs, what are their semantic distances&#x20;

## NLP Project Intuition&#x20;

*



## Code Snippets

### Running Database in Colab&#x20;

```python
# connects the extension to the databse file 
from sqlalchemy import create_engine 
tutorial_db_engine = create_engine(f"sqlite://sqlite_data/{databse}.db?charset=utf8mb4")

# connect the extention to the database 
%sql tutorial_db_engine 

# reload the database  
%reload_ext sql 
```



## Review april 22nd&#x20;

## Working with the Dictionary&#x20;

### LIWC

Correlation pattern: review April 17th lecture notes&#x20;

### Annotation-based emotion dictionaries&#x20;

* Affective Norms of the English Language (ANEW) by Bradley & Young
  * Captures the ways in which words are perceived
  * The impact that emotional features have on the processing and memory of words&#x20;
    * zqq

ML-based dictionaries&#x20;

* generally better than annotation-based&#x20;

## ML & AI Analysis Annotate&#x20;

* table 3, comparing lv2 and lv3 result&#x20;





## Language Analysis in Science&#x20;

Testing a priori hypotheses in language&#x20;

Prediction&#x20;

Exploratory correlates&#x20;

Measuring within-person change&#x20;

Exploiting semantic distances&#x20;



## Embedding

distance to probed points vs. factor analysis&#x20;



## Open Vocabulary Analysis&#x20;

feat\_occ\_filter --set\_p\_occ 0.05: set occurance to be 5%, so only features that have shown in more than 5% of the user  (notice here the feature table only uses one dash: -f ) — see the option 1 filter down the table we have, in slides&#x20;



Extracting 2-grams&#x20;

* add 2-gram extraction method&#x20;







