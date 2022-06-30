# 13-3 스트림
* 여러 자료의 처리에 대한 기능을 구현해 높은 클래스 
  * sorting, filter,,, etc
```
int [ ] arr = {1, 2, 3, 4, 5}
for(int i=0; i< arr.length; i++){
  System.out.println(i);
}

...

#stream
int [ ] arr = {1, 2, 3, 4, 5}
Arrays.stream(arr).forEach(n -> System.out.println(n));
```
* 스트림 연산
  * 중간 연산 
    * filter() 
      * 조건을 넣고 그 조건에 맞는 참인 경우만 추출
      * 문자열 길이가 5이상
      * sList.stream().filter(s -> s.length() >=5 ).forEach( s-> System.out.println(s));
    * map()
      * 클래스가 가진 자료 중 이름만 출력
      * customerList.stream().map(c-> c.getName()).forEach( s -> System.out.println(s));
  * 최종 연산
    * forEach(), sum(), count(),, 
    * 결과를 만들어 내는 곳에 이용
* 스트림의 특징
  * 자료의 대상과 관계없이 동일 연산을 수행
    * 컬렉션의 여러 자료 구조에 대해 저장된 요소값 출력, 조건에 따른 자료 추출, 등의 작업을 일관성 있게 처리
  * 한 번 생성하고 사용한 스트림은 재사용 불가능
    * 어떤 자료에 대한 스트림을 생성하고 이 스트림에 메서드를 호출하여 연산을 수행했담녀 해당 스틀미을 다시 다른 연산에 사용 불가능
    * 다른 기능을 호춯라려면 스트림을 새로 생성해야 함
  * 스트림의 연산은 기존 자료를 변경하지 않음
    * 생성하여 정렬한다거나 합을 구하는 연산을 한다해서 배열이나 컬렉션이 변경되지 않음
    * 스트림 연산을 위한 메모리 공간이 별도로 존재
  * 스트림의 연산은 중간 연산과 최종 연산이 있따
    * 중간 연산은 중간에 여러개 적용 가능
    * 최종 연산은 맨 마지막에 한 번 적용
* 예시
```dtd
        #public class TravelCustomer{
            private String name;
            private int age;
            private int price;
        }
        ...
        List<TravelCusomter> travelCustomerList = new ArrayList<TravelCusotmer>();
        
        ...
  
        travelCustomerList.stream()  //travelCusomterList에 대한 stream생성
        .mapToInt(c->c.getPrice())   // 비용 가져옴
        .sum();                     // 최종연산
  
        travelCustomerList.stream(). // stream 생성
        filter(c-> c.getAge() >= 20).  // age >=20 이상 filter()
        map(c-> c.getName()).        // 이름 추출
        sorted().                     // 정렬
        forEach(s->System.out.println(s)); //스트림에 대한 최종연산

```