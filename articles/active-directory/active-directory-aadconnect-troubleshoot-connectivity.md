<properties
    pageTitle="Azure'i AD-ühendus: Ühenduvuse tõrkeotsing | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse Azure'i AD-ühenduse ühendusprobleemide tõrkeotsing."
    services="active-directory"
    documentationCenter=""
    authors="andkjell"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/27/2016"
    ms.author="billmath"/>

# <a name="troubleshoot-connectivity-issues-with-azure-ad-connect"></a>Azure'i AD-ühenduse ühendusprobleemide tõrkeotsing
Selles artiklis selgitatakse, kuidas töötab Ühenduvus Azure'i AD-ühenduse ja Azure AD- ja ühenduvusprobleemide tõ. Järgmiste probleemide korral tõenäoliselt kõige puhverserveri keskkonnas näha.

## <a name="troubleshoot-connectivity-issues-in-the-installation-wizard"></a>Installiviisardis ühenduvuse probleemide tõrkeotsing
Azure'i AD-ühendus on kasutusel kaasaegne autentimine (abil ADAL Raamatukogu) autentimine. Installimise viisard ja sync engine proper jaoks on vaja machine.config olema õigesti konfigureeritud, kuna need on .NET rakenduste abil.

Selles artiklis näitame Fabrikam oma puhverserveri kaudu Azure'i AD-ühenduse loomise viisi. Puhverserveri nimega fabrikamproxy ja kasutab port 8080.

Kõigepealt on vaja veenduge, et [**fail "Machine.config"**](active-directory-aadconnect-prerequisites.md#connectivity) on õigesti konfigureeritud.  
![machineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/machineconfig.png)

>[AZURE.NOTE]
Mitte-Microsofti mõnes on dokumenteerida, et muudatused tuleks miiserver.exe.config hoopis. See fail on siiski ülekirjutatud iga uuendada nii, isegi kui see toimib algsel installimisel, süsteemi seisma esimese uuendada. Soovitus on fail "Machine.config" asemel värskendada.

Puhverserveri peab olema vajalik URL-ide avatud. [Office 365 URL-id ja IP-aadresside vahemikud ](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)on dokumenteerida ametliku nimekirja.

Järgmises tabelis on need, absoluutne tühjal miinimum saama ühenduse Azure AD üldse. Selles loendis ei sisalda valikulised funktsioonid, näiteks parooli tagasikirjutusega või Azure'i AD-ühenduse seisund. See on siin dokumenteerida tõrkeotsingu algse konfigureerimise.

URL-I | Port | Kirjeldus
---- | ---- | ----
mscrl.microsoft.com | HTTP/80 | Kasutatakse CRL loendite allalaadimine.
\*. verisign.com | HTTP/80 | Kasutatakse CRL loendite allalaadimine.
\*. entrust.com | HTTP/80 | CRL loendite allalaadimine MFA jaoks kasutada.
\*. windows.net | HTTPS-/ 443 | Kasutada Azure AD sisse logida.
Secure.aadcdn.microsoftonline-p.com | HTTPS-/ 443 | MFA jaoks kasutada.
\*. microsoftonline.com | HTTPS-/ 443 | Kasutada Azure AD kataloogi konfigureerimine ja andmete impordi/ekspordi.

## <a name="errors-in-the-wizard"></a>Viisardi tõrked
Installimise viisard on kasutusel kahte erinevat turvalisuse kontekstis. Klõpsake lehel **ühenduse Azure AD** kasutab praegu kehtib kasutaja. Klõpsake lehel **konfigureerimine** on muutumas [konto, kus töötab teenus sync engine](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-accounts). Puhverserveri kuvatakse teeme on globaalne masina, nii et probleemi korral probleem on tõenäoliselt juba **Azure AD ühenduse loomine** viisardi lehel kuvada.

Need on kõige levinumad vead kuvatakse installiviisardis.

### <a name="the-installation-wizard-has-not-been-correctly-configured"></a>Installimise viisard pole õigesti konfigureeritud
See tõrketeade kuvatakse, kui viisard ei jõua puhverserverist.
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomachineconfig.png)

