### 문제
숫자0~9와 알파벳 대문자로만 이루어진 문자열이 입력으로 주어질 때 모든 알파벳을 오름차순한 뒤 모든 숫자의 합을 이어서 출력

#### `입력`
1<=S의 길이 <=10000

#### `출력`
문자열

**`입력 예시`**  
K1KA5CB7

**`출력 예시`** 
ABCCC13


```py
s = input()
result =[]
value = 0
for x in s:
  if x.isalpha():
    result.append(x)
  else:
    value += int(x)
    
result.sort()

if value !=0:
  result.append(str(value))

print(''.join(result))

```