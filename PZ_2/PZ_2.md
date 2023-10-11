Практическое задание №2
================
Журавлева Юлия БИСО-01-20

# Тема

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

1.  Сколько уникальных рас персонажей (species) представлено в данных?

<!-- -->

    s

1.  Найти самого высокого персонажа.

<!-- -->

    s

1.  Найти всех персонажей ниже 170

<!-- -->

    s

1.  Подсчитать ИМТ (индекс массы тела) для всех персонажей. ИМТ
    подсчитать по формуле 𝐼 = 𝑚 ℎ2 , где 𝑚 – масса (weight), а ℎ – рост
    (height).

<!-- -->

    s

1.  Найти 10 самых “вытянутых” персонажей. “Вытянутость” оценить по
    отношению массы (mass) к росту (height) персонажей.

<!-- -->

    s

1.  Найти средний возраст персонажей каждой расы вселенной Звездных
    войн.

<!-- -->

    s

1.  Найти самый распространенный цвет глаз персонажей вселенной Звездных
    войн.

<!-- -->

    s

1.  Подсчитать среднюю длину имени в каждой расе вселенной Звездных
    войн.

<!-- -->

    s

1.  Оформить отчет в соответствии с шаблоном

<!-- -->

    s
