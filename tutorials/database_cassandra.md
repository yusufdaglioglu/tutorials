############################

############################
# DATABASE CASSANDRA
############################

############################

# Cassandra
Cassandra yazılımı, Apache foundation tarafından ücretsiz ve açık kaynaklı olarak sunulmaktadır. __DataStax__ firması Apache'nin repo'sunu base alarak; "__DataStax Enterprise (or DSE)__" isminde ürün sunmaktadır. __DataStax__ hem ücretli hem ücretsiz versiyon içermektedir. __DataStax__ sadece ek özellikler sunar ve teknik support verir. bu linkte ikisinin farkları açıklanmaktadır: (source-id: 123)

Cassandra Java'da yazıldığı için runtime'da JVM'e ihtiyaç duyar.

- cluster içindeki node'lar arasında master-slave ilişkisi yoktur. onun yerine __ring design__ söz konusudur. Bu durum "No Single Point of Failure" olmasını sağlamaktadır. her node isteklere cevap verebilecek tasarımdadır.
- Cassandra cluster yapısı içerisinde "ring design" ile haberleştiği için "cluster" terimi yerine "ring" terimi de kullanılmaktadır.
- multi data center supportu default olarak vardır. yani; WAN üzerinden dahi, aynı cluster'a bağlı Cassandra node'ları birbiri arasında paylaşım yapabilir. bunun için ek bir servise ihtiyaç yoktur. bu özellik Cassandra sunucularında default gelir.
- client cluster içerisindeki herhangi bir node'a istek yapabilir. Client'ın attığı isteği ilk alan node'a __coordinator (or koordinatör)__ ismi verilir. eğer bu istek query isteği ise; return data'sı koordinatörde yok ise; koordinatör arkaplanda bu bilgiyi diğer node'lara sorarak dönüş yapar. eğer istek kaydetme isteği ise; coordinatör fiziksel olarak o data'yı kaydedecek olan node'lara isteği yollar.
- __Ayarlanabilir Veri Tutarlılığı Seviyesi (or Consistency Levels)__: Veri tutarlılığı seviyesi, okuma isteğinin kaç sunucudan veri alarak cevaplanacağını ve yazma isteğinin kaç sunucuya veri yazıldıktan sonra istemciye "tamamlandı" olarak dönüleceğini belirler. Örneğin kopya sayısı 10 olan bir kümeye gelen yazma isteğinin tutarlılık seviyesi 3 ise, gelen veri 3 sunucuya yazıldıktan sonra (kalan 7 sunucuya kopyalama devam ederken)  istemciye "tamamlandı" olarak dönülecektir. Cassandra veritabanında veri tutarlılığı seviyesi küme ve sorgu bazında ayarlanabilir.
- eğer client data manipülasyonu yapacak istek yollarsa sırası ile şunlar işletilir:
  - Cassandra node'u gelen isteği fiziksel bellekte olan "commit log"'a kaydeder
  - client'a "committed" mesajını döner
  - data'yı __Memtable__ denilen hızlı hafızadaki bölgeye taşır
  - data'yı olması gereken yere taşır. yani sorting'e göre olması gerektiği yere taşır. En son taşınan bölgeye __SSTable__ adı verilir.
- Okuma işlemlerinde ise önce node'daki Memtable'a bakılır, eğer yok ise SSTable'a bakılır. kaynak: (source-id: 124) 1inci paragraf.

# insert into vs update
"insert into" işlemi if not exist özelliği içermiyor ve varsa aynı ID'li data'yı override ediyor. Bu durumda "insert into" ile "update" aynı görevi görüyor. fakat ikisi arasında ufak farklılıklar var. buradaki gibi: (source-id: 125) answer of billbaird at "May 16 '14 at 23:04". Stackoverflow'da billbaird'in bahsettiği konunun bug olmadığını gösteren kaynak: (source-id: 126)

Cassandra "lock" mekanizmasını desteklemez. (oysa bu durum distributed sistemlerde bu önemli bir gereksinimdir.)

