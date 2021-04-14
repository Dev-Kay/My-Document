# interface default/static method(~8)

> java 8 버전의 변경점 중 람다식의 도입으로 functionl programming이 가능해진 것과 함께 대대적인 변경 점중 하나라고 할 수 있겠다.

뭐... 엄밀히 따지면 functional programming을 가능하게 하기 위해 변경 된것 같기도 하고...

개념은 간단하다 기존에 interface에 abstract method를 선언하고 implement class 에서 구현하는 방식이였다면!   
interface내에서도 default 나 static을 사용하여 method를 구현이 가능 하다는 얘기다 

### default method
> 인터페이스 내에서 기본적인 구현을 같는 method
```java
public interface Calculator {
    public int plus(int a, int b);
    public int minus(int a, int b);
    default int multiple(int a, int b) { return a * b; }
}

public class CalculatorImpl implements Calculator {
    @Override
    public int plus(int a, int b) { return a + b; }
    
    @Override
    public int minus(int a, int b) { return a - b; }
}

public class CalculatorTest {
    public static void main(String[] args) {
        Calculator cal = new CalculatorImpl();
        int result = cal.multiple(3, 4);
        System.out.println(result);  // 12
    }
}
```
대략 이런 형태로 구현이 가능 하며 물론 override 하여 사용 가능 하다.
```java
public class CalculatorImpl implements Calculator {
    @Override
    public int plus(int a, int b) { return a + b; }
    
    @Override
    public int minus(int a, int b) { return a - b; }
    
    @Override
    public int multiple(int a, int b) {
        Calculator.super.multiple(a, b); // default를 실행 하려면~~~
        return a * b; 
    }
}
```

- 왜 나왔을까??   
    단편적으로 생각해 보면... 오랜기간 interface들이 추가되다보니 하위호환성과 유연성이 아닐까 한다.   
    결국 오래된 시스템일 수록 interface의 항목과 유지보수 상 변경 점이 있을 수 밖에 없는 상황이 올것이고,   
    그때 마다 전체를 다 변경 시킨다던가 deprecated된 method의 처리라던가 등등 이런 문제 점에 좀 더 유연히 대처하기 위해 나오지 않았나 한다.

### static method
> interface 내에 static method를 구현

```java
public interface Coin {
    public int getUnit();
    
    public static Coin findByPrice(Class<? extends Coin> e, int price) {
        return Arrays.stream(e.getEnumConstants())
                .filter(v -> v.getUnit() == price)
                .findFirst()
                .orElseThrow(IllegalArgumentException::new);
    }
}

public enum KrwCoin implements Coin {
    TEN(10),
    FIFTY(50),
    ;

    int unit;
    
    KrwCoin(int unit) { this.unit = unit; }
    
    @Override
    public int getUnit() { return this.unit; }
}

public class VendingMachine {
    public static void main(String[] args) {
        System.out.println(Coin.findByPrice(KrwCoin.class, 10)); // TEN
    }
}
```

- 왜 나왔을까??   
    유틸리티 메서드의 활용이 아니려나 싶다.. 
    오버라이드는 되지 않음!
