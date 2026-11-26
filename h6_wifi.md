# H6 Wifi

## a) Wifi Challenge Lab 2.5

Raporttia varten latasin lehtori Lari Iso-Anttilan tarjoaman Wifi-Challenge-Lab-2.1 tiedoston, jossa oli valmis haastepaketti VirtualBoxiin sovitettuna.
Tarkoituksena oli suorittaa paketista kolme, tai enemmän erilaisia haasteita ja kirjoittaa niistä raportti. 

Valitsin itselleni seuraavat kolme haastetta: 
-  What is the channel that the wifi-global Access Point is currently using? Recon osio.
-  What is the MAC of the Wifi-IT client? Recon osio.
-  What is the flag on the AP router of the wifi-guest network? OPN osio.

Suoritin tehtävät tuossa järjestyksessä, joten ne ilmenee myös raportissa siinä järjestyksessä. 

### Channel of wifi-global Access Point 

Koska koko Wifi Challenges alusta oli minulle täysin tuntematon, enkä tiennyt edes miten aloittaa, päätin kysyä Claude Sonnet 5.5 tekoälymallilta, mikä olisi hyvä aloituspiste. 
Ensiksi etsin verkkoliitäntöjä komennolla "iw dev", ja löysinkin useita.  

<img width="759" height="568" alt="image" src="https://github.com/user-attachments/assets/c0a2fdde-86bd-4db1-bc70-a923fc50d66d" />  

---  

Päätin takertua wlan70 liitäntään, ja suoritin siihen komennon "sudo airodump-ng wlan70", joka kuuntelee wlan70 aseman lähettämiä recon paketteja. Se on ilmoituspaketti, joka antaa yhdistäville laitteille tiedot (nimi, kanava, salausmenetelmä). Airodump komento antoi välittömästi seuraavan näkymän:  

<img width="868" height="633" alt="image" src="https://github.com/user-attachments/assets/29655f5e-e657-460f-ae62-58a43c923467" />
  
---  

Tarkoituksena oli siis löytää mitä kanavaa wifi-global tällä hetkellä käyttää, ja se lukeekin listassa rivillä 7. Wifi-global on kanavalla 44, ja sen MAC-osoite on F0:9F:C2:71:22:17. Taulukon rivillä lukee myös ENC/CIPHER/AUTH sarakkeissa WPA2/CCMP/MGT, joka tarkoittaa, että verkkoon kirjautumisen todennus tehdään käyttäjätunnuksella ja salasanalla.  

### Wifi-IT client MAC-address 

Tätä haastetta on luonteva jatkaa äskeisestä taulukosta eteenpäin, sillä näin jo listalla wifi-IT laitetunnuksen. Komennolla "sudo airodump-ng -c 11 --bssid F0:9F:C2:1A:CA:25 wlan70" voimme kuunnella tarkemmin wifi-IT clientin liikennettä. Komennon osio -c 11 kuuntelee ainoastaan kanavaa 11, ja --bssid näyttää ainoastaan tämän tukiaseman ja siihen yhdistetyt laitteet.  

<img width="873" height="635" alt="image" src="https://github.com/user-attachments/assets/98179992-f8fe-4e94-8198-95c8afab172a" />  

---  

Näin saimme esiin Wifi-IT asiakkaan MAC-osoitteen STATION -osiosta. Vastaus on "10:F9:6F:AC:53:52".  

---  

### Flag on the AP router of the wifi-guest network  

Tätä tehtävää varten piti yhdistää jo aikaisemmin löydettyyn verkkoon, sillä pelkkä kuuntelu ei tuota haluttua tulosta. Se tapahtuu komennolla "sudo ip link set wlan0 up" & "sudo iw dev wlan0 scan | grep SSID". Ensimmäinen komento kytkee verkon päälle, jälkimmäinen komento kuuntelee wlan0 yhteyttä ja palauttaa sieltä kaikki tunnisteet joissa lukee SSID.  

<img width="606" height="565" alt="image" src="https://github.com/user-attachments/assets/adbb3b7a-bdb8-4b88-8ff7-f190a84d75f3" />  

---  

Yhdistin wlan0 verkkoon komennolla "sudo iw dev wlan0 connect wifi-guest", ja tarkistin että olen yhteydessä siihen komennolla "iw dev wlan0 link".  

<img width="571" height="302" alt="image" src="https://github.com/user-attachments/assets/144c45f6-9a77-46a7-8761-8f91b4fd442e" />  

---  

Seuraavaksi hain IP-osoitteen komennolla "ip addr show wlan0". Sain osoitteeksi 192.168.10.61/24, ja kokeilin firefoxiin jos reitittimen oletusosoite olisi 192.168.10.1, ja sehän tärppäsi.  

<img width="873" height="137" alt="image" src="https://github.com/user-attachments/assets/889105aa-f5ef-447e-be99-17a7e419adbc" />  

<img width="840" height="545" alt="image" src="https://github.com/user-attachments/assets/f9583448-3be2-4ef3-b23d-04b52f313d6c" />  

---  

