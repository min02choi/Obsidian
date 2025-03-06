
## 1. WebQSP (WebQuestionsSP) 데이터셋

### 개요
WebQSP(WebQuestions Semantic Parsing)는 구글에서 제공하는 WebQuestions 데이터셋을 기반으로 만들어진, 지식 그래프 기반 질의 응답을 위한 데이터셋이다. 질문을 의미론적 구문 분석(semantic parsing)과 SPARQL 쿼리로 변환하는 연구에 활용됩니다.
### 특징
- **출처**: Freebase를 활용한 QA 데이터셋
- **질문 유형**: 단순 질문(Simple questions)이 많지만, 의미론적 구문 분석을 고려한 데이터 포함
- **구성**: 자연어 질문, 의미론적 분석, SPARQL 쿼리 제공
- **규모**: 4,737개의 질문 포함
### 데이터셋 내용
- **자연어 질문(Natural Language Questions)**: 사용자가 입력하는 단순 질의
    - 예시: "Who wrote Harry Potter?"
    - 예시: "What is the capital of France?"
- **SPARQL 쿼리(SPARQL Query)**: Freebase에서 해당 질문을 처리하기 위한 구조화된 질의
- **정답(Answers)**: SPARQL 쿼리 실행 결과로 도출된 정답
- **의미론적 분석(Semantic Parsing Annotation)**: 질문을 분석하여 주요 개체(Entity) 및 관계(Relation) 태깅
### 활용
- **지식 그래프 질의 응답 연구**
- **자연어 질문을 SPARQL 쿼리로 변환하는 기술 개발**
- **의미론적 구문 분석 및 NLP 평가 벤치마크로 사용**
### 참고 논문
- Yi Yang, Wen-tau Yih, and Christopher Meek. "Semantic Parsing on Freebase from Question-Answer Pairs." EMNLP 2015

***
## 2. CWQ (Complex Web Questions) 데이터셋

### 개요
CWQ(Complex Web Questions) 데이터셋은 복잡한 자연어 질문과 이에 대한 구조화된 쿼리를 포함하는 데이터셋이다. 기존의 단순한 질문-응답(QA) 데이터셋과 달리, CWQ는 **다단계 reasoning(추론)** 이 필요한 복합적인 질문을 다룬다.
### 특징
- **출처**: Freebase를 기반으로 한 질의 응답 데이터셋
- **질문 유형**: 다중 관계(Multi-relation), 복합 논리(Complex logical), 비교(Comparison) 등의 다양한 질문 유형 포함
- **구성**: 자연어 질문과 이에 해당하는 SPARQL 쿼리 제공
- **규모**: 34,689개의 질문 포함
### 데이터셋 내용
- **자연어 질문(Natural Language Questions)**: 사용자가 입력하는 실제 질문
    - 예시: "Which films directed by Christopher Nolan won an Oscar?"
    - 예시: "What are the capitals of countries that border Germany?"
- **SPARQL 쿼리(SPARQL Query)**: 해당 질문에 대한 Freebase 기반의 구조화된 질의
- **정답(Answers)**: SPARQL 쿼리 실행을 통해 얻어진 정답
### 활용
- **지식 기반 질의 응답(KBQA, Knowledge Base Question Answering)** 연구
- **SPARQL 변환 및 자연어 처리(NLP) 연구**
- **복합 질문 처리 능력을 평가하는 벤치마크로 활용**
### 참고 논문
- Alon Talmor, Jonathan Berant. “The Web as a Knowledge-base for Answering Complex Questions.” NAACL 2018

---
### 비교 요약

|데이터셋|출처|질문 개수|질문 유형|주요 내용|활용 분야|
|---|---|---|---|---|---|
|CWQ|Freebase|34,689|복합 질문 (Multi-relation, Logical, Comparison)|자연어 질문, SPARQL 쿼리, 정답|KBQA, SPARQL 변환, NLP 연구|
|WebQSP|Freebase|4,737|단순 및 의미론적 구문 분석 고려|자연어 질문, SPARQL 쿼리, 정답, 의미론적 분석|KBQA, SPARQL 변환, NLP 연구|

CWQ는 다단계 추론이 필요한 복잡한 질문을 다루는 반면, WebQSP는 보다 단순한 질문을 중심으로 의미론적 구문 분석 연구에 적합한 데이터셋이다. 

---

## GrailQA
