Axenix: путешествия легко
===
## Описание проекта:
Проект представляет собой разработанный алгоритм для персонализированных маршрутов по заданным пользователем параметрам, таким как время отправления, время прибытия, место отправления и прибытия. Решение включает в себя индивидуальные пожелания по наличию пересадок. Помимо этого, сервис гибко интегрирует разные виды транспорта и предлагает всевозможные маршруты, минимизирует время пути и количество пересадок.

Минимизация времени пути и количества пересадок включает в себя опции сортировки только по времени (от самого короткого по времени маршрута до самого долгого), только по количеству пересадок (от решения с самым маленьким количеством пересадок до решений с наибольшим их количеством) и по пересадкам и времени одновременно (таким образом, маршруты с одинаковым временем затем анализируются по количеству пересадок)

Разработан концепт приложения, который позволяет пользователю использовать единую платформу для принятия решения о выборе сложного маршрута. Интуитивно понятный интерфейс учитывает все пожелания пользователя и наглядно отображает все варианты путей.

### Установка и настройка
**1. Убедитесь, что у вас установлен Python 3.10 и выше:**
  ``` bash
python --version
```
**2. Установите необходимые библиотеки:**
Убедитесь, что у вас есть файл requirements.txt, и выполните команду для установки всех зависимостей:
``` bash
pip install -r requirements.txt
```
***
### Запуск проекта
Запуск алгоритма производится через файл MainFile.py:
``` bash
python MainFile.py
```
Все остальные файлы являются вспомогательными и используются в процессе работы алгоритма.
Ввод данных чувствителен к регистру — обратите на это внимание при вводе значений.
***
### Пример ввода
<img src="https://github.com/dqnbnm/ComplexRoute/blob/main/photo_2025-03-16_18-07-03.jpg?raw=true">

***
### Основной функционал
* Построение маршрутов
* Сортировка билетов

### Дизайн приложения
**Дизайн интерфейса можно найти в следующих файлах (полный набор изображений находится в папке Axenix Design):**
* Профиль клиент.png - экран личного кабинета
* построить маршрут.png - экран построения маршрута
* найденные маршруты.png - экран представления найденных маршрутов
* подробнее.png - экран подробного описания пути

<img src="https://raw.githubusercontent.com/dqnbnm/ComplexRoute/main/Axenix%20Design/Профиль%20клиент.png" width="135" height="293"> <img src="https://raw.githubusercontent.com/dqnbnm/ComplexRoute/main/Axenix%20Design/построить%20маршрут.png" width="135" height="293"> <img src="https://raw.githubusercontent.com/dqnbnm/ComplexRoute/main/Axenix%20Design/найденные%20маршруты.png" width="135" height="293"> <img src="https://raw.githubusercontent.com/dqnbnm/ComplexRoute/main/Axenix%20Design/%D0%BF%D0%BE%D0%B4%D1%80%D0%BE%D0%B1%D0%BD%D0%B5%D0%B5.png" width="135" height="293">

### Возникшие проблемы и пути их решения
В связи с невозможностью получения полных данных о ценах на билеты легальным и бесплатным способом, было принято решение не добавлять этот функционал даже частично. Наработки по решению этой проблемы:
1. Стоимость билетов на поезда и электирички.
   
  Для выгрузки цены на поезд используется API от Tutu.ru. К сожалению, данные передаются не в режиме реального времени и получать цены с точной датой и временем невозможно. Функция для получения цен и примеры ввода-вывода представлены в Подсчет цен/Анализ_цен_на_поезда_автобусы_электрички.ipynb
  
2. Стоимость билетов на автобусы.
   
  Для подсчета примерной стоиммости используется формула
Цена = (100 + 1.5 * K * C * S) * (1 − D) + 120 * p , где
* 100 - базовая ставка
* 1.5 - Стоимость за км
* K - количество километров
* C - коэффициент класса обслуживания
* S - коэффициент сезонности (бывают периоды, когда спрос на автобусы выше, соотвественно, стоимость билета повышается)
* D - коэффициент скидки (от 0 включительно до 1 невключительно, 0 - по умолчанию)
* 120 - стоимость 1 места багажа
* p - где 0, если пассажир едет без багажа, 1 - если пассажир занимает 1 место в багаже, 2 - если 2 места

