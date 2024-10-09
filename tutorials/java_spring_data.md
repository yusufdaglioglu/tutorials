############################

############################
# JAVA SPRING DATA
############################

############################

# Spring Data
en üst modülün ismi "Spring Data"'dır. Spring data bir arayüzdür. Bunun birçok implementasyonu mevcut. Örnek implementasyonlar:
- Spring Data JPA
- Spring Data MongoDB
- Spring Data JDBC
- Spring Data LDAP
- Spring Data Redis
- Spring Data for Apache Cassandra

'Spring Data' implementasyonları Spring tarafından geliştirilmez. Spring implementasyonları sadece JPA gibi API'lerin önüne bir API daha koyar. böylece; relational database'lere erişmek istediğimizde; JPA veya alternatifi bir framework ile erişmek istediğimizde tümüne aynı API'lerden erişebilmemizi sağlar.

Spring-data-commons; ortak annotation'ları vs barındıran jar'dır. Fakat ortak olmayan annotation'larda mevcut. örneğin; MongoDB için @Entity yerine @Document yazmak gerekli.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## Spring JTA
JavaEE'nin kullandığı Java Transaction API (or JTA)'yı kullanır. Yine, "Spring data" da olduğu gibi, Spring önüne wrapper atarak bir API sunar. Dolayısı ile bir JTA implementasyonuna ihtiyacımız var:

- Atomikos
- Narayana
- Java Open Transaction Manager (or JOTM)
- JBoss TS (JavaEE sunucusundan bağımsız çalışabilmektedir)
- Bitronix Transaction Manager (or BTM)

Yada Java EE managed bir Transaction Manager kullanabiliriz. Bunun için uygulamamızın JavaEE sunucu üzerinde kaldırmak zorundayız. Spring embedded web server ile geldiğinde de, embedded Java web server'ın sunduğu JTA'yı kullanabiliriz.

JTA birden fazla DB'nin de transactional çalışmasını sağlamaktadır. yani bir işlem diğer DB'ye kaydedilemediyse, ilk database'ye de kaydetmeyecektir.

JTA 2 amaçla kullanılabilir:
- JMS: Broker'a mesaj gitmesi, onun karşı taraftan okunduğunu ve karşı tarafta çalıştırıldığı metotun başarılı şekilde tamamlandığından emin olunması
- DB: DB bilgilerinin gerektiğinden bir den fazla DB'ye tümüyle kaydedilebilmesinden emin olunması

kaynak: (source-id: 279) 2 inci paragraf.

Spring, PlatformTransactionManager sınıfı ile transaction yapısını desteklemektedir. @Transactional annotation'unu bir metotun tepesine yazdığımızda, Spring application-context içerisinde PlatformTransactionManager implementasyonu (Bean) arayacaktır. Eğer manuel Bean tanımlatmak istiyorsak şunu yapmalıyız:

```java
@Bean
 public PlatformTransactionManager platformTransactionManager(){
    return new JtaTransactionManager();
}
```

PlatformTransactionManager class hiyerarşisi şu şekildedir:

```
- PlatformTransactionManager
-- HibernateTransactionManager
-- JTATransactionManager
-- JMSTransactionManager
-- JPATransactionManager
-- WebLogicJtaTransactionManager
```

kaynak: (source-id: 280)

## distributed transaction
'__Global transaction__' terimi XA protokolü dökümanında 'distributed transaction'ın kendisine verilen isimdir. global transaction her yerde kullanılan bir terim değildir. XA dökümanında, karışıklığı engellemek için bu şekilde kullanıldığı belirtilmiş. kaynak: (source-id: 281) "Distributed Transaction Processing:Reference Model", Version 3, title: "2.1 Transaction Definitions", subtitle: "Global Transaction" and "Distributed Transaction".

'local transaction' terimi XA protokolü dökümanında hiç kullanılmamıştır.

global transaction'lar birden fazla bağımsız kaynak üzerinde çalışırken transaction sağlayan yapılardır. global transaction'lar için üçüncü parti bir servisin kaynakları kontrol ediyor olması gerekmektedir. ancak "global transaction'lar" ile 2PC yapılabilmektedir. 2PC nin olması için üçüncü parti servisin DB gibi kaynaklarla iletişimde olması gerekiyor. Bu iletişim için kullanılan protokollerden bir tanesi XA'dır. kaynak: (source-id: 279) 3 üncü paragraf.

