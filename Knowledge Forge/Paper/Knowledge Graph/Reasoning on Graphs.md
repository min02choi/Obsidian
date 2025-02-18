
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

Planning 기법을 사용한 기존 모델의 한계
* 이전에도 Planning 기법을 사용하여 question을 해결하려는 시도가 있었지만, **hallucination** 문제 발생. hallucination으로 인해 잘못된 plan이 생성되어 잘못된 방향으로 lead 함
* [[Plan-and-Solve Prompting]] 논문은 planning 기법을 사용

![[Pasted image 20250218112424.png]]
