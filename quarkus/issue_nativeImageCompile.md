### native image compile

<br>
어플리케이션을 native image 로 컴파일 하는 과정에서 겪은 문제

```
Error: Native-image building on Windows currently only supports target architecture: AMD64 (?? unsupported)
```

window 환경에서 quarkus application을 natvie image로 컴파일 하기위해서는 
windows 10 sdk 설치 후 x64 Native Tools Command Prompt 에서 패키징 화 해야 함
<br>

이때 visutal studio installer 에서 language package을 한국어가 아닌 영어로 설정해 주어야함

ref: https://github.com/oracle/graal/issues/3122