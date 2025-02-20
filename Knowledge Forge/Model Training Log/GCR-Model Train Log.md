훈련일자:
사용한 GPU 스탯

이거 빔서치 정리하기, jsonl 파일 파악

# 📝 딥러닝 모델 훈련 로그

## 📅 날짜
-2025-02-19

## 🏷️ 실험 정보
- **모델 이름:**  
- **데이터셋:**  
- **실험 목적:**  
- **사용한 라이브러리 & 프레임워크:**  
  - TensorFlow/PyTorch/XGBoost 등
- **코드 경로:** `[링크 또는 파일명]`

## ⚙️ 하이퍼파라미터 설정
- **배치 크기 (Batch Size):**  
- **학습률 (Learning Rate):**  
- **에폭 (Epochs):**  
- **최적화 알고리즘 (Optimizer):**  
- **손실 함수 (Loss Function):**  
- **기타 설정:**  

## 🚀 학습 결과
- **최종 Loss:**  
- **최종 Accuracy:**  
- **기타 지표 (F1-score, AUC 등):**  

## 📊 주요 로그
```bash
# (필요한 경우, 로그 일부 추가)

```


***

CoG 모델 훈련 진행 과정

GPU 스탯
* 서버 위치: 데이터센터
* GPU 개수: 4
* 파티션: big_suma_rtx3090

훈련 관련 기본정보
* pre-trained model 사용: Llama-3.1-8B
* used dataset: rmanluo/RoG-webqsp의 test dataset(WebQTest-n)
	* 1628 row


## STEP1: Graph-constrained decoding

목표: several KG-grounded reasoning paths and hypotheses answers 를 생성하는 과정
* `predictions.jsonl`: 모델이 예측한 모든 경로 제시
```
{"id": "WebQTest-0", "question": "what does jamaican people speak",

"prediction":[
"# Reasoning Path:\nJamaica -> location.country.languages_spoken -> Jamaican Creole English Language\n# Answer:\nJamaican Creole English Language",
"# Reasoning Path:\nJamaica -> location.country.languages_spoken -> Jamaican English\n# Answer:\nJamaican English",
"# Reasoning Path:\nJamaica -> location.country.languages_spoken -> Jamaican Creole English Language\n# Answer:\nJamaican Creole English Language",
"# Reasoning Path:\nJamaica -> location.country.languages_spoken -> Jamaican English\n# Answer:\nJamaican English",
"# Reasoning Path:\nJamaica -> location.country.languages_spoken -> Jamaican Creole English Language\n# Answer:\nJamaican Creole English Language",
"# Reasoning Path:\nJamaica -> location.country.currency_used -> Jamaican dollar -> finance.currency.countries_used -> Jamaica\n# Answer:\nJamaica",
"# Reasoning Path:\nJamaica -> location.country.languages_spoken -> Jamaican English\n# Answer:\nJamaican English",
"# Reasoning Path:\nJamaica -> location.country.languages_spoken -> Jamaican Creole English Language\n# Answer:\nJamaican Creole English Language",
"# Reasoning Path:\nJamaica -> location.country.languages_spoken -> Jamaican English\n# Answer:\nJamaican English",
"# Reasoning Path:\nJamaica -> location.country.languages_spoken -> Jamaican Creole English Language\n# Answer:\nJamaican Creole English Language"],

"ground_truth": ["Jamaican English", "Jamaican Creole English Language"], "ground_truth_paths": ["Jamaica -> location.country.languages_spoken -> Jamaican English", "Jamaica -> location.country.languages_spoken -> Jamaican Creole English Language"], "input": "<|begin_of_text|><|start_header_id|>system<|end_header_id|>\n\nCutting Knowledge Date: December 2023\nToday Date: 26 Jul 2024\n\n<|eot_id|><|start_header_id|>user<|end_header_id|>\n\nReasoning path is a sequence of triples in the KG that connects the topic entities in the question to answer entities. Given a question, please generate some reasoning paths in the KG starting from tche topic entities to answer the question.\n\n# Question: \nwhat does jamaican people speak?\n# Topic entities: \nJamaica<|eot_id|><|start_header_id|>assistant<|end_header_id|>\n\n"}

```
* `detailed_eval_result.jsonl`: 예측 정확도나 추론 경로의 정확성 등 성능 평가 결과를 포함한 파일
```
{"id": "WebQTest-0",
"prediction": [
"# Reasoning Path:\nJamaica -> location.country.languages_spoken -> Jamaican Creole English Language\n# Answer:\nJamaican Creole English Language",
"# Reasoning Path:\nJamaica -> location.country.languages_spoken -> Jamaican English\n# Answer:\nJamaican English",
"# Reasoning Path:\nJamaica -> location.country.currency_used -> Jamaican dollar -> finance.currency.countries_used -> Jamaica\n# Answer:\nJamaica"],

"ground_truth": ["Jamaican Creole English Language", "Jamaican English"],
"ans_acc": 1.0, "ans_hit": 1, "ans_f1": 0.8, "ans_precission": 0.6666666666666666, "ans_recall": 1.0, "path_f1": 0.8, "path_precision": 0.6666666666666666, "path_recall": 1.0, "path_ans_f1": 0.8, "path_ans_precision": 0.6666666666666666, "path_ans_recall": 1.0}
```

