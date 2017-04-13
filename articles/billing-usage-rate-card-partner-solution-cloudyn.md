<properties
   pageTitle="Microsoft Azure'i kasutus- ja RateCard API-de luba Cloudyn ITFM ette kliendid | Microsoft Azure'i"
   description="Microsoft Azure'i arveldus partnerilt Cloudyn, oma kogemusi integreerimine Azure arveldus API nende toode pakub ainulaadse perspektiivi.  See on eriti kasulik Azure ja Cloudyn klientidele, kes on huvitatud abil/proovimise Cloudyn Azure'i teenuste jaoks."
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="08/16/2016"
   ms.author="mobandyo;bryanla"/>

# <a name="microsoft-azure-usage-and-ratecard-apis-enable-cloudyn-to-provide-itfm-for-customers"></a>Microsoft Azure'i kasutus- ja RateCard API-de luba Cloudyn ITFM ette kliendid

Cloudyn, arengu Microsofti partneri ja pilveteenuse halduse võimaluste, ees pakkuja valiti privaatne eelvaade uue Microsoft Azure'i ressursikasutus ja RateCard API-d.  Kasutus API pakub hinnanguline Azure'i andmetele juurdepääsu tellimusele. RateCard API pakub täielikku hinnateavet Azure'i teenuste, mitte - Enterprise'i leping EA klientide. Koos integreeritud, nende API-de alus kogu teave selle sisestatud rahandus Management (ITFM) tööriistu, nagu need, mida Cloudyn.

## <a name="introduction"></a>Sissejuhatus

Nn "korrutamine" andmete kasutus API RateCard API andmetega (hind kasutus [üksuste] [$unit] = üksikasjalik kasutus- ja kulu) loob kõige Varundustöö, täpne ja usaldusväärne arveldusteabe saadaval Azure täna.

![ITFM ülevaade][1]

Nende API-de tarbimine annab olulise teabe klientide kasutamine ja kulud, võimaldab Cloudyn lihtne, programmiline viisil kliendi kontod analüüsimiseks ja oma klientidele erinevate ITFM tööülesannete täitmiseks.

## <a name="integrating-cloudyn-with-the-ratecard-and-usage-apis"></a>Cloudyn integreerimise RateCard ja API-de kasutamine
RateCard API nõuab mitme sisendparameetrite – näiteks piirkonna teave, valuuta ja lokaadi--, kuid kõige olulisem on OfferDurableID, mis määrab Azure pakub kliendi tüüp kasutab (grupikindlustusleping, pärand 6 ja 12 kuu kohustuse lepingud, MSDN-i pakkumised, MPN pakkumised, reklaami pakub ja teised). Funktsiooni OfferDurableID leiate [Azure'i kasutus- ja arveldamine portaali](https://account.windowsazure.com/Subscriptions)ja jaotises "Pakkumine ID" antud tellimuse jaoks.

Registreerimisel [Cloudyn Azure](https://www.cloudyn.com/microsoft-azure/) teenuste kliendid saate lisada oma OfferDurableID koodi, mis võimaldab Cloudyn tõmmata oluline hinnateavet RateCard API kaudu.  Pakutakse erinevat tüüpi teavet ühte [Microsoft Azure'i pakkuda üksikasjade](https://azure.microsoft.com/support/legal/offer-details/) lehe.

![Cloudyn ITFM Engine ülevaade][2]

Cloudyn kasutab nii kasutuse ja RateCard API-d, lisaks Azure jõudluse API, luua täiendavaid kihid visualiseeringu, analytics, aruandlus, kulude haldamine ja vaidlustatavate soovitused, pakkudes Azure klientidele usaldusväärne enterprise cloud ITFM tööriista.

## <a name="cloudyn-itfm-use-cases-enabled-by-usage-and-ratecard-api-integration"></a>Cloudyn ITFM kasutamise juhtudel lubatud kasutus ja RateCard API integreerimine
Levinud Cloudyn ITFM juhtumid lubanud kasutus ja RateCard API lisada:

+ **Kulude analüüsi** - võimaldab cloud on katkenud allapoole mis tahes kohalikke isikupärase mõõtmed (pakkuja, teenuse, konto, piirkond jne). Azure'i kasutus- ja RateCard API-de teha seda lihtne ülesanne, pakkudes täpne jaotus kasutus- ja maksumus ühe konto, mis on seejärel rühmitatud ning Cloudyn järgi filtreeritud ja kasutajale, pildi või tabeli kujul esitatud andmed.

![Kulude analüüs sektordiagrammi loomine][3]

+ **Maksumus eraldatud 360** - lubab rahandus ja IT-juhid avastada tegelik kulu jaotus, draiverid ja trendide cloud nende kasutamise. See võimaldab täpsemaks juurutamise kulud hõlpsalt seostada business üksused, osakondade, piirkondade ja muud, pakub enneolematu ülevaateid cloud kulud ja hõlbustada enterprise Sissenõudmised ja showbacks haldurid. Azure'i kasutus- ja RateCard API-de kasutada sisendina Cloudyn's maksumus eraldatud mootor, mis on API-de täiendab määratlemine meetodite ja äriloogika sildistamata või untaggable ressursse.

![Kulude jaotamine 360 diagramm][4]

+ **Cost-Effective suurus** – pakub paremalt-sizing soovitused alakasutatud virtuaalmasinates, vähendades kliendi kulud mahukate või üle ettevalmistatud arvutites. Seda tehes kontrollides virtuaalse masina CPU ja RAM mõõdikute (kaudu jõudluse API) käitusaja (kaudu kasutus API) ja maksumus (RateCard API) kaudu. Cloudyn siis pakub paremale-sizing soovitusi alakasutatud CPU või RAM-i ressursid (jõudlus) ja arvutab eeldatava säästude korrutatakse hind delta (RateCard) vahel VMs tegelik aeg-kasutamine (kasutusega) alakasutatud masina järgi.

![Tõhus suuruse muutmine][5]

+ **Pilveteenuse teisaldamise soovitused** - annab rahalise nõu cloud teisaldamise. See uurib kasutaja praeguse kulud cloud ressursse, mis on suurem pilveteenuse tarnijatele juurutatud ja võrreldakse seda Azure võrdväärse kasutamise maksumus. Seejärel annab Varundustöö, kohta – ressurss, rahaliselt vastavalt teisaldamise Azure'i soovitusi. Pärast hindamise võrdväärse juurutamise nõutav Azure (vastavalt jõudluse mõõdikute ja kasutajale eelistused), kasutab Cloudyn RateCard API hinnata võrdväärse juurutamise Azure maksumus.

+ **Jõudlusearuannete** - lubanud Azure jõudluse API, nende aruannete massiivi funktsioone CPU ja RAM-i kasutamine optimeerimine soovitusi. Allpool on eksemplari kasutamine aruande näide, esitamine eksemplari jaotus, keskmise CPU kasutamine.

![Jõudluse aruanded][6]

+ **Kategooria halduri** - võimsaid funktsiooni Cloudyn, mis viib tellimuse organiseerimata cloud ressursid. See võimaldab kasutajatel vabadusastmega oma kordumatu kategooriate (sildid) tõhus mõõte ja aruannete loomiseks, mis vastab ettevõtte tavad. Lisaks kasutajad saavad hõlpsalt reguleerida ja kategooriate määramine tabelites sisalduvas sildistamine (st Näpu- ja muud erinevused) ja automaatselt tuvastada sildistamata ressursid täpne maksumus omistamine.

![Kategooria haldur][7]

## <a name="video"></a>Video

Siin on lühikest videot, mis näitab, kuidas olete Azure klient saate kasutada Cloudyn Azure ja Azure arveldus API ülevaateid Azure'i nende andmete põhjal.

> [AZURE.VIDEO cloudyn-provides-cloud-itfm-tools-via-microsoft-azure-apis]


## <a name="next-steps"></a>Järgmised sammud

+ Käivitage [Cloudyn Azure](https://www.cloudyn.com/microsoft-azure/) tasuta prooviversioon näha, kuidas saate hankida maksumus läbipaistvust kohandatud aruandlust ja analüüsi Microsoft Azure'i pilveteenuste juurutamiseks.
+ Leiate [oma Microsoft Azure'i ressursside tarbimine ülevaate saada](billing-usage-rate-card-overview.md) ülevaate Azure'i ressursikasutus ja RateCard API-d.
+ [Azure'i arveldus REST API viide](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c) nii API, mis on esitatud Azure ressursihaldur API-de määramine kohta lisateabe saamiseks vaadake.
+ Kui soovite alustada paremale proovi kood, vaadake meie Microsoft Azure'i arveldus API koodinäiteid [Azure'i koodinäiteid](https://azure.microsoft.com/documentation/samples/?term=billing)kohta.

## <a name="learn-more"></a>Lisateave
+ Lisateavet Microsoft Azure'i Enterprise lepingu (EA) pakub külastage [litsentsimise Azure'i ettevõtte] (https://azure.microsoft.com/pricing/enterprise-agreement/)
+ Lisateavet Azure ressursihaldur kohta leiate teemast [Azure ressursihaldur ülevaade](azure-resource-manager/resource-group-overview.md) .
+ Lisateavet on komplekt tööriistu, et aidata omandada cloud mõistmiseks veedavad, vt Gartner artiklis [Market juhend IT rahandus Management (ITFM) tööriistad](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb).

<!--Image references-->
[1]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Overview.png
[2]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Engine-Overview.png
[3]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Analysis-Pie-Chart.png
[4]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Allocation-360-Chart.png
[5]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Effective-Sizing.png
[6]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Performance-Reports.png
[7]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Category-Manager.png
