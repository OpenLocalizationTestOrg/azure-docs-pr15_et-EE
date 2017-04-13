<properties 
   pageTitle="Lapse tegevusraamatud Azure'i automaatika | Microsoft Azure'i"
   description="Kirjeldatakse neid erinevaid viise Azure'i automaatika lisamine käitusjuhendi alates teise käitusjuhendi ja nende vahel teabe jagamine."
   services="automation"
   documentationCenter=""
   authors="mgoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="automation"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/17/2016"
   ms.author="magoedte;bwren" />

# <a name="child-runbooks-in-azure-automation"></a>Lapse tegevusraamatud Azure'i automaatika

See on parim Azure'i automaatika korduvkasutatav, muutuv tegevusraamatud eraldi funktsiooniga, mida saab kasutada muude tegevusraamatud kirjutamiseks. Ema käitusjuhendi sageli kõne ühe või mitme lapse tegevusraamatud teha vajalikke funktsioone. On kaks võimalust helistamiseks lapse käitusjuhendi ja iga on suur erinevus, tuleks mõista nii, et saate määrata, mis tuleb sobib teie erinevaid stsenaariumeid lähemalt.

##  <a name="invoking-a-child-runbook-using-inline-execution"></a>Lapse käitusjuhendi, kasutades Tekstisisese täitmise kasutada

Käitusjuhendi tekstisisene: teise käitusjuhendi viidata käitusjuhendi nime ning sisestama parameetrite väärtused täpselt samamoodi, nagu kasutaksite tegevuse või cmdlet-käsk.  Kõik tegevusraamatud sama automatiseerimise konto on saadaval kõigi teistele sel viisil kasutada. Ema käitusjuhendi ootama lapse käitusjuhendi lõpule jõuaks, enne liigub järgmisele reale ja mis tahes väljundi tagastatakse otse ema.

Kui kutsute käitusjuhendi tekstisisene, see töötab sama nimega ema käitusjuhendi tööd. Tekib viita lapse käitusjuhendi, mis selle töö ajalugu. Mis tahes erandid ja mis tahes voo väljund lapse käitusjuhendi seostatakse ema. See vähem tööd, on tulemuseks ja muudab need kergemini jälgida ja kuna visanud lapse käitusjuhendi ja mis tahes selle väljundi voo erandid on seotud vanema töö tõrkeotsing.

Kui mõne käitusjuhendi on avaldatud, mis tahes lapse tegevusraamatud, mis nõuab juba avaldada. See on Azure automatiseerimine koostab mis tahes lapse tegevusraamatud seos, kui mõnda käitusjuhendi koostatakse. Kui nad pole, ema käitusjuhendi kuvatakse õigesti avaldada, kuid loob erandi käivitamisel. Sel juhul saate ema käitusjuhendi selleks, et õigesti viidata lapse tegevusraamatud uuesti avaldada. Teil pole vaja ema käitusjuhendi uuesti avaldada, kui mõni laps tegevusraamatud on muutunud, kuna seost on juba loodud.

