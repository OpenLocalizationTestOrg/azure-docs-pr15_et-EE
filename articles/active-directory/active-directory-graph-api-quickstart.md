<properties
   pageTitle="Kiirjuhend: Azure'i AD graafik API | Microsoft Aure"
   description="Azure Active Directory Graph API pakub Programmeerimisjuurdepääs Azure AD kaudu OData REST API lõpp-punktid. Rakenduste abil saate Graph API loomine, lugemine, värskendamine ja kustutamiseks directory andmete ja objektide (CRUD) toiminguid."
   services="active-directory"
   documentationCenter="n/a"
   authors="PatAltimore"
   manager="mbaldwin"
   editor=""
   tags=""/>


   <tags
      ms.service="active-directory"
      ms.devlang="na"
      ms.topic="article"
      ms.tgt_pltfrm="na"
      ms.workload="identity"
      ms.date="09/16/2016"
      ms.author="patricka"/>

# <a name="quickstart-for-the-azure-ad-graph-api"></a>Kiirjuhend: Azure'i AD graafik API

Azure Active Directory (AD) Graph API pakub Programmeerimisjuurdepääs Azure AD kaudu OData REST API lõpp-punktid. Rakenduste abil saate Graph API loomine, lugemine, värskendamine ja kustutamiseks directory andmete ja objektide (CRUD) toiminguid. Näiteks saate luua uus kasutaja, vaatamiseks või kasutaja atribuutide värskendamine, muuta kasutaja parooli, märkige rühmakuuluvus Rollipõhine juurdepääsu, keelamine või kustutamine kasutaja Graph API. Graph API funktsioone ja rakenduse stsenaariumid kohta leiate lisateavet teemast [Azure AD Graph API eeltingimuste](https://msdn.microsoft.com/library/hh974476.aspx)ja [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) . 

> [AZURE.IMPORTANT] Azure'i AD Graph API funktsioonid on saadaval ka läbi [Microsoft Graphi](https://graph.microsoft.io/)ühendatud API, mis sisaldab API-de muude Microsofti teenuste, nt Outlook, OneDrive, OneNote'i, Plaanur ja Office Graphi, puuetega inimestele juurdepääsetavate läbi ühe lõpp-punkti ja ühe juurdepääsu luba.

## <a name="how-to-construct-a-graph-api-url"></a>Kuidas koostada graafik API URL

Graph API, juurdepääsuks directory andmete ja objektide (ehk teisisõnu ressursid või üksuste) suhtes, mida soovite CRUD toiminguid saate URL-ide avamine andmete (OData) protokolli. URL-id, mida kasutatakse Graph API koosnevad neli põhiosa: teenuse juurkausta, rentniku ID, ressursside tee ja Päringusuvandid string: `https://graph.windows.net/{tenant-identifier}/{resource-path}?[query-parameters]`. Võtame näiteks järgmine URL: `https://graph.windows.net/contoso.com/groups?api-version=1.6`.

- **Teenuse Root**: Azure'i AD Graph API juurkausta teenus on alati https://graph.windows.net.
- **Rentniku identifikaator**: see võib olla kinnitatud (registreeritud) domeeninimi contoso.com Ülaltoodud näites. Rentniku objekti ID või "myorganiztion" või "mina" võib olla ka meilipseudonüüm. Lisateabe saamiseks vt [adressaatide valimiseks üksused ja toimingute Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-operations-overview)).
- **Ressursi tee**: see osa URL-i tuvastab ressurssi kasutanud (kasutajad, rühmad, teatud kasutaja, või mõne kindla rühma jne) Ülaltoodud näites on ülataseme "rühmad", mille ressursside määramine. Ka võite käsitleda konkreetset üksust, nt "kasutajate / {ObjectId väärtuse}" või "kasutajate/userPrincipalName".
- **Päringu parameetrite**:? ressursi tee jaotises päringu parameetrite jaotise eraldab. Päringuparameetri "api-versioon" on vaja kõik taotlustest Graph API. Graph API toetavad ka järgmised OData Päringusuvandid: **$filter**, **$orderby**, **$expand**, **$top**ja **$format**. Järgmised Päringusuvandid pole praegu toetatud: **$count**, **$inlinecount**ja **$skip**. Lisateabe saamiseks vt [toetatud päringute, filtrid ja Azure AD Graph API Piipar suvandid](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options).

## <a name="graph-api-versions"></a>Graph API versioonid

Saate määrata versioon Graph API taotlus Päringuparameetri "api-versioon". Versiooni 1,5 ja uuemad versioonid, saate kasutada numbritega versioon väärtust; API-versioon = 1,6. Varasemates versioonides, kasutate kuupäeva string, mis järgib YYYY-MM-DD-vormingusse näiteks api-versiooni 2013-11-08 =. Eelvaade funktsioone, kasutage string "beta"; näiteks api-versiooni beeta =. Graph API versioonidevaheliste erinevuste kohta leiate lisateavet teemast [Azure AD Graph API Versioonimine](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-versioning).

## <a name="graph-api-metadata"></a>API Graphi metaandmed

Tagasi Graph API metaandmete faili, lisada lõigu "$metadata" pärast URL-i näiteks rentniku identifikaator, tagastab järgmine URL metaandmete demo ettevõte, mis kasutavad Graphi Explorer: `https://graph.windows.net/GraphDir1.OnMicrosoft.com/$metadata?api-version=1.6`. Saate sisestada seda URL-i metaandmete kuvamiseks veebibrauseri aadressiribale. Tagastatud alla CSDL metaandmete dokumendi kirjeldatakse üksused ja keerukate tüübid, nende atribuutide ja funktsioonide ja toiminguid, mis on esitatud Graph API soovitud versiooni. Faktitabeli api-versioon parameetri tulemuseks metaandmete jaoks kõige uuem versioon.

## <a name="common-queries"></a>Levinud päringud

