**Kurssi:** Palvelinten Hallinta ICI001AS3A-3017<br>
**Tekijä:** Klaudija Majamäki <br>
**Käyttöjärjestelmä:** Pöytätietokone, Windows 11 -kone <br>
**Virtuaalikone:** Oracle Virtual Box Debian

# H3 Demoni
**5.11.2026**  <br>

## x) Tiivistelmät 

### Karvinen 2026: Apache installed with Ansible - quick notes

- Käytetään kolme vaihetta, apachen asenuksessa. Asennus, asetustiedostojen lisääminen ja käynnistäminen.
- Muutokset astuvat voimaan vasta, kun demoni on käynnistetty uudelleen
- apt asentaa ohjelman

 
### Ansible Community Documentation: Handlers: running operations on change
Handlers: running operations on change (johdantokappale pääotsikon alta)

- SE suorittaa tehtävän vain kun jokin toinen tehtävä ilmoittaa muutoksesta

Notifying handlers

- notify, se tehtävä ilmoittaa handlerille, että sen pitää suorittaa oma tehtävä.

### 'ansible-doc service':
johdantokappale (MODULE alta)

- enabled

- name

- state

- EXAMPLES




## a) Apassi. 
Tehtävä: Asenna Apache 2 käsin. Weppisivun tulee näkyä palvelimen etusivulla. Sivun tulee olla tavallisen käyttäjän muokattavissa, ilman root- tai sudo-oikeuksia.

