
The **Stream API** in Java is a feature introduced in **Java 8** that provides a functional-style approach to processing sequences of elements (collections, arrays, or I/O resources). It allows performing operations like **filtering, mapping, reducing, sorting, and collecting** efficiently using a **declarative** and **parallelizable** manner.

**Stream API** will help you to setup a pipeline to process your data

Stream is not a data Structure and it not store the data by itself it just setting the pipeline and query it.

```java
import java.util.stream.Stream
```
### Definition:

**The Java Stream API is a framework for processing collections of data in a functional programming way, supporting operations like filtering, mapping, and reducing with lazy evaluation and potential parallel execution.**

### Key Features:

- **Functional-style operations** (e.g., `map()`, `filter()`, `reduce()`)
- **Lazy evaluation** (intermediate operations are executed only when a terminal operation is called)
- **Parallel processing support** (`parallelStream()`)
- **Immutable and stateless processing** (does not modify the original data source)

generate method in stream class that generate data as stream , it works as data source

```java
Stream.generate(<something will be generated as stream>)
```

we can also pass a Supplier ---> functional interface has method T get();
it won't take anything but will return something

forEach method takes a consumer and for each object in the stream it consumes it

Consumer--> functional interface has function void accept(T t);
it doesn't return any thing but consumes object

```java
Supplier<String>supplier = new Supplier<String>(){
	@Override
	public String get(){
		return "Hello Stream";
	}
};
Stream<String> streamOfStrings = Stream.generate(supplier);

// for each will consume objects in the stream
// the for each method takes consumer
Consumer<String>consumer = new Consumer<String>(){
	@override
	public void accept(String t){
		System.out.println(t);
	}
};
streamOfStrings.forEach(consumer);
```

BiFunction ---> functional interface that has function called apply that take 3 Generic types (par1 , par2 , return type)

```java
BiFunction<Integer , String , Boolean> temp = new BiFunction(){
	@Override
	public Boolean apply(Integer x , String y){
		return y.length() >= x;
	}
};
```

we can use Stream interface to create finite stream using Stream.of() method
```java
Stream<Integer> stream = Stream.of(1 ,2 , 3 , 4 , 5 , 6 , 7, , 8, 9);
Consumer consumer = new Consumer<Integer>(){
	@override
	void accept(Integer t){
		System.out.println(t);
	}
};
stream.forEach(consumer);
```

-------------

## **Lambda**

### **Lambda Expression in Java**

A **lambda expression** in Java is an **anonymous function** that provides a concise way to express instances of **functional interfaces**. It was introduced in **Java 8** to enable functional programming by allowing the definition of short and simple methods without explicitly declaring a class.

### **Definition:**

_A lambda expression is a short block of code that takes parameters and returns a value, typically used for implementing functional interfaces in a concise way._

### **Syntax:**
```java
(parameter_list) -> { method_body }
```

- **`->`** is the lambda operator.
- Left side: List of parameters.
- Right side: Body of the function.
- if the body is one statement you don't need {}
- if only one parameter you don't need () around parameter

### Example
```java
// Without Lambda (Using Anonymous Class)
Runnable r1 = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello from Runnable!");
    }
};
r1.run();

// With Lambda Expression
Runnable r2 = () -> System.out.println("Hello from Lambda!");
r2.run();

```

### **Key Features of Lambda Expressions:**

✔ **Concise syntax** – Eliminates boilerplate code.  
✔ **Improves readability** – More functional and expressive.  
✔ **Works with functional interfaces** – Interfaces with a single abstract method (e.g., `Runnable`, `Comparator<T>`, `Consumer<T>`).

## **Use collection as Stream Source**

In **Java 8+**, you can use the **Stream API** to process collections efficiently by converting them into **streams**. This allows you to perform **functional-style** operations like filtering, mapping, and reducing in a **concise and parallelizable** manner.

### **. Creating a Stream from a Collection**

