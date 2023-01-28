# 산업분야 별 이상탐지 사례 정리
이상탐지 유형 별 적용 가능 알고리즘과 산업군 별 이상탐지 사례를 살펴본다.

## 스터디원
김유리, 김재원, 최동민

## 목차
### 

<table>
    <thead>
        <tr>
            <th>구분</th>
            <th>목차</th>
            <th>강의/실습</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan=2>1. 개요</td>
            <td>개요1. 이상탐지의 정의와 발생원인</td>
            <td><a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/개요/note1.md">강의 - 개요2. 이상탐지의 종류</a></td>
        </tr>
        <tr>
            <td>개요2. 이상탐지의 종류</td>
			<td> <a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/개요/note2.md">강의 - 개요2. 이상탐지의 종류</a></td>
        </tr>
		<tr>
			<td rowspan=4> 2. 방법론 </td>
			<td> 2.1. 거리기반 </td>
			<td> a. 3 sigma, box plot </br> 
				b. 마할라노비스 거리 </br>
				c. KNN </br> 
				d. LOF</td>
		</tr>
		<tr>
			<td> 2.2. 분포 & 패턴 기반 </td>
			<td> a. Isolation Forest </br> 
				 b. One class SVM </br> 
				 c. AutoEncoder</td>
		</tr>
		<tr>
			<td> 2.3. 시각화 기반 </td>
			<td> a. 축소차원(PCA) 시각화(t-SNE)Isolation Forest 
		</tr>
		<tr>
			<td> 2.4. Sequence 기반  </td>
			<td> a. 시계열 분해 이상탐지  </br> 
					&nbsp;&nbsp;&nbsp;&nbsp; - STL </br> 
					&nbsp;&nbsp;&nbsp;&nbsp; - Holt-Winters
		</tr>
    </tbody>
</table>

### 분야
1. 커머스 및 유저 행동
	- 이상매출
	- 이상 유저
	- 이상 쿠폰 사용
	- 온라인 구매 유저 이탈
	- 통신사 유저 이탈
	- 온라인 광고 이상클릭
	- 텍스트 기반 사기(이메일, 통화)
3. 게임
	- 서바이벌 FPS 게임 버그 유저
	- 밸런스 이상 캐릭터
	- 이상 로그인 시도
4. 금융
	- 신용카드 사기거래
	- 온라인 결제 사기거래
	- 시계열 보험 청구 사기
	- unlabel 보험 청구 사기
	- 요금 청구서 사기
	- 신용불량고객 이상
5. 기계, 제조
	- Water Circulation
	- Bearing failure
	- 반도체 공정
	- 장비 충돌 이상
6. 의료
	- 심장 질병 이상 환자
	- 만성 신장 질환 이상 환자
	- unlabel 헬스케어 데이터 이상
7. 공공
	 - 공유자전거 이상 사용
	 -  전력사용
	 - 전력 소비량 이상 탐지
8. 기타
	- 서버 셧다운
	- 사이트 트래픽 이상
	- 기후 이상
	- 태양 흑점 활동 이상



