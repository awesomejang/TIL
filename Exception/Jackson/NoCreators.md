# [Jackson] Cannot construct instance of (no Creators, like default constructor, exist)


## 발생상황
Controller호출 시 model에 JSON형식의 데이터를 매핑하는 과정에서 발생

---
## 에러원인 
__JACKSON 라이브러리가 객체 직렬/역직렬화 도중 기본 생성자의 부제로 발생__

---

## 해결방법
* __직렬/역직렬 대상의 객체에 기본생성자를 생성한다.__
* __@JsonCreator, @JsonProperty 어노테이션을 사용하여 대상 필드를 지정한다.__