You can convert a `Collection` (like `List`, `Set`) into a stream using:

- `stream()` → Creates a **sequential** stream.
- `parallelStream()` → Creates a **parallel** stream (for multi-threaded processing).

#### **Example: Converting a List to a Stream**
```java
List<Student> studentList = Arrays.asList(x , y , z);
Stream <Student> stream = studentList.stream();
stream.forEach((Student student)->System.out.println(student));
```

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");

        // Convert list to stream and filter names starting with 'A'
        List<String> filteredNames = names.stream()
                .filter(name -> name.startsWith("A"))
                .collect(Collectors.toList());

        System.out.println(filteredNames); // Output: [Alice]
    }
}
```

### **Using `Set` as a Stream Source**
```java
import java.util.Set;
import java.util.stream.Collectors;

public class SetStreamExample {
    public static void main(String[] args) {
        Set<Integer> numbers = Set.of(10, 20, 30, 40, 50);

        // Convert set to stream and map values to their squares
        Set<Integer> squaredNumbers = numbers.stream()
                .map(n -> n * n)
                .collect(Collectors.toSet());

        System.out.println(squaredNumbers); // Output: [100, 400, 900, 2500, 1600]
    }
}
```

### **Using `Map` as a Stream Source**

Since `Map` is not a `Collection`, you can stream its keys, values, or entries separately:

```java
import java.util.Map;

public class MapStreamExample {
    public static void main(String[] args) {
        Map<Integer, String> users = Map.of(1, "Alice", 2, "Bob", 3, "Charlie");

        // Stream through map keys
        users.keySet().stream().forEach(System.out::println);

        // Stream through map values
        users.values().stream().forEach(System.out::println);

        // Stream through map entries
        users.entrySet().stream()
                .forEach(entry -> System.out.println(entry.getKey() + " -> " + entry.getValue()));
    }
}
```

----------
### Using Stream to Filter data

you can use Stream to do data filtering it takes predicate (functional interface has function "boolean test(T x)")

```java
Predicate<Integer> x = new Predicate<Integer>(){  
    @Override  
    public boolean test(Integer x){  
        return x > 5;  
    }  
};  
Stream<Integer> s = Stream.of(1 , 3 , 45 , 4325 ,456);  
s = s.filter(x);  
```

```java
public static void main(String []args){
	Integer[] numbers = {1 , 2 , 3 , 4 , 5 , 6 , 7 , 8 , 9 , 10};
   // equalivant to Stream.of
	Stream<Integer> stream = Arrays.stream(numbers);
	stream = stream.filter(d -> d > 5); // filter takes predicate
	stream.forEach(d -> System.out.println(d));
}
```

we can chain stream operations like this 
```java
Integer[] numbers = {1 , 2 , 3 , 4 , 5 , 6 , 7 , 8 , 9 , 10};
   // equalivant to Stream.of
Arrays.stream(numbers).filter(d -> d > 5).forEach(d -> System.out.println(d));
```

peek method help in debugging and takes consumer

```java
Integer[] numbers = {1 , 2 , 3 , 4 , 5 , 6 , 7 , 8 , 9 , 10};
   // equalivant to Stream.of
Arrays.stream(numbers).peek(d -> System.out.println(d)).filter(d -> d > 5).forEach(d -> System.out.println(d));
```

map method help in doing transformation , it takes Function<T , R> functional interface that has method apply : R apply (T t){}
```java
Stream.of(1 , 2 , 3 , 4 ,5  , 6).filter(num -> num > 5).map(number->{
	switch(number){
		case 1: return "one";
		case 2: return "two";
		case 3: return "three";
		case 4: return "four";
		case 5: return "five";
		case 6: return "six";	
	}
	return "number greater than 6";
}).forEach(num -> System.out.println(num));
// one , two , three , four , five , six 
// Not 1 , 2 , 3 , 4 , 5 , 6
// becasue the transformation happen first
```

In Java Streams, operations are categorized into **intermediate operations** and **terminal operations**.

### 1. **Intermediate Operations**

- These **transform** the stream but **don’t produce a result immediately**.
- They are **lazy**, meaning they are only executed when a terminal operation is called.
- They return another stream, allowing method chaining.
- Examples: `map()`, `filter()`, `sorted()`, `distinct()`, `limit()`

### 2. **Terminal Operations**

- These **trigger** the stream processing and produce a **result or side effect**.
- After a terminal operation, the stream cannot be used again.
- Examples: `collect()`, `forEach()`, `count()`, `reduce()`, `toArray()`, `anyMatch()`
```java
import java.util.List;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<String> names = List.of("Alice", "Bob", "Charlie", "David", "Alex");

        // Intermediate operations (filter and map)
        List<String> result = names.stream()
            .filter(name -> name.startsWith("A"))  // Filters names starting with "A"
            .map(String::toUpperCase)              // Converts to uppercase
            .collect(Collectors.toList());         // Terminal operation (collects into a list)

        System.out.println(result); // Output: [ALICE, ALEX]
    }
}
```

we can also use collect function which will collect the data in the stream and transform it to a collection
```java
Stream <String> s = Stream.of(1 , 2 , 3 , 4 , 5 , 6 , 7);
List <String> x = s.collect(Collectors.toList());
```


we can also use count method that return the number of objects in the stream
```java
Stream <String> s = Stream.of(1 , 2 , 3 , 4 , 5 , 6 , 7);
int c = s.count(); // number of elements in the stream
```

we can use limit method that limit objects size
```java
Stream <String> s = Stream.of(1 , 2 , 3 , 4 , 5 , 6 , 7);
s.filter(x -> x <= 6).limit(2).forEach(x -> System.out.println(x)); //  take only 2 elements from the list
```

we can use sorted method to sort the object stream
```java
Stream < Integer > s = Stream.of(1234 , 234 , 234 , 345 , 546 , 764 , 234).sorted(); // intermediate function
```

sorting a stream in reverse order
```java
	Stream.of(1 , 2 , 3 ,4 ,5).sorted(Comparator.reverseOrder())
	.forEach(num -> System.out.println(num));
```

takeWhile method --> short circuit filter and stateful intermediate operation , whenever the predicate return false it stop processing the rest elements
```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9); 
// Take elements while they are less than 5 
List<Integer> result = numbers.stream() .takeWhile(n -> n < 5) .collect(Collectors.toList()); System.out.println(result); 
// Output: [1, 2, 3, 4]
```

skip method ---> skip first x elements , stateful intermediate operation
```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5, 6, 7, 8, 9); 
List<Integer> result = numbers.stream() .skip(8) .collect(Collectors.toList()); System.out.println(result);
// output is 9 only
```

distinct method ---> remove duplicates 
```java
List<Integer> numbers = List.of(1, 1, 2, 2, 3, 3 ,7, 8, 9); 
List<Integer> result = numbers.stream() .distinct() .collect(Collectors.toList()); System.out.println(result);
```

----------------
### stateful vs stateless

In Java Streams, the terms "stateful" and "stateless" describe whether or not an operation maintains or relies on some internal state during the processing of elements in a stream.

### Stateless Operations:

A **stateless** operation does not maintain any state between elements, and its behavior is independent for each element in the stream. Each element is processed in isolation, without any knowledge of the other elements. These operations are generally more efficient because they don’t require extra memory or complex tracking across elements.

#### Example of Stateless Operations:

- `map()`: Transforms each element independently.
- `filter()`: Filters out elements based on a condition without any reference to previous or subsequent elements.
- `forEach()`: Applies an action to each element, without remembering anything about past or future elements.

```java
Stream<Integer> numbers = Stream.of(1, 2, 3, 4, 5);
numbers.filter(n -> n % 2 == 0).forEach(System.out::println);  // Stateless: Just filters and applies forEach
```

### Stateful Operations:

A **stateful** operation, on the other hand, depends on the state or context of the stream's elements. It may involve remembering information about previously processed elements, affecting the computation of future elements. These operations can be more expensive in terms of time and space, as they may require buffering or holding onto intermediate results.

#### Example of Stateful Operations:

- `distinct()`: Removes duplicate elements. This needs to remember all previously seen elements, hence it has state.
- `sorted()`: Sorts the stream, which requires comparison between elements.
- `collect()`: The terminal operation for accumulating elements into a collection or summary.
```java
Stream<Integer> numbers = Stream.of(1, 2, 3, 4, 5, 1, 3, 2);
numbers.distinct().forEach(System.out::println);  // Stateful: Keeps track of seen elements
```

### Key Differences:

- **Stateless operations** process each element independently and do not require any additional memory or tracking.
- **Stateful operations** involve maintaining state across elements and often need extra memory (like for sorting or keeping track of duplicates).

### Performance Considerations:

- Stateless operations are generally more efficient because they don’t need to maintain or manage any internal state.
- Stateful operations can impact performance, especially for large streams, as they may need to buffer data or perform additional comparisons.

In summary, stateless operations are simpler and faster, while stateful operations are more powerful but can introduce complexity and overhead.

----------
## iterate method in java streams

In Java Streams, the `iterate` function is a static method in the `Stream` class that generates an infinite or finite sequential stream of elements by repeatedly applying a function to the previous element.

```java
Stream<T> iterate(T seed, UnaryOperator<T> f)
```

```java
Stream<T> iterate(T seed, Predicate<? super T> hasNext, UnaryOperator<T> next)
```

Unary operator --> Function takes only one Type (Transformation in same type)
### **Parameters:**

1. **`seed`** – The initial value (first element of the stream).
2. **`f` (UnaryOperator)** – A function applied to generate each subsequent element.
3. `hasNext` (Predicate) – A condition to limit stream generation (used in the second version).
#### **Infinite Stream (`iterate(seed, f)`)**

This version keeps generating elements infinitely

```java
Stream.iterate(2, n -> n + 2)
      .limit(10) // Limit the stream to 10 elements
      .forEach(System.out::println);

```

#### Finite Stream (`iterate(seed, hasNext, f)`)

```java
Stream.iterate(1, n -> n <= 10, n -> n + 1)
      .forEach(System.out::println);

```

## **Key Points:**

- The first version (`iterate(seed, f)`) is **infinite**, so always use `limit()` to avoid running indefinitely.
- The second version (`iterate(seed, hasNext, f)`) provides a stopping condition.
- Useful for **generating sequences**, **mathematical series**, or **custom iterators**.

--------

## Method Reference

Method referencing in Java is a shorthand notation of lambda expressions that allows you to directly refer to an existing method by its name. It is used with functional interfaces and helps in making the code more readable and concise.

```
ClassName::methodName
```

```
instanceName::methodName
```

There are **four** types of method references in Java:

1. **Reference to a Static Method**
2. **Reference to an Instance Method of a Particular Object**
3. **Reference to an Instance Method of an Arbitrary Object of a Particular Type**
4. **Reference to a Constructor** (Constructor Reference)
### **Reference to a Static Method**

When a method is static, you can reference it using:
```java
import java.util.function.Function;

class MathUtils {
    public static int square(int x) {
        return x * x;
    }
}

public class Main {
    public static void main(String[] args) {
        // Using lambda expression
        Function<Integer, Integer> lambda = x -> MathUtils.square(x);
        
        // Using method reference
        Function<Integer, Integer> methodRef = MathUtils::square;

        System.out.println(lambda.apply(5));  // Output: 25
        System.out.println(methodRef.apply(6));  // Output: 36
    }
}
```

### **Reference to an Instance Method of a Particular Object**

If you already have an instance of a class, you can reference its instance method using:

```java
import java.util.function.Supplier;

