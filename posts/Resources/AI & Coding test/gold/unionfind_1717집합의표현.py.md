```py
# https://www.acmicpc.net/problem/1717
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**6)
#RecursionError를 처음 봤고 해결책도 처음봄

# n,m 입력받기
n, m = map(int, input().split())
arr = [i for i in range( n + 1)]


def find(a):
    if arr[a] == a:
        return a
    arr[a] = find(arr[a]) #이게 중요한데 처음에 return find(arr[a])를 해서 틀림
                          #왜냐하면 리스트를 업데이트해줘야함. 반환값만 쓰는게 아니라
    return arr[a]


def union(a, b):
    pa = find(a)
    pb = find(b)
    #if pa <= pb:
    arr[pa] = pb #현재는 딱히 조건이 없어서 이렇게 적지만 일반적으로 숫자가 낮은경우가 대표노드로 됨


for _ in range(m):
    u, a, b = map(int, input().split())
    
    if u == 0:
        union(a, b)
        
    elif u == 1:
        if find(a) == find(b):
            print("YES")
        else:
            print("NO")
        


```