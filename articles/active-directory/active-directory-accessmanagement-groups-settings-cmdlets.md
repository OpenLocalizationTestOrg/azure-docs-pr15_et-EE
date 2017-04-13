<properties
    pageTitle="Azure Active Directory cmdlet-käsud rühma sätete konfigureerimine | Microsoft Azure'i"
    description="Kuidas hallata Azure Active Directory cmdlet-käskude kasutamine rühmade sätteid."
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
    ms.date="09/22/2016"
    ms.author="curtand"/>


# <a name="azure-active-directory-cmdlets-for-configuring-group-settings"></a>Azure Active Directory cmdlet-käsud rühma sätete konfigureerimine

Kataloogi saab konfigureerida järgmisi sätteid ühendatud rühmad:

1.  Liigitusi: komaga eraldatud loend liigitusi, mida kasutajad saavad seadistada rühmas. Näiteid oleks "Tasuta", "Salajane" ja "Salajase."

2.  URL-i kasutuse juhised: URL, mis osutab kasutajate kasutustingimused ühendatud rühmad, kasutades teie asutuse määratletud. Seda URL-i ilmub kasutajaliides, kus kasutajad kasutada rühmad.

3.  Rühma loomine lubatud: kas pole, mõned või kõik kasutajad tohivad ühendatud rühmad luua. Kui sisse lülitatud, kõik kasutajad saavad luua rühmad. Kui valitud välja lülitatud, ei ole kasutajad saavad luua rühmad. Kui väljas, saate määrata ka väärtpaberi rühma nime, kelle kasutajad, kes veel lubatud rühmad luua.

Need sätted on konfigureeritud, kasutades soovitud sätteid ja SettingsTemplate objektid. Esialgu ei kuvata mõnda sätted objektide kataloogis. See tähendab, et kataloogi on konfigureeritud vaikesätted. Vaikesätete muutmiseks loote uue sätted objekti sätted malli abil. Microsoft on määratletud sätted mallid.

