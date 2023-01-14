`Snippet`: 큰 수준에서 사용 가능한 작은 파편 수준의 코드나 텍스트. 이식성이 좋아야 하며 공유가 다른 사람과의 공유가 쉬움




[[#^1d1a1c | echo_stage ]]

```yml
stages:

- test

  

test-code-job1:

stage: test

script:

- echo "[$CI_JOB_STAGE] stage start"

```

^1d1a1c

