<properties
    pageTitle="Saadaolevad konnektorid ja API rakenduste loendit | Microsoft Azure'i rakendust Service"
    description="Lugege konnektorid ja API rakendused rakendus Azure'i teenus"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="mandia"/>


# <a name="list-of-connectors-and-api-apps-to-use-in-your-logic-apps"></a>Konnektorid ja API rakenduste kasutamine loogika rakenduste loendit
>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakenduste 2014-12-01-eelvaade skeemi versioon. Loogika rakenduste General Availability (GA) versiooni, vaadake [Uut konnektorid lisanud](../connectors/apis-list.md).

Teavet kõik saadaolevad konnektorid ja API rakendused Microsofti loodud loogika rakendusi kasutada.

Hinnad teavet ja loendit, mis on kaasas iga teenuse kiht, leiate [Azure'i rakenduse teenuse hinnad](https://azure.microsoft.com/pricing/details/app-service/).

> [AZURE.NOTE] Minge alustada loogika rakendused enne Azure'i konto kasutajaks, [Proovige loogika rakenduse](https://tryappservice.azure.com/?appservice=logic). Saate luua kohe lühiajaline starter loogika rakendus App teenuses. Nõutav; krediitkaardid kohustusi.

## <a name="core-connectors"></a>Core konnektorid
Järgmises tabelis on loetletud kõik saadaolevad konnektorid ja API Microsofti loodud saadaolevate rakenduste Core konnektorid nimega:

Nimi | Kirjeldus
--- | ---
[Bing Translator](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) | Bingi abil saate teksti tõlkida muus keeles.
[HTTP KAUDU](app-service-logic-connector-http.md) | HTTP kuulajale avatakse lõpp-punkti, mis toimib HTTP-serverisse ja sissetulevad HTTP- või HTTPS taotlused on kuulata. HTTP toiming ei nõua rakenduse API ja toetatakse algupäraselt loogika rakendustes.
[Vaikne](app-service-logic-connector-slack.md) | Ühenduse vaikne ja postitada vaikne kanaleid.


## <a name="enterprise-integration-connectors"></a>Ettevõtte integreerimine konnektorid
Järgmises tabelis on loetletud kõik saadaval konnektorid ja API rakendused loodud Microsoft Enterprise integreerimine konnektorid saadaval:

Nimi  | Kirjeldus
------------- | -------------
[BizTalki reeglid](app-service-logic-use-biztalk-rules.md) | BizTalki reeglite abil saate määratleda ja määrata äriloogika ettevõttes. Ettevõtte poliitika saab värskendada ilma ümberkompileerimise või ümbersuunamine seotud rakendusi.
[BizTalki XPath Extractor](app-service-logic-xpath-extract.md) | Otsib ja ekstraktib andmete põhjal valite XPathi XML-sisu.
[DB2 konnektor](app-service-logic-connector-db2.md) | Loob abil IBM DB2 andmebaas kohapealse ja Azure virtuaalne arvutis töötab Windowsi operatsioonisüsteemi. Saate vastendada Web API-ja OData API informixi Structured Query Language käsud. <br/><br/>Pole päästikute. Toimingud kaasa tabeli Vali, lisa, värskendamine, kustutamine ja kohandatud lause<br/><br/>Selle konnektori sisaldab ka Microsoft Client DRDA TCP/IP võrgus informixi serveriga ühendust.
[Faili](app-service-logic-connector-file.md) | Kasutamisega, saate luua ühenduse kohapealse failisüsteemi või võrgu ja täielik erinevad faili tööülesanded, sh üles, kustutamine, loendi failid ja muud.
[Informixi](app-service-logic-connector-informix.md) | Loob IBM informixi andmebaasi, kohapealse ja Azure virtuaalne arvutisse, mis töötab Windowsi operatsioonisüsteemi. Saate vastendada Web API-ja OData API informixi Structured Query Language käsud.<br/><br/>Pole päästikute. Toimingud sisaldavad tabeli Vali, lisa, värskendamine, kustutamine ja kohandatud lause.<br/><br/>Kohapealse kasutamisel saab kasutada VPN- või Azure'i ExpressRoute. Selle konnektori sisaldab ka Microsoft Client DRDA TCP/IP võrgus informixi serveriga ühendust.
[Microsoft SQL Server](app-service-logic-connector-sql.md) | Loob ühenduse asutusesisese SQL serveri või SQL Azure'i andmebaasi. Saate luua, värskendada, hankimine ja SQL-i andmebaasi tabeli kirjeid kustutada.
MQ | Ühendab IBM WebSphere MQ serveri versioon 8 kohapealse ja Azure virtuaalse masina, mis töötab Windowsi operatsioonisüsteemi. Kohapealse kasutamisel saab kasutada VPN- või Azure'i ExpressRoute. Konnektor sisaldab ka Microsoft Client MQ.<br/><br/>Pole päästikute. Toiminguid pole.<br/><br/>**Märkus** Praegu ei saa kasutada loogika rakendustega.

## <a name="connectors-as-triggers"></a>Päästikute nimega konnektorid
Päästikute pakuvad mitut konnektorid loogika rakendused. Nende päästikute on kahte tüüpi:

1. Küsitluse päästikute: Need päästikute küsitlus määratud sagedusega uute andmete otsimiseks teenust. Kui uued andmed on saadaval, loogika rakenduse uue eksemplari käivitatakse andmetega sisendina. Samade andmete takistada tarbitud mitu korda, käivitab võib puhastada lugeda ja edasi loogika rakenduse andmed. Sellise konnektorid on faili ja SQL Azure'i salvestusruumi.
2. Tõuketeatised päästikute: Need päästikute kuulavad andmete lõpp-punkti jaoks või sündmuse tekkida. Seejärel päästik loogika rakenduse uue eksemplari. HTTP kuulajale ja Twitteri on näiteks selline konnektorid.

## <a name="connectors-as-actions"></a>Konnektorid nimega toimingud
Konnektorid saate kasutada ka oma loogika rakenduses toimingud. Toimingud on kasulikud loogika rakendus, mida saate siis kasutada täitmise andmete otsimine. Näiteks soovite otsida andmeid SQL-andmebaasi kohta lisateabe kliendi tellimuse töötlemine. Või, peate võib-olla kirjutamine, värskendamine ja kustutamine andmete sihtkoht. Saate seda teha, kasutades toimingud, mida konnektorid. Toimingud Vastendage toimingute API rakendustes (määratletud oma ärplema metaandmed).

## <a name="create-your-own-connectors-and-api-apps"></a>Luua oma konnektorid ja API rakendused
[Konnektorid ja API rakenduste viited](http://aka.ms/appservicesconnectorreference)  
[Azure'i rakenduse teenuse API rakenduse päästikute](../app-service-api/app-service-api-dotnet-triggers.md)  
[Loogika rakenduse viide](https://msdn.microsoft.com/library/azure/dn948510.aspx)

## <a name="more-on-connectors-and-api-apps"></a>Lisateavet konnektorid ja API rakendused
[Mis on konnektorid ja BizTalki API rakendusi](app-service-logic-what-are-biztalk-api-apps.md)  
[Azure'i rakendust Service hübriid ühenduse halduri kasutamine](app-service-logic-hybrid-connection-manager.md)  
[Hallata ja jälgida oma sisseehitatud API rakendused ja konnektorid](app-service-logic-monitor-your-connectors.md)
