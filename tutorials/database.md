############################

############################
# DATABASE
############################

############################

# CouchDB
Document based bir NoSQL'dir.

# CouchDB vs Couchbase
__CouchDB__, IBM'de çalışan bir devleoper tarafından geliştiriliyordu. Daha sonra bu developer, kodu açık kaynaklı olarak yayımladı ve __Apache vakfı__ projeyi devam ettirdi ve "__Apache CouchDB__" ismi ile devam etti.

__CouchOne (or old-name:CouchIO)__, "__Apache CouchDB__"'i geliştiren bir yazılım şirketinin ismidir.

__CouchOne__ firması, __Membase (or old-name: NorthScale)__  firması ile birleşerek __Couchbase__ isimli şirket kuruldu.

__Membase__ şirketi, __Membase__ isimli product'ı geliştiriyordu. __Membase__'i geliştiren lead developer'lar __Memcached__ projesinde de çalışmıştı. __Memcached__ ürünü, __Memcached__ protokolünü kullanır.

__Couchbase__ firması, __Membase__ ve __Apache CouchDB__ yazılımlarını birleştirerek "__Couchbase Server__" isimli yazılımı yarattı. Yeni yazılım __Membase__'in yeni versiyonu olarak tanıtıldı. __CouchDB__ ile uyumluluk gibi bir durum söz konusu değildir.

Yıl 2022 itibariyle "__Couchbase Server__" ve "__Apache CouchDB__" olarak 2 ayrı proje birbirinden bağımsız şekilde geliştirilmektedir.

__Couchbase Server__'ın hem __Community__ hemde __Enterprise__ sürümü mevcut.

# N1QL
acronym of: "non-first normal form query language".

pronounced: nickel.

Couchbase'in kullandığı SQL alternatifi dildir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# nesne-ilişkisel empedans uyumsuzluğu (or object-relational impedance mismatch)
Object-oriented bir dilde, DB entity'lerimizde kullandığımız teknikler birbirlerinden farklıdır:

- Granularity Problem

  Customer içinde Adres olan bir nesnenin tek bir tabloda karşılığı olamaz. Bunun için her class'a istinaden bir tablo açmak gerekir.

- inheritance

  RDBMS'te inheritance yoktur.

- identity

  Object-oriented'da equals metodunda herhangi bir field ve/veya logic ile eşitlik kontrolü sağlanabilir. Oysa RDBMS'te sadece primary key ile bir satırın diğerine eşit olup olmadığını tanımlayabiliyoruz.

- data navigation

  Object-oriented'da bir nesyeye erişim referans olan field'dan yapılabilir: customer.getOrder().getAddress(). Oysa RDBMS'te bu iş "SQL join" ile olmaktadır.

- type mismatch

  list gibi tipler RDBMS'te tutulduğunda bambaşka bir formtta tutulmaktadırlar. Diğer her tipi 1e1 karşılığı olmayabilmektedir.

Bu sorunları ortadan kaldırmak için NoSQL'e başvurulabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Lookup table
lookup kelime anlamı: arama.

Türkçe kaynakların bazılarında "__başvuru çizelgesi__" olarak kullanılıyor.

lookup table'ın sıradan bir tablodan hiçbir fakrı yoktur. Sadece tablonun oluşturulma amacı birşeylere hızlı veya daha simple bir API ile erişim sağlayabilmektedir.

lookup table'ın kaç sutunu olabileceği gibi startdartlar yoktur. Lookup table çok genel bir terimdir.

Örnek durumlar:
- çok sık arama için kullandığımız sadece product-id ve product-name bilgilerini cache'te hızlı erişmek için tuttuğumuz tablo lookup table'dır. (Hızlı erişim için çözüm)
- Resim dosyalarında ekranda resmi gösterirken veya resmi editlerken, ekranda olan renkler hızlı erişim için editor tarafından gruplanarak tutuluyor. Bu gruplu bilgiler sadece RAM'de tutuluyor. Resim dosyasında bu bilgiler yok. Bu RAM'deki tabloya lookup table denir. (Komplek API için basit API çözümü)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# scale and precision
database'lerin birçoğunda numeric tipindeki sütunların sunduğu özelliktir. varchar(90)'ın aldığı 1 parametre gibi, numeric tiplerde scale ve precision parametresi alabilir. örnek:

```sql
CREATE TABLE dbo.MyTable
(
  MyDecimalColumn DECIMAL(5,2),
  MyNumericColumn NUMERIC(10,5)
);

INSERT INTO dbo.MyTable VALUES (123, 12345.12);
```

__precision__ is the maximum total number of decimal digits to be stored. This number includes both the left and the right sides of the decimal point. The precision must be a value from 1 through the maximum precision of 38. The default precision is 18 on MS-SQL.

__scale__ is the number of decimal digits that are stored to the right of the decimal point.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DUAL
Oracle içerisinde gelen bir tablodur. Tablonun içerisinde sadece "DUMMY" isimli, "char" tipinde bir sütun vardır. İçerisinde de sadece 1 kayıt vardır. Onun içeriği de 'X''tir.

Dual tablosu aşağıdaki tarz sorguları atabilmemizi sağlamaktadır:

```sql
SELECT 1+1
FROM dual;

SELECT 1
FROM dual;

SELECT USER
FROM dual;

SELECT SYSDATE
FROM dual;
```

Ms-SQL'de dual tablosuna ihtiyaç yoktu çünkü bu çalışmaktadır:

```sql
SELECT 1+1
```

Bazı veritabanlarında Oracle ile uyumlu olmak için dual tablosu default'ta gelmektedir. Bazı veritabanlarında ise aynı amaçla farklı tablo ismi kullanılmaktadır. Bazılarında ise hiç dual tablosu yoktur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# BSON (or binary JSON)
MongoDB'nin data'ları file-system'a kaydettiğinde kullandığı bir formattır.

örnek JSON dosyası:

```json
 {"hello": "world"}
```

buna denk gelen BSON dosyası:

```
\x16\x00\x00\x00          // total document size
\x02                      // 0x02 = type String
hello\x00                 // field name
\x06\x00\x00\x00world\x00 // field value
\x00                      // 0x00 = type EOO ('end of object')
```

BSON dosyası, ona denk gelen JSON dosyası ile karılaştırıldığında bazen daha büyük boyutlu veya bazen daha küçük boyutlu olabiliyor.

BSON, JSON'ın desteklemediği date, binary-data gibi formatları da destekliyor. Bunlara "BSON type" deniliyor ve her bir tipe bir numara verilmiş. örneğin; String 2, Date 9. kaynak: https://www.mongodb.com/docs/manual/reference/bson-types/

BSON dosyaları binary oldukları için human-readable değillerdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# cluster vs data center (or data centre)
data center fiziksel bir kavramdır. data center aynı LAN'a bağlı olan bilgisayarlar topluluğudur. Oysa cluster tamamen logically sınıflandırılmış/gruplandırılmış yazılımlar kümesi anlamına gelir.

Aynı yazılımın 1 cluster'ı içerisinde birden fazla data center olabilir. Veya 1 data center içerisinde, aynı yazılımın birden fazla cluster'ı olabilir.

1 cluster içerisindeki veriler sync olmak zorunda değil. parçalanmış olabilir, yada sadece bazı data'lar sync, bazı data'lar parçalanmış olabilir. Bu tamamen yazılımın nasıl çalıştığına bağlıdır.

Cluster'lar arası haberleşmede şu gibi durumlar olabilir:
- cluster çökme senaryoları için, bir cluster diğer bir cluster ayakta mı? eğer ayakta değilse devreye girmeye hazırlanması için bazen birbirleri arasında haberleşme sağlayabilirler.
- cluster'ın tümünün çökme senaryoları için bir cluster diğer bir cluster'ı sürekli kendine kopyalayabilir. Bu herhangi bir client'ın o cluster'dan data çekmesine benzerdir.
- ve dahası...

Ama asıl olarak cluster'lar arasında şu olamaz/olmamalı: Bir cluster içindeki bir node, diğer bir cluster'ın içindeki node'un tamamlayıcısı olamaz/olmamalı.

Yazılım sunucuları birçok node'da ayağa kaldırıldığında bu yazılımlara her cluster ve her data-center'ı tanıtmak gerekir. Çünkü data-center'lar arası data paylaşım politikası ile data-center içerisindeki data paylaşım politikası farklı olabilir. Örneğin; Cassandra'da, aynı cluster içindeki farklı, data-center'larda farklı politika izlemesini sağlayabiliyoruz. örnek:

```sql
CREATE KEYSPACE custom_keyspace
-- here column names...
WITH REPLICATION = { 'class' : 'NetworkTopologyStrategy',
                     'datacenterNewYork':5,
                     'datacenterDubai':10
                   }
```

# availability domain
yoğun yük durumuna karşı çalışan data center'ların tümüdür.

# High-availability clusters (or HA clusters or fail-over clusters)
fail-over kelime anlamı: yük devretme

Bu terim sıkça DB alanında kullanılsa da, bilişimde diğer birçok alan içinde geçerlidir. Herhangi bir server yazılım grubu için High-availability'den bahsedilebilir.

Her DB'nin genelde clone'u/replikası tutulur. Bu şekilde bir DB instance'ına zarar gelirse, yedeği olmuş olur ve aşığı yük karşı işlemler dağıtılmış olur. Bunu yapmak için birçok farklı teknik var:

- __Active/active__

  her node sync bir şekilde birbirinin her güncellemesini anlık olarak alır/sync olur. Bu yavaş bir tekniktir fakat en stabildir. Her instance/node sürekli dışarıya hizmet veriyor olur.

- __Active/passive__

  passive node'lar, active olan (dışarıya hizmete açık olan) database node'larındaki tüm değişiklikleri kendine sürekli olarak alır. Fakat passive node'lar dışarıya hizmet vermezler. Eğer active bir node'a zarar gelirse, passive olan herhangi bir node active duruma geçer.

