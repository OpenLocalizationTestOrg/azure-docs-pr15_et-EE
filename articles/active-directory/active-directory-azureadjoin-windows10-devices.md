<properties
    pageTitle="Windows 10 seadmes kasutamine oma töökoha | Microsoft Azure'i"
    description="Pakub võimalusi hetktõmmise kasutajatele ja seda kontrastne erineval viisil seadme saab ette valmistatud ja kasutada ettevõttes on Windows 10."
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="femila"/>

# <a name="using-windows-10-devices-in-your-workplace"></a>Windows 10 seadmes oma töökoha kasutamine

Kehtib: Windows 10 PC-arvutisse

Windows 10 pakub kolme mudelid ettevõtted, mis võimaldab kasutajatel töö ressursse turvaline ja mugav viis.

- **Azure Active Directory liitumine** (Azure'i AD-ühendus), töötajatele, kes on juurdepääs Office 365 peamiselt pilves nagu. Azure'i AD-ühendus on Iseteeninduslik töö ettevalmistamise kogemus, mis on uut rakenduses Windows 10.
- **Domeeni Liitu**, ettevõtted, mis on kohapealse rakendused ja ressursse. Domeeni Liitu pakub Windows 10 kui ühendatud Azure AD parem kogemus.
- **Uus lihtsustatud BYOD komplekt**, kasutajatele, kes konto töö või kooli lisamiseks Windowsi ja hõlpsasti Accessi ressursse isikliku seadmetes.

Järgnevas tabelis on toodud võimalused kasutajate ja selle hetktõmmise administraatorid, kontrastne erineval viisil seadme saab ette valmistatud ja kasutada ettevõttes on Windows 10:

|                                                                                                                                                                 | Domeeni liitumine     | Azure'i AD-ühendus | Isiklik seade |
|-----------------------------------------------------------------------------------------------------------------------------------------------------------------|-----------------|---------------|-----------------|
| Windowsi seadmes logige sisse töökoha või õppeasutuse konto.                                                                                                                      | Jah             | Jah           | Ei              |
| Kasutaja ühekordse sisselogimise (SSO) Office 365 ja Azure AD rakendused. SSO-d on võimalus sisse logida ainult üks kord juurdepääsu ettevõtte ressursse. | Jah             | Jah           | Jah             |
| Kasutaja SSO Kerberos/NTLM rakendused.                                                                                                                                  | Jah             | Piiratud       | Jah, VPN-i kaudu         |
| Keeruka autoriseerimine ja mugav sisselogimine Microsoft Passporti ja Windows Hello töökoha või kooli kontoga.                                                                   | Jah             | Jah           | Jah             |
| Juurdepääs ettevõtte Windowsi poe töökoha või kooli kontoga (mitte Microsofti konto).                                                                                    | Jah             | Jah           | Jah             |
| Ettevõtte nõuetele vastavuse rändluse kasutaja sätted kõigis seadmetes töökoha või kooli kontoga.                                                                                 | Jah             | Jah           | Jah             |
| Võimalus ettevõtte rakendused ja seadmed, mis ühilduvad ettevõtte seadme poliitikate juurdepääsu piiramiseks.                                                      | Jah             | Jah           | Jah             |
| Kasutaja iseteenindusliku ettevalmistamise seadmete "igal pool töö."                                                                                                | Ei              | Jah           | Jah             |
| Võimalus seadmete haldamine.                                                                                                                                       | Jah, GP/SCCM kaudu | Jah           | Jah             |

## <a name="use-work-owned-devices-with-azure-ad-join-and-domain-join-in-windows-10"></a>Kasutage töö seadmetes Azure'i AD-ühendus ja domeeni liitumine Windows 10

Windows 10 pakub töö seadmetes juurdepääsu töö ressursid kaks võimalust:

- Azure'i AD-ühendus
- Domeeni liitumine

 Nii saab kehtivad suvandid sõltuvalt ettevõtte vajadustele ja nõuetele. Mõnel juhul võib ettevõtted kasu allpool on mõlema meetodi kasutamise lubamine.

## <a name="when-to-use-azure-active-directory-join"></a>Millal kasutada Azure Active Directory liitumine

Azure'i AD-ühendus on uus iseteenindusliku töö ettevalmistamise teenuse Windows 10.  See on suunatud töötajad, kes töö ressursid, nt Office 365 peamiselt pilves juurde. See on kerge viis konfigureerida arvutite, tahvelarvutite ja ettevõtte telefonid. Mobiilsideseadmete haldus, hallatakse ühtsete juhtelementide abil Windowsi keskkondades seadmed.

**Kasutage Azure AD liitumine mõni järgmistest põhjustest**:

- Soovite lubada selle iseteenindusliku ettevalmistamise seadmete töötajate liikvel olles.
- Saate anda kasutajatele töö kuulub mobiilseadmete nagu tahvelarvutitesse.
- Soovite hallata kasutajate kogumi Azure AD asemel Active Directory, näiteks hooajaliste töötajate, alltöövõtjate või õppuritele.
- Soovite anda liitumist võimaluste remote harukontorite töötajate piiratud kohapealse taristu.
- Teil pole kohapealse Active Directory.

Mõni ettevõte kasutab Azure AD liitumine esmane viis juurutamise töö seadmetes, eriti siis, kui nad migreerida enamik või kõik oma ressursid pilve. Hübriidjuurutuse ettevõtted, kus nii Active Directory ja Azure AD võite ka juurutamiseks meetod ühe või teise kasutaja või osakonna sõltuvalt.

Kooli alasid ja kõrgkoolide, näiteks võib hallata Active Directory töötajate ja õppurite Azure AD. Mõnes ettevõttes võib soovite hallata remote büroode või müügi osakondade Azure AD. Azure'i AD-ühendus nii domeeni Liitu meetodite saab hübriid organisatsiooni sees. Azure'i AD-ühendus võib olla suur täiendus domeeni liituda seadmed töökeskkonnas juurutamine.

**Kui kõige ettevõtteressursid on pilves, kaudu oma ettevõttes võib nautida täiendavaid eeliseid, kui**:

- Saate eemaldada sõltuvuse kohapealse identiteedi taristu.
- Saate lihtsustada mudelisse seadmed juurutamise saada eemale kujutise lahenduste iseteenindusliku konfiguratsiooni lubades.
- Mobiilsideseadmete haldus abil saate hallata eri kõigis oma seadmetes.

Azure'i AD liitumise kohta leiate lisateavet teemast [laiendamine pilve võimalusi Windows 10 seadmete kaudu Azure Active Directory liituda](active-directory-azureadjoin-overview.md).

## <a name="when-to-use-domain-join-or-keep-using-it"></a>Millal kasutada domeeni Liitu (või Hoia seda kasutama)

15 aasta jooksul kasutanud paljud organisatsioonid domeeni Liitu töö seadmete ühendamiseks. See võimaldab kasutajatel logige sisse oma Active Directory tööd oma seadmed- või koolikonto. Domeeni Liitu võimaldab seda hallata ühes keskses kohas ja täielikult järgmistesse seadmetesse. Ettevõtted tavaliselt toetuvad kujutise võimalusi sätte seadmed ja sageli kasutada neid hallata System Center Configuration Manager (SCCM) või rühma poliitika (GP).

**Teie ettevõtte tuleks kasutada domeeni Liitu (või Hoia seda kasutama) mõni järgmistest põhjustest**:

- Teil on juurutatud need seadmed, mis kasutavad NTLM/Kerberos Win32 rakendused.
- Vajate GP või SCCM/DCM seadmete haldamine.
- Soovite jätkata kujutise lahenduste abil saate konfigureerida seadmete töötajatele.

**Domeeni Liitu Windows 10 annab teile kui ühendatud Azure AD järgmised eelised**:

- Keeruka seadme seotud autentimis- ja mugav sisselogimine Microsoft Passporti ja Windows Hello töökoha või kooli kontoga.
- Ettevõtte Windowsi poe juurdepääsu seadmed, mida kasutada töö- või koolikonto (Microsofti konto pole nõutav).
- Ettevõtte nõuetele vastavuse rändluse kasutaja sätted kõigis seadmetes, mis kasutavad töökoha või kooli konto (Microsofti konto pole nõutav).
- Võimalus ettevõtte rakendused ja seadmed, mis ühilduvad ettevõtte seadme poliitikate juurdepääsu piiramiseks.

Azure'i AD liitumise kohta leiate lisateavet teemast [laiendamine pilve võimalusi Windows 10 seadmete kaudu Azure Active Directory liituda](active-directory-azureadjoin-overview.md).

## <a name="enable-joining-of-personally-owned-devices-for-work-or-school"></a>Luba liitumise isiklikult seadmetes tööl või koolis.

Windows 10 annab kasutajale BYOD toetamiseks ettevõttes võimalus lisada oma arvutis, tahvelarvutis või telefonis töökoha või kooli kontoga. Pärast kasutaja lisab töökoha või kooli kontoga, seade on registreeritud Azure AD ja soovi korral registreeritud mobiilsideseadme halduse süsteemis asutuses on konfigureeritud. Kataloogi jäänud järgmistesse seadmetesse nimega 'Registreeritud' vs "Azure AD liitunud". IT administraors saate rakendada selle teabe, pakkudes veidi heledam seadmetes isiklikult kuulub kui töö seadmetes, kui soovitud põhjal erinevad reeglid.

Kasutajad saavad lisada töö või kooli konto abil oma isiklikku seadet väga mugavalt. Nad saavad teha tööd rakenduse esmakordsel kasutamisel või nad saavad teha seda käsitsi menüü Sätted kaudu. Selle konto annab SSO ettevõtte ressursid.

Azure'i AD liitumise kohta leiate lisateavet teemast [ühenduse loomine domeeni ühendatud seadmete Azure AD Windows 10 jaoks kogemusi](active-directory-azureadjoin-devices-group-policy.md).

## <a name="enable-domain-join-or-azure-ad-join"></a>Luba domeeni liitumine või Azure'i AD-ühendus

Kõik meetodid eespool kirjeldatud (domeeni Liitu Azure'i AD-ühendus ning lisa töö- või koolikontoga) on sissepääsud kasutusvõimalused Windows 10. Siiski kõik nõuavad IT-administraator infrastruktuuri funktsiooni lubamiseks enne kogemusi ei tööta.

## <a name="requirements-for-deploying-azure-ad-join"></a>Nõuded juurutamine Azure AD liitumine

Juurutada Azure AD liitumine mis tahes hulga kasutajate jaoks vajate järgmist:

- Tellimuse Azure AD.
- Azure'i AD Premiumi tellimus, nt mobiilsideseadme halduse automaatseks registreerimiseks, kui vajate rohkem võimalusi.
- Mobiilsideseadmete haldus – näiteks Microsoft Intune'i tellimus, mobiilsideseadmete haldus Office 365 või mis tahes partneri mobiilsideseadme halduse tarnijatele integreerimine Azure AD. (Vt [jaotisest KKK](#frequently-asked-questions) lisateavet käesoleva artikli lõpus).

Kui teie on hübriid, me soovitatav juurutada Azure'i AD-ühenduse Azure AD kohapealse kataloogi laiendada.

## <a name="requirements-for-using-domain-join-with-azure-ad"></a>Azure AD kaudu liitumine domeeni kasutamise nõuded

Töö tagamiseks on alati jätkuvalt domeeni Liitu. Siiski Azure AD kasu saada on vaja järgmist:

- Tellimuse Azure AD.
- Azure'i AD-ühenduse laiendada kohapealse kataloogi Azure AD kasutuselevõtt.
- Poliitika, mis lubab domeeni ühendatud seadmete tingimusvormingu juurdepääsu Azure AD.
- Poliitika, mis lubab juurdepääsu seadmed "domeeni ühendatud", kui soovite piirata juurdepääsu mõne seadme jaoks.
- System Center Configuration Manager versioon 1509 tehnilise eelvaate lubada reeglid nõudva nõuetele vastavuse seadmete jaoks. (Vt TechNeti dokumentatsiooni ja ajaveebipostitusest).

Domeeni liitumine Windows 10 kohta leiate lisateavet teemast [ühenduse loomine domeeni ühendatud seadmete Azure AD Windows 10 jaoks kogemusi](active-directory-azureadjoin-devices-group-policy.md)

## <a name="requirements-for-using-byod-and-add-a-work-or-school-account"></a>BYOD ja "Töökoha või kooli konto lisamine" kasutamise nõuded

Lubada "tuua oma seadme" (BYOD) töökoha või kooli kontoga, on teil vaja järgmist:

- Tellimuse Azure AD.
- Azure'i AD Premiumi tellimus, nt mobiilsideseadme halduse automaatseks registreerimiseks, kui vajate rohkem võimalusi.

## <a name="requirements-for-using-microsoft-passport"></a>Microsoft Passporti kasutamise nõuded

Microsoft Passporti lubamiseks peate järgmist:

- Avaliku võtme infrastruktuuri (PKI) serdi autentimise tugiteenuste, mis kasutab Microsoft Passporti.
- Intune tellimuse serdi autentimise tugiteenuste, mis kasutab Microsoft Azure AD liitumine Passport ja töö-või koolikonto.
- System Center Configuration Manager versioon 1509 Technical Preview (vt TechNeti dokumentatsiooni ja ajaveebipostitusest) serdi autentimise tugiteenuste, mis kasutab Microsoft Passporti domeeni Liitu.
- Microsoft Passporti ettevõte, mis võimaldab poliitika.

PKI abil teise võimalusena saate lubada põhinev Microsoft Passporti, tehes järgmist:

- Juurutada Windows Server 2016 "Tootmise eelvaade 1" DCs (ei ole vaja domeeni või mets otstarbekas tasemed; mitme DCs koondamise kus pakutakse piisab iga Active Directory saidi jaoks).
- Microsoft Passporti lubamiseks organisatsiooni poliitika määramine.

Microsoft Passporti ja Windows Hello Windows 10 kohta leiate lisateavet teemast < link-to-MS-Passport-and-Windows-Hello-document >.

## <a name="frequently-asked-questions"></a>Korduma kippuvad küsimused
### <a name="which-partner-mobile-device-management-products-integrate-with-azure-ad"></a>Milliste toodete partneri mobiilsideseadme halduse integreerimine Azure AD?

Järgmiste hankija toodete integreerimine Azure AD ühendatud registreerumise ja juurdepääsu Windows 10 jaoks:

- Ofort VMware järgi
- Citrix Xenmobile
- Lightspeed Mobile haldur
- SOTI kohapealse mobiilsideseadmete haldus
- MobileIron

### <a name="what-about-workplace-join-in-windows-10"></a>Kuidas on lood töökoha liitumine Windows 10?
Windows 8.1 kasutati lubamiseks BYOD töökoha Liitu. Opsüsteemis Windows 10 BYOD on lubatud "Lisada töö või kooli kontot" kaudu, nagu on selgitatud selle dokumendi. Ettevõtted, mis pole integreerimine Azure AD oma mobiilsideseadmete haldus, kasutajate saate registreeruda seadet arvesse juhtimise käsitsi **sätete**kaudu > **kontod** > **töö juurdepääsu**.


### <a name="can-users-connect-their-microsoft-account-to-their-domain-account-in-windows-10"></a>Saate kasutajad ühendada oma Microsofti kontoga oma domeeni kontoga Windows 10?
Pole opsüsteemis Windows 10. Windows 8.1, kasutaja domeeni-ühendatud seadmete "ühendust" oma Microsofti kontoga (nt Hotmail, Live, Outlook, Xbox jne) oma domeeni kontoga teatud funktsioonid nagu SSO reaalajas teenustega, kasutamine Windowsi poest ja kõigis seadmetes, kasutajasätete rändlusteenuse lubamiseks. Windows 10, ei ole enam Microsofti konto "ühendust" funktsioone. Kasutaja saab lisada ühe või mitme Microsofti konto nimega täiendavate kontode lubamiseks SSO tarbija teenustele nagu Windowsi poest. See on tehtud **sätted** > **kontod** > **kontole**.

Kasutajad, kes on üleminekut opsüsteemilt Windows 8.1 domeeni ühendatud seadmete ja kellel oli oma Microsofti kontoga ühendatud, automaatselt on oma ühendatud Microsofti kontoga, lisatakse nad kasutavad täiendavate kontode loend.


## <a name="additional-information"></a>Lisateave
* [Windows 10 ettevõtte: seadmete jaoks töö kasutamine](active-directory-azureadjoin-windows10-devices-overview.md)
* [Pilveteenuse võimaluste ja Windows 10 seadmed kaudu Azure Active Directory liitumine laiendamine](active-directory-azureadjoin-user-upgrade.md)
* [Azure'i AD liitumise teave kasutamise stsenaariumid](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [Ühenduse loomine Windows 10 kogemusi Azure AD domeeni ühendatud seadmete](active-directory-azureadjoin-devices-group-policy.md)
* [Azure'i AD liitumine häälestamine](active-directory-azureadjoin-setup.md)
