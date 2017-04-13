<properties
    pageTitle="Apache Tomcat Linux VM häälestamine | Microsoft Azure'i"
    description="Siit saate teada, kuidas häälestada Apache Tomcat7 abil Azure'i virtuaalarvuti (VM) töötab Linux."
    services="virtual-machines-linux"
    documentationCenter=""
    authors="NingKuang"
    manager="timlt"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="12/15/2015"
    ms.author="ningk"/>

#<a name="how-to-set-up-tomcat7-on-a-linux-virtual-machine-with-microsoft-azure"></a>Kuidas häälestada Tomcat7 Linux Virtual arvutisse Microsoft Azure

Apache Tomcat (või lihtsalt Tomcat, varem ka Jakarta Tomcat) on avatud allika veebiserver ja servlet container väljatöötatud Apache tarkvara Foundation (ASF). Tomcat rakendab Java Servlet ja p Microsystems kaudu JavaServer lehtede (JSP) kirjeldused ning tuuakse puhas Java HTTP web serveri keskkonnas, kus Java koodi käivitada. Lihtsaim konfiguratsiooni, Tomcat töötab üks operatsioonisüsteem protsess. See protsess töötab Java virtuaalse masina (töötab). Iga HTTP taotluse brauseri kaudu Tomcat töödeldakse eraldi protsessi Tomcat käigus.  

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Sellest juhendist tomcat7 installida Linux pilt ja rakendada see Microsoft Azure'i.  

Saate:  

-   Kuidas luua virtuaalse masina Azure.
-   Kuidas ette valmistada virtuaalse masina tomcat7.
-   Kuidas installida tomcat7.

Eeldatakse, et lugeja on juba Azure tellimuse.  Muul juhul saate registreeruda tasuta prooviversiooni [http://azure.microsoft.com](https://azure.microsoft.com/)juures. Kui teil on MSDN-i tellimus, lugege teemat [Microsoft Azure'i teisiti hinnad: MSDN-i, MPN ja Bizspark eeliste](https://azure.microsoft.com/pricing/member-offers/msdn-benefits/?c=14-39). Azure'i kohta leiate lisateavet teemast [mis on Azure?](https://azure.microsoft.com/overview/what-is-azure/).

Selles teemas eeldab, et teil on lihtne tundmine tomcat ja Linux.  

##<a name="phase-1-create-an-image"></a>Etapp 1: Luua pilt
Selles etapis loote Linux pildi kasutamine Azure virtuaalse masina.  

###<a name="step-1-generate-an-ssh-authentication-key"></a>Samm 1: Luua mõne SSH autentimise võti
SSH on oluline vahend süsteemi administraatorid. Kuid konfigureerimine Accessi turbes põhjal inimeste määratud parool ei ole parimaid tavasid. Pahatahtlik kasutaja saab oma kasutajanime ja parooliga nõrk süsteemi katkestada.

Häid uudiseid, et on viis Kaugpöördusteenuse avatuks ja ei peaks muretsema paroolid. Meetod koosneb asümmeetriline krüptograafia autentimist. Kasutaja privaatvõti on üks, mis määrab autentimine. Saate isegi kasutajakonto keelamiseks Paroolautentimine täielikult lukustada.

Selle meetodi eelis on peate erinevad paroolid serveritest sisse logima. Saate isikliku privaatvõti abil kõik serverid, mis takistab meeles mitu paroolide autentida.

Samuti on võimalik sisse logida selle meetodi paroolkaitsega.

Järgmiste juhiste abil luua SSH autentimise võti.

1.  Laadige alla ja installige puttygen järgmises asukohas: [http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html)
2.  Käivita PUTTYGEN. EXE.
3.  Klõpsake nuppu **Genereeri** luua võtmed. Protsessi saate suurendada juhuslikkust viies kursori akna tühja ala.  
![][1]
4.  Pärast Genereeri Puttygen.exe kuvatakse teie loodud võti. Näiteks:  
![][2]
5.  Valige ja kopeerige avalik võti **võti** ja salvestage see fail nimega publicKey.pem. Ärge klõpsake **Salvesta avalik võti**, kuna avalik võti salvestatud failivormingus erineb soovime avalik võti.
6.  Klõpsake nuppu **Salvesta privaatvõti** ja salvestage see fail nimega privateKey.ppk.

###<a name="step-2-create-the-image-in-the-azure-portal"></a>Samm 2: Looge pilt Azure'i portaalis.
[Azure'i portaal](https://portal.azure.com/), klõpsake nuppu **Uus** tegumiribal luua pilt, valides Linux pildi vastavalt oma vajadustele. Järgmises näites kasutatakse Ubuntu 14.04 pilt.
![][3]

Jaoks **Hostinimi** määrata URL, mida te ja Interneti-kliendid kasutavad juurdepääsuks selle virtuaalse masina nimi. DNS-i nimi, näiteks tomcatdemo Viimane osa määratlemine ja Azure loob URL-i nimega tomcatdemo.cloudapp.net.  

**SSH autentimise võti**, kopeerige võtmeväärtuse **publicKey.pem** fail, mis sisaldab loodud puttygen avalik võti.  
![][4]

Muude sätete konfigureerimine vastavalt vajadusele ja seejärel klõpsake nuppu Loo.  

##<a name="phase-2-prepare-your-virtual-machine-for-tomcat7"></a>Etapp 2: Tomcat7 virtual arvuti ettevalmistamine
Selles etapis konfigureerimine lõpp tomcat liikluse ja seejärel ühenduse, virtuaalse seadme.
###<a name="step-1-open-the-http-port-to-allow-web-access"></a>Samm 1: Avage HTTP port web juurdepääsu lubamiseks
Lõpp-punktid Azure koosnevad protokoll (TCP või UDP) koos avalike ja privaatvõ port. Privaatne port on teenus on kuulamise virtuaalse masina port. Avalik port on Azure pilveteenuses on kuulamise väliselt sissetulevate, Interneti liikluse port.  

TCP port 8080 on millised tomcat kuulab ka vaikimisi pordinumber. Avamine selle pordi Azure lõpp võimaldab teil ja teiste klientide Interneti-ühendus, et tomcat lehed.  

1.  Azure'i portaalis, klõpsake nuppu **Sirvi** -> **virtuaalse masina**ja klõpsake seejärel nuppu virtuaalse masina loodud.  
![][5]
2.  Lõpp-punkti lisamiseks oma virtuaalse masina klõpsake väljal **lõpp-punktid** .
![][6]
3.  Klõpsake nuppu **Lisa**.  
    1.  **Lõpp-punkti**, tippige lõpp-punkti lõpp-punkti nimi ja tippige **Avaliku**port 80.  

        Kui seate selle 80, ei pea kaasamine URL, mis võimaldab kasutada kõuts pordi number. Näiteks http://tomcatdemo.cloudapp.net.    

        Kui seate selle teise väärtusega, nt 81, peate pordinumber tomcat URL-i lisamine. Näiteks http://tomcatdemo.cloudapp.net:81 /.
    2.  Tippige era Port 8080. Vaikimisi kuulab tomcat TCP porti 8080. Kui olete muutnud vaikimisi kuulata kõuts pordi, peate värskendama era Port olema sama, mis on tomcat kuulata pordi.  
    ![][7]

4.  Klõpsake nuppu **OK** lõpp-punkti lisamiseks oma virtuaalse masina.



###<a name="step-2-connect-to-the-image-you-created"></a>Samm 2: Ühenduse loodud pilti.
Saate valida, mis tahes SSH tööriista ühenduse loomiseks arvuti virtuaalne. Selles näites me kasutame Putty.  

Esmalt toomine Azure portaali virtual arvuti DNS-i nimi. **Klõpsake nuppu Sirvi** -> **virtuaalmasinates** -> oma virtuaalse masina nimi -> **Atribuudid**ja seejärel vaadake paani **Atribuudid** väljal **Domeeni nimi** .  

Pordi number saamiseks SSH ühendused **SSH** välja. Siin on näide.  
![][8]

Laadige alla [siin](http://www.putty.org/) Putty.  

Pärast allalaadimist, klõpsake täitmisfail PUTTY. EXE. Hosti nimi ja põhiliste suvandite konfigureerimine ja pordi number, mis on saadud arvuti virtuaalne atribuutide. Siin on näide:  
![][9]

Klõpsake vasakpoolsel paanil nuppu **ühenduse** -> **SSH** -> **Auth** ja seejärel klõpsake käsku **Sirvi** **privateKey.ppk** fail, mis sisaldab privaatvõti asukoha määramiseks loodud puttygen etapp 1: pildi loomine. Siin on näide:  
![][10]

Klõpsake nuppu **Ava**. Teil võib teatisi teateboks. Kui teil on konfigureeritud DNS-i nimi ja pordi number õigesti, klõpsake nuppu **Jah**.
![][11]  


Peaksite nägema järgmine:  
![][12]

Sisestage kasutaja nimi, mis on määratud, kui olete loonud virtuaalse masina etapp 1: pildi loomine. Kuvatakse umbes järgmine:  
![][13]





##<a name="phase-3-install-software"></a>Etapp 3: Tarkvara installimine
Selles etapis installimist Java runtime keskkond, tomcat ja muud tomcat komponendid.  

###<a name="java-runtime-environment"></a>Java runtime keskkonnas
Tomcat on kirjutatud Java. On kahte tüüpi Java arengu komplektid (JDKs) (OpenJDK ja Oracle'i JDK) ja soovi korral saate soovitud variant.  

>AZURE'I. Märkus: Nii JDKs on peaaegu sama kood klassid Java API, kuid virtuaalse masina kood on tegelikult erinevad. Kui tegemist on teekide, OpenJDK võib kasutada avatud teekide Oracle'i võib kasutada suletud neist. Kuid Oracle'i JDK on rohkem tunnid ja mõned fikseeritud vigu ja Oracle'i JDK säilib rohkem kui OpenJDK.

Järgmised käsud erinevate JDKs alla laadida.  

Ava – jdk   

    sudo apt-get update  
    sudo apt-get install openjdk-7-jre  

Oracle'i-jdk  

-   Oracle'i veebisaidilt alla laadida soovitud JDK:  

        wget --header "Cookie: oraclelicense=accept-securebackup-cookie" http://download.oracle.com/otn-pub/java/jdk/8u5-b13/jdk-8u5-linux-x64.tar.gz  

-   Kataloog sisaldama JDK failide loomiseks tehke järgmist.  

        sudo mkdir /usr/lib/jvm  

-   Kataloogi/usr/teegi/töötab/ekstrakti JDK failid:  

        sudo tar -zxf jdk-8u5-linux-x64.tar.gz  -C /usr/lib/jvm/  

-   Vaikimisi töötab Oracle'i JDK seadmiseks tehke järgmist.  

        sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_05/bin/java 100  
        sudo update-alternatives --install /usr/bin/javac javac /usr/lib/jvm/jdk1.8.0_05/bin/javac 100  

####<a name="test"></a>Test:
Saate testida kui Java runtime keskkond on õigesti installitud järgmine käsk:  

    java -version  

Kui installisite Ava-jdk, peaksite nägema umbes selline teade:![][14]

Kui installisite Oracle'i-jdk, peaksite nägema umbes selline teade:![][15]

###<a name="tomcat7"></a>Tomcat7
Järgmise käsu abil saate installida tomcat7:  

    sudo apt-get install tomcat7  

Kui te ei kasuta tomcat7, kasutage selle käsu korral variatsioon.  

####<a name="test"></a>Test:

Märkige ruut, kui tomcat7 on installitud, siis sirvige tomcat serveri DNS-i nimi (http://tomcatexample.cloudapp.net/ on näide URL selle artikli teemad). Kui näete umbes järgmine leht, teil on installitud tomcat7 õiged.
![][16]


###<a name="install-other-tomcat-components"></a>Muude Tomcat komponentide installimine
On valikuline tomcat muud komponendid, mille saate installida.  

Käsu **sudo apt vahemälu otsing tomcat7** kuvamiseks saadaval komponendid. Järgmised käsud on mõned kasulikud osad installimiseks näited.  

    sudo apt-get install tomcat7-admin      #admin web applications
    sudo apt-get install tomcat7-user         #tools to create user instances  

##<a name="phase-4-configure-tomcat"></a>Etapp 4: Konfigureerimine Tomcat
Selles etapis saate manustada tomcat.
###<a name="start-and-stop-tomcat7"></a>Käivitamine ja peatamine tomcat7
Tomcat7 server käivitub automaatselt, kui installite. Võite ka alustada seda ise järgmine käsk:   

    sudo /etc/init.d/tomcat7 start

Tomcat7 lõpetamiseks tehke järgmist.  

    sudo /etc/init.d/tomcat7 stop

Tomcat7 oleku vaatamiseks tehke järgmist.  

    sudo /etc/init.d/tomcat7 status

Tomcat teenuste taaskäivitamiseks järgmist.  

    sudo /etc/init.d/tomcat7 restart

###<a name="tomcat-administration"></a>Tomcat haldus
Saate redigeerida Tomcat kasutaja konfiguratsiooni faili setup administraatori identimisteave järgmine käsk:  

    sudo vi  /etc/tomcat7/tomcat-users.xml   

Siin on näide:  
![][17]  

>AZURE'I. Märkus: Looge keeruka parooli administraatori kasutajanime.  

Pärast selle faili redigeerimine, peate tomcat7 teenuste tagamaks, et muudatused jõustuvad järgmine käsk:  

    sudo /etc/init.d/tomcat7 restart  

Avage brauser ja sisestage URL-i **http://<your tomcat server DNS name>/halduri/html**. Selles artiklis näiteks URL on http://tomcatexample.cloudapp.net/manager/html.  

Pärast ühenduse loomist, peaksite nägema umbes järgmine:  
![][18]

##<a name="common-issues"></a>Levinud probleemid

###<a name="cant-access-the-virtual-machine-with-tomcat-and-moodle-from-the-internet"></a>Te ei pääse juurde virtuaalse masina Tomcat ja Moodle Interneti kaudu

-   **Sümptom**  
Tomcat töötab, kuid te ei näe teie brauseriga Tomcat vaikelehe.
-   **Võimalikud root juhul**   
    1.  Tomcat kuulata port on sama mis era pordi tomcat liikluse virtuaalse masina lõpp-punkti.  

        Kontrollige oma avaliku Port ja privaatne Port lõpp-punkti sätteid ja veenduge, et era Port on sama, mis on tomcat kuulata pordi. Vt etapp 1: Luua pilt juhised lõpp-punktid virtual arvuti jaoks konfigureerimise kohta.  

        Avage tomcat kuulata pordi määratlemiseks /etc/httpd/conf/httpd.conf (punane rolli väljaanne) või /etc/tomcat7/server.xml (Debian väljaanne). Vaikimisi on tomcat kuulata pordi 8080. Siin on näide:  

            <Connector port="8080" protocol="HTTP/1.1"  connectionTimeout="20000"  URIEncoding="UTF-8"            redirectPort="8443" />  

        Kui kasutate virtuaalse masina nagu Debiani või Ubuntu ja te soovite muuta, on vaikimisi port of Tomcat kuulata (nt 8081), peaksite avama pordi ka OS. Kõigepealt avage eemaldatav profiil.  

            sudo vi /etc/default/tomcat7  

        Klõpsake viimase rea eemaldamine ja muutmine "ei" "Jah".  

            AUTHBIND=yes

    2.  Tulemüüri on keelatud kõuts pordi kuulata.

        Kui näete ainult kohaliku Host Tomcat vaikelehe, siis probleem on kõige tõenäolisemad, et tulemüüri portide, mis on kuulda tomcat, on blokeeritud. W3m tööriista abil saate sirvida veebilehele. Järgmised käsud w3m installida ja liikuge sirvides lehele, Tomcat vaikimisi.  

            sudo yum  install w3m w3m-img
            w3m http://localhost:8080  

-   **Lahendus**
    1. Kui soovitud tomcat kuulata port pole sama, mis era pordi lõpp-punkti jaoks virtuaalse masina, muutmist peate olema sama, mis on tomcat kuulata pordi pordi era.   

    2.  Kui probleem on põhjustatud tulemüüri/iptables, lisada järgmised read /etc/sysconfig/iptables:  

            -A INPUT -p tcp -m tcp --dport 80 -j ACCEPT
            -A INPUT -p tcp -m tcp --dport 443 -j ACCEPT  

        Pange tähele, et teine rida on vaja ainult https-liikluse.  

        Veenduge, et see on kohal kõik read, mis oleks globaalselt piirata juurdepääsu, nt mõni järgmistest:  

            -A INPUT -j REJECT --reject-with icmp-host-prohibited  

        Funktsiooni iptables uuesti laadimiseks, käivitage järgmine käsk:  

            service iptables restart  

        See on testitud CentOS 6.3.

###<a name="permission-denied-when-upload-you-project-files-to-varlibtomcat7webapps"></a>Luba keelatud, millal te projekti /var/lib/tomcat7/webapps failide üleslaadimine /  

-   **Sümptom**  
Mis tahes SFTP kliendi (nt FileZilla) kasutamisel ühenduse virtual arvuti ja liikuge /var/lib/tomcat7/webapps/avaldada oma saidi kuvatakse tõrketeade sarnaneb järgmisega:  

        status: Listing directory /var/lib/tomcat7/webapps
        Command:    put "C:\Users\liang\Desktop\info.jsp" "info.jsp"
        Error:  /var/lib/tomcat7/webapps/info.jsp: open for write: permission denied
        Error:  File transfer failed

-   **Võimalikud juurkausta puhul** Teil pole õigusi /var/lib/tomcat7/webapps kaust.  
-   **Lahendus**  
Õigusi on vaja toomine root konto. Saate muuta selle kausta omandiõiguse juurkausta kasutasite ettevalmistamise masina kasutajanimi. Siin on näide azureuser konto nimi:  

        sudo chown azureuser -R /var/lib/tomcat7/webapps

    -R suvandi abil saate rakendada ka kõik failid kataloogi sees õiguste.  

    Pange tähele, et see käsk töötab ka kataloogid. -R suvand muudab kõik failid ja kataloogid kataloogi sees õigusi. Siin on näide:  

        sudo chown -R username:group directory  

    See käsk muudab kõigi failide ja kataloogide sees directory ja kataloogi ise omaniku (kasutaja ja rühma).  

    Järgmine käsk ainult muutub õiguse kaust kataloogi, kuid jätab failide ja kaustade sees kataloog eraldi.  

        sudo chown username:group directory



[1]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-01.png
[2]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-02.png
[3]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-03.png
[4]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-04.png
[5]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-05.png
[6]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-06.png
[7]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-07.png
[8]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-08.png
[9]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-09.png
[10]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-10.png
[11]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-11.png
[12]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-12.png
[13]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-13.png
[14]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-14.png
[15]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-15.png
[16]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-16.png
[17]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-17.png
[18]: ./media/virtual-machines-linux-classic-setup-tomcat/virtual-machines-linux-setup-tomcat7-linux-18.png
