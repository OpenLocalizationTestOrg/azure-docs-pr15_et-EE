<properties
   pageTitle="Azure'i automaatika tegevusraamatud lisamine taastamise | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse, kuidas Azure saidi taastamise nüüd võimaldab teil laiendada taastamine lepingute abil keerukaid ülesandeid täita taastamisel Azure'i Azure automatiseerimine"
   services="site-recovery"
   documentationCenter=""
   authors="ruturaj"
   manager="gauravd"
   editor=""/>

<tags
   ms.service="site-recovery"
   ms.devlang="powershell"
   ms.tgt_pltfrm="na"
   ms.topic="article"
   ms.workload="required"
   ms.date="10/23/2016"
   ms.author="ruturajd@microsoft.com"/>


# <a name="add-azure-automation-runbooks-to-recovery-plans---classic"></a>Azure'i automaatika tegevusraamatud lisamine taastamine lepingud – klassikaline


Selles õppetükis kirjeldatakse, kuidas Azure saidi taastamise integreerub Azure automatiseerimine laiendatavuse taastamise esitada. Taastamise saate korraldab oma virtuaalmasinates kaitstud Azure saidi taastamise nii dispersioonanalüüs teisene pilveteenusesse ja Azure stsenaariumid kopeerimise taastamine. Samuti aitavad tegemisel taastamine **süsteemne täpne**, **korduvate**ja **automatiseeritud**. Kui te ei suuda üle oma Azure'i virtuaalmasinates, integreerimine Azure automatiseerimine laiendab taastamine lepingud ja pakub võimalus tegevusraamatud, võimaldades võimas automaatika ülesandeid täita.

