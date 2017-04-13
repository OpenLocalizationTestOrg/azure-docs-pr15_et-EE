<properties
   pageTitle="Azure'i ressursside seisundi ülevaade | Microsoft Azure'i"
   description="Azure'i ressursi seisundi ülevaade"
   services="Resource health"
   documentationCenter="dev-center-name"
   authors="BernardoAMunoz"
   manager=""
   editor=""/>

<tags
   ms.service="resource-health"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="Supportability"
   ms.date="06/01/2016"
   ms.author="BernardoAMunoz"/>

# <a name="azure-resource-health-overview"></a>Azure'i ressursside seisundi ülevaade

Azure'i ressursi seisund on teenus, mis seab üksikute Azure ressursse seisundi ja annab vaidlustatavad juhised probleemide tõrkeotsing. Cloud keskkonnas, kus ei saa otse juurde serverid või taristu elemente, ressursside seisundi eesmärk on klientide kulutada tõrkeotsing, eriti vähendada ajakulu kindlaks teha, kui probleemi juurkaustas näeb rakenduse sees või kui see on põhjustatud sündmuse sees Azure'i platvormi aja.

## <a name="what-is-considered-a-resource-and-how-does-resource-health-decides-if-the-resource-is-healthy-or-not"></a>Mida peetakse ressursi ja kuidas ressursi seisundi otsustab kui ressurss on terve või mitte? 
Ressursi on kasutaja loodud eksemplar ressursi tüüpi pakutav teenus, näiteks: virtuaalse masina, Web Appi või SQL-andmebaasi. 

Ressursi seisundi tugineb signaale ressurss ja/või teenuse kas ressursi on terve või mitte. See on oluline märkate, et praegu ressursi seisundi ainult kontod ühe kindla ressursi seisundi tippige ja kaaluge muid elemente, mis võivad aidata üldine seisund. Näiteks kui peetakse virtuaalse masina, ainult Arvuta osa infrastruktuuri olekut, st võrgu probleemid ei kuvata ressursi tervise, kui ei ole deklareeritud teenuse elektrikatkestus, aruandlus sel juhul, see on olema uurinud kaudu banner ülaosas tera. Lisateavet teenuse katkestuste pakutakse selle artikli. 

## <a name="how-is-resource-health-different-from-service-health-dashboard"></a>Kuidas erineb ressursi seisundi teenuste seisundi armatuurlaud?

Teavet ressursside seisund on täpsema kui, mis on esitatud teenuste seisundi armatuurlaud. SHD edastab sündmused, mis mõjutavad piirkonnas teenuse kättesaadavus, ressursside seisund seab teatud ressursi teabe, nt kuvatakse nähtavaks tegemine sündmused, mis mõjutavad virtuaalse masina, web appi või SQL-andmebaasi olemasolu. Näiteks kui sõlm ootamatult taaskäivitamisega, kliendid, kelle virtuaalmasinates operatsioonisüsteemiga sõlme saab hankida põhjus, miks nende VM on saadaval teatud aja jooksul.   

## <a name="how-to-access-resource-health"></a>Kuidas pääseda juurde ressursi seisund
Teenuste seisundi ressursside kaudu, on 2 võimalused ressursside seisund.

### <a name="azure-portal"></a>Azure'i portaal
Ressursi seisundi tera Azure'i portaalis saate üksikasjalikku teavet seisundi kohta ressursi kui ka Soovitatavad toimingud, mis sõltuvad praegune seisund ressursi. See blade pakub parima kasutuskogemuse kui päringu ressursi seisund, nagu hõlbustab juurdepääsu muud ressursid portaali sees. Nagu enne mainitud, sõltub Soovitatavad toimingud ressursi seisundi tera kogumi praegune seisund:

* Terve ressursse: Kuna pole probleemi, mis võivad mõjutada ressursi seisundi tuvastas keskenduvad toimingud aidata tõrkeotsingu protsessi. Näiteks pakub otsest juurdepääsu tõrkeotsing teravik, mis pakub juhised, kuidas lahendada levinumaid probleeme kliendid näo.
* Vigane ressurss: Azure'i kaasnevate probleemide, kuvatakse tera toimingud Microsoft kasutab (või on kasutanud) ressursi taastada. Kasutaja kaasnevate probleemide algatatud toimingud, siis tera kliendiloendist toiminguid saate teha nii probleemi lahendamiseks ja ressursi taastada.  

Kui olete Azure portaali sisse logitud, on kaks võimalust ressursi seisundi tera juurdepääsu: 

###<a name="open-the-resource-blade"></a>Ressursi tera avamine
Avage ressursi tera antud ressursi. Kõrval ressursi tera avatavas enne sätted, klõpsake ressursi seisund avamiseks ressursi seisundi tera. 

![Ressursi seisundi blade](./media/resource-health-overview/resourceBladeAndResourceHealth.png)

### <a name="help-and-support-blade"></a>Spikker ja tugi blade
Abi ja toe saamiseks tera avamiseks klõpsake paremas ülanurgas asuv küsimärk ja valite Spikker + tugi. 

**Ülemisel navigeerimisribal**

![Abi + tugi](./media/resource-health-overview/HelpAndSupport.png)

Klõpsake paani avatakse ressursi seisundi tellimuse tera, mis loetleb kõik teie tellimus ressursid. Iga ressursi kõrval on ikoon, mis näitab, selle seisund. Iga ressursi klõpsamisel avatakse ressursi seisundi tera.

**Ressursi seisundi paan**

![Ressursi seisundi paan](./media/resource-health-overview/resourceHealthTile.png)

## <a name="what-does-my-resource-health-status-mean"></a>Mida mu ressursi seisundi olek tähendab?
On 4 erinevad olekud võidakse kuvada oma ressursi.

### <a name="available"></a>Saadaval
Teenust ei ole avastanud probleemideta platvorm, mis võivad mõjutada ressursi olemasolu. See on tähistatud roheline märge ikooni. 

![Ressurss pole saadaval](./media/resource-health-overview/Available.png)

### <a name="unavailable"></a>Pole saadaval

Sel juhul teenus on tuvastanud jätkuva probleemi platvorm, mis on ressurss, näiteks sõlme kus VM töötab ootamatult rebooted kättesaadavus mõjutavad. See on tähistatud punase ikoon hoiatus. Lisateave probleemi kohta on esitatud keskmises jaotises teravik, sh: 

1.  Milliseid toiminguid Microsoft võtab taastada ressurss 
2.  Üksikasjalik ajaskaala probleemi, sh oodatud kulunud aeg
3.  Soovitatavad toimingud kasutajate loend 

![Ressurss pole saadaval](./media/resource-health-overview/Unavailable.png)

### <a name="unavailable--customer-initiated"></a>Pole saadaval – klientide algatatud
Ressurss on näiteks ressursi peatamine või taaskäivitamine taotleb kliendi taotluse tõttu saadaval. Seda näitab sinine teavitamise ikooni. 

![Ressursi pole kasutaja algatatud toimingu tõttu](./media/resource-health-overview/userInitiated.png)

### <a name="unknown"></a>Pole teada
Teenust ei ole esitatud teavet selle ressursi kohta rohkem kui 5 minutit. See on tähistatud halli küsimärk. 

See on oluline Pange tähele, et see ei ole lõplik märk on midagi valesti ressurssi, et kliendid peaks nende soovituste:

* Kui ressursi töötab ootuspäraselt, kuid selle seisund väärtuseks pole teada ressursi seisund, ei ole probleeme ja eeldatavaid oleku ressursi võtta kasutusele terve mõne minuti pärast.
* Kui leidub juurdepääsu ressursile ja selle seisundi probleeme asub tundmatu ressursi seisund, võib see olla ennetähtaegse märk võib olla probleeme ja täiendavad juurdluse tuleks teha kuni seisundi värskendatakse terve või vigane

![Ressursi seisund pole teada](./media/resource-health-overview/unknown.png)

## <a name="service-impacting-events"></a>Teenuse mõjutavad sündmused
Kui pidev teenuse mõjutavad korral võib mõjutada ressursi, kuvatakse venitada ressursi seisundi tera ülaosas. Klõpsates banner avatakse teravik auditilogi sündmused, mis kuvatakse selle katkestuste Lisateave.

![Ressursi seisund võib mõjutada on SIE](./media/resource-health-overview/serviceImpactingEvent.png)

## <a name="what-else-do-i-need-to-know-about-resource-health"></a>Mida peaksin veel on vaja teada ressursi seisundi kohta?

### <a name="signal-latency"></a>Latentsus signaal
Signaale kanali ressursi seisund, võib olla kuni 15 minuti hilinenud, mis võivad põhjustada praegune seisund ressursi olekut ja selle tegelik kättesaadavus. See on oluline pidage meeles nagu see aitab kõrvaldada tarbetu ajakulu uurimise võimalikke probleeme. 

### <a name="special-case-for-sql"></a>SQL-i puhul teisiti 
Ressursi seisundi aruannete SQL-andmebaasi SQL serveri olekut. Läheb seda teed pakub suurema seisund pildi, on vaja, et mitme komponente ja teenuseid tuleb arvesse võtta andmebaasi seisundi määratlemiseks. Praeguse signaal tugineb sisselogimise andmebaasiga, mis tähendab, et andmebaaside jaoks, mis saadetakse tavalise sisselogimise (mis sisaldab muuhulgas päringu täitmise taotluste vastuvõtmine) seisundi olek regulaarselt kuvatakse. Kui andmebaas on külastatud ei või enama 10 minuti jooksul, teisaldatakse see tundmatu olek. See ei tähenda, et andmebaas on saadaval, lihtsalt, et signaal emiteerimist kuna pole sisselogimise viidud. Andmebaasi ühenduse ja päringu käitamisel kuvatakse anda signaale, mis on vaja määrata ja värskendada andmebaasi oleku seisund.

## <a name="feedback"></a>Tagasiside
Oleme alati avatud tagasiside ja soovituste! Saatke meile oma [soovitusi](https://feedback.azure.com/forums/266794-support-feedback). Lisaks saate kaasata meiega [Twitteri](https://twitter.com/azuresupport) või [MSDN-i foorumites](https://social.msdn.microsoft.com/Forums/azure)kaudu.
