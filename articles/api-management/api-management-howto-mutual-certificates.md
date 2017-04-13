<properties 
    pageTitle="Kuidas tagada tagaandmebaas services kliendi serdi autentimist Azure'i API haldamine" 
    description="Saate teada, kuidas turvaline tagaandmebaas teenuste kasutamine Azure API Management kliendi serdi autentimist." 
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

# <a name="how-to-secure-back-end-services-using-client-certificate-authentication-in-azure-api-management"></a>Kuidas tagada tagaandmebaas services kliendi serdi autentimist Azure'i API haldamine

API haldus võimaldab juurdepääsu API tagaandmebaas teenuse secure kliendi sertide abil. Sellest juhendist kuvatakse haldamine API Publisheri portaalis serdid ja API serdi abil oma tagaandmebaas teenuse konfigureerimine.

API haldamine REST API kasutamine haldamise kohta leiate teavet teemast [Azure API halduse REST API serdi üksus][].

## <a name="prerequisites"> </a>Eeltingimused

Sellest juhendist näitab, kuidas konfigureerida kasutama kliendi serdi autentimist tagaandmebaas teenus API juurdepääsu oma API halduse eksemplari. Enne selle teema juhiste, peaksite on konfigureeritud lubama kliendi serdi autentimist ([konfigureerimiseks serdi autentimist Azure veebisaitide vaadake käesoleva artikli][]) tagaandmebaas teenust ja serti ja parooli serdi üleslaadimine API haldusportaal Publisheri juurde.

## <a name="step1"> </a>Kliendi serdi üleslaadimine

Alustamiseks klõpsake nuppu **Halda** API halduse teenust Azure klassikaline portaalis. See viib teid Publisheri API haldusportaali.

![Publisheri API portaal][api-management-management-console]

>Kui te pole veel loonud API halduse teenuse eksemplar, teemast [Azure API kasutamist alustada][] õpetuses [loomine API halduse teenuse eksemplar][] .

Klõpsake vasakul menüüst **API halduse** **Turvalisus** ja nuppu **Kliendi serdid**.

![Kliendi sertide][api-management-security-client-certificates]

Soovite uut serti, klõpsake nuppu **laadige üles sert**.

![Laadige üles sert][api-management-upload-certificate]

Otsige sirvides üles oma sert ja seejärel sisestage parool serti.

>Peab sert olema **pfx** -vormingus. Iseallkirjastatud serdid on lubatud.

![Laadige üles sert][api-management-upload-certificate-form]

Klõpsake **üles** laadige üles sert.

>Serdi parool on kinnitatud sel ajal. Kui see on vale, kuvatakse tõrketeade.

![Üles laaditud serdi][api-management-certificate-uploaded]

Kui sert on üles laaditud, kuvatakse see menüü **Kliendi serdid** . Kui teil on mitu sertifikaati, veenduge, et teema üles või viimati neli märki sõrmejälje, mida kasutatakse konfigureerimisel API kasutamine sertifikaatide kohta, nagu järgmises [API kasutada kliendi serti lüüsi autentimise konfigureerimine][] jaotise alla serdi valimiseks.

>Väljalülitamiseks serdi ahelas valideerimine kasutamisel, näiteks iseallkirjastatud serdi järgige selles FAQ [üksus](api-management-faq.md#can-i-use-a-self-signed-ssl-certificate-for-a-back-end).

## <a name="step1a"> </a>Kliendi serdi kustutamine

Serdi kustutamiseks klõpsake soovitud serti kõrval nuppu **Kustuta** .

![Serdi kustutamine][api-management-certificate-delete]

Klõpsake nuppu **Jah, kustutage see** kinnitamiseks.

![Kustutamise kinnitamine][api-management-confirm-delete]

Kui sert on kasutusel API, kuvatakse hoiatus ekraan. Serdi kustutamiseks peavad serdi eemaldamine esmalt API-d, mis on konfigureeritud seda kasutada.

![Kustutamise kinnitamine][api-management-confirm-delete-policy]

## <a name="step2"> </a>API kliendi serti kasutamiseks lüüsi autentimise konfigureerimine

Klõpsake vasakul menüüst **API haldus** **API-d** , klõpsake soovitud API nime ja klõpsake vahekaarti **Turvalisus** .

![API turvalisus][api-management-api-security]

Valige **kliendi sertide** **mandaadiga** rippmenüü loendis.

![Kliendi serdid][api-management-mutual-certificates]

Valige loendist **Kliendi sert** ripploendis soovitud serti. Kui määratud on mitu serdid saate vaadata teema või viimati neli märki sõrmejälje, nagu eelmises jaotises määratlemiseks õige sert.

![Valige sert][api-management-select-certificate]

Klõpsake nuppu **Salvesta** API konfiguratsiooni muudatuse salvestamiseks.

>See muudatus on tõhus kohe ja toimingud, et API kõned kasutatakse serdi autentida tagaandmebaas serveris.

![API muudatuste salvestamine][api-management-save-api]

>Kui sert on määratud API teenuse tagaandmebaas lüüsi autentimiseks, osaks poliitika selle API ja poliitika redaktoris saab vaadata.

![Serdi poliitika][api-management-certificate-policy]

## <a name="next-steps"></a>Järgmised sammud

Lisateavet muul viisil kindlustada kirjutamata teenust, nt HTTP tavaline või ühiskasutatava salajane autentimine, leiate järgmisest videost.

> [AZURE.VIDEO last-mile-security]

[api-management-management-console]: ./media/api-management-howto-mutual-certificates/api-management-management-console.png
[api-management-security-client-certificates]: ./media/api-management-howto-mutual-certificates/api-management-security-client-certificates.png
[api-management-upload-certificate]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate.png
[api-management-upload-certificate-form]: ./media/api-management-howto-mutual-certificates/api-management-upload-certificate-form.png
[api-management-certificate-uploaded]: ./media/api-management-howto-mutual-certificates/api-management-certificate-uploaded.png
[api-management-api-security]: ./media/api-management-howto-mutual-certificates/api-management-api-security.png
[api-management-mutual-certificates]: ./media/api-management-howto-mutual-certificates/api-management-mutual-certificates.png
[api-management-select-certificate]: ./media/api-management-howto-mutual-certificates/api-management-select-certificate.png
[api-management-save-api]: ./media/api-management-howto-mutual-certificates/api-management-save-api.png
[api-management-certificate-policy]: ./media/api-management-howto-mutual-certificates/api-management-certificate-policy.png
[api-management-certificate-delete]: ./media/api-management-howto-mutual-certificates/api-management-certificate-delete.png
[api-management-confirm-delete]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete.png
[api-management-confirm-delete-policy]: ./media/api-management-howto-mutual-certificates/api-management-confirm-delete-policy.png



[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Azure'i API kasutamist alustada]: api-management-get-started.md
[API Management policy reference]: api-management-policy-reference.md
[Caching policies]: api-management-policy-reference.md#caching-policies
[API halduse teenuse eksemplari loomine]: api-management-get-started.md#create-service-instance

[Azure'i API halduse REST API serdi üksus]: http://msdn.microsoft.com/library/azure/dn783483.aspx
[WebApp-GraphAPI-DotNet]: https://github.com/AzureADSamples/WebApp-GraphAPI-DotNet
[konfigureerida serdi autentimist Azure veebisaitide vaadake käesoleva artikli]: https://azure.microsoft.com/en-us/documentation/articles/app-service-web-configure-tls-mutual-auth/

[Prerequisites]: #prerequisites
[Upload a client certificate]: #step1
[Delete a client certificate]: #step1a
[API kliendi serti kasutamiseks lüüsi autentimise konfigureerimine]: #step2
[Test the configuration by calling an operation in the Developer Portal]: #step3
[Next steps]: #next-steps


 
