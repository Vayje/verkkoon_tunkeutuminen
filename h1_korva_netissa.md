#H1 Korva netissä

## x) Spektrini
Tehtävässä piirretään sähkömagneettinen spektri, ja havainnoidaan siitä muutamia eri kohtia sekä sovelluksia. Teen tämän hyvin yksinkertaisesti ilman varsinaista graafista kuvaa radioaallosta. 

-----------------Taajuus kasvaa--------------------->  
<---------------Aallon pituus kasvaa---------------  
<---------------Antennin koko kasvaa--------------  
<---------------Läpäisyvyys kasvaa----------------  
-----------------Kyky siirtää tietoa kasvaa------->  

Tässä muutamia sovellus- ja käyttöesimerkkejä sähkömagneettisesta spektristä järjestyksessä vasemmalta oikealle niin, että vasemalla on matalamman taajuuden esimerkit, ja oikealla korkeamman taajuuden.  

| Radiohaallot | Mikroaallot | Infrapuna | Näkyvä valo | UV | Röntgen | Gamma |
|---|---|---|---|---|---|---|
| Radio | Mikroaaltouuni | Kaukosäädin | Valaisin | UV-lamppu | Röntgenkuva | Säteilyhoito |  

## a) webDSR

Tehtävässä on tarkoitus etsiä webDSR palvelusta jokin vähintään 400km omasta etäisyydestä sijaitseva lähetys (mieluusti yleisradio, ei yksityinen), ja kertoa speksit millä sen löysi. 
Valitsin listalla heti ensimmäisenä löytyvän lähettimen, joka sijaitsee Twenten yliopistossa Hollannissa. Tämän avasin lähettimen sivuilla sijaitsevan oppaan jossa oli ohjeet radiokanavan löytämiseksi. 
Seurasin näitä uskollisesti. Ensiksi lähdin kuuntelemaan 7300kHz taajuutta (7.3MHz), ja vaihdoin modulaatiokseni AM. Sen jälkeen zoomasin waterfall osiota viisi kertaa, jotta signaalin löytäminen olisi hieman tarkempaa. 
Lähdin tämän jälkeen summanmutikassa rullailemaan, kunnes suhina loppuisi ja kuulisin jotain selkeämpää. Kuinkas ollakkaan, pienen hakemisen jälkeen löytyi musiikkia! 
Spekseillä AM-modulaatio, 9kHz aallonpituus ja 7210kHz taajudella löysin kiinalaisen radion.  

<img width="1181" height="493" alt="image" src="https://github.com/user-attachments/assets/d2a9d812-0b03-4609-8282-db78e6b6735d" />  

---  

## b) Moduwhat? 

Äsken löysin AM taajuuden kanavan, nyt etsitään FM taajuudelta jotain muuta kuin suhinaa. 
Toistin täysin samat vaiheet kuin aikaisemmassa vaiheessa, mutta valitsin vain FM modulaation, kuuluvan kanavan löytäminen oli jostain syystä kivuttomampaa! Käytin edelleen samaa hollantilaista lähetintä.  

<img width="1178" height="512" alt="image" src="https://github.com/user-attachments/assets/25c10515-9125-4572-9a45-7680fdf0a273" />  

## c) ADS-B

Tarkoituksena on etsiä ADS-B palvelusta jokin lentokone, kuvailla sitä ja sen lentoa, sekä kertoa miten tiedot lentokoneesta ovat saatu. 
Valitsin Suomen sisällä sijaitsevan lennon, ja tällä kertaa valikoitui kyseinen:  

<img width="791" height="804" alt="image" src="https://github.com/user-attachments/assets/14e47bb6-4a8c-4939-a53c-5874e8c391fe" />  

---  

Full details ikkunan avattua saamme auki kattavat tiedot koneesta. Sen fyysisest ominaisuudet, lentohistorian sekä muita tietoja jotka ovat maksumuurin takana. 
Vasemmassa palkissa kuvankaappauksessani nähdään koneen nykyinen korkeus, nopeus ja muita vastaavia tietoja.  

ADS-B on lyhenne termistä "Automatic Dependent Surveillance - Broadcast". Siinä käytetään hyödyksi lentokoneen sisäistä paikanninta, joka lähettää sijaintiaan automaattisesti ilman, että pilottien tarvitsee tehdä mitään ylimääräistä. Tätä käytetään hyödyksi ympäri maailmaa lennonjohdossa, ja muussa lennon liikenteen ohjauksessa tai paikantamisessa. 
Lentokoneissa oleva transponderi lähettää sijaintiaan 1090MHz taajuudella maassa sijaitsevaan radiomastoon. Toinen tapa on lähettää sitä satelliittien kautta. 










## Lähteet

1. https://terokarvinen.com/verkkoon-tunkeutuminen-ja-tiedustelu/
2. https://www.britannica.com/science/radio-wave
3. http://websdr.ewi.utwente.nl:8901
4. https://www.adsbexchange.com/

