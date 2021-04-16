# Spring

## 📖 Contents
1. [왜 Service를 생성시 interface와 implements class를 별도로 만드는가??](왜-Service를-생성시-interface와-implements-class를-별도로-만드는가)
2. [필드인젝션이 좋지 않은 이유](필드인젝션이-좋지-않은-이유)

### 왜 Service를 생성시 interface와 implements class를 별도로 만드는가??
> 개인적인 견해일뿐이고 다른 의견이 있으시면 얼마든지 의견 감사합니다.

1. OOP를 제대로 갖추기 위해.
2. AOP 구현시 사용하고자(Feat. JDK Proxy)
3. 스프링은 인터페이스를 통한 DI 활용방법을 사용하도록 권장한다.
4. 객체지향 설계 5대원칙 중 개방 폐쇄의 원칙(OCP)에 기반한 전략 패턴을 사용하고 약결합 시켜 유지보수의 편리성을 증가 시키려고.
> 그냥 각자 융통성을 갖고 개발하면 되지 않을까???

👆[Top](#Spring)

### 필드인젝션이 좋지 않은 이유
> & 생성자 주입을 사용해야 하는 이유 

1. 생성자에 Null이 입력 되지 않은 한 NullPointerException을 방지 해준다.
2. 주입받을 필드를 final로 선언가능하여 무결성이 보장 된다.
3. 무분별한 의존성 주입방지
4. 순환 참조 오류 방지! (개인적으로 가장 이부분이 중요해 보임)
5. 테스트 코드 작성이 용이 하다. Filed Injection의 경우 단위 테스트시 객체를 생성해서 주입 할 수가 없다.

👆[Top](#Spring) :memo:[자세히 보기](./contents/why-field-injection-is-bad.md)
