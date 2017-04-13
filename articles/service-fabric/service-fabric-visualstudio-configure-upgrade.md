<properties
   pageTitle="Teenuse struktuuri rakenduse täiendamine konfigureerimine | Microsoft Azure'i"
   description="Saate teada, kuidas sätteid konfigureerida uuendamise teenuse struktuuri rakenduse Microsoft Visual Studio abil."
   services="service-fabric"
   documentationCenter="na"
   authors="cawaMS"
   manager="paulyuk"
   editor="tglee" />
<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="07/29/2016"
   ms.author="cawa" />

# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>Uuele versioonile teenuse struktuuri rakenduse Visual Studio konfigureerimine

Visual Studio tööriistad Azure teenuse struktuuri toetada versioonitäienduse kohaliku või kaugandmebaasiga kogumite avaldamine. On kaks eelised rakenduse asemel asendades rakenduse testimine ja silumine uuemale versioonile üleminekut.

- Rakenduse andmed ei lähe kaotsi versiooni täiendamise käigus.
- Kättesaadavus endiselt kõrge nii, et ei teenuse peatamine uuendamisel, kui seal on piisavalt eksemplaride laiali täiendamine domeenide.

Testide käitamise taotluse vastu samal ajal, kui see on uuendatud.

## <a name="parameters-needed-to-upgrade"></a>Parameetrite vaja versiooni täiendamine

Saate valida kahte tüüpi juurutamise: regulaar- või täiendamise. Tavaline juurutamise kustutab kõik eelmise juurutamise ja andmete klaster, kuvatakse versioonitäienduse juurutamise säilitab seda. Kui täiendate teenuse struktuuri rakenduse Visual Studios, peate pakkuda rakenduse täiendamine parameetrid ja seisundi kontrollimine poliitika. Rakenduse täiendamine parameetrid aitavad versiooniuuenduse ajal sisse rakendamist kindlaks teha, kas versioonitäienduse õnnestus. Vt [teenuse struktuuri rakenduse täiendamine: täiendamine parameetrite](service-fabric-application-upgrade-parameters.md) rohkem üksikasju.

On kolme versioonitäienduse viisi: *jälgitud*, *UnmonitoredAuto*ja *UnmonitoredManual*.

  - Jälgitud versiooniuuenduse automatiseerib täiendamise ja rakenduse seisundikontrolli.

  - Versiooniuuenduse UnmonitoredAuto automatiseerib uuele versioonile, kuid ignoreerib rakenduse seisundikontrolli.

  - Kui te seda versiooniuuenduse UnmonitoredManual, peate käsitsi värskendada iga uuendamise domeeni.

Iga versioonitäienduse režiimi nõuab erinevaid parameetrid. Vt [rakenduse täiendamine parameetrite](service-fabric-application-upgrade-parameters.md) versioonitäienduse saadaolevate suvandite kohta.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Teenuse struktuuri rakenduse Visual Studio täiendamine

Kui kasutate versiooni täiendamiseks teenuse struktuuri rakenduse Visual Studio teenuse struktuuri tööriistu, saate määrata avalda protsess olema versiooniuuendust, mitte tavaline juurutus, märkides ruudu ruut **rakenduse täiendamine** .

### <a name="to-configure-the-upgrade-parameters"></a>Versioonitäienduse parameetrite konfigureerimine

1. Märkeruudu kõrval nuppu **sätted** . Kuvatakse dialoogiboks **Redigeerimine versiooni täiendamine parameetrid** . Dialoogiboksi **Redigeerimine versiooni täiendamine parameetrid** toetab jälgitud, UnmonitoredAuto ja UnmonitoredManual versioonitäienduse režiimi.

2. Valige versioonitäienduse režiim, mida soovite kasutada, ja seejärel täitke parameeter ruudustik.

    Iga parameetri on vaikeväärtused. Valikuline parameeter *DefaultServiceTypeHealthPolicy* võtab räsi tabeli sisestatud. Siin on näide räsi Sisestuskeel tabelivormingu *DefaultServiceTypeHealthPolicy*jaoks:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* on teine valikuline parameeter, mis viib räsi tabeli sisendit järgmises vormingus:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    Siin on näide tegeliku elu:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```

3. Kui valite UnmonitoredManual versioonitäienduse režiimis, peate käsitsi käivitada PowerShelli konsooli jätkamiseks ja lõpetamiseks versiooniuuendus. Viidata [teenuse struktuuri rakenduse täiendamine: kogenud](service-fabric-application-upgrade-advanced.md) saate teada, kuidas käsitsi uuendamist töötab.

## <a name="upgrade-an-application-by-using-powershell"></a>Rakenduse täiendamine PowerShelli abil

PowerShelli cmdlet-käskude abil saate luua teenuse struktuuri rakenduse täiendamine. Üksikasjalikku teavet leiate [teenuse struktuuri rakenduse täiendamine õpetuse](service-fabric-application-upgrade-tutorial.md) ja [Algus-ServiceFabricApplicationUpgrade](https://msdn.microsoft.com/library/mt125975.aspx) .

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Rakenduse Avaldamisfail seisundi kontrolli poliitika määramine

Igal teenusel teenuse struktuuri rakendus võib olla oma seisundi parameetrid, mis vaikeväärtused alistada. Saate sisestada need parameetrite väärtused rakenduse Avaldamisfail.

Järgmises näites on kujutatud rakenduse manifestis iga teenuse jaoks kordumatu seisundi kontrolli poliitika rakendamise kohta.

```
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Järgmised sammud
Rakenduse kasutamise kohta leiate lisateavet teemast [Deploy olemasoleva rakenduse Azure teenuse struktuuri](service-fabric-deploy-existing-app.md).
