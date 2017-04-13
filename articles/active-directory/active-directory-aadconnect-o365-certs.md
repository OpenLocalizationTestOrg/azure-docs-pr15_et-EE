<properties
    pageTitle="Serdi uuendamise juhised Office 365 ja Azure Active Directory kasutajad | Microsoft Azure'i"
    description="Selles artiklis selgitatakse Office 365 kasutajate e-kirju, mis teavitab neid sertifikaadi uuendamine probleemide lahendamiseks."
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


# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>Office 365 ja Azure Active Directory federation serdid uuendamine

##<a name="overview"></a>Ülevaade

Eduka ühinemise Azure Active Directory (Azure AD) ja Active Directory Federation Services (AD FS) vahel, jaoks kasutatavaid sisselogimise turvalisus sõned Azure AD AD FS-i serdid peaksid olema samad, mis on konfigureeritud Azure AD. Mis tahes lahknevuse võib kaasa tuua katkenud usalda. Azure AD tagab, et see teave on sünkroonitud AD FS-i ja Web rakenduse puhverserveri juurutamisel (Suhtevõrgu) jaoks.

See artikkel annab teile täiendavat teavet hallata oma luba sisselogimise serdid ja hoida neid sünkroonis Azure AD, järgmistel juhtudel:

* Juurutate teenuserakenduse puhverserverit, ja seetõttu ei ole saadaval on Suhtevõrgu federation metaandmete.
* Te ei kasuta vaikimisi konfiguratsiooni AD FS-i luba sisselogimise serdid.
* Kasutate kolmanda osapoole identiteedipakkuja.

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>Vaikimisi konfigureerimine AD FS-i luba sisselogimise serdid

Turbeloa sisselogimine ja luba dekrüptimine serdid on tavaliselt iseallkirjastatud serdid ja on üks aasta. Vaikimisi sisaldab AD FS-i automaatse uuendamise protsessi, mis nimetatakse **AutoCertificateRollover**. Kui kasutate AD FS 2.0 või uuemat versiooni, Office 365 ja Azure AD automaatselt värskendada oma serdi enne selle lõppemist.

### <a name="renewal-notification-from-the-office-365-portal-or-an-email"></a>Office 365 portaalis või e-posti uuendamise teatis

