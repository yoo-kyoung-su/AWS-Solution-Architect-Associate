# AWS-Solution-Architect-Associate
AWS SAA 시험 관련 개념 정리 (본 모든 정보는 AWS FAQ 및 docs를 기반으로 재구성)
지속적으로 업데이트 할 것


## 1. AWS 기본 용어
###  Region  

 - AWS에서 사용하는 일종의 IDC의 집합으로서 거의 모든 클라우드 서비스가 탑재되는 곳으로 다수의 Availability Zone으로 구성됨  
 - 한 곳의 AZ가 기능마비되더라도 다른 AZ가 기능을 수행함  
 - 전세계 주요 대도시에 분포되어있음(서울 포함)  
 - AWS 사용자는 각 Region마다 별도의 클라우드망을 구성할 수 있음

###  Availability Zone  

 - 가용 영역이라 불리우며 IDC, 즉 데이터센터의 역할을 함  
 - 보통 3~4개의 AZ가 각 Region마다 존재  
 - 하나가 파괴더라도 다른 AZ가 기능을 수행함  
 - 네트워크 구성에서 하나의 서브넷은 하나의 AZ를 의미  

###  On-premise  

 - 클라우드 방식이 아닌 자체적으로 보유한 전산실과 같은 인프라를 의미  

###  Edge Location  

 - CDN 서비스인 Cloudfront가 사용하는 캐시서버  
 - 전세계에 분포되어있으며 Edge Location끼리 데이터를 공유함  

###  탄력성(Elasticity)과 확장성(Scalability)  

 - 탄력성은 AWS의 상징과도 같은 서비스인 Autoscaling처럼 단시간에 갑작스러운 요구에 따라 사용량을 늘리거나 줄이는 것을 의미(Scale out)  
 - 확장성은 장기적인 관점, 즉 아키텍쳐적인 관점에서 예상치를 충분히 감당할 수 있는 리소스를 보유하는 것을 의미(Scale up)  

  

## 2. EC2  
###  Elastic Compute Cloud  
  
- **가상 서버를 제공하는 서비스**  
- 실제 물리서버와 똑같은 형태의 서비스를 제공하며 Linux나 Window 같은 기본 운영체제가 설치되어있음  
- SSH로 원격 연결이 가능함(공인 IP가 있는 경우 직접 연결 가능)  
- 기본 동작으로는 시작, 중지,종료,재부팅이 있음  
- 중지가 가능한 디스크 기반 인스턴스인 EBS 기반 인스턴스와 임시 스토리지를 제공하여 중지가 불가능한 Instance Store 기반 EC2로 나뉨  
- 재부팅의 경우 EBS 기반 EC2와 인스턴스 스토어 기반 EC2 모두 사용 가능하나 중지는 EBS 기반 EC2만 가능(중요!)  
- 인스턴스 유형으로는 범용, 컴퓨팅 최적화, 메모리 최적화, 스토리지 최적화 등이 있음  

###  EC2 상태  
  
- **Pending** : 인스턴스가 구동하기 위해 준비중인 상태, 요금 미청구  
- **Running** : 인스턴스를 실행하고 사용할 준비가 된 상태, 요금 청구  
- **Stopping** : 인스턴스가 중지 모드로 전환되려는 상태, 요금 미청구  
- **Shutting-down** : 인스턴스가 종료할 준비중인 상태, 요금 미청구  
- **Terminated** : 인스턴스가 종료된 상태, 요금 미청구  

###  EC2 구매 옵션  
  
- **온디맨드(On Demand)** : 필요할 때 바로 생성하여 사용하는 방식으로 1시간 단위로 과금이 이루어짐, 1분을 사용하더라도 1시간 과금을 물리는 방식  
- **스팟(Spot)** : 경매 방식의 인스턴스, 최초 생성시 기준가격이 화면에 나타나며 화면의 가격보다 높은 가격을 제시하면 계속 사용이 가능함. 그러나 다른 사람이 더 높은 가격을 입찰했다면 인스턴스가 종료됨. 불시에 중단되어도 상관없거나 각종 테스트에 적합  
- **예약(Reserved)** : 12개월 또는 36개월 단위로 예약하여 사용하는 인스턴스로 온디맨드에 비해 가격이 대폭 할인됨. 장기적으로 사용할 경우 추천, 예약 인스턴스이기 때문에 사용하지 않더라도 요금이 부과됨  

###  AMI  
  
- **Amazon Machine Image**  
- 인스턴스를 시작하는데 필요한 정보를 제공하는 소위 ‘**이미지**’  
- AMI를 지정하여 인스턴스를 생성할 수 있음  
- EBS 지원 AMI와 인스턴스 스토어 지원 AMI로 나뉨  
- EBS 지원 AMI는 후술할 EBS 스냅샷에서 루트 디바이스 스토리지가 생성되며, 인스턴스 스토어 지원 AMI는 S3에 저장된 템플릿에서 생성된 스토어   볼륨을 사용함  
- AMI와 연결된 스냅샷은 지울 수 없음  
- 다른 리전으로 복사가 가능하며 권한을 준다면 다른 계정과도 공유 가능  
- AWS Market Place에서 다른 사람이 만들어둔 AMI를 쓰거나 공유 가능  

###  Elastic IP  
  
- **EC2에 설정되는 네트워크 인터페이스의 공인 IP**  
- EC2가 기본적으로 갖는 Public IP와 Private IP 등과는 구분해야함  
- Elastic IP를 사용하면 EC2로 하여금 중지되었다가 다시 시작하더라도 고정된 공인 IP를 사용하게 할 수 있음  
- Elastic IP는 계정 내 리전당 최대 5개까지 보유 가능하며 그 이상 필요시 AWS에 요청해야 함  
- 프리티어의 경우, 사용 중이 아닐 땐 요금이 부과됨  

###  Key Pair  
  
- EC2는 SSH 접속시 'key pair'라고 하는 퍼블릭 키 암호화 기법을 사용하여 접속하며 로그인 정보를 암호화 및 해독함  
- Key Pair 분실시 접속 불가  
- 또한 접속시 OS 별로 접속 name이 다름(Linux : EC2-USER 등)  

###  Batch Group  
  
- **클러스터** : 인스턴스를 AZ 내에서 근접하게 배치함. 결합된 노드간 낮은 지연 시간의 네트워크 달성 가능  
- **파티션** : 인스턴스가 담긴 그룹을 논리 세그먼트로 나누어 각 파티션에 배치함. 최대 7개의 파티션을 가질 수 있으며, 각 파티션은 자체     랙 세트를 보유하고 자체 네트워크와 전원을 보유함  
- **분산** : 파티션이 논리 세그먼트로 분리된 인스턴스 그룹인 것과 달리 분산은 **‘인스턴스’** 개체 하나가 자체 랙에 분산 배치됨. AZ당 최대 7개의 인스턴스 배치 가능  

  
## 3. EBS(Elastic Block Store)  
  
###  Elastic Block Store  
  
- **EBS 지원 EC2가 갖는 블록 형태의 스토리지**  
- 애플리케이션의 기본 스토리지로 쓰거나 시스템 드라이브용으로 쓰기 적합  
- 인스턴스 생성시 루트 디바이스 볼륨이 생성되며 사용중에는 언마운트할 수 없음  
- 또한 인스턴스는 여러 볼륨을 마운트할 수 있고 추가볼륨에 대해서는 사용중이라도 마운트/언마운트가 가능  
- 볼륨은 여러 인스턴스에 마운트할 수 없음  
- EBS를 특정 AZ에서 생성하더라도 다른 AZ의 인스턴스에 즉시 붙일 수 있음  
- 인스턴스 스토어 볼륨과는 달리 EBS 기반 인스턴스는 중지 / 재시작이 가능함  
- 사용중인 EBS더라도 볼륨 유형과 사이즈를 변경할 수 있음(사이즈 축소는 불가)  

###  EBS 볼륨 유형(중요!)  
  
- **범용 SSD(gp2)** : 시스템 부트 사용 가능, 대부분의 워크로드에서 사용  
- **프로비져닝된 IOPS SSD(io1)** : 지속적인 IOPS 성능이나 16,000 IOPS 이상의 볼륨당 처리량을 필요로 하는 경우 적합(DB 워크로드)  
- **처리량 최적화된 HDD(st1)** : 시스템 부트 사용 불가능, IOPS가 아닌 처리량을 기준으로 하며 자주 액세스하는 워크로드에 적합한 저비용 HDD 볼륨, 빅데이터나 데이터 웨어하우스에 사용  
- **Cold HDD(sc1)** : 시스템 부트 사용 불가능, 자주 액세스하지 않는 대용량 데이터 처리에 적합, 스토리지 비용이 최대한 낮아야 할 경우 사용  

###  스냅샷과 EBS  
  
- 스냅샷을 이용하여 EBS 볼륨의 데이터를 S3에 저장할 수 있음, 증분식 백업이기 때문에 스냅샷은 하나이며 마지막 스냅샷 이후 변경된 블록만 추가적으로 저장됨  
- 각 스냅샷에는 스냅샷을 만든 시점의 데이터를 새 EBS 볼륨에 복원하는 데 필요한 정보가 들어 있음  
- EBS의 스냅샷은 S3에 저장되며 S3에 저장된 스냅샷으로 EBS 볼륨 복구 가능  
- 암호화된 볼륨의 스냅샷은 자동으로 암호화됨  
- 암호화된 스냅샷에서 생성되는 볼륨은 자동으로 암호화됨  
- 암호화되지 않은 EBS에서 암호화된 스냅샷은 생성할 수 없음  

###  EBS 암호화  
  
- EBS를 암호화함으로써 부팅 및 데이터 볼륨을 모두 암호화할 수 있음  
- 다음 유형의 데이터가 암호화됨  
    - 볼륨 내부 데이터  
    - 볼륨과 인스턴스 사이에서 이동하는 데이터  
    - 볼륨에서 생성된 모든 스냅샷  
    - 스냅샷에서 생성된 모든 볼륨(스냅샷이 암호화되면 해당 스냅샷에서 생성된 볼륨은 자동 암호화)  
