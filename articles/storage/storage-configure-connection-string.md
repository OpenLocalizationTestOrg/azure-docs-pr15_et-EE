<properties 
    pageTitle="Ühendusstringi Azure mäluruumi konfigureerimine | Microsoft Azure'i"
    description="Ühendusstringi Azure storage konto konfigureerimine. Ühendusstringi sisaldab autentida salvestusruumi konto juurdepääsu oma rakendusest käitusajal vajalik teave."
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="configure-azure-storage-connection-strings"></a>Azure'i salvestusruumi ühendusstringi konfigureerimine

## <a name="overview"></a>Ülevaade

Ühendusstringi sisaldab autentimise juurde pääseda rakenduse käitusajal Azure storage konto andmete vajalik teave. Ühendusstringi abil saate konfigureerida.

- Ühenduse Azure'i salvestusruumi emulaator.
- Salvestusruumi konto Azure juurde.
- Juurdepääs määratud ressursside Azure kaudu ühiskasutusse antud juurdepääs signatuuri (SAS).

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

## <a name="storing-your-connection-string"></a>Oma ühendusstring talletamine

Teie taotlus vaja juurdepääsu autentimiseks Azure Storage taotlused käitusajal ühendusstring. Teil on mõni erinevaid võimalusi oma ühendusstring talletamiseks:

