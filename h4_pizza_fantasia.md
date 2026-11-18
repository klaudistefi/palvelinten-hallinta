**Kurssi:** Palvelinten Hallinta ICI001AS3A-3017 <br>
**Tekijä:** Klaudija Majamäki <br>
**Käyttöjärjestelmä:** Pöytätietokone, Windows 11 -kone <br>
**Virtuaalikone:** Oracle Virtual Box Debian <br>



# h4 Pizza Fantasia
**17.11.2026** <br>
**18.11.2026**


# x) Tiivistelmät

### Karvinen 2023: Configuration Management of Distributed Systems over Unreliable and Hostile Networks (pdf, ̣mirrors: local, archive.org), vain nämä kohdat:

4.12.1 Size and Complexity of Some DSLs (112. Ominaisuuksien määrä.) <br>
4.12.2 Use of DSL Functions in Case Configuration (112-115. Mitä oikeasti käytetään.) <br>
4.12.3.1 Dependencies Between Main Functions (115-117. Tärkeimmät rakennuspalikat.) <br>

 # a) Räpylä.

**Tehtävä:** Asenna itse valitsemasi demoni käsin. Jokin muu kuin tunnilla tai kotitehtävissä aiemmin asennettu, eli ei apache, ngninx eikä openssh-server. Kuten aina, testaa lopputulos.

MariaDB on minun valinta. MariaDB on tietokannan hallintajärjestelmä (Database Management System, DBMS). Sen avulla voi luoda tietokantoja ja tauluja, tallentaa tietoa, etsiä tietoja, muokata tietoja.

Päivitin pakettilistan - sudo apt update. Asensin MariaDB-palvelimen - apt install mariadb-server. Tarkistin, että MariaDB oli käynnissä - sudo systemctl status matiadb. Tila - active (running).

<img width="546" height="109" alt="image" src="https://github.com/user-attachments/assets/0171ede6-5002-4d4a-8149-ff274f7d3965" />

<img width="944" height="292" alt="image" src="https://github.com/user-attachments/assets/1948180c-83af-4e66-ba46-d351450eb03a" />

Kirjauduin MariaDB - sudo mariadb. 

Miten toimi: sudo antaa järjestelmänvalvojan oikeudet ja mariadb avaa tietokannan hallintakonsolin.

Testitietokannan saa luotu CREATE DATABASE omapaikkadb;. Siirryin tietokantaan USE omapaikkadb;. exit, että voi poistu MariaDB sta.

Toimi.

<img width="944" height="254" alt="image" src="https://github.com/user-attachments/assets/98457c02-f11c-4049-aeb9-c1283c8f408f" />

<img width="944" height="128" alt="image" src="https://github.com/user-attachments/assets/b8ad19ea-c75a-4d53-9a83-d733491ab9b4" />



```
sudo apt update
sudo apt install mariadb-server
sudo systemctl status mariadb
sudo mariadb
CREATE DATABASE omapaikkadb;
USE omapaikkadb;
exit
```






 # b) Automaatti.

**Tehtävä:** Automatisoi valitsemasi demonin asennus Ansiblella.

Avasin ansible kansion - cd ansible. Tein uuden kansion MariaDB roolia varten - mkdir -p mariadb/tasks.

<img width="944" height="358" alt="image" src="https://github.com/user-attachments/assets/e2862a75-d340-4e6c-b09e-1badb6c5e032" />

Ja lisäsin main.yml tiedostoon MariaDB tarvittavat tehtävät. 

<img width="584" height="359" alt="image" src="https://github.com/user-attachments/assets/8e8f8cab-b7c0-47d9-832a-2b877697e20e" />

Ansible kansiossa olevaa site.yml tiedostoa ja lisäsin uuden MariaDB roolin mukaan.

<img width="944" height="212" alt="image" src="https://github.com/user-attachments/assets/597277bc-913b-4ac8-9f0b-5f2000b0a45e" />

Käynnistin playbookin - ansible-playbook site.yml --ask-become-pass. VIRHE. 

<img width="944" height="437" alt="image" src="https://github.com/user-attachments/assets/2232a223-f956-4df7-ae86-524a68324f64" />

Tarkistin site.yml tiedoston syntaksin - ansible-playbook site.yml --syntax-check. Syntaksissa ei ollut virheitä. Sitten taas - ansible-playbook site.yml -K. VIRHE.


