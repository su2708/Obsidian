#TIL #스파르타코딩클럽 [[TIL]]

## next
### 1. next
- iterator로부터 다음 항목을 반환하는 함수
- iterable한 객체는 `next()`와 직접 사용될 수 없기 때문에 iterator로 변환해야 함
- 반복 가능한 객체 -> `iter()` -> `next()`
```python
# 반복 가능한 객체
numbers = [1, 2, 3]

# 이터레이터로 변환
iterator = iter(numbers)

# next()로 값 하나씩 가져오기
print(next(iterator))  # 1 출력
print(next(iterator))  # 2 출력
print(next(iterator))  # 3 출력
```



