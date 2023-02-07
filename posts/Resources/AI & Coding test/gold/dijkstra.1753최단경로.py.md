```py

# https://www.acmicpc.net/problem/1753

import sys    #임포트 sys
input= sys.stdin.readline

#v,e 입력받기
#k 입력받기
V, E = map(int,input().split())
K = int(input())

from queue import PriorityQueue#priorit큐 임포트
q = PriorityQueue()

#편의를 위해 1부터 V까지의 값으로 리스트 초기화. 입력이 1이기에
visited = [False] * (V+1) #visited false로 초기화
distance= [sys.maxsize] *(V+1)#distance k제외한 나머지 sys.maxsize로 초기화
arr = [[] for _ in range(V+1) ]#인접노드리스트

#엣지 입력받기
for _ in range(E):
    u,v,w = map(int, input().split())
    arr[u].append((v,w))
    
distance[K] = 0#거리 k번째는 0으로 초기화
q.put((0,K)) #앞의 인자 중요. 우선순위큐의 기준


# q확인하면서 distance값 > distance[현재] + next의w값 보다 작으면 업데이트
while q.qsize()>0:
    current = q.get()[1]
    
    #방문했으면 True이기에 해당 노드 처리 무시 왜냐하면 가장작은경우에 한해 visited가 
    #true로업데이트가 됨으로
    
    if visited[current]:
        continue
    visited[current] = True
    for temp in arr[current]:
        
        next = temp[0]
        value = temp[1]

        #main 알고리즘
        if distance[next] > distance[current] + value:
            distance[next]  = distance[current] + value
            q.put((distance[next],  next)) #우선순위 큐를 사용하기에 distance값 기준으로
            
for i in range(1, V+1):
    if visited[i]:
        print(distance[i])
    else:
        print("INF")
        

```
