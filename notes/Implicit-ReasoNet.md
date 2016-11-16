##### Implicit ReasoNet: Modeling Large-Scale Structured Relationships with Shared Memory. (Yelong Shen, Po-Sen Huang, Ming-Wei Chang, Jianfeng Gao) [ arXiv:1611.04642 [cs.AI]](https://arxiv.org/abs/1611.04642)




##### Brief Summary 2x Speed: #####
------

* Beats the previous models for FB15k benchmark by 5.7%

Tasks:
----

* Knowledge Base Completion (KBC tasks (Bordes et al., 2013))<sup id="a1">[1](#f1)</sup>

* A Synthetic Task: shortest path synthesis, to evaluate the inference capability over a shared memory.

  - Evaluate IRNs on another task requiring multi-step inference.
  - Select the sequence generation task so that we are able to analyze the inference capability of IRNs in details.

Contributions:
----

* Implicit ReasoNets (IRNs), is the model introduced here with a new architecture using a shared memory guided by a search controller, allows to model large-scale structured relationships implicitly.
* IRNs demonstrated on above mentioned tasks:
  * it beats the FB15k benchmark previous models by 5.7%.
  * IRNs for shortest path synthesis does better than sequence-to-sequence.<sup id="a2">[2](#f2)</sup>


----
##### Brief Summary 1x Speed:

This model (IRN)s can be looked at as a part of the same models meentioned below.

Except the "biggest difference between IRNs model and the existing frameworks is the search controller & the use of the shared memory."

* Memory Networks (MemNN) (Weston et al.,<sup id="a3">[3](#f3)</sup> 2014; Sukhbaatar et al.,2015<sup id="a4">[4](#f4)</sup>) and Neural Turing Machines (NTM) (Graves et al., 2014; 2016)<sup id="a10">[10](#f10)</sup>.


* IRN is almost Reasonet++, (Shen et al., 2016)<sup id="a5">[5](#f5)</sup>. Where in this case the search controller module dynamically perform a multi-step inference depending on the complexity of the instance.

* Other models mentioned such as MemNN and NTM explicitly store inputs (which is expensive and makes a bulky model), while IRNs does not store all the observed inputs in the shared memory. The shared memory component comes in to assist here.


Magic: please explain??
* We randomly initialize the memory and update the memory with respect to task-specific objectives. Inspired from the work of Munkhdalai<sup id="a6">[6](#f6)</sup>. This allows IRNs to perform multi-step for each instance dynamically.

Model Structure:
----

**note: Input & Output are task dependent.**

1. *Input Module*: The input module takes a query and converts the query into a vector representation q.

2. *Shared Memory* **(M)**:
  * The shared memory is denoted as. It consists of a list of memory vectors,
M = {mi}i=1...I , where mi is a fixed dimensional vector. The memory vectors are randomly initialized and automatically updated through back-propagation. The shared memory component is shared across all instances.

3. *Search Controller* **(s)**: is an RNN, keeps internal state sequences to track the current search process + history.

  Search Controller Suppliments:
  * Attention mechanism used to fetch information from relevant memory vectors in **M**.
  * This decides to tiehr output the prediction or continue to generate the next possible output.

  RNN gates:
  - *Internal State:* **S**, which is a vector representation of the search process. *s1* is vector representation of q at initial state.

  - *Attention to memory:* Attention vector xt at t-th time step is generated based on the current internal state st and the shared memory M.

  - *Termination Control:* In this paper, this is a logistical regression where the W is learned. The terminate gate produces a stochastic random variable according to the current internal state tt, . tt is a binary random variable. This decided to output or continue to generate next steps.

4. *Output Module* **function f(o)** :, takes hidden state received from the search controller (s) into an output O.

* the model is optimized using the output prediction O with respect to a ground-truth target using a task-specified loss function.

#### Okay how does this really work though?
----

note: below is Algorithm 1 from the paper.

1. Model converts a task-dependent input to a vector representation through the input module.
2. Model uses the input representation to initialize the search controller.
3. Each time step, the search controller determines whether the process is finished by sampling from the distribution according to the terminate gate.
  * If termination: the output module will generate a task-dependent prediction given the search controller states.
  * If continuation: the search controller will move on to the next time step, and create an attention vector based on the current search controller state and the shared memory.
  * *note: RL alert, "The inference process of an IRN is considered as a Partially Observable Markov Decision Process (POMDP) (Kaelbling et al., 1998)"<sup id="a7">[7](#f7)</sup>...."The reward can only be received at the final termination step", thus they have used inspiration from their previous work: "REINFORCE (Williams, 1992)<sup id="a8">[8](#f8)</sup> based Contrastive Reward method, to maximize the expected reward."*

![img](http://i.imgur.com/jsmeV40.png)


----

Future work by the authors might include:

* IRNs in twoways: develop techniques to exploit ways to generate human understandable reasoning interpretation from the shared memory, such as Ribeiro et al. (2016)<sup id="a9">[9](#f9)</sup>
* Apply IRNs to infer the relationships in unstructured data such as natural language. (I think they want to beat Babi Tasks as well :) )




----

ToDo by me:
* Compare IRNs to EntNet, since both of them have an efficient approach towards read & write VS expensive NTM, DNC.
* Show the Shortest Path details here:



----

References:

<b id="f1">1</b>[↩](#a1): Antoine Bordes, Jason Weston, Ronan Collobert, and Yoshua Bengio. Learning structured embeddings of knowledge bases. In Proceedings of the Twenty-Fifth AAAI Conference on Artificial Intelligence, pp. 301–306, 2011.

<b id="f2">2</b>[↩](#a2): Sainbayar Sukhbaatar, JasonWeston, Rob Fergus, et al. End-to-end memory networks. In Advances in Neural Information Processing Systems, pp. 2440–2448, 2015.

<b id="f3">3</b>[↩](#a3): Jason Weston, Sumit Chopra, and Antoine Bordes. Memory networks. arXiv preprint arXiv:1410.3916, 2014.

<b id="f4">4</b>[↩](#a4): Sainbayar Sukhbaatar, JasonWeston, Rob Fergus, et al. End-to-end memory networks. In Advances in Neural Information Processing Systems, pp. 2440–2448, 2015.

<b id="f5">5</b>[↩](#a5): Yelong Shen, Po-Sen Huang, Jianfeng Gao, andWeizhu Chen. Reasonet: Learning to stop reading in machine comprehension. CoRR, abs/1609.05284, 2016.

<b id="f6">6</b>[↩](#a6): Tsendsuren Munkhdalai and Hong Yu. Neural semantic encoders. CoRR, abs/1607.04315, 2016.

<b id="f7">7</b>[↩](#a7): Leslie Pack Kaelbling, Michael L. Littman, and Anthony R. Cassandra. Planning and acting in partially observable stochastic domains. Artificial Intelligence, 101:99–134, 1998.

<b id="f8">8</b>[↩](#a8): Ronald J.Williams. Simple statistical gradient-following algorithms for connectionist reinforcement learning. Machine Learning, 8:229–256, 1992.

<b id="f9">9</b>[↩](#a9): Marco Tulio Ribeiro, Sameer Singh, and Carlos Guestrin. "why should i trust you?": Explaining the predictions of any classifier. In KDD, 2016

<b id="f10">10</b>[↩](#a10): Alex Graves, Greg Wayne, Malcolm Reynolds, Tim Harley, Ivo Danihelka, Agnieszka Grabska-Barwinska, Sergio Gómez Colmenarejo, Edward Grefenstette, Tiago Ramalho, John Agapiou, et al. Hybrid computing using a neural network with dynamic external memory. Nature, 2016.