<properties
   pageTitle="Lõpp-punktid Azure'i liikluse halduris haldamine | Microsoft Azure'i"
   description="See artikkel aitab teil lisamine, eemaldamine, lubamine ja keelamine lõpp-punktid: Azure'i liikluse haldur."
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/17/2016"
   ms.author="sewhee" />

# <a name="add-disable-enable-or-delete-endpoints"></a>Lisamine, keelata, lubada või kustutada lõpp-punktid

Funktsiooni veebirakenduste Azure'i rakendust Service pakub juba Tõrkesiirde ja round-jaan liikluse marsruutimise funktsioonid veebilehed andmekeskuses, olenemata veebisaidi sees. Azure'i liikluse haldur võimaldab teil määrata Tõrkesiirde ja round-jaan liikluse marsruutimine eri andmekeskuste veebisaitide ja pilveteenuse teenuste jaoks. Kõigepealt tuleb pakkuda funktsionaalsust, et on lisada pilvepõhise teenuse või veebisaidi lõpp-punkti liikluse haldur.

>[AZURE.NOTE] Ei saa lisada välise asukohtadele või liikluse haldur profiilid nimega Azure klassikaline portaalis lõpp-punktid. Kasutage REST API [Loomine määratlus](http://go.microsoft.com/fwlink/p/?LinkId=400772) või Windows PowerShelli [Lisa-AzureTrafficManagerEndpoint](http://go.microsoft.com/fwlink/p/?LinkId=400774).

Samuti saate keelata üksikute lõpp-punktid, mis on osa liikluse haldur profiili. Lõpp-punktid kaasata nii pilveteenustega ja veebisaidid. Lõpp-punkti keelamine jätab profiili osana, kuid profiili toimib kui lõpp-punkti ei sisalda seda. See toiming on väga kasulik ajutiselt eemaldamine on hooldus režiimis või kogurahastus lõpp-punkt. Kui lõpp-punkti on uuesti ja töötab, saate selle lubatud.

>[AZURE.NOTE] Lõpp-punkti keelamine ei ole midagi Azure juurutamise olekuga. Terve lõpp-punkti jäävad üles ja saada liikluse isegi siis, kui keelatud liikluse haldur. Lisaks keelamine lõpp üks profiil ei mõjuta selle mõne muu profiili olek.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>Pilveteenuse teenuse või veebisaidi endpoint lisamine


1. Azure'i klassikaline portaalis liikluse haldur paanil üles liikluse haldur profiili, mis sisaldab lõpp-punkti sätteid, mida soovite muuta, ja seejärel klõpsake soovitud profiili nimi paremal olevat noolt. See avab profiili lehel sätted.
2. Klõpsake lehe ülaosas nuppu **lõpp-punktid** lõpp-punktid, mis on juba teie konfiguratsiooni osa kuvamiseks.
3. Klõpsake lehe allosas nuppu **Lisa** lehele **Lisada teenuse lõpp-punktid** . Vaikimisi on loetletud lehe pilveteenuseid jaotises **Teenuse lõpp-punktid**.
4. Pilveteenustega, valige pilveteenuseid loendis saaksid nimega selle reegli lõpp-punktid. Pilvepõhise teenuse nimi tühjendada eemaldab selle loendi lõpp-punktid.
5. Veebisaitide jaoks, valige ripploendist **Teenuse tüüp** ja valige **Web app**.
6. Valige veebisaitide loendis kuuluva selle reegli lõpp-punktid. Veebisaidi nime tühjendada eemaldab selle loendi lõpp-punktid. Pange tähele, et saate valida ainult ühe veebisaidi Azure andmekeskuse (tuntud ka kui piirkond) kohta. Kui valite veebisaidi andmekeskuses, mis hostib mitme veebisaidi, kui valite esimese veebisaidi, muutuvad teiste sama andmekeskuses valiku jaoks saadaval. Samuti võtke arvesse, et ainult Standard veebilehtede on loetletud.
7. Pärast seda profiili lõpp-punktide valimiseks klõpsake parempoolses allnurgas muudatuste salvestamiseks klõpsake märke.

>[AZURE.NOTE] Kui kasutate *Tõrkesiirde* liikluse marsruutimise meetod, kui lisate või eemaldate lõpp kindlasti Tõrkesiirde loendis prioriteet lehel konfiguratsioon kajastamiseks Tõrkesiirde järjestuses, milles soovite konfiguratsioonist reguleerimine. Lisateavet leiate teemast [Tõrkesiirde konfigureerimine liikluse marsruutimist](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>Lõpp-punkti keelamine

1. Azure'i klassikaline portaalis liikluse haldur paanil üles liikluse haldur profiili, mis sisaldab lõpp-punkti sätteid, mida soovite muuta, ja seejärel klõpsake soovitud profiili nimi paremal olevat noolt. See avab profiili lehel sätted.
2. Klõpsake lehe ülaosas nuppu **lõpp-punktid** lõpp-punktid, mis kuuluvad teie konfiguratsiooni kuvamiseks.
3. Klõpsake nuppu lõpp-punkti, mida soovite keelata, ja klõpsake lehe allosas **Keela** .
4. Liikluse katkestab minevaid vastavalt selle DNS-i aeg-to-Live (TTL) domeeninimi halduri liikluse jaoks konfigureeritud lõpp-punkti. Saate muuta TTL liikluse haldur profiili lehel konfigureerimine.

## <a name="to-enable-an-endpoint"></a>Kui soovite lubada lõpp

1. Azure'i klassikaline portaalis liikluse haldur paanil üles liikluse haldur profiili, mis sisaldab lõpp-punkti sätteid, mida soovite muuta, ja seejärel klõpsake soovitud profiili nimi paremal olevat noolt. See avab profiili lehel sätted.
2. Klõpsake lehe ülaosas nuppu **lõpp-punktid** lõpp-punktid, mis kuuluvad teie konfiguratsiooni kuvamiseks.
3. Klõpsake nuppu lõpp-punkti, mida soovite lubada, ja seejärel käsku **Luba** lehe allosas.
4. Teenusega uuesti, mille määrab profiili rakendamist alustatakse liikluse.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>Pilveteenuse teenuse või veebisaidi endpoint kustutamine


1. Azure'i klassikaline portaalis liikluse haldur paanil üles liikluse haldur profiili, mis sisaldab lõpp-punkti sätteid, mida soovite muuta, ja seejärel klõpsake soovitud profiili nimi paremal olevat noolt. See avab profiili lehel sätted.
2. Klõpsake lehe ülaosas nuppu **lõpp-punktid** lõpp-punktid, mis on juba teie konfiguratsiooni osa kuvamiseks.
3. Klõpsake lehel lõpp-punktid lõpp-punkti, mida soovite kustutada profiili nimi.
4. Klõpsake lehe allosas nuppu **Kustuta**.

>[AZURE.NOTE] Ei saa kustutada välise asukohtadele või liikluse haldur profiilid nimega Azure klassikaline portaalis lõpp-punktid. Kasutage Windows PowerShelli. Lisateavet leiate teemast [Eemalda-AzureTrafficManagerEndpoint](https://msdn.microsoft.com/library/dn690251.aspx).

## <a name="next-steps"></a>Järgmised sammud

- [Tõrkesiirde marsruutimise meetod konfigureerimine](traffic-manager-configure-failover-routing-method.md)
- [Round jaan marsruutimise meetod konfigureerimine](traffic-manager-configure-round-robin-routing-method.md)
- [Jõudluse marsruutimise meetod konfigureerimine](traffic-manager-configure-performance-routing-method.md)
- [Tõrkeotsingu liikluse haldur halvenenud olek](traffic-manager-troubleshooting-degraded.md)
- [Toimingute kohta liikluse Manager (REST API teatmematerjalid)](http://go.microsoft.com/fwlink/p/?LinkID=313584)
