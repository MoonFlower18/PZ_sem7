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

### Часть 1. Подготовка данных

#### 1. Импортировать данные DNS

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

``` r
library(jsonlite)
```

    Warning: пакет 'jsonlite' был собран под R версии 4.2.3

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

#### 2-3. Добавить пропущенные данные о структуре данных (назначении столбцов). Преобразовать данные в столбцах в нужный формат.

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

#### 4. Просмотреть общую структуру данных с помощью функции `glimpse()`

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

### Часть 2. Анализ

#### 1. Сколько участников информационного обмена в сети Доброй Организации?

``` r
uni_id <- unique(dns_l$`id.orig_h`)
uni_proto <- unique(dns_l$`id.resp_h`)
uni_user <- union(uni_id, uni_proto)
uni_user %>% length()
```

    [1] 1359

#### 2. Какое соотношение участников обмена внутри сети и участников обращений к внешним ресурсам?

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

#### 3. Найдите топ-10 участников сети, проявляющих наибольшую сетевую активность

``` r
users_top_10 <- dns_l %>% 
  group_by(ip = id.orig_h) %>% 
  summarise(num_activ = n()) %>% 
  arrange(desc(num_activ))

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

#### 4. Найдите топ-10 доменов, к которым обращаются пользователи сети и соответственное количество обращений

Для начала, создадим датафрейм, который будет включать в себя только
нужную информацию без мусора (чтобы `qtype_name` включал в себя “А” и
“АААА”). Убедимся, что в начале и в конце есть оба значения.

``` r
filtered_df <- subset(dns_l, dns_l$`qtype_name ` == "A")
filtered_df_2 <- subset(dns_l, dns_l$`qtype_name ` == "AAAA")
merged_df <- union(filtered_df_2, filtered_df)

merged_df %>% head(10)
```

    # A tibble: 10 × 23
           `ts ` `uid ` id.orig_h id.orig_p id.resp_h id.resp_p `proto ` `trans_id `
           <dbl> <chr>  <chr>         <dbl> <chr>         <dbl> <chr>          <dbl>
     1    1.33e9 Cgrcu… 192.168.…     60821 172.19.1…        53 udp             3550
     2    1.33e9 Cgrcu… 192.168.…     60821 172.19.1…        53 udp             3550
     3    1.33e9 Cgrcu… 192.168.…     60821 172.19.1…        53 udp            35599
     4    1.33e9 Cgrcu… 192.168.…     60821 172.19.1…        53 udp            35599
     5    1.33e9 CnPAI… 192.168.…     61591 172.19.1…        53 udp            33869
     6    1.33e9 CnPAI… 192.168.…     61591 172.19.1…        53 udp            33869
     7    1.33e9 CnPAI… 192.168.…     61591 172.19.1…        53 udp            31088
     8    1.33e9 CnPAI… 192.168.…     61591 172.19.1…        53 udp            31088
     9    1.33e9 CiZuY… 192.168.…     56191 192.168.…        53 udp            62911
    10    1.33e9 CJjH3… 192.168.…     60440 192.168.…        53 udp            60105
    # ℹ 15 more variables: `query ` <chr>, `qclass ` <chr>, `qclass_name ` <chr>,
    #   `qtype ` <chr>, `qtype_name ` <chr>, `rcode ` <chr>, `rcode_name ` <chr>,
    #   `QR ` <lgl>, `AA ` <lgl>, `TC RD ` <lgl>, `RA ` <lgl>, `Z ` <dbl>,
    #   `answers ` <chr>, `TTLs ` <chr>, `rejected ` <lgl>

``` r
merged_df %>% tail(10)
```

    # A tibble: 10 × 23
           `ts ` `uid ` id.orig_h id.orig_p id.resp_h id.resp_p `proto ` `trans_id `
           <dbl> <chr>  <chr>         <dbl> <chr>         <dbl> <chr>          <dbl>
     1    1.33e9 CxuQj… 192.168.…     57419 192.168.…        53 udp            12849
     2    1.33e9 CBcEz… 192.168.…     58581 192.168.…        53 udp            12430
     3    1.33e9 CogUQ… 192.168.…     48795 192.168.…        53 udp            30362
     4    1.33e9 Csl3I… 192.168.…     51576 192.168.…        53 udp            34814
     5    1.33e9 CYT03… 192.168.…     37153 192.168.…        53 udp            11770
     6    1.33e9 CuhnV… 192.168.…     50914 192.168.…        53 udp            65099
     7    1.33e9 CnIiL… 192.168.…     35790 192.168.…        53 udp            20085
     8    1.33e9 Cra6d… 192.168.…     48782 192.168.…        53 udp            62110
     9    1.33e9 Cxkot… 192.168.…     46721 192.168.…        53 udp            54061
    10    1.33e9 C6j3k… 192.168.…     52646 192.168.…        53 udp            57534
    # ℹ 15 more variables: `query ` <chr>, `qclass ` <chr>, `qclass_name ` <chr>,
    #   `qtype ` <chr>, `qtype_name ` <chr>, `rcode ` <chr>, `rcode_name ` <chr>,
    #   `QR ` <lgl>, `AA ` <lgl>, `TC RD ` <lgl>, `RA ` <lgl>, `Z ` <dbl>,
    #   `answers ` <chr>, `TTLs ` <chr>, `rejected ` <lgl>

Выводим ТОП-10 доменов.

``` r
domains_top_10 <- merged_df %>% 
  group_by(address = merged_df$`query `) %>% 
  summarise(req_count = n()) %>% 
  arrange(desc(req_count))

