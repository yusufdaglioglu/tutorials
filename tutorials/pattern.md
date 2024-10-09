
############################

############################
# PATTERN
############################

############################

The entire list below is described in order in this document. If a pattern is described in a different document, a link has been added next to it.

Some titles in the below list may not be counted directly as patterns. But they are design approaches. If we try to implement these design approaches, we will definitely be making use of some patterns.

- # Creational pattern
  - Prototype pattern
  - Singleton pattern
  - Builder pattern
  - Factory pattern + Factory method pattern (or Virtual Constructor) + abstract factory pattern (or Factory of Factory pattern or eng:kit)
  - [file"java_bean.md" - title:"DI (or Dependency Injection)"](./java_bean.md#di-or-dependency-injection)
  - object pool pattern (It explained under "Thread pool pattern")
  - Lazy Initialization Pattern
- # Structural pattern
  - Private class data pattern
  - Facade pattern
  - Proxy Pattern (or vekil deseni)
  - Key differences of patterns: proxy vs facade vs adapter vs delegation
  - Delegation Pattern
  - Adapter pattern
  - Composite pattern
  - Bridge pattern (or Handle/Body pattern)
  - Decorator pattern
- # Behavioral pattern
  - Mediator pattern
  - Parameter Object pattern
  - command pattern (or Action pattern or Transaction pattern)
  - Iterator pattern (or Cursor pattern)
  - Null object pattern
  - Observer pattern (or Gözlemci deseni)
  - State pattern (or Objects for States pattern) vs strategy pattern vs template method pattern vs provider pattern
  - Specification pattern
  - Memento pattern (or Token pattern)
  - Chain Of Responsibility
- # Concurrency patterns
    - Thread pool pattern (It can be categorized also under "Creational pattern". Because it is an implementation of "object pool pattern".)
- # Architectural patterns
  - # Patterns that can be applied mostly in microservice based system
    - Decomposition patterns: "Decomposition by domain capabilities" vs "Decomposition by business capabilities"
    - Sidecar pattern (or sidekick pattern)
    - database per service pattern
    - Shared database
    - microservice pattern (or mikroservis deseni) vs Monolithic pattern (or monolitik desen) vs modular monolith
    - SOA (or Service-oriented architecture)
      - Enterprise application integration (or EAI) vs Enterprise Integration Patterns (or EIP)
      - service bus (or enterprise service bus or ESB)
    - Externalized configuration pattern
    - API gateway pattern
    - API composition pattern
    - Circuit Braker (or devre kesici)
    - CQS (or Command Query Separation) vs CQRS (or Command Query Responsibility Segregation)
    - Materialized View pattern
    - two-phase commit (or 2PC)
    - saga pattern
      - Choreography
      - Orchestration
      - Hybrid
    - Transactional outbox pattern
      - Polling publisher pattern
      - Transactional Log Tailing
    - event sourcing pattern
    - Service registry pattern
    - Client-side Service Discovery Pattern vs Server-side Service Discovery Pattern
    - Self Registration pattern
    - 3rd Party Registration
    - Blue-Green Deployment Pattern
    - Microservice chassis
    - Service Template
    - Self-contained service
  - # Other Architectural Patterns
    - [file:"pattern_ddd.md" - title:"Domain-driven design (or DDD) vs data-centric design (or database-centric design or data-driven design)"](./pattern_ddd.md#domain-driven-design-or-ddd-vs-data-centric-design-or-database-centric-design-or-data-driven-design)
    - MVC vs Web MVC vs MVP vs MVVM vs Redux vs Flux vs Presentation Model (or Application Model) [file:"pattern_mvc.md"](./pattern_mvc.md)
      - Flow Synchronization pattern vs Observer Synchronization pattern [file:"pattern_mvc.md"](./pattern_mvc.md)
    - [file:"pattern_mvc.md" - title:"micro-frontends (or micro frontends)"](./web_custom_elements.md#micro-frontends-or-micro-frontends)
    - Humble Object pattern
    - Data Access Object (or DAO) pattern (This pattern can be categorized as "Structural" too.)
    - Repository pattern
    - Front Controller pattern
    - Backend For Frontend
    - unit of work pattern
    - multitier architecture (or n-tier architecture) vs multilayer architecture (or n-layer architecture)
    - onion architecture
    - hexagonal architecture (or Ports and Adapters architecture)
    - clean architecture
    - Rich Domain Model vs Anemic Domain Model
- # Database pattern
  - [file:"database_cassandra.md" - title:"one table per query pattern (or one-table-per-query pattern) vs query-first approach (or query-driven modelling)"](./database_cassandra.md#one-table-per-query-pattern-or-one-table-per-query-pattern-vs-query-first-approach-or-query-driven-modelling)
  - [file:"database_cassandra.md" - title:"time series pattern"](./database_cassandra.md#time-series-pattern)
  - [file:"database_cassandra.md" - title:"wide partition pattern (or wide row pattern)"](./database_cassandra.md#wide-partition-pattern-or-wide-row-pattern)
- # Uncategorized
  - Higher-order function
  - mixin (or mix-in)
  # Test Pattern
  - AAA (or Arrange Act Aspect) pattern
- # Anti-Pattern
  - Don't Use Exceptions For Flow Control
  - Blob (or Monster Class or god Bean or god object or god class or Winnebago)
    - God Method (or god function)
  - Dependency hell
  - Spaghetti code
  - Reinventing the square wheel (or Reinventing the wheel)
  - Tester-driven development (or bug-driven development) (No explanation needed)
  - Programming by permutation (or programming by accident or shotgunning) (No explanation needed)
  - Copy and paste programming (or Copy-and-paste programming or Cut-And-Paste Programming or Clipboard Coding or Software Cloning or Software Propagation) (No explanation needed)
  - law of the instrument (or law of the hammer or Maslow's hammer or golden hammer or Old Yeller or Head-in-the sand)
  - Softcoding vs Hardcoding
  - magic string
  - Lava Flow (or Dead Code)
  - boat anchor (or boatanchor)
  - Error hiding
  - Busy waiting
  - Overengineering (or over-engineering or over-kill) (No explanation needed)
  - Calling super (or call super)
  - Cargo cult programming
  - vendor lock-in (or proprietary lock-in or customer lock-in)
  - Functional Decomposition (or No Object-Oriented AntiPattern or No OO)
  - Don't repeat yourself (or DRY or do not repeat yourself)
  - Silver Bullet
  - micromanagement (No explanation needed)
  - Smart UI Anti-Pattern

# Other Architectural Patterns
Chris Richardson, author of the book "Microservices patterns", listed the following (separately) as pattern under his contributor page: www.microservices.io/patterns (source-id: 461).

Since I personally know the following topics, I did not feel the need to explain them in this document.

- __Multiple service instances per host__ - deploy multiple service instances on a single host
- __Service instance per host__ - deploy each service instance in its own host
- __Service instance per VM__ - deploy each service instance in its VM
- __Service instance per Container__ - deploy each service instance in its container
- __Serverless deployment__ - deploy a service using serverless deployment platform
- __Service deployment platform__ - deploy services using a highly automated deployment platform that provides a service abstraction (Kubernetes, OpenShift...)
- __Service per team__ - Each team of organization manages one or more services independently.
- __Access Token__ - security konusu
- __Health Check API__ - each servis opens a health endpoint. An independed servis checks periodically all services.
- __Application metrics__ - merkezi bir serviste, tüm servislerin metriklerinin toplanması yöntemidir. 2 çeşittir: ya tüm servisler buraya bilgiyi kendileri yollar, yada merkezi metrik servisi tüm diğer servislerden periyodik olarak bu bilgileri bir endpoint aracılığı ile okur/toplar.
- __Exception tracking__ - All services reports all exceptions to a centralized exception tracking service that aggregates and tracks exceptions and notifies developers.
- __Distributed tracing__ - Zipkin gibi tool'lar ile isteklerin yönünü ve harcadığı zamanı görebilmek için merkezi bir monitoring tool'u kullanma yöntemidir. Burada her servis her HTTP request alıp attığın merkezi service (örneğin Zipkin'e) haber verir.
- __Log deployments and changes__ - deployment yapılan herşeyi detaylı şekilde log'landığı yapılardır. Burada sadece deployment'ın ne olduğu değil, aynı zamanda öncesi ve sonrası metric'lerin ortalamaları gibi bilgiler de olmalıdır. Bu şekilde her deployment'ın etkileri ve sorunları gözlemlenebilmiş olur.
- __Audit logging__ - user activity'nin database'e log'landığı ve takip edilmesinin sağlanmasıdır. audit log'unda temel olarak user'ın activity'leri ve bu activity'lerin tetiklediği activity'ler takip edilir. diğer log'lar genel log mekanizmalarından takip edilmelidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# prototype pattern
Prototip deseni belirli nesnelerin kopyasını oluşturarak daha sonra yapılacak işlemler için, oluşturulan kopyanın kullanılmasını sağlayan yaratımsal desendir.

```java
User user2 = user1.clone();
user2.setName("mehmet");
```

Örneğin biyolojik bir hücre kendini iki parçaya bölüyor ve aynı hücreden iki tane oluyor.

Bazı durumlarda new ile obje yaratmak, her değeri tek tek set edileceğinden çok maliyetli olabiliyor. Bu gibi durumlarda tercih edilebilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# singleton pattern
bir objeyi, tüm uygulamada aynı instance'ı kullandırma yöntemidir.

Bu sık kullanıldığında veya dependency-injection yerine kullanıldığında anti-pattern olabilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# builder pattern

```java
User myUser = User.UserBuilder("Ahmet", "AnySurName")

                  .age(30)

                  .phone("1234567")

                  .address("Fake address 1234")

                  .build();
```

avantajlar:
- okunaklığı arttırıyor. bir constuctor'da 10 tane parametre olursa, hangi parametre nedir anlaşılmaz.
- constuctor'lara tüm parametreleri vermek istemeyiz. null göndermek yerine bu yapı tercih edilir. 10 adet alabilen bir constuctor için tüm kombinasyonları hazırlamak maliyetli olacaktır ve kod kalabalığı yaratacaktır.

# Chaining Methods (or method chaining or named parameter idiom)
bir metotun kendi instance'ını veya bir sonraki çağıracağımız metotun instance'ını döndürmesi durumudur. böylece zincirleme metotları üstüste çağırabiliriz. "chaining" sadece builder pattern'inden kullanılmaz.

# Fluent API (or fluent interface)
fluent kelime anlamı: akıcı.

metot chaining yaparak kompleks operasyonların yapılabilmesini sağlayan API'lerdir. 

# Fluent ve method chaining'e örnekler
- QueryDSL
- Apache Camel integration patterns API
- Java 8 Date Time API. Example: 

```java
LocalDateTime ldt  = LocalDateTime.now()
                                  .withDayOfMonth(1)
                                  .withYear(1878)
                                  .plusWeeks(2)
                                  .minus(3, ChronoUnit.HOURS);
```

- Builder pattern sadece kendi nesnesini döndüren setter'lar sunar. Builder'ın özel işlevler gerçekleştiren API sunmasına gerek olmadığı için, Builder, Fluent API kullanır veya kullanmaz konusu çok net değildir.

# Method cascading
syntactic sugar'dır. Dilin desteklemesi şarttır.

Pseudo kod olarak gösterirsek, aşağıdakinin:

```
a..b()
 ..c();
```

Buna denk gelmesini sağlar:

```
a.b();
a.c();
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# factory pattern (or simple factory pattern)
tek bir metot ile; new MyClass(param1, param2...) gibi bir yapıya ihtiyaç duymadan yeni sınıfları türetme yöntemidir. örneğin; sadece getMyClass(param1, param2...) yazıp aynı ihtiyacı giderebiliriz.

Bu pattern'de bir factory sınıfı vardır ve bu sınıfın içinde factory metotları vardır. factory metotlar dışında public metotlar olmamalıdır.

Spring'deki "applicationContext.getBean" fonksiyonu bu pattern'e örnek olarak verilebilir.

# Factory Method pattern (or Virtual Constructor)
"factory pattern" ile aynıdır. tek farkı abstract bir sınıf döndürmesidir.

```java
interface Interviewer
{
    askQuestions();
}

class Developer implements Interviewer
{
    askQuestions()
    {
        print('hey developer!');
    }
}

class Manager implements Interviewer
{
    askQuestions()
    {
        print('hey Manager!');
    }
}

abstract class HiringManager
{
    // Factory method
    abstract Interviewer makeInterviewer();

    doInterview()
    {
        Interviewer interviewer = this.makeInterviewer();
        interviewer.askQuestions();
    }
}
```

Now "HiringManager" should be implemented like "DevelopmentManager", "MarketingManager"...

# abstract factory pattern (or Factory of Factory pattern or eng:kit)
2 farklı implementasyon/kullanım mevcut:

1- Burada factory dönen metot bize factory tarafından sunuluyor:

```java
public interface Shape {
   void draw();
}

public class RoundedRectangle implements Shape {
   @Override
   public void draw() {
      System.out.println("Inside RoundedRectangle::draw() method.");
   }
}

public class RoundedSquare implements Shape {
   @Override
   public void draw() {
      System.out.println("Inside RoundedSquare::draw() method.");
   }
}

public class Rectangle implements Shape {
   @Override
   public void draw() {
      System.out.println("Inside Rectangle::draw() method.");
   }
}

public abstract class AbstractFactory {
   abstract Shape getShape(String shapeType) ;
}

public class ShapeFactory extends AbstractFactory {
   @Override
   public Shape getShape(String shapeType){
      if(shapeType.equalsIgnoreCase("RECTANGLE")){
         return new Rectangle();
      }else if(shapeType.equalsIgnoreCase("SQUARE")){
         return new Square();
      }
      return null;
   }
}

public class RoundedShapeFactory extends AbstractFactory {
   @Override
   public Shape getShape(String shapeType){
      if(shapeType.equalsIgnoreCase("RECTANGLE")){
         return new RoundedRectangle();
      }else if(shapeType.equalsIgnoreCase("SQUARE")){
         return new RoundedSquare();
      }
      return null;
   }
}

public class FactoryProducer {
   public static AbstractFactory getFactory(boolean rounded){
      if(rounded){
         return new RoundedShapeFactory();
      }else{
         return new ShapeFactory();
      }
   }
}
```

2- İlişkili Factory metotlarını aynı class içerisinde bulundururuz. örnek kapımız ve kapıyı montaj edecek uzman (door fitting expert) olsun. bu kişinin ilişkili olması gerekli.

```java
interface DoorFactory
{
    public Door makeDoor();
    public DoorFittingExpert makeFittingExpert();
}

class WoodenDoorFactory implements DoorFactory
{
    public Door makeDoor()
    {
        return new WoodenDoor();
    }

    public DoorFittingExpert makeFittingExpert()
    {
        return new Wooden_Door_Fitting_Expert();
    }
}

class IronDoorFactory implements DoorFactory
{
    public Door makeDoor()
    {
        return new IronDoor();
    }

    public DoorFittingExpert makeFittingExpert()
    {
        return new Iron_Door_Fitting_Expert();
    }
}
```

# static factory method vs constructor

örnek:

```java
new BigInteger("3")
```

vs

```java
BigInteger.valueOf("3")
```

- static'lerin avantajları:

  - constuctor'da metot ismi kullanmaz. sınıf ismi kullanır. bu sebeple constructor'ı çağırdığımızda yapılacak işin amacı için dökümantasyona bakmak şarttır. koddan direk anlaşılmaz.

  - static metot bize zaten var olan instance'ı dönebilir. oysa constructor'ın yeni instance yaratması şarttır.

  - bazı diller constructor overload'a izin vermiyor. eğer SL4J, SLF.Net gibi tüm cross-language facada hazırlanacaksa static factory kullanmak gerekebilir.

  - static factory metotlar aldığı parametreye göre (uygunluğu varsa) farklı bir sınıfta dönebilir. bunu constructor'da yapamayız.

- static'lerin dezavantajları:

  - OOP felsefesine aykırı

  - constructor'u olmayan sınıflar'dan subclass yaratılamaz. bu sebeple sadece static metot yaratıp, constructor'ları yazmaktan kaçamayız. fakat hem static hem constructor olunca, API'mizi kullanan developer'lara çok fazla seçenek sunmuş oluruz.

    constructor'larımızı protected yaratabiliriz. fakat bu sefer başka paketlerden kendimiz çağıramayız. bu dezavantajı gidermek için, sub-class yaratmak yerine "composition over inheritance" yapabiliriz. yada constructor'ları çok basit tutarız ve kullandırtmak istemiyorsak bunu DOC'lara not düşeriz.

genelde kullanılan static factory metot isimlendirmeleri:

- from: örnek Date.from(instant); instant birçok formatta olabilir. from bize istediğimiz sınıf yada subclass'a cast edecek.

- of: Set\<Rank> faceCards = EnumSet.of(JACK, QUEEN, KING); birçok parametre alarak bize uygun olan class'ı döner.

- valueOf: from ile aynı.

- getInstance veya instance: Calendar.getInstance(); getInstance bazen ekstra parametre isteyebilir.

- create or newInstance: Array.newInstance(classObject, arrayLen); yeni bir instance döndürür. ayarları parametre olarak alır.

Yukarıdaki isimlendirmeler zorunlu değildir. fakat genelde bu isimlendirme kullanılır. örneğin; Calendar.getInstance() aynı instance'yi döndürmüyor. singleton pattern'de getInstance metotu aynı instance'yi döndürür. fakat calendar buna uymamış. calendar bu metotu şu amaçla böyle isimlendirmişler: önce default Locale'i bul ve bana instance'yi oluştur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Lazy Initialization Pattern
Dependency injection framework'leri bunu kolaylaştırsa da manuel de (aspect gibi gelişmiş teknolojiler kullanmadan da) uygulanabilir bir pattern'dir.

Manuel uygulamak istediğimizde:
- bir class'ın constructor'ında, class'a ait private field'larımızı init etmeyiz.
- class'ımızdan bir metot çağrıldığında, ilgili metot da kullanılan ve init olmamış bir field var ise önce onları init edebiliriz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Private class data pattern
bir Java sınıfında sınıf değişkenleri tamamen dışarıya kapatılmak istenebilir. bunun için tüm değişkenleri private yapıp setter'larını kaldırmamız gerekecek.
Fakat buda yetmez, bu bilgilerin o sınıftan dahi değiştirilmemesini isteyebiliriz. Bunun için tüm data'yı yine sınıf değişkeni olarak; bütün bilgileri içeren tek bir sınıf yaratabiliriz. Bu oluşturduğumuz yeni sınıf constructor haricinde setter'a sahip olmamalıdır.

```java
class Math{
  private int kose1;
  private int kose2;

  alanHesapla(){
    // other code here
  }

  // getters
}
```

Bu yapılması daha iyi olacaktır:

```java
class MathData{

  private int kose1;
  private int kose2;

  MathData(kose1, kose2){
    // set private fields
  }

  // getters
}

class Math {

  private MathData data;

  Math(kose1, kose2) {
    // create new MathData and set to data.
  }

  alanHesapla(){
      // other code here
      // here we can call: data.getKose1()
      // but we can not call "setKose1"
  }

  // other code here
}
```

Artık Math sınıfımızda data.setKose1(); çağıramayız. Oysa ilk Math sınıfımızda kose1=3; yapabilirdik.

Yani dışarıya kapattığımız yetmezmiş gibi; ilgili sınıfımızda da önlem almış oluyoruz.

Bu çözüm yerine Java'da sunulan "final" keyword'ünü kullanabiliriz. Java'da "Final" field'lar constructor'da set edilebiliyor.

- Bu pattern eğer ilgili programlama dilinde final keyword'ü yoksa kullanılabilir.
- Bu pattern aynı field'lar birden fazla sınıf içerisinde kullanılıyorsa kullanılabilir. Fakat bu use-case, tam olarak bu pattern'in amacı değil. Çünkü bu pattern'de mutlaka MathData içerisinde setter olmamasından bahsediyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Facade pattern
facade kelime anlamı: cephe, Bir yapının ön tarafta bulunan bölümü

birden fazla sınıfın işlevini birleştirip çağırmak durumunda kalabiliyoruz. örneğin; getComputer() işlemi kendi içerisinde %90 getKlavye(); getMotherBoard(); gibi işlemleri yapması gerekiyor. Bunları tek tek çağırmak yerine facada sınıfı yaratıp, sadece facada sınıfından yararlanma metotolojisidir.

pattern'in tanımında belirttiğimiz gibi; bir metot (init metotu) diğer tüm ihtiyaçlarımızı görecek metotları çağırmaktadır.

Kullanım örnekler:
- Simple Logging Facade for Java (SLF4J). SLF bir interface ile tüm implementasyonların değil, birçok log kütüphanelerinin kullanılabilmesi için arayüz sunmaktadır. burada SLF4J kütüphanesinin init() metotunu çağırdığımızda:
  - A log kütüphanesi için 2 adet metot
  - B log kütüphanesi için ise 1 metot çağırıyor olabilir.
- 3üncü parti veya dokunamadığımız legacy code'ları, dışarıya basit bir API ile çağrabilmemizi sağlar. Örneğin; legacy veya 3üncü parti kodumuz, sipariş veriyor olsun. siparişi verme, ödemeyi alma, raporu çıkarma ve bunu email atma süreçleri tek akış ile çağrılması gereksin. İşte bunların tümünü facade'de toplayıp tek 1 metod ile çağrabiliriz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Proxy Pattern (or vekil deseni)
Proxy sınıfımız, aracılık ettiği gerçek sınıfı implemente eder. Böylece aynı API'yi sunar.

Proxy asıl sınıfın bir impelentasyonu olduğu için onun yerine her yerde kullanılabilir:

```java
// with proxy:
Service service = new ProxyService();

// without proxy:
Service service = new Service();
```

Proxy asıl sınıfı çağırmadan önce veya sonra şu gibi işlemleri yapabilir:

- önce komutu koşturabilecek yetki olup olmadığı kontrol edilir (security)
- ek input/data validasyonu yapabilir
- caching yapabilir
- logging

Gerçek kullanım örnekleri:
- Sistemimizde 3üncü parti bir sınıf olsun. Bu sınıfı başka bir sınıfa kullandırtacağız. Kullandırtırken ek validasyon gerekiyor. Bu validasyonları proxy sınıfı ile yapabiliriz.
- A sınıfın B sınıfına (paket yetkileri sebebi  ile) erişimi yoktur. Böyle bir durumda A ile B arasına bir proxy sınıfı konulabilir.
- Spring Framework şunlar için Proxy kullanır:
  - aspect metodlarının gerçek sınıfları koşturmadan önce veya sonra çalışmaları için.
  - bean'lerdeki field validation'ları yapmak için (setter getter'lardan önce validasyon yapar)
  - JPA Entitiy ve diğer bean'lerde Autowired edilmiş field'larda Lazy initialization yapmak için.
- Uygulamamızda eklenti altyapımız olsun. Eklentinin metodu çağrılmadan önce, ona bir proxy atıp, ona giden gelen parametreleri (ve daha birçok şeyi) yönetebiliriz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Key differences of patterns: proxy vs facade vs adapter vs delegation

# wrapper vs others
wrapper bir pattern değildir. wrapper bir sınıfı kendi sınıfımızın içinde barındırdığımızda kullanılan terimdir. "Wrapper kullanıldı" denildiğinde bir amacı ifade etmez. Oysa proxy bir pattern'dir. belli bir amacı vardır. Proxy ve diğer birçok pattern, başka bir sınıfı wrap edilerek implemente edilebilir.

"Design Patterns: Elements of Reusable Object-Oriented Software" isimli kitapta Adapter ve Decorater patternleri için "known as" olarak "wrapper" yazmaktadır. Fakat piyasada "wrapper" kelimesi, yukarıdaki paragrafta belirtildiği gibi çok genel anlamda kullanılmaktadır.

# facade vs (proxy & delegation)
facede's purpose is to simplify a complex API and expose a simple one instead. but proxy and delegation pattern's purpose is to make additional pre or post operations before we call the target class.

# adapter vs others
gerçek kod implementasyonuna bakıldığında, aşırı benzer bir kod çıktısı görebiliriz. fakat temeldeki amaçları farklıdır. adapter bir sınıfı diğerine adapte edebilmek için kullanılır. fakat örneğin proxy arada validasyon gibi ek işlemler yapabilmemizi sağlar.

# proxy vs delegation
proxy pattern ile benzerdir. 2 temel fark vardır:
- Proxy, client tarafa, target obje ile aynı interface'i sunar. Delegation inheritance yerine, composition kullanarak bu işi yapar.
- Delegator pattern, referans olduğu sınıfın metotlarını aynen çağırırken, proxy pattern arada log'lama, auth-kontrol gibi birçok işlev gerçekleştirir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Delegation Pattern (or Delegator Pattern)
bazı kaynaklarda "__proxy chains__" olarakta geçmektedir.

Detay için farklı başlıkta anlatılan "proxy vs delegation"'a bakılmalı.

```java
class MyClass {

  Delegator d; // Delegator birden fazla olabilir.

  method1(){
    d.method1();
  }

  method2(String a){
    d.method2(String a);
  }
}
```

Delegator pattern için gerçek kullanım örnekler:
- proxy pattern'de yapabileceğimiz herşeyi yapabiliriz. Fakat aynı API ile dışarıya açsakta objeyi diğerinin referanslarının olduğu yerde taşıyamayız kullanamayız:

```java
// with proxy:
Service service = new ProxyService();

// without proxy:
Service service = new Service();

// with delegator:
Service service = new DelegatorService(); // it can not cast. it will throw exception.
```

- inheritance yerine composition kullandığımız zamanlar Delegator pattern'i kullanılırız.
- Bazı diller multiple inheritance desteklemiyor (örnek Java). Bu gibi durumlarda Delegator pattern kullanılabilir.
- Multiple inheritance destekleyen dillerde, aynı metod imzasına sahip iki super class olduğu durumlarda compile sırasında Proxy pattern kullanırsak hata alacağız. Buna çözüm olarak Delegator pattern kullanılabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# adapter pattern
adapter is a bridge between two incompatible interfaces.

örnekler:

- Bazı __middleware__ yazılımlar adapter pattern'i karşılamaktadır. Örneğin: eski teknoloji kullanan bir back-end yazılımımız production ortamında çalışıyor olsun. Bunu artık modern bir API ile dışarı açmak istiyoruz. Eski olan koda dokunmak istemeyen şirket, bu yazılımın önüne bir middleware yazıyor ve artık bu yeni yazılımı dışarıya açıyor. Yeni yazılım arkaplanda eski yazılımı wrap ederek çalışıyor.  Bu senaryoda, eski API'leri, yeni API'lere uyumlu olacak şekilde adapte etmiş oluyoruz.

  Burada middleware yazılımı __adapter__ rolünde oluyor.

  __Adapte edilen__ rolünde ise eski back-end yazılımımız oluyor.
  Kelime hakkında:
  - bunun İngilizce'si "__adapted__" olmasına rağmen bazı kaynaklarda "__adaptee__" olarak yazılmış. Fakat İngilizce sözlüklerde adaptee'nin karşılığını hiç bulamadım.
  - İngilizce'de benzer olarak "adoptee" kelimesi var. Fakat "benimsenmiş" anlamına gelmektedir.
  - "Adaptee" kelimesi Wiktionary sözlüğüne (kaynak: https://en.wiktionary.org/wiki/adaptee) göre Fransızca'daki "adaptée" den alınmış ve sadece yazılım dünyasında kullanılmaktadır.

- Bir API bizden int (primitive) yerine Integer (class) parametresini argüman olarak zorunlu tutuyorsa, o zaman bu senaryoda Integer (class) adapter rolünde bir wrapper'dır. adapte edilen ise primitive int'tir.

- Bir JSON mesaj alıp dönen HTTP API'miz olsun. Müşteri bizden XML alıp atmak istedi. Bunu yapmak için eski koda dokunamıyorsak, var olan JSON servisimizin önüne JSON-TO-XML-CONVERTER isimli bir servis yazarız. Bu yazıdğımız yeni servis adaptor rolündedir. Adapte edilen ise eski servisimizdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# composite pattern

- örnek;

Data class'ımız olsun. Folder ve File implementasyonumuz olsun. File sadece tek bir "Data" Objesi barındıracaktır. Oysa Folder, bir Data Listesi barındıracaktır. Bu "Data" listesi içinde file yada folder olabilir.

Şimdi main class'ımızda ;

```java
Folder root = new Folder();
root.add(new Folder());
root.add(anotherSubFolder);
```

yukarıdaki kod akışında içiçe dizinler yaratabiliyoruz. İşte bu tarz bileşik data tutulup ağaç yapısı hazırlayabilecek tasarımlara "composite pattern" denir.

- örnek;

```java
class Employee {

  String name;
  List<Employee> subEmployees;
}
```

YUkarıdaki obje ile tüm şirketin hiyerarşisi bile yaratılabilir. en üstte genel müdür employee'si olacaktır.

- örnek;

AritmeticExpression sınıfı. Bu sınıf büyük bir denklemi barındırıyor. List\<Operand\> barındırıyor. Her operand yine kendi içinde operand listesi barındırıyor.

1 # ( 2 + ( 3 + 4 ) ) denklemimizi örnek alabilir. bir Operand 1,# ve X'i barındırıyor olacak. X operand'ı ise içinde; 2, + ve Y operand'ını bulunduracak. Y operand'ı ise; 3, + 4 ü barındırıyor olacak gibi...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# bridge pattern (or Handle/Body pattern)
pattern'in amacı;
- 1- modüldeki implementasyonlardan ortak bir soyut sınıf yaratırız.
- 2- Her implementasyonumuz bu soyut sınıftan türetiz.
- 3- Artık soyu bu sınıfımız, modülün diğer kısımları için bir köprü olmuş olur. Böylece bu köprüyü istediğimiz yerde kullanabileceğiz.

Gerçek örnek üstündne gidelim: Satış raporlarının çıktısı alan kodlarımız olsun:

```
- Rapor         (Interface)
  - Internet    (Interface)
    - PDF       (Implementation)
    - DOC       (Implementation)
  - Magaza      (Interface)
    - PDF       (Implementation)
    - DOC       (Implementation)
```

(Yukarıdaki her maddeye gerçek örneğimiz üzerinden not alarak ilerleyelim)

- 1- modüldeki implementasyonlarımız: PDF ve DOC. Bunlardan "Format" isimli abstract bir sınıf yaratırız.
- 2- PDF ve DOC, abstract olan Format'tan türetiriz.
- 3- Artık Rapor nesnemize format parametresini geçebiliriz. Yani format bizim için bir köptü olmaktadır.

Sonuçta bu mimari oluşur:

```
- Rapor         (Interface)
  - Internet    (Implementation)
  - Magaza      (Implementation)

- Format      (Interface)
  - PDF       (Implementation)
  - Doc       (Implementation)
```

Köprü rolünde olan sınıf Format'tır. FormatBridge diye adlandırabilirdik.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Decorator pattern

```java
interface Cloud_Computer_Service
{
  getCost();
  getDescription();
}

class Simple_Cloud_Computer_Service implements Cloud_Computer_Service
{
  getCost(){
      return 10;
  }

  getDescription(){
      return 'Simple Cloud Computer Service';
  }
}

class VIP_Cloud_Computer_Service implements Cloud_Computer_Service
{
  private Simple_Cloud_Computer_Service simpleService;

  getCost(){
      return simpleService.getCost() + 5; // VIP computers has 5 GB more RAM. the cost increased.
  }

  getDescription(){
      return 'VIP Cloud Computer Service (+5 more GB RAM)';
  }
}
```

Yukarıda, VIP bilgisayarlar, simple'lara göre her zaman 5 $ fazla olacak. Bu dinamik olarak şekilde ayarlanmış. Bu inherit edilerek de yapılabilirdi, fakat inheritance yerine composition tercih etmiş olduk.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Mediator pattern
birçok farklı ve birbirinden bağımsız sınıf, tek bir merkezi sınıf (bu sınıf "__mediator__" rolünde oluyor) üzerinden yönetildiği pattern'dir.

örnekler:

- GUI form component

  GUI uygulamarındaki kullandığımız Form objeleri birçok bağımsız GUI objesini (button, textbox...) barındırabiliyor. Bu şekilde toplu işlem yapabilmemizi sağlamaktadır. örneğin; "send" buttonuna tıklandığında, tüm textbox'lardaki validator'ları aktif execute edebiliriz.

  form componentimiz mediator rölündedir.

- chat uygulaması

  Birden fazla User ve sadece 1 adet Chat-Group sınıfımız olsun. tüm user'lara 1 sınıfı üzerinden direk mesaj atabilmekteyiz.

  Chat-Group sınıfımız mediator rölündedir.

- Fligh management system

  Havacılıkta, tüm uçaklar sadece "kule" ile temasa geçerek inip inmeyeceğine veya nereye inceğine karar veriyor. Uçaklar direk birbirleri ile hiçbir şekilde temasa geçemiyorlar. Fakat kule tüm uçaklara aynı anda bildirim atabiliyor. kule, kendi belirleyeceği, diğer uçuşların statülerine bakarak, tüm uçaklara (veya sadece belli filtreye uyan uçaklara) bildiri atabiliyor.

  Kule sınıfımız mediator rölündedir.

- Message Orinented Middleware (or MOM)

  Birçok "subscriber" sadece Kafka, ActiveMQ gibi bir sistem üzerinden bilgi alışverişi yapabilmektedir. dolayısı ile ActiveMQ, subscriber'lara veya sadece belli topic'leri takip eden subscriber'lara mesaj atabiliyor.

  ActiveMQ "mediator" rolündedir.

- Executor/Scheduler frameworks

  - java.util.Timer class scheduleXXX() methotları
  - Java Concurrency Executor execute() methotları

  Bu sınıflar birden fazla thread'in açılıp kapanmasını merkezi bir yerdne yönetebilmemizi sağlıyor. Dolayısı ile Timer ve Executor sınıfları "mediator" rolündedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Parameter Object pattern
Bir fonksiyona biçok parametre geçmek yerine, 1 nesne içerisinde parametreler geçildiği durumdaki pattern'dir.

avantajlar:
- parametrelerin çok fazla olduğu fonksiyonlarda okunabilirlik azalıyor.
- "Event" gibi mekanizmalarda, kod yazıldıktan bir süre sonra yeni bir field eklendiğinde tüm metodu taşıyan fonksiyonlarda güncelleme yapmaya gerek kalmıyor.

dezavantajlar:
- dikkatsiz kod yazıldığında, parametre objesinin içinde kullanılmayan field'lar oluşuyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# command pattern (or Action pattern or Transaction pattern)
Yapılacak bir işin detaylarını, bu işi tetikleyen taraftan soyutlamak için kullanılan bir pattern'dir. örnek:

```java
class RemoveThingsFromDB implements Runnable {
    public void run() {
        System.out.println("Removing things from DB...");
        // add here many code
    }
}

class EditThingsOnDB implements Runnable {
    public void run() {
        System.out.println("Editing values on DB...");
        // add here many code
    }
}

public class MainApp {
    public static void main(String[] args) {
        Thread thread = new Thread(new RemoveThingsFromDB());
        thread.start();

        Thread thread1 = new Thread(new EditThingsOnDB());
        thread1.start();
    }
```

Yukarıdaki kod örneğinde; main thread'in diğer thread'lerde yapılan işlemlerden hiç bilgisi yok. Sadece ne yapmak istediğini söylüyor ve işi ilgili sınıfın ilgili metotu hallediyor. Böylece işleri birbirinden soyutlamış oluyoruz.

Command pattern async veya sync olabilir. Ama her 2 durumda da genelde (uç durumlar hariç) bir domain bilgisi dönmesi beklenmez. Trace-id, invocation-date gibi teknik bilgiler dönebilir. Opsiyonel olarak, "java.util.concurrent.Feature" objesi ile yönetilebilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Iterator Pattern (or Cursor pattern)
for loop yerine iterator objesi ile dönmemizi sağlayan pattern'dir.

Iterator'lerin avantajı şudur:
- uygulamamızda tüm koleksiyon nesnelerimiz aynı iterator arayüzünden türemiş sınıfları kullanır. bu şekilde bize gelen koleksiyon tipini düşünmeden o koleksiyonda işlem yapabiliriz (döngü kurma gibi).
- koleksiyon sınıfımızdaki iterator objesi bize filtreli dönüşte yapabilir. örnek:

Radyo dinleme cihazımız olsun.

```java
Kanal {
   Frekans;
   Language;
}

class KanalListesi {

   kanalEkle(Kanal);
   kanalSil(Kanal);

   iterator(Language);
}
```

Yukarıdaki KanalListesi koleksiyonumuzun döndüreceği iterator'ın next(); metotu bize aldığı Language parametresine göre filtrelenip dönecektir. Zaten iterator'ın next() metotunun implementasyonunu KanalListesi sınıfını yazan yazılımcı yazacaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Null Object Pattern
Bir AbstractUser class'ımız olsun. Bundan 2 tane implementasyon olsun. 1 tanesi normal koşullarda yapacağımızı User implementasyonumuz, diğeri ise NullUser implementasyonumuz. NullUser'ın metotları hiç null pointer döndürmesin. örnek: NullUser.getName(); --> "User not available" dönsün. gibi.

Gerçek hattan kullanım örnekleri:
# 1
Bir ekran koruyucu yazılımımız olsun. Bu yazılım ekranda toplar gösterecek olsun. Toplar için effect'lerimiz olsun. Effectler biz dizinden yükleniyor olsun. dizinde hiçbir dosya olmadığında NullEffect sınıfımız devreye girecek ve default olarak ekranın efektsiz çalışmasını sağlayacaktır.

# 2
logger implementasyonlarımız olsun:
  - database-logger --> gelen her log'u database'ye kaydeder
  - file-logger --> gelen her log'u dosyaya kaydeder
  - system-out-logger --> gelen her log'u ekrana basar
  - null-logger --> gelen her log'u ignore eder (/dev/null gibi)

# 3
https://github.com/j-easy/easy-random/wiki/excluding-fields Bu sayfada da yazdığı gibi SkipRandomizer sınıfı Null Object Pattern'i implemente eder.

org.jeasy.random.randomizers.misc.SkipRandomizer sınıfının içeriğine bakarsak şunu görürüz:

```java
public class SkipRandomizer implements Randomizer<Object> {

    @Override
    public Object getRandomValue() {
        return null;
    }
}
```

Randomizer'den türemiş diğer bazı sınıflar bu listede: https://github.com/j-easy/easy-random/tree/0426dcd8b2df555d276a16513ab92d79f99f8de5/easy-random-core/src/main/java/org/jeasy/random/randomizers/misc

Bu listeden bazıları:
- BooleanRandomizer.java --> Random boolean döndürüyor
- ConstantRandomizer.java --> Sabit döndürüyor
- UUIDRandomizer.java --> random UUID döndürüyor
- SkipRandomizer.java --> hiçbir şey döndürmüyor. Null olmasını istediğimiz objeleri SkipRandomizer aracılığı ile set'liyoruz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Observer pattern (or Gözlemci deseni)

Observer tasarım deseni; birbirleri ile bire çok (yani bir nesnenin içinde başka bir nesnenin listesinin bulunması olarak düşünebiliriz) ilişki olan nesneler arasında olay bazlı bir etkileşim olduğu durumları düzenler. Örnek senaryolar:

- Bir e-ticaret sitesinde bir üründeki stok değişiminde o ürünü takip eden üyelere haber verilmesi"

- Facebook'ta bir paylaşıma yapılan yorumlar için paylaşımcıya ve diğer yorumculara bildirim gitmesi

İlk senaryodan gidelim;

- "Observer (or consumer)" yapısına denk gelen sınıf "Üye" sınıfı olsun.

- "Observable (or Subject or object)" yapısına denk gelen sınıf "Ürün" sınıfı olsun.

ürün içinde, üyelerin listesi olsun.

Urun.updateFiyat(int newFiyat) metotunun içinde tum uye listesine email ile fiyatın değiştiğini haber veren işlem yapılmalıdır.

Observer tasarım deseni, Open-Closed Prensibine çok güzel bir örnektir. Yeni observer class'ları yarattığımızda, subject'i değiştirmeye gerek kalmıyordu.

# publish-subscribe vs Observer
publish-subscribe sistemlerde arada event bus (or sistemde adlandırabileceğimiz herhangi bir mekanizma: broker, message broker, event bus) vardır. oysa observer sistemde event bus yoktur. Observer'da, subject direk Observer'ların metotlarını çağırır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# State pattern (or Objects for States) vs strategy pattern (or policy pattern) vs Template method pattern vs provider pattern

# state vs strategy
iki pattern de birbirine çok benzerdir. fakat temel farklılık hangi nesnelerin hangi bilgiyi içerdiği ile ilgilidir.

iki pattern'de de bir fonksiyon çağırıp, o fonksiyonun davranışını dışarıdan verdiğimiz parametre ile değiştirebiliriz. Bu sayede de if/else gibi durumlardan kısmen kurtulmuş oluruz.

Fonksiyona geçdiğimiz parametre ile akışı değiştirebiliriz dedik. Bu akışı 2 şekilde değiştirebiliriz:
- State: state objesini fonksiyona geçeriz. Ve şu andaki durumu fonksiyona bildirmiş oluruz. Fonksiyon zaten kendi bu state'e göre aksiyon alacaktır.
- Strategy: Strategy objesini fonksiyona geçeriz. Fonksiyon bu strategy nesnesindeki fonksiyonları çağıracaktır. Dolayısı ile fonksiyonun algoritmasını değiştirmiş olacağız.

State pattern'e bir örnek verelim: Bir arabamız var. Client olarak, sadece bu arabanın gaza bas, direksiyonu döndür gibi metodlarını çağırabiliriz. Fakat bu fonksiyonlar ancak arabanın durumuna göre işleyebilecektir. İşte bu durum state'tir. Örnek kod:

```java
CarState state = new FerrariCarState();
car.setState(state);

state.setFuel("%0");
car.run(); // car will not run. "car" is the "context" role in this pattern.

// we can change the state on runtime on the same thread...
state.setFuel("%80");
car.run(); // car will run.

state.setFuel("%99");
state.setLocked(true);
car.run(); // car will not run.

// the code(algorithm) which decides to run depending on the fuel and other condition is inside "car" object. The conditions are inside the state object.

// State objesi sadece integer benzin, boolean lock, gibi field'ları barındırmıyor. FerrariCarState'inin getFuel metodu ile BmwCarState'in getFuel'i çok farklı çalışıyor olabilir. Birinde yedek benzin deposud olabilir. Yani; farklı state implementasyonları olabilir ve bunlar fonksiyonları override ederek yönetiliyor. Zaten eğer farklı implementasyon olmasaydı, basit bir fonksiyona DTO parametresi geçmekten farkı olmayacaktı bu pattern'in.
```

State için gerçek yazılımsal bir örnek:

```java
DocumentState state = new HtmlDocumentState();
mailAttachmentService.setState(state);

state.setDraft(true);

mailAttachmentService.sendMail(); // document will not be attached. Because it is draft yet.
```

State için gerçek yazılımsal farklı bir örnek:

```java
CustomerState state = new VipCustomerState();
creditService.setState(state);

state.setTotalMoneyInBank("9999999 $"); // the customer is very rich.

creditService.requestNewCredit(); // bank will give credit to that customer. Because he is rich. He has quarantee to pay the money back.
```

Strategy pattern'de de sınıfların rollerinin isimleri aynıdır:

```java
CreditStrategy strategy = new VipCustomerCreditStrategy();
creditService.setDecisionStrategy(strategy); // "creditService" named as "context".

creditService.requestNewCredit(strategy, "9999999 $");

// CreditStrategy includes the decision algorithm/logic.
```

# Template method pattern
template kelime anlamı: şablon

Spring'deki RestTemplate, KafkaTemplate, JdbcTemplate gibi "template" prefix'li sınıfları bu pattern'i baz almaktadır.

JdbcTemplate'i ele alalım. Connection açar, sql execute eder... Bolca tekrarlı/boilerplate kod vardır. Bunları bizim için template sınıfımız template method'lar aracılığı ile halleder.

# Template vs Command vs facade
ki pattern'de çok benzerdir. Fakat amaçları tamamen farklıdır. Amacı gereği command pattern'de response yoktur.

Facade kompleks API'leri basitleştirmek için API sunarken, template aynı işi yapacağımız kod bloklarını(boilerplate'leri) ortak template sınıfında toplayabilmemizi sağlar. Yani; template sınıfı birçok farklı class'tan çağrılırken, Facade sadece 1 client tarafından çağrılabilir.

Command ise amcı asenkron olarka işletilebilecek dönüşün olmaması gerektiği servisler (API'ler) için kullandığımız bir terimdir.

# Template method pattern vs ( state pattern vs strategy pattern )
Template pattern'de compile time'da class seçimi söz konusudur. Oysa state pattern ve strategy pattern runtime'da class seçimi söz konusudur.

# provider pattern
strategy pattern ile aynıdır. Provider sınıfı bir başka sınıftan çağrılmak üzere hazırlanmış config ve metodlar içerir. Örnek kullanım: java.security.SecureRandom sınıfının constructor'ı parametre olarak Provider alıyor. Her provider farklı kaynaklardan data çekmeye yarıyor. kaynak: (source-id: 466) title: "SecureRandom Number Generation Algorithms".

# Specification pattern

```java
Spec vip_specification = new VIP_Product_Spec();
Product product = productRepository.getUsersBasket();
boolean isValidForThisUser = vip_specification.isSatisfied(product);
```

Yukarıdaki gibi spesifikasyonlar yaratıp, business logic'lerimizi tek bir noktadan karar verebilmemizi sağlar. Specification sınıfımızın mutlaka birden fazla implementasyonları olmak (veya ilerde olacak) zorunda. 

# Specification object as parameter
Specification objelerimizi parametre olarakta yollayabiliriz. örneğin; repository'den data çekerken filtreleme amaçlı da kullanabiliriz. örnek:

```java
Spec vip_specification = new VIP_Product_Spec();
productRepository.filterBySpec(vip_specification);
```

## Composite Specification
birden fazla isSatisfied metotunu birlikte kullanabilmemiz (or, and...) üzerine kurulu bir pattern'dir.

# Wrong usage of this pattern
Bir web servisimize istek gelsin. customer-id'ye göre bir sınıfı execute edelim. Bu durumda Specification pattern'i implmenete etmeye çalışalım. Kod örneği aşağıdadır.

Fakat buradaki örnek Specification pattern değildir. Çünkü Specification'ımızın bir farklı bir implementasyonu yoktur veya ilerde planlanmamaktadır. Bu durumda; ServiceSpesification sınıfımız standart bir "util" veya "service-provider" veya "service-factory" tarzı bir sınıf olmaktadır.

```java
class Webservice{

    public void httpPost(int customerId){

        ServiceSpesification spec = new ServiceSpesification();
        IService service = spec.where(customerId);
        service.start();
    }
}

class ServiceSpesification{

    private Map<Integer, IService> map;

    ServiceSpesification(){
        map = fetchFromDatabase();
    }

    public IService where(int customerId){
        map.get(customerId);
    }
}

public class IService {
    public void start();
}

public class ServiceBmw implements IService {
    public void start(){
        
    }
}

public class ServiceFerrari implements IService {
    public void start(){
        
    }
}
```

# Real Implementation of this pattern
Spring-Data bu pattern'i burada uyguluyor:

The source code of Spring Framework:

```java
package org.springframework.data.jpa.repository;

import java.util.List;
import java.util.Optional;

import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;
import org.springframework.data.jpa.domain.Specification;
import org.springframework.lang.Nullable;

public interface JpaSpecificationExecutor<T> {

  List<T> findAll(Specification<T> spec);

  Page<T> findAll(Specification<T> spec, Pageable pageable);

  List<T> findAll(Specification<T> spec, Sort sort);

  // other methods...
}
```

We implement that repository:

```java
interface ProductRepository extends JpaRepository<Product, String>, JpaSpecificationExecutor<Product> {
}
```

We define our custom specification class:

```java
private Specification<Product> nameLike(String name){

  return new Specification<Product>() {

    @Override
    public Predicate toPredicate(Root<Product> root,
                                  CriteriaQuery<?> query,
                                  CriteriaBuilder criteriaBuilder) {

      return criteriaBuilder.like(root.get("productName"), "%"+name+"%");
    }
  };
}
```

The same code can be written using Lambda expression:

```java
private Specification<Product> nameLike(String name){
  return (root, query, criteriaBuilder)
  -> criteriaBuilder.like(root.get("productName"), "%"+name+"%");
}
```

Also we above code can be define as inline:

```java
Specification<Product> nameLike =
      (root, query, criteriaBuilder) ->
         criteriaBuilder.like(root.get("productName"), "%"+name+"%");
```

And we use it:

```java
List<Product> products = productRepository.findAll(namelike("book"));
```

# Specification vs State
Client taraftan kullanımları incelendiğinde çok benzerdirler:

```java
// specification pattern usage by client:
service.action( specification );

// strategy pattern usage by client:
service.action( strategy );
```

Fakat Specification, predicate'ler ile çalışır ve kendisi algoritma içermez. Oysa strategy'nin kendisi algoritma içerir. Yani action'ın nasıl bir execution süreci yürüteceğinin bilgisini tutar. Oysa Specification'daki predicate'ler sadece seçim (filter) gibi işlemler için kullanılır.

Specification, predicate kullandığından dolayı "composite predicate" yapılabilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Memento Pattern (or Token pattern)
Bir class'ın her durumunu saklamak için kullanacağımız bir pattern'dir. Örneğin text editor geliştiriyoruz. Undo, redo yaptığımızda, hem cursor'un pozisyonu hem de text'in kendisi değişmektedir.

- Memento: Saklamak istediğimiz nesnemizin tamamını ya da bir kısmını tutan sınıftır.
- CareTaker: Memento'ların (saklanan nesnelerin) referansının tutulduğu sınıftır.
- Originator: Değerleri tutulacak olan ve önceki değerlerine geri dönebilen sınıftır.

```java
TextMementoCollection coll = new TextMementoCollection(); // TextMementoCollection --> CareTaker

TextEditor editor = new TextEditor(); // TextEditor --> Originator
editor.type('111');
editor.type('222');

TextMemento saved1 = editor.save(); // TextMemento --> Memento
coll.add(saved1);

editor.type('333');

editor.restore(saved);
// or
// editor.restore(coll.get(0));
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Chain Of Responsibility
Java servlet'ler buna en iyi örnektir. zincir gibi kimlik doğrulama, loggining, compression gibi birçok işlem zincirleme yapılır ve herkesin sorumluluğu farklıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Thread pool pattern
İlgili uygulamada thread sayısının merkezi bir modülden limitlenerek sırası ile çalıştırılma pattern'idir. Merkezi modülümüz isterse thread'leri önceden işletim sisteminde hazır tutabilir, yada işi biten thread'leri kapatmadan, sıradaki thread'e direk teslim edebilir. Fakat bu detaylar pattern'de opsiyoneldir.

Gerçek kullanım örnekleri:
- birçok iletişim protokolünün client'ında (HTTP Client gibi) thread pool kullanılabilir.
- ORM client'larında thread pool kullanılabilir.
- Scheduler'larda (Örnek framework: Quartz) thread pool kullanılabilir.

# Object pool pattern
Thread pool pattern'in atasıdır. Thread yerine Obje, yani herhangi bir şey tuttuğu için çok genel bir pattern'dir.

JVM'in en sık kullanılan tamsayıları önceden hazır tutması gibi birçok pooling yöntemi, 'Object pool pattern'e güzel bir örnek olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Decomposition patterns: "Decomposition by domain capabilities" vs "Decomposition by business capabilities"
"Decomposition by domain capabilities" bazı kaynaklarda "__Decomposition by subdomain capabilities__" olarakta geçmektedir.

Hibrit olan bir yazılımı, parçalara bölme yöntemleridir. Örneğin bu pattern'lerde birer decomposition pattern'dir:
- Sidecar pattern
- Service per team
- Self-contained Service

Bir hibrit uygulamayı mikro parçalara bölmek için ya domain bazlı yada business işleyişine göre böleriz.

- # Decomposition by domain capabilities

DDD'de bahsedilen pattern'dir. "Decomposition by business capabilities"'ın tersidir.

Aşağıdaki listede * ile başlayanlar bizim domain'imiz. Altında da bu domain'de sağladığımız hizmetler mevcut. domain yada altında yazan hizmetlerimiz ayrı microserviser olabilir veya olmayabilir. Fakat 1 yıldız'ın tümü bir grup olarak bağımsız çalışabilecek şekilde tasarlanmalıdır.

```
* IT
- network team
- software developer team
- mobile-device provider team
- laptop provider team

* Administrative Management
- Human resource
- table/chair provider
```

- # Decomposition by business capabilities

```
* provider
- mobile-device provider team
- laptop provider team
- table/chair provider

* IT
- network team
- software developer team

* Administrative Management
- Human resource
```

Not: Diğer bir örnekler: şunlar olabilir:
- Her birimin kendi 'Insan Kaynakları' mı olacak (Decomposition by domain capabilities), yoksa 1 insan kaynakları tüm birimlere eleman mı sağlayacak (Decomposition by business capabilities).
- Her birimin kendi yazılım ekibi mi olacak yoksa tek bir yazılım birimi, tüm diğer birimlere mi hizmet verecek?
- Tek bir DevOps ekibimi tüm projelerin deployment'larını yapacak? yoksa her projenin kendi DevOps ekibi mi olacak?

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Sidecar pattern (or sidekick pattern)
Bu bir çeşit decomposition pattern'dir. Çünkü birçok işlevi asıl uygulamamızdan ayrıştırmış oluruz.

Bir uygulama yanında sidecar dediğimiz farklı bir uygulamayı bulundurur. Sidecar, asıl uygulamamızın yapması gereken bazı işlevleri onun için yerine getirecektir. Bunun avantajını direk örnek üzerinden inceleyelim:

Spring Cloud Eureka (discovery server) client'leri Java için yazılmış durumda. Peki cloud'da NodeJS microservisimiz nasıl Eureka'ya bağlanacak. işte böyle bir durumda sidecar pattern aklımıza gelir.

- Yeni bir microservis yaratırız.
- Spring-cloud-netflix-sidecar bağımlılığını ekleriz.
- sidecar projesinin config dosyasına da NodeJS'in healt-check URL'sini verir.
- NodeJS uygulamamızın bir portundan sürekli olarak healtcheck (Spring cloud doc'unda standardı yazıyor) status OK cevabı döneriz.

Proje ayağa kaldırıldığında sidecar sürekli olarak healcheck'e request atacak ve cevap aldığı sürece asıl Eureka server'ımıza register olacak. sidecar Eurekaya register olması için NodeJS'e aracılık etmiş oluyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# database per service pattern
her microservisin kendi database'i olması mimarisidir. temel olarak şu tarz alt çözümlere gidilebilir:

  - Private-tables-per-service – each service owns a set of tables that must only be accessed by that service
  - Schema-per-service – each service has a database schema that’s private to that service
  - Database-server-per-service – each service has it’s own database server.

tablolar arası ilişkilerin nasıl tutulacağı konusu için çözümler aşağıdadır. aşağıdaki listedeki her 2 durum içinde ilişkiler database tarafında tutulduğunda, kolonlarda foreign key olamayacak.

  - 1- ilişki tablosu sadece 1 veritabanında olacak.
  - 2- ilişki tablosu ilgili tüm servislerde tutulacak.

Yukarıda 1'inci durumda bir servis eğer ilişki tablosunu içermiyor ise; her defasında ilişki tablosunu kullanan microservice istek yapacaktır. bu da network yavaşlığına sebep olacaktır.

2inci maddede ise, ilişkiler ilgili servislerin veritabanlarında olacağı için 2 dezavantaj var:
  - dublicate veriler oluyor (ilişkiler birçok veritabanında saklanıyor)
  - update ve add işlemleri tüm veritabanlarında güncelleniyor. bu yavaşlık sebebi.

avantajı ise:
  - 1inci çözümde olduğu gibi her ilişki tablosuna erişim gerektiğinde diğer servislere istek yapılmıyor. direk database'den okunuyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Shared database
database per service pattern'in tersidir. Birden fazla servisin ortak DB kullanmasıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# microservice pattern (or mikroservis deseni) vs Monolithic pattern (or monolitik desen) vs moduler monolith
Monolith kelime anlamı: tekparça, bütüncül.

microservice'lerde bağımlılıkların neredeyse hiç olmaması (veya minimum olması) gerekiyor. bu da ortak kod bloklarının microservisler arasında hiç olmamasını gerektiriyor. genelde kullanılan ortak kod bloklarını gruplandırarak incelersek;

- database model

  her microservis kendi database'sine gitmesi gerekir. örneğin user ve adres modellerimiz olsun. her servis kendi database'sinde verileri ve ilişkileri tutmalıdır. İlişkiler arası bağlantı kurmak gerekirse, diğer servise runtime sırasında sorgu atması gerekecektir.

  bu konu "database per service pattern" başlığında daha detaylı anlatılıyor.

- utility kodları

  bir borsa uygulaması düşünelim. para çevrimi aşaması neredeyse her servis tarafından kullanılır. klasik mimarilerde olsa common sınıfa utility olarak yazılır ve herkes bu kod parçasına depend olur. oysa microservislerde 'money-converter' isimli bir servisimiz olur ve herkes runtime sırasında bu servise sorgu atarak para çevrimi yapması beklenir.

- 3party dependencies

  tabiki Apache-common-utils gibi servisleri her projemizde kullanabiliriz.

- config strings

  config'ler farklı bir config micro-servisinden okunmalıdır.

- common Maven configs + common Maven dependencies + common codes

  runtime sırasında olmayan konfigürasyonlardır pom.xml gibi config dosyalarında tanımlanır. bunları standart tutmak istiyorsak, template'ler haline getirip, bu template'leri hızlıca kopyalanabilir şekilde ayarlayıp, her projeye ayrı ayrı atabiliriz. bu konu "Microservice chassis" pattern'i başlığında anlatılıyor.

  bu son madde için bazı developer'lar common dependency yapılabilir olduğunu düşünüyor. bu konuda farklı görüşler var. burada görüş ayrılığı olmasının sebebi şu: business logic olan kısımlarda bağımlılık olmazsa genelde sorun olmayacağı düşünülür. çünkü business logic olmayan kodlar teknoloji için gerekli utility sınıflarıdır. aynı Apache-commons-util'de olduğu gibi. dolayısı ile bizde kendi şirketimiz içinde bir utility yazmış sayılıyoruz.

  shared lib olabilmesi için örnek: (source-id: 153) "Exceptions" başlığı

  Bazı developer'lar ise kod dublikasyonu olsa bile common lib yapılmaması görüşünde. kaynak: (source-id: 154) "Do you share anything?" başlığı.

  Bu konu burada da cevaplanmış: (source-id: 155)

  Fakat genel olarak; teknik ortak kodların olmasında pek sakınca görülmezken, business logic olan kodların kesinlikle ortak olmaması öneriliyor.

# avantajlar

- bir projede bir kütüphane değiştirildiğinde diğer kütüphanelerin zarar görmediğinden emin olabilir. örneğin; SpringMVC kullanırken aynı projede, Spring'in farklı bir modülünü ekleyeceğiz. yeni kütüphaneyi eklerken SpringMVC dependency'leri ile çakışmıyor görünse de, her zaman beklendiği gibi olmayabiliyor. bu durumun önüne geçilmiş oluyor. Docker gibi her proje/yazılım birbirinden bağımsız olmuş oluyor.

- her proje farklı dillerde yazılabiliyor.

- projelerde aynı kütüphanenin farklı sürümleri kullanılabiliyor

- projeler çok ufak ve fazla parçalara bölünmüş olacağından; deploymentlar daha ufak parçalar şeklinde ve daha az riskli şekilde yapılabilecektir. yani; tüm paketi birden çıkmak zorunda kalmıyoruz. continuous delivery'yi kolaylaştırıyor.

- projeler çok ufak ve fazla parçalara bölünmüş olacağından; doğası gereği modülerliği arttırmaya yöneltiyor.

- bir modülde sorun çıkınca tüm sistem çökmemiş oluyor. sadece o modül kullanılamıyor.

- update işlemleri için tüm sistemi durdurmaya gerek yok.

- lokalde development için tüm sistemi açmaya gerek yok.

- operasyonel işlerde startup süreçleri hızlanır.

- devre dışı bırakılacak modüller için sistemi kapatmaya gerek yok. sadece o servisleri kapatmak yeterli.

# dezavantajlar

- her projenin farklı database'si olması, özel sorgular ve atomic işlemler için zorluk çıkarabiliyor (transactional işlemlerdeki zorluklar)

- Complex communication:
  - network ağı sürekli haberleşme ihtiyacı arttığından dolayı yoruluyor
  - hangi business akışında, hangi servisin nereye gittiğini bulmak ve bir servisteki değişikliğin, diğer hangi serviste etki yaratabileceği ancak E2E testlerinde görülebiliyor.

- mikroservis, monolitiğe göre çok sonradan çıkan bir pattern. dolayısı ile şu anda piyasada olan birçok uygulama monolitik. bunları mikroservis yapısına geçirmek çok zorlu bir süreç gerektirebiliyor.

- Infrastructure requirements. tüm servisleri (özellikle farklı data center'lar var ise) manuel yönetmek çok zor. bunun için çoğu proje Kubernetes/Openshift/Istio gibi çözümlere para vermek veya depend olmak zorunda kalıyor. Aynı zamanda bunları yönetecek bilgi sahibi birilerine ihtiyaç duyuluyor.

- daha tecrübeli yazılımcı ihtiyacı doğuruyor:
  - dependency olmaması gerektiğinden, tecrübeli yazılımcılar olmazsa, dublicate kodların sonucunda code smell'ler oluşabiliyor.
  - genel olarak bakıldığında; daha fazla infrastructure requirement bilgisi gerektiriyor. en basitinden lokal ortamda bağımsız development yapmak için bile Kubernetes kaldırmak gerekebiliyor.

- iyi optimize edilmezse, bazı durumlarda sistem requirement'leri (RAM gibi...) daha fazla harcayabiliyor (özellikle ufak projelerde dezavantaj yaratıyor).

# cloud native architecture
bir mimaridir. "native", doğal anlamına gelir. 'cloud native'den kasıt; cloud yapısına en uygun olandır.

Spring'in cloud native'i karşılayan modülleri vardır. "Spring cloud netflix" ismi ile geçer çünkü tüm implementasyonlar Netfix'in Spring'den bağımsız geliştirdiği uygulamaları kullanabilmek için yapılmıştır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# SOA (or Service-oriented architecture) vs web service
SOA kabaca bir mimarisel paradigmadır. Bu paradigma; birden fazla/farklı web servisini entegre ederek (veya gruplayarak) tek bir sistem gibi işlemesini sağlamaya dayanır. Her servis direk veritabanına erişirse, birçok servis aynı anda aynı veritabanında işlem yapmaya kalkar. Fakat bunun yerine, servis tabanlı bir yaklaşım ile her business işleten servis kendi işini arka planda yapmalı ve veritabanındaki değişikliği yapmalıdır.

Bu açıdan bakıldığında, biraz microservis mantığına benzer bir yapı vardır. Fakat SOA'nın temelinde microservisler gibi bağımsız uygulama geliştirme, scallable gibi fikirler yatmaz. SOA, biraz daha enterprise ve yüksek seviyede bir yaklaşım biçimidir. kaynak: (source-id: 463) 1st Footnote, last sentence.

web servis ise, web teknolojileri kullanarak, dışarıya API açarak, hizmet veren servislere verilen genel bir terimdir. web servis bir yaklaşım değildir, bir teknolojidir.

SOA paradigmasını birçok teknoloji ile gerçekleyebilir (implemente edebiliriz):
- web servisler
- CORBA
- REST
- gRPC
- EJB
- SOAP

# Enterprise application integration (or EAI) vs Enterprise Integration Patterns (or EIP)
monolitik uygulamalar'ın birbirleri ile haberleşebilmesi için gerekli entegrasyonlardır. Bu terim çok genel bir kümeyi kapsadığından, tam olarak neleri kavradığı belli olmayan (resmi şekilde yazılı olmayan) fakat piyasada sıkça kullanılan bir terimdir.

EAI pattern'lerini implemente etmemize yarayan bazı framework'ler:
- Spring Integration
- Apache Camel
- Red Hat Fuse
- Mule ESB
- Guarana DSL

Gregor Hohpe ve Bobby Woolf, '__Enterprise Integration Patterns (or EIP)__' için gerekli 65 pattern'i "Enterprise Integration Patterns" isimli kitapta anlatmıştır. EAI denildiğinde piyasada genelde bu kitabı referans alarak makale yazmaktadır. Bu kitaptaki pattern'lerin özetini burada liste halinde bulabiliriz: (source-id: 156). '__enterpriseintegrationpatterns.com__'a Apache Camel'ın resmi sitesinden referans verilmiştir. kaynak: (source-id: 157) 1st sentence.

Bu kitapta pattern'ler, '__enterpriseintegrationpatterns.com__'de listelendiği gibi tek tek ayrı başlıklarda ele alınmaktadır.

Bu kitaptaki bazı pattern'ler:
- File Transfer
- Shared Database
- Remote Procedure Invocation
- Point-to-Point Channel
- Publish-Subscribe Channel
- Message Bus
- Synchronous (Web Services)
- Kitapta anlatılan birçok pattern burada anlatılıyor: [file:"pattern_enterprise_integration.md"](./pattern_enterprise_integration.md)

# service bus (or enterprise service bus or ESB)
ESB birçok monolitik uygulamanın birbiri ile haberleşebilmesi ve entegrasyonunu sağlamak için merkezde olan bir component'tir. ESB'ler ek olarak, mesaj formatını değiştirme, filtreleme, yönlendirme, bazı business logic içerme gibi özellikleri de vardır.

Bu şekil ESB'yi güzel şekilde açıklamaktadır: (source-id: 462)

"Enterprise application integration" içinde önerilen mimarilerden biridir.

ESB'nin dezavantajları:
- Adding additional step/call in the service flow and add a slight delay to the response time.
- Additional cost to setup ESB servers, VPNs, and other infrastructure
- All services are routed through a single place; it will have a single point of failure.
- Not having too much options to scale up a single service in ESB infrastructure.

# ESB vs SOA vs microservice vs Monolithic
tam olarak birbirlerine alternatif olmasalarda, karşılaştırma yapılabilir. Fakat içiçe de kullanılmaktadırlar. Örnekler;
- ESB kullanıp her servisimizi micro seviyede yapabiliriz. Fakat tam olarak microservis yapamayacağız, çünkü ortak/merkezi olan bir service bus kullanmış olacaklar.
- Zaten ESB, SOA olan bir ortam için tasarlanmıştır.
- Sadece SOA kullanıp, ESB olmayan bir ortamda servislerimiz çok ufak ve scallable yaptıkça, microservis ortamını hazırlamış olmaktayız.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Externalized configuration
config server pattern'idir. servislerin tek bir servisten tüm configleri çekebilmesidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# API gateway
bir sunucu yazılımdır. edge server gibi çalışır. özellikleri:

- Farklı protokoller arası dönüşüm. örnek: SOAP->REST, TCP->SOAP
- darboğaz (throttling) yönetimi. her user'dan gelen istekleri sınırlandırmak. yani; belli zamana dilimlerinde, bazı isteklere limit getirebilmek.
- kimlik doğrulama
- API versiyonlama
- API grubu devreye alım/kapatma/açma
- log'lama, izleme, bildirim
- Servis birleştirme (service composition). birden fazla iç servisi tek bir servis olarak dışarıya açabilme.
- caching

piyasadaki bazı API gateway'ler: tyk, Kong

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# API Composition
API gateway uygulamaları bunu bizim için yapabilir. edge proxy diye adlandırılan bu sunucular API'lerimizi dışarıdan erişime açarken tek bir endpoint'ten açar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# CQS (or Command Query Separation) vs CQRS (or Command Query Responsibility Segregation)
Segregation kelime anlamı: ayırma

ikisi çok benzerdir fakat farklı pattern'lerdir.

2 pattern de metotları iki farklı gruba alınmasını önerir:

- Commands: Objenin veya sistemin durumunu değiştirir. Void dönüş yapar.

  'command' yerine modifier veya 'mutator' terimleri de kullanılır. kaynak: (source-id: 158)

- Queries: Sadece sonucu geriye döner herhangi bir objenin veya sistemin durumunu değiştirmez.

Bir command metotu (eğer dönüş yapmasının yan etkisi olmayacaksa), dönüş yapabilir. fakat bu pek tercih edilen bir yöntem olmamalıdır.

CQS daha çok mikro seviyede, CQRS  ise makro seviyede sistemi ele alan pattern'lerdir. CQS büyük resimden bakıldığında sistemdeki herhangi bir fonksiyonun ya query yada read işlemi yapmasını önerir. kaynak: (source-id: 158)

oysa CQRS (büyük resimden bakarak) bunların da ayrılmasını önerir:
- fonksiyonlara yollanan ve response alınan modeller/objeler
- microservislerin sadece query veya command olarak ayrılması
- sadece fonksiyon seviyesinde değil, sınıflar bazında ayrılması (yani; bir sınıftaki tüm metotlar ya query yapacak yada command yapacak)

kaynak: (source-id: 160) Luke Puplett'in cevabında açıklama mevcut. Mesajın altında Greg Young cevap yazmış fakat Greg web projelerinde kullanımı hakkında yorum düşmüş.

CQRS ilk "Greg Young" tarafından ortaya atılmıştır.

## query ve command'lar için DB modellerinin ayrılması
Bu durum "Materialized View pattern" başlığında da anlatılmıştır.

DB modellerimizi command ve query olarak ayıramaz isek, ve bu sistemde CQRS uygulamaya çalışırsak, sadece servislerimizi command ve query olarak gruplandırmış oluruz. Fakat bu sistem scallable olmaz. Örneğin; bu sistemde sadece command servislerimizi scale etmek istediğimizi düşünelim. DB server instance sayımız aynı olacağı için (çünkü DB modellerimizi ayıramamıştık), CQRS'in faydasını görmeyeceğiz. Çünkü darboğaz olarak DB karşımıza çıkacaktır. Fakat eğer command ve query'lerimiz ayrı modeller olursa, entity'leri ayrı DB'lere kaydedebiliriz. Her servise (query ve command servislerine) ayrı (bağımsız) çözüm uygulayabiliriz. örneğin; sadece query microservislerimizi scale ettikten sonra, sadece query yapacağımız DB instance'larını da scale edebiliriz. Oysa tersi durumda; tüm sistemi scale etmemiz gerekecekti.

CQRS bir sistemde kesinlikle query ve read modelleri ayrı olacak diye bir kesinlik yoktur. kaynak: (source-id: 161) 9uncu paragraf.

## View Models (or Projections or Query Models)
Query fonksiyonlarımız data okur (veritabanından). bu data'lar yansıtılmak için kullanılan bilgiler olduğu için, CQRS pattern'i dökümanlarında bu şekilde isimlendirilmiştir.

Bu pattern'in faydaları:
- CQRS yapıldığında "event sourcing"'de kolaylaşacaktır. çünkü sadece "Commands" servisimize gelen istekleri log'lamamız yeterli olacaktır.
- Bazı servislerimiz sadece database'ye read yetkisi olduğundan o servislerimizde güvenliği arttırmış oluruz. (secure by design)
- Sistemimizi micro servislere bölüştürmemiz kolaylaşacaktır.
- Read only query metotlarımız, yazma işlemi yapan metotlarla aynı sınıflarda olmayacağı için büyük DAO'lar karşımıza çıkmayacak. Çünkü command modeli ile view model'leri aynı olmak zorunda değil.
- Database'yi değiştiren servislerin hangileri olduğunu bildiğimizden hata tespiti kolaylaşabilir.
- read ve write yapan DB'ler aynı olmak zorunda değil. Zira write yapıldıktan sonra async bir thread'de de farklı bir DB'ye raporlama için (query servislerinin yararlanması için) kayıt atılabilir. Dolayısı ile; query ve command'lerin kullandıkları database'ler farklı. bunun da sonucunda; farklı database çözümleri sunabiliriz. örneğin; kayıt command'ler kayıt atarken MongDB kullanıp, raporları okurken Oracle kullanmalarını performance açısından verimlilik sağlayacak ise bu şekilde kullanılmasına olanak sağlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Materialized View pattern
Bazı durumlarda okuma yaptığımız ve yazdığımız dataları ayrı yerlerde tutarız. Bu duruma Materialized View pattern denir. Bu pattern'i aşağıdaki pattern'leri implemente ederken kullanabiliriz (source-id: 475):
- Event Sourcing pattern
- CQRS pattern

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# two-phase commit (or 2PC)

database/network sistemlerinde transaction işlemi yapacak yazılım karşıya önce "prepare for commit" mesajı iletir. eğer OK dönüşünü alırsa "commit" mesajını, her servise ayrı ayrı gönderir. Bu yaklaşım genelde uzakta birden fazla birbirinden bağımsız veritabanlarında işlem çalıştırılacağı zaman yapılır. böylece atomic işlem daha garantiye alınmış olunur.

2PC'de yazılım, diğer servise yada veritabanı servisine "prepare for commit" mesajını atması ve dönüşünü alması fazına __voting phase__ denir.

Klasik sistemlerde ise direk olarak ilk işlemde "commit" mesajını gönderilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# saga pattern
Türkçe kelime anlamı: destan

Saga pattern önyüzde de kullanılan bir pattern'dir. örnek Redux-saga; saga'larımızın yönetimini sağlar. Saga'lar senkron gibi gözüksekde, redux-saga framework'ü arkaplanda her yazdığımızı adım adım çağırır.

Saga, 2PC'ye altelnatiftir.

2PC'de global transaction manager olmak zorundadır. 2PC'de, eğer transaction herhangi bir adımda fail ederse, tüm işlem ignore edilir ve DB'ye hiçnir şey yazılmaz. Bu sebeple; özel bir revert işlemi yapmaya (işletmeye) gerek yoktur. Oysa saga'da her bağımsız servisin yaptığı işlemin revert işlemi de olmak durumundadır. Bu şekilde bir işlem fail ettiğinde, o ana kadar saga'nın çağırdığı her işlem için, revert işlemleri (compensating (telafi)) çağırılır. Bu şekilde tüm sistemlerde global bir transaction manager'a ihtiyaç duyulmaz. Tabi böyle olunca tüm sistemdeki kaynaklar (örnek: DB tabloları) lock'lanmasına gerek kalmaz. kaynak: (source-id: 162) "Two-phase commit (2PC) pattern" balığında ikinci resimde görülmektedir ki, bir işlem abort edildiğinde, ilgili servise verilerin veritabanına kayıt edilmemesi isteği yollanmakta. Diğer kaynak: (source-id: 164) "All transactions in the sequence complete successfully or compensating transactions are ran to amend a partial execution." yani; compensating (telafi) transaction'ları yine sequence içerisinde tanımlanır.

2PC senkron bir çözümdür. saga asenkron ve reaktiftir. saga'nın asenkron olabilmesi için, tüm servislerin ortak bir event bus'a bağlı olmaları gerekir. bu şekilde saga, global yürüyen transaction sürecinden haberdar olabileceklerdir. kaynak: (source-id: 162) "saga pattern" başlığındaki ilk paragraf.

# long-lived transaction (or LLT or long-running transaction)
saga, LLT yönetimi için kullanılır. Çünkü transaction sürerken, DB lock'lanmış şekilde beklemektedir. kaynak: 'saga' teriminin ilk ortaya atıldığı belge: (source-id: 166)

# disadvantages of saga
- saga'nın debug'ı, 2PC'e göre daha zordur. kaynak: (source-id: 162) "saga pattern" başlığındaki ilk paragraf. "Disadvantages of the Saga pattern" başlığı.

# advantages of saga
- saga'nın reactif olması bir avantajdır. 2PC'de işlemler bitene kadar thread'imiz açıkken, saga'da işlemler çağrılıp thread'ler sonlanmaktadır.

# disadvatages of 2PC
- 2PC'de bir transaction sadece bir servisin kendi içinde (seviyesinde) değil, tüm servisler (sistem) seviyesinde kullanılır. dolayısı ile transaction'ın başladığı andan, bittiği ana kadar veritabanında bazı değerler kilitli kalacaktır. bu durum saga'da böyle değildir. çünkü saga'da; 'transaction' sadece servislerin kendi içlerinde(seviyelerinde) gerçekleşecektir.

- 2PC'de kaynakların (örnek veritabanı) kilitlenmesi, 2PC'nin reactif manifestosuna uymadığını gösterir. kaynak: (source-id: 168) "The Saga Pattern in a Reactive Microservices Environment", authors: "Martin Stefanko", "Ondrej Chaloupka", "Bruno Rossi", title: "INTRODUCTION" başlığında ikinci paragrafın ortasında.

# Types of saga
saga ile 2 farklı şekilde yönetilebilir (implemente edilebilir):

- __Choreography (or tr:Koreografi)__

  bazı kaynaklarda "event yöntemi" olarak adlandırılıyor. fakat bu çok doğru bir terim değil.

  Koreografi kelime anlamı: Bir dans gösterisini oluşturan adım, figür ve anlatımların bütünü.

  her event diğer event'leri tetiklemesi için event bus'a mesaj gönderir. dolayısı ile tüm saga süreci, merkezi bir servisten yönetilmez.

- __Orchestration__

  bazı kaynaklarda "command" yöntemi olarak adlandırılıyor. fakat bu terim çok doğru değil.

  merkezi bir servisten tüm event'ler çağrılır ve revert servisleri de aynı orchestrator tarafından çağrılır. Yani saga sürecinin üst seviyeli yönetimi buradan yapılır.

  Orchestrator saga sürecini yönetirken sync olmasına gerek yoktur. Çünkü event çağırır ve event yakalar. Dolayısı ile, event yakaladığında hata veya success olma durumuna göre yeni event tetiklemek orchestrator'ın görevidir.

- __hybrid__
  Yukarıdaki Koreografi ve Orchestration, hibrit olarakta kullanılabilir. Yani saga'nın belirli bir sürecinde, Orchestration gibip, ilerdeki aşamalarında Koreografi'ye geçilebilir (veya tam tersi de olabilir).

  Bir sistemde, bazı saga'larda Orchestration, bazı saga'larda Koreografi de kullanılabilir.

# backward recovery vs forward recovery
__backward recovery__, saga herhangi bir adımda fail olduğunda, sadece şu ana dek çağrılan kısımlar için __companse event__'leri çağırma yöntemidir.

__forward recovery__ ise, saga fail olduğuda, kalan kısımlarının saga'nın resume/restart edilmesi ile yapılan recovery çözümüdür. Aslında bu yöntemde retry yapılıyor ve bunun bir sınırı olmalı. Yani 5 kere "forward recovery" denensin olmazsa, "backward recovery" yapılsın (veya business'a göre hiç bir aksiyon almaya gerekte olmayabilir) denilebilir.

__forward/backward recovery__ mode ise mix bir yöntem değildir. snapshot alarak yapılan bir yöntemdir. bu yöntemde saga tümüyle restart edilmez veya resume da edilmez. saga'nın belli adımlarında sistemin state'i bir yerde saklanır (snapshot alınır). Böylece eğer saga fail olursa, ilgili snapshot'a kadar olan kısım revert edilir (backward recovery) yapılır, ve daha sonra saga devam ettirilir (forward recovery).

Bu tarz yapıları elle hazırlamak zor. Çok fazla detay programcının karşısına çıkabiliyor. Bu sebeple saga yönetimini BPM gibi araçlarla yapmak verimli olabilir.

# event-driven architecture vs saga
(source-id: 171) sayfasında event driven architecture'ın deprecated ve saga ile replace olduğu yazmaktadır. bunun sebebi şudur:

event-driven architecture bir sistem çok genel bir terim olarak kalıyor. saga ise bunun biraz daha spesifik hali olarak düşünebiliriz. fakat her event-driven bir sistem zaten saga olmak zorunda. çünkü; saga reversable işlemlerden bahseder. event-driven daha geneldir ve reversable olmasından bahsetmez. fakat pratikte reversable olmayan event'ler neredeyse hiç yoktur. bu sebeple saga; daha net ve daha gerçekçi bir şekilde ihtiyaçları karşılayan bir pattern'dir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Transactional outbox pattern
bir servis atomic olarak hem DB'ye kayıt hemde message event'i atmak isteyebilir. Bu işlemlerin atomic olduğunu garanti edebilmek için outbox pattern kullanılır. İlgili servis DB işlemlerini yapar ve aynı DB-transaction'ında DB'de farklı bir tabloya da yazar (bu tabloya "Outbox table" denir). DB'ye yazıldıktan sonra, DB'den sürekli olarak Kafka veya bağımsız bir servis event kayıtlarını okur ve işletmeye başlar. Yani direk olarak Kafka'ya değil, arada DB vardır.

2 şekilde implemente edilebilir:

- # Polling publisher pattern
  Bağımsız bir service (scheduled job) sürekli olarak outbox table'ı okur. Buradan sonra ilgili event'i Kafka'ya fırlatır.

- # Transactional Log Tailing
  DB seviyesinde transaction-log'lardaki değişiklikler takip edilir. Her değişiklikle Kafka'ya direk DB tarafından kayıt atılır.

  Bu pattern'i uygulayan yazılımlara "__Change data capture (or CDC)__" deniliyor. örnek sistemler: Debezium, Kafka Connect (with a connector).

Gerçek kullanım senaryoları:
- sending an e-mail message after placing an order
- sending an event about new client registration to the messaging system
- processing another DDD Aggregate in different database transaction. For example after placing an order, we shpuld decrease the number of products in stock.

Outbox pattern, "dual write" problemine bir çözüm olabilir. "dual write"; hem database hem Kafka'ya atomik yazmamız gereken fakat yazamadığımız durumlarda (hata aldığımızdan ötürü) görülebilen bir hatadır. tekrar aynı flow'lar, aynı request-data'sı ile çağrıldığında işlem yarım kaldığı için, sistemin tekrar benzeri kayıtları atması durumunda "dual write" oluşur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# event sourcing pattern
order tablosundan gidelim. order'ın tamamlanması için birçok aşama vardır. işleme alınma, ödeme alınma, kargoya verilme gibi... bunların her biri event'tir bu event'ler "__event store__" da tutulmalı.

böylece;
- order'ın son haline event'leri takip ederek bile ulaşabiliriz.

  not: order'ın son halini sadece query'lerimiz (CQRS pattern'indeki query'lerimiz) için bir tabloda tutarız. fakat her event sonunda bu order tablomuzu güncellemeyiz. sadece tüm event'ler tamamlandığında order tablomuzu güncelleriz.

- event-driven architecture sağlar (reactive sistemler için ideal bir yapı)

- her event ne yapacağına kendi karar verecektir. bu da daha modüler/mikroservis mimarisine geçişimizi uygun kılacaktır.

# isimlendirme
event isimler geçmiş zaman olmalı ve fiil içermelidir. uygulamanın hangi durumda olduğunu net bir şekilde açıklamalıdır. örnekler:

| Badly named     | Well named      |
|-----------------|-----------------|
| AccountInserted | AccountCreated  |
| AccountUpdated  | FeeCharged      |
| AccountUpdated  | MoneyWithdrawn  |
| AccountUpdated  | AccountCredited |
| AccountDeleted  | AccountClosed   |

# Event storming
domain expert'lerin ve developer'ların bir araya gelerek yaptığı toplantıdır. bu toplantıda event'lerin ne olduğuna karar verilir.

# domain events vs event-sourcing
domain event'leri DDD'deki aggregate'ler arasındaki haberleşmede kullanılan event'lerdir. "event sourcing" event'leri ise her aggregate'in kendi içindeki değişiklikleri tutan event'lerdir. bu 2 event çeşidi tamamen bambaşka kavramlardır.

domain event'lere örnekler:

- A user has registered
- An order has been received
- The payment deadline has expired

Domain event'leri, event sourcing event'lerine göre daha yüksek seviyelidir. Domain expert'leri tarafından anlaşılırlar. Bu sebeple teknik olmamalıdırlar. Oysa evet sourcing event'leri daha alt seviyeli ve yazılım teknik elemanları tarafından anlaşılabilirler.

bu durumu bu linkteki: (source-id: 172) "Events from Event Sourcing ≠ Domain Events" başlığındaki bu resim iyi açıklamaktadır: (source-id: 173)

Martin'in bloğunda bu geçmektedir: (source-id: 174) "Event Sourcing ensures that all changes to application state are stored as a sequence of events.". Dolayısı ile her event sourcing işlemi uygulamanın state'inde değişiklik yapmaktadır. Her değişiklik sonrası state veritabanında saklanmalıdır.

Bu konuyu farklı anlatmak gerekirse; DDD başlığındaki örnekteki kodda, Aggregate class'ımız içindeki business metodlarımızda isteğe bağlı Domain Event fırlatabilmekteyiz. Bunlar sadece opsiyonel birer olaydır. Fırlatılan event objesinde ne data olacağı tamamen opsiyoneldir. Oysa "event sourcing" event'i aggregate class'ımızın tüm field'ları için gerekli tüm value'ları aşama aşama mutlaka içermelidir. İşte tam olarak fark budur. Aslında event sourcing'deki her event ile, aggregate'imizin snapshot'ını (veya snapshot diff'lerini) tümüyle almış oluyoruz.

# snapshots
event'lerin sayısı çok artabilir ve bir command birçok event çalıştırabilir. dolayısı ile, bir her event son durumu (state'i) okumak için sürekli ilk event'in log'undan son event'in log'una kadar okuyup, hesaplama yapması gerekebilir. örneğin; aynı kullanıcı toplu transaction işlemleri üstüste yapıyor. her event; yani transaction öncesi bakiyede para kalıp kalmadığını kontrol edeceğiz. tüm event'lerin ne kadar harcadığını bulmak zorundayız. bunu yapmak yerine 10 adet işle sonrası yeni bir snapshot yaratabiliriz. bu snapshot'ta son durumun özeti olabilir. bu snapshot'ları event tablosunda da tutabiliriz yada farklı bir tabloda sadece snapshot'ları tutabiliriz.

her snapshot'un event tablosunda tutulduğu bir örnek: (source-id: 175)

Snapshot'ların ne aralıklarda alınacağı, sistemin mimarisi, uygulamanın domain'i ve performans ölçekleri gibi birçok parametreye bağlıdır. snapshot almak; hem maliyetli hem de kod mantığını ve karmaşıklığı getirdiği unutulmamalıdır.

# Compensation events
Compensation kelime anlamı: tazminat, telafi.

Revert işlemi için attığımız event'lere verilen genel isim.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Circuit Braker pattern (or devre kesici deseni)
başka başlıkta anlatılıyor (Hystrix ile birlikte anlatılıyor).

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Service registry
"discovery server" konusu altında (başka başlıkta) anlatılıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Client-side Service Discovery Pattern vs Server-side Service Discovery Pattern
her iki pattern'de de, service discovery'ye her node kendi IP'sini bildiriyor.

Client-side pattern'inde her node, service discovery'ye erişerek, diğer node'lara gideceği IP adreslerini alır.

Server-side pattern'de ise her node direk olarak sistemdeki "load balancer"'a istek yaparak gitmek istediği servise erişir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Self Registration pattern
her node'un kendini service regisrty'ye kaydetmesi gerekmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# 3rd Party Registration
Her node'un kendisini servis registry'ye kaydetmesi yerine sidecar gibi üçüncü parti yazılımların bizim yerimize kaydetmesi pattern'idir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Blue-Green Deployment Pattern
tüm mikroservisleri durdurup yeni sürüm çıkmak ve hata olma durumunda geri alınmasını beklemek çok zaman alabilir. bunun önüne geçmek için bir ortamda daha yeni sürüm sistem ayağa kaldırılır. eski sistem hala ayaktadır. yeni sistem sadece tek bir yönlendirme yapılarak canlıya alınmış olur. bu yönteme Blue-Green Deployment denir.

yönlendirme yapılmadan önce yeni ortamda happy path testler koşulması önerilir.

"green"; o anda "production" ortamını, "blue" ise "staging" ortamını ifade etmek için kullanılan renklerdir.

Netflix bu renkleri red/black olarak kullanıyor. kaynak: (source-id: 176) title: "Blue/Green Deployment".

# canary releasing (or canary deployment)
release kelime anlamı: 1.serbest bırakmak 2. gösterime sokmak/yayına sunmak

"canary" terimi yazılım dünyasında "early version of software" anlamında kullanılıyor. bunun hikayesi; kömür madencilerinin önceden ortam havasından hastalanmalarını engellemek için kanarya kuşunu kullanmaları hikayesi olan 'canary in the coal mine'dan geliyor.

canary releasing bu tekniğinde; hem yeni, hemde eski ortam ayağa kaldırılır. canlı'nın trafiği azar azar yeni ortama açılır. burada monitoring mekanizlamalarımızın çok iyi şekilde tasarlanmış olmalıdır. çıkacak hata oranlarının eskisi ile karşılaştırılması ile yenisinin trafiği yavaş yavaş arttırılır.

# Rolling release (or rolling update or rolling deployment)
'continuous delivery' terimi ile aynı kapıya çıkmaktadır. 'Continuous delivery' terimi biraz daha teknik kalırken, başlıktaki terimler ise son kullanıcı tarafından anlaşılması için kullanılan bir terimdir.

Büyük paketler çıkarak güncellemek yerine, ufak ufak güncellemeler çıkılarak gerçekleştirilen deployment'lardır.

# Release cycle
Linux gibi dağıtımlar birçok projeyi upstream olarak kullanır (Linux, Gnome...). Bu sebeple 2 türlü update yöntemleri vardır:

- __fixed release (or point releases)__:

  Eskiden neredeyse tüm sistemler bunu kullandığı için artık "__standard release__" veya "__classical release__" diye de adlandırılır.

  bazı kaynaklarda ise "__stable releases__" olarak adlandırılır. çünkü paket sürekli yenilenmediği için ve daha fazla teste tabi tutulabildiği için daha stabil olduğundan esinlenilir.

  Bu yöntemi Microsoft Windows ve Ubuntu her zaman kullandı (yıl 2023). Bir sürüm çıkarılır ve daha sonra bu sürüme güncellemeler getirilir. Güncellemeler genelde bir süre sonra sadece güvenlik açıklarını kapatmaya yönelik olur.

- __rolling release__

  Sürekli ufak güncellemeler ile ilerleyen güncellemelerdir.

Bu yöntemlerin en önemli artıları ve eksileri:
- standart release, bir süre aynı sürümü frozen edip sadece güvenlik güncellemesi geçtiği için, son kullanıcı için daha stabil çalışmaktadır.
- rolling release güvenlik için daha önde gitmektedir. çünkü, standart release OS'lar üstünde çalıştırdıkları uygulamaların (Gnome, Firefox...) güvenlik güncellemelerini direk alamıyorlar, çünkü en güncel sürümlerine yükseltmiyorlar. Bu sebeple "backport" denilen patch'leri bekliyorlar. Bu bekleyiş bazen yıllar bile alabilmektedir. kaynak: https://www.privacyguides.org/en/os/linux-overview/#release-cycle title: Release cycle.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Microservice chassis
chassis Türkçe karşılığı: şasi (Motorlu kara taşıtlarının iskelet bölümü)

her yeni mikroservis oluşturulduğunda bir framework'ten yararlanıp, cross-cutting concern'lerin (loggining, security...) tekrar tekrar yazılmaması ve bununla zaman kaybedilmemesi gerekmektedir. örnek Spring boot-starter bunun için ideal bir örnektir. Spring boot starter yerine bizde onu wrap edip common bir base dependency yapabiliriz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Service Template
Microservice chassis'te dependency olarak base bir proje kullanılırken, burada base proje direk olarak copy/paste (fork) yöntemi ile kullanılmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Self-contained service
her microservisin availability'sini arttırmak için uygulanabilecek bir pattern'dir. Order servisini düşünelim. gelen isteği senkron olarak önce güvenlik servisine, sonra product servisine, işlem tamamlanınca da raporlama servisine bilgi yollayıp olması ve sonra son kullanıcıya dönüş yapması gerekir. Diğer servisler cevap vermediği zaman bu serviste işlem yapamayacaktır. İşte bunu engellemek için şöyle çözümlere gidilebilir:
- Gelen request JWT içerirse, JWT'yi kendi açıp onaylayabilir.
- product data'sını doğrulayabilmek için product listesinin veritabanının klonunu kendinde barındırabilir.
- raporlama servislerine de hiç senkron istek atmaz. channel üzerinden asenkron mesaj bırakır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Humble Object pattern
humble kelime anlamı: alçakgönüllü.

Bazı kodlar test edilmesi çok zor olabiliyor (Örneğin GUI). Bu sebeple bu gibi durumlarda Humble Object Pattern uygulanabilir. Pattern'i uygulamak için GUI'yi farklı kod bloklarına bölmemiz gerekiyor. GUI'nin behavioral tarafını farklı bir yere atarız. Çünkü davranışları kodla test etmek kolaydır. GUI pattern'lerine bakarsak bunu yapan pattern MVVM'dir. MVVM'e göre behavioral kodlar ViewModel'dadır. Bu sebeple ViewModel bu usa case'e göre Humble Object'miz oluyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# data access object (or DAO) pattern
örneğin Employee sınıfı modelimiz olsun. bunun erişimi için gerekli aşağıdaki sınıfları bir EmployeeDAO isimli sınıfına koyabiliriz:

```java
List<Employee> findById();

List<Employee> findByName();

boolean insertEmployee(Employee employee);

boolean updateEmployee(Employee employee);
```

Bazıları pattern olmadığını, sadece "separate of logic" temeline uyduğunu belirtiyor. Bazıları ise "Structural" pattern altında olduğunu belirtmektedir.

# repository pattern
DAO sadece persistence odaklıdır ve persistence işlemlerinin ayrılmasını sağlar. "repository" ise ona çok benzerdir fakat farklı kavramlardır.

Repository, sadece aggregate root'ları yönetmek için kullanılırken, DAO direk DB objeleri manipüle eder.

Repository, "collection of objects" olarak düşünülmelidir. Yani bir collection (liste) var ve biz bu collection'a ekleme çıkarma, o collection'da arama (query) gibi işlemler yapıyoruz. Aynı ArrayList'in metotlarında yaptığımız gibi. collection tek tipte bilgi store ettiğinden, repository tek tip model üzerine odaklanır, örnek: UserRepository, OrderRepository gibi...

Repository; Eric Evans'ın DDD kitabında anlatılıyor. kaynak: "Domain-Driven Design: Tackling Complexity in the Heart of Software", 2013, page 106.

Her aggregate veya aggregate root için sadece 1 repository class'ı kullanmamız önerilir. kaynak: (source-id: 178) "Define one repository per aggregate" başlığı, 1inci cümle. İşin aslına bakıldığında; bir aggregate altındaki tüm entity'ler aggregate root'lar tarafından update edilmektedir. Yani aggregate için yazılan repository aslında aggregate root için yazılmış oluyor (aynı kapıya çıkıyor). Aggregate altındaki diğer entity'ler için ayrı repository yazılmamalı. Bu durum tüm aggregate'in consistency'si için önemli.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Front controller pattern
Spring MVC'deki dispatcher-servlet bir Front controller örneğidir. tüm request'ler buradan geçer. Bu şekilde ortak olan concern'ler (security, internalization, and providing particular views for certain user...) burada halledilir.

"Martin Fowler", "Patterns of Enterprise Application Architecture" isimli kitabında bu pattern'den bahsetmiştir. kaynak: (source-id: 179)

Burada da detaylı şekilde anlatılmaktadır: (source-id: 180)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Backend For Frontend
Back-end servislerinin önüne facade gibi diğer servisleri çağıran servisler yazılır. Bu yeni servisler sadece son kullanıcının kullandığı client'a hizmet etmek için vardır. Bu client'lar daha az servis çağırarak daha çok iş yaptırabilir ve bu servisler buna göre optimize edilmiştir. Çünkü son kullanıcının kullandığı cihazlarda pil kullanımı gibi parametreler önemlidir.

Her client çeşidi için (ATM, mobile, web...) farklı servisler yazılabilir.

Bu yeni servislerde ne gibi optimizasyonlar yapılabilir:
- client'a dönen response'ta tarih gibi data'lar formatlıdır. client'ın tekrar formatlamasına gerek yoktur.
- Client'ın parse etmesi daha hızlı olan response biçimlerinde dönüş yapabilmelidir (örneğin client ortamında JSON, XML'e göre daha hızlı parse ediliyorsa, bu servisler JSON dönebilmelidir)
- Client'ın yapacağı X HTTP isteğinden sonra %90 Y isteği de yapılıyorsa, X ve Y isteğini aynı request'te karşılayabilmelidir.
- Cache'lenebilen hard-coded data'lar birçok microservisten ayrı ayrı çekilmesi yerine, tek bir servisten sunan servis yazılmalıdır.
- NoSQL'deki gibi client'a tüm data'yı tek hamlede yollayabilmek için, response de-normalize edilmiş olabilir.
- Bazı servisler çok fazla gereksiz data dönebilir. örneğin user servisi, user'ın yaşını dönüyordur. fakat Bu ATM'yi ilgilendirmez. Bu sebeple ATM servislerine yaş bilgisi dönmemelidir. Bu şekilde network ve client'ın response parser'ı daha az yorulacaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# unit of work pattern
DB'ye toplu işlem yaparken ayrı ayrı gitmek yerine, hepsini biriktirip, tek bir istek ile tüm SQL'lerin çalışmasını sağlamak.

Bu konunun bir örneği "flush vs commit" ve "Deferrable SQL Constraints" başlıklarında anlatılıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# multitier architecture (or n-tier architecture) vs multilayer architecture (or n-layer architecture)
tier ve layer'ın kelime anlamları birbirine çok yakın (aynı denebilir, fakat belki detaylarda ayrılıyordur. tam bilmiyorum.). bu sebeple piyasada birbiri yerine kullanılıyor. 
 
- "layer", logical olarak aynı runtime'da çalışa, ama birbirinden ayrılmış iş katmanlarını temsil eder. Örneğin; business-layer, database-crud-services-layer gibi... 

  Sadece business ve database-crud-services olan bir yazılım 2-layered yazılımdır.

- "tier" terimi fiziksel olarak ayrı olan katmanları temsil etmek için kullanılıyor. Örneğin; UI-service, user-service...

# three-layer architecture
multilayer architecture'da, n adet layer olabilir. Tabi her projede ne kadar sistem olacağına bağlı değişebiliyor bu durum.

Piyasada en sık örnek verilen __multilayer architecture__'dır. 3 adet layer olan sistemdir. buradaki layer'lar: 1-presentation layer, 2-logic layer, 3-data layer. Bunları görsel ve alternatif isimlendirmeleri ile gösterirsek:

```
--------------
-- UI        -
--------------
     |
-------------------
-- Business logic -
-------------------
     |
--------------
-- Data      -
--------------
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# onion architecture
Jeffrey Palermo tarafından 2008'de bu makalede ortaya atılmıştır: https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/

Hexagonal ile aşırı benzerdir. (Hexagonal başlığında farklılıklar anlatılıyor)

# focusing to domain-layer 
Dikkat edilirse DDD'deki gibi, domain modellerimiz ve persistence modellerimiz ve DTO'larımız farklı farklı. Bu sebeple onion, DDD ile çok büyük benzerlik göstermektedir. Yani şu şekilde mimarileri gruplayabiliriz:
- Data centeric design (or database centric design)
  - multitier architecture (or n-tier architecture) and/or multilayer architecture (or n-layer architecture)
- Domain driven design
  - DDD book of Eric Evans
  - Onion architecture
  - Hexagonal architecture
  - Clean architecture

# DDD vs onion architecture
onion sadece birkaç spesifik noktaya değinirken, DDD birçok konuyu ele almaktadır. İki mimari bir arada veya tekbaşlarına kullanılabilirler.

# n-layer vs onion
Temelde bakıldığında çok temiz yazılan bir n-layer uygulama onion'a benzer. Fakat onion'un net kuralları varken (ortaya atılan makalede net olarak anlatılmıştır), n-layer çok genel bir terimdir.

onion'da aşağıdakiler zorunlu iken, n-tier için bir standart/zorunluluk söz konusu değildir:
- sadece alt katmandaki bir metod üst katman tarafından çağrılabilir (tek yönlü).
- busines katmanında infra kodu olmamalı (@transactional annotation'u gibi kodlar olabilir).
- DI aracılığı ile interface'ler üzerinden layer'lar arası haberleşme olmalıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# hexagonal architecture (or Ports and Adapters architecture)
mimarinin ismi, görsel olarak gösterilmeye çalışıldığında çıkan şekilden esinlenerek isimlendirilmiştir.

2005 yılında Alistair Cockburn tarafından ortaya atılmıştır: http://alistair.cockburn.us/index.php/Hexagonal_architecture

Çok ufak microservislerde değil, "moduler monolith" uygulama geliştirirken tavsiye edilen bir mimaridir.

Tanımlar:
- __Adapter__: client API Controller'lar, admin API contollerlar'ın her biri birer adapter'dır.
  - __primary adapters (or driving adapters)__: Bizim sistemimize dışarıdan command atan her istek API controller'lar birer primary'dir.
  - __secondary adapters (or driven adapters)__: bizim yönettiğimiz sistemler ise secondary'dir. örnek: veritabanı, elasticsearch, email servisleri...
- __Port__: port her adapter'ın application logic (core) 'a eriştiği kapıdır. Port'lar birer interface veya facade pattern olarak düşünebiliriz.
- __application service__: port ve domain'i bu level'dan çağırıyoruz. port ve domain'ler hiçbir zaman birbirlerini direk çağramıyorlar.

# DDD vs hexagonal architecture
hexagonal sadece birkaç spesifik noktaya değinirken, DDD birçok konuyu ele almaktadır. İki mimari bir arada veya tekbaşlarına kullanılabilirler.

DDD kitabında, hexagona'daki gibi business layer'ın direk olarak port'lar aracılığı ile çağrılıp çağrılmamasına değinilmemiştir. Bu sebeple aynı şekilde, DDD kitabında secondary ve primary adapters kavramları da yoktur.

# onion vs vs hexagonal architecture
- hexagonal'de primary ve soconday olarak gruplandırılıyor. onion'da böyle bir gruplama yok.
- onion'da "application core" 3 temel katmana bölünmüş:
  - application services (piyasada use-case olarak isimlendirilen business-servisler)
  - object services (core busines servisleri) (bunlar daha çok application-service'ların common sınıfları olarak düşünülebilir)
  - object model (domain modeller)

  Bu katmanlardan "hexagonal architecture" da bahsedilmiyor.

  Onion'da bahsedilen bu 3 katmanın çok net çizgilerle neye göre ayrılacağı belirtilmemiş.
  
  application services'in object services'ten ayrı olması, en dış katmandaki servislerin (UI, test, DB...) daha bi bağımsız olması için teşvik olmuş oluyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# clean architecture
Robert C. Martin (Uncle Bob), "Clean Architecture: A Craftsman's Guide to Software Structure and Design" kitabında birçok mimarisel konudan bahsetmiştir. Bu kitabın 22'in bölümünde ("Clean Architecture" isimli bölüm) "Hexagonal Architecture" ve daha birkaç architecture'ı birleştirerek ortak noktaları "clean architecture" başlığı altında anlatmıştır. Kitaptaki 22inci bölümde bu 3 kitabı baz aldığını yazmıştır:

- __Hexagonal Architecture (also known as Ports and Adapters)__, developed by Alistair Cockburn, and adopted by Steve Freeman and Nat Pryce in their
wonderful book Growing Object Oriented Software with Tests
- __DCI__ from James Coplien and Trygve Reenskaug
- __BCE__, introduced by Ivar Jacobson from his book Object Oriented Software Engineering: A Use-Case Driven Approach

Kitaptakinden bağımsız olarak Robert buradaki (https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html) yazısında şu mimarileri de bu listeye eklemiştir:
- __Onion Architecture__ by Jeffrey Palermo
- __Screaming Architecture__ from a blog of mine last year

Not: Yukarıdaki link, Robert'ın orjinal makalesidir. Microsoft'un resmi dökümantasyonunda da "cleancoder"'a link verilmiş: https://docs.microsoft.com/en-us/dotnet/architecture/modern-web-apps-azure/common-web-application-architectures title: "Clean architecture", 1st paragraph.

Kitapta ve blogdaki yazısında Robert, yukarıdaki mimarinin bu karakteristiklerinin ortak olduğundan bahsetmiştir:

- __Independent of frameworks__ The architecture does not depend on the existence of some library of feature-laden software. This allows you to use such frameworks as tools, rather than forcing you to cram your system into their limited constraints.
- __Testable__ The business rules can be tested without the UI, database, web server, or any other external element.
- __Independent of the UI__: The UI can change easily, without changing the rest of the system. A web UI could be replaced with a console UI, for example, without changing the business rules.
- __Independent of the database__: You can swap out Oracle or SQL Server for Mongo, BigTable, CouchDB, or something else. Your business rules are not bound to the database.
- __Independent of any external agency__: business rules don’t know anything at all about the interfaces to the outside world.

# THE DEPENDENCY RULE
- İç katmanlar, dış katmanların ne yaptığından tamamen habersiz (izole) olmalıdır.
- iç katmanlar dış katmanlardan birşey çağıramazlar.
- ancak dış katman iç katmanı çağırabilir. (sadece 1 seviye içerdekini çağırabilir).

# Layers
İç katmandan dışarıya doğru sırayla:

- ENTITIES (enterprise business rules)
- USE CASES (application business rules)
- INTERFACE ADAPTERS
- FRAMEWORKS AND DRIVERS

Yukarıdaki seviyeler birçok mimaride hep mevcut. Fakat bu sayı 4 olmak zorunda değil. Duruma göre değişir. Fakat üst seviyeli bakıldığında konsept değişmemelidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Rich Domain Model vs Anemic Domain Model
Anemic türkçe kelime anlamı: anemik (kansız).

Anemic Data Model is a design pattern which separates the logic of domain model on different classes like *Service, *Util, *Manager, *Helper:

```java
class OrderUtil { // or OrderService

  public long calculateTotalPrice(Order order){
    // logic is here
  }

  // other methods...
}
```

But on rich design we write the logic inside domain model classes:

```java
order.getTotalPrice();
```

Advantages of rich domain model:
- Rich domain model applies object orianted principles. Because objects should have behaviors. Source: https://www.martinfowler.com/bliki/AnemicDomainModel.html 3th paragraph, writes:"the basic idea of object-oriented design; which is to combine data and process together. The anemic domain model is really just a procedural style design".
- If we will write the validations inside the domain object, then theese rules will be apply on all project by nature. Visa versa each service will write the same rules again and again. Source: https://www.martinfowler.com/bliki/AnemicDomainModel.html 5th paragraph.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Higher-order function
fonksiyonen dönen ve/veya fonskiyon parametresi alan fonksiyonlardır. source: https://kotlinlang.org/docs/lambdas.html#higher-order-functions title: "Higher-order functions".

Örnekler:
- React [file:"javascript_react.md" - title:"Higher-Order Components (or HOC)"](./javascript_react.md#higher-order-components-or-hoc)
- Java stream ile gelen map, reduce metodları.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# mixin (or mix-in)
Inheritance'taki amacımız interface'deki fonksiyonalitenin implementasyonlara geçmesidir. Fakat bazen birkaç metod vey ametod grubu için yeni bir interface yaratmak istemeyebiliriz. Bu gibi durumlarda sadece o fonksiyonaliteyi temsilen bir interface yaratır. İşte bu interface veya metod'lar mixin olarak adlandırılır. Örnek:

```java
class ElectronicDevice {
  // some code here...
}

class NokiaCalculator extends ElectronicDevice {
  // some code here...
}

class SamsungCalculator extends ElectronicDevice {
  // some code here...
}

// other 1000 concrete classes here (they only extends from ElectronicDevice)

// the only concrete class which extends from RandomNumberGenerator:
class LenovoCalculator extends ElectronicDevice, RandomNumberGenerator {
  // some code here...
}
```

RandomNumberGenerator bir mixin sınıftır. Burada sadece random sayı üretme fonksiyonalitesi vardır. Bunun yerine ElectronicDeviceWithRandomNumberGenerator diyebilirdik. Fakat sadece 1 model cihaz için bunu yapmak istemedik.

Mixin kullanımı diamond problemini engeller.

Farklı örnekler:
- Content-management (örnek Confluence) sistemi geliştirelim. Sistemde birçok medya dosyasını ÖRNEK: PDF, JPEG, PNG...) desteklediğimizi düşünelim. Her dosya tipinin metadata'sı var. Yani hepsi MetaData sınıfından extend ediyor. Fakat sadece 1-2 medya tipi varki özel olarak 1-2 farklı yeteneği dah amevcut. Örneğin dijital-imzalanabilme özelliği sadece Word ve Excel dosyalarında var. JPEG'de yok. Bu durumda DigitalSignature mixin'i yaratabiliriz.
- Archive-manager geliştirdiğimizi düşünelim. ZIP, RAR gibi formatlar hepsi Archivable sınıfından türeyecek. Fakat TAR formatı istisna olarak dosya sistemindeki metadata'ları da kaydedebilmemizi sağlıyor. Bu sebeple sadece onun için FileSystemMetada mixin'i yaratabiliriz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# AAA (or Arrange Act Aspect) pattern
Unit test yazarken uygulanan bir pattern'dir. Test'in oluştuğu akışları temsil eder:

- Arrange (kelime anlamı: ayarlamak/düzenlemek/planlamak)

Bu kısımda test edeceğimiz metot için test inputları, gerekli nesneler ve bu test inputları karışısında test edilen metottan beklediğimiz result hazırlanır.

- Act

Bu kısım hazırladığımız inputlarla test edilecek metodun çağırıldığı kısımdır.

- Assert

Test edilen metottan gelen resultla bizim beklediğimiz result'un assertler ile karşılaştırıldığı kısımdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Don't Use Exceptions For Flow Control (Anti-Pattern)
Exception kullanımının kodun akışını belirlememesi gerektiğini savunana bir görüştür. Bu durum dil'den dile değişmektedir. yani; bu öneri her dil için geçerli değildir. kaynak: (source-id: 145) 7'inci bölüm (her bölüm çizgi ile ayrılmış).

Örneğin; aşağıdaki gibi for-loop içinde bir döngümüzde kullanmamız önerilmez:

```java
try {
  for (int i = 0; i++){
    array[i]++;
    // Do anything here with "array[i]"
  }
} catch (ArrayIndexOutOfBoundsException e) {
  // exit from for-loop at this point or add any logic here
}
```

kaynak: (source-id: 145) 2'inci bölüm (her bölüm çizgi ile ayrılmış).

Ancak, bizim yazmadığımı herhangi bir kütüphane exception fırlatıyor ise; o zaman elden bir şey gelmez ve onu yakalamak gerekir:

```java
} catch(RedirectException e) {
  response.sendRedirect(e.getURL());
}
```

kaynak: (source-id: 145) 5'inci bölüm (her bölüm çizgi ile ayrılmış).

Aşağıdaki örneklerden birinde exception kullanılmış, diğerinde kullanılmamış:

```cs
if (conn.State != ConnectionState.Closed)
{
    conn.Close();
}
```

```cs
try
{
    conn.Close();
}
catch (InvalidOperationException ex)
{
    Console.WriteLine(ex.GetType().FullName);
    Console.WriteLine(ex.Message);
}
```

Yukarıda hangisini kullanmak önerilir? Eğer çoğunlukla exception case'ine düşülüyor ise; if bloğu ile kontrol etmek gerekli. Eğer nadir düşülecek bir case ise exception kullanmak gerekir. Çünkü eğer nadir karşılaşılıyor ise; if bloğu olmayacağı için hç if-kontrolü yapılmamış olacak. kaynak: (source-id: 148) "Handle common conditions without throwing exceptions" başlığı.

# performans etkileri
Exception kullanmak, performance konusunda bazen avantaj sağlarken bazen dezavantaj sağlayabilir. Bu dilden dile ve kod akışının durumuna göre değişiklik gösterebilir.

Artık JIT kavramı da Java dünyasına geldiği için, bu durum (performance) birçok faktörden etkileniyor. Burada (kaynak: (source-id: 149)) yazan tüm cevaplarda; Java dünyasında; try bloğuna girmenin neredeyse hiçbir maliyeti olmadığı belirtilmiş. Fakat asıl maliyet exception olduğunda ortaya çıktığı belirtilmiş.

Java'daki bu durum şundan kaynaklanıyor: Bytecode içerisinde try bloğu yoktur. Kod akışı aynen devam etmektedir. Fakat exception-table olarak isimlendirilen bir tabloda try catch gibi blokların meta bilgileri tutulmaktadır. Dolayısı ile herhangi bir exception olduğunda, JVM, bu tabloyu incelemekte ve buna göre akışı yönetmektedir. İşte maliyet burada ortaya çıkmaktadır. Eğer thread'in başlangıç noktasına kadar olan stack-trace'imiz büyükse (yani içiçe metotlar çağrılmış ise), bu durumda her metot için exception-table JVM tarafından okunup kontrol edilecektir. kaynak: (source-id: 150) "The Non-Heap Exception Table" başlığı. + bu kaynakta Bytecode örneği verilmiş. bu örnekteki Bytecode'da try-catch olmadığı da örneğin içerisinde yazmaktadır: (source-id: 149) Kingshuk Bandyopadhyay'nin cevabı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Blob (or Monster Class or god Bean or god object or god class or Winnebago) (Anti-Pattern)
Single Responsibility principle'a uymayıp birçok işi üstlenen, gereğinden fazla büyümüş olan class'lar için kullanılan bir terimdir.

# God Method (or god function)
God Class'taki gibi tüm iş yükününün sadece 1 fonksiyona düştüğü durumlardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Dependency hell (Anti-Pattern)

bu terim genelde Dependency'lerden kaynaklı sorunlar olduğunda kullanılır. bazen platform spesifik terimler kullanılır. örnek: jar hell, dll hell gibi.

Dependency hell birkaç çeşit formda karşımıza çıkabilir:

- __Long chains of dependencies__

  açıklama: dependency ağacının derinliğinin çok uzaması. Aslında bu direk olarak bir problem değil, fakat farklı bir dependency problemi ile karşılaştığımızda, sorunun çözümünü çok zorlaştıracak bir tehlike arz eden bir durumdur.

  muhtemel çözüm:
  - microservisler. bir projeyi farklı executable paketlerle kullanılabilir halde ayırmak. belki portable uygulamacıklar sunmak.
  - yada farklı dependency tercihlerinde bulunmak.

- __too many dependencies__

  muhtemel çözüm:
  - microservisler. bir projeyi farklı executable paketlerle kullanılabilir halde ayırmak. belki portable uygulamacıklar sunmak.
  - ortak super pom'lardan kurtulmak içi super pom'ları da ayırmak.

- __version collusion (or version conflict or sürüm çakışması)__

  açıklama: yürüteceğimiz bir programın bir bağımlılığı diğer yürüteceğimiz bir programın bağımlılığı ile sürüm çakışması yaşamaktadır. örnek: x programı Dependency-A'yı max 3 sürümü kabul ederken, Y programı min 4 kabul etmektedir.

  muhtemel çözüm: container (örnek Docker)

- __cyclic dependency (or circular dependency)__:

  açıklama: birbirine depend eden projeler.

  muhtemel çözüm:
  - projeleri birleştirmek
  - ortak kodları başka üçüncü bir projeye taşımak

# tree vs flat Dependency store management

Nix ve Yarn paket yöneticileri paketleri/bağımlılıkları bir dizinde tutar. buna store biçimine flat adı verilir. bu dizinde X paketi olsun. X paketinin bağımlılıkları X dizini içinde değildir. X ile aynı dizindedir. bu paket yönetim sistemi daha az yer kaplar. çünkü ortak bağımlılıklar ortak dizine referans eder.

tree'de ise her paket/Dependency kendi Dependency'lerini kendi içinde barındırır. ağaç şeklindedir. bu yöntem flat'e göre daha çok yer kaplar. fakat paketler taşınabilirdir. bir paketi başka sisteme kopyalayıp kullanabilirsiniz. çünkü herşey kendi içindedir. npm tree yöntemini kullanır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Spaghetti code (Anti-Pattern)
Özellikle kod akışının takibinin zor olduğu yazılımlarda kullanılan bir terimdir. Bu durum genelde "goto" kullanımlarında veya kod akışı tek yönlü olmadığında olur. farklı bir değişle; kodu çağıran fonksiyon, tekrar kodu çağıranı çağırdığı durumlar fazla olduğunda Spaghetti terimi kullanılır. En temelde kodun bakımının çok maliyetli olduğu durumlarda bu terim kullanılır.

Spaghetti kod olan sistemler için genelde "__Big ball of mud (or büyük çamur topu)__ ifadesi kullanılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Reinventing the square wheel (or Reinventing the wheel)
Biz yapmadan önce ilgili sorunumuza/isterimize çözüm üretilmiş olabilir. Özel bir sebep olmamasına rağmen, daha önceden üretilmiş bu çözümü kullanmıyorsak bu bir anti-pattern'dir.

# "Not invented here"
"Not invented here" terimi genelde dışarıdan ürün almama eğilimini temsil etmek için kullanılır. "Not invented here" sendromu in-house geliştirme çok daha verimsiz olmasına rağmen şirketin sürekli bu politikayı izlemesi durumunda kullanılan bir terimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# law of the instrument (or law of the hammer or Maslow's hammer or golden hammer or Old Yeller or Head-in-the sand)
hammer kelime anlamı: çekiç

Kişinin bildiği ve sevdiği favori framework/tool/çözümün en geçerli olan olduğu veya en iyisi olduğu yaklaşımıdır. Bu yaklaşım bri anti-pattern'dir. Zira kişinin tüm yeniliklere kapalı olmasına sebep olur.

çekiç terimi şuradan geliyor: bir çocuğa çekiç verdiğinizde, tüm problemleri çözmek için o çekici kullanır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Softcoding
Hardcoding'in zıttıdır. Business'in kodda değil, config dosyalarında olmasıdır. Tabiki ihtiyaçlar gereği uygulanabilir fakat debug etmeyi aşırı zorlaştırdığı gibi, sürekli çok fazla generic kod yazmaya sebep oluyor.

# Hardcoding
Kodun konfigürasyonlarının sabit olarak kodun içine gömülmesidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# magic string
yazılımcılar arasında "hardcode string" olarakta adlandırılır. enum, yada final static olmayan kod içine direk olarak yazılmış string'lere verilen isimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Lava Flow (or Dead Code)
Optimum koşullarda geliştirilmemiş kodun, canlıya alınması durumunda kullanılan terimdir. bu kodun en önemli sıkıntılarından biri bazı kısmının kullanılmıyor (Dead Code) veya amacının olmaması durumu olduğu içi anti-pattern'e aynı zamanda "Dead Code" denilebiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# boat anchor (or boatanchor)
boat anchor kelime anlamı: tekne çapası

kodun içerisinde ileride ihtiyacımız olabilecek fakat şu anda kullanılmayan özelliklerin olmasıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Error hiding
Catch edilen hatanın hiçbir yere log'lanmaması veya kaydedilmemesi yada son kullanıcıya dönülmemesi durumlarıdır. Hata catch edildiğinde, farklı bir iş logic'i uygulanıyor olabilir fakat her türlü bunların log'lanması gerekmektedir. Yoksa hata yok olur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Busy waiting
bir bilgi beklerken, CPU'yu meşgul edilmesi durumudur. Bunun yerine reactive çözümler uygulanmalıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Calling super (or call super)
Super class'ın metotunu çağırmak bir anti-pattern'dir. Biz abstract/arayüzdeki (super class'taki) metotları override etmeliyiz fakat super çağırmamalıyız. Api'mizi kullanan client zaten super class'taki metotları çağırmalı ve bu metotlarda override etmemiz beklenen metotları çağırmalıdır.

Bunun en temel sebebi, alt implementasyonların, üst sınıfları işleyişini iyi bilmeleri gereksinimidir. Ne zaman super çağrılacak ne zaman çağrılmayacak gibi konuları implementasyonlar bilmemelidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Analysis Paralysis
Paralysis kelime anlamı: felç

Proje analizinde gereğinden fazla kaynak harcanmasında ortaya çıkan sorundur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Cargo cult programming
Pattern'leri veya çözümleri anlamadan sadece uygulamak için uygulayıp program geliştirme tekniğidir.

Cargo cult terimi şuradan geliyor: 2inci dünya savaşında Pasifik adasında bir grup kitle sürekli olarak onlara kargo ile yemek getiren yüce insanları bekliyordu. Fakat arkada ne olup bittiğinden haberleri yok ve sorgulamıyorlardı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# vendor lock-in (or proprietary lock-in or customer lock-in)
Bir yazılımın bir altyapıya/ürüne çok fazla bağımlı olacak şekilde geliştirilmesi. Böyle olunca farklı bir ürüne geçiş çok maliyetli, hatta geçiş yerine projenin tümüyle yeniden yazılmasına sebebiyet verebiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Functional Decomposition (or No Object-Oriented AntiPattern or No OO)
bir sınıfın bir metotunun olması, tüm elemanlarının private olması, sadece 1 işi, 1 metot ile yapması, onu prosedürel değilmişçesine kullanılmasına sebep olan metotolojidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Don't repeat yourself (or DRY or do not repeat yourself)
tekrar yazılan kodları ortak yere alma prensibidir. Bu uygulanmadığında bir anti-pattern olur ve birçok kod sürekli farklı yerlerde tekrar yazılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Silver Bullet
kelime anlamı: gümüş kurşun

Bir teknolojinin (kütüphane, dil, pattern, çözüm...) her duruma göre çözüm sunamayacağı ortadadır. Fakat bunu böyle düşünmeyip, ilgili çözümü sürekli ve her duruma uygulandığı zaman olumsuz sonuçlar yaşanmaktadır. Yani; hiçbir teknoloji "silver bullet" değildir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Smart UI Anti-Pattern
Business logic'in önyüz kodunda olan sistemlerdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •