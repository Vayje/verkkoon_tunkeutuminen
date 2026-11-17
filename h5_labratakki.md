# H5 Labratakki

## a) ARP-hyökkäys
Tehtävässä on tarkoitus ajaa luennolla esitetty ARP-hyökkäys, ja tutkia miten se toimii. 

Ensiksi käynnistin mininetin VMwarella, ja suoritin siellä komennot "ryu-manager ryu.app.simple_switch_13", joka ajetaan mininetin terminaalissa, sekä "sudo mn --topo single,3 --mac --swtich ovsk -- controller remote", joka ajetaan windows powershellissä ssh-yhteyden läpi. Ne käymnistävät mininetin verkko-ohjaimen. 
Ssh-yhteyden otin Windowsin powershellillä komennolla "ssh mininet@ip-osoite". IP-osoitteen sain selville mininetin sisällä syöttämällä komennon "ip a".  

<img width="1919" height="1029" alt="image" src="https://github.com/user-attachments/assets/a356bf0b-f1b3-41bb-91a2-ca4903332604" />  

---  

<img width="682" height="305" alt="image" src="https://github.com/user-attachments/assets/213f5d43-3514-4bf3-8bfc-fe2eac306881" />  

---  

Sen jälkeen pääsin mininetin käyttöliittymään, ja suoritin komennon "pingall". Komennon sai ajettua onnistuneesti, ja kaikki pingit vastasivat kutsuun.  

<img width="331" height="192" alt="image" src="https://github.com/user-attachments/assets/c0b970f1-c91e-4644-996d-34a6a2aea37b" />  

---  

Mininetissä avasin node1 komennolla "xterm h1" (olettaisin että yksi virtuaalikoneista mininetin sisällä), ja suoritin komennon arp-n jolla voimme tarkastella nykyistä ARP-taulukkoa.  

<img width="958" height="723" alt="image" src="https://github.com/user-attachments/assets/9a40dc7f-b065-4016-a9d3-54177edc9731" />  

---  

Seuraavaksi suoritettiin liuta komentoja, jolla avasimme node3 sekä wiresharkin jotta pääsemme kuuntelemmaan liikennettä node kolmosen verkosta.  

<img width="819" height="145" alt="image" src="https://github.com/user-attachments/assets/5f13da75-b800-44c5-b34d-f47d9203b00a" />  

<img width="990" height="797" alt="image" src="https://github.com/user-attachments/assets/3c1f0747-c490-4d4e-bf48-cbcf87527b5b" />  

---  

Suoritettuani Larin antamat komennot, jotka lisään tämän osion loppuun kokonaisuudessaan sain ARP-myrkytyksen onnistumaan. Tässä myrkytyksessä ensin tarkistettiin ARP-taulukko node1 ikkunasssa, jossa näkyi "oikeat" MAC-osoitteet IP-osotteiden perässä. Sen jälkeen node3 suoritettiin routing komentoja ja ajettiin python scripti, joka käänsi h2 noden liikenteen suoraan node3. Eli node3:sta tuli "man in the middle".  

<img width="560" height="482" alt="image" src="https://github.com/user-attachments/assets/3abece5c-4670-4b09-b34f-46766679bd0b" />  

<img width="835" height="428" alt="image" src="https://github.com/user-attachments/assets/0b4e978a-0307-4d7c-81a9-364edabfe262" />  

<img width="1587" height="882" alt="image" src="https://github.com/user-attachments/assets/993552a2-7cc4-4b71-a676-3212530acfff" />  


---  

## b) ICMP Spoofing & TCP Session Hijacking 

Samassa opettajan tarjoamassa mininet virtuaaliympäristössä on myös ICMP Spoofing sekä TCP Session Hijacking labrat. Ne olisi tarkoitus ajaa läpi, ja kertoa miten tekniikat toimivat.  

Koska tarjotuissa materiaaleissa oli jo ensiohjeissa hieman epäselvyyksiä, luotin tässä vaiheessa Claude Sonnet 5 tekoälymalliin, joka neuvoi ensin katsomaan cat komennolla kaikki ICMP ja TCP hyökkäyksiin liittyvät skriptit. 
Kun Claude sai kiinni skriptien toiminnan, avasin taas mininetillä kolme erillistä ikkunaaa jokaiselle nodelle. 

Sen jälkeen käynnistin node3 ikkunassa "python3 spoof_icmp.py" skriptin, joka kuuntelee kaikkea ICMP-liikennettä koko verkkosegmentissä. En kuitenkaan saanut skriptiä mitenkään päin toimimaan edes tekoälyn avustuksella, joten nyt tyydyn vain selittämään mitä harjoituksessa olisi tarkoitus tapahtua. 

ICMP-labrassa olisi tarkoitus vastata node1 terminaalin pingeihin, jossa yritetään pingata node2. Node3 terminaalissa ajetun skriptin avulla se lähettäisi "väärennettyjä" vastauksia node1 pingeihin, ja teeskentelisi olevan node2.  

TCP-Session Hijackingissa jälleen tekoälyn avulla availin oikeat skriptit oikeisiin nodeihin, jonka jälkeen pääsin kuuntelemaan node3:lla TCP-liikenettä node1 ja node2 laitteista.  

<img width="963" height="515" alt="image" src="https://github.com/user-attachments/assets/78980145-b155-4c96-9c1b-1feb6e4b9993" />  

---  

Skriptin idea on se, että node3 kaappaa paketin matkalta, ja lähettää siitä väärennetyn version takaisin node1.  

## c) SDN-DDos

Opettajan tarjoamassa materiaalissa on labraskripti, jonka tarkoituksena on lähettää palvelimelle useita SYN-paketteja, sen jälkeen vastaanottaa SYN-ACK-paketti palvelimelta, johon olisi tarkoitus vastata ACK-paketilla. Skripti ei kuitenkaan viimeistä vastausta anna taikka viimeistele kättelyä, jolloin palvelin tukkeutuu keskeneräisistä yhteyedenottopyynnöistä. Hyökkäykset tapahtuvat 10 sekunnin intervalleissa, ja kokonaiskesto on 100 sekuntia.  

<img width="906" height="177" alt="image" src="https://github.com/user-attachments/assets/fb61e36c-fc5f-4ceb-8c2f-271930a6a07a" />  

## Lähteet
1. Claude Sonnet 5 -tekoälymalli
2. Iso-Anttila L. ARP-myrkytys. Verkkoon tunkeutumuinen ja tiedustelu -opintojakson esitysmateriaali Moodlessa. Luettu 17.11.2026
3. Iso-Anttila L. 17.11.2026. Lehtori. Verkkoon tunkeutuminen ja tiedustelu. Luento. Helsinki. 
