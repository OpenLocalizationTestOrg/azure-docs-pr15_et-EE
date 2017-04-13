<properties 
    pageTitle="Hulgilitsentsimise Microsoft® sujuva voogesituse kliendi teisaldamise komplekt" 
    description="Siit saate teada, kuidas Microsoft® sujuva voogesituse kliendi teisaldamise Kit litsentsimiskeskuse." 
    services="media-services" 
    documentationCenter="" 
    authors="xpouyat,vsood" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016"  
    ms.author="xpouyat"/>

#<a name="licensing-microsoft-smooth-streaming-client-porting-kit"></a>Hulgilitsentsimise Microsoft® sujuva voogesituse kliendi teisaldamise komplekt

##<a name="overview"></a>Ülevaade

Microsoft sujuva voogesituse kliendi teisaldamise Kit (**SSPK** lühikese) on sujuva voogesituse kliendi rakendamist, mis on optimeeritud, et aidata manustatud seadme tootjad, kaabel ja mobiilsideoperaatorid, sisu teenusepakkujatele, telefon tootjad, sõltumatu tarkvara tarnijate (tarkvaratoode) ja lahenduste pakkujad toodete ja teenuste streaming kohandatava tõrgeteta Streaming format streaming sisu loomiseks. SSPK on seade ja platvormi sõltumatu rakendamine sujuva voogesituse klient, saate teisaldatud teostab mis tahes seadme ja platvormi litsentsi omanik. 

Allpool toodud on kõrge taseme arhitektuur ja IIS-i sujuva voogesituse teisaldamise Kit ruut on Microsofti sujuva voogesituse kliendi rakendamist ja sisaldab core loogika taasesituse sujuva voogesituse sisu. See on siis teisaldatud partnerite jaoks kindla seadme või platvormi rakendades sobivat liidesed. 

![SSPK](./media/media-services-sspk/sspk-arch.png)

##<a name="description"></a>Kirjeldus

SSPK litsentsitud tingimustel, mis pakuvad suurepärast äri väärtust. SSPK litsentsi pakub valdkonna abil.

- Sujuva voogesituse teisaldamise Kit allika c++ 
  - rakendab sujuva voogesituse kliendi funktsionaalsus
  - lisab vorming sõelumine, heuristika, puhverdamine loogika jne.
- Pleieri rakendus API-d 
  - suhtlemine media Playeri rakenduse programmeerimise liidesed
- Platvormi võtmiseks kiht (PAL) kasutajaliides 
  - programmeerimine liidesed operatsioonisüsteem (teemad, sockets) suhtlemine
- Riistvara võtmiseks kiht (HAL) kasutajaliides 
  - programmeerimisega liidesed suhtluse riistvaraga A / V dekoodrid (dekodeerimise, muutes)
