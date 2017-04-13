<properties
    pageTitle="Azure'i välise teenuse kulude mõistmiseks | Microsoft Azure'i"
    description="Lisateavet väliste teenuste, turuplatsilt, varem kulude Azure arveldamine."
    services=""
    documentationCenter=""
    authors="adpick"
    manager="felixwu"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/12/2016"
    ms.author="adpick"/>

# <a name="understand-your-azure-external-service-charges"></a>Azure'i välise teenuse kulude mõistmiseks

Selles artiklis selgitatakse välisteenused Azure arveldamine. Välisteenused varem nimetati turuplatsi tellimusi. Välisteenused osutavad sõltumatu teenusepakkujate, kuid on täielikult integreeritud Azure ökosüsteemi. Siit saate teada, kuidas:

- Välisteenused tuvastamine
- Mõista, kuidas arveldamine erineb muude Azure ressursid
- Saate vaadata ja jälgida saate lisada kulud väliste teenuste kasutamine
- Välisteenuste tellimused ja kuidas maksta neid haldamine

## <a name="what-are-azure-external-services"></a>Mis on Azure välisteenused?

Välisteenused varem nimetati Azure'i turuplatsilt. Üldiselt nad avaldatud kolmandate osapoolte saadaval Azure teenused. Näiteks ClearDB ja SendGrid on välisteenused, mida te saate osta Azure, kuid pole Microsoft avaldatud.

### <a name="identify-external-services"></a>Välisteenused tuvastamine

Kui olete ette uue välisteenuste või ressurss, kuvatakse hoiatus:

![Turuplatsi osta hoiatus](./media/billing-understand-your-azure-marketplace-charges/marketplace-warning.PNG)

>[AZURE.NOTE] Ettevõtted, mis pole Microsoft on avaldatud välisteenused, kuid mõnikord Microsofti toodete ka kategooriasse välisteenused.

### <a name="external-services-are-billed-separately"></a>Välisteenused on eraldi arve

Välisteenused käsitletakse iga tellimuse Azure tellimuse. Arveldusperioodi eest iga teenuse jaoks on määratud teenuse ostmisel. Mitte segi arveldusperioodi eest, mille ostsite tellimuse. Saate ka eraldi arved ja teie krediitkaardilt on eraldi.

### <a name="each-external-service-has-a-different-billing-model"></a>Iga välisteenuste on erinev arvelduse mudel

Teatud teenuste on pensionitingimustega mood arve, kuigi teised kasutada kuu alusel maksmine mudel. Teil on vaja krediitkaarti Azure välisteenused, ei saa osta välise arve maksta teenuste.

### <a name="you-cant-use-monthly-free-credits-for-external-services"></a>Te ei saa kasutada kuu tasuta autorid välisteenused

Kui kasutate Azure'i tellimus, mis sisaldab [tasuta krediiti](https://azure.microsoft.com/pricing/spending-limits/), nad ei saa rakendada välisteenuste arved. Krediitkaardi abil välise teenuste ostmine.

## <a name="view-external-service-spending-and-history"></a>Vaate välisteenuste kulutuste ja ajalugu

Saate vaadata loendit välisteenused, mis on iga tellimuse [Azure portaali](https://portal.azure.com/): 

1. Logige sisse [Azure portaali](https://portal.azure.com/) ja [liikuge tera **arveldus** ](https://portal.azure.com/?flight=1#blade/Microsoft_Azure_Billing/BillingBlade).

    ![Valige Arveldamine jaoturi menüü](./media/billing-understand-your-azure-marketplace-charges/billing-button.png) 
  
2. Valige jaotises **Tellimuse maksumus** tellimus, mida soovite vaadata. 
   
    ![Valige tellimuse arvelduse tera](./media/billing-understand-your-azure-marketplace-charges/select-sub.png)

3. Klõpsake **välisteenused**.

    ![Klõpsake tellimuse tera välisteenused](./media/billing-understand-your-azure-marketplace-charges/external-service-blade.png)

4. Peaksite iga välisteenuste tellimuste, publisher nimi, teenuse kiht ostsite, andsite ressurss ja praeguse tellimuse oleku nimi. Valige teenust näha minevikus arved.

    ![Välise teenuse valimine](./media/billing-understand-your-azure-marketplace-charges/external-service-blade2.png)

5. Siit saate vaadata viimase maksuvabastuse jaotus arve summad.

    ![Vaate välisteenused arveldusajalugu.](./media/billing-understand-your-azure-marketplace-charges/billing-overview-blade.png)

## <a name="manage-payment-methods-for-external-service-orders"></a>Välisteenuste tellimuste makseviise hallata

Värskendage oma makseviise välisteenuste tellimusi [Keskuse konto](https://account.windowsazure.com/).

> [AZURE.NOTE] Kui ostsite tellimuse töö või kooli kontoga peaksite [tugiteenuste](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) muutma makseviisi.

1. Logige sisse [Konto Center](https://account.windowsazure.com/) ja [liikuge menüü **turuplats** ](https://account.windowsazure.com/Store)

    ![Valige konto keskuses turuplats](./media/billing-understand-your-azure-marketplace-charges/select-marketplace.png)

2. Valige välise teenus, mida soovite hallata

    ![Valige välise teenus, mida soovite hallata](./media/billing-understand-your-azure-marketplace-charges/select-ext-service.png)

3. Klõpsake lehe paremas servas nuppu **Muuda makseviisi** . Järgmine link viib teid erinevate portaali haldamiseks makseviisi.
    
    ![Tellimuse Kokkuvõte](./media/billing-understand-your-azure-marketplace-charges/change-payment.PNG)

4. Klõpsake nuppu **Redigeeri teave** ja järgige juhiseid, et värskendada oma makseteave.

    ![Valige suvand Redigeeri teave](./media/billing-understand-your-azure-marketplace-charges/edit-info.png)
    
## <a name="cancel-an-external-service-order"></a>Välisteenuste tellimuse tühistamine

Kui soovite välisteenuste tellimuse tühistada, peate kustutama ressursi [Azure portaali](https://portal.azure.com).

![Ressursi kustutamine](./media/billing-understand-your-azure-marketplace-charges/deleteMarketplaceOrder.PNG)

## <a name="need-help-contact-support"></a>Kas vajate abi? Pöörduge klienditoe poole.

Kui teil siiski on küsimusi, võtke [tugiteenuste](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) saamiseks probleemi lahendada kiiresti.
