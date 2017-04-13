<properties
    pageTitle="Kasutada MySQL-i andmebaaside kohta Azure virnas PaaS | Microsoft Azure'i"
    description="Mõista juurutada MySQL-i ressursikeskuse pakkuja ja esitada MySQL-i teenuse Azure virnas kiirtoimingud."
    services="azure-stack"
    documentationCenter=""
    authors="Dumagar"
    manager="bradleyb"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="dumagar"/>

# <a name="use-mysql-databases-as-paas-on-azure-stack"></a>Kasutage MySQL-i andmebaaside PaaS Azure'i virnas kohta

> [AZURE.NOTE] Järgmine teave kehtib ainult Azure virnas TP1 juurutuste.

Saate juurutada MySQL-i ressursikeskuse pakkuja Azure'i virnas kohta. Pärast juurutamist ressursi pakkuja, saate luua MySQL-i serverid ja andmebaasid Azure'i ressursihaldur juurutamise Mallid ja sisestage MySQL-i andmebaaside teenust. MySQL-i andmebaasidele, mis on levinud veebisaitidel, toetab mitme veebisaidi. Pärast juurutamist ressursi pakkuja, saate luua WordPress veebisaitide veebirakenduste Azure'i platvormi nimega service (PaaS) lisandmooduli Azure'i virnas.

## <a name="quick-steps-to-deploy-the-resource-provider"></a>Kiirtoimingud juurutamiseks ressursi pakkuja
Kui olete juba tuttav Azure'i virnas, tehke järgmist. Kui soovite rohkem üksikasju, linkide kaudu iga jaotise või minge otse [Deploy MySQL-i andmebaasi ressursi pakkuja adapterit Azure'i virnas POC](azure-stack-mysql-rp-deploy-long.md).

1.  Veenduge, et [täitke kõik häälestamine juhiseid](azure-stack-mysql-rp-deploy-long.md#set-up-steps-before-you-deploy) enne juurutamist ressursi pakkuja:

    - .NET 3.5 framework juba häälestamine rakenduses Windows Server pilti. (Kui laadisite Azure'i virnas bittide pärast, 23 veebruar 2016, saate jätate selle etapi.)
    - [Azure'i PowerShelli, mis ühildub Azure'i virnas soovitud versiooni installimist](http://aka.ms/azStackPsh).
    - Internet Exploreri turbesätete ClientVM, [Internet Exploreris turvalisuse on välja lülitatud ja küpsised on lubatud](azure-stack-mysql-rp-deploy-long.md#Turn-off-IE-enhanced-security-and-enable-cookies).

2. [MySQL-i ressursikeskuse pakkuja kahendfaile faili alla laadida](http://aka.ms/masmysqlrp) ja väljavõte ClientVM virnas oma Azure'i toimetulekuks mõistet (PoC).

3. [Käivitage bootstrap.cmd ja skriptide](azure-stack-mysql-rp-deploy-long.md#Bootstrap-the-resource-provider-deployment-PowerShell-and-Prepare-for-deployment).

    Skriptide komplekt, mis on rühmitatud kaks põhilist vahekaarti avatakse rakenduses PowerShelli integreeritud skriptimise keskkonnas (ISE). Kõik laaditud skriptide käitamiseks järjest vasakult paremale iga vahekaardi.

    1. Käivitage skriptide **ettevalmistamine** menüüs vasakult paremale, et:

        - Turvaline side ressursi pakkuja ja Azure ressursihaldur metamärgiga serdi loomine
        - MySQL-i EULA nõustumine ja MySQL-i kahendfaile alla laadida.
        - Laadige serdid ja kõik muud esemeid konto Azure virnas salvestusruumi.
        - Galerii-pakettide avaldamine, nii, et saate juurutada MySQL-i ressursid Galerii.

        > [AZURE.IMPORTANT] Kui mis tahes skriptide hangub ei ole nähtav põhjust, kui saadate oma Azure Active Directory rentniku, turbesätete blokeerivad käivitamiseks juurutamiseks nõutava dll-faili. Lahendanud probleemi, vaadake Microsoft.AzureStack.Deployment.Telemetry.Dll ressursi pakkuja kaustas paremklõpsake seda, klõpsake käsku **Atribuudid**ja seejärel märkige vahekaardil **üldist** **Aktiveeri** .

    2. Käivitage skriptide **Deploy** menüüs vasakult paremale, et:

        - [Deploy virtuaalse masina (VM)](azure-stack-mysql-rp-deploy-long.md#Deploy-the-MySQLResource-Provider-VM) , nii oma ressursi pakkuja, MySQL-i serverid ja andmebaaside, mida teil on väärtustada majutava. See skript viitab JSON parameetri fail, mida peate värskendama mõned väärtused enne skripti käivitamist.
        - [Registreerida kohaliku DNS-i kirje](azure-stack-mysql-rp-deploy-long.md#Update-the-local-DNS) , mille saab vastendada ressursi pakkuja VM.
        - [Ressursi pakkuja registreerida](azure-stack-mysql-rp-deploy-long.md#Register-the-MySQL-RP-Resource-Provider) koos kohaliku Azure'i ressursihaldur.

        > [AZURE.IMPORTANT] Kõik skriptid Oletame, et operatsioonisüsteemide pilt täidab eeltingimused (.NET 3.5 installitud, JavaScripti ja küpsiseid soovitud ClientVM ja Azure PowerShelli paigaldatud uusim versioon). Kui teil tekib tõrgete skriptide käivitamisel, kontrollige, kas eeltingimused täidetud.

5. [Testige oma uue MySQL-i ressursikeskuse pakkuja](/azure-stack-MySql-rp-deploy-long.md#create-your-first-mysql-database-to=test-your-deployment), juurutada MySQL-i andmebaasi Azure virnas portaalist. Klõpsake nuppu **Loo** &gt; **kohandatud** &gt; **MySQL-i Server ja andmebaas**.

See peaks saama pakkuja MySQL-i ressursikeskuse üles ja töötab huvipunktidesse.
