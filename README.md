# 방송 출연 식당 폐업 분류 예측

## 주제 선정 동기
연구에 따르면, 미슐랭 스타를 받은 식당은 폐업 가능성이 증가하는 경향이 있습니다. 이는 기대에 부응하기 위한 추가 비용과 대중의 장기적인 관심 감소가 원인으로 작용합니다. 본 프로젝트는 이러한 현상이 방송에 출연한 식당에도 유사하게 나타날 것이라는 가설에서 출발했습니다. 방송 출연 식당은 방영 직후 방문객이 급증하고 높은 기대를 맞춰야 하지만, 시간이 지나면서 관심이 줄어드는 공통점을 가지고 있습니다.
이에 따라, 우리는 방송 출연 이후 폐업한 식당과 운영 중인 식당을 비교하여 폐업의 주요 요인을 분석하고자 합니다. 특히, 네이버 블로그, 유튜브 댓글, 구글맵 리뷰 등 SNS 데이터를 활용해 고객 반응 중심의 요인 분석을 시도했습니다. 이 연구는 방송 출연 식당의 고객 유지 전략과 안정적인 영업 지속을 위한 인사이트를 제공하는 데 목적을 둡니다.

## 연구 문제
- 방송 출연 식당의 폐업에 영향을 미치는 주요 요인은 무엇인가?
  - 출연한 방송의 종류가 폐업 여부에 영향을 미쳤는가?
  - 네이버 블로그, 유튜브 댓글, 구글맵 리뷰에서의 긍정 및 부정 반응이 폐업과 상관관계를 보이는가?
  - 방송 출연 여부 외에 상권 요인이 더 큰 영향을 미쳤는가?

## 연구 방법
**0. Summary**

방송 출연 식당들의 상권 데이터, 출연 방송 정보, SNS 리뷰 및 댓글 데이터를 바탕으로 폐업 여부를 분류하고 예측합니다. 이를 통해 폐업 예측에 중요한 변수를 도출하고, 주요 요인들을 심층 분석합니다.

**1. 데이터 수집 및 전처리**
1) 분석 대상 식당 선정
    - 선정 기준: 유명 방송에 출연한 서울 소재 식당
    - 대상 방송: 골목식당, 수요미식회, 6시 내고향, 생활의 달인, 맛있는 녀석들, 또간집(유튜브 정기 업로드)
    - 선정 이유: 상권 간 차이를 최소화하기 위해 서울에 위치한 식당으로 제한
    - 대상 식당: 폐업 식당과 운영 중인 식당 각각 20개를 랜덤으로 선정
    
2) 상권 데이터 수집
    - 출처: [서울열린데이터광장](https://data.seoul.go.kr/dataList/datasetList.do#)의 '상권분석서비스' 사용
    - 방법: 분석 대상 식당의 행정동을 기준으로 상권 데이터를 매칭
    - 사용 데이터 목록: 상권변화지표, 추정매출, 길단위인구, 직장인구, 상주인구, 집객시설, 소득소비, 점포, 아파트 정보 등

3) 네이버 블로그 게시글 크롤링
  
4) 유튜브 관련 영상 댓글 크롤링
    - 방송과 무관한 내용이 부정 감정을 과대평가할 가능성 있기 때문에 방송과 관련 없는 댓글(예: 사용자 간 언쟁)은 수동으로 삭제
   
5) 구글맵 리뷰 크롤링
    - 특징: 구글맵은 폐업한 식당의 리뷰 데이터를 삭제하지 않아 분석에 유리
    - 방법: 분석 대상 식당에 대한 모든 리뷰 데이터를 크롤링

**2. 데이터 전처리**
1) 상권 데이터 전처리
    - 비슷한 의미의 칼럼은 삭제(예시 하나만 추가하기!!!!)

2) 크롤링 데이터 전처리
    - 유튜브 댓글: 방송과 관련 없는 댓글(ex. 사용자들끼리의 언쟁)은 수동으로 삭제함. 방송과 관련 없는 내용으로 부정의 정도가 높아질 것을 우려함.

**3. 리뷰 감성 분석**
1) 한국어 KNU 감성사전을 활용한 감성 분석
2) 영어 버전~
3) 비정태적 CNN과 비지도 학습을 활용한 감성 분석
  - 임베딩: FastText 사전 학습된 한국어 임베딩 사용. 서브워드 기반으로 단어 OOV 문제를 완화하고, 리뷰 특성에 맞는 초기 임베딩 제공.
  - 모델: Non-Static CNN 활용. 리뷰 내 로컬 패턴(n-그램)을 효과적으로 학습하고, 임베딩을 도메인 특화로 업데이트 가능.
  - 분석 흐름: 리뷰를 문장 단위로 분리(Kkoma 활용) → 감성 분석 진행 → 반지도 학습으로 라벨링 데이터 신뢰도 강화.
  - 데이터: AIHub 일반 담화 데이터로 학습, 네이버 블로그, 유튜브 댓글, 구글맵 리뷰 데이터를 사용해 최적화.
