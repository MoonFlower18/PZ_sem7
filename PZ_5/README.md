# Практическое задание №5
Журавлева Юлия БИСО-01-20

# Исследование информации о состоянии беспроводных сетей

## Цель работы

1.  Получить знания о методах исследования радиоэлектронной обстановки.
2.  Составить представление о механизмах работы Wi-Fi сетей на канальном
    и сетевом уровне модели OSI.
3.  Закрепить практические навыки использования языка программирования
    для обработки данных
4.  Закрепить знания основных функций обработки данных экосистемы
    `tidyverse` языка R

## Исходные данные

1.  ОС Windows 10
2.  RStudio Desktop
3.  Интерпретатор языка R 4.2.2
4.  Файл `mir.csv-01.csv`

## План

1.  Импортировать данные из файла `mir.csv-01.csv` и привести в вид
    “аккуратных данных”.
2.  Провести анализ точек доступа.
3.  Провести анализ данных клиентов.
4.  Ответить на поставленные вопросы.

## Ход выполнения работы

### Этап 1. Подготовка данных

Для начала установим пакет `dplyr`.

``` r
library(dplyr)
```

    Warning: пакет 'dplyr' был собран под R версии 4.2.3


    Присоединяю пакет: 'dplyr'

    Следующие объекты скрыты от 'package:stats':

        filter, lag

    Следующие объекты скрыты от 'package:base':

        intersect, setdiff, setequal, union

Импортируем данные из общего файла файла в 2 разных датафрейма: анонсы
беспроводных точек доступа и запросы на подключение клиентов к известным
им точкам доступа. При выводе всего датафрейма заметим, что в первом
датафрейме всего 167 строк.

``` r
ds1 <- read.csv("mir.csv-01.csv", nrows = 167)
ds1 %>% head(10)
```

                   BSSID      First.time.seen       Last.time.seen channel Speed
    1  BE:F1:71:D5:17:8B  2023-07-28 09:13:03  2023-07-28 11:50:50       1   195
    2  6E:C7:EC:16:DA:1A  2023-07-28 09:13:03  2023-07-28 11:55:12       1   130
    3  9A:75:A8:B9:04:1E  2023-07-28 09:13:03  2023-07-28 11:53:31       1   360
    4  4A:EC:1E:DB:BF:95  2023-07-28 09:13:03  2023-07-28 11:04:01       7   360
    5  D2:6D:52:61:51:5D  2023-07-28 09:13:03  2023-07-28 10:30:19       6   130
    6  E8:28:C1:DC:B2:52  2023-07-28 09:13:03  2023-07-28 11:55:38       6   130
    7  BE:F1:71:D6:10:D7  2023-07-28 09:13:03  2023-07-28 11:50:44      11   195
    8  0A:C5:E1:DB:17:7B  2023-07-28 09:13:03  2023-07-28 11:36:31      11   130
    9  38:1A:52:0D:84:D7  2023-07-28 09:13:03  2023-07-28 10:25:02      11   130
    10 BE:F1:71:D5:0E:53  2023-07-28 09:13:03  2023-07-28 10:29:21       1   195
       Privacy Cipher Authentication Power X..beacons X..IV           LAN.IP
    1     WPA2   CCMP            PSK   -30        846   504    0.  0.  0.  0
    2     WPA2   CCMP            PSK   -30        750   116    0.  0.  0.  0
    3     WPA2   CCMP            PSK   -68        694    26    0.  0.  0.  0
    4     WPA2   CCMP            PSK   -37        510    21    0.  0.  0.  0
    5     WPA2   CCMP            PSK   -57        647     6    0.  0.  0.  0
    6      OPN                         -63        251  3430  172. 17.203.197
    7     WPA2   CCMP            PSK   -27       1647    80    0.  0.  0.  0
    8     WPA2   CCMP            PSK   -38       1251    11    0.  0.  0.  0
    9     WPA2   CCMP            PSK   -38        704     0    0.  0.  0.  0
    10    WPA2   CCMP            PSK   -66        617     0    0.  0.  0.  0
       ID.length                     ESSID Key
    1         12              C322U13 3965  NA
    2          4                      Cnet  NA
    3          2                        KC  NA
    4         14            POCO X5 Pro 5G  NA
    5         25                            NA
    6         13             MIREA_HOTSPOT  NA
    7         12              C322U21 0566  NA
    8         13             AndroidAP177B  NA
    9         24  EBFCD57F-EE81fI_DL_1AO2T  NA
    10        12              C322U06 9080  NA

Следующим импортируем второй датафрейм - запросы на подключение клиентов
к известным им точкам доступа. Заметим, что нужно пропустить несколько
строк, которые относятся к предыдущему датафрейму. При задавании номера
строки skip нужно смотреть на положение датафрейма. Нужная строка =\>
169.

``` r
ds2 <- read.csv("mir.csv-01.csv", skip = 169)
ds2 %>% head(10)
```

             Station.MAC      First.time.seen       Last.time.seen Power X..packets
    1  CA:66:3B:8F:56:DD  2023-07-28 09:13:03  2023-07-28 10:59:44   -33        858
    2  96:35:2D:3D:85:E6  2023-07-28 09:13:03  2023-07-28 09:13:03   -65          4
    3  5C:3A:45:9E:1A:7B  2023-07-28 09:13:03  2023-07-28 11:51:54   -39        432
    4  C0:E4:34:D8:E7:E5  2023-07-28 09:13:03  2023-07-28 11:53:16   -61        958
    5  5E:8E:A6:5E:34:81  2023-07-28 09:13:04  2023-07-28 09:13:04   -53          1
    6  10:51:07:CB:33:E7  2023-07-28 09:13:05  2023-07-28 11:56:06   -43        344
    7  68:54:5A:40:35:9E  2023-07-28 09:13:06  2023-07-28 11:50:50   -31        163
    8         Galaxy A71                                                           
    9  74:4C:A1:70:CE:F7  2023-07-28 09:13:06  2023-07-28 09:20:01   -71          3
    10 8A:A3:5A:33:76:57  2023-07-28 09:13:06  2023-07-28 10:20:27   -74        115
                    BSSID Probed.ESSIDs
    1   BE:F1:71:D5:17:8B  C322U13 3965
    2   (not associated)   IT2 Wireless
    3   BE:F1:71:D6:10:D7  C322U21 0566
    4   BE:F1:71:D5:17:8B  C322U13 3965
    5   (not associated)               
    6   (not associated)               
    7   1E:93:E3:1B:3C:F4  C322U06 5179
    8                                  
    9   E8:28:C1:DC:FF:F2              
    10  00:25:00:FF:94:73              

Приведём в вид “аккуратных данных” первый датафрейм - анонсы
беспроводных точек доступа.

Посмотрим на первоначальный вид первого датафрейма.

