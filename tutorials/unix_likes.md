############################

############################
# UNIX-LIKES
############################

############################

# Unix-like'ta herşey dosya

UNIX tabanlı sistemlerde tüm kaynaklar dosyalama mantığı ile çalışmaktadır. bu dosyalar UNIX-like'larda "özel dosyalar" olarak adlandırılırlar. komut satırından bir dizindeki dosyaları listelediğimizde, bir dosyanın özel olup olmadığını; yetki satırındaki ilk harfinden anlayabiliriz. birden fazla dosya tipi vardır:

- regular file : dosya sistemindeki normal özelliksiz bir dosya

- dizin : dosya sistemindeki klasör/dizin

- Symbolic link (or symlink or soft link) : Microsoft Windows'taki kısayol

- Named pipe : Inter-process iletişimi için gerekli boru hatları dosyaları

- Socket : Inter-process iletişimi soketleri. network soketi ile karıştırılmamalıdır. bazı kaynaklarda "UNIX domain socket"'i diye de geçmektedir.

- device : driver dosyası

- Door : sadece Solaris işletim sisteminde kullanılan IPC için gerekli bir dosya

# dosya vs API

UNIX tabanlı olmayan sistemlerde (örnek Microsoft Windows) her kaynağa işletim sistemin sunduğu API'ler aracılığı ile erişilebilir. Örneğin; bir stream'e veri atarken Microsoft Windows'ta MyAPIClass.writeValue("new value"); diye yazılırken, Linux'ta herhangi bir outputstream ile ilgili dosyaya byte veri yollamak yeterlidir.

Stream olmasının en temel yararı stream kütüphaneleri her dilde varolduğundan her dilden API ihtiyacı olmadan işlemleri gerçekleştirebilmemizi sağlar.

örneğin; Linux'ta komut satırında "command > output.txt" komutu command'ı çalıştırır ve komut satırı çıktısını output.txt'ye aktarır. UNIX sistemlerde 3 özel dosya vardır. şu işlevi görür :

- "firefox > /dev/null" çalıştırırsan çıktılar hiçbir yere aktarılmaz. her yazım işlemi sonrası EOF döndürülür.

- /dev/random sürekli okumak için tasarlanmıştır. sürekli random sayı döndürür.

- /dev/zero sürekli okumak için tasarlanmıştır. ASCII'deki 0x00 (null) değerini döndürür.

# maximum açık dosya limiti

UNIX-like'larde açık dosya sayısının bir limiti vardır.

bu limit root yetkisi var ise arttırılabilir. sadece özel olarak bir process için dosya sayısı limitlenebilir, aynı şekilde sadece user yada tüm OS bazında limitler belirlenebilir.

böyle bir limitin olmasının sebebi UNIX-like'lerde herşeyin dosya olmasıdır: soket, process herşey bir dosya. dolayısı ile bir program kendisi çok bellekten harcarsa (örneğin durmadan integer array'ler oluşturursa) memory doldu hatası alacaktır. yani bu işletim sistemini etkilemez. fakat program durmadan soket açarsa, process açarsa, OS bunlara karşılık birer dosya oluşturur. bu durumda dosya sistemi bu durumdan etkilenecektir. dosya sistemi memory'de olmadığı için ve fiziksel olarak bilgiler diskte kaydolacağı için bu durum bilgisayarı çökertebilir. bunu engellemek amaçlı dosya sayısı limitlenmektedir.

# fork-bomb
sürekli olarak process açıp sistemi sıkıntıya sokan programlardır. sürekli dosya açılması sıkıntı yaratır.

# file descriptor (or dosya betimleyicisi)
her dosya kullanımında process içerisinde bu dosyanın adresi (programlama dilindeki obje/nesne) tutulur. bu adres file descriptor'tur. C dilinde bu bir tamsayıdır. Dosyalar sanal olarak /prod/pid/fd (/prod/process-id/file-descriptor) gibi dosyalarda tutulur. örneğin; /prod/1001/48 dosyası bir domain soketine yada bir sürücüye denk geliyor olabilir. bu dosyalar gerçekten de o dizinde yaratılırlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# freedesktop.org (or X Desktop Group or XDG)
X window system üzerindeki uygulamalara katkıda bulunan ticari olmayan topluluğun ismidir. Gnome, KDE gibi projelerin büyük destekçilerindendir.

# XDG Base Directory Specification
Birçok versiyonu vardır. Kaynak: https://specifications.freedesktop.org/ title:"basedir-spec".

Bu standartlara uyulmak zorunda değildir, fakat standart olduğu için birçok yazlım bu dizinleri tercih eder.

Bazı standart XDG değişkenleri:

- __$XDG_CACHE_HOME__

  cache directory.

- __$XDG_CONFIG_HOME__
 
  sadece config dosyalarıdır. başka makineye taşınabilir olmalıdır.

- __$XDG_DATA_HOME__

  Yazılımcı camiasında "static dosya" olarak tabir edilen dosyalar burada olabilir. örnek durumlar:

  - Thumbnail.
  - oyun uygulamasında son kullanıcının indirdiği custom ses dosyaları.
  - chat uygulamasında son kullanıcının indirdiği custom emoji dosyaları.
  - eklentiler.

- __$XDG_STATE_HOME__

  XDG'nin eski versiyonlarda yoktu.

  Uygulamanın anlık durumunu tutar. Örneğin:
  - file sync işlemi yapıyorduk, pause edip uygulamamızı kapattık. Sync statümüz burada kaydolmalıdır.
  - text-editor ise; en son açılan dosya listesi.
  - media-plaer ise; en son çalınan müzikler listesi.
  - web browser ise; cookie'ler, history.
  - log dosyaları.

- __$XDG_DATA_DIRS__

  OS üzerinde, __$XDG_DATA_HOME__ dan başka bir dizinde de DATA bilgisi tutuyor olabiliriz. Eğer böyle bir durum var ise, bu variable içerisinde bu dizinlerin listesini tutmalıyız.

- __$XDG_CONFIG_DIRS__

  __$XDG_DATA_DIRS__ ile aynı mantıkta bilgi tutmaktadır. Tek farkı, DATA yerine CONFIG bilgisine referans etmesidir.

Default değerler şu şekildedir:

| Type    | Purpose    | XDG Environment Variable       | Linux & BSD                                         | MacOS                                            | Windows                                              |
|---------|------------|--------------------------------|-----------------------------------------------------|--------------------------------------------------|------------------------------------------------------|
| Base    | home       | HOME                           | $HOME                                               | $HOME                                            | {FOLDERID_UserProfile}                               |
| Base    | cache      | XDG_CACHE_HOME                 | $XDG_CACHE_HOME or $HOME/.cache                     | $HOME/Library/Caches                             | {FOLDERID_LocalApplicationData}                      |
| Base    | config     | XDG_CONFIG_HOME                | $XDG_CONFIG_HOME or $HOME/.config                   | $HOME/Library/Application Support                | {FOLDERID_ApplicationData}                           |
| Base    | data       | XDG_DATA_HOME                  | $XDG_DATA_HOME or $HOME/.local/share                | $HOME/Library/Application Support                | {FOLDERID_ApplicationData}                           |
| Base    | dataLocal  | XDG_DATA_HOME                  | $XDG_DATA_HOME or $HOME/.local/share                | $HOME/Library/Application Support                | {FOLDERID_LocalApplicationData}                      |
| Base    | executable | XDG_BIN_HOME                   | $XDG_BIN_HOME or $HOME/.local/bin                   | null                                             | null                                                 |
| Base    | preference | XDG_CONFIG_HOME                | $XDG_CONFIG_HOME or $HOME/.config                   | $HOME/Library/Preferences                        | {FOLDERID_ApplicationData}                           |
| Base    | runtime    | XDG_RUNTIME_DIR                | $XDG_RUNTIME_DIR or null                            | null                                             | null                                                 |
| User    | audio      | XDG_MUSIC_DIR                  | $XDG_MUSIC_DIR                                      | $HOME/Music                                      | {FOLDERID_Music}                                     |
| User    | desktop    | XDG_DESKTOP_DIR                | $XDG_DESKTOP_DIR                                    | $HOME/Desktop                                    | {FOLDERID_Desktop}                                   |
| User    | document   | XDG_DOCUMENTS_DIR              | $XDG_DOCUMENTS_DIR                                  | $HOME/Documents                                  | {FOLDERID_Documents}                                 |
| User    | download   | XDG_DOWNLOAD_DIR               | $XDG_DOWNLOAD_DIR                                   | $HOME/Downloads                                  | {FOLDERID_Downloads}                                 |
| User    | font       | XDG_DATA_HOME                  | $XDG_DATA_HOME/fonts or $HOME/.local/share/fonts    | $HOME/Library/Fonts                              | null                                                 |
| User    | picture    | XDG_PICTURES_DIR               | $XDG_PICTURES_DIR                                   | $HOME/Pictures                                   | {FOLDERID_Pictures}                                  |
| User    | public     | XDG_PUBLICSHARE_DIR            | $XDG_PUBLICSHARE_DIR                                | $HOME/Public                                     | {FOLDERID_Public}                                    |
| User    | template   | XDG_TEMPLATES_DIR              | $XDG_TEMPLATES_DIR                                  | null                                             | {FOLDERID_Templates}                                 |
| User    | video      | XDG_VIDEOS_DIR                 | $XDG_VIDEOS_DIR                                     | $HOME/Movies                                     | {FOLDERID_Videos}                                    |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Window System

İşletim sisteminde GUI (Input Output - klavye, fare vs dahil) pencerelerin yönetimini sağlayan sistemdir. Tüm sistemi temsil eder. Hangi pencere açık, statüleri ve meta bilgileri burada tutulur. (Aşağıda anlatılan hem sunucu hem client tarafının tümünü temsil eder)

# X Window System (or X)
Piyasada en çok 11'inci sürüm kullanıldığından X11 olarakta isimlendirilmektedir.

Bir Window System'dır.

# Display Server (or Window Server)

Display Server bir sunucudur. Display server pencere meta bilgilerini tutar. Bunları, window manager ile birlikte window system'ın client tarafına sunar. client tarafı birden fazla olabilir. Zira uzaktan aynı makineye bağlanan ve kullanan birileri olabilir. Burada pencerelerin GUI'lerinin dekorasyonu ve işlevlerini window manager sunmaktadır. Sunucu ve client aralarında Telnet kullanarak haberleşirler.

X client her açtığımız uygulamadır. Bu uygulamalar yetkileri dahilinde diğer uygulamaları görebilirler (ekran görüntüleri alabilir gibi).

Firejail'in x11 sandboxing özelliği Firejail'in çalıştırıldığı bilgisayarda yeni bir x-server açıyor. bu x-server bu iki bağımsız yazılımdan biri olabilir: Xpra and Xephyr. Run edilmesi istenen GUI uygulamasının client'ı bu sunucuya bağlanıyor.

# X.Org Server

X Window System için sunucu tarafıdır (başka alternatiflerde piyasada mevcuttur). Bazı kaynaklarda kısaltılarak "X server" olarak kullanılıyor. Fakat "X Server" iki anlama da gelebilir: "X.Org Server" kısaltması, yada "X Window System" için sunucu tarafı. "X Window System" için birden fazla sunucu tarafı implementasyonu (yazılımı) olduğu için karışıklığa sebep olabilir.

# MacOS display server
__Quartz Compositor__ is the display server and compositing window manager.

X11'den çatallanan __XQuartz__ yazılımı, X11 uyumluluğunu sağlamak için kullanılan yazılımdır.

# $HOME/.Xauthority dosyası

Bu dosya home'a sahip olan user'ın "x.org server"'a eriştiğinde kullandığı oturumun Authentication token'ını saklar. eğer bu dosyaya yazma yetkisi yok ise o user görüntüyü alamaz. bu sebeple sudo ile açılan komutlarda bu sorun yaşanabiliyordu. yine bu sebeple "gksudo" komutu geliştirildi.

# Xresources
varsayılan olarak ~/.Xresources veya ~/.Xdefaults dizinindeki tutulan property'lerdir. bu property'ler her program için spesifik yada genel olarak tüm pencerelerin yapısı (rengi, text'lerin fontu...) hakkında bilgiler tutar.

# Mir vs Wayland

X çok eski bir altyapıya sahiptir. Bu sebeple X'e alternatif bu 2 açık kaynaklı projeler geliştirilmiştir.

X'te "compositor", opsiyonel bir yetenekti. Oysa Wayland'da zorunlu bir özelliktir. Wayland'da compositor'ün ayrı modül olması mantığı yoktur. display server ve compositor tek bir modül haline getirildi.

# Window Manager
Window System üzerinde pencerelerin yönetimi (işlevlerini belirleyen: minimize, maximize, always on top, dekorasyonu gibi) sağlayan yazılımdır.

Windows manager'ların kurulum paketlerinde gömülü ek uygulamalar ile geldiklerinde neredeyse bir masaüstü yöneticisi(Gnome) gibi hizmet verebilmektedirler.

tiplerine göre 4'e ayrılırlar.

- Compositing Window Manager (or compositor)

  - pencereler birbirleri üzerine binebilirler (bu durum overlapping olarak adlandırılır)
  - ek olarak transparan yapabilme gibi birçok özelliğe sahiptirler çünkü her pencere objesini manipüle edebilirler.

  örnekler:

  Compton, Sway, Compiz, KWin, Metacity, Mutter, Xfwm, Enlightenment, Xcompmgr, Gala, Muffin

- Stacking (or floating)

  - pencereler birbirleri üzerine binebilirler
  - pencereleri manipüle etme gibi bir durumları yoktur.

  örnekler:

  2bwm, Aewm, AfterStep, Amiwm, Blackbox, Cwm, Fluxbox, FVWM, IceWM, JWM, Matchbox, Mwm, Openbox, Sawfish, Twm, Window Maker, Eggwm, Evilwm, Flwm

- Tiling

  - pencereler birbirleri üzerine binemezler.

  örnekler:

  i3, Ion, ratpoison, wmii, StumpWM, Larswm, Qtile, Herbstluftwm, Bspwm, EXWM, Howm, Notion, Ratpoison, Subtle, Sway, Way-cooler, WMFS, WMFS2

- dynamic tilling (or dynamic)

  Tiling ve Stacking arasında geçiş yapabilen modları mevcuttur.

  örnekler:

  - DWM: 2000 satırı geçmeyen sadece C ile yazılmış kod olacak şekilde tasarlanmaktadır. sadece temel görevleri yerine getirmek için tasarlanmıştır.

  - Awesome: DWM türevidir. 2000 satırlık kod limiti bu fork'da aşılmıştır. aynı zamanda Lua kullanır.

  - Spectrwm, Xmonad, Catwm, Echinus, FrankenWM, Qtile, Wmii

# Client-side decoration (or CSD)
Pencere yönetim sistemi, normalde X server'ın görevidir. Fakat CSD ile, client'lar da artık kendi pencere yapılarını oluşturabiliyor.

bu sistemin tersine __Server-Side Decoration (or SSD)__ denir.

# Masaüstü yöneticisi (or desktop manager)
En üst seviyeli GUI ortamıdır. Masaüstlerinde sunulacak özellikleri (widget, notification system, simge panelleri, ayarlar sunan yazılımlar, hazır gelen yazılımlar gibi) belirlerler.

Bazı 'Window Manager'lar, 'desktop manager' olmadan kullanılabilir. Fakat sadece "Window system" tek başına ("window manager" olmadan) son kullanıcı için bir işe yaramaz.

# desktop manager'lar:

- Budgie: Gnome 2'inci sürüme benzer, sıfırdan yazılmış

- KDE Plasma: en gelişmiş DM olma yolundadır

- Gnome: genel olarak KDE'ye göre daha hafiftir

- Xfce: genel olarak Gnome'a göre daha hafiftir

- LXDE: Xfce'ye alternatiftir

- MATE: Gnome'un 2'inci sürümünden devam eden bir akımdır.

- Cinnamon: Gnome'un 3'üncü sürümünden devam eden bir akımdır. Gnome shell'e alternatiftir. Gnome için kabuktur.

- LXQt: LXDE'nin qt tabanlı türevidir.

- Budgie

- Sugar: Çocuklar için tasarlanan arayüzde çok kısıtlı seçenek sunan masaüstüdür

- Moksha: Enlightment projesinden türemiş, masaüstü yönetici olmaya odaklanmış bir projedir.

- Lumina: hafif olmayı amaçlar

- Pantheon: Elementary OS için yaratılan proje

- Manokwari: Gnome üzerine kurulu shell (Unity gibi)

- Deepin DE (or DDE)

# KDE Plasma vs KDE

KDE, projenin genel ismidir. KDE; "Plasma masaüstü yöneticisi", "KDE uygulamaları"" ve "KDE kütüphaneleri (frameworks)"" üçlüsünden oluşur. Sonuçta asıl amaç; masaüstü ortamı sunmak olduğu için; KDE; "K Desktop Environment" kelimelerinin kısaltmasından oluşmaktadır.

# KDE Neon

Ubuntu LTS bazlı KDE yüklü gelen, KDE takımı tarafından geliştirilen OS. KUbuntu LTS varken neden böyle bir OS geliştiriliyor? Çünkü Ubuntu LTS versiyonlarında stabilite gereği hiçbir uygulama (masaüstü yöneticisi de dahil) her zaman güncellenmez. Güncellense de genelde sadece güvenlik paketleri güncellenir. KDE takımı ise sadece kendi repolarını update edecek şekilde ayarlamıştır Neon içerisinde.

# GNOME Shell vs Unity

Gnome 3 üzerine inşa edilmiş farklı masaüstü yöneticileridir. Unity, Ubuntu tarafından geliştirilmekte, Gnome Shell ise, Gnome takımı tarafından geliştirilmektedir. Bu yüzden Gnome-shell, Gnome projesinin varsayılan (or referans) masaüstü olmaktadır. Gnome Shell'in eklenti altyapısı mevcuttur.

Gnome, 3'üncü sürümüne kadar tek başına bir masaüstü yöneticisi olarak paketlenip sunulmaktaydı. 3'üncü sürümü ile artık her topluluk/şirket bunun üzerine ek bir kabuk yazarak dağıtmak durumunda kalıyorlar (Gnome Shell, Unity, Cinnamnamon gibi).

Bazı işletim sistemleri, Gnome 3'ü hala sade (eski haline benzer) olarak gösterebilmektedirler. bunun birkaç yöntemi var:

1- GNOME Flashback (or GNOME Fallback): Gnome 3 ile eski görüntüyü sağlamak için bazı modüller devre dışı bırakılıyor ve shell'dekinden farklı bir window manager kullanılıyor. + eski panel kullanılıyor.

2- GNOME Classic (or GNOME Classic (no effects)):  Gnome 3.8 sürümü ile resmi olarak; birden fazla eklenti ile eklentiler ile eski görünümü Gnome shell üzerine verebiliyor.

Gnome 2'den 3e geçiş sürecinde oturum açma ekranlarında işletim sistemleri kendilerince isimlendirme kullandıkları için karışıklığa sebep olmaktadırlar.

# Ubuntu Gnome

Ubuntu'nun türevi. Masaüstü yöneticisi olarak sadece "Gnome Shell" yüklü gelmektedir.

Ubuntu 18.04 ile artık varsayılan masaüstü Gnome Shell'dir. Ekstra birkaç eklenti yüklü gelmektedir (örnek kısayol panel eklentisi). Bu sebeple Ubuntu Gnome projesine ihtiyaç kalmamış ve proje sonlanmıştır.

# Display manager (or login manager)

işletim sistemi login ekranında çıkan yazılımdır. masaüstü yöneticisin yada sadece pencere yöneticisini başlatmak için görevlidir. komut satırı yada GUI tabanlı olabilirler.

- Console

  - CDM
  - Console TDM
  - Ly
  - Nodm

- Graphic

  - Entrance - An EFL based display manager
  - GDM - The GNOME display manager.
  - LightDM - A cross-desktop display manager, can use various front-ends written in any toolkit.
  - LXDM - The LXDE display manager. Can be used independent of the LXDE desktop environment.
  - MDM - used in Linux Mint, a fork of GDM 2.
  - SDDM - The QML-based display manager and successor to KDE4's kdm; recommended for Plasma 5 and LXQt.
  - SLiM - Lightweight and elegant graphical login solution. (discontinued)
  - XDM - The X display manager with support for XDMCP, and host chooser.

# X display manager

Sadece X session'u başlatan yazılımdır (display manager'dır).

# GTK (or old-name:GIMP Toolkit or old-name:GTK+) vs Qt vs Java Swing vs Java AWT

Widget toolkit'lerdir. Pencerelerdeki button'ların ve diğer nesnelerin nasıl olacağına ve event'lere karar veren kütüphanelerdir. Window manager ekrana bir pencere açtığında bu kütüphanelerden yararlanır. Doğal olarak; default olarak her window manager'ın yanında bir widget toolkit olmalıdır. Bazı uygulamalar kendi içerisinde widget toolkitleri gömerler (Java ile yazılmış bir uygulamanın Swing'i kendi içerisinde bulundurması gibi, GIMP'in GTK'yı bulundurması gibi). Böyle durumlarda Window manager o uygulamayı, uygulamanın kendi widget toolkit'i ile açmak zorunda kalır. böyle durumlarda ekranda bazen sıkıntılar yola açabilir. çünkü her pencerede, bir çeşit toolkit aynı anda çalışıyor olur. bir yazılım, esnek tasarlanmış ise birden fazla toolkit ile başlatılabilir.

# Qt
açık kaynaklı bu proje, C++ ile yazılmış kütüphaneler grubudur. socket işlemleri, veritabanı işlemleri ve hatta QT widget'ları ile uygulama yazılmasını sağlar. Aynı zamanda bir IDE (pencere tasarlama modülü içeren) sunmaktadır.

qt API'leri cross platformdur. pazarı yüksek olan tüm os'ları destekler.

# Remote desktop using X

X sunucuları uzaktan bağlanılmak için gerekli altyapıya sahiptir. X sunucusuna birden fazla client aynı anda istek yapabilir. AYnı kullanıcı oturumu içerisinde dahi birden fazla client istek yapıp cevap alabilir. X sunucularından gelen bu istekleri uzak makinada X client karşılamak zorundadır. X client, Microsoft Windows'ta olmadığı için uzak makine orada yapılamaz. Fakat Apple cihazlar için Apple'ın kütüphane esnekliği ile open source X11 client'ı mevcuttur. Apple makinede x client kurulu ise uzak masaüstü yapılabilir. X org sadece masaüstünü komple değil, uygulama penceresi bazında da uzağa yansıtma yapabilir. Uzak masaüstü kendi masaüstünde uygulama açarmış gibi uygulamayı kullanabilir.

SSH üzerinden uzak masaüstü rahatlıkla yapılabilir. Komut satırından SSH ile username ile login olduktan sonra, komut satırından örneğin Firefox yazılır ise, Firefox SSH yapılan makinede açılacaktır.

X11'in bu özelliğini kullanmayan diğer uzak bağlantı uygulamaları (örneğin Teamviewer), sadece ekranın yada bir bölümünün ekran görüntüsünü çekerek uzak makinaya aktarırlar. Oysa X11'deki olay direk olarak native bir çalışma sergilemektedir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# apparmor vs seLinux
birbirlerine alternatif __Linux kernel security module (or LSM)__'leridir. dosyalara erişimi kısıtlarlar.

"Subject vs Principal vs User" başlığında da anlatıldığı gibi; her security context'inde:

- obje; erişilecek kaynaktır. LSM için: dosya ve dizin.
- subject; objeye erişmek isteyen process'tir. LSM için: OS üzerinde çalışan yazılımlar.

# discretionary access control (or DAC) vs mandatory access control (or MAC)
discretionary kelime anlamı: isteğe bağlı

mandatory kelime anlamı: zorunlu

apparmor ve seLinux yüklü olmayan UNIX-like sistemlerde, standart UNIX-like dosya koruması vardır. bu koruma DAC grubuna girer. çünkü DAC dosya sahibinin erişimi isteğe bağlı kısıtlayabildiği kontrol mekanizmalarına denir.

MAC ise; sistem yöneticisi tarafından verilen kurallar ile kısıtlama getirilen güvenlik mekanizmalarına denir.

Bu durumda apparmor vs seLinux MAC grubundadır. Fakat ek olarak DAC yetenekleri de sunarlar.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# DNS ve host ayarları

/etc/resolv.conf dosyasında DNS tanımları vardır:

nameserver 8.8.8.8

nameserver 8.8.4.4

Bu dosya birçok yazılım tarafından aynı anda düzenlemekte ve okunmaktadır. okuyanlar arasında "resolver" uygulaması da vardır.

Herkesin paralel okuması veya buraya yazılması durumuna çözüm getirmek için "resolvconf" yazılımı geliştirildi. diğer tüm uygulamalar (VPN client, network manager...) her DNS değişiminde "resolvconf" uygulamasına bilgi göndermesi beklenir. Fakat "resolvconf" yazılımı sonradan geliştirildi. hala onu tanımayan başka yazılımlar var ve bu sebeple "resolvconf" her zaman tam olarak başarıyı sağlayamıyor.

Son kullanıcı resolv.conf dosyasını düzenlememeli. "/etc/network/interfaces" dosyasını düzenlemelidir.

örnek:

```
dns-nameservers 12.34.56.78 12.34.56.79
```

network manager (root olarak çalışan yazılım ve önyüz uygulamaları (önyüz uygulamalarına örnek: nmcli, nmtui) aşağıdaki DNS ayarlarını kendince override eder:
- dhcp ayarlarındaki DNS'leri
- resolv.conf'taki DNS'leri
- /etc/dhcp/dhclient.conf'teki DNS'leri

NetworkManager GUI'den son kullanıcının belirlediği ayarları buraya kaydeder:

> /etc/NetworkManager/system-connections/name-of-connection

örnek:

> "/etc/NetworkManager/system-connections/Wired connection 1".

dosya içeriği örneği:

```
[802-3-ethernet]
duplex=full
mac-address=XX:XX:XX:XX:XX:XX

[connection]
id=Wired connection 1
uuid=xxx-xxxxxx-xxxxxx-xxxxxx-xxx
type=802-3-ethernet
timestamp=1385213042

[ipv6]
method=auto

[ipv4]
method=auto
dns=208.67.222.222;
ignore-auto-dns=true
```

Artık tüm network manager GUI'leri doğru bilgiyi gösterecektir.

# /etc/hosts

default örnek dosya içeriği:

```
cat /etc/hosts
127.0.0.1 localhost
127.0.1.1 machine-id

# The following lines are desirable for IPv6 capable hosts
::1     ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouter
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# PulseAudio vs Alsa (or Advanced Linux Sound Architecture)
PulseAudio, user seviyesine kurulu olan bir uygulamadır. Bu uygulama daha alt seviyeli olan Alsa'ya sesleri teslim eder.

Alsa normalde tek bir voice-input alabilir. Yani bir den fazla uygulamadan aynı anda ses veremez. Fakat PulseAudio, aynı anda açık olan uygulamalardan sesleri alır ve bunları tek bir input olarak Alsa'ya iletmemizi sağlar.

PulseAudio birçok ses kaynağından bilgi alabildiği için "sound server" denir.

PulseAudio'nun alternatifi olarak "__dmix__" kullanılabilir.

# Open Sound System (or OSS)
Lisansı tamamen özgür olmadığı için pek tercih edilmiyor. Fakat hem PulseAudio hemde Alsa ile yapılacakları sadece OSS tek başına yapabiliyor.

# Ses miksajı (or Audio mixing)
çok kanallı kayıtları nihai bir bir ses ürününd birleştirme işlemidir.

# MIDI (or Musical Instrument Digital Interface)
elektronik müzik aletleri ve bilgisayarlar arasında gerçek zamanlı veri alışverişini sağlayan, endüstri standardı haline gelmiş yaygın bir iletişim protokolü.

MIDI protokolünde ses verisi değil, temel bazı değişkenlere ilişkin sayısal bilgiler aktarılır; nota bilgileri, enstrüman atamaları, tempo değeri gibi bilgiler bunlardan bazılarıdır. 

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Ubuntu-restricted-extras

bu paket Ubuntu repo'larında duruyor fakat lisans sebepleri gereği ile default yüklü getirilemiyor. içerisinde şunları barındırıyor:

- Adobe flash player

- mp3 codec ve diğer codec'ler

- Microsoft TrueType core fontları

- Icedtea plugin

- unrar

# repositories
Ubuntu'da 4 ana repository var:

| repository name | open source | supported by canonical |
|-----------------|-------------|------------------------|
| main            | yes         | yes                    |
| universe        | yes         | no                     |
| Restricted      | no          | yes                    |
| Multiverse      | no/yes      | no                     |

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# root of Linux distros

- Debian (deb based)

  - SteamOS (Gnome masaüstü de yüklü geliyor. bu şekilde normal Linux uygulamaları da çalıştırılıyor.)

  - Raspbian (Raspberry üzerinde çalışması için tasarlanmış. lxde yüklü.)

  - MxLinux (sadece desktop sürümü olan. içerisinde son kullanıcı birçok için paket yüklü geliyor. bu sebeple çok kolay kullanımı var. xfce masaüstünü kullandığı içinde hafif.)

  - Kali (or old-name:BackTrack) (güvenlik testleri için geliştirilmiş tool'lar içeriyor)

  - Tails (or The Amnesic Incognito Live System) (tüm bağlantılar TOR üzerinden yapılır ve gizliliği amaçlar)

  - Pardus (Tübitak tarafından geliştiriliyor. eskiden pisi paket yöneticisi kullanırdı fakat daha sonradan deb'e geçti.)

  - KNOPPIX (temel amacı live cd ile çalışmak. çok hafif.)

  - Ubuntu

    - Mint

    - Elementary (sadece masaüstü sürümü mevcut. son kullanıcı için kolaylatırılmış ayarlar barındırıyor.)

    - Deepin

    - Zorin

    - Ubuntu Studio (Ubuntu'nun multimedia uygulamaları yüklü gelen türevi)

- Fedora (rpm based)

  - Mandrake

  - Red Hat Enterprise Linux (or RHEL) (ticari)

    - CentOS (or Community Enterprise Operating System) (community yönetiyor. RHEL'den hiçbir farkı yok. RHEL gibi bir ticari OS'u temel aldığı için üstünde paketler free olunca herkes tarafından özellikle tercih sebebi oluyor.)

    2020 civarındaki duyuruya göre artık "CentOS Stream" isimli dağıtım sunulmaktadır. centOS artık geliştirmesi durdurulmuştur. "CentOS Stream" RHEL'ın upstream projesi olmuştur.

    - Oracle Linux (ticari)

    - Scientific Linux (akademik çalışmalarda kullanılması için tasarlandı. 2019'da geliştirmesi durduruldu ve CentOS'a geçip ona destek verecekleri açıklandı.)

- Arch (Pacman paket yöneticisi). sistem kurulumunda sadece ihtiyaç olan paketleri seçtirebilmesi konusunda çok çok esnek.

  - Manjaro

- Gentoo (package manager: Portage) arch gibi sistemin paketlerine kadar kullanıcının kendisi kuruyor. arch'a göre daha fazla teknik bilgi gerektiriyor.

  - Sabayon

  - Chrome OS (Chromium OS)

- OpenSuse (rpm based)

  - SUSE. enterprise.

- PCLinuxOS (end user friendly. sadece masaüstü versiyonu mevcut. RPM based.)

- Mandriva (or old-name:Mandrake) (RPM based)

  - Mageia

  - OpenMandriva

- Puppy (live CD ile çalışabilmek için çok hafif tasarlanmıştır)

- Slackware

- NixOS (Nix package manager as default)

- Alpine (package manager: APK (or Alpine Package Keeper) )

- Solus (paket yöneticisi: eopkg (pisi türevidir)) (sadece son kullanıcı için masaüstü sürümü mevcuttur.)

distrowatch.com'dan bir OS'un detay sayfasına bakıldığında, temel yazılımlar için (browser, office...) yüklü olan güncel paket sürümleri gibi tüm detaylar dahi bulunmaktadır.

distrotest.net sitesi gerçek makinaya tarayıcı üzerinden VNC ile kullanma imkanı sağlıyor.

# ReactOS
Microsoft Windows dosyalarını yürütebiliyor ve ona benziyor. Linux tabanlı bir OS değildir. sıfırdan yazılmış bir OS çekirdeği kullanılıyor.

# Ubuntu netboot
Ubuntu türevi değildir. Ubuntu'nun standart ISO'sundan farklı olarak GUI ve birçok paketi yüklü getirmez. son kullanıcılar arasında "minimal ISO" yada "mini.iso" olarakta adlandırılır. bu ISO Ubuntu repo'larına kurulum sırasında bağlanarak indirme yapar. tabi güncel paket kuracağı için update işlemine gerek kalmaz.

Ubuntu 18.04 ile standart ISO ile kurulum sırasında yeni bir seçenek sunuyor: "minimal installation". bu seçenek yukarıdakinden tamamen bağımsızdır. bu standart masaüstü bileşenlerini ve utility'lerin kurulmasını fakat vlc, libreoffice, oyunlar gibi ekstra paketlerin kurulmamasını sağlıyor. normal installation ile arasındaki fark (kurulmayan tam paket listesi) (source-id: 442)

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# package-managers

# Portage

paket yönetim sistemidir. komut satırı uygulaması: __emerge__'dür.

# dnf (or Dandified YUM) vs yum (or "Yellowdog Updater, Modified")

rpm (or Redhat Package Manager) paket yönetim sisteminin komut satırı uygulamalarıdır. dnf yum'un türevidir. Fedora'nın birçok türevinden kullanılıyor.

# yum group install vs yum install

yum paketleri grup halinde barındırabiliyor. örneğin "graphic user interface" isimli bir grup varsa, bu grubun içinde GUI için gerekli tüm paketler mevcuttur. tek komut ile tümünü kurabiliyoruz.

# zypper

rpm paket yöneticisi. Suse tarafından kullanılıyor. __ZYpp (or libzypp)__ paket yöneticisinin ismi, onu yöneten komut satırı uygulamasının ismi: __zypper__.

# deb paket yönetim sistemi

# apt-get

Debian paket yöneticisi için komut satırı uygulamasıdır. "sudo apt-get install chat"

# apt-cache

Debian paket yöneticisinin source-list'i ile ilgili işlemlerin yapılmasını sağlayan komut satırı uygulaması. örnek: "sudo apt-cache search chat"

# apt

apt son kullanıcı için geliştirilmiş daha basitleştirilmiş hem apt-cache hemde apt-get komutlarını basitleştirip farklı bir arayüzde sunuyor.

# purge vs remove

apt-get'e geçilen iki parametrede uygulamayı silmemizi sağlar. purge ise configuration dosyalarını da siler. burada bahsi geçen config dosyaları /home içindeki dosyalar değil. bahsi geçen config'ler /etc içindeki config'lerdir ( /etc: config files for all users. example /etc/appname/ ).

purge seçeneği "Synaptic package manager"'de "Tamamen kaldır" seçeneğine tekabül ediyor.

# autoremove vs remove

remove yerine autoremove kullanılırsa, silinen ilgili paketin yüklemiş olduğu ve diğer paketler tarafından kullanılmayan dependency'ler(paketler) de sistemden silinir. genelde bu paketler çok ufak olduğunda tekrar ihtiyaç olması ihtimaline karşı pek kimse autoremove kullanmıyor.

# apt-get update

Paket listelerini güncellemek. liste /etc/apt/sources.list (text bazlı dosya) dosyasındadır.

# apt-get upgrade

kurulu paketleri günceller.

# apt-get dist-upgrade

işletim sistemini günceller.

# upgrade vs dist-upgrade

sources.list dosyasına baktığımızda içinde bu tarz URL görebiliriz: "http://archive.Ubuntu.com/Ubuntu xenial-security". dikkat edilirse; xenial (Ubuntu'nun bir sürümünün ismi) yazıyor. işte "upgrade" komutu run edildiğinde xenial için desteklenen en son sürüm vlc, Firefox ve diğer paketler kuruluyor. yani vlc'nin en son sürümü kurulacak diye bir durum söz konusu değildir. fakat "dist-upgrade" sources.list dosyasının bu URL'lerini de günceller. işte işletim sistemi yükseltme farkı budur. işletim sistemi de paketler topluluğudur. bu bakış açısı ile "xenial" kelimesi sources.list'te olmasaydı tüm sistem sürekli yükselirdi.

# apt-get clean

/var/cache/apt/archives/ içinde bulunan .deb uzantılı dosyaları siler. ".deb" dosyaları tekrar kurulma ihtimaline karşı hazır tutulur.

# apt-cache search paket_adı

programı aramak

# apt-cache show paket_adı

program hakkında bilgi almak

# repository

sources.list dosyasındaki her URL bir repository'dir. repository paketlerin dışarıdan indirilmesi için açılan sunucudur.

# ppa (or Personal Package Archive)

paketlerin dışarıdan indirilmesi için açılan Launchpad sunucularıdır. bir ppa sunucusunda birden fazla paket çeşidi indirilebilir. sadece bir paket olmak zorunda değildir.

apt-add-repository ppa:group-name/sub-name ile ppa kurulabilir. eklenen ppa source.list'e eklenmez. ppa farklı bir yerde tutulur: "/etc/apt/sources.list.d" dizininde dosyalar halinde tutulurlar.

# dpkg
deb dosyaları üzerinde direk işlem yapabilmemizi sağlayan komut satırı uygulamasıdır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# kernel (or çekirdek)

"işletim sistemi" ve "işletim sistemi çekirdeği" farklı kavramlardır. işletim sisteminin çekirdeği değişebilir, fakat kendisi aynı isimde farklı sürüm çıkarmaya devam edebilir.

# UNIX
işletim sistemi çekirdeğidir. açık kaynaklıdır.

Çekirdeğinde BSD kullanan 30'dan fazla OS var. Bunlardan bazıları:
- FreeBSD
- NetBSD
- OpenBSD

# Darwin
Apple çok eskiden UNIX işletim sistemi kullanıyordu. Daha sonra lisans sorunları sebebi ile BSD çekirdeğini kullanmaya başladı. BSD'den türettikleri işletim sistemi çekirdeğine Darwin adı verildi.

# BSD (or Berkeley Software Distribution or Berkeley Standard Distribution)
İlk çıktığında ismi "Berkeley Unix"'ti.

İşletim sistemi çekirdeğidir. UNIX'ten çatallanmıştır.

# Linux
Linux, UNIX çekirdeğinden çatallanmamıştır.

# Solaris
BSD türevidir. ilk çıktığında SunOS olarak isimlendiriliyordu. Daha sonra, sadece Solaris adını aldı. Daha sonra ise "Oracle Solaris" ismi ile dağıtıldı.

Şu anda Solaris'ten türemiş birçok OS vardır.

# Unix-like operating system (or UN*X or \*nix)
BSD, Linux, Unix ve türevlerini kapsayan işletim sistemi ailesidir. Resmi bir UNIX Spesifikasyonu var, fakat sertifikayı almamış bir çok Unix-like sistemlerde mevcuttur.

# POSIX (or Portable Operating System Interface for Unix)
__IEEE (or Institute of Electrical and Electronics Engineers)__ tarafından belirlenen, tüm işletim sistemlerinde geçerli olabilecek bir standartlar ailesidir. Resmi olarak sertifika almamış bir çok POSIX destekli (yada kısmen POSIX destekli) işletim sistemleri vardır. Tüm standartları sağlamayan fakat kısmen standartlara uyan işletim sistemleri vardır. Bunların arasında Microsoft Windows, BSD ve Linux vardır. Solaris ve MacOS, tamamiyle POSIX desteklidir.

POSIX standartları temelde şunlara benzer: İşletim sistemi üzerinde çalışan yazılımların birbirleri arasında yollayabildikleri sinyallerin tipleri, işletim sisteminin process ve thread yönetimi, tarih ve zamanlama konularında standartlar, desteklenen encoding'ler vs...

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# .desktop dosyaları
Birçok Linux dağıtımında ".desktop" uzantılı dosyalar mevcuttur. Bunlar normal birer text dosyasıdır. Run edildiklerinde, içindeki bilgilere göre bir komut çalıştırırlar. örnek .desktop dosyası içeriği:

```ini
[Desktop Entry]
Version=1.0
Type=Application
Name=Firefox
Comment=Internet Web Browser
Exec=/usr/bin/firefox.bin
Icon=firefox.jpeg
```

Desktop dosyalarının executable yetkisi varsa; dosya yöneticisi ekranda gösterirken .desktop uzantısını gizler. onun yerine "name" değerini ekranda gösterir. Executable yetkisi olmayan dosyalarda hala ".desktop" uzantısı görülür.

# Link dosyaları
Unix-like'ların tümünde standarttır. Link dosyaları, o dizindeyken orijinal dosyanın tüm özelliklerini döndürürler. Örneğin link'in boyutu 60 MB olabilir. Çünkü Link sadece orijinal dosyanın referansını tutarak, o dizindeymişçesine davranmasını sağlayabilir.

Link dosya sisteminin bir özelliğidir. Oysa desktop file manager'in bir özelliğidir.

# "open with" kısayolları
Nautilus'teki "open with" seçenekleri birer desktop dosyalarıdır. /usr/share/applications, /home/user-name/.local/share/applications gibi birçok dizin içerisinde bulunurlar. Gnome masaüstü initialize olurken bütün bu dizinleri gezer ve bu kısayolları menüde gösterir. kısayolların içindeki "exec" komutuna %U "open with" parametresi ile birlikte açılacak dosyanın adresini gönderir. örneğin Snapcraft uygulamaları portable'dır. fakat sistemde yüklü gibi gözükmelerini sağlamak için bir dizinde bütün Snap uygulamaların desktop dosyaları bulundurur. işletim sistemine kurulmuş olan Snap, bu dizini file manager'ların config dosyalarına ekler. artık Nautilus her başladığında bu dizini de tarayacaktır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Wine (or Wine İs Not Emulator)
Microsoft Windows executable dosyalarının UNIX-like'larda yürütmeye çalışan yazılım.

Wine ile çalıştırılan bir executable, sistem çağrısı yaptığında çalıştırdığı metot, POSIX'te denk gelen en yakın metottur. bu sebeple Wine %100 stabil çalışamamaktadır. çünkü sistem çağrıları metotları, Microsoft Windows'un sistem çağrı metotları ile %100 aynı şekilde çalışamamaktadırlar.

Wine yukarıdaki sebepten bir emülatör değildir. emülatör olsaydı, sistem çağrıları metotları Microsoft Windows'unki ile birebir aynı şekilde implemente edilmeleri gerekecekti.

örneğin Android emülatörleri CPU işlemlerini masaüstü PC'de geçekleştirdiğinde birebir CPU çevrimi yapmaktadır.

Wine; aynı Microsoft Windows 7'nin, "Microsoft Windows Vista uyumluluk modu" mantığı ile çalışmaktadır.

# PlayOnLinux
Wine ile Microsoft Windows programı yüklendikten sonra, yüklenen programın düzgün çalışabilmesi için Microsoft Windows'a denk gelen dizinlere dll atmak gibi birçok ek işlev gerekebiliyor. bu işlevler her program için toplanmış ve çözümleri internette aık kaynak olarak paylaşılmaktadır. PlayOnLinux uygulaması bunları son kullanıcıya otomatik yapıyor.

MacOS ve BSD için ayrı fork'lar üretilmiştir: PlayOnMac and PlayOnBSD.

# CrossOver
PlayOnLinux ile aynı görevi yapan ticari bir yazılımdır.

# dll (or Dynamic-link library)
Microsoft'un shared-library konsepti için implementasyonudur. dll dosyaları Microsoft Windows için portable kütüphanelerdir. bunun içindeki metotlar herhangi bir programlama dili ile Microsoft Windows üzerinde çalıştırılabilir.

# library path

çalıştırdığımız program, kütüphaneleri tararken, taradığı dizinler arasına yeni bir dizin eklemek istersek bu path parametresinden yararlanabiliriz:

```sh
export LD_LIBRARY_PATH="/list/of/library/paths:/another/path"
./program
```

bu bilinen/standart bir path değeri olduğundan her sistem bunu okur.

Java'da özel olarak ikinci bir yöntemde sunulmuştur:

```sh
java -Djava.library.path="/path/to/my/dll" -cp "/my/classpath/goes/here" "MainClass"
```

Linux'ta C/C++'ta iki tip shared object vardır.

- Dynamically linked shared object libraries
  - genelde .so uzantılı dosyalardır. __Shared Object__'in kısaltmasıdır.
  - bu dosyalar runtime sırasında ilgili program tarafından okunur.
- Static libraries
  - genelde .a uzantılı dosyalardır.
  - derleme sırasında ilgili programın içine gömülürler.
  - gcc komutu ile bir derlemeye örnek verelim:

    ```sh
    gcc "src-file.c" -lm -lpthread
    ```

    -l prefix'inden sonra "m" ve "pthread" gelmektedir. Bu 2 dosya static library olarak kabul edilir. derleme sırasında tüm sistemde "lib" prefix'i ile aranırlar. örnek bulunan dosyalar:
    - /usr/lib/libm.a
    - /usr/lib/libpthread.a
  - "a" uzantısı archive teriminin kısaltmasından gelmektedir. a dosyası "ar (archiver'ın kısaltması)" komut satırı uygulaması ile oluşturulur. ".a" dosyası birçok farklı dosyanın birleşimidir. örnek:

    ```sh
    ar rcs "libclass.a" "class1.o" "class2.o" "class3.o"
    ```

    "libclass.a" dosyası birçok .o dosyasının birleşimidir.

    ar komutu genelde sadece derlemelerde kullanılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Filesystem Hierarchy Standard (FHS)
POSIX'ler için dizin standarttır. Aşağıda bazı dizinler verilmiştir. ilgili olanlar gruplandırıldı.

__

/dev device files. everything is file on Linux konusu. example: HDD partitions.

/Proc is a special virtual directory like /dev. Başka başlıkta detaylı anlatılıyor.

__

/media mounted removable devices (CD-ROM, USB)

/mnt temporary mounted volumes (mounted ISO)

/cdrom CD-ROM mount point. it is not Linux standart. because Linux use /media.

/misc automatic mounted device , like archive files (like .ISO), or mounted remote filesystems

__

/lib shared libraries /lib32 /lib64 gibi dizinler olabilir. çünkü aynı isimde executable'lar aynı sistemde olabilir. bu dizinde aynı zamanda kernel-modülleri de vardır.
bazı OS'larda /lib /usr/lib'e linklenmiş durumdadır. bu tarz linklemeler birçok dizin için yapılmaktadır.

/usr apps own files.

/sbin system-binary'den gelmektedir. Utilities used for system administration (and other root-only commands). mostly recovering, booting, repairing things...

/bin command line based executable apps

/usr/bin commands for end user, GUI apps

/usr/sbin sistem açıldıktan sonra kullanılabilecek yönetimsel komutlar  buradadır. boot, recover, repair için olmayan fakat yönetimsel komutlar bu dizindedir.

/usr/local/sbin kullanıcı tarafından yüklenen sistemsel uygulamalar

/opt optional'dan gelmektedir. 3rd addons, or other system optional softwares.

__

/var app data for all users except config files (because config files are stored on /etc). the files inside these directory are grouped like: log files (/var/log), crash dumps (/var/crash), files which are sent to printer (/var/spool),

/etc config files for all users. example /etc/appname/.

/home/username user spesific home folder

/root root user's home folder

__

(others - gruplanamayanlar)

/lost+found hata durumlarında bazı dosyalar dosya sisteminde yok olabiliyorlar. fakat bu dosyalar tekrar kurtarılabilmeleri için eğer yapılabiliyorlarsa bu dizin altında linkleniyorlar.

/srv data to be served by the system for protocols such as, FTP, www

/tmp temporary files for all apps.

/boot boot files (example: Grub)

# MacOS dizin yapısı
MacOS'ta POSIX'lerdeki gibi bir yapı vardır. Bazı istisnalar şunlardır:

/Applications --> uygulamaların ve kendi bilgilerinın tutulduğu dizindir. her uygulama kendi isminde bir klasörde tutulur.

/Users --> Linux'taki /home dizinidir. sadece ismi farklıdır.

/Volumes --> mount edilen tüm bölümler buradadır.

/var/root --> Linux'taki /root dizini.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# file permission
Dosya sistemleri birçok farklı (bağımsız) standartta bilgi bulundurabilir:
- __POSIX.1-2017__ (ACL'ye göre çok daha basit yapıdadır. sadece 1 user, 1 grup ve other olarak permission belirlenebiliyor.)
- __POSIX.1e ACL__ (__access control list (or ACL)__)
- __NFSv4__ (ACL'den daha gelişmiş.)
- others...

Bir file-system aynı anda birden fazla permission scheme'yi destekliyor olabilir.

# POSIX.1-2017
Linux üzerinde dosyalar üzerindeki haklar kullanıcı(file owner), grup ve diğerleri olarak 3 grupta belirlenebilmektedir.

bir dosya için permission tanımlaması yaparken sadece owner, grup ve others olarak yapılabilir. bu sebepten eğer bir dosyaya, 2 farklı grubun hakkı olması isteniyorsa, yeni grup yaratılıp, o gruba yetki atanmalıdır.

1 kullanıcı birden fazla gruba mensup olabilir.

chown; owner değiştirme, chmod ise permission değiştirme için UNIX'lerde kullanılan komut satırı uygulamalarıdır. örnek:

```sh
chmod 730 "myfile.txt"
```

yada farklı bir formatta yetkiler sırası ile verilir:

```sh
chmod rwx-wx--- "myfile.txt"
```

Yukarıdaki ilk 3'lü (rwx) dosyanın owner'ı için geçerli izinlerdir. ikinci üçlü ise dosyanın grubundaki user'lara uygulanacak izinlerdir. Son üçlü ise "others" için geçerli olacak yetkilerdir.

"ls -la" komutu çıktısında, bulunduğumuz dizindeki dosyaları, yetkileri ile beraber görüntüleyebiliriz.

chmod'un aldığı sayısal parametre'ye UNIX'lerde "mode" olarak isimlendirilmektedir. mode şu şekilde belirlenir:

4 "read",

2 "write",

1 "execute"

0 "no permission"

her biri owner, grup, other için yanyana koyulur. aşağıdaki örnekler aynı anlama gelir:

```sh
chmod u=rwx,g=rx,o=r "myfile"
chmod 754 "myfile"
chmod rwxr-xr-- "myfile"
```

Sayısal değerleri tek tabloda gösterirsek:

| Octal | Binary | File Mode |
|-------|--------|-----------|
| 0     | 000    | ---       |
| 1     | 001    | --x       |
| 2     | 010    | -w-       |
| 3     | 011    | -wx       |
| 4     | 100    | r--       |
| 5     | 101    | r-x       |
| 6     | 110    | rw-       |
| 7     | 111    | rwx       |

Bazı mode'ların sembolik tanımlamaları da vardır. örnek:

- __a+rwx__ or __+rwx__

  __add (+)__ to __all (a) (owner, group, other)__ __"read" (r)__, __"write" (w)__ and __"execute" (x)__ permissions.

- __o+x__

  __add (+)__ only __"execute" (x)__ permission to __"others" (o)__.
  
  It does not override __"read"__ and __"write"__ permssions of __"others" (o)__.

- __go-rx__

  __remove (-)__ only __"execute" (x)__ and __"read" (r)__ permissions to __"group" (g)__ and __"others" (o)__.
  
  It does not override the __"write"__ permssion of __"group" (g)__ and __"others" (o)__.

- __go-rx,u=r__

  2 farklı bilgi içeriyor:

  - __go-rx__
  
    Bu kısım yukarıda anlatıldı.
  
  - __u=r__

    __"user" (u)__ 'ın tüm permission'larını sadece __"read" (r)__ olarak belirle.

# access control list (or ACL)
It is a file-system feature. It allows us to set file permissions for multiple users for the same file.

ACLs are not configured via __chmod__ command. Instead __setfacl__ is used. To read the ACLs we can use __getfacl__ command.

Adding __"r"__ and __"w"__ permissions to __"group2"__. __"group2"__ is not required to be the default file's group (of the default Linux file system).

```sh
setfacl -m g:group2:rw file.txt
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Superuser
- Linux işletim sisteminde hiç kısıtlaması olmayan kullanıcıdır.
- UID (user identifier) sıfırdır.
- bu kullanıcının ismi genelde 'root' olarak koyulur fakat bu bir zorunluluk değildir.
- dolayısı ile; terminolojik olarak root ve Superuser aynı şey değildir.

# su (command)
'substitute (vekil) user' ve 'switch user'ın kısaltmalarından gelmektedir. 'super user' ın kısaltması değildir. çünkü user switch yapmamızı sağlar ve o user ile komut çalıştırabilmemizi sağlar.

"su" komutu, kullanıcı logout/login yapmadan komut satırından diğer kullanıcılara geçiş yapmamıza yarar. "su -c" komutu ile sadece o vereceğimiz komutu run etmemizi sağlar.

başka kullanıcı ile işlem yaparken "exit" komutu ile eski kullanıcıya dönüş yapabiliriz.

sadece 'su -' komutu o anda root user'ı olmamızı sağlar. 'su abc' komutu ise; abc kullanıcısı olmamızı sağlar.

# 'sudo abc' (command)
"SuperUser Do" kısaltmasından gelmektedir.

"abc" komutunu başka kullanıcı yetkileri ile (kullanıcı ile değil!) işletmemiz sağlar. sudo komutunun geliştirildiği ilk zamanlar sadece SuperUser yetkileri ile işlem yapılması sağlanıyordu. bu sebeple ismi "SuperUser Do" kısaltmasıdır. fakat daha sonra herhangi bir kullanıcı yetkileri ile işlem yapabilmek için tasarlanmıştır. default olarak SuperUser yetkileri ile işlem yapılabilmesini sağlar.

sudo komutunu çalıştıran kullanıcının, sudo çalıştırmaya yetki verilmiş olmalıdır (sudoers dosyası aşağıda anlatılıyor). komut çalıştırılırken sorulacak şifre o andaki user'ın şifresidir.

Ubuntu, komut satırını kapatıp açana kadar yada belirli bir süreliğine tekrar şifresiz sudo komutu üzerinden komut yürütmemize izin verir. bu 2 özellik sudo'nun özelliğidir.

"sudo -i" ile sürekli olarak root yetkileri ile işlem yapmamızı sağlar.

sudo komutunun plugin altyapısı mevcuttur. kaynak: (source-id: 10) "Description" başlığı 2inci paragraf. bu sebeple her sistemdeki davranışları, OS'un plugin kurallarına göre değişkenlik gösterebilir.

# gksudo (command)
gksudo komutu $HOME dizinini /root olarak ayarlıyor. bu şekilde çalışan komut satırı uygulamasının config dosyaları kendini /root altına atıyor. oysa sudo ile çalıştırılsaydı config dosyaları /home/current-user-name altında root yetkileriyle dosyalarda bulunuyor olacaktı. yetki sorunları meydana gelecekti. Aynı zamanda (özellikle) X oturumu dosyalarında da yetkilendirme sorunu olabiliyor. Bu sebeple grafik arayüzü uygulamalarını ilgilendiren bir konu. Bu sebeple Gnome 'gksudo', KDE uygulamaları ise 'kdesudo' komutunu kullanıyor.

# Ubuntu root user
Ubuntu'da varsayılan olarak root kullanıcı ile giriş yapılmıyor. fakat kullanıcının root yetkileri mevcut. /etc/sudoers dosyasında hangi kullanıcıların kendi oturum şifrelerini bildiği takdirde root yetkileri ile komut çalıştırabileceklerini gösteriyor. işte sudo burada devreye giriyor. root kullanıcımızın bir şifresi olmamasına rağmen root yetkileri ile işlem yaptık. bu sudo komutunun bir özelliğidir. oysa Ubuntu'da "su" komutunu verdiğimizde root kullanıcısı şifresi sorar, Ubuntu'da (varsayılan olarak) root kullanıcı şifresi olmadığından root'a hiçbir zaman geçiş yapamayız. su genelde çoğu Linux sisteminde kullanılan bir komut iken; sudo bazı dağıtımlarda kullanılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Terminal (or uçbirim)
kelime anlamı:

1. Otobüs, uçak vb. taşıtların yolcularını ilk aldığı veya son bıraktığı yer.
2. Bir veri iletişim ortamında veri giriş çıkışını sağlayan donanım birimi veya donanım birimleri topluluğu.

text input/output environment. Bir sistemin yönetilebilmesi için giriş ve çıkışları olan parçadır/modüldür.

# Konsol (or console)
Türkçe sözlükte: "işletmen uçbirimi" olarak belirtilmiştir. yani bir terminal'dir. fakat işletmen, yani operatör (Bilgisayar vb. teknik aletleri işleten kimse) için geliştirilmiş bir terminaldir.

# Sanal konsol
İşletim sistemlerindeki komut satırı uygulamaları birer "sanal konsol"dur. komutu yazan yazılımcı/programcı için geliştirilmiş konsol.

# Video oyun konsolu
Digital ortamda oyun oynayabilmek için tasarlanmış konsoldur.

# shell (or kabuk)
shell, işletim sisteminde son kullanıcı için, işletim sistemine erişmesi için sunulan arayüzüdür. Bu arayüz Gnome, KDE olabilirken, sadece komut satırı masaüstleri yada sadece komut satırı uygulaması da bir "kabuk"'tur. çünkü işletim sistemine erişmemiz için bir arayüz sunarlar.

# UNIX Shell
bu terim; UNIX-like sistemlerde, komut satırı yorumlayıcıları kapsar.

# Shell Command Language (or sh or Bourne shell)
programming language + __"command line interpreter"__ ( dili yorumlayan yazılım) described by the POSIX standards. bir spesifikasyondur. implementasyon değildir.

"command line interface" ile "command line interpreter" kısaltmaları aynıdır. fakat ikisi farklı kavramlardır.

# Bash (or Bourne-Again SHell)
Bir "sh" implementasyonudur.

Geliştirildikten sonra sh standartlarının da dışına çıkmıştır. isteğe bağlı sh uyumlu modda çalışabilir. "sh uyumlu mod"da çalıştığında hem bash hemde sh özelliklerini barındıracak şekilde script'leri run eder.

Bash'in pazar payı yüksek olduğundan; Çoğu UNIX-like sistemde /bin/sh uygulaması çalıştırıldığında, aslında bash çalıştırılır. /bin/sh sadece bir sembolik bir linktir (kısayol). bin/sh sadece bash'i değil, sistemde tanımlı olan default sh implementasyonunu çağırır.

# Z Shell (or Zsh)
SH implementasyonudur.

# diğer shell'ler
Fish, c shell, Dash, Xiki Shell (or Xsh), KornShell...

MacOS, bash kullanmaktadır. Güncel sürümlerinde (ortalama yıl: 2019) artık Zsh ile birlikte gelmektedir.

# C Shell (or Csh)
bir 'shell'dir. C diline benzeyen syntax'ı olduğu için C ismini almıştır.

# Tcsh
C shell fork'udur. C-Shell ile geriye uyumludur.

Bazı OS'larda "csh" kısayolu, tcsh'ye yönlendirilmiştir (sh-->bash'teki gibi).

# Fish
Fish is not POSIX or shell compatible. fakat çok daha sonra geliştirilmesi başlandığı için çok güncel teknolojileri baz alıyor. GPL lisanslı. Herkes genel olarak çok başarılı buluyor fakat shell uyumlu olmaması sebebi ile hiçbir OS default yüklü getirmemektedir.

# Oh My Zsh
sadece Zsh için konfigürasyonlar community tarafından bu proje altında dağıtılmaktadır. bu konfigürasyonlar ile interpreter olan Zsh, son kullanıcı için çok daha fazla yeteneğe sahip oluyor. bu özellikler terminal'den bağımsızdır.

Oh-my-zsh bilgisayara kurulduğunda istenirse root izinli bir dizinine, yada istenirse de $HOME/.oh-my-zsh/ dizinine kuruluyor. burada birçok script mevcut. Bu script'lerin devreye girebilmesi için de $HOME/.zshrc dosyasında (bu dosya Zsh tarafından execute ediliyor) bir referans (basit bir script çağırma satırı) mevcut. yani Oh-my-zsh tamamiyle taşınabilir durumda. $HOME/.oh-my-zsh/ dışında hiçbir yere kurulum yapılmıyor. $HOME/.oh-my-zsh/ dizini bir setup'tan değil, "git" aracılığı ile çekiliyor.

# Terminal Emulator (or CLI or command line interface)
shell yada alternatiflerini kullanabilmemiz için GUI yazılımlarıdır.

"Terminal" UNIX-like dünyasında bir device dosyasıdır. Bu dosyaya yazma işini "Gnome terminal" gibi yazılımlarla; yani "terminal emulator"'larla yaparız.

Terminal Emulator içerisine komutlar yazdığımızdan, "Terminal Emulator"a eş anlamlı olarak CLI de kullanılmaktadır.

UNIX ve benzeri sistemlerde herşey dosyadır ve terminalde bir dosyadır. bir komutun çıktısı ve girdisi dosyadan okunacak şekilde basittir. Oysa powershell bir script dilidir. üst seviyeli API aracılığı ile Linux'ta yapılabilen birçok şeye imkan sunar.

# piyasadaki bazı terminal emülatörler

| name                              | notes                                                                                                                                                                                                                                   |
|-----------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| xterm                             | ilk olarak X Microsoft Windows manager ile uyumlu çalışabilmesi için tasarlandığından "x" prefix'i takılmıştır.                                                                                                                                    |
| UXTerm                            | xterm'in bazı karakter seti desteği eklenmiş paketinin ismidir.                                                                                                                                                                         |
| GNOME Terminal                    | Gnome desktop projesinin bir parçası olan terminal yazılımıdır. Uygulama açıldığında GUI'deki title'ı sadece "terminal"'dir. fakat bu son kullanıcıya kolay olması amaçlı bu şekilde tasarlanmıştır. orijinal ismi "Gnome terminal"'dir. |
| Terminator                        | Java ile yazılmış açık kaynaklı bir terminal yazılımıdır.                                                                                                                                                                               |
| MacOS için terminal               | MacOS içerisinde gömülü gelen terminal yazılımının özel bir ismi yoktur. direk olarak "terminal" olarak isimlendirilmektedir.                                                                                                           |
| Iterm                             | sadece MacOS için geliştirilen terminal yazılımıdır. proje uzun zamandır güncellenmediğinden __Iterm2__ isimli proje hayata geçirilmiştir.                                                                                              |
| Guake                             | Gnome için masaüstünde panelden açılabilen komut satırı yazılımıdır.                                                                                                                                                                    |
| Konsole                           | KDE projesinin parçası olan komut satırı yazılımıdır.                                                                                                                                                                                   |
| Terminology                       | enlightment masaüstü projesi içinde gelen terminal yazılımıdır.                                                                                                                                                                         |
| Upterm (or old-name:Black Screen) | web altyapısı üzerine kuruludur. sonlanmıştır (yıl 2019).                                                                                                                                                                               |
| hyper (or old-name:HyperTerm)     | web altyapısı üzerine kuruludur. hyper uygulaması, HyperTerminal ile karıştırılmamalıdır. HyperTerminal Microsoft Windows için çok farklı amaçta geliştirilmiş bir yazılımın ismidir.                                                   |
| Tilix                             |                                                                                                                                                                                                                                         |
| Terminus                          |                                                                                                                                                                                                                                         |
| Alacritty                         |                                                                                                                                                                                                                                         |
| Putty                             | SSH, için tasarlanmıştır. uzak masaüstündeki shell'i interpreter olarak kullanır. Mintty projesi buradan birçok alıntı kod içermektedir.                                                                                                |
| KiTTY                             | Putty projesinin fork'udur.                                                                                                                                                                                                              |
| Mintty                            | aşağıdaki başlıklarda açıklanmıştır                                                                                                                                                                                                     |
| Tilda                             |                                                                                                                                                                                                                                         |
| SecureCRT                         | ticari                                                                                                                                                                                                                                  |
# Microsoft Windows

- ## PowerShell
  command line interpreter'dır. (tabi yürüttüğü script dili de yenilendi. cmd.exe'den farklı dil kullanıyor.)

- ## Microsoft Windows Terminal
  kodadı: Cascadia

  Microsoft'un 2019'da geliştirmeye sıfırdan başladığı, açık kaynaklı, sadece terminal emulator.

- ## Microsoft Windows Console
  terminal emülatördür. açılan GUI ekranıdır. powershell ve cmd default'ta bununla açılır. kaynak: (source-id: 11) "Microsoft’s Big Bet – Windows NT" başlığı, son paragraf.

- ## command.com
  command line intepreter'dır.

  __MS-DOS command interpreter__ olarak ta anılır çünkü MS-DOS tabanlı sistemlerde bu kullanılıyordu. kaynak: (source-id: 12) 10'uncu sayfa, "Command shell overview" başlığı, ilk paragraf.

  .bat uzantılı dosyaları yürütür.

- ## cmd.exe (or Command prompt)
  command line intepreter'dır.

  terminolojik olarak; "Command prompt" ile aynı anlamdalar. (source-id: 11) "Microsoft’s Big Bet – Windows NT" başlığı, son paragraf.

  .CMD uzantılı dosyaları yürütür. fakat ".bat" ile geriye uyumludur. kaynak: kaynak: (source-id: 14) "Also, Cmd != MS-DOS!" başlığı, 7'inci madde.

  Microsoft Windows NT sürümlerinde default olarak 'cmd.exe'ye geçiş yapmıştır. kaynak: (source-id: 14) "Also, Cmd != MS-DOS!" başlığı, 6'ıncı madde.

- ## Microsoft Windows Console Host (or Console Microsoft Windows Host)
  conhost.exe'ile yürütülen yazılımdır.

  Microsoft Windows 7 ile gelmiştir. kaynak: (source-id: 16) "So, what’s inside the Windows Console?" başlığı.

  Microsoft Windows 7 öncesi conhost yerine "ClientServer Runtime System Service (or CSRSS)" kullanılıyordu.

  cmd ve powershell ve diğer komut satırı uygulamaları buraya attach olarak çalışırlar. buna zorunlu değiller. 3üncü parti conhost alternatifleri mevcuttur.

  conhost'un sunduğu API "Console API"'dir. Bu API'yi kullanabilmek için "Console Driver (ConDrv)"'a ihtiyacımız var.

# Komut satırı olarak terminal emülatörler

Bu tarz emülatörler menubar border içermezler ve farklı emülatör içerisinden de çağrılabilirler. Komut satırından çağrıldıklarında aynı satırda kendisi başlamış olur. Özellikle desktop manager/window manager olmayan ekranlarda kullanılmak üzere tasarlanmışlardır.

- Linux console
- GNU Screen
- Minicom
- Tmux

# Xterm türevleri
Xterm açık kaynaklıdır. onu fork eden birçok proje geliştirilmiştir:

liste için kaynak: (source-id: 17) "What versions are available?" başlığı.

- Ansi_xterm
- Color_xterm
- Cxterm (Chinese)
- Hanterm (Korean)
- Mxterm
- Nxterm
- Kterm (Japanese)
- Xterm (forked by "X Consortium")

Ekranda renkleri gösteren katman terminal emülatördür. interpreter karakterleri output olarak verir gerisine karışmaz. aldığı output'larda escape karakterlerin ne olup olmadığı terminal emulator'ın standartlarına bağlıdır. bu standartlara __escape sequences__ denir. örnek sequence: __DEC VT220__. kaynak: (source-id: 18) "DESCRIPTION" başlığının sondan birinci paragrafın son cümlesi.

Bazı kaynaklarda __escape sequences__ için protokol terimi de kullanılmaktadır. Bir terminal emülatör manual sayfasında hangi protokolleri desteklediğini yazar. Bazen desteklediği protokollerde "xterm" görebiliriz. Bu; xterm'in desteklediği protokolleri desteklemektedir, anlamında kullanılır.

"Terminal emulator" terimi de "terminal emulation"dan gelir. çünkü VT220 birer terminal cihazlarıdır (__Digital Equipment Corporation (or DEC)__ tarafından geliştirilmiştir). Onların sequence'lerini komut satırı GUI yazılımımız desteklediği için, GUI yazılımımıza terminal emulator denir.

Genelde "$TERM=xterm-*" olan terminaller xterm'in desteklediği escape sequences'ları dinliyor anlamına gelir. Fakat bu kuralı tüm terminaller doğru şekilde uygulamıyor. kaynak: (source-id: 17) "What versions are available?" başlığında ortada olan 3 maddelik paragraf.

Bazı terminaller birden fazla sequence'yi destekliyor. örnek "Gnome terminal". kaynak: (source-id: 18) "DESCRIPTION" başlığının son 3 paragrafı.

En başta belirtildiği gibi: Ekranda renkleri gösteren katman terminal emülatördür. fakat echo ve printf gibi komutlar (or herhangi bir çıktı üreten komutlar), escape karakterini dışarıya yansıtıp yansıtmayacağı önemlidir. çünkü yansıtıp yansıtmayağına göre, terminal emülatör onun renkli olup kontrol etmektedir. bu sebeple bazen komutun or shell'in nasıl davrandığı da renklendirmek için önemli olabiliyor.

# Cygwin vs MinGW (or Minimalist GNU for Microsoft Windows)

açık kaynaklı bazı Linux komut satırı uygulamaları, Microsoft Windows için tekrar derlenerek Microsoft Windows tarafında kullanılabilmektedirler. Fakat sadece POSIX uyumlu API'ler çağrıldığında sorun olacaktır. Buna çözüm olarak; Cygwin bu API'lerin yerini tutan Microsoft Windows karşılığındaki adaptor/emülatörler içermektedir. Cygwin, bir dll dosyasıdır ve POSIX için gerekli tüm API'lere denk gelen Microsoft Windows API'lerini çağırır.

MinGW de ise böyle bir katman yoktur. MinGW direk Microsoft Windows için çalışan native uygulamalar paketidir. normalde; GCC sadece Linux'a derlemek için tasarlanmıştır. Oysa MinGW; di̇rek olarak gcc'nin wi̇ndows'ta çalışacak Exe dosyasını üretmiştir. bunu yapabilmek için GCC'nin kodlarında gerekli değişiklikleri yapmaktadırlar. MinGW aynı zamanda birçok POSIX uygulamasını da sadece Microsoft Windows'ta çalıştırabilecek şekilde kodlarını değiştirip, Microsoft Windows için derleyip hazır şekilde sunmaktadır.

Cygwin ile Wine'ın mantığı tamamiyle farklıdır. Wine direk Microsoft Windows executable'larını, Linux'ta çalıştırırken, Cygwin Linux executable'ı çalıştırmaz. Cygwin, UNIX executable'larının Cygwin/Microsoft Windows için derlenmiş hallerini çalıştırır. çalıştırırken ek olarak bir dll aracılığı ile, bazı API isteklerinin önüne geçer.

# MSYS
Minimal SYStem'ın kısaltmasıdır.

MinGW ile derleme yapmayı kolaylaştırmak için ek geliştirilen bir altyapıdır. MSYS bir UNIX programının Microsoft Windows ortamında derlenebilmesi için gerekli tüm tool'ları barındırır. örnek:
- make
- gawk
- grep
- bash (sadece interpreter, terminal GUI uygulamasını barındırmaz)

kaynak: (source-id: 21) 1 ve 2inci paragraf.

# MSYS vs MSYS2
MSYS2, MSYS'den bağımsız geliştirilen çalışmadır. fork değildir. aynı amacı güttüğü için bu şekilde isim verilmiş.

MSYS2 kendi içerisinde pacman paket yöneticisini yüklü getirir.

MSYS ve MSYS2, birer Cygwin fork'udur. kaynak: (source-id: 22) 3 vs 6'ıncı paragraf.

"Git for Microsoft Windows" başlığında __git-bash__, __msysGit__ hakkında bilgiler mevcut.

# mintty
Microsoft Windows için açık kaynaklı terminal emülatördür. Microsoft Windows'un komut satırı ile hiçbir bağlantısı yoktur. aşağıdaki paketler opsiyonel olarak mintty ile entegreli çalışabilir:
- MSYS
- MSYS2
- Cygwin

# Termux
- Android için açık kaynaklı terminal emülatördür.
- Android işletim sistemi biraz farklı bir dizin yapısı kullandığı için apt-get ile kurulan komut satırı uygulamaları bu terminalde düzgün şekilde çalışamayabiliyor. benzer şekilde bu uygulama root olmayan cihazlarda çalışmak üzere tasarlandı. bu sebeple apt ile kurulan uygulamalar storage'ye kuruluyor. dolayısı ile komut satırı uygulamaları bu sebepten düzgün çalışamıyorlar. çünkü normalde o uygulamalar root yetkisi ile sistem kurulacak şekilde tasarlanmıştır. kurulum yöntemi farklı olunca, çalışmasında da sorunlar olabiliyor.
- Android üzerinde her uygulamanın kendi user'ı var. bu uygulamaya sandbox yaptırmayı kolaylaştırıyor. uygulama bir yetki istediğinde, aslında kullanıcıya yetki veriliyor. her app'in bir user olmasının birbirinin dosyalarına erişememelerini de sağlıyor. bu sebeple; Termux üzerinde çalıştırdığımız her komutta bunu değerlendirmemiz gerekiyor.

# CTRL+ALT+F1
Linux'ta GUI masaüstü ekranı açıkken buna basıldığında, tüm ekranda konsol ekranı açılır. arkaplanda varsayılan olarak 6 adet ekran açıktır. 7inci ekran GUI'nin kendisini (X or Wayland) açmış olan session'dır.

CTRL+ALT+F1 ile ilk konsole ekranı açılırken, CTRL+ALT+F2 ile ikincisini açabiliriz.

Eğer GUI ekranında değilsek, yani konsoldaysak, ALT+FX ilx X'inci session'a geçebiliriz. ALT+→ ve ALT+← ile bir önceki veya bir sonraki session'lara gidebiliriz.

# Windows Subsystem for Linux (or WSL)
- 1.0 ve 2.0 sürümleri arasında çok büyük farklar var.
- Tüm WSL sürümleri sadece 64 bit Microsoft Windows'larda ve 64 bit Linux executable'larını yürütebiliyor.
- Tüm WSL sürümleri Linux GUI uygulamalarını desteklemeyecek şekilde tasarlanmıştır.
- WSL 2 direk özel bir VM içerisinde çalışıyor. Linux executable'ları çalıştırılmak istendiğinde direk olarak çekirdekte çalıştırılıyor. Oysa WSL 1.0'da istek interpret ediliyor ve eğer varsa POSIX system call'ları emüle ediliyordu. kaynak: (source-id: 23) "Full System Call Compatibility" başlığı, 1inci paragraf.
- WSL 2'de optimize edilmiş özel bir Linux kernel'i ile geldiği için boot-time ve performance sorunları çok düşük. kaynak: (source-id: 23) "A quick explanation of the architectural changes in WSL 2" başlığı, 1inci paragraf.
- wsl komut satırı uygulaması ile Linux komutları CMD üzerinden de çağrılabilmektedir. örnek:

  ```sh
  wsl echo "hello"
  ```

  kaynak: (source-id: 25) "Run Linux tools from a Windows command line" başlığı.

- WSL'in Linux shell'i üzerinden Microsoft Windows uygulamaları da çağrılabilir:

  ```sh
  ipconfig.exe | grep "IPv4"
  ```

  kaynak: (source-id: 25) "Run Windows tools from Linux" başlığı.

- WSL içindeki Linux ile Microsoft Windows user'ları birbirlerinden tamamen bağımsız.
- Microsoft Windows dosyalarına WSL içerisinden bu şekilde erişiyoruz: "/mnt/c/Program Files"
- Microsoft Windows'tan WSL dosyalarına erişmek için:

  > \\\wsl$\ubuntu

  > \\\wsl$\distro-name

  direk olarak bu değeri path olarak kullanabiliriz. WSL içerisinde gömülü file server geliyor. Microsoft Windows ise client tarafta bu file server'ı dinliyor. Bu sebeple WSL kapalı ise içindeki dosyalara erişemeyiz.

  Aynı zamanda bu durumun bilinen önemli bug'ları var: https://devblogs.microsoft.com/commandline/whats-new-for-wsl-in-windows-10-version-1903/ "Known issues" başlığı.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# komut satırı path değişkenleri

işletim sistemi aşağıdaki dosyaların içindeki komutları sırası ile işletir.

/etc/profile  --> her login sonrası (tüm user'lar için) çalıştırır.

~/.profile -> her login sonrası bu home klasörünün sahibi olan user için bu dosyadaki komutlar çalıştırılır.

~/.bashrc  --> shell uygulaması başlatıldığında, ilk olarak bu dosyanın içindeki komutları sırası ile çalıştırılır. örneğin 'bash' komutunu çalıştırdığımızda buradaki kodlar tekrar çalıştırılır.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# builtin (internal) command
bu komutlar birer program değildir. bu komutlar terminal emülatörlerın sunduğu bir özellik değildir. bu komutlar interpreter'ın (Bash, Zsh gibi) sunduğu keyword'lerdir/özelliklerdir. örneğin "which echo" yazdığımızda sonuç alamayız. çünkü echo bir builtin command'dır.

Bazı terminaller yada OS'ler, shell builtin'leri ile aynı isimde programlar eklerler. bu durumda override eden (ki muhtemelen shell override edemeyecek) program çalışacaktır. Dolayısı ile komutlara aynı parametre yollasak bile farklı ortamlarda farklı sonuçlar alabiliriz.

"bash-builtins" komutu bize bash'in internal komutlarını listeler. bu liste aşağıdaki gibidir:

alias, bg, bind, break, builtin, case, cd, command, compgen, complete, continue, declare, dirs, disown, echo, enable, eval, exec,  exit,  export,  fc,  fg,  getopts, hash, help, history, if, jobs, kill, let, local, logout, popd, printf, pushd, pwd, read,  readonly,  return,  set,  shift,  shopt,  source, suspend,  test,  times,  trap,  type, typeset, ulimit, umask, unalias, unset, until, wait, while

isimleri ilginç fakat bunlarda builtin'dir:

```sh
:
```

```sh
exit 0 # (sağlıklı) dönüş yapan boş bir uygulamadır
```

```sh
.
```

source ile aynı görevi görür. parametre olarak source edilecek dosyayı alır.

```sh
[
```

çoğu yazılımcı bunu syntax olduğunu düşünür, oysa değildir. __"test"__ komutu ile aynı görevi görür.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## sourcing script vs executing script
topic search keyword: sourcing script vs execute script

"source ./file.sh" vs "./file.sh"

ikiside file'ı aynı şekilde işletir. fark şudur: source dosyayı current-process'te (current-shell'de) execute eder. bu sebeple file'ın değiştireceği her variable tüm shell akışı boyunca kalıcı olur. oysa "./file.sh" yeni bir sub-shell (sub-process) açılır ve file'ın oluşturduğu hiçbir variable/fonksiyon shell'de kalıcı olmaz.

source komutu yerine bu şekilde de kullanım aynı görevi görecektir:

```sh
. command
```

noktalı gösterim yerine source kullanmak daha best-practice'dir. çünkü "source" bir komut satırı uygulamasıdır. komut satırı olunca daha portable oluyor. çünkü built-in'ler her ortama göre değişebiliyor. bazı ortamlarda "." kullanılmıyor. örneğin fish'in güncel versiyonlarında "." kullanılmıyor. kaynak: (source-id: 27) 5'inci paragraf.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# komut satırı uygulamalarının help/manuel açıklamaları
"man pwd" ile pwd'nin manuel dosyasını ekrana bastırırız. her manuel'de SYNOPSIS (Türkçe kelime anlamı: özet) başlığı vardır. Bu başlıkta komut satırının parametrelerinin nasıl yollanacağı açıklanır.

SYNOPSIS'in %100 bir standartı yok. birçok firma kendi standardını resmi sitesinden yayınlıyor. fakat her uygulama bunlara uymuyor ve firmalar arası ufak farklar mevcut. örnek bir SYNOPSIS standartları standardı: (source-id: 441)

örnek SYNOPSIS ve anlamları:

```sh
command [options] <file1> [<dir1>]... param {a|b|c}
```

- "options" başlığındaki parametrelerden birini alacak anlamına gelir.
- options'taki [] işareti, bu alanın (options alanının) opsiyonel olduğunu gösterir. yani yollanması zorunlu değildir.
- file1 zorunlu bir alandır. çünkü [] içinde değil.
- file1 <> içine alınmış çünkü bir değeri ifade ediyor.
- dir1 opsiyoneldir ve ... birden fazla parametre yanyana olabilir anlamını kazandırır.
- "param" <> içine alınmamış. çünkü bir değeri ifade etmiyor. hardcode bir string. yani son kullanıcı buraya sadece "param" yollayabilir.
- {a|b|c} a veya b veya c parametresi alır. bazı standartlarda {} yerine () kullanılır. c'nin altı çizili olsaydı c default değer (yani son kullanıcı o parametreyi yollamazsa) alınacak değer anlamına gelir.

- kalın yazılmış kelimeler:
  - <> alınmayan (sabit/hardcode string'leri)
  - komutun kendisi

- italik yazılan kelimeler:
  - <> içerisine alınan değerlerdir

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# coreutils
açık kaynaklı c ile yazılmış, hiçbir dependency gerektirmeyen en temel komut satırı uygulamalar grubudur. tümü bir Git repository'sinde düzenli olarak geliştirilir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

## tty
TeleTYpewriter'ın kısaltmasından gelir.

Teknoloji dünyasında; __eng:teleprinter (or teletypewriter or teletype or TTY or tr:telem)__ bir metnin uzağa gönderilmesine yarayan, elektromanyetik cihaz.

tty komut satırı uygulaması; stdout'a, stdin (stream) dosyasının full path'ini dönen komut satırı uygulamasıdır.

örnek; "tty" komutu çıktımız "/dev/pts/0" olsun. aynı komut satırında "read my_variable" komutunu verirsek, komut satırı stdin'den bizim bir değer girmemiz ve bu değeri ekrana basıp aynı zamanda my_variable'a atayacaktır. read komutu bu işi görür. biz hiçbir değer girmezsek ve farklı bir terminal session'ında "echo hello > /dev/pts/0" yazarsak, diğer session'daki read komutuna "hello" string'i gider.

her açtığımız terminal session'ı (SSH olsun, local terminallerimiz olsun) hepsi farklı stdin'e sahiptir. bu sebeple her açtığımız terminal'de tty değerleri genelde bu şekilde ilerler: /dev/pts/0, /dev/pts/1, /dev/pts/2...

burada basit bir anlatım var: (source-id: 28) "Enter, the Pseudo Terminal (PTY)" başlığı.

## pseudoterminal master (or PTM) vs pseudoterminal slave (or PTS)
terminal interpreter'lar (SSH, xterm, Gnome terminal...) bir terminal session'ı açtıklarında bir PTS ve PTM dosyası oluştururlar. bunlar device dosyalarıdır.

__PTS__ ve __PTM__ ikilisine birlikte __Pseudoterminal (or PTY or Pseudo-TTY or Pseudotty or Pseudo-TTY)__ denir.

Terminal GUI uygulamamız (örnek "Gnome Terminal"), __PTM__'e (master'a) yazar ve oradan okuyup GUI'ye metin basar. Oysa "interpreter" (örnek bash,zsh) PTS'e yazar ve okur.

__PTM__ ve __PTS__ arasında sürekli I/O alışverişi var. Arada da başka katman yok. Aslında burada tek dosya ile de bu işlem yapılabilirmiş gibi görünüyor fakat bu 2 dosyanın neden ayrı olduğunu bilmiyorum.

USB'den bir fiziksel cihazdan input'lar okunduğu zaman __PTS__ ve __PTM__ kullanılmıyor. Sadece __PTS__ gibi çalışan "__TTY__" dosyası oluyor. Onun önünde de USB'ye aracılık eden diğer driver dosyaları oluyor.

Aslında; __PTS__ ve __PTM__, birer __PTY__ dosyasıdır. __Master__ ve __slave__ etiketlerini biz developer'lar atarız. Bazı kaynaklarda "__slave__" yerine "__follower__", "__master__" yerine "__leader__"

__tty__ komutu __PTS__ dosyasını döndürür.

Görsel olarak çizmek gerekirse her terminal sekmemiz (dikkat pencere değil!) için şöyle bir akış vardır:

```
                                  ____________          ____________          ____________          ____________ 
   GUI      ----> Input   ---->  |  PTY file  | ---->  | TTY Driver  | ----> |  PTY file  | ---->  |   Zsh      |
(End-user)  <---- Output  <----  | (PTM file) | <----  | (OS kernel) | <---- | (PTS file) | <----  | (or Bash)  |
                                  ____________          ____________          ____________          ____________ 
```

"__TTY Driver__" includes the "__line discipline__" component. "__line discipline__" yeni satır karakteri algılayacana kadar, gelen karakterleri bellekte tutmaya yarıyor. Yani her gelen karakteri direk PTS'e stream etmiyor. Tabi birçok görevi dah var.

Yukarıdaki görselde Zsh, "find" komutunu tetiklemiş olsun. find komutu, Zsh tarafından fork ediliyor. Fork edilen process'lerin input ve output'ları aynı dosyaya referans eder. Bu sebeple find komutunun çıktısı direk olarak PTY file'ına yazılır. Yani; Zsh aracılık (I/O stream) etmez.

Yukarıda anlatılan tüm bilgilerin OS'tan OS'a istisnası var ve bazı durumların opsiyonel olma özelliği var. Yukarıdaki bilgiler temel mantığı anlamak için yeterli.

# interactive Shell
stdin ve stdout'ların son kullanıcının terminalinden okunduğu shell session'ıdır.

tersi durum non-interactive Shell'dir.

# login shell
--login ile açılan shell oturumudur. tersi durum non-login shell'dir. örneğin;

```sh
bash --option
```

--login ile açılan shell, user profilinde olan startup script'lerini (örnek: ~/.bash_profile, ~/.bash_login, ~/.profile) otomatik execute eder. exit yapıldığında da ~/.bash_logout dosyasını execute eder.

login ve interactive kavramları birbirinden bağımsızdır.

putty ile uzak makinaya bağlandığımızda login ve interactive shell oturumu kullanırız. İlgili oturumda komut satırına bash yazarsak, açılan yeni oturum non-login shell ve interactive'dir.

Eğer bir shell script'i koşuyorsak (terminalden veya terminalsiz), bu oturum non-login ve non-interactive'tir.
