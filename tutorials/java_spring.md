############################

############################
# JAVA SPRING
############################

############################

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Proxy
Her spring-bean, ayrı bir proxy sınıfı tarafından wrap edilir.

```java
AccountService accountService = (AccountService) applicationContext.getBean(AccountService.class);
String accountServiceClassName = accountService.getClass().getName();
logger.info(accountServiceClassName);
```

The output will be:

```
INFO : transaction.TransactionProxyTest - $Proxy13
```

Örneğin @Cacheable, @Transactional annotation'ları bu proxy bean'indeki kodlarda değişiklik yapar. Dolayısı ile; bir metod, aynı spring-bean'indeki bir metodu çağırdığında proxy üzeirnden çağırmaz. Bu sebeplede bu annotation'lar tekrar devreye girmez.

# Technology for Proxy
Java'da 2 proxy yapabilmek için farklı teknoloji var:

- JDK-based dynamic proxy
  
JDK içinde gömülü gelir. Programatik Proxy yapabilmemizi sağlar:

```java
// Example:
// We proxy the java.util.Map class.

Map proxyInstance1 = (Map) Proxy.newProxyInstance(
				  getClassLoader(), 
				  new Class[] { Map.class }, 
				  new CustomHandler());

proxyInstance1.put("key1", "value1");
```

The handler class for above example:

```java
public class CustomHandler implements InvocationHandler {

    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
    
        print("Invoked method: {}", method.getName());

        return 1; // we return fake variable
    }
}
```

- Dynamic proxy based on Cglib

Projenin özel ismini "Code Generation Library" den türetmişlerdir.

Runtime'da programatik olarak JVM'in anladığı bytecode oluşturmaya yaramaktadır.

# Spring ne kullanıyor 
JDK-based dynamic proxy eğer proxy yapılacak sınıfın interface'i varsa kullanılabiliyor. Bu sebeple spring kullanabildiği zaman JDK-based dynamic proxy kullanır.

Fakat eğer bir Spring Bean'in interface'i yok ise, o zaman Spring, Cglib ile proxy oluşturur. Çünkü Cglib için intteface zorunluluğu yoktur.

Spring Boot veya Spring'in yeni sürümlerinde bu durum değişmiş olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# dependency yönetimi
Maven-pom'da şu eklenirse:

```xml
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>Spring-boot-starter-parent</artifactId>
        <version>1.5.2.RELEASE</version>
</parent>
```

Spring'in 1.5.2 versiyonunun tüm dependency'leri artık tanımlanmış (eklenmemiş,sadece tanımlanmış!) olur. Artık aşağıdaki gibi bir kütüphane ekleyebiliriz:

```xml
<dependencies>
        <dependency>
             <groupId>org.springframework.boot</groupId>
             <artifactId>Spring-boot-starter-web</artifactId>
        </dependency>
</dependency>
```

Spring-boot-starter-web'in sürümünü belirtmeye gerek yok çünkü zaten parent'ta tanımlanmıştı.

Spring-boot-starter-web projesi default olarak embedded Tomcat kullanır. biz bunun yerine jetty kullanmak istersek aşağıdakini yazmamı yeterli olacaktır:

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>Spring-boot-starter-web</artifactId>
        <exclusions>
            <exclusion>
                <groupId>org.springframework.boot</groupId>
                <artifactId>Spring-boot-starter-tomcat</artifactId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>Spring-boot-starter-jetty</artifactId>
    </dependency>
