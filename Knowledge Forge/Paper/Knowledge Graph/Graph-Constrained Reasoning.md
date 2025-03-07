---
aliases:
  - GCR
---
 논문 제목: Graph-constrained Reasoning: Faithful Reasoning on Knowledge Graphs with Large Language Models

* 훈련 결과: [[GCR|GCR Training Log]]
***
### **1. Background (연구 배경)**

대형 언어 모델(LLM)은 강력한 추론 능력 을 갖추었지만, 여전히 지식 부족과 환각(hallucination) 문제로 인해 신뢰할 수 없는 추론을 수행하는 경우가 많다. 이를 해결하기 위해 지식 그래프(KG)가 활용되고 있지만, 기존의 KG 기반 방법들은 정확한 지식 검색과 효율적인 그래프 탐색에 어려움을 겪고 있다.

---

### **2. Motivation & Idea (기존 연구 한계점 + 연구 동기 + 아이디어)**

![[paper_gcr1.png]]
- 기존의 KG 기반 추론 방법은 **(1) 검색 기반(retrieval-based)** 및 **(2) 에이전트 기반(agent-based)** 방식으로 나뉜다.
    - **검색 기반 방법**은 외부 검색기를 통해 KG에서 관련 지식을 검색하지만, 일반화 능력이 부족하고 그래프 구조를 온전히 반영하지 못한다.
    - **에이전트 기반 방법**은 LLM이 KG를 순차적으로 탐색하는 방식이지만, 계산 비용이 높고 응답 시간이 길다.
- 기존 연구 [[Reasoning on Graphs|RoG]]의 문제점:
	- 이 모델도 KG 기반 reasoning method를 제시하는 방법을 사용하지만, 33%의 hallucination error 가 존재함.
- 이러한 문제를 해결하기 위해 **"Graph-Constrained Reasoning (GCR)"**을 제안한다.
    - KG의 구조를 **LLM 디코딩 과정에 직접 반영**하여 신뢰할 수 있는 추론을 가능하게 한다.
    - **KG-Trie**라는 **접두사 트리(prefix tree) 기반의 인덱스 구조**를 사용하여 KG 탐색을 효과적으로 제어한다.
    - 경량 KG 특화 LLM과 강력한 일반 LLM을 조합하여 **그래프 기반 추론과 유도적(inductive) 추론을 결합**한다.

---

### **3. Contribution (주요 기여점)**

1. **Graph-Constrained Reasoning (GCR) 프레임워크 제안**
    - KG의 구조를 LLM 디코딩에 직접 통합하여 **신뢰할 수 있는 KG 기반 추론**을 수행할 수 있도록 한다.
2. **경량 KG 특화 LLM과 강력한 일반 LLM의 결합**
    - KG에 특화된 LLM을 사용하여 그래프 기반 추론을 수행하고, 일반 LLM을 사용하여 다중 추론 경로를 통합하는 방식 제안.
3. **최신 KGQA 벤치마크에서 SOTA(State-of-the-Art) 성능 달성**
    - 환각 없는 신뢰할 수 있는 추론을 달성하면서도 **새로운 KG에 대해 제로샷 일반화(zero-shot generalization) 가능**.

---

### **4. Method (방법론: Motivation과 Contribution과 연계하여 구체적으로 설명)**

![[paper_gcr2.png]]
1. **Knowledge Graph Trie (KG-Trie) 구축**
    - KG 내의 유효한 추론 경로를 문자열로 변환하여 Trie 구조로 저장.
    - 이를 통해 LLM 디코딩을 KG에 기반하여 제한함으로써 **유효한 경로만 탐색**할 수 있도록 함.
2. **Graph-Constrained Decoding**
    - KG-Trie를 활용하여 LLM이 생성하는 추론 경로를 **KG 내에서 유효한 경로로 제한**.
    - 이를 통해 hallucinations을 제거하고 신뢰할 수 있는 KG-Grounded Path, hypothesis 생성.
    - KG 특화 LLM이 여러 개의 추론 경로와 가설 정답을 생성.
3. **Graph Inductive Reasoning**
    - Graph-Constrained Decoding과정에서 생성한 경로와 가설 정답을 바탕을 일반 LLM의 input으로 제공
    - 최종적으로 general LLM이 이러한 경로를 바탕으로 최종 정답을 유도

