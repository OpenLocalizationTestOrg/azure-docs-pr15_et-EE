<properties
    pageTitle="Azure Active Directory aruandlus teatised"
    description="Azure Active Directory aruandlus kahtlaste Logi teatiste kasutamine lisandmoodulid."
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/07/2016"
    ms.author="dhanyahk"/>

# <a name="azure-active-directory-reporting-notifications"></a>Azure Active Directory aruandlus teatised

## <a name="what-reports-generate-email-notifications"></a>Milliseid aruandeid luua meiliteatised

Sel ajal, ainult korrapäratu sisselogimine tegevuse aruande päästikute meiliteenuse teatised.

## <a name="what-is-an-irregular-sign-in"></a>Mis on "Korrapäratu märk on"?

Korrapäratu sisselogimist on need, mis on meie Õppekeskuse algoritmide alusel koos mõne Anomaalne asukoht ja seadme "võimalik reisimise" tingimuses masina järgi. Sellele, et häkker on proovinud sisse logida selle kontoga.

## <a name="who-receives-the-email-notifications"></a>Kes saab saata meiliteatisi?

Meilisõnum saadetakse kõik üldadministraatorid, kellele on määratud on Active Directory Premium litsents. See on kohale toimetatud tagamiseks saadame selle administraatorid alternatiivne meiliaadress ka. Administraatorid peaks sisaldama aad-alerts-noreply@mail.windowsazure.com nii, et need ei jääks märkamata e-posti loendis Turvalised saatjad.

## <a name="how-often-are-these-emails-sent"></a>Kui sageli on need meile saata?

Meilisõnum saadetakse kui 10 uue korrapäratu sisselogimise tegevuste tekkida viimase 30 päeva jooksul või pärast viimast meilisõnum saadeti, kumma maht on väiksem.

## <a name="how-do-i-access-the-report-mentioned-in-the-email"></a>Kuidas e-posti mainitud aruande pääseb juurde?

Kui klõpsate linki, suunatakse Azure klassikaline portaali aruandelehele. Aruande juurdepääsuks peate olema nii:

- Admin või co-admin Azure tellimuse

- Üldadministraator kataloogis, ja määratud on Active Directory Premium litsents. Lisateavet leiate teemast [Azure Active Directory väljaanded](active-directory-editions.md).

## <a name="can-i-turn-off-these-emails"></a>Saate välja lülitada need meile?

Jah, Anomaalne sisselogimist Azure klassikaline portaali seotud teatised väljalülitamiseks klõpsake nuppu **Konfigureeri**ja valige **keelatud** jaotises **teatised** .

## <a name="whats-next"></a>Mis saab edasi
- Skype_for_businessis mis turvalisus, audit ja tegevuse aruanded on saadaval? Lugege teemat [Azure AD turvalisus, Audit, ja tegevuse aruanded](active-directory-view-access-usage-reports.md)
- [Azure Active Directory Premium töötamise alustamine](active-directory-get-started-premium.md)
- [Ettevõtte sümboolika Logi sisse ja kasutada paneeli lehtede lisamine](active-directory-add-company-branding.md)