- Kasutajaliidese digitaalõiguste halduse (DRM) 
  - programmeerimise liidesed käsitlemise DRM kaudu DRM võtmiseks Layer (DAL)
  - Microsoft PlayReady teisaldamise Kit teenusega eraldi, kuid ühendab selle kasutajaliidese kaudu. Microsoft PlayReady Device litsentsimise kohta lisateabe saamiseks klõpsake [siin](http://www.microsoft.com/playready/licensing/device_technology.mspx#pddipdl).
- Rakendamise näited 
  - Kuulake Linuxi PAL rakendamine
  - valimi HAL rakendamiseks GStreamer

##<a name="licensing-options"></a>Hulgilitsentsimise suvandid

Microsoft sujuva voogesituse kliendi teisaldamise Kit tehakse kättesaadavaks kes kaks erinevat litsentsi lepingute raames: üks sujuva voogesituse kliendi ajutised tooted ja teine levitamise lõppkasutajatele sujuva voogesituse kliendi lõplik toodete arendamise.
 
- Kiibistik tootjad, süsteemiintegraatorid või sõltumatu tarkvara tarnijate (tarkvaratoode) andmeallika koodi teisaldamise kit arendada ajutised tooteid vajavate, teostada Microsoft sujuva voogesituse kliendi teisaldamise Kit **Ajutised toote litsentsi** .
- Seadme tootjad või tarkvaratoode vajavate jaotuse õiguste sujuva voogesituse kliendi lõplik toodete lõppkasutajatele, peaks toimuma Microsoft sujuva voogesituse kliendi teisaldamise Kit **Lõpliku toote litsentsi** .

###<a name="microsoft-smooth-streaming-client-porting-kit-interim-product-license"></a>Microsoft sujuva voogesituse kliendi teisaldamise Kit ajutised toote litsentsi

Selle litsentsi, pakub Microsoft sujuva voogesituse kliendi teisaldamise Kit ja vajalikud intellektuaalomandi arendamise ja muude sujuva voogesituse kliendi teisaldamise Kit seadme kes levitamine sujuva voogesituse kliendi lõplik toodete sujuva voogesituse kliendi ajutised toodete jagada.

####<a name="fee-structure"></a>Struktuuri

USA $50.000 ühekordse litsentsi tasu annab juurdepääsu sujuva voogesituse kliendi teisaldamise komplekt. 

###<a name="microsoft-smooth-streaming-client-porting-kit-final-product-license"></a>Microsoft sujuva voogesituse kliendi teisaldamise Kit lõplik toote litsentsi

Selle litsentsi, pakub Microsoft kõik vajalikud intellektuaalomandi saadud muude sujuva voogesituse kliendi teisaldamise Kit kes sujuva voogesituse kliendi ajutised tooted ja levitamine ettevõtte sujuva voogesituse kliendi lõplik toodete lõppkasutajad.

####<a name="fee-structure"></a>Struktuuri

Sujuva voogesituse kliendi lõpliku toote ei pakuta royalty mudelit alusel:

- $0,10 tarniti seadme rakendamise kohta.
- Tasu ülempiir 50 000 iga aasta
- Pole royalty jaoks esmalt 10 000 seadme iga aasta 

##<a name="licensing-procedure-and-sspk-access"></a>Hulgilitsentsimise protseduuri ja SSPK juurdepääsu

Saatke [sspkinfo@microsoft.com](mailto:sspkinfo@microsoft.com) kõigi litsentsimise päringud.

Registreeritud ajutised kes pääseb [SSPK levitamise portaal](https://microsoft.sharepoint.com/teams/SSPKDOWNLOAD/) .

Vahe- ja lõplik SSPK kes saab esitada küsimusi tehnilise [smoothpk@microsoft.com](mailto:smoothpk@microsoft.com).

##<a name="microsoft-smooth-streaming-client-interim-product-agreement-licensees"></a>Microsoft sujuv voogesitamine kliendi ajutised toote lepingu kes

- Osav Business lahendusi, sh
- Täiustatud digitaalne leviedastuse SA
- AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.
- Albis tehnoloogiad Ltd
- Alticast Corporation
- Amazon Digital Services, Inc kaubamärk.
- AVC MMS tarkvara Ltd
- Cavium, Inc kaubamärk.
- EchoStar ostmist Corporation
- Enseo, Inc kaubamärk.
- Fluendo S.A.
- HANDAN BroadInfoCom Ltd
- Infomir GMBH
- Irdeto USA Inc kaubamärk.
- Liberty globaalne teenuste BV
- MediaTek Inc kaubamärk.
- MStar Co, Ltd
- Nintendo, Ltd
- OpenTV, Inc kaubamärk.
- Piiratud safran digitaalne
- Hiina Changhong Electric Co., Ltd
- SoftAtHome
- Sony Corporation
- Tatung tehnoloogia Inc kaubamärk.
- TCL tehnoloogiakomitee elektroonika (Huizhou), Ltd
- Eemalda Vestel Elektronik Sanayi Ticaret A.S.
- VisualOn, Inc kaubamärk.
- ZTE Corporation

##<a name="microsoft-smooth-streaming-client-final-product-agreement-licensees"></a>Microsoft sujuva voogesituse kliendi lõpliku toote lepingu kes

- Täiustatud digitaalne leviedastuse SA
- AirTies Kablosuz Iletism Sanayive Dis Ticaret A.S.
- Albis tehnoloogiad Ltd
- Amazon Digital Services, Inc kaubamärk.
- AmTRAN tehnoloogia Ltd
- Arcadyan Technology Corporation
- ATMACA ELEKTRONİK SAN. EEMALDA TİC. A.Ş
- Briti Sky edastamise piiratud
- Inc kaubamärk Shenzhen CastPal tehnoloogia.
- Compal Electronics Inc
- Dongguan digitaalne AV tehnoloogia Corp, Ltd
- EchoStar ostmist Corporation
- Enseo, Inc kaubamärk.
- Filmflex Filmid piiratud
- Fluendo S.A.
- Piiratud Gibson uuendused
- Haier teabe Applicantion S.R.L
- HANDAN BroadInfoCom Ltd
- Homecast Co., Ltd
- Hon Hai arvutustäpsuse Industry Ltd
- Infomir GMBH
- Kaonmedia Ltd
- KDDI Corporation
- Nintendo, Ltd
- Oranž SA
- Piiratud safran digitaalne
- Sagemcom lairiba SAS
- Shenzhen Coship Electronics CO. LTD
- Shenzhen Jiuzhou Electric Co., Ltd
- Shenzhen Skyworth digitaalne tehnoloogia Co., Ltd
- Hiina Changhong Electric Ltd
- Skardin tööstusliku Corp
- Sky Deutschland Fernsehen GmbH & Co.kg
- SmarDTV S.A.
- SoftAtHome
- Sony Corporation
- Piiratud TCL välismaal turundus (Macau äri Offshore)
- Technicolor kohaletoimetamise tehnoloogiate SAS
- Tongfang globaalne Ltd
- Toshiba elustiili toodete ja teenuste Corporation
- Universaalne meediumi Corporation /Slovakia/ s.r.o.
- VIZIO, Inc kaubamärk.
- Wistron Corporation
- ZTE Corporation

##<a name="media-services-learning-paths"></a>Media Servicesi Õppeteed

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
