<properties
    pageTitle="Azure'i AD Rakenduse puhverserveri konnektorid töötamine | Microsoft Azure'i"
    description="Hõlmab, kuidas luua ja hallata Azure AD Rakenduse puhverserveri konnektorid."
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/09/2016"
    ms.author="kgremban"/>


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a>Avaldamine rakendused eraldi võrgu ja asukohtade konnektor rühmade kasutamine

> [AZURE.SELECTOR]
- [Azure'i portaal](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure'i klassikaline portaal](active-directory-application-proxy-connectors.md)


Konnektori abil on mugav erinevad stsenaariumid, sh:

- Mitme ühendatud andmekeskuste saite. Sel juhul, mida soovite säilitada nii palju liikluse andmekeskuse võimalikult, kuna rist-andmekeskuse lingid on tavaliselt kallis ja aeglane. Saate juurutada konnektorid iga andmekeskuses olla ainult rakendusi, mis asuvad andmekeskuse sees. Seda moodust minimeeritakse rist-andmekeskuse linke ja tagab teile täielikult läbipaistev kasutajatele.
- Eraldatud võrkudes, mis pole osa peamised ettevõtte võrgus installitud rakenduste haldamine. Konnektor rühmade abil saate asjakohast konnektori installimine eraldatud võrkudes eristamiseks ka rakenduste võrku.
- Pilvepõhise juurdepääsu IaaS installitud rakendused, konnektor rühmade levinud pakkuda tagada juurdepääs kõik rakendused. Konnektor rühmad ei luua täiendavaid sõltuvus teie ettevõtte võrgus või fragment rakenduse kogemus. Konnektorid iga pilveteenuste andmekeskuse saab installida ja kasutada ainult rakenduste loendis selle võrgu. Saate installida mitu konnektorid kõrge-saadavus saavutamiseks.
- Mitme mets keskkonnas, kus saate teatud konnektorite juurutatud kohta mets ja määrata teatud rakenduste teenida tugi.
- Konnektor rühmad saab avariitaastet saitidel tuvastamiseks kas Tõrkesiirde või varundamise peamine saidi.
- Konnektor rühmi saab kasutada ka olla mitu ettevõtet ühe rentniku kaudu.

## <a name="prerequisite-create-your-connectors"></a>Nõutav: Looge oma konnektorid
Selleks, et teie konnektorid rühmitamiseks lasta veenduge et [installitud mitu konnektorid](active-directory-application-proxy-enable.md)ja selle nimi ja seejärel rühmitage need. Lõpetuseks tuleb määrata teatud rakendused.

## <a name="step-1-create-connector-groups"></a>Samm 1: Konnektor rühmade loomine
Saate luua nii palju konnektor rühmi, nagu soovite. Azure'i klassikaline portaalis saab teha konnektor rühma loomine.

1. Valige kataloogi ja klõpsake nuppu **Konfigureeri**.  
    ![Rakenduse puhverserver, konfigureerida kuvatõmmis – valige konnektor rühmade haldamiseks](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)

2. Jaotises rakenduse puhverserver, nuppu **Halda konnektor rühmad** ja konnektori uue rühma loomine, andes rühma nime.  
    ![Rakenduse puhverserveri konnektor rühmade kuvatõmmis - uue rühma nimi](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)

## <a name="step-2-assign-connectors-to-your-groups"></a>Samm 2: Teie rühmadele konnektorid määramine
Kui konnektor rühmad on loodud, liikuda konnektorid soovitud rühma.

1. Klõpsake jaotises **Rakenduse puhverserveri** **Haldamine konnektorid**.
2. Valige jaotises **rühma**iga Connectori soovitud rühm. Kuluda kuni 10 minuti aktiveerimise jaotises Uus konnektorid.  
    ![Rakenduse puhverserveri konnektorid kuvatõmmis – valige rippmenüüst rühm](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)

## <a name="step-3-assign-applications-to-your-connector-groups"></a>Samm 3: Määramine rakendusi konnektor rühm
Viimases etapis on konkreetse rakenduse konnektor rühma, mis aitab see.

1. Azure'i klassikaline portaalis kataloogi, valige rakendus soovite määrata rühma ja klõpsake nuppu **Konfigureeri**.
2. Valige jaotises **konnektor rühma**soovitud rühm rakenduse kasutamiseks. Selle muudatuse rakendatakse kohe.  
    ![Rakenduse puhverserveri konnektor rühma kuvatõmmis – valige rippmenüüst rühm](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)


## <a name="see-also"></a>Vt ka

- [Rakenduse puhverserveri lubamine](active-directory-application-proxy-enable.md)
- [Ühekordse sisselogimise lubamiseks](active-directory-application-proxy-sso-using-kcd.md)
- [Tingimusvormingu juurdepääsu lubamine](active-directory-application-proxy-conditional-access.md)
- [Teil on rakenduse puhverserveri probleemide tõrkeotsing](active-directory-application-proxy-troubleshoot.md)

Uudiseid ja uusimate värskenduste, vaadake [rakenduse puhverserveri ajaveeb](http://blogs.technet.com/b/applicationproxyblog/)
