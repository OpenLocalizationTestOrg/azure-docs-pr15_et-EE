 (varukoopia võlvid<properties
   pageTitle = "Azure Backup limits table"
   kirjeldus = "kirjeldatakse süsteemi piirangud Azure Backup."
   services="backup"
   documentationCenter="NA"
   authors="Jim-Parker"
   manager="jwhit"
   editor="" />
<tags
   ms.service="backup"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="10/05/2015"
   ms.author="trinadhk";"jimpark"; "aashishr" />


Järgmised piirangud kehtivad Azure varukoopia.

| Piiratud identifikaator | Vaikimisi limiit |
|---|---|
|Arvu serverid/seadmed registreerida iga vault suhtes.|Windows Serveri/kliendi/SCDPM 50 <br/> 200 IaaS vms|
|Andmed salvestatakse Azure vault salvestusruumi andmeallika suurus|54400 GB max<sup>1</sup>|
|Varukoopia võlvid iga Azure'i tellimus loodud arv|25 (varundamise võlvid) <br/> 25 taastamise teenused võlvkelder müüginäitajaid piirkonna kohta|
|Mitu korda päevas saab ajastada varundamine|3 päevas Windows Server/klient <br/> 2 SCDPM päevas <br/> Kord päevas IaaS vms|
|Andmete ketast manustatud Azure'i virtuaalarvuti varundamine|16|

- <sup>1</sup> 54400 GB limiit ei kehti IaaS VM varukoopia.

