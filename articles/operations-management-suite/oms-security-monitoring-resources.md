<properties
   pageTitle="Ressursid jälgimise toimingud halduse komplekti turvalisus ja Audit lahenduse | Microsoft Azure'i"
   description="Selle dokumendi abil saate kasutada OMS turvalisus ja jälgida oma ressursse ja turvalisuse probleemid, mis võimalusi."
   services="operations-management-suite"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="operations-management-suite"
   ms.topic="article" 
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/18/2016"
   ms.author="yurid"/>

# <a name="monitoring-resources-in-operations-management-suite-security-and-audit-solution"></a>Toimingute haldus komplekti turvalisus ja Audit lahenduse jälgimisega seotud ressursid

Selle dokumendi aitab teil OMS turvalisus ja Audit võimaluste abil jälgida oma ressursse ja turvalisuse probleemid.

## <a name="what-is-oms"></a>Mis on OMS?

Microsoft toimingute komplekti (OMS) on Microsofti pilveteenuste vastavalt selle lahendus, mis aitab hallata ja kaitsta oma kohapealse ja pilveteenuse taristu. OMS kohta lisateabe saamiseks lugege artiklit [Operations Management Suite](https://technet.microsoft.com/library/mt484091.aspx).

## <a name="monitoring-resources"></a>Jälgimisega seotud ressursid

Iga kord, kui on võimalik, et soovite takistada turvalisus juhtumite toimu kõigepealt. Siiski ei saa kõik turvalisus juhtumite vältimiseks. Kui juhtub turvalisus juhtum, peate veenduge, et selle mõju on minimeeritud.  On kolme kriitilised soovitusi, mis saab kasutada arvu ja turvalisus juhtumite mõju minimeerimiseks.

- Hinnake rutiinselt nõrkade kohtade teie keskkonnas.
- Rutiinselt märkige kõigi arvutisüsteemide ja võrguseadmete tagamaks, et nad on kõik uusima plaastrid installitud.
- Rutiinselt kontrollida kõik logid ja logimine menetlustele, sh operatsioonisüsteemi sündmuselogide, teatud logid ja sissetungi tuvastamise süsteemi logid.

OMS turvalisus ja Audit lahendus võimaldab aktiivselt jälgida kõiki ressursse, mis aitavad minimeerida turvalisus juhtumid. OMS turvalisus ja Audit on turvalisus domeene, mida saab kasutada jälgimise ressursid. Turvalisus domeenid annab kiire juurdepääsu soovitud suvandite turvalisus jälgimiseks, käsitletakse järgmisi domeene rohkem üksikasju:

- Malware hindamine
- Värskendage hindamine
- Identiteedi ja juurdepääs

> [AZURE.NOTE] Kõik need suvandid ülevaate lugeda [toimingute haldus komplekti turvalisus ja Audit lahenduse töötamise alustamine](oms-security-getting-started.md).

### <a name="monitoring-system-protection"></a>Kaitse jälgimine

Sügavus lähenemine vastuses, iga kiht kaitse oluline üldine turvalisus olek teie vara. Kuvatakse tuvastatud ohud arvutite ja arvutite piisavalt kaitse ründevara Assessment paani jaotises Turve domeenid. Malware hindamise toodud teabe abil saate tuvastada plaani kaitse rakendamiseks serverid, et seda vaja. Juurdepääs suvand järgige alltoodud juhiseid:

1. Klõpsake **Microsoft Operations Management Suite** peamine armatuurlaud **Turvalisus ja Audit** paani.

    ![Turvalisus ja Audit](./media/oms-security-responding-alerts/oms-security-responding-alerts-fig1.png)

2. **Turvalisus ja Audit** armatuurlaud, klõpsake jaotises **Turve domeenid** **Ründevaratõrje hindamine** . **Ründevaratõrje Assessment** armatuurlaua kuvatakse nagu allpool näidatud:

![Malware hindamine](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig2-ga.png)

Armatuurlaua **Ründevara hindamise** abil saate tuvastada turvalisus järgmist:

- **Aktiivse ohtude**: arvutid, mis on rikutud ja teil on aktiivne ohtude süsteem.
- **Remediated ohtude**: arvutid, mis on rikutud, kuid ohud olid tervendama.
- **Allkirja aegunud**: arvutites, kus on lubatud ründevara tõrje, kuid allkirja on aegunud.
- **Mitte reaalajas kaitse**: pole installitud ründevaratõrje arvutites.

### <a name="monitoring-updates"></a>Värskenduste jälgimine 

Viimati tehtud turbevärskendusi on levinud ja see tuleb lisada update halduse strateegia kavandamine. Teenuse Microsoft jälgimise Agent (HealthService.exe) loeb tagasisidet jälgida arvutite ja saadab see värskendatud teave OMS teenuse pilveteenuses töötlemiseks. Microsoft Agenti jälgimine on konfigureeritud automaatselt teenust ja peaks target arvutis alati töötab.

![värskenduste jälgimine](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig3.png)

Loogika on rakendatud andmete värskendamiseks ja pilveteenusesse andmed. Kui puuduvad värskendused on leitud, kuvatakse need armatuurlaual **värskendused** . **Värskenduste** armatuurlaua abil saate töötada puuduvad värskendused ja arendada plaani rakendamiseks serverid, kus see on vajalik. Järgige alltoodud juurdepääsu armatuurlaud **värskendused** :

1. Klõpsake **Microsoft Operations Management Suite** peamine armatuurlaud **Turvalisus ja Audit** paani.
2. **Turvalisus ja Audit** armatuurlaual nuppu **Värskenda Assessment** jaotises **Turve domeenid**. Värskenduse armatuurlaua kuvatakse nagu allpool näidatud:

![Värskendage hindamine](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig4.png)

Armatuurlaua saate teha update hinnang mõista praeguses olekus arvutite ja kõige tähtsamate vastu. **Kriitiline või turbevärskendusi** paani abil saab IT-administraatoritele juurdepääsu üksikasjalikku teavet värskendusi, mis on puudu, nagu allpool näidatud:

![otsingutulemite](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig5.png)

See aruandesse kriitilist teavet ohtu süsteemi on juurdepääs, mis sisaldab seotud värskendus ja MS Bulletin, mis sisaldab täpsemat teavet haavatavuse Microsoft KB artikleid tüüpi tuvastamiseks kasutatud.

### <a name="monitoring-identity-and-access"></a>Identiteedi ja juurdepääs

Kasutajatega töötamise igalt poolt, kasutades ühendamiseks ja juurdepääs suure hulga asutusesisese ja pilvepõhise rakendused, on oluline, et volituste on kaitstud. Mandaadi identiteedivarguse eest on need, kus saab ründaja algselt juurdepääsu regulaarne kasutaja mandaat süsteemi võrgustikus juurdepääs. Mitu korda, selle algse rünnaku on ainult viis, kuidas saada juurdepääsu, lõppeesmärgiks on avastada õiguste kontod. 

Ründajad jäävad võrgu, kasutades vabalt saadaval instrumentaarium identimisteave eraldamiseks muudelt kontodelt sisse logitud seansid. Sõltuvalt süsteemi konfiguratsioonist, saate neid mandaate hashes, piletid või isegi Lihttekst paroolide kujul ekstraktita.  

> [AZURE.NOTE] masinad, mis on otseselt Interneti näevad palju nurjunud katsete, mida proovite sisse logida kasutades kõik liiki tuntud kasutajanimed (nt administraator). Enamikul juhtudel on OK kui tuntud kasutajanimed ei kasutata ja parool on piisavalt tugev.

On võimalik neid sissetungijad tuvastamiseks enne, kui nad kahjustada õigustega kontot. Saate kasutada **OMS turvalisus ja Audit lahenduse** identiteedi ja juurdepääsu jälgimiseks. Järgige alltoodud juurdepääsu **identiteedi ja juurdepääsu** armatuurlaud:

1. **Microsoft Operations Management Suite** peamisi armatuurlaua klõpsake turvalisus ja Audit paani.
2. **Turvalisus ja Audit** armatuurlaua, klõpsake jaotises **Turve domeenid** **identiteedi ja juurdepääsu** . **Identiteedi ja juurdepääsu** armatuurlaua kuvatakse nagu allpool näidatud:

![identiteedi ja juurdepääs](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig6-ga.png)

Osana tavaline jälgimisega seotud strateegia kavandamine, peate lisama identiteedi jälgimine. IT-administraator peaks välja nägema, kui seal on teatud lubatud kasutajanimi, mis sisaldab palju. See võib näitab, kas ründaja, mis omandatud tegelikke kasutajanimi ja proovige jõuvõtete või mõne automaatselt tööriist kasutab raske koodiga parooli aegunud.

Armatuurlaua lubada kiiresti tuvastada identiteedi ja juurdepääsu ettevõtte ressursse seotud võimalike ohtudega. See on eriti oluline tuvastamiseks ka võimalikud trende, näiteks kui ka üle aja paanil, saate vaadata mitu korda on nurjunud sisselogimiskatse läbi aja jooksul. Sel juhul ei saanud arvuti **failiserver** 35 sisselogimise katsete. Klõpsates saate uurida täpsemat teavet sellest arvutist. 

![Lisateavet](./media/oms-security-monitoring-resources/oms-security-monitoring-resources-fig7-new.png)

Selles arvutis loodud aruande toob selle mustri väärtuslik üksikasjad. Märganud, et **konto** veerus annab teile kasutajakontol, mida kasutati süsteemi proovida, **TIMEGENERATED** veeru annab teile, kus on tehtud proovi ajavahemik ja **LOGONTYPENAME** veeru annab teile asukoht, kuhu see on tehtud. Kui nende katsete täitis kohalik süsteem programmi, veerust **PROCESS** oleks kuvatud selle protsessi nimi. Stsenaariumid, kus sisselogimiskatse on pärit programmi, olete juba protsessi nimi, mis on saadaval ja nüüd saate teha põhjalikumaks uurimine target süsteemi.

## <a name="see-also"></a>Vt ka

Selles dokumendis soovite õpitut kasutamine OMS turvalisus ja Audit lahendus jälgida oma ressursse. OMS turvalisuse kohta lisateabe saamiseks lugege järgmisi artikleid:

- [Toimingute haldus komplekti (OMS) ülevaade](operations-management-suite-overview.md)
- [Toimingute haldus komplekti turvalisus ja Audit lahenduse töötamise alustamine](oms-security-getting-started.md)
- [Jälgimine ja toimingute haldus komplekti turvalisus ja Audit lahenduse Turbeteatiste reageerimine](oms-security-responding-alerts.md)