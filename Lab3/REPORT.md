#№ Отчет по лабораторной работе №3
## по курсу "Логическое программирование"

## Решение задач методом поиска в пространстве состояний

### студент: Иванопуло А.Б.

## Результат проверки

| Преподаватель     | Дата         |  Оценка       |
|-------------------|--------------|---------------|
| Сошников Д.В. |              |               |
| Левинская М.А.|              |               |

> *Комментарии проверяющих (обратите внимание, что более подробные комментарии возможны непосредственно в репозитории по тексту программы)*


## Введение

Метод поиска в пространстве состояний - один из методов решения задач в декларативных языках программирования. Хорошим примером таких задач могут послужить поиск всевозможных путей в графе из одного узла в другой или решение задания типа "волк-коза-капуста", где необходимо из начального состояния найти путь в конечное.

Подобные задачи с легкостью сводятся к задачам логики предикатов, для решения которых отлично подходит язык Prolog.

## Задание
Три миссионера и три каннибала хотят переправиться с левого берега реки на правый. Как это сделать за минимальное число шагов, если в их распоряжении имеется трехместная лодка и ни при каких обстоятельствах (в лодке или на берегу) миссионеры не должны оставаться в меньшинстве.

## Принцип решения

В решении используется поиск в глубину, в ширину и в глубину с итеративным погружением. Состояние выглядит следующим образом: [M, C, B], где M и C - число миссионеров и каннибалов на левом берегу, а B - положение лодки (у левого берега или у правого).

Из текущего состояния проверяются все возможные переходы в другие состояния, а так же то, что то состояние, в которое мы можем перейти, уже не встречалось ранее.

Поиск в глубину:
```prolog
solve_dfs:-
	path_dfs([3,3,left],[0,0,right],[[3,3,left]],MoveList),
	nl, write('Список ходов:'), nl,
	print_res(MoveList).
```
Нахождение пути:
```prolog
path_dfs([A,B,C],[D,E,F],Visited,Moves1):-
	move([A,B,C],[I,J,K],Action),
	write('Try: '), write([I,J,K]),
		write(' '), write(Action), nl,
	safe([I,J,K]),
	not(member([I,J,K],Visited)),
	path_dfs([I,J,K],[D,E,F],[[I,J,K]|Visited],Moves2),
	Moves1 = [[[A,B,C],[I,J,K],Action]|Moves2].
```
Так же при нахождении пути (когда оказались в состоянии [0,0,right]), необходимо прекратить поиск решений из этого состояния:
```prolog
path_dfs([0,0,right],[0,0,right],_,[]).
```
Поиск в ширину работает по следующему принципу:

1.Берем путь из головы очереди.
2.Проверяем, куда можно еще пойти из этого пути.
3.Результат добавляем в конец очереди.

