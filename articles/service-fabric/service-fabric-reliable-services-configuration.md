<properties
   pageTitle="Azure'i teenus struktuuri usaldusväärsed teenused konfiguratsiooni ülevaade | Microsoft Azure'i"
   description="Lisateavet konfigureerimine stateful usaldusväärsed teenused Azure teenuse struktuuri."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor="vturecek"/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="10/18/2016"
   ms.author="sumukhs"/>

# <a name="configure-stateful-reliable-services"></a>Stateful usaldusväärne teenuste konfigureerimine

On kahte tüüpi usaldusväärne teenuste sätete konfigureerimine. Ühe on globaalne klaster usaldusväärne teenuste kõik muud määramine on teatud kindla töökindlat.

## <a name="global-configuration"></a>Globaalne konfigureerimine

Globaalne töökindlat konfiguratsioonis määratud kobar manifest kobar KtlLogger jaotises. See võimaldab konfigureerimine ühiskasutusega Logi asukoha ja suuruse pluss globaalse mälu piirangud, mida kasutavad logija. Kobar manifest on ühe XML-fail, mis hoiab sätted ja kuvatakse kõik sõlmed ja teenused klaster rakendamine. Fail on tavaliselt nimega ClusterManifest.xml. Saate vaadata klaster manifest klaster käsk Get-ServiceFabricClusterManifest PowerShelli jaoks.

### <a name="configuration-names"></a>Konfiguratsiooni nimed

|Nimi|Ühiku|Vaikeväärtus|Märkused|
|----|----|-------------|-------|
|WriteBufferMemoryPoolMinimumInKB|KB.|8388608|Vähim arv eraldada tuum režiimis puuraidur kirjutamine puhvri mälu pargi KB. See mälu pool kasutatakse olekuteabe enne kettale kirjutamine vahemällu.|
|WriteBufferMemoryPoolMaximumInKB|KB.|Piiramatult|Maksimaalne maht, millele logija kirjutamine puhvri mälu pargi võivad kasvada.|
|SharedLogId|GUID|""|Saate määrata kordumatu GUID tuvastamise kasutavad kõik usaldusväärsed teenused kõik sõlmed klaster, mis määravad selle SharedLogId nende teenuste teatud konfiguratsiooni ühiskasutusega logifaili vaikimisi kasutada. Kui SharedLogId pole määratud, siis SharedLogPath ka olema määratud.|
|SharedLogPath|Täielik tee nimi|""|Saate määrata, kus ühiskasutusega logifaili kasutavad kõik usaldusväärsed teenused kõik sõlmed klaster, mis määravad selle SharedLogPath nende teenuste teatud konfiguratsiooni täielik tee. Juhul, kui SharedLogPath pole määratud, siis SharedLogId ka olema määratud.|
|SharedLogSizeInMB|MB|8192|Määrab MB kettaruumi eraldada staatiliselt ühiskasutusega Logi arvu. Väärtus peab olema suurem või 2048.|

### <a name="sample-cluster-manifest-section"></a>Valimi kobar Manifestijaotis
```xml
   <Section Name="KtlLogger">
     <Parameter Name="WriteBufferMemoryPoolMinimumInKB" Value="8192" />
     <Parameter Name="WriteBufferMemoryPoolMaximumInKB" Value="8192" />
     <Parameter Name="SharedLogId" Value="{7668BB54-FE9C-48ed-81AC-FF89E60ED2EF}"/>
     <Parameter Name="SharedLogPath" Value="f:\SharedLog.Log"/>
     <Parameter Name="SharedLogSizeInMB" Value="16383"/>
   </Section>
```

### <a name="remarks"></a>Märkused
Logija on globaalne mälu eraldatud, mitte leheküljed tuum mälu, mis on saadaval kõigi usaldusväärne teenuste sõlme andmete vahemällu enne sellele sihtotstarbeline log seostatud töökindlat koopia. Pool suurus kontrollib WriteBufferMemoryPoolMinimumInKB ja WriteBufferMemoryPoolMaximumInKB sätted. WriteBufferMemoryPoolMinimumInKB saate määrata nii see mälu pool algse suuruse ja alumise suurus, millele mälu pool võib Kahanda. WriteBufferMemoryPoolMaximumInKB on kõrgeim suurus mälu rakenduskausta võivad kasvada. Iga töökindlat koopia, mis on avatud võib suurendada mälu pool kuni WriteBufferMemoryPoolMaximumInKB süsteemi kindlaks summa. Kui on nõutud rohkem mälu kaustast mälu, kui see on saadaval, taotlused mälu edasi lükata, kuni mälu on saadaval. Seetõttu kui kirjutamine puhvri mälu pargi on liiga väike kindla konfiguratsiooni seejärel jõudlust võivad on.

