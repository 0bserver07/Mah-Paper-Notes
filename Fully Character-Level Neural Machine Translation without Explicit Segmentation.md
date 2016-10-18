* character-level convolutional network with max-pooling at the encoder to reduce the length of source rep- resentation, allowing the model to be trained at a speed comparable to subword-level models while capturing local regularities.

* Attentional NMT models have three components: an encoder, a decoder and an attention mechanism.

* Share a single character- level encoder across multiple languages by training a model on a many-to-one translation task.
* Outperforms the subword-level encoder on all the language pairs.

* Nearly all previous work in machine translation has been at the level of words, due to "significantly longer when rep- resented in characters"
* This has driven NMT research to be al- most exclusively word-level (Bahdanau et al., 2015; Sutskever et al., 2015).
the complexity of training and decoding grows linearly with respect to the target vocabulary size.

* Unable to model rare, out-of- vocabulary words, making them limited in translat- ing languages with rich morphology such as Czech, Finnish and Turkish.


* Advantage of character-level models is that they are better suited for multilingual translation than their word-level counterparts which require a separate word vocabulary for each language

* We also showcase our model’s ability to handle intra-sentence code-switching, while per- forming language identification on the fly.


* (1) we can train character-to- character NMT model without any explicit segmentation; 
* (2) we can share a single character-level encoder across multiple languages to build a mul- tilingual translation system without increasing the model size.


* Others:

1. Costa-juss`a and Fonollosa (2016) replaced the word-lookup table with convolutional and highway layers on top of character embeddings,

2. Ling et al. (2015) employed a bidi- rectional LSTM to compose character embeddings into word embeddings. At the target side, another LSTM takes the hidden state of the decoder and
generates

3. Luong and Manning (2016) proposed a hybrid scheme that consults character-level information whenever the model encounters an out- of-vocabulary word. 
character-level NMT model with 4 layers of unidirectional LSTMs with 512 cells, with attention over each character.


Having a word-level decoder restricts the model to only being able to generate previously seenwords.


4. Sennrich et al. (2015) introduced a subword-level NMT model that is capable of open-vocabulary translation using subword-level segmentation based on the byte pair encoding (BPE) algorithm (bpe2bpe)


5. (Chung et al., 2016), which used a subword-level encoder from (Sennrich et al., 2015) and a fully character-level decoder (bpe2char). Their results show that character-level decoding performs better than subword-level decoding.

The Important part is the Encoder to look at...
segment here is not a word segment, it's their reduced character to segment


bpe: Byte pair encoding
https://en.wikipedia.org/wiki/Byte_pair_encoding

source: https://github.com/neubig/nmt-tips

