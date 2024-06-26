# К_11 Моделирование движения по перекрестку

Дана система, состоящая из следующих элементов:
+ пульта управления светофором;
+ светофора;
+ участка дороги с разметкой и перекрестком;
+ автомобилей;
+ экрана отображения информации о функционировании системы.

Участок дороги имеет двухстороннее движение по одной полосе в каждом направлении. Каждая полоса участка дороги размечена на квадраты. В каждом квадрате может расположится один автомобиль. В центре перекрестка висит светофор. Светофор является центром системы координат. Тогда система координат по пять квадратов в каждом направлении выглядит согласно приведенному в таблице 1.

Таблица 1.
| | | | |-1, 5|1, 5| | | | |
|-|--|-|-|-|-|-|-|-|-|
| | | | |-1, 4|1, 4| | | | |
| | | | |-1, 3|1, 3| | | | |
| | | | |-1, 2|1, 2| | | | |
|-5, 1|-4, 1|-3, 1|-2, 1|-1, 1|1, 1|2, 1|3, 1|4, 1|5, 1|
|-5, -1|-4, -1|-3, -1|-2, -1|-1, -1|1, -1|2, -1|3, -1|4, -1|5, -1|
| | | | |-1, -2|1, -2| | | |
| | | | |-1, -3|1, -3| | | |
| | | | |-1, -4|1, -4| | | |
| | | | |-1, -5|1, -5| | | |

Система функционирует по тактам. Допускаем, что все автомобили двигаются одинаковой скоростью. За один такт автомобиль может переместится на следующий квадрат по направлению движения.

Назовем квадраты с координатами (1,1), (1,-1), (-1,1), (-1,-1) – квадратами перекрестка.

Линии остановки перед светофором расположены перед квадратами перекрестка согласно направлению движения.

Правила функционирования системы:
1. Размер участка задается параметром n. Значение n это количество квадратов на полосе на каждом направлении от центра.
2. Каждый автомобиль имеет уникальный номер. В номере используются буквы латинского алфавита.
3. Первоначально автомобили расположены на квадратах вне квадратов перекрестка.
4. Если автомобиль по направлению движения выходит за рамки участка, то он покидает систему (удаляется из системы).
5. Первоначально зеленый цвет светофора включен для горизонтальных полос (см. таб. 1).
6. При включении желтого цвета после зеленного автомобили останавливаются перед линями остановки.  Если автомобиль находится на квадрате перекрестка, то продолжает движение. Жёлтый цвет всегда горит в течении двух тактов.
7. Продолжительность в тактах зеленого и красного цвета меняются по командам  пульта управления светофором. Установка выполняется относительно горизонтальных полос (см. таб. 1). Если, команды нет, то используется уже установленное значение.
8. В течении такта автомобиль передвигается на очередной квадрат по направлению движения, если этот квадрат не занят. Допускаем, что все автомобили движутся по прямой, не поворачивая на перекрестке.
9. В начале такта система может получить и отработать команду.

Надо моделировать работу данной системы. В рамках одного такта отрабатывает одна команда и автомобили выполняют допустимые движения.

## Команды системы.

Команда переключения светофора.
> Switching traffic lights «обозначение цвета» «количество тактов»

Данная команда моделирует установку продолжительности цвета светофора с пульта управления. Продолжительность желтого цвета всегда два такта и по команде не меняется (соответствующая команда в систему не поступает). Зеленый или красный цвет могут гореть от двух тактов и более. Другие значения «количество тактов» игнорируются. Установка всегда выполняется в начале такта. Если новая команда установки поступает раньше, чем отработают отмеченное в последней команде «количество тактов», то установка всё равно выполняется. По истечении количества тактов согласно последней команде, переключение выполняется на автомате согласно циклу зеленная, желтая, красная и желтая, а за количество тактов берутся значения из последней команды установки соответствующего цвета. Первоначально, для зеленного и красного цвета установлены по пять тактов.

Команда вывода координаты автомобиля.
> Car «номер автомобиля»

По данной команде выдается номер и координата автомобиля.
> Car «номер автомобиля» ( «цело число», «цело число» )

Если автомобиль уже покинул участок дороги, то выдается сообщение:
> «номер автомобиля» the car left the road section

Команда выдачи состояния системы.
> Display the system status

По данной команде выводиться состояние системы. С новой строки, цвет светофора относительно горизонтальных полос (см. таб. 1):
> Traffic light color is «цвет светофора»

Далее построчно список автомобилей на участке дороги, упорядоченные по номерам и их координаты:
> «номер автомобиля» ( «цело число», «цело число» )

Пустая команда (строка ничего не содержит). Элементы системы выполняют действия согласно такту.

Команда завершения работы системы.
> Turn off the system

