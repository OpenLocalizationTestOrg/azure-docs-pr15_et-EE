<properties 
    pageTitle="Teenuse siini relay näidised ülevaade | Microsoft Azure'i"
    description="Kategooria ja kirjeldatakse teenuse siini relay näidised koos linkidega iga."
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

# <a name="service-bus-relay-samples"></a>Teenuse siini relay näidised

Teenuse siini relay näidet kujutavad [teenuse siini relay](https://azure.microsoft.com/services/service-bus/)põhifunktsioone. Selles artiklis kategooria ja näidised, mis on saadaval koos linkidega iga.

>[AZURE.NOTE] Teenuse siini näidised ei installita koos SDK. Näidiste hankimiseks külastage [Azure'i SDK näidised lehe](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
>
>Lisaks on uuendatud kogumik teenuse siini relay näidised [siin](https://github.com/Azure-Samples/azure-servicebus-relay-samples) (kirjutamise, nad pole selles artiklis kirjeldatud).  

Kiirsõnumside näidised, leiate teemast [teenuse siini sõnumside näidised](../service-bus-messaging/service-bus-samples.md).

## <a name="service-bus-relay"></a>Teenuse siini edastamine

Järgmised näited selgitavad, kuidas kirjutada rakendusi, mis kasutavad teenuse siini vahendusteenusesse.

Pange tähele, et puhul relay näidised vaja juurdepääsu oma teenuse siini nimeruum ühendusstringi.

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

## <a name="service-bus-relay"></a>Teenuse siini edastamine

Näidised, mis illustreerivad teenuse siini relay.

### <a name="getting-started"></a>Alustamine

|Proovi nimi|Kirjeldus|Minimaalne SDK versioon|Kättesaadavus|
|---|---|---|---|
|[Edastatakse sõnumside: Azure'i](http://code.msdn.microsoft.com/Relayed-Messaging-Windows-0d2cede3)|Näitab, kuidas Azure teenuse siini klient ja teenuse käivitamine. See näide konfigureerib teenuse siini programmiliselt. Ainult keskkonna ja turvalisuse teave on talletatud failid.|1.8|Microsoft Azure'i teenus siini|
|[Võrdõigusvõrku sõnumside autentimine: Ühiskasutusse antud salajane](http://code.msdn.microsoft.com/Relayed-Messaging-92b04c02)|Näitab, kuidas väljaandja nimi ja väljaandja salajane kasutamise teenuse siini autentimiseks.|1.8|Microsoft Azure'i teenus siini|
|[Kiirsõnumside autentimise ümber: WebNoAuth](http://code.msdn.microsoft.com/Relayed-Messaging-a4f0b831)|Näitab, kuidas seada HTTP teenus, mis ei nõua kliendi kasutaja autentimine.|1.8|Microsoft Azure'i teenus siini|
|[Kiirsõnumside sidumiste ümber: WebHttp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-a6477ba0)|Näitab, kuidas kasutada **WebHttpRelayBinding** sidumine veebi programmeerimine mudeli kasutamine binaarandmeid tagastamiseks.|1.8|Microsoft Azure'i teenus siini|
|[Kiirsõnumside võrdõigusvõrku sidumiste: NetTcp ümber](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-2dec7692)|Näitab, kuidas kasutada **NetTcpRelayBinding** sidumine.|1.8|Microsoft Azure'i teenus siini|

### <a name="exploring-features"></a>Funktsioonide uurimine

Näidised, mis illustreerivad erinevaid teenuse siini relay funktsioone.

|Proovi nimi|Kirjeldus|Minimaalne SDK versioon|Kättesaadavus|
|---|---|---|---|
|[Kiirsõnumside võrdõigusvõrku autentimine: Lihtsa WebToken](http://code.msdn.microsoft.com/Relayed-Messaging-32c74392)|Näitab, kuidas kasutada autentimiseks teenuse siini lihtne web Turbeloa mandaati. Valimi sarnaneb kaja valimi, on mõned muudatused. Täpsemalt, lisab selle valimi käitumist ServiceHost (teenus) ja ChannelFactory (klient) rakendused.|1.8|Microsoft Azure'i teenus siini|
|[Võrdõigusvõrku sõnumid: Laadi saldo](http://code.msdn.microsoft.com/Relayed-Messaging-Load-bd76a9f8)|Näitab, kuidas kasutada Microsoft Azure'i teenus siini mitme vastuvõtjad sõnumite suunamiseks. See näitab mitmes eksemplaris lihtsa teenuse suhtlemine klienti **NetTcpRelayBinding** sidumine|1.8|Microsoft Azure'i teenus siini|
|[Kiirsõnumside võrdõigusvõrku sidumiste: Net sündmuse](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-c0176977)|Näitab **NetEventRelayBinding** sidumine kasutamine Microsoft Azure'i teenus siini.|1.8|Microsoft Azure'i teenus siini|
|[Kiirsõnumside võrdõigusvõrku sidumiste: WS2007Http seanss](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ef1f1fcb)|Näitab **WS2007HttpRelayBinding** sidumine abil usaldusväärne seansid lubatud. See näitab ka teenuse siini identimisteabe määramine konfiguratsioonifailis asemel programmiliselt.|1.8|Microsoft Azure'i teenus siini|
|[Kiirsõnumside võrdõigusvõrku sidumiste: WS2007Http MsgSecCertificate](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-f29c9da5)|Näitab, kuidas kasutada **WS2007HttpRelayBinding** sidumine sõnumi turvalisusega turvamiseks lõpuni sõnumite aga endiselt nõuab kliendi teenuse siini autentimiseks.|1.8|Microsoft Azure'i teenus siini|
|[Võrdõigusvõrku sõnumid: Metaandmete vahetamise](http://code.msdn.microsoft.com/Relayed-Messaging-Metadata-f122312e)|Näitab, kuidas seada metaandmete näitaja, mis kasutab relay sidumine. **MetadataExchange** on toetatud järgmised relay sidumiste: **NetTcpRelayBinding**, **NetOnewayRelayBinding**, **BasicHttpRelayBinding**ja **WS2007HttpRelayBinding**.|1.8|Microsoft Azure'i teenus siini|
|[Kiirsõnumside võrdõigusvõrku sidumiste: NetTcp otsesed](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ca039161)|Näitab, kuidas toetada **ühenduse hübriidrežiimis, mis esmalt loob ühenduse võrdõigusvõrku ja võimaluse korral automaatselt otsest seost klient ja teenuse** **NetTcpRelayBinding** side konfigureerimine.|1.8|Microsoft Azure'i teenus siini|
|[Kiirsõnumside võrdõigusvõrku sidumiste: NetTcp MsgSec kasutajanimi](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-30542392)|Näitab, kuidas kasutada **NetTcpRelayBinding** sidumine sõnumi turvalisusega.|1.8|Microsoft Azure'i teenus siini|
|[Kiirsõnumside võrdõigusvõrku sidumiste: Net Oneway](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-bb5b813a)|Näitab, kuidas seada ja tarbimine teenuse lõpp-punkti **NetOnewayRelayBinding** sidumine abil.|1.8|Microsoft Azure'i teenus siini|
|[Võrdõigusvõrku sõnumside sidumiste: Lihtne WS2007Http](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-aa4b793a)|Näitab **WS2007HttpRelayBinding** sidumine abil. See näitab lihtsa teenus, mis kasutab pole turbesuvandid, mis ei nõua kliendid autentida.|1.8|Microsoft Azure'i teenus siini|

## <a name="next-steps"></a>Järgmised sammud

Järgmistest teemadest leiate kontseptuaalne ülevaated teenuse siini.

- [Teenuse siini relay ülevaade](service-bus-relay-overview.md)
- [Teenuse siini ülesehitus](../service-bus-messaging/service-bus-architecture.md)
- [Teenuse siini põhialused](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)