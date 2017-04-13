<properties
   pageTitle="Azure'i turuplatsi väljamakse aruandlus mõista | Microsoft Azure'i"
   description="Saate teada, kuidas läbi vaadata ja neelata Azure'i turuplatsi väljamakse aruanne."
   services="marketplace-publishing"
   documentationCenter="na"
   authors="v-jeana"
   manager="lakoch"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/19/2016"
   ms.author="v-jeana; hascipio; v-dabosl"/>

# <a name="understand-your-azure-marketplace-payout-reports"></a>Azure'i turuplatsi väljamakse aruannete mõistmine

## <a name="access-and-view-your-payout-reports"></a>Juurdepääs ja väljamakse aruannete kuvamine

Kuigi me üleminek Arenduskeskus mõni väljamakse aruanne võib olla saadaval Arenduskeskus, veebisaidil https://dev.windows.com/en-us ajal teistega endiselt aadressil avaldamise portaalis https://publish.windowsazure.com.

Väljamakse aruandlus nüüd on saadaval **Arenduskeskus** jaoks mis tahes turuplatsi pakkumised, mis on seotud tänapäevane makseid; See hõlmab praegu:
- VMs
- Pakub B + C
- Andmed ja arendaja teenuseid pakkuda EA

Väljamakse aruandlus ikkagi **Avaldamisportaal** jaoks:
- Andmed ja arendaja teenuseid pakkuda Web otsese (mis kasutab pärand väljamakse süsteem).

Aruanded on saadaval 45 päeva pärast kvartali lõpu ja mis tahes toetused arvutamisel.

### <a name="access-payout-reports-in-dev-center"></a>Accessi aruannete väljamakse Arenduskeskus

1. Liikuge Arenduskeskus https://dev.windows.com/en-us juures.
2. Valige **armatuurlaud**.

    ![LandingPageDashboardHighlight][1]

3. Klõpsake nuppu **väljamakse Kokkuvõte**.

    ![DashboardPayoutSummary][2]


## <a name="view-your-payout-reports-in-dev-center"></a>Arenduskeskus väljamakse aruannete kuvamine

Iga kvartali väljamakse aruande kirjete selle kvartali toimunud tehingud.

- Reserveeritud summa viitab mis tahes maksete, mis on eelseisvad makse tsükkel (nt summa liigub eelseisvad makse järgmise kuu) väljaspool, mis pärinevad.  See summa on tavaliselt 0 (välja arvatud kliendi maksab aegsasti).
- Klõpsake eelseisvad makse või viimase makse **üksikasjade kuvamine** linkide kuvamiseks nende makseid Märkus.
- Klõpsake jaotises tulu üksikasjade vaatamiseks rakenduse/toote **Makse laused** .
- **Vaatelingi üksikud laused kuvamiseks** klõpsake.

    ![PayoutSummaryUpcomingMostRecentLinksStatement][3]

- **Tulu jaotus** filtri abil üksikute lause allosas vaadata tooted rakendused, kui need on olemas.

    ![PayoutSummaryPaymentStatementsFilterControl][4]



## <a name="view-your-payout-reports-in-publishing-portal"></a>Avaldamisportaal väljamakse aruannete kuvamine
Iga kvartali väljamakse aruande kirjete selle kvartali toimunud tehingud.

1. Liikuge avaldamisportaal https://publish.windowsazure.com juures.
2. Klõpsake jaotises **tootjad** **Väljamakse aruandeid**.
3. Klõpsake ripploendis kuvada kõik saadaolevad kvartali väljamakse aruandeid.

    ![accessingpayoutreport][5]


### <a name="read-your-payout-reports"></a>Lugege oma väljamakse aruanded

Iga kvartali väljamakse aruande kirjete selle kvartali toimunud tehingud.

- Kui otsite Pearaamat kirjed, mis on seotud konkreetse kvartali, valige ripploendist soovitud selle kvartali väljamakse aruanne. Näiteks kui olete huvitatud Pearaamat kirjed aprill 2015 juuni, valige ripploendist soovitud kuupäeva vahemiku.
- Kui otsite Lisateavet reeglite, mis on seotud konkreetse kvartali, valige väljamakse aruande järgmises kvartalis. Näiteks kui olete huvitatud selle makseid aprillis 2015 juuni, nende summade ilmub järgmise väljamakse aruande juuli mai 2015.
![readingpayoutreport][6]

- Rahandus Kokkuvõte paani kuvatakse saldo, krediidi ja eraldatud kategooria järgi.
- Pearaamat kirjete kuvamine üksiktehingute.

## <a name="definitions"></a>Määratlused

**Rahandus Kokkuvõte paneeli:**

![financialdefinitions][7]

**Pearaamat kirjed:**

![ledgerdefinitions][8]

## <a name="payout-questions"></a>Väljamakse küsimused

Kui teil on seotud teie maksed küsimusi, pöörduge meie tugimeeskond.

![payoutquestions][9]

1. Liikuge tugiteenuseid.
2. Valige **maksed**.
3. Valige **väljamakse seotud päringud**.
4. Klõpsake **taotluse**.

## <a name="next-steps"></a>Järgmised sammud

Muud tugiteenuste päringuid, logige veebisaidil <https://portal.azure.com>probleemi.

[1]: ./media/marketplace-publishing-report-payout/LandingPage-DashboardHighlight.png
[2]: ./media/marketplace-publishing-report-payout/Dashboard-PayoutSummary.png
[3]: ./media/marketplace-publishing-report-payout/PayoutSummary-UpcomingOrMostRecentPaymentLinksSingleStatementLink.png
[4]: ./media/marketplace-publishing-report-payout/PayoutSummary-PaymentStatements-SingleStatement-FilterControl.png
[5]: ./media/marketplace-publishing-report-payout/accessingpayoutreport.png
[6]: ./media/marketplace-publishing-report-payout/readingpayoutreport.png
[7]: ./media/marketplace-publishing-report-payout/financialdefinitions.png
[8]: ./media/marketplace-publishing-report-payout/ledgerdefinitions.png
[9]: ./media/marketplace-publishing-report-payout/payoutquestions.png