</dependencies>
```

# Starters
Spring-boot-starter-XYZ formatında yazılırlar. XYZ uygulamamızın tipidir.

örneğin;

Spring-boot-starter-web; Spring-MVC ve Tomcat embedded getirir.

Spring-boot-starter-jersey; web'e bir alternatiftir. Spring-MVC yerine Apache Jersey kullanır.

Spring starter'lar, Spring-boot-dependencies'i kullanırlar. Spring-boot-dependencies sadece her paketin versiyonunu barındırıyor. oysa starter'lar birçok paketi bir arada barındırıyor. örneğin web dediğimizde Tomcat vs yüklenmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# org.springframework.web.filter.OncePerRequestFilter
Klasik filter ile aynı özellikleri içerir. Fakat bu filter, her request için en fazla 1 kere çalıştırılacağını garanti eder.

bir request'i başka bir servlet'e redirect edebiliriz. eğer redirect ettiğimiz servlet'in bir filter'ı var ise, o zaman o filter'larda devreye girecektir. işte böyle bir durumda; eğer OncePerRequestFilter'dan türemiş bir filter var ise, ve bu filter birden fazla servlet'in önünde ise, birden fazla çalışması durumu olmayacaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Spring-retry

```xml
<dependency>
    <groupId>org.springframework.retry</groupId>
    <artifactId>Spring-retry</artifactId>
    <version>1.1.5.RELEASE</version>
</dependency>

<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>Spring-aspects</artifactId>
</dependency>
```

```java
@Configuration
@EnableRetry
public class AppConfig { ... }
```

```java
@Service
public class MyService {

    @Retryable(
              // Spring will try same method if the method throws SQLException
              value = { SQLException.class },
              // Spring will try 2 times
              maxAttempts = 2,
              backoff = @Backoff(delay = 5000))
    void retryService(String sql) throws SQLException{
       // any code here
    }

    // Recover is optional. Spring will call this method if 2 attempt will not work above.
    @Recover
    void recover(SQLException e, String sql){
       // any code here
    }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Properties Class of Java
Java Map sınıfından türetilmiştir. Properties dosyalarına tekabül eden (bu mantıkta çalışmalar için) özel tasarlanmıştır.

# System Environments from Java

- Java kodları içerisinden sistemin environment değerleri (PATH, JAVA_HOME gibi) çağrılabilmektedir: System.getEnv();

- Benzer mantıkta Java programına yada JVM argümanlarına parametre geçilebilir: "java -DmyParam1=myValue1 -jar myapp.jar". Bu parametreleri kod içerisinden almak için System.getProperties("myParam1") kullanılır.

- program argümanları ise; main'e string args[] ile geçilen parametrelerdir.

# Spring framework Externalized Configuration
Spring uygulaması başlarken onlarca sınıftan data okur. okuma öncelik sırasına göre aşağıdaki gibidir:

- Devtools global settings properties in the $HOME/.config/Spring-boot folder when devtools is active.

- @TestPropertySource annotations on your tests.

- properties attribute on your tests. Available on @SpringBootTest and the test annotations for testing a particular slice of your application.

- Command line arguments.

- Properties from SPRING_APPLICATION_JSON (inline JSON embedded in an environment variable or system property).

- ServletConfig init parameters.

- ServletContext init parameters.

- JNDI attributes from java:comp/env.

- Java System properties (System.getProperties()).

- OS environment variables.

- A RandomValuePropertySource that has properties only in random.*.

- Profile-specific application properties outside of your packaged jar (application-{profile}.properties and YAML variants).

- Profile-specific application properties packaged inside your jar (application-{profile}.properties and YAML variants).

- Application properties outside of your packaged jar (application.properties and YAML variants).

- Application properties packaged inside your jar (application.properties and YAML variants).

- @PropertySource annotations on your @Configuration classes. Please note that such property sources are not added to the Environment until the application context is being refreshed. This is too late to configure certain properties such as logging.* and Spring.main.* which are read before refresh begins.

- Default properties (specified by setting SpringApplication.setDefaultProperties).

kaynak: (source-id: 361) "2. Externalized Configuration" başlığı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# SpringApplicationBuilder
Spring boot uygulamamız başlatılmadan önce ayarlar set etmemizi sağlar. kullanımı direk @SpringBootApplication atılan sınıfın içinde olmalıdır. örnek:

```java
@SpringBootApplication
public class WebApplication extends SpringBootServletInitializer {

    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {

        application = new SpringApplicationBuilder();

        application.parent(new Object[]{"classpath:file1.xml", "classpath:file2.xml"})

       .profiles("abc")

       .properties("key1:test1", "key2:test2")

      .showBanner(false)

      .logStartupInfo(true)

       .headless(true)

      .application()

      .run();

        return application;
    }

    public static void main(String[] args) throws Exception {

        SpringApplication.run(WebApplication.class, args);
    }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Spring MVC
Rest supportu bu modül içindedir. ModelAndView objesi de bu modül içindedir.

## ModelAndView
Rest Controller'ımızı normal yazıyor ve gelen request'e cevap olarak ModelAndView döndürüyoruz.

```java
@Controller
class RegistrationController {

