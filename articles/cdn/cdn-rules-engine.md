<properties
    pageTitle="Alistamine HTTP vaikekäitumise Azure'i CDN reeglid mootori abil | Microsoft Azure'i"
    description="Reeglite engine võimaldab teil kohandada, kuidas HTTP päringuid käsitletakse Azure'i CDN-ID, nt blokeerida selle kohaletoimetamise teatud tüüpi sisu, määratleda vahemällu poliitika ja muutke HTTP päised."
    services="cdn"
    documentationCenter=""
    authors="camsoper"
    manager="erikre"
    editor=""/>

<tags
    ms.service="cdn"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/28/2016"
    ms.author="casoper"/>

# <a name="override-default-http-behavior-using-the-rules-engine"></a>Alistada vaikekäitumist HTTP reeglid mootori abil

[AZURE.INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Ülevaade

Reeglite engine võimaldab teil kohandada, kuidas käsitletakse HTTP päringuid, nt blokeerida selle kohaletoimetamise teatud tüüpi sisu, vahemällu poliitikast ja muutmine HTTP päised.  Selle õpetuse näitab luua reegli, mis muudab CDN varad vahemällu toimimist.  On ka video sisu saadaval jaotises ",[Vt ka](#see-also)".

## <a name="tutorial"></a>Õppeteema

1. Keelest CDN profiil nuppu **Halda** .

    ![CDN profiili blade haldamise nupp](./media/cdn-rules-engine/cdn-manage-btn.png)

    CDN haldusportaali avatakse.

2. Klõpsake vahekaardil **HTTP suur** , millele järgneb **Reeglid Engine**.

    Kuvatakse suvandid jaoks uus reegel.

    ![CDN uue reegli suvandid](./media/cdn-rules-engine/cdn-new-rule.png)

    >[AZURE.IMPORTANT] Järjestuses, kus on loetletud mitu reeglid mõjutab, kuidas nad käsitletakse. Järgmise reegli võib alistada eelmise reegli määratud toimingud.
    
3. Sisestage soovitud nime soovitud **nimi ja kirjeldus** tekstiväli.

4. Millist liiki taotlusi rakendatakse reegel.  **Alati** match tingimus on vaikimisi märgitud.  Saate kasutada **alati** selles õpetuses, nii jäta valitud.

    ![CDN vastavad tingimusele](./media/cdn-rules-engine/cdn-request-type.png)

    >[AZURE.TIP] Mitut tüüpi match on saadaval rippmenüüst tingimused.  Tingimuse vaste vasakul sinine teavitamise ikooni klõpsamisel selgitab üksikasjalikult praegu valitud tingimus.
    >
    >Match tingimused üksikasjalikult täieliku loendi leiate teemast [reeglid Engine vastavad tingimusele ja funktsiooni üksikasjad](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_0).

5.  Klõpsake soovitud **+** **funktsioonid** lisada uue funktsiooni kõrval nuppu.  Valige rippmenüüst vasakul **Jõusta sisemine Max vanus**.  Sisestage tekstiväljale, mis kuvatakse **300**.  Jätke ülejäänud vaikeväärtused.

    ![Funktsioon CDN-ID](./media/cdn-rules-engine/cdn-new-feature.png)

    >[AZURE.NOTE] Nimega match tingimusega, klõpsake sinisel teavitamise ikooni vasakul uue funktsiooni kuvatakse selle funktsiooni kohta.  **Jõusta sisemine Max vanus**me on alistamine vara **Olla** ja **aegub** päised juhtelemendile, kui CDN serva sõlm värskendab varade päritolu kaudu.  Näiteks 300 sekundi tähendab, et CDN serva sõlm vahemälu vara 5 minutit enne värskendamist varade oma päritolu.
    >
    >Funktsioonide üksikasjalik täieliku loendi leiate teemast [reeglid mootori Otsi tingimus ja funktsiooni üksikasjad](https://msdn.microsoft.com/library/mt757336.aspx#Anchor_1).

6.  Uue reegli salvestamiseks nuppu **Lisa** .  Uus reegel on nüüd kinnitamise ootel. Kui see on kinnitatud, muutub olekuks **Aktiivse**XML- **Ootel XML-** ist.

    >[AZURE.IMPORTANT] Reeglite muudatused võib kuluda kuni soovitud CDN levitada 90 minutit.

## <a name="see-also"></a>Vt ka
* [Azure reedeni: Azure'i CDN võimsaid Premium funktsioone](https://azure.microsoft.com/documentation/videos/azure-cdns-powerful-new-premium-features/) (video)
* [Reeglite mootori Otsi tingimus ja funktsiooni üksikasjad](https://msdn.microsoft.com/library/mt757336.aspx)
