---
## Front matter
title: "Лабораторная работа №3"
subtitle: "Модель боевых действий"
author: "Горяйнова Алёна"

## Generic otions
lang: ru-RU
toc-title: "Содержание"

## Bibliography
bibliography: bib/cite.bib
csl: pandoc/csl/gost-r-7-0-5-2008-numeric.csl

## Pdf output format
toc: true # Table of contents
toc-depth: 2
lof: true # List of figures
lot: false # List of tables
fontsize: 12pt
linestretch: 1.5
papersize: a4
documentclass: scrreprt
## I18n polyglossia
polyglossia-lang:
  name: russian
  options:
	- spelling=modern
	- babelshorthands=true
polyglossia-otherlangs:
  name: english
## I18n babel
babel-lang: russian
babel-otherlangs: english
## Fonts
mainfont: PT Serif
romanfont: PT Serif
sansfont: PT Sans
monofont: PT Mono
mainfontoptions: Ligatures=TeX
romanfontoptions: Ligatures=TeX
sansfontoptions: Ligatures=TeX,Scale=MatchLowercase
monofontoptions: Scale=MatchLowercase,Scale=0.9
## Biblatex
biblatex: true
biblio-style: "gost-numeric"
biblatexoptions:
  - parentracker=true
  - backend=biber
  - hyperref=auto
  - language=auto
  - autolang=other*
  - citestyle=gost-numeric
## Pandoc-crossref LaTeX customization
figureTitle: "Рис."
tableTitle: "Таблица"
listingTitle: "Листинг"
lofTitle: "Список иллюстраций"
lotTitle: "Список таблиц"
lolTitle: "Листинги"
## Misc options
indent: true
header-includes:
  - \usepackage{indentfirst}
  - \usepackage{float} # keep figures where there are in the text
  - \floatplacement{figure}{H} # keep figures where there are in the text
---

# Цель работы

Построить модель боевых действий на языке прогаммирования Julia и посредством ПО OpenModelica.

# Задание

Между страной $X$ и страной $Y$ идет война. Численность состава войск
исчисляется от начала войны, и являются временными функциями $x(t)$ и $y(t)$. В
начальный момент времени страна $X$ имеет армию численностью 80 000 человек,
а в распоряжении страны $Y$ армия численностью в 115 000 человек. Для упрощения
модели считаем, что коэффициенты $a, b, c, h$ постоянны. Также считаем $P(t)$ и $Q(t)$ непрерывные функции.

Построить графики изменения численности войск армии $X$ и армии $Y$ для  следующих случаев:

1. Модель боевых действий между регулярными войсками
$$\begin{cases}
    \dfrac{dx}{dt} = -0.3x(t)- 0.56y(t)+sin(t+10)\\
    \dfrac{dy}{dt} = -0.68x(t)- 0.33y(t)+cos(t+10)
\end{cases}$$

2. Модель ведение боевых действий с участием регулярных войск и партизанских отрядов

$$\begin{cases}
    \dfrac{dx}{dt} = -0.31x(t)-0.77y(t)+sin(2t+10)\\
    \dfrac{dy}{dt} = -0.67x(t)y(t)-0.51y(t)+cos(t+10)
\end{cases}$$

# Теоретическое введение

Законы Ланчестера (законы Осипова — Ланчестера) — математическая формула для расчета относительных сил пары сражающихся сторон — подразделений вооруженных сил. В статье «Влияние численности сражающихся сторон на их потери», опубликованной журналом «Военный сборник» в 1915 году, генерал-майор Корпуса военных топографов М. П. Осипов описал математическую модель глобального вооружённого противостояния, практически применяемую в военном деле при описании убыли сражающихся сторон с течением времени и, входящую в математическую теорию исследования операций, на год опередив английского математика Ф. У. Ланчестера. Мировая война, две революции в России не позволили новой власти заявить в установленном в научной среде порядке об открытии царского офицера.

Уравнения Ланчестера — это дифференциальные уравнения, описывающие зависимость между силами сражающихся сторон A и D как функцию от времени, причем функция зависит только от A и D.

