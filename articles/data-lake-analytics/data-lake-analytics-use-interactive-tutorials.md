<properties 
   pageTitle="Teavet Lake andmeanalüüsi ja U-SQL Azure'i portaal interaktiivne õpetused abil | Azure'i" 
   description="Lühijuhend Lake andmeanalüüsi ja U-SQL-i abil. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="edmacauley" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="05/16/2016"
   ms.author="edmaca"/>


# <a name="use-azure-data-lake-analytics-interactive-tutorials"></a>Kasutage Azure'i andmeanalüüsi Lake interaktiivsed õppematerjalid

Azure'i portaalis on interaktiivne õpetus saate alustada Lake andmeanalüüsi. See artikkel näitab, kuidas läbida õpetuses mõeldud logiandmete veebisaidi.


>[AZURE.NOTE]Kui soovite läbida samas õpetuse Visual Studio abil, vt [analüüsi veebisaidi logid Lake andmeanalüüsi abil](data-lake-analytics-analyze-weblogs.md).
>Veel interaktiivsed juhendid portaali lisatakse.


Muud õpetused leiate.

- [Azure'i portaalis Lake andmeanalüüsi kasutamise alustamine](data-lake-analytics-get-started-portal.md)
- [Alustamine Lake andmeanalüüsi Azure PowerShelli abil](data-lake-analytics-get-started-powershell.md)
- [Kasutades .NET SDK Lake andmeanalüüsi kasutamise alustamine](data-lake-analytics-get-started-net-sdk.md)
- [Arendamise U-SQL-i skriptide abil andmete Lake tööriistad Visual Studio](data-lake-analytics-data-lake-tools-get-started.md) 

**Eeltingimused**

Enne alustamist selles õpetuses, peab teil olema järgmised:

- **A Lake andmeanalüüsi konto**.  Teemast [Azure Lake andmeanalüüsi Azure'i portaalis alustamine](data-lake-analytics-get-started-portal.md).

##<a name="create-data-lake-analytics-account"></a>Andmeanalüüsi Lake konto loomine 

Saate kasutada mis tahes töö peab teil olema Lake andmeanalüüsi konto.

Iga Lake andmeanalüüsi konto on mõni [Azure andmesalve Lake](../data-lake-store/data-lake-store-overview.md) konto sõltuvus.  Selle konto nimetatakse Lake andmesalve konto.  Saate luua Lake andmesalve konto eelnevalt, või Lake andmeanalüüsi konto loomisel. Selles õpetuses loote Lake andmesalve konto Analyticsi kontoga

**Andmeanalüüsi Lake konto loomine**

1. [Azure'i portaali](https://portal.azure.com/signin/index/?Microsoft_Azure_Kona=true&Microsoft_Azure_DataLake=true&hubsExtension_ItemHideKey=AzureDataLake_BigStorage%2cAzureKona_BigCompute)logima.
2. Klõpsake vasakus ülanurgas olevat StartBoard avamiseks **Microsoft Azure'i** .
3. Klõpsake paani **turuplatsilt** .  
3. **Azure'i andmeanalüüsi Lake** tippige otsinguväljale **Kõik** tera ja vajutage sisestusklahvi **ENTER**. **Azure'i andmeanalüüsi Lake** peab kuvatakse loendis.
4. Valige loendist väärtus **Azure'i andmeanalüüsi Lake** .
5. Klõpsake nuppu **Loo** tera allservas.
6. Tippige või valige üks järgmistest:

    ![Azure'i andmed Lake Analytics portaali blade](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-create-adla.png)

    - **Nimi**: Analytics konto nimi.
    - **Lake andmesalve**: iga Lake andmeanalüüsi konto on sõltuvad Lake andmesalve konto. Funktsiooni Lake andmeanalüüsi ja sõltuvad Lake andmesalve konto asuma samas Azure andmekeskuse. Järgige juhiseid, et Lake andmesalve uue konto loomine või valige olemasoleva.
    - **Tellimus**: valige Azure'i tellimus, kasutatakse Analytics konto.
    - **Ressursirühm**. Valige olemasolev Azure'i ressursirühm või looge uus. Rakendused on tavaliselt valmistatud palju komponendid, näiteks web appi, andmebaasi, andmebaasi server, salvestusruumi ja 3 tootja teenused. Azure'i ressursi Manager (ARM) võimaldab teil töö ressurssidega oma rakenduse rühmana, nimetatakse ka Azure ressursirühma. Juurutada, värskendamine, jälgida või kustutada kõik ressursse rakenduse ühe ja koordineeritud toiming. Malli kasutamine juurutamiseks ja sellel mallil saate töötada viibite, nt katsetamine, lavastus ja tootmise. Arveldamine selgitada oma ettevõtte jaoks soovitud kulude terve rühma vaatamine. Lisateavet leiate teemast [Azure ressursihaldur ülevaade](azure-resource-manager/resource-group-overview.md). 
    - **Asukoht**. Valige Lake andmeanalüüsi konto keskuse sellise Azure'i andmed. 
7. Valige **Kinnita Startboard**. See on vaja pärast selle õpetuse.
8. Klõpsake nuppu **Loo**. See viib teid portaali StartBoard. Uue paani lisatakse avalehele, silt näitab "Juurutamine Azure'i Lake andmeanalüüsi". Mõni hetk Lake andmeanalüüsi konto loomiseks. Kui konto on loodud, avaneb portaali raseerimisterast uus konto.

    ![Azure'i andmed Lake Analytics portaali blade](./media/data-lake-analytics-get-started-portal/data-lake-analytics-portal-blade.png)

##<a name="run-website-log-analysis-interactive-tutorial"></a>Käivitage veebisaidi logi analüüs interaktiivne õpetus

**Veebisaidi Log Analytics interaktiivsed õpetuse avamine**

1. Portaalist, klõpsake käsku **Microsoft Azure'i** vasakpoolsest menüüst soovitud StartBoard avamiseks.
2. Klõpsake paani Lake andmeanalüüsi kontoga lingitud.
3. **Essentialsi** ribal nuppu **Uuri interaktiivse õpetused** .

    ![Andmete Lake Analytics interaktiivsed õppematerjalid](./media/data-lake-analytics-use-interactive-tutorials/data-lake-analytics-explore-interactive-tutorials.png)

4. Kui näete soovitud räägivad "Näidised, mis pole seadistatud, klõpsake..." oranž hoiatus, klõpsake nuppu **Kopeeri Näidisandmete** Näidisandmete kopeerimiseks Lake andmesalve vaikekonto. Interaktiivne õpetuse vajab käivitamiseks andmeid.
5. Klõpsake keelest **Interaktiivsed juhendid** **Veebisaidi Log Analytics**. Portaali avab uue portaali blade õpetuse.
5. Klõpsake **1 Sissejuhatus** ja seejärel järgige juhiseid

##<a name="see-also"></a>Vt ka

- [Microsoft Azure'i andmed Lake Analytics ülevaade](data-lake-analytics-overview.md)
- [Azure'i portaalis Lake andmeanalüüsi kasutamise alustamine](data-lake-analytics-get-started-portal.md)
- [Alustamine Lake andmeanalüüsi Azure PowerShelli abil](data-lake-analytics-get-started-powershell.md)
- [Arendamise U-SQL-i skriptide abil andmete Lake tööriistad Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Veebisaidi logid abil Azure'i andmeanalüüsi Lake analüüs](data-lake-analytics-analyze-weblogs.md)
