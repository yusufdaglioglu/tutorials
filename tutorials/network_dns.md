############################

############################
# NETWORK DNS
############################

############################


# DNS (or Domain Name System)
internet adres resolving sistemini temsil eden genel bir terimdir.

__DNS Server (or Name Server)__ ise IP'leri resolving yapmakla yükümlü yazılımdır. Bazen __Domain Name Server__ denir fakat yanlış bir kullanımdır.

# International Corporation for Assigned Names and Numbers (or ICANN)
2019 yılı itibariyle hala en yetkili kuruluştur. Amerika'ya bağlıdır. __registrar (or kaydedici)__ firmalar (örnek: Godaddy), ICANN'dan lisans almak zorundalar.

# IANA (or Internet Assigned Numbers Authority)
ICANN için gerekli bazı teknik konuları tamamlayan kuruluştur.

# RIR (yad Regional Internet Registry)
ICANN ve IANA'nın altında dünyanın 5 farklı bölgesinde yetkilendirilmiş kuruluşlar vardır. Bu kuruluşlar kendi bölgelerinden sorumludur ve devlete bağlı kuruluş değillerdir.

- AfriNIC

  African Region Internet Registry: Afrika

- APNIC

  Asia Pacific Network Information Centre: Asya ve Pasifik

- ARIN

  American Registry for Internet Numbers: Kuzey Amerika

- LACNIC

  Latin America and Caribbean Internet Addresses Registry: Latin Amerika ve Karayipler bölgesi

- RIPE NCC

  Reseaux IP Europeens Network Coordination Centre: Avrupa bölgesi.

RIR alanları yukarıda yazıldığı kadar keskin çizgilerle ayrılmaz. Örneğin, ARIN bölgesi esas olarak Kuzey Amerika olmakla beraber Güney Amerika ve Afrika'ya da servis verir. RIPE bölgesi esas olarak avrupa olmakla beraber Orta Doğu ve Orta Asya içlerine kadar servis vermektedir. Türkiye, RIPE NCC bölgesindedir. RIPE NCC'nin merkezi Hollanda'dadır.

# LIR (or Local Internet Registry)
kullanıcıya IP veren kuruluşlardır. ISP (or Internet Sevice Provider) (Türktelekom, Superonline...)'lere denk gelir. Yasal anlamda şirketi olan herkes bu görevi alabilir. Türkiye'de u servisi sağlayan 100'den fazla kuruluş vardır.

# Web tarayıcının DNS sorgusu
sırası ile:
- web browser DNS cache
- OS DNS cache
- "hosts" file
- __public DNS (or non-authoritative DNS server or recursive DNS server or recursive DNS resolver)__

  örnek Google DNS 8.8.8.8, 8.8.4.4(Google ikinci yedek IP), Cloudflare 1.1.1.1 gibi... Eğer public DNS tanımlamazsak ISP bizim yerimize default değeri atar.

  public DNS kendi içinde bilgiyi bulamaz ise, kendi gidip root DNS server'a sorar.

# hierarchy of DNS system

# 1- root DNS servers
  en üstte olan ve en yetkili sunuculardır.

  DNS sunucuları tüm domain bilgilerini saklayamaz. local cache'inde yok ise, ilk önce root server'lara sorar. root server bu bilgiyi lokalinde bulundurmuyorsa 2inci seviye root server'ın adresini sorgulayan client'a döndürür.

  root server'lar dünyada 12 adettir (yıl 2019). bunlar ICANN tarafından belirlenmiştir. fakat sadece 1 tanesi ICANN kuruluşa direk bağlı bir sunucudur. bu sunucuya __ICANN Managed Root Server (or IMRS)__ deniyor. diğerlerinin ICANN'a bağlı olmamasının sebebi, merkezi yönetimden kaçınmaktır.

  ICANN'ın resmi sayfası olan www.dns.icann.org sitesinde root-servers.org'a referans verilmiştir. buradan 12 server'ı görebiliriz.

  12 adet Root DNS server var, fakat bunlarda farklı instance'larla kendilerini çoğaltmışlardır. Yine bu instance'larda root-servers.org sayfasında görülebilir.

  2inci seviye DNS sunucu bilgileri tutar.

# 2- top-level DNS server
  2inci seviye DNS server'lardır. her "top-level DNS server" ".com", ".org" gibi sorumluluk alanları vardır.

# 3- authoritative DNS server
  3üncü seviye DNS server'lardır.

# 4-
  DNS sisteminde 4üncü seviye yoktur. Bu seviyeye istersek public DNS'lerimizi koyabiliriz.

# whois server
registrar'a yeni bir domain eklendiğinde, registrar bu bilgiyi o domain'in uzantısının sorumlusu olan "whois server"'a yollar. örneğin .com ve .Net alan isimleri için verisign şirketinin whois sunucusu merkezi sunucudur. daha sonra bu bilgi diğer üçüncü parti whois server'lar tarafından klonlanır.

Dolayısı ile; root server'lar dahi .com ve .Net için gerekli tüm domain'leri lokallerinde tutmazlar.

Verisign firması ICANN'a bağlıdır ve ICANN tarafından yetkilendirilmiştir.

ICANN'dan her firma Godaddy gibi kolayca yetki alamaz. fakat Godaddy'ye kolayca aracılık edebilir, yani Godaddy'nin bayisi olabilir.

# hostname vs domain name
domain name bir bilgisayara DNS sunucusu tarafından atanmış string'dir. oysa hostname local bir network'te her makinanın kendisini dışarıya tanıttığı isimdir. network içinde hostname kullanılırken, dışarıya gidişlerde domain kullanılır. dışarıdaki bir yere giderken, oradaki network'teki bir alt makineye erişmek istediğimizde hostname.domainname.com gibi bir adresten yararlanılır. hostname ve domain name aynı görevi görür. www internet tarayıcıları için gerekli varsayılan makinanın hostname'idir.

# subdomain
mail.Google.com, map.Google.com'deki mail ve map birer subdomain'dir. ayrı IP'leri vardır. fakat istenirse aynı IP'ye de yönlendirilebilir.

# domain'i hosting'e bağlama
wix.com bizim sayfamızı barındırıyor (sunucu hosting firmamız) olsun. godaddy.com'dan da domain'imizi almış olalım.

- wix.com'a domain-name'imizi bildiriyoruz.
- wix.com bize wix'in kendi DNS'lerini veriyor. bunlar genelde ns1.wix-dns.com tarzında URL'ler oluyor.
- Godaddy'de o domain'e ait nameserver'larımızı tanımlıyoruz. bunun için Godaddy'ye login oluyor, ilgili domain'imizi seçiyor, ayarlarına giriyor ve nameserver alanlarımızı dolduruyoruz. burada birden fazla nameserver olabilir. çünkü yedek nameserver olabiliyor.

Artık domain'imize istek yapıldığında, Godaddy DNS sorgusunu yapanı ns1.wix-dns.com'a yönlendiriyor. ns1.wix-dns.com gelen sorguyu görüyor ve onu sunucu bilgisayarın IP'sini dönüyor.

Bu örnekte Godaddy'ye verdiğimiz DNS sunucuları bizim kendi kişisel sunucularımız olabilir. Genelde büyük firmalar kendi DNS sunucularını kullanıyor.

# fully qualified domain name (or FQDN)
hostname + domain name birarada olduğunda verilen isimdir. örnek: tr.wikipedia.com, www.wikipedia.com...

# top-level domain (or TLD)
.com, .org, tr gibi terimlere verilen isimdir. TLD'nin çeşitleri vardır:

- # country code top-level domain (ccTLD)
  .tr, .fr, .de, .us gibi ülke bazlı gruplama için gerekli domain bilgisini ifade eder.

- # generic top-level domains (or gTLD)
  .com, .org gibi kurumların tipini belli eden domain ifadeleridir.

# domain levels
example: mail.Google.com

- 1st level domain (or first-level domain)

  com

- 2nd level domain (or second-Level domain)

  Google

- 3rd level domain (or third-level domain)

  mail

# Ters DNS Çözümlemesi (or Reverse DNS lookup or reverse DNS resolution or rDNS)
  IP'den DNS bulma işlemidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DNS over TLS (DoT) vs DNS over HTTPS (DoH)
Standart DNS client'lar, DoT ve DoH sunucularına istek yapamazlar. çünkü farklı protokollerdir.

standart DNS'te isteğin çoğu açık gider/gelir. oysa DoT ve DoH 'ta istek tamamen şifrelenmiştir.

DoT, TCP soket bağlantısı açıp, o soketi üzerinden TLS ile DNS sorgusu atar. DoH ise HTTP protokolünden yararlanarak DNS isteği atar.

Bu iki ptotokolün amacı ISP'lerden gizlenmek değildir. çünkü ISP, DNS sorgusundan sonra, gideceğimiz IP adresini zaten görecektir. buradaki amaç;

- man-in-the-middle (or MITM) attack'ı önlemek
- DNS sorgusu yapılıp, cevabı şifreli bir bağlantıda kullanılacak ise güvenliği sağlamış oluruz
- ISP'lerin DNS sunucularındaki yaptığı engellemeleri ortadan kaldırmak

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •