논문 제목: RNG-KBQA:Generation Augmented Iterative Ranking for Knowledge Base Question Answering

![[Pasted image 20250304102741.png]]

***
## 1. Background (연구 배경)

지식 기반(Knowledge Base, KB)은 대규모의 세계 지식을 저장하는 신뢰할 수 있는 정보 원천이지만, 이를 활용하려면 특정 질의 언어(예: SPARQL)를 알아야 하므로 일반 사용자에게 접근성이 낮다. 이에 따라 KB를 활용한 질문 응답(Knowledge Base Question Answering, KBQA) 시스템이 주목받고 있다. 기존 연구들은 독립적이고 동일한 분포(i.i.d.)를 따르는 데이터셋에서 높은 성능을 보였지만, 새로운 KB 스키마 요소를 포함하는 질문에서는 일반화에 실패하는 경우가 많았다.

---

## 2. Motivation & Idea (기존 연구 한계점 + 연구 동기 + 아이디어)

#### **기존 연구 한계점**
1. **Ranking 기반 접근법**
    - KB에서 생성된 후보 논리 형식(Logical Form)들을 점수화하여 최적의 후보를 선택하는 방식.
    - 일반화 성능은 뛰어나지만 **coverage issue (논리 형식의 누락)** 문제를 가짐.
2. **Generation 기반 접근법**
    - 질문을 직접 논리 형식으로 변환하는 방식(seq-to-seq 모델 활용).
    - 새로운 KB 스키마를 포함하는 질문에 대해 일반화가 어렵고, 논리 형식 생성이 까다로움.

#### **연구 동기**
- 기존 방법들의 한계를 극복하기 위해 **Ranking과 Generation을 결합한 새로운 접근법**을 제안.

#### **아이디어**
- **RnG-KBQA (Rank-and-Generate KBQA)**
    - **Ranking 단계:** Contrastive ranker를 활용하여 후보 논리 형식 중 최적의 논리 형식을 선정.
    - **Generation 단계:** 질문과 상위 랭킹 후보들을 입력으로 사용하여 최종 논리 형식을 생성.
    - Ranking과 Generation을 조합하여 coverage 문제를 해결하고 일반화 성능을 향상.

---

## 3. Contribution (주요 기여점)

1. **Ranking과 Generation의 조합**
    - 기존 ranking 기반 접근법의 coverage 문제를 해결하고, generation 기반 접근법의 일반화 한계를 극복.
2. **BERT 기반 Contrastive Ranker**
    - 기존 seq-to-seq 기반 ranker보다 더 강력한 대조 학습(contrastive learning)을 적용하여 학습.
3. **T5 기반 Generation 모델**
    - Ranker가 선정한 후보들을 바탕으로 부족한 논리 구조를 보완하는 방식.
4. **새로운 State-of-the-Art 성능**
    - GRAILQA 및 WEBQSP 데이터셋에서 기존 방법 대비 **크게 향상된 성능**을 보임.
    - 특히, zero-shot generalization에서 뛰어난 성능을 기록.

---

## 4. Method (방법론)

### 1) Logical Form Ranking
- **BERT 기반 Bi-Encoder Ranker**
    - 질문과 논리 형식을 함께 인코딩하여 점수화.
    - Contrastive loss를 활용하여 올바른 논리 형식과 스푸리어스(spurious) 논리 형식을 구별하도록 학습.
- **부트스트래핑(bootstrapping) 기법 활용**
    - 학습 초반에는 무작위 부정 샘플을 사용하고, 이후 학습이 진행될수록 더 어려운 부정 샘플을 제공하여 학습 성능을 향상.
### 2) Logical Form Generation
- **T5 기반 Transformer 모델**
    - 질문과 top-k ranked 논리 형식을 입력으로 받아 최종 논리 형식을 생성.
    - 불완전한 논리 형식의 누락된 부분을 보완.
