<properties
    pageTitle="Jälgimine ja haldamine on elastne andmebaasi rakenduskausta Azure'i portaalis | Microsoft Azure'i"
    description="Teada, kuidas Azure'i portaal ja SQL-andmebaasi sisseehitatud ärianalüüsi abil hallata, jälgida ja paremalt-suurus scalable elastne andmebaasi pool optimeerimine andmebaasi jõudlus ja hallata maksumus."
    keywords=""
    services="sql-database"
    documentationCenter=""
    authors="ninarn"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="09/22/2016"
    ms.author="ninarn"
    ms.workload="data-management"
    ms.topic="article"
    ms.tgt_pltfrm="NA"/>


# <a name="monitor-and-manage-an-elastic-database-pool-with-the-azure-portal"></a>Jälgimine ja haldamine on elastne andmebaasi rakenduskausta Azure'i portaalis

> [AZURE.SELECTOR]
- [Azure'i portaal](sql-database-elastic-pool-manage-portal.md)
- [PowerShelli](sql-database-elastic-pool-manage-powershell.md)
- [C#:](sql-database-elastic-pool-manage-csharp.md)
- [T-SQL-IS](sql-database-elastic-pool-manage-tsql.md)


Azure portaali abil saate jälgida ja hallata kohapeal on elastne andmebaasi ja andmebaaside kogumi. Portaalist saate jälgida kohapeal on elastne ja selle kausta andmebaasidele kasutamist. Saate teha muudatusi teie elastne ühendada ja esitage kõigi muutuste korraga. See hõlmab järgmisi muudatusi lisamise või eemaldamise andmebaaside, elastne rakenduskausta muutmine või teie andmebaasi sätete muutmine.

Allpool pilt kuvatakse elastne kohapeal on näide. Vaate sisaldab järgmist:

*  Diagrammide ressursikasutus elastne rakenduskausta ja andmebaaside pool jälgida.
*  Muudatuste tegemiseks elastne pool nupp **konfigureerimine** pool.
*  **Andmebaasi loomine** nupp, mis loob uue andmebaasi ja lisab selle praegusele elastne pool.
*  Elastne tööd, mis aitavad teil haldamiseks vastu kõik andmebaasid loendis skriptid Transact SQL suure hulga andmebaasid.

![Rakenduskausta kuvamine][2]

Selles artiklis toodud juhiseid töötamiseks peate vähemalt üks andmebaas ja elastne kohapeal on Azure SQL serveri. Kui teil on elastne rakenduskausta, vaadake [koostada andmebaas](sql-database-elastic-pool-create-portal.md); Kui teil pole andmebaasi, vt [SQL-i andmebaasi õpetuse](sql-database-get-started.md).

## <a name="elastic-pool-monitoring"></a>Elastne rakenduskausta jälgimine

Saate minna kindla pool, et näha selle ressursi kasutamine. Vaikimisi on konfigureeritud kausta kuvamiseks salvestus- ja eDTU kasutus viimase tunni. Diagrammi saab üle erinevate kellaaeg Windowsi erinevate mõõdikute kuvamiseks konfigureerida.

1. Valige pool, töötamiseks.
2. **Elastne rakenduskausta jälgimise** all on diagrammi sildiga **Ressursi kasutamine**. Klõpsake diagrammi.

    ![Elastne rakenduskausta jälgimine][3]

    **Meetermõõdustik** tera avab nähtaval üksikasjaliku ülevaate erinevate määratud mõõdikute määratud ajaakna jooksul.   

    ![Argumendil blade][9]

### <a name="to-customize-the-chart-display"></a>Kohandada diagrammi kuvamine

Saate muuta diagrammi ja argumendil blade, nt CPU protsenti, andmete IO protsenti ja log IO protsendimäär, mida kasutatakse muude mõõdikute kuvamiseks.

2. Enne argumendil, klõpsake nuppu **Redigeeri**.

    ![Klõpsake nuppu Redigeeri][6]

- **Diagrammi redigeerimine** tera, valige uus kellaaeg vahemik (viimase tund täna, või viimase nädala jooksul) või klõpsake **kohandatud** valimiseks mis tahes kuupäevavahemik viimase kahe nädala jooksul. Diagrammi tüüp (riba või joon) valige ressursside jälgimiseks.

> Märkus: Ainult mõõdikute sama mõõtühik abil saab kuvada diagrammil samal ajal. Näiteks kui valite "eDTU protsent" siis ainult saab valida muu mõõdikute protsendiga mõõtühik.

    ![Click edit](./media/sql-database-elastic-pool-manage-portal/edit-chart.png)

- Klõpsake nuppu **OK**.


## <a name="elastic-database-monitoring"></a>Elastne andmebaasi jälgimine

Üksikute andmebaasid saate jälgida ka võimalikke probleeme.

1. **Elastne andmebaasi jälgimine**, all on diagramm, mis kuvatakse mõõdikute viis andmebaaside jaoks. Diagramm kuvab vaikimisi ülemise 5 andmebaasi kogumi, Keskmine eDTU kasutus tunnis. Klõpsake diagrammi.

    ![Elastne rakenduskausta jälgimine][4]

2. **Andmebaasi ressursi kasutamine** tera kuvatakse. See pakub andmebaasi kasutus kogumi üksikasjalik vaade. Kasuta ruudustiku tera alumises osas, saate valida mis tahes andmebaase, et kuvada selle kasutamine diagrammis (kuni 5 andmebaasid). Saate kohandada, klõpsates nuppu **Redigeeri diagrammi**diagrammis kuvatud mõõdikute ja kellaaja akna.

    ![Andmebaasi ressursi kasutamine blade][8]

### <a name="to-customize-the-view"></a>Vaate kohandamine

1. **Andmebaasi ressursi kasutamine** labale nuppu **Redigeeri diagrammi**.

    ![Klõpsake nuppu Redigeeri diagrammi](./media/sql-database-elastic-pool-manage-portal/db-utilization-blade.png)

2. **Redigeeri** diagrammi tera, valige uus kellaaeg vahemik (viimase tund või viimase 24 tunni jooksul) või klõpsake nuppu **kohandatud** valida mõne muu päeva kuvamiseks viimase 2 nädala jooksul.

    ![Klõpsake nuppu Kohandatud](./media/sql-database-elastic-pool-manage-portal/editchart-date-time.png)

3. Klõpsake ripploendit **võrdlus andmebaasi** erinevate mõõdiku kasutada andmebaaside võrdlemisel valimiseks.

    ![Diagrammi redigeerimine](./media/sql-database-elastic-pool-manage-portal/edit-comparison-metric.png)

### <a name="to-select-databases-to-monitor"></a>Valige andmebaase jälgimine

**Andmebaasi ressursi kasutamine** tera loendis andmebaasi leiate kindla andmebaase, otsides lehtede loendi või andmebaasi nime tippimist. Andmebaasi valimiseks kasutage ruut.

![Andmebaaside jälgida otsimine][7]


## <a name="add-an-alert-to-a-pool-resource"></a>Teatise rakenduskausta ressursi lisamine

Saate lisada ressursse, mis saadavad meili inimesed või URL-i lõpp-punktid teatiste stringide kui ressursi tabab kasutamine lävi, mille seadistasite reeglid.

> [AZURE.IMPORTANT]Ressursi kasutamine elastne kaustu jälgimine on viivitusega vähemalt 20 minutit. Vähem kui 30 minuti teatiste seadmine elastne kaustu praegu ei toetata. Mis tahes teatiste seadmine elastne kaustu punktiga (parameeter nimega "-WindowSize" PowerShelli API) vähem kui 30 minuti võib olla käivitatud. Veenduge, et määratlete elastne kaustu teatisi kasutada või enama 30 minuti jooksul (WindowSize).

**Mis tahes ressursi teatise lisamiseks järgmist.**

1. Klõpsake diagrammi **Ressursi kasutamine** **meetermõõdustik** tera avamiseks klõpsake nuppu **Lisa teatise**, ja seejärel täitke teabe **lisamine reegli** tera (**Ressursi** on automaatselt häälestatud olema töötate pool).
2. Tippige **nimi** ja **Kirjeldus** , mille abil tuvastatakse teie ja adressaadi teatise.
3. Valige **meetermõõdustik** Teavita loendist soovitud.

    Diagramm kuvab dünaamiliselt ressursi kasutamine aitab teil valida piirmäära selle mõõdiku jaoks.

4. Valige **tingimus** (suurem kui väiksem kui jne) ja **lävi**.
5. Klõpsake nuppu **OK**.



## <a name="move-a-database-into-an-elastic-pool"></a>Andmebaasi viimine kohapeal on elastne

Saate lisada või eemaldada andmebaaside mõne olemasoleva kausta. Andmebaasid võivad olla muud kaustu. Siiski saate lisada ainult andmebaasidele, mis on sama loogiline server.

1. Klõpsake kausta labale **elastne** andmebaase **konfigureerimine pool**.

    ![Klõpsake nuppu Konfigureeri pool][1]

2. **Konfigureerimine rakenduskausta** tera, klõpsake käsku **Lisa kausta**.

    ![Klõpsake käsku Lisa kausta](./media/sql-database-elastic-pool-manage-portal/add-to-pool.png)


3. Valige **Lisa andmebaaside** tera, andmebaasi või kausta lisamiseks andmebaasid. Klõpsake nuppu **Vali**.

    ![Valige andmebaaside lisamine](./media/sql-database-elastic-pool-manage-portal/add-databases-pool.png)

    **Konfigureerimine pool** tera nüüd on loetletud teie valitud lisanud, et **Ootel**oma olekuks andmebaasi.

    ![Ootel rakenduskausta täiendused](./media/sql-database-elastic-pool-manage-portal/pending-additions.png)

3. **Konfigureerimine rakenduskausta blade**, klõpsake nuppu **Salvesta**.

    ![Klõpsake nuppu Salvesta](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="move-a-database-out-of-an-elastic-pool"></a>Andmebaasi kolima kohapeal on elastne

1. Valige **Konfigureeri pool** tera, andmebaasi või andmebaaside eemaldamiseks.

    ![andmebaaside loetelu](./media/sql-database-elastic-pool-manage-portal/select-pools-removal.png)

2. Klõpsake käsku **Eemalda kaustast**.

    ![andmebaaside loetelu](./media/sql-database-elastic-pool-manage-portal/click-remove.png)

    **Konfigureerimine pool** tera loetletud kohe valitud eemaldatakse olekut **Ootel**määratud andmebaasi.

    ![eelvaate andmebaasi lisamine ja eemaldamine](./media/sql-database-elastic-pool-manage-portal/pending-removal.png)

3. **Konfigureerimine rakenduskausta blade**, klõpsake nuppu **Salvesta**.

    ![Klõpsake nuppu Salvesta](./media/sql-database-elastic-pool-manage-portal/click-save.png)

## <a name="change-performance-settings-of-a-pool"></a>Jõudluse, on sätete muutmine

Kui teil jälgida ressursi kasutamise kohta on, võite avastada, et mõned muudatused on vaja. Võib-olla pool tuleb muudatuste jõudluse või salvestusruumi piirangud. Võimalik, et soovite andmebaasi kogumi sätete muutmine. Saate muuta igal ajal saada ülejäänud head jõudlust ja maksumus häälestamise pool. Vt [Millal kohapeal on elastne andmebaasi kasutada?](sql-database-elastic-pool-guidance.md) lisateabe saamiseks.

**EDTUs või mäluruumi piirangute kohta rakenduskausta ja eDTUs andmebaasi kohta muutmiseks tehke järgmist.**

1. Avage **konfigureerimine pool** tera.

    Jaotises **elastne andmebaasi rakenduskausta sätted**liuguriga mõlemal pool sätete muutmine.

    ![Elastne ressursi kasutamine](./media/sql-database-elastic-pool-manage-portal/resize-pool.png)

2. Kui muudab seda sätet, kuvatakse kuu prognoositud kulud muutmine.

    ![Rakenduskausta ja uue kuutasuga värskendamine](./media/sql-database-elastic-pool-manage-portal/pool-change-edtu.png)


## <a name="create-and-manage-elastic-jobs"></a>Luua ja hallata elastne

Elastne töö teile skripte Transact-SQL-i andmebaaside kogumi mõni muu arv peale. Saate luua uut töökohta või hallata olemasoleva portaalis.

![Luua ja hallata elastne][5]


Enne, kui kasutate töö, elastne töö komponentide installimine ja sisestage oma kasutajanimi ja parool. Lisateavet leiate teemast [elastne andmebaasi töö ülevaade](sql-database-elastic-jobs-overview.md).

Vt [mastaapimine välja koos Azure'i SQL-andmebaasi](sql-database-elastic-scale-introduction.md): elastne Andmebaasiriistad abil skaala-out, andmete teisaldamine päringu või luua tehingud.



## <a name="additional-resources"></a>Lisaressursid

- [SQL-andmebaasi elastne pool](sql-database-elastic-pool.md)
- [Portaalis kohapeal on elastne andmebaasi loomine](sql-database-elastic-pool-create-csharp.md)
- [PowerShelli abil kohapeal on elastne andmebaasi loomine](sql-database-elastic-pool-create-powershell.md)
- [C# kohapeal on elastne andmebaasi loomine](sql-database-elastic-pool-create-csharp.md)
- [Hind ja jõudluse kaalutluste kohta elastne andmebaasi kaustu](sql-database-elastic-pool-guidance.md)


<!--Image references-->
[1]: ./media/sql-database-elastic-pool-manage-portal/configure-pool.png
[2]: ./media/sql-database-elastic-pool-manage-portal/basic.png
[3]: ./media/sql-database-elastic-pool-manage-portal/basic-2.png
[4]: ./media/sql-database-elastic-pool-manage-portal/basic-3.png
[5]: ./media/sql-database-elastic-pool-manage-portal/elastic-jobs.png
[6]: ./media/sql-database-elastic-pool-manage-portal/edit-metric.png
[7]: ./media/sql-database-elastic-pool-manage-portal/select-dbs.png
[8]: ./media/sql-database-elastic-pool-manage-portal/db-utilization.png
[9]: ./media/sql-database-elastic-pool-manage-portal/metric.png