Client Cassandra'ya bir kayıt isteği attığında Cassandra hızlı dönüş yapabilmek için önce commit log'a yazar ve client'a dönüş yapar (bu konu başka başlıkta detaylı anlatılıyor). işte performance avantajı burada. commit log'daki data daha sonra gerekli yere fiziksel olarak taşınır. Burada ince bir nokta var: Commit log'a sadece kayıt atılabiliyor. İşte bu sebeple Cassandra'da update ve insert işlemi aynı şeye denk geliyor. Aynı sebepten, Cassandra "lock" mekanizması da destekleyemiyor. Çünkü commit log'a herhangi bir client bir bilgi atabilir. commit log hiçbir kontrol içermiyor. Bu sebeple çok hızlı. Eğer kontroller gelirse performance kaybı olur. Bu durumda farklı DB server'ları kullanmamız daha avantajlı olur. Cassandra zaten generic purpose bir DB değil. Bazı durumlarda kullanmak gerekiyor.

Cassandra read-before-write'tan kaçınır. kaynak: (source-id: 127) ilk 2 paragraf.

Cassandra ek olarak istisna durumlar için "if not exist" gibi özellikleri "lightweight transactions (or LWT)" isimli bir özellik ile destekliyor fakat bu pek önerilmiyor. kaynak: (source-id: 127) 2inci paragraf.

# SSTables
Cassandra'nın yazma hızının yüksek olmasının sebeplerden biridir. SSTables'lar birer fiziksel dosyadır. her dosya immutable'dır. yani; dosyanın içeriğini update edilmez. Memtable direk olarak SSTable olarak fiziksel olarak saklanır. (source-id: 129) kaynak: title: "Storing data on disk in SSTables", 1st paragraph.

Bu sebeple Cassandra'da silme veya update işlemi gerçekten fiziksel kaydı silmez/değiştirmez. Silme veya update işlemi de yeni bir row'dur(kayıttır). update edilen kayıt (row) okunacağı zaman her zaman en sondaki kayıt okunur.

SSTables'ların sayısı arttıkça, Cassandra arkaplanda işe yaramayan satırları (kayıtları) ignore eder/siler ve hala kullanılması gereken kayıtları yeni SSTable'lara merge eder. Bu işleme "compaction" denir.

Silme işleminde, silmek için atılan yeni kayıt'a tombstone (mezar taşı) adı verilir. Tombstone ile işaretlenen kayıtlar compaction ile sinir ve beraberinde tombstone'larda silinir.

# Cassandra vs MongoDB
Burada birçok detay farklılık çıkabilir. Fakat tüm sebepler aşağıdaki temel farklılıkların sonucunda ortaya çıkmaktadır:

- # cluster yapısı
MongoDB'de auto-election özelliği var. Tek bir MongoDB instance'ı isteklere cevap döner (master node). Diğer node'lar (slave node'lar) arkaplanda güncel data'lara sync olur. Eğer master node çökerse, auto-election devreye girer ve diğer node'lerden biri master olur. Burada ortalama 12 saniye zaman kaybı yaşanacaktır fakat isteğe bağlı olarak bu arada read-query'lerine cevap verilebilir. kaynak: (source-id: 130). Bazı kaynaklarda bu sürenin ortalama 1 dk olduğu yazar. kaynak: (source-id: 131) "Data Availability" başlığı. Oysa Cassandra'da master-slave ilişkisi yoktur. her node eşit miktarda data'yı fiziksel olarak barındırır (tabi bazı data'lar yine bu node'lar arasında yedekli şekilde barındırılır). gelen isteğe cevap verecek data yok ise; diğer node'a istek yönlendirilir. Eğer isteği alan node çökerse, diğer node'lar isteğe cevap zaten veriyor durumdadır. Auto-election gibi bir sürece ihtiyaç yok.

MongoDB'de slave node'lar (eğer istenirse/ayarlanırsa) read isteklerine cevap verebildiği için read'ler açısından Cassandra ve MongoDB aynı performansı sergiliyor diyebiliriz. Fakat iş yazmaya geldiğinde Cassandra çok daha üstün performance sergiliyor. Çünkü Mongo'da sadece master node yazma işlerini hallederken, Cassandra'da tüm node'lar bu görevi aynı anda üstleniyor.

- # join queries
Cassandra'nın yapısı gereği SQL dilindeki join gibi özellikler çalışmamaktadır. Aynı zamanda "where" ile sorgu her sütun için yapılamamaktadır. Bu sebeple sorgularda kısıtlamalar yaşanmaktadır. Benzer sebeplerden her sütuna index'te atılamamaktadır. (Aslında bu paragrafta belirtilenler Cassandra ile de yapılabilir, fakat eğer yapılırsa Cassandra kullanmanın hiçbir manası kalmaz.). Oysa Mongo-database zaten nested dökümanlarla çalıştığı için onlar içerisinde gelişmiş sorgular yapabilmemizi destekliyor.

- # scheme
Cassandra'da tablo yapısı (şeması) önceden belli/sabit iken, Mongo database'de böyle bir zorunluluk yoktur. Mongo-database'de şama opsiyonel'dir.

Hatta MongoDB'de bir field farklı kayıtlarda farklı tipte (int, string...) olabilir. kaynak: (source-id: 132) title: "Flexible Schema", 1inci madde.

- # nested objects
Mongo-database'de, içiçe (nested) JSON objeler tutulabilir. Oysa Cassandra'da böyle bir imkan yoktur. Cassandra'da içiçe bilgi yerine, aynı tabloda farklı sütunlara yazmak gerekiyor. Örneğin:

Mongo database:

```
order : {
  order_id: "99",
  order_date : "01 01 2002"
  user : {
    user_id: "123",
    user_name: "Ahmet"
    user_surname: "AnySurName"
  }
}
```

Cassandra database:

```
order : {
  order_id: "99",
  order_date : "01 01 2002"
  user_id: "123",
  user_name: "Ahmet"
  user_surname: "AnySurName"
}
```

Cassandra'da daha sonradan "collection" yapıları geldi.

collection'lar bir field'ın içinde map, list tutabilmemizi sağlıyor. fakat gerçek bir sütun gibi çalışılmasını sağlamıyor. limitasyonlar var. kaynak: (source-id: 133) Frozen olan collection'larda, frozen olan tüm collection update edilirken, tüm bilgiler güncelleniyor. frozen data'lar binary data olarak saklanıyor. Frozen olan collection'larda limitasyonlar çok daha fazla.

MongoDB terminolojisinde tek döküman içinde olan diğer (nested / içiçe) dökümanlara __embedded document__ adı verilir. ve Embedded dökümanlar native olarak Mongo-database tarafından desteklenir. Embedded kullanmak işlemleri hızlandırır fakat data dublication'ı arttırır. Onun yerine reference (Mongo database terminolojisinde DEBRef) kullanılabilir fakat o zaman query'ler yavaşlar fakat dublication azalır. Aynı durum Cassandra için de vardır. Fakat Cassandra nested veya referansları native olarak desteklemez. Onun yerine Cassandra'yı kullanan yazılımın bunu manuel yapması gerekir.

# Cassandra veri yapısı
- __Keyspace__, relational-database'deki şemaya veya veritabanına denk gelmektedir. en üst seviyeli database kavramıdır.
- __column family__, relational-database'deki tabloya denk gelmektedir. Java'da şu objede tutulur:

  ```java
  Map<RowKey, SortedMap<ColumnKey, ColumnValue>>
  ```

  ```java
  Map<PartitionKey, SortedMap<ColumnKey, ColumnValue>> // (daha ileride ne oldukları anlatılıyor) kaynak: (source-id: 134) "Column family" başlığı.
  ```

  Her satır RowKey ile temsil edilir. Her satır'da birden fazla column-key ve her column-key'e denk gelen value vardır.

  Aslında bakıldığında relational-database'de de aynı şekilde data'yı temsil edebiliriz: her satırın mutlaka bir ID'si olsun. her satırda, bir sütuna karşılık gelen bir değer vardır. (bu konu başka başlıkta anlatılıyor: "column family")

