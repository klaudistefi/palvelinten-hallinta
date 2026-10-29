
**Kurssi:** Palvelinten Hallinta ICI001AS3A-3017<br>
**Tekijä:** Klaudija Majamäki <br>
**Käyttöjärjestelmä:** Pöytätietokone, Windows 11 -kone <br>
**Virtuaalikone:** Oracle Virtual Bpx Debian

# H1 h0 Oikeasti netissä
**29.10.2026**  <br>


## x) Tiivistelmät 

### Niemi 2025: H4 Tehtävät
- Kuvakaappaukset oli esitetty hyvin
- Vaiheet oli selitetty selkeästi.
- Virtuaalipalvelimen alkutoimet oli avattu ja selitetty todella hyvin.

### Partanen 2025: Harjoitus 5: Nimekäs: a) Nimi (Vain a-kohta.)
- Alidomainit oli luotu.
- Kaikki toimi niin kuin piti, mutta olisi kuitenkin kiva, jos vaiheissa olisi näytetty tai kerrottu tarkemmin, mitä painikkeita tai valintoja käytettiin.

<br>

## a) Paikallista. Asenna Debian 13 paikallisesti virtuaalikoneeseen.

Asensin Debian 13 VirtualBoxiini. Asennusvaiheet löytyy tehtävän H1 yhteydessä. Löytyy täältä: https://github.com/klaudistefi/palvelinten-hallinta/blob/main/h1_hei_ansiblen_maailma.md 

<br>

## f) Tehdä alkutoimet ja asentaa weppipalvelimen paikalliseen virtuaalikoneeseen.

Olen edellisellä Linux kurssilla vuokrannut palvelimen enkä välttämättä halunnut vuokrata uutta VPS palvelinta. Sen takia päätin tehdä alkutoimet ja asentaa web-palvelimen paikalliseen virtuaalikoneeseen. 
<br>

### Virtuaalikoneen tarkistaminen
Avasin VirtualBoxin ja käynnistin Debian VM terminaalin. Tarkistin käyttäjänimen, koneen nimen ja Linuxin version komennoilla whoami, hostname ja cat /etc/os-release. 

Tarkistin myös sudo oikeudet - sudo whoami. Tulos tuli root, joten sudo toimi oikein.

```
whoami
hostname
cat /etc/os-release
sudo whoami
```
<img width="544" height="337" alt="image" src="https://github.com/user-attachments/assets/0fa8843b-3e84-42d5-ad44-c2eb641c079d" />
<img width="544" height="64" alt="image" src="https://github.com/user-attachments/assets/3c32b0f9-80ae-4087-b16a-abd3573b9495" />

### Palomuurin asetukset

Asensin UFW palomuurin - sudo apt-get install ufw. Palomuurin tarkoitus on hallita koneeseen tulevia verkkoyhteyksiä. Sallitin SSH yhteydet - sudo ufw allow OpenSSH ja HTTP liikenteen - sudo ufw allow 80/tcp. Otin palomuurin käyttöön - sudo ufw enable ja tarkistin asetukset - sudo ufw status.
```
sudo apt-get install ufw
sudo ufw allow OpenSSH
sudo ufw allow 80/tcp
sudo ufw enable
sudo ufw status
```

<img width="523" height="360" alt="image" src="https://github.com/user-attachments/assets/683b3e4e-83f8-4764-84e1-e7049d4f327e" />
<img width="446" height="254" alt="image" src="https://github.com/user-attachments/assets/f5400388-ca74-4248-8a4f-c216c38e995e" />

### Järjestelmän päivittäminen

Päivitin komennolla sudo apt-get update. Sen jälkeen päivitin asennetut ohjelmat komennolla sudo apt-get upgrade. Käynnistin virtuaalikoneen uudelleen komennolla sudo reboot, jotta päivitykset  toimii.
```
sudo apt-get update
sudo apt-get upgrade
sudo reboot
```

### Apache-web-palvelimen asentaminen

Asensin Apache web palvelimen - sudo apt-get install apache2. Käynnistin palvelun - sudo systemctl start apache2. Asetin sen käynnistymään automaattisesti - sudo systemctl enable apache2.
Tarkistin palvelun tilan - sudo systemctl status apache2. Palvelu oli käynnissä. 
Avasin Debianin selaimessa localhost. Sivulle avautui Apachen sivu, että toimi.
```
sudo apt-get install apache2
sudo systemctl start apache2
sudo systemctl enable apache2
sudo systemctl status apache2
http://localhost
```

<img width="544" height="394" alt="image" src="https://github.com/user-attachments/assets/eb54ffee-f05c-48d4-80f3-683ba0a46ff8" />
<img width="544" height="300" alt="image" src="https://github.com/user-attachments/assets/4829ecbb-84a8-4cae-8f3b-ed8f7b110f4f" />


### Oma web-sivu

Tein oman HTML sivun. Apachen sivu on /var/www/html/, ja sivun tiedoston nimi on index.html. Avasin tiedoston micro editorilla - sudo micro /var/www/html/index.html.
Korvasin sivun sisällön oamalla HTML koodilla. Tallensin tiedoston Ctrl + S ja suljin editorin Ctrl + Q.
Avasin selaimessa localhost. Oma sivuni näkyi, joten mahtavaa.
```
sudo micro /var/www/html/index.html
http://localhost
```

KUVA
KUVA


# Lähteet: 
Tero Karvinen – Palvelinten Hallinta. Luettavissa: [https://terokarvinen.com/linux-palvelimet/](https://terokarvinen.com/palvelinten-hallinta/#h0-oikeasti-netissa)

Raportin kirjoittaminen Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/ 

H1 Hei Ansiblen maailma. Luettavissa:
[Downloading Debian. https://www.debian.org/download](https://github.com/klaudistefi/palvelinten-hallinta/blob/main/h1_hei_ansiblen_maailma.md )

h3 Hello Web Server. Luettavissa:
https://github.com/klaudistefi/linux-course/blob/main/h3_hello_web_server.md

Install and Configure Apache. Luettavissa: https://ubuntu.com/tutorials/install-and-configure-apache#1-overview
