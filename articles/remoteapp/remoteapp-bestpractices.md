<properties
    pageTitle="Azure RemoteApp head tavad | Microsoft Azure'i"
    description="Head tavad konfigureerimine ja kasutamine Azure RemoteApp."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin" />

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="elizapo" />

# <a name="best-practices-for-configuring-and-using-azure-remoteapp"></a>Konfigureerimise ja Azure RemoteApp kasutamise head tavad

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Järgmine teave aitab teil konfigureerimine ja kasutamine Azure RemoteApp produktiivselt.

## <a name="connectivity"></a>Ühenduvus


- Kasuta alati kliendi uusim versioon. Kasutamine vanem kliendid võib põhjustada ühenduvusprobleemide ja teiste halvenenud kogemusi. Lubada automaatse rakenduste uuendusi oma seadme tagada uusimate kliendi installitakse alati.
- Kasuta alati kõige ühed ja usaldusväärne Interneti-ühendus on saadaval.  
- Kasutage toetatud ainult puhverserveri ühendused ühenduvuse optimaalse jõudluse.  Puhverserveri sokid ei toetata.

## <a name="applications"></a>Rakenduste


- Salvestage ja sulgege RemoteApp rakenduste, kui olete valmis rakenduse. Andmete kaotsimineku võib põhjustada ei sulgeda rakendus.
- Kinnitage enne kasutamine Azure RemoteApp kohandatud rakendused. See hõlmab need töötada mitme seansi platvormi ja ära kasutada mittevajalike ressursid, nt mälu ja CPU, mis võivad nälga teise kasutaja sama. Lisateavet leiate alla laadida ja vaadata [Rakenduse ühilduvus head tavad Kaugtöölauateenuseid](http://www.dabcc.com/resources/Application%20Compatibility%20Best%20Practices%20for%20Remote%20Desktop%20Services.pdf).

## <a name="configuration-and-management"></a>Konfigureerimine ja haldamine


- Ajakohastamine teie malli pilte, installides tarkvaravärskendused ja muud kriitilised parandused vastavalt vajadusele. See tagab, et Azure RemoteApp automaatne-skaalasid täita teie võimsus, nagu on paigatud iga eksemplari.  
- Veenduge, et teie Active Directory Federation Services (AD FS) juurutusega on turvaline ja usaldusväärne. Muul juhul kliendi isikutuvastusi võib nurjuda, takistab kasutajate juurdepääsu Azure RemoteApp.
- Konfigureerida mallide piltide nii, et need on installitud rakendused, rollide või funktsioonid. Need ärge klõpsake esinemisvõimaluse virtuaalmasinates RemoteApp teenus, mis on püsivate olekus.
    - Hoida kõik kasutajate andmed kasutajaprofiilide või mujal salvestusruumi välise teenusega, näiteks kohapealse faili aktsiad või OneDrive.
    - Ühiskasutusega andmete talletamiseks salvestusruumi asukohad välise teenusega, näiteks kohapealse faili aktsiad või OneDrive.
    - Mis tahes kogu süsteemi sätteid konfigureerida, klõpsake malli pilti, mitte üksikute virtuaalmasinates mõnes muus teenuses.
    - Tarkvara automaatse uuendamise avaldatud rakenduste keelamine – selle asemel rakendada neid käsitsi malli pilti ja need enne juurutamist malli testida.
