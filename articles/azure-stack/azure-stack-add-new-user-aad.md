<properties
    pageTitle="Azure'i virnas rentniku uue konto lisamine Azure Active Directory | Microsoft Azure'i"
    description="Pärast võtavad Microsoft Azure'i virnas POC, peate vähemalt üks rentniku kasutajakonto loomine, nii et saate uurida rentniku portaalis."
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
    ms.date="09/26/2016"
    ms.author="erikje"/>

# <a name="add-a-new-azure-stack-tenant-account-in-azure-active-directory"></a>Azure Active Directory Azure'i virnas rentniku uue konto lisamine

Pärast [juurutamine Azure'i virnas POC](azure-stack-run-powershell-script.md), peate rentniku kasutajakonto nii, et saate uurida rentniku portaalis ja testige oma pakkumised ja lepingud. [Azure portaali](#create-an-azure-stack-tenant-account-using-the-azure-portal) kaudu või [PowerShelli](#create-an-azure-stack-tenant-account-using-powershell)abil saate luua rentnikukonto.

## <a name="create-an-azure-stack-tenant-account-using-the-azure-portal"></a>Azure'i virnas rentnikukonto Azure portaali loomine

Azure'i tellimuse kasutada Azure portaali peab teil olema.

1. [Azure'i](http://manage.windowsazure.com)sisse logida.

2.  Microsoft Azure'i vasakpoolsel navigeerimisribal nuppu **Active Directory**.

3.  Directory loendis klõpsake kataloogi, mida soovite kasutada Azure virnas või looge uus.

4.  Klõpsake selle kataloogi lehel **Kasutajad**.

5.  Klõpsake nuppu **Lisa kasutaja**.

6.  Valige loendist **Tüüp kasutaja** viisardi **Lisa kasutaja** **ettevõttes uue kasutaja**.

7.  Tippige väljale **kasutajanimi** kasutaja nimi.

8.  Klõpsake soovitud **@** väljale, valige vastav kirje.

9.  Järgmise noolt.

10.  **Kasutajaprofiili** lehe viisardi, tippige soovitud **eesnimi**, **perekonnanimi**ja **kuvatav nimi**.

11. Valige loendis **rolliga** **kasutaja**.

12. Järgmise noolt.

13. Klõpsake lehel **saada ajutine parool** , klõpsake nuppu **Loo**.

14. Kopeerige **Uus parool**.

15. Logige sisse Microsoft Azure'i uue kontoga. Kui kuvatakse vastav viip parooli muutmine.

16. Logige sisse `https://portal.azurestack.local` rentniku portaalis kuvamiseks uue kontoga.

## <a name="create-an-azure-stack-tenant-account-using-powershell"></a>PowerShelli kasutamine Azure virnas rentniku konto loomine

Kui teil pole Azure tellimuse, ei saa kasutada Azure portaali rentnikukonto kasutaja lisamiseks. Sel juhul saate kasutada funktsiooni Azure Active Directory moodul Windows PowerShelli jaoks hoopis.

> [AZURE.NOTE] Kui kasutate juurutamise Azure'i virnas PoC Microsoft Account (Live ID), ei saa kasutada AAD PowerShelli rentniku konto loomiseks. 

1.  Installige [Microsoft Online Services sisselogimisabimehe IT-spetsialistid RTW jaoks](https://www.microsoft.com/en-us/download/details.aspx?id=41950).

2.  Installige [Azure'i Active Directory moodul Windows PowerShelli jaoks (64-bitine versioon)](http://go.microsoft.com/fwlink/p/?linkid=236297) ja avage see.

3.  Käivitage järgmised cmdlet-käsud:




    ```powershell
    # Provide the AAD credential you use to deploy Azure Stack PoC
   
            $msolcred = get-credential
    
    # Add a tenant account "Tenant Admin <username>@<yourdomainname>" with the initial password "<password>".
    
            connect-msolservice -credential $msolcred
            $user = new-msoluser -DisplayName "Tenant Admin" -UserPrincipalName <username>@<yourdomainname> -Password <password>
            Add-MsolRoleMember -RoleName "Company Administrator" -RoleMemberType User -RoleMemberObjectId $user.ObjectId
    
    ```

4.  Logige sisse Microsoft Azure'i uue kontoga. Kui kuvatakse vastav viip parooli muutmine.

5.  Logige sisse `https://portal.azurestack.local` rentniku portaalis kuvamiseks uue kontoga.



