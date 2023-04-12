# 밸런스 이상 캐릭터 탐지

</br>

> ## 문제 정의
- 캐릭터 밸런스 이상으로 게임 퀄리티 저하
- 신규 진입 유저 감소, 기존 유저 이탈 증가

</br>

> ## 보유 데이터 & 특징

- 캐릭터 이름
- 캐릭터 설명(description)
- 캐릭터 역할(attacker, speedster, defender,...)
- 플레이 난이도(expert, intermediate, novice)
- 각종 스탯(offense, endurance, support,...)

</br>

> ## 성과 측정
- 밸런스 조정 후 신규 유저 진입률 비교
- 밸런스 조정 후 기존 고객 이탈률 비교

</br>

> ## 이상 탐지 접근 방법
- 알고리즘 사용 X, 캐릭터 역할/난이도에 따른 스탯 분포를 비교해가며 합리적이지 않은 캐릭터를 찾음   
   
  ex)    
  캐릭터 A : 스탯 총합 15, 역할 all-rounder, 난이도 중간      
  캐릭터 B : 스탯 총합 9.5 / 역할 defender, 난이도 어려움    
  --> 모든 상세 스탯이 캐릭터 A가 더 높고, 난이도도 더 쉬움. 밸런스가 맞지 않는다고 의심할 수 있음.    
 
 </br>
 
- radar chart 시각화
~~~
# 컬럼 6개의 데이터프레임: 캐릭터명 & 5개의 능력치
df_rader = df[['Name', 'Offense',	'Endurance',	'Mobility',	'Scoring',	'Support']] 
df_rader = df_rader.head()
df_rader

# 레이더차트 시각화
from math import pi
from matplotlib.path import Path
from matplotlib.spines import Spine
from matplotlib.transforms import Affine2D
plt.style.use(['default'])

def rader_chart(df_rader) :

  ## 하나로 합치기
  labels = df_rader.columns[1:]
  num_labels = len(labels)
      
  angles = [x/float(num_labels)*(2*pi) for x in range(num_labels)] ## 각 등분점
  angles += angles[:1] ## 시작점으로 다시 돌아와야하므로 시작점 추가
      
  my_palette = plt.cm.get_cmap("Set2", len(df_rader.index))
  
  fig = plt.figure(figsize=(4,4))
  fig.set_facecolor('white')
  ax = fig.add_subplot(polar=True)
  for i, row in df_rader.iterrows():
      color = my_palette(i)
      data = df_rader.iloc[i].drop('Name').tolist()
      data += data[:1]
      
      ax.set_theta_offset(pi / 2) ## 시작점
      ax.set_theta_direction(-1) ## 그려지는 방향 시계방향
      
      plt.xticks(angles[:-1], labels, fontsize=10) ## 각도 축 눈금 라벨
      ax.tick_params(axis='x', which='major', pad=10) ## 각 축과 눈금 사이에 여백을 준다.
  
      ax.set_rlabel_position(0) ## 반지름 축 눈금 라벨 각도 설정(degree 단위)
      plt.yticks([0,2,4,5],['0','2','4','5'], fontsize=8) ## 반지름 축 눈금 설정
      plt.ylim(0,5)
      
      ax.plot(angles, data, color=color, linewidth=1, linestyle='solid', label=row.Name) ## 레이더 차트 출력
      ax.fill(angles, data, color=color, alpha=0.2) ## 도형 안쪽에 색을 채워준다.
      
  plt.legend(loc=(0.9,0.9))
  plt.show()
~~~
</br>

> ## 해당 강의 회차 종합 평가
- 밸런스 이상을 판단하기 위해서는 캐릭터 속성(역할, 난이도, 스탯) 이외에 다른 정보들이 필요할 것 같음.   

  - 보유 스킬 : 인게임에서 스탯 이상의 영향력을 행사할 수 있음
  - 캐릭터 선택(pick)률 : 실제로 많이 사용되는지
  - 캐릭터 평균 승률 : 실제로 스탯상 우위에 있는 캐릭터들이 결과적으로 승률도 좋은지 확인 필요. 승률까지 좋아야 밸런스에 문제가 있을 것.   
  
  -> 결국 '실제 게임 결과'(승/패, 점수, 랭크 등)와 연관지어 분석까지 진행해야 밸런스 이상 여부를 판단할 수 있을듯.
