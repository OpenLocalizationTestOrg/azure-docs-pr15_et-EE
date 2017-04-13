<properties
    pageTitle="Saada alustamine Azure'i MFA pilveteenuses | Microsoft Azure'i"
    description="See on Microsoft Azure'i mitmekordne autentimine leht, mis kirjeldab, kuidas alustada Azure'i MFA pilveteenuses."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>Alustamine: Azure'i Mitmikautentimise pilveteenuses
Selles artiklis tutvustatakse, kuidas alustada, kasutades Azure Mitmikautentimise pilveteenuses.

> [AZURE.NOTE]  Järgmised dokumendid teave kohta, kuidas kasutajad saavad **Azure klassikaline portaali**. Kui otsite teavet selle kohta, kuidas häälestada Azure'i Mitmikautentimise O365 kasutajate jaoks, lugege teemat [häälestamine Office 365 mitmikautentimise.](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![MFA pilveteenuses](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisites"></a>Eeltingimused
Järgmine kohustuslik tarkvara on nõutavad, enne kui saate lubada Azure Mitmikautentimise kasutajate jaoks.


1. [Azure tellimuse kasutajaks](https://azure.microsoft.com/pricing/free-trial/) – kui teil juba on Azure'i tellimus, tuleb teil sisse logida mõne. Kui olete just tööd alustamas ja Azure MFA kasutamine kasutate prooviversiooni tellimuse
2. [Loo mitme teguri Auth pakkuja](multi-factor-authentication-get-started-auth-provider.md) ja määrata kataloogi või [kasutajatele litsentside määramine](multi-factor-authentication-get-started-assign-licenses.md)

> [AZURE.NOTE]  Litsentsid on saadaval kasutajatele, kellel on Azure MFA, Azure AD Premium või ettevõtte mobiilsus komplekti (EMS).  MFA sisaldub Azure AD Premium ja keskkonnajuhtimissüsteemi. Kui teil on piisavalt litsentse, peate looma ka Auth pakkuja.


## <a name="turn-on-two-step-verification-for-users"></a>Lülitage sisse kaheastmelise kontrollimise kasutajate jaoks
Seda, kuidas nõuda kahepoolse alustada kinnitamine klõpsake kasutaja alustamiseks muuta kasutaja olekus keelatud lubada.  Kasutaja kohta lisateabe saamiseks lugege teemat [Azure Mitmikautentimise kasutaja riikide](multi-factor-authentication-get-started-user-states.md)

Järgmiste toimingute abil saate lubada MFA kasutajate jaoks.

### <a name="to-turn-on-multi-factor-authentication"></a>Mitmikautentimise sisselülitamine

1.  Logige sisse [Azure klassikaline portaali](https://manage.windowsazure.com) administraatorina.
2.  Klõpsake vasakul nuppu **Active Directory**.
3.  Valige jaotises kataloog kataloog soovite lubada kasutaja jaoks.
![Klõpsake nuppu kataloog](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  Ülaosas nuppu **Kasutajad**.
5.  Klõpsake lehe allosas nuppu **Hallata mitut tegurit Auth**. Avatakse brauseris uus vahekaart.
![Klõpsake nimistust](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  Leidke kasutaja, mida soovite lubamine kaheastmelise kontrollimise. Kui peate vaate ülaosas. Veenduge, et olek on **keelatud.** 
 ![Luba kasutaja](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  **Märkige ruut** paigutamiseks tema nime kõrval olev ruut.
7.  Klõpsake parempoolsel paanil nuppu **Luba**.
![Kasutaja lubamine](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  Klõpsake nuppu **Luba mitme teguri auth**.
![Kasutaja lubamine](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  Pange tähele, et kasutaja on muutunud **keelatud** , **lubatud**.
![Lubamine](./media/multi-factor-authentication-get-started-cloud/user.png)

Kui teil on lubatud teie kasutajad, tuleb teil sellest need e-posti teel. Need järgmine kord, kui nad proovige sisse logida, palutakse teil registreerida oma konto kaheastmelise kontrollimise. Kui nad kasutuselevõtt kaheastmelise kontrollimise, peavad nad ka häälestada rakenduse paroolid vältimiseks on lukus,-brauseri rakendused.


## <a name="use-powershell-to-automate-turning-on-two-step-verification"></a>PowerShelli kasutamine automatiseerimiseks kaheastmelise kontrollimise sisselülitamine

[Riigi](multi-factor-authentication-whats-next.md) [Azure AD PowerShelli](../powershell-install-configure.md)kaudu muutmiseks saate kasutada järgmist.  Saate muuta `$st.State` võrdub ühte järgmistest:

- Lubatud
- Jõustatud
- Keelatud  

> [AZURE.IMPORTANT]  Me takistada vastu otse keelata oleku kasutajate üleviimine sundkorras olek. Mitte-brauseripõhiste rakenduste seisma kuna kasutaja on MFA registreerimise kaudu läinud ja saadud on [rakenduse parooli](multi-factor-authentication-whats-next.md#app-passwords). Kui teil on brauseris-põhiste rakenduste ja rakenduse parooli küsimiseks, soovitame minge olekust keelatud lubatud. See võimaldab kasutajatel registreerida ja paroolide rakenduse hankimine. Pärast seda, mida saab teisaldada sundkorras.

PowerShelli kasutamine oleks hulgi, mis võimaldab kasutajatel soovitud suvand. Praegu ei ole hulgi luba funktsioon Azure portaali ja peate valima iga kasutaja ükshaaval. Kui teil on palju kasutajat, võib see olla päris tööülesande. Loomisega lisamine PowerShelli skripti kaudu järgmist, saate kasutajate loendit kaudu pidev ja lubada neid.

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

Siin on näide:

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


Lisateabe saamiseks lugege teemat [Azure Mitmikautentimise kasutaja riikide](multi-factor-authentication-get-started-user-states.md)

## <a name="next-steps"></a>Järgmised sammud
Nüüd, kui olete loonud Azure'i Mitmikautentimise pilves, saate konfigureerida ja häälestada oma juurutuse. Üksikasjalikumat teavet teemast [Konfigureerimise Azure'i Mitmikautentimise](multi-factor-authentication-whats-next.md) .
