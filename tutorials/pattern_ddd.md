
############################

############################
# PATTERN DOMAIN DRIVEN DESIGN
############################

############################

# Kitap
DDD ilk olarak Eric Evans tarafından "Domain-Driven Design: Tackling Complexity in the Heart of Software" kitabında, 2003 yılında ortaya atılmıştır. Bu kitaba piyasada "The Big Blue Book" da denilmektedir. Daha sonra, 2014'te Eric, tanımların referanslarını kısaca yazdığı "Domain-Driven Design Reference: Definitions and Pattern Summaries" isimli kitapta özetlemiştir ve eski kitabına çok ufak eklemeler yapmıştır. Bu yeni eklemeler özellikle kitabın title index'inde her başlıkta yıldızlı şekilde belirtilmiştir. Bu yeni eklemeler şunlardır:
- Domain Events
- Partnership (from "Context Mapping")
- Big Ball of Mud (from "Context Mapping")

DDD 2003 kitabında birçok öneri/pattern'den bahsedilmektedir. Bunun yanında;
- continuous integration
- Refactoring
- Clean&Readable Code
- Factory sınıfları
- takım çalışmasının önemi
- side effect free functions

gibi konuların önemlerine bir kez daha bu kitapta özellikle değiniliyor. zaten çoğu bilgi kitap ilk yazıldığında piyasada bilinmiyordu. Bu konular DDD çalışması içinde temel oluşturduğu için bu konularda kitapta yer almıştır.

# Bu dökümandaki bilgilerin kaynağı
Bu dökümanda her bilgi sadece DDD'nin ilk kitabına dayanmıyor. Diğer yazılmış olan farklı yazarların kaynaklarındaki bilgiler doğrultusunda bu döküman hazırlanmıştır.

# ne zaman kullanılmalı
Büyük çapta ve business logic'in kapsamlı ve kompleks olduğu uygulamalarda kullanılması tercih edilen bir yaklaşımdır.

# ubiquitous language
Domain expert'leri ve yazılımcılar ortak isimlendirme kullanmalıdır. isimlendirmeler teknik değil; domain uzmanlarının verdiği isimler olmalıdır. Bu sebeple bu isimlendirmeye "ubiquitous language" (ubiquitous kelime anlamı: yaygın) adı verilmiştir.

# Domain-driven design (or DDD) vs data-centric design (or database-centric design or data-driven design)
DDD'yi anlamak için __data-centric design__ ile karşılaştırmak verimli olacaktır. Çünkü DDD, __domain-centric design (or domain-driven design)__ mantığı ile iş geliştirmeyi yapmamızı savunur.

Program yazarken önceliği hangi katmana vereceğimiz önemlidir. DDD, merkezde domain'i barındırırken, data-centric yapıda ise merkezde data vardır. DDD, önce domain modellerini çıkarılıp, daha sonra kalıcı veri yapısı (data) kısmı tasarlanmasını tavsiye eder. Domain ve persistence DB'deki tablolarımız farklı olabilir. Domain tarafında bu DB'den hiç habersiz (__persistence ignorance__) geliştirme yapılmalıdır. Domain business'i geliştirilirken, domain modelleri üzerinden konuşulmalıdır. Oysa data-centric yapıda; data'lar business ile içiçe karışmış olmasa bile (yani; iyi bir dizayn yapılmış olsa bile), uzun zaman sonra data'ya göre business akışı uyarlanabiliyor. Örneğin;

- 1- Geliştirdiğimiz yazılımda, "Product" tablosunun 3 farklı feature sütunu olsun: F1, F2, F3. Bunun yerine "feature" tablosu yapıp, her product'ın özelliklerini burada tutabiliriz. Her product'ın özelliksini de, feature-tipi (F1, F2, F3 değeri alabilir) ve feature değeri şeklinde tutabiliriz.

- 2- data-centric bir yapıda NoSQL veritabanı kullanmış ve denormalizasyonları yaptığımızı varsayalım.

