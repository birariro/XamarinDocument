# ResourceDictionary

배경색 , 텍스트 색 , 스타일 등 <br/>
하나의 앱에서는 어느정도 지정된 패턴을 사용하게된다. 이때 이 속성들을 각각의 xaml에서 처리한다면<br/>
중복적인 코드가 늘고 관리가 힘들어진다.
이를 관리하기 좋게 처리할수있다.

## 공유 ResourceDictionary <br/>
### App.xaml<br/>
```
<?xml version="1.0" encoding="utf-8" ?>
<Application xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MyApp.App">
    <Application.Resources>
        
            <Color x:Key="MainColor">Red</Color>
            <Color x:Key="SubColor">Fuchsia</Color>
            
            <Style TargetType="Label" x:Key="MainLabelStyle">
                <Setter Property="TextColor" Value="Blue"/>
                <Setter Property="FontSize" Value="32"/>
            </Style>
            <Style TargetType="Label" x:Key="SubLabelStyle">
                <Setter Property="TextColor" Value="{StaticResource SubColor}"/>
                <Setter Property="FontSize" Value="20"/>
            </Style>
    </Application.Resources>
</Application>
```

위의 코드를 보면 Color 과 Style를 직접 만들어둔것을 볼수있다. <br/>
해당 리소스에 접근하는기준은 x:Key 를 가지고 접근하게되며 <br/>
사용할때는 다른 xaml 에서 {StaticResource "Key"} 를 이용하여 사용한다. <br/>

```
<Label x:Name="title" FontSize="Title" Padding="30,10,30,10" Style="{StaticResource MainLabelStyle}"/>
<Label x:Name="body" FontSize="16" Padding="30,0,30,0" Style="{StaticResource SubLabelStyle}"/>
<Label FontSize="16" Padding="30,24,30,0" BackgroundColor="{StaticResource MainColor}">
```


## 개별 ResourceDictionary <br/>
개별적으로 사용할경우가 많지는않겠지만 있을수있다. <br/>

```
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MyApp.MainPage">
    <ContentPage.Resources>
    
        <ResourceDictionary>

            <Style TargetType="Label" x:Key="labelStyle">
                <Setter Property="TextColor" Value="Fuchsia"/>
                <Setter Property="FontSize" Value="32"/>
            </Style>
            
        </ResourceDictionary>
        
    </ContentPage.Resources>
``` 
최상위 페이지.Resources 를 통해 <br/>
ResourceDictionary 를 만들수있다.
<br/>
<br/>
<br/>

## DynamicResource
위에서 리소스를 사용할때 StaticResource 를 통해 사용하였다.  <br/>
만약 버튼을 클릭했을때 색을 변경하고싶다면  <br/>
해당 컨트롤 들은 Resource 의 색을 따르고있기때문에  <br/>
Resource 의 색을 직접 변경해줘야한다.  <br/>

```
private void Button_Clicked(object sender, EventArgs e)
{
	Resources["MainColor"] = Color.Black;
}
```
리스소의 Key를 직접 변경하는 코드이다.  <br/>  <br/>

이렇게만 해서는 안된다.  <br/>
view에서 리소스를 사용할때 StaticResource 가 아닌 DynamicResource 로 변경해야한다.  <br/>
```
<Label FontSize="16" Padding="30,24,30,0" BackgroundColor="{DynamicResource MainColor}">
```


## 리소스 파일별 관리하기
결국 Style , Color 등 전체적으로 사용될 리소스정보 들을 모아  ResourceDictionary 한곳에서 관리하는것을  <br/>
목적으로 위와같이 작성하였다.  <br/>
하지만 이부분에서도 문제점은 있다.  <br/>

적은 양의 리소스를 보관한다면 상관없지만 어느정도 많은 양의 리소스정보를 모두 한곳 ResourceDictionary 에 보관한다면  <br/>
이또한 관리가 힘들어진다.  <br/>  <br/>

따라서 어느정도 정리가 필요하다.  <br/>

위에서 사용된 Style 와 Color을 따로 분리하여 사용하는 방법을 알아볼것이다.  <br/>

### ColorDictionary.xaml
```
<?xml version="1.0" encoding="UTF-8"?>

<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms" 
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml">

    <Color x:Key="MainColor">#FF12FF</Color>
    <Color x:Key="SubColor">#911512</Color>
</ResourceDictionary>

```
파일명 그대로 이곳에는 Color 만 작성하였다.

### StyleDictionary.xaml
```
<?xml version="1.0" encoding="UTF-8"?>
<?xaml-comp compile="true" ?>
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms" 
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml">

    <ResourceDictionary.MergedDictionaries>
        <ResourceDictionary Source="ColorDictionary.xaml" />

    </ResourceDictionary.MergedDictionaries>
    <Style TargetType="Label" x:Key="MainLabelStyle">
        <Setter Property="TextColor" Value="Blue"/>
        <Setter Property="FontSize" Value="32"/>
    </Style>
    <Style TargetType="Label" x:Key="SubLabelStyle">
        <Setter Property="TextColor" Value="{StaticResource SubColor}"/>
        <Setter Property="FontSize" Value="20"/>
    </Style>
</ResourceDictionary>
```
이곳또한 Style 만을 정의하였다. <br/>
하지만 Color 과 다른부분이 있다.

```
<?xaml-comp compile="true" ?>
...
<ResourceDictionary.MergedDictionaries>
        <ResourceDictionary Source="ColorDictionary.xaml" />

</ResourceDictionary.MergedDictionaries>
```
다른 Dictionary인  ColorDictionary 의 값을 가지고 와서 사용하기위해 필요한정보이다.
```
 <Setter Property="TextColor" Value="{StaticResource SubColor}"/>
``` 
SubLabelStyle 을 보면 ColorDictionary 의 SubColor 를 가져와서 사용하는것을 볼수있다. <br/>
<br/>

이제 이 2가지 Dictionary 를 한곳에 모아서 다른 xaml에서 사용할수있게 할것이다. <br/> <br/>

### MyResourceDictionary.xaml
```
<?xml version="1.0" encoding="UTF-8"?>
<?xaml-comp compile="true" ?>
<ResourceDictionary xmlns="http://xamarin.com/schemas/2014/forms" 
                    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml">
    <ResourceDictionary.MergedDictionaries>

        <ResourceDictionary Source="StyleDictionary.xaml" />
        <ResourceDictionary Source="ColorDictionary.xaml" />

    </ResourceDictionary.MergedDictionaries>
</ResourceDictionary>

```
이또한 두 xaml 을 사용하기위해
```
<?xaml-comp compile="true" ?>
```
추가가 된 모습을 볼수있다. <br/> <br/>

이제 이것을 사용하려는 xaml 에서 
```
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="MyApp.MainPage">

    <ContentPage.Resources>
        <ResourceDictionary Source="MyResourceDictionary.xaml"/>
    </ContentPage.Resources>
```

MyResourceDictionary 를 Resources 의 Source 로 지정하면된다.


### Class 에서 Resources 변경 및 얻기

#### 리소스 값 얻기
```
Application.Current.Resources.TryGetValue("key", out var value);
```

##### 수정
```
Application.Current.Resources["key"]="0x00";
```