``` r
ds1 %>% glimpse() 
```

    Rows: 167
    Columns: 15
    $ BSSID           <chr> "BE:F1:71:D5:17:8B", "6E:C7:EC:16:DA:1A", "9A:75:A8:B9…
    $ First.time.seen <chr> " 2023-07-28 09:13:03", " 2023-07-28 09:13:03", " 2023…
    $ Last.time.seen  <chr> " 2023-07-28 11:50:50", " 2023-07-28 11:55:12", " 2023…
    $ channel         <int> 1, 1, 1, 7, 6, 6, 11, 11, 11, 1, 6, 14, 11, 11, 6, 6, …
    $ Speed           <int> 195, 130, 360, 360, 130, 130, 195, 130, 130, 195, 180,…
    $ Privacy         <chr> " WPA2", " WPA2", " WPA2", " WPA2", " WPA2", " OPN", "…
    $ Cipher          <chr> " CCMP", " CCMP", " CCMP", " CCMP", " CCMP", " ", " CC…
    $ Authentication  <chr> " PSK", " PSK", " PSK", " PSK", " PSK", "   ", " PSK",…
    $ Power           <int> -30, -30, -68, -37, -57, -63, -27, -38, -38, -66, -42,…
    $ X..beacons      <int> 846, 750, 694, 510, 647, 251, 1647, 1251, 704, 617, 13…
    $ X..IV           <int> 504, 116, 26, 21, 6, 3430, 80, 11, 0, 0, 86, 0, 0, 0, …
    $ LAN.IP          <chr> "   0.  0.  0.  0", "   0.  0.  0.  0", "   0.  0.  0.…
    $ ID.length       <int> 12, 4, 2, 14, 25, 13, 12, 13, 24, 12, 10, 0, 24, 24, 1…
    $ ESSID           <chr> " C322U13 3965", " Cnet", " KC", " POCO X5 Pro 5G", " …
    $ Key             <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…

Преобразуем тип данных `chr` в `dttm` для столбцов `First.time.seen` и
`First.time.seen`. Это нужно для корректного отображения даты и времени.
Затем посмотрим на изменённый датафрейм.

``` r
ds1$First.time.seen <- as.POSIXct(ds1$First.time.seen, 
                                  format = "%Y-%m-%d %H:%M:%S")
ds1$Last.time.seen <- as.POSIXct(ds1$Last.time.seen, 
                                 format = "%Y-%m-%d %H:%M:%S")
ds1 %>% glimpse() 
```

    Rows: 167
    Columns: 15
    $ BSSID           <chr> "BE:F1:71:D5:17:8B", "6E:C7:EC:16:DA:1A", "9A:75:A8:B9…
    $ First.time.seen <dttm> 2023-07-28 09:13:03, 2023-07-28 09:13:03, 2023-07-28 …
    $ Last.time.seen  <dttm> 2023-07-28 11:50:50, 2023-07-28 11:55:12, 2023-07-28 …
    $ channel         <int> 1, 1, 1, 7, 6, 6, 11, 11, 11, 1, 6, 14, 11, 11, 6, 6, …
    $ Speed           <int> 195, 130, 360, 360, 130, 130, 195, 130, 130, 195, 180,…
    $ Privacy         <chr> " WPA2", " WPA2", " WPA2", " WPA2", " WPA2", " OPN", "…
    $ Cipher          <chr> " CCMP", " CCMP", " CCMP", " CCMP", " CCMP", " ", " CC…
    $ Authentication  <chr> " PSK", " PSK", " PSK", " PSK", " PSK", "   ", " PSK",…
    $ Power           <int> -30, -30, -68, -37, -57, -63, -27, -38, -38, -66, -42,…
    $ X..beacons      <int> 846, 750, 694, 510, 647, 251, 1647, 1251, 704, 617, 13…
    $ X..IV           <int> 504, 116, 26, 21, 6, 3430, 80, 11, 0, 0, 86, 0, 0, 0, …
    $ LAN.IP          <chr> "   0.  0.  0.  0", "   0.  0.  0.  0", "   0.  0.  0.…
    $ ID.length       <int> 12, 4, 2, 14, 25, 13, 12, 13, 24, 12, 10, 0, 24, 24, 1…
    $ ESSID           <chr> " C322U13 3965", " Cnet", " KC", " POCO X5 Pro 5G", " …
    $ Key             <lgl> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…

Далее приведём в вид “аккуратных данных” второй датафрейм - запросы на
подключение клиентов к известным им точкам доступа.

Посмотрим на первоначальный вид второго датафрейм.

``` r
ds2 %>% glimpse() 
```

    Rows: 12,269
    Columns: 7
    $ Station.MAC     <chr> "CA:66:3B:8F:56:DD", "96:35:2D:3D:85:E6", "5C:3A:45:9E…
    $ First.time.seen <chr> " 2023-07-28 09:13:03", " 2023-07-28 09:13:03", " 2023…
    $ Last.time.seen  <chr> " 2023-07-28 10:59:44", " 2023-07-28 09:13:03", " 2023…
    $ Power           <chr> " -33", " -65", " -39", " -61", " -53", " -43", " -31"…
    $ X..packets      <chr> "      858", "        4", "      432", "      958", " …
    $ BSSID           <chr> " BE:F1:71:D5:17:8B", " (not associated) ", " BE:F1:71…
    $ Probed.ESSIDs   <chr> "C322U13 3965", "IT2 Wireless", "C322U21 0566", "C322U…

Преобразуем тип данных `chr` в `dttm` для столбцов `First.time.seen` и
`First.time.seen`. Это нужно для корректного отображения даты и времени.
Затем посмотрим на изменённый датафрейм.

``` r
ds2$First.time.seen <- as.POSIXct(ds2$First.time.seen, 
                                  format = "%Y-%m-%d %H:%M:%S")
ds2$Last.time.seen <- as.POSIXct(ds2$Last.time.seen, 
                                 format = "%Y-%m-%d %H:%M:%S")
ds2 %>% glimpse() 
```

    Rows: 12,269
    Columns: 7
    $ Station.MAC     <chr> "CA:66:3B:8F:56:DD", "96:35:2D:3D:85:E6", "5C:3A:45:9E…
    $ First.time.seen <dttm> 2023-07-28 09:13:03, 2023-07-28 09:13:03, 2023-07-28 …
    $ Last.time.seen  <dttm> 2023-07-28 10:59:44, 2023-07-28 09:13:03, 2023-07-28 …
    $ Power           <chr> " -33", " -65", " -39", " -61", " -53", " -43", " -31"…
    $ X..packets      <chr> "      858", "        4", "      432", "      958", " …
    $ BSSID           <chr> " BE:F1:71:D5:17:8B", " (not associated) ", " BE:F1:71…
    $ Probed.ESSIDs   <chr> "C322U13 3965", "IT2 Wireless", "C322U21 0566", "C322U…

Заметим, что в некоторых данных есть лишние пробелы, которые нужно
убрать, для читабельности датафреймов.