Yukarıdaki 2 madde DB tarafında verimli bir çözüm olsa da, bu kısmın domain tarafında hiç bilinmemesi gerekiyor. İşte DDD bu yönelimin olmaması için mimarisel öneriler sunuyor. Çünkü yazılımın temel ihtiyacı domain'e hizmet vermektir. DDD'de konular çok keskin bir şekilde ayrılmış değil. Yani şu, şu şekilde yapılacak, yoksa DDD olmaz diye öneriler mevcut değil. DDD sadece bir dizayn yaklaşımıdır.

Yukarıdaki maddeleri şuradan çıkarabiliriz: book: "Patterns, Principles, And Practices Of Domain-Driven Design", authors: "Scott Millett" and "Nick Tune", title: "Domain Model Implementation Patterns", subtitle: "domain model", 63 page, 1st paragraph.

Bazı domainlerde (örneğin; metric/istatistik, yapay zeka sistemlerinde) DDD yerine data-centric mimariler kullanmak daha verimli olabiliyor.

# business katmanı VS repository implementasyonu (persistence katmanı)
DDD kitabında repository'lerin sadece aggregate root'ları üzerinde işlem yapması gerektiğinden bahsedilmiş. Bazen bazı kod bloklarının repository implementasyonlarımızda mı, yoksa business logic'lerin olduğu servislerde mi olacağı karışmaktadır. Örneğin bir data'yı silmeye kalktığımızda, önce onunla bağlantılı data'ları silmemiz gerekmektedir. İşte bu kod bloğu repository tarafında olmalıdır. Burada şöyle düşünmek lazım: Eğer ki; database tarafında çok farklı bir yapı kullanıyor olsaydık, bu kod bloğu değişecekmiydi? Database değiştiğinde, domain'in bundan haberi olmaması gerektiği için, bu kod bloğunu repository tarafına çekmeliyiz. Gerçek bir örnekten gidersek; RDMBS kullandığımızı varsayalım. Fakat bunun yerine ileride 1 entity'ye ait tüm ilişkileri Cassandra DB'de tek 1 tablo içerisinde tutabiliriz. Bu durumda domain'in değişmemesi lazım. Bu sebeple, bahsedilen kod bloğu repository altında olmalıdır. Tabi burada ince bir nokta daha var: Domain servisleri, repository metodlarını çağırırken, ilişkili data'ları ayrı ayrı silineceğini bildirmez, sadece aggregate root'un son halini yollar ve bunun silineceğini belirtir:

```java
// pseudo code örneği

// compatible with DDD:
repository.save( orderInstanceDomainObject );

// "Repository" implementations may have inside anyting like JPA-Entities.
```

# 4 Layers of DDD
DDD'de bir sistem 4 temel katman bulunmalıdır (kaynak: "Eric Evans", "DDD", "Layered Architecture" başlığı.):

- __User Interface (or Presentation Layer)__: web servislerinin bulunduğu, önyüzlerin bulunduğu kodlar.

- __Application__: bu kısım 'use case'leri içerir. bu kısımda business logic yoktur. Controller'larımızın gittikleri ilk servisler burasıdır diyebiliriz.

  Görevlerine bazı örnekler;
  - farklı client'lar için yapılan transformation ve validasyon işlemleri yapılabilir.
  - UnitOfWork, transactional gibi yönetimlerin tümünü bu katman yapar.
  - Dış dünyaya data döneceği zaman, domain-objelerini dönmez. Bunları DTO'ya çevirir. Dışarıya DTO döner.

- __Domain__: bu kısım business logic'lerin olduğu kısımdır.

- __Infrastructure__: diğer hizmetlerin (mail, database...) bulunduğu servisler/kodlar. Bu kısma aynı zamanda ORM mapperlar'da dahildir.

Yukarıdaki her maddenin ayrımı için kaynak: "Eric Evans", "DDD", şekil: "Figure 4.1. Objects carry out responsibilities consistent with their layer and are more coupled to other objects in their layer.".

# Domain types
- __subdomain__

  Domain: ecommerce, sub-domain: checkout, order.

  bir domain'de birden fazla subdomain olabilir. bir sub-domain'de birden fazla bounded context olabilir.

  best-practice'lere göre 1 bounded-context, 1 sub-domain'e denk gelmelidir. Fakat bunu yapmak çok zordur.