- AES-256 알고리즘 사용  

### • RAID 구성  
  
- **RAID 0** : I/O 성능이 내결함성보다 더 중요한 경우  
- **RAID 1** : 내결함성이 I/O 성능보다 더 중요한 경우  

  
  
## 4. S3(Simple Storage Service)
  
###  Simple Storage Service  
  
- **웹서비스 인터페이스(HTTP)를 이용하여 웹에서 언제 어디서나 원하는 양의 데이터를 저장하고 검색할 수 있는 스토리지**  
- 버킷(Bucket)과 객체(Object)로 나뉘며, 저장하고자 하는 모든 요소는 하나의 객체로 저장되고, 오브젝트를 담는 곳이 바로 버킷
- S3 자체는 글로벌 서비스이지만 버킷을 생성할 때에는 리전을 선택해야 함
- 객체는 객체 데이터와 메타 데이터로 나뉘며, 각자의 고유한 URL을 가지며 해당 URL로 접속 가능
  

###  버킷(Bucket)의 정의와 특징  
  
- **객체를 담고 있는 구성요소**
- 크기는 무제한이며, 리전을 지정하여 버킷을 생성해야 함
- 버킷의 이름은 반드시 고유해야하며 중복될 수 없음
- 한 번 설정된 버킷의 이름은 다른 계정에서 사용할 수 없음
  
  
###  객체(Object)의 정의와 특징  
  
- **S3에 업로드되는 1개의 데이터를 객체라 함**
- 키, 버전 ID, 값, 메타데이터 등으로 구성됨
- 객체 하나의 최소 크기는 1(0) byte ~ 5TB
- 스토리지 클래스, 암호화, 태그, 메타데이터, 객체 잠금 설정 가능
- 객체의 크기가 매우 클 경우 멀티파트 업로드를 통해 신속하게 업로드 가능

###  객체의 스토리지 클래스(중요!!) 
  
- 객체의 접근빈도 및 저장기간에 따라 결정되는 객체의 특성
- **Standard Type** : 클래스를 선택하지 않을 경우 선택되는 일반적인 클래스
- **Standard_IA(Ifrequent Access)** : 자주 액세스하지는 않지만 즉시 액세스할 수 있는 데이터여야 하는 경우 선택되는 클래스
- **One Zone_IA** : Standard_IA와 기능은 동일하나 Standard_IA의 경우 세 곳의 AZ에 저장되는 것과 달리 한 군데의 AZ에만 저장되어 해당 AZ가 파괴될 경우 정보 손실 가능성 존재(저장요금이 적음)
- **Intelligent tiering** : 액세스 빈도가 불규칙하여 빈도를 가늠하기 어려운 경우 선택되는 클래스
- **Glacier** : 검색이 아닌 저장이 주용도인 스토리지로 저장요금이 위 클래스들보다 훨씬 저렴함. 다만 저장이 주용도이기 때문에 검색에 3~5시간이 걸림
- **Glacier Deep Archive** : 10년 이상 저장할 데이터를 저장하는 스토리지 클래스

