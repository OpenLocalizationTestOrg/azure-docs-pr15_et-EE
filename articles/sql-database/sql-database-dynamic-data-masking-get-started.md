<properties
   pageTitle="Alustamine SQL-i andmebaasi dünaamiliste andmete Masking (Azure'i portaal)"
   description="Kuidas alustada SQL-i andmebaasi dünaamiliste andmete Masking Azure'i portaalis"
   services="sql-database"
   documentationCenter=""
   authors="ronitr"
   manager="jhubbard"
   editor="v-romcal"/>

<tags
   ms.service="sql-database"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="07/10/2016"
   ms.author="ronitr; ronmat; v-romcal; sstein"/>


# <a name="get-started-with-sql-database-dynamic-data-masking-azure-portal"></a>Alustamine SQL-i andmebaasi dünaamiliste andmete Masking (Azure'i portaal)

> [AZURE.SELECTOR]
- [Dünaamiliste andmete kaitset – klassikaline Azure'i portaal](sql-database-dynamic-data-masking-get-started-portal.md)

## <a name="overview"></a>Ülevaade

SQL-i andmebaasi dünaamiliste andmete Masking piiratakse tundlikku teavet käes masking it-õigustega kasutajad. Dünaamiliste andmete kaitset Azure'i SQL-andmebaasi V12 versioon on toetatud.

Dünaamiliste andmete kaitset aitab tundliku teabe volitamata juurdepääsu vältimiseks, võimaldades klientide määrata palju tundlikku andmete rakenduse kiht minimaalse mõju avaldada. See on poliitika põhineva turvalisuse funktsioon, mis peidab tundliku loomuga andmete päringu tulemite hulk üle määratud andmebaasi välju, kui andmed andmebaasis on muutunud.

Näiteks esindajaga kõnekeskuse võib tuvastada helistajad ja nende isikukood või krediitkaardi number mitu viimast, kuid need andmed üksused peaksid täielikult kokku puutuda teenuse esindajaga. Saate määratleda varjab reegli kõik maskid, kuid neli viimast numbrit isikukood või krediitkaardi number tulemis määrata mis tahes päringu. Teine näide saate mõne õigete andmete maski määratleda isikuttuvastava teabe (PII) andmete kaitsmiseks nii, et arendaja saate päringu tootmise keskkondades ilma rikub nõuetele vastavus määruste tõrkeotsinguks.

## <a name="sql-database-dynamic-data-masking-basics"></a>SQL-i andmebaasi dünaamiliste andmete Masking põhialused

Saate häälestada dünaamiline andmeid, masking poliitika portaalis Azure SQL-andmebaasi konfigureerimine tera või sätted blade dünaamiliste andmete Masking toiming valides.


### <a name="dynamic-data-masking-permissions"></a>Dünaamilise varjab õigused

Dünaamiliste andmete kaitset saab konfigureerida Azure'i andmebaasi administraator, serveri administraator või turvalisus käsutaja rollid.

### <a name="dynamic-data-masking-policy"></a>Dünaamilise varjab poliitika

* **Varjab välistatud kasutajate SQL** - SQL-i kasutajate või AAD identiteedid, mis saavad maskimata andmete SQL-päringu tulemid. Pange tähele, et administraatori õigustega kasutajad alati jäetakse varjab algsed andmed ilma mis tahes mask kuvatakse.

* **Masking reeglid** – reeglid, mis määratlevad määratud väljad, et olla maskeeritud ja varjab funktsioon, mida kasutatakse kogumi. Andmebaasi skeemi nimi, tabeli nimi ja veeru nimi abil saate määratleda määratud väljad.

* **Funktsioonide masking** - kogumi meetodid, mis juhib andmete erinevaid stsenaariumeid lähemalt käes.

| Funktsioon Masking | Loogika Masking |
|----------|---------------|
| **Vaikimisi**  |**Täielik varjab vastavalt määratud väljade andmetüübid**<br/><br/>• Kasutamine XXXX või Xs vähem kui välja maht on väiksem kui 4 märkide stringi andmetüübid (nchar, ntext, nvarchar).<br/>• Kasutada nullväärtus arvandmete tüüpe (suur täisarv, bit, koma, int, raha, arv, smallint, smallmoney, tinyint, hõljumine, real).<br/>• Kasutada 01-01-1900 kuupäeva/kellaaja andmetüüpide (kuupäev, datetime2, kuupäev ja kellaaeg, datetimeoffset, smalldatetime, aeg).<br/>• SQL-i variant vaikeväärtust, milleks on praeguse tüüp on kasutada.<br/>• XML dokumendi <masked/> kasutatakse.<br/>• Kasutada tühja väärtust teisiti andmetüübid (ajatempli tabeli hierarchyid, GUID, kahendarvuks, image, varbinaar ruumilise tüüpi).
| **Krediitkaardiga maksmine** |**Masking meetod, mis pakub neli viimast numbrit määratud väljade** ja lisab konstandi stringi eesliide krediitkaardiga kujul.<br/><br/>XXXX XXXX-XXXX 1234|
| **Isikukood** |**Masking meetod, mis pakub neli viimast numbrit määratud väljade** ja lisab konstandi stringi eesliide Ameerika isikukood kujul.<br/><br/>XXX-XX-1234 |
| **E-posti** | **Masking meetod, mis seab esimese tähe ja asendab XXX.com domeeni** e-posti aadressi kujul konstandi stringi eesliite abil.<br/><br/>aXX@XXXX.com |
| **Juhuslik arv** | **Masking meetod, mis on genereeritud juhusliku arvu** vastavalt valitud piirmäärad ja tegelike andmete tüüpi. Kui määratud piirid on võrdne, siis funktsioon varjab on konstandiga.<br/><br/>![Navigeerimispaan](./media/sql-database-dynamic-data-masking-get-started/1_DDM_Random_number.png) |
| **Kohandatud teksti** | **Masking meetod, mis seab esimese ja viimase märgid** ja lisab kohandatud täidise stringi keskel. Kui algse stringi on lühem exposed ees- ja järelliide, kasutatakse ainult täidise string. <br/>eesliite [täidise] järelliide<br/><br/>![Navigeerimispaan](./media/sql-database-dynamic-data-masking-get-started/2_DDM_Custom_text.png) |


<a name="Anchor1"></a>
### <a name="recommended-fields-to-mask"></a>Soovitatavate väljade mask

DDM soovitused engine lipuga teatud väljade andmebaasi nagu potentsiaalselt tundlik väljad, mis võib olla hea varjab. Dünaamiliste andmete Masking tera portaalis, kuvatakse Soovitatavad veergude andmebaasi. Tegemiseks on vaja **Maski lisada** ühe või mitme veeru jaoks, ja seejärel **salvestage** mask neid välju rakendamiseks klõpsake nuppu.

## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Dünaamiliste andmete kaitset Azure'i portaalis andmebaasi häälestamine

1. Käivitage Azure portaali aadressil [https://portal.azure.com](https://portal.azure.com).

2. Avage andmebaas, mis sisaldab tundliku andmeid, mida soovite peita tera sätted.

3. Klõpsake käivitab **Dünaamiliste andmete Masking** konfiguratsiooni tera **Dünaamiliste andmete Masking** paani.

    * Teise võimalusena saate liikuge kerides jaotiseni **toimingud** ja klõpsake **Dünaamiliste andmete Masking**.

    ![Navigeerimispaan](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>


4. **Dünaamiliste andmete Masking** konfiguratsiooni tera võidakse kuvada soovitusi engine on lipuga märgitud varjab andmebaasi veerge. Selleks, et aktsepteerida soovitusi, klõpsake lihtsalt **Maski lisada** ühe või mitme veeru jaoks ja mask luuakse vaikimisi tüüp veeru alusel. Saate muuta, klõpsates varjab reegli ja redigeerimine soovitud varjab varjab funktsiooni välja vorming mõnda muusse vormingusse oma valik. Kindlasti **salvestage** sätete salvestamiseks klõpsake.

    ![Navigeerimispaan](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>


5. Andmebaasi mask mis tahes veeru lisamiseks klõpsake **Dünaamiliste andmete Masking** konfiguratsiooni tera ülaosas **Lisada maski** **Masking reegli lisamise** konfiguratsiooni tera avamiseks

    ![Navigeerimispaan](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>

6. Valige **skeemi**, **Tabel** ja **veerg** määratud välja, mis maskeeritud määratlemiseks.

7. Valige loendist kategooriad masking andmete **Masking välja vorming** .

    ![Navigeerimispaan](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>     

8. Klõpsake andmeid masking reegli blade kogumi masking reeglite masking poliitika dünaamiline andmete värskendamiseks nuppu **Salvesta** .

9. Tippige SQL-i kasutajate või AAD identiteedid ei tohiks varjab, mis on juurdepääs maskimata tundliku loomuga andmeid. See peaks olema semikooloniga eraldatud kasutajate loendit. Pange tähele, et administraatori õigustega kasutajad alati juurdepääsu maskimata algsed andmed.

    ![Navigeerimispaan](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)

    >[AZURE.TIP] Muuta nii, et rakenduse kiht saate rakenduse au kasutajate tundliku loomuga andmeid kuvada, SQL-i kasutaja või AAD identiteedi rakendus kasutab päringu andmebaasi lisada. See on soovitatav, et see loend sisaldavad minimaalsete arvu õigustega kasutajad käes tundliku loomuga andmete minimeerimiseks.

10. Klõpsake masking konfiguratsiooni blade salvestamine uue või värskendatud varjab poliitika andmed nuppu **Salvesta** .

## <a name="set-up-dynamic-data-masking-for-your-database-using-powershell-cmdlets"></a>Dünaamiliste andmete masking PowerShelli cmdlet-käskude kasutamine andmebaasi häälestamine

Teemast [Azure SQL-i andmebaasi cmdlet-käsud](https://msdn.microsoft.com/library/azure/mt574084.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-rest-api"></a>Dünaamiliste andmete kaitset REST API abil andmebaasi häälestamine

Lugege teemat [toimingud SQL Azure'i andmebaasid](https://msdn.microsoft.com/library/dn505719.aspx).
