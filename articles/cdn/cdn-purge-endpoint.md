<properties
    pageTitle="Azure'i CDN lõpp likvideerite | Microsoft Azure'i"
    description="Saate teada, kuidas likvideerite kõik vahemälus talletatud sisu CDN lõpp."
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

# <a name="purge-an-azure-cdn-endpoint"></a>Likvideerite Azure'i CDN lõpp

## <a name="overview"></a>Ülevaade

Azure'i CDN serva sõlmed vahemälu varad kuni vara aja live (TTL) aegub.  Pärast vara TTL aegumist, kui klient taotleb vara sõlme serva, sõlme serva tõmmata värskendatud koopia varade teenida kliendi taotlus ja poe vahemälu värskendamine.

Vahel võite likvideerite vahemällu talletatud sisu: kõik serva sõlmed ja sundige neid kõigi uute värskendatud varasid tuua.  See võib olla tingitud värskendused oma veebirakenduse või kiiresti update varad, mis sisaldavad.

> [AZURE.TIP] Pange tähele, et puhastamine kustutab ainult CDN Edge'i serverid vahemällu talletatud sisu.  Mis tahes järgmise etapi vahemälu, nt puhverserverid ja kohalik brauseri vahemälu, võib olla veel vahemällu salvestatud faili koopia.  See on oluline meeles pidada seda, kui seate faili aja live.  Saate jõustada järgmise etapi kliendi taotleda faili uusim versioon, andes sellele kordumatu nimi iga kord, kui värskendate see või ära [päringu stringi vahemällu](cdn-query-string.md).  

Selles õpetuses juhendab teid puhastamine varad: kõik serva sõlmed lõpp.

## <a name="walkthrough"></a>Kiirtutvustus

1. Sirvige [Azure portaali](https://portal.azure.com)CDN profiili, mis sisaldab lõpp-punkti, mida soovite puhastada.

2. Keelest CDN profiil nuppu puhastada.

    ![CDN profiili blade](./media/cdn-purge-endpoint/cdn-profile-blade.png)

    Puhastada tera avaneb.

    ![CDN puhastada blade](./media/cdn-purge-endpoint/cdn-purge-blade.png)

3. Enne puhastada, valige teenuse aadress, mida soovite puhastada rippmenüüst URL-i.

    ![Likvideerite vorm](./media/cdn-purge-endpoint/cdn-purge-form.png)

    > [AZURE.NOTE] Samuti saate avada puhastada tera **likvideerite** CDN lõpp-punkti enne nupu klõpsamisega.  Sel juhul **URL-i** välja on juba eelnevalt täidetud teenuse selle kindla lõpp-punkti aadress.

4. Valige mida soovite puhastada kaudu serva sõlmed varad.  Kui soovite eemaldada kõik varad, märkige ruut **Kustuta kõik** .  Vastasel korral tippige täielik tee iga vara, mida soovite puhastada (nt `/pictures/kitten.png`) väljale **tee** .

    > [AZURE.TIP] Lisateavet **tee** tekstiväljad kuvatakse pärast seda, kui sisestate teksti abil saate luua mitme varade nimekirja.  Saate kustutada varad loendist, klõpsates nuppu kolmikpunkti (…).
    >
    > Tuleb teed suhteline URL, mis sobivad järgmine [Lihtavaldise](https://msdn.microsoft.com/library/az24scfc.aspx): `^\/(?:[a-zA-Z0-9-_.\u0020]+\/)*\*$";`.  Jaoks **Azure'i CDN Verizon** (Standard ja Premium), tärn (\*) võib kasutada metamärke (nt `/music/*`).  Metamärkide ning **Kõik likvideerite** ei ole lubatud **Azure CDN Akamai kaudu**.
    
5. Klõpsake nuppu **Kustuta** .

    ![Nupp Kustuta](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [AZURE.IMPORTANT] Puhastada päringutele võtta umbes 2 – 3 minutit protsessi **Azure'i CDN Verizon** (Standard- ja Premium) ja ligikaudu 7 minutit **Azure'i CDN Akamai kaudu**.  Azure'i CDN on piiratud 50 samaaegseid taotlusi likvideerite igal ajal. 

## <a name="see-also"></a>Vt ka
- [Azure'i CDN lõpp-punkti eelse laadi varad](cdn-preload-endpoint.md)
- [Azure'i CDN REST API - likvideerite või eel-laadida lõpp](https://msdn.microsoft.com/library/mt634451.aspx)
