# Android Control

안드로이드의 기본적인 Control 설명서<br/>



# Context 

안드로이드의 기본적인 App,service,view등을 상속받는 클래스에선 Context를 요구할시 this로 context를 설정한다(activity Context).<br/>

컨트롤러 인터페이스등에서의 클래스에선 application의 context(Application Context)로 설정해줘야한다.
```
Android.App.Application.Context
```

# SystemService

안드로이드의 기본적인 서비스 즉 NotificationManager나 AlaramService등을 컨트롤하기위해서 해당 시스탬핸들러를 가져와야하는데
기본적으로 라이브러리로 가춰지지않기때문에 직접들고와야한다.

```
var setName = (You need Class)Android.App.Application.Context.GetSystemService(Android.Context.Context.You need Service)

```


# Intent

엑티비티 이동간 사용할수있으나 엑티비티를넘어선 어플리케이션단의 intent를 사용시엔 어플리캐이션으로 지정해줘야한다.

```
var intent = new intent (application Context or Activity Context,type  Ex: typeof(Activity))

```