``` r
ds1 <- mutate_all(ds1, trimws) 
ds1 %>% glimpse() 
```

    Rows: 167
    Columns: 15
    $ BSSID           <chr> "BE:F1:71:D5:17:8B", "6E:C7:EC:16:DA:1A", "9A:75:A8:B9…
    $ First.time.seen <chr> "2023-07-28 09:13:03", "2023-07-28 09:13:03", "2023-07…
    $ Last.time.seen  <chr> "2023-07-28 11:50:50", "2023-07-28 11:55:12", "2023-07…
    $ channel         <chr> "1", "1", "1", "7", "6", "6", "11", "11", "11", "1", "…
    $ Speed           <chr> "195", "130", "360", "360", "130", "130", "195", "130"…
    $ Privacy         <chr> "WPA2", "WPA2", "WPA2", "WPA2", "WPA2", "OPN", "WPA2",…
    $ Cipher          <chr> "CCMP", "CCMP", "CCMP", "CCMP", "CCMP", "", "CCMP", "C…
    $ Authentication  <chr> "PSK", "PSK", "PSK", "PSK", "PSK", "", "PSK", "PSK", "…
    $ Power           <chr> "-30", "-30", "-68", "-37", "-57", "-63", "-27", "-38"…
    $ X..beacons      <chr> "846", "750", "694", "510", "647", "251", "1647", "125…
    $ X..IV           <chr> "504", "116", "26", "21", "6", "3430", "80", "11", "0"…
    $ LAN.IP          <chr> "0.  0.  0.  0", "0.  0.  0.  0", "0.  0.  0.  0", "0.…
    $ ID.length       <chr> "12", "4", "2", "14", "25", "13", "12", "13", "24", "1…
    $ ESSID           <chr> "C322U13 3965", "Cnet", "KC", "POCO X5 Pro 5G", "", "M…
    $ Key             <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, NA…

``` r
ds2 <- mutate_all(ds2, trimws) 
ds2 %>% glimpse() 
```

    Rows: 12,269
    Columns: 7
    $ Station.MAC     <chr> "CA:66:3B:8F:56:DD", "96:35:2D:3D:85:E6", "5C:3A:45:9E…
    $ First.time.seen <chr> "2023-07-28 09:13:03", "2023-07-28 09:13:03", "2023-07…
    $ Last.time.seen  <chr> "2023-07-28 10:59:44", "2023-07-28 09:13:03", "2023-07…
    $ Power           <chr> "-33", "-65", "-39", "-61", "-53", "-43", "-31", "", "…
    $ X..packets      <chr> "858", "4", "432", "958", "1", "344", "163", "", "3", …
    $ BSSID           <chr> "BE:F1:71:D5:17:8B", "(not associated)", "BE:F1:71:D6:…
    $ Probed.ESSIDs   <chr> "C322U13 3965", "IT2 Wireless", "C322U21 0566", "C322U…

### Этап 2. Анализ

#### Точки доступа

##### 1. Определим небезопасные точки доступа (без шифрования – OPN).

``` r
open_df <- ds1 %>% filter(Privacy == "OPN") %>%
  select(BSSID, Privacy, LAN.IP, ESSID)

open_df %>% arrange(desc(open_df))
```

                   BSSID Privacy          LAN.IP         ESSID
    1  E8:28:C1:DE:74:32     OPN 172. 17. 93.250 MIREA_HOTSPOT
    2  E8:28:C1:DE:74:31     OPN   0.  0.  0.  0              
    3  E8:28:C1:DE:74:30     OPN   0.  0.  0.  0              
    4  E8:28:C1:DE:47:D2     OPN   0.  0.  0.  0 MIREA_HOTSPOT
    5  E8:28:C1:DE:47:D1     OPN   0.  0.  0.  0              
    6  E8:28:C1:DE:47:D0     OPN   0.  0.  0.  0  MIREA_GUESTS
    7  E8:28:C1:DD:04:52     OPN 172. 17.216.149 MIREA_HOTSPOT
    8  E8:28:C1:DD:04:51     OPN   0.  0.  0.  0              
    9  E8:28:C1:DD:04:50     OPN 192.168.  0.  1  MIREA_GUESTS
    10 E8:28:C1:DD:04:42     OPN   0.  0.  0.  0              
    11 E8:28:C1:DD:04:41     OPN 172. 17. 84.175  MIREA_GUESTS
    12 E8:28:C1:DD:04:40     OPN 172. 17. 84.175 MIREA_HOTSPOT
    13 E8:28:C1:DC:FF:F2     OPN 172. 17. 84.217              
    14 E8:28:C1:DC:C8:32     OPN 192.168.155. 53 MIREA_HOTSPOT
    15 E8:28:C1:DC:C8:31     OPN   0.  0.  0.  0              
    16 E8:28:C1:DC:C8:30     OPN 172. 17.105.120  MIREA_GUESTS
    17 E8:28:C1:DC:C6:B2     OPN 192.168.  0.  1              
    18 E8:28:C1:DC:C6:B1     OPN   0.  0.  0.  0              
    19 E8:28:C1:DC:C6:B0     OPN   0.  0.  0.  0  MIREA_GUESTS
    20 E8:28:C1:DC:BD:52     OPN   0.  0.  0.  0 MIREA_HOTSPOT
    21 E8:28:C1:DC:BD:50     OPN   0.  0.  0.  0  MIREA_GUESTS
    22 E8:28:C1:DC:B2:52     OPN 172. 17.203.197 MIREA_HOTSPOT
    23 E8:28:C1:DC:B2:51     OPN   0.  0.  0.  0              
    24 E8:28:C1:DC:B2:50     OPN 172. 17. 95.169  MIREA_GUESTS
    25 E8:28:C1:DC:B2:42     OPN   0.  0.  0.  0              
    26 E8:28:C1:DC:B2:41     OPN 169.254.175.203  MIREA_GUESTS
    27 E8:28:C1:DC:B2:40     OPN 172. 17.203.197 MIREA_HOTSPOT
    28 E8:28:C1:DC:33:12     OPN   0.  0.  0.  0              
    29 E8:28:C1:DC:0B:B2     OPN   0.  0.  0.  0              
    30 E0:D9:E3:49:00:B1     OPN   0.  0.  0.  0              
    31 E0:D9:E3:48:FF:D2     OPN 192.168. 14.235              
    32 02:CF:8B:87:B4:F9     OPN   0.  0.  0.  0       MT_FREE
    33 02:BC:15:7E:D5:DC     OPN   0.  0.  0.  0       MT_FREE
    34 02:67:F1:B0:6C:98     OPN   0.  0.  0.  0       MT_FREE
    35 00:AB:0A:00:10:10     OPN   0.  0.  0.  0              
    36 00:53:7A:99:98:56     OPN   0.  0.  0.  0       MT_FREE
    37 00:3E:1A:5D:14:45     OPN   0.  0.  0.  0       MT_FREE
    38 00:26:99:F2:7A:EF     OPN   0.  0.  0.  0              
    39 00:26:99:F2:7A:E0     OPN   0.  0.  0.  0              
    40 00:25:00:FF:94:73     OPN   0.  0.  0.  0              
    41 00:03:7F:12:34:56     OPN   0.  0.  0.  0       MT_FREE
    42 00:03:7A:1A:03:56     OPN   0.  0.  0.  0       MT_FREE

