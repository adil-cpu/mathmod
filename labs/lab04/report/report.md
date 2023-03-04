---
## Front matter
title: "Лабораторная работа №4"
subtitle: "Математическое моделирование"
author: "Мухамедияр Адиль"

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
lot: true # List of tables
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

# Вводная часть

## Объект и предмет исследования

- Модель гармонических колебаний
- Язык программирования Julia
- Система моделирования Openmodelica

## Цели и задачи

- Построение математической модели колебаний гармонического осциллятора
- Визуализация модели на языках Julia и OpenModelica

## Материалы и методы

- Язык программирования Julia
- Пакеты "Plots", "DifferentialEquations"

 ## Теоретическая справка
 
Движение грузика на пружинке, маятника, заряда в электрическом контуре, а также эволюция во времени многих систем в физике, химии, биологии и других науках при определенных предположениях можно описать одним и тем же дифференциальным уравнением, которое в теории колебаний выступает в качестве основной модели. Эта модель называется линейным гармоническим осциллятором.

Уравнение свободных колебаний гармонического осциллятора имеет следующий вид: $$\ddot{x} + 2\gamma\dot{x} + \omega_{0}^{2}x = 0$$, где $x$ --- переменная, описывающая состояние системы (например, смещение грузика), $\gamma$ --- параметр, характеризующий потери энергии (например, трение в механической системе), $\omega_{0}$ --- собственная частота колебаний.

Зададим начальные условия: $\begin{cases}x(t_{0}) = x_{0}\\dot{x}(t_{0}) = y_{0}\end{cases}$.

Для первого случая потери в системе отсутствуют ($\gamma = 0$), поэтому уравнение принимает вид: $\ddot{x} + \omega_{0}^{2}x = 0$. Представим его в виде системы: $\begin{cases}\dot{x} = y\\\dot{y} = -\omega_{0}^{2}x\end{cases}$.

Для второго случая появляются потери в системе, и система уравнений принимает вид: $\begin{cases}\dot{x} = y\\\dot{y} = -2\gamma\dot{x}-\omega_{0}^{2}x\end{cases}$.

Для третьего случая помимо потерь на систему влияет внешняя сила, описываемая функцией $P(t)$. Тогда система уравнений примет вид: $\begin{cases}\dot{x} = y\\\dot{y} = P(t)-2\gamma\dot{x}-\omega_{0}^{2}x\end{cases}$.

Для всех этих систем начальные условия примут вид: $\begin{cases}x(t_{0}) = x_{0}\\y(t_{0}) = y_{0}\end{cases}$


# Содержание лабораторной работы


## Постановка задачи

### Вариант № 6

Постройте фазовый портрет гармонического осциллятора и решение уравнения
гармонического осциллятора для следующих случаев
1. Колебания гармонического осциллятора без затуханий и без действий внешней
силы $$\ddot{x} + 8x = 0$$
2. Колебания гармонического осциллятора c затуханием и без действий внешней
силы $$\ddot{x} + 4\dot{x} + 3x = 0$$
3. Колебания гармонического осциллятора c затуханием и под действием внешней
силы $$\ddot{x} + 3\dot{x} + 6x = sin(0.5t)$$

На интервале t ∈ [0; 45] (шаг 0.05) с начальными условиями $$\x_{0} = -1, \y_{0} = 0$$
 


# Решение программными средствами

**1 случай на *Julia***
```
using Plots
using DifferentialEquations
const x0 = -1
const y0 = 0
const omega = 8
const gamma = 0
P(t) = 0
T = (0, 45)
u0 = [x0, y0]
p1 = (omega)
p2 = (omega, gamma)
function F1(du, u, p, t)
    omega = p
    du[1] = u[2]
    du[2] = -omega*u[1]
end

prob1 = ODEProblem(F1, u0, T, p1)
sol1 = solve(prob1, dtmax=0.05)
plt = plot(sol1, vars=(2,1), color=:red, label="Фазовый портрет", title="1 случай", xlabel="x-axis", ylabel="y-axis")
plt2 = plot(sol1, vars=(0,1), color=:blue, label="x(t)", title="1 случай", xlabel="t")
plot!(plt2, sol1, vars=(0,2), color=:green, label="y(t)")
savefig(plt, "Julia1_1.png")
savefig(plt2, "Julia1_2.png")
```
Получаем графики решения уравнения (рис. @fig:001) и фазовый портрет гармонического осциллятора (@fig:002). Из замкнутости фазового портрета можно сделать вывод о консервативности системы, то есть об отсутствии влияния на систему со стороны внешних сил.

![Первый случай на Julia](image/Julia1_1.png){#fig:001 width=70%}

![Первый случай на Julia](image/Julia1_2.png){#fig:002 width=70%}

**2 случай на *Julia***
```
using Plots
using DifferentialEquations

const x0 = -1
const y0 = 0
const omega = 3
const gamma = 4

P(t) = 0

T = (0, 45)

u0 = [x0, y0]

p1 = (omega)
p2 = (omega, gamma)

function F2(du, u, p, t)
    omega, gamma = p
    du[1] = u[2]
    du[2] = -gamma*du[1]-omega*u[1]
end

prob2 = ODEProblem(F2, u0, T, p2)
sol2 = solve(prob2, dtmax=0.05)

plt = plot(sol2, vars=(2,1), color=:red, label="Фазовый портрет", title="2 случай", xlabel="x-axis", ylabel="y-axis")
plt2 = plot(sol2, vars=(0,1), color=:blue, label="x(t)", title="2 случай", xlabel="t")
plot!(plt2, sol2, vars=(0,2), color=:green, label="y(t)")

savefig(plt, "Julia2_1.png")
savefig(plt2, "Julia2_2.png")
```
Для второго случая получаем графики решения уравнения (рис. @fig:003) и фазовый портрет гармонического осциллятора (@fig:004). Фазовый портрет незамкнут, отсюда можно сделать вывод о том, что система не является консервативной, то есть на нее влияет какая-то внешняя сила, например, сила трения.

![Второй случай на Julia](image/Julia2_1.png){#fig:003 width=70%}

![Второй случай на Julia](image/Julia2_2.png){#fig:004 width=70%}
**3 случай на *Julia***
```
using Plots
using DifferentialEquations

const x0 = -1
const y0 = 0
const omega = 6
const gamma = 3

P(t) = sin(0.5*t)

T = (0, 45)

u0 = [x0, y0]

p1 = (omega)
p2 = (omega, gamma)

function F3(du, u, p, t)
    omega, gamma = p
    du[1] = u[2]
    du[2] = P(t)-gamma*du[1]-omega*u[1]
end

prob3 = ODEProblem(F3, u0, T, p2)
sol3 = solve(prob3, dtmax=0.05)

plt = plot(sol3, vars=(2,1), color=:red, label="Фазовый портрет", title="3 случай", xlabel="x-axis", ylabel="y-axis")
plt2 = plot(sol3, vars=(0,1), color=:blue, label="x(t)", title="3 случай", xlabel="t")
plot!(plt2, sol3, vars=(0,2), color=:green, label="y(t)")

savefig(plt, "Julia3_1.png")
savefig(plt2, "Julia3_2.png")
```
Получаем графики решения уравнения (рис. @fig:005) и фазовый портрет гармонического осциллятора (@fig:006) для третьего случая. Из незамкнутости графика фазового портрета видно, что система неконсервативна, следовательно на нее действуют внешние силы.

![Третий случай на Julia](image/Julia3_1.png){#fig:005 width=70%}

![Третий случай на Julia](image/Julia3_2.png){#fig:006 width=70%}

Теперь пишем на OpenModelica:

**1 случай на *OpenModelica***

```
model Lab4
parameter Real x0 = -1;
parameter Real y0 = 0;
parameter Real omega = 8;

Real x(start=x0);
Real y(start=y0);

equation

der(x) = y;
der(y) = -omega*x;
end Lab4;
```
Видим графики решения уравнения (рис. @fig:007) и фазовый портрет гармонического осциллятора (@fig:008). Графики идентичны графикам, полученным в Julia. (рис. @fig:007)
![Первый случай на OpenModelica](image/1_1.jpg){#fig:007 width=70%}
![Первый случай на OpenModelica](image/1_2.jpg){#fig:008 width=70%}

**2 случай на *OpenModelica***

```
model Lab4
parameter Real x0 = -1;
parameter Real y0 = 0;
parameter Real omega = 3;
parameter Real gamma = 4;

Real x(start=x0);
Real y(start=y0);

equation

der(x) = y;
der(y) = -gamma*der(x)-omega*x;
end Lab4;
```
Видим графики решения уравнения (рис. @fig:009) и фазовый портрет гармонического осциллятора (@fig:010).
![Второй случай на OpenModelica](image/2_1.jpg){#fig:009 width=70%}
![Второй случай на OpenModelica](image/2_2.jpg){#fig:010 width=70%}
**3 случай на *OpenModelica***

```
model Lab4
parameter Real x0 = -1;
parameter Real y0 = 0;
parameter Real omega = 6;
parameter Real gamma = 3;

Real P;
Real x(start=x0);
Real y(start=y0);

equation
P = sin(0.5*time);
der(x) = y;
der(y) = P-gamma*der(x)-omega*x;
end Lab4;
```
Видим графики решения уравнения (рис. @fig:011) и фазовый портрет гармонического осциллятора (@fig:012).
![Третий случай на OpenModelica](image/3_1.jpg){#fig:011 width=70%}
![Третий случай на OpenModelica](image/3_2.jpg){#fig:012 width=70%}
## Вывод
Во время решения данной лабораторной работы мы поняли как работать с математической маделью гармоническим осциллятором. Изучили разные случаи решения как на Julia, так и на OpenModelica.