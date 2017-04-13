<properties
    pageTitle="Andmete teisaldamine ja sealt Azure Storage | Microsoft Azure'i"
    description="Selles artiklis antakse ülevaade eri meetodid andmete teisaldamine ja sealt Azure Storage."
    services="storage"
    documentationCenter=""
    authors="micurd"
    manager="jahogg"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/21/2016"
    ms.author="micurd"/>

# <a name="moving-data-to-and-from-azure-storage"></a>Andmete teisaldamine ja sealt Azure Storage

Kui soovite teisaldada kohapealseid andmeid Azure Storage (ja vastupidi), on mitmesuguseid viise. Teie jaoks sobivaim lähenemine oleneb stsenaariumist. See artikkel annab kiirülevaate stsenaariumid ning iga vastav pakkumisi.

## <a name="building-applications"></a>Koosteüksuse rakendused

Kui olete taotluse vastu REST API arendamise hoone või ühte meie mitme kliendi teegid on hea viis andmete teisaldamine ja sealt Azure Storage.

Azure'i salvestusruumi pakub rikkaliku kliendi teegid .net-i, iOS-i, Java, Android, Universal platvormil (UWP), Xamarin, C++, Node.JS, PHP, Ruby ja Python. Kliendi teegid pakkuda täpsemalt võimalusi, nt proovi uuesti loogikat, logimine ja paralleelsete üles. Saate arendada ka otse vastu REST API, mille saab kutsuda mis tahes keeleks, mis toodab HTTP/HTTPS päringuid.

Vaadake lisateavet [Azure'i bloobimälu alustamine](storage-dotnet-how-to-use-blobs.md) .

Lisaks pakume ka [Azure salvestusruumi andmete liikumine teegi](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement) , on mõeldud suure jõudlusega kopeerimise andmete Azure'i teeki. Vaadake meie andmete liikumine teegi [dokumentatsiooni kohta](https://github.com/Azure/azure-storage-net-data-movement) lisateavet. 

## <a name="quickly-viewinginteracting-with-your-data"></a>Kiiresti vaatamise ja andmetega töötamine oma

Kui soovite lihtne viis oma Azure Storage andmete võttes võimalus üles- ja allalaadimine andmete kuvamiseks, siis kaaluge Azure'i salvestusruumi Explorer.

Vaadake meie [Azure'i salvestusruumi maadeavastajad](storage-explorers.md) Lisateavet loend.

## <a name="system-administration"></a>Süsteemi haldamine

Kui vajate või on mugavam käsurea kasuliku (nt süsteemi administraatorid), on teile silmas pidada mõni järgmised võimalused.

### <a name="azcopy"></a>AzCopy

AzCopy on mõeldud suure jõudlusega kopeerimise andmete Azure Storage Windows käsurea kasuliku. Samuti saate kopeerida salvestusruumi konto või muu salvestusruumi kontode vahel andmete.

Lisateavet vt [AzCopy käsurea kasuliku andmete edastamiseks](storage-use-azcopy.md) .

### <a name="azure-powershell"></a>Azure'i PowerShelli

Azure'i PowerShelli on moodul, mis pakub haldamise teenuste Azure cmdlet-käsud. See on toimingupõhise käsurea kesta ja skriptimiskeele, mis on mõeldud eelkõige süsteemi haldamine.

Teemast [Azure PowerShelli kasutamine Azure Storage](storage-powershell-guide-full.md) lisateavet.

### <a name="azure-cli"></a>Azure'i CLI

Azure'i CLI pakub avatud allikas komplekti platvormidel käsud töötamise Azure teenused. Azure'i CLI on saadaval Windows, OSX ja Linux.

Vaadake lisateavet [Azure CLI Azure Storage abil](storage-azure-cli.md) .

## <a name="moving-large-amounts-of-data-with-a-slow-network"></a>Suurte andmehulkade koos aeglase võrguühenduse teisaldamine

Üks suurim suurte andmehulkade teisaldamisega seotud probleeme on edastamine aeg. Kui soovite saada andmeid Azure Storage ilma muretsema võrkude kulud või koodi, siis Azure impordi/ekspordi on sobiva lahenduse.

Vaadake lisateavet [Azure impordi/ekspordi](storage-import-export-service.md) .

## <a name="backing-up-your-data"></a>Andmete varundamine

Kui soovite lihtsalt varundada oma andmeid ja Azure Storage, Azure varukoopia on tee minna. See on võimas lahendus kohapealsed andmed ja Azure VMs varundada.

Vaadake lisateavet [Azure varukoopia](../backup/backup-introduction-to-azure-backup.md) .

## <a name="accessing-your-data-on-premises-and-from-the-cloud"></a>Juurdepääs oma andmeid asutusesiseselt ja pilveteenuses

Kui teil on vaja lahenduse juurdepääsuks oma andmeid asutusesiseselt ja pilvest, siis võiksite kasutada Azure hübriid pilve salvestusruumi lahendus StorSimple. See lahendus koosneb füüsilise StorSimple seadme mida arukalt poed sageli kasutatud andmete SSD, aeg-ajalt kasutatud andmete HDD ja Azure Storage passiivne/varundamise/arhiveerimise andmeid.

Vaadake lisateavet [StorSimple](../storsimple/storsimple-overview.md) .

## <a name="recovering-your-data"></a>Andmete taastamine

Kui teil on kohapealse töökoormus ja rakendused, peate lahendus, mis võimaldab teie ettevõtte jätkamiseks õnnetuste töötab. Azure'i saidi taastamine tegeleb dispersioonanalüüs, Tõrkesiirde ja virtuaalmasinates ja füüsilise serveri. Kopeeritud andmed on salvestatud Azure Storage, mis võimaldab kõrvaldada teisene kohapeal andmekeskuse.

Vaadake lisateavet [Azure saidi taastamine](../site-recovery/site-recovery-overview.md) .