##### 2. Определим производителя для каждого обнаруженного устройства. Для начала найдём уникальные устройства из списка обнаруженных.

``` r
mac_id <- sub("^([A-Fa-f0-9]{2}:[A-Fa-f0-9]{2}:[A-Fa-f0-9]{2}).*", "\\1", open_df$BSSID) %>% unique()

mac_id 
```

     [1] "E8:28:C1" "00:25:00" "E0:D9:E3" "00:26:99" "02:BC:15" "00:AB:0A"
     [7] "00:03:7A" "00:03:7F" "00:3E:1A" "02:67:F1" "02:CF:8B" "00:53:7A"

С помощью сервиса https://www.wireshark.org/tools/oui-lookup.html
определим производителя для каждого обнаруженного устройства. Некоторые
устройства не находятся с помощью данного сервиса, поэтому приведём
список тех, которые смогли найти:

-   00:03:7A Taiyo Yuden Co., Ltd.
-   00:03:7F Atheros Communications, Inc.
-   00:25:00 Apple, Inc.
-   00:26:99 Cisco Systems, Inc
-   E0:D9:E3 Eltex Enterprise Ltd.
-   E8:28:C1 Eltex Enterprise Ltd.

##### 3. Выявим устройства, использующие последнюю версию протокола шифрования WPA3, и названия точек доступа, реализованных на этих устройствах.

``` r
open_df <- ds1 %>% filter(Privacy == "WPA3 WPA2") %>%
  select(ESSID, Privacy, BSSID)

open_df
```

                   ESSID   Privacy             BSSID
    1                    WPA3 WPA2 26:20:53:0C:98:E8
    2         Christie’s WPA3 WPA2 A2:FE:FF:B8:9B:C9
    3                    WPA3 WPA2 96:FF:FC:91:EF:64
    4 iPhone (Анастасия) WPA3 WPA2 CE:48:E7:86:4E:33
    5 iPhone (Анастасия) WPA3 WPA2 8E:1F:94:96:DA:FD
    6            Димасик WPA3 WPA2 BE:FD:EF:18:92:44
    7  iPhone XS Max 🦊🐱🦊 WPA3 WPA2 3A:DA:00:F9:0C:02
    8                    WPA3 WPA2 76:C5:A0:70:08:96

##### 4. Отсортировать точки доступа по интервалу времени, в течение которого они находились на связи, по убыванию.

``` r
f_time <- as.POSIXct(ds1$First.time.seen, tz = "UTC")
l_time <- as.POSIXct(ds1$Last.time.seen, tz = "UTC")

diff_time <- difftime(l_time, f_time, units = "secs")
diff_time <- as.data.frame(diff_time)
diff_time
```

        diff_time
    1   9467 secs
    2   9729 secs
    3   9628 secs
    4   6658 secs
    5   4636 secs
    6   9755 secs
    7   9461 secs
    8   8608 secs
    9   4319 secs
    10  4578 secs
    11  9633 secs
    12  9524 secs
    13  4086 secs
    14  8661 secs
    15  9726 secs
    16  9725 secs
    17  9723 secs
    18  9724 secs
    19  9795 secs
    20  9707 secs
    21  9725 secs
    22  9492 secs
    23  9628 secs
    24  9776 secs
    25  4433 secs
    26  5190 secs
    27  9451 secs
    28  9555 secs
    29  9555 secs
    30  9710 secs
    31  9746 secs
    32  9595 secs
    33  8989 secs
    34  5643 secs
    35     0 secs
    36  1045 secs
    37  8445 secs
    38  9270 secs
    39   508 secs
    40  9348 secs
    41  4173 secs
    42  4572 secs
    43  5624 secs
    44  7483 secs
    45     0 secs
    46  8307 secs
    47  9212 secs
    48  6218 secs
    49  9305 secs
    50  9305 secs
    51  8693 secs
    52  9400 secs
    53  9400 secs
    54     0 secs
    55  1969 secs
    56  9041 secs
    57   220 secs
    58  9045 secs
    59  4331 secs
    60  8811 secs
    61     2 secs
    62  4997 secs
    63  8915 secs
    64     0 secs
    65     0 secs
    66     0 secs
    67  8390 secs
    68     0 secs
    69  7271 secs
    70  3265 secs
    71  8318 secs
    72     0 secs
    73  7199 secs
    74  3074 secs
    75     0 secs
    76     0 secs
    77  2733 secs
    78    13 secs
    79  5356 secs
    80     0 secs
    81  2082 secs
    82  1928 secs
    83     0 secs
    84  3879 secs
    85   295 secs
    86  6819 secs
    87   782 secs
    88   415 secs
    89     4 secs
    90  2743 secs
    91     0 secs
    92     0 secs
    93  4611 secs
    94     2 secs
    95     0 secs
    96     0 secs
    97     9 secs
    98     7 secs
    99  4252 secs
    100 4995 secs
    101    0 secs
    102    0 secs
    103 1224 secs
    104 1379 secs
    105    9 secs
    106    6 secs
    107  288 secs
    108    8 secs
    109 1312 secs
    110    0 secs
    111 4614 secs
    112    0 secs
    113  401 secs
    114    0 secs
    115  498 secs
    116 3008 secs
    117    2 secs
    118    7 secs
    119    0 secs
    120 4071 secs
    121    0 secs
    122    0 secs
    123   36 secs
    124    1 secs
    125  846 secs
    126  842 secs
    127    0 secs
    128    0 secs
    129 4577 secs
    130    5 secs
    131 2635 secs
    132    0 secs
    133 4392 secs
    134 4144 secs
    135 4224 secs
    136 4255 secs
    137 3451 secs
    138    0 secs
    139    0 secs
    140    0 secs
    141    0 secs
    142    0 secs
    143    0 secs
    144    0 secs
    145    0 secs
    146    0 secs
    147 2987 secs
    148 2688 secs
    149    0 secs
    150  651 secs
    151    2 secs
    152 2241 secs
    153   58 secs
    154   41 secs
    155 1300 secs
    156    0 secs
    157 1248 secs
    158  868 secs
    159  832 secs
    160    2 secs
    161    0 secs
    162    1 secs
    163    0 secs
    164    0 secs
    165    0 secs
    166   43 secs
    167    0 secs

``` r
sort_time <- ds1 %>% mutate(diff_time) %>%
  arrange(desc(diff_time)) %>%
  select(ESSID, BSSID, diff_time)

sort_time %>% head(10)
```

               ESSID             BSSID diff_time
    1                00:25:00:FF:94:73 9795 secs
    2  MIREA_HOTSPOT E8:28:C1:DD:04:52 9776 secs
    3  MIREA_HOTSPOT E8:28:C1:DC:B2:52 9755 secs
    4                08:3A:2F:56:35:FE 9746 secs
    5           Cnet 6E:C7:EC:16:DA:1A 9729 secs
    6   MIREA_GUESTS E8:28:C1:DC:B2:50 9726 secs
    7                E8:28:C1:DC:B2:51 9725 secs
    8                48:5B:39:F9:7A:48 9725 secs
    9                E8:28:C1:DC:FF:F2 9724 secs
    10      Vladimir 8E:55:4A:85:5B:01 9723 secs