- __core domain__

  bir doktor için yazılım düşünelim. Temelde 2 ihtiyaç var: 1-randevu sistemi, 2-hastaların bilgileri

  Burada core domain 2inci oluyor. Çünkü; hastaların bilgilerini tutma, tahlil sonuçlarını göstermek temel amaç. Bu sebeple "randevu" domain'i, __supporting domain__'dir.

- __generic subdomain__

  user-management, file-archive gibi core domain bilgisi hiç gerektirmeyen katmanlar, "generic domain" olarak adlandırılıyor. Bunlar bazne 3üncü parti servisler olabilir, dışarıdan satın alınabilir yzılımlar olabilir...

# bounded context
Bir model herkes için farklı görünebilir.

örnek-1; USB ile satılan bir Java yazılımı; aynı şirkette;
- satış departmanı için satılacak "ürün"
- lojistik departmanında "kargo"
- yazılımcı içinse bir "jar dosyası"dır

örnek-2; her kullanıcı; aynı şirkette;
- security-manager için user
- order-management için customer
- admin-panel-service için ürünlerin dağıtımını yapmak için depoda çalışan işçiler olabilir.

Bazen farklı kavramlar ise aynı ismi alabilmektedir. Örnek: "account" tanımı; farklı domainlerde;
- banka için; paranın işlem gördüğü yapıdır.
- e-ticaret için; kullanıcı register olunca oluşturulan ve kullanıcının ayarlarının kaydolduğu yapıdır.

"Bounded context" içerisindeki tüm modeller aynı field'ları içerir. Bu yazılım kodları grubuna bounded context denir.

Yukarıdaki örnek-1'de satış ve lojistik ve yazılım departmanları ayrı birer bounded context içerisindedir. Her bounded context kendi içinde aynı dilden konuşur.

Eğer bizim bounded context'teki bir modeli başka bir bounded context'teki bir model ile map'leme yapacak isek bu konu "context mapping" başlığında anlatılmaktadır. (Bu map'leme ihtiyacı, haberleşme sırasında doğar)

# example grouping of all services
Aşağıdaki her servis kendince birer bounded-context'i ifade etmektedir. Sonuçta bounded-context; birbiriyle yakından ilişkili servisleri aynı yerde moduler olarak konuslandırmak anlamına geliyor.

- checkout __(domain)__ (composition root)
  - basket __(domain)__ (aggregate root)
    - kampanyalar __(subdomain)__ (core domain)
    - gift    __(subdomain)__ (core domain)
    - kupon   __(subdomain)__ (core domain)
    - product __(aggregate)__ (core domain)
  - delivery __(domain)__ (aggregate root)
  - payment __(domain)__ (aggregate root)
  - order __(domain)__ (aggregate root)
- user __(generic domain)__
- product-search __(core domain)__

# defining the bounded contexts
bounded context'lere ayırma işinin resmi bir standardı yoktur. bu iş biraz da sezgisel (heuristic) yapılır. Bounded context'lere ayırma işlemi yapılırken bazı dikkat edilecek noktalar şunlardır:
- uygulama geliştirme süreci ilerledikçe aynı isimler farklı field veya model'lerde karşımıza çıkmaya başlar. Bu durumda acaba bunlar farklı bounded context'lerde mi olmalı diye düşünmeliyiz.
- Yapılacak işin altyapısına göre değil, iş mantığına göre belirlenmelidir.
- transaction süreci göz önünde bulundurulabilir.

# context map/mapping
bounded context'ler arası model ilişkilerinin nasıl yönetileceği için disiplinleri belirler. Bu disiplinlerin bazıları aşağıda listelenmiştir.

Context mapping farklı bounded context'ler arası modellerin ilişkilerini gösteren bir dökümandır. (source-id: 477) book:"Domain Driven Design Quickly", authors:"Abel Avram", "Floyd Marinescu", page:73, writes:"A Context Map is a document which outlines the different Bounded Contexts and the relationships between them. A Context Map can be a diagram like the one below, or it can be any written document. The level of detail may vary. What it is important is that everyone working on the project shares and understands it."

- Partnership

  iki context'i geliştiren ekip birbirinin projeleri için uygun/uyumlu şekilde ortak kararlarla modellerini günceller.

