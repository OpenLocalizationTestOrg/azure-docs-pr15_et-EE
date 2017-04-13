<properties
   pageTitle="Rakenduse elutsükli sisse teenuse struktuuri | Microsoft Azure'i"
   description="Kirjeldatakse arendamise, juurutamine, katsetada, üleminekut, säilitades ja teenuse struktuuri rakenduste eemaldamine."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>


<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>


# <a name="service-fabric-application-lifecycle"></a>Teenuse struktuuri rakenduse elutsükkel
Nagu ja muude platvormide jaoks loodud rakenduse kohta Azure teenuse struktuuri tavaliselt läheb läbi järgmised etapid: kujundus, arengu, katsetada, juurutus, üleminekut, hooldus ja eemaldamine. Teenuse struktuuri toetab esimese klassi pilv rakendusi, arengu juurutus, igapäevane haldus ja hooldus kuni lõpliku elutsükli rakenduse täisversioon. Teenuse mudel võimaldab mitme erinevad rollid rakenduse elutsükli sõltumatult osaleda. Selles artiklis antakse ülevaade funktsiooni API-d ja kuidas neid kasutavad erinevad rollid kogu teenuse struktuuri rakenduse elutsükli etapid.

## <a name="service-model-roles"></a>Teenuse mudeli rollid
Teenuse mudeli rollid on:

- **Teenuse arendaja**: areneb muutuv ja üldise teenuste, mida saab uuesti hädavajalikke ja kasutada mitut rakendustes sama tüüpi või erinevat tüüpi. Näiteks saab järjekorda teenuse loomise piletimüügi rakendus (kasutajatugi) või rakenduse pood (ostukorv).

- **Rakenduse arendaja**: loob rakenduste integreerimise teenusekomplekt vastavad teatud kindlate nõuete või stsenaariumid abil. Näiteks võib veebisait pood integreerida "JSON kodakondsuseta eesprotsessor teenus", "Oksjoni Stateful teenus" ja "Järjekorda Stateful teenus" enampakkumise lahenduse koostamine.

- **Rakenduse administraator**: rakenduse konfigureerimine (täitmine konfiguratsiooni malli parameetrid), juurutamise (saadaval ressursid vastendus) ja teenuste kvaliteeti teeb. Näiteks rakenduse administraator otsustab keele lokaadi (inglise keeles Ameerika Ühendriikide puhul) või Jaapani Jaapan, nt rakenduse. Erinevate juurutatud rakendusega võib olla erinevad sätted.

- **Korraldaja**: kasutab rakendused rakendus konfiguratsiooni alusel ja teenuserakenduse administraatori määratud nõuetele. Näiteks tehtemärgi sätted ja rakendus kasutab ja tagab, et see töötab Azure. Tehtemärgid rakenduse seisundi ja jõudluse kohta teabe jälgimine ja haldamine füüsilise infrastruktuuri vastavalt vajadusele.


## <a name="develop"></a>Töötada
1. *Teenuse arendaja* areneb eri tüüpi teenuste abil [Usaldusväärne osalejate](service-fabric-reliable-actors-introduction.md) või [Usaldusväärsed teenused](service-fabric-reliable-services-introduction.md) programmeerimise mudel.
2. *Teenuse arendaja* kirjeldatakse deklaratiivse arendatud teenuse tüüpi teenuse manifest failis koosneb ühe või mitme koodi, konfigureerimine ja andmete paketid.
3. Mõni *rakendus arendaja* koostab seejärel kasutav teenuse eri tüüpi.
4. Mõni *rakendus arendaja* deklaratiivse kirjeldab rakenduse tüüp on rakenduse manifestis teenuse manifestid komponendi teenuste ja õigesti alistamine ja parameterizing eri konfiguratsiooni ja kasutamise sätteid komponendi teenuste viitamine.

Vaadake teemat [Alustamine usaldusväärne osalejad](service-fabric-reliable-actors-get-started.md) ja [Alustamine usaldusväärsed teenused](service-fabric-reliable-services-quick-start.md) näited.

