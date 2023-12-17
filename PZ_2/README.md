# Практическое задание №2
Журавлева Юлия БИСО-01-20

# Основы обработки данных с помощью R с использованием пакета dplyr.

## Цель работы

1.  Развить практические навыки использования языка программирования R
    для обработки данных;
2.  Закрепить знания базовых типов данных языка R;
3.  Развить пркатические навыки использования функций обработки данных
    пакета dplyr – функции `select()`, `filter()`, `mutate()`,
    `arrange()`, `group_by()`.

## Исходные данные

1.  ОС Windows 10
2.  RStudio Desktop
3.  Интерпретатор языка R 4.2.2
4.  Пакет `dplyr`

## План

1.  Установить пакет `dplyr`
2.  Выполнить задания на наборе данных `starwars` и ответить на
    поставленные вопросы.

## Ход выполнения работы

Для начала, установим пакет `dplyr`. Его можно установить в RStudio с
помощью команды `install.packages("dplyr")`. После этого подключим пакет
к текущему проекту с помощью `library(dplyr)`.

    install.packages("dplyr")

``` r
library(dplyr)
```

    Warning: пакет 'dplyr' был собран под R версии 4.2.3


    Присоединяю пакет: 'dplyr'

    Следующие объекты скрыты от 'package:stats':

        filter, lag

    Следующие объекты скрыты от 'package:base':

        intersect, setdiff, setequal, union

### Вопросы:

#### №1. Сколько строк в датафрейме?

``` r
starwars %>% nrow()
```

    [1] 87

#### №2. Сколько столбцов в датафрейме?

``` r
starwars %>% ncol()
```

    [1] 14

#### №3. Как просмотреть примерный вид датафрейма?

``` r
starwars %>% glimpse()
```

    Rows: 87
    Columns: 14
    $ name       <chr> "Luke Skywalker", "C-3PO", "R2-D2", "Darth Vader", "Leia Or…
    $ height     <int> 172, 167, 96, 202, 150, 178, 165, 97, 183, 182, 188, 180, 2…
    $ mass       <dbl> 77.0, 75.0, 32.0, 136.0, 49.0, 120.0, 75.0, 32.0, 84.0, 77.…
    $ hair_color <chr> "blond", NA, NA, "none", "brown", "brown, grey", "brown", N…
    $ skin_color <chr> "fair", "gold", "white, blue", "white", "light", "light", "…
    $ eye_color  <chr> "blue", "yellow", "red", "yellow", "brown", "blue", "blue",…
    $ birth_year <dbl> 19.0, 112.0, 33.0, 41.9, 19.0, 52.0, 47.0, NA, 24.0, 57.0, …
    $ sex        <chr> "male", "none", "none", "male", "female", "male", "female",…
    $ gender     <chr> "masculine", "masculine", "masculine", "masculine", "femini…
    $ homeworld  <chr> "Tatooine", "Tatooine", "Naboo", "Tatooine", "Alderaan", "T…
    $ species    <chr> "Human", "Droid", "Droid", "Human", "Human", "Human", "Huma…
    $ films      <list> <"A New Hope", "The Empire Strikes Back", "Return of the J…
    $ vehicles   <list> <"Snowspeeder", "Imperial Speeder Bike">, <>, <>, <>, "Imp…
    $ starships  <list> <"X-wing", "Imperial shuttle">, <>, <>, "TIE Advanced x1",…

#### №4. Сколько уникальных рас персонажей (species) представлено в данных?

``` r
starwars %>% select(species) %>% 
  unique()
```

    # A tibble: 38 × 1
       species       
       <chr>         
     1 Human         
     2 Droid         
     3 Wookiee       
     4 Rodian        
     5 Hutt          
     6 <NA>          
     7 Yoda's species
     8 Trandoshan    
     9 Mon Calamari  
    10 Ewok          
    # ℹ 28 more rows

#### №5. Найти самого высокого персонажа.

``` r
starwars %>% filter(height == max(height, na.rm = TRUE)) %>% 
  select(name, height)
```

    # A tibble: 1 × 2
      name        height
      <chr>        <int>
    1 Yarael Poof    264

