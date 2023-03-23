
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
-   A data warehouse is <mark style="background: #BBFABBA6;">subject-oriented, integrated, time-variant, and nonvolatile</mark>.
-   Data warehousing is the process of constructing and using data warehouses.
-   Data warehouses are organised around major subjects and provide a simple and concise view around particular subject issues by <mark style="background: #ABF7F7A6;">excluding data that are not useful</mark> in the decision support process.
-   Data warehouses are constructed by integrating multiple, heterogeneous data sources, and data cleaning and data integration techniques are applied.
-   The time horizon for the data warehouse is significantly longer than that of operational systems, and every key structure in the data warehouse contains an element of time, explicitly or implicitly.
-   Data warehouses are nonvolatile, meaning that they are a <mark style="background: #ADCCFFA6;">physically separate store of data</mark> transformed from the operational environment, and operational update of data does not occur in the data warehouse environment.
-   Data warehouses are built specifically for analytics to support decision making, that is, online analytical processing (OLAP), while operational databases are used for day-to-day operations, that is, online transaction processing (OLTP).
-   The formula for calculating the <mark style="background: #FF5582A6;">number of cells</mark> in an n-dimensional data cube with m levels for each dimension is <mark style="background: #FF5582A6;">C = (m+1)^n - 1,</mark> where C is the total number of cells in the cube, n is the number of dimensions in the cube, and m is the number of levels for each dimension.

### [[1. Multi-dimensional Data Cubes]]

-   A data warehouse uses a <mark style="background: #FF5582A6;">multidimensional data model</mark> called a <mark style="background: #FF5582A6;">data cube</mark>.
-   A data cube allows data to be viewed and analyzed in multiple dimensions.
-   The cube axes represent dimensions such as item, time, or location.
-   The <mark style="background: #BBFABBA6;">cube cells hold measures</mark> such as dollars sold, quantity sold, and quantity returned.
-   A <mark style="background: #FF5582A6;">cuboid </mark>is a component of a data cube and can be used to aggregate measures along one or more dimensions of the base cuboid.
-   The <mark style="background: #FFB8EBA6;">top-most 0-D cuboid is called the apex cuboid</mark>, which holds the highest level of summarization in a single cell.
-   The lattice of cuboids forms a data cube.
-   The bottom-most base cuboid contains cells for every possible combination.
-   Data cubes are commonly used in data warehousing and business intelligence to analyze and report on large amounts of data.

### [[2. Concept Hierarchies]]

-   A concept hierarchy provides mappings <mark style="background: #BBFABBA6;">from low-level concepts to higher-level</mark>, more general concepts.
-   They allow for the modeling of data at different levels of abstraction.
-   Examples include a geography hierarchy and a sales organization hierarchy.
-   In a geography hierarchy, data is aggregated by country at the top level and becomes more granular as you move down to store location.
-   In a sales organization hierarchy, data is aggregated by year at the top level and becomes more granular as you move down to the day level.

### [[3. Modelling the Cube in a Relational Database]]

- Dimension tables, such as item and time, and a fact table containing measures are necessary for data warehousing.
-   <mark style="background: #FF5582A6;">Star schema</mark> is a structure where <mark style="background: #FFB8EBA6;">a fact table</mark> in the middle is connected to a set of <mark style="background: #D2B3FFA6;">dimension tables</mark>.
-   In the star schema, the primary key for the fact table is composed of a key for each dimension and the remaining attributes are the measures.
-   <mark style="background: #FF5582A6;">Snowflake schema</mark> is a <mark style="background: #FFB8EBA6;">refinement of star schema</mark> where some dimensional hierarchy is normalized into a set of smaller dimension tables.
-   <mark style="background: #FF5582A6;">Fact constellation</mark> is a structure where <mark style="background: #ABF7F7A6;">multiple fact tables</mark> share dimension tables, viewed as a collection of stars or a galaxy schema.
-   Snowflake and Constellation schemes are extensions of the basic Star schema to more complex data.


### [[4. Data Mining in Data Warehouses]]

