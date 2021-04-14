## 정의
> Enum이란 Enumeration의 앞 글자로 열거라는 의미를 갖는다. 관련이 있는 상수들의 집합입니다.   
> 자바에서는 final로 String과 같은 문자열이나 숫자들을 나타내는 기본 자료형의 값을 고정할 수 있습니다.   
> 이렇게 고정된 값을 상수라고 합니다. 영어로는 constant입니다. 어떤 클래스가 상수만으로 작성되어 있으면   
> 반드시 class로 선언할 필요는 없습니다. 이럴 때 class로 선언된 부분에 enum이라고 선언하면   
> 이 객체는 상수의 집합이다. 라는 것을 명시적으로 나타냅니다.   

기존에는 인터페이스나 클래스 내에서 상수를 선언함으로써 상수를 관리 하였는데 클래스 내에서 선언하는 부분은 네이밍이 겹칠 수 있고 불 필요하게 상수가 많아지는 단점이 있다.
인터페이스로 관리하는 경우 이런 부분은 줄어들지만 여전히 IDE의 지원을 적극적으로 받을 수 없고 타입 안정성이 떨어지는 단점을 가지고 있었다. 이를 보완하며 나온 것이 Enum이다.

### 특징
1. 클래스를 상수처럼 사용 할 수 있다.
2. Enum 클래스를 구현하는 경우 상수 값과 같이 유일하게 하나의 인스턴스가 생성되어 사용된다.
3. 서로 관련 있는 상수 값들을 모아 enum으로 구현하는 경우 유용하다.
4. 클래스와 같은 문법 체계를 따른다.
5. 상속을 지원하지 않는다.
   모든 enum들은 내부적으로 java.lang.enum 클래스에 의해 상속된다.   
   자바는 다중 상속을 지원하지 않기 때문에 Enum은 다른 클래스를 상속받을 수 없다. 

### 예제 Enum
> 예제 구현시 해당 enum을 참조 한다.
   ```java
   enum KrwBill {
       ONE_THOUSAND(1000),
       FIVE_THOUSAND(5000),
       TEN_THOUSAND(10000),
       FIFTY_THOUSAND(50000)
       ;
    
       int unit;
       KrwBill(int unit) {this.unit = unit;}
       public int getUnit() {return this.unit;}
   }
   ```
### Enum의 내부 Api
1. values()   
   - Enum 클래스가 가지고 있는 모든 상수 인스턴스를 배열로 리턴
   ```java
   for (KrwBill krwBill : KrwBill.values()) {
       System.out.println("ENUM NAME : " + krwBill + " / UNIT : " + krwBill.getUnit());
   }
   ```
   Result 
   ```
   ENUM NAME : ONE_THOUSAND / UNIT : 1000
   ENUM NAME : FIVE_THOUSAND / UNIT : 5000
   ENUM NAME : TEN_THOUSAND / UNIT : 10000
   ENUM NAME : FIFTY_THOUSAND / UNIT : 50000
   ```
   > enum.toString()은 enum의 이름을 반환 한다.

2. valueOf()
   - String 을 파라미터로 받아 일치하는 상수 인스턴스 명이 존재 하면 인스턴스를 반환
   - 인스턴스가 존재 하지 않으면 IllegalArgumentException을 일으킴
   ```java
   KrwBill krwBill = KrwBill.valueOf("ONE_THOUSAND");
   System.out.println("ENUM NAME : " + krwBill + " / UNIT : " + krwBill.getUnit());
   ```
   Result
   ```
   ENUM NAME : ONE_THOUSAND / UNIT : 1000
   ```
   
3. ordinal()
   - 상수의 index를 반환
   ```java
   for (KrwBill krwBill : KrwBill.values()) {
       System.out.println("ENUM NAME : " + krwBill + " / UNIT : " + krwBill.getUnit() + " / INDEX : " + krwBill.ordinal());
   }
   ```
   Result
   ```
   ENUM NAME : ONE_THOUSAND / UNIT : 1000 / INDEX : 0   
   ENUM NAME : FIVE_THOUSAND / UNIT : 5000 / INDEX : 1
   ENUM NAME : TEN_THOUSAND / UNIT : 10000 / INDEX : 2
   ENUM NAME : FIFTY_THOUSAND / UNIT : 50000 / INDEX : 3
   ```

## 사용 및 활용

단순히 상수로서의 사용 뿐아니라 응용하여 적용 할 수 있는 사례들

### 데이터의 그룹화 및 관리
- 관련되어 있지만 관련성을 표시하기 힘든 형태의 데이터를 한 곳에서 관리할 수 있다.   
- Enum은 완전한 클래스의 형태를 보이고 있기 때문에 관련 로직의 구현이 가능하다.

