we can create Anonyms class that implement a certain interface , in case we will use it once.

```java
ExampleInterface ex1 = new ExampleInterface() {
	@Override
	public void ExampleMethod(String S){
		System.out.println(S);
	}
};
ex1.ExampleMethod();
```

in case the interface has only one function this is called functional interface and we can use Lambda expression instead of Anonyms class , in case method takes only one line curly braces are not important and in case one parameter () not important

```java
ExampleInterface ex1 = msg -> System.out.println(msg);
ExampleInterface ex2 = (msg) -> {
	System.out.println(msg);
};
```

we can mark the functional interface with annotation to make it clear this interface should have only one function , and it can has many default and static methods  , this not break it

```java
@FunctionalInterface
public inteface ExampleInterface {
	void ExampleMethod(String msg);
}
```

you can bind a function interface to a method reference instead of implementing the function itself

```java
ExampleInterface ex3 = System.out::println;
```

There are some functional interfaces that exist in the JAVA libraries itself for example
- Consumer : it has function accept takes 1 argument that run your code and return nothing
- Supplier : it has function get takes nothing and return  value
- Predicate : it take one parameter and return boolean , it has function test
- Function : it takes 1 parameter and return value and has function apply

```java
Consumer<String> consumer = str -> System.out.println(str);
consumer.accept("hello");

Supplier<String> supplier = () -> return 10;
System.out.println(supplier.get());

Predicate<String> predicate = msg -> msg.length() == 5;
System.out.println(predicate.test("hello"));

// parameter , return type
Function<Integer , String> function = x -> String.valueOf(x);
System.out.println(function.apply(10));
```

---
### Functional Programming

```java
List <Integer> nums = Arrays.asList(1 , 2 , 3 , 4 , 5);
for(int i = 0;i < nums.size();i++){
    System.out.println(nums.get(i));
}
for (Integer x : nums)
	System.out.println(x);

// using functional programming it can be like
nums.forEach(System.out::println); // it takes consumer
```

Optional : it's goal is to remind you to handle null checking

```java
Optional<Order> o = order.findById(10);
if(o.isPresent()){
	Order o1 = o.get();
}
// ---------------------------------------
Optional<Order> o = order.findById(10).ifPresent( order -> order.printData()); // takes consumer
// ---------------------------------------
Optional<Order> o = order.findById(10).orElseGet(() -> new Order()); // takes supplier
// ---------------------------------------
Optional<Order> o = order.findById(10).orElseThrow(() -> new RuntimeException("not exist")); // takes supplier
// ---------------------------------------
Order o = new Order();
Optional<Order> no = Optional.of(o);
// ---------------------------------------
Optional.empty(); // empty optional
// ---------------------------------------
Optional.ofNullable(order); // if null send empty otherwise send optional of it
```
----
### Streams
- filter : takes predicate a only allow elements who is valid for the condition
- map : transform from one thing to another
- collect : collect the result and return it
- reduce : reduce the elements to single Aggregate 
- count : count the elements passed through the streams
- findFirst : get the first element pass through the stream and return optional
- findAny : get first element pass through the stream and terminate (useful in threads , any thread find any element terminate the stream)
- AnyMatch : get any element match your predicate 
  
```java
List<Integer> nums = Arrays.asList(1 , 2 , 3, ,4 , 5 , 6 , 7, 9 , 10 , 8);
List<Integer> res = new ArrayList<>();
for(Integer num : nums) if(num % 2 == 0) res.add(num * 2);

List<Integer> res2 = nums.stream().filter(num -> num % 2 == 0).map(num -> 2 * num).collect(Collectors.toList());

Set<Integer> res3 = nums.stream().filter(num -> num % 2 == 0).map(num -> 2 * num).collect(Collectors.toSet());

int res = nums.stream().filter(num -> num % 2 == 0).map(num -> 2 * num).reduce(initail_value , (accumlator , element) -> return ...);

Optional<Integer> first = nums.stream().filter(num -> num > 3).findFirst();

Integer x = nums.stream().anyMatch(num -> num > 5);
```

Stream will not run unless there is a Terminal operation , the intermediate operations will not trigger it




