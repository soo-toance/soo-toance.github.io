---
published: true
permalink: /algorithm/almost-sorted
comments: true
categories:
  - algorithm
title: '[HackerRank] Almost Sorted'
---
HackerRank : almost-sorted

## Question
Given an array of integers, determine whether the array can be sorted in ascending order using only one of the following operations one time.

1. Swap two elements.
2. Reverse one sub-segment.
Determine whether one, both or neither of the operations will complete the task. If both work, choose swap.


## Solution
### (1) 첫 번째 풀이 (오답)

- 분류  
  - 순차 (1 2 3 4 5)
  - 비순차 (5 4 3 2 1) -> swap/reverse
  - 순차 + 비순차 -> (1 2 5 4 3) -> swap/reverse
  - 순차 + 비순차 + 순차 -> (1 2 4 3 5) -> swap/reverse 
  
- 알고리즘 
  1. 순차/비순차 블럭 찾기 
    - 처음부터 순차 블럭(increaseBreak)을 찾는다. 
    - 순차 블럭 다음부터 비순차 블럭(decreaseBreak)을 찾는다. 
  2. 정렬/비정렬 판단
    - 순차 : `if (decreaseBreak == 0)`
    - 비순차 : `if (increaseBreak == 0)`
    - 순차 + 비순차 : `if ((decreaseBreak == (arr.Length -1) && (increaseBreak == 0 || arr[decreaseBreak] > arr[increaseBreak -1])))`
    - 순차 + 비순차 + 순차 : `else`
    
- 결과 
  - 위 풀이 방법은 아래 케이스를 커버하지 못했다. 
    - 순차 + 순차 + 순차 (1 2 3 5 4) => swap 3 4 
    - 비순차 + 순차 (2 1 3 4 5) => swap 1 2 
    - 순차 + 순차 (1 2 4 3 5) => swap 3 4
        
  
- 소스코드
```c#
 int increaseBreak = 1;
        int decreaseBreak = 0;

        // Find increaseBreak, decreaseBreak; 
        while (increaseBreak < arr.Length)
        {
            if (arr[increaseBreak] >= arr[increaseBreak - 1])
                increaseBreak += 1; 
            else {                
                decreaseBreak = increaseBreak; 
                increaseBreak = increaseBreak -1; 
                break; 
            }
        }

        // Find decreaseBreak 
        while ((decreaseBreak != 0) && (decreaseBreak < (arr.Length - 1)))
        {
            if (arr[decreaseBreak] >= arr[decreaseBreak + 1])
                decreaseBreak += 1; 
            else
                break; 
        }

        
        // Console.WriteLine("increaseBreak - " + increaseBreak);
        // Console.WriteLine("decreaseBreak - " + decreaseBreak);


        if (decreaseBreak == 0)
        {
            Console.WriteLine("yes");
            return;
        }

        if (increaseBreak == 0)
        {
            if (decreaseBreak == (arr.Length -1))
            {
                Console.WriteLine("yes");
                Console.WriteLine((decreaseBreak - increaseBreak == 1 ? "swap " : "reverse ") + (increaseBreak + 1) + " " + (decreaseBreak + 1));
            }
            else
            {
                Console.WriteLine("no");            
            }
            return;
        }


        // if there is no decreaseBreak 
        if ((decreaseBreak == (arr.Length -1) && (increaseBreak == 0 || arr[decreaseBreak] > arr[increaseBreak -1])))
        {
            Console.WriteLine("yes");
            Console.WriteLine((decreaseBreak - increaseBreak == 1 ? "swap " : "reverse ") + (increaseBreak + 1) + " " + (decreaseBreak + 1));
        }
        else {
            if (arr[decreaseBreak] > arr[increaseBreak - 1])
            {
                Console.WriteLine("yes");
                Console.WriteLine((decreaseBreak - increaseBreak == 1 ? "swap " : "reverse ") + (increaseBreak + 1) + " " + (decreaseBreak + 1));
            }
            else
            {
                Console.WriteLine("no");
            }
        }
```  


### (2) 두 번째 풀이 (정답)
- 분류 
  - 순차 (1 2 3 4 5)
  - 비순차 (5 4 3 2 1) 
  - 순차 + 비순차 (1 2 5 4 3)
  - 순차 + 순차 (1 2 4 3 5)
  - 비순차 + 순차 (2 1 3 4 5)
  - 순차 + 비순차 + 순차 (1 2 4 3 5)
  - 순차 + 순차 + 순차 (1 5 3 4 2 6)
  
- 알고리즘 
  1. 블럭 찾기 
    - 기준값(Grade = C)을 기준으로 순차 블럭(Grade = U), 비순차 블럭(Grade = D)을 찾는다. 
    - 블럭의 마지막 인덱스(Pos)을 저장한다. 
  2. 정렬/비정렬 판단
    - 블럭 개수 1개 : `if (breakList.Count == 1)`
    - 블럭 개수 2개 : `else if (breakList.Count == 2)`
      - 순차 + 비순차 : `if (breakList[0].Grade == 'U' && breakList[1].Grade != 'U'....`
      - 순차 + 순차 : `else if (breakList[0].Grade == 'U' && breakList[1].Grade == 'U'.....`
      - 비순차 + 순차 : `else if (breakList[0].Grade == 'D' && breakList[1].Grade == 'U'....`
    - 블럭 개수 3개 : `else if (breakList.Count == 3)`
      - 순차 + 비순차 + 순차 : `if (breakList[0].Grade == 'U' && breakList[1].Grade == 'D' && breakList[2].Grade != 'D'....`
      - 순차 + 순차 + 순차 : `else if (breakList[0].Grade == 'U' && breakList[1].Grade == 'U' && breakList[2].Grade == 'U'....`
      
      
- 결론 
  - good : timeout 없이 전체 테스트 케이스 통과 
  - bad : 순차 + 순차 + 순차 케이스를 생각해내는데 시간이 꽤 걸렸다. 


- 소스코드
```c#
  class Break {
        public char Grade { get; set; }
        public int Pos { get; set; }
    }

    // Complete the almostSorted function below.
    static void almostSorted(int[] arr) {

        List<Break> breakList = new List<Break> {new Break { Grade = 'C', Pos = 1 }};
        for (int i = 1; i < arr.Length; i ++)
        {
            char thisGrade;
            int thisPos;

            // Find Grade 
            if (arr[i] > arr[i - 1])
            {
                thisGrade = 'U';
                thisPos = i + 1;
            }
            else
            { 
                thisGrade = 'D';
                thisPos = i + 1;
            }

            if (breakList.Last().Grade != 'C' && breakList.Last().Grade != thisGrade)
                breakList.Add(new Break { Grade = 'C', Pos = thisPos});
            else 
            {
                breakList.Last().Grade = thisGrade;
                breakList.Last().Pos = thisPos;
            }
        }


        // Console.WriteLine("Count - " + breakList.Count);
        // foreach (Break temp in breakList)
        //   Console.WriteLine("temp.Grade - " + temp.Grade + "  temp.Pos - " + temp.Pos);

        if (breakList.Count == 1)
        {
            // U , D 
            Console.WriteLine("yes");
            if (breakList.Last().Grade == 'D')
                Console.WriteLine((breakList.Last().Pos == 2 ? "swap" : "reverse") + " 1 " + breakList.Last().Pos);
        }
        else if (breakList.Count == 2)
        {
            // U + D(C) 
            // Console.WriteLine(arr[breakList[1].Pos -1]);
            // Console.WriteLine(arr[breakList[0].Pos -1]);


            if (breakList[0].Grade == 'U' && breakList[1].Grade != 'U' && (arr[breakList[1].Pos -1] < arr[breakList[0].Pos - 1]) && arr[breakList[1].Pos - 1] > arr[breakList[0].Pos - 2])
            {
                Console.WriteLine("yes");
                Console.WriteLine((breakList.Last().Pos - breakList.First().Pos  < 3 ? "swap" : "reverse") + " " + breakList.First().Pos + " " + breakList.Last().Pos);
            }
            else if (breakList[0].Grade == 'D' && breakList[1].Grade == 'U' && (arr[0] < arr[breakList[0].Pos]))
            {
                Console.WriteLine("yes");
                Console.WriteLine((breakList.First().Pos < 3 ? "swap" : "reverse") + " 1 " + breakList.First().Pos);
            }
            else if (breakList[0].Grade == 'U' && breakList[1].Grade == 'U' && (arr[breakList[0].Pos -1] < arr[breakList[1].Pos - 1]) && arr[breakList[0].Pos] > arr[breakList[0].Pos - 2] && arr[breakList[0].Pos - 1] < arr[breakList[0].Pos + 1])
            {
                Console.WriteLine("yes");
                Console.WriteLine("swap " + breakList[0].Pos + " " + (breakList[0].Pos + 1));
            }
            else {
                Console.WriteLine("no");
            }
        }
        else if (breakList.Count == 3)
        {   
            // Console.WriteLine(arr[breakList[2].Pos -1]);
            // Console.WriteLine(arr[breakList[1].Pos -1]);
            // Console.WriteLine(arr[breakList[0].Pos -1]);

            // U , D, U(C) 
             if (breakList[0].Grade == 'U' && breakList[1].Grade == 'D' && breakList[2].Grade != 'D' && (arr[breakList[1].Pos -1] < arr[breakList[0].Pos - 1]) && (arr[breakList[1].Pos -1] < arr[breakList[2].Pos - 1]))
            {
                Console.WriteLine("yes");
                Console.WriteLine((breakList.Last().Pos - breakList.First().Pos  < 3 ? "swap" : "reverse") + " " + breakList[0].Pos + " " + breakList[1].Pos);
            }
            else if (breakList[0].Grade == 'U' && breakList[1].Grade == 'U' && breakList[2].Grade == 'U' && (arr[breakList[1].Pos] > arr[breakList[0].Pos - 2]) && (arr[breakList[1].Pos] < arr[breakList[0].Pos]) && (arr[breakList[0].Pos - 1] > arr[breakList[1].Pos]) && (arr[breakList[0].Pos - 1] < arr[breakList[2].Pos -1]))
            {
                Console.WriteLine("yes");
                Console.WriteLine("swap " + breakList[0].Pos + " " + (breakList[1].Pos + 1));
            }
            else {
                Console.WriteLine("no");
            }

        }
        else {
            Console.WriteLine("no");
        }
```        



## Learning... 
- 알고리즘을 설계하는데 있어서 5개 input (1 2 3 4 5)를 가지고 설계를 했는데 그러다 보니 순차 + 순차 + 순차 케이스를 찾는데 시간이 꽤 걸렸다. 
. 



## Reference
- [HackerRank](https://www.hackerrank.com/challenges/almost-sorted/problem)
