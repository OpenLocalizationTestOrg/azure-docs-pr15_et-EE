<properties 
    pageTitle="Teenuse siini sõnumside näidised ülevaade | Microsoft Azure'i"
    description="Kategooria ja kirjeldatakse teenuse siini sõnumside näidised koos linkidega iga."
    services="service-bus"
    documentationCenter="na"
    authors="sethmanheim"
    manager="timlt"
    editor="" />
<tags 
    ms.service="service-bus"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="10/07/2016"
    ms.author="sethm" />

# <a name="service-bus-messaging-samples"></a>Teenuse siini sõnumside näidised

Teenuse siini sõnumside näidet kujutavad põhifunktsioone [teenuse siini sõnumside](https://azure.microsoft.com/services/service-bus/) (pilveteenuses) ja [Teenuse siini Windows Server](https://msdn.microsoft.com/library/dn282144.aspx). Selles artiklis kategooria ja näidised, mis on saadaval koos linkidega iga.

>[AZURE.NOTE] Teenuse siini näidised ei installita koos SDK. Näidiste hankimiseks külastage [Azure'i SDK näidised lehe](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Lisaks on uuendatud kogumik teenuse siini sõnumside näidised [siin](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) (kirjutamise, nad pole selles artiklis kirjeldatud).  

Relay näidised, leiate teemast [teenuse siini relay näidised](../service-bus-relay/service-bus-relay-samples.md).

## <a name="service-bus-messaging"></a>Teenuse siini sõnumside

Järgmised näited selgitavad, kuidas kirjutada rakendusi, mis kasutavad teenuse siini sõnumside.

Pange tähele, et kiirsõnumside näidised nõua juurdepääsuks oma teenuse siini nimeruum ühendusstringi.

### <a name="to-obtain-a-connection-string-for-azure-service-bus"></a>Ühendusstringi hankimiseks Azure'i teenus siini

1. [Azure'i portaali](http://portal.azure.com)sisse logima.

1. Klõpsake vasakpoolses veerus **Teenuse siini**.

1. Klõpsake loendis oma nimeruumi nimi.

3. Klõpsake nimeruum labale **jagatud juurdepääsupoliitikaid**.

4. Klõpsake **ühiskasutusega juurdepääsupoliitikaid** labale **RootManageSharedAccessKey**.

6. Ühendusstringi kopeerimine lõikelauale.

### <a name="to-obtain-a-connection-string-for-service-bus-for-windows-server"></a>Ühendusstringi hankimiseks teenuse siini Windows Server

1. Käivitage järgmine PowerShelli cmdlet-käsk:

    ```
    get-sbClientConfiguration
    ```

2. Kleepige App.config faili valimi ühendusstring.

### <a name="getting-started-samples"></a>Töö alustamine näidised

Need näited kirjeldavad sõnumside põhifunktsioone.

|Proovi nimi|Kirjeldus|Minimaalne SDK versioon|Kättesaadavus|
|---|---|---|---|
|[Alustamine: Sõnumside järjekorrad abil](http://code.msdn.microsoft.com/Getting-Started-Brokered-aa7a0ac3)|Näitab, kuidas kasutada Microsoft Azure'i teenus siini meilisõnumeid saata ega vastu võtta sõnumeid järjekorda.|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Alustamine: Sõnumside Teemad](http://code.msdn.microsoft.com/Getting-Started-Brokered-614d42e5)|Näitab, kuidas kasutada Microsoft Azure'i teenus siini meilisõnumeid saata ega vastu võtta sõnumeid teemaga mitu tellimust.|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|

### <a name="exploring-features"></a>Funktsioonide uurimine

Järgmised näited kujutavad teenuse siini erinevaid funktsioone.

|Proovi nimi|Kirjeldus|Minimaalne SDK versioon|Kättesaadavus|
|---|---|---|---|
|[HTTP sõnet pakkujad](http://code.msdn.microsoft.com/Service-Bus-HTTP-Token-38f2cfc5)|Näitab kinnitamise mõne HTTP/ülejäänud kliendi teenuse siini võimalust.|2.1|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Teenuse siini HTTP kliendi](http://code.msdn.microsoft.com/Service-Bus-HTTP-client-fe7da74a)|Näitab, kuidas sõnumeid saata ja vastu võtta sõnumeid teenuse siini ülejäänud/HTTP kaudu.|2.3|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Teenuse siini Autoforwarding](http://code.msdn.microsoft.com/Service-Bus-Autoforwarding-b9df470b)|Näitab, kuidas sõnumite automaatseks edastamiseks järjekorda, tellimuse või deadletter järjekorda teise järjekorda või teema. Lisaks demonstreerib sõnumi saatmine järjekorda või teema kaudu edastamine järjekorda.|2.3|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Vahendatud sõnumid: WCF-i kanali seansi valimi](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-0a526451)|Näitab, kuidas kasutada Microsoft Azure'i teenus siini Windows Communication Foundation (WCF) kanalite kasutamine. Valimi näitab kasutamist WCF-i kanalite kaudu teenuse siini järjekorda sõnumite saatmiseks ja vastuvõtmiseks. Valimi kuvatakse nii seansi-seansi side teenuse siini üle.|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Vahendajaks sõnumside: tehinguid](http://code.msdn.microsoft.com/Brokered-Messaging-8cd41d1e)|Näitab, kuidas kasutada Microsoft Azure'i teenus siini sõnumside tehing ulatus funktsioonide tagamiseks töölehed sõnumside toimingud on kinnitatud atomically.|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Vahendatud sõnumid: Toimingute abil ülejäänud](http://code.msdn.microsoft.com/Brokered-Messaging-569cff88)|Näitab, kuidas teenuse siini abil ülejäänud haldamise toiminguid.|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Ressursi pakkuja REST API-d](http://code.msdn.microsoft.com/Service-Bus-Resource-5d887203)|Näitab, kuidas uue teenuse siini RDFE REST API-de haldamine nimeruumid ja sõnumside üksuste abil.|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Vahendatud sõnumid: WCF-i teenuse seansi valimi](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-db4262c2)|Näitab, kuidas kasutada Microsoft Azure'i teenus siini kasutades WCF-i teenuse mudel. Valimi näitab tegemise teenuse siini järjekorda kaudu suhtlemine seansi mudeli WCF-i teenuse kasutamist.|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Vahendatud sõnumid: Taotluse vastus](http://code.msdn.microsoft.com/Brokered-Messaging-Request-2b4ff5d8)|Näitab, kuidas kasutada Microsoft Azure'i teenus siini ja taotluse/vastuse funktsioone. Valimi kuvatakse lihtne klientide ja serverite teenuse siini järjekorda kaudu suhtlemiseks.|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Vahendatud sõnumid: teostamatu järjekord](http://code.msdn.microsoft.com/Brokered-Messaging-Dead-22536dd8)|Näitab, kuidas kasutada Microsoft Azure'i teenus siini ja sõnumside "teostamatu järjekord" funktsioone. Valimi kuvatakse lihtsa saatja ja vastuvõtja teenuse siini järjekorda kaudu suhtlemiseks.|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Vahendatud sõnumid: Edasilükatud sõnumid](http://code.msdn.microsoft.com/Brokered-Messaging-ccc4f879)|Näitab, kuidas kasutada funktsiooni sõnumi edasilükkamise Microsoft Azure'i teenus siini. Valimi kuvatakse lihtsa saatja ja vastuvõtja teenuse siini järjekorda kaudu suhtlemiseks.|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Vahendatud sõnumid: Seansi sõnumite](http://code.msdn.microsoft.com/Brokered-Messaging-Session-41c43fb4)|Näitab, kuidas kasutada Microsoft Azure'i teenus siini ja sõnumside seansi funktsioone. Valimi kuvatakse lihtsa saatja ja vastuvõtja teenuse siini järjekorda kaudu suhtlemiseks.|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Vahendatud sõnumid: Taotluse vastuse teema](http://code.msdn.microsoft.com/Brokered-Messaging-Request-6759a36e)|Näitab, kuidas rakendada taotluse/vastuse mustri, kasutades Microsoft Azure'i teenus siini teemade ja tellimused. Valimi kuvatakse lihtne klientide ja serverite teenuse siini teema kaudu suhtlemiseks.|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Vahendatud sõnumid: Taotluse vastuse järjekorda](http://code.msdn.microsoft.com/Brokered-Messaging-Request-0ce8fcaf)|Näitab, kuidas kasutada Microsoft Azure'i teenus siini ja taotluse/vastuse funktsioone. Valimi kuvatakse lihtne klientide ja serverite kaks teenuse siini järjekorda kaudu suhtlemiseks.|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Vahendatud sõnumid: Duplikaatide tuvastamine](http://code.msdn.microsoft.com/Brokered-Messaging-c0acea25)|Näitab, kuidas kasutada Microsoft Azure'i teenus siini sõnumite tuvastamise järjekorrad. See loob kaks järjekorda, üks lubatud duplikaatide tuvastamine ja teine ilma duplikaatide tuvastamine.|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Vahendatud sõnumid: Sõnumside asünkroonse](http://code.msdn.microsoft.com/Brokered-Messaging-Async-211c1e74)|Näitab, kuidas kasutada Microsoft Azure'i teenus siini meilisõnumeid saata ega vastu võtta sõnumeid asünkroonselt järjekorda. Kuhjuda pakub saatja ja mis tahes arv vastuvõtjad sidumata, asünkroonne suhtlemine (siin ühe vastuvõtja).|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Vahendatud sõnumid: Täpsemad filtrid](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749)|Näitab, kuidas kasutada Microsoft Azure'i teenus siini avaldamine/Telli Täpsemad filtrid. Loob teema ja 3 tellimuste teise filtri määratlused, teema sõnumid saadetakse ja saab kõiki sõnumeid tellimused.|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Vahendatud sõnumid: Sõnumite i](http://code.msdn.microsoft.com/Brokered-Messaging-be2dac1d)|Näitab, kuidas kasutada funktsiooni Microsoft Azure'i teenus siini sõnumite i. See näitab, kuidas kasutada funktsiooni sõnumite i korral võta vastu.|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|

## <a name="service-bus-reference-tools"></a>Teenuse siini viide tööriistad

Järgmised näidet kujutavad erinevate teenuse omadused.

|Proovi nimi|Kirjeldus|Minimaalne SDK versioon|Kättesaadavus|
|---|---|---|---|
|[Teenuse siini Explorer](http://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a)|Teenuse siini Explorer võimaldab kasutajatel teenuse siini teenuse nimeruumi ühendamiseks ja sõnumside üksuste haldamine lihtne viisil. Tööriist pakub täiustatud funktsioone, näiteks impordi/ekspordi funktsioonid ja võimalus testida sõnumside üksuste ja edastamine teenused.|1.8|Microsoft Azure'i teenus siini; Teenuse siini Windows Server|
|[Luba: SBAzTool](http://code.msdn.microsoft.com/Authorization-SBAzTool-6fd76d93)|See näide näitab, kuidas luua ja hallata teenuse identiteedid Microsoft Azure Active Directory juurdepääsu reguleerimine (nimetatakse ka teenuse Access juhtelemendi või ACS) teenuse siini jaoks.|N/A|Microsoft Azure'i teenus siini|

## <a name="next-steps"></a>Järgmised sammud

Järgmistest teemadest leiate kontseptuaalne ülevaated teenuse siini.

- [Teenuse siini sõnumside ülevaade](service-bus-messaging-overview.md)
- [Teenuse siini ülesehitus](service-bus-architecture.md)
- [Teenuse siini põhialused](service-bus-fundamentals-hybrid-solutions.md)