3. Стоимость билетов на самолеты

   Цены на авиабилеты получаем с помощью Aviasales API. Функция для получения цен в файле Цены на авиа/avia.py. Пример ввода-вывода 
<img src="https://raw.githubusercontent.com/dqnbnm/ComplexRoute/main/%D0%A6%D0%B5%D0%BD%D1%8B%20%D0%BD%D0%B0%20%D0%B0%D0%B2%D0%B8%D0%B0/avia.jpg" width="300">

4. Стоимость проезда на такси.
   
При помощи открытых источников от Яндекс Go была выведена приблизительная цена поездки на такси без учета динамического ценообразования(погода, загруженность дорог и тд.)
* Taxi_price_msk = 189 + (время в пути - 3) * 13 + (расстоение в км -1) * 13
* Taxi_price_spb = 149 +  (время в пути - 3) * 12 + (расстоение в км -1) * 12
* Taxi_price_regions =  90 + (время в пути - 3) * 6 + (расстоение в км -1) * 13

5. Просчёт времени в пути на такси для оценки времени пересадки внутри города.

Для решения данной проблемы можно использовать API 2gis, которое просчитывает время в пути с учётом пробок и расстояние между точками.
Для этого напишем 3 функции:

| Функция  | Описание |
| ------------- | ------------- |
| get_coordinates  | Вспомогательная функция. На вход подаётся название/адрес локации и при помощи 2GIS Geocoding API находятся координаты. Формат выходных данных - кортеж (широта, долгота)|
| get_travel_time  | Основная функция. Рассчитывает время в пути между двумя точками с использованием 2GIS Routing API. На вход подаётся начальная и конечная локация. На выходе - время в пути в секундах  |
| get_travel_distance  | Аналогичная функция, только рассчитывает расстояние между локациями в метрах  |

Данные функции хранятся в файле 2gisTimeAndDistance в папке Подсчет цен
<img src = "https://github.com/dqnbnm/ComplexRoute/blob/main/Подсчет%20цен/image.png" width="600">

### Содержание файлов
Название    | Содержание 
-----------------|----------------------
Axenix Design |Дизайн-файлы
расчет метрики оптимальности| 
Подсчет цен| Выгрузка данных по ценам для поездов через API Tutu.ru. Получена формула для подсчета примерной стоимости билета на междугородние автобусы. Получено время в пути между двумя точками на такси 
Цены на авиа| Выгрузка данных по ценам для самолетов через API Aviasales
AllStation.py    |Формирование словаря со станциями (их коды, долготы и широты)
CostProcessing.py| 
InputInformation.py| Получение данных: Получение данных о кол-ве городов. Получение списка городов. Получение списка дат. Получение информации о потребности в пересадках. Получение информации о типе сортировки. На каждом этапе проверка на корректность введенных значений
MainFile.py     |Объединение маршрутов с пересадками и прямых маршрутов в единый словарь с помощью schedule Последовательный запуск функций сортировки и вывода результата
PrintResult.py|Получает на вход итоговый словарь со всеми маршрутами. Проходится по словарю и выводит о каждом маршруте информацию: Наименование маршрута, от какой станции следует, до какой станции следует, время отправления, время прибытия, наличие пересадки, тип транспорта. В конце каждого маршрута выводит итоговую длительность и общее кол-во пересадок
Shedule.py |Получает на вход информацию о городе отправления, прибытия, дату,  информацию о потребности пересадок. Делает запрос через API Яндекс.Расписаний. Получает JSON с информацией о маршруте
Sorting.py|Получает на вход итоговый словарь со всеми маршрутами. В зависимости от выбранной сортировки, ранжирует словарь. Возможные варианты сортировок: по длительности, по пересадкам, по длительности и пересадкам. При выборе любой из сортировок результат сокращается до 50 запросов(если в результирующем словаре было более 50 маршрутов). Есть возможность отказаться от сортировки и сокращения результата до 50 маршрутов
input_example| Пример ввода
requirements.txt| 
stations_list.json|
