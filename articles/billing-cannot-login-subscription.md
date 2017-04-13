<properties
    pageTitle="Ei saa sisse logida Azure tellimuse | Microsoft Azure'i"
    description="Selles artiklis kirjeldatakse Azure tellimuse login levinumate probleemide tõrkeotsing."
    services=""
    documentationCenter=""
    authors="genlin"
    manager="mbaldwin"
    editor=""
    tags="billing"
    />

<tags
    ms.service="billing"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/25/2016"
    ms.author="genli"/>

# <a name="i-cant-sign-in-to-manage-my-azure-subscription"></a>Ma ei saa sisse logida Azure tellimuse haldamiseks

Selles artiklis juhatab teid läbi mõne enamlevinud meetodi login probleemide lahendamiseks.

## <a name="page-hangs-in-the-loading-status"></a>Lehe hangub laadimise olek

Kui teie Interneti-brauseri lehe hangub, proovige järgmist iga seni, kuni kuvatakse [Azure portaali](https://portal.azure.com).

-   Värskendage lehte.
-   Kasutage mõnda muud brauserit Internet.
-   Kui kasutate Microsoft Internet Explorer, liikuge sirvides Azure portaali InPrivate-sirvimise režiimis. 

    V-SSE.  Klõpsake **menüü Tööriistad** ![nupu](./media/billing-cannot-login-subscription/Toolsbutton.png) > **ohutus** > **InPrivate-sirvimise**.

    B.  Liikuge sirvides [Azure portaali](https://portal.azure.com)ja seejärel logige sisse portaali.

## <a name="error-message-no-subscriptions-found"></a>Tõrketeade "Tellimust ei leitud"

Kui teie konto pole piisavaid õigusi, kuvatakse tõrge sõnumi **tellimust ei leitud** . Ainult kontohaldur pääsevad [Konto keskele](https://account.windowsazure.com/), mitte teenuseadministraatorid (SA) või kaasadministraatorite (CA).

**Stsenaarium 1: Tõrke sõnum [Azure'i portaal](https://portal.azure.com)**

Kui soovite konto [lisamine kaasautorluse administraatori või omaniku roll](billing-add-change-azure-subscription-administrator.md) probleemi lahendamiseks.

**Stsenaarium 2: Tõrke sõnum [Azure'i konto Center](https://account.windowsazure.com/Subscriptions)**

Kontrollige, kas konto, mida kasutasite on administraatori konto. Kui soovite kontrollida, kes on kontohaldur, tehke järgmist.

1.  [Azure'i portaali](https://portal.azure.com)sisse logida.
2.  Valige menüü jaoturi **tellimus**.
3.  Valige tellimus, mida soovite kontrollida, ja seejärel valige **sätted**.
4.  Valige **Atribuudid**. Tellimuse kontohaldur kuvatakse väljal **Konto administraator** .

## <a name="you-are-automatically-signed-in-as-a-different-user"></a>Teise kasutajana on automaatselt sisse logitud

See probleem võib ilmneda, kui kasutate Interneti-brauseri rohkem kui ühe kasutajakonto.

Probleemi lahendamiseks proovige ühte järgmistest meetoditest.

-   Vahemälu ja kustutage Interneti-küpsised. Valige Internet Exploreris **Tööriistad** ![nupu](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Interneti-suvandid** > **kustutada**. Veenduge, et ajutised failid, küpsised, parool ja sirvimisajalugu ruudud on valitud ja seejärel klõpsake nuppu Kustuta.

-   Lähtestage Internet Exploreri sätted taastada isiklikke sätteid, mille olete teinud. Klõpsake **menüü Tööriistad** ![nupu](./media/billing-cannot-login-subscription/Toolsbutton.png)> **Interneti-suvandid** > **Täpsemalt** > märkige ruut **Kustuta isiklikud sätted** > **Lähtesta**.

-   Liikuge sirvides Azure portaali InPrivate-sirvimise režiimis. Klõpsake **menüü Tööriistad** ![nupu](./media/billing-cannot-login-subscription/Toolsbutton.png) > **ohutus** > **InPrivate-sirvimise**.

## <a name="need-help-contact-support"></a>Kas vajate abi? Pöörduge klienditoe poole. 

Kui vajate veel abi saamiseks [tugiteenuste](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) probleemi lahendada kiiresti liikuda. 