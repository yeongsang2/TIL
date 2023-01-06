### Building A Native Executable

* application 을 native 실행가능하게 compile
* 컨테이너에 native 실행 가능파일 패키징

### GraalVM
* three distribution
    * Oracle GraalVM Community Edition (CE)
    * Oracle GraalVM Enterprise Edition (EE)
    * Mandrel
        * Oracle GraalVM CE의 다운스트림 배판

### enviroment
* JDK 11
* apache maven 3.8.6
* docker 20.10.21
* GraalVM CE 22.3
* windows
    * C development enviroment
    * https://medium.com/graalvm/using-graalvm-and-native-image-on-windows-10-9954dc071311

### Configuring GraalVM
* Oracle GraalVM Community Edition (CE) 사용
* https://github.com/graalvm/graalvm-ce-builds/releases
* 환경변수 설정
* naive-image install  
    * ${GRAALVM_HOME}/bin/gu install native-image
    * macOs Catalina 환경에서 아직 미해결 이슈가 있음
        * GraalVM을 포함하는 Docker 컨테이너에서 maven 작업실시

### create native executable
* CLI
    * quarkus build --native
* Maven 
    * ./mvnw install -Dnative
* Gradle
    * ./gradlew build -Dquarkus.package.type=native

윈도우 환경에서의 패키징<br>

x64 Native Tools Command Prompmt 실행후 project 폴더로 이동한뒤 <br>
```
     mvnw package -Pnative
```
target/getting-started-1.0.0-SNAPSHOT-runner 파일 생성<br>
build time : 05:28 min

### creatintg containter

containter-image extensions 활용

```
./mvnw package -Pnative -Dquarkus.native.container-build=true -Dquarkus.container-image.build=true
```
* quarkus.native.container-build=true -> GraaVM 없이도 linux 환경에서 실행가능한 파일을 생성
* quarkus.container-image.build=true -> quarkus 가 contaitnner image를 만들게끔 지시

### docker build

src/main/docker/Dockerfile.native-micro
```
FROM quay.io/quarkus/quarkus-micro-image:2.0
WORKDIR /work/
COPY target/*-runner /work/application
RUN chmod 775 /work
EXPOSE 8080
CMD ["./application", "-Dquarkus.http.host=0.0.0.0"]
```

도커 이미지 빌드 <br>
```
docker build -f src/main/docker/Dockerfile.native-micro -t quarkus-quickstart/getting-started .
```

실행
```
docker run -i --rm -p 8080:8080 quarkus-quickstart/getting-started
```