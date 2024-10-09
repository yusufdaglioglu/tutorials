############################

############################
# WEB HTTP HEADERS
############################

############################

# HTTP headers

> Age

proxy üzerinde kaç defa cache'den bu değerin kullanıldığıdır. otomatik proxy tarafından her defasından 1 arttırılır.

> Connection

keep-alive, close gidi değerler alır. Bir sonraki isteğin bu istekte kullanılan tc soket üzerinden gitmesini sağlar. bir sonraki istekler gönderileceği kesin ise; performans artışı sağlar. bu header'dan da anlaşıldığı gibi SSL bağlantıları her istekte eğer keep-alive gidiyorsa yeni soket üzerinden yapılmaz.

> Authorization

Standart auth işlemleri için kullanılan header

> Proxy-Authorization

Authorization ile aynı görevde fakat proxy'nin önemsediği değerdir.

> Accept

request'in kabul edeceği mimetype

> Accept-Charset, Accept-Encoding, Accept-Language

request'in kabul edeceği tipler

> Content-MD5

sadece body kısmının md5'i.

> Content-Type

sadece body kısmının mimetype'ı.

> TE

Transfer encoding'dir. Body kısmındaki verinin nasıl tutulduğunu belirtir. örneğin compress yapılmış ise; gzip gibi.

> Date

request'in oluşturulduğu tarih

> Max-Forwards

bu isteğin en fazla kaç tane proxy (aracı) üzerinden gidebileceğini limitler.

> DNT

do not track. 1 veya 0 değerini alır.

> Origin

CORS için gereklidir. O anda URL barda bulunan domain adresidir. Bunu Ajax'ı yapan Javascript değil, tarayıcının kendisi doldurmaktadır.

> Range

Büyük body kısmı olduğunda sürekli olarak arkaplanda tekrar işlem gönderiliyor. Böyle durumlarda bu header'a body'nin hangi byte'larının (byte aralığı) gönderildiği belirtilmektedir.

> Forwarded

X-Forwarded-For yerine kullanılan standartlaşmış bir header'dır.

client proxy'ye istek yaptığında bu değer boştur. daha sonra ilk proxy 2'inci proxy'ye istek atarken bunu doldurur. 1. proxy 2.inci proxy'ye istek yaparken bu header bu şekildedir:

Forwarded: for=CLIENT_IP

Daha sonra 2. proxy gerçek sunucuya isteği yönlendirdiğinde bu header şöyledir:

Forwarded: for=CLIENT_IP, for=PROXY1_IP; by=PROXY3_IP; proto=http; host=example.com

> Via

forwarded ile aynı bilgileri içerir aynı zamanda kullanıla protokollerin sürümlerini de içerir.

> Host

istek yapılan URL'nin adresi ve portu. path buna dahil değildir.

> Referer

Kelime olarak yanlış yazılmıştır fakat artık herkes bu şekilde kullanmaktadır.

Bir başka isteye yönlendirmelerde referer kısmı doldurulur. kullanıcının hangi siteden yönlendiğini anlamak için kullanılır.

> X-Forwarded-For

standart olmayan bir header'dır. Forwarded'dan önce vardı. fakat standart olarak referer sonradan belirlendi.  Fakat hala x-forwarded-for'u kullanan yazılım/donanımlar var.

X-Forwarded-For IP bilgilerini tutar. bir proxy bir isteği başka yere yönlendirdiği zaman bu bilgide orijinal isteği yapan sunucunun IP'sini barındırır. örnek:

X-Forwarded-For: client, proxy1, proxy2

> X-Forwarded-Host

X-Forwarded-For ile aynı bilgiler içerir. sadece IP yerine domain bilgisi tutar.

> X-Forwarded-Proto

X-Forwarded-For varken protokollerin yazılması için ayrı bir header belirlenmişti. her aracı bir sonraki aracıya isteği yaparken hangi protokol kullandıysa (örnek https) onu buraya yazar.

> X-Forwarded-IP

standart değildir. mail sunucularına istek yaparken kullanılan bir değerdir.

> Access-Control-Request-Method

CORS için gerekli. "Preflight" isteklerinde kullanılır. Preflight onayı alınırsa gönderilecek asıl isteğin metotudur (örnek : "put").

sunucu asıl request'e izin verip vermeyeceğine karar verebilmek için preflight'ta bu tarz detayları ister.

> Access-Control-Request-Headers

CORS için gerekli. "Preflight" isteklerinde kullanılır. Preflight onayı alınırsa gönderilecek asıl isteğin header'ını barındırır.

sunucu asıl request'e izin verip vermeyeceğine karar verebilmek için preflight'ta bu tarz detayları ister.

> Access-Control-Expose-Headers

CORS isteklerinde sunucu bu header'ı client'a döndürür. web tarayıcı JS tarafında sadece:

- burada belirtilecek olan header'ların
- default olarak web standartlarında belirtilmiş header'ların

okunmasına izin verecektir.

> Access-Control-Allow-Origin

Response'ta bulunur. Preflight ‘a cevapta bulunur. Server'ın izin verdiği tüm domain'leri içerir. bu domain'ler URL bar'daki domain'ler oluyor.

> Access-Control-Allow-Methods, Access-Control-Allow-Headers

Response'ta bulunur. Preflight'a cevapta bulunur. Server'ın izin verdiği bilgilerdir.

> Access-Control-Allow-Credentials

Response'ta bulunur. Preflight ‘a cevapta bulunur. Server'ın izin verdiği güvenlik bilgilerini içerir. örneğin; asıl atılacak istekte cookie olup olmayacağını belirtir.

> Access-Control-Max-Age

Preflight response'unda bulunur. Bu o isteğin ne kadar süre gereli olduğunu belirtir.

> X-Requested-With:

Bu header her zaman XMLHttpRequest değerini alır. Bunun yollanma sebebi isteğin CORS olup olmadığını ayırt edebilmektir. Çünkü CORS standartlarında yollanan isteklerde sadece bazı header'lar yer alabiliyordu. Bu header listede olmadığından CORS standartları çerçevesinde atılmadığını anlamak için kullanılır.

> User-agent

tarayıcı yada isteği yapan client'ın bilgisi

> Upgrade

istek yapılan yere HTTP sürümünü güncellemesi isteğidir.

> Status

HTTP status code

- 1xx Informational responses: payload gönderilmeye devam ediliği zaman, protokol sürümü değiştirileceği zaman...
- 2xx Success
- 3xx Redirection
- 4xx Client errors: URL too long, bad request, method not found, auth failed.
- 5xx Server error: internal problems on server.

Status code'lar Spring'de bu şekilde döndürülüyor:

```java
import org.springframework.http.ResponseEntity;

@RequestMapping(value = "/controller", method = RequestMethod.GET)
@ResponseBody
public ResponseEntity sendViaResponseEntity() {
    return new ResponseEntity(HttpStatus.NOT_ACCEPTABLE); // ilgili hata code'unu HTTP response'unun status'unda dönüyor
}
```

Diğer durumlarda Spring yine gerçek hataları atıyor. örneğin Filter'larda olmadık bir exception olursa, internal server error hata kodu dönüyor. standart HTTPkodları dönüyor.

ResponseEntity objesi body'yi wrap etmez yada response output'unu değiştirmez. ResponseEntity response'u daha detaylı değiştirebilmemize yarar. ResponseEntity'siz sadece body'yi değiştirebiliriz. fakat ResponseEntity ile header'ları, status code'ları da değiştirebilir.

Yukarıdaki header'lara ek olarak cache için kullanılan header'lar da vardır. bu header'lar başka başlıkta anlatılıyor.

Hata kodlarının listesi: (aşağıda birçok farklı kaynaktan buraya kopyalandı. Bazıları daha detaylı bilgi içeriyor çünkü.)

- [file:"web_http_status_codes_iana.md"](./web_http_status_codes_iana.md)
- [file:"web_http_status_codes_microsoft.md"](./web_http_status_codes_microsoft.md)

Bazı hata kodlarının açıklamaları:
- 404: "resource" terimi HTTP standartlarında URI ile temsil edilebilen servis veya kaynaklar olarak tanımlanmaktadır:

```
A network data object or service that can be identified by a URI.
```

Dolayısı ile /address/999 servisine istek yaptığımızda, bu adrese bağlı bir user aktif değil ise user-not-found hatası fırlatıldığında, 404 dönülmemelidir. Sadece ve sadece, "address" objesi bulunamaz ise 404 dönülmelidir.

- 5xx: bu hatalar business logic olan hatalar da olabilir. çünkü RFC'de şu yazmaktadır:

```
The 5xx (Server Error) class of status code indicates that the server is aware that it has erred or is incapable of performing the requested method.
```

Yani; sunucunun bu durumdan bilgisi vardır. Yani hata yakalanmış durumdadır. Programcı kendi yakalamasa bile bazı hataları web sunucusu (Tomcat vs) de yakalamış olabilir.

Fakat her business logic 500 olamayabilir. Örneğin; "409 Conflict" statüsü, kaynağın sunucudaki durumu sebebiyle işlemin yapılamadığı durumlarda client'a dönülür.

# header isimlendirmesi

Eskiden x- ile başlayanlar standart olmayan header için kullanılıyordu. fakat bazı header'lar sık kullanılmaya başlandığından, artık standart hale geldiler. bu sebeple artık custom header'lara farklı bir prefix yazılması önerilir.

# relative quality factor
q parametresi "accept" keyword'ünü içeren header'ların (Accept-Charset, Accept-Encoding...) value kısmında kullanılmaktadır. bu değer 0 ile 1 arasında bir sayı değeri almaktadır. örnek:

```
Accept: text/plain; q=0.5, text/html, text/x-dvi; q=0.8, text/x-c
```

Yukarıdaki header şu anlam geliyor: __html__ ve __x-c__ en öncelikli (q=1 default), eğer bu tipleri sunucu desteklemiyorsa, __dvi__ olarak cevap version. eğer onu da desteklemiyorsa, __plain__ cevap version.

# header'lar cookie'leri barındırır

header'lar arasında cookie diye bir değer bulunur. bu değerde cookie'ler taşınır. formatı şu şekildedir:


- client sunucuya atarken Header içerisinde şu satırlar yer alır:

Cookie: name=value; name2=value2
Cookie: name3=value3

Yukarıda expire secure gibi nitelikler isteğe bağlıdır.

- Sunucudan client'a response içerisinde dönen header'lar

Yukarıdaki ile aynı formattadır. tek farkı "Cookie:" yerine "Set-Cookie:" yazmasıdır. Aynı zamanda expire, secure gibi özellikler burada isteğe bağlı belirtilmelidir.

Set-Cookie: name=value; name2=value2; Expires=12.04.2018; Secure
Set-Cookie: name3=value3

her tarayıcı isteğin yapıldığı domain'e bağlı tüm cookie'leri sunucuya otomatik gönderir. her defasında yollanması istenmeyen cookie'ler, cookie olarak değilde, farklı storage'lere atılmalıdır.
