Практическое задание №2
================
Журавлева Юлия БИСО-01-20

# Основы обработки данных с помощью R с использованием пакета dplyr.

## Цель работы

1.  Развить практические навыки использования языка программирования R
    для обработки данных;
2.  Закрепить знания базовых типов данных языка R;
3.  Развить пркатические навыки использования функций обработки данных
    пакета dplyr – функции `select()`, `filter()`, `mutate()`,
    `arrange()`, `group_by()`.

## Ход выполнения работы

№1. Сколько строк в датафрейме?

    > starwars %>% nrow()

    [1] 87

№2. Сколько столбцов в датафрейме?

    > starwars %>% ncol()

    [1] 14

№3. Как просмотреть примерный вид датафрейма?

    > starwars %>% glimpse()

    Rows: 87
    Columns: 14
    $ name       <chr> "Luke Skywalker", "C-3PO", "R2-D2", "Darth Vader", "Leia Organa", "Owen Lars", "Beru Whitesun lars", "R5-D4", "Biggs Darklighter", "Obi-Wan Kenobi", "Anakin S…
    $ height     <int> 172, 167, 96, 202, 150, 178, 165, 97, 183, 182, 188, 180, 228, 180, 173, 175, 170, 180, 66, 170, 183, 200, 190, 177, 175, 180, 150, NA, 88, 160, 193, 191, 170…
    $ mass       <dbl> 77.0, 75.0, 32.0, 136.0, 49.0, 120.0, 75.0, 32.0, 84.0, 77.0, 84.0, NA, 112.0, 80.0, 74.0, 1358.0, 77.0, 110.0, 17.0, 75.0, 78.2, 140.0, 113.0, 79.0, 79.0, 83…
    $ hair_color <chr> "blond", NA, NA, "none", "brown", "brown, grey", "brown", NA, "black", "auburn, white", "blond", "auburn, grey", "brown", "brown", NA, NA, "brown", "brown", "…
    $ skin_color <chr> "fair", "gold", "white, blue", "white", "light", "light", "light", "white, red", "light", "fair", "fair", "fair", "unknown", "fair", "green", "green-tan, brow…
    $ eye_color  <chr> "blue", "yellow", "red", "yellow", "brown", "blue", "blue", "red", "brown", "blue-gray", "blue", "blue", "blue", "brown", "black", "orange", "hazel", "blue", …
    $ birth_year <dbl> 19.0, 112.0, 33.0, 41.9, 19.0, 52.0, 47.0, NA, 24.0, 57.0, 41.9, 64.0, 200.0, 29.0, 44.0, 600.0, 21.0, NA, 896.0, 82.0, 31.5, 15.0, 53.0, 31.0, 37.0, 41.0, 48…
    $ sex        <chr> "male", "none", "none", "male", "female", "male", "female", "none", "male", "male", "male", "male", "male", "male", "male", "hermaphroditic", "male", "male", …
    $ gender     <chr> "masculine", "masculine", "masculine", "masculine", "feminine", "masculine", "feminine", "masculine", "masculine", "masculine", "masculine", "masculine", "mas…
    $ homeworld  <chr> "Tatooine", "Tatooine", "Naboo", "Tatooine", "Alderaan", "Tatooine", "Tatooine", "Tatooine", "Tatooine", "Stewjon", "Tatooine", "Eriadu", "Kashyyyk", "Corelli…
    $ species    <chr> "Human", "Droid", "Droid", "Human", "Human", "Human", "Human", "Droid", "Human", "Human", "Human", "Human", "Wookiee", "Human", "Rodian", "Hutt", "Human", "Hu…
    $ films      <list> <"The Empire Strikes Back", "Revenge of the Sith", "Return of the Jedi", "A New Hope", "The Force Awakens">, <"The Empire Strikes Back", "Attack of the Clone…
    $ vehicles   <list> <"Snowspeeder", "Imperial Speeder Bike">, <>, <>, <>, "Imperial Speeder Bike", <>, <>, <>, <>, "Tribubble bongo", <"Zephyr-G swoop bike", "XJ-6 airspeeder">,…
    $ starships  <list> <"X-wing", "Imperial shuttle">, <>, <>, "TIE Advanced x1", <>, <>, <>, <>, "X-wing", <"Jedi starfighter", "Trade Federation cruiser", "Naboo star skiff", "Je…

