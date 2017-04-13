<properties
    pageTitle="Active Directory kopeerimise olek lahenduse Log Analytics | Microsoft Azure'i"
    description="Active Directory kopeerimise olek lahenduse pakett regulaarselt jälgib mis tahes kopeerimise tõrked keskkonna Active Directory ja aruanded tulemused OMS armatuurlauale."
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
    ms.date="10/10/2016"
    ms.author="banders"/>

# <a name="active-directory-replication-status-solution-in-log-analytics"></a>Active Directory kopeerimise olek lahenduse Log Analytics

Active Directory on oluline osa ettevõtte IT-keskkonda. Kõrge-saadavus ja suure jõudlusega tagamiseks on iga domeenikontrolleri Active Directory andmebaasi eraldi koopia. Domeenikontrollerid ise omavahel selleks, et muudatused kajastuma kogu ettevõttes. Selle protsessi kopeerimise tõrked võivad põhjustada mitmesuguseid probleeme kogu ettevõttes.

AD kopeerimise olek lahenduse pakett regulaarselt jälgib mis tahes kopeerimise tõrked keskkonna Active Directory ja aruanded tulemused OMS armatuurlauale.

## <a name="installing-and-configuring-the-solution"></a>Installimine ja konfigureerimine lahendus
Kasutage järgmist teavet, installida ja konfigureerida lahendus.

- Agentide peab olema installitud domeenikontrollerid, mis kuuluvad domeeni hinnatav, või liige serverid konfigureeritud OMS AD dispersioonanalüüs andmeid saata. Mõistmaks, kuidas luua ühenduse Windows arvutis OMS, teemast [ühenduse loomine Windows arvutite Log Analytics](log-analytics-windows-agents.md). Kui domeenikontrolleri on juba olemasoleva System Center Operations Manager keskkonnas, mida soovite ühendada OMS osa, vt [Ühenduse Toiminguhalduri Log Analytics](log-analytics-om-agents.md).
- Active Directory kopeerimise olek lahendus lisada oma OMS tööruumi [lisada Log Analytics lahenduste lahendusegaleriist](log-analytics-add-solutions.md)kirjeldatud protsessi abil.  On pole veel konfigureerimine vajalik.


## <a name="ad-replication-status-data-collection-details"></a>AD kopeerimise olek andmete kogumise üksikasjad

Järgmises tabelis on andmete kogumise meetodite ja muud üksikasjad kohta, kuidas koguda andmeid AD kopeerimise olek.

