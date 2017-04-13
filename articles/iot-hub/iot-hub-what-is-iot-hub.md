<properties
 pageTitle="Azure'i asjade jaoturi ülevaade | Microsoft Azure'i"
 description="Azure'i asjade jaoturi teenuse ülevaade: mis on asjade jaoturi, seadme Ühenduvus, Interneti asju side mustrite ja teenuse abil side muster"
 services="iot-hub"
 documentationCenter=""
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="08/25/2016"
 ms.author="dobett"/>

# <a name="what-is-azure-iot-hub"></a>Mis on Azure asjade jaoturi?

Tere tulemast Azure asjade jaoturi. Selles artiklis antakse ülevaade sellest, Azure'i asjade jaoturi ja kirjeldatakse, miks peaksin kasutama see teenus kasutusele Internet, asjade lahenduse. Azure'i asjade jaoturi on täielikult hallatav teenus, mis võimaldab turvalist kahesuunaline miljonid asjade seadmed ja lahenduse tagasi end vahelise suhtluse. Azure'i asjade jaoturi:

- Pakub usaldusväärne seadme pilve ja pilveteenuse-et-seade sõnumside tasandil.
- Lubab turvaline side seadme kohta turvalisus mandaadi abil ja juurdepääsu juhtimine.
- Pakub ulatusliku järelevalve seadme Ühenduvus ja seadme identiteedi haldamine sündmuste jaoks.
- Sisaldab seadme teekide jaoks kõige populaarsemate keelte ja platvormide jaoks loodud.

Artikli [asjade jaoturi võrdlus ja sündmuse jaoturi] [ lnk-compare] kirjeldatakse olulisi erinevused nende kahe teenuste ja tõstetakse teie asjade Solutions asjade jaoturi kasutamise eelised.