  @RequestMapping(value = "/register", method = RequestMethod.GET)
  public ModelAndView showRegister() {
    ModelAndView mav = new ModelAndView("register");
    mav.addObject("user", new User("Ahmet"));
    return mav;
  }
}
```

ModelAndView içinde 'model' ve 'JSP dosya path'i bilgisini barındırır. Spring "ViewResolver" sınıfında belirlendiği ayarlarla JSP'yi bulmaya çalışır. Viewresolver config örneği;

```java
@Configuration
@EnableWebMvc
class WebConfig extends WebMvcConfigurerAdapter {

  @Bean
  public ViewResolver viewResolver() {
        InternalResourceViewResolver resolver = new InternalResourceViewResolver();
        resolver.setPrefix("/WEB-INF/views/");
        resolver.setSuffix(".jsp");
        resolver.setExposeContextBeansAsAttributes(true);
        return resolver;
  }

  @Override
  public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {

        configurer.enable();
  }
}
```

Viewresolver'da belirtilen View'ı (JSP'yi) model ile doldurup response olarak yolluyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Spring batch
temel olarak 3 işlevi yerine getirmek için geliştirilmiş altyapıdır:

- reading data
- processing data
- writing data

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# JavaEE dezavantajlar
- application server specific deployment descriptors

- lots of XML files even the most basic EJB - Güncel sürümlerde XML yerine annotation kullanılıyor (Spring bu durumu daha önceden çözdüğü için Spring projesi ortaya çıkmıştı. Güncel sürümlerde bu sebep kalktı.)

- sunucu ayağa kalkma süresi (çünkü tüm kütüphaneler sunucu içerisinde) - Güncel sürümlerde sunucular çok hızlandı Spring daha büyüdüğü için kısmen daha yavaşladı

- sunucuya bağımlılık

# Spring dezavantajlar

- app sever üzerinde çalıştırılırsa ticari destek alınamaz

- app server'sız çalıştırıldığında monitoring tool'ları genelde JavaEE sunucularına uyumlu olduğundan, monitoring işlemlerinde sıkıntılar yaşanabilir

not: Spring sadece servlet container'a ihtiyaç duyuyor. Oysa JavaEE bir spesifikasyon ve komple tüm kütüphaneler sunucu içerisinde.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# stereotype
kelime anlamı: klişe, basmakalıp.

bu terim JavaEE ve Spring'de kullanılan bir terimdir. Java core'a ait değil.

Anlamından da çıktığı gibi standartar (basmakalıp) olan tanımlara/pattern'leri tmesilen bu terim kullanılır. Örneğin Spring ve JavaEE'deki kullanımına bakarsak, genel DI ve programlama dünyasında kullanılan standart sınıfıları işaretlemek amaçlı atıldığını görürüz.

- JavaEE'de; bu bir annotation'dur: "javax.enterprise.inject.Stereotype". JavaEE tarafından yazılmış bir (custom) annotation'dur. bu annotation JavaEE'nin kendi yazdığı bazı annotation'ların içinde kullanılır. bu annotation'un bir işlevi yoktur. amaç sadece gruplandırmak (mark etmektir).

- Spring'de; bu bir annotation değildir. Aşağıdaki annotation'lar "org.springframework.stereotype" paketi altındadır. amaç (JavaEE'dekine benzer olarak) sadece gruplandırmaktır.

  - Component
  - Controller
  - Indexed
  - Repository
  - Service

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Spring framework

# SpringSource
Spring framework'ünü geliştiren firmanın eski ismi. Firma ismi daha sonra Spring oldu.

# Spring Tools (or STS or Spring Tools Suite)
Eclipse için Spring eklentisidir. Eclipse'e yüklü şekilde de direk indirilebiliyor.

# Spring modules

Spring birçok modül sunuyor. Bu modüller gruplandırılmış durumda ve her modül, farklı gruptan olsa da, başka bir modüle depend ediyor olabilir.

Aşağıdaki listede en üst seviyedeki başlıklar (örnek: "Data access & Integration", "Core Container") sadece gruplamak için yazılmıştır. bir artifact'ı temsil etmiyorlar. Oysa alt seviyedeki her madde birer artifact (jar) dosyasıdır ve Maven'a eklendiklerinde ihtiyacı olan tüm bağımlılıkları çeker.

- Data access & Integration
  - __spring-jdbc__
  - __spring-orm__
  - __spring-oxm__: Object/XML mapping implementations for JAXB and other library
  - __spring-tx__: transaction support
- Web & Remoting
  - __spring-web__: HTTP-Client + Servlets (does not include REST classes like: @Controller)
  - __spring-webMVC__ (unofficial name: __web-servlet__): içinde Spring-web'i barındırır. sadece REST supportu (örnek @Controller) buradadır, aynı zamanda ModelView class'ları da bu modüldedir.
  - __spring-websocket__
  - __spring-webMVC-portlet__ (unofficial name: __web-portlet__)
- Core Container
  - __spring-core__: IoC özelliği bu modülün içinde tanımlıdır.
  - __spring-beans__: BeanFactory özelliği bu modülün içinde tanımlıdır.
  - __spring-context__: ApplicationContext özelliği bu modülün içinde tanımlıdır.
  - __spring-expression (or spEL)__: "Expression Language" modülüdür. string üzerinde kelime işleme özellikleri barındırmaktadır. aynı zamanda Bean property'lerine variable atayabilmek için özel syntax kullanımını sağlıyor.
- Aspect
  - __spring-aop__: method-interceptors (Spring's own Aspect library)
  - __spring-aspects__: AspectJ ile entegrasyonu sağlamaktadır (alternative of Spring-aop)
- Others
  - __spring-test__: JUnit + TestNG support.
  - __spring-instrument__: support for class loader implementations

# Maven artifact-ID isimlendirmesi

Spring yukarıdaki modüller hariç de birçok özellik sunuyor. Yukarıda yazan modüller temel seviyede olduklarından;

- group-ID: org.springframework
- artifact-ID: Spring-* (örnek Spring-webMVC)

şeklinde dağıtılıyorlar. yukarıda olup "core container" grubu içerisinde olanlar:

- group-ID: org.springframework
- artifact-ID: org.springframework.* (örnek org.springframework.context)

şeklinde dağıtılıyor. Yukarıda olmayan modüller (örnek cloud, boot, batch) ise şu şekilde dağıtılıyor:

- group-ID: org.springframework.* (örnek: org.springframework.batch)
- artifact-ID: org.springframework.* (örnek Spring-batch-core)

Spring her paketi paralelde OSGI uyumlu şekilde sunar. O paketlerin isimleri farklıdır. OSGI uyumlu paketler Enterprise Bundle Repository (EBR) üzerinden dağıtılmaktadır. Örnek:

- group-ID: org.springframework
- artifact-ID (Maven central): Spring-core
- artifact-ID (Spring EBR): org.springframework.core

kaynak: (source-id: 362) "Spring modules on the Maven repository başlığı". yazarlar: "Rob Harrop", "Clarence Ho", kitap adı: "Pro Spring 3"

# Spring version naming

(Spring cloud versiyonları başka başlıkta anlatılıyor.)

- GA (or General availability): stable versiyon.

- RC (or Release candidate): beta versiyon.

- M (or Milestone build): nightly versiyon.

Yukarıdaki sürüm terimleri genel olarak tüm yazılım sürüm politikalarınca kullanılmaktadır. sadece Spring'e özgü terimler değildir.

# Spring version history

| version number | release year | compatible JDK version |
|----------------|--------------|------------------------|
| 0.9            | 2002         |                        |
| 1.0            | 2003         |                        |
| 2.0            | 2006         |                        |
| 3.0            | 2009         |                        |
| 4.0            | 2013         |                        |
| 4.3            |              | 6-8                    |
| 5.0            | 2017         | 8-10                   |
| 5.1            |              | 8-12                   |

# Spring initializr
https://start.Spring.io sitesinin altyapısı olan projedir. bu site ile web arayüzden Spring modülleri seçebiliyoruz, paket ismi vs form girişi yaptıktan sonra bizim için Gradle/Maven, Java/Groovy gibi hazır boş Spring projesi oluşturmaktadır. bu projeyi anında download etmemizi sağlıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# application.properties

Spring başladığında default olarak proje içindeki application.properties dosyasını okur. farklı dosya verilmek istenirse;

> -Dspring.profiles.active=dev -Dspring.config.location=file:C:/application.yml

profillerde dosya içinde --- (üç çizgi) ile ayrışmalıdır:

```yml
Spring:
  profiles: dev

prop1: val1
prop2: val2

---

Spring:
  profiles: prod

prop1: val1
prop2: val2

```

3 çizgi YML formatının bir özelliğidir. kaynak: (source-id: 443) ("4.3.2. Document Boundary Markers" başlığı)

# bootstrap.yml vs application.yml
bootstrap dosyası microservisler ile karşımıza çıktı. config server'dan her servis configlerini aldığı için, Spring boot uygulaması ayağa kalkmadan bazı property'leri tutması gerekli: örneğin; config server'ın portu ve IP bilgisi. microservis; config'den aldığı port numarası bilgisi ile kendisini hizmete açacaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Spring Boot
sağladığı en temel yetenek: öntanımlı konfigürasyonlardır. diğer tüm özellikler buradan türer. örnekler:
- Konfigürasyon yapmadan Embeed servlet container getiriyor. böylece app server'a atma ihtiyacı duyulmuyor.
- projeye lib olarak database ekledik. eğer database ayarları yapılmadıysa in-memory database açıyor. bunu @EnableAutoConfiguration ile yapıyoruz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Spring boot version history
| version | released year | change log                                                                                                                                                                   |
|---------|---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1.1     | June 2014     | improved templating support, gemfire support, auto configuration for ElasticSearch and Apache solr.                                                                          |
| 1.2     | March 2015    | upgrade to servlet 3.1/Tomcat 8/jetty 9, Spring 4.1 upgrade, support for banner/jms/SpringBootApplication annotation.                                                        |
| 1.3     | December 2016 | Spring 4.2 upgrade, new Spring-boot-devtools, auto configuration for caching technologies(Ehcache, Hazelcast, Redis, Guava and|infinispan) and fully executable JAR support. |
| 1.4     | January 2017  | Spring 4.3 upgrade, Couchbase/Neo4j support, analysis of startup failures and RestTemplateBuilder.                                                                           |
| 1.5     | February 2017 | support for Kafka/LDAP, third party library upgrades, deprecation of CRaSH support and Actuator loggers endpoint to modify|application log levels on the fly.                |
| 2.0     |               |                                                                                                                                                                              |
| 2.1     |               |                                                                                                                                                                              |
| 2.2     |               |                                                                                                                                                                              |

Her sürüm için ayrı ayrı, changelog'lar özet olarak burada açıklanmaktadır: https://github.com/Spring-projects/Spring-boot/wiki/Spring-Boot-2.2-Release-Notes

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# @SpringBootApplication
bu annotation şu 3'ünü içeriyor:

- @Configuration (başka başlıkta anlatılıyor)

- @EnableAutoConfiguration (örneğin projeye lib olarak database ekledik. eğer database ayarları yapılmadıysa in-memory database açıyor. yukarıdaki ikinci madde.)

- @ComponentScan (eğer parantez içinde parametre vermezsek, sadece bulunduğu sınıfın paketindeki ve alt paketlerdeki sınıfları Spring ayarlarına kabul ediyor.)

# AutoConfiguration
tüm oto configuration sınıfları tek bir jar ile dağıtılır: Spring-boot-autoconfigure-{version}.jar

Bu sınıflar condition içerir. eğer biz önceden config Bean'lerimizi oluşturmadıysak, condition'lar sayesinde AutoConfiguration Bean'leri load edilecektir.

örnek: org.springframework.boot.autoconfigure.JDBC.DataSourceAutoConfiguration sınıfında

```java
@Configuration

// SQL lib are in classpath
@ConditionalOnClass({ DataSource.class, EmbeddedDatabaseType.class })

// default properties will be bind, if datasource properties are written in application.properties
@EnableConfigurationProperties(DataSourceProperties.class)

// other depended configurations
@Import({ Registrar.class, DataSourcePoolMetadataProvidersConfiguration.class })

public class DataSourceAutoConfiguration
{
```

some other auto config classes:

- org.springframework.boot.autoconfigure.web.DispatcherServletAutoConfiguration
- org.springframework.boot.autoconfigure.orm.jpa.HibernateJpaAutoConfiguration
- org.springframework.boot.autoconfigure.data.jpa.JpaRepositoriesAutoConfiguration
- org.springframework.boot.autoconfigure.jackson.JacksonAutoConfiguration
- org.springframework.boot.autoconfigure.mongo.MongoAutoConfiguration

Eğer bean tanımı olmazsa aşağıdan NoSuchBeanDefinitionException hatası alırız:

```java
applicationContext.getBean(MongoTemplate.class);
```

Aslında MongoTemplate bşr class fakat @Component içermiyor. Bu sebeple onun class-filed'larını uygun şekilde set eden ve yeni instance'ını, oluşturan Config class'ları olmalı. İşte MongoAutoConfiguration bunu yapan bir class.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Aware
Türkçe kelime anamı: farkında, tetikte, haberdar

ServletContextAware gibi birçok aware arayüzü vardır. ServletContextAware örnek olarak alınırsa, bu sınıftan implemente ettiğimiz her Bean ServletContext'i inject etmesine gerek kalmaz. örnek:

```java
@RestController("/mycontroller")
public MyController implements ServletContextAware {

    private ServletContext context;

    @Override
    public void setServletContext(ServletContext context) {

        //setServletContext ServletContextAware arayüzünün bir metotu. "context" private değerine istediğimiz ismi verebiliriz.

        this.context = context;
    }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Webjars

Maven'a aşağıdaki eklendiğinde:

```xml
<dependency>
    <groupId>org.webjars</groupId>
    <artifactId>jquery</artifactId>
    <version>3.1.1</version>
</dependency>
```

HTML'lerimizden get isteği ile script'i çekebilmemizi sağlar:

```html
<script src="/webjars/jquery/3.1.1/jquery.min.js"></script>
```

Konfigürasyonu bu şekildedir:

```java
@Configuration
@EnableWebMvc
public class WebConfig implements WebMvcConfigurer {

    @Override
    public void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry
          .addResourceHandler("/webjars/**")
          .addResourceLocations("/webjars/");
    }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
