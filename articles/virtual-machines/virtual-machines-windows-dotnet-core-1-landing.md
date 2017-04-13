<properties
   pageTitle="Azure virtuaalse masina DotNet Core Kuueosalisest 1 | Microsoft Azure'i"
   description="Azure virtuaalse masina DotNet Core õpetus"
   services="virtual-machines-windows"
   documentationCenter="virtual-machines"
   authors="neilpeterson"
   manager="timlt"
   editor="tysonn"
   tags="azure-resource-manager"/>

<tags
   ms.service="virtual-machines-windows"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="vm-windows"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="nepeters"/>

# <a name="automating-application-deployments-to-azure-virtual-machines"></a>Toimingute automatiseerimine rakenduse kasutuselevõttu Azure'i Virtuaalmasinates

See on nelja-osa sari details juurutamine ja konfigureerida Azure ressursside ja rakenduste Azure ressursside haldamine mallide kasutamine. Selle sarja proovi mall on juurutatud ja juurutamise malli uurida. Selle sarja eesmärk on harige Azure ressursse seoste kohta ja pakuvad käed komplekti juurutamine täielikult integreeritud Azure ressursihaldur mallid. Selle dokumendi eeldab teadmiste Azure'i ressursihaldur lihtsa taseme, selles õpetuses enne tutvumine Azure'i ressursihaldur põhimõtted.

## <a name="music-store-application"></a>Muusika poe rakendus

Selles sarjas kasutatud valim on soovitud .net Core rakenduse simuleerida ostmine kogemus muusika pood. Selle rakenduse saab kasutada Linux või Windowsi virtuaalse süsteemi, nii on loodud valimi juurutuste. Taotlus sisaldab veebirakenduse ja SQL-andmebaasi. Enne selle sarja artikleid lugeda, selle lehe leitud juurutamise nupu abil rakenduse juurutamine. Täielikult juurutamisel rakenduse / Azure arhitektuur näeb välja nagu järgmisel joonisel. 

Muusika poe ressursihaldur malli leiate siit, [Muusika poe Windows Mall](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows)

![Muusika poe rakendus](./media/virtual-machines-windows-dotnet-core/music-store.png)

Kõik need komponendid, sh sidusettevõtte malli JSON vaadatakse läbi neli järgmistest artiklitest.

- [**Rakenduse arhitektuur**](./virtual-machines-windows-dotnet-core-2-architecture.md) – nt veebisaitide ja andmebaaside komponendid peate olema majutatud Azure arvuti ressursse, nt virtuaalmasinates ja Azure SQL-i andmebaasid. Selles dokumendis tutvustatakse vastenduse Arvuta vajate, Azure ressursid ja juurutamine need ressursid on Azure ressursihaldur malli abil. 

- [**Juurdepääsu ja selle turvalisust**](./virtual-machines-windows-dotnet-core-3-access-security.md) – kui hosting rakendusi Azure'is, on vaja silmas pidada? kuidas pääseb rakendus ja erinevate rakenduse komponendid juurdepääsu üksteise. Selle dokumendi üksikasjad esitada ja turvaliseks Interneti-ühendus rakenduse ja Accessi rakenduse osade vahel.

- [**Kättesaadavus ja skaala**](./virtual-machines-windows-dotnet-core-4-availability-scale.md) – kättesaadavus ja mastaabi viitavad rakenduste võimalus töötab taristu tööseisakute ajal peatada ja võimalus ulatuse Arvuta ressursse rakenduse rahuldamiseks. Selle dokumendi üksikasjad juurutamine koormus tasakaalustatud vajalikke komponente ja rakenduse väga kättesaadav.

- [**Rakenduse juurutamine**](./virtual-machines-windows-dotnet-core-5-app-deployment.md) - rakenduste peale Azure'i Virtuaalmasinates juurutamisel meetod, mis on virtuaalne arvutisse installitud rakenduste kahendfaile tuleb arvestada. Selle dokumendi üksikasjad automatiseerimine rakenduse installimine abil Azure virtuaalse masina kohandatud skript laiendid.

Azure'i ressursihaldur Mallid väljatöötamisel eesmärk on Azure infrastruktuuri, installimine ja konfigureerimine kõik rakendused on majutatud see Azure'i infrastruktuuri juurutamise automatiseerimiseks. Töö kaudu artiklitest leiate näiteks seda.

## <a name="deploy-the-music-store-application"></a>Muusika poest rakenduse juurutamine

Selle nupu abil saab kasutada muusika poe rakendus.

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FMicrosoft%2Fdotnet-core-sample-templates%2Fmaster%2Fdotnet-core-music-windows%2Fazuredeploy.json" target="_blank">
    <img src="http://azuredeploy.net/deploybutton.png"/>
</a>

Azure'i ressursihaldur malli jaoks on vaja järgmiste parameetrite väärtused.

|Parameetri nimi |Kirjeldus   |
|---|---|
|ADMINUSERNAME   | Administraator kasutaja nimi, mida kasutatakse virtuaalse masina ja Azure SQL-andmebaas.  |
|ADMINPASSWORD | Parool, mida kasutatakse Azure virtuaalse masina ja SQL-andmebaasi.  |
|NUMBEROFINSTANCES | Luua virtuaalmasinates arv. Kõik need virtuaalmasinates majutada muusika poe veebirakenduse ja kõik liikluse on laadi need lõikes. |
|PUBLICIPADDRESSDNSNAME | DNS-i globaalselt kordumatu nimi seotud avaliku IP-aadressi. |

Kui malli juurutamise on lõppenud, sirvige avaliku IP-aadressi kasutades brauserit internet. .Net esitatakse Core muusika sait.

## <a name="next-steps"></a>Järgmised sammud

<hr>

[Samm 1 - rakenduse arhitektuur Azure ressursihaldur mallide kasutamine](./virtual-machines-windows-dotnet-core-2-architecture.md)

[Samm 2 - juurdepääsu ja turvalisuse Azure ressursihaldur Mallid](./virtual-machines-windows-dotnet-core-3-access-security.md)

[Samm 3 - saadavus ja Azure ressursihaldur korduvkasutatava skaala](./virtual-machines-windows-dotnet-core-4-availability-scale.md)

[Juhis 4 – rakenduse juurutamine Azure ressursihaldur mallide kasutamine](./virtual-machines-windows-dotnet-core-5-app-deployment.md)


