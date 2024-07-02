############################

############################
# SECURITY JWT
############################

############################

# JSON Web Token (or JWT)
alternative pronunciation: "jot". kaynak: (source-id: 268) "The JWT Handbook", "Sebastián E. Peyrott", "Auth0 Inc.", Version 0.11.0, 2016-2017, title: "Chapter 1", sub-title: "Introduction".

Session yönetimlerinde giden token'ın (kullanıcı auth konusuyla ilgili giden gelen tüm bilgilerin) formatı ile ilgili özel bir standarttır. içinde ne gideceği ve nasıl doğrulanacağı hakkında standart içermez.

JWT iki standarttan birini implemente ederek kullanılabilir: kaynak: (source-id: 269) title: "7.2. Validating a JWT", madde 7.
- __JSON Web Signature (or JWS)__
- __JSON Web Encryption (or JWE)__

# JWS vs JWE
Aşağıdaki tüm açıklamalar için kaynak: kaynak: (source-id: 268) "The JWT Handbook", "Sebastián E. Peyrott", "Auth0 Inc.", Version 0.11.0, 2016-2017, title: "Chapter 5", sub-title: "JSON Web Encryption (JWE)", 1-4th paragraph.

JWS; data kısmını şifrelemiyor. Dolayısı ile JWS'de token herkes tarafından okunabiliyor. JWS, token'ın diğer bağımsız makinelerden valide edilebilmesini sağlıyor. Oysa JWE; token'ın third party'lerin okuyamamasını sağlıyor. çünkü data kısmını şifreliyor. Sadece secret'a sahip kişiler token'ın içini açabiliyor/okuyabiliyor.

JWS ve JWT iki farklı scheme ile yetki yönetimini sağlıyor:

- shared secret scheme

  tüm sistemde sadece tek bir secret key var.

  - JWS bu şekilde işletiyor:

    secret key'e sahip kişiler:
    - yeni token oluşturabiliyor (token'ı imzalayabiliyor - encrypt yapmıyor çünkü JWS body'yi şifrelemez)
    - var olan token'ı valide edebiliyor
    - JWS'de body kısmı zaten şifrelenmediği için zaten decrypt'e gerek yok.

    Dikkat edilirse böyle bir sistemde, client web browser ise, muhtemelen secret key'e sahip olmayacak ve token'ı valide edemeyecek. Fakat içindekileri görebilecek. (yani web browser imza kısmını doğrulayamayacak).

  - JWE bu şekilde işletiyor:

    secret key'e sahip kişiler:
    - yeni token oluşturabiliyor (encrypt edebiliyor)
    - var olan token'ı decrypt edebiliyor

- public/private-key scheme

  - JWS bu şekilde işletiyor:

    aşağıdaki tabloda "token decrypt" yok çünkü zaten JWS body'yi şifrelemez. herkese açıktır.

    | sahip olunan key | token generade (token sign) | token decrypt | token validate |
    |------------------|-----------------------------|---------------|----------------|
    | private key      | true                        | ?             | true           |
    | public key       | false                       | ?             | true           |

  - JWE bu şekilde işletiyor:

    | sahip olunan key | token generade (token encrypt) | token decrypt | token validate |
    |------------------|--------------------------------|---------------|----------------|
    | private key      | true                           | true          | ?              |
    | public key       | true                           | false         | ?              |

    JWE kullanımı dikkat edilirse, web tarayıcılarındaki SSL çalışma mantığı ile aynı. Herkes (her tarayıcı) şifreleyebiliyor, fakat sadece tek bir server bunları açabiliyor.

Yukarıdaki tablolarda soru işaretleri, ilgili kısmın JWS veya JWE'nin ilgi alanı olmadığından kaynaklıdır. örnek "token validate", JWE'yi ilgilendiren bir konu değildir.

# JWE
İki şekilde serialize edilebilir: kaynak: (source-id: 271) title: "7. Serializations", all subtitles.

- JWE Compact Serialization

  ```
  BASE64URL(UTF8(JWE Protected Header)) || '.' ||
  BASE64URL(JWE Encrypted Key) || '.' ||
  BASE64URL(JWE Initialization Vector) || '.' ||
  BASE64URL(JWE Ciphertext) || '.' ||
  BASE64URL(JWE Authentication Tag)
  ```

- JWE JSON Serialization

  ```json
  {
  "protected":"<integrity-protected shared header contents>",
  "unprotected": "<non-integrity-protected shared header contents>",
  "recipients":[
    {"header":"<per-recipient unprotected header 1 contents>",
    "encrypted_key":"<encrypted key 1 contents>"},
    {"header":"<per-recipient unprotected header N contents>",
    "encrypted_key":"<encrypted key N contents>"}],
  "aad":"<additional authenticated data contents>",
  "iv":"<initialization vector contents>",
  "ciphertext":"<ciphertext contents>",
  "tag":"<authentication tag contents>"
  }
  ```

# Unsecured JWT
algoritması "none" olan bir JWT token'ıdır. (Aynı zamanda algoritması none olan bir JWS'tir.)

# JWT yapısı
JWT üç temel kısımdan oluşur:

- ## JWT header
__Javascript Object Signing and Encryption (or JOSE)__'da denir. çünkü JWT; JWS standartlarındaki JOSE header'ını kullanır.

header'da aşağıdaki iki kısım standart'tır. ekstra isteğe bağlı field'lar eklenebilir.

```json
{
    "alg": "HS256",
    "typ": "JWT" ,

    "other custom fields can be put here": "other values..."
}
```

Yukarıdaki JSON'da:
- alg: (algorithm). Veri bütünlüğünü korumak için kullanılacak cryptotic algoritmayı belirtir.
- typ: (type). sabit bir değerdir. farklı bir JSON objesi olup olmadığını ayır edebilmek için bu şekilde verilmiştir.

Bazı header property'leri önceden belirlenmiş durumda. listesi burada: (source-id: 272) title: "4.1. Registered Header Parameter Names" all subtitles. Bunlardan bazıları kısaca:

- alg (algorithm)

  JWS based JWT'lerde content'i şifrelemek için kullanılan algoritmanın adını içerir.

  JWE based JWT'lerde ise; content'i şifrelemek için kullanılacak key'i (__Content Encryption Key (or CEK)__) şifrelemek için kullanılacak algoritmanın adını tutar. kaynak: (source-id: 268) "The JWT Handbook", "Sebastián E. Peyrott", "Auth0 Inc.", Version 0.11.0, 2016-2017, title: "5.1.3 The Header".

- enc (Encryption Algorithm)

  Bu JWE standartından geliyor. (JWS'de bu yok.)

  content'i şifrelemek için kullanılan algoritmanın adını tutar. kaynak: (source-id: 268) "The JWT Handbook", "Sebastián E. Peyrott", "Auth0 Inc.", Version 0.11.0, 2016-2017, title: "5.1.3 The Header".

  Ek not: content, __Content Encryption Key (or CEK)__ ile şifrelenir. kaynak: (source-id: 268) "The JWT Handbook", "Sebastián E. Peyrott", "Auth0 Inc.", Version 0.11.0, 2016-2017, title: "5.1.3 The Header".

- Zip

  compression algorithm

- jku (JWK Set URL)
- jwk (JSON Web Key)
- kid (Key ID)
- x5u (X.509 URL)
- x5c (X.509 Certificate Chain)
- x5t (X.509 Certificate SHA-1 Thumbprint)
- x5t#S256 (X.509 Certificate SHA-256 Thumbprint)
- typ (Type)
- cty (Content Type)
- crit (Critical)

- ## JWT payload (or JWT claims)
JWT payload içerisindeki her field'a __claim__ denir. "claim" terimi iddia etmekten gelmektedir. bu sebeple payload'a onun çoğul hali olan "claims"'te denir.

```json
{
    "userId": "3344552266",
    "expire": 1486220816,
    "roles": ["admin", "user"]
}
```

payload içerisinde ne olacağına JWT karışmaz. fakat standart olması (çakışma olmaması açısından) amaçlı bazı önceden belirlenmiş property'ler vardır. bunların bazıları: (source-id: 269) "4.1.  Registered Claim Names" başlığı.

Registred claim'lere "__public claims__" de deniliyor. bu durumda, kendi projemizde kullandığımız custom claim'ler "__Private Claims__" oluyor. kaynak: kaynak: (source-id: 268) "The JWT Handbook", "Sebastián E. Peyrott", "Auth0 Inc.", Version 0.11.0, 2016-2017, title: "3.2.2 Public and Private Claims".

| field | long name       | description                                                                 | example               |
|-------|-----------------|-----------------------------------------------------------------------------|-----------------------|
| iss   | Issuer          | principal generate the JWT                                                  | account.Google.com    |
| sub   | Subject         | subject                                                                     | jack1321342@gmail.com |
| aud   | Audience        | this is an identifier of the service which which this token generated for   | mail.Google.com       |
| exp   | Expiration Time | expire date-time of this JWT token                                          |                       |
| nbf   | Not Before      | date-time on which the JWT will start to be accepted for processing.        |                       |
| iat   | Issued at       | creation date-time of this JWT                                              |                       |
| jti   | JWT ID          | Case sensitive unique identifier of the token even among different issuers. |                       |

- ## JWT imza

bu kısım bu algoritma ile oluşturulur:

```java
String signatureOfJtw = base64(toHMACSHA256(urlBase64(headerJson) + "." + urlBase64(payloadJson), SECRET_KEY_OF_SERVER));

String jwt = urlBase64(headerJson) + "."
           + urlBase64(payloadJson) + "."
           + signatureOfJtw;
```

Dikkat edilirse; her kısım arasında nokta işareti vardır. Giden token URL-base-64 encoded olduğundan yazılımcı bu token'ı URL'den de sunucuya atabilir. Dikkat edilirse veri bütünlüğü sağlandığı için sunucuda session ihtiyacı kalkar. Tüm bilgiler token içerisinde saklanabilir.

JWT token'ı URL-base-64 encoded olduğundan; içeriği (JSON'lar) okunabilir ve client tarafından da değiştirilebilir. Fakat imza kısmındaki secret olmadığından (sadece server'da olduğundan), veri bütünlüğünü sağlayamaz ve token geçersiz sayılır.

Standartlarda olmasa da genelde JWT HTTP request'inin header kısmına şu şekilde saklanır:

> Authorization: Bearer JWT_TOKEN_BURADA

Bearer kelime anlamı: taşıyıcı.

# JWT token types

- access token:
  - isteği yaptığımızda kontrol edilen JWT token'ı.

- refresh token:
  - kullanımı opsiyoneldir.
  - bunun formatı da JWT'dir.
  - claim'ler, access token'ın claim'leri ile aynı değildir.
  - access token yaratacak tüm bilgiler refresh token'da olmalıdır. Bu sayede refresh token'dan access token üretebilmek için DB'ye gitmememiz bize hız kazandıracaktır.

# refresh token'ın avantajları
  refresh token'ın 2 avantajı var:

  - bu işlem %100 bir güvenlik sağlamasa da; access token çalınma durumunda ve refresh token'ın çalınmaması durumunda güvenliği sağlamaktadır. en azından hacker sadece access token'ın süresi boyunca işlem yapabilecektir.

  - eğer refresh token çalınırsa, son kullanıcı refresh token'ı bloke etmesi için sunucuya bildirimde bulunabilir. refresh-token'ı çalınmış kullanıcıyı genel olarak sistemden bloklama (örnek DB'den user'ı disable etme) pek verimli bir çözüm değil. Çünkü user'ı tekrar aktif ettiğimizde, hacker'ın aynı refresh token ile gelip gelmeyeceğini bilemeyiz. Fakat refresh token'ı iptal edersek, son kullanıcıya, normal login süreci işleterek, yeni refresh token verebiliriz. Böylece son kullanıcıyı hiç disable etmeden sistem hizmet vermeye devam edebilir olacaktır. Daha farklı bir değiş ile sadece eski oturumunu kapatarak yeni oturum açmasını sağlayabiliriz. Böylece user'ı komple disable etmemiz oluruz.

  Ek not: refresh tokenlar sunucu tarafta tutulmaktadır. Tabi farklı data-center'lar arasında dahi bu database syn olmalıdır. Fakat bu durum token-based'in mantığını bozmamaktadır. çünkü refresh token'lar bir kalıcı database'de tutulabilirler. yani hızlı erişim belleğinde tutulma ihtiyacı yoktur. çünkü az sıklıkla erişilmektedirler. database'de olan bu tarz bilgiler 'state' statüsünde değillerdir.

# refresh token ve access token expire time
Güvenlik altyapımızda bu iki token'ın süresini birçok farklı şekilde implemente edebiliriz. Bu konu en iyi şu yöntemdir demek olmaz.

access token'ı yenilemek için istek atıldığında refreh token'ın kendisi yenilenmez. aynı kalır. eğer aynı kalmazsa refresh-token DB'miz şişer. Fakat bir süre sonra refresh-token kendisini güncelleyebilir. Güncellenip güncellenmeyeceğine sadece sunucu karar vermelidir. Bu güncelleme işlemi için bazen son kullanıcıdan OTP de istenebilir, yada istenmeyebilir (yada benzeri bir kullanıcı otorizasyonu istenebilir). Bu durum; business tarafından belirlenir. Eğer refresh token yenilenmez ve refresh token'ın süresi biterse, o zaman artık son kullanıcının normal login akışını mutlaka tekrarlaması gerekir.

Browser uygulamalarında her access token yeilenmesi için istek atıldığında refresh token'da güncellenmesi tercih edilirse yüksek güvenlik sağlanmış olur. Tabi burada refresh token'ımızın expire time'ını yüksek tutabiliriz. Çünkü kullanıcı sayfayı kapatıp tekrar bizim sitemize girer ise o refresh token'ı kullanarak access token elde edebilmeli.

Native uygulamalarda ise refresh-token güvenli şekilde local'de saklanmalıdır. Bu durumda yine refresh token süresini uzun tutabiliriz. Böylece kullanıcı tekrar login olma ihtiyacı duymayacaktır.

Refresh token'ın expire time'ı ihtiyaca göre 5 dakika ile 3 yıl arasında piyasada kullanılıyor. Fakat 2 hafta Android telefonunundaki uygulamayı açmayan bir user için tekrar login olması sakıncalı olmayacaktır. Bu tarz case'leri değerlendirirp karar vermek gerekli. Hem access hemde refresh token için süreler ne kadar kıs olursa o kadar sistem güvenli olacaktır.

# CSRF token with JWT
CSRF token sunucu tarafta bilgi tutmayı gerektiriyor. JWT buna aykırı olduğu için CSRF token JWT ile kullanılmamalı.

# JWT dezavantajlar
- login olmuş user'ın role veya permission bilgileri JWT token'da olduğu için, login olmuş kullanıcının rolü veya permission'u değiştirilirse direk aktif edilememektedir.
- servise JWT ile istek yaptık. serviste giden istek, diğer sistemimiz içinde dahili olan mikroservislere yönlenmektedir. 10 servise gitti ve her biri arasında 3'er saniye kaybetti. 30 saniye sürdü. eğer 20inci saniyede token expire olursa, bir sonraki mikroservis isteği geri atacak çünkü JWT token artık invalid olacaktır.

# JWT web browser'da nerede saklanmalı
Öncelikle refresh ve access token farklı kaynaklarda saklanmalıdır ki ek güvenlik sağlayalım. Varolan olası kaynaklarımız:
- cookie
- JS (başka sekmedeki JS diğer sekmedeki değeri okuyamaz)
- local ve session storage

Local veya session storage'de hiçbir token saklanmamalı. Çünkü bu storage'ler herhangi bir eklenti veya kötü niyetli farklı bir saldırı, herhangi bir JS kodu çalıştırdığında (XSS attack) direk bu storage'lere erişebilir ve istediği gibi HTTP isteği yaratabilir. Not: JS kodu çalıştıran XSS atağı, access token olan sekmede XSS yaparsa, sekmedeki JS'teki access token'a erişir. Fakat farklı br sekmede XSS atağı olursa, diğer sekmedeki access token'a erişemez. Burada access token'ı storage yerine, JS'e koyarak, çalınma ihtimalini %50 düşürmüş oluyoruz.

Bu sebeple;

- access token

  memory'de (JS'te) saklanmalıdır. Farklı sekme açıldığında refresh-token ile web sayfamız hemen yeni access token alacaktır.

- refresh token

  katı kurallarla (HTTPS (secure) only, HTTP Only, same site...) cookie'de saklanmalıdır. refresh-token'dan access token üreten HTTP request'imiz sadece POST olmalı.

Refresh token cookie'de saklanmalı. Çünkü; kötü niyetli siteden 2 şekilde CSRF atak olabilir. Her durumu inceleyelim:

- (auto medya loading olursa) server'a GET isteği atılır.

  Server tarafta API POST beklediği için, hacker response'ta hiçbir şey elde edemeyecek (server hatalı istek yaptınız diye cevap dönecek).

- (Eğer user tıklayıp form submit ettirilmesi başarılırsa) server'a POST isteği atılır.

  submit işlemi sonrası JS tarafı (ilgili sekme) response'u okuyamaz. çünkü browser'larda submit sonrası response aynı JS'ten okunamaz.

Ek bir diğer önlem ise: tarayıcının otomatik yapmayacağı bir yöntem seçmek. örneğin: access token'ı "Bearer" ile header'da manuel Javascript ile yollarsak, başka bir sekmeden kötü niyetli bir kodun bu isteği yaratma şansı düşer. örneğin; farklı domain'deki kötü niyetli kod, access token'ı bir şekilde okuyabilirse, JS tarafında manuel request oluşturması da gerekecek (hacker bunu da yapabilir, ama ihtimali düşürüyoruz). Oysa cookie'de olan bir JWT token'ı otomatik web tarayıcıya (tarayıcının kendisi aracılığı ile) yollatsaydık, o zaman farklı sekmedeki bir request bizim domain'e istek yaptığında, cookie'leri tarayıcı request'e otomatik eklerdi. Aslında RAM'de olan bir data'nın okunması, ancak (çok muhtemel) bir JS kodunun execute edilmesiyle olabilir. Bu durumda Bearer'ın pek bir manası kalmıyor. Fakat dediğim gibi; burada amaç ihtimalleri düşürmek. Zaten "Bearer" keyword'ü genelde şu amaçla kullanılıyor: Access token'ı cookie'de saklayanlar oluyor. Çünkü her yeni sekmede yeni token alınsın istemiyorlar (performans için). Bu sebeple token'ı direk server'a atmak yerine, "Bearer" keyword'ü ile yollamayı tercih ediyorlar. Çünkü cookie'ler otomatik olarak zaten her request'e ekleniyorlar. Dolayısı ile; token yollama işlemini yapmak için, JS tarafını da ele geçirmiş olmayı zorunlu hale getiriyorlar.

Ek not: Burada tarayıcı ile iletişimde olan sunucunun da dikkat etmesi gerekenler var:
- access ve refresh token'ın sürelerini kısa tutmak.
- şifrelenmiş token'ı açıp içindeki bigliler her istekte kontrol etmek:
  - en son kullanma tarihi
  - işlem yapan user, isteğin yapıldığı domain'e ait mi
- CORS işlemlerinde en katı kuralları uygulamak.
- ve tabi, her zamanki gibi; tüm bu ve diğer süreçlerde genel güvenlik standartlarına uymak (SSL ile haberleşme vs...)