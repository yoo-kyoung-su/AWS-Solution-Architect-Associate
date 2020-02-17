# AWS-Solution-Architect-Associate
AWS SAA 시험 관련 개념 정리

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




