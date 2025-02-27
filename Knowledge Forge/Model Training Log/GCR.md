---
aliases:
  - GCR Training Log
---

*논문 제목: Graph-constrained Reasoning: Faithful Reasoning on Knowledge Graphs with Large Language Models
***

**결과 파일 경로**
*/home/bys3158/model_test/GCR/graph-constrained-reasoning/resources*
![[Pasted image 20250227171316.png]]

**훈련 로그 경로**
*/home/bys3158/model_test/GCR/graph-constrained-reasoning/z_history_log*
![[Pasted image 20250227171450.png]]
***
### 목적
* 논문은 대형 언어 모델(LLM)을 사용하여 지식 그래프(KG) 상에서 신뢰성 있는 추론을 수행하는 방법을 다룸.
* 지식 그래프의 구조적 특성을 모델에 반영하여, 보다 정확하고 신뢰할 수 있는 추론을 이끌어내는 방법을 제시

### 실험 과정
 
KG에서 reasoning path를 도출하고, 이를 기반으로 최종 추론을 하고자 함

총 *두 단계*로 이루어짐:
1. **Step 1: Graph-constrained decoding**
	* KG-specialized LLM을 활용하여 KG에서 KG-grounded reasoning paths and hypotheses answers를 도출하는 과정
		* Used Model: Llama-3.1-8B
		* beam-search

2. **Step 2: Graph Inductive reasoning**
	* General LLM을 통해 Step1에서 도출한 resoning path에 대해서 최종 answer을 도출하는 과정
		* Used Model: Llama-3.1-8B. chatgpt-3.5-turbo


WebQSP에서 총 세번의 성능 실험을 함(step1 + step2)
* Llama + ChatGPT (gpu4, rtx3090)
* Llama + Llama (gpu4, rtx4090)
* Llama + ChatGPT (gpu4, rtx4090)

CWQ에서 총 세번의 성능 실험을 함(step1 + step2)

***
## WebQSP 데이터셋
### 1. Llama + ChatGPT (rtx3090)
* 일자: 25.02.19
#### 사용 GPU 및 모델
* **서버 위치**: 데이터센터
- **GPU 개수**: 4
- **파티션**: big_suma_rtx3090

- **Reasoning Path 과정**: `Llama-3.1-8B`
- **Reasoning Finnal Answer**: `gpt-3.5-turbo`
- **사용 데이터셋**: rmanluo/RoG-webqsp의 테스트 데이터(WebQTest-n)
    - **총 데이터 수**: 1628개

#### STEP1: Graph-constrained Decoding
* Model: Llama-3.1-8B

**성능 지표:**
- **Accuracy: 78.50835**
- Hit: 92.19422
     추가 성능지표 확인
        - F1: 58.08451783226196
        - Precision: 56.45388825474874
        - Recall: 77.88374665748543
        - Path F1: 54.956581743950935
        - Path Precision: 55.396849786831346
        - Path Recall: 72.49821259823082
        - Path Answer F1: 59.84054831682146
        - Path Answer Precision: 58.80381654813123
        - Path Answer Recall: 78.50798893908504

**테스트 결과**
- **총 데이터 수**: 1628개
- **전체 소요 시간**: 3h 17m 53s
- **개당 평균 소요 시간**: 7.29s
    - **최장 소요 시간**: `WebQTest-1395` (18.91s)
- **None 결과**: `WebQTest-521`
     WebQTest-521 Data
        ```json
        {
        	"id": "WebQTest-521",
        	"question": "who was anakin skywalker",
        	"answer": ["Ted Bracewell"],
        	"q_entity": ["g.125_cxx77"],
        	"a_entity": ["Ted Bracewell"],
        	"graph": [["Tatooine", "fictional_universe.fictional_setting.characters_that_have_lived_here", "g.125_cxx77"], ["m.011qx8t1", "film.performance.character", "g.125_cxx77"]],
        	"choices": []
        }
	        ```

#### STEP2: Graph Inductive reasoning
* Model: gpt-3.5-turbo

**성능지표:**
- Accuracy: 74.19227
- Hit: 89.68058
- F1: 70.34046
     추가 성능지표 확인
        - Precision: 76.89861
        - Recall: 74.19227

**테스트 결과**
- **총 데이터 수**: 1628개
- **전체 소요 시간**: 2m 18s
- **개당 평균 소요 시간**: 0.085s

> STEP1 + STEP2 최종 답안 도출 시간: 8s

***

### 2. Llama + Llama (rtx4090)
* 일자: 25.02.20
#### 사용 GPU 및 모델
* **서버 위치**: 데이터센터
- **GPU 개수**: 2
	- step1은 2개로 돌렸는데 step2 하려니까 4090에 대기있음... gpu 2->1 해놓음..
- **파티션**: big_suma_rtx4090

- **Reasoning Path 과정**: `Llama-3.1-8B`
- **Reasoning Finnal Answer**: `Llama-3.1-8B`
- **사용 데이터셋**: rmanluo/RoG-webqsp의 테스트 데이터(WebQTest-n)
    - **총 데이터 수**: 1628개

