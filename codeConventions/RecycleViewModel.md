# RecycleVM

여러가지 View를 관리하는 ViewModel은 기본적인 Rule 과ICommand마다 지켜야하는 Rule이 있어야한다.
```
 public ICommand IClickCommand { get; }


        public LoginViewModel()
        {
            IClickCommand = new Command<object>((e) => Login(e));
        } 
```

예를들어 LoginViewModel안에 IClickCommand가 있고 그것을 여러가지 로그인뷰에서 사용할시 View는 Vm에 룰에 따라줘야한다. 안그럼 재활용에 문제가생긴다.

```        
// MVVM  Login
// Rule
// 1. x:name 과 classid 는 일치해야한다.
// 2. commandparameter는 항상 mainGrid를 넘겨야한다.
// 3. mainGrid,DataGrid 가 항상 존재해야한다.
// 4. DataGrid이후 stackLayout이없이 단일로 Entry나 Box를 사용해야한다.
// 4-1. stackLayout을 사용해야할경우 mainGrid에서 해당하는 stacklayout을 children에서 찾은후 다시 해당레이아웃의 children에서 찾으면된다. 
```

이렇게 기본적인 Rule을 만들고 하나의 command에 새로운 Rule을 만드는것이 좋다.






Command로 받은 데이터의 가공
Vm은 View를 모른다. 따라 뷰에 어떤게 있고 그 뷰에 접근할수가 없다.
View에 Action이들어오면 Command로 받아 기본적인 Rule에 따라 Obj을 변형하여 처리한다.
```
   private void Login(object sendder)
        {   
            var mainGrid = sendder as Grid;
            var dataGrid = mainGrid.Children.Where(x => x.ClassId == "DataGrid").FirstOrDefault() as Grid;


            var idEntry = dataGrid.Children.Where(x => x.ClassId== "IDEntry").FirstOrDefault();
            var pwdEntry = dataGrid.Children.Where(x => x.ClassId== "PWDEntry").FirstOrDefault(); 

            string idvalue = (string)idEntry.GetValue(CustomColorEntry.TextProperty);
            string pwdvalue = (string)pwdEntry.GetValue(CustomColorEntry.TextProperty); 
        }

```
Rule 2에 따라 첫 obj는 MainGrid로 변형
Rule 1에 따라 Vm은 View의 x:name을 모르기때문에 Classid 로 접근한다.
Rule 3에 따라 넘겨온 Data를 받기위해 DataGrid로 변형
Rule 4에 따라 바로 Entry에 접근한다.

1. 원하는 view의 Children을 찾을땐 Linq로 사용하는것이 편하다.
2. Children의 값을 가져올땐 GetValue => Property를 이용해서 가져오는것이 정석이다.


