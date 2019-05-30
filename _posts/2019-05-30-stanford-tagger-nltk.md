---
layout: post
title: Introduction to Stanford CoreNLP in NLTK
---

## 1. What is Stanford CoreNLP?
Stanford CoreNLP is a collection of pretrained state-of-the-art models in NLP.

The Penn treebank POS tagset includes 36 POS tags and 12 others for punctuations and special symbols.

## 2. Stanford CoreNLP in NLTK
From version 3.3, the Stanford NER and POS taggers from ```nltk.tag``` are deprecated, instead they provide new API ```nltk.parse.corenlp.CoreNLPParser``` for this task. In this tutorial, I will show steps how to use this new API.  

* Download the CoreNLP packages from here:
[Stanford CoreNLP](https://stanfordnlp.github.io/CoreNLP/index.html#download)  
* Unzip your file and cd to that directory.  
* Start the CoreNLP server:

```
java -mx4g -cp "*" edu.stanford.nlp.pipeline.StanfordCoreNLPServer -preload tokenize,ssplit,pos,lemma,ner,parse,depparse -status_port 9000 -port 9000 -timeout 15000 & 
```  
* In Python:  

```python
>>> from nltk.parse import CoreNLPParser
>>> from nltk.parse.corenlp import CoreNLPDependencyParser
```

**Tokenizer**  
```python
>>> parser = CoreNLPParser(url='http://localhost:9000')
>>> list(parser.tokenize('If you are making a cup of coffee, could you make one for me?'))
```
Result
```
['If',
 'you',
 'are',
 'making',
 'a',
 'cup',
 'of',
 'coffee',
 ',',
 'could',
 'you',
 'make',
 'one',
 'for',
 'me',
 '?']
```
**POS Tagger**
```python
>>> pos_tagger = CoreNLPParser(url='http://localhost:9000', tagtype='pos')
>>> list(pos_tagger.tag('If you are making a cup of coffee, could you make one for me?'.split()))
```
Result:
```
[('If', 'IN'),
 ('you', 'PRP'),
 ('are', 'VBP'),
 ('making', 'VBG'),
 ('a', 'DT'),
 ('cup', 'NN'),
 ('of', 'IN'),
 ('coffee', 'NN'),
 (',', ','),
 ('could', 'MD'),
 ('you', 'PRP'),
 ('make', 'VB'),
 ('one', 'CD'),
 ('for', 'IN'),
 ('me', 'PRP'),
 ('?', '.')]

```
**NER Tagger**
```python
>>> ner_tagger = CoreNLPParser(url='http://localhost:9000', tagtype='ner')
>>> list(ner_tagger.tag('John Wick is an American neo-noir action thriller franchise that was created by Derek Kolstad'.split()))
```
Result:
```
[('John', 'PERSON'),
 ('Wick', 'PERSON'),
 ('is', 'O'),
 ('an', 'O'),
 ('American', 'NATIONALITY'),
 ('neo-noir', 'O'),
 ('action', 'O'),
 ('thriller', 'O'),
 ('franchise', 'O'),
 ('that', 'O'),
 ('was', 'O'),
 ('created', 'O'),
 ('by', 'O'),
 ('Derek', 'PERSON'),
 ('Kolstad', 'PERSON')]
```
**Syntactic Parser** 
```python
>>> parser = CoreNLPParser(url='http://localhost:9000')
>>> list(parser.parse("It's good weather today".split()))
```
Result:
```
[Tree('ROOT', [Tree('S', [Tree('NP', [Tree('PRP', ['It'])]), Tree('VP', [Tree('VBZ', ["'s"]), Tree('ADJP', [Tree('JJ', ['good'])]), Tree('NP-TMP', [Tree('NN', ['weather']), Tree('NN', ['today'])])])])])]
```
**Dependency Parser**
```python
>>> dep_parser = CoreNLPDependencyParser(url='http://localhost:9000')
>>> parses = dep_parser.parse('If you are making a cup of coffee, could you make one for me?'.split())
>>> [[(governor, dep, dependent) for governor, dep, dependent in parse.triples()] for parse in parses]
```
Result:
```
[[(('make', 'VB'), 'advcl', ('making', 'VBG')),
  (('making', 'VBG'), 'mark', ('If', 'IN')),
  (('making', 'VBG'), 'nsubj', ('you', 'PRP')),
  (('making', 'VBG'), 'aux', ('are', 'VBP')),
  (('making', 'VBG'), 'dobj', ('cup', 'NN')),
  (('cup', 'NN'), 'det', ('a', 'DT')),
  (('cup', 'NN'), 'nmod', ('coffee', 'NN')),
  (('coffee', 'NN'), 'case', ('of', 'IN')),
  (('make', 'VB'), 'punct', (',', ',')),
  (('make', 'VB'), 'aux', ('could', 'MD')),
  (('make', 'VB'), 'nsubj', ('you', 'PRP')),
  (('make', 'VB'), 'dobj', ('one', 'CD')),
  (('make', 'VB'), 'nmod', ('me', 'PRP')),
  (('me', 'PRP'), 'case', ('for', 'IN')),
  (('make', 'VB'), 'punct', ('?', '.'))]]
```

## References
* [https://github.com/nltk/nltk/wiki/Stanford-CoreNLP-API-in-NLTK]()