- Shared kernel

  farklı bounded context'ler ortak model direk kod olarak paylaşabilir. bu JAR/DLL paylaşarak olabilir... örneğin sadece "customer" modeli 2 bounded context için ortak ise bu yol izlenebilir.

- Customer/Supplier Development Teams

  bounded context'deki takımlardan biri customer biri supplier rolündedir. Bir takım diğer tamımdaki değişiklikleri izler ve planlayarak kendine alır. Burada direk modeli "hemen kabul etme" ilişkisinden ziyade, iki müşteri-tedarikçi rollerinde bir çalışma modeli söz konusudur.

- Conformist

  iki servisteki ekipler birbiri ile ortak çalışamıyor ise, 1 ekip diğerinin yaptığı her değişikliği almak zorunda olduğu durumdur. 1 ekip dğer ekibi hiç düşünmeden modellerini günceller.

- Anticorruption Layer

  bir context diğer context'teki ile uyuşmazlıklar yaşıyabilir. bu durumda en iyi çözüm; herkesin tamamen kendi modelini oluşturması ve olabildiğince birbirinin field'larına bağımlı kalmayacak şekilde servis istekleri hazırlanmalıdır. Bunun için adapter (integration layer) yazılması gerekmektedir.

- Open Host Service

  Daha çok public API açan hizmetlerdeki gibi bir çalışma modeli söz konusudur.

- Published Language

  calendar synx, contact sync, auth standarts (örnek: OAuth) gibi sistemler buna güzel birer örnektir. Belli dökümantasyona/standartlara uyan modellerle çalışıldığında, API'lere de zaten dolaylı yoldan uymuş olacağınız sistemlerdir.

- Seperate Ways

  Ortak bir çalışmanın mümkün olmadığı takımlarda artık tamamen farklı yollara gidilebilir. Hiçbir şekilde ortam model paylaşımı yapılmaz.

- Big ball of mud

  kelime anlamı: büyük çamur topu.

  Burada hiçbir boundary veya yuakrıdaki patternler uygulanmamaktadır. Fakat farklı takımlar birbirlerine haber vererek doğaçlam bir çalışma tekniği izlerler. Önerilen bir yöntem değildir.

# microservice vs bounded context
DDD mimarisinde microservis ile ilgili bir bilgi yoktur. DDD, "modüler monolith" bir yazılımda da kullanılabilir. Dolayısı ile şu durumlar olabilir:
- Bazı durumlarda 1 mikroservis tek başına bir bounded context'i temsil edebilirken, birden fazla mikroservis birlikte 1 bounded context'i temsil edebilir. kaynak: (source-id: 231) "Bir Bounded Context == Bir Mikroservis ?" başlığı. + (source-id: 232) 1inci paragrafın sonu.
- 1 domain birden fazla microservis'ten meydana gelebilir. Veya tam tersi de olabilir.

# servisler
objelerin doğasında olmayan yazılımsal süreçler servis olarak tasarlanmalıdır. servis isimleri yine ortak (Ubiquitous) olmalıdır.

# Aggregate vs Aggregate Root
1 aggregate'te birden fazla aggregate root içerebilir gibi cümleler tamamen yanlıştır. Aggregate birşeylerin bütün olarak kullanılmasını temsil eden bir terim. Biz DDD'de bunu pratiğe döktüğümüzde karşımıza aggregate root kavramı çıkar.

Farklı bir deyiş ile; birden fazla entity'nin transaction'larda birlikte uyumlu şekilde iş akışını tamamlamaları gerekir. birden fazla entity'nin birlikte kullanılması durumu "__aggregate__" olarak ifade edilmektedir. yani aggregate, entity'ler grubudur. aggregate; koda baktığımızda sadece bir sınıf ile temsil edilmez. aggregate; ucu açık ve soyut/logical bir kavramdır. Sadece DDD'ye ait bir kavram gibi düşünülmemelidir.

"__aggregate root__" ise; direk örnek üzerinden gidersek: "sipariş" bir 'aggregate root'tur. ancak siparişe bağlı, 'ödeme bilgisi', 'kargo bilgisi' entity'ler aggregate root'a bağlı (normal) entity olarak nitelenirler.

