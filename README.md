# AWS-Solution-Architect-Associate
AWS SAA 시험 관련 개념 정리

### 1. AWS 기본 용어
•Region  
 ◦AWS에서 사용하는 일종의 IDC의 집합으로서 거의 모든 클라우드 서비스가 탑재되는 곳으로 다수의 Availability Zone으로 구성됨  
 ◦한 곳의 AZ가 기능마비되더라도 다른 AZ가 기능을 수행함  
 ◦전세계 주요 대도시에 분포되어있음(서울 포함)  
 ◦AWS 사용자는 각 Region마다 별도의 클라우드망을 구성할 수 있음

•Availability Zone  
 ◦가용 영역이라 불리우며 IDC, 즉 데이터센터의 역할을 함  
 ◦보통 3~4개의 AZ가 각 Region마다 존재  
 ◦하나가 파괴더라도 다른 AZ가 기능을 수행함  
 ◦네트워크 구성에서 하나의 서브넷은 하나의 AZ를 의미  

•On-premise  
 ◦클라우드 방식이 아닌 자체적으로 보유한 전산실과 같은 인프라를 의미  

•Edge Location  
 ◦CDN 서비스인 Cloudfront가 사용하는 캐시서버  
 ◦전세계에 분포되어있으며 Edge Location끼리 데이터를 공유함  

•탄력성(Elasticity)과 확장성(Scalability)  
 ◦탄력성은 AWS의 상징과도 같은 서비스인 Autoscaling처럼 단시간에 갑작스러운 요구에 따라 사용량을 늘리거나 줄이는 것을 의미(Scale out)  
 ◦확장성은 장기적인 관점, 즉 아키텍쳐적인 관점에서 예상치를 충분히 감당할 수 있는 리소스를 보유하는 것을 의미(Scale up)  