```prolog
solve_bfs:-
	path_bfs([[[3,3,left]]],[0,0,right],MoveList),
	nl, write('Список ходов:'), nl,
	print_bfs(MoveList).
path_bfs([[[0,0,right]|T]|_],[0,0,right],[[0,0,right]|T]).
path_bfs([[P|T]|Q],[I,J,K],R):-
	findall(X,help_bfs([P|T],X),L),
	append(Q,L,QQ), !,
	path_bfs(QQ,[I,J,K],R).
```
## Результаты
1) Поиск в глубину
```
Try: [1,0,left] 1 миссионер переплывает обратно
Try: [2,0,left] 2 миссионера переплывают обратно
Try: [1,1,left] 1 миссионер и 1 каннибал переплывают обратно
Try: [0,1,right] 1 миссионер переплывает реку
Try: [0,0,right] 1 миссионер и 1 каннибал переплывают реку
Try: [1,0,right] 1 каннибал переплывает реку
Try: [0,1,left] 1 каннибал переплывает обратно
Try: [0,0,right] 1 каннибал переплывает реку
Try: [0,2,left] 2 каннибала переплывают обратно
Try: [2,1,left] 2 миссионера и 1 каннибал переплывают обратно
Try: [3,0,left] 3 миссионера переплывают реку обратно
Try: [2,0,right] 1 миссионер переплывает реку
Try: [1,0,right] 2 миссионера переплывают реку
Try: [0,0,right] 3 миссионера переплывают реку
Try: [0,3,left] 3 каннибала переплывают реку обратно
Try: [0,3,left] 2 каннибала переплывают обратно
Try: [2,2,left] 2 миссионера и 1 каннибал переплывают обратно
Try: [3,1,left] 3 миссионера переплывают реку обратно
Try: [0,0,right] 3 каннибала переплывают реку
```
2) Поиск в ширину
```
Список ходов:
[3,3,left] -> [2,2,right] : 1 миссионер и 1 каннибал переплывают реку
[2,2,right] -> [3,2,left] : 1 миссионер переплывает обратно
[3,2,left] -> [3,0,right] : 2 каннибала переплывают реку
[3,0,right] -> [3,1,left] : 1 каннибал переплывает обратно
[3,1,left] -> [1,1,right] : 2 миссионера переплывают реку
[1,1,right] -> [2,2,left] : 1 миссионер и 1 каннибал переплывают обратно
[2,2,left] -> [0,2,right] : 2 миссионера переплывают реку
[0,2,right] -> [0,3,left] : 1 каннибал переплывает обратно
[0,3,left] -> [0,0,right] : 3 каннибала переплывают реку
```
3) Поиск с итеративным погружением (максимальная глубина = 20)
```
Try: [1,0,left] 1 миссионер переплывает обратно
Try: [2,0,left] 2 миссионера переплывают обратно
Try: [1,1,left] 1 миссионер и 1 каннибал переплывают обратно
Try: [0,1,right] 1 миссионер переплывает реку
Try: [0,0,right] 1 миссионер и 1 каннибал переплывают реку
Try: [1,0,right] 1 каннибал переплывает реку
Try: [0,1,left] 1 каннибал переплывает обратно
Try: [0,0,right] 1 каннибал переплывает реку
Try: [0,2,left] 2 каннибала переплывают обратно
Try: [2,1,left] 2 миссионера и 1 каннибал переплывают обратно
Try: [3,0,left] 3 миссионера переплывают реку обратно
Try: [2,0,right] 1 миссионер переплывает реку
Try: [1,0,right] 2 миссионера переплывают реку
Try: [0,0,right] 3 миссионера переплывают реку
Try: [0,3,left] 3 каннибала переплывают реку обратно
Try: [0,3,left] 2 каннибала переплывают обратно
Try: [2,2,left] 2 миссионера и 1 каннибал переплывают обратно
Try: [3,1,left] 3 миссионера переплывают реку обратно
Try: [0,0,right] 3 каннибала переплывают реку
```
! Алгоритм поиска |  Длина найденного первым пути  |  Время работы  |
|-------------------------------------------------------------------|
| В глубину       |                 11               |      30 ms.          |
| В ширину        |                 5               |      30 ms.          |
| ID              |                 11               |      30 ms.          |

P.S. Почему-то поехала таблица

## Выводы

Лабораторная работа показывает ещё одно из применений различных видов поиска в графах: нахождение решений в пространстве состояний для решения логических задачах.

Поиск в глубину используется, когда необходимо найти хотя бы какое-либо решение, в то время как поиск в ширину подразумевает необходимость найти решение как можно быстрее, кратчайший путь. Поиск в ширину в некоторых случаях требует значительных затрат памяти, что может негативно сказаться на работоспособности программы.

Поиск в глубину с итеративным погружением используется в тех же случаях, что и обычный поиск в глубину с одним лишь отличием: необходимостью прекратить поиск, если глубина погружения превысила определенное максимальное значение. Данный вид поиска также рационально использовать, если заранее известно, что в дереве точно содержится необходимое для нас решение на какой-либо определенной высоте.




