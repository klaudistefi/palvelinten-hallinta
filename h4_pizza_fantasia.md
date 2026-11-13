**Kurssi:** Palvelinten Hallinta ICI001AS3A-3017 <br>
**Tekijä:** Klaudija Majamäki <br>
**Käyttöjärjestelmä:** Pöytätietokone, Windows 11 -kone <br>
**Virtuaalikone:** Oracle Virtual Box Debian <br>



# h4 Pizza Fantasia
**15.11.2026** <br>
**1.11.2026**



# x) Tiivistelmät

### Karvinen 2023: Configuration Management of Distributed Systems over Unreliable and Hostile Networks (pdf, ̣mirrors: local, archive.org), vain nämä kohdat:

4.12.1 Size and Complexity of Some DSLs (112. Ominaisuuksien määrä.)
4.12.2 Use of DSL Functions in Case Configuration (112-115. Mitä oikeasti käytetään.)
4.12.3.1 Dependencies Between Main Functions (115-117. Tärkeimmät rakennuspalikat.)

 # a) Räpylä.

Tehtävä: Asenna itse valitsemasi demoni käsin. Jokin muu kuin tunnilla tai kotitehtävissä aiemmin asennettu, eli ei apache, ngninx eikä openssh-server. Kuten aina, testaa lopputulos.

```

```
 # b) Automaatti.

Tehtävä: Automatisoi valitsemasi demonin asennus Ansiblella.




```

```


 # c) Asetus.

Tehtävä: Muuta asetustiedostoa herralla (master, "control node") ja aja ansible uudestaan. Osoita, että asetukset tulivat käyttöön.



```

```




 # d) Paikka remonttiin.

Tehtävä: Riko jotain asetuksia. Voit esimerkiksi poistaa demonin 'sudo apt-get purge foobar' (purge poistaa myös asetustiedostoja), poistaa asennuksen yhteydessä tulevan kansion (sudo rm -r /etc/foobar/ # vaarallista) tms. Ja sitten ajaa ansible-roolisi uudestaan ja todeta, että se korjaa tilanteen.



```

```




 # e) Idempotentti.

Tehtävä: Osoita, että tilasi on idempotentti.



```

```



# Lähteet:
Tero Karvinen - Palvelinten Hallinta. Luettavissa: https://terokarvinen.com/linux-palvelimet/ Luettu: 25.10.2026

Raportin kirjoittaminen Luettavissa: https://terokarvinen.com/2006/raportin-kirjoittaminen-4/ Luettu: 25.10.2026

Configuration Management of Distributed Systems over Unreliable and Hostile Networks. Luettavissa: https://westminsterresearch.westminster.ac.uk/item/w7vvz/configuration-management-of-distributed-systems-over-unreliable-and-hostile-networks Luettu: 15.11.2026
