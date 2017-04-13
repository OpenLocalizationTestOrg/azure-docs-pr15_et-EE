<properties
    pageTitle="Lisada Azure API Management jõudluse parandamiseks vahemällu | Microsoft Azure'i"
    description="Siit saate teada, kuidas parandada latentsus, läbilaskevõime kulu ja veebiteenuse laadimine API halduse teenuse kõnede jaoks."
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
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="sdanie"/>

# <a name="add-caching-to-improve-performance-in-azure-api-management"></a>Azure'i API Management jõudluse parandamiseks vahemällu lisamine

API haldamise toimingute saab konfigureerida vastuse vahemällu. Vastuse vahemällu võib oluliselt vähendada API latentsus, läbilaskevõime kulu, ja web teenuse laadi muutuda sageli andmete.

Sellest juhendist näidatakse, kuidas lisada oma API vahemällu vastuse ja valimi kaja API toimingute poliitikate konfigureerimine. Seejärel saate helistada toimingu arendaja portaalis vahemällu toimingu kinnitamiseks.

>[AZURE.NOTE] Vahemällu üksuste vajutamisega poliitika avaldiste kasutamise kohta leiate teavet teemast [kohandatud vahemällu Azure'i API haldus](api-management-sample-cache-by-key.md).

## <a name="prerequisites"></a>Eeltingimused

Enne toimingutega sellest juhendist, peab teil olema rakendusliidest API halduse teenuse eksemplar ja toote konfigureeritud. Kui te pole veel loonud API halduse teenuse eksemplar, teemast [Azure API kasutamist alustada][] õpetuses [loomine API halduse teenuse eksemplar][] .

## <a name="configure-caching"> </a>Vahemällu toimingu konfigureerimine

Selles etapis tuleb vaatate valimi kaja API toimimise **Ressursi (vahemälus talletatud) saada** vahemällu talletamise sätetest.

>[AZURE.NOTE] Iga API halduse eksemplari on eelkonfigureeritud koos kaja API, mis saab kasutada eksperimenteerida ning te saate tutvuda API haldus. Lisateavet leiate teemast [Azure API kasutamist alustada][].

Alustamiseks klõpsake nuppu **Halda** API halduse teenust Azure klassikaline portaalis. See viib teid Publisheri API haldusportaali.

![Publisheri portaal][api-management-management-console]

Klõpsake vasakul menüüst **API halduse** **API-de** ja klõpsake **Kaja API -ga**.

![Kaja API][api-management-echo-api]

Klõpsake vahekaarti **toimingud** ja seejärel klõpsake nuppu **saada ressursi (vahemälus talletatud)** **toimingute** loendist toimingu.

![Kaja API toimingud][api-management-echo-api-operations]

Klõpsake vahekaarti **vahemällu salvestamine** vahemällu talletamise sätetest selle toimingu jaoks kuvamiseks.

![Vahemällu menüü][api-management-caching-tab]

Vahemällu toimingu lubamiseks märkige ruut **Luba** . Selles näites on lubatud vahemällu.

Iga toimingu vastuse on sisestatud alusel väärtused väljadele **Varieeri päringustringi parameetrite** ja **Varieeri päised** . Kui soovite vahemälu päringustringi parameetrite või päiste alusel mitu vastust, saate need konfigureerida need kaks väljad.

**Kestus** määrab aegumise intervalli vahemällu talletatud vastused. Selles näites intervall on **3600** sekundit, mis on võrdne tund.

Selles näites vahemällu konfiguratsiooni kasutamisel **saada ressursi (vahemälus talletatud)** toimingu esimese taotluse tagastab vastuse kirjutamata teenuse. Selle vastus salvestatakse vahemällu, määratud päised ja päringustringi parameetrite sisestatud. Toimingu, kattuvad parameetritega järgnevate kõned on vahemällu talletatud vastuse tagastatud, kuni vahemälu kestus intervall on aegunud.

## <a name="caching-policies"> </a>Vahemällu poliitika

Selles etapis tuleb teil läbi vaadata vahemällu talletamise sätetest valimi kaja API toimimiseks **saada ressursi (vahemälus talletatud)** .

Kui toimingu menüü **vahemällu salvestamine** vahemällu talletamise sätetest on konfigureeritud, lisatakse vahemällu poliitikate toimimiseks. Need poliitikad, saate vaadata ja redigeerida rühmapoliitika redaktoris.

Klõpsake vasakul menüüst **API halduse** **poliitikate** ja seejärel valige **kaja API / saada ressursi (vahemälus talletatud)** **toiming** rippmenüü loendist.

![Poliitika ulatus toiming][api-management-operation-dropdown]

Selle toimingu jaoks poliitikate kuvatakse rühmapoliitika redaktoris.

![API rühmapoliitika halduskonsoolis][api-management-policy-editor]

Selle toimingu jaoks poliitika määratlus sisaldab poliitika, mis määrab vahemällu konfiguratsiooni, mis olid läbi eelmises etapis **vahemälu** vahekaardi abil.

    <policies>
        <inbound>
            <base />
            <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
                <vary-by-header>Accept</vary-by-header>
                <vary-by-header>Accept-Charset</vary-by-header>
            </cache-lookup>
            <rewrite-uri template="/resource" />
        </inbound>
        <outbound>
            <base />
            <cache-store caching-mode="cache-on" duration="3600" />
        </outbound>
    </policies>

>[AZURE.NOTE] Vahemällu poliitikate poliitika redaktoris tehtud muudatused kajastuvad **vahemälu** vahekaardil toiming ja vastupidi.

## <a name="test-operation"> </a>Kõne toimingu ja testimine ning vahemällu

Selle toimingu vahemällu vaatamiseks me kutsume toimingu arendaja portaalist. Klõpsake ülemise parempoolse menüü **arendaja portaalis** .

![Portaali arendaja][api-management-developer-portal-menu]

Klõpsake **API-de** ülemises menüüs ja seejärel valige **Kaja API**.

![Kaja API][api-management-apis-echo-api]

>Kui teil on ainult üks API konfigureeritud või näevad teie konto, siis API-de klõpsamine viib teid otse toimingud, et API.

Valige toiming **saada ressursi (vahemälus talletatud)** , ja klõpsake nuppu **Ava konsool**.

![Avatud konsooli][api-management-open-console]

Konsooli võimaldab autonoomsest toimingud otse portaalis arendaja.

![Konsooli][api-management-console]

Säilita **param1** ja **param2**vaikeväärtust.

Valige soovitud klahvi **- märkimise** ripploendist. Kui teie konto on ainult üks tellimus, pole see juba valitud.

Sisestage tekstiväljale **taotlemine päised** **sampleheader:value1** .

Klõpsake **HTTP Get** ja kirjutage vastus päised.

Sisestage tekstiväljale **taotlemine päised** **sampleheader:value2** ja klõpsake **HTTP Get**.

Pange tähele, on endiselt **sampleheader** väärtuse **väärtus1** vastuse. Proovige mõni muu väärtus ja pange tähele, et tagastatakse esimene vahemällu vastuse.

Sisestada **25** **param2** väljale ja seejärel klõpsake nuppu **HTTP Get**.

Pange tähele, on nüüd **sampleheader** vastuseks väärtuse **väärtus2**. Kuna toimingu tulemused on sisestatud päringustringi järgi, eelmine vahemällu talletatud vastus ei tagastatud.

## <a name="next-steps"> </a>Järgmised sammud

-   Vahemällu poliitika kohta leiate lisateavet teemast [vahemällu poliitikate][] [API halduse poliitika viide][].
-   Vahemällu üksuste vajutamisega poliitika avaldiste kasutamise kohta leiate teavet teemast [kohandatud vahemällu Azure'i API haldus](api-management-sample-cache-by-key.md).

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Azure'i API kasutamist alustada]: api-management-get-started.md

[API halduse poliitika viide]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Vahemällu poliitika]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[API halduse teenuse eksemplari loomine]: api-management-get-started.md#create-service-instance

[Configure an operation for caching]: #configure-caching
[Review the caching policies]: #caching-policies
[Call an operation and test the caching]: #test-operation
[Next steps]: #next-steps
