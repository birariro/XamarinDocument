# ControlEvent만들기
## GestureRecognizers 

Xamarin에서 각종 Control,  예로 Label .Image 등 클릭에 관련 이벤트를 사용해야 하는 경우가 종종 생기는데
button 과같이 당연히 이벤트가 발생할듯한 요소들은 기본적으로 이벤트를 제공하는 것과 다르게
그렇지 않은 요소들은 이벤트를 제공해 주지 않는다.

따라서 이러한 요소들은 GestureRecognizers를 이용하여 제스처에 대한 인식기를 만들어
직접 처리해야 한다.

GestureRecognizers에서 제공하는 이벤트로는
탭(클릭) ,이동, 밀기 등이 가능하다.
지금 필요한 것은 탭 이벤트이다.

```
<StackLayout >
	<Entry/>
	<Frame>
		<Label/>
	</Frame>
	<Label/>
</StackLayout>
```
위의 UI xaml 에서 StackLayout 에 Tap 이벤트를 만들어볼것이다.
```
<StackLayout >
	<StackLayout.GestureRecognizers>
		<TapGestureRecognizer Tapped="TapGestureRecognizer_Tapped"/>
	</StackLayout.GestureRecognizers>
	<Entry/>
	<Frame>
		<Label/>
	</Frame>
	<Label/>
</StackLayout>
```
<ControlName.GestureRecognizers> 를 사용하고
그 안에 원하는 제스쳐를 찾아 넣는다. 
이번에 할 제스쳐는 Tap 이기때문에 <TapGestureRecognizer> 로 작성
