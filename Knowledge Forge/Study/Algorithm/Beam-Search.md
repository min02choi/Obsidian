
## 1. Beam Search란?

* 순차적인 의사결정 문제(예: 자연어 생성, 기계 번역)에서 **가장 가능성이 높은 결과를 찾기 위한 탐색 알고리즘**
* Greedy Search(탐욕적 탐색)보다 더 나은 결과를 제공하면서도, 완전한 탐색(Exhaustive Search)보다는 연산량을 줄이는 방식으로 동작

## 2. 알고리즘 동작 방식

1. 초기 상태에서 가능한 후보를 여러 개 생성하고(Beam Width만큼), 가장 높은 확률을 가진 후보들을 유지한다.
2. 각 단계에서 선택된 후보들에 대해 가능한 다음 상태를 확장한다.
3. Beam Width를 초과하는 경우, 확률이 낮은 후보들을 제거한다.
4. 목표 상태(예: 문장의 끝)에 도달할 때까지 반복한다.

## 3. 주요 개념

- **Beam Width (빔 너비)**: 한 번에 유지하는 후보의 개수. 클수록 더 좋은 결과를 찾을 가능성이 높지만, 연산량이 증가한다.
- **Greedy Search vs. Beam Search**:
    - Greedy Search는 각 단계에서 가장 높은 확률을 가진 하나의 후보만 선택하는 방식이다.
    - Beam Search는 여러 개의 후보를 유지하며 탐색을 수행한다.
- **Diversity vs. Accuracy**: Beam Width를 증가시키면 다양한 후보를 탐색할 수 있지만, 너무 크면 계산 비용이 증가한다.
- Knowledge Graph(KG)와 같은 구조에서는 중복된 답이 발생할 수 있음

## 4. 장점과 단점

### 장점
- Greedy Search보다 더 최적에 가까운 결과를 찾을 가능성이 높음
- 완전 탐색(Brute-force)보다 연산량이 적음

### 단점
- Beam Width가 너무 작으면 최적해를 놓칠 가능성이 있음
- Beam Width가 너무 크면 연산량이 증가하여 속도가 느려질 수 있음
- 특정 문제에서는 Local Optima(지역 최적점)에 갇힐 가능성이 있음

## 5. 응용 분야
- 기계 번역 (Machine Translation)
- 음성 인식 (Speech Recognition)
- 자연어 생성 (Natural Language Generation)
- 챗봇 및 대화 시스템


Beam Search는 특히 **자연어 처리(NLP) 분야에서 필수적인 탐색 기법**으로 사용되며, 모델의 성능과 효율성을 조절하는 중요한 요소로 작용한다.