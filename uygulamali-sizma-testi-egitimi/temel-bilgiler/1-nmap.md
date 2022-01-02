

**Aşamalar**
1.  Host discovery
2.  Port tarama
3.  İşletim sistemleri
4.  Port üzerindeki servis, hangi teknoloji ya da uygulama?
5.  Banner grabbin, versiyon?
6.  Uygulama 	✓ versiyon? - teknoloji	✓ versiyon? (vsftp	✓ - versiyon? zafiyet?)
7.  Zafiyet	✓ exploit ?
8.  exploit	✓ uygun şartlar?

# Nmap #

- `nmap -sP 192.168.1.0/24` ping taraması. Firewall benzeri geri istek dönmesini engelleyen sistemler varsa cevap dönmeyecektir.
- `nmap 192.168.1.0/24 --top-port-100 --open > btkanaliz` şeklinde en çok kullanılan 100 portun taramasını yapıp _btkanaliz_ dosyasına yükleniyor
- `cat btkanaliz | grep Nmap` yazdığımızda _Nmap scan report for <IP ADRES>_ şeklinde bir cevap alacağız
- `cat btkanaliz |grep Nmap | awk '{print $5}' > ip` yazarak da yalnızca ip adresleri kalacak şekilde filtrelendirip ip dosyasına sonucu yazıyoruz.

# Nmap Hedef Tanımlama #
- `nmap <IP ADRES>` port taraması
  - ![resim](https://user-images.githubusercontent.com/63648396/147383963-0d1a2541-93af-4f1f-a8cf-d3b9d37b46f1.png)
  - karşı tarafa bir _ping_ gönderiliyor karşılığında bir cevap geliyorsa o zaman port taraması yapıyor. Bu örnekteki gibi bir filtreleme vermezsek en çok kullanılan 1000 portu tarar
- `nmap 192.168.1.113,200` şeklinde iki farklı ip adresi için de tarama yapılabilir
  - ![resim](https://user-images.githubusercontent.com/63648396/147384067-f905632c-2a0e-49b3-96de-92ba30467897.png)
- `nmap 192.168.1.113-200` bu sefer de 192.168.1.113 ve 192.168.1.200 ipleri arasındaki tüm ipler için port taraması yapacak.
- `nmap 192.168.1.0/24` şeklinde de subnete yönelik bir tarama yapabiliriz.
- `nmap yandex.com.tr` şeklinde domain için de port taraması yapabiliriz.
- içerisinde alt alta aşağıdakiler gibi ip ve domainler yazan bir dosya oluşturduk. (-iL : include list)
  - nmap -iL <dosya adı>
  - > 192.168.1.0/25 
  - > 192.168.2.0/24
  - > yandex.com.tr
  - > 10.0.0.0/24
- `nmap 192.168.1.0/24 --exclude 192.168.1.200` 192.168.1.200 haricindeki o subnetteki tüm iplere tarama yapılacak.

# Nmap Port Tanımlaması #
- `nmap -p 80 <IP ADRESI>` 80 portunu tarıyor.
- `nmap -p 80 192.168.1.0/24` ağdaki tüm ipler üzerinde 80 portunu tarar (açık olan da kapalı olan da gözükür).
- `nmap -p 80 192.168.1.0/24 --opem` ağdaki tüm ipler üzerinde 80 portu açık olanları gösterir.
- `nmap -p 80,445 192.168.1.0/24 --open` ağdaki 80 ve 445. portu açık olan tüm ipleri gösterir.
- `nmap -p 80-445 192.168.1.0/24 --open` 80 ve 445 aralığındaki portları açık olan ipler
- `nmap -p 0-65535 192.168.1.0/24 --open` 65535 porttan açık olanları gösterir
- `nmap -p- 192.168.1.0/24 --open` yine aynı şekilde 65535 portu tarar.
- `nmap 192.168.1.113 --open --top-ports=5000`
  
# Nmap Tarama Tipleri #
- `nmap 192.168.1.113 --open --top-ports=100 -sS` SYN paketi ile port tarama
- `nmap 192.168.1.113 --open --top-ports=100 -sT` TCP ile port tarama
  - neden TCP taramasına ihtiyaç duyuyoruz, syn taraması neden yetmiyor? Port taraması yaptığımız sunucu tarafında bizim paketlerimizi alan _SYN PROXY_ gibi teknolojiler olabiler. Bunun gibi teknolojiler arkada port açık olmasa dahi bize açıkmış gibi cevap gönderebilir. Birçok portun açık olduğunu gördüğümüzde (81,82,83,84,85) ve böyle bir teknolojinin varlığından şüphelendiğimizde TCP taramasını yaparız, üçlü el sıkışma bize gerçekte de açık olan portları filtrelememizi sağlar.
- `nmap 192.168.1.0/24 -sU` UDP taraması
- `nmap 192.168.1.113 -sN` Null, içinde flag olmayan bir tarama. Bir servis var ve bana cevap vermiyor. Biri ile boş boş bakışmak gibi 
  - open filtered |firewall bypass yöntemlerinde kullanılır

# Nmap Ek Parametreler #
- `nmap 192.168.1.113 -O` tahmini işletim sistemi ve versiyonu
- `nmap 192.168.1.113 -sV` normal taramada ftp, smtp gibi yazılar tahmini olarak vardı. ancak bu tarama ile versiyon taraması yaparak port üzerinde tam olarak ne çalıştığını öğreniyoruz.
- `nmap 192.168.1.113 -A` TCP ACK taraması. işletim sistemi, versiyon taraması, ufak fingerprint scriptleri
- `nmap 192.168.1.113 -oN akademi` tarama çıktısını akademi dosyasına kaydet (output N)
- `nmap 192.168.1.113 -oX akademi` xml şeklinde kaydeder.
- `nmap 192.168.1.113 -oG akademi` grepable bir çıktı.
- `nmap 192.168.1.113 -oA akademi` bütün output şekillerinde çıktı (gnmap, nmap, xml)
- `nmap 192.168.1.113 -T1 akademi` T zaman ayarlaması. default olarak 3 gelir. her artırıldığında artar. güvenlik aracı varsa T1 daha iyi. 
- `nmap 192.168.1.200 -Pn` ping cevap vermiyorsa. hiç ping atmadan port taraması yapar. -sP pinge cevap gelirse port taraması yapıyorsa
- `nmap 192.168.1.0/24 -vv --open -Pn --top-ports=5000 -sT -sV -O -oA akademi2` root yetkisi istiyor. 
  - -n verilirse DNS resolution yapmaz
  
# Nmap Scripting Engine #
- https://nmap.org/book/nse.html
- /usr/share/nmap/scripts altında tüm scriptleri gözükür
- `nmap -p 445 192.168.1.200 -sC` script taraması
- `nmap --script-updatedb` script kısmını güncelliyor. sudo gerekli
- `nmap --script-smb-os-discovery 192.168.1.200`
- `nmap --script-help smb-os-discovery` HELP 
- `nmap 192.168.1.200 --script=smb-vuln-* -p 445` smb ile ilgili bir zafiyet var mı diye hepsini deniyor.

# Nmap Firewall Bypass #
- default olarak syn taraması yapılıyordu.
- `nmap 192.168.1.113 -f` paketi parçalayarak tespit edilmesinin önüne geçer
- `nmap 192.168.1.113 -ff`  daha fazla parçalar 
- `nmap 192.168.1.113 -ff -D 192.168.1.15` sanki sondaki ip hesabından çok fazla istek geliyormuş gibi karşıda boş loglar oluşturup kendinizi gizleyebilirsiniz.
- `nmap 192.168.1.0/24 -ff -D 192.168.1.15 --max-paralellism 1`  sisteme çok fazla yükleme yapmayalım. aynı anda sadece 1 tane tarama yap .
- `nmap 192.168.1.113 -ff -D 192.168.1.15 -T1` süreyi azaltarak daha iyi tarama yapılır
- `sudo nmap --script=firewall-bypass 192.168.1.113`
