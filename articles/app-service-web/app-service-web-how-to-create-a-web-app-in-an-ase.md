<properties
    pageTitle="Luua veebirakenduse rakenduse teenuse keskkonnas"
    description="Saate teada, kuidas luua veebirakenduste ja rakenduse teenuse lepingute rakenduse teenuse keskkonnas"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article" 
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="create-a-web-app-in-an-app-service-environment"></a>Luua veebirakenduse rakenduse teenuse keskkonnas

## <a name="overview"></a>Ülevaade

Selle õpetuse näitab, kuidas luua veebirakenduste ja rakenduse teenuse lepingute [Rakenduse teenuse keskkonnas](app-service-app-service-environment-intro.md) (ASE). 

> [AZURE.NOTE] Kui soovite teada, kuidas luua veebirakenduse, kuid ei pea seda teha rakenduse teenuse keskkonnas, lugege teemat [loomine .NET web appi](web-sites-dotnet-get-started.md) või mõnda muude keelte ja raamistiku seotud õpetused.

## <a name="prerequisites"></a>Eeltingimused

Selle õpetuse eeldab, et olete loonud rakenduse teenuse keskkonnas. Kui see pole seda veel teinud, lugege teemat [loomine rakenduse teenuse keskkonnas](app-service-web-how-to-create-an-app-service-environment.md). 

## <a name="create-a-web-app"></a>Veebirakenduse loomine

