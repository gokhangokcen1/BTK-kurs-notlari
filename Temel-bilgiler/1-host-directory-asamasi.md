**Aşamalar**
1.  Host discovery
2.  Port tarama
3.  İşletim sistemleri
4.  Port üzerindeki servis, hangi teknoloji ya da uygulama?
5.  Banner grabbin, versiyon?
6.  Uygulama 	✓ versiyon? - teknoloji	✓ versiyon? (vsftp	✓ - versiyon? zafiyet?)
7.  Zafiyet	✓ exploit ?
8.  exploit	✓ uygun şartlar?

**Nmap**

- `nmap -sP 192.168.1.0/24` ping gönderiliyor. Firewall benzeri geri istek dönmesini engelleyen sistemler varsa cevap dönmeyecektir.
- `nmap 192.168.1.0/24 --top-port-100 --open > btkanaliz` şeklinde en çok kullanılan 100 portun taramasını yapıp _btkanaliz_ dosyasına yükleniyor
- `cat btkanaliz | grep Nmap` yazdığımızda _Nmap scan report for <IP ADRES>_ şeklinde bir cevap alacağız
- `cat btkanaliz |grep Nmap | awk '{print $5}' > ip` yazarak da yalnızca ip adresleri kalacak şekilde filtrelendirip ip dosyasına sonucu yazıyoruz.
