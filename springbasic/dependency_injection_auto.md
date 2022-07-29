## 의존관계 자동 주입

#### 의존관계 주입 방법 4가지
* 생성자 주입
* 수정자 주입(setter 주입)
* 필드 주입
* 일반 메서드 주입

### 생성자 주입
* 이름 그대로 생성자를 통해서 의존 관계를 주입 받는 방법이다.
* 지금까지 우리가 진행했던 방법이 바로 생성자 주입이다.
* 특징
  * 생성자 호출시점에 딱 1번만 호출되는 것이 보장됨
  * 불변, 필수 의존관계에 사용
```java

    @Componenet
    public class OrderServiceImpl implements OrderService {
      private final MemberRepository memberRepository;
      private final DiscountPolicy discountPolicy;

      @Autowired
      public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy
              discountPolicy) {
        this.memberRepository = memberRepository;
        this.discountPolicy = discountPolicy;
      }
    }
```
##### 중요! 생성자가 딱 1개만 있으면 @Autowired를 생략해도 자동 주입 됨. 물론 스프링 빈에만 해당

### 수정자 주입(setter 주입)
* setter라 불리는 필드의 값을 변경하는 수정자 메서드를 통해 의존관계를 주입하는 방법
* 특징
  * 선택, 변경 가능성이 있는 의존관계에 사용
  * 자바빈 프로퍼티 규약의 수정자 메서드 방식을 사용하는 방법

```java
@Component
public class OrderServiceImpl implements OrderService {
  private MemberRepository memberRepository;
  private DiscountPolicy discountPolicy;

  @Autowired
  public void setMemberRepository(MemberRepository memberRepository) {
    this.memberRepository = memberRepository;
  }

  @Autowired
  public void setDiscountPolicy(DiscountPolicy discountPolicy) {
    this.discountPolicy = discountPolicy;
  }
}
```
참고: @Autowired의 기본 동작은 주입할 대상이 없으면 오류가 발생, 주입할 대상이 없어도 동작하게 하려면 @Autowired(required = false)로 지정하면 됨

### 필드 주입
* 이름 그대로 필드에 바로 주입하는 방법
* 특징
  * 코드가 간결해서 많은 개발자들을 유혹하지만 외부에서 변경이 불가능해서 테스트 하기 힘들다는 치명적인 단점이 있다.
  * DI프레임워크가 없으면 아무것도 할 수 없다.
  * 사용x
```java
@Component
public class OrderServiceImpl implements OrderService {
 @Autowired
 private MemberRepository memberRepository;
 @Autowired
 private DiscountPolicy discountPolicy;
  }
```

### 일반 메서드 주입
* 일반 메서드를 통해서 주입 받을 수 있다.
* 특징
  * 한번에 여러 필드를 주입 받을 수 있다.
  * 일반적으로 잘 사용x
  
```java
@Component
public class OrderServiceImpl implements OrderService {
  private MemberRepository memberRepository;
  private DiscountPolicy discountPolicy;

  @Autowired
  public void init(MemberRepository memberRepository, DiscountPolicy
          discountPolicy) {
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
  }
}
```
참고: 의존관계 자동 주입은 스프링 컨테이너가 관리하는 스프링 빈이어야 동작함. 스프링 빈이 아닌 Member같은 클래스에서 @Autowired 코드를 적용해도 아무 기능 동작 xx

### 옵성 처리
* 주입할 스프링 빈이 없어도 동작해야 할 경우
* @Autowired만 사용하면 required 옵션의 기본값이 true로 되어 있어서 자동 주입 대상이 없으면 오류가 발생함

### 자동 주입 대상을 옵션으로 처리하는 방법 3가지
* @Autowired(required = false): 자동 주입할 대상이 없으면 수정자 메서드 자체가 호출 안됨
* org.springframework.lang.@Nullable: 자동 주입할 대상이 없으면 null이 입력됨
* Optional<> : 자동 주입할 대상이 없으면 Optional.empty가 입력됨
```java
//호출 안됨
  @Autowired(required = false)
  public void setNoBean1(Member member) {
   System.out.println("setNoBean1 = " + member);
  }
  //null 호출
  @Autowired
  public void setNoBean2(@Nullable Member member) {
   System.out.println("setNoBean2 = " + member);
  }
  //Optional.empty 호출
  @Autowired(required = false)
  public void setNoBean3(Optional<Member> member){
    System.out.println("setNoBean3 = "+member);
  }
```

