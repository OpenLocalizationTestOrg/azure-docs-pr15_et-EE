<properties
    pageTitle="Oma API Azure'i API haldusega kaitsta | Microsoft Azure'i"
    description="Siit saate teada, kuidas kaitsta oma API kvootide ja ahendamise poliitikate (määr piirata)."
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

# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>Määr piirangud Azure'i API haldusega kaitsta oma API

Sellest juhendist näete, kui lihtne on Kaitske oma kirjutamata API konfigureerimisega määr limiit ja kvoodi poliitikate haldamise Azure'i API-ga.

Selles õpetuses loote "Tasuta prooviversioon" API toode, mis võimaldab arendajatel kuni 10 kõned minutis ja kuni 200 kõned nädalas oma API [limiit kõne määr, per tellimus](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) ja poliitikate [seadmine kvoodi tellimuse kohta](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) . Seejärel saate avaldada API ning testida määr limiit poliitika.

[Rate-limiit-järgi-võti](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) ja [kvoodi-võti](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) reeglite abil täpsemate ahendamise stsenaariumid, leiate [Täpsemad taotluse Azure'i API haldamise ahendamine](api-management-sample-flexible-throttling.md).

## <a name="create-product"> </a>Toote loomine

Selles etapis loote tasuta prooviversioon toode, mis ei nõua tellimuse kinnitamine.

>[AZURE.NOTE] Kui teil juba on konfigureeritud toote ja soovite seda kasutada selles õpetuses, saate [konfigureerimine kõne määr limiit ja kvoodi][] juurde liikumiseks ja järgige seal kasutamise asemel tasuta prooviversioon toote õpetuse.

Alustamiseks klõpsake nuppu **Halda** API halduse teenust Azure Classic. See viib teid Publisheri API haldusportaali.

![Publisheri portaal][api-management-management-console]

>Kui te pole veel loonud API halduse teenuse eksemplar, teemast [loomine eksemplari API halduse teenuse][] [haldamine oma esimese API Azure'i API Management][] õpetuses.

Klõpsake **toodete** **toodete** lehe kuvamiseks vasakul menüüs **API haldus** .

![Toote lisamine][api-management-add-product]

Klõpsake nuppu **Lisa toote** **Lisa uus toode** dialoogiboksi kuvamiseks.

![Lisa uus toode][api-management-new-product-window]

Tippige väljale **tiitel** **Tasuta prooviversioon**.

Tippige väljale **Kirjeldus** järgmine tekst:  **tellijad saab käitada 10 kõned minutis kuni 200 kõned nädalas, pärast mida juurdepääs keelatud.**

API halduse võib olla kaitstud või avada. Kaitstud toodete peab tellinud, enne kui saate neid kasutada. Avatud toodete saab ilma tellimus. Veenduge, et **nõuab tellimust** on valitud kaitstud toode, mis nõuab tellimuse loomiseks. See on vaikesäte.

Kui soovite läbi vaadata ja aktsepteerimiseks või hülgamiseks tellimuse püüab selle toote administraator, valige **tellimuse kinnitamise nõudmine**. Kui ruut on märkimata, saab tellimuse katsete automaatse kinnitatud. Selles näites tellimused automaatselt kinnitada, et ruut.

Arendaja kontod mitu korda tellida uus toode lubamiseks märkige ruut **Luba mitu samaaegne tellimust** . Selle õpetuse kasutada samaaegselt mitu tellimust, et jätta märkimata.

Kui kõik väärtused on sisestanud, klõpsake nuppu **Salvesta** toote loomiseks.

![Toode on lisatud][api-management-product-added]

Vaikimisi kuvatakse uute toodete **administraatorite** rühma kasutajad. Me **arendajate** jaotise lisada. Klõpsake **Tasuta prooviversioon**ja seejärel vahekaarti **nähtavuse** .

>API haldus, kasutatakse rühmade haldamiseks nähtavuse toodete arendajatele. Toodete anda nähtavuse rühmade ja arendajad saaksid vaadata ja tellimine tooted, mis on nähtavad rühmad, kuhu nad kuuluvad. Lisateabe saamiseks vaadake, [Kuidas luua ja kasutada rühmade Azure'i API haldus][].

![Arendajate rühma lisamine][api-management-add-developers-group]

Märkige ruut **arendajad** ja klõpsake siis nuppu **Salvesta**.

## <a name="add-api"> </a>Toote API lisamiseks

Selles etapis tuleb õpetuse lisame kaja API uue tasuta prooviversioon.

>Iga eksemplari API haldamise tuleb eelnevalt konfigureeritud eksperimenteerida ja õppida API halduse kasutatavate kaja rakendusliidest. Lisateabe saamiseks lugege artiklit [haldamine oma esimese API Azure'i API haldamine][].

Klõpsake vasakul menüüst **API halduse** **tooted** ja klõpsake **Tasuta prooviversiooni** toote konfigureerimiseks.

![Toote konfigureerimine][api-management-configure-product]

Klõpsake **Toote API lisada**.

![Toote API lisamine][api-management-add-api]

Valige **Kaja API**ja klõpsake siis nuppu **Salvesta**.

![Kaja API lisamine][api-management-add-echo-api]

## <a name="policies"> </a>Kõne määr limiit ja kvoodi poliitikate konfigureerimine

Poliitika redaktoris on konfigureeritud piirmäärad ja piirmäärasid. Klõpsake vasakul menüüs **API haldamise** **poliitikate** . Klõpsake loendis **tootenumber** **Tasuta prooviversioon**.

![Toote poliitika][api-management-product-policy]

Klõpsake nuppu **Lisa poliitika** importida poliitika mallide ja alustage määra limiit ja kvoodi reeglite loomine.

![Teabehalduspoliitika lisamine][api-management-add-policy]

Poliitika lisamiseks asetage kursor kas **sissetulevaid** või **väljaminevaid** jaotisse poliitika malli. Määr limiit ja kvoodi reeglid on sissetulevad reeglid, asetage kursor nii sissetulevate elementi.

![Poliitika redaktor][api-management-policy-editor-inbound]

Selles õpetuses lisame kaks poliitika on poliitikate [limiit kõne määr, per tellimus](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) ja [määrata kvoodi tellimuse kohta](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) .

![Poliitika laused][api-management-limit-policies]

Pärast seda, kui kursor asub **sissetuleva** poliitika elementi, klõpsake **limiit kõne määr, per tellimuse** lisamiseks oma malli kõrval olevat noolt.

    <rate-limit calls="number" renewal-period="seconds">
    <api name="name" calls="number">
    <operation name="name" calls="number" />
    </api>
    </rate-limit>

**Piiratud kõne arv tellimuse kohta** saab kasutada toote tasemel ja saab kasutada ka API ja toimingu nimi tasandil. Selles õpetuses kasutatakse ainult toote taseme poliitikad, nii kustutada **api** ja **toiming** elementide **- piirangu** element, ainult väline **piirangu** elemendi jääb, nagu on näidatud järgmises näites.

    <rate-limit calls="number" renewal-period="seconds">
    </rate-limit>

Tasuta prooviversiooni toote, suurim lubatud kõne on 10 kõned minutis, seega sisestage **10** **kõned** atribuudi väärtus ja **60** **- ajavahemiku** atribuudi.

    <rate-limit calls="10" renewal-period="60">
    </rate-limit>

Poliitika **määramine kvoodi tellimuse kohta** konfigureerimiseks Paigutage kursor äsja lisatud **piirangu** elemendi sees **sissetulevate** elemendi kohe alla ja klõpsake **määratud kasutuse kvoodi tellimuse kohta**vasakul asuvat noolt.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    <api name="name" calls="number" bandwidth="kilobytes">
    <operation name="name" calls="number" bandwidth="kilobytes" />
    </api>
    </quota>

Kuna see poliitika on ka mõeldud toote tasemel, kustutada elementide **api** ja **toimingu** nimi, nagu on näidatud järgmises näites.

    <quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
    </quota>

Kvootide saate alusel arvu intervall, läbilaskevõime või mõlemad. Selles õpetuses meil on pidurdamise põhineb läbilaskevõime, seega Kustuta **läbilaskevõime** atribuut.

    <quota calls="number" renewal-period="seconds">
    </quota>

Tasuta prooviversioon on kvoodi 200 kõned nädalas. Määrata **200** **kõned** atribuudi väärtus ja seejärel määrake **604800** **- ajavahemiku** atribuudi väärtus.

    <quota calls="200" renewal-period="604800">
    </quota>

>Poliitika intervalle on määratud sekundite. Arvutamiseks intervalli nädal, saate tundi (24) päeva, üks tund (60), sekundites (60) minutiga minutite arv arvuga korrutada (7) päevade arv: 7 *24* 60 * 60 = 604800.

Kui olete lõpetanud konfigureerimine poliitika, seda peaksid olema samad järgmises näites.

    <policies>
        <inbound>
            <rate-limit calls="10" renewal-period="60">
            </rate-limit>
            <quota calls="200" renewal-period="604800">
            </quota>
            <base />

    </inbound>
    <outbound>

        <base />

        </outbound>
    </policies>

Pärast soovitud poliitika on konfigureeritud, klõpsake nuppu **Salvesta**.

![Poliitika salvestamine][api-management-policy-save]

## <a name="publish-product"></a> Avaldada toote

Nüüd, kui on selle API-d on lisatud ja poliitika on konfigureeritud, toote peab olema avaldatud nii, et seda saab kasutada arendajatele. Klõpsake vasakul menüüst **API halduse** **tooted** ja klõpsake **Tasuta prooviversiooni** toote konfigureerimiseks.

![Toote konfigureerimine][api-management-configure-product]

Klõpsake nuppu **Avalda**ja seejärel klõpsake nuppu **Jah, avaldada** kinnitamiseks.

![Toote avaldamine][api-management-publish-product]

## <a name="subscribe-account"> </a>Tellimise arendaja konto tootega

Nüüd, kui toodet on avaldatud, on tellinud ja kasutada arendajatele saadaval.

>Administraatorid API halduse eksemplari on tellinud automaatselt iga toote. Selles etapis tuleb kuueosalisest me Telli ühe konto-administraatori arendaja tasuta prooviversiooni tootega. Kui teie konto arendaja on administraatorid rolli, siis võite järgida koos selles etapis tuleb isegi juhul, kui teil on juba märgitud.

Klõpsake vasakul menüüs **API haldus** **Kasutajad** ja klõpsake oma arendaja konto nimi. Selles näites me kasutame **Clayton Gragg** arendaja konto.

![Arendaja konfigureerimine][api-management-configure-developer]

Klõpsake **Lisa tellimus**.

![Tellimuse lisamine][api-management-add-subscription-menu]

Valige **Tasuta prooviversioon**ja seejärel klõpsake nuppu **Telli**.

![Tellimuse lisamine][api-management-add-subscription]

>[AZURE.NOTE] Selles õpetuses pole lubatud tasuta prooviversioon samaaegselt mitu tellimust. Kui need olid, oleks teilt nime tellimus, nagu on näidatud järgmises näites.

![Tellimuse lisamine][api-management-add-subscription-multiple]

Pärast nupu **Telli**, kuvatakse toote **tellimuse** loendis kasutaja jaoks.

![Tellimuse lisatud][api-management-subscription-added]

## <a name="test-rate-limit"> </a>Kõne toimingu ja testimine määr limiit

Nüüd, kui tasuta prooviversioon on konfigureeritud ja avaldatud, saame kõne mõned toimingud ja testida määr limiit poliitika.
Aktiveerige arendaja portaali, klõpsates paremas menüü **arendaja portaalis** .

![Portaali arendaja][api-management-developer-portal-menu]

Klõpsake ülemises menüüs **API-d** ja klõpsake **Kaja API**.

![Portaali arendaja][api-management-developer-portal-api-menu]

Klõpsake nuppu **saada ressursside**ja klõpsake **seda proovida**.

![Avatud konsooli][api-management-open-console]

Säilita vaikimisi parameetrite väärtused ja seejärel valige oma tellimuse tootenumber tasuta prooviversioon.

![Märkimise][api-management-select-key]

>[AZURE.NOTE] Kui teil on mitu tellimust, veenduge, et klahv **Tasuta**prooviversiooni, muidu eelmistes toimingutes konfigureeritud poliitikate ei saa tegelikult.

Klõpsake nuppu **saada**ja seejärel vaadata vastus. Pange tähele **vastuse olek** **200 OK**.

![Toimingu tulemused][api-management-http-get-results]

Klõpsake nuppu **saada** määr, mis on suurem kui 10 kõnede minutis määr limiit poliitika. Pärast määr limiit poliitika on suurem, tagastatakse **429 liiga palju taotlusi** vastuse olek.

![Toimingu tulemused][api-management-http-get-429]

**Vastuse sisu** näitab ülejäänud intervall enne korduskatsed eduka.

Kui määr limiit poliitika 10 kõnede minutis rakendub, ajakava nurjub kuni 60 sekundi möödumist esimesest 10 eduka kõnede tootega enne määr piirang on ületatud. Selles näites on ülejäänud intervalli 54 sekundit.

## <a name="next-steps"> </a>Järgmised sammud

-   Vaadake demo järgmises videos piirmäärad ja kvootide seadmine.

> [AZURE.VIDEO rate-limits-and-quotas]


[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Oma esimese API Azure'i API Management haldamine]: api-management-get-started.md
[Kuidas luua ja kasutada rühmade Azure'i API haldus]: api-management-howto-create-groups.md
[View subscribers to a product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[API halduse teenuse eksemplari loomine]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Kõne määr limiit ja kvoodi poliitikate konfigureerimine]: #policies
[Add an API to the product]: #add-api
[Publish the product]: #publish-product
[Subscribe a developer account to the product]: #subscribe-account
[Call an operation and test the rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota
