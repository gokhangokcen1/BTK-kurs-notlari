## Subfinder - HTTPX - DNSX - Nüklei indir. ## 
  - Verdiğimiz bir domainin diğ                                                                 er subdomainlerini bize veriyor
  - `subfinder -d yandex.com.tr` 
    - /root/go/bin/subfinder altına kurulu kullanmak için /go/bin içerisindeyken  `./subfinder -d yandex.com` yazabiiriz.
  - `./subfinder -d yandex.com -p yandex-domains`
    - yukarıdaki ile kaydettik çıktıları ve bunu httpx içerisine gönderdik
    - `head -n 100 yandex-domains > yandex1` ilk 100 satırır yandex1 e verdik
    - `./httpx -ip -tech-detect -l yandex1 -o yandex2` ip adresi ve tespit edilen teknolojileri veriyor
  - `echo hackerone.com | ./httpx -tech-detect -ip`
  - `./dnsx -l yandex1 -a -ptr -txt -mx -ns -resp` 
  - `./dnsx -l yandex-domains -txt -resp`
  - `fierce --dns yandex.com.tr`
----
# Typhoon üzerinde test #

  * `nmap 192.168.1.113 -p 53 -Pn -n -sV`  53 portu açık ve karşıda ISC BIND çalışıyor.. DNS 
  * `nmap -p 445 192.168.1.113 --script=smb-os-discovery` karşıda FQDN: typhoon.local
    * `dig A yandex.com` Ağ kayıtları 
    * `dig NS yandex.com @8.8.8.8` Name server
  * `dig AXFR @192.168.1.113 typhoon.local` bana zone dosyalarını ver 
    * `dig NS zonetransfer.me` buradan tüm zone dosyalarını çekmemizi yarar. 
  
---
  - `./subfinder -d yandex.com -o yandex-d && ./httpx -l yandex-d -o yandex-h && nuclei -l ./yandex-h -t cves/ -severity critical` 
    - yandex subdomainlerini bulup `yandex-d` dosyasına kaydedecek. httpx http mi https mi belirleyip kaydedecek. nuclei bütün cveleri bunlar için deneyecek
      - `echo 192.168.1.0/24 | ./httpx -o btk-network && ./nuclei -l btk-network -t cves/`




















