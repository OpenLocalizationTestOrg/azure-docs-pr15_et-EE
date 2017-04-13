<properties
    pageTitle="Kuidas jälgida salvestusruumi konto | Microsoft Azure'i"
    description="Saate teada, kuidas jälgida salvestusruumi konto Azure Azure portaali kaudu."
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
    ms.topic="article"
    ms.date="08/03/2016"
    ms.author="robinsh"/>

# <a name="monitor-a-storage-account-in-the-azure-portal"></a>Salvestusruumi konto Azure'i portaalis jälgimine

## <a name="overview"></a>Ülevaade

Saate jälgida [Azure portaali](https://portal.azure.com)konto salvestusruumi. Kui konfigureerite konto salvestusruumi portaali kaudu jälgida, kasutab Azure Storage [Salvestusruumi Analytics](http://msdn.microsoft.com/library/azure/hh343270.aspx) jälgida mõõdikute konto ja logige taotluse andmed.

> [AZURE.NOTE]Lisatasud seostatud läbi vaadanud [Azure portaali](https://portal.azure.com)jälgimisega seotud andmed. Lisateabe saamiseks vt <a href="http://msdn.microsoft.com/library/azure/hh360997.aspx">salvestusruumi Analytics ja arveldamine</a>. <br />

> Azure'i failide talletamine praegu toetab salvestusruumi Analytics mõõdikute, kuid veel ei toeta logimine. Saate lubada mõõdikute Azure'i faili Storage [Azure portaali](https://portal.azure.com)kaudu.

> Salvestusruumi kontod dispersioonanalüüs tüüpi Zone liigsete mälu (ZRS) pole mõõdikute või logimise võimalus praegu lubatud. 

> Põhjalikud juhised salvestusruumi analüüsi- ja muude tööriistade abil tuvastada, diagnoosimine ja Azure Storage seotud probleemide tõrkeotsing, vt [kuvaris, diagnoosimine, ja Microsoft Azure'i Tabelimäluga tõrkeotsing](storage-monitoring-diagnosing-troubleshooting.md).


## <a name="how-to-configure-monitoring-for-a-storage-account"></a>Kuidas: jälgimise salvestusruumi konto konfigureerimine

1. [Azure portaali](https://portal.azure.com)nuppe **salvestusruum**, ja klõpsake armatuurlaual avamiseks salvestusruumi konto nimi.

2. Klõpsake nuppu **Konfigureeri**ja kerige **jälgimise** bloobimälu, tabeli ja järjekorda teenuste sätteid.

    ![MonitoringOptions](./media/storage-monitor-storage-account/Storage_MonitoringOptions.png)

3. **Jälgimine**, määrata tasemel jälgimise ja andmete säilituspoliitika iga teenuse jaoks.

    -  Jälgimisega seotud taseme määramiseks valige üks järgmistest:

      **Minimaalne** - kogub mõõdikute näiteks sissepääsu/sealt, kättesaadavus, latentsus ja edu protsendid, mis on koondatud bloobimälu, tabeli ja järjekorda teenused.

      **Verbose** – Lisaks minimaalsete mõõdikud kogub samu mõõdikute iga salvestusruumi toimingu Azure'i salvestusruumi teenuse API. Paljusõnaline mõõdikute lubada täpsema analüüsi probleemid, mis ilmnevad teenuserakenduse toimingud.

      **Välja** – lülitab jälgimine. Olemasolevad andmed püsis säilitusperiood lõppu.

- Andmete säilituspoliitika, **säilitamise (päevades)**määramiseks tippige andmed 1 365 päeva säilitamise päevade arv. Kui te ei soovi säilituspoliitika määramiseks, sisestage null. Kui ei ole säilituspoliitika, on teie andmed kustutada. Soovitame säilituspoliitika põhjal kaua soovite säilitada nii, et kasutamata ja vana analytics andmete kustutamist süsteemi tasuta salvestusruumi analytics andmete teie konto jaoks.

4. Kui olete lõpetanud jälgimisega seotud konfiguratsiooni, klõpsake nuppu **Salvesta**.

Peab algama, kuvatakse andmete armatuurlaud ja umbes üks tund pärast lehe **jälgimine** .

Kuni jälgimise salvestusruumi konto konfigureerimist jälgimisega seotud andmed kogutakse ja mõõdikute diagrammide armatuurlaud ja **kuvari** lehel on tühi.

Kui määrate jälgimise tasemed ja säilituspoliitikad, saate valida, millist saadaval mõõdikute jälgimiseks [Azure portaali](https://portal.azure.com)ja millised mõõdikute mõõdikute diagrammide diagrammile. Vaikimisi määratud mõõdikute kuvatakse iga jälgimisega seotud tasemel. Saate **Lisada mõõdikute** lisamiseks või eemaldamiseks loendist mõõdikute mõõdikute.

Salvestusruumi konto nimega $MetricsTransactionsBlob, $MetricsTransactionsTable, $MetricsTransactionsQueue ja $MetricsCapacityBlob nelja tabelites on talletatud mõõdikute. Lisateabe saamiseks vt [Talletusruumi Analytics mõõdikute](http://msdn.microsoft.com/library/azure/hh343258.aspx).


## <a name="how-to-customize-the-dashboard-for-monitoring"></a>Kuidas: armatuurlaua jälgimiseks kohandamine

Armatuurlaual saate valida kuni kuus mõõdikute diagrammile mõõdikute diagrammi saadaval üheksas mõõdikute põhjal. Iga teenuse (bloobimälu, tabeli ja järjekorda), kättesaadavus, edu protsenti ja kokku taotlusi mõõdikud on saadaval. Saadaval armatuurlaual mõõdikute on samad minimaalsete või Paljusõnaline jälgimiseks.

1. [Azure portaali](https://portal.azure.com)nuppe **salvestusruum**, ja klõpsake armatuurlaual avamiseks salvestusruumi konto nimi.

2. Mõõdikute, mis esitatakse diagrammi muutmiseks tehke ühte järgmistest toimingutest.

    - Klõpsake uue mõõdiku lisamiseks diagrammi värviline argumendil päise all diagrammi tabeli kõrval olev ruut.

    - Mõõdiku, mis on kantakse diagrammil peitmiseks tühjendage värviline argumendil päise kõrval olev ruut.

        ![Monitoring_nmore](./media/storage-monitor-storage-account/storage_Monitoring_nmore.png)

3. Vaikimisi kuvatakse diagrammi trende, kus on kuvatud ainult praegune väärtus iga mõõdiku ( **suhteline** valik diagrammi kohal). Y-telg absoluutväärtuse näeksite kuvamiseks valige **absoluutne**.

4. Ajavahemiku muutmine mõõdikute diagramm kuvatakse, valige 6 tunni jooksul 24 tundi 7 päeva diagrammi kohal.


## <a name="how-to-customize-the-monitor-page"></a>Kuidas: kohandamine lehe jälgimine

Klõpsake lehe **jälgimine** saate vaadata mõõdikute täiskomplekti salvestusruumi konto jaoks.

- Kui salvestusruumi konto on konfigureeritud minimaalsete jälgimine, nt sissepääsu/sealt, kättesaadavus, latentsus ja edu protsentide mõõdikute on koondatud bloobimälu, tabelist ja järjekorda teenused.

- Kui teie salvestusruumi konto on konfigureeritud Paljusõnaline jälgimine, mõõdikud on saadaval peenem eraldusvõimega üksikute salvestusruumi toimingute Lisaks teenusetaseme agregaadid.

Järgmistest toimingutest abil saate valida, millised talletusruumi mõõdikute mõõdikute diagrammide ja tabeli kuvamine **kuvari** lehel kuvatavate. Nende sätete ei mõjuta saidikogumi, koondamine ja säilitamine jälgida salvestusruumi konto andmeid.

## <a name="how-to-add-metrics-to-the-metrics-table"></a>Kuidas: mõõdikute mõõdikute tabeli lisamine


1. [Azure portaali](https://portal.azure.com)nuppe **salvestusruum**, ja klõpsake armatuurlaual avamiseks salvestusruumi konto nimi.

2. Klõpsake **jälgimine**.

    Avaneb leht **jälgimine** . Vaikimisi mõõdikute tabel kuvab saadaolevad jälgimise mõõdikute alamhulga. Joonisel kuvari vaikekuva salvestusruumi konto Paljusõnaline jälgida kõiki kolme teenuse jaoks konfigureeritud. Valige kõik saadaolevad mõõdikute põhjal jälgitava mõõdikud **Mõõdikute lisamine** abil.

    ![Monitoring_VerboseDisplay](./media/storage-monitor-storage-account/Storage_Monitoring_VerboseDisplay.png)

    > [AZURE.NOTE] Kui valite mõõdikud, kaaluge kulud. On tehingu ja sealt kulude jälgimise kuvab värskendamine. Lisateabe saamiseks vt [salvestusruumi Analytics ja arveldamine](http://msdn.microsoft.com/library/azure/hh360997.aspx).

3. Klõpsake **Lisa mõõdikute**.

    Aggregate mõõdikute, mis on saadaval minimaalsete jälgimine on loendi ülaosas. Kui ruut on märgitud, kuvatakse loendis mõõdikute mõõdiku.

    ![AddMetricsInitialDisplay](./media/storage-monitor-storage-account/Storage_AddMetrics_InitialDisplay.png)

4. Hoidke kursorit paremas servas dialoogiboksi kuvamiseks kerimisriba, mida saate lohistada täiendavaid mõõdikute vaatesse kerima.

    ![AddMetricsScrollbar](./media/storage-monitor-storage-account/Storage_AddMetrics_Scrollbar.png)


5. Mõõdiku mõõdiku on rakendatud kaasata toimingute loendi laiendamiseks klõpsake allanoolt. Märkige iga toimingu, mida soovite vaadata mõõdikute tabelis [Azure portaali](https://portal.azure.com).

    Järgmisel joonisel on laiendatud autoriseerimine tõrge protsendi mõõt.

    ![ExpandCollapse](./media/storage-monitor-storage-account/Storage_AddMetrics_ExpandCollapse.png)


6. Pärast kõigi teenuste mõõdikute valimiseks klõpsake nuppu OK (märge) jälgimisega seotud konfiguratsiooni värskendamiseks. Valitud mõõdikute lisatakse tabelisse mõõdikute.

7. Mõõdiku tabeli kustutamiseks klõpsake seda valimiseks meetermõõdustik ja klõpsake **Meetermõõdustik kustutada**.

    ![DeleteMetric](./media/storage-monitor-storage-account/Storage_DeleteMetric.png)

## <a name="how-to-customize-the-metrics-chart-on-the-monitor-page"></a>Kuidas: lehe jälgimine mõõdikute diagrammi kohandamine

1. Valige lehe **jälgimine** salvestusruumi konto, mõõdikute tabelis, kuni 6 mõõdikute mõõdikute diagrammile kantav väärtus. Mõõdiku valimiseks märkige ruut selle vasakul küljel. Diagrammi mõõdiku eemaldamiseks tühjendage ruut.

2. Diagrammi suhteline väärtused (kuvatakse ainult lõppväärtus) ja absoluutne väärtused (Y-telg kuvatud) vahel liikumiseks valige **suhteline** või **absoluutne** diagrammi kohal.

3.  Muuta aeg vahemiku mõõdikute diagramm kuvatakse, valige **6 tunni**, **24 tundi**või **7 päeva** diagrammi kohal.



## <a name="how-to-configure-logging"></a>Kuidas: logisse kandmise konfigureerimine

Iga salvestusruumi pakutavaid kontol salvestusruumi (bloobimälu, tabeli ja järjekorda), saate lugemine taotlusi, taotlusi kirjutamine ja/või kustutada taotlusi diagnostika Logide salvestamine ja määrata andmete säilituspoliitika iga teenuse jaoks.

1. [Azure portaali](https://portal.azure.com)nuppe **salvestusruum**, ja klõpsake armatuurlaual avamiseks salvestusruumi konto nimi.

2. Klõpsake nuppu **Konfigureeri**ja kasutada funktsiooni allanool klaviatuuril Kerige allapoole suvandini **logimine**.

    ![Storagelogging](./media/storage-monitor-storage-account/Storage_LoggingOptions.png)


3. Iga teenuse (bloobimälu, tabeli ja järjekorda), konfigureerida järgmist.

    - Logige taotluse tüüpi: lugemine taotlusi, kirjutage määramised ja kustutada taotlused.

    - Logitud andmete säilitamise päevade arv. Sisestage nullist on, kui te ei soovi säilituspoliitika määramiseks. Kui seate säilituspoliitika, on kuni logid kustutada.

4. Klõpsake nuppu **Salvesta**.

Diagnostika logid salvestatakse bloobimälu ümbris nimega $logs teie salvestusruumi konto. $logs container pääsemise kohta leiate teavet teemast [Kohta salvestusruumi Analytics logimine](http://msdn.microsoft.com/library/azure/hh343262.aspx).
