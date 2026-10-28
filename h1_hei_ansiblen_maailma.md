**Kurssi:** Palvelinten Hallinta ICI001AS3A-3017<br>
**Tekijä:** Klaudija Majamäki <br>
**Käyttöjärjestelmä:** Pöytätietokone, Windows 11 -kone <br>
**Virtuaalikone:** Oracle Virtual Bpx Debian

# H1 Hei Ansiblen maailma
**27.10.2026**  
**Aloitusaika**: 23:30  
**Lopetusaika**: 00:00 
**28.10.2026**  
**Aloitusaika**: 09:40 
**Tauko**: 11:00 
**Jakto**: 12:40  
**Lopetusaika**: 13:40  

## x) Tiivistelmät 
Karvinen 2026: SSH public key - Login without password
- easy way to log in without a hastle
- get your linux uptodate and create a public key
- test commands

Karvinen 2026: Hello Ansible
- this is a bit more indepth on how to opirate Ansible
- Ansible is a configuration management tool
- Not the easiest to start with but manageble

## a) Asenna Debian 13 paikallisesti virtuaalikoneeseen.

- VirtualBoxia oli jo asenettu tietokonella. Asensin vain uusi Debain VM.
- Base Memory: 4090 MB, Number of CPUs: 2, Disk Size: 60 GB. Käytäsin [minun vanhat]([https://pages.github.com/](https://github.com/klaudistefi/linux-course/blob/main/h1_oma_Linux_raportti.md) asennus ohjeet sekä [kurssin aikana anettut](https://github.com/punnalathomas/linux-course/blob/main/h1-installing-linux.md) anettut ohjeet. Asennus meni nätisit. paitsi käynistyksen aikana tuli errot. VM ei halunut käynistya tuli ABORTED viesti. mutta tietokonen uudeleen käynistamiseen jälkeen se avautunut.

Kieli: English
Location: other
Continent or region: Europe
Country: Finland
Configure locales: en_US.UTF-8
Configure the keyboard: Finnish
Edellä mainittujen jälkeen tulee hieman asennusta, jonka jälkeen jatketaan Configuratioo järjestyksessä
Hostname: debian
Domain name: punnala.com
Root password: jätetään tyhjäksi
Full name for the new user: thomas punnala
Username for your account: thomas
Password: valitaan vahva salasana, jota ei mainita tässä ohjeessa
Partition disks: Guided - use entire disk
Partition disks: valitaan ainut vaihtoehto SCI3
Partition disks: All files in one partition
Painetaan valintaa: Finish partitioning and write changes to disk
Kieli: English
Location: other
Continent or region: Europe
Country: Finland
Configure locales: en_US.UTF-8
Configure the keyboard: Finnish
Edellä mainittujen jälkeen tulee hieman asennusta, jonka jälkeen jatketaan Configuratioo järjestyksessä
Hostname: debian
Domain name: punnala.com
Root password: jätetään tyhjäksi
Full name for the new user: thomas punnala
Username for your account: thomas
Password: valitaan vahva salasana, jota ei mainita tässä ohjeessa
Partition disks: Guided - use entire disk
Partition disks: valitaan ainut vaihtoehto SCI3
Partition disks: All files in one partition
Painetaan valintaa: Finish partitioning and write changes to disk
partation disk : YES

##  b) Sshecrets. Asenna SSH-demoni käsin ja testaa se kirjautumalla SSH:lla. (localhost) ja c) Pubkey. Automatisoi ssh-kirjautuminen julkisella avaimella. (localhost)

Updated the system and Install the OpenSSH-server package

```
sudo apt-get update
```
```
sudo apt-get -y install ssh
```
<img width="1112" height="108" alt="1" src="https://github.com/user-attachments/assets/68308557-74a1-4782-85bb-9b5aa32c20a5" />
<img width="799" height="89" alt="2" src="https://github.com/user-attachments/assets/61231159-f271-4995-b87d-64d4be120c90" />



Make SSH daemon start on boot (enable) and also start it now (--now)
```
sudo systemctl enable --now ssh
```
<img width="1394" height="70" alt="3" src="https://github.com/user-attachments/assets/4fdcec43-3b06-4f87-ae2b-76fbbbc04109" />

Generate a keypair to Automatic login with SSH public key:
```
ssh-keygen
```
<img width="1065" height="509" alt="4 1" src="https://github.com/user-attachments/assets/31a8c91c-52a0-4fb8-ac6c-22771721a5e5" />




Copy it to hosts where you can already log in.
```
ssh-copy-id localhost
```
<img width="1573" height="332" alt="5" src="https://github.com/user-attachments/assets/a3f51870-a272-4db4-830b-5c6462a17b6c" />




Your public key is used automatically

```
ssh localhost
```
<img width="1403" height="209" alt="6" src="https://github.com/user-attachments/assets/819754ab-caa9-4e36-be31-d392b372d719" />



Troubleshooting
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
Here I had to install Ansible and print Hello world. Get updatess and the install Ansible. 

```
sudo apt-get update
sudo apt-get install ansible micro bash-completion tree
```
<img width="536" height="35" alt="c1" src="https://github.com/user-attachments/assets/57959cb3-7969-4d11-80ed-10da1102d62d" />



Let's make a folder for our Ansible configuration.
```
cd
mkdir ansible/
cd ansible/
```
Let's write a list of hosts we'll be controlling. Create "hosts.ini", and add "localhost" there.
<img width="316" height="86" alt="c2" src="https://github.com/user-attachments/assets/cd5fb638-ab70-4ed1-afe7-3075465491b9" />

```
micro hosts.ini
cat hosts.ini

localhost
```
<img width="583" height="62" alt="c3" src="https://github.com/user-attachments/assets/41ee1792-c897-4020-a577-95cd6185797e" />

To save file ctrl + s and to exit ctrl + q.

Sitten 
Unohdin otta kuvakaaupauksi jostan procesista, but I had run into problem where I needed to Let's tell it to use the obvious Python command. We can add variables to host groups in inventory. This one we'll add to all and then 
```
micro hosts.ini
cat hosts.ini

localhost

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```
<img width="1264" height="140" alt="c10" src="https://github.com/user-attachments/assets/427cbcc1-b577-49da-955b-535b2d5106b1" />
<img width="822" height="165" alt="с6" src="https://github.com/user-attachments/assets/71238b32-f3a4-467d-b745-06eb2b2e9904" />



```
micro ansible.cfg
cat ansible.cfg

[defaults]
inventory = hosts.ini
```
<img width="847" height="132" alt="с7" src="https://github.com/user-attachments/assets/f79910ea-b2aa-492e-82fc-bd6fcc1ea942" />


```
micro main
```
```
ssh localhost 'cat /tmp/hello-ansible'
```


<img width="1099" height="271" alt="c11" src="https://github.com/user-attachments/assets/918b263e-0fee-4fdd-bdc1-a15296af7569" />


<img width="711" height="115" alt="c12" src="https://github.com/user-attachments/assets/8075a8a4-81d0-4170-a932-f3d254a336ac" />
<img width="1189" height="194" alt="c13" src="https://github.com/user-attachments/assets/5d368f47-b714-420f-94d2-69df6cc0978b" />



<img width="703" height="35" alt="c14" src="https://github.com/user-attachments/assets/4231a328-6651-4364-bca7-0eca33635564" />

Had to go a bit bad and forth. But eventually got it right. 














## Lähteet: 
Heinonen, Johanna 2025. How to Install Linux to Virtualbox? Luettavissa: https://github.com/johannaheinonen/johanna-test-repo/blob/main/linux-20082025.md#1-virtualbox-installation

Tero Karvinen – Palvelinten Hallinta. Luettavissa: [https://terokarvinen.com/linux-palvelimet/](https://terokarvinen.com/palvelinten-hallinta/#h0-oikeasti-netissa)

Raportin kirjoittaminen Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/ 

Downloading Debian. https://www.debian.org/download

SSH public key - Login without password. Luettavissa: [https://terokarvinen.com/2020/command-line-basics-revisited/?fromSearch=command%20line%20basics%20revisited ](https://terokarvinen.com/ssh-public-key-login-without-password/)

Hello Ansible. Luettavissa: [https://terokarvinen.com/hello-ansible/ ](https://terokarvinen.com/hello-ansible/)
