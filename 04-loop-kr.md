---
layout: page
title: 유닉스 쉘(Unix Shell)
subtitle: 루프
minutes: 15
---
> ## 학습 목표 {.objectives}
>
> *  파일 집합에서 각 파일에 따로 따로 나뉘어서 하나 혹은 명령어 다수를 적용하는 루프를 작성한다.
> *  루프가 실행되는 동안에 루프 변수가 취하는 값을 추적한다.
> *  변수명과 변수값 차이에 대해 설명한다.
> *  왜 공백과 일부 구두점 문자는 파일 이름에 사용되지 말아야 되는지 설명한다.
> *  어떤 명령어가 최근에 실행되었는지를 확인하는 방법을 시범으로 보여준다.
> *  명령어를 다시 타이핑하지 않고 최근에 실행된 명령어를 다시 실행한다.

반복적으로 명령어를 실행하게 함으로써 자동화를 통해서 **루프**는 생산성 향상에 핵심이 된다. 와일드 카드와 탭 자동완성과 유사하게, 루프를 사용하면 타이핑 상당량(타이핑 실수)을 줄일 수 있다. 와일드카드와 탭 자동완성은 타이핑을 (타이핑 실수를) 줄이는 두가지 방법이다. 또다른 것은 쉘이 반복해서 특정 작업을 수행하게 하는 것이다. `basilisk.dat`, `unicorn.dat` 등으로 이름 붙여진 게놈 데이터 파일이 수백개 있다고 가정하자. 이번 예제에서, 단지 두개 예제 파일만 있는 `creatures` 디렉토리를 사용할 것이지만 동일한 원칙은 훨씬 더 많은 파일에 즉시 적용될 수 있다. 디렉토리에 있는 파일을 변경하고 싶지만, 원본 파일을 `original-basilisk.dat`과 `original-unicorn.dat`으로 이름을 변경해서 저장한다. 하지만 다음 명령어를 사용할 수 없다:

~~~ {.bash}
$ cp *.dat original-*.dat
~~~

왜냐하면 상기 두 파일 경우에 전개가 다음과 같이 될 것이기 때문이다:

~~~ {.bash}
$ cp basilisk.dat unicorn.dat original-*.dat
~~~

오류가 대신에, 파일을 백업하지 않는다:

~~~ {.error}
cp: target `original-*.dat' is not a directory
~~~

`cp` 명령어가 입력값 두개 이상을 받을 때 이런 문제가 발생한다. 이런 상황이 발생할 때, 마지막 입력값을 디렉토리로 예상해서 모든 파일을 해당 디렉토리로 넘긴다. 
`creatures` 디렉토리에는 `original-*.dat` 라고 이름 붙은 하위 디렉토리가 없기 때문에, 오류가 생긴다.

대신에, 리스트에서 한번에 연산작업을 하나씩 수행하는 
**루프(loop)**를 사용할 수 있다.
교대로 각 파일에 대해 첫 3줄을 화면에 출력하는 단순한 예제가 다음에 나와 있다:

~~~ {.bash}
$ for filename in basilisk.dat unicorn.dat
> do
>    head -3 $filename
> done
~~~
~~~ {.output}
COMMON NAME: basilisk
CLASSIFICATION: basiliscus vulgaris
UPDATED: 1745-05-02
COMMON NAME: unicorn
CLASSIFICATION: equus monoceros
UPDATED: 1738-11-24
~~~

쉘이 키워드 `for`를 보게 되면, 쉘은 리스트에 있는 각각에 대해 명령문 하나(혹은 명령문 집합)을 반복할 것이라는 것을 알고 있다. 이번 경우에 리스트는 파일이름 두개다. 
루프를 반복할 때마다, 현재 작업하고 있는 파일 이름은 `filename`으로 불리는 **변수(variable)**에 할당된다. 루프 내부에서, 변수 이름 앞에 `$` 기호를 붙여 변수 값을 얻는다: 즉, `$filename` 루프가 첫번째 돌 때 `basilisk.dat`이 되고, `unicorn.dat`는 두번째가 되고 계속 이어진다.

달러 기호를 사용해서, 쉘 해석기에게 `filename`을 변수명으로 처리하고, 값을 해당자리에 치환하라고 일러준다. 
변수명을 분명히 구분하는데 중괄호 내부에 변수명을 넣어서 변수로 사용하는 것도 가능하다: `$filename` 은 `${filename}`와 동치지만, `${file}name`와는 다르다. 
이 표기법을 다른 사람 프로그램에서 찾아볼 수 있다.

마지막으로 실제 실행되는 명령어는 오랜 친구인 `head`가 되어서, 루프는 교대로 각 데이터 파일의 첫 3줄을 출력한다.

> ## 프롬프트 따라가기 {.callout}
>
> 루프안에서 타이핑을 할 때, 쉘 프롬프트가 `$`에서 `>`으로 바뀐다. 
> 두번째 프롬프트는, `>`, 온전한 명령문 타이핑이 끝마치지 않았음을 상기시키려고 다르다.
> 세미콜론 `;` 을 사용해서 단일 명령줄에 작성된 두 명령을 구별한다.

목적을 좀더 사람 독자에게 명확히 하기 위해서 루프의 변수를 `filename`로 했다. 
쉘 자체는 변수가 어떻게 작명되든지 문제삼지 않는다. 만약 루프를 다음과 같이 작성하거나:


~~~ {.bash}
for x in basilisk.dat unicorn.dat
do
    head -3 $x
done
~~~

혹은:

~~~ {.bash}
for temperature in basilisk.dat unicorn.dat
do
    head -3 $temperature
done
~~~

둘다 정확하게 동일하게 동작한다. 
*이렇게는 절대 하지 마세요*. 사람이 프로그램을 이해할 수 있을 때만 프로그램이 유용하기 때문에, (`x`같은) 의미없는 이름이나, (`temperature`같은) 오해를 줄 수 있는 이름은 오해를 줘서 독자가 생각하기에 프로그램이 수행해야 할 것을 프로그램이 수행하지 못할 가능성을 높인다.

다음에 좀더 복잡한 루프가 있다:

~~~ {.bash}
for filename in *.dat
do
    echo $filename
    head -100 $filename | tail -20
done
~~~

쉘이 `*.dat`을 전개해서 쉘이 처리할 파일 리스트를 생성한다. 그리고 나서 **루프 몸통(loop body)** 부분이 파일 각각에 대해 명령어 두개를 실행한다. 
첫 명령어 `echo`는 명령 라인 매개변수를 표준 출력으로 화면에 뿌려준다. 
예를 들어:

~~~ {.bash}
$ echo hello there
~~~

다음을 화면에 출력한다:

~~~ {.output}
hello there
~~~

이 사례에서, 쉘이 파일 이름으로 `$filename`을 전개했기 때문에, 
`echo $filename`은 단지 파일 이름만 화면에 출력한다. 다음과 같이 작성할 수 없다는 것에 주의한다:

~~~ {.bash}
for filename in *.dat
do
    $filename
    head -100 $filename | tail -20
done
~~~

왜냐하면, `$filename`이 `basilisk.dat`으로 전개될 때 루프 처음에 쉘이 프로그램으로 인식한 `basilisk.dat`를 실행하려고 하기 때문이다. 
마지막으로, `head`와 `tail` 조합은 어떤 파일이 처리되든 81-100줄만 선택해서 화면에 뿌려준다.

> ## 파일, 디렉토리, 변수 등 이름에 공백 {.callout}
> 
> 루프 파일 이름 전개가 파일명에 공백을 사용하지 말아야 되는 또다른 이유가 된다. 
> 데이터 파일이 다음과 같은 이름으로 되었다고 가정하자:
> 
> ~~~
> basilisk.dat
> red dragon.dat
> unicorn.dat
> ~~~
> 
> 다음을 사용하여 파일을 처리하려고 한다면:
> 
> ~~~
> for filename in *.dat
> do
>     head -100 $filename | tail -20
> done
> ~~~
> 
> 그러면, 쉘은 `*.dat`을 전개해서 다음을 생성한다:
> 
> ~~~
> basilisk.dat red dragon.dat unicorn.dat
> ~~~
> 
> 좀 오래된 배쉬(Bash) 혹은 대부분의 쉘과 마찬가지로, `filename`에 순차적으로 다음 값으로 할당될 것이다:
> 
> ~~~
> basilisk.dat
> red
> dragon.dat
> unicorn.dat
> ~~~
>
> 이것이 문제가 된다. 
> `head`는 `red`와 `dragon.dat` 파일을 읽을 수가 없다.
> 왜냐하면 파일이 존재하지 않고 `red dragon.dat` 파일을 읽어올 수도 없다.
> 변수 사용에 **인용부호(quoting)** 처리해서 약간 더 강건하게 스크립트를 작성할 수 있다:
> 
> ~~~
> for filename in *.dat
> do
>     head -100 "$filename" | tail -20
> done
> ~~~
>
> 하지만, 파일명에 공백 혹은 다른 특수 문자의 사용을 피하는 것이 훨씬 더 간단하다.

원본 파일 이름을 바꾸는 문제로 다시 돌아가서, 다음 루프를 사용해서 문제를 해결할 수 있다:

~~~ {.bash}
for filename in *.dat
do
    cp $filename original-$filename
done
~~~

상기 루프는 `cp` 명령문을 각 파일이름에 대해 실행한다.
처음에 `$filename`이 `basilisk.dat`로 전개될 때, 쉘이 다음을 실행한다:

~~~ {.bash}
cp basilisk.dat original-basilisk.dat
~~~

두번째에는 명령문이 다음과 같다:

~~~ {.bash}
cp unicorn.dat original-unicorn.dat
~~~

> ## 두번 두드려 측정하고, 한번 실행 {.callout}
> 
> 루프는 많은 작업을 한번에 수행하는 방법이다 --- 혹은 만약 잘못된 작업을 수행한다면, 한번에 많은 실수를 저지르게 된다. 
> 루프가 수행하는 작업을 *확인하는* 한 방법은 실제로 작업을 실행하는 대신, 실행할 명령어를 `echo`를 사용하여 메아리 치는 것이다. 
> 예를 들어, 다음과 같이 파일 이름을 복사하는 루프를 작성할 수 있다:
> 
> ~~~
> for filename in *.dat
> do
>     echo cp $filename original-$filename
> done
> ~~~
> 
> `cp`를 실행하는 대신, 루프가 `echo`를 실행해서, 실제 명령어를 실행하지 *않고* 다음을 화면에 출력한다:
> 
> ~~~
> cp basilisk.dat original-basilisk.dat
> cp unicorn.dat original-unicorn.dat
> ~~~
> 
> 그리고 나서, 위쪽 화살표를 사용해서 루프를 다시 화면에 출력하고, 뒤쪽 화살표를 사용해서 `echo` 단어에 도달해서 삭제하고 실제 `cp` 명령어로 루프를 실행하기 하도록 "엔터(enter)"키를 누른다. 
> 이 방법은 실패없는 완벽한 것은 아니지만, 루프가 어떻게 동작하고 있는지 학습할 때, 무슨 일이 일어나고 있는지를 살펴볼 수 있는 간편한 방법이다.

## Nelle의 파이프라인: 파일 처리하기

Nelle은 이제 데이터 파일을 처리할 준비가 되었다. 
아직 쉘을 어떻게 사용하는지 학습단계에 있기 때문에, 단계별로 요구되는 명령어를 차근히 작성하기로 마음먹었다.
첫번째 단계는 적합한 파일을 선택했는지를 확인하는 것이다 --- 'Z'가 아닌 'A' 혹은 'B'로 파일이름이 끝나는 것이 적합한 파일이라는 것을 명심한다. 홈 디렉토리에서 시작해서, Nelle이 다음과 같이 타이핑한다:

~~~ {.bash}
$ cd north-pacific-gyre/2012-07-03
$ for datafile in *[AB].txt
> do
>     echo $datafile
> done
~~~
~~~ {.output}
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
NENE02043A.txt
NENE02043B.txt
~~~

다음 단계는 `goostats` 분석 프로그램이 생성할 파일이름을 무엇으로 할지 결정하는 것이다. 
"stat"을 각 입력 파일에 접두어로 붙이는 것이 간단해 보여서, 루프를 변경해서 작업을 수행하도록 한다:

~~~ {.bash}
$ for datafile in *[AB].txt
> do
>     echo $datafile stats-$datafile
> done
~~~
~~~ {.output}
NENE01729A.txt stats-NENE01729A.txt
NENE01729B.txt stats-NENE01729B.txt
NENE01736A.txt stats-NENE01736A.txt
...
NENE02043A.txt stats-NENE02043A.txt
NENE02043B.txt stats-NENE02043B.txt
~~~

She hasn't actually run `goostats` yet,
but now she's sure she can select the right files and generate the right output filenames.

Typing in commands over and over again is becoming tedious,
though,
and Nelle is worried about making mistakes,
so instead of re-entering her loop,
she presses the up arrow.
In response,
the shell redisplays the whole loop on one line
(using semi-colons to separate the pieces):

~~~ {.bash}
$ for datafile in *[AB].txt; do echo $datafile stats-$datafile; done
~~~

Using the left arrow key,
Nelle backs up and changes the command `echo` to `goostats`:

~~~ {.bash}
$ for datafile in *[AB].txt; do bash goostats $datafile stats-$datafile; done
~~~

When she presses enter,
the shell runs the modified command.
However, nothing appears to happen --- there is no output.
After a moment, Nelle realizes that since her script doesn't print anything to the screen any longer,
she has no idea whether it is running, much less how quickly.
She kills the job by typing Control-C,
uses up-arrow to repeat the command,
and edits it to read:

~~~ {.bash}
$ for datafile in *[AB].txt; do echo $datafile; bash goostats $datafile stats-$datafile; done
~~~

> ## Beginning and End {.callout}
>
> We can move to the beginning of a line in the shell by typing `^A`
> (which means Control-A)
> and to the end using `^E`.

When she runs her program now,
it produces one line of output every five seconds or so:

~~~ {.output}
NENE01729A.txt
NENE01729B.txt
NENE01736A.txt
...
~~~

1518 times 5 seconds,
divided by 60,
tells her that her script will take about two hours to run.
As a final check,
she opens another terminal window,
goes into `north-pacific-gyre/2012-07-03`,
and uses `cat stats-NENE01729B.txt`
to examine one of the output files.
It looks good,
so she decides to get some coffee and catch up on her reading.

> ## Those Who Know History Can Choose to Repeat It {.callout}
> 
> Another way to repeat previous work is to use the `history` command to
> get a list of the last few hundred commands that have been executed, and
> then to use `!123` (where "123" is replaced by the command number) to
> repeat one of those commands. For example, if Nelle types this:
> 
> ~~~
> $ history | tail -5
>   456  ls -l NENE0*.txt
>   457  rm stats-NENE01729B.txt.txt
>   458  bash goostats NENE01729B.txt stats-NENE01729B.txt
>   459  ls -l NENE0*.txt
>   460  history
> ~~~
> 
> then she can re-run `goostats` on `NENE01729B.txt` simply by typing
> `!458`.

> ## Variables in Loops {.challenge}
> 
> Suppose that `ls` initially displays:
> 
> ~~~
> fructose.dat    glucose.dat   sucrose.dat
> ~~~
> 
> What is the output of:
> 
> ~~~
> for datafile in *.dat
> do
>     ls *.dat
> done
> ~~~
>
> Now, what is the output of:
>
> ~~~
> for datafile in *.dat
> do
>	ls $datafile
> done
> ~~~
>
> Why do these two loops give you different outputs?

> ## Saving to a File in a Loop - Part One {.challenge}
>
> In the same directory, what is the effect of this loop?
> 
> ~~~
> for sugar in *.dat
> do
>     echo $sugar
>     cat $sugar > xylose.dat
> done
> ~~~
> 
> 1.  Prints `fructose.dat`, `glucose.dat`, and `sucrose.dat`, and the text from `sucrose.dat` will be saved to a file called `xylose.dat`.
> 2.  Prints `fructose.dat`, `glucose.dat`, and `sucrose.dat`, and the text from all three files would be 
>     concatenated and saved to a file called `xylose.dat`.
> 3.  Prints `fructose.dat`, `glucose.dat`, `sucrose.dat`, and
>     `xylose.dat`, and the text from `sucrose.dat` will be saved to a file called `xylose.dat`.
> 4.  None of the above.

> ## Saving to a File in a Loop - Part Two {.challenge}
>
> In another directory, where `ls` returns:
>
> ~~~
> fructose.dat    glucose.dat   sucrose.dat   maltose.txt
> ~~~
> 
> What would be the output of the following loop?
>
> ~~~
> for datafile in *.dat
> do
>     cat $datafile >> sugar.dat
> done
> ~~~
>
> 1.  All of the text from `fructose.dat`, `glucose.dat` and `sucrose.dat` would be 
>     concatenated and saved to a file called `sugar.dat`.
> 2.  The text from `sucrose.dat` will be saved to a file called `sugar.dat`.
> 3.  All of the text from `fructose.dat`, `glucose.dat`, `sucrose.dat` and `maltose.txt`
>     would be concatenated and saved to a file called `sugar.dat`.
> 4.  All of the text from `fructose.dat`, `glucose.dat` and `sucrose.dat` would be printed
>     to the screen and saved to a file called `sugar.dat`

> ## Doing a Dry Run {.challenge}
>
> Suppose we want to preview the commands the following loop will execute
> without actually running those commands:
>
> ~~~ {.bash}
> for file in *.dat
> do
>   analyze $file > analyzed-$file
> done
> ~~~
>
> What is the difference between the two loops below, and which one would we
> want to run?
>
> ~~~ {.bash}
> # Version 1
> for file in *.dat
> do
>   echo analyze $file > analyzed-$file
> done
> ~~~
>
> ~~~ {.bash}
> # Version 2
> for file in *.dat
> do
>   echo "analyze $file > analyzed-$file"
> done
> ~~~

> ## Nested Loops and Command-Line Expressions {.challenge}
>
> The `expr` does simple arithmetic using command-line parameters:
> 
> ~~~
> $ expr 3 + 5
> 8
> $ expr 30 / 5 - 2
> 4
> ~~~
> 
> Given this, what is the output of:
> 
> ~~~
> for left in 2 3
> do
>     for right in $left
>     do
>         expr $left + $right
>     done
> done
> ~~~

