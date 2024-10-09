############################

############################
# OTHERS
############################

############################

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Uzun Hashcode'a ihtiyaç duyulmadığı durumlar için genel çözüm
Hashcode ile kayıt edip arama yaptığımız durumlarda hashcode'un uzunluğu çok önemlidir. çünkü büyük tablolarda yavaşlığa sebep olur. Bu gibi durumlarda hash fonksiyonunun çıktı boyutu çok önemlidir.

Eğer hash fonksiyonumuz ihtiyacımızdan daha fazla uzunluktaysa, rastgele belirlediğimiz önceden tanımlı index'lerdeki karakterleri silebiliriz. örnek: hash çıktısının başındaki iki karakter ve/veya sondaki 1 karakteri silebilir.. Böylece hash code DB'de daha az yer kaplayacaktır. Fakat eğer bunu yaparsak çakışma oranımız yükselecektir. Çünkü hash-code zaten çok uzun bir şey diil (zaten collision olabilen bir value), birde onu kısaltırsak, collision oranımız yükselecektir. Bunun yerine kesin çözüm olarak şu yapılabilir:

Direk örnek üzerinden gidelim: yeni bir user-name kaydedecek olalım. önce sorgu yapmamız gerekli. sırası ile şu adımları uygulamamız gerekir:
- sorgulanacak user-name'in hash'i alınır.
- hash'in son 2 karakteri silinir. yeni değerimiz __X__ olsun.
- DB'de, __X__'in varolup olmadığı kontrol edilir.
  - A- Eğer DB'de __X__ yok ise; bu user-name kayıtlı değildir demektir. __X__'i kaydedebiliriz.
  - B- Eğer DB'de __X__ var ise; bu başka bir user-name'in __X__ değeri de olabililir anlamına gelmektedir. Bu sebeple; yeni user-name'in __X__'ini direk olduğu şekilde kaydedemeyiz. Önce username'in başına veya sonuna, önceden belirlediğimiz rastgele bir string ekleriz (bu string tüm uygulamanın yaşamı boyunca sabit olmalıdır). Daha sonra bu yeni user-name ile tüm adımları baştan sonra tekrar ederiz. Ne zamanki çakışma yaşamayacağız (A maddesine gireriz), o zaman kayıt işlemini yapabiliriz demektir.

  Burada ilgili user kaydında __predifined_string_added_count__ tarzı bir sutunda bilgi tutmamız şarttır.

Sonuç olarak sisteme sırası ile jack, steve ve alice eklendiğinde DB'miz şu şekilde olur:

```
username  temp-username  real-hash  trimmed-hash    predifined_string_added_count
jack      jack           123459     12345           0
steve     steveA         123458     12345           1
alice     aliceAA        123457     12345           2
```

Yukarıdaki tüm sutunları DB'de tutmak zorunda değiliz. Göstermek(konu anlatımı yapabilmek) amaçlı bu tabloya herşey eklendi.

Artık query attığımızda şu olacaktır:

isUsernameExistOnDB("steve") --> Bize 3 tane sonuç dönecek. Çünkü biz, hızlı olması için, sorgumuzu index'lenmiş olan ve düşük sayıda karaktere sahip "trimmed-hash" sutunu üzerinden yapıyoruz. Daha sonra RAM'e taşınan 3 satırdaki username'ler for-loop ile manuel karşılaştırılarak doğru sonucu bulabiliriz.

Ek not: DB'ye her defasında gitmek yerine "bloom filter" tekniğini kullanarak performansı arttırabiliriz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# base 62
bir hash algoritması değildir. matematikteki basit "taban" konusudur.

Bir sayıyı 62 tabanına çevirdiğimizde, 1-9 ve a-Z kümesi ile temsil edebileceğimiz bir sayı çıkar karşımıza.

Bu son kullanıcı için daha okunaklı ve kısa olduğundan, genelde URL Shortener'larda kullanılmaktadır. 

örneğin: 10'luk tabanındaki 11157 sayısı, 62 lik tabanda [2, 55, 59] -> [2, T, X] olmaktadır.

Her sayının karşılığı aşağıda gösterilmiştir:

- 0-0
- ...
- 9-9
- 10-a
- 11-b
- ...
- 35-z
- 36-A
- ...
- 61-Z

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Run-length encoding (RLE)
data'nın formatından bağımsız çalışabilen, kayıpsız sıkıştırma tekniğidir.

Her bir bilginin kaç kere olduğu sayı ile belirtilir. Örnek:

wwwwaaadexxxxxx --> w4a3d1e1x6

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# backporting
Backporting is the action of taking parts from a newer version of a software system or software component and porting them to an older version of the same software. It is commonly used for fixing security issues in older versions of the software.

diğer başlıklar altında, "rolling release" konularında buna direk örnek verildi.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# core dump (or memory dump or crash dump or system dump)
dump kelime anlamı: çöplerin toplandığı yer.

Yazılım işletim sistemi tarafından zorla kapatılırsa memory'deki bilgisi ve diğer ek bilgiler bir dosyaya saklanır. bu şekilde daha sonra bu dosya analiz edilebilir. bu dosya core dump denir.

Eğer yazılımın memory'deki alanı dosyaya uygulama sağlıklı çalışırken alınırsa buna __snapshot dump (or Snap dump)__ denir.

executable dosya çalıştırıldığında RAM'e yüklenir ("memory structure" başlığında anlatılıyor). Bu sebeple core dump içerisinde executable'ı bulabiliriz. Bu sebeple bazı kaynaklarda, core-dump'ın formatının, executable formatı olduğu yazar fakat bu tam olarak doğru değil. Core dump'ta, executable dosyaya ek olarak extra bilgiler (RAM'deki diğer bölgeler) de vardır.

OS'lar default olarak core dumb'ı bir yerde saklamaz. çünkü:
- boyutu büyük
- kötü niyetliler tarafından okunabilir olduğu (güvenlik açığı yaratabileceği)

Bunun için OS'a ayar çekmek gerekebilir. Bu ayarın nerede olduğu aynı OS çekirdeği olsa bile, OS'tan OS'a (distrubution'dan distribution'a) göre fark edebilmektedir.

# GNU Project Debugger (or gdb)
Assembly, C, C++ , Objective-C, go, Fortran, Pascal, Rust  gibi dilleri komut satırından debug edebilmemizi sağlar. şu şekilde başlatılır:

```sh
gdb "/path/to/executable"
```

isteğe bağlı olarak core dump dosyasını da parametre alırsa, ayağa kaldırdığı uygulamayı direk core dump dosyasındaki state ile başlatır:

```sh
gdb "/path/to/executable" "/path/to/core_dump_file"
```

Uygulama tekrar eski bir state'ten başlatıldığında, mantıklı şekilde çalışmayabilir. Çünkü önceden erişmekte olduu kaynaklar artık açık değildir. Bu sebeple muhtemelen yine hata fırlatacaktır.

# Java dump
- JVM'e "-Xdump:" ve ek parametreler geçebiliriz. Bu şekile JVM eğer anormal şekilde kapanır ise dump dosyası hazırlar. Fakat OS, JVM'i direk kill edebilir. Yani bazı durumlar vardırki process kapanacağını bilemez. Bu sebeple "-Xdump:" her daim çıktı üretmeyebilir.

- JVM'in sorunlu kapanması ile oluşan dosyaya "__Java dump (or Java core)__" denir.

- JVM, dump'ı dosyaya yazarken "__hprof__" formatını kullanır.

- Java sadece out-of-memory hatası verdiğinde core dump istiyorsak, Java uygulamamızı bu parametrelerle başlatmalıyız:

```sh
java -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=<dump-file-path>
```

- Çalışmakta olan Java uygulamasının dump'ını (process'i hiç kapatmadan) istersek şu şekilde alabiliriz:

```sh
jmap -dump:live,format=b,file=<dump-file-path> <Java-process-id>
```

- JDK içerisinde gelen JvisualVM ile GUI üzerinden herhangi bir Java uygulamasının aşağıdaki bilgilerini görebiliriz:
  - hangi parametrelerle başlatılmış
  - System.properties'te nelerin olduğu
  - historical CPU and memory usage
  - historical threads with states (waiting, running...)
  - and others...

# Java thread-dump
JvisualVM ile bir uygulamanın __thread-dump__'ını alırsak, her thread'in hangi statüde olduğunu ve her thread'deki stack-trace'i görebiliriz. Şu programımızı örnek alalım:

```java
public class MainAppX {

  public static void main(String[] args) throws InterruptedException {
      custom1();
  }

  private static void custom1() throws InterruptedException {

      while (true){
          System.out.println("for loop resume");
      }
  }
}
```

thread dump'ın bir kısmı bu şekilde olur:

```
"main" #1 prio=5 os_prio=0 tid=0x000002662e4b0000 nid=0xc1c4 waiting on condition [0x0000006bce5ff000]
   java.lang.Thread.State: TIMED_WAITING (sleeping)
        at java.lang.Thread.sleep(Native Method)
        at MainAppX.custom1(MainAppX.java:13)
        at MainAppX.main(MainAppX.java:7)

   Locked ownable synchronizers:
        - None

"VM Thread" os_prio=2 tid=0x0000026649a73800 nid=0xbc94 runnable

"GC task thread#0 (ParallelGC)" os_prio=0 tid=0x000002662e4c7800 nid=0xb41c runnable

"GC task thread#1 (ParallelGC)" os_prio=0 tid=0x000002662e4ca000 nid=0x4900 runnable
```

Görüldüğü üzere JVM'inde uygulamamıza bağlı bazı thread'leri var.

Eğer uygulamamızın __Heap-dump__'ını alırsak; her class'ın ayrı ayrı kaç adet instance'ı var görebiliyoruz ve her instance içindeki field'larn değerlerini görebiliyoruz.

# (for VisualVM) profiler vs sampler
profiling temelde instrumentation yöntemini kullanarak çalışır.

profiler runtime'da çalışan uygulamamıza Bytecode ekleyerek müdahale eder. oysa sampler böyle bir yöntem uygulamaz. bu yüzden sampler daha hızlıdır. fakat profiler daha çok farklı bilgiler sunar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# HTTP exception practices

- Facebook

```sh
curl -X GET "https://graph.Facebook.com/oauth/access_token?client_id=foo&client_secret=bar&grant_type=baz"
```

```json
{
    "error": {
        "message": "Missing redirect_uri parameter.",
        "type": "OAuthException",
        "code": 191,
        "fbtrace_id": "AWswcVwbcqf6546546546"
    }
}
```

- Twitter

```sh
curl -X GET "https://API.twitter.com/1.1/statuses/update.json?include_entities=true"
```

```json
{
    "errors": [
        {
            "code":215,
            "message":"Bad Authentication data."
        }
    ]
}
```

- default Spring error

```sh
curl -X GET -H "Accept: application/json" "http://localhost:8082/spring-rest/API/book/1"
```

```json
{
    "timestamp":"2019-09-16T22:14:45.624+0000",
    "status":500,
    "error":"Internal Server Error",
    "message":"No message available",
    "path":"/API/book/1"
}
```

- Google

```json
{
 "error": {
      "errors": [
              {
                "domain": "global",
                "reason": "invalidParameter",
                "message": "Invalid string value: 'asdf'. Allowed values: [mostpopular]",
                "locationType": "parameter",
                "location": "chart"
              }
      ],
  "code": 400,
  "message": "Invalid string value: 'asdf'. Allowed values: [mostpopular]"
 }
}
```

# RFC7807
bu RFC'de exception için standart yaratılmak istenmiş. 5 bilgi döndürüyor:

örnek:

```json
{
    // URL (or path) of help document.
    "type": "http://www.mywebsite.com/help-doc/errors/incorrect-user-pass",

    // human readable title (shorter then "detail").
    "title": "Incorrect username or password.",

    // HTTP status code.
    // Bu zaten header'da geliyor. Fakat bazı durumlarda proxy server'lar HTTP status code'u header'da değiştirebiliyor. Buna karşı alınan bir önlem gibi düşünülebilir.
    "status": 401,

    // human readable message.
    "detail": "Authentication failed due to incorrect username or password.",

    // URI of error.
    // Burası URL de olabilir. Sadece identifier (URI) olması yeterli.
    "instance": "/login/jack"
}
```

Standartta isteğe bağlı olarak ek parametreler eklenebileceği belirtilmiş. kaynak: (source-id: 30) title: "3.2. Extension Members".

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# fail-fast vs fail-safe
fail-fast bir sistem internal bir error durumunda, o anda, dışarıya sunduğu interface üzerinden hata döner. Oysa fail-safe bir sistem hata olması durumunda olabilecek diğer senaryoları tamamlar ve sonrasında dönüş yapar. Fail-safe sistem dönüş yaptığında içeride hata aldığını belirtip belirtmemesi bu konu dışındadır.

örneğin Java'da bazı collection'larda for-loop içerisinde modifikasyon yapıldığında, diğer döngüye girildiğinde ConcurrentModificationException hatası alınır. İşte bu hata döngü tamamlanmadan verildiği için fail-fast'tir.

nor: Java doc'ları detaylı okunduğunda şu görülür ki; ConcurrentModificationException her zaman fırlatılmayabilir. yani bu hatanın fırlatılacağı garanti edilmez.

ConcurrentModificationException iterator kullanıp, paralelden collection elemanı silinirse veya eklenirse bu hata fırlatılır (daha doğrusu; fırlatılabilir). Eğer farklı thread'ler ilgili collection'ı yönetiyorsa, eleman silme işi "collection.remove()" ile değil, "iterator.remove()" ile yapılmalıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# data warehouse (or DW or DWH or veri ambarı or bilgi ambarı or enterprise data warehouse or EDW)
ilişkili verilerin sorgulandığı ve analizlerinin yapılabildiği bir yazılımdır. Sadece veritabanı değil, verileri sabit diskten okuyan (load eden), verinin transformasyonu yapan yazılımlarla birlikte bu kümenin içindedir.

# data mart
mart kelime anlamı: çarşı, pazar, trade center

data mart is a simple form of a data warehouse that is focused on a single subject (or functional area). Data mart'lardan veri çekmek daha kolaydır. Çünkü daha client-centric (client isteklerine uygun) data tutulur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Continuous integration vs continuous delivery vs continuous deployment
integration'dan kasıt: 'main' branch'ine açılan PR'ın merge edilmesidir; yani entegre edilmesidir. "Continuous integration" CI tool'larında otomatik build ve test koşulduğu durumlardır.

"continuous delivery", "Continuous integration"'a ek olarak acceptence-test ve "stage" ortamına taşınma işleminin otomatize edildiği durumlardır. "stage" ortamı, prod ortamından önceki ortamdır.

"continuous deployment", "Continuous integration"'a ek olarak production ortamına da çıkışın otomatize edildiği durumlardır.

yukarıdakilerin tümü için kaynak: (source-id: 32)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Reproducible builds
açık kaynaklı da olsa uygulamaların executable'larının hangi kaynak koddan üretildiğini bulabilmek için kullanılan bir yöntemdir.

kaynak kodların derlenmesi sonucu deterministik bir executable ortaya çıkacak şekilde build aşamaları hazırlanır.

Aslında bu işlemde %100 güven sağlamaz. çünkü build sırasında kullanılan tool'larında bizim koda birşey atmadığına veya onlarda da hile olmadığını bilemeyiz. Bunun için GUIX projesi, bir yazılımı derlerken, kullanılan her kütüphaneyi dahi sıfırdan build ediyor. kaynak: https://guix.gnu.org/en/blog/2024/identifying-software/ "Guix goes further by implementing so-called full-source bootstrap: for the first time, literally every package in the distribution is built from source code, starting from a very small binary seed. This gives an unprecedented level of transparency, allowing code to be audited at all levels, and improving robustness against the “trusting-trust attack” described by Ken Thompson.".

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# thunk
kelime anlamı: gümletmek

as seen below; thunk is a function that returns a function and can be execute anytime. this type of calling is using in Redux-thunk library/middleware.

```js
const topla = (x, y) => x+y;

// 'thunk' is a function that takes no arguments
const thunk = () => topla(3, 4);

// the thunk can then be passed around without being evaluated...
doSomethingWithThunk(thunk);

// evaluated it anytime
thunk(); // === 7
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# cucumber

- SmartBear firması tarafından geliştirilmektedir.
- JBehave ile apaynı mantıkta işlemektedir.
- JBehave daha core bir framework'tür, oysa cucumber biraz daha kolaydır.
- cucumber framework'ü bir çok dil için ayrı ayrı yazılmış ve ayrı ayrı repo'larda geliştirilmektedir. Java, Ruby, Javascript için mevcuttur.
- testler (features dosyalarındaki her satıra karşılık gelecek fonksiyon/kod) Java'da yazılacak ise;
  - cucumber-java8 dependency'sini eklemek gerekli (lambda desteği olduğunda java8 isimlendirilmiş)
  - cucumber-java ile sadece annotation'lar kullanılarak test yazılması sağlanıyor
  - eğer JUnit ile entegre çalışacak ise cucumber-junit dependency'si eklemek gerekli. her senaryonun bir JUnit @Test'i olarak algılanmasını sağlıyor.
  - cucumber-JVM paketi deprecated'tır.
- testleri koştuktan sonra target dizini altında raporları özel formatta çıkarır.
- cucumber, .feature uzantılı dosyaları okur ve içerisindeki her satırı JBehave gibi işletir.
- .features dosyaları Gherkin dili ile yazılmıştır. örnek syntax aşağıdadır. her 1 feature dosyası altında bir den fazla senaryo var. her senaryonun içindeki her satır "step" olarak adlandırılır.

```gherkin
Feature: Guess the word

  # The first example has two steps
  Scenario: Maker starts a game
    When the Maker starts a game
    Then the Maker waits for a Breaker to join

  # The second example has three steps
  Scenario: Breaker joins a game
    Given the Maker has started a game with the word "silky"
    When the Breaker joins the Maker's game
    Then the Breaker must guess a word with 5 characters
```

# Karate
- cucumber üzerine kurulu bir framework'tür.
- Karate'nin en temel artısı tüm testlerin sadece feature dosyalarında olmasıdır.
- Karate gömülü DSL'ler barındırır:
  - direk HTTP calling sunan DSL'ler vardır.
  - direk web sayfasında selenium driver gibi gezinebilmemizi sağlayan DSL'ler vardır.
- Karate BDD framework'ü sayılmaz. Çünkü feature dosyalarında BDD'ye göre daha fazla teknik detay gerekir. Oysa BDD'deki cümleler ve yazılımcı olmayanın bile anlayacağı cinsten olması beklenir.

# Robot
- TDD framework'üdür.
- Karate mantığında çalışır. Gömülü BDD'lerin (bunlar ek modüllerle genişletilebilir) çağrılabilmelerini sağlar.
- robot testlerini yürütmek için onun executable'ına ihtiyaç vardır. robot Python ile yazılmıştır. dolayısı ile test için oluşturacağımız project structure'sini tamamen kendilerini belirlemiştir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# What You See Is What You Get (or WYSIWYG)
Microsoft Word, Openoffice Writer, Libreoffice Writer, birer WYSIWYG örneğidir. WYSIWYG'e uyan yazılımlar, direk olarak çıktıyı düzenlememizi sağlarlar.

Oysa markdown, Latex gibi diller WYSIWYG değildir. onlar __WYSIWYM (or what you see is what you mean)__'e uyarlar. Aslında teknik anlamda bakıldığında Microsoft Word de Latex'ten farksızdır. çünkü arkaplanda kendisi yine binary/XML olarak bilgileri tutmaktadır. fakat temel fark şu ki; Microsoft Word de direk olarak çıktıyı düzenleyebildiğiniz bir arayüz sunuyor. Latex or markdown'da böyle bir yazılım olmaz. zaten olması Latex ve markdown'un mantığına ters olur.

Piyasada bu terime benzer birçok farklı terim kullanılmaktadır. örnekler: 
- WYCIWYG (or what you cache is what you get)
- WYGIWYS (or what you get is what you see)
- WYSIWYW (or what you see is what you want)

# İşaretleme dili (or etiketleme dili or markup language)
Latex, HMTL, XML, XHTML gibi dillerdir. Bir dilin markup language olabilmesi için; parse edilip işlenmesi sonrası ortaya elektronik bir döküman çıkması gerekli. Bu dillerle bu dökümanın stili ve yapısı belirlenmektedir.

Bu sebeple Json bir markup language değildir. XML markup'tır, çünkü SVG gibi yapılarda bir döküman output'u verdiği görülür. (Teoride bir Json dosyasını da parse ederek döküman yaratılabilir, ama bu iş için Json verimli olmaz.)

- Markdown
  - Çok basit bir markup dilidir. türevleri:
    - Github Flavored Markdown (or GFM). spesifikasyonu: (source-id: 77)
    - CommonMark. farklı sürümleri mevcut: (source-id: 78)
    - farklı birçok türev mevcut: (source-id: 79) "4.  Examples for Common Markdown Syntaxes" başlığında türevler listelenmiştir.
  - dosya uzantısı '.md'dir

GFM, CommonMark'ın türevidir. Sadece ekstra özellikler eklenmiştir. Bu eklenen özellikler yeni spec dökümanında (extension) olarak etiketlenmiştir. Göze çarpan farklılık: tablo extension'ıdır. kaynak: (source-id: 80)

- Asciidoc
  - Markdown'a göre daha gelişmiş ve fakat daha kompleks bir markup dilidir.
  - dosya uzantısı 'adoc'tur.
  - "Asciidoc", "A2x", "Asciidoctor" isimli uygulamalar Asciidoc formatındaki bir dosyayı HTML, PDF gibi formatlara çevirmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Chrome
Cr simgeli atomun özel ismi.

Chrome terimi bilişim dünyasında User arayüzün dış tarafındaki kısımları temsil etmek için kullanılır. Eski Amerikan araçlarında dış kaplaması krom olan arabalar vardı. Muhtemelen buradan esinlenerek bilişim dünyasına yansımıştır.

Chrome terimi hangi context'ten bakıldığına göre değişmekte olan bir terimdir. işletim sistemi seviyesinde bakılırsa Chrome; panel'dir (Microsoft Windows'taki taskbar, başlat menüsü gibi). web tarayıcıları açısından bakılırsa Chrome, web content'in (HTML'in) dışında kalan kısımlardır: örnek: URL bar, menu bar, back and forward buttons...

Firefox, API ve dökümanlarında "chrome" terimini sıkça kullanır. "Google Chrome" ismi ise belli ki esinlenmiştir.

Chrome ismini bambaşka amaçlarla özel isim olarak kullanan farklı API'ler ve yazılımlar vardır. Bu yazılımlar "chrome" terimini özel isim olarak kullanmaktadırlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Bean (dependency injection) vs static vs new
Bu karşılaştırma doğru değil. Fakat yine de ne gibi artılar eksiler olabileceğini not almak amacı ile bu konu başlığını bırakıyorum.

Bir sınıftaki metotları 3 şekilde çağrabiliriz:

```java
CustomUtil.parseEmptyCharacters();
```

- CustomUtils bir Java-Bean olur
- CustomUtils Java-Bean olmaz fakat parseEmptyCharacters static metot olur
- CustomUtils Java-Bean olmaz ve parseEmptyCharacters static değildir

3'üncü durumda objemizi bir yerden çağırdığımızda "new CustomUtils()" ile çağırmak zorunda kalabiliriz. Bu her çağırışımıza 1 satır fazladan kod ekliyor ve her defasında yeni instance oluşturmak zorundayız. Eğer birçok kere kullanıyorsak belleği doldurmaya başlar. Aynı instance'ı kullanabiliyorsak Java-Bean yapmakta yarar var.

2'inci durumda static metotların genelde birçok programlama dili teknik yapısı sebebi ile test etmek zordur. bunun için compiler'a ek fazlar, yada framework'lerden yararlanmak gerekmektedir. örnek: Java'da Mockito static metotları override edemiyor. PowerMock framework'ünü kulanmak gerekiyor.

1'inci durumda scope'ları kullanma (her request için ayrı instance oluşturma) gibi, DI özelliklerinden yararlanabiliyoruz (lazy loading gibi). Bunu yanında içiçe dependency injection'ları constructor'da argüman olarak geçmek zorunda kalmıyoruz (çünkü new operatörünü çağırmıyoruz).

Java-Bean'ler test etmek için verimsiz bir altyapısı var ise, constructor based injection tercih edilebilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# multi-tenancy
tenancy kelime anlamı: kiracılık

bir web servisinin birçok client için çalışmasına göre ayarlanmış ise, o web servisi multi-tenancy destekliyordur. multi-tenancy'nin alternatifi şudur: her client için ayrı ayrı sunuculara ayrı ayrı kurulum yapılır.

multi-tenancy sistemlerde, genelde her veritabanı sorgusunda client-id parametresi where koşuluna eklenmelidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# dry run
A dry run testing is a process which is run is a controlled conditions to minimize the negative effects of any product failure.

örnek: iki dizini sync yapan bir uygulama olsun. sync işlemi riskli. bu sebeple öncesinde dry run koşulur. dry run ile sync uygulaması önce bir dry işlem yürütür. bu işlem gerçek kopyalama işlemi yaparmış gibi ilerler, tek farkı gerçekten kopyalamamasıdır.

dry run resmi bir tanım değildir. bu sebeple, dry run işleminin tam olarak ne yapacağı uygulamanın tasarımcılarına kalmıştır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# API first
"database first" gibi bir yaklaşımdır. önce API hazırlanır, daha sonra bunun arka tarafı yazılmaya başlanır. Bu şekilde test edenler, önyüz geliştiricileri de işlerine daha sorunsuzca başlar. örneğin Swagger/OpenAPI ile hazırlayacağımız YML dosyasını herkesle paylaşırsak, herkes geliştirmeye paralelden başlar ve birbirini beklemek zorunda kalmaz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# kernel space vs user space (or userland)
tüm user (root user'ı dahil) process'lerinin harcadığı memory bölgesi userspace'tir. kernel space ise, system call'lar yapıldığı sırada, system call'ların kendi içinde kullandığı ve OS çekirdeğinin kullandığı memory'dir.

# DMA (or direct memory access)
Her işlem için gerekli data CPU ya taşınmalıdır. kopyalama işlemleri de buna dahildir. fakat donanımlar özel olarak buna bir çözüm getirmişlerdir. donanımlar artık bir I/O'nun diğer I/O'ya direk data taşımasına izin vermektedir. bu duruma DMA denir.

DMA yapabilmek için hem donanımın desteklemesi, hemde OS ve DMA işlemi yapmaya çalışan yazılımın kodunun buna göre yazılmış olması gerekmektedir.

işlem başlatılması ve sonlanması gibi durumlarda CPU'ya gidecek fakat her data byte'ı tek tek CPU'ya taşınmaya gerek kalmayacak.

DMA işlemi aynı I/O cihazından aynı I/O cihazına da yapılabilir.

UNIX-like'lerdeki read() system call'u arkaplanda bilgiyi direk belleğe CPU'ya gitmeden taşımaktadır.

# zero-copy
user space'e taşımaya gerek kalmadan yapılan dosya kopyalama işlemidir.

Bunun implementasyonu OS'a göre değişebilmektedir. zero-copy için OS'a system call yapılır. system-call'un nasıl çalıştığı yazılımcı tarafından bilinmez. sadece dosya kopyalama işlemini temsil eder.

- normal for döngüsü ile dosya kopyalama

  normal bir for-loop ile okusaydık önce system call ile birkaç byte okuyacaktık (UNIX-like sistemlerde read() system call'u). kernel space'e taşınan byte, user space'e yollanacaktı. oradan da write ile user space'den önce kernel space, oradan da I/O'ya yollanacaktı.

  UNIX-like'teki "read()" system call'unu işlemi çağrıldığında, öncelikli olarak DMA üzerinden işlem yapılıp bu data direk olarak kernel'a (read fonksiyonunun içine) taşınmaktadır. read fonksiyonu da user space'e taşımaktadır.

- zero-copy ile kopyalama:

  data, kernel space'e taşınır (fakat user space'e taşımaz). direk kernel space'den I/O ya yollanır.

  Burada unutmamak lazım ki; o işlemde kullanılan kernel space (belki) CPU'nun içindeki (RAM'den daha hızlı olan) belleklerde olabilir. bu durumda RAM'e hiç gidilmemiş olacaktır. bu da daha hızlı kopyalama anlamına gelecektir.

  Eğer OS implementasyonu daha iyi ayarlanmış ise; dosya kopyalama için belki de hiç RAM kullanılmayacak, direk DMA kullanarak harddisk'ten harddisk'e kopyalama yapacaktır.

  Yukarıdakilerden hangisinin olacağı OS'un implementasyonuna ve donanımın desteğine bağlıdır.

Java'da NIO ile bu kopyalama işlemini yapabiliriz:

```java
RandomAccessFile fromFile = new RandomAccessFile("fromFile.txt", "rw");
FileChannel      fromChannel = fromFile.getChannel();

RandomAccessFile toFile = new RandomAccessFile("toFile.txt", "rw");
FileChannel      toChannel = toFile.getChannel();

long position = 0;
long count    = fromChannel.size();

toChannel.transferFrom(fromChannel, position, count); // returns the number of bytes that actually transferred
```

UNIX-like sistemlerde C'deki sys/socket.h içindeki sendfile() system call'u bunu yapmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Lua
Portekizce'de 'ay' anlamına gelmektedir.

- Lua dili, shell syntax'ına çok benziyor.

- betik dili olduğundan, sanal makine gerektiriyor.

- fonksiyonların return değerlerinde birden fazla değer dönebilmeyi destekliyor.

```lua
function triple(x)
    return x, x, x
end

local a, b, c = triple(5)
```

- Lua içerisinde standart olan, "table" tarzı primitive gibi kullanılabilen yapılar mevcut.

- Lua comment syntax

```lua
--[==[
  comment line 1
  We can include "]=]" inside this comment
--]==]

-- this is single line comment
```

# C API

- özellikle C ile interoperability'si çok başarılı olduğundan C ile (or herhangi bir dille) yazılmış uygulamaların bir kısmı or eklentileri Lua ile yazılmaktadır.

  bahsedilen interoperability Lua VM'inin yönetimine izin veriyor. Lua ekibi, C için bir kütüphane yazmış. Bu c kütüphanesini import edip, bu kütüphane aracılığı ile Lua VM'i runtime'da açıp, üzerinde Lua kodu çalıştırabilir, VM üzerindeki variable'larda değişiklik yapabilir.

  ```c
  #include <stdlib.h>

  // Lua C kütüphaneleri
  #include <lauxlib.h>
  #include <lua.h>
  #include <lualib.h>

  int main(void)
  {
      // open Lua VM
      lua_State *lvm_hnd = lua_open();

      // load libraries inside VM
      luaL_openlibs(lvm_hnd);

      // Load a standard Lua function from global table:
      lua_getglobal(lvm_hnd, "print");

      // Push an argument onto Lua C API stack:
      lua_pushstring(lvm_hnd, "Hello");

      // Call Lua function with 1 argument and 0 results:
      lua_call(lvm_hnd, 1, 0);

      // close VM
      lua_close(lvm_hnd);

      return 0;
  }
  ```

- Sanal makinesi çok ufak boyutta ve az bellek tüketimi yapıyor. Sanal makine C ile yazılmış. Bu sebeple C ile programlanan gömülü sistemlerde Lua da kolayca kullanılabilmektedir. Zira Lua sanal makinesi de C olduğundan, direk execute edilebilir ortam yakalanmış olur. Lua'nın gömülü sistemlerde tercih edilmesinin sebebi de budur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Httpbin
Httpbin test amaçlı kullanılan, hazır HTTP metotları sunan sunucu yazılımlara verilen genel bir prefix/suffix'tir. birçok Httpbin servisi vardır:

- www.github.com/postmanlabs/httpbin
- www.github.com/kevin1024/pytest-httpbin

Httpbin'lerin bazı hazır metotları:
- request'in herhangi bir header'ını dönen metotlar (örnek user agent)
- echo servisleri
- resim dönen servisler
- tüm bu servislerin get, post, put... kombinasyonları

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# gRPC
- Google tarafından geliştirilen açık kaynaklı bir protokoldür.
- arkaplanda HTTP/2 ve __Protocol Buffers (or Protobuf)__ kullanılır.
- HTTP bazlı olduğu için web tarayıcılarından da direk istek atılabilir. Sadece ek bir gRPC kütüphanesine ihtiyacımız var. Fakat web tarafından optisonel HTTP özellikleri (HTTP2'nin "trailers" özelliği) tam desteklenmediği için, gRPC'nin bazı özellikleri de desteklenemiyor.
- supports: authentication, bidirectional streaming and flow control, blocking or non-blocking bindings, call cancellation, timeouts.
- Binary iletişim sağladığı için human-readable değildir fakat daha performanslıdır.
- gRPC sadece Protobuf ile binary desteklese de, external olarak kullanacağımız kütüphanelerle JSON, XML gibi diğer formatları da destekleyebiliyor. kaynak: (source-id: 33)
- 4 farklı tipte haberleşme sunuyor:
  - unary: bir istek atıp bir cevap aldığımız durumlar.
  - server streaming: client bir istek atıyor, server sürekli istek dönüyor (stream yapıyor).
  - client streaming: server bir istek atıyor, client sürekli istek dönüyor (stream yapıyor).
  - Bi-directional Streaming: her 2 tarafta websocket'lerdeki gibi birbirlerine stream yapıyor.

# __Protocol Buffers (or Protobuf)__
binary'ye serileştirme yöntemidir. Google tarafından geliştiriliyor. XML, JSON gibi human-readable çıktı yapmaması dezavantaj, fakat performans ve memory konusunda çok daha verimli.

gRPC ve Protobuf'ın ayrı ayrı sürümleri mevcuttur.

# REST vs gRPC
REST bir mimari iken, gRPC bşr teknoloji. Ne kadar da karşılaştırma doğru olmasa da, birçok makalede karşılaştırmasını görebiliriz. Çünkü gRPC'nin sundupu bazı yetenekler (örnek: Bi-directional Streaming) REST mimarisine uygun değil.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Kernel panic
UNIX-like sistemlerde kullanılan bir terimdir. işletim sisteminin, çözümü olmayan durumu yakaladığında bilinçli olarak fırlattığı bir hatadır. Microsoft Windows'ta bu hata tipine __stop error (or blue screen of death or blue screen or BSoD)__ denir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# location transparency
bir kaynağın erişim adresini (örneğin network ise IP'si, dosya istemi ise dosya adresi, mikroservisler ise servis'e erişmek için IP'si) bilmek zorunda kalmadığımız, onun yerine o kaynağa gidebileceğimiz sanal bir adres verilmesi durumudur.

örnekler:

- bir makinen gerçek IP'sini vermek istemiyorsa, o makineye bağlanan kişilere farklı bir IP veririz ve yönlendirme yaparız. kişiler gerçek IP'yi bilmez.

- Docker servislerimiz ayakta olsun. birbirinin IP'lerini bilmiyorlar. fakat birbirlerine hostname'ler aracılığı ile erişiyorlar. kimse kimin nerede olduğunu bulmak için ekstra efor harcamıyor. böyle bir durumda Docker, sağladığı ortamda location transparency vardır diyebiliriz.

- Kafka, yada Axon'da event başlattığımızda, event'i başlatan uygulama event'i kimin çekeceğini bilmez. routing işlemini Kafka veya event'i yöneten mekanizma yapar. bu durumda; Axon veya Kafka location transparency sağlar anlamına gelir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# single point of failure (or SPOF)
çökmesi/bozulması durumunda, diğer tüm alt sistemlerin durmasına sebep olacak alt sisteme denir.

# Single Source of Truth (SSOT or Doğru Bilginin Tek Kaynağı or single point of truth or SPOT)
Data'nın merkezi ve tek noktada tutulduğu mimarilerdir.

Örneğin;
- Redux kütüphanesi storagesi 1 tanedir, yani SSOT mimarisine uyar.
- Bir şirkette bazı bilgiler Word, bazı bilgiler ise kağıda basılmış kopyaları farklı yerlerde korunuyor olabilir. Bu gibi durumlar ise bilginin SSOT mimarisinde olmadığını anlamına gelir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# NATS
ikinci adı: STAN (Nats'ın tersi) kaynak: (source-id: 34) (54 üncü satırda yorumda da bu isim kullanılmış)

publish/subscribe mesaj alışverişini sağlayan açık kaynaklı kütüphanedir. NATS ile haberleşilen ortamda "go" dili ile yazılmış bir sunucu bulunmaktadır. Sunucu kendi için kuyruk mekanizması barındırır. Birçok programlama dili için client yazılmıştır.

NATS; Neural Autonomic Transport System'ın kısaltmasından türetilmiştir. Geliştirici ekip, NATS'ı, merkezi bir sinir sistemi gibi çalıştığını düşündüğü için böyle bir isim üzerine karar vermiştir.

NATS'ın 2 çeşit sunucusu vardır: "NATS Streaming Server" ve "NATS Server". streaming server, normal NATS Server'ı wrap ediyor ve ek özellikler sunuyor. streaming server isminden anlaşılacağı gibi data stream'in yapmıyor. stream'in server server'daki bilgiyi persist ediyor (veritabanına kayıt ediyor) ve bunun sayesinde eski mesajları herkesin alabilmesini de sağlıyor. diğer tüm özellikler burada: https://nats-io.github.io/docs/nats_streaming/intro.html#features

nats-sub ve nats-pub, "golang" ile yazılmış, NATS client'larıdır. bu client'lar sadece komut satırından parametre alarak client işlem yaparlar. sadece DEMO/test amaçlı geliştirilmişlerdir.

örnek komut:

```sh
nats-pub "hello world"
```

Java client için basit kod örneği;

```java
Connection nc = Nats.connect("nats://localhost:4222");
nc.publish("topic1", "hello world");
nc.close();
```

streaming publish tarafının kodu:
```java
StreamingConnectionFactory cf = new StreamingConnectionFactory("cluster-id", "client-id");
StreamingConnection sc = cf.createConnection();
sc.publish("topic-id", "my-message".getBytes()); // synchronous
```

streaming subscribe tarafı kodu:
```java
// kendi içimizde tuttuğumuz bir obje. bu sadece bir integer. her defasında, value'sunu 1 düşürürüz.
// en sonda thread'imizi bekletmek için bu objemizden yararlanacağız
final CountDownLatch doneSignal = new CountDownLatch(1);// 1'den aşağıya geri sayım yapabileceğiz

Subscription sub = sc.subscribe("topic1", new MessageHandler() {

    public void onMessage(Message m) {
        System.out.printf("Received a message: " + m.getData() );
        doneSignal.countDown();
    }

}, new SubscriptionOptions.Builder().deliverAllAvailable().build() ); // 3rd parameter is Options

// burada countdown 0 olmadan thread beklemeye alınıyor
doneSignal.await();

sub.unsubscribe();
sc.close();
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Alternatives to Axon
__Eventuate Local__ Axon framework'üne alternatiftir. fakat sadece "event sourcing" framework'üdür.

Saga yönetimini "__Eventuate Tram Sagas__" framework'ü yapmaktadır.

Saga yönetimini "__MicroProfile Long Running Actions (or LRA)__" framework'ü sunmaktadır. LRA "transaction manager" olarak "Narayana" framework'ünden yararlanmaktadır.

# Axon framework
Java için açık kaynaklı CQRS, Saga, "event sourcing" pattern'lerini kolayca uygulamaya yarayan bir framework. CQRS, Saga, "event sourcing" pattern'leri manuel olarak yazılımcı tarafından da yapılabilir(implemente edilebilir). fakat framework'ün sunduğu birçok avantaj'da vardır.

Axon'un JVM üzerinde çalışan bir sunucusu vardır. her uygulama sunucuya bağlanır. sunucunun içinde event bus mekanizması vardır. bu event bus, yazılımdaki event'lerimiz tutmaya yaramaktadır. Sunucu, event'leri görüntülemek için web GUI'de sunmaktadır.

Axon'da CQRS ve Saga opsiyonel iken; "event sourcing" kullanımı zorunludur. event sourcing metotları boş bırakılır ve her event sourcing işleminde aggregate objelerimiz JPA'ya basılırsa, "event sourcing" kullanılmamış varsayabiliriz. fakat böyle bir durumda sadece event base programming yapmış oluruz ve Axon kullanmamızın pek anlamı kalmaz.

Axon mikroservislerde veya monolitik uygulamalarda da kullanılabilmektedir.

Axon sunucusunun event bus'ında, Command Bus, Event Bus ve Query Bus sürekli çalışmaktadır.

Axon'da temel kod örneklerine bakalım:

```java
import org.axonframework.commandhandling.CommandHandler;
import org.axonframework.eventsourcing.EventSourcingHandler;
import org.axonframework.modelling.command.AggregateIdentifier;
​
import static org.axonframework.modelling.command.AggregateLifecycle.apply;
​
public class GiftCard {
​
    @AggregateIdentifier // bu değer ID'nin bu aggregat için unique olduğunu belirtiyor. eğer birden fazla event/command çağrılırsa, "points" değeri aynı ID'li tüm aggregate işlemleri için ortak olacaktır.
    private String id;
    private int points;
​
    @CommandHandler // CQRS'teki "command" e denk gelmektedir.
    //her command metotunun içinde bir den fazla event olabilir.
    // business logic katmanlarından (business logic bu class'ta değil. business login'ler buraları çağıracak) event çağıramayız. command çağırmak durumundayız. her command kendi içinde zaten birçok event'i gerekiyorsa çağıracaktır.
    // command metotları "decision-making/business logic" kodları içermelidir.
    public GiftCard(IssueCardCommand cmd) {

       // apply ile bir event çağırıyoruz. event'i çağırıyoruz. event'i çağırırken, parametreleri event CardIssuedEvent objesi içerisinde gönderiyoruz.
       apply(new CardIssuedEvent(cmd.getCardId(), cmd.getAmount()));
    }
​
    @EventSourcingHandler
    // CardIssuedEvent tetiklediğimiz event tamamlandığında çağrılacak metottur.
    // bu metot içerisinde GiftCard objeminiz güncelleriz. GiftCard bizim database objemiz değildir. GiftCard şu anda Axon yapısının gerektirdiği bir sınıftır/yapıdır.
    // "state changes" lerin bulunduğu metotlar EventSourcingHandler metotlarıdır. state change'leri DB'deki ye yazılmaz. DB'ye yazacağımız metotlar farklı yerde.
    // "state changes" kesinlikle yukarıdaki command metotlarında yapılmamalı.
    // command metotları genelde state'i kontrol edip, event tetiklemelidir. eğer state te bir hata varsa, command metotu exception fırlatmalıdır.
    public void on(CardIssuedEvent evt) {

        // burada GiftCard sınıfının bilgilerinı güncelleriz. bu şekilde bu sınıfta (GiftCard sınıfında) olan diğer metotlar(command metotlar) da güncel olarak isteği bilgileri okuyabilecektir.
        id = evt.getCardId(); // bu işlemin en öncelikli yapılması lazım ki; diğer event'ler aynı GiftCard property'lerini okuyabilsin.
        points = 0;
        //bu adımda database'deki GiftCardDB objemiz güncellenmemiştir. bu adım sadece GiftCard sınıfı (bu sınıf) içindeki bilgileri günceller.
    }
​
    protected GiftCard() {
      // boş constructor Axon framework'ü tarafından zorunlu tutuluyor.
    }
}
```

```java
import org.axonframework.modelling.command.TargetAggregateIdentifier;
​
public class IssueCardCommand {
​
    @TargetAggregateIdentifier // bu değer, GiftCard objesindeki ID'ye denk geldiğini gösteriyor. mutlaka bu olmalı.
    private final String cardId;
    private final Integer amount;
​
    // constructor / getters / setters
}
```

# State storage
yukarıdaki kod örneğinde; "points" ve "id" değerleri storage'dir. storage'de tüm instance'ların değerleri mevcuttur. bu storage kısa süreli tutulması beklenen bir storage'dir. kalıcı olacak olan bilgiler zaten her event sonrası yada command sonrası database'ye yazılmalıdır. state storage'deki her değeri, o cammand'in çağırdığı her event okuyabilir. fakat dışarıdan kimse okuyamaz.

# event storage
her event'in kaydedildiği database'dir. Herhangi bir database kullanılabilir. fakat normal uygulamada çok fazla event olacağından (herşeyin modüler ve event yapısında olduğu düşünülürse), database'yi buna uygun şekilde seçmekte yarar var. AxonServer'ın kendi içinde buna spesifik geliştirilmiş bir API var. bu API bilgileri tutuyor ve yine select sorgusu atılabilmesini sağlıyor.

# Multi-entity Aggregates

Aggregate Root objemiz GiftCard olsun, onun altında bir çok transaction işlemi olsun. Bunu Axon ile bu şekilde ifade ediyoruz:

```java
import org.axonframework.modelling.command.AggregateIdentifier;
import org.axonframework.modelling.command.AggregateMember;
import org.axonframework.modelling.command.EntityId;
​
public class GiftCard {
​
    @AggregateIdentifier
    private String id;
​
    @AggregateMember
    private List<GiftCardTransaction> transactions = new ArrayList<>();
​
    private int remainingValue;
​
    // command handlers, event sourcing handlers
}
​
public class GiftCardTransaction {
​
    @EntityId
    private String transactionId;

    private int transactionValue;
    private boolean reimbursed = false;
​
    public GiftCardTransaction(String transactionId, int transactionValue) {
        this.transactionId = transactionId;
        this.transactionValue = transactionValue;
    }
​
    public String getTransactionId() {
        return transactionId;
    }
​
    // command handlers, event sourcing handlers
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Lombok

Lombok Java için birçok ek özellik sağlayan altyapıdır. jar olarak classpath'e eklendiğinde build sırasında devreye girmektedir, bu şekilde özelliklerini devreye sokabilmektedir.

örneğin;
- getter-setter'ları bizim için oluşturmaktadır.
- builder oluşturmaktadır
- toString oluşturmaktadır
- equals fonksiyonlarını oluşturmaktadır

özelliklerin tüm listesi buradadır: https://projectlombok.org/features/all

IDE'nin derlemeden önce bazı şeyleri görebilmesi için Lombok'un IDE eklentisinin yüklü olması gereklidir. IDE'de Lombok-eklentisi yüklü ise aşağıdaki kodda hata almayız.

```xml
<dependencies>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.4</version>
        <scope>provided</scope>
    </dependency>
</dependencies>
```

```java
public class UserIntegrationTest {

    @Test
    public void givenAnnotatedUser_thenHasGettersAndSetters() {
        User user = new User();
        user.setFirstName("Test");
        assertEquals(user.gerFirstName(), "Test");
    }

    @Getter @Setter
    class User {
        private String firstName;
    }
}
```

IDE eklentisi, direk kodları da generade ediyor. yani sadece eklenti olursa, dependency'ye ihtiyaç kalmadan ilgili kodları generate edebiliyor. bu özellik zaten IDE'lerin içinde var.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ArchUnit
Java test kütüphanesidir. derlenen projedeki Java sınıflarının mimarisini kontrol edip, mimari açıdan uygun olup olmadığını test edebilmemiz sağlar.

örnek:
- arayüzlerin isimleri şunlarla başlamalıdır
- implemetasyonların prefix'i şu olmamalıdır
- standart stream'lere  (system.out gibi) hiçbir yerden erişilmemiş olmalıdır

örnek:

aşağıdaki örnek 3 farklı test vardır. büyük harfle yazılanlar ArchUnit içerisinde yüklü gelen default mimari testler.
3'üncü loggers_should_be_private_static_final isimli fonksiyon'u ise biz tanımladık.

bu testleri JUnit içerisinde çağırıp koşabiliriz.

```java
@AnalyzeClasses(packages = "com.tngtech.archunit.example.layers")
public class CodingRulesTest {

    @ArchTest
    private final ArchRule no_generic_exceptions = NO_CLASSES_SHOULD_THROW_GENERIC_EXCEPTIONS;

    @ArchTest
    private final ArchRule no_java_util_logging = NO_CLASSES_SHOULD_USE_JAVA_UTIL_LOGGING;

    @ArchTest
    private final ArchRule loggers_should_be_private_static_final =
            fields().that().haveRawType(Logger.class)
                    .should().bePrivate()
                    .andShould().beStatic()
                    .andShould().beFinal()
                    .because("we agreed on this convention");
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Android Studio

IntelliJ IDEA üzerine ek olarak; Android geliştirme ortamı için eklentiler ile Google tarafından dağıtılan IDE.

# Android Developer Tools (or ADT) vs Andmore

Android plugin for Eclipse. It was developing by Google. Google now discontinued the project because they are developing plugins for IntelliJ Idea. Now the community fork the ADT project with new name: Andmore.

# project Structure of IDE's

birbirine denk gelen terimler aşağıdaki tabloda verilmiştir:

```
Eclipse       -> Workspace | Project

IntelliJ      -> Project   | Module

Visual Studio -> Solution  | Project

XCode         -> Workspace | Projects
```

- Android Studio, IntelliJ türevi olduğu için aynı mantıkta çalışmaktadır.

- IntelliJ bir pencerede birden fazla proje açmıyor. sadece 1 proje ve onun alt modülleri olabilir. IntelliJ aynı projeyi 2 farklı pencerede açılmasına da izin vermiyor. sadece sekmeler farklı sekmelerde açılabiliyor.

- IntelliJ'de Eclipse'teki gibi perspektifler yoktur.

# Maven/Gradle/IDE proje dizin yapıları hakkında
saf IDE'siz sadece Gradle Android projesi oluştursak bir Gradle dosyası yeterli. Gradle, Maven gibi altprojeleri destekliyor. "Android studio" proje dizininde Gradle görürse alt modülleri daha iyi yönetebilir. bu sebeple React-native gibi komut satırı uygulamaları Android studio'ya uygun Android projesi yaratır. yine bu sebeple Android kodlarımızın olduğu proje bir module olmaktadır. modülümüzün bir üst dizini (Android studio için project dizini) kodsuz bir Gradle projesidir. bu Gradle'nin alt modülü bizim Android projemizdir. eğer Android'e bir library yazmak istersek onu da Android ile aynı seviyede yeni bir modül olacak şekilde ayarlamalıyız.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# CPU Scheduling algorithms (or process Scheduling algorithms)

# time slice (or quantum)
işlemin CPU'da kesintisiz olarak kalacağı minimum süredir.

# algorithms

  - ## Round-robin (or RR)
    round kelime anlamı: tur

    robin kelime anlamı: özel bir kuş türü

    quantum süresi vardır. tüm işlemler kuyruğa alınır. ilk işlem kuantum süresinde CPU'da kalır ve kuyruğun en sonuna atılır ve yeni gelen process varsa yine kuyruğun sonuna atılır. quantum bittiğinden dolayı tekrar kuyrukta sırada bekleyen işlem, CPU'ya alınır. bu işlemde kuyruğun sonuna atılır ve yeni gelen işlem varsa kuyruğun sonuna atılır...

  - ## First-Come, First-Served (FCFS) Scheduling
    İlk gelen kesintisiz olarak CPU'da kalır ve işlemin tamamlanması beklenir.

  - ## Shortest-Job-Next (SJN) Scheduling (or shortest job first or SJF)
    her zaman o anda kısa olan process tamamlanana kadar CPU'ya girer.

  - ## Shortest Remaining Time
    SJN'ye benzer. fakat quantum süresi vardır. quantum süresi her tamamlandığında, en kısa olan process işleme alınır.

  - ## Priority Scheduling
    her işlemin bir önceliği vardır. kalan zaman, arrival time gibi değerlerin yanında priority değerine bakarak ta hangi process'in CPU'ya gireceği karar verilir. bunun birçok implementasyonu vardır.

  - ## Multiple-Level Queues Scheduling
    birden fazla CPU kuyruğu yaratılır. her kuyruğun kendi içinde farklı algoritması olabilir. ve her 2 kuyruktan işleme alınacak değer seçilirken de farklı algoritmalar uygulanabilir. yani her kuyruk kendi içinde hangi process'in işleme gideceğini belirler. daha sonra her kuyruktan sunulan/belirlenen işlemler arasında da bir seçim yapılır ve CPU'ya tek bir işlem yollanır. bu sebeple bu algoritmanın da birçok implementasyonu vardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# false positive (or type I error) vs false negative (or type II error)
medical testlerde, binary classification ve istatistik bilimlerinde kullanılan terimlerdir.

bir data incelenir (örnek: OCR ile bir karakter tespiti yapılmaya çalışılsın, yada hastalığın tanısı koyulama çalışılsın). inceleme sonucu X kararına varılır (örnek: OCR sonucu karakter 'A' olduğuna kanaat getirilsin, yada hastalığın zatüre olduğuna karar verilsin).

Eğer gerçek sonuç X ten farklı ise; bu duruma 'false positive' denir.

İnceleme sonucu 'X değil' kararına varılır (örnek: OCR sonucu 'A karakteri değil', yada 'hasta zatüre değil' kararına varılır).

Eğer gerçek sonuç X ise o zaman bu duruma 'false negatif' denir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Appmon
__Dynatrace__ firması tarafından geliştirilen "Application Performance Monitoring" yazılımıdır. örneğin; Java web uygulamamızı takip eder ve ona gelen istekleri gruplayıp, dashboard'larda detayları gösterir. stacktrace'e kadar analiz edilmesini sağlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Jhipster
app generator command line tool (boilerplate) for Spring boot microservices (netflix), Angular, React, Bootstrap, Maven/Gradle, Elastic Stack, Docker.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ramdisk
RAM, SSD'lerden dahi çok daha hızlı. bir yazılım RAM'de bellek ayırıp (bellekte sanal bir dosya sistemi yaratıp), bunu hard-drive mış gibi OS'a mount edebilir. tıpkı bir USB bellek gibi. işte bu RAM'deki belleğe ramdisk denir.

piyasada bir çok ramdisk görevi gören uygulama mevcut.

Linux'taki "mount" komutu ramdisk ihtiyacını giderebiliyor. örnek:

```sh
mkdir "/dir_to_mount"
mount -o size="16G" -t tmpfs none "/dir_to_mount"
```

mount komutu RAM'de 2 farklı sanal file system destekliyor: TmpFS ve RamFS (TmpFS'e göre daha eski)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# CGI (or Commond Gateway Interface)
server'da istekleri karşılayıp, istekteki URL'deki path'lere göre ilgili uygulamaya isteği yönlendiren yazılımdır. ISS, JavaEE sunucuları gibi gelişmiş sunucular çıktığında beri, özel durumlar hariç kullanılmıyor.

CGI'nin RFC'si vardır. yani belli standartlardadır. protokol hakkında detaylar RFC'de belirtilmiştir.

CGI kullanan birçok yazılım mevcut: mod_perl, mod_php4, mod_fastcgi

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Bottleneck (or darboğaz)
bu terim bilişim dünyasında bu tarz durumlarda kullanılmaktadır:
- network'e gelen istekler internet hızımız sebebi ile yavaşlaması. bu durumda network darboğaz edilir.
- bilgisayarda bir process CPU ve HDD aynı oranda hızlı olmadıklarından dolayı (örnek HDD yavaş ise), process'in yavaşlaması. bu durumda HDD darboğaz edilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# twelve-factor app

12factor.net'te yayımlanan, birçok contributer'ın desteği ile genel olarak tüm software-as-a-service yazılımlarda uyulması gereken 12 temel kural yer almaktadır. her kural 1-2 sayfa uzunluğunda açıklanmıştır. özetle:

- Codebase (Kod Tabanı)

  - tüm deploylar aynı code-base'den olmalı.
  - birkaç uygulama, aynı kodları içeriyorsa mutlaka o kodlar common lib yapılmalı.
  - sürüm kontrol sistemi uygulaması kullanılmalı.

- Dependencies (Bağımlılıklar)

  - bağımlılıklar net olarak belirtilmeli.
  - OS'tan kütüphane kullanılacak ise, bu uygulamanın içine gömülmeli.

- Config (Yapılandırma)

  configler mutlaka codebase'den ayrı ve prod, test gibi ortamlar için ayrı gruplanmalıdırlar

- Backing Services (Destek Servisleri)

  dış kaynaklar/hizmetlere gidebilmek için sadece config'lerin değiştirilmesi yeterli olmalıdır. örneğin; farklı bir database'ye giderken codebase'de değişiklik yapılmamalıdır. bir config değişikliği yetmelidir.

- Build, Release, Run (Derleme, Sürüm, Çalıştırma)

  - başlıktaki her aşama için ayrı ayrı pipeline olmalıdır. - eski sürüme dönülebilmelidir.
  - build numaraları unique olmalıdır ve sürüm çıkıldıktan sonra aynı sürüm numarası ile paket güncellenmemelidir.

- Processes (Süreçler)

  - stateless(durumsuz) ve shared-nothing(bir şey paylaşmayan) olmalıdırlar. Uygulamanın kullandığı herhangi bir veri, process'lerde (RAM, cache, dosya sistemi...) değil, veritabanlarında saklanmalıdırlar. Burada veritabanı denmesinin sebebi, veritabanının 3üncü parti bağımsız bir sistem olması ve HDD'ye yazma özelliği olduğu için kalıcılığı olmasıdır.

    Eğer RAM, cache, dosya sistemi veya benzeri bir yere bir bilgi kaydedilmiş ise (ki bunu yapabiliriz), uygulamamız ilerdeki süreçte, o bilginin mutlaka orada olacağına göre hareket etmemelidir.

  - Örneğin; "sticky sessions" gibi teknikler kullanılmamalıdır çünkü bu kuralı ihlal eder.

- Port Binding (Port Bağlama)

  hizmetleri dışarıya sadece port (TCP gibi...) açarak yapmalıdır.

- Concurrency (Eş Zamanlılık)

  aynı uygulamadan birden fazla açılabilmelidir.

- Disposability (Kullanıma Hazır Olma Durumu)

  disposable kelime anlamı: tek kullanımlık.

  - birkaç saniye içerisinde uygulama ayağa kalkabilmelidir.

  - SIGTERM yollandığında graceful (zarif) şekilde kapanmalıdır. eğer http request'i process ediliyorsa, bitmesini bekleyip o şekilde kapanmalıdır. Uygulamanın birkaç saniye içerisinde kapanması gerekiyor. Tabi bunun için HTTP isteklerinin cevap süreleri de çbirkaç saniye olmalıdır. eğer bu sürenin uzaması gerekiyorsa farklı çözümlere gidilmelidir (client'ın tekrar tekrar istek atıp süreci tamamlayabileceği çözümler olmalıdır).

  - Uygulama SIGTERM almadığı fakat OS veya donanımsal sebeplerle kill edildiği durumlara da hazırlıklı olunmalıdır.

- Dev/Prod Parity (Geliştirme/Üretim Ortamlarının Eşitliği)

  - dev ortamında kullanılan tool'lar ile prod/test ortamında kullanılan tool'lar mümkün oldukça aynı olmalıdır.
  
  - DevOps kültürü mutlaka olmalıdır.

- Logs (Günlükler)

  - her uygulama mutlaka sysout'a yazıyor olmalı. oradan sonrasına karışmıyor olmalı. oradan sonrasına OS veya OS'ta yüklü olan bir yazılım ilgilenmelidir. Yani; uygulamamız log dosyalarını yönetmemelidir.

- Admin Processes (Yönetici Süreçleri)

  - bir kerelik yapılması gereken işlemler olabiliyor. bu işlemleri komut satırı script'leri ile yapmalıyız (burada JVM dilleri, NodeJS... gibi script dilleri tercih edilebilir).

  - Bu admin-script'leri tüm ortamlarda aynı code base üzerinden çalıştırmalıyız.

  - Bu admin-script'leri için yine dependency'lerimiz net olmalı.

  - bu admin-script'leri kendileri bir izole ortamda bağımsızca çalışmalı (örneğin bir container'da çalışmalı).

  - bu admin-script'lerinin build, deploy gibi süreçleri CI/CD'ler ile yönetilmeli.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# metasearch engine
startpage.com ve Searx projesi buna bir örnektir. diğer arama motorlarını kullanarak sonuçları yansıtan arama motorlarıdır. kendi veritabanlarını bulundurmazlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Apache Hadoop

açık kaynaklı bir framework'tür. MapReduce mantığını temel alarak çalışır ve big-data'mız üzerinde işlem yapabilmemizi sağlar. merkezi servisimize istek gelir, Hadoop Java API'miz ile Hadoop sorgumuzu yazarız ve data'ları çekmek için Java-API her node'a istek atar. her node HDFS dosya sistemi ile bilgilerimızı tutmaktadır. her node kendi içerisinde bilgiyi bize (merkezi noktaya) döndürür. bu şekilde merkezi servisimizde yük diğer node'lara paylaştırılmış olur.

Hadoop'un Mödülleri:

- common

  utilities like Java API (lib)

- HDFS (or Hadoop Distributed File System)

  veritabanının formatıdır. bu tek bir makinede olmak zorunda değildir. birçok farklı lokasyonlardaki birçok farklı makinede parçalı şekilde bulunabilir.

Hadoop kendi içerisinde Zookeeper dan yararlanmaktadır. Hadoop'u kaldırmadan önce Zookeeper'ı ayağa kaldırmak gerekir. Zookeeper key/value bir storage manager'dır. (fakat piyasada geliştiriciler, açık olan web servisleri Zookeeper storage'sinde tutup Zookeeper'ı bir "servis discovery" olarak ta kullanmaktadırlar.)

# MapReduce

map ve reduce olarak iyi ayrı işlemi temsil eder. map, sorgunun başlangıcında HDFS'den topladığımız her değeri döner ve bu değer üzerinde değişiklik yapmamızı sağlayan fonksiyondur. reduce işlemi ise, map sonunda elde ettiğimiz tüm değerleri alır ve sonunda bir değer döndürmemizi sağlayan fonksiyondur. bu iki fonksiyon birbirini tamamlamaktadır.

# MongoDB built-in MapReduce example

aşağıdaki kod için kaynak: https://www.mongodb.com/docs/manual/core/map-reduce/

```js
// orders is a collection stored on mongoDB servers.

db.orders.mapReduce(

  // map function
  // Bu sunucu tarafta yapılan 2inci faz.
  // key(cust_id), value(amount)'ları Map< CustomerId, List<Amount> > şeklinde tutuyor. yani bir key'e birden fazla value gelebilir.
  function() { emit( this.cust_id, this.amount ); },

  // reduce function
  // Bu sunucu tarafta yapılan 3üncü faz.
  // map'ten dönen Map'i buradaki fonksiyona geçiyor.
  // Her function sonucunu, order_totals (client'a dönülen collection-tablo) içerisine 1 satır olarak ekliyor.
  function(key, values) { return Array.sum(values) },

  {
    query: { status: "A" }, // orders'lardan sadece status'u "A" olanlar işleme alınacak. Bu sunucu tarafta yapılan 1inci faz. 
    out: "order_totals" // tüm işlemler yapıldıktan sonra client'a döndürülen tablo (collection) ismi.
  }
)
```

MongoDB'de, Map-reduce fonskiyonu JavaScript dilinde yazılabilmektedir. 

Map-reduce'un özelliği işlemin tek bir makine tarafından işlenmemesi ve node'lara dağıtılmasıdır. Örneğin; yukarıdaki örnekteki tüm fonksiyonlar (map, query, reduce), birçok node'da paralel koşmaktadır.

Map-reduce işlemi RDBMS'lerde yapılırsa, hiç verimli olmamaktadır. Çünkü mantığına terstir. Her fonskiyonun başka farklı node'larda birbirinden bağımsızca RDBMS'in yapısına göre terstir. Her node'daki işlem görecek olan data parçacığının (satırların) ilişkisinin başka node'da olmaması gibi, sadece no-sql'e özgü yapısı olmalıdır. Eğer öyle bir datamız varsa, zaten baştan NoSQL kullanmamız gerekirdi.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Mesasuring maintainability

  - # line of codes (or LOC or source lines of code or SLOC)

  - # cyclomatic complexity

    if içinde if olma oranıdır. bu oran yükseldikçe maintainability düşer. Cyclomatic Complexity özel bir algoritma ismidir. düz if sayısının hesabı yapılmamaktadır.

  - # depth of inheritance tree (or DIT)

    sınıfların birbirinden extend etme oranıdır. ne kadar çok üst sınıftan extend edersek bakım maliyeti o kadar düşer.

# code smell
bu terim kodun best practice'ler ile yazılmadığını belli eden karakteristikleri için kullanılır. "kodun kötü kokması" manasında kullanılan bir terimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Greenfield systems vs Brownfield systems
Greenfield projeler sıfırdan başlanılan projelerdir. Brownfield ise var olan sitemin güncellenmesi gibi durumları temsil eder.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Microsoft Windows için paket yöneticileri

- # Chocolatey
Komut satırından çalışır.

Microsoft Windows için açık kaynaklı komut satırı paket yöneticisidir.

programları standart olarak program files içerisine kurar. fakat bu dizin istenirse her program için ayrı ayrı override edilebilir. kurulan programlar, normal koşullarda Microsoft Windows dizinine dll atıyorsa, Chocolatey ile kurulursa da aynı şekilde dll atarlar. Chocolatey native setup/installer'ı çalıştırır. çalıştırırken yüklenecek dizinleri override eder.

Burada birçok açıklama mevcut: (source-id: 35)

- # Scoop
Komut satırından çalışır.

Chocolatey'e alternatiftir. fakat Linux'taki Nix benzeridir. uygulamaları portable olarak HOME dizinine kurar. PATH değerine kendi oluşurduğu bir klasör ekler. Böylece komutlar dışarıdan erişilebilir olur. Başlat menüsüne de, kısayolları kendi atar.

Scoop admin-haklarına sahip isek, program-files içerisine de programları kurabiliyor (normal installer'ları çalıştırıyor arkada).

- # winget
Microsoft'un geliştirdiği komut satırı paket yöneticisidir. Chocolatey alternatifidir. Sadece uygulamanın resmi instller'ını çalıştırır. Programlar program-files'a kurulur.

# Microsoft Windows installers
- InstallShield
- Nullsoft Scriptable Install System (or NSIS): Winamp'ı geliştiren Nullsoft firması tarafından geliştirilmiştir.
- Microsoft Windows Installer: uzantısı .msi'dir. Microsoft'un resmi installer'ıdır. diğer installer'lar msi formatını kullanamaz. diğer installer'lar .Exe formatını kullanmak zorundalar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# BCD code (or Binary Coded Decimal code)
onluk sistemindeki bir sayının her basamağın ayrı ayrı ikilik tabana çevrilerek yazılmış halidir. 2 çeşit BCD vardır:

- Genişletilmiş (Uncompressed) BCD

Her basamak 1 byte ile temsil edilir. örnek:

```
Onluk Taban: 91
BCD: 0000 1001 0000 0001
```

9; 1001'a eşit olduğundan her zaman 1 byte'ın yarısı boşa gitmektedir.

- Sıkıştırılmış (Packed) BCD

Her basamak 4 bit ile temsil edilir.

```
Onluk Taban: 91
BCD: 1001 0001 (aradaki boşluk olmamalı. okunabilirliği arttırmak için boşluk bırakıldı.)
```

```
Onluk Taban: 123
BCD: 0000 0001 0010 (aradaki boşluk olmamalı. okunabilirliği arttırmak için boşluk bırakıldı.)

min 8 bit olarak kullanılan bir makinede yukarıdaki BCD değerini sol tarafına 4 bit koymak zorundayız. sonuçta bunu elde ederiz:
BCD: 0000 0000 0001 0010 (aradaki boşluk olmamalı. okunabilirliği arttırmak için boşluk bırakıldı.)
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# eşlik biti (or parity bit or check bit)
binary data kümemizin başına yada sonuna konular bir çeşit kontrol bitidir. data kümemizde bulunan toplam 1 sayısı tek sayı ise 1, çift sayı ise 0 olarak eşlik biti kaydedilir (tersi de olabilir: eğer tek sayı için 0 verildiyse, çift sayı 1 için 1 kullanılır). bu şekilde bilginin değişip değişmediği güvence altına alınmış olur. genelde iki bilgisayar arası haberleşme protokollerinde bu yöntem kullanılır. örnek:

| kontrol edilen data | count of "1" bits | data including parity |
|---------------------|-------------------|-----------------------|
| 0000000             | 0                 | 00000000              |
| 1010001             | 3                 | 11010001              |
| 1101001             | 4                 | 01101001              |
| 1111111             | 7                 | 11111111              |

Parity biti 1 bit değişikliğe uğrarsa, hatayı tespit edebilmemizi sağlıyor. fakat 2 bit değişikliğe uğramış ise o zaman hatayı tespit edemeyiz. dolayısı ile parity önemsiz data kontrollerinde kullanılmalıdır çünkü %100 doğruluğu yoktur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# media center
Youtube'den video izleme, network'teki birçok protokol ile (örnek: Samba) paylaşılmış media dosyalarını direk açma, yayın protokollerini izleme, local media dosyalarını yönetme izleme gibi birçok media işlevini yerine getiren birçok modülü olan uygulama.

# Kodi (or old-name:XBMC)
Cross platform, açık kaynaklı media center uygulamasıdır.

# Microsoft Windows Media Center
Microsoft'un media center uygulamasıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Module (or modül) vs Component (or Bileşen)
Bu terim piyasada birbirinin yerine kullanılıyor. fakat farklı kavramlardır.

Bir modül sisteme load edildiğinde, derlenmiş bir kod load edilmiş olur. Oysa component load edildiğinde, high-level logical bir yapı load edilmiş olur.

Burada bu konu ile ilgili bilgi mevcut: book: "OSGi in Action", title: "11.1.1 What are components", page:347.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# context (or tr:kontekst or bağlam)
birçok API'de context terimi geçer. context bulunan platform hakkında data bilgisidir. örneklerden gidersek daha net anlaşılır:

- servlet'e bir request geldi. servlet service metotu çalıştı 2 adet parametre aldı: request, response. service metotu içinden Servlet sınıfının getContext metotu çağrılabilir. getContext bize asıl amacımız olan request ve response dışındaki tüm data'yı sağlar. getContext Servlet'e (Servlet bu senaryoda platformumuz oluyor) geçilmiş parametreleri döndürüyor.

- Kubernetes'te komut satırından işlem yaptığımızda asıl amacımız container ağa kaldırmak, silmektir... Kubernetes'te context değiştirdiğimizde çalışma ortamımızın (platformumuzun) ayarlarını değiştiririz.

- Android'te context bir sınıftır. Bu sınıf service, activity gibi sınıfların super class'ıdır. context en tepededir ve uygulama seviyesinde (platform seviyesinde) bilgileri/metotları içerir. Android platformundaki Android.content.Context sınıfının içindeki yorumlara bakacak olursak:

```
/**
 * Interface to global information about an application environment.  This is
 * an abstract class whose implementation is provided by
 * the Android system.  It
 * allows access to application-specific resources and classes, as well as
 * up-calls for application-level operations such as launching activities,
 * broadcasting and receiving intents, etc.
 */
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Raft algoritması
Raft kelime anlamı: sal (deniz taşıtı).

- Raft, consensus (fikir birliği, uzlaşma) algoritmasıdır.
- İsmi bu kelimelerin baş harflerinden oluşur: 'reliable', 'replicated', 'redundant' And 'fault-tolerant'.
- Raft, __Paxos__ algoritmasına dayanır. PAXOS algoritmasının birçok türevi daha çıkmıştır (örnek: Disk Paxos, EPaxos...)
- Raft algoritması node'ların aralarında nasıl anlaşmaya varacaklarını belirleyen protokol için kullanılmaktadır.
- Örneğin; "Consul" yazılımının mimarisinde, her programcık, 'Consule agent'ını (kütüphanesini) bulundurmak zorundadır. Consul agent'lar consul server'lara bağlanır. sistemde bir çok "consul server" vardır. 'consul server'lar ise kendi içlerinde lider seçerek haberleşirler.

  1 agent birden fazla consul server'a bağlı olabilir. bu şekilde birine birşey olduğunda diğerinden bilgi almaya devam eder.

Raft'ta her node (yukarıdaki örnekteki "consul server") 3 farklı state'de olabilir:
- follower
- candidate (aday)
- leader

İlk başta her node, follower'dır. eğer bir liderden haber alamazlarsa, kendilerini candidate olarak belirleyebilirler. candidate olan node, diğer node'lara vote için request yollar. Bu olaya "__leader election__" denmektedir.

Sistemde bir data güncellemesi yapacak olalım. x=1 yapalım. client lidere x=1 bilgisini yolluyor. bu güncelleme her node'a leader tarafından yollanıyor. eğer çoğunluk (majority) dönerse, lider tekrar herkese bu bilginini herkes tarafından güncellendiğini onayladığı mesajını yolluyor. herkese bu bilgiyi yolladığında client tarafa da bilginin güncellendiğini bildiriyor. bu olaya "__Log Replication__" deniliyor. Lider tarafından herkese yollanan data güncelleme request'ine __Append Entry message__ deniliyor.

  - ## some timeouts

       - ### election timeout
         Raft algoritmasında lider seçmek için her follower'ın candidate olması için random bir süre vardır. genelde 150ms ile 300ms arasında değişir. bu şekilde tüm sistem aynı anda candidate olmamış olur.

       - ### heartbeat timeout
         lider sürekli olarak heartbeat ile follower'ları kontrol eder ve hepsinden dönüş alır. eğer follower'lar heartbeat isteğini liderden almazlarsa, o zaman yeniden o follower'lar election'a gidecektir. heartbeat için follower tarafında da bir timeout vardır.

# majority of votes
lider seçilirken, candidate'in en az sistemdeki node sayısı + 1 kadar oy alması gerekir. eğer bu sayı daha düşük olursa o zaman farklı bir candidate'te kendini lider olarak kabul edebilir.

# network sorunları
node'lar birbiri ile haberleşirken, node'ların bir kısmının birbiri ile iletişiminin kesildiğini düşünelim. böyle bir durumda her iki grup kendi içinde lider seçecektir. client hangi lidere data güncelleme bilgisi yollarsa, artık bilgisi sadece o grupta güncelleniyor olacaktır. bir süre sonra 2 grubun network erişimi tekrar düzelir ve birbirleri ile iletişime geçerlerse, iki grubun lideri daha çok oy toplamış bir grubun lideri karşısında kendini follower'a çekecek ve kendi güncellemelerini roll-back edecektir. yeni güncellemeleri lider'den alacaktır.

# Usage on cryptocurrency
sanal para sistemlerinde, lider seçimi yoktur. bu sebeple consensus algoritmaları kullanılsa bile, kripto para dünyasında lider seçimi için kullanılmaz. ancak çoğunluğu toplayıp data kabulu için Raft veya Paxos benzeri algoritmalar kullanılabilir. daha da doğrusu, data kabulu sürekli olarak yapılır. 1 lider seçip, o lider çökene kadar herkes onun dediklerini doğru kabul etmez.

İkinci önemli fark ise, Raft ve Paxos'ta her node güveilir olarak kabul edilirken, kriptoparalarda her node güvenilir kabul edilmez. Kriptoralarda %51'in üstünde oy alan adata doğru kabul edilir gibi farklı yöntemlere gidilmektedir.

# Gossip protocol (or epidemic protocol)
gossip kelime anlamı: dedikodu.

epidemic kelime anlamı: salgın.

peer-to-peer iletişim protokolüdür. Bu algoritmada; bir bilgi dağıtılmak istendiğinde, sadece 1 node'dan herkese yollanmaz. Diğer bazı node'lara bilgi yollanır, o node'larda diğer node'lara yollar.

Örneğin Cassandra, bu protokolü kullanır. her 1 node, etrafındaki 3 node'un bililerini tutar. Belirli saniyelerde bu işlem sürekli tekrarlanır. Eğer cluster'daki 1 node zarar görürse, artık bilgiler diğer 2 node'da da varolduğundan, oradan datayı okuyabileceklerdir. Burada Cassandra şu bilgiyi birbiri ile paylaşıyor: Hangi node'da hangi satır aralığındaki bilgiler var (Cassandra'da aynı tablonun farklı satırları farklı node'larda tutuluyor. Bu konu farklı başlıkta anlatılıyor).

# Sybil saldırısı (or Sybil Attack)
ismini, kişilik bozukluğu olduğu bilinen bir kadın hakkında yazılmış "Sybil" isimli bir kitaptan alıyor.

bu tarz saldırıları engellemek için farklı yöntemler tercih edilebilir:
- Kimlik Oluşturma Maliyetini Yükseltme. Örnek implementasyonlar: Proof Of Stake, Proof Of Work)
- itibar sistemi (sistemde eski olana daha çok güvenilir)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Cycle detection

- ### Floyd's Tortoise and Hare

  Floyd tarafından geliştirilen bu yöntemde kaplumbağa ve tavşan birer pointer'dır.

  Aşağıdaki kod üzerinde Floyd algoritması uygulanmış ve kaplumbağa=slow, tavşan=fast olarak isimlendirilmiştir.

  Floyd algoritmasında, tavşan kaplumbağadan önce döngü içerisine girecektir ve döngü içinde birbirlerine muhakkak denk geleceklerdir. algoritmanın kodlaması çok basittir. algoritma çalışmasında ek belleğe ihtiyaç duyulmaz. sadece 2 pointer vardır.

  Aşağıdaki şekilde döngüsel LinkedList'imiz olduğunu varsayalım.
  - Çizgiler x uzunluğunda (x adet node olsun),
  - eşittir işaretleri y uzunluğunda
  - yıldız z uzunluğunda

  Eğer tavşan ve kaplumbağa yıldızlı node'ların başında buluşuyorsa;
  - Distance travelled by slowPointer before meeting = x+y
  - Distance travelled by fastPointer before meeting = (x+y+z)+y = x + 2y + z

  Bu durumda;

  > 2 * dist(slowPointer) = dist(fastPointer)

  > x = z çıkmaktadır

  Slow ve fast buluştuğunda liste döngüsel demektir. Bunun sebebi yukarıdaki denklem değil. Yukarıdaki denklem ek olarak bizim döngüdeki döngünün başladığı noktayı bulmamızı şu şekilde sağlayacak: Eğer slowPointer'ı dizinin en başına koyarsak ve her iki pointer'ı da artık aynı hızda ilerletirsek, 2 pointer döngünün başladığı nokta olan kısımda (bizim şeklimizde eşittir işaretinin en başı) buluşacaklardır. çünkü x=z'dir.

  Aşağıdaki Java kodu sadece listenin döngüsel olup olmadığını tespit etmektedir.

```
    <...x...>                      <....y
________________======================  :
                *                    =  :
                *                    =  v
                *                    =
                **********************
                  <...z...>
```

```java
public class App {

  public static void main(String[] args) {
    Node ten = new Node("10");
    Node twenty = new Node("20");
    Node thirty = new Node("30");
    Node fourty = new Node("40");
    Node fifty = new Node("50");

    ten.next = twenty;
    twenty.next = thirty;
    thirty.next = fourty;
    fourty.next = fifty;

    // here is the cycle.
    // we point to first node.
    fifty.next = ten;

    if (isCircle(ten)) {
      System.out.println("test passed.");
    }
  }

  static boolean isCircle(Node first) {

    Node fast = first;
    Node slow = first;

    while(fast!= null && fast.next != null){
      fast = fast.next.next;
      slow = slow.next;

      if( slow == fast )
         return true;
    }
    return false;
  }

  static class Node {
    Node next;
    String data;

    public Node(String data) {
      this.data = data;
    }
  }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# language server protocol (or LSP)

Language server IDE'den bağımsız çalışan bir yazılımdır. ide (client) sunucuya (language server'a) bağlanır ve ona kodları yollar, kodları alan sunucu sürekli olarak client'e rapor verir. her ide için ayrı eklenti yazmaktansa, IDE'ler bu protokolü desteklediği sürece her dile destek verebilirler.

language server ile neredeyse herşey yapılabilir. örnek:  code completion, hover tooltips, jump-to-definition, find-references...

language server ile client'ın haberleştiği protokol LSP'dir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Google Cloud

 - g suite
   - gmail
   - docs
   - keep (personel note)
   - calendar
   - Google search
 - Google Cloud Platform
 - Firebase
   - Firebase Cloud Messaging (or FCM or old-name:Google Cloud Messaging (or GCM) ilk kurulduğundaki ismi: Android Cloud to Device Messaging (or Cloud to Device Messaging or C2DM)
   - Firebase Authentication
   - Firebase Crashlytics
 - compute
   - Compute Engine (remote VM)
   - App Engine (AWS Elastic Beanstalk alternatifi)
 - Google Maps Platform

Firebase: mobile uygulamaya (iOS ve Android) Google-firebase-lib ekleniyor. lib Google firebase servislerine bağlanıyor ve otomatik istatistik yolluyor.

Google cloud alt servislerinin ayrı ayrı, Microsoft bulut ve Amazon bulut hizmetlerinin hangilerine denk geldiği tek tek burada listelenmektedir:

(source-id: 36) "Service comparisons" başlığı.

(source-id: 37) "Service comparisons" başlığı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# VirtualBox içerisinde Resolution ayarlama
Çözünürlük ilk makina kurulduğunda küçük gelir. Arttırabilmek için "guest additions" eklentisi ziyaretçi makinaya kurulmalıdır. guest restart edilir. Virtualbox --> görünüm --> "misafir ekranını otomatik yeniden boyutlandır" seçeneği, anlık olarak guest işletim sisteminin VirtualBox penceresini kaplayacak şekilde genişlemesini sağlamaktadır.

# virt-manager içerisinde Resolution ayarlama
direk olarak masaüstü görüntü ayarları genişletilebilir. ek bir ayara gerek yok.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Virtualbox üzerinde dosya paylaşımı

- ISO file
dosyaları direk olarak mount edebiliyoruz. Bu sebeple bir dosyayı virtual machine'e taşımak istediğimizde ISO dosyası içerisine atmamız yeterli olacaktır. ISO dosyası direk 7zip, Peazip gibi uygulamalarla oluşturulamıyor. Linux ve Microsoft Windows için şu çözümler kullanılabilir:
- Microsoft Windows üzerinde dosyaları ISO dosyasına atabilmek için "Cdrtfe" (portableapps.com formatında da mevcut) uygulaması kullanılabilir. "Cdrtfe" açıldığında "options" butonuna basılır. "ISO image" bölümünden "create image only do not burn" enabled edilir.
- Linux üzerinde şu komut ile ISO yaratılır: mkisofs -o /home/user/folder.iso /home/user/folder

- drag and drop
"guest additions" eklentisi ziyaretçi makinaya kurulmalıdır. guest restart edilir. bu şekilde kopyalanan metinlerde, ziyaretçi-host arasında paylaşılabilir olmaktadır. Bu özelliği enable etmek için, General --> Advance kısmından enable edilmelidir.
Drag and drop ile klasör taşıma işlemlerinde sorun olduğu görüldü. Tek dosya taşımak gerekebilir.
Drag and drop ve kopyalama panosu paylaşımı özellikleri VirtualBox 5'inci sürümü öncesi stabil olmayan eklentilerle sağlanıyordu. Fakat 5'inci sürüm ile birlikte stabil ve eklentisiz desteklenmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# hypertext
gerek aynı dökümanın içerisinde bir yere referans ederek, gerekse farklı bir dökümana işaret eden linkler içeren text dosyalarına/dökümanlarına verilen isimdir.

# hyperlink
"link" ile eşanlamlıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Nexus vs Artifactory
birbirine alternatif Maven, Gradle, npm gibi birçok farklı altyapılar için merkezi dependency/artifact barındırıcısıdır.
Sonatype firması Nexus'u, JFrog firması Artifactory'yi geliştiriyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# remote desktop softwares and protocols

# Remote Desktop Protocol (or RDP)
İsimleri çok genel bu yüzden karışıklıklara sebep olabilir. Microsoft'un uzak masaüstü için kullandığı yazılımın protokolüdür.

Microsoft Windows ile bu uygulamalr aracılığı ile bu protokol kullanılabilmektedir:
- __Windows MSTSC (or Microsoft Terminal Services Client or mstsc.exe)__
- __Microsoft Remote Desktop (or MSTSC)__

# FreeRDP
Microsoft'un RDP protokolünün açık kaynaklı mplementasyon kütüphanesi ve GUI client'ıdır.

# Remmina
Microsoft Windows RDP'yi de destekleyen, Linux için multi-protocol (VNC, SSH...) client.

# rdesktop vs KRDC vs NeutrinoRDP
RDP clients.

# xrdp
open source RDP server for Unix-likes.

# VNC
- Açık kaynaklı uzak masaüstü destekleyen bir protokoldür.
- Protokolü kullanan birden fazla ürün mevcuttur.
- VNC, Teamviewer gibi ekran görüntüsünü paylaşmamaktadır. Sunucu tarafta sanal bir X session'u açmaktadır (bu __XVNC__ oluyor). bu X session'ına client taraftan bağlanmayı sağlamaktadır.

# WinVNC
VNC, x'e göre çalıştığından normal koşullarda Microsoft Windows ortamında çalışamaz. winVNC sadece Microsoft Windows için geliştirilmiş bir VNC sunucu uygulamasıdır.

# X11VNC
standart VNC protokolü sadece uzak masaüstündeki sanal X11'e bağlanabilmesini sağlıyor. fakat X11VNC; zaten açık olan X session'ına bağlanabilmeyi sağlıyor. X11VNC sadece server tarafındaki yazılımdır. herhangi bir VNC client'ta x11VNC'e bağlanabilir.

X11VNC ve VNC'nin yaptığını X zaten destekliyor. fakat VNC ve x11VNC bazı şeyleri disable ederek, network bağlantısında daha az data transferi üzerinden çalışmasını sağlıyor.

X11VNC projesinin geliştirilmesi 0.9.13 sürümünde durdu. daha sonra LibVNC github kullanıcısının, x11VNC reposunda çatallanarak geliştirilmesi devam ettirilmektedir.

LibVNC kullanıcısının altındaki LibVNCServer ve LibVNCClient repoları cross-platform C kütüphaneleridir. Bu kütüphaneler ile VNC client veya VNC uygulaması geliştirilebilir.

# NoVNC
VNC client using HTML5, WebSockets and Canvas.

# Teamviewer
mobile de bağlanılmasını sağlıyor.

birçok Teamviewer uygulaması mevcut:
- Android markette "QuickSupport" uygulaması, mobile'e bağlantı yapılabilmesini sağlayan uygulama.
- "QS Add-on" prefix'i ile başlayanlar QuickSupport uygulamasının yanına kurulan, her firma için ayrı ayrı logo ve hızlı bağlantı çeşitleri içeren uygulamalardır.
- Masaüstü versiyonlarda "QuickSupport" uygulaması kurulumsuz olarak başlayan ve sadece sunucu modunda açılan ayar sunmayan, iş yerleri müşterileri için hazırlanmış bir versiyondur.
- Android markette "Host" uygulaması Android'e servis olarak kuruluyor. böylece uygulama açık değilken bile uzaktan bağlantıya izin veriyor.

# Chrome Remote Desktop
- masaüstünde Chrome eklentisi olarak çalışıyor (sunucu modu dahi).
- mobile versiyon Google Chrome'a eklenti değil. Android uygulaması.
- Linux tarafında sunucu olması için bazı ayarlar yapmak gerekebiliyor.
- Chromium tarayıcısında da çalışıyor.
- mobile'den masaüstü yönetiliyor. fakat mobil yönetilemiyor.
- Google hesabı ile login zorunlu.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# cron
cron kelimesi yunanca'da zaman anlamına gelen xronos'tan gelmektedir.

"cron" UNIX tabanlı sistemlerde istenilen saatlerde çalışması istenen script'leri çalıştıran yazılımın özel ismidir. ismi çok sıkça kullanıldığından; cronjob denildiğinde, herhangi bir platformda arkaplanda scheduled yürütülen job'lar anlamında kullanılır. oysa bu terimi diğer job'lar için kullanmak tamamen yanlıştır.

- cronjob
cron uygulaması birçok farklı script'i farklı zamanlarda çalıştırabilir. her başlatılan script birer cronjob'tır.

- crontab
"cron table" cron uygulamasının configurasyon dosyasıdır. bu dosya tablo gibidir. her satırda zaman ve o zamanda başlatılacak script yazmaktadır.

scheduler görevi gören birçok uygulama yazılmıştır. bazıları aynı veya benzer komut satırı ismiyle çağrılmaktadır.

- expression format

  expression formatı her yazılıma göre değişmektedir. kaynak: (source-id: 38) "Features" başlığı, 11inci madde.

  - Unix (source-id: 39)
  - Cron4j (source-id: 40)
  - Quartz (source-id: 41)
  - Spring (source-id: 42)

  - diğerleri: nnCron

- tarih formatı

(Unix standartına göre anlatılmıştır)

```
* * * * * *
| | | | | |
| | | | | +-- Year              (range: 1900-3000)
| | | | +---- Day of the Week   (range: 1-7, 1 standing for Monday)
| | | +------ Month of the Year (range: 1-12)
| | +-------- Day of the Month  (range: 1-31)
| +---------- Hour              (range: 0-23)
+------------ Minute            (range: 0-59)
```

- özel karakterler

(Unix standartına göre anlatılmıştır)

  - \*

    'her' anlamında kullanılır. örneğin, dakika sütununa yıldız koyarsak, her dakika anlamına gelir.

  - ,

    birden fazla değer koymak istediğimizde kullanılırız.

  - \-

    birden fazla değeri kullanmak için kullanırız. örnek: mon-fri, or 9-17

  - */x

    'her n' anlamına kullanılır. örnek: her beş dakika için, dakika sütununa */5 yazılmalıdır.

- örnekler

(Unix standartına göre anlatılmıştır)

```
* * * * * *                                Each minute

59 23 31 12 5 *                            One minute  before the end of year if the last day of the year is Friday

59 23 31 DEC Fri *                         Same as above (different notation)

45 17 7 6 * *                              Every  year, on June 7th at 17:45

45 17 7 6 * 2001,2002                      Once a   year, on June 7th at 17:45, if the year is 2001 or 2002

0,15,30,45 0,6,12,18 1,15,31 * 1-5 *       At 00:00, 00:15, 00:30, 00:45, 06:00, 06:15, 06:30,
                                           06:45, 12:00, 12:15, 12:30, 12:45, 18:00, 18:15,
                                           18:30, 18:45, on 1st, 15th or  31st of each  month, but not on weekends

*/15 */6 1,15,31 * 1-5 *                   Same as above (different notation)

0 12 * * 1-5 * (0 12 * * Mon-Fri *)        At midday on weekdays

* * * 1,3,5,7,9,11 * *                     Each minute in January,  March,  May, July, September, and November

1,2,3,5,20-25,30-35,59 23 31 12 * *        On the  last day of year, at 23:01, 23:02, 23:03, 23:05,
                                           23:20, 23:21, 23:22, 23:23, 23:24, 23:25, 23:30,
                                           23:31, 23:32, 23:33, 23:34, 23:35, 23:59

0 9 1-7 * 1 *                              First Monday of each month, at 9 a.m.

0 0 1 * * *                                At midnight, on the first day of each month

* 0-11 * * *                               Each minute before midday

* * * 1,2,3 * *                            Each minute in January, February or March

* * * Jan,Feb,Mar * *                      Same as above (different notation)

0 0 * * * *                                Daily at midnight

0 0 * * 3 *                                Each Wednesday at midnight
```

https://crontab.guru/ sayfası expression'a karşılık gelen zaman dilimlerini ekrana basıyor. buradan kontroller yapılabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Omnibox
Çok amaçlı adres çubuğudur. tarayıcılarda adres çubuğu sadece URL'ye gitmek için değil, aynı zamanda arama yapma, matematik işlemleri yapma gibi birçok özellikte barındırır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Sistem Programcılığı
Daha çok işletim sistemi ile yada donanım bilgisi gerektiren yazılım dilleri ve kütüphaneleri kullanarak yazılım geliştirmedir. Bu sebeple; genelde üst seviyeli diller buna gruba dahil olmaz. Bu sebeple; genelde sürücü (driver) yazanlar bu gruba dahildir.

# sistem çağrıları (or çekirdek çağrıları)
İşletim sistemi ile kullanıcı programları arasında tanımlı olan arayüz, işletim sistemi tarafından tanımlanan bir prosedürler kümesidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# P2P (or peer-to-peer or eşlerarası)
"message queue" dünyasında "Point-to-Point Messaging Model (or P2P Model)" in kısaltmasına denk geliyor. Onunla karıştırılmamalıdır.

merkezi otorite olmayan sistemlerdir.

client'ların direk birbiri ile haberleşip transfer yaptığı, sunucunun aracılık yapmadığı protokollerdir. sunucu sadece client'ların birbirini görebilmelerini sağlamakla yükümlüdür. eğer client'lar birbirinin network adreslerini biliyor ise, sunucuya hiç gerek kalmaz. Örneğin BitTorrent'te ".torrent" dosyalarında direk diğer bilgisayarların IP'si olduğundan, sunucuya ihtiyaç kalmaz. Bu sebeple BitTorrent, P2P'ye en iyi implementasyon örneğidir.

BitTorrent'te Client-GUI uygulaması açıldığında hiç sunucuya bağlanmaz. ".torrent" dosyasının içerisinde sunucu IP'si vardır. Client-GUI uygulamasına bu dosyayı tanıttığımızda, sadece buraya bağlanarak dosyayı indirir. İlgili sunucu, indirdiğimiz dosyayı tutan diğer bilgisayarların listesini Client-GUI uygulamasına paylaşır ve Client-GUI uygulamamız tüm diğer sunuculara direk bağlanarak dosyayı parçalı biçimde indirir.

# BitTorrent
BitTorrent protokolün ismidir. aynı zamanda BitTorrent firmasının geliştirdiği, BitTorrent yazılımı bu protokolü kullanan bir client'tır.

# Torrent
paylaşımda yada paylaşıma açık yada hazır olan dosya yada dosyalar grubu için kullanılan terimdir.

# Torrent file (or METAINFO)
".torrent" uzantılıdır.

file that contains metadata about files and folders to be distributed, and usually also a list of the network locations of trackers.

# Availability
Also known as "distributed copies". The number of full copies of a file. Each seed adds 1.0 to this number. eğer 1 kişide tam dosya ve 1 kişide yarım dosya varsa o dosya için Availability 1.5 olur.

# Seed (or tohum)
"seeder", dosyayı paylaşan toplam client sayısıdır.

# Leecher
Leecher kelime anlamı: başkalarının sırtından geçinen. kan emici.

2 anlamı var:
- dosyayı çeken client'tır.
- Lurker gibidir fakat çok az da olsa dosya paylaşır.

# Lurker
dosya indiren fakat hiç paylaşmayan client'tır. sevilmezler.

# Peer (or eş)
Seeder ve Leecher ikilisini ifade eder.

# Tracker
Torrent client, tracker'e bağlanarak, kimde hangi dosyanın hangi parçalarının olduğunu öğrenirler. Tracker üzerinden kesinlikle dosya transferi gerçekleşmez.

# DHT (or Distributed Hash Table)
tracker'dan bağımsız çalışabilmek gerektiğinde, tracker gibi tüm istatistikleri toplamak gerekir. işte bu toplanan bilgilerin tümü DHT tablosunda toplanır.

# share ratio
Torrent'in Upload işleminin Download işlemine oranıdır.

- 1:1 (or 1.0) --> indirilen ve paylaşılan dosya boyutu eşit.
- 2:1 (or 2.0) --> paylaşılan dosya sayısı indirilenden 2 katı fazla.
- 0.5:1 (or 0.5) --> indirilen dosya sayısı paylaşılandan 2 katı fazla.

# Hit&run
Dosyayı çekip hemen paylaşımdan kaldırma işlemine ve kaldırana verilen isimdir. Torrent dünyasında kesinlikle sevilmezler.

# Choked
kelime anlamı: tıkanmış

Dosyayı paylaşan, dosyayı paylaşmayı o anlık red ediyor ise; dosyayı paylaşan "chocked" olmuş olur.

# swarm
kelime anlamı: sürü

bir Torrent'i paylaşan ve indiren tüm node'lara verilen isimdir.

# magnet URI Scheme
"torrent" dosyası yerine alternatif bir çözümdür.

".torrent" dosyaları tracker bilgilerini barındırır. client gidip sunucudan dosyaları paylaşan node'ların IP'leri gibi birçok bilgiyi çekmek zorundadır. Yani ilk aşamada tracker'lar bağlantının kurulması için aracılık yapar. Oysa magnet linkleri (sadece bi URI'dir, dosya değildir) direk indireceğimiz kaynağın URI'sini barındırır. Bu şekilde merkezi tracker'lara gerek kalmaz. Genelde tracker'lar devlet otoriteleri tarafından kapatıldığıiçin böyle bir çözüme gidilmiş.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# 3rd party software
bir yazılım satıldığında veya ücretsiz kullanıldığında, yazılımı üreten ve kullanan arasında bir söleşme oluyor. yazılımcı ve kullanıcı first ve second party oluyor. fakat first party veya second party terimlerinin kimin olduğu belli değildir.

3rd party'de burada bu iki grup dışında kalan kişi/gruplar için kullanılan bir terim oluyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# otonom (or autonomous)
kendi kuraları/işleyişi olan. özerk sistem.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# awesome

- bir rozet (or badge)'tir.

- "The awesome manifesto" altında tanımlaması yazılmıştır.

- en iyi kitaplar listesi, Android open source uygulamalar listesi, C# için yazılmış İngilizce kitaplar listesi gibi koleksiyon sayfalarına bu etiketi her isteyen koyabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Microsoft Windows kitaplıkları

Microsoft Windows'un yeni sürümleri ile birlikte dosya yöneticisinde kitaplık özelliği geldi. bir kitaplık birden fazla dizini aynı klasörmüş gibi gösterebiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Microsoft Windows güncellemeleri

Microsoft Windows XP'den itibaren güncellemeleri toplu şekilde gruplandırarak "service pack" isminde ayrı setup'lar da sunuyordu. tek tek güncelleme kurmak yerine bu tercih edilebilirdi.

Microsoft Windows 10 ile birlikte isimlendirme service pack olarak adlandırılmıyor. onun yerine piyasaya özel isimlerle sunuluyor. sırası ile 2018'e kadar aşağıdaki paketler sunuldu:

- November Update
- Anniversary Update
- Creators Update
- Fall Creators Update
- April 2018 Update
- October 2018 Update

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Orthogonality
Kodda bir değişiklik yapıldığı zaman, farklı yerlerde değişikliğe gerek kalmaması durumudur.

En basit örnek: aynı hard-coded-string birden fazla kere tanımlanmış ise, sadece o string'i güncellememiz yetmeyecektir. Bu durum yazılımımızın orthogonality'nin olmadığını gösterir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# debounce
bounce kelime anlamı: sıçramak

debounce kelime anlamı: sıçramayı engellemek

bir butona bastığımızda (or bir event olduğunda) genelde bir metot tetiklenir.  eğer bu event üstüste sürekli gerçekleşirse her defasında bu event'i yapmak yerine 3 saniyede bir yapmayı tercih ederiz. işte bu yapacağımız yöntem ile sıçramayı engellemiş oluruz.

örnek:

```js
var debouncedMethod = API.debounce(myMethod, 3);
```

artık debouncedMethod'i üstüste çağırırsak max 3 saniyede bir execute edilecektir. yani üstüste çağırmalarımız ignore edilecek.

Yukarıda API bir kütüphaneyi temsilen yazıldı. örneğin lodash'te debounce metotu vardır. her programlama dili için debounce sağlanabilir.

debounce kullanım alanına örnekler:

- web sayfasındaki form için post butonu event'i

- GUI'mizin resize olduğundaki event. her saniye tekrar render yerine, 2 saniyede 1 render etmek CPU'yu daha az yoracaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Automatic content recognition (or ACR)

media yada belge dosyalarının otomatik tanınması teknolojisidir.

# acoustic fingerprinting

ses dosyalarının tanınması için tasarlanan teknolojilerdir. ACR'nin bir alt kümesidir.

# video fingerprinting

video dosyalarının tanınması için tasarlanan teknolojilerdir. ACR'nin bir alt kümesidir.

# Watermark (or tr:filigran or tr:suyolu)
fiziksel olan: örneğin paralarda sadece ışığa tutulduğunda görünen bir şekil vardır bu şekilde o paraya ID atanabilir. fakat normal kullanımda kullanıcı bunu görmez/bilmez.

benzer şekilde digital dünyada da insan kulağının duymayacağı şekilde her ses dosyasına bir ses yerleştirilir. bu sesi ancak programatik olarak tespit edebiliriz. bu şekilde o ses dosyasını unique yapmış oluruz.

KuroLabs/Stegcloak projesi buna örnek verilebilir (başka başlıkta anlatılıyor).

# AcoustID

açık kaynaklı acoustic fingerprinting teknolojisidir.

# Chromaprint

AcoustID için client side C kütüphanesidir. AcoustID fingerprint'i üretir. AcoustID, projenin ismi olarak kullanılmaktadır.

# acoustic fingerprinting temel çalışma prensibi

her acoustic fingerprinting farklı teknikler kullanmaktadır. fakat temel olarak işleyişi birçoğunda aynıdır.

bir ses dosyasının acoustic fingerprint'i uzunca bir string'dir. MD5 ve SHA'nın tersine, ses dosyasındaki ufak değişiklikler acoustic fingerprint çıktısında ufak değişiklikler yaratmaktadır. tabi bu da tölere edilebilir bir durumdur. örneğin bir web servisine Fingerprint attığımızda en yakın fingerprint'ler benzerlik oranları ile dönmektedir:

```
{

    results =     (

                {

            id = "3fb2e35c-c4d5-4360-aaff-8dad0aad05e9";

            score = "0.976727";

        },

                {

            id = "b262c597-64b3-42c4-94b0-e89b8c496d76";

            score = "0.9310389999999999";

        },

                {

            id = "49f64d53-de52-4f4b-a08d-8d78ca0001cf";

            score = "0.690862";

        }

    )

}
```

Fingerprint'in %100 benzeşmesi zaten istenmeyen bir durumdur.

Fingerprint hesaplamaları ses çıktısı üzerinden yapılır, ses dosyası formatı ile bağlantılı bir durum değildir.

Örneğin bir ses dosyası olsun. Her 2 saniyesinin sonundaki ses anından bir bilgi üretilsin. bu bilgi 2 sn aralıklarla tutulmuş oluyor. AYnı şarkının farklı bir ses kaydında yada bir konserde, orijinal şarkı kaydı ile aynı tempoda gidilmeyeceği için kontrol edilen şarkı 1'er saniye delay yapılarak tekrar kontrol edililiyor.

Müzik veritabanları incelendiğinde (public ve ticari olanlarda) veritabanında aynı şarkının birden fazla fingerprint'i olduğu görülür. bunun sebebi şudur: orijinal kayıt sanatçının kayıt/media/prodüksiyon şirketi tarafından kaydedilir. 1 fingerprint o zaman oluşturulur. bu şirket hesabı onaylanmamış hesap olabilir. bu gibi bir durumda farklı bir ses kaydından fingerprint oluşturulup o şarkıya yollayan biri olabilir. yada konser versiyonunu fingerprint olarak o şarkıya atayan olabilir. bazı insanlar ise; bir şarkının konser yada remix versiyonunu  şarkı olarak kaydetmektedir. Bazı şarkılar  albümlerin içinde de gelmektedir, yada bir film müziği DVD'si içerisinde soundtrack hediye olarak gelmektedir. onları da  şarkı olarak kaydeden olmaktadır.

Eski klasik müziklerde iş karmaşıklaşmaktadır. Bir semfoniyi komple bir şarkı olarak kaydedenler mevcuttur. Semfoninin ilk 3 ile 5inci bölümünü bir şarkı olarak kaydedenler vardır. bir semfoninin 4üncü bölümünü içeren bir ses dosyasını Chromaprint'e verdiğimizde bize tüm olasılıkları döndürecektir.

# Gracenote

müzik veritabanını dışarıya ücretli API ile sunan bir cloud web servis firması.

# MusicBrainz

açık kaynaklı müzik veritabanı.

# Beets

açık kaynaklı müzik arşivi düzenleme programıdır.

# Picard

açık kaynaklı GUI kullanan AcoustID ile MusicBrainz foundution'dan local müzik dosyalarının tag'leyen uygulama.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Android Studio ve IntelliJ Idea için "build" vs "Make" vs "rebuild"

aşağıdaki işlemler project veya module için sunuluyor. project explorer'da ne seçili ise, aşağıdaki seçenekler seçili olan seviye (proje yada modül) için çalıştırılıyor.

- "Android studio: Make" "Intellij idea: build" ikisinin sadece menüdeki isimleri farklı. arkada aynı görevi işletiyorlar.

  compile + depend olan kodları/modülleri de compile eder. fakat sadece değişik yapılan sınıfları build eder. bu sebeple path, paket ismi, lib gibi değişikliklerde "rebuild" yapılmalıdır. "make" isminden de anlaşıldığı gibi executable yaratmak için kullanır. zaten bu sebeple depend olan projeleri de birlikte derler.

- clean

  her koşulda target dizinlerini siler.

- Rebuild

  her koşulda herşeyi tekrardan build eder. öncesinde clean'i çağırır. artifact oluşturmaz.

- compile

  sadece bir class yada paket(class grubu) seçili iken menüde görülen bir seçenektir. sadece ilgili(seçili) paketlerin/sınıfların içindeki kodları compile eder.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Semantic Versioning (or Semver)
semantic kelime anlamı: anlamsal

yazılım sürüm atlama kurallarıdır.

MAJOR.MINOR.PATCH şeklinde gider.

- Major: public API'lerimizde geri uyumlu olmayan geliştirmeler yapıldığında bu sürüm atlanır.

- Minor: add functionality in a backwards compatible manner

- Patch (or tr:yama): backwards compatible bug fixes

Sürümlemedeki bazı kurallar:

- Sürüm çıkıldıysa kesinlikle aynı sürümde güncelleme çıkılmamalıdır.

- Bazı firmalar yukarıdaki kurallara uymasa da bu kurallarla resmi makale mevcuttur.

- Yukarıda bahsedilen public API bir çok katman olabilir: örneğin komut satırından kullanırken ki komut parametreleri, web tarayıcısı için eklentilere sunduğu API, veya her ikisi birden olabilir.

- Sayısal artış matematiktekinin tersinedir. yani; 1.9.0 < 1.10.0 'dır.

- 0.y.z sürümünde public API ile sunulan şeyler stabil değildir. 1.0.0 ile artık production ortamına geçilmelidir ve public API kullanılabilir bir stabilite de olmalıdır.

- isteğe bağlı "pre-release" şu şekilde yapılabilir:
  - 1.0.0-alpha
  - 1.0.0-alpha.1
  - 1.0.0-0.3.7
  - 1.0.0-x.7.z.92
  - 1.0.0-x-y-z.--

  bu suffix'lerde sadece 0-9a-Za ve tire ve nokta işareti olabilir.

- isteğe bağlı "build metadata" şu şekilde yapılabilir:

  - 1.0.0-alpha+001
  - 1.0.0+20130313144700
  - 1.0.0-beta+exp.sha.5114f85
  - 1.0.0+21AF26D3----117B344092BD

  Yukarıda + işareti sonrası build bilgisini temsil eder.

  bu suffix'lerde sadece 0-9a-Za ve tire ve nokta işareti olabilir.

- tüm sürüm bilgisinde (build ve pre-release dahil) başlangıçta (veya noktadan sonra) birden fazla sıfır yanyana olmamalıdır.

Örnek:

Sürüm numarası küçükten büyüğe doğru:

- 1.0.0-alpha
- 1.0.0-alpha.1
- 1.0.0-alpha.beta
- 1.0.0-beta
- 1.0.0-beta.2
- 1.0.0-beta.11
- 1.0.0-rc.1
- 1.0.0

# Romantic Versioning (or RomVer)

HUMAN.MAJOR.MINOR şeklinde gider.

SemVer daha çok son kullanıcısı teknik olan ve teknik işlerde kullanılan yazılımlarda kullanılmaktadır. Oysa RomVer daha son yazılım geliştirici olmayanlar için tasarlanmış yazılımlarda kullanılmaktadır.

RomVer'de Public API'nin yerini GUI almaktadır.

- Human: son kullanıcının aksiyon almasını (config değiştirmesi gibi), veya GUI deki yer değişiklikleri gibi durumlarda artar. Veya önemli değişikliklerde veya önemli dökümantasyon değişikliklerinde bu sürüm artar.

- Major: incompatible API changes

- Minor: geriye uyumlu geliştirmeler veya son kullanıcının bir şey yapması beklenmediği bugfix'ler.

# Calendar versioning (or CalVer)

Kullanılırken birçok şeması olabilir. her proje kendi şemasını tanımlamalıdır.

Şema tanımlarken şu tanımlardan yararlanacağız:


- Major - The major segment is the most common calendar-based component.
- Minor - The second number in the version.
- Micro - The third and usually final number in the version. Sometimes referred to as the "patch" segment.
- Modifier - An optional text tag, such as "dev", "alpha", "beta", "rc1", and so on.
- YYYY - Full year - 2006, 2016, 2106
- YY - Short year - 6, 16, 106
- 0Y - Zero-padded year - 06, 16, 106
- MM - Short month - 1, 2 ... 11, 12
- 0M - Zero-padded month - 01, 02 ... 11, 12
- WW - Short week (since start of year) - 1, 2, 33, 52
- 0W - Zero-padded week - 01, 02, 33, 52
- DD - Short day - 1, 2 ... 30, 31
- 0D - Zero-padded day - 01, 02 ... 30, 31

Piyasada CalVer kullanan projelerin belirlediği şemalar:

- boltons - YY.MINOR.MICRO
- certifi - YYYY.MM.DD
- fusefs-ntfs - YYYY.MM.DD_MICRO
- LibreOffice - YY.MM
- OpenSCAD - YYYY.0M
- pip - YY.MINOR.MICRO
- PyCharm - YYYY.MINOR.MICRO
- Stripe's API- YYYY-MM-DD
- Unity - YYYY.MINOR.MICRO

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Atom
Electron tabanlı text editor. Github'ın microsoft tarafından satın alınması ile birlikte geliştirilmesi çok yavaşladı. Çünkü Microsoft VSCode'u geliştiriyor.

# atom IDE
Atom'a ekstra IDE prefix/suffix'li eklentiler kurularak atom IDE halini alıyor.

# Nuclide IDE
Atom tabanlı fakat paketlerin kurulu geldiği ve silinemediği bir IDE. React-native projeleri için geliştirilmiştir.

# Electron
NodeJS ve Chromium altyapısı ile cross platform uygulama geliştirmeye yarıyor. "Atom text editor", "Visual Studio Code (or VSCode)" bu altyapıyı kullanıyor. İlk olarak "Atom text editor" için geliştirilmiş, daha sonra herkes kullanmaya başlamıştır. bu sebeple eski ismi: "Atom Shell"dir.

# VSCodium
- VSCode'un içinde gelen kapalı kaynak paketlerin kaldırılmış fork'udur.
- VSCodium hem desktop hemde browser'dan çalışacak (cloud base) olarak çalışabilmektedir.

# Theia
- Eclipse'in geliştirdiği, VSCode eklenti API'si ile aynı olan text editor'dür.
- Electron tabanlıdır.
- VSCodium hem desktop hemde browser'dan çalışacak (cloud base) olarak çalışabilmektedir.
- Theia 'open-vsx.org'u marketplace olarak kullanmaktadır. "open-vsx.org" Eclipse vakfı tarafından geliştirilmektedir. "VSX" VSCode'un eklenti uzantısıdır. VSCode mağazası lisans sorunları ile başkaları tarafından kullanılamadığı için, böyle bağımsız bir uygulama marketine ihtiyaç duyulmuştur. VSCodium'da 'open-vsx.org'u kullanmaktadır.

# Eclipse Che
- Kubernetes ortamında kurulan bir çalışma ortamıdır. Her user'a aynı çalışma ortamını klonlar ve browser'dan IDE olarak o çalışma ortamına bağlanılmasını sağlar.
- Default olarak "Che-Theia" isimli componenet ile önyüze Theia IDE'yi yansıtmaktadır. Tabi bu tahmin edildiği gibi Theia altyapısını kullanmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# GitLab

Continuous integration, bug, repository hizmetlerinin sunucusu yönetimi sunan bir web yazılımıdır. Eklenti altyapısı da olduğundan her türlü amaçla kullanılabilmektedir.

- ## ana menüler

- İlk açıldığında "Project" ve "Groups" sekmeleri karşımıza çıkar. "groups" projelerin gruplandırılmış halini içerir. projects ise direk projeleri (gruplamadan) gösterir.

  her repository (yani proje), repository-URL'sinin önüne grup ve sub-group adını otomatik prefix olarak gelmektedir.

- "Activity" sekmesinde tüm projelerdeki her commit'i bir arada listeleyebiliyoruz.

- "Milestones" sekmesindeki listenin her elemanı bir task grubunu kapsaması için yaratılmıştır. örnek: OnlineÖdemeler ismini verdiğimiz milestonumuz olsun. bunun altına yine GitLab'den açacağımız task'ları bağlarız. Bu şekilde task'ların yüzde kaçının bittiğini, her task ile bağlantılı olan kaç commit atıldığını vs istatistiksel olarak takip edebiliriz. Milestone, Jira'daki Spring kavramına çok benzemektedir.

- ## alt menüler
Her projenin altında o projeye ait wiki, issues, CI/CD (Continuous development), Settings, Merge Request, repository sekmeleri bulunur. bu başlıkta sadece "issues" in altındakileri inceleyeceğiz:

issues sekmesinin altında;

- list: tüm issue'lar listelenir

- board: issue'ler grup halinde listelenir: kapanmışlar, backlog'dakiler vs..

- milestones: (yukarıda anlatılıyor)

- labels: task'lara/issue'lara atanmak için yaratılırlar. issue'lara keyword atamak için kullanılır. eksta bir özelliği yok.

# roller

sadece bunlar var: Guest , Reporter, Developer, Master, Owner

her rol için detaylı bilgi burada yazıyor: https://docs.gitlab.com/ee/user/permissions.html

temel olarak;

- Guest: yeni issue açabiliyor + kod hariç herşeyi sadece takip edebiliyor (wiki, diğer issue'lar, artifacts, job'lar)

- Reporter: guest + issue assign + label management + merge request assign gibi daha yönetimsel işleri yapabiliyor + ek olarak proje kodunu da görebiliyor.

- Developer: reporter + milestone yönetimi + kod gönderme isteği + wiki yazma + CI/DI için sadece stop-start gibi yetkilere sahip

- Master: Developer + user management + CI/DI yazma

- Owner: Master + remove everything

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# bootstrap kelime anlamı

sözlük anlamı: çizme (ayakkabı) atkısı (üst kısmıdır)

bilişim dünyasında farklı anlamda kullanılmaktadır. direk örnekler üzerinden gidersek;

- web sitemizde bootstrapper index.html dosyamızdır.

- bootstrap loader bir sistemi başlatan yazılım yada yazılımın ilk kodlarıdır. bootstrap loader; web sitemizi başlatacak olan framework/sunucu yazılımımızdır.

Bootstrapping; terimi ise yukarıdaki 2 maddeden bağımsızdır. Bootstrapping; bir sistemi kendi sisteminizle geliştirmenizdir. örneğin; C programlama dilinin yeni versiyonları yine C dili ile geliştirilmektedir. bu geliştirme sürecine bootstraping denir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ITIL (or Information Technology Infrastructure Library)

İngiltere Ticaret Bakanlığı tarafından geliştirilmiştir. ITIL, kitaplar şeklinde, bilişim altyapısı için yaklaşımlar anlatılmaktadır. Bu yaklaşımlar dünyanın birçok firması tarafından özellikle uyulmakta ve takip edilmektedir.

ITIL katı kuralları olan bir framework diildir. Öneriler ve açıklamalar içermektedir.

versiyonları vardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# declaration (or beyan or bildiri) vs definition (or tanımlama)

fonksiyon/variable imzalarının kodlandığı yerde declaration yapılmış olur. oysa bunlara değer atama, metotu implemente etme (body kısmını yazma) gibi kodlarının yapıldığı yerde definition yapılmış olur.

örnek:

> int PI; //declaration

> PI = 3; //definition

```java
int function topla(int a, int b); //declaration

topla = function(int a, int b){

      return a+b;
}
```

declaration ve definition aynı kod bloğunda da olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# truncate output

truncate Türkçe kelime anlamı: kesmek, ayırmak.

bir komut satırı çıktısının sadece bir kısmını göstermek anlamında kullanılır. örneğin pi sayısını (3.123456789...) truncate edip göstereceksek 3.124 gibi yuvarlayamayız. onun yerine sadece bir kısmını göstermemiz gerekir: 3.123.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# accessibility (or erişilebilirlik) destekleyen yazılımlar

kullanıcıların birçok çeşit engeli olabilir:

- renk körlüğü (or Color blindness or color vision deficiency or gr:Αχρωματοψία)

  çeşitleri:
  - renkleri farklı görme: mavi yerine pembe gibi
  - sadece siyah beyaz görme
  - renkleri farklı yada doğru gören insanların bazıları renklerin farklı tonlarını algılayamamaktadırlar

  birçok insan renk körü olduğunu bile bilmiyor. bu sebeple renklere bağlı seçim yaptırırken dikkat edilmelidir.

- hiç görememe

- duyamayanlar

- fareyi kullanamama

tüm engelliler klavyeyi kullanabiliyor.

dikkat edilmesi gereken bazı noktalar:

- HTML'de medya dosyalarına (video, resim, ses gibi) alt="media açıklaması" atanmalıdır. göremeyenlere resimler gösterilmediğinde bu metin ekrana basılıyor, veya göremeyenler için bu metin okunuyor. bu metinlerde multi-language desteği olmalıdır.

- yazılımdaki tüm metinlerde kısaltmalar az ve hatta mümkünse hiç olmamalı. örnek 11 Feb 2017. Feb yerine February yazmak lazım. metin okuma programları kısaltmaları tanımlayamayabilir.

- standartlara uymak gerekli. örneğin HTML'de h1, h2, p gibi tag'ler çok önemli. sayfanın yapısı tarayıcı tarafından algılanabilmeli.

- metinler ve resimler net olmalı. ve ufak boyutta olmamalı. hiçbir obje ufak olmamalı. örneğin checkbox bile ufak olmamalı. karmaşık fontlar kullanılmamalı.

- linklerde title'ı mutlaka kullanmalıyız. örnek:

```html
<p>İş yerlerine başvuru için formu doldurmanız gerekli. <a href="form.html" title="İş yerine başvuru için tıklayın.">İş yerine başvuru için tıklayın.</a>  </p>
```

- form'da her eleman için "label" olmalı. form alanı doldururken buradaki bilgi ilişkili olduğu için engelli kişi için tanımlayıcı oluyor. örnek:

```html
<form>
    <label for="yourName">Your Name</label>
    <input name="yourName" id="yourName">
    <!-- etc. -->
```

- form elemanlarını mümkünse gruplamalıyız:

```html
<form action="somescript.php" >
    <fieldset>
        <legend>Name</legend>
        <p>First name <input name="firstName"></p>
        <p>Last name <input name="lastName"></p>
    </fieldset>
    <fieldset>
        <legend>Address</legend>
        <p>Address <textarea name="address"></textarea></p>
        <p>Postal code <input name="postcode"></p>
    </fieldset>
    <!-- etc. -->
```

- penceredeki elemanlara "Access Keys" atamaları yapılmalıdır.

- tema ve renkler olabildiğince flat olmalı. bu şekilde resimlerin tonlarını algılamayanlar için sorunlar ortadan kalkacaktır.

- sadece ses efekti ile uyarı yapıldığında, paralel olarak bildirim de yapılmalıdır.

- tarayıcılarda erişilebilirlik açıkken farklı CSS'ler kullanılabilir. bu şekilde çözüm üretilemeyen durumlarda, gerekirse bambaşka bir sayfa gösterilebilir.

- bazen içeriklere hızlı erişim için ayrı ayrı linkler konulmalıdır. örnek:

```html
<header>
    <h1>The Heading</h1>
    <a href="#content">Skip to content</a>
</header>

<nav>
    <!--loads of navigation stuff -->
</nav>

<section id="content">
    <!--lovely content -->
</section>
```

bunlar'ı CSS ile erişilebilirlik kapalı ise göstermemeliyiz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# makefile

Linux'ta genelde program derlemek için önce "configure" komutu çağrılır. configure komutu o proje dizinindeki bir script'in ismidir. root klasöründe olan bu script dosyasıdır. zaten bu sebeple ./configure diye çağrılır. bu dosyanın görevi projenin derlenebilmesi için ön-hazırlık ve kontrollerin yapılmasıdır. örnek: işletim sisteminde projeyi derlemek için gerekli program var mı tespiti yapılmaktadır. örnek: javac (Java compiler) var mı gibi... Aslında burada "makefile" içerisindeki tüm komutların çalışabilmesi için gerekli tool'ların yüklü olup olmadığı kontrol edilir.

daha sonra "make" komutu çalıştırılır. make komutu bir program. örneğin "gnu make" açık kaynaklı bir program. işletim sisteminde kurulu olması gerekli. birçok make programı mevcut. make programı varsayılan olarak projenin root'undaki "makefile" dosyasını okur ve bu dosya içindeki tanımlara göre projeyi derler. makefile dosyası programlama dilinden bağımsızdır.

# example makefile

```makefile
variable1 = value1

# FORMAT OF MAKEFILE:

# target: prerequisite1(dependency1) prerequisite2(dependency2)
#   shell_command1(recipe1)
#   shell_command2(recipe2)

# ".o" is the extension of the builded C file.

targetFile1.o:
    echo "targetFile1 called"
    echo $(variable1)
    echo $variable1 # Bad practice, but works.

# fileName2 is the "target".
# "targetFile1.o" is the "prerequisite1(dependency1)".
targetFile2.o: targetFile1.o 
    echo "hello"

myTarget: targetFile1.o # this target/line does not run any command directly. it just run the dependency which is "targetFile1.o". This line works like an "alias".

target3.o: target3.c # if "target3.o" file is never then "target3.c" file, make-command will not run this command if the user called this command directly or un-directly (via dependency).
  echo "hello"
```

"make" process does not store any meta-data about timestamps, it directly reads the dates from OS filesystem.

# run spesific target

command to run only a spesific target:

```makefile
make "fileName2.o"
```

# default target

command to run the default target:

```makefile
make # this command will run the default target.

# default target is the first target which does not start with a dot (.) character.
```

# target dosya olmak zorunda değil
target eğer bir dosyaya tekabül ediyorsa, o dosyanın güncellenme tarihi önem kazanır. bu durumun örneği yukarıda örneklendirildi.

fakat eğer target'ın bir dosyaya tekabül etsin veya etmesin, eğer çağrılmış ise her türlü koşturulmasını istiyorsak, şu şekilde tanımlama yapmalıyız:

```sh
.PHONY: myTarget # PHONY is a special keyword

otherTarget1:
  echo hello

myTarget:
  echo "hello2"
```

# install
bu eğer target dosya ismine tekabül etmiyorsa, o zaman sürekli çağrılır. Bu sebeple "install" target'ı olursa (ve install dosyamız yok ise) bu artık bir script gibi çalıştırılacaktır. install genelde OS'a kurulum yapılması amaçlı yaratılan bir target'tır.

# shell script vs makefile
ikisi ile de proje yönetimi yapabiliriz. Makefile'ın en önemli artısı: dependency yönetimi yapması. yani bir fonksiyon, dependency'de belirtilen fonksiyonları (sadece gerektiğinde) otomatik çağırıyor. bu kolaylık shell'de default olarak mevcut değil. zaten shell'in amacı bu olmadığından böyle bir özelliğin varlığı beklenemez.

# GNU Autotools
__GNU Build System__ terimi; OS'a göre build alabilmemizi sağlayan script'leri temsil etmek için ullanılan bir terimdir.

__GNU Autotools__ ise "__GNU Build System__" ortamını hazırlamamız için gerekli olan yazılımları kümesine/ailesine verilen özel isimdir. Bu aile şu yazılımları içerir:__Autoconf__, __Automake__ ve birkaç tane daha...

__Autoconf__ komut satırı uygulaması, projede bulunan "__configure.ac__" dosyasından yararlanarak, "__./configure__" script'imizi oluşturuyor. Çünkü configure script'imiz her OS'tan OS'a değişebilir. Bu sebeple projeyi kendine çeken herkes bu işlemi yapması daha sağlıklı olacaktır.

"__automake__" komutu, "__makefile.am__" dosyasından yararlanarak "__makefile.in__" dosyalarını oluşturuyor.

"__./configure__" programını çalıştırdığımızda, __makefile__'ı oluşturabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# handler vs listener

aralarında çok ince bir fark var. listener kaynağı dinler ve kaynak aksiyon aldığında handler'ı tetikler. handler ise event tetiklenince ne yapılacağına karar verir. şöyle düşünebiliriz:

```java
button.setOnClickListener( OnClickListener {

    onClick() {

        ...

    }

});
```

yukarıdaki kodda; OnClickListener bir listener, onclick metotu ise bir handler'dır. farklı bir örnek:

```java
button.addEventListener('click', function() {

   ...

});
```

yukarıdaki addEventListener metotu aslında bizim için kendi içinde listener oluşturuyor. bizden sadece handler'ı parametre istiyor (anonim metotumuz - 2inci parametre).

Aslında; listener'da birşey tarafından tetikleniyor. bir bakıma listener'da bir handler olmuş oluyor. bu sebeple olaya nereden baktığımız önem kazanıyor. örneğin; bir Java programında, programcı için listener; JVM'in işletim sistemi ile arasındaki kod/program parçacığıdır. handler ise; programcının parametre yolladığı sınıftır. şimdi ise JVM açısından olaya bakalım: JVM için listener; işletim sisteminin event mekanizmasıdır, oysa handler JVM'in işletim sistemine event gerçekleştiğinde çalıştırması için gönderdiği kod/program parçacığıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# synchronization (or senkronizasyon or eşleme)

eşgüdümleme kelimesi eş anlamlı değildir. eşgüdümleme koordine etme, işbirliği içinde hareket ettirme anlamına gelmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Headless software

örnek: "headless java", "headless Linux"

bu terim o yazılımın GUI'siz çalıştığını belirtir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Wordpress vs Joomla vs Brutal
Açık kaynaklı PHP tabanlı content management sistemleridir. başlıktaki sırası ile basitten daha gelişmiş'e doğru gider. Daha basit kullanımı olan Wordpress neredeyse hiçbir teknik bilgi gerektirmeden kullanılabiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# dosya sistemi (or file system)

dosyaların bellekte tutulma formatıdır. örnek formatlar:

- NTFS (or NT File system)

- FAT12 (or File Allocation Table, 12-bit cluster indices)

- FAT16 (or File Allocation Table, 16-bit cluster indices)

- FAT32 (or File Allocation Table, 32-bit cluster indices)

  FAT16'ya göre en önemli yenilik dosya maximum bellek alan desteğinin genişletilmiş olmasıdır.

- ReiserFS (en başarılı özelliği unmount etmeden partition'un resize edilebilmesidir)

- btrfs

  en başarılı özellikleri:
  - logical volume: 1 partition altında sanal bölümlendirme yapıyor. ayrı ayrı mount edilebilen fakat ortak boş alanı kullanabiliyorlar. Bu açıdan bakıldığında LVM'e alternatif gibi görünsede, LVM parittion management formatı iken, BTRFS bir dosya sistemidir.
  - copy-on-write (başka başlıkta anlatılıyor)
  - spapshot: anında bir sanal bölümlendirmenin kopyasını alabilmemizi sağlıyor. 

- ext# (ext4, ext3...)

- XFS

- ExFAT (or Extensible File Allocation Table)

  Bazı yerlerde FAT64 olarak isimlendirilir fakat resmi ismi bu değildir.

  FAT32'nin fork'udur. FAT32 çok basit bir dosya sistemidir. Çok az özellik içerir. Onun fork'u yapıldı ve sadece minimum dosya boyutu desteği arttırıldı. Bu Microsoft Windows 7 ve sonrasında Microsoft tarafından default olarak support ediliyor. Linux ve MacOS içinde paketler çıkarılmıştır. Cross-OS basit bir dosya-sistemi çözümü olarak sunuldu.

  exFAT external drive'lar için önerilir.

- Resilient File System (or ReFS)

  projenin kod adı: Protogon

  Microsoft'un NTFS'e alternatif yeni dosya sistemidir. Microsoft Windows 8.1 ve sonrasında default support edilmektedir.

# ISO9660
CD-ROM dosya sistemi.

# journaling file system (günlükleme dosya sistemi or JFS)
değişiklik yapılacak dosyaların üstünde değişiklik yapar. ama her değişikliği log (journal) olarak tutar. bu şekilde işlem sırasında hata alınırsa log'lara bakarak işlemi geri alabilir.

write-ahead logging mantığında çalışır.

# Journaled File System
IBM'in geliştirdiği JSF tabanlı dosya sistemi. özel isimdir. teknolojisi ile aynı ismi barındırıyor.

# Copy-on-write (or CoW or implicit sharing or shadowing)
journaling'e alternatiftir.

copy-on-write, tüm transaction boyunca, değiştireceği dosyalarla ilgili alanları hiç kullanmaz. tamamen bağımsız şekilde işlemi yapar ve transaction tamamlandığında, eski olan dosyanın referanslarını geçersiz olarak configüre eder.

# journaling vs copy-on-write
- CoW varolan dosyanın bloklarını değiştirmiyor, sürekli yeni bloklara farklı yerlerden referans veriyor. bu sebeple journaling'e göre fragmantation ihtiyacı daha fazladır.
- Journaling, copy-on-wirte'a göre hata durumlarında daha başarısızdır. journaling herşeyi log'lara bakarak geri alabiliyor. fakat file-system'in kendi yazılımsal hatasında geri alamayabilir veya almış gibi davranır fakat alamamıştır.

# disk birleştirme (or disk defragment)

bir dosyanın fiziksel olarak yazıldığı noktalar bütünleşik olmayabiliyor. bu sebeple dosyalar arası boşluklar meydana geliyor ve tek dosya birçok parçalanma yaşayabiliyor. bu parçalanmalar dosya okuma yazma hızlarını da düşürüyor.  sebeple disk defragment yapan yazılımlar geliştirilmiştir.

ext4 gibi gelişmiş bazı dosya sistemlerinde okuma yazma yaparken akıllı algoritmalar sayesinde dosya bölünmeleri en aza indirgeniyor ve yeni dosyalarla boşluklar kapatılıyor. bu sebeple bazı dosya sistemlerinde disk birleştirme neredeyse hiç gerekmiyor.

# dosyaların silinmemesi

dosyalar dosya sisteminden silindiğinde, sadece dosya adresi silinir. bu sebeple hala dosyanın geri getirilme şansı vardır. bu sebeple bazı güvenlik yazılımları, silinene dosyaların üzerinden geçmek için, boş alanın tümünün üstüne yazar. boş alanı dolu olan bir bellekten dosyayı yazılımsal tekniklerle geri getirilme şansı yoktur. artık donanımsal/fiziksel çözümlerle eski dosyalar geri getirilebilir.

# inode

UNIX tarzı dosya sistemlerinde her dosya ve klasörün metadata'ları bir tabloda (INode table) tutulur. her inode bu tablonun her satırıdır. her inode (her satır) bir dosya yada klasörün meta data'larını tutar.

# HDD vs solid-state drive (or SSD or solid-state disk)

HDD mekanik SSD ise tamamen elektronik yapıdadır.

# HDD'nin fiziksel yapısı

- plak (or platter): HDD üstüste takılmış yuvarlak parçalardan meydana gelir. genelde bir HDD kendi içinde birkaç plak bulundurur.

- track: her plak içinde dairesel kayıt kısmına denir.

- sektör (or sector): her track kendi içinde ufak kısımlara bölünmüştür. en ufak birim sektördür ve genelde 512 byte'tır.

- Cluster (or Block): bir HDD'de sektör sayısı çok fazla olduğundan dosya sistemleri kendi içlerinde ardışık olan sektörleri gruplandırırlar. böylece sanal olarak en ufak kayıt birimi cluster olur. Microsoft Windows temelli sistemler cluster terimini kullanırken, POSIX'lerde block terimi kullanılır. bu birime "allocation unit"'te denir. cluster genelde 4 sektörü kapsar.

# lock files

bir program işlem yaptığı dosyayı kilitleyebilir. bu şekilde başkaları bu dosyaya erişmeye çalıştığında işletim sistemi izin vermeyecektir. dosya sistemi ve işletim sisteminin sunduğu bir çok lock çeşidi vardır. her işletim sistemi farklı çeşit lock seviyeleri sunuyor. örnek: bazı filesystem'ler dosyayı kilitleyip, diğer yazılımlara read-only açabiliyor. bazı dosya sistemleri dosyanın bir kısmını lock'lamaya izin veriyor.

# "size on disk" vs "size"

sadece "size" denildiğinde; dosyanın byte olarak ne kadar veri içerdiği kastedilir. oysa aynı dosya için "size on disk" çok daha fazla yada az bir değere tekabül ediyor olabilir.

Harddisk'lerde en küçük kaydedilebilir birim "allocation unit"'tir. dosya sistemimizin "allocation unit" boyutu 4096 KB olduğunu varsayalım. bizim dosyamız ise 512 KB olsun. bu durumda bizim dosyamız 4096 KB "size on disk" değerinde olacaktır.

dosya sisteminin şifreleme yada sıkıştırma gibi özellikleri oluyor. bu özellikler sonucunda dosyanın "size on disk" boyutu, "size"'a göre artmış yada azalmış olabilir.

# sparse file

dosya sisteminin bu özelliği native destekliyor olması şart. bazı dosyalar örneğin 1 gb yer almaktadır. fakat bir yazılım henüz dosyanın tümüne yazmamıştır. dolayısı ile yazılmayan kısımlar fiziksel olarak yer kaplamaz. yazılmayan kısımlar yazılımcı tarafından empty bytes (buna dosya sistemi dünyasında "hole" derler. \x00 karakteridir.) olarak yazılmalıdır.

bu kullanılmayan kısımlar sayesinde "size on disk" değeri "size" değerinden düşük olabilir.

yazılımımız runtime'da dosyayı çok büyük görecektir, fakat fiziksel olarak dosya küçüktür.

# LVM (or Logical Volume Manager)
Linux çekirdeğinde default desteklenen bir yapıdır. bir dosya sistemi değildir. disk partition bölümlendirme formatıdır. Alternatifleri:
- Storage Spaces (Microsoft'un)
- Dynamic Disks (Microsoft'un - deprecated)

LVM kullandığımız MBR veya GPT kullanmayacağımız anlamına gelmez. LVM kullansakta, MBR vey GPT kullanmak durumundayız ki, HDD, diğer bootloder'lar tarafından boot edilebilsin. Aşağıdaki iki sistem birbirine alternatiftir:

- MBR based partition
- GUID Partition Table (UEFI standartları)

LVM, tüm bilgilerini 1 adet MBR veya 1 adet GPT partition'unda bulundurur. Yani LVM tanımayan bir sistem, LVM partition'unu standart bir MBR vey GPT partition'u gibi algılar. Fakat LVM okuyabilen bir sistem, bu partition içindeki özel bilgileri okuyarak, ilgili partition içerisindeki alt partition'ları okuyabilir. LVM, %100 sanal bir yapıdır.

gnome-disk-utility ve Gparted uygulamaları LVM'leri resmi olarak desteklemez ve LVM partition'larını, standart bir MBR veya GPT partition'u olarak görüntüler. Sadece ek bilgi olarak; LVM partition'unun yanında "LVM2 FP" yazar. Bu da şu anlama gelir: LVM'in version 2'si kullanılmış ve bu partition, LVM standartlarında __Physical Volume (PV)__'dir. 

LVM dünyasında; __Logical Volume__ ise, 1 adet PV içerisindeki her ayrı partition'a verilen isimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# variable shadowing (or değer gölgeleme)

programlama dünyasında kullanılan bir terimdir. bir değer scope'ta ve üst scope'ta aynı isimde olabilir. örnek:

```java
class MyClass {

    int myVal;
    void method(int myVal){

         //burada 2 farklı myVal mevcut
    }
}
```

her zaman üst scope'taki değere 'shadowed' adı verilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Comet programming
comet kelime anlamı: kuyruklu yıldız

- This is an umbrella term. It can be implemented via many different technologies (like Ajax).
- Comet is a programming technique that allows a web server to send updates to clients without requiring the clients to explicitly request them.
- This kind of programming technique is called "__server push__", which means that the server pushes data to the client. The opposite style is "__client pull__", which means that the client must pull the data from the server, usually through a user-initiated event, such as a button click.
- Mostly preferred on old web browsers (before WebSocket).

# Short polling
polling kelime anlamı: 1.seçme, oy verme 2.yoklama (örnek: kişi yoklama)

Client sends periodically request to server. So without any user interaction, the data is always transferring from both sides to both sides.

# long polling
If this technique is using via Ajax calls then it named "__Ajax long polling__".

In this technique the client sends request and it remains always open by server. The server sends response when it needs. If the server response to client, the client sends directly new request and waits again.

# HTTP streaming
HTTP standards (officially) supports to send data without a predefined size. In that way the client or server side can send data via streaming. Using this way we can make "comet programming".

Good article for all above topics: (source-id: 55)

# Webhook
Hook Türkçe kelime anlamı: çengel, tuzak.

- Hook'lar genel olarak bilişim dünyasında bir API siteğini veya mesajını intercept edip arada farklı bir işlem yapabilen programlara/fonksiyonlara verilen isimdir.

- "Webhook" terimi "hook" tanımına uymuyor gibi görünebilir, fakat webhook'ta hook şu anda devreye giriyor: uzun işlemimiz bittiğinde bir fonskiyon tetikleniyor. Aşağıdaki örnekte batch işlemi tamamlandığında, batch işlemini yapan makinede bir fonksiyon (hook) tetikleniyor. Bu hook fonksiyonu client'a batch işleminin bittiğine dair HTTP isteği atıyor.

- webhook mesajlaşma teknolojisinden (HTTP vs) bağımsızdır.

- Direk örnek üzerinden anlatalım: Uzun sürecek bir işlem başlatalım (batch işlem olsun). İsteği başlatma mesajını attığımız sunucu, işlem bittiğinde bize HTTP request atsın. Bunu destekleyebilmemiz için hem client hemde server taraftaki yazılımın webhook desteklemesi şarttır.

- Webhook'u direk olarka web browser'dan yapamayız. Çünkü web browser'da server-socket yoktur. Yani dışardan kimse bize request atamaz. Fakat dolaylı yollar kullanabiliriz: Websocket ile paralelden sürekli server'dan data okuruz. HTTP isteği yerine server bize websocket'ten mesaj atar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# excel

# farklı dildeki ofis setlerinde formüller
excel dosyalarındaki formüller ofis dil yüklenmiş ofis paketlerinde sorunsuz açılabilmektedirler. çünkü formüller text olarak değil, referansları ile saklanmaktadırlar. dolayısı ile sadece ekrana basarken farklı dillerde gösterilmektedirler. fakat ekstra yüklenen eklentinin multi-language desteği yok ise, eklentinin sunduğu formüller doğal olarak multi-language olmayacaktır.

# $ işareti
excel'de $ işareti sütun yada satır adresinin önüne gelebilmektedir. örneğin; $A$2 yada $A2 yada A$2 olabilir. $ işaretinin olduğu bölüm (sütun yada satır), bulunduğu hücredeki formül kopyala yapıştır yapıldığında değişmemektedir. $ olmayan kısımlar her zaman değişebilir. örneğin bir satır aşağıya kopyalanan hücrenin içinde $ olmayan satır değerleri otomatik olarak 1 artar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# CSV (or Comma separated value)

CSV dosya formatının avantajı şu: txt formatı gibi her editörle açılabiliyor. aynı zaman excel ve alternatifi uygulamalar CSV açarken her virgül arasını bir sütuna koyup açtığı için aynı dosya excel ile de sütunlara bölünmüş şekilde açılabiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# getter-setter

kullanım sebepleri:

private değişkenlere erişim/read/write sırasında şunlar yapılabilmesini sağlar:

- yetki kontrolü yapılabilir

- set edilecek veri ise; verinin uygunluğu/validation'ı (null mı? formatı uygun mu? gibi) kontrol edilebilir

- getter setter işlemlerinde log'lama yapabiliriz

- debug ederken setter veya getter'a log atılması işimizi hızlandırabilir ve debug edebilmemizi sağlar.

- immutability sağlayabiliriz (aynı objenin klonunu döndürürüz, setter'da ise işlem yaptırmayız.)

- Mocking, Serialization gibi kütüphanelerin hangi nesneleri alması gerektiğini belirtebiliriz. (bunu sadece annotation kullanarakta yapabiliriz)

- bizim sınıfımızdan extend etmiş sınıfların getter/setter'larda farklı davranmaları için esneklik sağlamış oluruz.

- getter'a tüm paketlerden erişimi açabilir, setter'a ise sadece aynı pakettekilerin erişebilmesini sağlayabiliriz (public getName, protected setName gibi)

- getter'ı herkese açıp, setter'ı tamamiyle private yapabiliriz (veya hiç setter yazmayabiliriz.)

- JPA'nın yaptığı gbi lazy-loading yapabiliriz. (bunu annotation'larla da yapabiliriz.)

- Mocking, Serialization, test için kolaylık sağlar.

- event propagation. bir field set edildiğinde (değiştiğinde), bir event yada class'ta bir başka değişikliği tetiklemek isteyebiliriz.

Yukarıdaki bazı özellikleri annotation'larla da sağlayabiliriz. fakat bu durumda annotation'lara bağımlı kalırız.

Yukarıdaki özelliklerin hepsi "encapsulation" yapmış olmamızı sağlamaktadır. Bir class ilk yaratıldığında bu özelliklere encapsulation'a ihtiyacımız olmayabilir fakat bu durum ileride de ihtiyacımız olmayacağı anlamına gelmez. Bu sebeple dışarıdan field'lara erişen diğer sınıflara, ilk zamanlardan itibaren getter-setter'lı vermemiz daha doğru olacaktır. Aksi durumda encapsulation uygulamak istediğimizde, field'ları kullanan bütün diğer kod bloklarında değişiklik yapmamız gerekecek.

# naming convention
get/set prefix'leri gerçek getter-setter amaçlı kullanılmayan metotlarda kullanmamaya gayret edilmelidir. Bu sebeple get yerine örneğin; fetch, find, has/have gibi prefix'ler verilebiliyor mu diye değerlendirmekte fayda var.

# Accessor vs Mutator
programlama dünyasında; Accessors, getter metotlarına verilen isim, mutators ise; setter metotlarına verilen isimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# IBM DataPower Gateway

donanımdır. web servise gelen isteklerde manipülasyon yapabilmemizi sağlar, genel istekleri filtreleyebilmemizi sağlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Chrome extension vs Chrome app

Chrome app'e daha çok API açık durumda olduğundan daha çok özellik'ten yararlanabiliyor. ve uygulamalar tarayıcıdan bağımsız bir pencerede tarayıcı penceresi başlatılmadan dahi başlatılabiliyor. burada amaç kullanıcıya daha çok native programmış havası yaratmaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# protocol vs format vs codec (or çözücü)
format; dosyaların yapısını(standartlarını) belirlerken, protokol; communication yapılarını(standartlarını) belirlemektedir.

__codec__ terimi ise; "__coder-decoder__" ın birleşiminden gelir. sinyal veya digital olan bilgiyi, bir formdan başka bir forma __convert(decode/encode)__ eden __yazılım/yöntem/araç__'tır. Terminolojik olarak bakıldığında __codec__ terimi compression algoritmalarını da kapsar. Çünkü bu algoritmalarda da form değişikliği meydana gelmektedir. Bu sebeple codec terimininin açılımı compression/decompression terimlerinin baş harflerinden olduğunu yazan kaynaklar da görebiliriz.

Form yapısının değişmesine şu şekilde örnekler verebiliriz:

- Java 8 ile gelen Base64 bir codec'tir. Bu sebeple paet ismi "java.util.Base64"'dir.
- Feign client'ta coder tanımı:

```java
GitHub github = Feign.builder()
                      .encoder(new GsonEncoder()) // buraya JacksonEncoder da gelebilirdi
                      .decoder(new GsonDecoder())
                      .target(GitHub.class, "https://API.github.com");
```

__codec__, hem __encode__ hem __decode__ işlemini birlikte kapsar.

__encoder__, __coder__ le eş anlamlıdır. Türkçe'de __kodlayıcı__ olarak kullanılır.

__decode__ işlemi, Türkçe'de __kodçözücü__ veya __dekoder__ olarak kullanılır.

# digital media container vs codec
container, dosya formatına denk gelmektedir. bir dosya container'ı, kendi içinde birden fazla ses, video, birden fazla subtitle, metadata (albüm ismi, şarkıcı ismi, albüm resimleri...) bulundurabilir. bazı container'lar sadece ses içerebilir. bazıları ise sadece resim içerebilir (resim dosyalarının da container'ı var).

codec ise container'ın içindeki sesin veya videonun standartıdır/formatıdır.

bazı container'lar birden fazla codec desteklemektedir.

bir dosya uzantısı, çoğu zaman dosyanın container'ından gelmektedir. fakat bu her zaman böyle olmak zorunda değil. Örneğin;
- flac audio codec'i, yine aynı isimde flac container'ında kullanılmaktadır.
- "mp3" dosyası, "MPEG-1" container'ında çalışan, "MPEG-1 Audio Layer III" codec'inde çalışan bir dosyadır.

| container name                            | description                                                                                                                                                                                                            |
|-------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| AIFF                                      | IFF file format, widely used on Mac OS platform                                                                                                                                                                        |
| WAV                                       | RIFF file format, widely used on Microsoft Windows platform                                                                                                                                                            |
| XMF                                       | Extensible Music Format                                                                                                                                                                                                |
| FITS                                      | Flexible Image Transport System - still images, raw data, and associated metadata.                                                                                                                                     |
| TIFF                                      | Tagged Image File Format - still images and associated metadata.                                                                                                                                                       |
| 3GP                                       | used by many mobile phones; based on the ISO base media file format                                                                                                                                                    |
| ASF                                       | container for Microsoft WMA and WMV, which today usually do not use a container                                                                                                                                        |
| AVI                                       | the standard Microsoft Windows container, also based on RIFF                                                                                                                                                           |
| DVR-MS                                    | "Microsoft Digital Video Recording", proprietary video container format developed by Microsoft based on ASF                                                                                                            |
| Flash Video (FLV, F4V)                    | container for video and audio from Adobe Systems                                                                                                                                                                       |
| IFF                                       | first platform-independent container format                                                                                                                                                                            |
| Matroska (MKV)                            | not limited to any coding format, as it can hold virtually anything; it is an open standard container format                                                                                                           |
| MJ2                                       | Motion JPEG 2000 file format, based on the ISO base media file format which is defined in MPEG-4 Part 12 and JPEG 2000 Part 12                                                                                         |
| QuickTime File Format                     | standard QuickTime video container from Apple Inc.                                                                                                                                                                     |
| MPEG program stream                       | standard container for MPEG-1 and MPEG-2 elementary streams on reasonably reliable media such as disks; used also on DVD-Video discs)                                                                                  |
| MPEG-2 transport stream ( a.k.a. MPEG-TS) | standard container for digital broadcasting and for transportation over unreliable media; used also on Blu-ray Disc video; typically contains multiple video and audio streams, and an electronic program guide        |
| MP4                                       | standard audio and video container for the MPEG-4 multimedia portfolio, based on the ISO base media file format defined in MPEG-4 Part 12 and JPEG 2000 Part 12) which in turn was based on the QuickTime file format. |
| Ogg                                       | standard container for Xiph.org audio formats Vorbis and Opus and video format Theora                                                                                                                                  |
| RM                                        | RealMedia; standard container for RealVideo and RealAudio                                                                                                                                                              |

# kayıpsız ses
__Pulse code modulation (or PCM)__ kayıpsız olarak analog sesi, digital ortama kaydetme tekniğidir. özel bir tekniktir. bu bir format değildir. PCM'e alternatif farklı tekniklerde mevcuttur.

WAV formatı, raw ses data'sını tutar. yani kayıpsızdır. ortalama 5 MB'lik bir MP3 dosyası, ortalama 500 MB'lik bir WAV dosyasına denk gelmektedir.

FLAC formatı ise, kayıpsız sıkıştırma uygular. yani RAR, ZIP tarzı sıkıştırma uygularlar. fakat sıkıştırılan dosya formatı belli olduğu için, sıkıştırma oranı çok yüksektir. örneğin; 500 MB WAV dosyası, kayıpsız şekilde sıkıştırılıp, 50 MB'lik bir Flac dosyasına denk gelir.

Kayıplı sıkıştırmalar ise MP3 gibi dosyalardır. ses kalitesinden ödün vererek sıkıştırma uygularlar ve boyutları çok ufaktır.

Piyasada FLAC olmasına rağmen WAV formatı tercih edilebilir. Bunun en önemli sebebi, müzik editleme programıyla çalışanların, sürekli uncompress ve compress işlemlerini yapmak ile zaman kaybetmemesi gerektiğidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# BPM (or Business Process Manager)

İş akışını yöneten otomasyonlara verilen genel isimdir.

Piyasadaki bazı BPM Platformları:

- IBM BPM (eskiden Lombardi firması tarafından geliştiriliyordu. IBM satın aldı.)

- Activiti (açık kaynaklı, Alfresco firmasın tarafından geliştiriliyor)

- camunda (açık kaynaklı)

BPM platformları; BPM Engine'ini, IDE'ler için eklentiler, programlama dilleri için engin'i kullanmayı sağlayan API'leri, desteklediği formatları (örnek: BPNM) ve daha bir çok şeyi içinde kapsamaktadır.

BPM platformlarının birçok avantajı vardır. İş akışını belirler ve iş akışı için client tarafa data yollar. Örneğin HTML yollayabilir, bu şekilde her client bunu parse edip ekranda ne göstereceğine karar verebilir. ATM, Mobil, masaüstü, web client'ları tek bir iş akışı sunucusundan aynı endpoint'lerden yönetilebilir. Sadece client'a data yollamaz, aynı zamanda session ile ilişkilendirilip her user'ın hangi iş akışına girebileceğini, iş akışı ekranının client tarafta yanlışlıkla kapanması durumunda devam etmesini sağlayabilir. Benzer şekilde hangi user'ların hangi process'lerde ve hangi sayfada olduğunu monitör edip yöneticilere sunabilir. istenildiğinde user'lar engellenebilir. performans ölçümü yapılmasını sağlayabilir.

# Business Process Model and Notation (veya BPMN)

İş akışını gösteren bir XML standartıdır. Sürümleri mevcuttur.

bir bpmn dosyasının içinde birçok akış (or flow) olabilir.

her akış içerisinde birçok task olabilir. her task son kullanıcı için birer sayfa gibi düşünülebilir. fakat öyle olmak zorunda değildir. client kendi için bir sayfayı 2-3 sayfa içerisinde toplayıp sunucuya istekte bulunabilir.

Gateway (geçiş) her task'tan task'a giderken isteğe bağlı kullanılan karar mekanizmalarıdır. örnek diğer task'a giderken, koşul koyulabilir. eğer koşula sağlanmıyorsa kullanıcı farklı bir task'ta yönlendirilebilir.

flow, gateway, task gibi objeleri birçok türevi vardır.

BPM sürecini bir örnek ile açıklayalım: internetten kredi çekilebilsin. son kullanıcı banka sistemine login olur ve kredi sürecini başlatır. kullanıcıdan birçok bilgi alınıp son olarak onay tuşuna bastı. artık bu arada kredi henüz alınmadı. daha bankacının onay vermesi gerekmektedir. işte süreçte user 'a bir bildirim yapılıyor ve user daha ileri gidemiyor. ne zaman kredi çekmeye kalksa (yani süreci tamamlamak istese) "temsilcinizden onay bekliyorsunuz" mesajını görüyor. artık BPM akışı bankacının akışına yönlenmiş durumda ("bpmn" formatında ok işareti). eğer bankacı kendi sürecini başlatırsa, ve kendi sürecinde olan kısmını tamamlarsa yine ok işareti müşterinin sürecin içine ok işareti ile göstereceğinden, artık süreç müşteriden devam edecektir. müşteri artık kredi sayfasını açtığında karşısında raporu görecektir. rapor göstermede de sürecin (son) bir parçasıdır. eğer temsilci isterse kullanıcıyı başka bir akışa yönlendirip ek bilgi talebinde bulunabilirdi.

sub-process, bir task gibi görünür aram referansı bir sub-process yani task grubudur. tekrar kullanılabilirlik sağlamak için kullanılırlar.

# camunda modeler

İş akışlarını hazırlamaya yarayan masaüstü GUI yazılımıdır.

# kod örneği

aşağıdaki kod parçası Activity BPM Java kodudur:

```java
// engine'i devreye sokuyoruz

ProcessEngineConfiguration cfg = new StandaloneProcessEngineConfiguration()

        .setJdbcUsername("sa")

        .setJdbcPassword("");

    ProcessEngine processEngine = cfg.buildProcessEngine();

    String pName = processEngine.getName();

    String ver = ProcessEngine.VERSION;

//bir akış deploy ediyoruz

    RepositoryService repositoryService = processEngine.getRepositoryService();

    Deployment deployment = repositoryService.createDeployment()

        .addClasspathResource("moneytransfer.bpmn").deploy();

//processDefinition arama sonucunu içeriyor

    ProcessDefinition processDefinition = repositoryService.createProcessDefinitionQuery()

        .deploymentId(deployment.getId()).singleResult();



//money transfer akışının bir instance'ını başlatıyoruz

    RuntimeService runtimeService = processEngine.getRuntimeService();

    ProcessInstance processInstance = runtimeService

        .startProcessInstanceByKey("moneytransfer");

// BPM'in sunduğu bir çok servisi kullanmak için bir değere atıyoruz

    TaskService taskService = processEngine.getTaskService();

    FormService formService = processEngine.getFormService();

    HistoryService historyService = processEngine.getHistoryService();

// process dongüsüne girip task'lar çekiliyor ve her task'ı içindeki data çekiliyor ve data servise atılıyor

    while (processInstance != null && !processInstance.isEnded()) {

List<Task> tasks = taskService.createTaskQuery()

          .taskCandidateGroup("managers").list();

      System.out.println("Active outstanding tasks: [" + tasks.size() + "]");

      for (int i = 0; i < tasks.size(); i++) {

        Task task = tasks.get(i);

        System.out.println("Processing Task [" + task.getName() + "]");

        Map<String, Object> variables = new HashMap<String, Object>();

        FormData formData = formService.getTaskFormData(task.getId());

        for (FormProperty formProperty : formData.getFormProperties()) {

          if (StringFormType.class.isInstance(formProperty.getType())) {

            System.out.println(formProperty.getName() + "?");
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Swagger

"SmartBear Software" tarafından geliştirilen REST API doc'u hazırlanması ve client/server API kodlarının otomatik oluşturulması amaçlı geliştirilen yazılım tool'larının bütünüdür.

# Swagger definition file

REST API'lerin bilgilerinin Swagger formatında yazıldığı JSON yada YAML dosyası.

# Swagger editor

JSON/YAML dosyasını HTML'e çeviren (HTML API doc üreten) yazılımın adıdır. web projesidir. JSON/yaml'i verirsiniz, yan tarafta HTML sürümü görünür.

'Swagger editor'ün arayüzü bu şekildedir: (source-id: 57)

# Swagger Hub
Closed-source cloud based (not self-hosted) web UI for:
- Swagger-editor
- comment support on Swagger-editor
- Visual editor support for Swagger-editor.
- Team management (users, permissions...)
- Common models (DTOs) cross multiple(independent) Swagger(or OpenAPI) files(projects)
- versioning (simply: Swagger-hub get a snapshots of current file and tags it.)
- visual comparator of Swagger(OpenAPI) files (this feature named as "Forking & Merging at documentation)
- push Swagger(OpenAPI) file to remote Source Management System
- Style validation (models should have example values, description must mot have empty string...)

# Swagger Codegen

programlama dilinde yazılmış kodları YML dosyasına çeviren ve YML dosyasından server/client kodu hazırlayabilen yazılımdır.

# Swagger UI

Swagger-ui sadece HTML/Javascript'ten oluşan bir web yazılımıdır. bu kodları sunucuya attığınızda, index.html'i üzerinden bir web sayfası açılır. bu sayfa sizden YML dosyasının URL'sini ister. URL'yi girdiğinizde, sayfa otomatik olarak o YML'yi çeker ve parse eder. parse ettikten sonra, web sayfasına REST API DOC'ları listeler. bu şekilde doc hazırlanmış olur.

aslında "Swagger editor" arkaplanda bu yazılımı kullanır.

'Swagger UI'ın arayüzü bu şekildedir: (source-id: 58) Dikkat edilirse yukarıda URL bekleyen bir textbox var. Buraya girilen YML dosyası çekiliyor ve parse edilip sayfanın aşağısındaki kısma DOC yansıtılıyor.

# Swagger parser
OpenAPI dosyasını alıp, onu parse edebilmemizi sağlayan Java kütüphanesidir. örnek kullanım:

```java
OpenAPI openAPI = new OpenAPIV3Parser().read("./path/to/openapi.yaml");

// Read details of file from OpenAPI object
```

# OpenAPI Specification (or OAS or old-name:Swagger Specification)
Swagger YML dosyasında bir standartı var. bu WSDL dosyasının benzeri bir YML dosyası. bu dosyanın formatını 3.0 sürümüne kadar Swagger kendi belirliyordu. 3.0 ile OpenAPI olarak yeni bir repo'da geliştirmeye başlandı. OpenAPI'nin ilk sürümü 3.0 ile başlamış oldu.

bankacılık sektöründe kullanılan "open API" ile hiçbir bağlantısı yoktur. "OpenAPI Specification"'daki "open" kelimesi spesifikasyonun açık kaynaklı standartlara dayandığını temsil etmektedir. OAS sadece REST için bir spesifikasyondur.

OpenAPI repo'sunda eski sürümlerin doc'ları da yer almaktadır. onlar "open API 2" diye yazılmış fakat Swagger formatlarıdır.

Piyasada "open API" veya "public API" terimleri, API'nin dışarıya açıldığını göstermek için kullanılmaktadır. Bu da bazen karışıklığa sebep olabiliyor.

# Swagger version history (or OpenAPI versiyon history)
| version | release date | note                                             |
|---------|--------------|--------------------------------------------------|
| 3.1.0   | 2021-02-15   | Release of the OpenAPI Specification 3.1.0       |
| 3.0.3   | 2020-02-20   | Patch release of the OpenAPI Specification 3.0.3 |
| 3.0.2   | 2018-10-08   | Patch release of the OpenAPI Specification 3.0.2 |
| 3.0.1   | 2017-12-06   | Patch release of the OpenAPI Specification 3.0.1 |
| 3.0.0   | 2017-07-26   | Release of the OpenAPI Specification 3.0.0       |
| 2.0     | 2014-09-08   | Release of Swagger 2.0                           |
| 1.2     | 2014-03-14   | Initial release of the formal document           |
| 1.1     | 2012-08-22   | Release of Swagger 1.1                           |
| 1.0     | 2011-08-10   | First release of the Swagger Specification       |

# Springfox

Java-Spring projeleri için REST-DOC oluşturma parse kütüphanesidir. Springfox bir interface sunuyor. Springfox kullanmak için Springfox-Swagger gibi implementasyonların projeye import edilmesi şarttır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# .Net framework
sistemde yüklü olmalı ki, bu framework'ten yararlanan uygulama yürütülebilsin. aynı JVM'de olduğu gibi. .Net, sadece C# değil, başka diller için de API'si mevcuttur. ".Net" bir runtime'a ihtiyaç duyuyor. Aynı JVM gibi. bu runtime'ın ismi __Common Language Runtime (or CLR)__. JVM, Bytecode'u çalıştırabilirken, ".Net", __Microsoft intermediate language (or MSIL)__'ı çalıştırmaktadır.

# .Net core
.Net'in sadece Linux, MacOS ve Microsoft Windows'ta çalışılabilir bir türevidir. sadece Microsoft Windows'ta çalışacak bir uygulama yazılacaksa (classic) ".Net" tercih edilmelidir. "(classic) .Net", .Net core'a göre daha çok özellik içeriyor.

# ADO.NET vs EntityFramework vs OleDb vs ODBC (or Open Database Connectivity)
OleDb vs ODBC, Java'daki JDBC'ye alternatif geliyor. Fakat XML gibi data kaynaklarından da okuma yapabiliyor.

ADO.NET ise OleDb ve ODBC'yi wrap eden bir framework.

EntityFramework ORM'ye denk geliyor. EntityFramework, 'ADO.NET'i wrap eder.

# MONO
C# uygulamalarının Microsoft Windows harici sistemlerde derlenmesi ve çalıştırılması için gerekli tüm projeleri (IDE, CLR...) kapsar. açık kaynaklı bir projedir.

# MonoDevelop
MONO projesi altında geliştirilen IDE.

# Xamarin
MONO projesi tabanlı bir framework. mobil platformlar için C#'ta yazılan uygulamaların diğer sistemlerde çalışabilmesini sağlıyor.

# ISS (or Internet Information Services)
Microsoft'un web server'ı.

# ASP (or Active Server Pages)
Microsoft'un JSP alternatifi framework'ü.

".asp" dosya uzantısını kullanıyor.

Sadece vbscript ile kod yazılabilmesini sağlıyor.

örnek kod:

```html
<html>
<body>
    <%response.write("Welcome to GeeksforGeeks!")%>
</body>
</html>
```

# ASP.Net
.Net projesi'nin bir alt kümesidir. .Net'in sadece ASP kümesini kapsar. Artık CLR'ı kullana herhangi bir dil ile de kodlanma yapılabilmesini sağlıyor. Tabi bu şekilde .NET API'lerinin de kullanılması mümkün oluyor.

".aspx" dosya uzantısını kullanıyor.

örnek kod:

```html
@{
    var rank = 50;
}
<html>
<body>
@if (rank < 60)
{
    <p>Welcome to GeeksforGeeks!</p>
}
</body>
</html>
```

# ASP.Net core
.Net core projesi'nin bir alt kümesidir. .Net core'un sadece ASP kümesini kapsar. Bu yapı ile "ISS express" olarak adlandırılan portable ve Microsoft Windows harici sistemlerde çalışabilen bir ISS türevi geliştirilmiştir. böylece ASP.Net core ile cross platform sunucular çalıştırılabilmektedir.

# nuget
Maven tarzı C# için dependency yöneticisi.

# steel toe
"Spring cloud" mimarisindeki servislere entegrasyon sağlamak için hem sunucu hemde client kütüphanelerini içeren, C# için açık kaynaklı kütüphane.

# Build Tools (or Microsoft Build Engine or MSBuild or Build Tools for VS or Build Tools for Visual Studio)
is a platform for building Windows applications.

Since VS2017, MSbuild becomes a separate build package.

# Windows SDK
build apps for Windows 8+. It contains headers, libraries, metadata, and tools for building Windows 10+ applications.

# Windows App SDK
Windows SDK'dan farklıdır. Bu daha çok

# UWP (or Universal Windows Platform)
Windows 8+ ile gelen bir platformdur. Burada yazılan uygulamalar diğer birçok cihazda (PC, Xbox, Mobile, Hololens, iot, Mixed-reality headset...) çalışabilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Exif (or Exchangeable image file format)

Media dosyaları için kullanılan aynı dosyaya gömülen metadata bilgilerinin formatıdır. "Exiftool" açık kaynaklı komut satırı programıdır. Bu program ile dosyalarının Exif bilgileri okunup manipüle edilebilmektedir.

Exif bilgileri arasında birçok çeşit bilgi vardır. Örneğin; fotoğrafın çeken device bilgileri, fotoğrafın çekildiği saat, lokasyon...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# popup

açılan ufak pencerelerin tümüne verilen genel isimdir.

# Modal vs Dialog

ikiside aynı anlama gelmektedir. Pencerede kullanıcı için focus olunan bir popup açılır. Core HTML'in dialog özelliği mevcut (Önemli: Alert değil! İçine istediğimiz objeleri atabildiğimiz popup  var.). Fakat BootstrapJS gibi kütüphaneler kendi dialog'larını da yazmıştır.

# Popover vs Tooltip

ikiside aynı anlama geliyor. Genelde fare ile bir objenin üzerinde durulduğunda açıklaması için açılan popup'a verilen isimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Sistem Programcılığı

Daha çok işletim sistemi ile yada donanım bilgisi gerektiren yazılım dilleri ve kütüphaneleri kullanarak yazılım geliştirmedir. Bu sebeple; genelde üst seviyeli diller buna gruba dahil olmaz. Bu sebeple; genelde sürücü (driver) yazanlar bu gruba dahildir.

# sistem çağrıları (or çekirdek çağrıları)

İşletim sistemi ile kullanıcı programları arasında tanımlı olan arayüz, işletim sistemi tarafından tanımlanan bir prosedürler kümesidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Controller Area Network (or CAN bus)

bir iletişim protokolüdür. genelde araçlarda her birim arası haberleşme için tercih edilir.

her birim ağı sürekli dinler ancak ağ boş ise mesajını tüm ağa yollar. her birim kendi alacağı mesajı önceden bilir ve kendisini ilgilendirmeyen mesajları dikkate almaz. haberleşme limitli max 1km uzunlukta kablo ve 1mb/sec hızında gerçekleşmektedir. çünkü sürekli ağı dinleyen birimler vardır. eğer ağın boş olduğunu düşünüp, birden fazla birim aynı anda mesaj yollamaya kalkarsa hata (çatışma - collision) meydana gelir. bu sebeple kablo uzunluğu ve veri boyutu sınırlı tutulur. bu şekilde ağ dinlemelerinde uzaktan gelen veriden habersiz olma gibi bir durum söz konusu olmaz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# file type detection

dosya formatını tanımak %100 olarak mümkün değildir. bazı uygulamalar her format için dosya kalıplarını (format yapısını-template'ini) inceliyor ve benzerlik oranına göre sonuç döndürüyor.

bazı dosya formatları genelde dosyanın en başına dosya formatını yazan birkaç bitlik bir yer bulundururlar. fakat bu bile bir dosyanın %100 txt olmayacağı anlamına gelmez.

# binary file

saf text olmayan bütün dosyalara verilen genel isimdir. anlamlı hale getirebilmek için basit yada kompleks parse işlemleri yapılmalıdır.

PDF, Word, Zip, Exe, Bin dosyaları binary'dir. oysa JSON, XML, Java, CSV, SH dosyaları text dosyalarıdır.

dosya sistemleri seviyesinde bir farklılık yoktur. sadece son kullanıcı için "text dosyası" ve "binary dosyası" olarak gruplandırılmışlardır.

bazı programlama dilleri API'lerinde dosya okuma işlemlerinde binary olup olmadığı bilgisini isteyebilir. bunun birkaç sebebi var:

- eğer binary dosya açılırsa; satır sonu karakterlerini işletim sisteminin belirlediği default karaktere göre otomatik değiştirmez

- eğer binary açılırsa; end-of-file karakteri en sona otomatik koyulmaz.

- eğer binary açılırsa; write read metotları byte okur, oysa text açılırsa belli bir encoding'de string döner.

her API farklı kurallar/tepkiler gösterebilir. bu sebeple API'yi incelemek gerekli. bazı kütüphaneler byte ve text için ayrı ayrı sınıf/metot sunuyor olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Steganografi (or Steganography)

bilgiyi gizleme (önemli: şifreleme değil) bilimine verilen addır. Steganografi'nin şifrelemeye göre en büyük avantajı bilgiyi gören bir kimsenin gördüğü şeyin içinde önemli bir bilgi olduğunu fark edemiyor olmasıdır, böylece içinde bir bilgi aramaz. Bilişim dünyasında kullanılır. örneğin; seste veya görüntüdeki küçük bozuklukları insan beyni fark edemediği için, kasıtlı olarak periyodik bozukluklar şeklinde dosyanın içine başka bir dosya saklanabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Cache (or Önbellek)

yazılımsal ve donanımsal olarak ayrılmaktadır. donanımsal önbellekler; daha hızlı erişilen belleklere denir. örneğin "CPU cache". CPU cache CPU üzerindedir ve RAM'den dahi daha hızlı erişilebilir. fakat maliyetli olduğundan çok çok ufak boyuttadır. CPU cache'in, özel amaçlı donanımlar hariç, yazılımcılar tarafından kullanması önerilmez. fakat klasik işletim sistemi bu kısmın düzenlenmesi için API sunmuş olabilir.

# tr:tampon (or tampon bellek or data buffer or buffer or ara bellek)
cache ile tamamen farklı kavramlardır. cache bilgiye hızlı erişmek için bir data'nın farklı fiziksel alanda tutulan bölgesidir. oysa buffer; bir data'nın bellekte fiziksel olarak farklı bir yere taşınmadan önce tutulduğu bölgedir.

# JCache
JCache is the Java caching API. It was defined by JSR107.

Bunu kullanmamız bir için runtime'da bir implementasyona ihtiyaç duyarız. Örnek implementasyon: Ehcache.

arayüzler için Maven dependency'si:

```xml
<dependency>
  <groupId>javax.cache</groupId>
  <artifactId>cache-API</artifactId>
</dependency>
```

JCache'nin RI (or reference implementation)'u mevcut. Fakat bu RI, prod ortamında kullanılmak üzere tasarlanmamıştır. Sadece implementasyonun işleyişini gösterebilmek adına tasarlanmıştır.

# Spring Cache
birçok implementasyon (Jcache...) için bir wrapper API'dir.

```java
import org.springframework.cache.annotation.Cacheable;

@Cacheable("sum-cache-id")
// or the below is same:
@Cacheable(value = "sum-cache-id")
int cachedMethod(Class1 class1, int param2){

  // do something;
}
```

Spring stores the cache as a in-memory Key-Value storage. Spring stores to cache:
- the arguments of the function as "key"
- the return value of the function as a "value".

As default, the "key" is all arguments of a funtion. For the above example the key is: all the fields of class1 and param2.

But we can specify what should be the key by SpEL syntax. For example in the below example the key is only "int param2".

```java
@Cacheable("sum-cache-id", key = "#param2")
int cachedMethod(Class1 class1, int param2){

  // do something;
}
```

Or we can define only a spesific field of any parameter. For example in the below example, the key is only "fieldX (which is member of Class1)".

```java
@Cacheable("sum-cache-id", key = "#class1.fieldX")
// We assume that Class1 has "fieldX" field.
int cachedMethod(Class1 class1, int param2){

  // do something;
}
```

Spring as default gets the "hashCode" of all the arguments which are passed to method. On the new versions of Spring (with version 4), it started to use "equals" method to find the key. Because they were a collision on hashes.

We can optionally impelement a custom key generator (we need to impelemnt the KeyGenerator interface for that).

Spring as default uses all arguments and uses equals (or hash) to find the key. But if we will define the key manually (example: "#class1.fieldX") then Spring only uses equals (or hash) of fieldX argument.

We can also define condition if it will cached or not:

```java
@Cacheable("sum-cache-id", condition = "#age < 25")
int cachedMethod(Class1 class1, int age){

  // do something;
}
```

@CacheEvict is using to delete the cache which is specified as a parameter. For example on the below example we remove all the cache storeage with ID "sum-cache-id".

```java
@CacheEvict("sum-cache-id")
// or the below is same:
@Cacheable(value = "sum-cache-id")
int resetCache(Class2 class2){
  
  // do something;
}
```

On the below example each time we call resetCache method, Spring removes from cache the value which has "class1.fieldX". Remember that cache was a Map\<Key, Value\>.

```java
@CacheEvict("sum-cache-id", key = "#class1.fieldX")
// We assume that Class1 has "fieldX" field.
@Cacheable(value = "sum-cache-id")
int resetCache(Class1 class1){
  
  // do something;
}
```

# Guava Cache example

Guava isimli kütüphanenin cache modülü için örnek kod:

```java
CacheLoader<String, String> userLoader;
userLoader = new CacheLoader<String, User>() {
    @Override
    public User load(String userId) {
        return getFromDatabase(userId);
    }
};

LoadingCache<String, String> userCache;
userCache = CacheBuilder.newBuilder()
                        .maximumSize(50)
                        .expireAfterAccess(2, TimeUnit.MINUTES)
                        // or expireAfterWrite
                        .removalListener(myListener)
                        .build(userLoader);

// or by weight:
// if the userName is longer than 1000 it will be removed from cache.
userCache = CacheBuilder.newBuilder()
                        .maximumWeight(1000)
                        .weigher(new Weigher<String, User>() {
                            public int weigh(String userId, User user) {
                              return user.getName().lenght();
                            }
                        .build(userLoader);

// below get method will return null
System.out.println(employeeCache.get("300"));

// we add to cache with a key. cache is key-value storage.
// on below example 300 is the keyword of data.
// manually add value to cache:
employeeCache.put("300", new User("300", "Jack"));

// below the data will be return from cache:
System.out.println(employeeCache.get("300"));
```

görüldüğü gibi getFromDatabase yerine tekrar çağrıldığında RAM'deki bellekten getiriyor sonucu. yazılımsal cache'lerin en temel/basit örneklerden biri budur.

Bazı uygulamalar user-home dizininde bir klasörde cache isimli dizinler yaratmaktadır. uygulamalar kapandığında yada belli aralıklarla RAM'deki bu cache bilgilerinı dosyalara yazarlar. böylece uygulama restart edildiğinde, cache direk olarak RAM'e yüklenecektir. tekrardan cache'in dolması beklenmeyecektir.

# cache algorithms (or cache replacement algorithms or cache replacement policies)

bu tarz algoritmalar bir sıralı listeyi yönetmek için kullanılırlar. bu algoritmalar, hangi parçaların tutulacağı ve yeni parçalara yer açmak için hangi parçaların atılacağına karar vermek zorundadır.

 örnekler:

- First In First Out (or FIFO)

- Last In First Out (or LIFO)

- Least Recently Used (LRU): en eski kullanılan veri, yeni eleman geldiğinde ilk çıkarılır.

- Most Recently Used (MRU): en yeni kullanılan veri, yeni eleman geldiğinde ilk çıkarılır.

- Least Frequently Used (LFU): en az sıklıkta kullanılan veri, yeni eleman geldiğinde ilk çıkarılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# User Account Control (or UAC)

(başka bir başlıkta da bu konu hakkında yazı mevcut)

# Squirrel

Atom text editörün kullandığı bir altyapıdır. bu yazılım sayesinde uygulama program files'e değilde, user-home dizinine kurulmaktadır. bu şekilde, uygulama güncellendiğinde son kullanıcıdan yetki istememektedir. çünkü program files'teki yazlım güncellemesi için son kullanıcıdan yetki istenmektedir. Squirrel aynı zamanda update işlemlerini kolaylaştıran ve API sunan bir altyapıdır.

# Chrome auto update

Google Chrome Squirrel veya benzeri bir yapı kullanmamaktadır. Chrome kurulduğu işletim sistemine update işlemi için Microsoft Windows servisi eklemektedir. bu servis admin yetkisindedir. bu sebeple son kullanıcıya soru sormadan program files'teki Chrome'u güncelleyebilmektedir.

# Microsoft Windows (arkaplan) servisleri kullanıcı kısıtlamaları ile yürütülüyor

Microsoft Windows servisleri kullanıcı bazlı çalışmaktadırlar. bu sebeple yetkileri kısıtlanabilir. bu kullanıcıların home klasörleri olmayabilir. çünkü sadece yetki kısıtlama amaçlı oluşturulmuşlardır. az yetkiliden çok yetkiliye örnek kullanıcılar; LocalService, NetworkService, LocalSystem.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# mmap
Memory map'in kısaltmasıdır.

POSIX system call'udur. bir dosyayı memory'de adresler. dosyanın her byte'ına RAM'den erişilebilir olur. fakat birçok nmap implementasyonu tüm dosyayı RAM'e kopyalamaz. lazy loading yapar.

# genel UNIX-like mimarisindeki memory terimleri

- resident memory

  resident Türkçe kelime anlamı: oturan, sakin, yerleşmiş

  sadece RAM'deki memory.

- shared memory

  diğer process'lerle ortak kullanılan bellek boyutudur. Inter-process communication protokolleri aracılığı ile (veya benzeri yollar ile) kullanılan ortak memory'dir.

- Virtual Memory (önce "Sanal bellek (or swap space or takas alanı)" başlığı okunmalı)

  virtual memory; swap memory'den farklıdır.

  virtual memory = swap + resident memory + mmap

# "Gnome system monitor" ile görülen process değerleri

- memory

  memory = resident - shared (çıkarma işlemi)

- opened files

  "__lsof (list of open files__ kısaltmasından gelir)" komutu çıktısının aynısıdır.

  bu listede bir process'e ait açık olan tüm dosyalar tutulur. dosyalar soket, pipe (process output), binary files gibi dosyalar olabilirler (UNIX-like'ta herşey dosya olması yapısı)

- memory map

  bir process'in virtual memory'si için her detayın (kaynağın) tutulduğu haritadır. mmap system call'u ile alakası yoktur. bellekteki tüm değerler (swap + resident memory + mmap)  yanyana tutulmaz. parça parça tutulur. bu sebeple bu tüm parçaların başlangıç ve bitiş adreslerine memory map haritasından bakabiliriz.

# htop komutu ile gösterilen bazı sütunlar

- PPID

Parent process ID

- PGRP

Process group ID. her process'in group ID'si parent process'in group ID'sidir. fakat eğer process'in kendisi alt process'ler başlatmış ise, yeni bir group ID alır. aldığı group ID, process ID ile aynı atanır.

- NI (nice)

işlemciye giriş için öncelik seviyesi. numara büyüdükçe önceliği yükselir.

- PR (priority)

PR = 20 + NI. Nice -20 +20 aralığındaki değerini gösteriyor. bu değer user tarafından değiştirilebilen  öncelik seviyeleridir. oysa Linux'ta çok daha fazla değer vardır. tüm değerleri priority gösterirken, kullanıcı bazlı seviyeleri NI gösteriyor.

# top/htop shows different result than Gnome system monitor

CPU farkının sebebi: Gnome system monitor 8 CPU'muz varsa 8'i %100'e tamamlıyor. yani tüm CPU'lar tümüyle harcanırsa ancak %100 gösteriyor. fakat top yada htop komutunda bu değer %800 oluyor. bu sebeple genelde 8 CPU'lu bir makinede Gnome system monitor 8 kat daha düşük değer gösteriyor. tabi Gnome system monitor aynı zamanda bu değeri yuvarlıyor.

memory: memory değerleri de yuvarlanıyor.

# Microsoft Windows 10 task manager işlem detayları sekmesinde olan bilgiler:

- PID: Process ID

- Status: process state

- Session ID: Login user ID

- CPU Time: How much time the process use the CPU (not: process yaşam süresi değil)

- Image Path name: Path of executable which started

- Command Line: Hangi komut ile bu programın başlatıldığını yazar. Örneğin "myservice -param1 -param2" gibi. "myservice" burada full path'de olabilir.

- Platform: 32 bit or 64 bit executable is running

- Operating System Context: Hangi uyumluluk modunda çalıştırıldığını belirtir

- Description: İşletim sistemi belirtmiş ise uygulama ismi, belirtmemiş ise process adı yazar.

- data execution prevention: CPU ve işletim sisteminin desteklemesi gereken bir özelliktir. bu özellik sayesinde belirtilen uygulama, işletim sisteminin belirttiği bellek alanları dışında bir yere erişmesi engellenir. bu alan, window'un bu programa bu güvenliği uygulayıp uygulamadığını yazar.

- UAC Virtualization: UAC Microsoft Windows'ta yeni gelen bir teknoloji. Bu teknoloji ile uygulamaların yetkileri kısıtlanmaktadır. Eski uygulamalar ile uyumluluk sağlayabilmek amaçlı bu özellik o uygulama için enable hale gelebilir. Bir uygulama  için bu özellik aktif ise, uygulama yetkisi olmayan bir yere yazmaya kalktığında, yetki hatası almıyor fakat bu dizine yazdığı dosyalar aktarılıyor: C:\Users\username\AppData\Local\VirtualStore

- Base priority: İşlemciye giriş için öncelik seviyesidir.

- Handles: Microsoft Windows uygulamanın açtığı bazı kaynakları tutmak/yönetmek Handle kavramından yararlanır. Soket, dosya, pencereler, pencere objeleri gibi... Bunların yazılımdaki "sınıf"'ları gibi düşünülebilir. Bunların toplam sayısı bu alanda yazmaktadır.

- Cycle: CPU hayat döngüsünün (fetch-decode-execute instruction) yüzde kaçının bu işleme ait olduğunu yazıyor.

- Private working set: İşlemin kullandığı toplam RAM değeri

- Peak working set: Uygulamanın yaşam süresi boyunca aldığı maximum "working set" değeri.

- Working set: Private working set + Shared working set

- Working set delta: "Working set" değerindeki anlık değişimin toplam değeri. Örnek: 300'den 350 ‘ye çıkmış ise o saniyede 50 değerini alır.

- Shared working set: Diğer process'lerle paylaşılabilen toplam "working set" değeri.

- Commit size: Toplam Sanal bellek değeri

- Page faults: Uygulama bellekten bir bilgiye erişmek istediğinde, önce RAM'e bakar. orada yoksa "sayfa hatası" alır. bu demek oluyor ki, sanal belleğe (yani; HDD/SSD'ye yani; ikincil belleğe) bakması gerekli. uygulama başladığında itibaren kaç kere bu hatayı aldığını gösterir.

- PD Delta: "Page faults"'un o andaki değişim değeri.

# Linux Page Cache
Linux'ta olan bir özelliktir. "meminfo" ve "free" komutlarının çıktısında 'Linux Page Cache'nun kaç MB harcadığını 'Cached' başlığı altında görebiliriz. kaynak: (source-id: 62) "Memory Usage" başlığı ilk cümle.

Dosyadan read yapıldığında dosya memory'ye alınır. eğer dosya kod içerisinden kullanımı bırakılırsa; bu memory bölgesi Linux Page Cache'e saklanır. Eğer tekrar aynı dosya okunmak istenirse bu bölgeden okunur. kaynak: (source-id: 62) ilk paragraf.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Sanal bellek (or swap space or takas alanı)

sanal bellek terimi 3 farklı durum için eş anlamlı kullanılıyor:
- top komutunun manual'ında belirtildiği gibi 3 farklı memory var. bir çeşidi virtual memory olarak adlandırılmış. fakat bu bildiğimiz swap dosyası alanı değil. (source-id: 422)
- swap space
- her process , OS tarafından sanal bir bellekteymiş gibi çalıştırılabilir (bunun en gerçek örneği container'lar içindeki uygulamalardır). bu belleklere sanal bellek denilir.

bu başlıkta sanal bellek; swap space'e eş anlamı olarak kullanılmıştır.

sanal bellek; RAM'in yetmediği durumlarda kullanılan diğer bellek alanına (burada HDD/SSD oluyor) verilen isimdir. burada; RAM __birincil bellek__, HDD/SSD ise __ikincil bellek__ olarak isimlendirilir.

Sanal belleğin HDD/SSD'deki karşılığı bir dosya da olabilir yada bir partition'da olabilir. Microsoft Windows'ta her zaman bu bir dosyadır: C:\pagefile.sys

"Takas" ibaresi şuradan gelir: OS'lar ikincil bellek alanından data okuyacağı zaman onu önce RAM'e (birincil belleğe) kopyalar. oradan okur. bu işlem __takas yapma (or swapping or paging in or faulting in)__ olarak adlandırılır. çok swapping yapmakta zaman kaybettirir. bu oranı OS'un iyi tutturması gerekir.

Bir programda üretilen/kullanılan adresler gerçek __fiziksel adresler__ değildir. Programda üretilen adreslere __"logical address (or mantıksal adres)"__ denir. logical adresler, işletim sistemi seviyesinde veya donanımsal katmanda gerçek fiziksel adreslere çevrilmektedir.

sanal bellek yapısı kullanan sistemlerde "logical address" yerine __virtual address__ ibaresi de kullanılabilmektedir.

# segmentation (or segmentasyon)

OS'lar çalışan programlara ihtiyaçları kadar bellek atar ve bu bellekleri ardarda sıralayarak koyar. daha sonra bir process kapandığında onun kullandığı bellek kısmı silinir, fakat memory optimize edilmez. dolayısı ile memory'de arada boşluklar kalır. OS eğer boşluktan daha düşük memory isteyen bir uygulama gelirse, boşalan yeri dolduruyor. fakat %90 boşluğun tümünü doldurmayacağı için her zaman boşluklar kalıyor.

örnek:
```

   ******************
   *    100 MB      *  (A process için ayrılmış bellek alanı)
   ******************
   *                *
   *                *  (B process için ayrılmış bellek alanı)
   *    300 MB      *
   *                *
   ******************
   *    100 MB      *  (C process için ayrılmış bellek alanı)
   ******************


   B işlemi kapansın.


   ******************
   *    100 MB      *  (A process için ayrılmış bellek alanı)
   ******************
   *                *
   *                *  boş alan
   *    300 MB      *
   *                *
   ******************
   *    100 MB      *  (C process için ayrılmış bellek alanı)
   ******************


   250 MB isteyen E işlemi gelsin.


   ******************
   *    100 MB      *  (A process için ayrılmış bellek alanı)
   ******************
   *    250 MB      *  (E process için ayrılmış bellek alanı)
   *                *
   ******************
   *    50 MB       *  boş alan
   ******************
   *    100 MB      *  (C process için ayrılmış bellek alanı)
   ******************
```

Yukarıda görüldüğü gibi boş alanlar kalabiliyor. bu kalan kalıntılara __harici hafıza kırıntılarıdır (or external fragments or harici parçalar)__ denir. buna çözüm olarak __sayfalama (or Paging)__ yaklaşımı kullanılıyor.

# paging (or sayfalama)

fiziksel bellek alanı eşit büyüklükteki parçalara bölünür. Bunların her birine __sayfa (or page)__ denir. Bu sayfalara karşılık gelen fiziksel bellek alanlarına ise __sayfa çerçevesi (or page frame)__ denir.

sayfa ve çerçeve terimlerinin farklı olmasının sebebi şudur: bazı durumlarda birden fazla page, fiziksel bellekteki aynı çerçeveye bakıyor olabilir. örnek durumlar:
- shared memory
- örneğin bir yazılımı 2 kere başlatalım. OS ve uygulama bunun tersini belirtmemişse, uygulamanın kendisi bellekte bir kere yer kaplar.
- common readed files
- bazen 1 page hiç pencereye bakmıyor bile olabiliyor: hiç initialize edilmemiş bir array olsun yada tümü null/0 olarak allocate edilmiş bir array olsun. bu gibi durumlarda OS bu frame'lere bayrak atıyor ve gerçek fiziksel bellekte karşılıklarını tutmuyor.

sadece "frame" terimi "page frame" yerine kullanılmamalı. bu yanlış. karışıklığa sebep oluyor. çünkü "frame" tek başına benzer anlamlar taşıyabiliyor.

Paging çözümünü, segmentasyona göre daha avantajlı fakat %100 verimli değil. 1 frame birden fazla uygulama tarafından kullanılamaz. bu sebeple en az PAGE_SIZE kadar process'lere alan tahsis edilir.

PAGE_SIZE=100 olsun. 1 process 210 MB isterse ona 100*3=300 MB'lik yer tahsis edilir. burada 90 MB boşuna harcanır. bu kalan kalıntılara __iç hafıza kırıntısı (or internal fragments)__ denir.

PAGE_SIZE küçüldükçe memory'deki kırıntılar azalır fakat her page mapping genişleyeceği için performansta farklı dezavantajlar yaşanacaktır. OS bu oranı iyi tutturmalıdır.

# haritalama (or mapping)

__page table__ OS'un kendi içinde tuttuğu bir tablodur. bu tablo ile __mapping__ yapılabilmesini sağlar. logical adresleri fiziksel adreslere çevirir. örnek bir tablo;

| Sanal bellek mi gerçek bellek mi? | Page/segment'in başladığı gerçek fiziksel taban adresi |
|-----------------------------------|--------------------------------------------------------|
| gerçek                            | 1000                                                   |
| gerçek                            | 2000                                                   |
| sanal                             | 4000                                                   |

# sayfa hatası (or page fault)

bu konu "Microsoft Windows 10 task manager işlem detayları sekmesinde olan bilgiler" altında anlatılıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Concrete class
kelime anlamı: beton, somut, belirgin, elle tutulur.

"new" anahtar kelimesi ile instance'ını yaratabildiğimiz her sınıfa, object orianted dünyasında concrete denir.

Örnek: Java'daki abstract veya interface'ler concrete olamazlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# CAPTCHA (or Completely Automated Public Turing test to tell Computers and Humans Apart)
puzzle yapma, matematik işlemi yapma, ekrandaki metni girme, sesi dinleyip metne çevirme gibi birçok çeşidi vardır.

# No CAPTCHA
risk analizinden geçmiş kullanıcı akışına verilen özel isim.

# Hcaptcha
privacy orianted bir CAPTCHA'dır. site sahiplerine her çözülen CAPTCHA için Ethereum üzerinden "Human Token" verilmektedir.

# reCAPTCHA
Google'ın sunduğu CAPTCHA hizmetinin özel ismidir.

v2 ve v3 gibi farklı sürümleri ve yetenekleri vardır.

Google reCAPTCHA risk analizinde, Canvas Rendering fingerprint, User-Agent, ekran çözünürlülüğünden de yararlanılıyor. Bazı kaynaklarda mouse movements pattern'ler ile yapılan testlerde hiçbir fark görülmediği yazıyor. Kaynak: (source-id: 64) title: "Screen Resolution and Mouse".

Google artık metin göstermiyor. Artık resim gösteriyor. bunları son kullanıcılara gruplandırtıyor (kategorize ettiriyor). bu şekilde kendi veritabanını ve yapay zekasını geliştiriyor.

Google OCR olsun, resim olsun şu şekilde çalışıyor: 10 kutu resim seçeneği varsa bunların 6'sı Google'ın ne gruba girdiğini %100 bildiği resim oluyor. Diğer 4 ü Google'ın ne anlama geldiğini bilmediği resim oluyor. Google bisikletleri seçin dediğinde, Google aslında sadece 6 tane emin olduğu üzerinden validasyon(risk analizi) yapıyor. diğer 4'ü son kullanıcının insiatifine kalmış. bu bilgiler sunucuda tutuluyor. binlerce son kullanıcı bu 4 tane belirsiz resimlerde bisiklet olduğunu seçmiş ise, Google bu seçilen resimlerin bisiklet grubuna girdiğine karar veriyor. işte böyle böyle Google kendi veritabanını ve yapay zekasını geliştiriyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# NTFS long file names problem

NTFS, 255 Unicode karakter kabul ediyor. fakat eski dosya sistemleri etmiyor. bunun önüne geçmek için güncel Microsoft Windows işletim sistemleri her dosya için 2 farklı dosya isim tutuyor. örneğin;

> Microsoft.txt
> MICROS~1.TXT

eski dosya sistemleri hem daha az karakterleri olmak zorunda, hemde büyük harf olmak zorunda gibi farklı kombinasyon kuralları içerebiliyor...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# SLA (Service Level Agreement)
türkçe karşılığı: Hizmet Düzeyi Sözleşmesi (or Hizmet Seviyesi Sözleşmesi)

hizmetin son kullanıcı ile arasında sözleşmedir. SLA müşterinin ne alacağını belirtir ve servis sağlayıcısından ne beklendiğini açıklığa kavuşturur.

# Day-N
Day-N genel bir terimdir. Bir fazı belirtmez.

## Day(-1) (or Day-Minus-One)
__Day-1__ ile karışmaması için -1 paranteze alınır.

Day(-1)'in sonunda fiziksel cihazların onboarding'i başlamış olur. Bu süreçten önceki sürece Day(-1) denir.

## Day-0 (or Day-Zero)
Day-0'ın sonunda fiziksel cihazların onboarding'i bitmiş olur.

## Day-1 (or Day-One)
İstenen yazılımın kurulumu aşamasıdır.

## Day-2 (or Day-Two)
Kurulum yapıldıktan sonraki günleri kapsamaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Teknik destek

- Level 0 support (or level zero support): FAQs, documentations, manuals...

- level 1 support (or first level support): yardım alınan kişinin de teknik bilgisi yoktur. sadece business kurallarını iyi bilir.

- level 2 support (or second level support): yardım alınan kişinin veritabanı gibi yerler manuel sorgu atması gibi IT bilgisi vardır.

- level 3 support (or third level support): yardım alınan kişi direk yazılım geliştiricisidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# annotation vs XML (or any other format like JSON)

advantages of annotations:

- XML değiştirmek/okumak Java koduna göre daha zordur (JPA da her entity'yi XML'de tanımladığınızı düşünün)

- bir sınıf hakkındaki tüm annotation'lar (meta bilgiler) sınıfın içindedir.

- sınıf ismi değiştiğinde XML değiştirmek gerekmez

Advantages of XML files:

- annotation kullanılmışsa; bazen hangi sınıfın görevini bilmiyorsak; onu tek tek dosyalarda arama yaparak annotation'larını bulmamız gerekir. bazen IDE'ler kolaylık sağlayabilir bu konuda. fakat XML'lerde tüm sistemi birkaç dosyada görebiliriz (bazen geçerli olan bir avantaj/durum).

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# data binding

bir değeri bir objeye atama işleminin adı binding'tir. iki türlü binding vardır:

- one way binding

  bir değeri JS tarafında modelde tutalım. bu değeri ekranda bir input'a yazdırdık. kullanıcı bu input'u değiştirdiğinde otomatik olarak model'deki değer değişmez. aradaki senkronizasyonu programcı yapmak zorunda kalır. input değiştiğinde, tetiklenecek change metotunda programcı model'i değiştirmelidir.

- two way binding

  burada senkronizasyonu MVC kütüphanesini kendisi yapar. örneğin; Angular.JS'te two way binding vardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Eclipse IDE

Eclipse altyapısı ile birçok OSGI tabanlı uygulamalar oluşturulabilir. Bu uygulamalara "Eclipse application (or Eclipse software)" deniliyor. Bu uygulamalardan bir tanesi Eclipse IDE ‘dir.

Eclipse IDE kendi içinde  versiyonlar ile dağıtılıyor: Java, JavaEE, Android gibi. Bunlara "paket veya product" adı veriliyor.

Her paket kendi içerisinde farklı plugin'lerden oluşuyor.

Birçok plugin toplanılarak, "feature" olarak gruplandırılıyor. Son kullanıcı feature silip ekleyebiliyor. Feature başka özelliklere depend edebiliyor.

extension çok teknik seviyede kullanılan bir terim. extension; Eclipse eklenti geliştiricileri tarafından sunulan her objenin/kaynağın (functionality, help content gibi...) ismidir.

- "Eclipse java" paketinde yüklü gelen özellikler:

  - "Maven Integration for Eclipse" (or M2Eclipse or m2e)

  - "Mylyn" - Jira gibi sistemlerden kullanıcıya task'ı gösteren eklenti.

  - "Eclipse Web Tools Platform" paketinden sadece XML editor ve tool'ları

  - "Egit" - Git integration. Egit based on Jgit (Git Java implementation)

  - "code recommenders"

  - Gradle (feature ismi bilinmiyor)

- "Eclipse JavaEE" içerisinde gelenler:

  - "Eclipse java" versiyonu ile gelenler + aşağıdaki liste:

  - "Data Tools Platform" - Database management plugin.

  - "Java EE Developer Tools"

  - "Eclipse Web Tools Platform" (or WTP)

  - Plug-in Development Environment: sadece Eclipse paketi geliştirmek için gerekli altyapı.


Marketten indirebilenler:

WindowBuilder: GUI creator. pro version is exist.

# MyEclipse
Eclipse IDE'den türetilmiş bir IDE.

# project facet
facet:
- 1. façeta (elmasın yontulmuş yüzlerinden her biri).
- 2. taraf (bir konunun ya da herhangi bir şeyin çeşitli yüzler)

facet Eclipse'in sunduğu bir özelliktir. projenin property'lerini açtığımızda, birçok teknoloji ismi listelenir. hangilerini seçersek  proje için o teknolojilere özgü özellikler otomatik devreye girmektedir.

Örnek: ear facet'i devreye alındığında projenin classpath'ine otomatik olarak deployment descriptor eklenmektedir.

Bazı facet'ler birbirine depend ederken, bazıları ise aynı anda aktif edilememektedir.

IntelliJ'de "Add framework support" menüsünde facet ekleme vardır. aslında bakıldığında birçok IDE'de facet terimi olarak geçmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# nohup

POSIX için komut satırı yazılımı. aldığı parametreleri komut olarak arkaplanda çalıştırtır. böylece komut satırı kapatıldığında uygulama çalışmaya devam eder. nohup'a alternatif yazılımlar da mevcuttur. Microsoft Windows için "start" komutu benzer görevi görür.

# nohup vs disown vs &

  Aşağıdaki 3 örnekte 'commandX' yeni process'e atanır ve shell yeni komut alacak şekilde kendini ayarlar.

  aşağıdaki her işlemde de commandX'e sterr, stdin gibi stream'ler bağlanır.

| command usage                    | standart input (stdin)                      | standard output (stdout) and standard error (stderr)                        | job listesinde tutulur     | SIGHUP sinyalini                |
|----------------------------------|---------------------------------------------|-----------------------------------------------------------------------------|----------------------------|---------------------------------|
| commandX param1 param2 &         | program buraya data yollarsa hata alacaktır | hala terminal'e bağlıdır, bu sebeple durduk yere ekranda output görebiliriz | process hafızada tutuluyor | process'e yollar                |
| commandX param1 param2 \| disown | program buraya data yollarsa hata alacaktır | program buraya data yollarsa hata alacaktır                                 | process hafızadan silinir  | process'e yollanmasını engeller |
| nohup commandX param1 param2     | kapatır ve process'e EOF karakteri gider    | redirects to nohup.out                                                      | process hafızada tutuluyor | process'e yollanmasını engeller |

  Bazen programlar disown yada nohup'la başlatmamıza rağmen shell kapandığında process'te kapanır. bunun sebepleri:

  - shell kapatılınca her process'e SIGHUP sinyali yollanır. bu sinyal sonrası process kapanmak zorunda değildir. process istediği gibi sinyali handle edebilir. çoğu zaman programlar kapanmayı tercih eder.

  - shell kapandığında stin, stdout gibi stream'ler null olduğundan NullPointer gibi hatalar alan program hata fırlatarak beklenmedik durum gereği kapanır. bu sebeple bazıları stout'u > file.txt gibi bir dosyaya yönlendirmemizi önerir.

# job control
POSIX'lerde shell'ler arka plana attıkları yazılımları job listesinde tutar. bu sisteme "job control" denir.

terminal'in her başlattığı process 'child process'tir.

job control, temel olarak "process group" teknolojisini kullanarak çalışır.

"jobs" komutu o anda terminal'de çalışan arkaplan uygulamalarını print eder.

# POSIX sinyalleri

Özel bir IPC event'idir. POSIX standartları altında tanımlanmıştır. çalışan process'lere veya bir thread'e özel birçok çeşit sinyal gönderilebilir. bu sinyaller 64 tanedir. C'de signal.h içerisinde de tanımlıdırlar. Bazı sinyaller:

- SIGHUP - 1 numaralı sinyal - "signal hang up" yada "HUP" da deniliyor. terminal'de çalışan process, terminal kapatıldığında bu sinyal ilgili process'e yollanır.

- SIGINT - 2 numaralı sinyal - komut satırında Ctrl+C tuşu aynı görevi işler. Bu yazılımcı tarafından handle edildiğinde gözardı edilebilir. yani program kapanmak zorunda değil.

- SIGKILL - 9 numaralı sinyal - Uygulamayı yazılımcı handle edemeden kapatır. Core dump dosyasını saklar.

- SIGSTOP - 18 numaralı sinyal - komut satırında Ctrl+Z tuşu aynı görevi işler - uygulamayı pause eder.

- SIGCONT - 19 numaralı sinyal - uygulamayı pause durumundan resume ile devam ettirir.

Bazı sinyaller piyasada kullanılmakta fakat POSIX standartlarında belirtilmemiştir.

Diğer sinyaller:

kaynak: (source-id: 65)

```c
#define  SIGHUP 1  /* hangup */
#define  SIGINT  2  /* interrupt */
#define  SIGQUIT  3  /* quit */
#define  SIGILL  4  /* illegal instruction (not reset when caught) */
#ifndef _POSIX_SOURCE
#define  SIGTRAP  5  /* trace trap (not reset when caught) */
#endif
#define  SIGABRT  6  /* abort() */
#ifndef _POSIX_SOURCE
#define  SIGIOT  SIGABRT  /* compatibility */
#define  SIGEMT  7  /* EMT instruction */
#endif
#define  SIGFPE  8  /* floating point exception */
#define  SIGKILL  9  /* kill (cannot be caught or ignored) */
#ifndef _POSIX_SOURCE
#define  SIGBUS  10  /* bus error */
#endif
#define  SIGSEGV  11  /* segmentation violation */
#ifndef _POSIX_SOURCE
#define  SIGSYS  12  /* bad argument to system call */
#endif
#define  SIGPIPE  13  /* write on a pipe with no one to read it */
#define  SIGALRM  14  /* alarm clock */
#define  SIGTERM  15  /* software termination signal from kill */
#ifndef _POSIX_SOURCE
#define  SIGURG  16  /* urgent condition on IO channel */
#endif
#define  SIGSTOP  17  /* sendable stop signal not from tty */
#define  SIGTSTP  18  /* stop signal from tty */
#define  SIGCONT  19  /* continue a stopped process */
#define  SIGCHLD  20  /* to parent on child stop or exit */
#define  SIGTTIN  21  /* to readers pgrp upon background tty read */
#define  SIGTTOU  22  /* like TTIN for output if (tp->t_local&LTOSTOP) */
#ifndef _POSIX_SOURCE
#define  SIGIO  23  /* input/output possible signal */
#define  SIGXCPU  24  /* exceeded CPU time limit */
#define  SIGXFSZ  25  /* exceeded file size limit */
#define  SIGVTALRM 26  /* virtual time alarm */
#define  SIGPROF  27  /* profiling time alarm */
#define SIGWINCH 28  /* window size changes */
#define SIGINFO  29  /* information request */
#endif
#define SIGUSR1 30  /* user defined signal 1 */
#define SIGUSR2 31  /* user defined signal 2 */
```

Yazılımcı kod içerisinde bir listener aracılığı ile bu sinyalleri handle etmesi gerekir. C üzerinden örneklersek:

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <signal.h>

void sighandler(int);

int main () {

   // signal.h'taki bir fonksiyondur.
   // ilk parametresi: hangi sinyal için handle çalıştırılsın
   // ikinci parametresi: handler fonksiyonumuz
   signal(SIGINT, sighandler);

   while(1) {
      printf("sleep for a second...\n");
      sleep(1);
   }
   return(0);
}

void sighandler(int signum) {
   printf("Caught signal %d, coming out...\n", signum);
   exit(1);
}
```

Sinyaller process'imize iletildiğinde, process thread'i beklemeye alınır ve handler'ımız execute edilir. handler bittiğinde main process devam eder.

bir handler çalışırken, başka bir sinyal daha gelirse, önce ilk handler tamamlanır, sonra diğer handler için tekrar aynı handler devreye girer. 2inci handler bittiğinde main fonksiyonu çalışmaya devam eder. aşağıdaki kod ile bunun testini output kısmını inceleyerek görebiliriz. CTRL+C ile çalışan uygulamaya sinyal gönderildiğinde, uygulamanın çıktısı değişmektedir.

```c
#include <stdio.h>
#include <unistd.h>
#include <stdlib.h>
#include <signal.h>

void sighandler(int);

int main () {
   signal(SIGINT, sighandler);
   int mainLoopId = 0;
   while(1) {
      mainLoopId++;
      printf("main sleep -- mainLoopId=%d\n", mainLoopId);
      sleep(1);
   }
   return(0);
}

int signalHandlerId = 0;

void sighandler(int signum) {
   signalHandlerId++;
   int loopId = 0;
   while(loopId<5) {
      printf("handler sleep -- signalHandlerId=%d loop=%d \n", signalHandlerId, loopId);
      sleep(1);
      loopId++;
   }
}
```

## Kimler ve hangi durumlarda sinyal alınıp/yollanabilir

- İşletim sitemi arkaplanda bu sinyalleri yazılımlara yollayabilir. örnek:
  - son kullanıcı işletim sistemini kapatmak için event açsın. bu durumda işletim sistemi uygulamaların tümüne kapanmaları için özel bir sinyal gönderir.

- Yeterli yetkisi olan son kullanıcı bir process'e sinyaller gönderebilir. örnek:
  - kill, signal komut satırı uygulamaları ile process-ID'sini parametre olarak geçtiğimiz process'e, istediğimiz sinyali gönderebiliriz.

- yazılımcı kod içerisinden sinyal yollayabilir. örnek:
  - C'de; signal.h içindeki __int raise(int sig)__ fonksiyonu ile kendi işlemimize sinyal gönderebiliriz.
  - C'de; __int kill( pid_t pid, int sig )__ ile diğer process'lere sinyal gönderebiliriz.

# exit status (or exit code)

program kapandığında bir sinyal döner. bu sinyale verilen addır. bir sayıdır.

Java'da; System.exit(int status) metotu ile statü dönülerek işlem sonlandırılır.

bir shell fonksiyonundan dönen return değer ile, shell üzerinden çalıştırılan bir programın exit code'u aynı statüdedir.

fakat çoğu shell implementasyonu bir komut işlettiğinde, ilgili komut ne döndürürse onun 255 ile modunu alıp döndürüyor.

- 0 başarılı bittiği anlamına gelir.

- pozitif sayılar daha çok yazılımcı tarafından yakalanmış/yönetilmiş hatalardır.

- negatif sayılar yazılımcının yakalamadığı/yakalayamadığı durumlarda döndürülür.

Bazı exit statüler standarttır. örneğin bazı status'ler POSIX standartlarında belirtilmiştir. örnek:

code - açıklama

1 - programatic error example: 1/0 divide zero

2 - binary file format example: missing keyword. for example calling a function which is not on classpath

126 - Command invoked cannot execute. example: file is not executable

127 - command not found to execute

128 - exit code integer değilse o zaman bu hatayı verir.

128+x - "kill -9 $PPID" komutu çalıştırıldığında 9 sinyali programa yollanır. Eğer program bunu yakalayamazsa, program 128+9=137 fırlatır.

130 - CTRL+C process'e 2 sinyalini yollar. Bu sebeple 128+2=130'dur.

Yukarıdaki değerler yazılımcı tarafından da fırlatılabilir. fakat bu hiç önerilmez. best practice değildir.

Çoğu child process açan API (örnek C language API) child process kill olunca nax 128'lik bir değer dönecek şekilde yazılmışlardır. Dolayısı ile 128 üstü değerler döndürebiliriz fakat bunu kimin kullanacağı önemlidir. Örneğin sadece bir shell fonksiyonu çalıştırıyor isek döndürebiliriz. Zira bunu izden başka kimse shell dışında çağıramayacaktır. Fakat bir program yazdıysak ve bunu farklı birileri firejail gibi wrapper komutlardan çağırmak isteyebilirse 128 üstünü döndürmek iyi olmayabilir.

C ve C++ dillerinde standart kütüphanelerinin (sysexits.h) içinde aşağıdaki değerler vardır. fakat bunlar işletim sisteminin yada başka programlama dillerinin ve hatta diğer c kütüphanelerinin resmi olarak tanıdığı standartlar değildirler.

opensource.apple.com/source/Libc/Libc-320/include/sysexits.h dosyasının içeriği aşağıdadır:

```c++
#ifndef  _SYSEXITS_H_
#define  _SYSEXITS_H_

/*
 *  SYSEXITS.H -- Exit status codes for system programs.
 *
 *  This include file attempts to categorize possible error
 *  exit statuses for system programs, notably delivermail
 *  and the Berkeley network.
 *
 *  Error numbers begin at EX__BASE to reduce the possibility of
 *  clashing with other exit statuses that random programs may
 *  already return.  The meaning of the codes is approximately
 *  as follows:
 *
 *  EX_USAGE -- The command was used incorrectly, e.g., with
 *    the wrong number of arguments, a bad flag, a bad
 *    syntax in a parameter, or whatever.
 *  EX_DATAERR -- The input data was incorrect in some way.
 *    This should only be used for user's data & not
 *    system files.
 *  EX_NOINPUT -- An input file (not a system file) did not
 *    exist or was not readable.  This could also include
 *    errors like "No message" to a mailer (if it cared
 *    to catch it).
 *  EX_NOUSER -- The user specified did not exist.  This might
 *    be used for mail addresses or remote logins.
 *  EX_NOHOST -- The host specified did not exist.  This is used
 *    in mail addresses or network requests.
 *  EX_UNAVAILABLE -- A service is unavailable.  This can occur
 *    if a support program or file does not exist.  This
 *    can also be used as a catchall message when something
 *    you wanted to do doesn't work, but you don't know
 *    why.
 *  EX_SOFTWARE -- An internal software error has been detected.
 *    This should be limited to non-operating system related
 *    errors as possible.
 *  EX_OSERR -- An operating system error has been detected.
 *    This is intended to be used for such things as "cannot
 *    fork", "cannot create pipe", or the like.  It includes
 *    things like getuid returning a user that does not
 *    exist in the passwd file.
 *  EX_OSFILE -- Some system file (e.g., /etc/passwd, /etc/utmp,
 *    etc.) does not exist, cannot be opened, or has some
 *    sort of error (e.g., syntax error).
 *  EX_CANTCREAT -- A (user specified) output file cannot be
 *    created.
 *  EX_IOERR -- An error occurred while doing I/O on some file.
 *  EX_TEMPFAIL -- temporary failure, indicating something that
 *    is not really an error.  In sendmail, this means
 *    that a mailer (e.g.) could not create a connection,
 *    and the request should be reattempted later.
 *  EX_PROTOCOL -- the remote system returned something that
 *    was "not possible" during a protocol exchange.
 *  EX_NOPERM -- You did not have sufficient permission to
 *    perform the operation.  This is not intended for
 *    file system problems, which should use NOINPUT or
 *    CANTCREAT, but rather for higher level permissions.
 */

#define EX_OK    0  /* successful termination */

#define EX__BASE  64  /* base value for error messages */

#define EX_USAGE  64  /* command line usage error */
#define EX_DATAERR  65  /* data format error */
#define EX_NOINPUT  66  /* cannot open input */
#define EX_NOUSER  67  /* addressee unknown */
#define EX_NOHOST  68  /* host name unknown */
#define EX_UNAVAILABLE  69  /* service unavailable */
#define EX_SOFTWARE  70  /* internal software error */
#define EX_OSERR  71  /* system error (e.g., can't fork) */
#define EX_OSFILE  72  /* critical OS file missing */
#define EX_CANTCREAT  73  /* can't create (user) output file */
#define EX_IOERR  74  /* input/output error */
#define EX_TEMPFAIL  75  /* temp failure; user is invited to retry */
#define EX_PROTOCOL  76  /* remote error in protocol */
#define EX_NOPERM  77  /* permission denied */
#define EX_CONFIG  78  /* configuration error */

#define EX__MAX  78  /* maximum listed value */

#endif /* !_SYSEXITS_H_ */
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# command line argument passing types:

- POSIX like
  > myapp -myParam1 myValue1

- GNU like
  > myapp --my-param1 --my-param2=myValue2

- Java like

  Bu genelde D ve X olarak parametreleri gruplandırmak için yapılmaktadır.
  > myapp -Dmy.param1=myValue1 -XmyParam1myValue1

- (no name for this)

  Java-like gibi property1 ve property2 altında gruplayabilmek için kullanılır.
  > myapp --property1 myparam1=myValue1 myparam2=myValue2 --property2 myparam3=myValue3


- (no name for this)

  tar -x -z -f myfile.tar.gz ile aynı olabilir. bu uygulamadan uygulama değişir.
  > tar -xzf myfile.tar.gz

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

Aşağıdaki tüm bilgiler için kaynak: https://www.martinfowler.com/articles/mocksArentStubs.html title:"The Difference Between Mocks and Stubs".

# Test Double
ismi bir filmden esinlenerek koyulmuş. bu sebeple isminden bir anlam çıkarılmamalı.

stub, fake, dummy, mock gibi test için kullanılan bilgilere verilen genel bir terimdir.

# stub metot
bir metot daha yazılmamış ise (implemente edilmemiş ise) fakat başka yerlerden çağrılması gerekiyor ise metotun imzası hazırlanır fakat içinden null yada geçici bir değer dönülür. bu şekilde bu metotu kullanan diğer kodların geliştirmesi durmamış olur. böyle durumlarda bu metota stub adı verilir.

# stub class
içinde stub metotları bulunan sınıftır.

# dummy data
genelde sadece parametre/argüman yollamak(doldurmak) için kullanılan bilgilerdir. bu parametrelerin ne olduğu önemsizdir. yani; çağırdığımız metot bu parametrelere göre bize döneceği değer asıl akışımızı(metodu çağıran akışı) etkilemesi gerekir. ancak bu durumda bu bilgilere dummy data denir.

# mock data
bir yere yollandığında cevap olarak özel bir beklentimizin olduğu bilgilerdir.

# mock method
çağrıldığında hangi parametrelere göre neyi döneceğimizi önceden tanımladığımız metottur.

# fake data
gerçek birer implementasyondur. production ortamına çok yakın (simülasyon gibi) yaptığımız durumlar için bu terim kullanılır. Buna en iyi örnek Oracle DB yerine, test sürecinde in-memory-db kullanmamız verilebilir.

# spy (or plural:spies)
Stub ile aynı anlama gelir. Fakat spy ek olarak metodun kaç kere tetiklendiğini de kaydeder. Bu biliyi testlerde "assert" kontrollerimizde kullanılırız. örneğin; ödeme sistemi test ediyoruz. ödeme metodunun sadece 1 kere çağrıldığından emin olmak isteyebiliriz gibi..

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Büyüklük/ölçü birimleri

# px (or dot or pixel or gözek or picture element or eng:pel)
Görüntüdeki en ufak birimdir. Cihazdan cihaza boyutu değişir.

# sub-pixel
bu fiziksel bir kavramdır. tek bir pixeli oluşturmak için fiziksel olarak ihtiyaç duyulan birimlerdir. fakat yine en ufak görüntü birimi pixel kabul edilir. çünkü tek bir görüntü noktası (tek renk) olan en ufak birim pixel'dir.

# megapixel
1 milyon pixel'e sahip görüntüye denir. en*boy sonucu 1 milyon olan görüntüye denir. en boy oranı hakkında bilgi vermez. çubuk gibi de olabilir.

# Frame Rate
birimi __FPS (or frame per second)__'dir. her saniye kaç görüntünün yansıtıldığı sayısıdır.

__Refresh Rate__ terimi __FPS__ ile aynı anlamdadır. Sadece birimleri farklıdır. FPS, belli bir sürede kaç "görüntü" alınacağını belirtir. __Refresh Rate__ in birimi ise __Hz (or hertz)__'dir ve fizik kurallarına göre neyin refresh edileceğini spesifik belirtmemiz gerekir. Şu anda monitör/ekran dünyasından bahsettiğimiz için, __FPS__, __Refresh rate__ ile aynı kapıya çıkmaktadır. Fakat piyasada genelde __FPS__ (fiziksel/donanımsal) monitör'ün kaç ekran gösterebildiğini temsil etmek için kullanılırken, __Refresh Rate__ yazılımın (örnek 3D oyunun) desteklediği saniye başına ekran görüntüsünü temsil etmek için kullanılır.

# notasyonlar
medya dosyalarında en ve boy oranları çoğu zaman orantılı olacağından notasyonlarda buna göre daha kısa yazılmaktadır. örneğin;

- __1080p60__ --> 1920x1080 pixels, 60 FPS anlamına gelir. 1920 notasyonda yazılmaz. çünkü piyasadaki çoğu cihazlar bu oranda medya dosyası hazırlamaktadır. Youtube gibi son kullanıcıya görüntü veren sitelerde bu tarz kısaltmalar kullanılırken daha akademik/endüstri standartlarında bu şekilde kısa kullanım önerilmez.

- __720p60__ --> 1280x720 pixels, 60 FPS anlamına gelir.

- __720p__ --> 1280x720 pixels anlamına gelir. FPS değerinden bahsedilmez.

# full HD
| name                            | resolution |
|---------------------------------|------------|
| Standard Definition / SD / 480p | 854x480    |
| 540p / qHD                      | 960×540    |
| 720p / HD / High Definition     | 1280×720   |
| 1080p / Full HD / FHD           | 1920x1080  |
| 2K                              | 2048×1080  |
| 1440p / QHD / QuadHD / WQHD     | 2560×1440  |
| 2160p / UHD / Ultra HD          | 3840×2160  |
| 4K                              | 4096×2160  |
| 5K                              | 5120×2880  |
| 8K / 8K UHD                     | 7680×4320  |

# Inch (or inç)
symbol: in

symbol: "

2,54 cm'e denk gelmektedir.

# PPI (or pixel per inch)
DPI ile benzer bir kavramdır.

# DPI (or dot per inch)
PPI'a benzerdir. PPI pixel bazında olduğu için her grafik çıktısı bu terimi kullanamaz. örneğin; PPI'ı sadece monitörler/ekranlar kullanır. printer çıktısında PPI'dan bahsedilemez. bu sebeple DPI terimine ihtiyaç vardır.

örneğin bir görüntü oluşturduğumuzda (örneğin; Photoshop ile) birimler DPI olabilir. DPI ile oluşturulan görüntüsü o anda uygun olan pixel boyutuna çevrilecektir. fakat o cihazda farklı görünebilecektir. print ettiğimizde doğru görünebilecektir. örnek belki Photoshop, 1 dot = 1 pixel yapar, yada 1 dot 4 pixel yaparak ekranda gösteriyordur. işte burada o cihazın/yazılımın "DP" değeri önemlidir.

# DP (or DIP or density (yoğunluk) independent pixels)
Standartlara göre değişen bir birimdir. örneğin; Android'lerde 160 DPI bir ekranda; 1DP = 1 px'tir. 160 sayısı burada Android standartıdır. Fakat 320 DPI bir ekranda ise; 1 DP, 2 pixele eşit olacaktır.

# International System of Units (or SI)
Kısaltma Fransızca'daki "Système International d'unités" 'den gelir.


| Prefix name | Prefix symbol (case sensitive) | value  |
|-------------|--------------------------------|--------|
| quetta      | Q                              | 10^30  |
| ronna       | R                              | 10^27  |
| yotta       | Y                              | 10^24  |
| zetta       | Z                              | 1021   |
| exa         | E                              | 10^18  |
| peta        | P                              | 10^15  |
| tera        | T                              | 10^12  |
| giga        | G                              | 10^9   |
| mega        | M                              | 10^6   |
| kilo        | k                              | 10^3   |
| hecto       | h                              | 10^2   |
| deca        | da                             | 10^1   |
|             |                                | 10^0   |
| deci        | d                              | 10^−1  |
| centi       | c                              | 10^−2  |
| milli       | m                              | 10^−3  |
| micro       | μ                              | 10^−6  |
| nano        | n                              | 10^−9  |
| pico        | p                              | 10^−12 |
| femto       | f                              | 10^−15 |
| atto        | a                              | 10^−18 |
| zepto       | z                              | 10^−21 |
| yocto       | y                              | 10^−24 |
| ronto       | r                              | 10^−27 |
| quecto      | q                              | 10^−30 |

Birçok teknik farklı kavramlar için ek birimler türetilmiştir. örnek:

Frekans birimi; hertz (or abb:Hz). s^−1 (saniyede salınım)

# uzunluk tanımı
Uzunluk hesaplamalarında metre referans olarak alınmaktadır.

1 metre, ışığın boşlukta 1/299.792.458 saniyede aldığı yol olarak tanımlanmıştır.

# ağırlık tanımı
ağırlık hesaplamalarında gram referans olarak alınmaktadır.

Uluslararası kuruluşlarca Paris'teki Uluslararası Ağırlıklar ve Ölçüler Bürosu'nda bulunan İridyum platinden yapılmış silindir şeklindeki cisim 1kg olarak kabul edilir.

Paris'teki referansa en yakın tanım: gram, +4°C de 1 cm3 saf suyun kütlesidir.

Ağırlığı referans almak için piyasada şu yol izlenmektedir: dünyanın en yuvarlak nesnesi olan ve 2,15 x 10^25 adet silikon 28 atomuna sahip mükemmel küre şeklindeki bir cisim çok yüksek maliyetlere üretildi ve kopyalandı. kopyası dünyanın her tarafına dağıtıldı. herkes bu referansa göre ağırlığı tanımlıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# short scale (or kısa ölçek) vs long scale (or uzun ölçek)
birbirine alternatif 2 farklı ölçek isimlendirme sistemidir.

# karşılaştırma tablosu
| value | short scale (Turkish) | long scale (Turkish) | long scale (Alternative) (Turkish) | short scale (English) | long scale (English) | long scale (Alternative) (English) |
|-------|-----------------------|----------------------|------------------------------------|-----------------------|----------------------|------------------------------------|
| 10^6  | milyon                | milyon               |                                    | million               | million              |                                    |
| 10^9  | bilyon                | bin milyon           | milyar                             | billion               | thousand million     | milliard                           |
| 10^12 | trilyon               | bilyon               |                                    | trillion              | billion              |                                    |
| 10^15 | katrilyon             | bin bilyon           | bilyar                             | quadrillion           | thousand billion     | billiard                           |
| 10^18 | kentilyon             | trilyon              |                                    | quintillion           | trillion             |                                    |
| 10^21 | sekstilyon            | bin trilyon          | trilyar                            | sextillion            | thousand trillion    | trilliard                          |
| 10^24 | septilyon             | katrilyon            |                                    | septillion            | quadrillion          |                                    |
| 10^27 | oktilyon              | bin katrilyon        | katrilyar                          | octillion             | thousand quadrillion | quadrilliard                       |
| 10^30 | nonilyon              | kentilyon            |                                    | nonillion             | quintillion          |                                    |

For all numbers see: [file:"long_and_short_scale.md"](./long_and_short_scale.md)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# International System of Quantities (or ISQ)
Every abbreviation in this title is case sensitive.

ISO 80000 (or IEC 80000) dökümanında publish edilen sistemdir. __International Electrotechnical Commission (or IEC)__ tarafından yayımlanmıştır.

- __SI__ prefix'ine göre: 1 kB = 1000 byte
- __ISQ__ sistemine göre: 1 kiB = 1024 byte

__ISQ__'daki bit ve binary için prefix'ler:

| prefix   | abbreviation | value |
|----------|--------------|-------|
| Kibi     | Ki           | 2^10  |
| Mebi     | Mi           | 2^20  |
| Gibi     | Gi           | 2^30  |
| Tebi     | Ti           | 2^40  |
| Pebi     | Pi           | 2^50  |
| Exbi     | Ei           | 2^60  |
| Zebi     | Zi           | 2^70  |
| Yobi     | Yi           | 2^80  |

Örnek kullanım: kibibyte (KiB), mebibyte (MiB), gibibyte (GiB), kibibit, mebibit, gibibit...

"kibi", "gibi" prefix'leri aslında SI prefix'lerindeki prefix'lerden türetilmiş. SI'da 10'luk taban kullanılırken, ISQ'da 2'lik taban kullanıldığı için, "binary" kelimesinin kısaltması olan "bi", SI prefix'inin sağına eklenmiştir. Yani "kilo-binary", "kibi" olarak kısaltılmıştır. "bi" kısmı yazıldığı gibi okunmaktadır. kaynak: https://physics.nist.gov/cuu/Units/binary.html

Not: __SI__ ve __ISQ__ sistemindeki Kilo'ya tekabül eden "k", bazı kaynaklarda büyük veya küçük kullanılıyor. oysa "k" karakterinin SI'ın sisteminde küçük harf olması belirtilmiş. Dolayısı ile, piyasadaki bu kullanım yanlış, fakat bir karmaşaya sebep olmuyor. Fakat "y" karakterinin büyük veya küçük yazılması, "yocto" veya "yotta" ikilemine sokacaktır.

# kb vs kB
Every abbreviation in this title is case sensitive.

Bu konu ISQ veya SI standartlarından bağımsızdır.

1 kB = kilo byte

1 kb = kilo bits

Her merka ve her markanın farklı yıllarda çıkardığı ürünlerin dökümantasyonlarında, bit veya byte cinsinden bilgilendirmeler mevcut. kaynak: https://support.apple.com/en-us/HT201402

# örnekler
- 1 KiloByte = 1000*8 bit
- 1 KiloBit  = 1000*1 bit
- 1 KikiByte = 1024*8 bit
- 1 KibiBit  = 1024*8 bit

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# process states

- Created: yeni oluşturulmuş, fakat process hala tam olarak RAM'e alınmamıştır.

- Ready: CPU ya girmek için bekliyor.

- Running: CPUda işlem görüyor. Kernel mode ve User mode olarak çalışırlar. "User mode" kernel instructions'ları çağıramaz.

- Blocked: CPU ya girsede bir işe yaramayacak durumdadır, çünkü dış donanımlardan (kaynaklardan) cevap bekleniyordur.

- Terminated: işi bitmiş process. parent process "exit status"'u okumadığı için henüz , process bu statüde bekler. bu şekilde çok process olabileceğinden bunlara "zombie process" denir.

işletim sistemlerinin ek olarak birçok farklı statüleri olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# concurrent (or eş zamanlı) vs paralel
bu iki kavram sürekli birbirinin aynısı gibi bilinir. fakat ufak bir fark var: paralel çalışan işlemler, birden fazla CPU ile olmalıdır. yani 2 paralel thread var ise, en az 2 CPU olmalıdır. 2 işlem ancak bu şekilde paralel yürütülebilir. ancak concurrent işlemler; paralel olmak zorunda değillerdir. 2 farklı işlem birbirinden bağımsız yürür. aynı CPU'ya ikişer saniye aralıklarla girerlerse yine concurrent çalıştıkları anlamına gelir. kısaca; concurrent, birden fazla birbirinden bağımsız işlem olduğu; paralel ile zamanlama açısından ikisinin aynı anda işletileceğini belirtir.

Örneğin; OS'ların sunduğu "multitasking", 1 CPU'lu sstemde de çalışabiliyor. Bu demektir ki multitasking, concurrent mantığındadır. Fakat 2 CPU'lu makinada da çalışabildiğinden aynı zamanda paralel çalışma yeteneğine de sahiptir.

"__simultaneous (or tr:simultane)__" terimi "anında" anlamına geliyor. örneğin "anında tercüme". Fakat farklı kod thread'leri simultaneous olarak çalışıyor denildiğinde paralel ile aynı anlama çıkmaktadır. source: https://docs.oracle.com/cd/E19455-01/806-5257/6je9h032b/index.html Bu linkte aşağıdaki yazmaktadır:

- Parallelism: A condition that arises when at least two threads are executing simultaneously.
- Concurrency: A condition that exists when at least two threads are making progress. A more generalized form of parallelism that can include time-slicing as a form of virtual parallelism.

# distributed (or dağıtık) vs paralel
distributed işlemin birbirinden bağımsız ortamlarda yapıldığını temsil etmek için kullanılır. Oysa paralel işlemin aynı anda yapılıp yapılıp yapılmadığıdır. İkisi çok farklı terimlerdir. Bir işlem, sadece distributed olabilir veya paralel olabilir veya hem paralel hem distributed olabilir.

# işlemci sayısı ve thread sayısı ilişkisi
- gerçekte bir bilgisayar tek thread çalıştırabilir. bir bilgisayarın CPU'su 2 işlemcili ise 2 thread aynı anda çalıştırabilir. yani 2 işlemi paralel yapabilir.

- standart bir PC'de işletim sistemleri binlerce thread aynı anda açık tutar. bu thread'lerin hepsi sanaldır. aslında her biri teker teker (daha doğrusu CPU sayımız kadarı) CPU'ya girerler. fakat işletim sistemi, üzerinde çalışan yazılımlar durmasın/hata vermesin diye onlara thread açar fakat beklemeye alır.

# fiber
kelime anlamı: lif

bir process birden fazla thread'e ayrılabilir. aslında thread'ler yönetim biçimlerine göre 2 ye ayrılır: fiber ve thread.

CPU bir thread'i herhangi bir anda interrupt edip, diğer thread ile işleme devam edebilir. Bu data-integrity için problem olabilir. Tabi burada multi-core CPU'nun avantajlarını yaşamış oluruz.

Oysa fiber'ler interrupt edilemezler. Daha doğrusu öngörüldüğünde (belli bir birimlik işi bitirdiğinde) interrupt edilirler.

Not: Her OS mutlaka thread'leri yukarıda yazıldığı gibi yönetecek diye bir kaide yok. Fakat genelde bu şekilde yönetir. Bu sebeple; fiberler user-seviyesinde manage edilirler. Genelde framework/kütüphane aracılığı ile yapılırlar. Bu sebeple genelde; fiberlerden OS'un haberi yoktur.

thread'ler; __preemptive scheduling (or preemptive multitasking)__'e dayanır (preemptive kelime anlamı: öncelikli). Oysa fiber'ler; __cooperative scheduling (or non-preemptive multitasking or non-preemptive scheduling)__'e dayanır.

Terim anlamlarını incelersek; "scheduler"; task'ları yöneten (başlatan, durduran...) sistemdir.

# CPU core

bir CPU içinde birden fazla çekirdeğe (core'a) sahip olabilir:

- Dual core --> 2 çekirdekli
- Quad Core --> 4 çekirdekli

# CPU socket (or CPU yuvası or CPU slot)

- isminden de anlaşıldığı gibi; anakart üzerinde bulunan, CPU'nun takılması için tasarlanmış fiziksel giriştir.

- bir anakartta birden fazla CPU slotu olabilir.

# Hyper-threading

CPU teknolojisi ismidir. bu teknoloji ile gerçekte x çekirdekli onlar bir sunucu daha fazla çekirdekliymiş gibi işletim sistemine gösteriliyor. yani bir nevi sanal CPU çekirdeği yaratılıyor. bu şekilde eğer çalıştırılan yazılımlar çok çekirdekli teknolojiye göre yazılmışlar ise; bir nebze kara geçilebiliyor. Çok basit anlamada; CPU'nun boşta kalabileceği sürelerde diğer thread'ler devreye gireceği için performans avantajı olduğu düşünülür.

gerçekte 4 core'umuz olsun. her core, 2 core gibi davransın. bu durumda işletim sistemi 8 adet "logical processor" olduğunu algılayacaktır.

# Virtual Processor vs logical processor

ikiside aynı anlamdadır. fakat Virtual sanallaştırma yaparken, sanal olacak işletim sistemine atadığımız CPU core sayısını belirtmek için kullanılıyor.

# Intel VT-x vs AMD-v

Intel ve AMD'nin donanım sanallaştırması için kullandığı teknolojilerin isimleri. bir donanımı sanal olan makinalara bölüştürmek için yazılımsal olarakta sanallaştırma yapılabiliyor, fakat donanım desteği olunca daha hızlı ve güvenlidir.

# Intel CPU isimlendirme
her grubun alt modellerinin isimlendirmeleri ayrı formattadır.

- core
  - 5th generation
    - i3
      - farklı core sayıları
      - farklı cache
      - Processor Base Frequency
      - farklı gömülü grafik işlemcisi
      - ...
    - i5
    - i7
  - 6th generation
    - i3
    - i5
    - ...
  - 7th generation
    - i3
    - i5
    - ...
- Xeon
  - ...
- Atom
- Pentium
- Xeon phi
- Quark SoC
- Celeron
- Itanium

## nitelik belirteçleri
U, Y, T, Q, H, G, K gibi keyword'ler içerebilir. Bunlar:

- U: Ultra Low Power. The U rating is only for laptop processors. These draw less power and are better for the battery.
- Y: Low Power. Typically found on older generation laptop and mobile processors.
- T: Power Optimized for desktop processors.
- Q: Quad-Core. The Q rating is only for processors with four physical cores.
- H: High-Performance Graphics. The chipset has one of Intel’s better graphics units in it.
- G: Includes Discrete Graphics. Typically found on laptops, this means there is a dedicated GPU with the processor.
- K: Unlocked. This means you can overclock the processor above its rating.

## örnek bir ürün
örnek product name: Intel Core i7-5950HQ Processor
- "Quad Core" gibi bir isimlendirme içermese de, 4 çekirdeklidir.
- cache değeri isminde yazmıyor. bu detaya Intel'in resmi sitesinden bakmak gerekli.
- 5th Generation modeli. fakat bu da isminde yazmıyor.
- H ve Q nun ne olduğu yukarıda belirtildi.
- 5950 sadece özel modelin isimlendirmesi. Aynı numaraya sahip başka Generation veya i3, i7 gibi gruplardan bir ürün yok. fakat bu numara ile aynı olup farklı özelliklere sahip olan CPU'lar üretiliyor. ilk baştaki "5", 5th generation olduğunu temsil ediyor. Bu durumda sadece "core" grubundakiler için geçerli.

## tüm liste ve diğer detaylar
Buradan tüm CPU seçimi yapılabilir ve her CPU'nun tüm detayları incelenebilir: (source-id: 66)

Buradan tüm CPU'lar tek sayfada listelenmiştir: (source-id: 67)

Burada her grup için nasıl bir isimlendirme standardı kullanıldığı ve nitelik belirteçleri yazmaktadır: (source-id: 68)

# Nvidia
Nvidia firmasının geliştirdiği birçok ürün var. Son kullanıcı tarafından tüketilen GPU ürünleri ailesine GeForce adı verilmektedir. Nvidia'nın farklı GPU ürün aileleri de vardır. örnek: "Quadro". Nvidia aynı zamanda GPU dışında da piyasada kullanılan ürünleri vardır.

# ATI
ATI firması, AMD tarafından satın alınmıştır.

ATI'nin son kullanıcı için sunduğu GPU ürünleri ailesine Radeon adı verilmektedir. Fakat daha farklı GPU'ları da mevcuttur. ATI'nin aynı zamanda GPU dışında da piyasada kullanılan ürünleri vardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# wrapper sınıfları

tüm programlama dilleri için ortak bir kavramdır. bazen bazı değerlerin (nesnelerin) bir sınıfın içinde bulunmasını isteyebiliriz. bunun birçok sebebi olabilir. örneğin; Java'daki collections'lar primitive değerler alamazlar. fakat elimizdeki primitive değerleri bir listede toplamak istiyoruz. böyle olunca bu değerleri geçici bir sınıfa atarız, ve bu sınıflardan oluşan bir collection yaratırız.

```java
Integer box = new Integer (x); // boxing

int y = box.intValue(); // unboxing
```

wrapper sınıfları bazen "holder" sınıfları olarakta geçmektedir.

__Autoboxing__ compiler'ın bizim için otomatik wrapper sınıfına atama yapmasıdır. örnek:

```java
Character ch = 'a';
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Liferay
Liferay, bir portal platformudur ve Ücretsiz (Community Edition) ve Ücretli (Enterprise Edition) olmak üzere iki şekilde sunulmaktadır.
Liferay Java'da yazılmıştır. OSGI kullanarak modüler bir yapıdadır.
Liferay bir content management system (CMS - içerik yönetim sistemi , Joomla, Drupal, WordPress gibi ) 'dır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# QA (or Quality Assurance or kalite güvencesi)

teknik insanlardan oluşan bir ekibin test ettiği platformdur.

# UAT (or User acceptance Testing)

müşterinin (veya ürününü kullanacak olan kişilerin) test ettiği platformdur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# profiling

bu terim yazılım geliştirme dünyasında, çalışan uygulamayı runtime sırasında monitör etmek (bellek, CPU kullanımı gibi parametreleri) anlamında kullanılmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Telemetri (or Uzölçüm or Telemetry)
bir sistem ya da tesisin uzaktan kablo veya kablosuz olarak izlenmesi veya kontrol edilmesidir.

# tele
Yunanca "tele" (uzak, ırak) anlamına gelmektedir. bu kelime birçok bilişim kelimesi için ön ek olarak kullanılmaktadır. örnek: telecommunication, telegraf, telephone, television...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Macromedia
Adobe tarafından satın alınan şirket.

# Adobe Photoshop
fixed size grafikler için kullanılan uygulama. Vektör grafik çizilemez.

# Adobe Illustrator vs Inkspace
Vektör grafikler çizim için tasarlanmış birbirine alternatif uygulamalar.

# Adobe Fireworks
hem bitmap hemde vektörel grafik tasarlamak için geliştirilmiş yazılım. sadece web teknolojileri/uygulama teması geliştirme gibi işler için tasarlanmıştır. bu sebeple Photoshop yada illustrator gibi gelişmiş değildir. daha basit basit ve hızlıdır.

# Adobe AIR
masaüstü için web teknolojileri ile uygulama yazmaya yarayan altyapı. air ile yazılmış uygulamayı run etmek için bilgisayarda "AIR Runtime" yüklü olmalıdır.

# Adobe Shockwave Player
Adobe Director tarafından geliştirilen uygulamalarını oynatan oynatıcıdır. web tarayıcısına eklentidir. "Flash Player"'dan tamamen bağımsız ve ona alternatif bir teknolojidir. flash temelde video oynatmak için tasarlanmıştır. oysa "Shockwave Player" web uygulaması geliştirmek için tasarlanmıştır.

# Macromedia Flash Player (or yeni adı: Adobe Flash Player)
Macromedia'nın geliştirdiği dönemlerde ismi "Shockwave Flash Player"'dı.

# Adobe Flex (or yeni adı: Apache flex)
flash uygulaması geliştirmeye yarayan framework.

# Adobe Dreamweaver
web sayfası tasarlamak için geliştirilmiş IDE'dir. HTML, CSS gibi web dilleri ile önyüz geliştirmek için tasarlanmıştır.

# 3ds Max (or old-name:3D Studio or old-name:3D Studio Max)
Autodesk firması tarafından geliştirilen 3d ve 2d grafik geliştirme yazılımı.

# AutoCAD
Autodesk firması tarafından geliştirilen teknik resim çizme yazılımıdır.

# teknik resim
Teknik resim, bir şeyin nasıl çalıştığını veya üretildiğini anlamak üzere yapılan çizim. mühendislik gibi alanlarda özellikle tercih edilir.

# Bilgisayar Destekli Tasarım (or CAD or Computer-Aided Design)
digital ortamdan yararlanılarak yapılan teknik resim çizimidir

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# null vs nil
temelde aynı manaya gelselerde bazı programlama dillerinde çok ince farklılıkları olabiliyor.

bazı programlama dillerinde Nil ve nil dahi birbirinden farklı da olabiliyor. büyük harfle başlayan "Nil" obje hali oluyor.

- "nil" kelime anlamı: hiçbir şey, boş
- "null" kelime anlamı: değersiz
- "void" kelime anlamı: geçersiz

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# GUI design patterns
Burada çok sade şekilde açıklanmış: (source-id: 69)

# hibrit yaklaşım
bazı ekranlar (web, mobil, masaüstü, tablet uygulamaları) tasarlanırken hem responsive hemde adaptive design kullanılabilir. Ekran boyut değerleri eşik değerlere ulaştığında (bu eşit değerleri tamamen seçime bağlıdır) adaptive, bazı boyut aralıklarında ise responsive kullanımına otomatik geçiş yapacak şekilde aynı ekran tasarlanır.

# responsive
ekran küçülüp büyüdükçe tüm nesneler kendi içinde sürekli büyür veya küçülür. Cihazda ekranı kaplamaya (ekrana sığmaya) çalışır.

# adaptive
sayfada her nesne fix boyuttadır. pencere boyutu büyültüp küçültüldüğü zaman ekrandaki nesnelerin yeri değişmektedir. ekran boyutu farklı boyutlardayken, her nesne farklı yere konumlanır. dolayısı ile cihaz gruplandırılması söz konusudur.

# static
pencere içerisinde ekran küçülsede, büyüsede nesnelerin yeri ve büyüklüğü değişmemektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# robots.txt

dosyaya erişim domain'in URL'sinin root'undan olmalıdır. bu dosyada yazan adresler arama motorları tarafından index'lenmemektedir. fakat buna uymayan robot'lar da mevcuttur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# tr:barkod (or eng:barcode or eng:bar code or çubuk kod or çizgi im)
2 boyutlu barkodlara __matrix code (or 2D barcode or 2D code or matrix kodu or iki boyutlu barkod)__ denilmektedir.

1 veya 2 boyutlu bir resimde kare kare siyah ve beyaz noktalar mevcut. Bunların her biri birer ikiliğe (byte) denk geliyor. Bu değerler resimden okunduğu (tarandığı) zaman, sonucunda uzunca bir byte array elde ediliyor. bu artık istenilen formatta anlamlandırılıyor. bu byte-array'i anlamlandırmak için birçok standart var. bu standartlardan biri de "__QR Code (or Quick Response Code)__"'dur.

Piyasada QR Code gibi birçok alternatif mevcut. tüm bu alternatiflere genel olarak barcode adı verilir. bu alternatiflerin bazı farkları:

- bazı barkod'lar renkli, bazı barkodlar sadece siyah beyaz

- bazı barkodlar 1 boyutlu (çizgi şeklinde), bazıları 2 boyutlu (matriks şeklinde), bazıları ise farklı geometrik şekillerden okunurlar. bu tarz farklılıklara buradan bakılabilir: (source-id: 70) "Matrix (2D) barcodes" başlığı.

- byte-array'in maximum boyutu

- byte array'in okunacak formatın encoding'i.

- byte array okunduktan sonra string'e çevriliyor fakat bu string ne anlama geliyor: örneğin:
  - calendar event
  - contact information
  - email adres
  - geo location
  - phone number
  - SMS
  - text
  - URL
  - Wi-Fi information (with password)

  burada desteklenen format tipleri kısıtlı olmaktadır.

- byte array okununca yanlış okunmuş olabilir veya barcode eski olduğundan bazı kısımları net olmayabilir (sönük vs..). bunların doğru olup olmadığını anlamak için barkod'un bir kısmı hata tespiti için hash gibi kısımlar içeriyor.

- ve daha birçok özellik standarta göre değişebiliyor

# bazı barcode çeşitleri:

| 1D product            | 1D industrial | 2D           |
|-----------------------|---------------|--------------|
| UPC-A                 | Code 39       | QR Code      |
| UPC-E                 | Code 93       | Data Matrix  |
| EAN-8                 | Code 128      | Aztec        |
| EAN-13                | Codabar       | PDF 417      |
| UPC/EAN Extension 2/5 | ITF           | MaxiCode     |
|                       |               | RSS-14       |
|                       |               | RSS-Expanded |

# ZXing ("Zebra Crossing")
açık kaynaklı Java barcode okuma kütüphanesidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# StackExchange
çeşitli konularda soru-cevap sitelerini bir araya getiren site ağı. StackExchange, aralarında Matematik, İstatistik, Din, Astronomi, Bilgisayar programcılığı gibi belirli konularda özelleşmiş 130'un üzerinde soru-cevap sitelerini barındırmaktadır. Tüm liste buradadır: https://stackexchange.com/sites. Bazıları aşağıdaki gibidir:
- bilgisayar programcılığı (yazılım geliştirme) ile olan site Stack Overflow'dur (stackoverflow.com).
- bilgisayar kavramları ile ilgili tüm soruların olduğu site: "superuser.com"'dur.
- Ubuntu hakkındaki soruların sitesi: "askubuntu.com"
- network/sistem soruların sitesi "serverfault.com"'dır.
- https://apple.stackexchange.com/ "Ask Different" sitesi olarak geçmektedir. Bu sitede Apple cihazlar ve yazılımları ile ilgili sorular sorulmaktadır.

Görüldüğü üzere domain URL'leri ve site isimleri farklı olabilmekle birlikte, bazı siteler direk "StackExchange.com" subdomaini olabilmekte, bazılarının ise kendi domainleri vardır. örnek: https://apple.stackexchange.com/ ve https://stackoverflow.com/ gibi.

# meta
StackExchange'in her tüm alt sitesinin (örnek: Stack Overflow) bağımsız bir soru listesi/cevabı olan "meta" siteleri (örnek: https://meta.stackoverflow.com/) vardır. Bu meta sitelerde, sitede olan policy'ler, özellikler ve commmunity'ler hakkında sorular sorulmaktadır.

StackExchange ile ilgili sorular: "örnek nerede bu soruyu sorabilirim", "neden konulara cevap yazamıyorum"... meta.stackexchange.com'da soruluyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# PMP (or Project Management Professional)
proje yönetimi konusunda en yaygın kabul gören sertifikadır. bu sertifika, merkezi ABD'de bulunan __Project management institute (or PMI)__ kuruluşu tarafından verilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Cocoa vs Cocoa Touch
Cocoa Apple altyapılı ürünler için GUI ve diğer uygulamaların çalışması için kütüphane kümesidir. Cocoa Touch, kısmen Cocoa modülleri içeren ve sadece tv, saat, telefon gibi ürünler için gerekli kütüphaneleri içerir.

# CocoaPods
xcode projelerinde third party kütüphanelerin yönetimini sağlayan paket yöneticisidir. Java'daki Maven gibi. xcode projesi ilk açıldığında Pod desteği olmaz. Pod desteğini proje dizinine giderek "pod init" komutu ile verir. daha sonra projenin içinde "podfile" isminde bir dosya oluşur. buranın için kütüphanelerimizi ekleriz. her ekleme sonrası komut satırından "pod install" komutunu çalıştırmak durumundayız.
Pod yapısı, aynı xcode workspace'inin içine "Pods" isminde bir proje daha oluşturur. bu projede tüm third party kütüphaneler mevcuttur. asıl projemiz sadece Pod projesine depend eder.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Breadcrumb
Bir uygulamada (web/desktop) kullanıcı bir ekrandan diğerine yönlendirilmektedir. bazen içiçe sayfalar çok sayıda olabilmektedir. böyle durumlar için sayfada nerede olduğunu gösteren ağaç-tarzı yapılara Breadcrumb denilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Kurumsal kaynak planlaması (or işletme kaynak planlaması or Enterprise Resource Planning or ERP)
işletmelerde mal ve hizmet üretimi için gereken iş gücü, makine, malzeme gibi kaynakların verimli bir şekilde kullanılmasını sağlayan bütünleşik yönetim sistemlerine verilen genel addır.

ERP'nin alt modülleri:
- __CRM (or customer relationship management)__: Satış programı, Pazarlama programı, Müşteri servisleri programı, teknik destek programı gibi görevleri içeren yazılımdır.
- İnsan kaynakları modülü
- muhasebe modülü
- ...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Standard streams

In computer programming, standard streams are preconnected input and output communication channels between a computer program and its environment when it begins execution. The three I/O connections are called standard input (stdin), standard output (stdout) and standard error (stderr). Originally I/O happened via a physically connected system console (input via keyboard, output via monitor), but standard streams abstract this. When a command is executed via an interactive shell, the streams are typically connected to the text terminal on which the shell is running, but can be changed with redirection, e.g. via a pipeline.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# PIN pad (or PIN entry device or PED)
PIN girilen cihazlara verilen genel isimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Jmeter

web servislerini test etmek (yük testi) için gerekli yazılım.

# JavaMelody

Java uygulamalarını (sunucu üzerinde olsa da) runtime sırasında monitör etmek için kullanılan yazılım. Java kütüphanesi olarak, incelenecek/run edilecek projenin içine eklenmelidir. Çalışan bu kütüphane runtime sırasında bir port açıyor ve browser üzerinden istatistikleri sunuyor. Uygulamaya yapılan HTTP request'ler hakkında istatistikler, database'ye yapılan SQL'ler hakkında birçok  istatistik mevcut.

# Jvisualvm

sadece JDK paketinin içinde bulunan (bin dizininde) görsel bir uygulamada performans monitör etme yazılımıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# SonarQube
Kod kalitesini arttırmak/incelemek amaçlı yazılımdır. Bu yazılım commit'lenen kodları sürekli olarak incelemeye alıp, kodun durumu hakkında rapor/bilgilendirme yapmaktadır. Bazı algoritmalar kod üzerinde tekrarlanan kısımları tespit edebilir, potansiyel hataları bulabilir (runtime'a ihtiyaç duymadan direk kodu inceleyerek karar verebilir).

"__Sonar__" projenin eski adıdır. __SonarLint__ ise; Eclipse IDE için yazılan eklentiye verilen isimdir.

# jshint vs eslint vs jslint
Javascript için potansiyel kod hatalarını ve düzenini sağlamak için kullanılan yazılımlardır.

- IDE'mizde ilgili paketin eklentisi var ise, root dizinimizde ilgili paketin config dosyasını ayar ve bu config'lere göre kodumuzu düzeltir, yada uyarı verir vs...
- komut satırından çağrılarak, hatalar (sonuçlar) çıktı olarak görülebilir.

# Eslint-plugin (or Eslint-parser) vs Eslint-config
Eslint için birden fazla parser (rule) çeşidi yazılabilir. bu parser'lar plugin'ler tarafından yazılır. config'ler ise hangi parser'ların enable/disable/error/warning olarak gösterileceğini belirler (extend eder). örnek: Airbnb-plugin-eslint, Babel-plugin-eslint piyasada sık kullanılan yazılımlardır.

ilgili dizin içerisinde ".eslintrc" dosyasında configler (parser plugin'i yazılır, hangi rule'ler disable edilecek belirtilir gibi) yazılır. IDE'deki ilgili plugin yada çalıştırılan komut satırı uygulaması alt dizinlerde bu kurallara göre hataları bulur.

IDE'lerin genelde kendi formatter'ları oluyor. bu formatter eklentileri, eslint'ten bağımsız çalışıyor. eslint'in kendi formatlama kısayol tuşu oluyor. burada bu durum tartışılmış: (source-id: 71)

Eslint'te hatalar ve formatlama farklı kavramlar. genellikle bu ikisi (bazı yazılımcılar tarafından) bazı durumlar için karıştırılabiliyor. bu sebeple eslint formatter'ı, (hataları gidermeyeceği için) formatlamamış olarak düşünülüyor. oysa eslint formatter'ı zaten hataları gidermez.

# lint (or linter)
potansiyel kod hatalarını yakalamak ve düzenini sağlamak için kodun kontrol edilmesi/eden tool'a verilen genel isimdir.

# EditorConfig
.editorconfig dosyası projemizin root'unda durur. EditorConfig'in neredeyse tüm IDE'lerde default/gömülü support gelir. Sadece kodun formatını belirleyen standartlar içerir.

EditorConfig, "Prettier" gibi formatlamak için executable içermez. EditorConfig sadece IDE'ye nasıl çalışması gerektiğini içeren bir standart dosyadır.

Eslint ve Prettier ile aynı projede EditorConfig de bulunabilir. Bunda bir sakıncası yoktur. Bu 3'ünün kısmen aynı görevleri gördükleri yerler var, fakat temelde farklı görevleri var: Eslint kod analizi yapar (kullanılmayan variable'ları uyarması gibi), Prettier ve EditorConfig ise aynı amaçtadır. Fakat Prettier IDE'lerde native desteklenmez, hep eklenti gereklidir. Her developer farklı IDE'de açıp ufak bir değişikliği commit etmek isteyebilir. Bu sebeple EditorConfig her zaman kullanılmalıdır.

# Code coverage
testler yazılırken, belirli metotlar çağrılmaktadır. yani belirli satırlar kodda çağrılmaktadır. code coverage yüzdelik cinsindendir. %100 olan kod'da, her satıra en az bir kere girilmiş demektir.

# static vs dinamik test
static test, asıl uygulamanın kodu yürütülmeden yapılan analizler/testlerdir. örneğin; kodun temiz olup olmaması, derlenip derlenmediği, dökümanlarının yazılıp yazılmadığı gibi faktörlerin testini kapsar. dinamik testlerde ise; JUnit ve integration testleri koşulur.

# integration test (or entegrasyon testi) vs unit test (or birim testi) vs end-to-end (or E2E test or uçtan uca test)
unit test bir birimin kendi içinde çalışıp çalışılmadığının testidir. örneğin; login işlemi olup olmadığı. unit testlerde mock kütüphaneleri ile diğer sistemler mock'lanır. böylece diğer sistemler çalışmıyor olsa bile unit testler tamamlanır.

birden fazla unit'in birbiri ile düzgün çalışıp çalışmadığını test eden testler entegrasyon testleridir. burada sistemin bazı kısımları mock'lanır.

E2E testlerde ise sistem hiç mock'lanmaz.

__system test (or sistem testi)__ terimi genelde E2E testi yerine kullanılıyor. fakat E2E terimini kullanmak daha net olduğu için, E2E terimi tercih edilmelidir.

# component test (or service component test)
birden fazla servis olduğunda, tek 1 servisin ayağa kaldırılıp diğerlerinin mock'landığı, sadece ayağa kaldırılan servisin testinin yapıldığı testlerdir.

# happy path test (or golden path test or happy day scenario test)
uygulamanın işleyip işlemediğini test etmek için yapılacak en basit testi ifade eder. örneğin ufak bir update çıkıldı. tüm sistem testi çalıştırılacak zaman olmasın. e-ticaret sitesi isek; sisteme login olunur, hızlıca bir alışveriş yapılır ve happy test başarılı olmuştur.

# smoke test (or confidence test or sanity test or build verification test or BVT or build acceptance test)
deployun düzgün olup olmadığını test etmek için sadece ana fonksiyonların test edildiği test grubudur.

smoke test, happy path test ile çok çok benzer kavramlardır. Tek istisna şudur: happy path'te hiçbir exceptional case senaryosu test edilmezken, smoke testte case'in önemine göre exceptional senaryolar da test edilebilir.

# Disaster Recovery Test
Deprem gibi felaket durumlarında olabilecek senaryolar test edilir. kaynak: (source-id: 450)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# .htaccess

Apache HTTP server'ın kullandığı bir dosyadır. bu dosya içerisinde basit tanımlar ile hangi dosyaların dışarıya açık, hangilerinin hangi IP'lere açık, 404 durumunda hangi dosyanın response olarak gideceği gibi bir çok özelliği ayarlanabilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Endüstri 4.0 (or 4. Sanayi Devrimi or Industry 4.0 or Industrie 4.0 or the fourth industrial revolution)

Tarih boyunca endüstride birçok devrim yaşanmıştır. sırası ile:

1. su ve buhar gücünün daha verimi kullanan sistemler oluşturulması (1800 yılları)

2. üretim bandı tasarımı ve elektriğin seri üretimde kullanılmaya başlanması

3. mekanik ve elektronik teknolojilerin yerini dijital teknolojiye bırakmasına sebep olan programlanabilir makinelerin kullanılmaya başlanması (1950 yılları)

4. burada amaç; endüstrinin (dikkat: sadece fabrikanın değil) siber-fiziksel (tüm fiziksel sistemlerin) sanal ortamla iletişimde kalarak çalışmasıdır.

   programlanabilir embedded cihazlar fabrikalarda belli firmalar tarafından dağıtılıp, sadece belirli işler için kullanılabiliyordu. bu sebeple çoğu yazılım özel-tasarım işletim sistemlerinde çalışıyordu. 4.0 ile yazılan yazılımlar daha modüler ve daha farklı amaçlarda kullanılabilmesi planlanıyor. Bunun için işletim sistemi yapılarının ortak olması gerekiyor. Ancak bu şekilde başkası sizin yazılımınıza depend olabilir yada fork edebilir. bu şekilde hem daha hızlı/kolay uygulama geliştirilebilecek, hemde tekrar kullanılabilir modüler uygulamalar artacak. burada işletim sistemli bu cihazlar iot cihazlarına denk gelmektedir. kolay ve hızlı uygulama geliştirme sayesinde ise; big data, yapay zeka gibi kavramlardan daha çok yararlanılabilecek. hatta bu durumdan sadece fabrikalar değil, o endüstriye bağlı internet siteleri etkilenecek. mesela e-ticaret sitesinde ürün stokları fabrikadaki stok sayısından otomatik gösterilecek. e-ticaret sitesindeki kullanıcıların isteğine bağlı renkte ürünler fabrikadan otomatik üretilecek ve teslim edilecek gibi...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

Aşağıdaki 3 terim piyasada birbirleri yerine kullanılmaktadır. oysa bu yanlış.

# Veri Mühendisi (or Data Engineer)
Ham verilerin toplanması, yönetilmesi, veri bilimcilerin yorumlaması için kullanılabilir bilgilere dönüştürülmesi ve erişim için veri altyapısının tasarlanması bir veri mühendisinin temel görevlerindendir. Kısaca veri mühendisi, ham veriler ve öğelerin analize hazır olup olmamasına odaklanır.

Veri mühendisleri, standart piyasadaki yazılımcılardır. Java, C# gibi dillerde yetkindirler.

# Veri bilimci (or Data Scientist) 
veri mühendisinden aldığı verileri, istatistik, veri analitiği ve/veya yapay-zeka kullanarak bir öneri veya tahminleme raporları sunmaktadır.

veri bilimci temelde data'yı bu 3 işlemi yaparak işletir: extract, transform, load (3 işlemin kısaltması: ETL).

veri bilimcisi SQL, Python, R gibi dillere hakim olmalıdır. Database'lere de hakim olmalıdır. Çünkü veri mühendisinden alınan data'lar ETL süreci için farklı veritabanlarına da taşınmaktadır.

# Data Analyst (or veri analisti)
daha çok domain bazlı (finans gibi) sektöre hakim ve business'ı bilen kişilerdir. veri bilimcilerin çıktılarını ilgili sektör için daha anlamlı hale getirerek, yeni raporlar/görseller sunarlar.

yetkinlikler: domain business, SPSS, Microsoft Excel, Microsoft Access, Visual Basic for Applications (or VBA) on Microsoft Office, SQL, R, Python.

# Machine Learning Engineering (or ML Engineering)
Özellikle Machine Learning kullananan veri bilimcidir.

# veri madenciliği (or Data Mining)
veri bilimcinin sıkça kullandığı metodolojilerin disiplinidir.

# İş zekası (or Business Intelligence)
endüstride, ham veriyi anlamlı ve kullanışlı bilgiye dönüştüren teoriler, metodolojiler, süreçlerin, mimarilerin ve teknolojilerin bir kümesidir. iş zekası işinde çalışanlar; istatistik, veri madenciliği, iş/süreç analizi, raporlama gibi bir çok farklı şeyden yararlanarak şirket için kararların alınmasında rol oynarlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Daemon (or Disk and Execution Monitor)
Daemon kelime anlamı: şeytan

Yazılım dünyasında bu terim arkaplanda çalışan uygulamalara verilmektedir. Direk olarak kullanıcı ile iletişimde değildir, fakat arkaplanda komut/request almak için beklemektedir. genelde uygulamanın isminin sonuna "d" harfi koyulur. örneğin; Syslogd, işletim sisteminde system log'larını tutan arkaplan uygulamasıdır. sonundaki d harfi daemon'dan gelmektedir. Microsoft Windows'taki servisler birer Daemon örneğidirler.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Internet Information Services (or IIS)

Web sayfalarının yayınlanmasını ve web uygulamalarının çalışmasını sağlayan, istemcilerden HTTP ve FTP üzerinden gelen talepleri Microsoft Windows sunucu tabanlı işletim sistemlerinde karşılayan birimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Sanal gerçekçilik (or Virtual Reality or VR)
Son kullanıcıyı tümüyle sanal bir ortama aktarıp, orada hareket etmesini sağlayan sistemlerdir.

# Artırılmış gerçeklik (or Augmented Reality or AR) vs Mixed Reality (or Karma Gerçeklik or MR)
2 teknolojide gerçek fiziksel dünyanın son kullanıcıya zenginleştirilip yansıtır. Fakat aralarında temel fark vardır: MR, gerçek dünyadaki objeler ile uyumludur ve onlarla etkileşimdedir. Oysa AR'da etkileşim yoktur. Örneğin;
- Örneğin AR bir gözlük aracılığı ile; son kullanıcı sokakta gezerken, aslıda olmayan bir insanı görebilir. Sadece o insan tamamen sanaldır. kalan herşey gerçektir. Fakat o insan etraftaki objeler ile etkileşim halinde ise bu MR ile yapılabilir.
- Çamaşır makinesine baktığımzıda AR ile gözümüzün önünde hemen bir sanal kitapçık popup'ı görüntüleyebiliriz. Fakat eğer çamaşır makinesi ile etkileşimli (onunla aynı düzlemde/3D uyumlu biçimde) çamaşır makinesini çalıştırmak için kolaylaştırılmış buttonlar görüyorsak, bu MR ile yapılabilir. (not: bu örnekte, cihazın üstünde oluşan sanal buttonlara bastığımızda, fiziksel makinedeki hiçbirşeye basmamış oluyoruz. fakat burada, kullandığımız gözlük çamaşır makinesine Wi-Fi üzerinden bilgi göndeebildiğini varsaydık.)
- Google'ın ilk çıkardığı "Google Glass" gözlükleri tam bir AR örneğidir. Etraf ile etkileşimli davranan sanal objeler yaratamamaktadır. Sadece son kullanıcıya popup ile etraf hakkında bilgi göstermektedir.

# extended reality (or XR or Genişletilmiş gerçeklik)
MR, AR ve VR yeteneklerinin 3'ünü kapsayan cihazlar için kullanılan genel bir terimdir.

# Holografi
Uzayda bir cismin varlığına ait bilgi bize genellikle ses veya ışık dalgaları halinde ulaşır. Holografi, cisimlerden gelen dal­galardaki bilgileri belirli bir şekilde depo edip, bu bilgide hiçbir kayıp olmadan tekrar ortaya çıkartmayı sağlayan bir tekniktir.

# Piyasadaki cihazlar
- Microsoft - Hololens
- Oculus/Facebook - Rift
- Samsung - Gear VR
- Google - Daydream
- Sony - PlayStation VR
- HTC/Valve - Vive

# Kinect
Microsoftun donanım cihazı. ufak bir yerde sabit duruyor. karşısındaki kişinin vücut yapısını uzaktan algılıyor. kullanıcının kıpırdamalarını (el, ayak vs) uzaktan algılıyor. bu şekilde oyun oynamayı sağlıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Statistical Package for the Social Sciences (SPSS)

İstatistik analiz yazılımıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# back-pressure (or backpressure or geri tepme or back pressure)
publisher çok fazla data sağlar ve subscriber bu bilgileri işlerken yavaş ise, subscriber back-pressure yapmalıdır. yani; subscriber publisher'a bir şekilde daha az data yollamasını iletebilmelidir. bu yeteneğe back-pressure denir.

back-pressure sistemi yavaşlatacaktır fakat sistemin, daha stabil (yoğunluğun stabiliteyi bozmayacak düzeyde akışkan) çalışmasını sağlayacaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# aspect oriented programming (or cephe yönelimli programlama or AOP)
başka başlıkta bununla ilgili şeyler yazıldı.

Bütün programlama yaklaşımlarında kodlar uzadıkça, kodların anlaşılabilirliği çok düşmekte, bazen de içinden çıkılmaz bir hal almaktadır.

aspect felsefesi ile ortak olan kod parçaları bir yerde toplanır. uygulamanın her hangi bir kısmında bu ortak metotları kullanarak kodu kısaltmış oluruz. aspect felsefesi budur. aspect felsefesi uygularken kütüphane şart değildir. kendimiz elle metotları çağırabilir. fakat %90 kütüphane kullanılır. sebebini bir örnekle açıklayalım: programımız her metot çağırdığında log atsın istiyoruz. bu ortak log metotunu sürekli her metotun başına ekleyebiliriz. fakat bir kütüphane olsa bize bunu otomatik yapar. bu en belirgin ve basit örnektir. eklenecek kütüphanenin metotların başına/sonuna işlem ekleme (method-interceptors) yapabiliyor olması gerekir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Behaviour Driven Development (or BDD)

TDD benzeri fakat X olursa şunu yap gibi terimlerle desteklenen test yazım felsefesidir.

Java'da JBhave frameworku (genelde) tercih edilmektedir.

given/when/then gibi terimler kullanılır.

(aşağıdaki 3 madde için kaynak: (source-id: 72))

- Given kodları kısmı; test edeceğimiz metot çağrılmadan ortamı hazırladığımız kısımdır. örnekler: test edeceğimiz metota giden parametreleri hazırlamak, test edeceğimiz metotun içindeki dışa bağımlılıkların mock'lanması.
- when'de ise test metotumuzun çağrıldığı kısımdır.
- then'de sonuçta beklenen durumun olup olmadığının kontrol edildiği kısımlardır.

Bu belirtimler DSL ile yazılabileceğinden, konuşma dili ile yazılan cümleler içerebilir. DSL'e indirgenebilmesi BDD'nin bir avantajıdır.

örnek:

```
Feature: User trades stocks
  Scenario: User requests a sell before close of trading
    Given I have 100 shares of MSFT stock
       And I have 150 shares of APPL stock
       And the time is before close of trading

    When I ask to sell 20 shares of MSFT stock

     Then I should have 80 shares of MSFT stock
      And I should have 150 shares of APPL stock
      And a sell order for 20 shares of MSFT stock should have been executed
```

Mockito kütüphanesinin org.mockito.Mockito sınıfının metotların isimlendirmeleri BDD'ye uygun değil. Bu sebeple org.mockito.Mockito'ile aynı işlevi gören (direk olarak onun metotlarını aynen çağıran) bir implementasyon tasarlandı: org.mockito.BDDMockito. kaynak: (source-id: 72)

# Test-driven development (or TDD)

Önce test sonra kod yazıma gidilerek yapılan kodlama felsefesidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# MATLAB
"matrix laboratory" nin kısaltması olarak isimlendirilmiştir.

MathWorks tarafından geliştirilen sayısal hesaplama/analiz yazılımıdır. MATLAB isimli programlama dili yada diğer programlama dilleri ile çalışılabilir.

MATLAB isimli uygulamada, Microsoft Access gibi, kullanıcıyı arayüz sunan ekranlar da hazırlanabilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DevOps
development ve operations kelimelerinin kısaltmalarının birleşiminden meydana gelmiştir. IT'deki tüm ekiplerin (yazılım geliştirme, destek...) birbiri ile ilişkili şekilde çalışması kültürüdür. Bu kültürün oluşabilmesi için operation kısmındaki birçok işin otomatize edilmesi gerekmektedir. İşte "DevOps engineer" burada devreye giriyor.

sadece "operations" kısmı şu birimlerden oluşmaktadır: sistem mühendisleri, sistem yöneticileri, sürüm mühendisleri, veritabanı yöneticileri (DBAs), network mühendisleri, güvenlik uzmanları vs...

# SecOps vs DevSecOps
Security engineer'ler DevOps'tan ayrı çalışıyorlardı. Fakat bu 2 gurubun birlikte çalışma kültürü/yönetim biçimi SecOps olarak adlandırılır. __DevSecOps__ ise hem development ekibinin de bu gruba dahil olması ile birlikte olur.

# Ops prefix
Aslında bakıldığında *Ops terimleri çok keskin çizgilerle ayrı değildir. Örneğin DevOps içerisinde zaten security olması gerekmektedir. Hiç güvenlik olmadan deployment süreci zaten işletilemez. Fakat gerçek hayatta şirketlerde ayrı olan bu grupları, daha efektif çalışabilmeleri amaçlı yeni terimler ortaya atılmaktadır. örnek terimler: __DataOps, ITOps (or TechOps), AIOps (or Artificial Intelligence Operations), MLOps (or Machine Learning Operations)__.

# ChatOps (or Chatbot Operations)
slack gibi sohbet araçlarından chatbot aracılığı ile IT operasyonlarının yapılmaya çalışıldığı yaklaşımlardır. chat üzerinden IP sorgulama, makine çalışıyor mu gibi sorguların yapılması bunlara en basit örneklerdendir...

# NetOps (or network operations)
network ekipleri ile etkileşimli çalışma yaklaşımıdır. özellikle cloud, Docker, virtualization kullanan şirketler için önemli bir kavramdır.

# NoOps
her IT departmanının nihai hedefidir. Hiçbir operasyon ekibinin girdisine gerek kalmadan sistemin işlemesini ifade eder.

# GitOps
"operations" birimlerinin de Git (SCM) üzerinde config'lerini tutarak çalışması kültürüdür. Bunun için gerektiğinde ek tool'lardan yararlanılabilir.

Git'te olan dosyalar execute edildiğinde, tüm ifrastructure altyapısı deploy olmalıdır. bu oluşan ifrastructure üzerinden de canlı ortama deploy çıkılabilmelidir.

# BizDevOps
Piyasada bazı kaynaklarda "__DevOps 2.0__" veya "__BizOps__" olarak geçmektedir.

Business, Development and Operations kelimelerinin birleşiminden oluşmaktadır. İş biriminin ihtiyaçlarını, geliştirilen yazılımın karşılayıp karşılamadığını hızlı şekilde bildirebilen sistemlerdir. Burada QA ve testçiler ve DevOps ekiplerinin çok yakın çalışması söz konusudur. Aynı zamanda agile sürecinde iş birimlerinini daha yoğun destek vermesi ile yürütülen süreçleri kapsar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# path: canonical vs absolute and relative

örnekler:

- absolute path: C:\XyzWs\\.\TEST.TXT  -> dosya sisteminin root'undan alınan fakat noktalama işaretleri ile yönlendirmeler alabilen path

- canonical path: C:\XyzWs\test.txt  -> dosya sisteminin root'undan alınan fakat noktalama işaretleri ile yönlendirmeler içermeyen path

- relative path: .\TEST.TXT -> dosya sisteminin başından değilde o anda runtime sırasında bulunan dizinden itibaren verilen path'tir.

  relative path için kullanılan işaretler:

  - ./images/image1 --> bulunduğumuz dizin altındaki images klasörü

  - ../images --> bulunduğumuz dizin üstündeki images klasörü

  - ../../images --> bulunduğumuz dizinin 2 üstündeki dizindeki images klasörü

  - ~/images --> user'ın home klasörünün içindeki images klasörünü temsil eder. bu relative path değildir. direk olarak root'tan verilmiş bir dizin olduğundan canonical path'tir. çünkü; ~ işareti sadece bir kısaltmadır (alias'tır).

# canonical form
canonical kelime anlamı: kurallara uygun, geleneklere uygun.

canonical form; "__normal form__" yada "__standard form__" olarakta adlandırılır.

herhangi bir matematik objesini farklı notasyonlarla gösterebiliriz. Eğer kullandığımız notasyon, aynı zamanda, objenin tekilliğini ifade ediyor ise, bu notasyon (form biçimi) canonical'dır.

örneğin File-path birçok şşekilde gösterilebilir: Canonical, relative gibi... Eğer relative gösterirsek file-path bulunduğumuz dizine göre değişiklik gösterir. Oysa canonical path gösteriminde bir tekillik söz konusudr. farklı bir değiş ile; File-path değeri canonical path ise; iki file-path'i canonical path ile karşılaştırmamız eşit olup olmadıkları bilgisini elde edebilmemiz için yeterli olmalıdır.

# Uniform Resource Name (or URN)
İlgili kaynağın nasıl erişilebileceğini gösteren bir bilgi sunmaz. İligli kaynağı tanımlayan bir bilgidir.

format:

urn:nameSpaceIdentifier:nameSpaceSpesificString

örnekler:

- ISBN numarası

  urn:isbn:096139210

  Kitaba referans eder. Dünyadaki herkes bu bilgiden hangi kitabın olduğunu anlayabilir, fakat o kitaba nasıl erişilecğeini bilemez.

- International Standard Audiovisual Number

  urn:isan:0000-0000-2CEA-0000-1-0000-0000-Y

  The 2002 film Spider-Man.

- UUID

  urn:uuid:6e8bc430-9c3a-11d9-9669-0800200c9a66

  version 1 UUID

URN'nin farklı versiyonları var. Örneğin 2017'de güncellendi: https://www.rfc-editor.org/rfc/rfc8141#appendix-B.1

# Amazon Resource Name (or ARN)
Amazon Cloud servislerindeki bir kaynağın tanımı: arn:partition:service:region:account-id:resource 

Amazon Cloud'da bir kaynağımız olabilir. O kaynağın ID'sidir fakat o dosyaya nasıl erişilebileceği hakkında hiçbir bilgi sunmaz.

örnekler:

- user:
  
  arn:aws:iam::123456789012:user/johndoe

- SNS topic

  arn:aws:sns:us-east-1:123456789012:example-sns-topic-name

# URL (or Uniform Resource Locator)
İlgili kaynağa nasıl erişilebileceğini temsil eder.

örnekler:
- ftp://ftp.site.com/file1.txt
- http://www.site.com/file1.txt
- ldap://[198.167.1.1]/c=GB?objectClass?one
- telnet://192.0.1.1:80/

# Uniform Resource Identifier (or URI)
soyut veya fiziksel olan bir kaynak için unique ID tanımıdır. URL ve URN bunun birer implementasyonudur. Hepsinin standartları RFC dökümanlarında belirlenmiştir.

standart syntax:

scheme://authority/path1/path2?query1#fragment

# File URI
Sadece file URL için özel bir RFC yayımlandı: https://www.rfc-editor.org/rfc/rfc8089#appendix-B

Buradan da görüldüğü gibi istisna olarak şu şekilde tanımlamalar yapılabiliyor:

- file:///path/to/file

  Bu rfc8089'de değişiklik gösteren bir syntax değil. Bu syntax zaten vardı. Local dosyalarda "autority" kısmı olmadığı için boş kalıyor ve bunun sonucunda 3 slash yanyana gözüküyor.

- file:/path/to/file

  rfc8089'de belirtilen özel syntax.

- file://host.example.com/path/to/file

  network üzerinden erişilen dosyalar için kullanılabilecek syntax.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Library (or kütüphane) vs Framework (or yazılım iskeleti or yazılım çerçevesi or yazılım çatısı)

Library, API'lerini çağırarak işlem yaptığımız ve bazen dönüş aldığımız yazılımlardır. Framework'te ise durum tam tersidir. Framework kendi çalışır ve bizim uygulamamızdaki API'leri çağırır. Bizi kendi akışının bir parçasıymış (modülüymüş gibi) kullanır.

Örneğin; Library; Log4J, ApacheCommonStringUtils verilebilir. Framework; Web uygulaması sistemi verilebilir: Spring, JavaEE gibi. Spring içerisinde bizim uygulamamız bir modül gibi kalıyor. Onun sınıflarını extend ederek akışlar oluşturuluyor.

# Toolkit vs SDK (Software Development Kit)

Bir kütüphaneyi yada bir framework'ü kullanmak yada ondan yararlanmak için zorunlu/isteğe bağlı olarak kullanılan yazılımlardır. resmi tanım olmadıklarından; ikiside aynı anlama gelmektedir. fakat piyasada genel olarak SDK daha çok; başka bir işletim sistemi yada ortam(platform) üzerinde uygulamayı yürütmek için gerekli olan yazılımlar için kullanılmaktadır. toolkit'ler ise SDK içinde yüklü gelebilirler ve daha çok opsiyonel kaynakları barındırırlar.

# yazılım (or software) vs program vs appllication (or app or uygulama or tr:aplikasyon)
Sadece British İngilizce'sinde "program" yerine "programme" da bazen kullanılmaktadır.

- program; bilgisayar biliminde makineye işletilmesi için verilen talimatlardır (kodlardır).

- yazılım; programlar grubudur.

- app; son kullanıcı ile etkileşimde olması şart olan bir yazılım çeşididir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# yorumlayıcı (or Interpreter)
Interpreter kelime anlamı: tercüman, yorumlayan

bir yazılımdır. bu yazılım kaynak kodu direk olarak yorumlar ve yorumladığı şeyleri yürütür. Kodu makine koduna çevirmek zorunda değildir. "Shell" buna en iyi örnektir. Shell aldığı komutları makine koduna çevirmez, kendisi yorumlar.

# JIT (Just-in-time) compile
Interpreter'lar gibi runtime sırasında kodu alır fakat Interpreter'lardan farklı olarak; okuduğu kodu makine koduna çevirir ve o kodu CPU'da işletir.

- JIT programlama dilinin özelliği değildir, runtime'da kodu işleten programın özelliğidir.

- JVM güncel sürümlerinde bu özelliği kullanılır.

# Ahead of time (or AOT) compiler
Ahead of time kelime anlamı: Vaktinden önce

C, C++ dilleri direk olarak makine koduna çevrildiğinden, runtime sırasında derleme işlemine gerek kalmaz ve bu sebepten daha hızlı çalışır.  bu şekilde direk makine koduna çevren compiler'lara AOT denir.

Aslında programlama dilleri JIT ve AOT'den bağımsızdır. İstersek özel bir derleyici geliştiririz. Bu derleyici, C++ kodunu, JVM'in yaptığı gibi runtime'da parse edip işletir. Yani JIT çalışır.

# betik dili (or script or scripting language)
Betik dili; bir environment üzerinde manipülasyon yapabilen dillerdir. kaynak: (source-id: 74) "4 Overview" başlığı 2inci paragraf.

örnekler:
- ECMAScript: web sayfası ortamında HTML CSS'lerde manipülasyon yapabilmektedir. ECMAScript ilk zamanlar, scripting language olmak için tasarlanmıştı. fakat daha sonra genel amaçlı oldu. (source-id: 74) "4 Overview" başlığı 2inci paragraf.
- Shell (Bash, Zsh...): OS ortamında manipülasyon yapabilmektedir

Bir dili tamamiyle script dili yada script dili olmadığı söylemek çok zor. kaynak: (source-id: 423) "Stavros Macrakis (macrakis@osf.org) replied" başlığı son paragraf. + (source-id: 424) "Description" başlığı 4üncü paragraf.

# compiled language
interpreter'e ihtiyaç olmayan ve direk makine koduna çevrilen dillerdir.

# Bytecode (or portable code or p-code)
yorumlayıcı gerektiren dillerde yorumlayıcının okuduğu dosyadır. JVM için .class dosyalarıdır.

# Microcode
işlemcinin kendi içinde kullandığı instruction'lardır. işlemciye dışarıdan makine kodu gelir, işlemci bu makine kodlarını microcode'lara çevirir.

# Java Bytecode
Java kodları (.Java uzantılı dosyalar) derlenir ve Bytecode'a çevrilir. Java için Bytecode .class dosyalarıdır.

Scala, Kotlin, Clojure ve Groovy, Java diline alternatiftir ve derlendiklerinde Java'da olduğu gibi Bytecode'a (.class dosyalarına) çevrilirler. Yani JVM'de run edilmek için tasarlanmışlardır.

# assembly language (or assembler language)
- En düşük seviyeli dildir.
- Makine kodu; 0 ve 1'lerin okunabilir hale getirilmesi için kullanılır. tamamı olmasa da assembly, neredeyse 1:1 mapping yapılarak makine koduna çevrilebilir. Birebir çevrilmeyenlerlerin bazıları:

  - jump labels
  - definition of the new constant
  - the keyword to define the sections (how machine code should be packed into sections in an output object file)

  Yukarıdaki bu değişebilen kısımlar her derleyici (asembler) tarafından farklı syntax veya keyword'ler ile yapılıyor olabilir.

- Microsoft Windows (veya diğer işletim sistemleri) üzerinde çalışan .Exe (veya diğer executable) uzantılı dosyalar, hem makine dilinden hemde işletim sisteminin anlayacağı kısımlar içerir. Bu sebeple; C'de yazılmış basit bir satırlık printf satırlık kod, Linux'ta derlenirse FreeBSD'de, MacOS'te or Microsoft Windows'ta çalışmamaktadır. Executable formatı zaten her işletim sisteminin farklıdır.
  - Linux'un executable formatı: __Executable and Linkable Format (or ELF or Extensible Linking Format)__. dosya uzantıları: .axf, .bin, .elf, .o, .prx, .puff, .ko, .mod, .so
  - Microsoft Windows'un executable formatı: __Portable Executable (or PE)__. dosya uzantıları: .acm, .ax, .cpl, .dll, .drv, .efi, .Exe, .mui, .ocx, .scr, .sys, .tsp

  Zaten executable dosyaların formatları aynı olsa bile, sistem kütüphanelerinin ismi, API'leri, çağrılan API'lerin return kodları, dosyaların dizinleri farklı olacağından yine runtime'da sorun yaşayacaktık.

- Aynı işletim sistemine sahip 2 işletim sistemi olsun: Debian, Suse. Debian üzerinde bir kod derledik. Eğer kod içerisinde Suse'de olmayan bir kütüphaneye link var ise, uygulama run edilir fakat runtime sırasında hata alınır.
- Compiler'ın yaptığı şey: kodları mapping yaparak bir dosyaya aktarmak (executable yaratmak). Bu sebeple Microsoft Windows executable'ı, Linux üzerinde derleme işlemi yapılarak oluşturulabilir. Örnek bunu yapan compiler: mingw.

# machine code (or makine kodu) vs assembly vs instruction

|                     | karşılığı  |
|---------------------|------------|
| machine code        | 1010100000 |
| Assembly            | ADD 21     |
| instruction name    | ADD        |
| instruction operand | 21         |

instruction direk CPU'nun işleteceği komuttur. dışarıdan CPU'ya iletebilen en düşük seviyeli istektir.

__instruction set__ bir CPU'nun kabul ettiği tüm instruction'ların kümesidir.

Example code:

```assembly
INC COUNT        ; Increment the memory variable COUNT

; comments should be start with semicolon

MOV TOTAL, 48    ; Transfer the value 48 in the
                 ; memory variable TOTAL

ADD AH, BH       ; Add the content of the
                 ; BH register into the AH register

AND MASK1, 128   ; Perform AND operation on the
                 ; variable MASK1 and 128

ADD MARKS, 10    ; Add 10 to the variable MARKS
MOV AL, 10       ; Transfer the value 10 to the AL register
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# WebP

Google'nin geliştirdiği resim formatı. Daha çok web sayfalarında gösterilmesi için geliştirilmiştir. bu sebeple; hızlı yüklenme ve az boyutlu olacak şekilde tasarlanmıştır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# .tar formatı

Tarball adı verilen bu dosyalar sadece arşivlenmek amaçlı kullanılmaktadır. UNIX sistemlerinde ki dosyalar için, dosya sistemindeki bilgilerini de .tar dosyası içerisinde saklamaktadır. oysa Zip gibi formatlarda sadece Microsoft Windows dosya sistemindeki bazı bilgiler saklanmaktadır. tar formatı sıkıştırılmadığından, genelde ek araçlarla sıkıştırılır ve dosya uzantısının yanına bir uzantı daha eklenir yada dosya uzantısı tamamen değiştirilir.

# ISO formatı

ISO formatı bazı dosya sistemlerinin partition bilgileri de tutmak için kullanılmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Microsoft Windows işletim sistemi dizin yapısı

> RECYCLED

> RECYCLER

> $Recycle.Bin

işletim sistemi versiyonuna göre ismi değişmektedir. bölümün root dizinindedir. çöp kutusu aslında buradadır. Microsoft Windows çöp kutusu buradaki tüm dosyaları tek klasördeymiş gibi gösterir.

> System Volume Information

NTFS dizinlerinde root'ta bulunur. bu dizin içerisinde system restore bilgilerini, hızlı arama (index) için bilgileri, kısayol ve link edilmiş tipteki dosyaların bilgileri vs gibi bilgileri tutar.

> Program files

"C:\Program files (x86)"" 32 bit uygulamaları barındırırken, 64 bit uygulamalar "C:\Program files" dizinindedir.

> C:\ProgramData

tüm kullanıcılar için ortak data saklayabileceği dizindir. uygulamalar alt klasörler yaratarak dosya saklayabilirler.

> C:\Documents and Settings\username

Microsoft Windows 2000, XP and 2003 için home dizini

> C:\Users\username

Microsoft Windows Vista, 7, 8 and 10 için home dizini

> C:\Users\AllUsers

C:\ProgramData dizine kısayoldur.

> C:\Users\Default

Microsoft Windows yeni kullanıcı açtığında buradaki dosyaları yeni user'a aktarır.

> C:\Users\Public

Tüm kullanıcılara ve network'teki kullanıcılara açık olan dizindir. kolay dosya paylaşımı yapabilmek için kullanılmaktadır.

> C:\Users\UserName\AppData\Roaming

Microsoft user synchronizing feature sync only these files.

> C:\Users\UserName\AppData\Local

ve

> C:\Users\UserName\AppData\LocalLow

app can store files here the files which will not sync with their Microsoft Windows account

> C:\Windows\System

16 bit dll dosyalarını bulunduruyor.

> C:\Windows\System32

32 bit dll dosyalarını bulunduruyor.

> C:\Windows\SysWOW64

64 bit dll dosyalarını bulunduruyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# SerDes
Serializer ve Deserializer aynı modülde olduğunda, o modüle bu kısaltma konulmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ISO (or International Organization for Standardization)
Teknik ve teknik olmayan konularda standartlar belirleyen komisyon. Sadece __IEC (or International Electrotechnical Commission or Uluslararası Elektroteknik Komisyonu)__'in ilgilendiği konularda standart belirlemiyorlar.

ISO terimi, kurumun başlığının kısaltması değildir. Yunanca'daki "isos" kelimesinden esinlenilmiştir: kaynak: (source-id: 76) book: "The Illustrated Network", yazar: Walter Goralski, "ISO, or International Standards Organization" ilk paragraf.

# ISO Standart isimlendirmeleri
ISO standartlarındaki dökümanlar özel olarak isilendirilmektedir. Örnek: ISO/TS 16952-1:2006. Bu örneğin parçalarını incelersek:
- "TS" terimi "Technical Specification" olduğunu belirtiyor. ISO standartlarında bu gibi birçok çeşit içerik tipi mevcut: Technical Reports (TR), Publicly Available Specifications (PAS)...
- -1 terimi standardın birinci parçasını temsil etmektedir.
- :2006 terimi bu dökümanın 2006 yılında yayımlandığını belirtiyor.

Bu örnekte olmasa da, ISO/IEC gibi terimlerde görebiliyoruz. Bu durumda bu standardın 2 farklı bir organizasyon ile birlikte geliştirildiğini belirtiyor. Yukarıdaki örnekte, IEC kurumu ile birlikte geliştirildiğini belirtmiş olduk.

# Request for Comments (or RFC)
is a type of publication from the __Internet Engineering Task Force (or IETF)__ and the __Internet Society (or ISOC)__ which includes contents about Internet.

RFC dökümanları güncellenebilir veya ilgili dökümana ekleme yapılabilir. Eğer yeni RFC eskisini update ediyor ise, yeni RFC eskisini "update" ediyordur. Eğer eski döküman geçersiz olmuşsa, yeni döküman eskisini "absolute" ediyordur. kaynak: (source-id: 340) "12. Relation to other RFCs" ilk paragraf.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# mantıksal analiz robotu (or yapay zeka or Artificial Intelligence or AI)
bir bilgisayarın, gerçek bir düşünebilen canlı gibi davranması (insan zekasını taklit edebilme) yeteneğidir. Bu terim kullanılırken arkaplanda bu işin nasıl yaptığı ile ilgilenilmez.

# yapay sinir ağı (or YSA or Artificial neural network or ANN)
YSA, "yapay zeka" yapabilmek için bir mimaridir/yöntemdir. yapay zeka için YSA kullanılmak zorunda değiliz. onun yerine eski oyunlarda olduğu gibi, araba yarışı tasarlayabilir ve diğer yarışçıların da akıllı davranabilmesini sağlayabiliriz.

# Transformer
YSA'nın özel bir türevidir.Bu model yapay zeka dünyasında en önemli dönüm noktalarındandır.

# Makine öğrenimi (or machine learning or ML)
yapay zekanın kendini geliştirmesidir. Yapay zeka bir girdiye karşılık bir çıktı verebilir. Fakat kendini eğitebilmesi konusu ayrıdır ve "Makine öğrenimi" konusuna girer.

# Artificial general intelligence (or AGI)
bazı kaynaklarda şu şekilde de geçmektedir:
- strong AI
- full AI
- human-level AI
- general intelligent action

Fakat "strong AI" terimi farklı anlamlara da gelmektedir. Bu sebeple karışıklığa sebep oluyor.

Bu zeka; insan beyninin mantığında çalışan, sadece 1 konuya değil, fakat genel olarak herhangi bir komnuya göre kendini geliştirebilir durumdadır. Yani sadece banka uygulamasında çalışacak bir chatbot'un AGI olmasına gerek yoktur. Fakat cep telefonlarındaki asistan genel olarak herşeyden sohbet edebilecğei için AGI olması beklenir.

# Generative AI (or Generative artificial intelligence or generative AI or GenAI or GAI or üretken yapay zeka)
yeni şeyleri kendi üretebilen zekalardır.

# Dil modeli (or Language Model or LM)
genel amaçlı olarak dili anlama ve üretme becerisiyle öne çıkan bir dil modeli türüdür.

Burada "model" terimi, yapay zekanın ağının yapısının modeline referans eder. Yani yapay zeka ağının nasıl olduğuna referans eder.

__Large Language Model (or LLM)__ çok büyük miktarda datalar üzerinde eğitilip, input olarakta alabilen LM'lerdir.

# markalar ve ürünler

__TensorFlow__ - makine öğrenimi yapmak için gerekli kütüphanedir.

__GPT__ - __OpenAI__ firmasının geliştirdiği yapay zeka.

__ChatGPT__ - __OpenAI__ firmasının geliştirdiği chatbottur. Arkaplanda GPT'den yararlanır.

__DALL-E__ - __OpenAI__ firmasının geliştirdiği yazılımdır. metinden çizim oluşturmaktadır.

__GPT4All__ - github'da geliştirilen GPU ihtiyacı olmadan standart makinelerde yapay zekanın çalıştırılmasını sağlayan proje.

__Llama (or Large Language Model Meta AI)__ - Meta firması tarafından geliştirilen yapay zeka modelidir.

__ollama__ - github'da geliştirilen farklı LLM'leri çalıştırabilmek için gerekli bir altyapı. Bu altyapı ile yazılan LLM'ler hem daha hılzı geliştirilebiliyor (çünkü onları destekleyici framework mevcut) hemde dışarıya açtıkları API'ler vs ortak oluyor. yani ollama tek başına bir işe yaramıyor, JVM gibi başka processleri işletiyor. ollamanın ortak API sundurduğunu buradkai kütüphaensindeki kod örneğinden anladım: https://github.com/ollama/ollama-js

- bazı LLM'ler: https://github.com/ollama/ollama?tab=readme-ov-file#model-library

- bazı UI yazılımları: https://github.com/ollama/ollama?tab=readme-ov-file#web--desktop

# NLP (or Natural Language Processing or Doğal Dil İşleme)
başka başlıkta anlatılmaktadır.

# derin öğrenme (or deep learning or deep structured learning or hiyerarşik öğrenme)
makine öğrenmesinin bir alt dalıdır. çok katmanlı YSA'ların kullanılmasıdır. çok katmanlı YSA'lar ile birlikte her level (katman) kendi içinde de YSA'lar barındırmaktadır. örnek: araba kullanan robot yapacağız. fonksiyonumuz araba kullanmak. fakat alt fonksiyonlarımız var: direksiyonu yönetme, acil durumlarda arabayı tamir etme gibi... İşte bu katmanlar ayrı ayrı birleştirilmektedir.

# Bulanık mantık (or bulanık eseme or puslu mantık or Fuzzy logic)
Bulanık mantığın temeli bulanık küme ve alt kümelere dayanır. Klasik yaklaşımda bir varlık ya kümenin elemanıdır (1) ya da değildir (0). bulanık mantık ile bir varlık üyelik derecesine göre birden fazla kümenin elemanı olabilir. üyelik derecesi 0-1 arasındadır. örnek olarak sıcaklık derecesi verilebilir. belirli bir seviyeden sonrasını sıcak belirli bir seviyeden sonrasını soğuk olarak ele alırsak; ılık sıcaklıkları tanımlayabiliriz.

# Sinaptik (or Sinaptic)
Sinaps (or Synapse) kelimesinde geliyor. Sinaps; nöronların (sinir hücrelerinin or neuron) diğer nöronlara ya da kas veya salgı bezleri gibi nöron olmayan hücrelere mesaj iletmesine olanak tanıyan özelleşmiş bağlantı noktalarıdır.

# YSA yapısı
Girişler her __hücreye (or node or düğüm)__ geldiğinde __ağırlıklar (or W or Weight)__ ile çarpılır. Her girişten gelen aynı derecede önemsenmeyeceği için bu çarpım işlemi yapılır.

Bütün girişler çağrıldıktan sonra sinir içerisinde bir matematiksel işleme tabi tutulurlar. Bu işlem "toplama fonksiyonu" olarak adlandırılır. Burada isim karışıklığı olabilir. Bu metot sadece toplama çarpımı değildir. Herhangi bir matematiksel işlem olabilir.

Çıkan çıktı ise en son yine bir matematiksel işlemden geçer. En sonki bu işlem çıkış için bir filtre görevi görür ve aktivasyon fonksiyonu ismini alır. Burada da isim karışıklığı olabilir. "aktivasyon fonksiyonu" matematikteki özel bir fonksiyona denk geliyor. Fakat YSA'da bu fonksiyon herhangi bir işlem olabilir.

# YSA Katmanlar (or Layers)

YSA'nın ilk girdi aldığı hücreler ve son çıkışa giden hücreler arasında da hücreler olabilir. aradaki katmanlara (ilk ve son katmanlar hariç) gizli katmanlar (or ara katmanlar) denir. Dolayısı ile gizli katmanlı olan bir ağ, en az 3 katmandan oluşuyordur.

çok katmanlı YSA en az 2 katmandan oluşur.

bir YSA en az 1 katmandan oluşabilir.

# Ağın Eğitilmesi

YSA'nın eğitilmesi için doğru olarak bilinen giriş ve çıkış değerlerinin elimizde olması gerekir. YSA'ya girdiler verilir ve YSA'nın verdiği çıktı ile gerçek çıktı karşılaştırılır. İşte bu hata payına göre, __ağırlık__ değerleri her işlem sonrası değiştirilir. Bu eğitim şekline __geri beslemeli (or geri yayılım or backpropagation)__ eğitim denir. Birçok farklı __eğitim (or training)__ şekli düşünülebilir.

Ağa ilk W değerleri ya rastgele yada belli referanslara dayanılarak ta atanabilir.

Test süreçlerinde YSA'nın eğitilmemesi gerekmektedir.

# Hopfield Ağları (Hopfield Net)

Aşağıdaki kurallara uyan sinir ağlarına verilen özel isimdir. Bunun gibi birçok ağ mevcuttur.

- Tüm hüclerelerde sadece 1 ve 0 değerleri giriş çıkışlarda olmalıdır

- Nöronlara gelen ağırlıklar aktivasyon fonksiyonuna gelmeden hemen önce toplanırlar

- Nöronlarda aktivasyon fonksiyonu olmalı ve eşik değeri kullanılmalıdır

Katman kavramı yoktur. Tüm hücreler diğer tüm hücrelere bağlıdır. Yani aslında tek katmanlıdır diyebiliriz. Böylece tüm girişler (hücreler) aynı zamanda çıkışları verecektir. n girişli ve n çıkışlıdır. Her bir hücre çıkışı, bir sonraki iterasyonda diğer tüm hücrelerin girişine etki etmektedir.

# iterasyon vs epoch

epoch'un Türkçe kelime anlamı: devir, çağ, dönem'dir.

iterasyon YSA'nın bir kere input alıp, bir kere çıktı vermesine kadar olan süreçtir.

YSA'lar eğitilirken bir eğitim seti (giriş ve çıkış seti) olmaktadır. Bu tüm set YSA'ya verilir ve YSA kendini sonuçlara göre eğitir. Tüm eğitim seti dolaşıldığında bir epoch süresi geçmiş sayılır. YSA eğitiminde bazen epoch sayısını (aynı eğitim seti için) birçok kere geçmek gerekebilir.

# Basamak (or adım or step or staircase) fonksiyonu

Belirli aralıklarda sadece 1 sayıyı çıktı olarak veren fonksiyonlar grubudur. Örnek; "Heaviside step function", "signum", "Constant" bu kümenin içindedir.

# Sigmoid fonksiyonu (simge: S)

bu fonksiyon her girdi için 0 ile 1 arasında değer üretir.

f(x)= 1/1+e^(-1)

# Eşik Değer (treshold) fonksiyonları

bu bir fonksiyon grubudur. Sigmoid ve basamak fonksiyonları grubun içindedir. eşik değerlere göre sonuç verirler.

# Birim fonksiyon (or özdeşlik fonksiyonu or özdeşlik gönderimi or özdeşlik dönüşümü or birim dönüşüm or birim işlev or unit)

f(x) = x

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# OpenStreetMaps (OSM)

Açık kaynaklı dünya haritası veritabanıdır. Forsquare gibi firmalar bu veritabanından yararlanır. sadece veritabanı sunduğundan, API'ler ancak üçüncü partiler tarafından yayınlanır.

OpenstreatMaps OSM'nin içindedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Döküman yönetim sistemleri

| name       | comment                                                                                                                       |
|------------|-------------------------------------------------------------------------------------------------------------------------------|
| Confluence | Atlassian'ın geliştirdiği wiki tool'u                                                                                         |
| MediaWiki  | Wikipedianın altyapısını sağlayan wiki tool'u. bu wordpress gibi hazır bir yapı. direk Wikipedia gibi site oluşturulabiliyor. |
| XWiki      | open source                                                                                                                   |

# CI sistemleri

- Bamboo

Atlassian şirketinin geliştirdiği CI (Continuous Integration) tool'u.

Agent dediğimiz uygulamalar, Bamboo'ya bağlanıyor. Bamboo bir task yapmak istediğinde, agent makineleri (agent'ın home klasörü ve her bir task için yaratılan workspace) üzerinde çalıştırıyor ve sonucu alıyor. Bamboo bulunduğu makinede bir işlem yapmıyor. Herşey agent'larda yapılıyor. Bu agent'lar; Amazon sunucusunda, Docker'da yada aynı makinedeki birden fazla işletim sistemi user'ında olabilir.

Bamboo'daki script'ler büyükten küçüğe şu şekilde gruplandırılıyor: Plan -> Stage -> Job > Task

- alternatifler: Jenkins, Hudson, GitLab

# SCM clients and server based systems

- alternatifler:

| name                                                | comment                                                           |
|-----------------------------------------------------|-------------------------------------------------------------------|
| Atlassian BitBucket                                 | Server                                                            |
| Gitlab                                              | Server (ekstra CI özellikleri içeriyor)                           |
| EGit                                                | Eclipse plugin (default installed)                                |
| Atlassian SourceTree                                | Client Desktop                                                    |
| SmartGit                                            | Client Desktop                                                    |
| GitHub                                              | web site (Proprietary software)                                   |
| Git-GUI                                             | Default Git's Client Desktop without history/log viewer           |
| Gitk                                                | Default Git's Client Desktop only history/log viewer              |
| CA Harvest Software Change Manager (or CCC/Harvest) | develop by Microsoft                                              |
| Bazaar (or Bazaar-NG)                               | Server                                                            |
| Subversive                                          | SVN Eclipse plugin (default installed on old versions of Eclipse) |
| TortoiseSVN                                         | SVN Desktop GUI Client                                            |

# SCM, CI gibi bir yazılımın tüm hayat döngüsünü sağlayan paket yazılımlar

- __Sourceforge__

- __Launchpad__

- __Azure DevOps Server (or old-name:Team Foundation Server or old-name:TFS or old-name:Visual Studio Team System or old-name:VSTS)__

  Hem lokalde hemde Azure'de yayımlanan sunucu versiyonları mevcut. Kendi SCM protokolünü olan __Team Foundation Version Control (or TFVC)__ ve/veya "git" protokolünü de desteklemektedir. TFS sadece SCM değil, aynı zamanda birçok işlevi CI, Proje yönetimi, test hayat döngüsü gibi de içermektedir. Bir yazılım için gerekli tüm hayat döngüsünü sağlayacak altyapıya sahiptir.

- __GitLab__

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# plug-in (or plugin or add-in or addin or add-on or addon or extension or uzantı or eklenti)

Hepsinin kelime anlamı aynıdır. Yazılım dünyasında işlevlerin farklılığını belirten bir standart yoktur. Her yazılım, ona yazılacak eklentinin çeşidini kendi rastgele karar vererek isimlendirir. Örneğin Firefox; eklentilere add-on derken, Chrome extension diyor. fakat iki tarayıcıda Adobe flash tipindeki eklentiler için plug-in kelimesini kullanıyor.

Uzantı kelimesi farklı anlamlara da gelebiliyor. Uzantı bir dosya formatının, dosyadaki kısa adıdır. Bazı yazılımların eklentilere uzantı demesinin sebebi de burada geliyor. Çünkü eklentilerin setup dosyalarının uzantıları, o yazılıma eklenti kurmaya yarıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# OS/2

IMB ile Microsoftun çok eskiden birlikte geliştirdikleri işletim sistemi. Daha sonra sadece IBM geliştirmelere devam etmiş, fakat sonrasında IBM'de geliştirmeyi tamamen kesmiştir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Critical section
Paralel execute edildiğinde dead-lock olabilecek bölgeye verilen özel isim.

# race condition
birden fazla thread'in olduğu yazılımlarda aynı kaynaklara eriştiklerinde bazen sorunlar olabiliyor. aynı anda bilgiyi değiştirdiklerinde diğeri bunu görmüyor vs.. Fakat bu durum her test yapıldığında olmuyor. çok nadiren yada her zaman olabiliyor. bu tarz hatalarda kodun, race condition'ı sağlanmadığı söylenir.

# Mutex (or Mutual Exclusion or Karşılıklı dışlama) vs Semafor (or Semaphore)
Semafor kelime anlamı: ulaşımda kullanılan işaret

Mutex sadece 1 işlemin o anda belirtilen kod bloğu içerisinden geçmesini sağlar. Oysa semaforlarda bu limit 1 den fazla olabilir. Semaforlarda örneğin en fazla 10 kişi sunucuya bağlanacak diğerleri sırada bekleyecek denilebilir.

# binary Semaphore vs counting Semaphore
binary; mutex gibi sadece 1 işlemin o anda belirtilen kod bloğu içerisinden geçmesini sağlar. counting Semaphore'da bu sayı birden fazladır.

binary Semaphore ve mutex aynı işi görür. fakat yapısal olarak farklıdırlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# JNI (or Java Native Interface)

Java ile native kod'u çağırma ve native kod'dan Java'yı çağırma işlemlerinin yapılabilmesini sağlar. aşağıdaki örnekte görüldüğü gibi navite keywordu ile başka dilden metot çağırmaktayız.

```java
public class Native {

        static {

                System.loadLibrary("native_library");

        }

        public static native int sum(int x, int y);

        public static void main(String[] args) {

                System.out.println(sum(3, 5));

        }

}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Inter-process communication (or IPC)

İşletim sistemleri üzerinde yürüyen işlemlerin birbirleri arasında haberleşme (uzak metot çağırma işlemi dahil) mekanizmasıdır.

# DBus (or D-Bus)

Açık kaynaklı bir IPC kütüphanesidir. Birçok implementasyonu ve default bir implementasyonu vardır.

# Unix-domain socket

IPC için uygulamalar birbirlerine veri transfer ederken; örneğin; localhost:7011 üzerinden de haberleşebilir. fakat bu best practice'lere uymaz. 3 sebepten;

- TCP/IP soket kuralları ile bu işin olması tercih edilmeyebilir

- 7011 portuna herkes istek yapabilir. bu güvensizdir.

- IPC metotları localhost'ta daha hızlı çalışır. localhost/loopback işlemleri zaman kaybettirebilir.

IPC işlemlerinde UNIX-like'larda "UNIX-domain soket" kullanılır.

# Dbus

her yazılım kendi içinde dışarıya birçok path (servis grubu açabilir). her path tüm işletim sisteminde tekil olmalıdır. bu path'lerin her birine "bus" denir. örnek:

> /com/appname/account

yukarıda account servisi var. bu servis içinde birçok parametre alan metot olabilir.

örnek Java d-bus kodu:

```java
import org.freedesktop.dbus.DBusInterface;

public interface ReceiveInterface extends DBusInterface {

   public int echoMessage(String str); // "member" terminolojik adı.

}
```

main app code:

```java
try {

    DBusConnection conn = DBusConnection.getConnection(DBusConnection.SESSION);

    conn.requestBusName("com.myapp"); //bu tüm OS'ta unique olmalı. bu değeri d-bus kendi içinde kullanıyor.

    conn.exportObject("/com/myappname/accounts", new JavaReceive()); // we export a service here. JavaReceive burada "object" deniliyor.

} catch (DBusException e) {

    e.printStackTrace();
}
```

send signal:

```java
Message message = new Message("/remote/object/path", "MethodName", arg1, arg2);

Connection connection = getBusConnection();

connection.send(message);
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# aspect ratio (or çerçeve oranı)
bir görüntünün eninin yüksekliğe oranıdır. width:height şeklinde ifade edilir. örnekler:
- 16:9 --> 16 birim en, 9 birim yüksekliktir.
- 1.85:1 --> İngilizce'de nokta karakteri ondalık ayraç olduğu için bu ifade şu anlama geliyor: en, yükseliğin 1.85 (yaklaşık 2) katıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Oyun motoru (or game engine)

oyun geliştirmek için DirectX, OpenGL gibi framework'lerin üzerine daha hazır API'ler sunarak, oyun yazılımını hızlandırmak amaçlı yazılmış kütüphanelerdir. Game engine'ler içerisinde audio engine, fizik motoru, yapay zeka gibi API'ler bulundururlar.

# rendering engine vs game engine

Oyun içerisindeki modeller (insan, araba gibi) modelleri ve API'leri oyun motorları bulundurur. Fakat bunları derleme işlemleri için render engin'e ihtiyaç vardır. Render engin OpenGL, DirectX olabilir fakat gömülü bir render'da içerebilir, yada belirli kısımları kendi derleyip bazı kısımları cross-rendered da tasarlanmış olabilir.

# Unreal vs Unity vs Source

Cross platform (çapraz platform) kapalı kaynak oyun motorlarıdır. Source Engine, Valve firması tarafından geliştirilmekte ve Counter-Strike, Half-life gibi oyunlarda kullanmıştır.

# Unity Web Player

Geliştirilesi durdurulmuş bir tarayıcı eklentisidir. Bu eklenti ile Unity oyunları  tarayıcı üzerindeki sayfalardan direk oynanabilmektedir.

# Quake

Quake oyununda kullanıldığı için ismi aynı olan, açık kaynaklı oyun motorudur.

# Render Engine

Önceden tasarlanmış modelleri işleyerek arayüze sunan uygulamadır. HTML rendering ve graphic rendering gibi birçok çeşit render sayılabilir.

# Blender

Açık kaynaklı 3d modelleme yapılması sunan tool'dur. "Blender Game Engine" de içermektedir. "Autodesk 3ds Max"'e alternatiftir.

# graphic engine

genel bir tanımdır. "Window system" yada OpenGL bu tanıma uymaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# on-screen display (or OSD)

bir yazılımın üzerinde açıldığı zaman ekranın her zaman önünde duran pencere yapısına verilen genel isimdir. örneğin; işletim sistemlerinde yada TV'lerde, ses açılıp arttırıldığında ekranda çıkan ses göstergesi OSD'dir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Opt-out vs Opt-in (or Double Opt-in)

izinli pazarlama (reklam mail/SMS'leri yapma) altındaki kavramlardır.

opt-out; kullanıcının onayı alınmadan reklam içerikli email gönderilebilir, ancak kullanıcı dilediğinde, çok kolay bir şekilde mail/SMS listesinden çıkabilir, anlamına gelmektedir.

opt-in ise; kullanıcıya önceden izni alınmadan hiçbir reklam gönderilemez. kullanıcı reklamları kabul ederse, tekrardan istediği zaman çıkabilmesi gereklidir.

opt-in ve opt-out terimleri en çok reklam SMS ve emailler için kullanılsa da, genel olarak herhangi bir 'hizmet/servis' içinde kullanılabilir.

- opt İngilizce'de 'tercih etmek' anlamına geliyor
- opt-out İngilizce'de 'ayrılmak/vazgeçmek' anlamına geliyor

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# screencast
__video screen capture (or screen recording)__ ile eş anlamlıdır.

# Chromecast
Google'ın geliştirdiği bir cihaz. USB bellek boyutundaki ufak cihaz, USB girişi şekilde HDMI girişi mevcut. HDMI girişini direk televizyona bağlıyoruz. Bu şekilde televizyonda akıllı-TV özelliği olmasa bile, Chromecast'in içerisindeki işletim sistemini HDMI aracılığı ile kullanmaya başlıyoruz. Cihazın USB girişi de mevcut. Bu girişi ile cihazı aynı zamanda paralelden elektrik alması için beslemek gerekiyor.

Chromecast içerisindeki işletim sisteminde uygulama çalıştırılmıyor. Chromecast ile aynı Wi-Fi'deki herhangi bir cihazın içerisindeki uygulama, Chromecast'e istediği ekran görüntüsünü ve sesi aktarıyor. Chromecast'e ekran görüntüsü atan yazılımlar: Android, iOS, Chrome-OS, Chrome eklentisi ile Google-Chrome.

"__Chromecast built-in (or old-name:Google Cast)__" chromecast cihazının için ekran görüntüsünü, uygulamadan, chromecast'e yollamak için kullandığı protokoldür.

# Miracast
screen mirroring standartıdır. Wi-Fi standartlarını belirleyen "Wi-Fi Alliance" organizasyonu tarafından yayımlanmıştır.

miracast standartını kullanan ürünler bunu etiket üzerinde belirtmek zorunda değil. bu sebeple her marka farklı şekilde bu standartı implemente eder. örnekler:

- LG's "SmartShare"
- Samsung's "AllShare Cast"
- Sony's "Screen mirroring"
- Panasonic's "Display mirroring"

Windows 10 ve 11 default olarak Miracast'i desteklemektedir.

# Wi-Fi Direct (or old-name:Wi-Fi Peer-to-Peer)
Modem'i aradan çıkartarak direk olarak diğer cihazlar ile haberleşmeyi sağlayan cihaz özelliği ve protokoldür. Bu özelliği kullanabilmek için Wi-Fi cihazımızın desteklemesi gerekmektedir.

Bu teknolojide modeme kesinlikle ihtiyaç yoktur (connection ilk initialize edilmesi için (başta) dahi yoktur).

Wi-Fi standartlarını belirleyen "Wi-Fi Alliance" organizasyonu tarafından yayımlanmıştır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Jasper Reports

Jasper report; raporun bir kere tasarlanması, çıktının ise dilediğiniz formatta (pdf, XML, CSV, txt, vs.), kodda veya tasarımda değişiklik yapmadan üretilmesini sağlayan sistemdir.

Raporlar belli bir formatta hazırlanır. Bu raporlar dilediğimiz formatta runtime sırasında çıktıyı verirler.

JasperReports ve iReports'un geliştiricileri daha sonra birleşerek JasperSoft isimli profesyonel bir şirket kurdular ve şu an Jasper Reports konusunda danışmanlık veriyorlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# bitwise operators
| Operator | Name                                                                   | Description                                         | Usage Example |
|----------|------------------------------------------------------------------------|-----------------------------------------------------|---------------|
| &        | AND (A·B or A∧B)                                                       | Sets each bit to 1 if both bits are 1               | 5 & 1         |
| \|       | OR                                                                     | Sets each bit to 1 if any of the two bits is 1      | 5 \| 1        |
| ~        | NOT                                                                    | Inverts all the bits                                | ~ 5           |
| ^        | XOR (eXclusive OR or EOR or EXOR or ⊕)                                | Sets each bit to 1 if they are different            | 5 ^ 1         |
| <<       | left shift (or zero-fill left shift)                                   | (read below)                                        | 9 << 1        |
| >>       | right shift (or signed right shift or arithmetic right shift)          | (read below)                                        | 9 >> 1        |
| >>>      | unsigned right shift (or zero-fill right shift or logical right shift) | (read below)                                        | 9 >>> 1       |

not: "left shift" ile "right shift" birbirinin tersi yönlü işlemler değildir. Aşağıda detayları açıklanmıştır.

Her programlama dili ve her veri yapısı signed bit'i (pozitif veya negatif için kullanılan bit) kullanmıyor olabilir. Bu sebeple her dilde bu başlıkta bahsedilen shift operatörlerinin tümü olmaz.

# XOR
Özellikle kriptografide XOR işlemini bolca duyarız. Sebebini direk örnek ile anlatalım:

```
X ^ Y = Z
ise;
Z ^ Y = X
Z ^ X = Y
```

Yani işlem kesinlikle güvenli şekilde tersine çevrilebiliyor. Fakat diğer logic işlemlerde (AND, OR...) bu şekilde geriye dönmek mümkün değil. Diğer logic işlemlerinde belki bazı sayılarda denk gelme olabilir, fakat XOR'daki gibi her zaman kesinlik söz konusu değildir.

# Left shift (<<) («)
C++'ta bu işaret 2 farklı amaç için kullanılıyor. bir tanesi burada yazan.

soldaki operatörü bit bazında sola doğru kaydırır. kaydırma işlemi; ikinci operatörde belirtilen sayı kadar gerçekleşir. bu durumda en sol'daki (leftmost) değerler yok olur (discard edilir). Sağda yeni oluşacak değerler ise "sıfır" değeri ile doldurulur.

Bu işlem Non-circular shifting'dir. Yani leftmost değer, rightmost'a transfer olmaz.

Bu işlem sonrası ilk-operator, ikinci-operator'ün iki katının üssü kadar artar. Yani; 6 << 3 = 6 * (2)^3'tür. (not: shift edeceğimiz operatör'ün max basamak sayısında değer var ise o zaman bu üs hesaplaması geçersiz olacaktır.)

# Logical right shift (>>>)
'Left shift'in sağ yöne olan tersidir.

Bu işlemin tersine ihtiyaç olmayacağı için (gereksiz olduğu için), <<< işlemi yoktur.

# Arithmetic right shift (>>) (»)
C++'ta bu işaret 2 farklı amaç için kullanılıyor. bir tanesi burada yazan.

\>>> işlemi ile çok benzerdir. Tek farkı, padding yaparken sıfır kullanmaz. Onun yerine 'most significant bit (or MSB)'i kopyalar ve padding olacak yerlere yapıştırır. "Two's complement" (başka başlıkta anlatılıyor) representation'una göre daha doğru sonuç verecek şekilde çalışmaktadır. Örnek:

-2,147,483,552 >> 4 = -134,217,722

(denk gelen aynı işlem: -2,147,483,552 / (2^4) = -134,217,722 )

Yani;

```
10000000 00000000 00000000 01100000 (-2,147,483,552) ("Two's complement" formatında)

11111000 00000000 00000000 00000110 (-134,217,722) ("Two's complement" formatında)
```

-2,147,483,552'da ilk bit negatif numarayı temsil ediyor. Bu sebeple; 4 adet "1" sol tarafa padding olarak geldi. Eğer pozitif sayı olsaydı, yani; 'most significant bit (or MSB)' "0" olsaydı, o zaman 4 adet "0" padding olarak gelecekti.

# all operators
| Operator                        | Symbols and examples                                                                  |
|---------------------------------|---------------------------------------------------------------------------------------|
| Post-unary operators            | expression++ , expression--                                                           |
| Pre-unary operators             | ++expression , --expression                                                           |
| Other unary operators           | - , ! , ~ , + ,                                                                       |
| Multiplication/division/modulus | * , / , %                                                                             |
| Addition/subtraction            | + , -                                                                                 |
| Shift operators                 | << , >> ,  >>>                                                                        |
| Relational operators            | < , > , <= , >= ,  instanceof                                                         |
| Equal to/not equal to           | == , !=                                                                               |
| Logical operators               | & (Bitwise AND) , ^ (Bitwise exclusive OR) ,  \| (Bitwise inclusive OR)               |
| Short-circuit logical operators | && (Conditional-AND), \|\| (Conditional-OR)                                           |
| Ternary operators               | boolean expression ? expression1 : expression2 (shorthand for if-then-else statement) |
| Assignment operators            | = , += , -= , *= , /= , %= , &= , ^= , \|= , <<= , >>= , >>>=                         |

# implementation of some functions for bit-wise operations

- returns the i'th bit of number

```java
int getBit(int num, int i) {
  int temp = num >> i; // shift the bit to first index.
  return temp & 1; // 1 = 00000001
}

// test
getBit(0b110, 1) == 1 // i=1 means the second index, 0 is the first index.
```

- set bit as 1

```java
int setBit(int number, int index) {
  return number | (1<<index); // returns the new value
}
```

- set bit as 0

```java
int clear(int number, int index) {
  int mask = ~(1 << index);
  return number & mask;
}
```

# ternary operator
ternary kelime anlamı: üçlü, üç parçadan oluşan

programlama dillerinin desteklediği bir syntaxtır. if bloklarını bu şekilde yazabilmemizi sağlar:

```js
var y = x === 4 ? `4'e eşit` : `4'e eşit değil`;
```

# unary operation
unary kelime anlamı: birli, sadece 1 parçadan oluşan

Sadece bir operand'a sahip olan operasyonlara unary operation adı verilir.

```c
++x;
x--;
```

# unary function
Sadece bir argüman alan fonksiyonlara unary function adı verilir.

```js
var func = x => x + 1;
```

# n-ary function and operations
- unary (or monadic)
- binary (or dyadic)
- ternary (or triadic)
- quaternary (or tetradic)
- Quinary (or Pentadic)
- Senary (or Hexadic)
- Septenary (or Hebdomadic)
- Octonary (or Ogdoadic)
- Novenary (or Enneadic or nonary)
- Denary (or Decadic or decenary)

şeklinde devam etmektedir.

birden fazla parametre alan tüm function'lara __Multary (or multiary or Polyadic)__ denir.

hiç parametre almayan fonksiyonlara __niladic function (or nullary function or 0-ary function or zero-airy function)__ denir.

# constant function (or sabit fonksiyon)
her zaman aynı değeri döndüren fonksiyondur.

# Arity
(bu kelimenin direk olarak Türkçesini bulamadım.)

matematik veya yazılım dünyasında; bir fonksiyona geçilen parametre sayısıdır.

# statement vs expression
Expression ve statement terimleri dilden dile değişiyor. kaynak: (source-id: 84) title: "1.4.1 Expressions", 1st paragraph.

Fakat genelde; "statement", kod dosyasında gördüğümüz her satıra denk gelmektedir. genelde noktalı virgül ile ayırırız. "expression" ise; en az 1 operator ve 1 value'dan oluşan parçadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Heroku
PaaS bir hizmettir. Heroku ortamında deploymentların hızlı şekilde yapılabilmesi için platform sağlamaktadır. Kodların otomatik olarak derlenip deploy edilmesi, dışarıya açılması gibi işlemler kolayca yapılabilmektedir.

# Ansible
Open source komut satırı uygulamasıdır. user'ın tanımladığı configurasyon dosyası okur ve ve bu dosyadaki yapılacak işlemleri remote makinelerde yapar. Bunu yaparken ansible remote makinelere SSH ile gider. Bu sebeple yötececeğimiz uzak makineye (__Managed node__'a), agent yazılımı kurmak gerekmez. bu sebeple __agentless__ mimarisi vardır.

"__Control machine__" uzak makinaları yönetecek olan (onlarda değişiklik yapacak olan) bilgisayarlara verilen isimdir.

Hem "Control machine" hemde "Managed node" tarafında, Microsoft Windows veya Unix-Like'lar desteklenmektedir.

# Argo CD (or ArgoCD)
"Kubernetes controller" (bir Kubernetes modülü) olarak Kubernetes'e kurulan yazılımdır.

GitOps için gerekli dosyalarımızı Git'e upload ederiz. Bu Git repository'sindeki değişiklikleri ArgoCD sürekli kontrol eder. Eğer değişiklik var ise, bu değişiklikleri Kubernetes'e uygular. ArgoCD sadece Kubernetes için tasarlanmıştır.

Git repo'muzdaki, GitOps dosyalarımız "Helm chart" veya farklı GitOps entegrasyon dosyaları olabilir. Farklı GitOps dosyaları (örnek: "Helm chart") için ArgoCD'ye ayrı plugin kurmak gerekebilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Amazon Elastic Compute Cloud (Amazon EC2)
Amazon firmasının, "Amazon Web Services (AWS)" isimli hizmet gruplarından bir tanesidir. Uzak masaüstü ile bağlantı yapılabilen sunucular (sadece makina) sunuyor. Bunlardan sistem gücüne göre saatlik ücret alıyor. Bu makineler üzerindeki HDD partition'larının backup'ları anında alınabiliyor (Amazon Machine Image - AMI) ve tekrar kullanılabiliyor - public olarak dağıtılabiliyor.

Amazona ücret ödendikten sonra verilen kullanıcı ve şifre ile uzaktan Amazon API'leri kullanılarak programatik olarak yeni makine yaratıp kapatılabiliyor. Bu şekilde gereksiz makinalar kapatılarak ücretten kar edilmiş olunabiliyor.

# Regions and Availability Zones

Amazon belirli bölgeler arasında sunucularını birbirinden bağımsız tutmuş (maliyet gibi sebeplerden). Bu sebeple bu bölgeler arasındaki makinelerdeki haberleşme normal internet üzerinden yürümekte, bu sebeple daha yavaş haberleşme olmaktadır. Backup alma işlemleri kısıtlı yürüyebilmektedir gibi.

# Elastic Beanstalk
AWS'nin bir alt hizmetidir. Heroku alternatifi bir sistemdir.

# Amazon networks:

büyükten küçüğe doğru:

- region (asia vs...): maliyetten düşülmesi için Amazon coğrafik konum olarak  yerlerde birbirinden bağımsız sistemler kurmuş. biz bunlar arasındaki sanal ortamlara erişmek istediğimizde, makinelerimizin public IP'lerimizi kullanmamız şart.

- VPC (or Virtual Private Cloud): Amazonun bulutta diğer network'lerden izole şekilde kullandırttığı network kümesidir. Amazon bulut makinaları subnet'ler altında gruplanır. Bu subnet'ler birbirine erişebilir. Fakat bu subnet'ler gruplandırıldığında VPC'ler altında toplanırlar ve birbirlerine erişemezler. VPC kısaltması birçok farklı ismin kısaltması olduğundan bazı yerlerde karışıklığa sebep olabilir.

- availability zone

- subnet

# Elastic IP

Elastic IP kullanıcının makineye atadığı public IP'dir. makine yeniden oluşturulduğunda değişmez. "public IP" ise her makine yeniden oluşturulduğunda değişir.

# security group

varsayılan olarak tüm makinelerin tü portları dışarıya kapalıdır. elle kullanıcı rule tanımlamalıdır. tanımlanan bu rule'lar gruplar altında toplanmaktadır. bu grubun ismi security group'tur.

# Network Interface

her makinada en az bir tane olmak üzere Network Interfaces attach olmuş olmalıdır. bu network kartı'dır. Bir makine yarattığımızda default Network Interface tanımlanır. bu arayüzü direk başka bir cihaza attach edebiliriz. default ismi eht0'dır.

# device farm

Amazonun mobil uygulamalar için test servisi. verilen testleri istenilen tüm cihazlarda koşmaktadır. sonuçları videolu şekilde kaydetmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DSL (Domain Spesific Language)

Kendi programlama dilinizi özel bir iş için geliştirdiğinize bu dil DSL olur. DSL sayesinde belirtilen syntax'ta ki yapılar, programlama diline çevrilir ve işlenir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Omniture vs Google Analitics vs CoreMetrics

Piyasada en çok tercih edilen istatistik toplama ve analiz etme sistemleri. Geliştirilen yazılımda, son kullanıcının yaptığı her aksiyon bu sitelere belirli formatlarda yollanıyor. Bu siteler ise, bu bilgilerle ilgili istatistikleri yazılımcılara sunuyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Xray

Test yönetimi sunan Jira eklentisi. ‘Test execution', ‘Test' gibi issue tipleri yaratıp bunları arayüzden düzenlemeyi sunuyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Selenium

web sayfaları testi için kullanılan yazılım paketleri.

# Selenium IDE:

Firefox eklentisi. Bu araç kullanılarak tarayıcı üzerinde elle yapılan işlemler script'lere dökülebilmektedir. IDE eklenti desteklemektedir.

# Selenium Client

Programlama dilleri için geliştirilen API. Test script'leri herhangi bir dilde yazılıp, execute edilebilir.

# Selenium WebDriver

API üzerinden yapılan istekler web-driver tarafından execute edilir (tarayıcıda çalıştırılır). Bu yüzden webdriver browser spesific yazılmaktadır. Selenium Web driver'ına gelen istekler sadece Selenium API ile olmayabilir. Aynı komutları kim gönderirse, uygun cevap dönülmektedir.

# HtmlUnit

Selenium'un geliştirdiği simülasyon tarayıcısıdır.

# SeleniumGrid (or Selenium Standalone Server)

birden fazla web driver olan test makinesi yönetimi içi kullanılan yapının genel ismidir. Java ile yazılmış bir uygulama mevcut. bu uygulama ya sunucu yada node rolünde başlatılabilmektedir (ikiside tek executable). sunucu bir adet olmalıdır (seleniumHub). node'lar ise istenildiği kadar olabilir ve sunucuya attach (register) olmaları gereklidir. Tüm test istekleri Hub'a gelir. Hub ilgili yönlendirmeleri uygun node'a havale eder. Bu şekilde birçok test paralelden çalıştırılabilir.

bir selenium node'unun konfigürasyonlarına webdriver dizini verilmelidir. bu şekilde ilgili node, hangi tarayıcıların driver'lerini çalıştırabiliyorsa, o testleri üzerinde yapmaya hazır şekilde bekler.

# Selenium RC

Eski sürüm selenium'larda kullanılırdı. WebDriver yerine bu vardı.

# SeleniumGridScaler

selenium-grid'in eklenti altyapısı mevcut. pom.xml'e selenium-grid eklenirse, bu sınıflardan yararlanarak bir Java projesi oluşturulursa bu Java projesi artık bir eklenti olmuş olacaktır. selenium-grid başlatıldığında, SeleniumGridScaler'ın install edilmiş hali (jar) class-path'e verildiğinde SeleniumGridScaler'ın kodları da yürütülmektedir.

SeleniumGridScaler eklentisi ile Amazon AMI'leri üzerinde node'lar sadece ihtiyaç olduğunda açık tutulurken, ihtiyaç olmadığında kapatılıyor. bu şekilde maliyet kazancı sağlıyor.

# RemoteWebDriver

webdriver'dan extend etmiş bir sınıftır. uzakta driver servlet'i var ise ona bağlanmak için kullanılır. örnek kodlardan daha kolay anlaşılabilir:

remote example:

```java
String Node = "http://localhost:4444/wd/hub";

DesiredCapabilities cap = DesiredCapabilities.chrome();

webDriver = new RemoteWebDriver(new URL(Node), cap);

local example:

System.setProperty("webdriver.chrome.driver", "/Users/murat/DEV/chromedriver");

webDriver = new ChromeDriver(); //Chrome driver'de webdriver'dan extend etmiştir.
```

# Jsoup
Java HTML parser'dır. dosya içeriği URL'den yada herhangi bir string olarak verilir, HTML'i parse etmiş şekilde okuyabilmemizi sağlayan API'ler sunar. arkaplanda headless web browser çalıştırmaz. bu sebeple JS motoru yoktur. bu sebeple sadece statik sayfalar parse edilebilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Gitweb

belirtilen dizinlerdeki Git projelerinin web tarayıcısından açılmasını sağlayan bir web arayüz projesidir. bu arayüzden commit'ler, log'lar, history'ler kolayca incelenebiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# CDN (or Content Delivery Network or İçerik Dağıtım Ağı)

CDN dünyanın bir çok yerine dağıtılmış sunucuların oluşturduğu bir alt yapıdır ve CDN'nin amacı ziyaretçilere, site içeriği en hızlı şekilde ulaştırmaktır. Bant genişlikleri, bölge bölge hız değerleri göz önüne alınarak dağıtım yapılır ve sunucular her tarafa uygun hızda iletim sağlamaya çalışır.

CDN hizmeti sunan birçok firma var. Tüm global dünyayı takip edip sunucuları uygun yerlere koyuyorlar.

CDN hizmetine atılan dosyaları çekmek daha hızlı olacağından;

```xml
<script src="js/jquery-2.0.2.min.js"></script>
```

yerine;

```xml
<script src="http://code.jquery.com/jquery-2.0.2.min.js"></script>
```

yazılması daha iyi olacaktır. Ziyaretçi önceden farklı siteden CDN li linki indirmiş ise tarayıcı cache'e dahi alacaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DOS (or Disk Operating System)
özel isim'dir.

bir işletim sistemidir. birçok firma tarafından farklı türevleri ve sürümleri çıkarılmıştır. FreeDOS (open source), MS-DOS gibi.

# MS-DOS (or Microsoft DOS)

MS-DOS 1980 yıllarında çıkmıştır. Microsoft, Microsoft Windows 2000 sürümü ile NT ailesine geçiş yapmıştır. NT, DOS çekirdeğini ortadan tamamiyle kaldırmıştır. NT öncesi, komut satırı DOS'un arayüzüydü (her işletim sisteminde olduğu gibi). DOS komutları ile aynı görevi yapan ve benzer isimlere sahip olan komutlar, NT sonrası da vardır. bu sebeple birçok kişi NT sonrası işletim sistemlerinde hala DOS'un komut satırı olduğunu sanmaktadır.

# Microsoft Windows NT ailesi

Microsoft Windows 3'üncü sürümü ile hem DOS hem de NT tabanlı sistemleri paralel geliştiriyor ve paketliyordu. NT tabanlılar sunucu ve kurumsal makinalara kuruluyor, DOS tabanlıları ise ev kullanıcılarına veriyorlardı.

# OS/2
Microsoft'un IBM ile birlikte geliştirdiği bir OS. DOS uyumlu olması için dikkat ediliyordu. 1990 ile 2006 yılları arasında geliştirildi ve kullanıldı. kaynak: (source-id: 85) "Microsoft + IBM == OS/2 ... briefly" başlığı.

# Microsoft Windows Me (or Microsoft Windows Millennium Edition)

Microsoft Windows 98 ile Microsoft Windows 2000 arasındaki sürümdür.

# Pocket PC

Microsoft'un geliştirdiği, cebe sığacak boyutta donanım cihazı. Üzerinde Microsoft Windows kurulu geliyor.

# Microsoft Windows Insider

Microsoft lisanslı Microsoft Windows ürünleri için isteyen kullanıcıları Microsoft Windows'un önceki sürümlerinin updatelerini alabilmelerini sağlamaya başladı. birçok Microsoft ürünü için "Insider" opsiyonu mevcut. "Insider" seçeneği aktif olan kişilere aynı anda güncelleme gelmiyor. bazı kullanıcılara bazı updateler çok önceden getiriliyor. düzenli şekilde gruplama mevcut.

# Microsoft Surface (or Surface RT)
Microsoftun donanımını da geliştirdiği, Microsoft Windows işletim sistemi Microsoft Windows yüklü gelen tablet cihazlar serisinin ismidir.

İçinde Microsoft Windows RT vardır. Microsoft Windows RT, Microsoft Windows 8.x'in arm için türevidir. Microsoft Windows RT sadece Microsoft Windows store uygulamalarını açabiliyor. daha sonra Microsoft geliştirmeyi kesti.

# MSDN
Microsoft'un sadece geliştiriciler için sunduğu web portal.

# TechNet
Microsoft'un tüm kullanıcılar için sunduğu web portal. örneğin visual studio ide ile ilgili indirmeler yada dökümanlar burada bulunmaz.

# RTM (or released to manufacturers)
Microsoft Windows ürününü piyasaya sunmadan son halini bazı gruplara açar. bu gruplar özellikle akademik ve donanım üreticileridir. bu şekilde piyasaya Microsoft Windows dağıtılmadan hazırlıklar yapılır. bu dağıtılan sürüm RTM'dir.

# Microsoft Windows version history
Burada hem server hemde masaüstü PC'ler için; Microsoft Windows version, Codenames, Release date, Release version, Editions, Latest build, Support status (date) olarak detaylı tablo mevcut: https://en.wikipedia.org/wiki/List_of_Microsoft_Windows_versions "Personal computer versions" ve "Server versions" başlığı altındaki tablolar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Gerçek Zamanlı İşletim Sistemleri (or Real time operating system or RTOS)

Üzerinde çalışan uygulamalar (servisler ve yazılımlar vs..) belirli bir süre içerisinde belirli bir işe cevap vermek zorundalardır. bu durumun oluşabilmesi için; işletim sistemi gerektiğinde, kendisinden daha öncelikli işlemlere öncelik verebilmesi gerekmektedir. aynı zamanda process'ler arası çalışmalarda öncelik yönetiminde, normal işletim sistemlerine göre daha çok seçenek sunmaktadır. bu tarz yönetimlere destek veren işletim sistemlerine RTOS denir. CPU önceliklendirilmesi özel olduğundan, bu işletim sistemlerinin sundukları API'ler, arkaplandaki servislerde buna göre yazılmıştır.

Genellikle RTOS'larda donanıma ve sürücülere bağımlılık yüksektir.

Örnekler:
- Quantum Software Systems OS (or QNX)
- Microsoft Windows CE
- RTLinux (or Real-time Linux)

Masaüstünde kullandığımız OS'lara "GPOS (or Normal General Purpose Operating System)" denir.

# monolithic kernel
Linux'un kullandığı çekirdek tipidir. tüm sistem tek bir ana process'te işletilir yani tek bir yapıya gömülüdür. u sebeple OS driver'larında veya dosya sisteminde değişiklik gerektiğinde OS restart edilmelidir.

kernel'da gömülü olan modüller: System calls, file system, virtual memory management, device drivers, IPC, CPU-process scheduler...

# Microkernel
sistem daha modüler yapıdadır. kernel modda çok az şey çalışır. tüm diğer modüller temel olarak farklı process'lerde çalışır. bu sebeple bir parça (örnek driver) hata verdiğinde ve restart istediğinde tüm sistem restart edilmek zorunda değildir.

"CPU-process scheduler" ve kernel process'i ile haberleşme amacı ile kullanılan IPC dışında hiçbir modül kernel'a gömülü değildir.

# hybrid kernel
bu tarz sistemlerde; bazı kısımlar çok bütünleşik iken, bazıları hiç değildir (modülerdir).

# embedded OS
gömülü sistem (or embedded system), donanımın değiştirilme gibi bir seçenek sunmadığı donanımlar ve üstünde çalışan yazılımlara verilen bir tabirdir. Bu cihazların gömülü olması, genel kullanıma hitap etmediklerinden kaynaklıdır. Aslında bakıldığında masaüstü bilgisayaralar ve laptoplar hariç tüm cihazlara gömülü diyebiliriz. Bu terim çok keskin çizgilerle ayrı değildir çünkü bazı gömülü sistemlerde parça değişikliği yapılabilir, bazı gömülü sistemler ise genel kullanıma uygun olabilir (örneğin; çok güçlü bir donanıma sahip büyük ekranlı bir tablet).

Gömülü sistemler daha çok bir amaç için kullanıldığından ve içindeki donanım seti önceden sabit olduğundan, üzerinden çalışan OS'ta gereksiz tüm yazılımlar ve özellikler kaldırılır ve üstünde çalıştığı donanıma göre optimize edilir. Aynı durum sunucu bilgisayarlar içinde geçerlidir. Bakıldığında normal bir bilgisayardan hiçbir farkları yoktur. Sadece çekirdekler donanıma support verecek şekilde optimize edilmiş ve gereksiz kernel modülleri kaldırılmıştır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# IaaS (or Infrastructure As a Service or Altyapı olarak servis)
uzaktaki sunucularda sizin adınıza sanal bir sistem (donanım) oluşturuluyor ve içine işletim sistemi kurulu şekilde hazır oluyor. (Örnek hizmet hizmeti: Amazon WS)

# PaaS (or Platform As a Service)
Bu sınıf yapısında size bir servis havuzu sunuluyor ve siz bu servislerden ihtiyacınıza göre ekleme çıkarma yapabiliyorsunuz. Tabiki her bir servis platformu belirli limitlerden sonra ayrı ayrı (+) maliyet demek. Örneğin; çeşitli servisler mevcut (Maven, MySQL veritabanları gibi).

Amazon Elastic Beanstalk, Heroku hizmetleri PaaS için birer örnektir.

# SaaS (or Software As a Service)
örnekler; Atlassian Jira, Gmail, Dropbox gibi.

# Mobile backend as a service (or MBaaS)
mobil uygulamalar için bulut sistemi hizmetleridir. örneğin notification yönetimi, app için analitik bilgi toplama işlemleri, crash hakkında bilgi toplama servisleri, kolayca erişilebilir veritabanları. Örnek hizmet: Google Firebase.

# BaaS (Backend As A Service)
MBaaS, bir çeşit BaaS'tır.

# on-premises software (or on-prem software)
yanlış kullanım: __on-premise software__

premises türkçe kelime anlamı: mülk (bina/arazı/taşınmaz)

donanımın ve üstünde çalışan yazılımın tümüyle kendi ortamımızda çalışabilen yazılım.

# Infrastructure as Code (o IaC)
Cloud ortamındaki config'lerinizi bir JSON dosyasında tutuyorsunuz. Bu JSON dosyasında belirtilen ortamı, Terraform komut satırı uygulaması sayesinde cloud'da (AWS, Google Cloud Platform, Azure...) ayağa kaldırabiliyorsunuz.

"Terrafom" uygulaması buna örnek verilebilir.

# IaC vs GitOps
IaC, GitOps'a göre daha alt seviyeli servisleri hazırlamak için yapılan bir tekniktir. IaC'de OS'ta hangi uygulamaların kurulu olacağı gibi kararlar alınırken, GitOps'ta bu uygulamaların hangi ayarlarla çalışacağı kararları alınır. Örnekler:
- GitOps'ta Kubernetes üzerindeki deployment'lar ayarlanır, HTTP routing'ler planlanır.
- IaC'de işletim sistemine hangi güvenlik yamalarının yapılacağı, hangi user'lara hangi güvenlik kısıtlamalarının olacağı, hangi version Kubernetes'in veya Docker'ın kurulacağı planlanır.

Fakat bazı durumlarda IaC'de Kubernetes kurulduğunda bazı spesifik ayarlar yapılması gerekebilir. Bu sebeple IaC vs GitOps ayrımı çok net çizgilerle belirlenmiş değildir.

# serverless
sunucu tarafta, business logic dışındaki işlerle ilgilenmemek için, güvenlik, oturum yönetimi, ölçeklendirilebilirlik, kurulum konfigürasyonları, ağ yönetimi gibi konular için dışarıdan hazır hizmet alınması tercih edilmektedir. bu da serverless mantığını ortaya koymuştur. isminden server olmama gibi anlaşılabilir, fakat olay bu değildir.

# edge computing
bu terim; tek bir merkezi yazılıma gitmeden işlem yapabilmemizi sağlayan sistemlere verilen genel bir terimdir. distributed sistemdir. bu sistemlere örnek olarak şunlar verilebilir:

- CDN

  bir content'e ulaşmak istediğimizde tek bir merkezi sunucudan değil, dünyanın her tarafında dağılmış bize en yakın CND server'ına bağlanmamız yeterlidir. Eriştiğimiz CDN server'ı, arkada bizim işlemi tamamlamamız için, daha da merkezi olan bir yazılıma gitme zorunluluğu yoktur.

- Otonom araçlar (akıllı şehirler ve/veya akıllı yollar ile birlikte kullanıldığında)

  - araçlar biraz ilerdeki yol durumunu (trafik, kaza vs...) tek bir merkezi sistemden değil, sadece yoldaki IoT device'ları ile haberleşerek alabilir.

  - park alanının dolu olup olmadığını, en merkezi sunucudan değil, yine ona en yakın IoT cihazlarından alabilir.

- otonom araçlar (tek başlarına - akıllı şehir olmadan)

  - kendi içinde, şöföre her daim yağ durumunu bildirmez. ancak belli bir seviyeye gelince şöföre (veya bir istatistik sunucusuna) bildirir.

- akıllı ev aletleri (smart home appliance)

  android cihazları ile son kullanıcıya bildirim yapan akıllı ev aletleri, her durumda push-server'a bilgi yollamaz. onun yerine kendi içinde bulunan sensörler sadece belli seviyelere gelince (belli durumlarda) push-server'a bilgi atar.

"Edge" terimi şuradan geliyor: tüm sistemin mimarisinde bir network var. bu network'te uç (edge) noktalara server'lar koyulmuş.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Genetic algorithm
belli bir sayıda aday çözüm üzerinde seçim, çaprazlama ve mutasyon operatörlerini kullanarak en optimum çözümler elde etmeye çalışan algoritmalardır.

Temel olarak şu döngü ile süreç işletilir:
- başlangıç popülasyonu (çözüm kümesi) (rastgele yada tahmin üzerine)
- uyumluluk hesaplaması (çözüm kümesindeki her eleman için uyumluluk hesaplaması yapılır)
- seçim operatörü uygulaması (her çözümün ayrı ayrı en iyi olanının incelenmesi)
- çaprazlama operatörü
- mutasyon operatörü
- program devam? program dur? kararı (en iyi çözüm yeterli mi? değil mi?)
  (eğer devam edilirse program 2'inci adıma dönecektir)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# No-code development platform
hiç kod yazmayı gerektirmeyen sadece config yaparak ve drag-drop ile uygulama geliştirmeyi sağlayan platformlardır.

# Low-code development platform
no-code gibi fakat gerektiğinde (bazı durumlarda) kod yazmayı da zorunlu kılan uygulama geliştirme ortamlarıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Kong
Kong yazılımı başladığında config'leri 3 farklı yerde okuyabilir:
- bir YML config dosyasından
- özel bir port üzerinden HTTP istekleri ile config'ler belirtilebilir. her ayar ayrı ayrı da yollanabilirken, tek bir seferde tüm config HTTP API isteği ile de atılabilir. Kong runtime sırasında kendini güncelleyebiliyor.
- config'leri database'den okuyor.

Kong açıldığında default olarak bu port'lardan hizmet vermeye başlar:

- :8000 listens for incoming HTTP traffic from your clients, and forwards it to your upstream services.
- :8443 same of 8000 but SSL enabled
- :8001 admin API
- :8444 admin API with SSL

örnek config dosyası:

```yml
_format_version: "1.1"

services:
- name: my-service
  url: https://example.com # servisimizin hizmet verdiği URL
  plugins:
  - name: key-auth # bu servise istek geldiğinde devreye girecek (araya girecek) eklentiler
  routes:
  - name: my-route # sadece routing'in ID'si.
    paths:
    - /my-service1 # artık http://kong-host:8080/my-service1 'e gelen istekler https://example.com 'a yönlendirilecektir.
```

Kong'da eklentiler Lua ile yazılmaktadır. eklenti altyapısı oldukça basit. Servlet filter'ı yazar gibi request ve response'u alıp manipüle edip bir sonraki eklentiye yollayabiliyoruz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Capitalization Styles
styles to convert a complete string into a single word.

- Raw: user login count

- __Snake Case__: user_login_count

- __Kebab Case (or spinal case or param case or Lisp case or dash case)__: user-login-count

- __Pascal Case__: UserLoginCount

- __Camel Case (or camel caps or medial capitals)__: userLoginCount

Akılda kalıcı olmaları için isimler gerçek hayattaki benzetmelere göre verilmiştir. Snake case her karakter küçük ve altçizgi olduğundan yılana benzetilmiştir. Kebap case ise, kebap yemeğindeki gibi şişteki her et parçasının arasında çubuk (tire işareti) vardır. Camel case'de develerin sırtı ortadaki çıktıya göre düşük seviyededir. sadece ortasında bir çıkıntı vardır (helloWorld).

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# interceptor vs filter vs middleware (or ara katman)
interceptor Türkçe kelime anlamı: yol kesen kimse

interceptor tüm programlama dillerinde kullanılan genel bir ifadedir. yukarıdaki tüm terimler yapılan bir işlemin önüne geçerek o işlem arasında işlem/kod çalıştırabilmemizi sağlamaktadır.

- filter'lar sunucuya giden isteklerin önüne geçen sınıflar için kullanılır.

- metot'ları execute etmeden önce çalıştırabileceğimiz sınıflara/metotlara interceptor denir.

- middleware ise interceptor konseptinin framework/kütüphane/modül(proje) seviyesi halidir. örneğin, Redux dünyasında "saga" bir middleware'dir. bakıldığında saga, basitçe "action" önüne giren bir interceptor'dür, fakat bu bir kütüphane/framework olduğu için middleware olarak adlandırılır. Oysa kütüphane olmayan ve elle tanımladığımız bir fonksiyon ancak bir interceptor olabilir. Benzer şekilde; ara katman servisleri dediğimiz yazılımlarda bir şekilde arkada olan servislerimizin önündedirler ve birer "proje" olduklarından middleware olarak adlandırılırlar.

örnek middleware'ler: 
- özellikle web server dünyasında çok sıkça kullanılır. sunucuya gelen istekleri karşışalrlar ve middleware olmayan diğer sunuculara yönlendiriler. request'lerin arasına girerek birçok farklı görevi üstlenebilirler: logging, security, transaction-monitoring... Farklı bir örnek daha verirsek: eski teknoloji kullanan bir back-end yazılımımız production ortamında çalışıyor olsun. Bunu artık modern bir API ile dışarı açmak istiyoruz. Eski olan koda dokunmak istemeyen şirket, bu yazılımın önüne bir middleware yazıyor ve artık bu yeni yazılımı dışarıya açıyor. Yeni yazılım arkaplanda eski yazılımı wrap ederek çalışıyor.
- __Messaging system (or message-oriented middleware or MOM)__: ActiveMQ gibi sistemler. Farklı servislerin aralarında mesajlaşması için aracılık yapar.
- Redux için Saga middleware.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# table vs grid
grid kelime anlamı: parmaklık, ızgara, haritayı karelere bölme sistemi, şebeke

tablo, standart olarak, Excel'deki gibi sadece header'ların veya ilk/son sütuna ait satırların farklı düzende olabileceği bir yapıdır. Oysa grid, tamamen düzeni custom belirlenebilen bir yapıdır. Örneğin bir web sayfanın tüm yapısını dahi grid ile parçalara bölebiliriz, fakat bir tabl ile bunu yapamayız.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Semantic UI
semantic kelime anlamı: anlamsal

Özel bir CSS framework'üdür. Aşağıdaki gibi kullanımı vardır:

```html
<div class="ui three labeled icon buttons">
  <button class="ui active button"><i class="pause icon"></i> One</button>
  <button class="ui inverted orange button"><i class="right arrow icon"></i> Two</button>
  <button class="ui orange button"><i class="play icon"></i> Three</button>
</div>
```

React için de hazır component'ler sunan farklı fork'ları mevcuttur.

# Material Design (or material ui or Materyal Tasarım)
Google'ın geliştirdiği bir tasarım düzenidir. Bu tasarımda yoğunca kart motifleri kullanılır, her objenin köşeleri belirgindir, zaten çoğunda da gölge vardır, bu şekilde her obje kendini daha ayırt edici hale getirir. Her obje, kağıt (burada sayfaya denk geliyor) üzerine pastel boya ile çizilmiş gibi duruyor. material ui; icon'lar, font'lar, effektler içinde standartlar içeriyor.

# Flat UI (or Flat design) vs Skeuomorphism
Skeuomorphism yerine bazen "rich design" terimi kullanılır.

skeuomorph sadece dizayn dünyasında kullanılan bir terimdir. flat UI'ın tersine, objenin gereksinimler dışında şekiller içermesi durumunda kullanılan dizayn biçimidir.

flat UI'da ise basit geometrik şekiller, düz renkler (farklı tonları aynı objeler de olmayacak şekilde), net ve keskin renkler kullanılır. bu şekilde kodlama altyapısı kolaylaştığı gibi, kod ve build edilen programın boyutları da düşmektedir.

# Ribbon
Ribbon kelime anlamı: Şerit, kurdele.

Microsoft office 2007 ile birlikte getirdiği toolbar'dır (component'tir).

"tabbed toolbars" olarak ta daha önceleri, başka GUI sunan programlar tarafından da kullanılmıştır.

# Metro UI
Microsoft'un geliştirdiği arayüz standardıdır. Marka sorunları sebebi ile yeni isimleri mevcuttur: "__Microsoft tasarım dili (or Microsoft Design Language or MDL)__".

Resmi olarak olmasa da, Microsoft bloglarında ve konferanslarında, MDL yerine "__Modern UI__" olarak ta kullanıldığı görülebilir.

Microsoft Windows 8 ile gelen yeni başlat menüsü tuşu ile açılan sayfada Metro UI kullanılmıştır.

Metro UI, temelde flat UI'ı baz alır.

MDL'nin 2inci versiyonu çıkmış ve daha sonra, yine Microsoft tarafından "Fluent Design System" geliştirilmiştir.

# design language (or tasarım dili)
metro ui, material ui gibi standartlara için kullanılan genel bir terimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Software release life cycle

- pre-alpha (or deployment release or nightly build or snapshot)

  The main development branch can be named as pre-alpha.

- alpha
- beta
- release canditate (or RC or gamma or delta)

  An RC should be move to live, if the important bugs will be fixed.

- RTM (or release to manufacturing or release to marketting)

  This is the stable version which is sending to manufacturers so they can install on devices. For example Microsoft sends the RTM build of the Windows to HP, ASUS etc...

- GA (or general availability)

  At the end of the RTM period GA build will release.

- production (or live release or gold)

  After GA the app may release new build with new version. Each new release named as "gold" version.

# RTW (or release to web)
downloadable version from internet. it's not a phase.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Netty
TCP veya UDP'den haberleşmeleri için kullanılan Java kütüphanesi.

Netty TCP'den byte byte istek atabilmemizi sağlıyor. fakat bu yapının üstüne kurulu çeşitli aracı sınıflarla HTTP gibi üst seviyeli yapılarda desteklenebiliyor.

Netty ile bağlantı kurulan tarafın Netty kullanmasını gerektirmez.

Netty non-blocking ve thread-pool yapısını destekliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# mkdocs
Builds static HTML files from Markdown files.

Bu şekilde PHP veya JVM çalıştırmadan web sunucusuna atılan dosyalar çalışabiliyor. Statik sayfaları son kulllanıcıya sunmak için Apache HTTP server dahi yeterlidir.

Mkdocs'un eklenti altyapısı da mevcuttur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Ebook vs PDF
PDF tüm ekranlarda apaynı görüntü ile açılması üzerine tasarlanmıştır. Yani fixed-size'dır. AÇılan sayfa büyütülürse veya küçültülürse, ekrandaki herşey ya büyür yada küçülür. Oysa e-book formatları farklı cihazlara (mobil gibi) direk uyumlu görüntü üretebilmektedir. Bu sebeple e-book re-flowable'dır.

Tabi e-book dosyaları da bir şekilde bazı trick'ler ile fied size hale gelebiliyor. Fakat bu her e-book formatı için resmi çözüm olmayabilir ve zaten tavsiye edilen bir yöntem değil.

E-book dosyaları herhangi bir viewe ile görüntülendiğinde, temelde HTML sayfası ile aynı mantıkta görüntülenir. Bu sebeple;
- SVG-based-image destekler,
- accesibility web browser'daki gibi çok rahat uygulanır,
- sadece text'lerin size'ı büyütülebilir/küçültülebilir,
- farklı temalar uygulanabilir (dark/white gibi).
- ve dahası...

Hatta, EPUB (özel bir e-book formatı) dosya formatı direk HTML üzerine kurulmuş bir formattır. Herhangi bir EPUB dosyasını, 7-Zip gibi bir uygulama ile açarsak, içinde HTML, CSS dosyaları göreceğiz. Tabi EPUB'ın kendi standardında farklı dosyalarda göreceğiz.

# e-book formats
https://en.wikipedia.org/wiki/Comparison_of_e-book_formats

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# interoperability (or Birlikte çalışabilirlik)
interop kelime anlamı: birlikte çalışma

genel bir terim olduğundan kullanılan domain'e göre farklı şeyi ifade edebilir. bazı örnekler:
- bir programlama dilinin interoperability karakteristiği; o dil ile yazılmış bir koddaki fonksiyonların, diğer diller tarafından yazılmış uygulamalar tarafından çağrılabilirliğini belirtir.
- diğer yazılımların kullandığı dosya formatına okuyup yazabilmekte, uygulamamızın interoperability karakteristiğini yansıtır.
- herkesin okuyabileceği ve yazabileceği bir dosya formatımız olursa, o dosya formatı için interoperability den bahsedilebilir.

İlk maddede belirtilen interoperability karakteristiğini şu maddelerle değerlendirebiliriz:
- dilin kullandığı primitive ve standart objeleri, diğer dillere pass edilebilirliği(uyumu) ve tersi
- diğer dillerden fonksiyonlar çağırdığındaki performansı ve tersi
- ve dahası...

- # Component Object Model (or COM)
Microsoft ürünlerinde kullanılabilen IPC için kullanılan, ortak data obje modelidir. dilden bağımsızdır. bu interoperability için değil IPC için kullanılmaktadır. COM; primitive tipler içeren bir standarttır. her dil bu kurallara uyar. bu şekilde haberleştiklerinde ortak değerler ile haberleşirler.

COM; IPC için kullanılır, fakat Microsoft dilleri arasında interoperability yapmak gerektiğinde de bu obje tipleri kullanılabilmektedir.

- # Distributed Component Object Model (or DCOM)
COM dan sonra çıkmış Remote procedure call'larda kullanılabilirlik eklenmiştir. bu sebeple Distributed prefix'i eklenmiştir.

- # Object Linking & Embedding (or OLE)
COM'a dayanan bir teknolojidir. Bir programdan bir dosyaya referans edebilmemizi sağlar. Örneğin, bir word dökümanında, başka bir excel dosyasının br parçasını gösterebiliriz. excel her güncellendiğinde otomatik olarak word'deki excel görüntüsü de güncellenecektir.

- # CORBA (or Common Object Request Broker Architecture)
Dilden bağımsız geliştirilmiş, DCOM'a alternatif bir standarttır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# rate limiter
- Çok fazla request'leri engelleyen modüle verilen genel isimdir.
- Şuralarda konumlanabilir:
  - client-side'a gömülü
  - API gateway'a gömülü
  - API gateway'ın önünde bağımsız bir servis olarak

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# List virtualization (or windowing)
Önyüz için kullanılan bir tekniktir. Bu teknikte; bir liste ekranda gösterileceği zaman sadece gösterileceği kısım render edilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Bloom filter
DB'ye sürekli sorgu atmak yerine tercih edilebilecek alternatif bir tekniktir.

space-efficient probabilistic (olasılıksal) bir tekniktir.

Örneğin DB'ye sürekli bu username var mı yok mu diye sorgu atmamız beklenmektedir. Çok user olan bir DB varsa bu sistemi çok yoracaktır. Username yerine hash almamız ve hash olan sutunu sorgulamamız bize bi yere kadar hız kazandırabilir. Bu sebeple şöyle bir çözüme gidilebilir:

Her değeri sıfır olan 64 (veya daha farklı bir sayı olabilir) bitlik bir dizi oluşturulur. Sisteme  her yeni user-name eklendiğinde user-name'in hash'i alınır ve bu hash mod-64 işlemine sokulur. Mod işleminin çıktısı örneğin 3 ise, 64 bitlik bu dizide 3'üncü elemanı "1" olarak set ediyoruz. User-table'a her insert işleminde, bu işlemi bu diziye de yapıyoruz.

Şimdi artık bir user-name sorgulamak istediğimizde, o user-name'in hash ve mod64 ünü alıyoruz. Çıkan sonucu index kabul edip, ilgili array'imizde:
- eğer 1 görüyorsak, bi ihtimal bu user-name sistemde var anlamına gelmektedir.
- Fakat eğer index'teki eleman "sıfır" ise, bu user-name kesinlikle sistemde yok demektedir.

# kullanımı
Buradan anlaşıldığı gibi bize kesin bilgi vermemektedir. İhtimaller ile çalışmaktadır. Bu sebeple business'ımızı buna uygun olması gerekir. Yoksa bu tekniği kullanamayız. Örneğin user-name sorgulama servisi, kesin bir dönüş beklenen bilgi servisi olduğu için, burada "Bloom filter" kullanamayız.

Fakat "Uzun Hashcode'a ihtiyaç duyulmadığı durumlar için genel çözüm" başlığında anlatılan durum için "bloom filter" kullanabiliriz. Daha doğrusu; "username kesin yok mu" diye sorguyu boolm-filter ile yaparız. Redis üzerind eyaparız ve çok hızlı olur. Eğer user yok ise, user'ı kaydederiz. Faat var ise, standart (bloom-filter olmayan) sorgu ile devam ederiz.

# filter'da birden fazla hash fonksiyonu kullanımı
Bloom filter için yukarıda anlatılan örnekte sadece 1 hash fonksiyonu kullandık. Opsiyonel olarak farklı hash fonksiyonları daha kullanabiliriz. Bu durumda aynı user-name'i; hash1("Jack") ve hash2("Jack") fonksiyonlarına sokar ve her ikisinin ayrı ayrı mod'larını alırız. Her ikisinin index'inin ayrı ayrı kontroler ederiz. Eğer index'teki elemanlardan biri boş ise "Jack" önceden kesinlikle kayıt edilmemiş anlamına gelir. Ama eğer array'deki iki elemanda dolu ise, muhtemelen "Jack" DB'de var anlamına gelir.

İstersek 3 tane de hash fonksiyonu yaparız. Hash fonksiyonlarını arttırmamız durumunda, array'in boyutunu küçültebiliriz. Burada CPU-RAM trade-off'u vardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ID generation için farklı çözümler

- Eğer DB'deki bir sutun için ID kullanıyorsak, JPA bizim için reserve ediyor.

- UUID

- Ticket server

  Her bağımsız node (servis) ID'ye ihtiyaç duyduğunda, merkezi bir sunucudan ticket aralığını reserve eder. Signle point of failure vardır. Fakat diğer node'lardan haberdar olmaya gerek yoktur.

- Twitter snowflake approach
  
  UUID gibi her node birbirinden bağımsızca 64 bit ID üretebilir. Her ID şu bilgiden oluşur:

  - 1 bit. It will always be 0. This is reserved for future uses.
  - timestamp with millisecond (41 bit)
  - data center ID (5 bit = 32)
  - machine ID (5 bit = 32)
  - sequence number (12 bit = 4096)

  Timestamp milisaniye cinsinden olduğu için, "sequence number" her millisaniye 4096 farklı sayı üretebilir.

  Ek not: Bu not "Twitter snowflake" makelelerinde yazmıyor. Kişisel eklemelerimdir:
  - Yukarıdaki timestamp değeri için başlangıç değerini POSIX-date'in kullandığı 1970 olarak değil, bu modülü çalıştıracağımız günün tarihi olarak kabul etmeyi düşünebiliriz. Böylece timestamp'teki bit sayısı çok düşecektir. Böylece bu bitleri "sequence number" için kullanabiliriz.
  - machine ID, Docker gibi container altyapılarında, her instance'larda yeni oluşturulmaktadır. Bunu göz önünde bulundurup çözüm uygulamak gerekebilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Software craftsmanship
craft kelime anlamı: zanaat (el sanatı, beceri).

İngilizce: http://manifesto.softwarecraftsmanship.org/#/en

```
As aspiring Software Craftsmen we are raising the bar of professional software development by practicing it and helping others learn the craft. Through this work we have come to value:

Not only working software, but also well-crafted software
Not only responding to change, but also steadily adding value
Not only individuals and interactions, but also a community of professionals
Not only customer collaboration, but also productive partnerships

That is, in pursuit of the items on the left we have found the items on the right to be indispensable. 
```

Türkçe: http://manifesto.softwarecraftsmanship.org/#/tr

```
Yüksek emeller peşinde koşan Yazılım Ustaları olarak bizler, profesyonel yazılım geliştirme çıtasını, bizzat uygulayarak ve başkalarının bu mesleği öğrenmelerine yardım ederek yükseltiyoruz. Bu çalışmaların sonucunda: 

Sadece çalışan yazılıma değil, ustaca üretilmiş yazılıma da
Sadece değişikliğe cevap vermeye değil, sürekli değer katmaya da
Sadece bireylere ve etkileşimlere değil, profesyoneller topluluğuna da
Sadece müşteri ile işbirliğine değil, üretken ortaklığa da

Değer vermeye kanaat getirdik. Sol taraftaki maddeleri takip etmekle birlikte, sağ taraftaki maddeleri vazgeçilmez bulmaktayız. 
```

Agile'ın çıkışının üzerine bazı yazılım kalitesini arttırmaya yönelik çıkarılan manifestodur. Bu manifestoda "agile" reddedilmiyor, fakat bu maddelerinde de çok önemli olduğu bastırılıyor/hatırlatılıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •
