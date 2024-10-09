
############################

############################
# NETWORK
############################

############################

# IPv6
> 2001:0db8:0000:0000:0000:8a2e:0370:7334

şu şekilde gösterilir:

> 2001:db8::8a2e:370:7334

IPv4 2^32 (ortalama 4 milyar) IP kabul edebilirken, IPv6 2^128 (ortalama 3.4×10^38) IP kabul edebilir. bu sebeple NAT ihtiyacını ortadan kaldırmaktadır. NAT ihtiyacı zorunlu olmaması privacy konularını gündeme getirmektedir, çünkü bir evdeki subnet'teki her cihaza IP'yi ISP atayabilecektir.

IPv6'da (sadece IP boyu büyümesinden etkilenen header'lar değil) tüm header yapısı da komple değişmiştir.

Bir LAN'da hem IPv6 hemde IPv4 birlikte çalışabilir. Fakat böyle bir durumda o LAN'da iki farklı birbirinden bağımsız network olmuş olur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# F5 Loadbalancer
"F5 Network" firması tarafından dağıtılan load balancer cihazıdır. birçok modeli vardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# network metrics / measures
aşağıdakiler sadece network katmanı için kullanılan terimlerdir. bu terimler piyasada sıkça yanlış kullanılmaktadır.

- __Bandwidth (or bant genişliği)__: belli bir sürede taşınabilen maximum data miktarıdır.

- __Throughput (or işlem hacmi)__: gerçek bandwidth değeridir. bu sebeple "maximum Throughput", "Bandwidth" ile aynı anlama gelir.

- __latency (or gecikme)__: bir datanın bir noktadan diğer noktaya taşınma süresidir. Bandwidth 100 GB/SECOND ise, latency 2 saniye ise, 100GB ancak 2 saniyede karşı tarafa taşınır anlamına gelir.

- speed: bu terimi kullanmak için tanımı daha net yapmak şarttır. tek başına net anlam ifade etmez.

- __data rate__: data eğer "bit" olacak ise, "__bit rate__" ifadesini kullanmak daha doğrudur. bir noktadan bir noktaya taşınan data miktarıdır. Bandwidth ve Throughput gibi maximum veya gerçekçi bir dataya referans edip etmediği bilgisi belirsizdir. bu sebeple bu terimi kullanırken daha net tanım yapmak gereklidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Captive Portal (or Kısıtlama Portalı)
portal hem Türkçe hem İngilizce kelimedir. "ana giriş kapısı" anlamına gelir. webportal gibi terimler bundan türemiştir.

Captive portal tekniği HTTP istemcisini interneti normal olarak kullanmadan önce ağ üzerinde özel bir Web sayfasını görmeye zorlar. Kısıtlama Portalı Web tarayıcısını kimlik denetleme cihazına çevirir. Bu, kullanıcı bir tarayıcı açıp İnternete erişmeye çalışana kadar porttan veya adresten bağımsız olarak bütün paketleri engelleyerek olur. Bu sırada tarayıcı kimlik denetleme ve/veya ödeme, ya da basitçe uygun kullanım poliçesini görüntüleyip kullanıcının onaylamasını sağlayacak Web sayfasına yönlendirilir. Kısıtlama Portalı uygulamaları en çok Wi-Fi erişim noktalarında kullanılır. Ayrıca kablolu kullanımı kontrol etmek için de kullanılabilir. (Otel odaları, iş merkezleri, vs.).

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# network interface

Wi-Fi, kablolu, loopback, sanal network arayüzleri aynı makinada olabilir.

Java ile bunların listesi şu metot'dan geliyor: NetworkInterface.getNetworkInterfaces();

istediğimiz network arayüzünden soket açabiliriz. bir soketi o network adresine bind etmemiz yeterli olacaktır.

bazı arayüzler:

- eno1 yada em1 onboard or embedded network card (genelde anakart içinde olan kartlar)

- eth# genelde anakart dışında ekstra bir kart olduğunda

- w# wireles

- lo loopback

- vbox# VirtualBox'un kendi içinde yarattığı network'ler

- virbr# Libvirt (KVM için sanal makine kütüphanesi) için kendi içinde kullandığı network'ler

- docker# Docker'ın kendi içinde kullandığı network'ler

bu isimleri driver'ın kendisi belirliyor. donanım değil. örneğin aynı donanımı, üçüncü parti geliştirilen açık kaynaklı ve daha sonra kendi resmi sitesindeki driver ile kullandığımızı varsayalım. ikisinde de farklı isim olabilir. eğer driver'dan bu isim algılanamaz ise; işletim sistemi kendi standartlarını kullanıyor. bu sebeple isimlendirmede kesinlik söz konusu değildir. birçok çeşit mevcut: fiberoptikler ayrı, kartın modeline göre yada bir özelliğine göre isimlendiren driver'ler de mevcut.

bir ağdayken her bilgisayarın değil, her network kartının bir IP'si vardır. işletim sisteminde bir yazılım uzak makineye bağlantı açsın. bu bağlantı sadece bir interface üzerinden sağlanabilir. eğer birden fazla NI varsa, destination IP'mize göre işletim sisteminde tanımlı olan routing table'lar aracılığı ile yazılım ilgili NI'a yönlendirilecektir. NI yollanacak bilgiyi yollanacak yere ulaştıracaktır. fakat isterse yazılımlar diğer arayüzleri kullanabilir. NetworkInterface.getNetworkInterfaces() kodu ile interface listesi döndüğünde, listeden istediğimiz elemanı alabiliriz. array döndüğü için interface name'i önceden bilmek zorunda değiliz.

```java
Socket socket = new Socket();
socket.connect(new InetSocketAddress(remote_ip, port));
```

Yukarıdaki yerine aşağıdakini yapabilirsek, hangi arayüzden dışarıya bilgiyi ileteceğimize biz karar vermiş oluruz:

```java
NetworkInterface nif = NetworkInterface.getByName("wlo0"); // arayüzümüzü önceden bildiğimiz varsaydık (yoksa ilk elemanı alabilirdik)
Enumeration<InetAddress> nifAddresses = nif.getInetAddresses(); // NI'ın IPv4 ve IPv6 adresleri array olarak dönmektedir.

Socket socket = new Socket(); //bu satır değişmedi
socket.bind(new InetSocketAddress(nifAddresses.nextElement(), 0)); // IPv4 adresi ile soketimizi bu NI'a bind etmiş oluruz. bu kısım olmazsa OS routing table'lara bakarak bizi uygun NI'ye attach edecekti.
socket.connect(new InetSocketAddress(remote_ip, port)); //bu satır değişmedi
```

# network interface card (or NIC or network interface controller)

network arayüzine denk gelen fiziksel kart.

# Loopback interface

ipconfig komutu ile görülebilen bu interface, tamamen sanal bir ağ bağdaştırıcıdır. bir donanımmış gibi davranır. Local makinanın DNS kayıtlarında IP adresi: 127.0.0.1'dir. Bu IP'nin hostname'i localhost'tur. bilgisayarın kendi içinde haberleşmesi için (örneğin local sunucuya istek yapmak için) kullanılır. cihazda hiç network kartı olmasa bile loopback vardır.

# multiple IP address to one interface

```java
NetworkInterface ni = NetworkInterface.getByName("wlp4s0");

ni.getInetAddresses(); //bu metot bize bir liste döner. bu listede IPv4 ve IPv6 adresleri vardır. bazen network manager sadece IPv4 yada IPv6'yı atamış olabilir.

ni.getInterfaceAddresses(); //getInetAddresses() ile aynıdır. ekstra olarak IP bilgisi yerine network class bilgisini de döner.
```

# virtual interface vs subinterface

örnek: A arayüzinin sub-arayüzleri olabilir. A'ya gelen istekler, hem A hemde onun sub'larına klonlanarak dağıtılır.

her subinterface, bir virtual arayüzdür. oysa tersi her zaman doğru değildir.

# server socket vs client socket

```java
//sunucu taraftaki kod

ServerSocket serverSocket = new ServerSocket(8089);

Socket clientSocket = serverSocket.accept(); //bu metot sürekli bekliyor

clientSocket.getPort(); // returns random port number (X numaralı port olsun)

clientSocket.getLocalPort(); // returns 8089

//client taraftaki kod:

Socket socket = new Socket(IP_OF_SERVER, 8089);

socket.getPort(); // returns 8089

socket.getLocalPort(); // returns random port - yukarıdaki ile aynı port numarası (X)
```

