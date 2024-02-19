---
title: "Linux Sistemlere NFS Sunucusu ve İstemcisi Nasıl Kurulur"
date: 2024-02-19
author: Ahmet Önder Moğol
url: /linux-nfs-server-client/
#image: images/2023-thumbs/xxx.jpg
categories:
  - Linux
tags: [Linux, NFS, Arch, Debian, Ubuntu]  
#draft: true
---


Linux'ta , **Samba** ve **NFS** depolama ve dosyaları ağ üzerinden paylaşmak için kullanılan dosya paylaşım protokolleridir.

**Samba** , Client-Server (istemci-sunucu) mimarisinde çalışan açık kaynaklı uygulama paketidir. Linux, Windows ve macOS işletim sistemlerinde kesintisiz dosya paylaşımına olanak tanıyan platformlar arası uyumluluk sunar. Linux sisteminde barındırılan bir dosya paylaşımına çeşitli platformlardan erişilebilir.

**Ağ Dosya Paylaşımı**'nın kısaltması olan **NFS** yaygın olarak kullanılan bir dosya paylaşım protokolüdür . Tıpkı Samba gibi , istemci-sunucu modeli üzerinde çalışır ve kullanıcının bir ağ üzerinden birden fazla uzak istemci kullanıcısıyla dizinleri ve dosyaları paylaşmasına olanak tanır.

Bu kılavuzda NFS sunucusunun ve istemcisinin Fedora, CentOS Stream, Rocky Linux ve AlmaLinux gibi RHEL tabanlı dağıtımlara nasıl kurulacağını kontrol edeceğiz .

## NFS Hizmetleri
Şu anda NFS'nin 3 sürümü bulunmaktadır; en sonuncusu internet üzerinden ve güvenlik duvarı üzerinden çalışabilme gibi özellikleri içeren NFSv4'tür . Ayrıca NFSv3 ve en eski protokol olan NFSv2 bulunmaktadır.

NFS hizmeti , NFS Sunucusu ve istemcisinden oluşur . NFS sunucusu aşağıdaki anahtar dosyalardan oluşur:

**nfs-server** – Bu, istemci sistemlerinin NFS ile paylaşılan dosyalara erişmesine olanak tanır.
**rpcbind** – RPC programlarını evrensel adreslere dönüştürür.
**nfs-idmap** – Kullanıcı ve grup kimliklerinin isimlere, kullanıcı ve grup adlarının kimliklere çevrilmesini gerçekleştirir.
**portmap** – Bu, RPC program numaralarını IP bağlantı noktası numaralarına dönüştüren bir sunucudur.
**nfslock** – NFS sunucusunun çökmesi durumunda nfslock gerekli RPC işlemlerini başlatır.

### NFS Yapılandırma Hizmetleri
NFS için önemli yapılandırma dosyalarından bazıları şunlardır :

/etc/exports – Uzak kullanıcılar tarafından dışa aktarılacak ve erişilecek dosya sistemlerini veya dizinleri belirleyen ana yapılandırma dosyası.
/etc/fstab – Bu, monte edilmiş bölümlerin girişlerini içeren bir dosyadır. NFS'de dosya, kalıcı olarak bağlanan ve yeniden başlatmaya devam edebilen NFS paylaşım dizinlerinin veya dosya sistemlerinin girişlerini içerir.
/etc/sysconfig/nfs – RPC hizmetlerinin çalıştırılması sırasında ihtiyaç duyulan bağlantı noktalarını tanımlar.
NFS Sunucusu ve İstemci Kurulumu
NFS paylaşımlarını kurmak için en az iki Linux / Unix makinesine ihtiyacımız olacak . Bu eğitimde iki sunucu kullanacağım.

NFS Sunucusu – IP 10.128.15.213 ile RHEL 9
NFS İstemcisi – IP 10.128.15.214 ile RHEL 9
NFS'yi Sunucu ve İstemciye Yükleme
Başlamak için her iki düğümde de ( NFS sunucusu ve istemci) oturum açmanız ve NFS hizmetlerini yüklemeniz gerekir. Öncelikle paket bilgilerini gösterildiği gibi güncelleyin. Aşağıdaki dnf komutu aynı zamanda tüm heyecan verici paketleri en son sürümlerine yükseltecektir.

```console
$ sudo dnf update
```
Güncelleme tamamlandıktan sonra devam edin ve gerekli NFS hizmetlerini yükleyin.

$ sudo dnf rpcbind nfs-utils -y'yi yükle
NFS'yi Linux'a yükleyin
NFS'yi Linux'a yükleyin
Bir sonraki adım, gösterildiği gibi NFS hizmetlerini etkinleştirmektir .

$ sudo systemctl nfs sunucusunu etkinleştir
$ sudo systemctl rpcbind'i etkinleştir
NFS hizmetlerini de başlattığınızdan emin olun .

$ sudo systemctl nfs sunucusunu etkinleştir
$ sudo systemctl rpcbind'i etkinleştir
Tüm NFS hizmetlerinin çalıştığını doğrulamak çok önemlidir .

$ sudo systemctl durumu nfs sunucusu
$ sudo systemctl durumu rpcbind
NFS Durumunu Kontrol Edin
NFS Durumunu Kontrol Edin
Gelen NFS servislerine izin verecek şekilde güvenlik duvarını da aşağıdaki gibi yapılandırmayı unutmayın .

$ sudo güvenlik duvarı-cmd --permanent --add-service={nfs,rpc-bind,mountd}
$ sudo güvenlik duvarı-cmd --yeniden yükle
NFS Paylaşım Dizini Oluşturun
Tüm NFS hizmetleri beklendiği gibi kurulup çalıştığında, ağdaki NFS istemcilerinin erişeceği dosyaları içeren NFS paylaşım dizinini oluşturmanın zamanı geldi.

Bu durumda home dizinimizde my_nfsshare adında bir NFS paylaşım dizini oluşturacağız .

$ mkdir -p /home/tecmint/my_nfsshare
Daha sonra dizin izinlerini atayın. Gösterim amacıyla, NFS istemcilerine okuma, yazma ve yürütme izinlerini atayacak genel izinler atayacağız.

$ sudo chmod 777 -R /home/tecmint/my_nfsshare
NFS Paylaşım Dizini Oluşturun
NFS Paylaşım Dizini Oluşturun
NFS Paylaşım Dizinini Dışa Aktarma
Bir sonraki adım NFS paylaşım dizinini dışa aktarmaktır. Bunu başarmak için /etc/exports dosyasına bir giriş yapmamız gerekiyor. Bu nedenle dosyaya tercih ettiğiniz metin düzenleyiciyi kullanarak erişin . Bu durumda Vim editörünü kullanacağız .

$ sudo vim /etc/exports
Aşağıdaki girişi ekleyin. Server-ip'i NFS sunucunuzun IP adresiyle değiştirdiğinizden emin olun.

/home/tecmint/my_nfsshare sunucu-ip/24(rw,no_root_squash)
Son olarak NFS paylaşım dizinini veya dosya sistemini dışa aktarın.

$ sudo ihracat -rv
NFS Paylaşım Dizinini Dışa Aktar
NFS Paylaşım Dizinini Dışa Aktar
NFS paylaşımlarını görüntülemek için aşağıdaki komutu çalıştırın.

$ showmount -e localhost
NFS Paylaşım Dizinini Görüntüle
NFS Paylaşım Dizinini Görüntüle
NFS İstemcisini Yapılandırma
Bu alıştırmanın geri kalan aşaması, NFS istemcisini paylaşılan dizine erişecek şekilde yapılandırmaktır. Öncelikle, NFS sunucusunda dışa aktarma listesini veya NFS paylaşımlarını görüntüleyebildiğinizi doğrulayın.

# showmount -e 10.128.15.213
NFS Paylaşım Dizinini Listele
NFS Paylaşım Dizinini Listele
Bir sonraki adım NFS paylaşımını sunucudan istemciye bağlamaktır. Bunun için öncelikle bir mount dizini oluşturmamız gerekiyor. Bu durumda nfs_backup adında bir dizin oluşturacağız .

# mkdir nfs_backup
Daha sonra NFS paylaşımını kök ana dizinde az önce oluşturduğumuz mount dizinine bağlayacağız.

# mount -t nfs 10.128.15.213:/home/tecmint/my_nfsshare ~/nfs_backup 
NFS paylaşımını sürdürmek için /etc/fstab dosyasını düzenleyin.

# vim /etc/fstab
Daha sonra aşağıdaki girişi ekleyin.

10.128.15.213:/home/tecmint/my_nfsshare /root/nfs_backup nfs varsayılanları 0 0
NFS Paylaşım Dizinini Bağlayın
NFS Paylaşım Dizinini Bağlayın
Yapılandırma dosyasını kaydedin ve çıkın.

NFS Kurulumunu Test Etme
Son adım, NFS kurulumunun beklendiği gibi çalışıp çalışmadığını doğrulamaktır. Sunucuda birkaç dosya oluşturacağız ve bunların NFS istemci tarafında kullanılabilirliğini doğrulayacağız.

Sunucu tarafında dosyaları NFS paylaşım dizininde oluşturacağız.

$ sudo touch my_nfsshare/file{1..4}.txt
Dosyaların oluşturulduğunu doğrulamak için ls komutunu uygulayacağız :

$ ls -l benim_nfspaylaşımım/
NFS Paylaşım Dizinini Doğrulayın
NFS Paylaşım Dizinini Doğrulayın
İstemci tarafına dönersek, aşağıdaki çıktıda görüldüğü gibi herhangi bir hizmetin yenilenmesi veya herhangi bir hizmetin yeniden başlatılması gerekmeden dosyaların bağlama dizininde mevcut olduğunu doğrulayın.

$ ls -l nfs_backup/
NFS Paylaşım Dosyalarını Kontrol Edin
NFS Paylaşım Dosyalarını Kontrol Edin
NFS Bağlantısını Çıkarma
Sisteminizde takılı dizine artık ihtiyacınız yoksa, aşağıdaki umount komutunu kullanarak bunların istemci tarafından bağlantısını kesebilirsiniz :

$ umount ~/nfs_backup
NFS Paylaşım Komutları
NFS için bazı önemli komutlar .

showmount -e – Yerel makinenizdeki mevcut paylaşımları gösterir
showmount -e ip adresi – Uzak sunucudaki mevcut paylaşımları listeler
showmount -d – Tüm alt dizinleri listeler
Exportfs -v – Sunucudaki paylaşılan dosyaların ve seçeneklerin listesini görüntüler
Exportfs -a – /etc/exports dosyasında listelenen veya verilen addaki tüm paylaşımları dışa aktarır
Exportfs -u – /etc/exports dosyasında listelenen veya adı verilen tüm paylaşımları dışa aktarır
Exportfs -r – /etc/exports dosyasını değiştirdikten sonra sunucunun listesini yeniler
Çözüm
Bu, NFS sunucusunun ve istemcisinin RedHat tabanlı dağıtımlara nasıl kurulacağına ilişkin kılavuzumuzu tamamlıyor . NFS servislerini sunucuya kurduk , bir NFS paylaşım dizini oluşturduk ve son olarak paylaşım dizinini istemciye bağladık. Son olarak sunucuda oluşturulan dosyaya client tarafından erişerek NFS kurulumunu doğruladık.