[Azure'i AD Graph API levinud päringute](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-supported-queries-filters-and-paging-options#CommonQueries) loendid levinud suhtes, mida saab kasutada Azure AD graafik, sh ülataseme ressursse kataloogis juurdepääsemiseks kasutatavate ja päringute kataloogis toimingute tegemiseks.

Näiteks `https://graph.windows.net/contoso.com/tenantDetails?api-version=1.6` tagastab ettevõtte teave directory contoso.com.

Või `https://graph.windows.net/contoso.com/users?api-version=1.6` on loetletud kõik kasutaja objektide kataloogi contoso.com.

## <a name="using-the-graph-explorer"></a>Graafik Explorerit

Azure AD Graph API Graphi Exploreri abil saate koostate rakenduse kataloogi andmete päring.

> [AZURE.IMPORTANT] Graafik Explorer ei toeta kirjutamiseks või kustutada andmete kataloogi. Te saate ainult toiminguid loetuks Azure AD kataloogi Graphi Exploreriga.

Järgmine on näeksid, kui te ei liikuge Graphi Explorer, valige Kasuta Demo ettevõte ja sisestage väljund `https://graph.windows.net/GraphDir1.OnMicrosoft.com/users?api-version=1.6` kuvada kõigi kasutajate kataloogis demo:

![Azure'i AD Graphi api explorer](./media/active-directory-graph-api-quickstart/graph_explorer.png)

**Graafik Exploreris laadimise**: tööriista laadimiseks liikuge [https://graphexplorer.cloudapp.net/](https://graphexplorer.cloudapp.net/). Klõpsake nuppu **Kasuta Demo ettevõtte** pidamine Graph Exploreri valimi rentniku andmetega. Peate kasutama demo ettevõtte mandaadi. Teise võimalusena saate klõpsake linki **Logi sisse** ja logige oma Azure AD konto mandaadid Graphi Exploreri vastuolus teie rentniku. Kui käivitate Graphi Exploreri oma rentniku vastu, peate teie või administraator nõusolekut sisselogimisel. Kui teil on Office 365 tellimus, on teil automaatselt mõne Azure AD rentniku. Mandaadi abil saate sisse logida Office 365 on tegelikult Azure AD kontod, saate neid mandaate Graphi Exploreriga.

**Päringu käitamine**: päringu käivitamiseks Sisestage päring tekstiväljale taotlus ja klõpsake nuppu **saada** või klõpsake nuppu **Sisesta** tootevõti. Tulemused kuvatakse väljal vastuse. Näiteks `https://graph.windows.net/graphdir1.onmicrosoft.com /groups?api-version=1.6` loetleb kõik kataloogis demo objektide rühmitamine.

Pöörake tähelepanu järgmistele funktsioonidele ja piirangutele Graphi Exploreri.
- Määrab automaatteksti videovõimaluste ressursi. Selle kuvamiseks klõpsake nuppu **Kasuta Demo ettevõtte** ja seejärel klõpsake taotluse tekstivälja (kui ettevõtte URL-i kuvatakse). Valige ripploendist seadmine ressursi.

- Toetab "mina" ja "myorganization" pseudonüümid. Näiteks saate kasutada `https://graph.windows.net/me?api-version=1.6` tagastamiseks kasutaja objekti sisselogitud kasutaja või `https://graph.windows.net/myorganization/users?api-version=1.6` praegust kausta kõigi kasutajate tagastamiseks. Pange tähele, et abil "mina" alias Tagastab veaväärtuse, demo ettevõttele sellepärast, et pole sisse logitud kasutajale taotluse.

- Vastus jaotise päised. Seda saab kasutada tõrkeotsinguks probleemid, mis ilmnevad, kui operatsioonisüsteemi päringud.

- JSON vaataja tagasiside Laienda ja Ahenda võimalustega.

- Pisipildi foto kuvamiseks ei toetata.

## <a name="using-fiddler-to-write-to-the-directory"></a>Kirjutage kataloogi viiuldaja abil

Kiirjuhend juhendi eesmärgil saate kasutada Fiddler Web Silur et harjutada tegelik 'kirjutamine toimingute teie vastu Azure AD kataloogi. Lisateabe saamiseks ja installimiseks viiuldaja vt [http://www.telerik.com/fiddler](http://www.telerik.com/fiddler).

Järgmises näites on Fiddler Web Silur abil luua uue turberühma 'MyTestGroup' Azure AD kataloogis.

**Hangi juurdepääsu sümboolse**: Azure'i AD Graphile juurdepääsu kliendid peavad edukalt autentimiseks Azure AD esmalt. Lisateavet leiate teemast [Autentimise stsenaariumid Azure AD](active-directory-authentication-scenarios.md).

**Koostamine ja käivitage päring**: täitke järgmised juhised.

1. Avage Fiddler Web Silur ja **helilooja** vahekaardi aktiveerimine.
2. Kuna soovite luua uue turberühma, valige rippmenüüst menüü meetod HTTP **postituse** . Toimingute ja õiguste rühma objekti kohta leiate lisateavet teemast [Azure AD Graphi REST API viide](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog) [rühma](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#GroupEntity) .
3. **Postituse**kõrval väljale tippige järgmine taotluse URL: `https://graph.windows.net/mytenantdomain/groups?api-version=1.6`.

    > [AZURE.NOTE] Peate asendada mytenantdomain Azure AD kataloogi domeeni nimega.

4. Tippige väljale otse all postituse rippmenüüst järgmist:

    ```
Host: graph.windows.net
Authorization: your access token
Content-Type: application/json
```

    > [AZURE.NOTE] SUBSTITUTE oma &lt;teie juurdepääsu luba&gt; koos Azure AD kataloogi Accessi sõnet.

5. Tippige väljale **koosolekukutse kehasse** järgmist:

    ```
        {
            "displayName":"MyTestGroup",
            "mailNickname":"MyTestGroup",
            "mailEnabled":"false",
            "securityEnabled": true
        }
```

    Rühmade loomise kohta leiate lisateavet teemast [Rühma loomine](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/groups-operations#CreateGroup).

Lisateavet Azure AD üksuste ja tüüpi, mis on esitatud graafik ja toimingud, mida saab teha neid Graphi teavet leiate teemast [Azure AD Graphi REST API viide](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog).

## <a name="next-steps"></a>Järgmised sammud

- Lisateavet [Azure AD Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/api-catalog)
- Lisateavet [Azure AD Graph API õiguste otsinguulatuste](https://msdn.microsoft.com/Library/Azure/Ad/Graph/howto/azure-ad-graph-api-permission-scopes)
