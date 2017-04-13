<properties
    pageTitle="Kuidas luua, hallata või Azure klassikaline portaali konto salvestusruumi kustutada | Microsoft Azure'i"
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

Azure'i salvestusruumi konto kaudu pääsete juurde Azure Storage Azure'i bloobimälu, järjekorda, tabeli või faili teenuseid. Konto salvestusruumi pakub üheste nimeruumi oma Azure Storage andmeobjektid. Vaikimisi on saadaval ainult teie konto omanik teie konto andmeid.

On kahte tüüpi salvestusruumi kontod.

- Kindlad konto sisaldab bloobimälu, tabelist, kuhjuda ja faili salvestusruumi.
- Salvestusruumi premium konto toetab praegu ainult Azure virtuaalse masina ketast. Vt [Premium mälu: suure jõudlusega salvestusruumi Azure virtuaalse masina töökoormus](storage-premium-storage.md) Premium salvestusruumi põhjaliku ülevaate.

## <a name="storage-account-billing"></a>Salvestusruumi konto arveldamine

Azure'i salvestusruumi kasutuse vastavalt teie konto salvestusruumi on arve. Salvestusruumi kulud põhinevad neli tegurit: mälumaht, dispersioonanalüüs värviskeemi, salvestusruumi tehingute ja sealt andmeid.

