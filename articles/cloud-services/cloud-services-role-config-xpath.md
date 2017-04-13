<properties 
pageTitle="Cloud Services roll config XPath täieliku leht | Microsoft Azure'i" 
description="XPath mitmesuguste sätete abil saate pilvepõhise teenuse rolli config esitamist sätted on muutuja." 
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
ms.date="08/10/2016" 
ms.author="adegeo"/>

# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>Esitamist rolli konfiguratsioonisätted on keskkonnas muutuv XPath

Pilvepõhise teenuse töötaja või web rolli teenuse määratluse faili, saate seada nimega keskkonna muutujate käitusaja väärtused. XPath järgmised väärtused on toetatud (mis vastavad API väärtused).

XPath need väärtused on ka [Microsoft.WindowsAzure.ServiceRuntime](https://msdn.microsoft.com/library/microsoft.windowsazure.serviceruntime.roleenvironment.aspx) teegi kaudu. 

## <a name="app-running-in-emulator"></a>Rakenduse emulaator töötab?

Näitab, et rakendus töötab emulaator.

| Tüüp  | Näide |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@emulated" |
| Kood  | var x = RoleEnvironment.IsEmulated; |


## <a name="deployment-id"></a>Juurutamise ID

Toob juurutamise ID eksemplari.

| Tüüp  | Näide |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/Deployment/@id" |
| Kood  | var deploymentId = RoleEnvironment.DeploymentId; |


## <a name="role-id"></a>Rolli ID 

Tagastab praeguse rolli ID eksemplari.

| Tüüp  | Näide |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@id" |
| Kood  | var id = RoleEnvironment.CurrentRoleInstance.Id; |


## <a name="update-domain"></a>Värskenda Domeen

Toob update domain astme.

| Tüüp  | Näide |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@updateDomain" |
| Kood  | var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |


## <a name="fault-domain"></a>Viga domeeni

Tagastab vea domeeni astme.

| Tüüp  | Näide |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@faultDomain" |
| Kood  | var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain; |


## <a name="role-name"></a>Rolli nimi

Toob linnanimede rolli nime.

| Tüüp  | Näide |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/@roleName" |
| Kood  | var rname = RoleEnvironment.CurrentRoleInstance.Role.Name;  |


## <a name="config-setting"></a>Otsingukonfiguratsiooni säte

Tagastab määratud konfiguratsioon sätte väärtus.

| Tüüp  | Näide |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Setting1']/@value" |
| Kood  | var säte = RoleEnvironment.GetConfigurationSettingValue("Setting1"); |
 
## <a name="local-storage-path"></a>Kohaliku tee

Toob tee kohaliku eksemplari.

| Tüüp  | Näide |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path" |
| Kood  | var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath; |


## <a name="local-storage-size"></a>Kohaliku mälumaht

Toob kohaliku eksemplari talletamist suurust.

| Tüüp  | Näide |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB" |
| Kood  | var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabytes; |

## <a name="endpoint-protocol"></a>Lõpp-punkti Protocol (protokoll) 

Lõpp-punkti protokoll eksemplari toob.

| Tüüp  | Näide |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol" |
| Kood  | var kaitsega = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. Protocol (protokoll); |

## <a name="endpoint-ip"></a>Lõpp-punkti IP

Saab määratud endpoint IP-aadress.

| Tüüp | Näide |
| ----- | ---- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address" |
| Kood  | var aadress = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Address |

## <a name="endpoint-port"></a>Lõpp-punkti port 

Lõpp-punkti pordi eksemplari toob.

| Tüüp  | Näide |
| ----- | ------- |
| XPath | xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port" |
| Kood  | var pordi = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Port; |





## <a name="example"></a>Näide

Siin on näide käivitus tööülesande nimega keskkonna muutujaga töötaja roll `TestIsEmulated` määramine on [ @emulated xpath väärtus](#app-running-in-emulator). 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Järgmised sammud

Lugege lisateavet [ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) faili.

[ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) paketi loomine.

Luba [Kaugtöölaud](cloud-services-role-enable-remote-desktop.md) rolli.
