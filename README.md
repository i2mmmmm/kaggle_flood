# kaggle_flood
Kaggle 홍수 예측 대회  
---
https://www.kaggle.com/competitions/playground-series-s4e5

---
🖥️ **환경설정**  
`vscode - python 3.8.19`  
`google colab`

---
🔼 **Baseline**  
 Linear Regression -> 0.84458

---
📊 **EDA**  
1) 데이터 변수 설명

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

2) 특징

각 변수 1 ~ 17 까지의 정수로 구성.  
히트맵을 통해 보여지는 상관관계 없음.  
타겟 0.005단위의 0 ~ 1 사이 확률로 구성.  
  -> 분류모델로 풀어보려 했으나 실패.
  
---

🎛️ **Feature Engineering**  

feature 추가 

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
   
🎛️ **Model**  

0. 회귀분석 (r2 : 0.844)  
1. 랜덤포레스트 (r2 : 0.655)  
2. gradient boosting (r2 : 0.614)  
3. catboost (r2 : 
4. xgboost
5. lgbm
4. ann_MLP

  


**모델 설계**  
`model = Sequential()`  
`model.add(Dense(64, activation='relu', input_shape=(X_train_scaled.shape[1],)))`  
`model.add(Dense(32, activation='relu'))`  
`model.add(Dense(1))`


**모델 설계**  
`model = Sequential()`  
`model.add(Dense(128, activation='relu', input_shape=(X_train.shape[1],)))`  
`model.add(Dropout(0.3))`  
`model.add(Dense(64, activation='relu'))`  
`model.add(Dropout(0.3))`  
`model.add(Dense(32, activation='relu'))`  
`model.add(Dense(1))`

`param_grid = {'batch_size': [16, 32, 64], 'epochs': [50, 100, 200], 'optimizer': [Adam(), RMSprop()]}`