- **실행 기반 추론 (Execution-Augmented Inference)**
    - 생성된 논리 형식을 KB에서 실행하여 올바른 응답을 찾음.
    - 실행 불가능한 경우, ranker의 최상위 논리 형식을 사용.
### 3) Entity Disambiguation as Ranking
- **BERT Ranker를 활용한 개체 식별(Entity Linking) 개선**
    - 질문과 개체 후보를 결합하여 가장 적절한 개체를 선택.

---

## 5. Result (주요 분석 및 결과 해석)

- **데이터셋:**
    - **GRAILQA** (일반화 평가 중심)
    - **WEBQSP** (전통적인 i.i.d. 평가)

### 1) GRAILQA 성능
- **기존 SOTA 대비 10.7 EM, 8.2 F1 향상** (최고 성능 기록)
- **Zero-shot generalization에서 16.1 F1 차이로 기존 방법보다 우수한 성능**
- Ranking과 Generation을 조합한 모델이 ranking-only 접근법보다 성능이 11.4 EM, 8.2 F1 높음.
### 2) WEBQSP 성능
- **F1 점수 75.6 기록, 기존 SOTA 대비 1.6 향상**
- 기존 방법들이 KB에서 직접 검색하는 방식과 비교하여 높은 정확도.
### 3) Ablation Study (성능 분석)
- **Ranking-only 모델 대비 Generation 추가 시 5.3 F1 향상 (GRAILQA 기준)**
- **Bootstrapping 없이 학습하면 성능 저하 (GRAILQA -1.2, WEBQSP -1.4 F1)**
- **Zero-shot setting에서 Generation이 상대적으로 덜 효과적이지만 여전히 개선 효과 존재.**

---

## 6. Limitation (논문의 한계 및 인사이트)

### 1) Generation 모델의 오류
- Generation 모델이 논리 형식을 보완하는 역할을 하지만, 가끔 잘못된 제약 조건을 추가할 수 있음.
- 예: 질문이 모호한 경우, 오히려 ranking 결과가 더 정확할 수도 있음.
### 2) Zero-shot Generalization 한계
- Unseen KB schema에 대한 일반화는 향상되었지만, 완벽하지 않음.
- 새로운 스키마에 대한 학습을 더 잘 지원하는 방법 필요.
### 3) Executability Constraints
- 생성된 논리 형식이 실행 가능해야 하지만, 보장할 수 없음.
- 향후 Grammar-based decoding 등을 활용하여 개선 가능.

---

## 7. Conclusion (결론)

- RnG-KBQA는 Ranking과 Generation을 결합한 새로운 KBQA 접근법을 제안.
- 기존 ranking-only 및 generation-only 방법의 한계를 해결.
- GRAILQA 및 WEBQSP에서 새로운 SOTA 성능을 기록.
- 특히 Zero-shot Generalization에서 강력한 성능을 보임.
- 향후 연구로 syntax-aware generation 기법을 추가하여 논리 형식의 정확도를 더욱 향상할 수 있음.

***
## Question about the Paper

> 논문을 읽으면서 궁금한점 & 의문점

#### Question1
generator이 논리 형식을 보완한다는거는 알겠어. 근데 generator 이 생성한 논리형식이 실행되지 않을 수 있다는 말은 무엇인가?
#### Answer
Generator는 Ranker가 제공한 상위 k 개의 논리 형식을 기반으로 **새로운 논리 형식을 생성**하지만,  이 논리 형식이 실제로 KB에서 실행되지 않을 가능성이 있다는 의미임.
즉, Generator가 논리적으로는 맞아 보이지만, 실제 KB에서 실행할 수 없는 (executable하지 않은) 논리 형식을 만들 수도 있음.
* Generator가 논리 형식을 생성할 때, **KB에 존재하지 않는 관계(relation)나 속성(property)을 포함할 가능성이 있음**.
* 실행 가능한 논리 형식이지만 결과가 없는 경우 (No Valid Results)


#### Question2