SharedLogId ja SharedLogPath sätete alati kasutatakse koos GUID ja asukoht vaikimisi ühiskasutusega Logi kõik sõlmed klaster. Vaikimisi ühiskasutusega logifaili kasutatakse kõikide määrata teatud teenuse settings.xml sätete usaldusväärne teenuste jaoks. Parima jõudluse tagamiseks tuleks paigutada ühiskasutusega logifailid kettale, mida kasutatakse ainult ühiskasutusega logifaili vähendada.

SharedLogSizeInMB määrab preallocate vaikimisi ühiskasutusega Logi kõik sõlmed kettaruumi.  Selleks, et täpsustada SharedLogSizeInMB määratud pole vaja SharedLogId ja SharedLogPath.


## <a name="service-specific-configuration"></a>Kindla teenuse konfigureerimine
Konfiguratsiooni pakett (Config) või teenuse rakendamise (kood) abil saate muuta stateful usaldusväärsed teenused vaikimisi konfiguratsioone.

+ **Config** - konfiguratsiooni config paketi kaudu saab teha, muutes Settings.xml faili, mis on loodud Microsoft Visual Studio paketi root jaotises Config kausta rakenduse iga teenuse jaoks.
+ **Koodi** - koodi kaudu konfiguratsiooni saab teha, luues ReliableStateManager, mis on vajalikud suvandid on seatud ReliableStateManagerConfiguration objekti abil.

Vaikimisi Azure teenuse struktuuri käitusaja otsitakse eelmääratletud jaotise nimed Settings.xml failis ja tarbib väärtuste määramine aluseks käitusaja komponentide loomisel.

>[AZURE.NOTE] Kas **ei** Kustuta jaotises nimed, klõpsake selle Settings.xml järgmised faili mis on loodud Visual Studio lahendus juhul, kui soovite konfigureerida oma teenuse kood.
Ümbernimetamine config paketi või jaotist nimed vajavad koodi muutmine soovitud ReliableStateManager konfigureerimisel.


### <a name="replicator-security-configuration"></a>Paljundaja turvalisus konfigureerimine
Turvaline side kanali dispersioonanalüüs ajal kasutatav kasutatakse paljundaja turvalisus konfiguratsioone. See tähendab, et teenused ei saa näha kohe üksteise kopeerimise liikluse, tagades, et andmeid, mis on väga kättesaadav on turvaline. Vaikimisi on tühi konfiguratsiooni turbejaotist takistab dispersioonanalüüs turvalisus.

### <a name="default-section-name"></a>Jaotis vaikenime
ReplicatorSecurityConfig

>[AZURE.NOTE] Selle jaotise nime muutmiseks alistada replicatorSecuritySectionName parameetri ReliableStateManagerConfiguration ehitaja ReliableStateManager selle teenuse loomisel.


### <a name="replicator-configuration"></a>Paljundaja konfigureerimine
Paljundaja konfiguratsioone konfigureerimine paljundaja, kes vastutab muuta stateful töökindlat olekus väga usaldusväärne imitatsiooniga ja püsib kohalik olek.
Vaikimisi konfiguratsioon on loodud Visual Studio Mall ja piisab. Selles jaotises räägib täiendavad konfiguratsioone häälestada kopeerija saadaolevate.

### <a name="default-section-name"></a>Jaotis vaikenime
ReplicatorConfig

>[AZURE.NOTE] Selle jaotise nime muutmiseks alistada replicatorSettingsSectionName parameetri ReliableStateManagerConfiguration ehitaja ReliableStateManager selle teenuse loomisel.


