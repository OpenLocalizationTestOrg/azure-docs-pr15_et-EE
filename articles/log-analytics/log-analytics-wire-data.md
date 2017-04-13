<properties
    pageTitle="Log Analytics andmete lahenduse traat | Microsoft Azure'i"
    description="Kaabel konsolideeritud võrgu ja jõudluse andmed on OMS agentide, sh Toiminguhalduri ja Windowsi ühendatud agentide arvutite kaudu. Log andmetega, mis aitavad teil oleksid andmed ühendatakse võrgu andmed."
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

# <a name="wire-data-solution-in-log-analytics"></a>Traat Log Analytics andmete lahendus

Kaabel konsolideeritud võrgu ja jõudluse andmed on OMS agentide, sh Toiminguhalduri ja Windowsi ühendatud agentide arvutite kaudu. Log andmetega, mis aitavad teil oleksid andmed ühendatakse võrgu andmed. OMS agentide teie IT taristu kuvari võrgu andmete saadetud ja nende arvutitest võrgu tase 2 – 3, sh erinevate kasutatud pordid ja protokollid [OSI mudeli](https://en.wikipedia.org/wiki/OSI_model) arvutisse installitud.

>[AZURE.NOTE] Kaabel andmete lahendust pole praegu saadaval, lisatakse tööruumid. Kliendid, kellel on juba lubatud kaabel andmete lahenduse saate jätkata kaabel andmete lahendus.

Vaikimisi kogub OMS hinnale Windowsi sisse ehitatud CPU, mälu, ketta ja võrgu jõudluse andmete sisseloginud andmed. Võrgu- ja muude andmete kogumine tehakse reaalajas iga agent, sh alamvõrku ja Rakendusetaseme Protokollid arvuti kasutab. Saate lisada muid jõudluse hinnale logide vahekaardil lehel sätted.

Kui olete kasutanud [sFlow](http://www.sflow.org/) või muu tarkvara [Cisco NetFlow](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html)protokoll, siis statistika ja andmed kuvatakse kaabel andmete põhjal on teile tuttavad.

Mõned sisseehitatud Log otsingupäringuid tüübid on järgmised:

- Kaabel andmetega varustavate agentide
- IP-aadress agentide kaabel andmed
- IP-aadresside väljaminevaks andmesideks
- Rakendus Protokollid saadetud baitide
- Teenuse rakenduse saadetud baitide
- Saadud erinevaid protokolle baiti
- Kokku baiti saadetud ja vastu võetud IP
- IP-aadressid, mis on temaga agentide 10.0.0.0/8 alamvõrgu
- Keskmise latentsuse ühendusi, mis olid usaldusväärselt
- Arvuti protsesside algatatud või vastuvõetud võrguliikluse
- Võrguliikluse jaoks protsess

Kui otsite kaabel andmete kasutamise, saate filtreerida ja vaadata teavet ülemise agentide ja ülemise Protokollid andmete rühmitamine. Saate vaadata, millal sisse- või teatud arvutites (IP-aadresside-või MAC-aadressid) edastada, kui kaua ja kui palju andmeid on saadetud--põhimõtteliselt, saate vaadata metaandmeid võrguliiklust, mis on otsingupõhiste.

Kuid kuna vaatate metaandmete, ei ole tingimata abiks põhjalikumat tõrkeotsingut. Kaabel andmete OMS pole täielik jäädvustada võrguandmete. Et see on mõeldud sügav paketi taseme tõrkeotsing.
On see eelis, kasutades agent, võrrelduna teiste saidikogumi meetodid, et teil pole seadmete installida, konfigureerida oma võrgu parameetrid või preform keeruline konfiguratsioone. Andmete kaabel on lihtsalt agent põhinev--arvutisse installida agent ja jälgib oma võrguliiklust. Teine eelis on see, kui soovite jälgida pilveteenuse pakkujate või majutusteenuse pakkuja või Microsoft Azure'i, kui kasutaja ei kuulu struktuuri layer töökoormus.

Seevastu pole täielik nähtavus, mis toimub võrgus, kui te ei installi agentide kõik arvutid võrgu taristule.

## <a name="installing-and-configuring-the-solution"></a>Installimine ja konfigureerimine lahendus
Kasutage järgmist teavet, installida ja konfigureerida lahendus.

- Lahendus kaabel andmeid saab andmete arvutid, kus töötab Windows Server 2012 R2, Windows 8.1 ja uuemat opsüsteemi.
- Arvutites, kuhu soovite hankida kaabel andmeid on vaja Microsoft .NET Framework 4.0 või uuem versioon.
- Lisage oma OMS tööruumi abil [lisada Log Analytics](log-analytics-add-solutions.md)Solutions lahendusegaleriist kirjeldatud kaabel andmete lahendus.  On pole veel konfigureerimine vajalik.
- Kui soovite kuvada kindlate lahenduse kaabel andmed, peate olema juba lisanud oma OMS tööruumi lahendus.

## <a name="wire-data-data-collection-details"></a>Traat andmete andmete kogumise üksikasjad

Kaabel andmete kogub metaandmete võrguliiklust agentide, mida teil on lubatud kasutamise kohta.

Järgmises tabelis on andmete kogumise meetodite ja muud üksikasjad kohta, kuidas andmeid kogutakse kaabel andmete jaoks.


| platvorm | Otsest Agent | SCOM agent | Azure'i salvestusruum | SCOM on nõutav? | Rühma kaudu saadetud SCOM agendi andmed | saidikogumi sagedus |
|---|---|---|---|---|---|---|
|Windowsi (2012 R2 / 8.1 või uuem versioon)|![Jah](./media/log-analytics-wire-data/oms-bullet-green.png)|![Jah](./media/log-analytics-wire-data/oms-bullet-green.png)|![Ei](./media/log-analytics-wire-data/oms-bullet-red.png)|            ![Ei](./media/log-analytics-wire-data/oms-bullet-red.png)|![Ei](./media/log-analytics-wire-data/oms-bullet-red.png)| iga 1 minuti|


## <a name="combining-wire-data-with-other-solution-data"></a>Ühendamine kaabel andmete muude lahenduse andmetega

Sisseehitatud päringute eespool näidatud tagastatud andmeid võib olla huvitav ise. Siiski kaabel andmete kasulikkus on aru, kui teil omavahel kombineerida teiste OMS lahenduste teabega. Näiteks saate kasutada kogutud turvalisus ja Audit lahenduse turvalisus sündmuse andmeid ja kombineerida kaabel andmeid otsida ebatavalised võrgu sisselogimise katsete nimega protsesside jaoks.  Selles näites oleks IN ja DISTINCT tehtemärkide abil liituda andmepunktide päringu.

Nõuded: Järgmises näites kasutamiseks peate olema installitud turvalisus ja Audit lahendus. Siiski saate muid lahendusi andmete kombineerimiseks kaabel andmetega sarnase tulemuse saavutamiseks.

### <a name="to-combine-wire-data-with-security-events"></a>Kaabel andmete ühendamiseks ja turvalisus sündmused

1. Klõpsake lehe Ülevaade **WireData** paani.
2. **Levinud WireData päringute**loendis nuppu **Summa, võrguliikluse (baitides) protsessi** tagastatud protsesside loendi kuvamiseks.
    ![wiredata päringud](./media/log-analytics-wire-data/oms-wiredata-01.png)
3. Kui protsesside loend on liiga pikk hõlpsalt vaadata, saate muuta päringu anda standardtabelile:

    ```
    Type WireData | measure count() by ProcessName | where AggregatedValue <40
    ```
    Järgmises näites on protsessi nimega DancingPigs.exe, mis võidakse kuvada kahtlaste.
    ![wiredata Otsingu tulemused](./media/log-analytics-wire-data/oms-wiredata-02.png)

4. Klõpsake loendis tagastatavaid andmeid kasutades nimega protsess. Selles näites klõpsatud DancingPigs.exe. Tulemused kuvatakse allpool kirjeldatakse võrguliiklust nagu väljaminev suhtlemine erinevate-protokolli.
    ![Kui tulemused näitavad nimega protsessi wiredata](./media/log-analytics-wire-data/oms-wiredata-03.png)

5. Kuna turvalisus ja Audit lahendus on installitud, saate te sond turvalisus sündmusi, mis on ProcessName sama välja väärtuse muutmise abil IN ja DISTINCT otsingu päringu tehtemärgid päringu abil. Saate seda teha siis kui nii kaabel andmete ja muu lahendus logid on väärtuste sama vormingut. Päringu sarnaneb muutmiseks tehke järgmist.

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName}
    ```    

    ![wiredata, on tulemuseks kuvab ühendatud andmed](./media/log-analytics-wire-data/oms-wiredata-04.png)
6. Ülaltoodud tulemused, näete, et konto teave kuvatakse. Nüüd saate välja selgitada, kui sageli kasutatud konto, turvalisus ja Audit andmed, kus protsessi päring, mis sarnaneb päringu täpsemalt piiritleda:        

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName} | measure count() by Account
    ```

    ![kuvab konto andmeid wiredata tulemused](./media/log-analytics-wire-data/oms-wiredata-05.png)



## <a name="next-steps"></a>Järgmised sammud

- [Otsingu logid](log-analytics-log-searches.md) üksikasjalik kaabel andmete otsing kirjete vaatamiseks.
- Vaadake, Dan [kaabel andmete kasutamise toimingute haldus komplekti Log otsing ajaveebi postitamine](http://blogs.msdn.com/b/dmuscett/archive/2015/09/09/using-wire-data-in-operations-management-suite.aspx) on täiendavat teavet, kuidas sageli koguda andmeid ja kuidas saab muuta Saidikogumi atribuudid Toiminguhalduri agentide.
