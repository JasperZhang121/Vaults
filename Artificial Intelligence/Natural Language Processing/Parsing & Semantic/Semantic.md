Semantic understanding is a fundamental aspect of NLP, aiming to comprehend the meaning of language beyond its surface structure. It deals with understanding the meaning of words, phrases, sentences, and even entire documents. Semantic understanding in NLP can be categorized into two main dimensions: logical semantics and lexical semantics.

**1. Logical Semantics:**

Logical semantics focuses on the formal representation of meaning in natural language. It aims to capture the <mark style="background: #FFB8EBA6;">truth conditions of sentences and the relationships between different linguistic expressions</mark>. Here are some key concepts within logical semantics:

**a. Propositional Logic:**
   - In propositional logic, statements are represented as propositions that can be either true or false.
   - Logical operators like AND, OR, NOT, and IMPLIES are used to connect propositions.
   - For example, the sentence "It is raining, and the sun is shining" can be represented as \(P \wedge Q\), where \(P\) represents "It is raining" and \(Q\) represents "The sun is shining."

**b. First-Order Logic (Predicate Logic):**
   - First-order logic extends propositional logic by introducing variables, predicates, and quantifiers.
   - Predicates represent relationships between objects, and quantifiers specify the scope of variables.
   - For example, the sentence "All cats chase mice" can be represented as \($\forall x (\text{Cat}(x) \rightarrow \exists y (\text{Mouse}(y) \wedge \text{Chase}(x, y)))$\).

**c. Semantic Roles and Frames:**
   - Semantic roles identify the specific functions that words or phrases play within a sentence's structure.
   - Frames are knowledge structures that capture the typical attributes and actions associated with certain concepts.
   - For example, in the sentence "The cat chased the mouse," "cat" would be associated with the frame for "animal" and the role of "agent," while "mouse" would be associated with the role of "patient."

**2. Lexical Semantics:**

Lexical semantics deals with the <mark style="background: #D2B3FFA6;">meaning of individual words</mark>, their relationships with other words, and how they combine to form meaningful phrases and sentences. Key aspects of lexical semantics include:

**a. Word Sense Disambiguation (WSD):**
   - Many words have multiple senses (meanings) depending on context.
   - WSD aims to determine the correct sense of a word in a given context.
   - For example, in the sentence "I saw a bat," "bat" could refer to either a flying mammal or a sports equipment, and WSD helps disambiguate it.

**b. Synonymy and Antonymy:**
   - Synonyms are words with similar meanings (e.g., "big" and "large").
   - Antonyms are words with opposite meanings (e.g., "hot" and "cold").
   - Lexical semantics explores relationships between words based on similarity and contrast.

**c. Word Embeddings:**
   - Word embeddings like Word2Vec and GloVe are techniques that represent words as dense vectors in a continuous vector space.
   - These vectors capture semantic relationships between words based on their co-occurrence patterns in large text corpora.

**d. Distributional Semantics:**
   - Distributional semantics posits that words with similar distributions in text are likely to have similar meanings.
   - It's the basis for many word embedding models and distributional similarity measures.