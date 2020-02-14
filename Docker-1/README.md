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

```
docker version
```
Bu komut docker client ve server hakkında bilgiler verir. Client ve Server'ın hangi işletim sistemine ve hangi docker sürümüne sahip olduğu gibi bilgileri gösterir.

```
docker search
```
Docker Hub üzerinde arama yapmayı sağlar. Örneğin; `docker search nginx` şeklinde bir komut yazarsanız nginx imajı için Docker Hub üzerinde arama yapacaktır.

```
docker ps

# -a => Çalışan çalışmayan bütün uygulamaları getirir
```
Bu komut çalışan işlemleri yani uygulama ve konteynerleri gösterir. "ps" Linux işletim sisteminden gelmektedir. "Process Status" kelimesinin kısaltmasıdır. 

```
docker pull
```
Docker Hub üzerindeki bir imajı yerel diskimize indirmeyi sağlar. Örneğin; `docker pull jenkins` şeklinde bir komut yazarsanız jenkins imajını  Docker Hub'dan bilgisayarımıza indirecektir.



[1]: https://www.emrealadag.com/docker-nedir.html
