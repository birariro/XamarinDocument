# CodeSnippet

코드 조각 은 코드를 신속하게 삽입할수있도록 준비된 코드 모음이라고할수있다. 코드조각 동작 키는 Tap 키 두번을 누르면된다..<br/>
프로젝트에 구속되는것이아닌 VS자체에 구속되는것이기 때문에 활용도도 놉다.<br/>


# 1. 내장 CodeSnippet
기본적으로 VS에 내장되어있는 코드조각중 유용하게 사용하는 것들을 기록하려한다.<br/>

## for,foreach
```
for (int i = 0; i < length; i++)
{

}
foreach (var item in collection)
{

}
```
반복문의 형태를 자동으로 완성한다.<br/>

## #if 

```
#if true

#endif
```
#if 와 #endif를 완성한다.<br/>

## propfull
```
private int myVar;

public int MyProperty
{
	get { return myVar; }
	set { myVar = value; }
}
```
프로퍼티를 완성해준다.<br/>

## cw
```
Console.WriteLine();
```
화면 출력 WriteLine을 완성해준다.




# 2. Custom Code Snippet
위에서본 내장 코드조각 말고도 커스텀 코드조각을 만들수있다.<br/>
```
<?xml version="1.0" encoding="utf-8"?>
<CodeSnippets
    xmlns="http://schemas.microsoft.com/VisualStudio/2005/CodeSnippet">
    <CodeSnippet Format="1.0.0">
        <Header>
            <Title></Title>
            <Author></Author>
            <Description></Description>
            <Shortcut></Shortcut>
        </Header>
        <Snippet>
            <Code Language="csharp">
                <![CDATA[]]>
            </Code>
        </Snippet>
    </CodeSnippet>
</CodeSnippets>
```
기본적인 틀은 이렇다<br/>

Title: 코드조각편집기에서 나오는 이 snippet의 이름<br/>
Author: 제작자이름<br/>
Description: snippet의 설명<br/>
Shortcut: intellicode에서 불러올이름<br/>
Code Language 어느 언어에서 사용할지 설정<br/>


## 매개 변수 설정
변수값을 사용해야할경우. 따로설정을 하지않으면 Default값이 들어간다.

```
  <Declarations>
    <Literal>
      <ID></ID>
      <ToolTip></ToolTip>
      <Default></Default> 
    </Literal>
  </Declarations>
```
ID: 변수이름<br/>
ToolTip: 변수에대한설명<br/>
Default: 기본값<br/>

## Snippet Function
함수를 사용해야할 경우. 따로설정 하지않으면 Default값이 들어간다.

```
        <Literal default="" Editable=""> //true or false
                    <ID></ID>
                    <ToolTip></ToolTip>
                    <Function></Function>
                    <Default></Default>
                </Literal>
```
Literal default: 기본값을허용할지 말지대한boolean<br/>
Literal Editable: 편집을허용할지 말지대한boolean<br/>
ID: 함수이름<br/>
ToolTip: 함수에대한설명<br/>
Default: 기본값<br/>
Function<br/>
GenerateSwitchCases(EnumerationLiteral) switch문과 case문의 집합생성 (Ex:GenerateSwitchCases($expression$))<br/>
ClassName() 삽입된 코드 조각을 포함하는 클래스의 이름을 반환 (Ex:ClassName())<br/>
SimpleTypeName(TypeName) 코드 조각이 호출되는 컨텍스트에서 가장 단순한 형태로 변환(Ex:SimpleTypeName(global::System.Exception))<br/>
