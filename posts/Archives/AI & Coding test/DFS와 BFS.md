
```
from collections import deque
import sys

input =sys.stdin.readline

n, m , start = map(int, input().split())

visited = [0] *(n+1)
graph = [[]*(n+1) for _ in range(n+1)]

for i in range(m):
  a,b = map(int , input().split())
  graph[a].append(b)
  graph[b].append(a)

for i in range(1, n+1):
  graph[i].sort()

def dfs(v):
  visited[v] = 1
  print(v, end =' ')
  for i in graph[v]:
    if not visited[i]:
      dfs(i)

      
def bfs(v):
  
  q = deque([v])
  visited [v] = 1

  while q:
    w= q.popleft()
    print(w ,end = ' ')
    
    for i in graph[w]:
      if not visited[i]:
        visited[i] = True
        q.append(i)
dfs(start)
visited = [0] *(n+1)
print()

bfs(start)

```