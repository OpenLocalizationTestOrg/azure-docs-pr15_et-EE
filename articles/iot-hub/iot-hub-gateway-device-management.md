<properties
 pageTitle="Hallatavate seadmete taga mõnda asjade lüüsi | Microsoft Azure'i"
 description="Juhiseid teemas on asjade lüüsi abil loodud asjade lüüsi SDK koos seadmete haldab asjade jaoturi abil."
 services="iot-hub"
 documentationCenter=""
 authors="chipalost"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="cstreet"/>
 
# <a name="enable-managed-devices-behind-an-iot-gateway"></a>Hallatavate seadmete soovitud asjade lüüsi taha

## <a name="basic-device-isolation"></a>Põhilised eraldamise

Ettevõtted sageli kasutada asjade lüüside suurendamiseks nende asjade lahendused üldine turvalisus. Mõne seadme peate pilveteenuses andmeid saata, kuid ei võimalda kaitsta end Internetis ohtude. Järgmised seadmed: välised saate kaitsta, nende muu maailma lüüsi kaudu suhtlemiseks.

Lüüsi asub piiri turvalises keskkonnas ja avage internet. Lüüsi räägi seadmed ja lüüsi edastab sõnumite mööda õige cloud sihtkohta. Lüüsi on välised vastu kõva, blokeerib volitamata taotlusi, võimaldab volitatud sisse seotud liikluse ja edastab selle sisse seotud liikluse õige seade.

![][1]

Saate lisada ka seadmete kaitsta end taha lüüsi lisatakse kiht turvalisus. Sel juhul peate ainult hoida selliste uusimate nõrkuste, selle asemel, et värskendada iga seadme OS paigatud OS lüüsi.

## <a name="isolation-plus-intelligence"></a>Eraldamise pluss ärianalüüsi

Kõva ruuteri on piisavalt lüüsi eristamiseks lihtsalt seadmed. Siiski asjade lahendused eeldavad, et lüüsi pakub rohkem kui lihtsalt eraldamiseks seadmete ärianalüüsi. Näiteks võite oma seadmete haldamine pilveteenuses. Teil on kasutada [LWM2M](https://github.com/OpenMobileAlliance/OMA_LwM2M_for_Developers/wiki), seadme haldamise protokolli, cloud halduse osa lahendus. Siiski seadmete saata, mitte TCP/IP kaudu telemeetria lubatud Protocol (protokoll). Lisaks seadmete aedvili palju andmeid ning soovite üles laadida telemeetria filtreeritud alamhulga. Saate koostada lahenduse, mis sisaldab asjade lüüsi, mis on võimalik tegeleda kaks erinevat voole andmed. Lüüsi tuleks:

-   **Telemeetria**mõista, filtreerida ja laadige lüüsi kaudu pilveteenusesse. Lüüsi pole enam lihtne ruuteri, mis lihtsalt edastab andmete seade ja pilveteenuse vahel.

-   Lihtsalt vahetada **LWM2M seadme haldamine andmed** seadmete ja pilveteenuse vahel. Lüüsi ei pruugi mõista sinna andmete ja ainult peab veenduge, et andmeid saab edasi ja tagasi seadmete ja pilveteenuse vahel.

Järgmine joonis kujutab selle stsenaariumi:

![][2]

## <a name="the-solution-azure-iot-device-management-and-the-iot-gateway-sdk"></a>Lahendus: Azure'i asjade mobiilsideseadmete halduse ja asjade lüüsi SDK 

Avalikkusele eelvaate versiooni [Azure'i asjade mobiilsideseadmete halduse] [ lnk-device-management] ja [Azure asjade lüüsi SDK] beetaversiooni lubamine seda stsenaariumi. Lüüsi tegeleb iga andmevoo järgmiselt:

-   **Telemeetria**: asjade lüüsi SDK abil saate koostada kohaletoimetamisel, mis mõistab, filtrid ja saadab telemeetria andmete pilve. Asjade lüüsi SDK pakub kood, mis rakendab osa selle nimel arendaja kohaletoimetamisel. Leiate lisateavet arhitektuur SDK [Asjade lüüsi SDK - alustamine] [ lnk-gateway-get-started] õpetuse.

-   **Seadme haldamine**: Azure'i mobiilsideseadmete halduse pakub LWM2M klient, mis töötab seadme kui ka pilveteenuses kasutajaliidese väljastamise haldamise käske seadmele.
    
    Mis tahes teisiti loogika lüüsi ei nõua, kuna see ei vaja LWM2M andmete seade ja teie asjade jaoturi vahetavad. Saate lubada Interneti-ühenduse ühiskasutus, funktsioon paljude operatsioonisüsteemides, lüüsi LWM2M andmevahetuse lubamiseks. Selle stsenaariumi operatsioonisüsteem on sobiv saate valida, kuna asjade lüüsi SDK toetab paljusid operatsioonisüsteemid. Siit leiate juhised Interneti-ühenduse ühiskasutusel opsüsteemis [Windows 10] ja [Ubuntu], kaks paljude toetatud operatsioonisüsteemide.

Järgmisel joonisel on kujutatud kõrge taseme arhitektuur kasutada seda stsenaariumi, kasutades [Azure asjade mobiilsideseadmete halduse] [ lnk-device-management] ja [Azure asjade lüüsi SDK].

![][3]

## <a name="next-steps"></a>Järgmised sammud

Asjade lüüsi SDK kasutamise kohta leiate teemast järgmised õpetused:

- [Asjade lüüsi SDK - Linuxi kasutamise alustamine][lnk-gateway-get-started]
- [Asjade lüüsi SDK-sõnumite saatmine seadmesse pilve Linuxi jäljendatud seadmega][lnk-gateway-simulated]

Asjade jaoturi võimaluste täpsemaks analüüsimiseks järgmistest teemadest.

- [Arendaja juhend][lnk-devguide]
- [Simuleerida seadme asjade lüüsi SDK][lnk-gateway-simulated]

<!-- Images and links -->
[1]: media/iot-hub-gateway-device-management/overview.png
[2]: media/iot-hub-gateway-device-management/manage.png
[Azure'i asjade lüüsi SDK]: https://github.com/Azure/azure-iot-gateway-sdk/
[Windows 10]: http://windows.microsoft.com/en-us/windows/using-internet-connection-sharing#1TC=windows-7
[Ubuntu]: https://help.ubuntu.com/community/Internet/ConnectionSharing
[3]: media/iot-hub-gateway-device-management/manage_2.png
[lnk-gateway-get-started]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-gateway-simulated]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-devguide]: iot-hub-devguide.md