##### 5. Обнаружить топ-10 самых быстрых точек доступа.

``` r
df1_speed <- ds1 %>% arrange(desc(Speed)) %>%
  select(ESSID, BSSID, Speed)

df1_speed %>% head(10)
```

                       ESSID             BSSID Speed
    1                        26:20:53:0C:98:E8   866
    2                        96:FF:FC:91:EF:64   866
    3     iPhone (Анастасия) CE:48:E7:86:4E:33   866
    4     iPhone (Анастасия) 8E:1F:94:96:DA:FD   866
    5                        1C:7E:E5:8E:B7:DE    65
    6               Vladimir 8E:55:4A:85:5B:01    65
    7                        9E:A3:A9:D6:28:3C    65
    8                        9E:A3:A9:DB:7E:01    65
    9                        9E:A3:A9:BF:12:C0    65
    10 Long Huong Galaxy M12 9A:9F:06:44:24:5B    65

##### 6. Отсортировать точки доступа по частоте отправки запросов (beacons) в единицу времени по их убыванию.

``` r
diff_time_num <- lapply(diff_time, as.numeric)
df_diff_time_num <- as.data.frame(diff_time_num)
df_beacons <- as.data.frame(as.numeric(ds1$X..beacons))

freq <- sapply(seq_along(df_beacons), function(i) df_beacons[[i]] / df_diff_time_num[[i]])
freq <- data.frame(freq)
freq <- round(freq, 3)

all_df <- ds1 %>% select(ESSID, BSSID, X..beacons)
all_df2 <- cbind(all_df, diff_time, freq)

sort_all_df2 <- arrange(all_df2, desc(freq)) %>% filter(!is.infinite(freq))

sort_all_df2
```

                           ESSID             BSSID X..beacons diff_time  freq
    1            iPhone (Uliana) F2:30:AB:E9:03:ED          6    7 secs 0.857
    2        Михаил's Galaxy M32 B2:CF:C0:00:4A:60          4    5 secs 0.800
    3          iPhone XS Max 🦊🐱🦊 3A:DA:00:F9:0C:02          5    9 secs 0.556
    4                    MT_FREE 02:BC:15:7E:D5:DC          1    2 secs 0.500
    5                    MT_FREE 00:3E:1A:5D:14:45          1    2 secs 0.500
    6                            76:C5:A0:70:08:96          1    2 secs 0.500
    7                       Саня D2:25:91:F6:6C:D8          5   13 secs 0.385
    8               C322U21 0566 BE:F1:71:D6:10:D7       1647 9461 secs 0.174
    9                    MT_FREE 00:03:7A:1A:03:56          1    6 secs 0.167
    10  EBFCD57F-EE81fI_DL_1AO2T 38:1A:52:0D:84:D7        704 4319 secs 0.163
    11             AndroidAP177B 0A:C5:E1:DB:17:7B       1251 8608 secs 0.145
    12                Galaxy A71 1E:93:E3:1B:3C:F4       1390 9633 secs 0.144
    13                           D2:6D:52:61:51:5D        647 4636 secs 0.140
    14              C322U06 9080 BE:F1:71:D5:0E:53        617 4578 secs 0.135
    15         iPhone (Искандер) 4A:86:77:04:B7:28        361 3008 secs 0.120
    16         iPhone (Искандер) 3A:70:96:C6:30:2C        145 1300 secs 0.112
    17                      витя 76:70:AF:A4:D2:AF        253 2733 secs 0.093
    18              C322U13 3965 BE:F1:71:D5:17:8B        846 9467 secs 0.089
    19          Redmi Note 8 Pro AA:F4:3F:EE:49:0B        738 9045 secs 0.082
    20                      Cnet 6E:C7:EC:16:DA:1A        750 9729 secs 0.077
    21            POCO X5 Pro 5G 4A:EC:1E:DB:BF:95        510 6658 secs 0.077
    22                OnePlus 6T 56:C5:2B:9F:84:90        317 4173 secs 0.076
    23                        KC 9A:75:A8:B9:04:1E        694 9628 secs 0.072
    24            Galaxy M012063 9C:A5:13:28:D5:89          3   43 secs 0.070
    25        GGWPRedmi Note 10S 36:46:53:81:12:A0         82 1248 secs 0.066
    26  EBFCD5C1-EE81fI_DN11AOcY 38:1A:52:0D:85:1D        130 2082 secs 0.062
    27  EBFCD6C2-EE81fI_DR21AOa0 38:1A:52:0D:8F:EC        107 2635 secs 0.041
    28          Redmi Note 8 Pro 2E:FE:13:D0:96:51          2   58 secs 0.034
    29        iPhone (Анастасия) CE:48:E7:86:4E:33          9  295 secs 0.031
    30                           E8:28:C1:DC:B2:51        279 9725 secs 0.029
    31        iPhone (Анастасия) 8E:1F:94:96:DA:FD         12  415 secs 0.029
    32              MIREA_GUESTS E8:28:C1:DC:B2:50        260 9726 secs 0.027
    33             MIREA_HOTSPOT E8:28:C1:DC:B2:52        251 9755 secs 0.026
    34                  Vladimir 8E:55:4A:85:5B:01        248 9723 secs 0.026
    35                 realme 10 5E:C7:C0:E4:D7:D4         85 3265 secs 0.026
    36  EBFCD615-EE81fI_DOL1AO8w 38:1A:52:0D:90:5D         90 4255 secs 0.021
    37                           1C:7E:E5:8E:B7:DE        142 9524 secs 0.015
    38  EBFCD597-EE81fI_DMN1AOe1 38:1A:52:0D:90:A1        112 8661 secs 0.013
    39                  Mi Phone A2:64:E8:97:58:EE         52 4252 secs 0.012
    40                      Damy 1E:C2:8E:D8:30:91          6  498 secs 0.012
    41                           48:5B:39:F9:7A:48        109 9725 secs 0.011
    42                      GIVC 00:26:99:F2:7A:E2         84 9707 secs 0.009
    43  EBFCD593-EE81fI_DMJ1AOI4 38:1A:52:0D:97:60         28 4086 secs 0.007
    44                       IKB 00:26:99:F2:7A:E1         65 9492 secs 0.007
    45                      GIVC 00:26:99:BA:75:80         61 9710 secs 0.006
    46              C239U51 0701 A6:02:B9:73:2F:76         26 4144 secs 0.006
    47                           9E:A3:A9:D6:28:3C         51 9451 secs 0.005
    48     Long Huong Galaxy M12 9A:9F:06:44:24:5B         22 4572 secs 0.005
    49                       IKB 00:23:EB:E3:81:FE         47 9305 secs 0.005
    50                      GIVC 00:23:EB:E3:81:FD         46 9305 secs 0.005
    51                           96:FF:FC:91:EF:64          9 1928 secs 0.005
    52                           0C:80:63:A9:6E:EE         42 9628 secs 0.004
    53                           9E:A3:A9:DB:7E:01         40 9555 secs 0.004
    54      DESKTOP-KITJO8R 5262 12:51:07:FF:29:D6         32 7483 secs 0.004
    55              C239U52 6706 A6:02:B9:73:83:18         17 4577 secs 0.004
    56                  Redmi 12 92:F5:7B:43:0B:69         18 4392 secs 0.004
    57              C239U53 6056 A6:02:B9:73:81:47         19 4224 secs 0.004
    58      DESKTOP-Q7R0KRV 2433 86:DF:BF:E4:2F:23         11 2688 secs 0.004
    59                           26:20:53:0C:98:E8          3 1045 secs 0.003
    60             MIREA_HOTSPOT E8:28:C1:DD:04:40         30 9400 secs 0.003
    61              MIREA_GUESTS E8:28:C1:DD:04:41         25 9400 secs 0.003
    62                           E8:28:C1:DD:04:42         23 8318 secs 0.003
    63              MIREA_GUESTS E8:28:C1:DD:04:50         20 8989 secs 0.002
    64                           E8:28:C1:DD:04:51          9 5643 secs 0.002
    65                       IKB 00:23:EB:E3:81:F1         19 9348 secs 0.002
    66              MIREA_GUESTS E8:28:C1:DC:BD:50          5 2743 secs 0.002
    67                   Redmi 9 B6:C4:55:B5:53:24          7 2987 secs 0.002
    68                   MT_FREE 02:67:F1:B0:6C:98          1  651 secs 0.002
    69             MIREA_HOTSPOT E8:28:C1:DC:C8:32         12 9555 secs 0.001
    70                      GIVC 00:23:EB:E3:81:F2          7 9595 secs 0.001
    71              MIREA_GUESTS E8:28:C1:DC:C8:30          7 8445 secs 0.001
    72                           9E:A3:A9:BF:12:C0          9 9270 secs 0.001
    73              MIREA_GUESTS E8:28:C1:DC:B2:41          5 8307 secs 0.001
    74             MIREA_HOTSPOT E8:28:C1:DC:B2:40          5 9212 secs 0.001
    75                           E8:28:C1:DC:B2:42          5 8693 secs 0.001
    76                       IKB 00:26:CB:AA:62:71          2 1969 secs 0.001
    77                           E8:28:C1:DC:C6:B1          5 8390 secs 0.001
    78                           E8:28:C1:DC:C8:31          8 7199 secs 0.001
    79              MIREA_GUESTS E8:28:C1:DC:C6:B0          4 3879 secs 0.001
    80                  vivo Y21 7E:3A:10:A7:59:4E          3 4611 secs 0.001
    81                           E8:28:C1:DC:FF:F2          0 9724 secs 0.000
    82                           00:25:00:FF:94:73          0 9795 secs 0.000
    83             MIREA_HOTSPOT E8:28:C1:DD:04:52          4 9776 secs 0.000
    84                           E8:28:C1:DE:74:31          2 4433 secs 0.000
    85             MIREA_HOTSPOT E8:28:C1:DE:74:32          1 5190 secs 0.000
    86                           08:3A:2F:56:35:FE          0 9746 secs 0.000
    87                           E8:28:C1:DE:74:30          0  508 secs 0.000
    88                           E0:D9:E3:48:FF:D2          0 5624 secs 0.000
    89                           00:26:99:F2:7A:E0          0 6218 secs 0.000
    90             MIREA_HOTSPOT E8:28:C1:DE:47:D2          3 9041 secs 0.000
    91             Redmi Note 9S 2A:E8:A2:02:01:73          0  220 secs 0.000
    92                           E8:28:C1:DC:3C:92          0 4331 secs 0.000
    93             tementy-phone 56:99:98:EE:5A:4E          1 8811 secs 0.000
    94            MGTS_GPON_B563 10:50:72:00:11:08          2 4997 secs 0.000
    95           Gnezdo_lounge 2 14:EB:B6:6A:76:37          0 8915 secs 0.000
    96                           CE:B3:FF:84:45:FC          0 7271 secs 0.000
    97                           E8:28:C1:DC:54:72          0 3074 secs 0.000
    98                           00:AB:0A:00:10:10          0 5356 secs 0.000
    99                           E8:28:C1:DC:C6:B2          0 6819 secs 0.000
    100                          E8:28:C1:DB:F5:F2          0  782 secs 0.000
    101                  Димасик BE:FD:EF:18:92:44          0    4 secs 0.000
    102                          00:23:EB:E3:49:31          0    2 secs 0.000
    103  DIRECT-08-HP M428fdw LJ EA:D8:D1:77:C8:08          1 4995 secs 0.000
    104             DIRECT-A6-HP CE:C3:F7:A4:7E:B3          0 1224 secs 0.000
    105                          E8:28:C1:DC:33:12          0 1379 secs 0.000
    106                          E8:28:C1:DB:FC:F2          0    9 secs 0.000
    107                          00:26:99:BA:75:8F          0  288 secs 0.000
    108           kak_vashi_dela DC:09:4C:32:34:9B          0    8 secs 0.000
    109                          E8:28:C1:DC:F0:90          0 1312 secs 0.000
    110                          E0:D9:E3:49:04:52          0 4614 secs 0.000
    111                          E0:D9:E3:49:04:50          0  401 secs 0.000
    112                          E0:D9:E3:49:04:40          0    7 secs 0.000
    113                IgorKotya 0A:24:D8:D9:24:70          2 4071 secs 0.000
    114                          E8:28:C1:DC:54:B0          0   36 secs 0.000
    115                          E0:D9:E3:49:00:B0          0    1 secs 0.000
    116                          E8:28:C1:DC:33:10          0  846 secs 0.000
    117             MIREA_GUESTS E8:28:C1:DB:F5:F0          0  842 secs 0.000
    118          Galaxy A30s5208 8A:A3:03:73:52:08          0 3451 secs 0.000
    119                 POCO C40 EA:7B:9B:D8:56:34          1 2241 secs 0.000
    120                          22:C9:7F:A9:BA:9C          0   41 secs 0.000
    121                          92:12:38:E5:7E:1E          0  868 secs 0.000
    122                          E8:28:C1:DC:0B:B0          0  832 secs 0.000
    123                          82:CD:7D:04:17:3B          0    2 secs 0.000
    124                          E8:28:C1:DC:54:B2          0    1 secs 0.000
    125        Mura's Galaxy A52 C6:BC:37:7A:67:0D          0    0 secs   NaN
    126                          12:48:F9:CF:58:8E          0    0 secs   NaN
    127                          E0:D9:E3:48:FF:D0          0    0 secs   NaN
    128                          E2:37:BF:8F:6A:7B          0    0 secs   NaN
    129              quiksmobile 8A:4E:75:44:5A:F6          0    0 secs   NaN
    130                          00:03:7A:1A:18:56          0    0 secs   NaN
    131                          00:09:9A:12:55:04          0    0 secs   NaN
    132                          E8:28:C1:DC:3A:B0          0    0 secs   NaN
    133                          E8:28:C1:DC:0B:B2          0    0 secs   NaN
    134                          E8:28:C1:DC:3C:80          0    0 secs   NaN
    135                          00:23:EB:E3:44:31          0    0 secs   NaN
    136                          A6:F7:05:31:E8:EE          0    0 secs   NaN
    137                   Amuler 12:54:1A:C6:FF:71          0    0 secs   NaN
    138                          E8:28:C1:DC:03:30          0    0 secs   NaN
    139                          B2:1B:0C:67:0A:BD          0    0 secs   NaN
    140                          E8:28:C1:DE:72:D0          0    0 secs   NaN
    141                          E0:D9:E3:49:04:41          0    0 secs   NaN
    142                      IKB 00:26:99:F1:1A:E1          0    0 secs   NaN
    143                          00:23:EB:E3:44:32          0    0 secs   NaN
    144                          E0:D9:E3:48:B4:D2          0    0 secs   NaN
    145                 Igorek✨ AE:3E:7F:C8:BC:8E          0    0 secs   NaN
    146                 HONOR 30 02:B3:45:5A:05:93          0    0 secs   NaN
    147                          00:00:00:00:00:00          0    0 secs   NaN
    148                          E8:28:C1:DC:3C:90          0    0 secs   NaN
    149                          30:B4:B8:11:C0:90          0    0 secs   NaN
    150                          00:26:99:F2:7A:EF          0    0 secs   NaN
    151                          E8:28:C1:DC:03:32          0    0 secs   NaN
    152                  MT_FREE 00:53:7A:99:98:56          0    0 secs   NaN
    153                          00:03:7F:10:17:56          0    0 secs   NaN
    154                          00:0D:97:6B:93:DF          0    0 secs   NaN

