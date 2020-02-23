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

***
## Komutlar

### A - Genel Komutlar
```
docker version
```
Bu komut docker client ve server hakkında bilgiler verir. Client ve Server'ın hangi işletim sistemine ve hangi docker sürümüne sahip olduğu gibi bilgileri gösterir.

```
docker search
```
Docker Hub üzerinde arama yapmayı sağlar. Örneğin; `docker search nginx` şeklinde bir komut yazarsanız nginx imajı için Docker Hub üzerinde arama yapacaktır.
***
### B - İmaj Komutları

```
docker images 

# -q => Sadece imaj Id'lerini gösterir
```
Bu komut sistemdeki imajları listelemek için kullanılır. Aynı zamanda onunla aynı işi gören `docker image ls` komutu da vardır.

```
docker image pull 
```
Bu komut Docker Hub üzerinde yer alan imajı sistemimize indirir.
İmajın adının sonuna herhangi bir versiyon veya etiket (tag) belirtilmezse "latest" etiketine sahip imaj indirilecektir. Örneğin; `docker pull image nginx` yazdığımız zaman bunun karşılığı `docker pull image nginx:latest` şeklinde olacaktır. "Latest" kelime manası olarak en son anlamına gelse dahi her zaman son sürümü getireceğini **garanti etmez.** Bu bir etikettir veya imajı üreten kişi hangi imaja bu etiketi verdiyse o indirilecektir. Eğer özel bir tag istiyorsak şu latest yazan yere istediğimiz etiketi yazmamız gerekiyor `docker pull image nginx:stable` gibi. Nginx için bütün etiketlere [ buradan ](https://hub.docker.com/_/nginx?tab=tags)ulaşabilirsiniz.

```
docker image inspect
```
Bu komut image hakkında detaylı bilgi almak istediğimiz zaman kullanılan bir komut. Kaç tane katmandan (layers) oluşuyor. Desteklediği işletim sistemleri vs. gibi bir çok konu hakkında bilgi sahibi olmamızı sağlıyor. Buradaki katman bölümü hakkında özet geçmek gerekirse; her imaj temel bir katmandan oluşur. Bu temel katman [CentOS](https://www.centos.org/) olsun. Bunun üzerine nginx kurduk. Katman sayısı iki oldu. Birde dotnet kurduk üç katmanlı bir image oldu. 

```
docker image rmi

# -f => Silmek için zorla. Eğer silmek istediğimiz imaj başka bir konteyner tarafından kullanılıyorsa silmeye zorlamak için kullanılır.

```
Bu komut imaj silmek için kullanılır. Daha genel kullanıma sahip olan `docker rmi` aynı işi görmektedir. 

***
### C - Konteyner Komutları
```
docker ps

# -a => Çalışan çalışmayan bütün uygulamaları getirir
```
Bu komut çalışan işlemleri yani uygulama ve konteynerleri gösterir. "ps" Linux işletim sisteminden gelmektedir. "Process Status" kelimesinin kısaltmasıdır. 

```
docker pull
```
Docker Hub üzerindeki bir imajı yerel diskimize indirmeyi sağlar. Örneğin; `docker pull jenkins` şeklinde bir komut yazarsanız jenkins imajını  Docker Hub'dan bilgisayarımıza indirecektir.

```
docker run 

# -p, => (Port) Port belirler -p 89:80 yazdığımızda ilk kısım (89) bizim bilgisayarımızda konteynere ulaşacağımız portu ifade eder.

# --interactive, -i => (Interactive) Konteyneri sadece komut satırı açık olduğu sürece ayakta tutar. Interaktif modu aktifleştirir. Bu komutla ayağa kaldırdığımız bir konteynerin çalışmaya devam etmesini istiyorsak "CTRL + PQ" tuşlarına basarak arka planda çalışmasını devam ettirebiliriz.

# --tty, -t => (TTY) Konteynerle iletişime geçebileceğimiz sanal bir terminal oluşturur. Genellikle `-i` parametresiyle  beraber kullanılır.

# --detach, -d => (Detach) Konteyneri arka planda ayağa kaldırır. Genellikle tek başına kullanılabilir. Fakat bazı durumlarda -it komutu ile beraber kullanılmak zorunda kalabilirsiniz. İmajın ayağa kaldırma komutu bir shell veya bash ise konteyner ayağa kalkmayacaktır. 

```
Docker Hub üzerindeki bir imajı yerel diskimizde arar eğer imajı bulamazsa; imajı indirir sonra konteyner ayağa kaldırır. `docker pull` ile arasında fark `docker pull` konteyneri ayağa kaldırmaz sadece indirir. Bu komut konteyner ayağa kaldırır. 

`-it` komutunun ne zaman birlikte kullanılması konusunda detaylı bilgiye [burada](https://stackoverflow.com/a/41918607/2336116) verilen cevaptan erişebilirsiniz.


```
docker container rm

# -f => Silmek için zorla. Eğer silmek istediğimiz konteyner  kullanılıyorsa silmeye zorlamak için kullanılır.

```
Bu komut konteyner silmek için kullanılır.  Daha genel kullanıma sahip olan `docker rm` aynı işi görmektedir. 

```
docker container export
```
Bu komut bir konteynerin dosya sistemini tar arşivi olarak yerel sisteme kaydeder. `docker container export [ContainerName] -o C:/ExportedContainer.tar`

```
docker container exec
```
Bu komut çalışan bir konteyner üzerinde komut çalıştırmanızı sağlar. Mesela bir ubuntu konteyneri var bunda bash çalıştırmak için   `docker container exec [ContainerName] -it /bin/bash`

```
docker container kill
```
Bu komut çalışan bir veya birden konteyneri durdurur. Bu komutla durdurulan konteynerler `docker container ps` komutunda **görünmez**.

```
docker container stop
```
Bu komut çalışan bir veya birden konteyneri durdurur. `docker container kill` yakın görevi görmektedir. Aradaki fark `stop` komutunda varsayılan 10 saniye kuralı vardır. Yani işlemi durdurmadan önce 10 saniye bekler sonra durdurur (Graceful shutdown). Kıyaslama için daha fazla bilgi [buraya](https://www.memogeeks.com/2018/11/kill-stop-and-pause-docker-commands.html#summary) göz atabilirsiniz. Bu komutlarla durdurulan konteynerler `docker container ps` komutunda **görünmez**.

```
docker container pause
```
Bu komut çalışan bir veya birden konteyner üzerindeki sistem süreçlerini geçici olarak durdurur. Bu komutla durdurulan konteynerler `docker container ps` komutunda görünür.

```
docker container unpause
```
Bu komut daha önceden geçici olarak durdurulan konteyneri çalışmasını devam ettirir. 

```
docker container stats
```
Bu komut çalışan konteynerlerin kullandığı işlemci ram gibi bilgileri anlık olarak gösterir. Sonuna herhangi bir konteyner Id veya ismi eklenirse sadece onunla ilgili durumu gösterir.

```
docker container restart
```
Bu komut bir veya birden fazla konteyneri yeniden başlatır.

```
docker container port
```
Bu komut port veya özel mapping işlemlerini gösterir.

```
docker container prune
```
Bu komut durdurulmuş olan bütün konteynerleri sistemden kaldırır.

```
docker container rename
```
Bu komut mevcutta olan bir konteynerin adını değiştirir. `docker container rename [KonteynerId] [Yeniİsim]`

```
docker container top
```
Bu komut konteynerde çalışan süreçleri listeler.

```
docker container attach
```
Bu komut yukarıda bahsettiğim `CTRL + PQ` kullanılarak askıya alınmış bir konteynere yeniden bağlanmayı sağlar.

```
FOR /f "tokens=*" %i IN ('docker ps -aq') DO docker stop %i && docker rm %i
```
Bu komut Windows komut satırını kullanarak bütün konteynerleri kaldırma işlemini yapar. Bu komutun bash karşılığı `docker container rm -f $(docker container ls -aq)` şeklindedir. 

```
FOR /f "tokens=*" %i IN ('docker images --format "{{.ID}}"') DO docker rmi -f %i
```
Bu komut Windows komut satırını kullanarak bütün imajları kaldırma işlemini yapar. Bu komutun bash karşılığı `docker rmi -f $(docker images -q)` şeklindedir. 

[Kaynak1](https://linuxize.com/post/how-to-remove-docker-images-containers-volumes-and-networks/#stop-and-remove-all-containers) [Kaynak2](https://gist.github.com/daredude/045910c5a715c02a3d06362830d045b6)

### Volume Yönetimi

Bir konteyner oluşturduğunuzda bütün bilgileri o konteyner içinde tutulur ve siz konteyneri sildiğinizde içindeki dosyalar hep birlikte silinir. Peki ya konteynerde yaptığınız işlemlerde oluşturduğunuz dosyaların silinmesini istemiyorsanız? O zaman `volume` parametresi devreye giriyor. Konteyneri ayağa kaldırırken volume parametresini belirttiğiniz zaman siz konteyneri sildiğiniz zaman volume silinmeyecektir ve dosyalarınız korunacaktır. Volume dosyaları `/var/lib/docker/volumes/` bu dizinde tutulur bu dizine ulaşıp konteynerin kullandığı dosyaları görebilirsiniz. **Bu durumda özel bir koşul var bu yola eğer Docker'ı Windows makinasında kullanıyorsanız ulaşamazsınız. Çünkü yarattığınız volume’ler C:\Users\Public\Documents\Hyper-V\Virtual Hard Disks\MobyLinuxVM.vhdx dizininde oluşacak ve linux’taki gibi dizine erişip çalışamayacaksınız.** [2]

```
docker volume create --name volumeTest
```
Bu komut "volumeTest" adında bir volume oluşturur. 

```
docker volume rm volumeTest
```
Bu komut "volumeTest" adındaki volume'u siler. Eğer volume bir konteyner tarafından kullanılıyorsa `-f` komutuyla beraber yazmanız gerekecektir.

```
docker volume inspect volumeTest
```
Bu komut "volumeTest" adındaki volume hakkında detaylı bilgiler verir.

```
docker run -d -p 8080:80 -v volumeTest:/usr/share/nginx/html/ nginx
```
Bu komut "volumeTest" adındaki volume'u nginx'in html klasörüne bağlar. Nginx ayağa kalkarken bu klasördeki index.html dosyasına bakmaktadır.  "/usr/share/nginx/html/" bu yolu nginx'in Docker Hub'da bulunan sayfasından bulabilirsiniz.

```
docker run -d -p 8080:80 -v D:\VolumeTest:/usr/share/nginx/html/ nginx
```
Bu komut Windows 10 makinasında çalışanlar için "D:\VolumeTest" adındaki volume'u nginx'in html klasörüne bağlar. Nginx ayağa kalkarken bu klasördeki index.html dosyasına bakmaktadır. "D:\VolumeTest"  buraya index.html dosyası eklerseniz nginx ayağa kalkarken sizin verdiğiniz dosyası okuyacaktır.


### Kaynak Yönetimi

```
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

```
docker run -d --cpus [Çekirdek Sayısı] [İmajAdı] 
```
Bu komut yine işlemci sınırlamak için kullanılır. Ama diğerinde yük bazlı sınırlama varken bu komutta çekirdek bazlı bir sınırlama söz konusu. Varsayılan değeri sıfırdır. Bu da limit olmaması anlamına gelir. `docker run --cpus 2 jenkins` şeklinde bir komut yazarsak jenkins konteynerinin kullanacağı işlemciyi iki çekirdekle sınırlamış olduk. 

```
docker run -d --memory [İstenilen Değer] [İmajAdı] 
```
Kısaltması `-m` olan bu komut toplam RAM miktarının ne kadarını kullanacağını ifade eder. Dört farklı birim kullanarak boyut verebiliriz. Atanabilecek minimum değer 4 MB'tır. Bunlar `b, k, m, g` açılımları bytes, kilobytes, megabytes, ve gigabytes. Örnek verecek olursak `docker run -d --memory 512m nginx` şeklinde bir komut yazdık. Nginx'e 512 megabytes hafıza vermiş olduk.


```
docker container update  --memory [İstenilen Değer] [İmajAdı] 
```
Bu komut yukarıda tanımladığımız memory veya cpu gibi bilgilerin güncellemesini sağlar. `docker container update --memory 300m` gibi örnek bir sorgu yazabiliriz.




[1]: https://www.emrealadag.com/docker-nedir.html
[2]: https://medium.com/devopsturkiye/i%CC%87nceleme-1-docker-volume-971c41122d83
