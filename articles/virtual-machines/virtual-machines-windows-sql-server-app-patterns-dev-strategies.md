<properties
    pageTitle="SQL serveri rakenduse mustreid VMs | Microsoft Azure'i"
    description="Selles artiklis antakse ülevaade rakenduse mustrid SQL Server Azure'i VMs. See loob arendajad ja lahenduse arhitektid aluse hea rakenduse arhitektuur ja kujundus."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="luisherring"
    manager="jhubbard"
    editor=""
    tags="azure-service-management,azure-resource-manager" />
<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="08/19/2016"
    ms.author="lvargas" />

# <a name="application-patterns-and-development-strategies-for-sql-server-in-azure-virtual-machines"></a>Rakenduse mustrite ja SQL Server Azure'i Virtuaalmasinates arengu strateegiad

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]


## <a name="summary"></a>Kokkuvõte:
Kindlaks teha, millised rakenduse mustri või mustritega kasutada Azure SQL-Server-põhiste rakenduste keskkond, mis on olulised kujundus otsus ja see nõuab ühtlase mõistmine, kuidas SQL serveri ja iga komponendi taristu Azure'i koos töötada. SQL Server Azure'i taristu teenused, saate hõlpsasti migreerida, hallata ja jälgida olemasoleva SQL serveri rakenduste ehitatud-on Windows Server Azure'i virtuaalmasinates.

Selles artiklis eesmärk on pakkuda arendajad ja lahenduse arhitektid hea rakenduse arhitektuur ja kujundus, mida nad saavad järgivad kui migreerimine olemasolevate rakenduste Azure, samuti uute rakenduste Azure.

Iga rakenduse mustri, leiate mõne kohapealse stsenaariumi, selle vastava pilveteenuse lubatud lahendus ja seotud tehnilised soovitused. Lisaks on artiklis käsitletakse Azure'i seotud arengu strateegiad nii, et saate kujundada rakenduste õigesti. Tõttu palju võimalike mustrid, on soovitatav arendajad ja arhitektide tuleb valida nende rakenduste ja kasutajate jaoks kõige paremini muster.

**Tehniline osaliste:** Luis Carlos Vargas heeringas, Madhan Arumugam Ramakrishnan

**Tehniline läbivaatajate:** Corey Sanders, Drew McDaniel, Narayan Annamalai, Nir Mashkowski, Sanjay Mishra, Silvano Coriani Stefan Schackow, Tim Hickey, Tim Wieman, Xin Jin

## <a name="introduction"></a>Sissejuhatus

Saate arendada mitut tüüpi n taseme rakendused, eraldades komponentide erinevates kihtidena erinevates arvutites, samuti eraldi komponendid. Näiteks saate lisada klientrakenduse ja business reeglite komponendid, ühe arvuti, veebi taseme ja andmed Accessi taseme komponendid mõnes muus arvutis ja Tagaandmebaasi taseme teise kohapeal. Seda tüüpi struktureerimine aitab iga taseme üksteisest eristada. Kui soovite muuta, kui andmed on pärit, te ei pea klient või web rakendust, kuid ainult andmed Accessi taseme komponendid muuta.

Tüüpilised *n-astme* rakenduse sisaldab esitluse esimese taseme, business taseme ja andmete taseme:


| Esimese taseme              | Kirjeldus                                                                                                                                                                     |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Esitluse** | *Esitluse esimese taseme* (web taseme, ees taseme) on kiht rakenduse suhtlevad kasutajad.                                                                      |
| **Business**     | Funktsiooni *business esimese taseme* (keskmisel taseme) on kiht, mis esitluse esimese taseme ja andmete taseme abil saate omavahel suhelda ja sisaldab põhifunktsioone süsteem. |
| **Andmete**         | *Andmete taseme* on põhiliselt server, mis talletab rakenduse andmeid (nt serveris, kus töötab SQL Server).                                                             |

