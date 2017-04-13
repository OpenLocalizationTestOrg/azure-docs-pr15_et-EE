<properties
   pageTitle="Azure'i Security Center oma virtuaalmasinates kaitsmine | Microsoft Azure'i"
   description="Selle dokumendi aadressid soovitused Azure'i turbekeskus, mis aitavad teil kaitsta oma virtuaalmasinates ja vastavalt turbepoliitikate jääda."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/25/2016"
   ms.author="terrylan"/>

# <a name="protecting-your-virtual-machines-in-azure-security-center"></a>Azure'i Security Center oma virtuaalmasinates kaitsmine

Azure'i turbekeskus analüüsib oma Azure turvalisus seisundi. Kui turbekeskus tuvastab võimalikud turvahaavatavusi, loob see soovitusi, mis juhendab teid protsessi konfigureerimine vajalik juhtelement.  Soovitused Azure'i ressursi tüüpi: networking, SQL-i ja rakenduste virtuaalmasinates (VMs).

Selles artiklis käsitletakse soovitused, mis rakenduvad VMs.  VM soovitused keskenduvad andmete kogumine, rakendades süsteemi värskendused ettevalmistamise ründevaratõrje, krüptimine oma VM ketast ja palju muud.  Kasutage järgmises tabelis, et aidata teil mõista saadaval VM soovitused ja mida igat teha, kui rakendate selle viitena.

## <a name="available-vm-recommendations"></a>Saadaval VM soovitused

|Soovitused|Kirjeldus|
|-----|-----|
|[Andmete kogumine tellimuste lubamine](security-center-enable-data-collection.md)|Soovitab lülitada andmete kogumise turbepoliitika iga tellimuste ja kõik virtuaalmasinates (VM) oma kontot.|
|[Migreerimisel ning probleemide lahendamisel OS nõrkade kohtade](security-center-remediate-os-vulnerabilities.md)|Soovitab joondada oma OS konfiguratsioone soovitatav konfiguratsioon reegleid, nt võimaldab paroole salvestada.|
|[Süsteemi värskenduste rakendamine](security-center-apply-system-updates.md)|Soovitab juurutama VMs puuduvad süsteemi turbe- ja kriitilised värskendused.|
|[Taaskäivitage pärast süsteemi värskendused](security-center-apply-system-updates.md#reboot-after-system-updates)|Soovitab VM süsteemi värskenduste rakendamine protsessi lõpuleviimiseks taaskäivitada.|
|[Endpoint Protection installimine](security-center-install-endpoint-protection.md)|Soovitab, et olete ette ründevaratõrje programmid vms (ainult Windows VMs).|
|[Endpoint Protection seisund teatiste lahendamine](security-center-resolve-endpoint-protection-health-alerts.md)|Soovitab lahendada endpoint protection ebaõnnestumist.|
|[Luba VM Agent](security-center-enable-vm-agent.md)|Võimaldab teil näha, mis nõuavad VMs VM Agent. VM Agent peab olema installitud VMs selleks, et ettevalmistamise paik skannimine, võrdlusalus skannimine ja ründevaratõrje programmid. Vaikimisi installitakse VM Agent vms Azure'i turuplatsilt juurutatud. Artikli [VM Agent ja laiendid – osa 2](http://azure.microsoft.com/blog/2014/04/15/vm-agent-and-extensions-part-2/) teave VM agendi installimise kohta.|
| [Rakendada ketta krüptimist](security-center-apply-disk-encryption.md) |Soovitab teie VM ketast Azure'i ketta krüptimise (Windows ja Linux VMs) abil krüptida. Teie VM OS nii andmete maht on soovitatav krüptimine.|
| [Värskenduse OS versioon](security-center-update-os-version.md) | Soovitab värskendada oma OS pere jaoks oma pilveteenuses, et saadaval kõige uuem versioon operatsioonisüsteemi (OS) versioon.  Pilveteenustega kohta leiate lisateavet teemast [pilveteenustega ülevaade](../cloud-services/cloud-services-choose-me.md). |
| [Haavatavushindamises pole installitud](security-center-vulnerability-assessment-recommendations.md) | Soovitab lahenduse haavatavuse assessment installimine oma VM. |
| [Migreerimisel ning probleemide lahendamisel nõrkade kohtade](security-center-vulnerability-assessment-recommendations.md#review-recommendation) | Võimaldab teil näha süsteemi ja rakenduste nõrkade kohtade tuvasta installitud oma VM haavatavuse hindamise lahendus. |

## <a name="see-also"></a>Vt ka

Soovitusi, mida rakendada muid Azure ressursside kohta lisateabe saamiseks vaadake järgmist:

- [Azure'i turbekeskus rakendused kaitsmine](security-center-application-recommendations.md)
- [Võrgu Azure'i turbekeskus kaitsmine](security-center-network-recommendations.md)
- [Teie teenuse Azure SQL Azure'i turbekeskus kaitsmine](security-center-sql-service-recommendations.md)

Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – saate teada, kuidas konfigureerida turbepoliitikate Azure'i tellimused ja ressursside rühmad.
- [Haldamine ja turbeteatised Azure'i turbekeskus vastuvõtmist](security-center-managing-and-responding-alerts.md) --saate teada, kuidas hallata ja turbeteatised vastata.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
