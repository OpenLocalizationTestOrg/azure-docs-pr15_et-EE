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


# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups---public-preview"></a>Avaldamine rakendused eraldi võrgu ja asukohtade konnektor rühmade – avaliku eelvaate kasutamine

> [AZURE.SELECTOR]
- [Azure'i portaal](active-directory-application-proxy-connectors-azure-portal.md)
- [Azure'i klassikaline portaal](active-directory-application-proxy-connectors.md)


Konnektori abil on mugav erinevad stsenaariumid, sh:

- Mitme ühendatud andmekeskuste saite. Sel juhul, mida soovite säilitada nii palju liikluse andmekeskuse võimalikult, kuna rist-andmekeskuse lingid on kallis ja aeglane. Saate juurutada konnektorid iga andmekeskuses olla ainult rakendusi, mis asuvad andmekeskuse sees. Seda moodust minimeeritakse rist-andmekeskuse linke ja tagab teile täielikult läbipaistev kasutajatele.
- Eraldatud võrkudes, mis pole osa peamised ettevõtte võrgus installitud rakenduste haldamine. Konnektor rühmade abil saate asjakohast konnektori installimine eraldatud võrkudes eristamiseks ka rakenduste võrku.
- Pilvepõhise juurdepääsu IaaS installitud rakendused, konnektor rühmade levinud pakkuda tagada juurdepääs kõik rakendused. Konnektor rühmad ei luua täiendavaid sõltuvus teie ettevõtte võrgus või fragment rakenduse kogemus. Konnektorid iga pilveteenuste andmekeskuse saab installida ja kasutada ainult rakenduste loendis selle võrgu. Saate installida mitu konnektorid kõrge-saadavus saavutamiseks.
- Mitme mets keskkonnas, kus saate teatud konnektorite juurutatud kohta mets ja määrata teatud rakenduste teenida tugi.
- Konnektor rühmad saab avariitaastet saitidel tuvastamiseks kas Tõrkesiirde või varundamise peamine saidi.
- Konnektor rühmi saab kasutada ka olla mitu ettevõtet ühe rentniku kaudu.

## <a name="prerequisite-create-your-connectors"></a>Nõutav: Looge oma konnektorid
Rühma oma konnektorid, peate veenduge, et olete [installinud mitu konnektorid](active-directory-application-proxy-enable.md). Kui installite uue konnektori, ühendab automaatselt **vaikimisi** konnektor rühma.

## <a name="step-1-create-connector-groups"></a>Samm 1: Konnektor rühmade loomine
Saate luua nii palju konnektor rühmi, nagu soovite. Konnektor rühma loomise saab teha [Azure portaali](https://portal.azure.com).

1. Valige **Azure Active Directory** halduse armatuurlaua jaoks kataloogi avamiseks. Valige sealt **Enterprise rakendused** > **rakenduse puhverserver**.

2. Valige nupp **Konnektor rühmad** . Uue konnektori rühma tera kuvatakse.

3. Asemele uus konnektor rühma nimi ja seejärel klõpsake rippmenüü abil saate valida, millised ühendused kuuluvad selles rühmas.

4. Valige **salvestamine** , kui teie konnektor rühm on lõpule jõudnud.

## <a name="step-2-assign-applications-to-your-connector-groups"></a>Samm 2: Määrata rakendusi konnektor rühm
Viimases etapis on konkreetse rakenduse konnektor rühma, mis aitab see.

1. Valige kataloogi armatuurlaualt halduse **Enterprise rakendused** > **Kõik rakendused** > soovite määramine rühmale konnektori rakenduse > **Rakenduse puhverserver**.
2. Jaotises **rühma konnektor**, kasutage rippmenüü ja valige rühm, mida soovite kasutada rakenduse.
3. Valige **Salvesta** muuta.


## <a name="see-also"></a>Vt ka

- [Rakenduse puhverserveri lubamine](active-directory-application-proxy-enable.md)
- [Ühekordse sisselogimise lubamiseks](active-directory-application-proxy-sso-using-kcd.md)
- [Tingimusvormingu juurdepääsu lubamine](active-directory-application-proxy-conditional-access.md)
- [Teil on rakenduse puhverserveri probleemide tõrkeotsing](active-directory-application-proxy-troubleshoot.md)

Uudiseid ja uusimate värskenduste, vaadake [rakenduse puhverserveri ajaveeb](http://blogs.technet.com/b/applicationproxyblog/)
