---
layout: page
title: 유닉스 쉘(Unix Shell)
subtitle: 쉘 스크립트
minutes: 15
---
> ## 학습 목표 {.objectives}
>
> *   고정된 파일 집합에 하나 명령어 혹은 일련의 다수 명령어를 실행하는 쉘 스크립트를 작성한다.
> *   명령 라인에서 쉘 스크립트 실행한다.
> *   명령 라인에서 사용자가 정의한 파일 집합에 연산작업을 수행하는 쉘 스크립트 작성한다.
> *   사용자가 작성한 쉘 스크립트를 포함하는 파이프라인을 생성한다.

마침내 쉘을 그토록 강력한 프로그래밍 환경으로 만드는 것을 볼 준비가 되었다. 
자주 반복적으로 사용되는 명령어를 파일에 저장할 것이고, 
단 하나의 명령어를 타이핑함으써 나중에 이 모든 연산 작업을 다시 실행할 수 있다. 
역사적 이유로 파일에 저장된 명령어 꾸러미를 통상 **쉘 스크립트(shell script)**라고 부르지만,
실수로 그렇게 부르는 것은 아니다: 실제로 작은 프로그램이다.

`molecules/` 디렉토리로 돌아가서 `middle.sh` 파일에 다음 행을 추가해서 시작해 봅시다:

~~~ {.bash}
$ cd molecules
$ nano middle.sh
~~~
~~~ {.output}
head -15 octane.pdb | tail -5
~~~

앞서 작성한 파이프에 변형이다: `octane.pdb` 파일에서 11-15 행을 선택한다. 
기억할 것은 명령어로서 실행하지 *않고*: 명령어를 파일에 넣는다.

파일에 저장하면, 쉘로 하여금 파일에 담겨진 명령어를 실행할 수 있다. 
지금 쉘은 `bash`이고, 다음과 같이 명령어를 실행한다:

~~~ {.bash}
$ bash middle.sh
~~~
~~~ {.output}
ATOM      9  H           1      -4.502   0.681   0.785  1.00  0.00
ATOM     10  H           1      -5.254  -0.243  -0.537  1.00  0.00
ATOM     11  H           1      -4.357   1.252  -0.895  1.00  0.00
ATOM     12  H           1      -3.009  -0.741  -1.467  1.00  0.00
ATOM     13  H           1      -3.172  -1.337   0.206  1.00  0.00
~~~

아니나 다를까, 스크립트의 출력은 정확하게 파이프라인을 직접적으로 실행한 것과 동일하다.

> ## 텍스트 vs. 텍스트가 아닌 것 아무거나 {.callout}
>
> 종종 마이크로소프트 워드 혹은 리브르오피스 Writer 프로그램을 "텍스트 편집기"라고 부른다.
> 하지만, 프로그래밍에 대해서 조금더 주의를 기울일 필요가 있다. 
> 기본 디폴트로, 마이크로소프트 워드는 `.docx` 파일을 사용해서 텍스트를 저장할 뿐만 아니라,
> 글꼴, 제목, 등등의 서식 정보도 함께 저장한다. 
> 이런 추가 정보는 문자로 저장되지 않아서, `head` 같은 도구에게는 무의미하다: 
> `head` 같은 도구는 입력 파일에 문자, 숫자, 표준 컴퓨터 키보드 특수문자만이 포함되기를 예상한다.
> 따라서, 프로그램을 편집할 때, 일반 텍스트 편집기를 사용하거나, 
> 혹은 일반 텍스트로 파일을 저장하도록 주의한다.

만약 임의 파일에서 행을 선택하고자 한다면 어떨까요? 
파일 이름을 바꾸기 위해서 매번 `middle.sh`을 편집할 수 있지만, 
단순히 명령어를 다시 타이핑하는 것보다 아마 시간이 더 걸릴 것이다. 
대신에 `middle.sh`을 편집해서 `octane.pdb`을 `$1`으로 불리는 특수 변수로 변경하자:

~~~ {.bash}
$ cat middle.sh
~~~
~~~ {.output}
head -15 "$1" | tail -5
~~~

쉘 스크립트 내부에서, `$1`은 "명령 라인에 첫 파일 이름(혹은 다른 매개변수)"을 의미한다. 
이제 스크립트를 다음과 같이 바꿔 실행할 수 있다:

