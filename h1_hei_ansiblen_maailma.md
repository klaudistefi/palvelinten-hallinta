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
**Lopetusaika**: 16:20  

## x) Tiivistelmät Raportin kirjoittaminen
Lue ja tiivistä. (Tässä x-alakohdassa ei tarvitse tehdä testejä tietokoneella, vain lukeminen tai kuunteleminen ja tiivistelmä riittää. Tiivistämiseen riittää muutama ranskalainen viiva. Ei siis vaadita pitkää eikä essee-muotoista tiivistelmää. Lisää kuhunkin jokin oma kysymys tai huomio. Paljon komentoja sisältävistä dokumenteista kannattaa nostaa mukaan keskeiset komennot ja selittää ne.)
Karvinen 2026: SSH public key - Login without password
Karvinen 2026: Hello Ansible

a) Asenna Debian 13 paikallisesti virtuaalikoneeseen. ja b) Sshecrets. Asenna SSH-demoni käsin ja testaa se kirjautumalla SSH:lla. (localhost) ja c) Pubkey. Automatisoi ssh-kirjautuminen julkisella avaimella. (localhost kelpaa)

- VirtualBoxia oli jo asenettu tietokonella. Asensin vain uusi Debain VM.
- Base Memory: 4090 MB, Number of CPUs: 2, Disk Size: 60 GB. Käytäsin [minun vanhat]([https://pages.github.com/](https://github.com/klaudistefi/linux-course/blob/main/h1_oma_Linux_raportti.md) asennus ohjeet sekä [kurssin aikana anettut](https://github.com/punnalathomas/linux-course/blob/main/h1-installing-linux.md) anettut ohjeet. Asennus meni nätisit. paitsi käynistyksen aikana tuli errot. VM ei halunut käynistya tuli ABORTED viesti. mutta tietokonen uudeleen käynistamiseen jälkeen se avautunut. 

```
sudo apt-get update
```

```
sudo apt-get -y install ssh
```
```
sudo systemctl enable --now ssh
```

```
ssh-keygen
```

```
ssh-copy-id localhost
```

```
ssh-copy-id localhost
```
```
ssh localhost
```
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
d) Hei Ansible. Tee hei maailma ansiblella ja kokeile sitä SSH:n yli. (localhost kelpaa)
```
sudo apt-get update
sudo apt-get install ansible micro bash-completion tree
```
Let's make a folder for our Ansible configuration.


```
cd
mkdir ansible/
cd ansible/
```
Let's write a list of hosts we'll be controlling. Create "hosts.ini", and add "localhost" there.


```
micro hosts.ini
cat hosts.ini

localhost
```
ctrl + s
ctrl + q

Sitten 


## Lähteet: 
Heinonen, Johanna 2025. How to Install Linux to Virtualbox? Luettavissa: https://github.com/johannaheinonen/johanna-test-repo/blob/main/linux-20082025.md#1-virtualbox-installation

Tero Karvinen – Palvelinten Hallinta. Luettavissa: [https://terokarvinen.com/linux-palvelimet/](https://terokarvinen.com/palvelinten-hallinta/#h0-oikeasti-netissa)

Raportin kirjoittaminen Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/ 

Downloading Debian. https://www.debian.org/download

SSH public key - Login without password. Luettavissa: [https://terokarvinen.com/2020/command-line-basics-revisited/?fromSearch=command%20line%20basics%20revisited ](https://terokarvinen.com/ssh-public-key-login-without-password/)

Hello Ansible. Luettavissa: [https://terokarvinen.com/hello-ansible/ ](https://terokarvinen.com/hello-ansible/)
