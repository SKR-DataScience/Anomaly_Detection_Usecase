# 산업분야 별 이상탐지 사례 정리
이상탐지 유형 별 적용 가능 알고리즘과 산업군 별 이상탐지 사례를 살펴본다.

## 스터디원
- 김유리, 김재원, 최동민


## 1. 개요와 방법론

<table>
    <thead>
        <tr>
            <th>구분</th>
            <th>목차</th>
            <th>강의/실습</th>
			<th>작성자</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan=5>1. 개요</td>
            <td>1.1. 정의와 종류 </td>
            <td><a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/개요/note01.md">1. 이상탐지의 정의와 발생원인</a> <br/>
			<a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/개요/note02.md">2. 이상탐지의 종류</a></td>
			<td>김유리</td>
        </tr>
		<tr>
            <td rowspan=2>2.2. Use Case</td>
			<td> <a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/개요/note03_DM.md">3. 이상탐지 대표 Use Case</a><br/>
			<a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/개요/note04_DM.md">4. 실제 현업 적용 사례 - 제조</a><br/>
			<a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/개요/note05_DM.md">5. 실제 현업 적용 사례 - 금융</a><br/>
			</td>
			<td>최동민</td>
        <tr>
			<td>
			<a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/개요/note06_JW.md">6. 실제 현업 적용 사례 - 게임</a><br/>
			</td>
			<td>김재원</td>
		</tr>
		<tr>
	    <td rowspan=2>1.3. 실무 적용 </td>
		<td> 
			<a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/개요/note07_JW.md">7. 이상탐지 문제해결 프로세스</a><br/>
			<a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/개요/note08_JW.md">8. 이상탐지 프로젝트 시작 시 점검해야 하는 내용 및 EDA</a><br/>
			<a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/개요/note09_JW.md">9. 이상탐지 프로젝트 셋업 및 일정 분배</a><br/>
			<a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/개요/note10_JW.md">10. 이상탐지 모델 비교를 위한 실험설계, 모델 선택법</a></td>
			<td> 김재원 </td>
		</tr>
		<tr>
	    		<td> <a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/%EA%B0%9C%EC%9A%94/note11_DM.md">11. 이상탐지 결과물 현업 전달하기</a><br/>
			</td>
			<td>최동민</td>
		</tr>
		<tr>
			<td rowspan=11> 2. 방법론 </td>
			<td rowspan=3> 2.1. 거리기반 </td>
			<td>
			<a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/%EB%B0%A9%EB%B2%95%EB%A1%A0/1_%EA%B1%B0%EB%A6%AC%EA%B8%B0%EB%B0%98/note1_3sigma_and_boxplot.md">1. 3-sigma, box plot</a></td>
			<td> 최동민 </td>
		</tr>	
		<tr>
			<td>
				<a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/방법론/1_거리기반/note2_mh_dist.md">2. 마할라노비스 거리 </a></br>
				<a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/방법론/1_거리기반/note3_KNN.md">3. KNN </a></br> 
			</td>
			<td> 김유리 </td>
		<tr>
		</tr>
			<td rowspan =5> 2.2. 분포 & 패턴 기반 </td>
			<td>
				<a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/방법론/2_분포_패턴기반/note1_LOF.md">1. LOF </a>
			</td>
			<td> 최동민 </td>
		<tr>
			<td> <a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/방법론/3_분류모델_기반/note1_Isolation_Forest.md">2. Isolation Forest </a> </td>
			<td> 김재원 </td>
		</tr>
		<tr>
			<td> <a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/방법론/2_분포_패턴기반/note2_one_class_svm.md">3. One class SVM </a> </td>
			<td> 김유리 </td> 
		</tr>
		<tr>
		<td> <a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/방법론/3_분류모델_기반/note2_AutoEncoder.md">4. AutoEncoder </a> </td>
		<td> 김재원 </td> 
		</tr>
		<tr>
		<td> <a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/방법론/2_분포_패턴기반/note5_RRCF.md">5. Robust Random Cut Forest</a> </td>
		<td> 최동민 </td> 
		</tr>
		<tr>
			<td> 2.3. 시각화 기반 </td>
			<td> <a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/방법론/4_시각화기반/note1_PCA_tSNE.md">1. 축소차원(PCA) 시각화(t-SNE) </a> </td>
			<td>김유리</td>
		</tr>
		<tr>
			<td rowspan =4> 2.4. 시퀀스 기반  </td>
			<td> <a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/방법론/5_시퀀스기반/note1_SH_ESD.md">1. S-H ESD </a> 
			</td>
			<td>김유리</td>
		<tr>
			<td> <a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/방법론/5_시퀀스기반/note2_STL.md">2. 시계열 분해 이상탐지
			(1) - STL  </a></br> 
			<a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/방법론/5_시퀀스기반/note3_Holt-winters.md">3. 시계열 분해 이상탐지
			(2) - Holt-Winters
			</td>
			<td>최동민</td>
		</tr>
    </tbody>
</table>