#### Данные клиентов

##### 1. Определить производителя для каждого обнаруженного устройства. Для начала найдём уникальные устройства из списка обнаруженных.

``` r
mac_id_2 <- ds2 %>% filter(grepl("(..:..:..)", BSSID)) %>% select(BSSID)
mac_id_3 <- unique(mac_id_2)
mac_id_3
```

                    BSSID
    1   BE:F1:71:D5:17:8B
    2   BE:F1:71:D6:10:D7
    4   1E:93:E3:1B:3C:F4
    5   E8:28:C1:DC:FF:F2
    6   00:25:00:FF:94:73
    7   00:26:99:F2:7A:E2
    8   0C:80:63:A9:6E:EE
    9   E8:28:C1:DD:04:52
    10  0A:C5:E1:DB:17:7B
    12  9A:75:A8:B9:04:1E
    13  8A:A3:03:73:52:08
    14  4A:EC:1E:DB:BF:95
    15  BE:F1:71:D5:0E:53
    16  08:3A:2F:56:35:FE
    17  6E:C7:EC:16:DA:1A
    21  2A:E8:A2:02:01:73
    22  E8:28:C1:DC:B2:52
    23  E8:28:C1:DC:C6:B2
    27  E8:28:C1:DC:C8:32
    28  56:C5:2B:9F:84:90
    30  9A:9F:06:44:24:5B
    31  12:48:F9:CF:58:8E
    33  E8:28:C1:DD:04:50
    35  AA:F4:3F:EE:49:0B
    37  3A:70:96:C6:30:2C
    39  E8:28:C1:DC:3C:92
    42  8E:55:4A:85:5B:01
    43  5E:C7:C0:E4:D7:D4
    44  E2:37:BF:8F:6A:7B
    48  96:FF:FC:91:EF:64
    50  CE:B3:FF:84:45:FC
    55  00:26:99:BA:75:80
    58  76:70:AF:A4:D2:AF
    59  E8:28:C1:DC:B2:50
    60  00:AB:0A:00:10:10
    61  E8:28:C1:DC:C8:30
    65  8E:1F:94:96:DA:FD
    69  E8:28:C1:DB:F5:F2
    75  E8:28:C1:DD:04:40
    77  EA:7B:9B:D8:56:34
    78  BE:FD:EF:18:92:44
    80  7E:3A:10:A7:59:4E
    81  00:26:99:F2:7A:E1
    82  00:23:EB:E3:49:31
    85  E8:28:C1:DC:B2:40
    86  E0:D9:E3:49:04:40
    87  3A:DA:00:F9:0C:02
    90  E8:28:C1:DC:B2:41
    91  E8:28:C1:DE:74:32
    96  E8:28:C1:DC:33:12
    99  92:F5:7B:43:0B:69
    102 DC:09:4C:32:34:9B
    103 E8:28:C1:DC:F0:90
    105 E0:D9:E3:49:04:52
    108 22:C9:7F:A9:BA:9C
    109 E8:28:C1:DD:04:41
    114 92:12:38:E5:7E:1E
    117 B2:1B:0C:67:0A:BD
    123 E8:28:C1:DC:33:10
    126 E0:D9:E3:49:04:41
    131 1E:C2:8E:D8:30:91
    133 A2:64:E8:97:58:EE
    135 A6:02:B9:73:2F:76
    137 A6:02:B9:73:81:47
    147 AE:3E:7F:C8:BC:8E
    155 B6:C4:55:B5:53:24
    158 86:DF:BF:E4:2F:23
    160 02:67:F1:B0:6C:98
    166 36:46:53:81:12:A0
    172 E8:28:C1:DC:0B:B0
    173 82:CD:7D:04:17:3B
    175 E8:28:C1:DC:54:B2
    179 00:03:7F:10:17:56
    180 00:0D:97:6B:93:DF

