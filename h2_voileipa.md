**Kurssi:** Palvelinten Hallinta ICI001AS3A-3017<br>
**Tekijä:** Klaudija Majamäki <br>
**Käyttöjärjestelmä:** Pöytätietokone, Windows 11 -kone <br>
**Virtuaalikone:** Oracle Virtual Box Debian

# h2 Voileipä
**31.10.2026**  noin 7 tuntia <br>
**1.11.2026** noin 3 tuntia <br>


## x) Tiivistelmät 
### Karvinen 2026: Sudo without password
- Kirjoitusvirheet voivat aiheuttaa paljon ongelmia. Jos on vaikka lukuvaikeuksia, voi joutua kärsimään kuin minä<br>
- Komento sudo -k tyhjentää aiemmin tallennetun salasanan vahvistus<br>

### Munroe 2006: xkcd 149: Sandwitch
- Hauska sarjakuva, joka selittää yksinkertaisesti miten Linuxin logiikka toimii

### Karvinen 2026: Passwordless Sudo with Ansible
- site.yml-tiedostossa määritetään, mitä rooleja Ansible suorittaa
- become: true tarkoittaa, että tehtävät suoritetaan pääkäyttäjän oikeuksilla
- -K-valinnalla Ansible kysyy sudo-salasanan ensimmäisellä suorituskerralla

### Ansiblen sisäänrakennettu dokumentaatio ansible-doc -kommennolla.
- monet kommentit ja ohjeet ovat tärkeitä ja auttavat tehtävän suorittamisessa
- dokumentaatiossa on selityksiä eri komennoista

## a) Sudoless

Tehvävä oli luoda Ansiblea varten uusi käyttäjä, joka voi käyttää sudo-komentoja ilman salasanaa. Myös SSH kirjautuminen ja sudon käyttö piti saada toimimaan automaattisesti.

### Käyttäjän ja ryhmän luominen
Teen uuden käyttäjän sudo adduser ansibleuser. Tein uuden ryhmän, jonka sodoless sudo adduser ansibleuser. Lisäsin käyttäjän ryhmään sudo adduser ansibleuser sodoless. Ryhmän tarkoituksena on helpottaa käyttöoikeuksien hallintaa. Kun käyttäjä kuuluu ryhmään, hän saa ryhmälle määritellyt oikeudet sudo adduser ansibleuser sodoless.

```
sudo adduser ansibleuser
sudo adduser ansibleuser
sudo groupadd sodoless
sudo adduser ansibleuser sodoless
```

<img width="394" height="359" alt="image" src="https://github.com/user-attachments/assets/0c7faf93-b3b7-4650-bdb4-aee17b7de052" />



### Pikku testi
Avasin uusi terminaali ikkunaan muodosta tarvittaessa SSH-yhteys kohdekoneeseen.
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
<img width="545" height="200" alt="image" src="https://github.com/user-attachments/assets/bffa0420-f61b-4d74-8ba9-579c26efc971" />

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
<img width="554" height="346" alt="image" src="https://github.com/user-attachments/assets/dcde487b-41ce-4bf2-aaba-83c12d10ca88" />
<img width="532" height="310" alt="image" src="https://github.com/user-attachments/assets/4a9ea471-73fc-45d4-a9f9-a74f6d9dccc1" />
<img width="544" height="294" alt="image" src="https://github.com/user-attachments/assets/e4fb847d-9284-46d7-8e9b-ff19cc8cde9f" />


### Testi 2

Kirjauduin uudelleen käyttäjälle ansibleuser ja testasin sudo-oikeuksia uudestaan. Tällä kertaa komento toimi ilman salasanan kysymistä. su - komento Linuxissa tarkoittaa "korvaavaa käyttäjää" (tai vaihda käyttäjää).

```
su – ansibleuser
sudo -k
sudo echo "Kaveri testi"
```

<img width="415" height="157" alt="image" src="https://github.com/user-attachments/assets/edd4b483-f7a8-4d07-93b5-bfa1b92f9697" />

## b) Antero
Tarkoitus oli tehdä Ansiblella uusi käyttäjä nimeltä antero. Käyttäjälle piti asettaa SSH kirjautuminen sekä sudo oikeudet ilman salasanan kysymistä.

### Ansible rooli
Tarkistin, että SSH avain on tässä ls -la ~/.ssh/. Se näyttää .ssh kansion sisällön. Löysin kansiosta aikaisemmin tehdyn julkisen SSH avaimen, ei tarvinnut tehdä uutta. Tarkistin julkisen avaimen sisällön cat ~/.ssh/id_edxxxxx.pub. Se näyttää julkisen SSH-avaimen, jonka kopioin. Siirryin Ansible kansiooncd ansible ja loin uuden roolin kansion mkdir -p roles/antero/tasks. Loin ja avasin tehtävätiedoston micro roles/antero/tasks/main.yml.

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
<img width="644" height="391" alt="image" src="https://github.com/user-attachments/assets/9799d2d4-2f53-438e-8982-0e4eea226dc0" />
<img width="442" height="467" alt="image" src="https://github.com/user-attachments/assets/e256604b-28e4-443c-abba-00974301623e" />


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
<img width="544" height="261" alt="image" src="https://github.com/user-attachments/assets/9dc640ce-00cd-4431-bab0-057df740f374" />
<img width="544" height="462" alt="image" src="https://github.com/user-attachments/assets/ca5193fe-30cc-4912-be99-9c57c841ebf5" />
<img width="544" height="382" alt="image" src="https://github.com/user-attachments/assets/96234fcd-80f0-4ff1-9cfb-c9a6ea10fc52" />


### c) Package - Pakettien asentaminen Ansiblella
Asensin kaksi ohjelmaa Ansiblella. Asensin curl ja htop. Loin uuden roolin kansiorakenteen mkdir -p roles/packages/tasks. Siirryin tehtäväkansioon ja avasin uuden tiedoston micro roles/packages/tasks/main.yml. Lisäsin sinne.
```
- name: Install curl
  apt:
    name: curl
    state: present

- name: Install htop
  apt:
    name: htop
    state: present
```

apt avulla Ansible voi asentaa ja hallita Debian paketteja.
```
mkdir -p roles/packages/tasks
micro roles/packages/tasks/main.yml
```

<img width="444" height="129" alt="image" src="https://github.com/user-attachments/assets/40956334-31b1-487f-b06c-2352698db5a6" />
<img width="444" height="206" alt="image" src="https://github.com/user-attachments/assets/71c98cb1-bc01-4d09-acb8-c2a263ce8727" />

### Roolin lisääminen site.yml-tiedostoon
Avasin micro site.yml. Lisäsin packages roolin. Niiden kautta Ansible suorittaa myös packages. 
```
- hosts: all
  become: true
  roles:
    - world
    - antero
    - packages
```
```
micro site.yml
```
<img width="429" height="360" alt="image" src="https://github.com/user-attachments/assets/9131ba28-c474-4f2b-97db-45c629514522" />
<img width="440" height="312" alt="image" src="https://github.com/user-attachments/assets/1cb2951e-e60a-4316-bfa6-ac42154b560b" />


### Testi
Suoritin playbookin ansible-playbook site.yml. Unohdin lisätä loppuun --ask-become-pass-valinnan. Ansible ei saanut tarvittavaa sudo salasanaa. Kaikki oli ok kuin suoritin  ansible-playbook site.yml --ask-become-pass. Viimeinen tarkistus curl –version ja htop –version. Toimi. Molemmat näyttivät ohjelmien versiotiedot.
```
ansible-playbook site.yml
ansible-playbook site.yml --ask-become-pass
curl –version
htop --version
```

<img width="444" height="188" alt="image" src="https://github.com/user-attachments/assets/ff08afca-a847-4c9d-8c0d-968c44fb930f" />


## d) File
Loin useamman rivin mittainen tekstitiedosto Ansiblella. Asetin omistajan, omistavan ryhmän ja käyttöoikeudet.

Kansiossa ansible loin uuden roolin mkdir -p roles/file/tasks. Avasin uuden tehtävätiedoston micro roles/file/tasks/main.yml. Lisäsin seuraavan tehtävän
```
- name: Create file
  copy:
    dest: /home/antero/now.txt
    content: ”Luon file. Yksi/n kaksi”
    owner: antero
    group: sodoless
    mode: "0644"
```

copy avulla Ansible luo tiedoston ja kirjoittaa siihen määritellyn sisällön. dest - tiedoston sijainti ja nimi. content - tiedoston teksti. owner: antero - käyttäjä antero, siis omistaja. group: sodoless - tiedosto omistava ryhmä sodoless ryhmä. mode: "0644"  -käyttöoikeudet oktaalimuodossa.

```
mkdir -p roles/file/tasks
micro roles/file/tasks/main.yml
```

<img width="323" height="221" alt="image" src="https://github.com/user-attachments/assets/1cb8f589-db48-4229-a7be-2c18e8593cc7" />

### Roolin lisääminen ja suorittaminen
Kansiossa ansible. Avasin micro site.yml.
```
- hosts: all
  become: true
  roles:
    - world
    - antero
    - packages
    - service
    - file
```
Sitten playbook ansible-playbook site.yml -K.
```
micro site.yml
ansible-playbook site.yml -K
```

<img width="489" height="165" alt="image" src="https://github.com/user-attachments/assets/c468d0f8-93ee-4416-a299-0ab37354beed" />
<img width="444" height="482" alt="image" src="https://github.com/user-attachments/assets/bf6b1f6a-2baf-4e49-ab8b-f2e965af8490" />

### Testi
Onko tiedosto luotu oikeaan paikaan ja että sen omistaja, ryhmä ja käyttöoikeudet oikein ls -l /tmp/now.txt.
```
ls -l /tmp/now.txt
```
<img width="444" height="50" alt="image" src="https://github.com/user-attachments/assets/e4812a1c-937d-413b-87fe-2508d60fcc3a" />

### Tarkoitus

-rw-r--r--
r - lukuoikeutta (read).
w - kirjoitusoikeutta (write).
-, ettei kyseistä oikeutta ole. 


## e) Jotain muuta - Apache
Valitsin service moduulin, jonka avulla voidaan hallita Linuxin järjestelmäpalveluita. Sillä voi käynnistää, pysäyttää ja käynnistää palveluita uudelleen.
Loin uuden roolin mkdir -p roles/service/tasks. Sitten micro roles/service/tasks/main.yml. Sinne:
```
- name: Start Apache service
  service:
    name: apache2
    state: started
    enabled: yes
```

Tarkoitus käynnistää Apache palvelu ja määrittää se käynnistymään automaattisesti tietokoneen käynnistyessä.

```
mkdir -p roles/service/tasks
micro roles/service/tasks/main.yml
```

<img width="444" height="164" alt="image" src="https://github.com/user-attachments/assets/27a9299f-b222-47a5-ab05-e929a80942b3" />
<img width="444" height="189" alt="image" src="https://github.com/user-attachments/assets/fb8f739f-7c32-4a0c-a287-d6cbadd6ed4f" />

Lisäsin roolin micro site.yml
```

- hosts: all
  become: true
  roles:
    - world
    - antero
    - packages
    - service
```
```
micro site.yml
```

<img width="470" height="70" alt="image" src="https://github.com/user-attachments/assets/4affddf7-7316-4b2c-a74d-0d2ff65d9718" />
<img width="444" height="226" alt="image" src="https://github.com/user-attachments/assets/73758fc2-d250-410e-8caa-d579febde96b" />

### Testi 
```
ansible-playbook site.yml -K
systemctl status apache2
```

<img width="444" height="467" alt="image" src="https://github.com/user-attachments/assets/06f3ffdc-901e-465e-a973-7607c14d7090" />
<img width="444" height="302" alt="image" src="https://github.com/user-attachments/assets/66854eec-6583-430d-a86f-850416796333" />



# Lähteet: 
Tero Karvinen – Palvelinten Hallinta. Luettavissa: [https://terokarvinen.com/linux-palvelimet/](https://terokarvinen.com/palvelinten-hallinta/#h0-oikeasti-netissa) Luettu: 25.10.2026

Raportin kirjoittaminen. Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/ Luettu: 25.10.2026

Sudo without password. Luettavissa: https://terokarvinen.com/passwordless-sudo/ Luettu: 31.10.2026

Passwordless Sudo with Ansible. Luettavissa: https://terokarvinen.com/passwordless-sudo-with-ansible/ Luettu: 31.10.2026

12.3 mkdir: Make directories. Luettavissa: https://www.gnu.org/software/coreutils/manual/html_node/mkdir-invocation.html Luettu: 31.10.2026 

How to Configure Apache web server on Linux using Ansible. Luettavissa: https://dev.to/paschalogu/how-to-configure-apache-web-server-on-linux-using-ansible-2mcd Luettu: 31.10.2026

Sandwich. Luettavissa: https://xkcd.com/149/ Luettu: 1.11.2026

Käytin tekoälyä tekstin kieliopin ja kirjoitusvirheiden tarkistamiseen. 
