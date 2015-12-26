---
layout: page
title: 유닉스 쉘(Unix Shell)
subtitle: 파일과 디렉토리 생성
minutes: 15
---
> ## 학습 목표 {.objectives}
>
> *   주어진 도표에 매칭되도록 계층적으로 디렉토리 구조 생성한다.
> *   편집기 혹은 이미 만들어진 파일을 복사하거나 이름을 바꾸어서 상기 계층적 디렉토리에 파일을 생성한다.
> *   명령-라인을 사용해서 디렉토리의 내용 화면에 출력한다.
> *   특정 파일과 디렉토리 혹은 각각을 따로 삭제한다.

이제는 어떻게 파일과 디렉토리를 살펴보는지 알게 되었지만, 
우선, 어떻게 파일과 디렉토리를 생성할 수 있을까요? 
Nelle의 홈 디렉토리, `/Users/nelle`, 로 돌아가서 
`ls -F` 명령어를 사용하여 무엇을 담고 있는지 살펴봅시다:

~~~ {.bash}
$ pwd
~~~
~~~ {.output}
/Users/nelle
~~~
~~~ {.bash}
$ ls -F
~~~
~~~ {.output}
creatures/  molecules/           pizza.cfg
data/       north-pacific-gyre/  solar.pdf
Desktop/    notes.txt            writing/
~~~

명령어 `mkdir thesis`을 사용하여 새 디렉토리 `thesis`를 생성합시다
(출력되는 것은 아무것도 없습니다.):

~~~ {.bash}
$ mkdir thesis
~~~

이름에서 유추를 할 수도, 하지 못할 수도 있지만, 
`mkdir`은 "make directory(디렉토리 생성하기)"를 의미한다. 
`thesis`는 상대 경로여서(즉, 앞에 슬래쉬가 없음), 
새로운 디렉토리는 현재 작업 디렉토리 아래 만들어진다:

~~~ {.bash}
$ ls -F
~~~
~~~ {.output}
creatures/  north-pacific-gyre/  thesis/
data/       notes.txt            writing/
Desktop/    pizza.cfg
molecules/  solar.pdf
~~~

하지만 `thesis` 디렉토리에는 아직 아무것도 없다:

~~~ {.bash}
$ ls -F thesis
~~~

`cd` 명령어를 사용하여 `thesis`로 작업 디렉토리를 변경하자. 
Nano 텍스트 편집기를 실행해서 `draft.txt` 파일을 생성하자:

~~~ {.bash}
$ cd thesis
$ nano draft.txt
~~~

