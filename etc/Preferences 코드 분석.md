# 개요
 Preferences 에 데이터를 저장하고 수정하고 삭제하는 부분에서 원활한 동작을 하지않는 경우가발생.
 예 clear 을 하고 다시 데이터를 얻어와봤는데 데이터가 살아있다.
 
 # 원인파악
 xamarin/Essentials 라이브러리를 사용하기때문에 코드 분석을 진행 <br/>
 [Essentials Git Url](https://github.com/xamarin/Essentials) <br/>
 
 ## 메소드 원형
 ```C#
 public static void Set(string key, string value) =>
            Set(key, value, null);
            
        public static void Set(string key, string value, string sharedName) =>
            PlatformSet<string>(key, value, sharedName);

 ```
 GET ,SET 메소드에 파라미터 2개 와 3개넘기는 메소드가 존재하고<br/>
 2개를 넘기면 마지막 파라미터에 null을 넣어서 결국<br/>
 PlatformGet,Set<T> 에게 전달한다.<br/>
  
  해당 메소드는 각 플랫폼에 맞춰서 호출되게 되는데<br/>
  여기서는 android를 볼것이다.<br/>
  
  ```C# 
        static void PlatformSet<T>(string key, T value, string sharedName)
        {
            lock (locker)
            {
                using (var sharedPreferences = GetSharedPreferences(sharedName))
                using (var editor = sharedPreferences.Edit())
                {
                    if (value == null)
                    {
                        editor.Remove(key);
                    }
                    else
                    {
                        switch (value)
                        {
                            case string s:
                                editor.PutString(key, s);
                                break;
                            case int i:
                                editor.PutInt(key, i);
                                break;
                            case bool b:
                                editor.PutBoolean(key, b);
                                break;
                            case long l:
                                editor.PutLong(key, l);
                                break;
                            case double d:
                                var valueString = Convert.ToString(value, CultureInfo.InvariantCulture);
                                editor.PutString(key, valueString);
                                break;
                            case float f:
                                editor.PutFloat(key, f);
                                break;
                        }
                    }
                    editor.Apply();
                }
            }
        }
  ```
  저장할때 value가 null 이면 해당 key에 대한 내용을 제거하는것을 볼수있다.<br/>
  어차피 null이니까 제거하는듯 하다.<br/>
  
  value의 자료형에 맞춰서 sharedPreferences의 메소드를 활용하여 갱신하는 모습을 볼수있고<br/>
  마지막에 Apply를 하여 저장하게된다<br/>
  
  Apply와 Commit 의 차이는<br/>
  Apply는 호출후 곧바로 리턴되어 스레드를 block 시키지않고<br/>
  Commit 는 스레드를 block 시키고 저장완료후 true/false 를 반환한다.<br/><br/><br/>
  
  
  ```C#
       static ISharedPreferences GetSharedPreferences(string sharedName)
        {
            var context = Application.Context;

            return string.IsNullOrWhiteSpace(sharedName) ?
#pragma warning disable CS0618 // Type or member is obsolete
                PreferenceManager.GetDefaultSharedPreferences(context) :
#pragma warning restore CS0618 // Type or member is obsolete
                    context.GetSharedPreferences(sharedName, FileCreationMode.Private);
        }
  ```
  sharedName의 경우 null을 넣게되면 GetDefaultSharedPreferences(Application.Context)를 통해 얻어오고<br/>
  null 이아니면 GetSharedPreferences() 를 통해 얻어온다.<br/>
  
  
  # 원인
  결국 정확히 알지는 못하였지만<br/>
  알수있었던것은 작업을 할때 lock 작업이 수행된다는것이다.<br/>
  즉 복수의 스레드가 동시접근을할경우 대기하는 시간이 걸리게될것이고<br/>
  이에따른 잘못된 데이터를 얻을수있는게 아닐까 한다.<br/>