<img width="944" height="463" alt="image" src="https://github.com/user-attachments/assets/a4cf35a9-d911-466f-8e1f-c0bcb9ce30a5" />

<img width="944" height="410" alt="image" src="https://github.com/user-attachments/assets/545dc45a-956b-428b-b10f-2b98c74d9472" />

Yritin korjata virheitä useita kertoja. Muokkasin main.yml tiedostoa paljon, mutta en saanut sitä heti mitään. 

### TESTI ja toimiva ratkaisu

Muokkasin - main.yml uudeleen. Hallelujah. Korjauksin jälkeen alkanut toimimaan.

<img width="402" height="426" alt="image" src="https://github.com/user-attachments/assets/e4affb92-2f26-4256-9e99-346b304d16a7" />

<img width="944" height="461" alt="image" src="https://github.com/user-attachments/assets/57e5b2cf-d539-45de-a9a9-778812f08514" />





```
cd ansible
cd rolses
mkdir -p mariadb/tasks 
cd mariadb
micro main.yml
micro site.yml
ansible-playbook site.yml --ask-become-pass
ansible-playbook site.yml --syntax-check
ansible-playbook site.yml -K
```


 # c) Asetus.

**Tehtävä:** Muuta asetustiedostoa herralla (master, "control node") ja aja ansible uudestaan. Osoita, että asetukset tulivat käyttöön.

Muokkasin MariaDB max_connections asetusta. Se määrittää monta yhteyttä MariaDB palvelimelle voi olla samanaikaisesti. Tein MariaDB roolin sisälle uuden files kansion - mkdir -p roles/mariadb/files. Avasin uuden asetustiedoston - micro roles/mariadb/files/maria-edit.cnf. 

Lisäsin sinne:
[mysqld]
max_connections = 200.

[mysqld] - asetukset koskevat MariaDB palvelinta. max_connections = 200 sallii enintään 200 samanaikaista tietokantayhteyttä. Oletusarvo on yleensä 151. Arvo 200 auttaa tarkistamaan, että Ansible on muuttanut asetuksen.

<img width="944" height="86" alt="image" src="https://github.com/user-attachments/assets/48c2509b-747b-467c-b8ff-30611d49b02c" />

maria.edit.cnf sisällä.

<img width="944" height="129" alt="image" src="https://github.com/user-attachments/assets/9d24d48a-8616-417f-884f-72c0d3a380c6" />

Uusi tehtävä MariaDB roolissä main.yml - micro roles/mariadb/tasks/main.yml.

<img width="944" height="605" alt="image" src="https://github.com/user-attachments/assets/9879756c-3b1a-47c5-a776-0c7f0f0780be" />


Uusi handleri, joka käynnistää MariaDB palvelun uudelleen, jos asetustiedosto muuttuu. Miksi? Koska Handler on erikoistehtävä, joka suoritetaan vain, jos joku sitä kutsuva tehtävä ilmoittaa muutoksesta.Uusi handlers kansio - mkdir -p roles/mariadb/handlers.

<img width="903" height="56" alt="image" src="https://github.com/user-attachments/assets/7b50c6a1-8271-423e-8753-38b6c7bcd48e" />

Handler-tiedoston - micro roles/mariadb/handlers/main.yml.

<img width="944" height="166" alt="image" src="https://github.com/user-attachments/assets/499353f0-9e78-4bf4-a10c-71db90c99af8" />

Käynnistin Ansible playbookin uudelleen - ansible-playbook site.yml -K.

<img width="944" height="98" alt="image" src="https://github.com/user-attachments/assets/326eef8c-1dee-46e5-ad2f-69fb304c116a" />

### TESTI

Tarkistin uuden asetuksen - sudo mariadb.  MariaDB [(none)]>, laitetaan: SHOW VARIABLES LIKE “max_connections”; 

Silti virhe. Käynnistin virtuaalikoneen uudelleen. Asetus alkoi toimia!

P.S Muokkasin max_connections arvon 200 sta 163 een.

<img width="944" height="86" alt="image" src="https://github.com/user-attachments/assets/0d0d277e-da9b-402a-809d-87bd064cde40" />

<img width="944" height="257" alt="image" src="https://github.com/user-attachments/assets/a625914f-b630-42bb-99e4-278eeb6d3f4a" />


```
mkdir -p roles/mariadb/files
micro roles/mariadb/files/maria-edit.cnf
[mysqld]
max_connections = 200.
micro roles/mariadb/tasks/main.yml
mkdir -p roles/mariadb/handlers
micro roles/mariadb/handlers/main.yml
ansible-playbook site.yml -K
sudo mariadb
SHOW VARIABLES LIKE “max_connections”;
```




 # d) Paikka remonttiin.

**Tehtävä:** Riko jotain asetuksia. Voit esimerkiksi poistaa demonin 'sudo apt-get purge foobar' (purge poistaa myös asetustiedostoja), poistaa asennuksen yhteydessä tulevan kansion (sudo rm -r /etc/foobar/ # vaarallista) tms. Ja sitten ajaa ansible-roolisi uudestaan ja todeta, että se korjaa tilanteen.

Poistin MariaDB asetustiedoston, jonka olin luonut - sudo rm /etc/mysql/mariadb.conf.d/maria-edit.cnf. Tarkistin, että tiedosto oli poistettu -  ls /etc/mysql/mariadb.conf.d/. Oma maria-edit.cnf asetus poistui.

<img width="944" height="96" alt="image" src="https://github.com/user-attachments/assets/6f9a08c5-3826-40c4-afef-3eef3ca52910" />

Playbook - ansible-playbook site.yml -K.

Ansiblella pystyy palauttamaan poistetun tiedoston. Ansible huomaa, että tiedosto puuttuu palvelimelta ja kopioi sen uudelleen control nodella olevasta roles/mariadb/files/ kansiosta.

Toimi.

<img width="944" height="115" alt="image" src="https://github.com/user-attachments/assets/abb08aa7-603b-4f89-930c-ab99fe51fdad" />


```
sudo rm /etc/mysql/mariadb.conf.d/maria-edit.cnf
ls /etc/mysql/mariadb.conf.d/
ansible-playbook site.yml -K
```




 # e) Idempotentti.

**Tehtävä:** Osoita, että tilasi on idempotentti.

Idempotentti - Ansiblea voi ajaa useita kertoja ilman, että se tekee muutoksia. Jos kaikki asetukset ovat jo oikein Ansible ei muuta niitä uudelleen.

Testasin suorittamalla playbookin kaksi kertaa - ansible-playbook site.yml -K. Tulos oli sama ok=15 changed=0 unreachable=0 failed=0.

  
 <img width="944" height="72" alt="image" src="https://github.com/user-attachments/assets/2391d229-1fa0-4f7e-8c24-282270c62228" />

<img width="944" height="77" alt="image" src="https://github.com/user-attachments/assets/205bdcc6-97b8-4ed5-997b-df66cc806ac4" />

 

```
ansible-playbook site.yml -K
```






# Lähteet:
Tero Karvinen - Palvelinten Hallinta. Luettavissa: https://terokarvinen.com/linux-palvelimet/ Luettu: 25.10.2026

Raportin kirjoittaminen Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/ Luettu: 25.10.2026

Configuration Management of Distributed Systems over Unreliable and Hostile Networks. Luettavissa: https://westminsterresearch.westminster.ac.uk/item/w7vvz/configuration-management-of-distributed-systems-over-unreliable-and-hostile-networks Luettu: 15.11.2026 

MariaDB. Luettavissa: https://wiki.debian.org/action/show/MariaDB?action=show&redirect=Mariadb Luettu: 17.11.2026
Install and Configure MariaDB on Debian 13. Luettavissa: https://www.hostmycode.com/tutorials/install-and-configure-mariadb-on-debian-13 Luettu: 17.11.2026

Syntax-check. Luettavissa: https://docs.ansible.com/projects/lint/rules/syntax-check/ Luettu: 18.11.2026

mariadb Client. Luettavissa: https://mariadb.com/docs/server/clients-and-utilities/mariadb-client  Luettu: 18.11.2026

What does cache=yes mean in apt: update_cache=yes in Ansible? Luettavissa: https://unix.stackexchange.com/questions/487400/what-does-cache-yes-mean-in-apt-update-cache-yes-in-ansible Luettu: 18.11.2026

Mitä `sudo rm -rf /` tekee Linuxilla? Luettavissa: https://www.reddit.com/r/AskProgramming/comments/1cgkbu2/what_does_sudo_rm_rf_do_on_linux/?tl=fi Luettu: 18.11.2026

What is idempotency in Ansible? Luettavissa:   https://medium.com/@haroldfinch01/what-is-idempotency-in-ansible-9d264c116193 Luettu: 18.11.2026

