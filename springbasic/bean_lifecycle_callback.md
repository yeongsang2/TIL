## 빈 생명주기 콜백

### 빈 생명주기 콜백 시작
데이터베이스 커넥션 풀이나, 네트워크 소켓처럼 애플리케이션 시작시점에 필요한 연결을 미리 해두고, 종료 시점에 연결을 모두 종료하는 작업을 진행하려면, 객체의 초기화와 종료 작업이 필요
* 스프링 빈은 간단하게 다음과 같은 라이플 사이클을 가짐
  * 객체 생성 -> 의존관계 주입
* 스프링 빈은 객체 생성하고 의존관계 주입이 다 끝난 다음에야 필요한 데이터를 사용할 준비가 완료됨
  * <strong>초기화 작업</strong>은 의존관계 주입이 모두 완료되고 난 다음에 호출해야함 -> 어떻게 알 수 있을까?
    * 콜백 메서드를 통해 초기화 시점을 알려주는 다양한 기능을 제공
    * 컨테이너가 종료되기 직전에 소멸 콜백을 줌

##### 스프링 빈의 이벤트 라이프사이클
스프링 컨테이너 생성 -> 스프링 빈 생성 -> 의존관계 주입 -> 초기화 콜백 -> 사용 -> 소멸전 콜백 -> 스프링 종료

* 초기화 콜백 : 빈이 생성되고, 빈의 의존관계 주입이 완료된 후 호출
* 소멸전 콜백 : 빈이 소멸되기 직전에 호출

###### 참고: 객체의 생성과 초기화를 분리하자 

### 스프은 크게 3가지 방법으로 빈 생명주기 콜백을 지원
* 인터페이스(InitializingBean, DisposableBean)
* 설정 정보에 초기화 메서드, 종료 메서드 지정
* @PostConstruct, @PreDestroy annotation 지원

어노테이션방식을 사용할 것을 권하므로 어노테이션방식에 관한 내용만 정리

```java
package hello.core.lifecycle;
import javax.annotation.PostConstruct;
import javax.annotation.PreDestroy;
public class NetworkClient {
 private String url;
 public NetworkClient() {
 System.out.println("생성자 호출, url = " + url);
 }
 public void setUrl(String url) {
 this.url = url;
 }
 //서비스 시작시 호출
 public void connect() {
 System.out.println("connect: " + url);
 }
 public void call(String message) {
 System.out.println("call: " + url + " message = " + message);
 }
 //서비스 종료시 호출
 public void disConnect() {
 System.out.println("close + " + url);
 }
 @PostConstruct
 public void init() {
 System.out.println("NetworkClient.init");
 connect();
 call("초기화 연결 메시지");
 }
 @PreDestroy
 public void close() {
 System.out.println("NetworkClient.close");
 disConnect();
 }
}
```

### @PostContstruct, @PreDestory 어노테이션 특징
* 스프링에서 권장하는 방법
* 패키지명이 javax.annotation.PostConstruct 
  * 스프링에 종속적인 기술x, 다른 컨테이너에서도 동작함
* 컴포넌트 스캔과 가장 잘 어울림
* 유일한 단점은 외부 라이브러리에는 적용하지 못한다는 점, 외부 라이브러리를 초기화, 종료해야하면 @Bean의 기능 사용
  * @Bean(init = "", destroyMethod ="close")

#### 정리
* @PostConstruct, @PreDestroy 어노테이션을 사용하자
* 코들 고칠 수 없는 외부 라이브러리를 초기화, 종료해야 하면 @Bean의 initMethod, destroyMethod를 사용하자
