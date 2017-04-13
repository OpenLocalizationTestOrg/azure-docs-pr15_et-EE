<properties
    pageTitle="Kuidas luua, hallata või Azure portaali konto salvestusruumi kustutada | Microsoft Azure'i"
    description="Mäluruumi uue konto loomine, haldamine oma konto kiirklahvide või Azure portaali konto salvestusruumi kustutada. Lisateavet standard ja premium salvestusruumi kontod."
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/26/2016"
    ms.author="robinsh"/>


# <a name="about-azure-storage-accounts"></a>Azure'i salvestusruumi kontode kohta

[AZURE.INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>Ülevaade

Azure'i salvestusruumi konto pakub kordumatut nimeruumi talletamine ja juurdepääs teie Azure Storage andmeobjektid. Kõigil objektidel salvestusruumi konto on arve koos rühmana. Vaikimisi on saadaval ainult teie konto omanik teie konto andmeid.

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

## <a name="storage-account-billing"></a>Salvestusruumi konto arveldamine

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

> [AZURE.NOTE] Azure'i virtuaalarvuti loomisel salvestusruumi konto luuakse teie jaoks automaatselt juurutamise asukoha kui te pole veel salvestusruumi konto soovitud kohta. Seega ei ole vaja luua salvestusruumi konto jaoks oma virtuaalse masina ketast alltoodud juhiseid järgida. Salvestusruumikonto nimi põhineb virtuaalse masina nimi. Leiate [Azure'i Virtuaalmasinates dokumentatsiooni kohta](https://azure.microsoft.com/documentation/services/virtual-machines/) lisateabe saamiseks.

## <a name="storage-account-endpoints"></a>Salvestusruumi konto lõpp-punktid

Iga objekti, mida talletatakse Azure Storage on kordumatu URL-i aadress. Salvestusruumikonto nimi vormid, milles käsitletakse alamdomeen. Alamdomeen ja domeeni nimi, mis on seotud iga teenuse, kombinatsiooni forms *lõpp-punkti* salvestusruumi konto jaoks.

Näiteks kui teie salvestusruumi konto nimi *mystorageaccount*, siis vaikimisi lõpp-punktid salvestusruumi konto jaoks on:

- Teenuse Bloobivahemälu: http://*mystorageaccount*. blob.core.windows.net

- Tabeli teenuse: http://*mystorageaccount*. table.core.windows.net

- Teenuse järjekorda: http://*mystorageaccount*. queue.core.windows.net

- Faili teenuse: http://*mystorageaccount*. file.core.windows.net

> [AZURE.NOTE] Ainult seab bloobimälu salvestusruumi konto bloobimälu teenuse lõpp-punkti.

URL-i juurdepääs objekti salvestusruumi konto on loodud objekti asukoha salvestusruumi konto lisamine lõpp-punkti. Näiteks bloobimälu aadress võib olla järgmises vormingus: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Samuti saate konfigureerida kohandatud domeeninime kasutada salvestusruumi kontoga. Klassikaline salvestusruumi kontod, vt üksikasjalikku [konfigureerimine kohandatud domeeni bloobimälu salvestusruumi lõpp-punkti nimi](storage-custom-domain-name.md) . Ressursihaldur salvestusruumi kontod, see funktsioon pole lisatud [Azure portaali](https://portal.azure.com) veel, kuid saate selle konfigureerida PowerShelliga. Lisateavet leiate teemast cmdlet [Set-AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607146.aspx) .  

## <a name="create-a-storage-account"></a>Salvestusruumi konto loomine

1. [Azure'i portaali](https://portal.azure.com)sisse logida.

2. Valige menüüs jaoturi **Uus** -> **andmete + talletusmaht** -> **salvestusruumi konto**.

3. Sisestage oma salvestusruumi konto nimi. Üksikasjalikku teavet kohta, kuidas kasutatakse salvestusruumikonto nimi oma objektid Azure Storage käsitlema teemast [salvestusruumi konto lõpp-punktid](#storage-account-endpoints) .

    > [AZURE.NOTE] Salvestusruumi kontonimed peab olema 3 ja 24 märki ja võib sisaldada arvude ja ainult väiketähti.
    >  
    > Oma salvestusruumikonto nimi peab olema kordumatu Azure'is. Kui valite selle salvestusruumikonto nimi on juba kasutusel näitab Azure portaali.

4. Määrake juurutamise mudeli kasutatava: **Ressursihaldur** või **klassikaline**. **Ressursihaldur** on soovitatav juurutamise mudel. Lisateabe saamiseks lugege teemat [mõistmine ressursihaldur juurutus- ja klassikaline juurutamise](../resource-manager-deployment-model.md).

    > [AZURE.NOTE] Bloobimälu salvestusruumi kontod saab luua ainult ressursihaldur juurutamise mudeli abil.

5. Valige salvestusruumi konto tüüp: **Üldine eesmärk** või **bloobimälu**. **Üldine eesmärk** on vaikimisi.

    Kui **Üldine eesmärk** on valitud, määrake jõudluse taseme: **Standard** või **Premium**. Vaikimisi on **Standardne**. Standard ja premium salvestusruumi kontode kohta leiate lisateavet teemast [Sissejuhatus Microsoft Azure'i Tabelimäluga](storage-introduction.md) ja [Premium mälu: suure jõudlusega salvestusruumi Azure virtuaalse masina töökoormus](storage-premium-storage.md).

    Kui **Bloobimälu** on valitud, määrake Accessi taseme: **kuum** või **lahedad**. Vaikimisi on **kuum**. Vt [Azure'i bloobimälu: lahedad ja kuum astme](storage-blob-storage-tiers.md) rohkem üksikasju.

6. Salvestusruumi konto suvand dispersioonanalüüs: **LRS**, **GRS**, **RA-GRS**või **ZRS**. Vaikimisi on **RA-GRS**. Azure'i talletussuvandite kopeerimise kohta leiate lisateavet teemast [Azure Storage kopeerimine](storage-redundancy.md).

7. Valige tellimus, kus soovite luua uue konto salvestusruumi.

8. Määrake uue ressursirühma või valige olemasoleva ressursi rühma. Ressursi rühmade kohta leiate lisateavet teemast [Azure ressursihaldur ülevaade](../azure-resource-manager/resource-group-overview.md).

9. Valige konto salvestusruumi geograafilise asukoha. Lisateavet selle kohta, milliseid teenuseid on regioonis saadaval leiate [Azure'i piirkondade](https://azure.microsoft.com/regions/#services) .

10. Klõpsake nuppu **Loo** konto salvestusruumi loomiseks.

## <a name="manage-your-storage-account"></a>Konto salvestusruumi haldamine

### <a name="change-your-account-configuration"></a>Muutke oma konto konfigureerimine

Pärast teie salvestusruumi konto loomist saate muuta oma konfiguratsioon, näiteks kasutatakse konto dispersioonanalüüs suvandi muutmine või juurdepääsu taseme bloobimälu salvestusruumi konto muutmine. [Azure portaali](https://portal.azure.com), liikuge salvestusruumi konto, valige **Kõik sätted** ja seejärel käsku **konfigureerimine** kuvamise ja/või muutke konto konfigureerimine.

> [AZURE.NOTE] Sõltuvalt jõudluse taseme salvestusruumi konto loomisel, ei pruugi mõned Tiražeerimissuvandid saadaval.

Dispersioonanalüüs suvandi muutmine muudab oma hinnad. Lisateavet leiate teemast [Azure salvestusruumi hinnad](https://azure.microsoft.com/pricing/details/storage/) lehe.

Bloobimälu salvestusruumi kontod, Accessi taseme muutmine tekkida kulude muudatuse Lisaks teie hinnad. [Bloobimälu salvestusruumi kontod – hinnad ja arveldamise kohta](storage-blob-storage-tiers.md#pricing-and-billing) lisateabe saamiseks lugege.

### <a name="manage-your-storage-access-keys"></a>Teie salvestusruumi kiirklahvide haldamine

Salvestusruumi konto loomisel Azure'i loob kaks 512-bitine salvestusruumi Accessi võtmed, mida kasutatakse autentimiseks konto salvestusruumi juurde. Kahe salvestusruumi kiirklahvide pakkudes Azure'i võimaldab teil taastada ilma katkestusteta salvestusruumi teenust või teenuse juurdepääsu võtmed.

> [AZURE.NOTE] Soovitame vältida kellelgi teisel teie salvestusruumi kiirklahvide ühiskasutusse. Salvestusruumi ressursid juurdepääsu lubada ilma annab välja oma kiirklahvide abil saate *ühiskasutusega juurdepääsu allkirja*. Ühiskasutusega juurdepääsu signatuuri pakub juurdepääsu ressursile teie konto jaoks intervall, mille määratlete ja õigustega, mille saate määrata. Lisateabe saamiseks vaadake [Kaudu ühiskasutusse antud juurdepääs allkirjade (SAS)](storage-dotnet-shared-access-signature-part-1.md) .

#### <a name="view-and-copy-storage-access-keys"></a>Vaatamine ja kopeerimine salvestusruumi kiirklahvide

[Azure portaali](https://portal.azure.com)liikuge salvestusruumi konto, valige **Kõik sätted** ja klõpsake **Pääsuklahvide** kuvamine, kopeerimine ja taastada oma konto kiirklahvide. **Kiirklahvide** tera ka eelnevalt konfigureeritud ühendusstringi oma kasutada oma rakendustes saate kopeerida esmaseid ja teiseseid klahvide abil.

#### <a name="regenerate-storage-access-keys"></a>Salvestusruumi kiirklahvide taastada

Soovitame, et muuta kiirklahvide perioodiliselt kaitsmiseks salvestusruumi ühenduste salvestusruumi kontole. Kahe kiirklahvide on määratud nii, et saate säilitada salvestusruumi konto ühenduse, kasutades ühte Accessi ajal saate taastada juurdepääsu võti.

> [AZURE.WARNING] Teie kiirklahvide regenereeruvate võib mõjutada teenuses Azure kui ka oma rakendustes, mis sõltuvad salvestusruumi konto. Kõik kliendid, mida kasutada kiirklahv salvestusruumi kontole juurdepääsemiseks tuleb värskendada kasutama uue tootenumbri.

**Media Servicesi** – kui teil on media Servicesi, mis sõltuvad teie salvestusruumi konto, peate uuesti sünkroonimise kiirklahvide teenusega meediumi pärast saate taastada võtmed.

**Rakenduste** - kui teil on veebirakenduste või salvestusruumi konto kasutavate pilveteenustega, lähevad ühenduste kui saate taastada klahvid, juhul, kui pöördute teie võtmed.

**Salvestusruumi maadeavastajad** – kui kasutate mis tahes [salvestamise Exploreri rakendusi](storage-explorers.md), saate tõenäoliselt tuleb värskendada salvestusruumi võti need rakendused.

Siin on pöörata teie salvestusruumi kiirklahvide protsessi:

1. Värskendage oma rakenduse koodi viitamiseks teisene kiirklahv salvestusruumi konto ühenduse stringid.

2. Esmane kiirklahv salvestusruumi konto taastada. **Kiirklahvide** enne, klõpsake **Võti1 taastada**, ja seejärel nuppu **Jah** kinnitada, et soovite luua uue tootenumbri.

3. Värskendage ühendusstringi viitamiseks uus esmane kiirklahv koodi.

4. Teisene kiirklahv taastada samal viisil.

## <a name="delete-a-storage-account"></a>Salvestusruumi konto kustutamine

Salvestusruumi konto, mida te enam ei kasuta eemaldamiseks liikuge salvestusruumi konto [Azure portaali](https://portal.azure.com)ja klõpsake nuppu **Kustuta**. Salvestusruumi konto kustutamine kustutab kogu konto, sh kõik andmed konto.

> [AZURE.WARNING] Ei saa taastada kustutatud salvestusruumi konto või mis tahes sisu, mis sisaldab see enne kustutamist tuua. Kindlasti midagi, mida soovite salvestada enne konto kustutamist varundada. See kehtib ressursse soovitud kontos – kui kustutate bloobimälu tabeli järjekorda või fail, kustutatakse jäädavalt.

Salvestusruumi konto, mis on seostatud mõne Azure virtuaalse masina kustutamiseks tuleb kõigepealt veenduda, et mis tahes virtuaalse masina ketast on kustutatud. Kui te esmalt kustutada oma virtuaalse masina ketast, siis konto salvestusruumi kustutamine katsel kuvatakse tõrketeade, mis sarnaneb:

    Failed to delete storage account <vm-storage-account-name>. Unable to delete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.

Kui salvestusruumi konto kasutab klassikaline juurutamise mudelit, saate kustutada virtuaalse masina ketta [Azure portaali](https://manage.windowsazure.com)järgmist:

1. Liikuge [klassikaline Azure portaali](https://manage.windowsazure.com).
2. Liikuge vahekaardile Virtuaalmasinates.
3. Klõpsake vahekaarti ketast.
4. Valige oma andmete ketas ja seejärel klõpsake nuppu Kustuta ketas.
5. Ketas piltide kustutamiseks liikuge menüüd pildid ja mis tahes pilte, mis on talletatud konto kustutada.

Lisateavet leiate [Azure'i Virtual seadme dokumentatsiooni](http://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i bloobimälu: Lahedad ja sooja astme.](storage-blob-storage-tiers.md)
- [Azure'i salvestusruumi dispersioonanalüüs](storage-redundancy.md)
- [Azure'i salvestusruumi ühendusstringi konfigureerimine](storage-configure-connection-string.md)
- [AzCopy käsurea kasuliku andmete edastamine](storage-use-azcopy.md)
- Leiate [Azure'i salvestusruumi meeskonna ajaveebi](http://blogs.msdn.com/b/windowsazurestorage/).
