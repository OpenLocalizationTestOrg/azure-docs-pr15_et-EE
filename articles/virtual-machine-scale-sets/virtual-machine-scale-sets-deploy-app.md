<properties
    pageTitle="Rakenduse virtuaalse masina skaala komplekti juurutamine | Microsoft Azure'i"
    description="Virtuaalse masina skaala komplekti rakenduse juurutamine"
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gbowerman"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="guybo"/>

# <a name="deploy-an-app-on-virtual-machine-scale-sets"></a>Virtuaalse masina skaala komplekti rakenduse juurutamine

Rakendus töötab VM skaala määramine on tavaliselt paigutatud ühel järgmistest viisidest:

- Uue tarkvara installimine platvormi pilt juurutamise ajal. Seoses platform pildi on operatsioonisüsteem pilt Azure'i turuplatsilt, nagu Ubuntu 16.04, Windows Server 2012 R2 jne.

Saate installida uut tarkvara abil on [VM laiend](../virtual-machines/virtual-machines-windows-extensions-features.md)platvormi pildile. VM laiend on tarkvara, mis töötab VM juurutamisel. Mis tahes koodi teile meeldib, kasutades kohandatud skript laiend juurutamise ajal käivitada. [Siin](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale) on näide Azure'i ressursihaldur malli kahe VM laiendiga: Linux kohandatud skript laiend Apache ja PHP installimiseks ja diagnostika laiend eraldavad jõudlusandmeid Azure'i Autoscale kasutavad.

Seda moodust eelis on teil eraldamine oma rakenduse koodi ja OS vahel on ja rakenduse eraldi säilitada. Muidugi tähendab, et on ka rohkem liikuvad osad ja VM juurutamise aeg võib olla pikem kui seal on palju skripti alla laadida ja konfigureerida.

**Kui olete oma ostuõiguse tundlikku teavet (nt parool) oma kohandatud skript laiend käsku, olla kindel, et määrata selle `commandToExecute` sisse selle `protectedSettings` kohandatud skript laiend, mitte atribuut on `settings` atribuut.**

- Saate luua kohandatud VM pilt, mis sisaldab nii OS ja rakenduse ühe VHD. Siin skaala komplektis kogumi VMs kopeeritud pildi teie poolt loodud, mis tuleb säilitada. Seda moodust nõuab pole täiendav konfigureerimine VM juurutamise ajal. Siiski soovitud `2016-03-30` versioon VM skaala komplekti (ja varasemad versioonid), on piiratud ühe salvestusruumi konto OS ketast vms skaala määramine. Seega saate olla kuni 40 VMs skaala komplekti, 100 VM kohta skaala asemel määrata limiit platvormi piltidega. Üksikasjalikumat teavet teemast [Skaala seadmine kujundus ülevaade](./virtual-machine-scale-sets-design-overview.md) .

- Platvormi või kohandatud pilt, mis on põhimõtteliselt container host juurutada ja ühe või mitme ümbriste orchestrator või konfiguratsiooni halduse tööriista haldavatele rakenduse installima. Seda moodust tore asi on, et on võetud oma pilvetaristu kaudu rakenduse kiht ja saate neid eraldi säilitada.

## <a name="what-happens-when-a-vm-scale-set-scales-out"></a>Mis juhtub, kui VM skaala määrata skaala välja?

Ühe või mitme VMs lisamisel määratud suurendada skaala – kas käsitsi või autoscale – rakendus installitakse automaatselt. Näiteks kui skaala on määratletud laiendid, töötada uue VM iga kord, kui see on loodud. Kui skaala määramine põhineb kohandatud pilt, mis tahes uue VM on kohandatud Lähtepildi koopia. Kui skaala määratud VMs on container hosts, siis peate võib-olla käivitus kood laadimiseks ümbriste kohandatud skript laiendamine või laiend võib installida agent, mille registreerib kobar orchestrator (nt Azure'i teenus).

## <a name="how-do-you-manage-application-updates-in-vm-scale-sets"></a>Kuidas hallata rakenduse värskendused VM skaala kogumitena?

Rakenduse värskenduste VM skaala kogumitena, tehke kolm peamist lähenemisviisi kolme eelmise rakenduse juurutamine meetodi kaudu.

* Värskendamise VM laiendiga. Mis tahes VM laiendid, mis on määratletud VM skaala seadmine täidetakse mõne olemasoleva VM iga kord, kui uus VM on juurutatud, reimaged või VM laiend värskendatakse. Kui peate värskendama rakenduse, otse rakenduse kaudu laiendid värskendamine on otstarbekas lähenemine – värskendate lihtsalt laiend määratlus. Üks lihtne viis selleks on, muutes fileUris osutamiseks uue tarkvara.

* Püsiv kohandatud pildi moodust. Kui te bake rakendus (või rakenduse komponendid) VM pildi saate keskenduda koostamise usaldusväärne kohaletoimetamisel Koosta, testi ja pildid juurutamise automatiseerimiseks. Saate kujundada oma arhitektuur hõlbustamiseks kiire vahetamine tootmisse etapiviisilise skaala kogum. Seda moodust hea näide on [Azure Spinnaker draiver töö](https://github.com/spinnaker/deck/tree/master/app/scripts/modules/azure) - [http://www.spinnaker.io/](http://www.spinnaker.io/).

Pakendaja ja Terraform toetavad ka Azure ressursihaldur, nii saate määratleda oma pilte "kood" ja neid koostada Azure, seejärel kasutada selle VHD oma skaala määramine. Siiski teha nii muutuks probleemne turuplatsi pilte, kus laiendid/kohandatud skriptide muutuvad tähtsamatele, kuna te ei töödelda otse bittide turuplatsilt.

* Ümbriste värskendada. Abstraktne rakenduse elutsükli haldus tasemele kohal pilvetaristu, näiteks kapseldamist rakendused ja rakenduse komponendid sisse ümbriste ja hallata neid ümbriste container orchestrators ja konfiguratsiooni haldurid nagu Chef/nuku kaudu.

Ulatuse määramine VMs siis muutuvad ühed substraat on ümbriste ja nõuda ainult aeg-ajalt turbe- ja OS seotud värskendusi. Nagu mainitud, on Azure Container teenus on hea näide seda lähenemisviisi ja koostamise teenuse ümber.

## <a name="how-do-you-roll-out-an-os-update-across-update-domains"></a>Kuidas te ärirakenduseks OS värskendus update domeenides?

Oletame, et soovite värskendada oma OS pilt säilitades VM skaala seadmine töötab. Üks viis selleks on värskendada VM pilte ühe VM korraga. Saate seda teha PowerShelli või Azure CLI. On eraldi käsud VM skaala seadmine mudeli (kui selle konfiguratsioon on määratletud) värskendamiseks ja väljaandmisel "käsitsi uuendamine" kutsub üksikute VMs.

[Siin](https://github.com/gbowerman/vmsstools) on näide Python skripti, mis automatiseerib protsessi uuendamine VM skaala määrata ühe värskenduse domeeni korraga. (Hoiatus: see on mitu tõendada kontseptsiooni kui kõva tootmise valmis lahenduse – võiksite lisada mõne tõrge veakontrolli jne.).
