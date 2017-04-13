<properties 
    pageTitle="Azure'i otsingu jõudlust ja optimeerimine kaalutluste | Microsoft Azure'i" 
    description="Azure'i otsingu jõudluse häälestamine ja optimaalse skaala konfigureerimine" 
    services="search" 
    documentationCenter="" 
    authors="LiamCavanagh" 
    manager="pablocas" 
    editor=""/>

<tags 
    ms.service="search" 
    ms.devlang="rest-api" 
    ms.workload="search" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.date="10/17/2016" 
    ms.author="liamca"/>

# <a name="azure-search-performance-and-optimization-considerations"></a>Azure'i otsingu jõudlust ja optimeerimine kaalutlused

Hea otsingut on palju Mobile edu ja veebirakenduste võti. : Kinnisvara, auto turgude online kataloogid, et kasutada kiirotsing ja asjakohaseid tulemeid mõjutab klientide programmikasutuskogemuse. Selles dokumendis on mõeldud abi, võite avastada, et head tavad hankimine kõige paremini Azure'i otsing, eriti keerukaid nõuded skaleeritavus, täpsemalt stsenaariumid mitme keele tugi või kohandatud järjestuse.  Peale selle dokumendi liigendab sisemust ja hõlmab lähenemisel, mis toimivad tõhusalt reaalseid kliendi rakendustes.

## <a name="performance-and-scale-tuning-for-search-services"></a>Jõudluse ja skaala häälestamine otsingu teenuste jaoks

Oleme kogu kasutatakse otsingumootorid nt Bing ja Google ja suure jõudlusega, nad pakuvad.  Selle tulemusena kliendid kasutamisel teie otsing lubatud veebis või mobiilirakenduses nad ootavad jõudluse sarnased omadused.  Kui optimeerimine otsingu jaoks, on üks parimad võimalused keskenduda latentsus, mis suunab päringu lõpetamine ja tulemeid korda.  Kui optimeerimine otsingu latentsus on oluline:

1. Valige target latentsus (või maksimaalne aeg) tüüpilised otsingu taotleb peaks tegema lõpuleviimiseks.

2. Luua ja testida reaal töökoormus suhtes koos realistlik andmekomplekti nende latentsus määrade mõõta oma otsinguteenuse.

3. Alustage päringute sekundis (Transpordieelseks) väike arv ja kasvab käivitada testi enne päringulatentsuse langeb allapoole määratud sihi latentsus arv.  See on mõni oluline võrdlusalus aitavad kavandada skaala rakenduse kasvab kasutus.

4. Võimaluse korral kasutada HTTP ühendused.  Azure'i otsingu .NET SDK kasutamisel Järelikult peaks uuesti kasutada ka eksemplari või [SearchIndexClient](https://msdn.microsoft.com/library/azure/microsoft.azure.search.searchindexclient.aspx) eksemplari ja REST API kasutamisel tuleks ühe HttpClient uuesti kasutada.
 
Need loomisel testimiseks töökoormus, on Azure otsingu pidage meeles, et mõned omadused:

1. On võimalik push korraga nii palju otsingupäringuid, et saadavalolevad Azure'i otsingu teenust ülekoormatud.  Sellisel juhul kuvatakse HTTP 503 vastuse koode.  See on kõige parem alustada erinevate vahemike otsimine taotluste vaatamiseks latentsus määrade erinevused lisamisel rohkem otsimine taotlusi.

2. Azure'i otsingu sisu üleslaadimine mõjutada üldise jõudluse ja Azure otsingu teenuse latentsus.  Kui kavatsete andmeid saata, kui kasutaja on otsinguid, on oluline selle töökoormus arvesse võtta teie.

3. Mitte iga otsingupäringu täidab jõudluse samal tasemel.  Näiteks dokumendi otsingu või otsingu päringusoovituste tavaliselt täidab kiiremini päringu suure arvu omadustega ja filtrid.  See on parim eeldate näha kontole, kui teie hoone erinevate päringute tegemiseks.  

