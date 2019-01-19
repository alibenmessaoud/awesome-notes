# seq2seq:

tf-seq2seq is a general-purpose encoder-decoder framework for Tensorflow that can be used for Machine Translation, Text Summarization, Conversational Modeling, Image Captioning, and more.

#### Getting started

```
git clone https://github.com/google/seq2seq.git
cd seq2seq

# Install package and dependencies
pip install -e .
```

#### Concept

- Configuration: Many objects, including Encoders, Decoders, Models, Input Pipelines, and Inference Tasks, are configured using key-value parameters. 
- Input Pipeline: An `InputPipeline`defines how data is read, parsed, and separated into features and labels. For example, `ParallelTextInputPipeline` reads data from two text files.
- Encoder: An encoder reads in "source data", e.g. a sequence of words or an image, and produces a feature representation in continuous space.
- Decoder: A decoder is a generative model that is conditioned on the representation created by the encoder. 
- Model: A model defines how to put together an encoder and decoder, and how to calculate and minize the loss functions.

#### Tutorial

[todo] https://google.github.io/seq2seq/nmt/

