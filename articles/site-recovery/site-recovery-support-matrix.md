<properties
    pageTitle="Azure'i saidi taastamine tugi maatriksi | Microsoft Azure'i"
    description="Azure'i saidi taastamise jaoks on toetatud opsüsteemid ja komponentide Kokkuvõte"
    services="site-recovery"
    documentationCenter=""
    authors="rayne-wiselman"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="site-recovery"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="storage-backup-recovery"
    ms.date="10/05/2016"
    ms.author="raynew"/>

# <a name="azure-site-recovery-support-matrix"></a>Azure'i saidi taastamine tugi maatriks

Selles artiklis on toodud toetatud opsüsteemid ja Azure saidi taastamise juurutuste komponendid. Loendi eeltingimuste ja toetatud komponendid on saadaval iga juurutamise stsenaariumi puhul iga vastava juurutamise artikli ja selles dokumendis on toodud need.

## <a name="supported-operating-systems-for-virtualization-servers"></a>Toetatud opsüsteemid virtualization serverite jaoks


**Allikas** | **Target (sihtkoht)** | **Host OS**
---|---|---|--- 
**Hyper-V hosts (ilma VMM)** | Azure'i | Windows Server 2012 R2 koos uusimad värskendused
**Hyper-V hosts (koos VMM)** | Azure'i | Windows Server 2012 R2 koos uusimad värskendused
**Hyper-V hosts (koos VMM)** | VMM saidi | At vähemalt Windows Server 2012 uusimad värskendused
**Vmware'i hosts/Vcenteri** | Azure'i | vCenter 5,5 või 6.0 (ainult 5,5 funktsioonide tugi) <br/><br/> vSphere 6.0, 5,5 või 5.1 uusimate värskenduste abil
**Vmware'i hosts/Vcenteri** | VMware saidi | vCenter 5,5 või 6.0 (ainult 5,5 funktsioonide tugi) <br/><br/> vSphere 6.0, 5,5 või 5.1 uusimate värskenduste abil

## <a name="supported-requirements-for-replicated-machines"></a>Tiražeeritud masinad toetatud nõuded