class Printer {
    public void printMessage() {
        System.out.println("Hello from Printer!");
    }
}

public class Main {
    public static void main(String[] args) {
        Printer printer = new Printer();

        // Using lambda expression
        Supplier<String> lambda = () -> {
            printer.printMessage();
            return "";
        };

        // Using method reference
        Supplier<String> methodRef = printer::printMessage;

        lambda.get();   // Output: Hello from Printer!
        methodRef.get(); // Output: Hello from Printer!
    }
}
```

### **Reference to an Instance Method of an Arbitrary Object of a Particular Type**

This is useful when working with collections where you reference a method of an instance that will be supplied at runtime.

```java
ClassName::instanceMethodName
```

```java
import java.util.Arrays;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

        // Using lambda expression
        names.forEach(name -> System.out.println(name));

        // Using method reference
        names.forEach(System.out::println);
    }
}
```

### **Reference to a Constructor**

You can reference a constructor when you need to create a new object.

```java
ClassName::new
```

```java
import java.util.function.Supplier;

class Person {
    public Person() {
        System.out.println("Person created!");
    }
}

public class Main {
    public static void main(String[] args) {
        // Using lambda expression
        Supplier<Person> lambda = () -> new Person();
        
        // Using constructor reference
        Supplier<Person> constructorRef = Person::new;

        lambda.get();    // Output: Person created!
        constructorRef.get(); // Output: Person created!
    }
}
```

### **Conclusion**

- **Static methods:** `ClassName::staticMethod`
- **Instance methods of a specific object:** `instance::method`
- **Instance methods of an arbitrary object of a particular type:** `ClassName::method`
- **Constructors:** `ClassName::new`

Method references make the code more readable and eliminate unnecessary lambda expressions.

-------
Using **method references** and the **Stream API** together in Java allows for cleaner, more readable, and concise functional-style programming.

## 🔹 What Are Method References?

Method references (`::`) are shorthand for lambda expressions that call an existing method. They improve readability and avoid redundant code.

There are **four types** of method references:

1. **Static Method Reference** → `ClassName::staticMethod`
2. **Instance Method Reference (from an object)** → `instance::instanceMethod`
3. **Instance Method Reference (from a class)** → `ClassName::instanceMethod`
4. **Constructor Reference** → `ClassName::new`

## 🔹 Using Method References with Stream API

The **Stream API** provides a way to process collections using functional operations like `map`, `filter`, `forEach`, etc. **Method references** can be used to simplify the lambda expressions in these operations.

using static method reference

```java
import java.util.List;
import java.util.stream.Collectors;

public class Main {
    public static void main(String[] args) {
        List<String> names = List.of("Alice", "Bob", "Charlie", "David");

        // Using method reference instead of lambda
        List<String> upperCaseNames = names.stream()
                                           .map(String::toUpperCase) // Instead of x -> x.toUpperCase()
                                           .collect(Collectors.toList());

        System.out.println(upperCaseNames); // [ALICE, BOB, CHARLIE, DAVID]
    }
}
```

using instance method reference
```java
import java.util.List;

public class Main {
    public static void main(String[] args) {
        List<String> names = List.of("Alice", "Bob", "Charlie");

        // Using instance method reference
        names.forEach(System.out::println); // Instead of names.forEach(name -> System.out.println(name))
    }
}

```

using constructor reference
```java
import java.util.List;
import java.util.stream.Collectors;

class Person {
    String name;
    Person(String name) {
        this.name = name;
    }
    @Override
    public String toString() {
        return name;
    }
}

public class Main {
    public static void main(String[] args) {
        List<String> names = List.of("Alice", "Bob", "Charlie");

        // Using constructor reference instead of lambda
        List<Person> people = names.stream()
                                   .map(Person::new) // Instead of name -> new Person(name)
                                   .collect(Collectors.toList());

        System.out.println(people); // [Alice, Bob, Charlie]
    }
}
```

