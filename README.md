# 📚 Java Streams Guide

## 🧵 What Is a Stream?

A **Stream** in Java is a sequence of elements supporting functional-style operations. It operates on a source (like arrays or collections) and enables efficient, readable data processing pipelines.

### 🔍 Key Characteristics:
- Streams do **not** store data
- Operations are **lazy** until a terminal operation is invoked
- Streams can be processed **sequentially** or in **parallel**

---

## 🌊 Creating Streams

1️⃣ From a Collection
```java
List<String> list = Arrays.asList("a", "b", "c");
list.stream().forEach(System.out::println);
```
2️⃣ From an Array
```java
String[] array = {"a", "b", "c"};
Stream.of(array).forEach(System.out::println);
```
3️⃣ From Individual Values
```java
Stream.of("a", "b", "c").forEach(System.out::println);
```
🔄 Intermediate Operations Overview

Intermediate operations are those that transform or filter a stream and return a new stream. They are called intermediate because they can be chained together to form a pipeline of transformations. Since they are lazy, they are not executed until a terminal operation is called.

Common Intermediate Operations

1. `filter`: Filters elements based on a predicate.
2. `map`: Transforms each element by applying a function.
3. `flatMap`: Transforms each element into a stream and flattens the result.
4. `distinct`: Removes duplicate elements.
5. `sorted`: Sorts the elements in a natural order or using a comparator.
6. `peek`: Performs an action on each element without modifying the stream.
7. `limit`: Limits the number of elements.
8. `skip`: Skips the first N elements.

Detailed Examples
1. `filter`

The `filter` method allows you to include only elements that match a given predicate.

```java
public class FilterExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry", "date");
        
        List<String> filteredWords = words.stream()
                                          .filter(word -> word.startsWith("a"))
                                          .collect(Collectors.toList());

        System.out.println(filteredWords); // Output: [apple]
    }}
```
2. `map`

The `map` method transforms each element by applying a function to it.
```java
public class MapExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry");

        List<Integer> lengths = words.stream()
                                     .map(String::length)
                                     .collect(Collectors.toList());

        System.out.println(lengths); // Output: [5, 6, 6]
    }
}
```
What is difference between map and filter ?
Purpose

- `map`: The `map` operation is used to transform each element of the stream into another form.
- It applies a given function to each element and produces a new stream of the transformed elements.

- `filter`: The `filter` operation is used to include only those elements that satisfy a given predicate.
- It evaluates each element of the stream and returns a new stream that consists only of elements that match the predicate.

Function Signature

- `map`: The function passed to `map` is a `Function<T, R>`, which takes an element of type `T` and returns an element of type `R`.

<R> Stream<R> map(Function<? super T, ? extends R> mapper)
- `filter`: The function passed to `filter` is a `Predicate<T>`, which takes an element of type `T` and returns a boolean value.

Stream<T> filter(Predicate<? super T> predicate)
Transformation vs. Filtering

- `map: Transforms elements. For example, converting a list of strings to a list of their lengths.
```java
List<String> words = Arrays.asList("apple", "banana", "cherry");
List<Integer> lengths = words.stream()
                             .map(String::length)
                             .collect(Collectors.toList());
// Output: [5, 6, 6]
```
- `filter`: Filters elements based on a condition. For example, filtering out strings that don’t start with the letter “a”.
```java
List<String> words = Arrays.asList("apple", "banana", "cherry");
List<String> filteredWords = words.stream()
                                  .filter(word -> word.startsWith("a"))
                                  .collect(Collectors.toList());
