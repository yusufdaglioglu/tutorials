############################

############################
# ELASTICSEARCH KIBANA LOGSTASH
############################

############################

# ELK (or Elastic Stack)

Elasticsearch + Logstash + Kibana üçlüsü çoğu zaman birlikte kullanılıyor. bu sebeple bu 3'lü pakete verilen kısaltma.

Logstash'e her uygulama log'u yollar, Logstash tüm veriyi Elasticsearch'e aktarır. Kibana ise Elasticsearch'ten okuduğu data'ları son kullanıcıya web arayüzünden gösterir.

Bu mimaride Logstash çok yorulur. Çünkü herkes sadece Logstash'e data (log) yollar. Burada yükü dağıtmak için "__Beats__" isimli uygulama sıkça kullanılır. Tüm projemizdeki her uygulama için ayrı ayrı Beats ayağa kaldırabilir yada n-adet uygulama için sadece 1 adet Beats kaldırabiliriz. Beats sadece aracılık yapar. Logstash'e bilgileri yollarken, backpressure yeteneği de olduğundan yoğunluk sırasında sistemin dengesini korur. Beats aynı zamanda isterse direk olarak Elasticsearch'e de aldığı bilgileri atabilir. Hatta bazı ek modüller sayesinde basit filtreleme işlemleri de yapabilir. (tüm paragraftaki bilgiler için kaynak: (source-id: 401) )

Beats'ler tipine göre farklı executable dosyaları indirme gerektiriyor. aşağıdaki tüm örnek Beats uygulamaları, o anda çalıştıkları OS'tan toplantıkları data'ları Logstash veya ElasticSearch'e yollarlar.

