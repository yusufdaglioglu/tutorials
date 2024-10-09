############################

############################
# UNIX LIKES COMMANDS
############################

############################

# bazı komut satırı uygulamaları + shell builtin'ler

## which

farklı path'lerde aynı isimde birçok dosya olabilir. örneğin; "which vlc" bize hangi dizindeki vlc'yi execute edeceğini gösterir.

## where

parametre aldığımı komutun nerelerde olduğunu listeler. which tek bir dizin dönerken, where birden fazla dönebilir.

## eval
eval fonksiyonu aldığı string parametrelerini shell'de execute eder. string parametrelerini direk olarak komut satırında da işletebiliriz. fakat bazı istisna durumlarda eval şart olabiliyor. örneğin gelen komutu hem ekrana bastırıp hemde execute etmek istediniz. string olarak gelen komut bu ise:

```sh
DYNAMIC_COMMAND_STRING="docker stop $(docker ps -a -q)"

eval $DYNAMIC_COMMAND_STRING # sağlıklı dönüş yapacak.

$DYNAMIC_COMMAND_STRING # oysa komutu bu şekilde işletmek hataya sebep olacaktır.
```

## echo

parametre aldığı string'i stdout'a yollar.

echo -e "hello \n \a" 'da bulunan -e parametresi stdout'a yollanacak string parametresindeki \n ve \a nın özel karakterler olduğunun algılanmasını sağlar. yani; \n satır sonu \a ise bip sesi çıkaracaktır. özel karakterlerin tüm listesi:

```
\a      alert (bell)
\b      backspace
\c      suppress further output
\e      escape character
\f      form feed
\n      new line
\r      carriage return
\t      horizontal tab
\v      vertical tab
\\      backslash
\0nnn   the character whose ASCII code is NNN (octal).  NNN can be 0 to 3 octal digits
\xHH    the eight-bit character whose value is HH (hexadecimal).  HH can be one or two hex digits
```

-E parametresi, escape karakteri --> \ ile belirtilen string'lerin özel karakterler olarak algılanmamasını sağlar. -e'nin zıttıdır.

-n parametresi echo komutunun sonuna satır sonu karakteri eklememesini sağlar.

echo çoğu sistemde built-in komut'tur. -n ve -e parametreleri çoğu komut satırı interpreter'ında default olarak zaten echo komutuna yollanır.

## printf
'print formatted'ın kısaltmasıdır.

parametre aldığı string'i stdout'a yollar. echo'ya göre ektra çok fazla özellik sunuyor. Aynı zamanda "echo" sistemden sisteme çok değişkenlik gösterebiliyor. oysa printf, %99 tüm sistemlerde aynı çalışıyor.

kullandığı format, C dilindeki printf ile aynı.

## pwd

(Print Working Directory) Bulunduğunuz dizinin ismini verir.

## hostname

Makinanın ismini verir.

## whoami

(Who Am I?) Sisteme giriş yaparken yazdığınız kullanıcı isminizi verir.

## uptime

Makinanın ne kadar süredir açık olduğu bilgisini verir.

## mkdir abc

(Make Directory) bulunduğu dizinde abc isminde klasör oluşturur

## grep xyz abc

abc dosyasındaki, xyz string'inin olduğu satırları dondurur

## ls

bulunduğu dizindeki dosyaları listeler

## top

çalışmakta olan process'lerin detaylı listesi

## history

komut satırında daha önce verilen komutların listesini çıkartır.

## cat dosya.txt

prints the file

## nano file.txt

basit text editor

## VIM

__vi__ editoründen esinlenerek yapıldığı için bu ismi almıştır.

__gvim__; GTK penceresi içine gömülmüş bir terminalde VIM çalıştırılmaktadır. ek olarak toolbar butonları bazı komutları yerine getirmek amaçlı sunmaktadır. bu tarz birçok VIM fork'u mevcuttur.

__vim-gnome__ ve __vim-gtk__ farklı bağımlılıklarla derlenen iki benzer fakat gvim tarzı çatallardır.

__vim-tiny__ VIM'i tüm özelliklerini barındırmayan bir fork'udur. komut satırından __vi__ yada __vim.tiny__ olarak çağrılır. Ubuntu ve türevlerinde bu paket yüklü gelmektedir.

VIM'in birçok modu var:
- insert mode
- normal mode
- visual mode

VIM'in hangi modda olduğunu en aşağıdaki panelde yazmaktadır. VIM ilk açıldığında insert modda başlar. insert modda klavyeden girilenler, dosyayı düzenlemek için kullanılır.

ESC tuşu ile normal moda geçiş yapılır.

Normal moddayken de sağa sola gidilebilir. bunun için sağa sol yerine, h j k l tuşları (or farklı tuşlar konfigüre edilebilir) kullanılabilir.

normal modda, 4 karakter sağa gidilecek ise; 4 ardından da "l" karakterine basılır.

normal modda sırası ile c,4,l tuşlarına basıldığında ise 4 karakter sağa gider, ardından da otomatik insert moda geçeriz. c karakteri bunu yapar.

normal moddayken insert mode'a geçiş yapmak için "i" tuşuna basmalıyız. eğer a tuşuna basarsak bizi insert tuşuna alır fakat imleci bulunan karakterin sağına atar.

normal moddayken komut girebiliriz. bunun için normal moddayken yada insert moddayken : tuşuna basmalıyız. ardından da komutumuzu gireriz. örnek:

> :q

VIM'i kapatır.

> :q!

değişiklikleri kaydetmeden kapatır

> :wq

değişiklikleri kaydederek çıkar.

> :w

sadece değişiklikleri kaydeder. VIM'i kapatmaz.

visual mode'a geçiş yapabilmek için v tuşuna basılmalıdır. v tuşuna basıldıktan sonra karakter grubu seçimi yapabiliriz. burada hem mouse supportu var, hemde yine "ghjk" karakterleri ile sağa sola gidip seçim yapabiliyoruz. 4 ardından da "l" tuşuna basarsak, 4 karakter sağa doğru seçim yapılmış olur gibi...

## Neovim

komut satırından __nvim__ ile çağrılır. VIM fork'udur. çok gelişmiş özelliklere sahiptir.

## Emacs

bu terim; genişletilebilir komut satırı text editörlerine verilene genel bir isimdir. tek başına bir uygulama değildir. mutlaka kendini Emacs olarak belirten bir türevini kurmak gereklidir:
- GNU Emacs
- XEmacs (or old-name:Lucid EMacs)
- remacs

komut satırından "emacs" diye çağrıldığında sistemde yüklü olan bir Emacs editorü (yukarıda yazanlardan biri) çağrılır.

Emacs, VIM'e göre çok daha gelişmiş bir yapıya sahiptir. fakat VIM'e göre konfigürasyonu daha zordur.

## more dosya.txt

prints the file. fakat ek olarak komut satırı ekrana sığmayan kısımları boşluk tuşuna basarak devamlı parça parça okumamıza olanak verir.

## head dosya.txt

print some of first lines of file.

## tail dosya.txt

prints some of last lines of file.

## head -n 5 dosya.txt

prints the first 5 lines

## tail -n 25 dosya.txt

prints the last 5 lines

## ls | grep abc

(| işareti kendinden önceki komutun çıktısını sonrakine girdi olarak aktarır.)
grep komutu; ls'nin output'unda sadece "abc" içeren satırları döndürür.

## ls | more

ekrana sığacak şekilde dosya isimlerini listeler. sığmayanları boşluk tuşuna basarak görebiliriz.

## less

more ile aynı görevi görüyor. less daha eski yılların bir projesi.

## netstat

socket listeleme programı

## nc
açılımı: netcat.

komut satırı uygulaması ile bir porttan direk TCP soketi açılaması, oradan data atılıp alınması gibi hızlı network işlemleri yapmamızı sağlıyor.

## cat

dosya okuma, dosyayı başka bir dosyanın sonuna direk ekleme gibi işlemler sağlar

## ln

- kısayol oluşturur.
- "ln -s" parametresi symbolic link (or soft link) oluşturur.
- "-s" parametresi geçilmezse hard link oluşturur.
- POSIX'teki dosya sistemleri her 2 türdeki link çeşidini de destekler.
- soft link, dosyanın path'ine işaret ediyor. dolayısı ile; link ettiğimiz dosyanın adı/path'i değişirse, linkimiz artık çalışmaz hale gelir.
- hard link ise; dosyanın içeriğine direk link ediyor. dosya ismi değişirse, linkimiz hala doğru yere referans etmeye devam eder.

  (dosyanın içeriği ile dosyanın adresi farklı göstergelerdir)

## man

   man vlc

   aldığı ilk parametre farklı bir programdır. ilgili programın manuel page'sini gösteriyor.

   POSIX'lerde her uygulama kurulduğunda manuel dosyalarını /usr/share/man/tr/i386 (or /usr/local/man) gibi dizinlere atar. bu dizinler OS'tan OS'a değişiklik göstermektedir.

   man programı bu dizinleri kurcalar ve bize ilgili programın manuel'ini gösterir.

   manuel dosyalarında şu şekilde bir tanım görürüz:

   > command [ A ] file ...

   - A burada köşeli parantez içinde olduğu için opsiyonel parametre olduğunu temsil eder.

   - file ise zorunlu bir parametredir.

   - üç okta ondan bir önceki parametreden sonsuz adet alabildiğini belirtir. yani file1'in yanına; file2, file3 şeklinde dilediğimiz kadar dosya atabiliriz.

   ## section

   man sadece komutların doc'larını değil, system call gibi birçok fonksiyonunda doc'unu göstermektedir. bunlar bazı kategorilere ayrılmış durumda. kategori listesi bu şekilde:

   - 1 - User commands (Programs)

     Those commands that can be executed by the user from within a shell.

   - 2 - System calls

     Those functions which wrap operations performed by the kernel.

   - 3 - Library calls

     All library functions excluding the system call wrappers (Most of the libc functions).

   - 4 - Special files (devices)

     Files found in /dev which allow to access to devices through the kernel.

   - 5 - File formats and configuration files

     Describes various human-readable file formats and configuration files.

   - 6 - Games

     Games and funny little programs available on the system.

   - 7 - Overview, conventions, and miscellaneous

     Overviews or descriptions of various topics, conventions and protocols, character set  standards, the standard filesystem layout, and miscellaneous other things.

   - 8 - System management commands

     Commands like mount(8), many of which only root can execute.

   örnek:

   "man 1 read" komutu read komutunun doc'unu gösterecektir. fakat "man 2 read" read system call'unun doc'unu gösterecektir.

   örnek2:

   (source-id: 314) sayfası bize read'in system call'unun doc'unu gösteriyor.

## httpie

   wget ve curl'a alternatif modern bir komut satırı HTTPclient uygulaması. modern uygulamaların stres testlerinde dahi kullanılabilir. komut satırından "http" olarak çağrılmaktadır.

## http-prompt

   httpie kullanan fakat komut satırında otomatik tamamlama sunan bir komut satırı uygulamasıdır. interaktif çalışır. komut satırına "http-prompt" yazıldığında interaktif moda geçer.

## ldd

   List Dynamic Dependencies'in kısaltmasıdır.

   "ldd executable" komutu ile "lib.so" dosyasının link ettiği diğer bağımlılıklarını görüyoruz. bu şekilde programa tanıtacağımız lib dosyasının sistemimizde ihtiyaç duyduğu bağımlılıkları görebiliriz.

## update-alternatives

   /etc/alternatives/ ve /var/lib/alternatives/ dizinlerini düzenler. bu dizinde her amaç için (örnek x-www-browser, editor) hangi programın işletim sisteminde varsayılan olarak açılacağı bilgisi vardır. örneğin /etc/alternatives/java bir symbolic linktir ve Java'nın executable dosyasına link eder.

## dd

dd komutu cp ile yapılabilecek herşeyi yapar fakat ekstrasında bazı ek özellikler sunar. sunduğu özellikler özellikle disklere (partitionlara) yazıp okumayı kolaylaştırmaktadır. partitionlar ve diskler Linux'ta birer dosya olduğu için "cp" komutu ile de aynı işler yapılabilir. fakat dd bu iş için biçilmiş kaftandır.

ISO formatı partition bilgilerini de tutmaktadır. bir red-hat Linux ISO'sunu direk dd ile çok basit şekilde bir device'a (USB'ye) aktarabiliriz. artık bu USB bootable bir OS olacaktır.

## basename

> basename '/home/user/Desktop/todo.txt'

dosya adını, örnekte; 'todo.txt'yi output'a yazar.

## dirname

> dirname '/home/user/Desktop/todo.txt'

dosya path'ini, örnekte; ''/home/user/Desktop'u output'a yazar.

## 7z

- __7-zip__

  Microsoft Windows için dağıtılan official paketin ismidir.

- __7-Zip for Linux/macOS__ 

  sadece __7-zip__ diyince Windows build kastediliyor. Oysa __7-Zip for Linux/macOS__ resmi 7-zip geliştiricilerinin MacOS ve Linux için geliştirdikleri 7-zip paketidir.
  
  7zip ilk çıktığında sadece Windows için çıkmıştı. Bu sebeple, __p7zip__ projesi Linux için fork olarak çıkarıldı. Ama artık 7-Zip resmi olarak Linux ve MacOS desteklediği için __p7zip__ güncelleme almamaktadır.

- __p7zip__

  7-Zip geliştiricilerinden bağımsız sunulan ve sadece POSIX'ler için geliştirilen paketin ismidir.

  __p7zip__, __7-Zip__'nin resmi sitesinde ayrı bir başlıkta referans olarak gösterilmektedir.

- __p7zip-full__ vs __p7zip__

  İki pakette independed developer'ların çıkardığı forkun linux-installer isimleridir. Artık bu paketler güncelleme almamaktadır.

  https://packages.debian.org/stable/p7zip-full

  https://packages.debian.org/stable/p7zip

  Yukarıdaki kaynaklardan alıntılar:

  > p7zip, provides 7zr, a light version of 7za, and p7zip, a gzip-like wrapper around 7zr.

  > p7zip-full, provides 7z and 7za which support more compression formats.

- lisans

  Orjinal 7zip paketi un-free olarak sınıflandırılır. çünkü unRAR kodu içermektedir.

  Resmi bir kaynak bulamadım ama bi yerde Debian paketinin free olabilmesi için "rar" kodunun silindiği yazmaktadır. Bu sebeple Debian'deki bazı paketler "free" sınıfında olabilir.

- 7-Zip'in içerisinde birçok binary vardır:
  
  - __7z__

    (binary/executable dosya) kendisiyle aynı dizinde olması beklenen Microsoft Windows'ta 7z.dll'i (Linux'ta 7z.so'yu) kullanılır.

    __7z.dll (Linux'ta 7z.so)__ asıl işi (7z engine) yaparken, __7z__ binary'si sadece komut satırı API'si sunar.
    
    7zip'in GUI'si, 7z'yi değil, 7z.dll (Linux'ta: 7z.so)'i kullanır.

  - __7zG__ ve __7zFM__
  
    binary'leri sadece Microsoft Windows için GUI sunmaktadır.

    __7zFM__ file-manager olarak açılan GUI kodlarını içeriyor.

    __7zG__ yine komut satırı 7Z gibi aynı parametreler ile çağrılıyor fakat konsole ouput yerine GUI açarak işlemleri gösteriyor (örneğin progress bar).
  
  - __7z.sfx__
  
    __self-extracting archive (or SFX or SEA)__ module only for Microsoft Windows.
  
  - __7zCon.sfx__
  
    console version on SFX module.
  
  - __7za.exe__
  
    a: alone

    standalone console version of 7-Zip. sadece console API'si sunmaktadır.
  
    __7za__ does not support same features as __7z__.

  - __7za.dll__
  
    same logic as __7za.exe__ but it is DLL.
  
  - __7zxa.dll__
  
    a: alone

    x: extract

    library only for extracting archives (no other dependency needed for this executable)

- __7-Zip Extra__

  paketi, __7-Zip__ geliştiricilerinin dağıttığı özel bir pakettir.
  
  GUI içermez ve sadece Microsoft Windows için çalışır.

  Hem DLL hem binary barındırır. Ek olarak 3üncü parti yazılım eklentilerini de barındırır.

## awk

girdi olarak multiline bir string alır ve her satırını ekrana nasıl basacağını parametre olarak alır.

örnek:

İlgili dosyanın her satırını ekrana basar:

> awk '{print $0}' myfile

- $0 for the whole line.
- $1 for the first field.
- $2 for the second field.
- ...

Her satır; "RS" yani; "input-record separator" ile birbirinden ayrılıyor. Her satır içindeki field'lar ise; "FS" yani;"the input-field separator" birbirinden ayrılıyor. Bu ayraçları override edebiliriz.

örnek:

> ss --all --processes --tcp --udp | awk -F ' ' '{print $1"\t"$2"\t"$5"\t"$6"\t"$7}'

## cd abc

(Change Directory) abc path'ine gider. cd bir komut satırı uygulaması değildir. komut satırı uygulamasının bir özelliğidir.

## pushd ve popd

cd'ye alternatif komut satırı uygulamalarıdır. cd'ye göre akışta ince detaylar fark etmektedir. fakat "cd" built-in oluğundan bu davranışları kesin olarak standarda dökmek yanlıştır. internette bir kaynağa göre davranışlar şu şekilde fark etmektedir:

kaynak: (source-id: 315) "Adaephon" answer.

```
cd somedir
    change directory to somedir
    save the original directory in OLDPWD
    set PWD="somedir"
    replace top element of the directory stack (as shown by dirs) with somedir (the number of elements on the stack does not change).
cd -:
    change directory to $OLDPWD
    swap values of PWD and OLDPWD
    modify the top element of the directory stack to reflect (the new) PWD
pushd somedir:
    change directory to somedir
    save original directory in OLDPWD
    set PWD="somedir"
    push somedir onto the directory stack (extending it by one element)
popd:
    save original directory in OLDPWD
    remove first element of the directory stack
    change directory to the new top element of the directory stack
    set PWD to the new top element of the directory stack
```

## alias

komut satırı dünyasında alias bir komutun yerine kullanılan keyword'dür. örneğin;

```sh
alias clr="clear"
```

komutu ile artık istediğimiz zaman "clr" komutu ile "clear" komutunu çalıştırmış oluruz.

```sh
unalias clr
```

komutu ile alias silinir.

# script
"script" komutu çağrıldıktan sonraki komutların ve çıktıların hepsini "exit" komutu çağrılana kadar kaydeder ve bir dosyaya aktarır. örnek:

```sh
script "$HOME/records1.txt"
echo "hello"
echo "world"
exit
```

Artık "$HOME/records1.txt" u text editor ile açıp okuyabiliriz. Bu dosya direk kolay okunabilir olmayabiliyor. Bunun birçok sebebi var. Bazıları:
- Komut satırını renklendirmek için gerekli kod karakterleri varsa onlarda burada oluyor.
- Komut satırında her yeni satırda $ gibi karakterler de oluyor. Onlarda bu dosyaya yazılıyor.

# commands which start with underscore
when we type a command and then we click TAB button, the terminl shows the possible arguments which we can send to command. example Maven --> TAB --> shows "clean", "install"...

This possible variables are returning from "_maven" function.

# locale
bu komut bize OS'un default olarak hangi dil ve coğrafi özelliklerine göre hizmet verdiğini göstermektedir.

"locale" komutun çıktısı aşağıdaki gibidir:

```sh
LANG="en_US.UTF-8" # Eğer bir değişkenin değeri yok ise (örnek LC_MONETARY boş ise), "LANG" değeri ise default değer olarak kabul edilir.

LANGUAGE=

LC_CTYPE="en_US.UTF-8" # using for character classification and case conversion. örneğin hangi karakter, 1 letter veya işarete tekabül ediyor gibi... Küçük büyük harf çevrimlerinde kullanılıyor. Örneğin; İngilizce'de I küçük harfe çevrilirken i diye çevriliyor, oysa Türkçe'de i diye çevriliyor. Dolayısı ile bunun nasıl yapılacağına LC_CTYPE karar veriyor.

LC_NUMERIC="tr_TR.UTF-8" # Specifies the decimal delimiter (or radix character), the thousands separator, and the grouping.

LC_TIME="tr_TR.UTF-8" # date formatting

LC_COLLATE="en_US.UTF-8" # Bu değer character/string "sorting/ordering" gibi algoritmalarda kullanılıyor.

LC_MONETARY="tr_TR.UTF-8" # monetary formats, including the currency symbol for the locale, thousands separator, sign position, the number of fractional digits, and so forth.

LC_MESSAGES="en_US.UTF-8" # Specifies the language in which the localized messages are written, and affirmative and negative responses of the locale (yes and no strings and expressions).

LC_PAPER="tr_TR.UTF-8" # her coğrafyada ve dilde 1 adet sayfanın boyutları değişebiliyor. örneğin Türkiye'de sıkça A4 kullanılıyor. dolayısı ile tr_TR.UTF-8 kuralları içerisinde A4 yazmaktadır.

LC_NAME="tr_TR.UTF-8" # How names are represented (surname first or last, etc.).

LC_RESPONSE="tr_TR.UTF-8" # Determines how responses (such as Yes and No) appear in the local language. LC_MESSAGES ile çok benzer. Bazı OS'larda iri olup diğeri olmayabiliyor.

LC_ADDRESS="tr_TR.UTF-8" # LC_ADDRESS Convention used for formatting of street or postal addresses. How addresses are formatted (country first or last, where Zip code goes etc.).

LC_TELEPHONE="tr_TR.UTF-8" # telephone number format (sadece nasıl gösterileceği, +90'dan sonra boşluk mu, yoksa parantez mi var gibi)

LC_MEASUREMENT="tr_TR.UTF-8" # What units of measurement are used (feet, meters, pounds, kilos etc.).

LC_IDENTIFICATION="tr_TR.UTF-8" # Metadata about the locale information.

LC_ALL=""  # şu örnekte boş. LC_ALL diğer tüm "LC" parametrelerini override etmek için kullanılmaktadır.
```

- Yukarıda tanımlanmış bu değerler shell'e ve diğer uygulamalara environment parametresi olarak geçilmektedir.

- "LC_MONETARY=tr_TR.UTF-8" değerini inceleyelim: Para birimleri için ayracın, nokta mı, virgül mü olacağına dil karar veriyor. yani "tr_TR". Fakat bu ayraçın hangi encoding ile tanımlandığı UTF-8 ile belirlenmiş.

- "locale -a" komutunun çıktısı tüm alabilecek(available) local kombinasyonları listelemektedir. Her satırı açıklaması ile görmek için bu komut çalıştırılmalıdır: "locale -v -a". Daha detaylı olarak çıktıya baktığımızda her lokalin rule'larının belli bir dosyaya gömülü olduğunu görürüz. Bu dosyalar programlar tarafından parse edilebilirdir. Örnek dosyalar:

  - locale: C.UTF-8 kuralları bu dosyalardadır: /usr/lib/locale/C.UTF-8/LC_ADDRESS, /usr/lib/locale/C.UTF-8/LC_TIME...

  - locale: tr_TR.utf8 nin kurallarının tümü (LC_ADDRESS + LC_TIME ...) bu dosyadadır: /usr/lib/locale/locale-archive

- Bazı durumlarda "tr_TR.UTF-8" yerine "POSIX" görebiliriz. Bu POSIX standartlarında belirtilen default değer anlamına gelmektedir. POSIX standartlarında ise "C" lokaline denk gelmektedir.

- "en_US.utf8" Amerika bölgesindeki İngilizce ve UTF-8 encoding'i ile birlikte kullanımını temsil ederken sadece "en_US" yazanlar farklı encoding ile oluyorlar. Hangi encoding olduğunu görmek için "locale -v -a" komutu ile incelenmelidir.

