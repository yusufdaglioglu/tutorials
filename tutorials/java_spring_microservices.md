############################

############################
# JAVA SPRING MICROSERVICES
############################

############################

# discovery server

micro-servisler, merkezi bir servise kendilerini register ederler. istenilen sayıda makine ona register olur ve son kullanıcı (yazılım geliştirici) buradan tüm servisleri monitör eder. bu ana sunucuya "discovery server" denilir. discovery server'a örnek olarak açık kaynaklı "Eureka server" verilebilir. Eureka server'ın client API'leri mevcut. bu API'ler ile register işlemleri çok kolay şekilde yapılabilmektedir. Alternatif discovery'ler: __Consul__, __Zookeeper__, __Etcd__.

"Zookeeper" ve "etcd" temelde key/value bir storage manager'dır. fakat yazılımcıya config'leri tutacak şekilde optimize edilmiştirler. Aslında, diğer NoSQL ve RDBMS'lere göre, CAP teoremindeki seçimleri/öncelikleri farklıdır. Zookeeper, CAP teoreminde P ve C'yi mutlaka sağlamaktadır. Fakat availability'den tölerans gösterir (feragat eder). Bu sebeple genelde "distributed coordination" için ve config'leri tutmak için kullanılırlar:
- açık olan microservis listesini tutarlar
- Kafka gibi sistemlerde topic listesini ve daha birçok bilgiyi tutarlar (Kafka'nın Zookeeper'da neleri tuttuğu Kafka'nın alt-başlığında anlatılıyor)

Çünkü; sistemdeki config'lerin veya servis listesini herkesle paylaşan Zookeeper'ın geç cevap vermesine tölerans gösterilebilir, fakat eski bilgi paylaşması ve bu bilgilerin merkezi yerde tutulmamasına tölerans gösterilemez.  

- # Eureka node types

    - # Eureka server

      diğer instance'laeın locationlarını tutan ve listesini tutan node'dur.

    - # Eureka service vs Eureka instance

      Eureka servis aynı isimdeki servisleri temsil etmek için kullanılan bir referanstır. her 'Eureka servis', birer 'servise ID' ile temsil edilmektedir. bir adet Eureka servisi, birden fazla instance'a denk gelebilir. instance'lar gerçek node'lara denk gelmektedir. örneğin; "order-servise" isimli Eureka servisine ait, 10 adet Eureka instance'ı olabilir.

    - # Eureka client

      any application can discover services. kendisini servis(instance) olarak tanıtmak zorunda değildir.

- # Eureka config

  Eureka'da her tipte yazılım için ayrı config'ler vardır. bunlar:

  - Eureka server config: org.springframework.cloud.netflix.eureka.server.EurekaServerConfigBean that implements com.netflix.eureka.EurekaServerConfig. All properties are prefixed by "eureka.server".

  - Eureka client config: org.springframework.cloud.netflix.eureka.EurekaClientConfigBean that implements com.netflix.discovery.EurekaClientConfig. All properties are prefixed by "eureka.client".

  - Eureka instance config: org.springframework.cloud.netflix.eureka.EurekaInstanceConfigBean that implements (indirectly) com.netflix.appinfo.EurekaInstanceConfig. All properties are prefixed by "eureka.instance".

- # Eureka config properties

  - Eureka.client.registerWithEureka

    Eureka server dahi kendisi bir client barındırır. ve client'ın kendi instance'ındaki server'a (kendi kendine) register olmasını istemiyorsak bu değeri Eureka server'da false'a çekmeliyiz. bu değer true olursa; node, server'a kendini instance olarak tanıtmaktadır.

  - Eureka.client.fetchRegistry = true

    Eureka sunucusundan, ona bağlı tüm servislerin bilgilerini çekmemize yarar. bunu yapan her node, instance olmak zorunda değildir (yani başkaları tarafından servis olarak algılanabilmek/erişilebilmek zorunda değildir.)

  - eureka.client.shouldDisableDelta = true

    fetchRegistry işleminde sadece değişikliklerin çekilmesi istenir. böylece network bandwidth'den kar edilmiş olur.

  - eureka.client.registryFetchIntervalSeconds = 30s

    fetchRegistry işlemi diğer işlemlerden bağımsızdır ve her 30 saniyede bir yapılır.

  - eureka.client.cacheRefreshExecutorExponentialBackOffBound

    fetchRegistry isteği, cacheRefreshExecutorExponentialBackOffBound*registryFetchIntervalSeconds süresi kadar başarısız olursa, artık node istek atmayı durdurur.

  - eureka.instance.leaseRenewalIntervalInSeconds = 30s

    instance olan her node, server'a 30 saniyede 1 heartbeat isteği atmaktadır.

  - eureka.instance.leaseExpirationDurationInSeconds = 90s

    eğer 90 saniye içinde instance'den heartbeat gelmez ise, server o node'u listeden kaldırır ve erişilemez yapar.

  - eureka.client.initialInstanceInfoReplicationIntervalSeconds = 40s

    com.netflix.appinfo.InstanceInfo değerinde instance hakkında birçok metadata vardır. bu metadata'lar sunucuya sürekli düzenli yollanır. bu işlem hearbeat'den bağımsızdır. bu bilginin sunucuya ilk olarak, sunucuya bağlanıldıktan 40 saniye sonra yollanır.

  - eureka.client.instanceInfoReplicationIntervalSeconds = 30s

    com.netflix.appinfo.InstanceInfo değeri ilk yollandıktan sonra, her 30 saniyede bir tekrar yollanır.

  - eureka.server.responseCacheUpdateIntervalMs = 30s

    server her 30 saniyede bir servis listesini yeniler. bu liste yenilenmeden, kaydolan servisler bir sonraki yenileme ile ancak dışarıdan erişilebilir hale gelecektir.

    "/eureka/apps" REST isteği listesinde, yenileme süresi geçmediyse, o arada register olan servisler gözükmeyecektir.

    dahsboard ise yenilemeyi beklememektedir. direk güncel bilgiyi döndürür.

    "/eureka/apps/appName/instanceId" REST isteği yenilemeyi beklemez. direk güncel bilgiyi döndürür.

  - Ribbon.ServerListRefreshInterval = 30s

    bu değer Eureka modülüne ait değil. Ribbon load balancer'dır ve Eureka server'dan servis listesini çeker. bu süre zarfından güncellenmiş servis var ise, onları ilgili node'umuz göremeyecektir.

  - eureka.server.numberRegistrySyncRetries = 5, Eureka.server.getWaitTimeInMsWhenSyncEmpty = 5m

    Eureka server peer-aware mode'da çalışabilir. yani diğer server'lar ile haberleşerek çalışabilir. bu da verimliliği arttırır. server bu mode'da iken, 5 kere diğer sunuculardan servis listesini çekmeye çalışır. 5 kere başarısız olursa, 5 dakika boyunca client'ların kendisinden bilgi almasına izin vermez.

  - eureka.server.enableSelfPreservation = true, eureka.instance.registry.expectedNumberOfRenewsPerMin = 4, eureka.server.renewalPercentThreshold = 0.85, eureka.server.renewalThresholdUpdateIntervalMs = 15m

    Eureka server'a 2 client bağlı olsun. her biri 30 saniyede 1 heartbeat atsın. bu durumda dakikada 4 adet istek gelmeli. bu değeri expectedNumberOfRenewsPerMin ile belirtiyoruz. 4 * 0.85 = 3.4. eğer dakikada 3 veya 4'ten daha az istek gelirse ve bu 15 dakika boyunca sürerse, Eureka server kendini enableSelfPreservation'a alır. bu mode'a girdiği zaman artık expire eden instance'ları listesinden silmez.

# config server

tüm diğer servislerin bir micro-servisten config'leri (property'leri) okuması gerekir. bu micro-sunucuya "config server" adı verilir.

