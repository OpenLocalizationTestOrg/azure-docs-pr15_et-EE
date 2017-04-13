<properties
    pageTitle="Sissejuhatus Azure'i DPM varundus | Microsoft Azure'i"
    description="DPM serverid Azure varukoopia teenuse kasutamist varundada tutvustus"
    services="backup"
    documentationCenter=""
    authors="Nkolli1"
    manager="shreeshd"
    editor=""
    keywords="Süsteemi andmete kaitse Manager, andmete kaitse manager, dpm varundamine"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="trinadhk;giridham;jimpark;markgal"/>

# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>Varundage töökoormus Azure DPM-i ettevalmistamine

> [AZURE.SELECTOR]
- [Azure varukoopia Server](backup-azure-microsoft-azure-backup.md)
- [SCDPM](backup-azure-dpm-introduction.md)
- [Azure varukoopia Server (klassikaline)](backup-azure-microsoft-azure-backup-classic.md)
- [SCDPM (klassikaline)](backup-azure-dpm-introduction-classic.md)


Selles artiklis antakse Sissejuhatus Microsoft Azure'i varundamise kaitsta teie süsteemi Center andmete kaitse Manager (DPM) serverites ja töökoormus. Lugedes, saate aru:

- Azure'i DPM serveri varukoopia tööpõhimõtted
- Eeltingimused saavutamiseks sujuvate varukoopia kogemus
- Tüüpilised ilmnenud tõrgete ja kuidas neid
- Toetatud stsenaariumid

System Center DPM varundab faili- ja rakenduse andmeid. Andmeid varundada DPM saate salvestatud lindile kettal, või Azure Microsoft Azure'i varundamise varundada. DPM suhtleb Azure varukoopia järgmiselt:

- **DPM juurutatud füüsilise serveri või kohapealse virtuaalse masina** – kui DPM juurutatakse füüsilise serveri või kohapealse Hyper-V virtuaalse masina saate varundada andmed on Azure varukoopiate hoidla Lisaks kettale ja lindile varukoopia.
- **Azure'i virtuaalarvuti juurutatud DPM** – kaudu System Center 2012 R2 Update 3, mis on Azure virtuaalse masina saab kasutada DPM. Kui DPM juurutatud Azure virtuaalse masina, mis te saate varundada andmeid Azure ketast lisatud DPM Azure virtuaalse masina või andmete talletamine offload toetuseta ta on Azure varukoopia vault ülespoole.

## <a name="why-backup-your-dpm-servers"></a>Miks varundada oma DPM servers?

Azure'i varundamise varundamiseks DPM serverid business eelised on järgmised.

- Kohapealse DPM juurutamiseks saate kasutada alternatiivina pikaajalise juurutamise lindile Azure'i varukoopiad.
- DPM juurutuste Azure, Azure varundus võimaldab salvestusruumi Azure'i ketas, et võimaldab skaala kuni talletades vanemate andmete Azure varukoopia ja uute andmete kettale.

## <a name="how-does-dpm-server-backup-work"></a>DPM serveri varukoopia tööpõhimõtted
Varundage virtuaalse masina, esmalt hetktõmmise punkti õigel ajal andmeid on vaja. Azure'i varukoopia teenus käivitab Varundustöö määratud ajal ja käivitab varukoopia laiend hetktõmmis. Varukoopia laiend-Külastajate VSS teenuse järjepidevuse saavutamiseks koordinaadid ja käivitab bloobimälu hetktõmmise API Azure Storage teenuse pärast järjepidevuse saavutamist. Seda saada ühtsete hetktõmmist ketast virtuaalse masina, ilma vajaduseta sulgema.

Pärast hetktõmmis on tehtud, andmed on üle Azure varukoopia teenuse varukoopiate hoidla. Teenuse hoolitseb tuvastamine ja kanda plokid, mis on muutunud viimase varukoopia tegemine varukoopiate salvestusruumi ja võrgu tõhusa. Kui andmeedastus on lõpule viidud, eemaldatakse hetktõmmis ja taastamise punkti on loodud. Siinkohal taastamine näha Azure klassikaline portaalis.

