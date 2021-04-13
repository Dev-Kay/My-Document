# JAVA Docuemnt

### :book: Contents
1. [함수형 Interface (Functional Interface)](#함수형-Interface-(Functional-Interface))
2. [Enum의 활용](./contents/enum.md)
3. [Interface default/static method (~8)[./contents/Interface-default.md]


#### 함수형 Interface (Functional Interface)
:memo:[자세히 보기](./contents/Funcional-interface.md)
|함수형 인터페이스|  메서드 | ARGUMENT | RETURN |
|---------------|---------|-----------|---------|
|`Runnable`       | void run() | ~~**X**~~ | ~~**X**~~ |
|`Supplier<T>`     | T get()    | ~~**X**~~ | **T** |
|`Consumner<T>`| void accept(T t) | **T** | ~~**X**~~ |
|`Function<T, R>`| R apply(T t) | **T** | **R** |
|`Predicate<T>` | boolean test(T t) | **T** | **boolean** |
|`BiConsumner<T, U>` | void accept(T t, U u) | **T, U** | ~~**X**~~ |
|`BiFunction<T, U, R>` | R apply(T t, U u)    | **T, U** |  **R** |
|`BiPredicate<T, U>`| boolean test(T t, U u) | **T, U** |  **boolean** |
|`UnaryOperator<T>` | T apply(T t) | **T** | **T** |
|`BinaryOperator<T>` | T apply(T t1, T t2)    | **T, T** | **T**|
