---
published: false
permalink: /csharp/asp-mvc-part2
comments: true
categories:
  - csharp
  - razor
title: '[C#] ASP.NET MVC Part2 : EntityFramework'
---
[개발토끼 youtube](https://www.youtube.com/channel/UCuuyTE8bBNYCujb7mzf8H7w)의 ASP.NET MVC 시리즈를 보고 스터디한 경험을 기록하였습니다.


## C#의 Database통신 
(1) ADO.NET 
  - WinForm, Classic ASP
  ⇒ 쿼리문 직접 작성 하게 되어 쿼리문 오류 디버깅이 어려움
  
(2) Enterprise Library 
  - Query문을 직접 작성 ⇒ 값을 처리
  - Logging
  ⇒ 쿼리문 직접 작성 하게 되어 쿼리문 오류 디버깅이 어려움

(3) ORM : 쿼리와 c# 코드를 상호 연결 시켜 직접 쿼리를 작성하지 않아도 통신이 가능함
  - Java JPA - 기준 ⇒ **하이버네이트**
  - C# EntityFrameWork 1.0 - 7.0 (ms)
  - C# Mapper (ms 외)

  - EntityFrameWork 버전 
    - EntityFrameWork 1.0 ~6.0 ⇒ .net Framework
    - ASP.NET Core ⇒ 6.0 → EntityFramework core


## EntityFramework의 개발 방식 2가지 
(1) Database First 방식 
  - Database DBA (데이터베이스 관리자)
  - 설계 완료, 물리적 데이터베이스도 모두 완성된 상태 
  ⇒ Database 기준으로 Application 개발 

(2) Code First 방식 
  - Database 기준으로 Application 개발 역으로 Code → Database 생성해 Application 개발
  
  

## 참고문서 
- [C#의 Database통신](https://www.youtube.com/watch?v=7PTFfqov1wY)
- [EntityFramework의 개발 방식 2가지](https://www.youtube.com/watch?v=7PTFfqov1wY)
