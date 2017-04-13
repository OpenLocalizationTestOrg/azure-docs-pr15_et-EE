<properties 
    pageTitle="Salvestamine ja teie API halduse konfiguratsiooni abil Git konfigureerimine" 
    description="Saate teada, kuidas salvestada ja teie API halduse konfiguratsiooni abil Git konfigureerimine." 
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


# <a name="how-to-save-and-configure-your-api-management-service-configuration-using-git"></a>Salvestamine ja teie API halduse konfiguratsiooni abil Git konfigureerimine

>[AZURE.IMPORTANT] Git API halduse konfigureerimine on praegu eelvaade. See on funktsionaalselt lõpule jõudnud, kuid on eelvaade, kuna me otsivad aktiivselt selle funktsiooni kohta tagasiside. On võimalik, võib teeme serveris, mis on muuta kliendi tagasiside põhjal, seega soovitame mitte sõltuvalt funktsiooni kasutamiseks Tootmiskeskkondades. Kui teil on tagasiside või küsimusi, palun andke meile teada aadressil `apimgmt@microsoft.com`.

Iga API halduse eksemplari säilitab konfiguratsiooni andmebaasi, mis sisaldab teavet konfiguratsiooni ja metaandmete eksemplari. Saate muudatusi eksemplari muutmata Publisheri portaalis, kasutades PowerShelli cmdlet-käsu või REST API helistamine. Lisaks nende meetodite saate hallata oma eksemplari konfiguratsiooni abil Git, lubada teenuse haldus stsenaariumid, näiteks:

-   Konfiguratsiooni Versioonimine - alla laadida ja salvestada oma teenuse konfigureerimine erinevates versioonides
-   Hulgiredigeerimiseks konfiguratsioonimuudatuste – muudatusi teha mitu osa teie teenuse konfigureerimine oma kohaliku andmebaasi ja ühekordne muudatused serverisse integreerimine
-   Tuttavad Git asukohti ja töövoo – kasutage Git instrumentaarium ja töövood, mille olete juba tuttav

Järgmisel joonisel on esitatud ülevaade erineval viisil konfigureerida oma API halduse eksemplari.

![Git konfigureerimine][api-management-git-configure]

Kui muudate Publisheri portaali, PowerShelli cmdlet-käskude või REST API teenust hallatava teenuse konfiguratsiooni andmebaasi abil soovitud `https://{name}.management.azure-api.net` lõpp-punkti, nagu on näidatud diagrammil paremal. Skeemi vasakus servas näitab, kuidas saate hallata oma teenuse konfiguratsiooni abil Git ja Git hoidla teie teenuse jaoks asub `https://{name}.scm.azure-api.net`.

Järgmised juhised annavad ülevaate haldamine API halduse teenuse eksemplari Git abil.

1.  Oma teenuse Git juurdepääsu lubamine.
2.  Salvestada oma Git hoidla andmebaasi teenuse konfigureerimine
3.  Klooni Git repo oma arvutisse
4.  Tõmmata uusima repo oma arvutisse alla ja kinnitamine ja tõuketeatised muudatused uuesti oma repo
5.  Oma teenuse konfiguratsiooni andmebaasi muudatusi teie Repo juurutamine

Selles artiklis kirjeldatakse, kuidas lubada ja Git abil saate hallata oma teenuse konfigureerimine ja antakse viite failide ja kaustade Git hoidla.

## <a name="to-enable-git-access"></a>Git juurdepääsu lubamiseks

Konfiguratsioonist Git olekut saate vaadata kiiresti vaatamise Publisheri portaali ülemises parempoolses nurgas ikooni Git. Selles näites Git Accessi pole veel lubatud.

![Git olek][api-management-git-icon-enable]

Vaatamiseks ja konfigureerimiseks Git konfiguratsioonisätted, võite Git ikooni, või klõpsake menüüd **Turvalisus** ja liikuge menüü **konfiguratsiooni hoidla** .

![GIT lubamine][api-management-enable-git]

Git juurdepääsu lubamiseks märkeruudu **Git Luba juurdepääs** .

Hetke pärast muudatuse salvestatakse ja kuvatakse kinnitusteade. Pange tähele, et värvi, mis näitab, et Git juurdepääs on lubatud ja nüüd näitab olekuteade, mis on salvestamata muudatusi hoidlasse Git ikoon on muutunud. See on, kuna API halduse teenuse konfiguratsiooni andmebaas pole veel salvestatud hoidlasse.

![Git lubatud][api-management-git-enabled]

>[AZURE.IMPORTANT] Mis tahes saladusi, mis on määratletud, kui atribuudid on salvestatud hoidla jääb ajaloos kuni te keelamine ja uuesti Git juurdepääsu lubamine. Atribuutide pakuvad kindlas kohas hallata konstandi stringi väärtusi, sealhulgas saladusi, kogu API konfigureerimine ja poliitikast, nii, et teil pole vaja salvestada need otse teie poliitika laused. Lisateabe saamiseks vaadake, [Kuidas kasutada Azure API teabehalduspoliitikate atribuudid](api-management-howto-properties.md).

Lubamine või keelamine Git Accessi abil REST API kohta leiate teavet teemast [lubada või keelata Git REST API abil](https://msdn.microsoft.com/library/dn781420.aspx#EnableGit).

## <a name="to-save-the-service-configuration-to-the-git-repository"></a>Teenuse konfiguratsiooni salvestamiseks Git hoidla

Kõigepealt enne kloonimine hoidla on praegune olek teenuse konfiguratsiooni salvestamiseks hoidla. Klõpsake nuppu **Salvesta hoidla konfigureerimine**.

![Salvestage konfigureerimine][api-management-save-configuration]

Kuval kinnitust tehke soovitud muudatused ja klõpsake nuppu **Ok** salvestada.

![Salvestage konfigureerimine][api-management-save-configuration-confirm]

Mõne hetke pärast konfiguratsiooni salvestatakse ja hoidla konfiguratsiooni olek kuvatakse, sh kuupäeva ja kellaaja konfiguratsiooni viimase muutmise ja viimase sünkroonimise teenuse konfiguratsiooni ja hoidla.

![Konfiguratsiooni olek][api-management-configuration-status]

Kui hoidla salvestatakse konfiguratsiooni, saate selle kloonitud.

Seda toimingut kasutades REST API kohta leiate teavet teemast [Kinnita konfiguratsiooni hetktõmmise REST API abil](https://msdn.microsoft.com/library/dn781420.aspx#CommitSnapshot).

## <a name="to-clone-the-repository-to-your-local-machine"></a>Klooni hoidla oma arvutisse

Klooni hoidla, peate URL-i oma andmebaasi, kasutajanime ja parooli. **Konfiguratsiooni hoidla** menüü ülaosas kuvatakse kasutaja nimi ja URL.

![Git klooni][api-management-configuration-git-clone]

Parool on loodud **konfiguratsiooni hoidla** menüü allosas.

![Luua parooli][api-management-generate-password]

Kui soovite luua parooli, esmalt veenduge, et **aegumise** on seatud soovitud aegumise kuupäeva ja kellaaja ja seejärel klõpsake nuppu **Loo Turbeloa**.

![Parooli][api-management-password]

>[AZURE.IMPORTANT] Märkige see parool. Kui jätate selle lehe parooli ei kuvata uuesti.

Järgmistes näidetes kasutatakse Git Bash tööriista [Git for](http://www.git-scm.com/downloads) Windows, kuid saate kasutada mis tahes Git tööriista, mis on teile tuttavad.

Avage oma Git tööriista soovitud kaust ja käivitage järgmine käsk klooni git hoidla Publisheri portaalis esitatud käsu oma arvutisse.

    git clone https://bugbashdev4.scm.azure-api.net/ 

Sisestage kasutajanimi ja parool, kui seda küsitakse.

Kui saate vigu, proovige muuta oma `git clone` käsk kaasata kasutajanimi ja parool, nagu on näidatud järgmises näites.

    git clone https://username:password@bugbashdev4.scm.azure-api.net/

Kui see tõrge, proovige kodeering käsk parooli osa URL-i. Ühe kiire viis selleks on avada Visual Studio ja küsimus järgmine käsk **Vahetu akna**. **Vahetu akna**avamiseks avage mis tahes lahenduse või projekti Visual Studio (või looge uus tühi konsool rakendus), ja klõpsake nuppu **Windows**, menüüst **silumine** **kohe** .

    ?System.NetWebUtility.UrlEncode("password from publisher portal")

Kasutage encoded parooli koos kasutaja nimi ja hoidla asukoha ehitada git käsk.

    git clone https://username:url encoded password@bugbashdev4.scm.azure-api.net/

Kui hoidla on kloonitud saate vaadata ja sellega töötada kohaliku failisüsteemis. Lisateavet leiate teemast [failide ja kaustade struktuuri juhend kohaliku Git hoidla](#file-and-folder-structure-reference-of-local-git-repository).

## <a name="to-update-your-local-repository-with-the-most-current-service-instance-configuration"></a>Värskendada oma kohaliku andmebaasi uusimad teenuse eksemplari konfigureerimine

Kui muudate oma API halduse eksemplari Publisheri portaalis või REST API abil peate salvestama need muudatused hoidlasse saate värskendada oma kohaliku andmebaasi viimaste muudatuste kohta. Selleks, klõpsake nuppu **Salvesta konfiguratsioon hoidlasse** Publisheri portaalis menüü **konfiguratsiooni hoidla** ja seejärel küsimus teie kohaliku hoidlas järgmine käsk.

    git pull

Enne töötab `git pull` veenduge, et olete kausta oma kohaliku andmebaasi. Kui olete just selle `git clone` käsku, siis peate muutma kataloogi oma repo käsu umbes järgmine.

    cd bugbashdev4.scm.azure-api.net/

## <a name="to-push-changes-from-your-local-repo-to-the-server-repo"></a>PTT muudatusi teie kohaliku Repo server repo

Push muudatused oma kohaliku andmebaasi serveri hoidla, peate oma muudatused ja seejärel murravad serveri hoidla. Muudatuste kinnitamiseks oma Git käsk tööriista avamine, aktiveerige oma kohaliku andmebaasi kataloogi ja probleemi järgmised käsud.

    git add --all
    git commit -m "Description of your changes"

Soovite lükata kõik selle võtab server, käivitage järgmine käsk.

    git push

## <a name="to-deploy-any-service-configuration-changes-to-the-api-management-service-instance"></a>Teenuse konfiguratsiooni muudatusi juurutamiseks eksemplari API haldus

Kui teie kohaliku muudatused on kinnitatud ja serveri hoidla lükata, saate need API halduse eksemplari juurutada.

![Juurutamine][api-management-configuration-deploy]

Seda toimingut kasutades REST API kohta leiate teavet teemast [juurutamine Git muudatused konfiguratsiooni andmebaasi REST API abil](https://msdn.microsoft.com/library/dn781420.aspx#DeployChanges).

## <a name="file-and-folder-structure-reference-of-local-git-repository"></a>Failide ja kaustade struktuuri viide kohaliku Git hoidla

Failide ja kaustade kohaliku git hoidla sisaldavad konfiguratsiooni teavet eksemplari.

| Üksuse                       | Kirjeldus                                                                                |
|-------------------------   |--------------------------------------------------------------------------------------------|
| juurkausta api-haldamine | Sisaldab ülataseme eksemplari konfigureerimine                                  |
| API-de kausta                | Sisaldab API-de teenuse eksemplari konfigureerimine                            |
| kausta rühmad              | Sisaldab teenuse eksemplari rühmade konfigureerimine                          |
| poliitika kausta            | Sisaldab poliitikad teenuse eksemplari                                              |
| portalStyles kausta        | Sisaldab teenuse eksemplari kohanduste portaali arendaja konfigureerimine |
| toodete kausta            | Sisaldab toodete teenuse eksemplari konfigureerimine                        |
| kausta Mallid           | Sisaldab e-posti Mallid eksemplari konfigureerimine                 |

Iga kaust võib sisaldada ühte või mitut faili, ja mõnel juhul ühte või mitut kausta, nt kausta iga API, toote või rühma. Iga kaustas failid on kindla üksuse tüüp, mis on kirjeldatud kausta nimi.

| Faili tüüp | Otstarve                                                                |
|-----------|------------------------------------------------------------------------|
| JSON      | Konfiguratsiooni teavet kohta vastav üksus                  |
| HTML-vormingus      | Üksuse, kuvatakse sageli arendaja portaalis kirjeldused |
| XML-i       | Poliitika laused                                                      |
| CSS-i       | Laadilehtede arendaja portaali kohandamine                        |

Neid faile saab luua, kustutada, redigeerida, ja hallata kohaliku failisüsteemi ja uuesti juurutatud muudatused on API halduse eksemplari.

>[AZURE.NOTE] Järgmiste üksuste sisalduvad Git hoidla ja ei saa konfigureerida Git.
>
>-    Kasutajatele
>-    Tellimused
>-    Atribuudid
>-    Arendaja portaali üksuste peale laadid

### <a name="root-api-management-folder"></a>Juurkausta api-haldamine

Juurkausta `api-management` kaust sisaldab mõnda `configuration.json` faili, mis sisaldab ülataseme teavet eksemplari järgmises vormingus.

    {
      "settings": {
        "RegistrationEnabled": "True",
        "UserRegistrationTerms": null,
        "UserRegistrationTermsEnabled": "False",
        "UserRegistrationTermsConsentRequired": "False",
        "DelegationEnabled": "False",
        "DelegationUrl": "",
        "DelegatedSubscriptionEnabled": "False",
        "DelegationValidationKey": ""
      },
      "$ref-policy": "api-management/policies/global.xml"
    }

Neli esimest sätted (`RegistrationEnabled`, `UserRegistrationTerms`, `UserRegistrationTermsEnabled`, ja `UserRegistrationTermsConsentRequired`) kaardi järgmised sätted, klõpsake jaotises **Turvalisus** vahekaarti **identiteedid** .

| Identiteedi säte                     | Kaardid                                               |
|--------------------------------------|-------------------------------------------------------|
| RegistrationEnabled                  | **Anonüümsete kasutajate sisselogimise leht ümber suunata** ruut |
| UserRegistrationTerms                | **Kasutaja registreerimine kasutustingimustega** tekstiväli               |
| UserRegistrationTermsEnabled         | Märkeruut **Kuva registreerumislehel kasutustingimused**         |
| UserRegistrationTermsConsentRequired | Märkeruut **nõua nõusolekut**                          |

![Identiteedi sätted][api-management-identity-settings]

Järgmised neli sätted (`DelegationEnabled`, `DelegationUrl`, `DelegatedSubscriptionEnabled`, ja `DelegationValidationKey`) järgmised sätted jaotises **Turvalisus** vahekaardil **delegeerimine** kaarti.

| Delegeerimine säte           | Kaardid                                    |
|------------------------------|--------------------------------------------|
| DelegationEnabled            | **Volitatud esindaja sisselogimine ja registreerumise** ruut    |
| DelegationUrl                | **Delegeerimine lõpp-punkti URL-i** tekstiväli        |
| DelegatedSubscriptionEnabled | **Volitatud esindaja toote tellimuse** ruut |
| DelegationValidationKey      | **Volitatud esindaja valideerimine klahvi** tekstiväli        |

![Delegeerimine sätted][api-management-delegation-settings]

Lõplik sätte `$ref-policy`, kaartide globaalse poliitika laused faili eksemplari.

### <a name="apis-folder"></a>API-de kausta

Funktsiooni `apis` kaust sisaldab kausta iga API teenuse eksemplari, mis sisaldab järgmist.

-   `apis\<api name>\configuration.json`– See on konfiguratsiooni API ja sisaldab teavet kirjutamata teenuse URL-i ja toimingute kohta. See on sama teave, mida saab tagastatud, kui te ei [saada teatud API](https://msdn.microsoft.com/library/azure/dn781423.aspx#GetAPI) kõne `export=true` sisse `application/json` vorming.
-   `apis\<api name>\api.description.html`– See on API kirjeldus ja vastab olevat `description` atribuudi [API üksus](https://msdn.microsoft.com/library/azure/dn781423.aspx#EntityProperties).
-   `apis\<api name>\operations\`– See kaust sisaldab `<operation name>.description.html` faile, mis Vastendage need toimingud API. Iga fail sisaldab ühekordne API, mis kaardid kirjeldust soovitud `description` atribuudi [toiming üksuse](https://msdn.microsoft.com/library/azure/dn781423.aspx#OperationProperties) REST API.

### <a name="groups-folder"></a>kausta rühmad

Funktsiooni `groups` kaust sisaldab kausta iga rühma määratletud eksemplari.

-   `groups\<group name>\configuration.json`– See on konfiguratsioon rühma. See on sama teave, mida saab tagastatud, kui te ei [saada kindlas rühmas](https://msdn.microsoft.com/library/azure/dn776329.aspx#GetGroup) funktsiooni.
-   `groups\<group name>\description.html`– See on rühma kirjeldus ja vastab olevat `description` atribuudi [rühma üksus](https://msdn.microsoft.com/library/azure/dn776329.aspx#EntityProperties).

### <a name="policies-folder"></a>poliitika kausta

Funktsiooni `policies` kaust sisaldab poliitika laused teenuse eksemplari puhul.

-   `policies\global.xml`-sisaldab poliitikate määratletud globaalse ulatuse oma eksemplari.
-   `policies\apis\<api name>\`– Kui teil on mis tahes poliitikate määratletud API ulatus, sisalduvad selles kaustas.
-   `policies\apis\<api name>\<operation name>\`kaust – kui teil on mis tahes poliitikate määratletud toiming ulatus, sisalduvad selles kaustas `<operation name>.xml` faile, mis poliitika laused iga toimingu vastendada.
-   `policies\products\`– Kui teil on mis tahes poliitikate määratletud toodete, sisalduvad selle kausta, mis sisaldab `<product name>.xml` faile, mis poliitika laused iga toote vastendada.

### <a name="portalstyles-folder"></a>portalStyles kausta

Funktsiooni `portalStyles` kaust sisaldab konfigureerimine ja laadi lehed arendaja portaali kohanduste jaoks eksemplari.

-   `portalStyles\configuration.json`-sisaldab nimesid kasutavad arendaja portaali laadilehed
-   `portalStyles\<style name>.css`-iga `<style name>.css` fail sisaldab laadide arendaja portaali (`Preview.css` ja `Production.css` vaikimisi).

### <a name="products-folder"></a>toodete kausta

Funktsiooni `products` kaust sisaldab kausta iga toote määratletud eksemplari.

-   `products\<product name>\configuration.json`– See on toote konfiguratsioon. See on sama teave, mida saab tagastatud, kui te ei [saada kindla toote](https://msdn.microsoft.com/library/azure/dn776336.aspx#GetProduct) funktsiooni.
-   `products\<product name>\product.description.html`– See on toote kirjeldus ja vastab olevat `description` atribuudi [toote üksuse](https://msdn.microsoft.com/library/azure/dn776336.aspx#Product) REST API.

### <a name="templates"></a>Mallid

Funktsiooni `templates` kaust sisaldab konfigureerimine [meilisõnumimalle](api-management-howto-configure-notifications.md) eksemplari.

-   `<template name>\configuration.json`– See on e-posti malli konfiguratsioon.
-   `<template name>\body.html`– See on meilisõnumi malli sisu.

## <a name="next-steps"></a>Järgmised sammud

Muud võimalused oma eksemplari haldamise kohta leiate teavet teemast:

-   Hallata oma eksemplari järgmist PowerShelli cmdlet-käskude kasutamine
    -   [Teenuse juurutamise PowerShelli cmdleti viide](https://msdn.microsoft.com/library/azure/mt619282.aspx)
    -   [Teenusehaldus PowerShelli cmdleti viide](https://msdn.microsoft.com/library/azure/mt613507.aspx)
-   Publisheri portaalis oma eksemplari haldamine
    -   [Oma esimese API haldamine](api-management-get-started.md)
-   Hallata oma eksemplari REST API abil
    -   [API haldamine REST API viide](https://msdn.microsoft.com/library/azure/dn776326.aspx)

## <a name="watch-a-video-overview"></a>Vaadake videot ülevaade

> [AZURE.VIDEO configuration-over-git]

[api-management-enable-git]: ./media/api-management-configuration-repository-git/api-management-enable-git.png
[api-management-git-enabled]: ./media/api-management-configuration-repository-git/api-management-git-enabled.png
[api-management-save-configuration]: ./media/api-management-configuration-repository-git/api-management-save-configuration.png
[api-management-save-configuration-confirm]: ./media/api-management-configuration-repository-git/api-management-save-configuration-confirm.png
[api-management-configuration-status]: ./media/api-management-configuration-repository-git/api-management-configuration-status.png
[api-management-configuration-git-clone]: ./media/api-management-configuration-repository-git/api-management-configuration-git-clone.png
[api-management-generate-password]: ./media/api-management-configuration-repository-git/api-management-generate-password.png
[api-management-password]: ./media/api-management-configuration-repository-git/api-management-password.png
[api-management-git-configure]: ./media/api-management-configuration-repository-git/api-management-git-configure.png
[api-management-configuration-deploy]: ./media/api-management-configuration-repository-git/api-management-configuration-deploy.png
[api-management-identity-settings]: ./media/api-management-configuration-repository-git/api-management-identity-settings.png
[api-management-delegation-settings]: ./media/api-management-configuration-repository-git/api-management-delegation-settings.png
[api-management-git-icon-enable]: ./media/api-management-configuration-repository-git/api-management-git-icon-enable.png




