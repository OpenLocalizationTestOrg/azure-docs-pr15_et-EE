<properties
    pageTitle="Rakendust avaldada Azure RemoteApp | Microsoft Azure'i"
    description="Saate teada, kuidas avaldada Azure RemoteApp rakenduste ja ressursse."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />


# <a name="how-to-publish-an-app-in-remoteapp"></a>Kuidas avaldada rakenduse RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Pärast oma RemoteApp saidikogumi loomist peate avaldada rakendused või ressursse, mida soovite muuta oma kasutajate jaoks saadaval. Mallide piltide koos teie tellimus on ainult paari rakenduste avaldatud vaikimisi - muude rakenduste ühiskasutusse anda, peate avaldada.

> [AZURE.NOTE] Kas peate värskendama rakendus? Peate värskendama [pildi](remoteapp-update.md) esmalt.

**Avaldamise** menüü portaalis, klõpsake nuppu **Avalda**. Saate lisada rakenduse malli pilti menüü **Start** või tee kui rakendus on installitud malli pilti. Kui otsustate lisada menüü **Start** , valige loendist avaldada rakendus. Kui valite rakenduse tee, sisestage nimi rakenduse ja tee rakendus. Kasutage muutujad tee – näiteks "systemdrive %" asemel "c:\".

> [AZURE.NOTE] Kui soovite lisada rakenduse menüü **Start** , peab teil olema *lisanud, et rakendus on * *käivitamine* * menüüs oma malli pilti.* Muul juhul RemoteApp ainult näete, millised *on* menüü **Start** ja te segi. 

>Veenduge, et teie rakendus on menüü **Start** , et asetada %systemdrive%\ProgramData\Microsoft\Windows\Start MS kausta otsetee - **LNK** - faili.

> Kui olete unustanud rakenduse lisamine menüü **Start** , kui olete loonud malli, valige tee lisamiseks rakenduse. (Või looge oma malli pilti, kuid see on üsna veidi lisatööd.)


 