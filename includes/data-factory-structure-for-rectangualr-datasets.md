## <a name="specifying-structure-definition-for-rectangular-datasets"></a>Ristküliku andmekomplektide struktuuri definitsiooni määramine
Struktuur jaotisele andmekomplektide JSON on **valikulise** jaotise ristküliku tabelite (koos read ja veerud) ja see sisaldab tabeli veergude kogumi. Kasutage struktuuri jaotis tüüp teisenduste kummagi annab teavet või teha veeruvastenduste. Järgmistes jaotistes kirjeldatakse neid funktsioone üksikasjalikult. 

Iga veerg sisaldab järgmised atribuudid:

| Atribuut | Kirjeldus | Nõutav |
| -------- | ----------- | -------- |
| Nimi | Veeru nimi. | Jah |
| tüüp | Veeru andmetüüpi. Dokumenditeisenduste jaotis all jaoks rohkem üksikasju kohta, millal peaks teie määratud tüüpi teavet teemast | Ei |
| Kultuur | .Net-i põhiste kasutada siis, kui tüüp on määratud ja .net-i tüüp, kuupäev ja kellaaeg või Datetimeoffset kultuur. Vaikimisi on "en-us".  | Ei |
| vormindamine | Vormingustring kasutada siis, kui tüüp on määratud ja .NET on tippige Datetime või Datetimeoffset. | Ei |

Järgmises näites kuvatakse struktuuri jaotis JSON tabel, milles on kolm veergude kasutajanimi, nimi ja lastlogindate.

    "structure": 
    [
        { "name": "userid"},
        { "name": "name"},
        { "name": "lastlogindate"}
    ],

Palun pidage silmas järgmisi juhiseid lisada teavet "struktuur" ja mida **struktuuri** jaotise lisada.

- **Struktureeritud andmeallikaid** selle salvestada andmed koos andmed (allikad nagu Oracle, SQL Server Azure'i tabel jne), "struktuur" jaotis tuleks määrata ainult siis, kui soovite ise skeemi ja tippige teavet tehke veeru kaardistamine kindla allika veergude valamu soovitud veerud ja nende nimed pole sama (vt üksikasjad veeru vastendamise jaotist allpool). 

    Eespool nimetatud tüüpi teavet on valikuline jaotis "struktuur". Liigendatud allikatest, teabe tüüp on juba olemas osana andmekomplekti määratlus andmesalve nii te ei tohiks hõlmata tüüpi teavet kui kaasate "struktuur" jaotis.
- **Skeemi kohta vt andmeallikate (spetsiaalselt Azure'i bloobimälu)** saate ilma andmetega skeemi või tippige teabe talletamise andmete talletamiseks. Seda tüüpi andmeallikaid tuleks kaasata "struktuur" 2 järgmistel juhtudel:
    - Soovitud toiming veeru vastendamise.
    - Kui andmekomplekti on allikas Kopeeri alusel, saate sisestada tüüpi teavet "struktuur" ja andmete factory kasutab seda tüüpi teavet teisendamiseks valamu kohalikke tüübid. Vaadake teemat [andmete ja sealt Azure'i bloobimälu](../articles/data-factory/data-factory-azure-blob-connector.md) artikkel rohkem teavet.

### <a name="supported-net-based-types"></a>Toetatud. NET põhinev tüübid 
Andmete factory toetab vt andmeallikate nagu Azure'i bloobimälu skeemi järgmised CLS nõuetele vastavuse .net-i põhiste tüüp väärtused tüüpi teabe "struktuur".

- Int16
- Int32 
- Int64
- Ühe
- Kahekordne
- Decimal
- Byte]
- Bool
- String 
- GUID
- Kuupäev ja kellaaeg
- Datetimeoffset
- Kuuline ajavahemik 

Kuupäeva ja kellaaja & Datetimeoffset saate soovi korral ka hõlbustamiseks sõelumine oma kohandatud kuupäeva ja kellaaja stringi "Kultuur" ja "vormindamine" stringi. Valimi tüübi muutmise alltoodud teemast.

