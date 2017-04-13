<properties
   pageTitle="Pilveteenustega ja teenuse struktuuri erinevused | Microsoft Azure'i"
   description="Kontseptuaalne ülevaade migreerimiseks teenuse struktuuri: pilveteenuste rakendusi."
   services="service-fabric"
   documentationCenter=".net"
   authors="vturecek"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/19/2016"
   ms.author="vturecek"/>

# <a name="learn-about-the-differences-between-cloud-services-and-service-fabric-before-migrating-applications"></a>Lisateavet pilveteenustega ja teenuse struktuuri erinevused enne migreerimine rakenduste.
Microsoft Azure'i teenuse struktuuri on väga paindlik, väga usaldusväärne jaotatud rakenduste edasi-genereerimine pilveteenuse rakenduse platvorm. See toob palju uusi funktsioone pakendit, juurutamine, üleminekut ja jaotatud cloud rakenduste haldamine. 

See on sissejuhatav teejuht migreerimine rakenduste kaudu pilveteenustega teenuse struktuuri. See keskendub arhitektuuri ja kujunduse erinevused pilveteenustega ja teenuse struktuuri.
 
## <a name="applications-and-infrastructure"></a>Rakenduste ja taristu

Põhiline vahe pilveteenustega ja teenuse struktuuri on VMs, töökoormus ja rakenduste seos. Siin on töökoormus on määratletud kirjutamise teatud toimingu või teenuse kood.
 
 - **Juurutamise taotluste VMs on pilveteenustega.** Koodi kirjutamise on kindlalt haagitud VM eksemplari, nt Web või töötaja roll. Juurutamiseks on töökoormus pilveteenustega on üks või mitu VM eksemplari, mis töötavad töökoormus juurutamine. Ei ole vahe rakenduste ja VMs ja seega ei ole ametlik määratluse rakenduse. Rakenduse saab vaadelda kogumi veebis või töötaja rolli eksemplari pilveteenustega juurutamise raames või on kogu pilveteenustega juurutamise. Selles näites on kujutatud rakenduse kogumi rolli aknad.
 
![Cloud Services rakenduste ja topoloogia][1]

 - **Teenus on juurutamise rakenduste olemasoleva VMs või teenuse struktuuri, kus käitatakse Windows või Linux.** Kirjutamise teenused on täielikult sidumata aluseks infrastruktuuri, mis on võetud ära teenuse struktuuri rakenduse platvormi, nii mitme keskkondades saab kasutada rakenduse kaudu. Soovitud töökoormus teenuse struktuuri nimetatakse "teenus" ja ühe või mitme teenuse on rühmitatud ametlikult määratletud rakendus, mis töötab teenuse struktuuri rakenduse platvormi. Ühe teenuse struktuuri arvutikobaras saab kasutada mitu rakendust.
 
![Teenuse struktuuri rakenduste ja topoloogia][2]
 
Teenuse ise on rakenduse platvormi kiht, mis töötab Windowsi või Linuxi, pilveteenustega on süsteemi juurutamine Azure'i haldusega VMs koos manustatud töökoormus.
Teenuse struktuuri rakenduse mudel sisaldab mitmeid eeliseid.

 - Kiire käivitamine korda. Loomise VM juhtudel võib olla aeganõudev. Teenuse struktuuri, VMs on ainult juurutatud pärast moodustamiseks, mis majutab teenuse struktuuri rakendus platvorm. Klõpsake sellega rakenduse paketid saab juurutada klaster väga kiiresti.
 - Tihedusega majutada. Cloud Services töötaja rolli VM majutab ühe töökoormus. Teenuse struktuuri, rakendused on eraldi vms, töötavad tähendab suure hulga rakenduste väheste VMs, mis võivad vähendada kogukulu suuremat juurutuste juurutamist.
 - Teenuse struktuuri platvormi saate käivitada suvalist kohta, mis on Windows Server või Linuxi masinad, kas see on Azure või kohapealse. Platvorm pakub mõne võtmiseks kiht üle aluseks infrastruktuuri rakenduse käivitada viibite. 
 - Jaotatud Rakendusehaldus. Teenus on platvormi, et hosts jaotatud rakenduste, vaid ka aitab hallata oma elutsükli sõltumatult majutusteenuse VM või seadme elutsükli.

## <a name="application-architecture"></a>Rakenduse arhitektuur

Pilveteenuste rakenduse arhitektuur tavaliselt sisaldab mitmeid välisteenuste sõltuvusi, nagu teenuse siini, Azure'i tabeli bloobimälu, SQL-i, Redis ja teised hallata riigi ja andmete ja suhtlemine Web ja töötaja rolli pilveteenustega juurutamine. Näide täieliku pilveteenustega rakendus võib välja nägema selline:  

![Cloud Services arhitektuur][9]

