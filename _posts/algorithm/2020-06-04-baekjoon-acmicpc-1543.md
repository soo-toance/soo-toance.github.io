---
published: true
permalink: /algorithm/baekjoon-acmicpc-1543
comments: true
categories:
  - algorithm
title: '[BaekJoon] 1543. # 문서 검색'
---
[BaekJoon] 1543. # 문서 검색


## Question

세준이는 영어로만 이루어진 어떤 문서를 검색하는 함수를 만들려고 한다. 이 함수는 어떤 단어가 총 몇 번 등장하는지 세려고 한다. 그러나, 세준이의 함수는 중복되어 세는 것은 빼고 세야 한다. 예를 들어, 문서가 abababa이고, 그리고 찾으려는 ababa라면, 세준이의 이 함수는 이 단어를 0번부터 찾을 수 있고, 2번부터도 찾을 수 있다. 그러나 동시에 셀 수는 없다.

세준이는 문서와 검색하려는 단어가 주어졌을 때, 그 단어가 최대 몇 번 중복되지 않게 등장하는지 구하는 프로그램을 작성하시오.

## Solution
(1) 첫번째 풀이 (정답) : 메모리 14408KB, 112ms
- indexOf(String str, int fromIndex) 사용해서 문자열 찾으면 그 다음번부터 다시 문자열 찾음

```java
import java.util.Scanner; 

public class Main { 
    public static void main(String[] args) { 
        String document;
        String searchString; 
        
        Scanner scn = new Scanner(System.in); 
        document = scn.nextLine(); 
        searchString = scn.nextLine(); 
        
        int matchCount = 0; 
        int startPosition = 0; 
	    while (document.indexOf(searchString, startPosition) > -1)
        {
            matchCount++;
            startPosition = document.indexOf(searchString, startPosition) + searchString.length();          	 
        }       
        
        System.out.println(matchCount);  
    } 
}
```



(2) 두번째 풀이 (정답) : 메모리 14384KB, 112ms 
- substring 사용해서 검색 불필요한 부분 삭제 후 다시 검색 

```java
import java.util.Scanner; 

public class Main { 
    public static void main(String[] args) { 
        String document;
        String searchString; 
        
        Scanner scn = new Scanner(System.in); 
        document = scn.nextLine(); 
        searchString = scn.nextLine(); 
        
        int matchCount = 0; 
	    while (document.indexOf(searchString) > -1)
        {
            matchCount++;           
          	document = document.substring(document.indexOf(searchString)  + searchString.length());    
        }       
        
        System.out.println(matchCount);  
    } 
}
```




(3) 세번째 풀이 (정답) : 메모리 14880KB, 112ms 
- substring 사용해서 document 일부분 추출하여 직접 searchString과 비교 

```java
import java.util.Scanner; 

public class Main { 
    public static void main(String[] args) { 
	    int matchCount = 0; 
        int searchPosition = 0; 
        while (searchPosition + searchString.length() <= document.length())
        {
            // System.out.println(searchPosition + " : " + document.substring(searchPosition, searchPosition + searchString.length()));
            if (document.substring(searchPosition, searchPosition + searchString.length()).equals(searchString)) {
                searchPosition += searchString.length(); // 일치한 부분 다음 문자부터 다시 검색
                matchCount++;
            }
            else 
                searchPosition += 1; // 다음 문자부터 다시 검색 
          
        }
        System.out.println(matchCount); 
    } 
}
```

## Learning... 
- java 문법 : 처음 시도에 Compile Error가 발생했던 코드
  1. string 대신 String 
  2. IndexOf 대신 indexOf
  3. length 대신 length() 
  


## Reference
- [BaekJoon](https://www.acmicpc.net/problem/1543)
