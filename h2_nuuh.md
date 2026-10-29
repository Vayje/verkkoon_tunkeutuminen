# H2 Nuuh

## x) Lue ja tiivistä

### Wireshark - Getting started (Karvinen 2025) 

Tero Karvisen kirjoittamassa artikkelissa kerrotaan mikä on Wireshark, mitä sillä voi tehdä ja miten se asennetaan omaan Linux työympäristöön.  
Artikkelissa on yksityiskohtaiset ohjaat asennukseen, ja mahdollisten ongelmatilanteiden ratkomiseen, kuten väärään kellon aikaan systeemissä. Myös käyttäjän lisääminen "wireshark" ryhmään on lisätty, silä ilman sitä 
ei wiresharkissa pitkälle pötkitä.  
Karvinen on myös avannut aluksi hieman monimutkaiselta vaikuttavaa wiresharkin UI:ta, mikä on todella kiva! Myös kaapatun verkkoliikenteen analysointiin on annettu vinkkejä, mitä aijon 
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

Tehtävänantona on asentaa Wireshark, ja siepata sillä liikennettä (omaa). Tämä onkin jo aikalailla tehtynä kahdessa aikaisemmassa osiossa. Wireshark löytyy natiivisti Kali Linuxista, ja pingi liikennettä tuli juuri siepattua 
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

Syy miksi paketti on alunperin lähtenyt, eli selvittämään annetun verkkosivun ip-osoitetta DNS-palvelusta.    

<img width="1229" height="460" alt="Näyttökuva 2026-10-29 150351" src="https://github.com/user-attachments/assets/c258ef24-921e-442a-936f-2759bef0a2c0" />  

---  
















## Lähteet 

1. https://terokarvinen.com/wireshark-getting-started/
2. https://terokarvinen.com/network-interface-linux/
3. https://www.hakatemia.fi/courses/tietoverkot/tietoverkot-ja-tcp-ip-malli
