
### [[1.1 Why Data Mining]]
- Explosive Growth of Data:
	- Collection and Availability: <mark style="background: #FF5582A6;">Automated tools</mark>
	- Sources: Business, Science, Society and everyone


### [[1.2 What is Data Mining]]

- **Interesting patterns and knowledge** 
	from data (**non-trivial, implicit, previously unknown and potentially useful)
- Building models
	**evaluating the models** for usefulness
- KDD 
	**Data cleaning** -> **Data integration** -> **Data selection** -> **Data transformation** ->  **Data mining** -> **Pattern evaluation** -> **Knowledge representation** -> **Deployment**
	

### [[1.3. What makes a pattern useful]]

- Interestingness:
	-  Easily understood 
	-  Valid (true), with some required degree of _certainty_, on sample data 
	-  Potentially useful or actionable
	-  Novel
- Objective:
	-  Based on pattern structure and statistical properties
	-  Require a user-selected threshold to be considered interesting
	-  Examples include <mark style="background: #FF5582A6;">support, confidence, coverage, accuracy, and simplicity</mark>
	-  Measures may be specific to a particular mining method and cannot be compared across methods
- Subjective:
	-  Based on user beliefs about the data
	-  Domain-dependent and require judgment by the data miner
	-  Results need to be communicated and shared with domain experts
	-  May include unexpected findings, actionable results, or confirming a previously held belief


### [[1.4 What kind of patterns can be mined]]

-   Data Mining for Generalisation
    -   Data warehousing
    -   Multidimensional class or concept description: Characterization and discrimination
    -   Generalize, summarize, and contrast data characteristics
    -   OLAP operations can be used to summarise data along user-specified dimensions

-   Frequent Patterns, Association and Correlation Analysis
    -   Frequent patterns
    -   Association, correlation vs. causality

-   Classification and Regression for Prediction
    -   Classification
    -   Regression
    -   Typical methods and applications

-   Clustering/ Cluster Analysis
    -   Unsupervised learning
    -   Group data to form new categories
    -   Principle: Maximizing intra-cluster similarity & minimizing inter-cluster similarity

-   Outlier Analysis
    -   Outlier: A data object that does not comply with the general behavior of the data
    -   Methods: by-product of clustering or regression analysis
    -   Useful in fraud detection, rare events analysis

-   Others
    -   Sequence, trend, and evolution analysis
    -   Structure, network and text analysis
    -   Inductive logic programming


[[1.5 Challenges in Data Mining]]
-   Basic Competency
    -   Selecting the right tool for the job
    -   Evaluating the output of the tool
    -   Interpreting the results in the context of the question

-   Mining Methodology
    -   Mining various and new kinds of knowledge
    -   Mining knowledge in multi-dimensional space
    -   Interdisciplinary skills
    -   Handling noise, uncertainty, and incompleteness of data
    -   Pattern evaluation and pattern- or constraint-guided mining

-   Leveraging Human Knowledge
    -   Goal-directed mining
    -   Interactive mining
    -   Incorporation of background knowledge
    -   Presentation and visualization of data mining results

-   Efficiency and Scalability
    -   Data reduction
    -   Efficiency and scalability of data mining algorithms
    -   Parallel, distributed, stream, and incremental mining methods

-   Diversity of Data Types
    -   Handling complex types of data
    -   Mining dynamic, networked, and global data repositories

-   Data Mining and Society
    -   Social impacts of data mining
    -   Privacy-preserving data mining
    -   Bias and algorithmic transparency

### [[1.6 Privacy Implications of Data Mining]]
-   Personal data used in data mining raises concerns about privacy
-   OECD principles for fair use of personal data include limiting collection, ensuring quality and security, specifying purposes, limiting use, and enabling individual participation and accountability
-   To protect privacy when data mining, data security enhancing techniques can include multilevel security models, encryption, intrusion detection, and physical access control
-   Privacy-preserving data mining techniques: <mark style="background: #FF5582A6;">randomization methods, k-anonymity methods, L-diversity methods, federated analytics, downgrading results, differential privacy, and statistical disclosure control.</mark>

--------

### [[2.1 Data Types and Representations]]

-   Data is structured as objects with properties or attributes and may include relationships to other objects.
-   Objects may also be called samples, instances, data points, observations, rows, tuples, records, or vectors, while attributes may be called fields, variables, dimensions, properties, or features.
-   The type of an attribute defines how its values are represented, how it can be manipulated, and appropriate methods for mining.
-   Attribute types include nominal (categories, states, classes), binary (nominal with only two values), ordinal (values have a meaningful order), numeric interval-scaled (measured on a scale of equal-sized units), and numeric ratio-scaled (inherent zero-point, permitting reasoning over multiples of values).
-   Another classification of attributes is <mark style="background: #FF5582A6;">discrete (finite or countably infinite set of values) and continuous (real numbers as values)</mark>.
-   Discrete attributes may include <mark style="background: #BBFABBA6;">binary, nominal, and ordinal</mark> attributes as a special case.
-   Continuous attributes are typically represented as <mark style="background: #BBFABBA6;">floating-point</mark> variables.


### [[2.2 Basic Statistical Descriptions of a Single Data Variable]]

-   Central tendency measures in statistics represent the typical or central value of a dataset.
-   The three most common measures of central tendency are <mark style="background: #FF5582A6;">mean (average value), median (middle value), and mode (most common value)</mark>.
-   The mean is the sum of all values in a dataset divided by the number of values, but it is sensitive to outliers.
-   The median is the middle value of a dataset when the values are arranged in order, and it is less sensitive to outliers than the mean.
-   The mode is the value that appears most frequently in a dataset and is useful for dealing with categorical or discrete data.
-   The arithmetic mean is the same as the regular mean but takes into account the weights of the values in the dataset.
-   The weighted mean is a variation of the arithmetic mean that multiplies each value by a weight that reflects its importance in the dataset.
-   The <mark style="background: #BBFABBA6;">midrange is the average of the largest and smallest value</mark>s and is a cheap indication of central tendency.
-   <mark style="background: #ABF7F7A6;">Skewness</mark> is a measure of asymmetry in data and can be indicated by the difference between the mean and mode.


### [[2.3 Measuring the Dispersion of Data]]

-   The range is the difference between the maximum and minimum values in a dataset.
-   Quartiles divide a dataset into four equal parts. The first quartile (Q1) is the 25th percentile, the second quartile (Q2) is the median, and the third quartile (Q3) is the 75th percentile.
-   The Interquartile Range (IQR) is the difference between the third quartile and the first quartile.
-   Variance measures how much the data deviate from the mean.
-   Standard Deviation is the square root of the variance and measures the amount of variation or dispersion of a set of values.
-   Chebyshev's inequality states that at least (1 - 1/k^2) x 100% of the observations are no more than k standard deviations from the mean.
-   The five-number summary consists of the minimum value, Q1, median, Q3, and maximum value of a dataset.
-   Boxplots can be used to visually compare sets of compatible data, with the box representing the quartiles and the whiskers extending to the min and max values or to 1.5 x IQR if there are outliers.
-   An outlier is a value below Q1 - 1.5 IQR or above Q3 + 1.5 IQR, and they are often plotted individually in a boxplot.