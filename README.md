### **중동 시장 진출을 위한 올리브영 인프라 구축**

> 실시간 데이터 수집 & 안정적인 인프라를 중심으로 현지시장에 맞춘 대규모 사용자 대상 인프라 설계 및 구축 프로젝트

아키텍처는 다음과 같습니다.
[아키텍처 링크](https://viewer.diagrams.net/index.html?tags=%7B%7D&target=blank&highlight=0000ff&edit=_blank&layers=1&nav=1&page-id=9BRYHBcp9KjD5vnArndL&title=%EC%9A%B0%EB%B0%94%EB%A6%AC%EC%98%AC.drawio#Uhttps%3A%2F%2Fdrive.google.com%2Fuc%3Fid%3D1QmI5ELLuDr3Tjr4iyQ2ej1wjfC1AO8hy%26export%3Ddownload#%7B%22pageId%22%3A%229BRYHBcp9KjD5vnArndL%22%7D)

올리브영의 중동 시장 진출 확대를 위한 인프라 구축 - 실시간 데이터 수집 파이프라인 & 운영에 최적화된 안정적인 인프라 구축을 중심으로

2024년 1월 - 2월

CJ Cloudwave 교육에서 실시한 프로젝트입니다.

✔️ Project
1) 중동시장 진출 확대를 위한 웹사이트를 새롭게 구축하고, 
2) 최근에 입사한 신규 개발자들이 해당 프로젝트에 투입되어 
3) 실시간 중동 고객 구매 패턴을 파악하는 파이프라인을 설계한다는 방향으로 프로젝트를 진행


✔️ Detail
인프라 도입과제
- 안정적인 인프라 구성
- 특정기간(올영세일)에 몰리는 대규모 트래픽에 대응
- 서비스의 안정적 운영을 위한 CICD, 모니터링 그리고 비용 최적화 
- 실시간으로 고객 구매 행동 로그를 수집하고 모니터링할 수 있는 파이프라인 구현

[프론트엔드- 백]
- 사용자가 올리브영 상품 구매를 위해 접근하는 웹페이지는, 
- 접근 편의성과 지연시간 최소화를 위해 route53과 cloudfront 를 통해 접속하도록 구성 
- 이때 보안적으로 안전하도록 WAF 와, Shield,ACM 사용
- 사용자가 보낸 구매 요청이 API GW로 들어오면, VPC Link, NLB 를 거쳐 API 요청이 안전하게 처리될 수 있도록 구성
- 모든 요청은 EKS 기반으로 처리됨 (Fargate 는 구축해본 결과, 여러 응용에 있어 제약 사항(demonset 불가 등)이 많아 EKS 로만 구성 ) 

[ 안정적인 EKS 구축 방안 ]
- 자가복구 : livenessProbe, readinessProbe
- 영향범위최소화 : pod AntiAffinity 
- 빠른확장 : HPA , Karpenter (오토스케일링)
- Locust 부하테스트를 통한 인프라 안정성 검증을 통한 response 실패율 0.278% 
- gatner 기준 어플리케이션 평균 실패율 0.5%보다 낮은 수치

[ CICD ]
- Gitops 기반 블루그린 배포방식
- ArgoCD appofapps 구성 

[ 비용 절감 방안 ]
- kube-downscaler
- KEDA
- Karpenter
- Kubecost 응용하여 비용절감 기준 확정 
- 비용 파이프라인 ( Finops )설계 : kubecost, Athena, QuickSight, CUR 등 이용

[모니터링]
- Datadog 사용 : 이미 올리브영이 데이터독 베스트 프랙티스 사례로 소개될 만큼 잘 사용하고 있고다른 모니터링 툴보다 이점(비즈니스 모니터링, 대시보드 커스텀, 엔터프라이즈 사용 가능 등)이 있기 때문

[ 데이터 수집 파이프라인 ]
- 요청 > API GW > Lambda > Kinesis datastream > kinesisfirehose > elastic Cache > S3 
- kafka 대비 운영복잡도 낮음

