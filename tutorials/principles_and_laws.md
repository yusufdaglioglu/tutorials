############################

############################
# PRINCIPLES AND LAWS
############################

############################

Note: Most of principles can be found in other documents. The principles and laws in this document could not categorized to any spesific topic. Therefore they are written here.

# Fallacies of Distributed Computing (or Dağıtık Sistemlerin Yanılgıları)
__Fallacies of Networked Computing (or Ağ Tabanlı Sistemlerin Yanılgıları)__ olarakta bilinir.

dağıtık sistemleri geliştirme sırasında başarısızlıklara yol açabilecek varsayımların (veya inançların) bir listesidir. Varsayımlar:

- The network is reliable
- Latency is zero
- Bandwidth is infinite
- The network is secure
- Topology doesn't change
- There is one administrator
- Transport cost is zero
- The network is homogeneous

Bu yanılgıların tümü tek bir kişi tarafından ortaya atılmadı. Yıllarca herkes birbirinin makalesine referans ederek, listeye yeni yanılgılar ekledi.

# Broken windows theory (or Kırık Camlar teorisi)
Ufka problemlerin (örneğin ortamsal problemlr, basit kod kalitesizlikleri...) zaman içinde çok büyük problemlere yol açtığını savunan teoridir.

# Convention over configuration (or coding by convention or CoC)

örnek; JPA'da aksini belirtmedikçe @Entity ve @ID annotation'larına sahip tüm POJO class'lar, attribute name'lerin ve class name'lerin birebir tablolarla ve column'larla eşleştiği kabulü üzerine dayanır ki buna convention denir. yani özel olarak ek konfigürasyon belirtmediğimizce yapılan code'lamalar daha temiz ve daha sadece görünmektedir. fakat bunun dezavantajı şudur: kodu okuyan başka birisinin kullandığımız framework/kütüphanelere hakim olması gerekir. çünkü configler hep default'tur.

# Diamond Problem

```
  A

B  C

  D
```

Yukarıdaki elmas görünümlü şekil extend ağacı olsun. A super class. B ve C, A'dan türemiş. D ise hem B hem C'den türemiş.

B ve C, A'nın X metotunu override etmiş olsun. D, A'nın X metotunu çalıştırmak isterse ne olacak? İşte bu probleme için birçok dil farklı çözümler getirmiş durumdadır.

# composition over inheritance

EFT ve Havale yapmak için sunucuya gönderdiğimiz iki ayrı nesnemiz olsun. bu nesnelerde tutar, karşı tarafın banka numarası, gibi ortak parametreleri var. Genelde yazılımcılar MoneyTransferCommonData objesi oluşturur ve içine amount, receiverNo gibi ortak değerleri atar. Daha sonra EFT ve Havale nesnelerini bundan türetir. Oysa "composition over inheritance" practice'si, bunun yerine  MoneyTransferCommonData objesini EFT ve Havale objelerinin içinde property olarak tutmamızı önerir. Bunun avantajları şudur:

- bazı diller birçok extend'e izin vermez. extend hakkımızı kullanmamış oluruz.

- ilgili sınıfımızı ileride bir çok interface/abstract/sınıftan türetebiliriz. bu türetmeler sonucu (özellikle aynı isimdeki metotlar) debug işlemlerimiz çok zorlaşacaktır. oysa; myProperty.myMethod() çok daha okunaklı. sadece myMethod() bize mi ait super class'tan mı geliyor diye sürekli bakmak gerekmektedir.

yani; "is a" ilişkisi yerine "has a" ilişkisi tercih edilmelidir.

# Halting problem (or Sonlanma problemi)

bir programı başlatırken birçok parametre ile başlatabiliriz. her parametreye karşılık bu programın sonsuza dek döngüye girip girmeyeceğini yada sonlanacağının garantisini veremeyiz. daha doğrusu bunu programatik olarak kanıtlayacak/ispatlayacak programı yazamayız.

örneğin IDE ile kodumuzu formatlarken sonsuz döngüye girme ihtimalimiz var. Böyle bir durumda IDE nasıl davranacak? O kodun formatlanırken IDE'nin sonsuz döngüye girip girmeyeceğini bilemiyoruz. Bunu bilemediğimizden IDE'ler belli bir süre sonra format işlemini durdurmaktadır. Çünkü henüz bir programın sonsuz döngüye girip girmeyeceğini belirleyen bir program yazılamamıştır.