~~~ {.bash}
$ bash middle.sh octane.pdb
~~~
~~~ {.output}
ATOM      9  H           1      -4.502   0.681   0.785  1.00  0.00
ATOM     10  H           1      -5.254  -0.243  -0.537  1.00  0.00
ATOM     11  H           1      -4.357   1.252  -0.895  1.00  0.00
ATOM     12  H           1      -3.009  -0.741  -1.467  1.00  0.00
ATOM     13  H           1      -3.172  -1.337   0.206  1.00  0.00
~~~

혹은 다음과 같이 다른 파일에 대해 스크립트 프로그램을 실행한다:

~~~ {.bash}
$ bash middle.sh pentane.pdb
~~~
~~~ {.output}
ATOM      9  H           1       1.324   0.350  -1.332  1.00  0.00
ATOM     10  H           1       1.271   1.378   0.122  1.00  0.00
ATOM     11  H           1      -0.074  -0.384   1.288  1.00  0.00
ATOM     12  H           1      -0.048  -1.362  -0.205  1.00  0.00
ATOM     13  H           1      -1.183   0.500  -1.412  1.00  0.00
~~~


> ## 인자 주위를 이중 인용부호로 감싸기 {.callout}
>
> 파일명에 공백이 포함된 경우 이중 인용부호 안에 `$1`을 넣는다.
> 쉘이 여백을 인자를 구분하는데 사용해서 인자내부에 여백을 갖는 인자를 사용할 때 주의해야 된다.
> 만약 인용부호를 벗겨내고, `$1`에 `methyl butane.pdb`같은 파일명을 연장하면,
> 실질적으로 스크립트 명령어는 다음과 같이 된다: 
>
>     head -15 methyl butane.pdb | tail -5
>
> 상기 명령어 `head`가 두개 별도 파일, `methyl` 와 `butane.pdb`을 호출하는데,
> 아마도 처음에 의도한 바는 아닐 것이다.

하지만, 매번 줄 범위를 조정할 때마다 여전히 `middle.sh` 파일을 편집할 필요가 있다.
이 문제를 특수 변수 `$2` 와 `$3` 을 사용해서 고쳐보자:

~~~ {.bash}
$ cat middle.sh
~~~
~~~ {.output}
head "$2" "$1" | tail "$3"
~~~
~~~ {.bash}
$ bash middle.sh pentane.pdb -20 -5
~~~
~~~ {.output}
ATOM     14  H           1      -1.259   1.420   0.112  1.00  0.00
ATOM     15  H           1      -2.608  -0.407   1.130  1.00  0.00
ATOM     16  H           1      -2.540  -1.303  -0.404  1.00  0.00
ATOM     17  H           1      -3.393   0.254  -0.321  1.00  0.00
TER      18              1
~~~

제대로 동작하지만, `middle.sh` 쉘스크립트를 읽는 다른 사람은 잠시 시간을 들여, 
스크립트가 무엇을 수행하는지 알아내야 할지 모른다. 
스크립트를 상단에 **주석(comments)**을 추가해서 좀더 낫게 만들 수 있다:

~~~ {.bash}
$ cat middle.sh
~~~
~~~ {.output}
# 파일 중간에 있는 행을 골라낸다.
# 사용법: middle.sh filename -end_line -num_lines
head "$2" "$1" | tail "$3"
~~~

주석은 `#`문자로 시작하고 행 끝까지 간다. 
컴퓨터는 주석을 무시하지만, 
사람들이 스크립트를 이해하고 사용하는데 정말 귀중한 존재다.

만약 많은 파일을 단 하나 파이프라인으로 처리하고자 한다면 어떨까? 
예를 들어, `.pdb` 파일을 길이 순으로 정렬하려면, 다음과 같이 타이핑한다:

~~~ {.bash}
$ wc -l *.pdb | sort -n
~~~