// Output: [apple]
```
Usage in Combination

You can often use `map` and `filter` together in a stream pipeline to perform complex data processing. For example, filtering a list of strings to include only those that start with “a” and then transforming them to uppercase.
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class MapFilterExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry", "avocado");

        List<String> result = words.stream()
                                   .filter(word -> word.startsWith("a")) // filter step
                                   .map(String::toUpperCase)             // map step
                                   .collect(Collectors.toList());

        System.out.println(result); // Output: [APPLE, AVOCADO]
    }
}
```
3. `flatMap`
The `flatMap` method is used to transform each element into a stream and then flatten the resulting streams into a single stream.
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class FlatMapExample {
    public static void main(String[] args) {
        List<List<String>> nestedList = Arrays.asList(
            Arrays.asList("apple", "banana"),
            Arrays.asList("cherry", "date")
        );

        List<String> flatList = nestedList.stream()
                                          .flatMap(List::stream)
                                          .collect(Collectors.toList());

        System.out.println(flatList); // Output: [apple, banana, cherry, date]
    }
}
```
4. `distinct`

The `distinct` method removes duplicate elements from the stream.
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class DistinctExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "apple", "cherry");

        List<String> distinctWords = words.stream()
                                          .distinct()
                                          .collect(Collectors.toList());

        System.out.println(distinctWords); // Output: [apple, banana, cherry]
    }
}
```
5. `sorted`

The `sorted` method sorts the elements in their natural order or by a custom comparator.
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class SortedExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("banana", "apple", "cherry");

        List<String> sortedWords = words.stream()
                                        .sorted()
                                        .collect(Collectors.toList());

        System.out.println(sortedWords); // Output: [apple, banana, cherry]
    }
}
```
6. `peek`

The `peek` method allows you to perform an action on each element without modifying the stream.
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class PeekExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry");

        List<String> result = words.stream()
                                   .peek(word -> System.out.println("Processing: " + word))
                                   .collect(Collectors.toList());

        System.out.println(result); // Output: [apple, banana, cherry]
        // Additionally, "Processing: apple", "Processing: banana", and "Processing: cherry" will be printed
    }
}
```
7. `limit`

The `limit` method restricts the number of elements in the stream to a given maximum size.
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class LimitExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry", "date");

        List<String> limitedWords = words.stream()
                                         .limit(2)
                                         .collect(Collectors.toList());

        System.out.println(limitedWords); // Output: [apple, banana]
    }
}
```
8. `skip`

The `skip` method discards the first N elements of the stream.
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class SkipExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry", "date");

        List<String> skippedWords = words.stream()
                                         .skip(2)
                                         .collect(Collectors.toList());

        System.out.println(skippedWords); // Output: [cherry, date]
    }
}
```
Combining Intermediate Operations

You can chain multiple intermediate operations together to form a processing pipeline.
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class CombinedExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry", "date");

        List<String> result = words.stream()
                                   .filter(word -> word.length() > 5)
                                   .map(String::toUpperCase)
                                   .sorted()
                                   .collect(Collectors.toList());

        System.out.println(result); // Output: [BANANA, CHERRY]
    }
}
```
Terminal Operations
What Are Terminal Operations?

Terminal operations consume the stream and produce a result or a side effect, such as a collection, a single value, or an action on each element. They are the endpoint of a stream pipeline.

Common Terminal Operations

1. `collect`: Converts the stream into a different form, such as a collection.
2. `forEach`: Performs an action for each element of the stream.
3. `reduce`: Combines elements of the stream into a single value.
4. `toArray`: Converts the stream into an array.
5. `count`: Returns the count of elements in the stream.
6. `anyMatch`: Checks if any elements match a given predicate.
7. `allMatch`: Checks if all elements match a given predicate.
8. `noneMatch`: Checks if no elements match a given predicate.
9. `findFirst`: Returns the first element of the stream.
10. `findAny`: Returns any element of the stream.

Detailed Examples
1. `collect`

The `collect` method is used to convert the elements of the stream into a different form, such as a list, set, or map.
```java
import java.util.Arrays;
import java.util.List;
import java.util.Set;
import java.util.stream.Collectors;

