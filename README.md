# :moneybag: INSERT COIN :moneybag:

## 1️⃣ Prologue
데이터 분석을 아장아장 배우던 때 발생한 코시국. 사람마다 다르겠지만 코시국하면 가장 많이 들어본 단어는 떡상 혹은 떡락 아닐까? 주식과 코인으로 동학개미들의 막대한 자본이 들어가면서 근 2년간 불시장을 형성했다. 나 포함 내 주변에서 주식의 '주' 자도 꺼내지 않던 친구들이 만나면 종목추천과 MTS를 열어보기 바빴다. 
이러던 도중 몇가지 의문이 생겼다.  
  
✔️ 다들 어디서 정보를 얻어서 주식이 오를거라고 판단하는걸까?  
✔️ 카더라가 주식을 움직이는걸까?  
✔️ 모든 정보가 사실과 다를수 있는데 정확하다는 판단은 어떻게 하는걸까?  
✔️ 주식을 움직이는 힘은 무엇일까?  
  
 이 모든 의문을 해결할 수 있는 방법은 단 한가지였다!  
   
 **'수많은 데이터 중 어떤 정보가 주식에 가장 큰 영향을 주는지 분석해보자'**
 
## 2️⃣ 그래서 insert coin은?
### 📍 insert coin 의 의미
insert coin 이라는 타이틀은 어릴적 문방구 앞 게임기의 문구에서 착안하였다.  
<img width="283" alt="스크린샷 2021-10-17 오후 8 13 04" src="https://user-images.githubusercontent.com/8296974/137625078-e920fc9a-3daa-4e8f-a3d5-bf675817b854.png">  
사람이 없는 게임기 화면에는 항상 'insert coin' 문구가 반짝거리고 있었다. 
그럼 우리는 무엇에 홀린 마냥 어머니께 받은 용돈을 꺼내 고전게임의 세계에 빠져들곤 했다.
돈을 넣어야 시작할 수 있다는 것부터 시작하면 끝내기 힘들다는 점이 어쩌면 게임기나 주식이나 비슷하지 않을까 하는 생각에 프로젝트 명을 insert coin으로 지었다. 

### 📍 insert coin을 통해 얻고자 하는것 
 insert coin 통해 얻고자 하는건 크게 두가지다.  
    ✔️ 데이터를 수집하여 어떤 정보가 주식에 가장 큰 영향을 주는지 분석해보자.  
    ✔️ 알고 있던 기술이 아닌 새로운 기술을 활용하여 개발하자 

## 3️⃣ 작업기간
    기획 : 2021-08-23 ~ 2021-09-10
    개발 : 2021-09-11 ~ ongoing 
    
## 4️⃣ 기술스텍
    server side : AWS, docker/docker-compose 
    language : python
    