>[AZURE.NOTE] Kui olete saanud e-posti või portaali teate, mis palub teil uuendada oma serdi Office, vt [haldamine muutub luba sisselogimise serdid](#managecerts) kontrollige kui teil on vaja midagi teha. Microsoft on teadlik võimalikke probleeme, mis võib kaasa tuua teatised saadetakse ka siis, kui midagi on vaja serdi pikendada.

Azure AD püüab federation metaandmete jälgimine ja luba sisselogimise serdid tähistatud selle metaandmete värskendamine. 30 päeva enne luba sisselogimise serdid, Azure AD kontrollib, kas uus serdid on saadaval, küsitlused federation metaandmete.

* Kui see edukalt küsitlus federation metaandmete ja tuua uue serdid, pole meiliteatise või Office 365 portaalis hoiatus antakse kasutajale.
* Kui seda ei saa tuua uue luba sisselogimise serdid, kas Kuna federation metaandmete pole kättesaadav või automaatne serdi edastamiseks pole lubatud, Azure AD ühenduvusprobleemide meiliteatise ja Office 365 portaalis hoiatus.

![Office 365 videoportaali teatis](./media/active-directory-aadconnect-o365-certs/notification.png)

>[AZURE.IMPORTANT] Kasutate AD FS-i järjepidevuse tagamiseks, veenduge, et teie serverid on järgmised värskendused, mis ei esine autentimine vead teadaolevate probleemide. See leevendab teadaolevad AD FS-i puhverserveri server Probleemid selle pikendamise ja tulevaste pikendamise perioodi:
>
>Server 2012 R2 - [Windows Server mai 2014 rollup](http://support.microsoft.com/kb/2955164)
>
>Server 2008 R2 ja 2012 – [läbi puhverserveri autentimine nurjub Windows 2008 R2 SP1 või Windows Server 2012](http://support.microsoft.com/kb/3094446)

## Märkige ruut, kui serdid on vaja uuendada<a name="managecerts"></a>

### <a name="step-1-check-the-autocertificaterollover-state"></a>Samm 1: Märkige ruut AutoCertificateRollover olek

Avage oma AD FS server, PowerShelli. Kontrollige, kas AutoCertificateRollover väärtuseks on seatud väärtusele tõene.

    Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

[AZURE.NOTE] Kui kasutate AD FS 2.0, käivitage esmalt lisa-Pssnapin Microsoft.Adfs.Powershell.

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>Samm 2: Veenduge, et AD FS-i ja Azure AD on sünkroonitud

Teie AD FS server, avage Azure AD PowerShelli käsuviibas ja Azure AD ühenduse.

>[AZURE.NOTE] Saate alla laadida Azure AD PowerShelli [siin](https://technet.microsoft.com/library/jj151815.aspx).

    Connect-MsolService

Märkige ruut serdid konfigureeritud AD FS-i ja Azure AD atribuutide määratud domeeni usaldusväärseks.

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

Kui rakenduses Mõlemad väljundid thumbprints vasta, teie serdid on sünkroonitud Azure AD.

### <a name="step-3-check-if-your-certificate-is-about-to-expire"></a>Samm 3: Kontrollige, kas teie sert on aegumas

Get-MsolFederationProperty või Get-AdfsCertificate väljund, märkige ruut kuupäeva jaotises "Pole pärast." Kui kuupäev on vähem kui 30 päeva, võtke toiming.

| AutoCertificateRollover | Sertide Azure AD sünkroonis | Metaandmete Domeeniliit on avalikult juurdepääsetava | Kehtivust | Toiming |
|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|:-----------------------:|
| Jah | Jah | Jah | - | Teil pole vaja midagi. Lugege teemat [pikenda luba allkirjasert automaatselt](#autorenew). |
| Jah | Ei  | - | Vähem kui 15 päeva | Pikendada kohe. Lugege teemat [pikenda luba allkirjasert käsitsi](#manualrenew). |
| Ei | - | - | Vähem kui 30 päeva | Pikendada kohe. Lugege teemat [pikenda luba allkirjasert käsitsi](#manualrenew). |

\[Ükskõik -]

## Luba allkirjasert automaatne pikendamine (soovitatav)<a name="autorenew"></a>

Te ei pea mis tahes käsitsi juhiste täitmiseks, kui mõlemad järgmised tingimused on täidetud:
- Rakenduse puhverserver, mille saate lubada juurdepääsu federation metaandmete kaudu soovitud Suhtevõrgu juurutanud.
- Kui kasutate AD FS-i vaikimisi konfiguratsiooni (AutoCertificateRollover lubatud).

Kontrollige järgmist kinnitamiseks serdi automaatselt värskendada.

**1. AD FS-i atribuut AutoCertificateRollover peab olema seatud tõene.** See näitab, et AD FS-i loob automaatselt uue Turbeloa sisselogimine ja Turbeloa dekrüptimine serdid, enne vana need aeguvad.

**2. AD FS liiduserveri metaandmete on avalikult kättesaadav.** Kontrollige oma federation metaandmete on avalikult juurdepääsetava järgmine URL navigeerides avaliku Interneti-ühendusega arvutid (ettevõtte võrku) eemaldada arvutist.


https:// (your_FS_name) /federationmetadata/2007-06/federationmetadata.xml

kus `(your_FS_name) `on asendatud funktsiooniga teie ettevõte kasutab, nt fs.contoso.com federation teenuse hosti nimi.  Kui teil on võimalik nii, et külastajad neid sätteid edukalt, teil pole midagi tegema.  

Näide: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## Allkirjasert käsitsi luba pikendamine<a name="manualrenew"></a>

Kui valite pikendamiseks luba sisselogimise serdid käsitsi. Näiteks järgmistel juhtudel töötaks paremini käsitsi pikendada:
* Turbeloa sisselogimise serdid on pole iseallkirjastatud serdid. On kõige levinum põhjus on selles, et teie asutus haldab registreeritud ettevõtte sert asutuse AD FS-i serdid.
* Võrgu turvalisus ei võimalda federation metaandmete avalikuks.

Need stsenaariumid, iga kord, kui värskendate luba sisselogimise serdid, peate ka värskendama oma Office 365 domeeni PowerShelli käsu, värskendus-MsolFederatedDomain abil.

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>Samm 1: Veenduge, et AD FS-i on uus luba sisselogimise serdid

**Mitte-vaikimisi konfigureerimine**

Kui kasutate AD FS-i (kus **AutoCertificateRollover** on seatud väärtusele **väär**), mitte vaikimisi konfiguratsiooni, kasutate ilmselt kohandatud serdid (pole iseallkirjastatud). Sisselogimise serdid AD FS-i luba pikendamise kohta leiate lisateavet teemast [juhised klientidele, kes ei kasuta AD FS-i iseallkirjastatud serdid](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert).

**Federation metaandmete ei ole avalikult kättesaadav**

Teisalt, kui **AutoCertificateRollover** on seatud **tõene**, kuid ei ole oma federation metaandmete avalikult kättesaadav, esmalt veenduge, et AD FS-i on loodud uue Turbeloa allkirjastamiseks serdid. Veenduge, et teil on uus luba sisselogimise serdid järgmiste toimingute abil:

1. Veenduge, et olete sisse logitud esmane AD FS server.
2. Märkige ruut praeguse allkirjastamiseks serdid AD FS-i, avades PowerShelli käsuviiba aken ja käivitage järgmine käsk:

    PS C:\>Get-ADFSCertificate – CertificateType luba sisselogimise

    >[AZURE.NOTE] Kui kasutate AD FS 2.0, tuleks esmalt lisa-Pssnapin Microsoft.Adfs.Powershell käivitada.

3. Vaadata käsu väljund veebisaidil mis tahes loetletud serdid. Kui AD FS-i on loodud uue serdi, kuvatakse kaks serdid väljund: üks, mille **IsPrimary** väärtus on **True** ja **NotAfter** kuupäev on 5 päeva jooksul ja ühte, mille **IsPrimary** on **False** ja **NotAfter** on umbes aasta tulevikus.

4. Kui teile kuvatakse ainult üks sert ja **NotAfter** kuupäev on 5 päeva jooksul, peate luua uut serti.

5. Uue serdi loomiseks käivitada PowerShelli käsuviibas järgmine käsk: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.

6. Värskenduse kinnitamiseks uuesti käivitage järgmine käsk: PS C:\>Get-ADFSCertificate – CertificateType luba sisselogimise

Nüüd loetletakse kaks serdid, millest üks on üks aasta tulevikus **NotAfter** kuupäeva ja mille **IsPrimary** väärtus on **False**.

### <a name="step-2-update-the-new-token-signing-certificates-for-the-office-365-trust"></a>Samm 2: Värskendada uue luba sisselogimise serdid usalda Office 365

Värskendage Office 365 uus luba sisselogimise serdid kasutatakse usalda, järgmiselt.

1.  Avage Microsoft Azure Active Directory moodul Windows PowerShelli jaoks.
2.  Käivitage $cred = Get-mandaati. Selle cmdlet-käsu küsib teilt mandaate, tippige oma pilvepõhise teenuse administraatori konto identimisteave.
3.  Käivitage ühenduse MsolService – mandaati $cred. Selle cmdlet-käsu ühendub pilveteenusesse. Tausta, mis ühendub pilveteenusesse loomiseks tuleb enne kasutusel installitud tööriist täiendavad cmdlet-käsud.
4.  Kui teil on need käsud pole AD FS liiduserveri esmane arvutis, käivitage Set-MSOLAdfscontext-arvuti <AD FS primary server>, kus <AD FS primary server> on sisemine FQDN nimi esmane AD FS server. Selle cmdlet-käsu loob konteksti, mis ühendub AD FS-i.
5.  Käivitage Update-MSOLFederatedDomain – DomainName <domain>. Selle cmdlet-käsu kasutusele cloud: AD FS-i sätete värskendamist ja konfigureerib usalda seos kahe.


>[AZURE.NOTE] Kui teil on vaja toetab mitut üladomeenide, nt contoso.com ja fabrikam.com, kasutage parameetrit **SupportMultipleDomain** mis tahes cmdlet-käskude abil. Lisateabe saamiseks lugege teemat [mitu ülemine tase domeeni tugi](active-directory-aadconnect-multiple-domains.md).

## Azure AD usalduskeskuse abil Azure'i AD-ühenduse parandamine<a name="connectrenew"></a>

Kui olete konfigureerinud oma serveripargi AD FS-i ja Azure AD usalduskeskuse abil Azure'i AD-ühenduse, saate Azure'i AD-ühenduse tuvastamiseks, kui peate vastu võtta teie luba sisselogimise serdid. Uuendab vajadusel saate Azure'i AD-ühenduse seda teha.

Lisateavet leiate teemast [Usalda parandamine](./active-directory-aadconnect-federation-management.md#repairing-the-trust).
