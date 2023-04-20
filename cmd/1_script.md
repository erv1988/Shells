# Задание
1. Написать скрипт, в которм создаются переменные строкового и числового типов
2. Написать скрипт для решения квадратного уравнения с выводом промежуточных результатов. Параметры A,B,C запрашивать в командной строке у пользователя.
3. Написать скрипт, реализующий цикл for, используя метки и goto. На каждом этапе цикла выводить текущее значение цикла, результат вычисления каких-либо функций с подстроками.


# Запуск

- через Главное меню Windows, 
- с использованием диалога Выполнить (комбинации клавиш Win+R) => cmd (C:\WINDOWS\System32\cmd.exe).

# Базовые команды

- HELP - вызов справки командной строки
- ECHO - вывод текста на экран консоли
- REM - комментарии в командных файлах
- CLS - очистка экрана в командной строке
- CD - смена каталога (Change Directory)
- DIR - отображение списка файлов и каталогов
- TREE - отображение структуры каталога в графическом виде
- COPY - копирование файлов и каталогов
- MOVE - перемещение файлов и каталогов
- MD - создание нового каталога
- DEL - удаление одного или нескольких файлов
- RD - удаление каталога
- RMDIR - удаление каталога
- CMD - запуск новой копии интерпретатора командной строки
- WHERE - определение места расположения файлов
- EXIT - выход из процедуры или командного файла
- FOR - организация циклической обработки
- IF - оператор условного выполнения команд
- GOTO - команда безусловного перехода
- SET - отображение и изменение переменных среды
- PAUSE - пауза при выполнении командного файла
- SORT - сортировка строк в текстовом файле
- START - запуск приложения или командного файла
- TASKKILL - завершение процессов на локальной или удаленной системе.
- TASKLIST - отображение списка выполняющихся приложений и служб Windows

# Работа с переменными

1. Задание переменных

Вручную
```
set x=stroka
set /a y=55
```

Ввод с клавиатуры
```
set /p x="BBeduTe cTpoky: "
```

2. Типы переменных

Cтрока Ограничение 8184 символа.
```
set x=stroka
```

Число. Ограничение от -2147483647 до 2147483647.

```
set /a x=25*5
```

3. Использование переменных

Вывод значения переменных
```
echo %x%
```

Существующие переменные

* %RANDOM% - раскрывается в случайное десятичное число между 0 и 32767.(от 0 до (2^17)-1)

```
set /a random10=%random%/3277
```

Выводит случайное число от 0 до 9.

* %CD% - раскрывается в строку текущей директории.
* %DATE% - раскрывается в текущую дату, используя тот же формат команды DATE.
* %TIME% - раскрывается в текущую дату, используя формат команды TIME.
* %ERRORLEVEL% - раскрывается в текущее значение ERRORLEVEL. Уровень ошибки, 0 - это нет ошибки, 1 - это есть ошибка, а другие это номера ошибки.

Чтобы получить полный список переменных и их значений введите команду SET


4. Операции со строковыми или численными переменными

Соединение 2-ух строковых переменных
```
set x=Gaz
set y=Prom
echo %x%%y%
rem (GazProm)
```

5. Вывод определенного(ых) символа(ов) из строки

Символы номеруются начиная с 0!

```
set str=babywka

rem Вывод 1-ого символа
echo %str:~0,1%
rem (b)

rem Вывод 3-х символов с конца строки
echo %str:~-3%
rem (wka)

rem Вывод всей строки кроме 2-ух первых символов
echo %str:~2%
rem (bywka)

rem Вывод всей строки кроме 2-ух последних символов
echo %str:~0,-2%
rem (babyw)

rem Вывод 3-х символов начиная с 3
echo %str:~2,3%
rem (byw)
```


6. Удаление подстроки из строки

```
set str=babywka
echo %str:ba=%
rem (bywka)
```

Замена подстроки из строки на другую подстроку
```
set str=babywka
echo %str:bab=xlop%
rem (xlopywka)
```

