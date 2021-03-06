# Набор скриптов и веб-интерфейс для правки границ

В этих каталогах лежит набор инструментов для редактирования набора границ
в формате [Osmosis Poly](http://wiki.openstreetmap.org/wiki/Osmosis/Polygon_Filter_File_Format).
Для работы требуется база данных PostgreSQL + PostGIS, инициализированная из
файла `scripts/borders.sql`. Для оценки размера файла MWM нужно заполнить
таблицу `tiles` из файла планеты (см. `scripts/process_planet.sh`).

Также для обновления и замены границ из OpenStreetMap желательно импортировать
таблицу `osm_borders` — см. `scripts/osm_borders.sh`. Начальный набор границ
для редактирования можно либо загрузить скриптом `scripts/poly2postgis.py`,
либо скопировать из таблицы `osm_borders` по, например, `admin_level=2`.

После редактирования набор файлов `poly` создаст скрипт `scripts/export_poly.py`.

## Серверная часть

Два скрипта в каталоге `server` должны работать постоянно на фоне.

* `borders-api.py` — сервер на Flask, работает на порту 5000. К нему обращается
    веб-интерфейс через XHR-запросы. В начале скрипта проверьте названия таблиц
    и флаг READONLY.

* `borders-daemon.py` — непрерывно проверяет таблицу `borders` на пустые значения
    в столбце количества данных, и найдя их, пересчитывает. Запустите, если нужна
    оценка размера MWM.

## Веб-интерфейс

Файлы в каталоге `www` не требуют каких-либо интерпретаторов или выделенных серверов:
просто откройте `index.html` в браузере. На карте нарисованы границы, по клику
на границу панель справа наполнится кнопками. Оттуда можно разрезать и склеивать
границы, переименовывать их, заменять и дополнять из таблицы `osm_borders`,
а также экспортировать в JOSM для сложных модификаций.

## Автор и лицензия

Написал Илья Зверев для MAPS.ME, опубликовано под лицензией MIT.
