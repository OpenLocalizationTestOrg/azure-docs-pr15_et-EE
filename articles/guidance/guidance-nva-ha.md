<properties
   pageTitle="Virtuaalne seadmed juurutamine kõrge kättesaadavus | Microsoft Azure'i"
   description="Kuidas võtta võrgu virtuaalse seadmete kõrge kättesaadavus."
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="telmos"/>

# <a name="deploying-virtual-appliances-in-high-availability"></a>Virtuaalne seadmed juurutamine kõrge kättesaadavus

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

See artikkel kirjeldab tavade väga kättesaadav viisil võrgu virtuaalne seadmed (NVAs) kasutamise kohta. Enne jätkamist, veenduge, et teil mõista, kuidas [kasutaja määratletud marsruudib (UDR)] [ udr-overview] ja [laadimine koormusetasakaalustusteenuse] [ lb-overview] Azure töö.

Saate erinevate NVAs saadaval Azure'i turuplatsil laiendavad Azure'i seadmete kasutamine oma kohapealse andmekeskuse samal viisil. Järgmisel joonisel on [ühe Dessandi] valimi kasutuselevõtt[ nva-scenario] tulemüüri seadme nimega. 

![[0]][0]

Kuigi eelnev juurutamise töötab, see toob ühtne tõrge. Kui virtuaalse seadme nurjub, pole liikluse juhtimine. Selle probleemi lahendamiseks peate kasutama mitut NVAs. Siiski, mis eeldab muud sätted ja ressursse, mida kasutatakse vastavalt oma vajadustele.

Ühte järgmistest lahendustest abil saate juurutada tugevalt saadaval Dessandi keskkonnas.

|Lahendus|Eelised|Kaalutlused|
|---|---|---|
|Sissepääsu layer 7 virtuaalse seadmete|Kõik sõlmed on aktiivne|Nõutav on Dessandi, mida saab lõpetada ühendused ja SNAT kasutamine<br/>Nõuab eraldi kogum NVAs liikluse Interneti kaudu ja Azure<br/>Saab kasutada ainult väljastpoolt Azure'i pärit liikluse|
|Sissepääsu-sealt layer 7 virtuaalse seadmete|Kõik sõlmed on aktiivne<br/>Võimalik liikluse pärit Azure'i reageerimine |Nõutav on Dessandi, mida saab lõpetada ühendusi ja kasutada SNAT<br/>Nõuab eraldi kogum NVAs liikluse Interneti kaudu ja Azure|
|PIP-UDR vahetamine|Kõik liikluse NVAs ühtsed<br/>Võite hakkama kõik liiklust (pole piiratud port reeglid)|Aktiivne passiivne<br/>Nõuab Tõrkesiirde protsess|

## <a name="ingress-with-layer-7-virtual-appliances"></a>Sissepääsu layer 7 virtuaalse seadmete
Kogumi NVAs taga mõnda Azure laadi koormusetasakaalustusteenuse abil saate esitada Ühenduvus Azure töökoormus võrdlemisi väike hulk serveripoolne pordid (nt HTTP ja HTTPS) taga. Järgmisel joonisel tõstab esile pakkuda kõrge kättesaadavus sel Dessandi tasemel tehke järgmist.

![[1]][1]

Selle stsenaariumi korral peate võrgu virtuaalse seadme kasutatud lõpetada kõik ühendused ja edastada alamvõrgu töökoormus. Töökoormus virtuaalmasinates (VM) vastata selle Dessandi talle kuvati taotluse ja probleemideta liikluse puhul. 

## <a name="ingress-egress-with-layer-7-virtual-appliances"></a>Sissepääsu-sealt layer 7 virtuaalse seadmete
Saate laiendamine eelmise arhitektuur ja lisage veel mõni NVAs reageerimine liikluse pärinevate Azure'i saabuvaid NVAs, nagu on näidatud järgmisel joonisel:

![[2]][2]

Selle stsenaariumi korral suunatakse kõik liikluse pärit Azure sisemine laadi koormusetasakaalustusteenuse, mis jagab vahel NVAs teistsugused. Need NVAs suunaks liikluse Interneti kaudu oma üksikute avaliku IP-aadressid. 

## <a name="pip-udr-switch"></a>PIP-UDR vahetamine
Saate vältida loomine mitme Dessandi virnadena aktiivne passiivne režiimis kahe NVAs abil. Selle stsenaariumi korral saate vahetada avaliku IP-aadress (PIP) ja kasutaja määratletud marsruudib (UDRs) kui aktiivse sõlme lõpetab.  

![[3]][3]

See stsenaarium on sarnane ühe Dessandi stsenaariumi. Ainus erinevus on, et PIP ja UDRs tuleb muuta selle NVAs vahel liikumiseks. Järgmisi muudatusi saab teha käsitsi või saate automatiseerida need. Automatiseerimiseks, saate juurutada NVAs, nii et rakendus, mis kontrollib aktiivse sõlme seisundi. Kui aktiivne sõlm on alla, saate muuta rakenduse PIP ja UDRs passiivne sõlme linkida.

Võimalik rakendamine see lahendus on kasutada mõnda [ZooKeeper] [ zookeeper] deemon NVAs reageerimine juhataja valimine (otsustada, milline sõlm on aktiivne sõlm). Kui juht on valitud, see nõuab Azure'i REST API TEKKEPÕHJUSTEGA eemaldamine nurjunud sõlm ja manustada selle juhataja. Tuleks muuta ka UDRs osutamiseks uue juhataja privaatne IP-aadress.

## <a name="next-steps"></a>Järgmised sammud

- Siit saate teada, kuidas [rakendada mõne DMZ Azure ja teie asutusesisese andmekeskuse vahelise] [ dmz-on-prem] kiht-7 NVAs abil.
- Siit saate teada, kuidas [rakendada mõne DMZ Azure ja Interneti vahel] [ dmz-internet] kiht-7 NVAs abil.

<!-- links -->
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[lb-overview]: ../load-balancer/load-balancer-overview.md
[zookeeper]: https://zookeeper.apache.org/
[nva-scenario]: ../virtual-network/virtual-network-scenario-udr-gw-nva.md
[dmz-on-prem]: guidance-iaas-ra-secure-vnet-hybrid.md
[dmz-internet]: guidance-iaas-ra-secure-vnet-dmz.md

<!-- images -->
[0]: ./media/guidance-nva-ha/single-nva.png "Ühe Dessandi arhitektuur"
[1]: ./media/guidance-nva-ha/l7-ingress.png "Layer 7 sissepääsu"
[2]: ./media/guidance-nva-ha/l7-ingress-egress.png "Kiht 7 sissepääsu ja sealt"
[3]: ./media/guidance-nva-ha/active-passive.png "Aktiivne passiivne kobar"