<properties
    pageTitle="Rentniku ressursikasutus API | Microsoft Azure'i"
    description="Viide ressursikasutus API, mis tuua Azure'i Virnlintdiagrammil kasutamise kohta."
    services="azure-stack"
    documentationCenter=""
    authors="AlfredoPizzirani"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="alfredop"/>

# <a name="tenant-resource-usage-api"></a>Rentniku ressursikasutus API

Rentniku saate kasutada rentniku API rentniku enda ressursi kasutamine andmete kuvamiseks. See API on kooskõlas Azure kasutuse API (praegu privaatne eelvaade).

Windows PowerShelli cmdlet-käsk **Get-UsageAggregates** abil saate hankida kasutusandmete nagu Azure.

## <a name="api-call"></a>API kõne

### <a name="request"></a>Taotlemine

Kutse saab tarbimine üksikasjad nõutud tellimused ja nõutud aja jooksul. Ei ole taotluse keha.

| **Meetod**  | **Taotluse URI** |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| HANKIMINE         | https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/usageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>Argumendid

| **Argument**             | **Kirjeldus** |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *Armendpoint*             | Azure'i ressursihaldur Azure'i virnas keskkonna lõpp-punktile. |
| *subId*                   | Tellimuse ID kasutaja teeb kõne. Saate selle API päringusse ainult ühe tellimuse kasutamine. Pakkujad saate kasutada pakkuja ressursi kasutamine API päringu kasutuse kõik rentnike jaoks. |
| *reportedStartTime*       | Päringu alguskellaaeg. Väärtus *DateTime* tuleks UTC-vormingus ja tunni, näiteks 13:00 alguses. Igapäevane liitmiseks, määrake selle väärtuseks UTC keskööst. Vorming on *pääsenud* ISO 8601, näiteks 2015-06-16T18% 3a53% 3a11% 2b00% 3a00Z, kus on koolon põgenes 3% ja pluss on põgenes % 2b nii, et see on sõbralik URI. |
| *reportedEndTime*         | Päringu lõppkellaaeg. Piiranguid, mis rakenduvad *reportedStartTime* rakendada ka seda argumenti. *ReportedEndTime* väärtus ei saa olla tulevikus. |
| *aggregationGranularity*  | Valikuline parameeter, mis sisaldab kahte eraldi võimalikud väärtused: iga päev ja tunnis. Nagu väärtused soovitamine, üks tagastab andmed igapäevane granulaarsus ja teine on mõne tunni eraldusvõime. Vaikimisi on iga päev suvand. |
| *subscriberId*            | Tellimuse ID-ga. Filtreeritud andmete toomine otse rentniku pakkuja Tellimuse ID on nõutav. Kui määratud pole tellimuse ID parameeter, tagastab kõne kasutusandmete kõik pakkuja otsese rentnikke. |
| *API-versioon*             | Mida saab teha taotlus-protokolli versiooni. Kasutage 2015-06-01-eelvaade. |
| *continuationToken*       | Luba vormilt kasutus API pakkuja viimase kõne. See on vajalik, kui vastus on suurem kui 1000 jooned. See on pooleli jaoks järjehoidja. Kui ei esine andmed tuuakse päeva alguses või tunni, võttes aluseks granulaarsus edasi. |

### <a name="response"></a>Vastus

SAADA /subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 & reportedEndTime = 2015-06-01T00% 3a00% 3a00% 2b00% 3a00 & aggregationGranularity = iga päev ja api-versiooni = 1.0

{

"value":\[

{

"id": "/ subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregate/sub1-meterID1",

"nimi": "sub1 meterID1"

"tüüp": "Microsoft.Commerce/UsageAggregate"

"atribuudid": {}

"subscriptionId": "sub1"

"usageStartTime": "2015-03-03T00:00:00 + 00:00",

"usageEndTime": "2015-03-04T00:00:00 + 00:00",

"instanceData": "{\\" Microsoft.Resources\\": {\\" resourceUri\\":\\" resourceUri1\\",\\" asukoht\\":\\" Alaskat\\",\\" siltide\\": null,\\" additionalInfo\\": null}}",

"kogus":2.4000000000,

"meterId": "meterID1"

}

},

…

### <a name="response-details"></a>Vastuse üksikasjad

| **Argument**      | **Kirjeldus** |
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| *ID*              | Kasutus kokku Ainuidentifikaator |
| *Nimi*            | Kasutus liitmise nimi |
| *tüüp*            | Ressursside määratlemine |
| *subscriptionId*  | Tellimuse Azure kasutaja ainuidentifikaator |
| *usageStartTime*  | UTC alguskellaaeg kasutus ämber, kuhu see kasutus aggregate kuulub |
| *usageEndTime*    | UTC lõppaega, kuhu see kasutus aggregate kuulub kasutus ämber |
| *instanceData*    | Võti ja väärtuse paarideks eksemplari üksikasjade (uus vormingus):<br>  *resourceUri*: nõuetele täielikult vastav ressursi ID-d, sh ressursside rühmad ja eksemplari nimi <br>  *asukoht*: piirkond, kus on selle teenuse käivitamine <br>  *Sildid*: ressursi sildid, mis määrab kasutaja <br>  *additionalInfo*: rohkem üksikasju kohta ressursi tarbitud, näiteks OS versioon või pildi tüüp |
| *kogus*        | Ressursside tarbimine toimunud selle aja jooksul |
| *meterId*         | Ainu-ID ressurss, mis oli tarbitud (nimetatakse ka kui *ResourceIdkasutamisel*) |

## <a name="next-steps"></a>Järgmised sammud

[Kasutus-seotud KKK](azure-stack-usage-related-faq.md)