![S3-class](https://user-images.githubusercontent.com/46843064/74656182-fabd9f80-51d0-11ea-9e86-c1a328389a3e.png)  


### 멀티 파트 업로드
- **오브젝트의 크기가 클 경우, 이를 조각내어 병렬적으로 처리하여 처리량을 개선하는 방법**
- 보통 객체의 크기가 100MB 이상일 경우 사용하는 것을 권고하며 최대 가능 크기는 5TB
- 조각의 갯수는 최대 1만개까지 가능하며, 조각의 크기는 대개 5MB~5GM 정도임

### 버전관리(중요!)
- **동일한 객체에 대해 여러 버전을 가질 수 있도록 하는 기능**
- 이미 S3에 존재하는 객체에 내용을 변경하여 업데이트할 경우 기존 버전이 사라지지 않고 이전 버전으로 존재할 수 있음
- 최신 버전과 이전 버전 모두 확인 가능
- 객체를 삭제하더라도 바로 삭제되는 것이 아닌 삭제 마커를 붙여 **다시 복구할 수 있는 기능 제공**

### 수명주기 관리(중요!)
- S3에 있는 오브젝트를 **일정 시간 후에 다른 타입으로 변경하는 것**을 의미
- 예를 들어, 자주 사용하던 Standard Type의 오브젝트를 60일 이후 더 이상 사용하지 않을 경우 Glacier Type으로 변환하여 비용을 줄일 수 있음
- 현재 버전과 이전 버전에 대해 설정 가능
- 또한 완료되지 않은 멀티파트 업로드와 버전관리가 적용된 오브젝트의 삭제 마커를 정리할 수 있음
- 더 이상 사용하지 않은 오브젝트에 대해 만료기간을 설정하여 정한 기간 후 삭제하도록 설정 가능(만료 기간 설정)
- Standard-IA와 One Zone IA는 보관 후 30일 이후 이전 가능

### 정적 웹 사이트 호스팅
- S3로 하여금 정적 페이지를 제공할 수 있는 호스팅 기능을 제공하게 하는 것
- 활성화 후 특정 페이지를 지정하면 S3 접속시 해당 페이지를 띄움
- 굳이 서버를 이용하지 않고 S3를 이용하여 웹 호스팅을 할 수 있음
- 호스팅 뿐만 아니라 요청을 다른 버킷 혹은 도메인으로 리디렉션 가능

### 기본 암호화(중요!)
- 암호화는 Client side와 Server side로 나뉘며 Client side는 Client에서 S3로 전송될 때의 암호화(data at transit)을, Server side는 S3에 저장될 때의 암호화(data at rest)를 의미함
### Server Side Encryption
- SSE-S3 : S3의 고유한 키로 암호화를 실시하며 암호화 주체가 S3가 되는 방식. 암호 알고리즘은 AES-256.
- SSE-KMS : Key Management Service를 이용하여 객체를 암호화하는 방식으로 KMS 고객 마스터 키(CMK)를 활용함. SSE-S3와 달리 고객에 키를 제어할 수 있음 
- SSE-C : 고객(Customer)가 제공하는 키로 암호화를 진행하는 방식으로 제공된 암호화 키를 사용하여 디스크를 쓰거나 해독할 때 객체에 액세스할 때의 모든 암호화를 관리함. 제공된 암호화키는 저장되지 않음

### Client Side Encryption
- S3로 데이터를 보내기 전의 암호화
- KMS에 저장된 고객 마스터키를 사용하여 암호화
- 애플리케이션 내 마스터 키를 사용하여 암호화


### 전송 속도 향상(Transfer Acceleration)
- Cloudfront의 Edge Location을 이용하여 **파일 업로드를 보다 빠르게 하는 기능**
- S3로 직접 업로드하는 것이 아닌 가장 가까운 Edge Location으로 전송하고 아마존 백본 네트워크를 통해 S3로 도달


## 5. Cloudfront

### Cloudfront의 개요와 Edge Location◦CDN(Content Delivery Network)
- **간단히 말하여 HTTP / HTTPS를 이용하여 S3 및 ELB, EC2, 외부 서버 등을 캐시하고 보다 빠른 속도로 콘텐츠를 전달하는 캐시 서버**
- 전 세계 각지에 퍼져 있는 Edge Location의 주변 Origin Server의 콘텐츠를 Edge Location에 캐싱하고 각 Edge Location간 공유를 통해 콘텐츠를 전달
- Distribution은 Edge Location의 집합을 의미
- 각 Edge Location간에는 아마존의 백본 네트워크를 통하기 때문에 매우 빠른 속도로 전달 가능
- 위에서 언급한 것처럼 S3, ELB, EC2 등의 AWS 서비스뿐만 아니라 외부의 서버도 캐싱 가능(이를 Custom Origin이라 함)
- TTL을 조절하여 캐시 주기를 통제할 수 있음

### 콘텐츠 제공 방법
  1. 사용자가 웹 사이트 혹은 앱에 액세스하고 이미지 혹은 HTML 파일을 요청함(정적 데이터)  
  2. DNS가 요청을 최적으로 서비스할 수 있는 Cloudfront Edge Location으로 요청을 라우팅함  
  3. Edge Location에서 해당 캐시에 요청된 파일이 있는지 확인하고 없으면 오리진 서버에 요청하여 확보 후 전달, 그리고 캐시 적재  

![Image-2-2](https://user-images.githubusercontent.com/46843064/74655842-62271f80-51d0-11ea-8304-837adc311ed3.png)  

### OAI(Origin Access Identity) 
- S3를 오리진 서버로 사용시, **Cloudfront를 제외하고 다른 경로로 S3를 접근하는 것을 막는 방법**
- OAI를 설정하게 되면 각각의 Distribution이 별도의 Identity를 갖게 되며, S3의 버킷 정책을 수동 혹은 자동으로 수정할 수 있음
- OAI가 적용된 S3의 버킷정책은 다음과 같이 수정됨


``` javascript
{
“Version”: “2008-10-17”,
“Id”: “PolicyForCloudFrontPrivateContent”,
“Statement”: [
 {
“Sid”: “1”,
“Effect”: “Allow”,
“Principal”: {
“AWS”: “arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity xxx”
},
“Action”: “s3:GetObject”,
“Resource”: “arn:aws:s3:::bucket-name/*”
}
 ]
 }
 ```
### Presigned URL
- **인증된 사용자만이 해당 Distribution를 사용할 수 있도록 제어하는 기능**
- 만료 날짜 및 시간까지 설정 가능
- Cloudfront 설정시 Presigned URL 사용과 Cloudfront Key Pair를 계정의 보안자격증명에서 생성해야 함
- 이를 조합하여 URL 서명을 생성하고 해당 URL을 통해 Cloudfront에 접근할 수 있음 


## 6. RDS

### Relational Database Service란?
- **관계형 데이터베이스를 AWS 상에서 사용할 수 있도록 지원하는 서비스, 쉽게 말해 Database 서비스**
- 생성 후 서비스를 이용하기만 되므로 SaaS에 해당
- MySQL, MariaDB, Postgre SQL, Oracle, MS SQL, Aurora 사용 가능
- DB 인스턴스에 대한 Shell 지원 불가 및 OS 제어 불가능(AWS 관리)
- 백업, 소프트웨어 패치, 장애 감지 및 복구를 AWS가 관리함 
- Storage 용량에 대하여 Auto Scaling 지원

### DB Instance 
- **Instance** : RDS 기본 구성요소로서 클라우드에서 실행하는 격리된 데이터베이스 환경을 의미함. 이 인스턴스 내에서는 여러 사용자가 만든 데이터베이스가 포함되며 액세스할 여러 도구와 앱 사용 가능.
- DB 인스턴스도 EC2처럼 다양한 클래스를 가지고 있음(db.m5, db.r5 등)
- 앞서 말한 것처럼 RDS도 클라우드에서 실행되기 때문에 하나의 AZ에서 격리되어 인스턴스로서 실행

### DB Instance Storage
- **Instance Storage** : 데이터베이스 유지를 위해 EBS(Elastic Block Storage)를 사용하며 필요한 스토리지 용량에 맞춰 자동으로 데이터를 여러 EBS 볼륨에 나누어 저장
- 스토리지 유형
   - **범용 SSD** : 대부분의 워크로드에서 사용하는 무난한 스토리지
   - **프로비져닝 IOPS** : 빠르고 일관적인 I/O 성능이 필요하고 일관적으로 낮은 지연시간이 요구될 경우 사용하는 스토리지 (I/O : Input / Output)
   - **마그네틱** : 접속 빈도가 적은 워크로드에 적합한 스토리지


### Multi-AZ(중요!)
- RDS는 Multi-AZ라는 기능을 통해 고가용성을 지원함
- 말그대로 **다수의 AZ에 DB 인스턴스를 둠으로써 하나 혹은 그 이상의 AZ가 파괴되어 서비스가 불가능할 때를 대비함**
- 또한 기본 인스턴스가 수행해야 할 작업(백업, 스냅샷 생성) 등을 대신하여 수행함으로서 **기본 인스턴스의 부담을 줄임**
- 기본 인스턴스에서 스냅샷을 캡쳐한 후 다른 AZ에 복원하여 ‘동기식’ 예비 복제본을 생성함
- 즉, Active(AZ A)-Standby(AZ B,C) 구조를 형성한 후 지속적으로 동기화함
- ‘예비’ 복제본이기 때문에 읽기 및 쓰기 작업을 수행할 수 없음(중요!)
- Multi-AZ를 사용하는 경우, 단일 AZ 배포에 비해 쓰기 및 저장 지연 시간이 길어질 수 있음(Standby에 데이터를 동기화해야 하기 때문)
- Multi-AZ를 활성화한 상태에서 DB 인스턴스에 문제가 발생하면 자동으로 다른 AZ의 예비 복제본(Standby)로 전환하며 서비스를 이어나감
- 전환에 사용되는 시간은 약 60-120초
- 전환되는 상황
   - 가용 영역(AZ) 중단
   - 기본 DB 인스턴스 오류
   - DB 인스턴스 서버 유형 변경
   - 기본 DB 인스턴스 OS에서 소프트웨어 패치 실시
   - 장애 조치 재부팅(Failover) 실시


### Read Replica(중요!)
- **읽기 전용 복제본**
- 기본 DB 인스턴스가 읽기와 쓰기를 담당한다면 **Read Replica는 읽기 작업만을 담당하여 마스터 DB 인스턴스의 부하를 줄임**
- 우선 DB 인스턴스의 스냅샷을 캡쳐한 후, 이를 기반으로 Read Replica를 생성하며, 데이터를 ‘비동기’ 복제 방식을 통해 업데이트함
- MySQL, Oracle, PostgreSQL, Maria DB에서 사용 가능
- 다른 리전에도 Read Replica를 두는 것이 가능
- 리전당 최대 5개까지 두는 것이 가능
- Read Replica 또한 독립된 인스턴스로 승격 가능


### Automated Backup
- **RDS의 자동백업으로 개별 데이터베이스를 백업하는 것이 아닌 DB 인스턴스 전체를 백업하는 것**
- 매일매일 백업이 이루어지며, 기본 보존기간은 CLI로 생성시 1일 & 콘솔로 생성시 7일이며 최저 1일부터 35일까지 가능
- 특정시점을 지정하여 복원가능하며 복원 기간내로부터 최근 5분까지 특정시점을 지정하여 복원 가능
- 사용자가 지정한 백업시간에 자동적으로 백업되며, 백업 중에는 스토리지 I/O가 일시적으로 중단될 수 있음(Multi-AZ 사용시 Standby에서 백업 실시)

### Snapshot
- **DB 인스턴스의 특정시점을 스냅샷으로 생성하는 것**
- 자동백업과 마찬가지로 스냅샷 역시 자동으로 생성가능하며, 수동으로도 생성 가능
- 자동백업과는 달리 스냅샷 생성시점으로만 복원가능
- 스냅샷으로 복원시 DB 인스턴스를 복원하는 것이 아닌 개별 DB 인스턴스가 생성됨( DB 스냅샷에서 기존 DB 인스턴스로 복원할 수 없으며, 복원하면 새 DB 인스턴스가 생성됨) 
- 스냅샷 복사, 공유, 마이그레이션이 가능함
- 스냅샷 생성 중에는 스토리지 I/O가 일시적으로 중단될 수 있음(Multi-AZ 사용시 Standby에서 백업 실시) 

### Enhanced Monitoring
- **RDS의 지표를 실시간으로 모니터링하는 ‘강화된’ 모니터링**
- 모니터링 지표는 CloudWatchs Logs에 30일간 저장됨
- 일반 모니터링과의 차이점은 Enhanced Monitoring은 인스턴스 내 에이전트를 통해 지표를 수집하는 반면, 일반 모니터링은 하이퍼바이저에서 수집한다는 점
- 최대 1초단위까지 수집 가능

### RDS vs DB in EC2
- **RDS**
   - 백업 및 소프트웨어 패치 등 서비스 유지에 필요한 기능을 AWS가 관리
   - SaaS이므로 서비스 최적화에 집중할 수 있음
   - SSH를 통한 접속이 불가하며, EC2를 통한 접속 혹은 Workbench와 같은 접속 프로그램을 통해서만 가능
   - 설정을 변경할 수 있는 파라미터 그룹이 존재하지만 변경 불가능한 설정도 존재

- **DB in EC2**
   - EC2 위에 데이터베이스를 직접 올리는만큼 설정을 마음대로 변경할 수 있고 커스터마이징 또한 가능
   - RDS와는 반대로 백업과 패치 등 관리를 직접해야 함
   - EC2에 설치하는 것이기에 SSH 접속 가능


### Amazon Aurora
- ‘클라우드에서 데이터베이스를 처음부터 설계하면 어떨까’라는 생각에서 출발한 DB 서비스
- MySQL과 PostgreSQL과 호환 가능함
- 각 AZ마다 2개의 데이터 복사본을 자동으로 유지하며, 에러를 스스로 찾아내고 복구함
- Read Replica는 다른 DB 서비스와 달리 최대 15개까지 가능하며, 백업과 스냅샷이 퍼포먼스에 영향을 주지 않음


## 7. VPC

### VPC 란?
- **AWS의 계정 전용 가상 네트워크 서비스**
- VPC 내에서 각종 리소스(EC2, RDS, ELB 등)을 시작할 수 있으며 다른 가상 네트워크와 논리적으로 분리되어 있음
- S3, Cloudfront 등은 비VPC 서비스로 VPC 내에서 생성되지 않음
- 각 Region별로 별도의 VPC가 다수 존재할 수 있음
- VPC는 하나의 사설 IP 대역을 보유하고, 서브넷을 생성하며 사설 IP 대역 일부를 나누어 줄 수 있음
- 허용된 IP 블록 크기는 /16(IP 65536개) ~ /28(IP 16개)
- 권고하는 VPC CIDR 블록(사설 IP 대역과 동일함)
   - 10.0.0.0 ~ 10.255.255.255(10.0.0.0/8)
   - 172.16.0.0 ~ 172.31.255.255(172.16.0.0/12)
   - 192.168.0.0 ~ 192.168.255.255.(192.168.0.0/16)


### Subnet
- **VPC 내 생성된 분리된 네트워크로 하나의 서브넷은 하나의 AZ(Availability Zone)에 연결됨**
- VPC가 가지고 있는 사설 IP 범위 내에서 ‘서브넷’을 쪼개어 사용가능
- 실질적으로 리소드들은 이 서브넷에서 생성이 되며 사설 IP를 기본적으로 할당받고 필요에 따라 공인 IP를 할당받음
- 하나의 서브넷은 하나의 라우팅 테이블과 하나의 NACL(Network ACL)을 가짐
- 서브넷에서 생성되는 리소스에 공인 IP 자동할당 여부를 설정할 수 있음
   - 이 기능을 통해 Public Subnet과 Private Subnet을 만들어 커스터마이징 가능
   - 서브넷 트래픽이 후술할 인터넷 게이트웨이로 라우팅 되는 경우 해당 서브넷을 Public Subnet, 그렇지 않은 서브넷의 경우 Private Subnet이라 함
- 각 서브넷의 CIDR 블록에서 4개의 IP 주소와 마지막 IP 주소는 예약 주소로 사용자가 사용할 수 없음, 예를 들어 서브넷 주소가 172.16.1.0/24일 경우
   - 172.16.1.0 : 네트워크 주소(Network ID)
   - 172.16.1.1 : VPC Router용 예약 주소(Gateway)
   - 172.16.1.2 : DNS 서버의 IP 주소
   - 172.16.1.3 : 향후 사용할 예약 주소
   - 172.16.1.255 : 네트워크 브로드캐스트 주소(VPC는 브로드캐스트를 지원하지 않음)


### ENI(Elastic Network Interface)
- **가상 네트워크 인터페이스**
- VPC 내 리소스들은 ENI와 사설 IP를 기본적으로 할당받음
   - 선택적으로 공인 IP도 할당 가능
   - Public Subnet의 경우, 리소스 생성시 자동으로 공인 IP를 할당함
- 사설 IP 주소는 추가로 할당 가능하며 공인 IP 역시 나중에 할당 가능

### Routing Table
- **서브넷 내의 트래픽이 전송되는 위치를 결정하는 라우팅의 규칙 집합**
- 라우팅 테이블은 기본적으로 VPC의 범위에 해당하는 범위를 기본 라우팅으로 가지며(ex. 172.16.1.0/24) 이를 ‘Local’로 표시함
- 또한 Internet Gateway, NAT Gateway, VPC Endpoint, Peering 등을 설정하고 그 서비스로 트래픽을 보내도록 라우팅 설정할 수 있음
- ‘0.0.0.0/0’은 default routing을 뜻하며 트래픽이 가고자 하는 목적지가 라우팅 테이블에 없는 경우(ex. 172.16.1.0/24 네트워크에서 외부 인터넷으로 나아가고자 할 때) 사용하는 라우팅이며 보통 Internet Gateway나 NAT Gateway로 외부 인터넷을 지정할 때 씀
- 하나의 서브넷은 하나의 라우팅 테이블만 가지지만, 하나의 라우팅 테이블은 다수의 서브넷을 가질 수 있음
- Public Subnet의 라우팅 테이블에는 인터넷 게이트웨이로의 라우팅이 있으며, Private Subnet에서 외부 인터넷 통신이 필요할 경우 NAT Gateway로의 라우팅이 잡혀 있음

![Image-10](https://user-images.githubusercontent.com/46843064/74655614-fb096b00-51cf-11ea-8f49-a6ec58cc1afe.png)  
<VPC의 한 형태(출처 : AWS docs)>

### Internet Gateway
- **VPC 내 리소스가 외부 인터넷을 사용하고자 할 때 사용하는 게이트웨이**
- 인터넷 게이트웨이가 없으면 외부 인터넷을 사용할 수 없음
- 인터넷 게이트웨이를 생성한 후, ‘0.0.0.0/0’에 대하여 라우팅 테이블을 인터넷 게이트웨이로 잡아주면 사용 가능
- 다만 인터넷 게이트웨이가 있다 하더라도, VPC 내 리소스가 공인 IP를 가지고 있지 않다면 인터넷 사용 불가능
- 또한 위의 설정을 모두 했음에도 인터넷이 제대로 되지 않는다면, 보안그룹과 Network ACL을 확인해야 함

![Image-11](https://user-images.githubusercontent.com/46843064/74655695-1eccb100-51d0-11ea-8622-cc1526b3d16f.png)  
<Internet Gateway의 설정(출처 : AWS docs)>

### NAT Gateway
- **외부에서의 접촉이 원천적으로 차단되어 있는 Private Subnet에서 인터넷 접속을 통해야 할 경우 사용하는 게이트웨이**
- VPC 내부 리소스가 NAT Gateway를 통해 인터넷을 접속할 수 있지만, 외부에서 NAT Gateway를 통해 VPC 내부로 들어올 수 없음
- 인터넷이 연결된 Public sunbet에 NAT Gateway(EIP를 갖고 있음)를 생성한 후, Private Subnet의 라우팅 테이블에 ‘0.0.0.0/0’에 대하여 라우팅을 NAT Gateway로 잡아주면 사용 가능
- CloudWatch를 이용하여 모니터링 가능


![Image-12](https://user-images.githubusercontent.com/46843064/74655762-3f950680-51d0-11ea-8ed1-528eb039f627.png)  
<NAT Gateway의 설정(출처 : AWS docs)>

### NAT Instance
- **Public Subnet에 생성된 NAT Gateway 대신 EC2 인스턴스를 사용하는 방법**
- Public Subnet에 공인 IP를 가진 특수한 인스턴스를 게이트웨이로 삼고, Private Subnet의 라우팅 테이블에 ‘0.0.0.0/0’에 대하여 NAT Instance로 설정한 후, SrcDestCheck 속성을 비활성화해야 함
- 커뮤니티 AMI에 있는 ‘ami-vpc-nat’로 시작하는 인스턴스로 사용 가능
- 인스턴스이기 때문에 후술할 보안그룹의 설정을 적용 받으므로 트래픽을 제어할 수 있음

### NAT Instance vs NAT Gateway
- NAT Gateway는 AWS에서 관리하기 때문에 유지보수할 필요가 없으나 NAT Instance는 사용자가 직접 관리해야 함
- NAT Instance는 인스턴스이기 때문에 장애가 발생할 가능성이 있어 스크립트로 인스턴스간 Failover를 신경써야 함
- NAT Gateway는 대역폭을 최대 45Gbps까지 확장할 수 있지만, NAT Instance는 인스턴스 유형에 따라 다름
- NAT Gateway는 보안그룹을 사용할 수 없지만, NAT Instance는 보안그룹을 사용할 수 있음
- NAT Gateway와 NAT Instance 모두 NACL을 통해 트래픽 제어 가능


### Security Group
- **VPC 내 인스턴스에 대한 인바운드 및 아웃바운드 트래픽을 제어하는 가상의 방화벽**
- 서브넷 수준이 아닌 인스턴스 수준에서 작동하기 때문에 각 인스턴스를 서로 다른 보안그룹으로 지정할 수 있음
- 기본적으로 모든 인바운드 트래픽을 거부하고 모든 아웃바운드 트래픽을 허용
- 인스턴스당 최대 5개의 보안그룹을 설정할 수 있음
- 보안그룹의 가장 큰 특성은 이른바 ‘Stateful’로 인바운드 트래픽과 아웃바운드 트래픽에 별도의 규칙을 지정할 수 있는 것
- 즉 인바운드에는 HTTP(80) 포트가 허용되어 있으나, 아웃바운드에 없는 경우 HTTP 트래픽이 ‘외부에서 들어왔다가 나갈 때’ 전혀 지장이 없음
- 허용 규칙만 존재하며 거부 규칙이 존재하지 않음
- Source IP, Protocol, Port 등을 설정할 수 있음
- 설정 변경시 즉시 적용됨

![Image-13-1024x550](https://user-images.githubusercontent.com/46843064/74655176-466f4980-51cf-11ea-91e5-3c155df427a8.png)  
<보안그룹의 예시(출처 : AWS docs)>

### Network ACL
- **서브넷 내부와 외부의 트래픽을 제어하기 위한 가상 방화벽으로 네트워크 스위치의 ACL과 역할이 같음**
- 서브넷의 가상 방화벽이기 때문에 서브넷에 속한 모든 인스턴스가 영향을 받음
- 기본적으로 모든 인바운드와 아웃바운드 트래픽을 허용함
- 하나의 ACL은 다수의 서브넷과 연결될 수 있으며, 하나의 서브넷은 하나의 ACL만 연결됨
- Network ACL의 가장 큰 특징은 ‘Stateless’로서 인바운드 규칙과 아웃바운드 규칙이 서로 영향을 줌
- 즉, 인바운드에는 HTTP(80) 포트가 허용되어 있으나, 아웃바운드에 없는 경우 HTTP 트래픽이 ‘외부에서 들어왔다가 나갈 때’ 아웃바운드 통신이 되지 않음 
- 허용과 거부 모두 가능
- Security Group과 달리 우선순위 값이 존재하며 가장 작은 값이 가장 높은 우선순위를 가지고 우선순위부터 순서대로 적용됨
- 변경 사항은 잠시 후 적용됨

![Image-3-1024x509](https://user-images.githubusercontent.com/46843064/74655403-a36aff80-51cf-11ea-9f32-c7b73846c900.png)  
<Network ACL의 예시(출처 : AWS docs)> 

### Security Group vs Network ACL(중요!)
- Security Group은 ‘Stateful’, Network ACL은 ‘Stateless’
- Security Group은 허용만 가능, Network ACL은 허용 및 거부 가능
- Security Group은 규칙 리스트에 있는 것 중 적용, Network ACL은 우선순위에 따라 우선 규칙 적용
- Security Group은 인스턴스의 가상 방화벽, Network ACL은 서브넷의 가상 방화벽

### VPC Peering
- **두 VPC 간의 트래픽을 전송하기 위한 기능**
- Source VPC와 같은 / 다른 리전의 VPC를 Destination으로 선택하여 Peering 요청을 보낸 후, 수락시 Peering 가능
- 요청과 수락이 필요한 이유는 다른 계정의 VPC도 연결 가능하기 때문
- Peering 생성 후 라우팅 테이블에 해당 peering을 집어넣으면 통신 시작
- VPC Peering은 Transit Routing 불가(2개의 VPC가 하나의 VPC를 통해 통신하는 것) 

### VPC Endpoint
- **VPC 내 요소들과 비VPC 서비스(S3, Cloudwatch, Athena 등)을 외부인터넷을 거치지 않고 아마존 내부 백본 네트워크를 통해 연결하는 방법**
- 그러므로 후술할 Direct Connect와 같은 전용선 서비스나 VPN, 인터넷 게이트웨이와 같은 외부 연결이 되어 있지 않는 서브넷에서 아마존의 여러 서비스를 연결할 수 있음
- **간단히 말하여 아마존 서비스 전용선**
- VPC 엔드포인트에는 Interface Endpoint, Gateway Endpoint 두 종류가 존재
- **Gateway Endpoint는 S3와 Dynamo DB만 가능(중요!)**

![Image-4](https://user-images.githubusercontent.com/46843064/74655492-cdbcbd00-51cf-11ea-9d88-e9c0b5f798f1.png)  
<VPC Endpoint를 통한 S3로의 연결(출처 : AWS docs)>

### VPN
- **AWS의 IPSEC VPN 서비스**
- 이 VPN을 통해 **AWS와 On-premise의 VPN을 연결하는 것이 가능**
- 고객 측 공인 IP를 뜻하는 고객 게이트웨이와 AWS 측 게이트웨이인 Virtual Private Gateway 생성 후 터널을 생성하면 사용 가능
- 그리고 반드시 터널 쪽으로 라우팅을 생성해야 함

### Direct Connect
- AWS의 전용선 서비스
- 표준 이더넷 광섬유 케이블을 이용하여 케이블 한쪽을 사용자 내부 네트워크의 라우터에 연결하고 다른 한 쪽을 Direct Connect 라우터에 연결하여 내부 네트워크와 AWS VPC를 연결함
- **보통 On-premise의 네트워크와 VPC를 연결할 때 사용**
- VPN보다 더 안전하고 빠른 속도를 보장받고 싶을 때 사용

## 8. Route 53

### Route 53 이란?
- **쉽게 말해 AWS의 DNS 서비스**
   - **도메인 등록**
   - **DNS 라우팅**
   - **상태 체크(Health check)**
- 위 3가지를 담당함
- 도메인 등록시 약 12,000원 정도 지불해야 하며, 최대 3일 정도 걸림
- 해당 도메인을 AWS 내 서비스(EC2, ELB, S3 등)와 연결할 수 있으며 AWS 외 요소들과도 연결 가능함
- 도메인 생성 후 레코드 세트를 생성하여 하위 도메인을 등록할 수 있음
- 레코드 세트 등록시에는 IP 주소, 도메인, ‘Alias’ 등을 지정하여 쿼리를 라우팅할 수 있음 

### Route 53의 라우팅 정책(중요!) 
- **Simple** : 동일 레코드 내에 다수의 IP를 지정하여 라우팅 가능, 값을 다수 지정한 경우 무작위로 반환함
- **Weighted** : Region별 부하 분산 가능, 각 가중치를 가진 동일한 이름의 A 레코드를 만들어 IP를 다르게 줌
- **Latency-based** : 지연시간이 가장 적은, 즉 응답시간이 가장 빠른 리전으로 쿼리를 요청함
- **Failover** : A/S 설정에서 사용됨, Main과 DR로 나누어 Main 장애시 DR로 쿼리
- **Geolocation** : 각 지역을 기반으로 가장 가까운 리전으로 쿼리 수행, 레코드 생성시 지역을 지정할 수 있음
- **Geo-proximity** : Traffic flow를 이용한 사용자 정의 DNS 쿼리 생성 가능
- **Multi-value answer** : 다수의 IP를 지정한다는 것은 simple과 비슷하지만, health check가 가능함(실패시 자동 Failover) 

### Alias(별칭) 
- **AWS만의 기능으로 Route-53 DNS 기능에 고유한 확장명을 제공**
- AWS의 리소스는 도메인으로 이루어져 있는데 이 도메인을 쿼리 대상으로 지정할 수 있도록 하는 기능
- Route 53에서 별칭 레코드에 대한 DNS 쿼리를 받으면 다음 리소스로 응답(즉 다음 리소스로 Alias 지정 가능)
   - Cloudfront Distribution
   - ELB
   - Web Site Hosting이 가능한 S3 Bucket
   - Elastic Beanstalk
   - VPC Interface Endpoint
   - 동일한 Hosting 영역의 다른 Route 53 레코드 

## 9. ELB

### Elastic Load Balancer란?
- AWS의 L4/ L7 로드밸런싱(서버 부하분산) 서비스
- **외부 혹은 내부에서 들어오는 클라이언트의 요청을 ELB에 연결된 가용영역에 있는 EC2로 고르게 나누어 전달하고 등록된 EC2들을 상태 검사하는 서비스**
- 외부/내부 트래픽은 ELB의 DNS 주소를 목적지로 하여 들어오며 이 트래픽을 받은 ELB는 가용영역에 등록된 EC2로 전달
- 보안그룹 적용
- ELB 또한 VPC 내 존재하며 공인 IP 혹은 공인/사설 IP를 모두 가질 수 있음
   - 인터넷 연결 option : 공인/사설 IP 생성
   - 내부 option : 사설 IP 생성
- 기본적으로 Port를 이용하여 로드밸런싱
(예를 들어 HTTP 요청을 로드밸런싱하는 경우, 그 ELB는 HTTP 요청만을 전달하고 다른 포트 요청은 전달하지 않음)
- ‘리스너’를 거느리며 리스너 하나당 요청을 받아들이는 Port 하나씩을 지정함
   - 예를 들어, 80 Port 요청을 받아 들이는 리스너, 443 Port 요청을 받아들이는 리스너를 모두 등록할 수 있음
- 상태 검사에 성공한 EC2만이 요청 전달 대상이 되며 상태 검사에 실패한 EC2는 요청 전달 대상에 제외됨
- SSL 인증서를 등록하여 HTTPS 암호화/복호화 통신을 대신하는 SSL Offload 가능
- ELB는 세 가지 로드밸런서로 구성됨
   - ALB(Application Load Balancer)
   - NLB(Network Load Balancer)
   - CLB(Classic Load Balancer)


### Target Group(대상그룹)
- **ELB에 등록된 EC2 인스턴스들의 그룹**
- 대상 그룹에 등록된 EC2, EC2 상태, 상태 검사 방법 등을 정의
- 상태 검사 방법에는 HTTP, HTTPS, TCP 등이 있음
- 방법뿐만 아니라 상태 검사에 필요한 시간도 정의 가능
- ALB와 NLB는 대상그룹을 지정하며, CLB는 로드밸런서에 직접 등록

### ALB(Application Load Balancer)
- L7 로드밸런서
- **HTTP/HTTPS Header 정보를 기반으로 요청을 전달하는 로드밸런서**
   - 즉 리스너가 HTTP/HTTPS를 전문적으로 다룸
- 요청에 따라 고정 페이지를 반환하거나 다른 경로로 리다이렉트
- HTTP Header / HTTP 요청 메서드 / Host Header / Source IP 등을 이용하여 요청 전달 가능
   - 예를 들어 HTTP Header 내 User-agent에는 Host의 OS 정보가 있는데 Android일 경우, ELB 리스너가 Android만 사용하는 대상 그룹으로 전달 가능
- SSL 인증서를 이용하여 SSL Offload 지원
- 후술할 교차영역 로드밸런싱 지원
- X-forwared-for를 지원하여 EC2로 하여금 클라이언트의 IP를 확인할 수 있도록 함
- ALB가 소유한 공인 IP가 유동적으로 변경됨

### NLB(Network Load Balancer)
- L4 로드밸런서
- **TCP / UDP를 기반으로 요청을 전달하는 로드밸런서**
- 프로토콜, 원본 IP 주소, 원본 포트, 대상 IP 주소, 대상 포트, TCP 시퀀스 번호를 이용하여 요청 전달 
- TCP, UDP, TCP/UDP로 리스너를 설정 가능
- TCP Header를 조작하여 전달하는 것이 아닌 그저 TCP의 기본인 3-way handshake만을 대신하여 진행
- Client IP를 변경하지 않고 서버에게 전달
- NLB의 IP가 ALB와 달리 변경되지 않고 고정됨
- 교차 영역 로드밸런싱 지원

### CLB(Classic Load Balancer)
- **L4/L7을 모두 지원하는 ‘과거의’ 로드밸런서**
- EC2-Classic을 사용하는 경우 CLB를 사용하면 됨
- TCP / HTTP / HTTPS(SSL)을 지원
- X-forwared-for 지원
- 교차영역 로드밸런싱 지원

### Sticky Session
- 세션 고정 기능
- **하나의 요청이 특정 EC2로 들어와 세션이 생성되었다가 종료되고, 들어왔던 요청이 다시 들어올 경우 이미 연결되었던 특정 EC2로 전달하는 기능**

### 교차영역 로드밸런싱
- 기본적으로 ELB는 대상그룹에 등록된 EC2별로 적절히 부하분산을 하는 것이 아닌 가용영역별로 비율을 나눔
   - 즉 교차영역이 2개라면 각 가용영역에 50%씩 전달
- 이렇게 될 경우, A 가용영역에 EC2가 2개이고, B 가용영역에 EC2가 8개일 경우 A/B 가용영역에 50%씩 전달되므로 A 가용영역의 EC2의 부하가 심해짐
- 이를 방지하기 위해 **교차영역 로드밸런싱을 활성화화면 가용영역이 아닌 EC2 갯수를 기반으로 계산하여 전달하므로 부하가 고르게 나누어짐**

### X-Forwared-for
- **Client의 IP를 담고 있는 HTTP Header**
- EC2 내 서비스가 Client의 IP를 확인할 필요가 있는 경우 ELB는 HTTP Header에 X-Forwared-for를 장착하여 전달

### 연결 드레이닝
- Autoscaling과 ELB를 함께 사용시 필요에 따라 EC2가 없어질 경우, 해당 EC2에 요청이 들어와있다면 사용중인 세션이 피해를 볼 수 있음
- **삭제되기 직전에 유휴 세션이 작업을 마칠 때까지 기다리는 기능**


## 10. CloudWatch

### Cloudwatch란?
- **AWS 클라우드 리소스와 AWS에서 실행되는 애플리케이션을 위한 모니터링 서비스**
- 리소스 및 애플리케이션에 대해 측정할 수 있는 변수인 ‘지표’를 수집하고 추적 가능
- 사용중인 모든 AWS 서비스에 대한 지표가 자동으로 표시되며, 사용자 지정 대시보드를 통해 사용자 지정 애플리케이션에 대한 지표를 표시하고 지정 집합 표시 가능
- ‘지표’는 Cloudwatch에 게시된 시간 순서별 데이터 요소 세트이며, 모니터링할 변수(가령 EC2의 CPU 사용량은 EC2가 제공하는 하나의 지표)
- 기본 모니터링과 세부 모니터링으로 나뉘며, 각각 5분과 1분 주기로 수집함
- 기본 모니터링은 자동활성화이지만, 세부 모니터링은 선택사항임
- 기본적으로 **CPU, Network, Disk, Status Check** 등을 수집함
   - **Memory 항목이 없음을 반드시 주의!!!!!!!!**
- 지표 데이터의 보존기간은 다음과 같음(암기 미권장)
   - 기간 60초 미만의 경우, 3시간
   - 기간 60초의 경우, 15일
   - 기간 300초의 경우, 63일
   - 기간 3600초의 경우, 455일
- AWS CLI 혹은 API를 이용하여, Cloudwatch에 사용자 정의 지표 게시 가능
- ‘경보’ 기능을 사용하여 어떤 지표가 일정기간동안 일정값에 도달할 경우 각 서비스가 취해야할 행동을 정의할 수 있음
   - 즉, 모니터링하기로 선택한 측정치가 정의한 임계값을 초과할 경우 하나 이상의 자동화 작업을 수행하도록 구성
   - EC2의 경우, 경보에 따라 인스턴스 중지, 복구, 종료, 재부팅 가능


### Cloudwatch Agent
- **EC2에 Agent를 설치하게 되면 더 많은 시스템 수준 지표를 수집할 수 있음**
- 온프레미스 서버 또한 Cloudwatch Agent 사용 가능
- 여기에 Memory 항목이 포함됨!!
   - 즉 메모리 항목은 사용자 지정 지표로 수집가능하며, 이는 Agent를 통해 수집함
- Cloudwatch Agent는 로그를 수집할 수 있으며, 후술할 Cloudwatch Logs 기능 사용 가능

### Cloudwatch Logs
- **EC2(Agent에서 수집된), CloudTrail, Route 53, VPC Flow log 등 기타 소스에서 발생한 로그 파일을 모니터링, 저장 및 액세스하는 기능**
- 앞에서 언급한 것처럼 Cloudwatch Agent를 사용하여 로그를 수집함
- Cloudwatch Log Insights를 사용하여 CloudWatch Logs에서 로그 데이터를 대화식으로 검색해 분석할 수 있음
- Agent는 기본적으로 5초마다 로그 데이터를 전송함

### Cloudwatch Events
- **AWS 각 서비스의 이벤트가 사용자가 지정한 이벤트 패턴과 일치하거나 일정이 트리거될 경우, 사용자가 원하는 기능을 발동시키도록하는 기능**
- 이벤트 소스와 대상으로 나뉨
   - **이벤트 소스** : AWS 환경에서 발생하는 이벤트이며, 가령 S3의 경우 오브젝트 등록, 삭제 등을 들 수 있음
   - **대상** : 이벤트 발생시 해야 할 행동을 정의하는 것이며, SNS 전송 혹은 람다, SQS 게시 등을 설정할 수 있음
- 즉 이벤트 소스에 해당하는 규칙이 트리거될 경우 대상에 해당하는 서비스를 실행시킴
   - 이벤트가 시스템에 생성해 둔 규칙과 일치하는 경우, AWS Lambda 함수를 자동으로 호출하고, 해당 이벤트를 Amazon Kinesis 스트림에 전달하고, Amazon SNS 주제를 알림 


## 11. Auto Scaling

### Auto Scaling이란?
- **EC2 인스턴스를 자동으로 시작하거나 종료하여 애플리케이션 로드를 처리하기에 적절한 수의 EC2를 유지할 수 있도록 하는 서비스**
- 사용자가 정의하는 조건에 따라 EC2 갯수를 자동으로 확장 또는 축소가 가능함
- 또한 모니터링을 통해 비정상 인스턴스를 탐지하고 교체할 수 있음
- **수요가 급증할 경우 EC2 수를 자동으로 늘려 성능을 유지하고 수요가 적을 경우 수를 줄여 비용을 절감**
- ELB의 대상그룹을 Auto Scaling Group(ASG)에 포함시켜 자동생성된 EC2로 하여금 트래픽 부하분산을 하도록 설정 가능
- 수요 변화가 예측 가능한 경우 ‘예약된 일정’을 통해 정해진 시간에 늘리거나 줄이도록 설정 가능
- ASG 내 손상된 인스턴스가 발견될 경우, Auto Scaling은 이를 자동으로 종료하고 새로운 인스턴스로 교체함
   - ELB를 사용하는 경우, ELB가 손상된 인스턴스를 트래픽 요청 대상에서 분리시킨 후, Auto Scaling이 이를 새로운 인스턴스로 교체함
- 비정상 서버 탐지후 Auto Scaling이 새로운 인스턴스를 In Service 상태로 만들기까지 5분 이내 소요

![Image-1](https://user-images.githubusercontent.com/46843064/74655062-0f993380-51cf-11ea-8ec3-af50a09eb4fb.png)  
<Auto Scaling의 장점(출처 : AWS docs)>

### 시작 구성(Launch Configuration)
- **Auto Scaling에서 새로운 인스턴스를 시작할 때 기반이 되는 구성**
- AMI(Amazon Machine Image), 인스턴스 유형, 보안그룹, 스토리지 등 EC2를 생성할 때와 마찬가지로 옵션을 구성할 수 있음
- **시작 구성은 한 번 생성시 수정이 불가하며, 복사와 삭제만 가능**
- 시작 구성을 변경하기 위해서는 기존의 시작구성을 새로운 시작 구성을 만드는 재료로 사용해야 함
- 하나의 ASG는 하나의 시작구성을 반드시 가짐

### 조정 정책(Scaling)
- **최소, 목표, 최대 크기를 설정할 수 있고, 별다른 정책이 없을 경우 목표 크기를 유지하려 함**
- 또한 3가지 동적 정책 사용 가능
- **대상 추적 정책** : ASG의 지표 평균값을 목표로 인스턴스 수를 조절
   - **평균 CPU 사용률, 네트워크 입/출력, 로드밸런서 요청 수**
- **단순 조정 정책** : 지표의 임계치에 도달할 경우, 사용자가 정한 인스턴스 수를 늘리거나 감소시키는 정책
- **단계 조정 정책** : 단순 조정 정책과 기본은 같지만, 지표 값에 따라 증감 수를 다르게 줄 수 있음(즉 트리거가 여러 개)

### 조정 휴지(Scaling Cooldown)
- **시작 구성을 이용한 ASG 생성 시점을 포함하여 인스턴스 생성 혹은 제거 후, 지표의 임계값을 넘더라도 인스턴스를 생성하지 않고 기다리는 시간**
- 기본 300초 

### 수명주기 후크(Life Cycle Hook)
- **Scale In / Out** : ‘In’은 감소를, ‘Out’은 증가를 뜻함
- 지표의 임계값에 의해, Scale In / Out 이 되고 난 후 ‘In Service’ 상태에 돌입하기 전에 사용자가 정의한 작업을 수행하는 시간을 설정하는 기능
- 기본 3600초
- 즉 인스턴스 시작/종료시 사용자가 작업을 수행할 수 있음
- 이 시간동안 인스턴스에 설치 / 설정 등이 가능
- 가령, Cloudwatch를 사용하면 수명주기 작업이 발생시, Lambda 함수를 호출시키도록 설정 가능


## 12. IAM

### IAM이란?
- **AWS 리소스에 대한 액세스를 안전하게 제어할 수 있는 웹 서비스, IAM을 사용하여 리소스를 사용하도록 인증 및 권한 부여된 대상을 제어**
- 주요 기능
   - AWS 계정에 대한 공유 액세스
   - 서비스별 세분화된 권한 제공 가능
   - EC2에서 실행되는 앱을 위한 AWS 리소스 액세스 권한 제공
   - 멀티 팩터 인증(MFA)
   - 자격 증명 연동
- AWS 서비스들은 IAM Role을 할당받아 권한을 부여받을 수 있음
- Access Key와 Secret Access Key를 직접 입력하지 않고 권한 부여 가능
- 또한 IAM 사용자 계정을 만들어 사용자에게 적절한 권한을 부여하고 사용 가능한 서비스를 제한할 수 있음

### 정책(Policy)
- **User, Group, Role이 사용할 수 있는 권한의 범위를 지정한 것**
- S3FullAccess, Administrator Access 등 다양한 액세스 권한이 이미 정의되어 있으며 이를 ‘AWS 관리형 정책’이라 함
- 사용자 정의 정책 생성
   - JSON 형식 또는 직접 선택을 통해 사용자 정의 정책 선택 가능


### 역할(Role)
- **특정 권한을 가진 계정에 생성할 수 있는 IAM 자격증명**
- 역할에는 다음과 같은 주체가 있음
   - AWS 계정의 IAM 사용자
   - AWS의 서비스(EC2, RDS, ELB 등)
   - 외부 자격 증명 공급자 서비스에 의해 인증된 외부 사용자
- 역할 생성시 IAM 사용자, 서비스, 외부 사용자 등 주체를 정해야 함
- **하나의 역할에는 다수의 정책을 연결할 수 있음**
- 생성된 역할을 서비스 혹은 IAM 사용자 등에 연결
- Region에 국한되지 않고 사용 가능
- 신규 유저는 생성시 아무런 권한이 없으며 Access Key와 Secret Access Key가 할당됨
- 각 키는 최초 생성시에만 볼 수 있으며 즉시 보관해야 함

### 그룹 & 사용자
- **사용자는 IAM 사용자를 의미하여 관리자 계정에 의해 부여받은 권한에 한해 제한된 서비스에 접근할 수 있는 계정을 의미함**
- 콘솔 로그인과 프로그래밍 액세스 가능 여부를 선택하여 생성 가능
- 콘솔 로그인이 승인된 경우, 별도의 링크를 통해 콘솔에 로그인 할 수 있음
- 각 사용자마다 정책을 부여할 수 있음
- 사용자 모두에게 일일이 부여하기 힘들거나 그룹단위로 통제하고 싶은 경우, ‘Group’을 사용할 수 있음
- 그룹은 이미 생성된 사용자와 권한을 설정할 수 있으며 그룹 내 모든 사용자는 그룹의 권한을 적용받음 


## 13. SNS & SQS

### Simple Notification Service(SNS)란?
- **‘구독’중인 엔드포인트 혹은 사용자에게 메시지를 보내는 서비스**
- 주제(Topic)와 구독(Subscription)으로 나뉨
- 하나의 주제에 다수의 구독자로 이루어질 수 있음
- 주제는 다음과 같은 요소를 설정함
   - 메시지를 게시할 수 있는 대상과 받을 수 있는 대상
   - 암호화 여부
   - 메시지 전송 정책 

- 구독은 메시지를 받을 대상을 설정함
   - HTTP(웹)
   - SQS
   - 이메일
   - Lambda(메시지를 전송하여 Lambda 호출 가능)
   - 모바일 애플리케이션
- ‘푸시’ 기반 서비스

### Simple Queue Service(SQS)란?
- AWS 최초의 서비스
- 2004년 런칭 시작
- 서버와 서버 혹은 서버와 다른 엔드포인트 통신 중 장애시 모든 요청을 잃어버릴 수 있는 사태를 대비할 수 있음 
- **서버1이 서버2로 요청을 보내는 경우, 서버2에 직접 보내지 않고 큐(SQS)에 담아두면, 서버2가 우체통 열 듯 큐(SQS)를 열어 메시지를 확인하고 수행함**
- 수행한 메시지는 서버2가 확인 후 삭제
- 각 메시지는 최대 256KB의 텍스트로 구성될 수 있음
- 최대 14일까지 저장 가능하나, 기본값은 4일
- **처리되지 않은 서비스는 다시 큐에 보존되며 다른 요청자가 열람할 수 있음**
- 즉, 서비스 요청을 저장하고 대기열을 만들어 처리할 수 있도록 하는 서비스
- Standard 대기열과 FIFO 대기열로 나뉨 
   - **Standard 대기열** : 표준 서비스로 초당 무제한에 가까운 요청을 처리할 수 있으며 최소 한 번의 요청을 처리하나 순서가 보장되지 않음
   - **FIFO 대기열** : 선입선출을 지키는 대기열, 초당 300개까지 처리 가능

### SQS vs SNS
- SNS는 Push 기반 서비스이나 SQS는 Pull 기반 서비스임
- 즉 SNS는 와서 전달하고, SQS는 가서 찾아와야 함


## 14. Storage Gateway


### Storage Gateway란?
- **On-premise 환경에서 Cloud 상의 Storage를 지원할 수 있게 하는 하이브리드 스토리지**
- 이름이 Storage ‘Gateway’인 이유는 Storage Gateway 자체가 스토리지의 역할을 하는 것이 아닌 **스토리지(S3)의 Gateway 역할을 하기 때문**
- Volume Gateway의 Stored Volume을 제외하고 나머지 유형은 AWS에 EC2를 이용한 Gateway를 생성하여 이를 Mount Point로 활용 가능
- 3가지 종류의 Storage Gateway 존재
   - File Gateway
   - Volume Gateway
   - Tape Gateway
- 모든 Storage Gateway는 말그대로 ‘Gateway’를 생성해야 함
   - 그 대상에는 EC2 혹은 하드웨어 어플라이언스가 해당될 수 있음
   - EC2의 공인 IP를 Mount point로 지정하여 연결
- 일반 PC에 마운트하여 사용하는 등, 다양한 용도로 사용 가능

### File Gateway
- **NFS와 SMB를 지원하는 Storage Gateway 유형**
- S3를 스토리지로 사용하며, Gateway(EC2 등)을 통해 S3에 데이터를 저장하고 이를 직접 S3에서 액세스할 수 있음
- 즉 하나의 파일은 하나의 오브젝트로 관리됨
- S3에 오브젝트로 관리되는 만큼, S3의 다른 기능을 사용할 수 있음

### Volume Gateway
- **iSCSI를 지원하는 Storage Gateway 유형**
- 두 가지 유형으로 나뉨
   - **Cached Volume** : S3를 기본 데이터 스토리지로 사용하되, 자주 액세스하는 데이터를 온프레미스 스토리지 게이트웨이의 캐시 및 업로드 버퍼 스토리지에 보관
   - **Stored Volume** : On-premise 스토리지를 기본 데이터 스토리지로 사용하고, 해당 데이터를 EBS Snapshot 형식으로 S3에 비동기 백업을 실시함
- Volume Gateway 역시 S3에 저장이 되지만 File Gateway와 달리, S3 내에서 데이터를 볼 수 없음
   - EBS Snapshot 형식으로 저장하기 때문


### Tape Gateway
- VTL(가상 테이프 라이브러리)를 지원하는 Storage Gateway
- 가상 테이프 데이터는 S3나 S3 Glacier에 저장될 수 있음


## 15. KMS

### Key Management System란?
- **Key Management System는 말그대로 ‘key’를 관리하는 시스템으로 데이터를 암호화하고 복호화하는 기능을 담당함**
- EBS, S3, RDS, EFS, SNS 등의 서비스에서 데이터 암/복호화 기능을 제공
- 즉 다른 서비스와 통합되어 사용됨
- KMS를 사용하여 암호화를 관리할 경우 키가 외부로 유출되는 위험이 적으며, AWS가 직접 키의 안전을 책임짐
- 다음 3가지 유형의 키로 나뉨
   - AWS 관리형 키
   - 고객 관리형 키
   - 사용자 키 스토어


### 고객 마스터 키(Customer Master Key, CMK)
- **데이터를 암호화하는데 사용하는 데이터 키의 생성에 관여하는 키**
- AWS 서비스가 암호화를 시작할 때 CMK의 생성을 요청한 후 데이터 암호화를 시작
- 즉, CMK를 생성한 후에 데이터 키를 생성하고 데이터 암호화를 시작함
- 위의 3가지 유형 키는 CMK를 누가 관리하느냐에 따라 달라짐

### 데이터 암호화 과정
- 먼저 암호화를 시작하고자 하는 서비스가 CMK의 생성을 요청함
- CMK가 생성되고 이를 통해 데이터를 직접적으로 암호화할 데이터 키(대칭키) 생성
- 데이터를 데이터 키(대칭키)로 암호화한 후, 데이터 키를 폐기
- 그리고 데이터 키(대칭키)를 마스터 키로 암호화하여 암호화한 데이터와 같이 동봉함
   - 이를 봉투 암호화라 함
- 서비스가 암호화된 데이터를 다시 복호화하여 사용하고자 할 경우 고객 마스터 키를 가지고 데이터 키를 복호화 한 뒤 이 데이터 키를 가지고 데이터를 복호화함
   - 즉, CMK에 대한 접근권한이 없다면 데이터를 복호화할 수 없음
   - CMK를 삭제하게 되면 데이터를 복호화할 길은 전혀 없음 

![Image-1-1](https://user-images.githubusercontent.com/46843064/74654996-e1b3ef00-51ce-11ea-8207-46ddd7715a3e.png)  
<출처 : AWS 코리아, AWS KMS를 활용하여 안전한 AWS 환경을 구축하기 위한 전략>

### AWS 관리형 키(CMK)
- **AWS가 직접 CMK를 생성, 관리하는 서비스**
- 사용자가 CMK에 대한 제어권한이 없음
- AWS가 주기적으로 CMK를 변경하여 사용함
- 고객 관리형 키를 쓰고 있지 않는데, 암호화를 사용중이라면 AWS 관리형 키를 이용해 암호화하고 있는 것

### 고객 관리형 키(CMK)
- **고객이 직접 CMK를 생성하고 관리하는 서비스**
- 키의 활성화, 비활성화, 삭제 등 제어 권한을 가짐
- IAM을 이용하여 CMK에 접근할 주체를 정할 수 있음

### 사용자 지정 키 스토어
- **CMK를 KMS가 아닌 CloudHSM에 저장하여 사용하는 방식**
- CloudHSM 클러스터가 생성되어 있어야 사용 가능


## 16. EFS

### Elastic File System이란?
- **네트워크 파일 시스템(NFS v4)를 사용하는 파일 스토리지 서비스**
- VPC 내에서 생성되며, 파일 시스템 인터페이스를 통해 EC2에 액세스
- 수천 개의 EC2에서 동시에 액세스 가능하며, 탄력적으로 파일을 추가하고 삭제함에 따라 자동으로 Auto Scaling 가능, 즉 미리 크기를 프로비저닝할 필요가 없음
- 페타바이트단위 데이터까지로 확장 가능
- 최대 1천개의 파일 시스템 생성 가능

### 스토리지 클래스 
- **Standard Class** : 자주 액세스하는 파일을 저장하는데 사용하는 클래스
- **Infrequent Access(IA) Class** : 저장기간이 길지만 자주 액세스하지 않는 파일을 저장하기 위한 클래스

### 가용성
- 여러 가용영역에서 액세스 가능
- 여러 가용영역에 중복 저장되기 때문에 하나의 가용영역이 파괴되더라도 다른 AZ에서 서비스 제공 가능
- IPSEC VPN 또는 Direct Connect를 통해 On-premise에서 접속 가능 

### 성능 모드 / 처리량 모드
- 성능 모드에 있어서 대부분의 파일시스템에 Bursting Mode를 권장하지만 처리량이 많을 경우, Provisioned Mode를 권장
- 처리량 모드에 있어서 액세스하는 EC2가 매우 많을 경우, MAX I/O mode를 사용하는 것이 바람직하며(지연시간이 약간 길어짐) 그 밖의 경우 General Mode를 사용하는 것을 권장

### 수명 주기 관리
- 위에서 언급한 스토리지 클래스와 관련된 서비스로 사용자가 지정한 일정기간동안 액세스하지 않은 파일을 Infrequent Access Class로 옮길 수 있도록 하는 기능

### 파일시스템 정책
- EFS를 사용하는 모든 NFS 클라이언트들에게 적용되는 IAM 리소스 정책 
- 전송 중 암호화, 루트 액세스 비활성화, 읽기 전용 액세스 등 설정 가능


## 17. Sheid & WAF

### Shield란?
- **분산 서비스 거부 공격(DDoS)으로부터 웹 애플리케이션을 보호하는 서비스**
   - 분산 서비스 거부 공격 : 다수의 좀비 PC를 이용하여 무의미한 서비스 요청을 서버에 퍼부어 기능을 마비시키는 공격
- 다음 2가지 유형으로 나뉨
   - Shield Standard
   - Shield Advanced
- Shield Standard는 기본적으로 적용되는 서비스로 설정을 하지 않아도 AWS 서비스에 활성화되어 있음
- Shield Advanced는 추가 비용을 내고 추가적인 서비스를 제공받는 것으로 L7 트래픽 모니터링, 사후 분석 등의 기능을 제공함
- Cloudfront와 통합되어 있기 때문에 AWS의 서비스가 아니더라도 Cloudfront의 origin이라면 보호 가능

### Web Application Firewall(WAF)이란?
- 간단히 말해 **웹방화벽**으로서 Cloudfront와 ALB를 통해 서비스를 제공함
(즉 ALB와 Cloudfront를 직접 지정하여 웹방화벽 서비스를 제공)
   - 웹방화벽 : 방화벽이 L3/L4 Layer의 방어(IP와 port 차단)을 이용한다면 웹방화벽은 L7( HTTP 헤더, HTTP 본문, URI 문자열, SQL 명령어, 스크립팅)을 이용한 공격을 방어
- 다양한 종류의 웹 공격에 대한 정보를 지닌 Rule을 선택하여 활성화하거나, 특정 IP의 웹 요청을 막을 수 있음


## 18. CloudFormation

### Cloudformation이란?
- 인프라 관리 간소화를 목적으로 하는 서비스
- **AWS의 리소스를 일일이 설정하지 않고 해당 서비스의 프로비져닝과 설정을 미리 구성하여 반복작업을 줄이도록 도와줌**
- EC2, Auto Scaling Group부터 ELB, RDS, S3 등을 사전에 구성하여 한 번의 클릭으로 다수의 서비스를 빠르게 생성할 수 있음
- 생성된 리소스 모음은 다른 계정 혹은 다른 리전에 옮겨 사용 가능함 

### Stack
- **하나의 단위로 관리할 수 있는 AWS 리소스들의 모음**
- 스택을 생성, 업데이트 또는 삭제하여 리소스 모음을 생성, 업데이트, 삭제를 할 수 있음
- 스택에서 실행중인 리소스를 변경해야 하는 경우 스택을 업데이트할 수 있는데 이 업데이트된 세트를 ‘변경세트’라 함
- 스택을 삭제하는 경우 삭제할 스택을 지정하면 해당 스택과 스택 내 모든 리소스를 삭제함
- AWS에서 리소스를 삭제할 수 없는 경우 스택이 삭제되지 않음
- 스택의 리소스 중 하나라도 성공적으로 생성되지 않은 경우 성공적으로 생성한 모든 리소스를 모두 삭제함(이를 Automatic rollback on error라 함)

### Template
- **스택을 구성하는 AWS 리소스를 JSON 혹은 YAML 형식으로 선언한 텍스트 파일 **
- 템플릿은 로컬 혹은 S3에 저장되며, 템플릿을 불러올 때 S3 bucket을 지정할 수 있음
- 템플릿을 ‘Designer’를 통해 생성할 수도 있으며 S3 bucket에 저장된 것을 불러와 생성할 수 있음
- 템플릿의 여러 가지 요소
   - **Parameters** : 선택 섹션, 스택 생성 및 업데이트시 템플릿에 전달하는 값, 사용자가 선택하는 여러 요소들(EC2 유형 – t2.micro 등)
   - **Conditions** : 선택 섹션, 조건문, 리소스가 생성되는 조건을 만들어 조건 충족시에만 리소스를 만들 수 있도록 하는 요소
   - **Resources** : 필수 섹션, Cloudformation에 포함될 리소스
   - **Metadata** : 선택 섹션, 템플릿에 대한 세부 정보를 제공하는 임의의 JSON, YAML 객체
   - **Mappings** : 선택 섹션, 프로그래밍 언어로 따지면 ‘Switch’ 조건문에 해당하며 ‘키’에 해당하는 값 세트를 생성하고 해당하는 키가 있으면 값 세트에 맞춰 리소스를 생성함
   
![이미지-3](https://user-images.githubusercontent.com/46843064/74654896-ac0f0600-51ce-11ea-9ecb-1989da2d95d1.jpg)     
<JSON으로 생성한 Cloudformation Template> 출처 : AWS docs

### Cloudformation의 과금 특징
- Cloudformation 서비스 자체는 **무료**이지만, Cloudformation을 통해 생성되는 모든 리소스는 유료임
- 즉, 생성된 리소스는 독립적인 요소로서 과금이 부여됨


## 19. Snowball

### Snowball이란?
- **페타바이트급 대용량 데이터를 전송하기 위한 서비스**
- 서비스와 더불어 물리적인 실체가 있는 장비가 존재하여 AWS에 요청하면 Snow ball를 배송받고 On-premise의 데이터를 빠르게 Snowball로 이동시킨 뒤, 작업이 완료되면 이 물리 장비를 다시 AWS로 배송하고 S3 Bucket에 저장함
- 스토리지 용량은 최대 80TB까지 저장 가능 
- Snowball 이외에 기능이 추가된 Snowball Edge가 있음

### 사용하는 경우
- **페타바이트 규모의 데이터를 AWS로 이송하는 경우 적합**
- VPN, Direct Connect, S3를 통한 직접적인 전송을 이용하기엔 데이터의 양이 많을 경우 Snowball을 사용하는 것이 좋음
- 또한 물리적으로 격리된 환경이거나 인터넷 환경이 좋지 않을 경우 사용
- 평균적으로 AWS로 데이터를 업로드하는데 1주일 이상이 소요되는 경우 Snowball 사용을 검토함

![Image-3](https://user-images.githubusercontent.com/46843064/74654315-66057280-51cd-11ea-83af-d4b7d332ba51.png)

## 20. CloudTrail

### Cloudtrail이란?
- **AWS 계정 내에서 이루어지는 모든 작업과 활동에 대해 기록하는 서비스**
- 계정 내에서 이루어지는 **API 호출도 모두 기록함**
- AWS 계정 생성시 자동으로 활성화되며 최대 90일간의 기록을 볼 수 있음
- 최근 이벤트를 확인할 수 있는 이벤트 기록, ‘추적’ 기능, Insight 이벤트로 이루어짐

### 추적
- ‘추적’을 활성화하면 모든 리전 혹은 선택한 리전에 대하여 활동기록을 기록하고 S3 Bucket에 보관할 수 있음
- Organization와 연동하여 사용가능하며 자신의 ‘조직’을 선택하면 조직 내 모든 계정을 추적할 수 있음 
- Insight 이벤트는 write API 호출에 관련된 비정상적인 호출을 탐지함

## 21. ElastiCache

### Cache란 무엇이며 왜 써야 하는가?
- Cache는 CPU 칩 안에 들어가 있는 작은 메모리(물리적 실체).
- 프로세서가 필요한 데이터가 있을 때마다 메인 메모리에 일일이 접근하여 속도가 지연되는 것을 막기 위해 자주 사용하는 데이터를 담아두는 곳
- 즉 처리 속도 향상을 위해 존재하는 작은 칩이자 메모리
- L1,L2,L3로 나뉘며 숫자가 적을 수록 도달하는 속도가 빠름
- Cache는 CPU와 메모리 사이 뿐만 아니라, 메모리와 디스크 사이에서도 발생함
- 후술할 In Memory Cache는 메모리와 디스크 사이의 Caching을 의미 

![diagram-MemoryCache-400x400-1](https://user-images.githubusercontent.com/46843064/74654525-d01e1780-51cd-11ea-8bc7-d10d1170448d.png)  
<하드웨어별 접근속도>

### In Memory Cache(In Memory DataBase)
- **데이터 처리 속도를 향상시키기 위한 메모리 기반의 DBMS**
- 메모리 위에 모든 데이터를 올려두고 사용하는 데이터베이스의 일종(ElastiCache가 AWS 카테고리에서 DB 부분에 있는 이유)
- 디스크에 최적화된 Database(RDS 등)에서 저장된 쿼리 결과나 자주 사용하는 데이터를 메모리에 적재하여 사용하는 것은 비효율적
- **즉, 모든 데이터를 메모리 위에 올려두어 굳이 디스크 기반의 데이터베이스에까지 이동하여 데이터를 가져와 속도가 저하되는 것을 막음**
- 또한 데이터베이스의 데이터뿐만 아니라, 디스크, 세션, 기타 동적으로 생성된 데이터를 저장할 수 있음
- 다만 메모리 기반의 데이터베이스이기 때문에, 휘발성 메모리라는 단점이 존재하며 전원 공급 차단시 모든 데이터가 유실되고 할당된 메모리에 한해 저장 가능 

### ElastiCache
- **AWS의 In Memory Cache Service**
- **Memcached와 Redis로 나뉨**
- Memached, Redis 모두 비관계데이터베이스형(NosQL) 서비스이며, Key-value 기반임
- Memached, Redis 모두 이미 존재하는 서비스이며 AWS에서 사용가능하도록 구현한 것
- ElastiCache는 Node로 구성되어 서비스를 제공하며, Node는 EC2처럼 다양한 Type을 가지고 유형에 따라 다양한 메모리 크기를 가짐
- 다양한 Type을 갖는 이유는 적은 양의 메모리가 필요할 경우, 작은 Type의 Node를 사용하여 비용을 적게 들게 하기 위함
- 유형이 결정된 Node들은 ‘고정된’ 메모리 크기를 가지며, 각자의 DNS로 이루어진 엔드포인트를 보유함 

![Image-1-1024x502](https://user-images.githubusercontent.com/46843064/74654604-0491d380-51ce-11ea-97e4-26c4a79e86cd.png)  
<AWS 설명서 : ElastiCache>

### Memcached의 특징
- **Cluster로 구성되어 있으며, Cluster 내에는 Node들이 존재하여 인 메모리 캐시로서의 역할을 담당함**
- 각 Node는 Type별로 메모리를 보유하며 서비스를 제공하며, 필요시 Node를 늘려 서비스 용량을 향상시킬 수 있음
- 각 Node별로 AZ를 따로 둘 수 있지만, 장애 조치(Failover)가 불가능하고 복제본을 둘 수 없음 

### Redis의 특징
- **기본적으로 Cluster로 구성되지는 않지만, Cluster로 구성이 가능하며 Shard와 Node를 가지고 있음**
- Shard는 여러 Node로 구성되며, 하나의 Node가 읽기/쓰기를 담당하고 나머지 Node는 복제본 역할을 함
- Cluster로 구성되지 않은 Redis는 하나의 Shard만을 가지지만, Cluster로 구성될 경우 다수의 Shard를 갖게 됨
- 복제본을 가지므로, 장애조치(복제본을 기본 Node로 승격)가 가능하며 Multi-AZ 기능을 지원함 




