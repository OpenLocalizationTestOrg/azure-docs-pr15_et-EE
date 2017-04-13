<properties
    pageTitle="Azure'i RemoteAppi App-V rakenduste kasutamine | Microsoft Azure'i"
    description="Saate teada, kuidas kasutada App-V rakenduste Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="ericorman"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo" />



# <a name="using-app-v-apps-in-azure-remoteapp"></a>Azure RemoteApp App-V rakenduste kasutamine

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Saate rakenduse-V rakenduste Azure RemoteApp hübriid kogumi, mis nõuab domeeni Liitu.

Enne alustamist veenduge, et installida rakendus-V 5.1 kliendi uusimaid värskendusi. Kas peate looma [kohandatud pilt](remoteapp-create-custom-image.md) , mis sisaldab App-V kliendi.  

See on lihtne App-V infrastruktuuri kasutamine Azure RemoteApp. Kuna hübriid saidikogumi juurutatakse Azure'i VNET, kellel on juurdepääs teie domeenikontrolleri sisse ja VMs on ühendatud domeeni, saate kasutada oma olemasoleva App-v taristu ja juurutamise meetodid App-V hostrakendus easyily Azure RemoteApp. Siin on mõned peaks olema teadlik, põhineb tüüpi App-V juurutus, mis teil praegu on asjaolu.

| Konfiguratsiooni suvandid |                       | Positiivne                                                               | Negatiivne                                                                                              |
|-----------------------|-----------------------|------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|
| Kohaletoimetusviis       | Streaming (vajadusel) | Rakendus on alati Viimane ja värske                                     | Esimese kellaaja latentsus                                                                                    |
|                       | Paigaldatud               | Kiireim; rakendus on juba olemas VM                              | Ballast - kulub pildi ruumi (127 GB limiit)                                                           |
| Rakenduse asukoht salvestusruum  | Ühiskasutusse antud sisu        | Rakendus töötab mälu Azure RemoteApp eksemplari                         | Sööb mälu ja hea ühenduse streaming server (fail), kus asub rakenduse                      |
|                       | Ketas (vahemälus talletatud)         | Kiire teostamine. Rakendus ei sõltu sisuallikas kättesaadavus | Ballast - kulub pildi ruumi (127 GB limiit)                                                           |
| Suunamise             | Kasutaja                  | Nõuab taristu täielik autonoomse App-V                          |                                                                                                       |
|                       | Globaalne (kohapeal)      |  Eelnevalt avaldamine või avaldamise kaudu suunata server                         |  Peate värskendama Azure pilt, kui soovite värskendada (suur) rakenduse. Suunab pilt ruumi vabastada. |

 Kui olete loonud kohandatud pilt ja hübriid kogumisse, avaldamine rakenduse, määrata kasutajatele ja nautida olemasolevaid App-V rakendusi majutatud Azure RemoteApp toimetada mis tahes seadme suvalist kohta.
