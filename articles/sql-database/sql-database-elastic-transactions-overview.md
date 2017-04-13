<properties
   pageTitle="Jaotatud tehingud üle pilve andmebaasid"
   description="Ülevaade elastne andmebaasi tehingud Azure'i SQL-andmebaas"
   services="sql-database"
   documentationCenter=""
   authors="torsteng"
   manager="jhubbard"
   editor="torsteng"/>

<tags
   ms.service="sql-database"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="sql-database"
   ms.date="05/27/2016"
   ms.author="torsteng"/>

# <a name="distributed-transactions-across-cloud-databases"></a>Jaotatud tehingud üle pilve andmebaasid

Elastne andmebaasitoiminguid Azure SQL-i andmebaasi (SQL-i DB) võimaldavad käivitage SQL dB andmebaasidest ulatuvad tehingud. SQL-i dB elastne andmebaasitoiminguid ADO .net-i kasutades .net-i rakendustele on saadaval ning integreerida tuttavad programmeerimise kogemus, kasutades [System.Transaction](https://msdn.microsoft.com/library/system.transactions.aspx) tunnid. Teegi kohta leiate teavet [.NET Frameworki 4.6.1 (Web Installer)](https://www.microsoft.com/download/details.aspx?id=49981).

Kohapealne, nõutav sellise stsenaariumi tavaliselt töötab Microsoft jaotatud Transaction Coordinator (DTC). Kuna MSDTC ei ole saadaval platvormi-kui-a-Teenuserakenduse Azure, võimalus koordineerimiseks jaotatud tehingud on nüüd otse integreeritud SQL DB. Rakendusi saate luua ühenduse mis tahes SQL-andmebaasi jaotatud tehingud käivitamiseks ja ühte andmebaaside läbipaistev koordineerimine jagatud toimingu, nagu on näidatud järgmisel joonisel. 

  ![Jaotatud tehingud Azure'i SQL-andmebaasi elastne andmebaasitoiminguid abil ][1]

## <a name="common-scenarios"></a>Levinumad stsenaariumid

SQL-i dB elastne andmebaasitoiminguid lubada rakenduste atomic muudatuste tegemiseks mitmeid eri SQL andmebaase talletatavad andmed. Eelvaate keskendub kliendipoolne arengu kogemusi ja C# .net-i. Serveripoolne kogemusi, T-SQL-i abil on planeeritud hiljem.  
Elastne andmebaasitoiminguid eesmärgid järgmistel juhtudel:

* Mitme andmebaasirakendusi Azure: koos selle stsenaariumi korral andmed on vertikaalselt liigendatud üle mitme andmebaasidega SQL DB nii, et erinevat tüüpi andmete asuvad eri andmebaasid. Teatud toimingute jaoks on vaja andmeid, mida talletatakse kaks või enam andmebaasi muudatustest. Rakendus kasutab elastne andmebaasitoiminguid koordineerimine muudatused üle andmebaasid ja veenduge, et atomicity.

* Azure sharded andmebaasirakendusi: koos selle stsenaariumi korral andmete taseme kasutab [elastne andmebaasi kliendi teek](sql-database-elastic-database-client-library.md) või enda loodud-sharding horisontaalselt partition üle mitme andmebaasidega SQL DB andmeid. Ühe märgatavaks kasutamine puhul on vaja teha atomic muudatused sharded mitme rentniku rakenduse kui muudatused üle rentnikke. Mõelge näiteks liikumist ühest rentnikust teise nii elavate erinevate andmebaasid. Teisel on kohandatud sharding muutmine võimsus vajadustega suurte rentniku, mis omakorda tavaliselt tähendab, et mõned atomic toiminguid peab ulatuvad üle andmebaasidest kasutatakse samal rentnikukontol. Kolmas näide on aatomi värskenduste viitamiseks on andmebaaside kogu kopeeritud andmeid. Atomic, tehingu, toimingute suunas saate koordineeritud nüüd üle andmebaasidest eelvaate kasutamisel.
Elastne andmebaasitoiminguid abil tagada tehingu atomicity andmebaaside kahefaasiline kinnitamine. See on hea sobib vähem kui 100 andmebaase, ühe toimingu ajal tehingute puhul. Need piirangud, pole jõustatud, kuid üks tuleks oodata jõudlus ja edukust elastne andmebaasitoiminguid all, kui need ületavad jaoks.


## <a name="installation-and-migration"></a>Installimine ja migreerimine

Võimaluste elastne andmebaasitoiminguid SQL dB jaoks on olemas värskenduste .NET teekide System.Data.dll ja System.Transactions.dll kaudu. DLL tagada selle kahefaasiline Kinnita kasutatakse atomicity tagamiseks. Elastne andmebaasitoiminguid rakenduste arendamise alustamiseks installida [.NET Frameworki 4.6.1](https://www.microsoft.com/download/details.aspx?id=49981) või uuem versioon. Varasema versiooni .NET Frameworki käivitamisel tehingud ei õnnestu edendada jagatud toimingu ja tõstetakse erandi.

Pärast installimist saate jagatud toimingu API System.Transactions SQL DB ühendused. Kui teil on olemasoleva MSDTC rakendustes, mis kasutavad järgmisi API-d, lihtsalt pärast installimist soovitud 4.6.1 taasloomine olemasolevaid rakendusi 4.6 .NET Framework. Kui projektide suunata .NET 4.6, kasutab ta automaatselt värskendatud dll-teekidega raamistiku uue versiooni ja nüüd õnnestub jagatud toimingu API kõned koos SQL DB ühendused.

Pidage meeles, et elastne andmebaasitoiminguid ei nõua MSDTC installimisel. Selle asemel otse hallatakse elastne andmebaasitoiminguid ja SQL-i DB piires. See oluliselt lihtsustab cloud stsenaariumid kasutuselevõtu MSDTC ei ole vaja kasutada jaotatud tehinguid SQL DB. Jaotis 4 selgitatakse täpsemalt elastne andmebaasitoiminguid ja nõutav .NET Frameworki koos cloud rakenduste Azure juurutamine.

## <a name="development-experience"></a>Arendamise kogemus

### <a name="multi-database-applications"></a>Mitme andmebaasirakendusi

Järgmine kood kasutab .NET System.Transactions tuttavad programmeerimise kogemus. TransactionScope klassi luuakse ümbritseva toimingu .NET-is. ("Ümbritseva tehing" on üks, mis asub praeguse teemas.) Kõik ühendused avatud TransactionScope osaleda tehingu. Kui andmebaaside osaleda, tõstetud tehingu automaatselt jagatud toimingu. Tehingu tulemust kontrollib lõpuleviimiseks tähistamiseks on Kinnita ulatuse häälestamise.

    using (var scope = new TransactionScope())
    {
        using (var conn1 = new SqlConnection(connStrDb1))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }

        using (var conn2 = new SqlConnection(connStrDb2))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T2 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }

### <a name="sharded-database-applications"></a>Sharded andmebaasirakendusi
 
SQL-i dB elastne andmebaasitoiminguid toetavad ka koordineerimine jaotatud tehingud, kui kasutate OpenConnectionForKey meetodit elastne andmebaasi kliendi teegi avamiseks ühendused mastaabitud välja andmete taseme. Kaaluge võimalust juhul, kui peate selleks, et tagada selgituseks järjepidevus muudatused üle mitme eri sharding väärtused. Ühenduste shards majutusteenuse erinevate sharding väärtus on vahendajaks OpenConnectionForKey abil. Üldine juhul saab ühendused erinevad shards nii, et tagada selgituseks garantiid nõuab jagatud toimingu. Järgmine kood näidis illustreerib seda moodust. Eeldatakse, et nimega shardmap muutuja teegist elastne andmebaasi kliendi kaardil Kildu väljendamiseks kasutatakse:

    using (var scope = new TransactionScope())
    {
        using (var conn1 = shardmap.OpenConnectionForKey(tenantId1, credentialsStr))
        {
            conn1.Open();
            SqlCommand cmd1 = conn1.CreateCommand();
            cmd1.CommandText = string.Format("insert into T1 values(1)");
            cmd1.ExecuteNonQuery();
        }
        
        using (var conn2 = shardmap.OpenConnectionForKey(tenantId2, credentialsStr))
        {
            conn2.Open();
            var cmd2 = conn2.CreateCommand();
            cmd2.CommandText = string.Format("insert into T1 values(2)");
            cmd2.ExecuteNonQuery();
        }

        scope.Complete();
    }


## <a name="net-installation-for-azure-cloud-services"></a>.Net-i installi jaoks Azure pilveteenused

Azure'i pakub mitmeid pakkumisi host .net-i rakendustele. Erinevate pakkumiste võrdlus on esitatud [Azure'i rakendust Service, pilveteenustega, ja Virtuaalmasinates võrdlus](../app-service-web/choose-web-site-cloud-service-vm.md). Kui külaline OS pakkumine on väiksem kui .NET 4.6.1 elastne tehingute puhul nõutav, peate 4.6.1 Külastajate OS versioonile. 

Azure'i rakenduse teenuste, täiendusi Külastajate OS pole praegu toetatud. Jaoks Azure'i Virtuaalmasinates, VM lihtsalt sisse logida ja käivitage installer .NET Frameworki uusima. Azure'i pilveteenustega, peate .NET uuema versiooni installimise käivitus ülesannete juurutamise kaasata. Mõisted ja juhised on avaldatud [Installida .net-i teenus Cloud roll](../cloud-services/cloud-services-dotnet-install-dotnet.md).  

Pange tähele, et .net-i 4.6.1 Installeri nõuda salvestusruumi ajutine Azure cloud Services kui installiprogrammi .NET 4.6 bootstrapping käigus. Installi õnnestumise tagamiseks peate suurendage ajutiste salvestusruum oma Azure pilveteenuses ServiceDefinition.csdef failis LocalResources lõik ja keskkonna sätteid käivitus tööülesande, nagu on näidatud järgmises näites:

    <LocalResources>
    ...
        <LocalStorage name="TEMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
        <LocalStorage name="TMP" sizeInMB="5000" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
        <Task commandLine="install.cmd" executionContext="elevated" taskType="simple">
            <Environment>
        ...
                <Variable name="TEMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TEMP']/@path" />
                </Variable>
                <Variable name="TMP">
                    <RoleInstanceValue xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='TMP']/@path" />
                </Variable>
            </Environment>
        </Task>
    </Startup>
    
