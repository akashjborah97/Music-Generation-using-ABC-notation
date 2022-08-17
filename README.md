# Music-Generation-using-ABC-notation
The project builds a Recurrent Neural Network (RNN) for music generation. The model is trained to to learn the patterns in raw sheet music in ABC notation, which generates music.
The dataset is collected from kaggle and contains ABC notation of songs. Link: https://www.kaggle.com/datasets/raj5287/abc-notation-of-tunes?datasetId=156963&sortBy=dateRun&tab=profile
The text in the dataset are first vectorised to create numeric representation to maintain a lookup table. 
We will be using some example sequences for training the RNN with a definite sequence length. There will also be a target sequence for predection the next character. This will be implemented using the batch method which will convert this stream of character indices to sequences of the desired size.
The RNN model we create will be based on LSTM architecture and has initializer "glorot_uniform" and activation function "sigmoid", where we will use state vector to maintain information about the temporal relationships between consecutive characters. The final output of the LSTM is then fed into a fully connected Dense layer where we'll output a softmax over each character in the vocabulary, and then sample from this distribution to predict the next character.

Layer 1: Embedding layer to transform indices into dense vectors of a fixed embedding size

Layer 2: LSTM with rnn_units number of units.

Layer 3: Dense (fully-connected) layer that transforms the LSTM output into the vocabulary size.

The RNN model is then trained using a form of the crossentropy loss (negative log likelihood loss)i.e., sparse_categorical_crossentropy loss, as it utilizes integer targets for categorical classification tasks. We want to compute the loss using the true targets -- the labels -- and the predicted targets -- the logits.
Hyperparameter are defined for setting and optimization. Adam optimizer is used as the optimizer for training operation. The model is trained for 3000 iterations, with batch size 10 and sequence length 100. The model learns better when the learning rate is set to 1e-3. 
To generate music, the model follows a prediction procedure:

Step 1: Initialize a "seed" start string and the RNN state, and set the number of characters we want to generate.

Step2: Use the start string and the RNN state to obtain the probability distribution over the next predicted character.

Step3: Sample from multinomial distribution to calculate the index of the predicted character. This predicted character is then used as the next input to 
the model.

Step 4: At each time step, the updated RNN state is fed back into the model, so that it now has more context in making the next prediction. After predicting the next character, the updated RNN states are again fed back into the model, which is how it learns sequence dependencies in the data, as it gets more information from the previous predictions.

Now, the model generate songs with a user defined "start_string" and "generation_length".
