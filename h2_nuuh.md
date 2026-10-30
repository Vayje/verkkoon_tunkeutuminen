# H2 Nuuh

## x) Lue ja tiivistä

### Wireshark - Getting started (Karvinen 2025) 

Tero Karvisen kirjoittamassa artikkelissa kerrotaan mikä on Wireshark, mitä sillä voi tehdä ja miten se asennetaan omaan Linux työympäristöön.  
Artikkelissa on yksityiskohtaiset ohjeet asennukseen, ja mahdollisten ongelmatilanteiden ratkomiseen, kuten väärään kellon aikaan systeemissä. Myös käyttäjän lisääminen "wireshark" ryhmään on lisätty, sillä ilman sitä 
ei wiresharkissa pitkälle pötkitä.  
Karvinen on myös avannut aluksi hieman monimutkaiselta vaikuttavaa wiresharkin UI:ta, mikä on todella kiva! Myös kaapatun verkkoliikenteen analysointiin on annettu vinkkejä, joita aion 
ehdottomasti referoida tulevaisuudessa.  

---  

### Network Interface Names on Linux (Karvinen 2025) 

Karvisen kirjoittamassa artikkelissa avataan verkkoyhteyskäyttöliittymien lyhenteitä ja termejä, joita linux terminaalissa tulee vastaan. Esimerkiksi "en" alkuinen prefix on langallinen ethernet verkko, "lo" on loopback osoite/verkko 
ja "wl" on langaton verkko. Karvinen on antanut artikkeliin vinkiksi myös komennot "ip a" ja "ip route" jotka kertovat käyttäjän nykyiset verkkoyhteydet.  

---  

## a) Linux 

Tehtävässä asennetaan Debian tai Kali linux virtuaalikoneeseen, ja raportoidaan mahdolliset ongelmatilanteet ja niiden ratkaisut. 
Molemmat löytyvät jo omalta virtuaalikoneeltani, ja näissä tehtävissä käytän ympäristönäni Kali Linuxia. Molempien asennus onnistui taannoin täysin ongelmitta, jos käytettävän RAM-muistin määrän lisäämistä Kaliin ei lasketa.  

Lisättäköön bonuksena vielä CachyOS distron asennus, jonka kotitietokoneelleni asensin dualboottina. Sekin meni Linuxin puolesta kivuttomasti, mutta levyaseman tilan allokointi Windowsin puolella meinasi olla ärsyttävää (yllätys).  

---  

## b) Ei voi kalastaa 

Tehtävässä on tarkoitus osoittaa että pystyy katkaisemaan oman virtuaalikoneen verkkoyhteyden, ja palauttamaan sen takaisin. 
Aloitin tehtäväni pingaamalla terminaalin kautta osoitetta 1.1.1.1, ja nappasin liikennettä wiresharkissa. Tässä normaali verkkoliikenne ennen yhteyden katkaisua.  

<img width="1620" height="317" alt="image" src="https://github.com/user-attachments/assets/7f21f309-0cef-4f77-8555-d722a44fd24d" />  

---  

Nettiyhteyden katkaisin VirtualBoxin omista asetuksista. Asetuksiin pääsee painamalla oikea ctrl + home näppäimiä, ja navigoimalla Devices -> Network -> Network Settings, ja raksi pois "Cable Connected" ruudusta. 
Tämän jälkeen kokeilin pingata osoitetta 1.1.1.1 uudestaan. Wiresharkkia ei tässä vaiheessa tarvitse edes avata.  

<img width="384" height="196" alt="image" src="https://github.com/user-attachments/assets/e29be7c7-9d59-482b-90cd-cd08c20f9a0b" />  

---  

Palauttaakseni yhteyden navigoin takaisin virtualboxin verkkoasetuksiin, laitoin ruksin takaisin ruutuun ja pingasin osoitetta 1.1.1.1 todistaakseni yhteyden palanneen.  

<img width="603" height="359" alt="image" src="https://github.com/user-attachments/assets/a512d230-f9f3-4593-b7aa-a9eab9979663" />  

<img width="1637" height="736" alt="image" src="https://github.com/user-attachments/assets/5055926d-b385-4be3-a39c-4bebcf107a85" />  

---  

