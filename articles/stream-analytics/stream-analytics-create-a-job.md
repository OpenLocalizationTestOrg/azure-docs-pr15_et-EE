<properties 
    pageTitle="Kuidas luua voo Analytics andmete Kasutusanalüüsi töötlemise töö | Microsoft Azure'i" 
    description="Voo Analytics andmete Kasutusanalüüsi töötlemise töö loomine | Õppekeskuse tee lõigu."
    keywords="andmete analüüs"
    documentationCenter=""
    services="stream-analytics"
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/> 

# <a name="how-to-create-a-data-analytics-processing-job-for-stream-analytics"></a>Kuidas luua voo Analytics andmete Kasutusanalüüsi töötlemise töö

Azure'i voo Analytics ülataseme ressursi on voo Analytics töö.  Koosneb sisendandmete ühest või mitmest allikast, avaldada andmete teisendus päringu ja ühe või mitme tulemused kirjutada väljund sihtkohtade. Koos need võimaldavad kasutajal andmete Kasutusanalüüsi töötlemise streaming andmete stsenaariumid teha.

Voo Analyticsi kasutamise alustamiseks kõigepealt luua uue voo Analytics töö.  Pange tähele, et see toiming ei mõjuta arvelduse kuni töö käivitatakse.

1.  Logige sisse online [Azure klassikaline portaali](http://manage.windowsazure.com) või [Azure portaali](https://portal.azure.com/).
2.  Portaali: **Klõpsake nuppu Uus**, seejärel nuppu **Data Services** või **Andmeanalüüsi** sõltuvalt teie portaali ja klõpsake **Azure voo Analytics** või **Voo Analytics** ja seejärel **Kiiresti luua**.

    ![Kasutusanalüüsi töötlemise töö viisard](./media/stream-analytics-create-a-job/1-stream-analytics-create-a-job.png)  

    ![Andmeanalüüsi töö loomine](./media/stream-analytics-create-a-job/4-stream-analytics-create-a-job.png)  

3.  Määrake soovitud konfiguratsiooni voo Analytics töö.
    - **Töö nimi** väljale Sisestage nimi voo Analytics töö tuvastamiseks. Kui **Töö nimi** on kinnitatud, kuvatakse roheline märge töö nimi väljale nimi. **Töö nimi** võib sisaldada ainult tähti ja numbreid ja "-" märkide ja peab olema 3 kuni 63 märki.
    - **Piirkonna** Azure portaali või **asukoht** Azure'i portaalis määramiseks kasutage geograafiline asukoht, kuhu soovite töö.
    - Kui Azure portaali kaudu, valige või salvestusruumi konto kasutamiseks **Piirkondliku jälgimine salvestusruumi konto**loomine. Selle konto salvestusruumi kasutatakse selle piirkonna kõik voo Analytics töid andmetele.
    - Kui Azure portaali, määrake uude või olemasolevasse **Ressursirühm** pikalt rakenduse seotud ressursid.

4.  Kui uus voo Analytics töö suvandid on konfigureeritud, klõpsake **Voo Analytics töö loomine**. Võib kuluda mõni minut voo Analytics töö luua. Oleku, saate teatiste keskuses edenemist jälgida.

    ![Andmete Kasutusanalüüsi töötlemise töö notfications jaoturi](./media/stream-analytics-create-a-job/2-stream-analytics-create-a-job.png)  

    ![Azure'i portaali andmete analüüsimise töö töö loomine](./media/stream-analytics-create-a-job/5-stream-analytics-create-a-job.png)  

5.  Uue töö kuvatakse olek on **loodud**. Pange tähele, et nupp **Start** on keelatud. Enne alustamist töö tuleb konfigureerida töö sisestus-, päringu- ja väljundi.

    ![Kasutusanalüüsi töötlemise töö töö oleku](./media/stream-analytics-create-a-job/3-stream-analytics-create-a-job.png)  

    ![Azure portaali andmete Kasutusanalüüsi töötlemise töö töö olek](./media/stream-analytics-create-a-job/6-stream-analytics-create-a-job.png)  

## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
