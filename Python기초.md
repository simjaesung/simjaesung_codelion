# DRY! -> Don't repeat yourself
이번 강의 중 나온 명언?조언?이다. 5번이상 반복하는 작업을 내가 직접하고있다면, 그것은 컴퓨터가 하는 작업을 내가 하고있는 즉, 컴퓨터를 활용을 잘 못하고 있다는 말이다

---
## 1.오늘은 뭐드실?
python의 기본 자료구조인 list, dictionary, set에 대하여 한번 짚고 갈수있는 시간을 갖게되어 좋았다.

## 1-1.  dictionary


dic.get("") -> key를 통해 value 값 얻어오는\
del dic[""] -> key를 통해 선택 삭제\
dic.clear() -> 전체 삭제\
dictionary for문 접근하는 방법!\
1.dic의 이름ㅇ만 쓰는 것이 아닌 dic.items()를 써야 사용가능\
2.인자를 2개 사용하여 key값 하나 value값 하나 얻을 수 있음


### for x,y in dic.items(): 

	print(x) => key
	print(y) => value

## 1-2. set
집합: 순서x 중복x

사용법: list를 먼저 만들고 set으로 변환 set(["짜장면"])


합집합: A | B\
교집합: A & B\
차집합: A - B

**중복제거에있어서 집합이 좋은점이** 같은 원소를 하나로 쳐주기 때문 !

set에서 랜덤 출력하는 방법은 다시 리스트로 바꾼후 random함수를 이용한다.\
random.choice(list(set_lunch))

---
## 2. 이상형이 뭐에요 ?
## 질문 + 답변과 선후관계가 필요한 자료들은 dictionary에 담는 것이 best!!!!
이 강의에서는 2가지 방식으로 질문과 답변에 대한 자료를 묶어서 담고있다.

1. 질문을 key에 답변을 value에, 하나의 dic에 저장
2. 질문과 답변을 value로 담은 dic을 list에 담기

1번 방식은 먼저 딕셔너리에 입력한 값을 key값을 저장, value 값을 공백으로 저장하고 추후 반복문으로 value값을 추가해줬다.

2번 방식은 {"질문":,"답변":}과 같은 dictionary에 우선 질문에 대한 value값을 저장하고 리스트에 추가, 추후 반복문으로 각 dictionary의 "답변"에 접근해 value값을 추가해줬다.

맞고 틀린 방법은 없지만 가독성을 조금더 높이는 2번방식이 조금더 괜찮은 것 같다는 생각이 들었다.
/..