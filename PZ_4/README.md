# Практическое задание №4
Журавлева Юлия БИСО-01-20

# Исследование метаданных DNS трафика

## Цель работы

1.  Закрепить практические навыки использования языка программирования R
    для обработки данных
2.  Закрепить знания основных функций обработки данных экосистемы
    `tidyverse` языка R
3.  Закрепить навыки исследования метаданных DNS трафика

## Исходные данные

1.  ОС Windows 10
2.  RStudio Desktop
3.  Интерпретатор языка R 4.2.2
4.  Пакет `dplyr`
5.  Файл `dns.log`
6.  Файл `header.csv`

## План

1.  Импортировать данные DNS
2.  Ответить на поставленные задачи.

## Ход выполнения работы

***Часть 1. Подготовка данных***

**1. Импортировать данные DNS**

Подключим библиотеки

``` r
library(dplyr)
```

    Warning: пакет 'dplyr' был собран под R версии 4.2.3


    Присоединяю пакет: 'dplyr'

    Следующие объекты скрыты от 'package:stats':

        filter, lag

    Следующие объекты скрыты от 'package:base':

        intersect, setdiff, setequal, union

``` r
library(readr)
```

    Warning: пакет 'readr' был собран под R версии 4.2.3

Для начала импортируем файл `header.csv` и занесём его в переменную
`headr`

``` r
headr <- read_delim("header.csv", delim = ",")
```

    Warning: One or more parsing issues, call `problems()` on your data frame for details,
    e.g.:
      dat <- vroom(...)
      problems(dat)

    Rows: 24 Columns: 3
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr (3): Field , Type, Description 

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

Импортируем данные DNS из файла `dns.log` и занесём его в переменную
`dns_l`

``` r
dns_l <- read_delim("dns.log", col_names = FALSE, delim = "\t")
```

    Rows: 427935 Columns: 23
    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: "\t"
    chr (13): X2, X3, X5, X7, X9, X10, X11, X12, X13, X14, X15, X21, X22
    dbl  (5): X1, X4, X6, X8, X20
    lgl  (5): X16, X17, X18, X19, X23

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

ℹ Use `spec()` to retrieve the full column specification for this data.
ℹ Specify the column types or set `show_col_types = FALSE` to quiet this
message.

**2. Добавить пропущенные данные о структуре данных (назначении
столбцов)**

``` r
colnames(dns_l) <- headr[,1] %>% unlist()
dns_l %>% glimpse()
```

    Rows: 427,935
    Columns: 23
    $ `ts `          <dbl> 1331901006, 1331901015, 1331901016, 1331901017, 1331901…
    $ `uid `         <chr> "CWGtK431H9XuaTN4fi", "C36a282Jljz7BsbGH", "C36a282Jljz…
    $ `id `          <chr> "192.168.202.100", "192.168.202.76", "192.168.202.76", …
    $ id.orig_h      <dbl> 45658, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137…
    $ id.orig_p      <chr> "192.168.27.203", "192.168.202.255", "192.168.202.255",…
    $ id.resp_h      <dbl> 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, …
    $ id.resp_p      <chr> "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp",…
    $ `proto `       <dbl> 33008, 57402, 57402, 57402, 57398, 57398, 57398, 62187,…
    $ `trans_id `    <chr> "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x…
    $ `query `       <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", …
    $ `qclass `      <chr> "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET",…
    $ `qclass_name ` <chr> "33", "32", "32", "32", "32", "32", "32", "32", "32", "…
    $ `qtype `       <chr> "SRV", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB", …
    $ `qtype_name `  <chr> "0", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ `rcode `       <chr> "NOERROR", "-", "-", "-", "-", "-", "-", "-", "-", "-",…
    $ `rcode_name `  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `QR `          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `AA `          <lgl> FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, …
    $ `TC RD `       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `RA `          <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1…
    $ `Z `           <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ `answers `     <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ `TTLs `        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…

Можем заметить, что в переменной `dns_l`, куда мы занесли данные DNS
появились названия столбцов из переменной `headr`. ![Caption for the
picture.](https://github.com/MoonFlower18/PZ_sem7/blob/main/PZ_4/screen1.png)

**3. Преобразовать данные в столбцах в нужный формат**

?????

**4. Просмотреть общую структуру данных с помощью функции `glimpse()`**

``` r
dns_l %>% glimpse()
```

    Rows: 427,935
    Columns: 23
    $ `ts `          <dbl> 1331901006, 1331901015, 1331901016, 1331901017, 1331901…
    $ `uid `         <chr> "CWGtK431H9XuaTN4fi", "C36a282Jljz7BsbGH", "C36a282Jljz…
    $ `id `          <chr> "192.168.202.100", "192.168.202.76", "192.168.202.76", …
    $ id.orig_h      <dbl> 45658, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137…
    $ id.orig_p      <chr> "192.168.27.203", "192.168.202.255", "192.168.202.255",…
    $ id.resp_h      <dbl> 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, …
    $ id.resp_p      <chr> "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp",…
    $ `proto `       <dbl> 33008, 57402, 57402, 57402, 57398, 57398, 57398, 62187,…
    $ `trans_id `    <chr> "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x…
    $ `query `       <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", …
    $ `qclass `      <chr> "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET",…
    $ `qclass_name ` <chr> "33", "32", "32", "32", "32", "32", "32", "32", "32", "…
    $ `qtype `       <chr> "SRV", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB", …
    $ `qtype_name `  <chr> "0", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ `rcode `       <chr> "NOERROR", "-", "-", "-", "-", "-", "-", "-", "-", "-",…
    $ `rcode_name `  <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `QR `          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `AA `          <lgl> FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, …
    $ `TC RD `       <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `RA `          <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1…
    $ `Z `           <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ `answers `     <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ `TTLs `        <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…

***Часть 2. Анализ***

**1. Сколько участников информационного обмена в сети Доброй
Организации?**

**2. Какое соотношение участников обмена внутри сети и участников
обращений к внешним ресурсам?**

**3. Найдите топ-10 участников сети, проявляющих наибольшую сетевую
активность**

**4. Найдите топ-10 доменов, к которым обращаются пользователи сети и
соответственное количество обращений**

**5. Опеределите базовые статистические характеристики (функция
summary() ) интервала времени между последовательным обращениями к
топ-10 доменам.**

**6. Часто вредоносное программное обеспечение использует DNS канал в
качестве канала управления, периодически отправляя запросы на
подконтрольный злоумышленникам DNS сервер. По периодическим запросам на
один и тот же домен можно выявить скрытый DNS канал. Есть ли такие IP
адреса в исследуемом датасете?**

## Оценка результатов

ааа

## Вывод

ааа
