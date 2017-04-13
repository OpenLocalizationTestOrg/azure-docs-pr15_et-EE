<properties
    pageTitle="Azure'i virnas rakenduse teenuse tehnilise eelvaate 1 enne alustamist | Microsoft Azure'i"
    description="Toimige enne juurutamist veebirakenduste kohta Azure virnas"
    services="azure-stack"
    documentationCenter=""
    authors="apwestgarth"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="app-service"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="anwestg"/>
    
# <a name="before-you-get-started-with-azure-stack-web-apps"></a>Enne alustamist Azure'i virnas Web Apps

> [AZURE.NOTE] Järgmine teave kehtib ainult Azure virnas TP1 juurutuste.

Vajate mõne üksuste installida Azure virnas veebirakenduste:

- [Azure'i virnas Technical Preview 1](azure-stack-run-powershell-script.md) lõplikus kasutuselevõtt
- Azure'i virnas Web Appsi small juurutamiseks Azure'i virnas süsteemis piisavalt ruumi.  Nõutav ruumi on umbes 20GB vaba kettaruumi.
- [Server, mis töötab SQL Server](#SQL-Server).

>[AZURE.NOTE] Kõik toimub kliendi VM järgmist.

## <a name="before-you-deploy-azure-stack-web-apps"></a>Enne juurutamist Azure virnas Web Apps

Ressursi pakkuja juurutamiseks peate käivitama PowerShelli integreeritud skriptimise Environment(ISE) administraatorina. Seetõttu peate küpsiste ja JavaScripti lubamiseks Internet Exploreris profiili, mida kasutate Azure Active Directory sisse logida.

## <a name="turn-off-internet-explorer-enhanced-security"></a>Internet Exploreri täiustatud turvalisus väljalülitamine

1.  Azure'i virnas tõendada mõiste (POC) arvuti ****AzureStack/administraatorina sisse logida ja avage **Serveri haldur**.
2.  **Internet Exploreri täiustatud konfiguratsiooni** välja lülitada nii administraatoritele ja kasutajatele.
3.  ClientVM.AzureStack.local virtual arvutisse administraatorina sisse logida ja avage **Serveri haldur**.
4.  **Internet Exploreri täiustatud konfiguratsiooni** välja lülitada nii administraatoritele ja kasutajatele.

## <a name="enable-cookies"></a>Küpsiste lubamine

1.  Valige **Start**, > **Kõik rakendused**> **Windowsi tarvikud**. Paremklõpsake **Internet Exploreri** > **veel** > **Käivita administraatorina**.
2.  Kui teilt küsitakse, valige **soovitatav kasutada turvalisus**ja seejärel klõpsake **nuppu OK**.
3.  Valige Internet Exploreris **Tööriistad** (hammasratta ikoon) > **Interneti-suvandid** > **privaatsusavalduse** > **Täpsemalt**.
4.  Valige **Täpsemalt**. Veenduge, et mõlemad **Aktsepteeri** ruudud on valitud ja klõpsake seejärel nuppu **OK** ja **OK** uuesti.
5.  Sulgege Internet Explorer ja taaskäivitage PowerShell ISE administraatorina.

## <a name="install-the-latest-version-of-azure-powershell"></a>Installige uusim versioon Azure PowerShell

1.  Logige sisse Azure'i virnas POC arvuti ****AzureStack/administraatorina.
2.  Kasutage funktsiooni kaugtöölaua ühendus ClientVM.AzureStack.local virtuaalse masina administraatorina sisse logida.
3.  Avage **Juhtpaneel** ja valige käsk **Desinstalli programm**. 
4.  **Microsoft Azure'i PowerShelli – November 2015** paremklõpsake ja valige käsk **desinstalli**.
5.  Pärast eemaldada viimistluse, ClientVM.AzureStack.local virtual arvuti taaskäivitama
6.  Laadige alla ja installige uusim [Azure PowerShelli](http://aka.ms/azstackpsh).


## <a name="sql-server"></a>SQL serveri

Azure'i virnas veebirakendustes installiprogrammi on vaikimisi kasutada SQL serveri, mis installitakse koos SQL Azure'i virnas ressursi pakkuja. Kui installite SQL serveri ressursi pakkuja (SQL-i RP) veenduge, et te arvestama andmebaasi administraatori kasutajanime ja parooliga. Peate mõlemad Azure'i virnas veebirakenduste installimisel.
Märkus: On ka teil võimalus juurutada ja muu serveri abil saate käivitada SQL serveri. Saate valida suvandi Azure'i virnas veebirakendustes installiprogrammi lõpulejõudmisel kasutada SQL serveri eksemplar.
