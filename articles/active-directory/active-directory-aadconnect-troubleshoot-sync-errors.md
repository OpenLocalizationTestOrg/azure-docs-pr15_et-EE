<properties
    pageTitle="Azure'i AD-ühendus: Tõrkeotsing sünkroonimise ajal | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse koos Azure'i AD-ühenduse sünkroonimisel ilmnes tõrkeotsing."
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
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="vakarand"/>

# <a name="troubleshooting-errors-during-synchronization"></a>Tõrkeotsing sünkroonimise ajal
Tõrked võivad ilmneda identiteedi andmed on sünkroonitud Windows Server Active Directory (AD DS) Azure Active Directory (Azure AD). Selles artiklis antakse ülevaade eri tüüpi Sünkroonimistõrkeid, mõned võimalikud stsenaariumid, mis põhjustavad nende vigade ja võimalike võimalused tõrgete parandamiseks. Selles artiklis sisaldab viga levinumat ja võib hõlmata võimalikke vigu.

 Selles artiklis eeldatakse, et lugeja tunneb aluseks [mõisted Azure AD kujundamine ja Azure'i AD-ühenduse](active-directory-aadconnect-design-concepts.md).

Azure'i AD-ühenduse uusim versioon koos \(August 2016 või uuem versioon\), Sünkroonimistõrgete aruande on [Azure portaali](https://aka.ms/aadconnecthealth) sünkroonimise seisund Azure'i AD-ühenduse osana.


Alates 1 mai 2016 [Azure Active Directory dubleerimine atribuut paindlikkust](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) funktsioon aktiveeritakse vaikimisi kõigi *uute* Azure Active Directory rentnikke. See funktsioon aktiveeritakse automaatselt olemasoleva rentnike jaoks tulemas kuud.

Azure'i AD-ühendus sooritab 3 tüüpi toiminguid, kataloogid, hoiab sünkroonis: sünkroonimise impordi ja ekspordi. Tõrked võivad olla kõik need toimingud. See artikkel keskendub peamiselt tõrgete ajal Azure AD ekspordi.

## <a name="errors-during-export-to-azure-ad"></a>Tõrgete ajal ekspordi Azure AD
Pärast jaotises kirjeldatakse eri tüüpi Sünkroonimistõrgete, mis võivad tekkida Azure AD Azure AD kasutamisega eksporditoimingu käigus. Selle konnektori võib olla tähistatud nime vorming on "contoso. *onmicrosoft.com*".
Tõrgete ajal ekspordi Azure AD näitavad, et selle toimingu \(lisada, värskendada, kustutada jne\) Azure'i AD-ühenduse alusel proovitakse \(Sync Engine\) klõpsake Azure Active Directory nurjus.

![Ekspordi vigade ülevaade](.\media\active-directory-aadconnect-troubleshoot-sync-errors\Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Andmete lahknevuse tõrked
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Kirjeldus
- Kui Azure'i AD-ühenduse \(sünkroonimine engine\) juhendab Azure Active Directory lisamiseks või värskendamiseks objektid, Azure AD vastab abil **sourceAnchor** atribuut **immutableId** atribuut objektide Azure AD sissetulevate objekti. See match nimetatakse **Raske vastavad**.
- Kui Azure AD **ei leia** objekti, mis vastab **immutableId** atribuut **sourceAnchor** atribuut sissetulevate objekti, mille enne uuele objektile, kuulub tagasi atribuudid ProxyAddresses ja UserPrincipalName vaste otsimiseks kasutada. See match nimetatakse **Pehmete vastavad**. Pehmeid vastavad eesmärk on lisatud/uuendatud sünkroonimise ajal uue objektid, mis tähistavad sama olemi (kasutajad, rühmad) kohapealne objektid juba olemas Azure AD (mis on pärit Azure AD) vastavad.
- **InvalidSoftMatch** tõrge ilmneb juhul, kui suur match ei leia mis tahes vastavaid objekti **ja** pehmete vastavad leiab vastavaid objekti, kuid objekti on muu väärtus kui sissetulevate objekti *SourceAnchor*, mis näitab, et vastavaid objekti on sünkroonitud kohapealse Active Directory mõne muu objekti *immutableId* .

Teisisõnu, selleks pehmete match töö, olema täidetud sujuvate koos objekti peaks olema *immutableId*mis tahes väärtus. Kui mis tahes objekti *immutableId* seadistatud väärtus on raske match ei kuid kriteeriumidele sujuvate-vastet, toimingu tulemuseks InvalidSoftMatch sünkroonimise tõrge.

Azure Active Directory skeemi ei luba kahe või enama objektid on sama väärtusega järgmisi atribuute. \(See ei ole tulemus.\)

- ProxyAddresses
- UserPrincipalName
- onPremisesSecurityIdentifier
- ObjectId väärtuse

>[AZURE.NOTE] [Azure'i AD-atribuut Dubleeri atribuut paindlikkust](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) funktsiooni ka võetakse kasutusele nimega Azure Active Directory vaikekäitumise.  Azure'i AD-ühenduse (nagu ka muud sünkroonimise kliendid) näinud Sünkroonimistõrgete arvu vähendab, muutes Azure AD paindlikumaks käsitlemisel dubleeritakse ProxyAddresses atribuudid ja UserPrincipalName leiduvate tööruumide AD keskkonnas viisil. See funktsioon pole kordamise tõrgete parandamine. Nii andmeid vajab kinnitada. Kuid see võimaldab ettevalmistamise uute objektide, mis on blokeeritud muidu on ette valmistatud tõttu dubleeritud väärtused Azure AD. See ka vähendab tagastatud sünkroonimise kliendile Sünkroonimistõrgete arvu.
Kui see funktsioon on lubatud teie rentniku, kuvatakse InvalidSoftMatch Sünkroonimistõrgete ettevalmistamise uute objektide näha.


#### <a name="example-scenarios-for-invalidsoftmatch"></a>Näidisstsenaariumid InvalidSoftMatch jaoks
1. Kahe või enama objektide sama väärtusega atribuudis ProxyAddresses olemas kohapealse Active Directory. Ainult ühe on saada Azure AD ette valmistatud.
2. Kahe või enama objektide userPrincipalName sama väärtusega olemas kohapealse Active Directory. Ainult ühe on saada Azure AD ette valmistatud.
3. Objekti lisamine sees tööruumides Active Directory atribuudis ProxyAddresses sama väärtusega mis olemasoleva Azure Active Directory objekti. Kohapealne lisatud objekti pole saada Azure Active Directory ette valmistatud.
4. Objekti lisati kohapealse Active Directory sama väärtusega atribuut userPrincipalName, mis on Azure Active Directory konto. Objekt pole saada Azure Active Directory ette valmistatud.
5. Sünkroonitud konto on teisaldatud pärineb A – mets B. Azure'i AD-ühenduse (Sünkrooni mootor) on atribuutiFederatedIdentityDistinguishedNameatribuuti abil arvutada selle SourceAnchor. Pärast mets liikuda, kuvatakse SourceAnchor väärtus erineb. Olemasoleva objekti sünkroonimiseks Azure AD ei suuda (mets B) uue objekti.
6. Sünkroonitud objekti sai kogemata kustutatakse kohapealse Active Directory uuele objektile loomise ja Active Directory sama olemi (nt kasutaja) kustutamata Azure Active Directory konto. Uue konto nurjub sünkroonimiseks olemasoleva Azure AD objekti.
7. Azure'i AD-ühendus on desinstallida ja uuesti installida. Uuesti installimise ajal valitud eri atribuut selle SourceAnchor. Objektid, mis olid varem sünkroonitud lõpetanud InvalidSoftMatch tõrketeade.

#### <a name="example-case"></a>Näide juhul:
1. **Bob Smith** on sünkroonitud kasutaja Azure Active Directory kohapealse Active Directory *contoso.com* .
2. Bob Smith **UserPrincipalName** on seatud **bobs@contoso.com**.
3. **"abcdefghijklmnopqrstuv =="** on **SourceAnchor** Azure'i AD-ühenduse kaudu sees Bob Smith **FederatedIdentity** abil arvutatakse ruumide Active Directory, mis on **immutableId** Bob Smith Azure Active Directory jaoks.
4. Bob on ka pärast atribuudis **proxyAddresses** väärtused.
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
5.  Uus kasutaja, **Bob Taylor**, lisatakse sees tööruumide Active Directory.
6. Bob Taylor **UserPrincipalName** on seatud **bobt@contoso.com**.
7. **"abcdefghijkl0123456789 ==" "** on **sourceAnchor** Azure'i AD-ühenduse kaudu sees Bob Taylor **FederatedIdentity** abil arvutatakse ruumide Active Directory. Bob Taylor objekt on sünkroonitud Azure Active Directory veel.
8. Bob Taylor on atribuudis proxyAddresses järgmised väärtused
    - smtp:bobt@contoso.com
    - smtp:bob.taylor@contoso.com
    - **smtp:bob@contoso.com**
9. Sünkroonimise ajal Azure'i AD-ühenduse tuvastab Bob Taylori kohapealse Active Directory lisamine ja paluge Azure AD sama muudatusi teha.
10. Azure AD täidab esmalt raske match. Seega otsib, kui mis tahes objekti soovitud immutableId on võrdne "abcdefghijkl0123456789 ==". Suur Match nurjub, kui muu objekt, Azure AD on selle immutableId.
11. Azure AD proovib siis sujuvate-match Bob Taylor. Mis on see otsib kui mis tahes objekti proxyAddresses on kolm väärtustega, shsmtp:bob@contoso.com
12. Azure AD otsib Bob Smith objekti kriteeriumidele sujuvate-match. Kuid objektil immutableId väärtus = "abcdefghijklmnopqrstuv ==". mis näitab, et see objekt on sünkroonitud kohapealse Active Directory mõne muu objekti. Seega Azure AD ei saa sujuvate-match need objektid ja tulemuseks viga **InvalidSoftMatch** sünkroonimine.

#### <a name="how-to-fix-invalidsoftmatch-error"></a>Kuidas lahendada InvalidSoftMatch tõrge
Kõige levinum InvalidSoftMatch tõrke põhjuseks on kaks erinevat SourceAnchor objektid \(immutableId\) on sama väärtusega ProxyAddresses ja/või UserPrincipalName atribuudid, mida kasutatakse sujuvate-match käigus Azure AD. Selleks, et parandada sobimatud pehmed Match

1.  Leidke dubleeritakse proxyAddresses, userPrincipalName või muu atribuudi väärtus, mis võib põhjustada tõrke. Ka kindlaks, millised kaks \(või mitme\) objektid on kaasatud konflikti. Aruande loodud [Azure'i AD-ühenduse seisund sünkroonimise](https://aka.ms/aadchsyncerrors) saate tuvastada kaks objekti.
2. Tuvastada, millisele objektile peaks jääma dubleeritakse väärtus ja millisele objektile ei tohiks.
3. Eemaldage dubleeritakse väärtus objekti, mis peaks olema selle väärtuse. Pange tähele, et peate kataloogi, kus objekt on pärit muutmine. Mõnel juhul soovite kustutada ühe objekti konfliktis.
4. Kui tegite muudatus kohta tööruumides AD, lasta Azure'i AD-ühenduse muutmine sünkroonida.

Pange tähele, et sünkroonimine tõrkearuanne sees Azure'i AD-ühenduse seisund sünkroonimise värskendatakse iga 30 minuti sisaldab uusim sünkroonimiskatse tõrgete.

>[AZURE.NOTE] ImmutableId, määratluse peaks ei muutu objekti eluiga. Kui mõned stsenaariumid arvesse ülaltoodud loendis pole konfigureeritud Azure'i AD-ühenduse, saate võib lõpuks olukorras, kus Azure'i AD-ühenduse arvutab muu väärtuse sourceAnchor AD objekti, mis tähistab sama üksus (sama kasutaja/rühma/kontakti jne), mis on olemasoleva Azure AD objekti, mida soovite jätkata abil.

#### <a name="related-articles"></a>Seotud artiklid
- [Dubleeritud või sobimatud atribuudid takistavad kataloogisünkroonimist Teenusekomplektis Office 365](https://support.microsoft.com/en-us/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Kirjeldus
Kui Azure AD proovib pehmed vastavad kaks objekti, on võimalik, et kaks objekti erinevate "objekti tüüp" (nt kasutaja, rühma, kontakti jne) on samad väärtused pehmete match täitmiseks kasutatud atribuudid. Nagu kattuvat neid atribuute ei ole lubatud Azure AD, saate tuua "ObjectTypeMismatch" sünkroonimise tõrge.

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>Näidisstsenaariumid ObjectTypeMismatch tõrke
- E-posti lubatud turberühma luuakse Teenusekomplektis Office 365. Administraator lisab uue kasutaja või kontakti kohapealne AD (mis ei sünkroonita Azure AD veel) sama väärtusega atribuut ProxyAddresses jaoks Office 365 rühm nimega.

#### <a name="example-case"></a>Näide juhul

1. Administraator loob uue e-posti lubatud turberühma Teenusekomplektis Office 365 osakonna maksuvabastuse ja meiliaadressi, mis pakub tax@contoso.com. Seda määrab selle rühma väärtusega atribuut ProxyAddresses**smtp:tax@contoso.com**
2. Uue kasutaja liitub Contoso.com ja konto on loodud kohapealne Kasutajaatribuut nimega ja kasutajale**smtp:tax@contoso.com**
3. Kui Azure'i AD-ühenduse sünkroonitakse uue kasutajakonto, see on kuvatakse tõrketeade "ObjectTypeMismatch".

#### <a name="how-to-fix-objecttypemismatch-error"></a>Kuidas lahendada ObjectTypeMismatch tõrge
ObjectTypeMismatch tõrke levinuim põhjus on kaks eri tüüpi (kasutaja, rühma, kontakti jne) objektid on sama väärtusega atribuut ProxyAddresses jaoks. Selleks, et parandada selle ObjectTypeMismatch:

1.  Määratlege dubleeritakse proxyAddresses (või muud atribuuti) väärtus, mis võib põhjustada tõrke. Ka kindlaks, millised kaks \(või mitme\) objektid on kaasatud konflikti. Aruande loodud [Azure'i AD-ühenduse seisund sünkroonimise](https://aka.ms/aadchsyncerrors) saate tuvastada kaks objekti.
2. Tuvastada, millisele objektile peaks jääma dubleeritakse väärtus ja millisele objektile ei tohiks.
3. Eemaldage dubleeritakse väärtus objekti, mis peaks olema selle väärtuse. Pange tähele, et peate kataloogi, kus objekt on pärit muutmine. Mõnel juhul soovite kustutada ühe objekti konfliktis.
4. Kui tegite muudatus kohta tööruumides AD, lasta Azure'i AD-ühenduse muutmine sünkroonida. Sünkroonimine tõrkearuanne Azure'i AD-ühenduse seisund sünkroonimise sees saab värskendada iga 30 minuti ja see sisaldab uusim sünkroonimiskatse tõrgete.


## <a name="duplicate-attributes"></a>Dubleeritud atribuudid
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Kirjeldus
Azure Active Directory skeemi ei luba kahe või enama objektid on sama väärtusega järgmisi atribuute. Mis on iga objekti Azure AD on sunnitud on kordumatu väärtus neist atribuutidest antud astmes.

- ProxyAddresses
- UserPrincipalName

Azure'i AD-ühenduse proovib uuele objektile lisada või värskendada väärtus ülaltoodud atribuudid, mis on juba määratud mõnele muule objektile Azure Active Directory objekti, tulemuseks toimingut "AttributeValueMustBeUnique" sünkroonimise tõrge.
#### <a name="possible-scenarios"></a>Võimalikud stsenaariumid:
1. Dubleeritud väärtus on määratud juba sünkroonitud objekti, mis on vastuolus sünkroonitud mõne muu objekti.

#### <a name="example-case"></a>Näide juhul:
1. **Bob Smith** on sünkroonitud kasutaja Azure Active Directory kohapealse Active Directory contoso.com.
2. Kohapealne Bob Smith **UserPrincipalName** on seatud **bobs@contoso.com**.
3. Bob on ka pärast atribuudis **proxyAddresses** väärtused.
    - smtp:bobs@contoso.com
    - smtp:bob.smith@contoso.com
    - **smtp:bob@contoso.com**
4. Uus kasutaja, **Bob Taylor**, lisatakse sees tööruumide Active Directory.
5. Bob Taylor **UserPrincipalName** on seatud **bobt@contoso.com**.
6. **Bob Taylor** on atribuudis **ProxyAddresses** järgmised väärtused ma. smtp:bobt@contoso.comII. smtp:bob.taylor@contoso.com
7. Bob Taylor objekt on sünkroonitud Azure AD edukalt.
8. Administraator on otsustanud värskendada Bob Taylor atribuuti **ProxyAddresses** järgmine väärtus: ma. **smtp:bob@contoso.com**
9. Azure AD proovib Bob Taylor objekti värskendamiseks Azure AD ülaltoodud väärtusega, kuid see toiming ei õnnestu nii, et ProxyAddresses väärtus on juba määratud Bob kask, tulemuseks "AttributeValueMustBeUnique" tõrge.

#### <a name="how-to-fix-attributevaluemustbeunique-error"></a>Kuidas lahendada AttributeValueMustBeUnique tõrge
Enamlevinud AttributeValueMustBeUnique tõrke põhjuseks on kaks erinevat SourceAnchor objektid \(immutableId\) on sama väärtusega ProxyAddresses ja/või UserPrincipalName atribuudid. Selleks, et AttributeValueMustBeUnique parandamine

1.  Leidke dubleeritakse proxyAddresses, userPrincipalName või muu atribuudi väärtus, mis võib põhjustada tõrke. Ka kindlaks, millised kaks \(või mitme\) objektid on kaasatud konflikti. Aruande loodud [Azure'i AD-ühenduse seisund sünkroonimise](https://aka.ms/aadchsyncerrors) saate tuvastada kaks objekti.
2. Tuvastada, millisele objektile peaks jääma dubleeritakse väärtus ja millisele objektile ei tohiks.
3. Eemaldage dubleeritakse väärtus objekti, mis peaks olema selle väärtuse. Pange tähele, et peate kataloogi, kus objekt on pärit muutmine. Mõnel juhul soovite kustutada ühe objekti konfliktis.
4. Kui tegite muudatus kohta tööruumides AD, lasta Azure'i AD-ühenduse muutmine saada fikseeritud tõrke sünkroonida.

#### <a name="related-articles"></a>Seotud artiklid
-[Dubleeritud või sobimatud atribuudid takistavad kataloogisünkroonimist Teenusekomplektis Office 365](https://support.microsoft.com/en-us/kb/2647098)


## <a name="data-validation-failures"></a>Andmete valideerimise nurjumine
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Kirjeldus
Azure Active Directory jõustab piirangute andmete enne selle kataloogi andmete põhjal. See on tagada, et kasutajad saavad kõige paremini võimalik kogemusi kasutades andmed sõltuvad rakendused.
#### <a name="scenarios"></a>Stsenaariumid
lisamine. Atribuudi UserPrincipalName väärtus on lubamatu/toetuseta märki.
b. Atribuudi UserPrincipalName järgi nõutav vorm.
#### <a name="how-to-fix-identitydatavalidationfailed-error"></a>Kuidas lahendada IdentityDataValidationFailed tõrge

lisamine. Veenduge, et atribuudi userPrincipalName on toetatud märgid ja nõutav vorm.

#### <a name="related-articles"></a>Seotud artiklid
- [Kasutajate Kataloogisünkroonimise kaudu Office 365 peale üleviimiseks valmistumine]( https://support.office.com/en-us/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)


### <a name="datavalidationfailed"></a>DataValidationFailed
#### <a name="description"></a>Kirjeldus
See on väga kindla juhtum, mis annab tulemuseks muutumisel kasutaja UserPrincipalName järelliite ühe ühendatud domeeni ühendatud lisadomeeni sünkroonimise tõrge **"DataValidationFailed"** .

#### <a name="scenarios"></a>Stsenaariumid
Sünkroonitud kasutaja, UserPrincipalName järelliite muudeti ühe ühendatud domeenis mõne muu välise domeeni kohapealne. Näiteks *UserPrincipalName = bob@contoso.com * muudeti *UserPrincipalName = bob@fabrikam.com *.

#### <a name="example"></a>Näide
1. Bob kask, konto jaoks Contoso.com, lisatakse uus kasutaja ja selle UserPrincipalName Active Directorysbob@contoso.com
2. Erinevate osakond ehk Fabrikam.com contoso.com teisaldab Bob ja tema UserPrincipalName on muutunudbob@fabrikam.com
3. Nii contoso.com ja fabrikam.com domeenid on ühendatud domeenides Azure Active Directory.
4. Bob userPrincipalName ei värskendata ja tulemuseks sünkroonimise tõrge "DataValidationFailed".

#### <a name="how-to-fix"></a>Kuidas lahendada
Kui kasutaja UserPrincipalName sufiks värskendati kaudu bob@ **contoso.com** , et bob@ **fabrikam.com**, kus nii **contoso.com** ja **fabrikam.com** on **ühendatud domeenides**, siis järgige selle all juhiseid sünkroonimine vea parandamiseks

1. Värskendage kasutaja UserPrincipalName Azure AD kaudu bob@contoso.com et bob@contoso.onmicrosoft.com. Saate Azure AD PowerShelli mooduli PowerShelli järgmine käsk:`Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`

2. Luba järgmise sünkroonimise tsükli proovida sünkroonimine. See aeg sünkroonimine on eduka ja see update Bob UserPrincipalName, et bob@fabrikam.com ootuspäraselt.


## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Kirjeldus
Atribuut ületab lubatud maht piiratud, pikkuse piirang või loendus Azure Active Directory skeemi määratud piirmäära, tulemuseks sünkroonimise toiming on **LargeObject** või **ExceededAllowedLength** Sünkroonimisprobleemid. Tavaliselt on see tõrge ilmneb järgmised atribuudid

- userCertificate
- thumbnailPhoto
- proxyAddresses

### <a name="possible-scenarios"></a>Võimalikud stsenaariumid
1. Bob userCertificate atribuut on liiga palju serdid määratud Bob salvestamine. Need võivad sisaldada vanem, aegunud serdid.
2. Bob thmubnailPhoto seadmine Active Directory on liiga suur Azure AD sünkroonida.
3. Ajal automaatse kogumi Active Directory atribuut ProxyAddresses, kas teil on määratud objekti > 500 ProxyAddresses.

### <a name="how-to-fix"></a>Kuidas lahendada

1. Veenduge, et tõrget atribuut on lubatud piirangute raames.

## <a name="related-links"></a>Seotud lingid
- [Otsimine Active Directory objekti Active Directory halduskeskus] (https://technet.microsoft.com/library/dd560661.aspx)
- [Kuidas päringu Azure Active Directory objekti Azure Active Directory PowerShelli abil](https://msdn.microsoft.com/library/azure/jj151815.aspx)
