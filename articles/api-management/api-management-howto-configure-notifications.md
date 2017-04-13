<properties 
    pageTitle="Kuidas konfigureerida teatised ja meilisõnumimalle Azure'i API haldus" 
    description="Siit saate teada, kuidas konfigureerida teatiste ja e-posti Mallid Azure'i API haldus." 
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

# <a name="how-to-configure-notifications-and-email-templates-in-azure-api-management"></a>Kuidas konfigureerida teatised ja meilisõnumimalle Azure'i API haldus

API halduse pakub võimalust konfigureerida teatiste teatud sündmuste ja konfigureerimiseks suhelda administraatorid ja arendajate API halduse eksemplari kasutatud e-posti mallid. See teema näitab, kuidas konfigureerida teatiste jaoks saadaolevad sündmused ja antakse ülevaade meilisõnumimalle, kasutatakse neid sündmuste konfigureerimine.

## <a name="publisher-notifications"> </a>Publisher teatiste konfigureerimine

Konfigureerida teatiste, klõpsake nuppu **Halda** API halduse teenust Azure klassikaline portaalis. See viib teid Publisheri API haldusportaali.

![Publisheri portaal][api-management-management-console]

>Kui te pole veel loonud API halduse teenuse eksemplar, teemast [Azure API kasutamist alustada][] õpetuses [loomine API halduse teenuse eksemplar][] .

Klõpsake vasakul kuvamiseks saadaval teatised menüükäsku **API haldus** **teatised** .

![Publisheri teatised][api-management-publisher-notifications]

Järgmises loendis sündmuste saab konfigureerida teatiste saamiseks.

-   **Tellimuste (heakskiitu)** - määratud e-posti adressaadid ja kasutajad saavad tellimuse taotlusi heakskiitu API toodete kohta meiliteatiste.
-   **Uue tellimused** - määratud e-posti adressaadid ja kasutajad saavad meiliteatiste uue API toote tellimused.
-   **Rakenduse Galerii taotlused** - määratud e-posti adressaadid ja kasutajad saavad meiliteatisi, kui rakenduste galeriisse on uue taotlused.
-   **Salakoopia** - määratud e-posti adressaadid ja kasutajad saavad e-posti pimedad koopia kõik meilisõnumid arendajatele.
-   **Uus teema või kommentaari** - määratud meilisõnumite adressaatide ja kasutajate saab meiliteatised, kui uus teema või arendaja portaalis välja kommentaari.
-   **Sule konto sõnumi** - määratud e-posti adressaadid ja kasutajad saavad meiliteatisi, kui konto on suletud.
-   **Lähenen tellimuse kvoodilimiit** - järgmised meilisõnumite adressaatide ja kasutajad saavad meiliteatisi, kui tellimuse kasutus saab kasutuse piirmäära lähedale.

Iga meilisõnumite adressaatide e-posti aadressi tekstivälja abil saate määrata, või saate kasutajate loendit.

E-posti aadressid, et teid teavitatakse määramiseks sisestage nende e-posti aadressi tekstiväljale. Kui teil on mitu e-posti aadressid, need eraldada komadega abil.

![Teatis adressaadid][api-management-email-addresses]

Kasutajaid teavitama määramiseks klõpsake nuppu **Lisa adressaat**, kasutajaid teavitama kõrval ruut ja klõpsake nuppu **OK**.

>Pange tähele, et loendis kuvatakse ainult administraatorid.

Pärast konfigureerimist teatis adressaadid, klõpsake **salvestamine** värskendatud teatis adressaadid.

>Kui lahkute **Publisheri teatised** vahekaardil Publisheri portaali annab märku, kui on salvestamata muudatusi.

## <a name="email-templates"> </a>Konfigureerimine e-posti Mallid

API haldus pakub meilisõnumimalle haldamise ja teenuse kasutamise käigus saadetud meilisõnumite jaoks. E-posti järgmised Mallid on saadaval.

-   Rakenduse Galerii esitamise kinnitatud
-   Arendaja hüvasti kirja
-   Teatis lähenemas arendaja kvoodilimiit
-   Kasutajate kutsumine
-   Uue kommentaari lisanud probleemi
-   Uue küsimuse saanud
-   Uus tellimus, mis on aktiveeritud
-   Tellimust uuendada, kinnituse
-   Tellimuse taotluse keelanud
-   Tellimuse taotluse vastu

Neid malle saab muuta soovitud.

Vaatamiseks ja konfigureerimiseks oma API halduse eksemplari e-posti Mallid, klõpsake vasakul **API haldus** käsku **teatised** ja valige vahekaart **Meilisõnumimalle** .

![E-posti Mallid][api-management-email-templates]

Saate vaadata või muuta kindla malli, valige see loendist **mallide** rippmenüü.

![E-posti mallide loendi][api-management-email-templates-list]

Iga e-posti mall on teema lihttekstina ja keha määratlus HTML-vormingus. Iga üksuse saab kohandada soovitud.

![Malli meiliredaktor.][api-management-email-template]

**Parameetrite** loend sisaldab loendit parameetrite, mis, kui teema või sisu lisada, saab asendada määratud väärtuse, kui meilisõnumi saatmist. Parameetri lisamiseks asetage kursor kohta, kuhu soovite minna parameetri ja klõpsake vasakus servas parameetri nimi noolt.

Klõpsake nuppu **Eelvaade** või **saada test** näha, kuidas meilisõnumeid vaadata või testi meilisõnumi saatmine.

>Pange tähele, et parameetrid on asendatud tegelike väärtuste eelvaate või saatmist test.
>
>Meilisõnumi malli muudatused salvestada, klõpsake nuppu **Salvesta**või tühistamiseks klõpsake muudatuste **tühistamine**.



[api-management-management-console]: ./media/api-management-howto-configure-notifications/api-management-management-console.png
[api-management-publisher-notifications]: ./media/api-management-howto-configure-notifications/api-management-publisher-notifications.png
[api-management-email-addresses]: ./media/api-management-howto-configure-notifications/api-management-email-addresses.png


[api-management-email-templates]: ./media/api-management-howto-configure-notifications/api-management-email-templates.png
[api-management-email-templates-list]: ./media/api-management-howto-configure-notifications/api-management-email-templates-list.png
[api-management-email-template]: ./media/api-management-howto-configure-notifications/api-management-email-template.png


[Configure publisher notifications]: #publisher-notifications
[Configure email templates]: #email-templates

[How to create and use groups]: api-management-howto-create-groups.md
[How to associate groups with developers]: api-management-howto-create-groups.md#associate-group-developer

[Azure'i API kasutamist alustada]: api-management-get-started.md
[API halduse teenuse eksemplari loomine]: api-management-get-started.md#create-service-instance