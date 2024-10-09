############################

############################
# JAVASCRIPT NEW FEATURES BY VERSION
############################

############################

# ES2017 ile gelen bazı yenilikler

- #### async ve await keywords

"await" is also verb and it's synonim of "wait".

Promise (Ecmascript güncel sürümleri ile gelen nesne) kullanımı için Syntactic sugar'dır. Örnek:

```js
async function f() {
  return 1;
}

f().then(alert);
```

Aşağıdaki ile tamamen aynıdır:

```js
function f() {
  return Promise.resolve(1);
}

f().then(alert);
```

await keyword'ü ise o satırın senkron çalışmasını; yani bitmesini bekleyene kadar dönmesini sağlıyor. Bunu yaparken, tam await satırına geldiğimizde ilgili metodun Promise dönmesini sağlıyor. Bu sebeple her await kullanılan fonksiyon, async olmak zorunda. Bunu daha iyi anlamak için aşağıdaki örneğe bakalım. Ekrana basılma sırası console-log'larda doğru yazılmıştır.

```js
async function f() {

  let promise = new Promise((resolve, reject) => {
    setTimeout(() => resolve("5"), 2000)
  });
  console.log("1");
  let result = await promise; // burada f fonksiyonu birşey return etmiyor. bu anda, f fonskiyonu çağıran kod, kaldığı yerden çalışamaya devam ediyor.
  console.log("4");
  console.log("result:" + result);// this line prints 5.
  return "6"; // bu değer Promise'e resolve olarak yollanıyor ve "f" fonskiyonundan return ediliyor.
}

var result = f(); // the type of result is Promise.
console.log("2");
result.then(console.log);// this line prints 6.
console.log("3");
```

Ek olarak şöyle bir yapı var: await'ten mutlaka Promise dönmek zorunda değiliz. Herhangi bir isimde bir class yaratırız. Bu class'ın Promise gibi metodları vardır (constructor, then...). Bu sınıfıda dönersek yine Promise dönmğşçesine kod işleyecektir. Bu yarattığımız class'a "Thenable" deniliyor.

Ek not: Eğer bir fonksiyon bizden Promise bekliyor ise; fakat bizim yapımız senkron ise, o zaman oraya Promise nesnesini şu şekilde geçebiliriz:

```js
// promise1'i 3üncü parti bir fonksiyona geçebilmek için yarattık:
const promise1 = Promise.resolve(123);

// artik 3üncü taraf bu Promise instance'ını istediği gibi kullanabilir.
promise1.then((value) => {
  // buradaki then fonskiyonu çağrıldığı anda execute edilecektir. Çünkü zaten Promise çoktan resolve oldu.
  console.log(value);
  // Expected output: 123
});
```

- #### Object.values()

```js
const person = { name: 'Ahmet', age: 28 }
Object.values(person) // ['Ahmet', 28]
```

Array içinde aynı özellik geçerlidir.

- #### Object.values()

```js
const person = { name: 'Ahmet', age: 28 }
Object.entries(person) // [['name', 'Ahmet'], ['age', 28]]
```

Array içinde aynı özellik geçerlidir.

- #### Object.getOwnPropertyDescriptors()

setter işlemimizi ezmiş olalım:

```js
const person1 = {
    set name(newName) {
        console.log(newName)
    }
}
```

artık bu işimizi görmeyecektir:

```js
const person2 = {}
Object.assign(person2, person1)
```

fakat getOwnPropertyDescriptors metotu bize istediklerimizi döndürecektir.

```js
const person3 = {}
Object.defineProperties(person3, Object.getOwnPropertyDescriptors(person1))
```

# ES2016 ile gelen bazı yenilikler

- #### Exponentiation Operator

Math.pow(4, 2) == 4 ** 2

# ES2015 ile gelen bazı yenilikler

- #### Default Parameter Values

```js
function f (x, y = 7) {

  return x + y;
}
```

- #### class yapıları (extend, constructor desteği ile)

- #### let operatörü (Block-Scoped Variables)

let a=5;

let for gibi döngülerin içerisinde tanımlandığında örnek:

for(let i=0; ...) ...

for dışından okunamaz olur. oysa "var" okunabiliyordu. Aynı durum if içinde geçerli. if içinde let tanımlaması if dışında geçersiz oluyor.

aynı zamanda let ile tanımlanan değerler, aşağıdaki satırlarda tekrar tanımlanamıyor. hata fırlatıyor. oysa var operatöründe aynı değer tekrar tanımlanabiliyordu.

- #### const keyword

Constants kelimesinden geliyor. Java'daki final'a denk geliyor.

```js
const a = 2;
```

const'ta "let" ile aynı mantıkta block-scoped variable yaratıyor. yani if, while gibi blokların dışından okunamıyor.

- #### Template literals (or Template strings)

  ` karakteri ile yazılan string'lerde bir çok özellik mevcut. tüm özellikler burada: (source-id: 218) bunlardan bazıları:

  - multi-line string

    ```js
    var myStr = `hello
    world`;
    ```

  - Expression interpolation

    ```js
    var myStr = `first element is: ${a} . but both element total are: ${a + b}`
    ```

    __interpolation (or tr: enterpolasyon)__ sözlükte şu anlama gelir: Bir serideki eksik verilerin hesaplanabilmesi için geliştirilen bir matematiksel yöntem.

    Ek not: Programlama dilinden bağımsız olarak String üzerinde yapılabilen işlemleri aşağıdaki gibi gruplandırabiliriz:

    ```java
    // ** "String interpolation".
    // ** variable substitution (tr:yerine geçme), terimi "String interpolation"ın daha genel halidir. yerine geçecek data'nın tipinin belirtmeden kullanılan genel bir terimdir.
    // - variable expansion (tr:genleşme), variable substitution ile aynı kapıya çıkmaktadır.
    // ** (Yukarıda anlatılan) "Expression interpolation" buradaki ile
    // benzer durumun expression'lar üzerinden uygulanmasıdır.
    print("I have ${apples} apples.")

    // "String concatenation"
    print("I have " + apples + " apples.")

    // "format String"
    print("I have %s apples.", apples)
    ```

- #### String format

```js
var soyad="Aykaç";

var fullName=`Merhaba ${soyad}`;
```

- ### Set liste türü

aynı elemanlar dublicate olmuyor. örnek:

```js
var items=new Set();
items.add(1);
```

- #### Iterators

- #### Maps

key/value tutan liste türü.

- #### WeakMap

Maps ile aynı. sadece enumerable değildir.

- #### is ve isnt operatörü

Java'daki instanceOf operatörü ile aynı işi görüyor.

- #### Destructuring Assignment

aynı satırda çoklu değer atama işlemleri sağlanıyor. örnek:

var [a,b] = [9,3];

//eğer sağ tarafta fazladan eleman var ise, sadece sol tarafta istenmeyen eleman yerine boşluk atanabiliyor.

var list = [ 1, 2, 3 ];

var [ a, , b ] = list;

- #### sınırsız parametre

```js
function argsTester(...theArgs){

  alert(theArgs.length);
}
```

burada the args aynı tipte olan verileri tutmuyor. tüm objeleri bir dizide tutuyor.

- #### Expression Bodies

```js
odds  = evens.map(v => v + 1)

pairs = evens.map(v => ({ even: v, odd: v + 1 }))

nums  = evens.map((v, i) => v + i)
```

sırası ile aşağıdaki satırlara denk gelmektedir:

```js
odds  = evens.map(function (v) { return v + 1; });

pairs = evens.map(function (v) { return { even: v, odd: v + 1 }; });

nums  = evens.map(function (v, i) { return v + i; });
```

- #### parametre birleştirme

var params = [ "hello", true, 7 ];

var other = [ 1, 2, ...params ]; // [ 1, 2, "hello", true, 7 ]


- #### parantezsiz metot çağırma işlemi

ajax("Google.com")

yerine artık:

ajax "Google.com"

yazılabilir.

- #### direct support for octal and binary

```JS
0b111110111 // 503;

0o767 // 503;
```

- #### Property Shorthand

JSON değerleri sağ taraftaki ile aynı isme sahip ise atama yapmaya gerek kalmıyor.

```js
obj = {

        x,

        y };
```

eskiden:

```js
obj = { x: x,

        y: y

      };
```

- #### module Value Export/Import

```js
//  lib/math.js
export var pi = 3;

//  someApp.js
import # as math from "lib/math";
console.log(math.pi);

//  otherApp.js
import { pi } from "lib/math";
console.log(pi);
```

- #### sınıf içindeki metotlara static operatörü geldi

- #### Generators (yield keyword)

- #### Promise nesnesi geldi
