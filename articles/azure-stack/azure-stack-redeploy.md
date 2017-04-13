<properties
    pageTitle="Azure'i Virnlintdiagrammil ümberkorraldamine | Microsoft Azure'i"
    description="Juurutage uuesti Azure virnas."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="erikje"/>

# <a name="redeploy-azure-stack"></a>Azure'i virnas ümberkorraldamine

Juurutage uuesti Azure'i virnas, peate käivitama üle algusest peale ise luua, nagu on kirjeldatud allpool.

## <a name="steps-to-redeploy-azure-stack"></a>Juhised Azure'i virnas ümberkorraldamine

1. Taaskäivitus selle hosti algse operatsioonisüsteemi (tühjal metallist installitud). See pole vaikesäte buutimine menüüs nii, et peate kasutama KVM või kohaliku konsooli selle valimiseks ning taaskäivitage arvuti ajal (häälestamise ajal saate funktsiooni "buutimine kaudu VHD" OS-"AzureStack TP2" nimega, see aitab tuvastada, mis on OS, mis).

    Te ei pea eemaldada olemasoleva kirje buutimine (uus tugiteenuste skripti "PrepareBootFromVHD.ps1" hoolitseb, et teie jaoks.)

2. Kui te ei saa KVM või soovite valida enne taaskäivitus buutimine OS:
    
    1. Otsige üles skripti.\BootMenuNoKVM.ps1. See fail on saadaval koos selle koostamine esitatud tugi skripte.
    
    2. Käivitage skript administraatoriõigustega. Valige algse Host OS nimi. See käivitada selle hosti sisse algse host OS ei ole vaja KVM juurdepääsu.
    
    3. Kui script on lõpule viidud palutakse kinnitada, et taaskäivitage arvuti.

    - Kui teised kasutajad sisse logitud, see käsk nurjub.

    - Palun lihtsalt käivitage järgmine käsk: taaskäivitada arvuti-jõustamine 
 
3. Kustutage CloudBuilder.vhdx faili, mida kasutati eelmise juurutamise käigus.

    Teil pole vaja kustutada olemasolevate talletusmahtu eelmise TP2 juurutamise. Juurutamise skripti tuvastab ja migreerimispaketiga olemasoleva ja seejärel loob uue.

5. Juurutage uuesti kopeerimine uue koopia CloudBuilder.vhdx, buutimine selle jne.

## <a name="next-steps"></a>Järgmised sammud

[Ühenduse loomine Azure virnas](azure-stack-connect-azure-stack.md)
