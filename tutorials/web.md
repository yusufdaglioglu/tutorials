############################

############################
# WEB
############################

############################

# Web browser User Agent

user agent header'da otomatik platform tarafından gönderilir. sadece browser değil, her client user-agent atmalıdır.

formatı incelersek; Çoğu "Mozilla/" ile başlar. ilk olarak bu isim verilmişti. daha sonra herkes devam ettirdi. yanındaki ilk numara format sürümüdür.

User agent içerisinde aşağıdakiler sürüm bilgileri ile yazar:

- tarayıcıda bazı kurulu eklentilerin listesini

- layout engine ismi

- tarayıcı ismi

- işletim sistemi ismi

- işletim sistemi çekirdek ismi

- Microsoft Windows için kurulu .Net ve service pack sürümleri

User agent dışında Javascript tarafında alınan bilgiler:

ekran çözünürlüğü, tarayıcı pencere boyutu, local saat, cookie ve Javascript'in açık olmadığı bilgisi.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Web Worker
Tarayıcılarda thread çalıştırmak için kullanılan JS objesidir. 

```js
 // it starts automatically. there is no any run method:
var w = new Worker("demo_worker_file.js");

// the communucation is supporting via events:
w.onmessage = function(event){
   console.log( event.data );
};

// terminate the worker:
w.terminate();
```

"async" veya "setTimeout" ile karşılaştırma yaparsak;
- "async" veya "setTimeout" ile başlattığımız thread'i terminate edemeyiz.
- "async" veya "setTimeout" ile başlatılan aslıda main thread bittikten sonra veya öncesinde çağrılıyor. Yani her türlü main thread'i engeller. Fakat WebWorker her türlü kesinlikle paralel çalışır. Gerçekten de 2inci bir OS level thread açılır. 

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Ajax (or Asynchronous JavaScript and XML)
web sayfasında reloading yapmadan sunucuya XML istekleri yaparak sayfayı yenileme tekniğidir. İlk çıktığında sadece Web Browser'larda kullanılması üzere bu terim ortaya atıldı.

# XMLHttpRequest (or XHR)
Core Javascript'te Ajax yollamaya yarayan objenin ismidir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# origin
kelime anlamı: menşei, köken

web dünyasında origin protokol, domain, host (subdomain olarakta adlandırılır), port dörtlüsünün bir arada temsil etmektedir. örneğin; iki URL değerimiz olsun. ikisi aynı origin olabilmesi için bu 4 bilgininde aynı olması şarttır.

path değeri önemsizdir.

Sadece Microsoft internet explorer'da:

- "Trust Zones" olarak tanımlı domain'lere
- port bilgisi farklı olan URL'lere

istisna tanınıp aynı origin'de oldukları varsayılır. bu her sürümde var mı bilmiyorum. kaynak: (source-id: 193)

# Cross origin domain policy

Web tarayıcılarının başka domain'lere istek yapılmasını güvenlik için engellemektedirler. Cross origin domain policy bu kıstası kapsamaktadır. web sitesinin bunu aşmasının birkaç yolu vardır:
- JSONP (or JSON with Padding)
- Adobe flash gibi eklentiler
- CORS

# Cross-Origin Resource Sharing (or CORS)

Modern tarayıcılar tarafından cross site script'in desteklenmesi standartlarıdır. Farklı bir domain'e istek yapılacağı zaman tarayıcı anında (artık - CORS destekleyen tarayıcılar) bloklama yapmaz. bunun yerine eğer;

- istek "basit istek" değil ise Preflight isteği yapılıyor.

- istek "basit istek" ise isteğe direk izin veriliyor.

tarayıcı arkaplanda; web sayfasının istek yapmak istedikleri adrese, istek tipini ve web sayfasının o andaki adresi gibi bilgileri gönderir. bu istek "option" metotu tipinde bir XHR'dır ve ismi "Preflight"'tır. sunucu bu isteğe cevap olarak tarayıcıya asıl isteği gönderip gönderemeyeceği bilgisini verir. eğer sunucu izin verirse web sayfasının Ajax işlemi gerçekleştirilir. burada bir nebzede olsa güvenlik sağlanmış olmaktadır. çünkü Preflight request'i web sayfası değil, tarayıcı tarafından hazırlanmaktadır. standartlar web sayfalarını tamamiyle serbest bırakmak yerine, bir nebze daha güvenli olacağı düşünülmüş bu taktiği uygulamaktadır. JSONP yönteminden daha modern bir yöntemdir.

CORS isteği hata verirse detaylarını JS tarafından programatik olarak alamayız. Bu güvenlik sebebiyle engellenmiştir.

# Basit istek (or simple request)

Eğer bir XHR isteği;

- GET veya HEAD veya POST ise

- Tarayıcıların default olarak kendi eklediği header'lar (örnek user-agent) dışında sadece bazı header'lar kabul edilir.

- İzin verilen header'ların bazılarında sadece bazı alanların olmasına izin verilir. örnek:

  Content-type header'ında sadece şu 3'ü kabul edilir:
  - application/x-www-form-urlencoded
  - multipart/form-data
  - text/plain

- Ek bazı kurallar daha vardır. kaynak: (source-id: 194) "Simple requests" başlığı.

# JSON vs JSONP
Ajax isteği ile ancak aynı domain'e istek yapıp cevap alabiliriz. farklı domain'lere yapılan istekler tarayıcılar tarafından güvenlik sebebi ile engellenir.

JSONP formatı; JSON formatındaki bir değerin bir metot içerisinde çağrılacak şekilde formatlanmış halidir.

örneğin;

JSON:
> { isim: ahmet }

JSONP:
> metotName ( { isim: ahmet } )

HTML sayfasında include edilen kütüphanelerin (script, CSS etc) tag'lerindeki URL'lerde cross domain'e izin verilir. İşte bu noktada JSONP ie bir açıktan yararlanılır:

JSONP ile istek yaparken HTTP isteği isteği yapılmaz. Sayfaya script include edilir. Bu include edilecek değer ise sunucudan gelecek olan response'tur.

```js
var elm = document.createElement("script");

elm.setAttribute("type", "text/javascript");

elm.src = "http://example.com/jsonp";

document.body.appendChild(elm);
```

Burada dönen cevap bir metotu çağırır. metot bizim Javascript tarafında tanımlı olduğundan ona veri olarak parametre gönderir. İşte tüm bunları simüle eden bir fonksiyon yapıp HTTP Request-Ajax işlemi yaparmış gibi cross domain kuralını aşarak işlem yapabilir.

# JSONP vs CORS
Cors web tarayıcıları tarafından önerilen bir yöntemdir.

JSONP'nin dezavantajları:

(tüm dezavantajlar \<src\> eklenip yapılmasından kaynaklıdır.)

- sadece get isteği yapılabilir
- exception handling mekanizması Ajax request yönetimine göre daha kötüdür. sadece request'te hata olup olmadığını görebiliriz.
- Header'ları seçip yollayamayız yada gelene header'ları göremeyiz

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Do Not Track (or DNT)

web siteleri kullanıcılardan istatistik toplar. web tarayıcıları HTTP header'larına, kullanıcın bilgi toplanmasını istemediğini yada istediğine dair bir metadata ekler. sitede buna göre davranır. yasal süreçlerin getirmiş olduğu bir zorunluluktur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Google Chrome os vs Chromium os

- embedded Adobe flash

- auto updater

- donanıma özgü performans paketleri içerir

- resmi destek Google ve/veya donanım üreticisi tarafından verilir. Chromium için topluluklar ile temasta olmak gerekir.

- Chromium browser (look at: Chromium browser vs Google Chrome browser )

- Name and logo tradememarks and licence

- ve daha fazlası

# Chromium browser vs Google Chrome browser

- default installed extensions

- embedded auto updater

- embedded Adobe flash (Linux paketinde de geliyor)

- Media codecs to support H.264, AAC and MP3 formats

- Name and logo tradememarks and licence

- isteğe bağlı olarak birçok bilginin istatistik olarak Google'a gönderilmesi seçeneği (Chromium üzerinde bulunan diğer hizmetlerde de bilgi gönderiliyor. detaylar farklı başlıkta yazıyor)

- resmi destek Google tarafından verilir. Chromium için topluluklar ile temasta olmak gerekir.

- ve daha fazlası

# notlar

- "built-in PDF viewer" and "print preview" future added both browsers after Chromium 47 version.

- Chromium has no release tags inside code review system. there is no way to find stable versions. açık kaynağı destekleyen topluluklar, kodun stabil olduğunu düşündükleri bir noktadan, Debian tabanlı sistemler için paket çıkmaktadırlar. şimdilik en stabil versiyonu bu.

- HTML ve Javascript derleyicilerin de bir farklılık yok. fakat buna rağmen Chromium ile teknik bir karşılaştırma yapmak mümkün değil, çünkü Chromium için aynı noktadan paket çıkılmış versiyon bulmak pratikte neredeyse imkansız.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# URL Safety
Below functions are embedded to all browsers as default.

```js
// Replace unsafe URL characters with %01 or %02...
// It accepts full URL, therefore it does not convert / ? characters.
var urlSafe = encodeURI("http://www.Google.com");
decodeURI(urlSafe); // returns real URL which includes unsafe characters.

// encodeURIComponent function replace all unsafe characters with %01 or %02...
// It is different from encodeURI. Because encodeURIComponent also converts / ? characters. We can use this function to make safe a part/component of URL. For example: query param, query value, path...
var safeStr = encodeURIComponent("This string has unsafe characters");
decodeURIComponent(safeStr);
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# WebAssembly (or Wasm)
web tarayıcılarında sunucudan gönderilen C/C++ gibi native code'lar çalıştırılıyor. bu kod güvenlik amaçlı sandbox'ta çalıştırılıyor ve yetkileri çok kısıtlı. JS gibi sadece belli kaynaklara erişebiliyor (cookies, DOM...).

örnek kod:

```java
var wasmModule = new WebAssembly.Module(wasmCode);
var wasmInstance = new WebAssembly.Instance(wasmModule, wasmImports);
wasmInstance.exports.factorial(10); // native taraftaki faktoriyel alan fonksiyonumuzu çağırdık.
```

örneğin; client tarafta video düzenleme uygulaması yaptık. bu gibi durumlarda bu teknoloji performans'ı çok etkiliyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# postback

web sayfasında forma tıklanıp, aynı sayfaya istek yapılan istek "postback" işlemidir. "POSTed back" to same page teriminden gelir. genelde JSP, JSF, ASP gibi platformlarda karşımıza çıkar, fakat teknolojiden bağımsızdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# REST Resource Naming
- URL bir identifier olduğu unutulmamalıdır

bu sebeple fiil içermemelidir. fiil içermemelidir.
örnek:

> /API/message/3/delete/

yerine

> /API/message/3

  DELETE metotu desteği olmalıdır

- tekil kaynak çağırma

  > /user-management/users/{id}

  > /user-management/users/admin

- çoğul kaynak çağırma

  > /device-management/managed-devices

  > /user-management/users

  > /user-management/users/{id}/accounts

  Tekil ve çoğul kaynaklarda "users" yada "user" yazılıp yazılmayacağı önemsizdir. fakat tüm servislerimizde ya çoğul yada tekil olan kelimeleri kullanmalıyız.

- fiil içerme durumları

  yapılacak işlem bir prosedürü başlatacak ise; fiil kullanılmak zorundayız. örnek:

  > /cart-management/users/{id}/cart/checkout

- geçerli olmayan standartlar

  Google, Facebook, Dropbox, Instagram gibi büyük servislerin API'leri aşağıdaki standartlar konusunda farklı yol izlemektedirler. Hatta aynı şirketin farklı servisleri farklı yöntemler kullanmaktadır.

  - okunabilirlik
    - userManagement
    - user-management
    - user-management
    - usermanagement

  - büyük küçük harf standartları

    rfc2616 standartlarına göre ( w3.org/Protocols/rfc2616/rfc2616-sec3.html#sec3.2.3 ) büyük küçük harf hostname'e kadar önemsiz iken, path kısmında önemlidir.

  - versioning

    a release number on:

    - In the path, at the beginning or at the end of the URI
    - As a parameter of the request's body
    - In a HTTP Header
    - With an optional (example: /API/users/1?v=2 )

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# placeholder
Türkçe kelime anlamı: yer tutucu

bazı HTML form elementleri için bir attribute'dir. bu attribute ile son kullanıcı text girmemiş ise, placeholder value'sinde yazan string değeri ekranda gösterilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# carousel
Türkçe'de kelime anlamı: atlıkarınca

HTML'de sağ ve sola doğru oklar aracılığı ile resim galerisi gezintisi sağlayan component'e verilen özel isimdir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Favicon
Favorites icon'un kısaltmasıdır. bu resim dosyası, sitenin sık kullanılanlara eklendiğinde, veya siteye gidildiğinde görünecek ufak simgesidir. HTML kodları içerisinde yer alır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# access inside HTML popup with pure (core) js

```js
var myWindow = window.open("", "myWindow", "width=200, height=100");
myWindow.document.write("hello I'm popup");

//or get the opener object
var opener = window.opener;

if(opener) {
    var oDom = opener.document;
    var elem = oDom.getElementById("elementId");
    if (elem) {
        var val = elem.value;
    }
}
```

HTML, aynı anda birden fazla popup desteklemektedir. fakat çoğu tarayıcı varsayılan olarak ikinci açılan popup'ları engellemektedir. bu sebeple açılması tavsiye edilmez.

# Backlink
Backlink, bir web sayfasından başka bir web sitesine giden köprü bağlantıya verilen isimdir. Bir web sitesine, diğer sitelerden verilen backlink ne kadar fazla olursa, bu web sitesi arama motorları açısından o denli popüler olur. burada önemli kriter: X sitesi, Y sitesine link veriyorsa, X'teki sayfanın içeriğinin Y'deki ile bağlantılı/alakalı olmasıdır. ilgisiz konulardaki linkler verimsiz olacağından arama motorlarındaki puanı olumsuz etkiler.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Tampermonkey vs Greasemonkey vs Violentmonkey

web tarayıcıları için birbirlerine alternatif eklentilerdir. Eklentiler sayesinde son kullanıcı Javascript'leri, istediği event (sadece standart Javascript event'leri değil, ekstra event'ler de kullanılabiliyor) sonrası çalıştırabiliyor. İnternette birçok hazır script paylaşılıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Jqplot
JQuery eklentisi. sadece chart oluşturmaya yarıyor.

# chart
grafiğin bir alt kümesidir. sadece değişkenler arasındaki ilişkiyi göstermeye yarayan çizgisel anlatım şekli.

çok kullanılan bazı chart çeşitleri:

- Line chart (or Çizelge): x-y ekseni üzerinde bir fonksiyonun gösterilmesidir.

- bar chart: birden fazla kalın çubukların yanyana dizilerek x-y ekseni üzerinde gösterilmesi.

- pie chart (or a circle chart): pasta üzerinde oranların paylaştırıldığı grafik

- histogram: gruplandırılarak gösterilen bir "Bar chart" çeşididir. örnek olarak aşağıdaki bilgiyi bar chart'ta gösterdiğimizi düşünelim:

| BOY (CM) GRUPLAR | KİŞİ SAYISI |
|------------------|-------------|
| 161-165          | 5           |
| 166-170          | 10          |
| 171-175          | 9           |

# diagram (or tr:diyagram)
grafiğin bir alt kümesidir. sadece herhangi bir olayın değişimini gösteren grafik. UML diyagramları buna örnektir.

# Cartogram (or kartogram)
grafiğin bir alt kümesidir. istatistiki bilgilerin harita üzerinde gösterildiği grafiktir.

# indicator (or indikatör)
gösterge anlamına gelmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Web Debugging Proxy Application

Yazılımlar network üzerinden Ajax işlemi yaparlar. Bu Ajax'ları intercept edip devam ettirmek ve manipüle etmek için aracı programlardan yararlanılır. Bu programlara genelde "Web Debugging Proxy Application" uygulamaları denir.

tüm network trafiğini intercept eden uygulamalar:
- Fiddler
- Charles
- Mitmproxy (açık kaynaklı): ismi man-in-the-middle-proxy kısaltmasından gelmektedir.
- Tamperchrome: Chrome için Google'nin geliştirdiği eklenti. sadece Google Chrome üzerinden çıkan Ajax işlemlerini intercept edebiliyor.

# Temel çalışma mantığı

"Web Debugging Proxy Application" aslında bir proxy uygulaması. bu sebeple örneğin; Firefox uygulamasından gidecek Ajax istekleri incelenecek ise; Firefox'un proxy ayarlarına "Web Debugging Proxy Application" proxy sunucusunun açtığı IP:port bilgisi veriliyor. artık Firefox'un yaptığı istekler "Web Debugging Proxy Application" GUI ekranında listelenmeye başlıyor.

# localhost problemi
lokalde development yaparken "Web Debugging Proxy Application" uygulamaları localhost'a yada 127.0.0.1'e yapılan istekleri yakalayamayabilir. bu gibi durumlarda localhost yerine makine-name'mizi kullanabiliriz. çünkü localhost direk loopback arayüzüne yönlendirilirken, makine-name'mimiz network üzerinden sorgulamaya çalışılıyor.

# Web Debugging Proxy Application'lerin SSL bağlantılarını izlemesi
normal koşullarda SSL bağlantısını kimse takip edemez. fakat, Proxy uygulamamız kendi "certificate Authority" sertifikasını işletim sistemine yada takip edeceğimiz client'a (Java app, web browser...) kurdurtuyor. Client isteği proxy'ye atıyor. proxy o anda, sisteme tanıtılmış olan certificate Authority'ye uygun bir sertifika üretiyor. proxy, isteği sunucuya kendi atıyor(yönlendirmiyor). dönen cevabı da kendi sertifikası ile imzalayıp döndürüyor. yani proxy, gerçek isteği yönlendirilmiyor. proxy arada sahtecilik yapıyor.

burada bazı sorular/sıkıntılar var. çözümleri ile birlikte açıklamak gerekirse:

- web tarayıcı DNS sorgusu sonrası sunucuya IP ile gidiyor. dolayısı ile HTTP isteği IP ile bağlantı kurulup yapılıyor. yani proxy hangi domain'e gitmek istediğini bilemiyor. bu sebeple nasıl o domain'e uygun sertifikayı o anda üretiyor?

  Proxy IP'ye aynı anda bir request atıp handshake işlemi yapıyor. proxy o IP'li sunucunun sertifikasına bakıyor. sertifikasında olan "Common Name" bilgisi zaten domain bilgisini vermektedir.

- bazı sertifikalar birden fazla domain bilgisi içerebiliyor. bu gibi durumda, client'ın hangi domain'e gitmek istediğini proxy nereden bilebilir?

  Proxy sunucudan indirdiği SSL sertifikasındaki "Subject Alternative Name" değerine bakıyor. burada birden fazla domain ismi oluyor. proxy o anda tüm bu domain'lere uygun bir sertifika üretiyor ve client ile bununla haberleşiyor.

şirketler çalışanlarının HTTPS bağlantıları bu yöntemle takip etmektedirler. her işletim sistemine certificate Authority tanıtmaktadırlar.

eğer bir kullanıcının TCP ile iletişim kurulacak yazılımına "certificate Authority" kurulmamış ise; tüm network trafiği izlense de kullanıcının bilgileri görünemez. çünkü client taraf, daha ilk mesajından itibaren sunucunun private key'i ile şifreli şekilde data'ları yolluyor. şirketler "certificate Authority" sertifikasını tüm web tarayıcılarına ve işletim sistemine baştan root yetkisi ile kuruyor ve kimseye sildirtmiyor. bu sahte "certificate Authority" sayesinde şirket aradaki tüm bağlantıları açık şekilde izleyebiliyor.

peki biz portable bir Firefox kurup, işletim sisteminden sertifikaları algılamamasına göre konfigüre edersek ne olacak? bu sefer bağlantıyı şirket takip edemeyecek fakat bağlantıda giden gelen data'lar anlamsızlaşacak ve iletişim kurulamayacak.

# Mitmproxy modes
Mitmproxy birden fazla mode ile çalışabilir. bazıları:

- Socks proxy

  SOCKS bir çeşit proxy türüdür. Birçok sürümü vardır: 4, 4a, 5...

  HTTP-proxy'ler sadece HTTP isteklerini karşılarken, SOCKS-proxy daha alt seviyeli birçok bağlantı çeşidini destekliyor: örnek TCP.

  Fakat HTTPS şifreli bağlantı olduğu için HTTP-Proxy'lerin, TCP-proxy'ler gibi davranmaları gerekiyor. Bu sebeple artık HTTP-proxy ve TCP-proxy yazılımları çok büyük benzerlik gösteriyor. HTTP-proxy aracılığı ile HTTPS bağlantı kurabilmek için HTTP'nin CONNECT metodu kullanılır (ilgili başlıktaki detayları okuyun).

- Reverse proxy

  sunucu tarafta (datacenter'ında) olan proxy'dir. target sunucularının önünde durur.

  reverse proxy'lere bazı kaynaklarda "gateway" de denir. kaynak: (source-id: 195) "Forward Proxies and Reverse Proxies/Gateways" başlığı, 1inci paragraf.

  Reverse proxy şu gibi amaçlarla kullanılır:

  - server'a gelen gideni log'lamak
  - myservice.mycompany.com'da bir Java server yazılımı çalışıyor olsun. Geçici olarak myservice2.mycompany.com'da bir Java server'ı devreye almak istiyoruz (debug amaçlı). myservice'daki makineye mitmproxy reverse mode ile kurulur ve oraya gelen istekler iç network'ten myservice2'deki Java uygulamasına yönlendirilir.
  - local'de web development yapıyoruz. local'de host dosyasımızı güncelleyip, myservice.mycompany.com'nin localhost:8080 olduğunu belirtiyoruz. local'de mitmproxy'yi reverse mode'da 8080'i dinleyecek şekilde açıyoruz. daha sonra mitmproxy'de istekleri nereye yönlendirmek istiyorsak yönlendiriyoruz.

- regular proxy

  default mode. "__forward proxy__" olarak davranır.

  forward proxy'ler, client tarafın ortamında olan proxy'lerdir. client proxy olarak bunu kullanacağını belirtmelidir/bilmelidir.

- Transparent proxy

  önce router'a giden client, daha sonra router tarafından Mitmproxy'ye yönlendirilir. daha sonra Mitmproxy default mode'daki gibi gelen istekleri yönlendirir. (aşağıdaki başlıkta daha detaylı anlatılıyor)

- Upstream: birden fazla proxy zinciri varsa, Mitmproxy bu zincirdeki bir proxy olarak çalışabilir.

# Transparent Proxying (or intercepting proxy or inline proxy or forced proxy)
Transparent: saydam, şeffaf, transparan

2 farklı anlamı vardır:

- network katmanında kullanıcı hiçbir ayar yapmasına gerek kalmadan yapılan proxy işlemidir.

- rfc2616 (source-id: 214) standardında belirtildiği gibi request ve response'u hiç değiştirmeyen proxy'lere denir. __non-transparent proxy__ ise request veya response üzerinde bazen yada her zaman değişiklik yapan proxy'lerdir.

Mitmproxy, Transparent mode'a ayarlandığında, aşağıdakilerden sadece bir tanesi yapılmalıdır;
- spoof edilecek cihazın network ayalarında gateway olarak Mitmproxy'nin çalıştığı bilgisayarın IP'si verilmelidir. yani gateway = Mitmproxy olacaktır. (bu durumda rooter'ın ayar yapmasına gerek kalmaz)
- router'ımızda Mitmproxy konfigürasyonu yapılmalıdır. bu ayar ile client, önce router'a gitmekte, router istekleri ise Mitmproxy'nin olduğu sunuya yönlendirmektedir. (bu durumda client'ın ayar yapmasına gerek kalmaz) (kaynak: (source-id: 440) "Transparent Proxy" başlığı, 1inci cümle.)

yukarıda 2 farklı çözüm ile Transparent modun çalışabileceği için kaynak: (source-id: 440) "Common Configurations" başlığı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# bookmarklet

web tarayıcıları URL'den Javascript çalıştırılmasına izin verir. bu sebeple birçok hazır JS sık kullanılanlara eklenerek tarayıcıya ek işlevler kazandırılır. bu uygulamacıklara bookmarklet denir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DOM Events
aşağıda bazı event ve bu event'lerin bazı grupları listelenmiştir.

- #### mouse events:

  - oncontextmenu : user right clicks to an object

  - ondblclick : on double click

  - onmousemove :bir obje üzerinde fare hareket ettiği zaman

  - onmouseenter : fare elementin üzerine geldiğinde

  - onmouseover : onmouseenter ilgili obje içerisindeki diğer objeleri kapsamazken, onmouseover alt objeleri de kapsıyor.

  - onmouseleave : fare objenin dışında çıktığında

  - onmouseout : onmouseleave'in alt objeleri de kapsadığı durum

- #### key events

  klavyeden bir tuşa basıldığında sırası ile bu event'ler çağrılıyor: keydown, (bu sırada text ekranda yazılmış görünüyor), keypress, keyup

  keypress metotu CTRL+V gibi işlemleri yakalamazken,  keydown bu işlemleri de yakalayabiliyor.

- #### Frame/Object Events

  aşağıdakiler her element'te geçerli olan event'ler değil. çoğu sadece body tag'inde çalışıyor.

  - onabort: The event occurs when the loading of a resource has been aborted

  - onbeforeunload: The event occurs before the document is about to be unloaded

  - onerror: The event occurs when an error occurs while loading an external file

  - onhashchange: The event occurs when there has been changes to the anchor part of a URL

  - onload: The event occurs when an object has loaded

  - onpageshow: The event occurs when the user navigates to a webpage

  - onpagehide: The event occurs when the user navigates away from a webpage

  - onresize: The event occurs when the document view is resized

  - onscroll: The event occurs when an element's scrollbar is being scrolled

  - onunload: once a page has unloaded

- #### Form Object events

  - onblur: object loses focus

  - onchange: the selection, or the checked state have changed (for input, keygen, select, and textarea)

  - onfocus: The event occurs when an element gets focus

  - oninput: keypress gibi fakat CTRL+V ile yazılanları da algılıyor.

  - oninvalid: örneğin; textbox'un boş olması

  - onreset: form reset button clicked

  - onselect: the user selects some text (for input and textarea)

  - onsubmit: a valid form's submit button clicked

- #### drag events

- #### clipboard events
  - oncopy
  - oncut
  - onpaste

- #### print events
  - onafterprint
  - onbeforeprint

- #### Media Events

- #### Animation Events

# event object
Javascript'te event callback metotuna geçilen event nesnesinin bazı property'leri ve metotları aşağıdadır.

aşağıdakilere ek olarak her event (örneğin click-event) kendi property'sini eklemektedir.

- #### property'ler

- bubbles: Returns whether or not a specific event is a bubbling event.

- cancelable: Returns whether or not an event can have its default action prevented

- currentTarget: Returns the element whose event listeners triggered the event

- defaultPrevented: Returns whether or not the preventDefault() method was called for the event

- eventPhase: Returns which phase of the event flow is currently being evaluated

- isTrusted: if user call the action or not.

- target: Returns the element that triggered the event

- timeStamp: Returns the time (in milliseconds relative to the epoch) at which the event was created

- type: Returns the name of the event

- view: Returns a reference to the Window object where the event occured

- #### metotlar

- event.preventDefault()

```html
<script>
function engelle(e){
    e.preventDefault();
}
</script>

<a href="http://www.mylink.org" onclick="engelle(event)">Tıkla</a>
```

a tag'ine tıklandığında sayfa başka yere gitmesi gerekirken gitmeyecektir. preventDefault bunu engelliyor. örneğin; keypress event'ine şu metotu yazarsak rakam haricindeki kullanıcı girişleri bir input'a yapılamayacaktır:

```js
if(e.keyCode>=58 || e.keyCode<=47)

   e.preventDefault();
```

- event.stopPropagation();
DOM'da Div içinde p, onunda içinde span elementi olsun. aşağıdaki tanımlamalar yapılmış olsun. span'a fare ile tıkladığımızda, stopPropagation diğer metotların çalışmasını engelleyecektir.

```js
$("span").click(function(event){

   event.stopPropagation();

   alert("The span element was clicked.");
});

$("p").click(function(event){

   alert("The p element was clicked.");
});

$("div").click(function(){

   alert("The div element was clicked.");
});
```

- stopImmediatePropagation()
aynı objenin click event'ine 2 tane metot atılmış olsun. diğer metotların çalışmasını engellemek için bu metot kullanılır.

- return of callback method

event metotu return false yapınca da belli anlama geliyor. aşağıdaki tabloda net görülmektedir.

|                          | stop bubbling | prevent default action | prevent event handlers        |
|--------------------------|---------------|------------------------|-------------------------------|
|                          |               |                        | Same Element / Parent Element |
| return false             | Yes           | Yes                    | No      /     No              |
| preventDefault           | No            | Yes                    | No      /     No              |
| stopPropagation          | Yes           | No                     | No      /     Yes             |
| stopImmediatePropagation | Yes           | No                     | Yes     /     ?               |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# upload file

tarayıcılardan sunucuya dosya yükleme işlemleri var. bu işlemler HTTP standartları altında yapılmaktadır. upload işlemi POST tipindedir.

"contenttype" header'ı "multipart/form-data" olmalıdır. bu işlemde HTML form içindeki elemanlar yollanırmış gibi data yollanır.

Header'da aşağıdaki bilgi olmalı:

```
contentType = part/form-data; boundary=--BURAYA_ISTEDIGIMIZ_BIR_STRING_GIRERIZ
```

tarayıcılar otomatik olarak bir form'daki bilgileri request'e uygun şekilde dolduruyor. Yukarıdaki header'daki string'imizi body kısmında kullanıyoruz. her form elemanını ayrı ayrı aynı body'de yolluyoruz. body kısmı:

```
--BURAYA_ISTEDIGIMIZ_BIR_STRING_GIRERIZ

Content-Disposition: form-data; name="username"



Ahmet

--BURAYA_ISTEDIGIMIZ_BIR_STRING_GIRERIZ

Content-Disposition: form-data; name="Filedata"; filename="profile_photo.png"

Content-Type: application/octet-stream



PHOTO_FILE_AS_BINARY_IS_HERE

--BURAYA_ISTEDIGIMIZ_BIR_STRING_GIRERIZ

Content-Disposition: form-data; name="country"



England

--BURAYA_ISTEDIGIMIZ_BIR_STRING_GIRERIZ
```

yukarıdaki işlem tek bir TCP soketi üzerinden yollanır. stream'ing yaparak yollar. ve yukarıdaki tek bir request'tir. tarayıcılarda tek işlem olarak algılanır.

Java REST sunucusu üzerinde metot çok genel olarak şu şekilde olacaktır:

```java
@post
fileUpload(inputStream) {

   while( inputStream.hasNext() ){

        inputStream.read();
   }
   return http.status.ok;
}
```

Alt seviyeli bakıldığında; herhangi kısa bir HTTP isteği de steraming yaparak yollanır. fakat büyük bir data olmadığı için inputStream'ler işin içine katılmaz. bu sebeple upload işlemlerinde inputStream'ler işin içine girer.

# download file
tarayıcılar bir dosya download edilmesi gerektiğinde, GET isteği ile download yapar. get'in response'u data'nın direk kendisidir (binary data). get işleminde hiçbir header eklemeye gerek yok. işlem tek bir request'tir. sadece response'un alınması uzundur. Java sunucu tarafı psudo olarak aşağıdaki gibidir:

```java
@RequestMapping(path = "/download", method = RequestMethod.GET)

public ResponseEntity<Resource> download(String param) throws IOException {

  InputStreamResource resource = new InputStreamResource(new FileInputStream(file));

  return ResponseEntity.ok()

          .contentLength(file.length())

          .contentType(MediaType.parseMediaType("application/octet-stream"))

          .body(resource);
}
```

# multipart/alternative vs multipart/mixed
bu isteklerin standartları mail sunucuları tarafından kullanılmaktadır. normal web sayfalarında kullanılmazlar. bunun gibi birçok mimetype mevcuttur.

# chunked HTTP request
chunk Türkçe kelime anlamı: yığın

HTTP 1.1 ile gelen bir özelliktir. HTTP 2.0 ile bu özellik artık kullanılamamaktadır. çünkü HTTP 2 daha farklı stream'ing özellikleri sunmaktadır.

"Content-Length" header'ı body'nin byte boyutunu belirtmektedir. eğer bu değer yoksa "Transfer-Encoding: chunked" header'ı elle yada server tarafından otomatik eklenmektedir. eğer işlem chunked ise sürekli olarak payload/body karşı taraftan çekilmektedir.

chunked işlemini hem sunucu hem client atabilir.

her chunk satır sonu karakteri ile sonlanmaktadır.

Her tarayıcı ve web browser (default ayarları ile, ek ayar yapılmamışsa) giden body'nin boyutu hesaplar ve belli bir limiti geçmişse chunked olarak karşıya atarlar.

"Content-Encoding: gzip" header'ı sıkıştırılmış chunked data atabilmemizi sağlamaktadır.

# chunked vs multipart

chunked alt seviyeli ve (HTTP API'yi geliştirmiyorsa) yazılımcı tarafından fark edilmemektedir. fakat multi-part daha üst seviyeli bir işlemdir.

# HTTP version history

HTTP versiyonları HTTP/1.1, HTTP/2 şeklinde gösterilmektedir.

- 0.9 (1991)
- 1.0 (1996)
- 1.1 (1999)
- 2 (2015)

Her sürüm arası köklü değişiklikler yapılmıştır.

# HTTP 1.1 vs HTTP 2

- HTTP 2 de her request frame'lere bölünmüştür. bir request/response birçok frame'den oluşabilir. en ufak birim frame'dir.

- eskiden her istek birer TCP bağlantısından gider gelirdi. yani birçok istek paralel yapılmak istendiğinde birçok TCP bağlantısı açılırdı. artık tek TCP üzerinden birçok frame karşı tarafa yollanabilir. bu sebeple hangi frame hangi request'e ait olduğunu anlayabilmek için her frame içinde stream-id kullanılır. aynı anda response geliyor olabilir ve biz data yolluyor olabiliriz. aynı stream'den giden ve gelen olamaz fakat diğer stream'lerden dönüş alınıyor olabilir. bu özelliğe "Multiplexing" deniyor.

- her origin için tek TCP bağlantısı olması daha basit bir yapıya kavuşturmuş oldu. eskiden bir tarayıcı her domain için ortalama 6 soket açıyordu. bu ölçümlere göre daha çok performans kaybına (CPU, RAM) sebep oluyor ve kompleksliği arttırıyordu.

- HTTP dünyasında "message" terimi bir response yada request'i temsil etmek için kullanılır. bu terim HTTP 2'ye özgü değildir.

- header compress özelliği gelmiştir.

- priority: client birçok istek yapsın, bunların dönüşünde hangilerine öncelik verilmesi gerektiğinin bilgisi de karşıya yollanıyor. bu özellik ile http-API'leri kendi içinde öncelik veriyor. bunun için yazılımcının ek bir ayar yada bilgiye ihtiyaç duymuyor. burada örneğin; web tarayıcıları önce CSS dosyalarının, ardından da resimlerin çekilmesini sağlayabilecek. çünkü resimlerin daha geç yüklenmesi, sayfanın hiç yüklenmemesinden daha verimli olacaktır..

- "HTTP/2 push" özelliği: sunucuya index.html'i döndürsün diye istek yaptık. bize index.CSS, main.CSS gibi değerleri de dönebiliyor ekstradan. bu request'leri zaten %99 yapacağını bildiği için sunucu request beklemeden atıyor. web tarayıcısı da bunları cache'leyebiliyor (isterse).

  HTTP 2 anlatan birçok makalede bu madde için "server push" terimi kullanılmış. fakat tam olarak bu maddeye uymuyor. "server push" başka başlıkta anlatılıyor.

- binary data: eski sürümlerde satır sonu karakterlerinin bir önemli vardı. dolayısı ile satır sonu karakterine denk gelecek bir encoding kullanmak zorundaydık. oysa artık byte seviyesindeki sinyaller kullanıldığından, byte seviyesinden bilgi alımı için ekstra parse/kontrol yapmaya gerek yoktur.

- http1.1 client, HTTP 2 sunucusunun döndüreceği cevabı algılayamaz. yani HTTP 2 geriye uyumlu değildir.

# SPDY

bir kısaltma değildir. özel isimdir. Google'ın HTTP alternatifi geliştirdiği protokoldür. fakat HTTP 2 çıkışı ile artık geliştirilmemektedir.

# HTTP request methods

- __get__

  return details of given data.

- __post__

  - HTML üzerinden bilgi göndermek için kullanılır (form bilgileri gibi)
  - daha karmaşık işlemler gerektiğinde bu yöntem tercih edilmelidir:
    - birden fazla kaynak (aynı/tek bir istekte) gönderirken
    - içiçe kaynak güncellemelerinde

- __patch__

  kaynağı güncellemek için kullanılır

- __put__

  override (or create if not exist) the given data.

  put ile yepyeni bir kaynak oluşturulup yollanabilirken, patch ile mutlaka sadece var olan kaynak güncellenebilir.

- __options__

  returns all methods of the given URL.

- __CONNECT__

  HTTP proxy aracılığı ile HTTPS isteği atamayız. çünkü HTTPS şifrelidir. Fakat HTTP proxy'ler buna çözüm olarak şunu sağlarlar:
  - Client, proxy'ye HTTP Connect metodu atar.
  - Artık proxy, gelen her TCP isteğini, destination'a (asıl gitmek istediğimiz server'a) yollar. Bu şekilde paketin içindekileri proxy göremez.

  CLient, proxy ve server arasında oluşturulan bu TCP bağlantısına __tunnel (or tünel)__ denir.

- __trace__

  request'te gönderilen bilgiyi aynı şekilde döndürmektedir. test amaçlı kullanılmaktadır.

- __DELETE__

  delete given data

- __HEAD__

  get metotu ile aynıdır. sadece response'ta body yoktur. response header'da, LastModified/ContentLength gibi değerler mevcuttur. bu bilgiler kullanarak tekrar get ile bilgiyi çekmeye gerek var mı diye kontrol edilebilir. (başka başlıkta anlatılıyor)

(source-id: 196)'de her işlemde hangi status kodun dönmesi gerektiği ve hangi bilgilerin hangi durumlarda dönmesi gerektiği belirtilmiştir. örneğin; "4.3.3.  POST" başlığında POST işlemi ile bir kaynak oluşturulduğunda, response olarak 201 dönülmeli ve header'da "Location" dönülmesi ve bu değerde (location değerinde), oluşturduğumuz yeni kaynağa erişebilmek için gerekli "get" isteği URL'sinin olması gerektiği belirtilmektedir.

rfc7231, (source-id: 197) gibi sayfalardan referans olarak kullanılmaktadır.

# Safe method (or safe function) vs Idempotent method (or Idempotent function or Idempotent operation)
Idempotent; Latince'deki "eş(denk)" anlamına gelen "idem", ve İngilizce'de "güç" anlamına gelen "potent", terimlerinin birleştirilmesinden oluşturulmuştur. Idempotent teriminin matematik ve bilişim dünyasında aşağıda anlatılan anlamı ile kullanılır.

GET, HEAD, OPTIONS, TRACE "safe" metotlardır. çünkü sunucudaki state'i değiştirmezler. RFC'de "safe metot" olması imkansız yazmaktadır, çünkü sunucu tarafta gelen istekler'de log basılması bile sunucudaki statüyü değiştirmektedir, yani side effect yaratmaktadır.

Aynı istek birden fazla kere atılması veya sadece 1 kere atılması hiçbir farklılık yaratmayacak ise, bu istekler Idempotent metotlardır. RFC'de bunların Idempotent olduğu belirtilmiştir: GET, HEAD, OPTIONS, TRACE, PUT and DELETE.

Idempotent istek birden fazla defa sunucuya yollandığında, sunucu aynı cevabı dönmek zorunda değildir. örneğin PUT ettik, "ok" döndü, tekrar PUT ettik bu sefer "already exist" döndü. ama bu "Idempotent" olmasını engellemiyor.

Idempotent terimi genel olarak yazılım dünyasında fonksiyonlar için kullanılmaktadır.

# conditional requests
Herhangi bir request'imize cache header'larını ekleyerek duruma göre işlem yapmasını bekleyebiliriz. örneğin; get'e header ekleyebiliriz. eğer veri değişmişse bize yeni data döndürülür. put'a header ekleriz. eğer data şimdiki yollayacağımız data ile aynı ise, sunucuda put yapmaya gerek kalmaz. bu tarz request'lere conditional request denir. HTTP standartlarında HEAD metotu bu iş için tasarlanmıştır. head'de body atılmaz sadece header'lar atılır. HEAD değişiklik var mı yok mu onun bilgisini döner. fakat body dönmez. standartlarda yoktur. bu sebeple herkes genelde; HEAD kullanmıyor. onun yerine diğer get veya put'a header eklemeyi tercih ediyor. çünkü head kullanılırsa tekrardan put ve get metotunu tekrar sunucuya atmak gerekebilir. fakat direk put ve get'e header ekleyince, sunucu condition'ı sağlarsa işlemi yapacaktır (get ise yeni bilgiyi  döndürecek, put ise bilgiyi update edecektir).

HEAD metotu header'da "ETag" veya/ve "Last-Modified" tag'ini barındırıyor. buna cevap olarak sunucu; aynı (istek) path'e "get" isteği yapılsaydı en son atılan response'takine göre değişiklik var mı yok mu onun bilgisini döner.

HEAD yerine get ve put kullanılacaksa hem "ETag" veya/ve "Last-Modified" hemde aşağıda anlatılan "if-match" gibi metotlarda kullanılabilir.

Apache HTTP server statik dosyaları dışarıya açarken bu özelliği otomatik sunar. yazılımcının ek bir configürasyon/kod girmesine gerek kalmaz.

JavaEE yada Spring'de Controller'larda head isteniyor ise; bu metot elle hazırlanmalıdır. örnek bir wrapper yazılabilir:

```java
protected void doHead(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

    doGet(req, response);
}
```

yukarıdaki metot filter'a eklenip eğer request HEAD ise çağrılabilir. bu şekilde her metot için ayrı HEAD yazmak zorunda kalmayabiliriz.

web tarayıcıları web standartlarını varsayılan olarak uygular ve static dosyalar için (örnek CSS, <img> ...) conditional request kullanıyor fakat head ile değil. bir elemanı çekmek için en son tarayıcının çektiği tarihi "get" metotuna "If-Modified-Since" olarak ekliyor. sunucu eğer hala değişiklik yoksa cevap olarak bunu bildiriyor. eğer değişiklik var ise; direk cevap olarak yeni bilgiyi gönderiyor. bu control web tarayıcı tarafından cache disable edilmedikçe otomatik yapılır.

"Spring-data-REST" modülü database'deki değerleri direk önyüze sunacak Controller'ları da otomatik hazırlıyor. Birkaç annotation kolayca entity'lerimizi önyüze hazırlıyor ve head kontrollerini de standartlara bağlı olarak dışarıya açıyor. Spring-data-REST pek tavsiye edilmez. çünkü database nesneleri genelde direk tümü ile önyüze açılmazlar. bazıları static parameter servislerini Spring-data-REST ile dışarıya açıyor. Spring-data-REST modülü her entity için put, get gibi tüm metotları dışarıya hazırlıyor ve Richardson Maturity Model Level-3'ü de hazırlıyor.

database'lerde otomatik olarak bir satır değiştiğinde, o satırda belirlediğimiz bir sutuna otomatik tarih atılabiliyor. bu özellik için Java'da da entity içinde o sütun için özel bir annotation bulunmaktadır. bu şekilde önyüze HEAD metotlarımızı hazırlarken bu sütundan yararlanabiliriz.

# strong ve weak validator

W/ karakterlerini hash'lerin önüne atarsak sunucunun weak kontrol yapmasını istemiş oluruz. atmazsak strong istemiş oluruz.

strong ve weak'in anlamı yazılım geliştiricinin tanımına kalmıştır. örneğin bazı mimarilerde strong kontrol, header'ları dahi kontrol eder. bazı mimarilerde ise; strong ilgili kaynağın çok önemli verilerinin değişip değişmediği anlamına gelir.

# cache ile ilgili header'lar

(cache harici diğer header'lar başka başlıkta anlatılıyor)

- #### Cache-Control (eski sürüm HTTP'lerde "Pragma"):

alabildiği değeler: (multiple değerler alabiliyor)

- no-cache: data must be re-validated on each request

- no-store: never cache

- public: can be cached by the browser and any intermediate (proxy) caches

- private: only browser should cache it

- max-age: max age of cached content as second. on new HTTP versions replaced by "Expires" header.

- s-maxage: max-age ile aynı görevi görüyor fakat bu client haricindeki aracılar (proxy) için geçerli bir değerdir.

- #### ETag

Cache verisinin değişip değişmediğini kontrol etmek için kullanılan değer.

Spring Framework notes: Spring ile bunu implemente etmek istediğimizde controller spesifik bir kod yazmak zorunda değiliz. ETag aslında sadece client için avantajdır. Server tarafta yine request tümüyle controller sürecindeki business akışına girmektedir. Spring'de hazır bir filter var: ShallowEtagHeaderFilter. Bu filter'ı istediğimiz controller'ın önüne koyuyoruz. Her request önce bu filter'a geliyor. filter contorller'ı execute ediyor. Contoller'dan dönecek response'un hash'ini alıyor ve E-tag süreçlerini kendi ilerletiyor. Yani; Eğer değişiklik var ise, reponse'u komple client'a dönüyor yoksa dönmüyor vs.

- #### If-Match

örnek kullanım:

If-Match: "34242"

sağ taraf kaynağın hash'idir. eğer değişiklik var ise sunucu kaynağı dönmeyecek. eğer değişiklik yok ise kaynağı dönecek.

if-match içine birden fazla değer atanabilir. örnek:

If-Match: "34242", "34666"

- #### If-None-Match

If-Match'in tersidir. eğer kaynak'ın hash'i bu değil ise tüm kaynak döndürülecektir.

- #### If-Modified-Since

burada bir tarih değeri sunucuya yollanıyor. eğer bu tarihten sonra ilgili kaynak güncellenmiş ise tüm kaynak cevap olarak dönecektir.

içine birden fazla tarih değeri atabiliriz.

- #### If-Unmodified-Since

If-Modified-Since'in tersi.

- #### IF-Range

içine tarih yada hash değerini alabiliyor. bu header ile beraber farklı bir header'da "Range" değerini yollamak şart.

içine sadece bir tane değer alabiliyor.

if the entity (only the part of range) is unchanged, send me the part(s) that I am missing; otherwise, send me the entire new entity.

- #### Last-Modified

sunucu tarafından client'a gönderilen bir header'dır. ilgili kaynağın en son ne zaman güncellendiğini belirtir.

- #### 304 Not Modified

bu status sunucu tarafından client'a eğer değişiklik yok ise gönderiliyor. eğer kaynakta değişiklik var ise sunucu 200 OK döndürüyor. fakat her request için aynı cevap dönmeyebiliyor. istisnai durumlar çok fazla.

# payload

Türkçe kelime anlamı: yük.

iletişim protokollerinde metadata'lar dışında kalan mesaj bloğudur. asıl gönderilmek istenen mesaj kısmıdır. doğal olarak; sadece Ajax işlemlerinde kullanılan bir terim değildir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Viewport
Sayfa yapısı ile ilgili kararları vermemize yarayan meta etiketidir. HTML HEAD tag'larının içerisinde olmalıdır.

örnek;

sayfa ilk yüklendiğinde %100 zoomlanmış olmalıdır:

<meta name="viewport" content="initial-scale=1" />

genişliğin 360 pixel olduğunu belirtiyoruz. tarayıcı buna göre sayfayı fit edebilir.

<meta name="viewport" content="width=360" />

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Document Object Model (or DOM)
DOM, bir programlama lisanı veya sadece JavaScript için ortaya atılmış bir olgu değildir. DOM; HTML ve XML gibi dokümanları temsil etmek oluşturulan modeldir. Yazılım dünyasında "Model" objemizde, o model'in davranışlarına karşılık gelen metodlar olabilir. Bu metodlar ile model'in özellikleri üzerinde işlem yapabiliriz (arama, güncelleme...). Bu durum "HTML DOM"'da da vardır.

"HTML DOM"; kendi içinde "HTML tree" barındırır ve bunu dışarıya bir API ile açar. 

"DOM API", DOM'un metodlarını temsil etmek için kullanılan bir terimdir.

Genelde DOM denilince HTML dökümanları için olan API akla gelir, fakat DOM, sadece HTML için ortaya atılmış bir terim değildir.

# Browser Object Model (or BOM)
HTML tarayıcılarının, DOM haricindeki, Javascript'e ekledikleri Javascript objeleridir. bu objeler NodeJS gibi ortamlarda yoktur.

window.history, window.navigator gibi objeler bu model'in içindedir.

# XHTML (or Extensible HyperText Markup Language)
HTML standartları katı kurallara sahip değildir. örneğin XML tag'leri büyük harf yazılabilir. oysa XHTML bu kuralları daha katı standartlara bağlamıştır. XHTML tarayıcılar tarafından desteklenir çünkü yine XML yapısındadır. sadece kurallar daha katı tutulur. temiz kod sağlanır.

# DHTML (or Dynamic HTML)
CSS Javascript ve HTML çalışan yapılara verilen isimdir.

# HTML DOM Levels
HTML DOM'ün sürümleridir. DOM'un çalışma prensiplerinin kurallarıdır (spesifikasyonlarıdır). DOM 1, DOM 2, DOM 3, DOM 4...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# cookie security policy
cookie'ler:

- port: aynı domain'inin tüm port'larındaki servisler tarafından kullanılabilir. port önemsizdir.
- subdomain: subdomain'ler cookie spesific programatik olarak belirtilebilmektedir.
- path: path bilgisi önemsizdir.

# cookies properties

- secure: sadece HTTPS üzerinden giden Ajax isteklerine eklenir. HTTP sise gönderilmez.

- httpOnly: Javascript tarafından bu cookie'ler okunamaz. fakat her Ajax isteğinde sunucuya gönderilir.

- Expires (or Max-Age): son kullanma tarihi

- Path: o domain için kimlerin bu cookie'ye erişebileceği yetkisidir. path'in prefix'idir. tüm bu prefix'e ait subdirectory'ler (sub-URL'ler) bu cookie'ye erişebilir.

- same-site:

  3 farklı değer alabilir:

  - Strict

    ilgili cookie sadece kendi path'ine bağlı yere yollanabilir.

  - Lax

    ilgili cookie; third-party sitelere sadece GET isteği ile yollanır veya top-level domain'de sadece cookie'nin path'indeki sunuculara yollanır.

  - none

    same-site alanı için default değerdir. eğer same-site boş bırakılırsa none olarak set edilir. fakat güncel tarayıcılarda artık default değer Lax'tir.

    tüm Ajax isteklerinde bu değer request'e ekleniyor.

- domain: hangi domain'lerin buna erişecek yetkisi olup olmadığıdır. normalde tarayıcılar başka domain'den cookie okumaya veya yazmaya izin vermez fakat bu kısım 2 sebepten ötürü yapılmıştır:
  - third party cookie'lere izin veriliyor (başka başlıkta anlatılıyor)
  - sub-domain'ler belirtilebilir: her subdomain (a.Google.com, b.Google.com gibi) sadece kendi yada o domain'e bağlı tüm subdomain'ler tarafından erişilebilir cookie set edebilir.

# third party cookie vs first party cookie
- a.com'a gittik.
- r.com içerisinden bir JS yüklendi
- r.com kendi içerisinde cookie set etti (Javascript'te cookie set ederken domain de belirtilebilir). bu senaryoda r.js kendi domain'ine cookie set ediyor.
- Daha sonra b.com'a gidildi ve yine r.com'un bir JS'i yüklendi. r.com b.com'a gidildiğinde, bu kullanıcının a.com'a da gittiğini görebiliyor. çünkü kendi cookie'leri third party.

son kullanıcı tarayıcıların ayarlarından basit bir şekilde bunu disable edebiliyor.

# cross-site cookie vs third party cookie
3rd parti cookie, asıl domain dışında kalan cookie'lere referans etmek için kullanılan bir terim. cross-site cookie'de, farklı domain'lerden de okunacak cookie anlamına gelir. bir cookie'nin farklı domain'den okunup okunmayacağını add-blocker'lar biliyor. bir site 3rd parti cookie'ye kaydediyor olabilir fakat sadece 1 domain'den o cookie'lere tekrar erişiyor olabilir. bu sebeple her 3rd cookie, cross olmak zorunda değildir, fakat %99 öyledir.

cross-site cookie'ler özellikle kullanıcıları takip amaçlı kullanıldıkları için "cross-site tracking cookie" olarakta isimlendirilirler.

# supercookie
sadece ".co.uk" ".com" gibi domain barındıran cookie'lerdir. çok tehlikelidirler. ".co.uk" olan her domain bu cookie'leri görebilir.

supercookie terimi, bazı kaynaklarda web browser'ın tuttuğu cookie dışında başka anlamda kullanılmaktadır. web standartlarında olan fingerprinting açıklarına da bu supercookie denilebilmektedir. örnekler:

- favicon'ı cache'te tutan tarayıcılar, bir siteye gittiğinde favicon için GET isteği atma gereği duymaz. Dolayısı ile abc.com sitesi, eğer hızlı bi şekilde son kullanıcının sayfasını 10-20 adet farklı siteye yönlendirip, her domain için favion varmı diye browser'ın GET isteği yapıp yapmadığını kontrol edebilir. 1-20 isteğin atılıp atılmadığının kombinasyonu tekil bir ID halini almaktadır. Bu güvenlik açığına supercookie denilmektedir.

  Bunu engellemek için Firefox şöyle bir çözüme gitti:
  
  Firefox her web sayfası için cache ve network isteklerini birbirinden bağımsız (makalelerinde "partition" olarak isimlendiriliyor) olarak tutuyor.
  
  Bu gidilen çözüm ile cache'ler aracılığı ile supercookie açığı da kapatılmış oldu.

- HTTPS Strict Transport Security flags supercookie

# media file transfer cookies
media dosyaları web tarayıcı tarafından otomatik sunucudan çekilirken, var olan cookie'ler sunucuya yollanmaktadır. nihayetinde resimlerde çekilirken, HTTP isteği yapılmaktadır.

# Firefox and Chrome cookie management
- Chrome ile bir sayfada inspect element yapıldığında, cookie kısmında www.a.com grubu altındaki cookie'ler listelendiğinde, domain sütununda bazen www.a.com'dan farklı domain'lerin olduğunu görebiliriz. bunun sebebi a.com'da (inspect ettiğimiz sayfada)  farklı iframe'ler olmasıdır. farklı domain grupları ise, Javascript tarafında gidilen sayfanın (a.com'un), farklı domain'ler için set ettiği cookie'lerdir. oysa Firefox'ta cookie'ler listelenirken sadece domain'e göre gruplandırılmaktadır.

- Firefox ve Chrome private modlarda cookie'ler ile ilgili hiçbir farklı politika izlemiyor.

- cookie'ler güvenlik sebebi ile varsayılan olarak tarayıcılar ile düzenlenemiyorlar. bunun için eklenti kullanmak şart.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Diğer sitelere login olup olmadığı bilgisini edinme

Örneğin Facebook'a login olunca, instagram ayrı sekmede Facebook'a login olduğumuzu anlayabiliyor. Bunu ufak bir hack yöntemi ile gerçekleştiriyorlar. Bu açık bir CSRF tipi bir saldırıdır. Bir siteye giderken yönlendirmeler kullanılabilmektedir. Örneğin; https://www.Facebook.com/login.php?next=www.Facebook.com/bookmarks/Fpages  bu sayfa ğer kullanıcı login olmuşsa bizi "next" parametresinde yazan yere yönlendirecektir. Eğer login olmamışsak login sayfasına yönlendirecektir. İşte bu isteği farklı bir sekmeden yaptığımızda, login sayfasına gidilmiyorsa, Facebook'a login olunmuştur demektir. Fakat farklı sekmede farklı domain'den request yapmamız güvenlik gereği engellenmiştir. İşte JSONP de olduğu gibi benzer bir trick uygulanıyor. \<IMG\> elementi sayfaya (DOM'a) include edilirmiş gibi bir kod yazılıyor:

```html
<img onload="alert('logged in to fb')" onerror="alert('not logged in to fb')" src="https://www.Facebook.com/login.php?favicon.ico">
```

Bu resim dosyasına web tarayıcıları her zaman farklı domain'e izin veriyor. çünkü siteler normalde CDN kullanmaktadır video ve resimler için. Yukarıdaki örnekte https://www.Facebook.com/login.php?next=www.Facebook.com/myprofile/photo.jpg olduğunda dosya eğer login olmuşsa kullanıcı çekilmektedir. Dolayısı ile kullanıcının login olup olunmadığı, hatta bazen profil fotosu da çekilebilmektedir.

Bazı siteler tüm resimleri CDN'e yüklendiklerinden işler daha da karışmaktadır ve bu tehlikeyi engellemektedirler. Fakat bazı sitelerin en çok kullanılan standart dosyası olan favico.ico resim dosyası çekilmektedir.

# "Enhanced Tracking Protection" of Firefox
- bu özellik başka bir çok tarayıcı eklentisinde de mevcuttur.
- "__Enhanced Tracking Protection__" altında birçok çeşit content engellenir. Engellenecekler listesi Firefox'a hardcode gömülüdür. Bunlar ayrı ayrı grulandırılmıştır ve son kullanıcıya ayrı ayrı disable/enable seçim hakkı sunulur. örneğin; "__standart mode__"'da bunların bazıları açık iken, "__strict mode__"'da hepsi açıktır. tabi "__custom mode__"'da son kullanıcı istediğini aktif-pasif yapabilir.
- Liste şu şekildedir:
  - __Social media trackers__ (burada sadece sosyal medya sitelerinin, diğer açık olan sosyal medya sitelerinden bilgi alması engellenir)
  - __Cross-site tracking cookies__ (also isolates remaining cookies) (bu "third-party tracking cookies" engellemesi olarakta bilinir. bir web sitenin, diğer web sitelerde ne yaptığımızı anlamak için kullandığı sadece 3üncü parti cookie'ler engellenir.)
  - __Fingerprinters__ (firefox'a hard-code gömülü olan ve fingerprint'imizi aldığı düşünülen web sitelerin listesi var. sadece bu listedeki sitelere Javascript API'leri fake fingerprint döndürüyor (font, resolution...))
  - __Cryptominers__ (cryptomining yapılmasını sağlayan JS'ler engelleniyor)
  - __Tracking content__ (web sitenin içerisinde olan herhangi bir content'i engellenebiliyor. bu content'lerin takip amaçlı olduğunu biliniyor olması şart. burada reklam veya diğer şeyler engellenmemektedir)

# "mixed content" of Firefox
HTTPS başlatılan bir sitede daha sonra HTTP yüklenen bir dosya var ise bu duruma denir.

"__mixed content blocking__" ise bu durumların engellenmesidir.

2 ye bölünür:

- active: CSS, HTML, JS, Iframe, Flash gibi sayfanın yapısını değiştirebilecek tehlikeli dosyalar. bu dosyalar tarayıcılar tarafından engellenmektedir ve developer consola çıktısı kırmızı ile uyarı şeklinde verilmektedir.

- pasif: video, ses, resim gibi sayfanın sadece görünümünü değiştirebilecek dosyalar. bu dosyalar tarayıcılar tarafından engellenmiyor fakat developer console'a warning veriliyor.

# "Total Cookie Protection" of Firefox
3rd parti cookie'lerinde farklı domain'lerden okunamamasını sağlıyor. Tabi bu durumda birçok site hata verecektir veya gerçekten takip etmeme amacı olan sistemlerde patlayacaktır. Bu sebeple Firefox kendi içinde hard-coded olarak, tehlikesiz gördüğü cookie tutacak domain'lerin listesini tutuyor. Onlara farklı domainler arası kullanılabilmesi için izin veriyor.

# "HTTPS-Only Mode" of Firefox
Bir web sitesine http gidilmeye çalışıldığında, https'e otomatik yükseltir. Eğer web site http desteklemiyorsa kullanıcıdan onay istemesini sağlatır.

Eğer secure gidilen sitede, secure olmayan bir resim, video gibi bir request var ise, o request'i bloklar. son kullanıcı geçici olarak bu sitenin secure olmayan resimlere erişmesini açabilir. 

# "sandbox" of Chromium
__Sandbox__ teknolojisi sayesinde chroumium-based tarayıcılar açtığı her process'in yetkisini kısıtlar. Böylece web sayfasının render edildiği thread'deki kötü niyetli bir kod, web tarayıcısındaki bir güvenlik aığından yararlanmaya kalkarsa, Chromium'un sandbox'una takılacaktır. sandbox'u da aşabilir. fakat bu bir seviye daha zordur.

# site isolation
Bu her origin'in (URL'nin) farklı thread'de çalışacağını garantileyen mimari yapıya verilen isimdir.

# XSS (or Cross Site Scripting)

Cascading Style Sheets (CSS) ile aynı kısaltmaya sahip olduğu için XSS olarak kısaltılır.

HTML formlarında, yada URL'den giden parametreler manuel olarak değiştirilerek, sunucu tarafta SQL injection , yada HTML tarafında değişikliklere yol açan script'lerdir.

XSS saldırılarını engellemenin en temel ve köklü yöntemi HTML'de her karaktere gelen entity'leri kullanmaktır.

çeşitleri: (kaynak: (source-id: 198) )

- # Stored XSS (AKA Persistent or Type I)

  kötü amaçlı kodların web sunucusunda veritabanında kayıt edildiği durumları temsil eder. web sitesindeki bir paylaşıma yapılan yorumlar, buna en iyi örnektir. buradaki bilgiler başka user'lar tarafından görüntülendiğinde kötü amaçlı kodlar çalışacaktır.

- # Reflected XSS (AKA Non-Persistent or Type II)

  bu tarz XSS'ler sadece o andaki user için çalışır. yani genelde input veya URL alanlarına yapılan girişler sonucunda, kötü amaçlı kodlar kendisine yansır. tabi saldırılar diğer kullanıcılara yapılacağı için, genelde başkasının bir yerlere tıklamasını sağlamak gerekir. örneğin sahte bir email atıp, kötü amaçlı kodların user'ın kendisi tarafından tetiklenmesi beklenir.

- # DOM Based XSS (AKA Type-0)

  DOM Manipülasyonu yapan XSS'lerdir.

# XSS in React framework
ReactJS yapısı gereği XSS ataklarının bazıları engelliyor. View katmanında (render'dan dönen view) dinamik variable varsa onu string olarak escape karakterlerini ekliyor. en basit örnek üzerinden gidelim:

```jsx
var htmlString = '<img src="javascript:alert('XSS!')" />';

render() {
    return (
        <div>{htmlString}</div>
    );
}
```

result:

```html
<span>"<img src="javascript:alert('XSS!')" />"</span>
```

Fakat bu tüm XSS açıklarının kapatıldığı anlamına gelmiyor. kaynak: (source-id: 199) "CyberPanda Consulting" answer at Aug 15 2018 at 3:33.

# HTML character entities

  Bir string'in her karakteri (or sadece özel karakterleri) HTML referansına çevirirsek ekranda her zaman bu metin gösterilecektir. fakat ekranda HTML olarak gösterilmesi gerekiyorsa işte o zamana riskler artmaktadır. buna örnek bir durum: gmail web arayüzünün bize HTML formatında atılan bir emaili, web browser'ımızda bir div'in içerisinde göstermesi örnek verilebilir. işte bu gibi durumlarda verilen HTML string'ini safe hale getiren (event'lerini silen), bilinen XSS saldırılarını temizleyen kütüphaneler kullanılmaktadır. örnek kütüphane: Htmlpurifier (source-id: 432)

  burada Firefox HTML4.0 için tüm character entity listesi vardır:

  (source-id: 200)

  Birçok karakterin hem name hemde code olarak gösterimi vardır. örnek:

  | Result | Description                        | Entity Name | Entity Number |
  |--------|------------------------------------|-------------|---------------|
  |        | non-breaking space                 | \&nbsp;     | \&#160;       |
  | <      | less than                          | \&lt;       | \&#60;        |
  | >      | greater than                       | \&gt;       | \&#62;        |
  | &      | ampersand                          | \&amp;      | \&#38;        |
  | "      | double quotation mark              | \&quot;     | \&#34;        |
  | '      | single quotation mark (apostrophe) | \&apos;     | \&#39;        |
  | ¢      | cent                               | \&cent;     | \&#162;       |
  | £      | pound                              | \&pound;    | \&#163;       |
  | ¥      | yen                                | \&yen;      | \&#165;       |
  | €      | euro                               | \&euro;     | \&#8364;      |
  | ©      | copyright                          | \&copy;     | \&#169;       |
  | ®      | registered trademark               | \&reg;      | \&#174;       |

  Bazı karakterleri bir önceki karakterlerin üzerine ekleme de yapabiliyor:

  | Mark | Character | Construct | Result |
  |------|-----------|-----------|--------|
  | ̀    | a          | a\&#768;  | à     |
  | ́    | a          | a\&#769;  | á     |
  | ̂    | a          | a\&#770;  | â     |
  | ̃    | a          | a\&#771;  | ã     |
  | ̀    | O          | O\&#768;  | Ò     |
  | ́    | O          | O\&#769;  | Ó     |
  | ̂    | O          | O\&#770;  | Ô     |
  | ̃    | O          | O\&#771;  | Õ     |

 HTML5'te birçok karakter daha destekleniyor:

  (source-id: 201)

  HTML ile artık karakterler Unicode code point'i ile referans ediliyor. tüm Unicode desteklenmemektedir. sadece belli bir kısmı destekleniyor.

  Fakat XHTML ile HMTL arasında bir ortak çözüm olmadığından ve HTML geriye uyumluluğu için &amp;, &lt;, &gt;, &quot; and &apos; haricindeki karakterler normal yazılması önerilir. kaynak: (source-id: 202)

# CSRF (or Cross-Site Request Forgery or XSRF)
Forgery kelime anlamı: sahtecilik

(Bu konu başka bir başlıkta da detaylı anlatılıyor)

resmi olmayan kullanımları:
- one-click attack
- session riding
- CSRF'in "sea-surf" olarak okunduğunu sıkça duyabiliriz.

yetkisi olmadan istek yapan HTTP istek çeşitleridir. örneğin; yetkisi olmadan şifre değiştirmeye çalışan, yada yetkisi olmadan mesaj silen her HTTP isteği CSRF grubu altındadır.

CSRF case'ine şöyle yakalanabiliriz: bir bankaya login olduk. Yan sekmede mailimize bakıyoruz. Kötü niyetli bir email açtık. Mailin içinde banka domain'ine yapılan bir request oldu. bu request birçok şekilde tetiklenebilir:
- event click sonrası (son kullanıcı hatası)- ki bazen bazı yerler link değilmiş gibi CSS yazılıyor, oysa orada bir link oluyor...
- tarayıcılarda medya dosyaları oto yüklenir. GET isteği atılması sağlanabilir. Oysa karşı tarafta bir medya yoktur. CSRF engellemek için REST servislerimizi (en azından önemli request'leri) GET değil, POST yapmamız gerekiyor. En azından, web tarayıcısı user-click yapmadan hiç POST yapmıyor.

Burada önemli olan nokta şu: isteği atarsınız ama dönen cevapta o domain'in cookie'sine vs yazamazsınız.

CSRF'i engellemenin en önemli çözümü, her response'ta client'a sadece memory'de tutması gereken bir random geçici token atmaktır. Her request'te bu token o client'tan mı gelmiş anlarız.

# CSRF vs XSS

XSS sadece client side koşulan kötü amaçlı script'leri kapsar. XSS'te yetki yada HTTP istek atma konusu olmak zorunda değildir. XSS, CSRF kümesini ve daha fazlasını kapsar.

# Chrome guest mode vs private mode
- her iki modda da pencereler kapatıldığında bilgiler saklanmaz.

- private mode sık kullanılanlarda değişiklik yapabilir ve eklentileri kullanmaya devam edebilir. oysa guest mode, anında (guest isimli) farklı bir profil açar ve pencere kapatılınca o profili siler.

- Google Chrome'un gizli moduna "incognito mode" ismi verilmiştir. Incognito Türkçe anlamı: kılık değiştirmek, sahte ad.

# Firefox vs Chrome private mode
Private mode seçeneklerinin ilk çıkış amacı, sadece private mode penceresi kapatıldığında history'nin saklanmamasıydı. Daha sonrasında farklı ek özelliklerde eklenmeye başladı. Bu sebeple bir web sayfası, private mode'da aynı şekilde açılmayabilir. Örneğin; Firefox (aksi tanımlanmadıkça) private mode'da Content Blocking'i açık tutuyor. Normal browsing'de bunu açmıyor. Normal browsing'de ancak özel tanımlama yapılırsa açılıyor.

Bir problemde; Firefox ve Chrome'un private mode'ları farklı yönetmesidir. Bir tarayıcıda private mode'da bir özellik aktif iken, diğerinde olmayabilir.

# Firefox profiles
Firefox -p parametresi ile profile manager açılır. Chrome'da olduğu gibi farklı profiller mevcuttur.

# Multi-Account Containers
Firefox'un bir eklentisidir. bu eklenti ile aynı profile içerisinde her sekmenin birbirinin cookie'leri ve diğer storage'leri hiç görmemesi sağlanabilir. örneğin 2 Facebook hesabı farklı sekmelerde açılabilir.

# adblock vs adblock plus
sadece reklamları engellemek amaçlı yapılan tarayıcı eklentileridir. 2 farklı proje.

# uBlock Origin (or uBlock₀ or old-name:μBlock) vs uMatrix
bu eklentiler tarayıcıdan content engellemesi için yapılmıştır. herhangi bir Javascript'i yada nesneleri filtreler hazırlayarak engelleyebilmemizi sağlar. adblock ve adblock plus'a göre çok daha gelişmişlerdir. kullandıkları filtreler, diğer add-block eklentilerindeki filtreleri de desteklemektedir(uyumludur).

# Noscript
web sayfasındaki elementleri parçalı/tamamiyle engelleyerek güvenliği sağlamakta. örneğin sadece script tag'lerini, sadece font'ları, sadece WebGL'i, sadece media'ları engelleyebiliyor.

# Ghostery
Analitic firmalarının istek yapmasını engelliyor. fakat bu sonuçları kendi sunucularında topluyor ve yine engellediği firmalara satıyor.

# WOT (or Web of Trust or myWOT)
bu eklenti kullanıcılardan site güvenliği hakındaki oylamalarını toplamaktadır. ve bu oyları sürekli tarayıcının tepesinden göstermektedir.

# adblocker web sitemizdeki bir kaynağı engellerse ne yapmalıyız?
- "adblock plus" eklentisi Firefox developer menüsünde özel sekme ekliyor. buradan bir web sitesine gidildiğinde hangi objelerin engellendiğini görebiliyoruz.
- önce hangi filtrenin sitemizdeki kaynağı engellediğini bulmalıyız. bunun için adblocker eklentimizdeki listeleri tek tek disable edip her disable işleminden sonra web sayfamızı reload etmeliyiz. hangi filtrenin engellediğini tespit edersek, o filtrenin resmi sitesine gidip geliştiricileri ile iletişime geçmemiz gerekir.
- filtreler adblock eklentileri/uygulamalarından bağımsız olarak başkaları tarafından geliştirilir.
- eğer engellenen objeler reklam ise, geliştiriciler olumsuz dönüş yapacaktır.

# ad blocker'a yakalanmamak için ne yapılmalıdır
- HTML veya kaynakların URL'lerinde "ad", "advertise", "banner" veya bunları içeren string'ler var ise bu kaynaklarımız mutlaka engellenecektir.
- bazı reklamların tasarımı uygun ise, bazı filtreler bunları kaldırmıyor. uygunluğun birçok standardı var. bunlardan bazıları:
  - browser'da bellek tüketimine sebep olmaması
  - video olmaması
  - resim ise resim formatının düşük kalitede olması
  - resim ise ufak boyutta olması
  - ekranda son kullanıcıyı rahatsız etmeyecek pozisyonda olması
  - bazı filtreler resimleri de engelliyor. sadece text reklamları kabul ediyor (hafif olduğu için)
  - arkada çalıştırdığı JS'in kullanıcı gizliliğine saygı duyuyor olması
  - ekranda ufak boyutta olması

Bazı filtreler +18 içerikleri de engelliyor. bu filtreler genelde iş yerlerinde aktif ediliyor.

# ad blocker'ı algılamak
browser'da adblocker olup olmadığı kolaylıkla algılanabilir. hatta kullanıcının hangi filtreleri devreye almış olduğu bile tespit edilebiliyor (çünkü bilinen sık kullanılan filtreler var - açık kaynaklı). bunlar için birçok farklı yöntemler var. fakat bu yöntemler sürekli olarak değiştiriliyor, çünkü her tespit eden koda karşılık, adblocker'lar tarafından bunları engelleyen kodlar tasarlanıyor.

ad blocker'ı algılamak için çok basit ve temel bir örneği ele alalım:
- /ads.js'e bir istek yaparız. eğer isteğimiz engelleniyor ise, kullanıcı adblocker kullanıyor demektir.
- sitemizde ID'si "advertising" olan bir div yaratırız. sayfa yüklendikten sonra bir JS ile bu div'in içindekilerin yada div'in manipüle edilip edilmediğine bakarız. eğer manipülasyona uğramışsa, son kullanıcı adblocker kullanıyor demektir.

Adblock, AdblockPlus, UblockOrigin, uMatrix varsayılan olarak kendilerinin çalıştığını karşı tarafa gizleme gibi bir dertleri yok (zorunlu kalınmadığı takdirde). fakat bu eklentilerin bazı çatalları kendilerinin çalıştığını ilgili web sayfasına engellemektedirler.

# adblocker'lar nasıl çalışır
 ABP-compatible filter'larda 2 tip engelleme var:
- network filters

  network isteği direk engellenir

- Cosmetic filters (UblockOrigin cosmetic diye isimlendiriyor. adblock ve adblock plus "element hiding" olarak isimlendiriyor.)

  network isteğinden dönen cevap engellenmez, fakat sayfadaki content, HTML DOM içerisinden silinir veya hide edilir.

# filtreler
UblockOrigin eklentisinin ayarlarındaki "filtre" sekmesine girildiğinde birçok farklı kategoride filtre görünecektir. Bunların bazıları:

- built-in

  karışık (diğer tüm kategorilerden filtreler burada var).

  default'ta UblockOrigin community'si tarafından yönetilen ve aktif edilmiş durumda olanlar.

- ads

  sadece reklamları engelleyen filtreler

- privacy

  takip edilmek için yüklenen content'leri engelleyen listeler.

- malware domains

  malware olduğu belirlenen domain'leri/content'leri engelliyor.

- annoyaces

  önemsiz bildirimleri/content'leri (gereksiz popup'ları: bun en güzel örnek: "cookie koşullarımızı kabul etmektesiniz" popup'ları) engelliyor.

- regions, languages

  ülke/dil bazlı sitelerin filtreleri.

# Canonical Name record (or CNAME record)
canonical kelime anlamı: kurallara uygun, geleneklere uygun.

DNS server'lar, bir DNS kaydını, farklı bir DNS'e referans ettirebilir. örneğin mail.domain1.com'u, domain1.com gibi gösterebiliriz. Bunu DNS sunucuları destekliyor. Bu tarz DNS kayıtlarına CNAME record denir.

Web tarayıcıları veya eklentileri 3üncü party siteleri engellemeye başladığından, bazı internet siteleri kendi sub domain'lere istek yaptırarak diğer 3üncü parti sitelere ulaştırmaktadırlar.

Bunu engellemek için UblockOrigin eklentisi ilk adımı atmıştır. first party'de olsa ilgili request'leri engelleyebilmektedir. fakat eklenti altyapısı gereği bu özellik sadece Firefox'ta desteklenebilmektedir. kaynak: (source-id: 203)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# HTML için "Object" ve "embed" tag'leri
iki tag'de aynı görevi görmektedir. örnek kullanım:

```html
<object type="application/pdf" data="filename.pdf" width="100%" height="100%">
</object>
```

bu şekilde tarayıcının eklentileri çağrılabilir durumda olmaktadır. örneğin PDF, WSF (flash) gibi. tarayıcı "data" da belirtilen dosya uzantısına göre yada "type" ile belirtilen mime-type'a göre eklentiyi devreye sokar. son kullanıcı bu dosyaların neyle açılacağını tarayıcının özelliklerinden set etmiş olmalıdır. örneğin bir video açılacakken tarayıcının embed video görüntüleyicisi de açılabilir, VLC'nin tarayıcıya kurduğu eklentide açılabilir.

HMTL5'te "embeed" tag'ı ile açılan video oynatıcının teması neredeyse tümüyle değiştirilebilir durumdadır. böylece herkes kendi oynatıcısını tasarlayabilmektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Browsers and their engines

# internet explorer version history
Burada birçok versiyon için minimum gereksinimler ve hangi işletim sistemine kurulabileceği detaylı şekilde yazmaktadır: (source-id: 204)

Burada ise daha eski versiyonlar için detaylı bir tablo mevcut: (source-id: 205) "Release history for desktop Microsoft Windows OS version" başlığı ikinci tablo.

# Edge version history
Detaylı bir tablo burada mevcut: (source-id: 206) "Release history" başlığındaki tablo (Wikipedia sayfasındaki bu tabloyu görüntülemek için tabloda "show" seçeneğini seçmek gerekiyor).

Edge Microsoft Windows'ta default yüklü gelmeyen OS'lara da kurulabiliyor. kaynak: (source-id: 207)

Birkaç versiyon:

| version  | engine            | public release date | comment                              |
|----------|-------------------|---------------------|--------------------------------------|
| 20.10240 | EdgeHTML 12.10240 | July 15, 2015       | first public release                 |
| 44.19041 | EdgeHTML 18.19041 | 2020                | latest version before Blink engine   |
| 79.0.309 | Blink 79          | January 15, 2020    | first public release of Blink engine |
| 81.0.416 | Blink 81          | April 13, 2020      |                                      |

# browser and engines

| NAME                                                                                         | JS ENGINE         | BROWSER ENGINE     |
|----------------------------------------------------------------------------------------------|-------------------|--------------------|
| Internet Explorer (11. sürümde geliştirmesi durduruldu. Artık Edge tarayıcısı kullanılıyor.) | ?                 | Trident(or MSHTML) |
| Microsoft Edge (or Spartan) (2018'in sonlarına kadar ki sürümler)                             | Chakra            | EdgeHTML           |
| Microsoft edge (or Spartan) (2020'den sonraki sürümler) (2)                                  | v8                | Blink              |
| Firefox                                                                                      | SpiderMonkey      | Gecko              |
| safari                                                                                       | Nitro*(1)         | WebKit             |
| Google Chrome                                                                                | v8 (or Chrome V8) | Blink              |
| Opera (14. sürüme kadar)                                                                     | Carakan           | Presto             |
| Opera (14. sürümden sonra)                                                                   | v8                | Blink              |
| yandex browser                                                                               | v8                | Blink              |

- (1) Safari Js Engine
  - WebKit, JavaScriptCore ve Webcore olarak 2 temel projeden oluşuyor.
  - JavaScriptCore, WebKit projesinin bir alt projesidir.
  - JavaScriptCore tekrardan birçok kere optimize edildi. Sırası ile yeni isimleri: SquirrelFish --> SquirrelFish Extreme (or SFX or Nitro)

- (2) Chromium tabanlı tarayıcı 2019'un başlarında developer kanallarından sunulmaya başladı. ilk public release 2020'nin başlarında çıktı.

# NodeJS
V8 tabanlıdır.

# SunSpider
benchmark(kıyaslama) tool that measure JavaScript performance.

# Netscape
Netscape firması tarafından geliştirilen tarayıcı. daha sonra açık kaynaklı oldu ve Mozilla kurumu ile geliştirilmeye devam edildi. fakat Mozilla daha sonra sıfırdan tarayıcı yazdı.

# Aurora
Firefox'un beta ile nightly version arasındaki sürümü.

# Firefox Focus
Firefox'un sürekli gizli modda açılan ve gömülü adblocker içeren versiyonu. lisans sorunları sebebi ile; almanca sürümlerinde "Firefox Klar" olarak adlandırılmaktadır.

# Firebug
Firefox'un içinde gelen default developer tool'un alternatifi açık kaynaklı bir eklentidir. Artık geliştirilmiyor.

# IceCat (or old-name:IceWeasel)
gnu projesi kapsamında Firefox'un  içindeki lisanslı paketlerin çıkarılarak yaratılan fork'udur. içerisinde varsayılan olarak eklentiler barındırıyor.

# Fennec F-Droid
Firefox, F-Droid gibi marketlerde yer almamaktadır ve public FTP indirme sunucusu sunmamaktadır. bu sebeple community, Firefox'un kodunu sürekli derleyerek F-Droid mağazasında bu isimde dağıtmaktadır.

# Waterfox
Düzenli olarak Firefox'tan fork olan proje. telemetrilerin disable edildiği, çok temel isteklerin yapıldığı proje.

# Palemoon
Firefox tabanlı fakat Firefox'u düzenli olarak fork etmeyen bir proje.

# GeckoView
Mozilla'nın resmi olarak geliştirdiği Android için Webview'dır.

- Firefox Preview (stabil sürüme çıktığında, normal "Firefox" olarak kullanılıyor)
- Firefox Reality
- Firefox Focus

uygulamaları bunu kullanarak geliştirilmektedir.

# Fennec
Firefox'un tüm mobile versiyonları için kullanılan resmi bir takma isimdir.

# Firefox Preview
Mozilla firması Android için Firefox'u sıfırdan tasarlamaktadır. proje var olan Firefox'un Android versiyonuna paralel yapılmaktadır.

projenin kod adı: Fenix

bu paket stabil versiyonuna ulaştığı için kaldırıldı. artık Google play store'dan Firefox'un stabil sürümü indirildiğinde bu güncelleme zaten geliyor. Fakat proje kodlarında artık Fennec yerine, Fenix kod adı kullanılmaktadır.

# Brave Browser
chromium tabanlı web tarayıcısı. fingerprint korumasında çok başarılı.

# mullvad browser
firefox'u upstream olarak kullanıyor. ek olarak tor browser'ın kullandığı tüm fingerprint ayarlarını patch olarak geçiriyor.

tor browser'dan farkı sadece tor-network'e bağlanması için gerekli ayarların yapılmaması. projenin geliştiricileri tor ve mullvad (privacyguides tarafından önerilen VPN firması).

# libreworlf
firefox'u upstream olarak kullanıyor. ek olarak default'ta; ublok-origin gibi eklentileri ve Arkenfox (user-script) config'lerini ekliyorlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# submit request vs Ajax
Bu konu başlığı da okunması faydalı olacaktır: [file:"java.md" - title:"JSP ile single page application"](./java.md#jsp-ile-single-page-application)

sunucu taraf isteğin submit mi yoksa sadece JS ile yollanmış Ajax isteği mi bilmiyor.

Sunucu, client'ın yapacağı bir Javascript-Ajax isteğinde sayfayı yönlendiremez. Javascript tarafında, Ajax sonrası programatik olarak sayfa URL'sini değiştirmek gerekir. Oysa submit işleminde buna gerek yok. sunucu, submit işlemi sonrası HTML döner. bu HTML direk olarak web tarayıcısında gösterilir. tabi web tarayıcısında artık istek yapılan URL vardır. Eğer son kullanıcı bu son durumda sayfayı refresh etmek isterse, aynı submit isteği aynı sekmede tekrarlanır. Submit işleminde kritik bilgi barınabileceğinden, web tarayıcısı güvenlik gereği popup'ta soru sorar.

```html
<form method="post" action="/login">

  <input type="text" name="name" />
  <input type="text" name="surname" />

  <input type="submit">Submit</input>
</form>
```

- HTML standartlarında form içinde form olmaması gerektiği belirtilmektedir. kaynak: "Example of a form" başlığında (source-id: 208)

- form submit default'ta GET'tir. eğer method="post" yazarsak POST olur.

- örnekteki "surname" ve "name" attribute'leri submission method GET ise URL'de sona eklenir (query param olarak eklenir). örnek:

  ```
  www.currentdomainofwebpage.com/login?name=Ahmet&surname=Kemal
  ```

  eğer method POST ise o zaman body'ye eklenir. tabi body olduğu için "enctype" alanı browser tarafından otomatik doldurulur veya form elementine developer ne yollanacağını belirtir:

  ```html
  <form
    action="/url"
    method="post"
    enctype="multipart/form-data">
  ```

- Birçok HTTP GUI Client (Postman gibi) form POST işlemlerini, "Form Data" diye özel bir kısımda gösterip son kullanıcıya doldurtur. Bu tamamen GUI'nin verdiği özel bir isimlendirme ve arayüzdür.

- form element can take "enctype":

  - __application/x-www-form-urlencoded__

    URL-encoded şekilde form elementleri body'de yollar:

    > name=jack&ag=30

  - __multipart/form-data__

    dosya yüklemesi yapıldığında bu kullanılmak zorundadır.(kullanmak için dosya yüklemesi olamk zorunda diildir)

  - __text/plain__

    A new form type introduced in HTML5. simply sends the data without any encoding.

- URL standartlarında boşluk karakteri yerine + karakterini koymanın tanımı yoktur. Boşluk karakteri yerine farklı bir karakter kullanmak gerektiği belirtilmiştir. kaynak: (source-id: 209).

  Fakat web standartlarında URL'de boşluk yerine, + koyulmalıdır. kaynak: (source-id: 210) "application/x-www-form-urlencoded" başlığı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

- hash mark (or #)

- hash-bang (or #!)

- ! (or bang or ünlem işareti or Exclamation Mark)

- Fragment
kelime anlamı: parça

- URL'lerde sadece hash işareti ile linkler sayfadaki farklı bir yere yönlendirmek için (sunucuya istek olmadan) kullanılır. # ile yönlendirilen sayfalar "fragment identifier" aracılığı ile sayfanın hangi kısmına gideceğini bilirler.


- _escaped_fragment_=
Bu string Google (ve diğer) arama robotları tarafından HTML sayfalarını gezerken kullanılmaktadır. Arama için index'lenecek sayfa URL'sinde hash-bank var ise; ! işareti kısmını bu string ile değiştiriyor. Bu şekilde gidilen sayfa bu keyword'ü okuyor ve eğer bu keyword var ise static (içeride Ajax işlemi yapmayan, robotlar Javascript çalıştırır) bir HTML sayfası döndürüyor. Çünkü arama motorları dinamik sayfaları çalıştıramaz.

- \<meta name="fragment" content="!"> etiketi Ajax içeren sayfalara koyulur. Eğer robot bu sayfaya gelirse bu etiketi görünce aynı URL'ye _escaped_fragment_= ekleyip yeni URL'ye gider.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# HTML5 yenilikler

- table footer gibi div yerine kullanılacak hazır tasarım şablonları

- video ve audio elementleri geldi. bunlarla flash yada eklenti ihtiyacı kalktı.

- bazı elementler ve attribute'ler çıkarıldı, ve yenileri eklendi.

- form elementler için tipler geldi.

- canvas

- offline web app (web-cache or app-cache): cache mekanizması. artık manifest dosyaları ile hangi dosyaların tamamen offline hangilerinin ise kesinlikle internetten çekilebileceği belirtilebiliyor. \<html manifest="cache.apache"> ile dosya verilmelidir. web cache daha sonra standartlardan kaldırıldı. HTML5 öncesi tarayıcılarda hiçbir şekilde cache mekanizması yoktu. ya özel Javascript kütüphaneleri ile kendi cache mekanizmaları yazılıyordu, yada web tarayıcıları hiçbir standarta bağlı olmadan kendilerince cache mekanizması uyguluyorlardı. bu sebeple bazı yazılımcılar web geliştirmesi yaparken bilinçsizce cache temizliği yaparlar.

- drag and drop

- history API: artık URL'lerde hash # tag'i kullanımı yerine direk olarak HTML 5'in getirdiği history kullanılmaktadır. HTML 5 ile artık URL değişmeden sayfanın yenilenmesi de sağlanabiliyor.

  HTML5 öncesi sayfa iki şekilde değişebiliyordu: 1-hashtag vs 2-URL ile gidilen yeni sayfa.

  HTML5 ile gelen history mekanizmasında hashtag ihtiyacı kalkmıştır.

  ```js
  // state objesi 640 bin karakterlik limiti var. çok büyük data'lar geçilmemelidir.
  let stateObj = {
    foo: "bar",
  }

  window.history.pushState(stateObj, "page 2", "bar.html");
  ```

  Yeni sayfa aynı URL olmak zorunda değil. Aynı URL olmadığında da sayfanın state'i yenilendiği için hasHtag'lere gerek kalmamıştır. parametreler:

  - stateobj: yenilenen sayfadan event listener ile bu parametre okunabilmektedir.
  - "page 2": title. bu değer ignore ediliyor. boş gönderilmesi öneriliyor.
  - "bar.html": relative path olmalıdır. çünkü ancak; aynı domain'de history olabilir. eğer boş yollanırsa, URL hiç değişmez fakat sayfa değişmiş olarak algılanır.

  pushState fonksiyonu çağrılınca server tarafa HTTP GET isteği yollanmıyor. Hashtag'lerde de böyleydi. Sadece sanal bir history yaratılıyor. Eğer sunucuya istek atmak istiyorsak; şöyle yapabiliriz:

  ```js
  window.onpopstate = function(event) {
      if(event && event.state) {
          location.reload();
      }
  }
  ```

- web messaging: iframe içerisinde event listenerlarla (message event'i ile) data dönderimi ve alımı yapılıyor.

- Web Worker: JS thread mekanizması. aksi durumlarda tarayıcı son kullanıcıya "not responding" mesajı verebiliyor.

- WebSocket (başka başlıkta anlatılıyor)

- WebRTC: tarayıcı ile tarayıcı arasında direk iletişim protokolü.

- geolocation: lokasyon tespiti için API.

  Her istek için şu bilgileri döndürüyor:

  - coords.latitude - enlem. decimal number
  - coords.longitude - boylam. decimal number
  - coords.accuracy - sadece enlem ve boylam için doğruluk oranı.
  - coords.altitude - rakım. (deniz seviyesinden yükseklik).
  - coords.altitudeAccuracy - rakım için doğruluk oranı.
  - coords.heading - rota (eğer cihaz hareket halinde ise). as degrees clockwise from North.
  - coords.speed - The speed in meters per second
  - timestamp - The date/time of the response

- WebSQL database: sorgu yapmayı sağlayan database. bu artık deprecated oldu.

- IndexedDB: NoSQL gibi düşünülebilir. LocalStorage küçük data'lar, IndexedDB büyük data'lar için tercih edilmelidir (zorunluluk değil).

- Web storage (local storage + session storage): bazı kaynaklarda "__DOM storage__" olarakta geçer. anahtar - value mantığı ile çalışır.

# cookie vs web storage
- Web storage API'si çok daha temizdir. cookie'lerde metadata'lar eklenince string merge işlemine girmek durumundasınız.
- web storage'ın boyutu cookie'lere göre çok daha fazladır.
- cookie'lerin oto-destroy özelliği vardır (time-to-live). web storage'de böyle bir özellik yoktur.
- cookie'ler header'a otomatik eklenebilir. web storage'da böyle bir feature yoktur. Asıl olan bu maddedir. Bu maddeye göre hangi storage'yi kullanacağımıza karar vermeliyiz (çok nadir durumlarda bunun istisnası olabilir tabi).

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# communication types

# dublex (or tr:dubleks)
- __full dublex__

  aynı anda hem data gönderip hem alabildiğimiz ileişim yönlemleridir. örnekler:
  - WebSocket
  - klasik telefon konuşmaları
  - whatsapp, facebook gibi sistemlerdeki kameralı veya sesli haberleşme yöntemleri
  - TCP (Ek not: TCP API olarak full-dublex'tir. Fakat onu ulaştırmak için kullandığımız fiziksel teknoloji full-dublex olmayabilir. Bunu şuna benzetebiliriz: Multi-thread uygulamalarda başlattığımız thread, her zaman OS tarafından o anda paralel işletilmeyebilir.)

- __half dublex (or semiduplex)__

  Her iki taraf birbirine mesaj atıp alabilir, fakat T anında sadece tek yönlü data transferi yapılabilir. diğer yönlü data transferi başlarsa, diğer yön transfer yapamaz. örnekler:
  - bas-konuş (or push-to-talk)

# Simplex
Sadece tek taraflı iletişime izin veren yöntemlerdir. örnekler:
- Televizyon yayını
- radyo yayını
- Network'te attığımız broadcast paketleri
- bebek izleme kameraları
- araba garaj kapısı açacağı
- wireles mikrofon (sadece mikrofon kısmı. kulkalığı kapsamayacak şekilde düşünelim)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# web 1.0 vs web 2.0 vs web 3.0
web dünyasını belirleyen standartlardır. web 1.0 sadece HTML'in olduğu fakat Ajax işleminin dahi gerçekleşmediği web standartlarını temsil eder. 2.0 ise 2003'ten itibaren olan tüm standartların tümünü temsil eder. 3.0 ise Ethereum gibi smart-contract'larla çalışan web sayfalarıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# canvas (or Tuval) vs svg (or Scalable Vector Graphics)
svg her çözünürlüğe uygundur.

# svg örneği:
```xml
<svg width="100" height="100">
  <circle cx="50" cy="50" r="40" stroke="green" stroke-width="4" fill="yellow" />
</svg>
```

# canvas örneği:

```js
<canvas id="myCanvas" width="200" height="100" style="border:1px solid #000000;"></canvas>

var c = document.getElementById("myCanvas");

var ctx = c.getContext("2d");

ctx.fillStyle = "red";

ctx.fillRect(0,0,150,75);
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# SASS
Sass, CSS'i bir programlama diline benzer yapıyla geliştirmemizi sağlayan, sürekli ihtiyaç duyduğumuz ve CSS'te bulunmayan birçok özelliği kullanarak, daha pratik ve okunaklı kod yazmamıza olanak verir.

Sass ile CSS yazarken; değişkenler (variables), döngüler (for, each, while), karar yapıları (if, else), fonksiyonlar kullanılabilir.

SCSS ve SASS iki farklı syntax sunan formata sahiptir. bu kodlar bir tool aracılığı ile derlenerek CSS formatı oluşturulur.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Mustache vs underscore vs lodash
Mustache ve underscore, Javascript kütüphanesi de hazırlanan template dosyalarını verilen model ile HTML'e render etmeye yaramaktadır.

underscore ek olarak JQuery gibi fonksiyonel fakat DOM'dan bağımsız metotlar içerir. kolay döngüler kurar vs...

lodash; underscore'un template engine'i dışındaki metotlarına alternatif sunan açık kaynaklı kütüphane. underscore'dan daha fazla özellik içeriyor.

Ecmascript'in 6'ıncı versiyonu ile artık underscore veya lodash ile yapılan %80 özellik default olarak sağlanabiliyor. underscore ve lodash'in kullanılmasının sebebi eski tarayıcılarda da aynı metotların desteklenmesi ve %20'luk Ecmascript'te olmayan fonksiyonların kullanılmasıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# önyüz için MVC karşılaştırması
Burada birçok önyüz framework'leri için basit TODO uygulaması mevcut: (source-id: 211)

# Knockout js
Javascript MVC kütüphanesidir.

# Backbone js
Javascript MVC kütüphanesidir.. fakat Angular ve knockout'a göre core bir framework'tür. herşeyi kendi başınıza tasarlamanız gerekir.

Backbone controller isminde bir sınıf içermez. bu sebeple bazı yerlerde MV bazlı bir kütüphane olduğu yazar. fakat backbone MVC'dir. çünkü backbone-View objesi aslında, MVC'deki controller'ın görevini karşılamaktadır. MVC'deki, View'ı ise; backbone-template'ler denk gelmektedir. Template'ler backbone-View içerisinde oluşturululduğundan yanlış algılamaya sebep olabilmektedir.

View'daki event'ler takip edilir, her event sonrası metotlar çalıştırılır model'ler update edilir. modeller update edilince view'lar update edilir. Burada ek olarak modeller update olduğunda, otomatik olarak web servislerine istek gider. bu şekilde sunucudaki model ile sync olmamızı sağlar. Buradaki (source-id: 212) "Models and Views" başlığındaki ilk şekil bu durumu iyi açıklamaktadır.

Buradaki örnek çok temel ve basit: (source-id: 213)

Çok basit bir örnek:

```html
<!DOCTYPE html>
<html>

    <head>
        <meta charset="utf-8" />
        <title>Your first Backbone.js App</title>

        <!-- The main CSS file -->
        <link href="assets/css/style.css" rel="stylesheet" />

    </head>

    <body>

        <form id="main" method="post" action="submit.php">
            <h1>My Services</h1>

            <ul id="services">
                <!-- The services will be inserted here -->
            </ul>

            <p id="total">total: <span>$0</span></p>

            <input type="submit" id="order" value="Order" />

        </form>

        <!-- JavaScript Includes -->
        <script src="https://ajax.Googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
        <script src="//cdnjs.cloudflare.com/ajax/libs/underscore.js/1.4.4/underscore-min.js"></script>
        <script src="//cdnjs.cloudflare.com/ajax/libs/backbone.js/1.0.0/backbone-min.js"></script>

        <script src="assets/js/script.js"></script>

    </body>
</html>
```

Yukarıdaki sayfa ile birlikte bu JS dosyasını çalıştırdığımıza sayfa içerisindeki HTML dolacaktır.

```js
$(function(){

    var Service = Backbone.Model.extend({

        defaults:{
            title: 'My service',
            price: 100,
            checked: false
        },

        toggle: function(){
            this.set('checked', !this.get('checked'));
        }
    });

    var ServiceList = Backbone.Collection.extend({

        model: Service,

        getChecked: function(){
            // where backbone'un özel bir metotudur. bunun gbi birçok metot sunar.
            return this.where({checked:true});
        }
    });

    var services = new ServiceList([
        new Service({ title: 'web development', price: 200}),
        new Service({ title: 'web design', price: 250}),
        new Service({ title: 'photography', price: 100}),
        new Service({ title: 'coffee drinking', price: 10})
    ]);

    var ServiceView = Backbone.View.extend({
        tagName: 'li',

        events:{
            'click': 'toggleService'
        },

        initialize: function(){
            this.listenTo(this.model, 'change', this.render);
        },

        render: function(){

            // Create the HTML

            this.$el.html('<input type="checkbox" value="1" name="' + this.model.get('title') + '" /> ' + this.model.get('title') + '<span>$' + this.model.get('price') + '</span>');
            this.$('input').prop('checked', this.model.get('checked'));

            return this;
        },

        toggleService: function(){
            this.model.toggle();
        }
    });

    // The main view of the application
    var App = Backbone.View.extend({

        // already  existing element on HTML
        el: $('#main'),

        initialize: function(){

            // Cache these selectors
            this.total = $('#total span');
            this.list = $('#services');

            this.listenTo(services, 'change', this.render);

            services.each(function(service){

                var view = new ServiceView({ model: service });
                this.list.append(view.render().el);

            }, this); // "this" is the context in the callback
        },

        render: function(){

            // Calculate the total order amount by agregating
            // the prices of only the checked elements

            var total = 0;

            _.each(services.getChecked(), function(elem){
                total += elem.get('price');
            });

            // Update the total price
            this.total.text('$'+total);

            return this;
        }
    });

    new App();

});
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Bootstrap
Bootstrap, UI objeleri yaratmaya yarayan bir kütüphanedir.

"boostrap ui" ise, Angular modülüdür. birçok bootsrap nesnesi Angular ile yazılmıştır. bu şekilde Angular ile daha uyumlu çalışılabilir. Bootstrap, JQuery'ye depend ederi. oysa Bootstrap modülü etmez. bu sebeple projeye JQuery include etme zorunluluğu ortadan kalkar.

# Bootstrap CSS vs BootstrapJS
Bootstrap görselleştirme kütüphanesi. Fakat JS dosyası bazı ek özellikler (genelde animasyon işlemleri) sunmak için isteğe bağlı kullanılabilir. BootstrapJS, JQuery'ye depend eder.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# JQuery vs JQuery mobile vs JQuery ui
JQuery, Javascript için temel metotları içeren kütüphanedir.

JQuery ui ise date-picker, drag-drop gibi görsel bölgelerde düzenlemeler sağlayan kütüphanedir.

JQuery mobile ise, mobile platformlarına uygun davranışlar sergilemesi için gerekli metotları içerir. örneğin mobile'de farklı event'ler varken, desktop'ta farklıdır. JQuery HTML elementlerine verilen attribute'ler ile sayfada nasıl duracağına vs karar verebilir. bu durumlar için mobile'e daha uygun bir kütüphane gerekir. bu sebeple JQuery-mobile kullanılmalıdır.

JQuery eklenti altyapısına sahiptir.

# JQuery version history

| version | release date       | latest version and its release date | note                                                                          |
|---------|--------------------|-------------------------------------|-------------------------------------------------------------------------------|
| 1.0     | August 26, 2006    |                                     | First stable release                                                          |
| 1.1     | January 14, 2007   |                                     |                                                                               |
| 1.2     | September 10, 2007 | 1.2.6                               |                                                                               |
| 1.3     | January 14, 2009   | 1.3.2                               |                                                                               |
| 1.4     | January 14, 2010   | 1.4.4                               |                                                                               |
| 1.5     | January 31, 2011   | 1.5.2                               |                                                                               |
| 1.6     | May 3, 2011        | 1.6.4                               |                                                                               |
| 1.7     | November 3, 2011   | 1.7.2 (March 21, 2012)              | New Event APIs: .on() and .off(), while the old APIs are still supported.     |
| 1.8     | August 9, 2012     | 1.8.3 (November 13, 2012)           |                                                                               |
| 1.9     | January 15, 2013   | 1.9.1 (February 4, 2013)            |                                                                               |
| 1.10    | May 24, 2013       | 1.10.2 (July 3, 2013)               |                                                                               |
| 1.11    | January 24, 2014   | 1.11.3 (April 28, 2015)             |                                                                               |
| 1.12    | January 8, 2016    | 1.12.4 (May 20, 2016)               |                                                                               |
| 2.0     | April 18, 2013     | 2.0.3 (July 3, 2013)                | Dropped IE 6–8 support for performance improvements and reduction in file size |
| 2.1     | January 24, 2014   | 2.1.4 (April 28, 2015)              |                                                                               |
| 2.2     | January 8, 2016    | 2.2.4 (May 20, 2016)                |                                                                               |
| 3.0     | June 9, 2016       | 3.0.0 (June 9, 2016)                |                                                                               |
| 3.1     | July 7, 2016       | 3.1.1 (September 23, 2016)          |                                                                               |
| 3.2     | March 16, 2017     | 3.2.1 (March 20, 2017)              |                                                                               |
| 3.3     | January 19, 2018   | 3.3.1 (January 20, 2018)            |                                                                               |
| 3.4     | April 10, 2019     | 3.4.1 (May 1, 2019)                 |                                                                               |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# AngularJS
DOM manipülasyonları JQuery gibi kütüphanelerle yapılıyor. Fakat MVC kavramını client tarafta uygulamak için Angular gibi kütüphaneler kullanılmaktadır. Angular MVC yapısı ile çalışır.

- #### sürüm

AngularJS, 2inci sürümü ile sıfırdan yazıldı ve yeni bir siteye taşındı (Angular.io). Angular geriye uyumlu yazılmadı.

3üncü sürüm, isim çakışması sebebi ile atlandı. 2'den sonra direk 4 üncü sürüme çıkıldı.

| sürüm no | çıktığı yıl |
|----------|-------------|
| 2        | 2016        |
| 4        | 2017        |
| 5        | 2017        |
| 6        | 2018        |
| 7        | 2018        |
| 8        | 2019        |

aşağıdaki tüm bilgiler Angular 1.6 baz alınarak yazılmıştır.

- #### JQuery

  - AngularJS, JQuery'ye ye depend etmez. kendi içinde JQuery Lite'ı (jqLite) bulundurur. eğer JQuery var ise, Angular jqLite yerine JQuery kullanır. jqLite sadece Angular ihtiyacı metotları ve en temel JQuery metotlarını sunar.

  - fonksiyonlara geçilen "element" objeleri her zaman JQuery objesine wrap edilmiş olarak geçilir. örneğin;


  ```js
  angular.module('myModule', [])

    .directive('failswithoutjquery', function() {

      return {

        restrict : 'A',

        link : function(scope, element, attrs) {

                 element.hide(4000);
               }
      }
  });
  ```

  Yukarıdaki örnekte element objesi JQuery objesi. hide elementi ise JQuery lite'ta yok. bu sebeple eğer JQuery inject edilmemişse bu kod bloğu hata verecektir.

  Angular.element DOM objesinin element (JQuery ile wrap edilmiş) olarak dönmektedir.

- #### modülerlik

  AngularJS eklenti desteği mevcut değil. çünkü her şey bir Angular modülü olarak tanımlanabiliyor ve başka modüller tarafından çağrılabiliyor. örneğin routing ayrı bir modüldür ve ayrı inject edilir.

  yazılım geliştirici yeni modüller ekler. bir sayfada birden fazla modül olabilir. her modülün altında birden fazla component, directive ve controller olabilir.

- #### Component

kullanımı:

```xml
<w3-test-directive></w3-test-directive>
```

yada

```xml
<div w3-test-directive></div>
```

yada

```xml
<div class="w3-test-directive"></div>
```

yada

```xml
<!-- directive: w3-test-directive -->
```

tanımlanması:

```js
var app = angular.module("myApp", []);

app.directive("w3TestDirective", function() {

    return {

        template : "<h1>Made by a directive!</h1>"

 };
});
```

- #### watch variables

two-way binding olduğundan; herhangi bir dinamik değer değiştirdiğinde onu referans eden tüm değerlerde anında değişmektedir. fakat Javascript'te bir değer console'dan da değiştirilebilir. bunu Angular nasıl anlıyor?

Angular bunları  anlayamıyor. bu sebeple click metotu yerine ng-click gibi alternatifler mevcut. çünkü her Angular standardındaki metotların çağrılması sonrası tüm değerler manuel kontrol ediliyor. yani console'dan, daya on-click event'i üzerinden bir değer değiştirildiğinde Angular bunu göremiyor.

bu sebeple şöyle bir sorun da ortaya çıkıyor:

controller içinde async bir thread üzerinden (timeout, Ajax call, Cordova plugin'in return callback'leri gibi) bir dinamik değer değiştirildiğinde, controller'ın main thread'inden sonra çalışırsa controller'ın haberi olmuyor. bu sebeple $scope.apply() metotu geliştirilmiştir. bu metotu çağrıldığında tüm dinamik değerler Angular tarafından tekrar kontrol edilmektedir. örneğin; Ajax-callback thread'in sonunda apply() metotu çağrılabilir.

Angular $http modülü Ajax işlemlerini yapmak için geliştirilmiştir. bu metotun callback'lerinde otomatik olarak apply metotu çağrılır. bu sebeple yazılımcının bu metotu çağırmasına gerek yoktur. fakat JQuery.Ajax yada Javascript core Ajax işlemi yapıyorsa apply metotu şarttır.

apply() metotu isteğe bağlı bir metot parametresi de alabilir. bu metot parametresi içindeki fonksiyon çalışmasını bitirdiğinde apply işlemini gerçekleştirir.

- #### watch metotu

```js
$scope.myVar= "estVar";

$scope.$watch('myVar', function() {

      alert('hey, myVar has changed!');

});
```

$watch metotunda apply() işlemi sırasında değişen variable'ler için event yaratabilmemizi sağlıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# HTTP Basic Authentication
HTTP ile istek yaparken auth mekanizmasının en kolay ve standartlaşmış şeklidir. sadece request'in header'ınına base64 tipinde username ve password bilgisi girilir. yapılan HTTP isteğine eğer bilgiler doğru ise cevap dönülür. Standart bir güvenlik şekli olduğundan, bazı web tarayıcıları bunu destekler ve popup'ta şifre ve kullanıcı adı sorar.

Basic auth işlemi her HTTP request'te yapılır. yani header'a sadece ekleme yapılır. web tarayıcıları lokalde bir site için cookie olarak saklar request ile giden değeri. bu sebeple her defasında sormaz username ve şifreyi. aynı şekilde örneği Java'da client objesi ile birçok istek yapabiliyoruz. aynı obje ile bir kere isteğin kimliği doğrulandıktan sonra cookie'leri client objesi tutuyor ise bir daha request gerekmeyebilir.

# HTTP Digest authentication
Diggest auth'ta, önce, client sunucuya güvenlik sorgusundan geçeceğini belirten bir istek yapar. Bu isteğe cevap olarak sunucu, tek kullanımlık bir anahtar değer döndürür. Client tarafı hem bu değeri, hemde MD5 algoritmasını kullanarak şifreleyip sunucuya gönderir (ek olarak; client tarafı da bir tek kullanımlık anahtar + URL adresi de şifrelemede kullanır...). Sunucu kullanıcı adı ve şifre ile aynı algoritmayı kendi uygular ve sonucun aynı olup olmadığına bakarak onay verir.

MD5 geri çevrilemez bir algoritma olduğu için güvenlik olarak basit auth'tan daha başarılır.

standart bir güvenlik şekli olduğundan, bazı web tarayıcıları bunu destekler ve popup'ta şifre ve kullanıcı adı sorar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# WebGL
Tarayıcılarda plugin gerektirmeden 3d ve 2d oyunlar, nesneler yaratılmasını sağlayan Javascript API'sidir. OpenGL ES altyapısını kullanmaktadır.

# OpenGL
AÇık kaynaklı DirectX alternatifidir.

# OpenGL ES (or OpenGL for Embedded Systems)
OpenGL'nin cep telefonu gibi gömülü sistemler için tasarlanmış versiyonudur.

# NativeClient (or NaCl)
AÇık kaynaklı geliştirilen API. Bu API ile eklentiye gereksinim olmadan WebGL gibi 3d ve 2d modeller hazırlanabiliyor. Tek farkı herhangi bir programlama dili ile yazılıp derlenmiş olan dosyanın tarayıcı tarafından (derlenmeye gerek kalmadan) direk olarak execute edilebilmesidir.Performans ve basitlik (Javascript ile yazmak yerine diğer dillerle de yazabilme) açısından WebGL'den çok daha başarılıdır. NaCl, PPAPI destekli bir eklentidir.

# Plugin Application Programming Interface (or PAPI)
Tarayıcıların native işletim sisteminden yararlanması için tarayıcı tarafındaki API.

# Netscape Plugin Application Programming Interface (or NPAPI)
Birçok tarayıcı desteklemektedir, ilk Netscape çıkardığı için ismi böyle kalmıştır. Artık NPAPI özelliği çoğu tarayıcıdan kaldırılmaktadır.

# Pepper Plugin API (PPAPI)
Google'ın geliştirdiği, NPAPI'nin türevidir. Daha çok portabilite ve  işletim sistemi process'inde çalışmaya yönelik geliştirmeler içermektedir.

Chromium based browser's built-in PDF viewer is a plug-in which based on PPAPI.

# ActiveX
Microsoft'un Internet Explorer tarayıcısı için geliştirdiği PAPI. Microsoft Edge (Microsoft Windows 10 ile gelen yeni tarayıcı), ActiveX'i desteklememektedir.

# Silverlight
Adobe Flash alternatifi açık kaynaklı Microsoft'un geliştirdiği yazılım. Lisans sebebi ile Silverlight kullanmayanlar projeyi çatallayarak Moonlight projesini oluşturmuştur. Fakat bu 2 projeninde geliştirilmesi durdurulmuştur.

# WebExtensions
Firefox 57inci sürümü ile zorunlu kıldığı yeni eklenti API'sinin ismidir. Bu API Chromium tabanlı tüm tarayıcılarda (örnek: Opera) da kullanıldığı için cross-browser eklenti yazılmasını sağlamaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# REST (or Representational State Transfer) vs RESTful
Representational kelime anlamı: temsili.

"Representational" kelimesi sunucu tarafta manipüle ettiğimiz kaynakları (state'leri) temsili değerlerle (genelde bir çeşit ID ile) yönetmemizden gelir.

__REST__ yazılım haberleşme tekniği/mimarisidir. "__RESTful__" ise, REST'i kullanan sistemler (uygulamalar) için kullanılır.

REST, 200 yılında "Roy Thomas Fielding" tarafından bir doktora tezinde ortaya atılmıştır.

REST bir protokol değildir. REST bir protokol mimarisidir. temelde şu kuralların olmasına dayanır:

- client-server

  sistem, client'ın herhangi bir platformda çalışabilecek şekilde tasarlanmış olmalıdır.

- stateless server

  request'lerde sadece client taraf state(context) ve session bilgisi tutmalıdır. sunucuda tutulmaz.

- Uniform interface

  her resource için CRUD (Create, update, delete...) metotları hazır olmalıdır.

- cacheable

  client veya aradaki proxy'ler cache'leme yapabilmelidir. her isteğin cacheable olup olmadığı her daim net bir şekilde belirtilmelidir.

- layered system

  sunucularımız birçok katmandan meydana geliyor olabilir. load balancer, proxy, cache, ayrı microservisler... bunlar client tarafından bilinmemesi gereklidir. client tek bir endpoint'den tüm ihtiyaçlarını giderebilmelidir.

- Code-On-Demand (optional)

  server, client'a execute etmesi için kod yollayabilir. örnekler: Javascript kodu yada Java-Applet'leri yollayabilir.

Eğer request/response'larımız bu kurallara uymuyorsa, sistem __RESTless__'dır.

Rest yapısı HTTP ile ortaya atılmıştır ve HTTP ile çok uyumludur. bu sebeple piyasada Rest kelimesi denilince herkesin aklına sadece HTTP gelmektedir. oysa bu doğru değildir.

REST mimarisinde geçen terimler ve buna denk gelen karşılıkları:

| Data Element            | Modern Web Examples                                     |
|-------------------------|---------------------------------------------------------|
| resource                | the intended conceptual target of a hypertext reference |
| resource identifier     | URL, URN                                                |
| representation          | HTML document, JPEG image                               |
| representation metadata | media type, last-modified time                          |
| resource metadata       | source link, alternates, vary                           |
| control data            | if-modified-since, cache-control                        |

# REST vs SOAP (or Simple Object Access Protocol)
SOAP'ın yeni spesifikasyonlarında kısaltma olduğu bilgisi kaldırıldı ve sadece özel isim olarak kullanılmasına karar verildi.

SOAP, mesajlaşma standartıdır. bu standart mesjaın nasıl karşıya gönderileceğine hiç karışmaz. Piyasanın büyük çoğunluğunda HTTP POST isteğinin body'sinde yollanır. Eskiden SMTP protokolü içerisinde kullanılıyordu. Güncel olarak ise Microsoft WebSocket'ler üzerinde sub-protokol olarak kullanıyor "SOAP Over WebSocket Protocol Binding (MS-SWSB)".

SOAP bir protokoldür, oysa REST, protokollerde kullanılan bir tekniktir. Bu sebeple karşılaştırılması doğru değildir. Fakat piyasada REST, sadece HTTP olarak algılandığı için bunun karşılaştırılması yapılıyor.

İstenirse JWT'nin tuttuğu bilgiler SOAP isteğinde tutulur ve SOAP mimarisi ile RESTful bir sistem yapılır. Fakat piyasada kimse bunu tercih etmez. çünkü best practice olarak zaten RESTful sisteme, HTTP request'leri çok uygundur.

SOAP body'si XML olmak zorundadır. Bu XML'in root'undaki element "Envelope"'tur. içinde "header" ve "body" vardır.

SOAP'ın farklı versiyonları var:
- 1.0 - year: 1999 - standart olarak kabul görmedi.
- 1.1 - year: 2000 - standart olarak kabul görmedi.
- 1.2 - year: 2001 gibi sunuldu, güncellenerek 2007'de kabul gördü. - standart olarak kabul gördü.

# SOAP with Attachments
Saf SOAP protokolü binary bilgi (attachment) atmayı desteklemiyor. Fakat ek bir feature standardı ile desteklenebiliyor: https://www.w3.org/TR/soap12-af/ ("__SOAP 1.2 Attachment Feature__")

"__SOAP 1.2 Attachment Feature__" ile __SOAPMessage__'ın direk yanına ek olarak attachment'lar eklenebiliyor. Bu attachment'ların olduğu kısım "__SecondaryPartBag__" olarak isimlendiriliyor.

Java dünyasında __SAAJ (or SOAP with Attachments API for Java)__ ile SOAP ve __SOAP with Attachment Feature__ kullanılabiliyor.

__SAAJ__, __JAX-WS__'in arkaplanda kullandığı kütüphanedir. source: https://javaee.github.io/metro-saaj/ 1st paragraph.

# raw of HTTP request
bir HTTP isteği TCP katmanında incelendiğinde şu şekilde görünmektedir:

```
GET /wx/in/kanpur/wx.php HTTP/1.0
Host: cdn.sstatic.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64)
Accept: text/html
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Connection: keep-alive

```

Görüldüğü gibi kompleks bir formatı yoktur. Her bilgi farklı satırda yollanır. En sonda bilginin bittiğine dair fazladan bir satır sonu da olmalıdır.

TCP seviyesindeki görüntüsünü Insomnia HTTP GUI client'ında rquest'i attıktan sonra "Console" sekmesinden görebiliriz. Veya Insomnia'da isteği seçip "generade code" seçeneğini seçip, programlama dilleri arasından "HTTP" yi seçersek raw datayı görebiliriz.

örneğin nc (netcat) komutu ile TCP üzerinden raw isteği yollayabilir:

```sh
nc "www.Google.com" "80"
```

komut satırı input bekleyecektir. aşağıdakiler girildiğinde karşı tarafa bilgi yollanacaktır:

```
GET /index HTTP/1.0
Accept: text/html
```

İki kez enter tuşuna basılıp satır sonu eklendiğinde; response komut satırında ekrana basılacaktır:

```
HTTP/1.0 404 Not Found
Content-Type: text/html; charset=UTF-8
Referrer-Policy: no-referrer
Content-Length: 1566
Date: Sat, 23 Mar 2019 10:11:18 GMT

<!DOCTYPE html>
<html lang=en>
  <meta charset=utf-8>
  <meta name=viewport content="initial-scale=1, minimum-scale=1, width=device-width">
  <title>Error 404 (Not Found)!!1</title>
  <style>
```

Telnet komut satırı ile de aynı durum söz konusudur. örnek:

```sh
telnet "httpbin.org" "80"
```

output:

```
POST /post HTTP/1.1
Host: httpbin.org
Connection: close
Content-type: application/json
Content-length: 11

{"test":true}
```

header'lar ile body arasında bir satır boş olmalı. o satır boşluk dahi içermemeli. eğer boşluk içerirse header'mış gibi algılanır. Spring ile test edildiğinde, eğer log'ları debug'a çekersek her gelen istek detaylı şekilde basılır:

```
logging:
  level:
    root: DEBUG
```

Telnet'ten alınan response direk terminal output'a yansıyacaktır. Eğer TCP connection'ı kapanırsa Telnet komutu sonlanacaktır. fakat TCP sonlanmaz, terminal yeni input beklemeye devam eder.

(source-id: 214) burada belirtildiği gibi client "Connection: close" atarsa sunucunun response sonrası TCP'yi kapatması beklenir. Eğer client atmaz sunucu atarsa connection'ı client kapatır.

Chunked request'lerde (body'nin parça parça yollandığı isteklerde), her chunk size'ı body'den önce ve sonra yollanır. Dolayısı ile Telnet output'undan her body öncesi ve sonrası anlamsız kısa text'ler görebiliriz. bu anlamsız text'ler yollanacak body chunk'ının size'ıdır.

# Telnet vs raw TCP socket
- Telnet RFC'si olan, TCP/IP model'inde en üst katmanında (Application level'da) olan bir protokoldür.

- Telnet'in default server portu 23'tür.

- Telnet'in detayları için birçok RFC var. Fakat asıl RFC 854'tür. kaynak: (source-id: 215) title: "Related RFCs".

- Telnet protokolü basittir. Bazı karakterlerin bazı anlamları vardır. sunucu veya client onları okursa ona göre tepki yapar (bir fonksiyon çalıştırır ve return olarak soketten karşıya döner).

- Telnet'te en ufak birim 8-bittir. kaynak: (source-id: 216) title: "INTRODUCTION".

- Telnet server olmayan bir TCP sunucuya (örnek HTTP server) Telnet client aracılığı ile (HTTP request'ine denk gelen) bir byte-array atarsak, karşılığında hata almayız. Normal server soket response'umuzu (yani HTTP response'u) alırız. Çünkü Telnet client, istediğimiz her bilgiyi karşı tarafa direk yollar. Ekstra yaptığı bir şey yoktur. Karşı tarafta Telnet server olmadığı için normal/saf TCP haberleşmesi gerçekleşmiş olur.

  Fakat burada bazı istisnalar var. örneğin;

  - Eğer server'dan gelen bir karakteri (veya karakter setlerini) bir Telnet komutu olarak algılarsa, o zaman Telnet client farklı bir tepki verebilir. çünkü ilgili komutun gereksinimini yerine getirmelidir.

  - Telnet LF satır sonlarını CR+LF convert eder.

  - Ctrl+] karakterlerini escape karakteri olarak algılar

  - gibi farklı durumlarda çıkabilir...

  Aynı durum Telnet-server içinde geçerli. Telnet server açarsak client Telnet değilse sorun yaşayabiliriz. bu sorunlar yukarıdaki maddelerle aynı sebeplerden kaynaklanacaktır.

  Telnet connection initialize olunca bazı bilgileri karşıya atar. örneğin "terminal GUI size" gibi.. Aslında bu bilgi bile bizim HTTP server'la raw soketten haberleşememizi sağlayacaktır, çünkü HTTP server bize exception atacaktır. Fakat pratikte HTTP server'a Telnet client ile istek atabiliyoruz. Bunun sebebi şu: Telnet komut satırı uygulamaları initialize olunca karşı tarafa environment bilgilerini atmıyor (her komut satırı farklı davranabiliyor). kaynak: (source-id: 217) answer of Mew, at Jan 26 2011, 14:02.

  Netcat (nc) komutu ise direk TCP soket seviyesinde işlem yapar ve hiçbir data'da oynama yapmaz veya anlamlandırmaya çalışmaz. görevi farklıdır. bu sebeple Telnet olmayan sunuculara Telnet komutu ile bağlanmamalıyız. insanlar alışığı için böyle yapıyor fakat bu yanlış.

# Richardson Maturity Model

tam anlamıyla RESTful bir servis yazıyorsak 3 numaralı seviyedeyizdir. REST mimarisini kötü kurguladıysak 0 nolu durumdayızdır.

- Level-0 - Swamp of POX

  swamp kelime anlamı: bataklık.

  POX ise; "Plain Old XML" in kısaltmasıdır.

  REST'i sadece data taşımak için kullanıyoruz. bu seviyedeyken, genelde, SOAP veya RPC benzeri altyapılar kullanıyoruzdur.

- Level-1 - Resources

  istek yaptığım endpoint'ler resource bazlı değişiyor ise bu seviyedeyizdir.

- Level-2 - HTTP Verbs

  kaynaklarıma erişirken post, get, delete gibi  istekleri aynı endpoint için kullanabiliyorsam bu seviyedeyizdir.

- Level-3 - HyperMedia Controls

  servisin dönüş değerleri; client'ın tekrardan yapacağı isteklerin URL'lerini içeriyor ise son seviyedeyizdir. buradaki amaç client tarafta daha az bilgi tutmaktır. örnek: doktor randevu boş saatleri servisten çekildi. dönüş değerinin içinde dolu saatlerin randevu almak için gerekli URL'si de olmalıdır.

  # Hypermedia As The Engine Of Application State (or HATEOAS)

  bu "Richardson Maturity Model" level-3 için kullanılan bir terimdir. client, dinamik olarak sayfanın state'ini (kısmen yada tamamını) network'ten gelen cevaplara/bilgilere göre oluşturmaktadır. bu da level-3 gibi bir sistemde en optimum yapılabilecektir.

  level-3 response örneği:

```json
{
    "name": "Alice",
    "links": [
          {
            "rel": "self",

            "href": "http://localhost:8080/customer/1"
          }, {
            "rel": "customer.search",

            "href": "http://localhost:8080/customer/search"
          } ]
}
```

- "rel" relation kelimesinin kısaltmasıdır.
- "self" dönen bu response için hangi kaynağın çağrılması (request) gerektiğini belirtir.
- bu tarz response veren sistemlere "hypermedia-driven system" da adlandırılır.
- farklı bir örnek:
  mesaj'ı döndüren bir servisimiz olsun. X ID'li mesajı çektik bize şöyle bir cevap dönebilir:

```json
{
    "text": "how are you",
    "links": [
          {
            "rel": "self",

            "href": "http://localhost:8080/message/x"
          }, {
            "rel": "delete",

            "href": "http://localhost:8080/message/delete/x"
          } , {
            "rel": "favorites",

            "href": "http://localhost:8080/message/favorites/x"
            "methods": ["put", "delete"]
          } ]
}
```

HATEOAS için birçok implementasyon mevcut. Bu implementasyonlar bu dosyadaki birçok standardı belirleyebiliyor. örnek:
- links, href, rel yerine ne yazılması gerektiği
- links, href, rel değerlerinin ne ve hangi formatta olacağı
- links, href, rel gibi hangi değerlerin olacağı

Yukarıdaki örnekte mesajımızı silme butonunu, yada favorilere ekleyip ekleme seçeneklerini sunup sunmayacağımıza client tarafta dinamik olarak karar veririz.

# Spring org.springframework.http.ResponseEntity

HATEOAS'u programatik olarak tanımlayabiliyoruz. örnek:

```java
import static org.springframework.web.servlet.MVC.method.annotation.MvcUriComponentsBuilder.fromMethodCall;
import static org.springframework.web.servlet.MVC.method.annotation.MvcUriComponentsBuilder.on;

@PostMapping(value = "/createUser")
public ResponseEntity<Void> createUser(int userId) {

    boolean result = repository.createUser(userId);
    if(result = success){

      URI location = fromMethodCall(on(UsersApiController.class).getUser(userId)).build().toUri();
      return ResponseEntity.created(location).build();

    } else {
       throw exception(); // or another logic
    }
}

```

Yukarıda createUser'ı 3 ID'si ile çağırırsak (yani yeni user yarat ve ID'si 3 olsun istersek) cevaba olarak Spring otomatik olarak bu header'ı ekler:

- Header Name: "Location"
- Header Value: "http://localhost:8080/users/3"

localhost yerine o anda çalışan Spring sunucusunun IP'si dönecektir.

Spring yukarıdaki örnekte getUser() metotunu run etmez. sadece onun path'ini alır.
