<properties 
   pageTitle="Azure'i SDK .net-i 2.6 väljalaskemärkmed" 
   description="Azure'i SDK .net-i 2.6 väljalaskemärkmed" 
   services="app-service/web" 
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

 
# <a name="azure-sdk-for-net-26-release-notes"></a>Azure'i SDK .net-i 2.6 väljalaskemärkmed

See dokument sisaldab selle väljalaskemärkmed Azure'i SDK .NET 2.6 väljaande. 

Azure'i SDK 2.6 saate arendada cloud teenuserakenduste (PaaS) suunamise .NET 4.5.2 või .NET 4.6, tingimusel, et käsitsi installimise suunatud .NET Frameworki pilvepõhise teenuse roll. Lugege [installima .net-i teenus Cloud roll](http://go.microsoft.com/fwlink/?LinkID=309796).


##<a name="service-bus-updates"></a>Teenuse siini värskendused

- Sündmuse jaoturi: 

    - Sündmuste saatmisel, asetades täiendavad Publisheri lõpp-punkti jaoks sündmuse jaoturi võimaldab nüüd suunatud juurdepääsu reguleerimine.
    - Täiendavad stabiilsuse ja täiustamise lisatakse sündmuse jaoturi funktsioon.
    - Lisades Amqp protokolli tugi üle WebSocket sõnumside ja sündmuse jaoturi.

##<a name="hdinsight-tools-for-visual-studio-updates"></a>Hdinsightiga Tools for Visual Studio värskendused

- **IntelliSense'i parandamiseks**: remote metaandmete päringusoovituste

    Hdinsightiga Tools for Visual Studio toetab nüüd remote getting metaandmete redigeerite oma taru skript. Näiteks saate tippida * *Valige* saatja** ja kuvatakse kõik tabeli nimed. Lisaks veergude nimed kuvatakse pärast tabeli.

