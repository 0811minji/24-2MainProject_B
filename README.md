# 방송 출연 식당 폐업 분류 예측

## 주제 선정 동기

연구에 따르면, 미슐랭 스타를 받은 식당은 폐업 가능성이 증가하는 경향이 있습니다. 이는 기대에 부응하기 위한 추가 비용과 대중의 장기적인 관심 감소가 원인으로 작용합니다. 본 프로젝트는 이러한 현상이 방송에 출연한 식당에도 유사하게 나타날 것이라는 가설에서 출발했습니다. 방송 출연 식당은 방영 직후 방문객이 급증하게 되고, 이에 따라 소비자의 높은 기대에 맞춰야 하지만, 시간이 지나면서 사람들의 관심이 줄어든다는 공통점을 가지고 있습니다.
이에 따라, 우리는 방송 출연 이후 폐업한 식당과 운영 중인 식당을 비교하여 폐업의 주요 요인을 분석하고자 합니다. 특히, 네이버 블로그, 유튜브 댓글, 구글맵 리뷰 등 SNS 데이터를 활용하여 고객 반응 중심의 요인 분석을 시도했습니다. 이 연구는 방송 출연 식당의 고객 유지 전략과 안정적인 영업 지속을 위한 인사이트를 제공하는 데 목적을 둡니다.

## 연구 문제  
- 방송 출연 식당의 폐업에 영향을 미치는 주요 요인은 무엇인가?  
  - 출연한 방송의 종류가 폐업 여부에 영향을 미쳤는가?  
  - 네이버 블로그, 유튜브 댓글, 구글맵 리뷰에서의 긍정 및 부정 반응이 폐업과 상관관계를 보이는가?  
  - 방송 출연 여부 외에 상권 요인이 더 큰 영향을 미쳤는가?  

## 연구 방법  
방송 출연 식당들의 상권 데이터, 출연 방송 정보, SNS 리뷰 및 댓글 데이터를 바탕으로 폐업 여부를 분류하고 예측합니다. 이를 통해 폐업 예측에 중요한 변수를 도출하고, 주요 요인들을 심층 분석합니다.  

### 1. 데이터 수집 및 전처리
**1) 분석 대상 식당 선정**  
    - 선정 기준: 유명 방송에 출연한 서울 소재 식당  
    - 대상 방송: 골목식당, 수요미식회, 6시 내고향, 생활의 달인, 맛있는 녀석들, 또간집(유튜브 정기 업로드)  
    - 선정 이유: 상권 간 차이를 최소화하기 위해 서울에 위치한 식당으로 제한  
    - 대상 식당: 폐업 식당과 운영 중인 식당 각각 20개를 랜덤으로 선정  
    
**2) 상권 데이터 수집**  
    - 출처: [서울열린데이터광장](https://data.seoul.go.kr/dataList/datasetList.do#)의 '상권분석서비스' 사용  
    - 방법: 분석 대상 식당의 행정동을 기준으로 상권 데이터를 매칭  
    - 사용 데이터 목록: 상권변화지표, 추정매출, 길단위인구, 직장인구, 상주인구, 집객시설, 소득소비, 점포, 아파트 정보 등  

**3) 네이버 블로그 게시글 크롤링**  
   - 데이터 수집 기준  
     검색 키워드를 ex) 시장횟집 - 골목식당 과 같이 식당 이름과 출연 프로그램 이름을 조합하여 제목을 검색한 뒤 
     네이버 블로그 API를 호출하여 관련 포스트 크롤링
      
      수집한 데이터는 다음의 내용을 포함  
      `title`: 블로그 포스트 제목  
      `link`: 블로그 포스트 URL  
      `description`: 검색 결과의 간단한 설명  
      `blogger_name`: 블로거 이름  
      `post_date`: 포스트 게시 날짜  
      `restaurant_name` : 식당이름  
      `program_name` : 출연 프로그램 이름  
  
**4) 유튜브 관련 영상 댓글 크롤링**  
    - 방송과 무관한 내용이 부정 감정을 과대평가할 가능성 있기 때문에 방송과 관련 없는 댓글(예: 사용자 간 언쟁)은 수동으로 삭제  
   
