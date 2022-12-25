
#AWS/CloudFront/Header 
#왜 #어디서 #제약사항 #구현 #문제해결


https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/controlling-the-cache-key.html#cache-key-create-cache-policy
## 왜 써야하는가?
그림을 통해보면 이해하기 쉽다
![[스크린샷 2022-12-13 오후 4.07.14.png]]
일반적으로 다음과 같은 흐름으로 CDN 서비스를 이용하게 된다.

![[스크린샷 2022-12-13 오후 4.10.21.png]]
엣지로케이션은 오리진의 부담을 줄이기 위해 헤더를 전부 전송하는 것이 아닌 일부분을 제거한 헤더를 오리진으로 전송하게 된다.

사용자가 오리진에 요청을 할 때 Edge location에서 임의로 헤더를 버리고 웹 서버에 요청하게 됩니다. 예를 들어 ==브라우저 정보나 OS등의 정보가 담긴 헤더를 버리게==됩니다이 때 모바일유저와 PC유저가 오리진에 요청한 경우 어떤 양식에 맞춰 보내줄지 판단이 불가능 하게 됩니다. 이 때 Forward Header를 사용해 헤더를 추가함으로써 사용자의 헤더정보를 오리진에 보낼 수 있게된다.


## 어디에 쓰는가?
캐시 적중률을 높이기 위해 사용한다. 서로 다른 디바이스가 접속시 구분이나, 특정 언어권에서의 요청만 승낙하는 경우, 

## 제약사항은 무엇인가?
헤더가늘어남으로써 처리하는 양이 늘어나기에 오리진에서 처리하는 속도가 느려질 수 있습니다.


## 구현 방법
[참조 링크](https://aws.amazon.com/ko/premiumsupport/knowledge-center/configure-cloudfront-to-forward-headers/)
	목차
	-준비 사항:AWS 계정
	
	-구현 과정:
	1.  단계에 따라 [CloudFront 콘솔을 사용하여 캐시 정책을 생성](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/controlling-the-cache-key.html#cache-key-create-cache-policy)합니다.
1.  **캐시 키 설정(Cache key settings)**에서 **헤더(Headers)**에 대해 **다음 헤더 포함(Include the following headers)**을 선택합니다. **헤더 추가(Add header)** 드롭다운 목록에서 **호스트(Host)**를 선택합니다.
2.  정책을 연결할 동작의 요구 사항에 따라 캐시 정책의 다른 모든 설정을 완료하고 **생성(Create)**을 선택합니다.
3.  캐시 정책을 생성한 후, 단계에 따라 [CloudFront 배포의 관련 동작에 정책을 연결합니다](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/controlling-the-cache-key.html#cache-key-create-cache-policy).
	
	-구현 결과 

![[Pasted image 20221214150458.png]]

## 문제해결


## 참고 문서
** 캐시 키 제어**
https://docs.aws.amazon.com/ko_kr/AmazonCloudFront/latest/DeveloperGuide/controlling-the-cache-key.html#cache-key-create-cache-policy
