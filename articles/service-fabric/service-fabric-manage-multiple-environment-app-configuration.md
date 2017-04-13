<properties
   pageTitle="Mitme keskkondade teenuse struktuuri haldamine | Microsoft Azure'i"
   description="Kogumite, mille pindala ühe seadme kaudu juurde tuhandetele masinad saate teenuse struktuuri rakendusi käivitada. Mõnel juhul, mida soovite konfigureerida teisiti teie taotlus nende erinevad keskkonnad. Selles artiklis antakse ülevaade kohta keskkonna erinevates parameetrite määratlemine."
   services="service-fabric"
   documentationCenter=".net"
   authors="seanmck"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotNet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="07/19/2016"
   ms.author="seanmck"/>

# <a name="manage-application-parameters-for-multiple-environments"></a>Rakenduse parameetrid mitme keskkonnas haldamine

Saate luua Azure teenuse struktuuri kogumite, kasutades suvalist ühest mitu tuhat masinad. Ajal Binaarfailid käitamise muutmata kujul üle selle mitmesuguseid keskkonnas, mida sageli soovite konfigureerimine rakenduse erineda sõltuvalt masinad juurutamist, et arv.

Lihtne näide kaaluge `InstanceCount` kodakondsuseta teenuse. Kui kasutate rakendusi Azure'is, on üldiselt soovite seada see parameeter teisiti väärtusest "-1". See tagab, et iga sõlme klaster töötab teie teenuse. Siiski, selle konfiguratsiooni ei sobi ühe-seadme kobar Kuna te ei tohi olla mitu protsesside listening sama lõpp-punkti ühte arvutisse. Selle asemel saate tavaliselt seadmine `InstanceCount` "1".

## <a name="specifying-environment-specific-parameters"></a>Keskkonna kohased parameetrite määramine

Selle konfiguratsiooni probleemi lahendamiseks on kogum parameetritega vaikimisi teenuste ja nende parameetrite väärtused antud keskkonnas täitke rakenduse parameetri failid. Rakenduste ja teenuste manifestid on konfigureeritud vaikimisi teenuste ja rakenduse parameetrid. Skeemi definitsiooni ServiceManifest.xml ja ApplicationManifest.xml failid on installitud teenuse struktuuri SDK ja *C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*tööriistu.

### <a name="default-services"></a>Vaikimisi teenused

Teenuse struktuuri rakenduste koosnevad eksemplaride kogum. Kuigi saate luua tühja rakenduse ja seejärel looge läbivalt kogu teenuse dünaamiliselt, on kõige rakendused core services, mis tuleks alati luua, kui rakendus on käivitatud kogum. Need on nimetatud "vaikimisi teenused". Need on määratud Rakendusmanifest kohatäidetega kohta keskkonna konfigureerimise kaasatud nurksulgudega:

    <DefaultServices>
        <Service Name="Stateful1">
            <StatefulService
                ServiceTypeName="Stateful1Type"
                TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]"
                MinReplicaSetSize="[Stateful1_MinReplicaSetSize]">

                <UniformInt64Partition
                    PartitionCount="[Stateful1_PartitionCount]"
                    LowKey="-9223372036854775808"
                    HighKey="9223372036854775807"
                />
        </StatefulService>
    </Service>
  </DefaultServices>

Kõik nimega parameetrid peab määratletud parameetrite element Rakendusmanifest:

    <Parameters>
        <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="2" />
        <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
        <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
    </Parameters>

Atribuut DefaultValue määrab väärtuse kasutada rohkem kohased parameetri puudumisel antud keskkonnas.

>[AZURE.NOTE] Kõik teenuse eksemplari parameetrid sobivad kohta keskkonna konfiguratsioon. Ülaltoodud näites määratud teenuse eraldamine kava LowKey ja HighKey väärtused konkreetselt kõik eksemplarid teenuse sektsiooni vahemikus on funktsiooni andmete domeeninime, mitte keskkonnas.


### <a name="per-environment-service-configuration-settings"></a>Kohta keskkonna konfiguratsioon Teenusesätted

[Teenuse struktuuri mudel](service-fabric-application-model.md) võimaldab teenuste konfigureerimine paketid, mis sisaldavad kohandatud võti ja väärtuse paarideks, mis on loetav käitusajal kaasata. Nende sätete väärtused võib eristada ka keskkond määramisega on `ConfigOverride` rakenduse manifestis.

