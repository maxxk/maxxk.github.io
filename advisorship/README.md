# М.А. Кривчиков

[maxim.krivchikov@gmail.com](mailto:maxim.krivchikov@gmail.com)

[maxim.krivchikov@math.msu.ru](mailto:maxim.krivchikov@math.msu.ru)

[vk.com/maxim.krivchikov](https://vk.com/maxim.krivchikov)

[https://vk.com/msumathprogx05](https://vk.com/msumathprogx05) — в группе VK есть видеозаписи вводных спецкурсов по системам типов в языках программирования:
 (отправьте заявку и напишите мне).

[http://seminar.s2s.msu.ru/students](http://seminar.s2s.msu.ru/students) — направления работы нашей исследовательской группы.

# Темы курсовых и дипломных работ

На этой странице кратко представлены варианты направлений для курсовых и дипломных работ, которые можно вести под моим руководством на [кафедре вычислительной математики](http://numa.math.msu.su/).

# Системы типов в языках программирования

Область исследований — приложение математических методов (в первую очередь — математической логики) программированию.

Цель — разработка надёжных программ.

«Система типов  это гибко управляемый синтаксический метод доказательства отсутствия в программе определенных видов поведения при помощи классификации выражений языка по разновидностям вычисляемых ими значений.»
(перефразируя Б. Пирса — «Типы в языках программирования»)

- Как сформулировать и доказать утверждения, характеризующие свойства программы (не алгоритма, а его реализации на каком-то языке программирования)?
- Как математические методы могут помочь разрабатывать программы без ошибок?

## Система типов на основе суперкомпиляции

Суперкомпиляция — метод анализа и преобразования программ, в ходе которого:
1. Моделируется выполнение программы на параметризованных входных данных.
2. В потенциально бесконечном графе конфигураций программы выделяются и сворачиваются циклы (рекурсивные вызовы).
3. Из модели выделяется остаточная программа.
— «анализ программы с параметром»

Самое точное описание поведения программы даёт не описание типов: 
```c
int main(int argc, char *argv[argc])
```
а её исходный код:
```c
{
    if (argc == 2) {
        printf("Hello, %s!", argv[1]);
        return 0;
    } else {
        printf("Not enough arguments!");
        return 1;
    }
}
```

Подход к анализу программ: определяем аппроксимацию входных данных. С помощью суперкомпиляции проверяем адекватность аппроксимации и уточняем поведение программы:

```
main(1, неизвестный_параметр) ⇒ { printf("Hello, %s!", неизвестный_параметр[1]); return 0; }

main(0, неизвестный_параметр) ⇒ { printf("Not enough arguments!"); return 1; }
```

Проблемы: комбинаторный взрыв; условия завершимости программы (проблема останова).

## Приложение: анализаторы программ на известных языках программирования

Современные языки программирования (Rust) предотвращают ошибки при работе с памятью с помощью системы типов.

Большая часть фундамента современного программного обеспечения (ядро операционной системы, системные библиотеки) написано на языке C.

Можно ли использовать анализ на основе суперкомпиляции для реализации анализа, аналогичного языку Rust, хотя бы для некоторых (адаптированных) программ на языке C.

Пример задачи для погружения в предметную область: расширение языка C в виде биекции на уровне синтаксиса.

Аналог clang modules на основе pkg-config:

`#include <pthreads.h>; gcc -lpthread` ↔ `#import pthreads`

Uniform function call syntax и импровизированные пространства имён на основе префиксов:
```
result = pthread_join(my_thread, &return_value) 
↔ 
#alias pthread_ (join, ...)
result = my_thread.join(&return_value)
```

Биективное преобразование: открываем файл на языке C; редактируем его в «красивом» виде с модулями и UFCS; сохраняем обратно в C и получаем читаемый текст программы с минимальными необходимыми модификациями. Таким образом, расширение получается не отдельным языком, а способом представления и редактирования существующего кода.

## Приложения: системы типов для минималистичных языков программирования

Typed Assembly Language (G. Morrisett, 1998): язык ассемблера с продвинутой системой типов (System F).

К блокам кода на ассемблере добавляется описание системы типов — требования к содержимому регистров на момент входа в блок.
Авторы не дошли до описания требований к содержанию памяти и эффектов системных вызовов.

На практике: reverse engineering — анализ программ, исходный код которых недоступен.

Forth — простой конкатенативный расширяемый язык программирования (советский независимый вариант с ВМК: язык ДССП).
Особенности:
- аргументы передаются на общем для всей программы стеке; каждая функция снимает со стека и кладёт на стек произвольное количество аргументов
- активно используется самомодифицируемость
- тривиальная реализация (4-8 КБайт машинного кода — по сравнению с десятками тысяч строк кода для компилятора языка C, ассемблера и т.д.).

На практике: анализ самомодифицируемых программ, поиск уязвимостей в программах.

## Приложения: автоматизированное доказательство теорем

Исчисление конструкций (λ-исчисление с зависимыми типами) — формальная система, которая используется для описания свойств программ.

Исчисление конструкций используется в качестве базовой модели для систем автоматизированного доказательства теорем. 
Например, французская система Coq была использована для повторения доказательства теоремы о четырёх красках.

Задача разработки системы автоматизированного доказательства теорем на основе суперкомпиляции может быть поставлена для оценки выразительности системы типов.

## Пример задачи «в сторону»

Некоторые исследователи и практики программирования говорят о том, что компилятор — это база данных правил преобразования кода (хотя в отечественной терминологии правильнее было бы сказать «база знаний»). Есть несколько докладов с такими тезисами (например, [Martin Odersky: Compilers Are (In-Memory) Databases, 2016 Scala World Keynote](https://www.youtube.com/watch?v=w1ca4KL9UXc)]), и попытки использовать эту точку зрения для ускорения работы компиляторов ([Queries: demand-driven compilation, Rust Compiler Development Guide](https://rustc-dev-guide.rust-lang.org/query.html)).

Для проверки гипотезы о том, что представление в виде базы данных является удобным для компилятора (т.е., например, сокращает объём и сложность кода по сравнению с классическими реализациями, не теряя при этом существенно в производительности, или получая при этом новые возможности) можно начать с реализации ассемблера (с прицелом на backend компилятора в будущем) в терминах реляционной модели.

В качестве стартовой точки нужно, с одной стороны, взять код одной из доступных реализаций ассемблера (NASM, FASM, [VASM](https://github.com/mbitsnbites/vasm-mirror) - с осторожностью, не свободный код, GAS - скорее всего, непонятный код) или backend компилятора (LLVM - скорее всего, большой и непонятный код, так что можно начать с [QBE](https://c9x.me/compile/)), и, с другой стороны, реализовать свой вариант ассемблера в терминах реляционной модели (например, на SQLite + Python) со следующей архитектурой.

В отдельной базе данных "Architecture definition" хранится описание системы команд процессора. Как минимум, там должны быть следующие сущности:

- RegisterKind - список видов регистров (целочисленные; в зависимости от разрядности — например, в x86_64 к разным видам будут относиться регистры RAX (64-битный), EAX (32-битный), AX (16-битный), AH и AL (8-битные), XMM0 (векторный 128-битный регистр), EFLAGS (специальный регистр флагов) и т.п.
- Register (kind : RegisterKind, name) - список регистров каждого вида.
- Operation - инструкция
- Operand - входной операнд инструкции с ограничениями на то, в каких регистрах он может находиться
- Output - результат инструкции с ограничениями на то, в каких входных операндах или конкретных регистрах или видах регистров он может находиться.

Что нужно сделать:
- доделать структуру базы данных (в том числе подумать, как представить перекрытие регистров:  регистры AH и AL - это старшие и младшие 8 бит регистра AX, который соответствует младшим 16 битам регистра EAX, который соответствует младшим 32 битам регистра RAX)
- заполнить для одной из архитектур (x86_64 — чуть сложнее, но у вас заведомо будет доступ к этому процессору, можно aarch64, если у вас есть ARM-машина, типа Apple M1/M2) базу данных:
    - все виды регистров
    - все регистры
    - по несколько инструкций с регистрами каждого вида
    - инструкции, которые необходимы для демонстрационных программ
- написать простые демонстрационные программы
- реализовать собственно ассемблер — процесс трансляции последовательности команд в последовательность байтов машинного кода — в виде последовательностей запросов к БД
- описать в каком-то виде frontend ассемблера (не обязательно из текстового файла, можно и на Python)
- сравнить качественно реализацию с одной из доступных

# Децентрализованные наукометрические системы

Кроме научных исследований, наш коллектив разрабатывает информационно-аналитическую систему «ИСТИНА»: [https://istina.msu.ru/](https://istina.msu.ru/).
Сейчас эта система работает в режиме «облака»: система существует в единственном экземпляре, и данные о научных результатах всех организаций, подключенных к ней, хранятся в центре обработки данных оператора системы. Такой «облачный» вариант удобен для верификации данных в системе. Однако по целому ряду других критериев — масштабируемость, контроль над данными, применимость системы в «закрытых» организациях, возможность доработки под нужды каждой из организаций — удобнее другая модель, в которой система разбита на большое количество равноправных независимых узлов, взаимодействующих на основе открытых протоколов. Такая модель называется децентрализованной.

На этом направлении есть как задачи, предполагающие большой объём программирования:
- разработка прототипов децентрализованной системы;
- разработка и анализ протоколов взаимодействия узлов;
- моделирование и анализ производительности системы;

так и задачи, требующие создания новых математических моделей для решения вопросов доверия в децентрализованной системе:
- как узлы определяют степень авторитетности данных на каждом из узлов;
- как определить степень «верифицированности» набора данных о результате, в зависимости от результатов их сравнения с версиями, которые есть на других узлах системы;
- каким образом система приходит к консенсусу о «канонической» (наиболее правильной) версии набора данных;
- как разрывать циклы уведомлений об обновлениях данных между узлами.

[Заметка про архитектуру децентрализованной наукометрической системы](d-istina.pdf)
