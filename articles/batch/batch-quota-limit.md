<properties
    pageTitle="Partii teenuse kvootide ja piirangud | Microsoft Azure'i"
    description="Lisateavet vaikimisi Azure'i paketi piirmäära, piirangud ja piiranguid ja kuidas taotleda kvoodi suureneb"
    services="batch"
    documentationCenter=""
    authors="mmacy"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.workload="big-compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/10/2016"
    ms.author="marsma"/>

# <a name="quotas-and-limits-for-the-azure-batch-service"></a>Kui ka Azure'i paketi teenuse piirangud

Azure'i muude teenustega on teatud ressursid paketi teenusega seotud piirangud. Need piirangud on vaikimisi kvootide rakendada, kui Azure tellimuse või konto tasemel. Selles artiklis käsitletakse nende vaikesätted ja kuidas saate taotleda kvoodi suurendab.

Kui plaanite tootmise töökoormus paketi käivitamiseks, peate suurendamine ühe või mitme kvootide vaikimisi kohal. Kui soovite kvoodi tõsta, saate avada ka online [klienditugi taotluse](#increase-a-quota) tasuta.

>[AZURE.NOTE] Kvoot on krediitkaardiga mahupiirangut, mitte võimsus garantii. Kui teil on suuremahuliste võimsus vajadustele, võtke ühendust Azure toega.

## <a name="subscription-quotas"></a>Tellimuse kvoote
**Ressurss**|**Vaikimisi limiit**|**Lubatud**
---|---|---
Paketi kontod piirkonna tellimuse kohta | 1 | 50

## <a name="batch-account-quotas"></a>Paketi konto kvoote
[AZURE.INCLUDE [azure-batch-limits](../../includes/azure-batch-limits.md)]

## <a name="other-limits"></a>Muud piirangud
**Ressurss**|**Lubatud**
---|---
[Samaaegseid tööülesannete](batch-parallel-node-tasks.md) MSDN kohta. | 4 x arv sõlm valdkond
[Rakenduste](batch-application-packages.md) paketi konto kohta        | 20
Rakenduse paketid taotluse kohta  | 40
Rakenduse paketi suurus (iga)       | Umbes<sup>1</sup> 195GB

<sup>1</sup> Azure'i salvestuslimiit mahu ülempiir Blokeeri bloobimälu

## <a name="view-batch-quotas"></a>Paketi kvoodi vaatamine

Saate vaadata oma paketi konto kvootide [Azure portaali][portal].

1. **Paketi kontod** portaalis valige paketi konto, mis teile huvi.

2. Valige **Atribuudid** paketi konto menüü tera

3. Tera atribuudid kuvatakse **kvootide** praegu rakendatud paketi konto

    ![Paketi konto kvoote][account_quotas]

## <a name="increase-a-quota"></a>Kvoodi suurendamine

Järgige juhiseid, saate taotleda kvoodi suurendamiseks [Azure portaali][portal].

1. Valige paremas ülanurgas portaali **abi + tugi** paan portaali armatuurlauale või küsimärki (****?).

2. Valige **Uus tugiteenuste taotlus** > **põhitõed**.

3. Enne **põhialused** :

    lisamine. **Probleemi tüüp** > **piirmäär**

    b. Valige oma tellimus.

    c. **Kvoodi tüüp** > **paketi**

    d. **Tugiteenuste lepingu** > **kvoodi tugi - sisalduv**

    Klõpsake nuppu **edasi**.

4. Enne **probleemi** :

    lisamine. Valige **raskusaste** vastavalt oma [ettevõtte mõju][support_sev].

    b. **Üksikasjad**, määrata iga kvoodi, mida soovite muuta, konto nimi ja uus limiit.

    Klõpsake nuppu **edasi**.

5. Enne **Kontaktteave** :

    lisamine. Valige **eelistatud kontaktmeetod**.

    b. Kontrollige ja sisestage nõutav kontaktandmed.

    Klõpsake nuppu **Loo** esitada tugiteenuse taotluse.

Kui olete oma tugiteenuse taotluse esitanud, Azure tugiteenuste töötaja võtab teiega. Pange tähele, et täitmise taotluse võib kuluda kuni 2 tööpäeva.

## <a name="related-topics"></a>Seotud teemad

* [Azure'i portaalis konto Azure'i paketi loomine](batch-account-create-portal.md)

* [Azure'i paketi funktsioonide ülevaade](batch-api-basics.md)

* [Azure'i tellimus ja teenuste piirangud, kvoote ja piiranguid](../azure-subscription-service-limits.md)

[portal]: https://portal.azure.com
[portal_classic_increase]: https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/
[support_sev]: http://aka.ms/supportseverity

[account_quotas]: ./media/batch-quota-limit/accountquota_portal.PNG
