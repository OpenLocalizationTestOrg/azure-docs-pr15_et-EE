<properties 
    pageTitle="Andmete - Andmehalduslüüsi | Microsoft Azure'i"
    description="Häälestada andmete lüüsi andmete kohapealse ja pilveteenuse vahel liikumiseks. Azure'i andmed Factory Andmehalduslüüsi abil andmete teisaldamine." 
    keywords="andmete lüüsi, andmete integreerimine, andmete, lüüsi mandaati teisaldamine"
    services="data-factory" 
    documentationCenter="" 
    authors="linda33wj" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="jingwang"/>

# <a name="move-data-between-on-premises-sources-and-the-cloud-with-data-management-gateway"></a>Andmete teisaldamine asutusesisestest allikatest ja pilveteenuse vahel koos Andmehalduslüüs
Selles artiklis antakse ülevaade andmete integreerida kohapealse andmete ja pilveteenuse andmete salvestab andmed Factory abil. Selles artiklis [Andmete liikumine tegevuste](data-factory-data-movement-activities.md) ja muude andmete factory core põhimõtet artiklid on: [andmekomplektide](data-factory-create-datasets.md) ja [torujuhtmete](data-factory-create-pipelines.md). 

## <a name="data-management-gateway"></a>Andmehalduslüüs
Teie kohapealse arvutisse lubamiseks jooksva andmeid hoida kohapealse Date ja sealt tuleb teil installida Andmehalduslüüsi. Lüüsi saab installida samasse arvutisse kui andmesalve või mõnda teise arvutisse, kui andmesalve saate luua ühenduse lüüs. 

> [AZURE.IMPORTANT] Vt [Andmehalduslüüsi](data-factory-data-management-gateway.md) artikkel Andmehalduslüüsi üksikasjad.   

Järgmised kiirtutvustus näitab, kuidas luua andmete factory kohaletoimetamisel, mis teisaldab andmed Azure'i bloobimälu asutusesisese **SQL serveri** andmebaasi. Funktsiooni kiirtutvustus osana installimist ja konfigureerimist Andmehalduslüüsi teie arvutis. 

## <a name="walkthrough-copy-on-premises-data-to-cloud"></a>Juhend: kohapealse andmete Cloud kopeerimine
  
## <a name="create-data-factory"></a>Andmete factory loomine
Selles etapis tuleb kasutada Azure portaali Azure'i andmed Factory eksemplari nimega **ADFTutorialOnPremDF**loomine. 

