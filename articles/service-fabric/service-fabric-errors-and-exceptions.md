<properties
   pageTitle="Levinud FabricClient erandid visati | Microsoft Azure'i"
   description="Kirjeldatakse levinud vigu, mis võib olla salvestamisel FabricClient API-d, läbimiseks rakenduste ja kobar haldamise toimingute ja erandid."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# <a name="common-exceptions-and-errors-when-working-with-the-fabricclient-apis"></a>Levinud erandid ja tõrked FabricClient API-de töötamisel
[FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) API-de lubada kobar ja rakenduse administraatorid teenuse struktuuri rakenduse, teenuse või klaster haldustoimingute sooritamiseks. Näiteks rakenduse juurutamine, versioonitäiendus ja eemaldamine, klaster seisundi kontrollimine või teenuse testimine. Rakenduste arendajatele ja kobar administraatorid saate kasutada FabricClient API-de vahendite loomiseks teenuse struktuuri kobar ja rakenduste haldamine.

On palju erinevaid toiminguid, mis võivad täita FabricClient.  Iga meetodi võivad põhjustada vigade Vale sisestuse tõttu, käitusaja tõrgete või siirdamiseks taristu probleemid erandid.  Leidmiseks, mis erandid on visanud meetod dokumentatsioonist viite API. On mõned erandid, kuid saate visanud paljude erinevate [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) API-d. Järgmises tabelis on loetletud erandid, mis on levinud kogu FabricClient API-d.

|Erandi| Visatud, kui|
|---------|:-----------|
|[System.Fabric.FabricObjectClosedException](https://msdn.microsoft.com/library/system.fabric.fabricobjectclosedexception.aspx)|[FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) objekt on suletud olekus. Kõrvaldada [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) objekti kasutate ja uue [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx) objekti väärtustada. |
|[System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx)|Toiming aegus. [OperationTimedOut](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) tagastatakse juhul, kui toiming võtab rohkem kui MaxOperationTimeout lõpuleviimiseks.|
|[System.UnauthorizedAccessException](https://msdn.microsoft.com/en-us/library/system.unauthorizedaccessexception.aspx)|Toimingu Otsi Accessi nurjus. Tagastatakse E_ACCESSDENIED.|
|[System.Fabric.FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)|Käitustõrge ilmnes nii toimingut. FabricClient meetodite potentsiaalselt võivad põhjustada [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx), [tõrkekood](https://msdn.microsoft.com/library/system.fabric.fabricexception.errorcode.aspx) atribuut näitab täpse põhjustada erandi. Tõrkekoodid on määratletud [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) loend.|
|[System.Fabric.FabricTransientException](https://msdn.microsoft.com/library/system.fabric.fabrictransientexception.aspx)|Toiming nurjus mingi tingimuse siirdamiseks tõrke tõttu. Näiteks toiming võib nurjuda, kuna on koopiad pole ajutiselt kättesaadav. Siirdamiseks erandid vastavad nurjunud toimingute kohta, mida saate uuesti proovida.|

Mõned levinud [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) tõrkeid, mis on [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)tagastatavad:

|Tõrge| Tingimus|
|---------|:-----------|
|CommunicationError|Ilmnes põhjustas selle toimingu nurjub, proovige uuesti.|
|InvalidCredentialType|Mandaadi tüüp on kehtetu.|
|InvalidX509FindType|Funktsiooni X509FindType ei sobi.|
|InvalidX509StoreLocation|Funktsiooni X509 poe asukoht ei sobi.|
|InvalidX509StoreName|Funktsiooni X509 poe nimi on kehtetu.|
|InvalidX509Thumbprint|Funktsiooni X509 serdi sõrmejälje string ei sobi.|
|InvalidProtectionLevel|Kaitse tase ei sobi.|
|InvalidX509Store|Funktsiooni X509 salv ei saa avada.|
|InvalidSubjectName|Teema nimi ei sobi.|
|InvalidAllowedCommonNameList|Levinud nime loendis stringi vorming on kehtetu. Peaks olema komaga eraldatud loend.|
