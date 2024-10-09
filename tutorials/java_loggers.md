############################

############################
# JAVA LOGGERS
############################

############################

# log
"log" kelimesi Türkçe'de "kayıtların bütünü" anlamına geliyor. (ek olarak çok farklı anlamları da var.)

# graylog
Java ile yazılmış bir sunucudur. buraya herkes HTTP gibi  protokollerle log atıyor uzaktan. kolay bir arayüz sunuyor log'ların okunması için. tüm sunucular buraya log atıyor. tek bir yerde depolanıyor ve yönetiliyor.

# Log4j
Java için log kütüphanesi.

default olarak; Log4j.properties yada log4j.xml dosyasından konfigürasyonları okur.

log4j 2'inci sürümü log4j2.properties yada XML dosyasını okur.

# Logback
Log4j'nin 1'inci sürümündeki bazı köklü sorunlarını gidermek için başlatılan proje. Log4j 2'inci sürümünde bunları giderdi, fakat Logback projesi hala devam etmektedir.

Logback config örneği:

aşağıdaki dosya opsiyonel olarak farklı profil dosyalarına referans ettirebiliriz:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration debug="true">
    <springProfile name="prod">
        <include resource="logback-prod.xml"/>
    </springProfile>
    <springProfile name="test">
        <include resource="logback-test.xml"/>
    </springProfile>
    <springProfile name="default,development">
        <include resource="org/springframework/boot/logging/logback/base.xml"/>
    </springProfile>
</configuration>
```

logback-prod.xml dosyası örneği:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<included>
    <!-- include ile Spring'in içerisinde gelen default tanımları import ediyoruz-->
    <include resource="org/springframework/boot/logging/logback/base.xml"/>

    <!-- property variable'ı kullanıyoruz. burası Spring properties'ten variable çekmemizi sağlıyor.-->
    <springProperty scope="context" name="application_name" source="Spring.application.name"/>

    <!--console'a basmamızı sağlayan appender. file-appender, yada logstash-tcp-appender gibi alternatifler vardır.-->
    <appender name="flatConsoleAppender" class="ch.qos.logback.core.ConsoleAppender">

        <!--encoder'lar içerisine yazılan bilgileri formatlı yazar. Logstash içerisinde gelen bu encoder ilk Logstash'in kolayca parse edebileceği bir formata çeviriyordu data'ları fakat daha sonrada generic bir encoder halini aldı. -->
        <encoder class="net.logstash.logback.encoder.LoggingEventCompositeJsonEncoder">,

            <!-- providers içerisindeki her data direk output'a gönderilecek anlamına gelir.-->
            <providers>
                <timestamp>
                    <timeZone>UTC</timeZone>
                </timestamp>
                <version/>
                <logLevel/>
                <message/>
                <threadName/>
                <context/>
                <pattern>
                    <pattern>
                        {
                        "my_custom_env": "${my_custom_env}",
                        "java_class": "%C",
                        "trace": {
                            "trace_id": "%mdc{X-B3-TraceId}",
                            "span_id": "%mdc{X-B3-SpanId}",
                            "parent_span_id": "%mdc{X-B3-ParentSpanId}"
                            }
                        }
                    </pattern>
                </pattern>
                <stackTrace/>
            </providers>
        </encoder>
    </appender>

    <!--log 7seviyesi info olan log'ları içerisindeki appender'a yazar.-->
    <root level="info">
      <!--yukarıdaki appender'ımızın name'i-->
        <appender-ref ref="flatConsoleAppender"/>
    </root>
</included>
```

