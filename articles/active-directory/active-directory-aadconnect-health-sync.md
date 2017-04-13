
<properties
    pageTitle="Azure'i AD-ühenduse seisundi abil sünkroonimine | Microsoft Azure'i"
    description="See on seisund Azure'i AD-ühenduse leht, et arutada, kuidas Azure'i AD-ühenduse sünkroonimine jälgimiseks."
    services="active-directory"
    documentationCenter=""
    authors="karavar"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="using-azure-ad-connect-health-for-sync"></a>Azure'i AD-ühenduse seisundi kasutamise sünkroonimine
Järgmised dokumendid on Azure AD ühenduse (Sünkrooni) Azure'i AD-ühenduse seisundi jälgimine.  Azure'i AD-ühenduse tervist AD FS-i jälgimise kohta teavet teemast [AD FS seisundi abil Azure'i AD-ühenduse](active-directory-aadconnect-health-adfs.md). Lisaks järelevalve Active Directory domeeniteenused Azure'i AD-ühenduse seisundi kohta teavet teemast [AD DS tervist abil Azure'i AD-ühenduse](active-directory-aadconnect-health-adds.md).

![Azure'i AD-ühenduse sünkroonimise seisund](./media/active-directory-aadconnect-health-sync/sync-blade.png)

## <a name="alerts-for-azure-ad-connect-health-for-sync"></a>Azure'i AD-ühenduse seisundi sünkroonimise teatised
Azure'i AD-ühenduse seisund teatiste sünkroonimine jaotise pakub teile aktiivse teatiste loendit. Iga teade sisaldab asjakohast teavet, eraldusvõime juhiseid ja lingid seotud dokumendid. Valides aktiivne või lahendatud teatise teile kuvatakse uue abaluuga täiendavat teavet ning te saate teha lahendamiseks teatise ja lingid täiendavaid dokumente. Andmeid saate vaadata ka varem lahendatud teatised.

Teatise teile antakse lisateavet samuti juhiseid valides saate teha lahendamiseks teatise ja lingid täiendavaid dokumente.

![Azure'i AD-ühendus sünkroonimise tõrge](./media/active-directory-aadconnect-health-sync/alert.png)

### <a name="limited-evaluation-of-alerts"></a>Teatiste piiratud hindamine
Kui Azure'i AD-ühenduse on kasutusel vaikimisi konfiguratsiooni (näiteks siis, kui atribuut filtreerimine on muutunud vaikimisi konfiguratsiooni kohandatud konfiguratsiooni), siis üles laadida Azure'i AD-ühenduse seisund agent seotud Azure'i AD-ühenduse tõrge sündmuste.

See piirab teatiste hindamisest teenus. Soovite kuvatakse venitada, mis näitab, et see tingimus Azure'i portaalis jaotises teenust.

![Azure'i AD-ühenduse sünkroonimise seisund](./media/active-directory-aadconnect-health-sync/banner.png)

Saate muuta seda, kui klõpsate "Settings" ja lubamisel Azure'i AD-ühenduse seisund agent kõik tõrge logid üles laadida.

![Azure'i AD-ühenduse sünkroonimise seisund](./media/active-directory-aadconnect-health-sync/banner2.png)

## <a name="sync-insight"></a>Sünkroonimine tutvustus
Administraatorid sageli peaksite teadma aega kulub Azure AD muudatuste sünkroonimine ja muutusi summa. Selle funktsiooni abil on lihtne see visualiseerida, kasutades funktsiooni all graafikud:   

- Latentsus Synci toimingud
- Objekti muutmine trendi.

### <a name="sync-latency"></a>Latentsus sünkroonimine
See funktsioon pakub graafiline trend latentsus sünkroonimine toimingute (impordi, ekspordi jne) konnektorid.  See annab kiire ja lihtne viis, et aru saada, mitte ainult latentsus oma toimingud (suurem kui teil on suur hulk muutusi), kuid võimalus tuvastamine kõrvalekaldeid latentsus, mis võivad nõuda uurimine.

![Latentsus sünkroonimine](./media/active-directory-aadconnect-health-sync/synclatency02.png)

Vaikimisi kuvatakse ainult "' eksporditoimingu Azure AD konnektor latentsus.  Soovite vaadata veel toiminguid konnektor või muude konnektorid toimingu käigus kuvamiseks paremklõpsake diagrammi, valige Redigeeri diagrammi või "Redigeerimine latentsus diagrammi" nuppu ja valige soovitud toiming ja konnektorid.

### <a name="sync-object-changes"></a>Muudatuste sünkroonimine objekt
See funktsioon pakub graafiline trend muudatused, mis on arvu hinnatud ja Azure AD eksportida.  Täna, püüab selle teabe kogumine sünkroonimine logid on keeruline.  Diagrammi annab teile, mitte ainult lihtsam viis jälgida muudatusi, mis on teie keskkonnas esinevate arvu, kuid vaate tõrkeid, mis toimuvad.

![Latentsus sünkroonimine](./media/active-directory-aadconnect-health-sync/syncobjectchanges02.png)

## <a name="object-level-synchronization-error-report-preview"></a>Objekti taseme Kataloogisünkroonimise kohta tõrketeatise (eelvaade)
See funktsioon pakub aruande Sünkroonimistõrgete, mis võivad tekkida identiteedi andmed on sünkroonitud Windows Server AD ja Azure AD Azure'i AD-ühenduse kasutamise kohta.

- Aruanne hõlmab salvestatud sünkroonimisrakendus tõrked (Azure'i AD-ühenduse versioon 1.1.281.0 või uuem versioon)
- See hõlmab ilmnenud tõrgete sünkroonimise Viimane toiming mootor sünkroonimine. ("Ekspordi" Azure AD konnektor.)
- Azure'i AD-ühendus seisund agent sünkroonimiseks peab olema Väljamineva meili Ühenduvus uusimate andmete kaasamiseks aruande jaoks nõutav lõpp-punktid. Teavet leiate üksikasjad [jaotises nõuetele](active-directory-aadconnect-health-agent-install.md#Requirements) .
- Aruanne on **värskendatud iga 30 minuti järel** andmeid Azure'i AD-ühenduse seisund agent sünkroonimise abil.
Järgmisi olulisi võimalusi pakub

    - Tõrgete kategoriseerimine
    - Objektide tõrkega kategooria kohta
    - Kõik andmed ühes kohas tõrked
    - Kõrvuti on erineb objektide tõrkega konflikti tõttu
    - Laadi alla tõrkearuande CV-d, (tulekul)

### <a name="categorization-of-errors"></a>Tõrgete kategoriseerimine
Aruande kategooria olemasoleva Sünkroonimistõrgete järgmiste kategooriate:

| Kategooria | Kirjeldus |
| -------------- | ----------- |
| Atribuut dubleerimine | Kui avaldab Azure'i AD-ühenduse loomine või värskendada objektide Azure AD, mis peab olema kordumatu rentniku, nt proxyAddresses UserPrincipalName ühe või mitme atribuutide dubleeritud väärtused. |
| Andmete lahknevuse | Tõrked sujuvate-match ei sobi objektid, mille tulemuseks Sünkroonimistõrgete. |
| Andmete valideerimine ebaõnnestus. | Tõrgete tõttu vigaste andmete sisestamist, nt toetamata märgid kriitiliste atribuute, nagu UserPrincipalName, vormindage tõrkeid, mis enne Azure AD kirjutatud valideerimine nurjub.|
| Suure atribuut | Tõrkeid, kui üks või mitu atribuudid on suurem kui lubatud maht, pikkus või loendus.|
| Muud | Kõik muud vead, mis ei sobi ülaltoodud kategooriatesse. Tagasiside põhjal jagatakse selle kategooria sub kategooriad.

![Tõrketeade aruande kokkuvõte sünkroonimine](./media/active-directory-aadconnect-health-sync/errorreport01.png)
![sünkroonimine tõrketeade aruande kategooriad](./media/active-directory-aadconnect-health-sync/errorreport02.png)

### <a name="list-of-objects-with-error-per-category"></a>Objektide tõrkega kategooria kohta
Iga kategooriasse süveneda just annab võttes viga kategooria objektide loendi.
![Sünkroonimise tõrge aruande loend](./media/active-directory-aadconnect-health-sync/errorreport03.png)

### <a name="error-details"></a>Tõrke üksikasjade
Andmed on saadaval iga tõrke üksikasjalik vaates

- Identifikaatorite seotud *AD objekt*
- Identifikaatorite *Azure AD objekti* seotud (vajaduse korral)
- Tõrke kirjeldus ja kuidas seda parandada
- Seotud artiklid

![Sünkroonimine tõrketeatise üksikasjad](./media/active-directory-aadconnect-health-sync/errorreport04.png)

### <a name="download-the-error-report-as-csv"></a>Laadige alla tõrkearuande CSV-vormingus
See funktsioon on Varsti tulekul. Hoia rohkem uuendused.



## <a name="related-links"></a>Seotud lingid
* [Tõrkeotsing sünkroonimise ajal](active-directory-aadconnect-troubleshoot-sync-errors.md)
* [Atribuut paindlikkust dubleerimine](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md)
* [Azure'i AD-ühenduse seisund](active-directory-aadconnect-health.md)
* [Azure'i AD-ühenduse seisund Agent installimine](active-directory-aadconnect-health-agent-install.md)
* [Azure'i AD-ühenduse seisund toimingud](active-directory-aadconnect-health-operations.md)
* [Kasutades Azure AD kasutajaga AD FS-i seisund](active-directory-aadconnect-health-adfs.md)
* [Kasutades Azure AD kasutajaga AD DS seisund](active-directory-aadconnect-health-adds.md)
* [Azure'i AD-ühenduse seisund KKK](active-directory-aadconnect-health-faq.md)
* [Azure'i AD-ühenduse seisund versiooniajalugu](active-directory-aadconnect-health-version-history.md)
