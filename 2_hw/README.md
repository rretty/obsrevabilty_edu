Добавил контейнер victoriametrics для сбора метрики с прометеуса с глубиной хранения в 2 недели:
    victoriametrics:
    
    image: victoriametrics/victoria-metrics
    container_name: victoriametrics
    hostname: victoriametrics
    ports:
      - 8428:8428
    volumes:
      - ./vmdata:/storage
    command:
      - '--storageDataPath=/storage'
      - '--httpListenAddr=:8428'
      - '--retentionPeriod=2w' // глубина хранения 2 недели
    restart: always

Добавил в конфиг прометеуса отправку метрик в викторию по апи:

    remote_write:
    - url: "http://victoriametrics:8428/api/v1/write"
        queue_config:
        max_samples_per_send: 10000

с ограничением в 10к эксземпляров (с запасом по советам из документации)

+ добавил для скрейпа метрик виктории 
scrape_configs:
  - job_name: victoriametrics
    static_configs:
      - targets: ['victoriametrics:8428']

+ довил новый лейбл для маркировки метрик прометеуса в виктории

external_labels:
    site: prod
