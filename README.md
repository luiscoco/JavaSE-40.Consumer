# JavaSE-Consumer

In Java, java.util.function.Consumer is a functional interface introduced in Java 8 as part of the Java.util.function package. 

It represents an operation that takes a single input argument and returns no result. Its main use is to consume or process the input without returning any value.

The Consumer interface has a single abstract method called accept, which takes one argument and performs the operation on it. Here's the method signature:

```java
@FunctionalInterface
public interface Consumer<T> {
    void accept(T t);
}
```

Let's look at a simple example to illustrate how Consumer can be used. Suppose you want to print each element of a list:

```java
import java.util.Arrays;
import java.util.List;
import java.util.function.Consumer;

public class ConsumerExample {
    public static void main(String[] args) {
        List<String> fruits = Arrays.asList("Apple", "Orange", "Banana", "Grapes");

        // Using Consumer to print each element of the list
        Consumer<String> printConsumer = (fruit) -> System.out.println(fruit);

        // Applying the consumer to each element of the list
        fruits.forEach(printConsumer);
    }
}
```

In this example, the Consumer is used to define a lambda expression (fruit) -> System.out.println(fruit), which simply prints the input argument. 

The forEach method is then used on the List to apply this Consumer to each element of the list.

You can also simplify the code using method reference:

```java
fruits.forEach(System.out::println);
```

Here, System.out::println is a method reference that refers to the println method of the PrintStream class, and it serves as a shorthand for the lambda expression (fruit) -> System.out.println(fruit).

# More samples

Here are a few more examples of using Consumer in different scenarios:

## Example 1: Modifying Elements in a List

```java
import java.util.ArrayList;
import java.util.List;
import java.util.function.Consumer;

public class ConsumerExample {
    public static void main(String[] args) {
        List<Integer> numbers = new ArrayList<>(Arrays.asList(1, 2, 3, 4, 5));

        // Using Consumer to square each element in the list
        Consumer<Integer> squareConsumer = (number) -> {
            int squared = number * number;
            System.out.println(squared);
        };

        // Applying the consumer to each element of the list
        numbers.forEach(squareConsumer);
    }
}
```

## Example 2: Updating Elements in a Map
```java
import java.util.HashMap;
import java.util.Map;
import java.util.function.Consumer;

public class ConsumerExample {
    public static void main(String[] args) {
        Map<String, Integer> ageMap = new HashMap<>();
        ageMap.put("John", 30);
        ageMap.put("Alice", 25);
        ageMap.put("Bob", 35);

        // Using Consumer to update ages in the map
        Consumer<Map.Entry<String, Integer>> ageUpdater = entry -> {
            String name = entry.getKey();
            int currentAge = entry.getValue();
            entry.setValue(currentAge + 1);
            System.out.println(name + "'s new age: " + entry.getValue());
        };

        // Applying the consumer to each entry in the map
        ageMap.entrySet().forEach(ageUpdater);
    }
}
```

## Example 3: Custom Consumer for File Processing

```java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Path;
import java.util.function.Consumer;

public class FileProcessor {
    public static void main(String[] args) throws IOException {
        Path filePath = Path.of("example.txt");

        // Using Consumer to process each line in a file
        Consumer<String> lineProcessor = line -> {
            // Custom processing logic (e.g., filtering, transforming)
            System.out.println("Processing line: " + line);
        };

        // Applying the consumer to each line in the file
        Files.lines(filePath).forEach(lineProcessor);
    }
}
```

These examples demonstrate how Consumer can be used in various scenarios, such as transforming elements in a list, updating entries in a map, or processing lines in a file. 

# More advance topics about  "Consumer" in Java

Let's delve into some more advanced topics related to the Consumer interface in Java.

## 1. Chaining Consumers with andThen Method:

The Consumer interface has a default method called andThen. This method allows you to combine two Consumer instances to form a chain. 

The andThen method returns a new Consumer that performs the operations of both original consumers in sequence.

```java
import java.util.function.Consumer;

public class ChainingConsumers {
    public static void main(String[] args) {
        // First Consumer: Convert to uppercase
        Consumer<String> toUppercase = s -> System.out.println(s.toUpperCase());

        // Second Consumer: Print the length
        Consumer<String> printLength = s -> System.out.println("Length: " + s.length());

        // Chaining the consumers
        Consumer<String> combinedConsumer = toUppercase.andThen(printLength);

        // Applying the chained consumer
        combinedConsumer.accept("hello");
    }
}
```

In this example, combinedConsumer is a result of chaining two consumers (toUppercase and printLength).

When you apply this combined consumer to a string, it first converts the string to uppercase and then prints its length.

## 2. Exception Handling with Consumer and Exception Interface:

When working with Consumer, you may want to handle exceptions that can occur during the execution of the accept method. 

Java 8 introduced the Exception functional interface, which can be used to handle checked exceptions within lambdas.

```java
import java.util.function.Consumer;

public class ExceptionHandlingConsumer {
    public static void main(String[] args) {
        // Consumer with Exception handling
        Consumer<String> exceptionHandlingConsumer = wrapperConsumer(s -> {
            // Some operation that might throw an exception
            System.out.println(Integer.parseInt(s));
        });

        // Applying the consumer with exception handling
        exceptionHandlingConsumer.accept("123");
        exceptionHandlingConsumer.accept("abc"); // This will handle NumberFormatException
    }

    // Utility method to wrap a Consumer with exception handling
    private static <T> Consumer<T> wrapperConsumer(ThrowingConsumer<T, Exception> consumer) {
        return arg -> {
            try {
                consumer.accept(arg);
            } catch (Exception e) {
                System.err.println("Exception caught: " + e.getMessage());
            }
        };
    }

    @FunctionalInterface
    private interface ThrowingConsumer<T, E extends Exception> {
        void accept(T t) throws E;
    }
}
```

In this example, the wrapperConsumer method takes a ThrowingConsumer (a consumer that can throw an exception), and it wraps it with exception handling to catch and print any exceptions that occur during the execution.