# Oracle's High Availability Architectures and Solutions
Oracle DB ürünü (default'ta) sadece tek başına tek 1 instance ve tek 1 cluster olarak çalışır. Diğer her feature ekstra çözüm uygulamak gerekir. Bunlar burada detaylı listelenmiştir: (source-id: 86)

Bu çözümlerden biride 'Data Guard'dır. Bu çözümde "Primary database" e hem yazılıp hem okunabiliyor. Fakat farklı data-center'larda olan birden çok Oracle instance'ı "Standby database" olarak çalışıyor. Primary'ya yapılan her işlem otomatik olarak her standby'a da yollanıyor. Standy sürekli olarak sadece read-only hizmet verebiliyor. Fakat eğer primary çökerse, bir stanby primary olarak çalışmaya başlıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# CAP theorem
Eric Brewer tarafından ortaya atıldığı için "__Brewer's theorem__" denir.

dağıtık bir sistemde veri üzerinden sunulan hizmet için aynı anda 3 özelliğin sağlanamayacağıdır. En fazla 2 tanesinin sağlanabildiğidir. Bu 3 özellik nedir:

- Consistency (or Tutarlılık): dağıtık sunuculardan birine X bilgisini kaydettik. diğer herhangi bir sunucudan o veriyi okumak istediğimizde X okuruz.

- Availability (or Erişebilirlik): istediğimiz zaman sunuculara erişim açık olacaktır (sunucular erişilebilir durumda olacaklardır). Bu madde "high-availability"'den farklıdır. "high-availability" yük dağıtmak ve hata durumunda diğer sunucuları hazır tutmak amaçlı iken, sadece "Availability" terimi sunucuların cevap vermeyi reddedip reddetmemesi karakteristiğidir.

- Partition Tolerance (or Bölünebilme Toleransı): sistemdeki data'lar tüm node'larda değil, parçalanmış durumda olması durumudur.

Örneğin; ağ arasındaki haberleşme giderse (P); C'nin sağlanması mümkün olamaz. Fakat A sağlanabilir.

Örneğin; Eğer data partitioning yaptıysak (P) ve node'lar arasında network sorunu olduğunda şu iki çözümden birini uygulamamız şart:
- client'tan gelen request'leri geri çevir --> bu durumda; sistemin tutarlılığı sağlanmış olur (C), fakat availablity'den (A) feragat etmiş oluruz
- client'lardan gelen request'lere, eldeki en sonki güncel data ile cevap ver --> bu durumda; sistemin tutarlılığı sağlanmamış olur (C), fakat availablity (A) sağlanır.

(yukarıdaki örnek için kaynak: (source-id: 87) "Peki bu Partition Tolerance ne işe yarıyor ? Hala tam anlayamadım diyorsanız biraz daha konunun üzerine gidelim." maddesi.)

Facebook gibi sosyal medya yazılımlarında, "like" sayıları anlık olarak herkese doğru gösterilmeyebilir. "Like" gibi tölerans gösterilebilen birçok data tipi vardır. Bunlar için uygun teknoloji seçimi yapmak gereklidir. CAP teoremi yazılarında, hangi ikili istendiğinde hangi veritabanı kullanılabileceği de yazmaktadır.

```
                                 CONSISTENCY (All client get the same data)
                                           x
                                        x     x              MongoDB
          RDBMS                    x             x           Redis
          MySql               x                      x       HBase
          MariaDB         x                             x    MemcacheDB
                     x                                     x
                 x                                             x
            x  x  x   x   x   x   x  x   x  x  x  x  x  x  x  x  x
AVAILABILITY                                                       PARTITION
(All clients can                   Cassandra                 (The system is not effected
always read and write)             Riak                       by network partitioning)
                                   CouchDB
                                   Dynamo
```

Yukarıdaki şekilde DB isimleri farklı köşelerde de olabilir. Çünkü, DB sistemleri yapılacak config'lere göre davranışı değişebilir.

# PACELC
pronounced "pass-elk".

Teorinin başlığı orjinal dökümanındaki bu cümledeki karakterlerin birleşiminden gelmektedir: "if there is a partition (P), how does the system trade off availability and consistency (A and C); else (E), when the system is running normally in the absence of partitions, how does the system trade off latency (L) and consistency (C)?"

Burada CAP'e ek olarak __latency (L)__ de işin içine katılmaktadır.

CAP teoreminin bir uzantısı olarak ortaya atılmıştır. Yukarıdaki cümle şunu diyor:
- P varsa, A ve C arasında seçim yapılması gerekir (Bunu zaten CAP söylüyor).
- P yoksa, C ve L arasında da seçim yapılması gerekir (işte bunu sadece PACEL söylüyor).

Normalde CAP, P yoksa, A ve C arasında seçim yapmamızı söylüyor. Oysa P yoksa, A ve C arasında seçim yapmamızdan ziyade, C ve L arasında seçim yapmamızın daha düşünülmesi doğru bir kriter olduğunu dile getiriyor. Çünkü P yoksa, zaten C ve A her türlü sağlanacak, fakat asıl mesele A'nın ne kadar Latency ile sağlandığı düşünülmelidir.

# kısaltmalar
bir sistemimiz/yazılımımız olsun. Bu yazılım PACEL'deki hangi şemaya göre çalışıyor ise ona göre bir isim verebiliyoruz.

Aşağıda "yok", "umursanmıyor" anlamında kullanılıyor.

Örnekler:

- __PA/EL__ system: Bu yazılımı/sistemi Partition yaparak işletirsek, A olur, C olmaz. Eper partitionsuz çalıştırırsak, L olur, C olmaz. Bu ssteme cassandra'yı örnek verebiliriz. Cassandra'yı partition'lu veya partitionsuz çalıştırabiliriz.
- __PC/EC__ system: P açık modundaysa, C var. A yok. Eğer P kapalı moddaysa, C var, L yok.

Yukarıdaki kısalatmada, P ile çalışır moddayken feragat edilmeyecek olan karakteristiği P'nin yanına yazıyoruz. P olmadığındaki modda, feragat edilmeyecek olan karakteristiği E'nin yanına yazıyoruz. İsimlendirme formatı şu şekilde:

> PX/EY

X ve Y feragat edilmeyecek karakteristiklerin baş harfidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Consistency Models
RDBMS'ler "Immediate Consistency" sunarlar. fakat ayarlanır ise; bazı NoSQL veri tabanlarıda "Immediate Consistency" sunabilir.

birçok çeşit Consistency Model'i var. Bu modeller burada anlatılmıştır: (source-id: 88) Yazar: Miguel Diogo, Bruno Cabral, Jorge Bernardino, "Consistency Models of NoSQL Databases", "Figure 2.Consistency Models based on Reference" görseli.

Consistency nereden bakıldığına göre (client ve sunucu açısından) değişir.

## User-centric consistency models

- ### Consistency Guarantees

  Bu başlık altındakiler birer model değildir. Fakat birer feature olarak değerlendirilirler.

  - ### read your own writes (or  read-your-own-writes or read-your-writes or read your writes or RYW)
    bir client bir bilgiyi kaydettikten hemen sonra o bilgiyi okumak isterse, güncel kaydettiği bilgiyi okur.

  - ### Monotonic writes (or MW)
    bir client sırası ile istek attığında bunlar sırası ile işletilecektir.

  - ### Monotonic reads (or MR)
    client bir bilgi okumuş ise; o bilgi okunduktan sonra tekrar update edilmedi ise, o bilgi tekrar tekrar okunduğunda aynı dönecektir.

- ###  Writes Follow Reads (or WFR)

- ### Eventual consistency (or nihai tutarlılık)
  weak ile strong arasında kalan bir tutarlılık biçimidir.

  weak'teki gibi; client'lar eski data'ları çekiyor olabilir. fakat nihai olarak, belli süre geçtikten sonra aynı bilgiyi çekecektir. burada Weak Consistency'den fark; işlemlerin sıralı olmasıdır. Weak Consistency'de sıra sağlanamadığından, herkes aynı zaman aralıklarında farklı data çekebilir.

  Bu fark neden olur? Cassandra'yı ele alalım. Cassandra'da, update ile insert işlemi aynı kapıya çıkıyor. 2 farklı Cassandra sunucusuna istek geldiğini düşünelim, ikiside insert işlemi(yada update) yapıyor olsun. ikisi aynı anda replica'lara dağıtılırken/sync olurken, o sıra data okumaya çalışan farklı client'lar var ise, farklı sunuculardan okurlarsa aynı anda farklı data çekmiş olacaklar.

  Cassandra örneğimizde; en son yazılan data, (belli bir süre sonra - nihai olarak) tüm replica'lara klonlanacağı için, nihai tutarlılık sağlanmış olacak.

## Data-centric consistency models

- ### Causal consistency

- ### FIFO consistency (or PRAM consistency)

- ### Cache consistency

- ### Processor consistency

- ### Strong Consistency (or immediate consistency or Linearization or strict consistency or atomic consistency)
  client'a cevap ancak; tüm sunuculara iletildikten ve kaydedildikten sonra gerçekleşir. dolayısı ile en yüksek seviye consistency budur. tüm sistemde kayıtların/işlemlerin sırası tektir/sabittir.

- ### Weak Consistency
  client'a gönderilen cevap en güncel data'yı içermeyebilir. tüm sistemde kayıtların/işlemlerin sırası tutulmaz.

  client'ın eski data alması durumuna "inconsistency window" adı verilmiştir.

- ### Sequential Consistency
  tüm client'lar aynı zaman aralıklarında aynı bilgiyi okurlar. örneğin; X güncellemesi, replica'lara sync ediliyor olsun. 10 replica'dan 5'i X güncellemesini alsın. Tam o sırada istek atan client'ların tümü X güncellemesini almalı yada herkes eski halini görmeli. Client'ların bir kısmı X ile bir kısmı eski halini görmemeli. Eğer bu durum sağlanabiliyorsa "Sequential Consistency" var demektir. kaynak: (source-id: 89) "2–1. Sequential Consistency" başlığı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Replication models

- ## Passive replication model (or Primary backup)
  tüm client'lar sadece "primary server" denilen sunucuya istek atar. bu sunucu diğer replica'lara klonlama işlemini gerçekleştirir. sync yada async yapıp yapmayacağı implementasyona göre değişir. eğer sync backup işlemi yapılıyorsa; client'a geç cevap dönülür. bu model'e __pessimistic__ adı verilir. Tersi durumda async backup alınır ve client'a hızlıca cevap dönülür. Bu model'e ise __optimistic (or eager)__ adı verilir.

  Yukarıdaki tüm paragraf için kaynak: (source-id: 90) Yazar: Leticia Pascual Miret, Master's Degree in Parallel and Distributed Computing, "2.4.1 Passive replication model" başlığı.

- ## Active replication model
  Passive'de tek bir yükümlü sunucu varken (primary server), burada birden fazla olmaktadır. Dolayısı ile, tutarlılığı sağlamak daha zordur. Fakat bu model'in __fault tolerance (or hata töleransı)__ vardır.

  kaynak: (source-id: 90) Yazar: Leticia Pascual Miret, Master's Degree in Parallel and Distributed Computing, "2.4.1 Passive replication model" başlığı.

- ## Semi-passive and semi-active replication models
  kaynak: (source-id: 90) Yazar: Leticia Pascual Miret, Master's Degree in Parallel and Distributed Computing, "2.4.1 Passive replication model" başlığı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Horizontal Partitioning (or Yatay Bölümleme)
aynı satır bütünüyle aynı makinada tutulur. fakat 1 tablonun farklı satırları, birçok farklı fiziksel dosyalarda bulunabilir. örneğin Users tablosu nun ilk 1000 satırı, X fiziksel dosyasındayken, ikinci 1000'lik kısmı (1000-2000 satırları) farklı fiziksel dosyada olması durumudur.

# Vertical Partitioning (or Dikey Bölümleme)
X dosyasında sadece S1, S2, S3 sütunları olsun. Y dosyasında ise S4, S5, S6 sütunları olması durumudur. kaynak: (source-id: 469) 1st sentence.

Vertical partitioning pratikte uygulama seviyesinde yapılır. Örneğin customer tablosundaki sutunları 2 farklı tabloya bölerek ve farklı makinelere dağıtarak bu işlemi yapabiliriz. Eğer tüm data'yı çekmek gerekirse 2 datayı ardarda çekmemiz gerekir.

# Horizontal Partitioning vs Vertical Partitioning
Sadece bazı sütunları okuyor isek, sadece bazı node'lara erişeceğimiz durumlar için "Vertical Partitioning" daha verimli. Fakat tüm sütunları çekiyor, fakat belli satırları query ile çekiyorsak (örneğin; şu tarihte kayıt yaptırmış kullanıcılar) bu durumda sadece bazı node'ları kullanacağımız için "Horizontal Partitioning" daha verimli.

# Horizontal + Vertical Partitioning
Bu ikisini birlikte uygularsak bilgilerimiz şu şekilde bölünür:

- A makinasında sadece S1,S2,S3 'ün ilk 1000 satırı vardır.
- B makinasında sadece S1,S2,S3 'ün ilk 1000-2000 satırları vardır.
- C makinasında sadece S4,S5,S6 'ün ilk 1000 satırı vardır.
- D makinasında sadece S4,S5,S6 'ün ilk 1000-2000 satırları vardır.

# partitioning vs sharding
sharding kelime anlamı: parçalama

partitioning kelime anlamı: bölümleme

partitionin bir tabloyu veya sutun grubunu ayrı fiziksel dosyalara ayırmaktır. Eğer partitioning işlemini farklı makinelere yapıyor isek bu işlem "sharding" olarak isimlendirilmektedir. kaynak: (source-id: 464) title: "What Is the Difference between Sharding and Partitioning?".

Ek not: DB programı farklı fiziksel makineler olup olmadığı anlayamaz. Burada DB, farklı/bağımsız DB instance'ı olup olmadığına bakar. Eğer bir data'yı farklı/bağımsız DB instance'larına dağıtıyor isek, sharding yapmış oluruz.

Bu sebeple; Horizontal Partitioning işlemini farklı makinelerde yapıyorsak Horizontal Sharding olmaktadır. Vertical Partitioning'te benzer şekilde, farklı makinalarda yapılıyorsa, Vertical Sharding olarak isimlendirilmelidir.

"shard" veya "partition" terimleri DB'den DB'ye farklı anlamları olabiliyor. Buna 2 örnek:
- Dosyalara parçalarken oluşturulan yeni tablolara "shard" veya "partition" adı verilir. kaynak: (source-id: 464) 1st paragraph.
- Dosyaların bölündüğü farklı veritabanı instance'larının her birine "shard" denir. kaynak: (source-id: 468) 1st paragraph, 2nd sentence.

# Horizontal Scaling (or Yatay Ölçeklendirme)
yeni makine/node eklenerek güç arttırılması.

# Vertical Scaling (or Diken Ölçeklendirme)
var olan makinelere CPU, memory, disk gibi kaynaklar eklenerek güç arttırılması. kaynak: (source-id: 93) "Partitioning and Sharding" başlığı, 3üncü paragraf, 1inci cümle.

Bunun en önemli 2 dezavantajı:
- Tüm bilgiler tek node'da toplanıyor (Single Point of failure)
- Her OS veya her donanım veya her yazılım sonsuz sayıda CPU veya memory veya disk gibi kaynakların eklenmesini resmi olarak desteklemiyor olabilir.

# scaling verbs
- scale up: adding more resource (RAM, CPU...) to a machines/instance.
- scale down: removing resources (RAM, CPU...) from a machines/instance.
- scale out: adding more machines to cluster or system
- scale in: removing machines from cluster or system

# Network Partitioning (or Split Brain Syndrome)
Bağımsız cluster'daki node'lar, diğer cluster çökmemesine rağmen çökmüş olarak algılama durumuna verilen isimdir. kaynak: (source-id: 94) 1inci paragraf.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Mongo DB User/Auth

Mongo da varsayılan olarak veritabanında hiç kullanıcı yokken herkes erişimi sağlayabilmektedir. bir kullanıcı eklendiğinde artık erişimlerin tümü kısıtlanmıştır.

Mongo'da "admin" database'si sistem tarafından kullanılan bir database'dir. bu database'nin içinde kullanıcılar ve rol bilgileri collection'larda tutulur. bu sebeple "admin" database'si, örneğin Robomongo tool'unda "System" klasörü altında gösterilir.

ilk Mongo açıldığında "admin" database'sinde bir user yaratılmalıdır. bu user'a "userAdminAnyDatabase" rolü atanmalıdır. bu role ile bu kullanıcı artık yaratılacak her yeni database'ye yeni kullanıcı ekleyebilir hale gelir. bu kullanıcı collection'lara veri ekleme çıkarma gibi işlemler yapamaz. bu rol'de atama işlemini şu komutlarla yapabiliriz:

```js
use admin

var user = {
  "user" : "admin_user",
  "pwd" : "manager",
  roles : [
      {
          "role" : "userAdminAnyDatabase",
          "db" : "admin"
      }
  ]
}

db.createUser(user);

exit
```

şimdi Mongo'ya bu yetki ile bağlanıp yeni bir database ve ona bağlı bir user yaratalım:

> Mongo admin -u admin_user -p

```js
use test_db

var user = {
  "user" : "test_db_user",
  "pwd" : "123",
  roles : [
      {
          "role" : "readWrite",
          "db" : "test_db"
      }
  ]
}

db.createUser(user);

exit
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Liquibase vs Flyway
programatik DB şema yönetimini sağlayan birbirine alternatif framework'lerdir.

Migration gibi işlemlerde, database şema işlemlerinde kullanılabilirler. Her şama değişikliğini detaylı şekilde historical tutarlar ve hata olması durumunda rollback senaryoları tanımlayabilmemizi sağlarlar.

Martin Fowler "Evolutionary database design" isimli bir makale yayımlamıştı. Bu makaledeki gibi database gelişimi istiyorsak bu tarz framework'lerden yararlanmamız çok verimli olacaktır.

örnek bir "liquibase.xml" dosyası:

```xml
<?xml version="1.1" encoding="UTF-8" standalone="no"?>
<databaseChangeLog xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                   xmlns="http://www.liquibase.org/xml/ns/dbchangelog"
                   xsi:schemaLocation="http://www.liquibase.org/xml/ns/dbchangelog http://www.liquibase.org/xml/ns/dbchangelog/dbchangelog-3.9.xsd">

    <!-- her changeSet sırası ile DB şemasına işlenir.  liquibase kendi tablosunu oluşturur.
    bu tabloda hangi changeSet'lerimn işlendiği, author'un kim olduğu, hangi date-time'da database'ye işlendiği
    gibi bilgileri tutar.-->

    <!-- changeSet'lerde herşey olabilir: DB'ye yeni kayıt (satır) ekleme,
         yeni tablo oluşturma, tablo silme, tablo şeması güncelleme... -->

    <changeSet author="ahmet" id="changeSetId1">
        <createTable tableName="users">

            <!-- 2 adet sütun birlikte (username ve last_name), bir primary key görevindeler. -->
            <column name="username" type="VARCHAR2(255 CHAR)">
                <constraints nullable="false" primaryKey="true" primaryKeyName="username_primary_key"/>
            </column>

            <column name="last_name" type="NUMBER(19, 0)">
                <constraints nullable="false" primaryKey="true" primaryKeyName="username_primary_key"/>
            </column>

            <column name="CREATED_BY" type="NUMBER(19, 0)"/>
            <column name="CREATED_DATE" type="TIMESTAMP(6)"/>
            <column name="LAST_MODIFIED_BY" type="NUMBER(19, 0)"/>
            <column name="LAST_MODIFIED_DATE" type="TIMESTAMP(6)"/>
        </createTable>
    </changeSet>

    <changeSet author="mehmet" id="changeSetId2">
        <createTable tableName="orders">

        </createTable>
    </changeSet>
</databaseChangeLog>
```

Yukarıdaki liquibase.xml dosyasını bu şekilde ilgili DB'de işlem görmesini sağlayabiliriz:

```sh
"/path/to/liquibase" --driver="oracle.JDBC.OracleDriver" --classpath="/path/to/ojdbc7-12.1.0.2.jar" --url='JDBC:oracle:thin:@www.custom-db.com:1521/MY_DB' --username="user" --password="password123" --changeLogFile="liquibase.xml" update
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# savepoint
"SAVE TRANSACTION" keyword'ü ile transaction içerisinde gelinen noktaya rollback yapılmasını sağlayan keyword'dür.

örnek:

```sql
BEGIN TRANSACTION
   INSERT INTO TestTable( ID, Value ) VALUES  ( 1, 'hi')
   SAVE TRANSACTION FirstInsert
   INSERT INTO TestTable( ID, Value ) VALUES  ( 2, 'hello')
   ROLLBACK TRANSACTION FirstInsert
COMMIT
```

Birçok programlama dilinde olduğu gibi SQL'de de try-catch vardır. Savepoint genelde aşağıdaki amaçlar için kullanılır:
- try-catch bloğunda hataya düşünce eski bir savepoint'e dönmek isteyebiliriz.
- try-catch kullanmayız. fakat birkaç SQL-işlemi yatıktan sonra, SELECT ile update edilen bir datayı çekeriz, ve olması gerektiği gibi mi diye kontrol ederiz (bir çeşit manuel business validasyonu gibi). Burada beklediğimiz sonucu alamazsak, eski bir savepoint'e dönmek isteyebiliriz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Oracle Exadata (or Oracle Exadata Database Machine)
sadece Oracle database kullanımı için optimize edilmiş, Oracle tarafından satılan fiziksel sunuculardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# varchar vs char vs nvarchar vs nchar

"n" prefix'i string'in UNICODE karakterlerinin tümünü desteklediği anlamına gelir. "n" olmayan string'lerde UNICODE'un sadece en sık kullanılan "plane 0"'ı (başka başlıkta anlatılıyor) destekleniyor.

- CHAR is fixed-size. char(100), 100 karaktere store etmese bile 100 karakterlik yer harcar. sürekli bu alanı reserved tutar.
- VARCHAR is variable-size. hücre, sadece karakterler sayısı kadar yer kaplar.

char; varchar'a göre çok daha performans sağlar fakat daha çok yer kaplar.

# Oracle Data Types
Oracle'da internal ve external data tipler mevcut.

- VARCHAR2, NVARCHAR, CHAR, NCHAR internal datatype olarak geçiyor.

- VARCHAR2 aynı zamanda external dataype olarak geçiyor.

# blob
blob kelime anlamı: damla

context'e göre farklı anlamları var:

-  açık kaynaklı bir OS çekirdeğinde; proprietary device driver (closed-source device driver)'ları için kullanılan bir terimdir. buradan yola çıkan bazı insanlar, daha sonra binary blob terimini açık kaynaklı olan bir yazılımın içindeki kapalı kod binary'leri içinde kullanmaya başlamışlardır.

- database sistemlerinde; __Binary Large OBject (or BLOB)__ olarak kullanılıyor. bu objeler media objeleri gibi herhangi bir binary dosyaları olabilir.

# Spring ile BLOB tanımlama
Database tarafında __CLOB (or Character large object)__ olarak kullanılırken, BLOB binary objeler tutar.

Spring bu ikisi için tek anotation sunar ve field'ın tipinden (String veya byte[]) CLOB veya BLOB olup olmadığını anlar.

```java
import javax.persistence.Lob;
import javax.persistence.Column;

@Lob
@Column
private String name;

@Lob
@Column
private byte[] photo;
```

# String tiplerin farklılıkları
varchar gibi tiplerin maximum büyüklükleri sınırlı olabiliyor. Oysa CBLOB kullanıldığında bir satırın büyüklüğü terabayte'lara kadar çıkabiliyor.

CBLOB, belli br karakter seti kabul edeceği için, bazı karakterlerde hata fırlatabilir veya hata fırlatmaz fakat "cast" işlemine tabi tutulabilir. bu sebeple karakter setine dikkat edilmelidir. eğer karakter setinini bilmiyorsak veya emin değilsek BLOB kullanmalıyız.

Bazı veritabanlarında karakter setini baştan verebiliyor. Örneğin MYSQL'de aşağıdaki gibi tanımlama yapılabilir:

```sql
CREATE TABLE t
(
  c1   VARCHAR(20)    CHARACTER SET utf8, -- utf8 is encoding but it has supported character set (bu durum başka başlıkta anlatılıyor)
  c2   TEXT           CHARACTER SET latin1 COLLATE latin1_general_cs -- cs ==> case-sensitive
);
-- collate kelime anlamı: harmanlama.
```

Her veritabanı birçok farklı data type destekliyor. Örneğin; "JSON" tipi destekleniyor olablir. Böyle durumlarda JSON'un validasyonu da veritabanı tarafında da yapılabilir. JSON manipülasyonları için özel SQL fonksiyonları da sunuluyor olabilir...

# comparison of data types between database systems
"Migration from Oracle to MySQL" gibi terimlerle aratıldığında çıkan dökümanlarda data tiplerinin karşılaştırılması bulunabilir. Örnekler:

- https://cloud.google.com/solutions/migrating-oracle-users-to-mysql-data-users-tables title: "Oracle to MySQL data type conversion".
- https://dev.mysql.com/doc/workbench/en/wb-migration-database-mssql-typemapping.html
- https://dev.mysql.com/doc/workbench/en/wb-migration-database-postgresql-typemapping.html

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# code first vs database first vs model first
birer database yazılım ilişki kurma yaklaşımlarıdır (or approach).

- ## code first
önce kod yazılır, ardından entity framework yazılımı database'yi oto oluşturur.

- ## database first
önce SQL kodları ile database oluşturulur, ardından yazılımcı buna denk gelen modelleri kodda yazar.

- ## model first
genelde IDE'lerde database dizayn araçları ile yapılan bir işlemdir. database'yi bu araçta hazırlarsınız, bu araç hem kod hem de database tarafını otomatik oluşturur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# obje
veritabanı dünyasında obje; tablo, view, index'lerin (ve daha fazlası fakat herşey değil!) grup ismidir. bu durumda; örneğin USER tablosu bir database instance'sidir.

# Schema (or şema)
1 veritabanında birden fazla şema olabilir. her şema altında birden fazla tablo vardır. Şema aslında tabloların, ilişkilerin ve daha birçok database objesinin tanımıdır.

A user'ı, Y şemasının altındaki tüm objelere erişebilir gibi ayarlamalar kolayca yapılabilir.

bir tablonun ismini yazarken şu şekilde formatlarız: şema_ismi.tablo_ismi

Şema bazı DB'lerde yok bazılarında ise farklı anlamlarda kullanılabiliyor. DB'den DB'ye çok değişiyor (bu konu başka başlıkta anlatılıyor).

# Constraint
Türkçe kelime anlamı: kısıt

şemayı oluştururken/değiştirirken tanımlanan, tabloların yapısını kısıtlayan kurallardır. örnek constraint'ler:
- primary key
- unique
- not null
- foreign key

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# In-memory DB

# data grid vs data structure store vs key-value store
grid kelime anlamı: kafes, şebeke

__data grid__; data'nın birçok node'a paylaştırılarak(distributed) yönetilmesini sağlayan sistemlere/mimarilere verilen genel ifadedir.

"data grid" terimi; '__grid computing__'den gelir. kaynak: (source-id: 95) 1inci paragraf.

__in-memory data grid (or IMDG)__ terimi ise isminden de anlaşıldığı gibi RAM'de yönetilen data-grid'lerdir. kaynak: (source-id: 95) "Example Use Cases" başlığı, 4üncü paragraf.

__key-value__ DB'leri sadece bir key'e karşılık herhangi bir tipte veri tutarlar. Fakat __data structure__ database'leri key tarafta list, set, array gibi daha karmaşık veri yapılarını da destekler. dolayısı ile __data structure__ database'leri aynı zamanda key-value veri yapısını da desteklemiş olur. kaynak: (source-id: 97) "What is Redis?" 2inci ve 3üncü paragraf.

# local cache
In-memory DB'lerin önemli özelliklerinden biri de local-cache özelliğidir. Redis gibi data store'ları genelde kalıcı data için değilde, çok kısa vadeli data'ları tutmak için kullanılırız. Bu sebeple genelde kayıt atan uygulama aynı data'yı tekrar okur. Bu sebeple local cache özelliği vardır. local-cache okunan bilgiyi lokalde tutar ve her güncellemesini de sunucu ile sync tutar. Bu şekilde tekrar okuması gerekirse direk lokal'den okur. Local-cache policy'leri DB'den DB'ye çok fark edebiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# distributed cache
NoSQL'lerdeki distributed ile apaynıdır. Tek farkı verinin kalıcı olan diil, hızlı erişim için gerekli bilgileri (çoğunlukla geçici) bellekte depolar.

örnek sistemler:
- CDN
- Redis, Hazelcast gibi yazılımların instance'larını distributed data tutması için multi-cluster ayarlayabiliriz.
- Her Java microservisimiz kendi içinde API respnse'larını cache'leyebilir. Aynı parametrelerle istek atıldığında aynı cevabı döner. (EK not: burada API'yi ilgilendiren bir data'da değişiklik olunca tüm microservislere reset-cache isteği yollanmalıdır).

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Distributed lock
Tüm client'ların update etmesini engellediğimiz data'ları lock'lamamız gerekebilir. Distributed bir sistemde lock yaptığımızda, "Distributed lock" yapmış oluruz. Distributed lock'ların client'lar tarafından evict (serbest bırakılması) gerekir. Sunucuda lock'ların listesi ve statüleri tutulur. Hangi data'nın lock'lanılacağına karışılmaz. Aynı şekilde Java'da standart Lock sınıfları da bu şekilde çalışmaktadır. Lock'lanan kod bloğunun ne içerdiği ile ilgilenilmez.

"Distributed transaction" ile "Distributed lock" farklı kavramlardır. "Distributed transaction" için saga veya 2PC(SQL transaction desteği) gibi çözümlere gidebiliriz. "Distributed Lock" ise farklı node'larda duran bir verinin belli bir süre farklı client'lar tarafından okunmaması veya yazılmaması için gerekli özelliktir.

# lock'a sahip client'ın çökme durumu
Client çöker ise ve sunucudaki "lock" bilgisini evict edemezse durumu için 2 farklı çözüm var:
- her lock işleminde, lock için timeout süresi belirtilir. Tanımlanan bu timeout süresi sonrasında sunucu, otomatik "lock" bilgisini evict eder/unclock eder.
- client tarafta watchdog olur. bu bağımsız bir thread'dir. her 30 saniyede bir eğer lock eden thread hala ayakta ise, lock'ın sresini uzatır. eğer lock'a sahip olan thread çökerse, lock'ı kaldırır. program tümüyle çökerse bu çözüm bir işe yaramaz. fakat thread beklenmedik şekilde kapanırsa bu çözüm hayat kurtarır.

## Sınıflar
Java, Standart olması açısından Lock için bir interface ortaya atmıştır: "__java.util.concurrent.locks.Lock__".

Redisson ise __java.util.concurrent.locks.Lock__'ta türemiş __org.redisson.api.RLock__ interfacesini kullanır.

## Sadece lock'a sahip thread, o lock'ı kaldırabilir
Redisson'un ve Java'nın tüm __java.util.concurrent.locks.Lock__ implementasyonlarıda, lock'a sahip olan (acquire eden) Java thread'i ancak lock'ı serbest bırakabilir. Başka thread lock'ı kaldırmak isterse IllegalMonitorStateException fırlatılır. Eğer başka client'a bu yetki verilecek ise Lock yerine "__java.util.concurrent.Semaphore__" kullanılmalıdır.

not: Bir uygulamada birden fazla client olabilir, hatta 1 thread'de de birden fazla bağımsız client olabilir.

## Redisson Lock implementasyonları
Redisson'un lock için sunduğu implementasyonların bazılarına bakalım. aşağıdaki tüm açıklamalar için kaynak: (source-id: 98)

Aşağıdaki lock mekanizmalarının desteklenmesi için Redis (sunucu) tarafında birçok metadata'nın tutulması gerekebilir.

- __Lock__

  Klasik lock işlemi yapar.

  ```java
  // redissonClient tüm lock'ların listesini (instance'larını) tutuyor. bu sebeple; getLock metotu ile aynı instance'ı istediğimiz yerden çağırabiliriz.
  RLock lock = redissonClient.getLock("myLock");

  lock.lock();
  // or acquire lock and automatically unlock it after 10 seconds
  lock.lock(10, TimeUnit.SECONDS);

  // or wait for lock aquisition up to 100 seconds
  // and automatically unlock it after 10 seconds
  boolean res = lock.tryLock(100, 10, TimeUnit.SECONDS);

  // do anything you want
  if (res) {
    try {
      // do anything here. Redis does cares about what writes here..
    } finally {
        lock.unlock();
    }
  }
  ```

- __Fair Lock__

  ```java
  RLock lock = redissonClient.getFairLock("myLock");
  ```

  "Redisson" (connection) instance'ındaki tüm thread'ler hangi sıra ile lock isteği yolluyorsa, o sıra ile lock'ları acquire edebilecekleri şekilde ayarlanmış lock implementasyonudur. aynı thread'in içindeki lock sırası değil, tüm thread'lerdeki lock sıralaması göz önünde bulundurulur.

- MultiLock

  birden fazla lock'ı grup içine alarak tek bir lock nesnesi üzerinden işlemlere devam edebiliriz.

  ```java
  RLock lock1 = redissonClient1.getLock("lock1");
  RLock lock2 = redissonClient2.getLock("lock2");
  RLock lock3 = redissonClient3.getLock("lock3");

  RLock multiLock = anyRedissonClient.getMultiLock(lock1, lock2, lock3);
  ```

- __RedLock__

  Redis'in ortaya attığı bir algoritmadır.

- __ReadWriteLock__

  Lock mekanizmasının genel ismi bu olarak geçse de; interface olarak buna denk gelir: __org.redisson.API.RReadWriteLock__. RReadWriteLock ise Java'daki __java.util.concurrent.locks.ReadWriteLock__ arayüzünden türer. Sınıf olarakta __org.redisson.RedissonReadWriteLock__ kullanılır.

  Read-Write-Lock'lar genel olarak hem read hem de write için ayrı ayrı seçenek sunan Lock'lardır. Örneğin read'i birden çok instance yapabilirken, write'ı sadece 1 instance'a izin verilir...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## Redis
- belirli aralıklarla (isteğe bağlı olarak) verileri dosyaya kaydedebilir.
- tümüyle c ile yazılmıştır. tek thread çalışır.

Redis client'ları:
- Lettuce
- Redisson
- Jedis
- Spring-data-Redis (arkaplanda Jedis yada farklı bir implementasyonu kullanır)

Terimler hakkında:
- Redis Lab; Redis'in enterprise versiyonunu geliştiren firmanın ismi.
- Redis lab'in kendi AWS'lerinde bize hazır sunduğu Redis'ler var. bu hizmetin özel adı "Redis Enterprise Essentials" dır.
- "Redis Enterprise Pro" ise Redis lab'ın sunduğu AWS entegrasyonudur. Kendi AWS hesabımızda kullanacağımız Redis'lerdir.

yukarıdaki 3 madde için kaynak: (source-id: 99)

Bu sayfada (source-id: 100) hangi özelliğin Redis lab veya open source proje tarafından geliştirdiği anlaşılabilir. Redis.io olan linkler Redis open source projesi tarafından destekleniyor fakat Redislabs.com'a yönlendirilen linkler sadece "Redis labs" tarafından destekleniyor.

# data types
Rediste her bilgi bir key'e karşılık tutulur. Örneğin bir liste tutacak isek bunun bir key'i vardır. o key aracılığı ile bu listeye erişebiliriz. 1 adet key bazen sadece 1 adet string tutmak için kullanılır.

Redis aşağıdaki tiplerden daha fazlasını destekler. Aynı zamanda aşağıdaki tipler içerisinde data'yı bit bazında okuma gibi birçok ek özellik sunar.

## String
bir key karşılığında bir string saklar. Örnek redis komut satırı ile bir key oluşturabiliriz:

```
> SET mykey "10"
```

var olan string'e append, eğer sayı ise 1 arttırma gibi özel komutlarda sunar.

## List
Redis komut satırları örnekler:

```
> LPUSH mylist a   # now the list is "a"
> LPUSH mylist b   # now the list is "b","a" # append to left side - left push
> RPUSH mylist c   # now the list is "b","a","c" # append to right side - right push
```

## Set
1 key'e karşılık tüm set'i barındırıyoruz.

```
> sadd myset 1 2 3
(integer) 3

> smembers myset
1. 3
2. 1
3. 2
```

## Hashes
1 key'e karşılık bir hash-map barındırıyoruz.

```
# Redis 4 ile birlikte, HMSET komutu HSET olarak değiştirildi.
> HSET myhash field1 "Hello"
(integer) 1

> HGET myhash field1
"Hello"
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# in-memory non-RDBMS

| name       | developer  | language | open source |
|------------|------------|----------|-------------|
| Infinispan |            | Java     | yes         |
| Hazelcast  | Hazelcast  | Java     | yes         |
| memcached  |            | c        | yes         |
| Redis      | Redis labs | c        | yes         |
| Ehcache    |            | Java     | yes         |

memcached, Redis'e göre daha az özellik içerir. kaynak: (source-id: 101) + (source-id: 102) "Which is better: Redis or Memcached" başlığı.

Redis için çok fazla 3üncü parti eklenti bulunur. Redis zaten çok gelişmiştir ve bu sebeple çok fazla hakimiyet ister. Bu sebeple çok gelişmiş özellikler kullanılmayacak ise memcached tercih edilmelidir.

tümüyle Java ile yazılanları özellikle production ortamında JVM'i de yönetmek gerekebilir. Özellikle büyük data'larda heap taştığında, JVM konularına büyük hakimiyet gerektirebilir.

# in-memory RDBMS

| name                 | open source | developer | language |
|----------------------|-------------|-----------|----------|
| h2                   | yes         |           | Java     |
| Derby                | yes         | Apache    | Java     |
| HSQLDB (or HyperSQL) | yes         |           | Java     |

# Ignite
- Apache vakfı tarafından geliştiriliyor.
- Açık kaynaklı.
- Java ile yazılmış.
- in-memory non-RDBMS kategorisindedir. Fakat SQL (hem manipülasyon, hem query) atılması için de ömzel bir feature'si mevcut. Fakat arkada gerçek tablo tutmuyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# RDBMS

## MySQL vs MariaDB
MySQL, Oracle tarafından geliştirilen open source bir DB'dir. ilk geliştirildiğinde Oracle'da değildi, fakat Oracle satın alınca community MariaDB'yi oluşturdu. MariaDB gerçek bir community ürünüdür ve geliştirme süreçleri bile takip edilebilir durumdadır. Oracle'ın kendi DB'si olduğundan, birçok proje, uzun vadede lisans veya farklı problemlerden kaçındığı için MariaDB kullanmaktadır.

MariaDB, resmi olarak "drop-in replacement of MySql" olarak sunulmaktadır. kaynak: (source-id: 429) ilk cümle.

yani MySQL yerine hemen geçilebilir durumdadır. Fakat son yıllarda geçiş iş yükü (farklılıklar) gitgide artmaktadır. MySQL client'ları MariaDB'ye de bağlanabilir, SQL syntax'ları aynıdır, desteklediği veri tipleri aynıdır...

Her sürüm için farklılıklar ayrı ayrı dökümante edilmiş: (source-id: 103)

## PostgreSQL (or postgres)
performance açısından MariaDB ve MySQL'e göre övülen açık kaynaklı bir DB.

- unsupported features:

(source-id: 104)

- supported features:

(source-id: 105)

## Microsoft SQL server (or MSSQL) vs Oracle DB
- genel olarak Oracle daha gelişmiş özellikler içeriyor fakat yönetimi daha zor.
- Oracle, OS bağımsız çalışırken, MSSQL sadece Microsoft Windows gibi limitli birkaç OS'ta çalışabiliyor.
- MSSQL Microsoft'un diğer ürünleri ile direk olarak destekleniyor. örneğin Linq'nın diğer database'lere olan entegrasyonu daha sonradan eklendi.

## DB2
IBM tarafından geliştirilen ilişkisel veritabanı. güncel sürümlerinde NoSQL tarzı özelliklerde sunmaya başlamıştır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# file based

## SQLite
sadece bir dosya içinde saklanabilen ilişkisel veritabanı altyapısıdır. sunucu runtime yazılımına ihtiyaç duymaz. çünkü sadece 1 tek dosyadan ibarettir. bu dosyaya erişenler dosyayı okuyup veritabanından neler olduğunu görebilir. aslında sadece bir dosya formatıdır. yazılım veya bir servis değildir.

## Microsoft Access

## LibreOffice Base

## comma seperated values (cvs)

# embedded db
client tarafta executable'ın içerisinde de açılabilen DB'lerdir.

her file based db, embedded kategorisindedir.

embedded-db kategorisinde ek olarak şunlar var:

- ## H2

- ## Apache derby

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# strong entity vs weak entity

Bir tablo (entity) sadece kendi başına bir objeyi ifade edebiliyor (kendi başına yaşayabiliyor / anlam ifade ediyor ise) o tablo güçlüdür. tersi durumda zayıftır. örneğin; "oda" zayıf bir nesnedir. çünkü bina olmadan, oda hiçbir şey ifade etmez. fakat bina tek başına yeterlidir.

Aslında tam olarak strong mu olup olmadığını tespit etmek için resmi bir yöntem var. şu örnekten gidelim:

Customer(customerid, name, surname) --> dışarıdan tablo referans edilmesine gerek yok. kendi başına yeterli. güçlü.

Address(addressId, addressName, customerId) --> customerId ile customer verilmesi lazım. Address güçsüz bir varlık.

Adres bir müşteriye ait olmasa; güçlü olacaktı. yani; veritabanına göre gerçek hayattaki nesneler güçlü yada güçsüz olabilir. gerçek hayattan örnek vererek tespit doğru değildir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# aday anahtar (or candidate key)
join yada benzeri bir sorgu sonucu dönen sonuç tablosunda; bir yada birden sütun, o tablo için private key oluyorsa; bu sütunlar aday anahtar demek oluyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# data domain
veritabanlarında data domain; bir sütunun alabileceği değerler kümesini belirtmektedir. örneğin; tc, 10000000 ile 99999999 arasında değer alabilir. is_enabled, true yada false alabilir gibi...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Normalizasyon
tekrarlanmayı engellemek için best practice'lere uygun veritabanı modeli uygulamak için yapılan işlemlerdir.

# Denormalized
1NF'in dahi karşılanmadığı veritabanı dizaynıdır.

Aşağıdaki örnek veritabanında Adres bilgileri çalıştığı ofisin adres bilgileridir. sütun ismi uzun olacağından, anlaşılır bir sütun isimi yazılmadı.

> ||| AdresId ||| Name ||| Surname ||| Adres1_İlçe ||| Adres2_İlçe ||| Calistigi_Ofis |||

# 1NF (or 1st normal form)
normal form: model/tip anlamına gelen norm ve form kelimelerinin birleşimidir.

- You can only have one value in a column (virgüllerle aynı sütunda değer olmamalı)
 -You should not create multiple columns for a one to many relationship

> ||| AdresId ||| Name ||| Surname ||| Adres ||| İlçe ||| Calistigi_Ofis |||

Adres2_İl'i eskiden dolu olan satırlar 2 satır olarak ayrı ayrı yazılmak durumunda kalacaklardır.

# 2NF
Her NF, derecesinden düşük NF'leri zaten destekliyor olması gerekiyor.

- (primary key birden fazla sütundan oluşuyor olabilir. bunu gö önünde bulundurarak;) her non-primary sütun; primary sütuna bağlı değilde, sadece 1 parçasına(sütununa) bağlıysa, o parçasına bağlı olduğu sütun ile birlikte farklı bir tabloya alınmalıdır.

Yukarıdaki örnekte primary-key: Name+Surname+Calistigi_Ofis birliktedir. Adres bilgileri sadece Calistigi_Ofis'ya aittir. bu sebeple farklı tabloya taşınırlar.

> ||| Name ||| Surname ||| Calistigi_Ofis |||

> ||| AdresId ||| Adres ||| İlçe ||| Calistigi_Ofis |||

# 3NF

- her non-primary sütun direk olarak sadece primary sütuna bağlı olmalıdır.

Örnekte ilk tablo olmuş 2NF'e uymuş. ikinci tabloda 2NF'e uymuş. bu durumda 3NF'i kontrol etmek gerekmekte. ilk tablo 3NF'e uymuş. fakat ikinci tablo 3NF'e uymamış. çünkü; ilçe sütunu adresId'ye tam bağımlı değil. zaten bu sebepten tekrarlanabilir veri oluşturuyor. örneğin; ilçe 10 kere tekrar edilebilir o sütunda. ilçe farklı bir tabloya atılmalı.

> ||| Name ||| Surname ||| Calistigi_Ofis |||

> ||| AdresId ||| Adres ||| İlçeId ||| Calistigi_Ofis |||

> ||| İlçeId ||| İlçe |||

# Diğer formlar

- Boyce–Codd normal form (or BCNF or 3.5NF)

- 4NF

Fakat en çok ilk üçünden bahsedilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Grafana connection pool metrics

## "Connections" panel
- Active: The connection is using by a thread.
- Idle: The connection is inside the pool and waiting for a thread to be used.
- Pending threads: The thread count which are waiting for a connection from the pool.

## "Connections time" panel
- Usage: When a connection is active, that means, it is being used by a thread. This metric shows how much time the connection was used by the thread.
- Creation: A connection takes time to be created. This metrics show how much it took to create them.
- Acquire: This metrics shows the time a thread needed to acquire a idle connection and move it to active connection. 

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# function vs stored prosedure
fonksiyonlar veritabanında insert, update, delete gibi herhangi bir değişiklik yapacak işlem yapamazlar. bunun sonucu olarak (mantıken); çoğu zaman en az bir parametre alması ve mutlaka bir değer döndürmesi gereklidir.

fakat stored prosedureler veritabanında değişiklik yapabilirler.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Unique Key (or UK) vs Primary Key (or PK)

bir sütun tekil değerler içeriyor ise unique'tir. unique sütunlar null değerler alabilir.

veritabanlarında bir sütunun unique olduğu belirtildiğinde; veritabanı sadece aynı değerlerin kaydedilmesini önleyecektir.

PK ise; veritabanının o sütunu diğer tablolar ile ilişkilendirmek için kullanacağını belirtmek için kullanılır. ilişkilendirme olabilmesi için PK, unique'tir.

Bir tabloda Unique sütun birden fazla olabilir. Oysa PK, bir tanedir. PK bazı durumlarda birden fazla sütunu birden kapsamaktadır. Fakat o sütunlar tek başına bir adet PK olarak ele alınır. Yani PK yine bir adettir (fakat birden fazla sütundan oluşur). Bu tarz PK'lara composite (bileşik) sıfatı verilmektedir.

Best practice açısından her tabloda bir primary key olması önerilir.

# surrogate key vs natural key
surrogate kelime anlamı: vekil.

her tablodaki primary key bir bilgiye tekabül etmemesi (surrogate key) best practice'dir . örneğin tc (natural key) primary key olmamalıdır. bunun temel 2 sebebi:

- primary key update yapmak zordur (diğer tablolarda aynı anda update gerekir vs) (örneğin kişinin TC'si değişirse, update işlemi zor olabilir)

- primary key'in ömür boyu unique olmayacağı veya hiçbir durumda null olmayacağının garantisini vermek zordur. bu garanti bugün verilse de yarın bu karar business gereği değişebilir. fakat eğer incremental bir ID kullanılsa, business değişikliklerinde primary key'lerde update gerekmeyecek.

Bunun bir dezavantajları:

- SQL query'lerinin sürekli olarak primary key'e göre yazılmasını gerektirmektedir. bu sadece manuel yazılan query'lerde değil, programatik query'leri de yoracak. yani performance'ı olumsuz etkileyecek.

- tc örneği üzerinden gidelim. çoğu projede PK index'lenir. PK, sadece tc olsaydı, sadece TC'yi index'leyecektik. fakat surrogate key varsa onunda index'lenmesi gerekecek. bu da database sistemine maliyeti arttıracak.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# primary key generation strategy
performans için önemli bir konu.

```java
@javax.persistence.Id // PK olduğunu JPA'ya bildirir.
@javax.persistence.GeneratedValue(strategy = GenerationType.AUTO) // GeneratedValue, bu field'ın oto-generated value olduğunu tanımlar.
Integer id;
```

GenerationType sınıfının içini incelersek:

```java
package javax.persistence;
/**
  * Defines the types of primary key generation strategies.
  *
  */
public enum GenerationType {
    /**
      * Indicates that the persistence provider must assign
      * primary keys for the entity using an underlying
      * database table to ensure uniqueness.
      */
    TABLE,
    /**
      * Indicates that the persistence provider must assign
      * primary keys for the entity using a database sequence.
      */
    SEQUENCE,
    /**
      * Indicates that the persistence provider must assign
      * primary keys for the entity using a database identity column.
      */
    IDENTITY,
    /**
      * Indicates that the persistence provider should pick an
      * appropriate strategy for the particular database. The
      * <code>AUTO</code> generation strategy may expect a database
      * resource to exist, or it may attempt to create one. A vendor
      * may provide documentation on how to create such resources
      * in the event that it does not support schema generation
      * or cannot create the schema resource at runtime.
      */
    AUTO
}
```

- AUTO

  GeneratedValue için default "strategy" değerdir.

  burada JPA kendisi, hangi GenerationType'ın kullanılacağına karar veriyor. Genelde SEQUENCE seçiliyor.

- IDENTITY

  ID değeri entity kayıt edildiğinde DB tarafından oluşturuluyor. bu sebeple aynı entity'den batch işlemlerinde performance konusunda sıkıntı yaşatıyor, çünkü bir işlem bitmeden, diğer işlemi JPA üstüste yollayamıyor. Burada dikkat: 10 adet liste tek JPA query'sinde DB ye yollanırsa sorun yok. Asıl performans sorunu üstüste farklı JPA insert query'si yapılırsa oluyor.

- SEQUENCE

  DB'de sequence kavramı var. JPA sequence'den ilgili entity'nin ID değerini çekiyor. bu IDENTITY'ye göre daha hızlı. JPA'nın bir kere DB'den sequence'i çekmesi yeterli. Çektiği zaman belli bir dilimi allocate edebiliyor. Bunu da bu şekilde set ediyoruz:

  ```java
  @Id
  @GeneratedValue(strategy = GenerationType.SEQUENCE, generator = "sequence1")
  @SequenceGenerator(name = "sequence1", allocationSize = 50)
  private Integer id;
  ```

- TABLE

  JPA'nın kendisi bir tablo oluşturuyor ve bu tabloda sequence'yi tutuyor. Performance için pek tercih edilen yöntem değil. Çünkü JPA'nın Her zaman bu tabloya query atması gerekiyor. Hangi tabloda tuttuğunu entity tarafında belirtmiyoruz. (JPA config'lerinde belirtiliyor olabilir). Yani biz yine field'ı bu şekilde tanımlıyoruz:

  ```java
  @javax.persistence.Id
  @javax.persistence.GeneratedValue(strategy = GenerationType.TABLE)
  Integer id;
  ```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# problems of NoSQL

# celebrity problem (or hotspot key problem)
Bazı node'daki verilerin bir kısmının, diğer node'daki datalara göre çok daha sık kullanılması problemidir. Örneğin, korona döneminde, "maske" çok
satışı çok olduğu için e-ticaret sitelerinde bu ürünlerin aramaları aşırı derecede artış gösterdi. Bu gibi durumlarda bu ürünleri özel node'lara taşımak ve sadece onları scale etmek gerekebilir.

# resharding data
bazen, fiziksel olarak aynı node'da duran data'ların farklı node'lara taşınması gerekebilir. bu işlem bolca kaynak tüketen bir işlemdir.

# join of de-normalized data problem
denormalize olmuş tablolardan bilgi çekmek için join yapmak gerekmektedir. bu gibi durumlarda bu dataları direk tek tabloda bulundurmak yararlı olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# sütun index'lemesi
bir sütun index'lendiğinde artık üzerinde daha hızlı query çalıştırılabilir hale gelir. fakat her insert veya update edilişi yavaşlar çünkü index'in de update edilmesi gerekecektir.

index kullanımı sunucuda memory kullanımını da arttırabilirken, sunucu açılma süresini de yavaşlatabilir.

```sql
-- CREATE UNIQUE INDEX my_index_name --> creates "primary index"

CREATE INDEX my_index_name -- --> Creates "secondary index"
ON Persons (LastName)
```

index'i kaldırma işlemi:

```sql
DROP INDEX index_name ON table_name
```

# primary index vs secondary index
primary index'ler sadece unique olan sütunlara atanabilir. oysa secondary index'ler unique olmayan sütunlara da atanabilir.

# clustered index vs non-clustered index
sıralı (clustered) olması istenen sütunlar clustered-index olması daha uygundur. ilgili tabloyu fiziksel olarak clustered-index yapılan sütuna göre sort'lar. dolayısı ile en fazla 1 adet clustered index'imizi olabilir.

primary key + incremental + integer olan sütunlar otomatik olarak "clustered index" olarak ayarlanabilir.

non-clustered index'ler sırasız şekilde sütunu ayrı bir fiziksel bellekte tutar.

# index strategy
index'lerin fiziksel olarak tutulduğu data-structure'ye denir. hangi strategy'nin kullanılacağını index oluştururken belirtebiliyoruz. eğer belirtmezsek default olan kullanılır. en sık kullanılan strategy'ler:
- B-Tree (binary tree'den farklıdır)
- B+tree (binary tree'den farklıdır)
- Bitmap

# cardinality (or πληθάριθμος or πληθικός αριθμός)
Sözlükte Türkçe karşılığını bulamadım. Fakat piyasada "kardinalite" olarak kullanılıyor.

kümenin eleman sayısıdır. (kümede aynı eleman olmadığını unutmayalım).

Veritabanı dünyasında, bir sütunun cardinality'sinin yüksek veya az olduğundan bahsedilir. tekrarlanan veri varsa cardinality düşüktür. tekrarlanan veri az ise cardinality yüksektir. önrk olarak cinsiyet sutununun kardinalitesi düşüktür.

Veritabanında index alırken çok düşük kardinalitesi olan veya sütunun toplam elemen sayısı az olan tabloların index'inin alınması önerilmez.

# unique column
unique sütunlar index'lenebilir fakat bir sütunun unique olduğunu veritabanına belirtmek ona atılan bazı sorguları aşırı hızlandırır. çünkü unique olmayan bir sütunda arama yapıldığında, birden fazla satır dönebileceği için tüm tablo aranmak zorundadır. Oysa unique bir sütunda arama yapıldığında, ilk sonuç bulunduğunda query sonlandırılır. çünkü devam etmeye gerek yoktur.

# unique column with index vs non-unique column with unique index
veritabanlarında "UNIQUE INDEX" oluşturabiliriz veya bir sütunu UNIQUE yapıp ona index atayabiliriz. Aralarında çok çok ince farklılıklar var. kaynak: (source-id: 460) 1st paragraph, 3th sentence.

# composite index

```sql
CREATE INDEX my_index ON users (username, age);
```

query'lerimizin where koşulunda sıkça username ve age sütunlarını görüyorsak o zaman bunları composite index yapmalıyız. Composite-index'ler şu şekilde tutulurlar:

| composite-index-key | primary-key-of-table |
|---------------------|----------------------|
| ahmet-30            | 99                   |
| ahmet-40            | 10                   |
| ahmet-50            | 26                   |
| cemal-20            | 190                  |
| cemal-60            | 100                  |
| zeynep-20           | 45                   |

Artık hem username hem de age sütununu where koşulunda bulunduğumuz sorgular hızlanacaktır.

Ek olarak where koşulunda sadece username olan sorgular da hızlanacaktır. Çünkü zaten username, composite-index-key'in ilk sırasında. Oysa sadece "age" olan bir where koşulu composite-key'imizden hiç verim alamayacaktır. Çünkü composite-index-key'in ikinci alanında arama yapmak ile gerçek tablonun tümünde aramanın hiçbir farkı yoktur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# SQL sorguları yaptıkları işlere göre gruplandırılırlar:

DDL (Data Definition Language) --> şemalar üzerinde işlem yapan operatörler: alter, create table gibi.

DQL (data query language) --> SELECT, FROM, WHERE gibi.

DML (Data Manipulation Language) --> satırlar üzerinde işlem yapmaya yarayan operatörler: INSERT, UPDATE gibi.

DCL (Data Control Language) --> user'ların yetkilerini ayarlamak için kullanılan SQL operatörleri. grant, revoke gibi.

TCL (Transaction Control Language) --> Transaction işlemlerinde kullanılan SQL operatörleridir: COMMIT, ROLLBACK gibi.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# parsing
DB sunucusu, SQL sorgularını parse edip optimize eder ve execute eder. Bu işleme "__hard parsing__" denir. Oysa aynı sorgunun tekrar tüm bu işlemlere girmesine gerek yok. DB sunucusu, her SQL'in optimize edilmiş halini, bellekte tutar. Eğer aynısı gelirse bu sefer "__soft parsing__" yapar ve execute eder.

Oracle database'de büyük küçük harf dahi (örneğin; Select, SELECT) sorgunun farklı olmasına sebep olmaktadır. kaynak: (source-id: 106) "Why do bind variables matter for performance?" başlığı.

Buna çözüm olarak SQL'lerin elle yazılmaması önerilir. Bu şekilde case-sensitivity sorunu ortadan kalkar. Aynı zamanda 'statement'lara dynamic variable'ları bind ederek'te aynı SQL'in farklı değerlerle çalıştırıldığında, hard parsing yapılmamasını sağlamış oluruz. Çünkü; eğer binding yapmazsak; SQL aynı fakat içindeki herhangi bir integer farklı ise bile hard parsing yapılır. kaynak: (source-id: 106) "Where do bind variables come into this?" başlığı.

Hard parsing ile soft parsing arasında performance olarak büyük fark var. bu kaynakta; (source-id: 106) "What's the impact of hard parsing?" başlığındaki örnekte, for loop içerisinde aynı sorgu bind edilerek ve bind edilmeden yollanmış. Bir döngü 4 saniyede tamamlanırken, diğeri 1 buçuk dakikada tamamlanmış.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Mongo database
Mongo database'de veri yapıları ve ona denk gelen RDBMS karşılıkları aşağıdaki gibidir:

- collection --> tablo
- document -> satır

İlişkiler MondoDB'de tutulmamaktadır. fakat bu ilişkiler yazılımsal olarak sağlanabilir.

Mongo database de veriler JSON formatında tutuluyor. JSON formatı dışında bir tipte değer tutmak gerekirse onu string'e cast edip tutabiliriz. yada database dışında bir yerde tutup, onun URL'sini JSON string'i olarak saklayabiliriz.

NoSQL'de standart SQL query'ler olmayabilir. örneğin MongoDB'de şu şekilde sorular yazılır:

db.users.find( { status: "A", age: { $lt: 30 } } ) gibi sorgu ile sadece tek tablo üzerinde sorgu yapılabilir. $lt değeri aldığı integer değerden daha küçük tüm satırları temsil etmek için kullanılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# SQL (or Structured Query Language)
'sekuel' diye de okunur (eski ismine benzerliği sebebi ile).

SQL bir çeşit DSL'dir.

tüm ilişkisel veritabanları için ortak dildir. Tüm sistemlerde çalışabileceği için az özellik içerir.

__American National Standards Institute (or ANSI)__ ve __International Organization for Standardization (or ISO)__ kurumları __International Electrotechnical Commission (or IEC)__'a bağlıdır.

Both ANSI and the ISO/IEC have accepted SQL as the standard language for relational databases. When a new SQL standard is simultaneously published by these organizations, the names of the standards conform to conventions used by the organization, but the standards are technically identical.

# Standard SQL version history

| Year | Name     | Alias            | Comments                                                                                                                                                                                                                                                                                                                                                                                                                                                             |
|------|----------|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1986 | SQL-86   | SQL-87           | First formalized by ANSI                                                                                                                                                                                                                                                                                                                                                                                                                                             |
| 1989 | SQL-89   | FIPS 127-1       | Minor revision that added integrity constraints, adopted as FIPS 127-1                                                                                                                                                                                                                                                                                                                                                                                               |
| 1992 | SQL-92   | SQL2, FIPS 127-2 | Major revision (ISO 9075), Entry Level SQL-92 adopted as FIPS 127-2                                                                                                                                                                                                                                                                                                                                                                                                  |
| 1999 | SQL:1999 | SQL3             | Added regular expression matching, recursive queries (e.g. transitive closure), triggers, support for procedural and control-of-flow statements, nonscalar types (arrays), and some object-oriented features (e.g. structured types), support for embedding SQL in Java (SQL/OLB) and vice versa (SQL/JRT)                                                                                                                                                           |
| 2003 | SQL:2003 |                  | Introduced XML-related features (SQL/XML), window functions, standardized sequences, and columns with autogenerated values (including identity columns)                                                                                                                                                                                                                                                                                                              |
| 2006 | SQL:2006 |                  | ISO/IEC 9075-14:2006 defines ways that SQL can be used with XML. It defines ways of importing and storing XML data in an SQL database, manipulating it within the database, and publishing both XML and conventional SQL-data in XML form. In addition, it lets applications integrate queries into their SQL code with XQuery, the XML Query Language published by the World Wide Web Consortium (W3C), to concurrently access ordinary SQL-data and XML documents. |
| 2008 | SQL:2008 |                  | Legalizes ORDER BY outside cursor definitions. Adds INSTEAD OF triggers, TRUNCATE statement, FETCH clause                                                                                                                                                                                                                                                                                                                                                            |
| 2011 | SQL:2011 |                  | Adds temporal data (PERIOD FOR) (more information at: Temporal database#History). Enhancements for window functions and FETCH clause.                                                                                                                                                                                                                                                                                                                                |
| 2016 | SQL:2016 |                  | Adds row pattern matching, polymorphic table functions, JSON                                                                                                                                                                                                                                                                                                                                                                                                         |
| 2019 | SQL:2019 |                  | Adds Part 15, multidimensional arrays (MDarray type and operators)                                                                                                                                                                                                                                                                                                                                                                                                   |

# PL/SQL (or Procedural Language/SQL)
Oracle'ın SQL türevi dili.

# PL/pgSQL
PostgreSQL'ın SQL türevi dili.

# T-SQL (or Transact-SQL)
Microsoft SQL Server'ın SQL türevi dili.

# PL/SQL vs T-SQL
"Migration from Oracle to MSSQL" gibi terimlerle aratıldığında çıkan dökümanlarda data tiplerinin karşılaştırılması bulunabilir.

Bu konu için güzel bir kaynak: (source-id: 109) title: "Differences Between Oracle and MS SQL Server".

# Other extensions
Makalelerin çoğunda, PL/SQL, T-SQL gibi extension'ların "prosedürel" olduğu yazar. Bunun sebebi şu: Standard SQL daha çok data çekmek için tek satırlık işlemlere odaklanmış bir standart. Oysa PL/SQL, T-SQL ve diğer extension'lar transaction, fonksiyon, batch gibi bloklardan oluşan (yani prosedürel programlama metotolojisine uygun) SQL'lere daha çok önem veren özellikler içermesidir.

| Source               | Abbreviation    | Full name                                                                                 |
|----------------------|-----------------|-------------------------------------------------------------------------------------------|
| ANSI/ISO Standard    | SQL/PSM         | SQL/Persistent Stored Modules                                                             |
| Interbase / Firebird | PSQL            | Procedural SQL                                                                            |
| IBM DB2              | SQL PL          | SQL Procedural Language (implements SQL/PSM)                                              |
| IBM Informix         | SPL             | Stored Procedural Language                                                                |
| IBM Netezza          | NZPLSQL         | (based on Postgres PL/pgSQL)                                                              |
| Invantive            | PSQL            | Invantive Procedural SQL (implements SQL/PSM and PL/SQL)                                  |
| MariaDB              | SQL/PSM, PL/SQL | SQL/Persistent Stored Module (implements SQL/PSM), Procedural Language/SQL (based on Ada) |
| Microsoft / Sybase   | T-SQL           | Transact-SQL                                                                              |
| Mimer SQL            | SQL/PSM         | SQL/Persistent Stored Module (implements SQL/PSM)                                         |
| MySQL                | SQL/PSM         | SQL/Persistent Stored Module (implements SQL/PSM)                                         |
| MonetDB              | SQL/PSM         | SQL/Persistent Stored Module (implements SQL/PSM)                                         |
| NuoDB                | SSP             | Starkey Stored Procedures                                                                 |
| Oracle               | PL/SQL          | Procedural Language/SQL (based on Ada)                                                    |
| PostgreSQL           | PL/pgSQL        | Procedural Language/PostgreSQL Structured Query Language (based on reduced PL/SQL)        |
| SAP R/3              | ABAP            | Advanced Business Application Programming                                                 |
| SAP HANA             | SQLScript       | SQLScript                                                                                 |
| Sybase               | Watcom-SQL      | SQL Anywhere Watcom-SQL Dialect                                                           |
| Teradata             | SPL             | Stored Procedural Language                                                                |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# statement
SQL dilindeki her komut (select, update, create, commit ...) ve o komuta yollanan parametreler birlikte 1 statement'tır. Her statement noktalı virgül ile ayrılmaktadır. Fakat bazı platformlar noktalı virgül koyulmasını zorunlu kılmaz. yeni satıra geçmek yeterlidir.

1 transaction, sadece 1 statement'ten da oluşuyor olabilir.

sadece 1 statement yolladığımızda, arkaplanda bizim için 1 transaction oluşturuluyor. kaynak: (source-id: 110) "Autocommit transactions" başlığı altında " Each individual statement is a transaction." yazıyor.

# transaction
Türkçe'de kelime anlamı: işlem

multiple statement olmak zorunda değil. tüm SQL işlemleri birer transaction'dır ve DB Admin GUI'lere bakarsak "transaction log" (history)'de görürüz.

# batch (or script)
multiple statement'lar kümesidir. batch, özellikle belirtilmediği sürece "transactional" (atomik) değildir.

# batch on Hibernate
spring-batch modülü ile karıştırılmamalıdır. tamamen farklı kavramlardır.

Hibernate ile Java uygulamasından attığımız SQL'ler normal koşullarda tek tek TCP bağlantısı üzerinden database sunucusuna iletilmektedir. Fakat Hibernate, bunları gruplandırarak batch halinde atmamızı da sağlamaktadır.

Hibernate ayarlarında Batch=1 (hibernate.jdbc.batch_size=1) ise aşağıdaki for döngüsü, hemen aşağısındaki çıktıyı vermektedir.

```java
for (int i = 0; i < 10; i++) {
    School school = createSchool(i);
    entityManager.persist(school);
}
entityManager.flush();
```

```sql
-- pseudo SQL

INSERT INTO SCHOOL VALUES 1
INSERT INTO SCHOOL VALUES 2
-- others here
INSERT INTO SCHOOL VALUES 10
```

Batch=5 olsaydı aynı for döngüsü aşağıdaki gibi çıktı üretirdi:

```sql
-- pseudo SQL

INSERT INTO SCHOOL VALUES 1, 2, 3, 4, 5 -- same insert statement (this is only 1 insert statement)
INSERT INTO SCHOOL VALUES 6, 7, 8, 9, 10
```

Hibernate'te "batch" sayısını arttırdığımızda, "hibernate.order_inserts" vs "hibernate.order_update" confg'lerine de dikkatli kullanmak gerekiyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# BASE
ACID'e alternatif bir karakteristik listesidir. Genelde NoSQL sistemlerde bu karakteristikler gözlemlenir. 3 maddenin kısaltmasıdır:

- __Basically Available__

  BASE'deki B ve A buradan geliyor.

  sistem sürekli cevap verir (tutarsız data dönse bile).

- __Soft state__

  Sistemdeki veriler yeni input olmasa bile, değişebilir (çünkü diğer veritabanları birbirlerine arkaplanda sync oluyor).

- __Eventual consistency__

  sistem bir süre sonra tutarlı (ilgili query için aynı bilgiyi) bilgi döndürecek hale gelecektir.

BASE kullanan bir sisteme örnek verirsek: arama motorları. Elasticsearch gibi NoSQL'e kayıt atar. Atılan kayıt hemen okunamaz. İndexlenmesi uzun sürer. Burada dikkat edilirse BASE bir sistem vardır.

# Atomicity vs Transactional
Bu terim biribiri yerine sıkça kullanılır. Fakat tamamen farklı kavramlardır. Atomicity, Transaction'ın sadece 1 karakteristiğidir. Oysa bir transaction'ın 4 farklı karakteristiği var. Bunlar ACID'dir.

İlk olarak bir akademik makalede A, C, D ortaya atıldı. Bu makalede bunların bününün bir "transaction" olduğu belirtildiği. Daha sonra farklı bir makalede Isolasion karakteristiği eklendi. Dolayısı ile "transaction" terimi ile "ACID transaction" terimi tümüyle aynı anlama gelmektedir.

# ACID
veritabanı transaction karakteristikleridir (Transaction Properties):

- __Atomicity__: Ya hep ya hiç demektir. Bir transaction içinde bütün işlemler yapılır, ya da biri dahi yapılamıyorsa hiçbiri yapılmaz. rollback özelliğidir. Eğer transaction içinde bir hata olursa sistem en başa dönebilmelidir (yaptıklarını geri alabilmelidir).

- __Consistency (or tutarlılık)__: Yapılan işlemler sonucunda oluşan çıktıların tutarlılığı anlamına gelir. yani; primary-key, foreign-key, not-null gibi tablo kurallarının bozulmadığı garanti edilir.

- __Isolation (or izolasyon or yalıtım)__: Transaction gerçekleşirken dışarıdan müdahaleye izin verilmemelidir. izolasyon seviyelerine göre değişiklik gösteren bir özelliktir. örneğin; gerekiyorsa/isteniyorsa kullanılan tablolar or satırlar dışarıdan erişim için bekletilmelidir (lock'lanmalıdır). yada gerekiyorsa/isteniyorsa; dışarıdakiler transaction'ın yazdıklarını okuyabilir, fakat değişiklik yapamaz. izolasyon seviyeleri başka başlıkta detaylı anlatılıyor.

- __Durability (or dayanıklılık)__: işlem client tarafa tamamlandı denildiğinde (commit edildiği onayı geldiğinde), verinin sorunsuzca kaydedildiğini garanti etme özelliğidir. herhangi bir elektrik kesintisinde dahi sistemin kayıt işlemini garanti etmesi gerekir.

# Isolation'ın olmaması yada olması durumunda çıkabilecek problemler
Aşağıdaki her madde "ANSI/ISO SQL" standartlarında "Read phenomena" başlığı altında tanımlanmıştır.

- __Lost Updates__: Aynı anda birden fazla transaction aynı anda bir veri üzerinde güncelleme yapmak istediği zaman en son işlem yapan transaction'ın kaydı geçerli olur. Bu yüzden veri kaybı yaşanır.

- __Dirty Reads__: Bir transaction ın bir veri üzerinde yapmış fakat commit etmemiş olduğu bilgileri diğer transaction tarafından gerçek kayıtmış gibi okuması durumudur. Buna "__read uncommitted__" veya "__uncommitted dependency__" de denir.

- __Non-Repeatable Reads__: Bir transaction bir veriyi okusun ve kendi içinde işleme soksun. Bu transaction commit etmeden başka bir transaction bu veriyi okusun. Daha sonra işlem yapan transaction tekrar veriyi okumak istediğinde veri, aynı veri olmayacağı için sorun yaşanmaktadır.

- __Phantom (hayalet) Reads__: Aynı anda çalışan transaction düşünelim. Bir transaction bir tablodaki verilerin tamamını çeksin ve kendi içindeki işleme soksun.Bu sırada da diğer transaction aynı tabloya veri ekleme işlemi yapsın.Daha sonra ilk transaction tekrar verileri çekmek istediğinde yeni eklenen veriler hayalet veri olarak görünür.

# Standart karmaşası
İsolasyon seviyeleri ve "Read phenomena" kavramları, "ANSI ve ISO SQL" de belirlenmiş olsa da bunlar çok net ve her durumu kapsayacak şekilde tanımlanmamıştır. Burada bununla ilgili bir makale yayınlamıştır: https://www.microsoft.com/en-us/research/publication/a-critique-of-ansi-sql-isolation-levels/, Makale buraya yönlenmektedir: https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/tr-95-51.pdf

Bu projede: https://github.com/ept/hermitage bu durum açıklanmış ve ek olarak veritabanları arasındaki farklar özel bir tabloda toplanmıştır.

https://github.com/ept/hermitage projesinde 'Read phenomena'ların sayısı, 'ANSI ve ISO SQL'de yetersiz olduğu için arttırılmış. Bazı read phenomena'lar:
- G0: Write Cycles (dirty writes)
- G1a: Aborted Reads (dirty reads, cascaded aborts)
- G1b: Intermediate Reads (dirty reads)
- P4: Lost Update

Buna rağmen projede şu uyarı düşülmüş:

```
The summary table above only describes safety properties, i.e. whether the database allows a certain race condition to occur. It doesn't describe how the anomaly is prevented (usually by blocking or aborting some of the transactions). In practice, how much transactions need to be blocked or aborted makes a big performance difference. For example, although PostgreSQL's serializable and MySQL's serializable have the same isolation guarantees, they have totally different implementations and very different performance characteristics.
```

Fakat tablodaki MySQL ve PostgreSQL için ayrılan "serializable" isolasyon satırındaki tüm bilgiler aynı. Buna rağmen farklılıklar göstermektedirler.

# isolation seviyeleri (or isolation levels)
"ANSI ve ISO SQL" standartlarının belirlediği 4 çeşit izolasyon tipi mevcuttur. Bunlara ek olarak her database farklı seviyeler sunabilir. Örneğin; Microsoft SQL Server 'ANSI ve ISO SQL'e ek olarak "SNAPSHOT" isimli bir isolasyon seviyesi daha sunmaktadır.

Burada sade bir anlatım mevcut: (source-id: 111)

JPA'da aşağıdaki gibi her transaction işlemi için izolasyon seviyesi verebiliriz. Eğer hiç izolasyon seviyesi kullanmazsak, JPA default olarak database'in izolasyon seviyesini kullanmaktadır.

```java
@Transactional(isolation = Isolation.REPEATABLE_READ)
```

# İsolasyon seviyesi hangi transaction'ı etkiler
X ve Y transaction'ları aynı anda çalışıyor olsun. X'e bir isolasyon seviyesi belirleyecek olalım. Burada, Y'nin isolasyon seviyesinin de önemi vardır. Fakat; bazı isolasyon seviyeleri sadece diğer transaction'lardan data okuyup okuyamayacağına karar verme kuralları içerirken, bazı isolasyon seviyeleri, kendisinden data okunup okunmayacağı hakkında da kurallar içerir.

Örneğin; level 0, diğer transaction'lardan uncommited olan dataları bile okur. Fakat kendinin uncommited verilerinin okunup okunmayacağı için bir kural içermez. Oysa level 2'de hem kendisinin uncommited'larının okunup okunamayacağı hemde başkasından okuyup okuyamacağı hakkında kural içerir.

# read lock vs write lock
İzolasyon seviyelerini okumadan önce bu 2 terimi bilmek gerekli:

- # read lock (or shared lock)
  eğer bir satır (yada tablo) "read lock" olarak flag'lendiyse, o satır okunabilir fakat yazılamaz(update edilemez).

- # write lock (or exclusive lock)
  eğer bir satır (yada tablo) "write lock" olarak flag'lendiyse, o satır okunamaz veya yazılamaz(update edilemez).

# isolation levels

- # Read Uncommitted (level 0)

  Başka transaction'lardaki "modified but not committed" olan data'ları okur. kaynak: (source-id: 112) "Arguments" başlığı, "READ UNCOMMITTED" altbaşlığı, ilk cümle.

  - engelleyemediği sorunlar: Dirty + Non-Repeatable + Phantom

  - Kullanım durumları: anlık raporlar çıkaran sistemlerde tercih edilir. Örneğin; DB'yi redis gibi hızlı erişmek amaçlı kullanıyorsak ve oradan rapor gibi data çekiyor isek bu seviyeyi tercih edebiliriz. Örneğin bir oyundaki oyuncu sayısını çeken serviste bunu kullanabilir. Oyuncu sayısı çok yüksek ise 1 eksik 1 fazla çok önemi olmayabilir.

- # Read committed (level 1)

  Çoğu RDBMS için default seviyedir.

  Diğer transaction'ların ellediği data'lardan bilgi okurken read-lock ve write-lock'u önemser.

  bu seviyede eğer write-lock'lu bi data okuyacak ise, lock'ın kaldırılmasını bekler. böylece dirty read yaşanmaz.

  işlem yaparken kullandığı satırları;

  - eğer okuyorsa, read-locked olarak işaretler
  - eğer güncelliyorsa, write-lock olarak işaretler

  Eğer, read yaptığı SQL parçacığından sonra başka SQL parçacığı ile okuma yapmak istediğinde (when it moves off the current row), o zaman eski okuduğu satırlardaki "read lock" flag'i kaldırılır. Fakat bu durum güncellediği satırlar için geçerli değildir. Güncellenen satırlardaki "write lock"'lar transaction sonlanana kadar kalırlar.

  Yukarıdaki paragraftaki "SQL parçacığı"; "each statement" olarak düşünülmelidir. kaynak: (source-id: 112) "Arguments" başlığı, "REPEATABLE READ" altbaşlığı, ikinci paragraf.

  - engellediği sorunlar: Dirty
  - engelleyemediği sorunlar: Non-Repeatable + Phantom

- # Repeatable Read (level 2)

  "Read committed (level 1)" ile aynıdır. Tek fark: Level 1, read yaptığı tüm bölgeleri transaction sonuna kadar "read locked" olarak işaretler.

  - engellediği sorunlar: Dirty + Non-Repeatable
  - engelleyemediği sorunlar: Phantom

- # Serializable (level 3)

  Repeatable Read (level 2) ile aynıdır. Tek fark şudur:

  - level 2; "shared lock"'ı belli satırlar için kullanırken,
  - level 3; "shared lock"'ı belli bir range için uygular. kaynak: (source-id: 112) "Arguments" başlığı, "SERIALIZABLE" altbaşlığı, 2'inci paragraf ilk cümle.

  Shared lock'ta yeni ilgili data'lar (range içerisine dahil olan data'lar) diğer transaction'lar tarafından modify edilemese de, ilgili range arasına yeni satır eklenebilir. Oysa "range lock" ta bu durumda engellenmektedir. kaynak: (source-id: 112) "Arguments" başlığı, "SERIALIZABLE" altbaşlığı, 3üncü madde ve 2inci paragraf 2inci madde.

  Yukarıdaki "range" kavramını farklı bir dilde örneklersek:

  ```sql
  SELECT * FROM Order
  ```

  Eğer range lock'lamıyorsak, yukarıdaki SQL sadece o anda Order tablosunda olan satırları lock'lar (örnek 500 satır). Oysa range lock'landığı zaman yukarıdaki SQL tüm tabloyu lock'layacaktır. Yani yeni kayıtlar artık engellenecektir.

  ```sql
  SELECT * FROM Order WHERE status = "DISABLED"
  ```

  Range lock var ise DISABLED olan yeni order'lar, Order tablosuna eklenemeyecek. Oysa range lock yok ise, SQL'i çalıştırığımız andaki DISABLED olan satırlar lock'lanacaktır.

  - engellediği sorunlar: tümü
  - engelleyemediği sorunlar: yok

# Snapshot isolation
Bu ANSI/ISO SQL'de olmayan bir tanımdır. Burada transaction başladığı andaki dataları sürekli okur. Yani database'nin o anda bir snapshot'u alınmış gibi düşünebiliriz. Snapshot isolasyonu kullanan transaction, paralel transaction'lar ne yaparsa yapsın hala, snapshot'taki datayı okumaya devam eder. En sonda datayı yazmaya kalktığında, eğer okduğu veya değiştireceği datalarda hiçbir değişiklik yok ise success olarak tamamlanır. Kendisi hiçbir satırı lock'lamaz.

X, Snapshot seviyesinde bir transaction olsun. X, Lock kullanmadığından dolayı, modifiye ettiği satırlar, başka transaction'lar tarafından modifiye edilmiş olabilir. Bu sebeple X muhtemelen hata alacaktır. Bunu aşmak için diğer transaction'ların UPDATE işlemini beklemeye aldırtmak için opsiyonel ek bir ayar sunulmaktadır. 

Bazı DB'ler "serializable" isolasyon seviyesini "snapshot" seviyesi olarak kullanır. Bazı makalelerde ise "Serializable Snapshot Isolation" olarak adlandırılır. Bu sebeple piyasada terminoloji karmaşası yaşanmaktadır.

# Lock of Tables
Tüm isolasyon seviyelerine bakıldığında tabloların tümüyle hiç lock'lanmadığını görebiliriz. Fakat buna rağmen, update veya delete işlemlerinde tabloların bazen lock'lanabilir. Bunun 2 sebebi var:

1- Database sunucuları kendi içlerinde dataları basit yöntemlerle ve tek 1 dosyada tutmazlar. Biz ufak bir datayı update ettiğimizde, bazen internal olarak bu fiziksel dosyaları farklı yerlere taşıyabilir, parçalayabilir, birleştirebilir veya içerisinde büyük bir işleme tabi tutabilir. Bunu kendisi optimizasyon için veya farklı sebeplerden yapıyor olabilir. Dolayısı ile bu durumda değişiklik yaptığımız tablo lock'lanıyor olabilir. Bizde bunu isolasyon seviyesinden kaynaklı sanıyor olabiliriz.

2-Genelde range-lock'lar her durumda developer tarafından yanlış algılanabiliyor. Örneğin şu denemeyi Microsoft SQL Server'da kendim birçok kere denemiştim ve bu şekilde sonuç verdiğini görmüştüm:

```sql
DELETE FROM USER_TABLE
WHERE ID BETWEEN 1000 AND 9999
```

Yukarıdaki transaction çalışırken aşağıdaki transaction cevap vermiyordu (yukarıdakinin bitmesini bekliyordu):

```sql
SELECT * FROM USER
```

Fakat aşağıdaki paralel çalıştığında cevap veriyordu:

```sql
SELECT * FROM USER
WHERE ID BETWEEN 1 AND 9
```

Nedenini muhtemelen cevap vermeyen SQL'in tüm tablo üzerinden işlem yapmaya çalışmasıdır. Cevap veren SQL ise ID ile spesifik gittiği için range lock'a takılmıyor. Detaylı düşünüldüğünde okuyan ve yazanın ne işlemler yaptığı önemli.

# Manuel lock table
Tüm tabloyu veya bir view'ı manuel lock'layabiliriz:

```sql
LOCK TABLES table_name READ
```

Yukarıdaki SQL'de;
- READ diğer thread'lerin tablodan okuyabilmesini fakat yazamamalarını sağlar.
- READ yerine WRITE kullanırsak; ilgili tablo okumaya dahi kapatılır. Diğer okuyacak thread'ler kuyrukta bekletilir (exception almazlar).

# locking mechanism

# locking types

Aşağıdaki tüm lock çeşitleri sadece JPA/veritabanı konusunda kullanılmazlar. Genel bilgisayar bilimleri konularıdır.

- # optimistic vs pessimistic
  bu lock çeşitleri, genel bilgisayar biliminde de kullanılan terimlerdir. Optimistic lock'ta lock'lanmış bir satırı, herkes çekebilir fakat herhangi bir thread bunu update etmeye kalktığında, eğer başkası önceden update etmiş ise, hata alır. Yani mutlaka; ancak ve ancak en güncel hali update edilebilir ve ilk update eden kazanır mantığı vardır. Oysa pessimistic lock'ta ilgili satır direk olarak lock'lanır ve kimse hiçbir şekilde o satırın lock'ı kaldırılana kadar ilgili satırı update edemez. Yani ancak ve ancak tek bir thread (lock'layan thread) ilgili satırı update edebilir.

- # optimistic locking
  Entity'mizde (tablomuzda) @Version tag'ini almış bir sütun olmalıdır. JPA bizim için bu sütunu otomatik yönetir. Her sütun değiştiğinde bu sürüm arttırılır. DB'deki ilgili satırımızın sürümü 5 olsun. Daha sonra bir transaction gelip bu satırı update etmeye kalktığında, eğer transaction verisi sürüm 4 ise OptimisticLockException alacaktır. Ancak 5'incisi sürüme sahip bir transaction o satırı update edebilir.

  Optimistic locking için version'lama yerine farklı yöntemlerde kullanılabilir. örnek versiyon yerine, timestamp te atanabilir. aynı timestamp değilse bizim versiyon değildir kontrolü kurulabilir.

  Optimistic locking tamamen yazılımsal katmandaki logic ile yapılan bir özelliktir. DB'nin desteklemesi gerekmez. dolayısı ile aşağıdaki 2 lock mode'unda da veritabanı seviyesinde lock mekanizması kullanılmaz.

  JPA, Optimistic lock için 2 tür mode sunar:

  - __OPTIMISTIC__ veya __READ__ (enum'lar aynı yere referans eder)

    veri update edildiğinde versiyon numarası yükseltilir.

  - __OPTIMISTIC_FORCE_INCREMENT__ veya __WRITE__ (enum'lar aynı yere referans eder)

    OPTIMISTIC enum'u ile aynı mantıkta çalışıyor, fakat ek olarak; veri okunduğunda dahi versiyon numarasını arttırıyor. Bu mode; bir bilgiyi okuduğumuzda onu serbest bırakana kadar başkaları tarafından güncellenememesini sağlar. çok önemli bilgiler gösterildiğinde bu kullanılabilir.

- # pessimistic locking
  pessimistic kelime anlamı: kötümser

  database'in lock mekanizmasını desteklemesi gerekir. JPA bir satır okunduğunda, o satırı database tarafında lock konumuna geçirir. lock'lanan satır diğer uygulamalar tarafından okunamaz.

  Bunu JPA SQL ile şu şekilde implemente eder:

  ```sql
  SELECT * FROM USERS 
  FOR UPDATE -- This is a pecial keyword on SQL. It locks the rows which used in this query.
  ```

  JPA'daki PESSIMISTIC lock çeşitleri:

  - __PESSIMISTIC_READ__

    lock'lanan entity diğer transaction'lar tarafından okunabilir fakat değiştirilemez.

  - __PESSIMISTIC_WRITE__

    lock'lanan entity diğer transaction'lar tarafından okunamaz.

  - __PESSIMISTIC_FORCE_INCREMENT__

    lock'ladığı her objenin versiyonunu (version sutununu) direk arttırır (update işlemi yapmasak bile).

- # semantic lock
  semantic kelime anlamı: anlamsal

  DB'deki bir kaydı lock'lama yöntemleri kullanmak yerine, o kaydın statüsünü bir sutunda tutup, diğer uygulamaların bu statü sutununa bakarak (anlamlandırıp), ilgili kaydı güncellemesini bilmesi gerektiği akışlara denir. yani; tamamen uygulama seviyesinde olan; özel bir lock mekanizması olmayan, uygulamamızın business logic katmanında birbirlerinin kayıt atmaması gerektiğini ayarlamaktadır.

  örneğin siparişimiz pending statüsündeyse, gün sonu işleminin bu satırı hiçbir şekilde update etmemesi buna örnek verilebilir. Çünkü gün sonu işlemleri ancak ve ancak tamamlanmış siparişler üzerinde çalıştırılabilir.

# usage in JPA
JPA her transaction için hangi lock mode'unu kullanabileceğimizi belirlememize izin veriyor. kullanım örnekleri:

Not: aşağıdaki örneklerde LockModeType herhangi bir değer alabilir: LockModeType.PESSIMISTIC_WRITE ...

```java
// entityManager Find
entityManager.find(Student.class, studentId, LockModeType.OPTIMISTIC);

// Query
Query query = entityManager.createQuery("from Student where id = :id");
query.setParameter("id", studentId);
query.setLockMode(LockModeType.OPTIMISTIC_INCREMENT);
query.getResultList();

// explicit Locking
Student student = entityManager.find(Student.class, id);
entityManager.lock(student, LockModeType.OPTIMISTIC);

// refresh (updates the existing entity)
Student student = entityManager.find(Student.class, id);
entityManager.refresh(student, LockModeType.READ);

// NamedQuery
@NamedQuery(name="optimisticLock",
  query="SELECT s FROM Student s WHERE s.id LIKE :id",
  lockMode = WRITE)
```

Also the scope of the locked space can be defined. Example: If the entity has other relations to other entities (using @JoinTable, @OneToOne, @OneToMany...), we can also lock those entities...

```java
Map<String, Object> properties = new HashMap<>();
map.put("javax.persistence.lock.scope", PessimisticLockScope.EXTENDED);

entityManager.find(
  Student.class, 1L, LockModeType.PESSIMISTIC_WRITE, properties);
```

__PessimisticLockScope.EXTENDED__ ilgili entity ve onun tüm relation'larını lock'larken, __PessimisticLockScope.NORMAL__ sadece ilgili entity'yi lock'lar.

Opsiyonel olarak lock'ın olacağı timeout süresini de verebiliriz:

```java
Map<String, Object> properties = new HashMap<>();
map.put("javax.persistence.lock.timeout", 1000L);
```