## <a name="deploy"></a>Juurutamine
1. *Teenuserakenduse administraatori* kohandab tüübi konkreetse rakenduse teenuse struktuuri arvutikobaras määrates sobivad parameetrid **ApplicationType** elemendi Rakendusmanifest kasutusele võtta.

2. *Tehtemärk* lisatud rakenduse pakett kobar pilt poe [ **CopyApplicationPackage** meetodit](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) või [ **Kopeeri-ServiceFabricApplicationPackage** cmdlet-käsu](https://msdn.microsoft.com/library/azure/mt125905.aspx)abil. Rakenduse pakett sisaldab Rakendusmanifest ja pakettide kogum. Teenuse struktuuri kasutab rakenduste poest pilt, mis võib olla mõni Azure'i bloobimälu poest või teenuse struktuuri süsteemiteenusel talletatud rakenduse pakett.

3. *Tehtemärk* sätted seejärel rakenduse tüüp soovitud klaster kaudu üles laaditud rakendusepaketi, kasutades [ **ProvisionApplicationAsync** meetodit](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), [ **Register-ServiceFabricApplicationType** cmdlet-käsk](https://msdn.microsoft.com/library/azure/mt125958.aspx)või [ **sätte rakenduse** ülejäänud toiming](https://msdn.microsoft.com/library/azure/dn707672.aspx).

4. Pärast rakenduse ettevalmistamine algab *tehtemärk* rakenduse *teenuserakenduse administraatori* [ **CreateApplicationAsync** meetod](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.createapplicationasync.aspx), [cmdlet-käsu **New-ServiceFabricApplication** ](https://msdn.microsoft.com/library/azure/mt125913.aspx)või [ **Luua rakenduse** ülejäänud toiming](https://msdn.microsoft.com/library/azure/dn707676.aspx)parameetritest.

5. Pärast kasutusele võetud rakendus, kasutab *tehtemärk* [ **CreateServiceAsync** meetod](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.createserviceasync.aspx), [cmdlet-käsu **New-ServiceFabricService** ](https://msdn.microsoft.com/library/azure/mt125874.aspx)või [ **Luua teenuse** ülejäänud toiming](https://msdn.microsoft.com/library/azure/dn707657.aspx) uued teenuse eksemplarid põhjal saadaval teenuste tüüpi rakenduse loomiseks.

6. Rakendus töötab nüüd teenuse struktuuri kobar.

Vt näited [Deploy rakendus](service-fabric-deploy-remove-applications.md) .

## <a name="test"></a>Test
1. Pärast võtavad kohaliku arengu kobar või testi kobar, töötab *teenus arendaja* sisseehitatud Tõrkesiirde test stsenaarium [**FailoverTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenarioparameters.aspx) ja [**FailoverTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.failovertestscenario.aspx) tunnid või [ **Invoke-ServiceFabricFailoverTestScenario** cmdlet-käsu](https://msdn.microsoft.com/library/azure/mt644783.aspx)abil. Tõrkesiirde test stsenaarium käivitatakse määratud teenuse olulised üleminekud ja failovers tagamaks, et see on endiselt saadaval ja töö kaudu.

2. *Teenuse arendaja* töötab seejärel sisseehitatud kaose test stsenaarium [**ChaosTestScenarioParameters**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenarioparameters.aspx) ja [**ChaosTestScenario**](https://msdn.microsoft.com/library/azure/system.fabric.testability.scenario.chaostestscenario.aspx) tunnid või [ **Invoke-ServiceFabricChaosTestScenario** cmdlet-käsu](https://msdn.microsoft.com/library/azure/mt644774.aspx)abil. Kaose test stsenaarium tekitab CEIP sõlm, koodi paketi ja koopia vead klaster sisse.

3. *Teenuse arendaja* [kontrollib - teenusest side](service-fabric-testability-scenarios-service-communication.md) , loome testi stsenaariumi, mis teisaldamine ümber klaster esmane koopiad.

Lisateabe saamiseks vaadake [viga analüüsi teenuse tutvustus](service-fabric-testability-overview.md) .

## <a name="upgrade"></a>Versiooni täiendamine
1. *Teenuse arendaja* komponendi teenuste eksempleeritud rakenduse värskendused ja/või parandab vead ja pakub teenuse manifest uue versiooni.

2. Mõne *rakenduse arendaja* alistab parameterizes konfiguratsiooni ja kasutamise sätete ühtse teenuste ja pakub Rakendusmanifest uue versiooni. Rakenduse arendaja seejärel ühendab uue versioonide teenuse manifestid rakendusse ja pakub värskendatud rakendusepaketi tüüpi rakenduse uus versioon.

3. *Teenuserakenduse administraatori* sisaldab sihtrakenduse tüüpi rakenduse uus versioon, värskendades sobivad parameetrid.

5. *Tehtemärk* lisatud värskendatud rakendusepaketi kobar pilt poe [ **CopyApplicationPackage** meetodit](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.copyapplicationpackage.aspx) või [ **Kopeeri-ServiceFabricApplicationPackage** cmdlet-käsu](https://msdn.microsoft.com/library/azure/mt125905.aspx)abil. Rakenduse pakett sisaldab Rakendusmanifest ja pakettide kogum.

6. *Tehtemärk* sätted target kobar rakenduse uus versioon, kasutades [ **ProvisionApplicationAsync** meetodit](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.provisionapplicationasync.aspx), [ **Register-ServiceFabricApplicationType** cmdlet-käsk](https://msdn.microsoft.com/library/azure/mt125958.aspx)või [ **sätte rakenduse** ülejäänud toiming](https://msdn.microsoft.com/library/azure/dn707672.aspx).

7. *Tehtemärk* uuendab sihtrakenduse [ **UpgradeApplicationAsync** meetod](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.upgradeapplicationasync.aspx), [ **Algus-ServiceFabricApplicationUpgrade** cmdlet-käsk](https://msdn.microsoft.com/library/azure/mt125975.aspx)või [ **rakenduse täiendamine** ülejäänud toimingu](https://msdn.microsoft.com/library/azure/dn707633.aspx)abil uue versiooni.

8. *Tehtemärk* kontrollib edenemist versiooniuuenduse [ **GetApplicationUpgradeProgressAsync** meetod](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.getapplicationupgradeprogressasync.aspx), [cmdlet-käsk **Get-ServiceFabricApplicationUpgrade** ](https://msdn.microsoft.com/library/azure/mt125988.aspx)või [ **Saada rakenduse täiendamine edenemise** ülejäänud toiming](https://msdn.microsoft.com/library/azure/dn707631.aspx).

9. Vajaduse korral *tehtemärk* muudab ja varustuses praeguse rakenduse täiendamine [ **UpdateApplicationUpgradeAsync** meetod](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.updateapplicationupgradeasync.aspx), [ **Värskendus-ServiceFabricApplicationUpgrade** cmdlet-käsk](https://msdn.microsoft.com/library/azure/mt126030.aspx)või [ **Värskenda rakenduse täiendamine** ülejäänud toiming](https://msdn.microsoft.com/library/azure/mt628489.aspx)parameetrid.

10. Vajaduse korral *tehtemärk* koondab tagasi praeguse rakenduse täiendamine [ **RollbackApplicationUpgradeAsync** meetod](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.rollbackapplicationupgradeasync.aspx), [ **Algus-ServiceFabricApplicationRollback** cmdlet-käsk](https://msdn.microsoft.com/library/azure/mt125833.aspx)või [REST **Tagasipööramine rakenduse täiendamine** toiming](https://msdn.microsoft.com/library/azure/mt628494.aspx).

11. Teenuse struktuuri uuendab sihtrakenduse klaster kaotamata kättesaadavus mis tahes komponendi teenused töötavad.

Vt [rakenduse täiendamine õpetuse](service-fabric-application-upgrade-tutorial.md) näiteid.

## <a name="maintain"></a>Haldamine
1. Operatsioonisüsteemi täienduste ja paikade, teenuse struktuuri liidesed Azure'i infrastruktuuri selleks, et tagada kõigi rakenduste töötab klaster kättesaadavus.

2. Täienduste ja paikade teenuse struktuuri platvormi, teenuse struktuuri uuendab ise kaotamata kättesaadavus ühte klaster töötavad rakendused.

3. *Rakenduse administraator* kinnitab lisamise või eemaldamisega sõlmed kaudu klaster pärast ajalooliste võimsus kasutamine andmete ja plaanitud tulevaste nõudmisel analüüsimine.

4. *Tehtemärk* lisab ja eemaldab sõlmed määratud *rakenduse administraatori poole*.

5. Kui lisatakse uus sõlmed või olemasoleva sõlmed eemaldatakse klaster, teenuse struktuuri automaatselt laadi-saldo töötavad rakendused üle kõik sõlmed klaster optimaalse jõudluse saavutamiseks.

## <a name="remove"></a>Eemaldamine
1. *Tehtemärk* kustutada kogu rakenduse abil [ **DeleteServiceAsync** meetod](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.servicemanagementclient.deleteserviceasync.aspx), [cmdlet-käsk **Eemalda-ServiceFabricService** ](https://msdn.microsoft.com/library/azure/mt126033.aspx)või [ **Kustutada teenuse** ülejäänud toiming](https://msdn.microsoft.com/library/azure/dn707687.aspx)eemaldamata konkreetsele eksemplarile töötava teenuse klaster.  

2. *Tehtemärk* saate kustutada ka rakenduse eksemplari ja kõiki selle teenuseid kasutades [ **DeleteApplicationAsync** meetod](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.deleteapplicationasync.aspx), [cmdlet-käsk **Eemalda-ServiceFabricApplication** ](https://msdn.microsoft.com/library/azure/mt125914.aspx)või [ülejäänud **Rakenduse kustutamine** toimingut](https://msdn.microsoft.com/library/azure/dn707651.aspx).

3. Kui rakenduste ja teenuste lõpetanud, saate *tehtemärk* ettevalmistamise tühistamine rakenduse [ **UnprovisionApplicationAsync** meetod](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.unprovisionapplicationasync.aspx), [ **Registreerimise tühistamise-ServiceFabricApplicationType** cmdlet-käsk](https://msdn.microsoft.com/library/azure/mt125885.aspx)või [ **ettevalmistamise rakenduse** ülejäänud toimingu](https://msdn.microsoft.com/library/azure/dn707671.aspx)tüüp. Rakenduse tüüp ettevalmistamise eemaldage selle ImageStore rakenduse pakett. Rakenduse pakett peab käsitsi eemaldada.

4. *Tehtemärk* eemaldab rakenduse pakett ImageStore, kasutades [ **RemoveApplicationPackage** meetodit](https://msdn.microsoft.com/library/azure/system.fabric.fabricclient.applicationmanagementclient.removeapplicationpackage.aspx) või [ **Eemaldamine – ServiceFabricApplicationPackage** cmdlet-käsk](https://msdn.microsoft.com/library/azure/mt163532.aspx).

Vt näited [Deploy rakendus](service-fabric-deploy-remove-applications.md) .

## <a name="next-steps"></a>Järgmised sammud

Arendamise kohta lisateabe saamiseks lugege teemat testimine ja teenuse struktuuri rakenduste ja teenuste haldamine:

- [Usaldusväärne osalejad](service-fabric-reliable-actors-introduction.md)
- [Usaldusväärsed teenused](service-fabric-reliable-services-introduction.md)
- [Rakenduse juurutamine](service-fabric-deploy-remove-applications.md)
- [Rakenduse täiendamine](service-fabric-application-upgrade.md)
- [Testability ülevaade](service-fabric-testability-overview.md)
- [ÜLEJÄÄNUD põhisest rakendusest elutsükli näidis](service-fabric-rest-based-application-lifecycle-sample.md)
