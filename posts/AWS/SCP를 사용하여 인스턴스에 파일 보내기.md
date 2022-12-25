#SCP #EC2 #Linux

22번 포트를 사용하여 파일을 전송할 수 있다.
scp를 인스턴스에 보내기 위해선 기본명령어 이외의 추가옵션을 붙인 명령어로 실행할 필요가있다.

참고용으로 기본 명령은 다음과 같다.
```shell
scp [소스파일] [보낼 위치]
```

인스턴스에 사용할경우 Bastion host를 접속하기 위한 인증키를 포함하여 명령어를 실행하여야한다.

`scp -i [Bastion EC2로 들어가기위한 pem] [EC2로 전송할 파일] [유저ID]@[주소]:위치`

이와같은 형태로 명령어를 사용하게 된다.  실제 사용하는 경우 다음과 같다

`scp -i onboarding.pem onboarding.pem ec2-user@3.37.176.70:/home/ec2-user
(명령어를 실행할 때의 폴더 위치에 모든 파일이 있는 상황)
ex)key-folder$ls
onboarding.pen

해석: Bastion EC2로 접속하기 위해 onboarding.pem이라는 인증키를 사용하여 onboarding.pem이라는 파일을 전송한다.
전송 유저는 ec-user이며  IP는다음과같다
전송 파일 저장 위치는 /home/ec2-user 아래이다.


## Unoritected Private Key File 오류가 생겼다면
 ![[스크린샷 2022-12-02 오후 4.02.25.png]]
 다음 오류는 전송하려는 파일의 권한이 수정이 가능하여 다른 사람에게 보낼 수 없음을 의미한다.

그래서 chmod라는 명령어를 통해 다른 유저는 이 파일을 볼 수도, 수정할 수도 없겠끔 권한 수정이필요하다.
`chomod 400 [key파일]`

