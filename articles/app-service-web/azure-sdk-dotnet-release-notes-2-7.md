
<properties 
   pageTitle="Azure'i SDK .NET 2.7 ja .NET 2.7.1 väljalaskemärkmed" 
   description="Azure'i SDK .NET 2.7 ja .NET 2.7.1 väljalaskemärkmed" 
   services="app-service\web" 
   documentationCenter=".net" 
   authors="Juliako" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="app-service"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/17/2016"
   ms.author="juliako"/>

# <a name="azure-sdk-for-net-27-and-net-271-release-notes"></a>Azure'i SDK .NET 2.7 ja .NET 2.7.1 väljalaskemärkmed

##<a name="overview"></a>Ülevaade

See dokument sisaldab selle väljalaskemärkmed Azure'i SDK .NET 2.7 väljaande. 

Dokumendi sisaldama soovitud väljalaskemärkmed Azure'i SDK jaoks .NET 2.7.1 versioon.

Azure'i SDK 2.7 toetab ainult Visual Studio 2015 ja Visual Studio 2013. [Azure'i SDK 2.6](https://azure.microsoft.com/downloads/) on Viimane toetatud SDK Visual Studio 2012.

Üksikasjalikku teavet selles versioonis, leiate [Azure'i SDK 2.7 teadaanne postitada](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) ja [Azure SDK 2.7.1 teadaanne postitada](http://go.microsoft.com/fwlink/?LinkId=623850).

##<a name="azure-sdk-for-net-27"></a>Azure'i SDK .net-i 2.7

###<a name="sign-in-improvements-for-visual-studio-2015"></a>Logige sisse täiustatud Visual Studio 2015

Azure'i SDK 2.7 Visual Studio 2015 toetab identiteedi uued haldusfunktsioonid Visual Studio 2015.  See hõlmab tugi kontod juurdepääs Azure'i Rollipõhine juurdepääsu juhtimine, pilve lahenduste pakkujad, DreamSpark ja muud tüüpi konto ja tellimuse kaudu.

Azure'i SDK 2.7 kaasas sisselogimise täiustused saadaval ainult Visual Studio 2015. Visual Studio 2013 tugi on kaasatud Azure'i SDK 2.7.1.


###<a name="mobile-sdk"></a>Mobiilse SDK

Värskendatud **Mobile'i rakendused** Mallid kajastamiseks uusim [Nugeti paketi](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Server/) ja häälestamise protsess.

###<a name="service-bus"></a>Teenuse siini 

Üldised veaparandused ja täiustused. Üksikasjalike värskendused ja funktsioone, vaadake väljalaskemärkmed uusima [Teenuse siini Nugeti](http://www.nuget.org/packages/WindowsAzure.ServiceBus/).

###<a name="hdinsight-tools"></a>Hdinsightiga tööriistad 

See väljaanne toimus järgmised värskendused. Need värskendused on eelvaates. [Lisateabe saamiseks vt.](http://go.microsoft.com/fwlink/?LinkId=619108)

- Taru graafikud mesilaspere Tez töökohtade
- Täielik taru piirmäära IntelliSense'i tugi
- Siga malli tugi
- Azure'i teenuste torm Mallid

####<a name="breaking-changes"></a>Suurte muudatuste

- Vana **torm** projekt tuleb täiendada, kui selle versiooni tööriistade abil. [Lisateabe saamiseks vt.](http://go.microsoft.com/fwlink/?LinkId=619108)
- Visual Studio Web Express pole enam toetatud. [Lisateabe saamiseks vt.](http://go.microsoft.com/fwlink/?LinkId=619108)

###<a name="azure-app-service-tools"></a>Azure'i rakendust Service tööriistad

Selles väljaandes järgmised värskendused tehtud Web tööriistad laiendid. [Lisateabe saamiseks vt.](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/) 

- DreamSpark kontod lisatud tugi
- Azure'i tööriistad tehtud tugi Azure ressursside halduse API täielik muutmine
- Azure'i rakenduse teenus [Cloud Explorer](#cloud_explorer) lisatud tugi

####<a name="known-issues"></a>Teadaolevad probleemid

Web Appi juurutamise pesa sõlmed ei kuvata Server Explorer Slots sõlme all ja veebirakenduse juurutamise pesa tütarsõlmede ei laadita jaotises Cloud Explorer. See probleem on lahendatud ja valmis järgmise SDK väljaanne. 


###<a name="cloud_explorer"></a>Pilveteenuse Exploreri Visual Studio 2015

Azure'i SDK 2.7 sisaldab Cloud Explorer Visual Studio 2015, mis võimaldab teil vaadata oma Azure ressursse, nende atribuutide kontrollimiseks ja võtme arendaja toimingute Visual Studio teha. 

Cloud explorer toetab järgmist:

- Azure'i ressursside vaated ressursirühm ja ressursside tüüp 
- Ressursid (saadaval vaates ressursi tüüp) nime järgi otsimine
- Tellimused ja ressursse, mis on rolli vastavalt juurdepääsu juhtimine (RBAC) rakendatud tugi 
- Integreeritud toimingu paani, kus on kuvatud arendaja seotud toiminguid kindla valitud ressursid. Näide: manustamine Kaug jaoks loodud Virtuaalmasinates ressursihaldur virnas, kasutades diagnostika andmeid vaadata Virtuaalmasinates jne.
- Integreeritud atribuudid paani, kus on kuvatud arendaja värskendustest atribuudid, mis on sageli vaja arendaja katse ajal 
- Kiirülevaate üleminek konto, mida soovite kasutada, kui nummerdamine ressursid (kasutage tööriistaribal käsku sätted). 
- Filtreerimine tellimuste kasutada nummerdamine ressursid (kasutage tööriistaribal käsku sätted) 
- Sügav lingid Azure portaali ressursid ja ressursside rühmade haldamine 
 
 
###<a name="azure-resource-manager-tools"></a>Azure'i ressursihaldur tööriistad 

Azure'i ressursihaldur tööriistad on värskendatud rolli vastavalt juurdepääsu juhtimine (RBAC) ja uus tellimus tüüpi töötamiseks.  Kaasas need muudatused on võimalus kasutada uute salvestusruumi kontod klassikaline salvestusruumi, lisaks juurutamisel esemeid talletamiseks.  

Kui kasutate mõnda Azure'i ressursirühm projekti mõnelt varasemalt versioonilt SDK SDK 2,7, uue juurutamise script on vaja juurutada salvestusruumi uue konto kasutamise asemel klassikaline salvestusruumi.  Teil palutakse enne muudatuste tegemist projekti uue skripti lisamiseks.  Vana skripti nimetatakse ümber ning teil tuleb teha käsitsi uue skripti muudatustest.
 
 
###<a name="storage-explorer-tools"></a>Salvestusruumi Exploreris tööriistad 

- Lisa plekid vaatamise tugi. Lisateabe saamiseks [selle](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/04/13/introducing-azure-storage-append-blob.aspx)ajaveebipostitus. 
- Tugi vaatamine Premium salvestusruumi kontod serveri Exploreri kaudu. Server Explorer kuvatakse ainult lehe plekid premium salvestusruumi konto, kui need on ainus toetatud tüüp premium salvestusruumi konto.

###<a name="azure-data-factory-tools-for-visual-studio"></a>Azure'i andmed Factory Tools for Visual Studio 

Visual Studio **Azure Andmeriistad Factory** tutvustus Allpool on lubatud funktsioonid. Vaadake lisateavet [selle ajaveebi](http://go.microsoft.com/fwlink/?LinkId=617530) .

- **Malli põhjal loome**: valige Kasuta ääretule vastavalt Mallid, andmete liikumine mallid või lõpuni andmete integreerimise lahenduse juurutamine ja alustamise käed kiiresti koos andmete Factory töötlemise mallid. 
- **Integreerimine Solution Exploreris koostamine ja juurutamine andmete Factory üksuste**: luua ja juurutada torujuhtmed ja seotud üksuste Visual Studio projektid. 
- **Diagrammi integreerimine vaatamine visuaalse suhtluse ajal loome**: visuaalselt autori torujuhtmete ja andmekomplektide diagrammi vaate kaudu abi. 
- **Server explorer sirvimine ja suhtlus juba käimasolevas üksustega integreerimine**: kasutada Server Explorer Sirvi juba juurutanud andmete tehased ja vastavad üksused. Projekti importimine juurutatud andmete factory või üksus (müügivõimaluste lingitud teenus andmekomplektide). 
- **JSON redigeerimine valideerimine skeemi ja rikkaliku IntelliSense'i**: tõhus konfigureerimine ja andmete Factory üksuste koos rikkaliku IntelliSense'i ja -skeemifailid valideerimine JSON dokumentide redigeerimine 
- **Mitme keskkonna avaldamise**: avaldamine torujuhtmete arendaja, testi või tootekataloogi keskkonna autoriks eraldi config failide iga keskkonna loomine.
- **Siga, taru ja .net-i põhiste andmete töötlemise tugi**: siga ja taru skriptide andmete Factory Projectis. Viitamine C# projekti haldamise .net-i tugi tegevuse.

##<a name="azure-sdk-for-net-271"></a>Azure'i SDK .net-i 2.7.1

Järgmine jaotis sisaldab värskendusi, mis võeti kasutusele Azure'i SDK jaoks .NET 2.7.1 versioon.
###<a name="hdinsight-tools"></a>Hdinsightiga tööriistad 

Üksikasjalikuma selgituse Hdinsightiga tööriistad värskenduste kohta, vaadake [selles blogis](http://go.microsoft.com/fwlink/?LinkId=623831).

- Taru töö tehtemärk vaade (uus funktsioon)

    Et aidata teil mõista taru päringu lisati paremini, funktsiooni taru tehtemärk vaade. Kõik tehtemärgid sees tipp kuvamiseks topeltklõpsake tipud töö graafik. Kindla korraldaja rohkem üksikasju kuvamiseks viige kursor haldaja.
- Taru tõrge markeri (uus funktsioon)

    Selleks, et saaksite vaadata grammatikavigade märgistus lisati kiire, funktsioon taru tõrge Marker. Lisaks on täiustatud tõrketeated ja nüüd saate kiire ülevaate üksikasjalik grammatikavigade (kuni selles versioonis saate oli esitada taru skripti klaster ning oodake veidi aega saada sõnum tõrke üksikasjade).  
- Torm topoloogia graafik (uus funktsioon)

    Visualiseerimine on väga oluline, kui soovite näha, kas teie topoloogia töötab ootuspäraselt. Selles väljaandes lisasime visualiseeringu jaoks Storm diagrammid. Te saate visualiseerida olulised mõõdikute teie topoloogia jaoks (nt Värv näitab ilm teatud polt on "hõivatud" või mitte). Võite ka topeltklõps polt/tila üksikasjade kuvamiseks.

- Azure'i portaalis (vigu parandada) loodud Hdinsightiga kogumite tugi

    Nüüd saate vaadata ja edastab tööd kõik teie Hdinsightiga kogumite, olenemata sellest, kus on loodud klaster Visual Studio.

- Lisateavet IntelliSense'i tugi ja kiiremini taru metaandmete laadimine (parandamine)

    Oleme parandanud soovitud IntelliSense'i, lisades rohkem kasutajasõbralik soovitusi. Näiteks tabeli pseudonüüm saate nüüd ka olema soovitatud IntelliSense'i kirjutamise päringu hõlpsam. Samuti oleme parandanud taru metaandmete laadimine nii mitu sekundit, et loetleda kõik andmebaasid, tabelite ja veergude oma taru metastore kulub vaid.

Üksikasjalikuma selgituse Hdinsightiga tööriistad värskenduste kohta, vaadake [selles blogis](http://go.microsoft.com/fwlink/?LinkId=623831).

###<a name="improvements-in-visual-studio-2013"></a>Visual Studio 2013 täiustused

- Azure'i SDK 2.7.1 võimaldab Visual Studio 2013 Azure kontod ja tellimuste Rollipõhine juurdepääsu juhtimine, pilve lahenduse pakkuja ja Dreamspark kaudu juurde pääseda.
- Azure'i SDK 2.7.1, Cloud Explorer tööriista uues aknas on nüüd saadaval ka Visual Studio 2013.

###<a name="known-issues"></a>Teadaolevad probleemid

Azure'i SDK 2.6 või installimise 2.7.1 Visual Studio ühenduse 2013 kohta on mitte - Ladina OS kuvatakse hoiatus inglise ja mitte-ingliskeelsed ressursside Visual Studio võib olla mitteühtivad. Hoiatus võib turvaline tagasi lükata. See kuvatakse üksnes juhul, kui seade ei sisalda Visual Studio ühenduse 2013 varasem install ja installite SDK kohta on mitte - Ladina OS. Hoiatus kuvatakse pärast keelepaketi RTM ressursside kehtib Visual Studios, kuid enne selle kehtib värskendus 4. Hoiatus sulgemisega võimaldab keelepaketi jätkamiseks ja lõpetamiseks rakendamise värskendus 4 versiooni keele paketi sisu.

LightSwitch projektid ei ole compatibile selles versioonis. See probleem lahendatakse järgmise SDK väljaandes.

##<a name="also-see"></a>Vt ka
[Azure'i SDK 2.7.1 teadaanne postituse](http://go.microsoft.com/fwlink/?LinkId=623850)

[Azure'i SDK 2.7 teadaanne postituse](https://azure.microsoft.com/blog/2015/07/20/announcing-the-azure-sdk-2-7-for-net/)

[Tugi- ja pensionile jäämise teavet Azure SDK .net-i ja API-de jaoks](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
