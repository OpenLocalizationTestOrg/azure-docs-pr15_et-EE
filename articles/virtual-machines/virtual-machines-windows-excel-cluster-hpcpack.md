<properties
 pageTitle="Excel ja SOA HPC Pack kobar | Microsoft Azure'i"
 description="Töö alustamine töötab suuremahuliste Exceli ja SOA töökoormus on Azure HPC Pack kobar"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,hpc-pack"/>

<tags
 ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-windows"
 ms.workload="big-compute"
 ms.date="08/25/2016"
 ms.author="danlep"/>

# <a name="get-started-running-excel-and-soa-workloads-on-an-hpc-pack-cluster-in-azure"></a>Töötab, Excel ja SOA töökoormus on HPC Pack klaster Azure alustamine

Selles artiklis kirjeldatakse Microsoft HPC Pack kobar klõpsake Azure'i virtuaalmasinates juurutamise mõni Azure Kiirjuhend mall või soovi korral on juurutamise Azure PowerShelli skripti abil. Klaster kasutab Azure'i turuplatsi VM pilte, mis on mõeldud töötama koos HPC Pack Microsoft Excelist või teenusele suunatud arhitektuuri (SOA) töökoormus. Klaster abil saate käivitada lihtsa Exceli HPC ja SOA teenuste kohapealse klientarvutist. Exceli töövihiku mahalaadimine ja Exceli kasutaja määratletud funktsioonide või UDF kaasamine HPC Exceli teenused.

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Kõrge, järgmisel joonisel on esitatud HPC Pack kobar loomist.

![Koos sõlmed operatsioonisüsteemi Exceli töökoormus HPC kobar][scenario]

## <a name="prerequisites"></a>Eeltingimused

*   **Klientarvuti** - peate Windowsi-põhise klientarvuti esitada näidis Excelis ja SOA töid klaster. Samuti vajate Windowsi arvuti käivitamiseks kobar juurutamise Azure PowerShelli skripti (kui valite selle juurutamise meetod) ja

*   **Azure'i tellimuse** – kui teil pole Azure tellimuse, saate luua [tasuta konto](https://azure.microsoft.com/pricing/free-trial/) vaid paar minutit.

*   **Tuuma kvoodi** - peate suurendada kvoote valdkond, eriti siis, kui juurutate mitu kobar sõlme mitmesoonelised VM suurusega. Kui kasutate mõni Azure Kiirjuhend Mall, tuuma kvoodi sisse ressursihaldur on Azure piirkonna kohta. Sel juhul peate konkreetse piirkonna talletusmahu suurendamiseks. Leiate [Azure'i tellimuse piirangud, kvootide ja piiranguid](../azure-subscription-service-limits.md). Suurendamiseks kvoodi, [avage mõni Online'i kliendi tugiteenuse taotluse](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/) tasuta.