테스트 요약
성능 관련 지표
* Accuracy: 78.50835262438086
* Hit: 92.19422249539029
* F1: 58.08451783226196
* Precision: 56.45388825474874
* Recall: 77.88374665748543
* Path F1: 54.956581743950935
* Path Precision: 55.396849786831346
* Path Recall: 72.49821259823082
* Path Answer F1: 59.84054831682146
* Path Answer Precision: 58.80381654813123
* Path Answer Recall: 78.50798893908504

총 1628개의 데이터에 대해서 진행
* 전체 소요 시간: 3:17:53
* 소요 시간: 개당 평균 7~8초, `WebQTest-1395`는 18.91 소요(최장시간)
* `WebQTest-521`: None result
```
{"id": "WebQTest-521", "question": "who was anakin skywalker", "answer": ["Ted Bracewell"], "q_entity": ["g.125_cxx77"], "a_entity": ["Ted Bracewell"], "graph": [["Tatooine", "fictional_universe.fictional_setting.characters_that_have_lived_here", "g.125_cxx77"], ["m.011qx8t1", "film.performance.character", "g.125_cxx77"]], "choices": []}
```

## STEP2: Graph Inductive reasoning

목표: LLM을 통해 step1에서 생성한 reasoning paths에서 최종 답안을 도출하는 과정

**1차시도(25.02.20) (GCR/graph-constrained-reasoning/step2_result_GCR_timeout.txt)**
* 뭔가 키가 안되어있나? 나 분명 추가한거같은데 openapi 불러오는 코드 부분을 좀 더 봐야 할듯
* 서버 시간 5시간으로 했는데 59퍼센트 함. 한 10시간은 잡아야할듯...????? 그럴거면 지금 돌려야하고
* 파일의 log가  왜 섞여서 나오냐? -- 이거 분리하고싶은데

* 아니 근데 log 보는데 Answer에 그냥 답으로 마지막 노드의 값을 도출하는거같은데 뭐지

문제점(terminal  결과)
* 토큰 사용 관련
* 에러가  안 뜨는 경우
```
Question:
what does jamaican people speak?

Based on the reasoning paths, please answer the given question. Please keep the answer as simple as possible and only return answers. Please return each answer in a new line.
Message:  Reasoning Paths:
# Reasoning Path:
John F. Kennedy -> people.person.parents -> Joseph P. Kennedy, Sr. -> people.person.children -> Robert F. Kennedy
# Answer:
Robert F. Kennedy
# Reasoning Path:
John F. Kennedy -> base.kwebbase.kwtopic.connections_from -> john fitzgerald kennedy allegedly assassinated by lee harvey oswald -> base.kwebbase.kwconnection.other -> Lee Harvey Oswald
# Answer:
Lee Harvey Oswald
# Reasoning Path:
John F. Kennedy -> film.film_subject.films -> Thirteen Days -> film.film.subjects -> Robert F. Kennedy
# Answer:
Robert F. Kennedy
# Reasoning Path:
John F. Kennedy -> government.us_president.vice_president -> Lyndon B. Johnson
# Answer:
Lyndon B. Johnson
```

