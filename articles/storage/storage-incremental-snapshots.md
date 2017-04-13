<properties
    pageTitle="Kasutage suureneva hetktõmmiseid varundamise ja taastamise Azure'i virtuaalmasinates | Microsoft Azure'i"
    description="Luua kohandatud lahenduse varundamise ja taastamise oma Azure virtuaalse masina ketast suureneva hetktõmmiseid abil."
    services="storage"
    documentationCenter="na"
    authors="aungoo-msft"
    manager="tadb"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="aungoo"/>


# <a name="back-up-azure-virtual-machine-disks-with-incremental-snapshots"></a>Azure virtuaalse masina ketast koos suureneva hetktõmmiseid varundamine

## <a name="overview"></a>Ülevaade

Azure'i salvestusruumi pakub võimalust plekid hetktõmmiste tegemiseks. Hetktõmmiseid jäädvustada bloobimälu olekus sel ajal. Selles artiklis kirjeldatakse, kuidas saate säilitada varukoopiaid virtuaalse masina ketast abil hetktõmmiseid luua stsenaarium. Kui te ei soovi kasutada Azure varundamise ja taastamise teenust ja soovite seda luua kohandatud varukoopia strateegia oma virtuaalse masina ketast, saate kasutada seda meetodit.

Lehe plekid Azure Storage salvestatakse Azure virtuaalse masina ketast. Kuna me räägi varukoopia strateegia selle artikli virtuaalse masina ketast, viidates me hetktõmmiseid lehe plekid kontekstis. Hetktõmmiste kohta lisateabe saamiseks vaadake [mõnda bloobimälu hetktõmmise loomine](https://msdn.microsoft.com/library/azure/hh488361.aspx).

## <a name="what-is-a-snapshot"></a>Mis on hetktõmmise?

Hetktõmmise bloobimälu on kirjutuskaitstud versioon bloobimälu, mis on jäädvustatud samal ajal aega. Kui hetktõmmise on loodud, seda saab lugeda, kopeerida, või kustutatud, kuid mitte muuta. Hetktõmmiseid võimalda varundamine on bloobimälu, nagu see kuvatakse ajahetkel. Kuni ülejäänud versioon 2015-04-05 pidite täielik pilte kopeerida. Ülejäänud versioon 2015-07-08 ja kohale, samuti saate kopeerida suureneva pilte.

## <a name="full-snapshot-copy"></a>Täielik hetktõmmise kopeerimine

Pilte saate kopeerida salvestusruumi teisele kontole nimega bloobimälu, hoida varukoopiaid base bloobimälu. Samuti saate kopeerida hetktõmmise üle base bloobimälu, mis sarnaneb selle bloobimälu varasema versiooni taastamine. Kui hetktõmmise kopeeritakse salvestusruumi ühelt kontolt teisele, see hõivata sama nimega base lehe bloobimälu ruumi. Seetõttu kopeerimise kogu salvestusruumi ühelt kontolt teisele hetktõmmiseid on aeglane ja ka tarbib palju ruumi target salvestusruumi konto.

>[AZURE.NOTE] Kui kopeerite base bloobimälu teise sihtkohta, ei kopeerita soovitud bloobimälu hetktõmmiste koos sellega. Kui kirjutate base bloobimälu koopia, pilte, mis on seotud base bloobimälu ei mõjuta ja säilib base bloobimälu nime all.

### <a name="back-up-disks-using-snapshots"></a>Kasutades hetktõmmiseid ketast varundamine

Varukoopia strateegia oma virtuaalse masina ketast, saate võtta kettale või lehe bloobimälu perioodiliste hetktõmmiseid ja kopeerimiseks teise salvestusruumi konto tööriistadega nagu [Kopeeri bloobimälu](https://msdn.microsoft.com/library/azure/dd894037.aspx) toiming või [AzCopy](storage-use-azcopy.md). Saate kopeerida hetktõmmise sihtkoha lehe bloobimälu erineva nimega. Tulemuseks sihtkoha lehe bloobimälu on kirjutatav lehe bloobimälu ja pole hetktõmmise. Selles artiklis kirjeldatakse juhiseid varukoopiaid virtuaalse masina ketast abil hetktõmmiste tegemiseks.

### <a name="restore-disks-using-snapshots"></a>Kasutades hetktõmmiseid ketast taastamine

Kui on aeg kõvakettal taastamine varukoopia hetktõmmiseid jäädvustatud ühed varasema versiooni, saate kopeerida hetktõmmise base lehe bloobimälu üle. Pärast hetktõmmis on esiletõstetud base lehele Bloobivahemälu, jääb hetktõmmise, kuid selle andmeallika kirjutatakse koopia, mida saab nii lugeda ja kirjutada. Selles artiklis kirjeldatakse juhiseid oma hetktõmmise kõvakettal varasema versiooni taastamine.

### <a name="implementing-full-snapshot-copy"></a>Täielik hetktõmmise Kopeeri rakendamine

Saate rakendada hetktõmmise täielik koopia, tehes järgmist

-   Esmalt base bloobimälu, [Hetktõmmise bloobimälu](https://msdn.microsoft.com/library/azure/ee691971.aspx) toiminguga hetktõmmise tegemiseks.
-   Kopeerige hetktõmmis target salvestusruumi konto [Kopeeri bloobimälu](https://msdn.microsoft.com/library/azure/dd894037.aspx)abil.
-   Korrake seda toimingut säilitada oma base bloobimälu varukoopiad.

## <a name="incremental-snapshot-copy"></a>Suureneva hetktõmmise kopeerimine

Uue funktsiooni [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) API pakub parem viis, hetktõmmiste lehe plekid või ketast varundada. API tagastab loendi muudatused base bloobimälu ja tõmmised vahel. See vähendab varukoopia kontol kasutatud salvestusruumi. API toetab lehe plekid Premium nii, et kindlad. See API abil saate nüüd koostada kiirem ja tõhusam varukoopia lahenduste Azure'i vms. See on saadaval ülejäänud versiooniga 2015-07-08 ja uuem versioon.

Suureneva hetktõmmise Kopeeri võimaldab kopeerida salvestusruumi ühelt kontolt teisele vahe,

-   Base bloobimälu ja selle hetktõmmise või
-   Mis tahes kahe hetktõmmiste base bloobimälu

Järgmised tingimused on täidetud,

- Funktsiooni bloobimälu loodi jaan-1-2016 või uuema versiooniga.
- Funktsiooni bloobimälu ei kirjutata [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) või [Kopeeri bloobimälu](https://msdn.microsoft.com/library/azure/dd894037.aspx) kaks hetktõmmiseid vahel.


**Märkus**: See funktsioon on saadaval Premium ja Standard Azure lehe plekid.

Kui teil on kohandatud varukoopia strateegia, mis kasutab pilte, kopeerides tõmmised salvestusruumi ühelt kontolt teisele võib olla väga aeglane ja tarbib palju salvestusruumi. Asemel kopeerida kogu hetktõmmise varukoopia salvestusruumi konto, saate kirjutada järjestikust hetktõmmist varukoopia lehe bloobimälu vahe. Sellisel viisil aega ruumi varukoopiate salvestamiseks käske Kopeeri ja märkimisväärselt vähendada.

### <a name="implementing-incremental-snapshot-copy"></a>Suureneva hetktõmmise Kopeeri rakendamine

Saate rakendada suureneva hetktõmmise koopia, tehes järgmist

-   Base bloobimälu, kasutades [Hetktõmmise bloobimälu](https://msdn.microsoft.com/library/azure/ee691971.aspx)hetktõmmise tegemiseks.
-   Kopeerige target varukoopia salvestusruumi konto abil [Kopeeri bloobimälu](https://msdn.microsoft.com/library/azure/dd894037.aspx)hetktõmmis. See on varukoopia lehe bloobimälu. Selle lehe varukoopia bloobimälu hetktõmmis ja konto varukoopia salvestada.
-   Võtta mõne muu hetktõmmist base bloobimälu, kasutades hetktõmmise bloobimälu.
-   Saada esimese ja teise hetktõmmiste base bloobimälu abil [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx)vahe. Uue parameetri **prevsnapshot** abil saate määrata kuupäeva ja kellaaja väärtuse soovite leida erinevus hetktõmmis. Kui see parameeter on olemas, ülejäänud vastus sisaldab ainult lehti, mis on muudetud target hetktõmmise ja eelmise hetktõmmise, sh Eemalda lehtede vahel.
-   Rakenda neid muutusi varukoopia lehe bloobimälu [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) abil.
-   Lõpuks varukoopia lehe bloobimälu hetktõmmis ja salvestada selle konto varukoopia säilitamist.

Järgmises jaotises kirjeldatakse lähemalt kuidas saate säilitada varukoopiaid ketast, kasutades suureneva hetktõmmise eksemplari

## <a name="scenario"></a>Stsenaarium

Selles jaotises kirjeldatakse stsenaarium, mis hõlmab kohandatud varukoopia strateegia virtuaalse masina ketast hetktõmmiseid abil.

Kaaluge võimalust premium salvestusruumi P30 kettal manustatud DS-sarja Azure VM. Nimega *mypremiumdisk* P30 ketas talletatakse premium salvestusruumi konto nimega *mypremiumaccount*. Kindlad konto, nimetatakse *mybackupstdaccount* kasutatakse salvestamiseks *mypremiumdisk*varukoopia. Kui soovime säilitada *mypremiumdisk* hetktõmmise iga 12 tunni järel.

Salvestusruumi konto ja ketast loomise kohta leiate Lisateavet vaadake [kohta Azure salvestusruumi kontod](storage-create-storage-account.md).

Azure VMs varundamise kohta lisateabe saamiseks vaadake [varukoopiate plaanimine Azure VM](../backup/backup-azure-vms-introduction.md).

## <a name="steps-to-maintain-backups-of-a-disk-using-incremental-snapshots"></a>Varukoopiate abil suureneva hetktõmmiseid ketta säilitamiseks juhised

Allpool kirjeldatud juhiseid võtab *mypremiumdisk* hetktõmmiseid luua ja hallata varukoopiate *mybackupstdaccount*sisse. Varundus on standardne lehe bloobimälu, nimetatakse *mybackupstdpageblob*. Varukoopia lehe bloobimälu kajastab alati sama olek *mypremiumdisk*viimase hetktõmmisena.

1.  Esmalt looge varukoopia lehe bloobimälu oma premium salvestusruumi ketas. Seda teha, *mypremiumdisk* hetktõmmis nimetatakse *mypremiumdisk_ss1*.
2.  Kopeerige selle hetktõmmise mybackupstdaccount nimega lehe bloobimälu, nimetatakse *mybackupstdpageblob*.
3.  *Mybackupstdpageblob* nimetatakse *mybackupstdpageblob_ss1*, kasutades [Hetktõmmise bloobimälu](https://msdn.microsoft.com/library/azure/ee691971.aspx) hetktõmmis ja salvestada selle *mybackupstdaccount*.
4.  Ajal varukoopia akna loomine mõne muu hetktõmmist *mypremiumdisk*, teatavad, et *mypremiumdisk_ss2*ja salvestada selle *mypremiumaccount*.
5.  Suureneva muutused kahe pilte, *mypremiumdisk_ss2* ja *mypremiumdisk_ss1*saamine, [GetPageRanges](https://msdn.microsoft.com/library/azure/ee691973.aspx) kasutamine *mypremiumdisk_ss2* **prevsnapshot** parameetriga määratud ajatempli *mypremiumdisk_ss1*. Kirjutage varukoopia lehe bloobimälu *mybackupstdpageblob* *mybackupstdaccount*need täiendavad muudatused. Kui täiendavad muudatused on kustutatud vahemikud, peab olema tühi bloobimälu varukoopia lehe kaudu. [PutPage](https://msdn.microsoft.com/library/azure/ee691975.aspx) abil varukoopia lehe bloobimälu suureneva muudatusi kirjutada.
6.  Funktsiooni varukoopia lehe bloobimälu *mybackupstdpageblob*, nimetatakse *mybackupstdpageblob_ss2*hetktõmmise tegemiseks. Eelmise hetktõmmise *mypremiumdisk_ss1* premium salvestusruumi konto kustutada.
7.  Korrake juhiseid 4 – 6 iga varukoopia akna. Sel viisil saate säilitada *mypremiumdisk* kindlad konto varukoopiaid.

![Suureneva hetktõmmiseid ketast varundamine](./media/storage-incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-to-restore-a-disk-from-snapshots"></a>Juhised hetktõmmiseid kettal taastamine

Premium kettale, varukoopia salvestusruumi konto *mybackupstdaccount* *mypremiumdisk* mõne varasema hetktõmmise taastab allpool kirjeldatud juhiseid.

1.  Leidke punkti aega, mida soovite taastada premium kettale. Oletame, et see on hetktõmmise *mybackupstdpageblob_ss2*, mis on salvestatud varukoopia salvestusruumi konto *mybackupstdaccount*.
2.  Edendada mybackupstdaccount, nagu uue varukoopia base lehe bloobimälu *mybackupstdpageblobrestored*hetktõmmise *mybackupstdpageblob_ss2* .
3.  See taastatud varukoopia lehe bloobimälu, nimetatakse *mybackupstdpageblobrestored_ss1*hetktõmmise tegemiseks.
4.  Kopeerige taastatud lehe bloobimälu *mybackupstdpageblobrestored* *mybackupstdaccount* *mypremiumaccount* premium uue ketta *mypremiumdiskrestored*nimega.
5.  *Mypremiumdiskrestored*, nimetatakse *mypremiumdiskrestored_ss1* tegemise tulevaste varundamiseks hetktõmmise tegemiseks.
6.  Punkti DS sarja VM on taastatud kettale *mypremiumdiskrestored* ja vana *mypremiumdisk* VM eemaldada.
7.  Alustage varundamise protsessi kirjeldatud on taastatud kettal *mypremiumdiskrestored*, kasutades *mybackupstdpageblobrestored* varukoopia lehe bloobimälu eelmisest jaotisest.

![Pilte ketta taastamine](./media/storage-incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet mõne bloobimälu hetktõmmiste loomise ja kavandamise oma VM varukoopia taristu abil saamiseks allpool olevaid linke.

- [Mõne bloobimälu hetktõmmise loomine](https://msdn.microsoft.com/library/azure/hh488361.aspx)
- [Teie VM varundamise taristu kavandamine](../backup/backup-azure-vms-introduction.md)