- Kui te ei näe seda, veenduge, et [fail "Machine.config"](active-directory-aadconnect-prerequisites.md#connectivity) on õigesti konfigureeritud.
- Kui see on õiged, järgige [kinnitamine puhverserveri ühenduvuse](#verify-proxy-connectivity) kui probleem on olemas ka viisardi väljaspool.

### <a name="the-mfa-endpoint-cannot-be-reached"></a>MFA lõpp-punkti ei pääse
See tõrketeade kuvatakse, kui lõpp-punkti **https://secure.aadcdn.microsoftonline-p.com** ei pääse ja teie globaalne administraator on lubatud MFA.  
![nomachineconfig](./media/active-directory-aadconnect-troubleshoot-connectivity/nomicrosoftonlinep.png)

- Kui te ei näe seda, veenduge, et lõpp-punkti secure.aadcdn.microsoftonline-p.com on lisatud puhverserverist.

### <a name="the-password-cannot-be-verified"></a>Parooli ei saa kontrollida
Kui installimise viisard on edukalt Azure AD ühenduse, kuid enda parool ei saa kontrollida, kuvatakse see: ![badpassword](./media/active-directory-aadconnect-troubleshoot-connectivity/badpassword.png)

- On ajutine parool parool ja muuta? See on tegelikult õige parooli? Proovige https://login.microsoftonline.com (teises arvutis kui Azure'i AD-ühenduse server) sisse logida ja veenduge, et konto saab kasutada.

### <a name="verify-proxy-connectivity"></a>Kontrollige puhverserveri Ühenduvus
Kui soovite kontrollida, kas server Azure'i AD-ühenduse on tegelik ühenduvuse puhverserveri ja Interneti kasutame mõned PowerShelli kuvamiseks, kui puhverserver on jäetud web nõuab või mitte. PowerShelli käsuviibas käivitage `Invoke-WebRequest -Uri https://adminwebservice.microsoftonline.com/ProvisioningService.svc`. (Tehniliselt esimene on https://login.microsoftonline.com ja see töötab ka, kuid muud URI on kiiremaks vastamiseks.)

PowerShelli kasutab konfiguratsiooni fail "Machine.config" puhverserverist ühendust võtta. Sätted on seatud winhttp/netsh peaks mõju need cmdlet-käsud.

Kui puhverserver on õigesti konfigureeritud, kuvatakse edu olek: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest200.png)

Kui te **ei saa ühendust kaugserverisse** saate siis PowerShelli proovib ilma proxy otsese helistamine või DNS-i pole õigesti konfigureeritud. Veenduge, et **fail "Machine.config"** fail on õigesti konfigureeritud.
![unabletoconnect](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequestunable.png)

Kui puhverserver on õigesti konfigureeritud, saame tõrge: ![proxy200](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest403.png)
![proxy407](./media/active-directory-aadconnect-troubleshoot-connectivity/invokewebrequest407.png)

Tõrge | Tõrge tekst | Kommentaari
---- | ---- | ---- |
403 | Keelatud | Puhverserveri ei ole avatud nõutud URL-i. Puhverserveri konfigureerimine uuesti ja veenduge, et [URL](https://support.office.com/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2) on avatud.
407 | Puhverserveri autentimine on nõutav | Puhverserveri nõutav Logi sisse ja pole esitatud. Kui teie puhverserveri server nõuab autentimist, veenduge, et on see konfigureeritud soovitud fail "Machine.config". Samuti veenduge, et kasutate domeenikontod töötava viisardi ka teenuse konto kasutaja.

## <a name="the-communication-pattern-between-azure-ad-connect-and-azure-ad"></a>Suhtlus mustri Azure'i AD-ühenduse ja Azure AD vahel
Kui järgisite kõiki neid juhiseid ülaltoodud ja ikka ei saa ühendust luua, võib sel hetkel alustada, vaadates võrgu logid. Selles jaotises on tavaline ja eduka ühenduvuse mustri dokumenteerimine. See on ka loetelu levinud punane heeringas, mis võib eirata kui loete võrgu logid.

- Tekib https://dc.services.visualstudio.com kõned. See ei pea olema seda installimise puhverserverit õnnestub Ava ja neid võib eirata.
- Näete, et DNS-i resolvimise loetletakse tegelik hostide DNS-i nimi ruumi nsatc.net ja muude nimeruumid microsoftonline.com all olevat. Siiski ei ole mis tahes web hooldustaotlused tegelik serveri nimesid ja te ei pea lisada need puhverserverist.
- Lõpp-punktid adminwebservice ja provisioningapi (vt allpool logid) on discovery lõpp-punktid ja tegelik lõpp-punkt leidmiseks on erinevad olenevalt teie regioonis ja kasutamine.

### <a name="reference-proxy-logs"></a>Viide puhverserveri logid
Siin on dump on tegelik puhverserveri Logi ja installi viisardi lehel, kus see on võetud (sama lõpp-punkti Topeltkirjete on eemaldatud). See saab oma puhverserveri ja võrgu logid abimaterjalina. Tegelik lõpp-punktid võivad olla erinevad teie keskkonnas (eriti neid *kursiiv*).

**Azure AD ühenduse loomine**

Aeg | URL-I
--- | ---
1-11-2016 8:31 | Connect://login.microsoftonline.com:443
1-11-2016 8:31 | Connect://adminwebservice.microsoftonline.com:443
1-11-2016 8:32 | ühenduse loomine: / /*bba800-ankur*. microsoftonline.com:443
1-11-2016 8:32 | Connect://login.microsoftonline.com:443
1-11-2016 8:33 | Connect://provisioningapi.microsoftonline.com:443
1-11-2016 8:33 | ühenduse loomine: / /*bwsc02-relay*. microsoftonline.com:443

**Konfigureerimine**

Aeg | URL-I
--- | ---
1-11-2016 8:43 | Connect://login.microsoftonline.com:443
1-11-2016 8:43 | ühenduse loomine: / /*bba800-ankur*. microsoftonline.com:443
1-11-2016 8:43 | Connect://login.microsoftonline.com:443
1-11-2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
1-11-2016 8:44 | ühenduse loomine: / /*bba900-ankur*. microsoftonline.com:443
1-11-2016 8:44 | Connect://login.microsoftonline.com:443
1-11-2016 8:44 | Connect://adminwebservice.microsoftonline.com:443
1-11-2016 8:44 | ühenduse loomine: / /*bba800-ankur*. microsoftonline.com:443
1-11-2016 8:44 | Connect://login.microsoftonline.com:443
1-11-2016 8:46 | Connect://provisioningapi.microsoftonline.com:443
1-11-2016 8:46 | ühenduse loomine: / /*bwsc02-relay*. microsoftonline.com:443

**Algne sünkroonimine**

Aeg | URL-I
--- | ---
1-11-2016 8:48 | Connect://login.Windows.net:443
1-11-2016 8:49 | Connect://adminwebservice.microsoftonline.com:443
1-11-2016 8:49 | ühenduse loomine: / /*bba900-ankur*. microsoftonline.com:443
1-11-2016 8:49 | ühenduse loomine: / /*bba800-ankur*. microsoftonline.com:443

## <a name="authentication-errors"></a>Autentimise tõrked
Selles jaotises antakse ülevaade vigu, mida saab tagasi ADAL (autentimine teegi kasutatavaid Azure'i AD-ühenduse) ja PowerShelli kaudu. Ülevaade viga tuleks aidata teil mõista järgmised toimingud.

### <a name="invalid-grant"></a>Vigaste andmine
Sobimatu kasutajanimi ja parool. Lisateabe saamiseks vaadake [parooli ei saa kontrollida](#the-password-cannot-be-verified) .

### <a name="unknown-user-type"></a>Tundmatu kasutaja tüüp
Azure AD kataloogi ei leitud või lahendatud. Võib-olla proovite sisse logida kinnitamata domeenis kasutajanimi?

### <a name="user-realm-discovery-failed"></a>Domeen avastamine nurjus.
Võrgu või puhverserveri konfigureerimise probleemid. Võrgu ei saa jõudnud, lugege teemat [tõrkeotsing ühenduvuse installiviisardis](#troubleshoot-connectivity-issues-in-the-installation-wizard).

### <a name="user-password-expired"></a>Kasutaja parooli on aegunud
Mandaat on aegunud. Parooli muutmine.

### <a name="authorizationfailure"></a>AuthorizationFailure
Tundmatu probleem.

### <a name="authentication-cancelled"></a>Tühistatud autentimine
Mitmikautentimise (MFA) juures on keerukas on tühistatud.

### <a name="connecttomsonline"></a>ConnectToMSOnline
Autentimine õnnestus, kuid Azure AD PowerShelli on probleem autentimist.

### <a name="azurerolemissing"></a>AzureRoleMissing
Autentimine õnnestus. Te ei ole üldadministraator.

### <a name="privilegedidentitymanagement"></a>PrivilegedIdentityManagement
Autentimine õnnestus. Õigustega identiteetide haldus on lubatud ja te pole praegu üldadministraator. Lisateabe saamiseks vaadake [Õigustega identiteetide haldus](active-directory-privileged-identity-management-getting-started.md) .

### <a name="companyinfounavailable"></a>CompanyInfoUnavailable
Autentimine õnnestus. Ettevõtte teave ei saanud: Azure'i AD tuua.

### <a name="retrievedomains"></a>RetrieveDomains
Autentimine õnnestus. Teavet domeeni ei saanud: Azure'i AD tuua.

## <a name="troubleshooting-steps-for-previous-releases"></a>Varasemates versioonides tõrkeotsingujuhiseid.
Versioonidega, alustades järgu number 1.1.105.0 (avaldatud veebruar 2016) soovitud sisselogimisabimees on kasutuselt kõrvaldatud. Selles jaotises ja konfiguratsiooni peaks enam vajalik, kuid oleksid abimaterjalina.

Kuvatakse ühe-sisselogimisabimees töötada, peab olema konfigureeritud winhttp. Seda saab teha [**netsh**](active-directory-aadconnect-prerequisites.md#connectivity).  
![netsh](./media/active-directory-aadconnect-troubleshoot-connectivity/netsh.png)

### <a name="the-sign-in-assistant-has-not-been-correctly-configured"></a>Funktsiooni sisselogimisabimees on õigesti konfigureeritud
Funktsiooni sisselogimisabimees ei jõua puhverserverist või puhverserveri ei luba taotluse kuvatakse see tõrge.
![nonetsh](./media/active-directory-aadconnect-troubleshoot-connectivity/nonetsh.png)

- Kui te ei näe seda vaadata puhverserveri konfiguratsiooni [netsh](active-directory-aadconnect-prerequisites.md#connectivity) ja veenduge, et see on õige.
![netshshow](./media/active-directory-aadconnect-troubleshoot-connectivity/netshshow.png)
- Kui see on õiged, järgige [kinnitamine puhverserveri ühenduvuse](#verify-proxy-connectivity) kui probleem on olemas ka viisardi väljaspool.

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet [oma kohapealse identiteedi ja Azure Active Directory integreerimine](active-directory-aadconnect.md).
