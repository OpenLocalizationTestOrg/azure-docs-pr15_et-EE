<properties
   pageTitle="Teenuse struktuuri rakenduste ja teenuste turbepoliitikate | Microsoft Azure'i"
   description="Ülevaade sellest, kuidas teenuse struktuuri rakenduse käivitamiseks klõpsake jaotises süsteem ja kohalik kontod, sh SetupEntry kohani, kus nõuab teatud õigustega toimingu sooritamiseks enne selle algust"
   services="service-fabric"
   documentationCenter=".net"
   authors="msfussell"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/22/2016"
   ms.author="mfussell"/>

# <a name="configure-security-policies-for-your-application"></a>Rakenduse turbepoliitikate konfigureerimine
Azure teenuse struktuuri pakub võimalus secure rakenduste töötamine erinevate Kasutajakontod klaster. Samuti tagab teenuse struktuuri ressursside rakenduste kasutatava ajal juurutamise kasutajakontoga, nt faile, kataloogid ja serdid. See muudab rakendused, isegi ühiskasutusega majutatud keskkonnas, turvalist üksteisest. 

Vaikimisi teenuse struktuuri rakendusi käivitada konto, mille Fabric.exe protsess töötab. Teenuse struktuuri pakub võimalus käivitada rakendusi kohaliku kasutajakonto või määratud sees on rakenduse manifesti kohalik süsteem konto. Toetatud kohalik süsteem kontotüüpide puhul on **LocalUser**, **NetworkService'i**, **LocalService**ja **paremal**.

 Teenuse struktuuri käivitamisel Windows Server oma andmekeskuse autonoomse Installeri abil saate kasutada Active Directory (AD) domeenikontod.

Kasutajarühmade saate määratletud ja loodud nii, et üks või mitu kasutajat saab lisada iga rühma haldama koos. See on kasulik, kui on mitu kasutajat muu teenuse kirje punktid ja ta peab olema teatud levinud õigused, mis on saadaval töörühma tasandil.

## <a name="configure-the-policy-for-service-setupentrypoint"></a>Poliitika SetupEntryPoint teenuse konfigureerimine

