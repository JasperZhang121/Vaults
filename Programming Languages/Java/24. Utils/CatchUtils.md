```java
/**  
 * Small "catch & wrap" helpers for validation and exception-safe execution. * * <p>Design goals:</p>  
 * <ul>  
 *   <li><b>Low ceremony</b>: avoid noisy try/catch blocks in business code.</li>  
 *   <li><b>Null-safe</b>: handle null collections / null returns gracefully.</li>  
 *   <li><b>Explicit intent</b>: return {@link Optional} / fallback values / errors list.</li>  
 * </ul>  
 *  
 * <p><b>Notes</b>:</p>  
 * <ul>  
 *   <li>By default, these methods catch {@link Exception} (not {@link Error}).</li>  
 *   <li>If you need to differentiate exception types, prefer catching explicitly in caller code.</li>  
 * </ul>  
 */  
public final class CatchUtils {  
  
    private CatchUtils() {  
        throw new UnsupportedOperationException("Utility class");  
    }  
  
    /**  
     * Validates an object with a list of rules. Each rule returns an {@link Optional} error message.  
     *     * <p>Convention: {@code Optional.empty()} means "pass", {@code Optional.of(msg)} means "fail".</p>  
     *  
     * @param obj   object to validate  
     * @param rules validation rules; null treated as empty  
     * @param <T>   object type  
     * @return list of error messages (empty if all pass)  
     */    public static <T> List<String> validate(T obj, List<Function<T, Optional<String>>> rules) {  
        return rules.stream()  
                .map(r -> r.apply(obj))  
                .filter(Optional::isPresent)  
                .map(Optional::get)  
                .collect(Collectors.toList());  
    }  
  
    /**  
     * Executes supplier and returns {@link Optional#empty()} if any exception occurs.  
     */    public static <T> Optional<T> tryGet(Supplier<T> supplier) {  
        try {  
            return Optional.ofNullable(supplier.get());  
        } catch (Exception e) {  
            return Optional.empty();  
        }  
    }  
  
    /**  
     * Executes supplier and returns a fallback value if any exception occurs.     *     * @param supplier      value supplier  
     * @param defaultValue  fallback value when supplier throws or returns null  
     */    public static <T> T tryGetOrDefault(Supplier<T> supplier, T defaultValue) {  
        try {  
            T v = supplier.get();  
            return v != null ? v : defaultValue;  
        } catch (Exception e) {  
            return defaultValue;  
        }  
    }  
  
    /**  
     * Executes supplier and returns a fallback from {@code defaultSupplier} if any exception occurs.  
     */    public static <T> T tryGetOrElse(Supplier<T> supplier, Supplier<T> defaultSupplier) {  
        try {  
            T v = supplier.get();  
            if (v != null) return v;  
        } catch (Exception ignored) {  
            // fall through  
        }  
        return defaultSupplier == null ? null : defaultSupplier.get();  
    }  
  
    /**  
     * Executes supplier, returning an Optional, and if exception occurs,     * passes it to {@code onError} (if not null).  
     */    public static <T> Optional<T> tryGet(Supplier<T> supplier, Consumer<Exception> onError) {  
        try {  
            return Optional.ofNullable(supplier.get());  
        } catch (Exception e) {  
            if (onError != null) onError.accept(e);  
            return Optional.empty();  
        }  
    }  
  
    /**  
     * Executes a runnable, suppressing any exception.     */    public static void tryRun(Runnable runnable) {  
        try {  
            runnable.run();  
        } catch (Exception ignored) {  
            // intentionally ignored  
        }  
    }  
  
    /**  
     * Executes a runnable, and if exception occurs, passes it to {@code onError}.  
     */    public static void tryRun(Runnable runnable, Consumer<Exception> onError) {  
        try {  
            runnable.run();  
        } catch (Exception e) {  
            if (onError != null) onError.accept(e);  
        }  
    }  
  
    /**  
     * Executes a callable, returning Optional.empty() on exception.     * Useful if your supplier throws checked exceptions.     */    public static <T> Optional<T> tryCall(Callable<T> callable) {  
        try {  
            return Optional.ofNullable(callable.call());  
        } catch (Exception e) {  
            return Optional.empty();  
        }  
    }  
  
    /**  
     * Executes a callable, returning a fallback value on exception.     * Useful if your callable throws checked exceptions.     */    public static <T> T tryCallOrDefault(Callable<T> callable, T defaultValue) {  
        try {  
            T v = callable.call();  
            return v != null ? v : defaultValue;  
        } catch (Exception e) {  
            return defaultValue;  
        }  
    }  
  
    /**  
     * Converts an exception-throwing callable into a safe supplier:     * returns Optional.empty() on exception.     */    public static <T> Supplier<Optional<T>> safe(Callable<T> callable) {  
        return () -> tryCall(callable);  
    }  
  
    /**  
     * Converts an exception-throwing supplier into a safe supplier:     * returns Optional.empty() on exception.     */    public static <T> Supplier<Optional<T>> safe(Supplier<T> supplier) {  
        return () -> tryGet(supplier);  
    }  
  
    /**  
     * Wraps a function call and returns Optional.empty() on exception.     */    public static <A, R> Optional<R> tryApply(Function<A, R> fn, A arg) {  
        try {  
            return Optional.ofNullable(fn.apply(arg));  
        } catch (Exception e) {  
            return Optional.empty();  
        }  
    }  
  
    /**  
     * Wraps a function call and returns fallback value on exception (or null result).     */    public static <A, R> R tryApplyOrDefault(Function<A, R> fn, A arg, R defaultValue) {  
        try {  
            R r = fn.apply(arg);  
            return r != null ? r : defaultValue;  
        } catch (Exception e) {  
            return defaultValue;  
        }  
    }  
  
    /**  
     * Filters a list by predicate, suppressing predicate exceptions per element.     * If predicate throws for an element, that element is treated as "false" and excluded.     */    public static <T> List<T> safeFilter(List<T> list, Predicate<T> predicate) {  
        if (list == null || list.isEmpty()) return Collections.emptyList();  
        if (predicate == null) return list.stream().filter(Objects::nonNull).collect(Collectors.toList());  
  
        return list.stream()  
                .filter(Objects::nonNull)  
                .filter(t -> {  
                    try {  
                        return predicate.test(t);  
                    } catch (Exception e) {  
                        return false;  
                    }  
                })  
                .collect(Collectors.toList());  
    }  
}
```