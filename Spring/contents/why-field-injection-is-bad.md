# 필드인젝션이 좋지 않은 이유 & 생성자 주입을 사용해야 하는 이유

## 개요
- Dependency Injection (의존관계 주입) 이란
  - Field Injection (필드 주입)
  - Setter Based Injection (수정자를 통한 주입)
  - Constructor based Injection (생성자를 통한 주입)
- 왜 생성자 주입을 해야하는가?? (feat. 순환참조)
  - 순환 참조 방지
  - 생성자 주입이 테스트 코드 작성하기 좋은 이유

## 시발점..

우선 필자는 이론 보다는 뭐랄까 실전파라 일단 구현 테스트 하면서 경험으로 습득하는 타입이라...   
아무 생각 없이 @Autowired를 사용 하고 있었고, 이걸 Dependency Injection 이라 부르는 지도 몰랐다..   
헌데 우연한 기회에 (면접을 봤는데.. 비록 이론적으로 매우 많이 비어있어 떨어졌지만 면접관님 감사합니다.)   
여러 질문들을 받게 되며 내게 비어있는 부분들을 경험에 녹이며 정리 해보고자 한다.   

## Dependency Injection (의존관계 주입)
> 의존관계 주입의 자세한 설명은 Spring IoC로 다시 작성 하자.

### Field Injection (필드 주입)
```java
public class Controller {
    @Autowired
    private Service service;
}
```

### Setter Based Injection (수정자를 통한 주입)
```java
public class Controller {
    private Service service;
    
    @Autowired
    public void setService(Service service) {
      this.service = service;
    }
}
```

### Constructor based Injection (생성자를 통한 주입)
```java
public class Controller {
    private final Service service;

    @Autowired
    public Controller(Service service) {
        this.service = service;
    }
}
```
> Spring4.3 버전 이후는 수정자와 생성자 주입시 @Autowired 어노테이션이 생략 가능하다.
> 여기서는 알기 쉽게 그냥 넣음. 


### 왜 생성자 주입을 해야 하는가??
1. 생성자에 Null이 입력 되지 않은 한 NullPointerException을 방지 해준다.
    - 허나 이경우는 Spring에서 Null일경우 아예 bean에 등록 되지 않기 때문에 Field Injection의 경우도 무관하긴 함.
2. 주입받을 필드를 final로 선언가능하여 무결성이 보장 된다.
3. 무분별한 의존성 주입방지
    - 생성자 매개변수가 몇십개가 되버리면 보기조차 끔찍하기에 리팩토링을 고려 하지 않을까...
4. **순환 참조 오류 방지!** (개인적으로 가장 이부분이 중요해 보임)
    - Field Injection으로 구현을 해보자
        ```java
        @Service
        public class MemberService {

            @Autowired
            private ProductService productService;

            public void memberMethod() {
                productService.productMethod();
            }
        }
        ```
        ```java
        @Service
        public class ProductService {

            @Autowired
            private MemberService memberService;

            public void productMethod() {
                memberService.memberMethod();
            }
        }
        ```
        MemberService의 memberMethod()는 ProductService의 productMethod()를 호출하고, ProductService의 productMethod()는 MemberService의 memberMethod()를 호출한다.
        계속 서로 호출을 끊임없이 반복 하다가 StackOverflowError를 발생시키고 죽는다.   
        이게 순환 참조 문제이고 Setter Base Injection의 경우도 마찬가지이다.   
        **결국 Bean을 생성할 시에는 정상적으로 생성이 되지만, 런타임시 서로 순환 참조에 빠져 죽어버리게 되니 미리 발견에 힘든 점이 있다.**
        
    - Constructor based Inject 으로 하게 되면
        ```java
        @Service
        public class MemberService {
            private final ProductService productService;
            
            public MemberService(ProductService productService) {
                this.productService = productService;
            }
            
            public void memberMethod() {
                productService.productMethod();
            }
        }
        ```
        ```java
        @Service
        public class ProductService {
            private final MemberService memberService;
            
            public ProductService(MemberService memberService) {
                this.memberService = memberService;
            }
            
            public void productMethod() {
                memberService.memberMethod();
            }
        }
        ```
        이 후 구동을 하게 되면
        ```
        ***************************
        APPLICATION FAILED TO START
        ***************************

        Description:

        The dependencies of some of the beans in the application context form a cycle:

        ┌─────┐
        |  memberService defined in file [/Dev-kay/.../MemberService.class]
        ↑     ↓
        |  productService defined in file [/Dev-kay/.../ProductService.class]
        └─────┘
        ```
        컨테이너에서 빈을 생성시에
        ```java
        new MemberService(new ProductService(new MemberService(.......)))
        ```
        이런 순환참조 현상이 발생 하기에 어플리케이션이 구동되지 않는다.
        
5. 테스트 코드 작성이 용이 하다.
    Filed Injection의 경우 단위 테스트시 객체를 생성해서 주입 할 수가 없다.   
    Spring IoC Container가 다 생성하여 주입하기에 단위테스트를 작성하게 되면 NullPointerException이 발생하게 된다.
    (나는... 이거 때문에 대부분 통합테스트로 진행 하였으나 시간도 오래걸리고 매우 비효율 적이다..)
    하지만 Constructor based injection으로 사용 할 경우 객체 생성시 구현체를 넘겨 주면 되기에 테스트 하기 편하다.
    ```java
    MemberService memberService = new MemberService();
    ProductService productService = new ProductService(memberService);
    productService.productMethod();
    ```
    
> 👆:[Top](#필드인젝션이-좋지-않은-이유--생성자-주입을-사용해야-하는-이유)    ↩️:[Back](../)    ⏫:[Main](https://github.com/Dev-Kay/My-Document)
