# Docker Nedir?

Docker, yazılım geliştiriciler ve sistemciler için geliştirilen açık kaynaklı bir sanallaştırma platformudur.[1]
Docker öncesinde sanallaştırma işlem sistemi üzerine farklı işletim sistemleri kurmak suretiyle yapılıyordu. Fakat Docker ile birlikte tek bir işletim sistemi üzerinde farklı işletim sistemi kurmaya gerek kalmadan birbirinden izole bir şekilde çalışabilen konteynerler oluşturabiliyorsunuz. Konteyner ile sanal makineler arasında farkı aşağıdaki görüntüde de görebilirsiniz.

![alt text](https://blogs.bmc.com/wp-content/uploads/2018/07/containers-vs-virtual-machines.jpg "Sanal Makineler vs Konteyner")

<div align="center" style="font-size:10px">
<a href="https://www.bmc.com/blogs/containers-vs-virtual-machines/" target=_blank>Görüntü Kaynağı</a>
</div>

Görüntüde gördüğünüz gibi sanal makinelerde (Virtual Machines) her bir uygulama için farklı işletim sistemi kurulmuş. Buna karşılık konteyner teknolojisinde (Containers) bir tane işletim sistemi var, diğer işletim sistemlerin yerini konteyner motoru alıyor.

Ben giriş kısmını burada bitiriyorum. Docker tarihçesi hakkında ve daha detaylı bilgi için [bu linki](https://gokhansengun.com/docker-nedir-nasil-calisir-nerede-kullanilir/) kullanarak Gökhan Şengün'ün sitesinde yer alan yazı dizisini okuyabilirsiniz.
***

## Terminoloji

**Image:** Kuruluma hazır dosya. Windows işletim sistemine sahip bilgisayara program kurmamıza yarayan .exe dosyası gibi düşünebilirsiniz.

**Docker Container:** İmajların çalıştırılmış veya durdurulmuş halidir. Windows işletim sistemine sahip bilgisayara kurduğumuz programın kendisi gibi düşünebilirsiniz.

**Docker Hub (Registry):** İmajlarımızı tuttuğumuz ve başkalarıyla paylaşmamızı sağlayan depo.

**Docker File:** İmajın nasıl kurulacağının yazılı olduğu bir dizi komut içeren dosya.

**Docker CLI:** Kullanıcının konteyner motoru (Docker Engine) ile haberleşmesini sağlayan arayüz.

**Docker Daemon:** Docker API (Docker CLI'dan) isteklerini alır. Konteyner, imaj, ağ ve volume bileşenlerini yönetir. Ayrıca sistemdeki diğer daemonlar ile iletişime geçerek Docker servislerini kontrol eder.

***

## Komutlar

### A - Genel Komutlar

```bash
docker version
```

Bu komut docker client ve server hakkında bilgiler verir. Client ve Server'ın hangi işletim sistemine ve hangi docker sürümüne sahip olduğu gibi bilgileri gösterir.

```bash
docker search
```

Docker Hub üzerinde arama yapmayı sağlar. Örneğin; `docker search nginx` şeklinde bir komut yazarsanız nginx imajı için Docker Hub üzerinde arama yapacaktır.
***

### B - İmaj Komutları

```bash
docker images

# -q => Sadece imaj Id'lerini gösterir
```

Bu komut sistemdeki imajları listelemek için kullanılır. Aynı zamanda onunla aynı işi gören `docker image ls` komutu da vardır.

```bash
docker image pull
```

Bu komut Docker Hub üzerinde yer alan imajı sistemimize indirir.
İmajın adının sonuna herhangi bir versiyon veya etiket (tag) belirtilmezse "latest" etiketine sahip imaj indirilecektir. Örneğin; `docker pull image nginx` yazdığımız zaman bunun karşılığı `docker pull image nginx:latest` şeklinde olacaktır. "Latest" kelime manası olarak en son anlamına gelse dahi her zaman son sürümü getireceğini **garanti etmez.** Bu bir etikettir veya imajı üreten kişi hangi imaja bu etiketi verdiyse o indirilecektir. Eğer özel bir tag istiyorsak şu latest yazan yere istediğimiz etiketi yazmamız gerekiyor `docker pull image nginx:stable` gibi. Nginx için bütün etiketlere [buradan](https://hub.docker.com/_/nginx?tab=tags) ulaşabilirsiniz.

```bash
docker image inspect
```

Bu komut image hakkında detaylı bilgi almak istediğimiz zaman kullanılan bir komut. Kaç tane katmandan (layers) oluşuyor. Desteklediği işletim sistemleri vs. gibi bir çok konu hakkında bilgi sahibi olmamızı sağlıyor. Buradaki katman bölümü hakkında özet geçmek gerekirse; her imaj temel bir katmandan oluşur. Bu temel katman [CentOS](https://www.centos.org/) olsun. Bunun üzerine nginx kurduk. Katman sayısı iki oldu. Birde dotnet kurduk üç katmanlı bir image oldu.

```bash
docker image rmi

# -f => Silmek için zorla. Eğer silmek istediğimiz imaj başka bir konteyner tarafından kullanılıyorsa silmeye zorlamak için kullanılır.

```

Bu komut imaj silmek için kullanılır. Daha genel kullanıma sahip olan `docker rmi` aynı işi görmektedir.

***

### C - Konteyner Komutları

```bash
docker ps

# -a => Çalışan çalışmayan bütün uygulamaları getirir
```

Bu komut çalışan işlemleri yani uygulama ve konteynerleri gösterir. "ps" Linux işletim sisteminden gelmektedir. "Process Status" kelimesinin kısaltmasıdır.

```bash
docker pull
```

Docker Hub üzerindeki bir imajı yerel diskimize indirmeyi sağlar. Örneğin; `docker pull jenkins` şeklinde bir komut yazarsanız jenkins imajını  Docker Hub'dan bilgisayarımıza indirecektir.

```bash
docker run

# -p, => (Port) Port belirler -p 89:80 yazdığımızda ilk kısım (89) bizim bilgisayarımızda konteynere ulaşacağımız portu ifade eder.

# --interactive, -i => (Interactive) Konteyneri sadece komut satırı açık olduğu sürece ayakta tutar. Interaktif modu aktifleştirir. Bu komutla ayağa kaldırdığımız bir konteynerin çalışmaya devam etmesini istiyorsak "CTRL + PQ" tuşlarına basarak arka planda çalışmasını devam ettirebiliriz.

# --tty, -t => (TTY) Konteynerle iletişime geçebileceğimiz sanal bir terminal oluşturur. Genellikle `-i` parametresiyle  beraber kullanılır.

# --detach, -d => (Detach) Konteyneri arka planda ayağa kaldırır. Genellikle tek başına kullanılabilir. Fakat bazı durumlarda -it komutu ile beraber kullanılmak zorunda kalabilirsiniz. İmajın ayağa kaldırma komutu bir shell veya bash ise konteyner ayağa kalkmayacaktır.
```

Docker Hub üzerindeki bir imajı yerel diskimizde arar eğer imajı bulamazsa; imajı indirir sonra konteyner ayağa kaldırır. `docker pull` ile arasında fark `docker pull` konteyneri ayağa kaldırmaz sadece indirir. Bu komut konteyner ayağa kaldırır.

`-it` komutunun ne zaman birlikte kullanılması konusunda detaylı bilgiye [burada](https://stackoverflow.com/a/41918607/2336116) verilen cevaptan erişebilirsiniz.

```bash
docker container rm

# -f => Silmek için zorla. Eğer silmek istediğimiz konteyner  kullanılıyorsa silmeye zorlamak için kullanılır.
```

Bu komut konteyner silmek için kullanılır.  Daha genel kullanıma sahip olan `docker rm` aynı işi görmektedir.

```bash
docker container export
```

Bu komut bir konteynerin dosya sistemini tar arşivi olarak yerel sisteme kaydeder. `docker container export [ContainerName] -o C:/ExportedContainer.tar`

```bash
docker container exec
```

Bu komut çalışan bir konteyner üzerinde komut çalıştırmanızı sağlar. Mesela bir ubuntu konteyneri var bunda bash çalıştırmak için   `docker container exec [ContainerName] -it /bin/bash`

```bash
docker container kill
```

Bu komut çalışan bir veya birden konteyneri durdurur. Bu komutla durdurulan konteynerler `docker container ps` komutunda **görünmez**.

```bash
docker container stop
```

Bu komut çalışan bir veya birden konteyneri durdurur. `docker container kill` yakın görevi görmektedir. Aradaki fark `stop` komutunda varsayılan 10 saniye kuralı vardır. Yani işlemi durdurmadan önce 10 saniye bekler sonra durdurur (Graceful shutdown). Kıyaslama için daha fazla bilgi [buraya](https://www.memogeeks.com/2018/11/kill-stop-and-pause-docker-commands.html#summary) göz atabilirsiniz. Bu komutlarla durdurulan konteynerler `docker container ps` komutunda **görünmez**.

```bash
docker container pause
```

Bu komut çalışan bir veya birden konteyner üzerindeki sistem süreçlerini geçici olarak durdurur. Bu komutla durdurulan konteynerler `docker container ps` komutunda görünür.

```bash
docker container unpause
```

Bu komut daha önceden geçici olarak durdurulan konteyneri çalışmasını devam ettirir.

```bash
docker container stats
```

Bu komut çalışan konteynerlerin kullandığı işlemci ram gibi bilgileri anlık olarak gösterir. Sonuna herhangi bir konteyner Id veya ismi eklenirse sadece onunla ilgili durumu gösterir.

```bash
docker container restart
```

Bu komut bir veya birden fazla konteyneri yeniden başlatır.

```bash
docker container port
```

Bu komut port veya özel mapping işlemlerini gösterir.

```bash
docker container prune
```

Bu komut durdurulmuş olan bütün konteynerleri sistemden kaldırır.

```bash
docker container rename
```

Bu komut mevcutta olan bir konteynerin adını değiştirir. `docker container rename [KonteynerId] [Yeniİsim]`

```bash
docker container top
```

Bu komut konteynerde çalışan süreçleri listeler.

```bash
docker container attach
```

Bu komut yukarıda bahsettiğim `CTRL + PQ` kullanılarak askıya alınmış bir konteynere yeniden bağlanmayı sağlar.

```bash
FOR /f "tokens=*" %i IN ('docker ps -aq') DO docker stop %i && docker rm %i
```

Bu komut Windows komut satırını kullanarak bütün konteynerleri kaldırma işlemini yapar. Bu komutun bash karşılığı `docker container rm -f $(docker container ls -aq)` şeklindedir.

```bash
FOR /f "tokens=*" %i IN ('docker images --format "{{.ID}}"') DO docker rmi -f %i
```

Bu komut Windows komut satırını kullanarak bütün imajları kaldırma işlemini yapar. Bu komutun bash karşılığı `docker rmi -f $(docker images -q)` şeklindedir.

[Kaynak1](https://linuxize.com/post/how-to-remove-docker-images-containers-volumes-and-networks/#stop-and-remove-all-containers) [Kaynak2](https://gist.github.com/daredude/045910c5a715c02a3d06362830d045b6)

### Volume Yönetimi

Bir konteyner oluşturduğunuzda bütün bilgileri o konteyner içinde tutulur ve siz konteyneri sildiğinizde içindeki dosyalar hep birlikte silinir. Peki ya konteynerde yaptığınız işlemlerde oluşturduğunuz dosyaların silinmesini istemiyorsanız? O zaman `volume` parametresi devreye giriyor. Konteyneri ayağa kaldırırken volume parametresini belirttiğiniz zaman siz konteyneri sildiğiniz zaman volume silinmeyecektir ve dosyalarınız korunacaktır. Volume dosyaları `/var/lib/docker/volumes/` bu dizinde tutulur bu dizine ulaşıp konteynerin kullandığı dosyaları görebilirsiniz. **Bu durumda özel bir koşul var bu yola eğer Docker'ı Windows makinasında kullanıyorsanız ulaşamazsınız. Çünkü yarattığınız volume’ler C:\Users\Public\Documents\Hyper-V\Virtual Hard Disks\MobyLinuxVM.vhdx dizininde oluşacak ve linux’taki gibi dizine erişip çalışamayacaksınız.** [2]

```bash
docker volume create --name volumeTest
```

Bu komut "volumeTest" adında bir volume oluşturur.

```bash
docker volume rm volumeTest
```

Bu komut "volumeTest" adındaki volume'u siler. Eğer volume bir konteyner tarafından kullanılıyorsa `-f` komutuyla beraber yazmanız gerekecektir.

```bash
docker volume inspect volumeTest
```

Bu komut "volumeTest" adındaki volume hakkında detaylı bilgiler verir.

```bash
docker run -d -p 8080:80 -v volumeTest:/usr/share/nginx/html/ nginx
```

Bu komut "volumeTest" adındaki volume'u nginx'in html klasörüne bağlar. Nginx ayağa kalkarken bu klasördeki index.html dosyasına bakmaktadır.  "/usr/share/nginx/html/" bu yolu nginx'in Docker Hub'da bulunan sayfasından bulabilirsiniz.

```command
docker run -d -p 8080:80 -v D:\VolumeTest:/usr/share/nginx/html/ nginx
```

Bu komut Windows 10 makinasında çalışanlar için "D:\VolumeTest" adındaki volume'u nginx'in html klasörüne bağlar. Nginx ayağa kalkarken bu klasördeki index.html dosyasına bakmaktadır. "D:\VolumeTest"  buraya index.html dosyası eklerseniz nginx ayağa kalkarken sizin verdiğiniz dosyası okuyacaktır.

### Kaynak Yönetimi

```bash
docker run -d --cpu-shares [İstenilen Değer] [İmajAdı]
```

Kısaltması `-c` olan bu komut toplam işlemcinin ne kadarını kullanacağını ifade eder. Örnek verecek olursak `docker run -d --cpu-shares 512 nginx` şeklinde bir komut yazdık. Belirtilmezse varsayılan değer olarak **1024** değerini almaktadır. Tek bir konteyner olduğu durumda bir bir şey ifade etmeyecektir. Çünkü tek konteyner bütün yükü karşılayacak doğal olarak bütün işlemciyi kullanacaktır. Fakat şöyle bir şey olursa; 2048,1024 ve 512 şeklinde üç tane konteyner ve isimleri sırasıyla A,B ve C olsun işlemci kullanımı %100'e ulaştığında karşılayacakları yük miktarının hesaplamasını şu şekilde yapabiliriz:

En küçük birim 512 diğer sayı değerlerini 512'ye bölelim.

A => 2048 / 512 = 4 Birim

B => 1024 / 512 = 2 Birim

C => 512 / 512 = 1  Birim

Toplamda **7 birim** oldu. O zaman şöyle olacak **100 / 7= 14.2**
Her birim için % 14.2'lik bir yük karşılama olacak. Sonuç olarak;

A İsimli Konteyner: 14.2 x 4 = % 56.8

B İsimli Konteyner: 14.2 x 2 = % 28.4

C İsimli Konteyner: 14.2 x 1 = % 14.2

Toplam yükü bu şekilde paylaşacaklardır. Ben yuvarlayarak yazdığımdan toplamda %100 yapmayabilir amacım nasıl hesaplandığını göstermekti. Bu konuda daha fazla bilgi için Marek Goldmann tarafından [burada](https://goldmann.pl/blog/2014/09/11/resource-management-in-docker/#_cpu) yazılan yazıya göz atabilirsiniz.

```bash
docker run -d --cpus [Çekirdek Sayısı] [İmajAdı]
```

Bu komut yine işlemci sınırlamak için kullanılır. Ama diğerinde yük bazlı sınırlama varken bu komutta çekirdek bazlı bir sınırlama söz konusu. Varsayılan değeri sıfırdır. Bu da limit olmaması anlamına gelir. `docker run --cpus 2 jenkins` şeklinde bir komut yazarsak jenkins konteynerinin kullanacağı işlemciyi iki çekirdekle sınırlamış olduk.

```bash
docker run -d --memory [İstenilen Değer] [İmajAdı]
```

Kısaltması `-m` olan bu komut toplam RAM miktarının ne kadarını kullanacağını ifade eder. Dört farklı birim kullanarak boyut verebiliriz. Atanabilecek minimum değer 4 MB'tır. Bunlar `b, k, m, g` açılımları bytes, kilobytes, megabytes, ve gigabytes. Örnek verecek olursak `docker run -d --memory 512m nginx` şeklinde bir komut yazdık. Nginx'e 512 megabytes hafıza vermiş olduk.

```bash
docker container update  --memory [İstenilen Değer] [İmajAdı]
```

Bu komut yukarıda tanımladığımız memory veya cpu gibi bilgilerin güncellemesini sağlar. `docker container update --memory 300m` gibi örnek bir sorgu yazabiliriz.

### Ağ (Network) Yönetimi

Bu bölümde konteynerin birbirleri arasında iletişiminden bahsedeceğiz. Docker ilk kurulumda varsayılan olarak 3 farklı ağ (network) tipi ile gelmektedir. Bunlar *bridge*, *host*, *none*  şeklinde sıralanabilir. Varsayılan olarak kurulu  gelmeyen *overlay* ve *macvlan* tipinde 2 tane daha tip mevcuttur. Eğer bunlarda ihtiyacınızı görmüyorsa [buradan](https://hub.docker.com/search?category=network&q=&type=plugin) docker hub üzerinde yazılan diğer ağ tipi eklentilerine ulaşabilirsiniz. [3]

![Alt text](/Images/Docker-Network-Ls.png?raw=true "Docker Ağ Tipleri")

Burada görüldüğü üzere Network ID, Name, Driver, Scope şeklinde sütunlar var. Bunları açıklamaya çalışalım.

**Network ID:** Ağ arayüzünün kimliğini ifade eder.

**Name:** Ağ arayüzünün adını ifade eder.

**Driver:** Ağ arayüzünün hangi tipte olduğunu ifade eder.

**Scope:** Ağ kapsamını ifade eder. Scope değer 3 farklı değer alabilir. Local, Global veya Swarm. [4]

Farklı bir tip belirtilmediği sürece varsayılan olarak bridge driver kullanılır.

Şimdi de ağ sürücü (Network Drivers) tiplerini tanıyalım. [5]

**Bridge:** Varsayılan ağ sürücüsüdür. Eğer aksi belirtilmezse oluşan konteyner bu sürüyü kullanarak oluşur. IP, Subnet ve Gateway adresi otomatik olarak konteynere atanır. Bu modu kullanan bir konteynerin dış dünya ile bağlantısı olmaz. Bunu yapabilmek için `-p` komutu ile port yönlendirme yapılması lazım. Varsayılan kullanıcı tarafından tanımlanmayan bir bridge ağ sürüsünü kullanarak oluşan konteynerler birbirleriyle iletişime geçebilir. **Bu tipin kullanılacağı en ideal yer; aynı Docker Host üzerinde konteynerin birbiriyle iletişime geçmesini istediğimiz senaryolarda kullanılır.** [6]

#### Kullanıcı Tanımlı Bridge vs Varsayılan Bridge

1. Normal şartlarda konteynerler birbiriyle IP üzerinde haberleşir. Fakat eğer konteynerlerin onlara atadığımız isimler üzerinden haberleşmesini istiyorsak kendi ağımızı oluşturmamız gerekiyor. Bu ağ varsayılan olarak belirtilen bridge ağ tipinde olsa dahi oluşturmamız gerekiyor. Eğer bunu oluşturmak istemezsek `link` komutuyla konteyneri göstermemiz gerekiyor. Fakat bu çok tercih edilen yöntem değildir.

2. Kullanıcı tanımlı ağ daha izole çalışmamızı sağlar. Şöyle ki; bir host içerisinde oluşturduğumuz bütün konteynerler varsayılan olarak aynı ağa ekleneceğinden yanlışlıkla birbiri arasında iletişim olmaması gereken konteynerler haberleşebilir.

3. Kullanıcı tanımlı ağa konteyner eklemek ve silmek daha kolaydır. Varsayılan ağa bir konteyner eklediğinizde onu ayırmak istersek o konteyneri silip farklı bir ağa bağlamanız gerekiyor.

4. Her kullanıcı tanımlı ağ kendi ayrı (Iptable vb.) konfigürasyonu ile gelir. Bunları her ağ için ayrı tanımlayabiliriz. Ama varsayılan olarak gelen ağda bu ayarları değiştirirseniz docker'ı yeniden başlatmanız gerekir.

**Bununla ilgili daha fazla bilgi için [burayı](https://docs.docker.com/network/bridge/) kullanabilirsiniz. Ben buradaki maddelerin bazılarını çevirmeye çalıştım.**

**Host:** Host makinenin IP ve port bilgileri konteyner ile paylaşılır. **Bu tipin kullanılacağı en ideal yer; bir konteynerin ağını Docker Host'tan ayırmak istemediğimiz ama diğer özelliklerin Docker Host'tan ayrılmasını istediğimiz senaryolarda kullanılır.** [7]

**None:** Bu ağ tipinin seçilmesi durumunda konteyner diğer konteynerlere ve dışarıya kapalı olur. **Genellikle özel bir ağ sürücüsü ile birlikte kullanılır.** [5]

**Overlay:** Birden fazla [Docker Daemon'u](/Docker-1#terminoloji) birbirine bağlanması durumunda kullanılan ağ sürücü tipidir. Genellikle farklı Docker Host'larda olan iki tane farklı konteyneri bağlamak için kullanılır. [8]

**Macvlan:** Konteynerlere fiziksel adres tanımlamak istediğimiz ve konteyneri ağımızda fiziksel bir makine olarak göstermek istediğimiz durumlarda kullanılan ağ tipidir. [9]

Bundan sonra nasıl bir ağ oluşturacağımızı ve yöneteceğimizi gösteren komutları görelim.

```bash
docker network create

# --driver => Arayüzün hangi tipte olacağını ifade eder. bridge, host vs.

# --subnet => CIDR formatında Subnet değerini ifade eder. 172.28.0.0/16 gibi. Docker varsayılan olarak 172.17.0.0/16 subnet'ini kullanmaktadır.

# --gateway => Subnet için Gateway adresini tanımlar. 172.28.5.254 gibi.

# --opt => Ek özellikleri tanımlamamızı sağlar. Mesela NAT'ı aktif etmek istersek --ip-masq=true şeklinde bir parametre geçmemiz gerekiyor.

```

Bu komut yeni bir ağ oluşturmamıza yardımcı olur. Aksini belirtmediğimiz sürece *bridge* modunda bir ağ oluşturacaktır. Bir parametresi mevcut. Onlara bakmak isterseniz [buradan](https://docs.docker.com/engine/reference/commandline/network_create/#specify-advanced-options) inceleyebilirsiniz.

```bash
docker network ls
```

Bu komut mevcutta olan ağları listeler.

```bash
docker network rm
```

Bu komut mevcutta olan ağı siler.

```bash
docker network connect [NetworkAdı] [KonteynerAdı]

# --ip Bu parametreyi belirtmezsek ip adresi arayüz tarafından otomatik olarak atanacaktır.
```

Bu komut bir konteyneri bir ağa bağlar.

```bash
docker network disconnect [NetworkAdı] [KonteynerAdı]

```

Bu komut bir konteyneri bir ağdan çıkartır.

**!!! Bu işlemleri yaparken konteynerin çalışıyor olması gerekmektedir.**

Şimdi bir uygulama yapalım. Senaryo şu şekilde olacak:

Dört tane Nginx konteynerimiz olacak. Bunların ikisi farklı bir ağa diğer ikisi farklı bir ağa bağlı olacak. Aynı ağ içinde olanların birbirine erişebildiğini göreceğiz. Farklı ağda olanların birbirine erişiminin olmadığını teyit edeceğiz.

**1. Adım** Ağlarımızı oluşturuyoruz.

İlk ağımızı oluşturuyoruz.

```bash
docker network create --driver bridge ilk_ag
```

İkinci ağı oluşturuyoruz.

```bash
docker network create --driver bridge ikinci_ag
```

Ağların başarılı bir şekilde oluşturulduğunu aşağıdaki komutu çalıştırarak ekranda gelen listede ağ isimlerini kontrol ediyoruz.

```bash
docker network ls
```

**2. Adım** Konteynerlerimizi oluşturuyoruz.

İlk konteynerimizi ve ikinci konteynerimizi "ilk_ag" isimli ağa bağlıyoruz. Bunları oluştururken direkt olarak ağa bağladık. Diğer iki konteynerleri oluşturup sonradan ağa bağlayacağız. Burada için [iputils](https://github.com/iputils/iputils) kurduğum bir imajı kullanacağız. Çünkü kurulumdan sonra içine girip diğer konteynerlere ping göndereceğiz. Sürekli gidip sürekli aynı paketleri kurmamak için bu şekilde hazır imajı kullanalım.

```bash
docker run -d --name nginx1 --network=ilk_ag omeraslan/nginx-iputils
```

```bash
docker run -d --name nginx2 --network=ilk_ag omeraslan/nginx-iputils
```

Burada sadece konteyner oluşturuyoruz sonradaki adımda ağa bağlayacağız.

```bash
docker run -d --name nginx3 omeraslan/nginx-iputils
```

"ikinci_ag" isimli ağa bağlıyoruz.

```bash
docker network connect ikinci_ag nginx3
```

Sonuncu konteyner için adımları tekrarlıyoruz

```bash
docker run -d --name nginx4 omeraslan/nginx-iputils
```

"ikinci_ag" isimli ağa bağlıyoruz.

```bash
docker network connect ikinci_ag nginx4
```

**3. Adım** Konteyner iletişimlerini kontrol ediyoruz

"nginx1" isimli konteynere bağlanıyoruz.

```bash
docker exec -it nginx1 /bin/bash
```

bağlandıktan sonra aşağıdaki komutu çalıştırarak diğer konteynerlere erişimini kontrol ediyoruz.

```bash
ping nginx2
```

"nginx2" isimli konteyner "nginx1" ile aynı ağda olduğundan sorunsuz erişecektir. Fakat `ping nginx3` yazarsak bağlantıda sorun yaşadığımızı göreceğiz.

Şimdi de "nginx3" isimli konteynere bağlanıyoruz.

```bash
docker exec -it nginx3 /bin/bash
```

bağlandıktan sonra aşağıdaki komutu çalıştırarak diğer konteynerlere erişimini kontrol ediyoruz.

```bash
ping nginx4
```

"nginx4" isimli konteyner "nginx3" ile aynı ağda olduğundan sorunsuz erişecektir. Fakat `ping nginx2` yazarsak bağlantıda sorun yaşadığımızı göreceğiz.

[1]: https://www.emrealadag.com/docker-nedir.html
[2]: https://medium.com/devopsturkiye/i%CC%87nceleme-1-docker-volume-971c41122d83
[3]:https://docs.docker.com/network/#network-drivers
[4]:https://docs.docker.com/engine/reference/commandline/network_ls/#filtering
[5]:https://docs.docker.com/network/
[6]:https://docs.docker.com/network/bridge/
[7]:https://docs.docker.com/network/host/
[8]:https://docs.docker.com/network/overlay/
[9]:https://docs.docker.com/network/macvlan/
