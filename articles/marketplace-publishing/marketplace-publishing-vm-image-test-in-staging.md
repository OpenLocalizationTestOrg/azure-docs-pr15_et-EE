<properties
   pageTitle="Testige oma VM pakkumine turuplatsil | Microsoft Azure'i"
   description="Mõista, kuidas testida Azure'i turuplatsil VM pilt."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/01/2016"
   ms.author="hascipio" />

# <a name="test-your-vm-offer-for-the-azure-marketplace-in-staging"></a>Testige oma VM pakkumine Azure'i turuplatsi lavastus

Lavastus tähendab, et teie SKU omaette "Liivakasti", kus saate testida ja kinnitage oma funktsionaalsuse enne juurutamist see kuvatakse Marketplace'ist juurutamine. Kasutatava SKU kuvatakse lavastus, nagu oleks kliendile, kes on kasutusele võetud. VM pilt peab olema sertifitseeritud lükata lavastus.

## <a name="step-1-push-your-offer-to-staging"></a>Samm 1: Push lavastus oma pakkumine

1. Klõpsake menüü **Avalda** **lavastus lükata**.

    ![Joonis](media/marketplace-publishing-vm-image-test-in-staging/vm-image-push-to-staging.png)

2. Kui soovitud Avaldamisportaal annab märku vigu, parandada.
3.  Klõpsake on **kellel on juurdepääs teie etapiviisilise pakkumise?** Azure kasutate eelvaate teie pakkumise [Azure eelvaade portaalis](https://portal.azure.com)tellimuste loendi sisestage dialoogiboksis.

    >[AZURE.NOTE] Virtuaalmasinates ja lahenduse korral Mallid, võtke DreamSpark või Ava Azure type CSP, **ei ole** nimekiri tellimused.


    > Korral Virtuaalmasinates, kui klõpsate nuppu **PUSH LAVASTUS**, tehakse järgmist stseeni taha. On võimalik, iga toimingu menüü AVALDA edenemise kuvamiseks klõpsake jaotises avaldamine portaalis. Kontrollige selle lehe regulaarse intervalliga (kuni olek on kuvatud etapiviisiline) tõrge teavet, mis on vaja paranduse oma lõppu.

    > - Esmalt lavastus kutse läheb sertimine meeskonna valideerimiseks on vhd. Juhul, kui teie taotlus on ainult neid muuta, siis sertimine juhise vahele.
    > - Kui kinnitamine on lõpule jõudnud, dispersioonanalüüs pakkumise alustada üle kõik Azure andmekeskuste. Üldiselt 24 – 48 tundi kopeerimise lõpuleviimiseks kulub aga võib kuluda kuni nädal soovitud vhd suurusest. Kui teie taotlus on ainult neid muuta, siis on dispersioonanalüüs aga kiirem.
    > - Kui soovitud kopeerimine on lõpule viidud, siis pakkumine on [Azure portaali](http:/portal.azure.com). Klõpsake jaotises avaldamine muutuvad ETAPIVIISILISE sel ajal oleku portaalis. Etapiviisilise pakkumine on nähtav ainult abil e-posti ID, mis on seotud tellimus, mis on etapiviisilise pakkumine [Azure portaali](http:/portal.azure.com) .

4. Logige sisse [Azure'i eelvaate portaalis](https://portal.azure.com) , kasutades ühte järgmistest loetletud eelmises etapis Azure'i tellimused.
5. Teie pakkumise otsimine ja kinnitage oma VM pilt punktid.
  - Veenduge, et turundus sisu kuvatakse õigesti kuvatakse Marketplace'ist.
  - Juurutamise VM pilt-lõpuni.

      ![img-MAPI-portaal](media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg)

> [AZURE.IMPORTANT] Teie pakkumise jääb lavastus seni, kuni te teavitamise Microsoft avaldamise portaali kaudu [menüü**Avalda** > nuppu **"taotlemine kinnitamise abil tõuketeatised abil loomine"**], et olete valmis push tootmisele. See on hea aeg olema üle kõik teie pakkumise läheb loetletud ettevalmistamiseks kõikidele liikmetele oma meeskonna sisse.

> Lavastus platvormi eesmärk testimiseks pakkumine eelvaate režiimis väljaandja. Soovitame tungivalt takistada selle platofrm äri kasutamisega.

## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui teie pakkumise on "etapiviisilise" ja selle funktsionaalsus ja turundus sisu testinud, jätkake avaldamise viimases etapis **Samm 4**: [juurutamine teie pakkumise kuvatakse Marketplace'ist](marketplace-publishing-push-to-production.md).

## <a name="see-also"></a>Vt ka
- [Alustamine: Azure'i turuplatsi avaldamine pakkumise kohta](marketplace-publishing-getting-started.md)
