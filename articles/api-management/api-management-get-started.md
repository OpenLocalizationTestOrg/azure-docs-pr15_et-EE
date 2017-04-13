<properties
    pageTitle="Hallata oma esimese API Azure'i API Management | Microsoft Azure'i"
    description="Saate teada, kuidas luua API-d, lisamistoimingute ja API kasutamist alustada."
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

# <a name="manage-your-first-api-in-azure-api-management"></a>Oma esimese API Azure'i API Management haldamine

## <a name="overview"> </a>Ülevaade

Sellest juhendist näitab, kuidas kiiresti alustada Azure'i API haldusega ja oma esimese API helistada.

## <a name="concepts"> </a>Mis on Azure API haldus?

Azure'i API halduse abil saate võtta mis tahes taustväärtus ja selle põhjal täieõiguslik API programmi käivitada.

Levinumad stsenaariumid on järgmised.

* **Tagada mobiilse taristu** väravamutatsiooniga Accessi API abil, vältimine DOS suunatud ahendamine või abil täpsemate turbepoliitikate nagu JWT Turbeloa valideerimise abil.
* **Lubada ISV partneri ökosüsteemide** pakkudes kiire partneri kasutuselevõtt arendaja portaali kaudu ja koostamise soovitud API fassaadi lahutada sisemise rakendusi, mis ei ole küps tarbimine partneri kaudu.
* **Töötab API programmi sisemine** pakkudes keskse asukoha organisatsiooni suhelda kättesaadavus ja API-d, väravamutatsiooniga Accessi vastavalt ettevõtte kontod, et viimaste muudatuste kohta kõik põhineb turvalise kanali API lüüsi ja selle taustväärtus vahel.


Süsteem koosneb järgmistest osadest:

* **API lüüsi** on lõpp-punkti mis:
  * Aktsepteerib API helistab ja neid oma taustaprogrammid marsruudib.
  * Kontrollib, kas API klahvid, JWT sõned, serdid ja muud mandaat.
  * Jõustab piirmäärasid ja piirangud.
  * Muudab oma API pealt ilma muudatused.
  * Vahemälu kirjutamata vastuseid, kus häälestamine.
  * Logide helistage metaandmete analytics eesmärgil.

* **Publisheri portaalis** on Administreerimine, kus saate oma API programmis häälestada. Kasutage seda.
    * Saate määratleda või API skeemi importimiseks.
    * Paketi API-de tooted.
    * Saate häälestada poliitikad, nt kvootide või teisendused on API-de kohta.
    * Saada ülevaadet Analytics.
    * Kasutajate haldamine.

* **Arendaja portaali** toimib peamise veebisaidi arendajatele, kus nad saavad:
    * Lugege API dokumentatsiooni.
    * Proovige API interaktiivne konsooli kaudu.
    * Looge konto ja tellimine saada API võtmed.
    * Accessi analytics oma kasutamise kohta.


## <a name="create-service-instance"> </a>API halduse eksemplari loomine

>[AZURE.NOTE] Selle õpetuse tegemiseks peate Azure'i konto. Kui teil pole kontot, saate luua tasuta konto vaid paar minutit. Lisateavet leiate teemast [Azure tasuta prooviversioon][].

Esimene samm töötamine API haldus on luua teenuse eksemplari. [Azure'i klassikaline portaali][] sisse logida ja klõpsake nuppu **Uus**, **Rakenduse teenused**, **API juhtimine**, **Loo**.

![Uue eksemplari API haldus][api-management-create-instance-menu]

Määratlege **URL-i**teenuse URL-i alamdomeeni kordumatu nimi.

Valige soovitud **tellimus** ja **regiooni** jaoks oma eksemplari. Pärast soovitud valikud, klõpsake nuppu **edasi** .

![Uue API halduse teenuse][api-management-create-instance-step1]

Sisestage **Contoso, Ltd** **Ettevõtte nimi**ja sisestage oma meiliaadress **Administraatori** e-post.

>[AZURE.NOTE] Selle e-posti aadressi kasutatakse API halduse süsteemist teatised. Lisateabe saamiseks vaadake, [Kuidas konfigureerida teatiste ja e-posti Mallid Azure'i API haldus][].

![Uue API halduse teenuse][api-management-create-instance-step2]

API halduse teenuse eksemplarid on saadaval kolmest tasemest: arendaja, Standard, ja. Vaikimisi luuakse uus API halduse teenuse eksemplari arendaja astme. Valige Standard või Premium taseme, märkige ruut **Täpsemad sätted** ja valige soovitud taseme järgmisel lehel.

>[AZURE.NOTE] Arendaja taseme on arengu, testimine ja katseprojekti API programmides, kus kõrge-saadavus ei ole muret. Standard- ja Premium astme, muudate oma reserveeritud ühiku arvu käsitlema rohkem liiklust. Standard- ja Premium astme pakuvad API halduse teenust kõige töötlemise võimsus ja jõudlus. Selles õppetükis saate täita, kasutades mõnda teise. Astme API haldamise kohta leiate lisateavet teemast [API halduse hinnad][].

Märkige ruut luua oma eksemplari.

![Uue API halduse teenuse][api-management-instance-created]

Kui eksemplari on loodud, on järgmiseks loomise või importimise API.

## <a name="create-api"> </a>API importimine

API koosneb toimingute kohta, mida saate kasutada kliendi rakendus. Olemasoleva web Services puhverdatud on API toimingud.

Saab luua API-de (ja toiminguid saab lisada) käsitsi või saab importida. Selles õpetuses Impordime valimi kalkulaator veebiteenuse Microsofti ja Azure majutatud API.

>[AZURE.NOTE] API loomine ja lisamine käsitsi toimingute juhised leiate teemast [API-de loomiseks](api-management-howto-create-apis.md) ja [Kuidas lisada toimingute API](api-management-howto-add-operations.md).

Azure'i klassikaline portaali kaudu avatavas portaalis publisher on konfigureeritud API-d. Publisheri portaali jõuda nuppu **Halda** API halduse teenust Azure klassikaline portaalis.

![Publisheri portaal][api-management-management-console]

Importimiseks kalkulaator API, klõpsake vasakul menüüst **API halduse** **API-de** ja klõpsake **Importimine API -ga**.

![API importimise nupp][api-management-import-api]

Järgmiste toimingute konfigureerimiseks kalkulaator API:

1. Klõpsake **Kaudu URL-i**, Sisestage tekstiväljale **Kirjeldus dokumendi URL-i** **http://calcapi.cloudapp.net/calcapi.json** ja klõpsake suvandinuppu **ärplema** .
2. Tippige tekstiväljale **Web API URL-i järelliite** **calc** .
3. Klõpsake väljal **tooted (valikuline)** ja valige **Starter**.
4. Klõpsake nuppu **Salvesta** importimine API.

![Lisage uus API][api-management-import-new-api]

>[AZURE.NOTE] **API halduse** toetab praegu ärplema dokumendi 1,2- ja 2.0 versiooni impordi. Veenduge, et, ehkki [ärplema 2.0 spetsifikatsioon](http://swagger.io/specification) kinnitab, et `host`, `basePath`, ja `schemes` atribuudid on valikuline, ärplema 2.0 dokumendi **peab** sisaldama nende atribuutide; muul juhul seda ei saada importida. 

Pärast importimist API API kokkuvõtte leht kuvatakse Publisheri portaalis.

![API Kokkuvõte][api-management-imported-api-summary]

Jaotise API on mitu vahekaarti. **Kokkuvõte** sakil lihtsa mõõdikute ja teavet API. Vahekaardi [sätted](api-management-howto-create-apis.md#configure-api-settings) saab vaadata ja redigeerida API konfigureerimine. Vahekaardil [toimingud](api-management-howto-add-operations.md) kasutatakse funktsiooni API toimingute tegemiseks. Lüüsi kirjutamata serveri autentimise konfigureerimine elementaarautentimine või [vastastikune serdi autentimist](api-management-howto-mutual-certificates.md)kasutades ja konfigureerimiseks [Kasutaja autoriseerimine OAuth 2.0 abil](api-management-howto-oauth2.md)saab kasutada vahekaarti **Turve** .  **Probleemid** menüü kasutatakse tarkvaraarendajad, kes kasutavad teie API-de leitud probleemide kuvamiseks. Vahekaart **tooted** kasutatakse konfigureerimiseks tooted, mis sisaldavad seda API-d.

Vaikimisi sisaldab iga API halduse eksemplari kahe valimi toodete.

-   **Starter**
-   **Piiramatu**

Selles õpetuses lisati tavaline kalkulaator API Starter toote API importimisel.

Selleks, et kõned API, arendajate tuleb esmalt tellida toode, mis annab neile juurdepääs sellele. Arendajad saate tellida toodete arendaja portaalis või administraatorid saate tellida arendajatel toodete Publisheri portaalis. Olete administraator, kuna loodud API halduse eksemplari eelmistes toimingutes õpetuse, nii, et teil on juba tellinud iga toote vaikimisi.

## <a name="call-operation"> </a>Kõne toimingu portaalist arendaja

Toimingute saab kutsuda otse arendaja portaali, mis pakub mugavam ülevaatus ja testimine API toimingud. Selles etapis tuleb kuueosalisest helistate tavaline kalkulaator API **Lisa kaks täisarvude** toiming. Klõpsake **portaali arendaja** menüü ülaosas paremal Publisheri portaali.

![Portaali arendaja][api-management-developer-portal-menu]

Klõpsake **API-de** ülalt menüüst klõpsake **Tavaline kalkulaator** kuvamiseks toimingud

![Portaali arendaja][api-management-developer-portal-calc-api]

Pange tähele, et valimi kirjeldused ja parameetrid koos API ja toiminguid, esitada dokumendid kasutavad seda toimingut arendajatele imporditud. Nende kirjeldused saab lisada ka siis, kui toimingud on käsitsi lisatud.

Klõpsake toimingu **Lisa kaks täisarvude** helistamiseks **proovima**.

![Proovige järele][api-management-developer-portal-calc-api-console]

Saate mõned väärtuste sisestamiseks parameetrite või säilitada vaikeväärtused ja klõpsake nuppu **saada**.

![HTTP Get][api-management-invoke-get]

Pärast toimingu rakendatakse, kuvab arendaja portaali **vastuse olek**, **vastus päised**ja **vastuse sisu**.

![Vastus][api-management-invoke-get-response]

## <a name="view-analytics"> </a>Kasutusanalüüsi kuvamine

Tavaline kalkulaator Kasutusanalüüsi kuvamiseks aktiveerige Publisheri portaali, valides menüü ülaosas **haldamine** paremal arendaja portaali.

![Haldamine][api-management-manage-menu]

Publisheri portaali vaikevaade on **armatuurlaud**, kus antakse ülevaade oma API halduse eksemplari.

![Armatuurlaua][api-management-dashboard]

Hõljutage kursorit üle diagrammi **Tavaline kalkulaator** kuvamiseks teatud mõõdikute API kasutamine antud aja jooksul.

>[AZURE.NOTE] Kui kõik read, ei kuvata diagrammil, aktiveerige arendaja portaali mõned helistada API sisse, oodake mõni minut ja siis naaske armatuurlaud.

Klõpsake nuppu **Kuva üksikasjad** kuvamiseks API, sh kuvatud mõõdikute suurema versiooni lehelt kokkuvõte.

![Kasutusanalüüsi][api-management-mouse-over]

![Kokkuvõte][api-management-api-summary-metrics]

Üksikasjalik mõõdikute ja aruanded, klõpsake **Analytics** **API haldus** käsku vasakule.

![Ülevaade][api-management-analytics-overview]

Jaotise **Analytics** on järgmised neli vahekaarti.

-   **Lühiülevaade** pakub üldine kasutus- ja seisundiandmete mõõdikute, kui ka ülemise arendajate, parimad tooted, ülemise API-de ja ülemise toimingud.
-   **Kasutus** pakub API kõned ja läbilaskevõime, sh geograafilist põhjalik ülevaade.
-   **Seisund** keskendub oleku koode, vahemälu edukust, vastuse kellaajad ja API ja teenuse vastuse kellaajad.
-   **Tegevuste** pakub aruandeid, mis süvitsi teatud tegevuse arendaja, toode, API ja toiming.

## <a name="next-steps"> </a>Järgmised sammud

- Siit saate teada, kuidas [kaitsta oma API määr piirangutega](api-management-howto-product-with-rules.md).

[Azure'i tasuta prooviversioon]: http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=api_management_hero_a

[Create an API Management instance]: #create-service-instance
[Create an API]: #create-api
[Add an operation]: #add-operation
[Add the new API to a product]: #add-api-to-product
[Subscribe to the product that contains the API]: #subscribe
[Call an operation from the Developer Portal]: #call-operation
[View analytics]: #view-analytics
[Next steps]: #next-steps


[How to manage developer accounts in Azure API Management]: api-management-howto-create-or-invite-developers.md
[Configure API settings]: api-management-howto-create-apis.md#configure-api-settings
[Kuidas konfigureerida teatised ja meilisõnumimalle Azure'i API haldus]: api-management-howto-configure-notifications.md
[Responses]: api-management-howto-add-operations.md#responses
[How create and publish a product]: api-management-howto-add-products.md
[Hinnad API haldus]: http://azure.microsoft.com/pricing/details/api-management/

[Azure'i klassikaline portaal]: https://manage.windowsazure.com/

[api-management-management-console]: ./media/api-management-get-started/api-management-management-console.png
[api-management-create-instance-menu]: ./media/api-management-get-started/api-management-create-instance-menu.png
[api-management-create-instance-step1]: ./media/api-management-get-started/api-management-create-instance-step1.png
[api-management-create-instance-step2]: ./media/api-management-get-started/api-management-create-instance-step2.png
[api-management-instance-created]: ./media/api-management-get-started/api-management-instance-created.png
[api-management-import-api]: ./media/api-management-get-started/api-management-import-api.png
[api-management-import-new-api]: ./media/api-management-get-started/api-management-import-new-api.png
[api-management-imported-api-summary]: ./media/api-management-get-started/api-management-imported-api-summary.png
[api-management-calc-operations]: ./media/api-management-get-started/api-management-calc-operations.png
[api-management-list-products]: ./media/api-management-get-started/api-management-list-products.png
[api-management-add-api-to-product]: ./media/api-management-get-started/api-management-add-api-to-product.png
[api-management-add-myechoapi-to-product]: ./media/api-management-get-started/api-management-add-myechoapi-to-product.png
[api-management-api-added-to-product]: ./media/api-management-get-started/api-management-api-added-to-product.png
[api-management-developers]: ./media/api-management-get-started/api-management-developers.png
[api-management-add-subscription]: ./media/api-management-get-started/api-management-add-subscription.png
[api-management-add-subscription-window]: ./media/api-management-get-started/api-management-add-subscription-window.png
[api-management-subscription-added]: ./media/api-management-get-started/api-management-subscription-added.png
[api-management-developer-portal-menu]: ./media/api-management-get-started/api-management-developer-portal-menu.png
[api-management-developer-portal-calc-api]: ./media/api-management-get-started/api-management-developer-portal-calc-api.png
[api-management-developer-portal-calc-api-console]: ./media/api-management-get-started/api-management-developer-portal-calc-api-console.png
[api-management-invoke-get]: ./media/api-management-get-started/api-management-invoke-get.png
[api-management-invoke-get-response]: ./media/api-management-get-started/api-management-invoke-get-response.png
[api-management-manage-menu]: ./media/api-management-get-started/api-management-manage-menu.png
[api-management-dashboard]: ./media/api-management-get-started/api-management-dashboard.png

[api-management-add-response]: ./media/api-management-get-started/api-management-add-response.png
[api-management-add-response-window]: ./media/api-management-get-started/api-management-add-response-window.png
[api-management-developer-key]: ./media/api-management-get-started/api-management-developer-key.png
[api-management-mouse-over]: ./media/api-management-get-started/api-management-mouse-over.png
[api-management-api-summary-metrics]: ./media/api-management-get-started/api-management-api-summary-metrics.png
[api-management-analytics-overview]: ./media/api-management-get-started/api-management-analytics-overview.png
[api-management-analytics-usage]: ./media/api-management-get-started/api-management-analytics-usage.png
[api-management-]: ./media/api-management-get-started/api-management-.png
[api-management-]: ./media/api-management-get-started/api-management-.png
