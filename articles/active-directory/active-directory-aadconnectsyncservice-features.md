<properties
    pageTitle="Azure'i AD-ühendus sünkroonimise teenuse funktsioonid ja konfiguratsiooni | Microsoft Azure'i"
    description="Kirjeldatakse teenuse küljel funktsioonid Azure'i AD-ühenduse sünkroonimise teenuse jaoks."
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
    ms.date="08/22/2016"
    ms.author="andkjell;markvi"/>

# <a name="azure-ad-connect-sync-service-features"></a>Azure'i AD-ühendus sünkroonimise teenuse funktsioonid

Azure'i AD-ühenduse sünkroonimise funktsioon koosneb kahest osast:

- Kohapealse komponent nimega **sünkroonimine Azure'i AD-ühenduse**, nimetatakse ka **sünkroonimine otsimootori**.
- Teenuse elavate Azure AD ka **Azure'i AD-ühenduse sünkroonimise teenuse**

See teema selgitab, kuidas **Azure'i AD-ühenduse sünkroonida teenuse** järgmised funktsioonid töötavad ja kuidas saate konfigureerida neid Windows PowerShelli kaudu.

Need sätted on konfigureeritud [Azure Active Directory moodul Windows PowerShelli jaoks](http://aka.ms/aadposh). Alla laadida ja installida eraldi Azure'i AD-ühenduse kaudu. Selles teemas dokumenteerida cmdlet-käskude võeti [2016 märts versioon (järk 9031.1)](http://social.technet.microsoft.com/wiki/contents/articles/28552.microsoft-azure-active-directory-powershell-module-version-release-history.aspx#Version_9031_1). Kui teil pole selles teemas dokumenteerida cmdlet-käskude või need pole sama tulemuse, siis veenduge, et käivitate uusim versioon.

Azure AD kataloogis konfiguratsiooni kuvamiseks käivitage `Get-MsolDirSyncFeatures`.  
![Get-MsolDirSyncFeatures tulem](./media/active-directory-aadconnectsyncservice-features/getmsoldirsyncfeatures.png)

Paljud siin mainitud sätted saate muuta ainult Azure'i AD-ühenduse.

Saate konfigureerida järgmisi sätteid `Set-MsolDirSyncFeature`:

DirSyncFeature | Kommentaari
--- | ---
[DuplicateProxyAddressResiliency<br/>DuplicateUPNResiliency](#duplicate-attribute-resiliency) | Võimaldab atribuudi abil karantiini, kui see on duplikaat mõne muu objekti, mitte ei ole kogu objekti ekspordi ajal.
[EnableSoftMatchOnUpn](#userprincipalname-soft-match) | Võimaldab objektide liituda userPrincipalName Lisaks esmane SMTP-aadress.
[SynchronizeUpnForManagedUsers](#synchronize-userprincipalname-updates) | Lubab värskendamine õnnestus/litsentsitud (ühendatud) kasutajate jaoks atribuudi userPrincipalName sünkroonimine.

Kui olete lubanud funktsiooni, ei saa uuesti keelata.

>[AZURE.NOTE] 24 August 2016 on funktsiooni *Dubleeri atribuut paindlikkust* vaikimisi uue Azure AD kataloogide. See funktsioon ka välja ja lubatud kataloogid loodud varasem kuupäev. Saate meiliteatise, kui kataloogi on umbes saada see funktsioon on sisse lülitatud.

Järgmised sätted on konfigureeritud Azure'i AD-ühenduse, ei saa muuta ning `Set-MsolDirSyncFeature`:

DirSyncFeature | Kommentaari
--- | ---
DeviceWriteback | [Azure'i AD-ühendus: Lubamine seadme tagasikirjutusega](active-directory-aadconnect-feature-device-writeback.md)
DirectoryExtensions | [Azure'i AD-ühendus sünkroonimine: Directory laiendid](active-directory-aadconnectsync-feature-directory-extensions.md)
PasswordSync | [Azure'i AD-ühenduse sünkroonimisprobleemide parooli sünkroonimise rakendamine](active-directory-aadconnectsync-implement-password-synchronization.md)
UnifiedGroupWriteback | [Eelvaade: Rühma tagasikirjutusega](active-directory-aadconnect-feature-preview.md#group-writeback)
UserWriteback | Praegu ei toetata.

## <a name="duplicate-attribute-resiliency"></a>Atribuut paindlikkust dubleerimine
Objektide asemel sätte probleemse dubleeritud UPN-ID / proxyAddresses, dubleeritakse atribuut on "pandud" ja ajutine väärtus on määratud. Kui konflikti on lahendatud, muudetakse ajutine UPN-i sobiva väärtuse automaatselt. Selline käitumine saab lubada UPN-i ja kasutajaatribuudis jaoks eraldi. Lisateabe saamiseks vt [identiteedi sünkroonimise ja dubleeritud atribuut paindlikkust](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md).

## <a name="userprincipalname-soft-match"></a>UserPrincipalName pehmete match
Kui see funktsioon on lubatud, sujuvate-match on lubatud UPN-i [esmane SMTP-aadress](https://support.microsoft.com/kb/2641663), mis on alati lubatud. Sujuvate-match kasutatakse vastavus pilveteenuses olemasolevatele kasutajatele Azure AD kohapealse kasutajatega.

Kui teil on vaja match kohapealse olemasolevaid kontosid loodud pilveteenuses AD kontod ja te ei kasuta Exchange Online'i, siis see funktsioon on kasulik. Selle stsenaariumi korral üldiselt ei pea põhjust määrata SMTP atribuut pilveteenuses.

See funktsioon on vaikimisi äsja loodud Azure AD kataloogide. Näete, kui see funktsioon on teie jaoks lubatud käivitades:  
```
Get-MsolDirSyncFeatures -Feature EnableSoftMatchOnUpn
```

Kui see funktsioon pole lubatud Azure AD kataloogi jaoks, siis saate lubada see töötab:  
```
Set-MsolDirSyncFeature -Feature EnableSoftMatchOnUpn -Enable $true
```

## <a name="synchronize-userprincipalname-updates"></a>UserPrincipalName värskendusi sünkroonida
Varem blokeeritud värskendused kohapealse sünkroonimise teenuse kasutamise atribuut UserPrincipalName, v.a juhul, kui mõlemad järgmised tingimused on täidetud:

- Kasutaja hallatakse (ühendatud).
- Kasutaja pole määratud litsentsi.

Leiate lisateavet teemast [Office 365, Azure, või Intune kasutajanimed ei ühti kohapealse UPN-i või alternatiivse sisselogimise ID](https://support.microsoft.com/kb/2523192).

Selle funktsiooni võimaldab sync engine värskendamiseks on userPrincipalName, kui see on muudetud asutusesiseselt ja kasutate parooli sünkroonimise. Kui kasutate federation, seda funktsiooni ei toetata.

See funktsioon on vaikimisi äsja loodud Azure AD kataloogide. Näete, kui see funktsioon on teie jaoks lubatud käivitades:  
```
Get-MsolDirSyncFeatures -Feature SynchronizeUpnForManagedUsers
```

Kui see funktsioon pole lubatud Azure AD kataloogi jaoks, siis saate lubada see töötab:  
```
Set-MsolDirSyncFeature -Feature SynchronizeUpnForManagedUsers -Enable $true
```

Pärast selle funktsiooni jäävad olemasolevate userPrincipalName väärtuste-on. Järgmise muutmise userPrincipalName atribuut asutusesisese, värskendatakse tavaline delta sünkroonimise kasutajatele UPN-i.  

## <a name="see-also"></a>Vt ka

- [Azure'i AD-ühendus sünkroonimine](active-directory-aadconnectsync-whatis.md)
- [Integreerimine Azure Active Directory oma kohapealse identiteedid](active-directory-aadconnect.md).
