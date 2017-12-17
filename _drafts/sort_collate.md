---
layout: post
title: linux sort 한글 정렬 이상

categories: [linux]
tags: [linux, bash, sort]
---

한글이 섞여 있는 파일을 [sort](http://www.gnu.org/software/coreutils/manual/coreutils.html#sort-invocation)로 정렬하는 경우 결과의 순서가 이상할때가 있다.

이런경우 `LC_COLLATE` 환경변수를 바꾸면 해결 된다. (물론 `LC_ALL`을 바꿔도 된다)

한글 UTF-8 기준으로 정렬이 필요하니 `LC_COLLATE="ko_KR.utf8"`로 설정하면 되겠다. 


```bash
# centos 7.2, GNU coreutils 8.22

$ cat text
바d
가l
자e
사m
다n
하g
나j
파h
타c
카f
아i
차b
마k
라a

$ sort text
라a
차b
타c
바d
자e
카f
하g
파h
아i
나j
마k
가l
사m
다n

$ C_COLLATE="ko_KR.UTF-8" sort text
가l
나j
다n
라a
마k
바d
사m
아i
자e
차b
카f
타c
파h
하g

$ LC_COLLATE="C" sort text
가l
나j
다n
라a
마k
바d
사m
아i
자e
차b
카f
타c
파h
하g
```

한글만 정렬할 경우 `C`,`POSIX`로 설정하여 `strcmp` 결과로 맞춰도 동일한 결과를 낼 수 있겠지만 영문 대소문자 정렬이 영향을 받으므로 주의해야 한다.


추가로 macOS 에서는 [UTF-8 관련 이슈](https://stackoverflow.com/questions/27395317/why-does-utf-8-text-sort-in-different-order-between-os-x-and-linux)로 `c`, `posix`로 맞춰야 정상 작동하는 것 같다. (대문자도 안되고 소문자로 해야 되는 것으로 보임)