**5) 구글맵 리뷰 크롤링**  
    - 특징: 구글맵은 폐업한 식당의 리뷰 데이터를 삭제하지 않아 분석에 유리  
    - 방법: 분석 대상 식당에 대한 모든 리뷰 데이터를 크롤링  

### 2. 데이터 전처리
**1) 상권 데이터 전처리**  
    - 비슷한 의미의 칼럼은 삭제 Ex) 아파트 단지 수 & 총 가구 수
    - SMOTE : 수집된 데이터 내에서 폐업하지 않은 식당 대비 폐업한 식당의 수가 상대적으로 작다는 
              불균형 문제를 해결하기 위해 SMOTE 기법 적용

**2) 크롤링 데이터 전처리**  
    - 유튜브 댓글: 방송과 관련 없는 댓글(ex. 사용자들끼리의 언쟁)은 수동으로 삭제함. 방송과 관련 없는 내용으로 
      부정의 정도가 높아질 것을 우려함.  

### 3. 리뷰 감성 분석  
**1) 한국어 KNU 감성사전을 활용한 감성 분석**  
    - KNU 한국어 감성사전을 기반으로 새롭게 보완한 맞춤형 감성 사전을 구축 
      이를 활용하여 각 단어에 감성 점수를 부여하고, 리뷰 내 긍정과 부정의 비율을 산출
      긍정적인 리뷰와 부정적인 리뷰를 그룹으로 분류한 후, 각 단어마다 점수를 매겨 점수를 수치화
      도출된 감성 점수는 나머지 두 가지 감성 분석 방법론과 긍부정방향성을 일치시키기 위해 역수를 취함

**2) X-ANEW 데이터셋을 활용한 영어 감성 분석**  
    - 구글 실시간 번역 Url 을 가져와 매크로 편집기에 VBA 코드를 추가하는 방식 사용: 실시간 번역 함수 GoogleTranslate() 를 생성하여 
      크롤링한 블로그, 유튜브, 구글맵 리뷰 데이터들을 전부 영어로 번역  
    - 소문자 변환, 특수문자 제거, 토큰화, 불용어 제거 등 prepocess 수행  
    - 각 단어에 대해 score를 부여한 X-ANEW dataset 호출하여 점수 부여  
    - 단어가 전달하는 감정의 긍정,중립,부정 척도 (Valence) / 감정의 흥분도 혹은 자극 정도 (Arousal) 산출  

**3) 비정태적 CNN과 비지도 학습을 활용한 감성 분석**  
    - 임베딩: FastText 사전 학습된 한국어 임베딩 사용. 서브워드 기반으로 단어 OOV 문제를 완화하고, 리뷰 특성에 맞는 초기 임베딩 제공.  
    - 모델: Non-Static CNN 활용. 리뷰 내 로컬 패턴(n-그램)을 효과적으로 학습하고, 임베딩을 도메인 특화로 업데이트 가능.  
    - 분석 흐름: 리뷰를 문장 단위로 분리(Kkoma 활용) → 감성 분석 진행 → 반지도 학습으로 라벨링 데이터 신뢰도 강화.  
    - 데이터: AIHub 일반 담화 데이터로 학습, 네이버 블로그, 유튜브 댓글, 구글맵 리뷰 데이터를 사용해 최적화.  

### 4. 폐업 분류 예측 
    - 가장 우수한 성능을 보인 모델: 로지스틱 회귀 모델 
      특수한 case인 폐업 가능 여부를 정확하게 분류하는 것이 중요한 모델 특성상 재현율에 집중하였고
      실험결과 정확도 80.4% 민감도, 재현율 85.7%
    - 파라미터 어떻게 설정했는지 내용 추가하기  
    - 피처 엔지니어링 : 특정 방송 출연여부가 폐업여부에 영향을 미치는지를 관찰하기 위해 원핫인코딩 처리, 
                        보다 신뢰성 있는 모델 학습을 위해 폐업여부와 상관관계가 높은 컬럼 47개를 선정

