<properties
    pageTitle="Mis on Azure iseteenindusliku registreerumise? | Microsoft Azure'i"
    description="Mõne ülevaade iseteenindusliku registreerumise Azure, kuidas hallata registreerimine protsess ja DNS-i domeeni nimi üle võtta, tehke järgmist."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/23/2016"
    ms.author="curtand"/>


# <a name="what-is-self-service-signup-for-azure"></a>Mis on Azure iseteenindusliku registreerumise?

See teema selgitab iseteenindusliku registreerumise protsessi ja DNS-i domeeninimi üle võtta, tehke järgmist.  

## <a name="why-use-self-service-signup"></a>Miks kasutada iseteenindusliku registreerumise?

- Saada klientide soovidega arvestamine kiiremini teenused.
- Saate luua teenuse pakkumised e-postipõhise.
- Saate luua e-postipõhise registreerimine puhul, mis võimaldavad kiiresti kasutajate identiteetide abil oma töö lihtne meeles pidada meilipseudonüümide loomiseks.
- Mittehallatavate Azure kataloogide saavad teha hallatavate kataloogid hiljem ja muude teenuste jaoks uuesti kasutada.

## <a name="terms-and-definitions"></a>Tingimused ja määratlused

+ **Iseteenindusliku registreeruda**: see on meetod, mille kasutaja registreerub pilveteenus ja on nende jaoks automaatselt loodud Azure Active Directory (Azure AD) identiteedi vastavalt oma domeenis e-posti.
+ **Mittehallatavate Azure directory**: see on kataloogi, kus luuakse seda isikut. Mõne mittehallatava directory on kataloogi, mis sisaldab pole üldadministraator.
+ **E-posti kontrollida kasutaja**: see on teatud tüüpi kasutajakonto Azure AD. Kasutaja, kes on pärast registreerumist iseteenindusliku pakkumise jaoks automaatselt loodud identiteedi nimetatakse kasutajana e-posti kontrollida. E-posti kontrollida rakendust on tavaline kodeeritud creationmethod kataloog = EmailVerified.

## <a name="user-experience"></a>Kasutusvõimalused

Näiteks Oletame, et kasutaja nime, kelle e-posti on Dan@BellowsCollege.com saab tundliku faile meili teel. Kaitstud failid on Azure Rights Management (Azure RMS) alusel. Kuid Dan ettevõtte lõõtsa kõrgkooli, on registreerunud ei Azure RMS-i, samuti on see juurutatud Active Directory RMS-i. Sel juhul Dan saavad registreeruda tasuta tellimuse RMS-i üksikisikutele selleks, et lugeda kaitstud faile.

Kui Dan on esimene kasutaja e-posti aadress: BellowsCollege.com ja seejärel soovitud mittehallatavate directory luuakse BellowsCollege.com Azure AD selle iseteenindusliku pakkuv kasutajaks. Kui teised kasutajad BellowsCollege.com domeeni kasutajaks registreerumine selle pakkumise või samasugust pakub iseteenindusliku, nad on ka e-posti kontrollida kasutajakontode Azure sama mittehallatava kataloogis loonud.

## <a name="admin-experience"></a>Administraator kogemus

Administraator, kellele on mittehallatavate Azure'i directory DNS-i domeeni nimi, saate üle võtta, või ühendada kataloogi pärast omandiõiguse. Järgmise jaotistes kirjeldatakse üksikasjalikumalt kogemusi administraator, kuid siin on kokkuvõte.

- Kui teil on mittehallatava Azure'i directory üle võtta, siis lihtsalt saab üldadministraator mittehallatava kataloogi. See on mõnikord nimetatakse ka sisemise ülevõtmist.
- Kui ühendate mõne mittehallatava Azure'i directory, mittehallatava kataloogi domeeninime DNS-i lisamine oma Azure hallatav ja kasutajate-ressurssi vastendus on loodud nii kasutajate jätkata katkestusteta teenustele juurdepääs puudub. See on mõnikord nimetatakse ka välise ülevõtmist.