### <a name="configuration-names"></a>Konfiguratsiooni nimed
|Nimi|Üksuse|Vaikeväärtus|Märkused|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Sekundid|0.015|Aja jooksul, mille juures teisel ootab pärast toimingu enne saatmist kopeerija tagasi kinnituse esmane. Üks vastus saadetakse mis tahes muud kinnitused toimingute töödeldakse selles intervall saata.|
|ReplicatorEndpoint|N/A|Pole vaike - parameeter|IP-aadress ja pordi kasutavate esmane/teisene paljundaja suhelda muude paljundajatelt koopia seada. See peaks viide TCP ressursi lõpp teenuse manifestis. Lisateave [teenuse manifest ressursid](service-fabric-service-manifest-resources.md) viidata manifestis teenuse lõpp-punkti ressursside määratlemise kohta. |
|MaxPrimaryReplicationQueueSize|Toimingute arv|8192|Suurim arv toiminguid Esmane järjekorda. Toiming on vabastatud pärast esmane paljundaja saab kinnituse kõik teisene paljundajatelt. See väärtus peab olema suurem kui 64 ja mingisse astmesse 2.|
|MaxSecondaryReplicationQueueSize|Toimingute arv|16384|Suurim arv toiminguid teisene järjekorda. Toiming on vabastatud pärast seisu väga püsimine kaudu kättesaadavaks tegemist. See väärtus peab olema suurem kui 64 ja mingisse astmesse 2.|
|CheckpointThresholdInMB|MB|50|Pärast, mis on checkpointed Logi faili ruumi hulk.|
|MaxRecordSizeInKB|KB|1024|Suurim kirje suurusega kopeerija võib kirjutada Logi sisse. Väärtus peab olema suurem kui 16 ja 4 kordne.|
|MinLogSizeInMB|MB|0 (süsteemi kindlaks)|Ülekande Logi suurus. Logi lubatakse pole kärpige suurus selle sätte all. 0 näitab, et kopeerija määrab minimaalse Logi mahtu. Väärtuse suurendamine suurendab tõenäosust oluline kärpimist langeb kirjed alates osaline koopiad ja varundamiseks tehke võimalust.|
|TruncationThresholdFactor|Dispersioonanalüüs|2|Määratleb, milliseid Logi suurus, käivitatakse kärpimine. Määrab kärpe lävi määratakse MinLogSizeInMB korrutatuna TruncationThresholdFactor. TruncationThresholdFactor peab olema suurem kui 1. MinLogSizeInMB * TruncationThresholdFactor peab olema väiksem kui MaxStreamSizeInMB.|
|ThrottlingThresholdFactor|Dispersioonanalüüs|4|Määratleb, milliseid Logi suurus, hakkavad koopia on rakendus. Max (MB) ahendamise läve määrab ((MinLogSizeInMB *ThrottlingThresholdFactor),(CheckpointThresholdInMB* ThrottlingThresholdFactor)). Ahendamise (MB) piirmäär peab olema suurem kui kärpimine (MB) sisse. (MB) kärpimine piirmäär peab olema väiksem kui MaxStreamSizeInMB.|
|MaxAccumulatedBackupLogSizeInMB|MB|800|Max kogunenud maht (MB) varukoopia logide antud varunduse Logi ahelas. Mõne suureneva varukoopia taotlused nurjub, kui selle varundamist tekitaks varunduse Logi, mis võib põhjustada akumuleeritud varukoopia logid pärast oluline täielik varundamist olla suurem kui selle suurust. Sellisel juhul kasutaja peab täielik varukoopia tegemiseks.|
|SharedLogId|GUID|""|Saate määrata kordumatu GUID tuvastamise ühiskasutusega logifaili kasutatakse selle koopia kasutamiseks. Tavaliselt teenuseid kasutada seda sätet. Juhul, kui SharedLogId pole määratud, siis SharedLogPath ka olema määratud.|
|SharedLogPath|Täielik tee nimi|""|Saate määrata, kus luuakse selle koopia ühiskasutusega logifaili täielik tee. Tavaliselt teenuseid kasutada seda sätet. Juhul, kui SharedLogPath pole määratud, siis SharedLogId ka olema määratud.|
|SlowApiMonitoringDuration|Sekundid|300|Määrab järelevalve intervalli hallatava API kõnede jaoks. Näide: kasutaja esitatud varukoopia tagasihelistamise funktsioon. Kui intervalli on möödunud, saadetakse hoiatus seisundi aruande seisund haldur.|

### <a name="sample-configuration-via-code"></a>Proovi konfiguratsiooni koodi kaudu
```csharp
class Program
{
    /// <summary>
    /// This is the entry point of the service host process.
    /// </summary>
    static void Main()
    {
        ServiceRuntime.RegisterServiceAsync("HelloWorldStatefulType",
            context => new HelloWorldStateful(context, 
                new ReliableStateManager(context, 
        new ReliableStateManagerConfiguration(
                        new ReliableStateManagerReplicatorSettings()
            {
                RetryInterval = TimeSpan.FromSeconds(3)
                        }
            )))).GetAwaiter().GetResult();
    }
}    
```
```csharp
class MyStatefulService : StatefulService
{
    public MyStatefulService(StatefulServiceContext context, IReliableStateManagerReplica stateManager)
        : base(context, stateManager)
    { }
    ...
}
```


### <a name="sample-configuration-file"></a>Näidisfaili konfigureerimine
```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="ReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="ReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
      <Parameter Name="CheckpointThresholdInMB" Value="512" />
   </Section>
   <Section Name="ReplicatorSecurityConfig">
      <Parameter Name="CredentialType" Value="X509" />
      <Parameter Name="FindType" Value="FindByThumbprint" />
      <Parameter Name="FindValue" Value="9d c9 06 b1 69 dc 4f af fd 16 97 ac 78 1e 80 67 90 74 9d 2f" />
      <Parameter Name="StoreLocation" Value="LocalMachine" />
      <Parameter Name="StoreName" Value="My" />
      <Parameter Name="ProtectionLevel" Value="EncryptAndSign" />
      <Parameter Name="AllowedCommonNames" Value="My-Test-SAN1-Alice,My-Test-SAN1-Bob" />
   </Section>
</Settings>
```


### <a name="remarks"></a>Märkused
BatchAcknowledgementInterval juhtelementide dispersioonanalüüs latentsus. Väärtus "0", on tulemuseks väikseim võimalik latentsuse hinnaga läbilaskevõime (nagu kinnituse rohkem sõnumeid peab saadetud ja töödelda, iga sisaldavad vähem kinnitused).
Suurem väärtus BatchAcknowledgementInterval, suurem üldine dispersioonanalüüs jõudlus, hinnaga kõrgemate toimingu latentsus. See otse vaste tehingu võtab latentsus.

CheckpointThresholdInMB väärtus määrab kopeerija abil saate talletada teavet selle koopia sihtotstarbeline logifail kettaruumi summa. Suurendab seda suurem kui vaikimisi väärtus võib põhjustada kiiremini ümberkonfigureerimine korda, kui lisatakse uus koopia määramine. Selle põhjuseks osaline olekus edastamine, mis toimub olemasolu veel ajalugu toimingute tõttu. See võib potentsiaalselt suurendada taastamise aeg koopia pärast krahhi.

MaxRecordSizeInKB säte määratleb kirje, mis võivad kirjutada kopeerija üheks logifaili maksimummaht. Enamikul juhtudel 1024 KB kirje vaikesuuruse on optimaalne. Juhul, kui teenuse põhjustab suuremat andmete üksuste osalema filtreerimisoleku teavet, siis seda väärtust võimalik, et peate suurendada. On vähe kasu tegemisel MaxRecordSizeInKB väiksem kui 1024, nagu väiksem kirjete kasutada ainult väiksem kirje jaoks vajalik ruum. Loodame, et see väärtus oleks vaja muuta ainult harva.

SharedLogId ja SharedLogPath sätted alati kasutatakse koos teenuse kasutamiseks sõlme vaikimisi ühiskasutusega Logi eraldi ühiskasutusega sisse logida. Parimaid tõhusust, tuleks võimalikult palju teenuseid võimalikult Määratlege sama ühiskasutusega Logi. Ühiskasutusega failide tuleks paigutada kettale, mida kasutatakse ainult ühiskasutusega logifaili vähendada pea liikumine. Loodame, et see väärtus oleks vaja muuta ainult harva.

## <a name="next-steps"></a>Järgmised sammud
 - [Teenuse struktuuri rakenduse Visual Studio silumine](service-fabric-debugging-your-application.md)
 - [Usaldusväärne teenuste tootearendusmaterjal](https://msdn.microsoft.com/library/azure/dn706529.aspx)
