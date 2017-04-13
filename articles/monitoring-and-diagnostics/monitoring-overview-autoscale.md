<properties
    pageTitle="Ülevaade Microsoft Azure'i Virtuaalmasinates, pilveteenustega ja veebirakenduste autoscale | Microsoft Azure'i"
    description="Microsoft Azure autoscale ülevaade. Kehtib Virtuaalmasinates, pilveteenustega ja veebirakenduste."
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="robb"/>

# <a name="overview-of-autoscale-in-microsoft-azure-virtual-machines-cloud-services-and-web-apps"></a>Microsoft Azure'i Virtuaalmasinates, pilveteenustega ja veebirakenduste autoscale ülevaade

Selles artiklis kirjeldatakse, mis Microsoft Azure'i autoscale on oma eelised ja kuidas seda kasutamise alustamine.  

Azure'i kuvari autoscale kehtib ainult [Virtuaalse masina skaala komplektid](https://azure.microsoft.com/services/virtual-machine-scale-sets/), [Pilveteenustega](https://azure.microsoft.com/services/cloud-services/)ja [Rakenduse teenuse - Web Apps](https://azure.microsoft.com/services/app-service/web/).

>[AZURE.NOTE] Azure'i on autoscale kaks võimalust. Varasem versioon autoscale kehtib Virtuaalmasinates (kättesaadavus komplekti). See funktsioon on piiratud tugi ja soovitame migreerimine kiirem ja usaldusväärseid autoscale tugiteenuste VM skaala komplektid. Selles artiklis sisaldub kasutamine vanem tehnoloogia link.  


## <a name="what-is-autoscale"></a>Mis on autoscale?

Autoscale võimaldab teil on õige kogus ressursse, mis töötavad rakenduse koormus käsitlema. See võimaldab teil lisada ressursse toime laadi tõusu ja ka säästa, eemaldades ressursse, mis on vähemalt jõude. Saate määrata miinimum- ja arvul käivitamine ja lisamiseks või eemaldamiseks VMs reeglikogumi põhjal automaatselt. Minimaalne teeb kindel, et teil on rakenduse alati töötab isegi ilma koormuse. Mille piirab oma kogukulu võimalike tunnis. Saate automaatselt mastaapida vahel nende kahe äärmused abil saate luua reeglid.

 ![Autoscale ülevaade. Lisamine ja eemaldamine VMs](./media/monitoring-autoscale-overview/AutoscaleConcept.png)

Kui reegli tingimused on täidetud, käivitatakse autoscale üks või mitu toimingut. Saate lisada ja eemaldada VMs või teha muid toiminguid. Kontseptuaalne järgmisel joonisel on esitatud selle protsessi.  

 ![Kontseptuaalne Autoscale vooskeemi](./media/monitoring-autoscale-overview/AutoscaleOverview3.png)


## <a name="autoscale-process-explained"></a>Autoscale protsessi ülevaade
Järgmine selgitus rakendada eelmise skeemi tekstilõigule.   

### <a name="resource-metrics"></a>Ressursi mõõdikud
Ressursside eraldavad mõõdikute, mida hiljem töödeldakse reeglid. Mõõdikute tulevad kaudu mitmel viisil.
VM skaala komplektid kasutab telemeetria Azure diagnostika agentide tuleks telemeetria veebirakenduste ja pilveteenustega tuleb otse Azure infrastruktuuri. Mõned sagedamini kasutatavad statistika kaasata CPU hõivatus, mälukasutust, jutulõnga loendab, järjekorra pikkus ja kettaruumi kasutus. Millised telemeetria andmed, mida saate kasutada loendi leiate teemast [Autoscale levinud mõõdikute](insights-autoscale-common-metrics.md).

### <a name="time"></a>Aeg
Ajakava põhinevad reeglid põhinevad UTC. Peate määrama oma ajavööndi õigesti reeglite häälestamisel.  

### <a name="rules"></a>Reeglid
Skeemi kuvatakse ainult üks autoscale reegel, kuid mitu neist. Saate luua keerukate kattuvad reeglid vastavalt vajadusele teie olukorda.  Reegli tüübid  

 - **Meetermõõdustik vastavalt** – näiteks teha seda toimingut, kui CPU hõivatus on üle 50%.
 - **Ajapõhiste** – näiteks päästik on webhook iga 08: 00 laupäev antud ajavööndi.

Meetermõõdustik põhinevaid reegleid, mis on mõõta rakenduse laadimine ja lisada või eemaldada vastavalt selle laadimisel VMs. Ajakava põhinevad reeglid võimaldavad teil näha oma laadi kellaaja mustrite ja soovite mastaapimiseks enne võimalike laadi suurendamine või vähendamine toimub mastaapimiseks.  


### <a name="actions-and-automation"></a>Toimingud ja automatiseerimine

Reegleid saab käivitada vähemalt ühte tüüpi toiminguid.

- **Skaala** - skaala VMs sisse või välja
- **E-posti** - tellimuse administraatorid, co – administraatorid ja/või e-posti aadressi teie määratud meilisõnumi saatmine
- **Automatiseerimine webhooks kaudu** – kõne webhooks, mis käivitab mitme keerukate toimingute või Azure väljaspool. Sees Azure, saate alustada Azure automatiseerimine käitusjuhendi, Azure funktsioon või Azure loogika rakendus. Näide 3 tootja URL-i Azure kaasata teenuseid, nagu vaikne ja Twilio.


## <a name="autoscale-settings"></a>Autoscale sätted
Autoscale kasutada järgmiste terminite ja struktuuri.

