---
published: true
permalink: /csharp/website-to-webapplication
comments: true
categories:
  - csharp
tags:
  - entityframework
  - session
title: '[C#] ASP.NET MVC Part3 : 회원가입, 로그인, 로그오프'
---
[개발토끼 youtube](https://www.youtube.com/channel/UCuuyTE8bBNYCujb7mzf8H7w)의 ASP.NET MVC 시리즈를 보고 스터디한 경험을 기록하였습니다.

## EntityFramework Core로 데이터베이스 생성하기 
(1) 순서 
  - EntityFramework 설치 : 아래 Nuget 패키지 설치  
    - Microsoft.EntityFramework.Core
    - Microsoft.EntityFrameworkCore.SqlServer
    - Microsoft.EntityFrameworkCore.Tools
  - Model Class 추가 
    ```c#
    public class User
    {
        public int UserNo { get; set; }
        public string UserName { get; set; }
        public string UserId { get; set; }
        public string UserPassword { get; set; }
    }
	```  
    
    ```c#
	public class Note
    {
        /// <summary>
        /// 게시글 번호 
        /// </summary>
        [Key]
        public int NoteNo { get; set; }

        /// <summary>
        /// 게시글 제목
        /// </summary>
        [Required]
        public string NoteTitle { get; set; }

        /// <summary>
        /// 게시글 내용 
        /// </summary>
        [Required]
        public string NoteCotents { get; set; }

        /// <summary>
        /// 사용자 번호 
        /// </summary>
        [Required]
        public int UserNo { get; set; }

        [ForeignKey("UserNo")] // 외래키, virtual은 lazy loading
        public virtual User User { get; set; }
    }  

 	```
  
  - DBContext  
  ```c#
  public class AspNoteDbContext : DbContext
  {
        public DbSet<User> Users { get; set;}
        public DbSet<Note> Notes { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            optionsBuilder.UseSqlServer(@"{connectionString}");
        }

  }  

  ```  
  
  - 실제 테이블 추가 (Nuget Package Console)
    - add-migration
    - update-database 
    
    ![step1.png]({{site.baseurl}}/assets/images/csharp/asp-mvc-part3-step1.png){: width="=60%"}  
   
   => 결과   

  ![step2.png]({{site.baseurl}}/assets/images/csharp/asp-mvc-part3-step2.png){: width="=60%"}  




## 회원가입 만들기  
(1) Controller 
  ```c#
    [HttpPost]
    public IActionResult Register(User model)
    {
        // ModelState.IsValid : Model에 정의한 값 
        if (ModelState.IsValid)
        {
            using (var db = new AspNoteDbContext())
            {
                db.Users.Add(model); // Memory
                db.SaveChanges(); // Database 
            }
            return RedirectToAction("Index", "Home");
        }

        return View(model);
    }
  ```

(2) View 
```html
<form class="form-horizontal" method="post" asp-controller="Account" asp-action="Register">
    <div class="form-group">
        <label>사용자 ID</label>
        <input type="text" class="form-control" asp-for="UserId" placeholder="사용자 ID 입력" />
        <span class="text-danger" asp-validation-for="UserId"></span>
    </div>

    <div class="form-group">
        <label>사용자 비밀번호</label>
        <input type="password" class="form-control" asp-for="UserPassword" placeholder="사용자 비밀번호 입력" />
        <span class="text-danger" asp-validation-for="UserPassword"></span>
    </div>

    <div class="form-group">
        <label>사용자 이름</label>
        <input type="text" class="form-control" asp-for="UserName" placeholder="사용자 이름 입력" />
        <span class="text-danger" asp-validation-for="UserName"></span>

    </div>

    <div class="form-group">
        <button type="SUBMIT" class="btn btn-primary">회원가입</button>
        <a class="btn btn-warning" href="#">취소</a>       
    </div>
</form>
```

=> 결과 
![step3.png]({{site.baseurl}}/assets/images/csharp/asp-mvc-part3-step3.png){: width="=60%"}  


## 로그인 만들기 
(1) 로그인 2가지 방법론 
  - Session 
    - 웹서버가 사용자 정보를 메모리에 담아두는 것 
    - 장점 : 보안성이 높다. 
    - 단점 : 웹 서버 메모리 부하가 높아진다. 
  - Cookie 
    - 웹 서버 로그인 -> 사용자 정보를 브라우저에 전달 -> 쿠키 
    - 장점 : 웹 서버 부하가 낮아진다. 
    - 단점 : 보안성이 낮다. 
    
(2) ViewModel
  - 로그인 시에는 `사용자 이름`이 필수 입력이 아니기 때문에 로그인 시에만 사용하는 Model이 별도 필요. 
  ```c#
    public class LoginViewModel
    {
        /// <summary>
        /// 사용자 ID
        /// </summary>
        [Required(ErrorMessage = "사용자 ID를 입력하세요.")] // NOT Null 설정 
        public string UserId { get; set; }

        /// <summary>
        /// 사용자 비밀번호 
        /// </summary>
        [Required(ErrorMessage = "사용자 비밀번호를 입력하세요.")] // NOT Null 설정 
        public string UserPassword { get; set; }
    }
  ```
  
    
(3) Controller 
  ```c#
    [HttpPost]
    public IActionResult Login(LoginViewModel model)
    {
        if (ModelState.IsValid)
        {
            using (var db = new AspNoteDbContext())
            {
                // Linq - 메서드 체이닝 
                // => : A Go to B 
                // var user = db.Users
                //     .FirstOrDefault(u => u.UserId == model.UserId && u.UserPassword == model.UserPassword);

                // == : Memory 누수가 발생하기 때문에 equals를 활용해야 함. 
                var user = db.Users
                    .FirstOrDefault(u => u.UserId.Equals(model.UserId) && u.UserPassword.Equals(model.UserPassword));

                if (user != null)
                {
                    // HttpContext.Session.SetInt32(key, value); - key: 식별자 
                    HttpContext.Session.SetInt32("USER_LOGIN_KEY", user.UserNo);

                    // 로그인에 성공 
                    return RedirectToAction("LoginSuccess", "Home");
                }
            }
        }

        // 로그인에 실패
        ModelState.AddModelError(string.Empty, "사용자 ID 혹은 비밀번호가 올바르지 않습니다.");
                
        return View(model);
    }
  ```
  
  (4) View 
   ```html
   <form class="form-horizontal" method="post" asp-controller="Account" asp-action="Login">
    <div class="text-danger" asp-validation-summary="ModelOnly"></div>
    <div class="form-group">
        <label>사용자 ID</label>
        <input type="text" class="form-control" asp-for="UserId" placeholder="사용자 ID 입력" />
        <span class="text-danger" asp-validation-for="UserId"></span>
    </div>

    <div class="form-group">
        <label>사용자 비밀번호</label>
        <input type="password" class="form-control" asp-for="UserPassword" placeholder="사용자 비밀번호 입력" />
        <span class="text-danger" asp-validation-for="UserPassword"></span>
    </div>

    <div class="form-group">
        <button type="SUBMIT" class="btn btn-primary">로그인</button>
        <a class="btn btn-warning" href="#">취소</a>
    </div>
</form>
   ```
=> 결과 
![step4.png]({{site.baseurl}}/assets/images/csharp/asp-mvc-part3-step4.png){: width="=60%"}    
![step5.png]({{site.baseurl}}/assets/images/csharp/asp-mvc-part3-step5.png){: width="=60%"}  


   
   
   
   
 [참고] Session 미들웨어 추가 
 ```c#
    // This method gets called by the runtime. Use this method to add services to the container.
    public void ConfigureServices(IServiceCollection services)
    {
        // DI 의존성 주입 - ASP.NET MVC 4,5에서는 Unity Nuget Package 설치가 필요했었음 
        // Session - 서비스에 등록함
        services.AddSession();

        // Identity
        // Web API 관련 기능
    }

    // This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseSession(); // Application에서 사용하겠다. 

    }
 ```


## 참고문서 
- [EntityFramework Core로 데이터베이스 생성하기](https://www.youtube.com/watch?v=MNmcTeEv07A&t=4s)
- [회원가입 만들기](https://www.youtube.com/watch?v=negMazMl7WQ)
- [로그인 만들기 ](https://www.youtube.com/watch?v=492E2t0gxEg)
- [Session 미들웨어 추가](https://www.youtube.com/watch?v=G5AxF9pt4Pw)