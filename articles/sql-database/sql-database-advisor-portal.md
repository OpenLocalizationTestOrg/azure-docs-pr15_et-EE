<properties 
   pageTitle="Azure SQL-i andmebaasi Advisor Azure'i portaalis | Microsoft Azure'i" 
   description="Azure'i portaalis Azure SQL-i andmebaasi Advisor saate läbi vaadata ja rakendada oma olemasolevaid SQL andmebaase, saate praeguse päringu jõudlust parandada." 
   services="sql-database" 
   documentationCenter="" 
   authors="stevestein" 
   manager="jhubbard" 
   editor="monicar"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="data-management" 
   ms.date="09/30/2016"
   ms.author="sstein"/>

# <a name="sql-database-advisor-using-the-azure-portal"></a>SQL-i andmebaasi Advisor Azure'i portaalis

> [AZURE.SELECTOR]
- [SQL-i andmebaasi Advisor ülevaade](sql-database-advisor.md)
- [Portaal](sql-database-advisor-portal.md)

Azure'i portaalis Azure SQL-i andmebaasi Advisor saate läbi vaadata ja rakendada oma olemasolevaid SQL andmebaase, saate praeguse päringu jõudlust parandada.

## <a name="viewing-recommendations"></a>Soovitused vaatamine

Soovitused leht on, kus kuvada soovitusi vastavalt nende mõjutada jõudluse parandamiseks. Saate vaadata ka ajalooliste toimingute olekut. Valige soovitused või oleku kuvamiseks rohkem üksikasju.

Vaatamine ja rakendada soovitusi, peate Azure'i õiguste õige [Rollipõhine juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md) . **Lugeja**, **SQL-i DB** osalusõigused on vaja vaade soovitused ja **omanik**, **SQL-i DB** osalusõigused on vaja käivitada kõik toimingud; luua või langev registrid ja tühistada registri loomine.

