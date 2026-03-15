
This example illustrates **nested functional calling** in Java through a compact, academic-style demonstration. The purpose is to show <mark style="background: #FFB8EBA6;">how a piece of business logic, expressed as a lambda, can be passed through multiple layers of infrastructure code before it is finally executed</mark>. This pattern is common in enterprise Java systems, especially where transaction management, retry control, and query execution are separated from business logic.

At the top level, the method getLatestWorkflowDef(String name) represents a service-style business method. It defines a SQL statement and supplies a lambda that describes how the query result should be processed. However, this lambda is not executed immediately. Instead, it is passed into a sequence of wrapper methods, each of which contributes one layer of behavior.

The execution begins in getLatestWorkflowDef, which calls queryWithTransaction. This method introduces a transaction-aware wrapper and delegates further execution to getWithRetriedTransactions. The latter adds retry semantics by passing the transactional operation into retryTemplate, which repeatedly attempts execution until success or until the retry limit is reached. Inside that retry process, getWithTransaction simulates opening a database connection, executing the operation within a transaction, and either committing or rolling back depending on whether an exception occurs. Finally, the query method creates a mock query object and invokes the original lambda.
  
Conceptually, the call chain is as follows:

```
getLatestWorkflowDef
    -> queryWithTransaction
        -> getWithRetriedTransactions
            -> retryTemplate
                -> getWithTransaction
                    -> query
                        -> lambda body executes
```

This structure demonstrates an important principle: the business logic is written once at the outer layer, while the inner layers transparently provide technical concerns such as retries and transaction handling. As a result, the design achieves a clear separation of concerns. The business method focuses only on what result should be obtained, while the lower-level wrappers determine how that work is safely executed.

The lambda passed from getLatestWorkflowDef is therefore a form of **deferred computation**. It is defined in one place, <mark style="background: #FFB86CA6;">transported through several method calls as a function object, and executed only when the innermost layer has fully established the execution context.</mark> This is the essential feature of nested functional calling.

A simplified version of the core business method can be understood as follows:

```java
public Optional<String> getLatestWorkflowDef(String name) {
    final String sql = "SELECT data FROM workflow WHERE name = ?";

    return Optional.ofNullable(
            queryWithTransaction(
                    sql,
                    query -> {
                        System.out.println("Executing query for workflow: " + name);
                        return "Workflow definition data: " + name;
                    }
            )
    );
}
```

Here, the lambda query -> { ... } contains the actual business action. The surrounding methods do not know the business meaning of the operation; they only know how to wrap and execute it. <mark style="background: #BBFABBA6;">This is characteristic of higher-order programming, where functions are passed as arguments to other functions</mark>.

The output sequence of the demo is also pedagogically useful, because it reveals the true runtime order. Although the lambda is written near the top of the program, its output appears only after the retry and transaction layers have been entered. This makes the deferred nature of execution explicit.

In academic terms, this example demonstrates three important ideas. First, it shows the use of **higher-order functions** in Java through functional interfaces and lambdas. Second, it illustrates **separation of concerns**, since business behavior is isolated from infrastructure behavior. Third, it models a simplified form of **framework-style execution flow**, similar to what happens internally in transaction templates, retry frameworks, and persistence libraries.

The design is intentionally minimal and does not represent a real database implementation. The transaction object is only a placeholder, the query class is a mock abstraction, and the retry logic is simplified. Nevertheless, the example is effective as a conceptual model. It captures how enterprise frameworks often allow developers to provide only the essential business operation, while the framework itself manages execution policies around that operation.

In summary, this demo is a compact illustration of nested functional calling in Java. A lambda representing business logic is passed through successive wrapper layers for retry and transaction management, and is executed only at the deepest level. The example therefore provides a clear academic demonstration of deferred execution, higher-order functions, and layered responsibility in Java design.

Code:
```java
import java.util.Optional;
import java.util.function.Supplier;

public class NestedFunctionDemo {

    public static void main(String[] args) {
        NestedFunctionDemo demo = new NestedFunctionDemo();

        Optional<String> result = demo.getLatestWorkflowDef("testWorkflow");
        System.out.println("Result: " + result.orElse("Not found"));
    }

    // Top-level business method
    public Optional<String> getLatestWorkflowDef(String name) {
        final String sql = "SELECT data FROM workflow WHERE name = ?";

        return Optional.ofNullable(
                queryWithTransaction(
                        sql,
                        query -> {
                            System.out.println("Executing query for workflow: " + name);
                            return "Workflow definition data: " + name;
                        }
                )
        );
    }

    // First wrapper layer: transaction-aware query entry
    protected <R> R queryWithTransaction(String query, QueryFunction<R> function) {
        System.out.println("1. Starting transaction-aware query: " + query);
        return getWithRetriedTransactions(tx -> {
            System.out.println("2. Executing query within transaction");
            return query(tx, query, function);
        });
    }

    // Second wrapper layer: retry logic
    <R> R getWithRetriedTransactions(TransactionalFunction<R> function) {
        System.out.println("3. Entering retry logic");
        try {
            return retryTemplate(() -> getWithTransaction(function));
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    // Simulated retry template
    private <R> R retryTemplate(Supplier<R> supplier) {
        int maxRetries = 3;
        for (int i = 0; i < maxRetries; i++) {
            try {
                System.out.println("4. Retry attempt: " + (i + 1));
                return supplier.get();
            } catch (Exception e) {
                if (i == maxRetries - 1) {
                    throw e;
                }
                System.out.println("Retrying...");
            }
        }
        throw new RuntimeException("Retry failed");
    }

    // Third wrapper layer: transaction management
    private <R> R getWithTransaction(TransactionalFunction<R> function) {
        System.out.println("5. Opening database connection");
        Object tx = new Object(); // Simulated transaction/connection
        try {
            R result = function.apply(tx);
            System.out.println("6. Committing transaction");
            return result;
        } catch (Exception e) {
            System.out.println("7. Rolling back transaction");
            throw new RuntimeException(e);
        }
    }

    // Simulated query execution
    private <R> R query(Object tx, String query, QueryFunction<R> function) {
        System.out.println("8. Running SQL: " + query);
        Query q = new Query();
        return function.apply(q);
    }

    @FunctionalInterface
    interface QueryFunction<R> {
        R apply(Query q);
    }

    @FunctionalInterface
    interface TransactionalFunction<R> {
        R apply(Object tx);
    }

    static class Query {
        public <T> T executeAndFetchFirst(Class<T> clazz) {
            return null;
        }
    }
}
```