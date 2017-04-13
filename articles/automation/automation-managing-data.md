<properties 
   pageTitle="Azure'i automaatika andmete haldamine | Microsoft Azure'i"
   description="See artikkel sisaldab mitut teemade haldamise keskkonnas Azure automatiseerimine.  Praegu sisaldab andmete säilitamise ja Azure automatiseerimine avariitaastet Azure automatiseerimine varundada."
   services="automation"
   documentationCenter=""
   authors="SnehaGunda"
   manager="stevenka"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/02/2016"
   ms.author="bwren;sngun" />

# <a name="managing-azure-automation-data"></a>Azure'i automaatika andmete haldamine

See artikkel sisaldab mitut teemade haldamise keskkonnas Azure automatiseerimine.

## <a name="data-retention"></a>Andmete säilitamine

Kui kustutate ressursi Azure'i automaatika, säilitatakse 90 päeva Auditeerimispoliitika eesmärgil enne eemaldatakse jäädavalt.  Te ei näe või selle ressursi kasutamine selle aja jooksul.  See poliitika kehtib ka ressursse, mis kuuluvad automatiseerimise kontoga, mis kustutatakse.

Azure'i automaatika kustutab automaatselt ja eemaldatakse jäädavalt tööd, mis on vanemad kui 90 päeva.

Järgmises tabelis on toodud erinevad ressursid säilituspoliitika.

|Andmete|Poliitika|
|:---|:---|
|Kontod|Eemaldatakse jäädavalt 90 päeva pärast seda, kui kasutaja on kustutatud konto.|
|Varad|Eemaldatakse jäädavalt 90 päeva pärast seda, kui kasutaja on kustutatud vara või 90 päeva pärast malliga vara kustutatakse kasutaja konto.|
|Moodulid|Eemaldatakse jäädavalt 90 päeva pärast seda, kui kasutaja on kustutatud mooduli või 90 päeva pärast mooduli kustutatakse kasutaja malliga konto.|
|Tegevusraamatud|Eemaldatakse jäädavalt 90 päeva pärast seda, kui kasutaja kustutatakse ressursi või 90 päeva pärast malliga ressursi kustutatakse kasutaja konto.|
|Tööde haldamine|Kustutatud ja jäädavalt eemaldatud 90 päeva pärast viimase muutmise. See võib olla pärast töö on lõpule jõudnud, on peatatud või on peatatud.|
|Sõlm konfiguratsioone/RM failid| Vana sõlm konfiguratsiooni eemaldatakse jäädavalt 90 päeva pärast seda, kui luuakse uus sõlm konfiguratsioon.|
|DSC sõlmed| Eemaldatakse jäädavalt 90 päeva pärast sõlme on registreerimata automatiseerimise konto Azure portaali või [Registreerimise tühistamise-AzureRMAutomationDscNode](https://msdn.microsoft.com/library/mt603500.aspx) cmdlet-käsu kasutamine Windows PowerShelli kaudu. Sõlmed eemaldatakse jäädavalt ka 90 päeva pärast malliga sõlme kustutatakse kasutaja konto. |
|Sõlm aruanded| 90 päeva pärast seda, kui luuakse uus aruanne sõlme eemaldatakse jäädavalt|

Säilituspoliitika kehtib kõigile kasutajatele ja praegu ei saa kohandada.

## <a name="backing-up-azure-automation"></a>Azure'i automaatika varundamine

Microsoft Azure'i konto automatiseerimise kustutamisel kustutatakse konto kõigil objektidel tegevusraamatud, moodulid, konfiguratsioone, sätted, töö ja varad. Pärast konto kustutamist ei saa taastada objektid.  Saate varundada automatiseerimise konto sisu enne selle kustutamist järgmine teave. 

### <a name="runbooks"></a>Tegevusraamatud

Saate eksportida oma tegevusraamatud Azure'i haldusportaal või [Get-AzureAutomationRunbookDefinition](https://msdn.microsoft.com/library/dn690269.aspx) cmdlet-käsu kasutamine Windows PowerShelli skripti faile.  Need skripti faile saab importida automatiseerimise teisele kontole nagu [loomine või importimine on Käitusjuhendi](https://msdn.microsoft.com/library/dn643637.aspx).


### <a name="integration-modules"></a>Integreerimine moodulid

Integreerimine moodulid ei saa eksportimine Azure automatiseerimine.  Peab veenduge, et need on saadaval väljaspool automatiseerimise konto.

### <a name="assets"></a>Varad

[Varade](https://msdn.microsoft.com/library/dn939988.aspx) ei saa eksportimine Azure automatiseerimine.  Azure'i haldusportaal tuleb märkida üksikasjad muutujate, mandaat, tunnistuste, ühendused ja ajakava.  Seejärel käsitsi tuleb teil luua mis tahes varad, mida kasutatakse tegevusraamatud, mida saate importida teise automatiseerimine.

[Azure'i cmdlet-käskude](https://msdn.microsoft.com/library/dn690262.aspx) abil saate tuua üksikasjad krüptimata varad ja salvestage need hilisemaks või looge vara automatiseerimise teisele kontole.

Ei saa tuua krüptitud muutujate või parooli välja cmdlet-käskude abil.  Kui te ei tea need väärtused, siis saate tuua need on käitusjuhendi [Get-AutomationVariable](https://msdn.microsoft.com/library/dn940012.aspx) ja [Get-AutomationPSCredential](https://msdn.microsoft.com/library/dn940015.aspx) tegevuste abil.

Te ei saa eksportida serdid Azure automatiseerimine.  Teil peab tagada mis tahes serdid Azure'i väljaspool.

### <a name="dsc-configurations"></a>DSC konfiguratsioone

Saate eksportida oma konfiguratsioone Azure'i haldusportaal või [Ekspordi-AzureRmAutomationDscConfiguration](https://msdn.microsoft.com/library/mt603485.aspx) cmdlet-käsu kasutamine Windows PowerShelli skripti faile. Neid konfiguratsioone saab importida ja automatiseerimise mõne muu kontoga kasutada.


##<a name="geo-replication-in-azure-automation"></a>Azure'i automaatika Geo-dispersioonanalüüs

Geo-dispersioonanalüüs, standard Azure automatiseerimine kontodele, varundab konto andmeid jaoks koondamise geograafiliste muule alale. Esmane piirkonnas saate valida, kui teie konto häälestamise ja seejärel teisene piirkonnas on määratud selle automaatselt. Kopeeritud piirkonnast esmane, teisene andmeid värskendatakse pidevalt andmete kaotsimineku korral.  

Järgmises tabelis on saadaval põhi- ja piirkonnasätete sidumiste.

|Esmane            |Teisese
| ---------------   |----------------
|Lõuna-, Kesk-USA   |Põhja Kesk-USA
|USA Ida 2          |Kesk-USA
|Lääne Euroopa        |Põhja-Euroopa
|Kagu-Aasia    |Ida-Aasia
|Jaapan Ida         |Jaapan Lääne

Juhul, et esmane regiooni andmeid läheb kaotsi, proovib Microsoft seda taastada. Kui lähteandmeid ei saa taastada, siis toimub geo-Tõrkesiirde ja mõjutada kliente teavitatakse sellest oma tellimuse kaudu.

