tidytext
========

"make text mining easier and more effective"

One observation per row.

sentiment lexicons
==================
afinn(integers), bing(positive/negative), nrc(characteristics - fear, trust,...)
They can misidentify. (eg "miss" is negative, but Austen uses it as a noun.


documents

term frequency

inverse document frequency
idf(term) = ln(n docs/ndocs containing term)

tf*idf = distinctive words of a document.
"more sophisticated than the brute force approach of stripping stop words (the, and, a)"

the next level
=============
n-grams (bigram, trigram, etc)
bigrams starting with a negation word - how does this affect sentiment analysis.
"Not happy".

he / she:
In Austen, women remember, read, feel. Men stop, take, reply

The Pudding: screen directives for scripts. "She giggles, he gallops"

when is it appropriate
======================
Not all the time!
convert back and forth from untidy and tidy.
"Tidy and cast"
Document term matrix -> ML

tidytextmining.com