- Mõne **autoscale säte** on kirjutuskaitstud autoscale mootori abil, kas skaala üles või alla. See sisaldab ühte või rohkem profiilid, target ressursi- ja teavitussätete teavet.
    - On **autoscale profiili** on kombinatsioon, mille maht on eeskirjad päästikute ja profiili ja korduvuse skaala toimingute kogum, millega. Teil võib olla mitu profiili, mis võimaldavad teil erinevate kattuvate eest.
        - **Võimsus säte** näitab miinimum, maksimum ja vaikeväärtuste eksemplaride arv. [vastavale kasutada joon 1]
        - **Reegli** sisaldab käivitamiseks – kas argumendil päästik või kord käivitada – ja skaala toimingut, mis näitab, kas autoscale peaksid skaala üles või alla, kui see reegel on täidetud.
        - **Korduvus** , tähendab see, kui autoscale tuleks jõustada seda profiili. Teil võib olla eri autoscale profiilid erineval päeva või nädalapäevadel, näiteks.
- **Meiliteatise säte** määratleb teatised, mis peaks toimuma autoscale sündmuse ilmnemisel ühe autoscale säte profiilid kriteeriumide alusel. Autoscale saate teavitamise üks või mitu e-posti aadressi või ühe või mitme webhooks helistada.

![Azure'i autoscale säte, profiili ja reegli struktuur](./media/monitoring-autoscale-overview/AzureResourceManagerRuleStructure3.png)

Täielik konfigureeritav väljad ja kirjeldused on saadaval [Autoscale REST API -ga](https://msdn.microsoft.com/library/dn931928.aspx).

Koodi näited

* [Täiustatud Autoscale konfiguratsiooni VM skaala komplektide ressursihaldur mallide kasutamine](insights-advanced-autoscale-virtual-machine-scale-sets.md)  
* [Autoscale REST API-ga](https://msdn.microsoft.com/library/dn931953.aspx)



## <a name="horizontal-vs-vertical-scaling"></a>Horisontaalne vs vertikaalne mastaapimine

Autoscale suurendab ressursside ainult skaalasid horisontaalselt, mis on suurendada ("välja") või vähenemine ("") VM eksemplaride arv.  Horisontaalne mastaapimist, mis on paindlikumad cloud olukorras, nagu seda saab käivitada potentsiaalselt tuhandetele VMs käsitlema laadi. Vertikaalne skaleerimist erineb. See hoiab samal hulgal VMs, kuid teeb VM ("kuni") rohkem või vähem ("alla") võimsaid. Power on mõõdetud mälu, CPU kiiruse kettaruumi jne.  Vertikaalne skaleerimist on veel piirangud. See sõltub kättesaadavus suuremat riistvara, mida saate piirkonniti ja kiiresti tabab ja ülempiir. Vertikaalne skaleerimist ka tavaliselt nõuab VM Peata ja start. Lisateavet leiate teemast [vertikaalselt mastaapimiseks Azure virtuaalse masina Azure automatiseerimine](../virtual-machines/virtual-machines-linux-vertical-scaling-automation.md).


## <a name="methods-of-access"></a>Accessi meetodid
Saate häälestada autoscale kaudu

- [Azure'i portaal](insights-how-to-scale.md)
- [PowerShelli](insights-powershell-samples.md#create-and-manage-autoscale-settings)
- [Mitu platvormi käsurea liides (CLI)](insights-cli-samples.md#autoscale )
- [Azure'i kuvari REST API-ga](https://msdn.microsoft.com/library/azure/dn931953.aspx )

## <a name="supported-services-for-autoscale"></a>Toetatud autoscale teenused


| Teenus                              | Skeemi & dokumendid                                       |
|--------------------------------------|-----------------------------------------------------|
| Web Apps                             | [Skaleerimise Web Apps](insights-how-to-scale.md)              |
| Pilveteenused                       | [Autoscale pilveteenus](../cloud-services/cloud-services-how-to-scale.md) |
| Virtuaalmasinates: klassikaline           | [Skaleerimise klassikaline virtuaalse masina kättesaadavus komplektid](https://blogs.msdn.microsoft.com/kaevans/2015/02/20/autoscaling-azurevirtual-machines/) |
| Virtuaalmasinates: Windows skaala komplektid| [Skaleerimist VM skaala seab Windows](../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md)  |
| Virtuaalmasinates: Linux skaala määrab  | [Määrab skaleerimist VM skaala Linux](../virtual-machine-scale-sets/virtual-machine-scale-sets-linux-autoscale.md) |
| Virtuaalmasinates: Windows näide   | [Täiustatud Autoscale konfiguratsiooni VM skaala komplektide ressursihaldur mallide kasutamine](insights-advanced-autoscale-virtual-machine-scale-sets.md) |

## <a name="next-steps"></a>Järgmised sammud

Lisateavet autoscale kasutamine Autoscale juhendavad tutvustused loetletud varem või viidata järgmistest allikatest:

- [Azure'i kuvari autoscale levinud mõõdikud](insights-autoscale-common-metrics.md)
- [Azure'i kuvari autoscale head tavad](insights-autoscale-best-practices.md)
- [Saada e-posti ja webhook teatiste autoscale toimingute abil](insights-autoscale-to-webhook-email.md)
- [Autoscale REST API-ga](https://msdn.microsoft.com/library/dn931953.aspx)
- [Tõrkeotsingu virtuaalse masina skaala komplektid Autoscale](../virtual-machine-scale-sets/virtual-machine-scale-sets-troubleshoot.md)
