Word2Vec is a popular technique in natural language processing (NLP) that is used to convert words or phrases into vectors of numerical values. These vectors capture the semantic meaning of words in a continuous vector space, allowing mathematical operations to be performed on them. Word2Vec models are trained <mark style="background: #ABF7F7A6;">on large text corpora to learn word embeddings</mark> that represent words in a way that similar words are closer to each other in the vector space.

**Motivation:** Word2Vec addresses the limitations of traditional text representation methods like one-hot encoding, which cannot capture semantic relationships between words. Word2Vec aims to create dense, continuous-valued vectors that encode semantic similarities between words.

**Two Variants of Word2Vec:**

Both are neural networks, ffter training, we extract the weights from the hidden layer corresponding to each word. These weights form the dense vectors or embeddings for each word.

1. **Skip-gram:**
    
    - The skip-gram model aims to <mark style="background: #BBFABBA6;">predict</mark> the <mark style="background: #FFF3A3A6;">context words</mark> (surrounding words) given a target word.
    - The target word is used as input, and the model tries to predict the context words.
    - Good for capturing meaning in less frequent words.
2. **Continuous Bag of Words (CBOW):**
    
    - The CBOW model aims to <mark style="background: #FFB8EBA6;">predict the target word given a context of surrounding words</mark>.
    - The context words are used as input, and the model tries to predict the target word.
    - Good for capturing meaning of more frequent words.

**Training Process:**

1. **Data Preparation:**
    
    - Large text corpora are used for training.
    - Text is preprocessed by tokenization and removing stop words, punctuation, etc.
2. **Creating Training Pairs:**
    
    - For the skip-gram model, pairs of (target word, context word) are generated from the text.
    - For the CBOW model, pairs of (context words, target word) are generated.
3. **Neural Network Architecture:**
    
    - A neural network is designed with an input layer, one or more hidden layers, and an output layer.
    - The hidden layer(s) represent the word embeddings.
4. **Training:**
    
    - The neural network is trained using backpropagation and gradient descent.
    - The weights of the network are updated to minimize the difference between predicted and actual context words.
5. **Learning Word Embeddings:**
    
    - After training, the weights of the hidden layer(s) are used as the word embeddings.

----

### Word2Vec Training using Skip-Gram Model

#### Data Preparation
1. Tokenize the text data into words.
2. Create a vocabulary of unique words.

#### One-Hot Encoding
1. Represent each word in the vocabulary as a one-hot encoded vector.
2. If vocabulary size is $V$, each word's one-hot vector has length $V$, with '1' at the word's index and '0' elsewhere.

#### Neural Network Architecture
1. The neural network consists of:
   - Input layer: Equal to vocabulary size ($V$).
   - Hidden layer: The embedding layer with size $N$, where $N$ is the desired size of word embeddings.
   - Output layer: Equal to vocabulary size ($V$).

#### Objective Function (Loss Function)
1. The loss function is the negative log likelihood.
2. For a single training pair (target word, context word):
   - Loss: $J(\theta) = -\log P(\text{context word}|\text{target word})$

#### Softmax Function
1. Calculate predicted probabilities using the softmax function:
   - $P(\text{context word}|\text{target word}) = \frac{e^{\text{context embedding} \cdot \text{target embedding}}}{\sum_{w \in \text{vocabulary}} e^{\text{context embedding} \cdot \text{embedding}_w}}$

#### Training
1. Initialize the embedding layer weights randomly.
2. For each training pair (target word, context word):
   - Forward pass to calculate predicted probabilities.
   - Calculate loss using negative log likelihood.
   - Backpropagation to calculate gradients.
   - Update embedding layer weights using gradient descent.

#### Learning Word Embeddings
1. After training, the weights of the embedding layer represent word embeddings.
2. These embeddings capture semantic relationships between words based on co-occurrence patterns.

#### Word Similarity
1. Use cosine similarity or other distance metrics to measure semantic similarity between word embeddings.
2. Similar words will have similar vector representations.

