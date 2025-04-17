---
description: Notes from Stanford Psych 290
---

# Johannes Eichstaedt's NLP Street Fighting Course

## DLATK

### Component of DLATK&#x20;

* Takes in message table
* Produce feature table
  * Be careful how this impacts the calculation. For example, if I have 1 gram, and want to know the average percentage of use (the group norm) of a single word, we can't just take the mean (missing all the people who did not use that word)
* Uses lexicon tables to store dictionaries
* Use outcome tables for correlations

### Lexicon tables:&#x20;

* stored in the central dlatk\_lexica database
* contains term (word), category (lexicon), and weight, sparse encoded
* All feature table names created with -add\_lex\_table contain "cat\_"&#x20;

```python

# 1 gram extraction 
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
!dlatkInterface.py \
    --corpdb eich \
    --corptable msgs\
    --correl_field user_id \
    --correlate\
    --rmatrix --csv --sort\
    --outcome 
```



## NLP Findings&#x20;







## Validation&#x20;

* see class page 20 to get all the validation approach / measures.&#x20;



## Code Snippets

Review April 17th lecture&#x20;

```python
from sqlalchemy import create_engine 

tutorial_db_engine = create_engine(f"sqlite://sqlite_data/{databse}.db?charset=utf8mb4")


```



## Dictionary&#x20;

