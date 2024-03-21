############################

############################
# ACTUATORS AND MONITORING TOOLS
############################

############################

# Spring Boot Actuator
Actuator kelime anlamı: 1. işletici, çalıştırıcı 2. uyarıcı

Spring'e dependency olarak eklendiğinde, o Spring uygulaması içerisinde REST endpoint'leri açıyor ve bu endpoint'ler üzerinden health check, disk usage, heap dump gibi bilgiler dışarıya veriyor.

http://localhost:8080/actuator -> bize Actuator'ın açtığı tüm metric gruplarının URL listesini döner. Liste aşağıdaki gibidir. Her URL, kendi içerisinde istediği bilgileri dışarıya açabilir. Bazı temel örnekler:

- http://localhost:8080/actuator/metrics -> current thread count, current memory usage, system update, all Java classes
- http://localhost:8080/actuator/beans -> all Spring beans
- http://localhost:8080/actuator/env -> all environment variables (her prop "app.yml", "bootstrap yml", Kubernetes'ten mi gelip gelmediği detayı ile birlikte yazıyor)
- http://localhost:8080/actuator/info -> information about Spring boot
- http://localhost:8080/actuator/trace -> all REST endpoints

Örneğin; "Kubernetes Java client"; Pod (container) name, namespace gibi bilgileri actuator/info'dan döndürüyor. Kubernetes isterse farklı bir URL'den bunları sunabilirdi. ama "Kubernetes Java client" geliştiricileri bu URL'den döndürecek şekilde geliştirme yapmış (standart olması açısından).

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Micrometer
JVM uygulamaları için facade kütüphanesidir. Bu kütüphane JVM based uygulamamız dışarı Spring-Actuator gibi bilgi vermektedir. Spring-Actuator bilgileri dışarı açarken belli bir formatta/standartta sunmaz. oysa Micrometer birçok monitoring tool'a destek verecek şekilde bunları dışarı açar.

Micrometer'ın support ettiği monitoring tool'lar:

AppOptics, Azure Monitor, Netflix Atlas, CloudWatch, Datadog, Dynatrace, Elastic, Ganglia, Graphite, Humio, Influx/Telegraf, JMX, KairosDB, New Relic, Prometheus, SignalFx, Google Stackdriver, StatsD, Wavefront.

Micrometer JVM uygulamalarında Spring-Actuator ile de entegreli çalışabilir. eğer istenirse Spring-Actuator'sız, saf Java projesinde de çalışabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Grafana
- birçok kaynaktan verileri çekerek, bunları kendisinin sunduğu web arayüzünden visualise eden bir yazılımdır. Topladığı dataları anlık olarak gösterir. Grafana herhangi bir veriyi toplayabilir. örnekler:
  - sadece metric'te okuyabilir: http://localhost:8080/metrics
  - PostgreSQL gibi RDBMS'lerde sadece bellirlediğimiz SQL sorguları çalıştırır ve o sorguların sonucunu kendi veritabanında toplar (örnek son 1 dakikadaki toplam siparişler: "SELECT count from Order WHERE date>(now-1m)").
  - Prometheus'tan, PostgreSQL'den okduğu gibi veri okuyablir. Grafana özellikle time-series data yönetimini çok iyi yaptığından dolayı Prometheus gibi data-source'lardan ve __time series DB (or TSDB)__ konularından çok bahsedilir.

- Grafana sadece şu zamanlarda kaynaktan data okur:
  - manuel query çalıştırdığımızda gider ve o data-source'tan bilgiyi alır.
  - dashboard'lar arkada query çalıştırır. ne sıklıkla query'nin execute edileceği, yine dashboard'un config'inden ayarlanır.

- Grafana üzerinden API key oluşturulabilir. Bu key'ler ile, Java gibi client'larımız aracılığı ile Grafana'ya query execute ettirebiliriz.

- API key yerine, Grafana web-UI üzerinde de query atabiliriz. Bu query'ler zaten dashboard oluşturmak için kullanılan query'lerle aynıdır.

- "Query Editor", Web-UI'daki "expore" sekmesindeki query oluşturmıza yarayan, auto-complete seçenekleri sunan HTML componentlerdir.

  Query editor bir query generate eder. bu query her data-source için ayrı bir formatta ve mantıktadır. örnek:

  PostgreSQL için query:

  ```
  SELECT hostname FROM host  WHERE region IN($region)
  ```

  PromQL için query:

  ```
  query_result(max_over_time(<metric>[${__range_s}s]) != <state>)
  ```

  Bu sebeple "query editor" data-source değiştirildiğinde (seçildiğinde) çok farklı bir component halini alabilir.

- Grafana kendi configurasyonlarını (aşağıdaki liste) kaydetmek için RDMBS (default'ta SQLite) kullanır:
  - user list to login Grafana Web UI
  - data sources connection info (ip, port, username, password...)
  - user generated dashboards

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Prometheus
Grafana'nın alternatifidir. Grafana ile en temel farkı Grafana DB'yi kendi tutmazken, Prometheus kendi DB'si mevcuttur (Bu DB time series DB'dir.). Fakat Prometheus'un visualizing modülü Grafana gibi başarılı değildir.

Prometheus bağımsız bir servis olarak ayağa kalkar. önceden her servisin healt (örneğin Spring'deki Actuator endpoint'leri) URL'si Prometheus'a tanıtılır. Prometheus bu URL'lere giderek düzenli olarak data çeker ve kendi database'sine kaydeder.

# exporter
Prometheus dünyasında "exporter"; okuduğu data'yı Prometheus'un anlayacağı formata çeviren yazılımdır. Piyasada neredeyse her yazılım Prometheus'a metric'lerini atabilmek için kendi exporter'ını geliştirmiştir. örnekler:
- https://github.com/danielqsj/kafka_exporter
- https://github.com/infinityworks/docker-hub-exporter

Bazı yazılımlar ise default'ta direk Prometheus'un anlayacağı formatta metric sunmaktadır. örnekler:
- Kubernetes
- RabbitMQ
- Zipkin

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •