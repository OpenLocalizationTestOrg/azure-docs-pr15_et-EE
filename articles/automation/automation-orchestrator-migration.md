<properties
   pageTitle="Migreerimine versioonist Orchestrator Azure automatiseerimine | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse System Center Orchestrator tegevusraamatud ja integreerimise pakendis migreerimine Azure automatiseerimine."
   services="automation"
   documentationCenter=""
   authors="bwren"
   manager="stevenka"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="02/09/2016"
   ms.author="bwren" />


# <a name="migrating-from-orchestrator-to-azure-automation-beta"></a>Migreerimine versioonist Orchestrator Azure automatiseerimine (beeta)

Klõpsake [System Center Orchestrator](http://technet.microsoft.com/library/hh237242.aspx) tegevusraamatud põhinevad tegevuste integreerimine tuvastamine, mis on kirjutatud spetsiaalselt Orchestrator ajal Azure'i automaatika tegevusraamatud põhinevad Windows PowerShelli kaudu.  [Graafilise tegevusraamatud](automation-runbook-types.md#graphical-runbooks) Azure'i automaatika on sarnase ilme, et Orchestrator tegevusraamatud nende tegevustega tähistav PowerShelli cmdlet-käsud, lapse tegevusraamatud ja varad.

[System Center Orchestrator migreerimise tööriistakomplekt](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) sisaldab tööriistu aidata teil teisendamine tegevusraamatud Orchestrator Azure automatiseerimine.  Lisaks teisendamine tegevusraamatud, ise, peab teisendada integreerimine moodulid koos Windows PowerShelli cmdlet-käskudega integreerimine pakendis koos tegevused, mida kasutada funktsiooni tegevusraamatud.  

Järgmine on põhiprotsess teisendamise Orchestrator tegevusraamatud Azure automatiseerimine.  Kõik need juhised on allpool üksikasjalikult kirjeldatud.

1.  Laadige alla [System Center Orchestrator migreerimise tööriistakomplekt](http://www.microsoft.com/download/details.aspx?id=47323&WT.mc_id=rss_alldownloads_all) , mis sisaldab tööriistu ja moodulid, mida selles artiklis käsitletakse.
2.  [Standard tegevused mooduli](#standard-activities-module) importimine Azure automatiseerimine.  See sisaldab standard Orchestrator tegevused, mida võib kasutada teisendatud tegevusraamatud teisendatud versiooni.
3.  Importida [System Center Orchestrator integreerimine moodulid](#system-center-orchestrator-integration-modules) Azure automatiseerimine jaoks neid kasutada oma tegevusraamatud, et juurdepääs System Center integreerimine pakendit.
4.  Kohandatud ja muude tootjate integreerimise pakendis [Integreerimine muunduri](#integration-pack-converter) kasutamine teisendada ja importida Azure automatiseerimine.
5.  Teisendada Orchestrator tegevusraamatud [Käitusjuhendi muunduri](#runbook-converter) kasutamine ja installige Azure'i automatiseerimine.
6.  Käsitsi luua nõutav Orchestrator varad Azure automatiseerimine Kuna Käitusjuhendi muundur ei saa muuta järgmisi ressursse.
7.  Konfigureerida [Hübriid Käitusjuhendi töötaja](#hybrid-runbook-worker) teisendatud tegevusraamatud, millele pääsevad kohalikud ressursid käivitamiseks keskuses kohalikud andmed.

## <a name="service-management-automation"></a>Teenuse halduse automatiseerimine

[Teenuse halduse automatiseerimine](http://technet.microsoft.com/library/dn469260.aspx) (SMA) salvestab ja töötab teie kohaliku andmekeskuse nagu Orchestrator tegevusraamatud ja kasutab sama moodulid integreerimine Azure automatiseerimine. [Käitusjuhendi muunduri](#runbook-converter) teisendab Orchestrator tegevusraamatud graafiline tegevusraamatud küll mis SMA ei toetata.  Saate ikkagi installida [Standardmoodul tegevused](#standard-activities-module) ning [System Center Orchestrator integreerimine moodulid](#system-center-orchestrator-integration-modules) üheks SMA, kuid peate käsitsi [kirjutada oma tegevusraamatud](http://technet.microsoft.com/library/dn469262.aspx).

## <a name="hybrid-runbook-worker"></a>Hübriidjuurutuse Käitusjuhendi töötaja

Klõpsake Orchestrator tegevusraamatud on andmebaasi serveris talletatud ja käivitage käitusjuhendi serverites nii oma kohalikud andmed keskele.  Azure'i automaatika tegevusraamatud on Azure pilves talletatud ja käitamise kasutamine [Hü Käitusjuhendi töötaja](automation-hybrid-runbook-worker.md)keskuses kohalikud andmed.  See on, kuidas te tavaliselt käivitada tegevusraamatud, kuna need on mõeldud töötama kohaliku serverites Orchestrator ümber.

## <a name="integration-pack-converter"></a>Integreerimine muunduri

Funktsiooni integratsiooni muunduri teisendab integreerimine pakendis abil [Orchestrator integreerimine tööriistakomplekti (OIT)](http://technet.microsoft.com/library/hh855853.aspx) integreerimine moodulid Windows PowerShelli, mida saate importida Azure automatiseerimine või teenuse halduse automatiseerimine põhjal loodud.  

Funktsiooni integratsiooni muunduri käivitamisel on esitatud viisard, mis võimaldab teil valige integreerimine pakett (.oip) faili.  Viisardi, siis on loetletud tegevuste integreerimine pakett sisaldab ja võimaldab teil valida, mis migreeritakse.  Kui olete viisardi, loob see integreerimine mooduli, sisaldab vastavaid cmdlet-käsu tegevuse algse integreerimine Pack.


### <a name="parameters"></a>Parameetrid

Mis tahes atribuudid tegevuse integreerimine Pack teisendatakse vastavate cmdlet-i integreerimine mooduli parameetrid.  Windows PowerShelli cmdlet-käsud on [levinud parameetrid](http://technet.microsoft.com/library/hh847884.aspx) , mida saab kasutada koos kõigi cmdlet-käsud.  Näiteks Paljusõnaline parameetrit - põhjustab cmdlet-käsu üksikasjalikku teavet selle toimingu väljund.  Pole cmdlet võib-olla sama nimega levinud parameetrina parameeter.  Kui tegevus on sama nimega levinud parameetrina atribuut, viisard teilt pakkuda muu parameetri nimi.

### <a name="monitor-activities"></a>Jälgida tegevuste

Kuvari tegevusraamatud rakenduses Orchestrator käivitada on [jälgida tegevuste](http://technet.microsoft.com/library/hh403827.aspx) ja pidevalt kasutada teatud sündmuse ootamine.  Azure'i automaatika ei toeta kuvari tegevusraamatud, nii, et mis tahes kuvari seotud integreerimine pakett ei saa teisendada.  Selle asemel luuakse kohatäite cmdlet-käsu integreerimine mooduli jälgida tegevuste jaoks.  Selle cmdlet-käsk pole funktsioonid, kuid see võimaldab teisendatud käitusjuhendi, mis kasutab seda installida.  See käitusjuhendi ei saa käivitada Azure automatiseerimine, kuid seda saab installida nii, et saate seda muuta.

### <a name="integration-packs-that-cannot-be-converted"></a>Integreerimine tuvastamine, mida ei saa teisendada

Integreerimine tuvastamine, mis on loodud pole OIT ei saa teisendada teisendi integreerimine keelepaketi. Olemas on ka mõned integreerimine pakendit pakub Microsoft, mida praegu ei saa teisendada see tööriist.  Teisendatud versioonide komplektid integreerimine on [esitatud allalaadimiseks](#system-center-orchestrator-integration-modules) nii, et need saab installida Azure automatiseerimine või teenuse halduse automatiseerimine.


## <a name="standard-activities-module"></a>Standard tegevused mooduli

Orchestrator sisaldab [standard tegevused](http://technet.microsoft.com/library/hh403832.aspx) , mida ei kaasata ka integreerimise keelepaketi, kuid kasutab palju tegevusraamatud kogumist.  Mooduli Standard tegevused on integreerimine mooduli, sisaldab võrdväärse cmdlet-käsu iga järgmiste tegevuste jaoks.  Peate installima selle mooduli integreerimine Azure automatiseerimine enne importimise teisendatud tegevusraamatud, standardne tegevuse kasutavad.

Lisaks toetab teisendatud tegevusraamatud, saab luua uue tegevusraamatud Azure'i automaatika keegi tuttav Orchestrator standard tegevused mooduli cmdlet-käsud.  Ajal kõik standard tegevuse funktsioone saab teha cmdlet-käsud, nad võivad töötada teisiti.  Cmdlet-käskude teisendatud standard tegevused mooduli töötab sama, mis vastavad tegevuse ja kasutada sama parameetrid.  See aitab olemasoleva Orchestrator käitusjuhendi autori Azure'i automaatika tegevusraamatud ette.

## <a name="system-center-orchestrator-integration-modules"></a>System Center Orchestrator integreerimine moodulid

Microsoft osutab [integreerimine pakendis](http://technet.microsoft.com/library/hh295851.aspx) tegevusraamatud automatiseerimiseks System Center komponendid ja muude toodete jaoks.  Mõned integreerimine komplektid põhinevad praegu OIT, kuid ei saa praegu teisendada integreerimine moodulid tõttu teadaolevad probleemid.  [System Center Orchestrator integreerimine moodulid](https://www.microsoft.com/download/details.aspx?id=49555) sisaldab teisendatud versiooni nende integreerimine tuvastamine, mida saate importida Azure automatiseerimine ja teenuse halduse automatiseerimine.  

See tööriist versiooni RTM avaldatakse integreerimine tuvastamine, põhineb OIT, mida saab teisendada integreerimine Pack muundur värskendatud versiooni.  Juhised antakse ka aidata teil teisendamine tegevusraamatud integreerimine pakendit, mis ei põhine OIT tegevuse abil.

## <a name="runbook-converter"></a>Käitusjuhendi muundur

Käitusjuhendi muunduri teisendab Orchestrator tegevusraamatud [graafiline tegevusraamatud](automation-runbook-types.md#graph-runbooks) , mida saate importida Azure automatiseerimine.  

Käitusjuhendi muunduri rakendatakse PowerShelli mooduli cmdlet-käsu nimega **ConvertFrom-SCORunbook** , mis sooritavad muutmise abil.  Kui installite tööriist, loob see laadib cmdlet PowerShelli seansil otsetee.   

Järgmine on põhiprotsess teisendada ka Orchestrator käitusjuhendi ja importida Azure automatiseerimine.  Järgnevatest jaotistest leiate täiendavaid üksikasju tööriista abil ja teisendatud tegevusraamatud töötamine.

1. Ühe või mitme tegevusraamatud eksportimine Orchestrator.
2. Hankige integreerimine moodulid käitusjuhendi kõigi tegevuste jaoks.
3. Teisendage Orchestrator tegevusraamatud eksporditud fail.
4. Vaadake üle logid valideerimiseks teisendamise ja määrata mis tahes käsitsi ülesande teave.
4. Importige teisendatud tegevusraamatud Azure automatiseerimine.
5. Luua mis tahes nõutav varad Azure automatiseerimine.
6. Azure'i automaatika muutmiseks nõutav tegevuse käitusjuhendi redigeerida.

### <a name="using-runbook-converter"></a>Käitusjuhendi muunduri kasutamine

**ConvertFrom-SCORunbook** süntaks on järgmine:

    ConvertFrom-SCORunbook -RunbookPath <string> -Module <string[]> -OutputFolder <string> 

- RunbookPath - tee sisaldav tegevusraamatud teisendamiseks ekspordifail.
- Mooduli - komaga eraldatud loend integreerimine moodulid, mis sisaldab soovitud tegevusraamatud tegevusi.
- OutputFolder - tee kausta loomiseks ümber graafiline tegevusraamatud. 


Näiteks järgmine käsk teisendab tegevusraamatud nimetatakse **MyRunbooks.ois_export**ekspordi faili.  Saate kasutada neid tegevusraamatud Active Directory ja andmete kaitse Manager integreerimine paketid.

    ConvertFrom-SCORunbook -RunbookPath "c:\runbooks\MyRunbooks.ois_export" -Module c:\ip\SystemCenter_IntegrationModule_ActiveDirectory.zip,c:\ip\SystemCenter_IntegrationModule_DPM.zip -OutputFolder "c:\runbooks" 


### <a name="log-files"></a>Logifailide

Käitusjuhendi muundur loob järgmised logifailide teisendatud käitusjuhendi samasse asukohta.  Kui failid on juba olemas, siis need kirjutatakse viimase muutmise teabega.

| Faili | Sisu |
|:---|:---|
| Käitusjuhendi muunduri - Progress.log | Üksikasjalikud juhised, sealhulgas teavet iga toimingu edukat teisendamist teisendatud ja hoiatus iga toiming ei ümber. |
| Käitusjuhendi muunduri - Summary.log  | Kokkuvõte, sh hoiatusi viimase muutmise ja tööülesandeid, mida tuleb teha näiteks muutuja, mis on teisendatud käitusjuhendi vaja luua Järeltegevus.  |

### <a name="exporting-runbooks-from-orchestrator"></a>Orchestrator tegevusraamatud eksportimine

Käitusjuhendi muundur töötab ka ekspordi faili Orchestrator, mis sisaldab ühe või mitme tegevusraamatud.  See loob vastava Azure automatiseerimine käitusjuhendi jaoks iga Orchestrator käitusjuhendi ekspordi faili.  

Mõne käitusjuhendi eksportimine Orchestrator, paremklõpsake käitusjuhendi Käitusjuhendi Designeris nime ja valige **ekspordi**.  Ekspordi kõik tegevusraamatud kausta, paremklõpsake kausta nime ja valige **ekspordi**.


### <a name="runbook-activities"></a>Käitusjuhendi tegevuste

Käitusjuhendi muunduri teisendab iga tegevusest Orchestrator käitusjuhendi vastavate tegevuste Azure'i automaatika.  Tegevused, mida ei saa teisendada, luuakse kohatäite tegevuse käitusjuhendi hoiatus tekstiga.  Pärast teisendatud käitusjuhendi importimine Azure'i automaatika, peate neid toiminguid asendama lubatud tegevused, mida teha vajalikke funktsioone.

Mis tahes Orchestrator seotud [Tegevuste standardmoodul](#standard-activities-module) teisendatakse.  On mõned standard Orchestrator tegevused, mida ei ole seda küll ja ei teisendata.  Näiteks **Saata platvormi sündmus** on pole Azure automatiseerimine samaväärsed, kuna sündmus on seotud Orchestrator.

[Jälgida tegevuste](https://technet.microsoft.com/library/hh403827.aspx) ei teisendata, kuna seal on Azure automatiseerimine neile vaste puudub.  Erandiks on [teisendatud integreerimine pakendis](#integration-pack-converter) mis teisendatakse kohatäite tegevuse jälgimine tegevusi.

Mis tahes tegevus [teisendatud integreerimine pack](#integration-pack-converter) teisendatakse kui esitate parameetriga **moodulid** integreerimine mooduli tee.  Süsteemi Center integreerimine pakid, saate [Süsteemi Center Orchestrator integreerimine moodulid](#system-center-orchestrator-integration-modules).


### <a name="orchestrator-resources"></a>Orchestrator ressursid

Käitusjuhendi muunduri teisendab ainult tegevusraamatud, mitte muude Orchestrator ressursside nagu hinnale, muutujate või ühendused.  Hinnale ei toeta Azure automatiseerimine.  Muutujate ja ühendused on toetatud, kuid peate need käsitsi looma.  Logifailide on teile teada, kui käitusjuhendi nõuab selliste ressursside ja määrake vastav ressursid, millest peate looma Azure'i automaatika jaoks teisendatud käitusjuhendi töötamist.

Näiteks on käitusjuhendi võib kasutada muutuja asustamiseks tegevuse kindla väärtuse.  Teisendatud käitusjuhendi konverteerib tegevuse ja määrata muutuv varade Azure automatiseerimine Orchestrator muutuja sama nimega.  See tuleb märkida **Käitusjuhendi muunduri - Summary.log** faili, mis on loodud pärast teisendamist.  Peate käsitsi luua selle muutuv varade Azure automatiseerimine enne käitusjuhendi abil. 

 
### <a name="input-parameters"></a>Parameetrid

Rakenduses Orchestrator tegevusraamatud Aktsepteeri sisendparameetrite tegevusega **Andmete lähtestamine** .  Kui teisendamise käitusjuhendi sisaldab see tegevus, siis on [sisendparameetrile](automation-graphical-authoring-intro.md#runbook-input-and-output) Azure automatiseerimine käitusjuhendi luuakse iga parameetri tegevuse.  Teisendatud käitusjuhendi, mis toob ja tagastab iga parameetri luuakse [töövoo skripti juhtelemendi](automation-graphical-authoring-intro.md#activities) tegevus.  Mis tahes käitusjuhendi tegevused, mida kasutada ka sisendparameetrile viidata väljund see tegevus.

Põhjus, miks see strateegia kasutatud on Orchestrator käitusjuhendi funktsiooni kõige paremini täita.  Uue graafilise tegevusraamatud tegevusi peaks viitaks otse sisendparameetrid Käitusjuhendi Sisestuskeel andmeallika abil.


### <a name="invoke-runbook-activity"></a>Autonoomsest Käitusjuhendi tegevus

Muud tegevusraamatud alustada tegevusraamatud rakenduses Orchestrator **Autonoomsest Käitusjuhendi** tegevuse. Kui teisendamise käitusjuhendi sisaldab selle tegevuse, **oodake lõpetamise** suvand on määratud, siis käitusjuhendi tegevuse luuakse seda teisendatud käitusjuhendi.  Kui **lõpetamise ootama** suvand pole määratud, siis töövoo skripti tegevuse luuakse kasutava **Algus-AzureAutomationRunbook** käitusjuhendi käivitamiseks.  Pärast teisendatud käitusjuhendi importimine Azure'i automaatika, peate muutma selle tegevuse märgitud teabega.




## <a name="related-articles"></a>Seotud artiklid

- [System Center 2012 – Orchestrator](http://technet.microsoft.com/library/hh237242.aspx)
- [Teenuse halduse automatiseerimine](https://technet.microsoft.com/library/dn469260.aspx)
- [Hübriidjuurutuse Käitusjuhendi töötaja](automation-hybrid-runbook-worker.md)
- [Orchestrator Standard tegevused](http://technet.microsoft.com/library/hh403832.aspx)
- [Süsteemi allalaadimiskeskuse Orchestrator migreerimise tööriistakomplekt](https://www.microsoft.com/en-us/download/details.aspx?id=47323)
 
