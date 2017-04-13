<properties
    pageTitle="Logige Analytics andmete turvalisus | Microsoft Azure'i"
    description="Siit saate teada, kuidas Log Analytics teie privaatsuse ja teie andmeid."
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
    ms.date="09/23/2016"
    ms.author="banders"/>

# <a name="log-analytics-data-security"></a>Logige Analytics andmete turvalisus

Microsoft on võtnud privaatsuse kaitsmine ja andmete turvamise tarkvara pakkumisel ja teenuseid, mis aitavad teil hallata oma ettevõtte IT taristu. Me tunne, et kui usaldate oma andmed teistele, et usalda nõuab range turvalisus. Microsoft järgib nõuetele vastavus ja turve range – kodeerimine teenuste kaudu.

Ning andmed on Microsofti prioriteet. Võtke meiega ühendust küsimusi, soovitusi, või probleemide kohta järgmise teabe, sh meie turbepoliitikate [Azure'i tugiteenused](http://azure.microsoft.com/support/options/)veebisaidil.

Selles artiklis selgitatakse, kuidas andmed on kogutud, töödeldud, ja turvatud Analytics Logi sisse toimingud halduse komplekti (OMS). Saate agentide ühenduse veebiteenuse, System Center Operations Manager abil koguda andmeid või Azure diagnostika kasutamiseks Log Analytics andmete toomine. Log Kasutusanalüüsi teenus, mis majutab Microsoft Azure serdi autentimise ja 3 SSL-i abil kogutud andmete saadetakse Interneti kaudu. Andmed on tihendatud agent enne saatmist.

Log Analytics teenust haldab pilvepõhist andmete turvaliseks, kasutades järgmist:

- andmete eraldamine
- andmete säilitamine
- füüsiline turvalisus
- sündmuse juhtimine
- nõuetele vastavus
- turvalisuse standardid kinnitamine


## <a name="data-segregation"></a>Andmete eraldamine

Klientide andmeid hoitakse iga komponendi kogu OMS teenuse loogiliselt eraldi. Kõik andmed on sildistatud organisatsiooni kohta. See sildistamine jätkub kogu andmete elutsükli ja see on jõustatud igal kiht teenuse. Iga kliendi on spetsiaalne Azure'i bloobimälu, mis majutab pikaajalise andmete

## <a name="data-retention"></a>Andmete säilitamine

Indekseeritud log otsingu andmed salvestatakse ja säilitatakse vastavalt teie hinnakirjad leping. Lisateabe saamiseks lugege teemat [Log Analytics hinnad](https://azure.microsoft.com/pricing/details/log-analytics/).

Microsoft kustutab kliendiandmete 30 päeva pärast OMS tööruum on suletud. Samuti kustutab Microsoft Azure Storage konto, kus andmed asuvad. Kliendiandmete eemaldamisel hävitatakse pole füüsilise draivid.

Järgmises tabelis on loetletud mõned OMS ja näited andmete tüüpi kogutakse saadaval lahendused.

| **Lahendus** | **Andmetüübid** |
| --- | --- |
| Konfiguratsiooni hindamine | Otsingukonfiguratsiooni andmete, metaandmete ja andmete |
| Võimsuse planeerimine | Jõudluse andmed ja metaandmed |
| Ründevaratõrje | Konfiguratsiooni andmed ja metaandmed |
| Süsteemi värskenduse hindamine | Metaandmete ja state andmed |
| Haldamine | Kasutaja määratletud sündmuselogide, Windows sündmuselogide ja/või IIS-i logid |
| Muutuste jälitus | Laoseisu tarkvara ja Windowsi teenuse metaandmed |
| SQL-i ja Active Directory hindamine | WMI andmeid, registriandmete, jõudlusandmeid ja SQL serveri dünaamilise juhtimise tulemuste kuvamine |

Järgmine tabel sisaldab andmetüüpide.

| **Andmetüüp** | **Väljad** |
| --- | --- |
| Teavita | Teavita nimi, teatise kirjeldus, BaseManagedEntityId, probleemi ID, IsMonitorAlert, RuleId, ResolutionState, prioriteet, raskusaste, kategooria, omaniku, ResolvedBy, TimeRaised, TimeAdded, LastModified, LastModifiedBy, LastModifiedExceptRepeatCount, TimeResolved, TimeResolutionStateLastModified, TimeResolutionStateLastModifiedInDB, RepeatCount |
| Konfigureerimine | Tellijaid, AgentID, EntityID, ManagedTypeID, ManagedTypePropertyID, praegune väärtus, ChangeDate |
| Sündmuse | EventId, EventOriginalID, BaseManagedEntityInternalId, RuleId, PublisherId, PublisherName, FullNumber, arv, kategooria, ChannelLevel, LoggingComputer, EventData, EventParameters, TimeGenerated, TimeAdded <br>**Märkus:** Kui logite sisse Windowsi sündmuselogi kohandatud välju sündmusi, OMS kogub neid. |
| Metaandmete | BaseManagedEntityId, ObjectStatus, OrganizationalUnit, ActiveDirectoryObjectSid, PhysicalProcessors, NetworkName, sordib, ForestDNSName, NetbiosComputerName, VirtualMachineName, LastInventoryDate, HostServerNameIsVirtualMachine, IP-aadress, NetbiosDomainName, LogicalProcessors, dnsnameWindows, DisplayName, DomainDnsName, ActiveDirectorySite, PrincipalName, OffsetInMinuteFromGreenwichTime |
| Jõudluse | ObjectName, CounterName, PerfmonInstanceName, PerformanceDataId, PerformanceSourceInternalID, SampleValue, TimeSampled, TimeAdded |
| Olek | StateChangeEventId, StateId, NewHealthState, OldHealthState, konteksti, TimeGenerated, TimeAdded, StateId2, BaseManagedEntityId, MonitorId, HealthState, LastModified, LastGreenAlertGenerated, DatabaseTimeModified |

## <a name="physical-security"></a>Füüsiline turvalisus

Log Analytics OMS teenuses on mehitatud Microsofti töötajad ja kõik tegevused on sisse logitud ja saab auditeerida. Teenuse töötab täielikult Azure ja Azure levinud engineering kriteeriumidele. Saate vaadata [Microsoft Azure'i turvalisuse ülevaade](http://download.microsoft.com/download/6/0/2/6028B1AE-4AEE-46CE-9187-641DA97FC1EE/Windows%20Azure%20Security%20Overview%20v1.01.pdf)lehel 18 füüsilise turvalisuse Azure varade üksikasjad. Füüsilise pääsuõigused kaitstud alad muudetakse ühe tööpäeva jooksul kõigile, kes pole enam OMS teenus, sh edastamine ja lõpetamise kohustusi. Saate lugeda globaalne füüsilise infrastruktuuri kasutame veebisaidil [Microsoft andmekeskuste](https://www.microsoft.com/en-us/server-cloud/cloud-os/global-datacenters.aspx).

## <a name="incident-management"></a>Sündmuse juhtimine

OMS on sündmuse juhtimine protsessi, et kõik Microsofti teenused kinni. Kokkuvõttes, saame:

- Kasutage jagatud vastutuse mudeli kui turvalisus kohustusi osa kuulub Microsoft ajal mõne osa kuulub kliendile
- Azure'i turvalisus juhtumite haldamine
  - Tuvasta juhtum juures on esimene märge juurdlust alustamiseks
  - Hinnake mõju ja tõsidust nõudmisel langeva vastuse meeskonna liikme juhtum. Põhineb tõendusmaterjalide kogumiseks, hindamise võib või ei pruugi täpsemaks laiendamine turvalisus vastuse meeskond.
  - Diagnoosimine juhtum turvalisus vastuse eksperdid juurdluse tehnilist või kohtuekspertiisi, tuvastamine piiramist, vähendamiseks ja lahendus strateegiad. Turvalisus meeskonnatöö arvab, et kliendiandmete muutuvad esitatud ebaseadusliku või volitamata isikuga, algab paralleelne kliendi juhtum teatis protsessi paralleelselt.  
  - Stabiliseerida ja taastada juhtum. Langeva vastuse meeskonnatöö loob taastamise kava probleemi leevendada. Kriisi piiramist juhiseid, nt karantiini kannatavas süsteemid võib ilmneda kohe ja paralleelselt diagnostika. Rohkem Termini kergendamise planeerida mis leiavad aset pärast kohe risk on möödas.  
  - Sulgege juhtum ja läbiviimine lahkamis. Langeva vastuse meeskonnatöö loob tapajärgse, mis kirjeldab juhtum, eesmärgiga muuta poliitika, toimingute ja protsesside sündmuse kordumist vältida üksikasju.
- Teavitage turvalisus juhtumite kliendid
  - Ulatust kannatavas kliendid ja anda igaüks, kellel mõjutab üksikasjalikku võimalikult teade
  - Looge esitada klientide üksikasjalikku piisavalt teavet, et nad saaksid sooritada oma lõpp uurimine ja mis tahes ajal mitte asjatult edasi lükata teavitamise lõppkasutajatele nende tehtud kohustuste teade.
  - Kinnitage ja deklareerida juhtum, vastavalt vajadusele.
  - Teavitage kliendid, kellel on mõistlik kohe ja mis tahes õigus- või vastavalt langeva teatis. Mis tahes viisil, mis valib Microsoft, sh e-posti toimetatakse teatis turvalisus juhtumite ühe või mitme kliendi administraatorid.
- Meeskonnatöö valmiduse ja koolitus
  - Microsofti töötajad vajalike turbe- ja teadlikkuse koolituse, mis aitab tuvastada ja võimaliku turvalisuse probleemist lõpuleviimiseks.  
  - Teenuses Microsoft Azure'i operaatorid on lisamine koolitus kohustused ümbritseva nende juurdepääsu tundliku loomuga süsteemide majutusteenuse kliendiandmete.
  - Microsofti turvalisus vastuse töötajad saavad oma ülesanded eriotstarbelisi koolitus


Mis tahes kliendi andmekadu, saame sellest iga kliendi ühe päeva jooksul. Kliendi andmete kaotsimineku kunagi ilmnes OMS abil. Lisaks soovime säilitada eksemplaride andmeid, mis on loodud ja geograafiliselt levitatakse.

Kuidas Microsoft vastaks turvalisus juhtumite kohta leiate lisateavet teemast [Microsoft Azure'i turvalisus vastuse pilveteenuses](https://gallery.technet.microsoft.com/Azure-Security-Response-in-dd18c678/file/150826/1/Microsoft Azure Security Response in the cloud.pdf).

## <a name="compliance"></a>Nõuetele vastavus

OMS tarkvara arendamise ja teenuse meeskonna turbe- ja juhtimise programmi toetab oma äri nõuetele ja järgib seaduste ja määruste, nagu on kirjeldatud [Microsoft Azure'i usalduskeskuses](https://azure.microsoft.com/support/trust-center/) ja [Microsoft usaldada keskmist nõuetele vastavus](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx). Kuidas OMS luuakse turbenõuetele, tuvastab turbemeetmed, haldab ja jälgib riske kirjeldatakse ka seal. Aastaintressiga me läbi poliitika standardid, toiminguid ja juhiseid.

Iga OMS arengu meeskonnaliige saab ametliku taotluse turvalisuse koolitus. Sees, kasutage versiooni kontrolli süsteem tarkvara arendamiseks. Iga tarkvara projekt on kaitstud versiooni kontrolli süsteem.

Microsoft on Turve ja nõuetele vastavus meeskond, mis juhib ja hindab Microsoft kõiki teenuseid. Teabe turvalisus tagavad meeskond üles ja nad ei ole seotud engineering osakondade, et arendada OMS. Funktsiooni turvalisus on oma haldusahelas ja läbiviimine sõltumatu hindamise toodete ja teenuste turvalisus ja nõuetele vastavuse tagamiseks.

Microsofti direktorite antakse teada ja Microsofti programmide aastaaruanne kohta kogu teabe turvalisust.

OMS tarkvara arendamise ja teenuse meeskonnatöö aktiivselt meeskondadel Microsoft Legal ja nõuetele vastavus ja muud valdkonna partneritega omandada mitmesuguseid kinnitamine.

## <a name="security-standards-certifications"></a>Turvalisuse standardid kinnitamine

Logige Analytics OMS praegu vastavad järgmistele turvalisus:

- [ISO/IEC 27001](http://www.iso.org/iso/home/standards/management-standards/iso27001.htm) ja [ISO/IEC 27018:2014](http://www.iso.org/iso/home/store/catalogue_tc/catalogue_detail.htm?csnumber=61498) nõuetele
- Makse kaart valdkonna (PCI nõuetele vastavuse) andmete turvalisus Standard (PCIDSS), PCI Security standardit.
- [Teenuse ettevõtte juhtelementide alusel 1 Tippige 1 ja SOC 2 tüüp 1](https://www.microsoft.com/en-us/TrustCenter/Compliance/SOC1-and-2) nõuetele
- Windowsi levinud matemaatika kriteeriumid
- Microsoft usaldusväärne arvutuste sertimine
- Nagu Azure teenuse komponendid, mis kasutab OMS kinni Azure nõuetele. Lugege veebisaidil [Microsoft usaldada keskmist nõuetele vastavus](https://www.microsoft.com/en-us/TrustCenter/Compliance/default.aspx).


## <a name="cloud-computing-security-data-flow"></a>Arvuti turvalisuse andmevoo Cloud
Järgmisel joonisel on pilv turbearhitektuur nimega teabe teie ettevõtte töötajad ja kuidas on tagatud, nagu on liigub Log Analytics teenusega lõpuks näinud olete OMS portaalis. Lisateabe saamiseks iga etapi järgmiselt skeem.

![Pildi OMS andmete kogumine ja turve](./media/log-analytics-security/log-analytics-security-diagram.png)

## <a name="1-sign-up-for-log-analytics-and-collect-data"></a>1. registreeruda Log analüüsi ja koguda andmeid

Ettevõtte Log Analytics andmeid saata, saate konfigureerida agentide Windows Azure'i virtuaalmasinates või OMS agentide Linuxi töötavate agentide. Kui kasutate Toiminguhalduri agentide, siis kasutate konfigureerimise toimingud konsooli konfigureerida neid. (Mis võib olla teie muude üksikkasutajate või rühm inimesi) ühe või mitme OMS kontode (OMS tööruumides) loomine ja agentide registreerida, kasutades ühte järgmistest kontodest.


- [Organisatsiooni ID](../active-directory/sign-up-organization.md)

- [Microsofti konto - Outlook, Office Live, MSN-i](http://www.microsoft.com/account/default.aspx)

Mõne OMS tööruum on, kus andmed on kogutud, kokkuvõtliku, analüüsida ja esitada. Tööruumi kasutatakse peamiselt, et sektsiooni andmed ja iga tööruum on kordumatu. Näiteks võite oma tootmisandmed hallatavate ühe OMS tööruumi ka teise tööruumi hallatavate testi andmete. Tööruumide aitavad administraatori kontroll kasutajate juurdepääsu andmetele. Iga tööruumi võib olla mitu kasutajakontot sellega seotud, ja igale kasutajakontole saab kasutada mitut OMS tööruumi. Andmekeskuse asukoha tööruume saate luua. Iga tööruum on kopeeritud muude andmekeskuste piirkonna, peamiselt OMS teenuse kättesaadavus.

Operations Manager, kui konfigureerimise viisard on lõpule jõudnud, iga rühma Toiminguhalduri loob ühenduse teenusega Log Analytics. Saate lisada arvutite viisardi abil seejärel valige haldus jaotises millises arvutis on lubatud teenuse andmeid saata. Muude agent iga ühendab turvaliselt OMS teenus.

Kõik suhtlemine ühendatud süsteemide ja Log Kasutusanalüüsi teenus on krüptitud.  Krüptimiseks kasutatakse TLS (HTTPS) protokolli.  Microsoft SDL processile järgneb Log Analytics ajakohasuse cryptographic Protokollid viimase uusimate tagamiseks.

Erinevat tüüpi agent kogub Log Analyticsi andmeid. Kogutud andmete tüüpi sõltub tüüpi lahendusi kasutada. Saate vaadata andmete kogumise [lisada Log Analytics lahenduste lahendusegaleriist](log-analytics-add-solutions.md)kokkuvõte. Lisaks saidikogumi üksikasjalikumat teavet on saadaval kõige lahendusi. Lahendus on eelmääratletud vaateid, log otsingupäringuid, andmete kogumise reeglid ja töötlemise loogika kogumile. Ainult administraatorid saavad kasutada Log Analytics importida lahenduse. Kui see on imporditud, teisaldatakse see Toiminguhalduri management serverid (kui kasutatakse) ja seejärel mis tahes agentide, et olete valinud. Pärast seda, agentide koguda andmeid.

## <a name="2-send-data-from-agents"></a>2 saata andmeid agentide

Kõigi agent registreerida on registreerimise võti ja vahel agent ja serdi autentimise ja SSL-i abil pordi 443 Log Kasutusanalüüsi teenus on loodud turvalist ühendust. OMS kasutab salajane poest saate luua ja hallata võtmed. Privaatvõtmete on pööratud iga 90 päeva ja salvestatakse Azure ja Azure toimingud, kes jälgivad range regulatiivsetele ja nõuetele vastavus tavad haldab.

Toiminguhalduri, tööruumi registreerida Log Analytics teenuse ja turvalise HTTPS ühendus on loodud Toiminguhalduri management serveri vahel.

Windows Azure'i virtuaalmasinates töötavate agentide kasutatakse lugeda diagnostika sündmuste Azure'i tabeli kirjutuskaitstud salvestusruumi klahvi.

Kui mis tahes agent ei saa suhelda mingil põhjusel teenusesse, kogutud andmete on talletatud kohalik ajutist vahemälu ja management server proovib andmeid iga 8 minuti 2 tundi. Operatsioonisüsteemi identimisteabesalve on kaitstud esindaja vahemällu talletatud andmetega. Kui teenus ei töödelda 2 tunni pärast, kas agentide järjekord andmed. Kui järjekorda saab täis, käivitab OMS andmetüübid, alustades tulemustega seotud andmete pukseerimine. Agent järjekorda on registrivõtme nii, et saate muuta seda, vajaduse korral. Kogutud andmed pakitakse kokku ja teenuse mööda kohapealse andmebaase, et seda ei saa lisada mis tahes laadi need saadetakse. Pärast saatmist kogutud andmete, eemaldatakse see vahemälu.

Eespool kirjeldatud andmeid oma agentide saadetakse SSL Microsoft Azure'i andmekeskuste. Soovi korral saate ExpressRoute andmete täiendava turvalisuse tagamiseks. ExpressRoute on võimalus otse ühenduse Azure'i WAN olemasoleva võrgu kaudu, nagu sildi mitme protokoll vahetamine (MPLS) VPN, võrgu teenuse pakkuja. Lisateavet leiate teemast [ExpressRoute](https://azure.microsoft.com/services/expressroute/).


## <a name="3-the-log-analytics-service-receives-and-processes-data"></a>3. Logi Analytics teenust saab ja töötleb

Log Kasutusanalüüsi teenus tagab, et sissetulevad andmed on usaldusväärsest allikast, serdid ja andmete terviklust Azure autentimise kontrollimine. Töötlemata toorandmetega talletatakse seejärel soovitud bloobimälu [Microsoft Azure](../storage/storage-introduction.md) Storage ja on krüptitud. Iga salvestusruumi Azure'i bloobimälu on siiski ainulaadsed klahvid kättesaadaval ainult selle kasutaja kogum. Mis on talletatud andmete tüüpi sõltub tüüpi lahendusi, mis olid imporditud ja kasutada andmete kogumine. Seejärel Logi Analytics teenuse töötleb toorandmetega Azure storage ajaveebi jaoks.

## <a name="4-use-log-analytics-to-access-the-data"></a>4. Kasutage Log Analytics andmetele juurdepääsuks

Te saate sisse logida Log Analytics OMS portaalis organisatsioonikonto või Microsofti konto, mille seadistasite varem abil. Kõik liiklus OMS portaali ja Log Analytics rakenduses OMS saadetakse turvalist HTTPS-kanalit. OMS portaali kasutamisel seansi ID luuakse kasutaja klient (veebibrauseri) ja kuni seanss lõpetatakse kohaliku vahemälu talletatakse andmeid. Kui lõpetada, kustutatakse vahemälu. Kliendipoolne küpsised, mis ei sisalda isikuandmeid, ei eemaldata automaatselt. Küpsised on tähistatud HTTPOnly ja on kinnitatud. Pärast kindlaksmääratud jõude, OMS portaali seanss lõpetatakse.

OMS portaali kasutamisel saate eksportida andmed CSV-faili ja teil on juurdepääs otsingu API-de kasutamine andmete. CSV eksport on piiratud 50 000 rida ekspordi kohta ja API andmed on piiratud 5000 ridade kohta otsida.

## <a name="next-steps"></a>Järgmised sammud

- [Alustamine Log Analytics](log-analytics-get-started.md) Lisateave Log Analytics ja saada ja töötab minutites.
