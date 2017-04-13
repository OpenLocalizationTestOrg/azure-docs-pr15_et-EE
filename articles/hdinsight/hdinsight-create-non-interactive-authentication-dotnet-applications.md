<properties
    pageTitle="Luua interaktiivne autentimine .NET Hdinsightiga ametiisikute | Microsoft Azure'i"
    description="Saate teada, kuidas luua interaktiivne autentimine .NET Hdinsightiga rakendusi."
    editor="cgronlun"
    manager="jhubbard"
    services="hdinsight"
    documentationCenter=""
    tags="azure-portal"
    authors="mumian"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/02/2016"
    ms.author="jgao"/>

# <a name="create-non-interactive-authentication-net-hdinsight-applications"></a>Interaktiivne autentimine .NET Hdinsightiga rakenduste loomine

Rakenduse .NET Azure Hdinsightiga rakenduse oma identiteedi (mitte-interaktiivne) ega alusel taotluse (interaktiivne) sisselogitud kasutaja identiteet käivitamiseks. Valimi interaktiivse rakenduse, leiate teemast [esitada taru/siga/Sqoop töö Hdinsightiga .NET SDK abil](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk). Selles artiklis kirjeldatakse, kuidas luua interaktiivne autentimine .net-i rakenduse ühenduse loomine Windows Azure Hdinsightiga ja esitada taru töö.

Rakendusest .NET peate:

- oma Azure tellimuse rentniku ID
- Azure'i Directory rakenduse kliendi ID
- Azure'i Directory rakenduse salajane võti.  

Peamine protsess koosneb järgmistest etappidest:

2. Azure'i Directory rakenduse loomine.
2. Saate määrata rakenduse AD rollid.
3. Klientrakenduse välja.


##<a name="prerequisites"></a>Eeltingimused

- Hdinsightiga kobar. Üks leitud [alustamise õpetus](hdinsight-hadoop-linux-tutorial-get-started.md#create-cluster)juhiste abil saate luua. 




## <a name="create-azure-directory-application"></a>Azure'i Directory rakenduse loomine 
Active Directory rakenduse loomisel selle tegelikult loob taotlus ja teenuse põhisumma. Jaotises rakenduse identiteedi rakenduse käivitamiseks.

Praegu peab Azure klassikaline portaali abil luua uue Active Directory rakenduse. Selle võimaluse lisatakse Azure portaali uuem väljaanne. Saate teha ka järgmist Azure PowerShelli või Azure CLI. PowerShelli või CLI põhisumma teenuse kasutamise kohta lisateabe saamiseks vt [autentimise teenus põhisumma Azure'i ressursihaldur](../resource-group-authenticate-service-principal.md).

**Azure'i Directory rakenduse loomine**

1.  [Azure'i klassikaline portaali]( https://manage.windowsazure.com/)sisse logida.
2.  Valige vasakul paanil **Active Directory** .

    ![Azure'i klassikaline portaali active directory](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\active-directory.png)
    
3.  Valige kataloogi, mida soovite luua uue rakenduse kasutamine. Võib olemasolev fail.
4.  Olemasolevate rakenduste loendi kohal nuppu **rakendused** .
5.  Alt uue rakenduse lisamiseks klõpsake nuppu **Lisa** .
6.  Sisestage **nimi**, valige **veebirakenduse ja/või veebi-API**ja seejärel klõpsake nuppu **edasi**.

    ![Azure'i active directory uus rakendus](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application.png)

7.  Sisestage **sisselogimise URL-i** ja **Rakenduse ID URI**. **Logi-ON URL-i**, sisestage URI lisamine veebisaidile, mis kirjeldab rakenduse. Veebilehe olemasolu ei kontrollita. Rakenduse ID URI-d pakuvad URI, mille abil tuvastatakse teie rakendus. Ja seejärel klõpsake nuppu **valmis**.
Mõni hetk rakenduse loomiseks.  Kui rakendus on loodud, kuvatakse portaali kiirülevaate Glace lehe uus rakendus. Ärge sulgege portaali. 

    ![uue azure active directory rakenduse atribuudid](.\media\hdinsight-create-non-interactive-authentication-dotnet-application\hdinsight-add-ad-application-properties.png)

**Saada taotlus kliendi ID ja salajane võti**

1.  Klõpsake äsja loodud AD Rakenduse lehel ülalt menüüst **konfigureerimine** .
2.  Tehke **Kliendi ID**koopia. Peate oma .net-i rakenduse.
3.  Jaotises **klahvid**, klõpsake **kestus valige** ripploendit ja valige kas **1 aasta** või **2 aastat**. Enne, kui salvestate konfiguratsiooni võtmeväärtuse ei kuvata.
4.  Klõpsake lehe allservas **salvestada** . Salajane võti kuvamisel teha koopia võti. Peate oma .net-i rakenduse.

##<a name="assign-ad-application-to-role"></a>AD Rakenduse rolli määramine

Rakenduse [rolli](../active-directory/role-based-access-built-in-roles.md) anda õigused toimingute jaoks peate määrama. Saate seada ulatuse tasemel tellimus, ressursirühm või ressurss. Õigused päritakse madalama taseme ulatus (nt lisamine rakenduse lugeja rollile ressursirühma tähendab, et seda saavad lugeda ressursirühma ja see sisaldab ressursse). Selles õpetuses seate ulatuse ressursi rühma tasemel.  Kuna Azure klassikaline portaali ei toeta ressursside rühmad, selles osas on vaja teha Azure portaalist. 

**Roll omaniku lisamiseks AD rakendus**

1.  [Azure'i portaali](https://portal.azure.com)sisse logida.
2.  Klõpsake vasakpoolsel paanil **Ressursirühma** .
3.  Klõpsake nuppu ressurss rühm, mis sisaldab Hdinsightiga kobar, kus te käivitavad teie päringu taru allpool olevat selles õpetuses. Kui loendis on liiga palju ressursi rühmad, saate kasutada filtrit.
4.  Klõpsake **Accessi** kobar keelest.

    ![pilveteenuse ja thunderbolt ikooni = Kiirjuhend](./media/hdinsight-hadoop-create-linux-cluster-portal/quickstart.png)
5.  Klõpsake nuppu **Lisa** **kasutaja** keelest.
6.  Järgige juhiseid, et lisada **omaniku** roll AD Rakenduse loodud viimast toimingut. Kui te lõpule viia, peab kuvatakse loendis omanik rolliga kasutaja tera rakendus.


##<a name="develop-hdinsight-client-application"></a>Arendada Hdinsightiga klientrakendus

Looge C# .net-i konsooli rakendus leitud [esitada Hadoopi töökohtade Hdinsightiga](hdinsight-submit-hadoop-jobs-programmatically.md#submit-hivepigsqoop-jobs-using-hdinsight-net-sdk)juhistele. Asendage GetTokenCloudCredentials meetodit järgmist:

    public static TokenCloudCredentials GetTokenCloudCredentials(string tenantId, string clientId, SecureString secretKey)
    {
        var authFactory = new AuthenticationFactory();

        var account = new AzureAccount { Type = AzureAccount.AccountType.ServicePrincipal, Id = clientId };

        var env = AzureEnvironment.PublicEnvironments[EnvironmentName.AzureCloud];

        var accessToken =
            authFactory.Authenticate(account, env, tenantId, secretKey, ShowDialog.Never)
                .AccessToken;

        return new TokenCloudCredentials(accessToken);
    }

Tuua rentniku ID PowerShelli kaudu:

    Get-AzureRmSubscription

Või Azure CLI:

    azure account show --json

      
## <a name="see-also"></a>Vt ka

- [Esitage Hadoopi töökohtade Hdinsightiga](hdinsight-submit-hadoop-jobs-programmatically.md)
- [Active Directory rakenduste ja teenuste põhisumma portaalis loomine](../resource-group-create-service-principal-portal.md)
- [Teenuse põhilise Azure'i ressursihaldur autentida](../resource-group-authenticate-service-principal.md)
- [Azure'i Rollipõhine juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md)
