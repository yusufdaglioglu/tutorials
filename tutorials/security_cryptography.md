############################

############################
# SECURITY CRYPTO
############################

############################

# terminology
Şifrelemede kullanılan özel terimleri, gruplar halinde burada bulabiliriz: (source-id: 466)

# Simetrik Şifreleme Algoritması (or gizli anahtarlı şifreleme or secret key encryption or symmetric-key algorithm)
bazı yerlerde eş anlamlı olarak bu terimlerde kullanılıyor: __single-key yada shared-key yada one-key__

Veriyi şifrelemek ve şifreli veriyi çözmek için her iki tarafında bildiği ortak-tek bir anahtar kullanılır.

# Asimetrik şifreleme Algoritması (or asymmetric algorithm or açık anahtarlı şifreleme algoritması)
public-private anahtar yapısına dayanır. public key ile şifrelenmiş bir data'yı, ancak o public key ile birlikte oluşturulan private key ile düzgün şekilde açabiliriz.

Aynı durum tersi için (private key ile şifrelenip), public key ile bu şifre açılabilir mi bilmiyorum. Bu konuda yazılar var, fakat yazılar spesifik şifreleme algoritmalarına göre yazıldığı için, tüm asimetriklerde bu kural geçerli mi kesin bilmiyorum. Üstelik RSA (asimetrik şifreleme) bunu desteklese bile, padding, ECB gibi extension'lar bu kuralı bozuyor olabilir. Bu konuyu bilmiyorum.

# anahtar (or key) ve şifrelerin çözülme hızı
- public ve private key'ler birer şifredir. dosya olmalarının sebebi büyük boyutta olmalarıdır. örnek 512 karakterli şifre.

- anahtar ile şifrelenmiş bir verinin çözülmesi uzun sürmektedir. örnek: ortalama değerler üzerinden konuşursak; 2017'de ortalama bir bilgisayar, 128'lik bir SHA ile şifrelenmiş bir veriyi 2 trilyon yılda çözebiliyor.

- Aynı anahtar uzunluğuna sahip simetrik algoritma, asimetriklere göre daha uzun sürede çözülebiliyor. örnek karşılaştırma:
  - 1024-bit RSA keys are equivalent in strength to 80-bit symmetric keys
  - 2048-bit RSA keys to 112-bit symmetric keys
  - 3072-bit RSA keys to 128-bit symmetric keys
  - 15360-bit RSA keys to 256-bit symmetric keys

  Yukarıdaki değerler için kaynak: (source-id: 294) "Series/Number: NIST Special Publication 800-57 Part 1 Revision 4", Title: "Recommendation for Key Management, Part 1: General".

# passphrase
pass (geçiş) ve phrase (ifade) kelimelerinin birleşimidir. parola ile aynı anlamda kullanılır. fakat passphrase daha çok son boşluk içeren kelimelerin birleşiminden olabileceğini ifade etmek için kullanılır. örnek passphrase: "Hello I am 1st developer$$"

# SSH-keygen komutunda istenen passphrase
SSH-keygen gibi komut satırı uygulamaları ile private-public key oluşturulmaktadır. bu private key'ler oluşturulduğunda bizden passphrase istenebilir. bu opsiyoneldir. eğer passphrase istenirse oluşturduğumuz key, bu passphrase ile şifrelenir. aynı şekilde public key'de buna istinaden oluşturulur. dolayısı ile private key dosyamıza ulaşan hacker, şifreli dosyamıza ulaşmış olur.

# elektronik imza (or electronic signature)
genel bir terimdir. "dijital imza", "parmak izi" gibi birçok çeşit imza çeşidini kapsar.

# sayısal imza (or digital signature)
bu kavram bir şifreleme algoritması değildir. sadece data'yı şifreleyen tarafın, kendi imzasında bilgiye eklemesi ve bu şekilde açan tarafın bu mesajın sadece o alıcıdan geldiğine emin olmasını sağlamasıdır + verinin bütünlüğünün (değiştirilmemiş olduğunun) sağlanmış olmasıdır.

Gönderici ---> Mesajı gönderen kişi, mesajı kendi private key'i ile şifreler. Mesajın yanına public key'ini de yerleştirir. alıcı taraf public key'i kullanarak private ile şifrelenmiş mesajı çözer. bu şekilde 2 şey kanıtlanmış olur:

  - verinin nereden geldiği kesinleşmiş olur. public key ile açtığımız veri ancak private ile şifrelenmiş olabilir. dolayısı ile private'ye sahip olan kişi, bizim iletişimde olduğumuz kişidir.

  - verinin bütünlüğü korunmuş olur. çünkü asıl mesaj şifrelenmeden önce hash değeri hesaplanıp, hash değeri şifrelenmeden önce mesajın sonuna koyuluyor. asıl mesajı açan alıcı, tekrar bir hash işlemi gerçekleştiriyor. hash sonucu çıkan sonuç mesajın sonunda olan hash değeri ile aynı olup olmadığına bakıyor.

# RSA
Ron Rivest, Adi Shamir, ve Leonard Adleman tarafından geliştirilmiştir. RSA ismi bu kişilerin soyisimlerinin başharflerinden gelir.

Bir çeşit asimetrik şifreleme algoritmasıdır.

# Shamir's Secret Sharing
Adi Shamir tarafından geliştirilen yöntemdir.

