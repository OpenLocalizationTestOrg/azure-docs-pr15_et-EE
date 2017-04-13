<properties
    pageTitle="DocumentDB globaalne andmebaasi tiražeerimine | Microsoft Azure'i"
    description="Saate teada, kuidas hallata globaalne dispersioonanalüüs konto DocumentDB Azure portaali kaudu."
    services="documentdb"
    keywords="Globaalne andmebaasi, dispersioonanalüüs"
    documentationCenter=""
    authors="mimig1"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="documentdb"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="mimig"/>

# <a name="how-to-perform-documentdb-global-database-replication-using-the-azure-portal"></a>Kuidas teha DocumentDB globaalne andmebaasi tiražeerimine Azure'i portaalis

Saate teada, kuidas kasutada Azure portaali andmete mitme piirkonna jaoks üld-saadavus andmete Azure'i DocumentDB korrata.

Lisateavet selle kohta, kuidas globaalne andmebaasi dispersioonanalüüs töötab DocumentDB leiate teemast [hajuta andmeid globaalselt DocumentDB](documentdb-distribute-data-globally.md). Läbimiseks globaalse andmebaasi tiražeerimine programmiliselt kohta leiate teemast [mitme piirkonna DocumentDB kontode arendamine](documentdb-developing-with-multiple-regions.md).

> [AZURE.NOTE] Globaalse jaotuse andmebaase DocumentDB on üldiselt kättesaadav ja mis tahes vastloodud DocumentDB kontode jaoks automaatselt lubatud. Töötame kõik olemasolevad kontodega globaalne jagamise lubamiseks, kuid vahepeal kui soovite, et teie kontol lubatud levitamist [tugiteenuste](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ja meil kuvatakse Lubage see teie eest kohe.

## <a id="addregion"></a>Lisage globaalse andmebaasi piirkondade

DocumentDB on saadaval enamikes [Azure piirkondade] [azureregions]. Pärast andmebaasi konto jaoks vaikimisi vormindusühtsuse taset, saate ühes või mitmes piirkonnas (olenevalt teie valitud vaikimisi vormindusühtsuse taseme ja globaalse jaotuse vajadustele) seostada.

1. [Azure'i portaalis](https://portal.azure.com/)Jumpbar, klõpsake käsku **DocumentDB kontod**.
2. Valige **Konto DocumentDB** labale andmebaasi konto, mida soovite muuta.
3. Konto tera, klõpsake **andmete globaalselt ise** menüüst.
4. Piirkondade lisamiseks või eemaldamiseks valige labale **ise andmete globaalselt** ja klõpsake siis nuppu **Salvesta**. Ei kulu lisamise regioonid, vt [hinnad lehe](https://azure.microsoft.com/pricing/details/documentdb/) või [hajuta andmeid globaalselt DocumentDB](documentdb-distribute-data-globally.md) artiklis lisateabe saamiseks.

    ![Klõpsake kaardi lisamiseks või eemaldamiseks nende piirkondade][1]

### <a name="selecting-global-database-regions"></a>Globaalne andmebaasi piirkondade valimine

Kahe või enama piirkondade konfigureerimisel on soovitatav piirkondade valitud piirkond paari, mis on kirjeldatud selle [Business järjepidevus ja Avariijärgne taaste (BCDR): Azure'i paaris piirkondade]  [ bcdr] artikkel.

Täpsemalt mitme piirkonna konfigureerimisel veenduge, et valida andmepunktipaaride piirkonna veerus ühepalju piirkondade (+/ – 1 paaritu). Näiteks kui soovite võtta kasutusele neli USA regiooni, valige kahe USA piirkonna vasaku veeru ja kahe alates paremalt. Nii, järgmine oleks kohased: Lääne USA, Ida-USA, Põhja keskse meil ja Lõuna keskse meil.

Juhised on oluline järgida ainult mõlema piirkonna jaoks Avariijärgne taaste stsenaariumid konfigureerimisel. Rohkem kui kaks regioonid, see juhiste on hea tava, kuid pole kriitiline, kui mõne valitud piirkondades järgida selles sidumine.

<!---
## <a id="selectwriteregion"></a>Select the write region

While all regions associated with your DocumentDB database account can serve reads (both, single item as well as multi-item paginated reads) and queries, only one region can actively receive the write (insert, upsert, replace, delete) requests. To set the active write region, do the following  


1. In the **DocumentDB Account** blade, select the database account to modify.
2. In the account blade, if the **All Settings** blade is not already opened, click **All Settings**.
3. In the **All Settings** blade, click **Write Region Priority**.
    ![Change the write region under DocumentDB Account > Settings > Add/Remove Regions][2]
4. Click and drag regions to order the list of regions. The first region in the list of regions is the active write region.
    ![Change the write region by reordering the region list under DocumentDB Account > Settings > Change Write Regions][3]
-->

## <a id="next"></a>Järgmised sammud

Saate teada, kuidas [järjepidevuse tasemete DocumentDB](documentdb-consistency-levels.md)lugedes järjepidevuse globaalselt tiražeeritud konto haldamine.

Lisateavet selle kohta, kuidas globaalne andmebaasi dispersioonanalüüs töötab DocumentDB leiate teemast [hajuta andmeid globaalselt DocumentDB](documentdb-distribute-data-globally.md). Programmiliselt imitatsiooniga andmete mitme piirkonna kohta leiate teemast [mitme piirkonna DocumentDB kontode arendamine](documentdb-developing-with-multiple-regions.md).

<!--Image references-->
[1]: ./media/documentdb-portal-global-replication/documentdb-add-region.png
[2]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-1.png
[3]: ./media/documentdb-portal-global-replication/documentdb_change_write_region-2.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[bcdr]: https://azure.microsoft.com/documentation/articles/best-practices-availability-paired-regions/
[consistency]: https://azure.microsoft.com/documentation/articles/documentdb-consistency-levels/
[azureregions]: https://azure.microsoft.com/en-us/regions/#services
[offers]: https://azure.microsoft.com/en-us/pricing/details/documentdb/