> ## 어떤 편집기가 좋을까요? {.callout}
>
> "`nano`가 텍스트 편집기다"라고 말할 때, 정말 "텍스트"만 의미한다. 
> 즉, 일반 문자 데이터만 작업할 수 있고, 테이블, 이미지, 혹은 다른 형태의 인간 친화적 미디어는 작업할 수 없다. 
> 예제로 `nano`를 사용하는데 이유는 거의 누구나 훈련없이 사용할 수 있기 때문이다. 
> 하지만 실제 작업에는 좀더 강력한 편집기 사용을 추천한다. 
> 유닉스 시스템 계열(맥 OS X, 리눅스)에서 많은 프로그래머가 [Emacs](http://www.gnu.org/software/emacs/) 혹은 [Vim](http://www.vim.org/)을 사용하거나, (둘다 완전히 비직관적이만, 심지어 유닉스 표준이기도 하다)
> 혹은 그래픽 편집기로 [Gedit](http://projects.gnome.org/gedit/)를 사용한다. 윈도우에서는 [Notepad++](http://notepad-plus-plus.org/)를 사용하는 것도 좋다.
> 
> 어떤 편집기를 사용하든, 파일을 검색하고 저장하는 것을 알 필요가 있다. 
> 쉘에서 편집기를 시작하면, (아마도) 현재 작업 디렉토리가 디폴트 시작 위치가 된다. 
> 컴퓨터 시작 메뉴에서 시작한다면, 대신에 데스크톱 혹은 문서 디렉토리에 파일을 저장하고 싶을지도 모른다. 
> "다른이름으로 저장하기(Save As ...)"로 다른 디렉토리로 이동하여 작업 디렉토리를 변경할 수 있다.

텍스트 몇 줄을 타이핑하고, 
컨트롤+O (Control-O)를 눌러서 데이터를 디스크에 쓰면 저장된다:

![Nano 작업화면](fig/nano-screenshot.png)

파일이 저장되면, 컨트롤+X (Control-X)를 사용하여 편집기를 끝내고 쉘로 돌아간다. 
(유닉스 문서에서 ^A로 줄여서 "컨트롤+A(control-A)"를 표기한다.) 
`nano`는 화면에 어떤 출력도 뿌려주지 않고 끝내지만, `ls` 명령어를 사용하여 `draft.txt` 파일이 생성된 것을 확인할 수 있다:

~~~ {.bash}
$ ls
~~~
~~~ {.output}
draft.txt
~~~

`rm draft.txt`을 실행해서 깨끗이 정리합시다:

~~~ {.bash}
$ rm draft.txt
~~~

이 명령문은 파일을 제거한다. ("rm"은 "remove"를 줄였다.) 
`ls`를 다시 실행하면, 화면에 출력되는 것은 없고 파일이 사라진 것을 확인할 수 있다:

~~~ {.bash}
$ ls
~~~

> ## 삭제는 영원하다 {.callout}
>
> 유닉스에는 삭제된 파일을 복구할 수 있는 휴지통이 없다. 
> (하지만, 유닉스에 기반한 대부분의 그래픽 인터페이스는 휴지통 기능이 있다)
> 파일을 삭제하면 파일시스템의 관리대상에서 빠져서 디스트 저장공간이 다시 재사용되게 한다. 
> 삭제된 파일을 찾아 되살리는 도구가 존재하지만, 
> 어느 상황에서나 동작한다는 보장은 없다. 
> 왜냐하면 파일이 저장되었던 공간을 컴퓨터가 바로 재사용할지 모르기 때문이다.

파일을 다시 생성하고 나서, `cd ..`를 사용하여 `/Users/nelle` 상위 디렉토리로 이동해보자:

~~~ {.bash}
$ pwd
~~~
~~~ {.output}
/Users/nelle/thesis
~~~
~~~ {.bash}
$ nano draft.txt
$ ls
~~~
~~~ {.output}
draft.txt
~~~
~~~ {.bash}
$ cd ..
~~~

`rm thesis`을 사용하여 전체 `thesis` 디렉토리를 제거하려고 하면 오류 메시지가 생긴다:

~~~ {.bash}
$ rm thesis
~~~
~~~ {.error}
rm: cannot remove `thesis': Is a directory
~~~

`rm` 명령어는 파일에만 동작하고 디렉토리에는 동작하지 않기 때문에 오류가 발생한다. 
올바른 명령어는 `rmdir`이고 "remove directory(디렉토리 제거하기)"를 줄여서 표현한다. 
하지만 이것도 동작하지 않는데 이유는 삭제하려는 디렉토리가 비어있지 않기 때문이다:

~~~ {.bash}
$ rmdir thesis
~~~
~~~ {.error}
rmdir: failed to remove `thesis': Directory not empty
~~~

이러한 작은 안전 기능이 정말 많은 사람을 슬픔으로부터 구원해줬다. 
특히, 여러분이 타이핑에 초보라면 더욱 그렇다. 
`thesis` 디렉토리를 제거하려면, 먼저 `draft.txt` 파일을 삭제해야 된다:

~~~ {.bash}
$ rm thesis/draft.txt
~~~

이제 디렉토리가 비워져서, `rmdir` 명령어로 삭제할 수 있다:

~~~ {.bash}
$ rmdir thesis
~~~

> ## 큰 힘에는 큰 책임이 따른다(With Great Power Comes Great Responsibility) {.callout}
>
> 디렉토리에 먼저 파일을 제거하고, 그리고 나서 디렉토리를 제거하는 방식은 지루하고 시간이 많이 걸린다. 
> 대신에 `-r` 옵션을 가진 `rm` 명령어를 사용할 수 있다. 
> `-r` 플래그 옵션은 "recursive(재귀적)"을 나타낸다.
>
> ~~~
> $ rm -r thesis
> ~~~
>
> 디렉토리에 모든 것을 삭제하고 나서 디렉토리 자체도 삭제한다. 
> 만약 디렉토리가 하위 디렉토리를 가지고 있다면, `rm -r`은 하위 디렉토리에도 같은 작업을 반복한다. 
> 매우 편리하지만, 부주위하게 사용되면 피해가 엄청날 수 있다.

다시 한번 디렉토리와 파일을 생성하자. 
이번에는 `thesis/draft.txt` 경로에서 `nano`를 실행함을 주목하자. 
이전에는 `thesis`디렉토리로 가서 `draft.txt`에 `nano`를 실행했다.

~~~ {.bash}
$ pwd
~~~
~~~ {.output}
/Users/nelle
~~~
~~~ {.bash}
$ mkdir thesis
~~~
~~~ {.bash}
$ nano thesis/draft.txt
$ ls thesis
~~~
~~~ {.output}
draft.txt
~~~

`draft.txt`가 특별한 정보를 제공하는 이름이 아니어서 `mv`를 사용하여 파일 이름을 변경하자. 
`mv`는 "move"의 줄임말이다:

~~~ {.bash}
$ mv thesis/draft.txt thesis/quotes.txt
~~~

첫 번째 매개변수는 `mv` 명령어에게 이동하려는 대상을, 두번째 매개변수는 어디로 이동되는지를 나타낸다. 
이번 경우에는 `thesis/draft.txt` 파일을 `thesis/quotes.txt`으로 이동한다. 
이렇게 파일을 이동하는 것이 파일 이름을 바꾸는 것과 동일한 효과를 가진다. 
아니나 다를까, `ls` 명령어를 사용하여 `thesis` 디렉토리에 이제 `quotes.txt` 파일만을 가지고 있음을 확인할 수 있다:

~~~ {.bash}
$ ls thesis
~~~
~~~ {.output}
quotes.txt
~~~

목표 파일명을 명세할 때 주의를 기울일 필요가 있다. 왜냐하면, `mv` 명령어는 동일 명칭을 갖는
어떤 기존 파일도 아주 조용히 덮어 써버려 데이터 손실에 이르게 된다.
부가적인 옵션 플래그, `mv -i` (즉 `mv --interactive`)를 사용해서
덮어쓰기 전에 사용자가 확인하도록 `mv` 명령어를 활용한다.

일관성이 없어, `mv`는 디렉토리에도 동작한다 --- 별도 `mvdir` 명령어는 없다.

`quotes.txt` 파일을 현재 작업 디렉토리로 이동합시다. `mv`를 다시 사용한다. 
하지만 이번에는 두번째 매개변수로 디렉토리 이름을 사용해서 파일이름을 바꾸지 않고, 새로운 장소에 놓는다. 
(이것이 왜 명령어가 "move(이동)"으로 불리는 이유다.) 
이번 경우에 사용되는 디렉토리 이름은 앞에서 언급한 특수 디렉토리 이름 `.` 이다.

~~~ {.bash}
$ mv thesis/quotes.txt .
~~~

과거에 있던 디렉토리에서 파일을 현재 작업 디렉토리로 옮긴 효과가 나타난다.
`ls` 명령어가 `thesis` 디렉토리가 비였음을 보여준다:

~~~ {.bash}
$ ls thesis
~~~

더 나아가, 
`ls` 명령어를 매개변수로 파일 이름 혹은 디렉토리 이름을 사용하면, 
그 해당 파일 혹은 디렉토리만 화면에 보여준다. 
이것을 사용하면, `quotes.txt` 파일이 현재 작업 디렉토리에 있음을 볼 수 있다:

~~~ {.bash}
$ ls quotes.txt
~~~
~~~ {.output}
quotes.txt
~~~

`cp` 명령어는 `mv` 명령어와 거의 동일하게 동작한다. 
차이점은 이동하는 대신에 복사한다는 점이다. 
매개변수로 경로를 두개 갖는 `ls` 명령어로 제대로 작업을 했는지 확인할 수 있다.
대부분의 유닉스 명령어와 마찬가지로, `ls` 명령어에 한번에 경로 수천개가 주어질 수 있다:

~~~ {.bash}
$ cp quotes.txt thesis/quotations.txt
$ ls quotes.txt thesis/quotations.txt
~~~
~~~ {.output}
quotes.txt   thesis/quotations.txt
~~~

복사를 제대로 수행했는지 증명하기 위해서, 현재 작업 디렉토리에 있는 `quotes.txt` 파일을 삭제하고 동일한 `ls` 명령어를 실행한다. 

~~~ {.bash}
$ rm quotes.txt
$ ls quotes.txt thesis/quotations.txt
~~~
~~~ {.error}
ls: cannot access quotes.txt: No such file or directory
thesis/quotations.txt
~~~

이번에는 현재 디렉토리에서 `quotes.txt` 파일은 찾을 수 없지만, 
삭제하지 않은 thesis 폴더의 복사본은 찾아서 보여준다.

> ## 파일 이름 바꾸기 {.challenge}
>
> Suppose that you created a `.txt` file in your current directory to contain a list of the
> statistical tests you will need to do to analyze your data, and named it: `statstics.txt`
>
> After creating and saving this file you realize you misspelled the filename! You want to
> correct the mistake, which of the following commands could you use to do so?
>
> 1. `cp statstics.txt statistics.txt`
> 2. `mv statstics.txt statistics.txt`
> 3. `mv statstics.txt .`
> 4. `cp statstics.txt .`

> ## 이동과 복사 {.challenge}
>
> 아래 보여진 일련의 명령문에 뒤에 `ls`명령어의 출력값은 무엇일까요?
>
> ~~~
> $ pwd
> /Users/jamie/data
> $ ls
> proteins.dat
> $ mkdir recombine
> $ mv proteins.dat recombine
> $ cp recombine/proteins.dat ../proteins-saved.dat
> $ ls
> ~~~
>
> 1.   `proteins-saved.dat recombine`
> 2.   `recombine`
> 3.   `proteins.dat recombine`
> 4.   `proteins-saved.dat`

> ## 디렉토리와 파일 조직화 {.challenge}
>
> 정훈이가 프로젝트 작업을 하고 있는데, 작업 파일이 그다지 잘 조직적으로 정리되지 않음을 알게 된다:
>
> ~~~
> $ ls -F
> analyzed/  fructose.dat    raw/   sucrose.dat
> ~~~
>
> `fructose.dat` 와 `sucrose.dat` 파일은 자료분석 결과 산출된 출력결과를 담고 있다.
> 이번 학습에서 배운 어떤 명령어를 실행해야,
> 아래 명령어를 실행했을 때 다음에 보여지는 출력을 생성할까요?
>
> ~~~
> $ ls -F
> analyzed/   raw/
> $ ls analyzed
> fructose.dat    sucrose.dat
> ~~~

> ## 다수 파일명 복사하기 {.challenge}
>
> 다음과 같이 몇개의 파일 이름과 디렉토리 이름이 주어졌을 때, `cp` 명령어는 무엇을 수행할까요?
>
> ~~~
> $ mkdir backup
> $ cp thesis/citations.txt thesis/quotations.txt backup
> ~~~
>
> 다음과 같이 세개 혹은 그 이상의 파일 이름이 주어졌을 때 `cp`는 무엇을 수행할까요?
>
> ~~~
> $ ls -F
> intro.txt    methods.txt    survey.txt
> $ cp intro.txt methods.txt survey.txt
> ~~~

> ## 재귀적으로 시간순으로 목록 출력하기 {.challenge}
>
> `ls -R` 명령어는 디렉토리의 내용을 재귀적으로 화면에 출력한다. 
> 즉, 하위 디렉토리, 하위의 하위 디렉토리 등등 알파벳 순으로 계층적 수준으로 뿌려준다. 
> `ls -t` 명령어는 마지막 변경사항의 시간 순으로 내용을 화면에 출력한다. 
> 즉, 가장 최근에 변경된 파일 혹은 디렉토리를 먼저 정렬하여 화면에 뿌려준다. 
> `ls -R -t`은 파일과 디렉토리를 어떤 순서로 화면에 보여줄까요?
