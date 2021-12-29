
  - `nmap -sP 192.168.1.0/24`
    - ![resim](https://user-images.githubusercontent.com/63648396/147687964-5f28a80c-48b7-4204-81bc-9aa1b25f6032.png)
  - `nmap -p 445 192.168.1.101,103 --script=smb-os-discovery`
    - hangi makine olduğunu öğrenmeye çalıştık. pwnme sitesini gördüğümüz için kevgir olduğunu anladık. 
    - ![resim](https://user-images.githubusercontent.com/63648396/147688080-a70b7bde-4973-4caf-9411-ae24b35fa106.png)
  - `nmap -Pn -sT -n -vv 192.168.1.100-110 -T4 --top-ports=100 -oA egitim` karşıda firewall gibi bir sistem varsa ve pinglere cevap alamıyorsak bunu kullanabiliriz.
    - 100-110 arasına değil de direkt olarak kevgir makinesinde tarama yaptık
    - Firewalla yakalanmamak için T değerini çok düşük yap  
    - ![resim](https://user-images.githubusercontent.com/63648396/147688729-e536ea2c-ba83-4af3-9661-788e8ceb47e9.png)
  - `nmap -Pn -sT -n -vv 192.168.1.100-110 -T4 --top-ports=100 -oA egitim -sV` -sV versiyon analizi yapıyor 
    - ![resim](https://user-images.githubusercontent.com/63648396/147688860-af8c83aa-5ab6-4567-a2ae-b27143ec8d2e.png)
    - `-O` işletim sistemi
    - `-A` hem işletim sistemleri hem de analize bağlı scriptleri çalıştırır.
      - ![Uploading resim.png…]()


 # KEVGIR ANALIZ 2 # 
