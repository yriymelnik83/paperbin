# Практична робота №2 - рекурсивні оператори `map`, `filter` та `reduce`

## Вступ

Одним з найважливіших інструментів функціонального програмування є
*функції вищого порядку* - функції, що приймають інші функції в якості
аргументів, а також повертають функції як результат. Поширеним
застосуванням функцій вищого порядку є функції, що обходять структури
даних, які зберігають набори елементів (як впорядковані, та і
невпорядковані), певним чином застосовуючи функцію-аргумент до кожного
елемента у наборі і комбінуючи отримані значення. Набори даних
найчастіше представляють у вигляді списків та дерев, які описуються
рекурсивними математичними визначеннями, тому процедури, що здійснюють
обхід таких структур, легко реалізувати за допомогою рекурсії,
повторюючи рекурсивне визначення самої структури. Такі процедури часто
називають *рекурсивними операторами*. Найбільш відомими широкому
загалу рекурсивними операторами є оператори `map`, `filter` та
`reduce`, які можна знайти в стандартних бібліотеках багатьох сучасних
мов програмування:

| Функція  | Common Lisp         | Python                   | Java                  | JavaScript   |
|----------|---------------------|--------------------------|-----------------------|--------------|
| `map`    | `mapcar f s`        | `map(f, s)`              | `Stream.map(f)`       | `.map(f)`    |
| `filter` | `remove-if-not f s` | `filter(p, s)`           | `Stream.filter(p)`    | `.filter(p)` |
| `reduce`   | `reduce f s`        | `functools.reduce(f, s)` | `Stream.reduce(u, f)` | `.reduce(f)` |

