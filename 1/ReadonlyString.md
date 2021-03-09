# static readonly
앱에는 당연히 사용자에게 설명하기위한 문자열 혹은 Lebel ,Button 과같은 View관련에도 문자열은 들어가게된다.<br/>
하지만 해당 view파일 혹은 class파일에서 문자열을 그대로 사용한다면 나중에 변경 혹은 관리가  쉽지않다.

Android의 toString()같이 한 리소스파일에서 문자열을 모두 관리하고 가져다가 쓰는 방식을 사용하고싶었다.

그렇기위해서 한 방법이 문자열을 한곳에 모아두는 Class파일을 static 형태로 두고
모든 문자열을 readonly 타입으로 두어 가져다가 쓰는형식을 사용하였다.

```
static class LabelMessages
{
	public static readonly string SuccessId = "사용 가능한 아이디입니다.";
	public static readonly string OverlapId = "중복된 아이디입니다.";
	public static readonly string Fail_Id = "사용할수없는 아이디 입니다.";

	public static readonly string NewPwdTitle = "신규 패스워드 입력"; 
	public static readonly string NewPwdHint = "새로운 패스워드"; 
	public static readonly string ReNewPwdHint = "새로운 패스워드 재입력";
	public static readonly string OkButtonText = "확인";

	public static readonly string UserDelete = "탈퇴 신청";
	public static readonly string UserDeleteDocument = "(신청 7일후,데이터가 완전히 삭제되어 복구되지 않습니다.)";
	public static readonly string UserDeleteCancel = "신청 취소";
	public static readonly string SellerCheckLogout = "로그아웃";

}
```

```
 IdCheckLabel.Text = LabelMessages.OverlapId;
 IdCheckLabel.Text = LabelMessages.Fail_Id;
```
