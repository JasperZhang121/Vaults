```java
/**  
 * A small collection of {@link Map}-related utility methods based on Java Stream APIs.  
 * * <p>Design goals:</p>  
 * <ul>  
 *   <li><b>Null-safe</b>: {@code null} lists and {@code null} elements are handled gracefully.</li>  
 *   <li><b>Deterministic merge</b>: when duplicate keys occur, the <b>first</b> value wins.</li>  
 *   <li><b>Common patterns</b>: indexing, grouping, partitioning, and "batch query then index" helpers.</li>  
 * </ul>  
 *  
 * <p><b>Notes</b>:</p>  
 * <ul>  
 *   <li>Methods in this class do not mutate input collections.</li>  
 *   <li>Returned maps are the standard Java map implementations produced by {@link Collectors}:  
 *       typically {@link java.util.HashMap} for {@code toMap()}, and {@link java.util.HashMap} for grouping/partitioning.</li>  
 *   <li>If you require key order preservation, consider using {@link java.util.LinkedHashMap} collectors instead.</li>  
 * </ul>  
 */  
public final class MapUtils {  
  
    /**  
     * Utility class; do not instantiate.     */    private MapUtils() {  
        throw new UnsupportedOperationException("Utility class");  
    }  
  
    /**  
     * Indexes values by keys derived from an input item list, using a batch query to load the values.     *     * <p>Typical use case:</p>  
     * <pre>{@code  
     * // items -> extract ids -> batch query -> index results by id  
     * Map<Long, UserProfile> profileByUserId = MapUtils.indexByKeys(  
     *     users,     *     User::getId,     *     userIds -> userProfileRepo.selectByUserIds(userIds),     *     UserProfile::getUserId     * );     * }</pre>  
     *  
     * <p>Algorithm:</p>  
     * <ol>  
     *   <li>Extract keys from {@code items} via {@code keyGetter}</li>  
     *   <li>Filter null keys, de-duplicate keys</li>  
     *   <li>Call {@code batchQuery} once with the distinct keys</li>  
     *   <li>Index returned values via {@code valueKeyGetter}</li>  
     * </ol>  
     *  
     * <p><b>Duplicate-key rule</b>: if {@code batchQuery} returns multiple values mapped to the same key,  
     * the first value wins.</p>  
     *  
     * <p><b>Null handling</b>:</p>  
     * <ul>  
     *   <li>If {@code items} is null/empty, returns {@link Collections#emptyMap()}.</li>  
     *   <li>Null elements in {@code items} are tolerated (keyGetter may receive null only if you pass nulls through; here we  
     *       don't filter items first, so prefer a null-safe keyGetter or ensure list doesn't contain nulls).</li>  
     *   <li>Null keys produced by {@code keyGetter} are ignored.</li>  
     *   <li>If {@code batchQuery} returns null, treated as empty list.</li>  
     *   <li>Null values returned by {@code batchQuery} are ignored.</li>  
     * </ul>  
     *  
     * @param items          source items used to extract distinct keys  
     * @param keyGetter      function to extract the query key from each item  
     * @param batchQuery     function to fetch values in batch by keys (e.g., DB query by IN clause)  
     * @param valueKeyGetter function to extract the index key from each returned value  
     * @param <T>            item type  
     * @param <K>            key type  
     * @param <V>            value type  
     * @return a map from key to value (first value wins on duplicate keys); empty map if no keys/values  
     */    public static <T, K, V> Map<K, V> indexByKeys(  
            List<T> items,  
            Function<T, K> keyGetter,  
            Function<List<K>, List<V>> batchQuery,  
            Function<V, K> valueKeyGetter  
    ) {  
        if (items == null || items.isEmpty()) return Collections.emptyMap();  
  
        // Extract distinct non-null keys from items.  
        List<K> keys = items.stream()  
                .map(keyGetter)  
                .filter(Objects::nonNull)  
                .distinct()  
                .collect(Collectors.toList());  
        if (keys.isEmpty()) return Collections.emptyMap();  
  
        // Batch query once, then index the returned values by their key.  
        // Merge policy: keep the first encountered value when duplicate keys exist.        return Optional.ofNullable(batchQuery.apply(keys))  
                .orElseGet(Collections::emptyList)  
                .stream()  
                .filter(Objects::nonNull)  
                .collect(Collectors.toMap(valueKeyGetter, Function.identity(), (a, b) -> a));  
    }  
  
    /**  
     * Builds an index map from a list by a derived key.     *     * <p><b>Duplicate-key rule</b>: the first element wins.</p>  
     *  
     * <p><b>Null handling</b>:</p>  
     * <ul>  
     *   <li>If {@code list} is null, treated as empty.</li>  
     *   <li>Null elements are ignored.</li>  
     * </ul>  
     *  
     * @param list  input list  
     * @param keyFn function to derive the map key from each element  
     * @param <T>   element type  
     * @param <K>   key type  
     * @return map indexed by {@code keyFn}; empty map if list is null/empty  
     */    public static <T, K> Map<K, T> indexBy(List<T> list, Function<T, K> keyFn) {  
        return Optional.ofNullable(list).orElseGet(Collections::emptyList).stream()  
                .filter(Objects::nonNull)  
                .collect(Collectors.toMap(keyFn, Function.identity(), (a, b) -> a));  
    }  
  
    /**  
     * Groups elements in a list by a derived key.     *     * <p><b>Null handling</b>:</p>  
     * <ul>  
     *   <li>If {@code list} is null, treated as empty.</li>  
     *   <li>Null elements are ignored.</li>  
     * </ul>  
     *  
     * @param list  input list  
     * @param keyFn function to derive the group key  
     * @param <T>   element type  
     * @param <K>   key type  
     * @return map from key to list of elements in that group; empty map if list is null/empty  
     */    public static <T, K> Map<K, List<T>> groupBy(List<T> list, Function<T, K> keyFn) {  
        return Optional.ofNullable(list).orElseGet(Collections::emptyList).stream()  
                .filter(Objects::nonNull)  
                .collect(Collectors.groupingBy(keyFn));  
    }  
  
    /**  
     * Partitions elements in a list into two groups based on a predicate:     * {@code true} group and {@code false} group.  
     *     * <p><b>Null handling</b>:</p>  
     * <ul>  
     *   <li>If {@code list} is null, treated as empty.</li>  
     *   <li>Null elements are ignored.</li>  
     * </ul>  
     *  
     * @param list input list  
     * @param pred predicate used to decide the partition bucket  
     * @param <T>  element type  
     * @return a map with keys {@code true} and {@code false}, each mapping to a list (possibly empty)  
     */    public static <T> Map<Boolean, List<T>> partitionBy(List<T> list, Predicate<T> pred) {  
        return Optional.ofNullable(list).orElseGet(Collections::emptyList).stream()  
                .filter(Objects::nonNull)  
                .collect(Collectors.partitioningBy(pred));  
    }  
}
```