>[AZURE.NOTE] Linux virtuaalmasinates, on võimalik ainult faili ühtsete varukoopia.

## <a name="prerequisites"></a>Eeltingimused
Ettevalmistused Azure varukoopia DPM varundada järgmiselt:

1. **Loo varundus vault** – luua võlvkelder Azure varukoopia konsooli.
2. **Laadi alla hoidla mandaat** – Azure varukoopia, laadige vault loodud serdi haldus.
3. **Installige Azure'i varukoopia Agent ja registreerimist server** –: Azure'i varukoopia, installige agent iga DPM server ja registreerida DPM serveri varukoopiate hoidla.

[AZURE.INCLUDE [backup-create-vault](../../includes/backup-create-vault.md)]

[AZURE.INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[AZURE.INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]


## <a name="requirements-and-limitations"></a>Nõuded (ja piirangud)

- DPM saate töötama füüsilise serveri või Hyper-V virtuaalse masina System Center 2012 SP1 või System Center 2012 R2 installitud. Saate ka töötama nimega Azure virtuaalse masina, mis töötab System Center 2012 R2 vähemalt DPM 2012 R2 Update Rollup 3 või töötab System Center 2012 R2 vähemalt VMWare virtuaalse masina Windows Update Rollup 5.
- Kui kasutate DPM System Center 2012 SP1, installige värskendus Rollup 2 System Center andmete kaitse Manager SP1. See on vajalik, enne kui saate installida Azure varundus Agent.
- DPM server peaks olema Windows PowerShell ja .net Framework 4.5 installitud.
- DPM saate varundada enamik töökoormus Azure varukoopia. Mis on toetatud, vaadake teemat täieliku loendi leiate Azure'i varundus toeta üksuste allpool.
- Andmed salvestatakse Azure varukoopia ei saa taastada suvandiga "koopia lindile".
- Peate Azure'i konto Azure varukoopia funktsioon on sisse lülitatud. Kui teil pole kontot, saate luua tasuta prooviversiooni konto vaid paar minutit. Lugege [hinnad Azure varukoopia](https://azure.microsoft.com/pricing/details/backup/).
- Azure'i varundamise nõuab Azure varundus Agent olema installitud serverid, mida soovite varundada. Iga server peab olema vähemalt 10% suuruse andmed, mida on varundab, kui kohalik tasuta salvestusruumi. Näiteks 100 GB andmeid varundada nõuab vähemalt 10 GB vaba ruumi nullist asukohta. Minimaalne on 10%, on soovitatav 15% tasuta kohaliku salvestusruumi vahemälu asukoha jaoks kasutada.
- Andmed salvestatakse Azure vault salvestusruumi. Saate varundada on Azure varukoopia võlvkelder andmehulga mingeid piiranguid, kuid andmeallikaga (nt virtuaalse masina või andmebaasi) suurust ei tohiks ületa 54,400 GB.

Järgmist tüüpi failide on toetatud tagasi registriredaktori Azure:

- Krüptitud (täielik varukoopiad ainult)
- Tihendatud (varundamiseks toetatud)
- Sparse (varundamiseks toetatud)
- Tihendatud ja vähe (koheldakse kui Sparse)

Ja neid ei toetata.

- Serverite tõstutundlik failisüsteemi ei toetata.
- Suur lingid (vahelejäetud)
- Liigenda points (vahelejäetud)
- Krüptitud ja tihendatud (vahelejäetud)
- Krüptitud ja vähe (vahele)
- Tihendatud voo
- Vähe voo

>[AZURE.NOTE] Kaudu rakenduses System Center 2012 DPM SP1, kus saate varundada üles töökoormus kaitstud DPM Azure kasutades Microsoft Azure varukoopia.
