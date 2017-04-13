<properties
    pageTitle="Azure'i Networking Analytics lahenduse Log Analytics | Microsoft Azure'i"
    description="Saate Azure'i Networking Analytics lahendus Log Analytics Azure võrgu turvalisus jaotises logid ja Azure rakenduse lüüsi logid."
    services="log-analytics"
    documentationCenter=""
    authors="richrundmsft"
    manager="jochan"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="richrund"/>

# <a name="azure-networking-analytics-preview-solution-in-log-analytics"></a>Azure'i Analytics Networking (eelvaade) lahenduse Log Analytics

>[AZURE.NOTE] See on [Eelvaade lahendus](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Saate Azure'i Networking Analytics lahendus Log Analytics Azure'i rakenduse lüüsi logid ja Azure võrgu turvalisus jaotises logid.

Saate lubada logimise Azure'i rakenduse lüüsi logid ja Azure võrgu turberühmad. Bloobivahemälu salvestusruumi, kus ta saab siis indekseerida Log Analytics otsimiseks ja analüüsi kirjutada need logid.

Rakenduse lüüsid on toetatud järgmised logid:

+ ApplicationGatewayAccessLog
+ ApplicationGatewayPerformanceLog

Võrgu turberühmad on toetatud järgmised logid:

+ NetworkSecurityGroupEvent
+ NetworkSecurityGroupRuleCounter

## <a name="install-and-configure-the-solution"></a>Installima ja konfigureerima lahendus

Järgmised juhised abil saate installida ja konfigureerida Azure Networking Analytics lahendus.

1.  Ressursid, mida soovite jälgida diagnostika logimise lubamiseks tehke järgmist.
  + [Rakenduse lüüsi](../application-gateway/application-gateway-diagnostics.md)
  + [Võrgu turberühm](../virtual-network/virtual-network-nsg-manage-log.md)
2.  Log Analytics kirjeldatud [JSON](../log-analytics/log-analytics-azure-storage-json.md)failid järgmises vormingus bloobimälu abil lugemise logid bloobimälu konfigureerimine.
3.  Azure'i Networking Analytics lahenduse lubada [lisada Log Analytics](log-analytics-add-solutions.md)Solutions lahendusegaleriist kirjeldatud.  

Kui te ei luba diagnostikalogimine ressursile tüübi, armatuurlaua labad selle ressursi tühjaks.

## <a name="review-azure-networking-analytics-data-collection-details"></a>Vaadake üle Azure'i Networking Analytics andmete kogumise üksikasjad

Azure'i Networking Analytics lahendus kogub diagnostika logid Azure'i bloobimälu Azure'i rakenduse lüüside ja võrgu turberühmad.
Pole agent on vaja andmete kogumine.

Järgmises tabelis on andmete kogumise meetodite ja muud üksikasjad kohta, kuidas koguda andmeid Azure'i Networking Analytics.

| Platvorm | Otsest agent | Süsteemide Center toimingute Manager (SCOM) agent | Azure'i salvestusruum | SCOM on nõutav? | Rühma kaudu saadetud SCOM agendi andmed | Saidikogumi sagedus |
|---|---|---|---|---|---|---|
|Azure'i|![Ei](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Ei](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Jah](./media/log-analytics-azure-networking/oms-bullet-green.png)|            ![Ei](./media/log-analytics-azure-networking/oms-bullet-red.png)|![Ei](./media/log-analytics-azure-networking/oms-bullet-red.png)| 10 minuti|

## <a name="use-azure-networking-analytics"></a>Azure'i võrgunduse Analytics kasutamine

Pärast installimist lahenduse, saate vaadata kliendi kokkuvõte ja serveri tõrgete jaoks teie jälgitud rakenduse lüüside abil **Azure'i Networking Analytics** paani Log Analytics lehel **Ülevaade** .

![Azure'i Networking Analytics paani pilt](./media/log-analytics-azure-networking/log-analytics-azurenetworking-tile.png)

Kui olete klõpsanud **Ülevaade** , saate vaadata oma logid kokkuvõtted ja seejärel Mine järgmisi üksikasju:

+ Accessi rakenduse lüüsi logib
  - Rakenduse lüüsi Accessi logide kliendi ja serveri tõrked
  - Kutsed iga rakenduse Gateway tunnis
  - Iga rakenduse lüüsi taotlusi tunnis nurjus
  - Tõrgete jaoks rakenduse lüüside kasutajaagendi alusel
+ Rakenduse lüüsi jõudlus
  - Rakenduse Gateway Host seisund
  - Maksimum- ja 95 protsentiili rakenduse lüüsi nurjunud taotluste
+ Võrgu turberühma blokeeritud puhul
  - Võrgu turvalisus jaotises reeglite blokeeritud puhul
  - MAC aadressid blokeeritud puhul
+ Võrgu turberühma puhul lubatud
  - Võrgu turvalisus jaotises reeglite puhul lubatud
  - MAC aadressid puhul lubatud


![Azure'i Networking Analytics armatuurlaua pilt](./media/log-analytics-azure-networking/log-analytics-azurenetworking01.png)

![Azure'i Networking Analytics armatuurlaua pilt](./media/log-analytics-azure-networking/log-analytics-azurenetworking02.png)

![Azure'i Networking Analytics armatuurlaua pilt](./media/log-analytics-azure-networking/log-analytics-azurenetworking03.png)

![Azure'i Networking Analytics armatuurlaua pilt](./media/log-analytics-azure-networking/log-analytics-azurenetworking04.png)

### <a name="to-view-details-for-any-log-summary"></a>Mis tahes Logi Kokkuvõte üksikasjade vaatamiseks

1. Klõpsake lehe **Ülevaade** **Azure'i Networking Analytics** paani.
2. Armatuurlaual **Azure'i Networking Analytics** läbi kokkuvõtlik teave ühes labad ja seejärel klõpsake ühte vaadata üksikasjalikku teavet lehel log.

    Mis tahes Logi otsing, saate vaadata tulemusi aeg, üksikasjalikud tulemused ja ajaloo log otsing. Samuti saate filtreerida omadustega tulemeid piiritleda.

## <a name="next-steps"></a>Järgmised sammud

- [Log otsingud Log Analytics](log-analytics-log-searches.md) abil saate vaadata üksikasjalikku Azure'i Networking Analytics andmete.
