논문 제목: Multi-granularity Temporal Question Answering over Knowledge Graphs
발표: ACL'23
![[Pasted image 20250314105014.png]]

---

## **1. Background (연구 배경)**

- **Temporal Knowledge Graphs (TKG, 시간 정보가 포함된 지식 그래프)**는 **시간에 따라 변화하는 사실을 포함**하는 KG이다.
- **TKGQA (Temporal Knowledge Graph Question Answering)**는 **TKG를 기반으로 자연어 질문에 답을 찾는 문제**이다.
- 기존 연구에서는 **CRONQUESTIONS**과 같은 데이터셋을 활용해 TKGQA를 연구했지만, 현실적인 시간 표현(날짜, 월, 연도 등)이 제대로 반영되지 않는 문제가 있었다.
- 현실에서는 **질문이 다양한 시간 단위(연도, 월, 일)를 포함할 수 있음**에도 불구하고, 기존 데이터셋은 연도 단위로만 구성된 경우가 많았다.

---

## **2. Motivation & Idea (연구 동기 및 아이디어)**

**📌 기존 연구의 한계점**

1. **"Pseudo-temporal" 문제:**
    - 기존 데이터셋의 많은 질문들이 **시간 제약이 없어도 답을 찾을 수 있는 비현실적인 질문**이었다.
    - 예: "What award did Carlo Taverna receive in 1863?" → 이 사람이 받은 상이 한 개뿐이라면, **1863이 없어도 정답을 찾을 수 있음**.
2. **단일 시간 단위만 고려:**
    - 기존 연구는 대부분 **연도(Year) 단위**로만 시간 정보를 처리했음.
    - 하지만, 현실에서는 **월(Month), 일(Day) 단위까지 포함된 질문이 많다.**
    - 예: "Who visited China in December 2015?" → 월(Month) 단위의 질문

**📌 연구의 핵심 아이디어**  
✅ **"Multi-granularity Temporal Question Answering"** 개념을 도입  
✅ **새로운 데이터셋 MULTITQ**를 개발하여 현실적인 시간 표현을 반영  
✅ **다양한 시간 단위(연도, 월, 일)를 모두 포함하는 모델 MultiQA**를 제안

---

## **3. Contribution (주요 기여점)**

📌 이 연구의 주요 기여점은 다음과 같다.

1️⃣ **Multi-granularity TKGQA 개념을 처음으로 정식화함**

- 현실적인 시간 질의응답 문제를 다루기 위해, 연도(Year), 월(Month), 일(Day) 단위의 질문을 고려하는 **Multi-granularity TKGQA 개념을 제안**

2️⃣ **새로운 대규모 데이터셋 MULTITQ 구축**

- **500,000개 이상의 질문**을 포함하며,
- **다양한 시간 단위(연도, 월, 일)와 다양한 관계(22개 주요 관계)로 구성됨.**
- **기존 데이터셋 대비 더 현실적인 시나리오를 반영**

3️⃣ **MultiQA 모델 제안 및 성능 검증**

- MultiQA는 기존 모델보다 **다중 시간 단위를 더 효과적으로 처리할 수 있도록 설계됨**
- 실험을 통해 기존 모델 대비 성능이 향상됨을 증명

---

## **4. Method (방법론)**

### **🔹 MULTITQ 데이터셋 구성**

1. **사용된 지식 그래프:** ICEWS05-15 (국제 위기 조기 경보 시스템, ICEWS)
    
    - 기존 연구(Wikidata 기반)와 달리, **더 다양한 사건 및 시간 정보를 포함**
    - 기존 KG의 문제점(정보 부족, 관계 수 제한)을 보완
2. **질문 생성 방식**
    
    - 22개의 주요 관계를 기반으로 **7,334개의 질문 템플릿 생성**
    - 각 질문을 **연도(Year), 월(Month), 일(Day) 단위로 변형**
    - **"Who visited China in December 2015?" → "Who visited China in 2015?"** 로 변형하여 다양한 시간 단위 반영
3. **질문 유형 분류**
    
    - **Single questions:** 단일 시간 제약을 포함하는 질문 (예: "Who visited China in 2015?")
    - **Multiple questions:** 복수 시간 제약을 포함하는 질문 (예: "Which country first visited China before 2015?")

---

### **🔹 MultiQA 모델 구조**

1. **질문 전처리 (Question Pre-processing)**
    
    - 질문에서 **엔티티(Entity)와 시간(Time) 정보를 추출**
    - RoBERTa를 활용하여 **질문의 의미를 임베딩으로 변환**
2. **다중 시간 단위 처리 (Multi-Granularity Time Aggregation)**
    
    - **날짜(Day) 정보를 기반으로 연도(Year), 월(Month) 단위의 시간 정보를 생성**
    - Transformer 기반 **시간 정보 융합(Time Fusion) 기법 적용**
3. **정답 예측 (Answer Scoring Module)**
    
    - **지식 그래프 임베딩을 활용하여 최적의 정답을 선택**

---

## **5. Result (주요 분석 및 결과 해석)**

**📌 실험 설정**

- MULTITQ 데이터셋을 활용하여 실험 진행
- 비교 대상: 기존 KBQA 모델들 (BERT, DistillBERT, ALBERT, EmbedKGQA, CronKGQA)

**📌 주요 결과**  
✅ MultiQA는 기존 모델 대비 **다중 시간 단위(연도, 월, 일)를 더 잘 처리함**  
✅ CronKGQA 대비 **Hits@1(정확도)에서 1~7% 성능 향상**  
✅ **특히 월(Month), 연도(Year) 단위 질문에서 성능이 크게 향상됨**

|**모델**|Hits@1 (정확도)|Hits@10 (정확도)|
|---|---|---|
|BERT|0.083|0.441|
|EmbedKGQA|0.206|0.459|
|CronKGQA|0.279|0.608|
|**MultiQA**|**0.293** 🏆|**0.635** 🏆|

📌 **즉, MultiQA는 기존 모델보다 다중 시간 단위 질문을 더 잘 처리할 수 있음!** 🚀

---

## **6. Limitation (한계점 및 인사이트)**

📌 **한계점:**

1. **데이터셋이 자동 생성됨 → 실제 질문과 차이가 있을 수 있음**
    
    - MULTITQ의 질문들은 자동 생성되었기 때문에, 실제 사용자들이 입력하는 질문과 차이가 있을 가능성이 있음.
2. **지식 그래프가 특정 도메인(국제 사건)에 집중됨**
    
    - ICEWS05-15 데이터셋은 **국제 관계 및 외교 사건 중심**이므로, 다른 도메인(예: 스포츠, 엔터테인먼트 등)에 일반화하는 데 한계가 있을 수 있음.
    - 향후 다양한 도메인 확장이 필요함.

📌 **향후 연구 방향:**  
✅ **더 다양한 시간 표현을 포함하는 질문 생성 기법 개발**  
✅ **다른 도메인(스포츠, 역사, 영화 등)에 확장 가능하도록 데이터셋 개선**

---

## **📌 결론**

✅ **Multi-granularity Temporal Question Answering(TKGQA)의 새로운 개념을 제안**  
✅ **MULTITQ: 현실적인 시간 표현을 반영한 새로운 대규모 데이터셋 구축**  
✅ **MultiQA: 다중 시간 단위를 효과적으로 처리하는 새로운 모델 개발**  
✅ **실험을 통해 기존 모델보다 성능 향상 확인**

**➡️ 이 연구는 KBQA 모델이 시간 정보를 더 정교하게 처리할 수 있도록 돕는 중요한 연구임! 🚀**