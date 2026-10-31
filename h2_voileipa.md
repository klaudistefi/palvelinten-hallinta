**Kurssi:** Palvelinten Hallinta ICI001AS3A-3017<br>
**Tekijä:** Klaudija Majamäki <br>
**Käyttöjärjestelmä:** Pöytätietokone, Windows 11 -kone <br>
**Virtuaalikone:** Oracle Virtual Bpx Debian

# h2 Voileipä
**31.10.2026**  <br>


## x) Tiivistelmät 




## a) Sudoless

Tehvävä oli luoda Ansiblea varten uusi käyttäjä, joka voi käyttää sudo-komentoja ilman salasana. Myös SSH kirjautuminen ja sudon käyttö piti saada toimimaan automaattisesti.

### Käyttäjän ja ryhmän luominen
Luon uuden käyttäjän sudo adduser ansibleuser. Loin uuden ryhmän, jonka sodoless sudo adduser ansibleuser. Lisäsin käyttäjän ryhmään sudo adduser ansibleuser sodoless. Ryhmän tarkoituksena on helpottaa käyttöoikeuksien hallintaa. Kun käyttäjä kuuluu ryhmään, hän saa ryhmälle määritellyt oikeudet sudo adduser ansibleuser sodoless.

```
sudo adduser ansibleuser
sudo adduser ansibleuser
sudo groupadd sodoless
sudo adduser ansibleuser sodoless
```

<img width="394" height="359" alt="image" src="https://github.com/user-attachments/assets/0c7faf93-b3b7-4650-bdb4-aee17b7de052" />



### Pikku testi
Avasin uusi terminaali ikunaan muodosta tarvittaessa SSH-yhteys kohdekoneeseen.
```
sudo -i
exit
```
<img width="221" height="220" alt="image" src="https://github.com/user-attachments/assets/1d960d5c-cfd4-49dd-a69d-6e97ada4415f" />

### Sudo oikeuksien määrittäminen

Määritin ryhmälle oikeuden, sudo komentoja ilman salasanaa sudo visudo /etc/sudoers.d/sodoless. Lisäsin tiedostoon %sudoless ALL = (ALL) NOPASSWD: ALL. Rivin tarkoitus on antaa kaikille sodoless ryhmän jäsenille oikeus suorittaa kaikkia komentoja eri käyttäjien oikeuksilla ilman salasanan kysymistä.

```
sudo visudo /etc/sudoers.d/sodoless
%sudoless ALL = (ALL) NOPASSWD: ALL
```

<img width="476" height="37" alt="image" src="https://github.com/user-attachments/assets/4c274655-ee5c-4fd0-a772-3123d44bba4b" />
<img width="445" height="175" alt="image" src="https://github.com/user-attachments/assets/bffa0420-f61b-4d74-8ba9-579c26efc971" />

### Testi

Testasin käyttäjän oikeuksia kirjautumalla käyttäjälle su – ansibleuser.  Tarkistin nykyisen käyttäjän - whoami. Tarkistin käyttäjän ryhmät – groups. Testasin sudo oikeuksia - sudo -k. Tämä tyhjentää sudon aiemmin tallentaman salasanan vahvistuksen. 
Testasin sudo-komennon suorittamista - sudo echo "Kaveri testi". Jos määritykset toimivat oikein, komentoa ei tarvitse vahvistaa salasanalla.

```
ssh antero@localhost -  Toimii
su – ansibleuser
whoami
groups
sudo -k
sudo echo "Testi kaveri"
```

<img width="444" height="290" alt="image" src="https://github.com/user-attachments/assets/7cca1eeb-d140-400b-86a9-015586b44e01" />

### Ongelmat

Kirjoitin ryhmän nimen väärin. Tiedostossa oli kirjoitusvirhe. Tämän vuoksi käyttäjä ei saanut sudo-oikeuksia. Tarkistin komennoilla whoami ja groups. Niiden avulla pystyin varmistamaan, millä käyttäjällä olin kirjautunut sisään ja mihin ryhmiin käyttäjä kuului. Ratkaisin ongelman kirjautumalla takaisin toiselle käyttäjälle ja korjaamalla sudoers-tiedoston. 
Virhe löytyi /etc/sudoers.d/sudoless.

```
whoami
groups
/etc/sudoers.d/sudoless
```
<img width="454" height="246" alt="image" src="https://github.com/user-attachments/assets/dcde487b-41ce-4bf2-aaba-83c12d10ca88" />
<img width="432" height="210" alt="image" src="https://github.com/user-attachments/assets/4a9ea471-73fc-45d4-a9f9-a74f6d9dccc1" />
<img width="444" height="194" alt="image" src="https://github.com/user-attachments/assets/e4fb847d-9284-46d7-8e9b-ff19cc8cde9f" />


### Testi 2

Kirjauduin uudelleen käyttäjälle ansibleuser ja testasin sudo-oikeuksia uudestaan. Tällä kertaa komento toimi ilman salasanan kysymistä. su - komento Linuxissa tarkoittaa "korvaavaa käyttäjää" (tai vaihda käyttäjää).

```
su – ansibleuser
sudo -k
sudo echo "Kaveri testi"
```

<img width="415" height="157" alt="image" src="https://github.com/user-attachments/assets/edd4b483-f7a8-4d07-93b5-bfa1b92f9697" />



# Lähteet: 
Tero Karvinen – Palvelinten Hallinta. Luettavissa: [https://terokarvinen.com/linux-palvelimet/](https://terokarvinen.com/palvelinten-hallinta/#h0-oikeasti-netissa)

Raportin kirjoittaminen. Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/ 

Sudo without password. Luettavissa: https://terokarvinen.com/passwordless-sudo/ 

Passwordless Sudo with Ansible. Luettavissa: https://terokarvinen.com/passwordless-sudo-with-ansible/

12.3 mkdir: Make directories. Luettavissa: https://www.gnu.org/software/coreutils/manual/html_node/mkdir-invocation.html 

How to Configure Apache web server on Linux using Ansible. Luettavissa: https://dev.to/paschalogu/how-to-configure-apache-web-server-on-linux-using-ansible-2mcd 
