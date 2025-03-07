# SKN09-EDA-2Team

<br>

# 0. Introduction Team (팀 소개)

### 🎥팀명 : 무빙

- "movie + bing = mobing"  영화계의 Bing(Microsoft Bing Search)
- 프로젝트 기간: 2025.01.15 ~ 01.21
- 팀원: 
<a href="https://github.com/youngseo98">김영서</a>, 
<a href="https://github.com/yujitaeng">유지은</a>, 
<a href="https://github.com/Hack012">전성원</a>, 
<a href="https://github.com/devunis">허정윤</a>
<br><br><br>

# 1. Introduction Project (프로젝트 개요)

### 🎥프로젝트 명

**무빙**: 데이터 기반 영화 제작 전략 분석

### 🎥프로젝트 소개

최근 20년간의 영화 데이터를 종합적으로 분석하여 영화 시장의 트렌드와 성공 요인을 도출하고, 이를 통해 영화 제작사들이 효과적인 의사결정을 할 수 있도록 지원하는 데이터 분석 프로젝트입니다. 마케팅, 데이터 분석, 홍보, 스토리 개발 등 영화 제작의 핵심 분야별 인사이트를 제공하여 실질적인 비즈니스 가치를 창출하고자 합니다.

### 🎥프로젝트 필요성(배경)

- 배경:
    - 영화 산업의 투자 리스크 증가와 관객 취향의 다변화
    - 데이터 기반 의사결정의 중요성 증대
    - 제작비 상승으로 인한 ROI 관리의 필요성
- 목표:
    - 객관적 데이터 분석을 통한 영화 시장 트렌드 파악
    - 장르별, 예산별 ROI 분석을 통한 최적 투자 전략 도출
    - 관객 선호도 분석을 통한 성공적인 콘텐츠 기획 가이드라인 제시
- 프로젝트에 사용된 데이터 출처:
    - 출처: IMDB Movies Dataset (1960-2023)
    - 링크:  https://www.kaggle.com/datasets/raedaddala/imdb-movies-from-1960-to-2023
    - 분석 범위: 2005-2024년 (최근 20년) 데이터
    - 주요 분석 항목:
        - 장르별 흥행 추이
        - 예산 대비 수익률
        - 관객 평점과 흥행의 상관관계

<br><br><br>

# 2. Data Pre-Processing

### :bar_chart: 사용 Dataset 
- Features 소개
  - Title: 영화 제목
  - Movie Link: 영화의 IMDB URL
  - Year: 영화 출시년도
  - Duration: 영화 상영시간
  - Votes: IMDB 상영자 투표 수
  - MPA: 영화 협회 상영 연령 제한 등급
  - Rating: IMDB 영화 평점
  - budget: 영화 제작 예산(USD)
  - grossWorldWide: 전 세계 박스오피스 수익
  - gross_US_Canada: 북미 박스오피스 수익
  - opening_weekend_Gross: 주말 개봉 영화 수익
  - directors: 감독 목록
  - writers: 작가 목록
  - stars: 주요 출연진
  - genres: 영화 장르
  - countries_origin: 제작 국가
  - filming_locations: 주요 촬영 장소
  - production_companies: 제작사
  - Language: 영화에서 사용된 언어
  - wins: 수상한 상의 개수
  - nominations: 총 후보 수
  - oscars: 오스카 n개 부문 수상 수
  - release_date: 영화의 공식 출시일

### :x: Step 1: 불필요 컬럼 삭제
- Movie Link: EDA에 불필요한 컬럼
- wins: 전체 데이터가 0인 컬럼
- release_date: 정확한 날짜가 없는 컬럼

