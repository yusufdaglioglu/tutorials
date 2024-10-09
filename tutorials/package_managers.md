############################

############################
# PACKAGE MANAGERS
############################

############################

# Homebrew
Sadece MacOS için yapılan açık kaynaklı paket yöneticisi projesinin ismi. komut satırından __"brew"__ olarak kullanılmaktadır.

Linux ve "Microsoft Windows Subsystem Linux" için aynı temele dayanan yazılım paralelden ayrı geliştiriliyordu (Linux için olan "Linuxbrew" projesiydi). daha sonra bu projelerin tümü birleştirildi.

Homebrew, Nix ile çok benzer yapıdadır. fakat Nix, homebrew ile karşılatırıldığında çok daha gelişmiş kalmaktadır.

## Formulae
Formulae paket kurulumu için gerekli tanımlamaları içeren DSL metindir. Homebrew tarafından kullanılır.

## Taps (or Third-Party Repositories)
Homebrew repository yapısını destekler.

homebrew-cask (homebrew/cask) ve homebrew-core (homebrew/core) repository'leri (Tap'ları) hala farklıdır.

# Bottles (or Binary Packages)
Homebrew local makinemizde bir uygulama kurarken, her defasında Formula'dan binary compile etmez. Çünkü bu enerji ve zaman tüketir. Bu sebeple önceden build edilmiş binary'ler indirilir. Tabi bu opsiyoneldir. Homebrew ile bir paketi komut satırına parametre geçerek compile ettirerek kurdurabiliriz.

Homebrew için hazır binary'ler farklı CPU mimarileri için farklı olabilir. Homebrew uygun olanını bizim için indirecektir. Hazır binary'ler genelde eski CPU'lara uygun şekilde build edilmiş oluyorki, her tarafta çalışabilsin. Fakat bu dezavantajlı bir durum.

## Cask
Homebrew modülüdür. GUI kullanan uygulamaların yönetimini sağlamaktadır.

ilk başlarda Homebrew'den bağımsız geliştiriliyordu. daha sonra Homebrew altında ortak geliştirmeye başlandı.

Cask sadece MacOS'ta desteklenmektedir (yıl 2019).

# Flatpak (or old-name:xdg-app)
flatpak, sadece UNIX tabanlı sistemler için geliştirilmiştir. user-yetkisinin yeterli olduğu sandbox teknolojisine dayanıyor (teknoloji adı: Bubblewrap).

# Bubblewrap vs Firejail vs Sandstorm
Linux'ta user-namespace teknolojisi genişletilerek root yetkisi olmadan sandbox içinde uygulama çalıştırmaya yarayan birbirlerine alternatif teknolojilerdir.

# Filesystem in USErspace (or FUSE)
root yetkisi olmadan bir process'in root dizinlerine mount edebilmesini sağlıyor. tabi bu mount edilen dizini sadece ilgili process'in kendisi görebiliyor.

OS kernel'inde "fuse kernel module" yüklü olması gerekmektedir. Process'in de "Libfuse Userspace library" kullanması (API'lerini çağırması) gerekmektedir.

Desteklediği OS resmi olarak sadece Linux ve BSD. Fakat 3üncü parti fork'lar MacOS (OSXFUSE-macFUSE) içinde çalışmasını sağlamaktadır.

FUSE bir sandbox teknolojisi değildir.

# AppImage (or old-name:klik or old-name:PortableLinuxApps)
tüm dependency'ler tek bir dosya içinde tutuluyor. hiçbir paket yöneticine ihtiyaç barındırmıyor. direk executable'ın yürütülmesi yeterli oluyor. FUSE tarzı altyapı kullanarak çalışıyor.

# Nix
Nix sistemden bağımsız çalışan paketleme sisteminin ismidir. her uygulama root dizininde "store" olarak isimlendirilen bir dizinde saklanıyor. her user ise kendi path'inde istediği kısayolları ekliyor. Nix bir paket yöneticisidir.

Nix, MacOS'te de native olarak çalışıyor. Nix namespace teknolojilerini, sadece build sırasında işlem yaparken kullanıyor. fakat kurulumdan sonra uygulamalar namespace teknolojileri ile çalışmıyor. bu sebeple Firejail gibi uygulamalar Nix ile entegre çalışabilirken, Snap ile çalışmamaktadır.

Snap ve flatpak ise cgroup, namespace gibi teknolojilerden direk olarak yararlanıyor. kaynak: (source-id: 267) 2nd paragraph.

# NixOS
tüm uygulamaların Nix paketleme yöntemi ile yönetildiği Linux tabanlı işletim sistemi.

# Guix
Nix'in sadece açık kaynaklı paketleri kabul eden türevi.

# nix komutlar
> nix-env -qaP | grep -i vlc

search ignoring case. the list is not same as official web site. her satır bir uygulamayı gösteriyor. ilk sütun program için "attribute name" ikinci ise "package name".

> nix-env -i vlc

install vlc by package name. install komutunun parametresi regex'tir. bu sebeple sadece "vlc" dediğimizde "vlc" regex'ine uyan tek bir sonuç var ise, o paketi yükler.

> nix-env -iA vlc

install vlc by attribute name

> nix-env -q

list all installed packages

> nix-env -e vlc

removes the package

> --arg config '{ allowUnfree = true; }'

bu parametre install ve update package komutlarına geçilebilir. böylece lisanslı paketlerde indirilmeye açık olur.

> nix-channel --update nixpkgs
> nix-env -u '*'

update Nix packages.

> nix-collect-garbage -d
when you remove a package with -e, it only removes the symbolic links from user profile. to remove completely the packages from "Nix store" use above command.

(with -d parameter we remove also the rollback (old generation) packages)

# remove Nix
- If you don't use Nix daemon, stop it.

- it is enough to remove the path variables of Nix from current OS user. the path file is auto added by Nix-installer here:

  $HOME/.profile

  remove this line:

  > if [ -e /home/user_name/.nix-profile/etc/profile.d/nix.sh ]; then . /home/user_name/.nix-profile/etc/profile.d/nix.sh; fi # added by Nix installer

- Also run this command (optionally): "sudo rm -r /nix"

- Also (optionally - because they are only folders) "rm $HOME/.nix-channels $HOME/.nix-profile $HOME/.nix-defexpr"

# wrapped vs unwrapped Nix packages

örneğin Firefox'un her iki prefix ile de paketi mevcut. wrapped olanı eklentilerin/patch'lerin kurulu olduğu pakettir. unwrapped ise saf halidir.

# Firejail
bazı komutlar:

(diğer komutlar burada https://firejail.wordpress.com/features-3/man-firejail-profile/)

"vlc" komutunun sağında kalan her parametre vlc'ye geçilir
> firejail vlc -file /videos/video.mp4

interneti devre dışı bırakmak için:
> firejail --net=none vlc

X'i farklı bir ekrandaymış gibi açar. bu sebeple Firefox'u sanal bir penceredeymiş gibi görürüz. açılan uygulama sistemden ekran görüntüsü alamayacaktır.
> firejail --x11 vlc

/root ve /home/user dizinleri artık tamamen boştur/sanaldır.
> firejail --private vlc

/home/user dizini artık /path1'dir.
> firejail --private=/path1 vlc

geçici oluşturulan /home/user dizinine path1'deki tüm dosyalar kopyalanır. sandbox kapandığında geçici olan home dizini silinir ve /path1 eskisi gibi kalır.
> firejail --private-home=/path1 vlc
> firejail --private-home=/path1/file1 vlc # sadece file1 geçici home dizinine kopyalanır

iki adet DNS algılatırız
> firejail --dns=8.8.8.8 --dns=8.8.4.4 firefox

local network adreslerine erişememesi önceden hazır gelen "nolocal.net" dosyası
> firejail --netfilter=/etc/firejail/nolocal.net firefox

IP (ifconfig yerine kullanılıyor) komutunu sanal olarak bir network'e atadık. network bilgilerini (örnek MAC adres) rastgele firejail tarafından üretiliyor. IP'yi elle verdiğimizi için rastgele üretilmedi. bu sanal network'un bağlı olduğu dış network eth1'dir. eth1 o sıra bilgisayarımızda ayakta olmalıdır.
> firejail --net=eth1 --ip=192.168.0.10 ip -c a

profil dosyasında komut satırında geçeceğimizi bir çok parametreyi bulundurabiliyoruz. bu şekilde komutlarımız daha kısa oluyor ve aynı profil dosyasını birçok  uygulama için kullanabiliyoruz.

Eğer "--noprofile" parametresini vermezsek default olarak aynı executable isminde bir dosyayı Firejail'in profilleri tuttuğu dizinde arar. eğer aynı isimde profil dosyası bulursa o profil dosyasını aktif eder.

"--noprofile" parametresi geçilmezse aynı zamanda şu da olur: default olarak bazı profiller otomatik devreye alınıyor. bu profil dosyaları okunduğunda komut satırına log atılıyor.

> firejail --profile=/file.profile vlc

# Guix System Distribution (or GuixSD)

Guix paket yöneticisi içeren işletim sistemi.

# portableapps.com

sadece Microsoft Windows için uygulamaların portable hale getirildiği projedir. portable uygulamayı çalıştırmak için paket yöneticisine ihtiyaç yoktur.

# Snap

container içinde masaüstü/sunucu uygulamaların çalıştırılmasını sağlıyor.

# Snappy

Snap app manager

# Snapcraft

"Snap formatı"'na uygun uygulama paketlemek için gerekli tool.

# Snap commands

- snap find vlc --> finds apps on market

- snap install vlc

- snap install --beta vlc

- snap install --devmode vlc --> Snap uygulamasının bazı dizinleri (örnek /etc/) okumaya yetkisi yoktur. bunu aşmak için tüm dizinlere yetki veriyoruz.

- snap remove vlc

- snap list --all -> lists installed apps

- snap refresh vlc --> updates vlc

- snap disable vlc --> VLC'yi silinmiş gibi yapar fakat dosyaları kalır. eğer arkaplan servisi varsa önce onu stop eder. enable'da ise servisi varsa başlatır.

- snap services start Docker --> servisi başlatır. her Snap app'te servis olmak zorunda değil. arkaplanda sürekli açık duran sistem servisleri Snap tarafından destekleniyor. POSIX'lerde log'lar /var/log dizini altında saklanırlar. Snap'in servislerinin log'ları da buraya düşmektedir.

# containerized apps HOME folder

Tüm uygulamalar normalde $HOME dizini içerisine tüm config dosyalarını kaydederler. Fakat containerized uygulamalarda farklı durum vardır:

- snap: /home/<user_name>/snap/<snap_app_name>/<app_version>/

- Portableapps.com: portableappname/Data/ dizinini kullanıyor.

- Appimage: executable dosya ile aynı dizinde ve aynı isimde suffix'i ".home" olan dizini kullanıyor. eğer yok ise normal sistemdeki $HOME dizinini kullanıyor.

- Nix: normal uygulamalar gibi sistemdeki $HOME dizinini kullanıyor.

- Flatpak: normal uygulamalar gibi sistemdeki $HOME dizinini kullanıyor.

- Docker: container içindeki /root dizinini kullanıyor.

Snap uygulamaları home dizininde şöyle seçeneklerde sunuyor:

- /home/<user_name>/snap/<snap_app_name>/<app_version>/ --> burası app_version sürümü için home dizini

- /home/<user_name>/snap/<snap_app_name>/common/ --> tüm sürümler için ortak olan home dizini

- /home/<user_name>/snap/<snap_app_name>/current/ --> bu bir link. yüklü olan (enable olan) sürümün home dizinine link ediyor.

Snap uygulamaları home dizini gibi işletim sistemi root dizinini altında /var/snap/ dizininde de her uygulama için read-write yetki olacak şekilde dizin yapısı kurmuş durumda. burası tüm OS user'ları için ortak config'leri barındırıyor.

# containerized apps dizin yapısı

- Portableapps.com: /<APP-NAME\>/App/

- Appimage: kendisi zaten tek bir (portable) dosya.

- Docker: o anda Docker runtime'ı, dizinleri container'a mount ediyor.

- Nix: işletim sistemi root dizini altında tek bir dizinde tüm dosyaları toplanmış durumda:

```
--/nix/

------/store/

------------/<HASH_OF_THIS_FILE_OR_DIR>-<PACKAGE_NAME>-<APP_VERSION>/ --> Burası dizin de olabilir sadece bir dosyada. her dependency içinde kendi dependency'lerini barındırmıyor. tüm dependency'ler de nix/store/ içinde.

------/var/

----------/profiles/

-------------------/<PROFILE_NAME_1>/

-------------------/<PROFILE_NAME_2>/

------------------------------------/bin/
```

Her user bir profil'e bağlanır. Her profil ise kendi içinde, Nix-store'da yüklü olan aynı uygulamanın farklı sürümlerine bağlanır. Bu sürümlerin komut satırı executable'ları /<PROFILE_NAME_2>/bin/ içinde yer alıyorlar.

Dolayısı ile OS içinde yeni bir user'ın $PATH'ine /<PROFILE_NAME_2>/bin/ dizinini eklemek yeterlidir. artık tüm uygulamalar kullanılabilir olur.

$HOME/.nix-profile --> bu dizin buraya link ediyor: /nix/var/nix/profiles/<PROFILE_NAME_X>

- Snap

işletim sistemi root içerisinde birçok dosyayı barındırıyor. fakat ekstradan işletim sistemi ile entegreli çalıştığından her tarafta sembolik linkler ve config dosyaları da barındırıyor.

```
--/snap/

-------/bin/ >-> executables of installed apps

-------/<APP_NAME>/

------------------/<VERSION>/

------------------/current/ ->link to enabled version
```

# Nix vs Docker

- Nix namespace'leri kullanmaz. programlar normal masaüstü uygulaması gibi açılır. Nix ile açılan uygulamada namespace'leri ayırmak istersek önüne firejail gibi uygulamalar kullanmamız gereklidir.

- Nix herhangi bir package install ederken programı baştan lokalde build eder ve mount-namespace'lerinden ve cgroups'tan yararlanır. onun dışında runtime'da özel bir namespace teknolojisini kullanmaz.

- Nix desktop (GUI) uygulamalarda da kullanılabilmesi daha kolaydır.

- Nix tamamen portable bir dizindedir. isteğe bağlı runtime servisi (daemon) açılabilir.

- Nix update/değişiklik sonrası rollback işlemleri kolaydır. çünkü grupça tüm paketleri rollback yapabilir. Git'teki gibi geri ileri alınabilir. Docker'da bunu yapabilmek için Docker'ın önünde Kubernetes gibi yazılımlar kullanmak gerekir.

- Dockerfile'de "apt-get update" işlemi yapıldığında paket indirme işleminin hangi paketleri indirdiği net değildir. oysa nix'te çok fazla paket detayına tek tek inilebilir. hem versiyon hemde SHA hem de her paket için ayrı ayrı repo verilebilir.

- NixOS sayesinde kernel modülü gerektiren uygulamalarda çalıştırılabilir. örnek VirtualBox NixOS'ta sorunsuz çalışır.

# (Appimage or Chown) vs Firejail vs Snap vs container
container tüm kaynakları ilgili process için yeni yaratır. Sadece istediğimiz kaynakları process'in görmesini sağlarız (ayarlamamız gerekir).

Appimage ve Chown sadece başlatılan process'in mount edilen root dizinini değiştirir ("mnt namespace" teknolojisinden yararlanır). diğer tüm kaynaklar OS'taki diğer process ile aynıdır.

Snap ise bazı kaynakların (yarı yarıya diyebiliriz) yeni yaratılmasını sağlayarak process'i yürütür.

firejail ise var olan process'i hiçbir yeni kaynak yaratmadan başlatır fakat çok basitleştirilmiş komut satırı parametreleri ile yeni namespace'ler (kaynaklar) yaratabilmemizi sağlar.