__X/Open (or Open Group for Unix Systems)__ birçok kuruluş tarafından desteklenen ortak bir camiadır. XA protokolü, X/Open tarafından geliştirilmiştir. bu dökümanında spesifikasyon tanımlanmıştır: (source-id: 283) Dökümanın dağıtım sayfası: (source-id: 284)

Dökümandaki bazı tanımlar:
- __Application Program__: transaction'ın adımlarını tanımlayan program.
- __Resource Manager__: DB, file-system, printer service gibi paylaşılacak kaynağı paylaşan yazılımdır.
- __Transaction Manager__: transaction'ın ne durumda olduğunu monitör eden, gerektiğinde rollback alması için ilgili RM'lere protokol üzerinden haber veren yazılımdır.

"Transaction"; aynı resource-manager'de yapılan, rollback edilebilir atomik işlemi kapsamaktadır. Oysa "Distributed transaction"; farklı resource manager'lerında olduğu data'lar üzerinde yapılan, rollback edilebilir atomik işlemleri kapsamaktadır.

Dökümanda 11.inci sayfada "Interface Overview" başlığında belirtildiği gibi XA, DB ile transaction manager arasındaki protokoldür. Bu sebeple DB tarafından desteklenmesi zorunludur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# EntityManager/EntityManagerFactory vs Session/SessionFactory
Session Hibernate spesific sınıftır. EntityManager ise JPA spesifiktir. EntityManager arkaplanda Hibernate'in Session'ını zaten kullanmaktadır.

EntityManager'dan Session instance'ımızı elde edebiliriz:

```java
Session session = entityManager.unwrap(Session.class);
```

# entity manager types
Basically JPA specification defines two types of entity managers:

- Application-Managed (__LocalEntityManagerFactoryBean__ kullanılır)

  Application Managed entity manager means "Entity Managers are created and managed by the application (example: our code)" .

- Container Managed (__LocalContainerEntityManagerFactoryBean__ kullanılır)

  Container Managed entity manager means "Entity Managers are created and managed by the J2EE container (example: our code doesn't directly manages instead entity managers are created and managed by container, and our code gets EM's through some way like using JNDI ).

# JPA Konfigürasyon

- ## Uygulama Tarafından Yönetilen EntityManager Konfigürasyonu

jpa-application-managed.xml:

```xml
<Bean id="entityManagerFactory"
      class="org.springframework.orm.jpa.LocalEntityManagerFactoryBean">

         <property name="persistenceUnitName"
                   value="rentAcar-application-managed" />
</Bean>
```

persistence.xml:

```xml
<persistence
    version="1.0"
    xmlns="http://java.sun.com/xml/ns/persistence"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/persistencehttp://java.sun.com/xml/ns/persistence/persistence_1_0.xsd">

        <persistence-unit name="rentAcar" />
        <persistence-unit name="rentAcar-application-managed">
            <class>com.myapp.Spring.Car</class>
            <class>com.myapp.Spring.Customer</class>
            <class>com.myapp.Spring.Rental</class>
            <properties>
                <property name="hibernate.connection.url" value="JDBC:hsqldb:mem:Spring-playground" />
                <property name="hibernate.connection.driver_class" value="org.hsqldb.jdbcDriver" />
                <property name="hibernate.dialect" value="org.hibernate.dialect.HSQLDialect" />
                <property name="hibernate.connection.username" value="sa" />
                <property name="hibernate.connection.password" value="" />
                <property name="hibernate.hbm2ddl.auto" value="create" />
                <property name="hibernate.show_sql" value="true" />
                <property name="hibernate.format_sql" value="true" />
                <property name="hibernate.hbm2ddl.show" value="true" />
            </properties>
        </persistence-unit>
</persistence>
```

örnek bir işlem:

```java
public static void main(String[] argv) {
    ApplicationContext context = new ClassPathXmlApplicationContext("jpa-application-managed.xml");
    EntityManagerFactory factory = (EntityManagerFactory) context.getBean("entityManagerFactory");
    EntityManager manager = factory.createEntityManager();
    EntityTransaction transaction = manager.getTransaction();
    transaction.begin();
    List result = manager.createQuery("select c.name from Customer c").getResultList();
    System.out.println(result.size());
    transaction.commit();
    manager.close();
    factory.close();
}
```

- ## Uygulama Sunucusu Tarafından Yönetilen EntityManager Konfigürasyonu

jpa-container-managed.xml:

```xml
<Bean id="entityManagerFactory"
      class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">

          <property name="jpaVendorAdapter">
                   <Bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter"/>
          </property>

          <property name="jpaProperties">
                  <props>
                    <prop key="hibernate.show_sql">true</prop>
                    <prop key="hibernate.format_sql">true</prop>
                    <prop key="hibernate.hbm2ddl.auto">create</prop>
                    <prop key="hibernate.hbm2ddl.show">true</prop>
                  </props>
          </property>

          <property name="dataSource" ref="dataSource" />
</Bean>
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Spring Data "delete" performance
delete metodu entityManager veya repository tarafından çağrılırsa, spring-data:
- tüm silmesi gereken sub entity'leri de silmesi gerekecektir. (cascade işlemler)
- cache varsa güncellemesi gerekecektir
- bir entity'ye (buna sub-entity'lerde dahil) event atanmış ise, o event'lerin (@PreRemove, @PostRemove...) tetiklenmesi gerekecektir. ("JPA Entity Lifecycle Events" konusu.)
- managed entity'lerin güncellenmesi gerekecektir.

Dolayısı ile "order" tablomuzdan 1000 adet order'ı sildiğimizde sistemin çok memory kullanmaya başladığını ve sürecin uzadığını göreceğiz.

Böyle bir durumda saf SQL execute etmemiz gerekecek.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Repository
Türkçe kelime anlamı: depo, ambar

```java
public interface MyRepository extends CrudRepository<Person, Long> {

  Person save(Person p);

  Optional<Person> findById(Long primaryKey);

  Iterable<Person> findAll();

  long count();

  void delete(Person p);

  boolean existsById(Long primaryKey);
}
```

Yukarıdaki metotlar default implemente edilmiş durumdalar. çağrıldıklarında veritabanında işlem yapacaklardır. isteğe bağlı ekstra metotlar yazılabilir. o zaman interface değil class yapılmalıdır. fakat metot body'si yazılmayacak ise (yani hep query'ler ile çalışılacak ise) sadece interface yapılıp metot imzaları yeterli olacaktır.

Super'den sub-arayüze doğru:

- Repository -> hiçbir metot içermiyor.

- CrudRepository -> count, delete, existById, findAll, save gibi metotlar içeriyor

- PagingAndSortingRepository --> pagination için metotlar içeriyor.

bunlardan türemiş birçok implementasyon mevcut.

# @NoRepositoryBean
ilgili sınıfın instance'ının repository olmadığını işaretler. örneğin baseRepository sınıflarımızda bu annotation'ı kullanabiliriz. örnek 2: PagingAndSortingRepository bu annotation'ı içeriyor.

# enabling JPA on project
```java
@EnableJpaRepositories(basePackages = "com.demo.repositories.jpa")
@EnableMongoRepositories(basePackages = "com.demo.repositories.Mongo")
interface Configuration { }
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# @Modifying
repository'lerde her eğer manuel bir query yazıyor isek ve database'yi güncelliyorsak, o zaman bu annotation'u o metotumuzun tepesine eklememiz şarttır. bunsuz da çalışır kod, fakat sağlıklı olmaz. JPA buna göre güncelleme olup olmadığını biliyor ve buna göre arkaplandaki davranışlarını belirliyor (örnek cache güncelleme gibi...)

```java
@Query("UPDATE my_table SET name = ahmet WHERE id = :id")
@Modifying
int increaseValue(@Param("id") Long id);
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# relations
(önce güçlü zayıf tablo başlığı okunmalı)

customer-order tablosunda ilişkilere bakalım. customer güçlü, order güçsüz tablodur. çünkü order'da customer-ID olmak zorundadır. yoksa o order bir anlam ifade etmez (diğer bir değiş ile sistemimiz için bir geçerliliği olmaz. diğer bir değiş ile business'a göre bir mantığı olmaz).

"CustomerEntity.java" içerisinde bu olabilir:

```java
@OneToMany(mappedBy="customer") //order class'ının private 'customer' property'sidir (Java field'ının ismi burada yer almalı, database sütun ismi değil!)
private List<Order> order;
```

"OrderEntity.java" içerisinde bu olabilir:

```java
@ManyToOne
@JoinColumn(name="customer_id") //order tablosundaki 'customer_id' sütununun adı.
// @JoinColumn(name="customer_id", referencedColumnName = "customer_national_identity_number") // Eğer böyle olursa order tablosundaki 'customer_id', Customer tablosundaki customer_national_identity_number sütununa denk geliyor anlamına gelir. Eğer referencedColumnName'i yazmazsak zaten DB ayarlarında olan foreign-key ayarı devreye girecektir. Fakat eğer database'de foreign-key ayarı yok ise, referencedColumnName'e mutlaka ihtiyaç vardır. (eğer bu ayar girilmezse JPA default olarak Customer'ın default primary key'ini referans ediyor)
private Customer customer;
```

# JoinColumn vs mappedBy
JoinColumn ile JPA'ya, ilişki sütununun (foregin key'in) olduğunu belirtiyoruz. mappedby belirtildiğinde ilgili tablomuzda foreign key olmamasına rağmen, Java objesinde sanal olarak varmış gibi davranabilmemiz sağlanabiliyor.

örneğin yukarıdaki örnekte;
- customer içinde herhangi bir foreign key olmamasına rağmen getOrder yapabileceğiz.
- oysa JoinColumn ile order tablosunun customer_id diye database'de bir sütun içerdiğini belirtiyoruz.

# crosstable (or jointable)
sadece tablo ilişkilerinin tutulduğu tabloya verilen isimdir. one-to-one yada many-to-one ilişkilerde pek tercih edilmiyor. çünkü yeni tablo yaratmak yerine, var olan tablolara bir foreign key ekleniyor. fakat many-to-many ilişkilerde kullanılması şart. ismi genelde ilişkilendirilen iki tablonun birleşimidir. örnek: customer_order.

# @jointable
kullanımı:

```java
@Entity
public class A {

    private Long id;

