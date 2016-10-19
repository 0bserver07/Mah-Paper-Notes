####  Fully Character-Level Neural Machine Translation without Explicit Segmentation (Jason Lee, Kyunghyun Cho, Thomas Hofmann) [arXiv:1610.03017](https://arxiv.org/abs/1610.03017v1)

##### Brief Summary 2x Speed:

* This is a Neural Machine Translation model on Character Level.
* Nearly all the previous approaches have been at the level of words and sub-words translation.
* The Encoder is the sauce, model details, (The Important part is the Encoder to look at if you just want to bounce):
  * Source-Target character level 
  * CNN+RNN encoder 
  * Bi-scale decoder 
  * {Fi,De,Cs,Ru}→En
* Introduces Many to One language translation through the use of same encoder.
* It outperforms a baseline with a subword-level encoder on DE-EN and CS-EN, and achieves a comparable result on FI- EN and RU-EN.

----
##### Brief Summary 1x Speed:

To understand the significance of this paper, we have to reflect a little on the previous approaches that has been applied towards machine translation in the line of Neural Machine Translation.

* Mapping direct character from source to target language is a challenge. That's why most of the NMT researches have been almost all at word-level.
* Most Attentional Neural Machine Translation models follow some variation of this architecture:

 `an encoder, a decoder and an attention mechanism`

The machine translation scene can be broken down to 3 tiers (no actual boundaries, this is just to build a perspective):

* Word level
* Sub-word level
* Hybrid encoder/or decoder to achieve character level on one of the sides.


##### Overall sub-word and word level models have their own shortcomings:
 * Unable to generate (output) words that the model haven't seen before. (sub-word segmentation, sort of helped with this issue to some extent)
 * The word-level model, needs a separate word vocabulary for each language. Doing any cross lingual task with the model is out of the window.
 * The arbitrary relationship between the orthography of a word and its meaning is a well- known problem in linguistics (de Saussure, 1916).

The sub-word level models, such as (Sennrich et al., 2015), uses byte pair encoding on the Encoder of the model, to achieve some level of representation while reducing the input sequence size. The model is referred to as `bpe2char`

Some inspirations for the model in the paper:

1. Costa-jussa and Fonollosa (2016) replaced the word-lookup table with convolutional and highway layers on top of character embeddings,

2. Ling et al. (2015) employed a bidirectional LSTM to compose character embeddings into word embeddings. At the target side, another LSTM takes the hidden state of the decoder and

3. Luong and Manning (2016) introduced a hybrid scheme that uses character-level information whenever the model sees a new word that is not from the model's previous seen vocabularies. (3 months to train)

4. Sennrich et al. (2015) introduced a subword-level NMT model that is capable of open-vocabulary translation using subword-level segmentation based on the byte pair encoding (BPE) algorithm (bpe2bpe)

5. (Chung et al., 2016), which used a subword-level encoder from (Sennrich et al., 2015) and a fully character-level decoder (bpe2char). Their results show that character-level decoding performs better than subword-level decoding.


**Encoder:** `[ Word Embeddings >> Conv 1d >> Max-Pooling w/ Stride >> Segment Embedding >> Bi-dir GRU >> ]`

![figure 1](http://i.imgur.com/C2TJGXw.png)
	 
	 - *Figure 1

**Attention:** [similar to (Bahdanau et al., 2015)] Here is one example of how the attention mechanism can help:

![attention](https://3.bp.blogspot.com/-3Pbj_dvt0Vo/V-qe-Nl6P5I/AAAAAAAABQc/z0_6WtVWtvARtMk0i9_AtLeyyGyV6AI4wCLcB/s640/nmt-model-fast.gif)
	 
	 - *Figure 2


**Decoder:** `A two-layer character-level decoder then takes the source context vector from the attention mechanism and predicts each target character.`



Parts of this architecture might not be new, however the previous approaches have not been on Character 2 Character fully, and the last few approaches were either partially character level and very slow.

Cool fun things out of this model:

* Share a single character- level encoder across multiple languages by training a model on a many-to-one translation task. Outperforms the subword-level encoder on all the language pairs.
* Advantage of character-level models is that they are better suited for multilingual translation than their word-level counterparts which require a separate word vocabulary for each language

* Model’s ability to handle intra-sentence code-switching, while performing language identification on the fly.
* Train character-to- character NMT model without any explicit segmentation;
* Share a single character-level encoder across multiple languages to build a multilingual translation system without increasing the model size.

More: 

Notes from: [Recent Advances and the Future of Neural Machine Translation](https://ufal.mff.cuni.cz/mtm16/files/12-recent-advances-and-future-of-neural-mt-orhat-firat.pdf)

Potential Benefits: 

1. Positive language transfer across many language pairs/directions 
	* Solution to low/zero-resource machine translation
2. Number of parameters grows linearly  with respect to the number of languages as opposed to the quadratic explosion when training many single-pair models.
3. Multi-source translation without requiring any multi-way parallel text


##### Other notes:
* bpe: [Byte pair encoding](https://en.wikipedia.org/wiki/Byte_pair_encoding)

-----


- Figure 1:  Jason Lee, Kyunghyun Cho, Thomas Hofmann, (2016)  Fully Character-Level Neural Machine Translation without Explicit Segmentation. arXiv:1610.03017
- Figure 2: Quoc V. Le & Mike Schuster, Research Scientists, Google Brain Team, (2016), [A Neural Network for Machine Translation, at Production Scale](https://research.googleblog.com/2016/09/a-neural-network-for-machine.html).

References:

* Ferdinand de Saussure. 1916. Course in General Linguistics.
* Rico Sennrich, Barry Haddow, and Alexandra Birch. 2015. Neural machine translation of rare words with subword units. In Proceedings of the 54th Annual Meeting of the Association for Computational Linguis- tics

* Marta R. Costa-Jussa and Jos` e A. R. Fonollosa. 2016. Character-based neural machine translation. In Proceedings of the 54th Annual Meeting of the Association for Computational Linguistics, page 357.

* Wang Ling, Isabel Trancoso, Chris Dyer, and Alan W Black. 2015. Character-based neural machine transla- tion. arXiv preprint arXiv:1511.04586.
* Minh-Thang Luong and Christopher D. Manning. 2016. Achieving open vocabulary neural machine translation with hybrid word-character models. In Proceedings of the 54th Annual Meeting of the Association for Com- putational Linguistics.
* Dzmitry Bahdanau, Kyunghyun Cho, and Yoshua Bengio. 2015. Neural machine translation by jointly learning to align and translate. In Proceedings of the International Conference on Learning Representations (ICLR).
