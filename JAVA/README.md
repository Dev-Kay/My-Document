# JAVA Docuemnt

## :book: Contents
1. [함수형 Interface (Functional Interface)](#함수형-Interface-(Functional-Interface))
2. [Enum의 활용](#Enum의-활용)
3. [Interface default/static method (~8)](./contents/Interface-default.md)





### 함수형 Interface (Functional Interface)
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

:raised_hands:[top](#JAVA-Docuemnt)

### Enum의 활용
:memo:[자세히 보기](./contents/enum.md)

- 클래스를 상수처럼 사용
- Enum 클래스를 구현하는 경우 상수 값과 같이 유일하게 하나의 인스턴스가 생성되어 사용
- 서로 관련 있는 상수 값들을 모아 enum으로 구현
- 클래스와 같은 문법체계를 따름
- 상속을 지원하지 않는다 
  - interface를 상속 받아 enum을 구현 할 수 있다.
- API
  - values() : Enum이 가지고 있는 모든 상수 값을 리턴
  - valueOf(String) : String이 Enum내 상수명과 일치 한 Enum 인스턴스를 리턴
  - ordinal() : 해당 Enum 인스턴스의 index를 반환

:raised_hands:[top](#JAVA-Docuemnt)

### Interface default/static method (~8)
:memo:[자세히 보기](./contents/Interface-default.md)
> JAVA8 이후 대대적 변경 점 중 하나
- default method
  - 인터페이스 내에서 기본적인 구현을 같는 메소드
  - 8 이전에는 인터페이스에서 추상 메서드를 정의하고 클래스에서 추상 메서드를 구현
- static method
  - interface내 static method의 구현이 가능 해지며 간단한 유틸리티성 인터페이스를 만들 수 있게 되었다.

:raised_hands:[top](#JAVA-Docuemnt)