1.  [Azure'i portaali](https://portal.azure.com)sisse logida. 
2.  Valige **+ Uus**, klõpsake **ärianalüüsi + Kasutusanalüüsi**ja klõpsake **Andmete Factory**.

    ![Uus -> DataFactory](./media/data-factory-move-data-between-onprem-and-cloud/NewDataFactoryMenu.png)  
2. **Uute andmete factory** tera, sisestage **ADFTutorialOnPremDF** nimi.

    ![Startboard lisamine](./media/data-factory-move-data-between-onprem-and-cloud/OnPremNewDataFactoryAddToStartboard.png)

    > [AZURE.IMPORTANT] 
    > Azure'i andmed factory nimi peab olema globaalselt kordumatu. Kui kuvatakse tõrketeade: **andmete factory nimi "ADFTutorialOnPremDF" on pole saadaval**, muuta andmete factory (nt yournameADFTutorialOnPremDF) nime ja proovige uuesti luua. Kasutage selle asemel ADFTutorialOnPremDF nimi täites ülejäänud juhised selles õpetuses.
    > 
    > Andmete factory nimi võib registreerida **DNS-i** nimi tulevikus ja seega muutuvad avalikult nähtav.
3. Valige **Azure'i tellimus** , kuhu soovite andmed factory luua. 
4.  Valige olemasolev **ressursirühm** või looge ressursirühma. Juhend on luua nimega ressursirühma: **ADFTutorialResourceGroup**. 
5.  Klõpsake nuppu **Loo** **Uus andmete factory** enne.

    > [AZURE.IMPORTANT] Andmete Factory eksemplari loomiseks peate olema [Andmete Factory kaasautori](../active-directory/role-based-access-built-in-roles.md/#data-factory-contributor) roll tasemel tellimuse/ressursi rühma liige. 
11. Pärast loomist on lõpule jõudnud, kuvatakse **Andmete Factory** tera, nagu on näidatud järgmisel pildil.

    ![Andmete Factory Avaleht](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDataFactoryHomePage.png)

## <a name="create-gateway"></a>Lüüsi loomine
5. Klõpsake **Andmete Factory** tera, **autor ja juurutamine** paani andmete factory **Editor** käivitamiseks.

    ![Koostamine ja juurutamine paan](./media/data-factory-move-data-between-onprem-and-cloud/author-deploy-tile.png) 
6.  Andmete Factory Editoris, klõpsake **... Lisateavet** tööriistaribal ja seejärel klõpsake käsku **Uus andmete lüüsi**. Teise võimalusena võite paremklõpsata **Andmete lüüside** puuvaates, ja klõpsake nuppu **Uus andmete lüüsi**. 

    ![Uute andmete lüüsi tööriistaribal](./media/data-factory-move-data-between-onprem-and-cloud/NewDataGateway.png)
2. **Loo** tera, sisestage **adftutorialgateway** **nimi**ja klõpsake nuppu **OK**.    

    ![Lüüsi blade loomine](./media/data-factory-move-data-between-onprem-and-cloud/OnPremCreateGatewayBlade.png)
3. Klõpsake **konfigureerimine** labale **otse arvutisse installida**. See toiming laaditakse eelmääratletud lüüsi, installib, konfigureerib ja registreerib arvutis lüüsi.  

    > [AZURE.NOTE] 
    > Kasutage Internet Explorer või Microsoft ClickOnce'i ühilduvad veebibrauseris.
    > 
    > Kui kasutate Chrome'i, lugege [Chrome'i](https://chrome.google.com/webstore/), otsida märksõna "ClickOnce'i" järgi, valige üks ClickOnce'i laiendid ja installige see. 
    >  
    > Tehke sama Firefox (installi lisandmoodul). Nuppu **Ava menüü** (paremas ülanurgas**kolme horisontaalsed jooned** ) tööriistaribal, klõpsake nuppu **lisandmoodulid**, otsida märksõna "ClickOnce'i" järgi, valige üks ClickOnce'i laiendid ja installige see.    

    ![Lüüsi - blade konfigureerimine](./media/data-factory-move-data-between-onprem-and-cloud/OnPremGatewayConfigureBlade.png)

    See viis on allalaadimine, installimine, konfigureerimine ja lüüsi registreerida ühe üksiktoiming on kõige lihtsam viis (üks klõps). Saate vaadata rakenduse **Microsoft andmete Andmehalduslüüsi Konfigureerimishalduri** on teie arvutisse installitud. Leiate ka käivitatava **ConfigManager.exe** kaustas: **C:\Program Files\Microsoft andmete haldamise Gateway\2.0\Shared**.

    Te saate ka allalaadimine ja installimine lüüsi käsitsi selle tera linkide abil ja registreerimine võti näidatud tekstiväljal **Uue TOOTENUMBRI** abil.
    
    Vt [Andmehalduslüüsi](data-factory-data-management-gateway.md) artikkel lüüsi üksikasjad.

    >[AZURE.NOTE] Peate olema administraator installima ja konfigureerima Andmehalduslüüsi edukalt kohalikus arvutis. Saate lisada täiendavate kasutajate Windows **Andmete Management Gateway kasutajate** rühma. Selle rühma liikmed saavad andmeid Andmehalduslüüsi Konfigureerimishalduri tööriista abil lüüsi konfigureerimine. 

5. Oodake paar minutit ja oodake, kuni kuvatakse järgmine teade:

    ![Lüüsi install õnnestus](./media/data-factory-move-data-between-onprem-and-cloud/gateway-install-success.png) 
6. Teie arvutis **Andmete Andmehalduslüüsi Konfigureerimishalduri** rakenduse käivitamine. Tippige aknas **Otsing** **Andmehalduslüüsi** selle kasuliku juurdepääsu. Leiate ka käivitatava **ConfigManager.exe** kaustas: **C:\Program Files\Microsoft andmete haldamise Gateway\2.0\Shared** 

    ![Andmehalduslüüsi Konfigureerimishalduri](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDMGConfigurationManager.png)
6. Veenduge, et näete `adftutorialgateway is connected to the cloud service` sõnum. Allosas olekuribal kuvatakse **ühendatud selle pilveteenusesse** koos **roheline märge**.

    Klõpsake menüü **Avaleht** võite teha ka järgmisi toiminguid: 
    - **Registreerida** lüüsi võti Azure'i portaalis Register nupu abil. 
    - **Peata** andmete haldamise lüüsi hostiteenus lüüsi teie arvutis töötab. 
    - **Ajakava värskenduste** installimiseks olema kindlal ajal päeva. 
    - Kui lüüsi oli **Viimati värskendatud**vaade.
    - Määrake aeg, kus saab installida lüüsi värskendus. 

8. Menüü **sätted** aktiveerimine Jaotises **serdi** määratud serdi kasutatakse krüptimine/dekrüptimine kohapealse andmesalve portaalis teie määratud mandaat. (valikuline) Klõpsake nuppu **Muuda** enda asemel kasutada. Vaikimisi kasutab lüüsi sert, mida on automaatselt genereeritud andmed Factory teenus.

    ![Lüüsi serdi konfigureerimine](./media/data-factory-move-data-between-onprem-and-cloud/gateway-certificate.png)

    Samuti saate teha järgmisi toiminguid vahekaardil **sätted** . 
    - Saate vaadata või eksportida, mida kasutavad lüüsi sert.
    - Saate muuta kasutatavat lüüsi HTTPS-i lõpp-punkti.    
    - Määrake HTTP-puhverserver lüüsi kasutavad.   
9. (valikuline) Vahekaardi **diagnostika** , märkige ruut **Luba Paljusõnaline logimine** , kui soovite lubada Paljusõnaline logimine, mille abil saate mis tahes lüüsi probleemide tõrkeotsingut. Logiteave leiate jaotises **rakenduste ja teenuste logid** **Sündmusevaatur**  -> **Andmehalduslüüsi** sõlm. 

    ![Diagnostika menüü](./media/data-factory-move-data-between-onprem-and-cloud/diagnostics-tab.png)

    **Diagnostika** menüüs saate teha ka järgmist: 
    
    - **Testi ühendust** jaotise abil kohapealse andmeallika lüüsi abil.
    - Klõpsake **Vaate logid** Andmehalduslüüsi log aknas Sündmusevaatur kuvamiseks. 
    - Klõpsake nuppu **Saada logid** logid seitsme viimase päeva zip faili üleslaadimiseks Microsoft oma probleemide tõrkeotsingu hõlbustamiseks. 
10. Vahekaardil **diagnostika** jaotises **Ühenduse testimiseks** valige **SqlServer** tüüpi andmesalve, sisestage andmebaasiserveri nimi andmebaasi nimi, määrake autentimistüüp, sisestage kasutajanimi ja parool ja **testida** testimiseks lüüsi saate andmebaasiga ühenduse loomiseks klõpsake. 
11. Veebibrauseri ja **Azure portaali**, klõpsake **OK** **konfigureerimine** enne ja seejärel **uute andmete lüüsi** enne.
6. Peaksite nägema **adftutorialgateway** jaotises **Andmete lüüside** puuvaates vasakul.  Kui klõpsate, peaksite nägema seotud JSON. 
    

## <a name="create-linked-services"></a>Looge lingitud teenused 
Selles etapis tuleb teil luua kaks lingitud teenuste: **AzureStorageLinkedService** ja **SqlServerLinkedService**. **SqlServerLinkedService** lingid andmete factory loomine asutusesisese SQL Serveri andmebaasiga ja **AzureStorageLinkedService** lingitud teenuse lingid mõne Azure'i bloobimälu talletada. Saate luua müügivõimaluste hiljem selle kiirtutvustus, mis kopeerib asutusesisese SQL serveri andmebaasi andmete Azure'i bloobimälu pood. 

#### <a name="add-a-linked-service-to-an-on-premises-sql-server-database"></a>Lingitud teenuse lisamine asutusesisese SQL Serveri andmebaasiga
1.  **Andmete Factory redaktori**, klõpsake tööriistaribal nuppu **uute andmete talletamiseks** ja valige **SQL serveri**. 

    ![Uue lingitud SQL Serveri teenuse](./media/data-factory-move-data-between-onprem-and-cloud/NewSQLServer.png) 
3.  **JSON-redaktor** paremal, tehke järgmist. 
    1. Määratlege **gatewayName** **adftutorialgateway**. 
    2. **ConnectionString**, tehke järgmist. 
        1. Sisestage **Serveri nimi**, SQL serveri andmebaasi majutava serveri nimi.
        2. **Databasename**, sisestage andmebaasi nimi.
        3. Klõpsake tööriistaribal nuppu **krüptimine** . See allalaaditavate failide ja käivitab identimisteabe halduri rakenduse.
        
            ![Identimisteabe halduri rakendus](./media/data-factory-move-data-between-onprem-and-cloud/credentials-manager-application.png)
        5. Dialoogiboksis **Säte identimisteabe** määrata autentimistüüp, kasutajanimi ja parool ja klõpsake nuppu **OK**. Kui ühendus õnnestus, krüptitud identimisteave on salvestatud on JSON ja dialoogiboks suletakse. 
        6. Sulgege tühja brauseri vahekaarti, mis on käivitatud dialoogiboksi, kui see on automaatselt suletud ja naasta vahekaardile Azure'i portaalis. 
  
            Lüüsiseadmega, neid mandaate on **krüptitud** serdiga andmete Factory teenuse omanik. Kui soovite kasutada serdi, mis on seotud Andmehalduslüüsi selle asemel, vt [turvaliselt identimisteabe määramine](#set-credentials-and-security).    
    1.  Klõpsake **Deploy** käsuriba lingitud SQL Serveri teenuse juurutamiseks. Peaksite nägema lingitud teenuse puuvaates. 
        
        ![Puuvaates lingitud SQL Serveri teenuse](./media/data-factory-move-data-between-onprem-and-cloud/sql-linked-service-in-tree-view.png)  

#### <a name="add-a-linked-service-for-an-azure-storage-account"></a>Lingitud teenuse konto Azure salvestusruumi lisamine
 
1. **Andmete Factory Editor** **uute andmete talletamiseks** käsuriba ja seejärel klõpsake käsku **Azure salvestusruumi**.
2. Sisestage oma Azure storage konto nimi **konto nimi**.
3. Sisestage konto Azure storage **konto võti**.
4. Klõpsake käsku **Deploy** **AzureStorageLinkedService**juurutamiseks.
   
 
## <a name="create-datasets"></a>Andmekomplektide loomine
Selles etapis tuleb luua sisendi ja väljundi kogumid, mis tähistavad sisend- ja andmete kopeerimine (asutusesisese SQL serveri andmebaasi = > Azure'i bloobimälu). Enne andmekomplektide loomist tehke järgmist (üksikasjalikud juhised järgneb loend).

- Nimega **emp** SQL serveri andmebaasi andmete factory lingitud teenus lisasite tabeli loomine ja lisamine paar valimi kirjed tabelisse.
- Luua nimega **adftutorial** Azure'i bloobimälu salvestusruumi konto andmeid factory lingitud teenus lisasite bloobimälu ümbris.

### <a name="prepare-on-premises-sql-server-for-the-tutorial"></a>Õpetuse asutusesisese SQL serveri ettevalmistamine

1. Määratud asutusesisese SQL serveri andmebaasi lingitud teenuse (**SqlServerLinkedService**) **emp** tabeli andmebaasi loomine järgmine SQL-i skripti abil.

        CREATE TABLE dbo.emp
        (
            ID int IDENTITY(1,1) NOT NULL, 
            FirstName varchar(50),
            LastName varchar(50),
            CONSTRAINT PK_emp PRIMARY KEY (ID)
        )
        GO 
2. Tabeli teatud valimi lisamiseks tehke järgmist. 

        INSERT INTO emp VALUES ('John', 'Doe')
        INSERT INTO emp VALUES ('Jane', 'Doe')

### <a name="create-input-dataset"></a>Sisestuskeel andmekomplekti loomine

1. Klõpsake **Andmete Factory redaktori** **... Lisateavet**, klõpsake käsku **Uus andmekomplekti** ja klõpsake käsku **SQL serveri tabel**. 
2.  Asendage JSON parempoolsel paanil järgmine tekst:
        
            {       
                "name": "EmpOnPremSQLTable",
                "properties": {
                    "type": "SqlServerTable",
                    "linkedServiceName": "SqlServerLinkedService",
                    "typeProperties": {
                        "tableName": "emp"
                    },
                    "external": true,
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    },
                    "policy": {
                        "externalData": {
                            "retryInterval": "00:01:00",
                            "retryTimeout": "00:10:00",
                            "maximumRetry": 3
                        }
                    }
                }
            }    

    Võtke arvesse järgmist. 

    - **Tüüp** on seatud **SqlServerTable**.
    - **tableName** on seatud **emp**.
    - **linkedServiceName** on seatud **SqlServerLinkedService** (olete loonud seda lingitud teenust varem juhendis.).
    - Sisestuskeel andmekogumi, ei loodud teise müügivõimaluste Azure'i andmed Factory sisse, peate määrama **välise** väärtuseks **true**. See tähistab sisendandmete toodetud välise Azure'i andmed Factory teenusega. Soovi korral saate määrata, mis tahes välisandmete poliitikate **poliitika** jaotise **externalData** elemendi abil.    

    JSON atribuutide kohta vt [teisaldada andmeid SQL Server](data-factory-sqlserver-connector.md) .
2. Klõpsake **Deploy** käsuriba andmekomplekti juurutamiseks.  


### <a name="create-output-dataset"></a>Väljundi andmekomplekti loomine

1.  **Andmete Factory redaktori**, klõpsake käsku **Uus andmekomplekti** , ja klõpsake nuppu **Azure'i bloobimälu**.
2.  Asendage JSON parempoolsel paanil järgmine tekst: 

            {
                "name": "OutputBlobTable",
                "properties": {
                    "type": "AzureBlob",
                    "linkedServiceName": "AzureStorageLinkedService",
                    "typeProperties": {
                        "folderPath": "adftutorial/outfromonpremdf",
                        "format": {
                            "type": "TextFormat",
                            "columnDelimiter": ","
                        }
                    },
                    "availability": {
                        "frequency": "Hour",
                        "interval": 1
                    }
                }
            }
  
    Võtke arvesse järgmist. 
    
    - **Tüüp** on seatud **AzureBlob**.
    - **linkedServiceName** on seatud **AzureStorageLinkedService** (olete loonud lingitud teenuse samm 2).
    - **folderPath** on seatud **adftutorial/outfromonpremdf** outfromonpremdf sisaldavasse kausta adftutorial ümbrises. Kui see pole juba olemas, saate luua **adftutorial** ümbrises. 
    - **Kättesaadavus** väärtuseks **tunni** (**sagedus** **tund** seadmine ja **intervall** väärtuseks **1**).  Andmete Factory teenuse loob mõne väljundi andmete sektorit iga tunni SQL Azure'i andmebaasi tabelisse **emp** . 

    Kui te ei määra on **väljund tabeli** **nimi** , **folderPath** failid teie enda loodud on nimega järgmises vormingus: andmed. <Guid>txt (nt:: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt.).

    **FolderPath** ja **failinimi** dünaamiliselt põhjal **SliceStart** aja määramiseks kasutage partitionedBy atribuuti. Järgmises näites folderPath kasutab aasta, kuu ja päeva SliceStart (sektorit, et töödelda alguskellaaeg) kaudu ja failinimi kasutab funktsiooni SliceStart tund. Näiteks kui tükk on koostatud 2014-10-20T08:00:00, wikidatagateway/wikisampledataout/2014/10/20 on seatud soovitud kaustanimi ja faili nimi on seatud 08.csv. 

        "folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
        "fileName": "{Hour}.csv",
        "partitionedBy": 
        [
            { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
            { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } }, 
            { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } }, 
            { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } } 
        ],

 
    [Teisalda andmed Azure'i bloobimälu](data-factory-azure-blob-connector.md) kohta vaadake teavet JSON atribuudid.
2.  Klõpsake **Deploy** käsuriba andmekomplekti juurutamiseks. Veenduge, et näete nii puuvaates kogumid.  

## <a name="create-pipeline"></a>Müügivõimaluste loomine
Selles etapis tuleb teil luua **müügivõimaluste** üks **Kopeeri tegevust** , mis kasutab **EmpOnPremSQLTable** sisendina ja **OutputBlobTable** väljundina.

1.  Andmete Factory redaktor klõpsake **... Veel**, ja klõpsake nuppu **Uus kohaletoimetamisel**. 
2.  Asendage JSON parempoolsel paanil järgmine tekst: 
    
            {
                "name": "ADFTutorialPipelineOnPrem",
                "properties": {
                "description": "This pipeline has one Copy activity that copies data from an on-prem SQL to Azure blob",
                "activities": [
                {
                    "name": "CopyFromSQLtoBlob",
                    "description": "Copy data from on-prem SQL server to blob",
                    "type": "Copy",
                    "inputs": [
                    {
                        "name": "EmpOnPremSQLTable"
                    }
                    ],
                    "outputs": [
                    {
                        "name": "OutputBlobTable"
                      }
                    ],
                    "typeProperties": {
                      "source": {
                        "type": "SqlSource",
                        "sqlReaderQuery": "select * from emp"
                      },
                      "sink": {
                        "type": "BlobSink"
                      }
                    },
                    "Policy": {
                      "concurrency": 1,
                      "executionPriorityOrder": "NewestFirst",
                      "style": "StartOfInterval",
                      "retry": 0,
                      "timeout": "01:00:00"
                    }
                  }
                ],
                "start": "2016-07-05T00:00:00Z",
                "end": "2016-07-06T00:00:00Z",
                "isPaused": false
              }
            }

    > [AZURE.IMPORTANT]
    > Asendage praeguse päeva- ja **lõppaeg** väärtus järgmisest päevast **käivitamine** atribuudi väärtust.

    Võtke arvesse järgmist.
 
    - Jaotises tegevused on ainult tegevust, mille **Tüüp** on seatud **Kopeeri**.
    - **Sisestusmeetodi** tegevuse jaoks on määratud **EmpOnPremSQLTable** ja **väljundi** tegevuse jaoks on määratud **OutputBlobTable**.
    - Jaotises **typeProperties** **SqlSource** on määratud **Reaallika tüüp** ja **BlobSink **on määratud **valamu tüüp**.
    - SQL-päringu `select * from emp` on määratud **sqlReaderQuery** atribuudi **SqlSource**.

     Mõlemad käivitamine ja end kuupäevade ja kellaaegade peab olema [ISO](http://en.wikipedia.org/wiki/ISO_8601)-vormingus. Näide: 2014-10-14T16:32:41Z. **Lõpuaeg** pole kohustuslik, kuid kasutame selles õpetuses. 
    
    Kui määrate atribuudi **end** väärtus arvutatakse kui "**start + 48 tundi**". Tulemas lõputult käivitamiseks määrata **9999/9/9** **end** atribuudi väärtust. 
    
    Määratlemisel, milles töödeldakse sektorid andmete põhjal **kättesaadavus** atribuudid, mis on määratletud iga Azure'i andmed Factory andmekogumi kestel.
    
    Näites on 24 andmete sektorit iga andmete sektorit on toodetud tunnis.     
2. Klõpsake käsku **Deploy** käsuribal juurutamiseks andmekomplekti (tabel on ristküliku andmekomplekti). Veenduge, et tulemas näitab üles puuvaates **torujuhtmete** sõlme all.  
5. Nüüd klõpsake kaks korda sulgemiseks labad naasmiseks **Andmete Factory** tera jaoks **ADFTutorialOnPremDF** **X** .

**Palju õnne!** Teil on edukalt loodud on Azure andmete factory, lingitud teenused, andmekomplektide ja müügivõimaluste ja ajastatud tulemas.

#### <a name="view-the-data-factory-in-a-diagram-view"></a>Diagrammi vaate factory andmete kuvamine 
1. **Azure'i portaal**, klõpsake **diagrammi** paani **ADFTutorialOnPremDF** andmete Factory avalehel. :

    ![Diagrammi Link](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramLink.png)

2. Peaksite nägema sarnaselt allolevale pildile skeemi:

    ![Diagrammivaate](./media/data-factory-move-data-between-onprem-and-cloud/OnPremDiagramView.png)

    Saate suurendada, vähendamine, suumi 100%, suumi mahutamiseks automaatselt paigutage torujuhtmed ja andmekomplektide ja Kuva pärandi teave (esile enne ja üksused valitud üksuste).  Topeltklõpsake objekti (sisend andmekomplekti või müügivõimaluste) seda atribuutide kuvamiseks. 

## <a name="monitor-pipeline"></a>Müügivõimaluste jälgimine
Selles juhises saate jälgida, mis toimub on Azure andmete factory Azure portaali. PowerShelli cmdlet-käskude abil andmekomplektide ja torujuhtmete jälgimine. Jälgimise kohta leiate üksikasjalikumat teavet teemast [jälgimine ja haldamine torujuhtmete](data-factory-monitor-manage-pipelines.md).

5. Topeltklõpsake soovitud diagrammi **EmpOnPremSQLTable**.  

    ![EmpOnPremSQLTable sektorid](./media/data-factory-move-data-between-onprem-and-cloud/OnPremSQLTableSlicesBlade.png)

6. Märkate, et kõik andmed viilud häälestamine on **valmis** olekus, kuna müügivõimaluste kestus (algusaja lõpuaeg) on varem. Samuti on Kuna sisestasite andmed SQL Serveri andmebaas ja see on kogu aeg. Veenduge, et allosas jaotise **probleemi sektorid** kuvataks pole sektorid. Kõik sektorid kuvamiseks klõpsake nuppu **Vaata veel** sektorid loendi allservas. 
7. Nüüd klõpsake **andmekomplektide** labale **OutputBlobTable**.

    ![OputputBlobTable sektorid](./media/data-factory-move-data-between-onprem-and-cloud/OutputBlobTableSlicesBlade.png)
9. Klõpsake mis tahes andmete sektorit loendist näete **Andmete sektorit** tera. Näete tegevuse töötab mitu sektorit. Kuvatakse ainult üks tegevuste käivitamine tavaliselt.  

    ![Andmete sektorit Blade](./media/data-factory-move-data-between-onprem-and-cloud/DataSlice.png)

    Kui **olete valmis** olekus pole sektorit, näete varustava sektoritele, mis ei ole valmis ja blokeerivad kaudu käivitamisel **varustava sektoritele, mis on valmis pole** loendis praegune sektorit.

10. Klõpsake soovitud **tegevuste käivitamine** loendist allosas **tegevuste käivitamine üksikasjade**kuvamiseks.

    ![Tegevuste käivitamine üksikasjad blade](./media/data-factory-move-data-between-onprem-and-cloud/ActivityRunDetailsBlade.png)

    Näete läbilaskevõime, kestus ja lüüsi andmete edastamiseks kasutada näiteks järgmist teavet. 
11. Klõpsake nuppu **X** , et sulgeda kõik labad, kuni te 
12. saada tagasi home tera **ADFTutorialOnPremDF**.
14. (valikuline) Klõpsake **torujuhtmete**, klõpsake **ADFTutorialOnPremDF**ja süvitsi Sisestuskeel tabelite (**Keskmine**) või väljundi andmekomplektide (**toodetud**).
15. Veenduge, et iga tund on loodud bloobimälu/faili näiteks kasutada tööriistu, nt [Microsoft salvestusruumi Exploreri](http://storageexplorer.com/) abil.

    ![Azure'i salvestusruumi Explorer](./media/data-factory-move-data-between-onprem-and-cloud/OnPremAzureStorageExplorer.png)

## <a name="next-steps"></a>Järgmised sammud

- Vt [Andmehalduslüüsi](data-factory-data-management-gateway.md) artikkel Andmehalduslüüsi üksikasjad.
- Teemast [Azure SQL Azure'i bloobimälu Kopeeri andmete](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) kasutamine andmete teisaldamiseks allika andmete poest valamu andmete poe Kopeeri tegevuse kohta. 
