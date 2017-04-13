<properties
 pageTitle="Arendaja juhend - faili üleslaadimine | Microsoft Azure'i"
 description="Azure'i asjade jaoturi arendaja juhend - asjade jaoturi seadmest failide üleslaadimise"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="multiple"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="09/30/2016" 
 ms.author="dobett"/>

# <a name="upload-files-from-a-device"></a>Laadige failid seadmest

## <a name="overview"></a>Ülevaade

Nagu on kirjeldatud [asjade jaoturi lõpp-punktid] [ lnk-endpoints] artiklis seadmed saate algatada faili üleslaadimiseks teatis – seadme suunatud endpoint saatmisega (**/devices/ {deviceId} / failid**).  Kui seadme teavitab asjade jaoturi lõplikus üleslaadimine, loob asjade jaoturi faili üleslaadimine teatised sõnumitena teenuse suunatud endpoint (**/messages/servicebound/filenotifications**) kaudu saab.

Asemel vahendajate sõnumite kaudu asjade jaoturi ise asjade jaoturi hoopis toimib seotud Azure Storage konto lisamine saatja. Seadme taotleb märgiks salvestusruumi asjade jaoturi, mis on seotud seadme soovib üles fail. Seadme kasutab SAS URI faili üleslaadimiseks salvestusruumi ja kui üleslaadimine on lõpule viidud seade saadab lõpetamise teate asjade jaoturi. Asjade jaoturi kinnitab, et fail on üles laaditud ja lisab seejärel faili üleslaadimine teatis uus teenus suunatud faili teatis sõnumside lõpp-punkti.

Enne kui laadite faili asjade jaoturi seadmest, tuleb konfigureerida oma keskuses seostada [Ka Azure Storage] [ lnk-associate-storage] selle kontoga.

Seadme saab siis [lähtestada üleslaadimine] [ lnk-initialize] ja seejärel [teavitamise asjade jaoturi] [ lnk-notify] kui üleslaadimine on lõpule jõudnud. Soovi korral, kui seadme teavitab asjade jaoturi, et üleslaadimine on lõpule jõudnud, teenuse saate luua [teavitussõnumi][lnk-service-notification].

### <a name="when-to-use"></a>Millal kasutada

Kui soovite üles laadida faili seadmest tagaandmebaas teenust asemel asjade keskuse kaudu sõnumi saatmist, kasutage seda asjade jaoturi funktsiooni.

## <a name="associate-an-azure-storage-account-with-iot-hub"></a>Asjade keskuses seostada Azure Storage konto

Faili üleslaadimine funktsiooni kasutamiseks tuleb esmalt konto Azure Storage asjade jaoturi linkida. Saate teha ühte [Azure portaali]kaudu[lnk-management-portal], või programmiliselt [asjade jaoturi ressursi pakkuja REST API-de][lnk-resource-provider-apis]. Pärast Azure Storage konto on seostatud teie asjade jaoturi, tagastab teenuse SAS URI seadmesse, kui seadme käivitab päringu fail üles.

