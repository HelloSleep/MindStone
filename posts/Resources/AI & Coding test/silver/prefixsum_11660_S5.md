```python
# https://www.acmicpc.net/problem/11660


import sys

input = sys.stdin.readline

n, m = map(int, input().split())

# 원본 배열 및 합 배열 생성.
# 편의를 위해 배열크기는 n+1로 쿼리(1,1) = 배열[1][1]
nums = [[0] * (n + 1)]
d = [[0] * (n + 1) for _ in range(n + 1)]
# 에러1 d에 대한 초기화 시 range범위를 n+1, n 으로 해줘서 오류

# 원본배열 저장 nums i->n
for i in range(n):
    # 원본 배열 저장
    num_row = [0] + list(map(int, input().split()))
    nums.append(num_row)
# 에러2  map함수를 list로 안 바꿔줘서 연산오류
edge = n + 1

for i in range(1, edge):
    for j in range(1, edge):
        d[i][j] = d[i - 1][j] + d[i][j - 1] - d[i - 1][j - 1] + nums[i][j]
# 에러3 마지막에 num[i][j]가 아닌 d[i-1][j-1]을 더해줌

for _ in range(m):
    x1, y1, x2, y2 = map(int, input().split())
    out = d[x2][y2] - d[x2][y1 - 1] - d[x1 - 1][y2] + d[x1 - 1][y1 - 1]
    print(out)
```

