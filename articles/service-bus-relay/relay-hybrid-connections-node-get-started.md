<properties
    pageTitle="Alustamine Relay hübriid ühendused | Microsoft Azure'i"
    description="Kuidas kirjutada sõlm konsooli rakendus hübriid ühendused"
    services="service-bus"
    documentationCenter="node"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="hero-article"
    ms.tgt_pltfrm="node"
    ms.workload="na"
    ms.date="10/28/2016"
    ms.author="jotaub"/>

# <a name="get-started-with-relay-hybrid-connections"></a>Alustamine Relay hübriid ühendused

[AZURE.INCLUDE [relay-selector-hybrid-connections](../../includes/relay-selector-hybrid-connections.md)]

## <a name="what-will-be-accomplished"></a>Mida saab teha

Kuna hübriid ühendused nõuab serveri komponent ja kliendi, loome kaks konsooli rakendusi selles õpetuses. Siin on toodud juhiseid.

1. Looge Relay nimeruumi, Azure'i portaalis.

2. Saate luua ühenduse hübriid, Azure'i portaalis.

3. Kirjutage serveri konsooli rakendus sõnumeid.

4. Kirjutage mõnda muud klienti konsooli rakendus sõnumite saatmiseks.

## <a name="prerequisites"></a>Eeltingimused

1. [Node.js](https://nodejs.org/en/) (see näide kasutab sõlm 7.0).

2. Azure'i tellimuse.

[AZURE.INCLUDE [create-account-note](../../includes/create-account-note.md)]

## <a name="1-create-a-namespace-using-the-azure-portal"></a>1. nimeruumi Azure portaali loomine

Kui teil on juba loodud Relay nimeruumi, liikuda jaotise [Loo ühendus hübriidjuurutuse abil Azure portaali](#2-create-a-hybrid-connection-using-the-azure-portal) .

[AZURE.INCLUDE [relay-create-namespace-portal](../../includes/relay-create-namespace-portal.md)]

## <a name="2-create-a-hybrid-connection-using-the-azure-portal"></a>2. Azure'i portaalis hübriid ühenduse loomine

Kui teil on juba loodud ühenduse hübriid, liikuda jaotise [Loo serveri rakendus](#3-create-a-server-application-listener) .

[AZURE.INCLUDE [relay-create-hybrid-connection-portal](../../includes/relay-create-hybrid-connection-portal.md)]

## <a name="3-create-a-server-application-listener"></a>3. server rakenduse (kuulajale) loomine

Kuulata ja selle edastamine sõnumeid saada, me kirjutada Node.js konsooli rakendus.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-server](../../includes/relay-hybrid-connections-node-get-started-server.md)]

## <a name="4-create-a-client-application-sender"></a>4. loomine klientrakendusega (saatja)

Sõnumite saatmiseks on Relay me kirjutada Node.js konsooli rakendus.

[AZURE.INCLUDE [relay-hybrid-connections-dotnet-get-started-client](../../includes/relay-hybrid-connections-node-get-started-client.md)]

## <a name="5-run-the-applications"></a>5. rakendusi käivitada.

1. Käivitage rakendus serveri.

2. Käivitage rakendus klient ja sisestage tekst.

3. Veenduge, et serveri rakenduse konsooli väljundid klientrakenduse sisestatud teksti.

![töötab-rakendused](./media/relay-hybrid-connections-node-get-started/running-applications.png)

Palju õnne, olete loonud rakenduse lõpuni hübriid ühendused.

## <a name="next-steps"></a>Järgmised toimingud:

- [Relay KKK](relay-faq.md)
- [Nimeruumi loomine](relay-create-namespace-portal.md)
- [.Net-i kasutamise alustamine](relay-hybrid-connections-dotnet-get-started.md)
- [Alustamine sõlm](relay-hybrid-connections-node-get-started.md)