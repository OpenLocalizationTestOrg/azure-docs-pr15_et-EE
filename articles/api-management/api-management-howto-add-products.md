<properties 
    pageTitle="Kuidas luua ja avaldada toote Azure'i API haldus" 
    description="Saate teada, kuidas luua ja avaldada toodete Azure'i API haldamine." 
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

# <a name="how-to-create-and-publish-a-product-in-azure-api-management"></a>Kuidas luua ja avaldada toote Azure'i API haldus

Azure'i API haldamine, sisaldab toote ühe või mitme API-de kui ka kasutuse kvoodi ja kasutustingimustega. Kui toode on avaldatud, arendajate toote tellimine ja hakata kasutama toote API-d. Teema sisaldab juhend luua toode, lisades API ja avaldades selle arendajatele.

## <a name="create-product"> </a>Toote loomine

Toimingud on lisatud ja konfigureeritud API Publisheri portaalis. Publisheri portaali, klõpsake nuppu **Halda** API halduse teenust Azure klassikaline portaalis.

![Publisheri portaal][api-management-management-console]

>Kui te pole veel loonud API halduse teenuse eksemplar, teemast [Azure API kasutamist alustada][] õpetuses [loomine API halduse teenuse eksemplar][] .

Klõpsake **toodete** **toodete** lehe kuvamiseks vasakpoolses menüüs ja klõpsake nuppu **Lisa toode**.

![Tooted][api-management-products]

![Uus toode][api-management-add-new-product]

Sisestage välja **nimi** ja kirjeldus väljale **Kirjeldus** toote toote kirjeldav nimi.

Toodete API Management võib olla **avatud** või **kaitstud**. Kaitstud tooted on tellinud enne neid kasutada, samal ajal Ava toodete saab kasutada ilma tellimus. Märkige ruut **nõua tellimuse** kaitstud toode, mis nõuab tellimuse loomiseks. See on vaikesäte.

Kui soovite läbi vaadata ja aktsepteerimiseks või hülgamiseks tellimuse püüab selle toote administraator, märkige ruut **tellimuse kinnitamise nõudmine** . Kui ruut on märkimata, saab tellimuse katsete automaatse kinnitatud. Tellimuste kohta leiate lisateavet teemast [vaate tellijad toote][].

Kui soovite lubada mitu korda tellida toote arendaja kontod, märkige ruut **Luba mitu tellimust** . Kui see ruut on märgitud, saate iga arendaja konto tellida ainult ühe kellaaja tootega.

![Piiramatu mitu tellimust][api-management-unlimited-multiple-subscriptions]

Mitme samaaegse tellimuste arv piirata, **limiit samaaegne tellimuste arv** , märkige ruut ja sisestage selle tellimuse. Järgmises näites on samaaegne tellimuste neli arendaja konto kohta.

![Nelja mitu tellimust][api-management-four-multiple-subscriptions]

Pärast kõigi uute toote valikud on konfigureeritud, klõpsake nuppu **Salvesta** uue toote loomiseks.

![Tooted][api-management-products-page]

>Vaikimisi uute toodete on avaldamata ja ainult nähtavaid **administraatorite** rühma.

Toote konfigureerimiseks klõpsake toote nimi menüüs **tooted** .

## <a name="add-apis"> </a>Toote API lisamine

**Toodete** leht sisaldab nelja linkide konfigureerimine: **Kokkuvõte**, **sätted**, **nähtavuse**ja **tellijad**. Vahekaart **Kokkuvõte** on, kus saate lisada API-d ja avaldamine või avaldamise toote.

![Kokkuvõte][api-management-new-product-summary]

Enne avaldamise toote peate lisama ühe või mitme API-d. Selleks klõpsake **Toote API lisada**.

![API-de lisamine][api-management-add-apis-to-product]

Valige soovitud API-d ja klõpsake nuppu **Salvesta**.

## <a name="add-description"> </a>Toote kirjeldava teabe lisamine

Vahekaart **sätted** saate selle eesmärk, API-de pakub juurdepääsu ja muud kasulikku teavet toote kohta üksikasjaliku teabe. Sisu on suunatud arendajate, mis nõuavad API ja saate kirjutada lihttekst või HTML-i märgistus.

![Toote sätted][api-management-product-settings]

Märkige ruut **nõua tellimuse** loomiseks kaitstud toode, mis nõuab tellimust kasutada, või tühjendage ruut Ava toote, saab helistada ilma tellimuse loomiseks.

Valige **tellimuse kinnitamise nõudmine.** kui soovite kõik toote tellimuse taotlusi käsitsi kinnitamiseks. Vaikimisi kõigi toote tellimused antakse automaatselt.

Arendaja kontod mitu korda tellida toote lubamiseks märkige ruut **Luba mitu tellimust** ja soovi korral määrake on piiratud. Kui see ruut on märgitud, saate iga arendaja konto tellida ainult ühe kellaaja tootega.

Soovi korral sisestage **kasutustingimused** välja, mis kirjeldab tellijad, mille peate vastu võtma, et toodet kasutada toote kasutustingimused.

## <a name="publish-product"> </a>Toote avaldamine

Enne selle toote API-d saab kutsuda, peab olema avaldatud toode. Toote vahekaardil **Kokkuvõte** nuppu **Avalda**ja seejärel klõpsake nuppu **Jah, avaldada** kinnitamiseks. Varem avaldatud toote privaatseks nuppu **Tühista avaldamine**.

![Toote avaldamine][api-management-publish-product]

## <a name="make-visible"> </a>Toote nähtavaks arendajatele

**Nähtavuse** menüü võimaldab teil valida, millised rollid on võimalik vaadata toote arendaja portaalis ja toote tellimine.

![Toote nähtavus][api-management-product-visiblity]

Lubada või keelata nähtavuse toote arendajatele rühma, märkige või tühjendage rühma kõrval olev ruut ja klõpsake siis nuppu **Salvesta**.

>Lisateavet leiate teemast [loomine ja kasutamine rühmade haldamiseks arendaja kontod Azure'i API haldus][].

## <a name="view-subscribers"> </a>Tellijad toote kuvamine

Vahekaart **tellijad** on loetletud arendajad, kes on liitunud toote. Üksikasjad ja sätted iga arendaja saab vaadata, klõpsates arendaja nime. Selles näites arendajad pole veel tellinud toote.

![Arendajad][api-management-developer-list]

## <a name="next-steps"> </a>Järgmised sammud

Kui soovitud API-de lisatakse ja toote avaldatud, saate arendajate toote tellimine ja alustage soovitud API-d. Õpetuse, mis näitab nende üksuste kui ka täpsemalt toote konfiguratsiooni näha, [Kuidas luua ja Azure API Management täpsemalt toote sätete konfigureerimine][].

Toodete töötamise kohta leiate lisateavet teemast järgmisest videost.

> [AZURE.VIDEO using-products]

[Create a product]: #create-product
[Add APIs to a product]: #add-apis
[Add descriptive information to a product]: #add-description
[Publish a product]: #publish-product
[Make a product visible to developers]: #make-visible
[Vaate tellijad toote]: #view-subscribers
[Next steps]: #next-steps

[api-management-management-console]: ./media/api-management-howto-add-products/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-add-products/api-management-add-product.png
[api-management-add-new-product]: ./media/api-management-howto-add-products/api-management-add-new-product.png
[api-management-unlimited-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-unlimited-multiple-subscriptions.png
[api-management-four-multiple-subscriptions]: ./media/api-management-howto-add-products/api-management-four-multiple-subscriptions.png
[api-management-products-page]: ./media/api-management-howto-add-products/api-management-products-page.png
[api-management-new-product-summary]: ./media/api-management-howto-add-products/api-management-new-product-summary.png
[api-management-add-apis-to-product]: ./media/api-management-howto-add-products/api-management-add-apis-to-product.png
[api-management-product-settings]: ./media/api-management-howto-add-products/api-management-product-settings.png
[api-management-publish-product]: ./media/api-management-howto-add-products/api-management-publish-product.png
[api-management-product-visiblity]: ./media/api-management-howto-add-products/api-management-product-visibility.png
[api-management-developer-list]: ./media/api-management-howto-add-products/api-management-developer-list.png



[api-management-products]: ./media/api-management-howto-add-products/api-management-products.png
[api-management-]: ./media/api-management-howto-add-products/
[api-management-]: ./media/api-management-howto-add-products/


[How to add operations to an API]: api-management-howto-add-operations.md
[How to create and publish a product]: api-management-howto-add-products.md
[Azure'i API kasutamist alustada]: api-management-get-started.md
[API halduse teenuse eksemplari loomine]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps
[Kuidas luua ja rühmade abil saate hallata arendaja kontod Azure'i API haldus]: api-management-howto-create-groups.md
[Kuidas luua ja Azure API Management täpsemalt toote sätete konfigureerimine]: api-management-howto-product-with-rules.md 