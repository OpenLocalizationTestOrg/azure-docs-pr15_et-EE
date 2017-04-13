<properties 
pageTitle="Kaugtöölaua ühendus rollist Azure pilveteenustega lubamine" 
description="Kuidas konfigureerida azure pilveteenuse teenuserakenduse lubama kaugtöölaua ühendused" 
services="cloud-services" 
documentationCenter="" 
authors="sbtron" 
manager="timlt" 
editor=""/>
<tags 
ms.service="cloud-services" 
ms.workload="tbd" 
ms.tgt_pltfrm="na" 
ms.devlang="na" 
ms.topic="article" 
ms.date="02/17/2016" 
ms.author="saurabh"/>

# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Kaugtöölaua ühendus rollist Azure pilveteenustega lubamine

>[AZURE.SELECTOR]
- [Azure'i klassikaline portaal](cloud-services-role-enable-remote-desktop.md)
- [PowerShelli](cloud-services-role-enable-remote-desktop-powershell.md)
- [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)


Kaugtöölaua võimaldab teil juurde pääseda töölaua töötab Azure rolli. Kaugtöölaua ühendus abil saate diagnoosida probleeme rakenduse töötamise ajal ja tõrkeotsing. 

Saate lubada kaugtöölaua ühendus oma rolli arendamise käigus, sh kaugtöölaua moodulid teenuse definitsiooni või soovi korral saate lubada Remote'i töölaua laiendamise Kaugtöölaud. Eelistatud lähenemine on kasutada kaugtöölaua laiend, saate lubada kaugtöölaua isegi juhul, kui rakendus on juurutatud ilma ümberkorraldamine rakenduse. 


## <a name="configure-remote-desktop-from-the-azure-classic-portal"></a>Kaugtöölaua Azure klassikaline portaalist konfigureerimine
Azure'i klassikaline portaali kasutab Remote'i töölaua laiend lähenemine, seega saate lubada kaugtöölaua isegi juhul, kui rakendus on juurutatud. Oma pilveteenuses **konfigureerimine** leht võimaldab teil Luba kaugtöölaud, muuta selle virtuaalmasinates ühendamiseks kohaliku administraatorikonto, serdi kasutatakse autentimise ja määrata aegumiskuupäeva. 


1. Klõpsake **Pilveteenustega**, klõpsake selle nime pilveteenusesse, ja klõpsake nuppu **Konfigureeri**.

2. Klõpsake nuppu **Remote**.
    
    ![Remote pilveteenustega](./media/cloud-services-role-enable-remote-desktop/CloudServices_Remote.png)
    
    > [AZURE.WARNING] Kõik rolli aknad taaskäivitatakse, kui te esmalt luba Kaugtöölaud ja klõpsake nuppu OK (märge). Selleks, et uuesti, peab olema installitud parooli krüptimiseks kasutatavat serti rolli. Vältida uuesti, [laadige üles sert jaoks pilveteenusesse](cloud-services-how-to-create-deploy/#how-to-upload-a-certificate-for-a-cloud-service) ja naaske selle dialoogiboksi.
    

3. **Rollid**, valige roll, mida soovite värskendada või valige **Kõik** kõigi rollide.

4. Tehke järgmisi muudatusi:
    
    - Kaugtöölaua lubamiseks märkige ruut **Luba kaugtöölaud** . Kaugtöölaua keelamiseks tühjendage ruut.
    
    - Looge konto kasutamine kaugtöölaua ühendusi rolli aknad.
    
    - Värskendage olemasolev konto parooli.
    
    - Valige on üles laaditud serdi autentimine (Laadi üles abil **serdid** lehel **üles** sert) kasutada või luua uut serti. 
    
    - Saate muuta kaugtöölaua konfiguratsiooni aegumiskuupäeva.

5. Kui olete lõpetanud oma konfiguratsioon värskendusi, klõpsake nuppu **OK** (märge).


## <a name="remote-into-role-instances"></a>Remote üheks rolli aknad
Kaugtöölaua on sisse lülitatud rollid saate remote rolli eksemplari erinevate tööriistade abil sisse.

Ühenduse rolli eksemplari Azure klassikaline portaalist:
    
  1.   Klõpsake **eksemplarid** **eksemplaride** lehe avamiseks.
  2.   Valige roll eksemplari, mis on konfigureeritud Kaugtöölaud.
  3.   Klõpsake nuppu **Ühenda**ja järgige juhiseid, et avada töölaua. 
  4.   Klõpsake nuppu **Ava** ja seejärel **Connect** alustamiseks kaugtöölaua ühendus. 


### <a name="use-visual-studio-to-remote-into-a-role-instance"></a>Kasutage Visual Studio kaugtöölaua rolli eksemplari

Visual Studio Server Explorer:

1. Laiendage soovitud **Azure'i\\pilveteenustega\\[pilvepõhise teenuse nimi]** sõlm.
2. **Kas **lavastus** - või**laiendamine
3. Laiendage üksikuid roll.
4. Paremklõpsake ühte rolli aknad, klõpsake nuppu **Loo ühendus abil kaugtöölaua...**ja siis sisestage kasutajanimi ja parool. 

![Server explorer Kaugtöölaud.](./media/cloud-services-role-enable-remote-desktop/ServerExplorer_RemoteDesktop.png)


### <a name="use-powershell-to-get-the-rdp-file"></a>PowerShelli kasutamine saada RDP-faili
[Get-AzureRemoteDesktopFile](https://msdn.microsoft.com/library/azure/dn495261.aspx) cmdlet-käsu abil saate tuua RDP-faili. Seejärel saate RDP-faili abil kaugtöölaua ühendus juurdepääsu pilveteenusesse.

### <a name="programmatically-download-the-rdp-file-through-the-service-management-rest-api"></a>Programmiliselt allalaadimine RDP-faili teenuse haldus REST API kaudu
ÜLEJÄÄNUD [RDP-faili alla laadida](https://msdn.microsoft.com/library/jj157183.aspx) selle toimingu abil saate RDP faili alla laadida. 



## <a name="to-configure-remote-desktop-in-the-service-definition-file"></a>Kaugtöölaua teenuse määratluse faili konfigureerimine

See meetod võimaldab kaugtöölaua lubamine rakenduste arendamise käigus. Seda moodust nõuab krüptitud paroolide talletamise oma teenuse konfiguratsiooni faili ja värskendusi Remote'i töölaua konfiguratsiooni nõuaks ülejäänud rakendus. Kui soovite need varjuküljed Ärge kasutage kirjeldatud Remote'i töölaua laiend vastavalt lähenemisviisi.  

Visual Studio abil saate [lubada kaugtöölaua ühendus](../vs-azure-tools-remote-desktop-roles.md) teenuse määratluse faili lähenemisviisi abil.  
Alltoodud juhiseid kirjeldada lubamiseks kaugtöölaua teenuse mudelifaile vajalikud muudatused. Visual Studio muudab need muudatused automaatselt, kui avaldamine.

### <a name="set-up-the-connection-in-the-service-model"></a>Teenuse mudeli ühenduse häälestamine 
**Impordi** elemendi abil saate importida **RemoteAccess** mooduli ja mooduli **RemoteForwarder** [ServiceDefinition.csdef](cloud-services-model-and-package.md#csdef) faili.

Teenuse definitsioonifail peaks sarnanema koos järgmises näites on `<Imports>` lisatud element.

```xml
<ServiceDefinition name="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition" schemaVersion="2013-03.2.0">
    <WebRole name="WebRole1" vmsize="Small">
        <Sites>
            <Site name="Web">
                <Bindings>
                    <Binding name="Endpoint1" endpointName="Endpoint1" />
                </Bindings>
            </Site>
        </Sites>
        <Endpoints>
            <InputEndpoint name="Endpoint1" protocol="http" port="80" />
        </Endpoints>
        <Imports>
            <Import moduleName="Diagnostics" />
            <Import moduleName="RemoteAccess" />
            <Import moduleName="RemoteForwarder" />
        </Imports>
    </WebRole>
</ServiceDefinition>
```
Faili [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#cscfg) peaks olema umbes järgmine näide, Pange tähele selle `<ConfigurationSettings>` ja `<Certificates>` elemente. Serdi määratud tuleb [üles laadida pilveteenusesse](../cloud-services-how-to-create-deploy.md#how-to-upload-a-certificate-for-a-cloud-service).

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceConfiguration serviceName="<name-of-cloud-service>" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration" osFamily="3" osVersion="*" schemaVersion="2013-03.2.0">
    <Role name="WebRole1">
        <Instances count="2" />
        <ConfigurationSettings>
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.Enabled" value="true" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountUsername" value="[name-of-user-account]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountEncryptedPassword" value="[base-64-encrypted-user-password]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteAccess.AccountExpiration" value="[certificate-expiration]" />
            <Setting name="Microsoft.WindowsAzure.Plugins.RemoteForwarder.Enabled" value="true" />
        </ConfigurationSettings>
        <Certificates>
            <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption" thumbprint="[certificate-thumbprint]" thumbprintAlgorithm="sha1" />
        </Certificates>
    </Role>
</ServiceConfiguration>
```


## <a name="additional-resources"></a>Lisaressursid

[Kuidas seadistada pilveteenustega](cloud-services-how-to-configure.md)