<properties
    pageTitle="Azure'i virnas tööriistad ja PaaS teenused | Microsoft Azure'i"
    description="Saate teada, kuidas alustada PaaS teenuste Azure'i virnas."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="tools-for-azure-stack"></a>Azure'i virnas tööriistad

Saate alla laadida allpool kirjeldatud tööriistu. Kui soovite teavitama uusi teenuseid ja tööriistu, järgige #AzureStack Twitter.

## <a name="template-tools"></a>Malli tööriistad

### <a name="azure-stack-github-templates"></a>Azure'i virnas Github Mallid
Tutvuge kasvab saidikogumi [Azure'i virnas GitHub](https://github.com/Azure/AzureStack-QuickStart-Templates)malle. [Azure'i](https://github.com/Azure/azure-quickstart-templates), nagu need "Kiirkäivituse" Azure ressursihaldur Mallid eesmärk algust teha lihtsa koosteüksused ja näiteid, valmis kasutama Microsoft Azure'i virnas tehnilise eelvaate tõendada, mõistet keskkonnale. Sisalduv on esimene tootja töökoormus näited [ad-mitte-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/ad-non-ha), [sql-2014-mitte-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sql-2014-non-ha), [SharePointi-2013 – mitte-ha](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/sharepoint-2013-non-ha), nagu ka mitmeid 101 mallide [101-lihtne windows vm](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-simple-windows-vm).


### <a name="marketplace-item-packaging-tool-and-sample"></a>Turuplatsi üksuse pakendit tööriista ja näidis
[Laadige alla ja pakendit tööriista kasutamine](http://www.aka.ms/azurestackmarketplaceitem) loomine turuplatsi jaoks virnas Azure'i turuplatsi lisada oma kohandatud malle. [Turuplatsi loomine](azure-stack-create-and-publish-marketplace-item.md)üksuse leiate juhised, kuidas turuplatsi üksuse loomine ja selle kättesaadavaks tegemiseks oma rentnikke.

## <a name="developer-tools"></a>Arendaja tööriistad


### <a name="use-visual-studio-and-azure-stack-tp2-on-the-mas-con01-virtual-machine"></a>Visual Studio ja Azure virnas TP2 MAS-CON01 virtuaalse masina kasutamine
Kui soovite kasutada Visual Studio konsooli VM töötamine Azure'i virnas Mallid, tuleb teil installida nõutavad tööriistad õigeid versioone. TP2 toetatud versiooni installimiseks tehke järgmist.

1. Kaugtöölaua ühendus abil MAS-CON01 virtuaalse masina azurestack\azurestackadmin identimisteabega sisse logida.
2. Installige ja Web platvormi Installeri avamine.
3. Otsida ja installida **Visual Studio ühenduse 2015 Microsoft Azure'i SDK - 2.9.5**.
4. **Microsoft Azure'i PowerShelli (Sept)** saab osana SDK installitud **Programmide lisamine või eemaldamine**kaudu desinstallida.
5. Web platvormi Installeri avamine.
6. Otsida ja installida **Microsoft Azure'i PowerShelli - Azure'i virnas Technical Preview 2**. 
7. Taaskäivitage MAS-CON01 virtuaalse masina.
8. Kaugtöölaua ühendus abil MAS-CON01 virtuaalse masina azurestack\azurestackadmin identimisteabega sisse logida.
9. Avage Visual Studio ja kinnitage, et te saate ühenduse Azure'i virnas keskkonnas, mallide saamiseks jne. 

### <a name="azure-powershell-sdk"></a>Azure'i PowerShelli SDK
Azure'i PowerShelli on moodul, mis pakub hallata Azure ja Azure virnas Windows PowerShelli cmdlet-käsud. Cmdlet-käskude abil saate luua, testida, juurutada ja hallata lahenduste ja teenustega virnas Azure'i platvormi kaudu.
[Azure'i PowerShelli SDK alla laadida](http://aka.ms/azStackPsh) ja [installida PowerShelli](azure-stack-connect-powershell.md).

### <a name="azure-cross-platform-command-line-interfaces"></a>Azure'i platvormi käsk rea liideste rist
Kiiresti installige Azure'i käsurea liides (Azure'i CLI) kasutamist avatud lähtekoodi shell-põhiste käskude loomise ja haldamise ressursid Microsoft Azure'i virnas.

[Laadige alla Windows CLI](http://aka.ms/azstack-windows-cli)

[Mac-arvutis CLI allalaadimine](http://aka.ms/azstack-linux-cli)

[Laadige alla Linux CLI](http://aka.ms/azstack-mac-cli)

>[AZURE.NOTE]
>
> + Kui olete Maci või Linuxi arvutis, saate avada ka CLI käsu abil`npm install -g azure-cli@0.9.11`</br>
> + Kui te olete serdi valideerimisprobleemid, käivitage käsk`set NODE_TLS_REJECT_UNAUTHORIZED=0`