public class CollectExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry", "apple");

        // Collect to List
        List<String> list = words.stream()
                                 .collect(Collectors.toList());
        System.out.println(list); // Output: [apple, banana, cherry, apple]

        // Collect to Set
        Set<String> set = words.stream()
                               .collect(Collectors.toSet());
        System.out.println(set); // Output: [apple, banana, cherry]
    }
}
```
2. `forEach`

The `forEach` method performs an action for each element of the stream. It’s often used for printing elements or applying side effects.
```java
import java.util.Arrays;
import java.util.List;

public class ForEachExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry");

        words.stream()
             .forEach(word -> System.out.println(word));
        // Output:
        // apple
        // banana
        // cherry
    }
}
```
3. `reduce`

The `reduce` method combines elements of the stream into a single value using an associative accumulation function.
```java
import java.util.Arrays;
import java.util.List;

public class ReduceExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

        // Sum of numbers
        int sum = numbers.stream()
                         .reduce(0, Integer::sum);
        System.out.println(sum); // Output: 15
    }
}
```
4. `toArray`

The `toArray` method converts the stream into an array.
```java
import java.util.Arrays;
import java.util.List;

public class ToArrayExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry");

        String[] array = words.stream()
                              .toArray(String[]::new);
        System.out.println(Arrays.toString(array)); // Output: [apple, banana, cherry]
    }
}
```
5. `count`

The `count` method returns the number of elements in the stream.
```java
import java.util.Arrays;
import java.util.List;

public class CountExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry");

        long count = words.stream()
                          .count();
        System.out.println(count); // Output: 3
    }
}
```
6. `anyMatch`

The `anyMatch` method checks if any elements match a given predicate.
```java
import java.util.Arrays;
import java.util.List;

public class AnyMatchExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry");

        boolean anyStartsWithA = words.stream()
                                      .anyMatch(word -> word.startsWith("a"));
        System.out.println(anyStartsWithA); // Output: true
    }
}
```
7. `allMatch`

The `allMatch` method checks if all elements match a given predicate.
```java
import java.util.Arrays;
import java.util.List;

public class AllMatchExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "apricot", "avocado");

        boolean allStartWithA = words.stream()
                                     .allMatch(word -> word.startsWith("a"));
        System.out.println(allStartWithA); // Output: true
    }
}
```
8. `noneMatch`

The `noneMatch` method checks if no elements match a given predicate.
```java
import java.util.Arrays;
import java.util.List;

public class NoneMatchExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry");

        boolean noneStartWithZ = words.stream()
                                      .noneMatch(word -> word.startsWith("z"));
        System.out.println(noneStartWithZ); // Output: true
    }
}
```
9. `findFirst`

The `findFirst` method returns the first element of the stream, if present.
```java

import java.util.Arrays;
import java.util.List;
import java.util.Optional;

public class FindFirstExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry");

        Optional<String> first = words.stream()
                                      .findFirst();
        first.ifPresent(System.out::println); // Output: apple
    }
}
```
10. `findAny`

The `findAny` method returns any element of the stream, which is useful in parallel processing.
```java
import java.util.Arrays;
import java.util.List;
import java.util.Optional;

