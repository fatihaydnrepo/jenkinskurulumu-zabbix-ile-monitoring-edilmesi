# Jenkins Kurulumu Ve Monitoring Edilmesi



# Jenkins Nedir ?

 Geliştirdiğimiz projeleri hızlı bir şekilde deploy etmek, yazdığımız testleri kontrol etmek ve kod kalitesini sağlamak adına yaygın kullanılan CI/CD aralarından biridir.
 
## Gereksinimler 
- Ubuntu server
-  Java openjdk


## Gerekli Paketlerin Kurulumu

Jenkins'in sunucu üzerinden çalışabilmesi için java openjdk kurmamız gerekmektedir. Bunun için aşağıdaki komutu sunucuda çalıştırın.
> sudo apt install openjdk-8-jdk

Kurulum için onay isteyecektir.  ***"y"***  tuşuna basıp enter tuşuna basmanız yeterli olacaktır. Kurulum biraz sürebilir. Tamamlandıktan sonra jenkins kurulumuna başlayabiliriz.


## Jenkins Paketlerinin Kurulumu

Bazı linux dağıtımlarında güncel paketlerin bulunması sorunlu olabiliyor. Bunun için jenkins'i kontrol edeceği adreslerin güncelliğinden emin olmak için aşağıdaki komutları çalıştıralım.
>wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

** "OK"** mesajını gördükten sonra da aşağıdaki konutu çalıştıralım.

>sudo sh -c 'echo deb http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

 Ardından da aşağıdakini çalıştıralım.
>sudo apt update

Ardından jenkins kurulumunu yapabiliriz.
>sudo apt install jenkins

Kurulum tamamlandıktan sonra aşağıdaki komutu çalıştırıp jenkins'in durumunu görebiliriz.
>sudo systemctl status jenkins

 Eğer "stopped" konumunda gözüküyorsa aşağıdaki komut ile başlatabiliriz.
 >sudo systemctl start jenkins


## Jenkins'in Aktifleştirilmesi

Sunucu üzerinden jenkins servisini başlattıktan sonra kendi bilgisayarınızın browser'ından isteğimizi yapabiliriz. Bunun için sunucumuzun ip'sine jenkins portu üzerinden istek atmamız yeterli.
>http://{sunucu_ip_adresi}:8080/

Adrese gittiğimizde bizi kurulum ekranıyla karşılayacaktır. Öncelikle Jenkins bizden admin kullanıcısı olduğumuzu doğrulamamız gerekir. Bunun için de sunucu üzerindeki bir dizine oluşturduğu dosyadan şifreyi almamızı ister.
Bunun için ssh bağlantımız üzerinden aşağıdaki komutu çalıştıralım;
>sudo cat /var/lib/jenkins/secrets/initialAdminPassword

Bu adımdan sonra bir şifre göreceksiniz. Bunu kopyalayıp browser üzerinde istediği alana yapıştıralım.

## Jenkins'in Kurulumu
Bir sonraki ekranda "**Install suggested plugins**" seçeneğine tıklaıyp kurulumun bitmesini bekleyelim.

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/1.png)

Kurulum tamamlandıktan sonra bizden admin kullanıcısı oluşturmamızı isteyecektir. Burada bir kullanıcı oluşturmamız güvenlik açısından önemlidir.

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/2.png)

"**Save and Continue**" diyip ilerleyelim.

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/3.png)

Bu adımda da bir değişiklik yapmayıp "**Save and Finish**" butonuna tıklayalım.

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/4.png)

Şimdi "**Start using Jenkins**" butonuna tıklayıp panelimize doğrudan giriş yapabiliriz. Bir sonraki girişlerimizde oluşturduğumuz admin kullanıcısı bilgilerini kullanmamız gerekecektir.

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/5.png)

Eğer bu aşamaya geldiyseniz artık kendi buildlerinizi alıp, deploylarınızı yapabilecek, varsa testlerinizi kontrol edebilecek bir aracın kurulumunu sağladınız.

# Zabbix İle Jenkins Monitoring İşlemi

##  Gereksinimler

-  Aktif Jenkins kurulumu
- Aktif Zabbix Server kurulumu

## Jenkins Yapılandırması
Jenkins arayüzünüzü açın ve arayüzdeyken admin kısmına tıklayın:

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/11.png)

Bu kısma girmelisiniz:

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/12.png)

Burada mevcut kullanıcınız için bir token ekleyeceğiz. Add new token butonuna tıklayın:
![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/13.png)

Ben bu kısma admin yazıp generate butonuna tıklıyorum.

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/14.png)

Burada ortaya çıkan token'i kopyalayın. Daha sonra bu tokeni kullanacağız. Ardından ise kaydet butonuna tıklayın. Tokenim 11d ile başlıyor.

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/15.png)

## Jenkins Metrics

Zabbix'in Jenkins üzerindeki metrikleri detaylı olarak inceleyebilmesi için Jenkins üzerinde metrics eklentisini kurmamız gerekiyor. Jenkins arayüzünden **Manage Jenkins** butonuna tıklayın. Sonrasında ise eklenti eklemek için **Manage Plugins** butonuna tıklayın.

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/16.png)

Metrics yazan yeri bulun ardından eklentinin tikini işaretleyin ve yükleyin
![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/17.jpg)

Ardından ilgili tiki işaretleyin ve Jenkins'in otomatik yeniden başlamasını sağlayın:

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/18.png)

IP adresinizin sonuna **/configure** yazarak ilgili sayfaya gidin ve metrics kısmını bularak yeni bir access key oluşturun.

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/19.png)

Burada tüm yetkileri verin ve yeni bir key oluşturun:

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/20.png)

Oluşan key'i kopyalayın ve ardından yapılandırmayı kaydedin:
![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/21.png)

## Zabbix Yapılandırması

Zabbix tarafında yeni bir host eklemeniz gerekiyor.

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/22.png)

Burada hostname kısmı özelleştirilebilir. Ardından jenkins templatesini seçin. Agent olarak Zabbix Server Agent'ini kullanacağız.

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/23.png)

Az önce birer adet key ve token oluşturduk. Açıklamasında metrics yazan macro'ya metrics için oluşturduğumuz key'i gireceksiniz. Admin kullanıcısı için oluşturduğumuz token'i ise JENKİNS.API.TOKEN macro'suna gireceksiniz. Ek olarak kullanıcı adı ve jenkins URL girmelisiniz. Ardından kaydedip çıkın.

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/24.png)

Ardından metriklerin sorunsuz çekildiğini görmek için discovery kısmına ilerleyin.

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/25.png)

Burada herhangi bir hata ile karşılaşmıyoruz. Monitoring işlemimiz başarılı bir şekilde sağlanmıştır.

![enter image description here](https://raw.githubusercontent.com/ReptilianusBileciktus/jenkins-kurulumu-ve-zabbix-ile-monitoring-edilmesi/main/26.png)