## 5️⃣ 서비스구성도 
### 📍 1-depth  : 프로세스 흐름도
client side 없이 Server 기준으로 처리하기 때문에 프로세스 흐름도는 단순하다.  
주변 사람들의 대회 속에서 가장 많이 등장하는 매체를 기준으로 데이터를 수집하고 분석한 다음 실제 주가 정보와 비교하여 해당 매체의 정보가 유의미 한지 분석한다.  
![insertcoin_프로세스흐름@1 600000023841858x (2)](https://user-images.githubusercontent.com/8296974/137586747-69040ce9-f767-4d1c-bad5-28d547a560fe.png)
### 📍 2-depth : 서비스 구성도 
insert coin을 구성하는 서비스는 크게 5가지로 볼수 있다. 
![export 3](https://user-images.githubusercontent.com/8296974/137592016-b56d31c6-db24-4a00-ae87-f5bfcebd8b39.png)

**✔️ 1. API Service**  
>: 기준정보가 되는 종목 코드 및 실시간 주가 정보 확인을 위하여 증권사 데이터를 수집하는 서비스이다.  
>: 현재까지 구현된 것은 아래와 같다.  
>>. 종목코드/섹터 수집   
>>. 종목별 일자별 시가/종가 수집   

**✔️ 2. Crawling Service** 
>: 주가에 영향을 미칠것이라고 예상 되는 매체 세가지를 우선적으로 뽑아서 데이터 수집하는 서비스이다.  
>: 현재까지 구현된 것은 아래와 같다. 
 
|서비스 종류 |데이터 수집 사이트|
|------|---|
|일자별 각 분야 별 랭킹뉴스 top 50|https://news.nate.com/rank/interest|
|종목토론방 글 |https://finance.naver.com/item/board.nhn|
|기업 재무제표정보|데이터 수집 사이트 : https://finance.naver.com/item/main.nhn|

**✔️ 3. Database** 
>: API와 crawling Service에서 수집한 정보를 저장하는 DBMS이다.  
>: 하나의 DBMS에 수집정보를 기준으로 테이블을 분리시켰다.  
>: 기준정보가 되는 테이블 앞에 m_를, 인터페이스 테이블 앞에 if_f를 붙여 관리하고 있다.   

![0F8A49BB-89AA-4E9F-9D2E-07CFB9B9D9D8_1_105_c](https://user-images.githubusercontent.com/8296974/137590429-4f7a2c76-74af-4a27-a0b0-5dbe917c26d6.jpeg)

**✔️ 4. Machine Learning Service**  
>: API Service 를 통해 적재된 데이터를 기준으로 해당 일자 별 시가와 종가의 등락폭이 높은 순위대로 10종목을 선정한다.     
>: 증권사 Crawler를 통해 수집된 데이터와 등락폭이 높은 순위와 비교하여 어떤 매체가 가장 영향력을 발휘하는지 일자별로 확인하여 저장한다. (예상 사용 알고리즘 : 주성분분석)    
>: 1년치 데이터를 기준으로 확인하였을때 가장 영향력있는 매채를 선별한다.   
	
**✔️ 5. CI/CD**  
>: 위 서비스가 유기적으로 개발/운영될 수 있도록 CI/CD 서비스를 구성하였다.  
>: AWS Codepipeline을 이용하여 형상관리에서부터 배포까지 관리한다.  

### 📍 3-dpeth : 인프라 구성도  
**✔️ 1. CodePipeline을 이용한 dev/ops 구현**  
>: AWS의 CodePipeline 서비스를 사용하여 개발한 소스 반영과 동시에 build/deploy 된다.   
>: CodePipeline에 연결한 서비스는 CodeCommit(형상관리), CodeBuild(소스빌드), CodeDeploy(빌드파일반영) 로 구성되어있다.     
>>. CodeBuild 시 Crawling Service의 경우 docker-compose명령어를, API서비스의 경우 pyinstaller를 사용하여 각각의 의 소스를 빌드한다.  
>>. CodeDeploy 시 Crawling Service의 경우 ecs에 , API서비스의 경우 ec2에 빌드된 결과물을 배포 한다.  
  
**✔️ 2. docker-compose를 이용한 MSA 구성**  
>: 하나의 crawling service 에 문제가 있다고 하더라도 서로 영향이 가지 않도록 하기 위해 docker-compose를 이용하여 데이터 수집 사이트 별로 서비스를 구동하였다.  
>: insert coin의 MSA를 구성하는 서비스 종류는 아래와 같다.


|Micro Service 종류|설명|
|------|---|
|NEWS|일자별 뉴스 수집 서비스|
|finantialreport|재무제표 수집 서비스|
|stockcode|신규 종목 수집 서비스|
|stockreport|종목별 시가/종가 수집 서비스|
|stockdebate|종목토론방 수집 서비스|

**✔️ 3. Service 종류에 맞는 AWS 서버 구성**  
>: 증권사의 경우 해당 API 수행을 위해서는 로그인이 필요한데 로그인시 공인인증서를 요구하기 때문에, 서버 구성 시 S3에 저장, EC2(Windows)에 배포 되도록 하였다.  
>: 그외 수집되는 데이터의 경우 docker기반으로 구성하였기 때문에, 인프라 구성 시 ECR에 저장 ECS에 배포 되도록 하였다.  

**✔️ 4. postgreSQL을 이용한 DBMS 구성**  
>: RDBMS 중 확장성이 뛰어나고 오픈소스이며 속도가 빨라 데이터 분석 응용 프로그램에 적합다는 점에서 postgreSQL로 DBMS를 구성하였다.  

![Web App Reference Architecture V2 (2)](https://user-images.githubusercontent.com/8296974/137591370-9e373aeb-f273-4b5b-aae6-8b4cdede6e0d.png)

## 6️⃣ TO-DO LIST 
**✔️ Crawling Service**  
- [ ] twitter(혹은 SNS) 중 주가에 영향력이 있을 만한 대상 추가 선정 
- [ ] 종목토론방 수집 시 발생하는 에러수정 (네이버에서 지속적으로 데이터 수집시 timeout 발생시킴) 
- [ ] 뉴스 수집 시 기사까지 수집가능한지 확인

**✔️ Machine Learning Serbice**  
- [ ] 머신러닝 서비스 구현 

**✔️ Infra**  
- [ ] DB서버 이중화 

## 7️⃣ Reference
. bulk data에 대한 python-posgresql 성능 분석한 블로그  
[https://medium.com/analytics-vidhya/part-4-pandas-dataframe-to-postgresql-using-python-8ffdb0323c09](https://medium.com/analytics-vidhya/part-4-pandas-dataframe-to-postgresql-using-python-8ffdb0323c09)  
. 파이썬 docker 이미지 생성  
[Docker 로 내가 원하는 이미지 만들기](https://zesow.github.io/docker-로-내가-원하는-이미지-만들기/)  
. buildspec.yml ecr 접속 불가 해결  
[프라이빗 레지스트리 인증](https://docs.aws.amazon.com/ko_kr/AmazonECR/latest/userguide/registry_auth.html). 
