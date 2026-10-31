**Kurssi:** Palvelinten Hallinta ICI001AS3A-3017<br>
**Tekijä:** Klaudija Majamäki <br>
**Käyttöjärjestelmä:** Pöytätietokone, Windows 11 -kone <br>
**Virtuaalikone:** Oracle Virtual Bpx Debian

# h2 Voileipä
**31.10.2026**  <br>


## x) Tiivistelmät 




## a) Sudoless

Tehtävän tarkoitus oli luoda Ansiblea varten uusi käyttäjä, joka voi käyttää sudo-komentoja ilman salasanan kysymistä. Myös SSH-kirjautuminen ja sudon käyttö piti saada toimimaan automaattisesti.

Käyttäjän ja ryhmän luominen
Luon uuden käyttäjän sudo adduser ansibleuser. Loin uuden ryhmän, jonka sodoless Sudo adduser ansibleuser. Lisäsin käyttäjän ryhmään sudo adduser ansibleuser sodoless. Ryhmän tarkoituksena on helpottaa käyttöoikeuksien hallintaa. Kun käyttäjä kuuluu ryhmään, hän saa ryhmälle määritellyt oikeudet sudo adduser ansibleuser sodoless.

sudo adduser ansibleuser
sudo adduser ansibleuser
sudo groupadd sodoless
sudo adduser ansibleuser sodoless









# Lähteet: 
Tero Karvinen – Palvelinten Hallinta. Luettavissa: [https://terokarvinen.com/linux-palvelimet/](https://terokarvinen.com/palvelinten-hallinta/#h0-oikeasti-netissa)

Raportin kirjoittaminen. Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/ 

Sudo without password. Luettavissa: https://terokarvinen.com/passwordless-sudo/ 

Passwordless Sudo with Ansible. Luettavissa: https://terokarvinen.com/passwordless-sudo-with-ansible/

12.3 mkdir: Make directories. Luettavissa: https://www.gnu.org/software/coreutils/manual/html_node/mkdir-invocation.html 

How to Configure Apache web server on Linux using Ansible. Luettavissa: https://dev.to/paschalogu/how-to-configure-apache-web-server-on-linux-using-ansible-2mcd 

