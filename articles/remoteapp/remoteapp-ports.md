
<properties
    pageTitle="Loendi pordid ja URL-ide nimekirja nimekiri Azure RemoteApp juurutatud kliendi virtuaalse võrgus | Microsoft Azure'i"
    description="Siit saate teada, millised pordid ja URL-ide peate kaudu Azure RemoteApp side konfigureerimine."
    services="remoteapp"
    documentationCenter=""
    authors="mghosh1616"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/16/2016"
    ms.author="elizapo" />



# <a name="list-of-ports-and-urls-to-permit-access-for-azure-remoteapp-deployed-in-customer-virtual-network"></a>Loendi pordid ja URL-id, et võimaldada juurdepääsu jaoks Azure RemoteApp juurutatud kliendi virtuaalse võrgu 

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Kehtib järgmine Azure RemoteApp pilveteenuses või hübriid saidikogumi kui juurutate virtuaalse võrku (VNET). Virtuaalne võrkude kohta lisateabe saamiseks lugege [Virtuaalse võrgu ülevaade](../virtual-network/virtual-networks-overview.md). Kui olete loonud piiramine liikluse oma virtuaalse võrgu ressursse, mille olete valinud Azure RemoteApp võrgu turberühma (NSG), veenduge, et puuetega inimestele juurdepääsetavate ja lubatud turbepoliitikate, virtuaalse võrgu kaudu. Võrgu turberühmad kohta lisateabe saamiseks lugege [, mis on võrgu turberühma? (NSG)](../virtual-network/virtual-networks-nsg.md).

##  <a name="azure-remoteapp-subnet-needs-access-to-these-endpoints-and-urls"></a>Azure RemoteApp alamvõrgu vajab juurdepääsu nende lõpp-punktid ja URL-ID: 
*   *. servicebus.windows.net
*    *. servicebus.net
*    https://*.RemoteApp.windwsazure.com  
*    https://www.RemoteApp.windowsazure.com 
*    https://*RemoteApp.windowsazure.com  
*    https://*.Core.Windows.net  
*    Väljamineva meili: TCP: TCP 443,: 10101-10175 
*    Valikuline – UDP: 10201-10275  
 
## <a name="azure-remoteapp-clients-need-access-to-these-endpoints-and-urls"></a>Azure RemoteApp kliendid on vaja juurdepääsu nende lõpp-punktid ja URL-ID: 

Klientide ma tähenda lauaarvutite, seadmete jne selle inimesed kasutavad rakendused juurutatud Azure RemoteApp saidikogumi loomiseks.

-  https://telemetry.RemoteApp.windowsazure.com  
-  (valikuline UDP-pordid on see aadress) https://*.RemoteApp.windowsazure.com 
-  https://login.Windows.net  
-  https://login.microsoftonline.com  
-  https://www.RemoteApp.windowsazure.com 
-  https://*.Core.Windows.net  
-  Väljamineva meili: TCP: 443  
-  Valikuline – UDP: 3391 