---
published: true
permalink: /algorithm/bigger-is-greater
comments: true
categories:
  - algorithm
tags: null
title: '[HackerRank] Bigger is Greater'  
---
HackerRank : bigger-is-greater

## Question

Given a word, create a new word by swapping some or all of its characters. This new word must meet two criteria:

- It must be greater than the original word
- It must be the smallest word that meets the first condition
For example, given the word `abcd`, the next largest word is `abdc`.

Complete the function biggerIsGreater below to create and return the new string meeting the criteria. If it is not possible, return `no answer`.

**Input Format**
The first line of input contains T, the number of test cases.
Each of the next T lines contains W.

**Output Format**
For each test case, output the string meeting the criteria. If no answer exists, print `no answer`.  
  
    
      
      


## Solution
(1) 첫번째 풀이 (오답) 

- 방법 
  - string을 char로 쪼개어서 int배열로 만든다. 
  ![1_1.JPG]({{site.baseurl}}/assets/images/algorithm/bigger-is-greater_1_1.JPG)

  - 뒷자리부터 시작해서
    - 늘어나는 수를 찾는다. 
    - 그리고 그 두 자리수를 바꾼다. 
    - 나머지 부분은 정렬한다. 
    ![1_3.JPG]({{site.baseurl}}/assets/images/algorithm/bigger-is-greater_1_3.JPG)

    
  - 마지막에 다시 int배열을 char배열로 바꾼다. 
  ![1_4.JPG]({{site.baseurl}}/assets/images/algorithm/bigger-is-greater_1_4.JPG)


- 결과  
  - 위 풀이방법은 아래 케이스를 커버하지 못하였다. 
    - `cnue` 
    
- 결론 
  - Bad : 테스트 케이스를 충분히 고려하지 못했다. 

- 소스코드 
```c#
   // Complete the biggerIsGreater function below.
    static string biggerIsGreater(string word) {

        // Make int Array 
        int[] intArray = new int[word.Length];
        for (int i = 0; i < word.Length; i ++)
            intArray[i] = (int)(word[i]);

        // Compare 
        for (int targetIndex=intArray.Length - 1; targetIndex >= 0; targetIndex --)
        {
            int target = intArray[targetIndex];

            // swap
            for (int nextIndex=targetIndex - 1; nextIndex >= 0; nextIndex--)
            {
                if (target > intArray[nextIndex])
                {
                    intArray[targetIndex] = intArray[nextIndex];
                    intArray[nextIndex] = target;
                    
                    // arrange 
                    Array.Sort(intArray, nextIndex+1, word.Length - nextIndex - 1);
                    
                    string result = "";    
                    foreach(int i in intArray)
                        result += (char)i;
                    
                    return result;
                }
            } 
        }

        return "no answer";
    }
```


(2) 두번째 풀이 (정답)
- 방법 
  - string을 char로 쪼개어서 int배열로 만든다. 
  ![2_1.JPG]({{site.baseurl}}/assets/images/algorithm/bigger-is-greater_2_1.JPG)

  - 뒷자리부터 시작해서 늘어나는 수를 찾는다.
  ![2_2.JPG]({{site.baseurl}}/assets/images/algorithm/bigger-is-greater_2_2.JPG)
  
  - 늘어나는 수부터 시작해서 있는 수들 중에서 늘어나는 수보다 크고 가장 작은 수를 찾아서 바꾼다. 
  ![2_4.JPG]({{site.baseurl}}/assets/images/algorithm/bigger-is-greater_2_4.JPG)

  - 나머지를 정렬한다. 
  ![2_5.JPG]({{site.baseurl}}/assets/images/algorithm/bigger-is-greater_2_5.JPG)
  
  - 마지막에 다시 int배열을 char배열로 바꾼다. 
  ![2_6.JPG]({{site.baseurl}}/assets/images/algorithm/bigger-is-greater_2_6.JPG)
  
- 결과  
  - 테스트 케이스 all 통과 
  
- 결론 
  - good : timeout 없이 전체 테스트 케이스 통과 
  - bad : 조금 더 단순한 방법이 있을 것 같은데 아직 그 방법을 찾지 못하였다. 

- 소스코드  
```c#
 // Complete the biggerIsGreater function below.
    static string biggerIsGreater(string word) {

        // Make int Array 
        int[] intArray = new int[word.Length];
        for (int i = 0; i < word.Length; i ++)
            intArray[i] = (int)(word[i]);

        // Find Minimun Number 
        int findIndex = word.Length - 1; 
        int minIndex = -1; 
        while (findIndex > 0)
        {
            if (intArray[findIndex] > intArray[findIndex - 1])
            {
                minIndex = findIndex - 1; 
                break;
            }
            
            findIndex = findIndex - 1; 
        }

        if (minIndex != -1)
        {
            int target = intArray[minIndex]; 

            // find swap Index 
            int swapIndex = minIndex + 1;
            int temp = intArray[minIndex + 1];

            for (int j = minIndex + 1; j < intArray.Length; j ++)
            {
                if ((temp > intArray[j]) && (intArray[j] > target))
                {
                    swapIndex = j;
                    temp = intArray[j];
                }
            }

            // Swap 
            intArray[minIndex] = intArray[swapIndex];
            intArray[swapIndex] = target; 

            // Arrange  
            Array.Sort(intArray, minIndex+1, word.Length - minIndex - 1);
            
        }

         

        if (minIndex == -1)
            return "no answer"; 
        else 
        {
            string result = "";
            foreach(int i in intArray)
                result += (char)i; 

            return result;
        }      
    }
```
  
    
      
      


## Learning... 
- next-lexicographical-permutation-algorithm
  - 뒤에서부터 시작해서 `inverse point`를 찾는다. (아래 유투브 영상에서 더 자세히 확인할 수 있다.) 
    - 4 : inverse point 
  - 나머지 숫자들 중 inverse point보다는 크지만 가장 작은 수를 찾는다. 
    - [4, 11, 8, 3] 
    - [8, 11, 4, 3] 
  - inverse point부터 해서 역순으로 배열한다. 
    - [8, 3, 4, 11] 

- 알고리즘을 설계하는데 있어서 주어진 input을 가지고 시작하지 말고,임의의 input(예를 들면, 12345)를 가지고 규칙을 찾아보는 것자. 

- testcase는 최대한 많이 돌려보고 `submit`을 하자. 



## Reference
- [HackerRank](https://www.hackerrank.com/challenges/bigger-is-greater/problem)
- [lexicographical permutation algorithm](https://www.nayuki.io/page/next-lexicographical-permutation-algorithm)
- [Youtube](https://www.youtube.com/watch?v=zGQq3HGBTXg)