Saate alla laadida moodul, mis sisaldab [Microsoft Connect saidi](http://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185)kaudu nende toimingute puhul kasutada cmdlet-käsud.

## <a name="create-settings-at-the-directory-level"></a>Tasemele kataloogi sätted loomine

Need juhised luua sätted directory tasemel, mis kehtivad kõigi Office rühmade kataloogis.

1. Kui te ei tea, millist SettingTemplate kasutada, tagastab see cmdlet-käsk sätted mallide loendi:

    `Get-MsolAllSettingTemplate`

    ![Loendi sätted Mallid](./media/active-directory-accessmanagement-groups-settings-cmdlets/list-of-templates.png)

2. Kasutus suunis URL-i lisamiseks tuleb kõigepealt hankida SettingsTemplate objekt, mis määratleb kasutus suunis URL-i väärtus; mis on Group.Unified malli:

    `$template = Get-MsolSettingTemplate –TemplateId 62375ab9-6b52-47ed-826b-58e47e0e304b`

3. Järgmiseks Looge sellel mallil põhineva sätteid uuele objektile.

    `$setting = $template.CreateSettingsObject()`

4. Seejärel värskendage kasutus suunis väärtus.

    `$setting["UsageGuidelinesUrl"] = "<https://guideline.com>"`

5. Lõpuks kehtivad sätted.

    `New-MsolSettings –SettingsObject $setting`

    ![Kasutus suunis URL-i lisamine](./media/active-directory-accessmanagement-groups-settings-cmdlets/add-usage-guideline-url.png)

Siin on määratletud Group.Unified SettingsTemplate sätteid.

 **Säte**                          | **Kirjeldus**                                                                                             
--------------------------------------|-----------------------------------------------
 <ul><li>ClassificationList<li>Tüüp: String<li>Vaikimisi: ""                  | Komaga eraldatud loend kehtiv liigitamine väärtused, mida saab rakendada ühendatud rühmad.                
 <ul><li>EnableGroupCreation<li>Tüüp: kahendmuutujaga<li>Vaikimisi: True              | Lipu, mis näitab, kas ühendatud rühma loomine on lubatud kataloogis.                               
 <ul><li>GroupCreationAllowedGroupId<li>Tüüp: String<li>Vaikimisi: ""         | GUID turberühma, mis on lubatud ühendatud rühmad luua ka siis, kui EnableGroupCreation == väär.
 <ul><li>UsageGuidelinesUrl<li>Tüüp: String<li>Vaikimisi: ""                  | Rühma kasutus juhiste link.                                                                       

## <a name="read-settings-at-the-directory-level"></a>Lugege tasemel kataloogi sätted

Neid juhiseid lugeda directory tasemel, sätted, mis kehtivad kõigi Office rühmade kataloogis.

1. Lugege kõik olemasolevad kataloogi sätted.

    `Get-MsolAllSettings`

2. Lugege kindla kõik sätted.

    `Get-MsolAllSettings -TargetType Groups -TargetObjectId <groupObjectId>`

3. Vt teatud kataloogi sätted, kasutades SettingId GUID:

    `Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

    ![Sätete ID GUID](./media/active-directory-accessmanagement-groups-settings-cmdlets/settings-id-guid.png)

## <a name="update-settings-at-the-directory-level"></a>Directory tasemel sätete värskendamine

Need juhised värskendada directory tasemel, sätted, mis kehtivad kõigi Office rühmade kataloogis.

1. Olemasoleva sätted objekt saamine

    `$setting = Get-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

2. Kuvada väärtus, mida soovite värskendada.

    `$value = $Setting.GetSettingsValue()`

3. Väärtuse värskendamiseks tehke järgmist.

    `$value["AllowToAddGuests"] = "false"`

4. Selle sätte värskendamiseks tehke järgmist.

    `Set-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c –SettingsValue $value`

## <a name="remove-settings-at-the-directory-level"></a>Directory tasemel sätete eemaldamine

See toiming eemaldab directory tasemel, sätted, mis kehtivad kõigi Office rühmade kataloogis.

    `Remove-MsolSettings –SettingId dbbcb0ea-a6ff-4b44-a1f3-9d7cef74984c`

## <a name="cmdlet-syntax-reference"></a>Cmdlet-käsk süntaksiviide

[Azure Active Directory cmdlet-käsud](http://go.microsoft.com/fwlink/p/?LinkId=808260)leiate Azure'i Active Directory PowerShelli rohkem dokumente.

## <a name="settingstemplate-object-reference-groupunified-settingstemplate-object"></a>SettingsTemplate objekti viide (Group.Unified SettingsTemplate objekt)

- "nimi": "EnableGroupCreation", "tüüp": "System.Boolean", "defaultValue": "true", "kirjeldus": "Kahendmuutujaga lipp näitab, kui on ühendatud rühma loomise funktsioon sisse lülitatud."

- "nimi": "GroupCreationAllowedGroupId", "tüüp": "System.Guid", "defaultValue": "", "kirjeldus": "GUID turberühma, mis on ühendatud rühmad luua whitelisted."

- "nimi": "ClassificationList", "tüüp": "System.String", "defaultValue": "", "kirjeldus": "Komaga eraldatud loend kehtiv liigitamine väärtused, mida saab rakendada ühendatud rühmad."

- "nimi": "UsageGuidelinesUrl", "tüüp": "System.String", "defaultValue": "", "kirjeldus": "Link rühma kasutus juhistest."

Nimi | tüüp | defaultValue | kirjeldus
----------  | ----------  | ---------  | ----------
"EnableGroupCreation"  | "System.Boolean"  | "true"  | "Kahendmuutujaga lipp näitab, kui on ühendatud rühma loomise funktsioon sisse lülitatud."
"GroupCreationAllowedGroupId"  | "System.Guid"  | ""  | "GUID turberühma, mis on ühendatud rühmad luua whitelisted."
"ClassificationList"  | "System.String"  | ""  | "Komaga eraldatud loend kehtiv liigitamine väärtused, mida saab rakendada ühendatud rühmad."
"UsageGuidelinesUrl"  | "System.String"  | ""  | "Link rühma kasutus juhistest."

## <a name="next-steps"></a>Järgmised sammud

[Azure Active Directory cmdlet-käsud](http://go.microsoft.com/fwlink/p/?LinkId=808260)leiate Azure'i Active Directory PowerShelli rohkem dokumente.

Täiendavad juhiseid Microsoft programmi juht Rob de Jong on saadaval [Rob's rühmade ajaveebi](http://robsgroupsblog.com/blog/configuring-settings-for-office-365-groups-in-azure-ad).

* [Azure Active Directory levirühmadega ressursid juurdepääsu haldamine](active-directory-manage-groups.md)

* [Teie kohapealse identiteetide integreerimine Azure Active Directory](active-directory-aadconnect.md)
