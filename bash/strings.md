# Задание

1. Создать скрипт my_format.sh, который:
   - выводит орматированный текст вида для списка из 13 выбранных вами стран. Информацию можно взять из Wiki:

```
#     Name     Country       Area               Currency
----------------------------------------------------------
01    Ivan     Russia        17,098,246 km2      Рубль ₽
02    John     England       130,279 km2         Pound sterling £
...
10    Hans     Deutchland    357,592 km2         Euro €
```

При этом обеспечить выравнивание средствами форматирования, определения длин строк и т.д. Специальные символы выводить через экранирование, можно вывести текст графикой выбранных стран.

2. Создать скрипт my_fix.sh, который:
   - получает на вход параметры:
   - выполняет поиск файлов по маске и для них выполняет действия
   - в файле ищет строку, укаазнную во втором параметре, и заменяет на строку, указанную в третьем параметре
   - выводит длину текста до замены и после замены.


# Кавычки

Кавычки, ограничивающие строки с обеих сторон, служат для предотвращения интерпретации специальных символов, которые могут находиться в строке. (Символ называется "специальным", если он несет дополнительную смысловую нагрузку, например символ шаблона -- *.)

```
bash$ ls -l [Vv]*
-rw-rw-r--    1 bozo  bozo       324 Apr  2 15:05 VIEWDATA.BAT
 -rw-rw-r--    1 bozo  bozo       507 May  4 14:25 vartrace.sh
 -rw-rw-r--    1 bozo  bozo       539 Apr 14 17:11 viewdata.sh

bash$ ls -l '[Vv]*'
ls: [Vv]*: No such file or directory
```

Note	
Некоторые программы и утилиты могут вызываться с дополнительными параметрами, содержащими специальными символы, поэтому очень важно предотвратить интерпретацию передаваемых параметров командной оболочкой, позволяя сделать это вызываемой программой.

```
bash$ grep '[Пп]ервая' *.txt
file1.txt:Это первая строка в file1.txt.
file2.txt:Это Первая строка в file2.txt.
```

Примечательно, что "не окавыченный" вариант команды grep [Пп]ервая *.txt будет правильно исполняться в Bash, но не в tcsh.

Вообще, желательно использовать двойные кавычки (" ") при обращении к переменным. Это предотвратит интерпретацию специальных символов, которые могут содержаться в именах переменных, за исключением $, ` (обратная кавычка) и \ (escape -- обратный слэш). [1] То, что символ $ попал в разряд исключений, позволяет выполнять обращение к переменным внутри строк, ограниченных двойными кавычками ("$variable"), т.е. выполнять подстановку значений переменных (см. Пример 4-1, выше).

Двойные кавычки могут быть использованы для предотвращения разбиения строки на слова. [2] Заключение строки в кавычки приводит к тому, что она передается как один аргумент, даже если она содержит пробельные символы - разделители.

```
variable1="a variable containing five words"
COMMAND This is $variable1    # Исполнение COMMAND с 7 входными аргументами:
# "This" "is" "a" "variable" "containing" "five" "words"

COMMAND "This is $variable1"  # Исполнение COMMAND с одним входным аргументом:
# "This is a variable containing five words"


variable2=""    # Пустая переменная.

COMMAND $variable2 $variable2 $variable2        # Исполнение COMMAND без аргументов.
COMMAND "$variable2" "$variable2" "$variable2"  # Исполнение COMMAND с 3 "пустыми" аргументами.
COMMAND "$variable2 $variable2 $variable2"      # Исполнение COMMAND с 1 аргументом (и 2 пробелами).
```


Tip	
Заключение в кавычки аргументов команды echo необходимо только в том случае, когда разбиение на отдельные слова сопряжено с определенными трудностями.

Пример. Вывод "причудливых" переменных

```
#!/bin/bash
# weirdvars.sh: Вывод "причудливых" переменных

var="'(]\\{}\$\""
echo $var        # '(]\{}$"
echo "$var"      # '(]\{}$"     Никаких различий.

echo

IFS='\'
echo $var        # '(] {}$"     \ символ-разделитель преобразован в пробел.
echo "$var"      # '(]\{}$"

# Примеры выше предоставлены S.C.

exit 0
```

Одиночные кавычки (' ') схожи по своему действию с двойными кавычками, только не допускают обращение к переменным, поскольку специальный символ "$" внутри одинарных кавычек воспринимается как обычный символ. Внутри одиночных кавычек, любой специальный символ, за исключением ', интерпретируется как простой символ. Одиночные кавычки ("строгие, или полные кавычки") следует рассматривать как более строгий вариант чем двойные кавычки ("нестрогие, или неполные кавычки").

Note	
Поскольку внутри одиночных кавычек даже экранирующий (\) символ воспринимается как обычный символ, попытка вывести одиночную кавычку внутри строки, ограниченной одинарными кавычками, не даст желаемого результата.
```
echo "Why can't I write 's between single quotes"

echo

# Обходной метод.
echo 'Why can'\''t I write '"'"'s between single quotes'
#    |-------|  |----------|   |-----------------------|
# Три строки, ограниченных одинарными кавычками,
# и экранированные одиночные кавычки между ними.
```


Экранирование -- это способ заключения в кавычки одиночного символа. Экранирующий (escape) символ (\) сообщает интерпретатору, что следующий за ним символ должен восприниматься как обычный символ.

Caution	
С отдельными командами и утилитами, такими как echo и sed, экранирующий символ может применяться для получения обратного эффекта - когда обычные символы при экранировании приобретают специальное значение.

Специальное назначение некоторых экранированных символов


используемых совместно с echo и sed
- \n - перевод строки (новая строка)
- \r - перевод каретки
- \t - табуляция
- \v - вертикальная табуляция
- \b - забой (backspace)
- \a - "звонок" (сигнал)
- \0xx - ASCII-символ с кодом 0xx в восьмеричном виде)

Пример. Экранированные символы

```
#!/bin/bash
# escaped.sh: экранированные символы

echo; echo

echo "\v\v\v\v"      # Вывод последовательности символов \v\v\v\v.
# Для вывода экранированных символов следует использовать ключ -e.
echo "============="
echo "ВЕРТИКАЛЬНАЯ ТАБУЛЯЦИЯ"
echo -e "\v\v\v\v"   # Вывод 4-х вертикальных табуляций.
echo "=============="

echo "КАВЫЧКИ"
echo -e "\042"       # Выводит символ " (кавычки с восьмеричным кодом ASCII 42).
echo "=============="

# Конструкция $'\X' делает использование ключа -e необязательным.
echo; echo "НОВАЯ СТРОКА И ЗВОНОК"
echo $'\n'           # Перевод строки.
echo $'\a'           # Звонок (сигнал).

echo "==============="
echo "КАВЫЧКИ"
# Bash версии 2 и выше допускает использование конструкции $'\nnn'.
# Обратите внимание: здесь под '\nnn' подразумевается восьмеричное значение.
echo $'\t \042 \t'   # Кавычки (") окруженные табуляцией.

# В конструкции $'\xhhh' допускается использовать и шестнадцатеричные значения.
echo $'\t \x22 \t'  # Кавычки (") окруженные табуляцией.
# Спасибо Greg Keraunen, за это примечание.
# Ранние версии Bash допускали употребление конструкции в виде '\x022'.
echo "==============="
echo


# Запись ASCII-символов в переменную.
# ----------------------------------------
quote=$'\042'        # запись символа " в переменную.
echo "$quote Эта часть строки ограничена кавычками, $quote а эта -- нет."

echo

# Конкатенация ASCII-символов в переменную.
triple_underline=$'\137\137\137'  # 137 -- это восьмеричный код символа '_'.
echo "$triple_underline ПОДЧЕРКИВАНИЕ $triple_underline"

echo

ABC=$'\101\102\103\010'           # 101, 102, 103 это  A, B и C соответственно.
echo $ABC

echo; echo

escape=$'\033'                    # 033 -- восьмеричный код экранирующего символа.
echo "\"escape\" выводится как $escape"
#                                   вывод отсутствует.

echo; echo

exit 0
```

- \" - кавычки

```
echo "Привет"                    # Привет
echo "Он сказал: \"Привет\"."    # Он сказал: "Привет".
```

- \$ - символ доллара (если за комбинацией символов \$ следует имя переменной, то она не будет разыменована)

```
echo "\$variable01"  # выведет $variable01
```

- \\- обратный слэш

```
echo "\\"  # выведет \
```

Note	
Поведение символа \ сильно зависит от того экранирован ли он, ограничен ли кавычками или находится внутри конструкции подстановки команды или во вложенном документе.

```
                      #  Простое экранирование и кавычки
echo \z               #  z
echo \\z              # \z
echo '\z'             # \z
echo '\\z'            # \\z
echo "\z"             # \z
echo "\\z"            # \z

                      #  Подстановка команды
echo `echo \z`        #  z
echo `echo \\z`       #  z
echo `echo \\\z`      # \z
echo `echo \\\\z`     # \z
echo `echo \\\\\\z`   # \z
echo `echo \\\\\\\z`  # \\z
echo `echo "\z"`      # \z
echo `echo "\\z"`     # \z

                      # Встроенный документ