#### STEP1: Graph-constrained Decoding
* Model: rmanluo/Llama-3.1-8B

**성능 지표:**
- Accuracy: 78.46735
- Hit: 92.19422
     추가 성능지표 확인
        - F1: 58.087459371338234
        - Precision: 56.490131803592156
        - Recall: 77.84786951019872
        - Path F1: 54.93910371255629
        - Path Precision: 55.34536248426845
        - Path Recall: 72.51294634221367
        - Path Answer F1: 59.8008934544002
        - Path Answer Precision: 58.77801192183491
        - Path Answer Recall: 78.46698989054858

**테스트 결과**
- **총 데이터 수**: 1628개
- **전체 소요 시간**: 3h 17m 53s
- **개당 평균 소요 시간**: 7~8s
    - **최장 소요 시간**: `WebQTest-1395` (18.91s)
- **None 결과**: `WebQTest-521`
      WebQTest-521 Data
        ```json
        {
        	"id": "WebQTest-521",
        	"question": "who was anakin skywalker",
        	"answer": ["Ted Bracewell"],
        	"q_entity": ["g.125_cxx77"],
        	"a_entity": ["Ted Bracewell"],
        	"graph": [["Tatooine", "fictional_universe.fictional_setting.characters_that_have_lived_here", "g.125_cxx77"], ["m.011qx8t1", "film.performance.character", "g.125_cxx77"]],
        	"choices": []
        }
	        ```

#### STEP2: Graph Inductive reasoning
* Model: Llama-3.1-8B
> step1에서 사용하는 모델은 KG Based 모델이고... step2는 general llm(Llama, Gpt)임ㅜㅜㅜ 아니 이걸로 시간 겁나 끌음..

**성능지표:**
- Accuracy: 72.44364
- Hit: 87.65356
- F1: 69.95159
     추가 성능지표 확인
        - Precision: 77.27516
        - Recall: 72.44364

**테스트 결과**
- **총 데이터 수**: 1628개
- **전체 소요 시간**: 12m 41s
- **개당 평균 소요 시간**: 0.47s

> STEP1 + STEP2 최종 답안 도출 시간: 약 8s

***
### 3. Llama + ChatGPT (rtx4090)
* 일자: 25.02.20
#### 사용 GPU 및 모델
* **서버 위치**: 데이터센터
- **GPU 개수**: 2
- **파티션**: big_suma_rtx4090

- **Reasoning Path 과정**: `Llama-3.1-8B`
- **Reasoning Finnal Answer**: `gpt-3.5-turbo`
- **사용 데이터셋**: rmanluo/RoG-webqsp의 테스트 데이터(WebQTest-n)
    - **총 데이터 수**: 1628개

#### STEP1: Graph-constrained Decoding
* Model: Llama-3.1-8B

Llama + Llama (rtx4090)에서 만든 동일한 reasoning path를 사용하므로 생략

#### STEP2: Graph Inductive reasoning
* Model: gpt-3.5-turbo

**성능지표:**
- Accuracy: 73.78062
- Hit: 89.12776
- F1: 70.532074
     추가 성능지표 확인
        - Precision: 77.23570
        - Recall: 73.78062

**테스트 결과**
- **총 데이터 수**: 1628개
- **전체 소요 시간**: 2m 16s
- **개당 평균 소요 시간**: 0.084s

> STEP1 + STEP2 최종 답안 도출 시간: 

***

## CWQ 데이터셋

* 우선 기본적으로 데이터셋이 커서 4090으로는 reasoning path 과정이 진행되지 않음.
### Llama + ChatGPT (rtx3090)
일자: 25.02.27
#### 사용 GPU 및 모델
* **서버 위치**: 데이터센터
- **GPU 개수**: 2
- **파티션**: big_suma_rtx3090

- **Reasoning Path 과정**: `Llama-3.1-8B`
- **Reasoning Finnal Answer**: `gpt-3.5-turbo`
- **사용 데이터셋**: rmanluo/RoG-cwq의 테스트 데이터(test)
    - **총 데이터 수**: 3531개

#### STEP1: Graph-constrained Decoding
* Model: Llama-3.1-8B

**성능 지표:**
- **Accuracy: 63.39327**
- Hit: 67.6988
     추가 성능지표 확인
        - F1: 39.89591525988563
        - Precision: 33.4333400974026
        - Recall: 63.044758275579305
        - Path F1: 35.820318694880925
        - Path Precision: 33.27493686868687
        - Path Recall: 50.06448307919947
        - Path Answer F1: 40.650900374836404
        - Path Answer Precision: 34.49338248556999
        - Path Answer Recall: 63.265436953519014

**테스트 결과**
- **총 데이터 수**: 3531개
- **전체 소요 시간**: 5h 55m 23s
- **개당 평균 소요 시간**: 6.04s
    - **최장 소요 시간**: `WebQTrn-261_c360906c6dbb01444144ff6ff216a168`, 66.9s
- **None 결과**: 11개 (암호화 문제?)

#### STEP2: Graph Inductive reasoning
* Model: gpt-3.5-turbo

