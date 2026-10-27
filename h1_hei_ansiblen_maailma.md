**Kurssi:** Palvelinten Hallinta ICI001AS3A-3017<br>
**Tekijä:** Klaudija Majamäki <br>
**Käyttöjärjestelmä:** Pöytätietokone, Windows 11 -kone <br>
**Virtuaalikone:** Oracle Virtual Bpx Debian

# H1 Hei Ansiblen maailma
**27.10.2026**  
**Aloitusaika**: 23:30  
**Lopetusaika**: 20:00  
**25.8.25**  
**Aloitusaika**: 15:40  
**Lopetusaika**: 16:20  

a) Asenna Debian 13 paikallisesti virtuaalikoneeseen. Voit käyttää esimerkiksi VirtualBoxia.

- VirtualBoxia olis jo asenettu tietokonella. Asensin uusi Debain VM.
- Base Memory: 4090 MB, Number of CPUs: 2, Disk Size: 60 GB, Use EFI: check. Käytäsin minun vanhat asennus ohjeet sekä kursin aikana anettut ohjeet. Asennus meni nätisit. paitsi tuli errot. VM ei halunut käynistya tuli ABORTED viesti. mutta tietokonen uudeleen käynistamiseen jälkeen se avautunut. 
https://github.com/klaudistefi/linux-course/blob/main/h1_oma_Linux_raportti.md
https://github.com/punnalathomas/linux-course/blob/main/h1-installing-linux.md



b) Sshecrets. Asenna SSH-demoni käsin ja testaa se kirjautumalla SSH:lla. (localhost kelpaa)

c) Pubkey. Automatisoi ssh-kirjautuminen julkisella avaimella. (localhost kelpaa)

d) Hei Ansible. Tee hei maailma ansiblella ja kokeile sitä SSH:n yli. (localhost kelpaa)

e) Vapaaehtoinen bonus: kokeile Ansiblella jokin näista asetuksista: paketin asennus, asetustiedosto /etc/ alle, käynnistä jokin demoni, tee uusi käyttäjä. Tarvitset todennäköisesti sudoa, become: true.


## Lähteet: 
Heinonen, Johanna 2025. How to Install Linux to Virtualbox? Luettavissa: https://github.com/johannaheinonen/johanna-test-repo/blob/main/linux-20082025.md#1-virtualbox-installation

Tero Karvinen – Palvelinten Hallinta. Luettavissa: [https://terokarvinen.com/linux-palvelimet/](https://terokarvinen.com/palvelinten-hallinta/#h0-oikeasti-netissa)

Raportin kirjoittaminen Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/ 

Downloading Debian. https://www.debian.org/download

SSH public key - Login without password. Luettavissa: [https://terokarvinen.com/2020/command-line-basics-revisited/?fromSearch=command%20line%20basics%20revisited ](https://terokarvinen.com/ssh-public-key-login-without-password/)

Hello Ansible. Luettavissa: [https://terokarvinen.com/hello-ansible/ ](https://terokarvinen.com/hello-ansible/)