``` r
mac_id2 <- sub("^([A-Fa-f0-9]{2}:[A-Fa-f0-9]{2}:[A-Fa-f0-9]{2}).*", "\\1", mac_id_3$BSSID) %>% unique()
mac_id2 
```

     [1] "BE:F1:71" "1E:93:E3" "E8:28:C1" "00:25:00" "00:26:99" "0C:80:63"
     [7] "0A:C5:E1" "9A:75:A8" "8A:A3:03" "4A:EC:1E" "08:3A:2F" "6E:C7:EC"
    [13] "2A:E8:A2" "56:C5:2B" "9A:9F:06" "12:48:F9" "AA:F4:3F" "3A:70:96"
    [19] "8E:55:4A" "5E:C7:C0" "E2:37:BF" "96:FF:FC" "CE:B3:FF" "76:70:AF"
    [25] "00:AB:0A" "8E:1F:94" "EA:7B:9B" "BE:FD:EF" "7E:3A:10" "00:23:EB"
    [31] "E0:D9:E3" "3A:DA:00" "92:F5:7B" "DC:09:4C" "22:C9:7F" "92:12:38"
    [37] "B2:1B:0C" "1E:C2:8E" "A2:64:E8" "A6:02:B9" "AE:3E:7F" "B6:C4:55"
    [43] "86:DF:BF" "02:67:F1" "36:46:53" "82:CD:7D" "00:03:7F" "00:0D:97"

