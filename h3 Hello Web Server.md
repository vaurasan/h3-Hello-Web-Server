# h3 Hello Web Server

#### Oma Host kokoonpanoni:

| Komponentti | Kuvaus | Lisätiedot |
| :---        |    :----:   |          ---: |
| Emolevy | MSI B550-A PRO | ATX, AM4 |
| Prosessori   | AMD Ryzen 9 5900X | 12-Core Processor 3.70 GHz |
| RAM   | G.Skill  Ripjaws V |  32GB (4x8GB) DDR4 3600MHz, CL 16, 1.3  |
| Näytönohjain   | Sapphire PULSE AMD Radeon RX 7900 GRE        | 16GB     |
| Kovalevy   | Kingston 1TB        | A2000 NVMe PCIe SSD M.2      |
| Kovalevy   | Crucial 512GB        | MX100 SSD     |
| Kovalevy   | Crucial 256GB        | MX100 SSD     |
| Virtalähde   | Asus 750W TUF Gaming Gold        | ATX 80 Plus Gold      |
| Kotelo   | Phanteks Enthoo        | Pro Full Tower      |

Käyttöjärjestelmä: Windows 11 Pro 23H2 22631.4037 Windows Feature Experience Pack 1000.22700.1027.0

## x) Lue ja tiivistä

The Apache Software Foundation 2023: Apache HTTP Server Version 2.4 Documentation: [Name-based Virtual Host Support](https://httpd.apache.org/docs/2.4/vhosts/name-based.html)<br>
- Name-based virtual hostingissa palvelin luottaa asiakkaan ilmoittamaan isäntänimeen HTTP-otsikoissa
- Name-based virtual hostingissa moni isäntä voi käyttää samaa IP-osoitetta
- Name-based virtual hosting usein yksinkertaisempaa, kuin IP-based Virtual Host
- Tulisi aina käyttää Name-based virtual hostingia, jos suinkin laitteisto ei muuta vaadi
- Historialliset syyt eivät enää päde IP-based Virtual hostingiin<br>

[Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address](https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/)<br>
Tätä on vaikea tiivistää, kun on kohtuullisen tiivis sivu jo valmiiksi:
- Yleensä käytössä on yksi IP-osoite ja monta web-sivua hostattavana, Apachen kanssa voi pitää useita verkkotunnuksia yhdellä IP-osoitteella
- Asennetaan apache2 ja konfiguroidaan web palvelin
> $ sudo apt-get -y install apache2<br>
> $ echo "Default"|sudo tee /var/www/html/index.html
- Lisätään uusi Name Based Virtual Host
> $ sudoedit /etc/apache2/sites-available/pyora.example.com.conf<br>
> $ cat /etc/apache2/sites-available/pyora.example.com.conf<br>
> $ sudo a2ensite pyora.example.com
- Uudelleenkäynnistetään eli restartataan apache2
> $ sudo systemctl restart apache2
- Luodaan web sivu normaalikäyttäjänä
> $ mkdir -p /home/xubuntu/publicsites/pyora.example.com/<br>
> $ echo pyora > /home/xubuntu/publicsites/pyora.example.com/index.html
- Viimeisenä koeajo
> $ curl -H 'Host: pyora.example.com' localhost<br>
> $ curl localhost

## a) Testaa, että weppipalvelimesi vastaa localhost-osoitteesta. Asenna Apache-weppipalvelin, jos se ei ole jo asennettuna.

*Aloitus 5.9.2024 klo 12:55*<br><br>
Apache2 ehdin asentaa eilen tunnilla<br>
> sudo apt-get install apache2<br>

Laitan seuraavan komennon, jotta apache2 käynnistyy, kun käyttöjärjestelmä käynnistyy:<br>
> sudo systemctl enable --now apache2<br>

![apache2now](https://github.com/user-attachments/assets/7703ac20-b2c8-47aa-a658-11d452ff7ea8)<br>

Seuraavaksi kokeillaan localhostia komentokehotteessa:
> curl http://localhost<br>

Ruudulle tulee html:ää, eli localhost toimii, kokeillaan vielä Firefoxilla<br>

![toimii](https://github.com/user-attachments/assets/7cde5b4d-7fed-4d67-98c6-28c96543fa47)<br>

Näkyy toimivan sekin:<br>

![toimii2](https://github.com/user-attachments/assets/1e2a1ded-50bd-4f34-9aaf-fbc29133b736)<br>

*Klo 13:04 valmis, aikaa kului 9min*

## b) Etsi lokista rivit, jotka syntyvät, kun lataat omalta palvelimeltasi yhden sivun. Analysoi rivit (eli selitä yksityiskohtaisesti jokainen kohta ja numero, etsi tarvittaessa lähteitä).

*Aloitus 5.9.2024 klo 13:10*<br><br>
Yritetään etsiä lokista tietoja: "sudo journalctl" ei näytä ainakaan vielä tietoja.<br>

![imagejournalctl](https://github.com/user-attachments/assets/d35b2147-6a58-4ddd-82f8-39b086968224)<br>

Kokeillaan hakea lokit toista kautta:<br>

> sudo tail /var/log/apache2/access.log<br>

![imagevarlog](https://github.com/user-attachments/assets/575d8183-423d-4166-8aac-9561810126bb)
<br>
Lokien analysointi onkin hieman hankalampi asia, yritän lonkalta ensin ja sitten haen tietoa internetistä.<br><br>
Vasemmalla näkyy 127.0.0.1, tämä on localhost IP-osoite.<br>
Päivämäärän jälkeen lukee GET, en tiedä mitä tässä tarkoittanee, paitsi jonkun asian hakemista.<br>
Sen jälkeen /icons/openlogo-75.png, selkeästi joku kuvatiedosto logosta.<br>
"Mozilla/5.0", olisikohan Firefoxin versio, tai tuo "Firefox/115.0".<br>

Arvailut sikseen, aika etsiä tietoa. Kohtalaiset ohjeet löysin täältä: https://www.sumologic.com/blog/apache-access-log/.<br>
Lisäksi käytin myös tätä: https://developer.mozilla.org/en-US/docs/Web/HTTP/Status.<br>
Sekä: https://disjoint.ca/til/2016/05/17/how-to-determine-the-file-size-of-a-remote-http-object/.<br>

Eli ensimmäisenä **127.0.0.1** IP osoite, josta pyyntö tuli.<br>
Tässä välissä itsestäänselvyytenä päivämäärä, sekä aika ja aikavyöhyke.<br>
**GET / HTTP/1.1** pyynnön tyyppi, sekä "GET" ja "HTTP1.1" välissä se mitä pyydetään.<br>
**200** HTTP vastauksen status koodi. "200" viittaa onnistuneeseen pyyntöön. Kyseessä on "GET" tyyppinen pyyntö, jolloin 200 tarkoittaa, että resurssi on onnistuneesti haettu ja toimitettu.<br>
**6040** asiakkaalle palautetun objektin koko byteinä, eli tavuina, tässä tapauksessa 6040 tavua, eli 6040 bytes.<br>
**http://localhost** viittaa osoitteeseen, josta pyyntö on tullut.<br>
**Mozilla/5.0 (X11; Linux x86_64; rv:109.0) Gecko/20100101 Firefox/115.0** on ympäristö, josta käyttäjä käyttää kyseistä palvelua. Tässä tapauksessa Mozilla Firefox Linuxilla.<br>

*Valmis klo 13:49, aikaa kului 39min*

##

##

##

##

## Lähteet

Fizpatrick, S. 2020. Understanding the Apache Access Log: View, Locate and Analyze. Luettavissa: https://www.sumologic.com/blog/apache-access-log/. Luettu 5.9.2024<br>
Karvinen, T. 2018. Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address. Luettavissa: https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/. Luettu 5.9.2024<br>
The Apache Foundation. 2024. Name-based Virtual Host Support. Luettavissa: https://httpd.apache.org/docs/2.4/vhosts/name-based.html. Luettu 5.9.2024



<br><br>

---

Tätä dokumenttia saa kopioida ja muokata GNU General Public License (versio 2 tai uudempi) mukaisesti. http://www.gnu.org/licenses/gpl.html<br>
Pohjana Tero Karvinen 2012: Linux kurssi, http://terokarvinen.com<br><br>
Kirjoittanut <em>Santeri Vauramo</em>, 2024
