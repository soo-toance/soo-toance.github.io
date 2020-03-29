---
published: false
permalink: /algorithm/find-maximum-index-product
comments: true
categories:
  - algorithm
title: '[HackerRank] Find Maximum Index Product'
---
HackerRank : Find Maximum Index Product


## Question
Left(i) = closest index j such that j < i and aj > ai. If no such j exists then Left(i) = 0.
Right(i) = closest index k such that k > i and ak and ai. If no such k exists then Right(i) = 0.

We define IndexProduct(i) = Left(i) * Right(i). You need to find out the maximum IndexProduct(i) among all i.

## Solution
(1) 첫번째 풀이 : array 활용
```c#
 // Complete the solve function below.
    static long solve(int[] arr) {
        long indexProduct = 0; 
        for (int i=0; i < arr.Length - 1; i ++)
        {                    
            if (arr[i] > arr[i+1])
            {               
                long left = i + 1, right = 0;
                
                int pivot = i + 1; 
                while (pivot < arr.Length - 1) 
                {
                    if (arr[pivot + 1] > arr[pivot])
                    {
                        right = pivot + 2;
                        break;
                    }

                    pivot++;
                }

                if (indexProduct < (left * right))
                    indexProduct = left * right;
                
            }
        }

        return indexProduct;
    }
```



(2) 두번째 풀이 : stack 활용 
```c#
    // int Right (int[] arr)
    static int[] Forward(int[] arr) {
        Stack<int> temp = new Stack<int>();
        int[] right = new int[arr.Length];

        for (int i = 0; i < arr.Length; i ++) {
            if (temp.Count == 0 || arr[temp.Peek()] > arr[i]) {
                temp.Push(i);
            }
            else {
                while(temp.Count > 0 && arr[i] > arr[temp.Peek()]) {
                    right[temp.Peek()] = i + 1; 
                    temp.Pop();
                }
                temp.Push(i);
            }
        }

        return right;        
    }

    // int Left (int[] arr)
    static int[] Backward(int[] arr) {
        Stack<int> temp = new Stack<int>();
        int[] left = new int[arr.Length];

        for (int i = arr.Length - 1; i >= 0; i --) {
            if (temp.Count == 0 || arr[temp.Peek()] > arr[i]) {
                temp.Push(i);
            }
            else {
                while(temp.Count > 0 && arr[i] > arr[temp.Peek()]) {
                    left[temp.Peek()] = i + 1; 
                    temp.Pop();
                }
                temp.Push(i);
            }
        }

        return left;        
    }


    // Complete the solve function below.
    static long solve(int[] arr) {
        int[] left = Backward(arr);
        int[] right = Forward(arr);

        // Console.WriteLine(String.Join(", " , left));
        // Console.WriteLine(String.Join(", " , right));

        long indexProduct = 0; 
        for (int i = 0; i < arr.Length; i ++)
            indexProduct = Math.Max(indexProduct, Convert.ToInt64(left[i]) * Convert.ToInt64(right[i]));

        return indexProduct;
    }
```

## Learning... 
- array로 풀고 나서 discussions을 보니 stack으로 풀 수 있다는 이야기가 있었어서 stack으로 다시 시도하였다. (그래서 data structure 섹션에 있었을 지도.. )
- 풀고나서 조금 더 가독성이 올라가긴 한 것 같은데, 아직까지는 stack으로 먼저 풀이를 시작하는 방법이 익숙하지 않은 것 같다ㅠㅠ.. 조금 더 연습이 필요해보인다.  


## Reference
- [HackerRank](https://www.hackerrank.com/challenges/find-maximum-index-product/problem)
