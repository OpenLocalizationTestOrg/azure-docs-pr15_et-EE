<properties
   pageTitle="SQL-i andmete ladu ohtude tuvastamise kasutamise alustamine"
   description="Kuidas alustada ohtude tuvastamise"
   services="sql-data-warehouse"
   documentationCenter=""
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/24/2016"
   ms.author="lodipalm;sonyama;barbkess"/>


# <a name="get-started-with-threat-detection"></a>Ohtude tuvastamise kasutamise alustamine

> [AZURE.SELECTOR]
- [Auditi](sql-data-warehouse-auditing-overview.md)
- [Ohtude tuvastamise](sql-data-warehouse-security-threat-detection.md)

## <a name="overview"></a>Ülevaade

Ohtude tuvastamise tuvastab Anomaalne andmebaasi tegevuste ohtude andmebaasiga, mis näitab. Ohtude tuvastamise on eelvaates ja SQL-i andmebaas on toetatud.

Ohtude tuvastamise pakub uus kiht turvalisus, mis võimaldab tuvastada ja vastata võimalike ohtudega, andes Turbeteatiste Anomaalne tegevuste esinevad kliendid. Kasutajaid saate uurida kahtlaste sündmuste [Azure SQL-i andmete laoseisu kontrollimine](sql-data-warehouse-auditing-overview.md) abil kindlaks teha, kui need tulenevad katse juurde, rikkumise või kasutada andmete andmebaas.
Ohtude tuvastamise lihtne aadress võimalike ohtudega expert väärtpaberi või hallata täpsemate turbesätete jälgimise süsteemid vajamata andmebaas.

Näiteks tuvastab ohtude tuvastamise teatud Anomaalne andmebaasi tegevuste võimalike SQL süsti katsete, mis näitab. SQL süst on üks levinud Web rakenduse väärtpaberi probleemid, mis Internetis, kasutada rünnaku Andmepõhiste rakendused. Ründajad ära rakenduse kohtade sisestab pahatahtlik SQL-lauseid kirje väljad, rikkumise või andmebaasi andmeid muutma.


## <a name="set-up-threat-detection-for-your-database"></a>Ohtude tuvastamise andmebaasi häälestamine

1. Käivitage Azure portaali aadressil [https://portal.azure.com](https://portal.azure.com).

2. Liikuge konfiguratsiooni tera SQL-i andmebaas, mida soovite jälgida. Valige sätted tera, **auditeerimine ja ohtude tuvastamise**.

    ![Navigeerimispaan][1]

3. **Valemiauditi ja ohtude tuvastamise** konfiguratsiooni tera omakorda **ON** auditeerimine, mis kuvab ohtu tuvastamise sätted.

    ![Navigeerimispaan][2]

4. Lülitage **ON** ohtude tuvastamise.

5. Konfigureerige kirju, mis saadetakse Turbeteatiste avastamisel Anomaalne andmete ladu tegevuste loendit.

6. **Valemiauditi & ohtu tuvastamise** konfiguratsiooni tera salvestada uue või värskendatud auditeerimine ja ohtu tuvastamise poliitika nuppu **Salvesta** .

    ![Navigeerimispaan][3]


## <a name="explore-anomalous-data-warehouse-activities-upon-detection-of-a-suspicious-event"></a>Anomaalne andmete ladu tegevuste avastamisel kahtlaste sündmuse uurimine

1. Saate meiliteatise Anomaalne andmebaasi tegevuste avastamisel. <br/>
E-posti annab teavet kahtlaste väärtpaberi sündmus, sh laadi Anomaalne tegevusi, andmebaasi nimi, serveri nimi ja sündmuse aeg. Lisaks annab teavet võimalike põhjuste ja Soovitatavad toimingud, uurida ja leevendada võimalikku ohtu andmebaasi.<br/>

    ![Navigeerimispaan][4]

2. E-posti, klõpsake **Azure SQL-i auditeerimine Log** link, mis käivitab Azure klassikaline portaali ja oluline Valemiauditi kirjete kahtlaste sündmuse ajal.

    ![Navigeerimispaan][5]

3. Auditilogi kirjed kahtlaste andmebaasi tegevuse SQL-lauset, nt üksikasjade kuvamiseks klõpsake nurjumise põhjust ja kliendi IP.

    ![Navigeerimispaan][6]

4. Auditi kirjeid tera, **avada Excelis** eelnevalt konfigureeritud avamiseks klõpsake Exceli malli importida ja käivitage auditilogi kahtlaste sündmuse kellaajal põhjalikult analüüsida.<br/>
**Märkus:** Rakenduses Excel 2010 või uuem versioon, Power Query ja sätte **Kiirühendamine** on nõutav

    ![Navigeerimispaan][7]

5. Klõpsake menüüs **POWER QUERY** - sätte **Kiirühendamine** konfigureerimiseks valige **Suvandid** dialoogiboksis Suvandid kuvamiseks. Valige jaotises Privaatsus ja valige teine valik – "Ignoreeri saadaoleva privaatsustaseme erinevusi ja potentsiaalselt parandada jõudlust".

    ![Navigeerimispaan][8]

6. SQL-i auditilogid laadimiseks tagada, et parameetrid menüü on õigesti seatud ja valige lindi "Andmed" ja 'Värskenda kõik' nuppu sätted.

    ![Navigeerimispaan][9]

7. Tulemused kuvatakse **SQL-i auditilogid** leht, mis võimaldab teil põhjalikult analüüsida tuvastatud Anomaalne tegevuste käivitamiseks ja leevendada väärtpaberi sündmus oma rakenduse.


<!--Image references-->
[1]: ./media/sql-data-warehouse-security-threat-detection/1_td_click_on_settings.png
[2]: ./media/sql-data-warehouse-security-threat-detection/2_td_turn_on_auditing.png
[3]: ./media/sql-data-warehouse-security-threat-detection/3_td_turn_on_threat_detection.png
[4]: ./media/sql-data-warehouse-security-threat-detection/4_td_email.png
[5]: ./media/sql-data-warehouse-security-threat-detection/5_td_audit_records.png
[6]: ./media/sql-data-warehouse-security-threat-detection/6_td_audit_record_details.png
[7]: ./media/sql-data-warehouse-security-threat-detection/7_td_audit_records_open_excel.png
[8]: ./media/sql-data-warehouse-security-threat-detection/8_td_excel_fast_combine.png
[9]: ./media/sql-data-warehouse-security-threat-detection/9_td_excel_parameters.png
