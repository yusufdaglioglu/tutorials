############################

############################
# JAVA SPRING AND JAVAEE
############################

############################

# front controller
genel bir server uygulama pattern'idir. bir server'a gelen istekleri yakalayan ilk sınıftır/servlet'tir. Bu servlet'in görevi ilgili yere bu isteği taşımaktadır.

# DispatcherServlet
Dispatch Türkçe kelime anlamı: sevk etmek. yazılım dünyasında metota/fonksiyona mesaj(parametre) yollama(sevk etme) anlamında kullanılır.

SpringMVC'de front controller'ın ismidir.

# FacesServlet
JSF için front controller'ın ismidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Tomcat variants

Tomcat: servlets + JSP

Tomee: Tomcat + JSF + JPA + CDI

Tomee-JAX-RS: Tomee + JAX-RS

Tomee+: Tomee-JAX-RS + Jax-WS

detailed comparison:

|                                                      | Tomcat | TomEE | TomEE JAX-RS (~ Microprofile) | TomEE+ | TomEE PluME | OpenEJB |
|------------------------------------------------------|--------|-------|-------------------------------|--------|-------------|---------|
| Java Servlets                                        | +      | +     | +                             | +      | +           |         |
| Java ServerPages (JSP)                               | +      | +     | +                             | +      | +           |         |
| Java ServerFaces (JSF)                               |        | +     | +                             | +      | +           |         |
| Java Transaction API (JTA)                           |        | +     | +                             | +      | +           | +       |
| Java Persistence API (JPA)                           |        | +     | +                             | +      | +           | +       |
| Java Contexts and Dependency Injection (CDI)         |        | +     | +                             | +      | +           | +       |
| Java Authentication and Authorization Service (JAAS) |        | +     | +                             | +      | +           | +       |
| Java Authorization Contract for Containers (JACC)    |        | +     | +                             | +      | +           | +       |
| JavaMail API                                         |        | +     | +                             | +      | +           | +       |
| Bean Validation                                      |        | +     | +                             | +      | +           | +       |
| Enterprise JavaBeans                                 |        | +     | +                             | +      | +           | +       |
| Java API for RESTful Web Services (JAX-RS)           |        |       | +                             | +      | +           | +       |
| Java API for XML Web Services (JAX-WS)               |        |       |                               | +      | +           | +       |
| Java EE Connector Architecture                       |        |       |                               | +      | +           | +       |
| Java Messaging Service (JMS)                         |        |       |                               | +      | +           | +       |
| EclipseLink                                          |        |       |                               |        | +           |         |
| Mojarra                                              |        |       |                               |        | +           |         |

full comparison: (source-id: 421)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Tomcat version history

| version | JSP | WebSocket spec | servlet spec | Java version                         | release date | Latest release | Latest version release date |
|---------|-----|----------------|--------------|--------------------------------------|--------------|----------------|-----------------------------|
| 2.0     |     |                |              |                                      | 1998         |                |                             |
| 3.0     |     |                |              |                                      | 1999         | 3.3.2          | 2004-03-09                  |
| 4.1     | 1.2 |                | 2.3          | 1.3 and later                        | 2002-09-06   | 4.1.40         | 2009-06-25                  |
| 5.0     | 2.0 |                | 2.4          |                                      | 2003-12-03   | 5.0.30         | 2004-08-30                  |
| 5.5     |     |                | 2.4          | 1.4 and later                        | 2004-11-10   | 5.5.36         | 2012-10-10                  |
| 6.0     | 2.1 |                | 2.5          | 5 and later                          | 2007-02-28   | 6.0.53         | 2017-04-07                  |
| 7.0     | 2.2 | 1.1            | 3.0          | 6 and later (WebSocket için min 7 ) | 2011-01-14   | 7.0.96         | 2019-07-29                  |
| 8.0     | 2.3 | 1.1            | 3.1          | 7 and later                          | 2014-06-25   | 8.0.53         | 2018-07-05                  |
| 8.5     |     | 1.1            | 3.1          | 7 and later                          | 2016-06-13   | 8.5.43         | 2019-07-09                  |
| 9.0     |     | 1.1            | 4.0          | 8 and later                          | 2018-01-18   | 9.0.22         | 2019-07-09                  |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# servlet version history

