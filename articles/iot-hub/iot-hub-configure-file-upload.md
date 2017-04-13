<properties
     pageTitle="Azure portaali abil saate konfigureerida faili üleslaadimine | Microsoft Azure'i"
     description="Ülevaade sellest, kuidas konfigureerida faili üleslaadimisel Azure'i portaalis"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="configure-file-uploads-using-the-azure-portal"></a>Faili üleslaadimiseks Azure'i portaalis konfigureerimine

## <a name="file-upload"></a>Faili üleslaadimine

[Faili üleslaadimine asjade keskuses funktsioonide]kasutamiseks[lnk-upload], peate esmalt seostada oma jaoturi Azure Storage konto. Valige **faili üles laadida** faili üleslaadimine atribuutide asjade jaoturi, mis muudetakse loendi kuvamiseks sätted.

![][13]

**Salvestusruumi container**: valige Azure Storage konto oma asjade keskuses seostada tellimuse praeguse Azure'i bloobimälu ümbris Azure portaali abil. Vajaduse korral saate luua **salvestusruumi kontod** blade ja bloobimälu ümbris **ümbriste** enne Azure Storage konto. Asjade jaoturi loob automaatselt SAS URI-d kirjutamine õigustega see bloobimälu ümbris kasutada faile üles laadida seadmete jaoks.

![][14]

**Teatiste vastuvõtmine üleslaaditud failid**: lubada või keelata faili üleslaadimine teatised tumblernupu kaudu.

**SAS TTL**: see säte on selle aja-to-live asjade jaoturi seadmele tagastatud SAS URI-d. Üks tund vaikimisi, kuid saab kohandada muude väärtuste liugurit.

**Faili teatise sätteid default TTL**: selle aja-to-live faili üleslaadimine teatise enne, kui see on aegunud. Määratud üks päev vaikimisi, kuid saab kohandada muude väärtuste liugurit.

**Faili teatis maksimaalne arv**: mitu korda asjade jaoturi üritab faili üleslaadimine teatis. 10 vaikimisi, kuid saab kohandada muude väärtuste liugurit.

![][15]

## <a name="next-steps"></a>Järgmised sammud

Asjade jaoturi faili üleslaadimine võimaluste kohta lisateabe saamiseks vaadake teemat [seadmest faile üles laadida] [ lnk-upload] arendaja juhendist.

Järgige neid linke Azure'i asjade jaoturi haldamise kohta lisateabe saamiseks:

- [Hulgi asjade seadmete haldamine][lnk-bulk]
- [Kasutus mõõdikud][lnk-metrics]
- [Toimingute jälgimine][lnk-monitor]

Asjade jaoturi võimaluste täpsemaks analüüsimiseks järgmistest teemadest.

- [Arendaja juhend][lnk-devguide]
- [Simuleerida seadme asjade lüüsi SDK][lnk-gateway]
- [Turvaline põhjusel teie asjade lahendus üles][lnk-securing]


  [13]: ./media/iot-hub-configure-file-upload/file-upload-settings.png
  [14]: ./media/iot-hub-configure-file-upload/file-upload-container-selection.png
  [15]: ./media/iot-hub-configure-file-upload/file-upload-selected-container.png

[lnk-upload]: iot-hub-devguide-file-upload.md

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md