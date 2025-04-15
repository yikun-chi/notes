---
description: Notes from Stanford Psych 290
---

# Johannes Eichstaedt's NLP Street Fighting Course

## DLATK Introo

Components of DLATK

* Takes in message table
* Produce feature table
* Uses lexicon tables to store dictionaries
* Use outcome tables for correlations

&#x20;

DLATK Commands Example

Extracting 1-grams:

!dlatkInterface.py \\

\--corpdb eich\  -- database name

\--corptable msgs\ -- message table

\--correl\_field user\_id \  -- group by what

\--add\_ngrams -n 1  -- doing what (do 1 gram) '

&#x20;

Feature table is in CSR sparse format

* Be careful how this impacts the calculation. For example, if I have 1 gram, and want to know average percentage of use of a single word, we can't just take the mean (missing all the people who did not use that word)'





## Validation&#x20;

* see class page 20 to get all the validation approach / measures.&#x20;





## Dictionary&#x20;