### :arrows_counterclockwise: Step 2: 데이터 변환
- Duration
  - `MPA컬럼`에 잘못 들어간 데이터 처리  
  <img width="687" alt="Image" src="https://github.com/user-attachments/assets/11038981-86c8-4c20-97cc-219213295c79" />
  
  - 시간을 분단위로 통일 및 h, m 문자열 제거 -> **수치형(float64)** 데이터로 변환  
  ![Image](https://github.com/user-attachments/assets/3570907e-7caf-4e1c-9163-08276161ec04)
- Votes
    - K, M 문자열 제거 -> **수치형(float64)** 데이터로 변환  
  ![Image](https://github.com/user-attachments/assets/d105bb94-d501-4eec-9589-21a01b5ebd54)

### :o: Step 3: 결측치 대체
- 수익 관련 데이터 결측치 처리: **0으로 대체**
    - grossWorldWide
    - gross_US_Canada
    - opening_weekend_Gross
- 연령 제한 등급 관련 데이터 결측치 처리
  - MPA : Not Rated(등급 미심사)으로 처리
- 상영시간 관련 데이터 결측치 처리
  - Duration: 전체 상영시간의 **평균값(mean)**
- 빈 배열인 값 결측치 처리: **Unknown [컬럼명]**
  - directors
  - writers
  - stars
  - countries_origin
  - filming_locations
  - production_companies
  - languages
- 예산 데이터 결측치 처리
  - budget
    i) **장르별 평균 예산이 있지만 일부 예산 데이터가 결측치인 경우**: 같은 장르에 해당하는 평균 예산으로 대체  
    ii) **특정 장르의 예산이 결측치인 경우**: 전체 장르의 평균 예산으로 결측치 대체
- 평점 관련 데이터 결측치 처리: **0으로 대체**
  - Rating
  - Votes
 
