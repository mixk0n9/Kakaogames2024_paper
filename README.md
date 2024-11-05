# :video_game: 언어 모델 기반 게임 봇 탐지 분석:video_game:

<img src="./kakaogames_image2024/카겜.jpeg" width="500" height="150"/>


**2024 카카오게임즈 산학 프로젝트**

----------------------

## :scroll:논문

김민경, 김유미, 민지현, 성현수, 정윤서. (2024+) Sequence-to-text 기법을 활용한 언어 모델의 분류 성능 비교, 한국데이터정보과학회지. (Under revision)

----------


## :book: 개요

### :dart:주제

MMORPG 게임 봇의 특징을 분석하고, sequence-to-text 기법을 활용한 언어모델 기반 게임 봇 탐지 성능 비교

### :calendar:기간

2024.04.08 ~ 2024.09

### :busts_in_silhouette:참여 인원

4명

### :memo:역할

게임 로그 데이터 가공 및 분석, Longformer, BERT, XGBoost, RandomForest 모델링

### :chart_with_upwards_trend: 배경
거래소 거래나 현금 구매는 일부 유저에게서 발생하는 것과 달리, 대부분의 접속 유저가 액션을 취하므로 모델의 적용 범위 확장 필요


### :triangular_flag_on_post:목표
- Transformer 기반 언어모델로 유저의 행동 패턴을 인식하여 봇을 탐지
- - 여러 언어모델의 성능 비교
- 유저의 행동 패턴 기반의 인사이트 도출


### :open_file_folder:사용 데이터
‘A’게임의 2024년 3월 행동 로그 데이터

### :bulb: 문제 정의
- 유저의 행동(action)은 순서가 중요하므로, 시간 정보를 다룰 수 있는 분석 도구 필요
- 정상유저보다 봇이 더 적은 불균형 문제 존재

### :crown:성과
- 유저의 행동 로그를 시퀀스로 구성해 봇을 탐지하는 연구는 현재 국내에 거의 없어 새로운 탐지 기법 제공에 의의
- 하루치의 정보만으로 봇을 탐지하는 능력이 뛰어남을 보임

---------

## :chart_with_upwards_trend:분석 과정

1. 데이터 및 Label 정의
- 2023년 3월 7일: 총 11,516명의 데이터
- 운영팀에서 정의한 영구정지, 보호조치 유저를 정답(true label)으로 가정
- binary label

2. 데이터 가공
- 하루 동안 유저의 행동 로그를 시간 순서대로 시퀀스로 변환, 이를 텍스트 문장으로 생성
- 캐릭터 변경 로그도 '캐릭터를 변경했다'는 문장을 만들어 텍스트에 추가
- 정상유저를 봇 유저 수만큼 언더샘플링
- Train, valid, test data 비율은 5:2:3

|Scenario|설명|
|:---:|:---:|
|Base|행동 종류 수&연속 행동에 대한 요약 정보(중위수, 최대값, 최솟값)|
|Text|행동 시퀀스 텍스트|
|Longtext|행동 종류 수&연속 행동에 대한 요약 정보(최대값)+행동시퀀스 텍스트|
|Longlongtext|행동 종류 수&연속 행동에 대한 요약 정보(중위수, 최대값, 최솟값)+행동시퀀스 텍스트|

- 머신러닝 모델 RandomForest, XGBoost은 Base를 tabular data로 만들어 적용
  - 변수: 행동 종류 수, 연속 행동 중위수, 연속 행동 최대값, 연속 행동 최솟값

3. 모델
- BERT, Longformer 등의 트랜스포머 기반의 언어모델
- RandomForest, XGBoost

4. 결과
- 4개의 Scenario에서 5회 시뮬레이션(논문 revision 전에는 5회로 진행)


**<Scenario 1: Base>**
|Models|Accuracy|Precision|Recall|F1-score|Computing time|
|:---:|:---:|:---:|:---:|:---:|:---:|
|RandomForest|0.6531(0.0049)|0.6115(0.0047)|0.8411(0.0061)|0.7080(0.0028)|0.6720(0.0301)|
|XGBoost|0.6525(0.0034)|0.6158(0.0041)|0.8122(0.0103)|0.7003(0.0028)|0.1400(0.0152)|
|BERT|0.6509(0.0041)|0.6183(0.0064)|0.7931(0.0206)|0.6940(0.0047)|173.1640(0.7272)|
|Longformer|0.6487(0.0090)|0.6018(0.0079)|0.8802(0.0035)|0.7148(0.0044)|2320.8400(2.3800)|

**<Scenario 2: Text>**
|Models|Accuracy|Precision|Recall|F1-score|Computing time|
|:---:|:---:|:---:|:---:|:---:|:---:|
|BERT|0.6509(0.0041)|0.6674(0.0039)|0.8265(0.0062)|0.7384(0.0021)|1590.7840(2.1226)|
|Longformer|0.6509(0.0041)|0.6422(0.0075)|0.8575(0.0101)|0.7341(0.0034)|3474.4860(19.7368)|

**<Scenario 3: Longtext>**
|Models|Accuracy|Precision|Recall|F1-score|Computing time|
|:---:|:---:|:---:|:---:|:---:|:---:|
|BERT|0.7058(0.0027)|0.6716(0.0068)|0.8079(0.0145)|0.7329(0.0021)|1587.6160(4.6619)|
|Longformer|0.6777(0.0077)|0.6294(0.0096)|0.8707(0.0186)|0.7298(0.0044)|3466.2780(7.7467)|

**<Scenario 4: Longlongtext>**
|Models|Accuracy|Precision|Recall|F1-score|Computing time|
|:---:|:---:|:---:|:---:|:---:|:---:|
|BERT|0.7021(0.0046)|0.6603(0.0077)|0.8359(0.0162)|0.7371(0.0034)|1588.5540(2.8508)|
|Longformer|0.6791(0.0086)|0.6289(0.0096)|0.8807(0.0161)|0.7330(0.0033)|3463.2480(27.0782)|


<img src="./kakaogames_image2024/취업 포트폴리오 최종_8.png" width="800" height="400"/>
<img src="./kakaogames_image2024/취업 포트폴리오 최종_9.png" width="800" height="400"/>