![Azure'i asjade jaoturi nimega cloud lüüsi Interneti asju lahenduse][img-architecture]

> [AZURE.NOTE] Asjade arhitektuuri põhjalik arutelu, leiate teemast [Microsoft Azure'i asjade viide arhitektuuri][lnk-refarch].

## <a name="iot-device-connectivity-challenges"></a>Asjade seadme-ühenduvuse probleemid

Asjade jaoturi ja seadme teekide abil saate vastata ühendamiseks usaldusväärselt ja turvaliselt seadmete lahenduse tagasi lõppu. Asjade seadmed:

- Sageli manustatud süsteemid pole inimeste tehtemärki.
- Saab remote asukohtades, kus on kallis füüsilise juurdepääsu.
- Ainult võib olla lahenduse tagasi lõpuks kaudu kättesaadav.
- Võivad olla piiratud power ja töötlemine ressursid.
- Võib-olla vahelduva, aeglane või kallis võrguühendus.
- Võib-olla vaja kasutada kaitstud, kohandatud või Valdkonnakohased rakenduse protokollid.
- Saate luua suure hulga populaarsed riist- ja tarkvara platvormid abil.

Lisaks ülaltoodud nõuetele asjade lahenduse, tuleb ka oma skaala, turvalisus ja usaldusväärsus. Tulemuseks ühenduvuse nõuded on raske ja aeganõudvamaks juhul rakendada, kui kasutate traditsiooniline tehnoloogiaid, nagu web ümbriste ja sõnumside maaklerid.

## <a name="why-use-azure-iot-hub"></a>Miks kasutada Azure asjade jaoturi

Azure'i asjade jaoturi aadressid seadme-ühenduvuse probleeme järgmisel viisil:

-   **Seadme kohta autentimis- ja turvaline**. Te saate ettevalmistamise iga seadmega oma [turvalisuse võti] [ lnk-devguide-security] asjade jaoturi ühenduse lubamiseks. [Asjade jaoturi identiteedi registri] [ lnk-devguide-identityregistry] talletab lahenduse seadme identiteedid ja võtmed. Lahendus tagasi end saate lisada üksikute seadmete lubamiseks või keelamiseks loendite lubamiseks seadme juurdepääsu üle täielik kontroll.

-   **Seadme ühenduvuse toimingute jälgimine**. Saate üksikasjalikku Logi seadme identiteedi toimingute ja seadme ühenduvuse sündmuste kohta. See jälgimise funktsioon lubab tuvastamiseks ühenduvusprobleemide, näiteks ühendust valesti mandaadiga seadmed sõnumite saatmiseks liiga sageli või Hülga kõik pilve-seadme sõnumid teie asjade lahendus.

-   **Seadme teekide hulgaliselt**. [Azure'i asjade seadme SDK-d] [ lnk-device-sdks] on saadaval ja erinevates keeltes ja platvormid--C palju Linuxi, Windowsi ja reaalajas operatsioonisüsteemide jaoks toetatud. Azure'i asjade seadme SDK-d toetama hallatavate tekst, nt C#, Java ja JavaScripti.

-   **Asjade protokollid ja laiendatavuse**. Kui teie lahendus ei saa kasutada seadme teekide, seab asjade jaoturi avaliku protokoll, mis võimaldab kasutada algupäraselt MQTT v3.1.1, HTTP 1.1 või AMQP 1.0 Protokollid seadmed. Samuti saate laiendada asjade jaoturi toetada kohandatud protokollid:

    - [Azure'i asjade lüüsi SDK] välja lüüsi loomine[ lnk-gateway-sdk] mis teisendab oma kohandatud protokolli ühe asjade jaoturi mõista kolme protokolli. 
    - [Azure'i asjade protokoll lüüsi]kohandamise[protocol-gateway], avatud allika komponent, mis töötab pilveteenuses.

-   **Skaala**. Azure'i asjade jaoturi skaala miljoneid korraga ühendatud seadmete ja sündmuste sekundis miljoneid.

Need eelised on üldise palju side mustrid. Asjade jaoturi praegu võimaldab teil rakendada järgmine teatis mustrite.

-   **Sündmus põhinev seadme pilve manustamisest.** Asjade jaoturi usaldusväärselt saate sündmuste sekundis miljoneid oma seadmetes. On võimalik siis protsessi neid oma kuum tee sündmuse protsessor mootori abil. Selle saab salvestada need oma analüüsi jaoks külma teed. Asjade jaoturi säilitab sündmuse andmed kuni seitse päeva selleks, et tagada usaldusväärne töötlemise ja ta peaks koormus.

-   * *Usaldusväärne cloud-seadme sõnumside (või *käsud*). ** lahenduse tagasi lõpuks saate kasutada asjade jaoturi on vähemalt-korraga kohaletoimetamise garantii sõnumeid saata üksikuid seadmed. Iga sõnumi on üksikud aja live säte ja tagasi lõpuks saate taotleda kohaletoimetamise-ja aegumise. Nende teatised tagada täielik nähtavus pilve-seadme sõnumi elutsükli. Seejärel saate rakendada äriloogika, mis sisaldab toiminguid, mis töötavad seadmed.

-   **Failide ja andmete vahemällu talletatud andur üleslaadimiseks pilve.** Seadmetes saate faile üles laadida Azure Storage abil saate hallata asjade jaoturi SAS URI-d. Asjade jaoturi saate luua teatised, et lubada tagasi lõpuks töödelda neid faile saabumisest.

## <a name="gateways"></a>Lüüside

Lüüsi lahenduse asjade on tavaliselt kas [protokolli lüüsi] [ lnk-gateway] mis on pilv või [välja lüüsi] juurutatud[ lnk-field-gateway] mis on juurutatud kohalikult seadmetes. Lüüsi protokolli teostab protokolli tõlke, näiteks MQTT AMQP abil. Välja lüüsi saate käivitada analytics serva, kiirete otsuste vähendada latentsus, seadme halduse teenuste turvalisuse ja privaatsuse piiranguid ja teha ka protokoll tõlge. Mõlemat tüüpi lüüsi toimivad töörühm oma seadmed ja oma asjade jaoturi vahel.

Välja lüüsi erineb lihtsa liikluse marsruutimise seadet (nt võrgu aadress tõlge seadme või tulemüür), kuna tehtav tavaliselt aktiivselt haldamisel Accessi ja teavet teie lahendus liikumist.

Lahendus võib sisaldada nii protokoll ja välja lüüsid.

## <a name="how-does-iot-hub-work"></a>Asjade jaoturi tööpõhimõtted

Azure'i asjade jaoturi rakendab [teenuse abil side] [ lnk-service-assisted-pattern] mustri olla vahendajaks seadmetest ja teie lahendus tagasi end. Eesmärk on side teenuse abil luua usaldusväärne, kahesuunaline side teed juhtelemendi süsteemi, nt asjade jaoturi ja sihtotstarbelise seadmete juurutatud mitteusaldusväärsel füüsilise ruumi. Mustri luuakse järgmistest põhimõtetest:

- Turvalisus alistab kõik muud võimalused.
- Seadmete Aktsepteeri soovimatute võrgu teavet. Seadme luuakse kõiki ühendusi ja marsruudib vaid väljamineva viisil. Käsu saada tagasi lõpuks seade, peab seadme algatada regulaarselt ühendust, et otsida mis tahes ootel käskude töötlemine.
- Seadmete peaks ainult ühenduse või luua marsruudib nad piilus, nt asjade jaoturi tuntud Services.
- Side tee seade ja või seadmest ja lüüsi vahel on turvatud kihis rakenduse Protocol (protokoll).
- Süsteemi tasemel autoriseerimine ja autentimise põhinevad seadme kohta identiteedid. Need võimaldavad juurdepääsu mandaadid ja õigused peaaegu kohe tühistatava.
- Kahesuunaline side jaoks ühendatud seadmete aeg-ajalt power või ühenduvuse probleemid hõlbustab hoides käskude ja seadme teatised, kuni seadme loob neid vastu võtta. Asjade jaoturi säilitab seadme järjekorrad saadab käske.
- Rakenduse last andmed on turvatud eraldi kaitstud läbimiseks lüüside konkreetse teenusega.

Mobiilsideseadmete valdkonna on kasutatud teenuse abil side mustri ulatusega rakendada tõuketeatised teatis osutamiseks, näiteks [Windows Push teatis][lnk-wns], [Google Cloud sõnumside][lnk-google-messaging], ja [Apple'i Lükketeatiste teenus][lnk-apple-push].

Asjade jaoturi toetatakse üle ExpressRoute's avaliku silmitsemine tee.

## <a name="next-steps"></a>Järgmised sammud

Siit saate teada, kuidas Azure'i asjade jaoturi võimaldab standardid vastavalt asjade mobiilsideseadmete halduse eemalt hallata, konfigureerida ja värskendada oma seadmete jaoks, lugege teemat [Azure ülevaade asjade jaoturi mobiilsideseadmete halduse][lnk-device-management].

Klientrakendustes rakendamiseks mitmesuguseid seadme riistvara platvormid ja opsüsteemides, saate kasutada seadme asjade SDK-d. Asjade seadme SDK-d kaasata teegid, mis hõlbustavad saatmise telemeetria soovitud asjade jaoturi ja vastuvõtt cloud-seadme käsud. Kui kasutate funktsiooni SDK-d, saate valida erinevaid võrguprotokollide asjade jaoturi suhelda. Lisateavet leiate teemast [teavet seadme SDK-d][lnk-device-sdks].

Alustamiseks mõned koodi kirjutamine ja töötavad mõned näidised, lugege teemat [Alustamine asjade jaoturi] [ lnk-get-started] õpetuse.

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png


[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Teenuse keda suhtlus, Clemens Vasters ajaveebipostitus"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-gateway]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk
[lnk-device-management]: iot-hub-device-management-overview.md