Alan turing böyle bir programın yazılamayacağını kanıtlamıştır. Kanıt "proof by contradiction (çelişki ile kanıt)" yöntemini kullanmaktadır.

A programımıza "i" input'unu verseydik; A programımız sonlanırmıydı?

```
isHalting( A, i );
  - returns;
    - true(durur)
    - false(durmaz)
```

isHalting'in koduna eklemeler yapmak amaçlı metotu wrap edelim:
- true(durur) dönerse; wrapper metotumuz sürekli çalışmaya devam etsin (kendini sonsuz döngüye soksun).
- false(durmaz) dönerse; wrapper metotumuz true(durur) dönsün ve kapansın.

Bu wrapper metotumuza isHalting+ adını verelim.

Şimdi şu işlemi yapalım:

isHalting+( isHalting+, (A,i) );

Bu metot bize ne dönecek?
- isHalting+ eğer true(durur) dönerse; isHalting(isHalting+, (A,i) ) false dönmüş demektir. Oysa isHalting+ hiç false dönemiyordu. False durumunda kendini (isteyerek - bu kodu biz yukarıda kendimiz ekledik) sonsuz döngüye sokuyordu. Böyle bir durum söz konusu olamaz (çelişki durumu). Yani makinemiz bize yanlış bilgi vermiş oluyor.

- isHalting+ false(durmaz) dönmek yerine kendini songuz döngüye sokuyordu (bizim yazdığımız kod). eğer kendini sonsuz döngüye sokarsa; isHalting(isHalting+, (A,i) ) true dönmüş demektir. Burası olağan gibi geliyor. Fakat isHalting(isHalting+, (A,i) ) true dönmüş olması için isHalting+'ın çalıştırıldığında sonlanmış olması; bir return değerini döndürmüş olması gerekli. isHalting+ sadece isHalting false durumunda true dönüş yapabiliyordu. diğer durumda sonsuz döngüye giriyordu. isHalting false ise; isHalting(isHalting+, (A,i) ) true dönmüş olamaz. Burada da bir çelişki söz konusudur.

# Dilbert Prensibi (or Dilbert Principle)
Dilbert prensibine göre yetenekli olmayan çalışanlar yönetim kadorlarına doğru yükseltilirler ki üretime verecekleri zarar aza indirilsin.

# Pareto Prensibi (80/20 Kuralı or Pareto Principle or 80/20 Rule or Law of the Vital Few and Principle of Factor Sparsity)
Pareto Prensibi der ki; çıktıların önemli bir çoğunluğu girdilerin çok azı tarafından oluşturulur:

- Bir yazılımın 80%'i harcanan zamanın %20'sinde yazılır (bir başka deyişle, kodun en zor %20'lik bölümü harcanan zamanı- %80'inde yazılır)
- Harcanan eforun %20'si sonucun %80'ini oluşturur
- Yapılan işin %20'si gelirin %80'ini oluşturur
- Koddaki hataların %20'si sistem sorunlarının %80'ini oluşturur
- Özelliklerin %20'si hizmetin %80'ini oluşturur

Gerçek dünyadan örnek: 2002'de Microsoft en çok rapor edilen hataların üstten %20'sini çözünce kullanıcıların yaşadığı sorunların %80'inin çözüldüğünü gözlemlemiş.

# Shirky Prensibi (or Shirky Principle)
Kurumlar, çözümü oldukları sorunun devam etmesini sağlamaya çalışırlar. Bu durum kasıtlı veya kasıtsız olabilir.

# Tüm Modeller Yanlıştır (or George Box Yasası or All Models Are Wrong or George Box's Law)
Tüm modeller yanlıştır, ancak bazıları kullanışlıdır.

# Project Management Triangle (or Triple Constraint or Iron Triangle or Project Triangle)

scope(kapsam), cost(maliyet), schedule(zaman) sabitlerinin köşeleri olduğu üçgendir. örneğin; maliyeti düşürürseniz diğerlerini arttırmak zorundasınız. 3 ü bir arada hiçbir zaman olamayacağını savunan teoridir.

# Ölü Deniz Etkisi (or Dead Sea Effect)
En yetenekli ve verimli BT mühendisleri şirketleri terketmeye en yakın olanlardır, kalıcı olma taraftarı olanlar ise tortuya (daha az yetenekli ve verimsiz) benzetilebilir.

# Hofstadter yasası
Bir olay her zaman planlanan zamandan daha geç biter. Hofstadter yasasını hesaba katsan bile.

# Hutber Yasası (or Hutber's Law)
İyileştirme, bozulma anlamına da gelir. Örneğin bir servis hızlı cevap vermesi için yapacağımız bir geliştirme, arkada verilerimizin denormalize olmasına yol açabilir ve bunun başka yerlerde bozulmalara yol açacağı bellidir.

# Hyrum Yasası (or Arabirimlerin Örtülü Hukuku or Hyrum's Law or The Law of Implicit Interfaces)
Kullanıcı sayısı arttığında, API'ler, API sözleşmesinde ne olduğuna göre değil, kullanıcıların ihityaçlarına göre şekillenecektir.

# Conway Yasası (or Conway's Law)
Melvin Edward Conway tarafından atılmış bir gözlemdir:

"Sistemleri tasarlayan organizasyonlar, kendi iletişim yapılarının birer kopyasını üretmekle sınırlıdır".

Bu söz ile; yazılım geliştirme sürecinde, birimler arası iletişim sorunlarının yazılımın çıktısına da yansıdığını savunur.

Yazılan orijinal makaleden önemli bir cümle: "Any organization that designs a system (defined broadly) will produce a design whose structure is a copy of the organization's communication structure.".

# Hollywood Principle
Hollywood'daki yapımcılar/yönetmenler önemli insanlardır (VIP - Very important person). onları aradığınızda ve bir teklif sunduğunuzda size böyle dönerler: "Don't call us, we'll call you".

Bu prensibe dayanan birçok yazılım framework'ü mevcut. örnek: Spring'in DI'ı bu prensibe dayalı çalışır: Bir Java Bean'i bir dependency'yi autowired ettiğinde, dependency'yi aramaz. Spring o sınıfa dependency'yi getirir. "Don't call around for your dependencies, we'll give them to you when we need you."

# Cunningham Yasası
İnternette doğru cevabı almanın en iyi yolu bir soru sormak değil, yanlış cevabı kullanarak aramaktır.

# Dunning-Kruger Etkisi (or The Dunning-Kruger Effect)
"Eğer bir konuda yetersizseniz, yetersiz olduğunuzu bilemezsiniz... Doğru bir cevap için ihtiyacınız olan yetenekler, doğru cevabın ne olduğunu anlamanız için ihtiyacınız olan yeteneklerdir."

Dunning-Kruger etkisi, David Dunning ve Justin Kruger tarafından 1999'da yapılan bir psikolojik çalışma ve makalede açıklanan teorik bir bilişsel önyargıdır. Çalışma, bir alanda düşük beceri seviyesine sahip kişilerin, o alandaki yeteneklerini muhtemelen abarttığını öne sürüyor. Bu önyargının önerilen nedeni, bir kişinin o alanda çalışabilme kapasitesi hakkında bilinçli bir fikir verebilmesi için o alanın karmaşıklığının yeterince "farkında" olması gerekliliğidir.

Dunning-Kruger etkisi bazen, "Bir kişi bir alanı ne kadar az anlarsa, o alandaki sorunları kolayca çözebileceklerine o kadar çok inanır ve o alanı basit olarak görme olasılıkları daha yüksektir". Bu genel kabul edilebilecek etki, teknolojide de oldukça önemlidir. Teknik olmayan ekip üyeleri veya daha az deneyimli ekip üyeleri gibi bir alana daha az aşina olan kişilerin, bu alandaki bir sorunu çözmek için gereken çabayı küçümseme olasılığının daha yüksek olduğunu gösterir.

Kişi bir alandaki bilgisi ve deneyimi arttıkça, başka bir etkiyle de karşılaşabilir.

# Brooks's law
bir projedeki kişi sayısı arttığında, projenin kısalma süresi gelen kişi sayısı ile doğru orantılı değildir. bu kişiler birbirleri ile iletişimde kaybedilen zaman artmaktadır. aynı zamanda birbirlerine olan aktarım süreleri de artmaktadır vs...

# law of demeter (or LoD or principle of least knowledge or Demeter Yasası)
Each unit should only talk to its friends; don't talk to strangers.

coupling'i azaltmak için temel bir prensiptir. her sınıf sadece kendi içindeki variable'ların metotlarını çağırmalıdır. Onunda içindek bir nesneyi çekip çağırmamalıdır.

```java
public class A{

   private B b;

   public void function(C c){
      h();             // Good!
      b.g();           // Good!
      c.u();           // Good!
      D d = new D();
      d.v();           // Good!
      E e = c.w();
      e.z();          // Breaks the Demeter's Law!
      // you should ask for "c" to do your job.
      // Prefer this one:
      // c.doSomethingWithZ();
   }
}
```

# Unix Felsefesi (or Unix philosophy)

Unix işletim sisteminin lider geliştiricilerinin deneyimine ve yaklaşımlarına dayalı yazılım geliştirme metotolojisidir. bu yöntemler genelde tüm POSIX'lerde tercih ediliyor.

genelde aşağıdaki metotolojilere dayanır:

- Write programs that do one thing and do it well.

- Write programs to work together.

- Write programs to handle text streams, because that is a universal interface.

# KISS (or Keep it simple, Stupid)

herşeyi basit olmasına dayanan felsefedir.

# Iterative and Incremental development

sürekli olarak ufak geliştirmeler çıkarak yapılan geliştirme yöntemidir. bu şekilde riskler düşürülür, feature/branch merge'ler kolaylaşır.

# Gall Yasası (or Gall's Law)
Çalışan karmaşık bir sistemin her zaman işe yarayan daha basit bir sistemden evrimleştiği kesinlikle söylenebilir. Başlangıçtan itibaren karmaşık tasarlanmış bir sistemin asla çalışmayacağı ve sonradan da düzeltilemeyeceği kesindir. Çalışan daha basit bir sistem ile başlamanız gerekir.

Buna verilebilecek en iyi örnek internettir. Internet ilk ortaya çıktığında çok basit bir yapıdaydı. Şu anda çok karmaşık ve çok fazla kullanılıyor.

# Goodhart's Law (or Goodhart Yasası)
"Gözlemlenen herhangi bir istatistiksel düzenlilik, kontrol amaçları için üzerine baskı uygulandığında çökme eğiliminde olacaktır."

"Bir ölçüm hedef haline geldiğinde, iyi bir ölçüm olmaktan çıkar."

Ölçümlere dayalı optimizasyonlar bir süre sonra istenilen amacın dışına dışılmasına sebep olabiliyor. Örneğin; yazılımcıların performansını yazdıkları kod satırı sayısına göre ölçmek sıkıntıya yol açabiliyor.

# Hick Yasası (Hick-Hyman Yasası or Hick's Law or Hick-Hyman Law)
Karar verme süresi, seçebileceğiniz seçeneklerin sayısı ile logaritmik orantılı olarak büyür.

# Abstraction principle
yazılımı abstract sınıflar kullanarak tekrarlanan kodları azaltma metotolojisidir.

Abstraction; karışık bir implementasyonu, kullanıcıdan veya programdan gizlemek için yeni bir katmanın geliştirilmesidir. Bunun sonucunda anlaşılması ve kullanılması daha kolay bir arayüzün ortaya çıkması beklenir.

# Big Design Up Front (or BDUF)

yazılımın mimarisi oturtulmadan sayfaların/business logic'in geliştirmeye başlanmadığı yazılım geliştirme sürecidir.

# top-down design (or top-down approach) vs bottom-up design (or bottom-up approach)
bottom-up design'da bir case için problem çözülür, daha sonra 2 case için ve daha sonra tüm case'leri kapsayacak durumlar çözülür. oysa top-down'da bunun tam tersi yapılır.

# design by contract

direk örnek üzerinden gidersek;

```java
int böl(int a, int b)
{
  contract.requires(b != 0);
  return a / b;
}
```

static kod analizinde derleyici bu kodu yakalayacaktır. işte bu tarz kodlara anlaşmalı kodlar deniliyor.

# Defensive design

kodun kendini koruduğu, böylece alacağı parametrelerle uygulamanın akışının engellenmediği yazılımlara denir. örneğin; ekleyeceğiniz plugin'ler hiçbir şekilde uygulamanızı durdurmamalı. gerekirse plugin kill edilmeli ve uygulama devam etmeli. başka bir örnek: getter'larda döndürülen listeler sadece read only olmalıdır. fakat Java'da referans döndüğünüz için biri bizim listemizi myObj.getList().add("new element"); fonksiyonunu çağırarak ekleme çıkarma yapabilir. self defensive olup getter'lardan objenin klonunu yada read only halini döndürebiliriz.

# service contract

servislerimizi inject edip başka yerden kullandığımızda inject ettiğimiz şey, bir interface olması görüşüdür. hangi implementasyonun run-time'de kullanılacağına framework/mimarimiz karar vermelidir. bunun avantajları:

- servislerimiz arayüzlere depend eder implementasyonlara değil. bu kural farklı pattern'ler'den gelmektedir.

- bazen servislerimizde protected yada public metotlarımız olabilir. bu metotları her zaman dışarıya değil, diğer internal sınıflarımıza açma gereği duyabiliriz. bu sebeple API'miz arayüzlerden okunmalıdır. böylece herkes bilir ki API sadece arayüzdedir.

Sadece 1 implementasyonumuz olacaksa bile bunu yapmalıyız. çünkü:

- ileride 2inci implementasyon gelirse, buna hazır olmuş oluruz.

- API'miz interface olursa dışarıdan okuyan kişi daha kolay ve net olarak hangi metotları çağırması gerektiğini okuyabilir.

Yukarıda bir API'den bahsedildi. API uygulamamızın dışarıya açtığı metotları temsil eder. fakat internal sınıflarımızda API kullanıyoruz demek pek doğru olmaz. Bu sebeple "contract (or anlaşma)" denilmektedir. tabi buradan türeyen diğer terimlerde kullanılabilir: "interface contract", "service contract", "service interface"... Servislerimizin aldığı parametrelere "Message Contact", "data contract" da denilebilmektedir.

# YAGNI (or you aren't gonna need it)

ihtiyaç olmayan özelliklerin projeye eklenmemesi prensibidir.

# Linus Yasası (or Linus's Law)
Ne kadar çok farklı göz bakarsa, hataların bulunması o kadar kolaylaşır.

# Kernighan Yasası (or Kernighan's Law)
Kodda hata ayıklama yapmak, o kodun sıfırdan yazılmasından iki kat daha zordur.

# Moore Yasası (or Moore's Law)
Entegre devre içerisindeki transistörlerin sayısı yaklaşık olarak iki yılda bir ikiye katlanır. 1970'lerden 2000'lere kadar çok ideal şekilde bu tahmin tutmuştur.

# Murphy Yasası (or Murphy's Law)
__Sod Yasası (or Sod's Law)__ ile aşırı benzerdir.

Eğer bir işin kötü gitme ihtimali varsa mutlaka kötü gider.

# Parkinson Yasası (or Parkinson's Law)
Bir iş, daima, bitirilmesi için kendisine ayrılan sürenin hepsini kapsayacak şekilde uzar. Şöyle ki; ekipler genelde proje bitiş tarihi yaklaşana kadar düşük verimde çalışırlar, bitiş tarihi yaklaştıkça bitirmek için yoğun bir çaba içine girerler ve sonuç olarak aslında bitiş tarihini tutturmuş olurlar.

# Olgunlaşmamış Optimizasyon Etkisi (or Premature Optimization Effect)
Vakti gelmeden yapılan optimizasyon bütün kötülüklerin anasıdır.

# Putt Yasası (or Putt's Law)
Teknolojide iki tür insan egemendir, yönetmedikleri şeyleri anlayanlar ve anlamadıkları şeyleri yönetenler.

# Sızdıran Soyutlamalar Yasası (or The Law of Leaky Abstractions)
Çok fazla soyutlama sorunlara yol açabilmektedir.

Örneğin dosya okuma yazma işlemleri disk tipinden soyutlanmış durumda. Fakat network'teki dosyalarda diskteymiş gibi soyutlama yapılabiliyor. Bu durumun birçok dezavantajı var.

# Önemsizlik Yasası (or The Law of Triviality)
Bu yasa diyor ki, ekipler önemsiz ve kozmetik sorunlara ciddi ve önemli sorunlara göre daha fazla zaman harcarlar.

# The Two Pizza Rule (or İki Pizza Kuralı)
Eğer bir ekibi iki pizza ile doyuramıyorsanız, o ekip büyük bir ekiptir.

İnsanlar arasındaki bağlantıların sayısını n(n-1)/2 şeklinde tanımlayabiliriz. Ekipte 10 insan varsa, 10*(9)/2=45 bağlantı vardır. (Yani herkesin herkesle kombinasyonu)
