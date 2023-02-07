
```py
# https://www.acmicpc.net/problem/11404
# 모든 노드들의 최단거리를 구하는 플로이드 워셜 알고리즘
import sys

input = sys.stdin.readline

# n,m입력받기
N = int(input())
M = int(input())

# distance 2차원 만들기 거리 INF로 초기화
distance = [[sys.maxsize for _ in range(N + 1)] for _ in range(N + 1)]


# 자기 위치에대한 거리는 0으로 초기화
for i in range(N + 1):
    distance[i][i] = 0


# m번만큼, u,v,w 입력받기
for i in range(M):
    u, v, w = map(int, input().split())
    if distance[u][v] > w:
        distance[u][v] = w


# 플루이드워셜 알고리즘.(N<=100이기에 사용가능 O(N^3))
for k in range(1, N + 1):
    for i in range(1, N + 1):
        for j in range(1, N + 1):
            # i에서 j로 갈 때 바로가는 경우(i->j)와 k를 거쳐가는 경우 (i>k>j) 가중치비교
            if distance[i][j] > distance[i][k] + distance[k][j]:
                distance[i][j] = distance[i][k] + distance[k][j]


for i in range(1, N + 1):
    for j in range(1, N + 1):
        if distance[i][j] == sys.maxsize:
            print(0, end=" ")
        else:
            print(distance[i][j], end=" ")
    print()


```