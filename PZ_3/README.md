# Практическое задание №3
Журавлева Юлия БИСО-01-20

# Основы обработки данных с помощью R с использованием экосистемы tidyverse.

## Цель работы

1.  Закрепить практические навыки использования языка программирования R
    для обработки данных.
2.  Закрепить знания основных функций обработки данных экосистемы
    tidyverse языка R.
3.  Развить пркатические навыки использования функций обработки данных
    пакета dplyr – функции select(), filter(), mutate(), arrange(),
    group_by().

<<<<<<< HEAD
## Исходные данные

1.  ОС Windows 10
2.  RStudio Desktop
3.  Интерпретатор языка R 4.2.2
4.  Пакет `dplyr`
5.  Пакет `nycflights13`

## План

1.  Установить пакет `dplyr`
2.  Установить пакет `nycflights13`
3.  Выполнить задания и ответить на поставленные вопросы.

=======
>>>>>>> 37f4aadac1e2673d811897aabc017933e739cf98
## Ход выполнения работы

№1. Сколько встроенных в пакет nycflights13 датафреймов? ***Ответ:*** 5
датафреймов. Можно узнать, нажав Tab после введения команды
`nycflights13::`.

    - nycflights13::airlines
    - nycflights13::airports
    - nycflights13::flight
    - nycflights13::planes
    - nycflights13::weather

№2. Сколько строк в каждом датафрейме?

    > airlines %>% nrow()
    [1] 16

    > airports %>% nrow()
    [1] 1458

    > flights %>% nrow()
    [1] 336776

    > planes %>% nrow()
    [1] 3322

    > weather %>% nrow()
    [1] 26115

№3. Сколько столбцов в каждом датафрейме?

    > airlines %>% ncol()
    [1] 2

    > airports %>% ncol()
    [1] 8

    > flights %>% ncol()
    [1] 19

    > planes %>% ncol()
    [1] 9

    > weather %>% ncol()
    [1] 15

№4. Как просмотреть примерный вид датафрейма?

    > airlines %>% head()
    # A tibble: 6 × 2
      carrier name                    
      <chr>   <chr>                   
    1 9E      Endeavor Air Inc.       
    2 AA      American Airlines Inc.  
    3 AS      Alaska Airlines Inc.    
    4 B6      JetBlue Airways         
    5 DL      Delta Air Lines Inc.    
    6 EV      ExpressJet Airlines Inc.



    > airlines %>% glimpse()
    Rows: 16
    Columns: 2
    $ carrier <chr> "9E", "AA", "AS", "B6", "DL", "EV", "F9", "FL", "HA", "MQ", "OO", "…
    $ name    <chr> "Endeavor Air Inc.", "American Airlines Inc.", "Alaska Airlines Inc…

№5. Сколько компаний-перевозчиков (carrier) учитывают эти наборы данных
(представлено в наборах данных)?

    > airlines %>% nrow()
    [1] 16

№6. Сколько рейсов принял аэропорт John F Kennedy Intl в мае?

    > flights %>% filter(origin=='JFK', month==5) %>% nrow()
    [1] 9397

№7. Какой самый северный аэропорт?

    > airports %>% filter(lat == max(lat))
    # A tibble: 1 × 8
      faa   name                      lat   lon   alt    tz dst   tzone
      <chr> <chr>                   <dbl> <dbl> <dbl> <dbl> <chr> <chr>
    1 EEN   Dillant Hopkins Airport  72.3  42.9   149    -5 A     NA   

№8. Какой аэропорт самый высокогорный (находится выше всех над уровнем
моря)?

    > airports %>% filter(alt == max(alt))
    # A tibble: 1 × 8
      faa   name        lat   lon   alt    tz dst   tzone         
      <chr> <chr>     <dbl> <dbl> <dbl> <dbl> <chr> <chr>         
    1 TEX   Telluride  38.0 -108.  9078    -7 A     America/Denver

№9. Какие бортовые номера у самых старых самолетов?

    > planes %>% filter(year == min(year, na.rm = TRUE)) %>% select(tailnum)
    # A tibble: 1 × 1
      tailnum
      <chr>  
    1 N381AA 

№10. Какая средняя температура воздуха была в сентябре в аэропорту John
F Kennedy Intl (в градусах Цельсия).

    > weather %>% filter(origin == 'JFK' & month == 9) %>% summarise(mean_temp = mean(temp, na.rm = TRUE))
    # A tibble: 1 × 1
      mean_temp
          <dbl>
    1      66.9

№11. Самолеты какой авиакомпании совершили больше всего вылетов в июне?

    > flights %>% filter(month == 6) %>% group_by(carrier) %>% summarize(flights_count = n()) %>% arrange(desc(flights_count)) %>% head(1)
    # A tibble: 1 × 2
      carrier flights_count
      <chr>           <int>
    1 UA               4975

№12. Самолеты какой авиакомпании задерживались чаще других в 2013 году?

    > flights %>% filter(year == 2013 & dep_delay > 0) %>% group_by(carrier) %>% summarise(delays_count = n()) %>% arrange(desc(delays_count)) %>% head(1)
    # A tibble: 1 × 2
      carrier delays_count
      <chr>          <int>
    1 UA             27261

## Оценка результатов

В ходе выполнения практического задания были изучены функции пакета
`dplyr` и выполнены задания с использованием датафреймов из пакета
`nycflights13`.

## Вывод

Были получены базовые навыки обработки данных с помощью языка R и
встроенного пакета `dplyr`, а также получен навык работы с датафреймом
из пакета `nycflights13`.