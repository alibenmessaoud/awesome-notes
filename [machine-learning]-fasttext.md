# fasttext

fastText is a library for efficient learning of word representations and sentence classification.



#### Tutorial

##### Text classification [https://fasttext.cc/docs/en/supervised-tutorial.html]

###### Init project

`wget https://github.com/facebookresearch/fastText/archive/v0.1.0.zip`

`unzip v0.1.0.zip`

`cd fastText-0.1.0`

`make`

###### Get and prepare data

`wget https://s3-us-west-1.amazonaws.com/fasttext-vectors/cooking.stackexchange.tar.gz && tar xvzf cooking.stackexchange.tar.gz`

`wc cooking.stackexchange.txt` 
15404  169582 1401900 cooking.stackexchange.txt

Our full dataset contains 15404 examples. Let's split it into a training set of 12404 examples and a validation set of 3000 examples:

`head -n 12404 cooking.stackexchange.txt > cooking.train`

`tail -n 3000 cooking.stackexchange.txt > cooking.valid`

###### First classifier

`./fasttext supervised -input cooking.train -output model_cooking`

Read 0M words
Number of words:  14598
Number of labels: 734
Progress: 100.0%  words/sec/thread: 75109  lr: 0.000000  loss: 5.708354  eta: 0h0m

It uses `cooking.train` as entry and and `model_cooking`  as store to the result model!

It is possible to directly test our classifier interactively: `./fasttext predict model_cooking.bin -`

Typing a sentence :

`Which baking dish is best to bake a banana bread ?`

The predicted tag is *baking* which fits well to this question. Let us now try a second example:

`Why not put knives in the dishwasher?`

The label predicted by the model is *food-safety*, which is not relevant. Somehow, the model seems to fail on simple examples.

To get a better sense of its quality, let's test it on the validation data by running: `./fasttext test model_cooking.bin cooking.valid `

N  3000
P@1  0.124
R@1  0.0541
Number of examples: 3000