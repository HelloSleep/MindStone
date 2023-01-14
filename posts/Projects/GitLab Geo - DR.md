
참고 개요 링크
https://medium.com/ctc-mzc/gitlab-geo-%EA%B5%AC%EC%84%B1%ED%95%98%EA%B8%B0-%EB%B6%84%EC%84%9D-1-37cef8c440df



```
sudo yum -y install openswan
```



---

secondary 컨테이너에서 primary container 접속시
실제예시
`gitlab-psql -U gitlab_replicator -d "dbname=gitlabhq_production sslmode=verify-ca" -h 10.0.1.66 -W`
이후 프롬프트에서 gitlab_replicator 생성시 입력했던 비밀번호 입력

설명
`gitlab-psql -U [유저이름] -d "dbname=gitlabhq_production sslmode=verify-ca" -h [프라이머리서버 ip] -W`


----

`gitlab-ctl replicate-geo-database \ --slot-name=<secondary_site_name> \ --host=<primary_site_ip> \ --sslmode=verify-ca`

`gitlab-ctl replicate-geo-database \ --slot-name=secondary \ --host=10.0.1.66 \ --sslmode=verify-ca`

slot-name psql에서 ` select * from pg_replication_slots;` 확인후 일치시키기

---
Postgresql 수정 적용

gitlab-ctl reconfigure

gitlab-ctl restart postgresql

---
gitlab 명령어로 포트 확인

gitlab-rake gitlab:tcp_check[<primary_site_ip>,5432]