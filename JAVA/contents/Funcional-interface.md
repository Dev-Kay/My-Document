# 함수형 인터페이스 (Functional Interface)

### 기본 함수형 인터페이스

|함수형 인터페이스|  메서드 | ARGUMENT | RETURN |
|---------------|---------|-----------|---------|
|`Runnable`       | void run() | ~~**X**~~ | ~~**X**~~ |
|`Supplier<T>`     | T get()    | ~~**X**~~ | **T** |
|`Consumner<T>`| void accept(T t) | **T** | ~~**X**~~ |
|`Function<T, R>`| R apply(T t) | **T** | **R** |
|`Predicate<T>` | boolean test(T t) | **T** | **boolean** |

> Function과 Predicate의 차이는 반환값이 boolean 이냐 정해진 타입이냐의 차이

```java
// Supplier
Supplier<String> supplier = () -> "Hello";
System.out.println(supplier.get());  // Hello

// Consumer
Consumer<String> consumer = (str) -> System.out.println(str);
consumer.accept("Hello");  // Hello

// Function
Function<String, String> function = (str) -> str;
String result = function.apply("Hello");
System.out.println(result); // Hello

// Predicate
Predicate<String> predicate = (str) -> str.equalse("Hello");
boolean result1 = predicate.test("Hello");
System.out.println(result1); // true
```

### Argument가 두개인 함수형 인터페이스
|함수형 인터페이스|  메서드 | RETURN |
|---------------|---------|---------|
|`BiConsumner<T, U>` | void accept(T t, U u) | ~~**X**~~ |
|`BiFunction<T, U, R>` | R apply(T t, U u)    | **R** |
|`BiPredicate<T, U>`| boolean test(T t, U u) | **boolean** |

```java
// BiConsumner
BiConsumner<Integer, Integer> biConsumner = (num1, num2) -> System.out.println(num1 + num2);
biConsumer.accept(1, 2); // 3

// BiFunction
BiFunction<Integer, Integer, Integer> biFunction = (num1, num2) -> num1 + num2;
int result = biFunction.apply(1, 2);
System.out.println(result);  // 3

// BiPredicate
BiPredicate<Integer, Integer> biPredicate = (num1, num2) -> num1 > num2;
boolean isResult = biPredicate.test(1, 2);
System.out.println(isResult); // false
```

### input 타입과 return 타입이 일치하는 함수형 인터페이스
|함수형 인터페이스|  메서드 |
|---------------|---------|
|`UnaryOperator<T>` | T apply(T t) |
|`BinaryOperator<T>` | T apply(T t1, T t2)    |

```java
// UnaryOperator
UnaryOperator<String> unaryOperator = (str) -> str;
String strResult = unaryOperator.apply("Hello");
System.out.println(strResult); // Hello

// BinaryOperator
BinaryOperator<Integer> binaryOperator = (num1, num2) -> num1 + num2;
int nResult = binaryOperator.apply(1, 2);
System.out.println(nResult); // 3
```

> :arrow_double_up:[Top](#함수형-인터페이스-(Functional-Interface))    :leftwards_arrow_with_hook:[Back](https://github.com/Dev-Kay/My-Document/tree/main/JAVA)    :information_source:[Home](https://github.com/Dev-Kay/My-Document)