Bir çeşit __Secret sharing (or secret splitting)__'dir. Bir anahtar birden fazla anahtar olarak parçalanır. Ancak bu parçalar biraraya getirilerek master key oluşturulur. Bu şekilde "secret zero" problemine (kısmen) çözüm getirilir.

Sahmir algoritmasında tüm parçaların bir araya gelmesine gerek yoktur. Minimum sayı var, o sayıda anahtarlar birleştirilmesi master key'in oluşturulması için yeterlidir.

# cipher (or cypher)
şifrelemek için kullanılan algoritmadır.

__ciphertext__ terimi "şifreli" anlamına gelmektedir. bunun tersi ise "__plaintext__"'tir ve Türkçe'de "__açık metin yada düzmetin__" olarak çevrilir.

# code
__code__ terimi; bazı cryptography makalelerde, kodlayıcının kullandığı metod olarak kullanılıyor.

__coder (or kodlayıcı)__ terimi; cypher ile çok benzerdir. fakat bir fark var. cypher; key ve data'yı input alır fakat belli bir algoritmaya göre çalışır ve output üretir. "coder" ise; sadece data'yı input olarak alır ve data'daki her birime karşılık farklı çıktılar üretir. yani bir nevi syntax değişikliği yapar.

"morse" code sistemi, code'a bir örnektir. oysa RSA, cypher'a bir örnektir.

__codebook__ terimi ise; code'un çalışması sırasında, her birimi çevirmek için her birime karşılık gelen bilgileri okuduğu veritabanıdır.

__cryptogram__ bilgisayar olmadan, elle çözülebilecek kadar basit, humanreadable characterlerdne oluşan şifrelenmiş metne denir. bu şifrelenmiş metin, bilgisayarsız ortamda da çözülebilsin diye ayarlanmıştır. eğlence amaçlı kullanılmaktadır.

# encrypting vs coding (or encoding) | decrypting vs decoding
coding ve decoding bir girdiyi farklı bir formata çevirmek anlamına gelir. çevirirken public olan bir şema/formata çevrilmesi gerekir. yani şifreli bir işlem yapılmamaktadır. eğer şifreli bir işlem olursa o zaman encrypting/decrypting olurlar.

# base64
çift yönlü bir algoritmadır. anahtarsız geri çevrilme işlemi yapılabilir. bu sebeple şifreleme algoritması değildir. çıktı kümesindeki karakterler, ASCII'nin sadece belirli elemanlarını içermektedir. çıktıda limitli bir karakter kümesi olabileceğinden terslik yapabilecek karakterler ortadan kaldırılmış olur. genelde işaret karakterleri olmadığından; dosya sistemindeki dosya isimleri, XML, JSON içerisindeki değerler gibi kullanım alanları vardır.

input olarak verilen binary data; 6 bit olarak okunur. 2^6=64 olduğundan; 64 farklı karakter çıktı kümesi vardır. çıktı kümesine "Base64 Alphabet" deniliyor. çıktılar şu şekilde:

- 000000 --> A
- 000001 --> B
- 000002 --> C
- ...
- 000025 --> Z
- 000026 --> a
- 000027 --> b
- ...
- 000051 --> z
- 000052 --> 0
- 000053 --> 1
- ...
- 000061 --> 9
- 000062 --> +
- 000063 --> /

base64'ün birçok implementasyonu var:

- RFC 3548 or RFC 4648 -> standart base64. Bu genel kabul gören ve özel bir amaç için tasarlanmayan base64.

- RFC 2045 -> MIME BASE64

- ve daha birçoğu: (source-id: 433) "Variants summary table" başlığı.

= (eşitlik) işareti bazı implementasyonlarda padding karakteri olarak kullanılmaktadır.

# Java'da base64
Java'da birçok base64 encode/decode kütüphanesi var.

- Java 6 ile

```java
javax.xml.bind.DatatypeConverter.printBase64Binary("hello".getBytes());

DatatypeConverter.parseBase64Binary("aGVsbG8gd29ybGQ=");
```

- Java 8'de

daha esnekleştirilerek birçok base64 destekleyecek şekilde yeni embedded kütüphane gelmiştir. direk örnek üzerinden incelersek:

```java
Base64.Encoder enc = java.util.Base64.getEncoder(); // standart base64

Base64.Encoder encURL = Base64.getUrlEncoder(); // URL safe base64. RFC4648 ile aynıdır. Ek olarak URL safe karakterler yapar. Bunun resmi bir RFC standardını bulamadım. Java kendi içinde böyle bir implementasyon yapmış. standart base64 ile aynı çıktıyı üretir. tek farkı şudur '+' yerine '-', '/' yerine '_' kullanır.

Base64.Encoder mimeEncoder = Base64.getMimeEncoder(); // mime base64

enc.encode(data.getBytes()); //ile encoding yapılıyor.
```

# hash algoritmaları (or özet fonksiyonları)
hash Türkçe anlamı: bir malzemenin işlemden geçirilerek yeniden sunulan malzeme anlamına gelir. örneğin; kıyılan etin tekrar sunulması gibi.

- çıktı ile, girdiyi bulmak mümkün değildir. yani algoritma tek yönlü çalışır. şifreleme algoritmalarında ise bunun tersi şeklinde; çıktıdan girdiyi (bir anahtar aracılığı ile) elde edebilirsiniz.

- tüm hash algoritmaları gibi; birden fazla girdi, aynı çıktıyı üretebilir. aynı büyüklükteki string'ler bile, aynı hash sonucunu verebilir. çünkü her 1 girdi'ye karşılık, 1 çıktı olması şart olduğundan, birden fazla girdi aynı çıktıyı vermek zorunda kalacaktır. bu duruma __collision (or çatışma or çarpışma)__ denir.

# non-cryptographic hash function
hash'leme işlemleri genelde geriye çözülememesi amacı ile yapılır. Örneğin şifreler hash'lenir. Fakat bazı hash'ler vardır ki, geriye dönülememesi amaçlanmaz. farklı amaçları vardır. örneğin hash çıktısının boyutu sabit olsun, bizde bir string'in referansını bir yerde primary key olarak kullanmak isteyelim. o zaman primary key'in size'ını hash boyutuna sabitleriz ve primary key'i hash olarak tutarız. bu tarz hash algoritmalarına "non-cryptographic hash" denir.

geriye dönülememezlik özelliğinden feragat ettiğimiz için başka özelliklerden kazanırız. örneğin; performans gibi...

örnek; Cassandra arkaplanda primary key'leri non-cryptographic hash kullanarak saklar.

"__Murmur__"; non-cryptographic hash algoritma ailesidir. birçok farklı türevi ve versiyonu vardır.

# cryptographic hash functions
- elektronik cihazlar güçlendikçe ve maliyeti düştükçe brute force atak yapmanın maliyeti ve hızı da düşmektedir.
- son kullanıcılar genelde belli kelimeleri birleştirerek şifre oluşturabilmektedirler. bu durumlara karşı sözlüklerdeki kelimeler üzerinden kombinasyonlar ile hash'leri deneyen brute force yazılımları mevcut.
- her string'in hash karşılığının tutulduğu veritabanları mevcut. bu veritabanlarından basit bir SQL sorgu ile hash karşılığı bulunabilir.

yukarıdaki sebeplerden şifreler mutlaka güçlü hash metotları ile hash'lenmelidir.

örnek: SHA256, SHA512, RipeMD, WHIRLPOOL

# Salt
kelime anlamı: tuz

