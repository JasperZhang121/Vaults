
In modern business process management (BPM) systems, the ability to programmatically retrieve and filter process models is essential for both runtime operations and administrative tooling. Flowable’s `ModelQuery` interface provides a fluent, type-safe API for constructing complex queries against the model repository. This document offers an academic overview of its design, implementation, and practical usage patterns.

#### 1. Introduction

A _model_ in Flowable represents a BPMN, CMMN, or DMN artifact that has been created or deployed to the Flowable Repository Service. Efficient querying of these models is crucial for:

1. **Dynamic UI rendering**, where only certain models should be displayed.
    
2. **Automated governance**, such as ensuring only the latest approved versions are executed.
    
3. **Multi-tenant isolation**, enforcing organizational boundaries.

Flowable addresses these requirements via its `ModelQuery` API, which extends a generic `Query<T,Q>` interface, enabling method chaining and compile-time safety.

#### 2. Interface Definition

```java
public interface ModelQuery 
    extends Query<ModelQuery, Model> {
    
    // Identification filters
    ModelQuery modelId(String id);
    ModelQuery modelKey(String key);

    // Name and category filters
    ModelQuery modelName(String name);
    ModelQuery modelNameLike(String expression);
    ModelQuery modelCategory(String category);
    ModelQuery modelCategoryLike(String expression);
    ModelQuery modelCategoryNotEquals(String category);

    // Versioning and deployment
    ModelQuery modelVersion(Integer version);
    ModelQuery latestVersion();
    ModelQuery deploymentId(String deploymentId);
    ModelQuery deployed();
    ModelQuery notDeployed();

    // Tenant isolation
    ModelQuery modelTenantId(String tenantId);
    ModelQuery modelTenantIdLike(String expression);
    ModelQuery modelWithoutTenantId();

    // Ordering clauses
    ModelQuery orderByModelCategory();
    ModelQuery orderByModelId();
    ModelQuery orderByModelKey();
    ModelQuery orderByModelVersion();
    ModelQuery orderByModelName();
    ModelQuery orderByCreateTime();
    ModelQuery orderByLastUpdateTime();
    ModelQuery orderByTenantId();
}
```

- Each `modelXxx(...)` or `orderByXxx()` method returns the same `ModelQuery` instance, enabling chained invocations.
    
- Query execution is triggered by terminal operations:
    
    - `.list()` → returns `List<Model>`
        
    - `.singleResult()` → returns a single `Model` (or throws)
        
    - `.count()` → returns `long`

#### 3. Implementation Internals

The concrete class, `ModelQueryImpl`, extends `AbstractQuery<ModelQuery,Model>` and encapsulates state in private fields:

```java
protected String id, key, name, nameLike;
protected String category, categoryLike, categoryNotEquals;
protected Integer version;
protected boolean latest, deployed, notDeployed;
protected String deploymentId;
protected String tenantId, tenantIdLike;
protected boolean withoutTenantId;
```

- **Setter methods** (e.g. `modelName(String)`) perform null checks and assign the corresponding field.
    
- **Mutual‐exclusion** is enforced—calling both `deployed()` and `notDeployed()` results in an immediate `FlowableIllegalArgumentException`.
    
- **Execution** delegates to `ModelEntityManager` via:
    
    - `executeCount()` → `findModelCountByQueryCriteria(this)`
        
    - `executeList()` → `findModelsByQueryCriteria(this)`


The SQL generated resembles:

```sql
SELECT * 
  FROM ACT_RE_MODEL 
 WHERE [conditions based on non-null fields]
 ORDER BY [property from orderByXxx()]
```


#### 4. Typical Usage Patterns

##### 4.1 Simple Retrieval

Retrieve all models with exact name match:

```java
List<Model> exact = repositoryService
    .createModelQuery()
    .modelName("PurchaseOrder")
    .list();
```

##### 4.2 Pattern Matching & Latest Versions

Fetch the latest version of all “invoice” models:

```java
List<Model> invoices = repositoryService
    .createModelQuery()
    .modelCategoryLike("%invoice%")
    .latestVersion()
    .list();
```

##### 4.3 Tenant-Scoped Queries

In a multi-tenant environment, limit results to a given tenant and sort by update time:

```java
List<Model> tenantModels = repositoryService
    .createModelQuery()
    .modelTenantId("orgA")
    .orderByLastUpdateTime().desc()
    .list();
```

##### 4.4 Pagination

Implementing page-by-page display in a UI:

```java
int pageSize = 20, pageNumber = 2;
List<Model> page = repositoryService
    .createModelQuery()
    .orderByModelName().asc()
    .listPage((pageNumber - 1) * pageSize, pageSize);
```

##### 4.5 Deployed vs. Draft Models

Distinguish between models that have been deployed and those still in draft:

```java
long draftCount = repositoryService
    .createModelQuery()
    .notDeployed()
    .count();

long liveCount  = repositoryService
    .createModelQuery()
    .deployed()
    .count();
```


#### 5. Discussion

- **Fluent Interface**: Enhances readability and reduces boilerplate when constructing complex criteria.
    
- **Type-Safety**: Chained calls are validated at compile time, preventing SQL-injection risks inherent in hand-rolled queries.
    
- **Extensibility**: New filters or ordering options can be added by extending `ModelQueryImpl` and its property enumeration (`ModelQueryProperty`).


#### 6. Conclusion

Flowable’s `ModelQuery` exemplifies a robust, academic-grade API for BPM model retrieval. Through clear separation of concerns—filter definition, validation, and execution—it supports sophisticated repository operations while maintaining high readability and maintainability in enterprise environments.