> [AZURE.NOTE] [Azure'i asjade jaoturi SDK-d] [ lnk-sdks] automaatselt hallata toomine SAS URI, faili üleslaadimisel ja teavitades asjade jaoturi lõplikus üleslaadimine.

## <a name="initialize-a-file-upload"></a>Faili üleslaadimine lähtestamine

Asjade jaoturi on lõpp konkreetselt taotleda SAS URI Storage faili üleslaadimiseks seadmete jaoks. Seadme käivitab faili üleslaadimine, saates POSTITUSES asjade jaoturi veebisaidil `{iot hub}.azure-devices.net/devices/{deviceId}/files` järgmised JSON keha:

```
{
    "blobName": "{name of the file for which a SAS URI will be generated}"
}
```

Asjade jaoturi tagastab seade kasutab faili üleslaadimiseks järgmist:

```
{
    "correlationId": "somecorrelationid",
    "hostname": "contoso.azure-devices.net",
    "containerName": "testcontainer",
    "blobName": "test-device1/image.jpg",
    "sasToken": "1234asdfSAStoken"
}
```

### <a name="deprecated-initialize-a-file-upload-with-a-get"></a>Taunitud: Faili üleslaadimine koos saamiseks lähtestamine

> [AZURE.NOTE] Selles jaotises kirjeldatakse, kuidas saada SAS URI asjade keskuse kaudu taunitud funktsioonid. Kasutage eespool kirjeldatud postituse meetod.

Asjade jaoturi on kaks ülejäänud lõpp-punktid toetamiseks faili üleslaadimisel, üks SAS URI salvestusruumi ja teine teatada asjade keskpunkt lõplikus üles leida. Seadme käivitab faili üleslaadimine, saates saamiseks asjade jaoturi veebisaidil `{iot hub}.azure-devices.net/devices/{deviceId}/files/{filename}`. Jaoturi tagastab SAS URI teatud faili üles laadida ja korrelatsiooni ID kasutada kui üleslaadimine on lõpule viidud.

## <a name="notify-iot-hub-of-a-completed-file-upload"></a>Teavitage asjade jaoturi lõplikus faili üleslaadimine

Seadme vastutab faili üleslaadimine kasutamine Azure salvestusruumi SDK-d. Kui üleslaadimine on lõpule viidud, seade saadab POSTITUSES asjade jaoturi veebisaidil `{iot hub}.azure-devices.net/devices/{deviceId}/files/notifications` järgmised JSON keha:

```
{
    "correlationId": "{correlation ID received from the initial request}",
    "isSuccess": bool,
    "statusCode": XXX,
    "statusDescription": "Description of status"
}
```

Väärtus `isSuccess` on kahendväärtus tähistav, kas fail on üles laaditud. Oleku kood `statusCode` on olek salvestusruumi, faili üles ja `statusDescription` vastab olevat `statusCode`.

## <a name="reference-topics"></a>Teemasid:

Järgmistes teemades viide teile seadmest failide üleslaadimise kohta lisateavet.

## <a name="file-upload-notifications"></a>Faili teated üleslaadimise kohta

Kui seade faili üleslaadimine ja teavitab asjade jaoturi üles lõpetamist, loob teenus soovi korral teatise sõnumi, mis sisaldab faili nimi ja salvestusruumi asukoht.

Nagu on selgitatud teemas [lõpp-punktid][lnk-endpoints], asjade jaoturi pakub faili üleslaadimine teatised teenuse suunatud endpoint (**/messages/servicebound/fileuploadnotifications**) kaudu sõnumeid. Faili üleslaadimine teatiste vastuvõtmine semantika on samad, mis on pilv-seadme sõnumite ja on sama [sõnumi elutsükli][lnk-lifecycle]. Iga sõnumi vormilt faili üleslaadimine teatis lõpp-punkti on JSON kirje järgmised atribuudid:

| Atribuut | Kirjeldus |
| -------- | ----------- |
| EnqueuedTimeUtc | Ajatempli, mis näitab, kui teate on loodud. |
| DeviceId | **DeviceId** seadet, mis on üles laaditud faili. |
| BlobUri | URI üleslaaditud faili. |
| BlobName | Üleslaaditud faili nimi. |
| LastUpdatedTime | Ajatempli, mis näitab, kui faili viimati värskendatud. |
| BlobSizeInBytes | Üleslaaditud faili maht. |

**Näide**. See on näide kehaosa faili üleslaadimine teavitussõnumi.

```
{
    "deviceId":"mydevice",
    "blobUri":"https://{storage account}.blob.core.windows.net/{container name}/mydevice/myfile.jpg",
    "blobName":"mydevice/myfile.jpg",
    "lastUpdatedTime":"2016-06-01T21:22:41+00:00",
    "blobSizeInBytes":1234,
    "enqueuedTimeUtc":"2016-06-01T21:22:43.7996883Z"
}
```

## <a name="file-upload-notification-configuration-options"></a>Faili üleslaadimine teatis konfiguratsiooni suvandid

Iga asjade jaoturi seab faili üleslaadimine teatiste konfigureerimine järgmistest suvanditest:

| Atribuut | Kirjeldus | Vahemik ja vaikeväärtus |
| -------- | ----------- | ----------------- |
| **enableFileUploadNotifications** | Kontrollib, kas faili üleslaadimine teatised kirjutada faili teatised lõpp-punkti. | Bool. Vaikimisi: True. |
| **fileNotifications.ttlAsIso8601** | Default TTL faili üleslaadimine teatised. | ISO_8601 intervall 48 H kuni (miinimum 1 minut). Vaikimisi: 1 tund. |
| **fileNotifications.lockDuration** | Faili üleslaadimine teatised järjekorra kestus lukustada. | 5 – 300 sekundi (miinimum 5 minutit). Vaikimisi: 60 sekundi järel. |
| **fileNotifications.maxDeliveryCount** | Teate järjekorra maksimaalne arv faili üleslaadimine | 1 kuni 100. Vaikimisi: 100. |

## <a name="additional-reference-material"></a>Täiendav viide materjali

Muud viited teemade arendaja juhend on järgmised.

- [Asjade jaoturi lõpp-punktid] [ lnk-endpoints] kirjeldatakse mitmesuguste lõpp-punktid, mis iga asjade jaoturi seab käitusaja ja haldus.
- [Pidurdamise ja kvootide] [ lnk-quotas] kirjeldatakse piirmäära, mis kehtivad teenuse asjade jaoturi ja ahendamise käitumine oodata, kui kasutate teenust.
- [Asjade jaoturi seade ja SDK-d] [ lnk-sdks] erinevate keele SDK-d on loetletud teil on kasutada, kui teil tekib nii seade ja rakendused, mis nendega asjade jaoturi abil.
- [Päringu keel kaksikud, võimalused ja töö] [ lnk-query] kirjeldatakse päringukeele abil saate otsida teavet asjade keskuse kohta oma seadmes kaksikud, võimalused ja -tööde haldamine.
- [Asjade jaoturi MQTT tugi] [ lnk-devguide-mqtt] annab Lisateavet kohta asjade jaoturi tugi MQTT protokolli.

## <a name="next-steps"></a>Järgmised sammud

Nüüd olete õppinud, kuidas asjade jaoturi abil seadmest faile üles laadida, võib olla huvitatud arendaja juhend järgmisi teemasid:

- [Seadme identiteedid asjade keskuses haldamine][lnk-devguide-identities]
- [Asjade jaoturi juurdepääsu reguleerimine][lnk-devguide-security]
- [Riigi ja konfiguratsioone sünkroonimine seadme kaksikud abil][lnk-devguide-device-twins]
- [Otsest meetodit seadmes][lnk-devguide-directmethods]
- [Mitmes seadmes tööde plaanimine][lnk-devguide-jobs]

Kui soovite proovida mõnda selles artiklis kirjeldatud mõisted, võib olla huvi järgmised asjade jaoturi õpetus:

- [Asjade jaoturi pilve seadmetest failide üleslaadimine][lnk-fileupload-tutorial]

[lnk-resource-provider-apis]: https://msdn.microsoft.com/library/mt548492.aspx
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-management-portal]: https://portal.azure.com
[lnk-fileupload-tutorial]: iot-hub-csharp-csharp-file-upload.md
[lnk-associate-storage]: iot-hub-devguide-file-upload.md#associate-an-azure-storage-account-with-iot-hub
[lnk-initialize]: iot-hub-devguide-file-upload.md#initialize-a-file-upload
[lnk-notify]: iot-hub-devguide-file-upload.md#notify-iot-hub-of-a-completed-file-upload
[lnk-service-notification]: iot-hub-devguide-file-upload.md#file-upload-notifications
[lnk-lifecycle]: iot-hub-devguide-messaging.md#message-lifecycle

[lnk-devguide-identities]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
