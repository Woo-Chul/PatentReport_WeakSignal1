# <img src="/img/kipi_logo.png" width="5%" height="3%" title="px(픽셀) 크기 설정" alt="kipi" align="left">KIPI DATA INSIGHT</img>

특허 DATA를 활용한 미래 기술 위크시그널 성장예측 분석
======================

> 이 글은 '[**KISTI DATA INSIGHT 제19호 : 미래기술 위크시그널 성장예측보고서**](https://www.kisti.re.kr/post/data-insight?t=1668036309836)'와 
> '[**KISTI DATA INSIGHT 제15호 : 미래기술 위크시그널**](https://www.kisti.re.kr/post/data-insight?t=1668036309836)'의 내용을 많은 부분 참고하였고 인용하였습니다.
> 아래 설명하는 실험들은 특허데이터만을 적용한 내용입니다.
> 또한 KISTI 미래기술 위크시그널 분석 방법이나 데이터에서 차이가 있음을 미리 알려드립니다. 


</br>
</br>
<p align="center">
<img src="/img/weaksignal_cover_19.png" width="40%" height="30%" title="px(픽셀) 크기 설정" alt="cover19" align="center"></img>
</p>
</br>
</br>

# Chaper 1. 미래기술 위크시그널
## 1. 위크시그널
> 위크시그널의 개념은 전략적 관리자로 알려진 Ansoff의 "Managing Strategic Surprise by Response to Weak Signals"라는 논문에서 기업이 외부변화로부터 받게 될 충격을 관리하기 위
> 해 충격을 야기할 수 있는 작은 신호로 사용되었습니다. 경험적 외삽으로는 설명하기 어렵고 그 중요성이 불분명하지만, 미래에 일어날 일들에 대한 정보를 담고 있는 작은 징후로 보기도 
> 합니다. 따라서 예전에는 전문가들이 토론을 거쳐서 결정하였고 급변하는 기술환경에서 빠르게 자동으로 판단하기 쉽지 않았습니다. 이에 따라 KISTI연구진은 위크시그널을 자동 탐지하는 방
> 법을 시도해보았고 한국특허정보원에서도 국내 특허데이터로 실험을 진행해보았습니다.

### 1.1 위크시그널 자동탐지 프로세스
이 글에서는 혼동을 방지하기 위해 참고한 KISTI 미래기술 위크시그널 방식은 'KISTI 위크시그널'로 명명하고 특허데이터를 적용한 방식은 'KIPI 위크시그널'로 구분하여 설명드리겠습니다.

> ```KISTI 위크시그널 자동탐지 프로세스```
<p align="center">
<img src="/img/KISTI_process.png" width="100%" height="100%" title="px(픽셀) 크기 설정" alt="kisti" align="center"></img>
</p>
</br>
</br>

> ```KIPI 위크시그널 자동탐지 프로세스```
<p align="center">
<img src="/img/KIPI_process.png" width="100%" height="100%" title="px(픽셀) 크기 설정" alt="kipi" align="center"></img>
</p>

### 1.2 위크시그널 자동탐지 현황

#### 1.2.1 실험 데이터

- 1999년 ~ 2018년 
- 특허 공개공보 데이터 
- CPC(Cooperative Patent Classification)에서 A section인 건만 대상
- 전체 3,360,316건 중 505,560건
- 대상 필드는 발명의 명칭, 요약, 배경기술, 기술분야로 적용

<p align="center">
<img src="/img/target_patent_docs.png" width="100%" height="100%" title="px(픽셀) 크기 설정" alt="taget" align="center"></img>
</p>
</br>

#### 1.2.2 키워드 추출

> SoyNLP L-[R] 명사 추출기 사용

- 버전 : SoyNLP 0.0493
- 통계기반의 미등록 용어문제 해결, L-[R] Graph 구조를 활용한 명사 추출
- Noun Extractor를 이용한 명사 및 복합명사 추출 → LTokenizer를 이용한 단어 분리(KorPatBERT Tokenizer에서도 Mecab형태소 분석기 사용자 사전 용어 등록 시 활용)
- LTokenizer 사용자 사전 명사 : 684,938건

> KorPatBERT CPC 분류기 모델 사용

- LTokenizer 사용자 사전 복합 명사 : 959,757건
- 총 구축 명사 1,644,694건
- Noun Score > 0.8
- 대상 문헌 기준 386,904건의 명사 추출

<p align="center">
<img src="/img/ex_keyword_extract.png" width="100%" height="100%" title="px(픽셀) 크기 설정" alt="taget" align="center"></img>
</p>
</br>

#### 1.2.3 키프레이즈 추출

> 실험 대상 기준 키프레이즈 추출

- 대상필드 : 발명의 명칭, 요약, 배경기술, 기술분야 필드
- 키 프레이즈란 약 2~4 내의 단어로 이루어진 의미적으로 완전한 어구
- 워드단위 < 프레이즈 단위 < 문장단위 

