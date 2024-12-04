
#### Definition of Data Modeling

- **Definition**: The process of discovering, analyzing, and determining data requirements, representing and communicating these requirements in a precise form called a data model. This process is iterative and may involve conceptual, logical, and physical models. 

#### Common Types of Data Models

- **Six Types**: Relational model, multidimensional model, object-oriented model, fact-based model, time-series model, and NoSQL model.
- Based on levels of detail, models can be categorized as conceptual, logical, and physical models. 

#### Business Drivers

1. Provide a common vocabulary for data.
2. Gather and document detailed information about data and systems within the organization.
3. Serve as a primary communication tool in projects.
4. Provide a starting point for application customization, integration, and even replacement.

#### Objectives of Data Modeling and Design

- To confirm and document an understanding of data requirements from different perspectives.
- Ensure that applications align better with current and future business needs.
- Lay the foundation for further data applications or management, such as master data management and data governance projects. 

#### Activities in Data Modeling and Design

1. Planning data modeling.
2. Creating data models (conceptual, logical, and physical).
3. Reviewing data models.
4. Maintaining data models. 

#### Inputs and Outputs

- **Inputs**: Existing data models and databases, data standards, data sets, initial and raw data requirements, data architecture, and enterprise taxonomies.
- **Outputs**: Conceptual, logical, and physical data models. 

#### Methods and Tools

- **Methods**: Naming conventions, database design standards, and database type selection.
- **Tools**:
    1. Data modeling tools.
    2. Data lineage tools.
    3. Data analysis tools.
    4. Metadata repositories (store descriptive information about data models).
    5. Data model patterns (elementary, assembly, and integration patterns).
    6. Industry data models. 

#### Benefits of Understanding Data Perspectives

1. **Formatting**: Defines structure concisely, preventing anomalies.
2. **Scope Definition**: Clarifies data context boundaries.
3. **Knowledge Retention**: Provides records for future understanding and evaluating changes' impacts. Models are reusable and help explore data structures within the environment.

#### Data Modeling in Practice

- Commonly used in system development and maintenance environments, also known as the system development lifecycle (SDLC).
- A model represents real-world entities or a desired creation pattern.
- Models can include diagrams to standardize symbols for rapid comprehension, such as maps, organizational charts, and blueprints.
- **Data Models**: Represent data that is understood or needed by the organization, using text-labeled symbols to visually convey data requirements. 

#### Components of a Data Model

1. **Entity**: A distinct thing of interest outside of data modeling. In data modeling, it is a carrier of information. Represented as rectangles, with the entity name in the center. Entities may be referred to as dimensions, fact tables, classes, or objects, depending on the modeling approach.
2. **Relationship**: An association between entities, represented as lines on a data model diagram. Cardinality (0, 1, many) and degree (unary, binary, ternary) describe these relationships. 
3. **Attribute**: Defines or measures an entity's properties. Attributes are listed within the entity rectangle in diagrams and appear as columns in physical implementations. 
4. **Domain**: Specifies all possible values for an attribute, standardizing its properties. Domains can be constrained by data types, formats, ranges, or rules. 


#### Keys in Data Models

1. **Identifiers**: Uniquely identify entity instances and can be single, composite, compound, or surrogate keys.
2. **Key Types**:
    - **Candidate Key**: Minimal set of attributes to uniquely identify an entity instance.
    - **Primary Key**: Chosen candidate key.
    - **Foreign Key**: Indicates relationships between tables in physical models.
    - **Alternate Key**: Candidate key not chosen as the primary key. 

#### Data Modeling Techniques

- **Relational Modeling**: Precisely expresses business data and eliminates redundancy, commonly used in operational systems.
- **Dimensional Modeling**: Optimizes data for querying and analysis, using axis notation to model relationships and dimensions.
- **Fact-Based Modeling**: Focuses on facts and relationships, with techniques like Object Role Modeling (ORM) and Fully Communication Oriented Information Modeling (FCO-IM).
- **Time-Based Modeling**: Deals with time-sensitive information, using techniques like Data Vault and Anchor Modeling.
- **NoSQL Modeling**: Tailored to document, key-value, column, and graph databases. 

#### Normalization and Denormalization

- **Normalization**: Reduces redundancy and inconsistencies by structuring data systematically (1NF, 2NF, 3NF, BCNF).
- **Denormalization**: Introduces controlled redundancy to improve query performance. 

#### Activities in Building Data Models

1. **Planning**: Define scope, assess needs, and determine storage management.
2. **Forward Engineering**: Conceptual → Logical → Physical models, capturing user perspectives and refining attributes.
3. **Reverse Engineering**: Document existing systems to understand their design.
4. **Review**: Validate model quality using techniques like scorecards.
5. **Maintenance**: Keep models updated and consistent with organizational changes. 

#### Data Model Quality Management

- Standards, version control, and best practices are essential. Metrics include completeness, consistency, and adherence to naming standards. Tools like scorecards evaluate model quality. 

