<properties
    pageTitle="Looge konto Azure'i paketi | Microsoft Azure'i"
    description="Saate teada, kuidas käivitamiseks suuremahuliste paralleelselt töökoormus pilveteenuses Azure portaali konto Azure'i paketi loomine"
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
    ms.topic="get-started-article"
    ms.date="09/21/2016"
    ms.author="marsma"/>

# <a name="create-an-azure-batch-account-using-the-azure-portal"></a>Azure'i portaalis konto Azure'i paketi loomine

> [AZURE.SELECTOR]
- [Azure'i portaal](batch-account-create-portal.md)
- [Paketi halduse .net-i](batch-management-dotnet.md)

Saate teada, kuidas [Azure portaali]konto Azure'i paketi loomine[azure_portal], ja üles leida olulised kiirklahvide ja konto URL-id, näiteks konto atribuudid. Käsitleme ka paketi hinnad ja Azure Storage konto linkimine konto paketi nii, et saate kasutada [rakenduse paketid](batch-application-packages.md) ja [püsivad töö ja tööülesannete väljundi](batch-task-output.md).

## <a name="create-a-batch-account"></a>Paketi konto loomine

1. [Azure'i portaali]sisselogimine[azure_portal].

2. Klõpsake nuppu **Uus** > **arvutada** > **paketi teenus**.

    ![Paketi soovitud turuplatsil][marketplace_portal]

3. **Uue paketi konto** tera kuvatakse. Lugege teemat üksuste *lisamine* kuni *e* all iga element blade kirjeldused.

    ![Paketi konto loomine][account_portal]

    lisamine. **Konto nimi**: teie paketi konto jaoks kordumatu nimi. See nimi peab olema kordumatu Azure piirkonnas konto loomine (vt allpool *asukoht* ). See võib sisaldada ainult väiketähed, arve, ja peab olema 3-24 märki.

    b. **Tellimus**: tellimus, kus soovite luua paketi konto. Kui teil on ainult üks tellimus, on see vaikimisi valitud.

    c. **Ressursirühm**: mõne olemasoleva ressursi rühmitamine paketi konto või soovi korral looge uus.

    d. **Asukoht**: An Azure'i regiooni paketi konto loomiseks. Suvandid kuvatakse ainult need regioonid, mis ei toeta teie tellimuse ja ressursirühma.

    e. **Salvestusruumi konto** (valikuline): **Üldine otstarve** salvestusruumi konto seostate paketi kontosse (link). Üksikasjalikumat teavet teemast [lingitud Azure Storage konto](#linked-azure-storage-account) all.

4. Klõpsake nuppu **Loo** konto loomiseks.

  Portaali näitab, et see on **juurutamine** konto ja lõpetanud, kuvatakse teavitus **juurutuste õnnestus** *teatised*.

## <a name="view-batch-account-properties"></a>Paketi konto atribuutide vaatamine

Kui konto on loodud, saate avada **paketi konto blade** juurde pääseda oma sätted ja atribuudid. Vasakpoolsest menüüst paketi konto tera abil pääsete juurde kõik konto sätted ja atribuudid.

![Paketi konto blade Azure'i portaalis][account_blade]

* **Paketi konto URL**: rakenduste [paketi arengu API-de](batch-technical-overview.md#batch-development-apis) loomist peate konto URL, mille soovite hallata ressursid ja konto tööde käitamine. Paketi konto URL on järgmises vormingus:

    `https://<account_name>.<region>.batch.azure.com`

![Paketi konto URL-i portaalis][account_url]

* **Kiirklahvide**: rakenduste vaja võti ressursid teie paketi konto töötamisel. Saate vaadata või taastada paketi konto kiirklahvide, sisestage `keys` **otsinguväljale vasakpoolses menüüs paketi enne konto,** seejärel valige **võtmed**.

    ![Paketi konto klahvid Azure'i portaalis][account_keys]

## <a name="pricing"></a>Hinnad

Paketi kontod pakutakse ainult "Tasuta teise," mis tähendab, et te ei lisandu paketi konto ise. Teilt aluseks Azure Arvuta ressursse, mis kasutavad teie paketi lahenduste ja ressursside tarbitud oma töökoormus käitamisel muude teenustega. Näiteks ostmisega Arvuta sõlmed oma kaustadesse ja andmete jaoks pole hoida Azure Storage sisendi või väljundi tööülesannete jaoks. Samamoodi kui partii [rakenduse paketid](batch-application-packages.md) funktsiooni kasutamiseks on lisandu Azure Storage ressursside kasutatud oma rakenduse paketid. Vt [paketi hinnad] [ batch_pricing] lisateabe saamiseks.

## <a name="linked-azure-storage-account"></a>Lingitud Azure Storage konto

Nagu varem mainitud, saate (soovi korral) **Üldine otstarve** salvestusruumi konto paketi kontoga linkida. Paketi [rakenduse paketid](batch-application-packages.md) funktsioon kasutab bloobimälu lingitud üldine otstarve salvestusruumi konto, ei [Paketi faili põhimõtted .NET](batch-task-output.md) teek. Need valikulised funktsioonid aitavad juurutamine tööülesannete paketi käivitamine rakendusi ja püsib toodavad andmed.

Partii praegu toetab *ainult* **Üldine otstarve** salvestusruumi konto tüüp, nagu on kirjeldatud juhises 5, [Loo konto salvestusruumi](../storage/storage-create-storage-account.md#create-a-storage-account) [kohta Azure salvestusruumi kontod](../storage/storage-create-storage-account.md). Kui Azure Storage konto linkimine konto paketi olla kindel lingi *ainult* **Üldine otstarve** salvestusruumi konto.

!["Üldine eesmärk" salvestusruumi konto loomine][storage_account]

Me soovitame teil luua salvestusruumi konto kasutab, saate oma paketi kontole.

>[AZURE.WARNING] Olge ettevaatlik regenereeruvate kiirklahvide lingitud salvestusruumi konto. Taastada ainult üks salvestusruumi konto võti ja klõpsake nuppu **Sünkrooni klahvid** lingitud salvestusruumi konto enne. Oodake viis minutit, mis võimaldab kajastuma Arvuta sõlmed oma kaustadesse, siis taastada ja sünkroonimine muu sisestage vajadusel võtmed. Kui te taastada korraga nii klahvid, oma Arvuta sõlmed ei saa sünkroonida seda all ja need kaotavad väliskasutajad juurdepääsu salvestusruumi kontole.

  ![Teenused lisatasu salvestusruumi konto võtmed][4]

## <a name="batch-service-quotas-and-limits"></a>Paketi teenuse kvootide ja piirangud

Pidage meeles, et koos Azure tellimuse ja muud Azure teenused, teatud [kvootide ja piirangud](batch-quota-limit.md) rakendada paketi kontod. Praeguse kvootide paketi konto kuvatakse portaali konto **Atribuudid**.

![Paketi konto kvootide Azure'i portaalis][quotas]

Kui olete kujundamise ja teie paketi töökoormus ülespoole võtke arvesse nende kvootide. Näiteks kui teie pool pole jõudmist Arvuta sõlmed määratud sihtrühma arv, olete võib jõudnud core kvoodilimiit paketi konto jaoks.

Samuti võtke arvesse, et teil on piiratud ühe paketi konto Azure tellimuse. Saate käivitada mitme paketi töökoormus paketi ühe konto või levitamine oma töökoormus paketi kontode sama tellimust, kuid Azure erinevate piirkondade vahel.

Paljude kvootide saate suurendada lihtsalt tasuta toote tugiteenuse taotluse Azure'i portaalis esitatud. Vt täpsemat paluda kvoodi suureneb [kvootide ja Azure'i paketi teenuste piirangud](batch-quota-limit.md) .

## <a name="other-batch-account-management-options"></a>Muude paketi Kontosuvandid haldus

Lisaks Azure portaali kasutamisel saate ka luua ja hallata paketi kontodel on järgmised:

* [Paketi PowerShelli cmdlet-käsud](batch-powershell-cmdlets-get-started.md)
* [Azure'i CLI](../xplat-cli-install.md)
* [Paketi halduse .net-i](batch-management-dotnet.md)

## <a name="next-steps"></a>Järgmised sammud

* Teemast [Azure paketi funktsioon ülevaade](batch-api-basics.md) Lisateavet paketi teenuse mõisted ja funktsioonid. Artikkel käsitletakse esmane paketi ressursse, nt kaustu, Arvuta sõlmed, töö ja tööülesanded ja antakse ülevaade sellest, teenuse funktsioone, mis võimaldavad suuremahuliste Arvuta töökoormus täitmise.

* Põhialused arendada paketi lubatud rakendus, kasutades [paketi .net-i kliendi teek](batch-dotnet-get-started.md). [Sissejuhatav artikkel](batch-dotnet-get-started.md) juhendab teid töötamise rakendus, mis kasutab teenust paketi käivitada on töökoormus mitme Arvuta sõlmed, ja sisaldab Azure Storage kasutamise töökoormus faili lavastus ja otsing.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Teenused lisatasu salvestusruumi konto võtmed"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