4. Variatsioonide otsingu taotluste on oluline, kuna andmete vahemällu kui täidate pidevalt sama otsing taotlusi, hakkavad jõudlust paremaks, kui see võib veel eri päringu abil määrata teha.

> [AZURE.NOTE] [Visual Studio laadi testimine](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release) on väga hea võimalus oma võrdlustestide täita võimaldab HTTP päringuid käivitada, kui peate päringute vastu Azure'i otsingu teostamiseks ja lubab parallelization taotlused.

## <a name="scaling-azure-search-for-high-query-rates-and-throttled-requests"></a>Azure'i otsimine kõrge päringu määrade suuruse muutmise ja taotlusi rakendus

Kui saate liiga palju throttled taotlusi või ületavad teie target latentsus hind on suurem päringu koormusest, saate vaadata vähendamiseks latentsuse määr on kaks võimalust:

1. **Suurendamiseks koopiad:**  Koopia on nagu koopia oma andmed, mis võimaldavad Azure'i otsingu laadimiseks vastu mitu eksemplari.  Kõik laadimine tasakaalustamiseks ja dispersioonanalüüs andmete üle koopiad haldab Azure'i otsingu ja eraldatud teenust igal ajal koopiate arvu saate muuta.  Mida saab eraldada kuni 12 koopiad Standard otsingu teenuses ja 3 koopiad lihtotsinguavaldis teenuses.  Koopiad saab kohandada, kas [Azure portaali](search-create-service-portal.md) või [Azure otsingu halduse API](search-get-started-management-api.md)kaudu.

2. **Suurendamiseks otsingu taseme:**  Azure'i otsing on saadaval [arvu astme](https://azure.microsoft.com/pricing/details/search/) ja kõik need astme pakub jõudluse erinevaid tasemeid.  Mõnel juhul võib olla nii palju, et taseme, mida on võimalik saada piisavalt madal latentsus hindu, isegi siis, kui koopiad on maxed läbi päringud.  Sel juhul võite kasutamine üks kõrgema otsingu astme nt Azure otsingu S3 kiht, mis sobib hästi suure hulga dokumentide ja ülisuure päringu töökoormus stsenaariumid silmas pidada.

## <a name="scaling-azure-search-for-slow-individual-queries"></a>Azure'i skaleerimise otsimine aeglane üksikute päringud

Teine põhjus, miks latentsuse määr võib olla aeglane on ühe päringu võtab liiga kaua aega lõpuleviimiseks.  Sel juhul lisamise koopiad ei paranda latentsuse määr.  Sel juhul on kaks võimalust saadaval.

1. **Sektsioonid suurendamine** Sektsiooni on andmete jagamine kogu eest ressursid süsteem.  Seetõttu, kui lisate teise sektsiooni, andmete saab jagada kaheks.  Kolmanda sektsiooni jagab oma index kolme jne.  See on ka efekti, et mõnel juhul aeglane päringute toimivad kiiremini arvutus parallelization tõttu.  On esitatud mõned näited, kus oleme näinud selle parallelization töötamine väga hästi päringud, mis on madal selektiivsuse päringud.  See koosneb päringuid, mis vastavad mitme dokumendi või kui faceting vajab loendab suure hulga dokumentide üle anda.  Kuna seal on palju vajalik Keskmine asjakohasus dokumentide või dokumentide arvu loendamiseks arvutus, lisada täiendavat sektsioonid abil saab anda täiendavad arvutus.  

   Võib olla kuni 12 partitsioonid Standard otsinguteenuse ja 1 sektsiooni lihtotsinguavaldis teenus.  Sektsioonid saab kohandada, kas [Azure portaali](search-create-service-portal.md) või [Azure otsingu halduse API](search-get-started-management-api.md)kaudu.