## c) Wireshark 

Tehtävänantona on asentaa Wireshark, ja siepata sillä liikennettä (omaa). Tämä onkin jo aika lailla tehtynä kahdessa aikaisemmassa osiossa. Wireshark löytyy natiivisti Kali Linuxista, ja pingi liikennettä tuli juuri siepattua 
todistaakseni verkkoyhteyden toimivuuden.  

---  

## d) Oikeesti TCP/IP 

Tarkoituksena on kaapata paketti ja osoittaa siitä TCP/IP mallin neljä ensimmäistä kerrosta. 
TCP/IP mallin neljä ensimmäistä kerrosta ovat peruskerros (link layer), verkkokerros (network layer), kuljetuskerros (transport layer) ja sovelluskerros (application layer). (Hakatemia 2026) 
Otin tähän osioon esimerkiksi paketin, joka syntyy kun avaan googlen. Kaappasin myös tällä kertaa liikennettä "eth0" valinnalla, enkä "any" valinnalla.  

Link layer = Ethernet II (laatikoitu sinisellä) 

Tämä kuljettaa paketin nykyisestä pisteestä seuraavaan, joka on tässä tapauksessa virtualboxin oma reititin.   

<img width="1202" height="817" alt="Näyttökuva 2026-10-29 150944" src="https://github.com/user-attachments/assets/f3eb07dc-6744-4143-8b57-b05d51d0c5fa" />  

---  
Network layer = Internet protokollan versio, eli IPv4 (laatikoitu sinisellä) 

Merkkaa pakettiin ip-lähtöosoitteen ja ip-kohdeosoitteen. Pitää huolen että paketti löytää perille.  

<img width="1219" height="705" alt="Näyttökuva 2026-10-29 150829" src="https://github.com/user-attachments/assets/f43b031a-2253-43aa-bb6d-230d7907917a" />  

---  
Transport layer = UDP protokolla (laatikoitu sinisellä) 

Ohjaa paketin oikeaan porttiin/sovellukseen. Tässä tapauksessa DNS kyselyyn eli porttiin 53.   

<img width="1228" height="823" alt="Näyttökuva 2026-10-29 150624" src="https://github.com/user-attachments/assets/d173f772-b771-4f3e-9e62-ed4ae236880e" />  

---  
Application layer = DNS kysely (laatikoitu sinisellä) 

Syy miksi paketti on alun perin lähtenyt, eli selvittämään annetun verkkosivun ip-osoitetta DNS-palvelusta.    

<img width="1229" height="460" alt="Näyttökuva 2026-10-29 150351" src="https://github.com/user-attachments/assets/c258ef24-921e-442a-936f-2759bef0a2c0" />  

---  

## e) Mitäs tuli surffattua?

Karvinen tarjoaa tehtävään .pcap tiedoston, jossa on jotain verkkoliikennettä ja sitä tulisi analysoida. 
Avattuani tiedoston wiresharkilla, tuli näkyviin heti DNS kyselyitä google.comista. Tämän jälkeen vastaavia kyselyitä tuli osoitteesta terokarvinen.com.  

<img width="1347" height="443" alt="image" src="https://github.com/user-attachments/assets/c4368b43-e9fb-4065-b73b-33eaa2d6bde7" />  

---  

Alempana on runsaansti TCP-paketteja, ja joitain viittauksia terokarvinen.com verkkosivuun. Voisi päätellä, että kaapatussa liikenteessä on surffailtu Tero Karvisen kotisivuilla. 
I/O graphista voimme nähdä, että kaapattu liikenne on kestänyt vain seitsemän sekuntia, ja suurin paketti "piikki" on tullut heti alussa kun on otettu yhteys googleen.  

<img width="857" height="698" alt="image" src="https://github.com/user-attachments/assets/2e2c8ce9-d0c3-4359-96e0-4dc16261294e" />  

---  

Conversations ikkunasta nähdään, että hirveän moni laite ei ehtinyt mukaan tähän kaappaukseen. Kai sekin on suhteellista, laitteita voisi olla huomattavasti enemmän näinkin lyhyessä kaappauksessa, jos oltaisiin otettu yhteyttä muutamaankiin eri verkkosivuun.  

<img width="1611" height="633" alt="image" src="https://github.com/user-attachments/assets/469b61f6-cb21-4f64-ae8e-8b4c5da8bbbf" />  

---  

## f) Mitä selainta käyttäjä käyttää?

Jälleen tehtävään on tarjoiltu Karvisen toimesta .pcap tiedosto, josta tulisi wiresharkissa päätellä mitä selainta on kaappauksessa käytetty. 
Tutkin ensiksi listalta löytyviä TCP, UDP ja DNS osioita, mutta niistä ei selainta paljastunut. Aiemmin käytetyt "endpoint" ja "I/O graph" välilehdet eivät myöskään paljastaneet mitään hyödyllistä tehtävään liittyen.  

Selain todennäköisesti paljastuisi, jos liikenne olisi salaamatonta HTTP liikennettä (serverfault 2014). Mutta nyt se ei ole, joten kokeilin filtteriä "tls.handshake.type == 1", jota serverfaultin foorumeilla suositeltiin (serverfault 2024).  

<img width="1264" height="765" alt="image" src="https://github.com/user-attachments/assets/8290cbfc-0723-4a2a-9b1d-6ecadfafa075" />  

---  

Aikani selailtua pakettejani ja tultuani umpikujaan päätin kysyä tekoälyltä (Claude, Sonnet 5.5), mihin kannattaisi kiinnittää huomiota tässä vaiheessa. 
Vastaukseksi sain: 
- Cipher suites, lista ja sen järjestys
- Extensions, mitä laajennuksia on mukana ja missä järjestyksessä
- GREASE arvot, tietyt selaimet lisäävät näitä arvoja ja se rajaa vaihtoehtoja pois
- Supported groups, ja onko mukana post-quantum ryhmiä. Nämä ovat selaimen tukemia salausmenetelmiä, jolla se voi ottaa palvelimeen yhteyttä. 
- Server name

Pistin claudelle kuvan cipher suites listasta. GREASE arvoja ei löytynyt, mutta cipher suites listalta kiinnitin huomioni kolmeen ensimmäiseen osioon. 0x1301, 0x1303 ja 0x1302, sillä nämä ovat selaimen preferoituja salausmenetelmiä.  

<img width="919" height="395" alt="image" src="https://github.com/user-attachments/assets/6b0505fe-59c7-4abe-befe-1dd5efb43d4d" />  

---  

Seuraavaksi lähdin googlettamaan selaimia, ja niiden cipher suite orderia. Googlasin ensimmäiseksi chromen, firefoxin ja edgen. Chromen tulokset olivat runsaat mutta ei kauhean hedelmälliset. Mozillan cipher suite orderia tutkiessa tuli jo osittain yhtäläisyyksiä, joten seuraavaksi kokeilin seurata omaa liikennettä mozillasta, ja verrata client hello pakettia Karvisen tarjoaman materiaalin pakettiin. Verrattuani cipher suites osiota client hello paketista, tulin siihen tulokseen että ne ovat lähes identtiset. Pieni ero voi johtua eri mozillan versioista, mutta melko varmasti voisin väittää, että tehtävässä on käytetty firefoxia. Vertailin myös muita aikaisemmin listaamiani kohtia keskenään, ja yhtäläisyyksiä löytyi. Omasta liikenteestäni ei myöskään löytynyt GREASE arvoja, jotka ovat chromen lisäämiä hilavitkuttimia.  

Tässä vielä vertailun vuoksi molempien "client hello" pakettien cipher suites. Tehtävän cipherit ovat ensimmäisenä, ja omani alempana.  

<img width="1625" height="807" alt="image" src="https://github.com/user-attachments/assets/ff3060a8-eded-431f-a110-3a145d55f73c" />

<img width="1852" height="758" alt="image" src="https://github.com/user-attachments/assets/88e5dda6-ce67-4b0d-93cd-bb62222651a9" />  

---  

## g) Minkä merkkinen verkkokortti käyttäjällä on? 

Seuraavaksi tarkoituksenani oli löytää Karvisen tarjoamasta .pcap filusta verkkokortti, jota materiaalissa on käytetty. Nenäni osoitti ensimmäiseksi MAC-osoitteen suuntaan, joka löytyy Ethernet II osiosta source address välilehdeltä. Osoitteen ensimmäinen puolisko (vasemmalta oikealle) on yleensä laitevalmistajan tarjoama numerosarja, jonka voisin googlettaa. Tässä vaiheessa on tärkeää huomioida, että valitsee paketin niin että source address on tutkittavan koneen IP-osoite, eikä takaisin tulevan paketin osoite.  

<img width="1106" height="803" alt="image" src="https://github.com/user-attachments/assets/ebc3dfb0-4dd1-4366-bd53-01499d5388b6" />  

---  

Seuraavaksi menin "Mac address lookup" palveluun, ja kirjoitin yllä näkyvän source osoitteen sinne.  

<img width="958" height="517" alt="image" src="https://github.com/user-attachments/assets/12f3588f-d731-4232-ad1e-0f89c6274896" />  

---  

Koska mitään ei löytynyt, tulin siihen tulokseen että kyseessä on VirtualBoxin verkkokortti.  

## h) Millä weppipalvelimilla käyttäjä on surffaillut? 

Vaikka liikenteen sisältö on salattua, näemme silti että materiaalissa on ensin otettu yhteys google.comiin, ja sen jälkeen terokarvinen.comiin. Surffailu on sen jälkeen jatkunut terokarvinen.comissa, ainakin päätellen ip-osoitteista. Conversations ikkunassa näemme että terokarvinen.comin IPv4-osoite sai eniten liikennettä.  

<img width="1873" height="472" alt="image" src="https://github.com/user-attachments/assets/199c853e-c585-4abc-a748-5a19a0b39a68" />  

<img width="1886" height="610" alt="image" src="https://github.com/user-attachments/assets/b92df99d-e164-419d-a15e-eab812e629a2" />  

---  

## i) Analyysi

Tehtävänantona on kaapata pieni määrä omaa liikennettä ja analysoida sitä. 
Aloitin liikenteen kaappaamisen kun mozilla oli jo auki. Sen jälkeen avasin googlen, hain hakukoneella "yle" ja avasin yle.fi verkkosivun. Lyhyessä 10 sekuntin ajassa saimme jo 1351 pakettia analysoitavaksi.  

<img width="1878" height="814" alt="image" src="https://github.com/user-attachments/assets/837fb155-8139-4ea6-9ef1-339378b006ed" />  

---  

Liikenne alkaa DNS kyselyllä, kun halutaan selvittää google.comin IP-osoite. Sen jälkeen siirrytään kättelyyn, ja kättelyn vastaus vaiheeseen, jossa tullaan yksimielisyyteen yhteydestä (ACK ja SYN, ACK).  

<img width="1321" height="338" alt="Näyttökuva 2026-10-30 131517" src="https://github.com/user-attachments/assets/dfa4b90c-e843-4e21-9004-4eb5f6d05a29" />  

---  

TCP-streamia seuraamalla huomaamme että liikenne on salattua.  

<img width="1246" height="854" alt="Näyttökuva 2026-10-30 131742" src="https://github.com/user-attachments/assets/fb4b9529-a179-4ad6-a6ce-e243eee751cc" />  

---  

Pakettilistan lopussa on vielä FIN ACK kättelyt, jossa yhteys katkaistaan.  

<img width="1637" height="817" alt="Näyttökuva 2026-10-30 131932" src="https://github.com/user-attachments/assets/76645ed9-018a-4017-b042-4e3774ce2228" />  

---  

Koen että liikenteessä ei tällä hetkellä ole enempää analysoitavia kohtia, joita ei olisi jo aikaisemmin käyty. Pyrin välttelemään toistuvaa tekstiä raportissa, ja koen että tehtävät olivat jo hieman itseääntoistavia tähän asti.  


## Lähteet 

1. https://terokarvinen.com/wireshark-getting-started/
2. https://terokarvinen.com/network-interface-linux/
3. https://www.hakatemia.fi/courses/tietoverkot/tietoverkot-ja-tcp-ip-malli
4. https://serverfault.com/questions/646063/user-agent-in-http-or-https-request
5. https://serverfault.com/questions/1167413/finding-out-websites-visited-via-https
6. https://docs.tlsref.org/cipher-suites.html
7. https://maclookup.app/
