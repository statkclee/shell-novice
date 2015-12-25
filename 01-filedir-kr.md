---
layout: page
title: 유닉스 쉘(Unix Shell)
subtitle: 파일과 디렉토리
minutes: 15
---
> ## 학습 목표 {.objectives}
>
> *   파일과 디렉토리의 차이점과 유사점을 설명한다.
> *   절대경로를 상대경로로 변환하고, 반대로 상대경로를 절대경로로 변환한다.
> *   특정 파일과 디렉토리를 식별할 수 있는 절대경로와 상대경로를 구성한다.
> *   쉘의 읽기-실행-출력(read-run-print) 주기를 각 단계별로 설명한다.
> *   명령-라인 호출에서 실제 명령어, 플래그, 파일명을 식별한다.
> *   탭 완성기능을 시연하고, 장점을 설명한다.

파일과 디렉토리 관리를 담당하고 있는 운영체제 부분을 **파일 시스템(file system)**이라고 한다. 
파일 시스템은 데이터를 정보를 담고 있는 파일과 파일 혹은 다른 디렉토리를 담고 있는 디렉토리(혹은 "폴더"")로 조직화한다.

파일과 디렉토리를 생성, 검사, 이름 바꾸기, 삭제하는데 명령어 몇개가 자주 사용된다. 
명령어를 살펴보기 위해, 쉘 윈도우를 연다:

~~~ {.bash}
$
~~~

달러 기호 ($)는 **프롬프트(prompt)**로 쉘이 입력을 기다리는 것을 보여준다; 
프롬프트로 여러분 컴퓨터 쉘은 다른 문자가 될 수 있고, 프롬프트 이전에 부가적인 정보가 보여질 수도 있다.
이번 학습 혹은 다른 학습에 나온 명령어를 타이핑할 때, 프롬프트는 타이핑하지 말고, 뒤에 따라 나오는 명령어만 타이핑한다.

`whoami` 명령어를 타이핑하고, 명령을 쉘에 보내기 위해서 엔터키(Enter Key, 종종 키보드에 따라 Return으로 표기도 됨)를 누룹니다. 
명령어에 대한 출력은 현재 사용자의 ID가 됩니다. 즉, 쉘이 생각하는 사용자가 누구인지를 보여준다:

~~~ {.bash}
$ whoami
~~~
~~~ {.output}
nelle
~~~

좀더 구체적으로, `whoami`를 타이핑할 때, 쉘은

1. `whoami` 라는 프로그램을 찾고,
2. 프로그램을 실행하고,
3. 프로그램 실행 결과를 출력하고,
4. 새로운 프롬프트를 화면에 출력해서 더 많은 명령어를 받을 준비가 되어 있음을 알려준다.

다음으로, `pwd` 명령어를 실행하여 지금 어느 위치에 있는지 알아본다. 
`pwd`는 "print working directory"의 첫 글자를 땄다. 
언제든지, **현재 작업 디렉토리(current working directory)**는 현재 기본디폴트 디렉토리가 된다. 
즉, 명시적으로 다른 곳으로 지정하지 않았다면, 
사용자가 명령어를 실행하는 디렉토리가 컴퓨터가 가정하는 디렉토리가 된다. 
다음에서, 컴퓨터의 응답은 `/Users/nelle`으로 넬(Nelle)의 **홈 디렉토리(home directory)**다:

~~~ {.bash}
$ pwd
~~~
~~~ {.output}
/Users/nelle
~~~

> ## 홈 디렉토리 {.callout}
>
> 홈 디렉토리 경로는 운영체제마다 다르게 보인다.
> 리눅스에서 `/home/nelle` 처럼 보이고, 윈도우에서는 
> `C:\Documents and Settings\nelle`와 유사하게 보인다.
> 윈도 버젼마다 다소 차이가 있을 수 있음에 주목한다.


> ## 알파벳 수프 (Alphabet Soup) {.callout}
>
> 사용자가 누구인지를 파악하는데는 `whoami`가 사용되면, 
> 사용자가 어디 있는지를 파악하는 명령어는 `whereami`가 되어야 한다. 
> 왜 `pwd`가 대신에 사용될까? 일반적인 대답은 다음과 같다. 
> 1970년대 초에 UNIX가 처음 개발되었을 때, 모든 키입력은 중요했다:
> 그 시절 장비는 느리고, 텔레타이프 되돌리기 실행취소가 너무나도 고생스러워서, 
> 타이핑 오류 숫자를 줄이기 위해 키입력 횟수를 줄이는 것이 
> 사용자 편의성(usability) 측면에서 진정한 승자가 되었다. 
> 현실은 종합계획없이 명령어가 전문어와 은어에 몰입된 사람들에 의해서 유닉스에 하나씩 하나씩 추가되었다. 
> 결과는 일관성이 없지만, 지금 우리는 뗄래야 뗄 수 없는 상태가 되었다.

"홈 디렉토리(home directory)"를 이해하기 위해서, 
파일 시스템이 전체적으로 어떻게 구성되었는지 살펴보자. 
최상단에 다른 모든 것을 담고 있는 **루트 디렉토리(root directory)**가 있다. 
슬래쉬 `/` 문자로 나타내고, `/users/nelle`에서 맨 앞에 슬래쉬이기도 하다.

홈 디렉토리 안쪽에 몇가지 다른 디렉토리가 있다:
`bin` (몇몇 내장 프로그램이 저장된 디렉토리), 
`data` (여러가지 데이터 파일이 저장된 디렉토리), 
`users` (사용자의 개인 디렉토리가 저장된 디렉토리), 
`tmp` (장기간 저장될 필요가 없는 임시 파일을 위한 디렉토리), 등등:

![파일 시스템](fig/filesystem.svg)

현재 작업 디렉토리 `/Users/nelle`는 `/Users` 내부에 저장되어 있다는 것을 알고 있는데, 이유는 `/Users`가 이름 처음 부분이기 때문에 알 수 있다. 마찬가지로 `/Users`는 루트 디렉토리 내부에 저장되어 있다는 것을 알 수 있는데, 이름이 `/`으로 시작되기 때문이다.

`/Users` 하단에, 컴퓨터 계정으로 각 사용자별 디렉토리를 볼 수 있다. 
미이라(Mummy) 파일은 `/Users/imhotep` 디렉토리에 저장되어 있고, 
늑대인가(Wolfman)의 파일은 `/Users/larry` 디렉토리에 저장되어 있고 
`/Users/nelle` 디렉토리에 `nelle`의 정보가 저장되어 있는데,
이것이 왜 `nelle`이 디렉토리 이름의 마지막 부분인 이유다.

![홈 디렉토리](fig/home-directories.svg)

> ## 경로(Path) {.callout}
>
> `/` 문자에 두가지 의미가 있음에 주목한다.
> 파일명 혹은 디렉토리명 앞에 나오면, 루트 디렉토리를 나타낸다.
> 명칭 *내부*에 나오면, 단지 구분자에 불과하다.

Nelle의 홈 디렉토리에 무엇이 있는지 `ls` 명령어를 실행해서 살펴보자.
`ls`는 "목록보기(listing)"를 나타낸다:

~~~ {.bash}
$ ls
~~~
~~~ {.output}
creatures  molecules           pizza.cfg
data       north-pacific-gyre  solar.pdf
Desktop    notes.txt           writing
~~~

![Nelle의 홈 디렉토리](fig/homedir.svg)

`ls`는 알파벳 순서로 깔끔하게 열로 정렬하여 현재 디렉토리에 있는 파일과 디렉토리 이름을 출력한다.
**플래그(flag)** `-F`를 추가하여 출력을 좀더 이해하기 좋게 생성할 수 있다. 
`ls`으로 하여금 디렉토리 이름 뒤에 `/`을 추가하게 일러준다:

~~~ {.bash}
$ ls -F
~~~
~~~ {.output}
creatures/  molecules/           pizza.cfg
data/       north-pacific-gyre/  solar.pdf
Desktop/    notes.txt            writing/
~~~

`/Users/nelle` 디렉토리에는 7개의 **서브 디렉토리(sub-directories)**가 담겨 있다. 
뒤에 슬래쉬를 갖지 않은 이름, 예를 들어 `notes.txt`, `pizza.cfg`, `solar.pdf`는 단순한 파일이다. 
`ls`과 `-F` 사이에 공백이 있음에 주목한다. 
공백이 없으면 쉘은 존재하지 않는 `ls-F` 명령어를 실행한다고 생각한다.


> ## 이름에는 무엇이 있나요? {.callout}
>
> Nelle의 파일 이름이 "무엇.무엇"으로 된 것을 알아챘을 것이다. 
> 이것은 단지 관례다: 파일 이름을 `mythesis` 혹은 원하는 무엇이든지 작명할 수 있다. 
> 하지만, 대부분의 사람들은 두 부분으로 구분된 이름을 사용하여
> 사람이나 프로그램이 다른 유형의 파일임을 구분하도록 돕는다. 
> 이름에 나온 두번째 부분을 **파일 확장자(filename extension)**라고 부르고, 
> 파일에 어떤 유형의 데이터가 담고 있는지 나타낸다. 
> `.txt` 확장자는 텍스트 파일임을, `.pdf`는 PDF 문서임을, 
> `.cfg` 확장자는 어떤 프로그램에 대한 구성정보를 담고 있는 형상관리 파일임을 나타낸다.
> 
> 단지 관습이기는 하지만 중요하다. 
> 파일은 바이트(byte) 정보를 담고 있다: PDF 문서, 이미지, 등에 대해서 규칙에 따라 
> 바이트를 해석하는 것은 사람과 작성된 프로그램에 맡겨졌다.
> 
> `whale.mp3`처럼 고래 PNG 이미지 이름을 갖는 파일을 고래 노래의 음성파일로 변환하는 마술은 없다. 
> 설사 누군가 두번 클릭할 때, 운영체제가 음악 재생기로 열어 실행할 수는 있지만 동작은 되지 않을 것이다.

이제 `ls -F data`(즉, 명령어 `ls`와 **인자(arguments)** `-F`과 `data`)를 실행하여 Nelle의 `data` 디렉토리에 무엇이 있는지 살펴보자. `ls` 명령어의 앞에 대쉬('-')가 없는 두번째 인자는 현재 디렉토리 이외의 목록을 보고자 한다고 것을 나타낸다:

~~~ {.bash}
$ ls -F data
~~~
~~~ {.output}
amino-acids.txt   elements/     pdb/	        salmon.txt
animals.txt       morse.txt     planets.txt     sunspot.txt
~~~

명령어 실행결과는 텍스트 파일 4개와 하위 디렉토리가 두개 있다는 것을 보여준다. 
이런 방식으로 계층적으로 조직하여 관리하는 것이 작업을 체계적으로 추적할 수 있게 도움을 준다: 
홈 디렉토리에 파일 수백개를 놓는 것도 가능하다. 
하지만, 마치 책상위에 종이 수백장을 쌓아 놓는 것과 유사해서, 이런 전략은 본인을 파멸로 이끌 수도 있다.

그런데 `data` 디렉토리 이름을 적은 것에 주목한다. 
끝에 슬래쉬가 없습니다. 
`-F` 플래그를 사용해서 파일과 디렉토리를 구별했을 때, 사용한 `ls` 명령어에 디렉토리 이름 뒤에 붙었다. 
그리고 **상대 경로(relative path)**이기 때문에 슬래쉬로 시작도 하지 않았다. 
즉, `ls`에게 파일의 루트에서가 아닌 현재 위치에서 찾으라고 한다.

> ## 매개 변수(Parameters) vs. 인수(Arguments) {.callout}
>
> [위키피디아(Wikipedia)](https://en.wikipedia.org/wiki/Parameter_(computer_programming)에 따르면, **인자(argument)** 와 **매개 변수(parameter)**는 약간 다른 것을 의미한다. 
> 하지만, 실무에서 대부분의 사람들은 상호 호환적으로 혹은 일관성 없이 사용한다. 
> 여기서도 구별없이 사용할 것이다.


If we run `ls -F /data` (*with* a leading slash) we get a different answer,
because `/data` is an **absolute path**:

~~~ {.bash}
$ ls -F /data
~~~
~~~ {.output}
access.log    backup/    hardware.cfg
network.cfg
~~~

The leading `/` tells the computer to follow the path from the root of the file system,
so it always refers to exactly one directory,
no matter where we are when we run the command.

What if we want to change our current working directory?
Before we do this,
`pwd` shows us that we're in `/Users/nelle`,
and `ls` without any arguments shows us that directory's contents:

~~~ {.bash}
$ pwd
~~~
~~~ {.output}
/Users/nelle
~~~
~~~ {.bash}
$ ls
~~~
~~~ {.output}
creatures  molecules           pizza.cfg
data       north-pacific-gyre  solar.pdf
Desktop    notes.txt           writing
~~~

We can use `cd` followed by a directory name to change our working directory.
`cd` stands for "change directory",
which is a bit misleading:
the command doesn't change the directory,
it changes the shell's idea of what directory we are in.

~~~ {.bash}
$ cd data
~~~

`cd` doesn't print anything,
but if we run `pwd` after it, we can see that we are now in `/Users/nelle/data`.
If we run `ls` without arguments now,
it lists the contents of `/Users/nelle/data`,
because that's where we now are:

~~~ {.bash}
$ pwd
~~~
~~~ {.output}
/Users/nelle/data
~~~
~~~ {.bash}
$ ls -F
~~~
~~~ {.output}
amino-acids.txt   elements/     pdb/	        salmon.txt
animals.txt       morse.txt     planets.txt     sunspot.txt
~~~

We now know how to go down the directory tree:
how do we go up?
We could use an absolute path:

~~~ {.bash}
$ cd /Users/nelle
~~~

but it's almost always simpler to use `cd ..` to go up one level:

~~~ {.bash}
$ pwd
~~~
~~~ {.output}
/Users/nelle/data
~~~
~~~ {.bash}
$ cd ..
~~~

`..` is a special directory name meaning
"the directory containing this one",
or more succinctly,
the **parent** of the current directory.
Sure enough,
if we run `pwd` after running `cd ..`, we're back in `/Users/nelle`:

~~~ {.bash}
$ pwd
~~~
~~~ {.output}
/Users/nelle
~~~

The special directory `..` doesn't usually show up when we run `ls`.
If we want to display it, we can give `ls` the `-a` flag:

~~~ {.bash}
$ ls -F -a
~~~
~~~ {.output}
./                  creatures/          notes.txt
../                 data/               pizza.cfg
.bash_profile       molecules/          solar.pdf
Desktop/            north-pacific-gyre/ writing/
~~~

`-a` stands for "show all";
it forces `ls` to show us file and directory names that begin with `.`,
such as `..` (which, if we're in `/Users/nelle`, refers to the `/Users` directory).
As you can see,
it also displays another special directory that's just called `.`,
which means "the current working directory".
It may seem redundant to have a name for it,
but we'll see some uses for it soon.
Finally, we also see a file called `.bash_profile`. This file usually contains settings to customize the shell (terminal). For this lesson material it does not contain any settings. There may also be similar files called `.bashrc` or `.bash_login`. The `.` prefix is used to prevent these configuration files from cluttering the terminal when a standard `ls` command is used.

> ## Orthogonality {.callout}
>
> The special names `.` and `..` don't belong to `ls`;
> they are interpreted the same way by every program.
> For example,
> if we are in `/Users/nelle/data`,
> the command `ls ..` will give us a listing of `/Users/nelle`.
> When the meanings of the parts are the same no matter how they're combined,
> programmers say they are **orthogonal**:
> Orthogonal systems tend to be easier for people to learn
> because there are fewer special cases and exceptions to keep track of.

> ## Another Useful Abbreviation {.callout}
>
> The shell interprets the character `~` (tilde) at the start of a path to
> mean "the current user's home directory". For example, if Nelle's home
> directory is `/Users/nelle`, then `~/data` is equivalent to
> `/Users/nelle/data`. This only works if it is the first character in the
> path: `here/there/~/elsewhere` is *not* `/Users/nelle/elsewhere`. Thus
> `cd ~` can be used to change to the home directory. An even shorter 
> way to return to user's home directory is `cd` (without arguments).


### Nelle's Pipeline: Organizing Files

Knowing just this much about files and directories,
Nelle is ready to organize the files that the protein assay machine will create.
First,
she creates a directory called `north-pacific-gyre`
(to remind herself where the data came from).
Inside that,
she creates a directory called `2012-07-03`,
which is the date she started processing the samples.
She used to use names like `conference-paper` and `revised-results`,
but she found them hard to understand after a couple of years.
(The final straw was when she found herself creating
a directory called `revised-revised-results-3`.)

> ## Output sorting {.callout}
>
> Nelle names her directories "year-month-day",
> with leading zeroes for months and days,
> because the shell displays file and directory names in alphabetical order.
> If she used month names,
> December would come before July;
> if she didn't use leading zeroes,
> November ('11') would come before July ('7'). Similarly, putting the year first 
> means that June 2012 will come before June 2013.

Each of her physical samples is labelled according to her lab's convention
with a unique ten-character ID,
such as "NENE01729A".
This is what she used in her collection log
to record the location, time, depth, and other characteristics of the sample,
so she decides to use it as part of each data file's name.
Since the assay machine's output is plain text,
she will call her files `NENE01729A.txt`, `NENE01812A.txt`, and so on.
All 1520 files will go into the same directory.

If she is in her home directory,
Nelle can see what files she has using the command:

~~~ {.bash}
$ ls north-pacific-gyre/2012-07-03/
~~~

This is a lot to type,
but she can let the shell do most of the work through what is called **tab completion**.
If she types:

~~~ {.bash}
$ ls nor
~~~

and then presses tab (the tab key on her keyboard),
the shell automatically completes the directory name for her:

~~~ {.bash}
$ ls north-pacific-gyre/
~~~

If she presses tab again,
Bash will add `2012-07-03/` to the command,
since it's the only possible completion.
Pressing tab again does nothing,
since there are 1520 possibilities;
pressing tab twice brings up a list of all the files,
and so on.
This is called **tab completion**,
and we will see it in many other tools as we go on.

![File System for Challenge Questions](fig/filesystem-challenge.svg)

> ## Relative path resolution {.challenge}
>
> If `pwd` displays `/Users/thing`, what will `ls ../backup` display?
>
> 1.  `../backup: No such file or directory`
> 2.  `2012-12-01 2013-01-08 2013-01-27`
> 3.  `2012-12-01/ 2013-01-08/ 2013-01-27/`
> 4.  `original pnas_final pnas_sub`

> ## Many ways to do the same thing - absolute vs relative paths {.challenge}
>
> For a hypothetical filesystem location of /home/amanda/data/, 
> select each of the below commands that Amanda could use to navigate to her home directory, 
> which is /home/amanda 

>1.  `cd .`
>2.  `cd /`
>3.  `cd /home/amanda`
>4.  `cd ../..`
>5.  `cd ~`
>6.  `cd home`
>7.  `cd ~/data/..`
>8.  `cd`
>9.  `cd ..`

> ## `ls` reading comprehension {.challenge}
>
> Assuming a directory structure as in the above Figure 
> (File System for Challenge Questions), if `pwd` displays `/Users/backup`,
> and `-r` tells `ls` to display things in reverse order,
> what command will display:
>
> ~~~
> pnas_sub/ pnas_final/ original/
> ~~~
>
> 1.  `ls pwd`
> 2.  `ls -r -F`
> 3.  `ls -r -F /Users/backup`
> 4.  Either \#2 or \#3 above, but not \#1.

> ## Default `cd` action {.challenge}
>
> What does the command `cd` without a directory name do?
>
> 1.  It has no effect.
> 2.  It changes the working directory to `/`.
> 3.  It changes the working directory to the user's home directory.
> 4.  It produces an error message.

> ## Exploring more `ls` arguments {.challenge}
>
> What does the command `ls` do when used with the `-s` and `-h`
> arguments?
