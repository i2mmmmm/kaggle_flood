# Kaggle 홍수 예측 대회  

https://www.kaggle.com/competitions/playground-series-s4e5

---
🖥️ **환경설정**  
![Python 3.8.19](https://img.shields.io/badge/python-3.8.19-blue.svg)  
`vscode`  
`google colab`

---
🔼 **Baseline**  
 Linear Regression -> 0.84458

---

📊 **EDA**  
<details>
 <summary> 1) 데이터 변수 설명 </summary>

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
</details>

2) 데이터 특징

각 변수 1 ~ 17 까지의 정수로 구성.

히트맵을 통해 보여지는 상관관계 없음.

타겟 0.005단위의 0 ~ 1 사이 확률로 구성.

  -> 분류모델로 풀어보려 했으나 실패.
  
---

🎛️ **Feature Engineering**  

<details>
<summary> feature 추가 </summary> 

    df['CombinedUrbanImpact'] = df['Urbanization'] * df['PopulationScore']
    df['EnvironmentalDegradation'] = df['Deforestation'] + df['Siltation'] + df['WetlandLoss']
    df['InfrastructureVulnerability'] = df['DeterioratingInfrastructure'] + df['DrainageSystems'] + df['DamsQuality']
    df['NaturalDisasterRisk'] = df['MonsoonIntensity'] + df['ClimateChange'] + df['Landslides'] + df['CoastalVulnerability']
    df['ManagementEffectiveness'] = df['RiverManagement'] + df['AgriculturalPractices'] + df['Encroachments'] + df['InadequatePlanning'] + df['PoliticalFactors']
    df['Infrastructure_Risk'] = df['DamsQuality'] * df['DrainageSystems']
    df['wet_Risk'] = df['WetlandLoss'] * df['Encroachments']

    df['total'] = df[BASE_FEATURES].sum(axis=1)
    df['mean'] = df[BASE_FEATURES].mean(axis=1)
    df['std'] = df[BASE_FEATURES].std(axis=1)
    df['max'] = df[BASE_FEATURES].max(axis=1)
    df['min'] = df[BASE_FEATURES].min(axis=1)
    df['median'] = df[BASE_FEATURES].median(axis=1)
    df['ptp'] = df[BASE_FEATURES].values.ptp(axis=1)
    df['q25'] = df[BASE_FEATURES].quantile(0.25, axis=1)
    df['q75'] = df[BASE_FEATURES].quantile(0.75, axis=1)
    df['ClimateImpact'] = df['MonsoonIntensity'] + df['ClimateChange']
    df['AnthropogenicPressure'] = df['Deforestation'] + df['Urbanization'] + df['AgriculturalPractices'] + df['Encroachments']
    df['InfrastructureQuality'] = df['DamsQuality'] + df['DrainageSystems'] + df['DeterioratingInfrastructure']
    df['CoastalVulnerabilityTotal'] = df['CoastalVulnerability'] + df['Landslides']
    df['PreventiveMeasuresEfficiency'] = df['RiverManagement'] + df['IneffectiveDisasterPreparedness'] + df['InadequatePlanning']
    df['EcosystemImpact'] = df['WetlandLoss'] + df['Watersheds']
    df['SocioPoliticalContext'] = df['PopulationScore'] * df['PoliticalFactors']
   </details>

  ---
   
🎛️ **Model**  

0. 회귀분석 (r2 : 0.844)  
1. 랜덤포레스트 (r2 : 0.655)  
2. gradient boosting (r2 : 0.614)  
3. catboost
4. xgboost
5. lgbm
6. ann_MLP
- ensemble(stacking)
---

🎛️ **하이퍼 파라미터 조정**

`lgbm_params = {'num_leaves': 183, 'learning_rate': 0.01183688880802108,  
    'n_estimators': 577, 'subsample_for_bin': 165697, 'min_child_samples': 114,  
    'reg_alpha': 2.075080888948164e-06, 'reg_lambda': 3.838938366471552e-07,  
    'colsample_bytree': 0.9634044234652241, 'subsample': 0.9592138618622019,  
    'max_depth': 9}`

`cat_params = {'n_estimators':8000, 'random_state':0, 'learning_rate': 0.011277016304363601,  
    'depth': 8, 'subsample': 0.8675506657380021, 'colsample_bylevel': 0.7183884158632279,  
    'min_data_in_leaf': 98, 'bootstrap_type': 'Bernoulli'}`

---

🏁 **제출 결과**  
Private Score : 0.86898 ( 154/2794 )

---

📓 **회고**

타겟이 0.005 단위로 분류되고 있었고, 각 변수들이 일정한 정수의 형태를 갖고 있었다.  
때문에 분류 문제로 풀어보고 싶었으나 성능이 잘 나오지 않았고 제출 시간이 임박해  
결국 회귀 모델을 적용했던 점이 아쉬움으로 남는다.  
 -> 다음에는 EDA 과정을 더 길게 잡고 시작하기

변수 조합 및 생성, 하이퍼 파라미터 최적화, 모델 선정, 앙상블 등  
여러 가지를 적용했던 점이 좋았고 즐거웠다.  
하지만 짧은 시간과 제출 횟수 제한으로 아쉬움이 남기도...  
 -> 다음에는 더 적합한 모델을 빨리 찾고 중점적으로 파기
