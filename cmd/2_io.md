# Задание

1. Изучить способы перенаправления ввода-вывода и анализа кода ошибки
2. Написать программу на языке C++/C#:
   - получает на вход 2 аргумента. Если аргументов меньше двух - в поток stderr пишет сообщение об ошибке.
   - проверяет, являются ли они числами. Если аргументы не числа - в поток stderr пишет сообщение об ошибке.
   - выводит сумму чисел в поток stdout.
3. Написать скрипт:
   - компиляция программы. Если компиляция не успешна, то выводится сообщение об ошибке. Необходимо анализировать код возврата компилятора
   - запускает программу:
     -  с числом аргументов не равным 2
     -  с числовыми и нечисловыми аргументами
     -  с числовыми аргументами
  -  для каждого запуска определить в какой поток (stdout или stderr) написано сообщение (например, через перенаправление потока в файлы и анализ их содержимого) и вывести соответствующее сообщение, например: "программа выполнена корректно, ответ = 5" или "программа выполнена некорректно, ошибка = число аргументов меньше двух"
4. Написать скрипт, которая создает N1 папок, в каждой из них N2 подпапок, в каждой из них N3 файлов с расширениями txt. В каждойм файле записан полниый путь к файлу. Для каждого из N1, N2, N3 файлов и папок имя генерируется случайно в диапазоне [0-99].
5. Написать скрипт, который выводит список файлов, удовлетворяющий правилу: содержит цифру в имени.

# Перенаправления ввода-вывода 

* \> - перенаправление вывода (stdout). Выходные данные записываются в файл или передаются на указанное устройство.

```
rem выполнить пинг петлевого интерфейса 5 раз с перенаправлением вывода в фиктивное устройство nul. Вывод результатов выполнения команды подавляется. 
ping –n 5 localhost > nul 

rem выполнить 100 раз пинг узла yandex.ru c записью результатов выполнения команды в файл C:\ping-ya.txt. 
ping –n 100 yandex.ru > C:\ping-ya.txt 

```

* 2> - перенаправление вывода потока ошибок (stderr). Выходные данные потока ошибок записываются в файл или передаются на указанное устройство.


* >> - то же, что и в предыдущем случае, но данные записываются в конец файла.

```
rem то же, что и в предыдущем примере, но, если файл не существует, то он будет создан, а если существует, то запись результатов будет выполняться в конец файла.
ping –n 100 yandex.ru >> C:\ping-ya.txt
```


* < - перенаправление ввода. Данные считываются не с клавиатуры, а из файла или другого устройства.

```
rem запустить командный процессор CMD и выполнить ввод данных из файла 1.txt. Если в файле поместить строку ping –n 100 yandex.ru, то выполнится команда, рассмотренная выше
cmd < 1.txt
```

* | - вывод первой команды перенаправить на вход следующей за ней.

Нередко, вывод одной команды нужно передать в качестве вводимых данных для другой, т.е. объединить команды в последовательную цепочку:

```
rem результат выполнения команды ping -n 100 microsoft.com передается в виде входных данных для команды поиска строк (find), содержащих текст "Превышен интервал"
ping -n 100 microsoft.com | find "Превышен интервал"

rem то же, что и в предыдущем примере, но с перенаправлением выводимых результатов выполнения команды в текстовый файл.
ping -n 100 microsoft.com | find "Превышен интервал" > C:\ping-ya.txt
```

# Использование дескрипторов ввода-вывода консоли.

Каждому открытому файлу или устройству соответствует свой дескриптор ( handle) который представляет собой неотрицательное число, значение которого используется породившим поток ввода-вывода процессом. По умолчанию, для всех процессов, в том числе и для командного интерпретатора cmd.exe :

* 0 ( STDIN ) – дескриптор стандартного ввода (ввод с клавиатуры).

* 1 (STDOUT) – дескриптор стандартного вывода (вывод на экран).

* 2 (STDERR) – дескриптор вывода диагностических сообщений (сообщений об ошибках на экран).

Дескрипторы можно использовать в тех случаях, когда требуется перенаправить (изменить) источники и приемники данных в стандартных потоках ввода-вывода. Например:

```
ping.exe –n 100 yandex.ru 2> C:\pinglog.txt
```
- стандартный поток сообщений программы ping.exe будет выводиться на экран, а ошибки ( стандартный вывод с дескриптором = 2 ) будут записаны в файл C:\pinglog.txt . В реальном случае для программы ping.exe приведенная конструкция значения не имеет, поскольку она выводит и диагностику, и результаты на экран.

Для задания перенаправления в существующие дескрипторы используется амперсанд (&), затем номер требуемого дескриптора (например, &1):

```
ping –n 100 yandex.ru >log.txt 2>&1
```
- стандартный поток сообщений об ошибках (дескриптор=2) перенаправляется в стандартный поток вывода (дескриптор = 1) и все это перенаправляется в файл log.txt текущего каталога.

```
ping –n 100 yandex.ru >log.txt 1>&2 
```
- стандартный вывод (дескриптор = 1) перенаправляется в вывод сообщений об ошибках (дескриптор=2) и все это записывается в текстовый файл.

Если дескриптор не определен, то по умолчанию оператором перенаправления ввода < будет ноль (0), а оператором перенаправления вывода > будет единица.

# Анализ результата выполнения программ 

Команда if выполняет условную обработку в пакетных программах.

Синтаксис

```
if [not] ERRORLEVEL <number> <command> [else <expression>]
if [not] <string1>==<string2> <command> [else <expression>]
if [not] exist <filename> <command> [else <expression>]
```

## Параметры

* **not**	Указывает, что команда должна выполняться только в том случае, если условие имеет значение false.
* **Errorlevel \<number\>**	Указывает истинное условие, только если предыдущая программа, запущенная Cmd.exe, вернула код выхода, равный или превышающий число.
* **\<command\>**	Указывает команду, которая должна выполняться при выполнении предыдущего условия.
* **\<string1\>==\<string2\>**	Указывает истинное условие, только если строки string1 и string2 совпадают. Это могут быть литеральные строки или переменные пакета (например, %1). Не нужно заключать литеральные строки в кавычки.
* **exist \<filename\>**	Указывает истинное условие, если указанное имя файла существует.
* **\<compareop\>**	Задает трехбуквенный оператор сравнения, в том числе:
    - EQU — равно
    - NEQ — не равно
    - LSS — меньше
    - LEQ — меньше или равно
    - GTR — больше
    - GEQ — больше или равно
* **/i**	Принудительное сравнение строк пропускает регистр. Параметр /i можно использовать в string1==string2 форме if. Эти сравнения являются универсальными, так как если строки string1 и string2 состоят только из числовых цифр, строки преобразуются в числа и выполняется числовое сравнение.
* **cmdextversion \<number\>**	Указывает истинное условие, только если внутренний номер версии, связанный с функцией расширений команд Cmd.exe, равен или больше указанного числа. Первая версия — 1. Он увеличивается на приращение единицы при добавлении значительных улучшений в расширения команд. Условие cmdextversion никогда не имеет значения true, если расширения команд отключены (по умолчанию расширения команд включены).
* **defined \<variable\>**	Указывает истинное условие, если определена переменная .
* **\<expression\>**	Указывает команду командной строки и все параметры, передаваемые команде в предложении else .
* **/?**	Отображение справки в командной строке.

Если условие, указанное в предложении if , имеет значение true, выполняется команда, следующая за условием. Если условие имеет значение false, команда в предложении if игнорируется, и команда выполняет любую команду, указанную в предложении else .

Когда программа останавливается, она возвращает код выхода. Чтобы использовать коды выхода в качестве условий, используйте параметр errorlevel .

В среду добавляются следующие три переменные: %errorlevel%, %cmdcmdline%и %cmdextversion%.

%errorlevel%: разворачивается в строковое представление текущего значения переменной среды ERRORLEVEL. Эта переменная предполагает, что еще не существует переменной среды с именем ERRORLEVEL. В этом случае вы получите это значение ERRORLEVEL.

%cmdcmdline%: разворачивается в исходную командную строку, которая была передана в Cmd.exe перед обработкой Cmd.exe. Предполагается, что еще не существует переменной среды с именем CMDCMDLINE. В этом случае вы получите это значение CMDCMDLINE.

%cmdextversion%: разворачивается в строковое представление текущего значения cmdextversion. Предполагается, что еще не существует переменной среды с именем CMDEXTVERSION. Если это так, вы получите это значение CMDEXTVERSION.

Необходимо использовать предложение else в той же строке, что и команда после if.

## Примеры

Чтобы отобразить сообщение Не удается найти файл данных, если не удается найти файл Product.dat, введите:

``` 
if not exist product.dat echo Cannot find data file
```

Чтобы отформатировать диск на диске A и отобразить сообщение об ошибке при возникновении ошибки во время процесса форматирования, введите следующие строки в пакетный файл:

```
:begin
@echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program.
```

Чтобы удалить файл Product.dat из текущего каталога или отобразить сообщение, если product.dat не найден, введите следующие строки в пакетном файле:

```
IF EXIST Product.dat (
del Product.dat
) ELSE (
echo The Product.dat file is missing.
)
```

Чтобы повторить значение переменной среды ERRORLEVEL после запуска пакетного файла, введите следующие строки в пакетном файле:

```
goto answer%errorlevel%
:answer1
echo The program returned error level 1
goto end
:answer0
echo The program returned error level 0
goto end
:end
echo Done!
```

Чтобы перейти к метки ок, если значение переменной среды ERRORLEVEL меньше или равно 1, введите:

```
if %errorlevel% LEQ 1 goto okay
```

# Компиляция программы через командную строку

# 1. C#

Проект в среде NetCore на C# можно компилировать с использованием утилиты dotnet.

```
dotnet build
```

Смотри подробнее [https://learn.microsoft.com/ru-ru/dotnet/core/tools/dotnet-build ]

Коды возврата ошибок подробно описаны здесь: [https://github.com/dotnet/templating/wiki/Exit-Codes]


# 2. C++

Visual Studio включает в себя командную строку C и компилятор C++.

```
cl /EHsc hello.cpp
```

Смотри подробнее [https://learn.microsoft.com/ru-ru/cpp/build/walkthrough-compiling-a-native-cpp-program-on-the-command-line?view=msvc-170]

Коды возврата ошибок подробно описсаны здесь: [https://learn.microsoft.com/en-us/cpp/build/reference/return-value-of-cl-exe?view=msvc-170]