* 에러 뜨는 경우
	* Number of Token 포함 파일: `chatgpt.py`
```
Question:
what did james k polk do before he was president?

Based on the reasoning paths, please answer the given question. Please keep the answer as simple as possible and only return answers. Please return each answer in a new line.
Number of token:  284
Error code: 401 - {'error': {'message': 'Incorrect API key provided: sk-xx. You can find your API key at https://platform.openai.com/account/api-keys.', 'type': 'invalid_request_error', 'param': None, 'code': 'invalid_api_key'}}
Message:  Reasoning Paths:
# Reasoning Path:
Fukushima Daiichi Nuclear Power Plant -> location.location.containedby -> Japan
# Answer:
Japan
# Reasoning Path:
Fukushima Daiichi Nuclear Power Plant -> location.location.street_address -> m.0ggj3z2 -> location.mailing_address.citytown -> Fukushima
# Answer:
Fukushima
# Reasoning Path:
Fukushima Daiichi Nuclear Power Plant -> location.location.containedby -> Japan -> location.country.administrative_divisions -> Fukui Prefecture
# Answer:
Fukui Prefecture
# Reasoning Path:
Fukushima Daiichi Nuclear Power Plant -> base.schemastaging.context_name.pronunciation -> g.125_p8yvl
# Answer:
g.125_p8yvl
# Reasoning Path:
Fukushima Daiichi Nuclear Power Plant -> common.topic.image -> The Fukushima 1 NPP -> common.image.appears_in_topic_gallery -> Fukushima Daiichi Nuclear Power Plant
# Answer:
Fukushima Daiichi Nuclear Power Plant
# Reasoning Path:
Fukushima Daiichi Nuclear Power Plant -> location.location.containedby -> Okuma
# Answer:
Okuma
```

(해결 시도) key  받는 부분 수정중...
```python
    ## 키 값 받는 과정 좀 수정함
    def prepare_for_inference(self, model_kwargs={}):
        api_key = os.getenv('OPENAI_API_KEY')
        client = OpenAI(
	        # api_key=os.environ['OPENAI_API_KEY'],  # this is also the default, it can be omitted
	        api_key=api_key     # 직접적으로 넣어줌
        )

        self.client = client
```

ctrl + J: terminal up and down

오 이거 되나?

GCR step1, 2 돌린 프롬포트
sbatch  --time=7:00:00 -q big_qos -p big_suma_rtx30900 ./run.sh

Step2 정리하면 될듯(Notion, Opsidian)
GPT 사용량 변화
* 0.36 -> 0.60 (양도 정리할것. 근데 왜 토큰은 안뜸..?)


***

STEP2: Graph Inductive reasoning(2차 시도)
목적: LLM을 통해 step1에서 생성한 reasoning paths에서 최종 답안을 도출하는 과정

`gpt-3.5-turbo` 사용하여 추출된 reasoning path에서 최종 답안 도출

성능 지표
* Accuracy: 74.19227965234637
* Hit: 89.68058968058968
* F1: 70.34046649011567
* Precision: 76.8986193986194
* Recall: 74.19227965234637

테스트 결과
* 총 데이터 수: 1628개(WebQTest)
* 전체 소요 시간: 질문당 약 0.085s
* 개당 평균 소요 시간

> results 안의 results history안에 
> * 3090 genpath, kgqa값을 넣음


4090, gpu 2개

sbatch  --time=7:00:00 -q big_qos -p big_suma_rtx4090 ./run2.sh