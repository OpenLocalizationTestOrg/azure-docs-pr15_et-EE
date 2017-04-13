<properties 
    pageTitle="Kuidas konfigureerida andmete väljundid voo Analytics tööde | Microsoft Azure'i" 
    description="Väljundeid konfigureerimine voo Analytics töö | Õppekeskuse tee lõigu."
    keywords="andmete väljund, andmete liikumine"
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

# <a name="how-to-configure-data-outputs-for-stream-analytics-jobs"></a>Kuidas konfigureerida andmete väljundid voo Analytics projektide jaoks

Azure'i voo Analytics töö saab ühendada ühe või mitme andmete väljundeid, mis määratlevad ühenduse mõne olemasoleva andmete valamu. Kui tööpäevad voo Analytics töötleb ja muudab sissetulevad andmed, andmevoo väljund sündmuste kirjutatakse oma töö väljund.

Voo Analytics andmete väljundeid saab kasutada andmeallika reaalajas armatuurlaudade või teatisi, andmete liikumine töövood, või lihtsalt arhiivida andmete Pakktöötlus uuem versioon. Voo Analytics on mitu Azure teenused, mis on üksikasjalikult esimese klassi integreerimine.

Voo Analytics tööpäevad väljund lisamiseks järgmist.

1. Azure'i klassikaline portaalis **väljundeid** nuppu ja seejärel klõpsake **Lisamine väljundi** voo Analytics tööpäevad.

    ![Väljundeid lisamine](./media/stream-analytics-add-outputs/1-stream-analytics-add-outputs.png)  

    Azure'i portaalis klõpsake **väljundeid** paani voog Analytics oma töökoha.

    ![Azure'i Porta lisamine väljundid](./media/stream-analytics-add-outputs/5-stream-analytics-add-outputs.png)

2. Määrake selle väljundi tüüp.

    ![Valige andmetüüp liikumine](./media/stream-analytics-add-outputs/2-stream-analytics-add-outputs.png)  

    ![Azure'i portaalis valige andmetüüp liikumine](./media/stream-analytics-add-outputs/6-stream-analytics-add-outputs.png)

3. Sisestage selle väljundi **Väljundi pseudonüüm** väljale sõbralik nimi. Selle nime saab oma töö päringu hiljem väljund viidata.  
    
    Täitke ülejäänud nõutavad ühenduse atribuudid ühenduse loomiseks oma väljundi.  Need väljad erinevad väljundi tüüp ja on määratletud üksikasjalikult siin.  

    ![Andmete väljundi atribuutide lisamine](./media/stream-analytics-add-outputs/3-stream-analytics-add-outputs.png)  

4. Sõltuvalt väljundi, peate määrata, kuidas andmeid seeriasertide või vormindatud. Iga väljundi jaoks teatud sariväljaanne sätted on siin dokumenteerida.

    Täitke ülejäänud nõutavad ühenduse atribuudid ühenduse andmeallikaga. Need väljad erineda sisestus- ja allika tüüp ja on määratletud üksikasjalikult [siin](stream-analytics-create-a-job.md).  

    ![Sündmuse jaoturi andmete väljund lisamine](./media/stream-analytics-add-outputs/4-stream-analytics-add-outputs.png)  

    ![Sündmuse jaoturi Azure portaali andmete väljund](./media/stream-analytics-add-outputs/7-stream-analytics-add-outputs.png)  

> [Azure.Note] Töö käivitatakse ja sündmuste hakkaksid, väljundi elemendi lisatakse tööd, mis tahes peab olemas. Näiteks kui kasutate bloobimälu väljund, töö pole loob salvestusruumi konto automaatselt. See peab kasutaja loodud enne ASA töö käivitamist.

## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)