cat <<EOF
\z
EOF                   # \z

cat <<EOF
\\z
EOF                   # \z
```


Отдельные символы в строке, которая записывается в переменную, могут быть экранированы, исключение составляет сам экранирующий символ.

```
variable=\
echo "$variable"
# Не работает - дает сообщение об ошибке:
# test.sh: : command not found
# В "чистом" виде экранирующий (escape) символ не может быть записан в переменную.
#
#  Фактически, в данном примере, происходит экранирование символа перевода строки
#+ в результате получается такая команда:   variable=echo "$variable"
#+                                          ошибочное присваивание

variable=\
23skidoo
echo "$variable"    #  23skidoo
                    #  Здесь все в порядке, поскольку вторая строка
                    #+ является нормальным, с точки зрения присваивания, выражением.

variable=\
#        \^    За escape-символом следует пробел
echo "$variable"        # пробел

variable=\\
echo "$variable"        # \

variable=\\\
echo "$variable"
# Не работает - сообщение об ошибке:
# test.sh: \: command not found
#
#  Первый escape-символ экранирует второй, а третий оказывается неэкранированным,
#+ результат тот же, что и в первом примере.

variable=\\\\
echo "$variable"        # \\
                        # Второй и четвертый escape-символы экранированы.
                        # Это нормально.
```

Экранирование пробелов предотвращает разбиение списка аргументов командной строки на отдельные аргументы.

```
file_list="/bin/cat /bin/gzip /bin/more /usr/bin/less /usr/bin/emacs-20.7"
# Список файлов как аргумент(ы) командной строки.

# Добавить два файла в список и вывести список.
ls -l /usr/X11R6/bin/xsetroot /sbin/dump $file_list

echo "-------------------------------------------------------------------------"

# Что произойдет, если экранировать пробелы в списке?
ls -l /usr/X11R6/bin/xsetroot\ /sbin/dump\ $file_list
# Ошибка: первые три файла будут "слиты" воедино
# и переданы команде 'ls -l' как один аргумент
# потому что два пробела, разделяющие аргументы (слова) -- экранированы.
```

Кроме того, escape-символ позволяет писать многострочные команды. Обычно, каждая команда занимает одну строку, но escape-символ позволяет экранировать символ перевода строки, в результате чего одна команда может занимать несколько строк.

```
(cd /source/directory && tar cf - . ) | \
(cd /dest/directory && tar xpvf -)
# Команда копирования дерева каталогов.
# Разбита на две строки для большей удобочитаемости.

# Альтернативный вариант:
tar cf - -C /source/directory . |
tar xpvf - -C /dest/directory
```

Note	
Если строка сценария заканчивается символом создания конвейера |, то необходимость в применении символа \, для экранирования перевода строки, отпадает. Тем не менее, считается хорошим тоном, всегда использовать символ "\" в конце промежуточных строк многострочных команд.

```
echo "foo
bar"
#foo
#bar

echo

echo 'foo
bar'    # Никаких различий.
#foo
#bar

echo

echo foo\
bar     # Перевод строки экранирован.
#foobar

echo

echo "foo\
bar"     # Внутри "нестрогих" кавычек символ "\" интерпретируется как экранирующий.
#foobar

echo

echo 'foo\
bar'     # В "строгих" кавычках обратный слэш воспринимается как обычный символ.
#foo\
#bar
```


Примечания
[1] Символ "!", помещенный в двойные кавычки, порождает сообщение об ошибке, если команда вводится с командной строки. Вероятно это связано с тем, что этот символ интерпретируется как попытка обращения к истории команд. Однако внутри сценариев такой прием проблем не вызывает.


Не менее любопытно поведение символа "\", употребляемого внутри двойных кавычек.

```
bash$ echo hello\!
hello!

bash$ echo "hello\!"
hello\!

bash$ echo -e x\ty
xty

bash$ echo -e "x\ty"
x       y
```             

[2] "Разбиение на слова", в данном случае это означает разделение строки символов на некоторое число аргументов.

# Работа со строками

Bash поддерживает на удивление большое количество операций над строками. К сожалению, этот раздел Bash испытывает недостаток унификации. Одни операции являются подмножеством операций подстановки параметров, а другие -- совпадают с функциональностью команды UNIX -- expr. Это приводит к противоречиям в синтаксисе команд и перекрытию функциональных возможностей, не говоря уже о возникающей путанице.

Длина строки

```
${#string}
expr length $string
expr "$string" : '.*'
stringZ=abcABC123ABCabc

echo ${#stringZ}                 # 15
echo `expr length $stringZ`      # 15
echo `expr "$stringZ" : '.*'`    # 15
```

Пример. Вставка пустых строк между параграфами в текстовом файле

```
#!/bin/bash
# paragraph-space.sh

# Вставка пустых строк между параграфами в текстовом файле.
# Порядок использования: $0 <FILENAME

MINLEN=45        # Возможно потребуется изменить это значение.
#  Строки, содержащие количество символов меньшее, чем $MINLEN
#+ принимаются за последнюю строку параграфа.

while read line  # Построчное чтение файла от начала до конца...
do
  echo "$line"   # Вывод строки.

  len=${#line}
  if [ "$len" -lt "$MINLEN" ]
    then echo    # Добавление пустой строки после последней строки параграфа.
  fi  
done

exit 0
```

Длина подстроки в строке (подсчет совпадающих символов ведется с начала строки)

```
expr match "$string" '$substring'
```

где $substring -- регулярное выражение.

```
expr "$string" : '$substring'
```
где $substring -- регулярное выражение.

```
stringZ=abcABC123ABCabc
#       |------|

echo `expr match "$stringZ" 'abc[A-Z]*.2'`   # 8
echo `expr "$stringZ" : 'abc[A-Z]*.2'`       # 8
```

Index

```
expr index $string $substring
```

Номер позиции первого совпадения в $string c первым символом в $substring.
```
stringZ=abcABC123ABCabc
echo `expr index "$stringZ" C12`             # 6
                                             # позиция символа C.

echo `expr index "$stringZ" 1c`              # 3
# символ 'c' (в #3 позиции) совпал раньше, чем '1'.
```

Эта функция довольно близка к функции strchr() в языке C.

#Извлечение подстроки

```
${string:position}
```
Извлекает подстроку из $string, начиная с позиции $position.

Если строка $string -- "*" или "@", то извлекается позиционный параметр (аргумент), [1] с номером $position.
```
${string:position:length}
```

Извлекает $length символов из $string, начиная с позиции $position.

```
stringZ=abcABC123ABCabc
#       0123456789.....
#       Индексация начинается с 0.

echo ${stringZ:0}                            # abcABC123ABCabc
echo ${stringZ:1}                            # bcABC123ABCabc
echo ${stringZ:7}                            # 23ABCabc

echo ${stringZ:7:3}                          # 23A
                                             # Извлекает 3 символа.

# Возможна ли индексация с "правой" стороны строки?

echo ${stringZ:-4}                           # abcABC123ABCabc
# По-умолчанию выводится полная строка.
# Однако . . .

echo ${stringZ:(-4)}                         # Cabc
echo ${stringZ: -4}                          # Cabc
# Теперь выводится правильно.
# Круглые скобки или дополнительный пробел "экранируют" параметр позиции.
```


Если $string -- "*" или "@", то извлекается до $length позиционных параметров (аргументов), начиная с $position.

```
echo ${*:2}          # Вывод 2-го и последующих аргументов.
echo ${@:2}          # То же самое.

echo ${*:2:3}        # Вывод 3-х аргументов, начиная со 2-го.
```

```
expr substr $string $position $length
```

Извлекает $length символов из $string, начиная с позиции $position.
```
stringZ=abcABC123ABCabc
#       123456789......
#       Индексация начинается с 1.

echo `expr substr $stringZ 1 2`              # ab
echo `expr substr $stringZ 4 3`              # ABC
```

```
expr match "$string" '\($substring\)'
```

Находит и извлекает первое совпадение $substring в $string, где $substring -- это регулярное выражение.

```
expr "$string" : '\($substring\)'
```

Находит и извлекает первое совпадение $substring в $string, где $substring -- это регулярное выражение.

```
stringZ=abcABC123ABCabc
#       =======

echo `expr match "$stringZ" '\(.[b-c]*[A-Z]..[0-9]\)'`   # abcABC1
echo `expr "$stringZ" : '\(.[b-c]*[A-Z]..[0-9]\)'`       # abcABC1
echo `expr "$stringZ" : '\(.......\)'`                   # abcABC1
```

Все вышеприведенные операции дают один и тот же результат.

```
expr match "$string" '.*\($substring\)'
```

Находит и извлекает первое совпадение $substring в $string, где $substring -- это регулярное выражение. Поиск начинается с конца $string.

```
expr "$string" : '.*\($substring\)'
```

Находит и извлекает первое совпадение $substring в $string, где $substring -- это регулярное выражение. Поиск начинается с конца $string.

```
stringZ=abcABC123ABCabc
#                ======

echo `expr match "$stringZ" '.*\([A-C][A-C][A-C][a-c]*\)'`    # ABCabc
echo `expr "$stringZ" : '.*\(......\)'`                       # ABCabc
```

# Удаление части строки

```
${string#substring}
```

Удаление самой короткой, из найденных, подстроки $substring в строке $string. Поиск ведется с начала строки

```
${string##substring}
```

Удаление самой длинной, из найденных, подстроки $substring в строке $string. Поиск ведется с начала строки

```
stringZ=abcABC123ABCabc
#       |----|
#       |----------|

echo ${stringZ#a*C}      # 123ABCabc
# Удаление самой короткой подстроки.

echo ${stringZ##a*C}     # abc
# Удаление самой длинной подстроки.
```

```
${string%substring}
```

Удаление самой короткой, из найденных, подстроки $substring в строке $string. Поиск ведется с конца строки

```
${string%%substring}
```

Удаление самой длинной, из найденных, подстроки $substring в строке $string. Поиск ведется с конца строки

```
stringZ=abcABC123ABCabc
#                    ||
#        |------------|

echo ${stringZ%b*c}      # abcABC123ABCa
# Удаляется самое короткое совпадение. Поиск ведется с конца $stringZ.

echo ${stringZ%%b*c}     # a
# Удаляется самое длинное совпадение. Поиск ведется с конца $stringZ.
```

Пример. Преобразование графических файлов из одного формата в другой, с изменением имени файла

```
#!/bin/bash
#  cvt.sh:
#  Преобразование всех файлов в заданном  каталоге,
#+ из графического формата MacPaint, в формат "pbm".

#  Используется утилита "macptopbm", входящая в состав пакета "netpbm",
#+ который сопровождается Brian Henderson (bryanh@giraffe-data.com).
#  Netpbm -- стандартный пакет для большинства дистрибутивов Linux.

OPERATION=macptopbm
SUFFIX=pbm          # Новое расширение файла.

if [ -n "$1" ]
then
  directory=$1      # Если каталог задан в командной строке при вызове сценария
else
  directory=$PWD    # Иначе просматривается текущий каталог.
fi

#  Все файлы в каталоге, имеющие расширение ".mac", считаются файлами
#+ формата  MacPaint.

for file in $directory/* # Подстановка имен файлов.
do
  filename=${file%.*c}   #  Удалить расширение ".mac" из имени файла
                         #+ ( с шаблоном '.*c' совпадают все подстроки
                         #+ начинающиеся с '.' и заканчивающиеся 'c',
  $OPERATION $file > "$filename.$SUFFIX"
                         # Преобразование с перенаправлением в файл с новым именем
  rm -f $file            # Удаление оригинального файла после преобразования.
  echo "$filename.$SUFFIX"  # Вывод на stdout.
done

exit 0
```


# Замена подстроки

```
${string/substring/replacement}
```

Замещает первое вхождение $substring строкой $replacement.

```
${string//substring/replacement}
```

Замещает все вхождения $substring строкой $replacement.

```
stringZ=abcABC123ABCabc

echo ${stringZ/abc/xyz}           # xyzABC123ABCabc
                                  # Замена первой подстроки 'abc' строкой 'xyz'.

echo ${stringZ//abc/xyz}          # xyzABC123ABCxyz
                                  # Замена всех подстрок 'abc' строкой 'xyz'.
```

```
${string/#substring/replacement}
```

Подстановка строки $replacement вместо $substring. Поиск ведется с начала строки $string.

```
${string/%substring/replacement}
```

Подстановка строки $replacement вместо $substring. Поиск ведется с конца строки $string.

```
stringZ=abcABC123ABCabc

echo ${stringZ/#abc/XYZ}          # XYZABC123ABCabc
                                  # Поиск ведется с начала строки

echo ${stringZ/%abc/XYZ}          # abcABC123ABCXYZ
                                  # Поиск ведется с конца строки
```

# Использование awk при работе со строками
В качестве альтернативы, Bash-скрипты могут использовать средства awk при работе со строками.

Пример. Альтернативный способ извлечения подстрок

```
#!/bin/bash
# substring-extraction.sh

String=23skidoo1
#      012345678    Bash
#      123456789    awk
# Обратите внимание на различия в индексации:
# Bash начинает индексацию с '0'.
# Awk  начинает индексацию с '1'.

echo ${String:2:4} # с 3 позиции (0-1-2), 4 символа
                   # skid

# В эквивалент в awk: substr(string,pos,length).
echo | awk '
{ print substr("'"${String}"'",3,4)      # skid
}
'
#  Передача пустого "echo" по каналу в awk, означает фиктивный ввод,
#+ делая, тем самым, ненужным предоставление имени файла.

exit 0
```

