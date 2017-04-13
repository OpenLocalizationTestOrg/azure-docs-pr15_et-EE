<properties
   pageTitle="Rakenduse täiendamine: kogenud | Microsoft Azure'i"
   description="Selles artiklis antakse ülevaade mõne täpsemalt teemade seotud teenuse struktuuri rakenduse täiendamine."
   services="service-fabric"
   documentationCenter=".net"
   authors="mani-ramaswamy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/14/2016"
   ms.author="subramar"/>

# <a name="service-fabric-application-upgrade-advanced-topics"></a>Teenuse struktuuri rakenduse täiendamine: täpsemad Teemad

## <a name="adding-or-removing-services-during-an-application-upgrade"></a>Lisamise või eemaldamise teenuste ajal rakenduse täiendamine

Kui rakendus, mis on juba juurutanud ja avaldatud versioonitäienduse lisatakse uus teenus, lisatakse uus teenus juurutatud rakendusega.  Seda ei mõjuta kõiki teenuseid, mis on juba rakenduse osa. Siiski teenus, mis lisati eksemplari tuleb luua uue teenuse aktiivsetena (abil soovitud `New-ServiceFabricService` cmdlet-käsk).

Teenuseid saate eemaldada ka rakenduse versiooniuuenduse käigus. Siiski kõik praeguse eksemplari to-be kustutatud teenuse peatada enne uuele versioonile (abil soovitud `Remove-ServiceFabricService` cmdlet-käsk). 

## <a name="manual-upgrade-mode"></a>Käsitsi versioonitäienduse režiim

> [AZURE.NOTE]  Unmonitored käsitsi režiimi tuleks kaaluda ainult nurjunud või ootel uuele versioonile. Jälgida režiim on soovitatav versioonitäienduse režiimi teenuse struktuuri rakendusi.

Azure teenuse struktuuri pakub mitme versioonitäienduse režiimide Arendus- ja kogumite toetamiseks. Valitud Juurutussuvandid võivad olla erinevad viibite.

Jälgida jooksva rakenduse täiendamine on kõige tüüpilised versioonitäienduse valmistamisel kasutada. Kui täiendamise poliitikat pole määratud, teenuse struktuuri tagab rakenduse terve enne uuele versioonile.

 Rakenduse administraator saab kasutada käsitsi jooksva rakenduse täiendamine režiim on kokku üle versioonitäienduse edenemist erinevate täiendamise domeenide kaudu. See režiim on kasulik, kui kohandatud või keerukas seisundi hindamise poliitika on vaja või tavapärasest täiendamine toimub (nt rakendus on juba andmekao).

Lõpuks automatiseeritud jooksva rakenduse täiendamine on kasulik või testimise keskkonnas anda kiire iteratsiooni tsükli teenuse arendamise käigus.

## <a name="change-to-manual-upgrade-mode"></a>Käsitsi versioonitäienduse režiimi
**Käsitsi**--lõpetamine rakenduse täiendamine veebisaidil praeguse UD ja muuta versioonitäienduse režiimi Unmonitored käsitsi. Administraator peab käsitsi kõne **MoveNextApplicationUpgradeDomainAsync** jätkata täiendamise või a tagasipööramine alustatakse uut versioonitäienduse käivitamine. Pärast versioonitäienduse sisestab käsitsi režiimi, jääb see käsitsi režiim, kuni algatatakse uus uuendada. Käsk **GetApplicationUpgradeProgressAsync** tagastab struktuuri\_rakenduse\_täiendamine\_olekus\_ROLLING\_edasi\_kuni.

## <a name="upgrade-with-a-diff-package"></a>Erinevus paketi versioonitäiendus

Teenuse struktuuri rakenduse versiooni täiendamist koos täielik iseseisev rakendusepaketi ettevalmistamise teel. Rakendus võib olla ka täiustatud erinevus paketi, mis sisaldab ainult värskendatud rakenduse failid, värskendatud rakenduse väljendada ning teenuse manifestifailidele abil.

Täielik rakenduse pakett sisaldab kõiki faile, mis on vajalikud käivitada ja teenuse struktuuri rakendus. Erinevus pakett sisaldab ainult failid, mis on muutunud viimase ja praeguse uuele versioonile pluss täielik Rakendusmanifest ja teenuste väljendada failid. Mis tahes viidet selle rakenduse loetelu või teenuse manifesti, mis ei leia Koosta paigutus on otsingud pilt pood.

Rakenduse täisversioon pakettide jaoks on vaja rakenduse klaster esimese installi. Edaspidised värskendused võib olla täielik rakendusepaketi või paketi erinevus.

Juhtudel erinevus pakett hea valik juhul, kui:

* Erinevus pakett on eelistatud, kui teil on suur rakendusepaketi mitme teenuse manifestifailidele ja/või mitme koodi pakettide, config pakettide või andmete pakettide viitav.

* Erinevus pakett on eelistatud, kui teil on juurutamise süsteem, mis loob Koosta paigutuse otse oma rakenduse koostamine protsess. Sel juhul isegi juhul, kui koodi pole muudetud, uute assemblereid saada erinevate kontrollsumma. Täielik rakendusepaketi abil nõuaks värskendamist pakettide kood versioon. Erinevus paketi kasutamisel ainult esitate faile, mis on muutunud ja manifestifailidele, kus versioon on muutunud.

Kui rakenduse versiooniuuendust Visual Studio abil, avaldatakse erinevus pakett automaatselt. Erinevus paketi käsitsi loomine rakenduse väljendada ning teenuse manifestid tuleb värskendada, kuid ainult muudetud pakettide tuleks lisada lõplik rakendusepaketi. 

Näiteks Alustame järgmine rakendus (versioon arve kergem mõista):

```text
app1        1.0.0
  service1  1.0.0
    code    1.0.0
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

Nüüd Oletame, et soovite värskendada ainult koodi paketi service1 abil erinevus paketi PowerShelli kaudu. Nüüd on värskendatud rakenduse järgmised kausta ülesehitust.

```text
app1        2.0.0      <-- new version
  service1  2.0.0      <-- new version
    code    2.0.0      <-- new version
    config  1.0.0
  service2  1.0.0
    code    1.0.0
    config  1.0.0
```

Sel juhul ei värskendada Rakendusmanifest 2.0.0 ja teenuse manifest service1 kajastamiseks kood paketi värskendus. Kausta oma rakendusepaketi oleks järgmine struktuur:

```text
app1/
  service1/
    code/
```

## <a name="next-steps"></a>Järgmised sammud

[Täiendamist oma rakenduse abil Visual Studio](service-fabric-application-upgrade-tutorial.md) juhendab teid rakenduse täiendamine Visual Studio abil.

[Rakenduse abil PowerShelli üleminekut](service-fabric-application-upgrade-tutorial-powershell.md) juhendab teid rakenduse täiendamine PowerShelli kaudu.

Saate määrata, kuidas rakenduse uuendab [Täiendamine parameetrite](service-fabric-application-upgrade-parameters.md)abil.

Muuta oma rakenduse uuendamine ühilduvad õppida, kuidas kasutada [Andmete sariväljaanne](service-fabric-application-upgrade-data-serialization.md).

Levinud probleemide rakenduse uuendamine viidates [Rakenduse uuendused tõrkeotsingu](service-fabric-application-upgrade-troubleshooting.md)juhiseid.
 
