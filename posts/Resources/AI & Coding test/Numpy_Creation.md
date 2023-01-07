`range`:숫자 생성
`np.arrange(시작:끝:스텝)` 특이하게 실수형으로 스텝 지정 가능

`ones` 1로 가득찬 ndarray 생성
np.ones(shape, dtype, order)
ex -> np.ones(shape(10 , ), dtype=np.int8 )

`zeros` 0으로 가득찬 ndarray생성


`empty` 빈공간으로 가득찬 ndarray 생성

## Something_like
한 array의 shape크기만큼 특정 원소로 가득찬 array 반환
np.ones_like(list)
np.zeros_like(list)
np.empty_like(list)


### eye
대각선이 1인 값인 array생성


### diag

