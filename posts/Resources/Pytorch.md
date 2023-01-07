
텐서 초기화
텐서의 속성
텐서 연산
 - NumPy식의 표준 인덱싱과 슬라이싱
 - 텐서합치기(torch.cat)
 - in-place 연산 x.copy_(), x.t_(), x.add_()
Numpy 변환(Bridge) 텐서와 NumPy배열은 메모리공유
  - `텐서->numpy` 텐서.numpy()
  - `NumPy->텐서` torch.from_numpy(narray)
  - 