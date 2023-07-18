<!-- #============================================================ -->
# NewsAnalysis v3
<!-- #============================================================ -->

UPV 대학원 석사 졸업 논문


<!-- #============================================================ -->
## Intro | 개요
<!-- #============================================================ -->





<!-- #============================================================ -->
## Model 및 그 개념 정의.
<!-- #============================================================ -->

### Media

언론사 정보

1. 자료구조
언론사명(name)
언론사URL(url)

### MediaNewsTarget

MediaNewsPage의 자료구조를 제공.

### MediaNewsPage

언론사별 시간대별 뉴스페이지들

1. 자료구조
뉴스페이지명(name)
뉴스페이지-URL(url)
수집일시(collect_dt)
수집화면HTML(html)

### NewsPageLayout

각언론사의 뉴스페이지의 화면배치.
- 어떤 뉴스(기사)가 해당 뉴스페이지 내에서 어떠한 위치를 차지하는가.
- 각 언론사들이 해당 화면에 어떻게 뉴스(기사)를 배치하였는가.

MediaNewsPage 의 HTML을 파싱한 후 저장.

모든 언론사의 웹페이지 '화면'을 수집.
뉴스 자체의 페이지(즉, 제목, 본문를 담고 있는)는 개별 언론사 모듈 수집한다.
왜냐하면 수집시간은 다르지만 동일한 뉴스를 수집할 수도 있기 때문이다.
URL을 로딩해서 순차적으로 하나씩 단위수집
- 네이버_모바일홈_뉴스영역_수집
- 네이버_모바일뉴스_홈_수집
- 네이버_모바일뉴스_정치_수집
- 네이버_모바일뉴스_경제_수집


어떤 화면에서 어떤 개별 뉴스의 배치정보를 통합하여 저장관리.
- layout TBL의 자료구조 제공.
- 수집대상 뉴스들을 검색로딩.
- 수집완료한 뉴스들을 표시.
========== 뉴스의 화면배치 필수정보 및 그 정의 ==========
'layout' TBL 자료구조.
뉴스제목 컬럼이 필요한 이유 : 같은 뉴스제목을 가졌더라도 네이버의 장난질에 의해서 href가 다른 경우가 종종 있다.
페이지명 : 홈페이지, 뉴스홈, 정치면, 경제면, etc.
첫화면_배치여부 : 단말기의 화면제약 때문에 중요성이 커진 단말기 화면 크기에 초기 로딩시 보여지는 첫화면
영역 : 한개 페이지 내의 가장 높은 수준에서의 블럭
- 영역배치순위
- 영역명
수집일시
뉴스
- 뉴스배치순위 : 한개 영역 내에서 뉴스가 배치된 순위. 주로 탑다운 방식으로 전개된다.
- 뉴스_url

screen의 파싱은, 개별 언론사 모듈에서 한다.
여기서는 파싱의 공통 함수만 제공한다.

URL에 따라 수집한 화면이 다르므로 다른 파서를 사용해야 한다.
예를들어, "네이버_모바일홈" 화면은 "네이버_모바일홈_파서"를 사용해야 한다.

[ LayoutParser ]

첫화면과 첫페이지는 다른 개념이다. 화면이란 screen 이고 페이지는 첫 로딩시 로드되는 데이터 블록이다.
네이버 모바일 뉴스홈 첫페이지의 태그 분석
상하단 두개의 블럭 : class_=r_group_reset
많이 본 뉴스 : class_='r_group_comp_popular'


### Article

뉴스기사.
기사제목, 기사본문, 기자정보, 기사에 대한 여론정보.
what : 제목, 본문
who : 기자
event : { 다루고있는 기사의 사건은 무엇인가.
   who : 사건의 등장인물
   what : 사건의 주제
   when : 사건 발생일시
   how : 사건의 경과 내용
}
layout TBL에서 미수집한 뉴스들을 수집저장.
개별 뉴스기사를 파싱할 때는 언론사 모듈을 이용해서 파싱.
news TBL에 대한 분석, 보고 작업들.


<!-- #============================================================ -->
## Algorithms used in this project 
<!-- #============================================================ -->

1. K-Means Clustering

2. MeansShift Clustering 

3. Linear Model Perceptron Classification 

    sklearn.linear_model.Perceptron




<!-- #============================================================ -->
## Packages.
<!-- #============================================================ -->

### etri

1. 일반정보
- Open API 서비스 : http://aiopen.etri.re.kr/service_api.php
    - 언어 분석 API
    - 어휘관계 분석 API
    - 질문 분석 API
    - 음성인식 API
- 말뭉치 공개 서비스 : http://aiopen.etri.re.kr/service_corpus.php


2. 어휘 정보 API
= "http://aiopen.etri.re.kr:8000/WiseWWN/Word"
 "argument": {
    "word": “YOUR_WORD”
}
3. 어휘 관계분석 API
= "http://aiopen.etri.re.kr:8000/WiseWWN/WordRel"
"argument": {
    “first_word”: “FIRST_YOUR_WORD”,
    “first_sense_id”: “FIRST_WORD_SENSE_ID”,#첫 번째 어휘의 의미 코드
    “second_word”: “SECOND_YOUR_WORD”,
    “second_sense_id”: “SECOND_WORD_SENSE_ID”#두 번째 어휘의 의미 코드
}
4. 동음이의어 정보 API
= "http://aiopen.etri.re.kr:8000/WiseWWN/Homonym"
 "argument": {
    "word": “YOUR_WORD”
}
5. 다의어 정보 API
= "http://aiopen.etri.re.kr:8000/WiseWWN/Polysemy"
 "argument": {
    "word": “YOUR_WORD”
}

6. 언어 분석 API
http://aiopen.etri.re.kr/doc_language.php


언어 분석 API는 요청된 자연어 문장의 분석된 결과를 JSON 형태의 Text 데이터로 반환합니다.

형태소 분석 : “morp”,
어휘의미 분석 (동음이의어 분석) : “wsd”
어휘의미 분석 (다의어 분석) : “wsd_poly”
개체명 인식 : “ner”
의존 구문 분석 : “dparse”
의미역 인식 : “srl”

분석 명 | 분석 코드 | 포함되는 내용
형태소 분속 | morp |	형태소 분석 결과
어휘의미 분석(동음이의어) | wsd | 형태소 분석 결과, 어휘의미 분석 결과 (동음이의어 분석)
(동음이의어에 대한 상세정보 조회는 동음이의어 정보 API 사용)
어휘의미 분석(다의어) | wsd_poly | 형태소 분석 결과, 어휘의미 분석 결과 (다의어 분석)
(다의어에 대한 상세정보 조회는 다의어 정보 API 사용)
개체명 인식 | ner | 형태소 분석 결과, 어휘의미 분석 결과 (동음이의어 분석), 개체명 인식 결과
의존 구문 분석 | dparse | 형태소 분석 결과, 어휘의미 분석 결과 (동음이의어 분석), 개체명 인식 결과, 의존 구문 분석 결과
의미역 인식 | srl | 형태소 분석 결과, 어휘의미 분석 결과 (동음이의어 분석), 개체명 인식 결과, 의존 구문 분석 결과, 의미역 인식 결과

### 어휘관계분석_API [lexicalRelation_analysis_api]
http://aiopen.etri.re.kr/doc_language.php
