<properties 
    pageTitle="Kuidas kohandada Azure API arendaja haldusportaali mallide kasutamine | Microsoft Azure'i" 
    description="Saate teada, kuidas kohandada Azure API arendaja haldusportaali mallide kasutamine." 
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
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>


# <a name="how-to-customize-the-azure-api-management-developer-portal-using-templates"></a>Kuidas kohandada Azure API arendaja haldusportaali mallide kasutamine

Azure'i API haldus pakub mitut kohandamise funktsiooni, mis võimaldavad kohandada [ilme ja olemus arendaja portaali](api-management-customize-portal.md)administraatorid, samuti sisu arendaja portaali lehtede kogumi malle, mida konfigureerimine sisu ise lehtede kohandamine. [DotLiquid](http://dotliquidmarkup.org/) süntaks ja esitatud kogumi lokaliseeritud stringide ressursid, ikoone ja lehe juhtelementide kasutamise teil suurt paindlikkust konfigureerida vastavalt vajadusele need mallide kasutamine lehtede sisu.

## <a name="developer-portal-templates-overview"></a>Arendaja portaali Mallid ülevaade

Arendaja portaali Mallid haldab arendaja portaalis API halduse eksemplari administraatorid. Arendaja mallide haldamiseks liikuge oma API halduse eksemplari Azure'i klassikaline portaali ja klõpsake nuppu **Sirvi**.

![Portaali arendaja][api-management-browse]

Kui olete juba Publisheri portaalis, pääsete, klõpsates **arendaja portaali**arendaja portaali.

![Portaali menüü Arendaja][api-management-developer-portal-menu]

Juurdepääs arendaja portaali Mallid, klõpsake vasakul kohandamine menüü kuvamiseks ikooni kohandamine ja nuppu **Mallid**.

![Arendaja portaali Mallid][api-management-customize-menu]

Mallide loend kuvatakse mitu kategooriat malle, mis hõlmab erinevate lehtede arendaja portaalis. Iga mall on erinev, kuid samme, et neid redigeerida ja avaldada muudatused on samad. Malli redigeerimine, klõpsake soovitud malli nime.

![Arendaja portaali Mallid][api-management-templates-menu]

Malli klõpsamine viib teid arendaja portaali leht, mis on kohandatav malli abil. Selles näites **tooteloendi** kuvatakse malli. **Toote** loendimalli juhtelemendid Kuva, mis on tähistatud punase ristküliku pindala. 

![Toodete loendimalli][api-management-developer-portal-templates-overview]

Mõned Mallid, nt **Kasutajaprofiili** malle, kohandada erinevate osade sama lehe. 

![Kasutaja profiili Mallid][api-management-user-profile-templates]

Iga arendaja portaali malli redaktor on kaks jaotist, kuvatakse lehe allosas. Vasakus servas kuvatakse malli redigeerimise paani ja paremal pool kuvatakse andmemudeli malli. 

Paani redigeerimine Mall sisaldab märgistus, mis määrab ilme ja käitumise arendaja portaalis vastavat lehte. Märgistuse malli kasutab [DotLiquid](http://dotliquidmarkup.org/) süntaks. Ühe DotLiquid populaarsed redaktor on [DotLiquid kujundajate jaoks](https://github.com/dotliquid/dotliquid/wiki/DotLiquid-for-Designers). Malli muutusi redigeerimise ajal kuvatakse reaalajas brauseris, kuid ei ole nähtav oma klientidele, kuni te [Salvesta](#to-save-a-template) ja [Avalda](#to-publish-a-template) mall.

![Malli märgistus][api-management-template]

Paanil **malli andmete** pakub andmemudeli juhend üksused, mis on kindla malli kasutamiseks saadaval. Sellest juhendist pakub, kuvades reaalajas andmed, mida praegu kuvatakse arendaja portaalis. Saate laiendada, klõpsates nuppu ristkülik **malli andmete** paani paremas allnurgas paanide malli.

![Malli andmemudel][api-management-template-data]

Eelmises näites on kaks arendaja portaalis kuvatakse tooted, mis olid tuua andmete **malli andmeid** kuvada, nagu on näidatud järgmises näites.

    {
        "Paging": {
            "Page": 1,
            "PageSize": 10,
            "TotalItemCount": 2,
            "ShowAll": false,
            "PageCount": 1
        },
        "Filtering": {
            "Pattern": null,
            "Placeholder": "Search products"
        },
        "Products": [
            {
                "Id": "56ec64c380ed850042060001",
                "Title": "Starter",
                "Description": "Subscribers will be able to run 5 calls/minute up to a maximum of 100 calls/week.",
                "Terms": "",
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            },
            {
                "Id": "56ec64c380ed850042060002",
                "Title": "Unlimited",
                "Description": "Subscribers have completely unlimited access to the API. Administrator approval is required.",
                "Terms": null,
                "ProductState": 1,
                "AllowMultipleSubscriptions": false,
                "MultipleSubscriptionsCount": 1
            }
        ]
    }

Märgistuse **tooteloendi** malli töötleb andmeid anda soovitud väljund iterating toodete kuvada teavet lingi iga toote kogum. Märkus selle `<search-control>` ja `<page-control>` elementide märgistuse. Need määrata funktsiooni otsimise ja Piipar lehekülje juhtelementide kuvamiseks. `ProductsStrings|PageTitleProducts`on lokaliseeritud string lahtriviide, mis sisaldab soovitud `h2` lehe päise tekst. Stringi loendi ressursid, lehe juhtelementide ja ikoonid saadaval arendaja portaali mallide kasutamine leiate teemast [API haldus veebiportaali Mallid tootearendusmaterjal](https://msdn.microsoft.com/library/azure/mt697540.aspx).

    <search-control></search-control>
    <div class="row">
        <div class="col-md-9">
            <h2>{% localized "ProductsStrings|PageTitleProducts" %}</h2>
        </div>
    </div>
    <div class="row">
        <div class="col-md-12">
        {% if products.size > 0 %}
        <ul class="list-unstyled">
        {% for product in products %}
            <li>
                <h3><a href="/products/{{product.id}}">{{product.title}}</a></h3>
                {{product.description}}
            </li>   
        {% endfor %}
        </ul>
        <paging-control></paging-control>
        {% else %}
        {% localized "CommonResources|NoItemsToDisplay" %}
        {% endif %}
        </div>
    </div>

## <a name="to-save-a-template"></a>Malli salvestamiseks

Malli salvestamiseks klõpsake nuppu Salvesta Mall redaktoris.

![Malli salvestamine][api-management-save-template]

Salvestatud muudatused ei ole reaalajas arendaja portaalis, kuni need on avaldatud.

## <a name="to-publish-a-template"></a>Malli avaldada.

Salvestatud malle saab avaldada ükshaaval või kõik korraga. Mõni üksikute Mall avaldamiseks klõpsake nuppu Avalda malli redaktoris.

![Vormimalli avaldamine][api-management-publish-template]

Klõpsake nuppu **Jah** kinnitage ja veenduge, et mall reaalajas arendaja portaalis.

![Kinnitage avaldamine][api-management-publish-template-confirm]

Kõik praegu avaldamata malli versioonid avaldamiseks klõpsake nuppu **Avalda** mallide loendi. Avaldamata Mallid on määratud tärni malli nime. Selles näites on avaldatakse **tooteloendi** ja **toote** mallid.

![Avaldamine Mallid][api-management-publish-templates]

Klõpsake nuppu **Avalda kohandused** kinnitamiseks.

![Kinnitage avaldamine][api-management-publish-customizations]

Äsja avaldatud Mallid on kohe arendaja portaali.

## <a name="to-revert-a-template-to-the-previous-version"></a>Malli eelmise versiooni taastamine

Malli avaldatud eelmise versiooni taastamine, klõpsake nuppu tagasi malli redaktoris.

![Malli tagasipööramine][api-management-revert-template]

Klõpsake nuppu **Jah** kinnitamiseks.

![Kinnitage][api-management-revert-template-confirm]

Malli varem avaldatud versioon on reaalajas arendaja portaalis, kui Taasta toiming on lõpule viidud.

## <a name="to-restore-a-template-to-the-default-version"></a>Malli taastamiseks vaikeversiooniks

Nende vaikimisi versiooni taastamine Mallid on kaheosaline toiming. Esmalt malle taastamist ning seejärel taastatud versioonides peab olema avaldatud.

Ühe malli vaikeversiooniks taastamiseks klõpsake raadionuppu Taasta malli redaktoris.

![Malli tagasipööramine][api-management-reset-template]

Klõpsake nuppu **Jah** kinnitamiseks.

![Kinnitage][api-management-reset-template-confirm]

Kõik Mallid oma vaikimisi versiooni taastamiseks klõpsake **taastamine vaikemallid** mallide loendi.

![Mallide taastamine][api-management-restore-templates]

Taastatud Mallid siis tuleb avaldada ükshaaval või korraga [avaldada malli](#to-publish-a-template)juhiseid järgides.

## <a name="developer-portal-templates-reference"></a>Portaali Mallid tootearendusmaterjal

Viide arendaja portaali Mallid, stringi ressursid, ikoonid ja lehe juhtelementide leiate artiklist [API halduse portaali Mallid tootearendusmaterjal](https://msdn.microsoft.com/library/azure/mt697540.aspx).

## <a name="watch-a-video-overview"></a>Vaadake videot ülevaade

Järgmised videost saate teada, kuidas arutelutahvli ja hinnangute lisamine API ja toiming lehtedele arendaja portaalis mallide kasutamine.

> [AZURE.VIDEO adding-developer-portal-functionality-using-templates-in-azure-api-management]


[api-management-customize-menu]: ./media/api-management-developer-portal-templates/api-management-customize-menu.png
[api-management-templates-menu]: ./media/api-management-developer-portal-templates/api-management-templates-menu.png
[api-management-developer-portal-templates-overview]: ./media/api-management-developer-portal-templates/api-management-developer-portal-templates-overview.png
[api-management-template]: ./media/api-management-developer-portal-templates/api-management-template.png
[api-management-template-data]: ./media/api-management-developer-portal-templates/api-management-template-data.png
[api-management-developer-portal-menu]: ./media/api-management-developer-portal-templates/api-management-developer-portal-menu.png
[api-management-browse]: ./media/api-management-developer-portal-templates/api-management-browse.png
[api-management-user-profile-templates]: ./media/api-management-developer-portal-templates/api-management-user-profile-templates.png
[api-management-save-template]: ./media/api-management-developer-portal-templates/api-management-save-template.png
[api-management-publish-template]: ./media/api-management-developer-portal-templates/api-management-publish-template.png
[api-management-publish-template-confirm]: ./media/api-management-developer-portal-templates/api-management-publish-template-confirm.png
[api-management-publish-templates]: ./media/api-management-developer-portal-templates/api-management-publish-templates.png
[api-management-publish-customizations]: ./media/api-management-developer-portal-templates/api-management-publish-customizations.png
[api-management-revert-template]: ./media/api-management-developer-portal-templates/api-management-revert-template.png
[api-management-revert-template-confirm]: ./media/api-management-developer-portal-templates/api-management-revert-template-confirm.png
[api-management-reset-template]: ./media/api-management-developer-portal-templates/api-management-reset-template.png
[api-management-reset-template-confirm]: ./media/api-management-developer-portal-templates/api-management-reset-template-confirm.png
[api-management-restore-templates]: ./media/api-management-developer-portal-templates/api-management-restore-templates.png