Rakenduse kihid kirjeldada loogilise rühmitustega funktsioonid ja komponentide rakenduses; samas astme kirjeldatakse füüsilise jaotuse funktsioonid ja komponentide eraldi füüsilise serveri, võrkude, või remote asukohad. Rakenduse kihid võib asuda sama füüsilise arvuti (sama taseme) või võib jaotada eraldi arvutite (n-astme) ja komponentide iga kiht suhelda komponendid muude kihtidena täpselt määratletud liideste kaudu. Kui arvate, Termini tasandi nii, et füüsilise jaotuse mustrite nt kaheastmelise, kolme esimese ja n-astme. **2 rakendus mustri** sisaldab kahte rakenduste astme: rakenduse serveri ja andmebaasi. Otsene edastamine juhtub vahel rakenduse serveri ja andmebaasi. Rakenduste serveri sisaldab nii veebi-astme ja business taseme komponendid. **3 rakendus mustri**, on kolmest rakenduse tasemest: web serveri, rakenduse server, mis sisaldab business loogika taseme ja/või business taseme andmed Accessi komponendid, ja andmebaasi. Veebiserver ja andmebaasi server suhtlemine toimub rakenduste serveri aja. Rakenduse kihid ja astme kohta leiate üksikasjalikumat teavet teemast [Microsofti rakenduse arhitektuur juhend](https://msdn.microsoft.com/library/ff650706.aspx).

Enne alustamist, et selle artikli lugemist, peaks teil olema SQL Server ja Azure olulise mõisted teadmisi. Lisateavet leiate teemast [SQL Server raamatuid Online](https://msdn.microsoft.com/library/bb545450.aspx), [SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-server-iaas-overview.md) ja [Azure.com](https://azure.microsoft.com/).

Selles artiklis kirjeldatakse mitme rakenduse mustreid, mis võib olla sobib lihtne rakenduste kui ka keeruline ettevõtte rakendused. Enne üksikasjalikult iga mustri, soovitame, et te peaksite end olemasolevad andmed salvestusruumi teenuses Azure, nt [Azure Storage](../storage/storage-introduction.md), [Azure SQL-andmebaas](../sql-database/sql-database-technical-overview.md)ja [SQL serveri on Azure virtuaalse masina](virtual-machines-windows-sql-server-iaas-overview.md). Parim kujundus otsuseid oma rakenduste mõista milliseid andmeid salvestusteenus selgelt kasutamine.

### <a name="choose-sql-server-in-an-azure-virtual-machine-when"></a>Valige SQL serveri rakenduses on Azure virtuaalse masina, kui:

- Peate SQL serveri ja Windowsi juhtelemendile. Näiteks see võivad SQL serveri versioon, teisiti käigultparandused jõudluse konfigureerimine jne.

- Peate täielik ühilduvuse asutusesisese SQL serveri, kuid soovite teisaldada olemasolevaid rakendusi nagu Azure-on.

- Soovite kasutada võimaluste Azure keskkonnas, kuid Azure'i SQL-andmebaasi ei toeta kõik funktsioonid, mis rakenduse jaoks on vaja. See võib hõlmata järgmisi teemasid:

    - **Andmebaasi suurus**: selles artiklis värskendati ajal SQL-andmebaasi toetab kuni 1 TB andmete andmebaas. Kui teie rakendus nõuab rohkem kui 1 TB andmeid ja te ei soovi rakendada kohandatud sharding lahendusi, on soovitatav SQL serveri kasutamine on Azure virtuaalse masina. Värskeima teabe saamiseks vt [Skaleerimist välja Azure SQL-i andmebaasid](https://msdn.microsoft.com/library/azure/dn495641.aspx) ja [Azure SQL-i andmebaasi teenuse astme ja jõudlust](../sql-database/sql-database-service-tiers.md).
    - **HIPAA nõuetele vastavus**: tervishoiuteenuste kliendid ja sõltumatu tarkvara tarnijatele (tarkvaratoode) võiksid valida [SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-server-iaas-overview.md) asemel [Azure'i SQL-andmebaasi](../sql-database/sql-database-technical-overview.md) , kuna käsitletakse SQL serveri on Azure virtuaalse masina järgi HIPAA Business seostada lepingu (BAA). Nõuetele vastavuse kohta leiate teavet teemast [Microsoft Azure'i Trust Center: nõuetele vastavus](https://azure.microsoft.com/support/trust-center/compliance/).
    - **Eksemplari taseme funktsioonid**: sel ajal, SQL-andmebaasi ei toeta live väljaspool andmebaasi funktsioone (nt lingitud serverite, Agent töö, FileStream, teenuse ta jne.). Lisateavet leiate teemast [Azure SQL-i andmebaasi juhised ja piirangud](https://msdn.microsoft.com/library/azure/ff394102.aspx).

## <a name="1-tier-simple-single-virtual-machine"></a>1-astme (lihtne): ühe virtuaalse masina

Selle rakenduse muster, juurutamist oma rakenduse SQL Server ja andmebaas autonoomse virtuaalse masina Azure. Virtuaalne samasse arvutisse sisaldab oma kliendi-ja veebirakenduse, business komponendid, andmete Accessi kiht ja andmebaasiserveri. Esitluse, äri- ja andmete pääsukood eraldatakse loogiliselt, kuid füüsilise asuvad üks-server kohapeal. Enamik kliendid alustada selle rakenduse mustri ja seejärel need mastaapimiseks välja lisades web rollid või virtuaalmasinates oma süsteem.

See rakendus muster on kasulik järgmistel juhtudel:

- Soovite lihtsa migreerimine Azure'i platvormi hinnata, kas platvormi vastab teie taotlus nõuetele või mitte.

- Mida soovite säilitada rakenduse astme majutatud virtual samasse arvutisse sama Azure andmekeskuse vähendada latentsus astme vahel.

- Soovite kiiresti ettevalmistamise arendamise ja testimine keskkonnas lühikese aja jooksul.

- Mida soovite teha stresstestimise töökoormus taseme, kuid samal ajal te ei soovi oma ja säilitada paljud füüsilise masinad kogu aeg.

Järgmine diagramm näitab lihtne kohapealse stsenaarium ja selle lubatud pilve lahenduse ühe virtuaalse masina Azure juurutamise.

![1 rakendus muster](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728008.png)

Juurutamine business kiht (äriloogika ja andmete juurdepääs komponendid) klõpsake sama füüsilise taseme nimega esitluse kiht saate maksimeerimine rakenduse jõudluse tagamiseks juhul, kui peate kasutama eraldi taseme tõttu skaleeritavus või turberühma probleemid.

Kuna see on väga levinud mustri alustada, võib olla teile kasulik järgmises artiklis migratsiooni andmete teisaldamiseks oma SQL serveri VM: [on Azure VM SQL serveri andmebaasi migreerimine](virtual-machines-windows-migrate-sql.md).

## <a name="3-tier-simple-multiple-virtual-machines"></a>3-tasandi (lihtne): mitme virtuaalmasinates

Selle rakenduse muster, juurutamist 3-tasandi rakenduste Azure paigutades iga taseme rakenduse erinevate virtuaalse masina. See pakub lihtne skaala üles ja skaala-out stsenaariumid paindlik. Kui üks virtuaalse masina sisaldab oma kliendi-ja veebirakenduse, teine majutab oma äri komponendid ja teine majutab andmebaasiserver.

See rakendus muster on kasulik järgmistel juhtudel:

- Soovite keerukate andmebaasirakendusi Azure'i Virtuaalmasinates migreerimine.

- Soovite erinevates astme eri piirkondade töötaks. Näiteks võib ühiskasutuse mitme piirkonna aruandluseks juurutatud andmebaasid.

- Soovite teisaldada ettevõtte rakenduste kohapealse virtualiseeritud platvormid Azure'i Virtuaalmasinates. Üksikasjalik arutelu ettevõtte rakendused, vaadake teemat [mis on Enterprise rakenduse](https://msdn.microsoft.com/library/aa267045.aspx).

- Soovite kiiresti ettevalmistamise arendamise ja testimine keskkonnas lühikese aja jooksul.

- Soovite sooritada stresstestimise töökoormus taseme, aga samal ajal te ei soovi oma ja säilitada paljud füüsilise masinad kogu aeg.

Järgmine diagramm näitab, kuidas saate lisada mõne lihtsa 3 rakendus Azure'i paigutades iga taseme rakenduse erinevate virtuaalse masina.

![3 rakendus muster](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728009.png)

Selle rakenduse muster, on ainult üks virtuaalse masina (VM) iga taseme. Kui teil on mitu VMs Azure, soovitame häälestamine virtuaalse võrgus. [Azure'i virtuaalse võrgu](../virtual-network/virtual-networks-overview.md) loob usaldusväärse turvalisus äärist ja võimaldab VMs privaatne IP-aadress kaudu omavahel suhelda. Lisaks alati veenduge, et kõik Interneti-ühendused minna ainult esitluse esimese taseme. Kui pärast selle rakenduse mustri, hallata juurdepääsu kontrollimiseks võrgu rühma reeglid. Lisateavet leiate teemast [Luba välise juurdepääsu oma VM Azure portaalis](virtual-machines-windows-nsg-quickstart-portal.md).

Skemaatilise diagrammi, saab Interneti-protokollide TCP, UDP, HTTP või HTTPS-i.

>[AZURE.NOTE] Azure virtuaalse võrgu häälestamine on tasuta. Siiski saate eest VPN-lüüsi, mis ühendab kohapealse. Selles tasuta põhineb aeg, et ühendus on ettevalmistatud ja saadaval.

## <a name="2-tier-and-3-tier-with-presentation-tier-scale-out"></a>taseme 2 ja 3-tasandi esitluse esimese taseme skaala-out

Selle rakenduse muster, juurutamist taseme 2 või 3-tasandi andmebaasirakenduse Azure'i Virtuaalmasinates paigutades iga taseme rakenduse erinevate virtuaalse masina. Lisaks muudate esitluse taseme tõttu suurema hulga kliendi sissetulevad taotlused.

See rakendus muster on kasulik järgmistel juhtudel:

- Soovite teisaldada ettevõtte rakenduste kohapealse virtualiseeritud platvormid Azure'i Virtuaalmasinates.

- Soovite esitluse taseme suurema hulga kliendi sissetulevad taotlused tõttu välja skaala.

- Soovite kiiresti ettevalmistamise arendamise ja testimine keskkonnas lühikese aja jooksul.

- Mida soovite teha stresstestimise töökoormus taseme, kuid samal ajal te ei soovi oma ja säilitada paljud füüsilise masinad kogu aeg.

- Mida soovite oma taristu keskkonnas, kus saate vajadusel üles skaala.

Järgmine diagramm näitab, kuidas saate lisada rakenduse astme mitme virtuaalmasinates Azure skaala läbi esitluse taseme tõttu suurema hulga kliendi sissetulevad taotlused. Nagu joonisel näha, Azure'i laadi koormusetasakaalustusteenuse vastutab levitamise liikluse üle mitme virtuaalmasinates ja ka kindlaks teha, millised veebiserver ühenduse loomiseks. Teil on mitu eksemplari web serverid taha laadi koormusetasakaalustusteenuse tagab esitluse esimese taseme kõrge olemasolu.

![Muster - esitluse esimese taseme skaala läbi](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728010.png)

### <a name="best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier"></a>Taseme 2, 3-tasandi või n-astme mustreid, mis on mitu VMs ühe taseme head tavad

Soovitatav on, et asetate virtuaalmasinates, mis kuuluvad sama taseme sama pilveteenuses ja sama kättesaadavus seadmine. Näiteks paigutada web serverite kogum **CloudService1** ja **AvailabilitySet1** ja andmebaasi serverite **CloudService2** ja **AvailabilitySet2**kogum. Saadavus Azure seadmine võimaldab teil koht kõrge-saadavus sõlmed eraldi viga domeenid ja täiendamine domeenid.

Ära kasutada mõne teise astme VM mitu eksemplari, peate Azure'i laadi koormusetasakaalustusteenuse konfigureerimine vahel rakenduse astme. Iga taseme laadi koormusetasakaalustusteenuse konfigureerimine, et luua koormusetasakaalustusega lõpp-punkti iga taseme VMs eraldi. Teatud taseme, luua esmalt VMs sama pilveteenuses. See tagab, et neil on sama avaliku virtuaalse IP-aadress. Järgmisena looge lõpp ühele virtuaalmasinates, klõpsake selle taseme. Seejärel määrata teiste jaoks koormusetasakaalustuseks teise astme virtuaalmasinates sama lõpp-punkti. Luues koormusetasakaalustusega kogum, levitamine liikluse üle mitme virtuaalmasinates ja ka luba laadimine mis sõlm ühenduse kirjutamata VM sõlm nurjub määratlemiseks koormusetasakaalustusteenuse. Näiteks on mitu eksemplari web serverid taha laadi koormusetasakaalustusteenuse tagab esitluse esimese taseme kõrge olemasolu.

Hea tava, alati veenduge, et kõik Interneti-ühendused esmalt minge esitluse esimese taseme. Esitluse kiht pääseb business taseme ja seejärel business taseme pääseb andmete taseme. Lisateavet selle kohta, kuidas lubada juurdepääsu esitluse kiht, lugege teemat [Luba välise juurdepääsu oma VM Azure portaalis](virtual-machines-windows-nsg-quickstart-portal.md).

Pange tähele, et laadimine Azure koormusetasakaalustusteenuse toimib sarnaselt laadimine soolise asutusesiseses keskkonnas. Lisateabe saamiseks vt [koormusetasakaalustuseks Azure'i infrastruktuuri teenuste jaoks](virtual-machines-windows-load-balance.md).

Lisaks soovitame seadistada privaatvõrk jaoks oma virtuaalmasinates Azure virtuaalse võrgu kaudu. See võimaldab neid privaatne IP-aadress kaudu omavahel suhelda. Lisateavet leiate teemast [Azure virtuaalse võrgu](../virtual-network/virtual-networks-overview.md).

## <a name="2-tier-and-3-tier-with-business-tier-scale-out"></a>taseme 2 ja 3-tasandi business taseme skaala-out

Selle rakenduse muster, juurutamist taseme 2 või 3-tasandi andmebaasirakenduse Azure'i Virtuaalmasinates paigutades iga taseme rakenduse erinevate virtuaalse masina. Lisaks võite levitamine rakenduse serveri komponentide mitme virtuaalmasinates rakenduse keerukuse tõttu.

See rakendus muster on kasulik järgmistel juhtudel:

- Soovite teisaldada ettevõtte rakenduste kohapealse virtualiseeritud platvormid Azure'i Virtuaalmasinates.

- Soovite levitada rakenduse serveri komponentide mitme virtuaalmasinates rakenduse keerukuse tõttu.

- Soovite teisaldada business loogika raske kohapealse LOB (joon, business) rakenduste Azure'i Virtuaalmasinates. LOB rakendused on kriitiliste arvuti rakendusi, mis on olulised töötab ettevõte, nt raamatupidamine, Personalihaldus (HR), palgad, pakkumise juhtimise ja rakenduste kavandamine ressursside kogumist.

- Soovite kiiresti ettevalmistamise arendamise ja testimine keskkonnas lühikese aja jooksul.

- Mida soovite teha stresstestimise töökoormus taseme, kuid samal ajal te ei soovi oma ja säilitada paljud füüsilise masinad kogu aeg.

- Mida soovite oma taristu keskkonnas, kus saate vajadusel üles skaala.

Järgmine diagramm näitab on kohapealse stsenaarium ja selle pilve lubatud lahendus. Selle stsenaariumi korral paigutada rakenduse astme mitme virtuaalmasinates Azure skaala läbi business taseme kohta, mis sisaldab business loogika taseme- ja andmed Accessi komponendid. Nagu joonisel näha, Azure'i laadi koormusetasakaalustusteenuse vastutab levitamise liikluse üle mitme virtuaalmasinates ja ka kindlaks teha, millised veebiserver ühenduse loomiseks. Kas teil on mitu eksemplari rakenduste serverid taha laadi koormusetasakaalustusteenuse tagab business taseme kõrge kättesaadavus. Lisateavet leiate teemast [head tavad taseme 2, 3-tasandi või n rakendus mustrite, mis on mitu virtuaalmasinates ühe astme](#best-practices-for-2-tier-3-tier-or-n-tier-patterns-that-have-multiple-vms-in-one-tier).

![Rakenduse business taseme skaala läbi mustriga](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728011.png)

## <a name="2-tier-and-3-tier-with-presentation-and-business-tiers-scale-out-and-hadr"></a>taseme 2 ja 3-tasandi esitluse ja business astme skaala välja ning HADR

Selle rakenduse muster, juurutamist taseme 2 või 3-tasandi andmebaasirakenduse Azure'i Virtuaalmasinates, jagades esitluse esimese taseme (veebiserver) ja business esimese taseme (rakenduse server) komponendid mitme virtuaalmasinates. Lisaks saate rakendada kõrge-saadavus ja katastroofi taastamine lahendused Azure'i virtuaalmasinates teie andmebaasidest.

See rakendus muster on kasulik järgmistel juhtudel:

- Soovite teisaldada ettevõtte rakenduste asutusesisesest virtualiseeritud platvormid Azure rakendades SQL serveri kättesaadavuse ja funktsioonid.

- Soovite esitluse taseme- ja business taseme suurema hulga kliendi sissetulevad taotlused ja rakenduse keerukuse tõttu välja skaala.

- Soovite kiiresti ettevalmistamise arendamise ja testimine keskkonnas lühikese aja jooksul.

- Mida soovite teha stresstestimise töökoormus taseme, kuid samal ajal te ei soovi oma ja säilitada paljud füüsilise masinad kogu aeg.

- Mida soovite oma taristu keskkonnas, kus saate vajadusel üles skaala.

Järgmine diagramm näitab on kohapealse stsenaarium ja selle pilve lubatud lahendus. Selle stsenaariumi korral muudate esitluse taseme- ja mitme virtuaalmasinates Azure business taseme komponendid. Lisaks saate rakendada kõrge kättesaadavus ja katastroofi taastamine (HADR) võtteid SQL serveri andmebaasi Azure.

Mitme eksemplari rakendus töötab eri VMs veenduge, et, et olete laadi tasakaalustamiseks taotlusi kogu neid. Kui teil on mitu virtuaalmasinates, peate tegema kindel, et kõik teie VMs on juurdepääsetav ja käivitatud ühel ajal. Kui konfigureerite koormusetasakaalustuseks, Azure laadimine koormusetasakaalustusteenuse jälitab VMs seisundi ja suunab sissetulevate kõnede terve toimiva VM sõlmi õigesti. Koormusetasakaalustuseks, on virtuaalmasinates häälestamise kohta leiate teavet teemast [laadimine jaoks Azure taristu teenused](virtual-machines-windows-load-balance.md). Heliprobleemide mitmes eksemplaris web ja rakenduse serveri taha laadi koormusetasakaalustusteenuse tagab kõrge olemasolu esitluse ja business astme.

![Skaala-out ja kõrge-saadavus](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728012.png)

### <a name="best-practices-for-application-patterns-requiring-sql-hadr"></a>Rakenduse mustrid nõudva SQL-i HADR head tavad

SQL serveri kättesaadavuse ja katastroofi taastamine lahendused Azure'i Virtuaalmasinates häälestamisel virtuaalse võrgu jaoks oma virtuaalmasinates [Azure virtuaalse](../virtual-network/virtual-networks-overview.md) võrgu häälestamine on kohustuslikud.  Virtuaalmasinates virtuaalse võrgustikus on isegi pärast teenuse tööseisakute ühed privaatne IP-aadress, seega saate vältida nõutavaid DNS-i nimelahendus värskenduse aja. Lisaks virtuaalse võrgu võimaldab teil laiendada kohapealse võrgu Azure ja loob usaldusväärse turvalisus äärist. Näiteks kui teie taotlus on ettevõtte Domeen piirangud (nt Windowsi autentimine, Active Directory), [Azure virtuaalse võrgu](../virtual-network/virtual-networks-overview.md) häälestamiseks on vaja.

Enamik klientidele, kes töötavad Azure kood, on Azure hoida nii põhi- ja koopiad.

Teavet ja õpetused kättesaadavuse ja katastroofi taastamise meetodite abil leiate teemast [kättesaadavuse ja SQL Server Azure'i Virtuaalmasinates avariitaastet](virtual-machines-windows-sql-high-availability-dr.md).

## <a name="2-tier-and-3-tier-using-azure-vms-and-cloud-services"></a>taseme 2 ja 3-tasandi Azure VMs ja pilveteenustega abil

Selle rakenduse muster, juurutamist Azure taseme 2 või 3-tasandi rakenduse abil nii [Azure'i pilveteenustega](../cloud-services/cloud-services-choose-me.md#tellmecs) (web ja töötaja rollid - platvormi teenust (PaaS)) ja [Azure'i Virtuaalmasinates](virtual-machines-windows-about.md) (infrastruktuuri teenus (IaaS)). [Azure'i pilveteenuste](https://azure.microsoft.com/documentation/services/cloud-services/) kasutamise esitluse esimese taseme/business taseme- ja SQL Server [Azure'i Virtuaalmasinates](virtual-machines-windows-about.md) andmete kiht on kasulik enamik töötavad Azure rakendused. Põhjus on võttes Arvuta eksemplari, mis töötab pilveteenustega pakub on lihtne juhtimine, juurutus, jälgimine ja skaala välja.

Pilveteenustega, Azure'i infrastruktuuri säilitab teie eest, sooritab hooldust, plaastrid opsüsteemide ja proovib taastamine teenuseid ja riistvara tõrkeid. Kui teie rakendus peab skaala-out, automaatne ja käsitsi skaala välja suvandid on saadaval pilvepõhise teenuse projekti suurendades või vähendades eksemplarid või virtuaalmasinates, mida kasutatakse rakenduse arvu. Lisaks saate juurutada rakendust pilvepõhise teenuse Project Azure kohapealse Visual Studio abil.

Kokkuvõttes, kui te ei soovi oma olulisel haldustoiminguid esitluse/ettevõtetele taseme ja rakenduse ei nõua keerukate konfiguratsiooni tarkvara või operatsioonisüsteemi, kasutage Azure'i pilveteenustega. Kui Azure'i SQL-andmebaasi ei toeta kõik funktsioonid, mida otsite, kasutage SQL serveri on Azure virtuaalse masina andmete taseme. Rakendus Azure'i pilveteenustega ja andmete salvestamiseks Azure'i Virtuaalmasinates ühendab endas mõlemad teenused. Üksikasjalikku võrdlust, jaotisest selles teemas [võrdlemine arengu strateegiad Azure](#comparing-web-development-strategies-in-azure).

Selle rakenduse muster, sisaldab esitluse esimese taseme web rolli, mis pilveteenustega komponent töötab Azure täitmise keskkonnas ja see on kohandatud veebirakenduse programmeerimine nii toetatud IIS-i ja ASP.net-i. Ettevõtetele või kirjutamata taseme sisaldab töötaja rolli, mis pilveteenustega komponent töötab Azure täitmise keskkonnas ja see on kasulik generalized arengu ja võib teha töötlemiseks tausta web rolli. Andmebaasi taseme asub Azure SQL serveri virtuaalse masina. Esitluse taseme- ja andmebaasi taseme suhtlemine toimub otse või üle business taseme – töötaja rolli komponendid.

See rakendus muster on kasulik järgmistel juhtudel:

- Soovite teisaldada ettevõtte rakenduste asutusesisesest virtualiseeritud platvormid Azure rakendades SQL serveri kättesaadavuse ja funktsioonid.

- Mida soovite oma taristu keskkonnas, kus saate vajadusel üles skaala.

- Azure'i SQL-andmebaasi ei toeta kõik funktsioonid, mida teie rakendus või andmebaasi vajab.

- Mida soovite teha stresstestimise töökoormus taseme, kuid samal ajal te ei soovi oma ja säilitada paljud füüsilise masinad kogu aeg.

Järgmine diagramm näitab on kohapealse stsenaarium ja selle pilve lubatud lahendus. Selle stsenaariumi korral, mille saab paigutada esitluse taseme web roll business teise töötaja roll, kuid andmed taseme Azure'i virtuaalmasinates. Töötab mitu eksemplari esitluse esimese taseme erinevate web rollid tagab kogu neid laadimiseks. Kui ühendate Azure'i pilveteenustega Azure'i Virtuaalmasinates, soovitame seadistada [Azure virtuaalse võrgu](../virtual-network/virtual-networks-overview.md) samuti. [Azure'i virtuaalse võrgu](../virtual-network/virtual-networks-overview.md), võib olla ühed ja püsivate privaatne IP-aadresside sees sama pilveteenuses pilveteenuses. Kui olete oma virtuaalmasinates virtuaalse võrgu määramiseks ja pilveteenused, Alustuseks suhtlemine omavahel üle privaatne IP-aadress. Lisaks on sama [Azure virtuaalse võrgu](../virtual-network/virtual-networks-overview.md) virtuaalmasinates ja Azure web/töötaja rollide pakub madal latentsus ja turvalisemad Ühenduvus. Lisateavet leiate teemast [mis on mõnda pilveteenusesse](../cloud-services/cloud-services-choose-me.md).

Nagu joonisel näha, Azure'i laadi koormusetasakaalustusteenuse jaotab liikluse üle mitme virtuaalmasinates ja ka määrab, millised veebiserverisse või rakenduste serveri ühenduse loomiseks. Heliprobleemide mitmes eksemplaris web ja rakenduse serveri taha laadi koormusetasakaalustusteenuse tagab esitluse esimese taseme ja business taseme kõrge kättesaadavus. Lisateabe saamiseks vt [rakenduse mustrid nõudva HADR SQL-i põhitõed](#best-practices-for-application-patterns-requiring-sql-hadr).

![Rakenduse mustrid pilveteenustega](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728013.png)

Teine võimalus rakendada selle rakenduse mustri on kasutada konsolideeritud web rolli, mis sisaldab nii esitluse esimese taseme business taseme komponendid, nagu on näidatud järgmisel joonisel. See rakendus muster on kasulik rakendusi, mis nõuavad stateful kujundus. Kuna Azure pakub kodakondsuseta Arvuta sõlmed veebi ja töötaja rollid, soovitame juurutada loogika talletamiseks seansi olek, kasutades ühte järgmistest tehnoloogiad: [Azure'i vahemällu](https://azure.microsoft.com/documentation/services/redis-cache/), [Azure'i Tabelimälu](../storage/storage-dotnet-how-to-use-tables.md) või [Azure'i SQL-andmebaasi](../sql-database/sql-database-technical-overview.md).

![Rakenduse mustrid pilveteenustega](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728014.png)

## <a name="pattern-with-azure-vms-azure-sql-database-and-azure-app-service-web-apps"></a>Mustri Azure VMs, Azure'i SQL-andmebaas ja Azure'i rakendust Service (Web Apps)

Selle rakenduse mustri peamine eesmärk on näete, kuidas ühendada Azure'i infrastruktuuri teenus (IaaS) komponendid Azure'i platvormi-kui-a-service komponendid (PaaS) teie lahendus. See muster keskendub Azure'i SQL-andmebaasi relatsiooniliste andmete salvestamine. Ei sisalda SQL Server Azure'i virtual kohapeal, mis on osa Azure'i infrastruktuuri teenuse pakkumine.

Selle rakenduse muster, juurutamist andmebaasi rakenduse Azure esitluse ja business astme sama virtual kohapeal ja juurdepääs andmebaasi Azure SQL-andmebaas (SQL-andmebaasi) serverites. Esitluse esimese taseme saate rakendada, kasutades traditsiooniline IIS-i-põhine web lahendusi. Või saate rakendada ühendatud esitluses ja business taseme [Azure'i veebirakenduste](https://azure.microsoft.com/documentation/services/app-service/web/)abil.

See rakendus muster on kasulik järgmistel juhtudel:

- Teil on juba olemasoleva konfigureeritud Azure SQL-andmebaasi serveri ja soovite testida rakenduse kiiresti.

- Mida soovite testida võimaluste Azure keskkonnas.

- Soovite kiiresti ettevalmistamise arendamise ja testimine keskkonnas lühikese aja jooksul.

- Oma ettevõtte loogika ja andmed Accessi komponendid võivad olla iseseisev web rakenduses.

Järgmine diagramm näitab on kohapealse stsenaarium ja selle pilve lubatud lahendus. Selle stsenaariumi korral, mille saab paigutada rakenduse astme ühe virtuaalse masina Azure'i SQL-andmebaasi andmete Azure ja Accessi rakenduses.

![Kombineeritud muster](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728015.png)

Kui valite rakendada kombineeritud web ja rakenduse taseme Azure'i veebirakenduste abil, soovitame hoida Kesk-astme või rakenduse taseme dünaamiliselt Lingitav teegid (dll) veebirakenduse kontekstis.

Lisaks läbi käesoleva artikli lõpus jaotises [võrdlemine web arengu strateegiad Azure](#comparing-web-development-strategies-in-azure) soovitustele Lisateavet programmeerimisega meetodite abil.

## <a name="n-tier-hybrid-application-pattern"></a>N-astme hübriid muster

N-astme hübriid muster, saate rakendada rakenduse mitmetasandiline kohapealse ja Azure vahel. Seetõttu loote paindlik ja korduvkasutatava hübriid süsteemi, mille saate muuta või lisada teatud taseme soovitud tasandi muutmata. Teie ettevõtte võrguga pilveteenusesse laiendada kasutate [Azure virtuaalse võrgu](../virtual-network/virtual-networks-overview.md) teenust.

See hübriid rakenduse muster on kasulik järgmistel juhtudel:

- Soovite, et luua rakendusi, mis töötavad osaliselt pilves ja osaliselt kohapealse.

- Mida soovite migreerida mõned või kõik elemendid kohapealse taotluse pilveteenusesse.

- Soovite teisaldada ettevõtte rakenduste kohapealse virtualiseeritud platvormid Azure.

- Mida soovite oma taristu keskkonnas, kus saate vajadusel üles skaala.

- Soovite kiiresti ettevalmistamise arendamise ja testimine keskkonnas lühikese aja jooksul.

- Soovite tõhus viis varukoopiad ettevõte andmebaasi rakenduste tegemiseks.

Järgmine diagramm näitab n-astme hübriid rakenduse mustri, mis ulatub üle asutusesisese ja Azure. Nagu on näidatud joonisel, sisaldab taristu kohapealse [Active Directory domeeniteenused](https://technet.microsoft.com/library/hh831484.aspx) domeenikontrolleri toetamiseks kasutaja autentimise ja luba. Pange tähele, et diagramm näitab stsenaarium, kus osa andmete taseme live kohapealse data Centeri kaudu tuleks osa taseme andmete reaalajas Azure. Sõltuvalt rakenduse vajadustele, saate rakendada mitut hübriid stsenaariumid. Näiteks võib hoida esitluse taseme- ja business taseme asutusesiseses keskkonnas, kuid andmed taseme Azure.

![N rakendus muster](./media/virtual-machines-windows-sql-server-app-patterns-dev-strategies/IC728016.png)

Azure'i, saate kasutada Active Directory autonoomse cloud directory ettevõtte või ka saate olemasoleva kohapealse Active Directory integreerimine [Azure Active Directory](https://azure.microsoft.com/documentation/services/active-directory/). Nagu joonisel näha, business taseme komponendid saate juurdepääsu mitmest andmeallikast, näiteks [SQL](virtual-machines-windows-sql-server-iaas-overview.md) server Azure'i privaatne sisemise IP-aadress, asutusesisese SQL Server [Azure'i virtuaalse võrgu](../virtual-network/virtual-networks-overview.md)kaudu või [SQL-andmebaasi](../sql-database/sql-database-technical-overview.md) .NET Framework andmete pakkuja tehnoloogiate kaudu. Selle diagrammi Azure SQL-andmebaas on valikuline andmete salvestusruumi teenus.

N-astme hübriid rakenduse muster, saate rakendada järgmised töövoo määratud järjestuses:

1. Määratlege ettevõtte andmebaasirakendusi, mille soovite teisaldada pilve, kasutades [Microsoft hindamise ja kavandamise (kaarti) tööriistakomplekt](http://microsoft.com/map). KAARDI tööriistakomplekt kogutakse varude ja jõudluse andmete kaalute Virtualization arvutitest ning annab soovitusi võimsus ja hindamise kavandamine.

1. Ressursid ja konfigureerimine vajalik Azure'i platvormi, nt salvestusruumi kontod ja virtuaalmasinates kavandamine.

1. Saate häälestada võrguühendus kohapealse ettevõtte võrgus ja [Azure virtuaalse võrgu](../virtual-network/virtual-networks-overview.md)vahel. Ettevõtte võrgus asutusesisese ja Azure virtuaalse masina vahelise ühenduse häälestada, kasutage ühte järgmised kaks võimalust:

    1. Luua ühendust kohapealse ja Azure kaudu avaliku lõpp-punkti virtual arvutisse Azure. See meetod pakub on lihtne häälestamise ja võimaldab teil kasutada SQL serveri autentimine virtual arvuti. Lisaks häälestada oma võrgu turvalisuse rühma reeglid määrata avaliku liikluse VM. Lisateavet leiate teemast [Luba välise juurdepääsu oma VM Azure portaalis](virtual-machines-windows-nsg-quickstart-portal.md).

    1. Kohapealse ja Azure Azure virtuaalse privaatse võrgu (VPN) tunneliga kaudu ühendust luua. See meetod võimaldab teil domeeni poliitika Azure virtuaalse masina laiendada. Lisaks saate tulemüüri reeglid häälestada ja kasutada arvuti virtuaalne Windowsi autentimist. Azure'i toetab praegu, turvaline-saidilt VPN-i ja punkti saidi VPN-ühendused:

        - Turvaline saitide abil saate luua võrguühendus kohapealse võrgu ja virtuaalse võrgu Azure. Soovitatav on andmete keskmist kohapealse keskkonna ühenduse Azure'i.

        - Turvaline punkti saidi abil saate luua võrguühendus Azure virtuaalse võrgu ja üksikute arvutid suvalist kohta. Soovitatav on enamasti arendamise ja testi.

    Azure SQL serveriga ühenduse loomise kohta leiate teavet teemast [ühenduse loomine abil SQL serveri virtuaalse masina Azure](virtual-machines-windows-classic-sql-connect.md).

1. Ajastatud tööd ja teatised, mis kohapealse varundada Azure virtuaalse masina ketta häälestamine. Lisateabe saamiseks vt [SQL serveri varundus ja taaste teenusega Azure'i bloobimälu salvestusruumi](https://msdn.microsoft.com/library/jj919148.aspx) ja [varundus ja taaste SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-backup-recovery.md).

1. Sõltuvalt rakenduse vajadustele, saate rakendada ühte järgmised kolm Levinumad stsenaariumid:

    1. Saate hoida oma veebiserverisse, rakenduse server, ja tundlik andmete andmebaasi serveri Azure samas hoiate tundliku loomuga andmete asutusesisene.

    1. Saate hoida oma veebiserverisse ja rakenduste serveri kohapealse tuleks andmebaasi serveri Azure virtuaalse masina.

    1. Saate hoida oma andmebaasi server, veebiserver ning rakenduste serveri kohapealse tuleks andmebaasi koopiad jätma Azure'i virtuaalmasinates. See säte võimaldab kohapealsed web serverid või andmebaasi koopiad Azure juurdepääsu aruandlusrakendustes. Seega võite saavutada töökoormus kohapealse andmebaasi langetada. Soovitame, et rakendate selle stsenaariumi jaoks raske lugeda töökoormus ja arengu eesmärgil. Azure'i andmebaasi koopiad loomise kohta leiate teavet teemast Kättesaadavusrühmad [kõrge-saadavus](virtual-machines-windows-sql-high-availability-dr.md)ja SQL Server Azure'i Virtuaalmasinates katastroofiabi.

## <a name="comparing-web-development-strategies-in-azure"></a>Web arengu strateegiad Azure võrdlus

Rakendada ja mitmekihilise Azure SQL serveri põhise rakenduse juurutamine, saate kasutada ühte järgmistest kaks programmeerimise:

- Seadistada Azure ja Accessi andmebaase SQL Server Azure'i Virtuaalmasinates traditsiooniline veebiserverisse (IIS - Internet Information Services).

- Rakendada ja juurutada pilveteenus Azure. Seejärel veenduge, et see pilveteenuses on juurdepääs andmebaase SQL Server Azure'i virtuaalse masinad. Pilveteenus võib sisaldada mitut veebi ja töötaja rollid.

Järgmises tabelis leiate Azure'i pilveteenustega ja veebirakenduste Azure SQL Server Azure'i Virtuaalmasinates suhtes traditsiooniline web arenduse võrdlus. Tabel sisaldab Azure'i veebirakenduste, et see on võimalik kasutada SQL Server Azure'i VM andmeallikana Azure Web Apps kaudu oma avaliku virtuaalse IP-aadress või DNS-i nimi.

||Traditsiooniline veebi arengut Azure'i Virtuaalmasinates|Azure pilveteenused|Web Hosting Azure Web Apps|
|---|---|---|---|
|**Rakenduse migreerimine kohapealsesse**|Olemasoleva taotluste-on.|Rakenduste vaja veebi ja töötaja rollid.|Olemasoleva taotluste – kuid sobib iseseisev veebirakenduste ja veebiteenuseid, mis nõuavad kiirülevaate skaleeritavus.|
|**Loomine ja juurutamine**|Visual Studio, WebMatrix, Visual Web arendaja, WebDeploy, FTP, TFS, PowerShelli IIS-i haldur.|Visual Studio Azure SDK TFS, PowerShelli. Iga pilveteenuses on kahes keskkonnas, millele saate juurutada oma teenuste paketti ja konfigureerimine: lavastus ja tootmise. Saate juurutada pilveteenus lavastus keskkonnas katsetada, enne kui selle edendada tootmisele.|Visual Studio, WebMatrix, Visual Web arendaja, FTP, GIT, BitBucket, CodePlex, Dropboxi, GitHub, Mercurial, TFS, Web juurutada PowerShelli.|
|**Haldamine ja häälestamine**|Teie vastutate haldustoiminguid rakenduse andmete tulemüüri reeglid, virtuaalse võrgu ja operatsioonisüsteem.|Teie vastutate haldustoiminguid rakenduse, andmete, tulemüüri reeglid ja virtuaalse võrgu.|Teie vastutate haldustoiminguid taotluse ja ainult andmed.|
|**Kõrge-saadavus ja avariitaastet (HADR)**|Soovitame, et paigutada virtuaalmasinates kättesaadavus samu ja sama pilveteenuses. Hoida oma VMs sama kättesaadavus määramine võimaldab Azure'i paigutada kõrge-saadavus sõlmed eraldi viga domeenid ja uuendada domeenid. Samuti hoida oma VMs sama pilveteenuses võimaldab koormusetasakaalustuseks ja VMs saaksid suhelda otse üksteise sees on Azure andmekeskuse kohalikus võrgus.<br/><br/>Teie vastutate rakendamiseks kättesaadavuse ja katastroofi lahendus SQL Server Azure'i Virtuaalmasinates mis tahes vältimiseks. Toetatud HADR tehnoloogiale, vt [kättesaadavuse ja SQL Server Azure'i Virtuaalmasinates katastroofiabi](virtual-machines-windows-sql-high-availability-dr.md).<br/><br/>Teie vastutate varundada oma andmeid ja.<br/><br/>Azure'i saate teisaldada oma virtuaalmasinates host masina andmekeskuse nurjumisel tõttu riistvara probleemid. Lisaks tekitada kavandatud tööseisakute oma VM majutusseadme uuendamisel turvalisuse ja tarkvara värskendused. Seetõttu soovitame, et haldate vähemalt kaks rakenduse iga taseme pidev kättesaadavuse tagamiseks VMs. Azure'i ette SLA ühe virtuaalse masina. Lisateavet leiate teemast [Azure paindlikkust tehnilised juhendid](../resiliency/resiliency-technical-guidance.md).|Azure'i haldab tõrkeid, mis tuleneb aluseks oleva riistvara või operatsioonisüsteemi tarkvara. Soovitame juurutada mitmes eksemplaris veebis või töötaja rolli rakenduse kõrge kättesaadavuse tagamiseks. Lisateavet leiate teemast [pilveteenustega, Virtuaalmasinates, ja virtuaalse võrgu teenuselepingu](http://www.microsoft.com/download/details.aspx?id=38427) ja [katastroofiabi ja Azure rakenduste kõrge kättesaadavus](../resiliency/resiliency-disaster-recovery-high-availability-azure-applications.md)<br/><br/>Teie vastutate varundada oma andmeid ja.<br/><br/>Andmebaaside elavate on Azure VM SQL serveri andmebaasi, vastutate rakendamiseks on kõrge kättesaadavus ja avariitaastet lahendus mis tahes vältimiseks. Toetatud HDAR tehnoloogiale, vt SQL Server Azure'i Virtuaalmasinates kättesaadavuse ja katastroofiabi.<br/><br/>**SQL serveri andmebaasi peegeldus**: Azure'i pilveteenustega (web/töötaja rollid) kasutamine. SQL serveri VMs ja pilvepõhise teenuse project võib olla sama Azure virtuaalse võrgu. Kui SQL serveri VM virtuaalse samasse võrku pole, peate looma side marsruutimiseks SQL serveri eksemplar SQL Server pseudonüüm. Lisaks pseudonüüm peab vastama SQL serveri nime.|Kõrge-saadavus pärineb Azure töötaja rollid, Azure'i bloobimälu ja Azure SQL-andmebaas. Näiteks Azure Storage säilitab bloobimälu, tabeli ja järjekorda andmete kolme koopiad. Korraga, Azure'i SQL-andmebaasi hoiab kolme koopiad kliendiandmed – üks peamine koopia ja kahe teisene koopiad. Lisateabe saamiseks lugege teemat [Azure Storage](https://azure.microsoft.com/documentation/services/storage/) ja [Azure SQL-andmebaasi](../sql-database/sql-database-technical-overview.md).<br/><br/>Kasutamisel SQL Server Azure'i VM andmeallikana Azure Web Apps, pidage meeles, et Azure Web Apps ei toeta Azure virtuaalse võrgu. Teisisõnu, kõik veebirakenduste Azure SQL serveri vms Azure ühendused tuleb läbida virtuaalmasinates avaliku lõpp-punkti. See võib põhjustada kõrge-saadavus ja Avariijärgne taaste stsenaariumid mõned piirangud. Näiteks ei oleks ühendust luua uus esmane server nõuab andmebaasi peegeldus, häälestada Azure SQL serveri host VMs Azure virtuaalse võrguga ühenduse SQL serveri VM koos andmebaasi peegeldus Azure'i veebirakenduste klientrakendus. Seetõttu abil **SQL serveri andmebaasi peegeldus** Azure Web Apps ei toeta praegu.<br/><br/>**SQL serveri Kättesaadavusrühmad**: saate häälestada Kättesaadavusrühmad SQL Server Azure'i VMs Azure'i Web Appsi kasutamisel. Kuid teil on vaja konfigureerida AlwaysOn kättesaadavus rühma kuulajale teatise marsruutimiseks peamine koopia kaudu avaliku koormusetasakaalustusega lõpp-punktid.|
|**Rist – eeldusel Ühenduvus**|Saate ühenduse loomiseks kohapealse Azure virtuaalse võrgu.|Saate ühenduse loomiseks kohapealse Azure virtuaalse võrgu.|Azure virtuaalse võrgu on toetatud. Lisateavet leiate teemast [Web Appsi virtuaalse võrgu integreerimine](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/).|
|**Skaleeritavus.**|Virtuaalse masina suurused suurendades või lisada rohkem ketast on saadaval skaala üles. Virtuaalse masina suurused kohta leiate lisateavet teemast [Azure virtuaalse masina suurused](virtual-machines-windows-sizes.md).<br/><br/>**Andmebaasi Server**: skaala-out on saadaval andmebaasi eraldamine tehnikate ja SQL serveri AlwaysOn kättesaadavus rühmad.<br/><br/>Raske lugeda töökoormus, saate kasutada mitut teisene sõlmed kui ka SQL serveri Dispersioonanalüüs [Kättesaadavusrühmad](https://msdn.microsoft.com/library/hh510230.aspx) .<br/><br/>Raske kirjutamine töökoormus, saate rakendada mitme füüsilise serverites pakkuda rakenduse skaala-out horisontaalne eraldamine andmed.<br/><br/>Lisaks saate rakendada skaala-out, kasutades [SQL serveri abil andmed sõltuvad marsruutimist](https://technet.microsoft.com/library/cc966448.aspx). Koos andmed sõltuvad marsruutimist (DDR), tuleb teil rakendada klientrakenduse, tavaliselt on business taseme kiht, mitu SQL serveri sõlmed andmebaasi taotlusi marsruutimiseks eraldamine süsteem. Taseme business sisaldab vastendused kuidas on liigendatud andmete ja sõlm, mis sisaldab andmeid.<br/><br/>Saate skaala virtuaalmasinates töötavad rakendused. Lisateavet leiate teemast [mastaapimiseks rakendus](../cloud-services/cloud-services-how-to-scale.md).<br/><br/>**Olulise teabe märge**: Azure'i **AutoScale** funktsiooni võimaldab automaatselt suurendamiseks või vähendamiseks Virtuaalmasinates, mida kasutatakse teie rakendus. See funktsioon tagab lõppkasutaja ei mõjuta negatiivselt tippväärtus ajal, ning VMs sulgema kui on madal. Soovitatav on, et te ei määra AutoScale suvandi jaoks oma pilveteenuses kui see sisaldab SQL serveri VMs. Põhjus on selles, et AutoScale funktsioon võimaldab Azure virtuaalse masina sisse lülitada, kui CPU hõivatus, klõpsake selle VM on suurem kui mõned lävi ja virtuaalse masina välja lülitada, kui CPU hõivatus läheb seda väiksem. AutoScale funktsioon on kasulik kodakondsuseta rakendusi, nagu web serverid, kus mis tahes VM saate hallata töökoormus ilma viiteid eelmise riik. Funktsiooni AutoScale pole abiks stateful rakendused, näiteks SQL serveri, kus ainult üks näiteks võimaldab andmebaasi kirjutamist.|Skaala üles on saadaval, kasutades mitut veebi ja töötaja rollid. Virtuaalse masina suurused web rollid ja töötaja rolli kohta leiate lisateavet teemast [Pilveteenustega suurused konfigureerimine](../cloud-services/cloud-services-sizes-specs.md).<br/><br/>**Pilveteenuste**kasutamisel saate määratleda mitmest rollide levitamine töötlemine ja ka saavutada paindlik skaleerimist rakenduse. Iga pilveteenuses sisaldab ühe või mitme web rollid ja/või töötaja rollid, iga oma rakenduse failid ja konfigureerimine. Saate skaala üles pilveteenus rolli protsesside (virtuaalmasinates) arvu suurendades rolli juurutatud ja skaala-down pilveteenus vähendades rolli eksemplaride arv. Üksikasjalikku teavet leiate teemast [Azure täitmise mudelid](../cloud-services/cloud-services-choose-me.md).<br/><br/>Skaala-out on saadaval sisseehitatud Azure kõrge-saadavus tugi [pilveteenustega, Virtuaalmasinates, ja virtuaalse võrgu teenuselepingu](http://www.microsoft.com/download/details.aspx?id=38427) ja laadi koormusetasakaalustusteenuse kaudu.<br/><br/>Mitmekihilise rakenduse, soovitame, et loote web/töötaja rollid rakenduse andmebaasi serveri VMs Azure virtuaalse võrgu kaudu. Lisaks Azure pakub laadi tasakaalustamiseks vms sama pilveteenuses levitada kasutaja taotlused kogu neid. Sel viisil ühendatud virtuaalmasinates saate suhtlevad otse sees on Azure andmekeskuse kohalikus võrgus.<br/><br/>Saate häälestada **AutoScale** Azure klassikaline portaali kui ka ajakava korda. Lisateavet leiate teemast [mastaapimiseks rakendus](../cloud-services/cloud-services-how-to-scale.md).|**Mastaapimiseks üles**: te saate muutus mahu eksemplari (VM) oma veebisaidi jaoks.<br/><br/>Skaala läbi: saate lisada rohkem reserveeritud eksemplarid (VM) oma veebisaidi jaoks.<br/><br/>Saate häälestada **AutoScale** portaali kui ka ajakava korda. Lisateavet leiate teemast [veebirakenduste skaala](../app-service-web/web-sites-scale.md).|

Valides vahel programmeerimise viiside kohta leiate lisateavet teemast [Azure veebirakenduste, pilveteenustega ja VMs: Millal kasutada mis](../app-service-web/choose-web-site-cloud-service-vm.md).

## <a name="next-steps"></a>Järgmised sammud

SQL Server Azure'i virtuaalse masinad käitamise kohta lisateabe saamiseks vt [SQL Server Azure'i Virtuaalmasinates ülevaade](virtual-machines-windows-sql-server-iaas-overview.md).
