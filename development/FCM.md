# FCM 
Firebase Cloud Messaging 의 약자로 Cloud 를 통해 클라이언트에게 메시지를 보내는것을 말한다.

이곳에서는 구현 방법과<br/>
어떻게 관리하고 사용하고있는지를 기록하려한다.<br/>

## 구현방법
[구현방법 블로그](https://blog.naver.com/vps32/222291238139) </br>


## 동작 방식
1. 각 클라이언트는 자신의 로그인 Id 를 구독한다.
2. 서버에게 모든 클라이언트에게 보내고싶은 메시지를 전달한다.
3. 서버는 회원id를 모두 조회하여 회원id를 구독으로보고 모두 FCM으로 순차 전송한다.


## 샘플 코드
[샘플코드 GIT](https://github.com/k4keye/Xamarin/tree/master/FCM_simple) </br>
