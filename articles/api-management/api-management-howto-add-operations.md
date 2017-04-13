<properties 
    pageTitle="Toimingute lisamine Azure'i API Management API | Microsoft Azure'i" 
    description="Saate teada, kuidas Azure'i API Management API toimingute lisamiseks." 
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

# <a name="how-to-add-operations-to-an-api-in-azure-api-management"></a>Kuidas lisada toimingute API Azure'i API haldamine

Enne API Management API saab kasutada, tuleb lisada toiminguid. Sellest juhendist näitab, kuidas lisada ja konfigureerida erinevat tüüpi toiminguid API API haldamine.

## <a name="add-operation"> </a>Toimingu lisamine

Toimingud on lisatud ja konfigureeritud API Publisheri portaalis. Publisheri portaali, klõpsake nuppu **Halda** API halduse teenust Azure klassikaline portaalis.

![Publisheri portaal][api-management-management-console]

>Kui te pole veel loonud API halduse teenuse eksemplar, teemast [Azure API kasutamist alustada][] õpetuses [loomine API halduse teenuse eksemplar][] .

Valige soovitud API Publisheri portaalis ja seejärel valige vahekaart **toimingud** . 

![Toimingud][api-management-operations]

Klõpsake nuppu **Lisa toiming** lisada uue toiming. Kuvatakse **Uus toiming** ja vaikimisi valitakse menüü **allkiri** .

![Toimingu lisamine][api-management-add-operation]

Määrake **HTTP-Verbi** , valides rippmenüü loendist.

![HTTP-meetod][api-management-http-method]

<a name="url-template"></a>

Malli URL-i, tippides URL-i fragment koosneb ühe või mitme URL-i tee segmente ja null või rohkem päringustringi parameetrite määratlemine URL-i Mall, lisatud base URL-i API, tuvastab HTTP ühekordne. See võib sisaldada ühte või rohkem nimega muutuv osa, mis on tähistatud looksulud. Nende muutuvate osade nimetatakse malli parameetrid ning on määratud väärtused ekstraktimist soovitud taotlus URL-i kui taotluse töötlemise platvormi API haldus.

![URL-i Mall][api-management-url-template]

<a name="rewrite-url-template"></a>

Soovi korral määrake **malli URL-i ümberkirjutamine**. See võimaldab teil standardne URL-i malli kasutamiseks klõpsake ees, sissetulevad taotlused töötlemiseks ajal tagaandmebaas teisendatud URL-i ümberkirjutamine malli vastavalt kaudu helistamine. Malli ümberkirjutamine tuleks kasutada malli parameetreid malli URL-i. Järgmises näites kirjeldatakse, kuidas sisutüübi kodeeritud nagu tee lõigu veebiteenuse eelmise näite põhjal saab esitada päringu parameetrina API avaldatud URL-i mallide kasutamine platvormi API halduse kaudu.

![URL-i malli ümberkirjutamine][api-management-url-template-rewrite]

Helistajad tööks kasutatakse vormingu `/customers?customerid=ALFKI` ja see vastendatakse `/Customers('ALFKI')` kui tagaandmebaas teenus on vaja järgida.


**Kuvatav** nimi ja **Kirjeldus** toimingu kirjelduse ja dokumentatsiooni anda seda API kasutamine arendaja portaali arendajad kasutatakse.

![Kirjeldus][api-management-description]

Toimingu kirjeldus saab määrata lihttekstina või HTML-vormingus teksti väljale **Kirjeldus** .

## <a name="operation-caching"> </a>Toiming vahemällu talletamine

Vastuse vahemällu vähendab tajuvad API tarbijad, vähendab läbilaskevõime kulu ja väheneb laadi HTTP veebis teenuse rakendamise API latentsus. 

Kiiresti ja hõlpsalt lubamiseks vahemällu toimimiseks, valige vahekaart **vahemälu** ja märkige ruut **Luba** .

![Vahemällu talletamine][api-management-caching-tab]

**Kestus** määrab aja jooksul vastuse toiming jääb vahemälu. Vaikeväärtus on 3600 sekundit või 1 tund.

Vahemälu võtit eristada vastuste nii, et vastav erinevate vahemälu klahvi vastus saavad oma eraldi vahemällu talletatud väärtus. Soovi korral sisestage teatud päringustringi parameetrite ja/või HTTP-päiste tuleb kasutada arvuti vahemälu väärtused väljadele **Varieeri päringustringi parameetrite** ja **päised Varieeri** vastavalt. Kui ükski pole määratud, täielik taotluse URL-i ja vahemälu võtme genereerimine kasutatakse HTTP päise järgmised väärtused: **Aktsepteeri** ja **Aktsepteeri-märgistik**.

>Vahemällu talletamine ja poliitikate vahemällu talletamise kohta leiate lisateavet teemast [Azure API Management vahemälu toimingu tulemused][].


## <a name="request-parameters"> </a>Parameetrite taotlemine

Vahekaardil parameetrid hallatakse toiming parameetrid. Määratud **URL-i malli** **Signatuur** vahekaardi Parameetrid lisatakse automaatselt ja saab muuta ainult URL-i malli redigeerimine. Täiendavad parameetrid saab käsitsi sisestada.

Uue Päringuparameetri lisamise, klõpsake nuppu **Lisa Päringuparameetri** ja sisestage järgmine teave:

-   **Nimi** – parameetri nimi.
-   **Kirjeldus** – Lühikirjeldus parameetri (valikuline).
-   **Tippige** - parameetri tüüp valitud rippmenüüst alla.
-   **Väärtused** - väärtusi, mida saab määrata see parameeter. Üks väärtus saab märkida vaikimisi (valikuline).
-   **Nõutav** - parameetri kohustuslikuks, märkides ruudu olev ruut. 

![Parameetrite taotlemine][api-management-request-parameters]

## <a name="request-body"> </a>Keha taotlemine

Kui toiming võimaldab (nt panna, postituse) ja nõuab asutuse esitate näide selle kõikide toetatud esituse vormingutes (nt json, XML). 

>Koosolekukutse sisusse kasutatakse dokumentatsiooni eesmärgil ja on kinnitatud.

Koosolekukutse kehasse sisestamiseks **keha** vahekaardi aktiveerimine.

Klõpsake nuppu **Lisa esituse**start, tippige soovitud sisutüübi nimi (nt rakenduse/json), klõpsake rippnoolt ja kleepige soovitud taotlus kehateksti näide tekstivälja valitud vormingus. 

![Koosolekukutse kehasse.][api-management-request-body]

Täiendavad esinduste, saate ka määrata on valikuline tekst kirjeldus väljale **Kirjeldus** teksti.

## <a name="responses"> </a>Vastused

See on hea tava näiteid vastuste kõik oleku koodid, mis võib põhjustada selle toimingu jaoks. Iga oleku koodi võib-olla rohkem kui üks vastus kehateksti näide, üks iga toetatud sisutüübid. 

Vastuse lisamiseks klõpsake nuppu **Lisa** ja hakake tippima soovitud olekukoodi. Selles näites on tähis on **200 OK**. Kui kood on kuvatud drop-down, valige see ja vastuse koodi on loodud ja lisatakse teie toiming.

![Vastuse koodi][api-management-response-code]

Klõpsake nuppu **Lisa esituse**, hakake tippima soovitud sisutüübi nime (nt rakenduse/json) ja seejärel valige rippmenüüst alla.

![Kehateksti sisutüüp][api-management-response-body-content-type]

Kleepige tekstiväljale vastus kehateksti näide valitud vormingus. 

![Vastuse sisu][api-management-response-body]

Soovi korral lisage soovi korral kirjeldus väljale **Kirjeldus** teksti.

Kui toiming on konfigureeritud, klõpsake nuppu **Salvesta**.


## <a name="next-steps"> </a>Järgmised sammud

Kui need toimingud on lisatud API, järgmise sammuna tuleb API seostada toote ja selle avaldada, et arendajad saate helistada oma tegevust.

-   [Kuidas luua ja avaldada toote][]

[api-management-management-console]: ./media/api-management-howto-add-operations/api-management-management-console.png
[api-management-operations]: ./media/api-management-howto-add-operations/api-management-operations.png
[api-management-add-operation]: ./media/api-management-howto-add-operations/api-management-add-operation.png
[api-management-http-method]: ./media/api-management-howto-add-operations/api-management-http-method.png
[api-management-url-template]: ./media/api-management-howto-add-operations/api-management-url-template.png
[api-management-url-template-rewrite]: ./media/api-management-howto-add-operations/api-management-url-template-rewrite.png
[api-management-description]: ./media/api-management-howto-add-operations/api-management-description.png
[api-management-caching-tab]: ./media/api-management-howto-add-operations/api-management-caching-tab.png
[api-management-request-parameters]: ./media/api-management-howto-add-operations/api-management-request-parameters.png
[api-management-request-body]: ./media/api-management-howto-add-operations/api-management-request-body.png
[api-management-response-code]: ./media/api-management-howto-add-operations/api-management-response-code.png
[api-management-response-body-content-type]: ./media/api-management-howto-add-operations/api-management-response-body-content-type.png
[api-management-response-body]: ./media/api-management-howto-add-operations/api-management-response-body.png


[api-management-contoso-api]: ./media/api-management-howto-add-operations/api-management-contoso-api.png

[api-management-add-new-api]: ./media/api-management-howto-add-operations/api-management-add-new-api.png
[api-management-api-settings]: ./media/api-management-howto-add-operations/api-management-api-settings.png
[api-management-api-settings-credentials]: ./media/api-management-howto-add-operations/api-management-api-settings-credentials.png
[api-management-api-summary]: ./media/api-management-howto-add-operations/api-management-api-summary.png
[api-management-echo-operations]: ./media/api-management-howto-add-operations/api-management-echo-operations.png

[Add an operation]: #add-operation
[Operation caching]: #operation-caching
[Request parameters]: #request-parameters
[Request body]: #request-body
[Responses]: #responses
[Next steps]: #next-steps

[Azure'i API kasutamist alustada]: api-management-get-started.md
[API halduse teenuse eksemplari loomine]: api-management-get-started.md#create-service-instance

[How to add operations to an API]: api-management-howto-add-operations.md
[Kuidas luua ja avaldada toote]: api-management-howto-add-products.md
[Kuidas vahemälu toimingu tulemused Azure'i API haldus]: api-management-howto-cache.md