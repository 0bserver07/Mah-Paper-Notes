
#### Memory networks: [(arXiv:1410.3916)](https://arxiv.org/abs/1410.3916)]

** Incomplete, need notes:**

This was for `ICLR 2015`

* Memory networks reason with inference components combined with a long-term memory component; they learn how to use these jointly.

* The long-term memory can be read and written to, with the goal of using it for prediction.

* general framework in Section 2,

* present a specific implementation in the text domain for the task of question answering in Section 3.

* We discuss related work in Section 4, describe our experiments in 5, and finally conclude in Section 6.


* A memory network consists of a memorym(an array of objects1 indexed bymi) and four (poten- tially learned) components I, G, O and R as follows:

    I: (input featuremap) – converts the incoming input to the internal feature representation.

    G: (generalization) – updates old memories given the new input. We call this generalization as there is an opportunity for the network to compress and generalize its memories at this stage for some intended future use.

    O: (output feature map) – produces a new output (in the feature representation space), given the new input and the currentmemory state.

    R: (response) – converts the output into the response format desired. For example, a textual response or an action.



Given an input x (e.g., an input character,word or sentence depending on the granularity chosen, an image or an audio signal) the flow of the model is as follows:

    1. Convert x to an internal feature representation I(x).
    2. Update memories mi given the new input: mi = G(mi, I(x),m), ∀i.
    3. Compute output features o given the new input and the memory: o = O(I(x),m).
    4. Finally, decode output features o to give the final response: r = R(o).




* How should a machine learning model deal with this? Ideally
A: A possiblewaywould be to use a languagemodel: given the neighboringwords, predictwhat theword should be, and assume the new word is similar to that.


* In our basic architecture, the I module takes an input text. Let us first assume this to be a sentence: either the statement of a fact, or a question to be answered by the system (later we will consider word-based input sequences). The text is stored in the next available memory slot in its original
form2, i.e., S(x) returns the next empty memory slot N: mN = x, N = N + 1.

* The G module is thus only used to store this new memory, so old memories are not updated. More sophisticated models are described in subsequent sections.
* The core of inference lies in the O and R modules. The O module produces output features by finding k supporting memories given x.
