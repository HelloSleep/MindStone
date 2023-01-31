```py
# https://www.acmicpc.net/problem/1934
import math

# gcd 사용을 위한 math import
# n 입력
n = int(input())


# n 만큼 반복
# 두수 저장
# 두수 gcd저장
# lcm = a*b/ gcd(a,b)
for _ in range(n):
    a, b = map(int, input().split())
    g = math.gcd(a, b)
    print(int((a * b) / g))



```