<properties
    pageTitle="Azure'i MFA ja AD FS-i | Microsoft Azure'i"
    description="See on Azure mitmekordne autentimine leht, mis kirjeldab, kuidas alustada Azure'i MFA ja AD FS-i."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na" ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-and-active-directory-federation-services"></a>Azure'i Mitmikautentimise ja Active Directory Federation Services töötamise alustamine



<center>![Pilveteenuse](./media/multi-factor-authentication-get-started-adfs/adfs.png)</center>

Kui teie asutus on ühendatud teie kohapealse Active Directory Azure Active Directory AD FS-i abil, on kaks võimalust Azure'i Mitmikautentimise kasutamise kohta.

- Turvaliste pilve ressursse, mis on Azure Mitmikautentimise või Active Directory Federation Services abil
- Turvaline pilve ja asutusesisese ressursse, mis on Azure mitmekordne autentimine serveri kasutamine

Järgmises tabelis on kokkuvõte kontrollimise kasutuskogemuse vahel turvaliseks Azure'i Mitmikautentimise ja AD FS-i ressursid

|Kontrollimise kasutamine – Sirvi-põhiste rakendused|Kontrollimise kasutamine – brauseri-põhiste rakendused
:------------- | :------------- | :------------- |
Azure'i Mitmikautentimise abil turvamise Azure AD ressursid |<li>Sooritatakse kinnitamine esmalt kohapealse AD FS-i abil.</li> <li>Teise etapi on pilv autentimist kasutades telefonipõhist meetod.</li>|Lõppkasutajad saate kasutada rakenduse paroolid sisse logima.
Active Directory Federation Services abil turvamise Azure AD ressursid |<li>Kõigepealt kontrollimise sooritatakse asutusesisese AD FS-i abil.</li><li>Teise etapi on tehtud kohapealse austus nõue.</li>|Lõppkasutajad saate kasutada rakenduse paroolid sisse logima.

Välise kasutajate jaoks rakenduse paroolidega piirangute:

- Rakenduse paroolid on kinnitatud cloud autentimist, kasutades nii, et nad mööduda federation. Federation kasutatakse ainult aktiivselt rakenduse parooli häälestamisel.
- Kohapealse kliendi juurdepääsu reguleerimine sätted on au rakenduse paroolid.
- Sellega kaotate kohapealse autentimis-logimise võimalus rakenduse paroole.
- Konto keelata/kustutamine võib kuluda kuni kolm tundi kataloogi sünkroonimine, keelata/kustutamise rakenduse paroolid cloud identiteedi edasi lükata.

## <a name="next-steps"></a>Järgmised sammud

Lisateavet Azure Mitmikautentimise või Azure mitme teguriga autentimine Server AD FS-i häälestamise kohta leiate järgmistest artiklitest:

- [Turvaline cloud ressursid Azure'i Mitmikautentimise ja AD FS-i abil](multi-factor-authentication-get-started-adfs-cloud.md)
- [Turvaline asutusesisese ja pilvepõhise ressursse, mis on Windows Server 2012 R2 AD FS Azure'i mitme teguriga autentimine serveri kasutamine](multi-factor-authentication-get-started-adfs-w2k12.md)
- [Turvaline pilve ja asutusesisese ressursse AD FS 2.0 Azure'i mitme teguriga autentimine serveri kasutamine](multi-factor-authentication-get-started-adfs-adfs2.md)