    @ManyToOne
    @JoinTable(
       name = "A_B",
       joinColumns = @JoinColumn(name = "B_ID"),
       inverseJoinColumns = @JoinColumn(name = "A_ID")
    )
    private B b;
}
```

# FetchType
@ManyToOne(fetch = FetchType.LAZY) şeklinde kullanılır. Lazy sadece get metotu çağrılınca database'den nesneyi çağırır, EAGER ise her zaman direk database'den okunmuş halde objeyi getirir. one-to-one ve many-to-one ilişki default olarak EAGER iken, many-to-many ilişki default'ta LAZY'dir.

# Basic
@Basic annotation'ı temelde hiçbir şey yapmaz. İlgili alana "FetchType" parametresini geçebilmek için yaratılmış boş bir annotation'dur. eğer zaten bir JPA annotation'u field üstünde varsa FetchType'ı o annotation'a parametre geçeriz. fakat hiçbir JPA annotation'u yoksa FetchType'ı @Basic ile geçeriz.

# CascadeType
@OneToMany(cascade=CascadeType.ALL) şeklinde kullanılır. örneğin order yaratıyoruz, fakat customer_id foreign key'inin null olmaması gerekiyor. dolayısı ile önce customer yaratılmış olmalı. bu sebeple iki tabloya birden kayıt atarken önce customer daha sonra order'a yazmasını istiyoruz JPA'dan. örnek:

```java
Student student = new Student();
student.setName("Ahmet");
student.setHomeAddress( new Address("Istanbul") );
entityManager.persist(student);
```

Yukarısı CascadeType belirtmediysek hata verecektir. çünkü adres objesi student içinde tanımlı değil. bunu şu şekilde çözebiliriz:

```java
@OneToOne(cascade = CascadeType.PERSIST)
private Address homeAddress;
```

CascadeType'lar (aşağıdaki her örnek Order ve Order içinde field olan OrderItem üzerinden anlatılmıştır):

- __PERSIST__: Order persist edilirse, OrderItem da persist edilir.

- __MERGE__: Order merge edilirse, OrderItem da merge edilir ("persist vs merge" farkı başka başlıkta anlatılıyor).

- __REMOVE__: Order silinirse, OrderItem da silinir.

- __REFRESH__: EntityManager.refresh(Order) metodu çağrılırsa, EntityManager.refresh(OrderItem) da çağrılır.

  EK not: EntityManager.refresh(entity) metotu DB'deki bilgilerin tümünü RAM'deki instance'a kaydediyor.

- __ALL__: Yukarıdaki tüm özellikleri birlikte aktif edilmesini sağlıyor.

Multiple kullanım da mevcuttur:

```java
@OneToOne(cascade = { CascadeType.PERSIST, CascadeType.MERGE })
```

# orphanRemoval
orphan kelime anlamı: öksüz.

Kullanımı:

```java
@OneToMany(orphanRemoval = true, cascade = CascadeType.PERSIST, mappedBy = "orderRequest")
private List<LineItem> lineItems;
```

__orphanRemoval__, __CascadeType.REMOVE__'dan farklıdır. __CascadeType.REMOVE__, "__Order__" silinirse, silinen __order__'a ait __orderItem__'ların tümünün silinmesini sağlar. Fakat __orphanRemoval__, order'ın silinme durumunda devreye girmez. orderItem-list'ten 1 eleman silindiğinde, ve repository.save(order) yapıldığında, sadece silinen order-item'ın DB'den silinmesini sağlar. farkı örneklerle gösterirsek:

```java
Order order = new Order();
order.removeOldItems();
entityManager.persist(order); // in this line, orphanRemoval will remove old items.
```

```java
Order order = new Order();
entityManager.remove(order); // in this line, CascadeType.REMOVE will work all "items" of Order.
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Entity manager methods: merge vs persist
persist edilen entity üzerinde farklı bir manipülasyon (sınıf alanı (sutun) güncellemesi) yapılırsa, transaction sonunda o manipülasyonlar da DB'ye otomatik yansıyacaktır. merge işleminde aynı durum söz konusu değildir.

Örnek:

```java
MyEntity e = new MyEntity();

// scenario 1
// transaction starts
em.persist(e);
e.setSomeField(someValue);
// transaction ends, and the row for someField is updated in the database

// scenario 2
// transaction starts
e = new MyEntity();
em.merge(e);
e.setSomeField(anotherValue);
// transaction ends but the row for someField is not updated in the database
// (you made the changes *after* merging)

// scenario 3
// transaction starts
e = new MyEntity();
MyEntity e2 = em.merge(e);
e2.setSomeField(anotherValue);
// transaction ends and the row for someField is updated
// (the changes were made to e2, not e)
```

# managed entity vs detached entity
persist metotuna yollanan entity'ler managed entity durumuna gelir. merge metotundan dönen instance, managed iken, merge metotuna parametre geçilen instance ise hala detached kalmaya devam etmektedir.

# bi-directional (or bidirectional) vs in-directional (or indirectional)
onetomany yada manytomany yada onetoone, tek yönlü yada çift yönlü olabilir. örneğin; onetoone ilişki yaptık, fakat 2 objenin sadece 1 tanesine foreign key koyduk. Java objesinden (entity içinde) de tanımlama yapmadık. dolayısı ile runtime sırasında sadece birinden diğerine gidiş olabilecek. dolaylı yoldan erişim yapılır fakat bidirectional ve indirectional terimleri direk erişim olup olmadığı ile ilgili bir durumdur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# transaction
bir metotun tepesine @Transactional annotation'u atılırsa o metotun içindeki işlemlerde yapılan database işlemleri atomic olur. eğer hata çıkarsa otomatik olarak SQL'ler geri alınır. aslında geri alınmasından ziyade yapılan database işlemleri database'de işlenmiyor (commit vs flush başlığında daha detaylı anlatılıyor).

@Transactional annotation'ı, transaction'ı rollback etmekle ve denetlemekle yükümlü bir aspect. @Transactional attığımız fonksiyondan exception fırlatılırsa, @Transactional bu exception'ı değiştirmez.

