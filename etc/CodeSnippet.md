# CodeSnippet

코드 조각을 사용하여 구문을 좀더 쉽게 사용할수있다.
프로젝트에 구속되는것이아닌 vs자체에 구속되는것이기 때문에 활용도도 놉다.

# Custom Code Snippet
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
기본적인 틀은 이렇다

Title: 코드조각편집기에서 나오는 이 snippet의 이름
Author: 제작자이름
Description: snippet의 설명
Shortcut: intellicode에서 불러올이름
Code Language 어느 언어에서 사용할지 설정


# 매개 변수 설정
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
ID: 변수이름
ToolTip: 변수에대한설명
Default: 기본값

# Snippet Function
함수를 사용해야할 경우. 따로설정 하지않으면 Default값이 들어간다.

```
        <Literal default="" Editable=""> //true or false
                    <ID></ID>
                    <ToolTip></ToolTip>
                    <Function></Function>
                    <Default></Default>
                </Literal>
```
Literal default: 기본값을허용할지 말지대한boolean
Literal default: 편집을허용할지 말지대한boolean
ID: 함수이름
ToolTip: 함수에대한설명
Default: 기본값
Function
GenerateSwitchCases(EnumerationLiteral) switch문과 case문의 집합생성 (Ex:GenerateSwitchCases($expression$))
ClassName() 삽입된 코드 조각을 포함하는 클래스의 이름을 반환 (Ex:ClassName())
SimpleTypeName(TypeName) 코드 조각이 호출되는 컨텍스트에서 가장 단순한 형태로 변환(Ex:SimpleTypeName(global::System.Exception))
