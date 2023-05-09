## aws-computing-power

### AWS EC2
###### Amazon Elastic Cloud Compute

* 가장 기본적인 형태의 클라우드 컴퓨팅 (=클라우드 컴퓨터 한 대)
* 온디맨드 : 선결제 금액이나 장기 약정 없이 저렴하게 유연하게 Amazon EC2를 사용하기 원하는 사용자
  *  쓴만큼만 요금 부과
* 스팟 인스턴스 : 시작 및 종료 시간이 자유로운 애플리케이션
* Auto scaling 지원
  * 그룹 안에서 자동으로 ec2 인스턴스 auto scaling 해줌

### AWS Elastic Beanstalk

* AWS 클라우드에서 애플리케이션을 신속하게 배포하고 관리할 수 있는 서비스
  * 어플리케이션 코드를 업로드하기만 하면 작동
* elasticBeanstalk = EC2 + 배포 버전 관리 + Elastic load Balancer + 모니터링 + 로그 트래킹 + 오토 스케일링

### AWS Fargate 

* Fargate 사용하면 컨테이너를 실행하기 위해 가상 머신의 클러스터를 프로비저닝, 구성 또는 조정의 필요가 없음
* 컨테이너 실행을 위해 컨테이너 실행을 위한 instance(ec2)를 실행시켜야하는 수고를 덜어줌
* 비용이 비싸다는 단점이 있음

### AWS ECR
###### Amazon Elastic Container Registry

* Amazon Elastic Container Registry는 안전하고 확장가능하고 신뢰할 수 있는 AWS 관리형 컨테이너 이미지 레지스트리 서비스
  * RDS(DB) = 데이터 저장소
  * S3 = 사진, 이미지 등 파일을 저장하는 장소
  * ECR = 도커 이미지를 저장하는 장소

### AWS ECS
###### Amazon Elastic Container Service

* ECS는 AWS에서 제공하는 컨테이너 오케스트레이션 서비스로 여러 어플리케이션 컨테이너를 쉽고 빠르게 실행하고, 컨테이너를 적절하게 분배 및 확장 & 축소할 수 있도록 도와주는 서비스
* AWS EC2와 AWS Fargate 중 원하는 환경에서 실행 가능
### AWS Lambda

* 서버 없이도 코드를 실행시킬 수 있는 서버리스 컴퓨팅 서비스
* 코드를 돌리기위한 리소스를 임의로 지정할 수 있으며, 사용시간에 따라 과금이 됨
* 사용예시
  * 비동기처리
  * 에측이 불가능 한 리소스 (대용량 처리 / 머신러닝)


##### Cold stat / Warm start
* 기본적으로 EC2와 같은 인스턴스보다는 Latency가 높다.
* 콜드 스타트 : 배포 패키지의 크기와 코드 실행 및 코드의 초기화 시간에 따라 새 실행 환경으로 호출을 라우팅 할 때 지연 시간이 발생하는 람다 호출 시작 


위 내용은 원티드 백엔드 프리온보딩 챌린지 강의내용을 바탕으로 작성한 내용입니다