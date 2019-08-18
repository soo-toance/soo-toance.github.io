---
title: "[C#] 명명적 인수와 선택적 인수 (named and optional parameter)"
categories:
  - C#
tags:
  - named parameter
  - optional parameter
---

가독성을 높이기 위해 인자값으로 함수의 파라미터명과 같이 전달할 수 있다. 


## Reference

- [스택 오버플로우](https://stackoverflow.com/questions/5262634/c-sharp-method-call-with-parameter-name-and-colon) 
- [스택 오버플로우](https://stackoverflow.com/questions/5262634/c-sharp-method-call-with-parameter-name-and-colon) 
- [마이크로소프트 문서](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/named-and-optional-arguments)


## Concept
- 파라미터명과 같이 함수에 기입하여 가독성을 높일 수 있고, 지정하지 않으면 디폴트값으로 설정하게할 수 있다. 
- C# 4부터 지원함 


```c#
public void Example(int required, string StrVal = "default", int IntVal = 0)
{
    // ...
}

public void Test()
{
    // This gives compiler error
    // Example(1, 10);

    // This works
    Example(1, IntVal:10);
}
```
