<properties
    pageTitle="Kasutamine Office 365 rentniku Azure tellimuse | Microsoft Azure'i"
    description="Saate teada, kuidas lisada teenusekomplekti Office 365 directory (rentnik) Azure märkimiseks, et seost."
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
    ms.date="09/16/2016"
    ms.author="cjiang"/>

# <a name="associate-an-office-365-tenant-with-an-azure-subscription"></a>Office 365 rentniku seostada Azure tellimuse
Kui olete ostnud Office 365 ja Azure tellimuste eraldi varem ja nüüd soovite juurdepääsu Office 365 rentniku Azure tellimusest, on lihtne teha. Selles artiklis kirjeldatakse, kuidas.

> [AZURE.NOTE] See artikkel ei kehti Enterprise lepingu (EA) klientidele.

## <a name="quick-guidance"></a>Kiirülevaate juhised
Oma Office 365 rentniku seostamiseks Azure tellimuse Azure'i konto abil saate lisada oma Office 365 rentniku ja seejärel Office 365 rentniku Azure tellimuse seostada.

## <a name="detailed-steps"></a>Üksikasjalikud juhised
Selle stsenaariumi korral Kelley seina on kasutaja, kes on Azure tellimuse konto kelley.wall@outlook.com. Kelley on ka Office 365 tellimuse konto kelley.wall@contoso.onmicrosoft.com. Nüüd soovib Kelley juurdepääs Office 365 rentniku Azure'i tellimus.

![Kuvatõmmis Azure Active Directory olek](./media/billing-add-office-365-tenant-to-azure-subscription/s31_msa-aad-status.png)

![Kuvatõmmis Office 365 admin center aktiivsed kasutajad](./media/billing-add-office-365-tenant-to-azure-subscription/s32_office-365-user.png)

### <a name="prerequisites"></a>Eeltingimused
Seost töötaks õigesti, järgmine kohustuslik tarkvara on vajalikud:

- Teil on vaja teenuse administraator Azure tellimuse identimisteabe. Koostöö administraatorid ei saa käivitada alamhulga juhiseid.
- Teil on vaja Office 365 rentniku üldadministraator identimisteabe.
- E-posti aadressi teenuse administraator peavad sisaldama pole Office 365 rentnik.
- Meiliaadress, et teenuse administraator peab pole sama mis tahes üldadministraator Office 365 rentniku.
- Kui kasutate praegu on Microsofti konto ja organisatsioonikonto meiliaadressiga, ajutiselt muuta teenuse administraator Azure tellimuse kasutada mõne muu Microsofti kontoga. Saate luua uue Microsofti konto [Microsofti konto registreerumislehel](https://signup.live.com/).


Kui soovite muuta oma teenuse administraator, tehke järgmist.

1. Logige sisse [portaali konto haldamine](https://account.windowsazure.com/subscriptions).
2. Valige tellimus, mida soovite muuta.
3. Valige **Redigeeri Tellimuse üksikasjad**.

    ![Kuvatõmmis, Azure tellimuse teave "Redigeeri Tellimuse üksikasjad" esile tõstetud](./media/billing-add-office-365-tenant-to-azure-subscription/s33_azure-edit-subscription-details.png)

4. Sisestage väljale **Administraator** selle meilikonto aadress, uue teenuse administraator.

    ![Kuvatõmmis dialoogiboksist "Edit tellimuse"](./media/billing-add-office-365-tenant-to-azure-subscription/s34_change-subscription-service-admin.png)

### <a name="associate-the-office-365-tenant-with-the-azure-subscription"></a>Office 365 rentniku seostada Azure tellimuse
Office 365 rentniku seostada Azure tellimuse, toimige järgmiselt.

1.  Logige sisse [kontole haldusportaali](https://account.windowsazure.com/subscriptions) teenuse administraatori identimisteabega.
2.  Valige vasakul paanil **ACTIVE DIRECTORY**.

    ![Kuvatõmmis Active Directory kirje](./media/billing-add-office-365-tenant-to-azure-subscription/s35-classic-portal-active-directory-entry.png)

    > [AZURE.NOTE] Näha peaks olema Office 365 rentnik. Kui näete, vahele jätta järgmise juhise juurde.

    ![Azure Active Directory vaikekataloogi kuvatõmmis](./media/billing-add-office-365-tenant-to-azure-subscription/s36-aad-tenant-default.png)

3. Office 365 rentniku Azure tellimusse lisada.

    lisamine. Valige **Uus** > **DIRECTORY** > **kohandatud loomine**.

    ![Kuvatõmmis Azure Active Directory kohandatud loomine](./media/billing-add-office-365-tenant-to-azure-subscription/s37-aad-custom-create.png)

    b. Valige lehel **Lisa directory** **DIRECTORY**, **Kasutage olemasoleva kausta**. Seejärel valige **olen valmis nüüd välja logitud**, ja valige **valmis** ![lõpuleviimine ikooni](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

    ![Kuvatõmmis "Kasuta olemasoleva kausta"](./media/billing-add-office-365-tenant-to-azure-subscription/s39_add-directory-use-existing.png)

    c. Kui teil on sisse logitud, logige sisse oma Office 365 rentniku globaalne administraatori identimisteave.

    ![Kuvatõmmis Office 365 üldadministraator sisselogimine](./media/billing-add-office-365-tenant-to-azure-subscription/s310_sign-in-global-admin-office-365.png)

    d. Valige **Jätka**.

    ![Kuvatõmmis kontrollimine](./media/billing-add-office-365-tenant-to-azure-subscription/s311_use-contoso-directory-azure-verify.png)

    e. Valige **Logi välja kohe**.

    ![Kuvatõmmis väljunud](./media/billing-add-office-365-tenant-to-azure-subscription/s312_use-contoso-directory-azure-confirm-and-sign-out.png)

    f. Logige sisse [kontole haldusportaali](https://account.windowsazure.com/subscriptions) teenuse administraatori identimisteabega.

    ![Kuvatõmmis, Azure sisselogimine](./media/billing-add-office-365-tenant-to-azure-subscription/s313_azure-sign-in-service-admin.png)

    g. Peaksite nägema oma Office 365 rentniku armatuurlaual.

    ![Armatuurlaua kuvatõmmis](./media/billing-add-office-365-tenant-to-azure-subscription/s314_office-365-tenant-appear-in-azure.png)

4. Azure'i tellimusega seostatud kataloogi muuta.

    lisamine. Valige **sätted**.

    ![Kuvatõmmis, Azure'i klassikaline videoportaali sätted](./media/billing-add-office-365-tenant-to-azure-subscription/s315_azure-classic-portal-settings-icon.png)

    b. Valige oma Azure'i tellimus ja seejärel valige **Redigeeri DIRECTORY**.
    ![Kuvatõmmis, Azure tellimuse redigeerimine kataloog](./media/billing-add-office-365-tenant-to-azure-subscription/s316_azure-subscription-edit-directory.png)

    c. Valige **Järgmine** ![edasi-ikoon](./media/billing-add-office-365-tenant-to-azure-subscription/s317_next-icon.png).

    !["Muuda seotud kataloogi" kuvatõmmis](./media/billing-add-office-365-tenant-to-azure-subscription/s318_azure-change-associated-directory.png)

    > [AZURE.WARNING] Saate kõik kaasadministraatorite eemaldatakse hoiatus.

    ![Azure'i kinnitada-directory-kaardistamine](./media/billing-add-office-365-tenant-to-azure-subscription/s322_azure-confirm-directory-mapping.png)

    >[AZURE.WARNING] Lisaks ka eemaldatakse kõik [Rollipõhine juurdepääsu reguleerimine (RBAC)](./active-directory/role-based-access-control-configure.md) kasutajad määratud juurdepääsu ressursi rühmi. Hoiatus kuvatakse ainult mainimised kaasadministraatorite eemaldamine.

    ![määratud--eemaldatud-ressursside-rühmad](./media/billing-add-office-365-tenant-to-azure-subscription/s325_assigned-users-removed-resource-groups.png)

    d. Valige **valmis** ![lõpuleviimine ikooni](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

5. Nüüd saate lisada oma Office 365 ettevõtte kontod nimega kaasadministraatorite Azure Active Directory rentniku.

    lisamine. Valige vahekaart **Administraatorid** ja seejärel valige **Lisa**.

    ![Kuvatõmmis, Azure'i klassikaline videoportaali sätted administraatorid menüü](./media/billing-add-office-365-tenant-to-azure-subscription/s319_azure-classic-portal-settings-administrators.png)

    b. Sisestage oma Office 365 rentniku organisatsioonikonto, valige Azure'i tellimus ja seejärel valige **valmis** ![lõpuleviimine ikooni](./media/billing-add-office-365-tenant-to-azure-subscription/s38_complete-icon.png).

    ![Kuvatõmmis, Azure'i koostöö administraator dialoogiboks lisamine](./media/billing-add-office-365-tenant-to-azure-subscription/s320_azure-add-co-administrator.png)

    c. Minge tagasi vahekaardile **Administraatorid** . Peaksite nägema organisatsioonikonto, kuvatakse koostöö administraator.

    ![Administraatorid menüü kuvatõmmis](./media/billing-add-office-365-tenant-to-azure-subscription/s321_azure-co-administrator-added.png)

6. Järgmine saate testida Accessi kaasautorluse administraatori poole.

    lisamine. Konto haldusportaali väljalogimine.

    b. Avage [konto haldusportaal](https://account.windowsazure.com/subscriptions) või [Azure portaali](https://portal.azure.com/).

    c. Kui Azure'i lehel on link, **logige sisse oma ettevõtte konto**, valige link. Muul juhul selle sammu vahele jätta.

    ![Kuvatõmmis, Azure'i sisselogimisleht](./media/billing-add-office-365-tenant-to-azure-subscription/3-sign-in-to-azure.png)

    d. Sisestage mandaat koostöö administraator ja valige **sisselogimine**.

    ![Kuvatõmmis, Azure'i sisselogimisleht](./media/billing-add-office-365-tenant-to-azure-subscription/s324_azure-sign-in-with-co-admin.png)

## <a name="next-steps"></a>Järgmised sammud
Seotud stsenaariumid on järgmised.

- Teil juba on Office 365 tellimus ning olete valmis Azure tellimuse kasutajaks, kuid soovite kasutada olemasoleva Office 365 Kasutajakontod Azure tellimuse.
- On ka Azure abonendi ning soovite saada Office 365 tellimuse kasutajate teie olemasoleva Azure Active Directory eksemplar.

Nende toimingute tegemise kohta leiate teavet teemast [kasutada oma olemasolevat Office 365 konto Azure tellimuse või vastupidi](billing-use-existing-office-365-account-azure-subscription.md).