- hash'leme algoritmaları ne kadar güçlü olsa da güvenliği arttırma amaçlı hash'lemeden önce her şifrenin(input'un) sonuna veya başına bir string eklenmesi önerilir. bu string'e salt denir.
- md5 ve sha1 salt olsa bile güvensiz olarak kabul edilmektedir.
- Salt'ın çok gizli tutulmasına çok takılmamak gerekli.
- Salt her user şifresi için ayrı üretilmelidir ve salt üretimi için "Cryptographically Secure Pseudo-Random Number Generator"lar kullanılmalıdır.

Farklı user'lar aynı şifreyi kullanarak sistemimize register oldu. Bu durumda ikisinin aynı hash değeri database'de kayıtlı olur bu bir risk'tir. Fakat her şifre için farklı salt kullanırsak bu durumu aşmış oluruz. Rainbow table attack'tan bizi korumaktadır.

# Pepper
kelime anlamı: biber

Salt ile tamamen aynı mantıkta kullanılır. Fakat salt genelde veritabanında hash'lenmiş çıktı ile aynı fiziksel yerde saklanırken, pepper farklı bir yerde saklanır (örneğin OS environment değerlerinde). pepper tüm şifreler için aynıdır/sabittir. pepper sadece ek koruma sağlamak için kullanılır. Hacker tüm sistemi değilde, sadece database'yi ele geçirmişse, pepper'ı bilmeyeceği için riskleri azaltmış olacağız.

# Sözlük Saldırısı (or Dictionary Attack)
brute-force attack'ın bir çeşididir. Sık kullanılan şifrelerden oluşmuş bir veri tabanındaki her şifre ile şifrelenmiş data açılmaya çalışılır.

# HashTable
Şifre ve ona karşılık hash'lerin bulunduğu bir veri tabanındaki bilgiler sayesinde hash'ten input'u elde edebiliriz. Önceden tüm hash'ler hesaplanmış olduğu için CPU'ya ihtiyacımız daha azdır.

Bu yöntemin en büyük dezavantajı, tüm şifrelerin olduğu bir DB terabayt'lar almaktadır.

# Gökkuşağı Tablosu Saldırısı (or Rainbow Table Attack)
bir çeşit özel hashtable'dır. Özel olarak üretilir ve üretildiği yöntem arama yapılmasını da kısaltır.

# şifre üretiminde kullanılması önerilen algoritmalar
şifrelerin bunlarla üretilmesi önerilir: Argon2, bcrypt, scrypt, PBKDF2

# key stretching (or anahtar germe)
anahtar üretimin zamanını arttırarak brute-force atakların önüne geçilebilir.

bunun için ideal algoritmalar: PBKDF2, bcrypt

Bu konu sanal paralardaki ASIC olayı ile benzerdir ("Equihash" başlığı). bu kaynakta: (source-id: 295) bu tarz algoritmalar "CPU-intensive hash function" olarak nitelendirilmiş. Bu URL, "Sam Newman" "Building microservices" kitabında 180inci sayfadaki "Go with the Well Known" başlığında, 3üncü paragrafta önerilmiştir.

# hash table (or hash map)
bir çeşit veri yapısıdır. bir tablo düşünelim. her satırı bir dizi'de tutmak yerine, her satırın primary değerleri ile hash'ini alıp, hash'ini index olarak kullanabiliriz.

Java'da "java.util.Hashtable" sınıfı thread-safe'dir, "java.util.HashMap" ise değildir.

hash table'larda arama işlemi çok hızlıdır. listeye bir data eklemek array'e göre daha uzundur çünkü hash alma işlemi gerektirir. işte bu durumlarda non-cryptographic hash kullanmakta yarar var. çünkü performanstan kazanç sağlayabiliriz. zira burada key gizli bir bilgi değildir.

Hash table'larda iki değerin hash'i aynı olduğunda (çarpışma or collision), hash değeri artık index/key gibi kullanılamıyor. bu noktada çözüm için birçok farklı yöntem mevcut. bazı hash-table implementasyonları hash'e denk gelen "Value" değerini linked-List yapıp, komple bu LinkedList'i hash'in value'su olarak set ediyor. okurken de bunları parse ediyor.

# hash kodları (or hash toplamları or hash sums or hash digest or hash özeti or digests or hashes or message digest)
hash fonksiyonlarından dönen değerlere (çıktı değerine) denir.

__hash fonksiyonu__ farklı uzunlukta input'lar alıp, sabit boyutta output dönen fonksiyonlardır.

# merkle tree (or Hash tree)
özel bir tree veri yapısıdır. ağacın her yaprağı altındaki ona bağlı yaprakların hash'ini tutmaktadır. en üstteki ağacın kök yaprağına da "hash sum" adı verilir.

# kontrol toplamları (or checksums)
dosyaların doğrulunu kontrol etmek istediğimizde yine "hash sums" değerine bakarız. fakat son kullanıcı açısından "hash sums" ile checksums farklı isimlendirilmiştir. oysa teknik anlamda aynı şeydir.

# Stream cipher
Block şifrelemedeki gibi blok blok değilde, stream'den gelen her bilgiyi direk şifreleyen algoritmalara denir. Stream cipher'da genelde, en ufak birim bit veya byte olarak baz alınır. Stream cyper, block şifrelemeye alternatiftir. Block şifrelemede en ufak birim block'tur.

# Blok Şifreleme (or Block Cipher)
Blok şifreleme kullanan algoritmalarda, şifrelenecek tüm mesaj 'block size' kadar bölünür. Her bölünen parça ayrı ayrı şifrelenerek, şifrelenmiş halleri birleştirilir.

Block şifrelemede, __block size__ ne kadar büyükse, şifrelemede o kadar güvenlik sağlanmış olur. Zira, blok uzunluğu büyükse, her bloğu ayrı ayrı tahminleme ile inceleyen hacker'a daha uzun bir çıktı kümesi vermiş oluruz.

Block size ile, şifrelemede kullanılacak anahtarın boyutunun bir ilişkisi olmak zorunda değildir (kişisel olarak ilişki olduğunu belirten bir kaynak okumadım).

Blok şifreleme kullanan birçok algoritma vardır. Fakat her blok şifrelemenin de birçok türevi/uzantısı vardır. Bunlar "__çalışma kipi (or mode of operation)__" olarak ta adlandırılırlar. örnek çalışma kipleri:

- # Elektronik kod defteri (or Electronic Codebook or ECB)
  
  Blok şifrelemenin en basitidir. Her blok bağımsızca şifrelenir ve sonrada yanyana dizilerek birleştirilir.

- # Şifre Blok Zincirlemesi (or Cipher Block Chaining or CBC)
  
  her blok bağımsız şifrelenir. fakat her blok şifrelenmeden önce, bir önceki blokun şifrelenmiş halini ile XOR'lanır (ve daha sonra şifrelenir).

  İlk bloğun öncesinde hiçbir blok olmadığı için, ilk blok manuel olarak verilecek "Initialization Vector" ile işleme tabi tutulur.

  CBC modunun dezavantajları (Aşağıdaki cümlelerin tersi, ECB'nin avantajlarıdır):
  
  - CPU her bloğu paralel şekilde şifreleme yapamaz. yani; şifrelerken yavaştır.

    Not: Hem CBC hemde ECB, decrypt işlemini her bloğu paralel şekilde işleterek yapabilir.

  - Şifrelenmiş mesajda herhangi bir bloğu kaybedersek, mesajın kaybolan kısmından sonrasını açamayız.

  CBC modunun avantajları (Aşağıdaki cümlelerin tersi, ECB'nin dezavantajlarıdır):
  
  - Herhangi bir blok, farklı bir bloktaki ile aynı açık metni şifreliyor olsun (böyle denk gelmiş olsun). Buna rağmen farklı şifrelenmiş çıktı verecektir. Çünkü her block bir önceki blok ile de matematiksel işleme tabi tutulduğu için aynı çıtkıyı vermez. 

- # Yayılımlı Şifre Blok Zincirlemesi (or Propagating Cipher Block Chaining or PCBC or plaintext cipher-block chaining)
  
  CBC ile aynı mantıktadır. CBC; sadece bir önceki bloğun şifreli halini kullanırken, PCBC buna ek olarak; bir önceki bloğun açık halinden (şifrelenmemiş halinden) de yararlanır.

- # Şifre Geri Beslemeli (or Cipher FeedBack or CFB)

- # Çıktı Geri Beslemeli (or Output FeedBack or OFB)

- # Sayıcı Modunda Şifreleme (or Counter Mode Encryption or CTR)
  
  Bazı kaynaklarda sadece 'CTR mode'un kısaltması olduğundan "__CM__" olarak da kullanılmaktadır. Bazı kaynaklarda ise; "__integer counter mode (or ICM) and segmented integer counter (or SIC)__" olarak kullanılır.

# Java Cipher
"algorithm/mode/padding" formatında "transformation" adı verilen string'i alır.

```java
Cipher c = javax.crypto.Cipher.getInstance("AES/CBC/PKCS5Padding");
```

Eğer getInstance'a sadece "AES" verilirse, bu durumda provider'ın sunduğu default değerler kullanılır.

# ilklendirme vektörü (or starting variable or SV or Initialization Vector or IV or İV or ilklendirme değişkeni or başlangıç vektörü or BV)
IV, "salt" ile karıştırılmamalıdır. Farklı şeylerdir fakat aynı konsepte hizmet ederler.

'Salt'ta da olduğu gibi her şifrelemede farklı kullanılmalıdır ve data açılmak istendiği zaman kullanmak üzere DB'de saklanmalıdır. 'Salt' standart algoritmaların dökümantasyonunda belirtilen bir terim değil iken, IV direk algoritmanın kendisinde olan bir terimdir.

IV yerine key kullanılamaz mıydı? Eğer key IV yerine kullanılırsa, o zaman aynı plan-text'ler aynı şifrelenmiş bilgiyi üretir. Bu da güvenlik açığı meydana getirebilir.

Eğer şifreleme işleminde IV kullanılmazsa, CBC tabanlı algoritmalarının yapısı gereği, aynı veri şifrelendiğinde, şifrelenmiş verinin sadece ilk bloğu, aynı çıktıyı verir. kaynak: (source-id: 296) "IV – the bit twister" başlığı.

örnek:

openssl komutu ile bir mesajı şifreleyelim:

```sh
$ openssl enc -aes128 -k "secretpassword" -a -p -nosalt -iv 00
key="2034F6E32958647FDFF75D265B455EBF"
Hello Mr Warrender, I have some terrible news

VVklkPrL5fczxmu4vZ93BnfBBpU8BWK1IQhHF6JRKSNZJ7PvpcaE8K/Mkbx1xgHa
```

```sh
openssl enc -aes128 -k "secretpassword" -a -p -nosalt -iv 00
key="2034F6E32958647FDFF75D265B455EBF"
Hello Mr Warrender, This is good news

VVklkPrL5fczxmu4vZ93BkAsVA64MLd5uah+zTVzr0XlOONVgDEd7ZunyIIzhpAo
```

Farklı bir IV kullanırsam, ilk bloğu farklı bir çıktı alırım:

```sh
openssl enc -aes256 -k "secretpassword" -a -p -nosalt
key="2034F6E32958647FDFF75D265B455EBF40C80E6D597092B3A802B3E5863F878C"
iv =AD0ACC568C88C116D57B273D98FB92C0
Hello Mr Warrender, This is good news

9/0FGE21YYBl8NvlCp1Ft8j1V7BiIpCIlNa/zbYwL5LWyemd/7QEu0tkVz9/f0JG
```

CBC tabanlı algoritmalarının yapısı gereği hacker'ın elinde IV yok ise, key ile sadece ilk bloğu (mesajın ilk kısmını) açamaz. Diğer blokların hepsini açabilir. kaynak: (source-id: 296) "Wait... The IV is not a key!" başlığı.

Yukarıdaki cümleyi şöyle örneklendirerek anlatalım:

Şifrelerken her bloğu sırası ile şifreliyoruz:

```java
String fullPlainText = plainText1 + plainText2 + plainText3;

// Phase 1. We are creating the first block of encrypted text in this phase.
enryptedText1 = encryptFunction( xorFunction( plainText1, IV), KEY);

// Phase 2:
enryptedText2 = encryptFunction( xorFunction( plainText2, enryptedText1), KEY);

// Phase 3:
enryptedText3 = encryptFunction( xorFunction( plainText3, enryptedText2), KEY);

result = enryptedText1 + enryptedText2 + enryptedText3;
```

Şimdi ise şifrelenmiş veriyi, "KEY" kullanarak decrypt edelim:

```java
// Aşağıdaki blokların açılması işlemleri paralel yapılabilir.
// Sırası ile olmak zorunda değil. Fakat şifrelersen sırası ile yapılmak zorunda.

// Burası okunmadan önce XOR işleminin kurallarına bakılması gerekiyor şart (Başka dökümanda bu konu anlatıldı).

tempData3 = decryptFunction(cipherText3, KEY);
plainText3 = xorFunction(tempData3, cipherText2);

tempData2 = decryptFunction(cipherText2, KEY);
plainText2 = xorFunction(tempData2, cipherText1);

tempData1 = decryptFunction(cipherText1, KEY);
plainText1 = xorFunction(tempData1, IV); // In this line we need IV. Only "key" is not enought for this block.
```

# çığ etkisi (or avalanche effect)
X string'in hash'i Y olsun. X string'inin bir karakterini değiştirdiğimizde ve tekrar hash aldığımızda Y ye benzer bir sonuç bekleriz, fakat çok farklı bir sonuç alırız. hash algoritmaları bunu bilerek yaparlar ki, değişiklikler daha belirgin olsun. buna çığ etkisi ismi verilmiştir.

# end-to-end encryption (or E2EE)
arada farklı aracı makineler olsa da şifrelenmiş data taşındığından ve sadece gönderen ve alıcının çözebildiği şifreleme sistemleridir.

# Java KeyStore (or JKS)
private ve public key'lerin, sertifikaların bir arada tutulabildiği dosyaya Java dünyasında verilen isimdir. keystore bir format değildir, bir Java sınıfıdır:

```java
KeyStore store = KeyStore.getInstance("PKCS12");
```

__keytool__ komut satırı uygulaması keystore dosyaları üzerinde değişiklik yapmamıza olanak sağlıyor. tabi tüm keystore dosya formatları bu uygulama tarafından desteklenmiyor.

JKS gibi içinde sertifika, key saklanan dosyalara yazılım dünyasında __keyring__ denir.

# TrustStore
içerisinde CA'ların sertifikaları bulunduran dosyalara Java dünyasında verilen isimdir. TrustStore bu dosyayı temsil eden sınıfın ismidir. TurstStore içerisindeki tüm sertifikalar ile kurulan bağlantılar güvenli olarak kabul edilir.

# Keychain
MacOS'larda yüklü gelen GUI desteği olan key manager uygulamasıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# padding
padding işlemi açık mesajın kendisine yapılır. padding Belli bir boyuta input bekleyen, cipher'lar içinde kullanılmaktadır. Örneğin blok şifrelemede; algoritma her bloğu eşit parçalara bölerek şifreler. bu durumda padding, input'un boyutuna göre şart olabilir. Blok şifrelemede açık metnin blok uzunluğunun katı olmak zorunda.

Her zaman padding kullanmak zorunda değiliz. Ama padding kullanmayacaksak; mutlaka gelen bilginin boyutunu bilmemiz gerekiyor. örneğin haberleşme protokollerindeki bilgilerin boyutu bellidir.

Kaç byte'ın veya bit'in padding yapıldığı bilgisi cypher tarafından dönülmez. Dolayısı ile bazı padding yöntemlerini data'mızın tipine göre seçmek gerekebilir. örneğin; binary olan data'da null karakteri olabileceğinden, aşağıdaki listede belirtilen "Pad with zero (null) characters" ile şifrelersek, bilgiyi tekrar düzgün açamayabiliriz.

padding için birçok farklı yöntem var. bazıları:

- PKCS#5

  3 adet padding varsa, "3" sayısı her padding karakteri olarak basılır.

  PKCS#5 sadece padding standartı değildir. PKCS#5 dökümanında padding'in yanında bir sürü daha standart vardır. Aynı şekilde PKCS#7'de de bu durum geçerlidir.

- PKCS#7

  PKCS#5 ile aynıdır (çok ufak bir farklılık var sadece).

- Pad with zero (null - 0x00) characters

- Pad with zerospace (0x20) characters

- OneAndZeroes

  padding'in ilk karakteri 1 (0x80), diğer tüm karakterleri 0 olarak kullanılır.

- Pad with zeroes except make the last byte equal to the number of padding bytes

# deterministic çıktı sorunu
bazı padding algoritmaları çıktının deterministik olmamasına sebebiyet vermektedir. bu konu burada tartışılmış durumda: https://crypto.stackexchange.com/questions/66521/why-does-adding-pkcs1-v1-5-padding-make-rsa-encryption-non-deterministic

Dolayısı ile şifre kontrollerinde padding algoritması önemli. şifre hiçbir zaman decrypt edilmeyip, sadece encrypt edip eşitliği kontrol ettiğinden dolayı, bu tarz padding'ler hataya sebep olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# X.509
Dosya formatı değildir. Sertifikasyon standartıdır. Şunları içerir:

- Sertifikaların neler içereceği bilgiler (sertifika imzalama algoritması, sertifika public key'i, versiyon, algoritma ID, seri numarası, konu, geçerlilik süresi...)

- root sertifikaları nedir?

# File formats
X.509 standartına uyan sertifikaları saklamak için farklı formatlar kullanılabilir. Bunlardan bazıları aşağıdaki gibidir.

Aşağıdaki bilgiler saf format bilgisini göstermektedir. İlgili format için farklı/bağımsız standartlar ek özellikler sunabilir. Bu sebeple; private key saklanmayan formatta private key saklayabilir, veya binary olan format base64 halinde kullanılabilir, ilgili dosya şifrelenebilir, gibi...

Private key veya public key dosyasının boyutu "Key size (or key length)" dediğimiz boyuttan farklıdır. Bunun birkaç sebebi var:
- key'in saf hali haricinde, şifreleme hakkında meta-bilgilerde (version, prime numbers...) tutulur.
- -----BEGIN PRIVATE KEY----- gibi delimiter'lar olabilir.
- private key'in saf hali (ve varsa diğer meta-bilgiler) base64 veya farklı bir formata çevrilmiş olabilir.
- dosya ek güvenlik olsun diye şifrelenmiş olabilir.

Key size ile şifrelenecek data'nın boyutu arasında bir max-min ilişkisi olabilir veya olmayabilir. Bu algoritmadan algoritmaya değişmektedir.

# .PFX or .P12
PFX Microsoft'un formatı. P12 ise PKCS#12'nin formatı. P12, PFX'in fork'u. Fakat daha sonra format üzerinde değişiklikler yapıldı mı bu konuda kaynak bulamadım. İnternette 2 formatın apaynı olduğu yazıyor. Fakat resmi kaynak bulamadım.

__Personal Information Exchange Format__

Supports storage only for:
- private keys
- public keys
- certificates

# .CER or .CRT
Supports storage only "for:
- single certificate

# .p8
Standard of PKCS #8.

Supports storage only "for:
- private keys

# .CRL
__Certificate Revocation List__

Designates a certificate that has been revoked.

# .CSR
__Certificate Signing Request__

This file type is issued by applications to submit requests to a Certification Authority.

# .DER
Supports storage only "for:
- single certificate

# .spc or .p7a or .p7b or .p7c
__Cryptographic Message Syntax Standard__

Standart of PKCS#7. Standartta 3 farklı versiyonu var: "a", "b" ve "c". Dosya isimleri ona göre oluyor.

Supports storage only "for:
- certificates

# .PEM
__Privacy-Enhanced Mail__

Supports storage only for:
- private keys
- public keys
- certificates

Base64 formatındadır. Bu sebeple human-readable'dır.

It use below delimiters for each information:

```
-----BEGIN PRIVATE KEY-----
PRIVATE_KEY_IS_HERE
-----END PRIVATE KEY-----
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# PKCS
"RSA Security" kuruluşu tarafından yayımlanan "public-key cryptography standards"'ın kısaltmasıdır. yayımlanan bu güvenlik standartları ailesidir.

15 adet standart içerir (yıl 2019). her sürümde farklı konular ele alınıyor. örneğin;

- PKCS #12

  bir dosya formatı standartıdır. Bu dosya formatında birçok sertifika saklanabiliyor ve bu dosyanın şifrelenmesi için de standart anlatılıyor. dosya uzantısı

  ".p12" olarak kullanılıyor. ".pfx" uzantılı olarakta saklanıyorlar fakat pfx farklı bir standart. sanırım standart yeni bir isimle çatallandı ve/veya çok az değiştirildi. bu konuda resmi bir kaynak bulamadım.

- PKCS #11

  'Cryptoki' olarak isimlendiriliyor. Bu standart sadece bir API. Token'lar üzerinde yapılabilecek işlemler için generic API sunulmuş.

Bazı sürümler:

```
ismi       version
PKCS #1    2.2
PKCS #3    1.4
PKCS #7    1.5
```

Tüm sürümler farklı zamanlarda çıkarılmıştır. Fakat sırası ile çıkmamıştır. Bu sebeple yeni sürümler daha iyi olarak düşünülmemelidir. dikkat edilirse PKCS sürümü ile isminde bulunan numara aynı değil.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DES (or Data Encryption Standart)
simetrik bir şifreleme algoritmasıdır.

# AES (or Advanced Encryption Standard or Gelişmiş Şifreleme Standardı)
Rijndael tarafından geliştirilmiştir. AES, Rijndael'ın proposal'ından ayrı olarak farklı isimde (AES ismi ile) resmileştirilmiştir.

DES artık eski kaldığı için pek tercih edilmemektedir. bu algoritma onun yerini alması için geliştirilmiştir. simetriktir.

"Key size (or key length)" genelde kripto dünyasında bit bazında birime sahiptir. Bu sebeple AES-128 (or AES128), 128 bitlik anahtar istemektedir (başka boyutta anahtar kesinlikle olmaz).

Wikipedia sayfasında bu şekilde notlar düşülmüş (fakat buna karşılık resmi bir kaynak bulamadım):

- Key sizes of 128, 160, 192, 224, and 256 bits are supported by the Rijndael algorithm, but only the 128, 192, and 256-bit key sizes are specified in the AES standard.
- Block sizes of 128, 160, 192, 224, and 256 bits are supported by the Rijndael algorithm for each key size, but only the 128-bit block size is specified in the AES standard.

# MD (or Message Digest)
İsmi hash fonksiyonlarının çıktıları ile aynı anlama geliyor. Bu sebeple anlam karmaşası yaratabiliyor. Aslında hash fonksiyonu ailesidir. kaynak: (source-id: 466) title: "MessageDigest Algorithms", contains MD and SHA on the same table.

Birçok sürümü vardır.

- md2
- md4
- md5: 128 bitlik bir çıktı verir. piyasada en sık kullanılandır.
- md6

# SHA (or Secure Hash Algorithm)
bir çeşit hash algoritmasıdır.

- __sha (or SHA-0)__ - 160 bit (40 karakter) lik hash (çıktı) üretir.

- __SHA-1__ - 160 bit (40 karakter) lik hash (çıktı) üretir.

- __SHA-2__ - altında bir çok şifreleme barındırır. her şifreleme ürettiği hash'in boyutuna göre isimlendirilir. örnek:

   - 256'lık çıktı veren algoritma birçok isimde gösterilir: __SHA-256 (or SHA 256 or SHA256 or SHA-256 bit or SHA2 256 or SHA-2 256 bit)__

   - __SHA-224__, __SHA-384__, __SHA-512__ gibi birçok örnek verilebilir.

   - Sayı ne kadar yüksekse o kadar güvenlik arttırılmış olur, çünkü kırılması o kadar zordur. çünkü; çıktının uzunluğu büyük olduğundan kombinasyonlar çok daha fazla olmaktadır.

- __SHA-3__

  sha2 gibi altında birçok şifreleme barındırır. sh2 gibi aynı şekilde; her şifreleme ürettiği hash'in boyutuna göre isimlendirilir.

  sha3, sha2 ile aynı boyutta hash çıktı boyutlarını destekler. sha3, sh2'den daha sonra çıktığından bu şekilde isimlendirmeler mevcuttur:

  - __SHA-224__ ---> SHA-2 224 bit
  - __SHA3-224__ ---> SHA-3 224 bit

# Neden hala SHA-512 yerine SHA-256 kullanılması öneriliyor?
Teorikte SHA-512 kullanılması önerilir. fakat pratikte 256'yı çözecek bir kudret olmadığından, 256 şu sebeplerden tercih ediliyor:

- 512'nin daha çok network'te bandwidth (or bant genişliği), memory, CPU vb harcıyor olması

- 256'nın yazılımlar tarafından daha çok desteklendiği için (eski olduğu için) daha az hatalı olması.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# JCE (or Java Cryptography Extension) vs JCA (or Java Cryptography Architecture)
JCE eskiden JDK dışında olan bir kütüphane grubuydu. Bağımsız geliştiriliyordu. Aynı zamanda JCA, JDK içerisinde geliştiriliyordu. İkisi benzer işleri yapabiliyordu.

JCE, JDK 1.4 ile birlikte JCA ile birleştirildi ve JDK altında geliştirilmeye devam edildi.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# "double encryption" is more secure and efficient?

Double encryption her zaman ek güvenlik sağlamayabilir. Bunun bazı sebepleri:

- 1

  bazı şifreleme algoritmalarında; bir bilgiyi:
  - K1 ve K2 key'leri ile şifreleme
  - K1 ve K2 şifrelelerini yanyana koyarak şifreleme

  Yukarıdaki 2 durumda da şifrelenmiş veri aynı hızda geri çözülebilmektedir.

  Bazı algoritmalarda ise; 10 ve 5 haneli şifre kullanarak ayrı ayrı şifrelemek yerine, 11 haneli şifre kullanıp 1 kere şifrelemek daha zor kırılmasını sağlamaktadır. Örnek durum: https://crypto.stackexchange.com/questions/6345/why-is-triple-des-using-three-different-keys-vulnerable-to-a-meet-in-the-middle

- 2

  Şifrelerimizi (2 adet farklı şifremiz var) birbirinden bağımsız ortamlarda tutamazsak, 2 kere şifrelememiz neredeyse hiçbir şey ifade etmeyecektir.

- 3

  Kendini kanıtlamış ve stabil algoritmalar zaten matematiksel olarak 1 işlemden oluşmuyorlar. eğer yeteri derecede güçlü olduğu düşünülmüyorsa, işlem sayısı uzatılıyor (veya ona göre algoritmada ek geliştirme yapılıyor). yani; örneğin X algoritması, arkaplanda A ve B algortimalarına benzer bir mantık ile üstüste şifreleme yapıyor olabilir. Dolayısı ile; hem A hemde B ile 2 kere şifreleme yapmanın, X algortimasından daha güvenli olacağını (veya tersini) söylemek çok aşırı detaylı bir çalışma ister.

  Nihayetinde stabil ve kendini kanıtlamış algoritmalar geliştirildiğinde, piyasada var olan diğer algortimalar da incelenip, geliştirilmektedir. Yeni algoritma dünyadaki bir çok bağımsız otorite tarafından onaylanıyor ise, çift şifrelemeye gerek duyan eksik bir geliştirmiş olma ihtimali çok düşüktür.

- 4

  Eğer 2 kere şifreleyeceksek, 2 farklı algoritma kullanmak mantıklı olacaktır. Tabi bu algoritmaların altında yatan logic'lerinde birbirinden çok farklı olduğunu da araştırmış olmamızda yarar var. Benzer temele dayanan algoritmalarla şifrelemek yukarıda yazan diğer maddelere takılmamıza sebep olacaktır. 

  2 kere şifreleme ek proje geliştirme süreçleri doğuyor:

  - algortima temellerini araştıracağımız vakit
  - 2inci şifrelerin farklı ortamlarda tutmak mantıklı demiştik. bu sebeple bu ortamların belirlenmesi, orada şifrelerin nasıl saklanacağı, ve algortimayı çalıştıran sisteme nasıl taşınacağının araştırılması vakti (ve tabiki ilerdeki zamanlarda bu geliştirmelerin bakım maliyeti olacak).

  Yukarıda harcanan vakit yerine; 1 kere şifreleyip, güvenlik sistemimizi best-practice'lere göre daha uygun yapabilmek daha kısa sürebilir ve hatta daha güvenli kalabilmemizi sağlayabilir.

## Ek not
iki kere aynı bilgiyi şifrelemek performansı da olumsuz etkilemektedir. Özellikle Whatsapp gibi end-to-end ecnryption kullanan uygulamalarda, anlık/canlı kameralı arama yapma gibi işlemlerde yavaşlık yaratabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •