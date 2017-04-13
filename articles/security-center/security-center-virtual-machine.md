<properties
   pageTitle="Azure'i turbekeskus ja Azure'i Virtuaalmasinates | Microsoft Azure'i"
   description="Selle dokumendi aitab teil mõista, kuidas Azure'i turbekeskus saate kaitsta teie Azure'i Virtuaalmasinates."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/07/2016"
   ms.author="yurid"/>

# <a name="azure-security-center-and-azure-virtual-machines"></a>Azure'i turbekeskus ja Azure'i Virtuaalmasinates

[Azure'i turbekeskus](https://azure.microsoft.com/services/security-center/) aitab vältida, avastada ja ohtude vastata. Integreeritud turvalisus jälgimine ja rühmapoliitika haldus pakub oma Azure tellimustes, aitab tuvastada ohud, mis võivad muidu märkamata ja töötab laialdane ökosüsteemi turvalisus lahendusi.

Selles artiklis kirjeldatakse, kuidas turbekeskus aitab teil tagada teie Azure'i Virtuaalmasinates (VM).

## <a name="why-use-security-center"></a>Miks kasutada turbekeskus?

Turbekeskus aitab teil kaitsta Azure virtuaalse masina andmed esitada nähtavus virtual seadme turbesätted. Kui turbekeskus kaitseb teie VMs, on saadaval järgmised funktsioonid:

- Operatsioonisüsteem (OS) turbesätted soovitatav konfiguratsioon reeglite abil
- Süsteemi turbe- ja kriitilised värskendused, mis on puudu
- Endpoint protection soovitused
- Ketta krüptimise valideerimine
- Haavatavushindamises ja parandamine
- Ohtude tuvastamise

Lisaks aitab kaitsta teie Azure VMs, turbekeskus annab turvalisus jälgimise ja halduse pilveteenustega, rakenduse teenused, virtuaalse võrgu ja palju muud. 

>[AZURE.NOTE] Lugege lisateavet Azure turbekeskus [Azure'i turbekeskus tutvustus](security-center-intro.md) .

## <a name="prerequisites"></a>Eeltingimused

Alustada Azure'i turbekeskus, peate teadma ja arvestage järgmisega.

- Peab teil olema Microsoft Azure'i tellimust. Security Center tasuta- ja standardversiooni astme kohta lisateabe saamiseks vaadake [Turvalisus Center hinnad](https://azure.microsoft.com/pricing/details/security-center/) .
- Teie turbekeskus kasutuselevõtu leping, leiate [Azure'i turbekeskus plaanimine ja toimingute juhend](security-center-planning-and-operations-guide.md) kavandamise ja toimingute kaalutluste kohta.
- Operatsioonisüsteemi klienditoe kohta leiate teemast [Azure turbekeskus korduma kippuvad küsimused (KKK)](security-center-faq.md). 

## <a name="set-security-policy"></a>Poliitika määramine

Andmete kogumine peab olema lubatud nii, et selle Azure'i turbekeskus koguda soovitusi ja teatisi teavet vastavalt turbepoliitika, saate konfigureerida. Alloleval joonisel näete, et **andmete kogumine** on **sisse lülitatud**.

Turvalisuse poliitika määratleb juhtelementide, mis on soovitatav ressursid määratud tellimuse või ressursirühma. Enne turbepoliitika, peab teil olema lubatud andmete kogumine, turbekeskus kogub andmeid oma virtuaalmasinates hinnake oma turvalisus oleku, soovituste turvalisus ja märku, et ohtude. Turbekeskus, saate määratleda oma Azure tellimuste või ressursside rühmad vastavalt oma ettevõtte turvalisus vajadustele ja rakenduste tüübi või tundlikkust iga tellimuse andmed. 

![Turbepoliitika](./media/security-center-virtual-machine/security-center-virtual-machine-fig1.png)

>[AZURE.NOTE] Lisateavet iga **vältimise poliitika** saadaval, vaadake artiklis [määramine turbepoliitikate](security-center-policies.md) .

## <a name="manage-security-recommendations"></a>Turvalisus soovitused haldamine

Turbekeskus analüüsib oma Azure turvalisus seisundi. Kui turbekeskus tuvastab võimalikud turvahaavatavusi, loob see soovitusi. Soovitused juhatavad teid protsessi konfigureerimine vajalik juhtelement.

Pärast turvalisuse poliitika seadmisele turbekeskus analüüsib oma ressursse, mis tähistavad võimaliku turvalisus olek. Soovitused on esitatud tabelina, kus iga rida tähistab ühe kindla soovituse. Järgmises tabelis antakse soovitused Azure'i VMs ja mida igat teha, kui rakendate selle näited. Kui valite soovituse, teile antakse teavet, mis näitab, kuidas rakendada soovitust Security Center.

|Soovitused|Kirjeldus|
|-----|-----|
|[Andmete kogumine tellimuste lubamine](security-center-enable-data-collection.md)|Soovitab lülitada andmete kogumise turbepoliitika iga tellimuste ja kõik virtuaalmasinates (VM) oma kontot.|
|[Migreerimisel ning probleemide lahendamisel OS nõrkade kohtade](security-center-remediate-os-vulnerabilities.md)|Soovitab joondada oma OS konfiguratsioone soovitatav konfiguratsioon reegleid, nt võimaldab paroole salvestada.|
|[Süsteemi värskenduste rakendamine](security-center-apply-system-updates.md)|Soovitab juurutama VMs puuduvad süsteemi turbe- ja kriitilised värskendused.|
|[Taaskäivitage pärast süsteemi värskendused](security-center-apply-system-updates.md#reboot-after-system-updates)|Soovitab VM süsteemi värskenduste rakendamine protsessi lõpuleviimiseks taaskäivitada.|
|[Endpoint Protection installimine](security-center-install-endpoint-protection.md)|Soovitab, et olete ette ründevaratõrje programmid vms (ainult Windows VMs).|
|[Endpoint Protection seisund teatiste lahendamine](security-center-resolve-endpoint-protection-health-alerts.md)|Soovitab lahendada endpoint protection ebaõnnestumist.|
|[Luba VM Agent](security-center-enable-vm-agent.md)|Võimaldab teil näha, mis nõuavad VMs VM Agent. VM Agent peab olema installitud VMs selleks, et ettevalmistamise paik skannimine, võrdlusalus skannimine ja ründevaratõrje programmid. Vaikimisi installitakse VM Agent vms Azure'i turuplatsilt juurutatud. Artikli [VM Agent ja laiendid – osa 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) teave VM agendi installimise kohta.|
| [Ketta krüptimise rakendamine](security-center-apply-disk-encryption.md) |Soovitab teie VM ketast Azure'i ketta krüptimise (Windows ja Linux VMs) abil krüptida. Krüptimist, on soovitatav kasutada oma VM OS nii andmete maht.|
| [Haavatavushindamises pole installitud](security-center-vulnerability-assessment-recommendations.md) | Soovitab lahenduse haavatavuse assessment installimine oma VM. |
| [Migreerimisel ning probleemide lahendamisel nõrkade kohtade](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Võimaldab teil näha süsteemi ja rakenduste nõrkade kohtade tuvasta installitud oma VM haavatavuse hindamise lahendus. |

>[AZURE.NOTE] Lisateavet soovitusi, vt [haldamine turvalisus soovitused](security-center-recommendations.md) artikkel.

## <a name="monitor-security-health"></a>Kuvari turvalisus seisund

Kui olete lubanud [turbepoliitikate](security-center-policies.md) ressursid on tellimus, analüüsib turbekeskus oma ressursse, mis tähistavad võimaliku turvalisus.  Saate vaadata oma ressursse probleemidest **ressursside turvalisus seisund** tera koos riigi turvalisus. **Ressursside turvalisus** seisund paani **virtuaalmasinates** klõpsamisel avaneb **virtuaalmasinates** tera soovitused oma VMs. 

![Turvalisus seisund](./media/security-center-virtual-machine/security-center-virtual-machine-fig2.png)

## <a name="manage-and-respond-to-security-alerts"></a>Haldamine ja turbeteatised vastamine

Turbekeskus automaatselt kogub, analüüsib ja ühendab log andmeid oma Azure ressursse, võrgu ja ühendatud partnerilahenduste (nt tulemüüri ja lõpp-punkti kaitse lahendused), tuvasta ohud ja vähendamiseks vale-positiivsed. Tehtavate mitmesuguse koondamine [tuvastamise](security-center-detection-capabilities.md)võimalused, turbekeskus on võimalik luua tähtsuse turbeteatised aitavad kiiresti probleemi uurida ja soovituste kohta, kuidas migreerimisel ning probleemide lahendamisel võimalike eest.

![Turbeteatiste](./media/security-center-virtual-machine/security-center-virtual-machine-fig3.png)

Valige turbeteates Lisateavet teatise ja mida, käivitanud sündmus (ed) kui mis tahes, peate tegema, et migreerimisel ning probleemide lahendamisel rünnaku juhised. Turbeteatiste rühmitatud [Tüüp](security-center-alerts-type.md) ja kuupäev.


## <a name="see-also"></a>Vt ka

Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – saate teada, kuidas konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad.
- [Haldamine ja turbeteatised Azure'i turbekeskus koosolekukutsetele](security-center-managing-and-responding-alerts.md) --saate teada, kuidas hallata ja turbeteatised vastata.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