#### №6. Найти всех персонажей ниже 170

``` r
starwars %>% select(name, height) %>% 
  filter(height < 170)
```

    # A tibble: 22 × 2
       name                  height
       <chr>                  <int>
     1 C-3PO                    167
     2 R2-D2                     96
     3 Leia Organa              150
     4 Beru Whitesun Lars       165
     5 R5-D4                     97
     6 Yoda                      66
     7 Mon Mothma               150
     8 Wicket Systri Warrick     88
     9 Nien Nunb                160
    10 Watto                    137
    # ℹ 12 more rows

#### №7. Подсчитать ИМТ (индекс массы тела) для всех персонажей. ИМТ подсчитать по формуле 𝐼 = 𝑚/ℎ2 , где 𝑚 – масса (weight), а ℎ – рост (height).

``` r
starwars %>% mutate(BMI = mass / ((height / 100) ^ 2)) %>% select(name, BMI)
```

    # A tibble: 87 × 2
       name                 BMI
       <chr>              <dbl>
     1 Luke Skywalker      26.0
     2 C-3PO               26.9
     3 R2-D2               34.7
     4 Darth Vader         33.3
     5 Leia Organa         21.8
     6 Owen Lars           37.9
     7 Beru Whitesun Lars  27.5
     8 R5-D4               34.0
     9 Biggs Darklighter   25.1
    10 Obi-Wan Kenobi      23.2
    # ℹ 77 more rows

#### №8. Найти 10 самых “вытянутых” персонажей. “Вытянутость” оценить по отношению массы (mass) к росту (height) персонажей.

``` r
starwars %>% mutate(long_per = mass / height) %>% 
  arrange(desc(long_per)) %>% head(10) %>% 
  select(name, long_per)
```

    # A tibble: 10 × 2
       name                  long_per
       <chr>                    <dbl>
     1 Jabba Desilijic Tiure    7.76 
     2 Grievous                 0.736
     3 IG-88                    0.7  
     4 Owen Lars                0.674
     5 Darth Vader              0.673
     6 Jek Tono Porkins         0.611
     7 Bossk                    0.595
     8 Tarfful                  0.581
     9 Dexter Jettster          0.515
    10 Chewbacca                0.491

#### №9. Найти средний возраст персонажей каждой расы вселенной Звездных войн.

``` r
starwars %>% group_by(species) %>% 
  summarise(mean(birth_year, na.rm = TRUE))
```

    # A tibble: 38 × 2
       species   `mean(birth_year, na.rm = TRUE)`
       <chr>                                <dbl>
     1 Aleena                               NaN  
     2 Besalisk                             NaN  
     3 Cerean                                92  
     4 Chagrian                             NaN  
     5 Clawdite                             NaN  
     6 Droid                                 53.3
     7 Dug                                  NaN  
     8 Ewok                                   8  
     9 Geonosian                            NaN  
    10 Gungan                                52  
    # ℹ 28 more rows

#### №10. Найти самый распространенный цвет глаз персонажей вселенной Звездных войн.

``` r
starwars %>% count(eye_color) %>% 
  filter(n == max(n))
```

    # A tibble: 1 × 2
      eye_color     n
      <chr>     <int>
    1 brown        21

#### №11. Подсчитать среднюю длину имени в каждой расе вселенной Звездных войн.

``` r
starwars %>% group_by(species) %>% 
  summarise(sr_name_leng = mean(nchar(name)))
```

    # A tibble: 38 × 2
       species   sr_name_leng
       <chr>            <dbl>
     1 Aleena           12   
     2 Besalisk         15   
     3 Cerean           12   
     4 Chagrian         10   
     5 Clawdite         10   
     6 Droid             4.83
     7 Dug               7   
     8 Ewok             21   
     9 Geonosian        17   
    10 Gungan           11.7 
    # ℹ 28 more rows

## Оценка результатов

В ходе выполнения практического задания были изучены функции пакета
`dplyr`.

## Вывод

Были получены базовые навыки обработки данных с помощью языка R и
встроенного пакета `dplyr`.