С помощью сервиса https://www.wireshark.org/tools/oui-lookup.html
определим производителя для каждого обнаруженного устройства. Некоторые
устройства не находятся с помощью данного сервиса, поэтому приведём
список тех, которые смогли найти:

-   00:03:7F Atheros Communications, Inc.
-   00:0D:97 Hitachi Energy USA Inc.
-   00:23:EB Cisco Systems, Inc
-   00:25:00 Apple, Inc.
-   00:26:99 Cisco Systems, Inc
-   08:3A:2F Guangzhou Juan Intelligent Tech Joint Stock Co.,Ltd
-   0C:80:63 Tp-Link Technologies Co.,Ltd.
-   DC:09:4C Huawei Technologies Co.,Ltd
-   E0:D9:E3 Eltex Enterprise Ltd.
-   E8:28:C1 Eltex Enterprise Ltd.

##### 2. Обнаружить устройства, которые НЕ рандомизируют свой MAC адрес. Устройства, которые не рандомизируют свой MAC-адрес, будут иметь постоянный BSSID.

``` r
no_rand <- ds2 %>%
  filter(!grepl("\\(not associated\\)", BSSID) & !Station.MAC %in% grep(":", ds2$Station.MAC, value = TRUE)) %>%
  distinct(Station.MAC, .keep_all = TRUE)

no_rand2 <- as.data.frame(no_rand$Station.MAC)
names(no_rand2) <- "Devices"
no_rand2
```

                                                                                Devices
    1                                                                        Galaxy A71
    2                                                                   Galaxy A30s5208
    3                                                                      C322U06 9080
    4                                                                             Kesha
    5                                                                               Дом
    6                                                                     MIREA_HOTSPOT
    7                                                                               M26
    8                                                                           kmkdz_g
    9                                                                  Moscow_WiFi_Free
    10                                                         AAAAADVpTWoADwFlRedmi 4X
    11                                                                    helvetia-free
    12                                                                     MIREA_GUESTS
    13                                                                RT-5GHz_WiFi_5756
    14                                                                     vestis.local
    15                                                                           -D-13-
    16                                                                     MGTS_5makmak
    17                                                                     TP-Link_3144
    18                                                                               Шк
    19                                                                AndroidShare_8397
    20 \\xA7\\xDF\\xA7\\xD1\\xA7\\xE3\\xA7\\xE4\\xA7\\xD6\\xA7\\xE9\\xA7\\xDC\\xA7\\xD1
    21                                                                AndroidShare_8795
    22                                                                      home 466_5G
    23                                                                          MT_FREE
    24                                                                         Redmi 12
    25                                                                AndroidShare_2335
    26                                                                AndroidShare_2061
    27                                                                        KHRISTAKI
    28                                                              MTSRouter_5G_142878
    29                                                                AndroidShare_1901
    30                                                                       拯救者 Y70
    31                                                                AndroidShare_1576
    32                                                                       Beeline121
    33                                                                          edeeeèe
    34                                                                               it
    35                                                                    Snickers_ASSA
    36                                                                           podval
    37                                                                         Hornet24
    38                                                                                1
    39                                                                        CPPK_Free
    40                                                                     SevenSky2.4G
    41                                                                      Timo Resort
    42                                                                 MTS_GPON5_ac0968
    43                                                         BgAAAFytPg4AHwF7Redmi 4A
    44                                                             \\xAC\\xBA\\xAC\\xDC
    45                                                                             WiFi
    46                                                                       lenovo_pad
    47                                                                              RST
    48                                                                Beeline_5G_F2F425

##### 3. Кластеризовать запросы от устройств к точкам доступа по их именам. Определить время появления устройства в зоне радиовидимости и время выхода его из нее.

``` r
clust_df <- ds2 %>%
  filter(!is.na(Probed.ESSIDs), !is.na(Power), !is.na(First.time.seen), !is.na(Last.time.seen)) %>% 
  group_by(Station.MAC, Probed.ESSIDs) %>% summarise("first_log" = min(First.time.seen), 
                                                     "last_log" = max(Last.time.seen),
                                                     
  Power = ifelse(all(is.numeric(as.numeric(Power))), sum(as.numeric(Power)), NA)) %>% arrange(first_log)
```

    `summarise()` has grouped output by 'Station.MAC'. You can override using the
    `.groups` argument.

``` r
clust_df %>% head(10)
```

    # A tibble: 10 × 5
    # Groups:   Station.MAC [10]
       Station.MAC       Probed.ESSIDs  first_log           last_log           Power
       <chr>             <chr>          <chr>               <chr>              <dbl>
     1 5C:3A:45:9E:1A:7B "C322U21 0566" 2023-07-28 09:13:03 2023-07-28 11:51:…   -39
     2 96:35:2D:3D:85:E6 "IT2 Wireless" 2023-07-28 09:13:03 2023-07-28 09:13:…   -65
     3 C0:E4:34:D8:E7:E5 "C322U13 3965" 2023-07-28 09:13:03 2023-07-28 11:53:…   -61
     4 CA:66:3B:8F:56:DD "C322U13 3965" 2023-07-28 09:13:03 2023-07-28 10:59:…   -33
     5 5E:8E:A6:5E:34:81 ""             2023-07-28 09:13:04 2023-07-28 09:13:…   -53
     6 10:51:07:CB:33:E7 ""             2023-07-28 09:13:05 2023-07-28 11:56:…   -43
     7 68:54:5A:40:35:9E "C322U06 5179" 2023-07-28 09:13:06 2023-07-28 11:50:…   -31
     8 74:4C:A1:70:CE:F7 ""             2023-07-28 09:13:06 2023-07-28 09:20:…   -71
     9 8A:A3:5A:33:76:57 ""             2023-07-28 09:13:06 2023-07-28 10:20:…   -74
    10 CA:54:C4:8B:B5:3A "GIVC"         2023-07-28 09:13:06 2023-07-28 11:55:…   -65

##### 4. Оценить стабильность уровня сигнала внутри кластера во времени. Выявить наиболее стабильный кластер

``` r
s_clust <- clust_df %>% group_by(Station.MAC, Probed.ESSIDs) %>%
  summarise(Power = mean(Power)) %>% arrange((Power))
```

    `summarise()` has grouped output by 'Station.MAC'. You can override using the
    `.groups` argument.

``` r
s_clust %>% head(1)
```

    # A tibble: 1 × 3
    # Groups:   Station.MAC [1]
      Station.MAC       Probed.ESSIDs  Power
      <chr>             <chr>          <dbl>
    1 8A:45:77:F9:7F:F4 iPhone (Дима )   -89

## Оценка результатов

В ходе практической работы были импортированы, подготовлены и
проанализированы данные трафика Wi-Fi сетей, а также даны ответы на
поставленные задачи.

## Вывод

Были развиты практические навыки использования языка программирования R
для обработки данных с помощью пакета `dplyr`.
