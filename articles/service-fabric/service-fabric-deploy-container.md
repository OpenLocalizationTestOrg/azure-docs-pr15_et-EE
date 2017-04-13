<properties
   pageTitle="Teenuse struktuuri ja juurutamisel ümbriste | Microsoft Azure'i"
   description="Teenuse struktuuri ja ümbriste kasutamine juurutada microservice rakendused. Selles artiklis kirjeldatakse võimalusi, mis pakub teenuse struktuuri ümbriste ja juurutamise klaster rakendusse Windows container pilt"
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
   ms.date="10/24/2016"
   ms.author="msfussell"/>

# <a name="preview-deploy-a-windows-container-to-service-fabric"></a>Eelvaade: Juurutada Windowsi container teenuse struktuuri

> [AZURE.SELECTOR]
- [Windowsi Container juurutamine](service-fabric-deploy-container.md)
- [Keskmise suurusega Container juurutamine](service-fabric-deploy-container-linux.md)

Selles artiklis tutvustatakse koostamise Windows ümbriste konteinerite teenused. 

>[AZURE.NOTE] See funktsioon on eelvaates Linux ja Windows Server 2016 jaoks pole praegu saadaval. See on saadaval teenuse struktuuri järgmises versioonis Windows Server 2016 eelvaade ja toetatud järgmise väljaande pärast seda.

Teenuse struktuuri on mitu container võimalused, mis aitavad teil rakenduste mis koosnevad microservices, mis on konteiner loomine. Neid nimetatakse konteinerite teenused. 

Võimaluste hulka;

- Container pilt juurutus- ja aktiveerimine
- Ressursside juhtimine
- Hoidla autentimine
- Container port host port kaardistamine
- Container-container discovery ja suhtlus
- Võimalus määrata keskkonna muutujate ja konfigureerimiseks

Võimaldab vaadata iga võimaluste omakorda kui pakendamine konteinerite teenuse rakenduse tuleb kaasata.

## <a name="packaging-a-windows-container"></a>Windowsi container pakendamine

