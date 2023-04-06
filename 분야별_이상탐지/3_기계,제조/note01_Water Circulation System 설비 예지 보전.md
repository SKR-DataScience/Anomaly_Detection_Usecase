# Water Circulation System 설비 예지 보전

</br>

> ## 문제 정의
- 사업 확장으로 인한 관리 Point 증가에 따른 운영 및 유지보수 리소스 부족
- Water Circulation System 고장으로 인한 리스크 발생 

</br>

> ## 기대효과
- 이상진단 시스템을 활용한 사전 유지보수를 통해 고장 발생 리스크 감소
- 이상진단 솔루션 제공을 통한 추가 고객 유치

</br>

> ## 성과 측정
- 이상진단 시스템 활용 전/후 고장 발생률 비교
- 이상진단 시스템 운영 전/후 신규 고객 증가 및 기존 고객 만족도 조사   
  (단, 전/후 차이가 '이상진단 시스템 운영 여부'만 있어야 유효함. 다른 원인은 배제)

</br>

> ## 보유 데이터 & 특징

* 시계열 센서 데이터
  - 시간
  - 진동 가속도
  - 전기모터 암페어(Ampere), 전기모터 전압(Volt)
  - 엔진 온도, 순환 루프 유체 온도 (섭씨)
  - 워터펌프 후 루프 압력(Bar)
  - 루프 내부의 유체 순환 유량(Liter/min)
  - 이상 여부


</br>

> ## 이상 탐지 접근 방법
- Isolation Forest 사용
  - 주어진 문제가 contextual detection 보다는 point detection에 가깝다고 판단 --> 해당 유형에 적합한 모델 사용
    - 근데 이 문제가 point detection이 맞나? contextual detection에 가까운 것 같음  
    - 관련자료: https://velog.io/@kangtae/%EC%95%84%EC%9D%B4%ED%9A%A8-Anomaly-detection  

- 성능 평가는 recall 위주로 함. 이상 탐지에서는 '실제 이상 중 몇 개나 맞췄는지'가 중요하기 때문.

- Isolation Forsest에서 score 기반 threshold 조정하기
  - decision_function() 사용해 outlier score 산출 --> 원래는 음수일 경우 outlier로 분류
  - 주어진 score를 가지고 anomaly 여부를 판단할 수 있는 threshold를 직접 정할 수 있음
  ~~~
  # ▶ Score 변수 할당
  y_pred_train_score =  clf.decision_function(x_train)
  y_pred_test_score = clf.decision_function(x_test)

  # ▶ Threshold 조정 (모델이 뱉은 score를 가지고 직접 anomaly 여부를 정함. 아래 기준으로는 0.05 기준)
  y_pred_train = np.where(y_pred_train_score < 0.05, 1, 0)
  y_pred_test = np.where(y_pred_test_score < 0.05, 1, 0)
  ~~~
 - 위 방법으로 threshold만 0에서 0.05로 조정해도, test set 기준 recall 0.33 --> 0.82로 향상
 
</br>

> ## 해당 강의 회차 종합 평가
