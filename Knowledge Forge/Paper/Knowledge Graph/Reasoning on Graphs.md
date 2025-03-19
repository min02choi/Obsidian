---
aliases:
  - RoG
---
**논문 제목:** *Reasoning on Graphs: Faithful and Interpretable Large Language Model Reasoning*
주요 아이디어: 
***
# Abstract

LLM의 단점
* lack up-to-date knowledge
* hallucinations

현재 KG-based LLM reasoning 동향
* only treat KGs as factual knowledge bases
* overlook the importance of their structural information

Goal & What we do => Reasoning on Graphs(ROG)
* synergizes LLMs with KGs to enable faithful and interpretable reasoning
* planning-retrieval-reasoning framework (계획-검색-추론 프레임워크)
* KG를 활용하여 모델이 신뢰할 수 있는(faithful) 추론을 가능하게 함
	* 기존 방법: KG를 단순 DB로 사용
	* 제안한 방법: KG의 구조적 정보를 사용
* 임의 LLM과 접목시켜도 높은 성능

***
# 1. Introduction

기존 두 가지 접근법의 문제점: KG의 구조적 정보를 사용하지 않고, fact 자체만 사용
1) semantic parsing methods
2) retrieval-augmented methods

✨**planning-retrieval-reasoning framework 을 제안**
* Planning Module: 지식 그래프(KG)에 기반한 관계 경로(relation paths)를 생성하여 신뢰할 수 있는(faithful) 계획을 수립
* Retrieval-Reasoning Module: KG로부터 유효한 추론 경로(valid reasoning paths)를 검색하고, 이를 활용하여 신뢰할 수 있는 추론(faithful reasoning)을 수행

RoG의 optimizing method
1) planning optimization: 지식 그래프(KGs)로부터 지식을 추출(distill)하여 LLM이 신뢰할 수 있는 관계 경로(faithful relation paths)를 계획으로 생성할 수 있도록 함
2) retrieval-reasoning optimization: 검색된 관계 경로를 기반으로 정확한 추론을 수행하고, 그 결과를 해석 가능한 방식으로 생성

RoG는 KG resoning tasks에 state-of-the art를 달성함

***

# 2. Related Work

**LLM Reasoning Prompt**
- **Wei et al., 2022; Wang et al., 2022; Yao et al., 2023; Besta et al., 2023**:  프롬프트를 통해 LLM의 추론 능력을 활용하는 방법을 제시
- **[[Plan-and-Solve Prompting]](Wang et al., 2023c)**: LLM에게 계획을 생성하고 그 계획을 바탕으로 추론을 수행하도록 유도
- **DecomP (He et al., 2021)**: 이 방식은 LLM에게 추론 작업을 여러 개의 하위 작업(sub-task)으로 분해하여 단계적으로 해결하도록 유도
- **ReACT (Yao et al., 2022)**: LLM을 에이전트(agent)로 취급하여, 환경과 상호작용하여 최신 지식을 얻고 이를 바탕으로 추론을 수행하도록 함
- **FAME (Hong et al., 2023)**: 몬테카를로 계획(Monte-Carlo planning)을 도입하여 신뢰할 수 있는 추론 단계를 생성
- **RR (He et al., 2022) KD-CoT (Wang et al., 2023b)**: 지식 그래프(KGs)에서 관련 지식을 검색하여 LLM이 신뢰할 수 있는 추론 계획을 세울 수 있도록 함

*한계: **hallucinations과 lack of knowledge 문제**는 LLM의 추론의 신뢰성(faithfulness)에 영향을 미침*


**Knowledge Graph Question Answering (KGQA)**
* embedding-based methods
* LLM + KGQA
	* UniKGQA: [[Sementic Parsing Methods]] 가 Question을 structural query로 변환함(이 경우 query engine으로 answer이 가능)
	* DECAF: semantic parsing + LLMs

*한계: 이러한 방법들은 **생성된 쿼리의 질**에 따라서 달라짐*
* 쿼리가 executable하지 않다면, 정답이 도출되지 않음

***

# 3. Preliminary

* Knowledge Graphs (KGs): [[Triple]] 구조를 기반으로 함
* Relation Paths: relation 의 시퀀스
* Reasoning Paths: instance of relation path
* Knowledge GraphQuestionAnswering (KGQA)

> **Example**
> * relation path(z): $z$ = marry_to $\rightarrow$ father_of
> * reasoning path instance(w): $w_z$ = Alice $\overset{\text{marry-to}}{\rightarrow}$ Bob $\overset{\text{father-of}}{\rightarrow}$ Charlie

entities($e_q$) 와 answer($a$) 는 지식그래프($G$) 에 linked, labeled 되어 있다고 가정

***

# 4. Approach

## 4.1 Reasoning on Graphs: Planning-Retrieval-Reasoning

#### Planning 기법을 사용한 기존 모델의 한계