## <a name="what-gets-created-in-azure-active-directory"></a>Mis saab Azure Active Directory kataloogis loonud?

#### <a name="directory"></a>kataloog

- Mõne Azure Active Directory directory domeeni luuakse üks kataloog iga domeeni.
- Azure AD kataloogi on pole globaalne administraator.

#### <a name="users"></a>Kasutajatele

- Iga kasutaja, kes registreerub kasutaja objekt on loodud Azure AD kataloogi.
- Iga objekt on märgitud nii välise.
- Iga kasutaja on antud juurdepääs teenus, et nad.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Kuidas ma saan taotleda iseteenindusliku Azure AD kataloog ma oma domeeni?

Saate nõuda iseteenindusliku Azure AD kataloogi domeeni valideerimise abil. Domeeni kinnitamine näitab, et Domeen kuulub teile, luues oma DNS-i kirjed.

On kaks võimalust teha DNS-i ülevõtmine Azure'i AD-kaustas.

- sisemise ülevõtmine (administraator leiab mõne mittehallatava Azure'i directory ja soovib hallatav muutuda)
- välise ülevõtmine (administraator proovib uue domeeni lisamiseks Azure hallatav)

Teil võib huvi pakkuda ka kontrollimine domeeni omandiõigust, kuna te kasutate üle mõne mittehallatavate directory pärast kasutaja tehtud iseteenindusliku registreerumise või siis võib olla lisada uue domeeni hallatavate olemasoleva kausta. Näiteks teil on nimega contoso.com domeen ja soovite lisada uue domeeni nimega contoso.co.uk või contoso.uk.

## <a name="what-is-domain-takeover"></a>Mis on domeeni ülevõtmine?  

Selles jaotises antakse ülevaade kinnitada, et Domeen kuulub teile

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Mis on domeeni kinnitamine ja miks seda kasutada?

Selleks, et kataloogi toiminguid, Azure AD jaoks on vaja DNS-i domeeni omandiõiguse kinnitamiseks.  Domeeni kinnitamine võimaldab teil nõuda, kataloogi ja kas edendada hallatav, et Iseteeninduslik kataloogi või ühendada iseteenindusliku kataloogi hallatavate olemasoleva kausta.

## <a name="examples-of-domain-validation"></a>Domeeni kinnitamine näited

On kaks võimalust DNS-i kataloogi ülevõtmine teha.

+ sisemise ülevõtmine (nt administraator leiab iseteenindusliku, mittehallatavate directory ja soovib muutuda hallatav)
+ välise ülevõtmine (nt admin proovib uue domeeni lisamine hallatav)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-to-be-a-managed-directory"></a>Sisemise ülevõtmine - edendada iseteenindusliku, mittehallatava directory olevat hallatav

Kui te ei tee sisemise ülevõtmist, kataloogi saab teisendatakse mittehallatava Directory hallatav. Peate täitma DNS-i domeeni nimi valideerimise, kui loote DNS-tsooni MX-kirje või TXT-kirje. See toiming

+ Kinnitab, et Domeen kuulub teile
+ Muudab hallatavate kataloog
+ Mida teeb üldadministraator kataloogi

Oletame, et IT-administraator kõrgkooli lõõtsa leiab, et kooli kasutajad on registreerunud iseteenindusliku pakkumisi. Kui DNS-i registreeritud omaniku nimi BellowsCollege.com ja IT-administraator kinnitage DNS-i nimi Azure omandiõiguse ja mittehallatava kataloogi üle võtta, siis. Kataloogi saab siis hallatav ja IT-administraator on määratud üldadministraatori rolli BellowsCollege.com kataloog.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Välise ülevõtmine - ühendada iseteenindusliku directory hallatavate olemasoleva kausta

Klõpsake mõnda välise ülevõtmine on juba hallatav ja soovite, et kõik kasutajad ja rühmad mittehallatava directory kaudu liituda selle hallatav, mitte oma kaks eraldi kataloogid.

Hallatav administraatorina domeeni lisamine ja selle domeeni juhtub on seostatud mõne mittehallatava directory.

Näiteks Oletame, et olete IT-administraator ja teil on juba hallatav Contoso.com, domeeni nimi, mis on registreeritud teie organisatsioonile. Võite avastada, et oma asutuse kasutajate tehtud iseteenindusliku üles pakkumine, kasutades e-posti domeeninime user@contoso.co.uk, mis on muu domeeninimi on ettevõtte omanik. Praegu on nende kasutajate kontod mittehallatava Directorys contoso.co.uk jaoks.

Te ei soovi haldamine kahe eraldi kataloogi, nii, et contoso.co.uk mittehallatava kausta ühendamine oma praegust seda hallata kataloogist contoso.com.

Välise ülevõtmine järgib sama DNS-i valideerimine protsessi nagu sisemine ülevõtmist.  Erinevust: kasutajad ja teenused on aadressirida IT hallatavasse kataloogi.

#### <a name="whats-the-impact-of-performing-an-external-takeover"></a>Milline on selle mõju mõne välise ülevõtmine täita?

Mõne välise ülevõtmine, kus luuakse kasutajad – ressurssi vastendus nii, et kasutajad saavad jätkata katkestusteta teenustele juurdepääs puudub. Mitmel otstarbel, sealhulgas RMS-i üksikisikutele, toime kasutajad – ressursse kaardistamine ka ja kasutajad saavad jätkata juurdepääsu nende teenuste ilma muutmine. Kui rakendus ei oska vastenduse kasutajad-ressursside tõhus, välise ülevõtmine võib olla konkreetselt blokeeritud saate takistada kasutajatel halva kogemuse kaudu.

#### <a name="directory-takeover-support-by-service"></a>teenus Directory ülevõtmine tugi

Praegu ei toeta järgmisi teenuseid ülevõtmist.

- RMS-I


Järgmisi teenuseid kiiresti toetavad ülevõtmine:

- PowerBI

Järgnev ei ja täiendavad administraator nõuab kasutaja andmete migreerimiseks on välised ülevõtmine pärast.

- SharePointi/OneDrive


## <a name="how-to-perform-a-dns-domain-name-takeover"></a>Kuidas teha DNS-i domeeni nimi ülevõtmine

Teil on mitu võimalust, kuidas teha domeeni valideerimine (ja ei soovi ülevõtmine).

1.  Azure'i haldusportaal

    Ülevõtmine käivitatakse, tehes domeeni lisamine.  Kataloogi domeeni juba olemas, siis on teil võimalus teha ka välise ülevõtmine.

    Mandaadi abil Azure portaali sisse logida.  Avage oma olemasoleva kausta ja seejärel **Lisa domeen**.

2.  Office 365

    Saate suvandid, klõpsake lehel [domeenide haldamine](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) Teenusekomplektis Office 365 töötamiseks oma domeenide ja DNS-i kirjed. Lugege teemat [Office 365 domeeni kinnitamine](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).

3.  Windows PowerShelli

    Windows PowerShelli kaudu valideerimise täitma toimige järgmiselt.

    Toiming    |   Cmdlet-käsu kasutamine
    ------- | -------------
    Mandaadi objekti loomine | Get-mandaati
    Ühenduse loomine Azure AD | Ühenduse MsolService
    domeenide loendi hankimine   | Get-MsolDomain
    Probleemiks loomine  | Get-MsolDomainVerificationDns
    DNS-i kirje loomine   | Tehke oma DNS-i serveris
    Veenduge, et ülesanne    | Kinnitage MsolEmailVerifiedDomain

Näiteks:

1. Ühenduse loomine abil identimisteavet, mida kasutati vastata iseteenindusliku pakkumise Azure AD: impordi moodul MSOnline $msolcred = get-identimisteavet ühenduse msolservice-mandaati $msolcred

2. Domeenide loendi hankimine:

    Get-MsolDomain

3. Käivitage probleemiks loomiseks Get-MsolDomainVerificationDns cmdlet-käsk:

    Get-MsolDomainVerificationDns – DomainName *your_domain_name* – režiimi DnsTxtRecord

    Näiteks:

    Get-MsolDomainVerificationDns – domeeninimi contoso.com – režiimi DnsTxtRecord

4. Kopeerige (ülesanne) väärtus, mis tagastatakse see käsk.

    Näiteks:

    MS = 32DD01B82C05D27151EA9AE93C5890787F0E65D9

5. Looge DNS-i txt-kirje, mis sisaldab väärtust, mis eelmises etapis kopeeritud avaliku DNS-i nimeruumi.

    Kirje nimi on ema domeeni nimi, nii kui loote selle ressursi kirje, kasutades DNS-i roll Windows Server, jäta kirje nimi tühi ja lihtsalt kleebi väärtus tekstiväljale

6. Käivitage juures on keerukas kinnitamiseks kinnitamine-MsolDomain cmdlet-käsk:

    Kinnitage MsolEmailVerifiedDomain - DomainName *your_domain_name*

    näiteks:

    Kinnitage MsolEmailVerifiedDomain - DomainName contoso.com

Eduka probleemiks naaseb viip ilma tõrge.

## <a name="how-do-i-control-self-service-settings"></a>Kuidas kontrollida iseteenindusliku sätted?

Administraatoritel on kaks iseteenindusliku juhtelementide täna. Nad saavad juhtimiseks tehke järgmist.

- Kas kasutajad saavad liituda kataloogi e-posti teel.
- Kas kasutajad saavad litsentsi ise rakenduste ja teenuste jaoks.


### <a name="how-can-i-control-these-capabilities"></a>Kuidas määrata, neid võimalusi?

Administraator saab konfigureerida järgmisi võimalusi nende Azure AD cmdlet-käsu Set-MsolCompanySettings parameetrite abil:

+ **AllowEmailVerifiedUsers** kontrollib, kas kasutaja saab luua või sellega liitumine on mittehallatava kataloogi. Kui seate selle parameetri $false, pole e-posti kontrollida kasutajad saavad liituda kataloogi.
+ **AllowAdHocSubscriptions** reguleerida kasutajatel teha iseteenindusliku üles. Kui seate selle parameetri $false, pole kasutajad saavad teha iseteenindusliku registreerumise.


### <a name="how-do-the-controls-work-together"></a>Kuidas juhtelemendid töötavad koos?

Neid kahte parameetrit saab kasutada koos määratleda täpsemalt juhtida iseteenindusliku üles. Näiteks järgmine käsk võimaldab kasutajatel teha iseteenindusliku üles, kuid ainult siis, kui nende kasutajate juba konto Azure AD (ehk teisisõnu kasutajatele, kellel on vaja luua e-posti kontrollida konto ei saa teha iseteenindusliku üles):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

Järgmised vooskeem selgitatakse erinevate kombinatsioonide järgmiste parameetrite ja tulemuseks tingimused nimistust ja iseteenindusliku üles.

![][1]

Lisateavet ja näiteid, kuidas kasutada järgmiste parameetrite leiate teemast [Set-MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx).

## <a name="see-also"></a>Vt ka

-  [Kuidas installida ja konfigureerida Azure PowerShell](../powershell-install-configure.md)

-  [Azure'i PowerShelli](https://msdn.microsoft.com/library/azure/jj156055.aspx)

-  [Azure'i cmdleti viide](https://msdn.microsoft.com/library/azure/jj554330.aspx)

-  [Set-MsolCompanySettings](https://msdn.microsoft.com/library/azure/dn194127.aspx)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png