yukarıda görüldüğü gibi; accept işlemi sonrası hala haberleşme server-soket portu (8089) üzerinden devam ediyor. fakat paralelden server-soket o porttan dinlemeye devam ediyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# CCNA (or Cisco Certified Network Associate)
- Cisco kurumumun verdiği network sertifikaları grubudur. altında birçok çeşit sertifika barındırır: güvenlik, data center teknolojileri, bulut bilişim gibi...
- en çok bilinen sertifika "CCNA Routing and Switching"'dır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# imap (or Internet Access Message Protocol) vs pop3 (or Post Office Protocol version 3)

ikiside birer mail sunucu ile haberleşme protokolleridir. imap, pop3'ten daha gelişmiştir.

# smtp (or Simple Mail Transfer Protocol)
imap ve pop3 gibi client-sunucu arasındaki mail protokolüdür. ek olarak smtp; sunucular arası mailleşmek içinde kullanılabilmektedir.

# mail sunucusu
mail sunucusu diğer emaillerle haberleşen yazılımdır. tabi client'lar ile de haberleşir. mailleri client'lardan alır ve kaşı tarafa email atar. bunları backup'ını da tanımlı veritabanında saklar. fakat saklamak zorunda değildir.

# mail client
mail client'lar mail sunucusu tarafından sağlanmak zorunda değildir. web client, mail sunucusuna mail sunucunun açmış olduğu API ile erişir.

# Microsoft Exchange
sunucu yazılımının ismidir. herkes kendi local network'üne kurabilir. mail sunucusu görevinin yanında birçok ek işlev de sağlamaktadır.

# Messaging API (or MAPI)
Microsoft tarafından kullanılan IMAP gelişmişi protokoldür. temel amacı email olsa da birçok ek işleve destek vermektedir.

# CC (or carbon copy)
Carbon Türkçe kelime anlamı: kopya kağıdı

carbon copy Türkçe kelime anlamı: kopya, tıpatıp benzeri

# BCC (or blind carbon copy)
diğerinin göremediği alıcı.

# Multipurpose Internet Mail Extensions (or MIME)
SMTP mail protokolü ile birlikte artık emaillerde text yerine farklı tiplerde bilgiler kullanılabilmektedir. MIME bu standardın ismidir.

Daha sonradan MIME type birçok protokol tarafından kullanılmaya başlandı. Buna en bilindik örnek HTTP'dir. HTTP'de gönderilen mesajın body'sinin tipi ("MIME Type" bilgisi), "Content-Type" header'ında yollanır.

# MIME Type
Güncel resmi dökümanlarda Media Type olarak isim değiştirmiştir.

MIME desteği ile gönderilen emaillerde, gönderilen data tipidir. Bu tipi yazmak için belirli bir standart format vardır:

```
type/subtype.
```

subtype'lar için katı kurallar olmasada "type" kısmında IANA sadece kısıtlı tiplere izin vermektedir.

örnekler:

```
# application others olarak düşünülmelidir. diğer kategorilere giremeyenler burada konumlandırılır.
application/pdf

image/png

# .avi extension file:
video/x-msvideo

# human readable'lar text type'ında yollanmalıdır:
text/plain
text/csv
text/html

# fontlar
font/woff
font/ttf

# .pptx extension file:
application/vnd.openxmlformats-officedocument.presentationml.presentation

# .ppt extension file:
application/vnd.ms-powerpoint

# multipart data'lar bu type ile yollanmalıdır:
multipart/form-data
multipart/byteranges

# "Model" for a 3D object or scene.
model/3mf

# "example" sadece dökümantasyonlarda kullanılmak üzere özel olarak ayrılmıştır. Hem type hemde subtype için kullanılabilir:
audio/example
example/plain

# message isminden de anlaşıldığı gibi email gibi mesajlaşmala sistemleri için kullanılmaktadır.
message/rfc822 (for forwarded or replied-to message quoting)
message/partial to allow breaking a large message into smaller ones automatically to be reassembled by the recipient.
```

Bazı prefix'ler şu anlamda kullanılmaktadır:
- "vnd.": genelde ticari veya vendor spesific ürünlerin standartlarında kullanılmaktadır.
- "prs.": genelde kişisel amaçla (personal) kullanılan MIME type'larda kullanılmaktadır.
- "x." veya "x-": experimental MIME types

Opsiyonel olarak parametrikte yapılabilmektedir:

```
type/subtype;parameter=value
```

Örnek:

```
text/plain;charset=UTF-8
```

IANA ortak bir veritabanı tutumaktadır: https://www.iana.org/assignments/media-types/media-types.xhtml

Bazı MIME type'lar benzerdir. Tamamen aynı tipi referans etse dahi, farklı yazılımlar kendi stamdartlarını belirlediği için dublica olmuş olabilirler. Örnek:

```
application/json
application/x-javascript
text/javascript
text/x-javascript
text/x-json
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DMZ (or demilitarized zone or perimeter network)
Örnek üzerinden gidilirse; bir şirket ağında tüm iç network'ün önünde bir güvenlik duvarı vardır. Dışarıdan içeriye giriş yoktur yada kısıtlıdır. Fakat bazı müşterilerin erişebileceği, yada tüm halka açık bir alt ağ olmasını isteyebilirsiniz. Bu alt ağa verilen özel isim DMZ'dir.

# local area network (or LAN or yerel alan ağı) vs wide area network (or WAN or Geniş alan ağı)
WAN, LAN'ın dış dünyaya açıldığı ilk network için kullanılan bir terimdir. Örneğin; evde kullandığımız internetler için konuşacak olursak, evin içindeki internal network'ümüz LAN, ISP'nin network'ü WAN oluyor.

WAN'ın da WAN'ı olabilir.

# metropolitan area network (or MAN or Metropol alan ağı)
WAN ve LAN arasındaki büyüklükteki network'lere verilen genel bir terimdir.

# intranet
şirket içi (yada özel bir amaç için) network'e verilen takma isim.

# Ekstranet (or extended intranet or extranet)
intranet'e ek olarak organizasyona bağımlı tedarikçiler, müşteriler için genişletilen ağdır. internet ağı buranın dışında kalır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# iptables
Netfilter projesi (birçok network modülü içeren proje ailesi) altında geliştirilen, Linux üzerinde çalışan bir komut satırı uygulamasıdır. IP/port filtrelemesi (örnek engellenmesi) yapar. güvenlik duvarı görevi görür.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Samba
Microsoft Windows harici sistemler için yazılmış, açık kaynaklı dosya paylaşım yazılımıdır. genel olarak dosya paylaşım mekanizmalarında; bir dosya paylaşım yazılımı hem sunucu hem client görevi görebilmektedir. dosya paylaşımı yapıldığında sunucu kısmı, başka makineden dosya okuma işlemleri client modülleri ile gerçekleşir. Samba her ikisini birlikte yapabilmektedir.

# SMB
Microsoft Windows'un dosya paylaşım protokolüdür. Samba yazılımı da bunu desteklemektedir. SMB protokolünün birçok türevi ve sürümü vardır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DHCP (or Dynamic Host Configuration Protocol)
Network'teki cihazlara IP atamak için cihazlarla iletişim protokolüdür.

"DHCP server" ise bu protokolü kullanarak cihazlara IP atayan yazılımdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# broadcast (or yayın)
network üzerinde bir paket yollandığında bir IP belirtilerek yollanır. Fakat her alt ağda bir broadcast IP'si vardır. bu IP'ye atılan paketler, tüm alt ağa komple gönderilir.

# Multicast
network üzerinde belirli bir IP uzayına yapılan yayındır.

# Unicast
Network üzerinde sadece 1 adet IP ye iletilen yayındır.

# anycast
Network'te birden fazla gruplanmış network uzayı olabilir. Bu gruplara multicast ile mesaj atıyoruz. Fakat bazı özel durumlarda, her gruptan sadece 1 node'a istek atmak isteyebiliriz. İşte bu duruma anycast denir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Subnet (or Alt ağ or subnetwork)
IP blokları gruplanarak; alt ağlar meydana getirilir. Bu şekilde hem gruplama yapılmış olur, yönetimi daha kolay olur ve broadcasting işlemleri networku daha az yorar (çünkü sadece ilgili alt ağa broadcast yapılacaktır).

bir network'ü subnetwork'lere ayırma işlemine __subnetting__ denir.

# subnet altındaki özel IP'ler
192.168.1.0/24 --> 24'ten bu çıkıyor: submask = 255.255.255.0 --> 256 tane IP olabilir bu ortamda (bu subnet'te). 192.168.1.0-192.168.1.255 IP aralığıdır.

- network-id -> 192.168.1.0 (bunu kullanamazsın ve bir cihaza atanmaz. bu sadece bir göstergedir.)
- broadcast IP --> 192.168.1.255

# default gateway (or varsayılan ağ geçici)
subnet'te network'te bulamadığımız IP'leri default gateway'a yollarız. bu şekilde ilgili yönlendirmeleri artık ona bırakmış oluruz.

genelde subnet'teki ilk kullanılabilen IP yada son IP veriliyor. fakat istenilen herhangi bir IP'de gateway olarak verilebilir.

# Subnet Mask (or IP Mask or altağ maskesi or IP maskesi)
İki makine birbiri ile aynı ağda olup olmadığını IP ve "IP Mask" değerlerini kullanarak anlayabilirler. "IP Mask" değeri bir altağda herkeste aynıdır.

Örneğin; Aynı network'te, IP maskesi 255.255.255.0 ise, tüm makinelerin IP'lerini ilk 3 sekizliği (yani ilk 24 biti) diğer karşılaştırılan IP ile aynı olmak zorundadır.

# Classless Inter-Domain Routing (or CIDR or Sınıfsız alanlar arası yönlendirme)
Classful network'e alternatif olarak sonradan çıkmıştır.

255.255.255.224 bit bazında yazıldığında: 11111111 11111111 11111111 11100000, 27 tane "1" bit olduğundan, "/27" class network olarak sınıflandırılır. Buradaki 1 bit sadece gösterim amaçlıdır. Fakat kastedilen şudur: "1" yazan basamaklar, o subnet'teki IP'ler tarafından değiştirilemiyor. Yani o kısımlar gerçekten "1" olmak zorunda değil.

# subnetting örnek değerler
örnek 128'lik bloklar halinde subnet'ler oluştursak bu değerler olacaktır:

192.168.1.0/25 --> 25'ten bu sonuç çıkıyor: submask = 255.255.255.128  --> 128 tane IP olabilir.

192.168.1.128/25 --> 25'ten bu sonuç çıkıyor: submask = 255.255.255.128  --> 128 tane IP olabilir.

192.168.2.0/25 --> 25'ten bu sonuç çıkıyor: submask = 255.255.255.128  --> 128 tane IP olabilir.

192.168.2.128/25 --> 25'ten bu sonuç çıkıyor: submask = 255.255.255.128  --> 128 tane IP olabilir.

192.168.3.0/25 --> 25'ten bu sonuç çıkıyor: submask = 255.255.255.128  --> 128 tane IP olabilir.

64'lük subnetwork'lere bölseydik:

192.168.1.0/26 --> 26'ten bu çıkıyor: submask = 255.255.255.192  --> 64 tane IP olabilir.

192.168.1.64/26 --> 26'ten bu çıkıyor: submask = 255.255.255.192  --> 64 tane IP olabilir. (örnek bu ortamda 64 ile 128 arasındaki IP'ler tanımlanabilir)

# Classful network

Aşağıdaki tabloda her "A" (bit değeri), ilgili subnet için değiştirilemez değerdir. Yani ilgili subnet'te hangi IP'yi kullanırsak kullanalım, ancak "x" olanları kullanabiliriz. "A" olanları kullanamayız. A olanlar o subnet için sabittir.

| Class   | IP (bit bazında)                | sub ID (CIDR terminolojisindeki ID'si) |
|---------|---------------------------------|----------------------------------------|
| Class A | AAAAAAA.xxxxxxx.xxxxxxx.xxxxxxx | /8                                     |
| Class B | AAAAAAA.AAAAAAA.xxxxxxx.xxxxxxx | /16                                    |
| Class C | AAAAAAA.AAAAAAA.AAAAAAA.xxxxxxx | /24                                    |
| Class D | AAAAAAA.AAAAAAA.AAAAAAA.AAAAAAA | /32                                    |
| Class E | özel durumlar için ayrılmış     |                                        |

Sub-id "/12" gibi farklı bir değer olursa, ona özel isim verilmemiş. Bu tarz sınıflara "classless subnet" deniliyor.

# subnetting example
Subnetting örneği yapalım: 2 PC var. 2 router. 2 PC farklı subnet'lerde. bunun ağ yapısını IP'leri ile göstererek çizelim.

Her subnet'te total 4 IP'ye ihtiyaç var: 1 PC + network ID + 1 default gateway (router) + 1 broadcast

bu durumda subnet'ler 4'lü olarak bölünecek.

255.255.255.252 = 11111111.11111111.11111111.11111100 --> subnet mask (Burada 4 IP yok. 3 IP var. fakat subnetmask'larda hesaplama hep 1 fazlası olacak şekilde yapılır.)

subnet mask'te 30 adet "1" var.

192.168.1.0 / 30 --> CIDR gösterimi

Network-1:
- network-ID         192.168.1.0
- PC-1               192.168.1.1
- router-1           192.168.1.2 (this is network-1's gateway)
- broadcast IP       192.168.1.3

Network-2
- network-ID         192.168.2.0
- PC-2               192.168.2.1
- router-2           192.168.2.2 (this is network-2's gateway)
- broadcast IP       192.168.2.3

Network-3
- network-ID         192.168.3.0
- router-1           192.168.3.1
- router-2           192.168.3.2
- broadcast IP       192.168.3.3

çizim:

```
                (network-3)
############                      ############
# router-1 # ******************** # router-2 #
############                      ############
   *                                  *
   *                                  *
   *(network-1)                       *(network-2)
   *                                  *
   *                                  *
