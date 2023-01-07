

[[ST Implementation 피드백]]

DB너무 신경쓰지말고 접근만 되면 되고
CI/CD 데모 신경 쓰고 (블루 그린으로 리소스 늘어나고 업데이트  되는 부분)
어떤 문제상황에서 어떻게 해결했나 라는 부분이 보여야하고
하루를 미루면 그만큼 기대감은 올라가는 것이고
CodeCommit은 IAM써서 제한할 수 있는데 블로그 찾아보면 나오고
ARN기반으로 접근 권한 하고

- MySQL dump를 사용한 마이그레이션
  2000개가 넘는 데이터를 가진DB

  
  mysqldump명령어

  mysqldump 복원 명령어
  ```mysql
  #먼저 옮기고자 하는 DB에서 빈 database 생성
  create database [임의의 DB]
  mysql -u root -p [임의의 DB] < [dump파일 경로][dump 파일].sql ```
  sql문으로 생성된 DB
  ![[스크린샷 2022-12-02 오후 2.29.28.png]]

==aaa =========

- Bastion에서 ssh로 접속

  
  
- Bastion에서 DB 접속


dump파일을 Aurora로 옮기는데 문제


---
Aurora S3 Access
https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Integrating.LoadFromS3.html

mysql 설치
[[AWS _Amazon_Linux2_MySQL _Install.sh]]

```
mysql -h [엔드포인트] -u admin -p [Aurora의 임의 DB] < [dump하려는 sql파일]
```

Parameter그룹 및 IAM설정  Aurora에 적용하기
https://yahwang.github.io/posts/97
https://anand-prakash.medium.com/aurora-mysql-export-data-to-s3-8c2323be1cb9


```
mysql> SELECT @@GLOBAL.aurora_load_from_s3_role;


```



---
[[AWS _Amazon_Linux2_Wordpress _Install.sh]]

```
aws deploy push \
  --application-name WordPress_App \
  --s3-location s3://plum-wordpress/WordPressApp.zip \
  --ignore-hidden-files
```



```

```

WordPress 대문 수정

```
/var/www/html/wp-admin

sudo vi about.php

 23                         <div class="about__header-title">
 24                                 <h1>
 25                                         <?php
 26                                         printf(
 27                                                 /* translators: %s: Version number. */
 28                                                 __( 'WordPress %s' ),
 29                                                 $display_version+10
 30                                         );
 31                                         ?>
 32                                 </h1>
 33                         </div>


29번째 줄 display_version+10 부분 수정

```

Code Commit Codedeploy Code Pipieline만들기

Code deploy ASG연결하기

ASG ELB연결하기


---
Code Deploy



---
ALB및 ASG

ALB , TG
### ALB WordPress Heathcehck. Apache succeed code is 301, so just add health code 301 in ALB
> g. In the **success codes** enter **200, 301** (After Installing WordPress when we will hit the request to ALB endpoint Internally Apache Web Server will Redirect our traffic to WordPress and it will give 301 status code. Flow of the request will be — Request on ALB Endpoint -> Apache WebServer -> WordPress. So basically our Apache WebServer is redirecting the traffic so success code for TG will be 301). If you don’t add 301 in **Success Codes** while Creating TG your Target Health Check might fail with error **Getting 301 status Code**