**성능지표:**
- Accuracy: 56.98023
- Hit: 62.33361
- F1: 51.92698
     추가 성능지표 확인
        - Precision: 52.0043
        - Recall: 56.98023

**테스트 결과**
- **총 데이터 수**: 3531개
- **전체 소요 시간**: 4m 20s
- **개당 평균 소요 시간**: 0.0736s

> STEP1 + STEP2 최종 답안 도출 시간: 6.04+0.073 = 6.113s
***
### Llama + Llama (rtx3090)
#### STEP1: Graph-constrained Decoding
* Model: Llama-3.1-8B

Llama + Llama (rtx4090)에서 만든 동일한 reasoning path를 사용하므로 생략

#### STEP2: Graph Inductive reasoning
* Model: Llama-3.1-8B

**성능지표:**
- Accuracy: 
- Hit: 
- F1: 
     추가 성능지표 확인
        - Precision: 52.0043
        - Recall: 56.98023

**테스트 결과**
- **총 데이터 수**: 3531개
- **전체 소요 시간**: 4m 20s
- **개당 평균 소요 시간**: 0.0736s

> STEP1 + STEP2 최종 답안 도출 시간: 6.04+0.073 = 6.113s


***
## Appendix

.sh 파일 for step1

```sh
#!/bin/bash
#SBATCH --job-name=my_GCR
#SBATCH --output=step1_result4090_GCR.txt
#SBATCH --gres=gpu:2

export PYTHONPATH=$(pwd):$PYTHONPATH

MODEL_PATH=rmanluo/GCR-Meta-Llama-3.1-8B-Instruct
MODEL_NAME=$(basename "$MODEL_PATH")

python workflow/predict_paths_and_answers.py \
  --data_path rmanluo \
  --d RoG-webqsp \
  --split test \
  --index_path_length 2 \
  --model_name ${MODEL_NAME} \
  --model_path ${MODEL_PATH} \
  --k 10 \
  --prompt_mode zero-shot \
  --generation_mode group-beam \
  --attn_implementation flash_attention_2
```

.sh 파일 for step2
```sh
#!/bin/bash
#SBATCH --job-name=my_GCR
#SBATCH --output=step2_result_GCR.txt
#SBATCH --gres=gpu:4

export PYTHONPATH=$(pwd):$PYTHONPATH

# MODEL_PATH=rmanluo/GCR-Meta-Llama-3.1-8B-Instruct
MODEL_NAME="gpt-3.5-turbo"
REASONING_PATH="results/GenPaths/RoG-webqsp/GCR-Meta-Llama-3.1-8B-Instruct/test/zero-shot-group-beam-k10-index_len2/predictions.jsonl"

python workflow/predict_final_answer.py \
  --data_path rmanluo \
  --d RoG-webqsp \
  --split test \
  --model_name $MODEL_NAME \
  --reasoning_path $REASONING_PATH \
  --add_path True \
  -n 10
```
* 위는 gpt를 사용한 경우

terminal pronpt 예시
```text
sbatch  --time=7:00:00 -q big_qos -p suma_rtx4090 ./scripts/run2.sh
```
* 근데 확실히 4090은 할당받는거 자체로도 오래 걸리드라...

```text
python workflow/predict_final_answer.py --data_path rmanluo --d RoG-webqsp --split test[:9] --model_name rmanluo/GCR-Meta-Llama-3.1-8B-Instruct --model_path rmanluo/GCR-Meta-Llama-3.1-8B-Instruct --reasoning_path "results/GenPaths/RoG-webqsp/GCR-Meta-Llama-3.1-8B-Instruct/test/zero-shot-group-beam-k10-index_len2/predictions.jsonl" --add_path True  -n 10
```
이거는 규환오빠가 알려준 방식.

```text
srun --gres=gpu:1 --time=1-00:00:00 --pty bash -i
```
터미널 가지고 놀이 좀 어렵다


CWQ 데이터셋, meta-llama/Llama-3.1-8B-Instruct  step2 돌리기
```sh
#!/bin/bash
#SBATCH --job-name=my_GCR_llama
#SBATCH --output=step2_result_GCR_llama.txt
#SBATCH --gres=gpu:2
  
export PYTHONPATH=$(pwd):$PYTHONPATH

### Model Selection(Llama/GPT)
MODEL_PATH=meta-llama/Llama-3.1-8B-Instruct
MODEL_NAME=Llama-3.1-8B-Instruct
# MODEL_NAME="meta-llama/Llama-3.1-8B-Instruct"
# MODEL_NAME=$(basename "$MODEL_PATH")
# MODEL_NAME="gpt-3.5-turbo"
REASONING_PATH="results/GenPaths/RoG-cwq/GCR-Meta-Llama-3.1-8B-Instruct/test/zero-shot-group-beam-k10-index_len2/predictions.jsonl"

python workflow/predict_final_answer.py \
  --data_path rmanluo \
  --d RoG-cwq \
  --split test \
  --model_name ${MODEL_NAME} \
  --model_path ${MODEL_PATH} \
  --reasoning_path ${REASONING_PATH} \
  --add_path True \
  -n 10
```