public class FindAnyExample {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "cherry");

        Optional<String> any = words.stream()
                                    .findAny();
        any.ifPresent(System.out::println); // Output: apple (or any other element)
    }
}
```
5. Advanced Stream Operations Example

1. Merging Two Sorted Lists

Concept:
When you have two sorted lists and you want to merge them into a single sorted list, Java streams provide a straightforward way to do this using the `Stream.concat` method followed by sorting.

Detailed Steps:
1. Create Streams: Convert each list into a stream.
2. Concatenate Streams: Combine the two streams into one.
3. Sort the Combined Stream: Sort the concatenated stream.
4. Collect the Result: Collect the sorted elements back into a list.

Example:
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class MergeLists {
    public static void main(String[] args) {
        List<Integer> list1 = Arrays.asList(1, 3, 5);
        List<Integer> list2 = Arrays.asList(2, 4, 6);

        // Step 1: Create Streams from both lists
        Stream<Integer> stream1 = list1.stream();
        Stream<Integer> stream2 = list2.stream();

        // Step 2: Concatenate the streams
        Stream<Integer> concatenatedStream = Stream.concat(stream1, stream2);

        // Step 3: Sort the concatenated stream
        Stream<Integer> sortedStream = concatenatedStream.sorted();

        // Step 4: Collect the sorted stream into a list
        List<Integer> mergedList = sortedStream.collect(Collectors.toList());

        System.out.println(mergedList); // Output: [1, 2, 3, 4, 5, 6]
    }
}
OR

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;

public class MergeLists {
    public static void main(String[] args) {
        List<Integer> list1 = Arrays.asList(1, 3, 5);
        List<Integer> list2 = Arrays.asList(2, 4, 6);

        List<Integer> mergedList = Stream.concat(list1.stream(), list2.stream())
                                         .sorted()
                                         .collect(Collectors.toList());
        System.out.println(mergedList); // Output: [1, 2, 3, 4, 5, 6]
    }
}
```
2. Finding the Intersection of Two Lists

Concept:
The intersection of two lists means finding elements that are present in both lists. You can achieve this using the `filter` method to retain only the elements that exist in both lists.

Detailed Steps:
1. Create a Stream: Convert the first list into a stream.
2. Filter the Stream: Retain only elements that are present in the second list.
3. Collect the Result: Collect the filtered elements into a new list.

Example:
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class IntersectionExample {
    public static void main(String[] args) {
        List<Integer> list1 = Arrays.asList(1, 2, 3, 4, 5);
        List<Integer> list2 = Arrays.asList(3, 4, 5, 6, 7);

        // Step 1: Create a stream from the first list
        List<Integer> intersection = list1.stream()

            // Step 2: Filter elements that are also in the second list
            .filter(list2::contains)

            // Step 3: Collect the filtered elements into a new list
            .collect(Collectors.toList());

        System.out.println(intersection); // Output: [3, 4, 5]
    }
}
```
3. Removing Duplicates While Preserving Order

Concept:
To remove duplicates from a list while preserving the original order, you can use the `distinct` method provided by streams. This method ensures that only unique elements are kept in the resulting stream.

Detailed Steps:
1. Create a Stream: Convert the list into a stream.
2. Remove Duplicates: Use the `distinct` method to eliminate duplicates.
3. Collect the Result: Collect the unique elements back into a list.
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class RemoveDuplicates {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 2, 4, 1, 5, 6, 5);

        // Step 1: Create a stream from the list
        List<Integer> uniqueNumbers = numbers.stream()

            // Step 2: Remove duplicates using distinct
            .distinct()

            // Step 3: Collect the unique elements into a new list
            .collect(Collectors.toList());

        System.out.println(uniqueNumbers); // Output: [1, 2, 3, 4, 5, 6]
    }
}
```
4. Summing Transaction Amounts by Day

Concept:
When you have a list of transactions, and you want to calculate the total transaction amount for each day, you can use the `groupingBy` and `summingInt` collectors to achieve this.

Detailed Steps:
1. Create a Stream: Convert the list of transactions into a stream.
2. Group Transactions by Date: Use the `groupingBy` collector to group transactions based on the date.
3. Sum the Amounts: Use the `summingInt` collector to sum the transaction amounts for each day.
4. Collect the Result: Collect the results into a map where the key is the date and the value is the total amount for that date.
```java
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

class Transaction {
    private String date;
    private int amount;

    public Transaction(String date, int amount) {
        this.date = date;
        this.amount = amount;
    }

    public String getDate() {
        return date;
    }

    public int getAmount() {
        return amount;
    }
}

public class SumTransactionAmounts {
    public static void main(String[] args) {
        List<Transaction> transactions = Arrays.asList(
            new Transaction("2022-01-01", 100),
            new Transaction("2022-01-01", 200),
            new Transaction("2022-01-02", 300),
            new Transaction("2022-01-02", 400),
            new Transaction("2022-01-03", 500)
        );

        // Step 1: Create a stream from the list of transactions
        Map<String, Integer> sumByDay = transactions.stream()

            // Step 2: Group transactions by date and sum the amounts
            .collect(Collectors.groupingBy(Transaction::getDate, Collectors.summingInt(Transaction::getAmount)));

        System.out.println(sumByDay); // Output: {2022-01-01=300, 2022-01-02=700, 2022-01-03=500}
    }
}
```
5. Finding the k-th Smallest Element in an Array