@Transaction opsiyonel olarak şu parametreleri alabilir:

```java
@Transactional(rollbackFor = MyException.class,
               propagation = Propagation.REQUIRED,
               timeout = 35)
```

- rollbackFor

  sadece belirtilen exception(lar) için rollback yapılacağıdır. default'ta herhangi bir Java exception'ında SQL'ler rollback edilir.

  eğer bu property'de belirtilenlerden farklı bir exception alınırsa, oraya kadar ki SQL'ler commit edilir.

- timeout

  eğer bu süre geçerse rollback edilir

- readOnly

  ilgili transaction bloğunun sadece read-only işlem yaptığının bilgisini transaction manager'a yollar.

  transaction manager bunu runtime'da optimizasyon için kullanabilir.

  eğer "true" verilmişse; transaction manager fail vermek zorunda değil. bu transaction manager'a kalmış bir durum. kaynak: (source-id: 285)

- propagation

  Türkçe kelime anlamı: yayılma

  @transactional bir metot içinde farklı bir @transactional Bean'in metotu çağrılabilir. o zaman bu yeni metot'daki işlemlerin bağımsız mı yoksa, bir önceki transaction'a bağlı mı olacağına propagation özelliği ile karar veriliyor. propagation'ın alabileceği bazı değerler aşağıdaki gibidir.

  not: eğer çarğrılan metot içerisinde exception alınırsa, exception muhtemelen(genelde) catch edilmeyecektir. çünkü exception'ı @Transactional annotation'u algılasın isteyeceğiz ki revert işlemini yapabilsin. Tabi bu durumda; parent metot'ta bu exception'u manuel olarak catch etmez isek, o zaman parent metot'da exception fırlatır ve artık parent metottaki @Transactional annotation'u çalışır. İŞte burada bunun yönetimi yazılımcıya kalmıştır. Yani try-catch'i manuel tutmalı ve akışı business'a göre kendi manuel yönetmelidir.

  - REQUIRED

    Spring'de default değerdir. Aktif bir Transaction yoksa yeni bir transaction açar. Aktif varsa buna katılır.

  - REQUIRES_NEW

    Aktif bir transaction işlemi varsa bunu bekletir (Suspend) Yeni bir tane açarak kendi işini hallettikten sonra bu transaction işlemini kaldığı yerden devam ettirir. 2 transaction birbirinden bağımsızdır.

  - NOT_SUPPORTED

    Transaction varsa suspend edilir servis metotu transaction'sız çalıştırılır.

  - NEVER

    Transaction varsa Exception fırlatır.

  - MANDATORY

    Transaction yoksa exception fırlatır

  - SUPPORTS

    önceden açılmış transaction varsa; onu kullanır. yoksa transaction'sız işlem yapar.

  - NESTED

    Aktif bir Transaction yoksa yeni bir transaction açar. Aktif varsa buna katılır. Buraya kadarki kısım REQUIRED ile aynıdır. fakat ek olarak metot başlangıcında bir "savepoint" yaratır. ve eğer hata alınırsa bu savepoint'e dönülür. Transaction'lar arkaplanda DB tarafında yönetildiği için, buradaki savepoint'ler DB'deki 'SAVE TRANSACTION save-point-name' e denk gelmektedir.

@Transactional dekleratif çözüm. Programatik çözüm için PlatformTransactionManager veya TransactionTemplate sınıflarından yararlanılır. örnek:

```java
@Autowired
PlatformTransactionManager txManager;

myFunction(){
  DefaultTransactionDefinition def = new DefaultTransactionDefinition();
  // explicitly setting the transaction name is something that can only be done programmatically
  def.setName("SomeTxName");
  def.setPropagationBehavior(TransactionDefinition.PROPAGATION_REQUIRED);
  def.setTimeout(10);
  TransactionStatus status = txManager.getTransaction(def);
  try {
    // execute your business logic here
  }
  catch (MyException ex) {
    txManager.rollback(status);
    throw ex;
  }
  txManager.commit(status);
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# application.properties dosyasındaki örnek datasource tanımı

bu tanım JPA sadece JDBC kullanılacakken,

```yml
mydabatabase:
  datasource:
    url: JDBC:sqlserver://IP:1433;databaseName=mydb
    username: usr
    password: 123
    driver-class-name: com.Microsoft.sqlserver.JDBC.SQLServerDriver
    testOnConnect: true # her database connection'ı oluşturulduğunda öncesinde yapılan kontroldür
    testWhileIdle: true # bir connection'dan kimse bir süredir işlem yapmadıysa, bağlantı üzerinden validationQuery çağrılır.
    logValidationErrors: true # validationQuery yapıldığında alınacak olan hataların log'lanıp log'lanlanmayacağı
    validationQuery: "SELECT 1" # yukarıdaki event'lerde (testOnConnect, testWhileIdle...) çağrılacak SQL sorgusu

```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# flush vs commit
commit transaction'ı sonlandırmak için kullanılır. transaction-begin var fakat end yoktur. end metotu commit olarak isimlendirilir.

flush ise transaction'ı yöneten framework'ün sunduğu fonksiyondur. transaction'lar arkaplanda aslında DB tarafında yönetilir. fakat framework, RAM'de tuttuğu fakat henüz DB'ye atmadığı SQL sorguları olabilir. Bu durum sadece "transaction" varolduğu durumlarda olur. Bu RAM'deki SQL'leri şu durumlarda framrwork database'e fiziksel olarak yollar:
- her query hazırlandığı zaman otomatik framework flush eder
- manuel flush edebiliriz (flush fonksiyonu ile)
- framework kendi belli aralıklarla da flush edebilir (buradaki zaman aralığı konfigüratif olabilir)
- framework commit sırasında otomatik flush eder (bu kesin tüm entity-framework'ler tarafından yapılmak zorundadır)

Örneğin Hibernate'in Flush modları burada yazmaktadır: https://docs.jboss.org/hibernate/orm/3.5/javadocs/org/hibernate/FlushMode.html 

# unit of work pattern
"Flush" konusuna bakıldığında, asında Hibernate'in "unit of work" pattern'ini zaten arkaplanda otomatik işlettiğini görebiliriz. RAM'de tuttuğu, ve henüz database'e fiziksel olarak atmadığı SQL'ler "unit of work" pattern'ine tekabül eder. "Flush" metodu çağrıldığında unit-of-work mantığında biriktirdiği SQL'leri DB'ye yollar.

unit of work pattern'i transactional olabilir veya olmayabilir.

# Unit-of-work vs SQL batch vs sping-batch
unit-of-work application'ın business katmanında kullanılan bir patterndir.

batch ise birçok sql statement'ının tutulduğu SQL topluluğudur. SQL standardında olan bir terimdir.

unit-of-work, batch kullanarak çalışır.

hibernate-batch feature'ü (spring-batch modülü ile karıştırılmamalıdır. tamamen farklı kavramlardır.) batch ve unit-of-work mantığında çalışır. fakat arkaplanda opsiyonel olarak ek optimizasyonlar yapar.

# Flush'lamanın yan etkileri
- read-uncommited isolation seviyesi DB'de açık ise, flush fonksiyonunu çağırırsak başka transaction'lar, flush yaptığımız transaction'ın flush'ladığı data'ları görebilir.
- Constraint'lerin validation'ları işleme alınabilir.

# Deferrable SQL Constraints
Deferrable kelime anlamı: ertelenebilir.

Transactional işlemlerde table constraints'lerin (kuralların NOT-NULL, FOREIGN-KEY gibi) ne zaman yapılacağı konusu biraz karmaşık. Her DB bunu farklı manage edebiliyor. Öncelikle data'nın flush edilmesi şart. Bu her DB için geçerli. Flush edilmeden, DB tarafında data'lar onaylanamaz/valide edilemez. Yani "entity framework" (örnek JPA) bu validasyonu kendi yapamaz. Fakat flush edilse bile anında kontrol sağlanmıyor olabilir. İşte burası karışık olan kısım.

Flush edilmiş data'da bazı constraint'ler o anda kontrol edilirken, bazıları sadece transaction'ın sonunda kontrol ediliyor/valide ediliyor. İşte hangilerinin ne zaman kontrol edileceğini belirlemek için PostgreSQL'de her constraint için farklı modlar sunuyor. Tablo oluştururken her sütuna için mod vermek zorundayız (aşağıda örneği var). Eğer mod verilmezse varsayılan mod kullanılır. Her modun kuralları aşağıdaki tablodaki gibidir (source-id: 459):

Mod tanımlamak için SQL syntax'ı şu formatta olmalıdır:

```
ALTER CONSTRAINT constraint_name [ DEFERRABLE | NOT DEFERRABLE ] [ INITIALLY DEFERRED | INITIALLY IMMEDIATE ]
```

Örnek mod tanımlama:

```java
ALTER TABLE foo
  ADD CONSTRAINT foo_bar_fk
  FOREIGN KEY (bar_id) REFERENCES bar (id)
  DEFERRABLE INITIALLY IMMEDIATE;
