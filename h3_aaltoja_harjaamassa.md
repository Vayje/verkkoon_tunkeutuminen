# H3 Aaltoja harjaamassa

## x) Lue ja tiivistä

### Universal radio hacker SDR tutorial (Hubmartin, tammikuu 2019, 3.20 - 7.40 min)

Videolla Martinilla on käytössä Universal Radio Hacker sovellus, jonka toimintaa hän demonstroi kaappaamalla signaaleja kaukosäätimestä. Sen jälkeen Martin siirtyy analysoimaan kaappaamaansa liikennettä. 
Martin analysoi kaappaamiensa (kaukosäätimen) signaalien aallon pituutta, bittien järjestystä ja signaalien eheyttä. Jokseenkin hankalan näköinen sovellus näin äkkiseltään.  

### Decode weather station data (Cornelius 2022) 

Artikkelissa puretaan Lidlistä ostetun Auriol merkkisen sääaseman dataa. Projekti alkaa kun kytketään asema tietokoneeseen kiinni ja napataan signaalia rtl_433 softalla, joka tunnisti protokollat automaattisesti. 
Sen jälkeen kirjoittaja lähtee purkamaan signaaleja vielä syvemmin URH softaan, joka jälleen vaikutti omiin silmiin tässä vaiheessa vielä hieman monimutkaiselta. 
Pitkän teknisen tekstin jälkeen päädyin lopputulokseen, että nykyään softaradioilla ja urh työkalulla voidaan napata ja purkaa signaaleja entistä helpommin.  

## b) rtl_433 

Tehtävässä on tarkoituksena asentaa rtl_433 softa. Softan, sen asennusohjeet, troubleshootin ja manuaalit löytää kaikki githubista. 
Asennus tapahtuu Linuxissa (itselläni käytössä Kali) komennolla "sudo apt install rtl-433". 
Loppuun vielä demo, että asennus on suoritettu onnistuneesti.  

<img width="614" height="197" alt="kuva" src="https://github.com/user-attachments/assets/25be2ab5-cb77-4ce0-bf78-56c10f65e9f3" />  

---  

## c) Automaattinen analyysi 

Tehtävään Karvinen on tarjonnut 'Converted 433.92M 2000k.cs8' tiedoston, jota olisi tarkoitus analysoida. Mitä näytteessä tapahtuu? Mitä tunnisteita löytyy? 
Annetun tiedoston lukeminen onnistui komennolla "rtl_433 -r <tiedoston_nimi>", jonka jälkeen avautui seuraava näkymä.  

<img width="950" height="908" alt="Näyttökuva 2026-11-03 182756" src="https://github.com/user-attachments/assets/017d73c0-1628-418b-b810-95b9afcb610a" />  

---  

Näytteessä on muutama signaali napattuna, ja koska niitä on vain noin vähän, voitaisiin päätellä että näytettä ei ole nauhoitettu kauaa. 
Signaalien aikaväli on myös tiheä, kuten vasemmasta reunasta voi tulkita. 
Voisiko kyseessä olla kaukosäädin, jossa painetaan virtanäppäintä? Perustelen väitettä lähinnä "State: OFF" kentän pohjalta.  

## d) Too complex 16? 

Tehtävään on annettu tiedosto, joka on nauhoitettu URH softalla .complex16s muotoon. Nyt olisi tarkoitus muuntaa näyte 
rtl_433 yhteensopivaksi ja analysoida sitä. Ainoa jippo joka täytyi tehdä, että tiedoston sai rtl_433 ajettavaksi oli vaihtaa tiedoston loppupääte .complex16s -> .cs8. 
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

Nyt olisi tarkoitus analysoida URH:illa Karvisen tarjoamaa materiaalia. Kuinka pitkä? Millä taajudella? Milloin nauhoitettu? Miltä näyte silmämääräisesti näyttää? Avattuani näytetiedoston URH:illa, oli näkymä seuraavanlainen.  

<img width="943" height="865" alt="image" src="https://github.com/user-attachments/assets/51603738-b1c3-4753-8422-454dfeaf9096" />  

---  

Näyte on kaukosäätimen kliksauttelu, ja sen voisi päätellä signaalien tasaisesta kolmesta värähtely "blockista".  
Signaalin pituuden voimme laskea seuraavalla kaavalla: pituus = näytteiden määrä / näytteenottotaajuus. Näytteiden määrän saamme selville, kun painetaan vasemmassa yläkulmassa sijaitsevaa sinistä "i" nappia. Silloin avautuu signal details ikkuna, jossa on näytteiden määrä. Tässä tapauksessa se on 5491580. Näytteenottotaajuutta en löytänyt URH sisältä mistään, joten luotan Karvisen luoman tiedoston nimeen, jossa näytteenottotaajuudeksi on merkitty 2MHz. Tällöin näytteen pituudeksi tulisi 5491580 / 2000000 = 2,75s. (Kaava kysytty tekoälyltä. Claude Sonnet 5.5).  

<img width="886" height="464" alt="image" src="https://github.com/user-attachments/assets/0c2db43b-71e3-4964-badf-410d388f7e15" />  

---  

En myöskään löytänyt taajuutta tai nauhoitusaikaa muualta kuin tiedostonnimestä, joten nyt on luotettava Karvisen antamiin tietoihin. Taajuus on siis 433.912MHz, ja se on nauhoitettu 12.4.2025 klo 11.38.  

<img width="810" height="58" alt="image" src="https://github.com/user-attachments/assets/4adce7e9-cfc6-4646-9fa3-5f8d12117446" />  

---  

## g) Bittistä 

Tehtävässä tarkoituksena on demoduloida signaali niin, että siitä tulee raakabittejä. Mikä on oikea modulaatio? Miten pitkä yksi raakabitti on ajassa? 

Signaalin bitit saadaan näkyviin "Analysis" ikkunasta.  

<img width="945" height="612" alt="image" src="https://github.com/user-attachments/assets/a930ba3b-1e76-4949-af1b-8bfa5946f04b" />  

---  

Modulaatioksi päättelin ASK, koska signaali menee tasaisesti pois ja päälle, eikä sen runsaampia heittoja ole (Geeksforgeeks 2025). 

Raakabitin pituutta ajassa en osannut laskea, joten päädyin kysymään apua Claude Sonnet 5.5 tekoälymallilta. Kaava menee näin: bitin pituus = montako näytettä per symboli / näytteenottotaajuus. Näyte/Symboli saadaan URH interpretation välilehdeltä, ja taajuus taas tiedoston nimestä. 
Tässä tapauksessa se menee näin: 500 / 2000000 = 0,00025s. 

Tätä voisi verrata vaikka silmänräpäykseen, joka on noin 100-150ms (Scienceinsights 2025). Bitin pituus millisekunteina on 0,25ms, eli huomattavasti nopeampi kuin silmänräpäys.  


## Lähteet
1. hubmartin, tammikuu 2019. Universal Radio Hacker SDR Tutorial on 433 MHz radio plugs. Video katsottavissa: https://www.youtube.com/watch?v=sbqMqb6FVMY&t=199s. Katsottu 3.11.2026.
2. Cornelius 2022. Decode 433.92 MHz weather station data. Luettavissa: https://www.onetransistor.eu/2022/01/decode-433mhz-ask-signal.html. Luettu 3.11.2026.
3. https://github.com/merbanan/rtl_433 Luettu 3.11.2026
4. https://github.com/jopohl/urh Luettu 3.11.2026
5. Claude Sonnet 5.5 tekoälymalli
6. Geeksforgeeks heinäkuu 2025, Digital Modulation Techniques. Luettavissa: https://www.geeksforgeeks.org/electronics-engineering/digital-modulation-techniques/#phase-shift-keying. Luettu 4.11.2026.
7. Sciense Insights marraskuu 2025, How fast is a blink? Luettavissa: https://scienceinsights.org/how-fast-is-a-blink-the-science-behind-the-speed/. Luettu 4.11.2026
