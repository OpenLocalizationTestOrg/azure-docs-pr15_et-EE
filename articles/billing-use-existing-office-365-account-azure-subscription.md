<properties
    pageTitle="Jagada ühe Azure AD rentniku Office 365 ja Azure tellimustes | Microsoft Azure'i"
    description="Siit saate teada, kuidas oma Office 365 Azure AD rentniku ja selle kasutajad Azure tellimuse ühiskasutusse anda ja vastupidi"
    services=""
    documentationCenter=""
    authors="JiangChen79"
    manager="mbaldwin"
    editor=""
    tags="billing,top-support-issue"/>

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="ibiza"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/17/2016"
    ms.author="cjiang"/>

# <a name="use-an-existing-office-365-account-with-your-azure-subscription-or-vice-versa"></a>Kasutage olemasoleva Office 365 konto Azure'i tellimus ja vastupidi
Stsenaarium: Teil juba on Office 365 tellimus ning olete valmis Azure tellimuse kasutajaks, kuid soovite kasutada olemasoleva Office 365 kasutajakontode Azure tellimuse. Teise võimalusena on mõni Azure abonendi ja soovite saada Office 365 tellimuse kasutajate jaoks oma olemasoleva Azure Active Directory. Selles artiklis kirjeldatakse, kui lihtne on mõlemad saavutamiseks.

> [AZURE.NOTE] See artikkel ei kehti Enterprise lepingu (EA) klientidele. Kui vajate rohkem abi, mis tahes hetkel selle artikli, [pöörduge klienditoe poole](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) saada teie probleemi lahendada kiiresti.


## <a name="quick-guidance"></a>Kiirülevaate juhised

