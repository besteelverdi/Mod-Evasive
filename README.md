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
```markdown 
sudo apache2ctl -M
sudo apache2ctl -M | grep evasive 

## Mod Evasive Kurulumu
Mod_evasive'i kurun:

Etkinleştirilen modüllerin listesini görüntüleyin:

Evasive.conf ve evasive.load dosyaları eklendi. /Etc/apache2/mods-available/mod-evasive.conf dosyası modül yapılandırma yönergelerini içerir. /Etc/apache2/mods-available/mod-evasive.load dosyası, Apache2'ye modülü dosya sisteminde nerede bulacağını söyler.

Mod kaçınma modülü kendi başının çaresine bakar. komutla kontrol edebilirsiniz
Günlükleri depolayacak klasörü oluşturun:


# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/besteelverdi/printf-HelloWorld-/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.
