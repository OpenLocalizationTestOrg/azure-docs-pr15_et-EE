<properties
   pageTitle="Asjad, mida parimad Interneti | Microsoft Azure'i"
   description="Artiklis Microsoft Internet asju parimad ja üldised soovitused eelkoostatud loendit."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="StevenPo"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="yurid"/>

# <a name="internet-of-things-security-best-practices"></a>Asjad, mida parimad Internet

Turvaliseks Interneti, asjade taristu on kriitiline keegi kaasatud asjade lahendusi. Seotud seadmete arv ja nende seadmete jaotatud laadi, Turve sündmuse seotud asjade seadmete miljonite kahjustada mõju on ravisaajate ja võib olla levinud mõju.

Seetõttu tuleb asjade turvalisus turve põhjalikud lähenemine. Andmete peab olema turvaline pilves ja hiirekursori üle era- ja avalikud võrgud. Meetodid peavad olema kohas turvaliselt ettevalmistamise asjade seadmete ise. Iga kiht seadmest, võrku Cloud tagaandmebaas peab tugev turvalisus kinnitused.

Asjade head tavad võib liigitada järgmiselt:

- Asjade riistvara tootja või integraator
- Asjade lahendus arendaja
- Asjade lahenduse deployer
- Asjade lahenduse tehtemärk

Selles artiklis antakse ülevaade [Internet, mida turvalisus headest tavadest](../iot-suite/iot-security-best-practices.md). Lugege selle artikli üksikasjalikumat teavet.

## <a name="iot-hardware-manufacturer-or-integrator"></a>Asjade riistvara tootja või integraator

Kui olete soovitud asjade riistvara valmistamine või riistvara integraator, tehke alltoodud põhitõed.

- **Ulatus riistvara miinimumnõuded**: riistvara kujundus peaks sisaldama miinimum, mida on vaja riistvara ja midagi veel. 
- **Tegemine võltsida tõendada riistvara**: koostada korra tuvastamiseks füüsilise võltsimist riistvara, nagu avamine seadme tiitelleht, eemaldades osa seadme jne. 
- **Luua turvalist riistvara ümber**: kui lubate [OMAHIND](https://en.wikipedia.org/wiki/Cost_of_goods_sold) , koostada turbefunktsioonid, nt turvaline ja krüptitud salvestusruumi ja buutimine usaldusväärse platvormi mooduli TPM-põhised funktsioonid.
- **Turvaline uuendused**: üleminekut püsivara seadme elu jooksul on.

## <a name="iot-solution-developer"></a>Asjade lahendus arendaja

Kui olete soovitud asjade lahendus arendaja, tehke alltoodud põhitõed.

- **Jälgi turvaline tarkvara arengu meetod**: arendamise turvalise tarkvara nõuab maa-up mõelda turvalisus projekti algusest lõpuni, et selle rakendamist, testimine ja juurutamine.
- **Valige avage ettevaatusega**: Avage pakub võimalust kiiresti arendada lahendusi.
- **Integreerida ettevaatusega**: tarkvara turvalisuse vigu paljude olemasolevate teekide ja API-d. 

## <a name="iot-solution-deployer"></a>Asjade lahenduse deployer

Kui olete soovitud asjade lahenduse deployer, tehke alltoodud põhitõed.

- **Riistvara turvaline juurutamine**: asjade juurutuste võib olla nõutav riistvara ebaturvaliste asukohtade, nagu näiteks avaliku tühikute või järelevalveta asukohtades juurutatud.
- **Autentimise võtmed turvamiseks**: juurutamisel, iga seadme jaoks on vaja seadme ID-d ja seotud autentimise klahvid loodud pilveteenusesse. Säilita isegi pärast juurutamise füüsilise turvalised need võtmed. Mis tahes rikutud klahvi saab maskeeruda olemasoleva seadme pahatahtlik seade.

## <a name="iot-solution-operator"></a>Asjade lahenduse tehtemärk

Kui olete korraldaja asjade lahenduse, tehke alltoodud põhitõed.

- **Süsteemide ajakohastamine**: Veenduge, et seade operatsioonisüsteemide ja kõik draiverid on värskendatud uusima versiooniga. 
- **Kaitse suhtes pahatahtlik tegevus**: kui operatsioonisüsteem võimaldab, asetage uusima viirusetõrje ja ründevaratõrje võimaluste iga seadme operatsioonisüsteem. 
- **Auditilogi korduma**: auditeerimine asjade taristu turvalisus seotud probleemid on oluline turvalisus juhtumite koosolekukutsetele.
- **Füüsilise kaitse asjade taristu**: kehvem turvalisus eest suhtes asjade taristu käivitatakse füüsilist juurdepääsu seadmete abil.
- **Kaitse cloud identimisteabe**: cloud autentimine mandaat kasutatakse konfigureerimise ja opsüsteem on asjade juurutamise on võib-olla on kõige lihtsam viis juurdepääsu ja kahjustada asjade süsteem. 