########                            ########
# PC-1 #                            # PC-2 #
########                            ########
```

Tek bir subnet'i bizim için oluşturan hesaplayıcıdan da buna benzer hesaplamaları basitçe görebiliriz: http://jodies.de/ipcalc?host=192.168.1.0&mask1=30&mask2=

Yukarıdaki network örneğinde PC-1, PC-2'ye direk istek atmak istediğinde atamayacaktır. Bunu yapabilmek için router-1'e şu özel tanımlama yapılmalı: netwrok-2'lik bir istek geldiğinde, bu isteği network-3'e yolla. Bu tanımda şu bilgi yok: "router-2 altında network-2 var". Farklı bir ifadeyle router-1'e şu tanım yapılıyor: "network-2--->router to--->network-3. Network-3'te olan router-2 zaten isteği alması gerektiğini biliyor.

Yukarıdaki network'teki router-2 yerine sadece bir "dummy switch" cihazı taksaydık, o zaman network-3 ve network-2 tek bir network olacaktı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# TCP Socket states

- LISTEN - sunucu - soket gelebilecek bağlantıları bekliyor

- SYN_SENT - client - soket bağlantı kurmak için karşıya istek yapıyor durumda

- SYN_RECV - server - client server-sokete bağlanma isteği yolladı, server kabul etti, ve son onayı tekrar client'tan bekliyor durumda

- ESTABLISHED - sunucu ve istemci - soket bağlantı kurmuş durumda

Buradan aşağıdaki tüm durumlar kapatılma ile ilgili:

- FIN_WAIT1 - sunucu ve istemci - bağlantı sonlandırma bildirisi yollandı. cevap bekleniyor.

- FIN_WAIT2 - sunucu ve istemci - FIN_WAIT1 ile gönderilen bağlantı sonlandırma isteğine, uzak soket onay döndü. artık uzağın bağlantısını kapattığına dair son onayı yollaması bekleniyor. bu bekleniyorken karşıya hiçbir şey atılmıyor. yani; paket alındı, tekrar yeni paket bekleniyor.

- TIME_WAIT - sunucu ve istemci - Uzak soket bağlantıyı kapattığına dair son onayı yolladı. soket closed duruma geçiş için son process'i yürütüyor.

- CLOSED - sunucu ve istemci - bağlantı sonlandırıldı.

- CLOSE_WAIT - sunucu ve istemci - karşı taraf, kapatma bildirisini bize gönderdi. Karşı tarafa bildiri alındığına dair onay yollanır ve soketi kapatma işlemine geçilir. soketi kapatma işlemi için yazılımın onay vermesi gerekir. bu yüzden bu statüde takılan birçok soket oluyor.

- LAST_ACK - sunucu ve istemci - CLOSE_WAIT sonrası Soket kapatıldığında, son olarak kapatıldı bilgisi karşı tarafa gönderilir. gönderildiğinde bu soket CLOSED olur.

# acknowledgement (or acknowledgment or ack)
protokol içerisindeki onay mesajıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Quality of Service (QoS)
Genel bir ismi var fakat özel olarak; internet ağı içinde network'te giden gelen paketlere öncelik vermek için gerekli yapılandırma ve ayarlar bütünüdür. Örneğin modem, voip ve HTTP üzerinden isteklere, download edilen bir dosyadan daha fazla öncelik verir. Bu sadece bir parametredir. bunun gibi bir çok önlem alınabilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# VPN (or Virtual Private Network) vs Proxy server (or Vekil sunucu or yetkili sunucu)
Temelde aşırı benzer teknik mimarilerdir. Fakat amaçlı farklıdır. VPN client'ı başka bir network'teymiş gibi davrandırtırken, proxy sadece isteklere aracılık etmek için vardır.

VPN ve Proxy yazılımları yazılım katmanında oldukları için bir çok opsiyonel ek özellik sunabilirler. Örneğin;
- yasaklı internet sitelerine erişmek veya tersi (engellemek)
- ek şifreleme yapabilirler (böylece HTTPS olmayan bağlantıları ISP dahi göremez),
- güvenlik için paketleri inceleyebilir,
- log sistemi olabilir,
- GUI sunabilir,
- isteklere aracılık yaparak internet hızını arttırmak,
- tüm client'lar için statik datalara cache koyabilir...

Hem VPN hemde Proxy için:
- hem client hemde server tarafta yazılımın olması şarttır.
- OS seviyesinde veya admin yetkisi ile çalışılmak zorunda değildir. Sadece user seviyesi yeterlidir.
- Aynı OS'ta, sadece istediğimiz istediğimiz (bazı) uygulamalarda VPN ve/veya Proxy kullanabiliriz. Fakat;
  
  - özellikle şirketler, çalışanlarının makinelerine admin yetkisi ile VPN ve/veya proxy kurar. Bu şekilde o VPN uygulamasının tüm OS uygulamalarında geçerli olmasını sağlarlar.

  - VPN vey Proxy uygulamaları admin olarak çalışmazlarsa, runtime'daki tüm uygulamaların connection'larını yönetemezler. OS izin vermez. Dolayısı ile her programın içine girip, VPN veya Proxy ayarlarını manuel set etmek gerekir ki bu çok uzun ve genelde elle yapılabilen bir işlemdir. Bu sebeple çoğu VPN veya Proxy uygulaması admin olarak çalışacak şekilde tasarlanır. VPN admin olarak çalıştırıldığında, kendini OS'a NIC olarak ekler. Bu NIC'i, ipconfig komutu ile basitçe görebiliriz. (Proxy'ler NIC'e mi ekliyor kendini bilmiyorum).

- Aynı OS'ta, aynı yazılımda farklı TCP bağlantılarında farklı VPN'ler ve/veya Proxy'ler kullanabiliriz veya kullanmayabiliriz. Yani VPN ve Proxy connection bazlıdır.

# OpenVPN
Açık kaynaklı VPN uygulaması ve protokolüdür. Aynı zamanda bu yazılımı geliştiren firma, OpenVPN protokolü ve yazılımları ile bulut VPN hizmeti de sunmaktadır.

# SSL
Proxy ve/veya VPN SSL'i isterse yönetebilir. Fakat web tarayıcısı sertifika uyarısı verecektir. Eğer SSL'i yönetirse ve sertifika uyarısını ignore edersek, VPN ve/veya Proxy dataları açık şekilde görebilir. Fakat eğer Web tarayıcısında Javascript'te değil isek, yani; native OS uygulaması kullanıyor isek, SSL yokmuş gibi connection açmadan önce (bağımsız) ek bir şifreleme yapabiliriz. Bu şekilde SSL açılsa bile aracı olan VPN ve/veya Proxy, data'ları anlamlandıramayacaktır. (Aslında bu işlemi Browser'da da yapabiliriz. Fakat network admin JS'teki key'i çok rahat elde edebileceği için, bir anlamı kalmayacaktır).

Hiçbir proxy türü, client'a sertifika uyarısı verdirtmeden aradaki data'ları göremez/açamaz. Bunu şuradan çıkarabiliriz: https://docs.mitmproxy.org/stable/concepts-howmitmproxyworks/ Bu sayfada "Explicit HTTPS" başlığında HTTPS isteklerinin standart bir Proxy üzerinden nasıl iletildiğini anlatıyor. Hemen bir alt başlığın olan "The MITM in mitmproxy" de ise, mitmproxy'nin sertifika kullanarak işlemi kendi şifrelediğni anlayabiliyoruz.

## HTTPS proxy vs HTTP proxy
Bu tanımların RFC gibi akademik kaynaklarda karşılığını bulamadım.

HTTP protokolünü destekleyene proxy'lerde şu özellikler olabilir:

- 1. protokol desteklemesi:
     - sadece HTTP deskteler, HTTPS desketkelemez.
     - hem HTTP, hemde HTTPS destekler.
- 2. client ve proxy arasındaki bağlantıyı ek SSL protokolü ile bir şifreleme yapar veya yapmaz.  Böylece client ve proxy arasında olan;
     - HTTP istekleri (SSL olmayanlar)
     - HTTPS fakat bağlantı kurulurkenki handshake header'larını
     - Eski sürüm SSL kullanan bağlantıları

     kötü niyetli aracılardan ve ISP'den gizlemiş olur.

Bazı makaleler, yukarıdaki iki özellikten sadece birini destekleyen sunuculara HTTPS proxy diyor. Bazı makaleler ise her iki özelliği birden destekleyen sunuculara HTTPS proxy deniliyor. Dolayısı ile okuduğumuz makalede bu durumunu netleştirmek zor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Network Address Translation (or NAT or Ağ Adresi Dönüştürme)
Router'ın bir IP adresi olsun. Bu router'ın arkasında birden fazla cihaz olabilir. Yani bir (çıkış) IP'yi birçok cihaz kullanmaktadır. Yönlendirici kendi içinde haritalama yaparak kimin hangi IP'yi kullandığını belirleme mekanizmasıdır. Bu mekanizmanın birden fazla yöntemi vardır:

  - # Static NAT
    Router'larda çıkış IP'si tek olmayabilir. Bize bir çıkış IP aralığı verilmiş olabilir. Böyle durumlarda içerideki her cihazı, sabit olarak bir çıkış IP'sine haritaladığımızda bu yöntemi kullanmış oluruz.

  - # Dynamic NAT
    Static ile aynı mantıkta çalışır. Tek farkı hangi IP'nin hangi çıkış ile gideceğine router o sıra karar verir (önceden belirlenmez).

# Port Adres Çevirimi (Port Address Translation or PAT or NAPT or Network PAT)
Overloading (aşırı yüklenme) olduğu zamanlar tercih edilen NAT yöntemidir. Örneğin; bir çıkıp IP'miz var. İçeride ise on tane makinamız. Bu durumda router, içerideki her IP'nin her IP:PORT'u çıkıştaki bir IP:PORT'a bağlar. çıkışta bir adet IP'miz olduğundan maximum TCP soket portu sayısı kadar farklı bağlantı kurabiliriz. Bu durumda lokalde 2 makina olsun, 2 makina birde 65bin adet portunu aynı anda kullanamayacaktır.

# Port yönlendirme (or port mapping or port redirection)
Router ayarında olan bir özelliktir. Dışarıdan X portuna gelen bağlantılar, local tarafta A IP'li bilgisayarın Z portuna yönlendirilmesi sağlanabiliyor.

# Port Triggering (or Port tetikleme)
ort yönlendirme ile aynı şeydir. sadece ek olarak; yönlendirilen port kullanılmadığında kapalı kalır. bu şekilde ek güvenlik sağlanır.

# virtual servers
port forwarding ile aynı şeydir. fakat bu terim sadece external port ve internal port aynı olduğu zaman kullanılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# fiziksel network aygıtları

- ## 1- Hub
Yerel ağ oluşturmak için gerekli cihaz. Sadece veriyi alıp dağıtmak için kullanılır. Bir özelliği yoktur. Gelen veri-paketlerini bağlı olan tüm kablolarına dağıtır.

- ## 2- Switch (or switching hub or bridging hub or MAC bridge or dağıtıcı or ağ anahtarı)

Temel olarak 2 ye bölünüyor:

- ### 2.1- Dummy switch

Hub ile aynı görevi görür. Fakat hub'dan daha zekidir. Gelen paketi inceleler ve hangi port'a (kablo girişine) o bilgi gönderilecekse, sadece oraya aktarır. Gereksiz ağ trafiğini de engellemiş olur. Bu işlemi yaparken MAC adreslerine bakıp yapar. IP'ye göre dağıtım yapmaz.

- ### 2.2- Managable switch

Portlara bağlı olan LAN tanımları cihaza yapılır. Bu şekilde gereksiz yere her tarafa paket atmayabilir. Eğer Managable switch'e hiçbir ayar yapılmaz ise dummy switch gibi çalışır.

Not: Piyasada satılan modem ve router'larda genelde bir çok özellik içinde geliyor. Fakat sadece modem veya sadece saf router satılan ürünlerde var. Aşağıdaki mödüller router veya modem içerisine gömülü gelebiliyor:
- ek güvenlik çözümleri (firewal)
- switch
- DHCP server

- ## 3- Router (or Yönlendirici)
Router'lar switch'lere benzer mantıkta çalışırlar fakat bağlı olan bilgisayarları değil, network'leri birbirine bağlarlar. Bu yüzden genelde az portu olan bir cihazdır.

son kullanıcıya satılan router'lar içerilerinde switch teknolojisi ile entegre gelirler. bu şekilde router sonrası switch alma gereksinimini ortadan kaldırır. bu yüzden piyasada, çok porta sahip router'lar görebiliriz.

switch, LAN içerisinde MAC adreslerini bilir ve ona göre paketleri filtreler. Oysa router, IP adreslerini bilir. network'leri birbirine bağlar.

- ## 4- Modem
Dijital-analog sinyal çevirimini yaparak verilerin kablo üzerinden iletilmesini sağlar. Bu yüzden ismini modulator-demodulator kelimelerinden almaktadır. Son kullanıcıya sunulan modem'lerde çoğunda router modülü entegre gelir. bu şekilde router almaya gerek kalmaz.

# Ağların fiziksel iletişim çeşitleri

- ## Dial-up Internet access (or Çevirmeli ağ)
telefon standartlarını (kablolarını) kullanarak erişimin sağlandığı fiziksel yöntemdir. Max hız 56 kbit/s'dir.

- ## digital subscriber line (or DSL)
bakır kablo ile iletişim sağlanan iletişim yapısıdır. xDSL olarak adlandırılırlar. x yerine kullanılan teknolojinin harfi gelir: aDSL, vDSL gibi.

- ## Fiberoptik
Metal kablolar yerine fiber kablolar kullanılır. fiber kablolar cam veya plastik'ten meydana gelmektedir. ışık ile veri iletimini sağlar. metal kablolara göre temel avantajları: manyetik alandan hiç etkilenmemeleri ve daha diğer gürültü çeşitlerinden daha az etkilenmeleridir.

- ## uydu

- ## kablo (or cable)
Kablo kelime anlamı olarak tüm kablo çeşitlerini kapsamaktadır. Fakat local ağlarda iletişim için kullanılmak üzere geliştirilmiş özel network kabloları vardır.

# Sinyal çeşitleri

- ## Dijital Sinyal (or sayısal signal)
Bu sinyalde iki değer vardır. Bu iki değer 1 ve 0 dır. Bu iki değer farklı şekillerde adlandırılabilir. Açık ve kapalı,doğru ve yanlış,yüksek ve düşük gibi.

- ## Analog sinyal (or analog signal)
Yönü ve şiddeti zamanla değişen sinyallere denir. Analog sinyale en güzel örnek ses sinyalidir. Digital sinyallerde max 1, min 0 olarak temsil edilirken, analog'da bu değer herhangi bir sayı aralığı olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Access Point
bilgisayarın ilgili iletişim ağından erişilen ilk noktadır. Örneğin bir bilgisayar bir Wi-Fiye erişiyorsa, Wi-Fi cihazı bir access point olur. aynı şekilde Wi-Fi haricindeki örneklerde verilebilir. fakat günlük hayatta halk arasında genelde sadece Wi-Fi için bu terim kullanılmaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Ethernet
Sadece kablo aracılığı ile cihazlar arası iletişim kurulması için gerekli standartlardır. Wi-Fi ise radyo dalgaları (radyo: elektrik dalgalarının özelliğinden yararlanarak seslerin iletilmesi sistemi) ile iletişim kurma standardıdır.

# IEEE 802
Tüm network iletişim standartlarının tanımlandığı standartlar ailesidir. ALtında birçok standart vardır. Bu standartlar yanında numara ve karakterlerle temsil edilirler. Örneğin; IEEE 802.11 kablosuz ağ standartlarıdır. 802.11g kablosuz ağ standartlarının bir sürümüdür. IEEE 802.3 Ethernet standartlarıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# fiziksel telefon ağ yapıları

- ## Plain old telephone service (or POTS or plain ordinary telephone service)
  Bazı kaynaklarda "classic telephone system (or klasik telefon sistemi)" olarakta isimlendirilmektedir.

  eski usul analog telefon iletişim protokolüdür.

- ## ISDN (or Integrated Services Digital Network or Tümleşik Hizmetler Sayısal Şebekesi)
  en eski usul telefonlardan sonra çıkan ilk dijital telefon altyapısıdır. internet ağı üzerinden haberleşmez. Kendi özel bir ağdır. Fakat ISDN, video ve görüntü aktarımı da yapabilmektedir.

# internet telefonu
internet bağlantısı kullanılarak haberleşmeyi sağlayan altyapılara verilen genel isimdir. internet telefonları direk internet kablosuna bağlıdır. içerisinde yazılım gömülü gelir ve internetten (Skype, Whatsapp'ın yaptığı gibi) sesli veya görüntülü konuşmayı sağlar. (Not: Görüntülü konuşmaya imkan veren her telefon, "internet telefonu" olmak zorunda değil. "ISDN" hatları da görüntülü konuşmayı desteklemektedir.)

# VoIP (or voice over IP)
yazılım protokolüdür. genel bir isimdir. cihazlar aracılığı ile paketler diğer telefon altyapılarına çevrilecek şekilde kolay tasarlanmıştır. bu şekilde internet üzerinden, normal telefonlar ile konuşulabilmektedir. normal telefonlardan daha ucuz olabilirler. çünkü normal telefon hatları mesafeler uzadıkça daha maliyetli olmaktadır. fakat voip hizmeti sunan sunucular, bizi internet ağı üzerinden ulaşacağımız telefona en yakın yere yönlendirip, yönlendirdiği endpoint üzerinden paketleri çevirerek normal telefon hattı olan bir yere bağlamaktadır. bu da maliyeti ucuzlatmaktadır.

# SIP (or Session Initiation Protocol)
özel bir voip protokolüdür.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# UPnP (or Universal Plug&Play or Evrensel Tak&Çalıştır)
Elektronik cihazların internet ağı üzerinde birbirini otomatik algılaması için geliştirilen bir protokoldür. Ağ'ı yöneten router'ın UPnP desteği olması ve çalışan yazılımın (elektronik cihaz içerisindeki) UPnP supportu olması şarttır.

örneğin UPnP destekli modemler, modeme bağlı client'lardan aldığı komutlar doğrultusunda port yönlendirme yapabiliyor. böylece örneğin VNC içerisindeki bir seçenek ile port yönlendirme yapılmış oluyor ve Teamviewer gibi merkezi sunucuya ihtiyaç kalmıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Delik Açma (or Hole Punching)
Teamviewer gibi port yönlendirmenin yapılmaması istenen sistemlerde birbiri ile haberleşmesi gereken makinalar söz konusudur. böyle durumlarda merkezi bir sunucu ihtiyacı vardır. bu sunucu "S", "A"  ve "B" makinelerinin haberleşmesini sağlar. Öncelikle A ve B, S'e bağlanır. S, A'ya B'nin, B'ye de A'nın bilgilerini verir. artık A yada B programın ihtiyacına göre sunucu gibi davranır ve yeni bir soketten dinleme yapmaya başlar. örneğin; A dinleme yapmaya başladı. B ise anın portunu biliyor. bu yöntem ile artık B, A ile soket üzerinden haberleşebilir. bu yönteme "delik açma" yöntemi denir.

bu yöntem her zaman %100 temiz çalışmayabiliyor. zira router'ların nat sistemi her zaman beklenilen gibi işlemeyebiliyor. örneğin; dışarıdan  bağlantı gelince router bunu tehlike olarak görüp portu kapatabilir. yada local-external port arada değişebilir vs... bu sebeple son kullanıcı uygulamalarında bu teknik kullanılmaz. Teamviewer gibi uygulamalarda Teamviewer'ın sunucusu a'dan b'ye ekran görüntülerini kendi üzerinden aktarır. yani tüm transfer edilen veriler Teamviewer sunucuları üzerinden geçer. Teamviewer sunucusu yazılımsal aracı görevi görür.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Encapsulation
OSI TCP layer'daki gibi bir protokolün, alt protokole data yerleştirmesidir. tersi (__decapsulation__) ise alt protokoldeki bilgilerin üst protokollere taşınmasıdır/yerleştirilmesidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# routing table (or routing information base or RIB)
router içinde yada bilgisayarda (OS'ta) tanımlı olan bir tablodur. bu tabloda hangi IP'ye gidildiğinde bizi nereye yönlendireceği belirtilir. normalde; bir bilgisayar bir IP'ye gitmek istediğinde network'te bağlı olduğu router'a paketi atar ve paket router tarafından gerekli yere yollanır. fakat bazen;

- uzak network'teki iç bilgisayarlara özel bir yönlendirme yapmak zorunda kalabiliriz

- hızlı routing olması amaçlı bildiğimiz makinelere routing yaptırabiliriz (arada paket farklı router veya benzeri cihazlara sorgu atmamış olur)

- farklı network kartlarından (sanal yada fiziksel kartlardan) hangisinden çıkacağımızı manuel belirlememiz gerekebilir

gibi...

örnek routing table listeleyen komut:

```sh
route -n
# or "ip route"
# or "netstat -rn"
```

çıktısı:

```
Destination   Gateway      Genmask        Flags Metric Ref   Use Iface
172.16.55.0   0.0.0.0      255.255.255.0  U     0      0       0 eth0
172.16.50.0   172.16.55.36 255.255.255.0  UG    0      0       0 eth0
127.0.0.0     0.0.0.0      255.0.0.0      U     0      0       0 lo
0.0.0.0       172.16.55.1  0.0.0.0        UG    0      0       0 eth0
```

Yukarıdaki çıktıda:
- Iface --> Interface anlamına geliyor
- Genmask - ilgili arayüzye bağlı network'ün subnetmask değeri
- gateway sütununda yıldızlı bir satır var ise, ilgili network/interface için gateway'a ihtiyaç yok/kullanılmıyor demektir. (bu değeri override edebiliriz. network'e yanlış paket gereksiz/yanlış oluruz.)
- 0.0.0.0 satırı (son satır) eğer farklı bir routing satırına uymayan istek için default algılanacak arayüzdür.
- Genelde internete (dış dünyaya) açılan kartımız default olan olmalı. eğer böyle bir durum yok ise, bunu yine "route" komutu ile default'u değiştirebiliriz. routing table user tarafından düzenlenebilir.
- flag'lerin anlamları:
  - U - routing is enabled
  - H - bu routing satırının spesific bir host'a yönlendirdiğini belirtiyor (örneğin bir IP'yi spesific bir host'a yönlendirebiliriz). Bu flag yoksa, ilgili request network'e yönlendirilir.

Yukarıdaki routing table olan bir makinede bir IP'ye request yapalım. OS önce routing table'dan eşleştirme yapmaya çalışıyor. eğer bulamazsa default olan Network card'ına yönlendirecek. Eşleştirme yaparken destination ve genmask'a bakıyor. genmask, bağlı olan network kartındaki diğer IP'lerin bilgisini alabilmemiz sağlıyor. Destination ise network-ID olarak düşünülmelidir. Dolayısı ile eğer ilgili request bu network'teki bir cihaza gidecek ise ilgili network kartına yönlendiriyor.

# komut satırı uygulamaları ile takip etme
"traceroute" POSIX'lerde, "tracert" ise Microsoft Windows sistemlerde komut satırı uygulamalarıdır. bu uygulamalar bir IP'ye gidilmesi için gerekli tüm aradaki node'ların(router'ların) iplerini gösterir. bu işlemi gerçekten de karşıya paket atarak yapar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DigitalOcean
Amazon'un bulut hizmetleri gibi sanal sunucular sunan bir firma.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Skype Wi-Fi
Microsoft'un Skype-Wi-Fi için anlaştığı Wi-Fi olan bir mekanda Skype hesabınız ile Wi-Fiye login oluyorsunuz. bu Skype hesabınızda önceden kredi yüklü olmalıdır. Wi-Fi'ye login olunduğunda Skype-Wi-Fi uygulaması başlatılmalıdır. Skype Wi-Fi artık dakika üzerinden ücret tarifesi başlatmakta ve interneti sınırsız olarak kullandırtmaktadır. Skype-Wi-Fi, Skype uygulamasından tamamen bağımsız bir uygulamadır.

# Hot spot firmaları
Skype Wi-Fi gibi bir çok firma anlaşmalı olarak bu işi yapabilmektedir. tek bir modemden Hotspot yaparak birçok ağ yaratılıp birçok markanın ağına bağlanabiliyoruz. bunu yapan hizmet sağlayıcılar mevcut.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# instant messaging (IM)
anlık mesajlaşma teknolojilerine verilen genel isim.

# Extensible Messaging and Presence Protocol (XMPP)
mesajlaşma protokolüdür. merkezi bir sunucusu yoktur. herkes bir sunucu kurup bu hizmeti bağımsızca kullanabilmeyi sağlar. eskiden Google ve Facebook chat protokollerinden XMPP tabanlı sistemlerden yararlanıyordu fakat artık bu teknolojiyi bıraktılar. şu anda XMPP Android ve iOS için push notification'larda kısmen kullanılmaktadır.

# Jabber
XMPP'nin eski ismidir. Şu anda Jabber.org sitesinden XMPP protokolü kullanan bir chat mesajlaşma sunucusu hizmet vermektedir.

# Xabber
Android XMPP client app.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# TCP/IP protocol suite


# protocol suite vs protocol stack
iki terim piyasada birbiri yerine kullanılmaktadır. Oysa bu iki terim net bir şekilde ayrılmıştır.

  - # protocol suite (or Internet Protocol suite)
      Protocol stack implementasyonundaki herhangi bir katmanı (veya katmanları) yürütebilmemiz için gerekli tüm protokollerin kümesidir.

      - # TCP/IP protocol suite
        TCP/IP model'indeki transport ve internet katmanlarını yürütebilmemiz için gerekli bir protokol kümesidir.

  - # protocol stack (or Internet Protocol Stack)

    tüm ağ iletişim yapısını temsilen modelleyen katmanlı mimaridir.

    - # Open Systems Interconnection (or OSI or Open System Interconnection Reference Model or OSI/RM)
      ISO tarafından belirlenmiş ağ katmanlarının belirlendiği modelidir.

    - # TCP/IP
      OSI'ye alternatif bir ağ katman modelidir. Bu model'de sadece 5 katman vardır. TCP IP adlandırılmasının sebebi, en çok bu protokollerin kullanılmasıdır.

# TCP/IP vs OSI
aşağıdaki tablo için kaynak: (source-id: 336) "Table 4-2 TCP/IP Protocol Stack"

| OSI Ref. Layer No. | OSI Layer Equivalent               | TCP/IP Layer     | TCP/IP Protocol Examples                                                    |
|--------------------|------------------------------------|------------------|-----------------------------------------------------------------------------|
| 5,6,7              | Application, Session, Presentation | Application      | NFS, NIS+, DNS, Telnet, FTP, Rlogin, RSH, RCP, RIP, RDISC, SNMP, and others |
| 4                  | Transport                          | Transport        | TCP, UDP                                                                    |
| 3                  | Network                            | Internet         | IP, ARP, ICMP                                                               |
| 2                  | Data Link                          | Data Link        | PPP, IEEE 802.2                                                             |
| 1                  | Physical                           | Physical Network | Ethernet (IEEE 802.3) Token Ring, RS-232, others                            |

TCP/IP protokolü birçok makalede farklı seviye isimleri ve hatta farklı seviye sayıları ile tanıtılmıştır. kaynak: (source-id: 337) "Layer names and number of layers in the literature". Wikipedia'daki belirtilen bu tabloda her sütun için kaynak belirtilmiştir.

Alt katmandaki protokol, bir üst katmandaki veriyi alıp ona header'ını ekliyor ve gerekiyorsa veriyi manipüle ediyor (örnek: şifreliyor...). Tüm sistem her katmanında bu işlem gerçekleşiyor.

# OSI Model
Üst seviyeden aşağıya doğru detaylar aşağıda yazmaktadır:

- isim
  - ilgilendiği birim
  - açıklama
  - örnek protokoller

- Application
  - Data
  - High-level APIs
  - HTTP, FTP, Telnet, SMTP, SSH

- Presentation (Sunum)
  - Data
  - ensures that data is in usable format. Examples:
    - character encoding
    - data compression
    - encryption/decryption
    - MIME types (png, text...)
  - SSL/TLS

- Session (Oturum)
  - Data
  - Burası o anda kullandığımız protokolün mantığına göre değişir. 1 oturumda bir den fazla data yollanabilir. Oturum hala açık kalacaktır. Taki oturum kapatılıp yeni oturum açılana kadar.
  - Managing communication sessions

- Transport
  - Segment (when using TCP) / Datagram (when using UDP)
  - Reliable transmission of data segments between points on a network.
  - TCP, UDP, port

- Network
  - Packet
  - transport katmanından gelen verilere destination IP, source IP gibi data'larla çerçeveler.
  - IPv4, IPv6

- Data link
  - Frame
  - verinin frame'lere bölünmesini sağlar. Services provided by the Data Link Layer to the Network Layer include data link connection, sequencing, error notification, flow control, and data unit transfer. kaynak: (source-id: 338) "DATAPRO" report. "ISO Reference Model for Open Systems Interconnection (OSI)".
  - ATM, Ethernet. kaynak: (source-id: 339)

- Physical
  - Bit
  - bit bazında işlemlerin fiziksel olarak transfer olmasından sorumludur. tamamı donanım kısmında yapılır.
  - Ethernet, USB, Bluetooth

# TCP socket header
header kısmı en aşağıdaki data haricindeki kısmı kapsar.

Aşağıdaki büyük grafik bir TCP paketini göstermektedir. Birçok TCP paketi birleşerek bir segment'i oluşturur.

```
    0                   1                   2                   3

    0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1 2 3 4 5 6 7 8 9 0 1

   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   |          Source Port          |       Destination Port        |

   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   |                        Sequence Number                        |

   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   |                    Acknowledgment Number                      |

   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   |  Data |           |U|A|P|R|S|F|                               |

   | Offset| Reserved  |R|C|S|S|Y|I|            Window             |

   |       |           |G|K|H|T|N|N|                               |

   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   |           Checksum            |         Urgent Pointer        |

   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   |                    Options                    |    Padding    |

   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+

   |                             data                              |

   +-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+
