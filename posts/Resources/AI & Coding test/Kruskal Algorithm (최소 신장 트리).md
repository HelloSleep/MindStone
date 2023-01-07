[[기타 그래프]]
무방향 edge에 대하여 모든 vertext(node)를  1번씩 지나가는 cycle에서 최소 비용을 구할 때 사용

간선의 개수 E 개일 때 시간복잡도 O(ElogE)



```py
def find_parent(parent, x):
  if parent[x] !=x:
    parent[x] = find_parent(parent, parent[x])
  return parent[x]


def union_parent(parent, a,b):
  a= find_parent(parent, a)
  b= find_parent(parent, b)
  if a<b:
    parent[b] = a
  else:
    parent[a] = b

v, e = map(int, input().split())
parent = [0] * (v+1)

edges= []
result =0


for i in range(1, v+1):
  parent[i] = i

for _ in range(e):
  a,b,cost = map(int, input().split())
  edges.append((cost, a, b))

edges.sort()

for edge in edges:
  cost, a, b =edge
  if find_parent(parent,a) != find_parent(parent,b):
    union_parent(parent, a,b)
    result +=cost
print(result)

  

  




```