```java
// 각 국가별 도시 그룹
public enum  Country {
    KOREA("한국", Arrays.asList("Seoul","Inchun","Busan")),
    USA("미국", Arrays.asList("Washington","New York","LA")),
    ;

    private final String country;
    private final List<String> cities;

    Country(String country, List<String> cities) {
        this.country = country;
        this.cities = cities;
    }

    public String getCountry() {return this.country;}
    public List<String> getCities() {return this.cities;}

    // 필요한 로직을 추가 할 수 있다.
    public boolean isCity(String city) {
        return this.cities.contains(city);
    }

    // static 으로 유틸성 메서드도 가능
    public static Country findByCity(String city) {
        return Arrays.stream(values())
                     .filter(v -> v.isCity(city))
                     .findFirst()
                     .orElseThrow(IllegalArgumentException::new);
    }
}

public static void main(String[] args) {
    System.out.println(Country.KOREA.isCity("Seoul")); // true
    System.out.println(Country.findByCity("Seoul"));  // KOREA
}
```

### Funtional Interface 활용
_Funtional Interface에 대해서 알고 싶다면? :point_right: [여기로](/JAVA/contents/Funcional-interface.md)_

간단한 계산기를 작성 해보자(그냥 생각나는데로...)
```java
public class Calculator {
    public static calc(int a, int b, String symbol) {
        switch (symbol) {
            case "+": return (double)a + b;
            case "-": return (double)a - b;
            case "*": return (double)a * b;
            case "/": return BigDecimal.valueOf(a).divide(BigDecimal.valueOf(b)).doubleValue();
            default: thorw new IllegalArgumentException();
        }
    }
}
```
대충 이런 형태 라면 이걸 enum 화 시킬 경우
```java
public enum Calculator {
    PLUS("+", (a, b) -> (double)a + b),
    MINUS("+", (a, b) -> (double)a - b),
    MULTIPLE("*", (a, b) -> (double)a * b),
    DIVIDE("/", (a, b) -> BigDecimal.valueOf(a).divide(BigDecimal.valueOf(b)).doubleValue()),
    ;
    
    String symbol;
    BiFunction<Integer, Integer, Double> biFunction;
    
    Calculator(String symbol, BiFunction<Integer, Integer, Double> biFunction) {
        this.symbol = symbol;
        this.biFunction = biFunction;
    }
    
    public static Double calc(int a, int b, String symbol) {
        Calculator calculator = Arrays.stream(values())
                     .filter(v -> v.symbol.equals(symbol))
                     .findFirst()
                     .orElseThrow(IllegalArgumentException::new);
        return calculator.biFunction.apply(a, b);
    }
}
```
> 하고 보니 더 길어지긴 했는데... 간단한 프로그램이라 그렇고...   

### interface 상속을 통한 다형성
> enum은 java.lang.Enum 클래스를 기본적으로 상속받고 있기 때문에 이중상속이 안되는 JAVA 특성상 class나 enum끼리의 상속은 안됨   
> interface만 상속 가능함.

자판기를 예로 들어보자 메인 머신은 그대로고 동전주입기를 다른국가의 화폐로 변경이 가능하다.
대충 이런 자판기 메인 머신을 만든다는 가정하에 머신은 변경이 없이 주입기 교체만으로 모든게 정상적으로 서비스가 되어야 한다.   
막 생각나는데로 프로그램을 짜보자면~~~

```java
public enum KrwCoin {
    TEN(new BigDecimal(10)),
    FIFTY(new BigDecimal(50)),
    ONE_HUNDRED(new BigDecimal(100)),
    FIVE_HUNDRED(new BigDecimal(500))
    ;

    BigDecimal unit;
    KrwCoin(BigDecimal unit) { this.unit = unit; }
    public BigDecimal getUnit() { return this.unit; }
    public static KrwCoin findByPrice(BigDecimal price) {
        return Arrays.stream(values())
                .filter(v -> v.unit.equals(price))
                .findFirst()
                .orElseThrow(IllegalArgumentException::new);
    }
}

public enum UsdCoin {
    PENNY(new BigDecimal("0.01")),
    NICKEL(new BigDecimal("0.05")),
    DIME(new BigDecimal("0.1")),
    QUARTER(new BigDecimal("0.25")),
    HALF(new BigDecimal("0.5")),
    DOLLAR(new BigDecimal("1"))
    ;

    BigDecimal unit;
    UsdCoin(BigDecimal unit) { this.unit = unit; }
    public BigDecimal getUnit() { return this.unit; }
    public static UsdCoin findByPrice(BigDecimal price) {
        return Arrays.stream(values())
                .filter(v -> v.unit.equals(price))
                .findFirst()
                .orElseThrow(IllegalArgumentException::new);
    }
}

public class CoinMachine {
    BigDecimal currentInsert = new BigDecimal(0.00);

    public BigDecimal add(BigDecimal coin) {
        return currentInsert.add(coin);
    }
}

public class VendingMachine {
    // args[0] : 통화, args[1] : 입금금액
    public static void main(String[] args) {
        CoinMachine coinMachine = new CoinMachine();
        
        switch (args[0]) {
            case "KRW":
                coinMachine.add(KrwCoin.findByPrice(new BigDecimal(args[1])));
                break;
            case "USD":
                coinMachine.add(UsdCoin.findByPrice(new BigDecimal(args[1])));
                break;
        }
        
        
    }
}
```

뭐 대충 이런식일 것이다 (예제로 설명을 위해 대충 만들어서 컴파일도 안해본 소스이기에 개념 잡는 정도로만 봐줬으면 좋겠습니다....)   
interface 상속을 하여 리팩토링을 하게 되면

```java
public interface Coin {
    public BigDecimal getUnit();
    
    public static Coin findByPrice(Class<? extends Coin> e, BigDecimal price) {
        return Arrays.stream(e.getEnumConstants())
                .filter(v -> v.getUnit().equals(price))
                .findFirst()
                .orElseThrow(IllegalArgumentException::new);
    }
}

public enum CountryCoins {
    KRW(KrwCoin.class),
    USD(UsdCoin.class),
    ;

    Class<? extends Coin> coins;

    CountryCoins(Class<? extends Coin> coins) {
        this.coins = coins;
    }

    public Class<? extends Coin> getCoins() {
        return coins;
    }
}

public enum KrwCoin implements Coin {
    TEN(new BigDecimal(10)),
    FIFTY(new BigDecimal(50)),
    ONE_HUNDRED(new BigDecimal(100)),
    FIVE_HUNDRED(new BigDecimal(500));

    BigDecimal unit;
    
    KrwCoin(BigDecimal unit) {
        this.unit = unit;
    }
    
    @Override
    public BigDecimal getUnit() {
        return this.unit;
    }
}

public enum UsdCoin implements Coin {
    PENNY(new BigDecimal("0.01")),
    NICKEL(new BigDecimal("0.05")),
    DIME(new BigDecimal("0.1")),
    QUARTER(new BigDecimal("0.25")),
    HALF(new BigDecimal("0.5")),
    DOLLAR(new BigDecimal("1"));

    BigDecimal unit;

    UsdCoin(BigDecimal unit) {
        this.unit = unit;
    }

    @Override
    public BigDecimal getUnit() {
        return this.unit;
    }
}

public class CoinMachine {
    BigDecimal currentInsert = new BigDecimal(0.00);

    public BigDecimal add(Coin coin) {
        return currentInsert.add(coin.getUnit());
    }
}

public class VendingMachine {
    // args[0] : 통화, args[1] : 입금금액
    public static void main(String[] args) {
        CoinMachine coinMachine = new CoinMachine();
        
        Coin addCoin = MonetaryUnit
                .findByPrice(CountryCoins.valueOf(args[0]).getCoins(), new BigDecimal(args[1]));
        coinMachine.add(MonetaryUnit);
    }
}
```
취급 가능한 통화가 늘어날때마다 메인은 변경이 없이 enum만 작업 하여 처리가 가능해 진다.   

### 그외 적용 사례들
하나의 interface를 상속받는 세개의 서비스가 있다고 가정 하고 각각의 서비스들은 요청된 parameter에 따라 분기하여 처리 해야 한다고 하자.
```java
public class Controller {
    public void doService(String param) {
        switch(param) {
            case "API_A":
                new ServiceA().run();
                break;
            case "API_B":
                new ServiceB().run();
                break;
            case "API_C":
                new ServiceC().run();
                break;
        }
    }
}
```
대충 이런 형태 일것이다... 아마도... 어쨌건 enum 화 시키면
```java
public enum Apis {
    API_A(new ServiceA()),
    API_B(new ServiceA()),
    API_C(new ServiceA()),
    ;
    
    Service service
    Apis(Service service) {
        this.service = service;
    }
    
    public Service getService() {return this.service;}
}

public class Controller {
    public void doService(String param) {
        Apis.valueOf(param).getService().run();
    }
}
```
코드가 한결 깔끔해진다.   


**Enum을 사용했을 시 장점을 들자면**
1. 연산에 대해 명확한 상수명을 가지고 상태와 로직을 모두 enum에서 관리 하고 소스의 가독성 또한 좋아진다.
2. 연산자를 추가시 별도의 가이드나 공유가 없더라도, IDE에서 즉각 지원하기에 알기 쉽다.
3. 내용등의 추가 변경시 Enum내에서만 작업을 하면 되어, 리팩토링시 범위가 적다.
4. Type safe한 로직을 작성 할 수 있어 유지보수나 가독성에 이점..
5. 상수와 관련 로직을 한곳에서 관리 가능.
6. enum이 public static final 이라는 점은 Singleton과 유사한 특성을 지니고 있다.   
   따라서 중복 생성이 안되면서도 일반 클래스 기능을 할 수도 있고, 필요한 때에는 enum의 특성을 활용할 수도 있다.   
   이런 특성 때문에 Singleton 패턴, Abstract Factory 패턴, Factory Method 패턴, State 패턴, Strategy 패턴, Visitor 등 다양한 패턴을 enum으로 구현이 가능하다.

> :arrow_double_up:[Top](#정의)    :leftwards_arrow_with_hook:[Back](https://github.com/Dev-Kay/My-Document/tree/main/JAVA)    :information_source:[Home](https://github.com/Dev-Kay/My-Document)
