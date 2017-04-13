<properties
   pageTitle="Saada ülevaadet Azure'i turbekeskus andmetest Power BI | Microsoft Azure'i"
   description="Azure'i turvalisus Center Power BI sisu pakett on lihtne leida Turbeteatiste, soovitused, ründas ressursid ja trende, vastavalt teie aruannete jaoks loodud Andmekomplekt."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>

# <a name="get-insights-from-azure-security-center-data-with-power-bi"></a>Saada ülevaadet Power BI Azure'i turbekeskus andmete põhjal
[Power BI armatuurlaua](http://aka.ms/azure-security-center-power-bi) Azure'i turbekeskus võimaldab teil visualiseerida, analüüsida ja filtreerida soovitused ja turbeteatised igalt poolt, sh mobiilsideseadme kaudu. Power BI armatuurlaua abil esile trendide ja rünnata mustrite - vaade Turbeteatiste ressursi või allika IP-aadress ja adressaadita ohtu turvalisusele ressursi või vanus. 

Te saate ka koondada turbekeskus soovitused ja turbeteatised muude andmetega huvitav viisil, näiteks [Azure'i auditilogid](https://powerbi.microsoft.com/blog/monitor-azure-audit-logs-with-power-bi/) ja [Azure SQL-i andmebaasi auditeerimine](https://powerbi.microsoft.com/blog/monitor-your-azure-sql-database-auditing-activity-with-power-bi/)andmete kasutamine. Mõlemad pakuvad Power BI armatuurlauad ja võite eksportida andmed Excelisse lihtne teatamise oma cloud security seisundi kohta.

##<a name="using-azure-security-center-dashboard-to-access-power-bi"></a>Power BI armatuurlaua Azure'i turbekeskus kaudu
Azure'i turbekeskus armatuurlaua abil juurdepääs Power BI aruanded. Järgige selle toimingu sooritamiseks: 

1. **Azure'i turbekeskus** armatuurlaual **Uuri Power BI** nuppu.

    ![Ühenduse loomine Azure turbekeskus Power BI abil](./media/security-center-powerbi/security-center-powerbi-fig1-new10.png) 

2. **Power BI Uuri** tera avaneb paremal pool, nagu on näidatud järgmisel kuvatõmmisel:

    ![Ühenduse loomine Azure turbekeskus Power BI abil](./media/security-center-powerbi/security-center-powerbi-fig1-new2.png)

3. Kui loote Power BI armatuurlaua esimest korda, saate valida ühe järgmistest suvanditest **Uuri Power BI** tera: 

    - **Turvalisus ülevaateid armatuurlaua**: valige see suvand, kui soovite luua armatuurlaud, mis sisaldab turvalisuse olek, teemad ja avastused. See suvand on sagedamini DevOps rolli, mis vastutab oma kaitse seisundi analüüsimiseks ja tuvastatud teatiste tellimustes.
    - **Poliitika halduse armatuurlaua**: valige see suvand, kui soovite uurida haldus-ja.  See suvand on rohkem levinud keskse seda, kes on suunatud rohkem juhtimine. Armatuurlaua abil saate need saada nähtavuse ja teadmisi, Turve poliitika järgimine asutuses.
    - Kui teil juba on Power BI armatuurlaua, klõpsake **praeguse Power BI armatuurlaua minna**.

4. Selles näites **Turvalisus ülevaateid armatuurlaua** suvandit. Kui see on esimene kord, loote Power BI armatuurlaua turbekeskus teil palutakse sisu on installitud. Klõpsake **Power BI sisu tuvastamine** aknas nuppu **saada** , nagu on näidatud järgmisel kuvatõmmisel.

    ![Azure'i turvalisus Center turvalisus ülevaateid armatuurlaud](./media/security-center-powerbi/security-center-powerbi-fig1-new3.png)

5. **Azure'i turvalisus Center turvalisus ülevaateid ühenduse** aken. Veenduge, et **autentimise** meetodit on **oAuth2** , nagu allpool näidatud ja klõpsake nuppu **Logi sisse** .
    
    ![Autentimine](./media/security-center-powerbi/security-center-powerbi-fig1-new4.png)

6. Teil võidakse paluda kinnitamiseks uuesti Azure mandaat. Pärast autentimist armatuurlauale luuakse. Kui armatuurlaual on loodud kuvatakse aruande sarnase struktuuriga, nagu on näidatud järgmisel kuvatõmmisel.

    ![Power BI armatuurlaua](./media/security-center-powerbi/security-center-powerbi-fig1-new5.png)


> [AZURE.NOTE] Toimub iga päev on ajastatud värskendamine aruande. Juhuks, kui teil tekib tõrge selle värskendamise, lugege lisateavet tõrkeotsingu kohta [Võimalikud värskendamine probleemid Azure'i turvalisus Center Power BI](https://blogs.msdn.microsoft.com/azuresecurity/2016/04/07/azure-security-center-power-bi-refresh-fails/).

Siin näete Turbeteatiste ja soovitused arv, samuti VMs, SQL Azure'i andmebaasid ja võrgu ressursse Azure'i turbekeskus kontrollitakse arv.

Azure'i turbekeskuse link suunab teid Azure portaali. Diagrammide oleks hõlpsam visualiseerimine turvalisus soovitused ja teatised, sh teave:

- Ressursside turvalisus olek
- Ootel soovitused
- VM soovitused
- Teatiste aja jooksul
- Ründasid ressursid
- Ründasid IP-d

Igale diagrammile taga on täiendavad ülevaateid. Valige paan lisateabe saamiseks. Näiteks Paani **Ressursside turvalisus olek** kuvatakse täiendavad üksikasjad kohta ootel soovitused ressursid, nagu on näidatud järgmisel kuvatõmmisel.

![Soovitused](./media/security-center-powerbi/security-center-powerbi-fig1-new6.png)

Kui klõpsate selle graafik igal real, teised lähevad Hall välja ja saate keskenduda ainult valitud. Armatuurlauale naasmiseks klõpsake selle lehe vasakpoolsel paanil valikut **armatuurlauad** **Azure'i turbekeskus** .

> [AZURE.NOTE] Kui soovite kohandada oma aruandeid lisada täiendavaid välju või muuta olemasolevaid visuaale, saate redigeerida aruanne. Lugege lisateavet [Interact aruande redigeerimisvaates Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-interact-with-a-report-in-editing-view/) .

Paanid **Ründaja IP -d** ja **teatiste aja jooksul ründas ressursid** on sarnased väljundi igat selle klõpsamisel. See juhtub, kuna aruande kõigi kolme muutujaga puudutav teave liitmise ja helistab **ressursid rünnaku all** , nagu on näidatud järgmisel kuvatõmmisel.

![Ressursside rünnaku all](./media/security-center-powerbi/security-center-powerbi-fig1-new7.png)

Selles etapis saate ka aruande koopia salvestamine, printida või avaldamine veebis saadaval menüü **fail** suvandite abil.

![Menüü fail](./media/security-center-powerbi/security-center-powerbi-fig8.png)

## <a name="exploring-your-azure-security-center-data-with-power-bi-services"></a>Power BI teenuseid Azure'i turbekeskus andmete uurimine

Ühenduse loomine Power BI [Power BI sisu Pack teenused](https://msit.powerbi.com/groups/me/getdata/services) ja käivitada järgmist:

1. **Power BI sisu pakett** aknas kuvatakse kaks võimalust, nagu allpool näidatud.

    ![Power BI sisu pakett](./media/security-center-powerbi/security-center-powerbi-fig1-new.png)

    >[AZURE.NOTE] Kui juba täidetud käesoleva artikli esimene osa kuvatakse ainult üks võimalus, mis on Azure turvalisus Center poliitika haldus.

2. Selleks, et selle näite puhul nuppu **saada** paani **Azure'i turvalisus Center poliitika haldus** .

3. **Ühenduse Azure turvalisus Center poliitika Management** aknas veenduge, et valida **oAuth2** jaotises **Autentimise meetodit** rippmenüü nagu on näidatud allpool ja klõpsake nuppu **Logi sisse** .

    ![Poliitika halduse aken](./media/security-center-powerbi/security-center-powerbi-fig1-new8.png)

4. Teid suunatakse ümber autentimise lehele, kus tuleks sisestada identimisteavet, mida kasutate ühenduse Azure'i turbekeskus. Pärast autentimist on lõpule jõudnud, hakkavad Power BI importida andmed teie aruannete koostamine. Selle aja jooksul võib brauseri paremas allnurgas kuvada järgmine teade:

    ![Ühenduse loomine Azure turbekeskus Power BI abil](./media/security-center-powerbi/security-center-powerbi-fig4.png)

    >[AZURE.NOTE] Kui armatuurlaual on esmakordsel loomisel võib kuluda rohkem kui tavaliselt, peamiselt stsenaariumid, kus teil on mitu tellimust. 

5. Kui protsess on lõpule jõudnud, laaditakse Azure'i turvalisus Center Power BI armatuurlaua **Rühmapoliitika haldus** aruande, mis sarnaneb allpool näidatud:

    ![Poliitika halduse armatuurlaua](./media/security-center-powerbi/security-center-powerbi-fig1-new9.png)

## <a name="see-also"></a>Vt ka
Selles dokumendis soovite õpitut Azure'i turbekeskus Power BI kasutamine. Azure'i turbekeskus kohta leiate lisateavet teemast järgmist:

- [Azure'i turvalisus Center plaanimine ja kasutusjuhend](security-center-planning-and-operations-guide.md) – saate teada, kuidas Azure'i turbekeskus kasutuselevõtu kavandamine.
- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – saate teada, kuidas Azure'i turbekeskus sätete konfigureerimine
- [Haldamine ja turbeteatised Azure'i turbekeskus vastuvõtmist](security-center-managing-and-responding-alerts.md) – saate teada, kuidas hallata ja turbeteatised vastamine
- [Azure'i turvalisus Center KKK](security-center-faq.md) – korduma kippuvad küsimused teenuse kasutamise kohta otsimine
- [Azure'i turvalisus ajaveeb](http://blogs.msdn.com/b/azuresecurity/) – Leia ajaveebi Azure Turve ja nõuetele vastavuse kohta
