<properties
    pageTitle="Parooli aegumise poliitika määramine Azure Active Directory | Microsoft Azure'i"
    description="Saate teada, kuidas kontrollida aegumise poliitika ja muuta kasutaja parooli aegumise eraldi või Azure Active directory paroolide hulgi"
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
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="set-password-expiration-policies-in-azure-active-directory"></a>Parooli aegumise poliitika määramine Azure Active Directory

> [AZURE.IMPORTANT] **Kas olete siin, sest teil on sisselogimisega probleeme?** Kui, siis [siin on, kuidas saate muuta ja enda parool lähtestada](active-directory-passwords-update-your-own-password.md).

Nimega Microsofti pilveteenuses üldadministraator, saate selle Microsoft Azure Active Directory moodul Windows PowerShelli jaoks häälestada kasutaja paroolid ei aeguks. Eemaldamiseks saate kasutada ka Windows PowerShelli cmdlet-käsud on kunagi-lõpeb konfiguratsiooni või kasutaja paroolid on seadistatud ei aeguks. Sellest artiklist leiate abi pilveteenustega, nt Microsoft Intune'i ja Office 365, kes kasutavad Microsoft Azure Active Directory jaoks identiteedi ja kataloogiteenused.

  > [AZURE.NOTE] Ainult paroolide Kasutajakontod, mida pole sünkroonitakse Kataloogisünkroonimise kaudu saab konfigureerida ei aeguks. Kataloogisünkroonimise kohta leiate lisateavet teemast [Kataloogisünkroonimise teejuht](https://msdn.microsoft.com/library/azure/hh967642.aspx)teemade loend.

Windows PowerShelli cmdlet-käskude kasutamiseks esmalt installige need.

## <a name="what-do-you-want-to-do"></a>Mida soovite teha?

- [Kuidas saan kontrollida, parooli aegumise poliitika](#how-to-check-expiration-policy-for-a-password)

- [Parooli aegumise lubamine](#set-a-password-to-expire)

- [Nii, et see ei lõpe parooli seadmine](#set-a-password-to-never-expire)

## <a name="how-to-check-expiration-policy-for-a-password"></a>Kuidas saan kontrollida, parooli aegumise poliitika

1.  Ühenduse loomine Windows PowerShelli abil oma ettevõtte administraatori identimisteave.

2.  Tehke ühte järgmistest.

    - Kas on määratud ühe kasutaja parooli aegumatuks kuvamiseks käivitage järgmine cmdlet kasutaja turvasubjektinimi (UPN) abil (nt aprilr@contoso.onmicrosoft.com) või kasutaja ID, mida soovite kontrollida kasutaja:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`

    - Kõigi kasutajate sätte "Parool kunagi lõpeb" kuvamiseks käivitage järgmine cmdlet-käsk:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

## <a name="set-a-password-to-expire"></a>Parooli aegumise lubamine

1.  Ühenduse loomine Windows PowerShelli abil oma ettevõtte administraatori identimisteave.

2.  Tehke ühte järgmistest.

    - Parool aegub ühe kasutaja parooli seadmiseks käivitage järgmine cmdlet selle kasutaja turvasubjektinimi (UPN) abil või kasutaja kasutaja ID-d:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`

    - Kõigi kasutajate paroolide seadmiseks organisatsioonis nii, et nad aegub kasutage järgmine cmdlet-käsk:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

## <a name="set-a-password-to-never-expire"></a>Parooli muutmine aegumatuks

1. Ühenduse loomine Windows PowerShelli abil oma ettevõtte administraatori identimisteave.

2.  Tehke ühte järgmistest.

    - Ühe kasutaja aegumatu parooli seadmiseks käivitage järgmine cmdlet kasutaja turvasubjektinimi (UPN) abil või kasutaja kasutaja ID-d:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`

    - Aegumatu ettevõtte kõigi kasutajate paroolide seadmiseks käivitage järgmine cmdlet-käsk:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>Järgmised sammud

* **Kas olete siin, sest teil on sisselogimisega probleeme?** Kui, siis [siin on, kuidas saate muuta ja enda parool lähtestada](active-directory-passwords-update-your-own-password.md).