yukarıdaki config ile her satırın (log'un) örnek çıktısı:

```json
{
    "@timestamp": "2020-10-16T10:19:47.741Z",
    "@version": "1",
    "level": "WARN",
    "message": "log message from log.info( message ) ",
    "thread_name": "org.springframework.Kafka.KafkaListenerEndpointContainer#0-0-C-1",
    "application_name": "user-service",
    "hostname": "hostname_of_machine",
    "java_class": "org.Apache.Kafka.clients.NetworkClient",
    "trace": {
        "trace_id": "43344532",
        "span_id": "436546574",
        "parent_span_id": "34645364"
    }
}
```

# Log4j12
Log4j'nin 1.2'inci sürümünün paket adıdır. artık geliştirilmemektedir.

# Commons-logging (or old-name:Jakarta Commons Logging or old-name:JCL)
Java için log kütüphanesi. Apache-commons projesinin bir parçasıdır.

Commons-logging, SLF4J gibi tek başına kullanılamaz. başka bir logger altyapısına yönlenmesi gerekir. SL4J ile benzer yapıdadır, fakat SLF4J'den önemli bir farklılığı vardır: JCL classpath'te olan bir kütüphaneyi bulur ve log'ları ona yönlendirir. yani kullanılacak logger kütüphanesi JCL implementasyonu olmak zorunda değildir. örneğin bir projede Log4j ve JCL kütüphanesi olması yeterlidir. JCL otomatik lo4j'ye yönlenecektir.

# SLF4J (or Simple Logging Facade for Java)
Birçok Java log kütüphanesi için facada (ön adaptör) sunan bir kütüphanedir. bu şekilde kullanıcı istediği log kütüphanesine geçişi çok kolay bir şekilde yapabilmektedir. SLF4J tek başına kullanılmaz. diğer kütüphanelerin ortak bir API üzerinden kullanılmasını (çağrılmalarını) sağlar.

SL4J ilk başladığında bir implementasyon arar. eğer yok ise; default olarak kendi içinde gelen no-operation (or NOP or NOPLogger) implementasyonunu kullanır. eğer birden fazla implementasyon kütüphanesi classpath'e eklenmiş ise; o zaman SL4J bunlardan sadece 1 tanesini kullanır ve bize sadece bir uyarı verir. kullandığımız kütüphanelerde (örneğin; SQL-server-client) SL4J implementasyonu "optional" dependency olarak gelir. böylece herkes eklediği her kütüphanede birden fazla SL4J implementasyonu sorununu yaşamamış olur.

SL4J başlarken org.SL4J.impl.StaticLoggerBinder sınıfını arar. bu sınıf her SL4J implementasyonunda mevcuttur.

- SL4J-simple: çok basit bir SL4J implementasyonudur.

- SL4J-jdk14: bir SL4J implementasyonudur. Java.util.logging kullanmamızı sağlar.

- SL4J-API: SL4J core kütüphanesinin paket adıdır.

- SL4J binding: bu terim SL4J implementasyonunu kullanılması anlamında kullanılır

# Slf4net
Slf, facade olduğundan birçok dilde aynı API'yi destekliyor. bu pakette ".Net" framework'ü için geliştirilmiş Slf'dir.

# example of SL4J with Log4j binding
Aşağıdaki dependency'ler şarttır:

- SL4J-API
- Log4j-SL4J-impl
- Log4j-API //Log4j impl bunu şart koşuyor.
- Log4j-core //Log4j için gerekli bir bağımlılık

# bridge vs migrator
bridge paketleri JUL-to-SL4J gibi arada "to" olacak şekilde isimlendirilir. migrator paketlerinde ise arada "over" keyword'ü yer alır. örnekler:

> JCL-over-SL4J

> Log4j-over-SL4J

> JUL-to-SL4J

- migrator paketleri classpath'te var olan diğer log kütüphanesinin kodlarını(API'lerini) ezer ve log'ları SL4J'ye aktarır. örnek: jcl-over-SL4J: classpath'te JCL olan kütüphanenin kodlarını ezer ve SL4J'ye aktarır.

- bridge paketlerinin de amacı aynıdır. fakat farklı şekilde çalışır. örnek üzerinden gidersek: JUL-to-SL4J paketi, classpath'teki JUL API'lerine Handler ekler ve log'ları SL4J'ye aktarır. Bu handler'lara SLF4JBridgeHandler ismi verilir.

JUL migrator paketi olamaz. çünkü JUL, "java.#" paketleri altındadır. bu paketler ezilemeyeceğinden JUL için migrator paketi yaratılamıyor.

# java.util.logging (or JUL)
JRE içinde geliyor. logging.properties dosyasını okur.

```java
val l = java.util.logging.Logger.getLogger("MyClass"); // aldığı parametre o andaki sınıfın ismidir. böylece log'u attığımızda hangi sınıfın o log'u attığını okuyabiliriz.
```

# isteğe bağlı handler'lar
handler atanmazsa her zaman default handler olur.

```java
val h = new ConsoleHandler

h.setLevel(Level.ALL) // we output ALL the logs to the console

val f = new FileHandler("warn.log", true)

f.setLevel(Level.WARNING) // we also send all WARNINGs and more criticals to a file

f.setFormatter(new SimpleFormatter)

l.addHandler(h)

l.addHandler(f)
```

# Log4j FileAppender vs RollingFileAppender vs DailyRollingFileAppender

RollingFileAppender FileAppender'dan extend eder ve dosya boyutu maximum boyuta geldiğinde yeni bir dosya oluşturur eskisini backup alır.

DailyRollingFileAppender RollingFileAppender'ı extend eder ve dosya boyutu maximum boyuta gelince değil, istenilen gün sayısı sonrasında yeni dosyaya geçiş yapar.

# log levels
SL4J ve Log4j ve Log4j2 için:

- Trace: sadece developer'ın anlayacağı log'lardır. fonksiyonların içlerinde kodun nerede olduğunu belli etmek için kullanılır.

- Debug: IT çalışanlarının de anlayacağı log'lama olmalı. IT çalışanlarının bir sorunu incelerken buraya başvurabilmeli.

- Info: servis başladı. kapandı gibi bilgileri vermek için kullanılır.

- Warn: uygulama için bazen önemli olabilecek akışları bildirmeli. örneğin; soket timeout verdi. tekrar deneniyor. buralar bir hata değildir. sayısı çok arttıkça uygulama incelemeye alınmalı, yada uygulamada bir terslik olduğunda ilk bakılacak kısımlar burada olmalı.

- Error: hataların yazıldığı seviye

- Fatal: sistemin durması gerektiğinde veya zarar gördüğü durumlarda

- All: bu bir seviye değildir. log basarken tüm log'ların basılacağını belirtir.

Yukarıdaki level'lar kütüphaneden kütüphaneye değişebiliyor. bazılarında bazı levellar olmuyor, bazılarında yukarıdakilerden daha fazla çeşit level sunuluyor. bazı diğer leveller:

- level -> yukarıdaki seviyelerden denk olanı

- SEVERE (Türkçe kelime anlamı: şiddetli) -> Error

- WARNING -> Warn

- CONFIG -> Info

- FINE -> Debug

- FINER -> Debug

- FINEST -> Debug/Trace

- VERBOSE (Türkçe kelime anlamı: gereksiz sözler) -> Trace

# audit trail (or audit log)
audit kelime anlamı: denetim

trail kelime anlamı: iz

audit log'lar domain event'leri gibi, domain'de olan her olayın log'larıdır. örnek: a transaction is created, a user is performing an action... Aslında audit log'ları user'ın activity'lerini takip etmek için kullanılan log'lardır.

# best practices:
- SLF4 kullanırken;

log.debug("Found " + records + " records matching filter: '" + filter + "'");

yerine bunu kullanmak daha iyi:

log.debug("Found {} records matching filter: '{}'", records, filter);

Log4j'de SL4j gibi bir metot yok. bu sebeple ilk satırdaki yöntem kullanılmak zorundadır.

- if(logger.isDebugEnabled) gibi kodlar kullanılmamalı

- hatalar sadece bu şekilde log'lanmalıdır:

log.error("Error reading configuration file", e);

- log.debug("the name is");

  log.debug(name);

gibi iki satıra kesinlikle yayılmamalı. paralelden gelen log'lar bu ikisi arasına girebilir ve log'lar anlamını kaybeder.

- her sınıfın toString'i olmalı ki log'lar düzgünce basılabilsin. fakat gizli bilgiler loga basılmayacağı için onlara ekstra dikkat edilmeli. gerekirse toString'den gizli bilgiler çıkarılmalı veya hash'lenerek tutulmalı (or hash'lenerek print edilmeli).

- log.info("Processing ", request.size());  request'in null gelme durumunda bu kod satırı patlayacaktır. request null kontrolü yapılması istenmiyor ise; request'in toString'i olmalı ve size() metotu kullanılmamalıdır. zaten logger toString'i çağırdığında, log'ları okuyan kişi dizinini boyutunu da çıkarabilecektir. eğer bu şekilde istenmiyor ise; null kontrolü yapılmalıdır.

# Spring'de logging
Spring'de Java sınıflarının bulunduğu paket isimleri ile log level'ı belirleyebiliriz. örnek:

```yml
logging:

  level:

    ROOT: ERROR  # tüm sistem için genel seviyeyi temsil ediyor

    org.springframework: ERROR

    org.springframework.security: DEBUG

    org.springframework.security.web.FilterChainProxy: ERROR

    com.myapppackages: INFO # kendi sınıflarımız

    org.MongoDB: ERROR   # Üçüncü parti dependency olsa bile bunu yapabiliriz

  file: /Users/myapp/application.log
```

Spring kendi içinde Commons-logging kullanıyor. SLF4J kullanılmamasının sebebi eskiden kalma olduğundan değiştirilmesinin çok maliyetli olmasıdır. Commons-logging yerine sl4j kullanacaksak, JCL-yi exclude edip sl4j'yi eklememiz (ve SL4J implementasyonu eklememiz) yeterli olacaktır. fakat Log4j kullanacaksak; Log4j eklemek yeterli olacaktır. JCL zaten otomatik Log4j'yi görmektedir.

Spring-boot-starter, default olarak JCL'yi Logback'e yönlendirilmektedir ve default olarak logback-Spring.xml dosyasından config'leri okumaktadır.
