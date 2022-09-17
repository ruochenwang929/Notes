# Week4: Text Pre-Processing_1

Basic Tasks in Text Preprocessing 

- Tokenization
- N-grams and Collocations
- Case Normalization
- Stopping — Remove Stop Words
- Stemming & Lemmatisation
- Sentence Segmentation

**The goal of text analysis**: provide understanding of how the text is processed without having a human read it.

The capability of computers :point_down::point_down::point_down:

:smiley: What a computer can do:

- Examine the individual characters in each word, and how those words are arranged.

:worried: What a computer cannot do:

- Know what the information is communicated by the text syntactically and semantically.

***Syntax*** v.s. ***Semantics***

:blue_heart: **Syntax**: the structure of language, e.g., grammar rules

- How individual words are composed to make well-formed sentences and paragraphs.

:green_heart: **Semantics**: the meaning of the individual words within the surrounding context

- Understand the theme of a given text fragment.

## Examples of Text Analysis Tasks

- Analyse sentence structure — e.g., syntactic parsing and dependency parsing
- Topic modelling
- Topical collocation
- Break down document into topically coherent chunks — Text segmentation
- Language Models
- Word Embedding

## Basic Tasks in Text Preprocessing

- Tokenisation
- Case Normalisation
- Stopping
- Stemming & Lemmatisation
- Sentence segmentation

### Tokenisation

**Tokenisation:** the process of breaking a stream of text into tokens.

- Text is usually represented as sequences of characters by computers. eg. *"A data wrangler is the person performing the wrangling tasks."*

- Most natural language processing (NLP) and text mining algorithms can only operate on tokens. eg. *["A", "data", "wrangler", "is", "the", "person", "performing", "the", "wrangling", "tasks"]*

Challenging issues:

- Periods in Abbreviations
  - Common acronyms with periods: U.K., U.N. etc.
  - Other abbreviations with a similar pattern: P.M., A.M., i.e., etc.
- Currency and Percentages
  - Different currencies: $10,000.00, £10,000,000.00, AUD100, EUR$10.555 and CNY555.55.
  - Percentages: 23%, 23.23% and 100.00%
- Hyphens and Apostrophes
  - Hyphens:“co-operate”, “co-education” and “pre-process”
  - Apostrophes: “don’t", “she’ll"

### N-grams and Collocations

**N-grams:** a sequence of number of words

**Collocations:** multi-word expressions that occur frequently and correspond to some conventional way of saying things.

- Noun phrases: “strong tea” and “weapons of mass destruction”
- Verb phrases: “make up”, “wind up”

Collocations are characterised by limited compositionality.

- Non-compositional phrases:
  - "He is known for his fair and square dealings and everybody trusts his work."
  - "I had a shooting pain near the heart."
- Compositional phrases:
  - “I have a stomach pain”

How to extract collocation

- NLTK Collocation package
- Simple tutorial: http://www.nltk.org/howto/collocations.html

### Case Normalisation

Capitalisation helps readers differentiate, for example, between nouns and proper nouns

- Common nouns: writer, teacher, cookies, . . .
- Proper noun: Herman Melville, Snoopy, University of Melbourne, . . .

**Case normalisation:** covert all the words into either uppercase or lowercase words.

- "data" v.s. "Data"

### Removing Stop Words

**Stop words:** words that are extremely common and carry little lexical content. They are often function words in English.

For example:

- articles (e.g., "a", "the", and "an")
- pronouns (e.g., "he", "him", and "they")
- particles (e.g., "well", "however" and "thus")

Stop words usually refer to the most common words in a language. The general strategy for determining whether a word is a stop word or not is to compute its total number of appearances in a corpus.

 :cop: Why should we remove stop words?

- Stop words usually appear to be of little value and have little impact on the final results, as the presence of stop words in a text document does not really help distinguishing it from other documents.
- Failing to remove those common words could lead to skewed analysis results. For example,
  - Email analysis: remove headers (e.g., "Subject", "To", and "From"), remove a lengthy legal disclaimer, . . .

### Stemming & Lemmatisation

Question: :question::question::question:Should we keep word forms like "educate", "educated", "educating", and "educates" separate or to collapse them?

**Stemming:** the process of identification and removal of prefixes, suffixes, and pluralisation, which leaves you with a stem.

- watches -> watch
- parties -> party
- carrying -> carry
- loving -> lov

**Lemmatization:** a more advanced form of stemming that makes use of, for example, the context surrounding the words, an existing vocabulary, morphological analysis of words and other grammatical information (e.g., part-of-speech tags) to determine the basic or dictionary form of a word, which is known as the lemma.
Use `from nltk.stem import WordNetLemmatizer`

- “meeting” + (POS = ’v’) → “meet”
- “meeting” + (POS = ’n’) → “meeting”

### Sentence Segmentation

**Sentence segmentation:** a challenging problem in natural language processing, which is about deciding where sentences begin and end.

:jack_o_lantern: Challenge: punctuation marks are often ambiguous

- Is something ending with one of the following punctuations “.”, “!”, “?” ?
- Does a period always indicate sentence boundaries?
  - Some periods occur as part of abbreviations, monetary numerals, percentages, decimal point, or an email address.

The NLTK’s Punkt Sentence Tokenizer was designed to split text into sentences “by using an unsupervised algorithm to build a model for abbreviation words, collocations, and words that start sentences.”