-   There are three kinds of data warehouse applications: <mark style="background: #FF5582A6;">information processing</mark>, <mark style="background: #FF5582A6;">analytical processing</mark> (OLAP), and <mark style="background: #FF5582A6;">multidimensional data mining</mark> (OLAM).
-   Information processing supports <mark style="background: #BBFABBA6;">querying, basic statistical analysis, and reporting using crosstabs, tables, charts and graphs</mark>.
-   <mark style="background: #FF5582A6;">Analytical </mark>processing (OLAP) is <mark style="background: #FF5582A6;">multidimensional analysis</mark> of data warehouse data that supports basic OLAP operations, slice-dice, drilling, pivoting and derives information summarised at multiple granularities from user-specified subsets.
-   Multidimensional data <mark style="background: #FF5582A6;">mining</mark> (OLAM) is knowledge <mark style="background: #BBFABBA6;">discovery from hidden patterns and supports finding associations, constructing analytical models,</mark> performing classification and prediction, and presenting the mining results using visualization tools.
-   OLAP operations are typically implemented in OLAP servers, while SQL standard also defines some OLAP operators but these are generally implemented inconsistently in relational databases.
-   A common architecture for multidimensional cube operations is <mark style="background: #FFF3A3A6;">ROLAP (Relational OLAP)</mark> server, which extends and optimizes relational architecture to form an OLAP server and relies on a star schema (+snowflake, fact constellation) database structure.
-   An <mark style="background: #FFF3A3A6;">MOLAP (Multidimensional OLAP)</mark> server uses a column-oriented data storage architecture, particularly well-suited for optimizing rapid access to aggregate data and storage of sparse cubes.
-   A <mark style="background: #FFF3A3A6;">hybrid architecture HOLAP</mark> (Hybrid OLAP) server combines ROLAP and MOLAP, with detailed data in a relational database and aggregations in a MOLAP store, and has the performance advantages of each.


### [[5. Typical OLAP Operations]]

**<mark style="background: #FF5582A6;">Rollup</mark>** (also called drill-up) summarizes data in one of two ways:

1.  By <mark style="background: #FFF3A3A6;">dimension reduction</mark>
2.  By <mark style="background: #FFF3A3A6;">climbing up the concept hierarchy</mark>

**<mark style="background: #FF5582A6;">Drill-down</mark>** (also called roll down) is the <mark style="background: #FFF3A3A6;">reverse of rollup</mark>, navigating from less detailed data to more detailed data.

**Slice and Dice**

1.  Slice <mark style="background: #BBFABBA6;">cuts off one dimension of the cube</mark>, not by aggregating but by selecting only one fixed value along one dimension.
2.  Dice cuts out a <mark style="background: #BBFABBA6;">sub-cube</mark>, not by aggregating but by selecting multiple fixed values for each of multiple dimensions.

**Pivot** (also called rotate) is a <mark style="background: #ADCCFFA6;">visualization operator and does not change the data</mark>. It changes the data axes in view to an alternative presentation.

**Other OLAP Operations** offered by OLAP servers:

-   Drill-across: involving (across) more than one fact table
-   Drill-through: through the bottom level of the cube to its back-end relational tables (using SQL)
-   Ranking: top-k or bottom-k items in a list ordered by some measure
-   Moving averages: over time
-   Statistical functions
-   Domain specific operators: growth rates, interest, internal rate of return, depreciation, currency conversion.

### [[6. Processing OLAP queries]]

-   Data warehouses contain large volumes of data but aim to <mark style="background: #BBFABBA6;">answer queries in interactive query time-frames</mark>.
-   Pre-computing all aggregate measures required in a data cube can deliver fast response times, but this is expensive in storage.
-   Data warehouses must support efficient cube computation, access methods, and query processing techniques.
-   A data cube is a lattice of cuboids where each cuboid represents a choice of group-by attributes.
-   The total number of cuboids in a data cube with n dimensions and m levels for each dimension is (m+1)^n - 1.
-   <mark style="background: #ADCCFFA6;">OLAP servers can materialize every, none, or some cuboids to reduce run-time query processing costs</mark>.
-   Processing OLAP queries involves determining which operations to perform on available cuboids and selecting the materialized cuboid(s) with the least estimated query processing cost.
-   Efficient cube materialization strategies include full cube materialization, cube shell, and iceberg cube.
-   The three OLAP server architectures are ROLAP, MOLAP, and HOLAP.

---


### [[4.1 Motivation]]

-   <mark style="background: #FF5582A6;">Frequent pattern mining</mark> discovers <mark style="background: #FF5582A6;">associations and correlation</mark>s in itemsets in databases.
-   Techniques were initially developed for commercial market basket analysis in the 90s.
-   The language of frequent pattern mining assumes data items are grouped into transactions.
-   The goal is to <mark style="background: #BBFABBA6;">find patterns of items that occur in a high proportion of transactions</mark>.
-   Examples include supermarket shopping, pharmaceutical benefits, cross-marketing, and DNA sequence analysis.
-   Frequent pattern mining is used in many other applications such as catalogue design, sale campaign analysis, and web clickstream analysis.
-   The focus is on association mining to <mark style="background: #FF5582A6;">learn patterns of association rules</mark>.


