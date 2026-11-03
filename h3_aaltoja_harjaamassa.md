# H3 Aaltoja harjaamassa

## x) Lue ja tiivistä

### Universal radio hacker SDR tutotial (Hubmartin, tammikuu 2019, 3.20 - 7.40 min)

Videolla Martinilla on käytössä Universal Radio Hacker sovellus, jonka toimintaa hän demonstroi kaappaamalla signaaleja kaukosäätimestä. Sen jälkeen Martin siirtyy analysoimaan kaappaamaan liikennettä. 
Martin analysoi kaappaamiensa (kaukosäätimen) signaalien aallon pituutta, bittien järjestystä ja signaalien eheyttä. Jokseenkin hankalan näköinen sovellus näin äkkiseltään.  

### Decode weather station data (Cornelius 2022) 

Artikkelissa puretaan Lidlistä ostetun Auriol merkkisen sääaseman dataa. Projekti alkaa kun kytketään asema tietokoneeseen kiinni ja napataan signaalia rtl_433 softalla, joka tunnisti protokollat automaattisesti. 
Sen jälkeen kirjoittaja lähtee purkamaan signaaleja vielä syvemmin URH softaan, joka jälleen vaikutti omiin silmiin tässä vaiheessa vielä hieman monimutkaiselta. 
Pitkän teknisen tekstin jälkeen päädyttiin lopputulokseen, että nykyään softaradioilla ja urh työkalulla voidaan napata ja purkaa signaaleja entistä helpommin.  

## b) rtl_433 

Tehtävässä on tarkoituksena asentaa rtl_433 softa. Softan, sen asennusohjeet, troubleshootin ja manuaalit löytää kaikki githubista. 
Asennus tapahtuu linuxissa (itselläni käytössä Kali) komennolla "sudo apt install rtl-433. 
Loppuun vielä demo, että asennus on suoritettu onnistuneesti.  

<img width="614" height="197" alt="kuva" src="https://github.com/user-attachments/assets/25be2ab5-cb77-4ce0-bf78-56c10f65e9f3" />  

---  

## c) Automaattinen analyysi 

Tehtävään Karvinen on tarjonnut 'Converted 433.92M 2000k.cs8' tiedoston, jota olisi tarkoitus analysoida. Mitä näytteessä tapahtuu? Mitä tunnisteita löytyy? 
Annetun tiedoston lukeminen onnistui komennolla "rtl_433 -r <tiedoston_nimi>", jonka jälkeen avautui seuraaava näkymä.  

<img width="950" height="908" alt="Näyttökuva 2026-11-03 182756" src="https://github.com/user-attachments/assets/017d73c0-1628-418b-b810-95b9afcb610a" />  

---  

Näytteessä on muutama signaali napattuna, ja koska niitä on vain noin vähän, voitaisiin päätellä että näytettä ei ole nauhoitettu kauaa. 
Signaalien aikaväli on myös tiheä, kuten vasemmasta reunasta voi tulkita. 
Voisiko kyseessä olla kaukosäädin, jossa painetaan virtanäppäintä? Perustelen väitettä lähinnä "State: OFF" kentän pohjalta.  

## d) Too complex 16? 

Tehtävään on annettu tiedosto, joka on nauhoitettu URH softalla .complex16s muotoon. Nyt olisi tarkoitus muuntaa näyte 
rtl_433 yhteensopivaksi ja analysoida sitä. Ainoa jippo joka täytyi tehdä, että tiedoston sai rtl_433 ajettavaski oli vaihtaa tiedoston loppupääte .complex16s -> .cs8. 
Sen jälkeen softa osaa suoraan tulkita näytettä.  

<img width="1037" height="906" alt="Näyttökuva 2026-11-03 185212" src="https://github.com/user-attachments/assets/87d1519a-924f-4f89-8c99-efd952d5c5a0" />  

---  

Näyte vaikuttaisi olevan identtinen. C tehtävän näyte taisi olla vain käännetty versio tästä "alkuperäisestä".  

## e) Ultimate 

Tehtävänantona on asentaa URH, universal radio hacker. Se onnistuu komennolla "pipx install urh".  

<img width="1036" height="81" alt="Näyttökuva 2026-11-03 185739" src="https://github.com/user-attachments/assets/4eca5052-761c-49ac-ac2c-41d427a9d122" />  

---  

Vaan eipä onnistunutkaan. Kaliin ei jostain syystä tuo urhin asennus sopinut, enkä nyt ollut troubleshoottaus tuulella Kalin suhteen, joten vaihdoin Debianiin.  

<img width="1919" height="967" alt="Näyttökuva 2026-11-03 193209" src="https://github.com/user-attachments/assets/da386d58-0af2-477d-813f-473b670bf340" />  

---  

## f) Yleiskuva 

Nyt olisi tarkoitus analysoida URH:illa Karvisen tarjoamaa materiaalia. Kuinka pitkä? Millä taajudella? Milloin nauhoitettu? Miltä näyte silmämääräisesti näyttää? 











## Lähteet
1. hubmartin, tammikuu 2019. Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs. Video katsottavissa: https://www.youtube.com/watch?v=sbqMqb6FVMY&t=199s. Katsottu 3.11.2026.
2. Cornelius 2022. Decode 433.92 MHz weather station data. Luettavissa: https://www.onetransistor.eu/2022/01/decode-433mhz-ask-signal.html. Luettu 3.11.20266.
3. https://github.com/merbanan/rtl_433
4. https://github.com/jopohl/urh
