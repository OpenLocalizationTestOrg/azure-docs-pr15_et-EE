<properties
   pageTitle="Microsoft Power BI manustatud alustamine"
   description="Power BI manustatud, lisada business intelligence rakenduse Power BI interaktiivseid aruandeid"
   services="power-bi-embedded"
   documentationCenter=""
   authors="guyinacube"
   manager="erikre"
   editor=""
   tags=""/>
<tags
   ms.service="power-bi-embedded"
   ms.devlang="NA"
   ms.topic="hero-article"
   ms.tgt_pltfrm="NA"
   ms.workload="powerbi"
   ms.date="10/04/2016"
   ms.author="asaxton"/>

# <a name="get-started-with-microsoft-power-bi-embedded"></a>Microsoft Power BI manustatud alustamine

**Power BI manustatud** on Azure'i teenus, mis lubab rakenduse Power BI interaktiivseid aruandeid lisada oma rakendusi. Olemasolevate rakenduste **Power BI manustatud** töötab ilma plaani või kasutajate muutmine Logi sisse.

**Microsoft Power BI manustatud** ressursid on ette valmistatud [Azure'i ARM API-de](https://msdn.microsoft.com/library/mt712306.aspx)kaudu. Sellisel juhul olete ette ressurss on **Power BI tööruumi saidikogumi**.

![](media\power-bi-embedded-get-started\introduction.png)

## <a name="create-a-workspace-collection"></a>Tööruumi saidikogumi loomine
**Tööruumi saidikogumi** on ülataseme Azure ressursside ja sisu, mis on manustatud rakenduse ümbris. **Tööruumi saidikogumi** saab luua kahel viisil:

   -    Käsitsi abil Azure'i portaal
   -    Azure'i ressursi Manager(ARM) API-de programmiliselt abil

Vaatame juhiseid koostamiseks **Tööruumi saidikogumi** Azure'i portaalis.

   1.   Avage ja **Azure portaali**sisse logida: [http://portal.azure.com](http://portal.azure.com).

   2.   Valige **+ Uus** pealmine.

       ![](media\power-bi-embedded-get-started\create-workspace-1.png)

   3.   Klõpsake jaotises **andmete + Analytics** **Power BI manustatud**.
   4.   **Loomise Blade**, sisestage nõutav teave. **Hinnakirjad**, leiate teemast [Power BI manustatud hinnad](http://go.microsoft.com/fwlink/?LinkID=760527).

       ![](media\power-bi-embedded-get-started\create-workspace-2.png)

   5. Klõpsake nuppu **Loo**.

**Tööruumi saidikogumi** saavad võtta mõne hetke aega ettevalmistamine. Kui lõpule viidud, suunatakse **Tööruumi saidikogumi Blade**.

   ![](media\power-bi-embedded-get-started\create-workspace-3.png)

**Loomiskuupäeva Blade** sisaldab API-d, tööruumide ja sisu juurutamiseks vajalikku teavet.

<a name="view-access-keys"/>
## <a name="view-power-bi-api-access-keys"></a>Vaade Power BI API Accessi nooleklahvid

Üks kõige olulisemad teavet Power BI REST API-d on **Kiirklahvide**. Neid kasutatakse **rakenduse sõned** autentida oma API taotlusi kasutatud loomiseks. Teie **Kiirklahvide**kuvamiseks klõpsake nuppu **Sätted Blade** **Kiirklahvide** . **Rakenduse sõned**kohta leiate lisateavet teemast [autentimine ja lubab koos Power BI manustatud](power-bi-embedded-app-token-flow.md).

   ![](media\power-bi-embedded-get-started\access-keys.png)

Saate "teate, et teil on kaks võtit.

   ![](media\power-bi-embedded-get-started\access-keys-2.png)

Kopeerige need võtmed ja salvestaks need turvaliselt rakenduse. On väga oluline, et valite klahve nii, nagu teeksite parooli, sest annab juurdepääsu kogu sisu oma **Tööruumi saidikogumi**.

Kui loendis on kaks võtit, ainult üks klahv on vaja teatud ajal. Teise võtme on esitatud nii, et teil perioodiliselt taastada klahvid teenusele juurdepääsu katkestamata.

Nüüd, kui teil on näiteks Power BI rakendus ja **Kiirklahvide**jaoks, saate importida oma rakenduse aruande. Enne kui saate teada, kuidas importida aruande, järgmises jaotises kirjeldatakse loomise Power BI andmekomplektide ja aruannete rakendusse manustamiseks.

## <a name="create-power-bi-datasets-and-reports-to-embed-into-an-app"></a>Power BI andmekomplektide ja rakendusse manustamise kohta aruannete loomine

Nüüd, kui olete loonud eksemplari Power BI rakenduse, ja teil **Kiirklahvide**, peate luua Power BI andmekogumite ja aruanded, mille soovite manustada. **Power BI Desktopi**abil saab luua andmekomplektide ja aruandeid. Saate [Power BI Desktopi jaoks tasuta](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)alla laadida. Või kiiresti alustada, saate alla laadida selle [jaemüügi analüüsi valimi PBIX] (http://go.microsoft.com/fwlink/?LinkID=780547). Lisateavet **Power BI Desktopi**kasutamise kohta leiate teemast [Power BI Desktopi kasutamise alustamine](https://powerbi.microsoft.com/en-us/guided-learning/powerbi-learning-0-2-get-started-power-bi-desktop).

**Power BI Desktopi**, kus saate oma andmeallikaga ühenduse **Power BI Desktopi** koopia andmete importimine või **DirectQuery**andmeallika ühenduse.

Siin on **impordi** - ja **DirectQuery**kasutamise erinevused.

|Importimine | DirectQuery
|---|---
|Tabelite, veergude *ja andmete* imporditakse või **Power BI Desktopi**kopeerida. Visualiseeringute töötamisel **Power BI Desktopi** päringute andmete koopia. Aluseks olevad andmed tehtud muudatuste vaatamiseks värskendage või lõpule jõudnud, praegune andmehulga uuesti importimiseks.|Ainult, *tabelite ja veergude* on imporditud või **Power BI Desktopi**kopeerida. Visualiseeringute töötamisel **Power BI Desktopi** päringute aluseks oleva andmeallika, mis tähendab, et vaatate alati praegusi andmeid.

Andmeallikaga ühenduse loomise kohta leiate lisateavet teemast [andmeallikaga ühenduse loomine](power-bi-embedded-connect-datasource.md).

Kui salvestate oma töö **Power BI Desktopi**, luuakse PBIX faili. See fail sisaldab aruandesse. Lisaks, kui määrate andmete importimiseks soovitud PBIX sisaldab täieliku andmekomplekti või **DirectQuery**kasutamisel on PBIX andmekomplekti skeem. Funktsiooni PBIX juurutamist programmiliselt tööruumi [Power BI impordi API](https://msdn.microsoft.com/library/mt711504.aspx)abil.

> [AZURE.NOTE] **Power BI manustatud** on täiendavad API-de määramine teenuse konto mandaat, kasutavate andmekomplekti oma andmebaasiga ühenduse loomiseks ja muutmiseks server ja andmebaas, mis osutab oma andmekomplekti. Vaadake [postituse SetAllConnections](https://msdn.microsoft.com/library/mt711505.aspx) ja [paik lüüsi andmeallikas](https://msdn.microsoft.com/library/mt711498.aspx).

## <a name="next-steps"></a>Järgmised sammud
Et eelmiste juhiste järgi loodud tööruumi kogumine ja teie esimene aruanne ja andmekomplekti. Nüüd on aeg saate teada, kuidas **Power BI manustatud**koodi kirjutamiseks. Aitavad teil alustada, oleme loonud valimi web appi: [valimi kasutamise alustamine](power-bi-embedded-get-started-sample.md). Valimi näidatakse, kuidas soovite:

  - Sätte sisu
      - Tööruumi loomine
      - PBIX faili importimine
      - Ühendusstringi värskendada ja määrake oma andmekogumite mandaat.

  - Aruande turvaliselt manustamine

## <a name="see-also"></a>Vt ka
- [Valimi kasutamise alustamine](power-bi-embedded-get-started-sample.md)
- [Autentimist ja lubab koos Power BI manustatud](power-bi-embedded-app-token-flow.md)
- [Power BI Desktopi](https://powerbi.microsoft.com/documentation/powerbi-desktop-get-the-desktop/)
