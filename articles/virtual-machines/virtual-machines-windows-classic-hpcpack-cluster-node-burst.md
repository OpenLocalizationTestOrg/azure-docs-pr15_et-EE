<properties
 pageTitle="Lisada mõne HPC Pack kobar lõhkemist sõlmed | Microsoft Azure'i"
 description="Saate teada, kuidas laiendada mõne HPC Pack kobar Azure tellitavate rakenduses, lisades töötaja rolli aknad töötab pilveteenus"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="add-on-demand-burst-nodes-to-an-hpc-pack-cluster-in-azure"></a>Nõudmisel "plahvatuse" sõlmed lisamine on Azure HPC Pack kobar



Kui olete häälestanud [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) kobar Azure, võiksite nii kiiresti mastaapimiseks kobar võimsus üles või alla, säilitades eelkonfigureeritud MSDN VMs ilma. Selles artiklis kirjeldatakse nõudmisel "plahvatuse" sõlmed (töötaja rolli aknad töötab pilveteenus) lisamiseks nimega pea sõlme Azure Arvuta ressursse. 

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

![Burst sõlmed][burst]

Selles artiklis toodud juhiseid, mis aitavad teil Azure sõlmed kiiresti lisada pilvepõhist HPC Pack pea sõlme VM testi või tõendada mõiste juurutamine. Üksikasjalik toimingud on samad, mis "lõhkemise Azure" juhiseid lisada cloud arvutada võimet on kohapealse HPC Pack kobar. Juhendi, leiate [häälestamine hübriid arvutada kobar Microsoft HPC Pack](../cloud-services/cloud-services-setup-hybrid-hpcpack-cluster.md). Üksikasjalikke juhiseid ja tootmise juurutuste kohta leiate teemast [Azure Microsoft HPC Pack lõhkeda](https://technet.microsoft.com/library/gg481749.aspx).


## <a name="prerequisites"></a>Eeltingimused

* **HPC Pack pea sõlme juurutatud on Azure VM** – saate kasutada eraldiseisev pea sõlme VM või üks, mis on suurem kobar osa. Eraldiseisev pea sõlme loomiseks vaadake teemat [Deploy HPC Pack pea sõlm sisse on Azure VM](virtual-machines-windows-hpcpack-cluster-headnode.md). Vt automatiseeritud HPC Pack kobar Juurutussuvandid, [luua ja hallata Windows HPC kobar Azure Microsoft HPC Pack suvandid](virtual-machines-windows-hpcpack-cluster-options.md).

    >[AZURE.TIP] Kui [HPC Pack IaaS juurutamise skripti](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) abil saate luua klaster Azure, võite kaasata Azure lõhkemist sõlmed automatiseeritud juurutamise. Vaadake selle artikli näiteid.

* **Azure'i tellimuse** - lisada Azure sõlmed, saate valida ühe tellimuse juurutamiseks pea sõlme VM, kasutada või mõnda muud tellimust (või tellimuste).

* **Tuuma kvoodi** - peate suurendada kvoote valdkond, eriti siis, kui valite mitu Azure sõlme mitmesoonelised suurused juurutamiseks. Suurendamiseks kvoodi, [avage mõni Online'i kliendi tugiteenuse taotluse](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) tasuta.

## <a name="step-1-create-a-cloud-service-and-a-storage-account-for-the-azure-nodes"></a>Samm 1: Pilveteenus ja salvestusruumi konto Azure sõlmed loomine

Azure'i klassikaline portaali või samaväärne tööriistade abil saate konfigureerida järgmisi ressursse, mida on vaja oma Azure sõlmed juurutamiseks:

* Uue Azure pilveteenuses
* Uue konto Azure salvestusruum

>[AZURE.NOTE] Ärge kasutage olemasolevaid pilveteenuses, teie tellimus. 

**Kaalutlused**

* Konfigureerige eraldi pilveteenuses iga Azure sõlme malli, mida kavatsete luua. Siiski saate salvestusruumi sama konto jaoks mitu sõlme malle.

* Soovitame leidke pilveteenusesse ja salvestusruumi konto kasutuselevõtuks Azure piirkonna.




## <a name="step-2-configure-an-azure-management-certificate"></a>Samm 2: Azure'i halduse serdi konfigureerimine

Arvuta ressurssidena lisada Azure sõlmed, peate pea sõlme ja üles vastava Azure tellimusega juurutamise jaoks kasutatava serdi haldus sert.

Selle stsenaariumi korral saate valida **Vaikimisi HPC Azure'i Management sert** , mida HPC Pack installida ja konfigureerida automaatselt pea sõlme. See tunnistus on kasulik testimiseks eesmärgil ja tõendada mõiste juurutuste. Selle serdi kasutamiseks üles laadida faili C:\Program Files\Microsoft HPC Pack 2012\Bin\hpccert.cer pea sõlme VM tellimusega. Laadige üles sert [Azure klassikaline portaalis](https://manage.windowsazure.com), klõpsake nuppu **sätted** > **Halduse serdid**.

Halduse serdi konfigureerimine lisasuvandid, leiate [Azure'i halduse serdi Azure'i lõhkeda juurutuste konfigureerimiseks stsenaariumid](http://technet.microsoft.com/library/gg481759.aspx).

## <a name="step-3-deploy-azure-nodes-to-the-cluster"></a>Samm 3: Juurutada Azure sõlmed klaster



Lisada ja Alustage Azure sõlmed sel juhiseid üldjuhul sama, mis on kohapealse pea sõlme samme. Lisateavet leiate järgmistest jaotistest [juurutada Azure sõlmed Microsoft HPC Pack](https://technet.microsoft.com/library/gg481758.aspx)samme:

* Azure sõlme malli loomine

* Azure sõlmed lisamine Windows HPC kobar

* Käivitage (sätted) olevat Azure sõlmed

Pärast lisamist ja käivitage sõlmed, nad on valmis, saate kasutada kobar-tööde haldamine.

Kui juurutamine Azure sõlmed ilmneb probleeme, vaadake teemat [Tõrkeotsing juurutuste, Azure sõlmed Microsoft HPC Pack](http://technet.microsoft.com/library/jj159097.aspx).

## <a name="next-steps"></a>Järgmised sammud

* Arvuta mahukat eksemplari suurus lõhkemist sõlmed, lugege kaalutlused [H-sari ja Arvuta mahukat A-sarja VMs kohta](virtual-machines-windows-a8-a9-a10-a11-specs.md).

* Kui soovite automaatselt kasvata või Kahanda Azure arvuti ressursid vastavalt kobar töökoormus, lugege teemat [automaatselt kasvada ja vähendage Azure Arvuta ressursid on HPC Pack klaster](virtual-machines-windows-classic-hpcpack-cluster-node-autogrowshrink.md).

<!--Image references-->
[burst]: ./media/virtual-machines-windows-classic-hpcpack-cluster-node-burst/burst.png
