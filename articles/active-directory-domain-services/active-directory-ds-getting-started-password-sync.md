<properties
    pageTitle="Azure'i AD domeeniteenused: Luba parooli sünkroonimise | Microsoft Azure'i"
    description="Azure Active Directory domeeniteenused töötamise alustamine"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>Azure'i AD domeeniteenused parooli sünkroonimise lubamine
Ülesanded, saate lubada Azure AD domeeniteenused oma Azure AD rentniku jaoks. Järgmise ülesande on nõutav Azure AD domeeni teenuste sünkroonimine Kerberose ja NTLM autentimine mandaati hashes anda. Pärast mandaadi sünkroonimine on häälestatud, kasutajad saavad sisse logida hallatava domeeni ettevõtte mandaadi abil.

Seotud toimingud on erinevad vastavalt kas teie ettevõttel on vaid Azure AD rentniku või kohapealse kataloogi sünkroonimine on seatud kasutab Azure'i AD-ühenduse.

<br>

> [AZURE.SELECTOR]
- [Vaid Azure AD rentniku](active-directory-ds-getting-started-password-sync.md)
- [Sünkroonitud Azure AD rentniku](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-cloud-only-azure-ad-tenant"></a>Ülesanne 5: Luba parooli sünkroonimine AAD domeeniteenused vaid Azure AD rentniku
Azure'i AD domeeniteenused peab mandaadi hashes Kerberose ja NTLM autentimiseks hallatava domeeni kasutajate autentimiseks sobivas vormingus. Juhul, kui teie rentniku AAD domeeniteenused lubamiseks Azure AD luua või talletada mandaati hashes vormingus vaja NTLM või Kerberose autentimist. Selge turvalisuse põhjustel Azure AD ka hoidke mis tahes identimisteabe tühjendage tekstina. Seetõttu ei ole Azure AD viis nende NTLM või Kerberose mandaati hashes kasutajate olemasoleva mandaadi põhjal luua.

> [AZURE.NOTE] Kui teie ettevõte kasutab mõnda vaid Azure AD rentniku kasutajatele, kes peavad Azure AD domeeni teenuste kasutamiseks peate oma parooli muuta.

Importimistoimingut parooli muutmine põhjustab mandaadi hashes nõutud Azure AD domeeni teenused genereeritakse Azure AD Kerberose ja NTLM-autentimist. Te saate kas rentniku, mis tuleb kasutada Azure AD domeeniteenused või paluge need kasutajad oma parooli muutma kõigi kasutajate paroolide aegumise.


### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>Lubage Kerberose ja NTLM mandaati räsi genereerimine vaid Azure AD rentniku
Siin on juhiseid tuleb sisestada lõppkasutajad, saate oma parooli muutma.

1. Ettevõtte veebisaidil [http://myapps.microsoft.com](http://myapps.microsoft.com)Azure AD kasutada paneeli lehele liikumiseks.

2. Valige selle lehe vahekaardi **profiil** .

3. Klõpsake selle lehe paani **parooli muutmine** .

    ![Luua virtuaalse võrgu Azure AD domeeni teenuste jaoks.](./media/active-directory-domain-services-getting-started/user-change-password.png)

    > [AZURE.NOTE] Kui te ei näe lehel Access Panel suvand **parooli muutmine** , veenduge, et teie organisatsioon on konfigureerinud [Azure AD parooli haldus](../active-directory/active-directory-passwords-getting-started.md).

4. **Parooli muutmine** lehel sisestage oma olemasoleva (vana) parool ja seejärel tippige uus parool ja kinnitage see. Klõpsake **esitada**.

    ![Saate luua virtuaalse võrgu Azure AD domeeni teenuste jaoks.](./media/active-directory-domain-services-getting-started/user-change-password2.png)

Kui olete parooli muutnud, uus parool on peagi kasutuskõlblikud Azure AD domeeniteenused. Mõne minuti pärast (tavaliselt umbes 20 minutit), saate arvutite ühendatud hallatava domeeni äsja muudetud parooliga sisse logida.

<br>

## <a name="related-content"></a>Seotud teemad

- [Kuidas värskendada oma parooli](../active-directory/active-directory-passwords-update-your-own-password.md)

- [Alustamine: parooli halduse Azure AD](../active-directory/active-directory-passwords-getting-started.md).

- [Luba parooli sünkroonimine AAD domeeniteenused sünkroonitud Azure AD rentniku](active-directory-ds-getting-started-password-sync-synced-tenant.md)

- [Mõni Azure AD domeeniteenused hallatava domeeni haldamine](active-directory-ds-admin-guide-administer-domain.md)

- [Liitumine virtuaalse masina Windows Azure AD domeeniteenused hallatava domeeni](active-directory-ds-admin-guide-join-windows-vm.md)

- [Azure'i AD domeeniteenused hallatava domeeni punane rolli Enterprise Linux virtuaalse masina liitumine](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