Nagu on kirjeldatud [mudel](service-fabric-application-model.md) on **SetupEntryPoint** õigustega sisendpunkti, mis jookseb sama identimisteavet enne muude sisendpunkti nimega teenuse struktuuri (tavaliselt *NetworkService'i* konto). Määratud **EntryPoint** käivitatava on tavaliselt pikaajalisi majutusteenus, seega on eraldi häälestamise kirjet, osutage hoiab ära, et käivitamine käivitatava majutusteenus kõrge privileegidega pikema perioodi. Määratud **EntryPoint** käivitatava käivitatakse pärast **SetupEntryPoint** väljub edukalt. Tulemuseks protsess on jälgida ja uuesti uuesti alustades **SetupEntryPoint**, kui see kunagi lõpeb või jookseb.

Järgnevalt on lihtsa teenuse manifest näide, mis näitab selle SetupEntryPoint ja peamised EntryPoint teenuse.

~~~
<?xml version="1.0" encoding="utf-8" ?>
<ServiceManifest Name="MyServiceManifest" Version="SvcManifestVersion1" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyServiceType" />
  </ServiceTypes>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyServiceHost.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>
  <ConfigPackage Name="Config" Version="1.0.0" />
</ServiceManifest>
~~~

### <a name="configure-the-policy-using-a-local-account"></a>Poliitika kohaliku konto konfigureerimine

Kui olete teenuse olema häälestamise kirjet, osutage, saate muuta turvalisus õigused, mis see töötab jaotises rakenduse manifesti. Teiste näites on kujutatud konfigureerimise teenuse käivituma kasutaja konto administraatoriõigused.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupAdminUser">
            <MemberOf>
               <SystemGroup Name="Administrators" />
            </MemberOf>
         </User>
      </Users>
   </Principals>
</ApplicationManifest>
~~~

Esmalt Looge **põhisumma** jaotis kasutaja nimi, näiteks SetupAdminUser. See näitab, et kasutaja on süsteemi administraatorite rühma liige.

Järgmiseks jaotises **ServiceManifestImport** konfigureerimine poliitika see peamine rakendamiseks **SetupEntryPoint**. See ütleb teenuse struktuuri, et **MySetup.bat** faili käitamisel peaks olema RunAs administraatoriõigustega. Et teil *pole* rakendatud poliitika peamine koht, **MyServiceHost.exe** koodi jookseb **NetworkService'i** konto kaudu. See on vaikimisi konto, mis käivitatakse kõik teenuse kirje punkte nimega.

Vaatame nüüd lisage faili MySetup.bat Visual Studio projekti testimiseks administraatori õigusi. Visual Studio, Paremklõpsake teenuse project ja lisage uus fail nimega MySetup.bat.

Järgmiseks tagada, et MySetup.bat faili teenuse paketti. Vaikimisi ei ole. Valige fail, paremklõpsake kontekstimenüü saada ja valige **Atribuudid**. Veenduge dialoogiboksis Atribuudid **väljundi kataloogi** on määratud **kopeerimine kui uuemat versiooni**. Vt järgmine pilt.

![Visual Studio CopyToOutput jaoks SetupEntryPoint pakkfail][image1]

Nüüd avage fail MySetup.bat ja lisage järgmised käsud:

~~~
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable "MyValue"
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
~~~

Järgmiseks Koostage ja Juurutage lahendus kohaliku arengu kobar.  Kui teenus on hakanud, nagu on näidatud teenuse struktuuri Exploreris, näete selle MySetup.bat õnnestus kahel viisil. Avage käsuviip PowerShell ja tippige.

~~~
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
~~~

Märkus siis, kui teenus on juurutatud ja alustada teenuse struktuuri Exploreris, näiteks sõlme 2 sõlme nimi. Järgmiseks avage rakenduse eksemplari töö kausta out.txt fail, mis näitab **TestVariable**väärtuse leidmiseks. Näiteks kui see teenus on juurutatud sõlm 2, siis saate selle tee minna **MyApplicationType**jaoks:

~~~
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
~~~

###  <a name="configure-the-policy-using-local-system-accounts"></a>Poliitika kohalik süsteem kontode konfigureerimine
Sageli on soovitatav käivitus skripti abil administraatorid konto, nagu on näidatud eelneva asemel kohalik süsteem konto. Töötab soovitud RunAs poliitika administraatorid kujul tavaliselt ei tööta hästi, sest masinad on kasutajate juurdepääs kontroll (UAC) vaikimisi sisse lülitatud. Sellisel juhul **soovitus on paremal olevat SetupEntryPoint käivituma asemel kohalik kasutaja lisatakse administraatorite rühma**. Järgmises näites on kujutatud seada käivituma paremal SetupEntryPoint.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MyServiceTypePkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
   </ServiceManifestImport>
   <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
~~~

##  <a name="launch-powershell-commands-from-a-setupentrypoint"></a>Käivitage PowerShelli käskude valimiskoht on SetupEntryPoint
PowerShelli käivitamiseks **SetupEntryPoint** punktist käivitada **PowerShell.exe** paketi fail, mis viitab PowerShelli faili. Esmalt lisage PowerShelli faili teenuse projekt, nt **MySetup.ps1**. Pidage meeles, et fail on ka teenuse paketi *kopeerimine kui uuem* atribuudi seadmiseks. Järgmises näites on kujutatud paketi näidisfaili käivitage PowerShelli fail nimega MySetup.ps1, mis määrab süsteemi keskkonna muutuja nimega **TestVariable**.


MySetup.bat PowerShelli faili käivitada.

~~~
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
~~~

PowerShelli faili, lisage soovitud muutuja seadmiseks järgmine tekst:

~~~
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
~~~

**Märkus:** Vaikimisi pakkfail käitamisel tundub veebisaidil nimega **töö** failide kausta. Sel juhul, kui MySetup.bat töötab soovime see leida soovitud MySetup.ps1 samasse kausta, mis on rakenduse **koodi paketi** kausta. Selle kausta muutmiseks Määrake töökausta järgmist.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
~~~

## <a name="using-console-redirection-for-local-debugging"></a>Konsooli ümbersuunamise kasutamise kohaliku silumine
Aeg-ajalt on kasulik näha konsooli väljund skripti silumine eesmärgil. Selleks saate konsooli ümbersuunamine poliitika, mis kirjutab väljund faili. Faili väljund kirjutatakse kausta nimega **log** sõlme, kus rakendus on juurutatud ja käivitada (vaadake teemat kus Otsitav kasutamine eelnevas näites).

**Märkus: kunagi** konsooli ümbersuunamine poliitika juurutatud valmistamisel, kuna see võib mõjutada rakenduse Tõrkesiirde rakenduse kasutamine. **Ainult** kasutage seda kohaliku arengu ja silumine eesmärgil.  

Järgmises näites on kujutatud säte konsooli ümbersuunamise FileRetentionCount väärtusega.

~~~
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
~~~

Kui muudate nüüd MySetup.ps1 fail käsu käivitava **kaja** kirjutamiseks, see kirjutab väljundfail silumine eesmärgil.

~~~
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
~~~

**Kui teil on oma Skripti silumisel, kohe eemaldada see konsooli ümbersuunamine poliitika**

## <a name="configure-policy-for-service-code-packages"></a>Poliitika teenuse kood pakette konfigureerimine 
Eelmist toimingut, teile kuvati SetupEntryPoint RunAs poliitika rakendamise kohta. Vaatame lähemalt pisut põhjalikult sellest, kuidas luua eri põhisumma, mida saab rakendada nii teenuse poliitikate.

### <a name="create-local-user-groups"></a>Kohalik kasutaja rühmade loomine
Kasutajarühmade saate määratletud ja mis loodud ühe või mitme kasutajad saaksid rühma lisada. See on eriti kasulik, kui on mitu kasutajat muu teenuse kirje punktid ja ta peab olema teatud levinud õigused, mis on saadaval töörühma tasandil. Järgmises näites on kujutatud nimetatakse **LocalAdminGroup** administraatoriõigustega kohalikku rühma. Kaks kasutajat, Customer1 ja Customer2, on valmistatud kohaliku rühma liikmed.

~~~
<Principals>
 <Groups>
   <Group Name="LocalAdminGroup">
     <Membership>
       <SystemGroup Name="Administrators"/>
     </Membership>
   </Group>
 </Groups>
  <Users>
     <User Name="Customer1">
        <MemberOf>
           <Group NameRef="LocalAdminGroup" />
        </MemberOf>
     </User>
    <User Name="Customer2">
      <MemberOf>
        <Group NameRef="LocalAdminGroup" />
      </MemberOf>
    </User>
  </Users>
</Principals>
~~~

### <a name="create-local-users"></a>Kohaliku kasutajate loomiseks
Saate luua kohalik kasutaja, mida saab kasutada rakendusest teenus secure. Kui rakenduse manifesti põhisumma osas on määratud **LocalUser** konto tüüp, loob teenuse struktuuri kohalikke kasutajakontosid arvutites, kus on juurutatud rakendus. Vaikimisi on need kontod sama nimed, märgitud Rakendusmanifest (näiteks järgmised valimis Customer3). Selle asemel dünaamiliselt ja juhusliku paroole.

~~~
<Principals>
  <Users>
     <User Name="Customer3" AccountType="LocalUser" />
  </Users>
</Principals>
~~~

<!-- If an application requires that the user account and password be same on all machines (for example, to enable NTLM authentication), the cluster manifest must set NTLMAuthenticationEnabled to true. The cluster manifest must also specify an NTLMAuthenticationPasswordSecret that will be used to generate the same password across all machines.

<Section Name="Hosting">
      <Parameter Name="EndpointProviderEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationEnabled" Value="true"/>
      <Parameter Name="NTLMAuthenticationPassworkSecret" Value="******" IsEncrypted="true"/>
 </Section>
-->

### <a name="assign-policies-to-the-service-code-packages"></a>Teenuse kood pakettide poliitika määramine
Mõne **ServiceManifestImport** **RunAsPolicy** jaotises saate määrata konto põhisumma jaotisest, mida tuleks kasutada koodi paketi käivitamine. See seostab ka koodi pakettide teenuse manifestist põhisumma jaotises Kasutajakontod. Saate määrata selle häälestamine või peamised või määrata `All` rakendamiseks mõlemad. Järgmises näites on kujutatud erinevad reeglid ei rakendata:

~~~
<Policies>
<RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup"/>
<RunAsPolicy CodePackageRef="Code" UserRef="Customer3" EntryPointType="Main"/>
</Policies>
~~~

Kui **EntryPointType** pole määratud, kuvatakse vaikimisi on EntryPointType = "Main". Täpsustatakse **SetupEntryPoint** on eriti kasulik, kui soovite käivitada teatud kõrgete õigustega häälestamise toiming süsteemi kasutajakontot. Tegelik teenuse kood saab käitada jaotises alumises-õigustega kontot.

### <a name="apply-a-default-policy-to-all-service-code-packages"></a>Teenuse kood pakettide vaikepoliitika rakendamine
Jaotise **DefaultRunAsPolicy** abil saate määrata vaikimisi kasutajakonto kõik kood paketid, mis pole teatud **RunAsPolicy** määratletud. Enamik koodi määratud teenuse manifestis kasutamisest pakettide peate käivitama samadel kasutajat, kui rakendus lihtsalt saate määratleda RunAs vaikepoliitika kasutaja kontoga. Järgmises näites saate määrata, et kui paketi kood ei ole **RunAsPolicy** määratud, koodi paketi peaks töötama jaotises **MyDefaultAccount** põhisumma jaotises määratud.

~~~
<Policies>
  <DefaultRunAsPolicy UserRef="MyDefaultAccount"/>
</Policies>
~~~
### <a name="using-an-active-directory-domain-group-or-user"></a>Active Directory domeeni rühmale või kasutajale
Windows Server autonoomse Installeri abil installitud teenuse struktuuri, käivitage identimisteabe teenuse Active Directory (AD) kasutaja või rühma konto. Märkus: See on Active Directory kohapealne teie domeeni kasutavatele ja pole koos Azure Active Directory (AAD). Domeeni kasutaja või rühma abil pääsete juurde siis muid ressursse (nt failiketastel) domeen, mis on vastavad õigused.

Järgmises näites on kujutatud AD kasutaja nimega *TestUser* oma domeeni parooliga krüptitud ehk *MyCert*sert. Saate kasutada funktsiooni `Invoke-ServiceFabricEncryptText` PowerShelli käsu luua salajane salakiri teksti. Lugege artiklit [saladusi teenuse struktuuri rakenduste haldamine](service-fabric-application-secret-management.md) , üksikasjalikku teavet. Serdi parooli dekrüptida privaatvõti tuleb kasutusele võtta kohalikus arvutis on riba-, meetodi (see on ressursihaldur kaudu Azure) sisse. Siis, kui teenuse struktuuri juurutamine teenuse paketi masina, on võimalik dekrüptida salajane ja koos kasutajanimi, autentida AD käivituma mandaadi.

~~~
<Principals>
  <Users>
    <User Name="TestUser" AccountType="DomainUser" AccountName="Domain\User" Password="[Put encrypted password here using MyCert certificate]" PasswordEncrypted="true" />
  </Users>
</Principals>
<Policies>
  <DefaultRunAsPolicy UserRef="TestUser" />
  <SecurityAccessPolicies>
    <SecurityAccessPolicy ResourceRef="MyCert" PrincipalRef="TestUser" GrantRights="Full" ResourceType="Certificate" />
  </SecurityAccessPolicies>
</Policies>
<Certificates>
~~~


## <a name="assign-securityaccesspolicy-for-http-and-https-endpoints"></a>HTTP ja HTTPS-i lõpp-punktide jaoks SecurityAccessPolicy määramine
Kui rakendate RunAs poliitika teenuse ja teenuse manifesti kinnitab lõpp-punkti ressursse HTTP-protokolli, peate määrama **SecurityAccessPolicy** tagada pordid eraldatud nende lõpp-punktid õigesti pääsuõiguste loetletud RunAs kasutajakonto, mis töötab teenus. Muul juhul **http.sys** ei ole juurdepääs teenusele ja tõrkeid kõnede toomine kliendi. Teiste näide kehtib Customer3 konto lõpp nimetatakse **ServiceEndpointName**, annab see täielikud juurdepääsuõigused.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
   <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
</Policies>
~~~

HTTPS-i lõpp-punkti teil ka serdi naasmiseks kliendi nimi. Selleks **EndpointBindingPolicy**, kasutades määratletud serdid jaotist rakenduse manifesti sertifikaadiga.

~~~
<Policies>
   <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
  <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
   <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
  <!--EndpointBindingPolicy is needed if the EndpointName is secured with https -->
  <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
</Policies
~~~


## <a name="a-complete-application-manifest-example"></a>Täieliku taotluse manifest näide
Järgmised Rakendusmanifest näitab paljude erinevate sätted.

~~~
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="Application3Type" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Parameters>
      <Parameter Name="Stateless1_InstanceCount" DefaultValue="-1" />
   </Parameters>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
      <ConfigOverrides />
      <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="Customer1" />
         <RunAsPolicy CodePackageRef="Code" UserRef="LocalAdmin" EntryPointType="Setup" />
        <!--SecurityAccessPolicy is needed if RunAsPolicy is defined and the Endpoint is http -->
         <SecurityAccessPolicy ResourceRef="EndpointName" PrincipalRef="Customer1" />
        <!--EndpointBindingPolicy is needed the EndpointName is secured with https -->
        <EndpointBindingPolicy EndpointRef="EndpointName" CertificateRef="Cert1" />
     </Policies>
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="Stateless1">
         <StatelessService ServiceTypeName="Stateless1Type" InstanceCount="[Stateless1_InstanceCount]">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
   <Principals>
      <Groups>
         <Group Name="LocalAdminGroup">
            <Membership>
               <SystemGroup Name="Administrators" />
            </Membership>
         </Group>
      </Groups>
      <Users>
         <User Name="LocalAdmin">
            <MemberOf>
               <Group NameRef="LocalAdminGroup" />
            </MemberOf>
         </User>
         <!--Customer1 below create a local account that this service runs under -->
         <User Name="Customer1" />
         <User Name="MyDefaultAccount" AccountType="NetworkService" />
      </Users>
   </Principals>
   <Policies>
      <DefaultRunAsPolicy UserRef="LocalAdmin" />
   </Policies>
   <Certificates>
     <EndpointCertificate Name="Cert1" X509FindValue="FF EE E0 TT JJ DD JJ EE EE XX 23 4T 66 "/>
  </Certificates>
</ApplicationManifest>
~~~


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Järgmised sammud

* [Rakenduse mudel mõistmine](service-fabric-application-model.md)
* [Määrake teenuse manifestis ressursid](service-fabric-service-manifest-resources.md)
* [Rakenduse juurutamine](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
