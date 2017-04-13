<properties
 pageTitle="Ajasti kõrge-saadavus ja usaldusväärsus"
 description="Ajasti kõrge-saadavus ja usaldusväärsus"
 services="scheduler"
 documentationCenter=".NET"
 authors="derek1ee"
 manager="kevinlam1"
 editor=""/>
<tags
 ms.service="scheduler"
 ms.workload="infrastructure-services"
 ms.tgt_pltfrm="na"
 ms.devlang="dotnet"
 ms.topic="article"
 ms.date="08/16/2016"
 ms.author="deli"/>


# <a name="scheduler-high-availability-and-reliability"></a>Ajasti kõrge-saadavus ja usaldusväärsus

## <a name="azure-scheduler-high-availability"></a>Azure'i Scheduler kõrge-saadavus

Azure'i platvormi teenuse core Azure'i Scheduler on väga saadaval ja funktsioonid nii geograafilise liigne teenuse juurutus- ja geo-piirkonna töö kopeerimine.

### <a name="geo-redundant-service-deployment"></a>Geograafilise liigne teenuste juurutamine

Azure'i Scheduler on saadaval peaaegu iga geo piirkonnas, mis on Azure täna Kasutajaliidese kaudu. Regioonid, mis on saadaval Azure ajasti loendis on [loetletud siin](https://azure.microsoft.com/regions/#services). Kui andmekeskuse majutatud piirkonnas on renderdamise pole saadaval, on Azure Scheduler Tõrkesiirde võimaluste nii, et teenus pole saadaval teise andmekeskuse.

### <a name="geo-regional-job-replication"></a>Geo-piirkonna töö dispersioonanalüüs

Mitte ainult on Azure ajasti ees halduse kutsed, kuid oma töö jaoks saadaval on ka geo kopeeritud. Kui ühe piirkonna on mõni katkestuste, Azure'i Scheduler ei üle ja tagab käivitatakse töö teise andmekeskuse andmepunktipaaride geograafilised piirkonna kaudu.

Näiteks kui olete loonud tööd Lõuna keskse meile, Azure'i Scheduler automaatselt tiražeerib Põhja keskse meile selle töö. Kui ilmneb tõrge Lõuna keskse meile, Azure'i Scheduler tagab käivitatakse töö Põhja keskse meile. 

![][1]

Selle tulemusena Azure'i Scheduler tagab, et teie andmed jääb samas laiem geograafilised piirkonnas Azure tõrke korral. Seetõttu ei vaja dubleerida kõrge-saadavus lisamiseks lihtsalt oma töö – Azure Scheduler automaatselt võimaldab kõrge-saadavus oma töö.

## <a name="azure-scheduler-reliability"></a>Azure'i Scheduler töökindluse

Azure'i ajasti tagab oma kõrge-saadavus ja võtab eri lähenemine kasutaja loodud projektidele. Näiteks võib oma tööd alustada lõpp HTTP, mis pole saadaval. Azure'i Scheduler proovib siiski käivitada oma töö edukalt, andes tõrke lahendamiseks muid võimalusi. Azure'i Scheduler see kahel viisil:

### <a name="configurable-retry-policy-via-retrypolicy"></a>Konfigureeritav proovige rühmapoliitika kaudu "retryPolicy"

Azure'i Scheduler võimaldab teil konfigureerida uuesti poliitika. Vaikimisi töö nurjumisel ajasti proovitakse töö uuesti neli korda, 30 sekundi järel. Uuesti konfigureerige see uuesti poliitika olema tõhusamaks (näiteks kümme korda, 30 sekundi järel) või kompaktne (nt kaks korda, päeva järel.)

Näiteks, kui see võib aidata, võite luua tööd, mis töötab kord nädalas ja käivitab HTTP lõpp. Kui HTTP lõpp-punkti ei tööta, kui teie töö töötab mitu tundi, võib-olla soovite nädal töö uuesti käivitada, kuna isegi uuesti vaikepoliitika nurjub, oodake. Sellisel juhul võib-olla konfigureerima standard uuesti poliitika uuesti iga 3 tunni järel (nt) asemel iga 30 sekundi järel.

Saate teada, kuidas konfigureerida uuesti poliitika, vaadake [retryPolicy](scheduler-concepts-terms.md#retrypolicy).

### <a name="alternate-endpoint-configurability-via-erroraction"></a>Alternatiivne lõpp-punkti seadistatavuse kaudu "errorAction"

Kui target lõpp-punkti oma Azure'i ajasti töö kättesaamatu, Azure'i Scheduler langeb tagasi alternatiivse tõrketöötluse lõpp-punkti pärast selle uuesti poliitika. Kui alternatiivse tõrketöötluse lõpp on konfigureeritud, käivitab Azure'i Scheduler seda. Alternatiivne lõpp oma tööd on väga kättesaadav ees tõrge.

Näitena alloleval joonisel Azure'i Scheduler järgib uuesti poliitika tabas New York veebiteenusest. Pärast soovitud korduskatsed ei õnnestu, see kontrollib, kas asendaja. Seejärel läheb edasi ja käivitab esitavad päringuid asendaja sama proovi uuesti poliitika.

![][2]

Pange tähele, et sama proovi uuesti poliitika kehtib nii algse toiming ja toimingu alternatiivse tõrge. Samuti on võimalik, et alternatiivse tõrge toimingu toimingu tüüp erineda peamised toimingu toimingu tüüp. Näiteks ajal peamine toimingu võib kasutada HTTP lõpp-punkti, toimingu tõrge selle asemel võib salvestusruumi järjekorda, teenuse siini järjekorda või teenuse siini teema toiming, mis teeb logimine.

Saate teada, kuidas konfigureerida Alternatiivne lõpp, vaadake [errorAction](scheduler-concepts-terms.md#action-and-erroraction).

## <a name="see-also"></a>Vt ka

 [Mis on ajasti?](scheduler-intro.md)

 [Azure'i Scheduler põhimõtet, terminoloogia ja üksuse hierarhia](scheduler-concepts-terms.md)

 [Azure'i portaalis Scheduler kasutamise alustamine](scheduler-get-started-portal.md)

 [Lepingud ja arvelduse Azure'i ajasti](scheduler-plans-billing.md)

 [Kuidas luua keerukate ajakavasid ja täpsemalt Korduvus Azure'i Scheduleri abil](scheduler-advanced-complexity.md)

 [Azure'i Scheduler REST API viide](https://msdn.microsoft.com/library/mt629143)

 [Azure'i Scheduler PowerShelli cmdlet-käskude viitamine.](scheduler-powershell-reference.md)

 [Azure'i Scheduler piirangud, vaikesätted ja kuvatavad tõrkekoodid](scheduler-limits-defaults-errors.md)

 [Azure'i Scheduler väljaminevat autentimist](scheduler-outbound-authentication.md)


[1]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image1.png

[2]: ./media/scheduler-high-availability-reliability/scheduler-high-availability-reliability-image2.png
