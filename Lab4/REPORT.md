#№ Отчет по лабораторной работе №4
## по курсу "Логическое программирование"

## Обработка естественного языка

### студент: Москвин А. А.

## Результат проверки

| Преподаватель     | Дата         |  Оценка       |
|-------------------|--------------|---------------|
| Сошников Д.В. |              |               |
| Левинская М.А.|              |               |

> *Комментарии проверяющих (обратите внимание, что более подробные комментарии возможны непосредственно в репозитории по тексту программы)*


## Введение

Задача анализа естественных языков не решена до конца, однако они являются одними из самых распространенных, которые решаются языками логического прогарммирования. Для этого обычно используют расширенные сети переходов либо разностные списки.

При анализе искуственных языков можно выделить несколько основных этапов:

1. Выделение в тексте токенов, смысловых единиц
2. Построение дерева разбора
3. Трансляция в код стековой машины
4. Интерпретатор стековой машины
Язык пролог является довольно удобным для решения подобного рода задач в виду простоты реализации перебора с возвратами и удобства манипулированием символьной информацией.

## Задание

Перенесите сюда условие задачи - это упростит проверку и чтение отчета.

## Принцип решения
Основной предикат: `an_morph(L, morph(Pristavka, Koren, Okonchanie, Rod, Chislo))`. Первый аргумент - список букв слова, например, `['в','ы','у','ч','и','л','а']`.
Производится разбиение списка всеми возможными способами на 3 части при помощи предиката `append`.
Затем, при помощи предикатов `an_pristavka(X, prist(X1))`, `an_koren(X, prist(X1))`, `okonchanie_list` производится проверка на соответствие какой-либо из морфологических частей слова, которые заранее помещены в словари наподобие
```prolog
okonchanie_list(L):-
L=[
['и','л']:'ил',
['и','т']:'ит',
['и','л','а']:'ила',
['и','л','о']:'ило',
['и','л','и']:'или'
].
```

Род и число определяется по последней букве слова.

## Результаты

```
?- an_morph(['п','е','р','е','у','ч','и','л','а'], R).
R = morph(prist(пере), kor(уч), okon(ила), rod(женский), chislo(единственное)) ;
false.

?- an_morph(['з','а','у','ч','и','л','и'], R).
R = morph(prist(за), kor(уч), okon(или), rod(неопределенный), chislo(множественное)) ;
false.
```

## Выводы

Основным предназначением грамматического разбора является выделение необходимой информации из входного сообщения. Достаточно описать правила, по которым мы будем определять, содержится ли в предложении необходимая для нас информация или нет.

Одними из задач грамматического разбора являются задачи определения, принадлежит ли какое-либо сообщение определенной грамматике. Для этого довольно удобно описать необходимые правила этой самой грамматики, а затем применить их к входному сообщению. Пролог позволяет достаточно легко определить эти правила, а затем проверить на соответствие им входные данные.

При помощи средств Пролога можно с легкостью выделить любую необходимую информацию из предложения: разобрать каждое слово на составляющие, определить части речи слов, их число и прочее.




