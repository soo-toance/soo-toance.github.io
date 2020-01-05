---
permalink: /csharp/file-dimensions
title: "[C#] 파일의 가로/세로 픽셀 계산"
categories:
  - csharp
tags:
  - dimension
  - pixel
---

파일의 가로 세로 픽셀 수를 계산하기 위해서는 이미지로 변환 후 계산해햐 한다.


## Reference

- [스택 오버플로우](https://stackoverflow.com/questions/5190069/check-uploaded-images-dimensions) 


## Concept
- 가급적 using문을 사용하여 다 사용한 객체를 처리해주는 것이 좋다. 


```c#
 using (Stream memStream = new MemoryStream(bytes))
  {
    using (Image img = System.Drawing.Image.FromStream(memStream))
    {
      int width = img.Width;
      int height = img.Height;
    }
  }
```
