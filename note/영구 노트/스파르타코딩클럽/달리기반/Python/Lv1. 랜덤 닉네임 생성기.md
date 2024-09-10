
## 문제: 랜덤한 닉네임을 생성하는 파이썬 코드 작성

### 요구사항
1. **사용자는 최소 27가지 이상의 닉네임 중 하나를 랜덤으로 print 할 수 있습니다.** (아래의 키워드를 사용해주세요!)
    - 기철초풍, 멋있는, 재미있는
    - 도전적인, 노란색의, 바보같은
    - 돌고래, 개발자, 오랑우탄

---
## 내 답변

``` python
import random

first_list = ["기절초풍", "멋있는", "재미있는"]
second_list = ["도전적인", "노란색의", "바보같은"]
third_list = ["돌고래", "개발자", "오랑우탄"]

def create_random_nickname():
    # 여기에 랜덤으로 닉네임을 만드는 코드를 적어주세요
    i = random.randint(0, 2)
    j = random.randint(0, 2)
    k = random.randint(0, 2)

    random_nickname = f"{first_list[i]} {second_list[j]} {third_list[k]}"
    
    return random_nickname

my_nickname = create_random_nickname()
print(my_nickname)
```