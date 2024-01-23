Добавил контейнер графана в основной стек мониторинга 

    grafana:
    image: grafana/grafana
    user: root
    depends_on:
      - prometheus
    ports:
      - 3000:3000
    volumes:
      - ./grafana:/var/lib/grafana
      - ./grafana/provisioning/:/etc/grafana/provisioning/
    container_name: grafana
    hostname: grafana
    restart: always

добавил таргет графаны в конфиг прометеуса

	scrape_configs:
        ...
	  - job_name: grafana
	    static_configs:
	      - targets: ['grafana:3000']
	...
в самой графане добавил две папки с дашбордами (infra  и app)
сриншоты в папке screens