## <a name="transactions-across-multiple-servers"></a>Tehingud üle mitme serveriga

Elastne andmebaasitoiminguid on toetatud Azure'i SQL-andmebaasi erinevate loogilise serverites. Kui tehingud cross loogiline serveri piirmäärad, osalevate serverid Kõigepealt tuleb sisestada vastastikune side seose. Suhtlus seose koostatud iga andmebaasi kaks serverid saavad osaleda elastne tehingud andmebaasidega muude serverist. Tehingud kestvad rohkem kui kaks loogilised serverid, kus side seose peab olema iga paari loogilised serverid alles.

Rist-server side jaoks elastne andmebaasitoiminguid seoste haldamine järgmist PowerShelli cmdlet-käskude abil:

* **Uus-AzureRmSqlServerCommunicationLink**: cmdlet-käsu abil saate luua uue teatise seose kahe loogilise dB Azure SQL serveri vahel. Seose on sümmeetriline, mis tähendab, et nii saate algatada tehingud muude serveriga.
* **Get-AzureRmSqlServerCommunicationLink**: cmdlet-käsu abil saate tuua side olemasolevad seosed ja nende atribuudid.
* **Eemalda – AzureRmSqlServerCommunicationLink**: cmdlet-käsu abil saate eemaldada olemasoleva side seose. 