Concept:
To find the k-th smallest element in an array, you can sort the array and then retrieve the k-th element. Streams provide a convenient way to do this using `sorted`, `skip`, and `findFirst`.

Detailed Steps:
1. Create a Stream: Convert the array into a stream.
2. Sort the Stream: Sort the elements in the stream.
3. Skip Elements: Skip the first k-1 elements.
4. Retrieve the k-th Element: Use `findFirst` to get the k-th element.
5. Handle Empty Result: Use `orElse` to provide a default value if the stream is empty.
```java
import java.util.Arrays;

public class KthSmallestElement {
    public static void main(String[] args) {
        int[] array = {4, 2, 7, 1, 5, 3, 6};
        int k = 3; // Find the 3rd smallest element

        // Step 1: Create a stream from the array
        int kthSmallest = Arrays.stream(array)

            // Step 2: Sort the stream
            .sorted()

            // Step 3: Skip the first k-1 elements
            .skip(k - 1)

            // Step 4: Retrieve the k-th element
            .findFirst()

            // Step 5: Provide a default value if the stream is empty
            .orElse(-1);

        System.out.println(kthSmallest); // Output: 3
    }
}
```
6. Finding the Frequency of Each Word in a List

Concept:
To find the frequency of each word in a list, you can use the `groupingBy` collector to group the words and then count the occurrences of each word using `counting`.

Detailed Steps:
1. Create a Stream: Convert the list of words into a stream.
2. Group by Word: Use the `groupingBy` collector to group the words.
3. Count Occurrences: Use the `counting` collector to count the occurrences of each word.
4. Collect the Result: Collect the results into a map where the key is the word and the value is the count.

Example:
```java
import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class WordFrequency {
    public static void main(String[] args) {
        List<String> words = Arrays.asList("apple", "banana", "apple", "cherry", "banana", "apple");

        // Step 1: Create a stream from the list of words
        Map<String, Long> wordFrequency = words.stream()

            // Step 2: Group by word and count the occurrences
            .collect(Collectors.groupingBy(word -> word, Collectors.counting()));

        System.out.println(wordFrequency); // Output: {banana=2, cherry=1, apple=3}
    }
}
```
7. Partitioning

a List Based on a Predicate

Concept:
Partitioning a list means dividing it into two groups based on a condition (predicate). The `partitioningBy` collector helps to achieve this by creating a map with two keys: `true` and `false`.

Detailed Steps:
1. Create a Stream: Convert the list into a stream.
2. Partition by Predicate: Use the `partitioningBy` collector to partition the elements based on a condition.
3. Retrieve Partitions: Retrieve the partitions from the resulting map.
```java
Example:

import java.util.Arrays;
import java.util.List;
import java.util.Map;
import java.util.stream.Collectors;

public class PartitioningExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9);

        // Step 1: Create a stream from the list
        Map<Boolean, List<Integer>> partitioned = numbers.stream()

            // Step 2: Partition by even or odd
            .collect(Collectors.partitioningBy(n -> n % 2 == 0));

        // Step 3: Retrieve the partitions
        List<Integer> evenNumbers = partitioned.get(true);
        List<Integer> oddNumbers = partitioned.get(false);

        System.out.println("Even numbers: " + evenNumbers); // Output: [2, 4, 6, 8]
        System.out.println("Odd numbers: " + oddNumbers); // Output: [1, 3, 5, 7, 9]
    }
}
```
