# Chip4-Stock-Analysis


- Towards interpretable stock trend prediction through causal inference. Expert Syst. Appl. 238(Part B): 121654 (2024) 논문을 참고하여 진행된 프로젝트
- 뉴스 데이터와 주가 데이터 연동성을 비교하기 위해 반도체 분야를 선택했고, Chip4 동맹이 최종적으로 체결되었던 2022년도 7월 뉴스 데이터를 기준으로 주가 데이터 등락 예측
- 종속변수: 1(이전날보다 상승),0(상승x)

  # 분석 과정 정리
  ### 0. 전처리
  - Word Journal Street에서 7월 뉴스 데이터 크롤링
  - 야후 파이낸셜에서 반도체 관련 주가 다운
 
  ### 1. 분석과정
1) Word2Vec을 통해 뉴스 데이터를 임베딩하고, K-means를 통해 클러스터링 진행.
2) 클러스터 별 단어에서 주가의 상승과 하락 날짜별 뉴스 헤드라인 내 단어의 등장횟수의 차이가 가장 큰 2개의 클러스터 선택해 추출
   - 진행이유: 뉴스 데이터에서 주가에 매우 높은 영향을 주는 단어(ex. stock, ...)는 제외하여야 하고, 상승과 하락에 따라 독특하게 발생하는 단어들을 필터링 하기 위함.
3) 인과추론 알고리즘 FGES를 이용해 Causal Graph의 추출 간선이 있는 단어노드만을 선택
   - 이 때, Causality의 특성상, 너무 많은 관측치로는 어떠한 인과성을 추출할 수 없으므로 랜덤 샘플링 진행 -> 여러번 진행 후, 가장 Domain Knowledge에 합당한 그래프 선택
4) 단어노드가 존재하는 헤드라인들만을 재추출하여 Hierarchical Attention Network로 최종 1,0 예측

### 2. 결과
- 모델 성능은 RocAuc로 선택하였고 0.8이라는 높은 수준의 예측도를 보임

# 아쉬운점
- 7월달로 한정하여 진행하였기 때문에 개장날이 얼마 없었고, 게다가 상승과 하락으로 나누고, train-test로 한번 더 나누니 클래스 별 관측치가 생각보다 적었다.
-   train 24개, test 5개

