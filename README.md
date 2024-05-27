# kaggle_flood
Kaggle 홍수 예측 대회  
---
https://www.kaggle.com/competitions/playground-series-s4e5

<details>
<summary>
2024.05.25
</summary>
  
- 캐글 참가 신청  
└ `google colab` 환경에서 캐글 연결해서 데이터 가져오기 **성공**

- train 데이터의 **EDA 실행**
1. 전체 null 값 없음
2. (1117957, 22) 크기의 데이터 프레임

- 📃 데이터 변수 설명  
MonsoonIntensity: 몬순 강도  
TopographyDrainage: 지형 배수  
RiverManagement: 강 관리  
Deforestation: 산림 벌채  
Urbanization: 도시화  
ClimateChange: 기후 변화  
DamsQuality: 댐의 품질  
Siltation: 침적  
AgriculturalPractices: 농업 관행  
Encroachments: 침해  
IneffectiveDisasterPreparedness: 비효과적인 재난 대비  
DrainageSystems: 배수 시스템  
CoastalVulnerability: 해안 취약성  
Landslides: 산사태  
Watersheds: 유역  
DeterioratingInfrastructure: 악화되는 인프라  
PopulationScore: 인구 점수  
WetlandLoss: 습지 손실  
InadequatePlanning: 부적절한 계획  
PoliticalFactors: 정치적 요인  

---

- **시각화**

1. 각 변수에 따른 산점도 (대체로 비슷한 형태)
2. 상관관계 히트맵 (모든 변수가 같은 색으로 표시)
3. FloodProbability 히스토그램

---

- **baseline**

train 데이터로 회귀분석  
R2 Score: 0.8449901321915165 // 
MSE: 0.00040393373468618546

---

- **feature engineering**

3개 변수 추가  
`train['Climate_Risk'] = train['MonsoonIntensity'] * train['ClimateChange']`  
`train['Infrastructure_Risk'] = train['DamsQuality'] * train['DrainageSystems']`  
`train['wet_Risk'] = train['WetlandLoss'] + train['Encroachments']`  

추가 이후 회귀 결과  
R2 Score: 0.8449834588748664 // 
MSE: 0.00040395112440308126
</details>

**제출결과 : 0.84458**


<details>
<summary>
2024.05.26
</summary>
  
- 다양한 모델링 도전  
0. baseline_회귀분석  
R2 Score: **0.844**8773362840329  
MSE: 0.000403206587090558  
1. 랜덤포레스트  
R2 Score: 0.6555031624976388  
MSE: 0.0008954422956993097  
2. grandient boosting  
R2 Score: 0.6142897853539955  
MSE: 0.001002567229880375
3. 


</details>

**제출결과 : 0.86089**