`wc -l`은 파일에 행갯수를 출력하고(wc는 'word count'로 -l 플래그를 추가하면 'count lines'의미가 됨을 상기한다), `sort -n`은 숫자순으로 파일의 행갯수를 정렬한다. 
파일에 담을 수 있지만, 현재 디렉토리에 `.pdb` 파일만을 정렬한다. 
다른 유형의 파일에 대한 정렬된 목록을 얻으려고 한다면, 
스크립트에 이 모든 파일명을 얻는 방법이 필요하다. 
`$1`, `$2` 등등은 사용할 수 없는데, 
이유는 얼마나 많은 파일이 있는지를 예단할 수 없기 때문이다. 
대신에, 특수 변수 `$@`을 사용한다. 
`$@`은 "쉘 스크립트 모든 명령-라인 매개변수"를 의미한다. 
공백을 포함한 매개변수를 처리하려면 이중 인용부호 내부에 `$@`을 넣어야 된다
(`$@`은  `"$1"` `"$2"` ... 와 동치다).
예제가 다음에 있다:

~~~ {.bash}
$ cat sorted.sh
~~~
~~~ {.output}
wc -l "$@" | sort -n
~~~
~~~ {.bash}
$ bash sorted.sh *.pdb ../creatures/*.dat
~~~
~~~ {.output}
9 methane.pdb
12 ethane.pdb
15 propane.pdb
20 cubane.pdb
21 pentane.pdb
30 octane.pdb
163 ../creatures/basilisk.dat
163 ../creatures/unicorn.dat
~~~

> ## 왜 쉘 스크립트가 어떤 작업도 수행하지 않을까? {.callout}
>
> 스크립트가 파일 꾸러미를 처리하고 했지만, 어떠한 파일 이름도 주지 않는다면 무슨 일이 발생할까요? 
> 예를 들어, 만약 다음과 같이 타이핑한다면 어떨까요:
>
>     $ bash sorted.sh
>
> 하지만 `*.dat` (혹은 다른 어떤 것)를 타이핑하지 않는다면 어떨까요? 
> 이 경우 `$@`은 아무 것도 전개하지 않아서, 스크립트 내부의 파이프라인은 사실상 다음과 같다:
>
>     wc -l | sort -n
>
> 어떠한 파일이름도 주지 않아서, `wc`은 표준 입력을 처리하려 한다고 가정한다.
> 그래서, 단지 앉아서 사용자가 인터랙티브하게 어떤 데이터를 전달해주길 기다린다. 
> 하지만, 밖에서 보면 사용자에게 보이는 것은 스크립트가 거기 앉아서 정지한 것처럼 인다:
> 스크립트가 아무 일도 수행하지 않는 것처럼 보인다.

간단한 쉘 스크립트 학습을 마치기 전에 두개가 더 있다. 
다음과 같은 스크립트를 살펴보면:

~~~
wc -l "$@" | sort -n
~~~

아마도 스크립트가 어떤 작업을 수행하는지 퍼즐을 풀어 생각해 낼 수 있다. 
다른 한편으로 다음 스크립트를 살펴보면:

~~~
# 행갯수로 정렬해서 파일 목록을 화면에 출력한다.
wc -l "$@" | sort -n
~~~

사용자가 퍼즐을 풀어 생각해 낼 필요가 없다 --- 
상단 주석이 무엇을 수행하는지 자동으로 말해준다. 
한줄 혹은 두줄로 된 이와 같은 문서화는 앞으로 여러분 자신과 다른 사람이 여러분이 작성한 스크립트나 프로그램을 재사용하기 쉽게 한다. 
주의할 점은 매번 스크립트를 변경할 때마다, 주석이 여전히 정확한지 확인해야만 된다: 
독자에게 잘못된 방향을 주는 설명은 아무것도 없는 것보다 더 나쁠 수 있다.

둘째로, 유용한 무언가를 수행하는 일련의 명령어를 방금 실행했다고 가정하자 --- 
예를 들어, 논문에 사용될 그래프를 스크립트가 생성. 
필요하면 나중에 그래프를 다시 생성할 필요가 있어서, 
파일에 명령어를 저장하고자 한다. 
명령문을 다시 타이핑(그리고 잠재적으로 잘못 타이핑할 수도 있다)하는 대신에, 다음과 같이 할 수도 있다:

~~~ {.bash}
$ history | tail -4 > redo-figure-3.sh
~~~

`redo-figure-3.sh` 파일은 이제 다음을 담고 있다:

~~~
297 bash goostats -r NENE01729B.txt stats-NENE01729B.txt
298 bash goodiff stats-NENE01729B.txt /data/validated/01729.txt > 01729-differences.txt
299 cut -d ',' -f 2-3 01729-differences.txt > 01729-time-series.txt
300 ygraph --format scatter --color bw --borders none 01729-time-series.txt figure-3.png
~~~

명령어의 일련 번호를 제거하기 위해서 편집기에서 한동안 작업한 후에,
그림을 어떻게 생성했는지에 관한 정말 정확한 기록을 갖게 된다.

> ## 매긴 번호 지우기(Unnumbering) {.callout}
>
> 앞선 명령에 붙은 일련번호를 제거하기 위해
> Nelle은 `colrm` ("column removal(열 제거)"의 줄임말)를 사용할 수도 있다. 
> 매개변수는 입력에서 제거할 문자 범위가 된다.
>
> ~~~
> $ history | tail -5
>   173  cd /tmp
>   174  ls
>   175  mkdir bakup
>   176  mv bakup backup
>   177  history | tail -5
> $ history | tail -5 | colrm 1 7
> cd /tmp
> ls
> mkdir bakup
> mv bakup backup
> history | tail -5
> history | tail -5 | colrm 1 7
> ~~~

실제로, 대부분의 사람들은 쉘 프롬프트에서 몇번 명령어를 실행해서 올바르게 수행되는지를 확인한 다음, 재사용을 위해 파일에 저장한다. 
이런 유형의 작업이 데이터와 작업흐름(workflow)에서 발견한 것을 `history`를 호출해서 재사용할 수 있게 하고, 출력을 깔끔하게 하기 위해 약간의 편집을 하고 나서, 쉘 스크립트로 저장한다.


## Nelle의 파이프라인: 스크립트 생성하기

지도교수의 즉석 코멘트로부터 Nelle이 파일을 처리할 때 `goostats`에 몇개 추가 매개변수를 주어야 한다는 것을 깨닫게 되었다. 
수작업으로 모든 분석을 했다면 아마도 재난이었을 것이다. 
하지만, 루프덕분에 다시 작업하는데는 몇시간만 소요될 것이다.

하지만, 만약 무언가가 두번 수행될 필요가 있다면 아마도 세번 혹은 네번 수행될 필요가 있다는 것을 경험으로 알고 있다. 
편집기를 실행하고 다음과 같이 작성한다:

~~~
# Calculate reduced stats for data files at J = 100 c/bp.
for datafile in "$@"
do
    echo $datafile
    bash goostats -J 100 -r $datafile stats-$datafile
done
~~~

(매개변수 `-J 100`과 `-r`은 지도교수가 사용해야 된다고 알려준 것이다.) 
`do-stats.sh` 이름으로된 파일에 저장해서, 
다음과 같이 타이핑해서 첫번째 단계 분석을 다시 수행할 수 있다:

~~~ {.bash}
$ bash do-stats.sh *[AB].txt
~~~

또한 다음과 같이도 할 수 있다:

~~~ {.bash}
$ bash do-stats.sh *[AB].txt | wc -l
~~~

그렇게 해서 출력은 처리된 파일 이름이 아니라 처리된 파일의 숫자만 출력된다.

Nelle의 스크립트에서 주목할 한가지는 스크립트를 실행하는 사람이 무슨 파일을 처리할지를 결정하게 하는 것이다. 
스크립트를 다음과 같이 작성할 수 있다:

~~~
# Calculate reduced stats for  A and Site B data files at J = 100 c/bp.
for datafile in *[AB].txt
do
    echo $datafile
    bash goostats -J 100 -r $datafile stats-$datafile
done
~~~

장점은 이 스크립트는 항상 올바른 파일만을 선택한다: 'Z'파일을 제거했는지 기억할 필요가 없다. 
단점은 *항상* 이 파일만을 선택한다는 것이다 --- 
모든 파일('Z'를 포함하는 파일), 혹은 남극 동료가 생성한 'G', 'H' 파일에 대해서 스크립트를 편집하지 않고는 실행할 수 없다. 
좀더 모험적이라면, 스크립트를 변경해서 명령-라인 매개변수를 검증해서 만약 어떠한 매개변수도 제공되지 않았다면 `*[AB].txt`을 사용한다. 
물론, 이런 접근법은 유연성과 복잡성 사이에 서로 대립되는 요소 사이의 균형, 즉 트레이드오프(trade-off)를 야기한다.

> ## 쉘 스크립트에 변수 {.challenge}
>
> `molecules` 디렉토리에서, 다음 명령어를 포함하는 `script.sh`라는 쉘스크립트가 있다:
>
> ~~~
> head $2 $1
> tail $3 $1
> ~~~
> 
> `molecules` 디렉토리에서 다음 명령어를 타이핑한다:
>
> ~~~
> bash script.sh '*.pdb' -1 -1
> ~~~
> 
> 다음 출력물 결과 중 어떤 결과가 나올 것으로 예상하나요?
>
> 1. `molecules` 디렉토리에 있는 `*.pdb` 확장자를 갖는 각 파일에 첫번줄과 마지막줄 사이 모든 줄을 출력.
> 2. `molecules` 디렉토리에 있는 `*.pdb` 확장자를 갖는 각 파일에 첫번줄과 마지막 줄을 출력.
> 3. `molecules` 디렉토리에 있는 각 파일에 대한 첫번째와 마지막 줄을 출력.
> 4. `*.pdb` 를 감싸는 인용부호로 오류가 발생.
>

> ## 유일무이한 개체 목록으로 나열 {.challenge}
> 
> Leah는 수백개의 데이터 파일이 있고, 각각은 다음과 같은 형식을 가지고 있다:
> 
> ~~~
> 2013-11-05,deer,5
> 2013-11-05,rabbit,22
> 2013-11-05,raccoon,7
> 2013-11-06,rabbit,19
> 2013-11-06,deer,2
> 2013-11-06,fox,1
> 2013-11-07,rabbit,18
> 2013-11-07,bear,1
> ~~~
> 
> 임의 파일이름을 명령-라인 매개변수로 갖는 `species.sh` 이름의 쉘 스크립트를 작성하라. 
> `cut`, `sort`, `uniq`를 사용해서 각각의 파일별로 나오는 유일무이한 개체에 대한 목록을 화면에 출력하세요.
>

> ## 주어진 확장자를 가진 가장 긴 파일을 찾아낸다 {.challenge}
> 
> 매개변수로 디렉토리 이름과 파일이름 확장자를 갖는 `longest.sh`이름의 쉘 스크립트를 작성해서,
> 그 디렉토리에서 해당 확장자를 가지는 파일 중에 가장 긴 줄을 가진 파일이름을 화면에 출력하세요. 
> 예를 들어, 다음은
> 
> ~~~
> $ bash longest.sh /tmp/data pdb
> ~~~
> 
> `/tmp/data` 디렉토리에 `.pdb` 확장자를 가진 파일 중에 가장 긴 줄을 가진 파일이름을 화면에 출력한다.
> 

> ## 명령어를 실행하기 전에 history에 있는 명령어를 왜 기록해야 하는가? {.challenge}
> 
> 만약 다음 명령어를 실행하면:
> 
> ~~~
> history | tail -5 > recent.sh
> ~~~
> 
> 파일의 마지막 명령어는 `history` 명령어 자체다. 
> 즉, 쉘은 `history`에 실제로 실행하기 전에 명령어 이력(log)을 추가한다. 
> 사실, 쉘은 *항상* 명령어를 실행하기 전에 이력(log)에 명령어를 추가한다. 
> 그렇게 하는 이유가 무엇이라고 생각합니까?
> 

> ## 스크립트 독해 능력 {.challenge}
> 
> Joel의 `data` 디렉토리에는 `fructose.dat`, `glucose.dat`, `sucrose.dat` 파일 세개가 담겨 있다. 
> 만약 다음 행을 담고 있는 스크립트로 `bash example.sh *.dat`을 실행할 때, `example.sh` 이름의 스크립트가 무엇을 수행하는지 설명하세요:
> 
> ~~~
> # 스크립트 1
> echo *.*
> ~~~
> 
> ~~~
> # 스크립트 2
> for filename in $1 $2 $3
> do
>     cat $filename
> done
> ~~~
> 
> ~~~
> # 스크립트 3
> echo $@.dat
> ~~~
