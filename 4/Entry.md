# Entry 

사용자에게 데이터를 입력받는 Control 중 하나인 Entry 의 속성을 정리한다.


1. IsPassword : 사용자 입력을 * 로 표시한다.(패스워드)
2. Placeholder : 입력란 위치에 힌트를 표시한다.
3. HorizontalTextAlignment : Text 혹은 힌트 의 위치를 지정한다 Entry 시작 , 가운데 , 끝
4. Completed : 입력이 끝났을때 발생하는 이벤트
    1. 입력후 엔터를 누르면 발생
    2. 다른곳을 클릭해서 포커스를 나가면 발생안함 (주의)
5. TextChanged : 입력값이 바뀌었을때 발생하는 이벤트
    1. e.OldTextValue : 바뀌기 전 값
    2. e.NewTextValue : 바뀐후 값
    3. TextChanged 이벤트 특성상 하나 변경할때마다 변경이되기때문에 온전히 바뀌기전 바뀐후를 판단할수없다.
6. Keyboard : 입력 유형을 지정한다.
    1. Numeric : 숫자와 . 만 입력할수있는 키보드가 나온다.
    2. 이 의외에도 Email , Url 등 다양하게있지만 사용하기에는 문제가 있다.
