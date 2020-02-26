---
title: Learning to Align and Translate
categories: NLP
tags: [attention, machine-translation]
---
## Neural Machine Translation by Jointly Learning to Align and Translate (2014)
> Authors : Dzmitry  Bahdanau, Kyunghyun  Cho, Yoshua  Bengio

## Abstract
- **Task** : Machine Translation (English - French).

- Encoder - decoder architectures were introduced as an alternative to **phrase-based** models.

- Encoder produces a *fixed length* vector for an input sequence, and the decoder tries to generate a output sequence from that vector.

- **Problem**
	- Difficult to capture all the information in a single vector, especially for a longer sequence words.

- **Solution**
	- Introducing **"Attention mechanism"** to decoder.

## Proposed Idea

- **Objective** : argmax<sub>y</sub> *p* ( y | x ), maximize the conditional probability of target sentence *y* given *x*.

- Instead of generating fixed length vectors, whenever a decoder generates a output, it searches (soft-search) among source words where the most relevant information is present.

- We especially uses RNN as our basic component for encoder and decoders.

## Implementation
![image](/assets/images/Attention-Week-1/attention_1_2_arch.png)
##
![image](/assets/images/Attention-Week-1/attention_1_3_arch.png)
- **Encoder** (BiLSTM) accepts previous hidden state (h<sub>t-1</sub>) and input word (x<sub>t</sub>)
	- We skip the output of encoder and keeping only the hidden states, which we pass to next time step. equation,
	h<sub>t</sub> = f ( x<sub>t</sub> , h<sub>t-1</sub> )
	, where  _f_ is non-linear
- **Decoder** (BiLSTM) accepts **context vector** (c<sub>i</sub>), previous output (y<sub>i-1</sub>) and last hidden state (s<sub>i-1</sub>).
	- equation,
	y<sub>t</sub> = g ( y<sub>t-1</sub> , s<sub>t</sub> , c<sub>t</sub>)
	, where  _g_ is non-linear

- **STEPS**
	1. **Producing the Encoder Hidden States** - Encoder produces hidden states of each element in the input sequence
![image](/assets/images/Attention-Week-1/attention_2_encoder.png)
	2. **Calculating Alignment Scores** between the previous decoder hidden state and each of the encoder’s hidden states are calculated (Note: The last encoder hidden state can be used as the first hidden state in the decoder)
![image](/assets/images/Attention-Week-1/attention_3_alignment_score.png)
	3. **Softmaxing the Alignment Scores** - the alignment scores for each encoder hidden state are combined and represented in a single vector and subsequently softmaxed
![image](/assets/images/Attention-Week-1/attention_4_softmax.png)
	4. **Calculating the Context Vector** - the encoder hidden states and their respective alignment scores are multiplied to form the context vector
![image](/assets/images/Attention-Week-1/attention_5_context_vector.png)
	5. **Decoding the Output** - the context vector is concatenated with the previous decoder output and fed into the Decoder RNN for that time step along with the previous decoder hidden state to produce a new output
![image](/assets/images/Attention-Week-1/attention_6_decoder_output.png)
	7. **The process (steps 2-5)** repeats itself for each time step of the decoder until an token is produced or output is past the specified maximum length
- **Overall Architecture**
![image](/assets/images/Attention-Week-1attention_1_arch.JPG)
## Training
- Corpus - 348M words
- Experimenting with 2 models
	1. RNNenc-dec (RNN encoder-decoder without attention)
	2. RNNsearch (RNN encoder-decoder with attention)
- On 2 data setting,  sentences of 
	1. Length up to 30 words -> RNNencdec-30, RNNsearch-30
	2. Length up to 50 words -> RNNencdec-50, RNNsearch-50
![image](/assets/images/Attention-Week-1/attention_7_training.png)

## Output
![image](/assets/images/Attention-Week-1/attention_8_output.png)
