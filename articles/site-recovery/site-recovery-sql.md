<properties 
    pageTitle="Kaitse SQL serveri SQL serveri avariitaastet ja Azure saidi taastamine | Microsoft Azure'i" 
    description="Selles artiklis kirjeldatakse, kuidas kopeerida SQL Server Azure'i saidi taastamise SQL serveri katastroofi võimaluste kasutamine." 
    services="site-recovery" 
    documentationCenter="" 
    authors="rayne-wiselman" 
    manager="jwhit" 
    editor=""/>

<tags 
    ms.service="site-recovery" 
    ms.workload="backup-recovery" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/04/2016" 
    ms.author="raynew"/>


# <a name="protect-sql-server-with-sql-server-disaster-recovery-and-azure-site-recovery"></a>Kaitse SQL serveri SQL serveri avariitaastet ja Azure saidi taastamine 


Azure'i saidi taastamise teenuse aitab teie ettevõtte järjepidevus ja avariijärgne taastamine (BCDR) strateegia orkesteroinnin dispersioonanalüüs, Tõrkesiirde ja virtuaalmasinates ja füüsilise serveri. Masinad saab paljundada Azure'i või sekundaarne asutusesisese andmekeskuse. Kiire ülevaate saamiseks lugege [mis on Azure saidi taastamise?](site-recovery-overview.md).

 Selles artiklis kirjeldatakse, kuidas kaitsta SQL serveri tagasi lõpuks rakendus SQL serveri BCDR tehnoloogiad ja Azure saidi taastamise kombinatsiooni. Peate SQL serveri funktsioonid hea mõistmine (Tõrkesiirde rühmitamise, Kättesaadavusrühmad, andmebaasi peegelduse Logi saatmine) ja Azure saidi taastamise enne juurutamist stsenaariumid, mis on selles artiklis kirjeldatud.



## <a name="overview"></a>Ülevaade

Mitme töökoormus kasutada SQL serveri aluseks. Rakendusi nagu Dynamics SharePointi ja SAP-i abil SQL serveri andmeteenuste rakendada.  Rakenduste SQL serveri juurutamine mitmel erineval viisil:

- **Autonoomse SQL serveri**: SQL Server ja kõik andmebaasid on majutatud ühe masina (füüsilise või virtuaalne). Virtualiseeritud, kasutatakse host rühmitamise kõrge kättesaadavust. Pole Külastajate taseme kõrge-saadavus rakendatakse.
- **SQL Serveri Tõrkesiirde rühmitamise eksemplarides (alati sisse FCI)**: SQL Serveri eksemplari ühiskasutusega ketast kahe või enama sõlmed on konfigureeritud Windows tõrkesiirdeklastrite. Kui mõni kobar eksemplarid on alla, klaster ei pruugi üle SQL serveri eksemplar. See häälestus on tavaliselt kasutatakse HA esmane saidil. See tõrge või katkestuste ühine kiht ei kaitse. Ühiskasutuses ketta saab rakendada ISCSI Fiber kanali või ühiskasutusega VHDx abil.
- **SQL-i alati klõpsake kättesaadavus rühmad**: selle häälestamine on kaks sõlme häälestus ühiskasutusega midagi klaster kättesaadavuse nimel sünkroonse kopeerimise ja automaatne Tõrkesiirde konfigureeritud SQL serveri andmebaasidega.

Ettevõtte väljaannetes, SQL serveri näeb kohalikke katastroofi taastamise tehnoloogiad taastamisel andmebaase serveri saidi. Selles artiklis me kasutada ja integreerida neid kohalikke SQL-i katastroofi taastamise tehnoloogiad: 

- SQL-i alati klõpsake kättesaadavus rühmad avariitaastet SQL Server 2012 või 2014 Enterprise'i.
- SQL-i andmebaasi peegeldus kõrge ohutus režiimis SQL Server 2008 R2 või SQL Server Standard edition (kõik versioonid).


Saidi taastamine saate SQL serveri kaitsta, nagu on kokku võetud tabelis.

 |**Kohapealse kohapealse** | **Kohapealse Azure** 
---|---|---
**Hyper-V** | Jah | Jah
**VMware** | Jah | Jah 
**Füüsiline server** | Jah | Jah


## <a name="support-and-integration"></a>Tugi- ja integreerimine

Nende SQL serveri versiooni toetatud stsenaariumid selle artikli teemad:


- SQL serveri 2014 Enterprise ja Standard
- SQL Server 2012 SP1 ja standardne
- SQL Server 2008 R2 SP1 ja standardne


Saidi taastamine saab integreerida kohalikke BCDR SQL serveri tehnoloogiate summeeritud katastroofi taastamise lahendus järgmises tabelis.

**Funktsioon** |**Üksikasjad** | **SQL serveri versioon** 
---|---|---
**Kättesaadavusrühma** | Autonoomse mitmes eksemplaris, SQL serveri käivitada tõrkesiirdeklastrite, milles on mitme sõlmed.<br/><br/>Andmebaasid saate rühmitada Tõrkesiirde rühmad, mida saab kopeerida (peegelpilt) SQL serveri aknad nii, et ei jagatud salvestusruumi on vaja.<br/><br/>Pakub avariitaastet esmane saidi ja ühe või mitme teisene saitidel vahel. Kaks sõlme saate häälestada rakenduses ühiskasutusega midagi klaster kättesaadavuse nimel sünkroonse kopeerimise ja automaatne Tõrkesiirde konfigureeritud SQL serveri andmebaasidega. | SQL serveri 2014 & 2012 Enterprise edition
**Tõrkesiirde rühmitamise (AlwaysOn FCI)** | SQL serveri mõjutab Windows Tõrkesiirde rühmitamise jaoks kättesaadavuse asutusesisese SQL serveri töökoormus.<br/><br/>Töötab SQL Serveri eksemplari ühiskasutusega ketast sõlmed on konfigureeritud tõrkesiirdeklastrist. Kui näiteks ei tööta klaster ei üle teise.<br/><br/>Klaster ei kaitsta tõrge või katkestuste ühiskasutusega laos. Ühiskasutusega kettale saate rakendada iSCSI fiber kanali kohta, või VHDXs ühiskasutusse antud. | SQL Server Enterprise'i<br/><br/>SQL Server Standard edition (kuni kahe sõlme ainult)
**Andmebaasi peegeldus (kõrge turvalisuse režiim)** | Kaitseb ühtse andmebaasi ühe teisene koopia. Saadaval nii kõrge Turve (sünkroonse) ja suure jõudlusega (asünkroonne) kopeerimine režiimi. Tõrkesiirdeklastrite ei vaja. | SQL Server 2008 R2<br/><br/>SQL Server Enterprise kõik versioonid
**Autonoomse SQL Server** | SQL Server ja andmebaas on majutatud ühe server (füüsilise või virtuaalse). Kui server kuulub virtuaalse kasutatakse Host rühmitamise kõrge kättesaadavus. Pole Külastajate taseme kõrge kättesaadavus. | Enterprise või Standard edition

## <a name="deployment-recommendations"></a>Juurutamise soovitused


Selles tabelis on kokkuvõte meie soovitused BCDR SQL serveri tehnoloogiate integreerimise saidi taastamine.

**Versioon** |**Väljaande** | **Juurutamine** | **Kui soovite kohapeal kohapeal** | **Azure'i kohapeal** 
---|---|---|---|---
SQL serveri 2014 või 2012 | Ettevõtte | Eksemplari Tõrkesiirde kobar | Kättesaadavusrühmad | Kättesaadavusrühmad
 | Ettevõtte | Kättesaadavusrühmad jaoks kõrge-saadavus | Kättesaadavusrühmad | Kättesaadavusrühmad
 | Standard | Tõrkesiirde kobar eksemplari (FCI) | Saidi taastamise kopeerimisest kohaliku Peegelveerised | Saidi taastamise kopeerimisest kohaliku Peegelveerised
 | Enterprise või Standard | Autonoomse | Saidi taastamise kopeerimine | Saidi taastamise kopeerimine
SQL Server 2008 R2 | Enterprise või Standard | Tõrkesiirde kobar eksemplari (FCI) | Saidi taastamise kopeerimisest kohalik Peegelveerised | Saidi taastamise kopeerimisest kohaliku Peegelveerised
 | Enterprise või Standard | Autonoomse | Saidi taastamise kopeerimine | Saidi taastamise kopeerimine
SQL Server (kõik versioonid) | Enterprise või Standard | Tõrkesiirde kobar eksemplari - DTC rakendus | Saidi taastamise kopeerimine | Pole toetatud


## <a name="deployment-prerequisites"></a>Juurutamise eeltingimused

Siin on, mida peate enne käivitamine:

- Mõne asutusesisese SQL serveri juurutuse toetatud SQL serveri versioon. Tavaliselt peate ka Active Directory jaoks SQL serveris.
- Eeltingimused stsenaariumi, mida soovite kasutada. Eeltingimused leiate iga juurutamise artiklis. Lingid neile on esitatud [Ülevaade saidi taastamine](site-recovery-overview.md).
- Kui soovite häälestada Azure taastamise, peate käivitama SQL serveri virtuaalmasinates veendumaks, et need ühilduvad Azure ja saidi taastamine [Azure virtuaalse masina valmisoleku hindamise](http://www.microsoft.com/download/details.aspx?id=40898) tööriista.


## <a name="set-up-active-directory"></a>Active Directory häälestamine

Active Directory teisene taastamine saidi SQL serveri õigeks töötamiseks on vaja. on paar võimalust.

- **Väikeettevõte**– kui teil on väheste rakenduste ja ühe domeenikontrolleri kohapealse saidi ja soovite ei õnnestu üle terve saidi, soovitame kasutada saidi taastamine repication korrata domeenikontrolleri teisene andmekeskuse või Azure.

- **Keskmise suurettevõte**– kui teil on suur hulk rakenduse, kasutate on Active Directory kogumis ja soovite nurjuda rakendus või töökoormus, soovitame täiendavad domeenikontrolleri teisene andmekeskuses või Azure häälestamist. Märkus, et kui kasutate Kättesaadavusrühmad taastada on serveri saidi soovitame häälestamist teise täiendavad domeenikontrolleri saidi või Azure, kasutada taastatud SQL serveri eksemplar.

Selles dokumendis juhiseid eeldavad domeenikontrolleri on saadaval teisene asukohta. [Lisateavet](site-recovery-active-directory.md) Active Directory saidi taastamine kaitsmise kohta.

## <a name="integrate-protection-with-sql-server-always-on-on-premises-to-azure"></a>SQL serveri alati (kohapealse Azure) kaitse integreerimine


Saidi taastamine toetab algupäraselt SQL-i AlwaysOn. Kui teie loodud kättesaadavus rühma SQL Azure'i virtuaalarvuti häälestamine "Teisese", siis Tõrkesiirde kättesaadavus rühma haldamiseks saate kasutada saidi taastamine. 

>[AZURE.NOTE] See funktsioon on praegu eelvaade ja saadaval Hyper-V hosti serverites esmane andmekeskuse hallatakse VMM pilved ja kui VMware häälestamise haldab [Konfiguratsiooniserveri](site-recovery-vmware-to-azure.md#configuration-server-prerequisites). See funktsioon pole saadaval uus Azure'i portaalis praegu.

#### <a name="prerequisites"></a>Eeltingimused

Siin on vajalik integreerida SQL-i AlwaysOn saidi taastamine.

- Mõne asutusesisese SQL serveri (eraldi server või tõrkesiirdeklastrite).
- Ühe või mitme Azure'i virtuaalmasinates SQL serveri installitud
- SQL-i kättesaadavus rühma häälestamine vahel asutusesisese SQL Server ja Azure SQL serveris
- PowerShelli remoting peaks olema lubatud asutusesisese SQL serveri arvutisse. VMM server või konfiguratsiooniserveri peaks olema võimalik helistada remote PowerShelli SQL serveriga.
- Kasutajakonto lisatakse asutusesisese SQL Server, klõpsake nende SQL-i kasutajarühmade vähemalt järgmisi õigusi:
    - MUUTA kättesaadavaks rühma: õigused [siin](https://msdn.microsoft.com/library/hh231018.aspx)ja [siin](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - Laused ALTER andmebaasi - õigused[siin](https://msdn.microsoft.com/library/ff877956.aspx#Security)
- RunAs konto luuakse VMM serveris või luuakse konto konfiguratsiooniserveri, kasutades funktsiooni CSPSConfigtool.exe mainitud eelmises etapis kasutaja kohta 
- SQL-i PS mooduli peaks olema installitud ja SQL-i serverites töötab kohapealse ja Azure'i virtuaalmasinates
- VM Agent peaks olema installitud töötavate Azure'i virtuaalmasinates
- NTAUTHORITY\System peaks olema õigused pärast SQL Server Azure'i virtuaalmasinates töötab:
    - MUUTA kättesaadavaks rühma - õigused [siin](https://msdn.microsoft.com/library/hh231018.aspx)ja [siin](https://msdn.microsoft.com/library/ff878601.aspx#Anchor_3)
    - Laused ALTER andmebaasi - õigused [siin](https://msdn.microsoft.com/library/ff877956.aspx#Security)

####  <a name="step-1-add-a-sql-server"></a>Samm 1: Lisa SQL Server


1. Klõpsake nuppu **Lisa SQL-i** lisada uue SQL Server. 

    ![SQL-i lisamine](./media/site-recovery-sql/add-sql.png)

2. **SQL-i sätete konfigureerimine**rakenduses > **nimi** Sisestage sõbralik nimi, mis viitavad SQL serveri.
3. **SQL serveri (FQDN)** Määrake FQDN allika SQL serveri, mille soovite lisada. Juhul, kui SQL Server on installitud tõrkesiirdeklastrite, siis lisage FQDN klaster ja mitte mõnda kobar sõlmed.  
4. **SQL serveri eksemplar** vaikimisi eksemplari valida või sisestada kohandatud eksemplari nimi.
5. Valige VMM server **Management Server** või konfiguratsiooniserveri registrisse saidi taastamise hoidla. Saidi taastamise kasutab seda Management serveri SQL serveriga.
6. **Kui konto** sisestage RunAs konto, mis on loodud määratud VMM Server või konto, mis on loodud Configuraaaon serveri nimi. Selle konto kasutatakse SQL Serverit ja peaks olema lugemis- ja Tõrkesiirde õiguste kättesaadavus rühmad SQL serveri arvutisse.

    ![SQL-i dialoog lisa](./media/site-recovery-sql/add-sql-dialog.png)

Pärast SQL serveri lisamist kuvatakse see **SQL-i serverite** vahekaardil. 

![SQL serveri loend](./media/site-recovery-sql/sql-server-list.png)


#### <a name="step-2-add-a-sql-availability-group"></a>Samm 2: SQL-i kättesaadavus rühma lisamine

1. SQL serveri arvuti lisatakse järgmise sammuna tuleb lisada saidi taastamise kättesaadavus rühmad. Selleks, et sees SQL serveri lisatud eelmises juhises süvitsi ja klõpsake SQL-i kättesaadavus rühma lisada. 

    ![SQL-i AG lisamine](./media/site-recovery-sql/add-sqlag.png)

2. SQL-saadavus rühma saate imitatsiooniga ühe või mitme Azure'i virtuaalmasinates abil. Kui SQL-i kättesaadavus rühma nime ja tellimuse Azure virtuaalse masina kättesaadavus rühma olema nurjus üle saidi taastamise, kuhu on vaja lisamisel.

    ![SQL-i AG dialoog lisa](./media/site-recovery-sql/add-sqlag-dialog.png)

3. Ülaltoodud näites kättesaadavus rühma DEG1-AG muutuks esmane virtual arvutisse SQLAGVM2 Tõrkesiirde sees tellimuse DevTesting2 töötab. 

>[AZURE.NOTE] Ainult kättesaadavus rühmad, mis on esmane lisatud kirjeldatud sammu SQL Server on saadaval lisada saidi taastamine. Kui olete teinud soovitud kättesaadavus rühma esmane SQL Server või kui olete lisanud mitu kättesaadavus rühmad SQL serveris pärast seda, kui see on lisatud, värskendage see käsk Värskenda saadaval SQL serveris.

#### <a name="step-3-create-a-recovery-plan"></a>Samm 3: Loo taastamise kava

Järgmiseks on nii virtuaalmasinates ja rühmade kättesaadavus abil taastamise kava loomiseks. Valige sama VMM Server või konfiguratsiooniserveri, mida kasutasite samm 1 allikas ja Microsoft Azure'i sihtfail nimega.

![Looge taastamise kava](./media/site-recovery-sql/create-rp1.png)

![Looge taastamise kava](./media/site-recovery-sql/create-rp2.png)

SharePointi rakenduse koosneb näites 3 virtuaalmasinates kasutada SQL-i kättesaadavus rühma oma taustväärtus. See taastamise plaan me ei saanud valige nii kättesaadavus rühma ning virtuaalse masina mis moodustavad taotluse. 

Te saate kohandada taastamise kava, teisaldades virtuaalmasinates erinevate Tõrkesiirde rühmadesse järjestamine Tõrkesiirde järjestuse. Kättesaadavus rühma alati nurjus üle esimese, nagu seda kasutataks kirjutamata kõigist. 

![Taastamise kava kohandamine](./media/site-recovery-sql/customize-rp.png)

### <a name="step-4--fail-over"></a>Samm 4: Ei õnnestu üle

Erinevate Tõrkesiirde suvandid on saadaval, kui kättesaadavus rühma on lisatud taastamise kava.

Tõrkesiirde | Üksikasjad
--- | ---
**Kavandatud Tõrkesiirde** | Kavandatud Tõrkesiirde tähendab pole andmete kaotsimineku Tõrkesiirde. Et SQL-saadavus rühma kättesaadavus režiim on seatud esmalt sünkroonne ja seejärel Tõrkesiirde käivitatakse teha kättesaadavaks rühma esmane kellelegi virtuaalse masina samal ajal kättesaadavus rühma lisamine saidi taastamine saavutamiseks. Pärast selle Tõrkesiirde on lõpule jõudnud, seatakse kättesaadavus režiimis nagu see oli enne kavandatud Tõrkesiirde käivitati sama väärtuse.
**Planeerimata Tõrkesiirde** | Planeerimata Tõrkesiirde võib põhjustada andmekao sisse. Planeerimata Tõrkesiirde käivitamise ajal kättesaadavus režiimi kättesaadavus rühma on muutunud ja tehakse esmane kellelegi virtuaalse masina samal ajal kättesaadavus rühma lisamine saidi taastamine. Kui planeerimata Tõrkesiirde on lõpule viidud ja kohapealse server töötab SQL serveri on saadaval uuesti, on kättesaadavus rühma, käivitub kopeerimise tühistada. Pange tähele, et see toiming pole saadaval taastamise kava saavad võtta SQL-saadavus rühma SQL-i serverid vahekaardil
**Test Tõrkesiirde** | Test Tõrkesiirde SQL-saadavus rühma ei toetata. Kui saate käivitada Test Tõrkesiirde taastamise kava, mis sisaldab SQL-saadavus rühma, tuleks Tõrkesiirde vahele kättesaadavus rühma.


Kaaluge Tõrkesiirde suvanditest.

Suvand | Üksikasjad
--- | ---
**Suvand 1** | 1. teha testi Tõrkesiirde rakendus ja ees astme.<br/><br/>2. värskendage rakenduste taseme juurdepääsu koopia Kopeeri kirjutuskaitstud režiimis ja rakenduse kirjutuskaitstud testida.
**Variant 2** | 1. koopia koopia SQL Serveri eksemplari virtuaalse masina (kasutades VMM klooni-saidilt või Azure varukoopia) loomine ja avab võrgu testimine<br/><br/> 2. selle testi Tõrkesiirde taastamise kava abil teha.

Juhis 5: Nurjuda tagasi

Kui soovite teha kättesaadavaks rühma uuesti esmane asutusesisese SQL serveris siis saate seda teha käivitamise kavandatud Tõrkesiirde taastamine kavandamine ja käsu kohapealse VMM Server Microsoft Azure'i suunas.

>[AZURE.NOTE] Pärast planeerimata Tõrkesiirde on pööratud dispersioonanalüüs kättesaadavus rühma on kopeerimise jätkamiseks käivitub. Kuni seda tehakse dispersioonanalüüs jääb peatatud.



### <a name="protect-machines-without-a-vmm-server-or-a-configuration-server"></a>Kaitsmine ilma VMM Server või serveri konfiguratsioon

Keskkonnas on haldab VMM Server või serveri konfiguratsioon, saab konfigureerida skriptitud Tõrkesiirde SQL-saadavus rühmade Azure'i automaatika tegevusraamatud. Allpool on mõned toimingud, mis konfigureerimiseks:

1.  Looge kohalik skripti ei õnnestu üle kättesaadavus rühma või fail. Selle valimi skripti määrab tee kättesaadavaks rühma Azure koopia ja jätab üle selle koopia eksemplari. See skript käivitatakse koopia virtuaalse masina sooritab on kohandatud skript laiendiga SQL serveris.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2.  Laadige skripti lisamine bloobimälu Azure storage konto. Selles näites kasutamine

        $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key"
        Set-AzureStorageBlobContent -Blob "AGFailover.ps1" -Container "script-container" -File "ScriptLocalFilePath" -context $context

3.  Saate luua ka Azure automatiseerimine käitusjuhendi kasutada skriptide Azure SQL serveri koopia virtual arvutisse. See näide skripti abil tehke järgmist. [Lisateavet leiate teemast](site-recovery-runbook-automation.md) kasutamise automaatika tegevusraamatud taastamise kohta. 

        workflow SQLAvailabilityGroupFailover
        {
            param (
                [Object]$RecoveryPlanContext
            )

            $Cred = Get-AutomationPSCredential -name 'AzureCredential'
    
            #Connect to Azure
            $AzureAccount = Add-AzureAccount -Credential $Cred
            $AzureSubscriptionName = Get-AutomationVariable –Name ‘AzureSubscriptionName’
            Select-AzureSubscription -SubscriptionName $AzureSubscriptionName
    
            InLineScript
            {
            #Update the script with name of your storage account, key and blob name
            $context = New-AzureStorageContext -StorageAccountName "Account" -StorageAccountKey "Key";
            $sasuri = New-AzureStorageBlobSASToken -Container "script-container"- Blob "AGFailover.ps1" -Permission r -FullUri -Context $context;
     
            Write-output "failovertype " + $Using:RecoveryPlanContext.FailoverType;
               
            if ($Using:RecoveryPlanContext.FailoverType -eq "Test")
                {
                #Skipping TFO in this version.
                #We will update the script in a follow-up post with TFO support
                Write-output "tfo: Skipping SQL Failover";
                }
            else
                {
                Write-output "pfo/ufo";
                #Get the SQL Azure Replica VM.
                #Update the script to use the name of your VM and Cloud Service
                $VM = Get-AzureVM -Name "SQLAzureVM" -ServiceName "SQLAzureReplica";     
       
                Write-Output "Installing custom script extension"
                #Install the Custom Script Extension on teh SQL Replica VM
                Set-AzureVMExtension -ExtensionName CustomScriptExtension -VM $VM -Publisher Microsoft.Compute -Version 1.3| Update-AzureVM; 
                    
                Write-output "Starting AG Failover";
                #Execute the SQL Failover script
                #Pass the SQL AG path as the argument.
       
                $AGArgs="-SQLAvailabilityGroupPath sqlserver:\sql\sqlazureVM\default\availabilitygroups\testag";
       
                Set-AzureVMCustomScriptExtension -VM $VM -FileUri $sasuri -Run "AGFailover.ps1" -Argument $AGArgs | Update-AzureVM;
       
                Write-output "Completed AG Failover";

                }
        
            }
        }

4.  Kui loote taastamise kava rakenduse "eelnevalt rühm 1 buutimine" skriptitud juhise, mis käivitab automatiseerimise käitusjuhendi nurjumise kättesaadavus rühmad kohal lisamiseks.

## <a name="integrate-protection-with-sql-alwayson-on-premises-to-on-premises"></a>SQL-i AlwaysOn (kohapealse kohapealse) kaitse integreerimine

Kui SQL Server kasutab kättesaadavus rühmad kõrge-saadavus või Tõrkesiirde kobar eksemplari, soovitame selle taastamise saidil kättesaadavus rühmade kasutamine. Pange tähele, et juhised on mõeldud rakendusi, mis ei kasuta jaotatud tehingud.

1. [Konfigureerimine andmebaaside](https://msdn.microsoft.com/library/hh213078.aspx) kättesaadavus rühmadesse.
2. Saidi virtuaalse uue võrgu loomine
3. Saate häälestada-saidilt VPN uue virtuaalse võrgu ja esmane saidi vahel.
4. Luua virtuaalse masina saidil taastamine ja SQL serveri installimine.
5. Uue SQL serveri virtual arvutisse olemasoleva Kättesaadavusrühmad laiendamine. SQL serveri eksemplar konfigureerimine on asünkroonne koopia koopiana.
6. Mõne rühma kättesaadavus kuulajale loomine või olemasoleva kuulajale kaasata asünkroonne koopia virtuaalse masina värskendada.
7. Veenduge, et rakendus serveripargi on kuulajale abil häälestus. Kui see on häälestatud abil andmebaasiserveri nimi, värskendage seda kasutama kuulajale, seetõttu ei pea te seda pärast selle Tõrkesiirde konfigureerimiseks.

Jaotatud tehingud kasutavate rakenduste meil soovitus kasutada [Saidi taastamine SAN kopeerimise abil](site-recovery-vmm-san.md) või [VMWare/füüsilise serveri saidilt dispersioonanalüüs](site-recovery-vmware-to-vmware.md).

### <a name="recovery-plan-considerations"></a>Peaksite arvesse võtma taastamine kavandamine

1. Selle valimi skripti lisamiseks VMM teeki esmaseid ja teiseseid saitidel.

        Param(
        [string]$SQLAvailabilityGroupPath
        )
        import-module sqlps
        Switch-SqlAvailabilityGroup -Path $SQLAvailabilityGroupPath -AllowDataLoss -force

2. Kui loote taastamise kava rakenduse "eelnevalt rühm 1 buutimine" skriptitud juhise, mis käivitab skripti ei õnnestu üle kättesaadavus rühmad lisamiseks.



## <a name="protect-a-standalone-sql-server"></a>SQL serveri autonoomse kaitsmine

Selle konfiguratsiooni soovitame saidi taastamine kopeerimise abil kaitsta SQL serveri arvutisse. Täpsed juhised sõltub, kas virtuaalse masina või füüsiline server, SQL Server on häälestatud ja kas soovite paljundada Azure või teisese kohapealse saidi. Kõigi juurutamise stsenaariumide juhiste saamiseks [Saidi taastamise ülevaade](site-recovery-overview.md).


## <a name="protect-a-sql-server-cluster-standard-or-2008-r2"></a>Kaitse SQL serveri kobar (Standard- või 2008 R2)

SQL Server 2008 R2 või SQL Server Standard versioon töötab klaster soovitame dispersioonanalüüs saidi taastamise abil kaitsta SQL serveri.

### <a name="on-premises-to-on-premises"></a>Kohapealse kohapealse

- Kui soovitud rakendus kasutab jaotatud tehingud soovitame juurutamist [Saidi taastamine SAN kopeerimise abil](site-recovery-vmm-san.md) Hyper-V keskkonna ja [VMware/füüsilise serveri VMware](site-recovery-vmware-to-vmware.md) VMware keskkonnas.

- DTC rakenduste, kasutada ülaltoodud lähenemine taastada klaster eraldiseisev serveri kasutamine kohaliku kõrge turvalisuse DB võrreldes peegelpilt.

### <a name="on-premises-to-azure"></a>Kohapealse Azure

Saidi taastamine ei toeta Külastajate kobar tugi, kui imitatsiooniga Azure'i. SQL serveri ka ei paku madala hinnaga katastroofi taastamise lahendus Standard Edition. Soovitame teil kaitse asutusesisese SQL serveri klaster SQL serveri eraldi et ja Azure seda taastada.


1. Kohapealse saidi konfigureerida täiendavaid autonoomse SQL serveri eksemplar.
2. Konfigureerige eksemplar olla Peegelveerised, andmebaaside kohta, kus on vaja kaitsta. Saate konfigureerida, peegelduse kõrge ohutus režiimis.
3.  Saidi taastamine konfigureerida kohapealse saidi põhjal keskkonnas ([Hyper-V](site-recovery-hyper-v-site-to-azure.md) või [VMware/füüsilise serveri](site-recovery-vmware-to-azure-classic.md).
4.  Ise uus SQL serveri eksemplar Azure saidi taastamine kopeerimise abil. Koopia kõrge turvalisuse Peegelveerised on ja et see kuvatakse sünkroonitud esmane kobar, kuid see kopeeritakse Azure saidi taastamine kopeerimise abil.

Järgmisel joonisel on näidatud selle setup.

![Standardse kobar](./media/site-recovery-sql/BCDRStandaloneClusterLocal.png)


### <a name="failback-considerations"></a>Failback kaalutlused

Jaoks SQL standard kogumite, failback pärast planeerimata Tõrkesiirde nõua SQL-i varukoopia ja algse kobar ja taastada selle Peegelveerised Peegelveerised eksemplarist taastamiseks.

## <a name="next-steps"></a>Järgmised sammud
[Lisateavet](site-recovery-best-practices.md) selle kohta, kuidas valmis kasutama saidi taastamine.










 