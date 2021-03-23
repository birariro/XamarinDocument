# INotifyPropertyChanged
ViewModel 에서는 View에서 보여질 데이터들을 제공해준다. <br/>
만약 ViewModel 에서 제공하는 데이터가 변경이된다면 View에게도 데이터가 변경이 되었으니 화면을 갱신 시킬수있게 해줘야한다.<br/>
이때 INotifyPropertyChanged 를 구현한 PropertyChangedEventHandler 이벤트를 활용한다.<br/>

```
class UserSettingPageVM 
{
}
```

ViewModel 은 INotifyPropertyChanged 를 구현한다.<br/>

```
class UserSettingPageVM : INotifyPropertyChanged
{
	public event PropertyChangedEventHandler PropertyChanged;
	private void OnPropertyChanged(string propertyName = null)
	{
		PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
	}
}
```
위의 모습이 가장 기본이 되는 구조라고 볼수있다.<br/>

```
class UserSettingPageVM : INotifyPropertyChanged
{
	public event PropertyChangedEventHandler PropertyChanged;
	private void OnPropertyChanged(string propertyName = null)
	{
		PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
	}
	
	private UserSettingModel model;
	public UserSettingModel Model
	{
		get => model;
		set
		{
			model = value;
			OnPropertyChanged(nameof(Model));
		}
	}
	public UserSettingPageVM
	{
		Model = new UserSettingModel();
	}
}
```

Model 의 Set 부분을 보면 구현해놓은 이벤트 OnPropertyChanged 에 해당 이름을 넘기는것을 볼수있다.<br/>
이 의미는<br/>
지금 데이터가 변화를 하였다는걸 View에게 알리는 로직이다.<br/>
이로써 View는 데이터가 변화했다는것을 감지하고 화면에 다시 그려주게된다.<br/>