Kui pakendamine ümbris, saate valida, kas kasutada Visual Studio projekti malli või [luua rakenduse pakett käsitsi](#manually). Visual Studio kasutamisel rakenduse paketi struktuuri ja manifestifailidele on loodud uue projekti viisard teile (see on peagi järgmises versioonis).

## <a name="using-visual-studio-to-package-an-existing-container-image"></a>Olemasoleva pildi container paketti Visual Studio abil

>[AZURE.NOTE] Tulevaste väljalasete instrumentaarium SDK Visual Studio, on võimalik lisada rakenduse ümbris executable külaline täna lisamine sarnaselt. Vaadake teemat [Deploy külalisena käivitatava teenuse struktuuri](service-fabric-deploy-existing-app.md) . Praegu tuleb teha käsitsi pakendit, nagu allpool kirjeldatud.

<a id="manually"></a>
## <a name="manually-packaging-and-deploying-a-container"></a>Käsitsi pakendamine ja ümbris juurutamine
Protsessi käsitsi pakendamine konteinerite teenus põhineb järgmist:

1. Avaldatud on ümbriste oma andmebaasi.
2. Looge paketi kataloogi struktuuri.
3. Redigeeri teenuse Avaldamisfail.
4. Rakenduse Avaldamisfail redigeerida.

## <a name="container-image-deployment-and-activation"></a>Container pilt juurutus- ja aktiveerimise.
Ümbris tähistab teenuse struktuuri [mudel](service-fabric-application-model.md)rakenduse host, mis kus paigutatakse mitme teenuse koopiad. Juurutada ja ümbris aktiveerida, sellele nimi container pildi sisse on `ContainerHost` teenuse manifestis element.

Teenuse manifestis lisamine on `ContainerHost` kirje osutage ja seadke soovitud `ImageName` container hoidla ja pildi nimi. Järgmised osaline manifest kujutab näidet nimega *myimage:v1* nimega *myrepo* hoidla ümbris juurutamine

    <CodePackage Name="Code" Version="1.0">
        <EntryPoint>
          <ContainerHost>
            <ImageName>myrepo/myimagename:v1</ImageName>
            <Commands></Commands>
          </ContainerHost>
        </EntryPoint>
    </CodePackage>

Saate sisestada Sisestuskeel käskude container pilt, määrates valikuline `Commands` elemendi komaga eraldatud käsud käivitamiseks ümbrises. 

## <a name="resource-governance"></a>Ressursside juhtimine
Ressursside juhtimine on võimalus ümbrisest ja piirab hosti ümbris kasutavate ressursid. Funktsiooni `ResourceGovernancePolicy`, määratud Rakendusmanifest, pakub võimalust deklareerida teenuse kood paketi ressursi piirangud. Saate määrata ressursi piirangud;

- Mälu
- MemorySwap
- CpuShares (CPU suhteline kaal)
- MemoryReservationInMB  
- BlkioWeight (BlockIO suhteline kaal). 

>[AZURE.NOTE] Tulevaste väljalasete, teatud tekstiplokki IO piiride IOPs nt kasutajatugi, lugemis-ja kirjutamisõigusega BPS ja teised on võimalik.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ResourceGovernancePolicy CodePackageRef="FrontendService.Code" CpuShares="500" 
            MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
        </Policies>
    </ServiceManifestImport>


## <a name="repository-authentication"></a>Hoidla autentimine
Allalaadimiseks ümbris tuleb sisestada sisselogimisteave container hoidlasse. Sisselogimisteave, määratud *Rakendusmanifest* kasutatakse sisselogimisteave või SSH võti container pildi allalaadimiseks pilt hoidla määramiseks.  Järgmises näites on kujutatud konto nimega *TestUser* koos parool avatekstina. See **on soovitatav** .


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="12345" PasswordEncrypted="false"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Parooli saate ja peaksid krüptitud juurutatud masina sert.

Järgmises näites on kujutatud konto nimega *TestUser* parooliga krüptitud ehk *MyCert*sert. Saate kasutada funktsiooni `Invoke-ServiceFabricEncryptText` PowerShelli käsu luua salajane salakiri teksti parool. Lugege artiklit [saladusi teenuse struktuuri rakenduste haldamine](service-fabric-application-secret-management.md) , üksikasjalikku teavet. Serdi parooli dekrüptida privaatvõti tuleb kasutusele võtta kohalikus arvutis on riba-, meetodi (see on ARM kaudu Azure) sisse. Siis, kui teenuse struktuuri juurutamine teenuse paketi masina, on võimalik dekrüptida salajane ja konto nimi koos autentida container hoidla, kasutades neid mandaate.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <RepositoryCredentials AccountName="TestUser" Password="[Put encrypted password here using MyCert certificate ]" PasswordEncrypted="true"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

## <a name="container-port-to-host-port-mapping"></a>Container port host port kaardistamine
Saate konfigureerida, määrates ümbris suhtlemiseks kasutada host pordi lisamine `PortBinding` rakenduse manifestis. Pordi sidumine kaardid port teenus on kuulamise ümbrises hosti porti.


    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>


## <a name="container-to-container-discovery-and-communication"></a>Container-container discovery ja suhtlus
Abil soovitud `PortBinding` poliitika saate vastendada container portide, et mõne `Endpoint` teenuse manifestis, nagu on näidatud järgmises näites. Lõpp-punkti `Endpoint1` saate määrata fikseeritud port, nt pordi 80 või määrake porti pole üldse, sellisel juhul juhusliku port rühmad kasutajate rakenduse portide vahemiku valitakse teie eest.

Jaoks Külastajate ümbriste, milles on `Endpoint` nagu see teenus manifestis võimaldab teenuse struktuuri, et muud teenused töötavad klaster saate tuvastada selle probleemi lahendada teenuse ülejäänud päringute kasutamine container avaldada selle lõpp-punkti automaatselt nimetamise teenusega. 

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <Policies>
            <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
            </ContainerHostPolicies>
        </Policies>
    </ServiceManifestImport>

Sisestades nimetamise teenuse, saate teha hõlpsalt container-container side kood sees oma container [tagant puhverserveri](service-fabric-reverseproxy.md). Kõik, peate tegema on pööratud puhverserveri http kuulamise pordi ja teenused, mida soovite suhelda, seades neid keskkonna muutujate nimi. Juhised selle tegemiseks leiate järgmisest jaotisest.  

## <a name="configure-and-set-environment-variables"></a>Konfigureerimine ja määrata keskkonna muutujad
Keskkonna muutujate võib olla määratud vaenlane iga teenuse kood paketi näidata nii juurutatud ümbriste või protsesside/Külastajate täitmisfailid teenuste jaoks. Need keskkonna muutujaga saate alistada, eriti Rakendusmanifest või määratud juurutamisel nimega rakenduse parameetrid.

Järgmised teenuse manifest XML-i koodilõigu kujutab näidet keskkonna muutujate kood paketi määramine. 

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>a guest executable service in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
    </ServiceManifest>

Nende keskkonna muutujate saate alistada manifest tasemel:

    <ServiceManifestImport>
        <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
        <EnvironmentOverrides CodePackageRef="FrontendService.Code">
            <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvc]"/>
            <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
        </EnvironmentOverrides>
    </ServiceManifestImport>