# Zuul

bir microservis olarak çalışır. bu sunucuya "gateway server" yada "edge server" denilmektedir. "Zuul proxy" buna örnek bir uygulamadır.

bu servise register olan sunucuların API'lerini Zuul dışarıya kendisi üzerinden açmaktadır. örnek: user-service'imizin /url1 path'li bir metotu olsun. money-service'imizinde /url1 path'li bir metotu olsun. bunları dışarıdan şu şekilde çağrılmalarını sağlamaktadır. bu iki servis ayrı porttan çalışmaktadır. bu iki API'yi de tek bir porttan açmak istediğimizde Zuul proxy üzerinden açarız.

```yml
Zuul:

  routes:

    service1:

      path: /user-service/**

      url: http://localhost:8081/user-service/

    service2:

      path: /money-service/**

      url: http://localhost:8082/money-service/
```

yukarıdaki config şunu diyor: path'e gelen istekler (örnek: http://zuul_proxy_ip:zuul_port/zuul_proxy_service/money-service/url1 ) URL'ye yönlendirilsin ( örnek: http://localhost:8082/money_service/url1 )

Zuul-proxy'nin filter özelliği mevcut. istek önce ZuulServlet'e geliyor. Zuul servlet sırası ile bizim filtrelerimizi işletiyor. filtrelerimiz 3 çeşit:

- prefilter: istek yönlendirilmeden önce

- routing filter: istek burada henüz yönlendirilmedi, fakat httpRequest objesi burada hazırlanabiliyor ve request burada yapılıyor. request'in dönüşü de ilk bu sınıfta karşılanabiliyor.

- postfilter: istek geldikten sonra çalıştırılıyor

- error filter: herhangi bir aşamada exception alındığında bu filter devreye giriyor

basit filter örneği:

```java
public class QueryParamPreFilter extends ZuulFilter {

@Override

public int filterOrder() {

    return PRE_DECORATION_FILTER_ORDER - 1; // run before PreDecoration

}

@Override

public String filterType() {

    return PRE_TYPE;

}

@Override

public boolean shouldFilter() {

    RequestContext ctx = RequestContext.getCurrentContext();

    return !ctx.containsKey(FORWARD_TO_KEY) // a filter has already forwarded

           && !ctx.containsKey(SERVICE_ID_KEY); // a filter has already determined serviceId
}

@Override
public Object run() {

    RequestContext ctx = RequestContext.getCurrentContext();

    HttpServletRequest request = ctx.getRequest();

    if (request.getParameter("sample") != null) {

         // put the serviceId in `RequestContext`
         ctx.put(SERVICE_ID_KEY, request.getParameter("foo"));

    }
    return null;
}

}
```

Zuul proxy'yi internal microservislerimiz kendi arasında haberleşirken kullanmayız. Best practice olarak microservisler internal haberleştiğinde discovery'den haberleşirler. discovery'ye erişilmek istenen servis ismi verilir, discovery bize bir IP/port atar, o IP/port'a istek atarız. Zuul üzerinden de bu iş yapılabilir fakat best practice değildir.

# Circuit Braker (or devre kesici)

bu kütüphane sayesinde hata fırlatan veya cevap vermeyen (timeout'a düşen) metotlar için backup metotlar hazırlanabilir. Spring boot ile de entegrasyonu olan ama bağımsız da çalışabilen Hystrix buna bir örnek araçtır.

Hystrix örnek kod:

aynı sınıf içinde aşağıdaki iki metot olmalıdır. eğer fallbackMethod yok ise; Hystrix exception fırlatır. zaten bu exception'u fırlatacaktı diye düşünebiliriz. fakat fallback yapması ve timeout'ları da desteklemesi Hystrix'in avantajıdır.

```java
@HystrixCommand(fallbackMethod = "noBooks", ignoreExceptions = { NullPointerException.class })

public String fetchBooks(int a) {

    return restTemplate.get(uri, String.class);
}

public String noBooks(int a) { //aynı değeri buraya atıyor

    return "There is no book available now";
}
```

yukarıdaki fetchBooks metotu için timeout set etmek isteyelim. timeout ayarını bu projenin application.yml'sine eklememiz yeterlidir. bu projeyi restart ettiğimizde ayar devreye girmiş olacaktır.

# Ribbon

client side load balancer görevi görüyor. Ribbon REST-template yada Feign-client gibi hazır Spring modülleri ile dışarıdaki sunuculara istek yaptığımız zaman, ilgili sunuculara düzenli /testUrl gibi bir path üzerinden sürekli istek yapar. bu istek eğer 200 dönmezse o sunucuyu down olarak varsayar ve bizi diğer sunuculara yönlendirir. /testUrl path'i, timeout, gibi parametreler override/configure edilebilir.

Ribbon kütüphanesi Ribbon işlemi yapılacak client'a eklenmelidir. örnek bir config (yml) içeriğimiz (client uygulamada olmalı bu config'ler):

```yml
account-service: #this is service name

  Ribbon:

    Eureka:

      enabled: false

    listOfServers: localhost:8090,localhost:9092,localhost:9999

    ServerListRefreshInterval: 15000
```

Yukarıdaki config'de Eureka disabled edildiği için listOfServers'ı elle verdik. eğer Eureka enabled olsaydı, listOfServers otomatik algılanıyor olacaktı.

# Feign Client

feign Türkçe kelime anlamı: uydurmak

REST istekleri için kolayca client oluşturmamız sağlayan modül. REST server'daki kodların aynısını (body kısmı hariç) Feign-client annotation'lu arayüze kopyalanır. daha sonra REST istek yapılması istenen kod satırında şu şekilde kullanmak yeterli olacak:

```java
MyFeignClientInterface.doPostRequest("Hello");
```

örnek kod:

```java
@FeignClient(name = "account-service")
public interface CHAccountServiceClient {

  @RequestMapping(value = "/accounts/v1/transactions", method = RequestMethod.POST)

  public ServiceResponse<String> transaction(@RequestBody RequestDTO requestDTO);
}
```

# artifact ID
Feign client ilk sürümünden itibaren (source-id: 328) reposunda geliştiriliyor.

Eski paket ismi __com.netflix.feign:feign-core__'du. kaynak: (source-id: 329)

9.0.0 sürümü ile __io.github.openfeign:feign-core__ oldu. kaynak: (source-id: 330)

__org.springframework.cloud:Spring-cloud-starter-openfeign__ jar'ı Spring cloud'un, Openfeign'i depend ederek üzerine ek geliştirmeler ile sunduğu pakettir. kaynak: (source-id: 331) Bu kaynakta bu yazmaktadır: "Feign is a declarative web service client. It makes writing web service clients easier. To use Feign create an interface and annotate it. It has pluggable annotation support including Feign annotations and JAX-RS annotations. Feign also supports pluggable encoders and decoders. Spring Cloud adds support for Spring MVC annotations and for using the same HttpMessageConverters used by default in Spring Web. Spring Cloud integrates Ribbon and Eureka to provide a load balanced HTTP client when using Feign."

Zira __org.springframework.cloud:Spring-cloud-starter-openfeign__ source code içerisinde de Openfeign dependency'si görülmektedir. kaynak: (source-id: 332)

# Ribbon entegrasyonu
Feign client'ın Ribbon ile entegrasyonu vardır. Eğer @FeignClient(name = "account-service") yazılırsa, "account-service"'in adresi Ribbon'dan getirilecektir. eğer Ribbon kullanılması istenmiyorsa, direk URL verilmelidir:  @FeignClient(name = "http://mysite.com/rest/")

# test
Yukarıdaki CHAccountServiceClient Feign client bir arayüzdür. direk olarak autowired edilir ve implementasyonunu yaratmayız. bu sebeple test, prod, local gibi ortamlar (Spring profiller) için farklı Feign Inject edemeyeceğiz. çünkü profiller runtime sırasında implementasyon arar. Bunun için şöyle bir yol izleyebiliriz:

- Test kodlarımızda farklı URL'li Feign client yaparız. Mockito ile asıl Feign client'ımızdaki her metotu ikinci yazdığımız bu feignClient'a yönlendiririz.

- FeignFactory diye bir interface yaparız. 2 adet te bunun implementasyonu: FeignFactoryProdImpl ve FeignFactoryDevImpl.

```java
@Component

@Profile("prod")

class FeignFactoryProdImpl implements FeignFactory{

  @Autowired
  ProdFeign Feign;
}
```

Benzeri FeignFactoryDevImpl içinde olacak. Artık asıl Feignclient'ı kullanacağız sınıfa @autowired ile FeignFactory'yi inject edeceğiz. Prod profilindeyken runtime'de ProdFeign gelecektir.

Feign client builder'ı ile hem encoder'larımızı değiştirebiliriz hemde diğer client'ları wrap ettirip kullanabiliriz. örnek:

```java
GsonCodec codec = new GsonCodec();

GitHub github = Feign.builder()
                      .encoder(new GsonEncoder()) // buraya JacksonEncoder da gelebilirdi
                      .decoder(new GsonDecoder())
                      .target(GitHub.class, "https://API.github.com");
```

```java
GitHub github = Feign.builder()
                     .client(new OkHttpClient())
                     .target(GitHub.class, "https://API.github.com");
```

# Spring Cloud Sleuth

Sleuth Türkçe kelime anlamı: dedektif.

Sleuth, Spring'in cloud'un bir alt projesidir. Sleuth Zipkin gibi takip sistemlerini kapsar.

# Spring Cloud Netflix

Spring'in cloud'un bir alt projesidir. Service Discovery (Eureka), Circuit Breaker (Hystrix), Intelligent Routing (Zuul) and Client Side Load Balancing (Ribbon) projelerini içerir.

# OpenTracing vs OpenCensus vs OpenTelemetry
OpenTracing ve OpenCensus projeleri, OpenTelemetry isimli projede birleşti.

OpenTelemetry, Zipkin'e alternatif bir framework'tür. OpenTelemetry sunucu tarafı isterse Zipkin gibi diğer tool'lara da trace bilgilerini yollayabiliyor.

# Zipkin
servislerimizin haberleşmesini ve aralarında geçen zamanı trace (takip) edebilmemizi sağlayan yazılımdır.

Spring ile de entegrasyonu olan kendi başına çalışan bir microservistir. bu microservis yapılan tüm istekleri takip eder. bir transaction'ı baştan sona takip eder ve bir web-GUI ile bunları analiz etmemizi sağlar. log atarken bu transaction-ID'leri de basabilmekteyiz.

ortamdaki her microservis, her request aldığında ve response attığında, Zipkine bilgi vermelidir. bunu yapmaları için her microservice'de bir Zipkini destekleyen bir kütüphane bulunmalıdır. bu kütüphanelere  "__instrumentation library__" deniyor. her microservis'e "reporter" veya "instrumented app" deniliyor. sadece microservisler değil, aynı zamanda isteği yapan client'ta bu sürecin içine dahil edilebilir. client tarafta isteği yolladığı anda bu bilgiyi Zipkin sunucusuna bildirebilir.

Zipkin, her microservisten bilgi alabilmek için onlara bir API sunar. Bu API'ye "collector" denir. Zipkin, bu API'den topladığı bilgileri bir DB'ye kaydeder. Daha sonrasında, Zipkin bu DB'den rahat okuyabilmemiz için web GUI sunar. Aynı zamanda Zipkin üçüncü parti uygulamalarımızın da trace bilgilerine direk DB'den değilde, apı aracılığı i̇le eri̇şmesi̇ i̇çi̇n "Zipkin Query Service" (API) sunar.

Spring cloud'un Sleuth için dependency'leri:

- Spring-cloud-starter-zipkin -> BOM
- Spring-cloud-sleuth-stream -> deprecated oldu
- Spring-cloud-sleuth-zipkin -> Zipkin server'a her span için bilgi yollayan kütüphane
- Spring-cloud-sleuth -> Zipkin'e bildirim yapmaz. fakat trace ID'leri header'da taşır ve programatik olarak kodumuzdan görebilmemizi sağlar. kaynak: (source-id: 333)

Zipkin'de kullanılan terimler:

#### Annotation

sadece isteğin hangi durumda olduğunu içerir. örnek:

- cs: Client Sent. The client has made a request. This annotation indicates the start of the span.

- sr: Server Received: The server side got the request and started processing it. Subtracting the cs timestamp from this timestamp reveals the network latency.

- ss: Server Sent. Annotated upon completion of request processing (when the response got sent back to the client). Subtracting the sr timestamp from this timestamp reveals the time needed by the server side to process the request.

- cr: Client Received. Signifies the end of the span. The client has successfully received the response from the server side. Subtracting the cs timestamp from this timestamp reveals the whole time needed by the client to receive the response from the server.

kaynak: (source-id: 334)

#### BinaryAnnotation

istek yapılan URL bilgisi gibi birçok bilgi içerir.

#### span
Türkçe kelime anlamı: karış.

bir iş akışı süreci birçok iş akışı parçacığından oluşabilir. bu parçacıkların ne olacağı tamamen bize kalmış.

Spring kütüphaneleri default'ta, her microservis arasındaki geçişler birer span olarak kabul eder.

#### trace

request'in client'tan çıkması ve client'ın request'i aldım sinyalini yollayana kadar geçen akış için kullanılan terim. trace-ID tüm istek için baştan sona tekildir.

#### parent-ID
Buna parent-span-id'de denildiği olur.

isteğin hangi span-ID'den sonra yapıldığı bilgisidir.

#### Flags
bayrak string'i akışın olduğu yönde bir sonraki sunucuya yollanır.

#### b3 propagation
propagation Türkçe kelime anlamı: yayılma

Her microservis diğer microservice istek attığında header'da bazı metadata'ları geçmek zorundadır. Yani hem Zipkine hemde normal request'inde bazı trace bilgilerini geçmek zorundadır çünkü istek attığı servis, parent-id gibi bilgileri ancak bu şekilde bilebilir.

Zipkin, sunucular arası haberleşmedeki geçilen bilgilere (header isimlerine) karışmaz. bu sebeple 'b3' isimli bir standart belirlenmiştir. bu standartlarda header isimleri X-B3- prefix'i ile başlar ve yollanması gereken ID uzunlukları bellidir.

# Spring cloud version history
Spring cloud Spring'in bir alt projesidir ve altında birçok alt proje barındırır. Bu aile altında olan projeler daha çok distributed sistemlerde kullanılan dependency'leri barındırır (örnekler: service-discovery, circiut-braker...).

her alt projenin sürümü aslında diğerlerinden bağımsızdır. bu sebeple uygun sürümleri biraraya getirerek bir cloud projesi sürümü oluşturulur. bu projenin sürümü, numara ile gitmez. isim ile gider. isim sırası alfabetiktir. isimler Londra'da bulunan metro istasyonlarının isimleridir.

örnek: 1. sürüm "Angel", 2 "Brixton" şeklinde gider...

Yani "Angel" cloud sub-projeleri için bir sürüm kümesidir.

örneğin Feign-client 4üncü sürümünde ve Eureka 3'üncü sürümde olabilir (sayıları uydurdum).

Eğer "Angel"' kritik ve ufak bir güncelleme gerekiyor ise SR suffix'ini ("service releases") ve sürümü alıyor. örnek: Angel.SR1, Angel.SR2...

| code name         | release date |
|-------------------|--------------|
| Greenwich.RELEASE | Jan, 2019    |
| Greenwich.SR2     | Jun, 2019    |
| Greenwich.SR1     | Mar, 2019    |
| Finchley.RELEASE  | Jun, 2018    |
| Finchley.SR4      | Jun, 2019    |
| Finchley.SR3      | Feb, 2019    |
| Finchley.SR2      | Oct, 2018    |
| Finchley.SR1      | Aug, 2018    |
| Edgware.RELEASE   | Nov, 2017    |
| Edgware.SR6       | May, 2019    |
| Edgware.SR5       | Oct, 2018    |
| Edgware.SR4       | Jul, 2018    |
| Edgware.SR3       | Mar, 2018    |
| Edgware.SR2       | Feb, 2018    |
| Edgware.SR1       | Jan, 2018    |
| Dalston.RELEASE   | Apr, 2017    |
| Dalston.SR5       | Dec, 2017    |
| Dalston.SR4       | Oct, 2017    |
| Dalston.SR3       | Aug, 2017    |
| Dalston.SR2       | Jul, 2017    |
| Dalston.SR1       | May, 2017    |
| Camden.RELEASE    | Sep, 2016    |
| Camden.SR7        | May, 2017    |
| Camden.SR6        | Mar, 2017    |
| Camden.SR5        | Feb, 2017    |
| Camden.SR4        | Jan, 2017    |
| Camden.SR3        | Nov, 2016    |
| Camden.SR2        | Nov, 2016    |
| Camden.SR1        | Oct, 2016    |
| Brixton.RELEASE   | May, 2016    |
| Brixton.SR7       | Nov, 2016    |
| Brixton.SR6       | Sep, 2016    |
| Brixton.SR5       | Aug, 2016    |
| Brixton.SR4       | Jul, 2016    |
| Brixton.SR3       | Jul, 2016    |
| Brixton.SR2       | Jul, 2016    |
| Brixton.SR1       | Jun, 2016    |
| Angel.SR6         | Jan, 2016    |
| Angel.SR5         | Jan, 2016    |

2020 yılı ile Calendar Versioning'e geçildi.

# Spring Cloud Kubernetes
Spring cloud ile aynı arayüzlerin Kubernetes için implementasyonları içeren kütüphaneler/framework'lerdir. ama Spring cloud uygulamalarının Kubernetes ortamında Kubernetes özelliklerini kullanarak çalışmasıdır. kaynak: (source-id: 335)

bu kütüphanelerin sundukları:

- discovery server Eureka kullanmak yerine, servis listesi Kubernetes'in API'lerinden çekilmesini sağlayabilir (Spring-cloud-starter-Kubernetes)
- config'leri config server'dan çekmek yerine Kubernetes ortamındaki ConfigMap'ten (source-id: 438) veya Secret'lardan (source-id: 439) çekebilir (Spring-cloud-starter-Kubernetes-config)
- Ribbon load balancer'ın Kubernetes servisleri aracılığı ile servisin ayakta olup olmadığını kontrol ederek yapabilmesi sağlanabilir (Spring-cloud-starter-Kubernetes-Ribbon)

not: Spring-cloud-starter-kubernetes-all JAR'ı tüm yukarıdaki dependency'leri barındırıyor.

# Spring Cloud Stream
Message-broker destekleyen (Kafka, RabbitMQ...) altyapılar için orta API. Eskiden her message-broker için ayrı API vardı. Şimdi hepsi tek çatı altında toplandı.

Sadece "Spring for Apache Kafka" kütüphanesi spring-boot'a bağımlı değildir. Fakat "Spring Cloud Stream" spring-boot'a bağımlı.

"Spring Cloud Stream" tek başına bir işe yaramaz. RUntime'da bir implementasyn olması gerekir. Örneğin Kafka için 2 farklı implementasyon var:

- spring-cloud-stream-binder-kafka (Kafka Streams API ile uyumsuz)
- spring-cloud-stream-binder-kafka-streams (Kafka Streams API ile uyumlu)