- Kui teil juba on teenusekomplekti Office 365 tellimust ja soovite registreeruda Azure, kasutage suvand **logige sisse oma ettevõtte konto** . Seejärel jätkake Azure registreerumisprotsessi Office 365 kontoga. [Üksikasjalikud juhised selle artikli](#s1)teemast.

- Kui teil juba on Azure tellimuse ja soovite saada Office 365 tellimuse, logige sisse Office 365 Azure kontoga. Jätkake registreerumise juhiseid. Pärast registreerumist soovitud, lisatakse sama Azure Active Directory eksemplar, mis kuulub Azure tellimuse Office 365 tellimust. Lisateavet leiate teemast [üksikasjalikud juhised selle artikli](#s2)jaotist.

>[AZURE.NOTE] Office 365 tellimuse saamiseks kontot kasutate registreerumise peate olema üld- või administraator arveldus directory rolli teie Azure Active Directory rentniku liige. [Siit saate teada, kuidas määrata rolli Azure Active Directory](#how-to-know-your-role-in-your-azure-active-directory).

Mis juhtub, kui lisate tellimuse konto, vt [selle artikli taustateabe](#background-information).

## <a name="detailed-steps"></a>Üksikasjalikud juhised
<a id="s1"></a>
### <a name="scenario-1-office-365-users-who-plan-to-buy-azure"></a>Stsenaarium 1: Office 365 kasutajatele, kes kavatsete osta Azure
Selle stsenaariumi korral me endale Kelley seina on kasutaja, kellel on Office 365 tellimuse ning kavandab Azure'i tellida. On kaks täiendavad aktiivsed kasutajad, Aile ja Tricia. Kelley tema konto on admin@contoso.onmicrosoft.com.

![Office 365 kasutaja halduskeskus](./media/billing-use-existing-office-365-account-azure-subscription/1-office365-users-admin-center.png)

Azure'i kasutajaks, toimige järgmiselt.

1. Azure'i veebisaidil [Azure.com](https://azure.microsoft.com/)kasutajaks registreerumist. Klõpsake nuppu **Proovi tasuta**. Järgmisel lehel klõpsake nuppu **Alusta kohe**.

    ![Proovige Azure'i tasuta.](./media/billing-use-existing-office-365-account-azure-subscription/2-azure-signup-try-free.png)

2. Valige **logige sisse oma ettevõtte konto**.

    ![Azure'i sisse logida.](./media/billing-use-existing-office-365-account-azure-subscription/3-sign-in-to-azure.png)

3. Logige sisse oma Office 365 konto. Sel juhul on Kelley kasutaja Office 365 konto.

    ![Logige sisse oma Office 365 konto.](./media/billing-use-existing-office-365-account-azure-subscription/4-sign-in-with-org-account.png)

4. Sisestage teave ja Registreeruge.

    ![Sisestage teave ja kasutajaks registreerumist.](./media/billing-use-existing-office-365-account-azure-subscription/5-azure-sign-up-fill-information.png)

    ![Klõpsake nuppu Käivita minu teenuse haldamine.](./media/billing-use-existing-office-365-account-azure-subscription/6-azure-start-managing-my-service.png)

Nüüd olete valmis. Azure'i portaalis, peaksite nägema kuvataks sama kasutajad. Selle probleemi lahendamiseks tehke järgmist.

1. Klõpsake ekraani kasutada eelnevalt **hakata haldama oma teenuse** .
2. Klõpsake nuppu **Sirvi**ja seejärel klõpsake nuppu **Active Directory**.

    ![Klõpsake nuppu Sirvi ja seejärel klõpsake nuppu Active Directory.](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Klõpsake nuppu **Kasutajad**.

    ![Menüü kasutajad](./media/billing-use-existing-office-365-account-azure-subscription/8-azure-portal-ad-users-tab.png)

4. Kõik kasutajad, sh Kelley, on loetletud ootuspäraselt.

    ![Kasutajate loend](./media/billing-use-existing-office-365-account-azure-subscription/9-azure-portal-ad-users.png)

<a id="s2"></a>
### <a name="scenario-2-azure-users-who-plan-to-buy-office-365"></a>Stsenaarium 2: Azure'i kasutajatele, kes kavatsete osta teenusekomplekti Office 365

Selle stsenaariumi korral Kelley seina on kasutaja, kes on Azure tellimuse konto admin@contoso.onmicrosoft.com. Kelley soovib Office 365 tellida ja kasutada ta on juba Azure'i samas kaustas.

>[AZURE.NOTE] Office 365 tellimuse saamiseks sisselogimise jaoks kasutatava konto peab olema üld- või administraator arveldus directory rolli teie Azure Active Directory rentniku liige. [Saate teada, kuidas leida Azure Active Directory roll](#how-to-know-your-role-in-your-azure-active-directory).

![Azure'i portaali tellimuse sätted](./media/billing-use-existing-office-365-account-azure-subscription/10-azure-portal-settings-subscription.png)

![Azure Active Directory kasutajate](./media/billing-use-existing-office-365-account-azure-subscription/11-azure-portal-ads-users.png)

Office 365 tellimine, toimige järgmiselt.

1. Minge [lehele Office 365 toodet](https://products.office.com/business)ja valige leping, millele teile sobib.
2. Kui olete valinud leping, kuvatakse järgmine leht. Täitke vorm. Klõpsake lehe paremas ülanurgas nuppu **sisse logida** .

    ![Office 365 prooviversiooni leht](./media/billing-use-existing-office-365-account-azure-subscription/12-office-365-trial-page.png)

3. Logige sisse oma konto identimisteave. Selles näites on Kelley tema konto.

    ![Office 365 sisselogimine](./media/billing-use-existing-office-365-account-azure-subscription/13-office-365-sign-in.png)

4. Klõpsake nuppu **Proovige kohe**.

    ![Office 365 tellimuse.](./media/billing-use-existing-office-365-account-azure-subscription/14-office-365-confirm-your-order.png)

5. Klõpsake lehel tellimuse teatis nuppu **Jätka**.

    ![Office 365 tellimuse teatis](./media/billing-use-existing-office-365-account-azure-subscription/15-office-365-order-receipt.png)

Nüüd olete valmis. Office 365 halduskeskuse kaudu, peaksite nägema kasutajate Contoso kataloogist, kuvades aktiivsed kasutajad. Selle probleemi lahendamiseks tehke järgmist.

1. Avage Office 365 halduskeskus.
2. Laiendage **Kasutajad**ja klõpsake käsku **Aktiivsed kasutajad**.

    ![Office 365 admin center kasutajatele](./media/billing-use-existing-office-365-account-azure-subscription/16-office-365-admin-center-users.png)

### <a name="how-to-know-your-role-in-your-azure-active-directory"></a>Kuidas leida oma Azure Active Directory teie roll

1. [Azure'i portaali](https://portal.azure.com/)sisse logida.
2. Klõpsake nuppu **Sirvi**ja seejärel klõpsake nuppu **Active Directory**.

    ![Portaalis Azure Active Directory](./media/billing-use-existing-office-365-account-azure-subscription/7-azure-portal-browse-ad.png)

3. Klõpsake nuppu **Kasutajad**.

    ![Azure'i portaali vaikimisi Active Directory kasutajad](./media/billing-use-existing-office-365-account-azure-subscription/17-azure-portal-default-ad-users.png)

4. Klõpsake selle kasutaja. Selles näites on kasutaja Kelley seina.

    Pange tähele **Ettevõtte rolli**välja.

    ![Azure'i portaali kasutaja identiteet](./media/billing-use-existing-office-365-account-azure-subscription/18-azure-portal-user-identity.png)

## <a name="background-information-about-azure-and-office-365-subscriptions"></a>Azure'i ja Office 365 tellimuste kohta taustateabe
Office 365 ja Azure Azure Active Directory teenuse abil saate hallata kasutajate ja tellimused. Lugeda ka Azure'i directory ümbris, kus saate rühmitada kasutajaid ja tellimused. Azure'i ja Office 365 tellimuste sama kasutajakonto kasutamiseks peate veenduge, et tellimused on loodud samas kaustas. Pidage meeles järgmist:

- Tellimuse saab luua kaust, mitte teistpidi.
- Kataloogide, mitte teistpidi kuuluvad kasutajad.
- Tellimuse maandub kataloogis kasutaja, kes loob tellimus. Selle tulemusena Office 365 tellimuse on sama kontoga seotud nimega Azure tellimuse kui selle konto abil saate luua Office 365 tellimus.

![Taustateabe](./media/billing-use-existing-office-365-account-azure-subscription/19-background-information.png)

Lisateabe saamiseks lugege teemat [Kuidas Azure'i tellimused on seostatud Azure Active Directory](./active-directory/active-directory-how-subscriptions-associated-directory.md).

>[AZURE.NOTE] Azure'i tellimused kuuluvad üksikkasutajate kataloogis.

>[AZURE.NOTE] Office 365 tellimuste kuuluvad kataloogi ise. Kui kasutajad kataloogi raames on vajalikud õigused, töötavad need tellimuste kohta.

## <a name="next-steps"></a>Järgmised sammud
Ostsite Azure ja Office 365 tellimuste eraldi varem, kui soovite juurdepääsu Office 365 rentniku Azure tellimusest, vt [Office 365 rentniku Azure tellimusega seostada](billing-add-office-365-tenant-to-azure-subscription.md).

> [AZURE.NOTE] Kui teil on endiselt küsimusi, [pöörduge abi](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) saamiseks oma probleemi lahendada kiiresti.
