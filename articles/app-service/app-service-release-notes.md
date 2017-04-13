<properties 
   pageTitle="Azure'i SDK .net-i 2.5.1 väljalaskemärkmed" 
   description="Azure'i SDK .net-i 2.5.1 väljalaskemärkmed" 
   services="app-service" 
   documentationCenter=".net,nodejs,java" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/10/2016"
   ms.author="juliako"/>


# <a name="azure-sdk-for-net-251-release-notes"></a>Azure'i SDK .net-i 2.5.1 väljalaskemärkmed

See dokument sisaldab selle väljalaskemärkmed Azure'i SDK jaoks .NET 2.5.1 versioon. 

##<a name="azure-sdk-for-net-251-release-notes"></a>Azure'i SDK .net-i 2.5.1 vabastage märkmed

Järgmised uued funktsioonid ja Azure SDK .net-i 2.5.1 värskendused.

- Uue features\scenarios seotud **Web tööriistad laiendid**. 

    - Azure'i veebisaitide nimetati ümber Azure'i rakendust Service. Lisateavet leiate teemast [Azure'i rakendust Service ja Azure olemasolevad teenused](app-service-changes-existing-services.md).
    - Azure'i API rakendused (eelvaade) tugi on lisatud, et kliendid saavad avaldada ASP.net-i projektide API apps ja seejärel kasutage lisamine > Azure'i API rakenduse kliendi žest C# koodi juurutatud API rakenduse struktuuri põhjal luua projektid. 
    - Veebisaitide sõlm Server Explorer on taunitud asemel sõlme Azure'i rakenduse teenus, mis sisaldab ressursi rühma-põhiste rühmitamise Azure'i API rakenduste, mobiilirakenduste ja veebirakenduste tugi.
    - Azure'i mobiilirakenduste (eelvaade) tugi on lisatud, et kliendid saate luua uue mobiilirakenduste projektide, lisada mobiilirakenduste kontrollerid, avaldamine projektide ja eemalt silumine rakendusi.
    - Lisa > Azure'i API rakenduse kliendi žest toetab nüüd kohaliku ärplema JSON-failid Web API arendajate saate muude tootjate NuGets nagu Swashbuckle ärplema loomiseks või Autor selle käsitsi. Sellisel viisil kliendi arendajad saate kasutada koodi loomine funktsioone kasutada mis tahes ärplema lõpp-punkti C# projektid. 
    - Web App ja rakenduse API avaldamise dialoogid on täiustatud tugi Azure portaali mõiste ressursi rühmitamise ja valiku/loomine Azure ressursi rühmad ja rakenduse teenuse lepingud on esindatud uus Web App ja rakenduse API ettevalmistamise dialoogiboks. 
    - Azure'i API rakenduse Server Explorer sõlmed linke API rakenduste sügavat link Azure'i portaal, kui ka muid funktsioone, nagu Log Streaming ja Remote silumine.

    Teadaolevad probleemid ja praeguse piirangud Azure'i SDK .NET 2.5.1 [selles](app-service-release-notes.md#known_issues_2_5_1) jaotises allpool.


- Uue features\scenarios seotud **Hdinsightiga tööriistad** Visual Studio on lubatud selles versioonis. 
    - Kohaliku valideerimine taru skriptide. Kas on vigu oma skripti tööriistaribal nuppu skripti Valideeri. 
    - Täiustatud taru töökohtade silumine. Nüüd saate silumine taru tööde haldamine, kasutades lõng logid Visual Studios. Kui teie taotlus on jõudlusprobleemide, uurib LÕNG logid annab kasulikku teavet.
    - (Avaliku eelvaade) Märksõna automaatne lõpetamine ja IntelliSense'i tugi mesilaspere. Aidata teil Autor taru skriptide, Hdinsightiga Tools for Visual Studio lisatud märksõna automaatne lõpetamine ja IntelliSense'i tugi mesilaspere.
    - Torm tugi. Nüüd saate Hdinsightiga Tools for Visual Studio arendada Storm topoloogiatest/otsikuid/poldid C#. Saate esitada arendatud topoloogia Storm arvutikobaras ja topoloogia/polt/tila oleku kuvamine. Süsteemi logid ja klientide logide abil saate oma Storm topoloogiatest/poldid/otsikuid tõrkeotsing. Saate olemasoleva JAVA varad Storm Hdinsightiga kohta.
    
    Lisateavet leiate teemast [Hdinsightiga Hadoopi Tools for Visual Studio kasutamise alustamine](hdinsight-hadoop-visual-studio-tools-get-started.md).



##<a id="known_issues_2_5_1"></a>Azure'i SDK .net-i 2.5.1 teadaolevad probleemid ja piirangud

- Azure'i API rakendused on nähtav mobiilirakenduste juurutamise eesmärk. Web Appsi tuleks järgmises versioonis ainult sihtkoht mobiilirakenduste kohta. 
- Azure'i API rakenduse ettevalmistamise saate tulemuseks edu, kuid aeg-ajalt edenemist Azure'i rakenduse tegevuse aknas värskendamine nurjus. Lahendus on oleku uue Azure API rakenduse Azure'i portaalis. 
- Fail > uus projekt > API rakendus > F5 kasutusvõimalusi, on tulemuseks HTTP tõrge sellepärast, et pole default/index.html. Lahendus on käsitsi sirvimiseks /api/values URL-i. 
- Aeg-ajalt, kuvatakse lapik serveri Exploreri ikoonid. VS taaskäivitamine probleemi lahendab. 
- Kui erandi Expression.Error ajal Web Appi või API rakenduse ettevalmistamise (nt ületatud kvooditõrked või dubleeritud Azure'i API rakenduse lüüsi nimi), Kuva tõrked töötlemata JSON tekst. 
- Vahelduva projekti loomise probleemid, kui rakenduse ülevaated on märgitud projekti loomise ajal.
- Aeg-ajalt, genereeritud Azure'i API rakenduse kliendi koodi puudub nimeruumid, tuleb käsitsi lisada (või Visual Studio näpunäidete kaudu automaatselt imporditud) koodi koostada. 
- Mobiilirakenduse projektide peaks olema avaldatud veebirakenduste, kuid peate valima saidi loodud Azure'i portaalis rakendusena Mobile, kuna Mobile'i rakendus projektide andmebaasi. 
- Avalehe mobiilirakenduste kasutab Termini "mobiilsideteenuse" asemel "mobiilirakendused" 
- Mobiilirakenduse projekti loomise võib kuluda kuni minutiks loomiseks. 
- Ajal API rakenduse ettevalmistamise (mõnel) tõrke pärineb tagasi Azure'i API, mis kajastab, et õigused ei saa seada õigesti, ajal API rakendus on õigesti ette valmistatud ja on kasutamiseks valmis. Käsitsi määratavad õigused Azure'i portaalis.
- Rakenduse ülevaated API rakendusemallid ja mobiilirakenduse malle ei toetata.
- Projektide API rakendus ei saa kasutada koos pilveteenusesse projekte.
- Projekti API rakendusemallid saadaval ainult C#.
- C# on toetatud ainult API rakenduse tarbimine "Lisada Azure API rakenduse klient" kontekstimenüü kaudu.

 
