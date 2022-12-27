
#왜 #어디서 #제약사항 #구현 #문제해결

## 왜 써야하는가?
git `2.23`  이후 버전에서 추가된 명령어로
이전에 쓰던 git `checkout`이 기능이 너무 많기 때문에 이를 분리하기 위해서 사용. 

## 어디에 쓰는가?
-   [`switch`](https://git-scm.com/docs/git-switch): Switch branches
-   [`restore`](https://git-scm.com/docs/git-restore): Restore working tree files


## 제약사항은 무엇인가?
기존 checkout과 일부 다른 부분 존재


## 구현 방법


- 준비 사항
git 버젼 2.23+

- 구현 과정
```
git switch develop  
# same as 'git checkout develop'

git switch -c new-branch  
# same as 'git checkout -b new-branch'

```

```
git restore README.md  
# same as 'git checkout -- README.md'

git restore --staged README.md
# same as 'git reset HEAD README.md'
```

- 구현 결과



## 문제해결