- __colum key (or column name)__ relational-database'deki sütun ismine denk gelmektedir. her column key'e karşılık gelen bir de __column value__ vardır.

- __Cassandra Query Language (or CQL)__ dili ile query atılır. SQL'e çok benzerdir. __Cassandra query language shell (or cqlsh)__ komut satırı uygulaması ile bu query'ler atılabilir. CQL'de hala 'table' gibi terimler kullanılıyor. cqlsh, shell/bat script'idir fakat kendi içerisinde Python'u çağırır.

# Keyspace
Keyspace yaratan bir metot yazalım:

```java
public void createKeyspace(String keyspaceName) {

  String queryStr = "CREATE KEYSPACE IF NOT EXISTS " + keyspaceName +
                    "WITH replication = {'class':'SimpleStrategy','replication_factor':'2'};";

  session.execute(queryStr);
}
```

Dikkat edilirse her keyspace için ayrı ayrı replica sayısını verebiliyoruz. Eğer replica vermezsek; 1 satır 1 node'da yer alır. Fakat o tablonun her satırı aynı node'da olmak zorunda değildir. Cassandra node'ları her satırı kendi arasında paylaşmaktadır. Eğer tablonun replica sayısını 2 yaparsak, her satır mutlaka 2 node'da olacaktır.

Yukarıdakinden daha farklı stratejiler mevcut. Örneğin; "NetworkTopologyStrategy". (başka başlıkta anlatılıyor)

# Check all keyspace via CQL

```java
ResultSet result = session.execute("SELECT * FROM system_schema.keyspaces;");

List<String> allKeyspaceList = result.all();
```

# database design patterns (which are mostly used on Cassandra)

- # one table per query pattern (or one-table-per-query pattern) vs query-first approach (or query-driven modelling)
Aralarında farklılıklar var. Cassandra kullanırken çoğu zaman "one table per query pattern" yaparız fakat bu en optimum çözümü bulduğumuz anlamına gelmez. en optimum çözüm 'query first approach'tur.

table per query pattern'i açıklamak gerekirse: selectByTitle sorgumuz için özel bir tablomuz olsun: "booksByTitle". Durum böyle olunca duplicate data'larımız tüm veritabanında çoğalıyor. fakat bu durum Cassandra'da okumada performans artışı sağlanıyor. Artık her yeni kayıt işlemlerimizde bir 'book' ve 'booksByTitle'a ekleme yapmamız gerekecek.

query first approach; ise farklıdır. buradaki fark; query first approach'ta bazen çok benzer query'leri aynı tabloya atabilmemizdir. Sadece where koşulu değişebilir... çünkü  bazı durumlarda önceliği düşük sorgular için tekrarlı data olmamasını hıza tercih edebiliriz. Böyle durumlarda primary key birden fazla sütuna olur.

Ek not: Cassandra'da tablo isimlendirmeleri şu şekilde olmalı: user_by_name, users_by_id, reservations_by_user_id..

- # time series pattern
activity log olarak düşünebileceğimiz tablolardır. "money_transaction" tablosu buna en temel örneklerden biridir. blockchain mantığı gibi silinen data olmaması gerekir.

- # wide partition pattern (or wide row pattern)
ilişkili data'ların aynı partition'da tutulup, bu şekilde tek bir sorgu ile hızlı çağrılması pattern'idir.

# Chebotko notation
Artem Chebotko'nun geliştirdiği Cassandra için tablo gösteriminde kullanılan notasyondur. temelde şu şekildedir:

```
Q1 (Find User by name)
        ↓
*************************           *************************
*     users_by_name     *           * users_by_school_name  *
*************************           *************************
* user_name          K  *           * school_name        K  *
* user_last_name     C↑ *           * school_city        C↑ *
* user_id            C↓ *           * school_level          *
* user_address          *  --> Q2   *************************
* school_name           *     (Find
*************************      school by name)
```

Q1 ve Q2 application flow'una göre sırası ile yapılacak isteklerdir.

