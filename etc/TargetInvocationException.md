# TargetInvocationException

## 상황
ResourceDictionary 정리중 디버깅을 하기위해 실행하니 발생


## 메시지 명
```
System.Reflection.TargetInvocationException: 'Exception has been thrown by the target of an invocation.'
```
## 자세히 보기
```
-		$exception	{System.Reflection.TargetInvocationException: Exception has been thrown by the target of an invocation. ---> Xamarin.Forms.Xaml.XamlParseException: Position 22:44. StaticResource not found for key MainColor
  at Xamarin.Forms.Xaml.StaticResourceExtension.ProvideValue (System.IServiceProvider serviceProvider) [0x0008f] in D:\a\1\s\Xamarin.Forms.Xaml\MarkupExtensions\StaticResourceExtension.cs:27 
  at __XamlGeneratedCode__.__Typeccf6c6db52404697a80de9c4bd635b0f.InitializeComponent () [0x00012] in C:\Users\qoom\Desktop\k4keye\sellercheck\Xamarin\Todo\obj\Debug\netstandard2.1\Resources\Dictionary\StyleDictionary.xaml.g.cs:29 
  at __XamlGeneratedCode__.__Typeccf6c6db52404697a80de9c4bd635b0f..ctor () [0x00008] in C:\Users\qoom\Desktop\k4keye\sellercheck\Xamarin\Todo\obj\Debug\netstandard2.1\Resources\Dictionary\StyleDictionary.xaml.g.cs:23 
  at (wrapper managed-to-native) System.Reflection.RuntimeConstructorInfo.InternalInvoke(System.Reflection.RuntimeConstructorInfo,object,object[],System.Exception&)
  at System.Reflection.RuntimeConstructorInfo.InternalInvoke (System.Object obj, System.Object[] parameters, System.Boolean wrapExceptions) [0x00005] in /Users/builder/jenkins/workspace/archive-mono/2020-02/android/release/mcs/class/corlib/System.Reflection/RuntimeMethodInfo.cs:936 
   --- End of inner exception stack trace ---
  at System.Reflection.RuntimeConstructorInfo.InternalInvoke (System.Object obj, System.Object[] parameters, System.Boolean wrapExceptions) [0x00018] in /Users/builder/jenkins/workspace/archive-mono/2020-02/android/release/mcs/class/corlib/System.Reflection/RuntimeMethodInfo.cs:944 
  at System.RuntimeType.CreateInstanceMono (System.Boolean nonPublic, System.Boolean wrapExceptions) [0x00095] in /Users/builder/jenkins/workspace/archive-mono/2020-02/android/release/mcs/class/corlib/ReferenceSources/RuntimeType.cs:185 
  at System.RuntimeType.CreateInstanceSlow (System.Boolean publicOnly, System.Boolean wrapExceptions, System.Boolean skipCheckThis, System.Boolean fillCache) [0x00009] in /Users/builder/jenkins/workspace/archive-mono/2020-02/android/release/mcs/class/corlib/ReferenceSources/RuntimeType.cs:155 
  at System.RuntimeType.CreateInstanceDefaultCtor (System.Boolean publicOnly, System.Boolean skipCheckThis, System.Boolean fillCache, System.Boolean wrapExceptions, System.Threading.StackCrawlMark& stackMark) [0x00027] in /Users/builder/jenkins/workspace/archive-mono/2020-02/android/release/mcs/class/referencesource/mscorlib/system/rttype.cs:5770 
  at System.Activator.CreateInstance (System.Type type, System.Boolean nonPublic, System.Boolean wrapExceptions) [0x00039] in /Users/builder/jenkins/workspace/archive-mono/2020-02/android/release/mcs/class/referencesource/mscorlib/system/activator.cs:206 
  at System.Activator.CreateInstance (System.Type type, System.Boolean nonPublic) [0x00000] in /Users/builder/jenkins/workspace/archive-mono/2020-02/android/release/mcs/class/referencesource/mscorlib/system/activator.cs:190 
  at System.Activator.CreateInstance (System.Type type) [0x00000] in /Users/builder/jenkins/workspace/archive-mono/2020-02/android/release/mcs/class/referencesource/mscorlib/system/activator.cs:134 
  at Xamarin.Forms.ResourceDictionary+<>c.<SetAndLoadSource>b__11_0 (System.Type key) [0x00000] in D:\a\1\s\Xamarin.Forms.Core\ResourceDictionary.cs:74 
  at System.Runtime.CompilerServices.ConditionalWeakTable`2[TKey,TValue].GetValue (TKey key, System.Runtime.CompilerServices.ConditionalWeakTable`2+CreateValueCallback[TKey,TValue] createValueCallback) [0x00033] in /Users/builder/jenkins/workspace/archive-mono/2020-02/android/release/mcs/class/corlib/System.Runtime.CompilerServices/ConditionalWeakTable.cs:322 
  at Xamarin.Forms.ResourceDictionary.SetAndLoadSource (System.Uri value, System.String resourcePath, System.Reflection.Assembly assembly, System.Xml.IXmlLineInfo lineInfo) [0x00031] in D:\a\1\s\Xamarin.Forms.Core\ResourceDictionary.cs:74 
  at SellerCheck.App.InitializeComponent () [0x00012] in C:\Users\qoom\Desktop\k4keye\sellercheck\Xamarin\Todo\obj\Debug\netstandard2.1\App.xaml.g.cs:22 
  at SellerCheck.App..ctor () [0x00008] in C:\Users\qoom\Desktop\k4keye\sellercheck\Xamarin\Todo\App.xaml.cs:37 
  at SellerCheck.MainActivity.OnCreate (Android.OS.Bundle bundle) [0x001da] in C:\Users\qoom\Desktop\k4keye\sellercheck\Xamarin\Todo.Android\MainActivity.cs:98 
  at Android.App.Activity.n_OnCreate_Landroid_os_Bundle_ (System.IntPtr jnienv, System.IntPtr native__this, System.IntPtr native_savedInstanceState) [0x0000f] in <84ca7e914f6148f0b961431a9ac4287b>:0 
  at (wrapper dynamic-method) Android.Runtime.DynamicMethodNameCounter.8(intptr,intptr,intptr)}	System.Reflection.TargetInvocationException

```

## 분석
트레이스 를 보면 <br/>
App 의 생성자에서 <br/>
InitializeComponent() 즉 control들을 초기화 하는중에 <br/>
ResourceDictionary 를 호출하였고 <br/> 
에러가 발생한것으로 보인다. <br/> <br/>

즉 해당 ResourceDictionary 를 찾을수없어 리플레쉬를 하다가 문제가 발생한것.
<br/>
# 해결
사용중인 ResourceDictionary 는 <br/>
ColorDictionary , StyleDictionary 가있는데 <br/>
ColorDictionary 중 하나의 Color 의 Key를 변경하였는데<br/>
이를 사용하고있던 StyleDictionary 에서 Key를 변경하지않음.


