---
published: true
permalink: /algorithm/almost-sorted
comments: true
categories:
  - algorithm
title: '[HackerRank] Huffman Decoding'
Tree: Huffman Decoding
---
HackerRank : Tree: Huffman Decoding


## Question
Complete the function decode_huff in the editor below. It must return the decoded string.

decode_huff has the following parameters:

root: a reference to the root node of the Huffman tree
s: a Huffman encoded string

## Solution
```python
def decodeHuff(root, text):
    answer = ""

    curNode = root; 
    for s in text:
        if (s == "0") : curNode = curNode.left
        elif (s == "1") : curNode = curNode.right

        if (curNode.left == None or curNode.right == None) :
            answer += curNode.data
            curNode = root; 
    
    print(answer)
```

## Learning... 
- c#을 지원하지 않았던 문제라 오랜만에 python으로 문제를 풀었다. 
- 로직 자체는 간단했지만 python 문법 자체가 너무 오랜만이었다. (`printf..`나 `if..elif')



## Reference
- [HackerRank](https://www.hackerrank.com/challenges/tree-huffman-decoding/problem)
