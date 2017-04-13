
<properties
    pageTitle="Atribuutide abil luua täpsemad reeglid rühmakuuluvus Azure Active Directory eelvaade | Microsoft Azure'i"
    description="Kuidas luua täpsema dünaamilise rühma liikmete kaasamise reeglid toetatud avaldis reegli tehtemärgid ja parameetrid."
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
    ms.date="09/12/2016"
    ms.author="curtand"/>


# <a name="using-attributes-to-create-advanced-rules-for-group-membership-in-azure-active-directory-preview"></a>Atribuutide abil luua täpsemad reeglid rühmakuuluvus Azure Active Directory eelvaade

Azure portaali pakub teile võimalust lubada keerukamaid atribuut põhinev dünaamiline liikmelisused Azure Active Directory (Azure AD) eelvaade rühmade keerukamate reeglite loomiseks. [Mis on eelvaates?](active-directory-preview-explainer.md) See artikkel üksikasjad reegli atribuudid ja nende täiustatud reeglite loomiseks süntaks.

## <a name="to-create-the-advanced-rule"></a>Täiustatud reegli loomiseks

1.  Logige sisse kontoga, millel on kataloogi üldadministraator [Azure portaali](https://portal.azure.com) .

2.  Valige **rohkem teenuseid**, Sisestage tekstiväljale **kasutajad ja rühmad** ja seejärel valige **Sisesta**.

  ![Avamine kasutajate haldamine](./media/active-directory-groups-dynamic-membership-azure-portal/search-user-management.png)

3.  Enne **kasutajad ja rühmad** , valige **kõik rühmad**.

  ![Rühmade tera avamine](./media/active-directory-groups-dynamic-membership-azure-portal/view-groups-blade.png)

4. Enne **kasutajad ja rühmad - kõik rühmad** , valige käsk **Lisa** .

  ![Lisa uus rühm](./media/active-directory-groups-dynamic-membership-azure-portal/add-group-type.png)

5. Enne **rühma** , sisestage nimi ja kirjeldus uue rühma. Valige on **Tippige liikmelisuse** **Dünaamiline kasutaja** või **Dünaamiline seade**, olenevalt sellest, kas soovite luua reegli kasutajate või seadmed ja valige **Add dünaamiliseks päringu**. Seadme reeglite kasutatakse atribuutide, leiate teemast [seadme objektide reeglite loomiseks kasutamine atribuute](#using-attributes-to-create-rules-for-device-objects).

  ![Dünaamiliste liikmelisus reegli lisamine](./media/active-directory-groups-dynamic-membership-azure-portal/add-dynamic-group-rule.png)

6. **Dünaamiliste liikmelisusel** enne, sisestage oma reegli väljale **Lisa dünaamiline liikmelisuse täpsemalt reegel** , sisestusklahvi ja valige **Loo** tera allosas.

7. **Rühma** enne rühma loomiseks valige **Loo** .

## <a name="constructing-the-body-of-an-advanced-rule"></a>Sisu täpsemalt reegli loomine

Täiustatud reeglit, mida saate luua dünaamilisi liikmelisus rühmade jaoks on põhiosas kahendarvu avaldis, mis koosneb kolmest osast ja tulem on tõene või väär tulemus. Kolm osa on:

- Vasakul parameeter.
- Binaarsed tehtemärk
- Õige konstant

Täieliku täpsemalt reegli näeb välja selline: (leftParameter binaryOperator "RightConstant"), kus on vaja kogu kahendarvu avaldise avamine ja paremsulg jaoks õige konstandi vajalike jutumärkide ja vasakul parameetri süntaks on user.property. Täiustatud reegli võivad koosneda mitu semikooloniga eraldatud kahendarvu avaldiste- ja, - või, ja - ei loogika tehtemärke.

Järgnevalt on õigesti arvestusliku täpsemalt reegli näited.

- (user.department - eq "Müük")- või (user.department - eq "turundus")
- (user.department - eq "Müük")- ja - ei (user.jobTitle-sisaldab "SDE")

Toetatud parameetritest ja avaldis reegli tehtemärgid täieliku loendi leiate teemast allpool. Seadme reeglite kasutatakse atribuutide, leiate teemast [seadme objektide reeglite loomiseks kasutamine atribuute](#using-attributes-to-create-rules-for-device-objects).

Täiustatud reegli keha kogupikkus ei tohi ületada 2048 märki.

> [AZURE.NOTE]
>Stringi ja regex tehtavad toimingud on tõstutundlikud. Saate teha ka Null kontrollid, kasutades $null nimega konstant, nt, user.department - eq $null.
Stringid, mis sisaldab hinnapakkumised "abil tuleb seda vältida tuleks" märk, näiteks user.department - eq \`"Müük".

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
| Tõrge: Päringu koostamise viga.                                      | (user.department - eq "Müük")- ja (user.department - eq "Turundus")(user.userPrincipalName-match"*@domain.ext") | (user.department - eq "Müük")- ja (user.department - eq "turundus")<br/>Loogika tehtemärkide peaksid olema samad ülaltoodud loendist toetatud atribuudid. (user.userPrincipalName-vastavad ".*@domain.ext")or(user.userPrincipalName -vastavad "@domain.ext$")Error Lihtavaldise. |
| Tõrge: Kahendarvu avaldis ei ole õiges vormingus.                     | (user.department – eq "Müük") (user.department - eq "Müük") (user.department eq "Müük")                             | (user.accountEnabled - eq true)- ja (user.userPrincipalName-sisaldab"alias@domain")<br/>Päringus on mitu tõrked. Sulud kuulu õigesse kohta.                                                                                                                            |
| Tõrge: Dünaamiline liikmelisused häälestamise ajal ilmnes tundmatu tõrge. | (user.accountEnabled - eq "True" ja user.userPrincipalName-sisaldab"alias@domain")                               | (user.accountEnabled - eq true)- ja (user.userPrincipalName-sisaldab"alias@domain")<br/>Päringus on mitu tõrked. Sulud kuulu õigesse kohta.                                                                                                                            |

## <a name="supported-properties"></a>Toetatud atribuudid
Kõik kasutaja atribuudid, mida saate kasutada täiustatud reegli on järgmised:

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
| displayName                | Mis tahes stringiväärtuse                                                                                 | (user.displayName - eq "value")                            |
| facsimileTelephoneNumber   | Mis tahes väärtus või $null                                                                           | (user.facsimileTelephoneNumber - eq "value")               |
| givenName                  | Mis tahes väärtus või $null                                                                           | (user.givenName - eq "value")                              |
| Ametinimetus                   | Mis tahes väärtus või $null                                                                           | (user.jobTitle - eq "value")                               |
| e-posti                       | Mis tahes väärtus või $null (SMTP-aadress ja kasutajale)                                                  | (user.mail - eq "value")                                   |
| mailNickName               | Mis tahes stringiväärtuse (kasutaja meili pseudonüüm)                                                            | (user.mailNickName - eq "value")                           |
| mobiilse                     | Mis tahes väärtus või $null                                                                           | (user.mobile - eq "value")                                 |
| ObjectId väärtuse                   | Kasutaja objekti GUID                                                                            | (user.objectId - eq "1111111-1111-1111-1111-111111111111") |
| passwordPolicies           | Ükski DisableStrongPassword DisablePasswordExpiration DisablePasswordExpiration, DisableStrongPassword |   (user.passwordPolicies - eq "DisableStrongPassword")                                                      |
| physicalDeliveryOfficeName | Mis tahes stringiväärtuse või $null                                                                            | (user.physicalDeliveryOfficeName - eq "value")             |
| Sihtnumber                 | Mis tahes väärtus või $null                                                                            | (user.postalCode - eq "value")                             |
| preferredLanguage          | ISO 639-1 kood                                                                                        | (user.preferredLanguage - eq "en-US")                      |
| sipProxyAddress            | Mis tahes väärtus või $null                                                                            | (user.sipProxyAddress - eq "value")                        |
| olek                      | Mis tahes väärtus või $null                                                                            | (user.state - eq "value")                                  |
| streetAddress              | Mis tahes väärtus või $null                                                                            | (user.streetAddress - eq "value")                          |
| perekonnanimi                    | Mis tahes väärtus või $null                                                                            | (user.surname - eq "value")                                |
| telephoneNumber            | Mis tahes väärtus või $null                                                                            | (user.telephoneNumber - eq "value")                        |
| usageLocation              | Kahe asuv riigi kood                                                                           | (user.usageLocation - eq "USA")                             |
| userPrincipalName          | Mis tahes stringiväärtuse                                                                                     | (user.userPrincipalName - eq"alias@domain")               |
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

Kohandatud atribuudi nimi leitavad kataloogis abil päringu kasutaja atribuut Exploreriga Graph ja otsimine atribuudi nimi.

## <a name="direct-reports-rule"></a>Otsesed alluvad reegel
Nüüd saate asustada halduri kasutaja atribuudi alusel rühma liikmed.

**Rühma "Manager" rühmana konfigureerimine**

1. Järgige juhiseid 1 – 5 [Täpsemalt reegli loomiseks](#to-create-the-advanced-rule)ja valige **Dünaamiline kasutaja** **liikmelisuse tüüp** .

2. **Dünaamiliste liikmelisusel** enne, sisestage reegli järgmise süntaksi abil:

    Otse aruannete jaoks *otsesed alluvad jaoks {obectID_of_manager}*. Näiteks otsesed alluvad kehtiv reegel on

                    Direct Reports for "62e19b97-8b3d-4d4a-a106-4ce66896a863”

    kus on "62e19b97-8b3d-4d4a-a106-4ce66896a863" ning halduri ObjectId väärtuse. Objekti ID leiate Azure'i ad kasutajale, kes on ülemus kasutaja lehe **vahekaardi profiil** .

3. See reegel salvestamisel kuvatakse kõik kasutajad, mis vastavad reegli liitunud rühma liikmed. Mõne minuti rühma algselt asustamiseks võib kuluda.


## <a name="using-attributes-to-create-rules-for-device-objects"></a>Atribuutide abil seadme objektide reeglite loomine

Samuti saate luua reegli, mis valib seadme objektide liikmeks rühma. Saate kasutada seadme järgmisi atribuute:

| Atribuudid           | Lubatud väärtus                  | Kasutus                                                |
|----------------------|---------------------------------|------------------------------------------------------|
| displayName          | mis tahes stringi väärtus                | (device.displayName - eq "Rob Iphone")                 |
| deviceOSType         | mis tahes stringi väärtus                | (device.deviceOSType - eq "IOS")                      |
| deviceOSVersion      | mis tahes stringväärtuse                | (seade. OSVersion - eq "9.1")                         |
| isDirSynced          | True ja false null                 | (device.isDirSynced - eq "true")                      |
| isManaged            | True ja false null                 | (device.isManaged - eq "väär")                       |
| isCompliant          | True ja false null                 | (device.isCompliant - eq "true")                      |


## <a name="additional-information"></a>Lisateave
Nendest artiklitest teavet täiendavad Azure Active Directory rühmad.

* [Vaadake olemasoleva rühmad](active-directory-groups-view-azure-portal.md)
* [Uue rühma loomine ja liikmete lisamine](active-directory-groups-create-azure-portal.md)
* [Rühma sätete haldamine](active-directory-groups-settings-azure-portal.md)
* [Rühma liikmestaatuste haldamine](active-directory-groups-membership-azure-portal.md)
* [Dünaamiliste reeglite kasutajate rühma haldamine](active-directory-groups-dynamic-membership-azure-portal.md)
