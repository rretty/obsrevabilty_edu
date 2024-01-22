Дополнительно добавил два контейнера 
алертменеджер 

    alertmanager:
    image: prom/alertmanager:latest
    container_name: alertmanager
    hostname: alertmanager
    user: root
    ports:
      - 9093:9093
    volumes:
      - ./alertmanager/:/etc/alertmanager/
    restart: always
    command:
      - '--config.file=/etc/alertmanager/alertmanager.yml'
      - '--storage.path=/etc/alertmanager/data'

и контейнер интеграции с телеграмом (с контретным адресом контейнера алертменеджера, и токеном телеграм-бота):

    alertmanager-bot
    image: metalmatze/alertmanager-bot:0.4.3
    container_name: alertmanager-bot
    hostname: alertmanager-bot
    command:
      - --alertmanager.url=http://172.22.0.7:9093
      - --log.level=info
      - --store=bolt
      - --bolt.path=/data/bot.db
      - --telegram.admin=*******
      - --telegram.token=********
    user: root
    ports:
      - 9095:9095
    restart: always
    volumes:
      - ./telegram/data:/data

добавил в конфиг прометеуса таргет алертменеджера и конфиг правил алертов
    
    alerting:
      alertmanagers:
      - scheme: http
        static_configs:
        - targets: ['alertmanager:9093']
    
    rule_files:
      -  'rules.yml'
        ...
    scrape_configs:
      - job_name: alertmanager
    static_configs:
      - targets: ['alertmanager:9093']

и так же добавил несколько получателей в конфигурации алерменеджера
с сортировкой по важности и группы триггеров
  
    route:
    repeat_interval: 1h
    group_by: ['alertname', 'severity', 'groups']

    

