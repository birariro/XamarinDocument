# DependencyService 
Xamarin.forms 에서 Android의 자원이 필요한경우가 있다.<br/>
DependencyService 는 공유 코드(PCL) 에서 네이티브 플렛폼 기능을 호출할수있도록 하는 서비스 이다. <br/>
예로 이미지 , 소리 , html 파일등 Android의 res 나 asset의 자원을 사용해야하는경우 <br/>
인터페이스를 통해 얻어올수있다. <br/>


개발 사이클로는 
1. Xamarin.forms 와 Android의 소통을 위한 인터페이스를 PCL 에 생성
2. Android 는 1에서 만들어진 인터페이스를 구현
3. PCL 에서 해당 인터페이스를 호출하는것으로 원하는 데이터를 요청

## 요구사항
Xamarin.forms 에서 WebView 띄울것인데 띄우는 대상으 Android 에 assets 디렉토리 밑에있는 html 파일을 띄울것이다.
<br/><br/>

## 1. 인터페이스 생성
```
public interface IHtmlUrl
{
	string Get();
}

```
해당 인테페이스는 PCL 에서 생성한다. <br/><br/>


## 2. 인터페이스 구현
```
[assembly: Dependency(typeof(HtmlUri))]
namespace IamportApp.Droid
{
    class HtmlUri : IHtmlUrl
    {
        public string Get()
        {
            return "file:///android_asset/";
        }
    }
}
```
위의 코드중 중요한 부분은
```
[assembly: Dependency(typeof(HtmlUri))]
```
위의 부분으로 플렛폼에서 인터페이스를 구현한것을 DependencyService에 등록해서 <br/>
Xamarin.forms가 런타임에 해당 구현을 찾을수있도록한다. <br/><br/>

## 3. 인터페이스 호출
```
public partial class MainPage : ContentPage
{
	public MainPage()
	{
		InitializeComponent();
    
		string url = DependencyService.Get<IHtmlUrl>().Get();
		var source = new UrlWebViewSource
		{
			Url = System.IO.Path.Combine(url, "test.html")
		};
		webView.Source = source;

	}
}
```
