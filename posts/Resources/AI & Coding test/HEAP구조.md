
## 최소힙
리스트에 담긴 수 중 가장 작은 값을 먼저 return하면 O(log n)
최소 힙의 경우 기본제공 라이브러리사용
```py
import heapq

# 오름차순으로 힙 정렬
def heapsort(iterable):
  h=[]
  result=[]
  # 모든 원소를 차례대로 힙 삽입
  for value in iterable:
    heapq.heappush(h, value)
  for i in range(len(h)):
    result.append(heapq.heappop(h))
  return result
  

```


## 최대 힙
리스트(큐)에 담긴 수중 가장 높은 값을 먼저 return하면 O(log n)
최대 힙은 기본 라이브러리를 제공하지 않기에 최소 힙을 응용하여 구성
```py
import heapq

def heapsort(iterable):
  h = []
  result = []
  for value in iterable:
    heapq.heappush(h, -value)
  for i in range(len(h)):
    result.append(-heapq.heappop(h))
  return result


```


```py
import sys
import heapq

input = sys.stdin.readline
INF= int(1e9)

#노드의 개수, 간선 개수 입력받기
n, m, start = map(int, input().split())

#각 노드에 연결돼 있는 노드에 대한 정보를 담는 리스트 만들기
graph = [[] for i in range(n+1)]

distance = [INF] * (n+1)

for _ in range(m):
  a,b,c = map(int, input().split())
  graph[a].append((b,c))


def dijkstra(start):
  q = []
  #시작 노드로 가기 위한 최단 경로는 0으로 설정, 큐에 삽입
  heapq.heappush(q, (0,start))
  distance[start] = 0
  while q:
    dist, now = heapq.heappop(q)
    # 현재 노드가 처리 돼 있으면 무시
    if distance[now] < dist:
      continue
    for i in graph[now]:
      cost = dist + i[1]
      #현재 노드를 거쳐서, 다른 노드로 이동하는 거리가 더 짧은 경우
      if cost < distance[i[0]]:
        distance[i[0]] = cost
        heapq.heappush(q,  (cost, i[0])
                       
dijkstra(start)


for i in range(1, n+1):
  if distance[i] ==INF:
      print("INF")
  else:
      print(distance[i])

```