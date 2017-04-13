<properties
    pageTitle="Haldus- ja tööülesannete loend BizTalk teenused | Microsoft Azure'i"
    description="Plaanimine ja töö abi Azure BizTalki teenuste juurutamine."
    services="biztalk-services"
    documentationCenter=""
    authors="msftman"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="deonhe"/>

# <a name="administration-and-development-task-list-in-biztalk-services"></a>Haldamine ja arengu ülesandeloendi BizTalki teenused  

## <a name="getting-started"></a>Alustamine
Microsoft Azure'i BizTalki teenuste töötamisel on mitmeid kohapealse ja pilvepõhise komponendid, millega tuleks arvestada. Alustamiseks võtke arvesse järgmist protsessi kulgemist.  

|Toiming|Kes vastutab|Ülesanne|Seotud lingid|
|----|----|----|----|
|1.|Administraator|Loomine Microsoft Azure tellimuse Microsofti konto või ettevõtte konto abil|[Azure'i klassikaline portaal](http://go.microsoft.com/fwlink/p/?LinkID=213885)|
|2.|Administraator|Looge või BizTalki teenuse ettevalmistamise.|[Azure'i klassikaline portaalis BizTalki teenuse loomine](http://go.microsoft.com/fwlink/p/?LinkID=302280)|
|3.|Administraator|Teie või teie ettevõtte BizTalki teenuste juurutamine|[Registreerimisel ja värskendamise BizTalki teenuse juurutamise BizTalki Services portaal](https://msdn.microsoft.com/library/azure/hh689837.aspx)|
|4.|Administraator|Kehtib juhul, kui rakendus kasutab BizTalki adapterit teenuse ühenduse loomiseks kohapealse joon-, äri (LOB) süsteemiga või kasutab järjekorda või teema sihtkoht.  Azure'i teenus siini Namespace luua. Anda selle nimeruumi, teenuse siini väljaandja nimi ja teenuse siini väljaandja klahvi väärtuste arendaja.|[Kuidas: loomine või muutmine teenuse siini teenuse Namespace](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md) ja [saada väljaandja nimi ja väljaandja klahvi väärtused](biztalk-issuer-name-issuer-key.md)|
|5.|Arendaja|Installige SDK ja luua BizTalki teenuse project Visual Studio.|[Installige Azure'i BizTalki teenuste SDK](https://msdn.microsoft.com/library/azure/hh689760.aspx) ja [rikkaliku sõnumside lõpp-punktid Azure loomine](https://msdn.microsoft.com/library/azure/hh689766.aspx)|
|6.|Arendaja|Projekti BizTalki teenust majutatud Azure'i BizTalki teenust juurutamine.|[Juurutamine ja BizTalki teenuste projekti värskendamine](https://msdn.microsoft.com/library/azure/hh689881.aspx)|
|7.|Administraator|Kehtib juhul, kui kasutate EDI.  Saate lisada partnerite ja lepingute loomine Microsoft Azure'i BizTalki Services portaal. Kui loote leping, saate lisada bridge ja/või Teisendusteta loodud arendaja lepingu sätted.|[Konfigureerimise EDI, AS2 ja EDIFACT BizTalki Services portaal](https://msdn.microsoft.com/library/azure/hh689853.aspx)|
|8.|Administraator|Azure'i klassikaline portaali kasutamisel jälgida BizTalki teenust, sh jõudluse mõõdikute seisundi.|[BizTalki Services: Vahekaardid armatuurlaud, jälgimine ja skaala](http://go.microsoft.com/fwlink/p/?LinkID=302281)|
|9.|Administraator|Teenusekomplekti Microsoft Azure'i BizTalki Services portaal, hallata kasutada BizTalki teenuste ja sõnumite jälitamine bridge failide töötleb esemeid.|[BizTalki teenuste portaalis](https://msdn.microsoft.com/library/azure/dn874043.aspx)|
|10.|Administraator|Luua varukoopia BizTalki teenuse varundada.|[Süsteemi järjepidevuse ning avariitaastet BizTalki teenused](https://msdn.microsoft.com/library/azure/dn509557.aspx) |  
## <a name="next-steps"></a>Järgmised sammud
[Õpetused ning näidised](https://msdn.microsoft.com/library/azure/hh689895.aspx)

[Visual Studio projekti loomine](https://msdn.microsoft.com/library/azure/hh689811.aspx)

[Installige Azure'i BizTalki teenuste SDK](https://msdn.microsoft.com/library/azure/hh689760.aspx)

## <a name="concepts"></a>Mõisted
[Visual Studio projekti loomine](https://msdn.microsoft.com/library/azure/hh689811.aspx)  
[EDI AS2 ning EDIFACT sõnumside (Business Business)](https://msdn.microsoft.com/library/azure/hh689898.aspx)  
## <a name="other-resources"></a>Muud ressursid  
[Lisage allikas, sihtkoht ja Bridge sõnumside lõpp-punktid](https://msdn.microsoft.com/library/azure/hh689877.aspx)  
[Siit saate teada, ja sõnumi kaartide ja Teisendusteta loomine](https://msdn.microsoft.com/library/azure/hh689905.aspx)  
[BizTalki adapterit teenuse (BAS)](https://msdn.microsoft.com/library/azure/hh689889.aspx)  
[Azure'i BizTalki teenused](http://go.microsoft.com/fwlink/p/?LinkID=303664)
