---
published: false
permalink: /java/stream
comments: true
categories:
  - java
title: '[java] Stream'
---

자바 Stream  

## Reference
- [Docs](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/util/stream/Stream.html) 


## Notes
1) 자주 쓰는 Methods
  - `filter​(Predicate<? super T> predicate)` : 조건에 맞는 값 추출 
    - `Arrays.stream(colProtected).filter(n -> n == 0)` : colProtected Array에서 0인 값 추출 
