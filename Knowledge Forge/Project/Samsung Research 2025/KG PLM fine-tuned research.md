**Key Words**
knowledge graph
knowledge graph question answering
knowledge graph reasoning

***
### **리서치 내용**

**Knowledge Graph + PLM fine-Tuning**

1.     해당 논문의 task
2.     연구 동기(기존 연구의 문제점)
3.     방법론
4.     사용한 PLM, PLM의 역할
5.     사용한 데이터셋

***

#### **JAKET: Joint Pre-training of Knowledge Graph and Language Understanding, AAAI’22**

**1)**    **해당 논문의 Task**
지식 그래프(Knowledge Graph, KG)와 언어 이해(Language Understanding)를 통합하는 공동 사전 학습(Joint Pre-training)  
KG를 활용하여 PLM의 지식 표현 능력을 향상시키는 것이 목표

**2)**    **연구 동기(기존 연구의 문제점)**
* KG에는 많은 세계 지식(entity, 관계 정보 등)이 포함되어 있지만, 일반적인 PLM은 이를 제대로 활용하지 못함.
* PLM과 KG의 정보 표현 방식이 달라 통합이 어려움.
* 기존 연구들은 KG를 단순히 외부 지식으로 활용하거나 별도로 학습하는 데 그침

**3)**    **방법론: JACKET 모델**
* 공동 사전 학습(Joint Pre-training) 프레임워크: KG 기반 지식 임베딩(Knowledge Embedding) + PLM 기반 언어 표현(Language Representation) 동시 학습
* 두 모듈을 상호 보완적으로 학습(서로의 출력으로 학습)
* KG를 활용한 엔티티 임베딩(Entity Embedding) 과 문맥 정보 기반 언어 임베딩(Contextual Embedding)을 결합하여 지식과 문맥을 모두 반영하는 모델 개발

* 두 개의 모듈의 상호 강화(Self-supervised Learning) 훈련
	* 지식 모듈(Knowledge Module): 그래프 신경망(Graph Attention Network, GAT)을 활용하여 KG에서 엔티티 및 관계의 의미를 학습하여 임베딩 생성 (GNN)
	* 언어 모듈(Language Module): 문맥을 반영하여 텍스트 내 엔티티의 의미를 조정, RoBERTa-base 모델 사용

**4)**    **사용한 PLM, PLM의 역할**
사용한 PLM 모델: RoBERTa-base

**역할**
* 일반적인 PLM은 텍스트 내 문맥(Context)만 반영하지만, JAKET의 LM은 KG에서 학습된 엔티티 정보까지 포함하여 학습.
* 언어 모델이 단순한 단어 임베딩이 아니라, KG 기반 지식까지 반영된 문맥적 표현을 학습

***

#### **RTA: A reinforcement learning-based temporal knowledge graph question answering model,** **ScienceDirect’25**

**1)**    **해당 논문의 Task**
Temporal Knowledge Graph Question Answering (TKGQA): 시간(Time)이 포함된 지식 그래프(Temporal Knowledge Graph, TKG)를 활용하여 QA 수행

**2)**    **연구 동기**
시간 개념을 포함한 질의응답(TKGQA)은 기존 KGQA보다 더 복잡함

다중 엔티티가 포함된 질문을 다루는 것이 어려움

질문 내 다중 엔티티 탐색(Topic Entity Selection)을 하고, 강화 학습을 활용하여 최적의 경로 탐색(Optimal Path Reasoning)을 하고자 함

**3)**    **방법론: RTA 모델**

1-    질문 이해 단계(Question Understanding Stage): 문맥(Context) 분석 후 Topic Entity 선택
2-    강화 학습 기반 경로 탐색(Policy Learning): 최적의 추론 경로 탐색(후보 정답 별 가중치 계산)

**4)**    **사용한 PLM 및 역할**

사용한 PLM:

·         BERT 및 Transformer 기반 사전 학습된 언어 모델(PLM) 활용

PLM의 역할:

·         질문에서 핵심 엔티티(Topic Entities) 및 관계(Relations) 추출

·         텍스트 기반 시간 정보(Timestamp)를 임베딩하여 Temporal Reasoning 지원