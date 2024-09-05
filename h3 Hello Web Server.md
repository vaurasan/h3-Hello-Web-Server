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

Aloitus 5.9.2024 klo 12:55
Apache2 ehdin asentaa eilen tunnilla<br>
> sudo apt-get install apache2<br>





## Otsikko


## Lähteet

Karvinen, T. 2018. Name Based Virtual Hosts on Apache – Multiple Websites to Single IP Address. Luettavissa: https://terokarvinen.com/2018/04/10/name-based-virtual-hosts-on-apache-multiple-websites-to-single-ip-address/. Luettu 5.9.2024<br>
The Apache Foundation. 2024. Name-based Virtual Host Support. Luettavissa: https://httpd.apache.org/docs/2.4/vhosts/name-based.html. Luettu 5.9.2024



<br><br>

---

Tätä dokumenttia saa kopioida ja muokata GNU General Public License (versio 2 tai uudempi) mukaisesti. http://www.gnu.org/licenses/gpl.html<br>
Pohjana Tero Karvinen 2012: Linux kurssi, http://terokarvinen.com<br><br>
Kirjoittanut <em>Santeri Vauramo</em>, 2024