- Rakendust töölaual või seadmes, saate salvestada ühendusstringi mõne `app.config `faili või mõne `web.config` faili. Lisage ühendusstringi **AppSettings** jaotisele.
- Rakendus töötab Azure pilveteenuses, saate salvestada oma ühendusstring [Azure'i teenus konfiguratsioonifail skeemi (.cscfg)](https://msdn.microsoft.com/library/ee758710.aspx). Ühendusstringi lisada teenuse konfiguratsioonifail **ConfigurationSettings** osas.
- Saate oma ühendusstring otse koodis. Enamik stsenaariumid, soovitame siiski konfiguratsioonifailis salvestada oma konfiguratsioon string.

Oma ühendusstring konfiguratsiooni faili salvestamise hõlbustab värskendada salvestusruumi emulaator ja Azure storage konto pilveteenuses vaheldumisi ühendusstring. Peate ainult redigeerimiseks osutamiseks target keskkonna ühendusstring.

Saate [Microsoft Azure'i Configuration Manager](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) klassi juurde pääseda oma ühendusstring käitusajal sõltumata sellest, kus teie rakendus töötab.

## <a name="create-a-connection-string-to-the-storage-emulator"></a>Ühendusstringi salvestusruumi emulaator loomine

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Vt lisateavet mäluruumi emulaator [kasutamine Azure salvestusruumi emulaator ja testimine](storage-use-emulator.md) .

## <a name="create-a-connection-string-to-an-azure-storage-account"></a>Ühendusstringi Azure storage konto loomine

Ühendusstringi kontole Azure salvestusruumi loomiseks kasutage alltoodud ühenduse stringi vorming. Näitab, kas soovite ühendust võtta salvestusruumi HTTPS-i (soovitatav) kaudu või HTTP, Asendage `myAccountName` nimi oma konto salvestusruumi ja Asenda `myAccountKey` teie konto juurdepääsu võti:

    DefaultEndpointsProtocol=[http|https];AccountName=myAccountName;AccountKey=myAccountKey

Näiteks oma ühendusstring näeb välja umbes järgmine näidis ühendusstringi:

    DefaultEndpointsProtocol=https;AccountName=storagesample;AccountKey=<account-key>

> [AZURE.NOTE] Azure'i salvestusruumi toetab HTTP ja HTTPS ühendusstringi; siiski HTTPS-i kasutamine on soovitatav.

## <a name="create-a-connection-string-using-a-shared-access-signature"></a>Ühendusstringi abil ühiskasutusse antud juurdepääs signatuuri loomine

[AZURE.INCLUDE [storage-use-sas-in-connection-string-include](../../includes/storage-use-sas-in-connection-string-include.md)]

## <a name="creating-a-connection-string-to-an-explicit-storage-endpoint"></a>Ühendusstringi abil konkreetsete salvestusruumi lõpp-punkti loomine

Teenuse lõpp-punktid saate määrata konkreetselt oma ühendusstring asemel vaikimisi lõpp-punktid. Ühendusstringi, mis määrab konkreetsete lõpp-punkti loomiseks Määrake täieliku teenuse lõpp-punkti iga teenuse, sh protokolli määratlus (HTTPS (soovitatav) või HTTP) järgmises vormingus:

    DefaultEndpointsProtocol=[http|https];
    BlobEndpoint=myBlobEndpoint;
    QueueEndpoint=myQueueEndpoint;
    TableEndpoint=myTableEndpoint;
    FileEndpoint=myFileEndpoint;
    AccountName=myAccountName;
    AccountKey=myAccountKey

Üks stsenaarium, kus võiksite määrata konkreetsete lõpp on, kui teil on vastendatud bloobimälu salvestusruumi lõpp-punkti kohandatud domeeni. Sel juhul saate määrata oma kohandatud lõpp-punkti bloobimälu oma ühendusstring ja soovi korral määrata vaikimisi lõpp-punktid muu teenuse, kui teie rakendus kasutab neid.

Siin on lubatud ühendusstringi konkreetsete bloobimälu teenuse lõpp-punkti määratud näited.

    # Blob endpoint only
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    AccountName=storagesample;
    AccountKey=account-key

    # All service endpoints
    DefaultEndpointsProtocol=https;
    BlobEndpoint=www.mydomain.com;
    FileEndpoint=myaccount.file.core.windows.net;
    QueueEndpoint=myaccount.queue.core.windows.net;
    TableEndpoint=myaccount;
    AccountName=storagesample;
    AccountKey=account-key

Lõpp-punkti väärtus, mis on loetletud ühendusstringi kasutatakse ehitada bloobimälu teenusega taotluse URI-d ja see näitab, mis tahes URI-d, mis tagastatakse oma koodi kujul.

Pange tähele, et kui otsustate teenuse lõpp-punkti kaudu ühendusstringi jätta, siis te ei saa kasutada seda ühendusstring, et pääseda juurde selle teenuse andmete oma kood.

### <a name="creating-a-connection-string-with-an-endpoint-suffix"></a>Ühendusstringi järelliite lõpp-punkti loomine

Azure'i Hiina või Azure juhtimise, näiteks ühendusstringi jaoks salvestusteenus piirkondades või muu lõpp-punkti sufikseid eksemplarid loomiseks kasutage järgmist ühenduse stringi vormingut. Määrake, kas HTTP- või HTTPS-i kaudu salvestusruumi konto ühenduse, asendada `myAccountName` salvestusruumi konto nimi, Asendage `myAccountKey` oma konto võti ja Asenda `mySuffix` URI järelliide:


    DefaultEndpointsProtocol=[http|https];
    AccountName=myAccountName;
    AccountKey=myAccountKey;
    EndpointSuffix=mySuffix;


Näiteks oma ühendusstring peaks välja nägema järgmine ühendusstringi:

    DefaultEndpointsProtocol=https;
    AccountName=storagesample;
    AccountKey=<account-key>;
    EndpointSuffix=core.chinacloudapi.cn;

## <a name="parsing-a-connection-string"></a>Ühendusstringi sõelumine

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]


## <a name="next-steps"></a>Järgmised sammud

- [Azure'i salvestusruumi emulaator kasutamiseks katsetamine](storage-use-emulator.md)
- [Azure'i salvestusruumi maadeavastajad](storage-explorers.md)
- [Ühiskasutusega juurdepääsu allkirjad (SAS) abil](storage-dotnet-shared-access-signature-part-1.md)
