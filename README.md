# ngram2vec
The toolkit implements the ngram2vec model proposed in EMNLP2017<br>
**Ngram2vec: Learning Improved Word Representation from Ngram Co-occurrence Statistics**<br>

**ngram2vec** toolkit is a natural extension to **word2vec**, where ngrams are introduced into recent word representation methods inspired by traditional language modeling problem. The toolkit can generate state-of-the-art word embeddings and high-quality ngram embeddings. For example, PPMI achieves 85+ accuracy on Google analogy questions (semantic group). 

This toolkit may be also a good startpoint for those who want to learn about word representation models. It includes SGNS, GloVe, PPMI, and SVD and organize them in a pipeline. Arbitrary context features are supported. In terms of efficiency, it enables users to build vocabulary and co-occurrence matrix at a certain memory size. Also, we do optimization on many stages to speed up the process and reduce disk space required.


## Example use cases

Firstly, run the following codes to make some files executable.<br>
`chmod +x *.sh`<br>
`chmod +x scripts/clean_corpus.sh`<br>
`chmod +x word2vecf/word2vecf`<br>
`chmod +x glovef/build/glove`<br>

Also, a corpus should be prepared. We recommend to fetch it at<br> 
http://nlp.stanford.edu/data/WestburyLab.wikicorp.201004.txt.bz2 , a wiki corpus without XML tags. `scripts/clean_corpus.sh` is used for cleaning corpus in this work.<br> `clean_corpus.sh ${corpus} > ${corpus}.clean`<br>

run `./uni_uni.sh` to see baselines<br>
run `./uni_bi.sh` and PPMI of uni_bi type will bring you state-of-the-art results on Google semantic questions (85+) <br>
run `./bi_bi.sh` to see significant improvments achieved when ngrams are introduced into SGNS<br> 

Note that in this toolkit, we remove low-frequency words with a threshold of 100 to speed up training and evaluation process. One can set thr=10 to reproduce the results reported in the paper. 

## Some comments

**corpus2vocab** builds ngram vocabulary from corpus<br>
**corpus2pairs** extracts ngram pairs from corpus (multi-threading implementation), used by SGNS model<br>
**pairs2vocab** generates center word vocabulary and context vocabulary, which are used by all models. (note that the two vocabularies are different. In `uni_bi` case, center word vocabulary only contains words while context vocabulary contains both words and bigrams)<br>
**pairs2counts** builds co-occurrence matrix from pairs. We accelerate this stage by using mixed and stripes strategies. By now we only upload a coarse version and we will continue improving this code<br>
**counts2ppmi** learns PPMI matrix from counts<br>
**counts2shuf** shuffles the counts<br>
**counts2bin** transfer counts into binary format, which is supported by glove<br>
**glovef** To support arbitrary context features, we use a modification of word2vec, word2vecf (implemented by Omer Levy), to train SGNS model. In spirit of word2vecf, we implement glovef upon glove.

## Acknowledgments

This toolkit is inspired by Omer Levy's work http://bitbucket.org/omerlevy/hyperwords<br>
We reuse part of his code in this toolkit.
We build glovef upon glove https://github.com/stanfordnlp/GloVe
