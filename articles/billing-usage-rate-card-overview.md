<properties
   pageTitle="Saada ülevaate oma Microsoft Azure'i ressursside tarbimine | Microsoft Azure'i"
   description="Kasutatakse teadmisi Azure ressursside tarbimine ja trendide Azure'i arveldus kasutus ja RateCard API-de kontseptuaalne ülevaade."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="08/16/2016"
   ms.author="mobandyo;bryanla"/>

# <a name="gain-insights-into-your-microsoft-azure-resource-consumption"></a>Saada ülevaate oma Microsoft Azure'i ressursside tarbimine

Klientide ja partnerite jaoks on vaja täpselt prognoosida ja Azure kulude juhtimiseks.  Liikudes on investeeringute mudelit, mis Opex, samuti tuleb teha showback vs tagasinõude analüüsi, samuti anda režiimi töökindluse hinnang ja arveldus, eriti suure pilve juurutuste võimalus.

Azure'i ressursikasutus ja määr kaardi API-de käsitletakse selle artikli aadressi need vajadused, võimaldades uusi teadmisi oma tarbimine Azure ressursse.  

## <a name="introducing-the-azure-resource-usage-and-ratecard-apis"></a>Azure'i ressursikasutus ja RateCard API-de tutvustus

Ressursi pakkuja osana esitatud Azure ressursihaldur API-de pere rakendatakse Azure'i ressursikasutus ja RateCard API-d.  

### <a name="azure-resource-usage-api-preview"></a>Azure'i ressursikasutus API (eelvaade)
Klientide ja partnerite saate kasutada Azure ressursi kasutamine API nende hinnanguline Azure'i andmete toomine. Järgmised funktsioonid.

- **Azure'i Rollipõhine juurdepääsu reguleerimine** - klientide ja partnerite saate konfigureerida oma Accessi poliitika [Azure portaali](https://portal.azure.com) või [Azure PowerShelli cmdlet-käskude](powershell-install-configure.md) abil määrata, millised kasutajad või rakenduste pääseksid juurde selle tellimuse kasutusandmete kaudu. Helistajad peate kasutama standard Azure Active Directory sõned autentimist. Funktsiooni helistaja peab lisatakse ka lugeja, omanik või kaasautori roll saada juurdepääsu kasutamine andmete Azure teatud tellimuste puhul.

- **Tunni- või igapäevane liitmised** - helistajad saate määrata, kas nad soovivad oma tunni ämbrid või igapäevane ämbrid Azure kasutusandmete. Vaikimisi on iga päev.

- **Eksemplari metaandmete esitatud (sh ressursi sildid)** – eksemplari taseme üksikasjad, nt täielikult kvalifitseeritud ressursi uri (/subscriptions/ {tellimuse id} /..), koos ressursi rühma teabe ja ressursside siltide vastuseks saadud. See aitab joonistub kliendid ja programmiliselt eraldada kasutus sildid, kasutamise juhtudel nagu rist laadimise.

- Vastuseks paremini aru, mida on kasutatud helistajad anda ka küsimise **Ressursi metaandmete esitatud** - ressursi üksikasjad nagu meter nimi, arvesti kategooria, arvesti sub kategooria, üksuse ja piirkond. Me tegeleme ka joondamiseks ressursi metaandmete terminoloogia üle Azure portaali Azure kasutuse CSV, arveldus CSV ja muud avalikkusele suunatud kogemusi, kliendid vastavusse viia andmete üle kogemusi EA.

- **Kõigi pakkumise tüüpide kasutus** – kasutusandmete pääseb kõik pakkuda tüüpi grupikindlustusleping, MSDN-i, rahaliste kohustuse, rahaliste krediidi ja EA hulgas.

### <a name="azure-resource-ratecard-api-preview"></a>Azure'i ressursi RateCard API (eelvaade)
Klientide ja partnerite abil saate Azure'i ressursi RateCard API saada saadaval Azure ressursse, loendit koos hinnad iga teabe eeldatav. Järgmised funktsioonid.

- **Azure'i Rollipõhine juurdepääsu reguleerimine** - klientide ja partnerite saate konfigureerida oma Accessi poliitika [Azure portaali](https://portal.azure.com) või [Azure PowerShelli cmdlet-käskude](powershell-install-configure.md) abil saate määrata, millised kasutajad või rakenduste pääseb RateCard andmetele juurdepääsu kaudu. Helistajad peate kasutama standard Azure Active Directory sõned autentimist. Funktsiooni helistaja peab lisatakse ka lugeja, omanik või kaasautori roll saada juurdepääsu kasutamine andmete Azure teatud tellimuste puhul.

- **Tugi grupikindlustusleping, MSDN-i, rahaliste kohustuse ja rahaliste krediitkaardiga pakub (EA ei toetata)** – see API leiate Azure'i pakkumise taseme määr kohta, vs tellimuse taseme.  See API helistaja peate läbima pakkumise teave ressursi üksikasjad ja määra.  Nagu EA pakub kohandanud määr, per registreerimine, me ei saa anda EA määr sel ajal.

