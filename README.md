# :video_game: Kakaogames2024 :video_game:

<img src="./images/카겜.jpeg" width="500" height="200"/>

카카오게임즈 산학 프로젝트

----------------------

## :scroll:논문

김민경, 김유미, 민지현, 성현수, 정윤서. (2024+) Sequence-to-text 기법을 활용한 언어 모델의 분류 성능 비교, 한국데이터정보과학회지. (Under revision)

----------


## :book: 개요

### 주제

> MMORPG 게임 봇의 특징을 분석하고, 언어모델로 행동 시퀀스를 임베딩하여 봇을 탐지

### 기간
> 2024.04.08 ~ 2024.11

### 참여 인원
> 4명

### 역할
> 게임 로그 데이터 가공 및 분석, Longformer, BERT, XGBoost, RandomForest 모델링

### 배경
> - 거래소 거래나 현금 구매는 일부 유저에게서 발생하는 것과 달리, 대부분의 접속 유저가 액션을 취하므로 모델의 적용 범위 확장 필요
> - 단시간에 봇을 잡아낼 수 있는 프로세스 필요


### 목표
> - Transformer 기반 언어모델로 유저의 행동 패턴을 인식하여 봇을 탐지
> - 유저의 행동 패턴 기반의 인사이트 도출


### 사용 데이터
> ‘A’게임의 2024년 3월 행동 로그 데이터

---------

## :chart_with_upwards_trend:분석 과정

1. Label 정의
- 운영팀에서 정의한 영구정지, 보호조치 유저를 정답(true label)으로 가정
- binary label

2. 데이터 가공
- 하루 동안 유저의 행동 로그를 시간 순서대로 시퀀스로 변환, 이를 텍스트 문장으로 생성
- 캐릭터 변경 로그도 변경했다는 의미의 문장으로 생성
- 정상유저를 봇 유저 수만큼 언더샘플링
- Train, valid, test data 비율은 5:2:3

|변수|설명|
|:---:|:---:|
|Test|행동 시퀀스 텍스트|
|Longtext|행동 종류 수&연속 행동에 대한 요약 정보(최대값)+행동시퀀스 텍스트|
|Longlongtext|행동 종류 수&연속 행동에 대한 요약 정보(중위수, 최대값, 최소값)+행동시퀀스 텍스트|
  
3. 모델
- BERT, Longformer, ELECTRA 등의 트랜스포머 기반의 언어모델
- RandomForest, XGBoost

4. 결과

||Accuracy|Precision|Recall|F1-score|
|:---:|:---:|:---:|:---:|:---:|
|Test|0.9158|0.7164|0.6564|*0.6851*|