```

- #### Sequence Number vs Acknowledgment
ikiside bilginin başlangıç byte numarasıdır. karşı tarafa paket attığımızda bizim aynı pakette yolladığımız data'nın tüm segmentteki kaçıncı bayt olduğu bilgisi sequence'de yollanıyor. oysa ACK'da karşı taraftan kaçıncı sıradaki bilgiyi istediğimizi bilgisini yolluyoruz. örnek; yolladığımız pakette SEQ=5, ACK=90 olsun. bu şu demek oluyor: yolladığım paket tüm segment için 5'inci byte'tan başlayan bir data içeriyor. ve ben buna karşılık karşıdan 90inci bayttan başlayan bilgiyi istiyorum. karşı taraftan ne kadar büyüklükte bayt geleceği ve benim ne kadar büyüklükte bayt yolladığım bilgisi SEQ ve ACK'da belirtilmemektedir.

- #### data offset
options kısmının uzunluğu değişebiliyor. bu sebeple sadece tüm header'ın uzunluğu burada belirtilmektedir. bu şekilde data'nın nereden başladığını bilebiliriz.

- #### Reserved
hiçbir işe yaramaz. ileride  TCP sürümü çıktığında kullanılması için rezerve edilmiştir.

- #### Control Bits (or Flags)
bazı sinyalleri içerir.

- #### window
TCP protokolünde her paket sırası ile yollanır fakat cevap gelmeden diğerleri de yollanır. bunun yapılmasının sebebi kuyruk mekanizması yerine paralel mekanizma ile zamandan kazanılmasıdır.

  window kısmı byte biriminde bir uzunluk belirtir. bu kısımda bu paketi yollayan kişinin kaç adet paket daha almaya hazır olduğunu belirtir. bu şekilde karşı taraf paralel olarak buradaki boyu kadar byte yollayabilir. böylece karşılıklı denge ile paralel data yollanır. eğer bu window sayısı olmasaydı, herkes birbirine aynı anda tüm bandwidth kadar paket yollardı. rşı tarafın müsaitlik durumunu buradaki sayı ile anlayabiliriz.

- #### Checksum
destination IP + source IP + protocol bilgisi (bizim senaryomuzda "TCP") + TCP header ve body length --> bu 3 adet bilgi daha alt seviyeli bir katmanda yollanıyor. bu 3 adet bilginin hash'i checksum'a atılıyor.

- #### urgent point
"URG" control bit'i set edilmişse bu kısımda öncelik numarası verilmektedir. paketin önceliği için gerekli sıra numarası vardır.

- #### options
opsiyonel bir alandır. ekstra bilgiler içerir. örnek: timestamp.

- #### padding
TCP header'ın belli bir sayının katı olması beklenir. yani örneğin; 29 bit olamaz, onun yerine 32 olabilir. dolayısı ile 32'ye tamamlamak için padding kısmına boş data atılır.

# heartbeat
TCP soketi herşeyin sorunsuz olduğu koşullarda data yollanmazsa bile kapanmaz. sürekli açık bekler. fakat arada proxy varsa, firewall varsa gibi sebeplerden TCP soketleri aracılar tarafından kapatılabilir. bu sebeple TCP soketimizi açık kalmasını istiyorsak düzenli olarak karşı tarafa yaşadığımızı belli eden sinyal atmalıyız. protokol dünyasında bu tarz sinyallere yazılımcılar arasında heartbeat deniyor.

TCP standartlarında özel bir sinyal yok. fakat herkes tarafından kullanılan bir TCP sinyali var: "keep-alive". bu data kısmı boş olan bir TCP paketidir. hangi aralıklarla atılacağı tamamen yazılımcının kendisine kalmıştır.

# broken pipe
karşı taraf bağlantıyı kapatmış ise ve biz hala data yollamaya çalıştığımızda alacağımız hatadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# MAC adres (or media access control address)
MAC adres donanımın içinden sürücüler aracılığı ile yazılım tarafından okunur. fakat MAC adres bilgisi (en azından network cihazları için) network-internet ağı iletişimlerinde yazılım katmanında taşındığı için kolaylıkla sahte bilgi kullanılabilmektedir.

'mac' birçok terimin kısaltmasına denk gelmektedir: (source-id: 451). bazıları:
- modified, access, creation times on filesystem
- 'message authentication code' kriptografide kullanılan, mesajın hash değeridir. bu şekilde mesajın değişip değişmediği doğrulanabilir. MAC değeri key ile imzalandığı için aynı zamanda, mesajın istenilen alıcıdan geldiği de doğrulanmış olur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# SSH (or secure shell)
ağ üzerinde güvenli veri iletimi sağlamak için geliştirilmiş protokol.

shh uygulaması, o anda sunucu tarafta run edilen işletim sistemi user'ının şifresini istemez. onun yerine SSH sunucusunun config dosyasında belirtilen şifreyi yada public key'i ister. eğer istersek istenen şifreyi OS user'ın şifresine bağlayabiliriz. (Aynı durum FTP'de de vardır.)

SSH tamamen işletim sisteminden bağımsız olan bir uygulamadır. SSH'a bağlandığımızda SSH üzerinden verdiğimiz komutlar, SSH server uygulamasına TCP üzerinden yollanır. server uygulama bu komutları direk uzak işletim sistemine yollar. eğer admin gerektiren bir işlem yaparsak işletim sisteminin admin-user şifresini girmemiz gerekir. (Aynı durum FTP'de de vardır.)

aynı makineye birçok SSH bağlantısı olabilir. SSH bağlantısında açılan process'ler, SSH session'ı sonlandığında duruma göre kapatılmayabilir. (bu konu nohup/disown'un anlatıldığı başlıklarda anlatılıyor)

SSH ekstra olarak ("-X" parametresi verilmiş ise), kurduğu TCP bağlantısı üzerinden uzak masaüstündeki X server'a bağlanabilmemizi de sağlar.

# OpenSSH
açık kaynaklı SSH sunucu ve client uygulamasıdır.

# OpenSSL
açık kaynaklı SSL işlemleri yapan komut satırı uygulaması ve kütüphanesidir.

# Heartbleed
OpenSSL'in 2014'te çıkan meşhur güvenlik açığı.

# GnuPG (or GNU Privacy Guard)
__OpenPGP__ implementasyonudur.

OpenPGP ise __PGP yada Pretty Good Privacy)__'nin fork'udur.

hem OpenPGP hemde GnuPG kendi komut satırı uygulamalarını sunar. bu uygulama ile dosya, disk (herhangi bir data) şifrelenip decrypt edilebilir. şifreleme metotu ve diğer parametreler dışarıdan verilebilir.

OpenPGP, "pgp", GnuPG "gpg" komut satırı uygulamasını sunar.

OpenGPG ve diğer tüm implementasyonlar sadece şifreleme ile değil aynı zamanda sıkıştırma, digital imza gibi ek özellikler de sunar.

# FTP
dosya transferi için kullanılan özel bir protokol. bunu kullanan birçok farklı server ve client uygulaması var.

"FTP" komut satırı client için örnek. server içinse "vsftpd" komut satırı uygulaması mevcut.

# FTPS (or FTPES or FTP-SSL or S-FTP or FTP Secure or FTP over SSL)
FTP protokolünün SSL/TSL ile birlikte kullanımı için geliştirilen protokol.

# SFTP (or SSH file transfer protocol)
SSH yapısı ile tamamiyle bütünleşik FTP protokolüdür. arkaplanda kendisi SSH bağlantısı açıp, bağımsız olarak o bağlantı üzerinden FTP ile dosya yollama işlemi yapar.

# Simple File Transfer Protocol
Kısaltması SFTP olduğu için "SSH file transfer protocol" ile karıştırılmaktadır. Oysa bağımsız farklı protokollerdir. İki protokolünde RFC'leri de birbirinden tamamen ayrıdır.

# scp (or secure copy)
özel bir dosya transferi protokolüdür.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Lightweight Directory Access Protocol (or LDAP)
Uzak ve birbirine direk bağlı ağlarda bilgisayarlar arası paylaşım/izin gibi yetkilerin sorgulanmasını sağlayan açık kaynaklı protokoldür. Protokolün kullanılabilmesi için sunucu ve her bilgisayarda client yazılımı olması gereklidir. Bu protokolü destekleyen sunucular:

- Microsoft Active Directory
- OpenLDAP
- Apache Directory Server
- Red Hat Directory Service
- ve daha birçoğu...

LDAP sadece client ve server arasındaki haberleşme protokolüdür. Client veya serveR'ı çalışma mantığı hakkında hiçbir standart içermez.

Client sunucuya query atıp yetkisi dahilindeki user'ları, grupları, user detaylarını, hangi kaynaklara erişebildiğini sorgulayabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Surface web (or Visible Web or Indexed Web or Indexable Web or Lightnet)
Internette arama motorlarına kayıtlı siteler.

# Derin ağ (or derin internet or deep web or invisible web or hidden web)
Arama motorlarının index'lenmediği bir web sitesi, derin internet'teki bir site olmuş oluyor. bunun için özel ek bir işlem yapmaya gerek yok. site yetkilileri arama motorlarına index'lemeleri için başvuru yapmazlarsa index'lenmezler. bu tarz siteler genelde özel amaçlar yada herkes tarafından görünmemek için tercih ediliyor. URL'yi bilen adrese girebiliyor.

# dark web vs dark net (or darknet or dark network)
"network vs __web (or World Wide Web or web)__" sorusu ile aynı cevabı vardır.

Firefox gibi tarayıcılarla sitelerde gezinmemizi sağlayan üst seviyeli teknoloji kümelerini "WWW" kapsar. İlk web tarayıcısının resmi ismi WorldWideWeb'di.

"network" ise; bir teknoloji kümesini kapsamaz. İletişime giren herhangi bir cihaz network yapıyor ibaresi kullanılır.

"Internet" ise; WWW'in de çalışabilmesini sağlayan DNS gibi daha çok network yapısını oluşturan seviyedeki teknolojileri kapsar.

# TOR (or The Onion Router)
Anonim iletişim için gerekli ağ ve bu ağa bağlanmak için ilgili yazılımları geliştiren proje adıdır. Ağın ayakta kalabilmesi için sunucu kaynakları gerekli. burada TOR yazılımları kuran kullanıcılar, isterse birer TOR ağı olabiliyor. TOR ağı protokolleri log takibini zorlaştıracak şekilde tasarlanmaktadır. mutlak gizliliği sağlamamaktadır. proje açık kaynaklıdır. Ağ'a bağlanmak için gerekli tool'lar ve protokoller OSI network katmanında en üst seviyede (Application layer) çalışmaktadır. TOR network routing'leri ile gidilebilecek siteler mevcut. bu sitelere erişmek için sadece TOR kullanmak gereklidir. ".onion" uzantılı siteler mevcuttur.

# TOR Browser
TOR project'in resmi masaüstü tarayıcısı. Firefox türevi.

# Orbot
Android için uygulamaların TOR network'üne bağlanmasını sağlayan uygulama. Android'in yeni gelen VPN özelliği sayesinde, her uygulamanın ayrı ayrı TOR'dan çıkıp çıkmayacağı ayarlanabiliyor.

# Orweb
TOR project'in resmi Android tarayıcısı. varsayılan Android stock tarayıcı türevi. fakat Orfox'un stabilleşmesi üzerine artık marketten kaldırılmıştır.

# Orfox
TOR project'in resmi Android tarayıcı versiyonu. Firefox türevi.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Round-trip delay (or RTT or RTD or round-trip time)
round-trip kelime anlamı: gidiş dönüş.

network'te karşıya istek atıldığında dönüş beklenir. bu dönüş ve ACK'ların olduğu süreçleri kapsayan tüm süreye RTT denir.

Örneğin HTTPS isteği yaptığımızda; TLS 1.2 kullanılıyorsa; web browser ilk defa siteye gidiyorsa;
- DNS için 1 round-trip
- TCP handshake için 1 round-trip
- SSL handshake için 2 round-trip
- HTTPS için atılan istekte (eğer data-stream yok ise veya payload'ın boyutu çok büyük değil ise) 1 round-trip

Yukarıdaki round-trip sayıları SSL'in versiyonuna ve local DNS cache'teki durumumuza ve siteye ilk defamı gidip gitmediğimize göre değişmektedir (Çünkü HTTP client veya web browser, ikinci defa aynı siteye gittiğimizde aynı TCP soket bağlantısını kullanabilmektedir).

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# jump server (or jump host or jump box)
farklı bir network ağına bağlanabilmek için kullanılan sunucu makinedir. bu makine sadece diğer network'e erişebilmek için bir geçit olarak kullanılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •