<properties 
    pageTitle="Veebirakenduste Azure'i rakendust Service konfigureerimine" 
    description="Azure'i rakenduse teenused web appi konfigureerimine" 
    services="app-service\web" 
    documentationCenter="" 
    authors="rmcmurray" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="robmcm"/>

# <a name="configure-web-apps-in-azure-app-service"></a>Veebirakenduste Azure'i rakendust Service konfigureerimine #

See teema selgitab, kuidas konfigureerida web appi, [Azure portaali].

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="application-settings"></a>Rakenduse sätted

1. Avage [Azure portaali]tera web app.
2. Klõpsake nuppu **Kõik sätted**.
3. Klõpsake **rakenduse sätted**.

![Rakenduse sätted][configure01]

**Rakenduse sätted** tera on rühmitatud mitu kategooriat sätted.

### <a name="general-settings"></a>Üldsätted

**Versiooni**. Kui teie rakendus kasutab mõnda nende raamistiku nende suvandite määramine 

- **.NET Framework**: määrake .NET Frameworki versiooni. 
- **PHP**: määrake PHP versioon või **väljas** , et keelata PHP. 
- **Java**: valige Java versiooni või keelamiseks Java **väljas** . **Web Container** suvandi abil saate valida Tomcat ja Jetty vahelised erinevused.
- **Python**: valige Python versiooni või keelamiseks Python **väljas** .

Tehniline huvides oma rakenduse Java lubamine keelab .NET, PHP ja Python suvandid.

<a name="platform"></a>
**Platvormi**. Valib kas oma veebirakenduse töötab 32-bitine või 64-bitine keskkonnas. 64-bitine keskkonna jaoks on vaja Basic või Standard režiimis. Tasuta ja ühiskasutuses režiimide alati käivitada 32-bitine keskkonnas.

**Web Sockets**. Määrake **ON** lubamiseks WebSocket protokoll; Näiteks kui teie web Appis kasutab [ASP.net-i SignalR] või [socket.io].

<a name="alwayson"></a>
**Alati**. Vaikimisi on veebirakenduste maha, kui nad on teatud aja jooksul jõudeolekus. See võimaldab süsteemi ressursse säästa. Basic või Standard režiimis, saate lubada **Alati** hoida rakenduse laaditud kogu aeg. Kui teie rakendus töötab pidev tõstab, lubage **Alati**või web tööd ei pruugi töötada usaldusväärselt.

**Hallatavate müügivõimaluste versioon**. Määrab IIS-i [müügivõimaluste režiimis]. Jätke selleks väärtuseks integreeritud (vaikimisi) juhul, kui teil on pärand rakendus, mis nõuab IIS-i varasem versioon.

**Vaheta automaatselt**. Kui lubate automaatne vahetamine juurutamise pesa, Vaheta rakenduse teenuse automaatselt tootmisse web appi, kui vajutada selle pesa värskendust. Lisateabe saamiseks vt [Deploy, et lavastus pesa veebirakenduste teenuses Azure rakenduse] (web-saitide etapiviisilise-publishing.md).

### <a name="debugging"></a>Silumine

**Remote silumine**. Võimaldab remote silumine. Kui lubatud, saate selle Kaug Visual Studio oma veebirakenduse loomiseks. Remote silumine jäävad lubatuks 48 tundi. 

### <a name="app-settings"></a>Rakenduse sätted

See jaotis sisaldab nime ja väärtuse paarideks, mida te web app laadida avakuval üles. 

- .NET rakenduste sisestatakse nende sätete sisse oma .net-i konfigureerimine `AppSettings` käitusajal alistamine olemasoleva sätted. 

- PHP, Python, Java ja sõlm rakendused pääsevad neid sätteid nagu keskkonna muutujate käitusajal. Iga rakenduse sätte, kahes keskkonnas luuakse; ühe kasutaja nimi, mis on määratud rakenduse säte kirje ja teise APPSETTING_ eesliitega. Mõlemad sisaldavad sama väärtuse.

### <a name="connection-strings"></a>Ühendusstringi

Ühendusstringi lingitud ressursid. 

.NET rakenduste sisestatakse need ühendusstringi sisse oma .net-i konfigureerimine `connectionStrings` sätted käitusajal alistamine kirjet, mille võti on lingitud andmebaasi nimi. 

PHP, Python, Java ja sõlm rakenduste, saab neid sätteid nagu keskkonna muutujate käitusajal eesliide ühenduse tüüp. Keskkonna muutuv eesliidete tähised on järgmised: 

- SQL Server:`SQLCONNSTR_`
- MySQL-i:`MYSQLCONNSTR_`
- SQL-i andmebaas.`SQLAZURECONNSTR_`
- Kohandatud:`CUSTOMCONNSTR_`

Näiteks kui MySQL-i ühendusstringi nimetati `connectionstring1`, see oleks juurdepääsetav keskkonna muutuja `MYSQLCONNSTR_connectionString1`.

### <a name="default-documents"></a>Vaikimisi dokumendid

Vaikimisi dokumendi on veebileht, mis on kuvatud juurkausta veebisaidi URL-i.  Loendi esimese sobitamine faili kasutatakse. 

Veebirakenduste võib kasutada moodulid, et URL-i põhjal marsruutimiseks, mitte serveeritakse staatiliseks sisuks, mille puhul seal on vaikimisi dokumendi sellisena.    

### <a name="handler-mappings"></a>Sündmuseohjuri vastendused

Selles jaotises abil saate lisada kohandatud skripti protsessorid käsitlema teatud faililaiendid taotlused. 

- **Pikendamine**. Faililaiend saabuvaid, nt *.php või handler.fcgi. 
- **Skripti protsessor tee**. Skripti protsessori absoluutne tee. Failid, mis vastavad faililaiendit taotluste töötleb skripti protsessori. Kasutage tee `D:\home\site\wwwroot` oma rakenduse juurkaust viidata.
- **Täiendavad argumendid**. Valikuline käsurea argumendid skripti protsessori jaoks 


### <a name="virtual-applications-and-directories"></a>Virtuaalne rakendused ja kataloogide 
 
Määrake virtuaalse rakendusi ja kataloogide konfigureerimiseks iga virtuaalne kaust ja vastava füüsilise teest veebisaidi root suhtes. Soovi korral võite valida **rakenduse** ruut märkimiseks virtuaalkaust rakendusena.


## <a name="enabling-diagnostic-logs"></a>Diagnostikalogide lubamine

Diagnostikalogide lubamiseks tehke järgmist.

1. Valige veebirakenduse jaoks labale **Kõik sätted**.
2. Klõpsake **diagnostikalogid**. 

Suvandid veebirakenduses, mis toetab logimine diagnostikalogid kirjutamiseks: 

- **Rakenduse logimine**. Kirjutab logid failisüsteemis. Logimise kestab 12 tunni jooksul. 

**Tase**. Kui rakenduse logimine on lubatud, on see suvand teabe, mis on salvestatud (tõrge, hoiatus, teave või Verbose).

**Logimise veebiserver**. Logide salvestatakse W3C laiendatud logifaili vormingut. 

**Üksikasjaliku tõrketeated**. Salvestab tõrke üksikasjalik sõnumite HTML-failide. Failid on salvestatud /LogFiles/DetailedErrors alla. 

**Nurjunud taotluste jälitamine**. Logide nurjus taotluste XML-failid. Failid on salvestatud alla /LogFiles/W3SVC*xxx*, kus xxx on kordumatu. See kaust sisaldab XSL-faili ja üks või mitu XML-failid. Veenduge, et alla laadida xsl-fail, kuna see pakub funktsiooni vormingu ja XML-failide sisu filtreerimiseks.

Logifailide vaatamiseks peate looma FTP identimisteave järgmiselt:

1. Valige veebirakenduse jaoks labale **Kõik sätted**.
2. Klõpsake **juurutamise mandaat**.
3. Sisestage kasutajanimi ja parool.
4. Klõpsake nuppu **Salvesta**.

![Mandaadi seadmine juurutamine][configure03]

Täielik FTP kasutaja nimi on "app\username", kui *rakendus* on oma veebirakenduse nimi. Kasutajanimi on loetletud web appi blade jaotises **Essentialsi**.  

![FTP juurutamise identimisteave][configure02]

## <a name="other-configuration-tasks"></a>Muude toimingute konfigureerimine

### <a name="ssl"></a>SSL-I 

Basic või Standard režiimis, saate laadida SSL-sertide kohandatud domeeni. Lisateabe saamiseks vt [lubada HTTPS web appi]. 

Üleslaaditud sertide kuvamiseks klõpsake nuppu **Kõik sätted** > **kohandatud domeenid ja SSL-i**.

### <a name="domain-names"></a>Domeeninimed

Veebirakenduse jaoks kohandatud domeeninimesid lisada. Lisateavet leiate teemast [konfigureerimine veebirakenduse teenuses Azure rakenduse kohandatud domeeni nimi].

Oma domeeni nimede kuvamiseks klõpsake nuppu **Kõik sätted** > **kohandatud domeenid ja SSL-i**.

### <a name="deployments"></a>Juurutuste

- Pideva häälestada. Teemast [Veebirakenduste teenuses Azure rakenduse juurutamiseks Git abil](./web-sites-deploy.md).
- Juurutamise teenindusaegu. Vt [lavastus keskkondades teenuses Azure rakenduse Web Apps juurutamine].


Teie juurutamise slots kuvamiseks klõpsake nuppu **Kõik sätted** > **juurutamine**.

### <a name="monitoring"></a>Jälgimine

Basic või Standard režiimis, saate testida kättesaadavus HTTP- või HTTPS-i lõpp-punktid, kuni kolm geo jaotatud asukohtadest. Jälgimise test nurjub, kui HTTP vastuse koodi on tõrge (4xx või 5xx) või vastuse võtab rohkem kui 30 sekundit. Lõpp-punkti käsitletakse saadaval juhul, kui jälgimise testide õnnestub, alates määratud asukohtadesse. 

Lisateavet leiate teemast [kohta: web lõpp-punkti oleku jälgimine].

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus], kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.

## <a name="next-steps"></a>Järgmised sammud

- [Kohandatud domeeninime Azure'i rakendust Service konfigureerimine]
- [Azure'i rakendust Service rakenduse HTTPS-i lubamine]
- [Mastaapimiseks teenuses Azure rakenduse web app]
- [Veebirakenduste teenuses Azure rakenduse jälgimisega seotud põhitõed]

<!-- URL List -->

[ASP.net-i SignalR]: http://www.asp.net/signalr
[Azure'i portaal]: https://portal.azure.com/
[Kohandatud domeeninime Azure'i rakendust Service konfigureerimine]: ./web-sites-custom-domain-name.md
[Web Appsi Azure rakenduse teenuses lavastus keskkondades juurutamine]: ./web-sites-staged-publishing.md
[Azure'i rakendust Service rakenduse HTTPS-i lubamine]: ./web-sites-configure-ssl-certificate.md
[Kuidas: web lõpp-punkti oleku jälgimine]: http://go.microsoft.com/fwLink/?LinkID=279906
[Veebirakenduste teenuses Azure rakenduse jälgimisega seotud põhitõed]: ./web-sites-monitor.md
[müügivõimaluste režiimis]: http://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Mastaapimiseks teenuses Azure rakenduse web app]: ./web-sites-scale.md
[Socket.IO]: ./web-sites-nodejs-chat-app-socketio.md
[Proovige rakenduse teenus]: http://go.microsoft.com/fwlink/?LinkId=523751

<!-- IMG List -->

[configure01]: ./media/web-sites-configure/configure01.png
[configure02]: ./media/web-sites-configure/configure02.png
[configure03]: ./media/web-sites-configure/configure03.png
