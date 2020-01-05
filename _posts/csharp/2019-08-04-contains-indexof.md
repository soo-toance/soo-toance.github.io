---
title: "[C#] String.IndexOf과 String.Contains"
categories:
  - C#
tags:
  - Contains
  - IndexOf
---

문자열 내 검색 시 Contains와 IndexOf 중 어떤 것을 써야할까? 

## Reference

- [스택 오버플로우 : performance-of-indexofchar-vs-containsstring-for-checking-the-presence-of-a](https://stackoverflow.com/questions/28279933/performance-of-indexofchar-vs-containsstring-for-checking-the-presence-of-a) 
- [스택 오버플로우](https://stackoverflow.com/posts/498880/revisions) 
- [문서](https://docs.microsoft.com/ko-kr/dotnet/api/system.stringcomparison?view=netframework-4.8)
- [소스코드](https://referencesource.microsoft.com/#mscorlib/system/string.cs,8281103e6f23cb5c)


## Answer

> Contains은 기본적으로 IndexOf를 call하는 구조이나 StringComparison.Ordinal을, IndexOf는 기본으로 culture sensitive search를 하기 때문에 Contains이 보통 더 빠르다. 


- Contains
```c#
public bool Contains( string value ) {
    return ( IndexOf(value, StringComparison.Ordinal) >=0 );
}
```


- IndexOf
```c#
[Pure]
public int IndexOf(String value) {
    return IndexOf(value, StringComparison.CurrentCulture);
}
[Pure]
[System.Security.SecuritySafeCritical]
public int IndexOf(String value, int startIndex, int count, StringComparison comparisonType) {
    // Validate inputs
    if (value == null)
        throw new ArgumentNullException("value");

    if (startIndex < 0 || startIndex > this.Length)
        throw new ArgumentOutOfRangeException("startIndex", Environment.GetResourceString("ArgumentOutOfRange_Index"));

    if (count < 0 || startIndex > this.Length - count)
        throw new ArgumentOutOfRangeException("count", Environment.GetResourceString("ArgumentOutOfRange_Count"));
    Contract.EndContractBlock();

    switch (comparisonType) {
        case StringComparison.CurrentCulture:
            return CultureInfo.CurrentCulture.CompareInfo.IndexOf(this, value, startIndex, count, CompareOptions.None);

        case StringComparison.CurrentCultureIgnoreCase:
            return CultureInfo.CurrentCulture.CompareInfo.IndexOf(this, value, startIndex, count, CompareOptions.IgnoreCase);

        case StringComparison.InvariantCulture:
            return CultureInfo.InvariantCulture.CompareInfo.IndexOf(this, value, startIndex, count, CompareOptions.None);

        case StringComparison.InvariantCultureIgnoreCase:
            return CultureInfo.InvariantCulture.CompareInfo.IndexOf(this, value, startIndex, count, CompareOptions.IgnoreCase);

        case StringComparison.Ordinal:
            return CultureInfo.InvariantCulture.CompareInfo.IndexOf(this, value, startIndex, count, CompareOptions.Ordinal);

        case StringComparison.OrdinalIgnoreCase:
            if (value.IsAscii() && this.IsAscii())
                return CultureInfo.InvariantCulture.CompareInfo.IndexOf(this, value, startIndex, count, CompareOptions.IgnoreCase);
            else
                return TextInfo.IndexOfStringOrdinalIgnoreCase(this, value, startIndex, count);

        default:
            throw new ArgumentException(Environment.GetResourceString("NotSupported_StringComparison"), "comparisonType");
    }  
}
```


## Memo

- StringComparison.Ordinal : 서수(이진) 정렬 규칙을 사용하여 문자열을 비교
- StringComparison.CurrentCulture : 문화권 구분 정렬 규칙 및 현재 문화권을 사용하여 문자열을 비교.