Siparişe bağlı 'ödeme bilgileri', 'kargo bilgisi' bazı durumlarda (bazı domain'lerde) aynı aggregate root'a bağlı olmayabilir. Fakat "sipariş kalemi (yani bir siparişteki her item)", "sipariş" aggregate root'unun altında olması bu konuya daha net bir örnek olarak verilebilir.

DDD makalelerinde ve kitabında Aggregate ve Aggregate root aynı anlamda kullanılmaktadır. Çünkü biri soyut diğeri DDD'ye özel bir implementasyondur.

aggregate root; repository tarafından yönetilir ve client bir data load ettiğinde ancak aggregate root'a erişebilir. kaynak: (source-id: 233) answer of Jeff Sternal at Dec 24 '09 at 15:33. Fakat pratik'te böyle kod yazılmaz. genelde client'a sadece ihtiyacı olan data kısmı yollanır. bu durum anti-pattern değildir. client için gerektikçe optimizasyon yapılabilir.

```java
public class Order { // this is the "aggregate root" which has all information about "aggregate".

  UUID orderId;

  long orderNumberForEndUser;

  boolean disabled;

  List<OrderItem> orderItemList; // this is "domain object/domain entity".
  // all other domain objects (like orderItem) should be added here as an object, not ID!

  // Customer is another aggregate root. Therefore customer should not be added as object. It should be ID.
  long customerId;

  // Customer is another "root aggregate". Therefore we should not use the below fields on this class:
  // String customerName; // Which is inside Customer.
  // Customer customer; // this is another aggregate root.
  // Address cutomerAddress; // this is another domain object which is inside Customer.

  // getter and setters should be here...
  // "setters" ve "getters"'lar dışarıya zorunlu olmadıkça açılmamalı. Daha doğrusu açılabilir, fakat getter ile private field'ı
  // çekip başkası o private field içerisinde işlem yapamamalı. Aggregate root'u, Object orianted programlamada olduğu gibi
  // sadece public metodları ile işlem yapmalıyız. Yani Order içerisindeki domain objeleri üzerinde manipülasyon sadece Aggregate Root'un
  // metodları aracılığı ile olmalıdır. Örneğin tem eklemek isteyen client aşağıdaki metodu tercih etmeli.

  boolean addItem(OrderItem orderItem){

      if(disabled == true){
        return false;
      }

      if(orderItemList == null){
        orderItemList = new ArrayList<>();
      }

      if(orderItemList.size() > 999){
         throw new OrderException("You can not add more then 999 items to basket!");
      }

      orderItemList.add(orderItem);

      // This is "domain event".
      // Kafka can be used or Spring-event (application itself). By this way, other aggregates may effected by this operation.
      // It is optional to publish an event for each business method.
      // "eventToPublish" method does not publish the event directly. It adds the event to list. This event list should be publish directy after we save the Order Aggregate to DB. (This process should be transactional. "Outbox pattern" can be used.) (for the source check below note-1)
      kafka.eventToPublish( new OrderItemAddedEvent(orderItem) );

      return true;
  }

  // All other business logic specific to Order should be here...

  // All the data and method of this class should be "persistence agnostic".

  // Only "Order" can be sent to "repository" to save/update...
  // OrderItem should not sent to "repository". There should not be "OrderItemRepository" on the whole application code.

  // repository.save(order) method should be called from services or another layers.

  // Order should be saved transactional (via repository). Therefore the events (like OrderItemAddedEvent) should never publish before the transaction will success. (To achive this gap, if we use microservices we can prefer outbox pattern.)
}
```

source for above (note-1):

https://github.com/Sairyss/domain-driven-hexagon title:"Domain Events", sentence: "typeorm.repository.base.ts - repository publishes all domain events for execution when it persists changes to an aggregate.". The link "typeorm.repository.base.ts" redirects here: https://github.com/Sairyss/domain-driven-hexagon/blob/master/src/libs/ddd/infrastructure/database/base-classes/typeorm.repository.base.ts On this class we can see below code:

```typescript
async saveMultiple(entities: Entity[]): Promise<Entity[]> {
    const ormEntities = entities.map(entity => {
      entity.validate();
      return this.mapper.toOrmEntity(entity);
    });
    const result = await this.repository.save(ormEntities);
    await Promise.all(
      entities.map(entity =>
        DomainEvents.publishEvents(entity.id, this.logger, this.correlationId),
      ),
    );
    this.logger.debug(
      `[${entities}]: persisted ${entities.map(entity => entity.id)}`,
    );
    return result.map(entity => this.mapper.toDomainEntity(entity));
  }
```

# seperating the business logics of Aggregate methods
bir örnek üzerinden gidelim:

```java
public class Order { // this is the "aggregate root" which has all information about "aggregate".

  public void disableOrder(){

    order.setDisabled(true);
    order.revertBonus();

    // optionally other business logic here...
    // optionally preparing domain events here...
  }

  private void setDisabled(boolean newStatus){
    this.disabled = newStatus;
  }

  public void revertBonus(){
    // revert the bonus (points) from user.
  }

  // other methods here...
```

Yukarıdaki Aggregate'i kullanan kod bloğu şu şekilde işlem yapmaması gerekiyor:

```java
order.setDisabled(true);
order.revertBonus();
```

Bu sebeple setDisabled metodu private yapıldı. Yani disableOrder metodu bir facade görevi görüyor. Fakat bazı durumlarda sadece revertBonus yapılabileceği için revertBonus public olarak bırakıldı.

Tabi bu durum tamamen business'a bağlı. Yani; bu kuralın kesin ve her zaman bu şekilde doğru olduğunu varsayarak yukarıdaki kodu yazdık: "Eğer bir sipariş iptal ediliyor ise mutlaka o siparişten kazanılan bonus'lar geri alınmalıdır.".

# Domain Services vs Application Service
Service kavramı çok geneldir. 2'ye bölünür:

- Domain Servisler, aggregate'lerin doğasında olmayan metodların toplantığı süreçleri kapsayan sınıflar olmalıdır.

  Servis sınıfıların çok olması ve/veya Aggregate'lerin içindeki metodların servislerde bulunması iyi bir durumun göstergesi değildir.

  Domain servis'lerin state'i olmamalıdır.

- application serive, domain service ile farkını "4 Layers of DDD" başlığından çıkarabiliriz.

# Domain Events
domain expert'lerini de ilgilendiren olaylar kolay takip edilebilir olmalıdır. aynı zamanda her event'in ismi/tanımı ve ne yaptığı açıkça belirtilmeli ve herkes tarafından event sonrası ve öncesi nelerin gerçekleştiği herkes tarafından bilinmeli. isimlendirmeler yine ortak (Ubiquitous) olmalıdır.

Teknik anlamda konuşursak; domain event'leri, (farklı veya aynı "bounded context" içindeki) aggregate'lerin birbirleri ile haberleşmesi (bir aggregate'teki değişiklik, farklı bir aggregate'te de değişiklik gerektirdiğinde) durumunda fırlatılan event'lerdir. (Bu konuya event-sourcing başlığında da değinildi).

Bir domain event, bir aggregate tarafından yollanan event, aynı domaindeki farklı aggregate tarafından handle edilebilir. Farklı domain olmak zorunda değil.

Domain event'leri aynı domain içerisinden yakalanabilir. Farklı domain'den yakalanmamalıdır. source: https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/domain-events-design-implementation title:"What is a domain event?", paragraph: 1,8,9.

Domain event'ler senkron veya asenkron olabilirler. Fakat "Integration event"leri kesinlikle asenkron olmak zorundadır. source: https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/domain-events-design-implementation title:"What is a domain event?", paragraph: 9.

integration events domain dışındaki yerlere yollanan event'lerdir. source: https://docs.microsoft.com/en-us/dotnet/architecture/microservices/microservice-ddd-cqrs-patterns/domain-events-design-implementation title:"Domain events versus integration events", paragraph: 2.

Domain event'lerin tam olarak ne zaman fırlatılacağı Aggregate kod örneğinde belirtildi.

# DDD kitabında geçen bazı terimler
Aşağıdaki bazı terimleri direk olarak kitap içerisinde açıklanmış.

- # domain
  domain is the field for which a system is built. Airport management, insurance sales, coffee shops...

- # model
  representation of a real world object. example: student class is not a real student object. student class includent only name, surname, age and some others representation on digital world.

  Model objemiz her zaman fiziksel bir nesneye tekabül etmek zorunda değil. Bazen "para transferi" gibi bir modelmizde olabilir.

- # domain model vs database object
  domain objeleri, database objelerine benzer fakat database objelerinden tamamiyle ayrı olabilir. ilk uygulama tasarlandığında, "modeller" tasarlanmalıdır. daha sonra database-admin bunları nasıl isterse o şekilde database'ye kaydeder. database'de hızlı arama olsun diye bazı şeyler denormalize edilebilir, sorgu bazlı tablolar yapılması için data'lar mükerrer edilebilir vs... fakat bu optimizasyonlar database objelerinde yapılmalıdır.

  dolayısı ile; bu ikisi arasında gerçek hayattaki nesneye/kavrama en benzer olan; domain objeleridir. çünkü database yöneticileri gerçek hayata uzak değişiklikler yapabilir.

  işte DDD kitabı bu 'ünün uygulamamızda birbirinden bağımsız kalmasını, bu şekilde veritabanı yapısının, domain modellerini veya iş akışını etkilememesi konusunda yönlendirmektedir.

- # domain model
  domain model (class) is the model which is using by a domain.

- # DTO (or Data Transfer Object)
  DTO is the class/structure which is using to store data. It can be using to send data from network protocols or passing arguments to functions. DTO can store model or models or any data...

  gerçek bir uygulamadan örnek verirsek; repository'den dönen entity class'ı, önce model'e çevrilir, sonrada DTO'ya çevrilir ve client'a dönülür.

  DTO'lar, modellerimiz'in içerikleri aynı olacakları anlamına gelmez. Farklı da olabilirler. Yada aynı olabilirler ama modelleri direk döndürmek yerine, mapping yaparak aynı field'lara sahip farklı objeler de kullanabiliriz. Çünkü DTO'lar sadece Data transfer objeleridir ve servisler arası haberleşme'de data transferi için kullanırlar. Remote servis çağrıları maliyetli olduğu için, tek bir istekte birçok data'yı birden taşımak isteriz. Bu sebeple DTO' pattern'i ortaya atılmıştır. kaynak: (source-id: 234) 1st paragraph.

  Sun/Java community DTO'lara sadece "transfer obje" demektedir. Bu pattern'in yazılım dünyasında genel adı "Data Transfer Object (or DTO)"tir. kaynak: (source-id: 234) son paragraf.

- # entity
  yazılım framework'lerinde "entity" terimi, veritabanındaki bir tablonun yazılım tarafındaki temsilidir (örnek JPA "entity bean"). fakat Eric'in DDD kitabında "entity" terimi farklı amaçla kullanılmaktadır. kaynak: "Eric Evans", "DDD", "Entities (a.k.a. Reference Objects)" başlığı, 13'üncü paragraf.

  Eric'in kitapta bahsettiği entity kavramı; eşleştirilmeye kalktığımızda (bir eşitlik sorgusunda) field'ları ile değil, sadece ID'si ile kontrol etmemiz gereken domain modellerimizdir. kaynak: "Eric Evans", "DDD", "Entities (a.k.a. Reference Objects)" başlığı, 12'üncü paragrafın ilk cümlesi.

  Bazı kaynaklarda DDD'den bahsedilsin veya bahsedilmesin, "entity" terimi direk olarak hem domain model, hemde database nesnesinin yerine  kullanılmaktadır. Bunun 3 temel sebebi vardır:

  1- domain model, BD'ye farklı kaydedilse de, aslında ikisi birbirinin yansımasıdır. Farklı bir değiş ile; bir modelin veritabanına farklı bir biçimde kaydedilmiş olması (yansıtılmış olması) onun gerçek değerinin kaybolmasına sebep olmaz. Örneğin; aynı modelin, "materialized view" ile NoSQL ve RDBMS'e ayrı ayrı kaydediliğini düşünelim. İlgili model, veritabanı-1, veritabanı-2 arası mapper fonksiyonlarımız olsun. Aslında 3ü arasında git gel yapabiliyoruz. Bu durumda aslında 3 taraftaki data aslında aynı varlığa tekabül ediyor.

  2- JPA direk olarak kendi içinde bu terimi DDD'den bağımsız şekilde kullanmaktadır. Sadece DB tarafından bakıldığında her tablo birer 'varlık'tır.

  3- DDD uygulamadığımız sistemde, NoSQL tarzı sistemler kullanmıyorsak, genelde direk "entity bean"'lerini model olarak düşünüp birebir şekilde kullanırız.

- # value object
  - property'leri eşit olduğunda, birbirleri yerine kullanılabilen objeler'dir.
  - immutable olmalıdırlar.

  - Aşağıda bazı örnekler listelendi. Fakat bu objeler domainden domain'e (business logic'e göre) entity veya value obje olabilirlerler. Yani aşağıdakiler bu şekilde olacak diye bir kaide yoktur. Aşağıdakiler Sadece örnek amaçlı yazılmıştır.

  örnek-1:

  X ve Y veritabanı objelerimiz olsun. ikisinin tüm property'lerinin (isim, soyisim, yaş...) value'ları aynı olsun. buna rağmen bu objeler birbirlerine eşit değildir. çünkü ID'leri farklıdır. bu sebeple bu objeler "value object" değildir.

  örnek-2:

  Koordinat düzlemindeki noktayı tutan bir point1 ve point2'miz olsun. bu objelerin property'leri (x, y, z) eğer aynı ise bu 2 obje birbirlerine eşit demektedir. bu sebeple point objesi bir "value object"'tir.

  örnek-3:

  İki farklı kişi aynı adres'te oturuyor olabilir. Böyle bir durumda her iki Person objesinin hangi adrese işaret ettiğinini önemi yoktur. Bu sebeple "adres"'in value object olmasını bekleriz. Fakat bazı business'larda adres objelerinin birbirinin aynısı olup olmayacağını tanımlayan bir garanti olmayabiliyor. Çünkü adresler genelde manuel giriş yapılıyor. Son kullanıcı tarafından otomatik tamamlamadan seçilerek tanımlanmıyor. Uzunca bir string oluyorlar. Bu durum genelde e-ticaret sitelerinde oluyor. Bu sebeple e-ticaret sitelerinde adres bir entity'dir.

  Oysa bir harita sisteminde (örnek: google maps) bir adresin neresi olduğu tüm field'ları ile ayrılşmış şekilde sistematik olarak tutulur. Bu sebeple google-maps için adres value objedir.

  Not: E-ticaret'lerdeki adres objesinin entity olmasının sebebi; son kullanıcı tarafından update edilebilmesi (yani immutable kuralı bozması) değildir. Çünkü e-ticaret sitelerinde update edilen adres olduğunda, yeni bir adres objesi yaratılır ve eskisi disable edilir/edilebilir.

  örnek-4:

  renkler birer value object'tir. kaynak: kaynak: "Eric Evans", "DDD", "Is "Address" a VALUE OBJECT? Who's Asking?" başlığının hemen sonrasındaki ilk cümle.

  diğer örnekler:

  money, range, date birer value objedir.