### [[4.2 Basic Concepts]]

-   <mark style="background: #FF5582A6;">Item</mark>: a single piece of nominal data, like "apple," "orange," or "banana."
-   <mark style="background: #FF5582A6;">Itemset</mark>: a set of one or more items, denoted by ${\cal I} = {I_1, I_2,..I_m}$, for example ${\cal I}={apple, orange, banana}$.
-   <mark style="background: #FF5582A6;">k-itemset</mark>: an itemset containing k items, denoted by $X = {x_{1},...,x_{k}}$, for example, ${apple, orange}$ is a 2-itemset.
-   Transaction: a non-empty itemset identified by a unique identifier in a database. Denoted by $T\in D$, where $D$ is a set of task-relevant transactions, for example, $D = {T_1, T_2}$, $T_1={apple, orange}$, $T_2={apple, banana, orange}$.
-   <mark style="background: #FF5582A6;">Support Count: the number of transactions that contain an itemset</mark>, denoted by $|T, \text{such that } A\subseteq T \text{ and } T\in D|$. Also known as occurrence frequency, frequency, absolute support, and count.
-   <mark style="background: #FF5582A6;">Support: the proportion of transactions that contain an itemset</mark>, denoted by $\text{support}(A) = \frac{\text{support count of }A \text{ in } D}{\text{cardinality of }D}$. Also called relative support and sometimes frequency.
-   <mark style="background: #FF5582A6;">Frequent Itemset</mark>: an itemset with support greater than or equal to a minimum support threshold.
-   Association Rule: an implication of the form $A \Longrightarrow B$, where $A\subset {\cal I}$, $B\subset {\cal I}$, $A\neq \emptyset$, $B\neq \emptyset$, and $A\cap B = \emptyset$.
-   <mark style="background: #BBFABBA6;">Rule Support</mark>: the proportion of transactions that contain all of both $A$'s and $B$'s items. Denoted by $P(A\cup B)$, it measures the significance of the association rule in the dataset.
-   <mark style="background: #FF5582A6;">Confidence</mark>: the conditional probability of $B$ given $A$, denoted by $P(B\vert A)$. Also called the support ratio, it is the support count of $A\cup B$ in $D$ as a proportion of the support count of $A$ in $D$.
-   <mark style="background: #BBFABBA6;">Strong Rule</mark>: an association rule $A \Longrightarrow B$ is strong when $A\cup B$ is frequent.


### [[4.3 Interesting pattern]]

-   Lift: a <mark style="background: #ADCCFFA6;">measure of dependence or correlation</mark> between two itemsets $A$ and $B$. If lift = 1, $A$ and $B$ are independent; if lift > 1, they are positively correlated; if lift < 1, they are negatively correlated.
$$\frac{\text{support}(A\cup B)}{\text{support}(A)\times \text{support}(B)}$$

-   Chi-square test: a hypothesis test used to <mark style="background: #ADCCFFA6;">determine if two nominal variables are related</mark>. The chi-square statistic is calculated by comparing observed and expected frequencies in a contingency table, and <mark style="background: #FFB86CA6;">degrees of freedom are (r-1)*(c-1)</mark>, where r and c are the number of rows and columns in the table, respectively. If the chi-square value is greater than the critical value at a given level of significance and the degrees of freedom, we reject the null hypothesis and conclude that there is a relationship between the two variables.

$$\chi^2 = \sum_{i=1}^{r}\sum_{j=1}^{c} \frac{(O_{ij} - E_{ij})^2}{E_{ij}}$$
-   Chi-square test for non-singleton itemsets: a hypothesis test used to determine if there is a significant association between two or more items in a transaction. The chi-square value is calculated by comparing observed and expected frequencies in a contingency table, and the expected frequency is calculated as (row total * column total) / grand total.

### [[4.4 Frequent Itemset Mining]]

- Find all frequent itemsets--i.e. all the itemsets that occur at least $\mathit{min_sup} \times \mathit{cardinality~~of~~dataset}$ times in the dataset.

- Use the frequent itemsets to generate strong association rules that satisfy $\mathit{min_conf}$.


### [[4.5 Apriori algorithm]]

1.  Start by scanning the database of transactions to determine the support of each item, i.e., the number of transactions in which it occurs.
2.  Generate a list of frequent <mark style="background: #FF5582A6;">1-itemsets</mark> by selecting those items whose support is greater than or equal to a predefined minimum support threshold.
3.  <mark style="background: #FF5582A6;">Use the frequent 1-itemsets to generate candidate 2-itemsets</mark>, i.e., pairs of items, by joining each frequent itemset with itself.
4.  Scan the database again to determine the support of each candidate 2-itemset and generate a list of frequent 2-itemsets by selecting those whose support is greater than or equal to the minimum support threshold.
5.  Use the frequent <mark style="background: #FF5582A6;">k-itemsets to generate candidate (k+1) itemsets </mark>by joining each frequent itemset with itself.
6.  Repeat steps 4 and 5 until no new frequent itemsets are generated, i.e., <mark style="background: #FF5582A6;">until the list of frequent k-itemsets is empty.</mark>
7.  <mark style="background: #FF5582A6;">Use the frequent itemsets to generate association rules</mark>, i.e., rules of the form A→B, where A and B are disjoint itemsets and the support and confidence of the rule are greater than or equal to predefined minimum thresholds.

### [[4.6 Generating Association rules]]

-   All frequent itemsets and their non-empty subsets meet the minimum support requirement.
-   So, only the confidence of the rule needs to be checked.
-   To create association rules using Apriori algorithm:
-   For each frequent itemset, generate non-empty proper subsets.
-   Check if the confidence of the rule is greater than or equal to the minimum confidence threshold.
-   Output the rule if it meets the threshold.


### [[4.7 Efficient frequent Data Mining]]

- An important optimization in frequent itemset mining algorithms based on the fact that <mark style="background: #FF5582A6;">if an itemset is frequent</mark>, then <mark style="background: #FF5582A6;">all of its non-empty subsets are also frequent</mark>. This allows for significant memory-saving in algorithms like Apriori.

### [[4.8 Closed and Maximal Frequent]]

-   Closed frequent itemsets and maximal frequent itemsets are used to efficiently mine frequent itemsets without losing important information.
-   An itemset X is considered <mark style="background: #FF5582A6;">closed</mark> in a dataset D if there is <mark style="background: #FF5582A6;">no</mark> other itemset Y in D that is a <mark style="background: #FF5582A6;">superset</mark> of X and has the <mark style="background: #FF5582A6;">same support count</mark> as X.
-   A closed frequent itemset is both closed and frequent, meaning it occurs in the dataset with a frequency that is at least the minimum support threshold.
-   A <mark style="background: #FF5582A6;">maximal frequent itemset</mark> is an itemset that is frequent in the dataset and has <mark style="background: #ADCCFFA6;">no superset that is also frequent, satisfying the minimum support threshold</mark>.
-   Closed frequent itemsets with their support counts represent all frequent itemsets in the dataset, including those that are not closed.
-   Maximal frequent itemsets represent all the frequent itemsets but may not give the exact counts for all of them.
-   Mining closed and maximal frequent itemsets can efficiently discover all frequent itemsets in a large dataset without generating redundant or unnecessary itemsets.
-   This approach significantly reduces the computational cost of frequent itemset mining while preserving the important information about the frequency of itemsets in the dataset.


### [[4.9 Adavanced pattern Mining]]

-   Algorithms other than Apriori have been developed to reduce database scans or reduce main memory usage, which may scale better over large datasets.
-   Some of the most well-known alternative algorithms are FP-growth and mining closed and max-patterns.
-   Some algorithms rely on representing transactions in a vertical form as <item, set of TIDs> instead of the horizontal form <TID, set of items>.
-   Advanced pattern mining methods have been developed to apply more structure over the input data and the rules produced, such as:
    -   Multilevel association rules: employing a concept hierarchy to discover rules at different levels of abstraction.
    -   Multidimensional associations: replacing items in itemsets by instantiated relational predicates to allow mined rules to become more expressive.
    -   Rare patterns and negative patterns: looking for patterns that are surprisingly infrequent or strongly negatively correlated by lift.
    -   Compressed or approximate patterns: looking for the top-k most frequent patterns or clustering patterns to stand for all the patterns in the cluster more efficiently.
    -   Sequential patterns: where the order of items is significant, for example in text or DNA sequences.
    -   Graph patterns: where frequent sub-graphs may be discovered in graph network structures.
-   Extended data types, such as multidimensional patterns over nominal data, ordinal data, and quantitative (continuous) data, require different mining approaches and interpretation of rules.
-   A simple solution to distinguish the attributes for single-dimensional algorithms like Apriori is to transform the values of nominal attributes to explicit attribute-value pairs.
-   Dynamic discretization is a more sophisticated approach that interacts with the mining algorithm to choose good value ranges.
-   Interpreting rules over transformed data requires being aware of the transformation and its impact on the evaluation metrics for the rules.