---

### **5. Result (주요 분석 및 결과 해석)**

4. **SOTA 성능 달성**
    - WebQSP 및 CWQ 데이터셋에서 **최고 성능 달성 (Hit@1: 92.6% 및 75.8%)**
    - 기존의 RoG, GNN-RAG 등 최신 방법을 초월하는 성능.
5. **환각 제거 및 신뢰할 수 있는 추론 달성**
    - KG-Trie를 적용하지 않은 경우 **환각 발생률이 37.6% (WebQSP), 51.9% (CWQ)**였지만,
    - GCR 적용 시 **100% 신뢰할 수 있는 추론**을 수행.
6. **제로샷 일반화 성능**
    - ConceptNet 및 MedQA의 새로운 KG에서도 별도 학습 없이 성능 향상.
    - 기존 모델 대비 **7.6% (CSQA), 3.1% (MedQA) 정확도 개선**.

---

### **6. Limitation (의문점 및 이 논문의 한계점/인사이트)**

- **KG-Trie 구축 시 초기 오버헤드 발생**
    - 사전 구축이 필요하므로, 대규모 KG에서 적용할 경우 성능 저하 가능성 존재.
- **일반 LLM의 성능에 의존**
    - 강력한 일반 LLM(GPT-4o-mini, ChatGPT 등)을 필요로 하므로, 실행 비용이 높을 수 있음.
- **KG가 없는 경우 적용 어려움**
    - KG에 기반한 방법이므로, 충분한 구조화된 지식이 없는 경우 적용이 제한적일 수 있음.

***
## Insight fron the Paper

> 얻은 영감이나 아이디어들

* hops 가 데이터 탐색에 미치는 영향이나, 데이터의 complexability 에 따른 적절한 hops 수에 대한 연구

*** 
## Question about the Paper

> 논문을 읽으면서 궁금한점 & 의문점

#### Question1
GCR에서 KG를 Trie로 변환하면, Trie는 자료구조인데 단순히 알고리즘으로 경로를 찾을 수 있는 거 아닌가? 그런데 왜 굳이 KG-specialized LLM을 사용해서 경로를 찾는 것인가?
#### Answer
Trie는 단순히 **"유효한 경로(valid path)"를 저장하는 자료구조**이다. 하지만, **"최적의 경로를 선택하는 것"은 단순한 탐색 알고리즘만으로는 어렵기 때문에 LLM을 활용하는 것임.
Trie가 유효한 경로를 저장하는 자료구조**라면, 당연히 BFS(너비 우선 탐색)나 DFS(깊이 우선 탐색) 같은 알고리즘으로 경로를 찾을 수 있어야 한다.  
그런데 GCR에서는 **단순한 탐색이 아니라, 더 복잡한 reasoning이 필요**하다.
* 단순한 그래프 탐색이 아니라, 의미적 추론이 필요함
	* 예를 들어,
	- `"Who is the spouse of the ex-president of USA?"`
	- Trie에는 `"Barack Obama → spouse → Michelle Obama"` / `"Donald Trump → spouse → Melania Trump"` 같은 많은 경로가 있을 수 있음.
	- **어떤 경로를 선택해야 할지는 단순 탐색 알고리즘으로는 결정할 수 없음.**
	- KG-specialized LLM이 질문을 이해하고, 문맥에 맞는 경로를 선택하는 역할을 함.
- KG-Trie는 경로 제약을 거는 역할이지, reasoning을 대신하는 것이 아님
- 알고리즘만으로는 어떤 경로가 더 '적절'한가를 따질 수 없음

#### Question2
hops=2, k=10으로 선정한 이유가 무엇인지? WebQSP는 쉬운 질문들이라 그렇다 쳐도 CWQ같은 complex한 질문들은 hop의 수에 따라 정답이 달라질 수 있을 것 같음.
#### Answer



#### Question3
음 근데 Appendix에서 WebQSP 데이터셋의 max hop이 2인데 4로 실험한거는 뭐지?
![[Pasted image 20250228165230.png]]
Multi-hop Question Answering over Knowledge Graphs using Large Language Models, *arXiv 'Feb2024*