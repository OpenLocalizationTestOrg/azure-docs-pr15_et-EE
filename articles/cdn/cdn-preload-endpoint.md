<properties
    pageTitle="Eel-laadida varad Azure'i CDN lõpp-punkti | Microsoft Azure'i"
    description="Saate teada, kuidas eel-laadida vahemällu talletatud sisu on CDN lõpp-punkti."
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

# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Azure'i CDN lõpp-punkti eelse laadi varad

[AZURE.INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Vaikimisi vahemällu varad esmalt need soovile. See tähendab, et iga piirkonna esimese taotluse võib kuluda, kuna Edge'i serverid ei ole sisu vahemällu ja tuleb kutse edasi origin serveriga. Sisu eelnevalt laadimisel aitab vältida selle esimese tabas latentsus.

Lisaks parema Kliendikogemuse, laadimine eelnevalt oma vahemällu talletatud varasid saate vähendada ka serveris origin võrguliiklust.

> [AZURE.NOTE] Eelnevalt laadimine varad on kasulik suurte sündmuste või sisu, mida saab korraga suure hulga kasutajate, nt filmi uus versioon või tarkvara värskendus.

Selles õpetuses juhendab teid eelnevalt laadimine kõik Azure'i CDN serva sõlmed vahemällu talletatud sisu.

## <a name="walkthrough"></a>Kiirtutvustus

1. Sirvige [Azure portaali](https://portal.azure.com)CDN profiil, mis sisaldab lõpp-punkti, mida soovite eelmääratletud laadimine.  Profiili tera avaneb.

2. Klõpsake loendis lõpp-punkti.  Lõpp-punkti tera avaneb.

3. Keelest CDN lõpp-punkti nuppu Laadi.

    ![CDN lõpp-punkti blade](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)

    Laadi tera avaneb.

    ![CDN laadi blade](./media/cdn-preload-endpoint/cdn-load-blade.png)

4. Sisestage täielik tee iga vara soovite laadimine (nt `/pictures/kitten.png`) väljale **tee** .

    > [AZURE.TIP] Lisateavet **tee** tekstiväljad kuvatakse pärast seda, kui sisestate teksti abil saate luua mitme varade nimekirja.  Saate kustutada varad loendist, klõpsates nuppu kolmikpunkti (…).
    >
    > Tuleb teed suhteline URL, mis sobib järgmine [Lihtavaldise](https://msdn.microsoft.com/library/az24scfc.aspx): `^(?:\/[a-zA-Z0-9-_.\u0020]+)+$`.  Iga vara peab olema oma tee.  On eelnevalt peale varad no metamärkide funktsioone.

    ![Laadi nupp](./media/cdn-preload-endpoint/cdn-load-paths.png)

5. Klõpsake nuppu **Laadi** .

    ![Laadi nupp](./media/cdn-preload-endpoint/cdn-load-button.png)

> [AZURE.NOTE] Seal on piirang 10 Laadi taotlusi minutis CDN profiili kohta.

## <a name="see-also"></a>Vt ka
- [Likvideerite Azure'i CDN lõpp](cdn-purge-endpoint.md)
- [Azure'i CDN REST API - likvideerite või eel-laadida lõpp](https://msdn.microsoft.com/library/mt634451.aspx)