## 2. 분야 별 이상탐지
1. 커머스 및 유저 행동
	<table>
		<thead>
			<tr>
				<th>강의/실습</th>
				<th>작성자</th>
				<th>비고</th>
			</tr>
		</thead>
		<tbody>
			<tr>
			<td><a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/분야별_이상탐지/1_커머스_유저행동/note01_이상매출.ipynb"> 1. 이상매출 </a>
			</td>
			<td> 김유리 </td>
			<td> # 지점별매출 # stream data <br/>
			<a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/분야별_이상탐지/1_커머스_유저행동/rrcf.py"> rrcf.py </a>: 구 버전 numpy 오류 수정 </td>
			</tr>
			<tr>
			<td><a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/분야별_이상탐지/1_커머스_유저행동/note02_이상유저탐지.ipynb"> 2. 이상유저탐지 </a>
			</td>
			<td> 최동민 </td>
			<td> # 비정상 거래 # Pycaret # Precision/Recall </td>
			</tr>
			<tr>
			<td><a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/분야별_이상탐지/1_커머스_유저행동/note03_이상쿠폰.md"> 3. 이상쿠폰 </a>
			</td>
			<td> 김재원 </td>
			<td> # 마케팅 캠페인 # 쿠폰 발행 # Rule-base  </td>
			</tr>
			<tr>
			<td><a href="https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/분야별_이상탐지/1_커머스_유저행동/note04_통신사유저이탈.md"> 4. 통신사유저이탈 </a>
			</td>
			<td> 김유리 </td>
			<td> # Demographic # 가입정보 # pycarot </td>
			</tr>
			</tr>
			<tr>
			<td> 5. 온라인 구매 유저 이탈 </td>
			<td>  </td>
			<td>  </td>
			</tr>
			</tr>
			<tr>
			<td> 6. 온라인 광고 이상클릭 </td>
			<td>  </td>
			<td>  </td>
			</tr>
			<tr>
			<td> 7. 텍스트 기반 사기(이메일, 통화) </td>
			<td>  </td>
			<td>  </td>
			</tr>
		</tbody>
	</table>

3. 게임
	<table>
		<thead>
			<tr>
				<th>강의/실습</th>
				<th>작성자</th>
				<th>비고</th>
			</tr>
		</thead>
		<tbody>
			<tr>
			<td> <a href = "https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/분야별_이상탐지/2_게임/note01_FPS버그유저탐색.md">1. 서바이벌 FPS 게임 버그 유저 </a></td>
			<td> 최동민 </td>
			<td> # EDA 규칙기반 # FPS게임 </td>
			</tr>
			<tr>
			<td> 2. 밸런스 이상 캐릭터 </td>
			<td>  </td>
			<td>  </td>
			</tr>
			<tr>
			<td> 3. 이상 로그인 시도 </td>
			<td>  </td>
			<td>  </td>
			</tr>
		</tbody>
	</table>
4. 금융
	<table>
		<thead>
			<tr>
				<th>강의/실습</th>
				<th>작성자</th>
				<th>비고</th>
			</tr>
		</thead>
		<tbody>
			<tr>
			<td> <a href = "https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/분야별_이상탐지/4_금융/note01_신용불량고객탐지.md">1. 신용불량 고객 탐지 </a> </td>
			<td> 김유리 </td>
			<td> # 신용평가 # 대출 마케팅 대상 </td>
			</tr>
			<tr>
			<td> 2. 신용카드 사기 </td>
			<td>  </td>
			<td>  </td>
			</tr>
			<tr>
			<td> 3. 온라인 결제 사기 </td>
			<td>  </td>
			<td>  </td>
			</tr>
			<td> 4. 시계열 보험 청구 사기</td>
			<td>  </td>
			<td>  </td>
			<tr>
			<td> 5. unlabel 보험 청구 사기</td>
			<td>  </td>
			<td>  </td>
			</tr>
			<td> 6. 요금 청구서 사기</td>
			<td>  </td>
			<td>  </td>
			</tr>
		</tbody>
	</table>

5. 기계, 제조
	- Water Circulation
	- Bearing failure
	- 반도체 공정
	- 장비 충돌 이상
6. 의료
	- 심장 질병 이상 환자
	- 만성 신장 질환 이상 환자
	- unlabel 헬스케어 데이터 이상
7. 공공 데이터
	<table>
		<thead>
			<tr>
				<th>강의/실습</th>
				<th>작성자</th>
				<th>비고</th>
			</tr>
		</thead>
		<tbody>
			<tr>
			<td> <a href = "https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/분야별_이상탐지/6_공공데이터/note01_전력사용.ipynb">1. 전력소비량 이상 </a></td>
			<td> 최동민 </td>
			<td> # 시간대별 # 이상구간 상한, 하한 # 혼합분포 이상탐지 </td>
			</tr>
			<tr>
			<td> 2. 공유자전거 이상 사용 </td>
			<td>  </td>
			<td>  </td>
			</tr>
			<tr>
			<td> 3. 전력사용 </td>
			<td>  </td>
			<td>  </td>
			</tr>
		</tbody>
	</table>

8. 기타
	<table>
		<thead>
			<tr>
				<th>강의/실습</th>
				<th>작성자</th>
				<th>비고</th>
			</tr>
		</thead>
		<tbody>
			<tr>
			<td> <a href = "https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/분야별_이상탐지/7_기타/note01_서버셧다운.ipynb">1. 서버셧다운 </a></td>
			<td> 김유리 </td>
			<td> # 단변량 시계열 # 온도 이상 </td>
			</tr>
			<tr>
			<td> 2. 사이트 트래픽 이상 </td>
			<td> </td>
			<td>  </td>
			<td>  </td>
			<tr>
			<td> <a href = "https://github.com/SKR-DataScience/Anomaly_Detection_Usecase/blob/main/분야별_이상탐지/7_기타/note01_기후데이터.ipynb">3. 기후 데이터 이상 </a> </td>
			<td> 최동민 </td>
			<td> # 시계열분해 # STL # S-H-ESD </td>
			</tr>
			</tr>
			<tr>
			<td> 4. 태양흑점활동 이상</td>
			<td>  </td>
			<td>  </td>
			<td>  </td>
		</tbody>
	</table>




