<properties 
   pageTitle="Windowsi sündmuste logi Analytics logib | Microsoft Azure'i"
   description="Windowsi sündmuste logid on kasutatud Log Analytics kõige levinum andmeallikat.  Selles artiklis kirjeldatakse, kuidas konfigureerida Windowsi sündmuselogide kogumine ja nende loodud OMS hoidlas kirjete üksikasjad."
   services="log-analytics"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="log-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/18/2016"
   ms.author="bwren" />

# <a name="windows-event-log-data-sources-in-log-analytics"></a>Windowsi sündmuselogi andmeallikate Log Analytics

Windowsi sündmuste logid on üks kõige levinum [andmeallikate](log-analytics-data-sources.md) kasutatakse Windows agentide, kuna see on enamiku rakenduste meetod logige teavet ja tõrgete.  Lisaks määramisele mis tahes kohandatud logid loodud rakendustega, mida vajate jälgimiseks saate sündmuste kogumine standard logid nagu süsteem ja rakendus.

![Windowsi sündmuste](media/log-analytics-data-sources-windows-events/overview.png)     

## <a name="configuring-windows-event-logs"></a>Konfigureerimine Windowsi sündmuste logib

Windowsi sündmuste logid [menüü andmed Log Kasutusanalüüsi sätteid](log-analytics-data-sources.md#configuring-data-sources)konfigureerida.

Log Analytics kogub sündmuste ainult Windows sündmuselogide, mis on määratud sätted.  Logi nime tippimist ja klõpsates saate lisada uue logi **+**.  Iga Logi kogutakse ainult valitud raskusastme sündmusega.  Märkige ruut raskusastme kindla Logi, mida soovite koguda.  Ei saa anda täiendavate kriteeriumide filter sündmused.

![Windowsi sündmuste konfigureerimine](media/log-analytics-data-sources-windows-events/configure.png)


## <a name="data-collection"></a>Andmete kogumine

Log Analytics kogub iga sündmuse, mis vastab teie valitud raskusaste kaudu jälgida sündmuste logi sündmus loomisel.  Agent, siis salvestab iga kogutud kaudu sündmuste logi selle asemel.  Kui agent läheb ühenduseta teatud aja jooksul, siis Log Analytics kogub sündmused, kus see viimati pooleli jäi, isegi juhul, kui need sündmused on loodud agent võrguühenduseta oleku ajal.


## <a name="windows-event-records-properties"></a>Windowsi sündmuste kirjete atribuudid

Windowsi sündmuste kirjeid **sündmuse** tüüp ja atribuudid on järgmises tabelis.

| Atribuut | Kirjeldus |
|:--|:--|
| Arvuti            | Arvuti, mis on kogutud sündmuse nimi. |
| EventCategory       | Kategooria soovitud sündmus. |
| EventData           | Kõik sündmuse andmed vormingusse. |
| EventID             | Sündmuse arv. |
| EventLevel          | Sündmuse arvuline vorm tõsidust. |
| EventLevelName      | Sündmuse teksti kujul tõsidust. |
| EventLog            | Sündmuste logi kaudu kogutud sündmuse nimi. |
| ParameterXml        | Sündmuse parameetrite väärtused XML-vormingus. |
| ManagementGroupName | SCOM agentide haldus rühma nimi.  Muude töötajatega, see on AOI-<workspace ID> |
| RenderedDescription | Sündmuse kirjeldus koos parameetrite väärtused |
| Allikas              | Sündmuse allikas. |
| SourceSystem  | Sündmuse tüüp on kogutud. <br> OpsManager – Windows agent, kas otse ühendust luua või SCOM <br> Linux – kõik Linuxi agentide  <br> AzureStorage – Azure diagnostika |
| TimeGenerated       | Kuupäev ja kellaaeg, sündmuse loomise Windows. |
| Kasutajanimi            | Sisse logitud sündmuse konto kasutajanimi. |



## <a name="log-searches-with-windows-events"></a>Windowsi sündmuste logi otsinguid

Järgmises tabelis antakse eri näiteid tuua kirjete Windowsi sündmuste logi otsingud.

| Päringu | Kirjeldus |
|:--|:--|
| Tüüp = sündmuse | Kõigi Windowsi sündmuste. |
| Tüüp = sündmuse EventLevelName = tõrge | Kõigi Windowsi sündmuste ja tõsidust tõrge. |
| Tüüp = sündmuse & #124; Mõõtke count() allika järgi | Loendamine Windowsi sündmuste allika järgi. |
| Tüüp = sündmuse EventLevelName = tõrge & #124; Mõõtke count() allika järgi | Loendamine Windowsi tõrge sündmuste allika järgi. |

## <a name="next-steps"></a>Järgmised sammud

- Log Analytics koguda muude [andmeallikate](log-analytics-data-sources.md) analüüsi jaoks konfigureerida.
- Lisateavet [log otsingud](log-analytics-log-searches.md) andmeallikate ja lahenduste kogutud andmete analüüsimiseks.  
- [Kohandatud väljade](log-analytics-custom-fields.md) abil sündmuse kirjete sõeluda üksikuid välju.
- Konfigureerida oma Windows agentide [jõudluse hinnale kogum](log-analytics-data-sources-performance-counters.md) .