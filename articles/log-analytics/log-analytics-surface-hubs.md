<properties
    pageTitle="Surface'i jaoturi Log Analytics jälgimine | Microsoft Azure'i"
    description="Surface jaoturi lahenduse abil saate jälgida oma Surface jaoturi seisundi ja mõista, kuidas neid kasutatakse."
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
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="banders"/>

# <a name="monitor-surface-hubs-with-log-analytics"></a>Kuvari Surface'i jaoturi Log Analytics

Selles artiklis kirjeldatakse kasutamist Surface jaoturi lahenduse Log Analytics jälgida Microsoft Surface jaoturi seadmed koos selle Microsoft toimingute komplekti (OMS). Log Analyticsi abil saate jälgida oma Surface jaoturi seisundi ka mõista, kuidas neid kasutatakse.

Iga Surface jaoturi on Microsoft Agenti jälgimine installitud. Selle agent, et saate saata andmed teie pind keskuse kaudu OMS kaudu. Logifailide on lugeda teie Surface jaoturi ja kas siis saadetakse OMS teenuse. Probleemid, näiteks serverid on ühenduseta režiimis ei saa sünkroonida, kalendri või kui seadme kontot ei saa sisse logida Skype'i on näidatud OMS Surface jaoturi armatuurlaual. Armatuurlaua andmed abil saate tuvastada seadmetes, mis ei tööta, või mis on muid probleeme, ja rakendage potentsiaalselt parandusi tuvastatud probleemid.


## <a name="installing-and-configuring-the-solution"></a>Installimine ja konfigureerimine lahendus

Kasutage järgmist teavet, installida ja konfigureerida lahendus. Selleks, et hallata oma Surface jaoturi kaudu soovitud Microsoft toimingute komplekti (OMS), peate järgmist.

- [OMS](http://www.microsoft.com/oms)kehtiv tellimust.
- [OMS tellimuse](https://azure.microsoft.com/pricing/details/log-analytics/) tase, arv seadmed toetavad, mida soovite jälgida. OMS hinnad erinevad sõltuvalt sellest, kui palju seadmeid õpib ja kui palju andmeid protsessid. Peaksite seda Surface jaoturi plaanida kavandamisel arvesse võtta.

Järgmiseks on tellimuse OMS lisamine olemasoleva Microsoft Azure tellimuse või otse portaalis OMS uue tööruumi loomine. Üksikasjalikud juhised mõlema meetodi abil on [Log Analytics alustamine](log-analytics-get-started.md). Kui OMS tellimus on häälestatud, on kaks võimalust registreeruda Surface jaoturi seadmetes:

- Automaatselt InTune kaudu
- Käsitsi – Surface jaoturi seadme **sätted** .

## <a name="set-up-monitoring"></a>Häälestamine jälgimine

Saate jälgida seisundi ja oma Surface jaoturi abil Log Analytics OMS tegevus. InTune abil või kohalikult Surface jaoturi klõpsake **sätete** abil saate registreeruda Surface jaoturi OMS sisse.

## <a name="connect-surface-hubs-to-oms-through-intune"></a>Surface'i jaoturi ühenduse OMS InTune kaudu

Peate oma Surface jaoturi haldavatele OMS tööruumi tööruumi ID ja tööruumi võti. Saate need OMS portaalist.

InTune on Microsofti toote puhul saate ühes keskses kohas hallata OMS konfiguratsioonisätted, mis on rakendatud üks või mitu seadmetes. Seadme kaudu InTune konfigureerimiseks tehke järgmist

1. InTune sisse logida.
2. Liikuge **sätted** > **ühendatud allikatest**.
3. Luua või muuta poliitika Surface jaoturi malli põhjal.
4. Liikuge OMS (Azure'i töö ülevaated) jaotises poliitika, ja lisada poliitika *Tööruumi ID* ja *Tööruumi võti* .
5. Salvestage poliitika.
6. Poliitika seostada vastav rühma seadmed.

  ![InTune poliitika](./media/log-analytics-surface-hubs/intune.png)

InTune sünkroonib OMS sätted seejärel seadmete sihtrühma, kasutamine neid oma OMS tööruumis.

## <a name="connect-surface-hubs-to-oms-using-the-settings-app"></a>Surface jaoturi ühenduse OMS sätted rakenduse kasutamine

Peate oma Surface jaoturi haldavatele OMS tööruumi tööruumi ID ja tööruumi võti. Saate need OMS portaalist.

Kui te ei kasuta InTune keskkonna haldamise, saate registreeruda seadmete käsitsi iga Surface jaoturi **sätted** :

1. Avage oma pind jaoturis **sätted**.
2. Sisestage seadme administraatori identimisteave, kui seda küsitakse.
3. Klõpsake **selle seadme**, ja klõpsake jaotises **jälgimine**, **OMS sätete konfigureerimine**.
4. Valige **Luba jälgimine**.
6. OMS dialoogiboksis sätted, tippige **Tööruumi ID** ja **Tööruumi võti**.  
  ![sätted](./media/log-analytics-surface-hubs/settings.png)
7. Klõpsake nuppu **OK** , et konfigureerimise lõpuleviimiseks.

Ütleb teile, kas OMS konfigureerimine on rakendatud seadmele kuvatakse kinnitus. Kui see on, kuvatakse teade selle kohta, et agent ühendus on loodud OMS-teenuse. Seadme hakkab siis OMS, kus saate kuvada ja seda andmete saatmine.

## <a name="monitor-surface-hubs"></a>Surface'i jaoturi jälgimine

Jälgida oma Surface jaoturi abil OMS on palju nagu sisselogimise seadmete jälgimine.

1. OMS portaali sisse logida.
2. Liikuge jaoturi pind lahenduse keelepaketi armatuurlauale.
3. Seadme seisund kuvatakse.

  ![Surface'i jaoturi armatuurlaud](./media/log-analytics-surface-hubs/surface-hub-dashboard.png)

Saate luua [teatised](log-analytics-alerts.md) , võttes aluseks olemasolevad või kohandatud log otsinguid. Andmed on OMS kogub oma Surface jaoturi abil saate otsida probleemid ja teatis kuvatakse teie seadme jaoks määratud tingimustele.


## <a name="next-steps"></a>Järgmised sammud

- [Log otsingud Log Analytics](log-analytics-log-searches.md) abil Surface jaoturi üksikasjalike andmete kuvamine.
- [Teatiste](log-analytics-alerts.md) märku probleeme tekkida oma Surface jaoturi luua.
