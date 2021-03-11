# StackTrace

### 1. ctor

```
log=System.ArgumentOutOfRangeException: startIndex cannot be larger than length of string.
Parameter name: startIndex
  at System.String.Substring (System.Int32 startIndex, System.Int32 length) [0x0001d] in <7fe9c2dffdd2410b9237f9d439a51a69>:0 
  at app.WorkClass.WorkProcessing..ctor () [0x0001c] in <7cf8f61d65634889ac34b9f786cdaaac>:0 
  at app.Sellpage.Sellpage.AddItems (System.Boolean refreshFlag) [0x00030] in <7cf8f61d65634889ac34b9f786cdaaac>:0 
```
위의 app.WorkClass.WorkProcessing..ctor () 에서의 ctor은 constructor 의 약자로 생성자를 의미한다.
즉 생성자에서 subString 를 호출하다가 Exception이 발생한것
