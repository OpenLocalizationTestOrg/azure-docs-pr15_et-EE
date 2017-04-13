<properties 
   pageTitle="Microsofti toodete jälgimise haldus Teavita | Microsoft Azure'i"
   description="Teatise näitab, et mõned küsimus, mis nõuab tähelepanu administraatorilt.  Selles artiklis kirjeldatakse kuidas teatiste luuakse ja hallatakse System Center toimingute Manager (SCOM) ja Log Analytics erinevuste ja pakub parimaid tavasid kasutamine hübriid teatiste haldus strateegia kaks tooted." 
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="bwren" />

# <a name="managing-alerts-with-microsoft-monitoring"></a>Microsoft jälgimise teatiste haldamine 

Teatise näitab, et mõned küsimus, mis nõuab tähelepanu administraatorilt.  On suur erinevus System Center toimingute Manager (SCOM) ja Log Analytics toimingud halduse komplekti (OMS) kuidas luuakse teatised, nende hallata ja analüüsida ja kuidas teid teavitatakse kriitiline küsimus tuvastati.

## <a name="alerts-in-operations-manager"></a>Toiminguhalduri teatised
Teatised SCOM loodud üksikute reeglite või kuvarite konkreetse probleemi näitamiseks.  Kuvarit, saate luua teatise sisenemisel on tõrkeolekus ajal reegel võib luua teatise mõned kriitiline küsimus, mis on otseselt seotud hallatava objekti seisundi näitamiseks.  Halduse paketid sisaldada erinevaid töövooge, mis rakenduse või teenuse, mis haldavad teatiste loomine.  Uue halduspaketi konfigureerimise käigus on seda, et tagada, et te ei saa liigse teatiste probleeme, et te ei pea kriitilised häälestamine.

![SCOM teatiste kuvamine](media/operations-management-suite-monitoring-alerts/scom-alert-view.png)

SCOM pakub täielikku teatiste haldus teatiste staatusega saab muuta administraatorid, nagu need töötavad probleemi lahendamiseks.  Kui probleem on lahendatud, määrab administraator teatise sulgeda, mil seda ei kuvata enam aktiivne teatiste kuvamise vaadetes.  Teatised, mis luuakse kuvari saab automaatselt lahendada, kui terve taastatakse jälgida.

![Teatiste olek](media/operations-management-suite-monitoring-alerts/scom-alert-status.png)

## <a name="alerts-in-log-analytics"></a>Log Analytics teatised
Teatise Log Analytics on loodud logifailide päring, mis on automaatselt käivitada intervalliga.  Saate luua reegli, mis tahes Logi päringu.  Kui päring tagastab tulemused, mis vastavad teie määratud kriteeriumidele, luuakse teatise.  See võiks olla teatud päring, mis loob teatise teatud sündmuse tuvastamisel või võite kasutada üldisema päring, mis näeb välja mis tahes tõrke korral, mis on seotud konkreetse rakenduse jaoks.

Logige Logi päringu Kasutusanalüüsi teatiste kirjutada hoidlasse OMS, kui sündmus ja saab tuua.  Nad ei saa olek SCOM sündmusi, nii et saate määrata, kui probleem on lahendatud.

![OMS teatis](media/operations-management-suite-monitoring-alerts/oms-alert.png)

Kui SCOM kasutatakse andmeallikana Log Analytics, kirjutada SCOM teatiste hoidlasse OMS kui need on loodud ja muudetud.  

![Märku](media/operations-management-suite-monitoring-alerts/scom-alert.png)

[Teatiste haldus lahenduse](http://technet.microsoft.com/library/mt484092.aspx) annab ülevaate aktiivse teatised ja tuua erinevate levinud päringute määrab teatisi.  See pakub viljakamate teatiste kui SCOM aruande analüüs.  Saate süvitsi minna kokkuvõtted, et üksikasjalikud andmed ja kohapealseid päringuid tuua erinevaid teatiste loomine.

![Lahendus teatiste haldus](media/operations-management-suite-monitoring-alerts/alert-management.png)

## <a name="notifications"></a>Teatised
SCOM teatised saata teile e-posti või teksti teatised, mis vastavad teatud kriteeriumide vastuseks.  Saate luua muu teatise tellimused, mis on erinevad olenevalt sellist kriteeriumide jälgitava objekti, teatise tõsidust, millist probleem tuvastatud inimeste või kellaaeg.

Mõned tellimused saab rakendada täieliku teatise strateegia suure hulga halduse paketid.

![SCOM teatised](media/operations-management-suite-monitoring-alerts/alerts-overview-scom.png)

Log Analytics saate märku e-posti teel, et iga [reegli](http://technet.microsoft.com/library/mt614775.aspx)toimingu e-posti teatis seades on loodud teatise.  See ei ole SCOM võimaluse Telli mitme teatiste ühe reegli abil.  Peate oma teatiste reeglite loomiseks, kuna OMS ei paku mis tahes eelkonfigureeritud.

![Logige Analytics teatised](media/operations-management-suite-monitoring-alerts/alerts-overview-oms.png)

Te ei saa täielikult SCOM teatiste haldamine Log Analytics küll Kuna muuta saab ainult neid toiminguid konsooli.  Log Analytics on kasulik, kui osa on teatis juhtimine töötlemine küll esitada tööriistad selle SCOM eraldi ei ole.

## <a name="alert-remediation"></a>Teatiste ja kä
[Parandamise](http://technet.microsoft.com/library/mt614775.aspx) viitab katse automaatselt värskendada tähistatud vastava teatise.
  
SCOM saate käivitada diagnostika- ja makstud vastuse kuvarit sisestamise on vigane olekus.  See juhtub samaaegne teatise loomine jälgida.  Diagnostika- ja makstud on tavaliselt rakendatud skripti, mis käivitatakse agent.  Diagnostika proovib tuvastati teema kohta lisateabe kogumine ajal enam avaldab probleemi lahendamiseks.

Log Analytics võimaldab teil on [Azure automatiseerimine käitusjuhendi](https://azure.microsoft.com/documentation/services/automation/) või kõne alustamine on webhook vastuseks Log Analytics teatis.  Tegevusraamatud võib sisaldada keerukate loogika PowerShellis rakendada.  Skripti Azure käivitub ja pääsete juurde mis tahes Azure'i ressursid või välised ressursid saadaval pilveteenuses.  Azure'i automaatika on võimalus käivitada tegevusraamatud teie kohaliku andmekeskuse serveri, kuid see funktsioon pole praegu saadaval Log Analytics teatiste vastuseks käitusjuhendi käivitamisel.

Nii makstud SCOM sisse ja klõpsake OMS tegevusraamatud võib sisaldada PowerShelli skripti, kuid need raskem luua ja hallata, kuna need olla sisalduva halduspakett toiminguhalduri.  Azure'i automaatika, mis pakub funktsioone loome, testimine ja haldamise tegevusraamatud tegevusraamatud on talletatud.

Kui kasutate SCOM Log Analytics andmeallikana, saate luua Logi Analytics teatise, tuua SCOM teatiste talletatud OMS hoidla Logi päringu abil.  See võimaldab teil on Azure automatiseerimine käitusjuhendi käivitamiseks on märku vastuseks.  Muidugi, kuna käitusjuhendi käivituvad Azure, see ei oleks toimiv strateegia makstud kohapealse probleemide jaoks.

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada üksikasju [teatiste sisse System Center toimingute Manager (SCOM)](https://technet.microsoft.com/library/hh212913.aspx).