Lapse käitusjuhendi, nimetatakse Tekstisisese parameetrid võib olla mis tahes andmetüüp, sh keerukate objektide ja on pole [JSON sariväljaanne](automation-starting-a-runbook.md#runbook-parameters) on seal käitusjuhendi Azure haldusportaali abil käivitamisel või algus-AzureRmAutomationRunbook cmdlet-käsk.


### <a name="runbook-types"></a>Käitusjuhendi tüübid

Millist tüüpi saate helistada üksteist.

- [PowerShelli käitusjuhendi](automation-runbook-types.md#powershell-runbooks) ja [graafiline tegevusraamatud](automation-runbook-types.md#graphical-runbooks) saate helistada iga teksti sees, (mõlemad on vastavalt PowerShelli).
- [PowerShelli töövoo käitusjuhendi](automation-runbook-types.md#powershell-workflow-runbooks) ja graafiline PowerShelli töövoo tegevusraamatud saate helistada iga teksti sees, (mõlemad on PowerShelli töövoo põhine)
- PowerShelli tüüpide ja selle PowerShelli töövoog ei saa helistada üksteisest Tekstisisese ja tuleb kasutada algus-AzureRmAutomationRunbook.
    
Kui avaldamine tellimuse küsimuse:

- Avalda järjestuse tegevusraamatud küsimustes ainult PowerShelli töövoog ja graafiline PowerShelli töövoog tegevusraamatud.


Graafilised või PowerShelli töövoo lapse käitusjuhendi abil Tekstisisese täitmise helistamisel lihtsalt kasutada käitusjuhendi nime.  Kui helistate PowerShelli lapse käitusjuhendi, saate peab ees on nimi, mis on *.\\ * Kui soovite määrata, et skript asub kohalikku kausta. 

### <a name="example"></a>Näide

Järgmine näide kasutab testi lapse käitusjuhendi, mida aktsepteerib kolme parameetrite, keerukate objekti, täisarv ja on kahendväärtus. Lapse käitusjuhendi väljund on määratud muutujana.  Sel juhul on lapse käitusjuhendi PowerShelli töövoo käitusjuhendi

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = PSWF-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true

Järgmine on sama näide, kasutades PowerShelli käitusjuhendi lapse.

    $vm = Get-AzureRmVM –ResourceGroupName "LabRG" –Name "MyVM"
    $output = .\PS-ChildRunbook –VM $vm –RepeatCount 2 –Restart $true



##  <a name="starting-a-child-runbook-using-cmdlet"></a>Lapse käitusjuhendi, kasutades cmdlet-käsu käivitamine

[Algus-AzureRmAutomationRunbook](https://msdn.microsoft.com/library/mt603661.aspx) cmdlet-käsu abil saate alustada on käitusjuhendi, nagu on kirjeldatud [on käitusjuhendi Windows PowerShelliga alustamiseks](../automation-starting-a-runbook.md#starting-a-runbook-with-windows-powershell). On kaks viisi cmdlet-käsu kasutamine.  Ühe režiimis, tagastab cmdlet töö id niipea, kui luuakse lapse käitusjuhendi lapse töö.  Muud režiimis, mis lubate, määrates selle **-oodake** parameeter, oodake cmdlet lapse töö viimistluse ja väljund tulu lapse käitusjuhendi.

Lapse käitusjuhendi, alustamine cmdlet-käsu kaudu töö kestab eraldi töö ema käitusjuhendi kaudu. See rohkemate, kui kasutada käitusjuhendi teksti sees, on tulemuseks ning muudab need raskem jälgida. Ema saate alustada mitme lapse tegevusraamatud asünkroonselt ootamata iga lõpuleviimiseks. Paralleelne helistamiseks lapse tegevusraamatud Tekstisisese sama sellist ema käitusjuhendi oleks vaja kasutada [paralleelselt märksõna](automation-powershell-workflow.md#parallel-processing).

Parameetrite lapse käitusjuhendi, alustamine cmdlet-käsu jaoks on olemas on Hashtable talletatakse kirjeldatud [Käitusjuhendi parameetrid](automation-starting-a-runbook.md#runbook-parameters). Saab kasutada ainult lihtsa andmetüübid. Kui käitusjuhendi on parameetri keerukate andmetüübiga, siis see peab käivitama teksti sees.

### <a name="example"></a>Näide

Järgmises näites alustab lapse käitusjuhendi parameetrite ja ootab selle lõppemist abil algus-AzureRmAutomationRunbook-oodake parameeter. Kui lõpule jõudnud, on selle väljundi kogutud lapse käitusjuhendi.

    $params = @{"VMName"="MyVM";"RepeatCount"=2;"Restart"=$true} 
    $joboutput = Start-AzureRmAutomationRunbook –AutomationAccountName "MyAutomationAccount" –Name "Test-ChildRunbook" -ResouceGroupName "LabRG" –Parameters $params –wait


## <a name="comparison-of-methods-for-calling-a-child-runbook"></a>Helistamine lapse käitusjuhendi meetodite võrdlus

Järgmises tabelis on toodud kaks võimalust jaoks soovitud käitusjuhendi helistades teise käitusjuhendi erinevused.

| | Teksti sees| Cmdlet-käsk|
|:---|:---|:---|
|Töö|Lapse tegevusraamatud töötavad sama nimega ema tööd.|Lapse käitusjuhendi luuakse eraldi töö.|
|Täitmise|Ema käitusjuhendi ootab lapse käitusjuhendi lõpuleviimiseks enne jätkamist.|Ema käitusjuhendi ei lahene, kohe pärast lapse käitusjuhendi käivitatakse *või* ema käitusjuhendi ootab lapse töö lõpuleviimiseks.|
|Väljund|Ema käitusjuhendi pääsevad otse väljundi lapse käitusjuhendi kaudu.|Ema käitusjuhendi peab tuua väljundi lapse käitusjuhendi töö *- või* otse saada ema käitusjuhendi väljundi lapse käitusjuhendi.|
|Parameetrid|Lapse käitusjuhendi parameetrite väärtused on määratud eraldi ja saate kasutada mis tahes andmetüüp.|Lapse käitusjuhendi parameetrite saab ühendada ühe Hashtable talletatakse ja võib sisaldada ainult lihtne, väärtusi massiiv ja andmetüüpide selle võimendada JSON sariväljaanne objekti.|
|Automaatika konto|Ema käitusjuhendi saate kasutada ainult lapse käitusjuhendi automatiseerimine sama kontoga.|Ema käitusjuhendi saate kasutada mis tahes automatiseerimise kontolt sama Azure tellimuse ja mõnda muud tellimust lapse käitusjuhendi, kui teil on selle ühenduse.|
|Avaldamine|Lapse käitusjuhendi peab olema avaldatud enne ema käitusjuhendi on avaldatud.|Lapse käitusjuhendi peab olema avaldatud iga kord enne alustamist ema käitusjuhendi.|

## <a name="next-steps"></a>Järgmised sammud

- [Alates on käitusjuhendi Azure automatiseerimine](automation-starting-a-runbook.md)
- [Käitusjuhendi väljundi ja sõnumite Azure automatiseerimine](automation-runbook-output-and-messages.md)
