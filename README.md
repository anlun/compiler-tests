# Репозиторий с тестами для учебного компилятора
[Пример репозитория с компилятором](https://github.com/anlun/compiler-example) |
[Дополнительные материалы](https://drive.google.com/drive/folders/0B3UPtzTx9FB1YWlkNS1yWndXcXc?usp=sharing) |
[Результаты](https://docs.google.com/spreadsheets/d/1JzJuaO3GWcG0kCF9-eTzIfCDAGbxy5nHtW3UxFWsK9Y/edit?usp=sharing)

_В табличку 'Результаты' нужно записаться до 13 марта._

## Структура репозитория
- `core` - небольшой набор тестов, в котором есть (или будет) хотя бы по одному тесту
  на все необходимые к реализации части реализуемого языка;
- `expressions` и `deep-expressions` - много тестов на базовое подмножество языка
  (арифметические выражения + ввод / вывод);
- `performance` - пара тестов на сравнение эффективности компилятора с gcc.

В папках `core`, `expressions` и `deep-expressions` есть пачка тестов (`*.expr`),
входы для тестов (`*.input`), эталонные выводы (`orig/*.log`) и `Makefile`.

## Предполагаемый сценарий использования репозитория
Этот репозиторий добавляется как подмодуль (submodule) в корень репозитория компилятора.

В корне также находится папка `runtime`, в которой по запуску `make` генерируется объектный
файл окружения (`runtime.o`).  Таким образом, путь до `runtime.o` из подмодуля
`compiler-tests` - `../runtime/runtime.o`.

По запуску `make` в корне репозитория компилятора должен создаваться бинарник самого компилятора
с именем `rc`. Таким образом, путь до бинарника из подмодуля - `../rc`.

Интерфейс бинарника:
- `rc -i filename.expr` - интерпретация программы из файла `filename.expr`;
- `rc -s filename.expr` - компиляция программы из файла `filename.expr` в представление стековой машины с последующей интерпретацией стековой машиной;
- `rc -o filename.expr` - компиляция программы из файла `filename.expr` в запускаемый файл `filename`;
Во всех случаях для ввода и вывода используются стандартные потоки ввода и вывода.

## Описание корневого Makefile
TODO

```makefile
TESTS=test001 test002 test003 test004 test005 test006 test007 test008 test009 test010 test011 test012 test013 test014 test015 test016 test017 test018 test019 test020 test021 test022 test023 test024 test025 test026 test027 test028 

.PHONY: check $(TESTS) 

check: $(TESTS) 

$(TESTS): %: %.expr
	RC_RUNTIME=../runtime ../rc.native -o $< && cat $@.input | ./$@ > $@.log && diff $@.log orig/$@.log
	cat $@.input | ../rc.native -i $< > $@.log && diff $@.log orig/$@.log
	cat $@.input | ../rc.native -s $< > $@.log && diff $@.log orig/$@.log

clean:
	rm -f test*.log *.s *~ $(TESTS)
```

## Задания
01. Выражения + ввод / вывод в режиме интерпретатора (`core/test001.expr` - `core/test008.expr`, `expressions/*.expr`, `deep-expressions/*.expr`).
02. Выражения + ввод / вывод в режиме стековой машины (`core/test001.expr` - `core/test008.expr`, `expressions/*.expr`, `deep-expressions/*.expr`).
03. Выражения + ввод / вывод в режиме компиляции (`core/test001.expr` - `core/test008.expr`, `expressions/*.expr`, `deep-expressions/*.expr`).
04. If + циклы (`core/test009.expr` - `core/test022.expr`) в режиме интерпретатора.
05. If + циклы (`core/test009.expr` - `core/test022.expr`) в режиме стековой машины.
06. If + циклы (`core/test009.expr` - `core/test022.expr`) в режиме компиляции.
07. Функции (`core/test023.expr` - `core/test026.expr`) в режиме интерпретатора.
08. Функции (`core/test023.expr` - `core/test026.expr`) в режиме стековой машины.
09. Функции (`core/test023.expr` - `core/test026.expr`) в режиме компиляции.
10. Строки (`core/test027.expr` - `core/test028.expr`) в режиме интерпретатора.
11. Строки (`core/test027.expr` - `core/test028.expr`) в режиме стековой машины.
12. Строки (`core/test027.expr` - `core/test028.expr`) в режиме компиляции.
13. Массивы (`core/test029.expr` - `core/test030.expr`) в режиме интерпретатора.
14. Массивы (`core/test029.expr` - `core/test030.expr`) в режиме стековой машины.
15. Массивы (`core/test029.expr` - `core/test030.expr`) в режиме компиляции.

## Замечания
Массивы в языке бывают двух видов: bodex, массив указателей, и unboxed, массив значений. Массивы (как и все указатели в нашем языке) начинаются с заглавной буквы, все остальные переменные - со строчной. Для реализации массивов понадобится автоматическое управление памятью.
Функции работы с массивами:
- arrlen (<array>) - длина массива;
- Arrmake (<size>, <default_value>) - выделение памяти под boxed массива;
- arrmake (<size>, <default_value>) - выделение памяти под unboxed массива;
- operator [<element_number>] - доступ к соответствующему элементу массива.