## <a name="scenarios"></a>Stsenaariumid

Siin on mõned stsenaariumid, mida on võimalik kasutamine ja RateCard API-de kombinatsiooni:

- **Azure'i veedavad kuu jooksul** - kliendid saavad kasutada kasutamine ja RateCard API koos oma pilveteenuses parema ülevaate saamiseks kulutada kuu jooksul analüüsib kasutus- ja tasuta hinnangute ämbrid kord tunnis ja iga päev.

- **Teatiste häälestamine** – klientide ja partnerite saate häälestada ressursi või rahaliste põhisesse teatised nende cloud tarbimine saada hinnanguline tarbimis- ja tasuta hinnang kasutamine ja RateCard API abil.

- **Predict arve** – klientide ja partnerite pääseb nende hinnanguline tarbimine, ja cloud veedavad ning rakendada seadme õ algoritmide prognoosida, mida oma arve oleks arveldustsükli lõpuni.

- **Enne tarbimine kulude analüüsi** – kliendid saavad ka RateCard API prognoosida, kui palju oma arve oleks, kui need olid liikumiseks nende töökoormus Azure, mõõt pakkudes kasutamine soovitud kasutus arvud. Kui kliendid on olemasolev töökoormus muude pilved või privaatne pilved, nad ka kaart nende kasutamist on Azure määr saada paremini hinnata nende hinnanguline Azure veedavad. See pakub täiustatud vaadet, mida saab kaudu [Azure'i hinnakirjad kalkulaator](https://azure.microsoft.com/pricing/calculator/)(näiteks) partnerite arveldamine osutada liigendus pakkuda ja Võrdle/kontrast pakkumine eri tüüpi Lisaks grupikindlustusleping, sh rahaliste portfellivalikustsenaariumi kinnitamise ja rahaliste krediitkaardiga. Selle API-d pakuvad ka võimalus maksumus hinnang muudatused regiooniti, nõutav juurutamise otsuste, nagu juurutamisel ressursside maailma eri DCs võib olla otsene mõju kogumaksumus mõjuanalüüsi tüüpi lubamine.

- **Mõjuanalüüsi** -

    - Klientide ja partnerite saate määratleda, kas see peaks küll keerulisemaks käivitamiseks nende töökoormus teises regioonis või mõne muu konfiguratsiooni Azure ressursi. Azure'i ressursside kulude võivad erineda põhinevad Azure piirkond, kus töötab, ja see võimaldab klientide ja partnerite saada maksumus Optimeerimised.

    - Klientide ja partnerite saate määratleda kui Azure pakkumine mõnda muud tüüpi annab parema määr on Azure ressurss.

## <a name="partner-solutions"></a>Partnerite lahenduste

[Microsoft Azure'i kasutus- ja RateCard API -de lubamine Cloudyn pakuvad ITFM klientide](billing-usage-rate-card-partner-solution-cloudyn.md) kirjeldatakse integreerimine Azure arveldus API partneri [Cloudyn](https://www.cloudyn.com/microsoft-azure/)pakutud kogemus.  Sellest artiklist leiate üksikasjaliku ulatuse oma kogemusi, sh lühikest videot, mis näitab, kuidas olete Azure klient ja abil saate Cloudyn Azure arveldus API kasu ülevaateid Azure'i nende andmete põhjal.

[Pilveteenuse Cruiser ja Microsoft Azure'i arveldus API integreerimise](billing-usage-rate-card-partner-solution-cloudcruiser.md) kirjeldatakse [pilve Cruiser Express Azure'i paketi](http://www.cloudcruiser.com/partners/microsoft/) tööpõhimõte otse portaalis WAP võimaldab klientidel sujuvalt ühe kasutajaliidese kaudu hallata oma Microsoft Azure'i era- või majutatud avalikule pilve ja aspekte.   

## <a name="next-steps"></a>Järgmised sammud
+ Leiate [Azure'i arveldus REST API viide](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) üksikasjalikumat teavet nii API, mis on esitatud Azure ressursihaldur API-de määramine.
+ Kui soovite alustada paremale proovi kood, vaadake meie Microsoft Azure'i arveldus API koodinäiteid [Azure'i koodinäiteid](https://azure.microsoft.com/documentation/samples/?term=billing)kohta.

## <a name="learn-more"></a>Lisateave
+ Lisateavet Azure ressursihaldur kohta leiate teemast [Azure ressursihaldur ülevaade](azure-resource-manager/resource-group-overview.md) .
+ Lisateavet on komplekt tööriistu, et aidata omandada cloud mõistmiseks veedavad, vt Gartner artiklis [Market juhend IT rahandus Management (ITFM) tööriistad](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).