2. **Piirata kõrge arvutite väljad:** Kõrge arvutite välja koosneb facetable või filtreeritavad välja, mis on palju kordumatuid väärtusi, ja seetõttu võtab hulgaliselt ressursse, mis arvutada tulemused üle.   Näiteks seadmine välja toote ID või kirjeldus facetable/filtreeritavad oleks kõrge arvutite Kuna enamik dokumendis olevad väärtused, et dokument on kordumatu. Võimaluse korral piirata kõrge arvutite väljade arv.

3. **Suurendamiseks otsingu taseme:**  Jooksev kuni Azure'i otsingu jõudmine võib olla teine võimalus aeglane päringuid jõudluse parandamiseks.  Iga kõrgema taseme pakub ka kiirem CPU ja rohkem mälu, mis võib olla positiivne mõju päringu jõudluse.

## <a name="scaling-for-availability"></a>Skaleerimist jaoks kättesaadavus

Koopiad vähendada päringulatentsus vaid ka kõrge kättesaadavus võimaldavad.  Ühe koopia, kus teil tuleb arvestada perioodiliste tööseisakute tõttu serveri taaskäivitamisega pärast tarkvaravärskendusi või muid hooldustöid sündmusi, mis võivad ilmneda.  Seetõttu on oluline silmas pidada, kui teie rakendus nõuab kättesaadavuse otsingud (päring) ning kirjutab (indekseerimise sündmused).  Azure'i otsing pakub makstud otsing pakkumisi ka järgmiste atribuutidega SLA suvandid:

- 2 koopiad kättesaadavuse kirjutuskaitstud teenustest (päring)
- 3 või rohkem koopiad kättesaadavuse lugemis-ja kirjutamisõigusega teenustest (päringute ja indekseerimine)

