############################

############################
# CONTAINERS AND VM
############################

############################

# Network drivers

# VirtualBox network drivers
(network interface konusu başka başlıkta anlatılıyor.)

VirtualBox network ayarlarında network bağdaştırıcısı eklerken birden fazla farklı ayar (driver) mevcuttur. bunlar:

- Not attached: network arayüzü oluşturulur fakat internet bağlantısı olmaz.

- NAT

  Burada host ve guest işletim sistemi arasında private virtual bir network oluşturulur. host bir router gibi davranır ve NAT ile istekleri makineden dışarıya çıkarır. guest'in aldığı IP, private network içinden bir IP'dir. Host dışında bu sanal makineye erişmek için port-forwarding yapmak şarttır.

- NAT Network (or NAT Service)

  NAT yapısı ile aynı çalışmaktadır. Sadece NAT yapısında tek bir makine varken (çünkü "NAT" driver sadece 1 makineli private network yaratır) burada komple bir network vardır. Dolayısı ile yine VM'e bağlanan interface bir router gibi davranıp, sanal VM'lerden oluşan bir network'ü dışarıya NAT ile çıkarmaktadır. Doğal olarak; sanal/private network'te olan VM'ler birbirlerini de görebilmektedirler.

- bridge networking

  host'un bağlı olduğu DHCP'den yeni bir gerçek IP alır. DHCP'den iki tane IP alabilmek için farklı bir cihazmış gibi davrandırtıyor. Bunu yapabilmek için; Virtualbox kendi istekleri için sadece MAC adresi değiştiriyor. bu network çeşidi bazı işletim sistemi ve farklı donanım kartları/teknolojileri için desteklenemiyor (yada yarım destekleniyor yada çok farklı/dolaylı yöntemler uygulanmak zorunda kalınabiliyor).

- Host-only networking

  sanal bir ağ kartı yaratır. bu sanal ağ kartı içerisinde istediğimiz kadar VM'i birbiri ile iletişime geçirebilir. dış dünya ile iletişimde olmazlar. sadece kendi aralarında haberleşirler. istenildiği kadar sanal ağ kartı yaratılıp, VM'ler kendi içinde gruplandırılabilir. sanal ağ kartının yanında, network'te sanal DHCP sunucuda yaratılmaktadır (VirtualBox ayarlarından).

- Internal networking

  Host-only networking yöntemi ile apaynıdır. sadece sanal bir kart yaratılmaz. sanal bir kart yaratılmaması; var olan bir fiziksel kartın kullanılmasına sebep olur. Böylece Wireshark gibi bir uygulama ile 'Internal networking'e bağlı VM'lerin network trafiğini izleyebiliriz.

- UDP Tunnel Networking

  farklı host'ların ortak network yönetimi yapılabilmesini sağlayan bir driver'dır.

- VDE Networking

  farklı host'ların ortak network yönetimi yapılabilmesini sağlayan bir driver'dır.

Virtualbox ortamında birden fazla ağ bağdaştırıcısı eklenebilir. Dolayısı ile örneğin; bir VM içerisindeki işletim sistemi hem "internal network" ile diğer VM'ler ile haberleşirken, "NAT" bağdaştırıcısı ile internete çıkabilmektedir.

# ("host-only" and "internal networking") vs ("NAT" and "NAT Network")
"host-only" and "internal networking" private network'ten dışarıya VirtualBox'un engine'inde olan sanal bir "Switch" cihazı simülasyonu ile çıkarken, "NAT" and "NAT Network" yine VirtualBox'un yarattığı sanal bir router ile dışarıya çıkar.

# Docker network drivers

- none

  internet container içerisinde tamamen kapalıdır.

- bridge

  default network type. sanal interface yaratılır. bu interface üzerinden sadece bu bridge bağlı container'lar birbirlerini görür. bu container'lar private bir sub-network içerisindedir.

  bridge'yi router gibi düşünürüz. dolayısı ile bir container'a erişmek için port-forwarding (port exposing) yapmamız şart.

- overlay

  kelime anlamı: kaplama/örtü

  bridge mantığını multi-host üzerinden yapabilmemizi sağlar. yani 2inci bir host'taki bir container, 1inci host'taki bir container ile aynı network'teymiş gibi davranabilir.

- host

  container kendi IP'sini almaz. host ile aynı network namespace'sini kullanır. yani container içerisinde çalışan process, host'ta çalışıyormuş gibi düşünebiliriz. bu sebeple "-p" ile yapılan port forwarding'lere ihtiyaç yoktur (ignore edilirler).

- macvlan

  container'a bir mac adresi atar ve host'un network'ünde gerçek IP almasını sağlar. bu genelde subnetworking üzerinden işlem yapamayan legacy uygulamalar için kullanılabilir.

  host'un bulunduğu network'ün bilgilerini vermek zorundayız.

  ```sh
  docker network create -d macvlan \
                      --subnet="172.16.86.0/24" \
                      --gateway="172.16.86.1" \
                      -o parent="eth0" \
                      "pub_net"
  ```

  parent olarak ne vereceğimiz çalışma mode'u belirleyecektir:

  - bridge mode

    direk olarak gerçek fiziksel kartımız üzerinden çalışır. bu sebeple gerçek fiziksel arayüzümüzü parent'a yazmak zorundayız.

  - 802.1q trunk bridge mode

    sanal yaratılan bir interface üzerinden fiziksel karta bağlanır. böylece host'un arayüzünde olan routing veya filter'lara takılmamış oluruz.

    parent'a "eth0.50" gibi Docker engine'in belirttiği şekilde isim vermeliyiz.

- Docker plugin'lerinde 3üncü parti network plugin'leri kurup aktif hale getirebiliriz.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Sanallaştırma kavramları

# Hypervisor (or virtual machine monitor or VMM)

sanal makine yönetimini yapan yazılımlara verilen gelen isimdir.

Hypervisor run edildiği katmana göre 2'ye ayrılır:

#### Type-1 (or native or bare-metal Hypervisors)

işletim sisteminin kurulu olmasını gerektirmez. direk donanım ile temasta olan ilk katmanda bulunur. Hypervisor üzerine bir den fazla işletim sistemi kurulur.

Örnek: Microsoft Hyper-V (Linux'u da desteklemektedir), Xen, VMware ESXi (or eski adı VMware ESX)

#### Type-2 (or hosted Hypervisors)

işletim sistemi üzerine yüklenirler ve sanal makine içindeki uygulamaları host donanımın anlayacağı dile çevirir.

örnek: VirtualBox, QEmu (or Quick Emulator)

# Donanım sanallaştırma (or platform sanallaştırma)

kurulan işletim sistemlerine donanımları paylaştırma işlemidir. Hypervisor'ler bu paylaştırma işlemini yönetir. Hypervisor'ler donanımları paylaştırma işlemlerine göre 3'e bölünürler:

- Tam Sanallaştırma: tamamiyle sanal olarak bir donanım yaratılır ve işletim sistemlerine bu donanım kullandırtılır.

- Para-virtualization: gerçek donanımlar direk olarak işletim sistemlerine bağlanır. sanal donanım hiç yoktur.

- Kısmi Sanallaştırma: para ve tam arasında olan bir mekanizmadır. işletim sistemlerine bazı donanımlar sanal verilirken, bazı donanımlar sanal yaratılıp verilir.

# KVM (or Kernel-based Virtual Machine)

özel bir isimdir. KVM Linux modülüdür, Hypervisor'dür. işletim sistemi modülü olduğu için type-1 veya 2 olduğu tartışılır. sanal makinede olan işlemleri host'un anlayacağı dile çevirmez. bu sebeple type-1 olarak algılanabilir. fakat çekirdek üzerine kurulu olduğundan type-2 gibi düşünülebilir çünkü tüm guest'ler host makinanın birer process'iymiş gibi çalışmaktadır.

# Parallels Desktop (or Parallels Desktop for Mac)

MacOS için Hypervisor.

# yazılım sanallaştırma

Docker gibi uygulamalara verilen genel isimdir. uygulama çalıştırma seviyesinde olur, işletim sistemleri seviyesinde bir kavram değildir.

# Intel VT-x vs AMD-V

Sanallaştırma teknolojileri için işlemcilerin yapmış olduğu yeni teknoloji isimleri.

# Nested virtualization

içiçe sanallaştırma yani; sanal makine içinde sanal makine çalıştırmaktır.

# VMware ThinApp

VMware'in uygulaması, yazılım Sanallaştırması yapar. İşletim sisteminde bir uygulamanın sanal olarak yürütülmesini sağlar. var olan işletim sisteminin dosyalarını elleyemezler.

# Emülatör (or öykünücü or emulator) vs simulator (or simülatör)

emülatör gerçek sistemi bir sandbox'ta çalıştırıp sunarken, simülatör gerçek sisteme en yakın (benzeri) modeli sandbox'ta sunmaya çalışır. örnekler:

- para-virtualization, emülatör yada simülatör değildir. çünkü donanımı direk olarak yazılıma verir ve üzerindeki sistemler sandbox'ta çalıştırılmaz. sadece işletim sistemleri birbirinden bağımsız (birbirini görme yetkisi olmadan) çalışır.

- full ve partirial virtualization emülatördür. Android SDK'sında gelen Android sanal makineleri bu gruba girer. VirtualBox bu gruba girer. guest sistemler gerçek setup (örneğin ISO) dosyalarından kurulur.

- eğitimlerde kullanılan uçak yada araba oyunları birer simülatördür.

# VirtualBox OSE (or VirtualBox open source edition)

Virtualbox 4üncü sürümden önce kapalı kaynak ve açık kaynaklı sürüm olarak dağıtılıyordu. 4 üncü sürüm ile birlikte ikisi birleştirildi. ve lisans gerektiren ek özellikler; kapalı kaynak eklentiler olarak indirilebiliyor.

# VMware

VMware'in birçok farklı ürünü mevcut. Tüm ürünler kapalı kaynaktır.

# VMware Workstation (or VMware Workstation Pro)

Sanal makina çalıştırıcısıdır. lisans gerektiren versiyon.

# VMware Workstation Player (or VMware Player)

Kişisel kullanım için ücretiz sunuluyor. VMware Workstation'dan farklı bir executable olup, eksik özellikler içeriyor.

# VMware Fusion

Mac'in altyapısı farklı olduğundan, MacOS versiyonu farklı build üretilmiştir.

# Virtual Machine Manager (or virt-manager)
Genel bir terim gibi görünse de özel bir uygulama ismi. Gnome-boxes gibi sadece GUI'dir.

gnome-boxes son kullanıcı için çok kolaylaştırılmış bir önyüz uygulamasıdır. profösyönel kullanım için mutlaka virt-manager kullanılmalıdır.

# Gnome-boxes
Gnome projesi altında dağıtılan sanal makina yöneticisi için GUI'dir. Arkaplanda kendi teknolojilerini kullanmaz. Sadece GUI ile sanal makine yönetim (oluşturma, silme, açma...) işlerini kolaylaştırır.

# Vagrant

Virtualbox, VMWare gibi uygulamaların içerisinde olan makinaları, Docker tarzı yönetimini sağlayan açık kaynaklı bir komut satırı uygulamasıdır.

Örneğin; Virtualbox üzerinde çalışan Microsoft Windows-guest içerisinde komut çalıştırmamızı, diskin image'ini hazır indirmemizi, gibi işlemleri kolayca yapabilmemizi sağlar. BUnları yaparken VMWare ile de yapabilmemizi sağladığı için tek bir interface aracılığı ile yönetebilmemizi sağlar.

işletim sistemini hazır bulut görüntülerden indirip açar, run edip, bir user tanımlar, bu user'ın içine SSH atar ve SSH komutu ile içine girip istediğimizi çalıştırabilmemizi sağlar.

# chroot
teknoloji bazında alternatifi: __systemd-nspawn__

UNIX-like sistemlerde bir dizini root olarak belirleyip, o dizin içerisinde bir uygulamayı run etmeye yarayan programdır. run edilen uygulama o dizini root olarak görür ve üst dizinleri kullanamaz. bu durum run edilen uygulamayı hapse atmaya benzetilebilir. fakat pratikte; bir uygulama birçok kütüphaneye, dosyayı kullanır. böyle durumlar için uygulamayı run etmeden önce, istenilen dizinleri, chroot altındaki dizinlere mount etmek gerekir.

bir uygulamayı execute ederken, sanal dizin altında olabilmesini sağlayan bir komuttur. Chroot'un Sanallaştırma gibi bir amacı yoktur.

"chroot" ile önce boş bir dizin yaratılır. artık burası koşacak uygulamanın root dizinidir. buralara artık bağımlılıkları da atmak gereklidir. sadece bağımlılıklar değil, POSIX'te herşey dosya olduğundan stream out in, gibi dosyaları da kaydetmek gereklidir. tüm bir işletim sistemini hazırlamak gereklidir. bu elle zor olduğundan genelde chroot türevi  hazır paket yazılımlardan yararlanılır. yada var olan sistemdeki dizinler direk olarak chroot'un root dizini altındaki yerlere mount edilir. bu şekilde örneğin sistemimizdeki /etc dizinini komple chroot içine bağlamış oluruz.

# container vs VM
container işletim sistemi çekirdeği içermez. VM içerir.

# app container vs system container (or Operating System Container)

  - ## terminoloji
    - app container sadece bir process içerir. system container ise birden fazla process içerir. kaynak: (source-id: 241) "System Containers" başlığı 1inci paragraf.

    - "__System container__" terimi redhat tarafından multiprocess dışında bir anlamda kullanılmaktadır. kaynak: (source-id: 242) "System containers provide a way to containerize services that need to run before the Docker daemon is running".

    - __system container__' ile "__Operating System Container__" eş anlamda kullanılır. kaynak: (source-id: 243) 1inci paragraf, ilk cümle.

    - "__Operating System Container__" terimi pek çok kaynakta yer almaz. genelde "__system container__" terimi tercih edilir. "__Operating System Container__" multiprocess içeren container'lardır. kaynak: (source-id: 244) "Operating System Containers" başlığı.

  - ## neden birden fazla process'e ihtiyaç var
    system container'ın birden fazla process içermesinin sebebi; işletim sistemi çekirdeği haricinde tüm dependency'lerini (sadece kütüphane (dosya) değil, process dependency'leri de) kendi container'ı içinde bulundurur. yani birden fazla process içerirler (örnek systemd, SysVinit, Upstart, OpenRC...).

    multiprocess açmaya ihtiyaç olması için örnek process'ler:

    - SSH server

      kaynak: (source-id: 243) 2inci paragraf.

    - crond

      kaynak: (source-id: 243) 2inci paragraf.

    - Syslogd

      kaynak: (source-id: 243) 2inci paragraf.

    - sidecar pattern'i uygulayabilmemiz için, birden fazla process aynı container'da çalıştırılabilir.

      kaynak: (source-id: 248)

    - eski bir uygulamayı container'da çalıştırmak istediğimizde onun dependency'lerini de çalıştırmak zorunda kalabiliriz (bu madde yukarıdaki Syslogd ile aynı şeye denk geliyor)

    özellikle multiprocess içeren container üzerine uzmanlaşmış platformlar: BSD jails, Linux vServer, Solaris Zones, OpenVZ/Virtuozzo, LXC/LXD kaynak: (source-id: 243) son paragraf.

# VM vs container runtime (optionally with container engine)
karşılaştırılmaları doğru değil. biri VM container'ı, diğeri uygulama container'ı.

- VM'in açılması dakikalar alırken, container runtime saniyeler alır.

- VM fully portable'dır. fakat container runtime değildir. container runtime işletim sistemini sanallaştırmıyor. var olan OS çekirdeğini kullanıyor. bu sebeple başka makinaya atılan container, aşağıdaki durumlarda farklı tepkiler verebilir:

  - OS çekirdeği farklı sürüm ise
  - OS çekirdeği farklı ise
  - çekirdeğin bazı özellikleri aktif ise, başka makinada aynı özellikler aktif olmayabilir (örnek Linux kernel module)
  - donanım farklı ise (özellikle donanıma bağlı kodlarda) (bu durum kısmen VM'de de var)

# container runtime
- LXC
- Libcontainer
- Runc
- Crun
- Railcar
- Katacontainers
- Containerd (Runc'yi wrap ediyor/kullanıyor.) ("__ctr__" komut satırı client uygulamasıdır.)

yukarıdaki liste için kaynak: (source-id: 244) "Container Runtime" başlığı.

Birçok container runtime "__Open Containers Initiative (or OCI)__" standartlarına uyar. OCI'nin referans implementasyonu Runc'dir.

Tüm container runtime'ler, Linux'un içinde native gelen "cgroups" ve "Linux namespaces" özelliklerini kullanarak çalışır. Ek olarak SELinux ve AppArmor gibi özelliklerden de sıkça yararlanır.

# Container engines
LXC gibi özelliklerin son kullanıcı tarafından direk kullanılması zordur çünkü alt seviyelidir. Bu yüzden Docker ve LXD gbi projeler ortaya çıkmıştır.

| container engine name | company                           | based on container runtime                                         |
|-----------------------|-----------------------------------|--------------------------------------------------------------------|
| Docker                | Docker Inc                        | Sırası ile bu teknolojiler kullanıldı: 1-LXC 2-Libcontainer 3-Runc |
| LXD                   | canonical                         | lxc(Linux containers) developing by Canonical                      |
| Rkt (or Rocket)       | Red Hat (CoreOS Team)             |                                                                    |
| CRI-O                 | Cloud Native Computing Foundation | Runc                                                               |
| Podman                |                                   | Runc                                                               |

yukarıdaki liste için kaynak: (source-id: 244) "Container Engine" 1inci paragraf.

# Container Orchestration
container engine'leri wrap eder ve sayı olarak fazla/karmaşık olan systemimizi daha rahat düzenlememizi sağlar.

- Kubernetes
- Istio (based on Kubernetes)
- OpenShift (based on Kubernetes)
- Swarm (develop by Docker Inc)
- UCP (or Universal Control Plane) (develop by Docker Inc)
- Mesos (develop by Apache)

yukarıdaki liste için kaynak: (source-id: 244) "Container Orchestration" başlığı son paragraf.

# container runtime vs container engine
aşağıdaki tüm bilgiler buradan kopyalanmıştır: (source-id: 244) "Container Runtime" başlığı ve "Container Engine" başlığı.

The container runtime is responsible for:

- Consuming the container mount point provided by the Container Engine (can also be a plain directory for testing)
- Consuming the container metadata provided by the Container Engine (can be a also be a manually crafted config.json for testing)
- Communicating with the kernel to start containerized processes (clone system call)
- Setting up cgroups
- Setting up SELinux Policy
- Setting up App Armor rules

the container engine is responsible:

- Handling user input
- Handling input over an API often from a Container Orchestrator
- Pulling the Container Images from the Registry Server
- Expanding decompressing and expanding the container image on disk using a Graph Driver (block, or file depending on driver)
- Preparing a container mount point, typically on copy-on-write storage (again block or file depending on driver)
- Preparing the metadata which will be passed to the container Container Runtime to start the Container correctly
  - Using some defaults from the container image (ex.ArchX86)
  - Using user input to override defaults in the container image (ex. CMD, ENTRYPOINT)
  - Using defaults specified by the container image (ex. SECCOM rules)
- Calling the Container Runtime

# Podman
Pod Manager'in kısaltmasından özel isim türetilmiştir.

Podman; resmi sitesinden "daemonless container engine" olarak geçiyor. çünkü bir engine'e ihtiyacı yok. zira kendisi (sadece client komut satırı uygulaması) Linux'un içerisinde gelen teknolojileri kullanıyor. Dolayısı ile ek kurulumda gerektirmiyor.

root yetkisiz de process açabiliyor (tabi kısıtlı özellikler sunarak).

Podman kendi içinde, Kubernetes gibi, "pod" ve "container" olarak 2 farklı seçeneği de destekliyor.

# cgroups (or control groups)
Linux üzerinde proseslerin CPU, memory, network gibi kaynaklardan ne kadar öncelikli yararlanabileceği, yada limitli yararlanabilmeleri için yönetimi sağlayan Linux çekirdeğinin native özelliğidir.

# Linux namespaces
bazı kaynaklarda sadece 'namespace' olarak geçer, bu sebeple karışıklığa sebep olabilir, zira 'namespace' kelimesi birçok farklı alanda kullanılmaktadır.

Linux üzerinde proseslerin hangi kaynakları diğer prosesler ile ortak kullanıp kullanamayacağını belirleyen native Linux özelliğidir. örneğin 'namespace type'lar;

- Mount: bir grup proses, spesifik bir dizini görebilirken, diğer hiçbir proses göremeyebilir.

- Network: birden fazla network arayüzü (eth, Wi-Fi..) birden fazla network namespace'sine bağlanır. network namespace'si iptable gibi routing ayarlarını içerir.

- IPC: IPC namespace'leri vardır. bir IPC namespace'si içindeki diğer IPC'lere mesaj atamaz.

- PID: process ID grubu oluşturuluyor. bu şekilde, X pid namespace'indeki process'ler sadece birbirlerini görebiliyor.

- uts (or UNIX Time-Sharing): domain ve host name'leri okurken farklı gruplar yaratabiliyoruz

- User: bir process başlatıldığında o process'in kendini X user'ı ve Y grubu altındaymış gibi sanmasını sağlayabiliyoruz.

- time: her farklı time nameSpace'inde olan process'ler sistem saatini farklı algılıyor. (bu namespace tipi 2020 ile Linux 5.6'ya geldi)

- cgroup: bu kısıtlama yapmaya yarayan cgroup özelliği değildir. bu bir namespace'dir. çalışacak process'in kendini farklı bir cgroup kısıtlaması altında olduğunu sanmasını sağlayabiliriz.

# Linux namespace'ler nasıl çalışır?

> /proc/<PROCESS_ID>/ns/

dizini altında o process'in tüm namespace'leri ayrı ayrı link dosyalarıdır:

> /proc/<PROCESS_ID>/ns/ipc

> /proc/<PROCESS_ID>/ns/mnt

İşte bu dosyalar link olduğundan, örneğin X namespace'sine link eder. İşte o process'i Y namespace'sine yönlendirirsek o zaman container/sandbox yapmış oluruz.

Linux namespace'leri system çağrıları ile yapılır ve programatik olarak yapmak için sistem çağrısı yapan kütüphaneye ihtiyacımız vardır. örneğin C'de bunu yapmak için clone kullanılabilir [file:"c.md" - title:"clone"](./c.md#clone).

# sandbox vs container

container içinde uygulamalar çalıştırabilmek için (yani yeni namespace'ler yaratabilmek için) root yetkisine ihtiyaç vardır. fakat sandbox için root yetkisine gerek yoktur. örnek Docker root yetkisi ister, fakat Chromium based tarayıcılar için sandbox için user yetkisi yeterlidir.

__Sandbox__ teknolojisi sayesinde chroumium-based tarayıcılar açtığı her process'in yetkisini kısıtlar. Böylece web sayfasının render edildiği thread'deki kötü niyetli bir kod, web tarayıcısındaki bir güvenlik aığından yararlanmaya kalkarsa, Chromium'un sandbox'una takılacaktır. sandbox'u da aşabilir. fakat bu bir seviye daha zordur.

# Idle process
init process'ten tamamen farklı ve bağımsız programlardır.

CPU donanımsal mimarisi gereği boşta bekleyemez. Sürekli bir işlem yapmak zorundadır. Bu sebeple sonsuz döngülü bir program, OS tarafından arkaplanda (eğer başka bir process çalışmıyorsa) çalıştırılır. Bu process'e genel olarak "idle process" denir. Microsoft Windows dünyasında processin özel bir ismi vardır: "__System idle process (or sistem boşta işlemi)__".

Linux'ta process ID'si 0'dır.

# init process
işletim sistemi servis ve program yönetimi yazılımlarına verilen genel isimdir. sistem başlangıcında ilk başlayan işlemdir. bu sebeple __init process__ olarakta adlandırılırlar. tüm servisler ve process'lerin yönetimini sağlar. OS çekirdeği tarafından başlatılır. kaynak: (source-id: 254) 1inci paragraf.

process ID'leri 1'dir.

init process'ler bir OS içerisinde birkaç tane olabilir. bunun için bazı örnek durumlar:
- her container içerisinde init olabilir
- her OS user'ı için ayrı init olabilir. kaynak: kaynak: (source-id: 254) "NOTES" başlığı, 1inci paragraf.

Unix dağıtımlarda ilk geliştirilen init "System V" isim ile dağıtılan dağıtımda kullanılıyordu. Bunun çıkması ile birlikte birçok Linux dağıtımı dahi, buna uyumlu init sistemleri ile dağıtılıyordu. herkes kendi init'ini yazıyordu. bu sebeple bu tarz init'ler için "System V style init", "traditional init" gibi terimler kullanılır.

şu anda piyasada birçok init alternatifi mevcut: systemd, upstart...

"init" daha çok sıralı bir açılışa ağırlık verir. örneğin; update hizmeti, log servisine bağlı ise önce log servisinin başlatılması beklenir. hazır olunca update servisi başlar. oysa systemd'de işlemler paralel başlatılır. birbirlerine depend eden istekler beklemeye alınır. işlemler paralel başladığından CPU daha az boşta kalır ve daha hızlı açılış gözlenir.

systemd vs Upstart için komut satırı uygulamaları mevcut. bunların kullanım örnekleri aşağıdadır:

| Operation                          | Upstart Command                   | Systemd command                   |
|------------------------------------|-----------------------------------|-----------------------------------|
| Start service                      | start $job                        | systemctl start $unit             |
| Stop service                       | stop $job                         | systemctl stop $unit              |
| Restart service                    | restart $job                      | systemctl restart $unit           |
| See status of services             | initctl list                      | systemctl status                  |
| Check configuration is valid       | init-checkconf /tmp/foo.conf      | systemd-analyze verify $unitFile  |
| Show job environment               | initctl list-env                  | systemctl show-environment        |
| Set job environment variable       | initctl set-env foo=bar           | systemctl set-environment foo=bar |
| Remove job environment variable    | initctl unset-env foo             | systemctl unset-environment foo   |
| View job log                       | cat /var/log/upstart/$job.log     | sudo journalctl -u $unit          |
| tail -f job log                    | tail -f /var/log/upstart/$job.log | sudo journalctl -u $unit -f       |
| Show relationship between services | initctl2dot                       | systemctl list-dependencies --all |

not: "service" komutu bir shell script dosyasıdır ve init process türünü bilmeden işlem yapabilmemize olanak sağlayan bir wrapperdır.

Systemd servisleri başlatırken hangi servisleri başlatacağına target'lar aracılığı ile karar verir. örneğin sunucumuzda grafik arayüzü hiç kurulmamış ise, daha sonra GUI kurarsak ve varsayılan olarak GUI servislerinin açılmasını istiyorsak, default target'ı değiştirmeliyiz:

> systemctl set-default graphical.target

output:

```
Removed symlink /etc/systemd/system/default.target.
Created symlink from /etc/systemd/system/default.target to /usr/lib/systemd/system/graphical.target.
```

sistemde var olan target'ları görebilmek için:

> systemctl list-units --type=target

# Docker

Linux'larda run edilen uygulamaların birbirinden habersiz çalışması, "Linux Containers (or LXC or Runc)" isimli yapı ile zaten sağlanmaktadır. yürüyen uygulamalar birbirinden sadece port yada sadece dosya sistemlerini ortak yada bağımsız kullanmaları sağlanabilir. var olan bu teknolojinin konfigürasyonu zor ve ayrıntılıdır. Docker bu yapıyı değiştirerek (Libcontainer isimli proje ile) ve kolaylaştırarak kullanıcıların rahatça izole ortamlarda (container'larda) uygulama yürütebilmesini sağlamaktadır.

# Linux dışındaki işletim sistemlerinde Docker nasıl çalışıyor

Docker sanal makine kurmamakta yada çalıştırmamaktadır. ve "container" yapısı sadece Linux'ta vardır. fakat Microsoft Windows ve MacOS container yapısı desteklememektedir. Docker uygulaması Microsoft Windows'a yada MacOS'e kurulduğunda, çalıştırılacak uygulamalar Linux tabanlı olduğu için, Docker kendi içinde bir sanal Linux makinesi kurmaktadır (Docker-machine aracılığı ile kuruyor. "Tiny Core Linux" türevi olan Boot2docker isimli işletim sistemini çalıştırıyor). Bu makine üzerinde uygulamaları yürütmektedir. bu işlemleri Docker harici bir firma Boot2docker isimli uygulama ile yapmaktadır. ve arkaplanda sanal makine kurduğunu da önyüzde göstermemektedir. işletim sistemi Boot2docker iken, bunu VirtualBox'u otomatik kurup yöneten uygulama Boot2docker-cli'dir. artık Docker-machine komut satırı uygulaması ile Docker firması bu işi üstlenmektedir. bu sebeple Boot2docker projesi artık geliştirilmiyor.

Docker var olan işletim sisteminin çekirdeğini kullandığı için (ortada ikinci işletim sistemi olmadığı için), Hypervisor gibi işlemci teknolojilerine bağımlılığı yoktur. en azından Linux için bu şekilde. fakat Microsoft Windows, 2016 yılı sonrası sürümleri itibari ile container yapısını desteklemeye başlamıştır. fakat teknik altyapıda donanıma bağımlılık vardır. hyper-v tarzı teknolojilere dayanmaktadırlar.

Microsoft Windows base image olarak temelde şu 4ünü resmi olarak sunuyor:
- __Nano Server__ - is an ultralight Microsoft Windows offering for new application development. ".NET Core" kullanan uygulamalar için bu image yeterlidir.
- __Server Core__ - is medium in size for Microsoft Windows Server apps.
- __Windows__ is the largest image and has full Microsoft Windows API support.
- __Windows Server__ is slightly smaller than the "Microsoft Windows" image, has full Microsoft Windows API support, and allows you to use more server features.

Bunların üstüne kurulu .Net gibi birçok özel, ayrı ayrı hazır görüntüler de resmi olarak sunuluyor.

# Docker Daemon (or Docker engine)

Docker'ın container'ları yönetebilmesini sağlayan yazılımıdır. Libcontainer tabanlıdır. bir nevi Hypervisor görevi görür.

# Docker command line interface

Docker uygulamasının bir client'ıdır. Docker daemon'a erişir. Docker daemon'a CLI'den erişmek zorunlu değildir. API kullanılarak bağımsız client'lar yazılabilir. erişim TCP soketleri ve HTTP istekleri üzerinden yapılabilmektedir.

# Docker server vs Docker client

her makinede ikisi yada sadece bir tanesi yüklü olabilir. server Docker engine'dir. Docker'ları o makinede koşabilir. client ise user isteklerini Docker server'a gönderebilme yeteneğine sahiptir.

# Docker Registery

internette bulunana hazır container repo hizmetlerinin genel adıdır.

# Docker Hub

Docker firmasının sunduğu resmi 'Docker Registery'sinin ismidir.

# Docker-compose

YAML konfigürasyon dosyasını parametre olarak alır ve bu konfigürasyonlara göre birden fazla Docker container'ını ayağa kaldırır. Kolay bir konfigürasyon dosyası olması ile ön plana çıkar. komut satırında bizim uzunca yazacağımız şeyleri kısaltmış olur.

Docker-compose configürasyon dosyasının (yml) birçok sürümü vardır. dosyanın içinde en başa version belirtilmelidir. "version:3", "version 3.0" ile aynı anlama gelir. Tüm sürüm listesi buradadır: https://docs.Docker.com/compose/compose-file/compose-versioning

"links" özelliği Docker compose'da varken, stack'te yoktur. fakat stack'te olan bazı özellikler, Docker-compose'da yoktur.

```yml
links: # aşağıdaki servisler artık hostname üzerinden erişilebilir olur. bu servislerin network'leri mutlaka "links"i koyduğumuz container'ın network'ü ile aynı olmalı.
      - db # service_name
      - db:database # service_name:alias
      - redis # service_name
```

# stack

swarm modülünden bağımsızdır. stack Docker-compose'un gelişmişidir. swarm özelliği gibi Docker-compose'un yapamadığı bir çok özelliği destekler.

v3 YML örnek dosya:

```yml
version: "3"

services:  #service tanımı başka bir başlıkta anlatılıyor

  web:  #Docker servisinin adı. daha sonra bu ismi kullanarak Docker-compose komutları ile ekstra işlemler yapabileceğiz

    build: # service image'den değilde, Dockerfile'dan oluşturulacaksa bu kısım eklenir

          dockerfile: /my/dir/dockerfile

          args:    # Dockerfile içinde args varsa kullanılabilir

              PI = 3

    image: myusername/myrepo:mytag  #bu servis hangi image'den oluşturulacak. eğer bu varsa "build" property'si olmamalı.

    environment: # Dockerfile içindekileri override edebilmek için kullanılır

             PROFILE: "Docker"

             ANDROID_PATH: "/root/SDK/android"

    hostname: "web_domain_name" # diğer container'lar bu domain name ile bu container'a erişebilecek

    depends_on:  # bu container'lar başlamadan bu başlamaz

             - configserver

             - discoveryserver



    deploy:

      replicas: 5  #bu servise bağlı 5 tane container oluşacak

      resources:

        limits:

          CPUs: "0.1"  #her container bu makinedeki tüm CPU'ların max %10'unu harcayabilir.

          memory: 50M  #her container max 50 MB RAM harcayabilir

      restart_policy:

        condition: on-failure

      placement:

        constraints: [node.role == manager] # manager tipindeki node'larda bu container çalışacaktır. yada farklı seçeneklerde var.

                                            # örnek: [node.labels.layers == xxx] --> xxx label'ı verilmiş node'larda koşacak.

    ports:

      - "80:80" # içeriden dışarıya olan port yönlendirme

    networks:

      - webnet # var olan hangi network'e bağlı olacak. bu özellik yeni geldi.

networks:

  webnet #bir network ismi
```

# cgroups ve Linuxnamespace ayarları

(ne oldukları başka başlıkta anlatılıyor)

her container için ayrı ayrı ayar verilebilir. Linuxnamespace'ler Docker tarafından otomatik ayarlanıyor. oysa yukarıdaki Docker compose örneğinde görüldüğü gibi cgroups'lar user tarafından configüre edilebiliyor (örnek: CPU 0.1 gibi).

# Docker swarm

swarm Türkçe kelime anlamı: sürü

Docker'ın içinde gelen bir modüldür. Docker'da cluster/scale işlemlerini yapar. instance'ları(container'ları) scale ederek arttıran ve önlerine otomatik load balancer koyabilme yeteneklerine sahiptir.

# Universal Control Plane (or UCP)

birçok makinadaki Docker-daemon'lar tek bir noktadaki bu sunucu tarafından Web-GUI ile yönetilebiliyor/monitör edilebiliyor.

# UCP-agent (or UCP node)

2 çeşittir:

- manager: web-ui'ı sunar ve worker'lara komut gönderir.

- worker: Docker container'larını çalıştırır.

manager özelliğine sahip bir node'da isterse container çalıştırır fakat bu pek tavsiye edilmez. sadece hızlı test için, deneme amaçlı kurulumlar yapılması için manager'ların container çalıştırması serbest bırakılmıştır.

manager ve worker node'ları aynı sistemde birden fazla olabilir.

# UCP bundle (or UCP client bundle)

UCP panelden download edilir. Bir Zip dosyasıdır. içinde gerekli anahtarlar ve komut satırı script'leri bulunur. bu script'ler kullanılarak direk Docker yüklü bir makinadan UCP-web-ui panelindeymiş gibi komut satırından işlem yapılabilmesini sağlar.

# service

service bir container instance'ı değil. örneğin yukarıda 1 servise bağlı 5 instance oluşturduk. istersek hemen 10 tane daha ekleyebiliriz. servise yaptığımız ayar o servise bağlı tüm container'lara otomatik yapılmaktadır.

servis tanımımızı güncellediğimiz zaman o servisten türemiş tüm container'lar kapanır ve yeni tanımlama ile tekrar açılırlar.

# service types

Global service: her yeni worker node eklendiğinde bu servislerden birer instance otomatik o workder'da açılmaktadır. firewall/antivirüs, monitor-agents gibi uygulamalarımız burada olabilir.

Replicated service: İlgili servisten kaç adet instance'ın çalışacağına sizin karar verdiğiniz servis tipidir.

# Docker machine

ek bir komut satırı uygulaması. bu uygulama ile yeni sanal işletim sistemleri kurabiliyoruz. bu şekilde container desteklemeyen işletim sistemlerinde, kolayca Linux sanal makine yaratarak onun üzerinde Docker çalıştırmamızı sağlamaktadır. aynı şekilde sadece lokalde değil, uzak bilgisayarlardaki sanal makineler üzerinde de çalışabilmemizi sağlamaktadır.

Docker machine, Microsoft Windows ve Linux'ta VirtualBox kullanırken, MacOS'te HyperKit kullanmaktadır.

Docker machine driver'ları sayesinde VMware, VirtualBox, Microsoft Azure, Amazon Web Services gibi birçok farklı bulut altyapısındaki makina sistemlerine veya sanal makine yöneticilerinde uzaktan işlem yapabiliyoruz.

# Docker toolbox

Docker engine + Docker compose + Docker machine + Kitematic uygulamasını bir arada sunan bir setup'tır.

Microsoft Windows ve MacOS native destek sağladıklarından beri artık bu pakete destek kaldırıldı. Microsoft Windows ve MacOS'e kurulumlarda "Docker for Windows (or Docker Desktop for Windows)" veya "Docker for mac (or Docker Desktop for Mac)" kurulması öneriliyor.

"Docker for Windows" ve "Docker for Mac", Kubectl ve Minikube yazılımını da içinde bulundurmaktadır.

# Kitematic

cross platform Docker client GUI'sidir.

# Docker tooling

Eclipse için Docker client eklentisidir. Docker'ların içine komut yazabilmemizi de sağlıyor.

# Docker container vs Docker image vs Docker file

Docker file, Docker image'i yaratmak için Docker'ın anlayacağı tipte bilgiler/komutlar içeren dosyadır. bu dosya işlenerek (build işlemi) image image yaratılır.

Docker image, Docker file ile yaratılmış run edilmeye hazır sanal pakettir.

Docker image run edildiğinde, Docker container olur.

# Docker file

örnek Docker file:

```dockerfile
FROM Ubuntu # Docker container ilk açıldığında, boş bir dosya sistemi oluşturur. Burada bir işletim sistemi kullanmak gerekli. Bu şekilde temel kütüphaneler dosya sisteminde bulunmuş olur.

ARGS PI = 3 # container içinden bu değer okunamaz. komut satırından override edilebilir: "docker build --build-arg PI=4"

ENV ANDROID_HOME /my/dir # yada ENV ANDROID_HOME=/my/dir. Args ile aynı çağrılır. komut satırından override edilebilir: 'docker run -e "ANDROID_HOME=/my/another/dir"'

COPY /source/dir:/dest/dir #container dışındaki bir dizini yada dosyayı container içine kopyalar

ADD /source/dir:/dest/dir # copy ile aynıdır. ekstra olarak ADD; eğer source URL ise onu indirir ve eğer source arşiv dosyası ise (tar, Zip) onu açıp kopyalar.

RUN echo "hello number $some_variable_name" # yada "hello number ${some_variable_name}".

RUN apt-get install NodeJS # sadece "docker build" komutu ile çağrılır.

CMD ["/usr/bin/node", "/var/www/app.js"] # sadece "docker run" komutu ile çağrılır.

ENTRYPOINT ["/usr/bin/node", "/var/www/app.js1", "/var/www/app.js2"]   # CMD ile aynı görevi görür. tek farkı Docker'ı run ederken, ek parametreler alabilir. "Docker run myImage /var/www/app.js3" komutu ENTRYPOINT'e 3üncü parametreyi ekleyecektir.
```

# WORKDIR
Workdir, Dockerfile içinde kullanabileceğimiz "cd" komutuna denk geliyor.

# app installation with user password

example:

> apt-get install wget

This command will ask "yes or no"? The only way to bypass this action is possible by apt-get feature:

> apt-get get "-y"

parameter and check the user has the privilages to install the wget app.

# userName/containerName vs containerName vs myhub.mysite.com:8081/myContainerName

Docker file'da her iki formatta da paket indirilebilir. userName olmadan indirilenler Docker tarafından resmi olarak tag'lenmiş anlamına gelmektedirler.

"Docker registry" açık kaynaklı yazılımları bulunmaktadır. self-hosted şekilde kendimize yaratabiliriz. böyle durumlar için, IP'si verilmiş makineden de image çekebilir.

# host os (or Container OS)

Docker daemon'u çalıştıran işletim sistemidir. bu herhangi bir kullandığımız son kullanıcı os'u olabilir. fakat bazı OS'lar sadece Docker yürütmek için tasarlanmıştır. içinde gereksiz paketler yoktur.

örnek:
- Snappy Ubuntu Core
- CoreOS (kaynak: (source-id: 256) "Custom operating system" başlığı. )
- RancherOS
- Project Atomic
- Photon

# Snappy Ubuntu Core (or Snappy Ubuntu or Ubuntu Core)

Sadece Snap uygulamaları barındıran minimal bir Ubuntu türevidir. Docker'ın Snap paketi olduğundan sadece Docker kurulu şekilde kullanılması tercih edilebilir. yada iot cihazlarında tercih edilmektedir.

# Base OS (or base image os)

container içinde koşan işletim sistemidir. FROM komutu ile işletim sistemi baz alınırken, işletim sisteminin minimum olması beklenmektedir. bu durum; hem bellekten kazandırır hemde bağımlılıkların daha iyi belirlenmesini sağlar. buradaki işletim sistemleri tamamen farklı bir dizinde (chroot tarzı) saklanmaktadır. container'da run edilen uygulamalar sadece burayı görebilmektedirler. container içindeki uygulama, host işletim sisteminden sadece Linux çekirdeğini ortak kullanmaktadır. onun dışında tüm kaynaklar mount edilen yeni dizin altındadır.

bazen "base os" terimi, "minimal os" terimi yerine yanlış kullanılmaktadır. ikisi farklı şeyler. minimal os, normal destop sistemler içinde olabilir. minimal os, sadece ek paketlerin olmadığı anlamına gelmektedir.

bazen de "container os" terimi "base os" ile karıştırılmaktadır.

Linux'ta direk sadece işletim sistemi çekirdeği kullanan bir uygulama çalıştırılırsa baseOS'a ihtiyacımız olmaz.

örnek:
- Alpine(özelliği ufak olması)
- Ubuntu
- Busybox(özelliği ufak olması)
- CentOS
- Fedora
- Nanoserver(Microsoft Windows native container'lar için)

# container user

container üzerinde çalışan uygulamalar root kullanıcı ile çalıştırılır. Docker, runtime sırasında, root kullanıcısında komut yürütmemize izin verir.

# stop vs remove komutları

iki komutta container'ı durdurmaktadır. fakat remove komutu var olan state'i silmektedir. oysa stop komutu state'i korumaktadır. bu şekilde state istenirse farklı amaçlar için saklanabilir.

# attach vs exec komutu

Docker içindeki root user'ına komut satırı üzerinden yönetimini sağlar. attach olduğumuzda yeni bir komut satırı oturumu açılmaz, onun yerine son çalıştırılan komutun çıktılarını görürüz. eğer yeni bir komut satırı istiyor isek; "docker exec -i -t 878978547 bash command arg1 arg2" ile yeni bir bash açabiliriz. exec komutu en sağda yazan komutu container üzerinde çalıştırmamızı sağlar.

# Docker build command

Dockerfile'ın image olarak hazır hale getirilmesini sağlar. run edip anında ayağa kalkması için build olması şart. zira build sırasında Docker için gerekli dosyalar internetten indiriliyor.

# Docker run command

build edilmiş bir image'yi run etmemize yarar.

-d parametresi eklenince container'ın ID'sini döndürüp arkaplanda çalıştırır. -d verilmezse komut satırında işletim sisteminin system.out log'larını basar.

arkaplanda çalışan container'ın system.out çıktılarını "Docker log" komutu ile de görebiliriz.

# Docker commit komutu

bu komut ile çalışmakta olan bir image, kopyalanıyor ve yeni bir image yaratılıyor. bu image'nin ID'si ekrana basılıyor. image içindeki bilgiler tutulmuş oluyor. örneğin; MySQL image'si run edildi, daha sonra üzerine manuel olarak wget kuruldu. bu image kopyalanıp saklanılması isteniyor ise; commit komutundan yararlanılmalıdır. aslında yeni bir isimde klon image oluşturulmuş oluyor. fakat commit yerine Dockerfile yaratılması öneriliyor; çünkü image'in nasıl yaratıldığını daha net bilebiliriz.

# layer

container içinde yapılan her işlem yeni bir katman yaratıyor. örneğin layer değişmeden commit yapabilmemiz mümkün değil. çünkü değişen bir dosya olmayacaktır.

# Docker diff command

"Docker diff containerId" komutu verildiğinde o containerId'nin yaratıldığı image-layer'ı ile şu andaki layer arasındaki dosya sistemindeki farkları listeleyecektir.

# kill vs stop command

container'ların içindeki tüm uygulamalara terminate yada kill sinyali gönderiyor. sinyaller başka başlıkta anlatılıyor.

# -p parameter

container'ı run ederken port numarası mapping'i yapmak için kullanılır. örneğin;

> Docker run -p 80:8080 imageId

Bu şekilde host makinenin 80 portuna gelen istekler, Docker container içindeki 8080 portuna yönlendirilecektir.

# -v parameter

-p parametresi ile aynı formata kullanılıyor. fakat dizin bağlamak (mount etmek) için yapılıyor. örnek:

> docker run -v /home/host-user/dir:/home/Docker-user/dir imageId

# docker inspect

inspect yanına yazılan ID, herhangi bir kaynağın ID'si olabilir. örnek: volume ID, image ID, container ID, network ID, stack ID gibi.. tüm detaylı JSON formatında bize döndürür.

# docker --link

Docker container'ların birer IP si vardır. bu IP'ler birbirine erişebilmelidirler. bu yüzden host dosyasına DNS kaydı otomatik atılabilir. örnek:

> docker run --link myOtherContainerId:aliasOnDnsHostFile imageId

myOtherContainerId IP'si 10.0.0.1 ise; yeni yaratılan container içindeki host dosyasına bakıldığında bu satır görülecektir:

> 10.0.0.1 aliasOnDnsHostFile

# Docker hub'a image yollama

Docker login komutu ile komut satırından Docker'a login olunabilir. daha sonra herhangi bir image'ı push komutu ile Docker hub'daki hesabımıza yollayabilir.

# repository vs registry

repository bir Docker-image'sinin farklı sürümlerinin barındırıldığı sunucu yazılımıdır. tag ise Docker-image'nin sürümüdür. repository kelimesi birçok farklı Docker-image'ini barındırıyormuş gibi görünüyor, oysa aynı Docker-image'inin farklı sürümlerini barındırıyor. örneğin: repo:JDK tag:8.

registry ise birçok farklı Docker-image'sinin bulunduğu sunucu yazılımıdır. örnek "Docker hub", Docker Inc firmasının resmi registry'sidir ve default olarak Docker client'ta tanımlı gelir.

# Docker ce

community edition (free)

# Docker ee

enterprise edition

# Docker edge vs stable

edge is beta version.

# Docker Trusted Registry (or DTR)

locale kurulabilen registry.

# Docker collection

Docker'ın UCP'sinin vermiş olduğu bir özellik. birçok node UCP'ye bağlı olabilir. her node'da hangi servisi koşacağı stack-yml dosyalarında bellidir. fakat swarm-yml dosyaları her bilgiyi barındıramıyor. bu sebeple bazı bilgiler sadece UCP içinde kayıtlıdır. örnek:

- hangi volume'nin hangi node'da olacağı

- node'lara atanmış label bilgileri

- stack dosyaları haricinde yaratılmış servisler

bunların yedeğini alabilmek için gruplama ihtiyacı vardır. işte bu gruplara "collection" denir. collection sadece bu bilgileri değil, tü sistemin bilgileri saklar. örneğin; stack-yml dosyaları da buna dahildir.

her collection, UCP tarafından ayrı ayrı dizinler içerisinde saklanır.

# Moby

artık (2017 yılı ile) Docker açık kaynaklı proje olarak Moby projesi altında dağıtılıyor. Moby projesi DockerCE ve EE olarak Docker tarafından kapatılıp üzerinde bazı değişiklikler yapıp dağıtılmaktadır. Moby projesi ile hedef Docker'ı daha modüler hale getirmektir.

# union file system (or UnionFS)
UnionFS filesystem'in özel bir ismidir.

"__Union mount__" ise dosya sistemlerinde olan bir feature'dir. Bu özellik desteklenen dosya sistemlerinde, bir dosyanın farklı branch'leri (versiyonları) olabilmektedir.

UnionFS'i, Docker, run edilen container'lar için kullanır. örnek: Java kurduk ve uygulamamızı run ettik. Java ve Ubuntu-os otomatik kurulur. bu 2 container read-only'dir. run ettiğimiz Java uygulaması bir şeyler kaydetti. bu dosyalar Ubuntu-os'un dosyalarını da değiştirmiş olabilir yada ek dosya atayabilir. böyle durumlar için dosya sistemi böyle işliyor: Ubuntu-os ve Java'nın olduğu bir dosya sistemi read-only (no:1) oluşturuluyor. daha sonra farklı bir dosya sistemine (no:2) Java uygulamamız dosya yazıyor. eğer container içinden bir dosya okumaya kalkarsak; UnionFS önce no:2 dosya sistemine bakıyor. eğer bir dosya silme bilgisi yada yeni dosya oluşturulmuş ise bu dosyayı bize dönüyor. eğer hiçbir bilgi bulamaz ise no:1 dosya sistemindeki dosyayı dönüyor. bu sistemde no:2 dosya sistemine no:1'in branch'i adı veriliyor. istediğimiz kadar branch oluşturabiliyoruz.

var olan ve container açıldıktan sonra düzenlenen bir dosya branch'e kopyalanır ve düzenlenir. tabi eğer taşınan bu dosya çok büyükse ilk kopyalama işlemi zaman alacaktır.

UnionFS'e birçok alternatif var. bu alternatifleri Docker engine'den seçebiliyoruz. bazı dosya sistemleri tüm dosyayı değilde, dosyayı belli bloklara bölüyor ve sadece değişen kısmın bloğunu taşıyor. bu tarz farklı yöntemler mevcut.

# volume
bir container'a host bilgisayarımızdaki bir dizini mount edebiliriz. buna alternatif olarak sanal bölümler Docker tarafından oluşturulabilir. bunlar host makina tarafından okunamazlar. mount ve volume karşılaştırılacak olursa asıl önemli avantaj: volume'ların Docker tarafından mount edilmesi olacaktır. host bilgisayarda mount edeceğimiz dizinin yetkileri, dosya sisteminin desteklenip desteklenmeyeceğini gibi sorunlarla karşılaşabiliriz. oysa volume'larda böyle bir sorun yok.

# Docker temizlik

## dangling images

Türkçe kelime anlamı: askıda kalmak

örnek üzerinden direk gidersek. Docker içinde JDK indirdik. os olarak Fedora'yı indirdi. Fedora'yı indirince, Fedora ile "latest" isminde bir layer (image) oluştu. daha sonra JDK layer'ı oluştu. 1 ay sonra tekrar JDK'yı indirdik. bu sefer içindeki Fedora güncellendi. ve yine "latest" tag'i ile. dolayısı ile eski "latest" tag'li Fedora artık "none" isminde kaydediliyor. bu tarz durumlar bazı layer'ları askıda bırakıyor. sadece askıda olan layer'ları temizleyen komut mevcut.

tag vermeden oluşturduğumuz her image dangling'dir.

## Docker system prune

prune Türkçe kelime anlamı: budamak.

bu komut sırası ile şu komutları işletiyor:

- Docker container prune

- Docker image prune

- Docker network prune

- Docker volume prune

## Docker system prune -a

-a parametresi durmuş tüm kaynakların silinmesini sağlıyor. yani o sırada run edilenler haricindeki tüm kaynaklar siliniyor. hiçbir container run edilmezken çalıştırılırsa tüm kaynaklar silinir.

## Docker container prune

stop olmuş tüm container'ları siler.

## Docker image prune

dangling image'leri siler.

## Docker image prune -a

hiçbir container tarafından kullanılmayan image'leri siler.

## dangling volume

bir volume container run edildiğinde mount edilir. yani container ile ilişkilendirilir. eğer ilişkisiz bir volume var ise, bu volume dangling'dir. stop olmuş fakat silinmemiş container'lar hala volume'ler ile ilişkilidir.

## Docker volume prune

dangling volume'leri siler. bu komutun "-a" ile bir alternatifi yoktur. çünkü zaten container ile bağlantısı olmayan her volume dangling'dir.

## Docker network prune

volume ile aynı mantıktadır. bu komutun "-a" ile bir alternatifi yoktur. çünkü zaten container ile bağlantısı olmayan her volume dangling'dir.

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# Sanal makinelerde disk formatları

```
 extension   native-support      support               açıklama

- VMDK       VMWare              Virtualbox, QEMU/KVM

- VDI        VirtualBox          QEMU/KVM              Genel olarak VDI ve WDMK arasında önemli farklar mevcut değil.

- raw/img    QEMU/KVM                                  hiçbir özellik barındırmıyor. sanal diskte ne varsa direk burada binary olarak tutuluyor. format header dahi barındırmıyor. en iyi performans bunda.

- cow        QEMU/KVM

- qcow       QEMU/KVM            VirtualBox            cow'un yeni sürümü.

- qcow2      QEMU/KVM                                  qcow'un yeni sürümü.

- vhdx       Hyper-V             VirtualBox            vhd'nin yeni sürümü.

- vhd        Hyper-V             VirtualBox

- cloop      QEMU/KVM                                  özel amaçlı kullanılan bir format.

- qed        QEMU/KVM            VirtualBox            qcow2'nin özellikleri silinerek performans kazandırılmaya çalışılıyor. fakat proje durduruldu.
```

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

• • • • • • • • • • • • • • • • • • • • • • • •

# özet karşılaştırma: OpenShift vs istio vs Kubernetes vs Spring boot
bu karşılaştırmayı yapmak doğru değil. fakat başlıktaki teknolojilerin bazı özellikleri ortak problemi çözmektedirler. tek tablo olarak hızlıca buradan özellik listesini görebiliriz:

- (source-id: 257) "Feature Overlap" başlığındaki şekil.

- (source-id: 258) "Technology Mapping" başlığındaki ilk şekil ve "Best of Both Worlds" başlığındaki ikinci şekil.

# OpenStack
NASA'nın da geliştirmekte olduğu, açık kaynaklı Bulut bilişim altyapı projesidir. bulut bilişim sunan bir firmada olması gereken tüm yazılımlar bu proje altında açık kaynaklı olarak geliştirilmektedir. projede öncelik IaaS'tır.

# OpenShift
Kubernetes'i gömülü olarak kullanan onun üzerine ek özellikler sunan bir yazılımdır:

- hazır Web UI (web console)
- 3üncü parti (fakat sertifikalı) CI/CD ve Monitoring hizmetleri için gömülü entegrasyon özelliği.
- integrated image registry
- resmi ücretli destek
- güvenlik yamaları ek olarak sunulmaktadır (en azından açıkların Kubernetes'e nazaran daha hızlı kapatılacağının garantisini vermeye çalışıyorlar)

OpenShift 'oc' komut satırı uygulaması yada web arayüzünden yönetilmektedir.

Openshift'te her proje, Kubernetes'teki namespace'e denk gelmektedir. Diğer daha ufak birimler tamamiyle aynı isimde kullanılır. kaynak: (source-id: 259) "Namespaces" başlığı.

OpenShift'in yönetim paneli için sunduğu web arayüzünde, bir proje içerisinde yeni bir Pod ayağa kaldırılmak istendiğinde "namespace" bilgisi ister. burada istenen namespace bilgisi, Pod'un çalışacağı namespace değil, OpenShift'in sunduğu "imagestream" özelliği için kullanılacak namespace bilgisidir.

- ## Minishift
komut satırı uygulamasıdır. lokalde tek cluster OpenShift ayağa kaldırır.

- ## OKD (or Origin Community Distribution or old-name:OpenShift Origin)
ücretsiz lisanslı OpenShift versiyonu.

- ## OpenShift Container Platform (or old-name:OpenShift Enterprise)
OKD'nin lisansı ücretli ve destek alınabilen versiyonudur.

- ## Red Hat OpenShift Online (or RHOO) vs OpenShift Dedicated vs OpenShift.io
Ticari lisanslı sadece bulut makinelerde çalışabilen OpenShift hizmetleridir.

# Envoy
Türkçe kelime anlamı: elçi

L7 proxy'dir ve load balancing gibi ek özellikler barındırır.

# L4 vs L7 proxy
Proxy'ler data iletiminde aracılık yaparlar. aracılık yaparken gelen giden bilginin ne olduğu ile ilgilenilirken, L4 proxy'ler OSI katmanlarında 4üncü level seviyesinde data transferlerine manipülasyon yapabilirler. Yani UDP, TCP seviyesinde bilgiyi tanırlar. TCP ile gönderilen bilginin ne olduğu ile ilgilenemezler. Oysa L7 proxy'ler en üst seviye OSI katmanında, 7inci katman, application layer'da işlem yapabilirler.

Bazı durumlarda; L7 proxy; OSI 4 katmanında, L4 ise OSI 7 katmanında işlem yapması gerekebilir/yapabilir. Bir proxy yazılımının hangi tipte olduğunu tanımlarken en çok hangi katmandaki işlemlere öncelik verdiği değerlendirilir.

bir L7 proxy aşağıdaki özellikleri sunabilir:

- load balancing

  kendine gelen request'leri, önünde durduğu node'lara paylaştırır

- Health checking

  - Active

    önünde duruğu her node'a sürekli /healchecking gibi bir URL ile istek atar. cevap alamazsa, o node'a request'leri yollamaz

  - Passive

    önünde durduğu node'ların client'lara verdiği cevapları inceleler. eğer cevaplarda terslik varsa, o node'u sağlıksız olarak set eder ve ona request yönlendirmez

- Service discovery

  kendini servis discovery'ye tanıtır

- Sticky sessions

  cookie'lere bakarak, kullanıcını aynı session'ı boyunca aynı node'a gitmesini sağlar.

- dos ataklarının önüne geçmek için kalkan oluşturur

- rate limiting

  kullanıcının session'ına göre (state'ine göre) belli bir sürede yapabileceği request sayısı tanımlanır. eğer bu sayıyı geçerse kullanıcının engellenir.

- yönetim paneli

# Proxy türleri

- middle proxy

  klasik proxy sunucularıdır. gelen istekler ve döndürülen cevaplar bu proxy üzerinden geçer.

- Edge proxy

  API gateway olarakta adlandırılırlar.

- Embedded client library

  bir kütüphane olarak node'lara eklenir. bunun dezavantajı dilden bağımsız olamaması. "polyglot (or multilingual)" kütüphaneler yazılması gerekir.

- Sidecar proxy

  kendi başına genelde aynı container içerisinde ayağa kaldırılan proxy'dir.

# Direct server return (or DSR)
bu tarz proxy'ler dönen response'ları kendi üzerlerinden geçirmezler. response'lar direk client'a node tarafından yollanır.

# downstream vs upstream
upstream kelime anlamı: akıntı yönüne karşı.

downstream kelime anlamı: akıntı yönüne doğru.

proxy dünyasında; downstream request'i yollayan, upstream ise cevabı yollayan node'dur.

# Service Mesh
Mesh Türkçe kelime anlamı: çark, kafes.

mikroservislerin artmasıyla birlikte bu tanım ortaya çıktı. servislerin ağ yönetimi (birbirleri ile haberleşmeleri) sistem büyüdükçe daha zorlaşmaya başlıyor. bu ağı yönetebilmek için birçok çözüm sunuluyor. "Service Mesh" tanımı sıkça karşımıza çıkmaktadır. "service mesh"; ağın yönetimi çözümü için uygulanan altyapılara verilen genel bir terimdir.

sidecar proxy, service discovery, orchestration framework, load balancing, circuit breaker gibi altyapıları içeren çözümler sunulur.

(Sidecar pattern'i başka başlıkta anlatılıyor) Modern servise mesh çözümlerinde sidecar pattern'inden sıkça faydalanılıyor. modern çözümlerde, her Pod/container'a bağlı bir sidecar proxy bulunuyor. her gelen giden request buradan geçiyor. o Pod/container'a bağlı sidecar security, loggining gibi birçok görevi Pod/container'da yüklü olan uygulama yerine yapıyor.

istio sadece "service mesh" yönetimi sunarken, OpenShift hem orchestration hemde "service mesh" özellikleri sunar.

# Istio
Kubernetes içerisinde oluşturduğumuz her Pod içinde, istio (otomatik olarak) kendi container'larını açar. bu container'lar ile sidecar proxy pattern'ini baz alarak load balancing, logging gibi birçok servise mesh işlemimizi halleder.

'istioctl' komut satırı uygulaması ile yönetimi sağlar.

# Kubernetes (or abb:K8s)
UCP, Docker swarm ve Docker compose'a alternatif olan ve daha fazla özelliği içeren açık kaynaklı yazılımdır. Kubernetes container'ları yönetmek için kullanılır. container'ların ne ile yönetildiği ile ilgilenmez. yani Kubernetes Docker yada rkt ile de çalışabilir. kaldırdığı container'ları istediğimiz herhangi bir container yazılımı ile kaldırabiliriz.

Docker UCP'deki özelliklere ek olarak Kubernetes'in sundukları:
- cross-application config server barındırır
- cross-application discovery server barındırır
- port yönlendirme yapar, bu şekilde istediğimiz container'ın istediğimiz portuna bağlanabiliriz
- scheduled cronjobs

- ## command line tools

   aşağıdaki tüm komut satırı uygulamaları Kubernetes'ten ve birbirlerinden bağımsızdır. sadece istediğimiz paketin binary'sini indirip lokalde çalıştırabiliriz.

   - ### Kubectl
     Kubernetes command line tool'udur.

   - ### Minikube
     komut satırı uygulamasıdır. lokalde test amaçlı çalışmalar için geliştirilmiştir. local makinede sanal makina açmakta ve içerisinde tek node'lu Kubernetes yürütmektedir.

     eğer local makinede "Docker desktop" yüklü ise; sanal makinesiz de direk Kubernetes yürütebilmektedir çünkü Kubernetes tool'ları Docker desktop ile yüklü gelmektedir.

     Minikube "Minikube" isminde bir context oluşturuyor ve Kubectl'ye default context olarak set ediyor.

     Minikube başlatma:

     ```sh
     minikube start --driver="none"
     ```

     Opsiyonel olarak vereceğimiz driver'lar aşağıda listelenmiştir. Driver, cluster'ın tümüyle hangi ortama kurulacağını belirliyor.

     - VirtualBox

       VirtualBox üzerinde yeni VM açıyor ve bunun içine tümüyle Kubernetes admin ortamı/cluster'ı kuruyor.

     - Hyperv

       VirtualBox ile aynı mantıkta çalışıyor.

     - KVM

       VirtualBox ile aynı mantıkta çalışıyor.

     - VMware

       VirtualBox ile aynı mantıkta çalışıyor.

     - none

       Kubernetes admin ortamını ve bunun üzerinde bir cluster'ı local OS'umuza kuruyor. Bu pek tavsiye edilen bir yöntem değil. Çünkü var olan OS'umuzdaki dosyalarda değişiklik yapıyor.

       Kubernetes cluster'ı ve admin ortamını kuracağımız ortamda hem Docker requirement'i vardır hemde container'lar haricinde normal process'lerde çalıştırılır ve OS'un dosyalarına müdahale gerçekleşir. (zaten bunlardan kaçınmak için Minikube çıktı ve bu sebeple bu driver tavsiye edilmiyor)

     - Docker

       OS'umuzda bir Docker başlatıyor. Bu Docker'ın içerisine gerçek bir Docker kuruyor ve Kubeadm kurulumu yapıyor.

       kaynak: (source-id: 260) Pull request'in başlığında "Kind" kullanıldığı belirtilmiş.

       __KIND__ Kubernetes'in Docker ortamına kurulması için geliştirilen bir Docker-image projedir. KIND, Docker içerisine Docker kurulumu yapar ve image içerisine Kubernetes kurar. kaynak: (source-id: 261) "Deep Dive: KIND - Benjamin Elder & Antonio Ojea" videosunda, 2:08'de slide ekranında Docker içerisine neler kurulduğu yazılmış.

   - ### Kubeadm
     komut satırı uygulamasıdır. Kubernetes'in kendisini update edebilmemizi, cluster oluşturabilmemizi sağlıyor.

- ## components

     - ## Control Plane Components (or Master Components)
       Cluster'daki tüm node'ları ve sistemi yöneten modüllerin tümüdür. Aşağıdaki parçalardan oluşur:

        - ### Kube-apiserver
          API'nin sağlanması için gereken module. API olarak servis sunuyor. buraya HTTP isteği olarak yollanan emirleri Kubelet'lere yolluyor. Kube-apiserver master node içerisindedir.

        - ### etcd
          key-value veritabanıdır. bütün sistemin meta bilgilerinin yönetimi buradan yapılır.

        - ### kube-scheduler
          scheduler'ları düzenli olarak çalıştıran modüldür. aynı zamanda yeni oluşturulan POD'ları node'lara atayan modüldür.

        - ### kube-controller-manager
          node'ların ayakta olup olmadığını kontrol eder, açılan Pod'ların doğru adet olduğunu kontrol eden... modüldür

        - ### cloud-controller-manager
          kube-controller-manager'in bulut hizmetler (Amazon AWS gibi) için geliştirilmiş türevidir.

     - ## node components
       node bir VM yada fiziksel makinedir. node components, her node içinde yüklü olan modüllerdir.

        - ### Kubelet
          her node'da bir adet yüklüdür. bu yazılım kube-controller-manager'dan emirleri alır ve yerine getirir. o node üzerinde yeni Pod'ların ayağa kaldırılması kapatılması gibi işlevleri yerine getiriyor.

        - ### kube-proxy
          her node'da bir adet yüklüdür. "Kubernetes servis", "port yönlendirme" gibi network'sel işlemlerinin altyapısının her node'da çalışmasını sağlayan yazılımdır. "network proxy" yazılımıdır.

        - ## Container Runtime
          her node'da kurulmuş olması gereken Docker yada alternatifi container manager'ı.

- ## addons vs extension vs plugin
     Kubernetes dümnyasında 3'ü birbirinden farklı kavramlardır. Plugin olarak sadece "volume plugin"leri mevcuttur.

- ## namespace, context, cluster

     - ### namespace
       her cluster'da birçok namespace olabilir. her namespace kendi içinde Pod ve servisler bulundurur.

       tüm namespace'leri listele:
       > kubectl get namespaces

       yeni namespace yarat:
       > kubectl create namespace dev

     - ### context
       kubectl komutunu çalıştırırken, server'a login olacak bilgileri ve hangi namepsace'te komut çalıştıracağımız bilgisi gereklidir. bu login ve namespace bilgilerinin bütününe birlikte "context" denir.. örneğin bir context yaratalım:
       > kubectl config set-context minidev --cluster=cluster1 --user=user1 --namespace=namespace1

       switch to context:
       > kubectl config use-context minidev

     - ### cluster
       Kubernetes cluster altyapısını destekleyen platformdur. örneğin Google cloud, Amazon cloud bu yapıyı destekliyor. lokalden hangi cluster'a gideceğimizi komut satırından belirtmemiz gerekmektedir.

       Bir yazılım projesinin development, test, prod gibi ortamlarının ayrı cluster'lara (namespace'lere değil!) kurulması önerilir.

# Kubernetes Objects
Kubernetes'te yaşayan ve API üzerinden manage edilen her feature birer objedir. Obje listesi aşağıda verilmiştir.

- Pod
- service
- volume
- namespace
- Controllers
  - ReplicaSet
  - Deployment
  - StatefulSet
  - DaemonSet
  - Job

```yml
apiVersion: v1
kind: Service # this is "object".
metadata:
  name: my-object-name
  labels:
    run: my-object-label 
spec:
  # the field under "spec", depend on the object type.
```

# Pod
Kubernetes'in kullandığı en ufak birimdir. içerisinde birden fazla container olabilir.

her Pod network'ü ortak kullanıyor. istediği storage namespace'lerini ise ortak kullanabiliyor. Pod bir container değil. Pod container wrapper'ı. yani Pod kendi içinde bir container dizini barındırmıyor. Pod örneğin 2 container içeriyorsa, Pod 2 container'ı Docker ile ayağa kaldırıyor. 2 Docker'ı yöneten yapıya Pod deniliyor. Pod'un kendisini bir container gibi düşünmemek gerekli. çünkü container terimi farklı kapıya çıkıyor. Pod bir process olarak düşünülmeli. bu proses diğer açılan 2 Docker process'ini yönetiyor.

Pod'a en yakın tanım Docker-compose'dur. sadece belirtilen container'ları açmakla/yönetmekle ilgilenir. kendisi bir container değildir.

Pod tanımının bulunduğu dosya örneği:

```yml
apiVersion: v1 # in which Kubernetes version this file works.
kind: Pod # can be other types like: Service, Pod...
metadata:
  name: multi-container-example # name of this Pod.
  namespace: test
  labels:
    app: web # labels are custom key/values. (labels are explained in another topic)
spec:
  containers:
  - name: nginx
    image: nginx:stable-alpine
    resources:
      limits:
        memory: "600Mi" # if it will try to use more memory, this Pod will be terminate automatically by Kubernetes and will re-create again.
        cpu: "500m" # pod will not be killed if will try to use more then 500m CPU. source: https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/ title: "How Kubernetes applies resource requests and limits", paragraph: 2nd paragraph from the end.

        # cpu unit is the core cpu (virtual) count of the current machine. for example:
        # - 3000m ---> 3    CPU core of the current machine
        # - 10m   ---> 0.01 CPU core of the current machine
        
    ports:
    - containerPort: 80 # port which is exported to outside
    volumeMounts:
    - name: html
      mountPath: /usr/share/nginx/html # path which will mount inside container filesystem
  - name: content
    image: alpine:latest
    volumeMounts:
    - name: html
      mountPath: /html # path which will mount inside container filesystem
    command: ["/bin/sh", "-c"]
    args:
      - while true; do
          echo $(date)"<br />" >> /html/index.html;
          sleep 5;
        done
  volumes:
  - name: html # which is the reference name of this volume.
    emptyDir: {} # creates an empty volume.

    # alternative of "emptyDir":
    # hostPath:*
    #   path: /data # the full path of the namespace cluster machine

    # other alternatives of "emptyDir": (source-id: 262)
```

context'imizi komut satırından belirledikten sonra Pod'umuzu yaratabiliriz:

> kubectl create -f multi-container-example.yaml

Yukarıda 1 Pod içinde 2 adet container bulundurulmuştur. Mount şekli Docker'dan biraz farklıdır. Mount tanımı container tanımının dışındadır (üst seviyesindedir). bu mounting'e ihtiyacı olan container'lar "mountPath" ile bu tanımı kendileri için kullanabilirler. Yukarıdaki "html" isimli volume her 2 container tarafından kullanılmış. böyle durumlarda aynı dosyalar 2 container tarafından da görülüp yazılabilmektedir.

Aşağıdaki komut ile var olan Pod'ları listeleyebiliriz:

> kubectl get pods --show-labels

Log'a bakmak için:

> kubectl logs -f pod-name --tail 200

Eğer bir Pod içerisinde birden fazla container varsa, sadece ilgili container'ın log'una bakmak istiyorsak:

> kubectl logs -f pod-name --tail 200 -c container-name

get detail of running Pod:

> kubectl describe pod pod-name

delete Pod:

> kubectl delete pod pod-name

yada

> kubectl delete -f pod-manifest.yaml

# port-forward
Pod manifest YML dosyamızda dışarıya açılacak portu belirtmezsek; sonradan komut satırında şu şekilde portu dışarıya açabiliriz:

> kubectl port-forward pod-name 8081:8080

# CNI (or Container network interface)
host'lar ve container'lar arasındaki network iletişimini sağlayan Kubernetes plugin'leridir. isteğe bağlı kullanılırlar. örnek plugin'ler: Calico, Flannel, Weave Net

temel olarak; CNI eklentisi, her makinede bir bridge (eth0, loopback gibi) açıyor. bu bridge üzerinde routing table konfigürasyonları ile diğer makinedeki container'lara direk olarak, container IP'leri ile istek atabilmemizi sağlıyor.

bu route table'ların birbirlerini görebildiği sanal ağın (IP kümesinin) tümüne __overlay network__ adı veriliyor.

# label and selector

Pod veya service YML'mizin başında istediğimiz isimde label'lar kullanabiliriz. bu label'lar sayesinde daha sonra birçok sorgu, update gibi işlemleri belli Pod veya servislerimize sağlamış olacağız.

```yml
apiVersion: v1
kind: Pod
metadata:
  name: multi-container-example
  labels:
    app: nginx
    environment: prod
    # others labels here...
```

# service
service; selector'ler ile belirttiğimiz Pod grubuna referans edecek olan sanal yapıdır. örneğin; bir servise istek yaparsak, aslında ona bağlı Pod'lara istek yapmış oluruz gibi...

example YML file:

```yml
apiVersion: v1
kind: Service
metadata:
  name: user-service
  labels:
    run: user-service # the label of this service.
spec:
  type: NodePort # can export port for accessing outside of the cluster.
  ports:
    - port: 8080 # this is the exported port of this service (only for communication between POD's. not outside of the cluster.)
                 # By default and for convenience, the "targetPort" is set to the same value as the "port" field.
      targetPort: 8081 # if any request come to 8080(port), it will redirect on this port of POD
      nodePort: 8083 # the exported port of this service for outside of the cluster.
      protocol: TCP
      name: http-port # this is only a label for this port.
    - port: 8090
      targetPort: 8091
      nodePort: 8093
      protocol: TCP
      name: metrics-port # this is only a label for this port.
  selector:
    run: user-service-pod-label # this service is in front of all the POD's which have "user-service-pod-label".
```

# alternatives of "type"

- __NodePort__
  
  explained above (inside YML file).

- __ClusterIP__ (default)

  does not expose port for accessing the service from outside the world. This is ideal, when we call internal private services.

- __LoadBalancer__

  Cluster'daki implementasyona göre çalışan bir yapıdır. Örneğin cloud sistemlerde direk native load-balancer'ı arkada kulanan yapılar sunulur. eğer local kubernetes kurduysak ve __LoadBalancer__ kullanmak istiyorsak, bu durumda önce MetalLB veya HAProxy veya NGINX gibi uygulamaları kurmalıyız.

- __ExternalName__

# Ingress
Servis tipi değildir. kendisi başlıca "service" yapısına alternatif bir özelliktir. NodePort yapısına benzerdir, fakat Ingress ek olarak; gelen istekteki URL-PATH değerine göre istediğimiz servise yönlendirme yapabileceğimiz kurallar dizisi tanımlayabilmekteyiz.

# deployment
deployment içerisine birçok replica set ve Pod barındırabilen bir yapıdır.

yml dosyamızın içerisinde "kind: deployment" yazmamız gereklidir.

# Replica Controller
istediğimiz Pod sayısının ayakta kalmasını sağlar.

yml dosyamızın içerisinde "kind: ReplicationController" yazmamız gereklidir.

# Replica set
Replica Controller'ın alternatifidir. temelde aynı görevi görür.

yml dosyamızın içerisinde "kind: ReplicaSet" yazmamız gereklidir.

# desired state vs actual state
her Kubernetes objesinin bir 2 state'i vardır. biri yazılımcının istediği durum, diğer ise şu andaki durumudur. örneğin  ReplicaSet ile 100 adet container ayağa kaldırmak istemiş olalım, fakat henüz sadece 50 tanesi ayakta olsun. bu durumda "desired state":100, "actual state":50 dir.

# health checks
Health check işlemi başarısız olursa Kubernetes 2 farklı aksiyon alabilir:

- __Readiness probe__

  probe kelime anlamı: soruşturma/yoklama.

  Eğer probe işlemi (yapılan bu healt-check işlemi) başarısız olursa, Kubernetes bu pod'a gelen request'leri yönlendirmez.

- __Liveness probe__

  Eğer probe işlemi (yapılan bu healt-check işlemi) başarısız olursa, Kubernetes bu pod'u kill eder.

Kubernetes Healt check yapabilmek için 3 farklı seçenek sunuyor:
- HTTP isteği (pod'un healt-check için belirlenen HTTP isteğine 200 dönmesi yeterli)
- TCP isteği (Kubernetes healt-check için belirlenen TCP portuna bağlanma isteği yapar. Eğer connection açılırsa, pod healt-check'i başarılıdır anlamına gelir - Data transfer yapılmaz.)
- Komut (Kubernetes ilgili pod'da bir komut çalıştırır. eğer komut exit code 0 dönerse o zaman pod sağlıklı anlamına gelir)

# Helm
Helm kelime anlamı: dümen.

Helm komut satırı uygulamasıdır. Kubernetes client'ını wrap eder (arkada kubectl'i call ederek çalışır) ve sadece Helm tanımlamaları ile daha kolay Kubernetes ortamına deploy yapabilmemizi sağlar. 

Server-side tarafta Helm'e ait özel bir kurulum yapılması gerekmemektedir.

# Bitnami
Bitnami projesi altında bulut sistemler, Docker, Helm gibi sistemler için hazır sanal makine veya container'lar sunulmaktadır. Bunları sunarkende her platform için ortak ayarlar sunulmaktadır. Bu şekilde bir kere yapılan ayarlar tüm ortamlarda kolay taşınabilirlik sağlamaktadır. kaynak: https://hub.docker.com/r/bitnami/mariadb "Bitnami containers, virtual machines and cloud images use the same components and configuration approach - making it easy to switch between formats based on your project needs.".

# Kustomize
Burada çok basit bir anlatım mevcut: https://github.com/kubernetes-sigs/kustomize/tree/master/examples/helloWorld

Aşağıdaki gibi bir dosya yapısı yaratıyoruz. production ve uat-1 farklı ortamlarımız olsun. Sadece farklılıkları "overlays" dizini altına koyuyoruz.

```
/project-directory
├── base
│   ├── configMap.yaml --> Kubernetes'in native tanıdığı dosya biçimi
│   ├── deployment.yaml --> Kubernetes'in native tanıdığı dosya biçimi
│   ├── kustomization.yaml --> Kustomize'ın tanıdığı dosya biçimi
│   └── service.yaml --> Kubernetes'in native tanıdığı dosya biçimi
└── overlays
    ├── production
    │   ├── deployment.yaml --> Kubernetes'in native tanıdığı dosya biçimi
    │   └── kustomization.yaml --> Kustomize'ın tanıdığı dosya biçimi
    └── uat-1
        ├── kustomization.yaml --> Kustomize'ın tanıdığı dosya biçimi
        └── map.yaml --> Kubernetes'in native tanıdığı dosya biçimi
```

Artık direk komut satırından ilgili ortama deploy alabiliriz:

```sh
OVERLAYS=/project-directory/overlays/production
kustomize build $OVERLAYS/production
kustomize build $OVERLAYS/production | kubectl apply -f -
```

# Minikube vs K3s vs KIND
KIND, Kubernetes'i Docker içerisinde çalıştıran bir projedir. minikube, VM'de değilde, "Docker" driver'ı ile initialize edilirse, "KIND" projesinden yararlanıyor.

Minikube, saf Kubernetes'i olduğu gibi kullanıyor. sadece, initializer Kubectl ve Kubeadm'yi wrap ediyor ve kurulumları spesific konfig'ler ile gerçekleştiriyor.

K3s, resmi Github sayfasındaki Readme dosyasında yazana göre: https://github.com/k3s-io/k3s saf Kubernetes'i upstream olarak kullanıyorlar fakat patch uygulayarak şu gibi farklılıklar uygulamışlar:
- bazı özellikleri silinmiş:
  - In-tree storage drivers
  - In-tree cloud provider
- ortalama 1000 satırlık code depğişikliği var
- var olan özellikler değiştiriliyor:
  - Etcd3 yerine Sqlite3 kullanılmış.
- ek özellikler içeriyor:
  - Managing the TLS certificates of Kubernetes components
  - Managing the connection between worker and server nodes
  - Auto-deploying Kubernetes resources from local manifests in realtime as they are changed.
