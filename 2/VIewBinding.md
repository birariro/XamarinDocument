# ViewBinding
View에서 ViewModel 을 Binding 하여 사용하는것 뿐만아니라 <br/>
View 자신 Control 의 속성이 필요로 하는경우가있다. <br/>
<br/>
예로 사용자가 이름을 입력하면 이름을 얻어와서 어딘가에 표시를 해준다고하였을때 <br/>
이름을 입력한 Entry에서 Text를 얻어와 표시할 Label에 직접 넣어주는것은 번거로운 작업이다. <br/>
Label 의 Text는 Entry의 Text를 바인딩 하고있으면 된다. <br/>

```
<Entry x:Name="userName" VerticalOptions="Center" Text="name"/>
<Label x:Name="viewUserName" VerticalOptions="Center"/>
```
위와의 코드에서 Label 의 Text 를 Entry 의 Text 로 바인딩 할것이다.  <br/>

```
<Entry x:Name="userName" VerticalOptions="Center" Text="name"/>
<Label x:Name="viewUserName" VerticalOptions="Center" BindingContext="{x:Reference userName}" Text="{Binding Text}"/>
```
먼저 Label 의 바인딩 대상을 BindingContext 속성을 사용하여 지정한다. <br/>
해당 대상은 userName 이다. <br/>
<br/>
이제 Label 의 Text 부분을 바인딩할것이다. <br/>
바인딩 대상이 Entry 이기때문에 Entry의 모든 속성을 바인딩할수있다. <br/>
그중 원하는것은 Entry의 Text이니  {Binding Text} 를 하는 모습이다. <br/>
