<properties
   pageTitle="Taastamise teenused võlvkelder FAQ | Microsoft Azure'i"
   description="KKK see versioon toetab avaliku eelvaateversiooniga teenuse Azure varukoopia. Vastused korduma kippuvatele küsimustele varukoopia agent, varundamine ja säilitamine, taastamine, turvalisus ja muude levinud küsimustele Azure varukoopia lahendus."
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="varukoopia lahendus; varukoopia teenus"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="10/21/2016"
     ms.author="trinadhk; markgal; jimpark;"/>

# <a name="recovery-services-vault---faq"></a>Taastamise teenuste hoidla - FAQ


Sellest artiklist leiate teavet teatud taastamise teenused vault ja see täiendab [Azure varundus KKK](backup-azure-backup-faq.md). Azure'i varundus KKK pakub täiskomplekti küsimuste ja vastuste Azure varukoopia teenuse kohta.  

Saate esitada küsimusi, kohta käesoleva artikli või seotud artikli jaotises Disqus Azure varukoopia. Samuti saate postitada küsimustele Azure varukoopia teenust [Foorum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup).

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>Taastamine teenuste võlvid on vastavalt ressursihaldur. Varundus võlvid (klassikaline režiim) veel toetatud? <br/>
Jah, varundamise võlvid on endiselt toetatud. [Klassikaline portaali](https://manage.windowsazure.com)loomine varundamise võlvid. Looge taastamise teenused võlvid [Azure portaali](https://portal.azure.com). Soovitame teil luua taastamise teenuste hoidla kõik tulevaste täiustused on siiski saadaval ainult taastamise teenused vault.

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>Varundus vault saate migreerimine võlvikus taastamise teenused? <br/>
Kahjuks ei, praegu ei saa migreerite sisu varundamise vault võlvikus taastamise teenused. Me tegeleme selle funktsiooni lisamine, kuid see ei ole saadaval avalike eelvaade osana.

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>Taastamise teenused võlvid ei toeta klassikaline VMs või ressursihaldur vastavalt VMs? <br/>
Taastamine teenuste võlvid toetavad mõlemad mudelid.  Saate varundada VM loodud klassikaline portaalis (mis on klassikaline režiim VMs) või VM loodud taastamise teenused vault Azure'i portaalis (mis ei põhine ressursihaldur).

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode--how-can-i-backup-them-in-recovery-services-vault"></a>Ma varundatud minu klassikaline VMs varukoopiate hoidla. Nüüd soovin migreerida minu VMs klassikaline režiim ressursihaldur režiim.  Kuidas neid rakenduses taastamise teenuste hoidla varundada?
Klassikaline VMs varukoopiate hoidla varukoopiaid ei migreerida automaatselt taastamise teenused hoidla kui migreerite VMs klassikaline ressursihaldur režiim. Palun järgmiste juhiste migreerimise VM varukoopiaid.

1. Varukoopiate hoidla, minge menüüsse **Kaitstud üksused** ja valige VM. Klõpsake [Peata kaitse](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines). Jätke *seotud varundatud andmete kustutamine* suvand **märkimata**.
2. Migreerige virtuaalse masina klassikaline režiim ressursihaldur režiim. Veenduge, et salvestusruumi ja võrgu vastav virtuaalse masina migreeritakse ka ressursihaldur režiim.
3. Taastamise teenuste hoidla loomine ja konfigureerige varundamise migreeritud virtuaalse masina **varukoopia** toimingu tegemine vault armatuurlaua abil. Lisateavet selle kohta, kuidas [lubada varukoopia taastamise teenuste hoidla](backup-azure-vms-first-look-arm.md)