Oletame, et teil järgmise sätte Config\Settings.xml faili jaoks soovitud `Stateful1` teenuse:


    <Section Name="MyConfigSection">
      <Parameter Name="MaxQueueSize" Value="25" />
    </Section>

See väärtus kindla rakenduse/keskkonna paar alistamiseks loomine on `ConfigOverride` teenuse manifest rakenduse manifestis importimisel.

    <ConfigOverrides>
     <ConfigOverride Name="Config">
        <Settings>
           <Section Name="MyConfigSection">
              <Parameter Name="MaxQueueSize" Value="[Stateful1_MaxQueueSize]" />
           </Section>
        </Settings>
     </ConfigOverride>
  </ConfigOverrides>

Seda parameetrit saab konfigureerida keskkond nagu eespool näidatud. Seda teha tunnistada parameetrite osas Rakendusmanifest ja rakenduse parameetri failid, mis määrab keskkonna kohased väärtused.

>[AZURE.NOTE] Teenusesätted konfiguratsiooni puhul on kolmes kohas, kus saate määrata klahvi väärtus: pakett konfiguratsiooni, rakenduse väljendada ning rakenduse parameetri fail. Teenuse struktuuri valib alati taotluse parameetrite põhjal esimese (kui see on määratud), siis rakenduse väljendada ning lõpuks konfiguratsiooni pakett.


### <a name="application-parameter-files"></a>Rakenduse parameetri failid

Teenuse struktuuri rakenduses project võivad sisaldada ühe või mitme rakenduse parameetri faile. Üks neist määratleb kindlate parameetrite, mis on määratletud Rakendusmanifest väärtused:

    <!-- ApplicationParameters\Local.xml -->

    <Application Name="fabric:/Application1" xmlns="http://schemas.microsoft.com/2011/01/fabric">
        <Parameters>
            <Parameter Name ="Stateful1_MinReplicaSetSize" Value="2" />
            <Parameter Name="Stateful1_PartitionCount" Value="1" />
            <Parameter Name="Stateful1_TargetReplicaSetSize" Value="3" />
        </Parameters>
    </Application>

Vaikimisi sisaldab uus rakendus kahe rakenduse parameetri failid, nimega Local.xml ja Cloud.xml.

![Rakenduse parameetri failide Solution Exploreris][app-parameters-solution-explorer]

Uue parameetri faili loomiseks lihtsalt kopeerida ja kleepida olemasoleva ning sellele uue nime panna.

## <a name="identifying-environment-specific-parameters-during-deployment"></a>Keskkonna kohased parameetrite juurutamisel tuvastamine

Juurutamise ajal peate valima vastav parameeter faili oma rakendusega rakendada. Saate seda teha avalda dialoogiboksi Visual Studio või PowerShelli kaudu.

### <a name="deploy-from-visual-studio"></a>Visual Studio juurutamine

Kui avaldate rakenduse Visual Studio saate valida saadaolevad failide loend.

![Valige dialoogiboksis avalda parameetri faili][publishdialog]

### <a name="deploy-from-powershell"></a>PowerShelli kaudu juurutamine

Funktsiooni `Deploy-FabricApplication.ps1` PowerShelli skripti rakenduse project Mall sisaldab aktsepteerib avalda profiili parameetrina ja selle PublishProfile sisaldab viite rakendus parameetrite fail.

  ```PowerShell
    ./Deploy-FabricApplication -ApplicationPackagePath <app_package_path> -PublishProfileFile <publishprofile_path>
  ```

## <a name="next-steps"></a>Järgmised sammud

Selles teemas kirjeldatud keskendudes osa kohta leiate lisateavet teemast [teenuse struktuuri tehniline ülevaade](service-fabric-technical-overview.md). Muud saadaolevad Visual Studio rakenduse halduse võimaluste kohta leiate teavet teemast [teenuse struktuuri Visual Studio rakenduste haldamine](service-fabric-manage-application-in-visual-studio.md).

<!-- Image references -->

[publishdialog]: ./media/service-fabric-manage-multiple-environment-app-configuration/publish-dialog-choose-app-config.png
[app-parameters-solution-explorer]:./media/service-fabric-manage-multiple-environment-app-configuration/app-parameters-in-solution-explorer.png