* 기존 LLM 기반 추론 방식은 **프롬프트를 이용해 LLM이 직접 추론 계획(Reasoning Plan)을 생성**한 후 이를 기반으로 답을 도출함.
* 이전에도 Planning 기법을 사용하여 question을 해결하려는 시도가 있었지만, **hallucination** 문제 발생. hallucination으로 인해 잘못된 plan이 생성되어 잘못된 방향으로 lead 함
* [[Plan-and-Solve Prompting]] 논문은 planning 기법을 사용

#### **RoG의 접근 방식**

1. **Relation Path(관계 경로)를 계획(Planning) 단계에서 생성**
	* LLM이 직접 추론 계획을 세우는 대신, **지식 그래프(KG)의 구조를 활용해 "관계 경로(relation path)"를 생성**함.
	- 예시) "Alice의 자녀는 누구인가?"
	    - 관계 경로: `marry to → father of`
	    - 실행 과정: `Alice marry to → Bob father of → Charlie` → 답: "Charlie"

2. **Retrieval (검색) 단계에서 KG에서 관련된 추론 경로를 가져옴**
	- 지식 그래프의 최신 정보를 검색하여 추론 계획을 실행.

3. Reasoning (추론) 단계에서 LLM이 답을 생성

#### **최적화 목표**

RoG는 다음 확률을 최적화하는 문제로 정의됨:

![[rog1.png]]
- $P_θ(z∣q)$: LLM이 KG 기반으로 올바른 관계 경로를 생성할 확률 (Planning 단계)
- $P_θ(a∣q,z,G)$: 주어진 KG에서 추론하여 답을 생성할 확률 (Reasoning 단계)

위의 식을 기반으로 하여, 다음의 최종 식을 도출 할 수 있음 *(과정 생략)*

![[rog2.png]]

## 4.3 Planning Module

#### Planning Module 의 역할
- 질문에 대한 신뢰할 수 있는 **관계 경로(relation path)를 계획(Planning)**

#### 관계 경로를 생성하는 과정
1. LLM에게 특정한 프롬프트(Instruction Template)를 제공
`Please generate a valid relation path that can be helpful for answering the following question: <Question>`

2.  LLM이 관계 경로를 구조화된 형식으로 생성
`z = <PATH> r1 <SEP> r2 <SEP> ... <SEP> rl </PATH>`
	- `<PATH>`: 경로의 시작
	- `<SEP>`: 관계(relation) 사이를 구분하는 특수 토큰
	- `</PATH>`: 경로의 끝

3. 최적화 목표: LLM이 정확한 관계 경로를 예측하도록 학습
![[rog3.png]]
* Chain Rule을 적용하여 관계(relation) 하나씩 예측하는 방식으로 확률을 모델링
* 이전까지 생성된 관계`(r<sub><i></sub>)` 를 기반으로 현재 관계`(r<sub><i></sub>)`를 예측하는 방식

## 4.4 Retrieval-Reasoning Module
### Retrieval


### Reasoning

***

### Question while reading the Paper

#### Question #1
근데 이해가 안되는 건 1단계에서 사용자가 질문을 하잖아. 그러면 LLM이 관계 경로를 생성한다매. 근데 이때 KG를 보면서 하는거야? 만약 맞다면, 2단계와 1단계를 분리하는 이유가 뭐지? 논문에서 //1) given a question, we first prompt LLMs to generate several relation paths that are grounded by KGs as plans.// 이 부분에서, 'that are grounded by KGs as plans.'이 부분이 이해가 안가. KG를 안보고 LLM이 planning을 하는데 어떻게 'grounded by KG' 가 될 수 있지?

#### Answer
직접적으로 KG를 사용하지 않는게 맞음. 하지만 LLM은 사전 학습 과정에서 방대한 지식 그래프 기반의 데이터를 학습했을 가능성이 높다. 따라서 KG의 일반적인 구조나 관계 패턴을 어느 정도 알고 있음.
- 예를 들어, "파리(Paris) → 프랑스(France) → 유럽(Europe)" 같은 일반적인 관계는 LLM의 잠재 지식에 포함되어 있을 수 있음. 이 잠재 지식을 기반으로 초기 경로를 생성하는 것.

**Grounded by KGs**는 **직접적으로 KG 데이터를 보는 것**이 아니라, LLM이 가진 **잠재적 지식**과 **KG의 도메인적 이해**를 활용하여 **계획(Plan)을 세운다**는 의미이다. KG에 있는 실제 데이터를 탐색하는 것은 **2단계에서 이루어지는 작업**이고, 1단계는 KG 기반의 일반적인 관계 경로를 **예측하고 제안**하는 단계라고 볼 수 있다.

즉,
1단계는 **가능성 있는 경로를 제안**하고, 2단계에서 **실제 데이터를 기반으로 검증**함으로써 **잠재적 지식**과 **현실적 지식**을 연결하는 방식이라고 볼 수 있다.