domains_top_10 %>% 
  .[-10,] %>% 
  head(10)
```

    # A tibble: 10 × 2
       address                         req_count
       <chr>                               <int>
     1 teredo.ipv6.microsoft.com           17515
     2 www.apple.com                       13155
     3 safebrowsing.clients.google.com      9653
     4 tools.google.com                     6733
     5 imap.gmail.com                       3786
     6 api.twitter.com                      3547
     7 api.facebook.com                     3362
     8 creativecommons.org                  3329
     9 www.dokuwiki.org                     3146
    10 www.php.net                          3122

#### 5. Опеределите базовые статистические характеристики (функция `summary()`) интервала времени между последовательным обращениями к топ-10 доменам.

``` r
domains_top_10_new <- dns_l %>% 
  filter(tolower(dns_l$`query `) %in% domains_top_10$address) %>% 
  arrange(`ts `)

timing <- diff(dns_l$`ts `)

summary(timing)
```

        Min.  1st Qu.   Median     Mean  3rd Qu.     Max. 
    -1502.62     0.00     0.25     0.27     3.01 49677.59 

#### 6. Часто вредоносное программное обеспечение использует DNS канал в качестве канала управления, периодически отправляя запросы на подконтрольный злоумышленникам DNS сервер. По периодическим запросам на один и тот же домен можно выявить скрытый DNS канал. Есть ли такие IP адреса в исследуемом датасете?

``` r
evil_ip <- dns_l %>% 
  group_by(ip = tolower(dns_l$`id.orig_h`), domain = tolower(dns_l$`query `)) %>% 
  summarise(req_count = n()) %>% 
  filter(req_count > 1)
```

    `summarise()` has grouped output by 'ip'. You can override using the `.groups`
    argument.

``` r
uni_evil <- unique(evil_ip$`ip`)

uni_evil %>% length() 
```

    [1] 240

``` r
uni_evil %>% head(10)
```

     [1] "10.10.10.10"     "10.10.117.209"   "10.10.117.210"   "128.244.37.196" 
     [5] "169.254.109.123" "169.254.228.26"  "172.16.42.42"    "192.168.0.199"  
     [9] "192.168.0.3"     "192.168.100.130"

### Часть 3. Обогащение данных

#### 1. Определите местоположение (страну, город) и организацию-провайдера для топ-10 доменов. Для этого можно использовать сторонние сервисы. С нашем случае используем `https://ip-api.com/`.\*\*

``` r
top_address <- domains_top_10 %>% 
  .[-10,] %>% 
  head(10)

top_address
```

    # A tibble: 10 × 2
       address                         req_count
       <chr>                               <int>
     1 teredo.ipv6.microsoft.com           17515
     2 www.apple.com                       13155
     3 safebrowsing.clients.google.com      9653
     4 tools.google.com                     6733
     5 imap.gmail.com                       3786
     6 api.twitter.com                      3547
     7 api.facebook.com                     3362
     8 creativecommons.org                  3329
     9 www.dokuwiki.org                     3146
    10 www.php.net                          3122

Выведем данные из json файла и выведем его в датафрейме.

``` r
geo_ip <- fromJSON("top_10.json")
geo_ip_df <- as.data.frame(geo_ip)
geo_ip_df
```

                             host_name                  query        country
    1        teredo.ipv6.microsoft.com                     NA             NA
    2                    www.apple.com 2a02:26f0:b7:4af::1aca United Kingdom
    3  safebrowsing.clients.google.com        172.217.168.238  United States
    4                 tools.google.com         172.217.16.238          Spain
    5                   imap.gmail.com        142.250.102.109  United States
    6                  api.twitter.com           104.244.42.2  United States
    7                 api.facebook.com          163.70.151.23 United Kingdom
    8              creativecommons.org          172.67.34.140         Canada
    9                 www.dokuwiki.org        138.201.137.132        Germany
    10                     www.php.net            185.85.0.29        Germany
       regionName          city                            org
    1          NA            NA                             NA
    2     England        London            Akamai Technologies
    3  New Jersey       Trenton                     Google LLC
    4      Madrid        Madrid                     Google LLC
    5  California Mountain View                     Google LLC
    6  California San Francisco                    Twitter Inc
    7     England        London Meta Platforms Ireland Limited
    8     Ontario       Toronto               Cloudflare, Inc.
    9    Saarland   St. Ingbert                        Hetzner
    10    Bavaria        Munich             Myra Security GmbH

## Оценка результатов

В результате практической работы были получены ответы на все
поставленные вопросы с помощью языка R и библиотеки `dplyr`.
Дополнительно были изучены возможности редактирования датафрейма, а
также посмотрения датафрейма из json файла.

## Вывод

В ходе выполнения практической работы были подготовлены,
проанализированы и обогащены данные DNS трафика с помощью стороннего
ресурса `https://ip-api.com/` и оформлеты в виде датафрейма.
