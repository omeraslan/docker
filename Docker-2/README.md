## Ağ (Network) Yönetimi

Bu bölümde konteynerin birbirleri arasında iletişiminden bahsedeceğiz. Docker ilk kurulumda varsayılan olarak 3 farklı ağ (network) tipi ile gelmektedir. Bunlar *bridge*, *host*, *none*  şeklinde sıralanabilir. Varsayılan olarak kurulu  gelmeyen *overlay* ve *macvlan* tipinde 2 tane daha tip mevcuttur. Eğer bunlarda ihtiyacınızı görmüyorsa [buradan](https://hub.docker.com/search?category=network&q=&type=plugin) docker hub üzerinde yazılan diğer ağ tipi eklentilerine ulaşabilirsiniz. [1] 

![Alt text](/Images/Docker-Network-Ls.png?raw=true "Docker Ağ Tipleri")

Burada görüldüğü üzere Network ID, Name, Driver, Scope şeklinde sütunlar var. Bunları açıklamaya çalışalım.

**Network ID:** Ağ arayüzünün kimliğini ifade eder.

**Name:** Ağ arayüzünün adını ifade eder.

**Driver:** Ağ arayüzünün hangi tipte olduğunu ifade eder.

**Scope:** Ağ kapsamını ifade eder. Scope değer 3 farklı değer alabilir. Local, Global veya Swarm. [2] 

Farklı bir tip belirtilmediği sürece varsayılan olarak bridge driver kullanılır. 

Şimdi de ağ sürücü (Network Drivers) tiplerini tanıyalım. [3]

**Bridge:** Varsayılan ağ sürücüsüdür. Eğer aksi belirtilmezse oluşan konteyner bu sürüyü kullanarak oluşur. IP, Subnet ve Gateway adresi otomatik olarak konteynere atanır. Bu modu kullanan bir konteynerin dış dünya ile bağlantısı olmaz. Bunu yapabilmek için `-p ` komutu ile port yönlendirme yapılması lazım. Varsayılan kullanıcı tarafından tanımlanmayan bir bridge ağ sürüsünü kullanarak oluşan konteynerler birbirleriyle iletişime geçebilir. Kullanıcı tarafından oluşturulan bridge ağ ile varsayılan olarak oluşan ağ arasında farklı aşağıda anlatmaya çalışacağım. 

**Host:** Host makinenin IP ve port bilgileri konteyner ile paylaşılır. 


[1]:https://docs.docker.com/network/#network-drivers
[2]:https://docs.docker.com/engine/reference/commandline/network_ls/#filtering
[3]:https://docs.docker.com/network/
[4]:https://docs.docker.com/network/bridge/
[5]:https://docs.docker.com/network/overlay/
[6]:https://docs.docker.com/network/host/
[7]:https://docs.docker.com/network/macvlan/
