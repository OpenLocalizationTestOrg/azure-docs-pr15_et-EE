<properties
    pageTitle="Power BI armatuurlaua voo Analytics | Microsoft Azure'i"
    description="Ärianalüüs kogumine ja analüüsimine töö voo Analytics andmete reaalajas streaming Power BI armatuurlaua abil."
    keywords="Kasutusanalüüsi armatuurlaua reaalajas armatuurlaud"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"/>

#  <a name="stream-analytics--power-bi-a-real-time-analytics-dashboard-for-streaming-data"></a>Analüüsimine ja Power BI voona: streaming andmete reaalajas analytics armatuurlaua

Azure'i voo Analytics saate kasutada ühte ees ärianalüüsitööriistade, Microsoft Power BI. Saate teada, kuidas kasutada Azure voo Analytics suure mahu, videote voogesitamise ja saada ülevaate reaalajas Power BI armatuurlaua analytics analüüsida.

[Microsoft Power BI](https://powerbi.com/) abil saate koostada reaalajas armatuurlaua kiiresti. [Vaadake videot, mis illustreerivad seda stsenaariumi](https://www.youtube.com/watch?v=SGUpT-a99MA).

Sellest artiklist saate teada, kuidas luua oma kohandatud ärianalüüsitööriistade, kasutades Power BI väljund oma Azure'i voo Analytics töökohta ja kasutada reaalajas armatuurlaud.

## <a name="prerequisites"></a>Eeltingimused

* Microsoft Azure'i konto
* Sisend voo Analytics töö voogesituse andmete kasutamine. Voo Analytics aktsepteerib sisend Azure'i sündmuse jaoturi või Azure'i bloobimälu salvestusruumi.  
* Power BI töökoha või kooli kontoga

## <a name="create-azure-stream-analytics-job"></a>Azure'i voo Analytics töö loomine

[Azure'i klassikaline portaali](https://manage.windowsazure.com)nuppu **Uus, Data Services, voo Analytics, kiiresti luua**.

Täpsustage järgmised väärtused, klõpsake **loomine voo Analytics töö**:

* **Töö nimi** - sisestage töö nimi. Näiteks **DeviceTemperatures**.
* **Piirkonna** - valige piirkond, kuhu soovite asub töö. Kaaluma töö ja sündmuse jaoturi piirkonna optimaalse jõudluse tagamiseks ja proovima andmete edastamine kulud piirkondade vahel.
* **Salvestusruumi konto** - salvestusruumi konto, mida soovite kasutada kõigi voo Analytics töid selle piirkonna jaoks jälgimisega seotud andmete talletamiseks. Teil on võimalik valida olemasoleva salvestusruumi konto või looge uus.

Klõpsake vasakpoolsel paanil voo Analytics tööde nimekirja **Voo Analytics** .

![graphic1][graphic1]

> [AZURE.TIP] Uue töö on loetletud **Pole alustatud**olekuga. Pange tähele, et lehe allservas nuppu **Start** on keelatud. See on oodatud käitumine tuleb konfigureerida töö sisendi, väljundi, päringu, jne enne kui saate alustada tööd.

## <a name="specify-job-input"></a>Määrake töö sisendit

Selles õpetuses mõeldud oleme eeldades, et kasutate sündmuse jaoturi sisendina ja JSON sariväljaanne UTF-8 kodeeringus.

* Klõpsake töö nime.
* Klõpsake lehe ülaosas **sisendina** ja klõpsake **Sisestusmeetodi lisamine**. Avaneb dialoogiboks annab teile kaudu seadistamine sisendit arv.
*   Valige **Andmevoos**ja seejärel klõpsake nuppu paremale.
*   Valige **Sündmuse keskus**ja seejärel nuppu paremale.
*   Tippige või valige kolmanda lehel järgmised väärtused:
  * **Sisestuskeel pseudonüüm** - Sisestage sõbralik nimi selle töö sisestusteade. Pange tähele, et kasutate selle nimi päringu hiljem.
  * **Sündmuse jaoturi** – kui olete loonud sündmuse jaoturi on sama tellimuse voo Analytics töö, valige sündmuse jaoturi nimeruumi.
*   Kui teie sündmuse jaoturi on mõni muu tellimus, valige **Kasuta sündmuse keskus teise tellimusest** ja **Teenuse siini Namespace**, **Sündmuse jaoturi nimi**, **Sündmuse jaoturi poliitika nimi**, **Sündmuse jaoturi poliitika võti**ja **Sündmuse jaoturi Partition Count**teabe käsitsi sisestamiseks.

> [AZURE.NOTE]  See näide kasutab vaikearvu sektsioonid, mis on 16.

* **Sündmuse jaoturi nimi** – valige Azure sündmuse jaoturi teil nime.
* **Sündmuse jaoturi poliitika nimi** – valige sündmuse jaoturi poliitika jaoks kasutate sündmuse jaoturi. Veenduge, et see poliitika õiguste haldamine.
*   **Sündmuse jaoturi tarbija rühma** , saate selle tühjaks jätta või teil on oma sündmuse jaoturi tarbija rühma määrata. Pange tähele, et iga tarbija rühma mõne sündmuse jaoturi saab ainult 5 lugejad korraga. Vastavalt otsustada, õige tarbija rühma oma töö. Kui jätate välja tühjaks, kasutatakse selle tarbija vaikerühma.

*   Klõpsake paremal.
*   Määrake järgmised väärtused:
  * **Sündmuse Serializer vorming** – JSON
  * **Kodeerimine** - UTF8
*   Klõpsake nuppu kontrolli selle andmeallika lisamiseks ja kinnitamiseks, et voo Analytics saate ühendada sündmuse jaoturi.

## <a name="add-power-bi-output"></a>Power BI väljundi lisamine

1.  Klõpsake lehe ülaosas **väljundi** ja klõpsake **Väljundi lisada**. Kuvatakse Power BI suvandina väljundi.

    ![graphic2][graphic2]  

2.  Valige **Power BI** ja seejärel nuppu paremale.
3.  Kuvatakse ekraani umbes selline:

    ![graphic3][graphic3]  

4.  Selles etapis tuleb pakkumise töökoha või kooli kontoga voo Analytics töö väljundi jaoks. Kui teil juba on Power BI konto, valige **Lubada kohe**. Kui ei, siis valige **Registreeruge kohe**. [Siin on hea ajaveebi jalutuskäigul Power BI kasutajaks üksikasjad](http://blogs.technet.com/b/powerbisupport/archive/2015/02/06/power-bi-sign-up-walkthrough.aspx).

    ![graphic11][graphic11]  

5.  Järgmiseks kuvatakse ekraani umbes selline:

    ![graphic4][graphic4]  

Sisestage väärtused, nagu allpool.

* **Väljundi alias (pseudonüüm)** – saate panna mis tahes väljundi pseudonüümi, mis on lihtne viidata. Selle väljundi pseudonüüm on eriti kasulik, kui te otsustate on mitu väljundit oma töö. Sel juhul peate selle väljundi päringu viidata. Näiteks kasutame väljundväärtuse alias (pseudonüüm) = "OutPbi".
* **Andmekomplekti nimi** - andmekomplekti nimi soovitud väljund on Power BI. Näiteks kasutame "pbidemo".
*   **Tabeli nimi** – oma Power BI väljundi andmekomplekti jaotises tabeli nimi. Oletame, et me kutsume seda "pbidemo". Praegu voo Analytics tööde haldamine Power BI väljund võib olla ainult ühest tabelist Andmekomplekt.
*   **Tööruumi** – valige oma Power BI rentniku, mille andmekomplekti luuakse tööruumi.

>   [AZURE.NOTE] Ärge konkreetselt looge selle andmekomplekti ja tabeli kontol Power BI. Need luuakse automaatselt kui hakkate oma voo Analytics töö ja töö alustab pumpamiseks väljundi Power BI. Kui teie töö päring ei tagasta üheski tulemuses, andmekomplekti ja tabeli ei looda.

*   Klõpsake nuppu **OK**, **Testi ühendust** ja nüüd väljastate configuraiton on lõpule viidud.

>   [AZURE.WARNING] Arvestage, et kui Power BI oli juba andmekomplekti ja tabeli ühe andsite selle voo Analytics töö sama nimega, olemasolevad andmed kirjutatakse.


## <a name="write-query"></a>Kirjutage päring

Minge oma tööd **päringu** vahekaarti. Kirjutage oma päringu väljund, mille soovite oma Power BI. Näiteks see võiks olla midagi näiteks järgmine SQL-päringu:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,1),
        dspl



Oma tööd alustada. Kinnitage, et teie sündmuse jaoturi võtab vastu sündmused ja päringu genereeritud oodatud tulemusi. Kui teie päringu tulemused 0 read, Power BI andmekomplekti ja tabeleid ei luuakse automaatselt.

## <a name="create-the-dashboard-in-power-bi"></a>Power BI Armatuurlaua loomine

Minge [powerbi.com lehe kaudu](https://powerbi.com) ja logige sisse oma töökoha või kooli kontoga. Kui voo Analytics töö päringu tulemused tulemused, kuvatakse teie andmekomplekti on juba loodud.

![graphic5][graphic5]

Armatuurlaua loomise, minge suvandi armatuurlaudade ja luuakse armatuurlaud.

![graphic6][graphic6]

Selles näites me Label see "Demo armatuurlaud".

Nüüd klõpsake oma voo Analytics töö (Käesolevas näites praeguse pbidemo) loodud andmekomplekti. Saate võetakse lehele peal tuvastatavate-diagrammi loomiseks. Järgmised on vaid üks näide, saate luua aruandeid.

Valige Σ temp ja kellaaja väljad. Need automaatselt Avage väärtuse ja telje diagrammi.

![graphic7][graphic7]

Seda, saate automaatselt diagrammi järgmiselt:

![graphic8][graphic8]

Jaotises väärtus klõpsake rippmenüüst alla temp ja valige **Keskmine** temperatuur ja diagrammi, klõpsake **visualiseeringu** ja valige **joondiagramm**:

![graphic9][graphic9]

Nüüd saate joondiagrammi keskmise aja jooksul.  Nagu allpool PIN-koodi suvandi abil saate kinnitada see varem loodud armatuurlaua:

![graphic10][graphic10]

Nüüd armatuurlaua kinnitatud aruande kuvamisel kuvatakse aruande värskendamine reaalajas. Proovige muuta andmete oma sündmused – kühvli temp või midagi sellist ja te näete reaalajas mõju mis kajastuksid armatuurlauale.

Pange tähele, et selles õpetuses näidanud, kuid ühte tüüpi diagrammi jaoks andmekomplekt loomise kohta. Power BI abil saate luua oma ettevõtte teiste klientide ärianalüüsitööriistade. Power BI armatuurlaua teise näiteks videost [Power BI kasutamise alustamine](https://youtu.be/L-Z_6P56aas?t=1m58s) .

Lisateavet Power BI väljundi konfigureerimise kohta ja kasutada Power BI rühmad, lugege läbi [Power BI jaotis](stream-analytics-define-outputs.md#power-bi) , [mõistmine voo Analytics väljundid](stream-analytics-define-outputs.md "mõistmine voo analüüsi tulemused"). Lisateavet Power BI armatuurlaudade loomine mõne muu kasulik ressurss on [Power BI armatuurlauad](https://powerbi.microsoft.com/documentation/powerbi-service-dashboards/).

## <a name="limitations-and-best-practices"></a>Piirangud ja head tavad

Power BI töötab nii kokkulangevus ja läbilaskevõime piiranguid, nagu on kirjeldatud allpool: [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing "Power BI hinnad")

Sellepärast, et need Power BI lossib ise kõige loomulikult juhul, kui Azure'i voo Analytics ei olulisi andmeid laadi vähendamine.
Soovitame kasutada TumblingWindow või HoppingWindow tagamaks, et andmete tõuketeatised on kõige 1 tõuketeatised sekundis ja et päringu maandub sees läbilaskevõime nõuete – saate anda oma akna sekundites väärtuse arvutamiseks järgmist võrrandit:

![equation1](./media/stream-analytics-power-bi-dashboard/equation1.png)  

Näide – kui teil on iga teine andmete saatmine 1000 seadmed, kasutate Power BI Pro SKU, mis toetab 1 000 000 rida tunnis ja soovite saada keskmiste näitajate kohta seadme Power BI saate teha kõige push iga 4 sekundi kohta seadet (nagu allpool näidatud).  

![equation2](./media/stream-analytics-power-bi-dashboard/equation2.png)  

Mis tähendab, et me ei muudaks algse päring:

    SELECT
        MAX(hmdt) AS hmdt,
        MAX(temp) AS temp,
        System.TimeStamp AS time,
        dspl
    INTO
        OutPBI
    FROM
        Input TIMESTAMP BY time
    GROUP BY
        TUMBLINGWINDOW(ss,4),
        dspl

### <a name="powerbi-view-refresh"></a>PowerBI view värskendamine

Levinud küsimus on "Miks ei armatuurlaud uuendus sisse PowerBI?".

Selleks klõpsake PowerBI kasutada k ja v ja küsib, näiteks "Maksimum, kus ajatempli on juba täna temp" küsimus ja kinnitada, et paani armatuurlaud.

### <a name="renew-authorization"></a>Luba pikendamine

Peate Power BI konto uuesti autentida, kui selle parool on muutunud, kuna teie töö loodud või viimase autenditud. Kui teie peate ka Power BI autoriseerimine järel pikendamiseks Azure Active Directory (AAD) rentniku kohta on konfigureeritud mitme teguriga autentimine (MFA). Selle probleemi sümptom on töö väljund ja "autentimise kasutaja tõrke" toiming logid.

![graphic12][graphic12]

Kui töö püüab alustada samal ajal luba on aegunud, tekib tõrge ja töö käivitamine nurjub. Viga nägema nagu allpool:

![PowerBI valideerimisreegli tõrge](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-expire.png) 
 

Selle probleemi lahendamiseks töötava tööpäevad lõpetada ja liikuge oma Power BI väljundi.  Klõpsake linki "Pikenda autoriseerimine" ja taaskäivitage oma töö lõpetanud viimast andmete kaotsimineku vältimiseks.

![PowerBI valideerimine pikendamine](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renew.png) 

Pärast värskendamist Power BI luba kuvatakse roheline teatise luba ala.

![PowerBI valideerimine pikendamine](./media/stream-analytics-power-bi-dashboard/stream-analytics-power-bi-dashboard-token-renewed.png) 

## <a name="get-help"></a>Abi saamine
Abi saamiseks proovige meie [Azure'i voo Analytics Foorum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Järgmised sammud

- [Azure'i voo Analytics tutvustus](stream-analytics-introduction.md)
- [Azure'i voo Analyticsi kasutamise alustamine](stream-analytics-get-started.md)
- [Skaala Azure'i voo Analytics tööde haldamine](stream-analytics-scale-jobs.md)
- [Azure'i voo Analytics päringu keel viide](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure'i voo Analytics halduse REST API viide](https://msdn.microsoft.com/library/azure/dn835031.aspx)


[graphic1]: ./media/stream-analytics-power-bi-dashboard/1-stream-analytics-power-bi-dashboard.png
[graphic2]: ./media/stream-analytics-power-bi-dashboard/2-stream-analytics-power-bi-dashboard.png
[graphic3]: ./media/stream-analytics-power-bi-dashboard/3-stream-analytics-power-bi-dashboard.png
[graphic4]: ./media/stream-analytics-power-bi-dashboard/4-stream-analytics-power-bi-dashboard.png
[graphic5]: ./media/stream-analytics-power-bi-dashboard/5-stream-analytics-power-bi-dashboard.png
[graphic6]: ./media/stream-analytics-power-bi-dashboard/6-stream-analytics-power-bi-dashboard.png
[graphic7]: ./media/stream-analytics-power-bi-dashboard/7-stream-analytics-power-bi-dashboard.png
[graphic8]: ./media/stream-analytics-power-bi-dashboard/8-stream-analytics-power-bi-dashboard.png
[graphic9]: ./media/stream-analytics-power-bi-dashboard/9-stream-analytics-power-bi-dashboard.png
[graphic10]: ./media/stream-analytics-power-bi-dashboard/10-stream-analytics-power-bi-dashboard.png
[graphic11]: ./media/stream-analytics-power-bi-dashboard/11-stream-analytics-power-bi-dashboard.png
[graphic12]: ./media/stream-analytics-power-bi-dashboard/12-stream-analytics-power-bi-dashboard.png
[graphic13]: ./media/stream-analytics-power-bi-dashboard/13-stream-analytics-power-bi-dashboard.png
