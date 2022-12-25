np.array(list ,type) `하나의 데이터` 타입만 지원

list에서 a[0] is b[0]의 경우 True지만
narray에서는 False. 왜냐하면 리스트는 같은 값이면 같은 메모리를 가리키지만, narray는 다른 메모리에 저장되기 때문

#### shape
shape narray의 dimension반환
dtype narray의 데이터 type반환
```
test_array.shape()
```

1차원 shape -> ( n, )
2차원 shape ->(m, n)
3차원  shape -> (l ,m ,n)
새롭게 rank가 늘어날 때 마다 뒤로 쌓인다

ndim - array dimension
size  - total data count

dtype=np.float64
부호빼고 2^63까지 표현가능

`test_array().nbytes`
SIZE * dtype = 숫자(바이트단위로 반환)





`np.array(test_matrix).reshape(-1,4)`
element가 동일하기 때문에 -1은 나머지 자리를 자동으로 채우게 된다.
할당이 아니라서 원본은 바뀌지 않음

`flatten`  다차원의 array를 1차원 array로 변환
`np.array(test_matrix).flatten()`


[[Indexing and Slicing]]
[[Numpy_Creation]]
[[numpy_Axis]]
[[numpy_concatenate]]
[[numpy operation]]