Розглянемо варіанти цих функцій, що працюють зі списками:
  - функція `map` приймає аргументами функцію від одного аргумента <a href="https://www.codecogs.com/eqnedit.php?latex=F(x)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?F(x)" title="F(x)" /></a> та список

    > <a href="https://www.codecogs.com/eqnedit.php?latex=S&space;=&space;[s_1,&space;s_2,&space;...,&space;s_n]," target="_blank"><img src="https://latex.codecogs.com/gif.latex?S&space;=&space;[s_1,&space;s_2,&space;...,&space;s_n]," title="S = [s_1, s_2, ..., s_n]," /></a>

    і повертає результатом список

    > <a href="https://www.codecogs.com/eqnedit.php?latex=R&space;=&space;[F(s_1),&space;F(s_2),&space;...,&space;F(s_n)]," target="_blank"><img src="https://latex.codecogs.com/gif.latex?R&space;=&space;[F(s_1),&space;F(s_2),&space;...,&space;F(s_n)]," title="R = [F(s_1), F(s_2), ..., F(s_n)]," /></a>

    тобто список з такою ж кількістю елементів, як і вхідний список, але кожен елемент <a href="https://www.codecogs.com/eqnedit.php?latex=r_i" target="_blank"><img src="https://latex.codecogs.com/gif.latex?r_i" title="r_i" /></a> якого є значенням функції <a href="https://www.codecogs.com/eqnedit.php?latex=F(s_i)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?F(s_i)" title="F(s_i)" /></a> від відповідного елемента списка <a href="https://www.codecogs.com/eqnedit.php?latex=s" target="_blank"><img src="https://latex.codecogs.com/gif.latex?s" title="s" /></a>;

  - функція `filter` приймає аргументами предикат від одного аргумента <a href="https://www.codecogs.com/eqnedit.php?latex=P(x)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?P(x)" title="P(x)" /></a> та список

    > <a href="https://www.codecogs.com/eqnedit.php?latex=S&space;=&space;[s_1,&space;s_2,&space;...,&space;s_n]," target="_blank"><img src="https://latex.codecogs.com/gif.latex?S&space;=&space;[s_1,&space;s_2,&space;...,&space;s_n]," title="S = [s_1, s_2, ..., s_n]," /></a>

    і повертає результатом список

    > <a href="https://www.codecogs.com/eqnedit.php?latex=R&space;=&space;[s_i&space;|&space;P(s_i)&space;=&space;True]" target="_blank"><img src="https://latex.codecogs.com/gif.latex?R&space;=&space;[s_i&space;|&space;P(s_i)&space;=&space;True]" title="R = [s_i | P(s_i) = True]" /></a>

    тобто список, що містить лише ті елементи <a href="https://www.codecogs.com/eqnedit.php?latex=s_i" target="_blank"><img src="https://latex.codecogs.com/gif.latex?s_i" title="s_i" /></a> вхідного списка, значення предиката для яких <a href="https://www.codecogs.com/eqnedit.php?latex=P(s_i)&space;=&space;True" target="_blank"><img src="https://latex.codecogs.com/gif.latex?P(s_i)&space;=&space;True" title="P(s_i) = True" /></a> (у випадку *Common Lisp* - **не** `NIL`);

  - функція `reduce` приймає аргументами функцію від двох аргументів <a href="https://www.codecogs.com/eqnedit.php?latex=F(a,&space;x)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?F(a,&space;x)" title="F(a, x)" /></a>, список

    > <a href="https://www.codecogs.com/eqnedit.php?latex=S&space;=&space;[s_1,&space;s_2,&space;...,&space;s_n]," target="_blank"><img src="https://latex.codecogs.com/gif.latex?S&space;=&space;[s_1,&space;s_2,&space;...,&space;s_n]," title="S = [s_1, s_2, ..., s_n]," /></a>

    та необов'язкове (опційне) початкове значення <a href="https://www.codecogs.com/eqnedit.php?latex=u" target="_blank"><img src="https://latex.codecogs.com/gif.latex?u" title="u" /></a>, і повертає результатом значення

    > <a href="https://www.codecogs.com/eqnedit.php?latex=r_n&space;=&space;F(...F(F(F(u,&space;s_1),&space;s_2),&space;s_3)...,&space;s_n)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?r_n&space;=&space;F(...F(F(F(u,&space;s_1),&space;s_2),&space;s_3)...,&space;s_n)" title="r_n = F(...F(F(F(u, s_1), s_2), s_3)..., s_n)" /></a>

    тобто результат послідовного застосування функції <a href="https://www.codecogs.com/eqnedit.php?latex=F(a,&space;x)" target="_blank"><img src="https://latex.codecogs.com/gif.latex?F(a,&space;x)" title="F(a, x)" /></a> до попереднього проміжного значення <a href="https://www.codecogs.com/eqnedit.php?latex=r_i" target="_blank"><img src="https://latex.codecogs.com/gif.latex?r_i" title="r_i" /></a> та кожного елемента вхідного списку <a href="https://www.codecogs.com/eqnedit.php?latex=s_i" target="_blank"><img src="https://latex.codecogs.com/gif.latex?s_i" title="s_i" /></a>:

    > <a href="https://www.codecogs.com/eqnedit.php?latex=r_{i&plus;1}&space;=&space;F(r_i,&space;s_i)," target="_blank"><img src="https://latex.codecogs.com/gif.latex?r_{i&plus;1}&space;=&space;F(r_i,&space;s_i)," title="r_{i+1} = F(r_i, s_i)," /></a>

    де <a href="https://www.codecogs.com/eqnedit.php?latex=r_1&space;=&space;u" target="_blank"><img src="https://latex.codecogs.com/gif.latex?r_1&space;=&space;u" title="r_1 = u" /></a>, якщо <a href="https://www.codecogs.com/eqnedit.php?latex=u" target="_blank"><img src="https://latex.codecogs.com/gif.latex?u" title="u" /></a> наявне, або <a href="https://www.codecogs.com/eqnedit.php?latex=r_1&space;=&space;s_1" target="_blank"><img src="https://latex.codecogs.com/gif.latex?r_1&space;=&space;s_1" title="r_1 = s_1" /></a>.

Для того, щоб детальніше розібратись з роботою цих операторів, можна
поекспериментувати з стандартними функціями *Common Lisp*, що їх
реалізують (див. табличку вище).


## Завдання для реалізації

Необхідно **самостійно** реалізувати наступні функції, використовуючи
рекурсію для обходу списку та конструювання нових списків чи
повернення результату (аналогічно до лабораторної роботи 1):

``` lisp
;; % на початку назви кожної функції для того, щоб уникнути
;; конфліктів з існуючими стандартними фукнціями CL map та reduce.

(defun %map (f list)
  ...)

(defun %filter (f list)
  ...)

(defun %reduce (f list init)
  ...)
  
```


## Захист

Для захисту лабораторної роботи необхідно продемонструвати роботу
**власних реалізацій** всіх трьох функцій, що описані вище, для
випадків порожнього списку, списку з одним елементом, та списку з
будь-якою іншою кількістю елементів.
