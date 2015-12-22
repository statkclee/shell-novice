---
layout: page
title: 유닉스 쉘(Unix Shell)
---
유닉스 쉘(Unix Shell)은 대부분의 컴퓨터 사용자가 살아온 것보다 오래 동안 존재했다. 
오래동안 생존한 이유는 사용자로 하여금 단지 키보드 몇번 쳐서 복잡한 작업을 수행할 수 있게 하는 강력한 도구이기 때문이다. 좀더 중요하게는 기존의 프로그램을 새로운 방식으로 조합해서 반복적인 작업을 자동화함으로써, 동일한 작업을 반복적으로 하지 않게 만든다.
쉘 사용은 폭넓게 다양하고 강력한 도구와 컴퓨팅 자원(슈퍼컴퓨터와 "고성능 컴퓨팅(High Performance Computing, HPC)"이 포함)을 사용하는 근본이 된다.  
이번 학습은 효과적으로 이런 자원을 사용하는 과정으로 시작한다. 


> ## 전제조건 {.prereq}
>
> 이번 학습에서 파일 시스템과 쉘 기초를 안내한다.
> 만약 컴퓨터에 파일을 저장한 적이 있고, "파일(file)"과 "디렉토리(directory)" 혹은 "폴더(folder)"라는 단어을 인지했다면, 이번 학습에 준비가 되었다.
> 
> 만약 파일과 디렉토리를 조작하고,
> `grep` 과 `find` 명령어로 파일을 검색하고, 
> 간단한 루프와 스크립트를 작성하는데 이미 편안하다면,
> 아마도 이번 학습에서 그다지 배울 것은 없다.

> ## 사전 준비 {.getready}
>
> 수업을 따라하는데 필요한 파일을 다운로드한다:
> 
> 1. 바탕화면에 `shell-novice` 이름으로 새로운 폴더를 생성한다.
> 2. [shell-novice-data.zip](./shell-novice-data.zip)을 다운로드하고, 파일을 1번 폴더 안으로 옮긴다.
> 3. 만약 아직 압축을 풀지 않았다면, 두번 클릭해서 압축을 푼다. 최종 결과작업은 `data` 폴더가 새로 생겨야 된다.
> 4. 다음 명령어로 유닉스 쉘에서 상기 폴더로 접근할 수 있다:
>
> ~~~ {.input}
> $ cd && cd Desktop/shell-novice/data
> ~~~

## 학습주제
|   한국어(Korean)      |    영어(English)            |
|------------------------|---------------------------|
|1.  [쉘(shell) 소개](00-intro-kr.html)                         |1.  [Introducing the Shell](00-intro.html)   |
|2.  [파일과 디렉토리](01-filedir-kr.html)                      |2.  [Files and Directories](01-filedir.html)  |
|3.  [파일과 디렉토리 생성](02-create-kr.html)              |3.  [Creating Things](02-create.html)        |
|4.  [파이프와 필터](03-pipefilter-kr.html)   |4.  [Pipes and Filters](03-pipefilter.html)   |
|5.  [루프](04-loop-kr.html)                           |5.  [Loops](04-loop.html)                         |
|6.  [쉘 스크립트](05-script-kr.html)         |6.  [Shell Scripts](05-script.html)              |
|7.  [파일, 문자, 디렉토리  등 찾기](06-find-kr.html)        |7.  [Finding Things](06-find.html)              |

      
## 추가 학습교재       

*   [참고문헌](reference.html)
*   [토론](discussion.html)
*   [강사 안내서](instructors.html)