№4. Сколько уникальных рас персонажей (species) представлено в данных?

    > starwars %>% select(species) %>% unique()

    # A tibble: 38 × 1
       species       
       <chr>         
     1 Human         
     2 Droid         
     3 Wookiee       
     4 Rodian        
     5 Hutt          
     6 Yoda's species
     7 Trandoshan    
     8 Mon Calamari  
     9 Ewok          
    10 Sullustan     
    # ℹ 28 more rows
    # ℹ Use `print(n = ...)` to see more rows

Выведем на экран все 28 строк, в которых содержится результат работы
команды.

    > starwars %>% select(species) %>% unique() %>% print(n = 38)

    # A tibble: 38 × 1
       species       
       <chr>         
     1 Human         
     2 Droid         
     3 Wookiee       
     4 Rodian        
     5 Hutt          
     6 Yoda's species
     7 Trandoshan    
     8 Mon Calamari  
     9 Ewok          
    10 Sullustan     
    11 Neimodian     
    12 Gungan        
    13 NA            
    14 Toydarian     
    15 Dug           
    16 Zabrak        
    17 Twi'lek       
    18 Vulptereen    
    19 Xexto         
    20 Toong         
    21 Cerean        
    22 Nautolan      
    23 Tholothian    
    24 Iktotchi      
    25 Quermian      
    26 Kel Dor       
    27 Chagrian      
    28 Geonosian     
    29 Mirialan      
    30 Clawdite      
    31 Besalisk      
    32 Kaminoan      
    33 Aleena        
    34 Skakoan       
    35 Muun          
    36 Togruta       
    37 Kaleesh       
    38 Pau'an

№5. Найти самого высокого персонажа.

    > starwars %>% filter(height == max(height, na.rm = TRUE)) %>% select(name, height)

    # A tibble: 1 × 2
      name        height
      <chr>        <int>
    1 Yarael Poof    264

№6. Найти всех персонажей ниже 170

    > starwars %>% select(name, height) %>% filter(height < 170) %>% print(n = 23)

    # A tibble: 23 × 2
       name                  height
       <chr>                  <int>
     1 C-3PO                    167
     2 R2-D2                     96
     3 Leia Organa              150
     4 Beru Whitesun lars       165
     5 R5-D4                     97
     6 Yoda                      66
     7 Mon Mothma               150
     8 Wicket Systri Warrick     88
     9 Nien Nunb                160
    10 Watto                    137
    11 Sebulba                  112
    12 Shmi Skywalker           163
    13 Dud Bolt                  94
    14 Gasgano                  122
    15 Ben Quadinaros           163
    16 Cordé                    157
    17 Barriss Offee            166
    18 Dormé                    165
    19 Zam Wesell               168
    20 Jocasta Nu               167
    21 Ratts Tyerell             79
    22 R4-P17                    96
    23 Padmé Amidala            165

№7. Подсчитать ИМТ (индекс массы тела) для всех персонажей. ИМТ
подсчитать по формуле 𝐼 = 𝑚 ℎ2 , где 𝑚 – масса (weight), а ℎ – рост
(height).

    > starwars %>% mutate(BMI = mass / ((height / 100) ^ 2)) %>% select(name, BMI)

    # A tibble: 87 × 2
       name                 BMI
       <chr>              <dbl>
     1 Luke Skywalker      26.0
     2 C-3PO               26.9
     3 R2-D2               34.7
     4 Darth Vader         33.3
     5 Leia Organa         21.8
     6 Owen Lars           37.9
     7 Beru Whitesun lars  27.5
     8 R5-D4               34.0
     9 Biggs Darklighter   25.1
    10 Obi-Wan Kenobi      23.2
    # ℹ 77 more rows
    # ℹ Use `print(n = ...)` to see more rows

Выведем на экран все 87 строк, в которых содержится результат работы
команды.

    > starwars %>% mutate(BMI = mass / ((height / 100) ^ 2)) %>% select(name, BMI) %>% print(n = 87)

    # A tibble: 87 × 2
       name                    BMI
       <chr>                 <dbl>
     1 Luke Skywalker         26.0
     2 C-3PO                  26.9
     3 R2-D2                  34.7
     4 Darth Vader            33.3
     5 Leia Organa            21.8
     6 Owen Lars              37.9
     7 Beru Whitesun lars     27.5
     8 R5-D4                  34.0
     9 Biggs Darklighter      25.1
    10 Obi-Wan Kenobi         23.2
    11 Anakin Skywalker       23.8
    12 Wilhuff Tarkin         NA  
    13 Chewbacca              21.5
    14 Han Solo               24.7
    15 Greedo                 24.7
    16 Jabba Desilijic Tiure 443. 
    17 Wedge Antilles         26.6
    18 Jek Tono Porkins       34.0
    19 Yoda                   39.0
    20 Palpatine              26.0
    21 Boba Fett              23.4
    22 IG-88                  35  
    23 Bossk                  31.3
    24 Lando Calrissian       25.2
    25 Lobot                  25.8
    26 Ackbar                 25.6
    27 Mon Mothma             NA  
    28 Arvel Crynyd           NA  
    29 Wicket Systri Warrick  25.8
    30 Nien Nunb              26.6
    31 Qui-Gon Jinn           23.9
    32 Nute Gunray            24.7
    33 Finis Valorum          NA  
    34 Jar Jar Binks          17.2
    35 Roos Tarpals           16.3
    36 Rugor Nass             NA  
    37 Ric Olié               NA  
    38 Watto                  NA  
    39 Sebulba                31.9
    40 Quarsh Panaka          NA  
    41 Shmi Skywalker         NA  
    42 Darth Maul             26.1
    43 Bib Fortuna            NA  
    44 Ayla Secura            17.4
    45 Dud Bolt               50.9
    46 Gasgano                NA  
    47 Ben Quadinaros         24.5
    48 Mace Windu             23.8
    49 Ki-Adi-Mundi           20.9
    50 Kit Fisto              22.6
    51 Eeth Koth              NA  
    52 Adi Gallia             14.8
    53 Saesee Tiin            NA  
    54 Yarael Poof            NA  
    55 Plo Koon               22.6
    56 Mas Amedda             NA  
    57 Gregar Typho           24.8
    58 Cordé                  NA  
    59 Cliegg Lars            NA  
    60 Poggle the Lesser      23.9
    61 Luminara Unduli        19.4
    62 Barriss Offee          18.1
    63 Dormé                  NA  
    64 Dooku                  21.5
    65 Bail Prestor Organa    NA  
    66 Jango Fett             23.6
    67 Zam Wesell             19.5
    68 Dexter Jettster        26.0
    69 Lama Su                16.8
    70 Taun We                NA  
    71 Jocasta Nu             NA  
    72 Ratts Tyerell          24.0
    73 R4-P17                 NA  
    74 Wat Tambor             12.9
    75 San Hill               NA  
    76 Shaak Ti               18.0
    77 Grievous               34.1
    78 Tarfful                24.8
    79 Raymus Antilles        22.4
    80 Sly Moore              15.1
    81 Tion Medon             18.9
    82 Finn                   NA  
    83 Rey                    NA  
    84 Poe Dameron            NA  
    85 BB8                    NA  
    86 Captain Phasma         NA  
    87 Padmé Amidala          16.5

№8. Найти 10 самых “вытянутых” персонажей. “Вытянутость” оценить по
отношению массы (mass) к росту (height) персонажей.

    > starwars %>% mutate(long_per = mass / height) %>% arrange(desc(long_per)) %>% head(10) %>% select(name, long_per)

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

№9. Найти средний возраст персонажей каждой расы вселенной Звездных
войн.

    > starwars %>% group_by(species) %>% summarise(mean(birth_year, na.rm = TRUE))
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
    # ℹ Use `print(n = ...)` to see more rows

