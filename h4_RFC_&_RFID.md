# H4 RFC & RFID 

## a) Tarkastele käytössäsi olevia RFID-laitteita, ja analysoi suojautumisesi urkinnalta 

Ensimmäisenä mieleen tulee pankkikortin lähimaksu, ja sen käyttö myös mobiilipuhelimen kautta (Google Pay). Tämä taitaa olla itselläni aktiivisimmassa käytössä RFID laitteiden piiristä. 
Myös Haaga-Helian kirjastokorttini on skannattavassa muodosssa Tuudo sovelluksessa. Harvemmin käytössäni on passi, jossa on myös nykyään NFC-skannattava siru, joka täytti esimerkiksi juuri hetki sitten 
henkilötiedot automaattisesti UK ETA hakemukseen, jonka tarvitsi britteihin matkustamiseen. Muita RFID skannattavia objekteja omistuksessani on kaikki muut kortit (kela, kantiskortit jne.), joita harvemmin 
tulee käytettyä. Työpaikallakin tulee käytettyä kulkulätkää saadakseen pääsyn pukuhuoneeseen ja varastoon.  

Fyysiset kortit itselläni ei ole varsinaisesti suojattuina, pois lukien pankkikortin pin-koodi. Puhelimessa käytössä olevat maksusovellukset, kirjastokortit ym. ovat aina pinkoodin, salasanan ja/tai biometrisen 
tunnistuksen takana. Mielestäni olen suojautunut urkinnalta riittävästi, joskin aina parannettavaa löytyy. 
Jotta analyysi ei kuitenkaan jäisi liian tylsäski, lähdin tutkimaan miksi juuri Google Pay on turvallinen RFID implimentaatio. 

Google Payssa ei ikinä käytetä korttisi tietoja maksuun, vaan sovellus luo maksutapahtumaasi virtuaalisen tokenin, jota voi käyttää vaan ja ainoastaan siinä tilanteessa. Tässäkin sinun on tunnistauduttava vahvasti, 
ennenkuin saat maksukorttisi toimimaan sovelluksessa. Maksutapahtuman yhteys on päästä päähän salattu, eikä siihen teoriassa pitäisi ulkopuolisten päästä väliin. 
Kuulostaa turvallisesta. Jos Googleen voi luottaa. 

## b) Tutustu APDU komentojen rakenteeseen

APDU on lyhenne termistä Application Protocol Data Unit. APDU on kommunikointi protokolla, jota älykortti ja lukija käyttävät keskenään. Se määrittää datan rakenteen jota keskustelussa käytetään. 
APDU viestejä on kahta tyyppiä, jotka muodostavat yhdessä kysymys-vastaus parin: komento APDU ja vastaus APDU. Komento osiossa on kehys, joka kertoo vastaanottajalle minkälaista dataa ollaan lähettämässä. Se voi olla esimerkiksi luettavaa tai kirjoitettavaa dataa. 
Kehys kertoo myös lähetettävän datan pituuden, ja mahdolliset muut parametrit joita voidaan tarvita. Kuten voisi toisin luulla, komento APDU lähetetään esimerkiksi maksutapahtumassa kortinlukijasta korttiin, eikä toisinpäin. 

Vastaus APDU sen sijaan sisältää komento APDUN pyytämät (vaatimat) tiedot, ja kortti lähettää ne takaisin. Vastaus APDU voi sisältää (tai olla sisältämättä mitään dataa) minkälaista dataa tahansa. Se voi pitää sisällään tekstiä, binääridataa tai virheilmoituksia. 

Komento APDU koostuu neljän (4) tavun otsikosta. Tässä käytin hieman Claude Sonnet 5.5 tekoälymallia, joka avasi komennon toimintaa selkokielelle.  

| CLA | INS | P1 - P2 | 
|-----|-----|---------|
| Komentoluokka | Komento | Parametrit | 

- **CLA** = Kenelle ja millä säännöillä tämä viesti on tarkoitettu
- **INS** = Itse käsky, lue tai kirjoita 
- **P1-P2** = Tarkentavat parametrit, esimerkiksi kohta tiedostossa johon kirjoitettava data sijoitetaan

Otsikon jälkeen alkaa vaihtoehtoinen Body-osio, jossa on kolme kenttää.  

| Lc | Data | Le | 
|----|------|----| 

- **Lc** = Kertoo kuinka monta tavun dataa komennossa lähetetään 
- **Data** = Itse "lähetys", esimerkiksi pin-koodi 
- **Le** = Kuinka monta tavua dataa lukija odottaa vastaukseksi enintään 

Vastaus APDU:ssa on vain kaksi osaa.  

| Data | SW1-SW2 | 
|------|---------| 
| Vastattava data | Status Word |  

- **Data** = Pyydetty data, ei pakollinen kenttä 
- **SW1** = Kertoo tuloksen, onnistuiko, onko varoituksia tai virheitä
- **SW2** = Tarkentaa SW1, kertoo esimerkiksi paljonko siirrettävää dataa on jäljellä, mahdolliset virheilmoitukset. 

## c) Tutki ja kerro mielenkiintoisesta RFID hakkerointi uutisesta 

Selasin The Hacker Newsiä, ja nappasin sieltä artikkelin joka oli tuorein RFID kategoriassa. Se käsittelee takaovea raudan sisällä RFID korteissa joita käytetään hotelleissa ja toimistoissa ympräi maailmaa. 
Haavoittuvuus löytyy FM11RF08S kortista, jossa on Shanghai Fudan Microelectronicsin vuonna 2020 julkaisemasta MIFARE Classic variantti. Haavoittuvuuden löysi Quarklabsin tutkija. 
Haavoittuvuus sallisi ovien avauksen, eli pääsyn tuntemattomalla avaimella. Takaovi mahdollistaa kaikkien luotujen avainten selvittämisen, vaikka ne olisi diversifoitu. Vastaava haavoittuvuus löytyy myös 
aikaisemman sukupolven samantyylisestä kortista FM11RF08, jossa kyseistä takaovea on havaittu jo vuonna 2007 ensimmäisen kerran. 
Periaatteessa haavoittuvuutta voisi hyödyntää kuka tahansa, joka saa alkuperäisen kortin muutamaksi minuutiksi haltuunsa. 


## Lähteet
1. Wessels E. Is Google Pay secure? Luettavissa: https://nfcw-shop.com/blog/is-google-pay-safe/. Luettu 10.11.2026.
2. Olosunde A. 4.4.2023. Understanding APDU for Software Developers. Luettavissa: https://medium.com/@lovisgod/understanding-apdu-for-software-developers-b82c17cc890a. Luettu 10.11.2026. 
3. Cardlogix 2025. APDU (Application Protocol Data Unit). Luettavissa: https://www.cardlogix.com/glossary/apdu-application-protocol-data-unit-smart-card/. Luettu 10.11.2026 
4. Claude Sonnet 5.5 tekoälymalli
5. Lakshmanan R. 22.8.2024. Hardware Backdoor Discovered in RFID Cards Used in Hotels and Offices Worldwide. Luettavissa: https://thehackernews.com/2024/08/hardware-backdoor-discovered-in-rfid.html. Luettu 10.11.2026
