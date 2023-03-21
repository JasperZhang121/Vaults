
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
-   The five-number summary consists of the <mark style="background: #BBFABBA6;">minimum value, Q1, median, Q3, and maximum value</mark> of a dataset.
-   Boxplots can be used to visually compare sets of compatible data, with the box representing the quartiles and the whiskers extending to the min and max values or to 1.5 x IQR if there are outliers.
-   An <mark style="background: #BBFABBA6;">outlier is a value below Q1 - 1.5 IQR or above Q3 + 1.5 IQR</mark>, and they are often plotted individually in a boxplot.


### [[2.4 Computation of Measures]]

-   In statistics, <mark style="background: #FFB86CA6;">computational</mark> cost of all processing stages should be considered when dealing with big data.
-   Some measures of data character, data summarization, and data pattern interestingness are computationally more difficult to compute than others.
-   Measure functions can be classified as <mark style="background: #FF5582A6;">distributive, algebraic, or holistic</mark> based on their computational complexity.
-   Distributive functions are applied to each partition of the data and then combined to obtain the result.
-   Algebraic functions can be computed by a function with a bounded number of arguments, each of which is obtained by applying a distributive function.
-   Holistic functions are difficult to compute efficiently and often require approximations.
-   Mean, median, mode, midrange, quartiles, variance, standard deviation, and interquartile range are commonly used measures of central tendency, variability, and distribution in data.
-   Boxplots can be used to summarize and compare data, and to identify outliers.


### [[2.5 Measuring Correlation amongst Two Variables]]

-   Measures of correlation are used to assess the relationship between two variables.
-   <mark style="background: #FF5582A6;">Pearson's correlation coefficient</mark> measures the strength and direction of the linear relationship between two continuous variables.
$$r = \frac{\sum_{i=1}^{n}(x_i - \bar{x})(y_i - \bar{y})}{\sqrt{\sum_{i=1}^{n}(x_i - \bar{x})^2} \sqrt{\sum_{i=1}^{n}(y_i - \bar{y})^2}}$$
-   The coefficient <mark style="background: #BBFABBA6;">ranges from -1 (perfect negative correlation) to 1 (perfect positive correlation), with 0 indicating no correlation</mark>.
-   Spearman's rank correlation coefficient and Kendall's tau coefficient are nonparametric measures of correlation that do not require the assumption of a linear relationship between the variables. $$r_s = 1 - \frac{6\sum d_i^2}{n(n^2 - 1)}$$
$$\tau = \frac{2}{n(n-1)} \sum_{i<j} sgn(x_i - x_j) sgn(y_i - y_j)$$

-   Spearman's rank correlation coefficient is based on the ranks of the observations, while Kendall's tau coefficient is based on the signs of the differences between the observations.
-   These measures of correlation can be calculated using various statistical software packages or programming languages.


### [[2.6 Measuring Similarity and Dissimilarity of Multivariate Data]]

-   Similarity and dissimilarity measures are used to assess how alike or unlike objects are to each other.
-   <mark style="background: #BBFABBA6;">Similarity is a numerical measure of how alike two data objects</mark> are, often falling in the range of [0, 1].
-   Dissimilarity, also known as distance, is a numerical measure of how different two data objects are, with the minimum dissimilarity being 0 and upper limit varying.
-   A dissimilarity matrix is a common data structure used to represent dissimilarity among objects, with an n x n matrix data structure showing dissimilarity or distance d(i,j) for all pairs of n objects i and j.
-   The matrix is symmetric or triangular with zeros along the diagonal and duplicates in the upper right triangle.
-   Similarity can be expressed as a function of dissimilarity, such as <mark style="background: #FF5582A6;">sim(i,j) = 1 - d(i,j)</mark>, when d(i,j) is normalized to [0,1].
-   Distances can be calculated for each attribute type, and then combined to give an overall object distance for objects with heterogeneous types of attributes.


### [[2.7 Proximity Measures for Nominal Attributes]]

1.  **Simple matching method:** Calculate the number of matches (m) between two objects for all their attributes and divide it by the total number of attributes (p) to obtain the dissimilarity measure (d). The similarity measure (sim) can be obtained by subtracting d from 1. This method is based on the idea that objects with a higher number of matches are more similar.

$$d(i,j) = \frac{p-m}{p}$$



