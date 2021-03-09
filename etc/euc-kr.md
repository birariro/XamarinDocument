# EUC-KR

HTML 파싱을 하면서 euc-kr 로 인코딩 된 문자열을 제대로 가지고 오지못하는 상황이 생겼다.
```
int euckrCodepage = 51949;
Encoding encode = Encoding.GetEncoding(euckrCodepage);

Stream strReceiveStream = result.GetResponseStream();
StreamReader reqStreamReader = new StreamReader(strReceiveStream, encode);
```
위의 코드를 실행하면
```
encoding 514949 data could not be found , mask sure you have correct international codeset assembly installed and enabled
```
해당 인코딩 데이터를 찾을수없다고 한다.
euc-kr은 기본으로 제공하지않기때문에 발생한 문제.

Android 프로젝트 속성에서 Android 옵션 하단에 있는 추가 지원 인코딩에 CJK 를 추가하는것으로 해결가능
