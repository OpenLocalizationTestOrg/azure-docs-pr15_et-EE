
<properties
    pageTitle="Keerukamate reeglite loomiseks atribuutide abil | Microsoft Azure'i"
    description="Kuidas-'s keerukamate reeglite loomiseks rühma, kaasa arvatud toetatud avaldis reegli tehtemärgid ja parameetrid."
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="curtand"/>


# <a name="using-attributes-to-create-advanced-rules"></a>Atribuutide abil keerukamate reeglite loomiseks

Azure'i klassikaline portaali pakub võimalus lubada keerukamaid atribuut põhinev dünaamiline liikmelisused Azure Active Directory (Azure AD) rühma keerukamate reeglite loomiseks.  

Kui mis tahes kasutaja Muuda atribuute, süsteem analüüsib kõik dünaamilise rühma reeglite kuvamiseks, kui kasutaja atribuudi muutmine oleks käivitamine rühma kataloogis lisab või eemaldab. Kui kasutaja reegli rühmas, neid rühma lisada liige. Kui need pole enam vastavad need on liige rühma reeglit, eemaldatakse need on isikuid selle rühma.

## <a name="to-create-the-advanced-rule"></a>Täiustatud reegli loomiseks

1. [Azure'i klassikaline portaalis](https://manage.windowsazure.com)valige **Active Directory**ja avage ettevõttekataloogile.

2. Valige vahekaart **rühmad** ja seejärel avage jaotis, mida soovite redigeerida.

3. Valige vahekaart **konfigureerimine** , suvand **Täpsemad reegel** ja Sisestage tekstiväljale täpsemalt reegel.

## <a name="constructing-the-body-of-an-advanced-rule"></a>Sisu täpsemalt reegli loomine

Täiustatud reeglit, mida saate luua dünaamilisi liikmestaatuste rühmade jaoks on põhiosas kahendarvu avaldis, mis koosneb kolmest osast ja tulem on tõene või väär tulemus. Kolm osa on:

- Vasakul parameeter.
- Binaarsed tehtemärk
- Õige konstant

Täieliku täpsemalt reegli näeb välja selline: (leftParameter binaryOperator "RightConstant"), kus on vaja kogu kahendarvu avaldise avamine ja paremsulg jaoks õige konstandi vajalike jutumärkide ja vasakul parameetri süntaks on user.property. Täiustatud reegli võivad koosneda mitu semikooloniga eraldatud kahendarvu avaldiste- ja, - või, ja - ei loogika tehtemärke.
Järgnevalt on õigesti arvestusliku täpsemalt reegli näited.

- (user.department - eq "Müük")- või (user.department - eq "turundus")
- (user.department - eq "Müük")- ja - ei (user.jobTitle-sisaldab "SDE")

Toetatud parameetritest ja avaldis reegli tehtemärgid täieliku loendi leiate teemast allpool.

Täiustatud reegli keha kogupikkus ei tohi ületada 2048 märki.

> [AZURE.NOTE]
>Stringi ja regex tehtavad toimingud on tõstutundlikud. Saate teha ka Null kontrollid, kasutades $null nimega konstant, nt, user.department - eq $null.
Stringid, mis sisaldab hinnapakkumisi "abil tuleb seda vältida tuleks" märk, näiteks user.department - eq \`"Müük".

## <a name="supported-expression-rule-operators"></a>Toetatud avaldis reegli tehtemärgid
Järgmises tabelis on loetletud kõik toetatud avaldis reegli tehtemärgid ja nende süntaks, mida kasutatakse kehas täpsemalt reegel:

| Tehtemärk        | Süntaks         |
|-----------------|----------------|
| Ei võrdu      | -ne            |
| Võrdub          | -eq            |
| Pole algab | -notStartsWith |
| Algab     | -startsWith    |
| Ei sisalda    | -oleSee sisaldab   |
| Sisaldab        | -sisaldab      |
| Ei sobi       | -notMatch      |
| Match           | -vastavad         |


## <a name="query-error-remediation"></a>Päringu veaväärtuse parandamine
Järgmises tabelis on loetletud võimalike tõrgete ja kuidas neid parandada, kui neil ilmnevad

| Päringu sõeluda tõrge     | Tõrge kasutus       | Parandatud kasutus             |
|-----------------------|-------------------|-----------------------------|
| Tõrge: Atribuut pole toetatud.                                      | (user.invalidProperty - eq "Value")       | (user.department - eq "value")<br/>Atribuut peaksid olema samad üks [atribuutide loendi toetatud](#supported-properties).                          |
| Tõrge: Tehtemärk ei toeta atribuuti.                       | (user.accountEnabled-sisaldab true)                                                                               | (user.accountEnabled - eq true)<br/>Atribuut on tippige kahendmuutuja. Kasutage toetatud tehtemärke (-eq või - ne) tüüpi ülaltoodud loendi kohta.                                                                                                                                   |
| Tõrge: Päringu koostamise viga.                                      | (user.department - eq "Müük")- ja (user.department - eq "Turundus")(user.userPrincipalName-match"*@domain.ext") | (user.department - eq "Müük")- ja (user.department - eq "turundus")<br/>Loogika tehtemärkide peaksid olema samad ühte ülaltoodud loendis toetatud atribuudid. (user.userPrincipalName-vastavad ".*@domain.ext")or(user.userPrincipalName -vastavad "@domain.ext$")Error Lihtavaldise. |
| Tõrge: Binary avaldis ei ole õiges vormingus.                     | (user.department – eq "Müük") (user.department - eq "Müük") (user.department eq "Müük")                             | (user.accountEnabled - eq true)- ja (user.userPrincipalName-sisaldab"alias@domain")<br/>Päringus on mitu tõrked. Sulud kuulu õigesse kohta.                                                                                                                            |
| Tõrge: Dünaamiline liikmelisused häälestamise ajal ilmnes tundmatu tõrge. | (user.accountEnabled - eq "True" ja user.userPrincipalName-sisaldab"alias@domain")                               | (user.accountEnabled - eq true)- ja (user.userPrincipalName-sisaldab"alias@domain")<br/>Päringus on mitu tõrked. Sulud kuulu õigesse kohta.                                                                                                                            |

## <a name="supported-properties"></a>Toetatud atribuudid
Järgnevalt on kasutaja kõik atribuudid, mida saate kasutada täiustatud reegli.

### <a name="properties-of-type-boolean"></a>Tippige atribuudid kahendmuutuja

Lubatud tehtemärgid

* -eq


* -ne


| Atribuudid     | Lubatud väärtus  | Kasutus                          |
|----------------|-----------------|--------------------------------|
| accountEnabled | True, false      | user.accountEnabled - eq true)  |
| dirSyncEnabled | True ja false null | (user.dirSyncEnabled - eq true) |

### <a name="properties-of-type-string"></a>Tippige stringi atribuudid

Lubatud tehtemärgid

* -eq


* -ne


* -notStartsWith


* -StartsWith


* -sisaldab


* -oleSee sisaldab


* -vastavad


* -notMatch

| Atribuudid                 | Lubatud väärtus                                                                                        | Kasutus                                                     |
|----------------------------|-------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| linna                       | Mis tahes väärtus või $null                                                                           | (user.city - eq "value")                                   |
| riik                    | Mis tahes väärtus või $null                                                                            | (user.country - eq "value")                                |
| osakonna                 | Mis tahes väärtus või $null                                                                          | (user.department - eq "value")                             |
| displayName                | Mis tahes stringi väärtus                                                                                 | (user.displayName - eq "value")                            |
| facsimileTelephoneNumber   | Mis tahes väärtus või $null                                                                           | (user.facsimileTelephoneNumber - eq "value")               |
| givenName                  | Mis tahes väärtus või $null                                                                           | (user.givenName - eq "value")                              |
| Ametinimetus                   | Mis tahes väärtus või $null                                                                           | (user.jobTitle - eq "value")                               |
| e-posti                       | Mis tahes väärtus või $null (SMTP-aadress ja kasutajale)                                                  | (user.mail - eq "value")                                   |
| mailNickName               | Mis tahes stringiväärtus (kasutaja meili pseudonüüm)                                                            | (user.mailNickName - eq "value")                           |
| mobiilse                     | Mis tahes väärtus või $null                                                                           | (user.mobile - eq "value")                                 |
| ObjectId väärtuse                   | Kasutaja objekti GUID                                                                            | (user.objectId - eq "1111111-1111-1111-1111-111111111111") |
| passwordPolicies           | Ükski DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |   (user.passwordPolicies - eq "DisableStrongPassword")                                                      |
| physicalDeliveryOfficeName | Mis tahes väärtus või $null                                                                            | (user.physicalDeliveryOfficeName - eq "value")             |
| Sihtnumber                 | Mis tahes väärtus või $null                                                                            | (user.postalCode - eq "value")                             |
| preferredLanguage          | ISO 639-1 kood                                                                                        | (user.preferredLanguage - eq "en-US")                      |
| sipProxyAddress            | Mis tahes väärtus või $null                                                                            | (user.sipProxyAddress - eq "value")                        |
| olek                      | Mis tahes väärtus või $null                                                                            | (user.state - eq "value")                                  |
| streetAddress              | Mis tahes väärtus või $null                                                                            | (user.streetAddress - eq "value")                          |
| perekonnanimi                    | Mis tahes väärtus või $null                                                                            | (user.surname - eq "value")                                |
| telephoneNumber            | Mis tahes väärtus või $null                                                                            | (user.telephoneNumber - eq "value")                        |
| usageLocation              | Kahe asuv riigi kood                                                                           | (user.usageLocation - eq "USA")                             |
| userPrincipalName          | Mis tahes stringi väärtus                                                                                     | (user.userPrincipalName - eq"alias@domain")               |
| userType                   | liikme Külastajate $null                                                                                    | (user.userType - eq "Liige")                              |

### <a name="properties-of-type-string-collection"></a>Tippige stringi Saidikogumi atribuudid

Lubatud tehtemärgid

* -sisaldab


* -oleSee sisaldab

| Poperties      | Lubatud väärtus                        | Kasutus                                                |
|----------------|---------------------------------------|------------------------------------------------------|
| otherMails     | Mis tahes stringi väärtus                      | (user.otherMails-sisaldab"alias@domain")           |
| proxyAddresses | SMTP: alias@domain smtp:alias@domain | (user.proxyAddresses-sisaldab "SMTP:alias@domain") |

## <a name="extension-attributes-and-custom-attributes"></a>Laiend atribuudid ja kohandatud atribuudid
Dünaamiliste liikmelisusel toetavad laiend atribuudid ja kohandatud atribuudid.

Laiend atribuudid on sünkroonitud eeldusel aken Server AD ja võtta vorming "ExtensionAttributeX", kus X on 1-15.
Reegli, mis kasutab laiend atribuut näiteks oleks

(user.extensionAttribute15 - eq "turundus")

Kohandatud atribuudid on sünkroonitud eeldusel Windows Server AD või ühendatud SaaS rakendusest ja soovitud vormingu "user.extension_[GUID]\__ [atribuut]", kus [GUID] on rakenduse AAD atribuuti loodud kordumatu tunnuse AAD ja [atribuut] on atribuudi nimi, nagu see on loodud.
Näiteks reegli, mis kasutab kohandatud atribuut

User.extension_c272a57b722d4eb29bfe327874ae79cb__OfficeNumber  

Kohandatud atribuudi nimi leitavad kataloogis abil päringu kasutaja atribuut Graphi Explorerit ja otsimine atribuudi nimi.

## <a name="direct-reports-rule"></a>Otsesed alluvad reegel
Nüüd saate asustada halduri kasutaja atribuudi alusel rühma liikmed.

**Rühma "Manager" rühmana konfigureerimine**

1. Azure'i klassikaline portaalis, klõpsake **Active Directory**, ja klõpsake ettevõttekataloogile nime.

2. Valige vahekaart **rühmad** ja seejärel avage jaotis, mida soovite redigeerida.

3. Valige vahekaart **konfigureerimine** ja valige **Täpsem reegel**.

4. Tippige reegli järgmise süntaksi abil:

    Otse aruannete jaoks *otsesed alluvad jaoks {obectID_of_manager}*. Näiteks otsesed alluvad kehtiv reegel on

                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”

    kus on "62e19b97-8b3d-4d4a-a106-4ce66896a863" ning halduri ObjectId väärtuse. Objekti ID leiate Azure'i ad kasutajale, kes on ülemus kasutaja lehe **vahekaardi profiil** .

3. See reegel salvestamisel kuvatakse kõik kasutajad, mis vastavad reegli liitunud rühma liikmed. Mõne minuti rühma algselt asustamiseks võib kuluda.


## <a name="using-attributes-to-create-rules-for-device-objects"></a>Atribuutide abil seadme objektide reeglite loomine

Samuti saate luua reegli, mis valib seadme objektide liikmeks rühma. Saate kasutada seadme järgmisi atribuute:

| Atribuudid              | Lubatud väärtus                  | Kasutus                                                       |
|-------------------------|---------------------------------|-------------------------------------------------------------|
| displayName             | mis tahes stringi väärtus                | (device.displayName - eq "Rob Iphone")                       |
| deviceOSType            | mis tahes stringväärtuse                | (device.deviceOSType - eq "IOS")                             |
| deviceOSVersion         | mis tahes stringi väärtus                | (seade. OSVersion - eq "9.1")                                |
| isDirSynced             | True ja false null                 | (device.isDirSynced - eq "true")                             |
| isManaged               | True ja false null                 | (device.isManaged - eq "väär")                              |
| isCompliant             | True ja false null                 | (device.isCompliant - eq "true")                             |
| deviceCategory          | mis tahes stringi väärtus                | (device.deviceCategory - eq "")                              |
| deviceManufacturer      | mis tahes stringi väärtus                | (device.deviceManufacturer - eq "Microsoft")                 |
| deviceModel             | mis tahes stringi väärtus                | (device.deviceModel - eq "IPhone 7 +")                        |
| deviceOwnership         | mis tahes stringi väärtus                | (device.deviceOwnership - eq "")                             |
| Domeeninimi              | mis tahes stringväärtuse                | (device.domainName - eq "contoso.com")                       |
| enrollmentProfileName   | mis tahes stringväärtuse                | (device.enrollmentProfileName - eq "")                       |
| isRooted                | True ja false null                 | (device.deviceOSType - eq "true")                            |
| managementType          | mis tahes stringväärtuse                | (device.managementType - eq "")                              |
| organizationalUnit      | mis tahes stringiväärtuse                | (device.organizationalUnit - eq "")                          |
| deviceId                | lubatud deviceId                | (device.deviceId - eq "d4fe7726-5966-431c-b3b8-cddc8fdb717d" |

> [AZURE.NOTE]
> Seadme reegleid ei saa luua "lihtsa reegel" ripploend Azure klassikaline portaalis.


## <a name="additional-information"></a>Lisateave
Azure Active Directory esitada lisateavet järgmistest artiklitest.

* [Dünaamiliste liikmelisused rühmade tõrkeotsing](active-directory-accessmanagement-troubleshooting.md)

* [Azure Active Directory levirühmadega ressursid juurdepääsu haldamine](active-directory-manage-groups.md)

* [Azure Active Directory cmdlet-käsud rühma sätete konfigureerimine](active-directory-accessmanagement-groups-settings-cmdlets.md)

* [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)

* [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