- **Hdinsightiga emulaator tugi**

    Kohe Hdinsightiga Tools for Visual Studio toetavad ühenduse Hdinsightiga emulaator, nii, et teie taru skriptide võib tekkida kohalikult ilma tutvustus mis tahes maksumus, seejärel käivitada nende skriptide Hdinsightile oma kogumite suhtes. 

    Lisateabe saamiseks vaadake [selle käsitsi](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

- **Üldise Hadoopi kogumite kasutajatugi Hdinsightiga Tools for Visual Studio** (Eelvaade)

    Hdinsightiga Tools for Visual Studio toetab nüüd üldise Hadoopi kogumite, nii, et Hdinsightiga Tools for Visual Studio abil saate teha järgmist:

    - ühenduse loomine oma kobar 
    - Kirjutage taru päringu toega täiustatud IntelliSense'i/automaatne lõpetamine 
    - Saate vaadata kõigi klaster on intuitiivse Kasutajaliidese abil. 

    Lisateabe saamiseks vaadake [selle käsitsi](http://go.microsoft.com/fwlink/?LinkID=529540&clcid=0x409).

##<a name="in-role-cache-updates"></a>Rolli vahemälu värskendused

- **Microsoft Azure'i salvestusruumi SDK** versiooni 4.3 kasutama värskendatud **- Rolli vahemälu** . Seni oli **-Rolli vahemälu** Azure'i salvestusruumi SDK versioon 1.7 abil.

    Klientide Azure SDK 2,5 või allpool peaks Azure'i SDK 2.6 värskendamine ja teisaldamine teenusekomplekti Azure'i salvestusruumi SDK uus versioon. 

    Praegu on Azure Storage versioon 2011-08-18 ajastatud 1 August 2016 eemaldada. Mis tahes migratsioon-rolli vahemälu Azure'i SDK 2,5 või allpool 2.6 peate olema valmis selleks. Azure Storage versioon 2011-08-18 pensionile kohta leiate lisateavet teemast [Microsoft Azure'i salvestusruumi teenuse versioon eemaldamine Update: 2016 laiendamine](http://blogs.msdn.com/b/windowsazurestorage/archive/2015/10/19/microsoft-azure-storage-service-version-removal-update-extension-to-2016.aspx).

>[AZURE.IMPORTANT]Me pole teada 30 November 2016 aegumine Azure'i hallatavate vahemälu teenuse ja Azure-rolli vahemälu. Soovitame migreerimiseks Azure'i Redis vahemällu selle aegumine ettevalmistamiseks. Kuupäevad ja migreerimise juhised selle kohta leiate lisateavet teemast [mis Azure'i vahemälu pakkumine on mulle sobivaim?](../redis-cache/cache-faq.md#which-azure-cache-offering-is-right-for-me)

##<a name="azure-app-service-tools"></a>Azure'i rakendust Service tööriistad

Värskendati järgmisi üksusi Azure'i SDK 2,6 väljaanne.

- Azure'i avaldamise täiustatud kaasata Azure'i API Apps juurutamine sihtfail nimega.
- API rakendused ettevalmistamise funktsioonid võimaldavad kasutajatel API rakenduse loomine ja ettevalmistamise funktsionaalsusega.
- Server Explorer muudetud kajastamiseks uus rakendus teenuse sõlm Rühmitusalus ressursirühm API Web ja Mobile rakendustega.
- Lisage Azure API rakenduse kliendi žest lisatakse enamik C# projektid, mis võimaldab ärplema lubatud API rakendused töötavad kasutaja Azure tellimuse automaatne loomine.
- Ainult on saadaval Visual Studio 2013 API rakenduste instrumentaarium ja rakenduse teenuse sõlmed Server Explorer. 

##<a name="azure-resource-manager-tools-updates"></a>Azure'i ressursihaldur tööriistad värskendused

Azure'i ressursi manager tööriistad on värskendatud kaasata Mallid Virtuaalmasinates, võrgunduse ja salvestusruumi. Lisada uue liigendusvaate Mallid ja redigeerida malli kasutades Koodilõigud JSON on värskendatud JSON, redigeerida. Visual Studio juurutatud mallide kasutamine PowerShelli skripti varustatud projekt, nii skripti tehtavaid muudatusi kasutab Visual Studio.

##<a name="diagnostics-improvements-for-cloud-services"></a>Pilveteenustega diagnostika täiustused

Azure'i SDK 2.6 toob tagasi tugi Azure Arvuta emulaator diagnostika logide kogumine ja kanda neid arengu salvestusruumi. Mis tahes diagnostika logib (sh rakenduse Jälita logid sündmuse jälgimise Windows (ETW) logid, jõudluse hinnale, taristu logid ja Windowsi sündmuselogide) loodud kui rakendus töötab emulaator saab üle kanda arengu salvestusruumi kinnitamaks, et teie kohalikus arvutis töötab teie diagnostika logimine. 

Nüüd saab määrata diagnostika salvestusruumi konto konfigureerimine (.cscfg) teenusefail hõlpsam kasutada erinevaid diagnostika salvestusruumi kontod viibite. On olulised erinevused kuidas ühendusstringi töötanud Azure'i SDK 2.4 ja Azure SDK 2.6. Diagnostika salvestusruumi ühenduse kasutamise kohta lisateabe saamiseks stringi ja kuidas see mõjutab oma projekte leiate [Azure'i pilveteenustega diagnostika konfigureerimine](http://go.microsoft.com/fwlink/?LinkID=532784).

##<a name="breaking-changes"></a>Suurte muudatuste

###<a name="azure-resource-manager-tools"></a>Azure'i ressursihaldur tööriistad 

- Saadaval Azure SDK 2,5 **Pilve juurutamise projektid** projekti tüüp on ümber **Azure'i**ressursirühma.
- **Pilve juurutamise projektid** Azure'i SDK 2,5 loodud projekte saate kasutada 2.6, kuid juurutamine Visual Studio malli nurjub. Siiski juurutamine PowerShelli skripti endiselt töötab nii nagu varem.  Lisateavet selle kohta, kuidas kasutada **Pilve juurutamise projektid** 2.6 lugeda, selle [postitada](http://go.microsoft.com/fwlink/?LinkID=534086).
 
##<a name="known-issues"></a>Teadaolevad probleemid

- 64-bitine operatsioonisüsteem on kogumiseks diagnostika logide emulaator nõuab. Diagnostika logid kogutakse ei, kui 32-bitine operatsioonisüsteem. See ei mõjuta muid emulaator funktsioone. 

- Azure'i SDK 2.6 välja 2015/4/29 oli kaks küsimust: 

    - Universaalne App ei saa Visual Studio 2015 laadida, kui arvutisse on installitud Azure'i SDK 2.6.
    - Visual Studio 2013 ja Visual Studio 2015, kus Visual Studio ei reageeri ja dialoogiboks teatega "Emulaator seadistamine diagnostika" kuvamisel jookseb ei silumine pilveteenuses projekti.
    
    Azure'i SDK 2.6 värskendus ilmus 5/18/2015. Värskendatud versioon on 2.6.30508.1601; See sisaldab kahte eespool kirjeldatud probleeme. Saate tuvastada koostamine juhtpaneeli kaudu SDK -> programmid ja funktsioonid-Microsoft Azure'i Tööriistad > Microsoft Visual Studio 2013 – v 2.6. Versiooni veerus kuvatakse järgu number.

    Kui teil on endiselt ees ülaltoodud probleemid, installige uusim versioon Azure 2.6 SDK [VS 2012](http://go.microsoft.com/fwlink/p/?linkid=323511&clcid=0x409), [VS 2013](http://go.microsoft.com/fwlink/p/?linkid=323510&clcid=0x409) või [VS 2015](http://go.microsoft.com/fwlink/?linkid=518003&clcid=0x409).
 
##<a name="see-also"></a>Vt ka

[Tugi- ja pensionile jäämise teavet Azure SDK .net-i ja API-de jaoks](https://msdn.microsoft.com/library/azure/dn479282.aspx/)
