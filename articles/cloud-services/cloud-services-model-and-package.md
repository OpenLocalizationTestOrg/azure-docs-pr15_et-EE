<properties
    pageTitle="Mis on pilveteenus ja paketi | Microsoft Azure'i"
    description="Kirjeldatakse pilveteenuste mudeli (.csdef, .cscfg) ja Azure pakett (.cspkg)"
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>
<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>

# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a>Mis on pilveteenuses mudel ja kuidas see pakett see?
Pilveteenus on loodud kolmele, teenuse määratlus _(.csdef)_, teenuse config _(.cscfg)_ja pakett _(.cspkg)_. Nii **ServiceDefinition.csdef** ja **ServiceConfig.cscfg** failid on XML-põhine ja kirjeldada struktuuri pilveteenusesse ja kuidas see on konfigureeritud; mida nimetatakse mudel. **ServicePackage.cspkg** on zip-fail, mis on loodud **ServiceDefinition.csdef** ja muu hulgas, mis sisaldab kõiki nõutud binary-põhiste sõltuvused. Azure'i loob pilveteenus **ServicePackage.cspkg** nii **ServiceConfig.cscfg**.

Kui Azure töötab pilveteenusesse, saate selle faili **ServiceConfig.cscfg** kaudu uuesti konfigureerida, kuid te ei saa muuta määratluse.

## <a name="what-would-you-like-to-know-more-about"></a>Mida soovite rohkem teada?

