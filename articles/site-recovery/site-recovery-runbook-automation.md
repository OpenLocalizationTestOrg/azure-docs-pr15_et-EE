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


# <a name="add-azure-automation-runbooks-to-recovery-plans"></a>Azure'i automaatika tegevusraamatud taastamise lisamine


Selles õppetükis kirjeldatakse, kuidas Azure saidi taastamise integreerub Azure automatiseerimine laiendatavuse taastamise esitada. Taastamise saate korraldab oma virtuaalmasinates kaitstud Azure saidi taastamise nii dispersioonanalüüs teisene pilveteenusesse ja Azure stsenaariumid kopeerimise taastamine. Samuti aitavad tegemisel taastamine **süsteemne täpne**, **korduvate**ja **automatiseeritud**. Kui te ei suuda üle oma Azure'i virtuaalmasinates, integreerimine Azure automatiseerimine laiendab taastamine lepingud ja pakub võimalus tegevusraamatud, võimaldades võimas automaatika ülesandeid täita.

Kui te ei ole kuulnud Azure automatiseerimine veel, registreeruda [siin](https://azure.microsoft.com/services/automation/) ja nende valimi skriptide allalaadimiseks [siin](https://azure.microsoft.com/documentation/scripts/). Lugege lisateavet [Azure saidi taastamine](https://azure.microsoft.com/services/site-recovery/) ja kuidas korraldab taastamine Azure taastamise abil [siin](https://azure.microsoft.com/blog/?p=166264).

Selles õpetuses käsitleme kuidas saate taastamise Azure'i automaatika tegevusraamatud integreerida. Me lihtsa varasemas versioonis nõutav manuaalset toiminguid automatiseerida ja vaadake, kuidas muuta ühe klõpsuga taastamine toimingu mitme toimingu taastamine. Me ka vaatame, kuidas saate otsida lihtsa kui valesti läheb.

## <a name="customize-the-recovery-plan"></a>Taastamise kava kohandamine

1. Andke meile kõigepealt operning ressursi tera taastamise kava. Saate vaadata taastamise kava on kaks virtuaalmasinates lisatakse see taaskasutamiseks. 

    ![](media/site-recovery-runbook-automation-new/essentials-rp.PNG)
---------------------

2. Klõpsake nuppu Kohanda hakata on käitusjuhendi. See avab taastamise kava blade kohandamine.


    ![](media/site-recovery-runbook-automation-new/customize-rp.PNG)


3. Paremklõpsake jaotises Alusta 1 ja valige lisamine postitusele toimingu lisamiseks.

4. Valige uus laba skripti valida.

5. Nime skripti "Tere, maailm".

6. Valige automatiseerimise konto nimi. See on konto Azure automatiseerimine. Pange tähele, et selle konto võib olla mis tahes Azure'i geograafia, kuid peab olema sama nimega saidi taastamise hoidla tellimus.

7. Valige soovitud käitusjuhendi kontolt automatiseerimine. See on skripti, mis töötab pärast taastamis esimese rühma taastamine lepingu täitmise ajal.

    ![](media/site-recovery-runbook-automation-new/update-rp.PNG)


8. Valige lisatav skript salvestamiseks nuppu OK. Selle jaotise postitusele toimingu rühma 1 skripti: käivitamine.


    ![](media/site-recovery-runbook-automation-new/addedscript-rp.PNG)


## <a name="salient-points-of-adding-a-script"></a>Olulisemad punktid skripti lisamine

1. Saate skripti paremklõps ja valige "kustutamine toimingut" või "värskendada skripti".

2. Skripti Azure Tõrkesiirde ajal käivitada asutusesisesest Azure ja käitamise Azure esmane pool skripti enne sulgumist, kui migreerite Azure'i kohapealse ajal.

3. Kui skript töötab, kuvatakse see annavad taastamise lepingu raames.

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
CloudServiceName (mudelis ressursihaldur juurutamise) | Mis on loodud virtuaalse masina Azure'i ressursirühma nimi.


## <a name="using-complex-variables-per-recovery-plan"></a>Kompleksarvu muutujate kohta taastamise kava abil

Mõnikord on käitusjuhendi nõuab rohkem teavet kui lihtsalt RecoveryPlanContext. On edasi parameeter on käitusjuhendi pole esimese klassi süsteem. Siiski saate kui soovite kasutada sama skripti kaudu mitme taastamise lepingute kasutada taastamise leping kontekstis muutuja 'RecoveryPlanName' ja kasutada funktsiooni all katse meetod on Azure automatiseerimine kompleksarvu muutuja kasutamine on käitusjuhendi. Järgmises näites on kuvatud, kuidas saate luua kolme eri kompleksarvu muutuja varad ja kasutada neid käitusjuhendi põhjal taastamine lepingu nimi.

Võtke arvesse, et soovite 3 täiendavate parameetrite kasutamine on käitusjuhendi. Andke meile kodeerida neid JSON vormi {"ZM1": "testautomation", "Var2": "Planeerimata", "Var3": "PrimaryToSecondary"}

[AA kompleksarvu muutuja](../automation/automation-variables.md#variable-types Complex variable) abil saate luua uue automatiseerimise vara.
Kui muutuja nimi <RecoveryPlanName>http - Authentication.
Siin on viide abil saate luua [kompleksarvu muutujana](https://msdn.microsoft.com/library/dn913767.aspx?f=255&MSPPError=-2147217396).

Erinevate taastamise lepingute korral nime nagu muutuja

1. recoveryPlanName1 >-http Authentication
2. recoveryPlanName2 >-http Authentication
3. recoveryPlanName3 >-http Authentication

Nüüd skripti viidata selle nimega http Authentication

1. RP nime toomine selle $rpname = $Recoveryplancontext muutuja
2. Saada $paramValue vara = "$($rpname)-http Authentication"
3. Kasutada seda kompleksarvu muutuja taastamise kava helistades Get-AzureAutomationVariable [-AutomationAccountName] <String> -$paramValue nimi.

Näiteks SharepointApp taastamise kava kompleksarvu muutuja/parameetri saamiseks luua Azure automatiseerimine kompleksarvu muutuja nimega "SharepointApp-http Authentication".

Kasutada seda taastamise kava muutuja ekstraktimine vara kasutamine lause Get-AzureAutomationVariable [-AutomationAccountName] <String> [-nime] $paramValue. [See viide üksikasjalikumat teavet](https://msdn.microsoft.com/library/dn913772.aspx)

Sellisel viisil sama skripti saab kasutada erinevate taastamise leping talletades leping teatud kompleksarvu muutuja vara.

## <a name="sample-scripts"></a>Näidisskriptid

Hoidla skripte, mis saate otse automatiseerimise kontole importida, leiate [Kristian Hiina OMS hoidla skriptide](https://github.com/krnese/AzureDeploy/tree/master/OMS/MSOMS/Solutions/asrautomation)

Skript siin on Azure ressursihaldur Mall, mida saavad kõik juurutamine selle all skriptide

* NSG

NSG käitusjuhendi kuvatakse määramine avaliku IP-aadressid iga VM sees taastamise kava ja lisada oma virtuaalse võrguadapteri võrgu turberühma, mis võimaldab vaikimisi suhtlus

* PublicIP

Avaliku IP käitusjuhendi määrab iga VM sees taastamise kava avaliku IP-aadressid. Juurdepääs masinad ja rakenduste sõltub tulemüüri sätted iga külaline sees


* CustomScript

CustomScript käitusjuhendi on avaliku IP-aadressi määramiseks iga VM sees taastamise kava ja installige kohandatud skript laiend, mis pull skripti viitate juurutamisel Mall

* NSGwithCustomScript

NSGwithCustomScript, käitusjuhendi iga VM sees on taastamine kavandamine, luua kohandatud skript kasutamine laiend ja luua ühenduse virtuaalse võrguadapteri NSG, mis võimaldab installi avaliku IP-aadressid määrata vaikimisi Kaugpöördusteenuse sissetulevate ja väljaminevate teatis

## <a name="additional-resources"></a>Lisaressursid

[Azure automatiseerimine teenuse käivitamine meilikontoga](../automation/automation-sec-configure-azure-runas-account.md)

[Azure'i automaatika ülevaade] (http://msdn.microsoft.com/library/azure/dn643629.aspx "Azure'i automaatika ülevaade")

[Azure'i automaatika näidisskriptid] (http://gallery.technet.microsoft.com/scriptcenter/site/search?f[0].Type=User&f[0].Value=SC%20Automation%20Product%20Team&f[0].Text=SC%20Automation%20Product%20Team "Azure'i automaatika näidisskriptid")
