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

## b) Antero
Tarkoitus oli luoda Ansiblella uusi käyttäjä nimeltä antero. Käyttäjälle piti määrittää SSH kirjautuminen sekä sudo oikeudet ilman salasanan kysymistä.

### Ansible rooli
Tarkistin, että SSH avain on tässä ls -la ~/.ssh/. Se näyttää .ssh kansion sisällön. Löysin kansiosta aikaisemmin luodun julkisen SSH avaimen, ei tarvinnut luoda uutta. Tarkistin julkisen avaimen sisällön cat ~/.ssh/id_edxxxxx.pub. Se näyttää julkisen SSH-avaimen, jonka kopioin. Siirryin Ansible kansiooncd ansible ja loin uuden roolin kansion mkdir -p roles/antero/tasks. Loin ja avasin tehtävätiedoston micro roles/antero/tasks/main.yml.

```
ls -la ~/.ssh/
cat ~/.ssh/id_edxxxxx.pub
cd ansible
mkdir -p roles/antero/tasks
micro roles/antero/tasks/main.yml
```

### Tehtävien määrittäminen
Lisäsin main.yml tiedostoon - micro main.yml kansiossa roles/antero/tasks/. Korvasin oma julkinen avain tekstin omalla julkisella SSH avaimela.
```
micro main.yml

- group:
    name: "sodoless"
    state: present
- user:
    name: "antero"
    state: present
    groups: ["sodoless", "sudo", "adm"]
- authorized_key:
    user: "antero"
    key: "oma julkinen avain"
- copy:
    dest: "/etc/sudoers.d/sodoless"
    content: "%sodoless ALL = (ALL) NOPASSWD: ALL\n"
    owner: "root"
    group: "root"
    mode: "0644"
```

<img width="544" height="50" alt="image" src="https://github.com/user-attachments/assets/e36bf22f-eb0a-4af4-8382-d3c7d124a6f2" />
<img width="543" height="30" alt="image" src="https://github.com/user-attachments/assets/ef793034-28f4-4fcb-86e0-6e271e2f0312" />
<img width="544" height="291" alt="image" src="https://github.com/user-attachments/assets/9799d2d4-2f53-438e-8982-0e4eea226dc0" />
<img width="342" height="367" alt="image" src="https://github.com/user-attachments/assets/e256604b-28e4-443c-abba-00974301623e" />


### Roolin lisääminen site.yml tiedostoon
Avasin olemassa olevan site.yml tiedoston ansible kansiossa - micro site.yml. Lisäsin siihen uuden roolin.
```
- hosts: all
  become: true
  roles:
    - world
    - antero
```
```
micro site.yml
```

<img width="444" height="155" alt="image" src="https://github.com/user-attachments/assets/c943486c-2245-4346-9d66-10b80f8cd153" />

### Playbookin suorittaminen
Suoritin playbookin - ansible-playbook site.yml. Alukisi Ansible ilmoitti, että sudo salasana puuttuu. Tämä  voi johtua siitä, että tehtävät vaativat pääkäyttäjän oikeudet.
Ratkaisin ongelman suorittamalla playbookin uudelleen ansible-playbook site.yml --ask-become-password tai eri versio ansible-playbook site.yml -K. S tarkoitta saman asian. Annoin oman admin käyttäjän salasanan. 
Tarkistin toiminon ssh antero@localhost ja whoami ja sudo woami. Komento palautti root ilman salasanan kysymistä.
site.ymltiedostossa oli määritelty become: true. Ansible suorittaa tehtävät pääkäyttäjän oikeuksilla. Ilman -K-valintaa Ansible ei saanut tarvittavaa sudo salasanaa.
```
ansible-playbook site.yml
ansible-playbook site.yml --ask-become-password
ansible-playbook site.yml -K
ssh antero@localhost
whoami
sudo woami
```
<img width="444" height="161" alt="image" src="https://github.com/user-attachments/assets/9dc640ce-00cd-4431-bab0-057df740f374" />
<img width="444" height="362" alt="image" src="https://github.com/user-attachments/assets/ca5193fe-30cc-4912-be99-9c57c841ebf5" />
<img width="444" height="282" alt="image" src="https://github.com/user-attachments/assets/96234fcd-80f0-4ff1-9cfb-c9a6ea10fc52" />



# Lähteet: 
Tero Karvinen – Palvelinten Hallinta. Luettavissa: [https://terokarvinen.com/linux-palvelimet/](https://terokarvinen.com/palvelinten-hallinta/#h0-oikeasti-netissa)

Raportin kirjoittaminen. Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/ 

Sudo without password. Luettavissa: https://terokarvinen.com/passwordless-sudo/ 

Passwordless Sudo with Ansible. Luettavissa: https://terokarvinen.com/passwordless-sudo-with-ansible/

12.3 mkdir: Make directories. Luettavissa: https://www.gnu.org/software/coreutils/manual/html_node/mkdir-invocation.html 

How to Configure Apache web server on Linux using Ansible. Luettavissa: https://dev.to/paschalogu/how-to-configure-apache-web-server-on-linux-using-ansible-2mcd 

Käytin tekoälyä tekstin kieliopin ja kirjoitusvirheiden tarkistamiseen.
