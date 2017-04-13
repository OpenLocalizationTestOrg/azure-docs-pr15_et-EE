<properties
   pageTitle="Logide kogumise Linux Azure'i diagnostika abil | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse, kuidas häälestada Azure'i diagnostika logid kogumine töötab Azure teenuse struktuuri Linux klaster."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/28/2016"
   ms.author="subramar"/>


# <a name="collect-logs-by-using-azure-diagnostics"></a>Logide kogumise Azure'i diagnostika abil

> [AZURE.SELECTOR]
- [Windows](service-fabric-diagnostics-how-to-setup-wad.md)
- [Linux](service-fabric-diagnostics-how-to-setup-lad.md)

Kui teil on mõni Azure teenuse struktuuri kobar, on mõistlik koguda logid kõik sõlmed ühes keskses kohas. Heliprobleemide logid ühes keskses kohas hõlbustab analüüsimiseks ja tõrkeotsing, kas need on teie teenuseid, rakenduse või klaster ise. Üks võimalus üles laadida ja kogumise logid on lisatud logid Azure Storage Azure'i diagnostika laienduse kasutada. Saate lugeda sündmuste salvestusruumi ja paigutamiseks toode nagu [Elastne otsingu](service-fabric-diagnostic-how-to-use-elasticsearch.md) või log sõelumine muu lahendus.

## <a name="log-sources-that-you-might-want-to-collect"></a>Logi allikad, mida võiksite kogumise
- **Teenuse struktuuri logid**: nõrgunud platvormi kaudu [LTTng](http://lttng.org) ja konto salvestusruumi üles laadida. Logide saab töö, sündmuste või käitusaja sündmused, mis eraldab platvormi. Need logid on talletatud kobar manifest määrab soovitud asukohta. (Salvestusruumi konto üksikasjade vaatamiseks sildi **AzureTableWinFabETWQueryable** otsida ja otsige **StoreConnectionString**.)
- **Rakenduse sündmused**: nõrgunud oma teenuse kood. Saate kasutada mis tahes logimine lahenduse, mis kirjutab tekstipõhine logifailid – näiteks LTTng. Lisateavet leiate rakenduse jälgimise kohta LTTng dokumentatsioonist.  


## <a name="deploy-the-diagnostics-extension"></a>Diagnostika laiend juurutamine
Esimene samm kogumise logid on juurutada diagnostika laienduse iga teenuse struktuuri kobar VM. Diagnostika laiend kogub logid iga VM ja lisatud neile salvestusruumi kontole määratud. Juhised sõltuvad sellest, kas kasutate Azure portaali või Azure ressursihaldur.

Diagnostika laiend juurutamiseks klaster VM osana kobar loomine seatud **diagnostika** **kohta**. Kui olete loonud klaster, ei saa seda sätet muuta portaali kaudu.

Konfigureerige Linux Azure'i diagnostika (POISI koguda failid ja paigutada need salvestusruumi kontole). Selle protsessi on selgitatud nimega stsenaarium 3 ("üles oma logifailid") artiklis [Jälgida ja Linux VMs diagnoosimine POISI abil](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md). Pärast seda protsessi saab teile juurdepääsu jälgi. Saate üles laadida jälgi visualizer oma valik.

Azure'i ressursihaldur abil saate juurutada diagnostika laiendamine. Protsess on sarnane Windows ja Linux ja on dokumenteerida jaoks Windowsi kogumite, [Kuidas koguda Azure'i diagnostika logid](service-fabric-diagnostics-how-to-setup-wad.md).

Samuti saate Operations Management Suite, nagu on kirjeldatud [Toimingud halduse komplekti Log Analytics Linux](https://blogs.technet.microsoft.com/hybridcloud/2016/01/28/operations-management-suite-log-analytics-with-linux/).

Kui olete selle konfiguratsiooni, POISI agent jälgib määratud logifailid. Iga kord, kui uus rida on lisatud faili, loob see Logi kirje, mis saadetakse teie määratud salvestusruumi.


## <a name="next-steps"></a>Järgmised sammud
Mõista, millised sündmused, siis tuleks uurida samas probleemide tõrkeotsingu artiklist [LTTng dokumentatsiooni](http://lttng.org/docs) ja [POISI abil](../virtual-machines/virtual-machines-linux-classic-diagnostic-extension.md).
