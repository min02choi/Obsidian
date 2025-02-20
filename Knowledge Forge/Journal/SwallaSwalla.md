#### 2025.02.20
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