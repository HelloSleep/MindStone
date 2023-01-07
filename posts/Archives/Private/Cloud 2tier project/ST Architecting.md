
[[ST Architecting 피드백]]

중요한 것. Architecture를 먼저 완성하는 것. 그래야 그 아키텍쳐를 보고 피드백을 받고 수정할 수 있음.
예를 들러 IAM을 사용한다고 하면 **"\~~한 이유로 사용하겠다** "까지만 하면 되지,   세세하게 정책 하나하나를 고려하지 않아도 됨. 최대 **"IAM UserGroup을 나누겠다 정도까지"**


- 네트워크
	- VPC 10.0.0.0/16
		-  Subnets Public 2(10.0.100,200/24) Private 4( 10.0.1,2,3,4.0/24)
	- Route Table
	- Internet GateWay(Plum_IGW)
	- NAT Gateway
	- Elastic Load Balancing
	- Route53(mc2bb.com, public hosting)
- 컴퓨팅
	- EC2
	- ALB
	- Auto Scaling
- 데이터베이스
	- RDS(Aurora)
- 스토리지
	- S3
- 컨텐츠배포
	- CloudFront
- 보안
	- ACM
	- IAM (EC2InstanceDeployRole, CodeDeployRole)
		-
	- 보안그룹 2 (Webserver, Plum -ALB, )
- CI/CD
	- Code Commit( MyDemoRepo)
		- 
	- Code Deploy(MyDemoApplication)
	- Code Pipeline(MyFirstPipeline)

- 다른 계정에서 Route53 도메인이동( 리전설정 수정 필요)
https://www.44bits.io/ko/post/aws-route-53-migration-domain-to-another-account

- ACM의 DNS검증을 통해 인증서 발급
>https://happy-jjang-a.tistory.com/102
>https://www.youtube.com/watch?v=9MdcEOHbIIc
>[다른블로그](https://libertegrace.tistory.com/entry/Route-53-%EC%84%9C%EB%B2%84-%EB%8F%84%EB%A9%94%EC%9D%B8%EC%9D%84-Route53%EC%97%90-%EB%93%B1%EB%A1%9D%ED%95%98%EA%B3%A0-%EC%9B%B9-%EC%84%9C%EB%B2%84%EC%97%90-%EB%8F%84%EB%A9%94%EC%9D%B8-%EC%97%B0%EA%B2%B0%ED%95%98%EA%B8%B0)


- 간단한 코드커밋과 코드파이프라인을 이용한 자동배포 
https://docs.aws.amazon.com/codepipeline/latest/userguide/tutorials-simple-codecommit.html
> #CodeCommit #CodePipeline 

- WordPress와 CodeDeploy와 S3사용 방법
https://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/tutorials-wordpress-launch-instance.html
> #WordPress #S3 #CodeDeploy


- CodeDeplo와 Auto Scaling 튜토리얼+이론내용 
https://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/integrations-aws-auto-scaling.html
[튜토리얼링크](https://docs.aws.amazon.com/ko_kr/codedeploy/latest/userguide/tutorials-auto-scaling-group.html)


- aurora는 아니지만 RDS로 DMS이용하여 마이그레이션
>https://cloudest.tistory.com/70
> #DMS

- WordPress파일을 S3에 업로드 후 CDN과 연동
> https://linuxer.name/2019/09/wordpress-s3-cloudfront-%EC%A0%81%EC%9A%A9%ED%95%98%EA%B8%B0/
> https://www.opsnow.com/aws%EC%97%90%EC%84%9C-%EC%9B%8C%EB%93%9C%ED%94%84%EB%A0%88%EC%8A%A4-%EB%B8%94%EB%A1%9C%EA%B7%B8%EC%97%90-cdn-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0/
> https://kinsta.com/knowledgebase/wordpress-amazon-s3/
>  #CDN #AWS/CloudFront #S3


- CloudTrail을 사용한 감사
>https://docs.aws.amazon.com/ko_kr/awscloudtrail/latest/userguide/cloudtrail-tutorial.html

- AWS Aurora Auto Scaling 메커니즘 설명
> https://anggeum.tistory.com/entry/RDS-AWS-Aurora-Auto-Scaling-%EB%A9%94%EC%BB%A4%EB%8B%88%EC%A6%98-Deep-Dive
#AWS #Aurora #AutoScaling



- MySQL dump를 사용하여 S3에 dump파일 저장후 이를 Aurora Cluster에서 구현하기
> https://docs.aws.amazon.com/ko_kr/AmazonRDS/latest/AuroraUserGuide/AuroraMySQL.Migrating.ExtMySQL.html#AuroraMySQL.Migrating.ExtMySQL.mysqldump
> 
> https://brunch.co.kr/@topasvga/1159
>  #mysqldump #Aurora #S3


- CloudFront 다양한 실습
>https://brunch.co.kr/@topasvga/1776
> #AWS/CloudFront


- Ssm 실습
>https://musma.github.io/2019/11/29/about-aws-ssm.html
> #SSM #Bastion




---


하면서 깨달은 것
- ALB를 연결하기 위한 VPC설정시 VPC에 인터넷게이트 웨이가 attatch 돼 있지 않다면 활성화 되지 않는다 #ALB
- Route53의 도메인등록에서 NS서버를 바꾼 경우 충분한 시간이 지나고 나야 등록이 된다. 충분한 시간이란 보통 24시간~48시간인데 그 이유는 네임서버의 캐쉬가 보통이 기간동안 살아있기 때문.왜냐하면 기본값의 NS서버의 TTL이 172800초로 시간으로따지면 48시간임 #Route53
>You transferred DNS service first, but you didn't wait long enough before transferring domain registration
>https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/troubleshooting-domain-unavailable.html#troubleshooting-domain-unavailable-transferred-domain-too-soon-after-dns-transfer
#mydomainunable



API라는 작업들을 관리하기 위한 것이 IAM
IAM이란 서비스의 세세한 부분을 제한하는 것이 아니라(가능은 하지만) 전체적인 서비스제한을 하기 위한 것