**Kurssi:** Palvelinten Hallinta ICI001AS3A-3017<br>
**Tekijä:** Klaudija Majamäki <br>
**Käyttöjärjestelmä:** Pöytätietokone, Windows 11 -kone <br>
**Virtuaalikone:** Oracle Virtual Bpx Debian

# H1 Hei Ansiblen maailma
**27.10.2026**  
**Aloitusaika**: 23:00  
**Lopetusaika**: 20:00  
**25.8.25**  
**Aloitusaika**: 15:40  
**Lopetusaika**: 16:20  

a) Asenna Debian 13 paikallisesti virtuaalikoneeseen. Voit käyttää esimerkiksi VirtualBoxia.

- VirtualBoxia olis jo asenettu tietokonella. Asensin uusi Debain VM.
- Base Memory: 4090 MB, Number of CPUs: 2, Disk Size: 50 GB, Use EFI: check ja Next.

-  nimi debian-palvelinten-hallinta-2026
-  Debian



b) Sshecrets. Asenna SSH-demoni käsin ja testaa se kirjautumalla SSH:lla. (localhost kelpaa)

c) Pubkey. Automatisoi ssh-kirjautuminen julkisella avaimella. (localhost kelpaa)

d) Hei Ansible. Tee hei maailma ansiblella ja kokeile sitä SSH:n yli. (localhost kelpaa)

e) Vapaaehtoinen bonus: kokeile Ansiblella jokin näista asetuksista: paketin asennus, asetustiedosto /etc/ alle, käynnistä jokin demoni, tee uusi käyttäjä. Tarvitset todennäköisesti sudoa, become: true.
