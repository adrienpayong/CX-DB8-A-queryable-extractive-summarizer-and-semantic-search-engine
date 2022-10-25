# CX DB8: A queryable extractive summarizer and semantic search engine
## Introduction

Extractive summarization is the task of automatically producing a summary of a text by deleting uninformative tokens. This is in contrast to abstractive summarization, which allows for deletion and replacement or insertion of tokens. One way of characterizing their difference is by
using the highlighter vs pen analogy. Extractive
Summarization is the process of highlighting a document to only include the most important on salient parts of the document. Abstractive Summarization is when one writes a completely new abstract based on the document. This abstract may include tokens which are not found in the document. CX_DB8 is an extractive summarization system.

## Why CX_DB8’s algorithm?

Prior work in extractive summarization systems includes many recent advances made in NLP (Jadhav and Rajan, 2018; Zhao et al., 2018). Unfortunately, these systems suffer from some key limitations. Most of them do not utilize the latest in pre-trained word and character embeddings. Due to a focus on maximizing grammatical correctness, they only populate summaries with candidate text at the sentence level. They also have a focus on creating “faithful” summaries – e.g. they usually select sentences with the goal of taking the most similar sentences to the document as a whole. We find that documents can sometimes be “summarized to say” whatever a reader wants them to say. CX_DB8’s algorithm is inspired by how competitive debaters abstractively summarize documents, which presents their accounting of what the evidence says and how it supports their argument.

## Backend Architecture
- CX_DB8 underlying Architecture are unsupervised, pre-trained text vectorization models. Text vectorization is the process of converting text into an N dimensional vector.
- Work in the field of text embeddings is rapidly progressing. For this reason, CX_DB8 is powered by the text embedding framework Flair (Akbik and Blythe, 2018). Flair is chosen because it has a simple interface that allows a user to combine word or document embeddings together arbitrarily. The authors of Flair make it a point to quickly incorporate new text embedding models – meaning that CX_DB8’s summarization capabilities will improve as the state-of-the-art models which power it improve.
- Many summarization techniques utilize supervised techniques, but we consciously avoided them, as we desired the stronger generalization performance available from unsupervised models.

## Algorithm Overview
### Word Windows
CX_DB8 can generate a user specified length summary from a document by computing the cosine similarity between a vectorized representation of a user-inputted query and each words corresponding word-window in the document. This produces a scaler for each word, which corresponds to the similarity of this word (and context) to the query. Figure 2 displays the difference in summarization as the word window is lengthened. We observe that as the word window
increases, the summaries tend to include longerruns of words, roughly proportional to the size of the word window.
![source](https://github.com/adrienpayong/object-detection/blob/main/algo.png)
### Bias Query
When executing CX_DB8, it first asks a user to input a bias query. This can be a single word (for semantic search), a sentence (or a “tagline” as debaters call it), or even a whole different document. Users who wish to generate an unbiased summary can enter “-1” and CX_DB8 will set the query to be the document to be summarized. Each word is vectorized and the bias vector is computed by mean pooling each word vector in the query.
## Summarization
During the main execution loop, the user enters the documents that they wish to summarize. CX_DB8 takes input in through the system’s standard input stream and a user indicates that they are done entering their document by pressing ctrl- d. Once the user has entered their document, CX_DB8 works as follows:
- For each word in the source document, the vectorized representation of the word window is computed. 
- CX_DB8 then computes the cosine similarity between the query vector and each word window vector. 
- CX_DB8 prompts the user to specify the percentage of the document that they want underlined and highlighted ahead of time. The words with the highest similarity are selected to be included in the summary. CX_DB8 also allows for a user to re-underline or re-highlight a document if they want a different sized summary or a summary with different settings.

## Pooling
- CX_DB8 inherits all hyperparamaters available in Flair’s DocumentPoolEmbeddings class. Any set of word vectors available through Flair, as well as custom user-trained models, can be selected to power CX_DB8. For instance, a user could leverage fine tuned word2vec, GloVe, fastText, and BERT models to summarize a document. Flair concatenates the embeddings together seamlessly.
- Computing the vector representation of a multi- word string is usually done by averaging each word or character vector, though Flair makes it possible to take the maximum or minimum of each vector.
## Semantic Search
- Some users of CX_DB8 noted the similarity that this model has to a semantic powered “find” or “search” tool. Typing a query like “Policies” on a politician’s web page may easily guide a user to their list of policies, even if the webpage chooses different but semantically similar words to describe their policy page. 
- One especially interesting use case of CX_DB8 is in information retrieval. A user mentioned that they were trying to use CX_DB8 in tandem with embeddings trained on pubmed abstracts to process medical documents about cancer and search for experimental or novel treatments for their family. 
## Conclusion
We presented CX_DB8, a queryable, word-level extractive summarization system designed for a specific domain. This tool leverages state-of-the- art pre-trained language models to generate its summaries. 

## References
[CX DB8:A queryable extractive summarizer and semantic search engine](https://arxiv.org/abs/2012.03942)

https://github.com/Hellisotherpeople/CX_DB8
