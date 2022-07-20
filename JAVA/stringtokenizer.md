## StringTokenizer

<hr>

#### 문자열 쪼개기

##### java.lang.Object <br> java.util.StringTokenizer

* test code
```java
    StringTokenizer st = new String("this is a test");
    while(st.hasMoreTokens()){
        System.out.println(st.nextToken());
        }
```
* ouput
```java
    this
    is
    a
    test
```

* Constructors
  * StringTokenizer(String str)
  * StringTokenizer(string str, string delim)
    * 특정 delim으로 문자열 분리
  * StringTokenizer(String str, String delim, boolean returnDelims)

* Method Summary

| Modifer and Type | Method and Description                                              |
|------------------|---------------------------------------------------------------------|
| int              | countTokens() <br> 남아있는 token의 개수 반환 전체 token이 아닌 현재 남아있는 token의 갯수 |
| boolean          | hasMoreElements() <br> 남아있는 token이 있으면 true반환                       |                                                                  |
| boolean          | hasMoreTokens() <br> hasMoreElements() 와 같음                         |
| objects          | nextElement() <br> 다음 토큰을 반환 (object로 반환)                           |
| string           | nextToken() <br> 다음 토큰을 반환                                          |
| string           | nextToken(String delim) <br> delim 기준으로 다음 토큰 반환                    |
