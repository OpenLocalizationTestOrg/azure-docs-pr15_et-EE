<properties
    pageTitle="Varundamise ja taastamise rakendust Service rakenduste ülejäänud abil"
    description="Saate teada, kuidas kasutada rahulik API kõned varundamine ja taastamine Azure'i rakendust Service rakenduse"
    services="app-service"
    documentationCenter=""
    authors="NKing92"
    manager="wpickett"
    editor="" />

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="nicking"/>
# <a name="use-rest-to-back-up-and-restore-app-service-apps"></a>Varundamise ja taastamise rakendust Service rakenduste ülejäänud abil

> [AZURE.SELECTOR]
- [PowerShelli](../app-service/app-service-powershell-backup.md)
- [REST API-GA](websites-csm-backup.md)

[Rakenduse rakendused](https://azure.microsoft.com/services/app-service/web/) saab varundada nimega plekid Azure salvestusruumi. Varukoopia võib sisaldada ka rakenduse andmebaasid. Kui rakendus on kunagi kogemata kustutanud või kui rakendus peab olema taastatud varasema versiooni, saate selle mis tahes eelmise varukoopia põhjal taastada. Varukoopiate saab teha igal ajal nõudmisel või varukoopiaid saab ajastada sobiva intervalliga.

Selles artiklis selgitatakse varundus ja taaste rahulik API taotlusi rakendus. Kui soovite luua ja hallata rakenduse varukoopiate graafiliselt Azure portaali kaudu, leiate [Azure'i rakendust Service veebirakenduse varundamine](web-sites-backup.md)

<a name="gettingstarted"></a>
## <a name="getting-started"></a>Alustamine
ÜLEJÄÄNUD päringuid, peate teadma oma rakenduse **nimi**, **ressursirühm**ja **tellimuse id**. Selle teabe leiate rakenduse **App teenuse** tera [Azure portaali](https://portal.azure.com)klõpsates. Selle artikli näited me konfigureerite veebisaidi **backuprestoreapiexamples.azurewebsites.net**. See on talletatud vaike-Web-WestUS ressursirühm ning töötab koos selle ID 00001111-2222-3333-4444-555566667777 tellimus.

![Valimi veebisaidi teave][SampleWebsiteInformation]

<a name="backup-restore-rest-api"></a>
## <a name="backup-and-restore-rest-api"></a>Varundus ja taaste REST API-ga
Oleme nüüd hõlmab mitmesuguseid varundus ja taaste rakenduse REST API abil. Iga näide sisaldab URL-i ja HTTP taotluse keha. Valimi URL sisaldab kohatäidete pakitud looksulud, nt {tellimuse id}. Asendage kohatäited vastav teave oma rakenduse. Siin on viide, iga kohatäidet, mis kuvatakse näiteks URL-id selgitus.

* tellimuse id – sisaldava rakenduse Azure tellimuse ID
* ressursinimi-rühma – ressursirühm, mis sisaldab rakenduse nimi
* nimi – rakenduse nimi
* varundus-id-ID rakenduse varukoopia

Kõik dokumendid API, sh mitu valikuliste parameetrite, mida saab lisada HTTP-päring leiate [Azure'i Resource Explorer](https://resources.azure.com/).

<a name="backup-on-demand"></a>
## <a name="backup-an-app-on-demand"></a>Varundamise rakendus nõudmisel
Varundage rakenduse kohe, saata **postituse** taotluse **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backup/**.

Siin on URL näeb meie näite veebisaidi kasutamine. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/sites/backuprestoreapiexamples/backup/**

Lisage JSON objekti kehas teie taotlus määrata, millist salvestusruumi abil salvestada varukoopia. JSON objekt peab olema nimega **storageAccountUrl**, mis on Azure Storage container, mis hoiab varukoopia bloobimälu kirjutamine juurdepääsu andmine [SAS URL-i](../storage/storage-dotnet-shared-access-signature-part-1.md) atribuut. Kui soovite oma andmebaasi varundada, peate määrama loend sisaldavad nimed, tüübid ja ühendusstringi andmebaasi varundada.

```
{
    "properties":
    {
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        “databases”: [
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”,
                "connectionString": "<your database connection string>"
            }
        ]
    }
}
```

Rakenduse varukoopia algab kohe, kui taotluse. Varundamist võib võtta kaua aega. HTTP vastus sisaldab ID, mida saate kasutada mõne muu taotluse varukoopia oleku kuvamine. Siin on näide HTTP vastuse meie varukoopia koosolekukutse kehasse.

```
{
    "id": "/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples",
    "name": " backuprestoreapiexamples ",
    "type": "Microsoft.Web/sites",
    "location": "WestUS",
    "properties":    {
        "id": 1,
        "storageAccountUrl": “https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl”,
        "blobName": “backup_201509291825.zip”,
        "name": "backup_201509291825",
        "status": 4,
        "sizeInBytes": 0,
        "created": "2015-09-29T18:25:26.3992707Z",
        "log": null,
        "databases": [
            {
                "databaseType": "SqlAzure",
                "name": "MyDatabase1",
                "connectionString": "<your database connection string>"
            }
        ],
        "scheduled": false,
        "correlationId": "ea730f4d-dd06-4f7f-a090-9aa09k662h36",
    }
}
```

>[AZURE.NOTE] Tõrketeated leiate log atribuudi HTTP vastuse.

<a name="schedule-automatic-backups"></a>
## <a name="schedule-automatic-backups"></a>Automaatvarunduse plaanimine
Lisaks varundada rakenduse nõudmisel, saate ka plaanida automaatselt juhtub varukoopia.

### <a name="set-up-a-new-automatic-backup-schedule"></a>Uue automaatse varukoopia ajakava seadistamine
Varunduse ajakava seadistamiseks saata **sellele** taotluse **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup**.

Siin on URL näeb näide veebisaidile. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup**

Koosolekukutse kehas peab olema JSON-objekti, mis määrab varukoopia konfiguratsiooni. Siin on näide kõik soovitud parameetrid.

```
{
    "location": "WestUS",
    "properties": // Represents an app restore request
    {
        "backupSchedule": { // Required for automatically scheduled backups
            "frequencyInterval": "7",
            "frequencyUnit": "Day",
            "keepAtLeastOneBackup": "True",
            "retentionPeriodInDays": "10",
        },
        "enabled": "True", // Must be set to true to enable automatic backups
        "name": “mysitebackup”,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

Selles näites konfigureerib rakenduse automaatselt iga seitsme päeva varundada. Parameetrite **frequencyInterval** ja **frequencyUnit** koos kindlaks teha, kui sageli varundatakse. **FrequencyUnit** sobivad väärtused on **tund** ja **päev**. Näiteks varundage rakenduse iga 12 tunni, määrake frequencyInterval 12 ja frequencyUnit tund.

Vana varukoopiate eemaldatakse konto salvestusruumi automaatselt. Saate määrata, kuidas ja vana varukoopiaid võib olla **retentionPeriodInDays** parameetri. Kui soovite alati olema vähemalt üks varukoopia salvestada, olenemata sellest, kuidas vana seda, on seatud **keepAtLeastOneBackup** true.

### <a name="get-the-automatic-backup-schedule"></a>Automaatse varunduse ajakava hankimine
Rakenduse varukoopia konfiguratsiooni hankimine saata **postituse** taotluse URL-i **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/config/backup/list**.

Meie näite saidi URL on **https://management.azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/providers/Microsoft.Web/sites/backuprestoreapiexamples/config/backup/list**.

<a name="get-backup-status"></a>
## <a name="get-the-status-of-a-backup"></a>Oleku varukoopia hankimine
Sõltuvalt sellest, kuidas suur rakendus on varukoopia võib võtta aega. Varukoopiate võib ka nurjuda ajalõpuni, või osaliselt õnnestuda. Mõni rakendus varukoopiate oleku vaatamiseks saata URL-i **GET** -päring **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups**.

Teatud varukoopia oleku vaatamiseks saata GET-päringu URL-i **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}**.

Siin on URL näeb näide veebisaidile. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**

Vastuse sisu sisaldab see näide sarnaneb JSON objekti.

```
{
    "properties":  {
        "id": 1,
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl",
        "blobName": "backup_201509291734.zip",
        "name": "backup_201509291734",
        "status": 2,
        "sizeInBytes": 151973,
        "created": "2015-09-29T17:34:57.2091605",
        "scheduled": false,
        "lastRestoreTimeStamp": null,
        "finishedTimeStamp": "2015-09-29T17:35:11.3293602",
        "correlationId": "2379fc13-418a-4b9c-920d-d06e73c5028d",
        "websiteSizeInBytes": 209920
    }
}
```

Varukoopia olekuks on loetletud tüüpi. Siin on iga riigi võimaliku.

* 0 – InProgress: varukoopia käivitamist, kuid pole veel lõpetamata.
* 1 – nurjus: Varundamine nurjus.
* 2 – õnnestus: Varundamine on lõpule viidud.
* 3 – semaforile: Varukoopia ei lõpetanud kellaaja ja on tühistatud.
* 4 – loodud: Varukoopia taotluse järjekorda on, kuid pole käivitatud.
* 5 – vahele: Varukoopia ei tehta liiga palju varukoopiate käivitamise ajakava tõttu.
* 6 – PartiallySucceeded: Varukoopia õnnestus, kuid mõned failid ei varundatud, kuna neid ei saa lugeda. See juhtub tavaliselt, kuna eksklusiivset lukustamist oli panna faile.
* 7 – DeleteInProgress: Varukoopia saanud kustutada, kuid pole veel kustutatud.
* 8 – DeleteFailed: Varukoopia ei saa kustutada. See võib juhtuda, kuna varukoopia loomiseks kasutatud SAS URL-i on aegunud.
* 9 – kustutatud: Varundamine on kustutatud.

<a name="restore-app"></a>
## <a name="restore-an-app-from-a-backup"></a>Rakenduse varukoopia põhjal taastamine
Kui teie rakendus on kustutatud, või kui soovite taastada rakenduse varasema versiooni, saate rakenduse varukoopia põhjal taastamine. Kasutada taastamise, saata **postituse** taotluse URL-i **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/restore**.

Siin on URL näeb näide veebisaidile. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/Restore**

Saatke koosolekukutse kehasse JSON objekti, mis sisaldab taastetoimingu atribuudid. Siin on näide, mis sisaldab kõik nõutavad atribuudid:

```
{
    "location": "WestUS",
    "properties":
    {
        "blobName": "backup_201509280431.zip",
        "databases": [ // Not required unless you’re restoring databases
            {
                “databaseType”: “SqlAzure”,
                “name”: “MyDatabase1”
        }],
        "overwrite": "true",
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl" // SAS URL to storage container containing your website backup
    }
}
```

### <a name="restore-to-a-new-app"></a>Uue rakenduse taastamine
Vahel soovite luua uue rakenduse, kui taastate varukoopia juba olemasoleva rakenduse ülekirjutamise asemel. Selle tegemiseks muutmiseks osutage käsule Uus rakendus, mida luua soovite kutse URL-i ja muuta selle JSON **kirjutada** atribuudi väärtuseks **false**.

<a name="delete-app-backup"></a>
## <a name="delete-an-app-backup"></a>Mõni rakendus varundamise kustutamine
Kui soovite kustutada varukoopia, saata URL-i **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}** **kustutamine** taotluse.

Siin on URL näeb näide veebisaidile. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1**

<a name="manage-sas-url"></a>
## <a name="manage-a-backups-sas-url"></a>URL-i lisamine varundamise SAS haldamine
Azure'i rakendust Service proovib kustutada varukoopia Azure Storage SAS URL-i, mis oli lisatud varukoopia loomisel. Kui seda SAS URL-i pole enam kehtiv, ei saa kustutada varukoopia REST API kaudu. Siiski saate värskendada, saates **postituse** taotluse URL-i varukoopia seostatud SAS URL-i **https://management.azure.com/subscriptions/ {subscription-id}/resourceGroups/{resource-group-name}/providers/Microsoft.Web/sites/{name}/backups/{backup-id}/list**.

Siin on URL näeb näide veebisaidile. **https://Management.Azure.com/subscriptions/00001111-2222-3333-4444-555566667777/resourceGroups/Default-Web-WestUS/Providers/Microsoft.Web/sites/backuprestoreapiexamples/backups/1/list**

Saatke koosolekukutse kehasse JSON objekti, mis sisaldab uus SAS URL. Siin on näide.

```
{
    "properties":
    {
        "storageAccountUrl": "https://account.blob.core.windows.net/backups?sv=2015-02-21&sr=c&sig=DzlkBl7h32C8qCv%2BifdBRxE63r4iv0kZ9L7E0qP16sY%3D&se=2016-09-15T22%3A46%3A54Z&sp=rwdl"
    }
}
```

>[AZURE.NOTE] Turvalisuse põhjustel ei tagastata SAS URL-i varukoopia seostatud teatud varukoopia GET-päringu saatmisel. Kui soovite vaadata seotud varukoopia SAS URL-i, saata sama URL-i ülaltoodud POST taotlus. Sisaldavad tühi JSON objekti koosolekukutse kehasse. Serverist vastust, mis sisaldab kõiki selle varundamise teavet, sh selle SAS URL-i.

<!-- IMAGES -->
[SampleWebsiteInformation]: ./media/websites-csm-backup/01siteconfig.png
