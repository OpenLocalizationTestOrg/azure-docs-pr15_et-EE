<properties
   pageTitle="Asjade Interneti turvalisuse ülevaade | Microsoft Azure'i"
   description=" Azure'i internet asjade teenuste pakub mitmesuguseid võimalusi. See artikkel aitab teil mõista, kuidas kaitsta oma asjade lahenduste Azure. "
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/09/2016"
   ms.author="terrylan"/>

# <a name="internet-of-things-security-overview"></a>Asjade Interneti turvalisuse ülevaade

Azure'i internet asjade teenuste pakub mitmesuguseid võimalusi. Need enterprise hinde teenused võimaldavad:

- Andmete kogumine seadmed
- Andmete voogu liikumise analüüs
- Salvestamine ja päringu suurte andmekogumite
- Reaalaja- ja ajalooliste andmete visualiseerimine
- Tagasi-Office'i süsteemide integreerimine

Esitamisel järgmisi võimalusi, Azure'i asjade komplekti paketid koos mitme Azure'i teenuste kohandatud laiendiga eelkonfigureeritud lahendusi. Need eelkonfigureeritud lahendused on levinud asjade lahenduse mustreid, mis aitab vähendada oma asjade lahenduste ajal base rakendusi. Kasutades asjade tarkvara arengu komplektid, saate kohandada ja laiendada oma nõuete täitmiseks järgmisi lahendusi. Saate kasutada järgmisi lahendusi näiteid või mallide väljatöötamisel uusi asjade lahendusi.

Azure'i asjade suite on võimas lahendus asjade teie vajadustele. See aga upmost oluline, et teie asjade lahendused on mõeldud turvalisust silmas pidades algusest. Tõttu asjade seadmete arv, mis tahes turvalisus juhtum saate kiiresti muutuvad on levinud oluline sündmus.

Et aidata teil mõista, kuidas kaitsta oma asjade lahendusi, meil järgmine teave.

## <a name="security-architecture"></a>Turbearhitektuur

Süsteemi kujundamisel oluline mõista süsteemi võimalike ohtudega ja lisage sobiv kaitset vastavalt sellele, süsteemi on loodud ning architected. See on eriti oluline kujundada toote algusest turvalisust silmas pidades, kuna mõistmine aitab kuidas ründaja võib süsteemi kahjustada kindel vastav kergendamise on olemas algusest.

Asjade turbearhitektuur kohta leiate teavet [Interneti asju turbearhitektuur](../iot-suite/iot-security-architecture.md)lugedes.

Selles artiklis käsitletakse järgmisi teemasid:

- [Turvalisus algab ohtu mudeli](../iot-suite/iot-security-architecture.md#security-starts-with-a-threat-model)
- [Asjade Turve](../iot-suite/iot-security-architecture.md#security-in-iot)
- [Ohtu modelleerimine Azure asjade viide arhitektuur](../iot-suite/iot-security-architecture.md#threat-modeling-the-azure-iot-reference-architecture)

## <a name="security-from-the-ground-up"></a>Tühjalt kohalt turvalisus

Soovitud asjade tekitab kordumatu turvalisus, privaatsus ja vastavus probleeme ettevõtetele kogu maailma ulatuses. Erinevalt traditsiooniline cyber tehnoloogia, kus järgmiste probleemide korral toetuvad tarkvara ja kuidas seda rakendatakse, puudutab asjade, mis juhtub, kui selle cyber ja füüsilise maailma lähenevad. Kaitsmine asjade lahenduste jaoks on vaja tagada turvaline ettevalmistamise seadmete turvaline Ühenduvus järgmistesse seadmetesse ja pilveteenuse, ja turvaline andmekaitse pilveteenuses andmetöötlus ja ajal. Töö selliste funktsioonide vastu, on siiski ressurssidega seadmed, geograafilised jaotuse juurutuste ja suure hulga seadmete lahenduse sees.

Saate teada, kuidas käsitlema neid valdkondi tuleks turvalisuse lugedes [asjade Internet security kohalt kaudu](../iot-suite/securing-iot-ground-up.md).

Artiklis käsitletakse järgmisi teemasid:

- [Turvaline taristu kohalt kaudu](../iot-suite/securing-iot-ground-up.md#secure-infrastructure-from-the-ground-up)
- [Microsoft Azure'i – turvaline asjade taristu oma ettevõtte jaoks](../iot-suite/securing-iot-ground-up.md#microsoft-azure---secure-iot-infrastructure-for-your-business)

## <a name="best-practices"></a>Head tavad

Asjade infrastruktuur turvaliseks vaja range turvalisus põhjalikud strateegiat. Andmekaitse pilves, kaitsmine andmetervikluse liikudes avaliku Interneti kaudu ja mis võimaldab turvaliselt ettevalmistamise seadmed, alates iga kiht koostab suurem turvalisus assurance üldise infrastruktuuri.

Te saate turvalisusest asjade Interneti head tavad lugedes [asjade Internet security põhitõed](../iot-suite/iot-security-best-practices.md).

Artiklis käsitletakse järgmisi teemasid:

- [Asjade riistvara tootja/integraator](../iot-suite/iot-security-best-practices.md#iot-hardware-manufacturerintegrator)
- [Asjade lahendus arendaja](../iot-suite/iot-security-best-practices.md#iot-solution-developer)
- [Asjade lahenduse deployer](../iot-suite/iot-security-best-practices.md#iot-solution-deployer)
- [Asjade lahenduse tehtemärk](../iot-suite/iot-security-best-practices.md#iot-solution-operator)
