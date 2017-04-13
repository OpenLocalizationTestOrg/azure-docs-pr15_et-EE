<properties 
    pageTitle="Azure RemoteApp võrgu läbilaskevõime - üldised juhised | Microsoft Azure'i"
    description="Mõista mõne lihtsa võrgu läbilaskevõime juhised oma Azure RemoteApp saidikogumite ja rakendused."
    services="remoteapp"
    documentationCenter="" 
    authors="lizap" 
    manager="mbaldwin" />

<tags 
    ms.service="remoteapp" 
    ms.workload="compute" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/15/2016" 
    ms.author="elizapo" />
    
# <a name="azure-remoteapp-network-bandwidth---general-guidelines-if-you-cant-test-your-own"></a>Azure RemoteApp võrgu läbilaskevõime - üldised juhised (kui te ei saa Testige oma)

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Kui teil pole aega ega võimalus käitatavad [võrgu läbilaskevõime testide](remoteapp-bandwidthtests.md) Azure RemoteApp, siin on mõned üsna üldised juhised, mis aitavad teil prognoosimiseks võrgu läbilaskevõime kasutaja kohta.

Kui teil on need stsenaariumid segu, me ei soovita midagi väiksem kui (või võrdne) 10 MB s nimega miinimum võrgu läbilaskevõime tänapäevane Interneti-ühendusega rakenduste kaugtöölaua keskkonnas. (Kuigi nagu, see ei garanteeri parem kui keskmine kasutuskogemuse.)

## <a name="complex-powerpoint-simple-powerpoint"></a>Keerukate PowerPointi lihtsa PowerPoint

Azure RemoteApp ei parim LAN 100 MB. Kui jitter kohal 120 ms on rohkem kui 5% 10 MB/s võrgu profiili näevad selle kasutaja Keskmine kogemus. 1 MB/s eri on silmipimestav – näiteks slaidiseansis, kasutaja ei pruugi näha animeeritud üleminekud üldse Kuna paneelide vahele.

## <a name="internet-explorer-mixed-pdf-pdf-text"></a>Internet Exploreris segatud PDF-faili, PDF-, teksti

10 MB/s võrgu profiili lähedane LAN enamik aspekte. 1 MB/s annab OK kogemus, kuigi võib olla mõni jitter kasutaja kerimise ajal ekraanil on pilti.

## <a name="flash-video-youtube"></a>Flash-video (YouTube)

100 MB/s LAN pakub parima kasutuskogemuse saamiseks 10 MB/s on aktsepteeritav (st paneeli määr kursis, kuid värin tõus). 1 MB/s jitter on väga kõrge ja märgatav.

## <a name="word-typing-word-remote-input"></a>Wordi tippimist (Word remote sisend)
See on väikese läbilaskevõimega kasutus stsenaariumi. 256 KB/s Pakume nii hea kogemus kui LAN.

## <a name="learn-more"></a>Lisateave
- [Azure RemoteApp võrgu läbilaskevõime kasutuse prognoosimiseks](remoteapp-bandwidth.md)

- [Azure RemoteApp – kuidas teha võrgu läbilaskevõime ja -kvaliteeti ning kogemusi töö koos?](remoteapp-bandwidthexperience.md)

- [Azure RemoteApp - tseting oma võrgu läbilaskevõime kasutuse mõne levinud stsenaariumi](remoteapp-bandwidthtests.md)