# Nmap #
  - içerisinde vuln geçen _scriptleri_ kullanabiliriz.
  - `cd /usr/share/nmap/scripts`
  - `ls -la | grep vuln` 
  - `nmap -p 445 192.168.1.200 --script=smb-vuln-*` smb üzerindeki tüm zafiyet scriptlerini dene
  - `nmap -p 80 192.168.1.113 --script=http-vuln-*` 

# Nikto #
  - `nikto -h 192.168.1.113` sunucuyu tara ve zafiyet varsa getir. shellshock buldu
  - Nuclei shellshock bulamamıştı bu buldu 

# Wfuzz #
  - dirbuster gibi bir araç
    - dirbuster yerine dirb yazarsak GUI olmaz
  -  `wfuzz --hc 404 -w /usr/share/wordlists/wfuzz/dirb/big.txt http://192.168.1.113/FUZZ`
 
---
  - Bir şeyler bulduktan sonra exploit-db.com gibi sitelerde zafiyeti olup olmadığına bak.
  - `how to hack apache tomcat` benzeri aramalar yapılabilir.
