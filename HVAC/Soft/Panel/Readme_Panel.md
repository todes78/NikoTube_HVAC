# Программное обеспечение панели СУ

Графическая панель **MT8090XE** (Weintek).

Программное обеспечение EasyBuilder Pro v6.03.02.294 Build 2019.12.03

## Описание экранов

10 - Background

11 - Main, главный экрна   

12 - Parameters, экран параметров

13 - Cooling, экран системы охлаждения

14 - Messages, экран списка сообщений

15 - Graphics, экран графиков

16 - Tools, экрна настроек

20 - F_Fault1, экран сообщений первых аварий СУ

91 - Terminal, экран терминала

92 - TerminalOption, экран настройки терминала


## Реализация макросов

### Макрос инициализации

Выполнется при загрузке графической панели 





## Реализация экранов

### Background (10)

Выбор меню осуществляется с помощью кнопок CB0..CB5 

LB0 - состояние кнопки "Главная"

LB1 - состояние кнопки "Параметры"

LB2 - состояние кнопки "Система охлаждения"

LB3 - состояние кнопки ""

LB4 - состояние кнопки ""

LB5 - состояние кнопки ""


**Описание переменных:**

Битовые переменные

LB0 - бит выбора Главного окна

LB1 - бит выбора окна параметров

LB1000 - бит, определяющий, что работает терминал

Числовые переменные LW

LW0 - 

LW1033 - 


**Макросы:**

Start

```pascal
macro_command main()
bool menu_button 
// Initialization menu button on start system
menu_button = true
SetData(menu_button, "Local HMI", LB, 0, 1)
end macro_command
```

MessageWindow_Init

```pascal
macro_command main()
int s
s = 20 
SetDataEx(s, "Local HMI", LW, 30, 1)
end macro_command
```

WindowSlider

```pascal
macro_command main()
short base_win, touch_pos[4], distance
GetData(touch_pos[0], "Local HMI", LW, 9042, 4)
distance=touch_pos[0]-touch_pos[2] //X is the moving distance
if touch_pos[1]>200 then //if Y > 400
  if distance > 100 then //if moving distance <-100
    GetData(base_win, "Local HMI", LW, 13, 1)
    base_win=base_win + 1 //current base window +1
    
    if base_win > 223 then //if current base window >16
      base_win = 221 //current base window =10
    end if
//    SetData(base_win, "Local HMI", LW, 20, 1) //send value in base win to LW20
    SetData(base_win, "Local HMI", LW, 13, 1) //send value in base win to LW20
  end if
  
  if distance < -100 then
    GetData(base_win, "Local HMI", LW, 13, 1)
    base_win=base_win - 1
    if base_win < 221 then
       base_win = 223
    end if
//    SetData(base_win, "Local HMI", LW, 20, 1)            
    SetData(base_win, "Local HMI", LW, 13, 1) //send value in base win to LW20
  end if
//  SetData(distance, "Local HMI", LW, 22, 1) //send value of distance to LW22
end if
end macro_command

```

TerminalInit

```pascal
macro_command main()
bool bb
short symbol
// Initialization terminal variables
bb=1
symbol = 0x000A
SetData(bb, "Local HMI", LB, 1001, 1)
SetData(symbol, "Local HMI", LW, 1016, 1)
end macro_command
```




## 