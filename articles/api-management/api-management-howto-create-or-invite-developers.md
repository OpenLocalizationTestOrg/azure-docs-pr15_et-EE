<properties 
    pageTitle="Kuidas hallata kasutajakontosid Azure'i API Management | Microsoft Azure'i" 
    description="Saate teada, kuidas luua või kutsuda kasutajad Azure'i API haldus" 
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

# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>Kuidas hallata kasutajakontosid Azure'i API haldamine

API haldus on arendajatele API-d, mida saate seada API haldusega kasutajad. Sellest juhendist kuvatakse loomise ja kutsuda arendajad API-d ja toodete kasutamiseks, et saate teha oma API halduse eksemplariga. Kasutajakontode haldamine programmiliselt kohta leiate teavet dokumentatsioonist [Kasutaja olemi](https://msdn.microsoft.com/library/azure/dn776330.aspx) viite [REST API haldus](https://msdn.microsoft.com/library/azure/dn776326.aspx) .

## <a name="create-developer"> </a>Luua uus arendaja

Uus arendaja loomiseks klõpsake nuppu **Halda** API halduse teenust Azure klassikaline portaalis. See viib teid Publisheri API haldusportaali. Kui te pole veel loonud API halduse teenuse eksemplar, teemast [Azure API kasutamist alustada][] õpetuses [loomine API halduse teenuse eksemplar][] .

![Publisheri portaal][api-management-management-console]

Klõpsake linki **Kasutajad** vasakul menüüst **API haldus** ja seejärel klõpsake nuppu **Lisa kasutaja**.

![Arendaja loomine][api-management-create-developer]

Sisestage **e-posti**, **parool**ja **nimi** uue arendaja ja klõpsake nuppu **Salvesta**.

![Arendaja loomine][api-management-add-new-user]

Vaikimisi vastloodud arendaja kontod on **aktiivne**ja **arendajate** rühmaga seostatud.

![Uue arendaja][api-management-new-developer]

Arendaja on **aktiivne** olekus kontod saab kasutada kõiki API-d, mille jaoks on neil tellimused. Äsja loodud arendaja seostada muud rühmad, vaadake, [Kuidas rühmad arendajatele siduda][].

## <a name="invite-developer"> </a>Kutsuda arendaja

Kutsuda arendaja, klõpsake linki **Kasutajad** vasakul menüüst **API haldus** ja seejärel klõpsake nuppu **Kutsu kasutaja**.

![Kutsu arendaja][api-management-invite-developer]

Sisestage nimi ja meiliaadress arendaja ja klõpsake nuppu **Kutsu**.

![Kutsu arendaja][api-management-invite-developer-window]

Kinnitusteade kuvatakse, kuid hiljuti kutsutud arendaja ei kuvata loendis kuni pärast kutse aktsepteerimist. 

![Kutsu kinnituse][api-management-invite-developer-confirmation]

Kui arendaja on kutsutud, saadetakse e-posti arendaja. See e-posti malli abil luuakse ja on kohandatav. Lisateabe saamiseks vaadake [konfigureerimine e-posti Mallid][].

Kui kutse on aktsepteeritud, aktiveerub konto.

## <a name="block-developer"></a> Desaktiveeri või Aktiveeri arendaja konto

Vaikimisi vastloodud või kutsutud arendaja kontod on **aktiivne**. Arendaja konto väljalülitamiseks klõpsake nuppu **Blokeeri**. Arendaja blokeeritud konto uuesti aktiveerimiseks klõpsake nuppu **Aktiveeri**. Blokeeritud arendaja kontot ei saa portaali arendaja või mis tahes API-d. Kasutajakonto kustutamiseks klõpsake nuppu **Kustuta**.

![Blokeeri arendaja][api-management-new-developer]

## <a name="reset-a-user-password"></a>Kasutaja parooli lähtestamine

Kasutajakonto parooli lähtestamiseks klõpsake konto nimi.

![Parooli lähtestamine][api-management-view-developer]

Klõpsake **parooli lähtestada** kasutaja parooli lingi saatmiseks.

![Parooli lähtestamine][api-management-reset-password]

Programmiliselt töötada Kasutajakontod, dokumentatsioonist [Kasutaja olemi](https://msdn.microsoft.com/library/azure/dn776330.aspx) viite [REST API haldus](https://msdn.microsoft.com/library/azure/dn776326.aspx) . Kindla väärtuse kasutajakonto parooli lähtestamiseks saate kasutada [kasutaja värskendada](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser) toiming ja määrake soovitud parool.

## <a name="pending-verification"></a>Kinnitamise ootel

![Kinnitamise ootel][api-management-pending-verification]

## <a name="next-steps"> </a>Järgmised sammud

Kui arendaja konto on loodud, saate seostada rollid ja tooted ja API-de tellimine. Lisateabe saamiseks vaadake, [Kuidas luua ja kasutada rühmad][].


[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png
[]: ./media/api-management-howto-create-or-invite-developers/.png



[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[Kuidas luua ja kasutada rühmad]: api-management-howto-create-groups.md
[Rühmade seostamist arendajad]: api-management-howto-create-groups.md#associate-group-developer

[Azure'i API kasutamist alustada]: api-management-get-started.md
[API halduse teenuse eksemplari loomine]: api-management-get-started.md#create-service-instance
[E-posti Mallid konfigureerimine]: api-management-howto-configure-notifications.md#email-templates