## <a name="monitoring-transaction-status"></a>Tehingu oleku jälgimine

Dünaamiliste haldusvaated () kasutamine SQL DB kuvari oleku ja edenemise poolelioleva elastne andmebaasitoiminguid. Kõik DMVs tehingute on olulised SQL dB jaotatud tehingute puhul. Leiate siit DMVs vastav loend: [tehingu seotud dünaamilise halduse vaadete ja funktsioonide (Transact-SQL-i)](https://msdn.microsoft.com/library/ms178621.aspx).
 
Nende DMVs on eriti kasulik.

* **sys.dm\_tran\_aktiivne\_tehingud**: loetletakse praegu aktiivne tehinguid ja nende oleku. Veeru UOW (tööüksust) saate tuvastada eri lapse tehingud, mis kuuluvad sama jagatud toimingu. Kõigi tehingute sama jagatud toimingu teha sama UOW väärtuse. Lugege üksikasjalikumat teavet [DMV dokumentatsiooni](https://msdn.microsoft.com/library/ms174302.aspx) .
* **sys.dm\_tran\_andmebaasi\_tehingud**: annab Lisateavet tehingud, nt paigutuse tehingu Logi. Lugege üksikasjalikumat teavet [DMV dokumentatsiooni](https://msdn.microsoft.com/library/ms186957.aspx) .
* **sys.dm\_tran\_lukud**: lukud, et omanik on parajasti käimas tehinguid teave. Lugege üksikasjalikumat teavet [DMV dokumentatsiooni](https://msdn.microsoft.com/library/ms190345.aspx) .

## <a name="limitations"></a>Piirangud 

Järgmistele piirangutele rakendada praegu elastne andmebaasitoiminguid SQL DB:

* Toetatakse ainult tehingud üle SQL DB andmebaasidega. Muude [X / avatud XA](https://en.wikipedia.org/wiki/X/Open_XA) ressursi pakkujad ja andmebaaside SQL DB väljaspool ei saa osaleda elastne andmebaasitoiminguid. See tähendab, et elastne andmebaasitoiminguid ei saa venitada üle kohapealne SQL Server ja Azure SQL-i andmebaasid. Kohapealne jaotatud tehingute puhul edaspidi MSDTC. 
* Toetatakse ainult kliendi koordineeritud tehingud .net-i rakenduse kaudu. T-SQL-i näiteks alustada tehingu LEVITADA serveripoolne tugi on kavandatud, kuid pole veel saadaval. 
* Toetatakse ainult Azure SQL-i DB V12 andmebaase.
* WCF-i teenustes tehingud ei toetata. Näiteks kui teil on WCF-i teenuse meetod, mis käivitab tehingu. Lisades kõne jooksul tehingu ulatus on [System.ServiceModel.ProtocolException](https://msdn.microsoft.com/library/system.servicemodel.protocolexception)nimega nurjub.

## <a name="additional-resources"></a>Lisaressursid

Veel abil elastne andmebaasi võimaluste Azure rakendusi? Vaadake meie [Dokumentatsiooni kaarti](https://azure.microsoft.com/documentation/learning-paths/sql-database-elastic-scale/). Küsimused, palun jõuda meile [SQL-andmebaasi Foorum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) ja soovitada funktsioone, lisage need [SQL-andmebaasi tagasiside Foorum](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-transactions-overview/distributed-transactions.png



