<properties
   pageTitle="Azure'i teenus struktuuri usaldusväärne osalejate KVSActorStateProvider konfiguratsiooni ülevaade | Microsoft Azure'i"
   description="Lisateavet Azure teenuse struktuuri stateful osalejate tüüpi KVSActorStateProvider konfigureerimise kohta."
   services="Service-Fabric"
   documentationCenter=".net"
   authors="sumukhs"
   manager="timlt"
   editor=""/>

<tags
   ms.service="Service-Fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="sumukhs"/>

# <a name="configuring-reliable-actors--kvsactorstateprovider"></a>Usaldusväärne osalejate--KVSActorStateProvider konfigureerimine
Saate muuta, muutes settings.xml faili jaoks määratud osaleja Microsoft Visual Studio paketi root jaotises Config kausta loodud vaikimisi konfiguratsiooni KVSActorStateProvider.

Azure teenuse struktuuri käitusaja otsitakse eelmääratletud jaotise nimed settings.xml failis ja tarbib väärtuste määramine aluseks käitusaja komponentide loomisel.

>[AZURE.NOTE] Tehke **ei** kustutate või muudate järgmised settings.xml faili, mis on loodud lahenduse Visual Studios nimede jaotise.

## <a name="replicator-security-configuration"></a>Paljundaja turvalisus konfigureerimine
Turvaline side kanali dispersioonanalüüs ajal kasutatav kasutatakse paljundaja turvalisus konfiguratsioone. See tähendab, et teenused ei näe üksteise kopeerimise liikluse, tagades, et andmeid, mis on väga kättesaadav on turvaline.
Vaikimisi on tühi konfiguratsiooni turbejaotist takistab dispersioonanalüüs turvalisus.

### <a name="section-name"></a>Jaotise nimi
&lt;ActorName&gt;ServiceReplicatorSecurityConfig

## <a name="replicator-configuration"></a>Paljundaja konfigureerimine
Paljundaja konfiguratsioone konfigureerimine paljundaja, mis on vastutab näitleja oleku pakkuja olekus väga usaldusväärne.
Vaikimisi konfiguratsioon on loodud Visual Studio Mall ja piisab. Selles jaotises räägib täiendavad konfiguratsioone häälestada kopeerija saadaolevate.

### <a name="section-name"></a>Jaotise nimi
&lt;ActorName&gt;ServiceReplicatorConfig

### <a name="configuration-names"></a>Konfiguratsiooni nimed

|Nimi|Ühiku|Vaikeväärtus|Märkused|
|----|----|-------------|-------|
|BatchAcknowledgementInterval|Minutit|0.015|Aja jooksul, mille juures teisel ootab pärast toimingu enne saatmist kopeerija tagasi kinnituse esmane. Üks vastus saadetakse mis tahes muud kinnitused toimingute töödeldakse selles intervall saata.|
|ReplicatorEndpoint|N/A|Pole vaike - parameeter|IP-aadress ja pordi kasutavate esmane/teisene paljundaja suhelda muude paljundajatelt koopia seada. See peaks viide TCP ressursi lõpp teenuse manifestis. Lisateave [teenuse manifest ressursid](service-fabric-service-manifest-resources.md) viidata manifestis teenuse lõpp-punkti ressursside määratlemise kohta. |
|RetryInterval|Minutit|5|Aja jooksul, pärast mida kopeerija edastab uuesti sõnumi kui ta ei saa toimingu kinnituse.|
|MaxReplicationMessageSize|Baiti|50 MB|Maksimaalne maht dispersioonanalüüs andmeid, mis võib edastada ühele sõnumile.|
|MaxPrimaryReplicationQueueSize|Toimingute arv|1024|Suurim arv toiminguid Esmane järjekorda. Toiming on vabastatud pärast esmane paljundaja saab kinnituse kõik teisene paljundajatelt. See väärtus peab olema suurem kui 64 ja mingisse astmesse 2.|
|MaxSecondaryReplicationQueueSize|Toimingute arv|2048|Suurim arv toiminguid teisene järjekorda. Toiming on vabastatud pärast seisu väga püsimine kaudu kättesaadavaks tegemist. See väärtus peab olema suurem kui 64 ja mingisse astmesse 2.|

## <a name="store-configuration"></a>Poe konfigureerimine
Poe konfiguratsioone kasutatakse kohalikku salve, mida kasutatakse oleku, mis on kopeeritud säilitamise konfigureerimine.
Vaikimisi konfiguratsioon on loodud Visual Studio Mall ja piisab. Selles jaotises räägib täiendavad konfiguratsioone saadaolevad häälestada kohalikku salve.

### <a name="section-name"></a>Jaotise nimi
&lt;ActorName&gt;ServiceLocalStoreConfig

### <a name="configuration-names"></a>Konfiguratsiooni nimed

|Nimi|Ühiku|Vaikeväärtus|Märkused|
|----|----|-------------|-------|
|MaxAsyncCommitDelayInMilliseconds|Millisekundit|200|Määrab partiide püsival kohalikku salve võtab intervalli maksimumväärtus.|
|MaxVerPages|Lehekülgede arv|16384|Kohalik versioon lehtede arvu ülempiir talletada andmebaasi. See määratleb tasumata tehingute maksimaalne arv.|

## <a name="sample-configuration-file"></a>Näidisfaili konfigureerimine

```xml
<?xml version="1.0" encoding="utf-8"?>
<Settings xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <Section Name="MyActorServiceReplicatorConfig">
      <Parameter Name="ReplicatorEndpoint" Value="MyActorServiceReplicatorEndpoint" />
      <Parameter Name="BatchAcknowledgementInterval" Value="0.05"/>
   </Section>
   <Section Name="MyActorServiceLocalStoreConfig">
      <Parameter Name="MaxVerPages" Value="8192" />
   </Section>
   <Section Name="MyActorServiceReplicatorSecurityConfig">
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
## <a name="remarks"></a>Märkused

Parameetri BatchAcknowledgementInterval juhtelemendid dispersioonanalüüs latentsus. Väärtus "0", on tulemuseks väikseim võimalik latentsuse hinnaga läbilaskevõime (nagu kinnituse rohkem sõnumeid peab saadetud ja töödelda, iga sisaldavad vähem kinnitused).
Suurem väärtus BatchAcknowledgementInterval, suurem üldine dispersioonanalüüs jõudlus, hinnaga kõrgemate toimingu latentsus. Otse väljendub see tehingu võtab latentsus.
