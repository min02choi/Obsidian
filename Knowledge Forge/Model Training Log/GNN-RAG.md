
***
### GNN 예측 결과 해석
GNN의 결과 값
```json
{
	"question": "what does jamaican people speak ",
	"0": {}, "1": {}, "2": {},
	"answers": ["m.01428y", "m.04ygk0"],
	"precison": 1.0, "recall": 1.0, "f1": 1.0, "hit": 1.0, "em": 1,
	"cand": [["m.01428y", 0.5186979174613953], ["m.04ygk0", 0.4811503291130066]]
}
```

* answer: GNN이 **정답 후보로 선택한 Knowledge Graph의 엔티티 ID들**
	* 예)
		* `"m.01428y"` → 영어 (`English language`)
		- `"m.04ygk0"` → 자메이카 파토아 (`Jamaican Patois`)
> (Freebase 등의 KG에서는 각 개념이 이런 식의 `m.`으로 시작하는 고유 ID를 가짐 )

* `cand`: GNN이 예측한 답변 후보들과 그 점수(확률)
**평가 지표**
* Precision: 예측한 답 중 정답 비율 -> 100%
* Recall: 전체 정답 중 예측한 비율-> 100%
* F1: Precision과 Recall의 조화 평균 -> 100%
* Hit@1: 정답이 상위 1개 안에 포함됐는지 -> Yes
* EM(Exact Match): 완전히 일치하는 정답을 예측했는지 -> Yes