В 1916 году, в разгар первой мировой войны, Фредерик Ланчестер разработал систему дифференциальных уравнений для демонстрации соотношения между противостоящими силами. Среди них есть так называемые Линейные законы Ланчестера (первого рода или честного боя, для рукопашного боя или неприцельного огня) и Квадратичные законы Ланчестера (для войн начиная с XX века с применением прицельного огня, дальнобойных орудий, огнестрельного оружия). В связи с установленным приоритетом в англоязычной литературе наметилась тенденция перехода от фразы «модель Ланчестера» к «модели Осипова — Ланчестера» [@wiki:bash].

# Выполнение лабораторной работы

## Модель боевых действий между регулярными войсками

$$\begin{cases}
    \dfrac{dx}{dt} = -0.3x(t)- 0.56y(t)+sin(t+10)\\
    \dfrac{dy}{dt} = -0.68x(t)- 0.33y(t)+cos(t+10)
\end{cases}$$

Потери, не связанные с боевыми действиями, описывают члены $-0.3x(t)- 0.56y(t)$ (коэффиценты при $x$ и $y$ - это величины, характеризующие степень влияния различных факторов на потери), члены $-0.68x(t)- 0.33y(t)$ отражают потери на поле боя (коэффиценты при  $x$ и $y$ указывают на эффективность боевых действий со стороны у и х соответственно). Функции P(t) = sin(t+10), Q(t) = cos(t+10)учитывают
возможность подхода подкрепления к войскам Х и У в течение одного дня.

Для начала построим эту модель на Julia:




В результате получаем следующий график (рис. [-@fig:001]):

![Модель боевых действий  между регулярными войсками](image/1.png){#fig:001 width=70%}

Из графика видно, что у обеих армий потери одинаковые, поэтому армия Y в любом случае в плюсе.

Теперь давайте построим эту же модель посредством OpenModelica.

В результате получаем слудющий график изменения численности армий (рис. [-@fig:002]):

![Модель боевых действий  между регулярными войсками](image/2.png){#fig:002 width=70%}

Здесь все также видно, одинаковое уменьшение числа войск.

Заметим, что график, построенный на Julia, и график из OpenModelica ничем не отлоичаются.

## Модель ведение боевых действий с участием регулярных войск и партизанских отрядов

Во втором случае в борьбу добавляются партизанские отряды. Нерегулярные
войска в отличии от постоянной армии менее уязвимы, так как действуют скрытно,
в этом случае сопернику приходится действовать неизбирательно, по площадям,
занимаемым партизанами. Поэтому считается, что тем потерь партизан,
проводящих свои операции в разных местах на некоторой известной территории,
пропорционален не только численности армейских соединений, но и численности
самих партизан. В результате модель принимает вид:

$$\begin{cases}
    \dfrac{dx}{dt} = -0.31x(t)-0.77y(t)+sin(2t+10)\\
    \dfrac{dy}{dt} = -0.67x(t)y(t)-0.51y(t)+cos(t+10)
\end{cases}$$

В этой системе все величины имею тот же смысл, что и в первой модели.

Построим модель на Julia:

В результате получаем слудющий график изменения численности армий (рис. [-@fig:003]):

![Модель боевых действий с участием регулярных войск и партизанских отрядов](image/3.png){#fig:003 width=70%}

Здесь выиграла армия X, причем численность армии Y уменьшается до нуля практически моментально. Заметим также, что  даже после того как армия X победила (то есть численность армии Y стала равна 0), ее численность продолжает уменьшаться на заданном интервале (поскольку у нас есть коэффиценты, характеризующие степень влияния различных факторов на потери). На данном графике сложно отследить, как происходило уменьшение численности армии Y, поэтому давайте возьмем временной интервал поменьше, чтобы было более наглядно, как умирает армия Y (рис. [-@fig:004]):

![Модель боевых действий с участием регулярных войск и партизанских отрядов](image/4.png){#fig:004 width=70%}

Теперь выполним построение второй модели в Open Modelica. Код выглядит следующим образом:


В результате получается такой график:

![Модель боевых действий с участием регулярных войск и партизанских отрядов](image/5.png){#fig:005 width=70%}

Из которого видно, что выиграла армия X, причем моментально, но тем не менее после победы ее численность продолжает уменьшаться.


# Выводы

В процессе выполнения данной лабораторной работы я построила модель боевых действий на языке прогаммирования Julia и посредством ПО OpenModelica, а также провела сравнительный анализ.

# Список литературы{.unnumbered}

::: {#refs}
:::