**Allikas** | **Mis on kopeeritud** | **Target (sihtkoht)** | **Host OS**
---|---|---|--- 
**Hyper-V VMs** | Mis tahes töökoormus | Azure'i | Mis tahes Külastajate OS [Azure'i ei toeta](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> VMs peab vastama [Azure nõuded](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**Hyper-V VMs (koos VMM)** | Mis tahes töökoormus | Azure'i | Mis tahes Külastajate OS [Azure'i ei toeta](https://technet.microsoft.com/library/cc794868.aspx)<br/><br/> VMs peab vastama [Azure nõuded](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**Hyper-V VMs (koos VMM)** | Mis tahes töökoormus | VMM saidi | Mis tahes Külastajate OS [ei toeta Hyper-V](https://technet.microsoft.com/library/mt126277.aspx)
**VMware VMs** | Mis tahes töökoormus, kus töötab Windows VM | Azure'i | 64-bitine Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 hoolduspaketiga veebisaidil vähemalt SP1<br/><br/> VMs peab vastama [Azure nõuded](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**VMware VMs** | Mis tahes töökoormus töötab Linux VM | Azure'i | Punane rolli Enterprise Linux 6,7, 7.1, 7.2 <br/><br/> CentOS 6.5, 6.6 6,7, 7.0, 7.1, 7.2 <br/><br/> Oracle'i Enterprise Linux 6.4 6.5 operatsioonisüsteemiga punane rolli ühilduvad tuum või purunematu Enterprise tuum väljaanne 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Salvestusruumi nõutav: Hajusfailisüsteemiga (EXT3, ETX4, ReiserFS, XFS); Silma tarkvara-seadme plaanuri (silma)); Helitugevuse halduri: (LVM2). HP CCISS kontrolleril salvestusruumi füüsilise serverite ei toetata. ReiserFS failisüsteemi toetatakse ainult SUSE Linux Enterprise Server 11 SP3.<br/><br/> VMs peab vastama [Azure nõuded](site-recovery-best-practices.md#azure-virtual-machine-requirements)
**VMware VMs** | Mis tahes töökoormus, kus töötab Windows VM | VMware saidi | 64-bitine Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 hoolduspaketiga veebisaidil vähemalt SP1
**VMware VMs** | Mis tahes töökoormus töötab Linux VM | VMware saidi | Punane rolli Enterprise Linux 6,7, 7.1, 7.2 <br/><br/> CentOS 6.5, 6.6 6,7, 7.0, 7.1, 7.2 <br/><br/> Oracle'i Enterprise Linux 6.4 6.5 operatsioonisüsteemiga punane rolli ühilduvad tuum või purunematu Enterprise tuum väljaanne 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Salvestusruumi nõutav: Hajusfailisüsteemiga (EXT3, ETX4, ReiserFS, XFS); Silma tarkvara-seadme plaanuri (silma)); Helitugevuse halduri: (LVM2). HP CCISS kontrolleril salvestusruumi füüsilise serverite ei toetata. ReiserFS failisüsteemi toetatakse ainult SUSE Linux Enterprise Server 11 SP3.
**Füüsilise serveri** | Mis tahes töökoormus, kus töötab Windows | Azure'i | 64-bitine Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 veebisaidil vähemalt SP1
**Füüsilise serveri** | Mis tahes töökoormus töötab Linux | Azure'i | Punane rolli Enterprise Linux 6.7,7.1,7.2 <br/><br/> CentOS 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle'i Enterprise Linux 6.4 6.5 operatsioonisüsteemiga punane rolli ühilduvad tuum või purunematu Enterprise tuum väljaanne 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Salvestusruumi nõutav: Hajusfailisüsteemiga (EXT3, ETX4, ReiserFS, XFS); Silma tarkvara-seadme plaanuri (silma)); Helitugevuse halduri: (LVM2). HP CCISS kontrolleril salvestusruumi füüsilise serverite ei toetata. ReiserFS failisüsteemi toetatakse ainult SUSE Linux Enterprise Server 11 SP3.
**Füüsilise serveri** | Mis tahes töökoormus, kus töötab Windows | Saidi | 64-bitine Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 veebisaidil vähemalt SP1
**Füüsilise serveri** | Mis tahes töökoormus töötab Linux | Saidi | Punane rolli Enterprise Linux 6.7,7.1,7.2 <br/><br/> CentOS 6.5, 6.6,6.7,7.0,7.1,7.2 <br/><br/> Oracle'i Enterprise Linux 6.4 6.5 operatsioonisüsteemiga punane rolli ühilduvad tuum või purunematu Enterprise tuum väljaanne 3 (UEK3) <br/><br/> SUSE Linux Enterprise Server 11 SP3 <br/><br/> Salvestusruumi nõutav: Hajusfailisüsteemiga (EXT3, ETX4, ReiserFS, XFS); Silma tarkvara-seadme plaanuri (silma)); Helitugevuse halduri: (LVM2). HP CCISS kontrolleril salvestusruumi füüsilise serverite ei toetata. ReiserFS failisüsteemi toetatakse ainult SUSE Linux Enterprise Server 11 SP3.


## <a name="provider-versions"></a>Pakkuja versioonid

**Nimi** | **Kirjeldus** | **Uusim versioon** | **Tugi** | **Üksikasjad**
---|---|---|---| ---
**Azure'i saidi taastamise pakkuja** | Koordinaate kohapealsed serverid ja Azure/teisene saidi vahelise suhtluse <br/><br/> Kohapealse VMM serverites või Hyper-V serverite installitud, kui ei ole VMM server | 5.1.1700 (saadaval portaalist) | [Uusimate funktsioonide ja parandused](https://support.microsoft.com/kb/3155002)
**Azure'i saidi taastamise ühendatud Setup (Vmware'i Azure'i)** | Kohapealse VMware serverite ja Azure vahelise andmevahetuse koordinaatide alusel <br/><br/> Kohapealse VMware serveritesse installida | 9.3.4246.1 (saadaval portaalist) | [Uusimate funktsioonide ja parandused](https://support.microsoft.com/kb/3155002)
**Mobiilsus teenuse** | Koordinaate dispersioonanalüüs kohapealsed VMware serverite/füüsilise serverid ja Azure/teisene saidi vahel | NA (saadaval portaalist) | Installitud iga Vmware'i VM või füüsiline server, mida soovite korrata. **Microsoft Azure'i taastamine (MARS) agent** | Hyper-V VMs ja Azure koordinaate dispersioonanalüüs<br/><br/> Installitud kohapealse Hyper-V serverite (koos või ilma VMM server) | 2.0.8689.0 (saadaval portaalist) | See agent on kasutusel Azure'i saidi varundus ja taaste Azure). [Lisateave] (https://support.microsoft.com/en-us/kb/2997692)

## <a name="next-steps"></a>Järgmised sammud

[Juurutamise ettevalmistamine](site-recovery-best-practices.md)