![](https://miro.medium.com/max/700/1*1J5fbKA_uCiWTm1dz8aQHg.png)

**Note:** Add 200 and 301 both the status code because till now when we hit the request the success code will be 200 but after installing and configuring WordPress the success code will be 301. Don’t forget to Add the ID of ALB Sg into the inbound rule of EC2 SG. 

[[AWS _Amazon_Linux2_Wordpress _Install.sh]]
ASG


appspec.yml
```
version: 0.0
os: linux
files:
  - source: /
    destination: /var/www/html/WordPress
hooks:
  BeforeInstall:
    - location: scripts/install_dependencies.sh
      timeout: 300
      runas: root
  AfterInstall:
    - location: scripts/change_permissions.sh
      timeout: 300
      runas: root
  ApplicationStart:
    - location: scripts/start_server.sh
    - location: scripts/create_test_db.sh
      timeout: 300
      runas: root
  ApplicationStop:
    - location: scripts/stop_server.sh
      timeout: 300
      runas: root

```

appspec.yml (for test after)
```
version: 0.0
os: linux
files:
  - source: /wp-content/themes/twentytwentythree/templates
    destination: /var/www/html/wp-content/themes/twentytwentythree/templates
hooks:
  BeforeInstall:
    - location: scripts/install_dependencies.sh
      timeout: 300
      runas: root
  AfterInstall:
    - location: scripts/change_permissions.sh
      timeout: 300
      runas: root
  ApplicationStart:
    - location: scripts/start_server.sh
    - location: scripts/create_test_db.sh
      timeout: 300
      runas: root
  ApplicationStop:
    - location: scripts/stop_server.sh
      timeout: 300
      runas: root


```

[[Codedeploy]]
code deploy agent 다운
배포시간 약 5분 30초
https://nobang.tistory.com/entry/AWS-CodePipeLine-%EC%B4%88-%EA%B0%84%EB%8B%A8-%EB%B2%84%EC%A0%84-2-CodeDeploy1
```

#!/bin/bash  
yum -y update  
yum install -y ruby  
yum install -y aws-cli  
cd /home/ec2-user  
wget [https://aws-codedeploy-us-east-2.s3.us-east-2.amazonaws.com/latest/install](https://aws-codedeploy-us-east-2.s3.us-east-2.amazonaws.com/latest/install)  
chmod +x ./install  
./install auto
```


---

- CodeCommit SSH 확인 명령어 [[CodeCommit]]
  ` git-codecommit.ap-northeast-2.amazonaws.com`

- CloudFront에서 S3 origin 설정시 OAC옵션 체크
  Policy를 복사버튼 클릭
  S3 버킷 권한에서 위에서 복사된 내용 붙여넣기


---
EC2 bastion tunneling [[AWS EC2]] [[Bastion host]]
```
ssh -i onboarding.pem -L 33323:10.0.1.152:22 ec2-user@3.37.176.70
ssh -i [베스천으로가기위한 프라이빗 키위치] -L [로컬포트]:[프라이빗EC2 IP]:22 [사용자이름, 일반적으로 ec2-user]@[베스천IP]
```
연결된 상태에서, 새로운 터미널창을 열고
```
ssh -i [프라이빗 키 위치] -p [위에서 설정한 로컬포트] [사용자이름 일반적으로ec2-user]@localhost
```

```shell
#Config 파일로 만들기
Host git-codecommit.*.amazonaws.com
  User APKA4ZVV7UEDWDZK6YM6
  IdentityFile ~/.ssh/id_rsa

Host rds_tunnel
  user ec2-user
  Hostname Bastion IP
  Localforward [로컬 포트] wordpress-instance-1.c04vwjeerjgd.ap-northeast-2.rds.amazonaws.com:3306
  IdentityFile [프라이빗키 위치]

Host redis_tunnel
  user ec2-user
  Hostname [Bastion IP]
  localforward [로컬포트] plum-001.tak7if.0001.apn2.cache.amazonaws.com:6379
  IdentityFile [프라이빗키 위치]

```
즉 Bastion host가 열린 상태에서 로컬의 임의의 포트가 프라이빗 EC2의 포트로 포워딩 됨.


---
배포시간

TG에서 Attribute의 Target Configuration의 Deregistration Delay를 300초->60초 (기존 배포 5분30초->6분 및 AfterAllowTraffic차이 없음 단 AfterBlockTraffic이 5분40초에서 2분 30초로 줄어듬)
TG의 Health Check Setting 수정 Healthy Threshold 5->3, Interval 30->10(기존 배포 5분 30초 -> 5분30초 단 AfterAllowTrafic이 46초, BlcokTraffic 1분30초로 수정)
CodeDeploy의 Deployment configuration수정 Allatonece-> 60%로수정 (오류와 함께 배포 실패 The deployment failed because no instances were found in your blue fleet.)

---
- MAC Redis 설치 [[Amazon ElastiCache]]
  https://cherish-it.tistory.com/35




---
[[Linux Command]]
Stress 유틸리티 다운
```
sudo amazon-linux-extras install epel -y

sudo yum install stress -y

ex)
stress --cpu 2 --timeout 600
stress를 줘라
cpu 두개를 만들어서 sqrt명령어를 반복
600초 동안
```

---
ssh 교환 후 WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED! 문구
```
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that the RSA host key has just been changed.
The fingerprint for the RSA key sent by the remote host is
-생략-
Please contact your system administrator.
Add correct host key in /home/warren/.ssh/known_hosts to get rid of this message.
Offending key in /home/warren/.ssh/known_hosts:1
RSA host key for -생략(ip)- has changed and you have requested strict checking.
Host key verification failed.
```



위의 상황이 일어나는 이유는 다음과 같다
[참고 링크](https://visu4l.tistory.com/entry/ssh-%EC%9B%90%EA%B2%A9-%EC%A0%91%EC%86%8D-%EC%97%90%EB%9F%ACWARNING-REMOTE-HOST-IDENTIFICATION-HAS-CHANGED)
ssh 최초접속시에 A와 B에서 서로간에 인증 과정을 하는데.. B는 새로 설치되었으니 B는 상관없지만..
A는 예전B에  IP로 인증이 되어있는 상태에서 B로 로그인을 하면
로그인시에 예전에 IP로 인증했던 정보를 가지고 B로 로그인을 하려고 하지만 B는 인증정보가 없기때문에
위와 같은 현상이 나타난다.

위에 상황을 예를 들면
A host 가 있고 B server가 있다. 
A는 항상 B 서버에 ssh접속하고 있었는데 B서버에 ssh나 os를 새로 설치 하는 작업을 했다.
그런후에 A는 똑같이 B에 접속을 한다. 이때 B에 IP는 똑같다면...
위와 같은 메시지가 뜬다

~/.ssh/knon_hosts에서 해당 포트 SSH 정보 삭제
or
`ssh-keygen -R "[localhost]:33323"`
`ssh-keygen -R [IP or DomainName]`



---
Launch Templates에서 기본 ENI의 Primary IP변경
추가 ENI의 Primary IP변경 후 서브넷 추가

---

redis-cli 설치
https://jojoldu.tistory.com/348
```sh

# make 하기 위핸 gcc 다운
sudo yum install -y gcc

# redis-cli 설치 및 make
wget http://download.redis.io/redis-stable.tar.gz && tar xvzf redis-stable.tar.gz && cd redis-stable && make

# redis-cli를 bin에 추가해 어느 위치서든 사용 가능하게 등록
sudo cp src/redis-cli /usr/bin/


```
