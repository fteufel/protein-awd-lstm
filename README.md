# Protein-AWD-LSTM

## Description
This repo contains implementations of AWD-LSTM and SHA-RNN to be used with TAPE for protein language modeling.  
Model code is based on:
- https://github.com/salesforce/awd-lstm-lm (AWD-LSTM)
- https://github.com/Smerity/sha-rnn (SHA-RNN)

Base classes are inherited from TAPE, which needs to be installed.  
https://github.com/songlab-cal/tape


## Features

### Hidden state resetting
These models are trained with stochastic truncated backpropagation through time, where all sequences are concatenated and subsegments are cut from this concatenation to yield training minibatches.  

It's unclear whether this is a good idea for proteins, as we would leave it to the model to learn that it should forget everything that came before a `SEP` token. These implemenation support hidden state resetting, where the model resets the respective positions of the hidden states to  0 after encountering a `SEP` token.  
There is also experimental support for this in SHA-RNN, where in addition the self-attention mask is modified to ignore memory before a `SEP`. The behaviour is controlled by specifiyng a `reset_token_id` in the model config.

### Memory-mapped datasets
The default implementations load full datasets into memory for truncated BPTT. This doesn't work for large datasets. We use `VirtualBatchTruncatedBPTTHdf5Dataset`, a class that uses pre-tokenized and pre-concatenated Hdf5 datasets to emulate the stochastic minibatch generation. The Hdf5 files are created with the provided static methods of the class.
  
## Notes  
  
Models are trained with tensors in (sequence_length, batch_size,) format.

The performance of the SHA-RNN implementation was not tested.