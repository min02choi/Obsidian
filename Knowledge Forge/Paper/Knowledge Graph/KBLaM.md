

*** 
## Question about the Paper

> 논문을 읽으면서 궁금한점 & 의문점

#### Question1
일단 KBLaM이 RAG 방식이 아니라는거는 알겠음. 근데 어찌되었든간에 모델의 update 측면에서 보자면 KG의 '새로운 지식'의 출현을 알기 위해서는 검색이든 그런 방식으로 알아야 하는게 아닌가?

#### Answer
새로운 지식을 얻는 과정은 어떤 방식이든 필수이고,  
그 지식을 어떻게 LLM에 반영하느냐가 RAG, KBLaM, fine-tuning 방식의 차이다.
- RAG는: **업데이트된 지식을 검색해서 바로 활용**하는 구조
- KBLaM은: **업데이트된 지식을 key-value로 바꿔서 memory에 넣는 구조**  
이렇게 활용 방식이 다른 거지, **업데이트 "탐지" 자체는 둘 다 외부 프로세스가 필요해.**

#### Question2
plug-and-play 방식이란?

#### Answer
지식을 따로 학습시키지 않고, 외부 memory처럼 꽂아 바로 쓰는 방식
- LLM은 원래 자기 내부 토큰 간 attention만 함
- 근데 KBLaM은 **외부 memory도 함께 attention 대상으로 추가**함.
- 즉, LLM의 `Q` (query 벡터)는 **context + knowledge** 양쪽을 동시에 바라봄.
- 학습 없이, 지식만 추가하면 됨 → plug-and-play.