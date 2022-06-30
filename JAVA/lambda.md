# 13-2 람다식
<hr>

* 함수형 프로그래밍?
  * 매개변수만을 사용하여 만드는 함수. 즉 함수 내부에서 함수 외부에 있는 변수를 사용하지 않아 함수가 수행되더라도 외부에 영향을 주지 않음
  * 입력받은 자료 이외에 외부 자료에 영향을 미치지 않음
    * 병렬 처리에 적합
    * 확장성 있는 프로그램 개발 가능

* 람다식?
  * 메서드 이름이 없고 메서드 실행하는데 필요한매개변수와 매개변수를 활용한 실행 코드를 구현
  * 함수형 인터페이스를 만들고, 인터페이스 람다식으로 구현할 메서드를 선언함
  * 클래스를 따로 생성할 필요 없이 메서드 바로 구현 가능
  * 람다식 내에서 지역 변수 수정 불가능
    *   지역 변수는 메서드 호출이 끝나면 메모리에서 사라지기 때문에

* 람다식 문법
  * 매개변수가 하나일 경우 괄호 생략 가능
    * str ->  {System.out.println(str);}
  * 매개 변수 두 개인 경우 괄호 생략 x 
    * (x, y) -> { System.out.println(x +y) }
  * 구현 부분 한 문장인 경우 중괄호 생략 가능
    * str -> System.out.println(str);
  * 구현 부분이 return문 하나라면 중괄호와 return 모두 생략
    * str -> return str.length
    * (x,y) -> x+y
    * (x,y) -> x>y ? x:y
* 구현


```
    pulic interface Lambda(){

	public void add(int x, int y);	
    }

    ....

    Lambda lambDa = (s, v) -> { return s +v } //일종의 구체화?
    int a = lambDa.add(1,2);
```
* 매개변수로 전달하는 람다식
  * 람다식을 변수에 대입하여 매개변수로 전달 
    * 이때 전달되는 매개변수의 자료형은 인터페이스

```
    interface PrintString{
        void showString(String str);
    }

    public class TestLambda{
        printString lambdaStr = s-> System.out.println(s);
        lambdaStr.showString("hello lambda_1);
        showMyString(lambdaStr);
    }
    public static void showMyString(PrintString p){
        p.showString("hello lambda_2");
    }
    }
```
* 반환 값으로 쓰이는 람다식
  * 메서드의 반환형을 람다식의 인터페이스로 구현

```
    public static PrintString returnString(){
        return s -> System.out.println(s + "world");
    }
    ...
    {
    ..
    PrintString reStr = returnString();
    reStr.showString("hello");
    }
```