* Kust leida lisateavet [ServiceDefinition.csdef](#csdef) ja [ServiceConfig.cscfg](#cscfg) failid.
* Ma juba teada, mis mulle [näited](#next-steps) saate konfigureerida.
* Soovin luua [ServicePackage.cspkg](#cspkg).
* Kasutan rakenduse Visual Studio ja tahan...
    * [Looge uus pilveteenuses][vs_create]
    * [Olemasoleva pilvepõhise teenuse taaskonfigureerimine][vs_reconfigure]
    * [Projekti pilveteenuses juurutamine][vs_deploy]
    * [Kaugtöölaua üheks pilvepõhise teenuse eksemplari][remotedesktop]

<a name="csdef"></a>
## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
**ServiceDefinition.csdef** faili saate määrata sätted, mida kasutatakse Azure konfigureerida mõnda pilveteenusesse. [Azure'i teenus definitsiooni skeemi (.csdef fail)](https://msdn.microsoft.com/library/azure/ee758711.aspx) pakub teenuse määratluse faili lubatud vorming. Järgmises näites on kujutatud sätteid, mida saab määratleda veebi ja töötaja rolle:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

Saate viidata paremini mõista siin kasutatud XML-skeemi [[teenuse määratluse skeemi]], kuid siin on mõned elementide kiire selgitus.

**Saidid**  
Sisaldab veebisaitide või web rakendusi, mis on majutatud IIS7 määratlusi.

**InputEndpoints**  
Definitsioonid kasutatakse pöörduda pilveteenusesse lõpp-punktide jaoks.

**InternalEndpoints**  
Definitsioonid jaoks lõpp-punktid, mida kasutatakse rolli aknad omavahel suhelda.

**ConfigurationSettings**  
Sisaldab säte määratlused funktsioonide kindla rolliga.

**Serdid**  
Definitsioonid sertide, mis on vajalikud rollid. Koodi eelmises näites kuvatakse sert, mida kasutatakse Azure ühenduse konfiguratsiooni.

**LocalResources**  
Definitsioonid kohalikku ressursid. Kohaliku ressursi on reserveeritud kataloog, kus töötab eksemplari rolli virtuaalse masina failisüsteemis.

**Import**  
Sisaldab imporditud moodulid määratlusi. Koodi eelmises näites kuvatakse moodulid kaugtöölaua ühendus ja Azure ühendust.

**Käivitus**  
Sisaldab ülesanded, mis käivitatakse roll käivitamisel. Tööülesanded on määratletud cmd või täitmisfail.



<a name="cscfg"></a>
## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
Oma pilveteenuses sätete konfigureerimine määrab failis **ServiceConfiguration.cscfg** väärtused. Saate määrata selle faili iga rolli soovite juurutada eksemplaride arv. Teenuse konfiguratsioonifail lisatakse konfiguratsioonisätted, mis teenuse definitsioonifail määratletud väärtused. Mis tahes halduse serdid, mis on seotud pilveteenusesse jaoks thumbprints lisatakse ka faili. [Azure'i teenus konfiguratsioon skeemi (.cscfg fail)](https://msdn.microsoft.com/library/azure/ee758710.aspx) pakub lubatud vorming teenuse konfigureerimise faili.

Konfiguratsioonifail teenus ei ole pakitud rakenduse, kuid Azure eraldi failina üles laadida ja seadistada pilveteenusesse. Uue teenuse konfiguratsiooni faili saate üles laadida ilma ümbersuunamine oma pilveteenuses. Selle konfiguratsiooni väärtused pilveteenusesse saab muuta pilveteenusesse töötamise ajal. Järgmises näites on kujutatud konfiguratsioonisätted, mis saab määratleda veebi ja töötaja rolle:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

Viidata saate [Teenuse konfiguratsioon skeemi](https://msdn.microsoft.com/library/azure/ee758710.aspx) XML-skeemi siin kasutatud paremaks mõistmiseks, kuid siin on kiire selgitus elemente:

**Eksemplari**  
Konfigureerib töötavate roll eksemplaride arvu. Oma pilveteenuses takistada potentsiaalselt muutub kättesaamatuks uuendamine, on soovitada juurutada mitu eksemplari oma web suunatud rollid. Seda tehes on järgida [Azure'i arvutada teenindustaseme leping (SLA)](http://azure.microsoft.com/support/legal/sla/), mis tagab 99,95% välise ühenduvuse Interneti-ühendusega rollide kahe või enama rolli aknad teenuse juurutamisel toodud juhised.

**ConfigurationSettings**  
Konfigureerib töötavad rakenduse rolli sätteid. Nimi on `<Setting>` elementide peab vastama säte määratlused failis teenuse määratlus.

**Serdid**  
Konfigureerib serdid, mida kasutatakse teenuse. Koodi eelmises näites kujutatakse serdi RemoteAccess mooduli määratlemine. *Sõrmejälje* atribuudi väärtuseks olema seatud serdi sõrmejälje kasutada.

<p/>

 >[AZURE.NOTE] Serdi sõrmejälje saab lisada konfiguratsiooni faili, kasutades tekstiredaktorit või väärtuse saab lisada rolli Visual Studio lehe **Atribuudid** vahekaardi **serdid** .



## <a name="defining-ports-for-role-instances"></a>Pordid rolli eksemplaride määratlemine
Azure'i võimaldab ainult üks sisendpunkti web rolli. See tähendab, et kõik liiklus toimub kaudu IP-aadress. Saate konfigureerida teie veebisaiti jagada pordi hosti päise taotluse õigesse suunamiseks konfigureerimisega. Samuti saate konfigureerida rakenduste kuulata tuntud pordid IP-aadress.

Järgmises näites kuvatakse web rolli veebisaidi ja veebi rakenduse konfigureerimine. Veebisaidi on konfigureeritud kirje vaikeasukohaks pordi 80 ja veebirakenduste on konfigureeritud Saada kutsed alternatiivset hosti päis, mida nimetatakse "mail.mysite.cloudapp.net".

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" <mark>port="80"</mark> />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" <mark>hostheader="mail.mysite.cloudapp.net"</mark> />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-the-configuration-of-a-role"></a>Konfiguratsiooni rolli muutmine
Saate värskendada oma pilveteenuses konfiguratsiooni töötamise ajal Azure, võtmata teenuse ühenduseta. Konfiguratsiooni teabe muutmiseks saate kas üles uus konfiguratsioonifail või redigeerida konfiguratsioonifail kohas ja rakendada selle oma töötab teenus. Teenuse konfiguratsiooni saab teha järgmisi muudatusi:

- **Muutuvad väärtused sätete konfigureerimine**  
Kui te102750817 konfiguratsiooni rolli eksemplari saate valida muudatuse rakendamiseks ajal eksemplari on võrguühendus või prügikastis eksemplari nõtkelt ja muuta ajal eksemplari on ühenduseta.

- **Teenuse topoloogia rolli aknad muutmine**  
Topoloogia muudatused ei mõjuta käivitatud eksemplari, välja arvatud juhul, kui näiteks eemaldatakse. Kõik ülejäänud esinemisjuhud tavaliselt vaja taaskasutada; Siiski saate prügikastis topoloogia muutmise rolli aknad.

- **Serdi sõrmejälje muutmine**  
Saate värskendada ainult serdi kui rolli eksemplari on ühenduseta. Kui sert on lisatud, kustutatud või muuta, kui rolli eksemplari on veebis, Azure'i nõtkelt võtab eksemplari ühenduseta värskendamiseks serti ja tuua tagasi võrgus, kui muudatus on lõpule jõudnud.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Käsitsemise konfiguratsioonimuudatuste ja teenuse käitusaja sündmused
[Azure'i käitusaja teek](https://msdn.microsoft.com/library/azure/mt419365.aspx) sisaldab [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.aspx) nimeruumi, mis pakub tunnid Azure keskkonna rolli eksemplari koodi suhtlemiseks. Klassi [RoleEnvironment](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) määratleb järgmised sündmused, mis on esitatud enne ja pärast konfiguratsiooni muuta:

- **Sündmuse [muutmine](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changing.aspx)**  
See juhtub enne määratud eksemplari rolli annab teile võimalus rolli aknad ette võtta, kui see on nõutav on rakendatud konfiguratsiooni muuta.
- **Sündmuse [muutunud](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.serviceruntime.roleenvironment.changed.aspx)**  
Toimub pärast konfiguratsiooni muuta rakendatakse määratud eksemplari rollid.

> [AZURE.NOTE] Kuna serdi muudatused alati võrguühenduseta rolli eksemplari, nad ei tõsta RoleEnvironment.Changing või RoleEnvironment.Changed sündmused.

<a name="cspkg"></a>
## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
Pilveteenus Azure taotluse juurutamiseks peate esmalt paketti rakenduse sobivas vormingus. **CSPack** käsurea tööriist (paigaldatud [Azure SDK](https://azure.microsoft.com/downloads/)) abil saate luua paketifaili Visual Studio alternatiivina.

**CSPack** kasutab teenuse definitsioonifail ja teenuse konfiguratsiooni faili sisu määratlemiseks paketi sisu. **CSPack** genereeritud taotluse pakettfaili (.cspkg), saate üles laadida Azure'i [Azure portaali](cloud-services-how-to-create-deploy-portal.md#create-and-deploy)kaudu. Paketi nimi on vaikimisi `[ServiceDefinitionFileName].cspkg`, kuid abil saate määrata mõni muu nimi on `/out` **CSPack**võimalus.

**CSPack** asub tavaliselt  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

>[AZURE.NOTE]
CSPack.exe (Windowsis) on saadaval, käivitades **Microsoft Azure'i Käsuviip** otseteed SDK installitud.  
>  
>Käivitage programm CSPack.exe ise dokumentatsiooni kohta kõigi võimalike parameetrid ja käskude kuvamiseks.

<p />

>[AZURE.TIP]
Käivitage oma pilveteenuses kohalikult **Microsoft Azure'i arvutada emulaator**, see valik kopeerib binaarsed failid rakenduse kataloogi paigutuse, millest ta saab käitada Arvuta emulaator **/copyonly** suvandi.

### <a name="example-command-to-package-a-cloud-service"></a>Näiteks käsu paketi pilveteenus
Järgmises näites luuakse rakendusepaketi, mis sisaldab teavet web rolli. Käsu määrab teenuse definitsioonifail kasutama, kust võib leida binaarsed failid, kataloogi ja paketi faili nimi.

    cspack [DirectoryName]\[ServiceDefinition]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /out:[OutputFileName]

Kui rakendus sisaldab nii veebi rolli töötaja roll, kui kasutatakse järgmine käsk:

    cspack [DirectoryName]\[ServiceDefinition]
           /out:[OutputFileName]
           /role:[RoleName];[RoleBinariesDirectory]
           /sites:[RoleName];[VirtualPath];[PhysicalPath]
           /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]

Kui muutujad on määratletud järgmiselt:

| Muutuja                  | Väärtus |
| ------------------------- | ----- |
| \[DirectoryName\]         | Alamkausta, Azure'i projekti .csdef faili sisaldava juurkaust projekti alusel.|
| \[ServiceDefinition\]     | Teenuse määratluse faili nimi. Vaikimisi on see fail nimega ServiceDefinition.csdef.  |
| \[OutputFileName\]        | Loodud paketi faili nimi. Tavaliselt on see määratud rakenduse nimi. Kui faili nime pole määratud, luuakse rakenduse pakett \[ApplicationName\].cspkg.|
| \[RoleName\]              | Roll teenuse definitsioonifail määratletud nimi.|
| \[RoleBinariesDirectory] | Binaarsed failid rolli asukoht.|
| \[VirtualPath\]           | Füüsilise kataloogide iga virtuaalse tee määratletud saitide osas teenuse määratlus.|
| \[PhysicalPath\]          | Füüsilise kataloogide iga virtuaalse tee määratletud teenuse määratluse sõlme saidi sisu.|
| \[RoleAssemblyName\]      | Kahendfailina rolli nimi.| 


## <a name="next-steps"></a>Järgmised sammud

Pilveteenuse pakett luua ja tahan...

* [Kaugtöölaud pilv teenuse eksemplari häälestamine][remotedesktop]
* [Projekti pilveteenuses juurutamine][deploy]

Kasutan rakenduse Visual Studio ja tahan...

* [Looge uus pilveteenuses][vs_create]
* [Olemasoleva pilvepõhise teenuse taaskonfigureerimine][vs_reconfigure]
* [Projekti pilveteenuses juurutamine][vs_deploy]
* [Kaugtöölaud pilv teenuse eksemplari häälestamine][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop.md
[vs_remote]: ../vs-azure-tools-remote-desktop-roles.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md
