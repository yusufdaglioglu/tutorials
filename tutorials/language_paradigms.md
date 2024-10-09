############################

############################
# LANGUAGE PARADIGMS
############################

############################

# Programming paradigm

paradigm Türkçesi: paradigma

paradigma kelime anlamı: 1-örnek, 2-bir şeyin nasıl üretileceği konusunda örnek/model

programlama dilinin özellikleridir: örneğin cephe yönelimli olması, nesneye yönelimli olması birer paradigmadır.

# multi-paradigm programming language

bazı programlama dillerinin bazı özellikleri birden fazla paradigmayı desteklemektedir.

# programlama dilleri jenerasyonları

- 1inci jenerasyon (1GL): 1 ve 0'lardan oluşan programlama dilleridir. 2inci dünya savaşında Nazi almanyasının şifreli mesajlaşma makinesi olan Enigma'yı çözebilmek için, Alen Turing, Turing makinesi ile 1940'larda ilk programlama mantığı ortaya atılmıştır. Fakat programlama dilleri olmadığı için manyetik teypler panel aracılığı ile kapatılıp açılıyordu (switch/toogle). bu şekilde sadece 1 ve 0'lar ile işlemler yapılıyordu. Bu sebeple bu jenerasyonun dillerinde kodu çevirmeye ihtiyaç yoktur. direk kod makinede işletilebilir durumdadır.

- 2inci jenerasyon (2GL): düşük seviyeli programlama dilleri. assembly. İlk 1950'lerde ortaya çıkmıştır. Bu jenerasyonda temel yenilik, 1 ve 0'ların insanlar tarafından okunmasının aşırı gereksiz ve zor olmasıdır.

- 3inci jenerasyon (3GL): yüksek seviyeli programlama dilleri. C, C++, C#, Java. Bu jenerasyonda temel yenilikler:
  - 2GL'nin daha çok cihazlara bağlı syntax'lar içermesinden kurtulmak
  - daha human-readable bir syntax yapmak
  - byte üzerinden yönetilebilen yeni bütünleşik veri tipleri oluşturabilmek. örnekler:
    - array
    - Class/obje
    - OS'un register'larının desteklemediği büyüklükte sayılar

- 4inci jenerasyon (4GL): diğer jenerasyondakiler gibi sadece bit, byte veya bunların üzerine kurulmuş temel veri tiplerini yönetmekle değil, direk olarak logical anlamda "veriler" kullanabilmektedirler. örneğin tablo üzerinde direk işlem sağlanabilmekte, ve ekrana GUI raporu alınabilmektedir. Örnekler: SPSS, Unix shell, SQL. Bazı araştırmacılar, bu jenerasyonun sadece DSL olduğunu düşünmektedir. Araştırmacılar bu jenerasyonun domain'e özel geliştirilen bir dil olduğunu düşünmektedir. örneğin; web development, database manipulating/reporting, mathematical optimization, GUI development...

- 5inci jenerasyon (5GL): programcının algoritma geliştirerek çözüm geliştirmesinin ötesinde, koşulları ve kısıtları bilgisayara verdiğinizde, bilgisayarın çözümü kendisinin bulmasına yönelik olarak tasarlanmaktadır. Bazı araştırmacılar, bu tarz dillerin jenerasyon grubu altında olamayacağını belirtmekte.

- akademik kaynaklar 4 ve 5inci jenerasyonları çok net bir çizgi ile ayırmış durumda değil. 5inci jenerasyonun, bir jenerasyon olmaması gerektiğini düşünenlerde var.

- Bir dil, birkaç jenerasyona birden uyabilir.

- jenerasyon yükseldikçe, daha kolay şekilde donanımdan daha bağımsız uygulamalar yazılması sağlanır. "high level programming language" terimi üst seviyeli diller için kullanılır.

- en üst seviyede dahi donanımdan bağımsız veya donanıma bağımlı uygulama yazılabilir. çağırdığımız API (kütüphaneler) ve/veya dilin binary/bit bazında işlem destekleyip desteklememesi gibi özellikler sayesinde donanıma bağımlı uygulamalar en üst seviyede de yazılabilir.

# bazı diller
popüler yüksek seviyeli diller (çıkış tarihleri ile - yıllar draft veya stable olabilir):
- Fortran (1954)
- Lips (1956)
- COBOL (1959)
- PASCAL (1970)
- Prolog, SQL, C (1972)
- Ada (1980)
- C++, Objective-C, ABAP (1983)
- MATLAB (1984)
- Perl (1987)
- Visual Basic, Python (1991)
- Lua (1993)
- R (1993)
- Java (1995)
- PHP 1995
- Ruby (1995)
- JavaScript (1995)
- VBScript (1996)
- ECMAScript (1997)
- ActionScript (1998)
- C# (2000)
- Visual Basic .NET (2001) (Bu dil "Visual Basic"'dan farklı. successor olarak yayımlandı.)
- Scala (2003)
- Groovy (2004)
- Go (2009)
- CoffeeScript (2009)
- Rust (2010)
- Kotlin (2011)
- TypeScript (2012)
- Swift (2014)

# go
__golang__ olarakta adlandırılmaktadır.

native derlenen bir dil. Sadece çok basit özellikler içeriyor. bu durum ona performans kazandırıyor.

concurrent uygulamaların az yer harcayacak şekilde çalışması için mimarisi uygun tasarlanmış. OS thread'i yerine, "goroutine" denilen kendi içinde sanal thread management'ı var. OS thread'i gibi çok bellek harcamıyor, çünkü temelde daha basit özellikler içeriyor. programcının hiçbir şeyden haberi olmuyor.

# Yordamsal programlama dili (or Procedural programming language or prosedürel programlama dili)

kod tekrarı engellemek amaçlı (goto/jump gibi terimler yazmaya gerek kalmadan) programın yordamlara bölünmesi ile yazılmayı destekleyen programlama dilleridir.

yordamsal programlama'da, programın dallandığı yerlere "subroutine (or subprogram or callable unit)" adı verilir. dolayısı ile kullandığımız dilin diğer paradigmalarına göre "prosedure" şunlardan biri olacaktır: "function", "method".

fonksiyonel programlamadan temel farkı; fonksiyonel programlamada daha çok "pure" fonksiyonlar ile çalışırken, prosedürel'de genelde pure fonksiyonlar yoktur.

# object orianted (or nesneye yönelimli) vs object based (or nesne tabanlı)

Javascript nesneye yönelimli değildir. nesne kullanır bu sebeple nesne tabanlıdır. fakat nesneye yönelimli dillerin desteklemesi gerektiği özellikleri destekleyecek şekilde özenle geliştirilmez. örneğin Javascript'te inheritance ve polimorfism yoktur. son çıkan Javascript sürümlerinde sınıflar gelmiştir. fakat bu seferde encapsulation yoktur.

bu iki kavram piyasada sürekli birbiri ile karıştırılmaktadır.

object orianted'lar ikiye ayrılır:

- # class based
  sınıfları kullanarak implementasyon yapan dillerdir. (Java buna bir örnektir)

- # prototype based
  bazı kaynaklarda bu şekilde de isimlendirilir: prototypal, prototype-oriented, classless, instance-based programming

  obje klonlayarak implementasyon yapan dillerdir. her obje türetilen objenin prototype'ını referans olarak tutar. bu yapının en güzel özelliği runtime sırasında prototype'a metot veya field eklenebilir. ve hatta prototype referansını tutan instance'lara da bu değişiklikler yansıtılabilir. (Javascript buna bir örnektir)

# 4 major principles of object orianted programming

Aşağıdaki 4 madde sadece en temel prensiplerdir. Birbirlerine çok yakın prensiplerdir.

"interface" veya "abstract" sınıflar ile OOP'nin aşağıdaki maddelerini gerçekleyebilmemizi sağlar.

not: her dilde; Java'daki gibi hem "interface" hemde "abstract" sınıf kavramları birden olmayabilir.

- # encapsulation (or kapsülleme)

  2 temel kavramdan oluşur:

  - Information hiding

    modifier'lar (public, private...) gibi keyword'ler aracılığı ile bilgiyi erişime kısıtlayabilmemizdir.

  - Packaging

    paket(ler) veya sınıf(lar) içerisinde tüm bilgilerin/metotların, bir unit olarak dışarıya sunulmasıdır..

- # Abstraction (or soyutlama)

  sınıf içerisindeki detayların (property'lerin/metotların), dışarıdan sınıfı çağıranlar tarafından bilinmesine gerek kalmamasıdır.

- # Inheritance (or kalıtım)

  sınıfların bir yada birden fazla yerden türeyebilme özelliğidir.

- # Polymorphism (or polimorfizm or çok biçimlilik)

  türeyen sınıflar, süper sınıfın metotlarını isterse override edebilmesidir.

# fonksiyonel programlama (or functional programming)

fonksiyonel programlama genelde;

- rekürsif işlemlerin yapıldığı,

- pure fonksiyonlar ile çalışıldığı

- fonksiyonlar fonksiyonlara parametre olarak geçilebildiği

dillerdir.

Burada "multi-paradigm programming language" başlığında belirtildiği gibi her dilin birden fazla özelliği desteklediğini hatırlatmak gerekli. Aynı zamanda yordamsal, fonksiyonel, nesneye yönelimli terimleri programlama dilinin bir özelliği değildir. aslında bir yazım tarzıdır. daha farklı bir bakış açısıyla;

- fonksiyonel dil: devide and concur mantığı ile problemlerin çözüldüğü diller

- prosedürel dil: sıralı şekilde birbiri ardına işlem yapılarak bir işin gerçekleştirildiği diller

- nesneye yönelimli dil: gerçek hayattaki nesne yapıları oluşturularak bunlar üzerinde birbirinin aksiyonları çağrılarak yazılan uygulamalardır

tabiki de eğer bir dilde nesne yapısı hiç yoksa nesneye yönelimli tarzda uygulama yazmak mümkün olmayacağı için bu terimler sanki dilin bir özelliğiymiş gibi anlatılıyor. oysa değil.

# Olaya dayalı programlama (or olay güdümlü programlama or event driven programming)

GUI, donanım sinyalleri gibi olaylar sonrası aksiyon alan programlama dilleridir. aslında tüm programlama dilleri kütüphaneleri desteklediği sürece event bazlı olabilir. fakat genelde ağırlıklı olarak event'lere dayalı programlar için bu terim kullanılır. örneğin Javascript. sadece son kullanıcı sayfada bir yere tıkladığında yada sayfa yüklediğinde bir aksiyon alınır. onun dışındaki aksiyonlar nadirdir. zaman tetiklemeleri mevcuttur. belirli zamanda event tetiklenir. timeout metotları gibi...

örneğin komut satırı ile işlem yapan script'ler event bazlı değildir.

# declarative programming

decleration Türkçe kelime anlamı: beyan etmek, bildirmek

dekleratif diller işin nasıl yapıldığına karışmaz. sadece ne yapılması istendiğini belirtir. örnek: SQL, CSS, HTML, Java'daki annotation'lar...

# imperative programming

imperative Türkçe kelime anlamı: zorunlu, mecbur

declarative'in tersidir. nasıl olacağına tam olarak karışır ve sonuçta ne istediği dilin kendisini ilgilendirmez.
