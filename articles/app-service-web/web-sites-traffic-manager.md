<properties
    pageTitle="Azure'i web appi liikluse Azure'i liikluse haldur juhtimine"
    description="Selles artiklis on toodud koondteave Azure liikluse Manager, mis puudutab Azure veebirakenduste."
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    writer="cephalin"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/25/2016"
    ms.author="cephalin"/>

# <a name="controlling-azure-web-app-traffic-with-azure-traffic-manager"></a>Azure'i web appi liikluse Azure'i liikluse haldur juhtimine

> [AZURE.NOTE] Selles artiklis on toodud koondteave Microsoft Azure'i liikluse haldur, mis puudutab Azure'i rakenduse teenuse Web Apps. Lisateabe saamiseks Azure'i liikluse halduri ise leiate käesoleva artikli lõpus linkide külastades.

## <a name="introduction"></a>Sissejuhatus
Saate määrata, kuidas web klientide päringutele jagatakse veebirakenduste Azure'i rakenduse teenuses Azure liikluse haldur. Web appi lõpp-punktid lisamisel Azure'i liikluse haldur profiilile Azure'i liikluse haldur jälgib, oleku oma veebirakenduste (töötab, peatatud või kustutatud) nii, et saate otsustada, millist nende lõpp-punktid saab liikluse.

## <a name="load-balancing-methods"></a>Laadi tasakaalustavad meetodid
Azure'i liikluse haldur kasutab kolme eri laadi tasakaalustavad meetodit. Need on kirjeldatud järgmises loendis need puudutavad Azure veebirakenduste.

* **Tõrkesiirde**: kui teil on web appi kloonid erinevate piirkondade, saate selle meetodi abil saate konfigureerida ühe veebirakenduse teenuse kõik web kliendi liikluse ja teise web appi konfigureerimine teises regioonis, et liiklus teenuse juhuks, kui esimene veebirakenduse kättesaamatu.

* **Round jaan**: kui teil on web appi kloonid erinevate piirkondade, saate selle meetodi abil jaotada liikluse võrdselt kogu veebirakenduste eri piirkondades.

* **Jõudluse**: The jõudluse meetod jaotab liikluse lühima pendellevi aja klientidele. Jõudluse meetodit saab kasutada veebirakenduste samas piirkonnas või eri piirkondadest.

##<a name="web-apps-and-traffic-manager-profiles"></a>Veebirakenduste ja liikluse haldur profiilid
Web appi liikluse juhtelemendi konfigureerimiseks Azure'i liikluse halduris kasutab üks kolmest koormuse tasakaalustavad eelnevalt kirjeldatud meetodite profiili loomine ja seejärel lisage lõpp-punktid (sel juhul veebirakenduste) jaoks, mida soovite reguleerida profiili-liikluse. Web appi oleku (töötab, on peatatud või kustutatud) regulaarselt edastada profiili nii, et Azure'i liikluse haldur saab suunaks liikluse vastavalt sellele.

Azure'i liikluse haldur kasutamisel Azure pidage meeles järgmist:

* Web app ainult juurutuste samasse piirkonda, veebirakenduste pakub juba Tõrkesiirde ja round-jaan funktsiooni arvestamata web app režiim.

* Kasutuselevõttu piirkonna, mis Web Apps kasutamine koos mõne muu Azure pilveteenuses, saab kombineerida mõlemat tüüpi lubamiseks hübriid stsenaariumid lõpp-punktid.

* Profiili saate määrata ainult ühe web appi lõpp-punkti müüginäitajaid piirkonna kohta. Kui valite web appi lõpp ühe piirkonna, selle piirkonna ülejäänud veebirakenduste seda profiili valimise kättesaamatuks muutuda.

* Web appi lõpp-punktid Azure'i liikluse haldur profiili määratud kuvatakse jaotises **Domeeninimede** konfigureerimine veebirakenduse profiili lehel, kuid ei saa seal konfigureeritav.

* Pärast seda, kui lisate web appi profiili, kuvatakse **Saidi URL-i** web appi portaalilehele armatuurlaual kohandatud domeeni URL-i veebirakenduse kui olete loonud ühte. Muul juhul kuvatakse liikluse haldur profiili URL (nt `contoso.trafficmgr.com`). Nii otsese domeeni nimi ja veebirakenduse liikluse haldur URL on nähtav web appi konfigureerimine lehel jaotises **Domain Names** .

* Oma kohandatud domeeni nimed ei tööta oodatud viisil, kuid lisaks lisamine oma veebirakenduste, tuleb konfigureerida ka liikluse haldur URL-i osutamiseks teie DNS-i kaarti. Azure'i veebiversiooni kohandatud domeeni häälestamise kohta leiate teavet teemast [konfigureerimine on Azure veebisaidi jaoks kohandatud domeeninime](web-sites-custom-domain-name.md).

* Saate lisada ainult veebirakenduste, mis on Azure liikluse haldur profiilile standardrežiimis.

## <a name="next-steps"></a>Järgmised sammud

Kontseptuaalne ja tehnilise ülevaate Azure'i liikluse haldur, leiate teemast [Liikluse haldur ülevaade](../traffic-manager/traffic-manager-overview.md).

Veebirakenduste liikluse halduri kasutamise kohta leiate lisateavet teemast postitused [Abil Azure'i liikluse haldur Azure veebisaitide](http://blogs.msdn.com/b/waws/archive/2014/03/18/using-windows-azure-traffic-manager-with-waws.aspx) ja [Azure liikluse Manager saate nüüd integreerida Azure veebisaitide](https://azure.microsoft.com/blog/2014/03/27/azure-traffic-manager-can-now-integrate-with-azure-web-sites/).