- Mälumaht viitab palju teie salvestusruumi konto üleandmise olete abil andmete talletamiseks. Lihtsalt andmete talletamise maksumus määratakse, kui palju andmeid on talletamine ja kuidas on kopeeritud.
- Dispersioonanalüüs määrab, mitu koopiat teie andmeid hoitakse korraga ja millised asukohtades.
- Tehingud viidata kõik lugeda ja kirjutada Azure Storage.
- Andmete sealt viitab üle läbi Azure piirkond. Kui konto salvestusruumi andmed on rakendus, mis ei tööta sama piirkonna juurde, kas see rakendus on pilveteenus või mõnda muud tüüpi rakendus, siis on teie eest andmete sealt. (Azure'i teenuste, saate teha toiminguid selleks rühmitamiseks teie andmeid ja teenuste sama andmekeskuste vähendamiseks või kõrvaldamiseks sealt andmesidetasud.)  

[Azure'i salvestusruumi hinnad](https://azure.microsoft.com/pricing/details/storage) lehelt leiate üksikasjaliku hinnateavet mälumaht, kopeerimise ja tehingud. [Andmete edastamine hinnad üksikasjade](https://azure.microsoft.com/pricing/details/data-transfers/) lehe pakub andmete sealt hinnakirjad üksikasjalikku teavet.

Salvestusruumi konto ja tulemuslikkuse sihtkohtade kohta leiate üksikasjalikumat teavet teemast [Azure salvestusruumi skaleeritavus ja jõudluse sihtkohad](storage-scalability-targets.md).

> [AZURE.NOTE] Azure'i virtuaalarvuti loomisel salvestusruumi konto luuakse teie jaoks automaatselt juurutamise asukoha kui te pole veel salvestusruumi konto soovitud kohta. Seega ei ole vaja luua salvestusruumi konto jaoks oma virtuaalse masina ketast alltoodud juhiseid järgida. Salvestusruumikonto nimi põhineb virtuaalse masina nimi. Leiate [Azure'i Virtuaalmasinates dokumentatsiooni kohta](https://azure.microsoft.com/documentation/services/virtual-machines/) lisateabe saamiseks.

## <a name="create-a-storage-account"></a>Salvestusruumi konto loomine

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com)sisse logima.

2. Klõpsake lehe allosas tegumiribal nuppu **Uus** . Valige **Data Services** | **salvestusruumi**ja klõpsake seejärel nuppu **Kiiresti luua**.

    ![NewStorageAccount](./media/storage-create-storage-account-classic-portal/storage_NewStorageAccount.png)

3. **URL-i**, sisestage oma salvestusruumi konto nimi.

    > [AZURE.NOTE] Salvestusruumi kontonimed peab olema 3 ja 24 märki ja võib sisaldada arvude ja ainult väiketähti.
    >  
    > Oma salvestusruumikonto nimi peab olema kordumatu Azure'is. Azure'i klassikaline portaal näitab, kui valite selle salvestusruumikonto nimi on juba kasutusel.

    Vt allpool [salvestusruumi konto lõpp-punktid](#storage-account-endpoints) kohta, kuidas teie objektid Azure Storage käsitlema kasutatakse salvestusruumikonto nimi.

4. Valige **Asukoht/osaleja rühma**salvestusruumi konto, mis on teie lähedal või oma klientidele asukoht. Kui teie salvestusruumi andmeid ei pääse teise Azure teenuse, nt Azure virtuaalse masina või pilveteenuses, võite osaleja rühma valimine loendist, et rühmitada sama andmekeskuse muude Azure teenustega, mida kasutate parandada jõudlust ja madalamad konto salvestusruumi.

    Pange tähele, et peate valima mõne osaleja rühma salvestusruumi konto loomisel. Ei saa teisaldada olemasolev konto osaleja rühma. Osaleja rühmade kohta leiate lisateavet teemast [osaleja rühmaga koostöö asukoht](#service-co-location-with-an-affinity-group) .

    >[AZURE.IMPORTANT] Määrata, millised asukohad on saadaval tellimuse, saate helistada [loendi kõigi ressursside teenusepakkujate](https://msdn.microsoft.com/library/azure/dn790524.aspx) toiming. Loendi pakkujate PowerShelli, helistada [Get-AzureLocation](https://msdn.microsoft.com/library/azure/dn757693.aspx). Kaudu .net-i, kasutage [loendi](https://msdn.microsoft.com/library/azure/microsoft.azure.management.resources.provideroperationsextensions.list.aspx) meetodit ProviderOperationsExtensions klassi.
    >
    >Lisaks leiate [Azure'i piirkondade](https://azure.microsoft.com/regions/#services) Lisateavet selle kohta, milliseid teenuseid on regioonis saadaval.


5. Kui teil on rohkem kui üks Azure'i tellimus, kuvatakse väljal **tellimuse** . Sisestage **tellimuse**Azure'i tellimus, mida soovite kasutada konto salvestusruumi.

6. Valige **Dispersioonanalüüs**, dispersioonanalüüs salvestusruumi konto jaoks soovitud taset. Geograafilise liigne dispersioonanalüüs, mis pakub maksimaalne kestvus teie andmete jaoks on soovitatav dispersioonanalüüs suvand. Azure'i talletussuvandite kopeerimise kohta leiate lisateavet teemast [Azure Storage kopeerimine](storage-redundancy.md).

6. Klõpsake nuppu **Loo konto salvestusruumi**.

    Võib kuluda mõni minut salvestusruumi konto loomisel. Oleku, saate jälgida teatised Azure klassikaline portaali allosas. Pärast salvestusruumi konto loomist salvestusruumi kontosse on **Online** olek ja on kasutamiseks valmis.

![StoragePage](./media/storage-create-storage-account-classic-portal/Storage_StoragePage.png)


### <a name="storage-account-endpoints"></a>Salvestusruumi konto lõpp-punktid

Iga objekti, mida talletatakse Azure Storage on kordumatu URL-i aadress. Salvestusruumikonto nimi vormid, milles käsitletakse alamdomeen. Alamdomeen ja domeeni nimi, mis on seotud iga teenuse, kombinatsiooni forms *lõpp-punkti* salvestusruumi konto jaoks.

Näiteks kui teie salvestusruumi konto nimi *mystorageaccount*, siis vaikimisi lõpp-punktid salvestusruumi konto jaoks on:

- Teenuse Bloobivahemälu: http://*mystorageaccount*. blob.core.windows.net

- Tabeli teenuse: http://*mystorageaccount*. table.core.windows.net

- Teenuse järjekorda: http://*mystorageaccount*. queue.core.windows.net

- Faili teenuse: http://*mystorageaccount*. file.core.windows.net

Kui konto on loodud saate vaadata salvestusruumi armatuurlaual [Azure klassikaline portaali](https://manage.windowsazure.com) konto salvestusruumi lõpp-punktid.

URL-i juurdepääs objekti salvestusruumi konto on loodud objekti asukoha salvestusruumi konto lisamine lõpp-punkti. Näiteks bloobimälu aadress võib olla järgmises vormingus: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Samuti saate konfigureerida kohandatud domeeninime kasutada salvestusruumi kontoga. Vaadake lisateavet [konfigureerimine bloobimälu salvestusruumi lõpp-punkti jaoks kohandatud domeeninime](storage-custom-domain-name.md) .

### <a name="service-co-location-with-an-affinity-group"></a>Osaleja rühmaga koostöö asukoht

Mõne *osaleja rühma* on geograafilised rühmitamise oma Azure'i teenuste ja VMs Azure'i salvestusruumi kontoga. Mõne osaleja rühma saate teenuse jõudluse parandamiseks asukoha sama andmekeskuse või lähedal sihtrühma kasutaja arvuti töökoormus. Samuti pole arve kulude tekivad sealt salvestusruumi konto andmed on pääseb muu teenus, mis on osa samade osaleja rühma.

> [AZURE.NOTE]  Osaleja rühma loomine avamine [Azure klassikaline portaali](https://manage.windowsazure.com)ala <b>sätted</b> , klõpsake <b>osaleja rühmad</b>ja seejärel klõpsake käsku <b>Lisa ka osaleja rühma</b> või nuppu <b>Lisa</b> . Saate luua ja Azure teenuse juhtimise API abil osaleja rühmade haldamine. Lisateabe saamiseks vaadake <a href="http://msdn.microsoft.com/library/azure/ee460798.aspx">osaleja rühmade toiminguid</a> .

## <a name="view-copy-and-regenerate-storage-access-keys"></a>Vaadata, kopeerimine ja taastada salvestusruumi kiirklahvide

Salvestusruumi konto loomisel Azure'i loob kaks 512-bitine salvestusruumi Accessi võtmed, mida kasutatakse autentimiseks konto salvestusruumi juurde. Kahe salvestusruumi kiirklahvide pakkudes Azure'i võimaldab teil taastada ilma katkestusteta salvestusruumi teenust või teenuse juurdepääsu võtmed.

> [AZURE.NOTE] Soovitame vältida kellelgi teisel teie salvestusruumi kiirklahvide ühiskasutusse. Salvestusruumi ressursid juurdepääsu lubada ilma annab välja oma kiirklahvide abil saate *ühiskasutusega juurdepääsu allkirja*. Ühiskasutusega juurdepääsu signatuuri pakub juurdepääsu ressursile teie konto jaoks saate määratleda intervalli ja õigustega, mille saate määrata. Lisateabe saamiseks vaadake [Kaudu ühiskasutusse antud juurdepääs allkirjade (SAS)](storage-dotnet-shared-access-signature-part-1.md) .

[Azure'i klassikaline portaali](https://manage.windowsazure.com)klahvidega **Haldamine** armatuurlaual või **salvestusruumi** lehe kuvamiseks, kopeerimine ja taastada bloobimälu, tabeli ja järjekorda teenustele juurdepääsuks kasutatavate kiirklahvide salvestusruumi.

### <a name="copy-a-storage-access-key"></a>Kopeerige salvestusruumi juurdepääsu võti  

Saate **Hallata klahvid** salvestusruumi kiirklahv kasutamine ühendusstringi kopeerimiseks. Ühendusstringi jaoks on vaja salvestusruumikonto nimi ja klahvi kasutamine autentimist. Ühendusstringi Azure storage teenustele juurdepääsuks konfigureerimise kohta leiate teemast [Konfigureerimine Azure'i salvestusruumi ühendusstringi](storage-configure-connection-string.md).

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com)nuppe **salvestusruum**, ja klõpsake armatuurlaual avamiseks salvestusruumi konto nimi.

2. Klõpsake nuppu **Halda võtmed**.

    Avaneb **Kiirklahvide haldamine** .

    ![Managekeys](./media/storage-create-storage-account-classic-portal/Storage_ManageKeys.png)


3. Salvestusruumi juurdepääsu võti kopeerimiseks valige klahv tekst. Seejärel paremklõpsake seda ja klõpsake käsku **Kopeeri**.

### <a name="regenerate-storage-access-keys"></a>Salvestusruumi kiirklahvide taastada
Soovitame, et muuta kiirklahvide perioodiliselt kaitsmiseks salvestusruumi ühenduste salvestusruumi kontole. Kahe kiirklahvide on määratud nii, et saate säilitada salvestusruumi konto ühenduse, kasutades ühte Accessi ajal saate taastada juurdepääsu võti.

> [AZURE.WARNING] Teie kiirklahvide regenereeruvate võib mõjutada teenuses Azure kui ka oma rakendustes, mis sõltuvad salvestusruumi konto. Kõik kliendid, mida kasutada kiirklahv salvestusruumi kontole juurdepääsemiseks tuleb värskendada kasutama uue tootenumbri.

**Media Servicesi** – kui teil on media Servicesi, mis sõltuvad teie salvestusruumi konto, peate uuesti sünkroonimise kiirklahvide teenusega meediumi pärast saate taastada võtmed.

**Rakenduste** - kui teil on veebirakenduste või salvestusruumi konto kasutavate pilveteenustega, lähevad ühenduste kui saate taastada klahvid, juhul, kui pöördute oma võtmed. 

**Salvestusruumi maadeavastajad** – kui kasutate mis tahes [salvestamise Exploreri rakendusi](storage-explorers.md), saate tõenäoliselt tuleb värskendada salvestusruumi võti need rakendused.

Siin on pöörata teie salvestusruumi kiirklahvide protsessi:

1. Värskendage oma rakenduse koodi viitamiseks teisene kiirklahv salvestusruumi konto ühenduse stringid.

2. Esmane kiirklahv salvestusruumi konto taastada. Klõpsake [Azure klassikaline portaali](https://manage.windowsazure.com)armatuurlaud või lehe **konfigureerimine** **Haldamine võtmed**. Klõpsake jaotises esmane kiirklahv **taastada** , ja seejärel nuppu **Jah** kinnitada, et soovite luua uue tootenumbri.

3. Värskendage ühendusstringi viitamiseks uus esmane kiirklahv koodi.

4. Teisene kiirklahv taastada.

## <a name="delete-a-storage-account"></a>Salvestusruumi konto kustutamine

Salvestusruumi konto, mida te enam ei kasuta eemaldamiseks kasutage armatuurlaual või lehe **konfigureerimine** **kustutada** . **Kustutamine** kustutab kogu salvestusruumi konto, sh kõik plekid, tabelite ja järjekorrad konto.

> [AZURE.WARNING] Ei saa taastada kustutatud salvestusruumi konto või mis tahes sisu, mis sisaldab see enne kustutamist tuua. Kindlasti midagi, mida soovite salvestada enne konto kustutamist varundada. See kehtib ressursse soovitud kontos – kui kustutate bloobimälu tabeli järjekorda või fail, kustutatakse jäädavalt.
>
> Kui teie konto salvestusruumi sisaldab VHD failid on Azure virtuaalse masina, peate kustutama soovitud pildid ja ketast, mis kasutavad nende VHD faile, enne kui saate kustutada salvestusruumi konto Esmalt virtuaalse masina lõpetada, kui see töötab ja kustutage see. Kustuta ketast, liikuge vahekaardile **ketast** ja kustutada kõik ketast seal. Piltide kustutamiseks liikuge menüüd **pildid** ja mis tahes pilte, mis on talletatud konto kustutada.

1. Klõpsake [Azure klassikaline portaali](https://manage.windowsazure.com) **salvestusruumi**.

2. Klõpsake suvalist salvestusruumi konto kirje peale nime ja seejärel klõpsake käsku **Kustuta**.

     - Või -

    Klõpsake armatuurlaual avamiseks salvestusruumi konto nimi ja seejärel klõpsake käsku **Kustuta**.

3. Klõpsake nuppu **Jah** , et kinnitada, et soovite konto salvestusruumi kustutada.

## <a name="next-steps"></a>Järgmised sammud

- Azure'i salvestusruumi kohta leiate lisateavet teemast [Azure Storage dokumentatsiooni](https://azure.microsoft.com/documentation/services/storage/).
- Leiate [Azure'i salvestusruumi meeskonna ajaveebi](http://blogs.msdn.com/b/windowsazurestorage/).
- [AzCopy käsurea kasuliku andmete edastamine](storage-use-azcopy.md)
