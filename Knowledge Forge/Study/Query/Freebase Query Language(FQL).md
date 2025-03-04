---
aliases:
  - Freebase Query Language
  - FQL
  - Graph Query Language
  - GQL
---
### **📌 Freebase Query Language(FQL) 개념 정리**

**Freebase Query Language(FQL)**는 Freebase(구글이 운영했던 대규모 지식 그래프)에서 **데이터를 질의(Query)하기 위해 사용되던 쿼리 언어**
SPARQL과 비슷하지만, JSON 스타일의 구조를 가지며 **그래프 기반 데이터베이스에서 관계를 탐색하는 방식**을 사용

---

## **1️⃣ FQL의 특징**

* JSON 기반 구조→ 사람이 읽기 쉽고, REST API를 통해 요청 가능  
* 그래프 탐색 쿼리 → 개체(Entity) 간의 관계를 따라가며 검색 가능  
* 트리플 구조를 활용 → `(주어, 관계, 목적어)` 형태로 데이터 저장  
* Freebase API와 함께 사용→ Freebase의 데이터를 검색하는 데 최적화됨

---

## **2️⃣ FQL의 기본 구조**

FQL은 **JSON 형식**으로 되어 있으며,  
쿼리는 객체(엔티티), 속성(프로퍼티), 값(데이터)를 기반으로 작동함.

### **(1) 기본적인 FQL 쿼리 예시**

**예제:** "Barack Obama의 출생지를 찾아라."

json형식의 쿼리
```json
{
	"type": "/people/person",
	"name": "Barack Obama",
	"place_of_birth": []
}
```
#### **📌 설명**
- `"type": "/people/person"` → 이 개체는 "사람(person)" 타입임
- `"name": "Barack Obama"` → 찾고자 하는 개체(바락 오바마)
- `"place_of_birth": []` → 바락 오바마의 출생지 정보를 가져옴

=> **결과:** `"place_of_birth": "Honolulu, Hawaii"` 반환

---
### **(2) 특정 개체의 모든 정보를 가져오는 쿼리**

**예제:** "Google에 대한 정보를 가져와라."

json
```json
{
	"type": "/business/company",
	"name": "Google",
	"*": null
}
```
#### **📌 설명**
- `"type": "/business/company"` → 기업 데이터에서 검색
- `"name": "Google"` → "Google"이라는 이름을 가진 엔티티
- `"*": null` → Google의 **모든 속성을 가져옴**

✅ **결과:** Google의 설립 연도, 창립자, 위치, CEO 등 반환

---
### **(3) 관계 기반 검색 (JOIN)**

**예제:** "바락 오바마의 자녀를 검색하라."

json
```json
{
	"type": "/people/person",
	"name": "Barack Obama",
	"children": [{     "name": null   }] }
```

#### **📌 설명**
- `"children": [{ "name": null }]` → 오바마의 자녀들의 이름을 검색  
    ✅ **결과:** `"children": ["Malia Obama", "Sasha Obama"]` 반환