## BufferedReader

<hr>

#### java.io.BufferedReader

```java
    BufferedReader in = new BufferedReader(new FileReader("foo.in"));
    BufferedReader in = new BufferedReader(new InputStreameReader(System.in)) //입력받을때
```

#### Constructors
* BufferedReader (Reader in)
* BufferedReader (Reader in , int sz)

### Method
* void close()
  * stream 종료
* Stream<String> lines()
  * stream을 반환
* void mark(int readAheadLimit)
  * 

* int read()
  * 하나의 문자 읽어들임
* String readLine()
  * 한줄을 읽음
  * string으로 반환
* long skip(long n)
  * n 만큼의 문자 skip
  * skip된 문자수 반환
* boolean ready()
  * stream이 읽을 준비가 되었는지 여부 반환
* void mark(int readAhreadLimit)
  * 스트림에 현재 위치에 마크
* void close() 
  * 스트림을 종료하고 자원ㅇ르 풀어줌

https://docs.oracle.com/javase/8/docs/api/
