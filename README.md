# AWS-Solution-Architect-Associate
AWS SAA 시험 관련 개념 정리

## 1. AWS 기본 용어
### • Region  

 ◦ AWS에서 사용하는 일종의 IDC의 집합으로서 거의 모든 클라우드 서비스가 탑재되는 곳으로 다수의 Availability Zone으로 구성됨  
 ◦ 한 곳의 AZ가 기능마비되더라도 다른 AZ가 기능을 수행함  
 ◦ 전세계 주요 대도시에 분포되어있음(서울 포함)  
 ◦ AWS 사용자는 각 Region마다 별도의 클라우드망을 구성할 수 있음

### • Availability Zone  

 ◦ 가용 영역이라 불리우며 IDC, 즉 데이터센터의 역할을 함  
 ◦ 보통 3~4개의 AZ가 각 Region마다 존재  
 ◦ 하나가 파괴더라도 다른 AZ가 기능을 수행함  
 ◦ 네트워크 구성에서 하나의 서브넷은 하나의 AZ를 의미  

### • On-premise  

 ◦ 클라우드 방식이 아닌 자체적으로 보유한 전산실과 같은 인프라를 의미  

### • Edge Location  

 ◦ CDN 서비스인 Cloudfront가 사용하는 캐시서버  
 ◦ 전세계에 분포되어있으며 Edge Location끼리 데이터를 공유함  

### • 탄력성(Elasticity)과 확장성(Scalability)  

 ◦ 탄력성은 AWS의 상징과도 같은 서비스인 Autoscaling처럼 단시간에 갑작스러운 요구에 따라 사용량을 늘리거나 줄이는 것을 의미(Scale out)  
 ◦ 확장성은 장기적인 관점, 즉 아키텍쳐적인 관점에서 예상치를 충분히 감당할 수 있는 리소스를 보유하는 것을 의미(Scale up)  



## 2. EC2  
### • Elastic Compute Cloud  
◦ **가상 서버를 제공하는 서비스**  
◦ 실제 물리서버와 똑같은 형태의 서비스를 제공하며 Linux나 Window 같은 기본 운영체제가 설치되어있음  
◦ SSH로 원격 연결이 가능함(공인 IP가 있는 경우 직접 연결 가능)  
◦ 기본 동작으로는 시작, 중지,종료,재부팅이 있음  
◦ 중지가 가능한 디스크 기반 인스턴스인 EBS 기반 인스턴스와 임시 스토리지를 제공하여 중지가 불가능한 Instance Store 기반 EC2로 나뉨  
◦ 재부팅의 경우 EBS 기반 EC2와 인스턴스 스토어 기반 EC2 모두 사용 가능하나 중지는 EBS 기반 EC2만 가능(중요!)  
◦ 인스턴스 유형으로는 범용, 컴퓨팅 최적화, 메모리 최적화, 스토리지 최적화 등이 있음  

### • EC2 상태  
◦ **Pending** : 인스턴스가 구동하기 위해 준비중인 상태, 요금 미청구  
◦ **Running** : 인스턴스를 실행하고 사용할 준비가 된 상태, 요금 청구  
◦ **Stopping** : 인스턴스가 중지 모드로 전환되려는 상태, 요금 미청구  
◦ **Shutting-down** : 인스턴스가 종료할 준비중인 상태, 요금 미청구  
◦ **Terminated** : 인스턴스가 종료된 상태, 요금 미청구  

### • EC2 구매 옵션  
◦ **온디맨드(On Demand)** : 필요할 때 바로 생성하여 사용하는 방식으로 1시간 단위로 과금이 이루어짐, 1분을 사용하더라도 1시간 과금을 물리는 방식  
◦ **스팟(Spot)** : 경매 방식의 인스턴스, 최초 생성시 기준가격이 화면에 나타나며 화면의 가격보다 높은 가격을 제시하면 계속 사용이 가능함. 그러   나 다른 사람이 더 높은 가격을 입찰했다면 인스턴스가 종료됨. 불시에 중단되어도 상관없거나 각종 테스트에 적합  
◦ **예약(Reserved)** : 12개월 또는 36개월 단위로 예약하여 사용하는 인스턴스로 온디맨드에 비해 가격이 대폭 할인됨. 장기적으로 사용할 경우 추천, 예약 인스턴스이기 때문에 사용하지 않더라도 요금이 부과됨  

### • AMI  
◦ **Amazon Machine Image**  
◦ 인스턴스를 시작하는데 필요한 정보를 제공하는 소위 ‘**이미지**’  
◦ AMI를 지정하여 인스턴스를 생성할 수 있음  
◦ EBS 지원 AMI와 인스턴스 스토어 지원 AMI로 나뉨  
◦ EBS 지원 AMI는 후술할 EBS 스냅샷에서 루트 디바이스 스토리지가 생성되며, 인스턴스 스토어 지원 AMI는 S3에 저장된 템플릿에서 생성된 스토어   볼륨을 사용함  
◦ AMI와 연결된 스냅샷은 지울 수 없음  
◦ 다른 리전으로 복사가 가능하며 권한을 준다면 다른 계정과도 공유 가능  
◦ AWS Market Place에서 다른 사람이 만들어둔 AMI를 쓰거나 공유 가능  

### • Elastic IP  
◦ **EC2에 설정되는 네트워크 인터페이스의 공인 IP**  
◦ EC2가 기본적으로 갖는 Public IP와 Private IP 등과는 구분해야함  
◦ Elastic IP를 사용하면 EC2로 하여금 중지되었다가 다시 시작하더라도 고정된 공인 IP를 사용하게 할 수 있음  
◦ Elastic IP는 계정 내 리전당 최대 5개까지 보유 가능하며 그 이상 필요시 AWS에 요청해야 함  
◦ 프리티어의 경우, 사용 중이 아닐 땐 요금이 부과됨  

### • Key Pair  
◦ EC2는 SSH 접속시 'key pair'라고 하는 퍼블릭 키 암호화 기법을 사용하여 접속하며 로그인 정보를 암호화 및 해독함  
◦ Key Pair 분실시 접속 불가  
◦ 또한 접속시 OS 별로 접속 name이 다름(Linux : EC2-USER 등)  

### • Batch Group  
◦ **클러스터** : 인스턴스를 AZ 내에서 근접하게 배치함. 결합된 노드간 낮은 지연 시간의 네트워크 달성 가능  
◦ **파티션** : 인스턴스가 담긴 그룹을 논리 세그먼트로 나누어 각 파티션에 배치함. 최대 7개의 파티션을 가질 수 있으며, 각 파티션은 자체     랙 세트를 보유하고 자체 네트워크와 전원을 보유함  
◦ **분산** : 파티션이 논리 세그먼트로 분리된 인스턴스 그룹인 것과 달리 분산은 **‘인스턴스’** 개체 하나가 자체 랙에 분산 배치됨. AZ당 최대 7개의 인스턴스 배치 가능  


