
<properties
   pageTitle="Rakenduse täiendamine: täiendamine parameetrid | Microsoft Azure'i"
   description="Kirjeldatakse parameetrit seotud teenuse struktuuri rakenduse, sh seisundikontrollid täitmiseks ja -poliitikad automaatselt tagasivõtmiseks uuele versioonile üleminekut."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>



# <a name="application-upgrade-parameters"></a>Rakenduse täiendamine parameetrid

Selles artiklis kirjeldatakse rakenduse Azure teenuse struktuuri uuendamisel rakendada eri parameetrid. Parameetrid sisaldab nime ja rakenduse versioon. Need on nupud, mis juhib ajalõpp ja mis on rakendatud uuendamisel kontrolle ja need määrata poliitikad, mis peab rakendatakse siis, kui versiooniuuenduse nurjub.


<br>

| Parameetri | Kirjeldus |
| --- | --- |
| ApplicationName | Mis on täiendatud rakenduse nimi. Näited: struktuuri: / VisualObjects struktuuri: / ClusterMonitor  |
| TargetApplicationTypeVersion | Tippige rakenduse versioon, mis versioonitäienduse sihtkohtade. |
| FailureAction | Toimingu võetud teenuse struktuuri, kui uuele versioonile nurjub. Rakendus võib tagasi pöörata eelse Uuenda versiooni (tagasipööramine) või täiendamise lõpetamiseni praeguse täiendamine domeeni juures. Viimasel juhul versioonitäienduse režiimi muudetakse ka käsitsi. Lubatud väärtused on tagasipööramine ja käsitsi. |
| HealthCheckWaitDurationSec | Aega oodata (sekundites), kui uuele versioonile on lõpule jõudnud, versioonitäienduse domeenis enne teenuse struktuuri hindab seisundi rakendus. Selle kestus pidada rakenduse arvutisse olema installitud, enne kui saate pidada terve aeg. Kui soovitud seisundikontrolli, versiooniuuenduse tulu järgmise täiendamine domeeni.  Kui seisundi kontrollimine nurjub, teenuse struktuuri ootab intervall (UpgradeHealthCheckInterval) seisundi kontrolli uuesti proovimiseks kuni soovitud HealthCheckRetryTimeout saavutamiseni. Vaike- ja soovitatav väärtus on 0-sekundiline. |
| HealthCheckRetryTimeoutSec | Kestus (sekundites) selle teenuse struktuuri jätkuvalt seisundi hindamise enne deklareerimist täiendamise ajal nurjus täita. Vaikimisi on 600 sekundit. Selle kestus algab pärast HealthCheckWaitDuration. Selle HealthCheckRetryTimeout jooksul teenuse struktuuri võib teha mitu seisundikontrollid rakenduse tervise. Vaikeväärtus on 10 minuti ja peaks rakenduse jaoks õigesti kohandatud. |
| HealthCheckStableDurationSec | Kestus (sekundites) kinnitamiseks rakenduse säilib enne järgmise täiendamine domeeni teisaldada või Versiooniuuenduse lõpuleviimine. See ootamine kestus kasutatakse avastamata muudatused tervise õige pärast seisundi kontrollimisel. Vaikeväärtus on 120 minutit ja peaks õigesti kohandatud rakenduse. |
| UpgradeDomainTimeoutSec | Ülempiir kellaaeg (sekundites) uuendamise ühe täiendamine domeeni. Kui see käivitub, täiendamise lakkab töötamast ja tulu põhjal UpgradeFailureAction sätet. Vaikeväärtus on kunagi (lõpmatu) ja peaks õigesti kohandatud rakenduse. |
| UpgradeTimeout | Ajalõpp (sekundites), mis rakendatakse kogu versioonile. Kui see käivitub, käivitatakse versioonitäienduse astmed ja UpgradeFailureAction. Vaikeväärtus on kunagi (lõpmatu) ja peaks õigesti kohandatud rakenduse. |
| UpgradeHealthCheckInterval | Frequency, et seisund olek oleks märgitud. See parameeter on määratud ClusterManager osas _kobar_ _näidata_ja osana versioonitäienduse cmdlet-käsk pole määratud. Vaikeväärtus on 60 sekundi järel.  |






<br>
## <a name="service-health-evaluation-during-application-upgrade"></a>Teenuse seisundi hindamist rakenduse täiendamine

<br>
Hindamiskriteeriumide seisund on valikulised. Kui versiooniuuenduse käivitamisel seisund hindamiskriteeriumide pole määratud, kasutab teenuse struktuuri rakenduste seisund poliitikad määratud ApplicationManifest.xml rakenduse eksemplari.


<br>