Выведем на экран все 38 строк, в которых содержится результат работы
команды.

    > starwars %>% group_by(species) %>% summarise(mean(birth_year, na.rm = TRUE)) %>% print(n = 38)
    # A tibble: 38 × 2
       species        `mean(birth_year, na.rm = TRUE)`
       <chr>                                     <dbl>
     1 Aleena                                    NaN  
     2 Besalisk                                  NaN  
     3 Cerean                                     92  
     4 Chagrian                                  NaN  
     5 Clawdite                                  NaN  
     6 Droid                                      53.3
     7 Dug                                       NaN  
     8 Ewok                                        8  
     9 Geonosian                                 NaN  
    10 Gungan                                     52  
    11 Human                                      53.4
    12 Hutt                                      600  
    13 Iktotchi                                  NaN  
    14 Kaleesh                                   NaN  
    15 Kaminoan                                  NaN  
    16 Kel Dor                                    22  
    17 Mirialan                                   49  
    18 Mon Calamari                               41  
    19 Muun                                      NaN  
    20 Nautolan                                  NaN  
    21 Neimodian                                 NaN  
    22 Pau'an                                    NaN  
    23 Quermian                                  NaN  
    24 Rodian                                     44  
    25 Skakoan                                   NaN  
    26 Sullustan                                 NaN  
    27 Tholothian                                NaN  
    28 Togruta                                   NaN  
    29 Toong                                     NaN  
    30 Toydarian                                 NaN  
    31 Trandoshan                                 53  
    32 Twi'lek                                    48  
    33 Vulptereen                                NaN  
    34 Wookiee                                   200  
    35 Xexto                                     NaN  
    36 Yoda's species                            896  
    37 Zabrak                                     54  
    38 NA                                         62 

№10. Найти самый распространенный цвет глаз персонажей вселенной
Звездных войн.

    > starwars %>% count(eye_color) %>% filter(n == max(n))

    # A tibble: 1 × 2
      eye_color     n
      <chr>     <int>
    1 brown        21

№11. Подсчитать среднюю длину имени в каждой расе вселенной Звездных
войн.

    > starwars %>% group_by(species) %>% summarise(avg_name_len = mean(nchar(name)))
    # A tibble: 38 × 2
       species   avg_name_len
       <chr>            <dbl>
     1 Aleena           13   
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
    # ℹ Use `print(n = ...)` to see more rows

Выведем на экран все 38 строк, в которых содержится результат работы
команды.

    > starwars %>% group_by(species) %>% summarise(avg_name_len = mean(nchar(name))) %>% print(n = 38)

    # A tibble: 38 × 2
       species        avg_name_len
       <chr>                 <dbl>
     1 Aleena                13   
     2 Besalisk              15   
     3 Cerean                12   
     4 Chagrian              10   
     5 Clawdite              10   
     6 Droid                  4.83
     7 Dug                    7   
     8 Ewok                  21   
     9 Geonosian             17   
    10 Gungan                11.7 
    11 Human                 11.3 
    12 Hutt                  21   
    13 Iktotchi              11   
    14 Kaleesh                8   
    15 Kaminoan               7   
    16 Kel Dor                8   
    17 Mirialan              14   
    18 Mon Calamari           6   
    19 Muun                   8   
    20 Nautolan               9   
    21 Neimodian             11   
    22 Pau'an                10   
    23 Quermian              11   
    24 Rodian                 6   
    25 Skakoan               10   
    26 Sullustan              9   
    27 Tholothian            10   
    28 Togruta                8   
    29 Toong                 14   
    30 Toydarian              5   
    31 Trandoshan             5   
    32 Twi'lek               11   
    33 Vulptereen             8   
    34 Wookiee                8   
    35 Xexto                  7   
    36 Yoda's species         4   
    37 Zabrak                 9.5 
    38 NA                    11

## Оценка результатов

В ходе выполнения практического задания были изучены функции пакета
`dplyr`.

## Вывод

Были получены базовые навыки обработки данных с помощью языка R и
встроенного пакета `dplyr`.