# strategic design vs tactical design
DDD kitabı 2 temel kısımdan oluşuyor. İçerdiği bazı konular aşağıdaki gibidir:

- tactical design (bu terim kitapta geçmiyor. Piyada kitabın bu kısmına hitap edenler bu terimi kullanıyor.)

  Kitabın "Part IV" bölümünden önceki tüm bölümlerini kapsıyor.

  - aggregate
  - value object
  - layered architecture
  - factories
  - repositories

- strategic design (name of the "Part IV" of the book)

  Biraz daha büyük resimden bakmamızı sağlayan, çok aşırıd etaylar belirtilmediği bir bölüm burası. Daha çok domain'in nasıl ayrılacağını anlatmaya çalışılıyor.

  - Bounded Context
  - Context Map
  - domain types (core-domain, sub-domain...)

Aslında kitabı bu şekilde net olarak 2'ye bölmek mümkün değil. Birbirine bağlı çok konu var. Örneğin "Ubiquitous Language" konusu tactical'da anlatılıyor. Fakat strategic'in altında olması daha doğru gibi. Zaten birindeki ibareler, diğerlerinin altyapısını hazırlıyor...

Strateji ve taktik kelimelerinin kelime anlamları için buradaki cevaplar okunabilir: https://english.stackexchange.com/questions/29415/whats-the-difference-between-the-adjectives-strategic-and-tactical
