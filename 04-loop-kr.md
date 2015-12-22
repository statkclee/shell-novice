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

> ## Follow the Prompt {.callout}
>
> The shell prompt changes from `$` to `>` and back again as we were
> typing in our loop. The second prompt, `>`, is different to remind
> us that we haven't finished typing a complete command yet. A semicolon, `;`, 
> can be used to separate two commands written on a single line.

We have called the variable in this loop `filename`
in order to make its purpose clearer to human readers.
The shell itself doesn't care what the variable is called;
if we wrote this loop as:

~~~ {.bash}
for x in basilisk.dat unicorn.dat
do
    head -3 $x
done
~~~

or:

~~~ {.bash}
for temperature in basilisk.dat unicorn.dat
do
    head -3 $temperature
done
~~~

it would work exactly the same way.
*Don't do this.*
Programs are only useful if people can understand them,
so meaningless names (like `x`) or misleading names (like `temperature`)
increase the odds that the program won't do what its readers think it does.

Here's a slightly more complicated loop:

~~~ {.bash}
for filename in *.dat
do
    echo $filename
    head -100 $filename | tail -20
done
~~~

The shell starts by expanding `*.dat` to create the list of files it will process.
The **loop body**
then executes two commands for each of those files.
The first, `echo`, just prints its command-line parameters to standard output.
For example:

~~~ {.bash}
$ echo hello there
~~~

prints:

~~~ {.output}
hello there
~~~

In this case,
since the shell expands `$filename` to be the name of a file,
`echo $filename` just prints the name of the file.
Note that we can't write this as:

~~~ {.bash}
for filename in *.dat
do
    $filename
    head -100 $filename | tail -20
done
~~~

because then the first time through the loop,
when `$filename` expanded to `basilisk.dat`, the shell would try to run `basilisk.dat` as a program.
Finally,
the `head` and `tail` combination selects lines 81-100 from whatever file is being processed.

> ## Spaces in Names {.callout}
> 
> Filename expansion in loops is another reason you should not use spaces in filenames.
> Suppose our data files are named:
> 
> ~~~
> basilisk.dat
> red dragon.dat
> unicorn.dat
> ~~~
> 
> If we try to process them using:
> 
> ~~~
> for filename in *.dat
> do
>     head -100 $filename | tail -20
> done
> ~~~
> 
> then the shell will expand `*.dat` to create:
> 
> ~~~
> basilisk.dat red dragon.dat unicorn.dat
> ~~~
> 
> With older versions of Bash,
> or most other shells,
> `filename` will then be assigned the following values in turn:
> 
> ~~~
> basilisk.dat
> red
> dragon.dat
> unicorn.dat
> ~~~
>
> That's a problem: `head` can't read files called `red` and `dragon.dat`
> because they don't exist,
> and won't be asked to read the file `red dragon.dat`.
> 
> We can make our script a little bit more robust
> by **quoting** our use of the variable:
> 
> ~~~
> for filename in *.dat
> do
>     head -100 "$filename" | tail -20
> done
> ~~~
>
> but it's simpler just to avoid using spaces (or other special characters) in filenames.

Going back to our original file copying problem,
we can solve it using this loop:

~~~ {.bash}
for filename in *.dat
do
    cp $filename original-$filename
done
~~~

This loop runs the `cp` command once for each filename.
The first time,
when `$filename` expands to `basilisk.dat`,
the shell executes:

~~~ {.bash}
cp basilisk.dat original-basilisk.dat
~~~

The second time, the command is:

~~~ {.bash}
cp unicorn.dat original-unicorn.dat
~~~

> ## Measure Twice, Run Once {.callout}
> 
> A loop is a way to do many things at once --- or to make many mistakes at
> once if it does the wrong thing. One way to check what a loop *would* do
> is to echo the commands it would run instead of actually running them.
> For example, we could write our file copying loop like this:
> 
> ~~~
> for filename in *.dat
> do
>     echo cp $filename original-$filename
> done
> ~~~
> 
> Instead of running `cp`, this loop runs `echo`, which prints out:
> 
> ~~~
> cp basilisk.dat original-basilisk.dat
> cp unicorn.dat original-unicorn.dat
> ~~~
> 
> *without* actually running those commands. We can then use up-arrow to
> redisplay the loop, back-arrow to get to the word `echo`, delete it, and
> then press "enter" to run the loop with the actual `cp` commands. This
> isn't foolproof, but it's a handy way to see what's going to happen when
> you're still learning how loops work.

## Nelle's Pipeline: Processing Files

Nelle is now ready to process her data files.
Since she's still learning how to use the shell,
she decides to build up the required commands in stages.
Her first step is to make sure that she can select the right files --- remember,
these are ones whose names end in 'A' or 'B', rather than 'Z'. Starting from her home directory, Nelle types:

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

Her next step is to decide
what to call the files that the `goostats` analysis program will create.
Prefixing each input file's name with "stats" seems simple,
so she modifies her loop to do that:

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

