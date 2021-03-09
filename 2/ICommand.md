View에서 발생하는 이벤트를 ViewModel에서 처리하기위해서는 <br/>
ICommand를 사용해야한다.


```
<Grid  xe:Commands.Tap="{Binding LogoutTap}">
<Grid  xe:Commands.Tap="{Binding UserDeleteTap}">
```
위와같이 Grid 를 Tap하였을때의 이벤트를 처리하기위해서는 위와같이 Binding를 통해 전달하게되고<br/>
해당 이벤트 Binding는 ICommand를 통해 ViewModel에서 처리한다.

```
public Logout_And_DeletePage()
{
	InitializeComponent();
	BindingContext = new Logout_And_DeleteVM();
}
```
Binding 가 필요한 ViewClass에서는 BindingContext에 ViewModel 을 지정하고

```
class Logout_And_DeleteVM 
{
	public ICommand LogoutTap { get; set; }
	public ICommand UserDeleteTap { get; set; }
	public Logout_And_DeleteVM()
	{
		LogoutTap     = new Command(async () => await Logout_Tapped());
		UserDeleteTap = new Command(async () => await UserDelete_Tapped());
	}
}
private async Task Logout_Tapped(){//로그아웃}
private async Task UserDelete_Tapped(){//계정 제거}
```
ViewModel에서는 View에서 처리하려하는 이벤트 Binding 이름 을 ICommand로 만들어서 처리한다.