##### [번외] Rating과 Votes의 결측치 Insight
i) 0과 평균값으로 Rating과 Votes를 채웠을 때 그래프  
![Image](https://github.com/user-attachments/assets/d52484dc-9feb-418f-91ee-2b2bdc694b3a)
![Image](https://github.com/user-attachments/assets/481251af-9826-47a1-946d-682e82c6dce3)
ii) 균등한 데이터 사용을 위해 이상치를 제거하고자 함 : **IQR** 방식 사용  
![Image](https://github.com/user-attachments/assets/e87cd5fd-38ae-45ab-93ba-67c49fbef2c4)
![Image](https://github.com/user-attachments/assets/6ee2390c-9ecc-4466-a7db-41119ee7396e)

:exclamation: 문제점: 이상치가 흥행에 성공한 영화의 값이라면 이상치 제거 후 데이터 분석 시 데이터 왜곡을 야기할 수 있음

iii) 이상치가 흥행에 성공한 영화의 값을 나타내고 있다고 생각이 듬
- 상위 10%, 하위 10%를 제외한 이상치 조정 방법 사용: **winsorize** 방식 사용  
![Image](https://github.com/user-attachments/assets/433a4803-650e-4889-9c27-40752f9cf176)
![Image](https://github.com/user-attachments/assets/f4d6a20c-af65-4553-95af-1488f3726261)

:exclamation: 문제점: 기대했던 것과 다르게 하위의 이상치가 명확히 잡히지 않음, 이상치 제거와 똑같이 데이터 왜곡을 야기할 수 있음

**결론**: 기존 데이터를 그대로 사용하되, 이상치의 분포가 0으로 채웠을 때와 평균값으로 채웠을 때 별반 다르지 않으므로 0으로 결측치를 대체하기로 결정
    
### :heavy_plus_sign: Step 4: 장르 그룹화
- `Category 컬럼` 추가
  - 시각화의 용이성, 장르의 세분화를 위해 

### 최종 데이터 생김새
- data.info()  
<img width="461" alt="스크린샷 2025-01-21 오전 9 24 30" src="https://github.com/user-attachments/assets/59fc61f0-fe48-4cd4-bbd6-cec59dbd72c6" />

# 3. EDA

## :busts_in_silhouette: Persona

### :bust_in_silhouette: Alex Kim
- 직업: 영화 제작사 홍보/마케팅 팀장 (PR & Marketing Manager)
- 목표: 시장 트렌드를 분석하여 관객들이 원하는 영화 장르와 특성을 파악하고, 투자 대비 수익성이 높은 영화 제작 아이디어를 도출하고자 함
- 관심사
  - 어떤 영화가 최근 트렌드에 부합하는지?
  - 흥행 성공 요인과 관련된 데이터 패턴 발견
  - 관객들이 선호하는 장르, 러닝타임, 배우 등 구체적인 요소 분석
  - 영화 제작 비용 대비 수익성을 극대화할 수 있는 전략
 
- 도식화

![Image](https://github.com/user-attachments/assets/4a8ee909-1ad6-40db-8a52-3cdbae0acfe7)
![Image](https://github.com/user-attachments/assets/fa471c9f-6c4a-48d9-9e00-293613853abc)
![Image](https://github.com/user-attachments/assets/80bdb9d6-0650-4579-b40e-0559abdb7e02)
### :bust_in_silhouette: Rachel Park
- 직업: 영화 제작사 스토리 개발 팀장 (Story Development Manager)
- 목표
  - 흥행 가능성이 높은 영화 스토리를 기획하고 개발 방향성을 제시
  - 기존 흥행 데이터를 바탕으로 관객이 선호하는 스토리와 장르를 파악
  - 영화 시나리오의 독창성과 시장성을 균형 있게 고려하여 성공적인 프로젝트를 기획
- 관심사
  - 관객들이 선호하는 스토리 요소(예: 감정, 서사, 결말 구조).
  - 특정 장르와 스토리 템플릿이 흥행 성공에 미치는 영향.
  - 감독, 작가, 배우의 조합이 스토리 매력에 미치는 효과 분석.
- 도식화

![output11](https://github.com/user-attachments/assets/1b990f12-d815-44af-898a-d3506ef93b3b)
![output12](https://github.com/user-attachments/assets/10859ca8-1f7c-4d81-a847-0ff1565ba38f)
![output13](https://github.com/user-attachments/assets/72671080-9d76-4ee6-8527-a80f4e57380f)

  
 ### :bust_in_silhouette: Sophia Jung
 - 직업: 영화 제작사 홍보/ 마케팅 팀장(PR & Marketing Manager)
 - 목표
    - 영화의 흥행 가능성을 높이기 위해 효과적인 홍보 및 마케팅 캠페인을 기획.    
    - 데이터 기반으로 타겟 관객층을 분석하고, 맞춤형 마케팅 전략을 수립.
    - 영화 개봉 전후 온라인과 오프라인에서 관객 참여를 극대화.
- 관심사
  - 수익성이 높은 영화의 특징을 분석해서 마케팅 전략을 수립
  - 수익성과 개봉일(평일 & 주말)의 관계에 따른 마케팅 전략 수립
  - 수상 경력, 수상 후보 등 영화의 품질이 수익성에 미치는 영향

- 도식화
- 예산과 수익: **예산(budget)** vs **전세계 수익(grossWorldWide)** 과 **북미 수익(gross_US_Canada)**
![예시 이미지](./img/sw_1.png)

- 등급과 수익: **연령 제한 등급(MPA)** vs **전세계 수익(grossWorldWide)**
- 등급 종류  
![예시 이미지](./img/MPA_rank.png)

<div>
    <img src="./img/sw_2.png" alt="예시 이미지" width="500" style="display:inline-block;"/>
    <img src="./img/sw_2_1.png" alt="예시 이미지" width="300" style="display:inline-block;"/>
</div>

- 영화 평점과 수익: **평점(Rating)** vs **전세계 수익(grossWorldWide)** 와 **주말 개봉 수익(opening_weekend_Gross)**
![예시 이미지](./img/sw_3.png)

- [추가분석]: **평점(Rating)** 별 / **총 수입(grossWorldWide)** 별 와 **주말 개봉 수익(opening_weekend_Gross)** 별 영화 분포도
![예시 이미지](./img/sw_3_ex.png)

    - 평점별 수익 top1 영화 변동사항 확인
      -> 평점별 주말 개봉 수익과 총 세계 수익의 관계에서 흥행한 영화의 변동사항을 확인하기 위함
      ![예시 이미지](./img/sw_3_ex2.png)
      
- **상 수상 여부(oscars)**, **수상 후보(nominations)** 와 **총 세계 수익(grossWorldWide)**
- oscars, nominations 분포도
![예시 이미지](./img/sw_4.png)

- grossWorldWide와 oscars, nominations 상관관계
![예시 이미지](./img/sw_4_1.png)

- oscar, nomination을 고려한 산점도
![예시 이미지](./img/sw_4_2.png)

- [추가분석]: **oscar 수상(oscars)** 을 다수 한 영화 **장르(genres)**  vs **총 세계 수입(grossWorldWide)** 이 가장 많은 **장르(genres)**

<div>
    <img src="./img/sw_4_ex.png" alt="예시 이미지" width="400" style="display:inline-block;"/>
    <img src="./img/sw_ex3.png" alt="예시 이미지" width="400" style="display:inline-block;"/>
</div>

- 데이터 프레임(Top5)
![예시 이미지](./img/sw_4_ex4.png)

#### :mag: 분석 Insight
- 많은 수익을 창출하고 싶은 경우: Action 장르, 평점 7점 이상의 영화를 홍보, 고예산 영화 위주로 마케팅 전략을 세워볼 수 있다.
- oscars 수상을 목표로 하는경우: Drama 장르의 영화를 마케팅

 ### :bust_in_silhouette: Emily Park
 - 직업: 영화 제작사 리서치 분석가 (Data Analyst)
 - 목표
   - 데이터 기반으로 신흥 시장 및 관객 니즈를 파악하여 새로운 영화 프로젝트의 성공 가능성을 높임
   - 과거 흥행 패턴을 분석해 제작사 내부에서 전략적인 의사결정을 지원
 - 관심사
   - 신흥 시장에서의 흥행 가능성이 높은 영화 장르 및 요소 분석
   - 낮은 제작비로 높은 수익을 낼 수 있는 영화 요소 탐구
   - 지역별로 효과적인 배급 및 마케팅 전략 수집
   - 데이터를 활용하여 영화 제작사 내부 보고서 작성 및 시각

 - 도식화

![3](https://github.com/user-attachments/assets/8ff3a30c-15b5-4b45-802b-05c231c584a9)
![output](https://github.com/user-attachments/assets/e8202ca7-3315-4b13-8a2f-cc0dbcf25c4e)
![1](https://github.com/user-attachments/assets/7bd01d5d-a69f-4435-94a6-3cb370b92dd5)
![2](https://github.com/user-attachments/assets/ea31ece6-88d7-4636-bdaf-57f32402c455)
 <br><br><br>

# 4. 결론
## 장르별 흥행 평점
- ### Drama 장르와 고예산 액션장르 흥행 가능성이 높음
- ### 저예산 영화 공포 장르 흥행 가능성 높음
- ### Epic 장르가 수상과 수익이 가장 높음
## 예산대비 수익율 
- ### 선형적 우상향 비례관계
## 관객평점과 흥행의 상관관계
- ### 높은 평점을 받은 영화는 일반적으로 높은 수익 창출

 # 5. 한줄 회고
 - :green_heart: 김영서 :  데이터가 매우 크고 다양한 특성을 가지고 있어서 EDA 과정에서 불필요한 정보가 많았습니다. 특정 변수에서 결측값이나 이상치가 많아 정리가 힘들었고, 변수간의 관계를 분석할 때 각 변수의 스케일 차이가 많았습니다. 하지만 팀원들과 함께 결측치를 미리 처리하고 전처리 절차를 하나씩 밟아가면서 필요한 데이터를 뽑은 것에 뿌듯함을 느꼈고, 다음엔 더욱 간단하고 자동화된 전처리 절차를 도입해보고 싶습니다.
- 💙 유지은 : 빅데이터 분석 과정에서 결측치와 이상치를 프로젝트 목표에 맞게 대체하는 방법을 학습했으며, 페르소나 가설을 설정하고 이를 바탕으로 유의미한 인사이트를 도출하는 시각화 작업을 경험했습니다. 특히, 인사이트를 효과적으로 전달하기 위해 subplot을 활용하여 연관성 있는 두 그래프를 한 화면에 배치해 결론을 보다 직관적으로 도출하는 방법을 터득했습니다. 또한, 팀원들과 코드리뷰를 하며 hue 값을 활용해 상관관계가 있는 세 가지 데이터를 한 그래프에 시각화하는 기술도 익혔습니다. 이러한 EDA 전반 과정을 직접 시도하며 분석 역량을 한 단계 높일 수 있었던 뜻깊은 시간이었습니다. 😊
- :black_heart: 전성원 :  좋은 팀원들과 함께한 덕분에 EDA를 진행할 때 분석의 시야를 넓힐 수 있었습니다. 비록 프로젝트 마감 기한이 짧아 모든 것을 완벽히 보여주지 못한 점은 아쉽지만, 이번 경험을 발판 삼아 파이썬 분석 라이브러리(numpy, pandas 등)와 시각화 그래프에 대한 정보를 보충한다면 앞으로 더욱 좋은 성과를 낼 수 있을 것이라는 자신감을 얻었습니다. 🙂
- :purple_heart: 허정윤 : matplotlib를 사용하여 도식화를 진행하면서 Deprecated Warning이 발생하는 부분이 많이 있었습니다. 해당 Warning을 분석하고 수정해 나가면서 EDA를 완성해 나가니 프로젝트의 완성도가 더 높아지는 걸 느낄 수 있었습니다.
영화 장르나 배우 정보 데이터들이 리터럴 배열로 들어가 있는 걸 확인했는데 해당 데이터를 replace, strip. split등의 함수를 사용하고 One-Hot Encoding 방식을 통해 (pd.get_Dummies) 정제하여 통계를 내는 부분을 잘 익힐 수 있었어요.