Tässä vaiheessa asia lähti liian laukalle, enkä kirjautumissivusta löytänyt mitään mielenkiintoista edes lähdekoodista joten palasin terminaaliin. Suoritin komennon "sudo nmap -sV 192.168.10.1", joka näyttää kaikki avoimet palvelut kyseisessä osoitteessa. Seuraavaksi kokeilin porttiskannata komennolla "sudo nmap -sV -p 1-1000,8080,8443,2050 192.168.10.1".  

<img width="882" height="433" alt="image" src="https://github.com/user-attachments/assets/7212bfeb-8209-423a-a9ce-98cc5a588e83" />  

---  

Pitkän pitkän pitkän vääntämisen ja monen umpikujan jälkeen Claude löysi docker-kansiosta flagin joka oli salattu, ja skriptin joka purkaa kyseisen flagin.  

<img width="889" height="284" alt="image" src="https://github.com/user-attachments/assets/0cec0d8e-2fca-4c8c-8442-26df8161be1b" />  

---  

Vihdoin viimein flagi on löydetty. En aijo jokaista umpikujaa tähän dokumentoida sillä siitä tulisi kohtuuttoman pitkä. Tästä tehtävästä en pitänyt, ja koen etten juuri edes oppinut mitään. En tiennyt mistä lähteä tutkimaan, miten ja millä työkaluilla sillä tästä ei varsinaisesti opetettu mitään luennolla. Voi olla, että tekoälyn avulla ekysin vääriin reitteihin, sillä tämä tehtävä tuntui huomattavasti haasteellisemmalta kuin aikaisemmat, vaikka onkin haastelistan alkupäässä.  

<img width="765" height="363" alt="image" src="https://github.com/user-attachments/assets/c13272b7-8d9d-475a-99e0-8247e75c5a44" />  

<img width="478" height="726" alt="image" src="https://github.com/user-attachments/assets/0a5594f4-78a7-4d6e-b7ea-c02736015c25" />  

---  

## b) Oppimisen raportti Lab 2.1 -harjoituksesta 

Oppiminen jäi hieman ehkä rajalliseksi itselläni tässä tehtävässä. Tarkoituksena oli löytää flageja monimutkaisia reittejä pitkin, enkä juuri ymmärtäny mitä olin tekemässä vaikka tekoäly niitä minulle avasikin. Olisiko tehtävät olleet erilaisia ilman tekoälyn aikakautta? Olisiko työmäärä tällaisten asioiden oppimiseen ollut inhimillinen meille annetussa ajassa?  

Opin kuitenkin hieman verkkoyhteyksien skannailusta ja tunnisteista, mutta sen syvempi ymmärrys ja soveltaminen vaatii vielä runsaasti harjoittelua. Osittain ehkä turhautumiseen vaikutti myös ajan puute, kun monimutkaisia tehtäviä yrittää suorittaa päivätöiden ohella. Verkotus ei myöskään kuulu omiin vahvuuksiin, joten oltiin hieman enemmän pulassa. Hyödyllistä olisi myös ehkä ollut edetä tehtäviä järjestyksessä, mutta kolmen ensimmäisen suorittaminen olisi tuntunut riman alitukselta.  

---  

## c) Miten suhtautumien WLanin turvallisuuteen on muuttunut tämän myötä? 

Rehellisesti ei juurikaan! Ymmärrän että langaton verkko ei ole läheskään niin turvallinen kun voisi luulla, mutta se vaatii silti hurjasti teknistä tietämystä, ymmärrystä ja soveltamiskykyä että annetuilla skannauksilla saa mitään vahinkoa aikaan. Avoimet verkot ovat juuri nimensä veroisia, ne ovat avoimia. Niihin ja niiden käyttöön tulisi suhtautua silti varoen. On itsestään kiinni mitä tietoja sille haluaa antaa, ja millä laitteella siihen haluaa yhdistää. Yhdistäisinkö yrityssalaisuuksia sisältävällä läppärillä mihin tahansa avoimeen wifiin? En. Yhdistäisinkö omalla puhelimellani satunnaisen ravintolan verkkoon? Ehdottomasti, niinkuin teinkin viikko ennen raportin kirjoitusta Lontoossa. Se oli oikeastaan väistämätöntä ellei tahdo maksaa kalliita roaming laskuja.  

Kiteyttääkseni: Ei varsinaisesti ole muuttunut, ainakaan käsitys avoimista verkoista. Suhtaudun ehkä hieman varovaisemmin antamieni tietojen osalta, mutta muuten olen edelleen samoilla linjoilla.

---  

## Lähteet 
1. Wifi Challenge Lab. Valitut tehtävät: Luettavissa: https://lab.wifichallenge.com/challenges. Luettu 26.11.2026
2. Wifi Challenge Lab. README.txt. Luettu 26.11.2026
3. Iso-Anttila L. Wifi. Verkkoon tunkeutuminen ja tiedustelu -opintojakson luentomateriaali moodlessa. Haaga-Helia ammattikorkeakoulu. Luettu 26.11.2026
4. Claude Sonnet 5.5 tekoälymalli
