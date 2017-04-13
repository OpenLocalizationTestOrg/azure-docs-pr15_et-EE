
<properties
    pageTitle="Kasutades Azure AD kasutajaga AD DS seisund | Microsoft Azure'i"
    description="See on seisund Azure'i AD-ühenduse leht, et arutada, kuidas AD DS jälgimiseks."
    services="active-directory"
    documentationCenter=""
    authors="arluca"
    manager="samueld"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/18/2016"
    ms.author="arluca"/>

# <a name="using-azure-ad-connect-health-with-ad-ds"></a>Kasutades Azure AD kasutajaga AD DS seisund
Järgmised dokumendid on Active Directory domeeniteenused Azure'i AD-ühenduse seisundi jälgimine. Toetatud versioonide AD DS-is on: Windows Server 2008 R2, Windows Server 2012 ja Windows Server 2012 R2.

Jälgimine Azure'i AD-ühenduse tervist AD FS-i kohta leiate lisateavet teemast [AD FS abil Azure'i AD-ühenduse seisund](active-directory-aadconnect-health-adfs.md). Lisaks jälgimine Azure'i AD-ühenduse (Sünkrooni) koos Azure'i AD-ühenduse seisundi kohta teavet vt [sünkroonimise kasutamine Azure'i AD-ühenduse seisund](active-directory-aadconnect-health-sync.md).

![Azure'i AD-ühenduse seisund AD DS](./media/active-directory-aadconnect-health/aadconnect-health-adds-entry.png)

## <a name="alerts-for-azure-ad-connect-health-for-ad-ds"></a>Teatiste Azure AD ühenduse seisund AD DS
Teatiste jaotises jooksul Azure'i AD-ühenduse seisund AD DS, pakub teile loendi seotud oma domeenikontrollerid active ja lahendatud teatised. Valige aktiivne või lahendatud teatise avab uue blade lisateabe koos eraldusvõime juhiseid, ja linke täiendavad dokumendid. Iga teatis võib olla üks või mitu eksemplari, mis vastavad iga selle kindla teatise poolt mõjutatud domeenikontrollerid. Teatiste tera allosas topeltklõpsake mõne probleemse domeenikontrolleri avamiseks on Lisalõiketera teatis juhul rohkem üksikasju.

See tera, saate lubada meiliteatised kohta teatiste saamiseks ning kuvada ajavahemiku muutmine. Selle ajavahemiku laiendamine võimaldab näha eelneva lahendatud teatised.

![Azure'i AD-ühendus sünkroonimise tõrge](./media/active-directory-aadconnect-health/aadconnect-health-adds-alerts.png)

## <a name="domain-controllers-dashboard"></a>Domeeni kontrollerid armatuurlaud
Armatuurlaua pakub teie keskkonnas koos olulisi funktsionaalseid mõõdikute ja iga hulka kuuluvad teie jälgitud domeeni seisund oleku topoloogiline vaadet. Esitatud mõõdikute aitab kiiresti tuvastada, mis tahes domeenikontrollerid, mis võivad nõuda uurimine. Vaikimisi kuvatakse ainult alamkomplekt veerud. Siiski leiate terve kogumi saadaolevad veerud, topeltklõpsates käsk veerud. Teie jaoks kõige, lülitab armatuurlaua ühte ja lihtne paigutada AD DS keskkonna seisundi kuvamiseks veergude valimine.

![Domeenikontrollerid](./media/active-directory-aadconnect-health/aadconnect-health-adds-domainsandsites-dashboard.png)

Domeenikontrollerid Rühmitusalus nende vastavas või saidil, mis on abiks keskkonna topoloogia mõistmine. Lõpuks, kui topeltklõpsate blade päis, armatuurlaud maksimeerib kasutada saadaval ekraani kinnisvara. Selle kuvamiseks suuremana on kasulik, kui kuvatakse mitu veergu.

## <a name="replication-status-dashboard"></a>Dispersioonanalüüs seisundi armatuurlaud
Armatuurlaua pakub vaate kopeerimise olek ja dispersioonanalüüs topoloogia hulka kuuluvad teie jälgitud domeeni. Viimati tehtud dispersioonanalüüs proovi olekut on loetletud koos vigu, mis on leitud kasulik dokumentatsioonist. Topeltklõpsake avamiseks uue blade teabega nagu domeenikontrolleri viga,: soovitatav lahendus ja lingid tõrkeotsingu dokumentatsiooni tõrke üksikasjade.

![Kopeerimise olek](./media/active-directory-aadconnect-health/aadconnect-health-adds-replication.png)

## <a name="monitoring"></a>Jälgimine
See funktsioon pakub graafilise trende erinevatele hinnale, mis pidevalt iga selle hulka kuuluvad jälgida domeeni. Domeenikontrolleri saate hõlpsalt tootluse domeenikontrollerid kõik muud jälgida oma mets. Lisaks saate vaadata erinevate jõudluse hinnale kõrval, mis on kasulik, kui teie keskkonnas probleemide tõrkeotsing.

![Jälgimine](./media/active-directory-aadconnect-health/aadconnect-health-adds-monitoring.png)

Vaikimisi on meil juba neli jõudluse hinnale; Siiski saate lisada teistele, klõpsates käsk Filtreeri ja valides või mis tahes soovitud jõudluse hinnale tühistamine. Lisaks saate topeltklõpsake jõudluse counter graafiku avada uus tera, mis sisaldab andmepunkti iga selle hulka kuuluvad jälgida domeeni.

## <a name="related-links"></a>Seotud lingid

* [Azure'i AD-ühenduse seisund](active-directory-aadconnect-health.md)
* [Azure'i AD-ühenduse seisund Agent installimine](active-directory-aadconnect-health-agent-install.md)
* [Azure'i AD-ühenduse seisund toimingud](active-directory-aadconnect-health-operations.md)
* [Kasutades Azure AD kasutajaga AD FS-i seisund](active-directory-aadconnect-health-adfs.md)
* [Azure'i AD-ühenduse seisundi kasutamise sünkroonimine](active-directory-aadconnect-health-sync.md)
* [Azure'i AD-ühenduse seisund KKK](active-directory-aadconnect-health-faq.md)
* [Azure'i AD-ühenduse seisund versiooniajalugu](active-directory-aadconnect-health-version-history.md)
