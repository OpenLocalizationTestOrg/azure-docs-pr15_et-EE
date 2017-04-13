<properties
   pageTitle="Rakendada ketta krüptimist Azure'i Security Center | Microsoft Azure'i"
   description="Selle dokumendi näidatakse, kuidas rakendada Azure'i turbekeskus soovitust **Rakenda ketta krüptimise**."
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
   ms.date="07/29/2016"
   ms.author="terrylan"/>

# <a name="apply-disk-encryption-in-azure-security-center"></a>Azure'i turbekeskus ketta krüptimine rakendamine

Azure'i turbekeskus soovitada rakendamist ketta krüptimise, kui teil on Windows või Linux VM ketast, mis on krüptitud Azure'i ketta krüptimise. Ketta krüptimise abil saate oma Windowsi ja Linux IaaS VM ketast krüptida.  Teie VM OS nii andmete maht on soovitatav krüptimine.


Ketta krüptimise mõjutab valdkonna standard [BitLockeri](https://technet.microsoft.com/library/cc732774.aspx) funktsioon Windowsi ja Linux OS- ja krüptimise kaitsta ja oma andmeid ning oma ettevõtte Turve ja nõuetele vastavus kohustusi esitada [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) funktsioon. [Azure'i klahvi Vault](https://azure.microsoft.com/documentation/services/key-vault/) aitab teil määrata ja hallata ketta krüptimise võtmed ja saladusi tellimuse võti Vault, tagades, et VM ketta kõik andmed on krüptitud [Azure Storage](https://azure.microsoft.com/documentation/services/storage/)korral on integreeritud ketta krüptimist.

> [AZURE.NOTE] Azure'i ketta krüptimise on toetatud järgmised Windows Serveri operatsioonisüsteemid - Windows Server 2008 R2, Windows Server 2012 ja Windows Server 2012 R2. Ketta krüptimise on toetatud Linux serveri operatsioonisüsteemide - Ubuntu, CentOS, SUSE ja SUSE Linux Enterprise Server (SLES).

## <a name="implement-the-recommendation"></a>Soovituse

1. **Soovitused** tera, valige **Rakenda ketta krüptimine**.
2. **Rakenda ketta krüptimise** tera, kuvatakse loendi ketta krüptimise on soovitatav VMs.
3. Järgige krüptimist rakendamiseks need VMs.

![][1]

Azure'i Virtuaalmasinates on kindlaks teinud turbekeskus enam vaja krüptimise krüptimiseks soovitame järgmist:

- Installima ja konfigureerima Azure PowerShelli. See võimaldab teil häälestama nõutav Azure'i Virtuaalmasinates krüptimiseks eeltingimused PowerShelli käskude käivitamiseks.
- Hankige ja Azure ketta krüptimise eeltingimused Azure PowerShelli skripti.
- Krüptige oma virtuaalmasinates.

[Krüpti on Azure virtuaalse masina](security-center-disk-encryption.md) juhendab teid järgmist.  See teema eeldab, et kasutate Windows 10, kust tuleb konfigureerida ketta krüptimist klientarvutis.

On mitmeid võimalusi, mida saab kasutada eeltingimused häälestada ja konfigureerida krüptimise Azure'i Virtuaalmasinates. Kui olete juba hästi kursis Azure PowerShelli või Azure CLI, siis võib-olla eelistate kasutada alternatiivse võimalustest. Muud võimalused need Lisateavet leiate teemast [Azure ketta krüptimine](../security/azure-security-disk-encryption.md).



## <a name="see-also"></a>Vt ka

Selle dokumendi ilmnes saate kuidas rakendada turbekeskus soovitust "Rakenda ketta krüptimise." Ketta krüptimise kohta leiate lisateavet teemast järgmist:

- [Krüptimise ja Azure klahvi Vault võtme juhtimine](https://azure.microsoft.com/documentation/videos/azurecon-2015-encryption-and-key-management-with-azure-key-vault/) (video 36 min 39 SEK) – siit saate teada, kuidas kasutada krüptimise IaaS VMs ja Azure klahvi Vault kaitsta ning oma andmeid.
- [Azure'i virtuaalarvuti krüptimine](security-center-disk-encryption.md) (dokument) – siit saate teada, kuidas krüptida Azure'i Virtuaalmasinates.
- [Azure'i ketta krüptimine](../security/azure-security-disk-encryption.md) (dokument) – siit saate teada, kuidas Windows ja Linux VMs ketta krüptimine.

Lisateavet turbekeskus, leiate järgmistest:

- [Säte turbepoliitikate Azure'i Security Center](security-center-policies.md) – siit saate teada, kuidas konfigureerida turbepoliitikate.
- [Turvalisus seisundi jälgimine Azure'i turbekeskus](security-center-monitoring.md) – saate teada, kuidas oma Azure ressursse seisundi jälgimine.
- [Haldamine ja turbeteatised Azure'i turbekeskus vastuvõtmist](security-center-managing-and-responding-alerts.md) --saate teada, kuidas hallata ja turbeteatised vastata.
- [Azure'i turbekeskus soovitused haldamine turvalisuse kohta](security-center-recommendations.md) – siit saate teada, kuidas soovitused aitab teil kaitsta oma Azure ressursse.
- [Azure'i turvalisus Center KKK](security-center-faq.md) – Otsi korduma kippuvad küsimused teenuse kasutamise kohta.
- [Azure'i turvalisus ajaveebi](http://blogs.msdn.com/b/azuresecurity/) --Otsi ajaveebi postituste Azure Turve ja nõuetele vastavuse kohta.



<!--Image references-->
[1]: ./media/security-center-apply-disk-encryption/apply-disk-encryption.png