#### 생성자 주입 선택해라!
<strong>이유</strong>
##### 불변
* 대부분의 의존관계 주입은 한번 일어나면 애플리케이션 종료시점까지 의존관계 변경할 일이 없음. 오히려 대부분의의존관계는 애플리케이션 종료 전까지 변하면 안됨
* 수정자 주입을 사용하면, setXXX메서드를 public으로 열어두어야함
  * 누군가 실수로 변경할 수 있음
* 생성자 주입은 객체를 생성할 때 딱 1번만 호출되므로 이후에 호출되는 일이 없음.
  * 불변한 설계 가능

##### final 키워드
생성자 주입을 사용하면 필드에 final 키워드를 사용할 수 있다. 그래서 생성자에서 혹시라도 값이 설정되지 않은 오류를 컴파일 시점에 막아줌
```java
@Component
public class OrderServiceImpl implements OrderService {
  private final MemberRepository memberRepository;
  private final DiscountPolicy discountPolicy;

  @Autowired
  public OrderServiceImpl(MemberRepository memberRepository, DiscountPolicy
          discountPolicy) {
    this.memberRepository = memberRepository;
  }
  //..
}
```
* 잘 보면 필수 필드인 discountPolicy에 값을 설정해야 하는데, 이 부분이 누락됨
  * 자바 컴파일 시점에 다음 오류 발생시킴
참고: 수정자 주입을 포함한 나머지 주입 방식은 모두 생성자 이후에 호출되므로, 필드에 final 키워드를 사용할 수 없다. 오직 생성자 주입 방식만 final 키워드를 사용할 수 있음 <br> 생성자는 final 필드의 최종 초기화를 마쳐야함

### 롬복
##### @RequiredArgsConstructor
* final이 붙은 필드를 모아서 생성자를 자동으로 만들어줌

```java
@Component
@RequiredArgsConstructor
public class OrderServiceImpl implements OrderService {
  private final MemberRepository memberRepository;
  private final DiscountPolicy discountPolicy;
}
```

#### 정리
최근에는 생성자를 딱 1개 두고, @Autowired를 생략하는 방법을 주로 사용 + @RequiredArgsConstructor

#### 조회 빈이 2개 이상 - 문제

* @Autowired는 타입(Type)으로 조회
* 인터페이스 하위 타입 두개를 스프링 빈으로 선언
  * 의존관계 자동주입
    * NoUniqueDefinitionException 오류 발생

#### 해결방법
* @Autowired 필드명
* @ Qualifier -> Qualifier 끼리 매칭 -> 빈 이름 매칭
* @Primary 사용

@Autowired 필드 명 맻이
* @Autowired는 타입 매칭을 시도하고, 이때 어려 빈이 있으면 필드 이름, 파라미터 이름으로 빈 이름을 추가 매칭

기존코드
```java
      @Autowired
      private DiscountPolicy discountPolicy
```

필드 명을 <strong>빈</strong>이름으로 변경
```java
  @Autowired
  pricate DiscountPolicy rateDiscountPolicy
```
* 필드명이 rateDiscountPolicy이므로 정상 주입됨
* 필드 명 매칭은 먼저 타입 매칭을 시도하고 그 결과에 여러 빈이 있을때 추가로 동작하는 기능

##### @Autowired 매칭 정리
1. 타입 매칭
2. 타입 매칭의 결과가 2개 이상일 때 필드명, 파라미터 명ㅇ로 빈 이름 매칭

##### @Qualifier 사용
* @Qualifier는 추가 구분자를 붙여주는 방법, 빈이름을 변경하는 것이 아니고 주입시 추가적인 방법을 제공하는 것

빈 등록시 @Qualifier를 붙여줌
```java
  @Component
  @Qualifier("mainDiscountPolicy")
  public class RateDiscountPolicy implements DiscountPolicy{}
```
주입시에 @Qualifier를 붙여주고 등록한 이름을 적어줌
```java
  public OrderServiceImpl(MemberRepository memberRepository, @Qualifier("mainDiscountPolicy") DiscountPolicy discountPolicy){
    this.memberRepository = memberRepository;
    this.discountPolicy = discountPolicy;
        }
```

