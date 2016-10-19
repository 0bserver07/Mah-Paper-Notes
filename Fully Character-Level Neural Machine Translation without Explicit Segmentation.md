##### Brief Summary 2x Speed:

* This is Neural Machine Translation model on Character Level.
* Nearly all of the previous approaches have been at the level of words and sub-words translation.
* The Encoder is the sauce, model details: 
  * Source-Target character level 
  * CNN+RNN encoder 
  * Bi-scale decoder 
  * {Fi,De,Cs,Ru}→En
* Introduces Many to One language translation through the same encoder.
* It outperforms a baseline with a subword-level encoder on DE-EN and CS-EN, and achieves a comparable result on FI- ENandRU-EN.

----
##### Brief Summary 1x Speed:

To understand the significance of this paper, we have to reflect a little bit on the previous approaches that has been applied towards machine translation in the line of Neural Machine Translation.

* Mapping direct character from source to target language is a challenge. That's why most NMT research is almost exclusively at word-level.
* Most Attentional Neural Machine Translation model follow some variational type of this architecture:
 `an encoder, a decoder and an attention mechanism`

The machine translation scene can be broken down to 3 tiers:

* Word level
* Sub-word level
* Hybrid encoder/or decoder to achieve character level one of the sides.


* Overall sub-word and word level models have their own shortcomings:
 * Unable to generate (output) words that the model haven't seen before. (sub-word segmentation, sort of helped with this issue to some extend)
 * The word-level model, needs a separate word vocabulary for each language. Doing any cross lingual task with the model is out of the window.
 * The arbitrary relationship between the orthography of a word and its meaning is a well- known problem in linguistics (de Saussure, 1916).

The sub-word level models, such as (Sennrich et al., 2015), uses byte pair encoding on the Encoder of the model, to achieve some level of representation while reducing the input sequence size. The model is referred to as `bpe2char`

Some of the inspirations for the model in the paper:

1. Costa-jussa and Fonollosa (2016) replaced the word-lookup table with convolutional and highway layers on top of character embeddings,

2. Ling et al. (2015) employed a bidirectional LSTM to compose character embeddings into word embeddings. At the target side, another LSTM takes the hidden state of the decoder and

3. Luong and Manning (2016) introduced a hybrid scheme that uses character-level information whenever the model sees a new word that is not from the model's previous seen vocabularies. (3 months to train)

4. Sennrich et al. (2015) introduced a subword-level NMT model that is capable of open-vocabulary translation using subword-level segmentation based on the byte pair encoding (BPE) algorithm (bpe2bpe)

5. (Chung et al., 2016), which used a subword-level encoder from (Sennrich et al., 2015) and a fully character-level decoder (bpe2char). Their results show that character-level decoding performs better than subword-level decoding.

In the paper we have:

Encoder [ Word Embedings + Conv 1d + Max-Pooling Stride + Segment Embedding + Bi-GRU ]

![figure 1](http://i.imgur.com/C2TJGXw.png)
Figure 1

Decoder [Similar to]

Image

Attention:

Here is one example of how the attention mechanism can help:
![attention](https://3.bp.blogspot.com/-3Pbj_dvt0Vo/V-qe-Nl6P5I/AAAAAAAABQc/z0_6WtVWtvARtMk0i9_AtLeyyGyV6AI4wCLcB/s640/nmt-model-fast.gif)

It achives these things which are impressive:

First foremost, this architecure might not be new to the scene, however the previous approaches have not been on Char 2 Char and the last few approaches were pretty much

Presenetation that hints to the upcoming work:

ManyToMany....

My personal suggestions: Swapping out Highway Network to something else could work nice.

















* The paper introduces a character-level convolutional network with max-pooling at the encoder to reduce the length of source rep- resentation, allowing the model to be trained at a speed comparable to subword-level models while capturing local regularities.

Attentional NMT models have three components: an encoder, a decoder and an attention mechanism.

Share a single character- level encoder across multiple languages by training a model on a many-to-one translation task.
Outperforms the subword-level encoder on all the language pairs.

Nearly all previous work in machine translation has been at the level of words, due to "significantly longer when rep- resented in characters"
This has driven NMT research to be al- most exclusively word-level (Bahdanau et al., 2015; Sutskever et al., 2015).
the complexity of training and decoding grows linearly with respect to the target vocabulary size.

Unable to model rare, out-of- vocabulary words, making them limited in translat- ing languages with rich morphology such as Czech, Finnish and Turkish.


Advantage of character-level models is that they are better suited for multilingual translation than their word-level counterparts which require a separate word vocabulary for each language

We also showcase our model’s ability to handle intra-sentence code-switching, while per- forming language identification on the fly.


(1) we can train character-to- character NMT model without any explicit segmentation;
(2) we can share a single character-level encoder across multiple languages to build a mul- tilingual translation system without increasing the model size.


Others:

Potential Benefits:
1. Positive language transfer across many language pairs/directions 
	* Solution to low/zero-resource machine translation
2. Number of parameters grows linearly  with respect to the number of languages as opposed to the quadratic explosion when training many single-pair models.
3. Multi-source translation without requiring any multi-way parallel text

Above notes from: [Recent Advances and the Future of Neural Machine Translation](https://ufal.mff.cuni.cz/mtm16/files/12-recent-advances-and-future-of-neural-mt-orhat-firat.pdf)



The Important part is the Encoder to look at...


##### Other notes:
* bpe: [Byte pair encoding](https://en.wikipedia.org/wiki/Byte_pair_encoding)

-----

* Ferdinand de Saussure. 1916. Course in General Lin- guistics.
* Rico Sennrich, Barry Haddow, and Alexandra Birch. 2015. Neural machine translation of rare words with subword units. In Proceedings of the 54th Annual Meeting of the Association for Computational Linguis- tics

* Marta R. Costa-Jussa and Jos` e A. R. Fonollosa. 2016.
Character-based neural machine translation. In Pro- ceedings of the 54th Annual Meeting of the Association for Computational Linguistics, page 357.

* Wang Ling, Isabel Trancoso, Chris Dyer, and Alan W Black. 2015. Character-based neural machine transla- tion. arXiv preprint arXiv:1511.04586.
* Minh-Thang Luong and Christopher D. Manning. 2016. Achieving open vocabulary neural machine translation with hybrid word-character models. In Proceedings of the 54th Annual Meeting of the Association for Com- putational Linguistics.