## 결론  
**- 블로그**   
  Feature Importance 결과 상위 지표에 블로그 지표가 다수 포함되어 있었다는 점에서 긍정적인 ‘블로그’ 리뷰가 매우 중요
  고객들이 식당 방문 의사결정을 할 때에 가장 영향을 많이 미치는 리뷰 매체라고 해석되겠으며,
  이에 식당 운영 측에서는 긍정적인 블로그 리뷰 증가를 위해 블로그 체험단 등을 적극적으로 활용하는 등의 방안을 사용

**- 구글**  
  변수 중요도 2위인 구글 리뷰 역시 폐업여부에 중요한 영향을 미침을 확인
  이때, 타 SNS와 비교했을 때 외국인 이용자가 많은 구글리뷰 특성상 한국어 기반의 딥러닝과 감성사전이 상관관계가 낮아 배제
  반면, 영어 사전을 활용한 분석의 중요도는 높았음.
  즉, 구글맵 특성 상 폐업 방지를 위해서는 구글맵 리뷰의 영어기반 감성분석에 보다 신경을 써야 할 것

**- 유튜브**    
  초기 데이터 수집 단계에서, 방송 원본 혹은 크리에이터들이 창작한 식당 방문 브이로그 등의 댓글을 살펴보면
  폐업과 관련한 정보를 얻을 수 있을 것이라고 기대
  그러나, 분석 결과 유튜브를 활용한 감성 분석 변수들은 모두 상관관계가 낮은 변수로 예측 모델에서 제외됨
  이는 유튜브에서는 방송과 출연진에 관심이 집중되어 식당에 대한 평가가 유의미할 정도로 많지 않기 때문이라고 해석 가능

**- 상권**  
  상권 지표 중 ‘교육’, ‘유동인구 연령대’, ‘교통’이 유의미한 영향
  초등학교 중학교 수가 많으며 교육비 지출 비율이 높은 지역일수록,
  그리고 30~40대 유동인구가 많을수록 폐업 확률이 낮아짐을 확인
  마지막으로, 지하철역과의 거리보다도 버스 정류장의 밀도가 높은 것이 더 중요 

**- 상업 vs 주거**  
  모든 지표가 인구가 많을수록 폐업 확률이 낮음을 가리켰던 반면, 상주 인구의 경우 폐업 식당 인근에서 더 높았다.
  반면 직장인구의 경우 폐업한 식당 인근에서 더 낮음. 이 점에서 주거지역보다는 직장 인구가 많은 상업지역에서
  영업이 지속될 확률이 높다는 점 확인 
  
초중학교가 밀집되어 교육비 지출 비율이 높으며 3~40대 유동인구가 높은 버스 밀집 상업 지역이 
상권데이터 분석 결과 가장 효율적인 입점 고려사항이 될 것
  
**- 방송**  
  골목식당에 출연한 식당들의 폐업 비율이 타 방송에 비해 10배 이상 높음이 확인 가능.
  이는 골목식당 방송 특성에 기인한 것으로 판단, 장점을 위주로 부각시키는 타 방송에 비해
  운영의 미흡한 점을 노출시켜야 하는 골목식당은 해당 방송 출연이 폐업으로 이어질 확률이 높은 것으로 해석
  식당 운영의 측면에서 방송을 출연할 때 솔루션을 받는 형식 등 운영의 미흡한 점을 방송에 노출시키는 것을 매우 신중해야 할 것

**- 기대효과**  
   식당을 운영중이신 사장님께서는 특히 고객들의 블로그 리뷰와 구글리뷰의 영어감성 지표를 집중적으로 관리를 하셔야 함을 확인
   폐업 방지 솔루션은 폐업 위험군 식당을 초기에 발견하고 경각심을 가지고 개선의 필요성을 느껴
   운영 안정화와 생존율 향상에 기여할 수 있을 것으로 기대
   또한 외식업 뿐만 아니라 해당 모델을 향후 숙박업 등 다른 서비스 산업에도 적용 가능할 것으로 기대

