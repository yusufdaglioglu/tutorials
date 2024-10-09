############################

############################
# SOLID
############################

############################

# SOLID prensipleri
object orianted için çıkmış 5 tane prensibi birarada bulunduran bir terimdir (baş harflerini birleştirilmiştir): SRP, OCP, LSP, ISP, DIP.

## Single Responsibility Principle (or SRP or Tek Sorumluluk Prensibi)
Bir sınıfın yahut metotun tek bir sorumluluğu olmalıdır. eğer metotun/sınıfın değişmesi gerekiyorsa; muhtemelen ona yeni sorumluluklar mı eklenecek şüphesi yaratmalıdır.

## Open/closed principle (or OCP)
code/software should be open for extension, but closed for modification. yani; yeni eklemeler yapıldığında programda minimum değişiklik yeterli olabilmelidir. örneğin; online ödemeler. yeni bir ödeme geldiğinde yeni ödemeyi bir sınıftan implemente edip bir listeye eklememiz yeterli olabilmelidir.

Farklı bir değişle; yeni geliştirme geldiğinde; var olan kodumuzu az düzenlemeliyiz fakat yeni sınıflar yaratmalıyız. Bunu ne kadar iyi yapabiliyorsak, kodumuz OCP'ye o kadar yakındır.

2 gerçek örnek:
- Observer pattern. Yeni observer class'ları yarattığımızda, subject'i değiştirmeye gerek kalmıyordu.
- JDBC-driver. Yeni driver eklediğimizde, sistemde daha önceden kullanılan driver'ları hiç değiştirme gereği duymuyoruz.

## Liskov'un Yerine Geçme Prensibi (or Liskov Substitution Principle or LSP)
Alt sınıflardan oluşan nesnelerin, üst sınıfın nesneleri ile yer değiştirdikleri zaman, aynı davranışı sergilemesi gerekmektedir.

implementasyonlarımızda not-implemented hatası fırlatan metotumuz varsa, bu kuralı ihlal ediyoruz anlamına gelir.

Örneğin Ördek sınıfımız olsun. Bu sınıftan oyuncak örnek ve gerçek ördeği implemente edelim. gerçek ördeğin fly metotu varken, oyuncak ördeğin fly metotu not-implemented fırlatacaktır. Bu durumda alt sınıfı üst sınıf ile yer değiştirirse kodumuz hata verecektir. Bunu istemiyoruz. Bunun için farklı çözümlere gidilebilir. Örneğin; UçanÖrdek (sadece fly metotunu implemente eder) ve YüzenÖrdek arayüzlerimizi yazarız. Her implementasyon birkaç hangi özellikleri destekliyorsa sadece onları implemente eder.

## Interface Segregation Principle (or ISP or Arayüz Ayrımı Prensibi)
Direk örnek üzerinden gidelim: Mesaj arayüzümüz olsun. bunu implemente eden bir MesajImpl sınıfımız olsun. MesajImpl kendi içinde attachmentVideo, attachmentPhoto gibi özellikler içerecek. fakat her mesajda video olmayabilir. sadece foto olabilir. bu sebeple burada önerilen; birden fazla MesajImpl olsun ve her biri video için yada photo için spesifik geliştirilsin. (bir mesajda hem foto hem video olamayacağını varsayarak örnek verilmiştir). bu şekilde her implementasyon sadece tek bir işe odaklı yani; arayüzün metotlarını implemente etmeye odaklı olmalıdır. böylece kullanılmayan metotlar çıkarılmış olacaktır.

## ISP vs LSP
Bu 2'si aynı kapıya çıkmaktadır. ISP arayüzlerin, gruplanıp tek bir arayüz haline gitirilmemesini anlatırken, LSP arayüz, yerine subclass'lardan birini çağırınca aynı davranışların sergilenmesini beklemelizi der. Bu iki prensipten birini tamamen düzgün şekilde yaptığımızda, diğer prensibi de düzgün uygulamış oluruz.

## Bağımlılığın Ters Çevrilmesi Prensibi (or Dependency Inversion Principle or DIP)
iki kuralı vardır:

1. High-level modules should not depend on low-level modules. Both should depend on other high levels.

   Burada; 'High level' rolündeki sınıf, alt seviyeli sınıfı çağıran sınıftır. üst seviyeli sınıf, alt seviyeli sınıfın metotlarını çağırarak işlem yapar. Örneğin; shopping servisi, ödeme servisini çağırarak işlem yapar.

2. Abstractions should not depend on details. Details should depend on abstractions.

Örnek üzerinden gidelim;

```java
public abstract Shopping {

    private MySqlDatabase mySqlDatabase = new MySqlDatabase();

    public pay(){

        // payment operations

        // log the result to DB
        mySqlDatabase.insert("payment finished");
    }
}
```

Yukarıda kodda üst seviyeli sınıf Shopping oluyor. Alt seviyeli sınıf ise MySqlDatabase oluyor. Bu örnekte Shopping, MySqlDatabase'e bağımlı. DIP, MySqlDatabase yerine IDatabase (interface) kullanmamızı öneriyor.