```

There are only 3 modes on PostgreSQL: source:(source-id: 473) title: "Description", 2th paragraph.

| Mode                             | Constraints are validated  | this setting can change per (by) transaction. |
|----------------------------------|----------------------------|-----------------------------------------------|
| NOT DEFERRABLE (note-1)          | immediately(note-2)        | not allowed                                   |
| DEFERRABLE + INITIALLY IMMEDIATE | immediately(note-2)        | allowed(note-3)                               |
| DEFERRABLE + INITIALLY DEFERRED  | at transaction commit time | allowed(note-3)                               |

note-1: NOT DEFERRABLE is always IMMEDIATE. source:(source-id: 473) title: "Description", 2th paragraph.

note-2: immediately after each row is updated individually.

note-3: Any transaction can override the rule with below SQL syntax:

```sql
BEGIN;
  -- bu satır ile artık erteleme modunu sadece "foo_bar_fk" için açmış oluyoruz.
  SET CONSTRAINTS foo_bar_fk DEFERRED;
  -- ...
  -- now we can do whatever we want to bar_id
  -- ...
COMMIT;
```

Bazı constraint'lere her modu set edemeyebiliyoruz. source: (source-id: 473) title: "Description", 5th paragraph.

(Not: JPA tarafında bunun yönetimini sağlayan bir yapı bulamadım)

# Use cases
Some usa cases for those modes
- (source-id: 474) title: "Use cases".
- (source-id: 459) title: "Why Defer?".

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# org.springframework.transaction.PlatformTransactionManager

arayüzdür:

```java
public interface PlatformTransactionManager {
    TransactionStatus getTransaction(TransactionDefinition definition) throws TransactionException;
    void commit(TransactionStatus status) throws TransactionException;
    void rollback(TransactionStatus status) throws TransactionException;
}
```
TransactionStatus arayüzü ise:

```java
public interface TransactionStatus extends SavepointManager {
  boolean isNewTransaction();
  boolean hasSavepoint();
  void setRollbackOnly();
  boolean isRollbackOnly();
  void flush();
  boolean isCompleted();
}
```

setRollbackOnly() metotu o anda var olan transaction'ı rollback edileceği anlamına gelir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ddl-auto
Spring'in JPA modülünde bu property: "spring.jpa.hibernate.ddl-auto", sadece Hibernate kullanıldığında buna denk gelmektedir: "hibernate.hbm2ddl.auto".

aldığı değere göre database modülümüz init olduğunda aksiyon alır. alabileceği değerler:

- create: first drops existing tables, then creates new tables.

- create-drop: Similar to create, with the addition that Hibernate will drop the database after all operations are completed. Typically used for unit testing.

- validate: Hibernate only validates whether the tables and columns exist, otherwise it throws an exception

- update: yeni sutun ekleme, index oluşturma gibi işlemler yapar fakat hiçbir zaman DB'deki bir şeyi silmez.

- none: hiçbir aksiyon almaz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# envers
Hibernate'in modülüdür ve hibernate'in çekirdek repository'sinin içinde geliştirilir.

Her tablo veya field değişikliğinde audit tablolarına otomatik olarak değişiklikleri yazar. Hangi tablo veya field'ın audit'ini istiyorsak onların üstüne enversin annotation'larını atmamız yeterlidir.

Envers CriteriaUpdate, Criteria delete bulk işlemlerini handle edemiyor. Bunu resmi dökümantasyondan okudum. Fakat muhtemelen native query'leri de destekleyemiyordur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
