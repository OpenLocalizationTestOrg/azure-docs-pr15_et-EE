<properties 
    pageTitle="Juhtimise API võti mõisted" 
    description="Lisateavet API-d, toodete, rollid, rühmad ja muude halduse API võti mõisted." 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

#<a name="what-is-api-management"></a>Mis on API haldus?

API haldus aitab ettevõtted avaldada API-väliste, partnerite ja sisemise arendajad rakendada oma andmeid ja võimalusi. Ettevõtted kõikjal on kas digitaalse platvormi oma tegevust laiendada, loomise uued kanalid, uute klientide otsimine ja juhtimise süvitsi kaasamine olemasolevate. API haldus pakub core oskusi eduka API programmi kaudu arendaja kaasamine, äri teadmisi, analüüsi, Turve ja kaitse tagamiseks.

Vaadake järgmisest videost ülevaate Azure'i API haldus ja saate teada, kuidas API halduse abil saate lisada oma API, sh juurdepääsu reguleerimine, rate piiramine, jälgimine, sündmuste logimine ja vastuse vahemällu minimaalsete töötamise teiepoolse mitmeid funktsioone.

> [AZURE.VIDEO azure-api-management-overview]

API halduse kasutamiseks administraatorid loovad API-d. Iga API koosneb ühe või mitme toimingute ja iga API saab lisada ühe või mitme. API kasutamiseks arendajate tellida toode, mis sisaldab selle API ja seejärel nad saavad helistada mis tahes kasutuse poliitika, mis võib olla tegelikult on API tegevus.

Selles teemas antakse ülevaade halduse API võti mõisted.

