---
tags: [java, collections, jdk, map, list, set]
---
# COLLECTIONS

*Last update: 2019*

### JDK maps

Java 7 and before

    int size();
    boolean isEmpty();
    boolean containsKey(Object key);
    boolean containsValue(Object value);
    V get(Object key);
    V put(K key, V value);
    V remove(Object key);
    void putAll(Map<? extends K, ? extends V> m);
    void clear();
    Set<K> keySet();
    Collection<V> values();
    Set<Map.Entry<K, V>> entrySet();
    // Comparison and hashing
    boolean equals(Object o);
    int hashCode();


JAVA 8

    void forEach(BiConsumer<? super K, ? super V> action);
    // Loop on all entries
    // Example:  map.forEach((k, v) -> System.out.println("Key=" + k + " - value=" + v));


    void replaceAll(BiFunction<? super K, ? super V, ? extends V> function)
    // In-place update of the value for all keys
    // Example: map.replaceAll((k, v) -> v == null ? "default" : v);


    V putIfAbsent(K key, V value)
    // If the key exists and its value is not null, do nothing
    // If the key exists and its value is null, replace the value in the map
    // If the key does not exist, add the key-value entry in the map
    // Returns the value in the map

    boolean remove(Object key, Object value)
    // If the key-value entry is present in the map, remove them and return true
    // Otherwise returns false.


    boolean replace(K key, V oldValue, V newValue)
     // If the key-oldValue entry is present in the map, replace its value with the newValue and return true
    // Otherwise returns false.

    V replace(K key, V value)
    // If the key exists in the map, replace its value with the new value
    // Returns the value in the map

    V computeIfAbsent(K key, Function<? super K, ? extends V> mappingFunction)
    // If the key is absent or its value is null, use the mappingFunction to calculate the new value and save it in the map
    // Returns the value in the map
    // Example: map.computeIfAbsent(key, k -> new HashSet<V>()).add(v)


    V computeIfPresent(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)
    // If the key is present and its value is not null, use the remappingFunction to calculate the new value and save it in the map
    // if the remappingFunction value is null, the entry is removed from the map
    // Returns the value in the map
    // Example 1:
    // I've used computeIfPresent as a null-safe way to fetch lowercase values from a map of strings.
    // String s = fields.computeIfPresent("foo", (k,v) -> v.toLowerCase())
    // Example 2:
    // Map<String, Collection<String>> strings = new HashMap<>();
    // void addString(String a) {
    //    String index = a.substring(0, 1);
    //    strings.computeIfAbsent(index, ign -> new HashSet<>()).add(a);
    // }
    // void removeString(String a) {
    //    String index = a.substring(0, 1);
    //    strings.computeIfPresent(index, (i, c) -> c.remove(a) && c.isEmpty() ? null : c);
    //}

    V compute(K key, BiFunction<? super K, ? super V, ? extends V> remappingFunction)
    // Use the remappingFunction to calculate the new value and save it in the map if the value is not null
    // The original entry could be absent
    // Returns the value in the map
    // The operation is ATOMIC in the ConcurrentHaspMap
    // Example1: map.compute(key, (k, v) -> (v == null) ? msg : v.concat(msg))}
    // Example2:  ConcurrentMap<String, Integer> map = new ConcurrentHashMap<>();
    //            map.compute("apple", (key, value) -> value == null ? 1 : value + 1);
    // Example3: ConcurrentMap<String, LongAdder> map2 = new ConcurrentHashMap<>();
    //           map2.computeIfAbsent("apple", key -> new LongAdder()).increment();

    V merge(K key, V value, BiFunction<? super V, ? super V, ? extends V> remappingFunction)
    // If the key is absent or its value is null, save key and value in the map.
    // If the key is present and its value is not null, use the remappingFunction to calculate the new value and save it in the map
    // If the remappingFunction returns null, the entry is removed from the map
    // Returns the value in the map or null


JAVA 9

    Map<K, V> of()
    // Returns an empty and immutable map

    Map<K, V> of(K k1, V v1.... k10, v10)
    // Returns an immutable map from the max 10 key-value tuples

    Map<K, V> ofEntries(Entry<? extends K, ? extends V>... entries)
    // Returns an immutable map from the input entries.


JAVA 10

    Map<K, V> copyOf(Map<? extends K, ? extends V> map)
    // Returns an immutable copy of the input map. Key and values cannot be null.


### JDK java.util.Collection

Java 7 and before

    int size();
    boolean isEmpty();
    boolean contains(Object o);
    Iterator<E> iterator();
    Object[] toArray();
    <T> T[] toArray(T[] a);
    boolean add(E e);
    boolean remove(Object o);
    boolean containsAll(Collection<?> c);
    boolean addAll(Collection<? extends E> c);
    boolean removeAll(Collection<?> c);
    boolean retainAll(Collection<?> c);
    void clear();
    boolean equals(Object o);
    int hashCode();


JAVA 8

    boolean removeIf(Predicate<? super E> filter)
    // Replaces all elements in the collection for whom the filter is true.

    Spliterator<E> spliterator()
    // creates a spliterator from the collection

    Stream<E> stream()
    // Creates a stream from the collection

    Stream<E> parallelStream()
    // Creates a parallel stream from the collection


JAVA 11

    T[] toArray(IntFunction<T[]> generator)


### JDK sets

Java 7 e precedenti

    int size();
    boolean isEmpty();
    boolean contains(Object o);
    Iterator<E> iterator();
    Object[] toArray();
    <T> T[] toArray(T[] a);
    boolean add(E e);
    boolean remove(Object o);
    boolean containsAll(Collection<?> c);
    boolean addAll(Collection<? extends E> c);
    boolean retainAll(Collection<?> c);
    boolean removeAll(Collection<?> c);
    void clear();
    boolean equals(Object o);
    int hashCode();

JAVA 8

    Spliterator<E> spliterator()
    // Creates a spliterator from the set


Java 9

    Set<E> of()
    // Returns an empty and immutable set

    Set<E> of(v1   ....  v10)
    // Creates an immutable set with max 10 not-null input values

    Set<E> of(E... elements)
    // Creates an immutable set with the not-null input values


Java 10

    <E> Set<E> copyOf(Collection<? extends E> coll)
    // Creates an immutable set with a copy of the values included in the `coll` collection



### JDK lists

Java 7 e precedenti

    int size();
    boolean isEmpty();
    boolean contains(Object o);
    Iterator<E> iterator();
    Object[] toArray();
    <T> T[] toArray(T[] a);
    boolean add(E e);
    boolean remove(Object o);
    boolean containsAll(Collection<?> c);
    boolean addAll(Collection<? extends E> c);
    boolean addAll(int index, Collection<? extends E> c);
    boolean removeAll(Collection<?> c);
    boolean retainAll(Collection<?> c);
    void clear();
    boolean equals(Object o);
    int hashCode();
    E get(int index);
    E set(int index, E element);
    void add(int index, E element);
    E remove(int index);
    int indexOf(Object o);
    int lastIndexOf(Object o);
    ListIterator<E> listIterator();
    ListIterator<E> listIterator(int index);
    List<E> subList(int fromIndex, int toIndex);


JAVA 8

    void replaceAll(UnaryOperator<E> operator)
    // Apply the operator to all elements in the list. In-place change
    // Example: lista.replaceAll(Strings::trim)
    // Example: employeeList.replaceAll(employee -> {employee.setSalary(employee.getSalary() * 1.1);

    sort(Comparator<? super E> c)
    // Order the collection using the input comparator
    // Example: employeeList.sort((emp1, emp2)-> Double.compare(emp1.getSalary(),emp2.getSalary()));

    Spliterator<E> spliterator()
    // Creates a sliterator from the list


JAVA 9
    
    List<E> of()
    // Creates an empty and immutable list


    List<E> of(e1 ...  e10)
    // Creates an immutable list with the max 10 not-null input values


    List<E> of(E... elements)
    // Creates an immutable list with the not-null input values


Java 10

    <E> List<E> copyOf(Collection<? extends E> coll)
    // Creates an immutable list with a copy of the values included in the `coll` collection