Yukarıdaki tablo notasyonuna göre join'leme kısmı application client tarafında yapılması beklenir.

K primary key'i temsil eder.

C↑ clustering key'i temsil eder. ASC sorting yapar.

# partitioning key vs clustering key

Simple bir tablo yaratmaya bakalım:

```sql
create table simple_table (
    TC_NO int PRIMARY KEY, -- partitioning key: TC_NO
    TELEFON int
);
```

Şimdi ise __composite key (or compound key or bileşik anahtar)__ olan tablo yaratalım:

```sql
create table table1 (
      TC_NO int,
      TELEFON int,
      DOGUM_YILI int,
      PRIMARY KEY(TC_NO, TELEFON) -- partitioning key: TC_NO, Clustering Key: TELEFON
);
```

"PRIMARY KEY" parantezinin içindeki ilk kısım __partitioning key__'dir. Where koşulları ile data çekerken bu key şarttır. Çünkü Cassandra node'ları artık partitioning key'e göre dışarıdan aramaya hizmet vermektedir.

Örneğin; TC hash'leri 0-999 arasındaki satırlar node1'de, 1000-1999 arası satırlar node2'de bulunuyor. Her node toplamda eşit sayıda satırları barındırıyor. Bu şekilde denge sağlanmış oluyor. Cassandra bir satıra ait tüm primary key'lerin birlikte hash'ini alır ve buna göre node'lara dağıtır. Hash çıktısı standart olduğundan (belli boyutta uzunluğu, çıktıdaki olabilecek karakterler kümesi/çeşitliliği...), her node'a dağıtma işlemini kümeleyip/mapping yapabilir. (Cassandra hash algoritması olarak non-cryptographic hash metotları kullanılır) kaynak: (source-id: 134) "Partition key" başlığı 3üncü paragraf. + kaynak: (source-id: 136) "Node, Start range, End range" içeren tablo.

NoSQL'ler bilgiyi binlerce node'a dağıtır. Bu durumda bir sorgu gerektiğinde tüm bilginin binlerce node'da aranması gerekir. Bunu hızlandırmanın tek yolu şöyle bir sorgu atabilmektir:

```sql
Select * from USERS where user_name = 'Ahmet' and node_id = new_york_9
```