Удаление кавычек(") из строки

```
set str2="qwerty"
echo %str2:"=%
rem (qwert)
```

Существуют 2 способа использовать переменную в переменной, например: вывод n-ого символа

```
rem Первый способ с call set
call set x=%%str:~%n%,1%%
echo %x%

rem Второй способ с for и setlocal enabledelayedexpansion
setlocal enabledelayedexpansion
for /l %%i in (%n%,1,%n%) do (set x=!str:~%%i,1!)
echo %x%
```


7. Операции с числовыми переменными

Увеличивание на единицу
```
set /a x+=1
```

Увеличивание в 2 раза
```
set /a x+=%x%
```

аналогично
```
set /a x*=2
```

Возведение в квадрат
```
set /a x*=%x%
```

Возведение в куб
```
set /a x=%x%*%x%*%x%
```

Деление
Деление в CMD является целочисленным!(то есть делится до целого числа)
```
set /a x=15
set /a y=4
set /a xy=%x%/%y%
rem (3)
```

Сложение
```
set /a x=5
set /a y=-6
set /a xy=%x%+%y%
rem (5+(-6)=5-6=-1)
```

Вычитание

```
set /a x=5
set /a y=-6
set /a xy=%x%-%y%
rem (5-(-6)=5+6=11)
```

Вычисление остатка от деления
```
set /a xy=%x% "%" %y%
rem (при записи в батник, процент "%" нужно удваивать "%%")
```

Унарные операторы

Логическое отрицание (Logical NOT)

```
set /a y="!"%x%
```

дает результат(%y%) 1 (True), если переменная(%x%) равна 0 (False), и 0 (False) (%y%) в любых других случаях

```
set /a x=5
set /a y="!"%x%
rem (0)
set /a x=0
set /a y="!"%x%
rem (1)
```

Побитовая инверсия (Bitwise NOT):
```
set /a y="~"%x%
```

rem дает результат -1-%x% (%y%)
```
set /a x=5
set /a y="~"%x%
rem (-1-5=-(1+5)= -6)
set /a x=-3
set /a y="~"%x%
rem (-1-(-3)=-1+3=3-1= 2)
```

Унарный минус (устанавливает/сбрасывает знаковый бит)
```
set /a y="-"%x%
```
дает результат 0-%x% (%y%)

```
set /a x=5
set /a y="-"%x%
rem (-5)
set /a x=-3
set /a y="-"%x%
rem (3)
```

Двоичные операторы
```
set x=3
rem (в двоичной системе счисления - 0011)
set y=5
rem (в двоичной системе счисления - 0101)
```

Побитовое И (AND)

Побитовое И — это бинарная операция, действие которой эквивалентно применению логического И к каждой паре битов, которые стоят на одинаковых позициях в двоичных представлениях операндов.
Другими словами, если оба соответствующих бита операндов равны 1, результирующий двоичный разряд равен 1; если же хотя бы один бит из пары равен 0, результирующий двоичный разряд равен 0.

```
set /a xy=%x%"&"%y%
rem (1, в двоичной системе счисления - 0001)
```

Побитовое ИЛИ (OR)

Побитовое ИЛИ — это бинарная операция, действие которой эквивалентно применению логического ИЛИ к каждой паре битов, которые стоят на одинаковых позициях в двоичных представлениях операндов.
Другими словами, если оба соответствующих бита операндов равны 0, двоичный разряд результата равен 0; если же хотя бы один бит из пары равен 1, двоичный разряд результата равен 1.

```
set /a xy=%x%"|"%y%
rem (7, в двоичной системе счисления - 0111)
```

Побитовое исключающее ИЛИ (XOR)

Побитовое исключающее ИЛИ (или побитовое сложение по модулю два) — это бинарная операция, действие которой эквивалентно применению логического исключающего ИЛИ к каждой паре битов, которые стоят на

одинаковых позициях в двоичных представлениях операндов.
Другими словами, если соответствующие биты операндов различны, то двоичный разряд результата равен 1; если же биты совпадают, то двоичный разряд результата равен 0.

```
set /a xy=%x%"^"%y%
rem (6, в двоичной системе счисления - 0110)
```

К битовым операциям также относят битовые сдвиги. При сдвиге значения битов копируются в соседние по направлению сдвига.
Различают сдвиг влево (в направлении от младшего бита к старшему) и вправо (в направлении от старшего бита к младшему).
При логическом сдвиге значение последнего бита по направлению сдвига теряется (копируясь в бит переноса), а первый приобретает нулевое значение.

Двоичный арифметический сдвиг

Арифметический сдвиг аналогичен логическому, но значение слова считается знаковым числом, представленным в дополнительном коде.
Так, при правом сдвиге старший бит сохраняет свое значение. Левый арифметический сдвиг идентичен логическому.

Вправо
```
set /a xy=%x%">>"1
rem (1, в двоичной системе счисления - 0011->0001)
set /a xy2=%y%">>"1
rem (2, в двоичной системе счисления - 0101->0010)
set /a n=13
rem (в двоичной системе счисления - 1101)
set /a xn=%n%">>"2
rem (3, в двоичной системе счисления - 1101->0011)
set /a my=-%y%">>"1
rem (-3, в двоичной системе счисления - 1011(-5)->1101(-3))
```

Влево
```
set /a xy=%x%"<<"1
rem (6, в двоичной системе счисления - 0011->0110)
set /a xy2=%y%"<<"1
rem (10, в двоичной системе счисления - 0101->1010)
set /a xy3=%y%"<<"4
rem (80, в двоичной системе счисления - 0101->1010000)
set /a my=-%y%"<<"1
rem (-10, в двоичной системе счисления - 1011(-5)->10110(-10))
```

Максимальный размер отдельной переменной среды составляет 8192 байта.(у меня выходило только 8184, наверное это вместе с названием...)
Максимальный общий размер всех переменных среды, включая имена переменных и знак равенства, составляет 65 536 Кбайт.


Системы счисления

Числовые значения рассматриваются как десятичные, если перед ними не стоит префикс 0x для шестнадцатеричных чисел, и 0 для восьмеричных чисел. Например, числа 0x12, и 022 обозначают десятичное число 18.

Обратите внимание на запись восьмеричных числе: 08 и 09 не являются допустимыми числами, так как в восьмеричной системе исчисления цифры 8 и 9 не используются.

Восьмеричная система счисления
```
set /a x=022
rem (Это 22 в восьмеричной системе счисления, и 18 в десятичной)
```

Можно производить все операции, также как и с десятеричными числами.
Но после задания значения переменной, значение хранится в десятичной системе счисления.
Например, сложение
```
set /a xy=022+07
rem (Это 22+7=31 в восьмеричной системе счисления, и 31 в десятичной)
```

Шестнадцатеричная система счисления
```
set /a x=0x3A
rem (Это 3A в восьмеричной системе счисления, и 58 в десятичной)
set /a xy=0x3A-0x66
rem (Это 3A-66=-54 в восьмеричной системе счисления, и -44 в десятичной)
```

Сохранение в переменной вывода программы
К сожаление, передача вывода программ на вход команды set не работает:
```
echo 1|set /p AA=""
set AA
```
Поэтому можно воспользоваться временным сохранением в файл:
```
echo 1> 0.tmp
set /p AA="" <0.tmp
del 0.tmp
echo %AA%
```

Примеры использования
Узнать динамически генерируемое имя архива WinRar:
```
rar a -z%Comment% -p%p% "-ag yyyy-mm-dd[n]" %OutPath%\%arhivename%.%ext% @%FileList% >rar.log.tmp
for /f "tokens=2*" %%I in ('find /i "Creating archive" ^<rar.log.tmp') do @echo %%J >rarfilename.tmp
set /p rarfilename="" <rarfilename.tmp
del rarfilename.tmp
```

Узнать имя последнего изменённого файла в папке:
```
dir /b /a-d /o-d *.* >%temp%\0.tmp
set /p lastfile="" <%temp%\0.tmp
del %temp%\0.tmp
echo "%lastfile%"
```

Оператор == используется только для строчного сравнения. Кавычки необходимы если в переменной/операнде имеются пробелы.
Для арифметического сравнения необходимо использовать арифметические операторы:

    EQU : Равно (=)
    NEQ : Не равно (!=)
    LSS : Меньше (<)
    LEQ : Меньше или равно (<=)
    GTR : Больше (>)
    GEQ : Больше или равно (>=)

Использовать операторы, указанные в скобках, не представляется возможным, потому что, например, операторы < и > являются указателями перенаправления ввода-вывода.