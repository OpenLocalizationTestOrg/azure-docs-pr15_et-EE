<properties
    pageTitle="Tõrkeotsing, kui kustutate Azure salvestusruumi kontod, ümbriste või VHDs ressursihaldur juurutamine | Microsoft Azure'i"
    description="Kui kustutate Azure salvestusruumi kontod, ümbriste või VHDs ressursihaldur juurutamine tõrgete tõrkeotsing"
    services="storage"
    documentationCenter=""
    authors="genlin"
    manager="felixwu"
    editor="na"
    tags="storage"/>

<tags
    ms.service="storage"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="genli"/>

# <a name="troubleshoot-errors-when-you-delete-azure-storage-accounts-containers-or-vhds-in-a-resource-manager-deployment"></a>Kui kustutate Azure salvestusruumi kontod, ümbriste või VHDs ressursihaldur juurutamine tõrgete tõrkeotsing

[AZURE.INCLUDE [storage-selector-cannot-delete-storage-account-container-vhd](../../includes/storage-selector-cannot-delete-storage-account-container-vhd.md)]

Tõrgete võidakse kuvada, kui proovite kustutada Azure storage konto, container või virtuaalse kõvaketta (VHD) [Azure portaali](https://portal.azure.com). Sellest artiklist leiate juhised tõrkeotsing on Azure ressursihaldur juurutuse probleemi lahendamiseks.

Kui see artikkel ei oma Azure probleemile, külastage [MSDN-i ja virnas ületäitumise](https://azure.microsoft.com/support/forums/)Azure foorumeid. Saate postitada oma probleemi foorumites või juurde @AzureSupport Twitter. Samuti saate faili Azure'i tugiteenuse taotluse, valides **tootetugi** [Azure tugiteenuste](https://azure.microsoft.com/support/options/) sait.

## <a name="symptoms"></a>Sümptomid

### <a name="scenario-1"></a>Stsenaarium 1

Kui proovite kustutada on VHD salvestusruumi konto ressursihaldur juurutamine, kuvatakse järgmine tõrketeade:

**Bloobimälu 'vhds/BlobName.vhd' kustutamine nurjus. Tõrge: On praegu rendilepingu soovitud bloobimälu ja üürilepingu ID on määratud taotluse.**

See probleem võib ilmneda, sest virtuaalse masina (VM) on rendilepingu VHD, mille soovite kustutada.

### <a name="scenario-2"></a>Stsenaarium 2

Saate salvestusruumi konto ressursihaldur juurutuse ümbrise kustutada katsel kuvatakse järgmine tõrketeade:

**Salvestusruumi container 'vhds' kustutamine nurjus. Tõrge: On praegu rendilepingu ümbris ja üürilepingu ID on määratud taotluse.**

See probleem võib ilmneda, kuna ümbris on VHD, mis on lukus üürilepingu olekus.

### <a name="scenario-3"></a>Stsenaarium 3

Kui proovite kustutada salvestusruumi konto ressursihaldur juurutamine, kuvatakse järgmine tõrketeade:

**Salvestusruumi konto 'StorageAccountName' kustutamine nurjus. Tõrge: Salvestusruumi konto ei saa kustutada oma esemeid kasutusel tõttu.**

See probleem võib ilmneda, kuna salvestusruumi konto sisaldab VHD, mis on üürilepingu olekus.

## <a name="solution"></a>Lahendus

Nende probleemide lahendamiseks saate tuvastada, VHD, mis võib põhjustada tõrke ja seotud VM. Seejärel VHD VM (jaoks andmete kettalt) eemaldada või kustutada VM, mis kasutab funktsiooni VHD (OS ketast). See eemaldab üürileping on VHD ja lubab seda, et kustutada.

### <a name="step-1-identify-the-problem-vhd-and-the-associated-vm"></a>Samm 1: Probleemi tuvastada VHD ja seotud VM


1. [Azure'i portaali](https://portal.azure.com)sisse logida.
2. Valige menüüs **jaoturi** **kõik ressursid**. Avage salvestusruumi konto, mille soovite kustutada, ja seejärel valige **plekid** > **vhds**.

    ![Portaali konto salvestusruumi ja esile tõstetud "vhds" ümbris kuvatõmmis](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/opencontainer.png)

3. Kontrollige iga VHD ümbrises atribuute. Otsige üles VHD, mis on **renditud** olekus. Seejärel määrata, milline VM on kasutusel on VHD. Tavaliselt, saate määrata hoidev VM on VHD, märkides ruudu nimi on VHD:

    - OS ketast üldiselt järgige selles nimetamistava: VMNameYYYYMMDDHHMMSS.vhd
    - Andmete ketast üldiselt järgige selles nimetamistava: VMName-AAAAKKPP-HHMMSS.vhd

    ![Container teave portaalis kuvatõmmis, kus nime VM, üürilepingu oleku "Lukus" ja "Renditud" esile tõstetud üürilepingu olek](./media/storage-resource-manager-cannot-delete-storage-account-container-vhd/locatevm.png)

### <a name="step-2-remove-the-lease-from-the-vhd"></a>Samm 2: Selle VHD rendilepingu eemaldamine

VM, mis kasutab funktsiooni VHD (OS ketast) kustutamiseks tehke järgmist.

1.  [Azure'i portaali](https://portal.azure.com)sisse logida.
2.  Valige menüüs **jaoturi** **Virtuaalmasinates**.
3.  Valige malliga rendilepingu on VHD VM.
4.  Veenduge, et midagi aktiivselt kasutab virtuaalse masina, ja et te enam ei vaja virtuaalse masina.
5.  Tera **VM üksikasjad** ülaosas valige **Kustuta**ja seejärel klõpsake nuppu **Jah** kinnitage.
6.  VM tuleks kustutada, kuid selle VHD tuleks säilitada. Siiski ei tohiks olla selle VHD rendilepingu seda. Võib kuluda paar minutit anda vabastatakse. Veenduge, et üürileping on välja, minge **kõik ressursid** > **Salvestusruumikonto nimi** > **plekid** > **vhds**. Paanil **bloobimälu atribuudid** **Üürilepingu oleku** väärtus peab olema **lukustamata**.

Et eemaldada VHD VM, mis kasutab (jaoks andmete kettalt):

1.  [Azure'i portaali](https://portal.azure.com)sisse logida.
2.  Valige menüüs **jaoturi** **Virtuaalmasinates**.
3.  Valige malliga rendilepingu on VHD VM.
4.  Valige **ketast** **VM üksikasju** enne.
5.  Valige malliga rendilepingu on VHD andmete ketas. Saate määrata, mis on lisatud VHD ketas, märkides ruudu on VHD URL-i.
6.  Määratlege kindlalt, et midagi aktiivselt kasutab andmed kettale.
7.  Klõpsake käsku **Eemalda** **ketta üksikasju** enne.
8.  Ketas peaks nüüd VM, eemaldada ja selle VHD enam peaks olema rendilepingu seda. Võib kuluda paar minutit anda vabastatakse. Veenduge, et üürileping väljaandmisest, minge **kõik ressursid** > **Salvestusruumikonto nimi** > **plekid** > **vhds**. Paanil **bloobimälu atribuudid** **Üürilepingu olek** väärtus peab olema **lukustamata**.

## <a name="what-is-a-lease"></a>Mis on rendilepingu?

Rendilepingu on lukus, mida saab kasutada mõne bloobimälu (nt VHD) juurdepääsu piiramiseks. Kui soovitud bloobimälu renditakse, ainult omanikud on asjaolude pääsete juurde selle bloobimälu. Rendilepingu on olulised järgmistel põhjustel.

-   See takistab andmeid kui mitu omanikud proovige kirjutada selle bloobimälu sama osale korraga.
-   See takistab selle bloobimälu kustutamise kui midagi aktiivselt kasutab seda (nt VM).
-   See takistab salvestusruumi konto on kustutatud, kui midagi aktiivselt kasutab seda (nt VM).



## <a name="next-steps"></a>Järgmised sammud

- [Salvestusruumi konto kustutamine](storage-create-storage-account.md#delete-a-storage-account)
- [Kuidas murda lukustatud rentida, bloobimälu Microsoft Azure (PowerShelli)](https://gallery.technet.microsoft.com/scriptcenter/How-to-break-the-locked-c2cd6492)
