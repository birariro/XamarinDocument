# AXAML 을 작성할때 사용되는 Control 의 속성값의 순서를 부여해서 가독성을 높이기 위한 목적

## 자주사용하는 Control 은 StaticResource로 정의후 사용한다.

## 각 속성은 우선순위를 둔다.

## 1 순위 :  x:Name
control 의 이름을 최상단의 두는것으로 원하는 control을 바로 식별가능하게 한다.<br/>
## 2 순위 Grid : 좌표
전체 control의 리소스 좌표를 파악하기 쉽게 하기위해 grid좌표를 상단에 둔다.<br/>

## 3 순위 : Style
style 가 점점 표준화가 되어가면 이제 해당 style의 이름을 보고 해당control의 목적을 파악할수있게될것이다<br/>
따라서 Style도 상단에 둔다.<br/>

## 4 순위 : Color 
Color 를 최대한 StaticResource로 사용하여 관리한다.

## 5 순위 : 이벤트 관련
이 뒤부터는 거의 의미가없다.<br/>
## 6 순위 : 기타
나머지 데이터 속성 들 (Text , Placeholder 등)<br/>
## 맨뒤 
크기 관련 HorizontalOptions , WidthRequest ,Margin 속성을 맨뒤에 두자.<br/>

예시
```
<Entry x:Name="IdValue"  Style="{StaticResource EntryInput}" TextChanged="Id_Text_Change"  Placeholder="  아이디" HorizontalOptions="Fill" VerticalOptions="Center" >
<Button x:Name="Login_Button" Grid.Row="2" Style="{StaticResource ButtonNotEnabled}" Clicked="_LoginButton" Text="로그인" Padding="0"  WidthRequest="240" HeightRequest="40">
```