| Servlet API version | Released       | Specification    | Platform                                                  | Important Changes                                                           |
|---------------------|----------------|------------------|-----------------------------------------------------------|-----------------------------------------------------------------------------|
| Servlet 4.0         | Sep 2017       | JSR 369          | Java EE 8                                                 | HTTP/2                                                                      |
| Servlet 3.1         | May 2013       | JSR 340          | Java EE 7                                                 | Non-blocking I/O, HTTP protocol upgrade mechanism                           |
| Servlet 3.0         | December 2009  | JSR 315          | Java EE 6, Java SE 6                                      | Pluggability, Ease of development, Async Servlet, Security, File Uploading  |
| Servlet 2.5         | September 2005 | JSR 154          | Java EE 5, Java SE 5                                      | Requires Java SE 5, supports annotation                                     |
| Servlet 2.4         | November 2003  | JSR 154          | J2EE 1.4, J2SE 1.3                                        | web.xml uses XML Schema                                                     |
| Servlet 2.3         | August 2001    | JSR 53           | J2EE 1.3, J2SE 1.2                                        | Addition of Filter                                                          |
| Servlet 2.2         | August 1999    | JSR 902, JSR 903 | J2EE 1.2, J2SE 1.2                                        | Becomes part of J2EE, introduced independent web applications in .war files |
| Servlet 2.1         | November 1998  | 2.1a             |                                                           | First official specification, added RequestDispatcher, ServletContext       |
| Servlet 2.0         | December 1997  | N/A              | JDK 1.1                                                   | Part of April 1998 Java Servlet Development Kit 2.0                         |
| Servlet 1.0         | December 1996  | N/A              | Part of June 1997 Java Servlet Development Kit (JSDK) 1.0 |                                                                             |

# servlet 3.1 non-blocking IO

```java
public void doGet(request, response) {
    final AsyncContext asyncContext = request.startAsync();

    asyncContext.start(new Runnable() {
        @ Override
        public void run() {
            // do some work here which is on a new thread. that makes free the blocking HTTP thread.

            // burada aslında aşağıdaki gibi bir şey yapmazsak neredeyse hiçbir anlamı kalmaz
            asyncHttpCLient.sendPostRequest().
                                    than(response -> {
                                                       log(response);
                                                       response.getOutputStream().print(response);
                                                       asyncContext.complete();
                                    })
        }
    });
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Catalina
Tomcat'in servlet container'ın ismidir.

# Jasper
Tomcat'in JSP derleme motorudur. JSP derlenir ve bir servlet'e dönüştürülür.

# $CATALINA_HOME
Tomcat runtime dosyalarının bulunduğu dizindir.

# $CATALINA_BASE
birden fazla Tomcat instance'ı aynı anda çalıştırılabilir. her instance'ın bulunduğu config'ler CATALINA_BASE içinde olmalıdır.

eğer CATALINA_BASE set edilmemişse; CATALINA_HOME değeri ona set edilecektir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Eclipse IDE Tomcat config files
Eclipse Tomcat'i başlatırken config dosyaları için workspace içinde otomatik olarak "/Eclipse-workspace/Servers/Tomcat v6.0 Server at localhost-config" isminde bir dizin oluşturuyor. bu dizindeki config'ler Tomcat dizini içindeki config'leri override ediyor.

Tomcat start edilirken ona zaten tüm config parametreleri (config dosyalarının path'leri, örnek server.xml) komut satırından geçilebiliyor. Eclipse'te bu config dosyalarını, Tomcat binary'sine otomatik olarak parametre geçiyor. yani gerçek Tomcat dizinini override etmemiş oluyor. böylece tek bir Tomcat dizini birden fazla Eclipse workspace'i tarafından kullanılabiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Tomcat directory structure

- bin: executable files and "start Tomcat", "run as service" like scripts

- lib: jar available for all webapps

- logs

- work: sadece JSP dosyaları, servet'e çevrildiklerinde, burada o servlet'ler saklanır.

- conf: server.xml gibi config dosyalarının bulunduğu dizin

  - catalina.policy:

    yetkilendirme tanımları bu dosyadadır. örnek:

    permission java.util.PropertyPermission "java.version", "read";

  - catalina.properties

    paket(Java class paketleri) bazında ve jar bazında hangi projenin hangilerini okuyabileceğinin bilgisinin yer aldığı dosya.

  - server.xml

    sunucunun tüm ayarları buradan yapılabiliyor. detaylı anlatım aşağıda mevcut. bu dosyadan her war dosyasının içinde olamaz.

  - web.xml

    bu dosya her war içindeki web.xml parse edilmeden önce işletilir. daha sonra war içindeki web.xml okunur.

- webapps

  - docs: dökümantasyon içeren bir proje

  - examples: örnek JSP sayfaları ve kaynak kodlarını gösteren bir site

  - ROOT: context-path'i olmayan bir proje. burada sadece Tomcat'e hoşgeldiniz ve diğer Apache.org projelerine linkler verilmiş bir site bulunmaktadır.

  - manager: Tomcat üzerinde JVM memory detayları, yüklü olan projeleri context-path'leri gibi bir çok detay monitör edilebilir ve deploy/undeploy işlemleri yapılabilmesini sağlar.

  - host-manager: "Virtual Host" özelliğini monitör etmek için kullanılmaktadır. Virtual host birden fazla webapp klasörü yaratılabilmesini her her webapp içindeki projelerin birer domain-name'e karşılık gelmesini sağlamaktadır. Örneğin; Google.com ve Amazon.com'a yapılan istekler aynı Tomcat instance'ımıza gelecek ve fakat farklı webapp dizinlerindeki projelere yönlendirilebilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Tomcat Realm
realm Türkçe kelime anlamı: diyar, krallık, alan, alem

A Realm is a "database" of usernames and passwords that identify valid users of a web application. this database includes also roles of users.

Tomcat içinde belirlediğimiz uygulamalar, güvenlik katmanlarında isterlerse bu kullanıcıları kullanabilirler. Böylece tüm uygulamalar için ortak kaynak belirlemiş oluruz.

Tomcat içinde gelen "manager" uygulaması, varsayılan olarak Realm'daki user'lara göre kullanıcıların login olup arayüzde işlem yapmasına izin vermektedir.

Realm kaynağı kulanmak için org.Apache.catalina.Realm'dan türemiş bir implementasyona ihtiyacımız var. Tomcat içerisinde bunlar yüklü gelmektedir:

- JDBCRealm

JDBC jar'ı aracılığı ile veritabanına bağlanır. tabloları bizim oluşturmuş olmamız gereklidir.

/Tomcat/conf/server.xml'de buna benzer bir tanımlamayı eklememiz gereklidir:

```xml
<Realm
    className="org.Apache.catalina.realm.JDBCRealm"
    driverName="org.gjt.mm.mysql.Driver"
    connectionURL="JDBC:mysql://localhost/authority?user=dbuser&amp;password=dbpass"
    userTable="users" userNameCol="user_name" userCredCol="user_pass"
    userRoleTable="user_roles" roleNameCol="role_name"/>
```

veritabanında yapılan satır değişiklikleri Tomcat'e anında yansımaktadır. fakat bir kullanıcının rolü değişirse, yada kullanıcı kaldırılırsa, kullanıcı logout olana kadar session'ı kullanabilecektir.

eğer servlet'lerde biri protected bir kaynağa erişmek isterse Tomcat otomatik olarak org.Apache.catalina.realm.JDBCRealm.authenticate() metotunu çağıracaktır.

- DataSourceRealm

JNDI tanımı aracılığı ile veritabanına bağlanmaktadır.

```xml
<Realm
   className="org.Apache.catalina.realm.DataSourceRealm"
   dataSourceName="JDBC/authority"
   userTable="users" userNameCol="user_name" userCredCol="user_pass"
   userRoleTable="user_roles" roleNameCol="role_name"/>
```

- JNDIRealm

JNDI tanımı aracılığı ile LDAP sunucusundan kullanıcı bilgilerini çeker.

- UserDatabaseRealm

JNDI aracılığı ile XML'den kullanıcı bilgilerini okunduğu realm'dır. varsayılan olarak XML dosyası /tomcat/conf/tomcat-users.xml'dir. örnek tomcat-users.xml:

```xml
<tomcat-users>
  <user name="tomcat" password="tomcat" roles="tomcat" />
  <user name="role1"  password="tomcat" roles="role1"  />
  <user name="both"   password="tomcat" roles="tomcat,role1" />
</tomcat-users>
```

- MemoryRealm

DEMO için geliştirilmiş bir implementasyondur. UserDatabaseRealm ile aynıdır (aynı XML'i okur), tek eksisi XML'de yapılan değişiklikler restart edilene kadar algılanmaz.

- JAASRealm

istek yapan kullanıcıları "Java Authentication & Authorization Service (or JAAS)" ile onaylayan Realm'dır.

- CombinedRealm

birden fazla sub-real kullanabilmemize imkan tanıyan Realm'dır.

```xml
<Realm className="org.Apache.catalina.realm.CombinedRealm" >

   <Realm className="org.Apache.catalina.realm.UserDatabaseRealm"
          resourceName="UserDatabase"/>

   <Realm className="org.Apache.catalina.realm.DataSourceRealm"
          dataSourceName="JDBC/authority"
          userTable="users" userNameCol="user_name" userCredCol="user_pass"
          userRoleTable="user_roles" roleNameCol="role_name"/>
</Realm>
```

- LockOutRealm

CombinedRealm'dan türemiştir. kullanıcıları, fazla şifre denemesi isteği yaptıkları durumda onları engelleyebilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# context path

context; Tomcat'te bir war dosyasına denk geliyor (server.xml'den de anlaşılacağı gibi).

comntext path; URL'de host ve port'tan sonra gelen web projesinin ilk seviye URL'sidir.

Webapp altındaki her proje URL'den http://host/directoryName olarak çağrılırlar. tek istisna ROOT folder'ıdır. o direk http://host/ ile çağrılır. root içerisindeki bir web projesi, örnek /tomcat/webapps/ROOT/webapp1, http://host/webapp1 olarak çağrılır.

TOMCAT/WEBAPPS/demo#v1#myfeature/ projesi URL'den bu şekilde erişilir: http://host/demo/v1/myfeature context.

\## (çift diez) örnek: foo##2.war sürüm olarak algılanmaktadır. sürüm numaraları URL'ye (context path'e) yansıtılmaz.

/tomcat/conf/server.xml dosyası sadece Tomcat restart edildiğinde işlenir, bu sebeple context'lerin server.xml'e yazılmaması önerilir.

# context.xml

bu dosya server.xml içindeki host elementinin içine yazılan kısmı taşıyabilir. bir sistemde birden fazla context belirteci olabilir. aynı sistemde birden fazla belirteç yazılmış olabilir. bunlar birbiri ile çakıştıklarında öncelik sırası parantez içerisinde verilmiştir (önceliği yüksek olan 1 numara):

- (1) /tomcat/conf/context.xml

- (1) tomcat/conf/server.xml içindeki context elementi

- (2) /tomcat/conf/[Engine_name]/[Host_name]/war-name.xml

- (2) /tomcat/conf/context.xml

- (3) war içindeki /META-INF/context.xml

Tek istisna: tomcat/conf/server.xml içindeki context elementinde "override=true" yok ise; sistemin herhangi bir yerine yazdığımız context değeri algılanır.

# META-INF
Bu dizin war dosyalarının içindedir. war export edilirken, bu dizinin içindekiler tomcat/webapps/app1/ içerisine taşınır.

# server.xml

```xml
<Server>
  <Service name="MyService1">
    <Connector port="8443"/>
    <Connector port="8444"/>
    <Engine>
      <!-- default appBase: webapps -->
      <Host name="yourhostname" appBase="/webapps2">
        <!-- file1 path is: TOMCAT/webapps2/file1.war -->
        <Context path="/webapp1" docBase="file1.war" />
        <Context path="/webapp2"/>
      </Host>
    </Engine>
  </Service>
</Server>
```

- Görüldüğü üzere her bilgi __server__ içerisine yazılıyor.

- örnekte 1 adet service içerisinde birden fazla connector (bunlar sadece port'u dinleyip yönlendirme yapmaya yarıyor), webapp1 vs webapp2'ye yönlendirme yapıyor.

- 2 ayrı port tek bir engin'e yönlenmiş durumda. webapp1 ve webapp1 bir adet __engine__'e bağlanmış durumdalar.

- __engine__ burada __Catalina__'ya denk geliyor. Catalina ise bir __servlet container__'dır.

- Service, engine gibi gruplamalar log'lardan da takip edilebildiği için büyük kolaylık sağlamaktadır.

- 80'e gelen istekleri eğer client SSL bağlantısı kurma isteği yapıyorsa 443'e yönlendirebiliriz:

```xml
<Connector port="80" protocol="HTTP/1.1" connectionTimeout="20000" redirectPort="443" />
```

Bu connector tanımı yetmeyecektir. Çünkü son durumda 443'ün tanımı yoktur. 443'ün tanımını da yapmalıyız:

```xml
<Connector port="443"
           protocol="org.Apache.coyote.http11.Http11Protocol"
           SSLEnabled="true"
           scheme="https"
           secure="true"
           sslProtocol="TLS"
           keystoreFile="/path/to/kestorefile"
           keystorePass="my_keystore_password"/>
```

Bazı Connector parametreleri:

- address

  br sunucu birden fazla IP alıyor olabilir. bunlardan hangisinin o connector tarafından listening'de olacağını belirtir. default olarak tüm IP'ler dinlenir.

- maxPostSize

  post request'inin max boyutu. varsayılan 2 MB.

- scheme

  protokolün ismidir. URL'deki değer değildir. default http.

- secure

  Java kodumuzda request.isSecure() true/false dönüşü yapsın istiyorsak bu değeri true olarak set etmeliyiz.

- URIEncoding

  URL'nin encoding'idir. default ISO-8859-1.

- useBodyEncodingForURI

  eğer request'in header'ında "contentType" varsa, URIEncoding değerini hiçe sayar ve contentType'a göre URL'yi parse eder. default değeri false.

Yukarıda protocol ile class ismi (implementasyon) belirtmezsek Tomcat otomatik olarak bir implementasyon seçecektir. SSL standarttır. bu sebeple bu implementasyonlar SSL protokolünün işleyişinde bir değişiklik yapmıyorlar, bunlar sadece konfigürasyonları fark ettiriyor.

- JNDI

Direk \<server> içerisine bu tanımlama yapılabilir. bu şekilde tüm uygulamalar ismi ile JDNI'a erişebilir olur.

```xml
<GlobalNamingResources>
  <Resource name="UserDatabase" auth="Container"
            type="org.Apache.catalina.UserDatabase"
            description="the user informations"
            factory="org.Apache.catalina.users.MemoryUserDatabaseFactory"
            pathname="conf/tomcat-users.xml" />
</GlobalNamingResources>
```

Bu JNDI'ı kullanacak war dosyaları kendi web.XML'lerinde bunu belirtmek zorundalar:

```xml
<resource-ref>
    <description>
          database of Tomcat
    </description>
    <res-ref-name>
          UserDatabase
    </res-ref-name>
    <res-type>
          org.Apache.catalina.UserDatabase
    </res-type>
    <res-auth>
          SERVLET
    </res-auth>
</resource-ref>
```

Yukarıdaki res-aut "CONTAINER" da olabilirdi.

- Environment variables

Direk \<server> içerisine bu tanımlama yapılabilir. bu şekilde tüm uygulamalar için environment variable atamış oluruz.

```xml
<Environment name="simpleValue" type="java.lang.Integer" value="30"/>
```

- catalina alternatives

catalina dışında bir container kullanmak istersek:

```xml
<Service name="MyService1" className="com.my.class.Name" >
```

Eğer className verilmezse default olarak org.Apache.catalina.Service arayüzü kullanılır.

- Listeners

Birçok event listener mevcut. Bunlar default olarak catalina'ya assign edilmiş durumdadır.

```xml
<Listener className="org.Apache.catalina.startup.VersionLoggerListener" />
```

# shutdown

aşağıdaki satır JVM'in 8005'ten port dinlemesi ve SHUTDOWN string'i o porta yollandığı takdirde kendini kapatmasını belirtmektedir.

```xml
<Server port="8005" shutdown="SHUTDOWN">
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# deployment descriptors

| file                   | platform        | directory           |
|------------------------|-----------------|---------------------|
| application.xml        | Java EE         | META-INF            |
| application-client.xml | Java EE         | META-INF            |
| beans.xml              | CDI             | META-INF or WEB-INF |
| ra.xml                 | JCA             | META-INF            |
| EJB-jar.xml            | EJB             | META-INF or WEB-INF |
| faces-config.xml       | JSF             | WEB -INF            |
| persistence.xml        | JPA             | META-INF            |
| validation.xml         | Bean Validation | META-INF or WEB-INF |
| web.xml                | Servlet         | WEB-INF             |
| web-fragment.xml       | Servlet         | WEB-INF             |
| webservices.xml        | SOAP            | META-INF or WEB-INF |

# "Java EE deployment descriptors" vs "runtime deployment descriptors"
JavaEE implementasyonları (app server'lar) kendi içlerinde ek olarak XML istiyor olabilir. bunlar "runtime deployment descriptors"tır. örnek: sun-application.XML, sun-web.xml.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# dependency Injection types

## Constructor-based

```java
public class Service {

  private Collaborator collaborator;

  Service(Collaborator collaborator) {

    this.collaborator = collaborator;
  }
}
```

## Setter-based

```java
public class Service {

  private Collaborator collaborator;

  @Required
  public void setCollaborator(Collaborator c) {
    this.collaborator = c;
  }
}
```

## field-based

```java
public class Service {
  @AUtowired
  private Collaborator collaborator;
}
```

# Avantajlar ve dezavantajlar
- field-based daha sadece okunabilirdir. diğerlerinde kod blokları daha uzundur.

- field-based olmayan injection'larda, instance'larını kontrol etme ve kontrol sonrası duruma göre aksiyon alabilmemiz mümkündür. ne gibi kontroller yapabiliriz:
  - inject edilen sınıfların null gelme durumları (log basma veya null gelen yerine NullObjectPattern uygulayabilme)
  - inject edilen sınıfların içindeki default variable değerlerini kontrol etme durumu
  - Eklenti altyapımız varsa, ek güvenlik için inject edilen sınıfların hangi class referansı olduğu durumunu kontrol etmek isteyebiliriz. çünkü Spring duruma göre (profillere, classpath'e göre) farklı çeşit class'lar inject edebilir.

- Constructor-based'de tüm instance'ları tek bir metotta kontrol edebiliriz. bu şekilde bir instance'ın özel bir durumu diğerini etkileyebilir. oysa setter-based'de sadece ilgili dependency için aksiyon alabiliriz.

- bir sınıfımıza gereğinden fazla field eklediğimizi düşünelim. bu sınıfı parçalara bölmek genelde doğru bir çözüm olmaktadır. eğer constructor-based injection kullanırsak, static code analizi yapan eklentiler bizi constructor çok fazla eleman kabul ediyor diye uyaracaktır. fakat field ve setter based injection'larda bu uyarıları alamayız.

- Field-based injection yapılan bir sınıfı Spring app container'sız init etme şansımız yok (tek şansımızı reflection kullanmak). Bu sebeple Spring container'sız çağrılabilecek sınıflar (bu durum bazen Spring kullanılmayan unit testlerimizde olabiliyor) var ise o sınıfların içinde field-based constructor kullanmamalıyız.

- Java'da filed'ın önüne annotation yazabiliyoruz fakat bu farklı dillerde olmayabilir veya DI framework'ü bunu desteklemiyor olabilir. Bu sebeple constructor based annotation'da spesific bir dependency'yi @Lazy yapamayabiliriz. Örnek:

```java
@Service
public class MyService {

  private Dependency1 d1;
  private Dependency2 d2;

  @Autowired
  MyService(Dependency1 d1, @Lazy Dependency2 d2){
      this.d1 = d1;
      this.d2 = d2;
  }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# circular dependency
A Bean'i B sınıfını inject ediyor ve B sınıfı, A sınıfını inject ediyorsa runtime sırasında BeanCurrentlyInCreationException hatası alırız.

Bu durum bir anti-pattern'dir. Mimari yeniden düzenlenmesi önerilir. Fakat redesign yapılamazsa birçok workarround çözümleri vardır. bazıları:

- circular dependency yaratan Bean'lerin inject edildiği yerlere @Lazy annotation'u koymak.
- setter-based injection kullanmak.

Bu workarround çözümler temelde şunu yapar: Spring, circular dependency problemi çıkaran Bean'leri aynı fazda initialize etmez. Spring birini sonradan initialize eder. böylece circular dependency problemi olmaz. Örneğin; constructor injection yapıldığında tüm field'ları Spring aynı anda initialize etmeli ki constructor çağrılabilsin. Bu sebeple constructor injection'da circular dependency problemi alabiliriz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# app server vs web server
Web sunucusuna bir istek geldiğinde, sunucu bu isteği basitçe en iyi karşılayabilecek programa aktarır. kısacası; yapılan çağrıları ilgili yerlere aktarmaktır ve dönen cevabı döndürmektir. load balancing, caching, gibi bir çok ek işlevi de yapabilir..

örnek sunucular: Apache HTTP Server, nginx

uygulama sunucusu ise; iş mantığını yürüten (işleten - üzerinde uygulamacıklar çalıştıran) ve API sunan bir motordur.

örnek sunucular: GlassFish, Weblogic, jboss (or WildFly), IBM WebSphere

Java dünyasının standartlarına göre; "EJB Container" içermeyen hiçbir yazılım uygulama sunucusu olamaz. Bu tanıma göre; Tomcat bir web sunucusudur.

benzer şekilde sadece web sunucusu olanlar: Jetty

# uygulama neden sunucuya ihtiyaç duyuyor?

her Java app server birer JavaEE implementasyonudur. JavaEE kütüphaneleri Maven'a eklenirken arayüzler ve abstractlar bazen de sınıflar tanımlanır. fakat uygulama ayağa kalkması için runtime'da bunların implementasyonlarına ihtiyaç duyar. her uygulama sunucusu JavaEE'yi kendisi farklı implemente etmiş olabilir.

Bir "web" uygulamasını ayağa kaldırabilmek için en az "Servlet Container"'a ihtiyaç vardır. İşte bu kütüphane tüm app server'larda + Tomcat'te dahi mevcuttur. Spring-framework uygulaması dahi bunu yapmaya zorunludur. Fakat Spring bu kütüphaneyi direk olarak kendi içinde bulundurmaktadır. Daha sonra hayata geçirilen Spring-boot projesi bu kütüphanelerin uygulama içerisine gömülmesini daha çok kolaylaştırmaktadır. Spring projesini bir app server'da çalıştırdığımızda, Spring uygulaması app server'ın "servlet container"'ını kullanabilir.

Servlet container bu sebeple "web container" olarak ta adlandırılmaktadır.

not: Spring sadece 'servlet container'a ihtiyaç duyuyor. Oysa JavaEE bir spesifikasyon ve komple tüm kütüphaneler sunucu içerisinde.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# javax.servlet.Servlet
groupId: __javax.servlet__, artifactId: __javax.servlet-API__ paketi servlet arayüzlerini içeriyor. bu paket JDK içinde de geliyor. runtime sırasında JavaEE app server'ında bunu implemente eden sınıflar zaten çalışıyor. kod direk implementasyonları çağırıyor.

__javax.servlet.Servlet__ arayüzünün birçok implementasyonu var: FacesServlet, GenericServlet, HttpServlet.

javax.servlet.Servlet şu metotları barındırıyor:
- destroy() servlet artık hiç kullanılmayacağı zaman çağrılıyor
- init(ServletConfig config) uygulamamızın hayatı boyunca 1 kere çağrılıyor. ServletConfig buraya otomatik JavaEE sunucusu tarafından pass ediliyor.
- service(ServletRequest req, ServletResponse res) her request'te çağrılıyor
- getServletConfig() servlet ayarlarında verdiğimiz config'leri çekebilmemizi sağlıyor. init metotunda verilen ServletConfig'i çekebilmemizi sağlıyor.

1 servlet instance'ını birden fazla thread aynı anda kullanıyor olabilir. eğer hiçbir thread o instance'ı kullanmazsa o zaman servlet container bu servlet'i destroy edebilir. bu şekilde kaynakları boşaltmış olur. fakat hemen ardından gerekiyorsa yeni instance oluşturur. bu kararlar servlet container'a kalmıştır.

javax.servlet.http.HttpServlet ise javax.servlet.Servlet'ten extend eder ve bu metotları da içerir:
- doGet(HttpServletRequest req, HttpServletResponse resp)
- doHead(HttpServletRequest req, HttpServletResponse resp)
- doPost(HttpServletRequest req, HttpServletResponse resp)
- ...

# javax.servlet.ServletConfig
Servlet'e pass edilen interface. İçerisinde aşağıdaki metotlar var.

```java
public String getServletName();
public ServletContext getServletContext();

// init params are key-value pairs that we give on deployment and runtime descriptors.
public String getInitParameter(String name);
public Enumeration<String> getInitParameterNames();
```

# javax.servlet.ServletContext
ServletConfig içerisindeki bir metot sayesinde, Servlet sınıfı ServletContext'e erişir. ServletConfig sınıfı ile Servlet spesific bilgi bulundurabiliriz. Fakat ServletContext tüm web uygulamsında globaldir ve her Servlet aynı ServletContext'i okur. ServletContext bu sebeple aşağıdaki gibi metotlar barındırır:

```java
public ClassLoader getClassLoader();
public int getSessionTimeout();
public void setSessionTimeout(int sessionTimeout);

// below method return configs from deployment or runtime descriptors.
public String getRequestCharacterEncoding();
```

Yani; herhangi bir servlet içinde servlet'in kendi config'lerine erişmek için bu __ServletConfig__ kullanılır:

```java
ServletConfig servletConfig = getServletConfig(); //getServletConfig javax.servlet.Servlet sınıfında zaten tanımlıdır.
String servletName = servletConfig.getInitParameter("ServletName");
```

Fakat tüm servlet'lerde kullanılacak olan bilgiler ServletContext'ten çekilir:

```java
ServletContext servletContext = getServletContext();
String projectOptions = servletContext.getInitParameter("projectOptions");
```

# request.getSession().getServletContext() vs getServletContext()
__getServletContext()__ Servlet class'ından extend eden sınıflarda direk kullanılabilir bir metottur. oysa request objesi herhangi bir kod satırında kullanılabilir. request'ten de ServletContext'e erişilebilir. İkisi aynı instance'ı döndürürler.

# RequestContextListener
Bu sınıftan implemente etmiş bir sınıfı Spring'e tanıttığımızda request oluştuğundaki event'lerimizi tanımlayabiliriz.

# DispatcherServlet
Dispatch Türkçe kelime anlamı: sevk etmek. yazılım dünyasında metota/fonksiyona mesaj(parametre) yollama(sevk etme) anlamında kullanılır.

SpringMVC'de __front controller__'ın ismidir.

__DispatcherServlet__, __HandlerMapping__'e başvurarak ilgili Controller bilgisine ulaşır. Varsayılan olarak __BeanNameUrlHandlerMapping__ handler olarak kullanılır.

her servlet'in bir URL'si vardır. alt-URL'ler 1 adet servlet sınıfının kodunun içinde if blokları ile dağıtılır. fakat Spring MVC bunu bizim için otomatik yapar. Spring MVC'nin (request'i ilk karşılayan) servlet'i __org.springframework.web.servlet.DispatcherServlet__'dir.

# Web XML
örnek web.xml'imizde Spring MVC'nin config'lerini değiştiriyoruz:

```xml
<web-app>

  <listener>
      <!-- Burada application context'in hangi framework loader'ı ile ayağa kaldırılacağını belirtiyoruz. ContextLoaderListener default olarak /WEB-INF/applicationContext.xml dosyasını okur. fakat biz bu örnekte bunu, <context-param> ile override ettik. -->
      <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

  <context-param>
      <!-- root-context'imiz: applicationContext. app-context içerisinde Bean'lerin tanımları mevcut.-->
      <param-name>contextConfigLocation</param-name>
      <param-value>/WEB-INF/app-context.xml</param-value>
  </context-param>

  <!--
  Yukarıda dikkat edilirse XML based bir tanımlama yaptık. Eğer Bean'lerimizi annotation ile tanımlamak istersek:
  <context-param>
      <param-name>contextClass</param-name>
      <param-value>
        org.springframework.web.context.support.AnnotationConfigWebApplicationContext
      </param-value>
  </context-param>
  -->

  <servlet>
      <servlet-name>app</servlet-name>
      <!-- bu servlet'imiz bizim elle tanımladığım değil, Spring'in Spring-MVC (Spring web MVC) modülünde sunduğu front-controller pattern'ine karşılık gelen servlet'idir. bu servlet exception-handling, view-resolver, request mapping (for redirection of sub-URL's to other servlets), MultipartResolver gibi görevleri yapabiliyor. -->
      <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
      <init-param>
          <!-- child-context'imiz: webApplicationContext. dispatcher-servlet-context-1.xml içerisinde Bean'lerin tanımları mevcut. bu Bean'ler sadece bu DispatcherServlet'in yönettiği Servlet'ler tarafından kullanılabilecektir. -->
          <param-name>contextConfigLocation</param-name>
          <param-value>/WEB-INF/dispatcher-servlet-context-1.xml</param-value>
      </init-param>
      <load-on-startup>1</load-on-startup>
  </servlet>

  <servlet-mapping>
      <!-- servlet-name bir ID'dir. detayları yukarıda tanımlı. burada sadece hangi URL'yi karşılayacağı yazıyor. -->
      <servlet-name>app</servlet-name>
      <url-pattern>/app/*</url-pattern>
  </servlet-mapping>

</web-app>
```

Yukarıdaki tanımları servlet ayağa kaldıracaksak yapmak zorundayız. Çünkü;

1- web projeleri, war dosyalarından oluşur. app server'lar, war dosyalarını okurken (standartlar gereği), önce web.xml'ini okur. bu sebeple; servlet sınıfımızın, Spring (DispatcherServlet) tarafından yönetildiğini app server'a belirtmemiz şarttır.

2- "dispatcher-servlet-context-1.xml" tanımı olmazsa dispatcher-servlet-context-1.xml'de belirtilen Bean'lerimizi ilgili servlet'imizde kullanamayız. Servlet'ler birer "Spring Bean" olmadıkları için onları inject edebilmemiz için ek olarak "dispatcher-servlet-context-1.xml" tanımlaması şarttır.

Yukarıda 2'inci maddede belirtildiği gibi normal koşullarda servlet'ler Bean değildir. fakat Bean olarak tanımlanırlarsa Spring boot bunları servlet container'a otomatik ekler. kaynak: (source-id: 348) "Registering Servlets, Filters, and Listeners as Spring Beans" başlığı, 1inci paragraf.

Yukarıdaki gibi birçok __DispatcherServlet__'i aynı projede kullanabiliriz. Her __DispatcherServlet__'in URL'si farklı olacaktır.

Not: 1 dispatcher-servlet yada 1 servlet, birden fazla WebApplicationContext'e sahip olabilir. kaynak: (source-id: 349) "1.1.1. Context Hierarchy" 2. paragraf.

# WebApplicationContext
Spring içinde olan bir sınıftır. Bize __javax.servlet.ServletContext__ ile bütünleşik çalışabilmemizi sağlar. Bütünleşik çalışmadan kasıt şudur: Her Spring Bean'i isterse __javax.servlet.ServletContext__'e erişebilecektir. Bu şu şekilde olur:

__WebApplicationContext__ aracılığı ile ayağa kaldırılan her Bean, __ServletContextAware__'ten implemente ettiği takdirde, __javax.servlet.ServletContext__'e erişebilecektir.

__ServletContextAware__ koduna bakarsak; ServletContext'in inject ediliğini görürüz:

```java
package org.springframework.web.context;

public interface ServletContextAware extends Aware {
     void setServletContext(ServletContext servletContext);
}
```

WebApplicationContext genelde 1 uygulamada 1 adet olur. Tabi bu durumda repository, business-logic-service'lerimizde WebApplicationContext içerisinde olur. Çünkü servlet'e gelen istekten bu diğer Bean'leri çağırmamız gerekecektir. kaynak: (source-id: 349) "1.1.1. Context Hierarchy" 1. ve 2. paragraf.

# Spring boot embedded Tomcat or jetty server
Tomcat gibi app server'lar, servlet container barındırır. Bu sebeple Spring özel bir ApplicationContext türevi kullanır: ServletWebServerApplicationContext. Bu sınıf aynı zamanda WebApplicationContext türevidir. Bu sınıf otomatik olarak ServletWebServerFactory Bean'lerini bulur. Bu Bean'lerin implementasyonları da runtime sırasında TomcatServletWebServerFactory, JettyServletWebServerFactory gibi karşımıza çıkar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
