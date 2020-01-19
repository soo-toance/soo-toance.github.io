---
published: false
permalink: /algorithm-cycle-detection
comments: true
categories:
  - algorithm
tags: null
title: '[HackerRank] Cycle Detection'
---
## Question

If there is a cycle, return true; otherwise, return false.

**Input Format**
Our hidden code checker passes the appropriate argument to your function. You are not responsible for reading any input from stdin.

**Output Format**
If the list contains a cycle, your function must return true. If the list does not contain a cycle, it must return false. The binary integer corresponding to the boolean value returned by your function is printed to stdout by our hidden code checker.


## Solution
- **good** : hashset 데이터 구조 활용해서 문제풀이를 시도하였다. 
- **bad** : data 기준으로 cycle검증을 진행하려 해서 시간이 꽤 걸렸다.  

```c#
 static bool hasCycle(SinglyLinkedListNode head) {
        SinglyLinkedListNode currentNode = head;
        bool result = false; 
        HashSet<SinglyLinkedListNode> savedList = new HashSet<SinglyLinkedListNode>(); 

        while (currentNode.next != null)
        {  
            if (!savedList.Add(currentNode))
            {
                result = true;
                break; 
            }

            currentNode = currentNode.next;
        }

        return result;
}
```

## Reference
- [HashSet.Add](https://docs.microsoft.com/en-us/dotnet/api/system.collections.generic.hashset-1.add?view=netframework-4.8#System_Collections_Generic_HashSet_1_Add__0_)
- [HackerRank](https://www.hackerrank.com/challenges/detect-whether-a-linked-list-contains-a-cycle/problem)