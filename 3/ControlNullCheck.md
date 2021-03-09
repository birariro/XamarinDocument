사용자가 입력한 Label 의 Text를 가져오는등
Control의 접근하여 값을 가져올때 항상 null인지 조심하고 가져와야한다.

만약 PasswordText 라는 Label의 Text를 가져올때 사용자가 아무값도 입력을안했다면
```
string userPassword = PasswordText.Text;
```
위와같이 코드를 실행할때 null의 Text를 얻어오려하니 에러가 발생한다.
해당 요소에 접근하기전에 null인지 먼저 검사를 해야한다.
```
string userPassword = PasswordText?.Text;
```
위와같이 Text를 가져오기전에 ? 를 붙히면 앞의 요소가 null이라면 null을 반환하고 아니라면 Text에 접근하여 원하는 값을 반환한다.
```
string userPassword = PasswordText?.Text;
if(userPassword != null)
{
    
}
```
