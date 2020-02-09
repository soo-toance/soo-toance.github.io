---
published: false
permalink: /csharp/website-to-webapplication
comments: true
categories:
  - csharp
title: '[C#] WebSite -> WebApplication'
---
WebSite Project(WSP)를 Web Application Project(WAP)로 변환했던 경험을 기록하였습니다.    
  
    
    
  
## 계기 
---
[마이크로소프트 공식 문서](https://docs.microsoft.com/ko-kr/previous-versions/dd547590(v=vs.100))에 따르면 **WAP**가 **WSP**보다 무조건 성능이 좋은 Project는 아닙니다.   
하지만, **WSP**는 실시간으로 컴파일이 이루어져 테스트코드 작성에 어려운 부분들이 있어서 테스트코드를 작성하기 위해서라도 **WAP**로 전환이 필요해보였습니다.   




## 방법 
---
변환하는 방법은 이 [문서](https://devblogs.microsoft.com/aspnet/converting-a-web-site-project-to-a-web-application-project/)를 참고했습니다. 

### Step1 : WebSite Project 정리
1, WebSite Project를 열기 
![Step1_1.png]({{site.baseurl}}/_posts/csharp/Step1_1.png)  

2. .sln파일로 저장 (Ctrl + shift + s) 후 빌드 (Ctrl + shift + b)
![Step1_2.png]({{site.baseurl}}/_posts/csharp/Step1_2.png)

3. 빌드 후 에러가 발생하지 않도록 기능 수정 
  - 경고로 나오는 메시지들은 이 단계에서는 일단 무시해도 괜찮음. 



### Step2 : 새로운 WebApplication Project 추가 
1. Step1의 프로젝트에서 다시 새로운 Web Application Project 추가 
  ![Step2_1.png]({{site.baseurl}}/_posts/csharp/Step2_1.png)  

  ![Step2_2.png]({{site.baseurl}}/_posts/csharp/Step2_2.png)

2. '빈 프로젝트' 및 테스트 프로젝트 추가를 선택 후 생성 
![Step2_3.png]({{site.baseurl}}/_posts/csharp/Step2_3.png)

3. 생성된 Web.config 삭제 



### Step3 : WebApplication으로 이관 
1. WebSite Project의 App_Code를 WebApplication으로 copy&paste 후 웹 응용프로그램으로 변환 후 빌드 이루어지는 지 확인 
![Step3_1.png]({{site.baseurl}}/_posts/csharp/Step3_1.png)

3. 나머지 파일들도 순차적으로 copy&paste 후 웹 응용프로그램으로 변환
- 전체 copy & paste 후 확인하지 말고 일부 copy & paste 후 확인하는 것이 좋음. 
    
      

### Step4 : 변환한 WebApplication 동작 점검! 
  
  
## 참고문서 
---
- [Web Application Projects versus Web Site Projects](https://docs.microsoft.com/ko-kr/previous-versions/dd547590(v=vs.100))
- [unit-testing-asp-net-web-site-project-code-stored-in-app-code](https://stackoverflow.com/questions/1198555/unit-testing-asp-net-web-site-project-code-stored-in-app-code)
- [converting-a-web-site-project-to-a-web-application-project](https://devblogs.microsoft.com/aspnet/converting-a-web-site-project-to-a-web-application-project/)
