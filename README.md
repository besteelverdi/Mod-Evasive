## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/besteelverdi/printf-HelloWorld-/edit/main/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### MOD EVASIVE

Mod_evasive, DDoS, DoS ve brute force dahil olmak üzere Apache web sunucusunda çeşitli saldırı türlerine karşı koruma sağlamak için kullanılabilen bir Apache modülüdür. Mod_evasive, saldırılar durumunda kaçınma eylemi sağlar ve e-posta ve syslog aracılığıyla kötü niyetli etkinlikleri rapor eder. Dinamik bir IP adresleri ve URL'ler komut çizelgesi (hash table) kullanarak bir apache web sunucusuna gelen trafiği inceleyerek çalışır, ardından önceden belirlenmiş bir eşiği aşan IP adreslerinden gelen trafiği engeller. Aşağıdaki durumlarda IP adresini reddeder:
- Aynı sayfayı saniyede birkaç defadan fazla talep etmek
- Aynı child için saniyede 50'den fazla eşzamanlı istekte bulunma
- Geçici olarak kara listeye alınmışken herhangi bir istekte bulunma (engelleme listesinde)

## Mod Evasive Nasıl Çalışır ?
Bir web hit isteğinin gelmesi ile aşağıdaki adımlar gerçekleşir:
- İstekte bulunanın IP adresi geçici kara listede aranır
- İstekte bulunanın IP adresi ve URI'nin her ikisi de bir "anahtar" olarak hash haline getirilir. The listerner's internal hash table dosyasında, aynı host'un son 1 saniye içinde bu sayfayı bir kereden fazla talep edip etmediğini belirlemek için bir arama gerçekleştirilir.
- İstekte bulunanın IP adresi bir "anahtar" olarak hash hale getirilir. Aynı host'un son saniye içinde 50'den fazla nesne talep edip etmediğini belirlemek için the listerner's internal hash table'ında bir arama gerçekleştirilir (aynı child'den).

Yukarıdakilerden herhangi biri doğruysa 403 yanıtı gönderilir.Tek bir 403 olayı gerçekleştiğinde, mod_evasive tüm IP adresini 10 saniyelik bir süre için bloke eder. Host bu süre içinde bir sayfa isterse, daha da uzun süre beklemek zorunda kalır. Bu, aynı URL'yi saniyede birden çok kez talep etmekle tetiklendiğinden, bu yine meşru kullanıcıları etkilemez. Böylece DoS saldırısı durumunda bant genişliğini ve sistem kaynakları korunmuş olur. 

## Mod Evasive Kurulumu
Mod_evasive'ı kurmak için mevcut tüm modülleri kontrol edin:
`sudo apache2ctl -M
 sudo apache2ctl -M | grep evasive `

Mod_evasive'i kurun:
`sudo apt-get install libapache2-mod-evasive`

Etkinleştirilen modüllerin listesini görüntüleyin:
`sudo ls /etc/apache2/mods-enabled/`

Evasive.conf ve evasive.load dosyaları eklendi. /Etc/apache2/mods-available/mod-evasive.conf dosyası modül yapılandırma yönergelerini içerir. /Etc/apache2/mods-available/mod-evasive.load dosyası, Apache2'ye modülü dosya sisteminde nerede bulacağını söyler.

Mod kaçınma modülü kendi başının çaresine bakar. komutla kontrol edebilirsiniz
`sudo a2enmod evasive
Module evasive already enabled`

Günlükleri depolayacak klasörü oluşturun:
`sudo mkdir /var/log/apache2/mod_evasive
sudo touch /var/log/apache2/mod_evasive/dos_evasive.log
sudo chown -R www-data:www-data /var/log/apache2/`

## Mod Evasive Yapılandırma
Mod_evasive'ı yapılandırmak için:
 `sudo nano /etc/apache2/mods-enabled/evasive.conf`
 
/Etc/apache2/mods-enabled/evasive.conf dosyasındaki yönergeler Apache tarafından yönetilen tüm sitelere uygulanacaktır. Mod_evasive'ı belirli bir site ile kullanmak için, mod_evasive yönergelerini site yapılandırma dosyasına ekleyin. /Etc/apache2/sites-available/mon-site.conf sitesini < ? IfModule mod_evasive20.c> ve < ? IfModule>.Bu dosyada bulunan yönergeler daha sonra genel yapılandırmadakilerin yerini alacaktır.
 `<IfModule mod_evasive20.c>
 DOSHashTableSize 3097
 
# Saniyede en fazla 2 sayfa.
 DOSPageCount 2
 DOSPageInterval 1
 
# Saniyede 100'den fazla istek (Görseller, CSS, ...)
 DOSSiteCount 100
 DOSSiteInterval 1
 
# İstemcinin engellendiği saniye cinsinden süre.
 DOSBlockingPeriod 500
 
# Bir veya daha fazla beyaz liste IP adresi ekleyin.
# Yerel IP adresi beyaz listeye alınabilir.
# DOSWhitelist 127.0.0.1
# Sunucunun IP adresi beyaz listeye alınabilir.
# DOSWhitelist xxx.xx.xxx.xxx
# 3 IP adresi Google Bot'unkilerdir.
 DOSWhitelist 66.249.65. *
 DOSWhitelist 66.249.66. *
 DOSWhitelist 66.249.71. *
 
# Uyarıyı bir e-posta ile bildirin.
 DOSEmailNotify admin@example.org
 
# Apache2 günlük klasörünün yolu, mod_evasive.
# Dosya kara listeye alınmış IP adreslerini içerecektir.
# DOSLogDir "/ var / log / apache2 / mod_evasive"
# IP adresine göre bir dosya döndürür. İçerdiği değer tutarsız.
# cat dos-127.0.0.1, 19915. 
# Kendi komut dosyanızla günlük yönetimine öncelik verin.
 
# DOSSystemCommand ile bir komut başlatın.
# Örneğin, bir Iptables komut dosyası başlatın:
# DOSSystemCommand "sudo iptables -A INPUT -s% s -j DROP"
# Günlükleri yazın:
 DOSSystemCommand "/ bin / echo% s >> /var/log/apache2/mod_evasive/dos_evasive.log && / bin / date >> /var/log/apache2/mod_evasive/dos_evasive.log"
</IfModule>

# Apache2 yapılandırmasının sözdizimini kontrol edin:
sudo apache2ctl -t
# Sözdizimi Tamam

# Değişiklikleri hesaba katmak için Apache2'yi yeniden başlatın:
sudo /etc/init.d/apache2 yeniden başlatma`

## Parametre Listesi
### DOSHashTableSize
Karma tablonun boyutu.
Bu sayıyı artırmak performansı artırır ancak daha fazla bellek tüketir.
Varsayılanı bırakın.

### DOSPageCount
DOSPageInvernal aralığında aynı sayfa için istek sayısı, bunun ötesinde, IP adresi engellenir.

### DOSSiteCount
DOSPageInvernal aralığında aynı site için istek sayısı, bunun ötesinde, IP adresi engellenir.

### DOSPageInterval
Aynı sayfa için saniye cinsinden istek sayısı.

### DOSSiteInterval
Aynı site için saniye cinsinden istek sayısı.

### DOSBlockingPeriod
IP adresinin engelleneceği saniye cinsinden süre (Yasak döndürülür).

### DOSWhitelist
IP adreslerini beyaz listeye almanıza izin verir.
DOSWhitelist 192.168.0. * Özel bir yerel ağdaki makinelerden gelen isteklere izin verir.

### DOSEmailNotify
Mail olarakbilgilendirme için mail adresi girilmelidir.

### DOSLogDir
Logların tutulacağı dizini belirtir.


