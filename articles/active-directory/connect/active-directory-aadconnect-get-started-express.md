<properties
    pageTitle="Azure'i AD-ühendus: Alustamine kiire sätete abil | Microsoft Azure'i"
    description="Saate teada, kuidas alla laadida, installida ja käitada häälestusviisardi jaoks Azure'i AD-ühenduse."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/13/2016"
    ms.author="billmath"/>

# <a name="getting-started-with-azure-ad-connect-using-express-settings"></a>Alustamine: Azure'i AD-ühenduse kiire sätete abil
Azure'i AD-ühendus **Kiirsätteid** kasutatakse, kui teil on ühe – mets topoloogia ja autentimise [parooli sünkroonimine](../active-directory-aadconnectsync-implement-password-synchronization.md) . **Kiire sätted** on vaikimisi valitud ja kasutatakse enamasti juurutatud stsenaariumi. Teil on ainult mõne lühike hiireklõpsuga ära, et laiendada kohapealse kataloogi pilveteenusesse.

Enne alustamist installimist Azure'i AD-ühenduse, veenduge, et alla [laadida Azure'i AD-ühenduse](http://go.microsoft.com/fwlink/?LinkId=615771) ja täielik eelse nõutavad etappide [Azure'i AD-ühenduse: eeltingimuste ja riistvara](../active-directory-aadconnect-prerequisites.md).

Kui kiirsätteid ei vasta teie topoloogia, lugege teemat teiste stsenaariumide [seotud dokumendid](#related-documentation) .

## <a name="express-installation-of-azure-ad-connect"></a>Kiire Azure'i AD-ühenduse installimine
Näete järgmist toimingut jaotise [videod](#videos) .

1. Logige sisse administraatorina server soovite installida Azure AD-ühenduse. Tehke seda soovite olla sünkroonimine serveriga serveris.
2. Liikuge ja topeltklõpsake **AzureADConnect.msi**.
3. Tervituskuval, märkige ruut nõustute hulgilitsentsimise tingimustega ja klõpsake nuppu **Jätka**.  
4. Kuval Express sätted nuppu **Kasuta kiirsätteid**.  
![Tere tulemast Azure AD ühenduse loomine](./media/active-directory-aadconnect-get-started-express/express.png)
5. Loo ühendus Azure AD kuvale, sisestage oma Azure AD kasutajanime ja parooli üldadministraator. Klõpsake nuppu **edasi**.  
![Ühenduse loomine Azure AD](./media/active-directory-aadconnect-get-started-express/connectaad.png) kui kuvatakse tõrketeade ja ühenduvuse probleeme, siis lugege teemat [ühendusprobleemide tõrkeotsing](../active-directory-aadconnect-troubleshoot-connectivity.md).
6. AD DS Kuva ühenduse Sisestage ettevõtte administraatori konto kasutajanimi ja parool. Saate sisestada domeeni osa NetBios või FQDN-vormingus, st FABRIKAM\administrator või fabrikam.com\administrator. Klõpsake nuppu **edasi**.  
![Ühenduse loomine AD DS](./media/active-directory-aadconnect-get-started-express/connectad.png)
7. [**Azure AD sisselogimise konfigureerimine**](../active-directory-aadconnect-user-signin.md#azure-ad-sign-in-configuration) lehel kuvatakse ainult, kui te ei saanud lõpule [kontrollida oma domeenide](../active-directory-add-domain.md) [eeltingimused](../active-directory-aadconnect-prerequisites.md).
![Kinnitamata domeenid](./media/active-directory-aadconnect-get-started-express/unverifieddomain.png)  
Kui näete selle lehe, siis lugege läbi iga domeeni, mis on tähistatud **Ei lisata** ja **Kinnitamata**. Veenduge, et need domeenid, mida kasutate on kinnitatud Azure AD. Kui olete kontrollinud oma domeeni, klõpsake nuppu Värskenda sümbol.
8. Kuva konfigureerimiseks valmis, klõpsake nuppu **Installi**.
    - Soovi korral saate konfigureerida lehe valmis, valimise ruut **Käivita sünkroonimine kohe, kui konfigureerimine on lõpule jõudnud** . Kui soovite teha täiendavaid konfiguratsiooni, nt [filtreerimine](../active-directory-aadconnectsync-configure-filtering.md), tuleks valimise see ruut. Kui te valimise see suvand, viisard konfigureerib sünkroonimine, kuid jätab ajasti keelatud. See ei tööta, kuni lubate seda käsitsi taaskäivitamisel [installimise viisard](../active-directory-aadconnectsync-installation-wizard.md).
    - Kui teil on oma kohapealse Active Directory Exchange'i, siis teil ka suvandi [**Exchange'i hübriidjuurutuse**](https://technet.microsoft.com/library/jj200581.aspx)lubamiseks. Selle suvandi võite lubada näiteks siis, kui plaanite on nii pilves ja samal ajal kohapealse Exchange'i postkasti.
![Kas olete valmis Azure'i AD-ühenduse konfigureerimine](./media/active-directory-aadconnect-get-started-express/readytoconfigure.png)
9. Kui installimine on lõpule jõudnud, klõpsake käsku **välju**.
10. Kui installimine on lõpule jõudnud, logige ja uuesti sisse logida enne, kui kasutate sünkroonimise Service Manager või sünkroonimise redaktoris.

## <a name="videos"></a>Videod

Video kasutamise kiire installimise kohta, vaadake:

>[AZURE.VIDEO azure-active-directory-connect-express-settings]

## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui teil on Azure'i AD-ühenduse installitud saate [kontrollida installimine ja litsentside määramine](../active-directory-aadconnect-whats-next.md).

Lisateavet need funktsioonid, mis olid lubatud installi: [Automaatne versioonitäiendus](../active-directory-aadconnect-feature-automatic-upgrade.md), [kustutab vältida juhuslikku](../active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)ja [Azure'i AD-ühenduse seisund](../active-directory-aadconnect-health-sync.md).

Lugege lisateavet teemadest levinud: [ajasti ja kuidas sünkroonimise käivitamiseks](../active-directory-aadconnectsync-feature-scheduler.md).

Lugege lisateavet [oma kohapealse identiteedid Azure Active Directory integreerimine](../active-directory-aadconnect.md).

## <a name="related-documentation"></a>Seotud dokumendid

Teema: |  
--------- | ---------
Azure'i AD-ühendus ülevaade | [Teie kohapealse identiteetide integreerimine Azure Active Directory](../active-directory-aadconnect.md)
Installige, kasutades kohandatud sätteid | [Kohandatud installi Azure'i AD-ühenduse](active-directory-aadconnect-get-started-custom.md)
DirSync täiendamine | [Uuendada Azure AD sünkroonimistööriist (DirSync)](active-directory-aadconnect-dirsync-upgrade-get-started.md)
Installi kasutatud kontod | [Lisateavet leiate Azure'i AD-ühenduse kontod ja õigused](active-directory-aadconnect-accounts-permissions.md)