### @Primary 사용
* @Primary는 우선순위를 정하는 방법. @Autowired 시에 여러 빈이 매칭되면 @Primary가 우선권을 가짐
```java
@Component
@Primary
public class RateDiscountPolicy implements DiscountPolicy {}
}
```
#### 조회와 빈이 모두 필요할 때, List, Map
* 해당 타입의 스프링 빈이 다 필요한 경우
  * ex) 할인서비스 -> 클라이언트가 선택 가능
```java
ApplicationContext ac = new
AnnotationConfigApplicationContext(AutoAppConfig.class, DiscountService.class);
 DiscountService discountService = ac.getBean(DiscountService.class);
 Member member = new Member(1L, "userA", Grade.VIP);
 int discountPrice = discountService.discount(member, 10000,
"fixDiscountPolicy")
        
....

static class DiscountService {
  private final Map<String, DiscountPolicy> policyMap;
  private final List<DiscountPolicy> policies;
  public DiscountService(Map<String, DiscountPolicy> policyMap,
                         List<DiscountPolicy> policies) {
    this.policyMap = policyMap;
    this.policies = policies;
    System.out.println("policyMap = " + policyMap);
    System.out.println("policies = " + policies);
  }
  public int discount(Member member, int price, String discountCode) {
    DiscountPolicy discountPolicy = policyMap.get(discountCode);
    System.out.println("discountCode = " + discountCode);
    System.out.println("discountPolicy = " + discountPolicy);
    return discountPolicy.discount(member, price);
  }

```
### 로직 분석
* DiscountService는 Map으로 모든 DiscountPolicy를 주입받는다. 이때fixDiscountPolicy, rateDiscountPolicy가 주입된다.
* discount() 메서드는 discountCode로 "fixDiscountPolicy"가 넘어오면 map에서 fixDiscountPolicy 스프링 빈을 찾아 실행함. <br> 물론 "rateDiscountPolicy"가 넘어오면 rateDiscountPolicy 스프링 빈을 찾아서 실행

### 주입 분석
* Map<String, DiscountPolicy> : map의 키에 스프링 빈의 이름을 넣어주고, 그 값으로 DiscountPolicy 타입으로 조회한 모든 스프링 빈을 담아줌
* List<DiscountPolicy> : DiscountPolicy 타입으로 조회한 모든 스프링 빈으 ㄹ담아줌
* 만약 해당하는 타입의 스프링 빈이 없으면, 빈 컬렉션이나 Map을 주입함

<strong>참고 - 스프링 컨테이너를 생성하면서 스프링 빈 등록하기<strong>
스프링 컨테이너는 생성자에 클래스 정보를 받음. 여기에 클래스 정보를 넘기면 해당 클래스가 스프링 빈으로 자동 등록됨
```java
  new AnnotationConfigApplicationContext(AutoAppConifg.class, DiscountPolicy.class);
```
* new AnnotationConfigApplicationContext()를 통해 스프링 컨테이너 생성
* AutoAppConfig.class, DiscountService.class를 파라미터로 넘기면서 해당 클래스를 자동으로 스프링 빈으로 등록
--> 스프링 컨테이너를 생성하면서, 해당 컨테이너 동시에 AutoAppConfig, DiscountService를 스프링 빈으로 자동 등록함


### 자동, 수동의 올바른 실무 운영 기준
* 편리한 자동 기능을 기본으로 사용
* 업무 로직 빈 : 웹을 지원하는 컨트롤러, 핵심  비즈니스 로직이 있는 서비스,  데이터 계층의 로직을 처리하는 리포지토리등이 모두 업무 로직이다.
  * 유사한 패턴 존재, 문제파악 이 비교적 쉬움 -> 자동기능 적극 사용 권장
* 기술 지원 빈 : 기술적인 문제나 공통 관심사(AOP)를 처리할 때 주로 사용됨. 데이터베이스 연결이나, 공통 로그 처리 처럼 업무 로직을 지원하기 위한 하부 기술이나 공통 기술
  * 업무로직과 비교해서 그 수가 적고, 어플리케이션에 광범위하게 영향을 미침, 문제파악 어려움 -> 수동빈 등록 권장
* 비즈니스 로직 중에서 다형성을 적극 활용할 때
  * 수동빈으로 등록하거나 자동으로하면 특정 패키지에 같이 묶어두는 것이 좋음

### 정리
* 편리한 자동 기능을 기본으로 사용
* 직접 등록하는 기술 지원 객체는 수동 등록
* 다형성을 적극 활용하는 비즈니스 로직은 수동 등록을 고민
