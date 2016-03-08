---
layout: page
title: NLP_Toolkit
math_support: mathjax
---


### Stanford CoreNLP

#### 1. direct call
[CoreNLP](http://stanfordnlp.github.io/CoreNLP/index.html)

[a jar package wrapper in python](https://github.com/brendano/stanford_corenlp_pywrapper)

#### 2. Server

[CoreNLP Server](http://stanfordnlp.github.io/CoreNLP/corenlp-server.html)

**Run Server**

~~~bash
# Set up your classpath. For example, to add all jars in the current directory tree:
export CLASSPATH="`find . -name '*.jar'`"

# Run the server
java -mx4g -cp "*" edu.stanford.nlp.pipeline.StanfordCoreNLPServer [port=9000]

# send post data & get parameter to the server
wget --post-data 'the quick brown fox jumped over the lazy dog' 'localhost:9000/?properties={"tokenize.whitespace": "true", "annotators": "tokenize,ssplit,pos", "outputFormat": "json"}' -O -
~~~

**Kill Server**

C-c

or
~~~bash
wget "localhost:9000/shutdown?key=`cat /tmp/corenlp.shutdown`" -O -
~~~

**python talking with server**
install lib:
~~~bash
pip install pycorenlp
~~~

how to use:
~~~python
from pycorenlp import StanfordCoreNLP

if __name__ == '__main__':
    nlp = StanfordCoreNLP('http://localhost:9000')
    text = (
        'Pusheen and Smitha walked along the beach. Pusheen wanted to surf,'
        'but fell off the surfboard.')
    output = nlp.annotate(text, properties={
        'annotators': 'tokenize,ssplit,pos,depparse,parse',
        'outputFormat': 'json'
    })
    print(output['sentences'][0]['parse'])
    output = nlp.tokensregex(text, pattern='/Pusheen|Smitha/', filter=False)
    print(output)
    output = nlp.semgrex(text, pattern='{tag: VBD}', filter=False)
    print(output)
~~~

#### 3. demo

http://corenlp.run/

### NLTK (python)

http://www.nltk.org/

source: https://github.com/nltk/nltk

### spaCy.io (python)

https://spacy.io/







