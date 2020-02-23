---
published: false
permalink: /csharp/asp-mvc-part1
comments: true
categories:
  - csharp
  - razor
title: '[C#] ASP.NET MVC Part1 : ASP.NET 기초와 Razor문법'
---
[개발토끼 youtube](https://www.youtube.com/channel/UCuuyTE8bBNYCujb7mzf8H7w)의 ASP.NET MVC 시리즈를 보고 스터디한 경험을 기록하였습니다.


## ASP.NET 
(1) Web Form
  - Winform 
  - 웹페이지 내에 소스 코드 존재할 수 있다.   
⇒ 유지보수 굉장히 어려움.   

(2) ASP.NET MVC
  - View → html + css + javascript (front-end dev)
  - Controller → DB 통신, 기타, 계산 (back-end dev)
  - Model → User   
  
(3) SignalR
  - 실시간 채팅 서비스 
    
(4) Web API   
- 데이터베이스에서 나온 정보를 XML, JSON 형식 송출해주는 서비스   
  1) StateLess : 커넥션을 항상 잡고 있지 않음   
  2) 모든 플랫폼 통신이 가능.   
⇒ java spring ajax  
⇒ wpf winform javafx → 윈도우 프로그램에도 적용  
⇒ 안드로이드, IOS 앱과 통신 가능    
  
    
      
        
        

## ASP.NET와 ASP.NET Core 
(1) 공통점 : 기능상 모두 비슷함
(2) 차이점 : 
- ASP.NET : window 
  - System.Net.XXXX 사용 
- ASP.NET Core : linux, max, window 
  - System.Net.XXXX 사용하지 않음 => 속도 차이가 남. 
(3) 버전 
- ASP.NET 4.6.1 - ASP.NET MVC 5
- ASP.NET 5 - ASP.NET MVC 6 OR Core  
  
    
      
        
        

## Razor Syntax
- `@` 사용 
```c#
@{
  var name = "홍길동";
  var age = 10; 
}
<h1>@name 님 환영합니다. </h1>
```

- `if`, `for`, `foreach` 가능
```c#
@if (num ==10)
{
  <h1>@name 님은 @age살입니다. </h2>
}
else
{
  <h2>@name 님 나이를 알 수 없습니다.</h2>
}
```

- (string) -> (int), ToString() 가능
```
@for (var index = 1; index < 10; index++)
{
  <h3>@index 번째 입니다.</h3>
}

@foreach (var num in list)
{
}
```
  
  
    
      
        
        
## Controller에서 View로 데이터 전달하기 
(1) 첫 번째 방법 
```c#
public IActionResult Index()
{
	var hongUser = new User
	{
		UserNo = 1,
		UserName = "홍길동"
	};
	return View(hongUser);
}
```  

```html
<h1>사용자번호 : @Model.UserNo</h1>
<h1>사용자이름 : @Model.UserName</h1>
```


(2) 두 번째 방법 
```c#
public IActionResult Index()
{
	var hongUser = new User
	{
		UserNo = 1,
		UserName = "홍길동"
	};

	ViewBag.User = hongUser; 

	return View();
}
```  

```html
<h1>사용자번호 : @ViewBag.User.UserNo</h1>
<h1>사용자이름 : @ViewBag.User.UserName</h1>
```


(3) 세 번째 방법 
```c#
public IActionResult Index()
{
	var hongUser = new User
	{
		UserNo = 1,
		UserName = "홍길동"
	};

	ViewData["UserNo"] = hongUser.UserNo;
  ViewData["UserName"] = hongUser.UserName;

	return View();
}
```  

```html
<h1>사용자번호 : @ViewData[UserNo"]</h1>
<h1>사용자이름 : @ViewData[UserName"]</h1>
```





## 참고문서 
- [ASP.NET](https://www.youtube.com/watch?v=Y_X4A0P06Os)
- [ASP.NET와 ASP.NET Core](https://www.youtube.com/watch?v=Y_X4A0P06Os)
- [Razor Syntax](https://www.youtube.com/watch?v=GRHy0FgrJrw)
- [Controller에서 View로 데이터 전달하기](https://www.youtube.com/watch?v=TTQW2ou3w7c)

