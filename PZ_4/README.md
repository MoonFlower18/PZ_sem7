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

    Rows: 23 Columns: 3
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
    $ id.orig_h      <chr> "192.168.202.100", "192.168.202.76", "192.168.202.76", …
    $ id.orig_p      <dbl> 45658, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137…
    $ id.resp_h      <chr> "192.168.27.203", "192.168.202.255", "192.168.202.255",…
    $ id.resp_p      <dbl> 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, …
    $ `proto `       <chr> "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp",…
    $ `trans_id `    <dbl> 33008, 57402, 57402, 57402, 57398, 57398, 57398, 62187,…
    $ `query `       <chr> "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x…
    $ `qclass `      <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", …
    $ `qclass_name ` <chr> "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET",…
    $ `qtype `       <chr> "33", "32", "32", "32", "32", "32", "32", "32", "32", "…
    $ `qtype_name `  <chr> "SRV", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB", …
    $ `rcode `       <chr> "0", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ `rcode_name `  <chr> "NOERROR", "-", "-", "-", "-", "-", "-", "-", "-", "-",…
    $ `QR `          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `AA `          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `TC RD `       <lgl> FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, …
    $ `RA `          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `Z `           <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1…
    $ `answers `     <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ `TTLs `        <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ `rejected `    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…

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
    $ id.orig_h      <chr> "192.168.202.100", "192.168.202.76", "192.168.202.76", …
    $ id.orig_p      <dbl> 45658, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137…
    $ id.resp_h      <chr> "192.168.27.203", "192.168.202.255", "192.168.202.255",…
    $ id.resp_p      <dbl> 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, 137, …
    $ `proto `       <chr> "udp", "udp", "udp", "udp", "udp", "udp", "udp", "udp",…
    $ `trans_id `    <dbl> 33008, 57402, 57402, 57402, 57398, 57398, 57398, 62187,…
    $ `query `       <chr> "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x…
    $ `qclass `      <chr> "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", "1", …
    $ `qclass_name ` <chr> "C_INTERNET", "C_INTERNET", "C_INTERNET", "C_INTERNET",…
    $ `qtype `       <chr> "33", "32", "32", "32", "32", "32", "32", "32", "32", "…
    $ `qtype_name `  <chr> "SRV", "NB", "NB", "NB", "NB", "NB", "NB", "NB", "NB", …
    $ `rcode `       <chr> "0", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ `rcode_name `  <chr> "NOERROR", "-", "-", "-", "-", "-", "-", "-", "-", "-",…
    $ `QR `          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `AA `          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `TC RD `       <lgl> FALSE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, TRUE, …
    $ `RA `          <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…
    $ `Z `           <dbl> 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 0, 0, 0, 0, 1, 1, 1, 1…
    $ `answers `     <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ `TTLs `        <chr> "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", "-", …
    $ `rejected `    <lgl> FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE, FALSE,…

***Часть 2. Анализ***

**1. Сколько участников информационного обмена в сети Доброй
Организации?**

``` r
uni_id <- unique(dns_l$`id.orig_h`)
uni_proto <- unique(dns_l$`id.resp_h`)
uni_user <- union(uni_id , uni_proto)
uni_user %>% length()
```

    [1] 1359

**2. Какое соотношение участников обмена внутри сети и участников
обращений к внешним ресурсам?**

Диапазоны ip-адресов:

1.  10.0.0.0 - 10.255.255.255
2.  100.64.0.0 - 100.127.255.255
3.  172.16.0.0 - 172.31.255.255
4.  192.168.0.0 - 192.168.255.255

``` r
ip_all <- c("10.", "100.([6-9]|1[0-1][0-9]|12[0-7]).", "172.((1[6-9])|(2[0-9])|(3[0-1])).", "192.168.")
internal_ips <- uni_user[grep(paste(ip_all, collapse = "|"), uni_user)]
sum_int <- sum(uni_user %in% internal_ips)
count_ext <- length(uni_user) - sum_int

rate <- sum_int / count_ext
rate
```

    [1] 15.57317

**3. Найдите топ-10 участников сети, проявляющих наибольшую сетевую
активность**

``` r
users_top_10 <- dns_l %>% group_by(ip = id.orig_h) %>% summarise(num_activ = n()) %>% arrange(desc(num_activ))

users_top_10 %>% head(10)
```

    # A tibble: 10 × 2
       ip              num_activ
       <chr>               <int>
     1 10.10.117.210       75943
     2 192.168.202.93      26522
     3 192.168.202.103     18121
     4 192.168.202.76      16978
     5 192.168.202.97      16176
     6 192.168.202.141     14967
     7 10.10.117.209       14222
     8 192.168.202.110     13372
     9 192.168.203.63      12148
    10 192.168.202.106     10784

**4. Найдите топ-10 доменов, к которым обращаются пользователи сети и
соответственное количество обращений**

``` r
domains_top_10 <- dns_l %>% group_by(dns_l$`query `) %>% summarise(req_count = n()) %>% arrange(desc(req_count))

domains_top_10 %>% head(10)
```

    # A tibble: 10 × 2
       `dns_l$\`query \``                                                  req_count
       <chr>                                                                   <int>
     1 "teredo.ipv6.microsoft.com"                                             39273
     2 "tools.google.com"                                                      14057
     3 "www.apple.com"                                                         13390
     4 "time.apple.com"                                                        13109
     5 "safebrowsing.clients.google.com"                                       11658
     6 "*\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x00\\x0…     10401
     7 "WPAD"                                                                   9134
     8 "44.206.168.192.in-addr.arpa"                                            7248
     9 "HPE8AA67"                                                               6929
    10 "ISATAP"                                                                 6569

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