*   **Microsoft Office'i litsentsi** – kui juurutate Arvuta sõlmed turuplatsi HPC Pack VM pilt kasutades Microsoft Excel, Microsoft Exceli Professional Plus 2013 30-päevane prooviversioon on installitud. Pärast tutvumist võimaldava peate sisestage kehtiv Microsoft Office'i litsentsi aktiveerimiseks Exceli edasi töötada töökoormus. Selle artikli teemast [Exceli aktiveerimine](#excel-activation) . 


## <a name="step-1-set-up-an-hpc-pack-cluster-in-azure"></a>Samm 1. Häälestamine on Azure HPC Pack kobar

Näitame häälestamiseks klaster kaks võimalust: peate esmalt mõni Azure Kiirjuhend Mall ja Azure portaali; ja teine, on juurutamise Azure PowerShelli skripti abil.


### <a name="option-1-use-a-quickstart-template"></a>Suvand 1. Kiirjuhend malli kasutamine
Mõne Azure Kiirjuhend malli abil saate kiiresti ja kerge vaevaga juurutada mõne HPC Pack kobar Azure'i portaalis. Malli avamisel portaalis saate lihtsa kasutajaliides, kus saate sisestada sätted klaster. Siin on toodud juhised. 

>[AZURE.TIP]Soovi korral võite kasutada mõni [Azure'i turuplatsi Mall](https://portal.azure.com/?feature.relex=*%2CHubsExtension#create/microsofthpc.newclusterexcelcn) , mis loob sarnaseid kobar spetsiaalselt Exceli töökoormus. Veidi erineda järgmisi juhiseid.

1.  Külastage [loomine HPC kobar github leht mallina](https://github.com/Azure/azure-quickstart-templates/tree/master/create-hpc-cluster). Kui soovite, lugege teavet Mall ja lähtekoodi.

2.  Klõpsake käsku **Deploy Azure** juurutamine alustada malli Azure'i portaalis.

    ![Azure'i malli juurutamine][github]

3.  Portaalis, tehke parameetrid sisestage HPC kobar malli.

    lisamine. Klõpsake lehel **Parameetrid** sisestage või muutke malli parameetrite väärtused. (Klõpsake iga säte abi saamiseks kõrval olevat ikooni.) Näidisväärtuste on näidatud järgmisel kuvatõmmisel. Selles näites loob nimega *hpc01* *hpc.local* Domeen koosneb pea sõlme klaster ja 2 arvutada sõlmed. Arvuta sõlmed on loodud HPC Pack VM pilt, mis sisaldab Microsoft Exceli.

    ![Sisestage parameetrid][parameters]

    >[AZURE.NOTE]Pea sõlme VM luuakse automaatselt [uusima turuplatsi pilt](https://azure.microsoft.com/marketplace/partners/microsoft/hpcpack2012r2onwindowsserver2012r2/) HPC Pack 2012 R2 Windows Server 2012 R2. Praegu põhineb HPC Pack 2012 R2 Update 3 pilt.
    >
    >Arvuta sõlm VMs luuakse valitud Arvuta sõlm pere uusim pilt. Valige suvand **ComputeNodeWithExcel** uusima HPC Pack Arvuta sõlm pilt, mis sisaldab Microsoft Exceli Professional Plus 2013 hindamise versiooni jaoks. Juurutada klaster üldist SOA seansid või Exceli UDF mahalaadimine, valige suvand **ComputeNode** (ilma installitud Excel).

    b. Valige tellimus.

    c. Ressursirühma kobar, nt *hpc01RG*jaoks luua.

    d. Valige asukoht ressursirühma, nt Kesk-USA.

    e. Vaadake lehel **õiguslikult** tingimustel. Kui nõustute sellega, klõpsake nuppu **osta**. Siis, kui olete lõpetanud, malli jaoks soovitud väärtused, klõpsake nuppu **Loo**.

4.  Kui juurutamise lõpetab (tavaliselt kulub 30 minutit umbes), kobar pea sõlme kobar serdi faili eksportida. Hiljem juhises impordite selle avaliku serdi esitada serveripoolne autentimise jaoks HTTP turvaline sidumine klientarvutis.

    lisamine. Ühenduse loomine kaugtöölaua pea sõlme Azure portaalist.

     ![Ühenduse loomine pea sõlme][connect]

    b. Kasutage standard toiminguid serdi haldur eksportida ilma privaatvõti (asub jaotise Cert: \LocalMachine\My) pea sõlme serdi. Selles näites eksportimine *CN = hpc01.eastus.cloudapp.azure.com*.

    ![Serdi eksportimine][cert]

### <a name="option-2-use-the-hpc-pack-iaas-deployment-script"></a>Variant 2. Kasutage skripti HPC Pack IaaS juurutamine

HPC Pack IaaS juurutamise skripti pakub mitmekülgne teine võimalus on HPC Pack kobar juurutamine. See loob klaster mudelis klassikaline juurutamise tuleks Mall kasutab Azure ressursihaldur juurutamise mudel. Lisaks skripti ühildub teenuses Azure üld- või Azure Hiina tellimus.

**Täiendavad eeltingimused**

* **Azure'i PowerShelli** - [installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) (versioon 0.8.10 või uuem versioon) klientarvutis.

* **HPC Pack IaaS juurutamise skripti** - alla laadida ja lahti script [Microsofti allalaadimiskeskusest](https://www.microsoft.com/download/details.aspx?id=44949)uusim versioon. Skripti versiooni kontrollimiseks käivitades `New-HPCIaaSCluster.ps1 –Version`. Selles artiklis põhineb versioon 4.5.0 või uuem versioon, skripti.

**Otsingukonfiguratsiooni faili loomine**

 HPC Pack IaaS juurutamise skript kasutab XML-konfiguratsioonifail sisendina HPC kobar taristu kirjeldav. Klaster koosneb pea sõlme ja 18 Arvuta sõlmed Arvuta sõlm pilt, mis sisaldab Microsoft Exceli loodud juurutamiseks asendada oma keskkonna konfiguratsioon järgmine näidisfaili väärtused. Konfiguratsioonifail kohta lisateabe saamiseks vaadake Manual.rtf faili skripti kausta ja [loomine mõne HPC kobar HPC Pack IaaS juurutamise skripti](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md).

```
<?xml version="1.0" encoding="utf-8"?>
<IaaSClusterConfig>
  <Subscription>
    <SubscriptionName>MySubscription</SubscriptionName>
    <StorageAccount>hpc01</StorageAccount>
  </Subscription>
  <Location>West US</Location>
  <VNet>
    <VNetName>hpc-vnet01</VNetName>
    <SubnetName>Subnet-1</SubnetName>
  </VNet>
  <Domain>
    <DCOption>NewDC</DCOption>
    <DomainFQDN>hpc.local</DomainFQDN>
    <DomainController>
      <VMName>HPCExcelDC01</VMName>
      <ServiceName>HPCExcelDC01</ServiceName>
      <VMSize>Medium</VMSize>
    </DomainController>
  </Domain>
   <Database>
    <DBOption>LocalDB</DBOption>
  </Database>
  <HeadNode>
    <VMName>HPCExcelHN01</VMName>
    <ServiceName>HPCExcelHN01</ServiceName>
    <VMSize>Large</VMSize>
    <EnableRESTAPI/>
    <EnableWebPortal/>
    <PostConfigScript>C:\tests\PostConfig.ps1</PostConfigScript>
  </HeadNode>
  <ComputeNodes>
    <VMNamePattern>HPCExcelCN%00%</VMNamePattern>
    <ServiceName>HPCExcelCN01</ServiceName>
    <VMSize>Medium</VMSize>
    <NodeCount>18</NodeCount>
    <ImageName>HPCPack2012R2_ComputeNodeWithExcel</ImageName>
  </ComputeNodes>
</IaaSClusterConfig>
```

**Märkmete konfiguratsiooni faili kohta**

* **VMName** pea sõlme **peab** olema sama mis **teenuse nimi**või SOA töö ei käivitamiseks.

* Veenduge, et saate määrata **EnableWebPortal** , nii et pea sõlme sert on loodud ja eksportida.

* Faili saate määrata järgmised PowerShelli skripti PostConfig.ps1, mis töötab pea sõlme. Järgmine näidis skript konfigureerib Azure storage ühendusstring, eemaldab Arvuta sõlme rolli pea sõlme ja toob kõik sõlmed võrgus, kui need on juurutatud. 

```
    # add the HPC Pack powershell cmdlets
        Add-PSSnapin Microsoft.HPC

    # set the Azure storage connection string for the cluster
        Set-HpcClusterProperty -AzureStorageConnectionString 'DefaultEndpointsProtocol=https;AccountName=<yourstorageaccountname>;AccountKey=<yourstorageaccountkey>'

    # remove the compute node role for head node to make sure the Excel workbook won’t run on head node
        Get-HpcNode -GroupName HeadNodes | Set-HpcNodeState -State offline | Set-HpcNode -Role BrokerNode

    # total number of nodes in the deployment including the head node and compute nodes, which should match the number specified in the XML configuration file
        $TotalNumOfNodes = 19

        $ErrorActionPreference = 'SilentlyContinue'

    # bring nodes online when they are deployed until all nodes are online
        while ($true)
        {
          Get-HpcNode -State Offline | Set-HpcNodeState -State Online -Confirm:$false
          $OnlineNodes = @(Get-HpcNode -State Online)
          if ($OnlineNodes.Count -eq $TotalNumOfNodes)
          {
             break
          }
          sleep 60
        }
```

**Käivitage skript**

1.  Avage PowerShelli konsooli kliendi arvutisse administraatorina.

2.  Muuta kataloogi kausta script (selles näites E:\IaaSClusterScript).

    ```
    cd E:\IaaSClusterScript
    ```
    
3.  Juurutada HPC Pack kobar, käivitage järgmine käsk. Selles näites eeldab, et konfiguratsiooni fail asub E:\HPCDemoConfig.xml.

    ```
    .\New-HpcIaaSCluster.ps1 –ConfigFile E:\HPCDemoConfig.xml –AdminUserName MyAdminName
    ```

HPC Pack juurutamise script töötab juba mõnda aega. Skript ei te102827792ei eksportida ja laadige alla serdi kobar ja salvestage see praeguse kasutaja kausta dokumendid klientarvuti. Skripti luuakse järgmine teade. Järgmises etapis asjakohane tunnistus poes serdi importimist.    
    
    You have enabled REST API or web portal on HPC Pack head node. Please import the following certificate in the Trusted Root Certification Authorities certificate store on the computer where you are submitting job or accessing the HPC web portal:
    C:\Users\hpcuser\Documents\HPCWebComponent_HPCExcelHN004_20150707162011.cer

## <a name="step-2-offload-excel-workbooks-and-run-udfs-from-an-on-premises-client"></a>Samm 2. Exceli töövihikute Offload ja käivitage UDF-ID asutusesiseses klient

### <a name="excel-activation"></a>Exceli aktiveerimine

Tootmise töökoormus ComputeNodeWithExcel VM pilt kasutamisel peate sisestage kehtiv Microsoft Office'i võti aktiveerida Exceli Arvuta sõlmed. Muul juhul Exceli prooviversioon aegub 30 päeva pärast ja töötab Exceli töövihikuid ei õnnestu COMException (0x800AC472). 

Võite taasrelvastuma Exceli teise 30 päeva jooksul hindamise: logige pea sõlme ja clusrun `%ProgramFiles(x86)%\Microsoft Office\Office15\OSPPREARM.exe` sisse kõik Exceli arvutada sõlmed HPC halduri kaudu. Võite taasrelvastuma kuni kaks korda. Pärast seda, peate sisestama kehtiva Office'i litsentsi võti.

Office Professional Plus 2013 installitud VM pilt on hulgilitsentsiväljaannet koos mõne üldise helitugevuse litsentsi võti (GVLK). Saate selle aktiveerida kaudu Võtmehaldusteenus (KMS) / Active Directory-Based aktiveerimine (AD-BA) või multiaktiveerimisvõti (MAK). 

    * Kui soovite kasutada KMS-i/AD-BA, olemasoleva KMS-i serveri või häälestada uus Microsoft Office 2013 helitugevuse litsentsi Pack abil. (Kui soovite, saate seadistada serveri pea sõlme.) Seejärel aktiveerige KMS-i hosti võtme Internetis või telefoni teel. Seejärel clusrun `ospp.vbs` KMS-i serveri ja pordi määramine ja kogu Office'i aktiveerimine Exceli arvutada sõlmed. 
    
    * Kasutama MAK-i esimese clusrun `ospp.vbs` sisestada klahvi ja seejärel aktiveerige kõik Exceli arvutada sõlmed Internetis või telefoni teel. 

>[AZURE.NOTE]Jaemüügi tootevõtmed Office Professional Plus 2013 ei saa kasutada seda VM pilt. Kui teil on kehtiv võtmed ja installikandja Office'i või Exceli väljaanded, peale seda Office Professional Plus 2013 hulgilitsentsiväljaannet, saate nende asemel kasutada. Esmalt desinstallige see hulgilitsentsiväljaannet ja millele teil on väljaande installimiseks. Exceli installeerida MSDN saate jäädvustata juurutamise skaala kasutamine kohandatud VM pilt.

### <a name="offload-excel-workbooks"></a>Exceli töövihikute Offload

Järgige neid juhiseid, et nii, et see käivitatakse HPC Pack kobar Azure Exceli töövihik. Selle tegemiseks peate Excel 2010 või 2013 kliendi arvutisse juba installitud.

1. Kasutage ühte samm 1 juurutada mõne HPC Pack kobar koos Exceli suvandid Arvutage sõlm pilt. Hankige kobar serdifail (CER) ja kobar kasutajanimi ja parool.

2. Klientarvuti, importige jaotises Cert: \CurrentUser\Root kobar sert.

3. Veenduge, et on installitud Excel. Looge Excel.exe.config fail Excel.exe samasse kausta klientarvuti järgmised sisuga. Selles etapis tuleb tagab, et selle HPC Pack 2012 R2 Exceli COM-lisandmooduli laadib edukalt.

    ```
    <?xml version="1.0"?>
    <configuration>
        <startup useLegacyV2RuntimeActivationPolicy="true">
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.0"/>
        </startup>
    </configuration>
    ```
    
4.  Häälestada kliendi esitada töid HPC Pack klaster. Üheks võimaluseks on täielik [HPC Pack 2012 R2 Update 3 installimine](http://www.microsoft.com/download/details.aspx?id=49922) alla laadima ja installima HPC Pack kliendi. Teise võimalusena alla laadida ja installida [HPC Pack 2012 R2 Update 3 kliendi Utiliidid](https://www.microsoft.com/download/details.aspx?id=49923) ja vastav Visual C++ 2010 redistributable teie arvutis ([x64](http://www.microsoft.com/download/details.aspx?id=14632), [x86](https://www.microsoft.com/download/details.aspx?id=5555)).

5.  Selles näites me kasutame valimi Exceli töövihiku nimega ConvertiblePricing_Complete.xlsb. Saate selle tasuta alla [siin](https://www.microsoft.com/en-us/download/details.aspx?id=2939).

6.  Kopeerige töökausta, nt D:\Excel\Run Exceli töövihik.

7.  Avage Exceli töövihik. Lindi **töötada** , klõpsake **COM-lisandmoodulid** ja veenduge, et selle HPC Pack Exceli COM-lisandmooduli on edukalt laaditud.

    ![Exceli lisandmoodul HPC Pack][addin]

8.  VBA makrot HPCControlMacros Excelis redigeerida, muutes eemaldage kommenteeritud väljad, nagu on näidatud järgmises skripti. Asendage väärtused keskkonnas.

    ![Exceli makro HPC Pack][macro]

    ```
    'Private Const HPC_ClusterScheduler = "HEADNODE_NAME"
    Private Const HPC_ClusterScheduler = "hpc01.eastus.cloudapp.azure.com"

    'Private Const HPC_NetworkShare = "\\PATH\TO\SHARE\DIRECTORY"
    Private Const HPC_DependFiles = "D:\Excel\Upload\ConvertiblePricing_Complete.xlsb=ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.Initialize ActiveWorkbook
    HPCExcelClient.Initialize ActiveWorkbook, HPC_DependFiles

    'HPCWorkbookPath = HPC_NetworkShare & Application.PathSeparator & ActiveWorkbook.name
    HPCWorkbookPath = "ConvertiblePricing_Complete.xlsb"

    'HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath
    HPCExcelClient.OpenSession headNode:=HPC_ClusterScheduler, remoteWorkbookPath:=HPCWorkbookPath, UserName:="hpc\azureuser", Password:="<YourPassword>"
```

9.  Kopeerige Exceli töövihik üles kataloogi, nt D:\Excel\Upload. See kaust on määratud HPC_DependsFiles konstandi VBA makro.

10. Azure klaster töövihiku käivitamiseks nuppu **kobar** töölehel.

### <a name="run-excel-udfs"></a>Käivitage Excel UDF-ID

Exceli UDF käivitamiseks järgige eelmises 1 – 3, et klientarvuti häälestamine. For Excel UDF-ID, ei pea olema installitud Arvuta sõlmed Exceli rakendus. Siis, kui loomise klaster arvutada sõlmed, võib valida tavaline arvutada sõlm pildi asemel Arvuta sõlm pilt Exceli.

>[AZURE.NOTE] On 34 märgi limiit rakenduses Excel 2010 ja 2013 kobar Connectori dialoogiboks. Selle dialoogiboksi abil saate määrata kobar, mis töötab soovitud UDF-ID. Kui täielik kobar nimi on pikem (nt hpcexcelhn01.southeastasia.cloudapp.azure.com), ei sobi dialoogiboksis. Lahendus on väärtusega pikk kobar nime nagu *CCP_IAASHN* hõlmav muutujana. Seejärel sisestage *CCP_IAASHN %* dialoogiboksi kobar pea sõlme nimi. 

Pärast klaster on edukalt juurutatud, jätkake sisemiste valimi käivitamiseks tehke järgmist Exceli UDF-i. Kohandatud Exceli UDF-ID, vaadake neid [ressursid](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) on XLL luua ja juurutada neid IaaS klaster.

1.  Avage uus Exceli töövihik. Lindi **töötada** , klõpsake nuppu **Lisandmoodulid**. Dialoogiboksis valige **Sirvi**, liikuge kausta, %CCP_HOME%Bin\XLL32 ja valimi ClusterUDF32.xll. Kui soovitud ClusterUDF32 pole kliendi seadme, kopeerige see pea sõlme %CCP_HOME%Bin\XLL32 kaustast.

    ![Valige UDF][udf]

2.  Klõpsake menüü **fail** > **Suvandid** > **Täpsemalt**. Märkige jaotises **valemid**ruut **Luba kasutaja määratletud XLL funktsioonide käitamiseks arvutikobaras käivitamiseks**. Klõpsake nuppu **Suvandid** ja sisestage täielik kobar nime **kobar pea sõlme nimi**. (Nagu märgitud varem see väljale on piiratud 34 märki, nii, et ei pruugi pikk kobar nimi. Võite kasutada hõlmav muutuja siin pikk kobar nimi.)

    ![UDF konfigureerimine][options]

3.  Klaster UDF arvutuse käivitamiseks klõpsake lahtrit, mille väärtus =XllGetComputerNameC() ja vajutage sisestusklahvi Enter. Funktsiooni toob lihtsalt Arvuta sõlme töötab UDF nime. Esimese jaoks dialoogiboksis mandaadid küsib kasutajanime ja parooli ühenduse IaaS kobar.

    ![UDF käivitamine][run]

    Kui paljudes lahtrites arvutamiseks, vajutage klahvikombinatsiooni Alt-Shift-Ctrl + F9 töötab arvutamise kõik lahtrid.

## <a name="step-3-run-a-soa-workload-from-an-on-premises-client"></a>Samm 3. Käivitage SOA töökoormus kohapealse klient

Üldised SOA rakenduste käitamiseks HPC Pack IaaS klaster esmalt kasutage meetodite samm 1 klaster. Määrake üldise arvutada sõlm pilt sel juhul, kuna teil pole vaja Exceli Arvuta sõlmed. Seejärel järgige järgmisi juhiseid.

1. Pärast toomine kobar sert, importige see jaotises Cert: \CurrentUser\Root klientarvutis.

2. Installige [HPC Pack 2012 R2 Update 3 SDK](http://www.microsoft.com/download/details.aspx?id=49921) ja [HPC Pack 2012 R2 Update 3 kliendi Utiliidid](https://www.microsoft.com/download/details.aspx?id=49923). Nende tööriistade võimaldavad arendamise ja käivitage SOA klientrakendustes.

3. Laadige alla HelloWorldR2 [proovi kood](https://www.microsoft.com/download/details.aspx?id=41633). Avage soovitud HelloWorldR2.sln Visual Studio 2010 või 2012.

4. Projekti EchoService esmalt koostada. Seejärel juurutada teenuse IaaS kobar saate võtta kasutusele mõne kohapealse kobar samal viisil. Üksikasjalikud juhised leiate teemast Readme.doc HelloWordR2 sisse. Saate muuta ja koostada selle HellWorldR2 ja muud projektide, nagu on kirjeldatud järgmises jaotises SOA klientrakendustes, mis on Azure IaaS kobar loomiseks.

### <a name="use-http-binding-with-azure-storage-queue"></a>Azure'i salvestusruumi järjekorda Http sidumine kasutamine

Http sidumine kasutamiseks on Azure storage järjekorda muuta mõne proovi kood.

* Värskendage kobar nimi.

    ```
// Before
const string headnode = "[headnode]";
// After e.g.
const string headnode = "hpc01.eastus.cloudapp.azure.com";
or
const string headnode = "hpc01.cloudapp.net";
```

* Soovi korral saate kasutada vaikimisi TransportScheme SessionStartInfo või konkreetselt Http määramine.

```
    info.TransportScheme = TransportScheme.Http;
```

* Kasutage funktsiooni BrokerClient vaikimisi sidumine.

    ```
// Before
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session, binding))
// After
using (BrokerClient<IService1> client = new BrokerClient<IService1>(session))
```

    Või konkreetselt funktsiooni basicHttpBinding abil.

    ```
BasicHttpBinding binding = new BasicHttpBinding(BasicHttpSecurityMode.TransportWithMessageCredential);
binding.Security.Message.ClientCredentialType = BasicHttpMessageCredentialType.UserName;    binding.Security.Transport.ClientCredentialType = HttpClientCredentialType.None;
```

* Soovi korral võite UseAzureQueue lipp väärtuseks true SessionStartInfo sisse. Kui pole määratud, selle väärtuseks true, kui kobar nimi on Azure domeeni sufiksid ja selle TransportScheme on Http vaikimisi.

    ```
    info.UseAzureQueue = true;
```

###<a name="use-http-binding-without-azure-storage-queue"></a>Kasutage Azure storage järjekorda Http sidumine

Http sidumine on Azure storage järjekorda kasutamiseks konkreetselt määratud UseAzureQueue lipp soovitud SessionStartInfo FALSE.

```
    info.UseAzureQueue = false;
```

### <a name="use-nettcp-binding"></a>Kasutage NetTcp sidumine

NetTcp sidumine kasutamiseks on sarnane mõne kohapealse kobar ühenduse konfiguratsiooni. Teil on vaja avada mõne lõpp-punktid pea sõlme VM. Kui kasutasite HPC Pack IaaS juurutamise skripti klaster loomiseks, näiteks seada lõpp-punktid Azure klassikaline portaalis järgmiselt.


1. Peatage VM.

2. Lisage TCP-pordid 9090, 9087, 9091, 9094 seansi, maakler, maakler töötaja ja Data services, vastavalt

    ![Lõpp-punktid konfigureerimine][endpoint]

3. Käivitage VM.

SOA klientrakendusega nõuab muudatusi peale IaaS kobar täisnimi pea nime muutmata.

## <a name="next-steps"></a>Järgmised sammud

* Leiate lisateavet selle kohta, kuidas Exceli töökoormus HPC Pack [need ressursid](http://social.technet.microsoft.com/wiki/contents/articles/1198.windows-hpc-and-microsoft-excel-resources-for-building-cluster-ready-workbooks.aspx) .

* Lisateavet juurutamine ja SOA teenustega HPC Pack haldamise kohta leiate [Microsoft HPC Pack SOA teenuste haldamine](https://technet.microsoft.com/library/ff919412.aspx) .

<!--Image references-->
[scenario]: ./media/virtual-machines-windows-excel-cluster-hpcpack/scenario.png
[github]: ./media/virtual-machines-windows-excel-cluster-hpcpack/github.png
[template]: ./media/virtual-machines-windows-excel-cluster-hpcpack/template.png
[parameters]: ./media/virtual-machines-windows-excel-cluster-hpcpack/parameters.png
[create]: ./media/virtual-machines-windows-excel-cluster-hpcpack/create.png
[connect]: ./media/virtual-machines-windows-excel-cluster-hpcpack/connect.png
[cert]: ./media/virtual-machines-windows-excel-cluster-hpcpack/cert.png
[addin]: ./media/virtual-machines-windows-excel-cluster-hpcpack/addin.png
[macro]: ./media/virtual-machines-windows-excel-cluster-hpcpack/macro.png
[options]: ./media/virtual-machines-windows-excel-cluster-hpcpack/options.png
[run]: ./media/virtual-machines-windows-excel-cluster-hpcpack/run.png
[endpoint]: ./media/virtual-machines-windows-excel-cluster-hpcpack/endpoint.png
[udf]: ./media/virtual-machines-windows-excel-cluster-hpcpack/udf.png
