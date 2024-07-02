############################

############################
# WEB CUSTOM COMPONENTS
############################

############################

# micro-frontends (or micro frontends)
web-component teknolojilerinden yararlanarak, her component'i farklı projeler olarak geliştirmeyi ve bunları önyüzde hepsini birlikte load edip çalıştırma yöntemidir.

- web-component altyapısı kullanılmazsa, web-component'i sağlayan polyfill'lerden yararlanılması şarttır.
- Hepsini birlikte load edip etmeyeceğimiz aslında bize kalmış, ihtiyaç durumunda da load edilebilir. (kısmen business'e bağlı)
- Her proje tamamen farklı repo'larda geliştirilebilir ve farklı teknolojiler içerebilir.
- Tüm projeler aslında tek bir proje tarafından call edilmelidir. Bu main proje web sitemizin ilk açıldığı index.html'dir. Burada tüm diğer projeler import edileceği için, isim çakışması olmaması gerekir. Bunun için ya web-pack'e ayar yapacağız yada her proje baştan kendi standartlarını (diğer projelerle çakışmayan prefix'lerini) belirleyecek.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# reflow
reflow is the name of the web browser process for re-calculating the positions and geometries of elements in the document, for the purpose of re-rendering part or all of the document.

# DocumentFragment
bunun "web component" ile hiçbir bağlantısı yoktur.

Elimizde bir dizi olsun ve dizinin her elemanını bir DOM nesnesinin içine append edelim. Bunun için for-loop döner ve her defasında append child çağırırız. İşte her appendChild metotunu çağırdığımızda re-flow gerçekleşir. Performansı olumsuz etkiler.

Bunu önlemek için boş bir DocumentFragment yaratırız ve bu DocumentFragment'a ekleme yaparız. En sondada bu DocumentFragment'ı append ederiz.

Bunun yerine boş bir div element'i oluşturup içine RAM'de ekleme yapıp for-loop bittiğinde RAM'deki nesnesi tek hamlede de append edebilirdik. AYnı kapıya çıkardı. Fakat DocumentFragment bu iş için sunuldu ve gereksiz div elementleri oluşturmaktan kaçmamızı sağladı.

Boş bir DocumentFragment oluşturma:

```js
let fragment = new DocumentFragment();
// or
var fragment = document.createDocumentFragment();
```

içine bir şeyler ekleyelim:

```js
const myDiv = document.createElement('div');
fragment.appendChild(myDiv);
```

fragment'ımızı DOM'a append edelim:

```js
parentElement.appendChild(fragment);
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Web component
HTML ortamında 3 farklı şekilde "web component" yapılabilir (Mozilla bu şekilde kategorilendirmiş: (source-id: 455) title: "Concepts and usage"):

- custom elements
- shadow DOM
- HTML template
  - template
  - slot

3ü birbirinden bağımsız özelliklerdir.

# custom elements
tarayıcılar tarafından yeni desteklenmeye başlanan özellik. custom HTML'e elementleri yaratmamıza yarıyor. kendi event'leri ve içinde birçok HTML elementi barındırabiliyor.

```html
<div class="my element">
```

gibi var olan alternatif çözümler ile neredeyse aynı kapıya çıkıyor. fakat okunabilirlik açısından çok daha ileri seviyede olmaktadır.

yazdığımız bir HTML tag'ini register etmezsek (tarayıcının desteklemesi gerekir) DOM üzerinde aktif olamaz fakat yine de tarayıcı hata vermediği için çalışmaya devam edebilir. fakat custom-component'ini destekleyen bir tarayıcı ise, yeni tip HTML objemizi tarayıcıya register ederiz ve gerçek bir HTML objesiymiş gibi kullanabilir (best practice'e uygun).

Custom component'imizi yaratalım:

```js
// below extends the "p" elemen. we can also extends others.
// If we don't want to extend any element, we can use the most generic class which is: "HTMLElement".
class MyCustomClass extends HTMLParagraphElement {
  constructor() {
    // this is calling when a new instance is creating.

    // Always call super first in constructor. this is required (only for constructor).
    super();

    this.setAttribute('prop1', 'val1');
    this.innerHTML = "<b>Hello world</b>";

  }

  connectedCallback(){
    // this is called when we attach custom-element to DOM.
  }

  // any custom method
  isDisabled() {
    return this.hasAttribute('disabled');
  }

  disconnectedCallback(){
    // this is calling when removed from DOM.
    // use this when the component use data from indexedDB, cookies...
  }

  attributeChangedCallback(attrName, oldVal, newVal){
    // this is calling when prop changes.
    // this function is only calling props on "observedAttributes".
  }

  static get observedAttributes() {
    return ['prop1', 'prop2'];
  }

  adoptedCallback(){
    // The custom element has been moved into a new document (e.g. someone called document.adoptNode(el)).
  }

}
```

hemen ardından global olarak tarayıcıya tanıtalım:

```js
// customElements is under window object. class type: "CustomElementRegistry".
customElements.define('my-custom-component-name', MyCustomClass, { extends: 'p' });
```

artık istediğimiz yerde kullanabiliriz:

```html
<MyCustomClass></MyCustomClass>
```

# Template
custom-component'ten tamamen bağımsız bir özelliktir.

```html
<template id="my-paragraph">
  <p>My paragraph</p>
</template>
```

Template'ler güncel tarayıcılarca desteklenen bir elementtir. Template içerisinde koyduğumuz herşeyin kolayca başka yere klonlayabilmemizi (re-use) sağlanır:

```js
let template = document.getElementById('my-paragraph');
let templateContent = template.content;
document.body.appendChild(templateContent);
```

# shadow Dom
Shadow DOM kavramı Web browser'ların sunduğu bir teknolojidir. React veya virtual DOM ile hiçbir bağlantısı yoktur. Shadow DOM query ile erişilemeyen DOM objeleri yaratabilmemizi sağlıyor. böylece sayfadaki CSS'lerden ve JS event'leri bu Objelere etki edemiyor. Örneğin Facebook'un "share" button'u. tüm dünyadaki web sayfalarında standart CSS ve event'lerle çalışması bekleniyor ise; bu "share" objelerini shadow DOM ile yapmalıyız.

shadow DOM'lar web tarayıcı inspector'ünde görülebilir fakat JS ile erişilemezler.

```js
<span class="shadow-host">
</span>

// select element by class ID
const shadowEl = document.querySelector(".shadow-host");

// make this element shadow
// attachShadow fonksiyonu "shadowEl" içindeki tüm DOM objelerini shadow'a olarak set ediyor.
// bizim örnekte "shadowEl" içinde hiçbir şey yok. bu sebeple hiçbir DOM objesi shadow içine alınmayacak fakat "shadowEl" ve içi artık shadow DOM'a aittir.
const shadow = shadowEl.attachShadow({mode: 'open'});

// create a new element
const link = document.createElement("a");
link.href = "Facebook.com/share"

// add the new element inside the shadow element
shadow.appendChild(link);
```
