---
published: true
permalink: /csharp/string-replaceat
comments: true
categories:
  - csharp
title: '[C#] String의 N번째 문자를 Replace하는 방법'
---

String의 N번째 문자를 Replace하는 방법 


## Reference

- [스택 오버플로우](https://stackoverflow.com/questions/9367119/replacing-a-char-at-a-given-index-in-string/9367179) 


## Problem 
- String의 N번째 문자를 `string[index] = '{newChar}'로 바꾸려고 하니, `Property or indexer `string.this[int]' cannot be assigned to (it is read-only)` 오류가 발생

## Solution 
```c#
public static string ReplaceAt(this string input, int index, char newChar)
{
    if (input == null)
    {
        throw new ArgumentNullException("input");
    }
    char[] chars = input.ToCharArray();
    chars[index] = newChar;
    return new string(chars);
}
```