Ülaltoodud näites me määratud konkreetsete väärtus on `HttpGateway` muutuja (19000) väärtus ajal `BackendServiceName` parameeter on seatud kaudu soovitud `[BackendSvc]` rakenduse parameeter. See võimaldab teil määrata väärtus `BackendServiceName`ajal rakenduse juurutamine väärtus ja pole manifestis konstanti. 

## <a name="complete-examples-for-application-and-service-manifest"></a>Rakenduste ja teenuste manifesti täieliku näited
Järgmises näites Rakendusmanifest, mis näitab container funktsioonid.


    <ApplicationManifest ApplicationTypeName="SimpleContainerApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description>A simple service container application</Description>
        <Parameters>
            <Parameter Name="ServiceInstanceCount" DefaultValue="3"></Parameter>
            <Parameter Name="BackEndSvcName" DefaultValue="bkend"></Parameter>
        </Parameters>
        <ServiceManifestImport>
            <ServiceManifestRef ServiceManifestName="FrontendServicePackage" ServiceManifestVersion="1.0"/>
            <EnvironmentOverrides CodePackageRef="FrontendService.Code">
                <EnvironmentVariable Name="BackendServiceName" Value="[BackendSvcName]"/>
                <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
            </EnvironmentOverrides>
            <Policies>
                <ResourceGovernancePolicy CodePackageRef="Code" CpuShares="500" MemoryInMB="1024" MemorySwapInMB="4084" MemoryReservationInMB="1024" />
                <ContainerHostPolicies CodePackageRef="FrontendService.Code">
                    <RepositoryCredentials AccountName="username" Password="****" PasswordEncrypted="true"/>
                    <PortBinding ContainerPort="8905" EndpointRef="Endpoint1"/>
                </ContainerHostPolicies>
            </Policies>
        </ServiceManifestImport>
    </ApplicationManifest>


Järgmine on näide teenuse manifesti (määratud eelnev Rakendusmanifest) kuvava container funktsioonid

    <ServiceManifest Name="FrontendServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <Description> A service that implements a stateless frontend in a container</Description>
        <ServiceTypes>
            <StatelessServiceType ServiceTypeName="StatelessFrontendService"  UseImplicitHost="true"/>
        </ServiceTypes>
        <CodePackage Name="FrontendService.Code" Version="1.0">
            <EntryPoint>
            <ContainerHost>
                <ImageName>myrepo/myimage:v1</ImageName>
                <Commands></Commands>
            </ContainerHost>
            </EntryPoint>
            <EnvironmentVariables>
                <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
                <EnvironmentVariable Name="BackendServiceName" Value=""/>
            </EnvironmentVariables>
        </CodePackage>
        <ConfigPackage Name="FrontendService.Config" Version="1.0" />
        <DataPackage Name="FrontendService.Data" Version="1.0" />
        <Resources>
            <Eendpoints>
                <Endpoint Name="Endpoint1" Port="80"  UriScheme="http" />
            </Eendpoints>
        </Resources>
    </ServiceManifest>
