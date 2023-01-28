# 개요2. 이상탐지의 종류


- 데이터 종류, 해결 문제에 따라 다른 이상탐지 알고리즘 적용

## 데이터 종류
1. 시계열 데이터(sequence)
    - sequence vs static data
        - 데이터 간 순차적 연관성 있으면 sequence, 없으면 static
    - ex. 시간에 따른 휴대폰 온도
2. 단변량 vs 다변량
    - 반드시 다른 방법론을 적용할 필요는 없지만, 변량의 종류에 따라 알맞은 방법론 적용
3. 데이터 타입
    - 이진, 범주형, 연속형, 혼합형 등 데이터 타입에 따라 다른 방식의 이상탐지
4. 데이터 간 관계(relational vs independent)
    - 주로 피쳐 엔지니어링에서 데이터 간 관계 파악
    - 예컨대, 회귀분석에서 상관계수 높으면 다중공선성 문제로 처리 필요
5. Usecase 존재여부
    - 이미 알려진 문제의 이상탐지의 경우 usecase 벤치마킹
    - 알려지지 않은 문제의 경우 여러 방법론 적용, 어려운 문제


## 이상탐지의 종류
<table>
    <thead>
        <tr>
            <th>구분</th>
            <th>종류</th>
            <th>특징</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan=2>정적</td>
            <td>1. Point Anomaly Detection</td>
            <td>- 일정 기간 내 수집된 데이터 내 분포로부터 이상 탐지  <br/>
            - 특정 포인트의 이상 감지 <br/>
            - 일반적으로 outlier</td>
        </tr>
        <tr>
            <td>2. Distriubted Anomaly Detection</td>
			<td> - 정상 분포로부터 벗어난 이상 데이터 감지</td>
        </tr>
        <tr>
            <td rowspan=2>동적</td>
            <td>3. Contextaul Anomaly Detection</td>
            <td>- 시계열(시퀀스)에서의 이상감지 <br/>
            - 연속적 맥락 내 동떨어진 패턴 감지  <br/>
            - global 이상
        </tr>
        <tr>
            <td>4. Collective Anomaly Detetcion</td>
			<td> - 다른 집합과의 비교를 통해 비유사성 감지 <br/> 
            - local 이상</td>
        </tr>
        <tr>
            <td> 기타 </td>
            <td> 5. Online Anomaly Detetction </td>
            <td> - 실시간 데이터에서의 이상감지 </br> - 빠른 처리가 관건이므로 과하게 복잡한 모델은 지양 </td>
        </tr>
    </tbody>
</table>

## Label vs Unlabel 이상탐지 비교