| platvorm | Otsest Agent | SCOM agent | Azure'i salvestusruum | SCOM on nõutav? | Rühma kaudu saadetud SCOM agendi andmed | saidikogumi sagedus |
|---|---|---|---|---|---|---|
|Windows|![Jah](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![Jah](./media/log-analytics-ad-replication-status/oms-bullet-green.png)|![Ei](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![Ei](./media/log-analytics-ad-replication-status/oms-bullet-red.png)|![Jah](./media/log-analytics-ad-replication-status/oms-bullet-green.png)| iga 5 päeva|


## <a name="optionally-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>Soovi korral saate lubada mitte-domeenikontrolleri AD andmeid saata OMS
Kui te ei soovi ühenduse mis tahes hulka kuuluvad teie domeeni otse OMS, saate kasutada muid OMS ühendatud arvuti AD kopeerimise olek lahenduse pakett andmete kogumise ja saata andmed domeeni.

### <a name="to-enable-a-non-domain-controller-to-send-ad-data-to-oms"></a>Mitte-domeenikontrolleri AD andmeid saata OMS lubamiseks
1.  Veenduge, et arvutisse oleks seda domeeni, mida soovite jälgida, kasutades AD kopeerimise olek lahenduse liige.
2.  [Ühenduse loomine Windowsi arvuti OMS](log-analytics-windows-agents.md) või [ühendage see oma olemasoleva Toiminguhalduri keskkond OMS kasutamine](log-analytics-om-agents.md), kui see pole juba ühendatud.
3.  Selles arvutis seadmine järgmisele registrivõtmele:
    - Võti: **HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\HealthService\Parameters\Management rühmade\<ManagementGroupName > \Solutions\ADReplication**
    - Väärtus: **IsTarge**
    - Väärtuseandmed: **true**

    >[AZURE.NOTE]Need muudatused jõustuvad kuni teie taaskäivitamist teenus Microsoft jälgimise Agent (HealthService.exe).

## <a name="understanding-replication-errors"></a>Dispersioonanalüüs tõrgete mõistmine
Kui teil on saadetud OMS AD dispersioonanalüüs olek andmeid, kuvatakse järgmine paan, mis näitab, mitu dispersioonanalüüs tõrkeid, mis teil praegu on OMS armatuurlaual.  
![AD kopeerimise olek paan](./media/log-analytics-ad-replication-status/oms-ad-replication-tile.png)

**Dispersioonanalüüs kriitilisi tõrkeid** on need, mis on 75% teie Active Directory kogumis [hauakivi neid elu jooksul](https://technet.microsoft.com/library/cc784932%28v=ws.10%29.aspx) või rohkem.

Kui olete klõpsanud, kuvatakse tõrgete kohta rohkem teavet.
![Armatuurlaua AD kopeerimise olek](./media/log-analytics-ad-replication-status/oms-ad-replication-dash.png)


### <a name="destination-server-status-and-source-server-status"></a>Sihtkoha serveri olek ja allika serveri olek
Need terad sihtkoha serverid ja allikas serverites, kus on probleeme dispersioonanalüüs tõrgete oleku kuvamine Pärast iga domeenikontrolleri nimi arv näitab selle domeenikontrolleri dispersioonanalüüs tõrgete arvu.

Tõrgete nii sihtkoha serverid ja allika serverid on näidatud, kuna mõned probleemid on hõlpsam seisukohalt sihtkoha server allikas server perspektiivi ja teiste tõrkeotsing.

Selles näites näete, et paljud sihtkoha serverid on umbes sama palju tõrkeid, kuid on üks allikas server (ADDC35), mis sisaldab palju veel vigu, kui kõik teised. See on tõenäoline, et see ei õnnestu andmeid saata partneritega dispersioonanalüüs põhjustavat ADDC35 kohta on mõni probleem. Kinnitamine probleemide ADDC35 tõenäoliselt lahendada paljud vigu, mis kuvatakse sihtkoha server tera.

### <a name="replication-error-types"></a>Kopeerimise tõrge tüübid
Selle põhjal annab teile tüüpi kogu ettevõtte tuvastatud tõrgete kohta. Iga vea, on kordumatu arvulise koodi ja sõnum, mis aitavad teil kindlaks tõrke.

Rõngas ülaosas annab aimu, milline vigu, kuvatakse lisaks ja harvem teie keskkonnas.

See näitab, kui mitu domeenikontrollerid kogemus kopeerimine sama viga. Sel juhul saate on võimalik, et leida lahenduse ühe domeenikontrolleri tuvastamine ja seejärel korrake seda teiste sama viga poolt mõjutatud domeenikontrolleritesse.

### <a name="tombstone-lifetime"></a>Hauakivi neid elu jooksul
Hauakivi eluiga määratleb kaua kustutatud objekti, hauakivi edaspidi, säilitatakse Active Directory andmebaasi. Kui kustutatud objekti edastab hauakivi eluiga, prügi kogumise protsessi eemaldab automaatselt Active Directory andmebaasist.

Vaikimisi hauakivi eluiga on kõige uuemad versioonid Windowsi, 180 päeva, kuid see oli 60 päeva vanemad versioonid ja seda saab muuta konkreetselt Active Directory administraator.

See on oluline teada, kui teil esineb dispersioonanalüüs tõrkeid, mis on lähenemas või on juba möödas hauakivi eluiga. Kui kaks domeenikontrollerid kopeerimise tõrge, mida püsib viimase hauakivi eluiga, keelatakse dispersioonanalüüs vahel nende kahe domeenikontrollerid isegi juhul, kui on fikseeritud aluseks kopeerimise tõrge.

Hauakivi eluiga tera aitab tuvastada kohti, kus on see juhtub olevate. Iga tõrge kuvatakse **üle 100% TSL** kategooria tähistab sektsioon, mis pole kopeeritud jaoks oma lähte- ja serveri vahel vähemalt hauakivi eluiga mets jaoks.

Sellisel juhul lihtsalt tõrke dispersioonanalüüs ei saa piisavalt. Vähemalt, peate käsitsi välja selgitada probleemi tuvastamiseks ja puhastamise toimingutest objekte enne saab uuesti dispersioonanalüüs. Peate võib-olla isegi domeenikontrolleri eemaldada.

Lisaks kindlaks dispersioonanalüüs vigu, mis on kestnud viimase hauakivi eluiga, samuti peaksite tähelepanu pöörama **50-75% TSL** või **75-100% TSL** liitmisel vigu.

Need on tõrkeid, mis on selgelt toimingutest, mitte siirdamiseks, nii, et nad tõenäoliselt on vaja oma sekkumise lahendamiseks. Hea Uudised on, et nad pole veel kätte hauakivi eluiga. Kui te nende probleemide kohe ja *enne* jõudmist hauakivi eluiga, saate dispersioonanalüüs taaskäivitage minimaalsete manuaalset.

Nagu varem, armatuurlaua paani lahenduse AD kopeerimise olek kuvatakse arv *kriitilised* dispersioonanalüüs tõrgete teie keskkonnas, mis on määratletud tõrkeid, mis on 75% hauakivi eluiga (sh tõrkeid, mis on üle 100% TSL). Püüdma Säilita see arv 0.

>[AZURE.NOTE] Hauakivi eluiga protsent arvutused põhinevad tegeliku hauakivi eluiga jaoks teie Active Directory kogumis, nii, et usaldate, et need protsendid on õiged, isegi juhul, kui teil on kohandatud hauakivi eluea väärtuse määrata.

### <a name="ad-replication-status-details"></a>AD kopeerimine oleku üksikasjad
Kui klõpsate nuppu loendid üksuse, kuvatakse täiendavad üksikasjad selle Log otsingu abil. Tulemused on filtreeritud, et kuvada ainult selle üksusega seotud tõrgete. Näiteks kui klõpsate loendis **Sihtkoha serveri olek (ADDC02)**esimese domeenikontrolleri, kuvatakse otsingutulemite filtreeritud loendis Kuva tõrked selle domeenikontrolleri sihtkoha server:

![AD kopeerimine olek tõrgete otsingutulemite](./media/log-analytics-ad-replication-status/oms-ad-replication-search-details.png)

Siit saate filtreerida täpsemaks, muuta päringu ja jms. Log otsingu kasutamise kohta leiate lisateavet teemast [Log otsingud](log-analytics-log-searches.md).

Väljal **HelpLink** kuvatakse selle tõrke kohta lisateabe lehte TechNeti URL-i. Saate kopeerida ja kleepida selle lingi oma brauseriakna tõrkeotsing ja tõrke kohta leiate teavet.

Võite klõpsata ka **ekspordi** tulemused eksportimine Excelisse. See võimaldab teil kopeerimise tõrge andmete mis tahes viisil, kui soovite visualiseerida.

![eksporditud AD dispersioonanalüüs olek tõrgete Excelis](./media/log-analytics-ad-replication-status/oms-ad-replication-export.png)

## <a name="ad-replication-status-faq"></a>AD kopeerimise olek KKK
**Q: kuidas sageli on AD dispersioonanalüüs olek andmeid värskendatakse?**
V: teavet värskendatakse iga 5 päeva.

**Q: Kas on võimalik konfigureerida andmete värskendamise sagedust?**
V: pole sel ajal.

**Q: Kas ma pean kõik hulka kuuluvad minu domeeni lisamine mu OMS tööruumi selleks, et näha kopeerimise olek?**
V: ei, tuleb lisada ainult ühe domeenikontrolleri. Kui teil on mitu domeenikontrollerid OMS tööruumi, saadetakse need kõik andmed OMS.

**K: ma ei soovi lisada mis tahes domeenikontrollerid minu OMS tööruumi. Saate siiski kasutada AD kopeerimise olek lahendus?**
V: Jah. Saate määrata selle registrivõtme väärtust. Lugege teemat [lubamine mitte - domeenikontrolleri OMS AD andmeid saata](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).

**Q: mis on nimi, mis teeb andmete kogumise protsessi?**
V: AdvisorAssessment.exe

**Q: kuidas palju aega kulub andmete kogumine?**
V: andmete kogumise aeg sõltub keskkonna Active Directory suurust, kuid kulub tavaliselt vähem kui 15 minutit.

**Q: milliseid andmeid kogutakse?**
V: dispersioonanalüüs andmed LDAP kaudu.

**Q: Kas on võimalik konfigureerida, kui andmed on kogutud?**
V: pole sel ajal.

**Q: milliseid õigusi on vaja koguda andmeid?**
V: tavaline kasutusõiguste Active Directory on tavaliselt piisavad.

## <a name="troubleshoot-data-collection-problems"></a>Andmete kogumine probleemide tõrkeotsing
Selleks, et koguda andmeid, AD kopeerimise olek lahenduse pakett nõuab vähemalt üks domeenikontrolleri OMS tööruumi ühendamiseks. Enne selle tegemist, kuvatakse teade, et **andmed on endiselt kogutud**.

Kui vajate abi ühenduse ühte hulka kuuluvad teie domeeni, saate vaadata [ühenduse loomine Windows arvutite Log Analytics](log-analytics-windows-agents.md)dokumentatsiooni. Teise võimalusena kui domeenikontrolleri on juba süsteemi keskmist Toiminguhalduri keskkond juba ühendatud, saate vaadata dokumentatsiooni [Ühenduse System Center Operations Manager Logi Analytics](log-analytics-om-agents.md).

Kui te ei soovi mõnda hulka kuuluvad teie domeeni otse OMS või SCOM ühendamiseks, lugege teemat [lubamine mitte - domeenikontrolleri OMS AD andmeid saata](#to-enable-a-non-domain-controller-to-send-ad-data-to-oms).


## <a name="next-steps"></a>Järgmised sammud

- [Log otsinguid Log Analytics](log-analytics-log-searches.md) abil saate vaadata üksikasjalikku Active Directory kopeerimise olek andmed.
