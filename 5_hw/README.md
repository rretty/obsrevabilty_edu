Развернул так же в докере для удобства
файл конфигурации в каталоге GAP-2

Скрипты обнаружений в том же каталоге
Реализация посредством двух простоейших скриптов: один задает имена метрик для пользовательских макросов {#METRIC}
второй передает непосредственно рандомные значения по каждой метрики
Скрипты без изящности выводят массив, воспринимаемый заббиксом как JSON, после чего создается элемент данных с предобработкой JSONPath $.{#METRIC}

Прототип элемента '{#METRIC}' с ключом 'otus_important_metrics[{#METRIC}]'
Прототип триггера '{#METRIC} to much values' с выражением 'last(/otus_test/otus_important_metrics[{#METRIC}])>95'

Скрины графиков значений в каталоге screens
Ссылка на тг канал: https://t.me/pivo_alerter



