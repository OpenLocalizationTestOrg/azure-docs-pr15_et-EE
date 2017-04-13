<properties
   pageTitle="Azure'i ressursihaldur taotluse piirangud | Microsoft Azure'i"
   description="Kirjeldab, kuidas kasutada pidurdamise Azure'i ressursihaldur taotlusi, kui tellimus limiidid on täis."
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="tomfitz"/>

# <a name="throttling-resource-manager-requests"></a>Ressursihaldur taotlusi ahendamine

Iga tellimuse ja rentniku, ressursihaldur piirangud taotluste 15000 tunnis lugeda ja kirjutada taotlusi 1200 tunnis. Kui teie rakendus või skripti jõuab need piirangud, peate oma taotlusi throttle. Selles teemas kirjeldatakse, kuidas kindlaks teha, peate enne piirangu jõudmist ülejäänud taotluste ja kuidas vastata, kui olete jõudnud limiit.

Kui jõuate limiit, kuvatakse HTTP olekukoodi **429 liiga palju taotlusi**.

Arv on rakendatud tellimuse või teie rentniku. Kui teil on mitu, samaaegseid taotlusi tegemist teie tellimus rakenduste lisatakse need rakendused päringutele koos ülejäänud taotluste arvu määramiseks.

Tellimuste rakendatud on need kaasata, läbides oma tellimuse id, näiteks toomine ressursi pakuvad teie tellimus. Rentniku veebirakendust kutsed ei sisalda oma tellimuse id, näiteks lubatud Azure asukohtade toomine.

## <a name="remaining-requests"></a>Ülejäänud taotluste

Saate määratleda ülejäänud taotluste arv uurides vastus päised. Iga taotlus sisaldab väärtuste arvu ülejäänud lugemine ja kirjutamine taotlused. Järgmises tabelis kirjeldatakse vastus päised, saate uurida nende väärtuste jaoks.

| Vastuse päis | Kirjeldus |
| --------------- | ----------- |
| x-MS-ratelimit-remaining-Subscription-reads | Tellimuse veebirakendust loeb allesjäänud |
| x-MS-ratelimit-remaining-Subscription-writes | Tellimuse veebirakendust kirjutab allesjäänud |
| x-MS-ratelimit-remaining-tenant-reads | Rentniku veebirakendust loeb allesjäänud |
| x-MS-ratelimit-remaining-tenant-writes | Rentniku veebirakendust kirjutab allesjäänud |
| x-MS-ratelimit-remaining-Subscription-Resource-requests | Tellimuse rakendatud ressursi tüüp taotlusi allesjäänud.<br /><br />See päis väärtus tagastatakse ainult kui teenus on vaikimisi alistada. Ressursihaldur lisab selle asemel tellimuse loeb või kirjutab väärtuse. |
| x-MS-ratelimit-remaining-Subscription-Resource-Entities-read | Tellimuse rakendatud ressursi tüüp saidikogumi taotlusi allesjäänud.<br /><br />See päis väärtus tagastatakse ainult kui teenus on vaikimisi alistada. See väärtus pakub ülejäänud saidikogumi taotlusi (loendi ressursid). |
| x-MS-ratelimit-remaining-tenant-Resource-requests | Rentniku rakendatud ressursi tüüp taotlusi allesjäänud.<br /><br />See päis lisatakse ainult taotluste rentniku tasemel ja ainult siis, kui teenus on alistada vaikesäte limiit. Ressursihaldur lisab selle asemel rentniku loeb või kirjutab väärtuse. |
| x-MS-ratelimit-remaining-tenant-Resource-Entities-read | Rentniku rakendatud ressursi tüüp saidikogumi taotlusi allesjäänud.<br /><br />See päis lisatakse ainult taotluste rentniku tasemel ja ainult siis, kui teenus on alistada vaikesäte limiit. |

## <a name="retrieving-the-header-values"></a>Päise väärtused toomine

Need päise väärtused koodi või skripti toomine ei erine päise väärtuse toomine. 

Näiteks **C#**, saate tuua päise väärtus **HttpWebResponse** objekti nimega **vastus** järgmine kood:

    response.Headers.GetValues("x-ms-ratelimit-remaining-subscription-reads").GetValue(0)

**PowerShelli**tuua päise väärtus Invoke-WebRequest toiming.

    $r = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/{guid}/resourcegroups?api-version=2016-09-01 -Method GET -Headers $authHeaders
    $r.Headers["x-ms-ratelimit-remaining-subscription-reads"]
    
Või kui soovite vaadata ülejäänud taotluste silumine, saate sisestada soovitud **-silumine** **PowerShelli** cmdlet-käsu parameeter.

    Get-AzureRmResourceGroup -Debug
    
Mis annab hulgaliselt teavet, sh vastuse järgmine väärtus:

    ...
    DEBUG: ============================ HTTP RESPONSE ============================

    Status Code:
    OK

    Headers:
    Pragma                        : no-cache
    x-ms-ratelimit-remaining-subscription-reads: 14999
    ...

**Azure'i CLI**, saate tuua päise väärtus veel Paljusõnaline suvandi abil.

    azure group list -vv --json

Mis annab hulgaliselt teavet, sh järgmised objekti:

    ...
    silly: returnObject
    {
      "statusCode": 200,
      "header": {
        "cache-control": "no-cache",
        "pragma": "no-cache",
        "content-type": "application/json; charset=utf-8",
        "expires": "-1",
        "x-ms-ratelimit-remaining-subscription-reads": "14998",
        ...

## <a name="waiting-before-sending-next-request"></a>Enne järgmise kutse saatmist ootamine

Kui jõuate taotluse limiit, ressursihaldur tagastab **429** HTTP olekukoodi ja **Proovige uuesti pärast** väärtuse päis. Selle **Uuesti pärast** väärtus määrab, mitu sekundit rakenduse ootama (või magada) enne järgmise kutse saatmist. Kui saadate taotluse enne proovi uuesti väärtuse, ei töödelda teie taotlus ja tagastatakse uue proovi uuesti väärtuse.
