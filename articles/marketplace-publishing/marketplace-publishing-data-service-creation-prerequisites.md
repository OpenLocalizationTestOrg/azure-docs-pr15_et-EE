<properties
   pageTitle="Tehniline eeltingimused loomise Data Service turuplatsil | Microsoft Azure'i"
   description="Nõuded loomise Data Service juurutada ja müüa Azure'i turuplatsilt"
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager="hascipio"
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/26/2016"
   ms.author="hascipio; avikova" />

# <a name="technical-pre-requisites-for-creating-a-data-service-offer-for-the-azure-marketplace"></a>Tehniline eeltingimused loomise andmete teenust pakub Azure'i turuplatsil

>[AZURE.IMPORTANT] **Sel ajal me ei ole enam kasutuselevõtt mis tahes uute andmete teenuse tootjad. Uue dataservices ei saada heaks kirjet.** Kui teil on SaaS ärirakendusi soovite avaldada AppSource leiate lisateavet [siin](https://appsource.microsoft.com/partners). Kui teil on mõni IaaS rakendusi või arendaja teenuse soovite avaldatava Azure'i turuplatsi võite leida rohkem teavet [siin](https://azure.microsoft.com/marketplace/programs/certified/).

Protsessi põhjalikult enne alustamist lugege ja mõista, miks ja kuhu iga toimingu sooritatakse. Võimalikult palju, saate peaks oma ettevõtte andmete ja muude andmete ettevalmistamine, vajalikud tööriistad ja/või luua tehnilise komponendid enne pakkumise loomisprotsessi.

Peaks olema valmis enne selle toimingu algust järgmist:

## <a name="make-a-decision-on-what-technology-will-be-used-to-publish-your-data-service-offer"></a>Otsustab, millist tehnoloogiat kasutatakse avaldada oma andmeid teenuse pakkumine

Avaldaja saate valida mitme tehnoloogiad avaldamisel Data Service Azure'i turuplatsilt. Peamised tehnoloogiaid, mis on toetatud allpool kirjeldatud. Sõltumata sellest, milline mis tehnoloogiat kasutatakse avaldada Data Service, lõppkasutaja tarbib kaudu soovitud **OData kanali** esitatud teenuse Azure'i turuplatsi andmed. Täielik teave OData teenuse leiate [http://www.odata.org/](http://www.odata.org/)

## <a name="sql-azure-database"></a>SQL Azure'i andmebaas

Andmekomplekti valmis SQL Azure'i on avaldaja kohustusi. Peate tellimine Azure, ette vastav andmebaasi mahtu ja SQL Azure'i DB andmete laadida. Publisheri vastutab hoida oma andmed alati ajakohased. Lisateabe saamiseks Azure'i teenuste tellimise leiate [https://azure.microsoft.com/services/sql-database/](https://azure.microsoft.com/services/sql-database/)


Kui liigub SQL Azure'i andmed, Azure'i turuplatsi saate seada, tabelid ja vaated. Avaldaja saate määrata, millised tabelid ja vaated ja veergude puutuvad lõppkasutaja. Täpsemaks sisu pakkuja saate määrata veergude saate kasutataks lõppkasutaja ja millised on last ainult tagastatud. See annab kõrge paindlikkust kohta, millised andmed andmebaasis puutuda. Veerud, mida saab vaja peab toetama vähemalt ühe andmebaasi indeksid.

## <a name="rest-based-web-service"></a>Vastavalt REST veebiteenuse

Toetatud Protocol (protokoll): **HTTPS ainult**

Olemasoleva ülejäänud põhiste teenuste võivad puutuda Azure'i turuplatsi kaudu. Kuna andmekomplekti on alati avatud lõppkasutaja nimega OData kanali, vastavalt Azure'i turuplatsi teenuse vajadustele saama teenuse vastendamine loomine OData teenuse. Esitamist kõik parameetrid HTTP parameetrid vaja teha nii, et ülejäänud vastavalt lõpp-punktid.

Funktsiooni last peab olema vorm, mis saab vastendatud mõne ATOMI vastusesse. Seega sisaldavad vastuse teenuste peab olema XML-vormingus ja ta saab ainult ühe korduvat elementi, mis sisaldab last väärtusi (nt kirjekomplekti). Azure'i turuplatsi teenus on vastendada atribuudi sõlmed sees kirje sõlm korduva sõlm kirje sõlm ATOMI ja last väärtused.

Luba teabe (nt API võti autentimise Turbeloa, jne) peab olema antud mõne HTTP parameetrina või HTTP päises (võtmeväärtuse paari) – elementaarautentimine toetatakse ka. Kehtiv võti peab olema ja kõik kutsed Azure'i turuplatsi kaudu tehakse selle klahvi kaudu. Kasutaja jälgimine ja arveldamine toimub kihis Azure'i turuplatsilt.

Teenuse tulemusel ilmnenud tõrked vaja HTTP olekukoodi, tuleb kaardistada. Juhuks, kui teenus tagastab XML-i, mis sisaldab viga need hakkavad Azure'i turuplatsi teenus HTTP olek koodide vastendada.

## <a name="soap-based-web-services"></a>Vastavalt SOAP veebiteenused

Protokoll: **HTTPS ainult**

Nõuded on sama mis ülejäänud vastavalt jaotist teenuse. Ainus erinevus on, et parameetrid saab anda ka kehasse XML-i, mis postitatakse avaldaja teenusega alati taotlusega Azure'i turuplatsi kaudu. See tähendab, et kasutaja annab selle ees HTTP parameetrite tõlgitakse XML-elementide XML-dokumendi, mis postitatakse sisu pakkuja veebiteenuse taotluse.

## <a name="odata-based-web-services"></a>OData vastavalt veebiteenused

Protokoll: **HTTPS ainult**

OData teenuste Azure'i turuplatsi andmed avatud. Süsteemi saab teenuse kaudu edastavad ja asendab selle juurdomeen Azure'i turuplatsi teenuse root – kõik ajakava läbida Azure'i turuplatsi tagamiseks.

OData teenuste ainult pole vaja minna on taustväärtus andmebaasi. OData toetab mis tahes tüüpi salvestusruumi või business loogika teenuse juhtida.


## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui selle eeltingimused läbi vaadata ja vajalikud toimingud lõpule, saate liikuda edasi luua oma andmete teenuse pakkumine nimega üksikasjalikud [Andmed teenuse avaldamise juhend](marketplace-publishing-data-service-creation.md).

Või kui soovite kogu protsessi ja artiklites üle vaadata iga avaldamise etapid, külastage [Alustamine: Kuidas avaldada pakkumise Azure'i turuplatsi](marketplace-publishing-getting-started.md).

[link-acct]:marketplace-publishing-accounts-creation-registration.md
