############################

############################
# JAVASCRIPT
############################

############################

# Javascript promise
promise keyword'ü daha çok kullanılsa da farklı kütüphaneler, yada programlama dilleri farklı isimlerde kullanmaktadır: Future, delay, deferred...

Javascript'te parametre olarak fonksiyon geçilebilmektedir. bu şekilde error, success gibi event'lerde o callback'lerimiz çağrılmaktadırlar. fakat promise ile; parametre geçmek yerine, promise bize bir nesne (promise nesnesi) döner. biz bu promise nesnesinin success fail gibi metotlarına ilgili callback metotlarımızı ona yollarız. daha sonra promise metotumuzu run ederiz. aslında farklı yöntemlerle yapılabilecek bir iş. fakat promise yöntemi okunabilirliği arttırmaktadır, standart olmasını sağlamaktadır ve eğer dilin native Promise kütüphanesinden yararlanıyorsak, asenkron thread işlemi yapabilmemizi sağlamaktadır.

promise aslında programlama dilinden bağımsız bir best practice'dir.

Ecmascript 6 ile Promise nesnesi native olarak gelmiştir. daha önceki Ecmascript sürümlerinde Promise 3üncü parti kütüphanelerle yapılmaktaydı. güncel Ecmascript promise kullanımı şu şekildedir:

```js
const myPromise = new Promise((resolve, reject) => {

  // this function here called "executor".

  // do any asynchronous operation here:
  setTimeout(() => {
    resolve('foo');
  }, 300);

  // if we will call "resolve" here those will be ignored.
  // "resolve" method works only once.
});

myPromise
  .then(handleResolvedA, handleRejectedA)
  .then(handleResolvedB, handleRejectedB)
  .then(handleResolvedC, handleRejectedC)
  .finally(finallyHandler);

// if we have only 1 reject-handler:
myPromise
  .then(handleResolvedA)
  .then(handleResolvedB)
  .then(handleResolvedC)
  .catch(handleRejectedAny)
  .finally(finallyHandler);
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# "use strict"
bu satır ile Javascript dilinin derlenmesi daha detaylı ve daha temiz olmasını sağlar. bazı örnekler:

- "var" olamadan global değerler yaratılmasını engeller.

- throws exception on Assignment to a non-writable global:

```js
var undefined = 5; // throws a TypeError
var infinity = 5; // throws a TypeError
```

"use strict" olmasaydı yukarıdaki satırlar hata fırlatmayacak fakat variable simleri özel oldukları için bu isimde variable da oluşmayacaktı.

- throws exception on Assignment to a non-writable property

```js
var obj1 = {};
Object.defineProperty(obj1, 'x', { value: 42, writable: false });
obj1.x = 9; // throws a TypeError
```

- throws exception on Assignment to a getter-only property
```js
var obj2 = { get x() { return 17; } };
obj2.x = 5; // throws a TypeError
```

"use strict" satırı bir metotun içine yazılırsa sadece metot için geçerli olmaktadır. eğer globale yazılırsa tüm sistemi etkiler.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# variable defining sytax

```js
var var1 = "hello", var2, var3 = 99;
```

equals:

```js
var var1 = "hello";
var var2;
var var3 = 99;
```

# multi equal character on same line

```js
var a = b = c;
a = b = c;
z = a = b = c;
```

Yukarıdaki örnekte sırası ile: "b = c", "a = b" çalıştırılıyor. en sondaki örnekte ek olarak; en sonda "z = a" çalıştırılıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Web Hypertext Application Technology Working Group (or WHATWG)
https://github.com/whatwg reposunda geliştirilen web standartlarını dökümante edilen bir web projesi geliştiren topluluktur. __World Wide Web Consortium (or W3C)__'ten farklı bir topluluktur fakat birlikte çalışmaları vardır. Yayınladıkları eski dökümanlarda ufak farklılıklara rastlayabiliriz. Fakat daha sonrasında bu farklılıklar kapatıldı.

# fetch

Web tarayıcılarında uzun zamandır desteklenen yeni API. Promise tarzı çalışması en büyük avantajıdır. örnek kullanım:

```js
fetch('send-Ajax-data.php')

    .then(data => console.log(data))

    .catch(error => console.log('Error:' + error));
```

ES7 async/await example:

```js
async function doAjax() {
    try {
        const res = await fetch('send-Ajax-data.php');
        const data = await res.text();
        // dikkat edersek hem "text" hemde "fetch" metotları promise dönüyor.
        // çünkü "fetch", header'ları çekiyor promise'i sonlandırırken,
        // "text" metotu da body'yi load edip promise'i sonlandırıyor.
        // yani body stream ediliyor. bu durum aşağıda daha detaylı anlatılıyor.
        console.log(data);
    } catch (error) {
        console.log('Error:' + error);
    }
}

doAjax();
```

Fetch API spesifikasyonu burada: (source-id: 303)

Eski tarayıcılar için core Javascript polyfill kütüphanesi: https://github.com/github/fetch (whatwg-fetch isimli npm paketi). Fetch polyfill kendi içinde var XMLHttpRequest'i wrap eder. Kaynak: (source-id: 304)

Fetch, JQuery.Ajax()'den bazı farklılıklar var. bu linkte farklılıklar yazmaktadır: (source-id: 305)

example codes for fetch:

```js
var metaDatas = {
    method: 'post',
    headers: {
      "Content-type": "application/x-www-form-urlencoded; charset=UTF-8"
    },
    body: 'foo=bar&lorem=ipsum'
  };

fetch('./API/some.json', metaDatas)
  .then(
    function(response) {
      if (response.status !== 200) {
        console.log('Looks like there was a problem. Status Code: ' + response.status);
        return;
      }

      // read some headers:
      console.log(response.headers.get('Content-Type'));
      console.log(response.headers.get('Date'));

      // read some metadata:
      console.log(response.status);
      console.log(response.statusText);
      console.log(response.type);
      console.log(response.URL);

      // json() is a stream.
      // response.status, headers are coming first from the TCP connection. they are ready inside this function, otherwise fetch will call "catch".
      // but the payload is coming after header and status. payload can be very long. so we call it with stream.
      // kaynak: (source-id: 306) sayfanın sonundaki Android emülatörlü animasyonda 8 saniyelik download'un nasıl stream edildiği gösteriliyor.
      response.json().then(function(data) {
        console.log(data);
      });
    }
  )
  .catch(function(err) {
    console.log('Fetch Error ' + err);
  });
```

another example:

```js
// instead of response.json() we can call the reader (which is a stream-reader)
const reader = response.body.getReader();

// Not: Bu kısmı daha iyi anlamak için "JSON network stream" başlığına bakılmalı.

// infinity loop while the body is downloading
while(true) {
  // done is true for the last chunk
  // value is Uint8Array of the chunk bytes
  const {done, value} = await reader.read();

  if (done) {
    break;
  }

  console.log(`Received ${value.length} bytes`)
}
```

another example:

```js
function status(response) {
  if (response.status >= 200 && response.status < 300) {
    return Promise.resolve(response)
  } else {
    return Promise.reject(new Error(response.statusText))
  }
}

function json(response) {
  return response.json()
}

fetch('users.json')
  .then(status) // yukarıdaki status metotu oto çağrılıyor ve response parametresi oto geçiliyor
  .then(json) // yukarıdaki status metotu oto çağrılıyor ve response parametresi oto geçiliyor

  // burada istediğimiz kadar then kullanabiliriz. her then metotunda return ettiğimiz her obje, diğer then'e parametre olarak geçecektir.

  .then(function(data) {
    console.log('Request succeeded with JSON response', data);
  }).catch(function(error) {
    console.log('Request failed', error);
  });
```

# Axios
- browser'da XMLHttpRequest'i wrap eden, NodeJS'te "http" objesi ((source-id: 307) ) ile çalışan açık kaynaklı Javascript HTTP client. kaynak: (source-id: 308) "Features" başlığı 1 ve 2inci madde.
- fetch-API ile yapılacak birçok şeyi daha kolay şekilde yapabilmemizi sağlıyor.
- fetch-API'ye göre yapılamayan bazı özellikleri de var (örnek upload'da progress'ler adım adım takip edilemiyor).

# JSON network stream
JSON formatını stream edebilmek için (örneğin network üzerinden HTTP ile) her parçasının anlamlı olması gerekli. Yoksa client taraf neye göre stream edecek? parçaları nasıl anlamlandıracak? Normalde JSON için böyle bir durum mümkün değil. Fakat buna çözüm olması amaçlı; JSON içindeki field'larn sonuna satır sonu karakteri ekleyerek network'ten atarsak, client taraf ise, satır sonunu görünce varolan field'ı valid bir JSON olarak kabul ederse bu işi çözmüş oluruz. Satır sonu karakterini JSON'a eklediğimizde yeni bir standart yaratmış oluyoruz. Bunun için şu anda (yıl 2023) 2 farklı spesification var:
-  JSON Lines (web site: https://jsonlines.org/)
- Newline Delimited JSON (web site: https://ndjson.org/)

Bu iki standart neredeyse aynı. Burada da bu durum tartışılıyor: https://github.com/ndjson/ndjson-spec/issues/35

Örneğin Python için geliştirilen "jsonlines" kütüphanesi yukarıda yazan 2 format ile de çalışabiliyor. kaynak: https://jsonlines.readthedocs.io/en/latest/ 1st sentence.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# immutable.js

```js
const { Map } = require('immutable')

const map1 = Map({ a: 1, b: 2, c: 3 })

const map2 = map1.set('b', 50)

map1.get('b') + " vs. " + map2.get('b') // 2 vs. 50
```

Yukarıdaki Map sınıfı, JS'in default bir sınıfı değil. tamamen immutable'a özel bir sınıftır. yani aşağıdaki gibi bir obje değildir:

```js
var myArr = { a: 1, b: 2, c: 3 };
```

# immer.js

```js
import produce from "immer"

const baseState = [
    {
        todo: "Learn typescript",
        done: true
    },
    {
        todo: "Try immer",
        done: false
    }
]

const nextState = produce(baseState, draftState => {
    draftState.push({todo: "Tweet about it"})
    draftState[1].done = true
})
```

at the end baseState will be untouched. dikkat edilirse; immutable.JS'teki gibi native olmayan JS class'ı kullanılmamış. sadece draftState native olmayan bir JS class'ıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# vanilla-js
vanilla JS bir kütüphane/framework'tür. fakat içi boş(kod olmayan) bir JS projesidir: (source-id: 309) . Bu proje, herkesin çok fazla gereksiz JS kütüphanesi kullanmasına tepki olarak oluşturulmuştur.

VanillaJS, saf JS kodu ile yazılmış projelere takma isim olarak kullanılmaktadır.

"vanilla" teriminin sık kullanılması ile birlikte artık herkes her framework/uygulamaya "vanilla" takma ismini prefix olarak koymaktadır. o uygulamayı yada framework'ü eklenti olmadan/saf hali ile kullandığını belirtmek için kullanılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Polyfill
bu terimin Türkçesi yoktur.

sadece web development'ında kullanılan bir terimdir. eğer bir kod parçası, web tarayıcının desteklemediği bir özelliği çalıştırabilmemizi sağlıyorsa, o kod parçasına Polyfill denir.

# Shim
tüm programlama dünyasında kullanılan bir terimdir. eğer bir kod parçası, bizim yazdığımız API metotunun önünü intercept edip başka bir metota yönlendiriyorsa, o kod parçasına shim denir. shim'ler genelde geriye uyumluluk için kullanılırlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# cross-env
https://www.npmjs.com/package/cross-env sitesinde dağıtılan node modülüdür. bu modül ile npm script'lerinde çağrılan komutlara environment variable'ları geçebilmemizi sağlamaktadır. bu özellik normalde de npm tarafından desteklenir, fakat cross-env, bunu OS'tan bağımsız bir syntax ile yapabilmemizi sağlar.

örnek usage:

```json
// package.json file
{
  "scripts": {
    "build": "cross-env NODE_ENV=production webpack --config build/webpack.config.js"
  }
}
```

cross-env isimli node modül 2 adet komut export eder: cross-env ve cross-env-shell. cross-env komuta parametre geçilen komut variable'ı okuyabilir. fakat ikinci komut bu variable'ı okuyamaz. iki veya daha fazla komut üstüste çağrılacak ise cross-env-shell kullanılması şarttır. cross-env-shell için örnek:

```json
{
  "scripts": {
    "greet": "cross-env-shell XXX=111 YYY=222 \"echo $XXX && echo $YYY\""
  }
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# flow
flow.org sitesinde dağıtılan JS için type desteği getiren bir typechecker'dır.

örnek JS kodu:

```js
let num: number = 1 + 2;

//fonksiyon örneği:
function square(n: number): number {
  return n * n;
}

square("2"); // hem derleyici hata verir, hemde IDE olan flow eklentimiz (or lint içindeki flow eklentimiz) hata verir.

// sol tarafta bir nesneye referansı atanmadan yazılan örnekler:
(1234: number);
("hi": string);
(true: boolean);
([1, 2]: Array<number>);
({ prop: "value" }: Object);
(function method() {}: Function);

```

Flow tipleri JS syntax'ında yoktur. bu sebeple derleyicimizde de ayar yapmamız gerekecektir.

- configs:
  - JS dosyanın flow'a göre algılanması için "// @flow" satırının, JS dosyanın bir yerinde olması gerekmektedir.
  - projenin root'unda ".flowconfig" dosyası olmalıdır. bu dosya için ignore/include edilecek dizinler gibi ayarlar bulunuyor

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Javascript performans avantajı nereden kaynaklı?

Javascript tek thread çalışır. thread içerisinde çağrılan paralel gibi görünen thread'lerin (setTimeout, Ajax gibi) callback'leri aslında asıl thread bittikten sonra çalıştırılır. örneğin Ajax işlemine callback parametresi geçtik. Ajax işlemi browser'a verilir ve asıl thread bitmeden Javascript o callback'i Ajax sonuç dönmüşse dahi çalıştırmaz.

Ecmascript thread mekanizmasında zorunlu standartlar getirmemiştir. dolayısı ile Ecmascript'in Javascript harici diğer implementasyonları tek thread olmak zorunda değildir.

thread kavramı process thread'i olarak algılanmamalıdır. Javascript motoru multi-thread'dir. Javascript'in çağırdığı Ajax işlemi multithread olabilir. fakat Javascript'in kod seviyesinde tek thread olarak işletilir.

# Event loop
JavaScript'in concurrency model'i __event loop__'a dayanır. Tüm işlemler (event'ler) bir queue'da bekletilir. Hepsi sıra ile tek thread'de execute edilir. Bu mekanizmaya __event loop__ denir.

# sunucudaki performans üstünlüğü

Javascript tek thread olmasına rağmen daha hızlı çalışır. servlet'lere istek gelir. servlet'e gelen istek database'ye gider. database'den işlemin dönüş yapması 5 saniye olsun. bu 5 saniye boyunca 1 thread beklemede durmaktadır (blocking-IO). Java'da database işlemini thread yapsak ve aşağıdaki gibi olsa main thread devam eder ve database işlemi thread'de çalışır.


```java
//class definition on server side
class ThreadX extends Thread {

     run(){
           aLongDatabaseOperation();
           System.out.println("3");
     }
}


//servlet code start

System.out.println("1");
t = new ThreadX();
t.run();
System.out.println("2");

//servlet end
```

yukarıdaki kod bloğunda hala Javascript daha hızlı olacaktır. günümüzde 1 CPU max 8 CPU'lu olabiliyor. bu durumda; thread'in database işlemini beklemesi demek, CPU'yu da boşuna bekletmesi demek. aynı zamanda thread memory'si demektir. CPU aynı anda birçok işlem yapabilseydi Javascript bu kadar performanslı olmayacaktı. JS yukarıdaki gibi bir işlemde main thread'i bitiriyor. yani ekrana 1 ve 2 print ediliyor. eğer veritabanı işlemi bitmiş ise; JS hemen ekrana 3 basıyor. fakat database işlemi bitmemişse; servlet'e gelen diğer isteklere cevap vermeye devam ediyor.

# performans comparison: single-thread non-blocking I/O vs multithread blocking-IO

yukarıdaki servlet örneği alsında bu karşılaştırmanın bir spesifik örneği oldu. sistemden sisteme bu karşılaştırmanın olumlu ve olumsuz yanları olabilir. fakat servlet'lerde sunucuya 1000 istek geldi diyelim. 10000 thread açılacak Java'da. oysa Javascript'te 1 thread olacak. Javascript motoru tabi arkaplanda çok thread açıyor. fakat açılan thread'ler hiç block'lanmadıkları için hemen kapanıyorlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# moment.js

tarih/saat işlemlerini kolaylaştıran Javascript kütüphanesi. bazı özellikler:

- tarihi formatlayıp gösterme
- bir tarihi verip şu andan ne kadar eski olduğunu gösterme
- toplama çıkarma

# moment-timezone.js

- timezone bilgilerini içeren moment eklentisi. bir datetime bilgisi, istenilen timezone'a çevrilebilmesi sağlanıyor.

- moment-timezone ile convert işlemi yapabilmek için bağımlılıklarımız şunlardır:

  - moment.js: core kütüphane

  - moment-timezone.js: timezone'u çevirme API'si için

  - moment-timezone-with-data.js: convert işlemini yapan JS içerisinde her ülkenin timezone bilgisi kodları ve saatleri ile birlikte yok. o yüzden bu dosyaya da ihtiyacımız var. dosyanın isminden de anlaşıldığı gibi bu dosyanın içinde zaten moment-timezone.js vardır. her ülkenin yıllara göre timezone'u değişmektedir. bu sebeple bu data yıllara göre tutulmaktadır. bu sebeple kütüphanenin dosya büyüklüğü artmaktadır. Buna çözüm olarak farklı paketler sunulmaktadır:

    - moment-timezone-with-data-2012-2022.js

      son 10 yılın bilgisi ile gelen paket.

    - moment-timezone-with-data-1970-2030.js

      1970 ile 2030 arasındaki data'lar ile gelen paket.

    - moment-timezone-with-data.js

      tüm yılları içeren paket.

    Moment-js-with-data bu data'ları buradan topluyor ve kütüphanenin içine gömüyor: (source-id: 310) kaynak: (source-id: 311) "mj1856" commented on Jul 27, 2016. 1inci paragraf.

# other JS date/time libraries
MomentJS projesi ek geliştirmeler yapılmayacak statüye geçmiştir. kendi resmi sitesinde diğer önerilerle ilgili güzel bir yazı var: (source-id: 312) title: "Recommendations".

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# numeral.js

sayısal işlemleri kolaylaştıran Javascript kütüphanesi. bazı özellikler:

- istenilen formatta para birimi, byte birimi formatlayarak gösterebilme

- 1inci, 2inci gibi çıktıları verebilme

- üs sayılarını formatlayabilme 1e+2 gibi

- basit matematik işlemleri yapabilme

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# NodeJS

## express

NodeJS için sunucu yazılmasını kolaylaştıran framework'tür, npm modülüdür. özellikle REST controller işini yapması ile ön plana çıkar.

## REPL (or Read-Eval-Print Loop or interactive toplevel or language shell)
işlevini gören NodeJS içinde gelen komut satırı uygulamasıdır. sadece "node" komutu komut satırına yazıldığında komut satırı yeni bir satıra geçer ve son kullanıcıdan Javascript girişi bekler. buraya yazılan Javascript'ler o anda işletilir.

REPL genel bir kavramdır. Örneğin Java 9 ile de REPL gelmiştir.

# Yarn vs Bower
NPM'e alternatif NodeJS için paket yöneticileri.

# Gulp vs Grunt
NodeJS modülleridir. bunlar sayesinde projemizde minify, auto-watch and auto-deploy gibi işlemlerimizi task'lar tanımlayarak yapabiliyoruz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# how to use common static string variables

Javascript kütüphanelerinin source code'una bakıldığında string'ler hardcode girilmiş olarak görülebilir. bunun sebebi minify edilirken, yada minify edilmeden prod ortamına hazırlanırken, kodun daha hızlı çalışabilmesi için string'lerin gömülmesidir.

örneğin; MomentJS'te

```js
moment().add(1, 'month');
```

buraya gönderilen string'lerin common bir yerde tutulması tavsiye edilir.

MomentJS geliştirme ortamında bu kodlar hardcode değildir. prod'a paketlenirken hardcode girilir.

yani; CommonStrings diye bir sınıf isteyerek yaratılmıyor. çünkü MomentJS'te moment().add(1, CommonStrings.MONTH); kodu yerine MomentJS'te moment().add(1, "month"); daha hızlı çalışmaktadır. dev ortamında CommonStrings varken canlı Javascript kodlarında CommonStrings kullanılmamaktadır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Javascript core ile obje klonlama

Ecmascript güncel versiyonları ile gelen aşağıdaki ile yapılabilir:

```js
var newObject = { ...oldObject, {} };
```

Syntax yazılımcılar arasında "object spread" deniliyor. (spread kelime anlamı: yayılmış). 3 noktaya (...) "spread operatörü" denir.

Yukarıda oldObject array veya obje olabilir. her türlü JS otomatik iterate ediyor.

Aşağıdaki işlem spread operatörü ile yapılan işlem ile aynı işi yapmaktadır:

```js
var newObject = Object.assign(oldObject);
```

assign metotu sonsuz parametre alıyor. tüm parametreleri tek objede topluyor.

Tek farkı şudur: Object.assign setter(newValue) kullanarak yeni objeye değerleri atıyor. yani setter metotu tetikleniyor. fakat spread direk nesneyi property atayarak oluşturuyor.

# JQuery ile obje klonlama

- Shallow copy

Shallow Türkçe kelime anlamı: yüzeysel

```js
var newObject = jQuery.extend({}, oldObject);
```

shallow copy yeni obje yaratıyor. eski objenin en üstteki property'lerini geziyor. bu property'leri literal tipte ise; variable'larını kopyalıyor. eğer property'ler obje ise referansları ile kopyalıyor. alt elementlere girmiyor.

- Deep copy

```js
var newObject = jQuery.extend(true, {}, oldObject);
```

Deep copy'de tüm alt elementler dahi geziliyor ve tüm değerler variable olarak yeni objeye kopyalanıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# REST operator
burada kullanılan "..." (üç nokta) REST operatörü olarak adlandırılır. fonksiyonlara sınırsız parametre yollanabilmesini sağlar.

```js
function f(x, y, ...arr1) {

  // arr1 is simple Javascript array.
}

// calling above method
f("a", "b", 1, 2, 3, 4);
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Watchman
Facebook'un geliştirdiği bir yazılım. bir dosya dizini veriliyor, bu dosya dizinlerini sürekli takip ediyor eğer bir değişiklik olursa, önceden belirlenen komutları tetikliyor.

Executable dosyası, ek olarak FB-Watchman ismi ile NodeJS paketi olarak dağıtılıyor. Böylece Javascript API'si ile de kullanılabiliyor.

Genelde web uygulamalarında development yaparken bu uygulamadan çok yararlanılıyor. live-reloading işlemi yapılması sağlanıyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Modernizr
bu JS kütüphanesi ile hangi teknolojilerin tarayıcıda var olup olmadığı kontrol edilebiliyor. aşağıdaki örnekteki gibi tüm teknolojiler tek bir variable'da kontrol edilebiliyor. örnek:

```js
if(modernizr.cookies){

  //cookies enabled
}
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Javascript scope
The "global" scope is the outermost scope. It is accessible from any inner/local scope. global scope on web browser is accessible via "window" object. on other Javascript engines can be different.

```js
this.window === window.window // true
this === window // true
```


fonksiyonların kendi scope'u vardır. new olmazsa yukarıdakinin "this"'i ile fonksiyon içindeki this eşit olur. fakat aynı this'i kullandıkları aynı scope'ta oldukları anlamına gelmiyor. aşağıdaki örnek bu durumu açıklıyor:

```js
// function tanımlama. burada execute edilmiyor.
function run() {
  // "run" function scope
  var message = 'hello';
  console.log(message);
  console.log(this === window);
}

run(); // burada execute ediliyor.
// prints:
// "hello"
// true

console.log(message); // throws ReferenceError

new run(); // burada execute ediliyor.
// prints:
// "hello"
// false
```

__lexical scoping (or static scoping)__ closure konusunda anlatılmıştır.

# closure (or lexical closure or function closure)

closure Türkçe kelime anlamı: kapatma

```js
var add = function () { // function-1

    var counterVAR = 5;
    counter = 5;
    this.counterTHIS = 5;
    let counterLET = 5;
    const counterCONST = 5;

    return function () { // function-2
        counterVAR += 1;
        counter += 1;
        counterTHIS += 1;
        counterLET += 1;
        // const variables can not be changed! they are read-only.
        // (that rule is not about "scopes".)
        // counterCONST += 1;

        console.log("INNER- counterVAR: " + counterVAR);
        console.log("INNER- counter: " + counter);
        console.log("INNER- counterTHIS: " + counterTHIS);
        console.log("INNER- counterLET: " + counterLET);
        console.log("INNER- counterCONST: " + counterCONST);
    }
};

console.log(typeof add); // function (which is "function-1")

var addFunc = add();

console.log(typeof addFunc); // function (which is "function-2")

addFunc();
addFunc();
// new addFunc(); --> ile çalıştırılacak fonksiyonun scope'unu değiştiriyoruz, ama function-2 scope'unda kullanılan bir variable yok. o yüzden aşağıdaki sonuçlar değişmeyecekti.

console.log(typeof addFunc()); // undefined. because function-2 does not have a return value.

// counterVAR is undefined on this scope
// var addFunc = new add(); --> yapsaydık yine de aynı şekilde olacaktı.
try {
console.log("counterVAR:" + counterVAR); // throws undefined exception
} catch(e) { console.log("counterVAR is undefined"); }

// counter, en üst seviye "this" olan "window" objesinin scope'undadır.
// var addFunc = new add(); --> yapsaydık yine de aynı şekilde olacaktı.
console.log("counter:" + counter); // prints 8

// counterTHIS add fonksiyonumuzu "window" scope'unda run ettiğimiz için "counter" ile aynı seviyede tanımlanmıştır.
// var addFunc = new add(); --> yapsaydık, zaten addFunc() hata verecekti.
// Bu kısım özellikle önemli!
// new operatörü kullanmazsak this=window oluyor.
// new operatörü kullanırsak this=Microsoft Windows olmuyor. yeni bir "this" oluyor. fakat bu durumda inner scope'lar buraya erişemiyor.
console.log("counterTHIS:" + counterTHIS); // prints 8

// counterLET is undefined on this scope
// var addFunc = new add(); --> yapsaydık yine de aynı şekilde olacaktı.
try {
console.log("counterLET:" + counterLET); // throws undefined exception
} catch(e) { console.log("counterLET is undefined"); }
```

closure ile API'lerin private metotlar ve değerler içerebilmesi sağlanmaktadır.

resmi olarak closure'un ne olduğu net değil. bazıları: return edilen metot'un önceki metottaki variable'lara hala erişebilme tekniğine "Closure" denirken, bazıları ise return edilen fonksiyona closure adını veriyor.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Javascript language

# null vs undefined
farklıdır. undefined hiç değer almamış anlamına gelir. örneğin;

```js
var a1;

var a2 = null;

typeof a1; //prints undefined

typeof a9; //prints undefined. a9 is not even typed before.

typeof a2; //prints null;
```

# data types
- Javascript'te number, boolean ve string birer primitive type'tır.

- fonksiyonlar (aşağıda detaylı anlatım var), typeof operatörü ile bakıldığında "fonksiyon" döndürürler. bu tipte olmalarının sebebi daha hiç new instance'ı ile oluşturulmamış olmasıdır.

- diğer data tiplerinin tümü (null dahil) objedir. array'ler objeden türemiş birer objedir. dolayısı ile objedir.

# Accessing Object Properties
- objectName.propertyName

- objectName["propertyName"]

# global vs local variable
local değerler metotların sadece içinden erişilebilir. oysa global değerler tüm program tarafından erişilebilir. bir metot içerisinde "var" ile variable tanımlaması yapılmadan bir bilgiye değer atanırsa; o değer otomatik olarak global olarak tanımlanır. fakat böyle bir şeyin yapılması kesinlikle tavsiye edilmez.

# === vs ==
- == karşılaştırılan değerlerin eşit olup olmadığına (valueOf() metotu ile) bakarken, === ek olarak tiplerinde eşit olup olmadığına bakar.

- zorunlu kalmadıkça == kontrolü yapmamak gerekli. mantığı biliyor olsakta, başkasının okumasını zorlaştırıyoruz.

- === kontrolü daha doğal ve basit. Javascript bilgisi dahi gerektirmiyor. tek istisna NaN'da oluyor. NaN === NaN -> false dönüyor.

- == için kurallar sırasıyla işletilmelidir:

  1- aynı tiptelerse === karşılaştırması yapılıyor. farklı tipler için aşağıdaki kurallar geçerlidir:

  2- null ve undefined eşittir

  3- sayı ve string karşılaştırılınca string önce sayıya çevrilir. JS içinde Number(string) metotu mevcut.

  4- en az bir operand boolean ise; true->1, false->0'ya çevirir ve tüm kuralları tekrar işletir.

  5- obje karşısında string yada number varsa, obje.valueOF çağrılır ve kurallar tekrar işletilir.

  6- diğer tüm durumlar false döner

# NaN
__Not-A-Number__ anlamına gelmektedir. isNaN gibi metotlar bazen beklenmedik sonuçlar verebilir. örneğin " " değeri 0 a eşit sayıldığından isNaN false döner.

NaN bilgisayar bilimlerinde bir sayıdır. örneğin 0/0 tanımlanamaz ve bu sebeple NaN döndürmelidir. bu sebeple Javascript'te typeof ile bakarsak; NaN, "number" tipindedir.

JS beklenmedik sonuçlar dönebilmektedir. birçok alışılmadık durumları buradan inceleyebiliriz: jsfuck.com

# debugger; keyword
bu keyword'ü içeren satırlar debug modda olan IDE tarafından durdurulurlar.

# Regular expressions
objeden türemişlerdir. Javascript'te özel syntax'ı vardır. tırnak içerisine almaya gerek yoktur.

# object (or nesne or tr:obje) vs instance
instance kelime anlamı: 1. örnek 2. hal/durum

"class", programlama dillerinde "nesne" tanımlayabilmek için gerekli bir tiptir. "User" eğer class tipindeyse nesnedir. User bir arayüzde olabilirdi.

Instance ise bellekte ayrı yeri olan klon nesnedir.

bu başlıkta ve bu notlarda, obje ve nesne terimlerini kullanırken doğru olmasına hiç dikkat edilmedi. ikiside birbiri yerine kullanılmıştır.

# Javascript function vs Javascript class
Javascript'te class yoktur (Javascript'te class Ecmascript 6 ile geldi). JS, sınıf olmamasına rağmen nesne tabanlı bir dildir. "nesne" yerine "prototip" kavramı vardır.

aşağıdaki person fonksiyonu bize objeyi döndürmek için kullanılan fonksiyondur. nesne döndürdüğü için constructor da denir. (hatta nesne döndürdüğü için class'ta denir fakat bu doğru değildir).

```js
function person(first) {

    this.firstName = first;

    this.lastName = null;

    this.changeLastName = function (name) {

        this.lastName = name;
    };
}

var myFather = new person("John"); // (inside "person" function) "this" (scope) is a new scope.
print(myFather.firstName);
print(typeof myFather); // object

var myMother = person("Esra"); // (inside "person" function) "this" (scope) is the current "window" object.
print(myMother); //prints undefined. If we had "return "ahmet" on last line of person function, myMother would be "Ahmet".
print(window.firstName);//prints Esra.
```

Ecmascript'in güncel standartlarında class'lar vardır. fakat geriye uyumluluk adına herkes tarafından kullanılması tercih edilmeyebiliyor. örnek:

```js
class User {
  constructor(name) { this.name = name; }
  sayHi() { alert(this.name); }
}

alert(typeof User); // function (yeni sürüm JS'teki "class" olmasına rağmen)
```

Yeni sürüm JS class'ları ile, eski sürüm JS function'ları tamamiyle aynı şeylerdir. 'Class' yapısı arkaplanda aynı işlevi bizim için yerine getirmektedir, yani syntactical sugar'dır. kaynak: (source-id: 313) title: "Defining a class", writes: "JavaScript classes, introduced in ECMAScript 2015, are primarily syntactical sugar over JavaScript's existing prototype-based inheritance. The class syntax does not introduce a new object-oriented inheritance model to JavaScript.".

Eski tip Function'larda extend işlemi yapılamaz. Function'un türettiği objede extend yapılabilir ("Javascript object" başlığında anlatılıyor). Oysa yeni JS'teki class'larda extend işlemi Java'daki syntax gibi yapılır.

# Javascript object

aşağıdaki (nesne2) tanımlama ile fonksiyonsuz bir şekilde obje türetmiş oluyoruz.

```js
var nesne2 = {

    Ad: 'Melih',

    Soyad: 'Orhan',

    AdSoyad: function () {

        return this.Ad + ' ' + this.Soyad
    }
}
```

Aşağıdaki şekilde inheritance yapabiliriz:

```js
// 2'inci parametrede descriptor'lar yollanıyor (başka başlıkta anlatılıyor).

var obj1 = Object.create(nesne2, {

    prop1: {
      writable: true,
      configurable: true,
      value: 'value1'
    },

    prop2: {
      writable: true,
      configurable: true,
      value: 'value2'
    },

});
```

Yukarıda nesne2 yerine "Object.prototype" yazarsak boş objeden türetmiş oluruz.

# descriptor
her property'nin descriptor'ları vardır. bir obje içine bir property eklediğimizde JS'in aşağıdaki metotunu kullanabiliriz. bu metot üzerinden gidersek descriptor'ları daha iyi anlayabiliriz.

```js
Object.defineProperty(obj, prop, descriptor)
```

örnek:

```js
Object.defineProperty(myObject, 'myInteger, {
  value: 99, // myInteger'ın değeri
  writable: true, // myInteger = 1; ile değiştirilip değiştirilemeyeceği
  enumerable: true, // for-loop ile dönüldüğünde bu değerin de döngüye girip girmeyeceği
  configurable: true, // property'nin silinip silinmeyeceği veya tipinin değiştirilip değiştirilmeyeceği
  get: function() { return val; },
  set: function(newValue) { val = newValue; }
});
```

Artık myObject.myInteger bize 99 u verecektir.

Dikkat edilirse defineProperty'nin aldığı tüm JSON elementine "descriptor" deniliyor. yani her biri birer descriptor değil. (bazen karışıklığa sebep olabiliyor. o yüzden bu cümleyi not aldım).

ES2017 ile getOwnPropertyDescriptors metotu gelmiştir (başka yerde anlatılıyor).

# own-property
bir nesnenin süper variable'ları bu kümeye dahil değildir. __hasOwnProperty("propertyToCheck")__ ile sorgulayabiliriz.

# fonksiyon ve obje içinde gelen property ve metotlar

#### fonksiyon properties:

- __length__

  fonksiyon kaç parametreli olduğunu tutar.

  ```js
  function func2(a, b) {}
  console.log(func2.length);
  // output: 2
  ```

- __arguments__

  (deprecated or not standart) fonksiyon çağrılınca ona yollanan argümanlar dizi olarak tutulur. bu argümanlar bu property'de tutulur.

- __arity__

  (deprecated or not standart) length ile aynı görevi görür. fakat length standartlarda olduğundan length kullanılmalıdır.

- __caller__

  (deprecated or not standart) fonksiyon o anda çağıran fonksiyon referansı.

- __display name__

  (deprecated or not standart) default olarak metotun kendi ismidir, fakat developer bunu istediği gibi ezebilir.

- __name__

  fonksiyon adının string halini tutar.

- __prototype__

  tipi JSON objesidir. array değildir. JS objesi olduğundan saf JS syntax'ında olduğu gibi buraya bir çok değer atayabiliriz:

  > myFunction.prototype.newProp = 99;

  prototype objesi, ilgili fonksiyondan üretilecek veya daha önceden üretilmiş her instance'a "__\_\_proto\_\___" property'si referans geçilir. bu sebeple buraya atadığımız her değer diğer eski ve yeni yaratılacak her instance'ın \_\_proto\_\_'sunda vardır.

  Aynı zamanda önceden veya sonradan oluşturulan instance'lara prop olarak kopyalanır. örnek:

  ```js
  myFunction.prototype.newProp = 99;

  instanceOfFunc.newProp; // 99

  instanceOfFunc.__proto__.newProp; // 99

  instanceOfFunc.prototype; // objelerin prototype'ı olmaz. sadece function'ların prototype'ı vardır.
  ```

  prototype ile yeni prop eklemek çok kolaydır. fakat var olan prop'u güncellemek daha karmaşıktır. zira instance'ı üretmek için kullanılan constructor, zaten prop'u override edecektir. Bu durumu aşağıdaki örnekten anlayabiliriz:

  ```js
  let f = function () {
    this.a = 1;
    this.b = 2;
  }
  let o = new f();

  f.prototype.b = 3;
  f.prototype.c = 4;
  // do not set the prototype f.prototype = {b:3,c:4}; this will break the prototype chain. because prototype has also super prototype.

  // o.__proto__ ---> has properties b and c
  // o.__proto__.__proto__ --> is Object.prototype
  // o.__proto__.__proto__.__proto__ --> is null.

  console.log(o.b); // 2
  // Is there a 'b' own property on o? Yes, and its value is 2.
  // The prototype also has a 'b' property, but it's not visited.
  // This is called Property Shadowing.
  ```

#### fonksiyonlar:

- __call(thisArg, arg1, arg2 ...)__

  ilgili fonksiyonu execute eder. args ile execute ederken göndereceğimiz parametreleri bir dizide göndeririz. thisArgs ise, fonksiyonun içindeki "this" scope'unu ezmek için kullanırız.

- __apply(thisArg, args)__

  "call" ile aynı görevi görür. sadece parametre alışı farklıdır. args bir dizi olmak zorundadır.

- __bind(thisArg, arg1, arg2 ...)__

  "call" ile aynı görevi görür. tek farkı; "bind", metotu execute etmez. ona parametreleri geçer. metot farklı satırda execute edilebilir. eğer argümanlar(args1, args2...) yollanmazsa; fonksiyon bir sonraki kod bloklarında execute edildiğinde de verilebilir. bind metotunda argümanlar verilirse; ileride execute ederken vermek gerekmez.

- __isGenerator()__

  (deprecated or not standart) fonksiyon içerisinde "yield" keywordu bulunup bulunmadığını döndürür. (yield başka başlıkta anlatılıyor)

- __toSource()__

  (deprecated or not standart) fonksiyon içindeki kod bloğunu string olarak döndürür.

- __toString()__

  toSource() ile aynı görevi görür. fakat toString daha standarttır.

#### objelerin içerdiği fonksiyonlar:

- __valueOf()__

  o objenin primitive değerini döndürmelidir. == işaretinin yaptığı karşılaştırma için kullanılır. bu sebeple unique bir primitive döndürmelidir. valueOf() metotu objenin tipinden bağımsız bir eşitlik kontrolüdür. (başka başlıkta == vs === anlatılıyor)

  istisna olarak bir array'in valueOf'u array'in kendisini döner.

  eğer valueOf, developer tarafından hiç override edilmediyse JSON objesi döner. bu JSON objesinde ne olduğuna örnek üzerinden bakarsak:

  ```js
  function person(first) {

      this.firstName = first;

      this.lastName = null;

      this.changeLastName = function (name) {

          this.lastName = name;
      };
  }

  var myFather = new person("John");

  print(myFather.firstName);
  // output:
  // {
  //   firstName: John,
  //   lastName: null,
  //   changeLastName = function (name) {
  //         this.lastName = name;
  //   },
  //   __proto__: {}
  // }
  ```

# accessor property
Özel bir syntax'tır. "get a()" satırı, "object.a" dediğimizde döndürülecek değeri belirler. benzer şekilde "set a(v)" satırı, onu set ettiğimizde çalıştırılacak metotu çalıştırır. örnek:

```js
var o = {

  n: 1,

  get a(){ return this.n; },

  set a(v){ this.n = v*v; }
};

o.a // prints 1

o.a = 4;  //set ettiğimiz satır

o.a // prints 16
```

# JS'te bir syntax örneği

```js
(  function(name) {
              console.log("hello " + name)
}) ("ahmet")
```

Burada, "name" parametresi alan anonim bir fonksiyon tanımlanıyor. Bu fonksiyonu oluşturuyoruz (parantez içerisinde) ve hemen yanında metot çağırdığımızda yaptığımız gibi parantez içinde parametrelerini veriyoruz ve anonim fonksiyonu execute ediyoruz. sonuç olarak console'da "hello ahmet" çıktısını alıyoruz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# CommonJS module API vs AMD (Asynchronous Module Definition) vs ECMAScript module vs SystemJS
JS dosyaları yazıldığında ve daha sonra kullanılabilir hale getirilmesi için belli bir formatta yazılması gerekmektedir. bu formatların standartlarıdır.

balıkta yazanlar 3 farklı en çok tercih edilen standartlardır.

"ECMAScript module" ECMAScript 6 ile gelmiştir.

CommonJS; sadece sync module load'a izin veriyor. AMD ise buna çözüm olarak sadece asenkron load edilebilen modül formatı olarak ortaya çıkmıştır. Daha sonrasında ECMAScript modüle tanımı getirilmiştir. Fakat geriye uyumluluk ihtiyacı olduğundan pek piyasada tercih edilemiyor.

Yukarıdaki alternatifleri kullanmadan da kendimiz ayrı ayrı dosyalarda ayrı Javascript objeleri tutup, kendimizde module benzeri bir altyapı kurabiliriz. Fakat bu, static code analyzer'ler tarafından algılanmayacağı için sıkıntı yaşayabiliriz.

Yukarıdaki tüm alternatifler (SystemJS'in nasıl çalıştığını bilmiyorum. bu sebeple onu ayrı değerlendirmek gerekebilir) aslında birer syntax'tır. Bunların çalışabilmeleri için runtime veya build sırasında destek şarttır. Dolayısı ile implementasyona ihtiyacımız var. Bu implementasyonlar şunlar olabilir: web browser JS motoru, NodeJS, RequireJS, Dojo, Webpack and Browserify...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# require.js
Javascript kütüphanesi. sadece ilgili JS modüllerinin yüklenmesini sağlamaktadır. yüklenen modüllerin bağımlılıkları varsa onları da otomatik yüklemektedir.

requirejs(['app/main']); ile istediğimiz modülün yüklenmesini sağlıyoruz.

modül tanımlamasını şu şekilde yapıyoruz:

```js
define("alpha", ["credits","products"], function(credits,products) {

        //my module code is here

});
```

bağımlılık olmadığı durumlarda define ikinci aldığı dizi parametresini hiç almayabilir. alpha ise bu tanımlayacağımız modülün ID'si. biri dependency ile bu ID ile çağırıyor bizim modülü.

require("myLib") ile bize "myLib" ID'li kütüphaneyi dönüyor. bu objeyi kullanarak; içinden bir metot çağırabiliriz:

require("myLib").myMethod();

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# library oluştururken genelde yapılan tanımlamalar:

```js
if (typeof define === 'function' && define['amd']) {

     define(['jquery'], factory);

} else {

// other codes here
```

Burada "factory" takip edilirse; API'nin (kütüphanenin) olduğu kodu olduğu görülür.

"define" bir fonksiyon mu diye kontrol edilmiş. "define" RequireJS'in modül tanıtmak için public sunduğu bir metot'dur. eğer bu varsa require.js vardır demek oluyor. require.js var ise; define['amd'] ile "define" içerisinde AMD desteğinin olup olmadığına bakılır.

ek not:

```js
define['amd'] = define.amd // true
```

eğer AMD support var ise if bloğuna giriliyor. require.js ile yeni bir modül yaratıyor. 'JQuery' dependency'sini alacak şekilde yeni modülümüz tanımlanıyor. else durumlarda require.js yokken yapılacaklar yazılıyor.

- örnek

```js
(function (global) {

    'use strict';

    var MyModule = function () {

      /* Your awesome module logic here... */

    };

    // ...and here

    MyModule.foo = 'bar';

    // AMD support

    if (typeof define === 'function' && define.amd) {

        define(function () { return MyModule; });

    // CommonJS and Node.js module support.

    } else if (typeof exports !== 'undefined') {

        // Support Node.js specific `module.exports` (which can be a function)

        if (typeof module !== 'undefined' && module.exports) {

            exports = module.exports = MyModule;

        }

        // But always support CommonJS module 1.1.1 spec ('exports' cannot be a function)

        exports.MyModule = MyModule;

    } else {

        // Eğer AMD, NodeJS, CommonJS yoksa window.mymodule ile erişilebilmesi için bu işlem yapılıyor.

        global.MyModule = MyModule;
    }
})(this);
```

sondaki this ile Microsoft Windows objesi yakalanmış oluyor. kendini execute eden metot "global" parametresi ile this'i (window'u) alıyor.

Kütüphaneler kendini parantez içine alıp execute ediyor. Eğer bunu yapmazlarsa "var" oluşturdukları her variable global scope'ta tanımlanır. Bu da diğer lib'lerdeki variable'lerle çakışmasına sebep olabilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# web page lifecycle

- #### events

  - > $(document).ready( func );

    readystate interactive olduğunda bu metotu JQuery çalıştırmaktadır. (readystate aşağıda anlatılıyor)

  - > document.addEventListener("DOMContentLoaded", func);

    Executes when HTML-Document is loaded and DOM is ready to run any Javascript safely.

    It waits to execute all internal inline Javascript's (script tags) and also external src Javascript documents. only exception is "async" and "defer" attributes of external scripts.

    browser'ların autofill-form (otomatik form tamamlama) DOMContentLoaded event'ini beklemektedir.

    DOMContentLoaded eğer sayfa yüklendikten sonra handler'a set edilirse hiçbir zaman execute edilmez.

  - > $(window).load( func ); yada window.onload = func;

    iframe'lerin içi yüklendiğinde, resimler tamamen indirildiğinde bu metot çağrılır

  - > window.onunload = func;

    kullanıcı sekmeyi kapattığı zaman

  - > window.onbeforeunload = func;

    kullanıcı sekmeyi kapatmak istediği zaman. burada tek seçeneğimiz string döndürmek:

    ```js
    window.onbeforeunload = function() {

        return "There are unsaved changes. Leave now?";
    };
    ```

    bu string'i tarayıcı bir dialog'da gösteriyor ve evet hayır seçeneğini sunuyor. evet seçeneğini seçerse sekme kapatılıyor. hayır'ı seçerse kapatılmıyor.


- #### readystate

DOMContentLoaded sadece readystate loading durumundayken handler'a attach edilebilir.

```js
if (document.readyState == 'loading') {

  document.addEventListener('DOMContentLoaded', func);
}
```

document readyState's:

> "loading" – the document is loading.

> "interactive" – the document was fully read.

> "complete" – the document was fully read and all resources (like images) are loaded too.

readystate'in değiştiği event'i de yakalayabiliriz:

```js
document.addEventListener('readystatechange', func );
```

- #### event'ler ile ilgili notlar
  - aşağıdaki iki metot aynı şeyi yapar:

    - $(window).load( func );

    - $(window).on('load', func )

    Yukarıdaki load metotu document'te alabilir, button da alabilir.

  - aşağıdaki iki code ayni anlama geliyor. JQuery sık kullanıldığı için bu şekilde ayarlanmış.

    - $( document ).ready( func );

    - $( func );

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# $ prefix on variables
Javascript kodlarında çoğu zaman JQuery'nin namespace'i olan $ işareti görülür. bunun sebebi oluşturulan variable'ın JQuery'den dönen bir değeri tuttuğunu belli etmektir. örneğin;

var a = 3;
var $myObject = $("#email");

buna benzer bir durum birçok diğer kütüphaneyi kullanan programcıların yaptığı görülebilir. örneğin; underscore JS kullananlar
\_myVariable diye adlandırırlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# namespace

Javascript'te en global değerler namespace olarak kullanılır. örneğin bir kütüphane eklediğimizde kendini buraya bir tanımlama yaparak ekler. bu şekilde herkes buradan onu çağırır. bu sebeple Javascript'te buradaki değerler namespace'indendir.

genelde bu tanımlama ile init yapılır:

var MAYAPPLICATION = MYAPPLICATION || {};

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# yield keyword

yield Türkçe kelime anlamı: esneme.

fonksiyona kalındığı yerden devam edilebilmesini sağlar. yield return eder, fakat bir "Generator" objesi döndürür. "function \*" ibaresindeki yıldız işareti fonksiyonun bir generator objesi döndürdüğünü temsil eder. örnek:

```js
function* foo(x) {

    while (true) {

        x = x * 2;
        yield x;
    }
}

var g = foo(2);

g.next(); // -> 4

g.next(); // -> 8

g.next(); // -> 16
```

# yield*
yield içindekileri bir üstteki metotun generator'ı ile birleştirmek istersek yield# kullanırız. örnek:

```js
function* g1() {

  yield 2;

  yield 3;

  yield 4;
}

function* g2() {

  yield 1;

  yield* g1();

  yield 5;
}

var iterator = g2();

console.log(iterator.next()); // {value: 1, done: false}

console.log(iterator.next()); // {value: 2, done: false}

console.log(iterator.next()); // {value: 3, done: false}

console.log(iterator.next()); // {value: 4, done: false}

console.log(iterator.next()); // {value: 5, done: false}

console.log(iterator.next()); // {value: undefined, done: true}
```

# yield ile hiçbir şey döndürülmediğinde ne olur?
yield keyword'unun yanında hiçbir şey olmadığında, örnek:

```js
function* g1() {

  yield;
}
```

iterator şunu döner: {value: undefined, done: true};

# generator nasıl done=true olduğunu anlıyor?
eğer yield'dan sonra herhangi bir kod satırı var ise; done:false oluyor.

# var iterator = g2();
"g2" bir generator dönen obje ise; "var iterator" artık generator objesidir ve "g2()" kodunda parentez açılmasına rağmen; hiçbir kod execute edilmez. g2'nin ilk satırları dahi ancak iterator.next() dediğimiz zaman çağrılır.

# yield kullanılan fonksiyonlarda return keyword'ü
return de iterator next metotunda yield gibi değer döndürür. yield ile apaynı davranışı sergiler. fakat her zaman done:true'dur. bundan sonra iterator devam etmez. tek fark budur.

return değeri aşağıdaki gibi değer atama görevi görmektedir.

```js
function# g1() {

  yield 2;
  return 3;
}

function# g2() {

  yield 1;
  var a = yield# g1();
  console.log(a);
  yield 4;
}

var iterator = g2();

console.log(iterator.next()); // {value: 1, done: false}

console.log(iterator.next()); // {value: 2, done: false}

console.log(iterator.next()); // {value: 3, done: false}

3 //console.log(a) çıktısı

console.log(iterator.next()); // {value: 4, done: false}

console.log(iterator.next()); // {value: undefined, done: true}
```

Yukarıda "var a = yield# g1();" daki satırda # karakteri hiç kullanılmasaydı:

var a = yield g1();

a değerine hiçbir değer atanmayacaktı. generator'ımız;

{value: 2, done: false}

yerine;

{value: Generator, done: false}

dönecekti. ve a değeri undefined olacağından console.log undefined basacaktı. fakat; next(); 'e yollayacağımız ilk parametre a'ya atanıyor olacaktı. aşağıda direk örnekle gidelim:

var a = yield g1(); satırını işleten next() metotundan sonraki next() metotuna parametre geçseydik: next(99); o zaman "var a" değerimiz 99 olacaktı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# noConflict metotları
Javascript'te namespace'ler diğer dillerdeki gibi düzenli yönetilemeyebiliyor. karışıklık olmaması için her kütüphane en tepede noConflict isimli bir metot tanımlıyor. örneğin JQuery'de bu metot şunu yapıyor.

```js
var j = jQuery.noConflict();
j( "div p" ).hide();
$( "content" ).style.display = "none";
```

Yukarıda noConflict metotuna true parametresi geçilseydi artık 3 üncü satırda $ işareti de kullanılamaz hale gelecekti. noConflict(true)'yu, $ işaretini başka bir kütüphane kullanıyor ise yapabiliriz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# arrays

- #### obje'den türemiştir.

var cars = ["Porsche", "Volvo", "BMW"];

var cars = new Array("Porsche", "Volvo", "BMW"); // array fonksiyonu.

new Array tercih edilmemeli, çünkü yanlış anlaşılma olabilir. zira constructor tek parametre ve sayı alınca boş array oluşturuyor:

new Array(20); //20'lik bir dizi oluştu. bazıları bunu 1 elemanlı bir dizi olduğunu düşünebilir.

- #### erişmek için:

> cars[1];

Javascript'te diziler birer objedir. aslıda her dizi elemanı bir property'dir. sadece access'i farklıdır. bunun sebebi:

herhangi bir objede 0 veya 1 isimli property'ye şu şekilde erişebiliyoruz:

> myArr["0"]

yada

> myArr[0]

Sayı vererek eriştiğimizde aslında; Javascript interpreter'ı onu string'e çeviriyor. Bu sebeple myArr["0"] değeri myArr[0] ile aynı şeyi veriyor.

Yani bu erişim dizilere özgü bir durum değildir. dizilerin içindeki metotlar otomatik property'leri sayısal cinsten tutmaktadır. örneğin "JQuery objesi" (konu başka başlıkta anlatılıyor) dizi dönmüyor fakat liste içinden bir elemana erişmek istediğimizde; yine index ile erişebiliyoruz.

- #### array'in hazır metotları:

- cars.length;

- cars.sort(); default sorts alphabetically.

  sort; isteğe bağlı olarak bir parametre olarak fonksiyon alıyor. örnek:

  cars.sort(function(a, b){

    return (b - a);
  });

  negatif değer dönerse a, b den küçük demektir. tüm dizinin elemanları birbiri ile karşılaştırıyor.

- fruits.pop(); // removes last element

- fruits.push("Banana"); //adds element to last index and return the last new index

- fruits.shift(); // removes first element

- fruits.unshift("Lemon"); // adds element to first index

- fruits.splice(2, 4, "Lemon", "Banana"); //2'inci elemandan itibaren, 4 tane eleman siliyor (silinen elemanların index'leri: 3,4,5,6) + 2'inci index'ten itibaren sağ tarafta olan bütün argümanları; "Lemon" ve "Banana"'yi diziye ekliyor ekliyor. örnek:

   var fruits = ["1", "2", "3", "4"];

   fruits.splice(2, 0, "x", "y");

   prints(fruits); // prints 1,2,x,y,3,4

- arr1.concat(arr2, arr3);

  sırası ile birleştiriyor. sonsuz parametre kabul ediyor.

- fruits.slice(1,3); returns new array from 1-3 index

# For loop comparison table

| how to call                                                      | change scope/context | iterate over objects | how to break                              | access all array list | change orijinal variable | iterate over empty slots |
|------------------------------------------------------------------|----------------------|----------------------|-------------------------------------------|-----------------------|-------------------------|--------------------------|
| lodash.forEach(arr,   function( index, val )); (or)  lodash.each | false                | true                 | return  false/null/undefined  on callback | false                 | true (read NOT-A)       | true                     |
| JQuery.each( arr,  function( index, val ));                      | false                | true                 | return false/null/undefined on callback   | false                 | true (read NOT-A)       | true                     |
| for(val in arr) { ... }                                          | true                 | true                 | break keyword                             | true                  | true                    | false                    |
| for(i=0; i<arr.length; i++) { ... }                              | true                 | false                | break keyword                             | true                  | true                    | true                     |
| underscore.each(arr,function(val, index, arr),context)           | true                 | true                 | not possible                              | true                  | true (read NOT-A)       | true                     |
| underscore.every(arr,function(val, index, arr),context)          | true                 | true                 | return false/null/undefined on callback   | true                  | true (read NOT-A)       | true                     |
| arr.forEach(function(val, index, arr), context);                 | true                 | false                | not possible                              | true                  | true (read NOT-A)       | false                    |

NOT-A:

orijinal elemanı callback'e geçiyor. dolayısı ile değişkenler burada değiştirilebilirler. fakat genel Javascript kuralları gereği bilinmesi gerekenler var. callback metotu yeni bir fonksiyon olacağı için geçilen değerler 2 durumda değiştirilemezler:

1- eğer yeni bir obje ataması yapılırsa (çünkü yeni objenin referansı farklı. orijinal  obje'nin içeriği değiştirilmemiş olacak. ve orijinal dizi; referansla elemanları tutuyor.

2- değiştirilen eleman bir primitive (string, int...) ise. çünkü primitive'ler metotlara atanırken sadece değerleri geçer (referansları değil).

Bu iki durum için geçici olarak "access all array list" kuralı true ise; orijinal objenin içindeki referans manuel değiştirilir.


- #### Tablo hakkında

- lodash.forEachRight ve lodash.eachRight aynı metotlardır. lodash.each ile aynı özelliklere sahipler. tek farkı liste elemanlarının sırasını tersten dönmesidir.

- "iterate over empty slots" kuralı:

   boş slotlar (hiç atanmamış değerler) için callback çağrılıyor dönmüyor.

   örnek:

   ```js
   var a = ['a0'];

   a[3] = 'a3';

   a[4] = undefined;

   a[5] = null;
   ```

   sadece a[1] ve a[2] boş. bunlar için for loop callback çağrılması.

   eğer boş olup olmadığı kütüphane içinde kontrol ediliyorsa bu yavaşlığa sebep olmaktadır.

- "iterate over object" kuralı:

   eğer list bir JS objesi ise, yine döngü çalışıyor ve her property'si dönmeye başlıyor.

   metotun aldığı parametreler'in anlamı değişiyor:

   list array ise; (value, index)

   list obje ise; (value, key)

- elle for döngüsünün yazılması ( for(i++) şeklinde ) diğer tüm döngülerden çok daha performanslı çalışmaktadır.

- $.each() metotu ile $(selector).each() aynı değildir. $(selector).each() bir JQuery objesinin içerisinde dönmeye yarıyor.

- çoğu kütüphanenin "map" metotları da vardır. bu metotlar for metotları ile aynı parametreleri alır. map metotları callback metotlarından return edilen her elemanı yeni bir diziye atar. bu yeni diziyi de map metotu return eder. map metotunun temel felsefesi budur. for yerine map yada map yerine for kullanmak anti-pattern'dir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Ecmascript vs Javascript vs Jscript vs ActionScript
Javascript ve Jscript vs ActionScript; birer Ecmascript implementasyonudur. buna rağmen, bazı implementasyonlar kendilerince spesifikasyon dışı ek özellikler içerebilirler.

# CoffeeScript vs typescript
Javascript'e çevrilen bir programlama dilleridir.

# source-to-source compiler (or transcompiler or transpiler)
Bir dil, derlenmeden önce başka bir dile çevrilebilir. Bu işlemi yapan çeviriciye transpiler denir.

# ECMAScript
ECMA-262 ve ISO/IEC 16262 standartları ile oluşturulan betik dilidir.

# ECMAScript (or ES) version history

| Name                                               | Date Puplished |
|----------------------------------------------------|----------------|
| ES5                                                | 2009           |
| ES5.1                                              | 2011           |
| ECMAScript 2015 (or ES2015 or ECMAScript 6 or ES6) | 2015           |
| ECMAScript 2016 (or ES2016 or ECMAScript 7 or ES7) | 2016           |
| ECMAScript 2017 (or ES2017 or ECMAScript 8 or ES8) | 2017           |
| ECMAScript 2018 (or ES2018 or ECMAScript 9 or ES9) | 2018           |

"ES.Next" ECMAScript'in br sonraki versiyonunu belirten özel bir takma addır.

Ecmascript ile Javascript'in versiyonları aynıdır. Çünkü Ecmascript'in referans implementasyonu Javascript'tir.

Typescript'in herhangi bir sürümü, istenilenilen Javascript'in herhangi bir sürümüne çevrilebilir. ancak istisna olarak bazı özellikler Javascript'te hiç desteklenmiyor olabilir. böyle durumlarda, JS tarafında sürüm yükseltmek gerekebilir.

# Ecmascript implementasyonları standardize edilirken, her sürüm kendi içinde belli sürümlere ayrılır.

| sürüm kodu | sürüm adı | açıklama                                          |
|------------|-----------|---------------------------------------------------|
| stage-0    | Strawman  | just an idea, possible Babel plugin               |
| stage-1    | Proposal  | this is worth working on                          |
| stage-2    | Draft     | initial spec                                      |
| stage-3    | Candidate | complete spec and initial browser implementations |
| stage-4    | Finished  | will be added to the next yearly release          |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Browser Object Model (or BOM)
HTML tarayıcılarının Javascript'e ekledikleri Javascript objeleridir. bu objeler NodeJS gibi ortamlarda olmuyorlar.

## window

genel bir objedir. JS'te global tanımlanan her değer buraya bağlanmaktadır. örnek metot:

> window.close()

## window.screen

fiziksel ekranla ilgili bilgiler içeren Javascript objesidir. örnek:

> screen.width

> screen.height

## window.location

URL ile ilgili metotlar buradadır.

> window.location.href

## window.history

> history.back();

## window.navigator

tarayıcının user agent bilgilerini barındırır.

## document.cookie

cookie metotları buradadır.

## document

Microsoft Windows.document ile de aynı objeye erişilebiliyor. DOM ile ilgili metotlar buradadır. örnek:

> document.appendChild(element);

> document.write(text);

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# JQuery object (or JQuery collection object)

$( 'li' ); metotundan dönen obje, JQuery objesi olarak adlandırılır. bu adlandırma JQuery'nin kendi içinde belirlediği bir standarttır. yazılımcıların birbiri arasında anlaşabilmesi için konulan özel bir isimdir. Javascript'te herşey ( $, JQuery dahi ) birer obje olduğundan "Javascript objesi" ile isim karışıklığı yaşanabilmektedir.

JQuery object, array objesi ile benzer property'ler içeriyor fakat Array yada Array'den türemiş bir obje değildir.

Örnek kullanımlar:

```js
var listItems = $( 'li' );
var rawListItem = listItems[0]; // or listItems.get( 0 );
$( '<p>Hello!</p>' ); //creates new element

//search inside a spesific block
var paragraph = $( 'p' );
$( 'a', paragraph );
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Javascript executes code in 3 phases

1- First, the engine walks through the code, looks for function declarations and hoists them (moves them to the top)

2- it hoists variable declarations

3- finally it runs the "normalized" code.

example:

```js
function bar() {

        return foo; // foo değerinin tanımı yok.

        foo = 10; // foo değerinin hala tanımı yok.

        function foo() {} // foo değerini tanımlaması burada. yukarıda eksikti. bu sebeple kodun en başına alınacak.

        var foo = 11; // foo değerinin artık tanımlanması var. "var" keyword'üne artık gerek yok.
}
```

"normalized" code:

```js
function bar() {

    function foo() {}

    return foo; // kod execute edildiğinde kod bu kısımda sonlanacaktır. aşağıdaki kodlar unused'dır.

    foo = 10;

    foo = 11;
}
```
