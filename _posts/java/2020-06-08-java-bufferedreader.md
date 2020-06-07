---
published: true
permalink: /java/bufferedreader
comments: true
categories:
  - java
title: '[java] BufferedReader'
---
자바 BufferedReader  

## Reference
- [Docs](https://docs.oracle.com/javase/8/docs/api/java/io/BufferedReader.html)

  
    
    
## Notes
1) Constructors 
- `BufferedReader(Reader in)` : 기본값 사이즈의 입력 스트림 생성 
- `BufferedReader(Reader in, int sz)` : 주어진 사이즈의 입력 스트립 생성 
  

2) Usage
- 키보드로부터 입력받을 때 

```java
BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

int N = Integer.parseInt(br.readLine());
String book = br.readLine();
```

- 파일로부터 입력받을 때 

```java
BufferedReader br = new BufferedReader(new FileReader("file.txt"));

String book = br.readLine();
```

3) Learning.. 
- Scanner클래스를 사용하는 것보다 속도가 빠름. 
- 예외처리 필요함. 
