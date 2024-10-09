############################

############################
# JAVA TEST
############################

############################

# RestTemplate
Aşağıdaki gibi kullanımı olan REST client'tır. Spring-web modülü içinde gelir.

```java
RestTemplate restTemplate = new RestTemplate();

People people = restTemplate.getForObject("http://Google.com/people", People.class);

log.info(people.toString());
```

# TestRestTemplate
Bu sınıf RestTemplate'ten extend etmez fakat RestTemplate'i private olarak kendi içinde bulundurur. neredeyse tüm metotları RestTemplate'teki metotları direk çağırır. yani RestTemplate'i wrap eder. fakat ekstradan testleri kolaylaştırmak amaçlı bazı metotlar içerir. örnek:

- basic auth için password ve username'i veririz, artık her istekte bunları header'da yollar.

- Constructor with HttpClientOption sunuyor. böylece options'ları constructor'da geçebiliyoruz.

Aynı zamanda TestRestTemplate exception fırlatmaz. kodumuz çalışmaya devam eder. böylece testlerimizde try-catch bloklarımızın sayısı azalır. bunun yerine dönen response'u elle kontrolünü bize bırakır. bu durum testlerde try-catch'den dah açok terchi ediliyor çünkü.

TestRestTemplate kesinlikle sadece testler için kullanılmalıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Java'da assert kavramı
Türkçe kelime anlamı: öne sürmek, iddia etmek.

assert programlama dünyasında kullanılan genel bir mantıktır. assert, Java'da anahtar bir kelimedir. bu yüzden bir metot gibi kullanışı (syntax'ı) yoktur. Java 1.4 ile gelmiştir. örnek kullanımı şu şekildedir:

```java
assert( 3 == myNumber );
```

eğer kod runtime sırasında bu satıra geldiyse ve condition false ise; satır AssertionError fırlatacaktır. Eğer condition true ise; kod hiçbir sey yokmuş gibi devam edecektir.

assert syntax'ı şu şekilde de kullanılabilir:

```java
assert ( 3 == myNumber) : "error: number is not 3";
```

condition sağlanmaz ise sağ taraftaki blok toString'e çevrilir ve AssertionError içerisine yazılır.

Java da bir uygulama başlatırken assert keyword'lu satırlar devre dışıdır. yani hiç çalıştırılmaz. bunu enable etmek için Java uygulamasına en başta parametre geçmek gerekli. İsteğe bağlı olarak assert'leri sadece belirli paketlerde enable edebiliriz.

# assert kütüphaneleri
bazı kütüphaneler (JUnit gibi) assert metotları sunmaktadır. bu metotlar bildiğimiz Java sınıflarıdır. özel bir syntax olma durumu söz konusu değildir.

Bazı assert kütüphaneleri, kendi içinde Java'daki 'assert'ü kullanıyor veya AssertionError fırlatıyor olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# JBehave
Java için BDD test sürecini işletmek için gerekli kütüphane. Temel olarak öncelikle; test için yapılacak işlemler story dosyalarına belirli formatlarda yazılıyor. Daha sonra Java içerisinde bu dosyalardaki her satıra (işleme) tekabül eden Java metotları yazılıyor. Örneğin;

story dosyasının bir satırında:

"When the page is www.google.com it will print type to search on the screen"

Java sınıfında:

```java
@When("When the page is " $URL ' it will print ' $PRINTED_TEXT " on the screen")
public void checkWhatSiteHasPrinted(String URL, String PRINTED_TEXT){

     //do the test here
}
```

Daha sonra JBehave konfigürasyonları (çıktı rapor türü formatı gibi) yapılır. JBehave daha sonra story'leri run eder ve rapor çıktılarını istenilen dizine oluşturur.

JBehave kendi içinde JUnit'e depend eder. JBehave kendi içinde JUnit testlerini çağırır. Bu çağrılan JUnit-test story dosyalarını okur ve bunları işletir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Testcontainers
birçok programlama dili için açık kaynaklı test kütüphanesidir. container'ları programatik ayağa kaldırmamızı sağlar. bu şekilde temel amaç testlerden önce ilgili testin, container içinde koşmasını sağlamaktır.

isteğe bağlı; JUnit ile entegreli çalışabilir. bunun için ekstra lib eklemek gerekli: group-id:org.testcontainers package-id:JUnit-jupiter.

```java
@Testcontainers
class MyTestcontainersTests {

     // will be shared between test methods
    @Container
    private static final MySQLContainer MY_SQL_CONTAINER = new MySQLContainer();

     // will be started before and stopped after each test method
    @Container
    private PostgreSQLContainer postgresqlContainer = new PostgreSQLContainer()
                                                              .withDatabaseName("foo")
                                                              .withUsername("foo")
                                                              .withPassword("secret");

    @Test
    void test() {

        String mySqlUrl = MySQLContainer.getJdbcUrl();

        assertTrue(MY_SQL_CONTAINER.isRunning());
        assertTrue(postgresqlContainer.isRunning());
    }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