Teenuse struktuuri rakenduste võite ka sama välisteenused kasutamine täieliku. Selles näites pilveteenustega arhitektuur teenuse struktuuri: pilveteenuste lihtsaim migreerimise tee on asendada ainult pilveteenustega juurutamise teenuse struktuuri rakendusega, hoides üldise struktuuri sama. Veebi ja töötaja rolli saate teisaldatud teenuse struktuuri kodakondsuseta teenuste minimaalsete koodi muudatusi.

![Pärast migreerimist lihtsa teenuse struktuuri arhitektuur][10]

Selles etapis peaks süsteem sama nagu enne tööd jätkata. Ära teenuse struktuuri stateful funktsioone, välise olekus poed saate sisestamine, nagu stateful teenuste vajaduse korral. See on keerulisem kui lihtne migreerimise Web ja töötaja rolli teenuse struktuuri kodakondsuseta teenustele, mis nõuab kirjutamine kohandatud teenused, mille rakenduse võrdväärse funktsioone nagu enne välise teenuseid. Tehes eelised on järgmised. 

 - Väliste sõltuvuste eemaldamine 
 - Juurutamise, haldus ja versioonitäienduse mudelite ühendamine. 
 
Näide tulemuseks arhitektuur, nende teenuste internaliseerimine võib välja näha selline:

![Pärast kogu migreerimise teenuse struktuuri arhitektuur][11]

## <a name="communication-and-workflow"></a>Suhtlus ja töövoo

Enamik pilveteenuses rakenduste koosnevad rohkem kui ühe taseme. Samuti teenuse struktuuri rakenduse koosneb rohkem kui ühe teenuse (tavaliselt palju teenused). Kaks levinud side mudelid on otsene side ja mõne välise püsival salvestusruumi kaudu.

### <a name="direct-communication"></a>Otsest suhtlus

Otsest teatisega astme saate suhelda otse lõpp-punkti esitatud iga taseme. Nt pilveteenustega, see tähendab, valides VM roll, näiteks juhusliku ID-ga või ringi-jaan saldo koormus ja otse selle lõpp-punkti ühenduse kodakondsuseta keskkonnas.

![Pilveteenustega otsene teabevahetus][5]

 Otsest teatis on levinud side mudeli teenuse struktuuri. Teenuse struktuuri ja pilveteenustega võtme vahe on selle tekstivormingus pilveteenustega loote VM, tuleks teenuse struktuuri ühendamist teenus. See on oluline erinevus paari põhjustel.

 - Teenuse struktuuri teenused pole seotud VMs, et majutada teenuste võib liikumine klaster ja tegelikult, eeldatakse, et liikuda mitmel põhjusel: ressursi tasakaalustamiseks, Tõrkesiirde, rakenduse ja infrastruktuuri uuendamine ja paigutuse või laadi piiranguid. See tähendab, et teenuse eksemplari aadressi saate igal ajal muuta. 
 - Klõpsake teenuse struktuuri VM majutada mitmes teenuses, igal versioonil kordumatu lõpp-punktid.

Teenuse struktuuri võimaldab teenuse discovery, nimetatakse nimeteenus, mida saab kasutada lõpp-punkti aadresside teenuste lahendamiseks. 

![Teenuse struktuuri otsene suhtlus][6]

### <a name="queues"></a>Järjekorrad

Levinud side süsteemi vahel astme kodakondsuseta keskkonnas, nt pilveteenustega on kasutada ka välise salvestusruumi järjekorda tööülesanded kaudu ühe taseme teise püsivalt talletamiseks. Stsenaarium on web astme, mis saadab töö Azure'i järjekorda või teenuse siini, kus töötaja rolli aknad dequeue ja tööde töötlemiseks.

![Cloud Services järjekorda suhtlus][7]

Suhtlus mudelit saab kasutada teenuse struktuuri. See võib olla kasulik, kui migreerimine pilveteenustega taotluse teenuse struktuuri. 

![Teenuse struktuuri otsene suhtlus][8]
 
## <a name="next-steps"></a>Järgmised sammud

Teenuse struktuuri: pilveteenuste lihtsaim migreerimise tee on asendada ainult pilveteenustega juurutamise teenuse struktuuri rakendusega, hoides rakenduse üldine arhitektuur ligikaudu sama. Järgmises artiklis on toodud juhend, mis aitab teisendamine veebis või töötaja rolli teenuse struktuuri kodakondsuseta teenus.

 - [Lihtne migreerimise: teisendamine veebis või töötaja rolli teenuse struktuuri kodakondsuseta teenus](./service-fabric-cloud-services-migration-worker-role-stateless-service.md)

<!--Image references-->
[1]: ./media/service-fabric-cloud-services-migration-differences/topology-cloud-services.png
[2]: ./media/service-fabric-cloud-services-migration-differences/topology-service-fabric.png
[5]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-direct.png
[6]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-direct.png
[7]: ./media/service-fabric-cloud-services-migration-differences/cloud-service-communication-queues.png
[8]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-communication-queues.png
[9]: ./media/service-fabric-cloud-services-migration-differences/cloud-services-architecture.png
[10]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-simple.png
[11]: ./media/service-fabric-cloud-services-migration-differences/service-fabric-architecture-full.png
