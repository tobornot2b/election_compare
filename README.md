# 21대 총선 vs 20대 대선 득표율 비교
- 자료출처
    - 중앙선거관리위원회 국회의원선거 개표결과 정보_20200415 (총선)
    - 중앙선거관리위원회 홈페이지 20대 대통령 선거 개표결과 (대선)
    
## 고려할 사항

1. 얼마나 상세히 보여줄 것인가?
    1) '시군구'
        - 기본적인 단위
    2) '읍면동'
        - 실질적으로 가장 상세한 단위
        - 제대로 분석하려면 이 단위까지 내려가야 하는 것이 맞다.
        - 이 단위까지 표기하면 실제 지역 특성이 나타나기 시작한다.
        - 정보량이 많아서 모바일에서는 느리다. PC에서도 느린게 느껴진다.
        - geojson 파일을 다루기도 쉽지 않은 편.
    3) '투표소'
        - 가장 상세한 단위
        - 실제 구획으로는 '통', '반' 으로 진입하게 된다.
        - 우리나라의 주소체계는 3개. '법정동', '행정동', '도로명'
        - 행정동과 법정동은 서로 완전한 포함관계가 아님.
        - 투표소는 '통', '반' 으로 설정되어 있음.
        - 구청 홈페이지에 가면 '통', '반' 의 구획을 찾을 수 있음        
        - 그래서 실제로 찾아보면 한 아파트 단지에서 층수로 '통', '반'이 나뉨. (물리적으로 수직 구조가 존재)
        - 사실상 2차원 평면으로 표기하기 어렵다.
2. 선거인 수는 적지만 넓은 지역에서 오는 왜곡을 어떻게 보정할 것인가?
    1) 면적은 넓고 인구는 적은 곳은 과대하게 보이는 왜곡이 있다.
    2) 카토그램 같은 것으로 보정이 가능하다.
    3) 투표수 기준으로 표기하면 인구적은 넓은 지역이 과소평가되는 경향이 생긴다.
3. 결론 : 그냥 기본(시군구)으로 진행


## 1. 21대 총선 자료

### 1) 로직
- 지역구별 분리된 엑셀파일 형식을 하나로 모은다.
    1. 먼저 하위폴더명을 따서 folders 에 저장한다.
    2. url 뒤에 붙여서 저장된 폴더들을 순회한다.
    3. 폴더에 진입하면 다시 해당폴더에 든 파일명을 files 에 저장한다.
    4. url 뒤에 붙여서 저장된 파일들을 순회한다.
    5. 파일을 열면 다시 해당엑셀파일에 있는 시트명을 sheets 에 저장한다.
    6. url 뒤에 붙여서 엑셀파일 안에 있는 모든 시트를 순회한다. (이번 경우는 시트 1개)
    7. 엑셀파일을 데이터프레임으로 불러온다.
    8. 정보가 될 몇 개의 column 을 새로 생성한다.
        - '시도'는 폴더명에서 가져옴
        - ~~'구시군'은 파일명에서 잘라냄~~ 총선 자료파일의 2번째 라인에서 가져다 쓰는 것으로 변경
    9. 시트 하나를 읽으면 메인 데이터프레임에 concat 으로 가져다 붙인다.
    10. 위의 과정을 반복하여 3단 for문으로 모두 읽어온다.
    
    
## 2. 20대 대선 자료

### 1) 로직
- 기본적으로 국회의원 선거와 같으나 정식자료가 나오지 않은 상태에서 홈페이지 데이터만 긁어온 상태
- 지역구별 구획이 아니다. 총선자료와는 '시군구2' 항목으로 맞춰야 한다.


- 1_ 로 시작하는 파일은 로컬에서 엑셀파일로 데이터처리
- 2_ 로 시작하는 파일은 웹에서 스크랩하여 데이터처리