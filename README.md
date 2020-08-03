# hotelManagement
Intensive Level2 AWS Cloud - final team project

실습수업교재
MSA School - https://msaschool.io/#/%EC%84%A4%EA%B3%84--%EA%B5%AC%ED%98%84--%EC%9A%B4%EC%98%81%EB%8B%A8%EA%B3%84/04_%EA%B5%AC%ED%98%84/index

WorkFlowy - https://workflowy.com/s/msa/27a0ioMCzlpV04Ib

1) [AWS] Cluster 생성 
 - eksctl create cluster --name admin24-scluster --version 1.15 --nodegroup-name standard-workers --node-type t3.medium --nodes 2 --nodes-min 1 --nodes-max 3
 
2) MSAez 모델 다운 소스 다운

3) [GIT] git 레포지토리 생성
 - git init
 - github에 git remote add git remote add origin https://github.com/[YourName]/demo.git
 - git add .
 - git commit -m "first commit"
 - git push -u origin master
 
4) AWS ecr에서 레파지토리 만들고 github에 올린거 ubuntu에서 clone으로 가져오기
 - rm target //뭐가 남아 있다. 그래서 삭제. 패키지하기 위해.
 - mvn package // 패키징
 - docker build -t 052937454741.dkr.ecr.eu-central-1.amazonaws.com/order2:v1 . //각각의 aggregate에 대해서 실행
 - docker push 052937454741.dkr.ecr.eu-central-1.amazonaws.com/order2:v1

5) [ubuntu] 카프카 설치
 - curl -o kafka2.5.tgz -l http://mirror.navercorp.com/apache/kafka/2.5.0/kafka_2.13-2.5.0.tgz
 - 주키퍼 실행 ./zookeeper-server-start.sh ../config/zookeeper.properties
 - 카프카 실행 ./kafka-server-start.sh ../config/server.properties
 
6) AWS 배포
 - kubectl create deploy order --image=052937454741.dkr.ecr.eu-central-1.amazonaws.com/order2:v1
 - kubectl expose deploy order --type=ClusterIP --port=8080
 - kubectl edit svc gateway //LoadBalancer 수정. 외부로 IP노출.
 
7) AWS AutoScale 을 위해 Matrics, 설치 필요.

8) CI,CD에서는 siege 옵션을 -c1 -t300S 걸어주고 배포했을때 availability 100% 확인.
 
