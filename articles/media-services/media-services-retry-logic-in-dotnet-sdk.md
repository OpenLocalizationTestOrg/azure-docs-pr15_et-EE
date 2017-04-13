<properties
    pageTitle="Uuestiproovimise loogikale Media Services SDK .net-i jaoks | Microsoft Azure'i"
    description="Teema annab ülevaate uuesti loogika Media Services SDK .net-i jaoks."
    authors="Juliako"
    manager="erikre"
    editor=""
    services="media-services"
    documentationCenter=""/>

<tags
    ms.service="media-services"
    ms.workload="media"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016" 
    ms.author="juliako"/>


# <a name="retry-logic-in-the-media-services-sdk-for-net"></a>Uuestiproovimise loogikale Media Services SDK .net-i jaoks

Microsoft Azure'i teenuste töötades võib ilmneda siirdamiseks vead. Siirdamiseks vea juhul enamasti mõne korduskatsed pärast toimingu õnnestub. Media Services SDK .net-i rakendab käsitlema siirdamiseks vead seotud erandid ja tõrkeid, mis põhjustavad web nõuab, proovige uuesti loogika tööd päringute, salvestades muudatused ja salvestusruumi toimingud.  Vaikimisi aktiveeritakse Media Services SDK .net-i neli korduskatsed enne uuesti korrutamine erandi rakenduse. Rakenduse koodi siis tuleb toime seda erandit õigesti.  
  
 Lühike juhend Web koosolekukutse, salvestusruumi, päringu ja SaveChanges poliitika on järgmine:  
  
-   Salvestusruumi poliitika kasutatakse bloobimälu salvestusruumi toimingute (üles või alla laadima varade failide).  
  
-   Web taotluse poliitika kasutatakse üldist taotlemine (nt saada autentimine on luba ja lahendamine kasutajate kobar lõpp-punkti).  
  
-   Päringu poliitika kasutatakse päringu üksuste ülejäänud (nt mediaContext.Assets.Where(...)).  
  
-   SaveChanges poliitika kasutatakse tee midagi, mis muudab andmeid teenuses (nt luua üksuse värskendamine üksuse toimingu teenuse funktsiooni helistaja).  
  
 Selles teemas on loetletud erand ja tõrkekoodide, mida käsitletakse Media Services SDK .net-i uuestiproovimise loogikale.  
  
## <a name="exception-types"></a>Erandi tüübid  

Järgmises tabelis kirjeldatakse erandid, mis Media Services SDK .net-i tegeleb või ei oska teatud toiminguid, mis võivad põhjustada siirdamiseks vead.  
  
Erandi|Veebipäring|Salvestusruumi|Päringu|SaveChanges
----|------|----|---|---
WebException<br/>Lisateavet leiate jaotisest [WebException oleku koode](media-services-retry-logic-in-dotnet-sdk.md#WebExceptionStatus) .|Jah|Jah|Jah|Jah  
DataServiceClientException<br/> Lisateabe saamiseks vt [HTTP tõrkekoodid olek](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Ei|Jah|Jah|Jah
DataServiceQueryException<br/> Lisateabe saamiseks vt [HTTP tõrkekoodid olek](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Ei|Jah|Jah|Jah  
DataServiceRequestException<br/> Lisateabe saamiseks vt [HTTP tõrkekoodid olek](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Ei|Jah|Jah|Jah  
DataServiceTransportException|Ei|Ei|Jah|Jah
TimeoutException|Jah|Jah|Jah|Ei
SocketException|Jah|Jah|Jah|Jah  
StorageException|Ei|Jah|Ei|Ei 
IOException|Ei|Jah|Ei|Ei
  
###  <a name="WebExceptionStatus"></a>WebException oleku koodid  

Järgmine tabel näitab, millised WebException tõrkekoodid puhul uuesti loogika rakendatakse. [WebExceptionStatus](http://msdn.microsoft.com/library/system.net.webexceptionstatus.aspx) loendamine määratleb olek koode.  
  
Olek|Veebipäring|Salvestusruumi|Päringu|SaveChanges  
-----|-----------------|-------------|-----------|----------  
ConnectFailure|Jah|Jah|Jah|Jah
NameResolutionFailure|Jah|Jah|Jah|Jah  
ProxyNameResolutionFailure|Jah|Jah|Jah|Jah  
SendFailure|Jah|Jah|Jah|Jah
PipelineFailure|Jah|Jah|Jah|Ei  
ConnectionClosed|Jah|Jah|Jah|Ei  
KeepAliveFailure|Jah|Jah|Jah|Ei  
UnknownError|Jah|Jah|Jah|Ei 
ReceiveFailure|Jah|Jah|Jah|Ei  
RequestCanceled|Jah|Jah|Jah|Ei  
Ajalõpp|Jah|Jah|Jah|Ei
Protokollitõrge <br/>Uuesti sisse protokollitõrge kontrollib HTTP oleku koodi töötlemine. Lisateabe saamiseks vt [HTTP tõrkekoodid olek](media-services-retry-logic-in-dotnet-sdk.md#HTTPStatusCode).|Jah|Jah|Jah|Jah|  
  
###  <a name="HTTPStatusCode"></a>HTTP tõrkekoodid olek  

Päringu-ja SaveChanges põhjustada DataServiceClientException, DataServiceQueryException või DataServiceQueryException, tagastatakse atribuudi StatusCode HTTP tõrkekoodi olek.  Järgmine tabel näitab, millised tõrkekoodid puhul uuesti loogika rakendatakse.  
  
 
Olek|Veebipäring|Salvestusruumi|Päringu|SaveChanges 
---|----|----|----|----
401|Ei|Jah|Ei|Ei
403|Ei|Jah<br/>Korduskatsed enam ootab ja käitlemine.|Ei|Ei  
408|Jah|Jah|Jah|Jah
429|Jah|Jah|Jah|Jah  
500|Jah|Jah|Jah|Ei  
502|Jah|Jah|Jah|Ei  
503|Jah|Jah|Jah|Jah  
504|Jah|Jah|Jah|Ei  
  
Kui soovite võtta pilk Media Services SDK reaalselt .net-i uuestiproovimise loogikale, leiate [Azure'i-sdk-jaoks-meediumi-teenused](https://github.com/Azure/azure-sdk-for-media-services/tree/dev/src/net/Client/TransientFaultHandling).

## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Tagasiside saatmine

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]
