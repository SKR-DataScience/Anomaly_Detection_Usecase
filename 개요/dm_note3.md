# 개요3. 이상탐지 대표 Use Case
- 이상 탐지를 활용하는 산업군 및 대표 활용 사례 알아보기

<br/>

#### 1. Game Abuse Detection
  - 게임을 정상적으로 이용하지 않는 버그 유저 탐지
  - 버그 유저가 많으면 신규or헤비유저가 불만을 느끼고 이탈할 수 있으므로 risk가 됨

#### 2. Cyber-Intrusion Detection
  - 컴퓨터 시스템상 침입을 탐지, 단순 분석보다는 만들어진 소프트웨어를 많이 활용
  - RAM, file system, log file 등 시계열 데이터 활용
  - 서버 자원에 대한 모니터링 등으로 운영됨

#### 3. Fraud Detection
  - 보험, 신용, 금융 관련 불법 행위 탐지
  - 시계열보다는 표 형식(tabular) 데이터 활용

#### 4. Malware Detection
  - Malware(악성코드) 검출
  - tabular 데이터를 활용하거나, Malware 파일을 binary(1/0)화 한 뒤 gray scale image로 변환하여 CNN 등 딥러닝으로 분류하기도 함

#### 5. Medical Anomaly Detection
  - 의료 영상 및 이미지, 뇌파 기록 등에서 이상 탐지
  - 주로 신호 or 이미지 데이터
  - X-ray, MRI 등 다양한 장비로부터 얻은 이미지를 다루며 난이도 높음
  - 실제로는 ML/DL을 활용한 이상 탐지로 환자를 직접 진단하지는 않음, support 역할로 활용

#### 6. Social Networks Anomaly Detection
  - 주로 Social Network상의 Text 데이터를 활용
  - 스팸 메일, 비매너 이용자, 허위 정보 유포자를 탐지

#### 7. Log Anomaly Detection
  - 시스템 log를 보고 실패 원인을 추적
  - 주로 Text 데이터(시스템 에러메시지)를 분석
  - pattern matching 및 rule-based 방법을 사용해 간단히 해결할 수도 있고,
  - 새로운 에러메시지가 계속 추가될 경우 복잡하므로 딥러닝 기반 방법론을 활용하는 것이 효과적

#### 8. IoT Big-Data Anomaly Detection
  - IoT 장치, 센서로부터 얻은 시계열 데이터를 활용
  - 여러 장치들이 복합적으로 구성되어 데이터 원천도 다양하며, 분석 난이도가 높음
  - ex) 세탁기 센서 데이터를 분석하여 사용 패턴이 이상한지 탐지하고, 고객에게 고장 위험 알림을 보냄   
    -> (SKR) EV 주행/충전시 BMS 데이터 전류,전압 변화를 분석 >> 운전자 바람직하지 않은 차량이용패턴 alert
    
#### 9. Industrial Anomaly Detection
  - 제조업 데이터에 대한 이상치 탐지
  - 주로 이미지 데이터를 활용한 외관 검사 or 시계열 데이터를 활용한 장비 고장 예측