- __Filebeat__; istediğimiz dosyalardan okuma yapar
- __Winlogbeat__; Microsoft Windows log sisteminden log'ları takip eder
- __Auditbeat__; "Linux Audit Framework" ten topladığı audit bilgilerini toplar (Linux Audit Framework Linux'ta son kullancının yaptıı işlemleri tutmaktadır)
- __Metricbeat__; desteklediği birçok yazılımdan (MongoDB server, Redis, Zookeper...) metric toplarlar.

Filebeat için örnek config file:

```yml
filebeat.prospectors:
- input_type: log
  paths:
    - /var/log/httpd/access.log # Filebeat'in sürekli okuyacağı dosya

document_type: Apache-access # okunacak dosyanın dosya formatı

fields_under_root: true

output.Logstash:
  hosts: ["127.0.0.1:5044"] # filebeat hangi IP'deki Logstash'e data'ları (log'ları) göndereceği
```

Logstash'in sırası ile 3 farklı fazı vardır:
- input: input'tan bilgilerin okunması
- filter: bilgilerin manipüle edilmesi
- output: bilgilerin farklı bir yere yollanması

Logstash için örnek config file:

```groovy
input { // hangi faz için tanımlama yapacağımızı tepede belirtiyoruz
  beats { // Beats'ten okuyacağımızı belirtmemiz gerekli.
    port => 5044 // input'un nereden okunacağı
  }
}

// yukarıda "Beats" ('ten okumak yerine) yerine şunları kullanabilirdik:

// - file (Logstash ile aynı makinede bulunan bir dosyadan okumamızı sağlardı)
// - Redis (Redis'ten okumamızı sağlardı)

// input örnekleri için kaynak: (source-id: 402)

filter { // hangi faz için tanımlama yapacağımızı tepede belirtiyoruz

  grok { // Logstash ile gömülü gelen bir filter plugin'i
    match => { "message" => "%{IPORHOST:clientip} %{USER:ident} %{USER:auth} \[%{HTTPDATE:time}\] "(?:%{WORD:verb} %{NOTSPACE:request}(?: HTTP/%{NUMBER:httpversion})?|%{DATA:rawrequest})" %{NUMBER:response} (?:%{NUMBER:bytes}|-)" }
  }

  date { // Logstash ile gömülü gelen bir filter plugin'i
    match => [ "timestamp" , "dd/MMM/yyyy:HH:mm:ss Z" ]
  }
}

// yukarıda Grok eklentisi sayesinde, Logstash'e gelen anlamsız log satırı anlamlı hale dönüştürülüyor. log formatımızı Grok'a parametre olarak veriyoruz. date eklentisine geldiğimizde ise artık tarih değerimizi log satırından parse etmemize gerek yok. tarihi "timestamp" olarak yeni bir JSON field'ı olarak kaydediyoruz. artık ElasticSearch'e baktığımızda "timestamp" field'ı istediğimiz formatta görünecektir. ek not: "@timestamp" field'ı Logstash tarafından otomatik atılmaktadır. "@timestamp" Logstash'e ne zaman log gelirse o anın tarihi olarak set edilmektedir.

output { // hangi faz için tanımlama yapacağımızı tepede belirtiyoruz
  ElasticSearch { // bilgileri hangi sunucuya yollayacağımızı belirtmemiz lazım.
     hosts => ["localhost:9200"] // Elasticsearch'in IP'si ve portu
  }
}

// yukarıda "ElasticSearch" ('e bilgileri atmak yerine) aşağıdakileri kullanabilirdik:

// - file (Logstash ile aynı makinede bulunan bir dosyaya yazmamızı sağlar)
// - Graphite (Graphite isimli programa bilgileri yollayabilmemizi sağlar)
// - statsd (aynı makinede bulunan statsd isimli servise data yollayabilmemizi sağlar)

// output örnekleri için kaynak: (source-id: 402)
```

Logstash filter'ları için farklı bir örnek: (source-id: 404)

# Syslog
Birçok implementasyonu olan fakat RFC3164 ve daha sonrasında RFC5424 ile standartlaştırılmış log formatı ve protokolüdür. Syslog standartlaştığından ötürü özel isimdir.

(source-id: 405)

(source-id: 406)

Syslog servisleri, 514 portundan dinleme yapar. platformdaki tüm uygulamalar buraya log'larını yollar. Logstash'te input plugin'i olarak Syslog yazılırsa, Logstash 514 portundan dinleme yapmaya başlar. aynı bir Syslog daemon'u gibi log'ları dinler ve bunları Elasticsearch'e yollar.

# OpenSearch
Elasticsearch fork'udur. Elasticsearch'e göre daha community-driven yönetilmektedir ve lisansı daha serbesttir.

"Open Distro" projesi Elasticsearch daha özgür forkudur. "Open Distro" projesi, OpenSearch çıkınca durmuştur.

OpenSearch ekibi Logstash ve Beats gibi projeleri forklamadı. Çünkü zaten onlarla uyumlu çalışmaktadır.

# OpenSearch Dashboards
Kibana fork'udur. Kibana'ya göre daha community-driven yönetilmektedir ve lisansı daha serbesttir.

# Kibana
ElasticSearch'teki data'ları son kullanıcıya göstermek için web arayüzü sunan bir yazılım.

Logstash kendi internal'da kullandığı config'lerini:
- index patterns
- saved searches
- saved visualizations
- ve dahası...

ElasticSearch'te farklı bir index'e (RDBMS'teki tabloya denk gelen yapıya) kaydeder.

Kibana web-UI'ındaki sekmeler:

- discover: arama yapılmasını sağlıyor.
  discover sekmesindeki alt widget'lar:
  - bir widget'ta "Selected Fields" ve "Available Fields". bunlar arama sonuçları ekranında gösterilebilecek sütunları belirtmektedir. örnek: "Available Fields"'de log'larda akan tüm alanlar mevcuttur. örnek: appName, IP, timestamp, message. "Selected Fields"'a "Available Fields"'lardan seçim yaparak alan ekleriz. böylece sadece "Selected Fields"'daki alanlarımız sonuç ekranında görünmüş olur. eğer "Selected Fields" ı boş bırakırsak sonuç ekranında "\_source" sütunu olur ve bu sütunda tüm log bilgileri(alanları) bir JSON şeklinde gösterilir.
  - bir widget'ta standart olan "__Apache Lucene Query Syntax__" veya "__Kibana Query Language (or KQL)__" ile arama yapabilmemizi sağlayan textbox mevcuttur. Lucene arama örneği: "status:200 AND appName:user-service". ("Lucene Query Syntax" başka başlıkta detaylı anlatılıyor)
  - bir widget'ta filter'lar mevcut. bunlar Lucene stili aramasına ek filtreler eklememizi sağlıyor. Lucene ile de yapılabilecek bir şey fakat filtreler kolay enable disable edilebiliyor. oysa Lucene'de bir filtreyi yorum satırı yapmazsınız. silip tekrar yazmamız gerekir.

  Ekranda yaptığımız her ayar URL'ye yansır. URL'yi başka tarayıcıya yapıştırdığımızda bu arama aynen açılır.

- dev tools: JSON query'leri ile arama yapılmasını sağlıyor. örnek:

```
  GET _search
  {
    "query": {
      "match_all": {}
    }
  }
```

- visualize: grafikler hazırlamamızı sağlıyor. buradan grafik oluşturuyoruz. fakat bu grafikleri direk web-ui'ı açan user'a gösteremiyoruz. bu grafikleri dashboard'a bağlamak zorundayız.

- dashboard: bir filtre yazıyoruz ve visualize'da hazırladığımız her görsel'i buradaki filtreye bağlıyoruz. örneğin filtremiz 'status:200' olsun. altında da buna bağlı 3 tane grafik bağlamış oluruz. bu dashboard'a "başarılı istekler" adını verelim. birçok dashboard'umuz olabilir. artık dashboard sekmesine girip, listeden "başarılı istekler"'i seçen user, karşısında 3 tane grafik görecektir.

# Kibana index pattern
index Elasticsearch'te RDBMS'teki tabloya denk gelen bir terimdir.

index patterns tell Kibana which ElasticSearch indices you want to explore. An index pattern can match the name of a single index, or include a wildcard (*) to match multiple indices.

For example, Logstash typically creates a series of indices in the format Logstash-YYYY.MMM.DD.

# Kibana Query Language (KQL)
Lucene syntax'ına benzemektedir. Lucene syntax'ının sunmadığı ek özellikler sunmaktadır.

# ElasticSearch Query DSL
API üzerinden sunulan query sorgulama JSON syntax'ıdır. Tüm ElasticSearch API'si "Query DSL" değildir. "Query DSL" sadece "query" field'ı içindeki syntax'a verilen isimdir.

```
GET /_search
```

```json

{
    "query": {
        "match" : {
            "message" : {
                "query" : "this is a test"
            }
        }
    }
}
```

# Graylog
Graylog; Logstash ve Kibana'ya alternatif, ELK'ya göre daha community based bir projedir.

Graylog genelde ElasticSearch ve Mongo ile kullanılıyor. MongoDB'de genel graylog'un config'leri saklanırken, ElasticSearch'te log'ların kendileri saklanıyor.

Graylog sunucusuna direk mesajları atabiliriz. Fakat Beats veya Logstash ile de ek modüllerle supportu mevcut.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Solr vs ElasticSearch
Ikiside açık kaynaklı birbirine alternatif arama index'leyici ve query yazılımlardır.

Solr'da field isimleri ve field type'lar baştan belirtilmek zorundadır. Yeni field eklemeye kalktığınızda bütün kayıtları tekrar Solr'a aktarmanız gerekiyor. Elasticsearch'te böyle bir durum yok. Çünkü Elasticsearch, scheme barındırmıyor. NoSQL-database gibi.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ElasticSearch
"elastic", yazılımı geliştiren firmanın adı. firmanın eski ismi ElasticSearch'tü.

ElasticSearch document based (Mongo database gibi) data storage'tır. JSON formatında saklar. fakat dökümanın her parçasını index'ler.

bu index'leme tekniği birçok arama motoru kullanır ve buna "inverted Index" denir. örneğin;

3 adet kaydımız olsun:

```
döküman numarası - içeriğindeki kelimeler
1- a b c
2- x y z
3- a x
```

Elasticsearch şu şekilde index tutar:

```
a -> 1, 3
b -> 1
c -> 1
x -> 2, 3
y -> 2
z -> 2
```

'x' terimini arattığımızda direk 2 ve 3üncü document'lerde olduğunu bulmamızı sağlar.

"inverted index"'in tersi "Forward Index"'tir. yani her döküman sadece tokenizing edilir. index'lenmiş olur. performansa tek katkısı zaten tokenizing edilmiş olmasıdır. parse edilmesine gerek yoktur.

"inverted Index" kabaca yukarıdaki gibi bir matris tutar. spesifik implementasyonları bakarsak farklı bilgiler de içerebilir. örnekler:
- her kelimenin dökümanda kaç kere yer aldığı bilgisi
- kelimenin dökümandaki pozisyonu

ElasticSearch dışarıya REST API sunar, bu dilden bağımsız sorgu atabilmemizi sağlar.

### shard
kelime anlamı: parça

Elasticsearch'in tuttuğu her index tek bir makinede tutulmayabilir. onu partition'lara ayırabiliriz. ayırdığımız her partition'a shard denir.

1 node'a, 'shard' dememek gerekli. çünkü 1 node, 1'den fazla shard içerebilir ve aynı zamanda replica görevi de görebilir. kaynak: (source-id: 59) answer of 'prayagupd' at Nov 7 '13 at 15:29.

### replica
her shard'ın tutulacağı replika node'lara verilen isimdir.

### index
relational-database'deki tablo'ya denk gelen kavramdır. en üst seviye bilgi grubudur. her index'e bir alias atılmak zorunda değildir. fakat atılması mutlaka önerilir. bir sistemde min 1 index olmalıdır. kaynak: (source-id: 60)

### Indices
'index' teriminin çoğul hali olduğundan, relational-database'lerdeki veritabanına denk geliyor diyebiliriz. çünkü; birden fazla index yani tablo bir database'i tanımlar.

### Mapping
relational-database'deki şemaya denk gelir. istenirse tipler önceden mapping ile belirtilebiliyor.

fakat ElasticSearch yeni bir kayıt eklendiğinde verinin yapısını zaten tespit ediyor. kaynak: (source-id: 61) "Elastik" başlığı 1inci paragraf.

### Document
relational-database'deki tablo içindeki kayda (row) denk gelir.

### Type
Document'in tipini belirtir. user, tweet tipinde farklı document'ler olabilir. relational-database'deki tablo'ya denk gelir.

### field
relational-database'deki her tablo içinde olan bir sütuna denk gelir. Elasticsearch, NoSQL olduğu için JSON tutar ve her field bir JSON property'dir.

### kayıt eklemek

ElasticSearch önceden şema istemediğinden attığımız yeni kaydı direk kabul eder ve bundan arama yapılabilmesini kendisi sağlar. yeni kaydımızı Rest API'si ile ekleyelim:

> PUT ELASTIC_SEARCH_URL/customer/_doc/1

```json
{
  "name": "John Doe"
}
```

dönüşü:

```json
{
  "_index" : "customer",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "result" : "created",
  "_shards" : {
    "total" : 2,
    "successful" : 2,
    "failed" : 0
  },
  "_seq_no" : 26,
  "_primary_term" : 4
}
```

şimdi kaydımızı çekelim:

> GET /customer/_doc/1

dönüşü:

```json
{
  "_index" : "customer",
  "_type" : "_doc",
  "_id" : "1",
  "_version" : 1,
  "_seq_no" : 26,
  "_primary_term" : 4,
  "found" : true,
  "_source" : {
    "name": "John Doe"
  }
}
```

Birçok döküman index'lenecek ise, tek tek yerine performans açısından "_bulk" API'si tercih edilmelidir.

Birçok kayıt index'lemiş olalım. Şimdi bu kayıtlarımızı arayalım:

> GET /bank/_search

```json
{
  "query": { "match_all": {} },
  "sort": [
    { "account_number": "asc" }
  ]
}
```

dönüşü:

```json
{
  "took" : 63, //how long it took ElasticSearch to run the query, in milliseconds
  "timed_out" : false, // request timeout or not.
  "_shards" : { // how many shards were searched and a breakdown of how many shards succeeded, failed, or were skipped
    "total" : 5,
    "successful" : 5,
    "skipped" : 0,
    "failed" : 0
  },
  "hits" : {
    "total" : {
        "value": 1000,
        "relation": "eq"
    },
    "max_score" : null,
    "hits" : [ {
      "_index" : "bank",
      "_type" : "_doc",
      "_id" : "0",
      "sort": [0],
      "_score" : null,
      "_source" : {"account_number":0,"balance":16623,"firstname":"Bradshaw","lastname":"Mckenzie","age":29,"gender":"F","address":"244 Columbus Place","employer":"Euron","email":"bradshawmckenzie@euron.com","city":"Hobucken","state":"CO"}
    }, {
      "_index" : "bank",
      "_type" : "_doc",
      "_id" : "1",
      "sort": [1],
      "_score" : null,
      "_source" : {"account_number":1,"balance":39225,"firstname":"Amber","lastname":"Duke","age":32,"gender":"M","address":"880 Holmes Lane","employer":"Pyrami","email":"amberduke@pyrami.com","city":"Brogan","state":"IL"}
    },
    //...
    ]
  }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Nested objects
ElasticSearch stores:
- the inner(nested) JSON objects as different(seperate) documents. The data type of these documents are "nested" type. The standard document type is "object".

```json
PUT my-index-000001
{
  "mappings": {
    "properties": {
      "user": {
        "type": "nested"
      }
    }
  }
}

PUT my-index-000001/_doc/1
{
  "group" : "fans",
  "user" : [
    {
      "first" : "John",
      "last" :  "Smith"
    },
    {
      "first" : "Alice",
      "last" :  "White"
    }
  ]
}

GET my-index-000001/_search
{
  "query": {
    "nested": {
      "path": "user",
      "query": {
        "bool": {
          "must": [
            { "match": { "user.first": "Alice" }},
            { "match": { "user.last":  "Smith" }}
          ]
        }
      }
    }
  }
}
```

- the arrays as a flattened format.

```json
PUT my-index-000001/_doc/1
{
  "group" : "fans",
  "user" : [
    {
      "first" : "John",
      "last" :  "Smith"
    },
    {
      "first" : "Alice",
      "last" :  "White"
    }
  ]
}
```

It stores internally as:

```json
{
  "group" :        "fans",
  "user.first" : [ "alice", "john" ],
  "user.last" :  [ "smith", "white" ]
}
```

We can query using:

```json
GET my-index-000001/_search
{
  "query": {
    "bool": {
      "must": [
        { "match": { "user.first": "Alice" }},
        { "match": { "user.last":  "Smith" }}
      ]
    }
  }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# ElasticSearch vs Cassandra vs MongoDB
Elasticsearch bir search ve analytics engine'i iken, diğerleri NoSQL olarak geçmektedir. 3 alternatif yazılım data'ları NoSQL olarak hard-drive'a kaydedip, bunları client'a sunsalar da, tutuş biçimleri performans önceliklerine göre değişmektedir.

MongoDB generic-purpose bir NoSQL database olarak kullanılır. istediğimiz sütunları index'leriz. Cassandra da ise, sorgularımız baştan belli olmalıdır ve daha incremental data'ların kaydı için kullanılır. ElasticSearch ise tüm sütunları (field'ları) ve field'ların içindeki data'ları sürekli index'ler. bu şekilde tüm data'larda hızlı arama yapılabilmesini sağlar. fakat bu durum insert işlemlerini yavaşlatır, çünkü her insert işleminde index'leme çalışması şarttır (ve tüm field'lar index'lenmek zorundadır).

ElasticSearch "document store" olarak kayıt tutmaktadır.

ElasticSearch ek olarak "__full-text search__" destekleyene bir storage'dir. "full-text search" teriminin kapsamı keskin çizgilerle çizilmemiştir. SQL'deki "like" keyword'ü de text search özelliğidir, fakat "full-text search" birçok kompleks özelliği kapsar. bunlardan bazıları:
- __stop word__'lerin otomatik çıkarılması ("and", "I" gibi kelimeler...)
- kelime köklerinin algılanmasını sağlamak ("git" arandığında "gidiyorum"'un sonuçlarda görünmesi)
- petabyte'lar üzerinden arama yapabilmek
- birden fazla sütunu birleştirerek arama yapabilmek (ve hatta bu sütunların bazıları farklı tipte olabilirken (örnek: char, varchar...))
- tüm bu özelliklerin opsiyonel olabilmesi