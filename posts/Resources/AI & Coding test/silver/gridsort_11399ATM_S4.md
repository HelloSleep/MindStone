

```py
# https://www.acmicpc.net/problem/11399
n = int(input())

# 배열입력
arr = list(map(int, input().split()))
sum_list = [0 for _ in range(n)]
# 오름차순정렬
arr.sort()
sum_list[0] = arr[0]

# 최솟값 반환

for i in range(1, n):
    # for j in range(i + 1):  # 에러1 range(0)이 아무것도 반환하지 않은걸 깨달음 ->92ms
    #    s += arr[j]

    # 더 좋은 방법. 이전 sum 값을 그대로 이용->48ms 약 2배빠름
    sum_list[i] = sum_list[i - 1] + arr[i]


print(sum(sum_list))

```