# ViewModel 

ViewModel을 사용한다는것은 기존의 한곳에 모여있는 UI 관련 코드 , 데이터 관련 코드 , 데이터베이스 관련 코드등 <br/>
여러 역활이 하나의 Class에서 처리하던것을 분리하는것이다.

View는 View에 대해서만 수행하고 View가 필요로 하는 데이터 혹은 이벤트 처리 등을  ViewModel 에서 처리한다.

이벤트 처리는 [ICommand](https://github.com/k4keye/XamarinDocument/blob/main/2/ICommand.md) 로 처리하게되니
여기서는 View에서 필요로하는 데이터를 ViewModel에서 받아서 사용할것이다.

정석적으로는 ViewModel 에서도 데이터 관련은 Model이 있어야하지만 생략한다.

```
<StackLayout>
	<Frame BackgroundColor="#2196F3" Padding="24" CornerRadius="0">
		<Label x:Name="title" Text="Welcome to Xamarin!" HorizontalTextAlignment="Center" TextColor="White" FontSize="36"/>
	</Frame>
	<Label x:Name="body" Text="Hello World" FontSize="Title" Padding="30,10,30,10"/>
</StackLayout>
```
위는 ViewModel을 적용하기전 코드이다. xaml 에서 나 혹은 ViewClass 에서 Label에 들어갈 Text를 직접 넣은 코드이다.

```
<StackLayout>
	<Frame BackgroundColor="#2196F3" Padding="24" CornerRadius="0">
		<Label x:Name="title" Text="{Binding TitleText}" HorizontalTextAlignment="Center" TextColor="White" FontSize="36"/>
	</Frame>
	<Label x:Name="body" Text="{Binding BodyText}" FontSize="Title" Padding="30,10,30,10"/>
</StackLayout>
```
ViewModel 을 사용하기위한 View 의 변화이다. <br/>
Text 부분이 {Binding ...} 로 바뀌었는데 <br/>
이것은 어떠한 대상에게 있는 값을 사용하겠다고 이해할수있다. <br/>

쉽게 보면 다른 클래스에게 있는 어떠한 변수를 해당 Label 의 Text로 사용하겠다는것이다. <br/>

```
class MainVM
{
	public string TitleText { get; set; }
	public string BodyText { get; set; }
	public MainVM()
	{
		TitleText = "Welcome to Xamarin!";
		BodyText = "Hello World";
	}
}
```
ViewModel 이다  코드를 보면 View에서 사용하고자하는 변수 2개가 존재하는것을 볼수있다. <br/>

이제 View에서 이 ViewModel을 사용하겠다고 알려주면된다.
```
public MainPage()
{
	InitializeComponent();
	BindingContext = new MainVM();
}
```
