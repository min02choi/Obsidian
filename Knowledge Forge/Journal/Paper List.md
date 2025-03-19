# NLP

| **Title**                                                                            | **Year** | **Published** | **Read Date** | **Summary** | **Keywords** |
| ------------------------------------------------------------------------------------ | -------- | ------------- | ------------- | ----------- | ------------ |
| [[Attention is All You Need]]                                                        | 2017     | Arxiv         |               |             |              |
| [[BERT]]: Pre-training of Deep Bidirectional Transformers for Language Undurstanding | 2019     | ACL           |               |             |              |
|                                                                                      |          |               |               |             |              |
|                                                                                      |          |               |               |             |              |

# Knowledge Graphs

| **Title**                                                                                                                          | **Conf'yy** | **Read Date** | **Summary**                                                                                         | **Keywords** |
| ---------------------------------------------------------------------------------------------------------------------------------- | ----------- | ------------- | --------------------------------------------------------------------------------------------------- | ------------ |
| [[Plan-and-Solve Prompting]]: Improving Zero-Shot Chain-of-Thought Reasoning by Large Language Models                              | ACL'23      | 250217        | 즉석적인 CoT 방식을 보안함. LLM이 추론하기 전, 미리 계획을 세워 이를 기반으로 문제 해결                                              |              |
| [[Reasoning on Graphs]]: Faithful and Interpretable Large Language Model Reasoning<br>                                             | ICLR'24     |               | planning-retrieval-reasoning framework 제안. LLM으로 질문에 대한 추론 경로 생성-> KG기반 실제 유효한 경로 탐색-> 최종 답변과 설명 제공 |              |
| [[Graph-Constrained Reasoning]]: Faithful Reasoning on Knowledge Graphs with Large Language Models                                 | ArXiv'24    | 250226        | RoG의 후속작. KG-Trie와 KG specialized LLM을 사용하여 reasoning path 도출, 이를 기반으로 General LLM로 최종답 도출          |              |
| [[Reasoning with Trees]]: Faithful Question Answering over Knowledge Graph                                                         |             |               |                                                                                                     |              |
| [[Rng-KBQA]]: Generation Augmented Iterative Ranking for Knowledge Base Question Answering                                         |             |               | 전통 KBQA 방식인 ranking, generaion 방식의 결합                                                               |              |
| [[Query Instruction Parsing Plugin\|Effective Instruction Parsing Plugin for Complex Logical Query Answering on Knowledge Graphs]] |             | 250307        |                                                                                                     | FOL query    |
| [[Distribution Shifts Are Bottlenecks]]: Extensive Evaluation for  Grounding Language Models to Knowledge Bases                    |             |               | 현재 벤치마크의 한계점 제시(분포 문제), KBQA모델의 robortness를 4가지 측면에서 평가, 새로운 평가 프로토콜 및 데이터 증강기법 제안                  |              |
| [[Multi-granularity Temporal Question Answering over Knowledge Graphs]]                                                            | ACL’23      |               |                                                                                                     |              |

### Knowledge Graph + PLM fine-tuning(Samsung Research Project)

* PLM을 fine-tuning하여 KG에 사용한 사례 조사
* **Key Words**
	* Knowledge Graph
	* Knowledge Graph Question Answering
	* Knowledge Graph Reasoning

| **Title**                                                                                   | **Conf'yy**      | Task    | **Summary(Method)**                                                                                                     |
| ------------------------------------------------------------------------------------------- | ---------------- | ------- | ----------------------------------------------------------------------------------------------------------------------- |
| JAKET: Joint Pre-training of Knowledge Graph and Language Understanding                     | AAAI’22          |         | JACKET: Knowledge Module + Language Module Joint(상호 강화) 훈련                                                              |
| RTA: A reinforcement learning-based temporal knowledge graph question answering model       | ScienceDirect’25 | TKGQA   | RTA: 질문 이해 단계(Topic entity 선택)->강화학습 기반 optimal 경로 탐색(가중치 계산)                                                           |
| QUERY2BERT: Combining Knowledge Graph and Language Model for Reasoning on Logical Queries   | IEEE’25          |         | QUERY2BERT: KG Embedding(Node2Vec-엔티티 임베딩 + BERT-엔티티의 text description 임베딩) -> K-D Tree Indexing(NN Search) -> 논리 연산 수행 |
| Incorporating Structural Knowledge into Language Models for Open Knowledge Graph Completion | WWW'25           | OpenKGC | struOKGC: Unified Prompt(Semantic Prompt + Structure Prompt)생성. 이를 PLM에 입력                                              |
| Multi-turn Response Selection with Commonsense-enhanced Language Models                     | arXiv’24         |         | SinGL:PLM과 KG(GNN)의 결합(외부 상식을 통해 문맥을 보강하고자 함). 각각의 embedding 간 similarity를 최대화 하는 방식                                    |
|                                                                                             |                  |         |                                                                                                                         |
|                                                                                             |                  |         |                                                                                                                         |




***

### **논문 예약!**
- [ ] Multi-hop Question Answering over Knowledge Graphs using Large Language Models, *arXiv 'Feb2024*
	* hop수에 따른 다양한 실험 결과를 제시하려나?
- [ ] Diffusion Language Model?
- [ ] Simple Is Effective: The Roles of Graphs and Large Language Models in Knowledge-Graph-Based Retrieval-Augmented Generation, ICLR'25
- [ ] LG 논문
- [ ] Two-layer knowledge graph transformer network-based question and answer explainable recommendation

**KGQA Basic 논문**
- [ ] RNG-KBQA: Generation Augmented Iterative Ranking for Knowledge Base Question Answering
- [ ] Multi-granularity Temporal Question Answering over Knowledge Graphs
- [ ] Distribution Shifts Are Bottlenecks: Extensive Evaluation for Grounding Language Models to Knowledge Bases
- [ ] FC-KBQA: A Fine-to-Coarse Composition Framework for Knowledge Base Question Answering, ACL’23
* KGQA 논문 읽기 순서(고민되네)
	![[Pasted image 20250303174130.png]]

그래프 엣지 제거하고 싶으면 이런식으로도 가능~
[보이고 싶은 링크](보이고 싶은 링크)
[숨기고 싶은 링크](숨기고 싶은 링크)

ACC, 시간