## Построить программу-систему, которая использует объекты:
1. Объект «система».
2. Объект для чтения исходных данных и команд. Объект моделирует работу оператора пульта управления светофором и водителей. Считывает данные для первоначальной подготовки и настройки системы. Считывает команды. После чтения очередной порции данных для настройки или данных команды, объект выдает сигнал с текстом полученных данных. Все данные настройки и данные команд по структуре корректны. Каждая строка команд соответствует одному такту (отрабатывается в начале такта). Если строка пустая, то система отрабатывает один такт (все элементы системы отрабатывают положенные действия или находятся в состоянии ожидания).
3. Объект пульта управления моделирует работу оператора пульта управления светофором, отрабатывает переключение светофора по команде или на автомате. Объект выдает соответствующий сигнал для светофора.
4. Объект, моделирующий участок дороги. Содержит размер участка. К данному объекту по иерархии подчинены автомобили, находящиеся на участке. Содержит разметку по квадратам.
5. Объект, моделирующий автомобиль. Содержит информацию о координате местоположения автомобиля. Выдает сигнал для определения свободен квадрат перед автомобилем или нет. По команде вывода состояния автомобиля, формирует текст и выдает сигнал к устройству вывода.
6. Объект для вывода информации. Текст для вывода объект получает по сигналу от других объектов системы. Каждое присланное сообщение выводиться с новой строки.

## Архитектура иерархи объектов.
+ Система движения по перекрестку
    + Объект ввода.
    + Пульт управления светофором.
        + Светофор.
    + Участок дороги.
        + Автомобиль 1.
        + Автомобиль 2.
        + . . . . .
        + Автомобиль m.
    + Объект вывода.

## Написать программу-систему, реализующую следующий алгоритм:

1. Вызов от объекта «система» метода build_tree_objects().
    + 1.1. Построение исходного дерева иерархии объектов. После построения любой объект перевести в состоянии готовности.
    + 1.2. Цикл для обработки вводимых данных.
        + 1.2.1. Выдача сигнала объекту чтения для ввода очередной строки данных (количество квадратов на полосе, номера автомобилей на участке и координаты первоначального расположения). Допускаем, что номера автомобилей различны, координаты заданы корректно.
        + 1.2.2. Отработка операции чтения очередной строки входных данных.
        + 1.2.3. Создание нового объекта. Установка связей сигналов и обработчиков с новым объектом.
    + 1.3. Установка связей сигналов и обработчиков между объектами.
2. Вызов от объекта «система» метода exec_app().
    + 2.1. Цикл для обработки вводимых команд.
        + 2.1.1. Определение номера очередного такта.
        + 2.1.2. Выдача сигнала объекту ввода для чтения очередной команды.
        + 2.1.3. Отработка команды.
        + 2.1.4. Отработка действий согласно такту.
    + 2.2. После ввода команды «Turn off the system» завершить работу.

Все приведенные сигналы и соответствующие обработчики должны быть реализованы. Запрос от объекта означает выдачу сигнала. Все сообщения на консоль выводятся с новой строки.

В набор поддерживаемых команд добавить команду «SHOWTREE» и по этой команде вывести дерево иерархии объектов системы с отметкой о готовности и завершить работу системы (программы). Реализовать отладочный тест такой командой.

При решении задачи необходимо руководствоваться методическим пособием и приложением к методическому пособию

## Входные данные
Первая строка, размер участка n:
> «цело число»

Начиная со второй строки, построчно вводятся номер автомобиля и координаты исходного расположения на участке дороги:
> «номер автомобиля» «цело число» «цело число»

Ввод номеров автомобилей заканчивается вводом текста:
> End of cars

Далее построчно вводятся команды. Они могут следовать в произвольном порядке.

Команда переключения светофора.
> Switching traffic lights «green | red» «количество тактов»

Команда вывода информации об автомобиле.
> Car «номер автомобиля»

Команда вывода информации о системе.
> Display system status information

Команда завершения работы системы.
> Turn off the system

Пример ввода:
```
r001xx-77 -1 -3
r003xx-77 1 -4
End of cars
Display the system status
Switching traffic lights red 3
Switching traffic lights green 3
Car r001xx-77
Car r003xx-77


Display the system status


Display the system status
Turn off the system
```

## Выходные данные
После завершения ввода исходных данных выводиться текст:
> Ready to work

После команды вывода информации об автомобиле могут быть выданы разные сообщения. Если автомобиль находиться на участке дороги:
> Car «номер автомобиля» («цело число», «цело число»)

Если автомобиль покинул участок дороги:
> «номер автомобиля» the car left the road section

После команды запроса информации о системе, с новой строки, цвет светофора:
> Traffic light color is «цвет светофора»

Далее построчно список автомобилей на участке дороги, упорядоченные по номерам и их координаты:
> «номер автомобиля» ( «цело число», «цело число» )

Пример вывода:
```
Ready to work
Traffic light color is green
r001xx-77 ( -1, -3 )
r003xx-77 ( 1, -4 )
r001xx-77 the car left the road section
Car r003xx-77 ( 1, -2 )
Traffic light color is red
r003xx-77 ( 1, 1 )
Traffic light color is yellow
r003xx-77 ( 1, 4 )
Turn off the system
```
---
### Автор - https://vk.com/aptekkkar / https://t.me/glaucine 
   + Прямой путь по ID: https://vk.com/id169607065
