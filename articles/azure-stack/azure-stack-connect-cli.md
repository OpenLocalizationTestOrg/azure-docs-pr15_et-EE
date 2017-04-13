<properties
    pageTitle="Ühenduse loomine Azure virnas CLI | Microsoft Azure'i"
    description="Saate teada, kuidas platvormidel käsurea liides (CLI) abil saate hallata ja ressursse Azure'i Virnlintdiagrammil juurutamine"
    services="azure-stack"
    documentationCenter=""
    authors="HeathL17"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="helaw"/>

# <a name="install-and-configure-azure-stack-cli"></a>Installima ja konfigureerima Azure'i virnas CLI

Selles dokumendis me juhatavad teid protsessi haldamine Azure'i virnas ressursid Linuxi ja Mac kliendi platvormide Azure'i käsurea liides (CLI) abil.  

## <a name="install-azure-stack-cli"></a>Installige Azure'i virnas CLI

Kui kasutate Maci või Linuxi, saate CLI abil järgmine käsk:
  
    `npm install -g azure-cli@0.10.4`.


## <a name="connect-to-azure-stack"></a>Ühenduse loomine Azure virnas
Järgmistes juhistes, saate konfigureerida Azure CLI ühenduse Azure'i virnas. Seejärel logige sisse ja tellimuse teabe toomiseks.

1.  See PowerShelli täites tuua active directory ressursi id väärtus:
        
          (Invoke-RestMethod -Uri https://api.azurestack.local/metadata/endpoints?api-version=1.0 -Method Get).authentication.audiences[0]

2.  CLI järgmise käsu abil saate lisada Azure virnas keskkonnas, veendudes, et värskendada *--active directory ressursi id* andmetega URL-i tuua eelmises etapis:

           azure account env add AzureStack --resource-manager-endpoint-url "https://api.azurestack.local" --management-endpoint-url "https://api.azurestack.local" --active-directory-endpoint-url  "https://login.windows.net" --portal-url "https://portal.azurestack.local" --gallery-endpoint-url "https://portal.azurestack.local" --active-directory-resource-id "https://azurestack.local-api/" --active-directory-graph-resource-id "https://graph.windows.net/"

3.  Logige sisse, kasutades (Asenda *kasutajanimi* oma kasutajanimega) järgmine käsk:

        azure login -e AzureStack -u “<username>”

    >[AZURE.NOTE]Kui ilmneb serdi valideerimisprobleemid keelamine serdi valideerimine käsu `set        NODE_TLS_REJECT_UNAUTHORIZED=0`.

4.  Määrata režiimis Azure konfigureerimine Azure'i ressursihaldur abil järgmine käsk:

        azure config mode arm

5.  Tellimuste loendi toomiseks.

        azure account list     

## <a name="next-steps"></a>Järgmised sammud

[Azure'i CLI malle juurutamine](azure-stack-deploy-template-command-line.md)

[Ühenduse loomine PowerShelli abil](azure-stack-connect-powershell.md)

[Kasutajaõiguste haldamine](azure-stack-manage-permissions.md)
