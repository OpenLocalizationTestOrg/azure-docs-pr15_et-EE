<properties
    pageTitle="Logige Analytics KKK | Microsoft Azure'i"
    description="Vastused korduma kippuvatele küsimustele Log Analytics teenuse kohta."
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

# <a name="log-analytics-faq"></a>Logige Analytics KKK

Selles Microsoft FAQ loend korduma kippuvatele küsimustele Log Analytics rakenduses Microsoft toimingud halduse komplekti (OMS) kohta. Kui teil on Log Analytics täiendavaid küsimusi, minge [Foorum](https://social.msdn.microsoft.com/Forums/azure/home?forum=opinsights) ja postitage oma küsimused. Keegi meie kogukonnast aitavad teil oma vastused. Kui palutakse tavaliselt küsimus, lisame selle selle artikli nii, et seda võib leida kiiresti ja kerge vaevaga.

## <a name="general"></a>Üldine

**Milliste kontrollitakse reklaami ja SQL-i Assessment lahendusi?**

V-SSE. Järgmine päring kuvatakse kõik kontrollid praegu läbi kirjeldus:

```
(Type=SQLAssessmentRecommendation OR Type=ADAssessmentRecommendation) | dedup RecommendationId | select FocusArea, ActionArea, Recommendation, Description | sort Type, FocusArea,ActionArea, Recommendation
```

Tulemused saab eksportida Excelisse seejärel edasiseks läbivaatamiseks.

* *Miks ma näen midagi muud kui Q: *OMS* rakenduses SCOM Administration? **

V: sõltuvalt mis Update Rollup, SCOM olete, võidakse kuvada sõlm *System Center Advisor*, *Töö ülevaated*või *Log Analytics*.

Teksti stringi värskendus *OMS* sisaldub halduspaketi, mis tuleb käsitsi importida. Järgige uusimate SCOM Update Rollup teabebaasi artikkel ja värskendage OMS konsooli *OMS* sõlme uusimate värskenduste kuvamiseks.

* *Q: on olemas ka *kohapealse* versiooni OMS? **

V: ei. Log Analytics töötleb ja talletab väga suurte andmehulkade. Nimega pilveteenus Log Analytics on võimalik skaala üles vajadusel ja vältida jõudluse oma keskkonnas.

Ka on pilv vahendid vaja Log Analytics taristu kursis ja töötab ja saada sagedased funktsiooni värskendused ja parandused.

## <a name="configuration"></a>Konfigureerimine
**Q. nimi, mida kasutatakse loe: Azure'i diagnostika WAD tabel/bloobimälu ümbrisest saab muuta?**  

V-SSE.  Ei, see praegu ei ole võimalik, kuid tulevaste väljalasete jaoks kavandatakse.

**Q. mis IP-aadresside teha OMS teenuste kasutamist? Kuidas tagada minu tulemüüri võimaldab ainult liikluse OMS teenuseid?**  

V-SSE. Log Kasutusanalüüsi teenus on ehitatud peal Azure ja vastu võtta lõpp-punktid IP-d, mis on [Microsoft Azure'i andmekeskuse IP-vahemikke](http://www.microsoft.com/download/details.aspx?id=41653).

Teenuse juurutuste tehakse, saate muuta OMS teenuste tegelik IP-aadressid. DNS-i nimed, kelle soovite lubada teie tulemüüri kaudu on avaldatud veebisaidil [puhverserveri ja tulemüüri sätete konfigureerimine Log Analytics](log-analytics-proxy-firewall.md).

**Q. ma kasutan ExpressRoute ühenduse Azure'i. Kas minu Log Analytics liikluse kasutada minu ExpressRoute ühendus?**  

V-SSE. Erinevat tüüpi ExpressRoute liikluse on kirjeldatud [ExpressRoute dokumentatsiooni](./expressroute/expressroute-faqs.md#supported-services).

Liikluse Log Analytics kasutab avaliku silmitsemine ExpressRoute ringi.

**Q. on lihtne ja lihtne viis Log Analytics olemasoleva tööruumi teisaldamiseks teise Log Analytics tööruumi/Azure tellimuse?**  Meil on mitme kliendi OMS tööruumid, mida me olid testimine ja trialing meie Azure'i tellimus ja liigutatakse tootmisele soovime teisaldage need oma Azure'i/OMS tellimus.  

V-SSE. Funktsiooni `Move-AzureRmResource` cmdlet-käsk annab teile mõne Log Analytics tööruumi ja automatiseerimise konto ühe Azure tellimuselt teisele liikumine. Lisateavet leiate teemast [Teisalda-AzureRmResource](http://msdn.microsoft.com/library/mt652516.aspx).

Selle muudatuse teha ka Azure'i portaalis.

Ei saa andmeid ühest Log Analytics tööruumi teisele liikumine või piirkonna, mis on talletatud Log Analytics andmete muutmine.

**Q: kuidas SCOM OMS lisada?**

V: ajakohastamine uusimate update rollup ja importimine halduse paketid võimaldab teil SCOM ühenduse Log Analytics.

Märkus Logi Analytics SCOM ühendus on ainult saadaval SCOM 2012 SP1 ja uuem versioon.

**Q: kuidas kinnitada, agent suudab suhelda Log Analytics?**

V: tagamaks, et agent saaksid suhelda OMS, minge: Juhtpaneel, Turve ja sätted **Microsoft Agenti jälgimine**.

Otsige vahekaardil **Azure'i Log Analytics (OMS)** roheline märge. Roheline märge ikooni kinnitab, et agent OMS teenuse suhelda.

Kollane hoiatusikoon tähendab agent on probleeme OMS suhtlus. Üks levinud põhjus on teenuse Microsoft Agenti jälgimine on peatatud ja tuleb taaskäivitada.

**Q: kuidas agendi kaudu suhtlemine Log Analytics peatada?**

V: SCOM, eemaldada arvuti OMS hallatavate loendist. See lõpetab kõik suhtlust SCOM selle agent. Ühendatud OMS otse töötajatele, te saate peatada nende OMS kaudu suhtlemine: Juhtpaneel, Turve ja sätted **Microsoft Agenti jälgimine**.
**Azure'i Log Analytics (OMS)**, klõpsake jaotises eemaldada kõik tööruumid, mis on loetletud.

## <a name="agent-data"></a>Agendi andmed

**Q. Kui palju andmeid saab saata – agent Logi Analytics? Kas on suurt hulka andmeid kliendi kohta?**  
V-SSE. Tasuta leping määrab iga päev ots 500 MB tööruumi kohta. Standard- ja premium lepinguid ei ole jooksul andmeid, mis on üles laaditud summa. Nimega pilveteenus Log Analytics rakenduses OMS mõeldud automaatselt skaala kuni pidet helitugevuse pärit kliendi – ka siis, kui see on TB iga päev.

See on väike jalajälg ning kas mõne lihtsa andmete pakkimine loodi Log Analytics agent. Ühte meie klientide tegelikult kirjutas ajaveebi meie agent ja kuidas muljet need olid katsete. Andmete maht sõltub klientide võimaldab lahendused. Saate üksikasjalikku teavet andmete maht ja väljasõit lahendus jaotises **kasutus** paani OMS ülevaade lehel.

Lisateabe saamiseks lugege [Kliendi ajaveebi](http://thoughtsonopsmgr.blogspot.com/2015/09/one-small-footprint-for-server-one.html) OMS agent madal andmed väiksemates vähem ruumi.

**Q. palju võrgu läbilaskevõime kasutatakse Microsoft Management Agent (MMA) Log Analytics andmete saatmisel?**

V-SSE. Läbilaskevõime on saadetud andmehulga funktsioon. Andmed on tihendatud, kuna see on saadetud võrgu kaudu.

**Q. Kui palju andmeid on saadetud agent kohta?**

V-SSE. See suuresti sõltub:

- teil on lubatud lahendused
- logide ja jõudluse hinnale kogutakse arv
- logide andmete maht

Tasuta hinnakirjad taseme on hea võimalus pardal mitu serverid ja hinnata tüüpilised andmete maht. Üldised kasutus on näidatud lehel **kasutus** .
Arvutites, on võimalik käivitada WireData agent, näete, saadetakse palju andmeid kasutada järgmist päringut:

```
Type=WireData (ProcessName="C:\\Program Files\\Microsoft Monitoring Agent\\Agent\\MonitoringHost.exe") (Direction=Outbound) | measure Sum(TotalBytes) by Computer
```



## <a name="next-steps"></a>Järgmised sammud

- [Alustamine Log Analytics](log-analytics-get-started.md) Lisateave Log Analytics ja saada ja töötab minutites.