Yukarıdaki sorguda data'nın new_york'taki 9'uncu instance'ta (node'da) olduğunu bilerek sorgu atmış olduk. Oysa bunu bilmemiz imkansız. İşte NoSQL sistemler tablodaki bir veya birden fazla sütunu bizden tabloyu oluştururken özellikle istiyor. bu özel sütunlardaki her satırın hash'i alınıyor. her hash kümesi fiziksel olarak 1 instance/node'a kaydediliyor. yukarıdaki TC örneğindeki gibi. Dolayısı ile artık bilgiye erişmek istediğimizde ilgili node'un nerede olduğunu ilgili sütunun hash'inden anında bulabiliyoruz. Dolayısı ile milyonlarca node bile olsa anında sonucu bulabiliyoruz. Temel konseptte birçok NoSQL bilgiyi bu şekilde distributed olarak paylaştırıyor ve bu mekanizmaya __Distributed Hash Table (or DHT)__ deniliyor.

Yukarıdaki tablo oluşturma SQL'inde "Primary key" 2 kısımdan oluşur. ilk kısım "__partition key__", ikinci kısım "__clustering key__" olarak değerlendirilir. "partition key" birden fazla olduğunda parantez içerisine alınmalıdır. clustering key'lere göre sorting yapabiliriz anlamına gelir. her satır clustering key'lere göre fiziksel olarak sorting yapılarak tutulur. Örneğin TC'si 0-999 arasındaki satırlar node1'de olsun. node1 içerisinde fiziksel olarak TELEFON numarasına göre sorting yapılmış şekilde tüm satırlar tutulur.

"__partition key__" and "__shard key__" and "__sharding key__" and "__distribution key__" terms are equivalent. (source-id: 470) 1st subtitle.

Diğer örnekler:

```sql
create table table2 (
      TC_NO int,
      TELEFON int,
      DOGUM_YILI int,
      OLUM_YILI int,
      PRIMARY KEY((TC_NO, TELEFON), DOGUM_YILI) -- partitioning key: TC_NO ve TELEFON, Clustering Key: DOGUM_YILI
);
```

```sql
create table table3 (
      TC_NO int,
      TELEFON int,
      DOGUM_YILI int,
      OLUM_YILI int,
      PRIMARY KEY(TC_NO, TELEFON, DOGUM_YILI) -- partitioning key: TC_NO, Clustering Key: TELEFON ve DOGUM_YILI
);
```

Where koşullarında partition key'deki tüm elemanları girmek zorunludur. Yani table2'de TC_NO ve TELEFON (ikisi ile birlikte) ile where sorgusu yapmak zorundayız. table2 için örnek vermeye devam edersek; Where koşuluna, ek olarak; DOGUM_YILI da kullanılabilir. fakat zorunlu değildir. DOGUM_YILI sort'landığı için hızlı dönüş sağlayacaktır. kaynak: (source-id: 134) "A note about querying clustered composite keys" altında "valid queries" örnekleri.

Partitioning konusu tüm NoSQL'ler için geçerli olduğu için çok önemli. Data'yı farklı node'lara bölmek aşağıdaki gibi avantajlar sağlayabilir:
- sadece belli partition'daki data'nın kolayca replike edilmesi (diğer node'lardaki partition'lara erişim durduğu zaman da devam edebilmesi)
- sadece belli partition'daki data'lara sorgu atılması (performans avantajı)

# Consistent hashing
NoSQL'lerde kullanılan bir tekniktir.

Klasik HashMap'lerde store edilecek data hash'lenir ve module işlemine tabi tutulur. Bu işlemin sonucunda data'nın nereye store edileceği bulunmuş olur. Yani; module işleminin sonucunda, node'un index'ini bulmuş oluruz.

```
node-index = mod( hash(key-of-data) , total-index-count)
```

Örnek:

4 node'umuz var. TC: 1111119999 olan satırın hangi node'da olduğunu belirleyelim:

```
43 = hash(1111119999)
3 = mod(43 , 4)
```

Fakat bu yöntem ile bir node düştüğünde veya yeni bir node eklendiğinde, o node'a ait data'ların tümünü taşımak zorunda kalacağız. Bu durum istenen bir durum değildir. Bu data taşıma işleminin bir şekilde en aza indirilmesi gerekmektedir. Bu sebeple yukarıdaki yönteme alternatif olarak "Consistent hashing" tekniği geliştirilmiştir.

Consistent hashing tekniğinde, her node belli bir aralıktaki data'ları alır. Tabi data'ların hash'li halinden bahsettiğimiz için düzenli bir çıktı kümesi söz konusudur. Person-National-Identifier'ı hash'lediğimizi varsayalım. Person-National-Identifier'ı hash'lenmiş hallerinin, 1000-3000 arası olanlar N1 nodunda, 3000-5000 arası olanlar N2 nodunda olcak gibi... Yani:

- 1000-3000 arası olanlar N1 nodunda
- 3000-5000 arası olanlar N2 nodunda

Eğer araya yeni bir node eklenirse, sadece yakın değerlerdeki bir kısım data, yeni node'a eklenecek ve data farklı 2 node'a bölünüştürülecek. Örneğin son durum şöyle olabilir:
- 1000-2000 arası olanlar N1 nodunda
- 2000-4000 arası olanlar N3 nodunda --> Bu node'a 1000 satır N1'den, 1000 satır'da N2'den taşınır. Yani iş bölüştürülmüştür.
- 4000-5000 arası olanlar N2 nodunda

"Consistent hashing" genel bir terimdir. Bu teniğin birçok implementasyonu vardır. Yukarıda anlatılan "ring-based hashing" dir ve detaylı şekilde anlatılmamıştır.

# blob
Cassandra'nın byte array data tipidir. Java'da bu data mapping yapıldığında ByteBuffer olarak tutulmalıdır. kaynak: (source-id: 138)

# Cassandra write consistency levels

Aşağıdaki tüm bilgiler için kaynak: (source-id: 139) Aynı linkte 'read consistency'ler yazmaktadır.

Güncel Cassandra'da, consistency level CQL komutunda verilemiyor. CQL'deki "WITH CONSISTENCY" kullanımdan kaldırıldı. consistency level programatik olarak driver seviyesinde, yada sunucu tarafta cluster veya data-center seviyesinde yada per-operation basis (for example insert or update) seviyesinde ayarlanabiliyor.

| Level        | Description                                                                                                                                                                                                                                                                                                        | Usage                                                                                                                                                                                                                                                                                                                                          |
|--------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ALL          | A write must be written to the commit log and memtable on all replica nodes in the cluster for that partition key.                                                                                                                                                                                                 | Provides the highest consistency and the lowest availability of any other level.                                                                                                                                                                                                                                                               |
| EACH_QUORUM  | Strong consistency. A write must be written to the commit log and memtable on a quorum of replica nodes in all data center.                                                                                                                                                                                        | Used in multiple data center clusters to strictly maintain consistency at the same level in each data center. For example, choose this level if you want a read to fail when a data center is down and the QUORUM cannot be reached on that data center.                                                                                       |
| QUORUM       | A write must be written to the commit log and memtable on a quorum of replica nodes.                                                                                                                                                                                                                               | Provides strong consistency if you can tolerate some level of failure.                                                                                                                                                                                                                                                                         |
| LOCAL_QUORUM | Strong consistency. A write must be written to the commit log and memtable on a quorum of replica nodes in the same data center as the coordinator node. Avoids latency of inter-data center communication.                                                                                                         | Used in multiple data center clusters with a rack-aware replica placement strategy, such as NetworkTopologyStrategy, and a properly configured snitch. Use to maintain consistency locally (within the single data center). Can be used with SimpleStrategy.                                                                                   |
| ONE          | A write must be written to the commit log and memtable of at least one replica node.                                                                                                                                                                                                                               | Satisfies the needs of most users because consistency requirements are not stringent.                                                                                                                                                                                                                                                          |
| TWO          | A write must be written to the commit log and memtable of at least two replica nodes.                                                                                                                                                                                                                              | Similar to ONE.                                                                                                                                                                                                                                                                                                                                |
| THREE        | A write must be written to the commit log and memtable of at least three replica nodes.                                                                                                                                                                                                                            | Similar to TWO.                                                                                                                                                                                                                                                                                                                                |
| LOCAL_ONE    | A write must be sent to, and successfully acknowledged by, at least one replica node in the local data center.                                                                                                                                                                                                     | In a multiple data center clusters, a consistency level of ONE is often desirable, but cross-DC traffic is not. LOCAL_ONE accomplishes this. For security and quality reasons, you can use this consistency level in an offline datacenter to prevent automatic connection to online nodes in other data centers if an offline node goes down. |
| ANY          | A write must be written to at least one node. If all replica nodes for the given partition key are down, the write can still succeed after a hinted handoff has been written. If all replica nodes are down at write time, an ANY write is not readable until the replica nodes for that partition have recovered. | Provides low latency and a guarantee that a write never fails. Delivers the lowest consistency and highest availability.                                                                                                                                                                                                                       |
| SERIAL       | Achieves linearizable consistency for lightweight transactions by preventing unconditional updates.                                                                                                                                                                                                                | You cannot configure this level as a normal consistency level, configured at the driver level using the consistency level field. You configure this level using the serial consistency field as part of the native protocol operation. See failure scenarios.                                                                                  |
| LOCAL_SERIAL | Same as SERIAL but confined to the data center. A write must be written conditionally to the commit log and memtable on a quorum of replica nodes in the same data center.                                                                                                                                         | Same as SERIAL. Used for disaster recovery. See failure scenarios.                                                                                                                                                                                                                                                                             |

## quorum
kelime anlamı: yeterli çoğunluk

Bu değer şu şekilde hesaplanıyor: (sum_of_replication_factors / 2) + 1
