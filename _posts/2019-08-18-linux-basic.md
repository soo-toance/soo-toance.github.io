---
title: "[linux] 명령어 모음zip"
categories:
  - linux
tags:
  - 
  - IndexOf
---

Linux 명령어 모음zip

## Reference

- [selinux 정리](https://itzone.tistory.com/646) 
- [스택 오버플로우](https://unix.stackexchange.com/questions/184413/how-to-set-the-permission-drwxr-xr-x-to-other-folders) 
- [리눅스 폴더/파일 찾기](http://javakorean.com/%EB%A6%AC%EB%88%85%EC%8A%A4-%ED%8F%B4%EB%8D%94%ED%8C%8C%EC%9D%BC-%EC%B0%BE%EA%B8%B0/)


## Concpet
(1) 파일 관련 명령어

1) 파일 권한 변경 명렁어
```
chmod {파일권한} {파일경로}

-- drwxr-xr-x
chmod 755  {파일경로}

-- drwxrwxr-x it is:
chmod 775  {파일경로}
```

2) 파일명 찾기 
```
find -name ‘*.pl’
```


(2) selinux 관련 명령어

1) 권한 확인 명령어 
```
ls -Z
```

2) 권한 부여 명령어 
```
chcon-t {권한} {파일경로}
```

