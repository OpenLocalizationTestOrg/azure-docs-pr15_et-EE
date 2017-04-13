<properties
    pageTitle="Analüüsi Log Analytics andmete kasutamine | Microsoft Azure'i"
    description="Log Analytics kasutuse lehel saate vaadata, kui palju andmeid saadetakse OMS teenuse."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="analyze-data-usage-in-log-analytics"></a>Analüüsi Log Analytics andmete kasutamine

Kasutusanalüüsi Logi sisse toimingud halduse komplekti (OMS) abil kogutud andmete ja saadab OMS teenusega perioodiliselt.  Saate vaadata, kui palju andmeid saadetakse OMS teenuse **kasutuse** lehel. **Kasutuse** lehel ka kuvatakse saadetakse lahenduste iga päev palju andmeid ja kui sageli oma serverid saadavad andmed.

>[AZURE.NOTE] Kui teil on tasuta konto loodud [OMS veebisaidi](http://www.microsoft.com/oms)kaudu, kui olete piiratud 500 MB andmeid saata OMS teenuse iga päev. Kui teil iga päev, Andmeanalüüs lõpetada ja jätkata järgmise päeva alguses. Samuti peate uuesti andmeid, mida ei olnud aktsepteeritud või töödelda OMS.

Paani **kasutamine** rakenduses OMS armatuurlaual **Ülevaade** abil saate vaadata oma kasutamine.

![kasutus paan](./media/log-analytics-usage/usage-tile.png)

Kui olete ületanud oma kasutus piirmäär või kui teil on oma limiit lähedal, saate soovi korral vähendada andmehulga OMS teenusega saadetavate lahenduse eemaldada. Lahenduste eemaldamise kohta leiate lisateavet teemast [lisamine Log Analytics lahenduste lahendusegaleriist](log-analytics-add-solutions.md).

![armatuurlaua kasutus](./media/log-analytics-usage/usage-dashboard.png)

**Kasutuse** lehel kuvatakse järgmine teave:

- Keskmine kasutus päeva kohta
- Andmekasutuse iga lahenduse viimase 30 päeva jooksul
- Kui palju andmeid teie keskkonnas serverid saadavad OMS teenuse viimase 30 päeva jooksul
- Teie andmete leping hinnad taseme- ja prognoositud kulud
- Teavet oma teenuse taseme leping (SLA), sealhulgas, kui kaua võtab OMS andmete töötlemiseks

## <a name="to-work-with-usage-data"></a>Kasutus andmetega töötamiseks

1. Klõpsake lehe **Ülevaade** **kasutus** paani.
2. Lehel **kasutus** vaadata kasutus kategooriad, mis näitavad alade olete muret.
3. Kui teil on lahenduse, mis on tarbimine liiga palju iga päev üles meiliteavitus, võiksite seda lahendust eemaldamine.

## <a name="to-view-your-estimated-cost-and-billing-information"></a>Kui soovite prognoositud kulud ja arveldusteabe kuvamine
1. Klõpsake lehe **Ülevaade** **kasutus** paani.
2. Klõpsake jaotises **kasutus**lehel **kasutus** noolenuppu (**>**) kõrval **hinnanguline maksumus**.
3. Klõpsake soovitud laiendatud **teie andmete lepingu** üksikasjad, saate vaadata oma hinnanguline, kuu maksumus.  
    ![Teie andmete leping](./media/log-analytics-usage/usage-data-plan.png)
4. Kui soovite vaadata arveldusteabe, valige **arve kuvamine** tellimuse teabe kuvamiseks.
    - Klõpsake lehel tellimused oma tellimuse üksikasjad ja kasutus-reaüksuse loendi kuvamiseks.  
        ![tellimuse](./media/log-analytics-usage/usage-sub01.png)
    - Kokkuvõte lehel tellimuse eest saate teha mitmesuguseid tööülesandeid hallata ning oma tellimuse üksikasjade kuvamiseks.  
        ![Tellimuse üksikasjad](./media/log-analytics-usage/usage-sub02.png)

## <a name="to-view-data-batches-for-your-sla"></a>Andmete pakettidena jaoks teie SLA kuvamiseks
1. Klõpsake lehe **Ülevaade** **kasutus** paani.
2. Klõpsake jaotises **Teenuselepingu** **allalaadimine SLA üksikasjad**.
3. Kuidas laaditakse Exceli XLSX-failina.  
    ![SLA üksikasjad](./media/log-analytics-usage/usage-sla-details.png)

## <a name="next-steps"></a>Järgmised sammud

- Vt [Log otsingud Log Analytics](log-analytics-log-searches.md) üksikasjaliku teabe kogutud lahendusi.