Olin asentanut Apache2 virtuaalikoneelle jo edellisessä tehtävässä. Apache näytti jo omaa tekemääni testisivua, joten uutta sivua ei tarvinnut luoda. [Linkki tässä.](https://terokarvinen.com/palvelinten-hallinta/#h0-oikeasti-netissa) Tarkistin missä sivu sijaitsee ja mitkä ovat sen käyttöoikeudet ls -l /var/www/html/.

<img width="944" height="102" alt="image" src="https://github.com/user-attachments/assets/337bcad2-b4fc-45af-bb1a-b693da4d62df" />

index.html tiedoston omistaja on root. Tiedoston oikeudet -rw-r--r--. Tavallinen käyttäjä pystyi lukemaan tiedostoa, mutta ei muokkaamaan sitä. Muutin tiedoston omistajaksi  sudo chown klaudi:klaudi /var/www/html/index.html. chown muuttaa tiedoston omistajaa. klaudi tarkoittaa uutta omistajaa ja toinen klaudi ryhmää. Avasin tiedoston micro editorilla ilman sudo oikeuksia micro /var/www/html/index.html.
Muokkasin sivun otsikkoa ja tallensin. Tarkistin, että muutokset näkyivät curl http://localhost.

Sivu toimi oikein. Pystyin muokkaamaan sitä ilman sudo oikeuksia.

<img width="944" height="759" alt="image" src="https://github.com/user-attachments/assets/7f04711c-0bfc-4cd7-95c6-4f04e59143bf" />
<img width="944" height="314" alt="image" src="https://github.com/user-attachments/assets/5dcec716-d26d-43f8-82da-a9233f7a5f6b" />


```
curl localhost
ls -l /var/www/html/
sudo chown klaudi:klaudi /var/www/html/index.html
ls -l /var/www/html/
micro /var/www/html/index.html
curl localhost
```

## b) Moottorix. 
Tehtävä: Asenna Nginx käsin. Weppisivun tulee näkyä palvelimen etusivulla. Sivun tulee olla tavallisen käyttäjän muokattavissa, ilman root- tai sudo-oikeuksia. (Muista sammuttaa Apache ensin.)


Pitää asentaa Nginx. Pitää pysäyttää Apache, että se vapauttaa portin 80. Pysäytin Apache sudo systemctl stop apache2. Tarkistin tilan systemctl status apache2. 

Näky inactive (dead). Apache ei ollut enää käynnissä. Asensin Nginxin sudo apt update ja sudo apt install nginx. Tarkistin onko Nginx käynnissä sudo systemctl status nginx. 

Toimi.

Edellisessä tehtävässä olin jo muuttanut /var/www/html/index.html tiedoston. Näin tiedosto oli jo tavallisen käyttäjän muokattavissa ilman sudo oikeuksia. 

<img width="944" height="365" alt="image" src="https://github.com/user-attachments/assets/d551d268-6f0b-41ff-a54d-a6c4756955ce" />
<img width="944" height="225" alt="image" src="https://github.com/user-attachments/assets/e8a9baab-3640-4b60-808d-99c4a3ba35cd" />

```
sudo systemctl stop apache2
systemctl status apache2
sudo apt update
sudo apt install nginx
sudo systemctl status nginx
```

## c) Automoottorix. 
Tehtävä: Automatisoi Nginx asennus Ansiblella. Ylläpitäjän osuus Ansiblella riittää, itse HTML-weppisivut voi tehdä käsin.

Pitää automatisoida Nginxin asennus Ansiblella. Ansiblen piti asentaa Nginx, käynnistää palvelu ja varmistaa, että se käynnistyy automaattisesti. Minulla oli jo Ansible projekti hakemisto /ansible. Siellä tiedostot hosts.ini, ansible.cfg ja site.yml sekä roles kansio. Siirryin Ansible hakemistoon. T arkistin sen sisällön cd ansible ja ls -R. Ja tarkistin site.yml tiedoston cat site.yml.

<img width="944" height="98" alt="image" src="https://github.com/user-attachments/assets/c909cfcd-5476-4e73-8d17-c19580b5db9e" />
<img width="944" height="324" alt="image" src="https://github.com/user-attachments/assets/4eab6cd2-4ed4-4b49-a7f9-5ac5958326d4" />

Tein Nginxille oman roolin Ansible Galaxy komennolla ansible-galaxy role init roles/nginx.

Komento antaa Nginx valmiin hakemisto rakenteen ja tarvittavat tiedostot. Tein Nginxr oolin site.yml tiedostoon ja muokkasin.

<img width="944" height="647" alt="image" src="https://github.com/user-attachments/assets/c8978589-c4a2-4e66-b926-a9b955a3ed4d" />

Avasin tiedoston micro site.yml ja lisäsin nginx listaan. 

<img width="944" height="275" alt="image" src="https://github.com/user-attachments/assets/904c3d20-df60-40e8-9ba4-d5a1626b1e73" />


Lisäsin Nginxin tehtävät tiedostoon micro roles/nginx/tasks/main.yml.

<img width="940" height="243" alt="image" src="https://github.com/user-attachments/assets/fa073db0-bf60-474b-bf4b-0c739e8221b5" />

### Testi

Testasin komennolla ansible-playbook site.yml. Oli kirjoitusvirheitä ja puuttuvia välilyöntejä, korjasin niitä. Ansible ilmoitti, että sudo salasana puuttuu. Kokeilin ansible-playbook site.yml --ask-become-pass.

<img width="944" height="817" alt="image" src="https://github.com/user-attachments/assets/879b3bcd-b573-4cd9-b841-4fac49232586" />
<img width="634" height="327" alt="image" src="https://github.com/user-attachments/assets/006f8297-ba7c-4aa6-a479-3aa5c41105b1" />
<img width="944" height="150" alt="image" src="https://github.com/user-attachments/assets/9ec40ddb-6461-4321-96cc-e75c31b30a99" />

Myös huomasin, että Ansible yritta suorittaa Apache ennen Nginx. Muutin roolien järjestystä site.yml tiedostossa. Tuli vielä virhe, joka liittyi vanhaan service rooliin. En saanut sitä korjattua, joten poistin kyseisen roolin site.yml tiedostosta.

Toimi.

<img width="435" height="504" alt="image" src="https://github.com/user-attachments/assets/2e848252-d632-4853-9992-79ba2199ec75" />
<img width="944" height="214" alt="image" src="https://github.com/user-attachments/assets/a40b5f62-6b18-46f3-9e0c-6048fac91a11" />
<img width="944" height="528" alt="image" src="https://github.com/user-attachments/assets/c78c7fc6-a4ab-45e8-a550-a5ad59cccf9b" />


```
cd ansible
ls -r
cat site.yml
ansible-galaxy role init roles/nginx
tree
micro site.yml.
micro roles/nginx/tasks/main.yml
ansible-playbook site.yml
ansible-playbook site.yml --ask-become-pass
ansible-playbook site.yml -K

```

### Hetkinen – Handlers

Tässä vaiheessa huomasin, että en ollut vielä tehnyt tehtävää kaikkien ohjeiden mukaan. Ansible handlers toimintoon puuttuu.

Handlers toiminnolla voidaan suorittaa tietty tehtävä silloin, kun asetustiedostoon on tullut muutoksia. Voidaan käynnistää uudelleen, jos sen asetuksia muutetaan.

Ansible galaxy komento oli jo luonut tarvittavan hakemistot. Muokkasin main.yml-tiedostoa sekä tasks että handlers kansiossa. Loin myös files kansioon nginxin asetustiedoston roles/nginx/files/example.com.conf.

<img width="404" height="217" alt="image" src="https://github.com/user-attachments/assets/b2f1d3ed-2363-4531-93c0-698cd8b5fae8" />

main.yml kansiossa task.

<img width="796" height="638" alt="image" src="https://github.com/user-attachments/assets/0d5681d5-bd00-4f7c-b7eb-2b7e4ea8d248" />

example.com.conf

<img width="842" height="359" alt="image" src="https://github.com/user-attachments/assets/0d9e88f7-db68-4668-9643-c4555ebc344f" />

Testasin uudelleen ansible-playbook site.yml -K ja curl localhost.

Toimi.

<img width="944" height="39" alt="image" src="https://github.com/user-attachments/assets/02d3ba0f-def8-4d1b-864b-4756e4a9f0aa" />
<img width="944" height="286" alt="image" src="https://github.com/user-attachments/assets/c88a6f08-09cc-4c1c-926b-4f63b737eabe" />




# Lähteet: 
Tero Karvinen – Palvelinten Hallinta. Luettavissa: https://terokarvinen.com/linux-palvelimet/

Raportin kirjoittaminen Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/

Apache installed with Ansible - quick notes. Luettavissa: https://terokarvinen.com/apache-ansible/

Handlers: running operations on change. Luettavissa: https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html

h2 Voileipä. Klaudija Majamäki. Luettavissa: https://github.com/klaudistefi/palvelinten-hallinta/blob/main/h2_voileipa.md 

Installing NGINX Open Source Luettavissa: https://docs.nginx.com/nginx/admin-guide/installing-nginx/installing-nginx-open-source 

Galaxy Developer Guide.  Luettavissa: https://docs.ansible.com/projects/ansible/latest/galaxy/dev_guide.html 