$$sim(i,j) = 1-d(i,j) = \frac{m}{p}$$    
2.  **Mapping to binary attributes method:** Create a new binary attribute for each state of the nominal attribute and map each object to a corresponding object with binary attributes. Then, use any method for proximity of binary attributes to calculate the similarity or dissimilarity. This method is also known as one-hot encoding.


### [[2.8  Proximity Measures for Binary Attributes]]

|                 | 1  | 0 |
|-----------------|-----------|----------|
| 1  | q         | r        |
|  0  | s         | t        |

-   Binary attributes have two values: 0 or 1.
-   There are two types of binary attributes: <mark style="background: #FFB86CA6;">symmetric and asymmetric</mark>.
	- Dissimilarity for symmetric binary attributes :
		- d(i,j) = (r + s) / (q + r + s + t)
	- Dissimilarity for asymmetric binary attributes:
		- d(i,j) = (r + s ) / (q + r + s )
		- sim(i,j) = 1 - d(i,j)

-   Treating binary attributes as numeric can be misleading.
-   The contingency table is used to count the number of matching and non-matching attributes for two objects.
-   For symmetric binary dissimilarity, the dissimilarity is defined as the proportion of non-matching attributes amongst all attributes.
-   For asymmetric binary dissimilarity, the dissimilarity is defined as the proportion of non-matching attributes among the matching attributes, and the corresponding similarity is called the Jaccard coefficient.
-   Alternatively, the Jaccard similarity between two objects can be thought of as the size of the intersection of the two sets divided by the size of the union of the sets.

### [[2.9 Normalisation of numeric data]]

There are different methods of normalization, such as:

- <mark style="background: #FF5582A6;">Min-Max Scaling</mark>: This is the most commonly used normalization technique, which rescales the data to a fixed range between 0 and 1. The formula for min-max scaling is:

$$ x_{\text{normalized}} = \frac{x - \min(x)}{\max(x) - \min(x)} $$

where $x$ is the original data and $x_{\text{normalized}}$ is the normalized data.

- <mark style="background: #FF5582A6;">Z-Score Normalization</mark>: This technique standardizes the data by transforming it to have a mean of 0 and a standard deviation of 1. The formula for z-score normalization is:

$$ x_{\text{normalized}} = \frac{x - \text{mean}(x)}{\text{standard deviation}(x)}$$

where $x$ is the original data, $\text{mean}(x)$ is the mean of the data, $\text{standard deviation}(x)$ is the standard deviation of the data, and $x_{\text{normalized}}$ is the normalized data.

- <mark style="background: #FF5582A6;">Decimal Scaling</mark>: This technique involves dividing each observation by a power of 10 to move the decimal point. The number of decimal places is chosen based on the maximum absolute value of the data. For example, if the maximum absolute value of the data is 1000, we can divide each observation by 1000 to move the decimal point three places to the left.


### [[2.10 Minkowski Distance]]

$d_{M}(x,y) = (\sum_{i=1}^{n} {|x_{i} - y_{i}|}^p)^\frac{1}{p}$


----

### [[0. Basic concepts]]

-   A data warehouse is a <mark style="background: #FF5582A6;">decision support database</mark> that is maintained separately from the operational database and supports information processing by providing a solid platform of <mark style="background: #FF5582A6;">consolidated, historical data</mark> for analysis.
-   A data warehouse is subject-oriented, integrated, time-variant, and nonvolatile.
-   Data warehousing is the process of constructing and using data warehouses.
-   Data warehouses are organised around major subjects and provide a simple and concise view around particular subject issues by excluding data that are not useful in the decision support process.
-   Data warehouses are constructed by integrating multiple, heterogeneous data sources, and data cleaning and data integration techniques are applied.
-   The time horizon for the data warehouse is significantly longer than that of operational systems, and every key structure in the data warehouse contains an element of time, explicitly or implicitly.
-   Data warehouses are nonvolatile, meaning that they are a physically separate store of data transformed from the operational environment, and operational update of data does not occur in the data warehouse environment.
-   Data warehouses are built specifically for analytics to support decision making, that is, online analytical processing (OLAP), while operational databases are used for day-to-day operations, that is, online transaction processing (OLTP).
-   The formula for calculating the number of cells in an n-dimensional data cube with m levels for each dimension is C = (m+1)^n - 1, where C is the total number of cells in the cube, n is the number of dimensions in the cube, and m is the number of levels for each dimension.