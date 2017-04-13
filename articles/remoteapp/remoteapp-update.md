<properties
   pageTitle="Värskendage oma Azure RemoteApp saidikogumi | Microsoft Azure'i"
   description="Siit saate teada, kuidas värskendada oma saidikogumi Azure RemoteApp"
   services="remoteapp"
   documentationCenter=""
   authors="lizap"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="remoteapp"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="compute"
   ms.date="08/15/2016"
   ms.author="elizapo"/>

# <a name="update-a-collection-in-azure-remoteapp"></a>Värskendage kogumik Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Tulevad aja paratamatult, kui peate värskendama rakendused või pildi oma Azure RemoteApp saidikogumi. Kui kasutate ühte kaasas tellimuse Azure RemoteApp pilveteenuses või hübriid kogumise, pildid tarkvaras kõik värskendused Azure RemoteApp ise, et hoiate saate lihtne.

Juhul, kui kasutate kohandatud pildi (ehitatud nullist või loodud muutes ühte meie pildid), olete eest pilt ja rakendused. Kui teil on vaja värskendada oma pildi või mõnda rakendust sees, peate luua uus, värskendatud versiooni pilt ja teie saidikogumi olemasoleva pildi asendamiseks selle uue värskendatud pilt.

Jah, kuidas lähete oma saidikogumi värskendamise kohta? See on üsna lihtne:

1. Pilt, mida kasutasite oma saidikogumi värskendamine Rakendada plaastrit või vajalikud värskendused ja seejärel salvestage see uue nimega.
2. [Üles laadida](remoteapp-uploadimage.md) või [importimine](remoteapp-image-on-azurevm.md) mis pilti, et RemoteApp.
3. Nüüd klõpsake lehel Saidikogumi, klõpsake nuppu **Värskenda**.
4. Valige loendist **Malli pilti** uus pilt.
4. Siin on keeruline osa – peate otsustada, kuidas toimida kõigile kasutajatele, kes kasutavad rakendust kogumist. On teil järgmised valikud.
    - **Kasutajatele 60 minutit pärast värskendamist**. Kui värskendus on lõpule jõudnud, kuvatakse Azure RemoteApp sõnumi mis tahes aktiivsed kasutajad räägib neile oma tööd salvestada ja logige välja ja uuesti sisse logida. Pärast 60 minutit, logitakse mis tahes aktiivsed kasutajad, kes on logib automaatselt välja. Kasutajad saavad kohe logige uuesti sisse.
    - **Logige välja kasutajad kohe**. Kui värskendus on valmis, logige kõik kasutajad automaatselt ilma mis tahes hoiatus. Kui valite selle suvandi, võivad kasutajad andmed lähevad kaotsi. Siiski ta saab taastada rakendusse kohe.

1. Klõpsake märkeruutu värskendamise alustamiseks.