>[AZURE.NOTE] Lisateavet leiate teemast soovitud [pilvepõhist API haldus: rakendamine API Power](http://j.mp/ms-apim-whitepaper) PDF-i lühiülevaade. Selle sissejuhatavad lühiülevaade API haldamise CITO uuringud hõlmab: 
>
> - Levinud API nõuded ja probleemid
>     - Lahutamine API-d ja fassaadid esitamine
>     - Saada arendajate ja töötab kiiresti
>     - Accessi turvamine
>     - Analüüsi- ja mõõdikud
> - Juhtelemendi ja ülevaate platvormi API haldus
> - Pilveteenuse vs kohapealne lahenduste abil
> - Azure'i API haldus

## <a name="apis"> </a>API-d ja toimingud

API-d on API halduse teenuse eksemplar. Iga API tähistab toimingute arendajatele. Iga API sisaldab viidet tagaandmebaas teenus, mis rakendab API ja toimingute kaart toimingute rakendanud teenuse tagaandmebaas. Toimingute API haldus on hästi konfigureeritav juhtida URL-i vastenduse, päringu ja parameetrite, taotlus ja vastuse sisu ja toimingu vastuse vahemällu. Piirangu, kvootide ja IP piirang poliitika saab rakendada API või toimingu tasemel.

Lisateabe saamiseks vt [API-de loomiseks][] ja [Kuidas lisada API toimingud][].


## <a name="products"></a> Tooted

Tooted on, kuidas API-d on uurinud arendajatele. Toodete API Management on üks või mitu API-d ja pealkiri, kirjeldus ja kasutustingimused on konfigureeritud. Toodete võib olla **avatud** või **kaitstud**. Kaitstud tooted on tellinud enne neid kasutada, samal ajal Ava toodete saab kasutada ilma tellimus. Kui toode on tarkvaraarendajad, kes saab avaldada kasutamiseks valmis. Kui see on avaldatud, seda saab vaadata (ja kaitstud toodete puhul tellinud) arendajatele. Tellimuse kinnitamise on konfigureeritud toote tasemel ja saate administraatori kinnitamise nõudmine või olema automaatse kinnitatud.

Rühmade kasutatakse toodete arendajatele nähtavuse haldamine. Toodete anda nähtavuse rühmade ja arendajad saaksid vaadata ja tellimine tooted, mis on nähtavad rühmad, kuhu nad kuuluvad. 

Lisateabe saamiseks lugege teemat [Kuidas luua ja avaldada toote][] ja järgmisest videost.

> [AZURE.VIDEO using-products]

## <a name="groups"></a> Rühmad

Rühmade kasutatakse toodete arendajatele nähtavuse haldamine. API haldus on järgmised püsiv süsteemi rühmad.

-   **Administraatorid** - Azure tellimuse administraatorid on selle rühma liikmed. Administraatorite haldamine API halduse eksemplare, API-d, toiminguid ja tooted, mis kasutavad tarkvaraarendajad loomine.
-   **Arendajate** - autenditud arendaja portaali kasutajad kuuluvad sellesse rühma. Arendajad on klientidele, et luua rakendusi, kasutades oma API-d. Arendajad on antud juurdepääs arendaja portaali ja luua rakendusi, millel helistada API toimingud.
-   **Külalistele** - autentimata arendaja portaali kasutajatele, näiteks tulevaste klientide külastamise arendaja portaali API halduse eksemplari kuuluvad sellesse rühma. Nad saavad anda teatud kirjutuskaitstud juurdepääsu, näiteks võimalust vaadata API-d, kuid mitte helistada.

Lisaks rühmade süsteem, administraatorid saate luua kohandatud rühmade või [kasutada välise rühmade seotud Azure Active Directory rentnikud](api-management-howto-aad.md#how-to-add-an-external-azure-active-directory-group). Kohandatud ja välise rühmad saab süsteemi rühmade arendajate nähtavuse ja juurdepääsu andmine API toodete kõrval. Näiteks võib arendajatele seotud kindla partneri ettevõtte ühest kohandatud rühma loomine ja võimaldab neile juurdepääsu selle API-de toode, mis sisaldab ainult asjakohaste API. Kasutaja võib olla mitu rühma liige.

Lisateabe saamiseks vaadake, [Kuidas luua ja kasutada rühmad][].

## <a name="developers"></a> Arendajad

Arendajate tähistavad kasutajakontode API halduse teenuse eksemplariga. Arendajad saate loodud või kutsutud liituma administraatorid või nad saavad registreeruda [arendaja portaali][]. Iga arendaja on üks või mitu rühma liige ja võib olla tooted, mida anda nähtavuse nende rühmade tellimine.

Kui toote tellimine arendajad on antud õigus selle esmaseid ja teiseseid tootenumber. Selle klahvi kasutatakse toote API kõne tegemisel.

Lisateabe saamiseks lugege teemat [Kuidas luua või kutsuda arendajad][] ja [Kuidas rühmad arendajatele siduda][].

## <a name="policies"></a> Poliitika

Poliitika on võimas võimalus API juhtimise, mis võimaldavad avaldaja, kelle soovite muuta serveri käitumist API konfigureerimise kaudu. Poliitika on kogumi laused, mis täidetakse järjest taotluse või vastuse API. Populaarsed laused kaasata vorming üleminekul JSON ja kõne rate piiramine piirata summa sissetulevate kõnede arendaja XML-i ja palju muu poliitika on saadaval.

Poliitika avaldiste saate kasutada atribuudiväärtused ega tekstväärtusi mis tahes API teabehalduspoliitikate juhul, kui poliitika määrab teisiti. Mõned poliitika, näiteks [Control flow](https://msdn.microsoft.com/library/azure/dn894085.aspx#choose) ja [määrata muutuja](https://msdn.microsoft.com/library/azure/dn894085.aspx#set-variable) poliitikate põhinevad poliitika avaldised. Lisateabe saamiseks vt [Täpsemalt poliitikad](https://msdn.microsoft.com/library/azure/dn894085.aspx#AdvancedPolicies), [poliitika avaldiste](https://msdn.microsoft.com/library/azure/dn910913.aspx)ja vaadake järgmist videot.

> [AZURE.VIDEO policy-expressions-in-azure-api-management]

API teabehalduspoliitikate täieliku loendi leiate teemast [poliitika juhend][]. Kasutamine ja konfigureerimine poliitika kohta leiate lisateavet teemast [API teabehalduspoliitikate][]. Vt luua toode määr limiit ja kvoodi õpetus, [Kuidas luua ja täiustatud toote sätete konfigureerimine][]. Demo, vaadake järgmist videot.

> [AZURE.VIDEO rate-limits-and-quotas]

## <a name="developer-portal"></a> Arendaja portaal

Portaali arendaja on kus saate arendajate teave oma API-d, kuvamine ja kõne toimingute ja tellimine tooted. Tulevaste klientide külastada arendaja portaalis API-d ja toimingute vaatamine ja registreeruda. Teie arendaja portaali URL-i asub armatuurlaua Azure'i klassikaline portaalis oma API halduse eksemplari.

Saate kohandada oma arendaja portaali ilme ja lisada kohandatud sisu, laadide kohandamine ja lisada oma sümboolika.

## <a name="api-management-and-the-api-economy"></a>API haldus ja API Standard

API haldamise kohta lisateabe saamiseks vaadake järgmist esitluse Microsoft Ignite 2015 konverentsi.

> [AZURE.VIDEO microsoft-ignite-2015-azure-api-management-and-the-api-economy]

[APIs and operations]: #apis
[Products]: #products
[Groups]: #groups
[Developers]: #developers
[Policies]: #policies
[Portaali arendaja]: #developer-portal

[Kuidas luua API-d]: api-management-howto-create-apis.md
[Kuidas lisada toimingute API]: api-management-howto-add-operations.md
[Kuidas luua ja avaldada toote]: api-management-howto-add-products.md
[Kuidas luua ja kasutada rühmad]: api-management-howto-create-groups.md
[Rühmade seostamist arendajad]: api-management-howto-create-groups.md#associate-group-developer
[Kuidas luua ja täiustatud toote sätete konfigureerimine]: api-management-howto-product-with-rules.md
[Kuidas luua või kutsuda arendajad]: api-management-howto-create-or-invite-developers.md
[Poliitika viide]: api-management-policy-reference.md
[API teabehalduspoliitikate]: api-management-howto-policies.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance



 
