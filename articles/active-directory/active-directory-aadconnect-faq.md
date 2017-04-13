<properties
    pageTitle="Azure'i AD-ühenduse: KKK | Microsoft Azure'i"
    description="See leht sisaldab korduma kippuvad küsimused Azure'i AD-ühenduse."
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="azure-ad-connect-faq"></a>Azure'i AD-ühenduse KKK

## <a name="general-installation"></a>Install üldist
**Q: kas installi töötavad, kui Azure AD globaalne administraator on lubatud 2FA?**  
Järgud veebruar 2016, kus see on toetatud.

**Q: Kas on võimalik installida Azure AD-ühenduse järelevalveta?**  
See on toetatud ainult installige Azure'i AD-ühenduse installimise viisardi abil. Järelevalveta ja vaikne installi ei toetata.

**K: Mul on mets, kus ühe domeeni ei saa ühendust võtta. Kuidas installida Azure AD-ühenduse?**  
Järgud: veebruar 2016, kus see on toetatud.

**Q: kas AD DS seisund agent töötab server core?**  
Jah. Pärast installimist agent, saavad teostada registreerimise käigus, kasutades järgmist PowerShelli abil: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

## <a name="network"></a>Võrgu
**Q: Mul on tulemüür, võrgu seade või midagi muud, mis piirab ülempiir kellaaeg ühendused saate avatuks minu võrgus. Kui kaua minu kliendi küljel ajalõpp lävi tuleks Azure'i AD-ühenduse kasutamise korral?**  
Kõigi võrgu tarkvara, füüsilise seadmete või midagi muud, mis piirab ühendused avatuks maksimaalne aeg peaksite järgima Ühenduvus server, kuhu on installitud Azure'i AD-ühenduse klient ja Azure Active Directory lävi vähemalt 5 minutit (300 sekundi). See kehtib ka kõigi varem välja Microsofti Identity sünkroonimise tööriistad.

**Q: kas SLDs (ühe sildi domeenid) toetatud?**  
Ei, Azure'i AD-ühenduse ei toeta kohapealse metsade/domeenide kasutamine SLDs.

**Q: on "punktiir" nimega NetBios toetatud?**  
Ei, Azure'i AD-ühenduse ei toeta kohapealse metsade/domeenid, kus NetBios nimi sisaldab perioodi "." nimi.

## <a name="federation"></a>Federation
**K: mida teha, kui ma, saavad meili, mis palub mul uuendada oma Office 365 serdi**  
Kasutage juhiseid, mis on välja toodud [pikendamine sertifikaatide](active-directory-aadconnect-o365-certs.md) kohta, kuidas pikendada serdi teema.

**K: Mul on "Automaatselt värskendada tuginedes tootja" O365 tuginedes peole. Kas mul on vastu võtta, kui minu luba allkirjasert automaatselt koondab?**  
Kasutage juhiseid, mida on kirjeldatud artiklis [pikendada serdid](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Keskkonnas
**Q: on see toetatud ümber nimetada server, kui arvutisse on installitud Azure'i AD-ühenduse?**  
Ei. Serveri nime muutmine põhjustab sync engine ei saa SQL-andmebaasiga ühenduse loomiseks ja teenus ei saa alustada.

## <a name="identity-data"></a>Identiteedi andmed
**Q: UPN-i (userPrincipalName) Azure AD atribuut ei sobi kohapeal UPN - miks?**  
Leiate järgmistest artiklitest:

- [Office 365, Azure või Intune kasutajanimed ei ühti kohapealse UPN-i või alternatiivse sisselogimise ID](https://support.microsoft.com/en-us/kb/2523192)
- [Muudatused ei ole sünkroonitud Azure Active Directory sünkroonimise tööriista pärast muutmist UPN-i kasutajakonto ühendatud muu domeeni kasutamine](https://support.microsoft.com/en-us/kb/2669550)

Samuti saate konfigureerida Azure AD lubamiseks sync engine soovitud userPrincipalName värskendada, nagu on kirjeldatud [Azure'i AD-ühenduse sünkroonimise teenuse funktsioonid](active-directory-aadconnectsyncservice-features.md).

## <a name="custom-configuration"></a>Kohandatud konfiguratsioon
**Q: kus asuvad PowerShelli cmdlet-käskude Azure'i AD-ühenduse dokumenteerida?**  
Cmdlet-käskude dokumenteerida sellel saidil, välja arvatud ei toetata PowerShelli cmdletid leitud Azure'i AD-ühenduse klientidele kasutamiseks.

* *K: kasutada "Serveri ekspordi/serveri impordi" leitud *sünkroonimise Service Manager* konfiguratsiooni liikuda servers? **  
Ei. See suvand on tuua kõik konfiguratsioonisätted ja ei tohi kasutada. Selle asemel peaks viisardi abil saate luua base konfiguratsiooni teise serveris ja sünkroonimine reegel redaktori abil saate luua PowerShelli skripti, mis tahes kohandatud reegli nihutamiseks meiliserverite vahel. Lugege teemat [kohandatud Otsingukonfiguratsiooni aktiivne lavastus serveri teisaldamine](active-directory-aadconnect-upgrade-previous-version.md#move-custom-configuration-from-active-to-staging-server).

## <a name="troubleshooting"></a>Tõrkeotsing
**Kuidas saan Azure'i AD-ühenduse abi?**

[Microsofti teabebaasi (KB) otsimine](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

- Otsige Microsofti teabebaasi (KB) tehnilisi lahendusi levinud probleemide kohta tugi Azure'i AD-ühenduse.

[Microsoft Azure Active Directory Foorumid](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

- Saate otsida ja liikuge tehniliste küsimused ja vastused ühenduse või esitage oma küsimus, klõpsates [siin](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Azure'i AD-ühendus klienditugi](https://manage.windowsazure.com/?getsupport=true)

- Kasutage seda linki saada tugi Azure portaali kaudu.