Lisateavet selle kohta, leiate [Azure'i otsingu teenuselepingu](https://azure.microsoft.com/support/legal/sla/search/v1_0/).

Kuna koopiad andmete koopiad, võttes mitme koopiad võimaldab Azure'i otsingu masina taaskäivitamisega ja hooldus vastu ühe koopia korraga, lubades päringute jätkamiseks muude koopiatega käivitada.  Seetõttu ka peate arvesse võtta, kuidas see tööseisakute võib mõjutada päringud, mis on nüüd vähem koopia andmed käivitada.

## <a name="scaling-geo-distributed-workloads-and-provide-geo-redundancy"></a>Skaleerimist geo jaotatud töökoormus ja sisestage geo-koondamine

Geo jaotatud töökoormus, leiate kasutajatel asuvate andmete personal, kus teie Azure'i otsinguteenuse majutatakse on kõrgem latentsus.  Seetõttu on sageli oluline on mitu otsingu teenuste regioonid, mis on need kasutajad lähemale.  Azure'i otsing ei ole praegu ette automatiseeritud meetodi geo imitatsiooniga Azure'i otsing registrite piirkondade lõikes, kuid on mõningaid viise, mida saab teha seda protsessi lihtne rakendada ja hallata. Need on esitatud mõned järgmistest jaotistest.

Otsingu teenuste kogumi geo jaotatud eesmärk on on saadaval kahe või enama registrid kahe või enama regioonid, kus kasutaja suunatakse Azure otsingu teenus, mis sisaldab madalam latentsus, nagu järgmises näites:

   ![Rist-menüü teenuste regiooniti][1]

### <a name="keeping-data-in-sync-across-multiple-azure-search-services"></a>Andmete otsing Azure'i mitmes teenuses sünkroonituna hoidmine

On kaks võimalust hoida oma jaotatud otsinguteenuste sünkroonis mis koosnevad kasutades kas [Azure'i otsingu indekseerija](search-indexer-overview.md) või Push API (nimetatakse ka [Azure Search REST API -ga](https://msdn.microsoft.com/library/dn798935.aspx)).  

### <a name="azure-search-indexers"></a>Azure'i otsingu Indexers

Kui kasutate Azure otsingu indekseerija, on juba andmemuudatuste keskse andmesalve, nt Azure SQL-i DB või DocumentDB importimine. Kui loote uue otsingu teenus, saate lihtsalt luua ka uue Azure otsingu indekseerija selle teenuse, mis viitab selle sama andmesalve. Iga kord, kui muudatusi tulevad andmesalve, nii need siis indekseeritud erinevate Indexers järgi.  

Siin on näide sellest, kuidas see arhitektuur näeks.

   ![Ühe andmeallika jaotatud indekseerija ja teenuse kombinatsioonid][2]


### <a name="push-api"></a>Tõuketeatised API 
Azure'i otsingu Push API värskendada [sisu Azure'i otsinguregistri](https://msdn.microsoft.com/library/dn798930.aspx)kasutamisel saate hoida otsing mitmesuguste sünkroonis lükates muudatused kõigi otsingu teenuste iga kord, kui värskendus on nõutav.  Kui see on oluline veendumaks, et mida teha juhul, kui ühe otsinguteenuse värskendus nurjub ja ühe või mitme värskenduste õnnestub.

## <a name="leveraging-azure-traffic-manager"></a>Azure'i liikluse halduri kasutamine

[Azure'i liikluse haldur](../traffic-manager/traffic-manager-overview.md) võimaldab marsruutimiseks taotlusi mitme geo asub veebisaite, mis on seejärel tagatud mitme Azure'i otsingu teenused.  Üks eeliseid liikluse haldur on, et see saab probe Azure'i otsingu kasutajad marsruutida alternatiivse teenuste korral tööseisakute ning veenduge, et see on saadaval.  Lisaks kui on marsruutimine otsing taotlused Azure veebisaitide kaudu, Azure'i liikluse haldur võimaldab saldo juhul, kui veebisaidi üles laadimist, kuid mitte Azure'i otsing.  Siin on näide sellest, millist arhitektuur varundustarkvara liikluse haldur.

   ![Rist-menüü teenuste regiooniti, keskse liikluse haldur][3]

## <a name="monitoring-performance"></a>Jõudluse jälgimine

Azure'i otsing pakub võimalus analüüsida ja jälgida oma teenust [Liikluse Analytics (STA)](search-traffic-analytics.md)kaudu. STA, kuni saate Azure Storage konto, mida saab siis analüüsida töödeldud või visualiseeritakse Power BI soovi korral logige isiklike toimingute kui ka liidetud mõõdikute.  STA mõõdikute saate vaadata tootlusstatistika, nt keskmine arv päringut või päringut vastuse korda.  Lisaks saate toiming logimine üldiseks teatud toimingute üksikasjad.

STA on väärtuslik vahend mõista latentsuse määr sellest Azure'i otsingu seisukohast.  Kuna päringu jõudluse mõõdikute logitud põhinevad täielikult töödelda Azure'i otsing (see on nõutav saatmise aeg) kaudu suunab päringu aeg, teil on võimalik selle abil saate määratleda, kas latentsus probleemid on Azure otsingu teenuse servast või väljaspool teenust, näiteks latentsus võrgu kaudu.  

## <a name="next-steps"></a>Järgmised sammud

Lisateavet hinnakirjad astme ja teenuste piirangud igale kirjele, leiate [Azure'i otsingu teenuse piirangud](search-limits-quotas-capacity.md).

Külastage [võimsuse planeerimine](search-capacity-planning.md) partition ja koopia kombinatsioonide kohta.

Veel süvitsiminek kohta ja näha, kuidas rakendada Optimeerimised, mida selles artiklis käsitletakse mõnda esitlused, vaadake järgmist videot:

> [AZURE.VIDEO azurecon-2015-azure-search-best-practices-for-web-and-mobile-applications]

<!--Image references-->
[1]: ./media/search-performance-optimization/geo-redundancy.png
[2]: ./media/search-performance-optimization/scale-indexers.png
[3]: ./media/search-performance-optimization/geo-search-traffic-mgr.png