1. Valige [Azure portaali](https://portal.azure.com/) **uus > Web + Mobile > Web Appi**. 

    ![][1]

2. Valige oma tellimus.  

    Kui teil on mitu tellimust pidage meeles, et teie rakendus teenuse keskkonnas rakenduse loomiseks peate kasutama sama tellimus, mida kasutasite keskkonna loomisel. 

3. Valige või looge ressursirühma.

    *Ressursi rühmad* võimaldavad Azure'i seotud ressursid haldamine üksus ja on kasulik, kui rakenduste eeskirjad *Rollipõhine juurdepääsu reguleerimine* (RBAC). Lisateabe saamiseks vaadake teemat [haldamine oma Azure ressursse][ResourceGroups]. 

4. Valige või looge soovitud rakendus teenusleping.

    *Rakenduse teenuse lepingute* hallatakse veebirakenduste komplekti.  Tavaliselt hinnad valimisel hind on rakendatud rakenduse teenusleping, mitte üksikute rakendused. Mõne ASE maksate Arvuta eksemplarid, mis on eraldatud ASE selle asemel, mis on loetletud teie ASP.  Web appi, skaalal rakenduse teenust eksemplarid eksemplaride arv skaalal leping ja see mõjutab kõiki veebirakenduste plaanis.  Mõned funktsioonid, näiteks saidi enda või VNET integreerimine on ka kogus piiranguid leping.  Lisateavet leiate teemast [Azure'i rakendust Service lepingute ülevaade](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)

    Saate tuvastada rakenduse plaanide oma ASE, vaadates asukohta, mis on märgitud lepingu nime all.  

    ![][5]

    Kui soovite kasutada rakendust Service lepingut, mis rakenduse teenuse keskkond juba olemas, valige see režiim. Kui soovite luua uue rakenduse teenusleping, vaadake järgmist jaotist õppetüki [loomine teenuse keskkonnas rakenduse App teenusleping](#createplan).

5. Sisestage oma veebirakenduse nimi ja seejärel klõpsake nuppu **Loo**. 

    Kui teie ASE kasutab mõnda välise VIP rakendus on ASE URL on: [*sitename*]. [*rakenduse teenuse keskkonna nimi*]. [*sitename*] asemel p.azurewebsites.net. azurewebsites.net
    
    Kui teie ASE kasutab on sisemine VIP siis URL-i rakendus on ASE: [*sitename*]. [*alamdomeen määratud ASE loomise ajal*]   
    Kui valite oma ASP ASE loomise ajal kuvatakse alamdomeen **nime** all värskendamine

## <a name="createplan"></a>Mõni rakendus teenusleping loomine

Rakenduse teenuse keskkonnas on rakenduse teenusleping loomisel teie töötaja Valikud on erinevad nagu ka ASE on pole ühiskasutusega töötajate.  Peate kasutama töötajad on need, mis on eraldatud ASE-administraator.  See tähendab, et luua uue lepingu, peate on veel kõigis teie plaanid selle töötaja pargis juba oma ASE töötaja rakenduskausta kui eksemplaride arv eraldatud töötajate.  Kui teil pole piisavalt töötajate oma ASE töötaja kausta loomiseks oma lepingut, peate oma ASE administraatori saada neid lisatud töötamiseks.

Teine erinevus rakenduse teenuse lepingutega, mis on majutatud rakenduse teenuse keskkond on hinnad valiku puudumine.  Kui teil on rakenduse teenuse keskkond maksavad Arvuta ressursid, kasutab ja on lisatud kulude plaanid selles keskkonnas.  Tavaliselt on rakenduse teenusleping loomisel valite hinnakirjad leping, mis määratleb arvelduse.  Rakenduse teenuse keskkond on põhiosas privaatne asukoht, kus saate luua sisu.  Maksate keskkonnas, mitte majutada oma sisu.

Järgmised juhised näitab, kuidas luua ka rakenduse teenusleping õpetuse eelmises jaotises kirjeldatud veebirakenduse loomise ajal.

1. Klõpsake nuppu **Loo uus** leping valikusse UI ja sisestage oma lepingu nimi nagu tavaliselt on ASE väljaspool.

2. Valige ASE, mida soovite kasutada oma asukohta Kuupäevavalija.

    Kuna keskkonnas rakenduse teenus on põhiosas privaatne juurutamise asukohta, kuvatakse jaotises asukoht. 

    ![][2]

    Pärast valiku mõne ASE asukoht valija, rakenduse teenuste leping loomiseks UI värskendused.  Asukoht kuvatakse nüüd ASE süsteem ja regiooni nimi see on ja hinnakirjad leping valija on asendatud funktsiooniga töötaja rakenduskausta valija.  

    ![][3]

### <a name="selecting-a-worker-pool"></a>Töötaja kausta valimine

Tavaliselt Azure'i rakendust Service ja väljaspool keskkonnas rakenduse teenus on 3 Arvuta suurused, mis on saadaval kõige uuemas sihtotstarbeline hind lepingu valimine.  Sarnasel viisil, on ASE jaoks saate määratleda töötajate teid kuni 3 ja määrata Arvuta suurus, mida kasutatakse selle töötaja pool.  Mida see tähendab rentnikud ASE, on selle asemel et hinnakirjad kava koos oma rakenduse teenusleping Arvuta suuruse valimine, mida nimetatakse *töötaja pool*valida.  

Töötaja pool valiku UI näitab Arvuta suurus, kasutatakse selle töötaja pool nime all.  Saadaval kogus viitab mitu arvutada eksemplarid on selle kogumi kasutamiseks saadaval.  Kokku pool võib olla mitu eksemplari arvust, kuid see väärtus viitab lihtsalt mitu ei kasutata.  Kui teil on vaja lisada veel arvutada ressursse leiate [rakenduse teenuse keskkonna konfigureerimise](app-service-web-configure-an-app-service-environment.md)teenuse rakenduse keskkonna reguleerimiseks.

![][4]

Selles näites kuvatakse ainult kahe töötaja kaustu saadaval. On põhjus ASE administraator eraldatud hosts ainult need kaks töötaja kaustadesse.  Kolmas näitaks üles, kui seal on eraldatud sinna VMs.  

## <a name="after-web-app-creation"></a>Pärast web appi loomine

On paar kaalutlused töötab veebirakenduste ja haldamine rakenduse plaanide ASE, mida tuleb arvesse võtta.  

Nagu varem, ASE omanik vastutab süsteemi suurust ja selle tulemusena nad on ka tagada, et majutada soovitud rakendus plaanide piisava mahuga eest vastutav. Kui pole saadaval töötajate, ei saa luua oma rakenduse teenusleping.  See on õige oma veebirakenduse ülespoole.  Kui teil on vaja mitu eksemplari siis pean oma rakenduse teenuse keskkonna admin lisada rohkem töötajate saada.

Pärast luua oma web app ja rakenduse teenusleping on mõistlik mastaapimiseks see.  Mõne ASE alati peate olema vähemalt 2 eksemplari rakenduse teenusleping rakenduste tõrketaluvust pakkuda.  Skaleerimist on rakenduse teenusleping sisse ka ASE on sama nagu tavaliselt rakenduse teenusleping Kasutajaliidese kaudu.  Skaala [mastaapimiseks web appi keskkonnas rakenduse teenuse kohta](app-service-web-scale-a-web-app-in-an-app-service-environment.md) lisateabe saamiseks

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
[ResourceGroups]: http://azure.microsoft.com/documentation/articles/resource-group-portal/
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
