**Kurssi:** Palvelinten Hallinta ICI001AS3A-3017<br>
**Tekijä:** Klaudija Majamäki <br>
**Käyttöjärjestelmä:** Pöytätietokone, Windows 11 -kone <br>
**Virtuaalikone:** Oracle Virtual Bpx Debian

# H1 Hei Ansiblen maailma
**27.10.2026**  <br>
**28.10.2026**  <br>

## x) Tiivistelmät 
Karvinen 2026: SSH public key - Login without password
- Helppo tapa kirjautua ilman salasanan kirjoittamista joka kerta
- Päivitetään Linux ja luodaan julkinen avain
- Testataan komennot ja kirjautuminen

Karvinen 2026: Hello Ansible
- tutustutaan tarkemmin siihen, miten Ansible toimii
- Ansible on työkalu, sillä voidaan hallita ja määrittää tietokoneita
- Ansible ei ole helpoin työkalu aloittelijalle (minulle), mutta sen käyttöä voi oppia 

## a) Asenna Debian 13 paikallisesti virtuaalikoneeseen.

VirtualBox oli jo asennettu. Asensin vain uuden Debian virtuaalikoneen. Käytin [minun vanhoja](https://github.com/klaudistefi/linux-course/blob/main/h1_oma_Linux_raportti.md) sekä kurssilla annettuja [ohjeita](https://github.com/punnalathomas/linux-course/blob/main/h1-installing-linux.md). Asennus meni hyvin, patsi virtuaalikoneen käynnistyksessä tuli virhe. Kone ei käynnistynyt, vaan näytölle tuli viesti ”ABORTED”. Tietokoneen uudelleenkäynnistyksen jälkeen virtuaalikone kuitenkin käynnistyi normaalisti.

Asetukset: Base Memory: 4090 MB, Number of CPUs: 2, Disk Size: 60 GB. Kieli: English, Sijainti: Other - Europe - Finland, Locale: en_US.UTF-8, Näppäimistö: Finnish, Hostname: debian, Levyn osiointi: Guided – use entire disk, Osiointi: All files in one partition, Valitaan: Finish partitioning and write changes to disk, partation disk : YES


##  b) Sshecrets. Asenna SSH-demoni käsin ja testaa se kirjautumalla SSH:lla. (localhost) ja c) Pubkey. Automatisoi ssh-kirjautuminen julkisella avaimella. (localhost)

Ensin päivitin järjestelmän ja asensin SSH-palvelimen.
```
sudo apt-get update
sudo apt-get -y install ssh
```
 <br>
<img width="1112" height="108" alt="1" src="https://github.com/user-attachments/assets/68308557-74a1-4782-85bb-9b5aa32c20a5" />
<img width="799" height="89" alt="2" src="https://github.com/user-attachments/assets/61231159-f271-4995-b87d-64d4be120c90" />

Laitoin SSH palvelun käynnistymään.

```
sudo systemctl enable --now ssh
```
 <br>
<img width="1394" height="70" alt="3" src="https://github.com/user-attachments/assets/4fdcec43-3b06-4f87-ae2b-76fbbbc04109" />

Loin avainparin SSH kirjautumista varten ja kopioin julkisen avaimen localhost osoitteeseen.
```
ssh-keygen
```
 <br>
<img width="1065" height="509" alt="4 1" src="https://github.com/user-attachments/assets/31a8c91c-52a0-4fb8-ac6c-22771721a5e5" />

```
ssh-copy-id localhost
```
 <br>
<img width="1573" height="332" alt="5" src="https://github.com/user-attachments/assets/a3f51870-a272-4db4-830b-5c6462a17b6c" />




Testasin kirjautumista SSH. Nyt pystyin kirjautumaan ilman salasanan.

```
ssh localhost
```
 <br>
<img width="1403" height="209" alt="6" src="https://github.com/user-attachments/assets/819754ab-caa9-4e36-be31-d392b372d719" />



### Troubleshooting
```
sudo systemctl status ssh
```
```
ssh klaudi@localhost
```
```
exit
```
<img width="570" height="34" alt="7" src="https://github.com/user-attachments/assets/de79f98a-43e6-407d-8658-8e15f8b56d30" />


## d) Hei Ansible. Tee hei maailma ansiblella ja kokeile sitä SSH:n yli. (localhost)

Päivitin järjestelmän ja asensin Ansiblen sekä muut tarvittavat ohjelmat.

```
sudo apt-get update
sudo apt-get install ansible micro bash-completion tree
```

<img width="536" height="35" alt="c1" src="https://github.com/user-attachments/assets/3824fb65-1f04-49cb-a1e5-e131f4171b44" />


Loin  hakemiston ja siirryin siihen, sekä loin hosts.ini tiedoston micro editorilla. Komento cat näyttää tiedoston sisällön.
```
cd
mkdir ansible/
cd ansible/
```

<img width="316" height="86" alt="c2" src="https://github.com/user-attachments/assets/d59eb2ca-a3b5-4567-89f8-0874151a08b9" />
```
micro hosts.ini
cat hosts.ini

localhost
```

Tiedostossa määritellään käytettävä kone (localhost). Määritellään Pythonin sijainti. Pakko. Tämän asetus, autta Ansible ilmoitta Python versioon liittyvästä ongelmasta. Tallensin tiedoston  Ctrl + S ja poistuin Ctrl + Q.

```
localhost

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```


<img width="583" height="62" alt="c3" src="https://github.com/user-attachments/assets/e804001b-8628-4d9c-8481-315a75049aea" />
<img width="822" height="165" alt="с6" src="https://github.com/user-attachments/assets/5bc47c82-e38d-422b-9d59-3ceab7312458" />


Loin Ansible asetustiedoston. Tämä kommunikoi Ansiblelle, että koneiden tiedot löytyvät hosts.ini sta.
```
micro ansible.cfg
cat ansible.cfg

[defaults]
inventory = hosts.ini
```
<img width="847" height="132" alt="с7" src="https://github.com/user-attachments/assets/4956ef1c-258c-45b9-8767-02238e05c10d" />



### Hello World

Loin roolille oman hakemistorakenteen ja tiedoston main.yml.
```
mkdir -p roles/hello/tasks
```
```
micro roles/hello/tasks/main.yml

- name: Create Hello World file
  copy:
    dest: /tmp/hello-ansible
    content: "Hello Ansible world!\n"
```

Tämä siis luo tiedoston /tmp/hello-ansible ja kirjoittaa siihen tekstin Hello Ansible world!



Sitten loin playbook tiedoston, joka käynnistää Hello World.
```
micro site.yml

- hosts: all
  roles:
    - hello
```

Se sitten määrittää, että Ansible suorittaa hello "roolin" kaikilla hosts.ini tiedostossa määritellyillä koneilla.

<img width="1099" height="271" alt="c11" src="https://github.com/user-attachments/assets/624fc326-f0e5-4e91-a538-65dc2683add5" />






```
ssh localhost 'cat /tmp/hello-ansible'
```






Had to go a bit bad and forth. But eventually got it right. 














## Lähteet: 
Heinonen, Johanna 2025. How to Install Linux to Virtualbox? Luettavissa: https://github.com/johannaheinonen/johanna-test-repo/blob/main/linux-20082025.md#1-virtualbox-installation

Tero Karvinen – Palvelinten Hallinta. Luettavissa: [https://terokarvinen.com/linux-palvelimet/](https://terokarvinen.com/palvelinten-hallinta/#h0-oikeasti-netissa)

Raportin kirjoittaminen Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/ 

Downloading Debian. https://www.debian.org/download

SSH public key - Login without password. Luettavissa: [https://terokarvinen.com/2020/command-line-basics-revisited/?fromSearch=command%20line%20basics%20revisited ](https://terokarvinen.com/ssh-public-key-login-without-password/)

Hello Ansible. Luettavissa: [https://terokarvinen.com/hello-ansible/ ](https://terokarvinen.com/hello-ansible/)
