<properties 
   pageTitle="Õppekeskuse PowerShelli töövoo"
   description="Selles artiklis on mõeldud kiirülevaate õppetüki autorid tuttav PowerShelli mõista PowerShelli ja PowerShelli töövoo teatud erinevused."
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
   ms.date="09/12/2016"
   ms.author="bwren" />

# <a name="learning-windows-powershell-workflow"></a>Õppekeskuse Windows PowerShelli töövoo

Azure'i automaatika tegevusraamatud on rakendada Windows PowerShelli töövood.  Windows PowerShelli töövoo sarnaneb Windows PowerShelli skripti, kuid on mõned olulised erinevused, mis võib olla segane uuele kasutajale.  Selles artiklis on mõeldud kasutajatele juba tuttav PowerShelli ja lühidalt selgitatakse, kui teisendate PowerShelli skripti PowerShelli töövoo lisamine käitusjuhendi kasutamiseks vajate põhimõtet.  

Töövoo on programmeeritud, ühendatud toiminguid, mis nõuavad mitme juhiseid koordineerimine mitmest seadmete või hallatavate sõlmed või pikaajalisi ülesanded jada. Töövoo üle tavaline skripti eelised sisaldavad võimalus korraga mitme seadmete vastu toimingu sooritamine ja võimalus automaatselt taastamine ebaõnnestumist. Windows PowerShelli töövoo on Windows PowerShelli skripti Windows töövoo Foundationi varundustarkvara. Kui töövoog on kirjutada Windows PowerShelli süntaks ja käivitanud Windows PowerShelli, töötleb Windows töövoo Foundation.

Üksikasjalikke juhiseid selle artikli teemad, lugege teemat [Alustamine Windows PowerShelli töövoo](http://technet.microsoft.com/library/jj134242.aspx).

## <a name="types-of-runbook"></a>Käitusjuhendi tüübid

On kolme tüüpi käitusjuhendi Azure'i automaatika, *PowerShelli töövoo*, *PowerShelli* ja *graafiline*.  Kui loote käitusjuhendi ja on käitusjuhendi ei saa teisendada muud tüüpi, kui see on loodud määratleda käitusjuhendi tüüp.

PowerShelli töövoo tegevusraamatud ja PowerShelli tegevusraamatud kasutajatele, kes eelistate töötada otse PowerShelli koodi kas kasutate teksti redaktori Azure'i automaatika või ühenduseta redaktoris nagu PowerShell ISE. Selle artikli teave peaks mõista, kui loote PowerShelli töövoo käitusjuhendi. 

Graafilise tegevusraamatud võimaldavad käitusjuhendi, samas tegevuste ja cmdlet-käskude abil, kuid graafilist liidest, mis peidab keerulisi aluseks PowerShelli töövoo loomine.  Selle artikli teemad, nt postkastid ja paralleelne põhimõtet endiselt rakendamine graafiline tegevusraamatud, kuid ei pea te muretsema üksikasjalik süntaks. 

## <a name="basic-structure-of-a-workflow"></a>Töövoo põhistruktuur

Esimene samm teisendamise PowerShelli skripti PowerShelli töövoo on see **töövoog** märksõna lisades.  Töövoo käivitab **töövoo** märksõna, millele järgneb skripti, mis on ümbritsetud looksulgudega keha. Selle töövoo nime, järgib **töövoo** märksõna, nagu on näidatud järgmist süntaksit. 

    Workflow Test-Workflow
    {
       <Commands>
    }

Selle töövoo nime, peab vastama automatiseerimise käitusjuhendi nime. Kui käitusjuhendi on imporditud, siis faili nimi peab vastama töövoo nimi ja klõpsake .ps1 lõpus peab olema.

Parameetrite töövoo lisamiseks kasutage võtmesõna **Param** , nagu teeksite skripti. 

## <a name="code-changes"></a>Koodi muudatusi

PowerShelli töövoo kood näeb välja peaaegu sama PowerShelli skripti koodi, välja arvatud mõned olulised muudatused.  Järgmistes jaotistes kirjeldatakse muudatused, mida on vaja teha seda PowerShelli skripti töövoo käivitada.

### <a name="activities"></a>Tegevuste

Tegevus on kindla tööülesande töövoo. Skripti koosneb ühe või mitme käsud, nagu töövoo koosneb ühe või mitme tegevusi, mis tehakse järjestuses. Windows PowerShelli töövoo teisendab automaatselt paljude Windows PowerShelli cmdlet-käskude tegevuste kui töövoog töötab. Määrates nende cmdlettide oma käitusjuhendi, haldab vastava tegevuse tegelikult Windows töövoo Foundation. Nende cmdlettide ilma vastavate tegevuste, Windows PowerShelli töövoog käivitub automaatselt cmdlet [InlineScript](#inlinescript) tegevuse. On cmdlet-käsud, mis ei kuulu ja ei saa kasutada töövoo juhul, kui kaasate konkreetselt need on InlineScript blokeerimine kogum. Vt täpsemat teavet nende põhimõtet [Skripti töövoogude abil tegevusi](http://technet.microsoft.com/library/jj574194.aspx).

Töövoo toiminguid jagada levinud parameetrite konfigureerimiseks nende toimimist. Töövoo levinud parameetrite kohta leiate üksikasjalikumat teavet teemast [about_WorkflowCommonParameters](http://technet.microsoft.com/library/jj129719.aspx).

### <a name="positional-parameters"></a>Positsiooniga seotud parameetrid

Ei saa kasutada positsiooniga seotud parameetrite tegevuste ja töövoo cmdlet-käskude abil.  Tähendab on, et peate kasutama parameetrite nimed.

Võtame näiteks järgmine kood, mis toob kõik töötavad teenused.

     Get-Service | Where-Object {$_.Status -eq "Running"}

Kui proovite seda sama koodi töövoo käivitada, kuvatakse sõnumi nagu "parameeter ei saa lahendada, kasutades määratud nimega parameetrid."  Probleemi lahendamiseks lihtsalt pakkuda parameetri nimi nagu järgmist.

    Workflow Get-RunningServices
    {
        Get-Service | Where-Object -FilterScript {$_.Status -eq "Running"}
    }

### <a name="deserialized-objects"></a>Deserialized objektid

Töövoogude objektide sarjadesse jaotamist tühistada.  See tähendab, et nende atribuudid on veel saadaval, kuid mitte nende meetodite.  Näiteks võtke arvesse järgmist PowerShelli koodi, mis lõpetab teenuse objekti meetodil Peata teenus.

    $Service = Get-Service -Name MyService
    $Service.Stop()

Kui proovite selle töövoo käivitada, kuvatakse tõrge "meetod kutsumise ei toeta Windows PowerShelli töövoo".  

Üheks võimaluseks on mähkimiseks need kaks koodiread on millised juhul $Service [InlineScript](#InlineScript) plokk oleks teenuse objekti jooksul blokeerida. 

    Workflow Stop-Service
    {
        InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
        }
    } 

Teine võimalus on kasutada mõne muu cmdlet, mis sooritavad samu funktsioone, mis meetodit, kui see on saadaval.  Meie valimi puhul cmdlet Peata-teenus pakub samu funktsioone, mis meetod ja töövoo võite kasutada järgmist.

    Workflow Stop-MyService
    {
        $Service = Get-Service -Name MyService
        Stop-Service -Name $Service.Name
    }


## <a name="inlinescript"></a>InlineScript

**InlineScript** tegevuse on kasulik, kui peate ühe või mitme käsud traditsiooniline PowerShelli skripti asemel PowerShelli töövoo käivitada.  Ajal töövoo käsud saadetakse Windows töövoo Foundationi töötlemiseks, töötleb käsud on InlineScript Blokeeri Windows PowerShelli. 

InlineScript kasutab süntaks allpool näidatud.

    InlineScript
    {
      <Script Block>
    } <Common Parameters>

Mõne InlineScript saate väljundi tulu määrates väljund muutujana. Järgmises näites lõpetab teenuse ja seejärel väljundi teenuse nimi.

    Workflow Stop-MyService
    {
        $Output = InlineScript {
            $Service = Get-Service -Name MyService
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Saate väärtused lähevad mõne InlineScript blokeerimine, kuid peate kasutama **$Using** ulatus muutja.  Järgmises näites on identne eelmises näites, välja arvatud teenuse nimi osutatakse muutujaga. 

    Workflow Stop-MyService
    {
        $ServiceName = "MyService"
    
        $Output = InlineScript {
            $Service = Get-Service -Name $Using:ServiceName
            $Service.Stop()
            $Service
        }

        $Output.Name
    }


Kuigi InlineScript tegevuste võib olla teatud töövoogudes kriitiline, ei toeta töövoo importida ning kasutada ainult vajadusel järgmistel põhjustel:

- Te ei saa kasutada [postkastid](#Checkpoints) on InlineScript Blokeeri sees. Kui tõrge ilmneb jooksul blokeerida, tuleb taastada blokeering algusest.
- Te ei saa kasutada [paralleelne](#parallel-execution) sees on InlineScriptBlock.
- InlineScript mõjutab skaleeritavus töövoo, kuna tal Windows PowerShelli seansi InlineScript blokeeri kogu pikkus.

Täpsemaks InlineScript kasutamise kohta leiate teemast [Opsüsteemi Windows PowerShelli käskude töövoo](http://technet.microsoft.com/library/jj574197.aspx) ja [about_InlineScript](http://technet.microsoft.com/library/jj649082.aspx).


## <a name="parallel-processing"></a>Paralleelne töötlemine

Üks eeliseid Windows PowerShelli töövood on võimalus teha kogum käsud paralleelselt asemel järjest nagu tavalise skripti abil. 

**Paralleelsetes** märksõna abil saate luua skripti plokk käivitavad samaaegselt mitu käskudega. See kasutab süntaks allpool näidatud. Sel juhul nυuded1 ja Activity2 hakkab samal ajal. Activity3 hakkavad alles nυuded1 nii Activity2 on.

    Parallel
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>


Näiteks, võtke arvesse järgmist PowerShelli käske, mis kopeerida mitut faili võrgu sihtkohta.  Need käsud käivitada järjest nii ühe faili peab lõpule kopeerimise enne järgmise alustamist.     

    $Copy-Item -Path C:\LocalPath\File1.txt -Destination \\NetworkPath\File1.txt
    $Copy-Item -Path C:\LocalPath\File2.txt -Destination \\NetworkPath\File2.txt
    $Copy-Item -Path C:\LocalPath\File3.txt -Destination \\NetworkPath\File3.txt

Järgmised töövoog käivitatakse need sama käsud paralleelselt nii, et need kõik alustada samal ajal kopeerimist.  Ainult siis, kui need on kõik täielikult kopeeritud lõpetamise teade kuvatakse.

    Workflow Copy-Files
    {
        Parallel 
        {
            $Copy-Item -Path "C:\LocalPath\File1.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File2.txt" -Destination "\\NetworkPath"
            $Copy-Item -Path "C:\LocalPath\File3.txt" -Destination "\\NetworkPath"
        }

        Write-Output "Files copied."
    }


Saate kasutada funktsiooni **ForEach-paralleelselt** ehitada protsessi käske kogumi samaaegselt iga üksuse jaoks. Üksuste kogumine töödeldakse paralleelselt ajal skripti plokk käsud käivitada järjest. See kasutab süntaks allpool näidatud. Sel juhul hakkavad nυuded1 kõigi üksuste kogumine samal ajal. Iga üksuse jaoks hakkavad Activity2 kui nυuded1 on lõpule jõudnud. Activity3 hakkavad alles nυuded1 nii Activity2 on kõigi üksuste.

    ForEach -Parallel ($<item> in $<collection>)
    {
      <Activity1>
      <Activity2>
    }
    <Activity3>

Järgmine näide sarnaneb eelmise näite kopeerimist samal ajal faile.  Sellisel juhul kuvatakse teade iga faili pärast selle valiku puhul kopeeritakse.  Ainult siis, kui need on kõik täielikult kopeeritud lõpetamise teade kuvatakse.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach -Parallel ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
        }
        
        Write-Output "All files copied."
    }

> [AZURE.NOTE]  Me ei soovita lapse tegevusraamatud töötab samal ajal, kuna see on näidatud usaldusväärsed tulemused.  Lapse käitusjuhendi mõnikord väljund ei kuvata ja sätete ühe käitusjuhendi võib mõjutada muude paralleelselt lapse tegevusraamatud 


## <a name="checkpoints"></a>Postkastid

*Kontrollpunkt* on töövoo, mis sisaldab muutujate praegust väärtust ja mis tahes väljundi loodud selle punkti hetkeseisu hetktõmmise. Kui töövoog lõpeb tõrge või peatatakse, siis järgmine kord, kui seda ei tee seda alustan selle asemel funktsiooni worfklow algus Viimane kontrollpunkt.  Saate seada postkasti töövoo **Kontrollpunkt-töövoo** tegevusega.

Järgmine proovi kood, erandi esineb pärast Activity2 põhjustada töövoo lõpetada. Kui töövoog käivitatakse uuesti, see algab Activity2 töötab, kuna see oli lihtsalt pärast viimast kontrollpunkt.

    <Activity1>
    Checkpoint-Workflow
    <Activity2>
    <Exception>
    <Activity3>

Määrake postkastid töövoo pärast tegevused, mida võib erand ja ei tohiks korrata, kui töövoog on jätkata. Näiteks võib oma töövoo luua virtuaalse masina. Saate väärtuseks postkasti enne ja pärast käskude virtuaalse masina loomiseks. Kui loomine nurjub, siis oleks käske korrata, kui töövoog käivitatakse uuesti. Kui soovitud funktsiooni worfklow nurjub pärast loomist on loodud, siis virtuaalse masina ei looda uuesti kui töövoog jätkatakse.

Järgmises näites kopeerib võrgukoht mitu faili ja määrab postkasti pärast iga faili.  Kui võrgukoht läheb kaotsi, siis töövoog lõpeb tõrge.  Kui see on uuesti luua, see jätkub veebisaidil Viimane kontrollpunkt, mis tähendab, et ainult failid, mis on juba kopeeritud jäetakse vahele.

    Workflow Copy-Files
    {
        $files = @("C:\LocalPath\File1.txt","C:\LocalPath\File2.txt","C:\LocalPath\File3.txt")

        ForEach ($File in $Files) 
        {
            $Copy-Item -Path $File -Destination \\NetworkPath
            Write-Output "$File copied."
            Checkpoint-Workflow
        }
        
        Write-Output "All files copied."
    }

Kuna kasutajanimi identimisteave on mitte hoitakse pärast seda, kui helistate [Peata-töövoo](https://technet.microsoft.com/library/jj733586.aspx) tegevuse või Viimane kontrollpunkt, peate määrama null ja seejärel toomiseks neid uuesti poest varade pärast **Peata-töövoos** või kontrollpunkt nimetatakse identimisteabe.  Muul juhul võidakse kuvada järgmine tõrketeade: *töövoo töö ei saa jätkata, kas Kuna püsimine andmete võib ei salvestata täielikult ega salvestatud on rikutud püsimine andmed. Töövoog tuleb taaskäivitada.*

Järgmine kood sama näitab, kuidas ta seda oma PowerShelli töövoo tegevusraamatud.

       
    workflow CreateTestVms
    {
       $Cred = Get-AzureAutomationCredential -Name "MyCredential"
       $null = Add-AzureRmAccount -Credential $Cred

       $VmsToCreate = Get-AzureAutomationVariable -Name "VmsToCreate"

       foreach ($VmName in $VmsToCreate)
         {
          # Do work first to create the VM (code not shown)
        
          # Now add the VM
          New-AzureRmVm -VM $Vm -Location "WestUs" -ResourceGroupName "ResourceGroup01"

          # Checkpoint so that VM creation is not repeated if workflow suspends
          $Cred = $null
          Checkpoint-Workflow
          $Cred = Get-AzureAutomationCredential -Name "MyCredential"
          $null = Add-AzureRmAccount -Credential $Cred
         }
     } 


See ei ole vajalik, kui teil on autentimist kasutades konfigureeritud põhisumma teenuse käivitamine nimega konto.  

Postkastid kohta leiate lisateavet teemast [Skripti töövoo postkastid lisamine](http://technet.microsoft.com/library/jj574114.aspx).


## <a name="next-steps"></a>Järgmised sammud

- Alustamine PowerShelli töövoo tegevusraamatud, lugege teemat [minu esimese PowerShelli töövoo käitusjuhendi](automation-first-runbook-textual.md) 
