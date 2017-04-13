<properties
    pageTitle="Azure'i rakendust Service ja Azure olemasolevad teenused mõju"
    description="Selgitab, kuidas uue Azure'i rakendust Service ja selle funktsioonide mõju Azure olemasolevad teenused."
    services="app-service"
    documentationCenter=""
    authors="yochay"
    manager="nirma"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/12/2016"
    ms.author="yochayk"/>


# <a name="azure-app-service-and-existing-azure-services"></a>Azure'i rakendust Service ja Azure olemasolevad teenused

See artikkel kirjeldab olemasoleva Azure'i teenuste muudatustest muudatuse viia kokku mitme Azure'i teenuste [Azure'i rakendust Service](https://azure.microsoft.com/services/app-service/), uue integreeritud pakkumise osana.

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="overview"></a>Ülevaade

[Azure'i rakenduse teenus](https://azure.microsoft.com/services/app-service/) on uus ja ainulaadne pilveteenuses, mis võimaldab arendajatel luua veebi ja mis tahes platvormi ja mis tahes seadme mobiilirakenduste. Rakenduse teenus on integreeritud lahendust sujuvamaks muutmine kodeerimise korduvate ülesannete, integreerida enterprise ja SaaS süsteemid ja samal ajal oma vajaduste turvalisus, töökindluse ja skaleeritavus äriprotsesside automatiseerimine.

Rakenduse teenuse koondab järgmised olemasoleva Azure teenused - [veebisaidid](https://azure.microsoft.com/services/websites/), [Mobile teenuste](https://azure.microsoft.com/services/mobile-services/)ja [BizTalki teenuste](https://azure.microsoft.com/services/biztalk-services/) kombineeritud teenusel lisamisel võimsaid uued võimalused.  Rakenduse teenus võimaldab teil majutada rakenduse järgmist tüüpi:

-   Web Apps
-   Mobiilirakenduste
-   API rakendused
-   Loogika rakendused

Järgmises tabelis selgitatakse olemasolevate Azure'i teenuste kaardi rakenduse teenuse ja rakenduse tüüpi kogumahu saadaval.

<table>
<thead>
<tr class="header">
<th align="left", style="width:10%">Olemasoleva Azure'i teenus</th>
<th align="left", style="width:10%">Azure'i rakendust Service</th>
<th align="left", style="width:80%">Mis on muutunud</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Azure'i veebisaidid</td>
<td align="left">Web Apps</td>
<td align="left"><li>Azure veebilehed, rakenduse teenus on tingimata piiratud veebirakenduste veebisaitide nime muutmata.
<p><li>Nüüd on kõik olemasolevad eksemplarid veebisaitide veebirakenduste rakenduse teenuses.</p>
<p><li>Olemasoleva veebisaidiga <a href="http://go.microsoft.com/fwlink/?LinkId=529715">Azure portaali</a>, kust leiate oma olemasolevatel saitidel jaotises <em>Web Apps</em>kaudu pääsete juurde.</p>
<p><li><em>Web Hosting Plan</em> on nüüd <em>Rakenduse teenuse kavandamine</em>. On <em>Rakenduse teenuse leping</em> majutada mis tahes rakenduse tüüpi rakenduse teenuse, nt Web, mobiiltelefon, loogika või API rakendused.</p>
<p><li>Azure'i rakenduse teenuse Web Apps on üldiselt kättesaadavus.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/web/">Lisateavet veebirakenduste</a>.</p></td>
</tr>
<tr class="even">
<td align="left">Azure'i mobiilsideseadmete teenused</td>
<td align="left">Mobiilirakenduste</td>
<td align="left"><p><li>Mobile teenuste jätkuvalt autonoomse teenus kättesaadav ja jäävad täielikult toetatud.</p>
<p><li>Mobiilirakenduste kohta on rakenduse tüüpi rakenduse teenus, mis ühendab Mobile teenuste ja rohkem funktsioone.</p>
<p><li>See on lihtne migreerida <a href="http://go.microsoft.com/fwlink/?LinkID=724279&clcid=0x409">Mobile teenuste mobiilirakenduste kohta</a>.</p>
<p><li>Rakenduse teenuse osana mobiilirakenduste hankimine uued võimalused Lisaks Mobile teenuste integreerimine kohapealsete ja SaaS süsteemide, nt lavastus slots, WebJobs, paremini skaleerimise suvandid ja muud.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/mobile/">Lisateavet mobiilirakenduste</a>.</p>
</tr>
<tr class="odd">
<td align="left"></td>
<td align="left">API rakendused</td>
<td align="left">
<p><li>API rakendused on uue rakenduse App teenus, mis võimaldab teil hõlpsasti luua ja kasutada API-de pilveteenuses.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/api/">Lisateavet rakenduste API</a>.</p></td>
</tr>
<tr class="even">
<td align="left"></td>
<td align="left">Loogika rakendused</td>
<td align="left">
<p><li>Loogika rakendused on uus rakendus rakenduse teenus, mis võimaldab teil hõlpsasti automatiseerida.</p>
<p><li><a href="http://azure.microsoft.com/services/app-service/logic/">Lisateavet rakenduste loogika</a>.</p></td>
</tr>
<tr class="odd">
<td align="left">Azure'i BizTalki teenused</td>
<td align="left">BizTalki API rakendused</td>
<td align="left">
<li><p>BizTalki teenuste jätkuvalt autonoomse teenus kättesaadav ja jäävad täielikult toetatud.</p>
<li><p>Kõigi võimaluste BizTalki teenuste integreeritakse rakenduse teenuse API rakenduste lubamine kasutajad täita ettevõtte rakenduste integreerimine ja B2B integreerimise stsenaariumid, mis tahes tüüpi rakendus App teenus</p>
<li><p>Loogika rakendustega nüüd automatiseerida visuaalne kujunduskogemus loomine töövoogude abil äriprotsesside</p></td>
</tr>
</tbody>
</table>

Lisateabe saamiseks külastage [rakenduse teenusedokumentatsiooni](https://azure.microsoft.com/documentation/services/app-service/).
