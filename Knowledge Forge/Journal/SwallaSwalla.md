#### 2025.02.20(금)
아니 미치겠음 GCR 하는데 왜 모델이 안불러와지냐...
* Llama + ChatGPT (gpu4, rtx3090) => 완료
* Llama + Llama (gpu4, rtx4090) => 하는중
* Llama + ChatGPT (gpu4, rtx4090) => 완료

지금 이거 세개 중에 가운데꺼 하고 있음. 원인을 찾은거 같긴한데 제발요 제발요 아 진짜 제발 착하게 살게요
와 근데 미친 된다. 근데 job 보내는 방식으로 훈련시키는데 진짜 운영체제에서 배우는 스케쥴링을 진짜 실감할 수 있음. 실습시간같아
```text
Map:   0%|          | 0/1628 [00:00<?, ? examples/s]
Map:  61%|██████▏   | 1000/1628 [00:29<00:18, 33.90 examples/s]
Map:  68%|██████▊   | 1111/1628 [00:29<00:13, 38.94 examples/s]
Map: 100%|██████████| 1628/1628 [00:32<00:00, 62.93 examples/s]
Map: 100%|██████████| 1628/1628 [00:32<00:00, 50.12 examples/s]
Save results to:  results/KGQA/RoG-webqsp/GCR-Meta-Llama-3.1-8B-Instruct/test/add_path_results_GenPaths_RoG-webqsp_GCR-Meta-Llama-3.1-8B-Instruct_test__9cbfedd264feaa4dc3c340742a2b95a7_no_dup
Prepare pipline for inference...

Loading checkpoint shards:   0%|          | 0/4 [00:00<?, ?it/s]
Loading checkpoint shards:  25%|██▌       | 1/4 [01:03<03:09, 63.01s/it]
```

아니 근데 되고 나니까 개빡친다 와 진심 진짜 허무함. 이래서 인자를 잘 보는 게

```text
    prediction = model.generate_sentence(input)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: GraphConstrainedDecodingModel.generate_sentence() missing 1 required positional argument: 'trie'
```
엥? 그래도 다른에러네 ㅎㅎㅎ

generate_sentence에 일단 인자 두개 한번 ㅋㅋㅋ줘봐야겠다
아니 근데 나 코딩을 하 못하나 파이썬 파일 개많으니까 헷갈리긴 한다.
와 미친 돌아감. 모델 이름으로 if 문을 구별해놨는데 그거때문에 걸려서 다른 else 문으로 들어갔음

하지만 또 다른 문제 봉착ㅎㅎ
```text
  File "/home/bys3158/anaconda3/envs/GCR/lib/python3.12/site-packages/transformers/tokenization_utils_fast.py", line 572, in _batch_encode_plus
    for key in tokens_and_encodings[0][0].keys():
               ~~~~~~~~~~~~~~~~~~~~^^^
IndexError: list index out of range
```
응 집 가서 고칠게(20:00)

#### 2025.02.24(월)
내가봤을 떄 
```
for key in tokens_and_encodings[0][0].keys():
               ~~~~~~~~~~~~~~~~~~~~^^^
IndexError: list index out of range
```
이 문제는 WebQTest-521 때문에 그러는거 같음. GenPath의 WebQTest-521데이터셋이 없걸랑? 그래서 에러나는게 맞는거같음.

그래서 521을 거르고 나머지 1627개에 대해서 한번 돌려보려다가, `test[:10]` 에 대해서 돌려봄. 근데 왠걸 결과가 다음과 같음.

```text
Accuracy: 0.0 Hit: 0.0 F1: 0.0 Precision: 0.0 Recall: 0.0
```

```text
{"id": "WebQTest-0", "question": "what does jamaican people speak", "prediction": "# Reasoning Path:\nJamaica", "ground_truth": ["Jamaican English", "Jamaican Creole English Language"], "input": "<|begin_of_text|><|start_header_id|>system<|end_header_id|>\n\nCutting Knowledge Date: December 2023\nToday Date: 26 Jul 2024\n\n<|eot_id|><|start_header_id|>user<|end_header_id|>\n\nReasoning Paths:\n# Reasoning Path:\nJamaica -> location.country.currency_used -> Jamaican dollar -> finance.currency.countries_used -> Jamaica\n# Answer:\nJamaica\n# Reasoning Path:\nJamaica -> location.country.languages_spoken -> Jamaican English\n# Answer:\nJamaican English\n# Reasoning Path:\nJamaica -> location.country.languages_spoken -> Jamaican Creole English Language\n# Answer:\nJamaican Creole English Language\n\nQuestion:\nwhat does jamaican people speak?\n\nBased on the reasoning paths, please answer the given question. Please keep the answer as simple as possible and only return answers. Please return each answer in a new line.<|eot_id|><|start_header_id|>assistant<|end_header_id|>\n\n"}
```

* generate_sentence부분을 chatgpt.py와 hf을 비교해가면서 하기


문제
* 521데이터 처리
* predict의 input 데이터 (final answer.py)


돌리기 전
* prediction `test[:5]` (돌리는 데이터셋과 동일한 파일) 지우고 돌리기
* if "GCR" in args.model_name: 이거 바꾸기(내가 수정했던거 다 수정하기...) 토큰 받는 법?

와 완성했다 근데 진짜 ㅋㅋㅋ나 빡대가리네
```text
predict_final_aws.py Line 60:  Llama-3.1-8B-Instruct
base_hf_casual_model.py > generate_sentence
100%|██████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████████| 1628/1628 [14:14<00:00,  1.91it/s]
Accuracy: 71.5221701370911 Hit: 86.67076167076166 F1: 69.33886678528441 Precision: 76.73914294428647 Recall: 71.5221701370911
```
kg 로 된 모델을 왜 쓰냐... 그냥 라마 써야지 바부야ㅜ

#### 25.02.27
일단 hop2로 다 돌렸고, 4시도하는 중
```text
sbatch  --time=3-00:00:00 -q big_qos -p big_suma_rtx3090 ./scripts/run2.sh
```
max값인 3일 줌ㅋㅋㅋ hop수가 증가함에 따라 값이 지수적으로 증가하므로...
개인적으로 결과가 너무 궁금하다.
오늘 논문(GCR)도 다 읽고 집 가야지 일단 본문이라도.