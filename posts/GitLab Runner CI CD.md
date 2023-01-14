test
```sh
#!/bin/bash
sudo hostnamectl set-hostname Gitlab.local

#Git 설치
sudo yum -y update
sudo yum install git -y

```


# docker 및 docker-compose 설치
```sh
#Install docker 
sudo yum install docker -y
sudo usermod -aG docker ec2-user
sudo systemctl enable docker
sudo systemctl start docker
sudo chmod 666 /var/run/docker.sock

#Install docker-compose
sudo curl -L https://github.com/docker/compose/releases/latest/download/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo chmod 666 /var/run/docker.sock

```

 nlb 설정



# GitLab Server 설치
- 방법1: https://insight.infograb.net/docs/setup/install/install_with_docker_compose/
- 방법2: https://docs.gitlab.com/ee/install/docker.html#pre-configure-docker-container

### 설치 디렉토리 생성
이 작업은 container로 생성된 gitlab의 3개의 디렉토리 폴더를 마운트 하기 위한 디렉토리 생성 작업이다. 그래서 위치가 변경 될 수 있다.
```sh
sudo mkdir gitlab  
cd gitlab

sudo mkdir data  
sudo mkdir logs  
sudo mkdir config


sudo chown -R $USER:$USER gitlab
sudo chmod -R 755 gitlab
#명령어 입력시 절대경로 상대경로 모두 가능.현재 작업 디렉토리 기준이기에 경로 생략
```
참고로 저장 되는 파일과 디렉토리 유형은 다음과 같다
![[스크린샷 2022-12-30 오후 2.27.45.png]]


### docker-compose.yml 파일 준비
Gitlab 작업 디렉토리에 환경 구성 파일 생성

```sh
vi docker-compose.yml
```

아래의 내용 입력 후  **`hostname`과 `external_url`은 설치할 서버의 IP 또는 도메인으로 반드시 수정**

```yml
version: '3.9'  
  
services:  
gitlab:  
image: "gitlab/gitlab-ee:14.7.2-ee.0"  
container_name: gitlab  
restart: always  
hostname: "gitlab.example.com" # Change this hostname to your domain name
environment:  
GITLAB_OMNIBUS_CONFIG: |  
external_url 'https://gitlab.example.com' # Chage this domain name to what you set
gitlab_rails['gitlab_shell_ssh_port'] = 8022  
# Add any other gitlab.rb configuration here, each on its own line  
TZ: 'Asia/Seoul'  
ports:  
- "80:80"  
- "443:443"  
- "8022:22"  #22번 포트를 다른 데몬이 사용하는 경우가 많기에 로컬의 다른 포트를 입력
volumes:  
- "./config:/etc/gitlab"  
- "./logs:/var/log/gitlab"  
- "./data:/var/opt/gitlab"

```


### docker-compose 실행
```sh
docker-compose up -d
...

#구동 명령 확인
docker-compose logs -f

#컨테이너 목록 확인
docker-compose ps

#최초의 root의 비밀번호 확인
docker exec -it gitlab grep 'Password:' /etc/gitlab/initial_root_password

```
username: root
password: 위 커맨드에서 입력해서 나온 값

만약 비밀번호 분실시 `Omnibus`로 설치 돼 있다면 <br>
`docker exec -it [gitlab 컨테이너 이름] bash` <br>
`sudo gitlab-rake "gitlab:password:reset"`



# GitLab-runner 설치
1. install -> https://docs.gitlab.com/runner/install/docker.html
2. regitst -> https://docs.gitlab.com/runner/register/index.html#docker

==여기서는 1번 방법인 local volum을 runner를 시작하기 위한 볼륨으로 사용==

### 1. install
```sh
docker run -d --name gitlab-runner --restart always \ -v /srv/gitlab-runner/config:/etc/gitlab-runner \ -v /var/run/docker.sock:/var/run/docker.sock \ gitlab/gitlab-runner:latest
```


### 2. regitst
```sh
docker run --rm -it -v /srv/gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner register
```
1. Enter your GitLab instance URL (also known as the `gitlab-ci coordinator URL`).
2. Enter the token you obtained to register the runner.
3.  Enter a description for the runner. You can change this value later in the GitLab user interface.
4.  Enter the [tags associated with the runner](https://docs.gitlab.com/ee/ci/runners/configure_runners.html#use-tags-to-control-which-jobs-a-runner-can-run), separated by commas. You can change this value later in the GitLab user interface.
5.  Enter any optional maintenance note for the runner.
6.  Provide the [runner executor](https://docs.gitlab.com/runner/executors/index.html). For most use cases, enter `docker`.
7. If you entered `docker` as your executor, you are asked for the default image to be used for projects that do not define one in `.gitlab-ci.yml`.



.gitlab-ci.yml
```yml
stages:
  - build
  - deploy
build-code-job:
  stage: build
  script:
    - echo "Check the ruby version, then build some Ruby project files:"
    - echo "Install zip"
    - apk add zip #기본 컨테이너 이미지에는 zip명령어가 없기에 zip 명령어 설치
    - zip -h
    - zip deploy.zip ./* #appspec.yml, sh파일, index.html을 deploy.zip이라는 이름으로 압축
  artifacts:
  # deploy.zip 파일을 다음 stage에도 유지하여 사용
    expire_in: 1 hour
    paths:
      - deploy.zip
    
deploy-job:
  stage: deploy
  image:
    name: amazon/aws-cli #aws-cli 사용이 가능한 이미지
    entrypoint: [""] #기본 entrypoint가 aws로 설정 돼 있기에 수정 필요
  script:
    - aws configure list # AWS 설정이 제대로 적용이 돼 있는 줄 확인 명령어
    - aws s3 cp ./deploy.zip s3://$S3_BUCKET/ #s3 버킷으로 단일 파일을 업로드.
    - aws deploy create-deployment --application-name web --deployment-config-name CodeDeployDefault.OneAtATime --deployment-group-name web-DG --s3-location bucket=$S3_BUCKET,bundleType=zip,key=deploy.zip
# 이미 배포된 상태의 codedeploy를 이용한다. 중요한 것은 s3-location속성의 버킷이름과 deploy.zip
```
runner의 이미지를 선언하지 않으면 알파인 리눅스가 기본으로 실행된다.
(리눅스 OS 확인을 위한 명령어 `cat /etc/*-release`를 통해 확인 가능)
$S3_BUCKET은 업로드할 s3 버킷이름 변수<br>
`cat /etc/*-release`
을 통해 실행하는 OS 확인 가능


appspec.yml
```yml
#/appspec.yml
version: 0.0
os: linux
# 실행하는 권한 설정 (root)
permissions:
  - object: /
    pattern: "**"
    owner: root
    group: root
files:
  - source: /index.html
    destination: /var/www/html
file_exists_behavior: OVERWRITE
hooks:
  BeforeInstall:
    - location: /BeforeInstall.sh
      runas: root
# AfterInstall앞은 2칸이다.
  AfterInstall:
    - location: /AfterInstall.sh
      runas: root
```


code-deploy agent가 EC2에 설치돼 있어야한다. code deploy가 deploy-agent에 요청하여 agent가 s3로부터 zip파일을 받아와 해제 후 appspec.yml을 보고 필요한 스크립트 실행 및 파일을 배포함
https://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/codedeploy-agent-operations-install-cli.html
EC2에서 압축된 zip 파일을 가져올 수 있게 ec2에 AmazonS3ReadOnlyAccess권한이 필요