<p align="center">
<img src="/img/key_phrase.png" width="100%" height="100%" title="px(픽셀) 크기 설정" alt="taget" align="center"></img>
</p>
</br>

#### 1.2.4 Poping 키워드 추출

> 경향성 = 빈도수(후반3년) - 빈도수(전반3년) > 10

> 규모성 = 빈도수(최근3년) >= 40

> 활동성 = 빈도수(최근3년/20년) >= 0.6

<p align="center">
<img src="/img/poping_keyword_extract.png" width="100%" height="100%" title="px(픽셀) 크기 설정" alt="taget" align="center"></img>
</p>
</br>

> 경향성 0초과 키워드 [135,809건] 대상

<p align="center">
<img src="/img/poping_keyword_extract_target.png" width="100%" height="100%" title="px(픽셀) 크기 설정" alt="taget" align="center"></img>
</p>
</br>

> BM25기반, TF-IDF기반 Poping 키워드 예시

<p align="center">
<img src="/img/poping_keyword_extract_ex.png" width="100%" height="100%" title="px(픽셀) 크기 설정" alt="taget" align="center"></img>
</p>
</br>

> Poping 키워드 추출 결과

<p align="center">
<img src="/img/poping_keyword_extract_result.png" width="100%" height="100%" title="px(픽셀) 크기 설정" alt="taget" align="center"></img>
</p>
</br>

#### 1.2.5 Poping 키프레이즈 추출

> PS-IDS

- PS(Phrase Similarity) : 키프레이즈-문서간의 유사도
- IDS(Inversed Document Similarity) : 키프레이즈와 총문서들간의 유사도 역수
- TF-IDF의 term을 키프레이즈로 변경

<p align="center">
<img src="/img/tf-idf.png" width="50%" height="50%" title="px(픽셀) 크기 설정" alt="taget" align="center"></img>
</p>

> Poping 키프레이즈 후보 추출 조건

<p align="center">
<img src="/img/poping_keyphrase_hubo.png" width="100%" height="100%" title="px(픽셀) 크기 설정" alt="taget" align="center"></img>
</p>

> 키프레이즈 PS-IDS 분포

- Log & Max scaling 기준 > 0.67 키프레이즈 : 73,313건
- 프레이즈 내부 키워드 별 경향성 합계 > 0  : 21,880 건

<p align="center">
<img src="/img/poping_keyphrase_hubo2.png" width="100%" height="100%" title="px(픽셀) 크기 설정" alt="taget" align="center"></img>
</p>

> Poping 키프레이즈 추출 조건

<p align="center">
<img src="/img/poping_keyphrase_final.png" width="100%" height="100%" title="px(픽셀) 크기 설정" alt="taget" align="center"></img>
</p>

> Poping 키프레이즈 후보군 규모성 & 활동성 분포

- 키워드 별 규모성 지표 평균 >= 8 and 키워드 별 활동성 지표 평균 >= 0.5 : 575건

<p align="center">
<img src="/img/poping_keyphrase_final2.png" width="100%" height="100%" title="px(픽셀) 크기 설정" alt="taget" align="center"></img>
</p>

> doc1-gram1 PS-IDS Score = 0.9 x log(3/0.9+0.3+0.2)+1)

> 연산율 감소 방안

- Gram vector = mean(noun1_v1, noun2_v, noun3_v)
- IDS 용 doc vector = 1/10 of total doc vector

<p align="center">
<img src="/img/ps-ids.png" width="100%" height="100%" title="px(픽셀) 크기 설정" alt="taget" align="center"></img>
</p>
</br>



#### 1.2.6 Poping 키워드&키프레이즈 검증

> 상위 6개의 경향이 대부분 동일함

> A24의 경우 타분류와 대비성이 강한 특징으로 유달리 높은 유사도 결과에 기인

> A24와 같이 유사코드가 많은 경우 더욱 유사도 관점에서 높은 키워드 존재

> 추출 결과가 출원 통계대비 크게 벗어나지 않은 결과를 보임

<p align="center">
<img src="/img/poping_keyword_phrase_check1.png" width="100%" height="100%" title="px(픽셀) 크기 설정" alt="taget" align="center"></img>
</p>

<p align="center">
<img src="/img/poping_keyword_phrase_check2.png" width="100%" height="100%" title="px(픽셀) 크기 설정" alt="taget" align="center"></img>
</p>

#### 1.2.7 일반 Poping 키워드

#### 1.2.8 신규 Poping 키워드

#### 1.2.9 Connected components & Cliques

#### 1.2.10 위크시그널 추출

### 1.3 Focus Index 추출


## 2. 위크시그널 성장예측모델

### 2.1 학습모델: Graph Convolutional Neural Network

### 2.2 학습데이터: 기준데이터, 성장데이터, 데이터 증강

### 2.3 학습모델 성능 및 2022 위크시그널에 대한 성장성 예측결과

# Chaper 2. 위크시그널 126 : 10년 후 고성장이 예측된 126개 위크시그널


