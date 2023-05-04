# Задание

Написать скрипт в котором реализовать:

1. Функцию Print_0_255, которая выводит числа от 0 до 255.
2. Функцию Print_A_B от двух аргументов, которая выводит числа от A до B.
3. Функцию SolveX2 от трех аргументов A, B, C, которая решает квадратное уравнение и выводит значения его корней.
4. Функцию TowerN от двух аргумента N и С, которая выводит N строк, содержащих по \[1..N\] символов, например, если С=\#, а N=11: 

```
#
##
###
####
#####
######
#######
########
#########
##########
########### 
```

# Справочная система

Подробнее смотрите [здесь](https://learn.microsoft.com/ru-ru/powershell/scripting/learn/ps101/02-help-system?view=powershell-7.3) 

## Get-Help

Get-Help — это многоцелевая команда. Get-Help помогает научиться использовать найденные команды. Get-Help также можно использовать для поиска команд, но, по сравнению с Get-Command, другим, менее прямым способом.

Если для поиска команд используется Get-Help, сначала выполняется поиск совпадений с подстановочными знаками имен команд на основе предоставленных входных данных. Если командлет не находит совпадение, выполняется поиск по самим разделам справки, и, если совпадений не найдено, ошибка возвращается. Вопреки распространенному мнению, Get-Help можно использовать для поиска команд, не содержащих разделов справки.

Следующая команда позволяет вывести раздел справки для cd.

```
PS C:\Windows\System32> get-help -Name cd

NAME
    Set-Location

SYNTAX
    Set-Location [[-Path] <string>] [-PassThru] [<CommonParameters>]

    Set-Location -LiteralPath <string> [-PassThru] [<CommonParameters>]

    Set-Location [-PassThru] [-StackName <string>] [<CommonParameters>]


ALIASES
    sl
    cd
    chdir


REMARKS
    Get-Help cannot find the Help files for this cmdlet on this computer. It is displaying only partial help.
        -- To download and install Help files for the module that includes this cmdlet, use Update-Help.
        -- To view the Help topic for this cmdlet online, type: "Get-Help Set-Location -Online" or
           go to https://go.microsoft.com/fwlink/?LinkID=2097049.

```

Попробуйте выполнить этот пример на компьютере, просмотрите выходные данные и запишите, как группируются сведения.

    NAME
    Краткий обзор
    SYNTAX
    DESCRIPTION
    Связанные ссылки
    ПРИМЕЧАНИЯ

Как вы видите, разделы справки могут содержать большой объем информации, причем это даже не весь раздел справки.

При указании параметра Full в значении Get-Help возвращается весь раздел справки.

```
Get-Help -Name Get-Help -Full
```
Попробуйте выполнить этот пример на компьютере, просмотрите выходные данные и запишите, как группируются сведения.

## Get-Command

Get-Command облегчает поиск команд. Выполнение Get-Command без указания параметров возвращает список всех команд в системе. В приведенном ниже примере показано использование командлета Get-Command, который определяет команды, работающие с процессами.

```
Get-Command -Noun Process
```

```
CommandType     Name                                               Version    Source
-----------     ----                                               -------    ------
Cmdlet          Debug-Process                                      3.1.0.0    Microsof...
Cmdlet          Get-Process                                        3.1.0.0    Microsof...
Cmdlet          Start-Process                                      3.1.0.0    Microsof...
Cmdlet          Stop-Process                                       3.1.0.0    Microsof...
Cmdlet          Wait-Process                                       3.1.0.0    Microsof...
```

Заметьте: в предыдущем примере, где выполнялся командлет Get-Command, используется параметр Noun, а командлет Process указан в качестве значения для параметра Noun. Что делать, если вы не знаете, как использовать командлет Get-Command? Get-Help можно использовать для вывода раздела справки для Get-Command.

Параметры Name, Noun и Verb поддерживают добавление подстановочных знаков. В приведенном ниже примере показаны подстановочные знаки, используемые с параметром Name.

```
Get-Command -Name *service*
```

## Get-Member

Get-Member помогает определить доступные для команд объекты, свойства и методы. Все команды, создающие объектно-ориентированные выходные данные, могут быть переданы в Get-Member. Свойство — это характеристика элемента. У водительского удостоверения есть свойство, называемое цветом глаз, а самыми распространенными значениями для этого свойства являются цвета: синий и карий. Метод — это действие, которое может быть выполнено с элементом. 

# О функциях

Если вы пишете однострочные коды и сценарии PowerShell, которые часто приходится изменять для применения в различных ситуациях, возможно, их стоит преобразовать в многократно используемую функцию.

## Именование

При именовании функций в PowerShell используйте имя в Регистр Pascal с утвержденным глаголом и существительным в единственном числе. Кроме того, перед существительным рекомендуется добавить префикс. Например: <ApprovedVerb>-<Prefix><SingularNoun>.

В PowerShell есть конкретный список утвержденных глаголов, которые можно получить, выполнив Get-Verb.

```
Get-Verb | Sort-Object -Property Verb
```

## Простая функция

Функция в PowerShell объявляется с помощью ключевого слова function, за которым следует имя функции, а затем — открывающая и закрывающая фигурные скобки. В этих скобках содержится код, который будет выполнять функция.

```
function Get-Version {
    $PSVersionTable.PSVersion
}
```

Показан простой пример функции, возвращающей версию PowerShell.

```
Get-Version
```

```
Major  Minor  Build  Revision
-----  -----  -----  --------
5      1      14393  693
```

## Параметры

PowerShell поддерживает традиционный и расширенный форматы описания параметров функций.

Традиционный:

```
function hello($name) {
  write-host "Hello, $name"
}
hello("World")      # традиционный синтаксис вызова функции (неправильно)
hello -name "World" # вызов в стиле PowerShell (правильно)
```

Результат вызова будет идентичен.

Чуть сложнее – на вход два параметра и немного логики внутри (-gt значит greater than, т.е. “>”):

```
function hello($name1, $name2) {
        if ($name1.length -gt 3) {
               write-host "Параметр 1 длиннее 3"
        } else {
               write-host "Параметр 1 короче 4"
        }
}
hello("ABCD", "ABCDE") # традиционный синтаксис вызова функции (неправильно)
hello -name1 "ABCD" -name2 "ABCDE" # вызов в стиле PowerShell (правильно)
```

При выполнении мы увидим разные результаты выполнения:

```
Параметр 1 короче 4
Параметр 1 длиннее 3
```

Предполагается, что первый параметр ABCD и его длина 4 символа. Почему при вызове со скобками он вдруг “короче 4”?

Любители типизации входных параметров, результат получат вроде логичнее, но это только замаскирует ошибку:

```
function hello([string]$name1, [string]$name2) {
...
Параметр 1 длиннее 3
Параметр 1 длиннее 3
```

Чуть изменим функцию (добавим первой строкой вывод типа и значения параметра):

```
function hello($name1, $name2) {
  write-host $name1
  write-host $name1.GetType()
  ...
```

Вызовем заново, посмотрим:

```
ABCD ABCDE
System.Object[]
Параметр 1 короче 4

ABCD
System.String
Параметр 1 длиннее 3
```

Т.е. оказывается вызов: hello("ABCD", "ABCDE") в первый параметр отправляет массив каких-то объектов, а мы-то ждем, что туда попадет строка «ABCD».Вызов:

```
hello("ABCD", "ABCDE")
```
Полностью эквивалентен:

```
hello -name1 @("ABCD", "ABCDE") -name2 $null
```

Т.е. любое перечисление в скобках – это всегда массив (точнее список).

Пример будет корректно работать так:

```
hello "Вася" "Костя"
```

Но надежнее так:

```
hello -name1 "Вася" -name2 "Костя"
```

## Передача значения по-умолчанию

Тут все совсем просто, т.к. синтаксис не отличается от других языков

function hello($name1="ABCD", $name2=$null) {
  write-host $name1, $name2
}

hello -name2 "ABCDE"
соотв. получим:

ABCD ABCDE

## Передача параметра по ссылке ([ref])

Передача параметра по ссылке делается приведением к типу [ref] (это специальный класс PowerShell). Две особенности:

* обращаться к переданному по ссылке объекту/параметру нужно через свойство .Value
* при вызове функции заключать параметр вместе с [ref] в скобки, иначе будут передаваться отдельно класс [ref] и отдельно значение параметра, списком из 2-х элементов (те грабли, о которых писал выше).

Пример:
```
function Test([ref]$aParam) {
  $aParam.Value = "Bye!"
}
$par = "Hello!"
Test -aParam ([ref]$par)
write-host $par
```

На экране будет “Bye!”

## Расширенный формат

Функцию в PowerShell можно легко преобразовать в расширенную. Одно из различий между функцией и расширенной функцией заключается в том, что расширенные функции имеют ряд общих параметров, которые добавляются в функцию автоматически. К этим общим параметрам относятся такие параметры, как Verbose и Debug.

```
function Hello {
    param (
        [string]$name1,
        [string]$name2
    )

    Write-Output "$name1-$name2"
}

Hello -name1 ABC -name2 ABCDE
```

На экране:
```
ABC-ABCDE
```

## Проверка параметров

Входные данные следует проверять в самом начале. Для чего продолжать выполнение кода, если это невозможно сделать без допустимых входных данных?

* [Parameter(Mandatory=$true)] - обязательность аргумента. Теперь, когда потребуется значение параметра ComputerName, которое пока не задано, функция выведет соответствующий запрос.

```
function Hello {
    param (
        [Parameter(Mandatory=$true)]
        [string]$name1,

        [string]$name2="QWERTY"
    )

    Write-Output "$name1-$name2"
}

Hello -name1 ABC -name2 ABCDE
Hello 
```
Вывод:
```
Hello -name1 ABC -name2 ABCDE
Hello 
ABC-ABCDE
Командлет Hello в конвейере команд в позиции 1
Укажите значения для следующих параметров:
name1: 123
123-QWERTY
```

* [ValidateNotNullOrEmpty()] - проверка значения на null или пусто.

```
function Hello {
    param (
        [Parameter(Mandatory=$true)]
        [string]$name1,

        [ValidateNotNullOrEmpty()]
        [string]$name2="QWERTY"
    )

    Write-Output "$name1-$name2"
}

Hello -name1 ABC -name2 ABCDE
Hello -name1 ABC -name2 ""
```

Вывод:

```
Hello -name1 ABC -name2 ABCDE
Hello -name1 ABC -name2 ""
ABC-ABCDE
Hello : Не удается проверить аргумент для параметра "name2". Аргумент пустой или имеет значение NULL. Укажите непусто
й аргумент, не имеющий значение NULL, после чего повторите выполнение команды.
строка:14 знак:25
+ Hello -name1 ABC -name2 ""
+                         ~~
    + CategoryInfo          : InvalidData: (:) [Hello], ParameterBindingValidationException
    + FullyQualifiedErrorId : ParameterArgumentValidationError,Hello
```
* [Parameter(Mandatory, ValueFromPipeline)] - значение перехватывается из потока данных (перенаправление)

Подробнее смотри [здесь](https://learn.microsoft.com/ru-ru/powershell/scripting/learn/ps101/09-functions?view=powershell-7.3)

# О циклах

Подробнее смотрите [здесь](https://learn.microsoft.com/ru-ru/powershell/scripting/learn/ps101/06-flow-control?view=powershell-7.3)

Когда вы переходите от написания однострочных элементов кода PowerShell к написанию сценариев, это может показаться намного сложнее, чем есть на самом деле. Сценарий — это всего лишь те же или похожие команды, которые вы можете запускать в интерактивном режиме в консоли PowerShell, если они не сохранены в виде файла .PS1. Существуют некоторые конструкции написания сценариев, которые вы можете использовать вместо командлета ForEach-Object, например цикл foreach. Эти различия могут ввести в заблуждение начинающих пользователей, особенно если они полагают, что foreach является конструкцией сценария и псевдонимом для командлета ForEach-Object.

## For

Цикл for выполняет итерацию до тех пор, пока заданное условие имеет значение "Истина". Цикл for я использую редко, но все же использую.

```for ($i = 1; $i -lt 5; $i++) {
  Write-Output "Sleeping for $i seconds"
  Start-Sleep -Seconds $i
}
```

```
Sleeping for 1 seconds
Sleeping for 2 seconds
Sleeping for 3 seconds
Sleeping for 4 seconds
```

В предыдущем примере цикл будет повторяться четыре раза, начиная с числа 1, и продолжит выполняться до тех пор, пока переменная счетчика $i не составит значение менее 5. Этот цикл будет бездействовать в течение всего 10 секунд.

## Do

В PowerShell существуют два разных цикла do. Цикл Do Until выполняется до тех пор, пока заданное условие имеет значение "Ложь".

```
$number = Get-Random -Minimum 1 -Maximum 10
do {
  $guess = Read-Host -Prompt "What's your guess?"
  if ($guess -lt $number) {
    Write-Output 'Too low!'
  }
  elseif ($guess -gt $number) {
    Write-Output 'Too high!'
  }
}
until ($guess -eq $number)
```

```
What's your guess?: 1
Too low!
What's your guess?: 2
Too low!
What's your guess?: 3
```

В предыдущем примере показана игра с цифрами, которая будет продолжаться до тех пор, пока значение, которое вы выдаете, не будет равняться тому же числу, которое было создано командлетом Get-Random.

Цикл Do While — полная противоположность. Он выполняется до тех пор, пока заданное условие не примет значение "Истина".


```
$number = Get-Random -Minimum 1 -Maximum 10
do {
  $guess = Read-Host -Prompt "What's your guess?"
  if ($guess -lt $number) {
    Write-Output 'Too low!'
  } elseif ($guess -gt $number) {
    Write-Output 'Too high!'
  }
}
while ($guess -ne $number)
```

```
What's your guess?: 1
Too low!
What's your guess?: 2
Too low!
What's your guess?: 3
Too low!
What's your guess?: 4
```

Аналогичные результаты можно получить с помощью цикла Do While, изменив условие проверки на значение "Не равно".

Циклы Do всегда выполняются не менее одного раза, так как заданное условие вычисляется в конце цикла.

## While

Цикл Do While, как и цикл While, выполняется до тех пор, пока заданное условие имеет значение "Истина". При этом различие состоит в том, что цикл While принимает условие в верхней части цикла перед выполнением любого кода. Поэтому этот цикл не выполняется, если условие принимает значение "Ложь".

```
$date = Get-Date -Date 'November 22'
while ($date.DayOfWeek -ne 'Thursday') {
  $date = $date.AddDays(1)
}
Write-Output $date
```

```
Thursday, November 23, 2017 12:00:00 AM
```

## Break

Инструкция Break используется для прерывания цикла. Кроме того, она часто используется с инструкцией switch.

```
for ($i = 1; $i -lt 5; $i++) {
  Write-Output "Sleeping for $i seconds"
  Start-Sleep -Seconds $i
  break
}
```

```
Sleeping for 1 seconds
```

Инструкция break, показанная в предыдущем примере, приводит к выходу цикла в первой итерации.

## Continue 

Инструкция continue позволяет перейти к следующей итерации цикла.

```
while ($i -lt 5) {
  $i += 1
  if ($i -eq 3) {
    continue
  }
  Write-Output $i
}
```

```
1
2
4
5
```

В предыдущем примере выводятся числа 1, 2, 4 и 5. В нем исключается число 3 и выполняется переход к следующей итерации цикла. Инструкция break, как и continue, прерывает цикл, только если это не текущая итерация. Затем процесс переходит к следующей итерации, а не к прерыванию и остановке цикла.

## return

Инструкция return позволяет выйти из существующей области.

```
$number = 1..10
foreach ($n in $number) {
  if ($n -ge 4) {
    Return $n
  }
}
```

```
4
```

Обратите внимание, что в предыдущем примере функция return выводит первый результат, а затем выходит из цикла. 

## Арифметические операции

Подробнее смотри [здесь](https://learn.microsoft.com/ru-ru/powershell/module/microsoft.powershell.core/about/about_arithmetic_operators?view=powershell-7.3)