Kui te ei ole kuulnud Azure automatiseerimine veel, registreeruda [siin](https://azure.microsoft.com/services/automation/) ja nende valimi skriptide allalaadimiseks [siin](https://azure.microsoft.com/documentation/scripts/). Lugege lisateavet [Azure saidi taastamine](https://azure.microsoft.com/services/site-recovery/) ja kuidas korraldab taastamine Azure taastamise abil [siin](https://azure.microsoft.com/blog/?p=166264).

Selle lühikese õpetuse me vaatame kuidas saate taastamise Azure'i automaatika tegevusraamatud integreerida. Me lihtsa varasemas versioonis nõutav manuaalset toiminguid automatiseerida ja vaadake, kuidas muuta ühe klõpsuga taastamine toimingu mitme toimingu taastamine. Me ka vaatame, kuidas saate otsida lihtsa kui valesti läheb.

## <a name="protect-the-application-to-azure"></a>Rakenduse Azure kaitsmine

Alustame koosneb kahest virtuaalmasinates lihtne rakendus. Siin on meil HRweb kohaldamine Fabrikam. Fabrikam-HRweb-frontend ja Fabrikam-Hrweb-taustväärtus on kaitstud Azure kaks virtuaalmasinates Azure saidi taastamise abil. Virtuaalmasinates, Azure saidi taastamise abil kaitsta, järgige alltoodud juhiseid.

1.  Lubage oma virtuaalmasinates kaitse.

2.  Veenduge, et selle virtuaalmasinates lõpetanud algse kopeerimise ja imitatsiooniga.

3.  Oodake, kuni algse kopeerimine on lõpule jõudnud ja kopeerimise olek ütleb kaitstud.

![](media/site-recovery-runbook-automation/01.png)
---------------------

Selles õpetuses loome Fabrikam HRweb rakenduse Tõrkesiirde rakenduse Azure taastamise kava. Seejärel saaksime seda integreerida käitusjuhendi, mis loob lõpp nurjunud Azure virtuaalse masina teenida veebilehtede port 80 üle.

Kõigepealt loome taastamise kava meie rakendus.

## <a name="create-the-recovery-plan"></a>Looge taastamise kava

Azure'i rakenduse taastamiseks peate looma taastamis.
Abil saate määrata selle virtuaalmasinates taastamise järjestuse taastamise kava. Rühma 1 virtuaalse masina kuvatakse taastamine ja käivitage esimene ja virtuaalse masina jaotises 2 järgige.

Saate luua taastamise kava, mis näeb välja nagu allpool.

![](media/site-recovery-runbook-automation/12.png)

Lugeda Lisateavet taastamine lepingute kohta vt dokumentatsiooni [siin](https://msdn.microsoft.com/library/azure/dn788799.aspx "siin").

Järgmiseks loome vajalikud esemeid Azure'i automaatika.

## <a name="create-the-automation-account-and-its-assets"></a>Automaatika konto ja varade loomine

Teil on vaja luua tegevusraamatud Azure automatiseerimine kontot. Kui teil on juba konto, liikuge Azure automatiseerimine vahekaarti, mis on tähistatud ![](media/site-recovery-runbook-automation/02.png)ja uue konto loomine.

1.  Tippige konto nimi kindlaks teha.

2.  Määrake geograafiliste piirkond, kuhu soovite konto.

Soovitatav on konto paigutamiseks ASR vault samas piirkonnas.

![](media/site-recovery-runbook-automation/03.png)

Järgmiseks luua järgmiste varade konto.

### <a name="add-a-subscription-name-as-asset"></a>Kui vara tellimuse nime lisamine

1.  Lisage uus säte ![](media/site-recovery-runbook-automation/04.png) Azure automatiseerimine varad ja vali![](media/site-recovery-runbook-automation/05.png)

2.  Valige muutuja tüüp **string**

3.  Määrata muutuja nimi **AzureSubscriptionName**

    ![](media/site-recovery-runbook-automation/06.png)

4.  Määrake muutuv väärtusena oma tegelik Azure tellimuse nime.

    ![](media/site-recovery-runbook-automation/07_1.png)

Saate tuvastada Azure portaali konto sätete lehel oma tellimuse nime.

### <a name="add-an-azure-login-credential-as-asset"></a>Kui vara on Azure sisselogimismandaati lisamine

Azure'i automaatika kasutab Azure PowerShelli ühenduse tellimuse ja toimib esemeid seal. Seda peate autentida, kasutades oma Microsofti kontoga või töökoha või kooli kontoga.
Saate salvestada konto identimisteave vara käitusjuhendi turvaliseks kasutamiseks.

1.  Lisage uus säte ![](media/site-recovery-runbook-automation/04.png) Azure automatiseerimine varad ja valige![](media/site-recovery-runbook-automation/09.png)

2.  Valige **Windows PowerShelli mandaati** mandaadi tüüp

3.  Määrake nimi kujul **AzureCredential**

    ![](media/site-recovery-runbook-automation/10.png)

4.  Määrake kasutajanimi ja parool logige sisse.

Nüüd, kui mõlemad järgmised sätted on saadaval oma varasid.

![](media/site-recovery-runbook-automation/11.png)

Rohkem teavet selle kohta, kuidas teie tellimus PowerShelli kaudu ühenduse on esitatud [allpool](../powershell-install-configure.md).

Järgmiseks loote mõnda käitusjuhendi Azure'i automaatika, mida saab lisada pärast Tõrkesiirde ees virtuaalse masina lõpp-punkt.

## <a name="azure-automation-context"></a>Azure'i automaatika kontekst

ASR edastab käitusjuhendi kirjutamisel tarkadeks skriptide kontekstis muutujana. Üks võib leiavad, et pilveteenusesse ja virtuaalse masina nimi on hõlpsalt prognoosida, kuid juhtub, et ei ole alati tõttu teatud stsenaariumid, nt see, juhul kui võimalik virtuaalse masina nimi nimi on muutunud tõttu toetamata märgid Azure. See teave edastatakse seega ASR taastamise lepingu *raames*osana.

Allpool on näide sellest, kuidas kontekstis muutujana.

        {"RecoveryPlanName":"hrweb-recovery",

        "FailoverType":"Test",

        "FailoverDirection":"PrimaryToSecondary",

        "GroupId":"1",

        "VmMap":{"7a1069c6-c1d6-49c5-8c5d-33bfce8dd183":

                {"CloudServiceName":"pod02hrweb-Chicago-test",

                "RoleName":"Fabrikam-Hrweb-frontend-test"}

                }

        }


Järgmine tabel sisaldab nimi ja kirjeldus iga muutuja kontekstis.

**Muutuja nimi** | **Kirjeldus**
---|---
RecoveryPlanName | Käivitatud lepingu nimi. Aitab teil teha toimingu nime järgi sama skripti abil
FailoverType | Saate määrata, kas selle Tõrkesiirde on test, kavandatud või planeerimata.
FailoverDirection | Määrake, kas taastamine on esmane või teisene
GroupID | Määratlege rühma arvu taastamise kava leping töötamisel
VmMap | Massiivi kõik virtuaalmasinates jaotises.
VMMap võti | Iga VM kordumatu võti (GUID). See on sama, mis VMM ID virtuaalse masina vajaduse korral.
RoleName | Azure'i VM, et tagastada nimi
CloudServiceName | Azure'i pilveteenuses nimi, mille all on loodud virtuaalse masina.


Tuvastamiseks VmMap võti kontekstis võib ka minge ASR lehe VM atribuutide ja atribuudi VM GUID vaadata.

![](media/site-recovery-runbook-automation/13.png)

## <a name="author-an-automation-runbook"></a>Autor on käitusjuhendi automatiseerimine

Nüüd saate luua käitusjuhendi ees virtuaalse masina porti 80 avamiseks.

1.  Azure'i automaatika konto nime **OpenPort80** uue käitusjuhendi loomine

    ![](media/site-recovery-runbook-automation/14.png)

2.  Liikuge käitusjuhendi Autor vaate ja mustand režiimis.

3.  Esmalt määrama muutuja taastamise lepingu raames kasutamiseks

    ```
        param (
            [Object]$RecoveryPlanContext
        )

    ```

4.  Järgmine ühenduse tellimuse mandaat ja tellimuse nime abil

    ```
        $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

        # Connect to Azure
        $AzureAccount = Add-AzureAccount -Credential $Cred
        $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
        Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    ```

    Pange tähele, et kasutada Azure varad – **AzureCredential** ja **AzureSubscriptionName** siin.

5.  Nüüd määrake lõpp-punkti üksikasjad ja GUID virtuaalse masina, mille soovite seada lõpp-punkti. Sellisel juhul ees virtuaalse masina.

    ```
        # Specify the parameters to be used by the script
        $AEProtocol = "TCP"
        $AELocalPort = 80
        $AEPublicPort = 80
        $AEName = "Port 80 for HTTP"
        $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"
    ```

    Seda määrab Azure lõpp-punkti protokolli, kohaliku porti VM ja oma vastendatud avaliku porti. Need muutujad parameetrite nõutud Azure käskude lisamine VMs lõpp-punktid. Funktsiooni VMGUID hoiab virtuaalse masina käitamiseks peate GUID.

6.  Skripti nüüd ekstrakti kontekstis VM GUID-i jaoks ja luua virtuaalse masina viidatud selle lõpp.

    ```
        #Read the VM GUID from the context
        $VM = $RecoveryPlanContext.VmMap.$VMGUID

        if ($VM -ne $null)
        {
            # Invoke pipeline commands within an InlineScript

            $EndpointStatus = InlineScript {
                # Invoke the necessary pipeline commands to add a Azure Endpoint to a specified Virtual Machine
                # Commands include: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including parameters)

                $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                    Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                    Update-AzureVM
                Write-Output $Status
            }
        }
    ```

7. Kui see on valmis, vajutage avalda ![](media/site-recovery-runbook-automation/20.png) lubama oma skripti täitmise jaoks saadaval olla.

Täieliku script on esitatud allpool oma viide

```
  workflow OpenPort80
  {
    param (
        [Object]$RecoveryPlanContext
    )

    $Cred = Get-AutomationPSCredential -Name 'AzureCredential'

    # Connect to Azure
    $AzureAccount = Add-AzureAccount -Credential $Cred
    $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
    Select-AzureSubscription -SubscriptionName $AzureSubscriptionName

    # Specify the parameters to be used by the script
    $AEProtocol = "TCP"
    $AELocalPort = 80
    $AEPublicPort = 80
    $AEName = "Port 80 for HTTP"
    $VMGUID = "7a1069c6-c1d6-49c5-8c5d-33bfce8dd183"

    #Read the VM GUID from the context
    $VM = $RecoveryPlanContext.VmMap.$VMGUID

    if ($VM -ne $null)
    {
        # Invoke pipeline commands within an InlineScript

        $EndpointStatus = InlineScript {
            # Invoke the necessary pipeline commands to add an Azure Endpoint to a specified Virtual Machine
            # This set of commands includes: Get-AzureVM | Add-AzureEndpoint | Update-AzureVM (including necessary parameters)

            $Status = Get-AzureVM -ServiceName $Using:VM.CloudServiceName -Name $Using:VM.RoleName | `
                Add-AzureEndpoint -Name $Using:AEName -Protocol $Using:AEProtocol -PublicPort $Using:AEPublicPort -LocalPort $Using:AELocalPort | `
                Update-AzureVM
            Write-Output $Status
        }
    }
  }
```

## <a name="add-the-script-to-the-recovery-plan"></a>Skripti lisada taastamise kava

Kui skript on valmis, saate selle lisada taastamise kava varem loodud.

1.  Pärast rühma 2 skripti lisamiseks valige taastamise kava loodud. ![](media/site-recovery-runbook-automation/15.png)

2.  Määrake skripti nimi. See on ainult sõbralik nimi seda skripti kuvamine, taastamine lepingu raames.

3.  Valige Tõrkesiirde Azure skripti – Azure automatiseerimine konto nimi.

4.  Valige Azure tegevusraamatud, mille autoriks käitusjuhendi.

![](media/site-recovery-runbook-automation/16.png)

## <a name="primary-side-scripts"></a>Esmane pool skriptide

Azure'i Tõrkesiirde täitmisel saate valida ka esmane pool skriptide käivitada. Nende skriptide käivituvad VMM Server Tõrkesiirde ajal.
Esmane pool skriptide on saadaval ainult eelse sulgumist ainult ja sinna postitamine sulgumist etapid. Seda sellepärast, et saaksime oodata esmane saidi tavaliselt saadaval olla, kui katastroofi.
Ajal planeerimata Tõrkesiirde, ainult siis, kui valite esmane saidi toimingute, proovib see peamine küljel skriptide käitamiseks. Kui nad on kättesaadav või ajalõpp, on tõrkesiire jätkavad selle virtuaalmasinates taastada.
Esmane pool skriptid on kaitstud Azure - ajal Tõrkesiirde Azure VMM ilma un VMware, füüsilise, Hyper-v saitide jaoks saadaval.
Juhul, kui te migreerite Azure'i kohapealse, esmane pool skriptide (tegevusraamatud) saab kasutada kõigi sihtkohtade peale VMware jaoks.

## <a name="test-the-recovery-plan"></a>Testige taastamise kava

Kui olete lisanud käitusjuhendi lepingule, saate alustada test Tõrkesiirde ja näha seda toimingut. Soovitatav on alati käivitage test Tõrkesiirde testimiseks rakenduse ja taastamise kava tagamaks, et pole vigu.

1.  Valige taastamise kava ja algatada test Tõrkesiirde.

2.  Lepingu täitmisel, näete, kas käitusjuhendi on täidetud või mitte kaudu selle olek.

    ![](media/site-recovery-runbook-automation/17.png)

3.  Näete üksikasjalikku käitusjuhendi Andmetäite olek käitusjuhendi lehel Azure'i automaatika tööde haldamine.

    ![](media/site-recovery-runbook-automation/18.png)

4.  Pärast selle Tõrkesiirde lõpulejõudmist peale käitusjuhendi täitmise tulem, saate vaadata, kas täitmise õnnestumise või mitte külastamise Azure virtuaalse masina lehe ja vaadates lõpp-punktid.

![](media/site-recovery-runbook-automation/19.png)

## <a name="sample-scripts"></a>Näidisskriptid

Kuigi jalutasime automatiseerimine ühte sageli kasutatud ülesande lisamise lõpp on Azure virtuaalse masina selles õpetuses, kas sa arvu muude võimas automatiseerimise toimingute abil Azure automatiseerimine. Microsoft ja Azure automatiseerimine ühenduse pakuvad valimi tegevusraamatud, mis aitavad teil alustada luua oma lahenduste ja kasuliku tegevusraamatud, mida saate kasutada koosteüksuste suurem automatiseerimine ülesannete jaoks. Käivitage Galerii kaudu ja luua võimsaid ühe klõpsuga taastamine lepingute jaoks rakenduste kasutamine Azure saidi taastamine.

## <a name="additional-resources"></a>Lisaressursid

[Azure'i automaatika ülevaade] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure'i automaatika ülevaade")

[Azure'i automaatika näidisskriptid] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Azure'i automaatika näidisskriptid")
