# monitor

## Serviços

```
services:
  prometheus:
    image: prom/prometheus:v2.30.3
    container_name: prometheus
    volumes:
      - ./prometheus:/etc/prometheus
      - prometheus_data:/prometheus
    command:
      - '--config.file=/etc/prometheus/prometheus.yml'
      - '--storage.tsdb.path=/prometheus'
      - '--web.console.libraries=/usr/share/prometheus/console_libraries'
      - '--web.console.templates=/usr/share/prometheus/consoles'
    ports:
      - 9090:9090
    restart: always

  node-exporter:
    image: prom/node-exporter:v1.3.1
    container_name: node-exporter
    volumes:
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /:/rootfs:ro
    command:
      - '--path.procfs=/host/proc'
      - '--path.sysfs=/host/sys'
      - '--collector.filesystem.mount-points-exclude=^/(sys|proc|dev|host|etc)($$|/)'
    ports:
      - 9100:9100
    restart: always

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    volumes:
      - grafana_data:/var/lib/grafana
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=admin
    ports:
      - 3000:3000
    restart: always

volumes:
  prometheus_data:
  grafana_data:
  ```

Este docker-compose.yml configura três serviços:

Prometheus: O servidor Prometheus que coleta e armazena as métricas.
Node Exporter: Um exportador que coleta métricas do sistema host.
Grafana: A interface de visualização para criar dashboards com as métricas coletadas.
Para usar este docker-compose:

Crie um diretório chamado prometheus no mesmo local do arquivo docker-compose.yml.

Dentro do diretório prometheus, crie um arquivo chamado prometheus.yml com o seguinte conteúdo:

```
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'node-exporter'
    static_configs:
      - targets: ['node-exporter:9100']
```

Pode usar o comando sudo nano prometheus.yml para criar e salvar o arquivo.

Execute o comando docker-compose up -d no diretório onde está o arquivo docker-compose.yml.
Após a inicialização dos containers:

Prometheus estará disponível em http://localhost:9090
Grafana estará disponível em http://localhost:3000 (use admin/admin para login inicial)
No Grafana, você precisará adicionar o Prometheus como fonte de dados (use http://prometheus:9090 como URL) e criar dashboards para visualizar as métricas coletadas.

Lembre-se de que este é um setup básico. Para um ambiente de produção, você deve considerar configurações adicionais de segurança, persistência de dados e personalização das métricas coletadas.

## Dashboards

Para monitoramento de máquina usando Prometheus e Grafana, existem vários templates de dashboard excelentes e prontos para uso. Um dos mais populares e abrangentes é o "Node Exporter Full" dashboard. Aqui estão os passos para usá-lo:

No Grafana, vá para "Create" (+) > "Import".

Na página de importação, insira o ID 1860 na caixa "Import via grafana.com".

Clique em "Load".

Selecione o datasource do Prometheus que você configurou.

Clique em "Import".