| Parameetri | Kirjeldus |
| --- | --- |
| ConsiderWarningAsError | Vaikeväärtus on False. Loeb hoiatus seisund sündmused rakenduse tõrked rakenduse uuendamisel seisundi võrdlemisel. Vaikimisi teenuse struktuuri ei väärtustata hoiatus seisund sündmuste olevat tõrked (vead) nii, et uuele versioonile saate jätkata ka siis, kui hoiatus sündmusi.   |
| MaxPercentUnhealthyDeployedApplications | Vaikimisi on soovitatav väärtus 0. Määrake, kui palju juurutatud rakenduste (vt [jaotist seisund](service-fabric-health-introduction.md)), mis võivad olla vigane enne taotluse käsitletakse vigane ja nurjub uuele versioonile. See parameeter määratleb rakenduse seisundi sõlme ja aitab tuvastada probleeme uuele versioonile üleminekul. Tavaliselt saada koopiad taotluse koormusetasakaalustusega muu sõlm, mis võimaldab taotluse kuvada terve, võimaldades jätkamiseks uuele versioonile. Range MaxPercentUnhealthyDeployedApplications seisundi määramisega teenuse struktuuri rakenduse pakett probleem kiiresti tuvastada ja abil saate koostada kiire versioonitäienduse fail. |
| MaxPercentUnhealthyServices | Vaikimisi on soovitatav väärtus 0. Määrake, kui palju teenuste rakenduse eksemplari, mis võivad olla vigane enne taotluse käsitletakse vigane ja nurjub uuele versioonile. |
| MaxPercentUnhealthyPartitionsPerService | Vaikimisi on soovitatav väärtus 0. Määrake, kui palju sektsioonide teenus, mis võib olla vigane enne teenus on vigane. |
| MaxPercentUnhealthyReplicasPerPartition | Vaikimisi on soovitatav väärtus 0. Määrake sektsioon, mis võib olla vigane enne sektsiooni käsitletakse vigane koopiate arvu ülempiir. |
| UpgradeReplicaSetCheckTimeout | **Kodakondsuseta teenuse**--ühe versioonitäienduse domeenis teenuse struktuuri proovib tagada teenuse ülejäänud esinemisjuhud. Kui target eksemplari arvestus on rohkem kui üks, teenuse struktuuri ootab rohkem kui ühe eksemplari saaks kasutada suurima ajalõpp väärtuseni. See käivitub on määratud atribuudi UpgradeReplicaSetCheckTimeout abil. Kui ajalõpp aegumist, teenuse struktuuri tulu uuendada, olenemata eksemplaride arv. Kui target eksemplari arvestus on üks, teenuse struktuuri ootama ja kohe tulu uuele versioonile. **Stateful teenuse**--ühe versioonitäienduse domeenis teenuse struktuuri proovib tagada, et koopia määramine otsustusvõimeline. Teenuse struktuuri ootab otsustusvõimeline saaks kasutada suurima ajalõpp väärtuseni (määratud atribuudi UpgradeReplicaSetCheckTimeout). Kui ajalõpp aegumist, teenuse struktuuri tulu uuendada, olenemata kvoorumi. See säte on määramine kui kunagi (lõpmatu) jooksvalt edasi ja 900 sekundit kui tagasipööramine. |
| ForceRestart | Konfiguratsiooni või andmete värskendamisel teenuse kood värskendamata teenuse taaskäivitamist ainult juhul, kui ForceRestart atribuut on seatud väärtusele tõene. Kui värskendamine on lõpule viidud, teavitab teenuse struktuuri teenuse uue konfiguratsiooni paketi või andmed on saadaval. Teenuse vastutab muudatusi rakendada. Vajaduse korral saate ise taaskäivitage teenus. |



<br>
<br>
Teenuse tüüp rakenduse eksemplari saab määrata MaxPercentUnhealthyServices, MaxPercentUnhealthyPartitionsPerService ja MaxPercentUnhealthyReplicasPerPartition kriteeriumid. Järgmiste parameetrite iga teenuse säte võimaldab rakenduse sisaldama erinevad teenused tüüpi teise poliitika. Näiteks saate kodakondsuseta lüüsi teenuse tüüp on MaxPercentUnhealthyPartitionsPerService, mis on erinev stateful engine teenuse tüüp konkreetse rakenduse eksemplari.

## <a name="next-steps"></a>Järgmised sammud

[Täiendamist oma rakenduse abil Visual Studio](service-fabric-application-upgrade-tutorial.md) juhendab teid rakenduse täiendamine Visual Studio abil.

[Rakenduse abil PowerShelli üleminekut](service-fabric-application-upgrade-tutorial-powershell.md) juhendab teid rakenduse täiendamine PowerShelli kaudu.

Muuta oma rakenduse uuendamine ühilduvad õppida, kuidas kasutada [Andmete sariväljaanne](service-fabric-application-upgrade-data-serialization.md).

Saate teada, kuidas täiendamisel ilmneb rakenduse [Täpsemate teemade](service-fabric-application-upgrade-advanced.md)viidates täpsemate funktsioonide kasutamiseks.

Levinud probleemide rakenduse uuendamine viidates [Rakenduse uuendused tõrkeotsingu](service-fabric-application-upgrade-troubleshooting.md)juhiseid.
 