1. [Azure'i portaali](https://portal.azure.com/)sisse logida.
2. Klõpsake nuppu **rohkem teenuseid** > **SQL andmebaase**, ja valige oma andmebaasi.
5. Klõpsake **jõudluse soovitus** kuvamiseks saadaval soovitused valitud andmebaasi.

> [AZURE.NOTE] Saada soovitused andmebaasi peab olema päeva kasutamise kohta ja on vaja teha mõned tegevuse. Samuti on vaja mõned süsteemset tegevust. SQL-andmebaasi Advisor saate kergemini optimeerida ühtsete päringu mustrite kui saate juhusliku täpiline puruneb tegevuse. Kui soovitusi ei ole saadaval, **jõudluse soovitus** lehe peaks andma sõnumile, mis selgitab, miks.

![Soovitused](./media/sql-database-advisor-portal/recommendations.png)

Siin on näide "Skeemi probleemi lahendamiseks" soovitus Azure'i portaalis.

![Skeemi probleemi lahendamiseks](./media/sql-database-advisor-portal/sql-database-advisor-schema-issue.png)

Soovitused on sorditud võib mõjutada jõudlust järgmist nelja kategooriasse:

| Mõju | Kirjeldus |
| :--- | :--- |
| Kõrge | Kõrge mõju soovitused peaks andma kõige olulisem jõudlust mõjutada. |
| Keskmine | Keskmine mõju soovitused tuleks parandada jõudlust, kuid mitte oluliselt. |
| Madal | Väike mõju soovitused peaks andma töökindluse kui ilma, kuid ei pruugi olulised täiustused. 


### <a name="removing-recommendations-from-the-list"></a>Soovitused eemaldamine loendist

Kui teie loendist soovitused sisaldab üksusi, mida soovite loendist eemaldada, saate hüljata soovitust:

1. Valige loendist **soovitused**soovituse.
2. Enne **üksikasjad** nuppu **Hülga** .


Soovi korral saate lisada kasutuselt kõrvaldatud üksuste tagasi **soovituste** loendi:

1. **Soovitused** enne nuppu **Kuva hüljata**.
1. Valige loendist üksikasjade kuvamiseks kasutuselt kõrvaldatud üksus.
1. Soovi korral klõpsake lisamiseks registri peamine loendist **soovitused** **Tagasivõtmine Hülga** .



## <a name="applying-recommendations"></a>Soovitused rakendamine

SQL-i andmebaasi Advisor annab teile täielik kontroll Kuidas soovitused on lubatud, kasutades ühte järgmised kolm võimalust: 

- Rakendada üksiku soovitused ühe korraga.
- Soovitused automaatseks rakendamiseks advisor lubamine (kehtib praegu ainult index soovitused).
- Rakendada soovitus käsitsi, käivitage soovitatav T-SQL-i skripti oma andmebaasist.

Valige mõni soovitus selle üksikasjade kuvamine ja seejärel klõpsake nuppu **Kuva skript** läbivaatamiseks üksikasjalik kirjeldus, kuidas soovitus on loodud.

Andmebaasi jääb online ajal nõustaja kehtib soovitus--abil SQL-i andmebaasi Advisor kunagi võtab andmebaasi ühenduseta.

### <a name="apply-an-individual-recommendation"></a>Rakendada ka üksikute soovitus

Saate vaadata ja nõustuge soovitused ühe korraga.

1. Enne **soovitusi** , klõpsake soovitust.
2. **Üksikasju** enne klõpsake nuppu **Rakenda**.

    ![Soovitus rakendamine](./media/sql-database-advisor-portal/apply.png)

### <a name="enable-automatic-index-management"></a>Registri automaatne haldamise lubamine

Saate seada SQL-i andmebaasi Advisor soovituste rakendamiseks automaatselt. Kui soovitusi muutuvad kättesaadavaks need rakendatakse automaatselt. Kui kõik registri toimingute hallatav teenus võttis kui jõudluse mõju on negatiivne soovitust tagasi.

1. Enne **soovitusi** , klõpsake **automatiseerimine**.

    ![Advisor sätted](./media/sql-database-advisor-portal/settings.png)

2. Määrata nõustaja automaatne **loomine** või **langev** registrid.

    ![Soovitatav registrid](./media/sql-database-advisor-portal/automation.png)


### <a name="manually-run-the-recommended-t-sql-script"></a>Soovitatav T-SQL-skript käsitsi käivitamine

Valige mõni soovitus ja klõpsake nuppu **Kuva skript**. Andmebaasi käsitsi rakendamiseks soovitus selle skripti vastuolus.

*Registrid, mis on käsitsi tehtud on jälgida ja jõudluse mõju teenus võttis kinnitatud* nii soovitatakse teil jälgida nende indeksite pärast loomist pakuvad jõudluse tõstmiseks ja reguleerida või kustutage need vajadusel kinnitamiseks. Registrite loomise kohta leiate üksikasjalikumat teavet teemast [CREATE INDEX (Transact-SQL-i)](https://msdn.microsoft.com/library/ms188783.aspx).


### <a name="canceling-recommendations"></a>Soovitused tühistamine

Soovitused, mis on **Ootel**, **Verifying**või **edu** olek saate tühistada. Soovitused olekuga **Executing** ei saa tühistada.

1. Märkige jaotises **Ajaloo häälestamine** avamiseks **soovitused üksikasjad** tera soovituse.
2. Klõpsake nuppu **Tühista** katkestada protsessi soovituse rakendamist.



## <a name="monitoring-operations"></a>Toimingute jälgimiseks

Rakendades soovitus võib ei juhtu hetkega. Portaalis on soovitus toimingute oleku kohta. Järgnevalt on võimalik olekus, mida saab registri.

| Olek | Kirjeldus |
| :--- | :--- |
| Ootel | Käsk sai ja ajaks ajastada soovitus rakendada. |
| Täitmine | Soovitus rakendatakse. |
| Edu | Soovitus on rakendatud. |
| Tõrge | Soovitus rakendamise käigus ilmnes tõrge. See võib olla ajutine probleem või võib-olla skeemi muuta tabeli ja skripti ei kehti enam. |
| Taastamist | Soovitus on rakendatud, kuid on peetud kiire ja automaatselt taastatud. |
| Taastatud | Soovitus on taastatud. |

Klõpsake-protsess soovitus loendist täpsema teabe kuvamiseks.

![Soovitatav registrid](./media/sql-database-advisor-portal/operations.png)


### <a name="reverting-a-recommendation"></a>Taastamist soovitus

Kui kasutasite nõustaja soovituse rakendamiseks (st saate käsitsi ei tööta T-SQL-skript) automaatselt taastatakse selle jõudluse mõju negatiivne leidmisel. Kui mingil põhjusel soovite taastada soovitus lihtsalt saate teha järgmist:


1. Märkige jaotises **Tuning ajalugu** edukalt rakendatud soovitus.
2. Enne **soovituse üksikasjad** nuppu **Taasta** .

![Soovitatav registrid](./media/sql-database-advisor-portal/details.png)


## <a name="monitoring-performance-impact-of-index-recommendations"></a>Jõudluse mõju index soovituste jälgimine

Pärast soovituste rakendamine (praegu indekseerida toimingute ja päringute soovitused ainult parameterize) saate klõpsata **Päringu ülevaateid** soovitus üksikasju enne [Päringu jõudluse ülevaateid](sql-database-query-performance.md) avada ja vaadata oma põhipäringud jõudlust mõjutada.

![Kuvari jõudluse mõju](./media/sql-database-advisor-portal/query-insights.png)



## <a name="summary"></a>Kokkuvõte

SQL-i andmebaasi Advisor pakub soovitused SQL-i andmebaasi jõudluse parandamiseks. T-SQL-i skriptide pakkudes üksikisiku ja täielikult automaatne (praegu indeksi ainult) nõustaja annab kasulik abi optimeerimine andmebaasi ja lõpuks päringu jõudluse parandamiseks.



## <a name="next-steps"></a>Järgmised sammud

Jälgida oma soovitused ja neid piiritlemiseks jõudluse jätkata. Andmebaasi töökoormus on dünaamiline ja pidevalt muuta. SQL-andmebaasi advisor jätkab jälgimine ja soovitusi, mis võivad potentsiaalselt teie andmebaasi jõudlust parandada. 

 - [SQL-i andmebaasi Advisor](sql-database-advisor.md) leiate ülevaate SQL-i andmebaasi Advisor.
 - [Päringu jõudluse ülevaateid](sql-database-query-performance.md) jõudluse mõju teie põhipäringud vaatamise kohta leiate teemast.

## <a name="additional-resources"></a>Lisaressursid

- [Päringu pood](https://msdn.microsoft.com/library/dn817826.aspx)
- [REGISTRI LOOMINE](https://msdn.microsoft.com/library/ms188783.aspx)
- [Rollipõhine juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md)






