<properties
   pageTitle="Sündmuste jälgimise tõrkeotsingu | Microsoft Azure'i"
   description="Ilmnes juurutamine teenust Microsoft Azure teenuse struktuuri levinumaid probleeme."
   services="service-fabric"
   documentationCenter=".net"
   authors="mattrowmsft"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="03/31/2016"
   ms.author="mattrow"/>


# <a name="troubleshoot-common-issues-when-you-deploy-services-on-azure-service-fabric"></a>Levinud probleemide teenust Azure teenuse struktuuri juurutamisel

Kui kasutate teenuseid arendaja arvutis, on lihtne kasutada [Visual Studio silumine tööriistad](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md). Remote kogumite, [Seisundiaruannete](service-fabric-view-entities-aggregated-health.md) on alati hea koht alustamiseks. Lihtsaim viis aruannete juurdepääs on PowerShelli või [SFX](service-fabric-visualizing-your-cluster.md). Selles artiklis eeldatakse, et teil on silumine remote kobar ja kas nende tööriistade kasutamine lihtne mõista.

##<a name="application-crash"></a>Rakenduse krahhi
Funktsiooni "sektsioon on all suunata koopia või eksemplari count" aruanne on hea märk, mis on teie teenuse krahh. Välja selgitada, kus teie teenuse krahh võtab veidi rohkem uurimine. Oma teenuse käivitamisel skaala saab teie sõber komplekti hästi välja jälgi.  Soovitame püüate [Azure'i diagnostika](service-fabric-diagnostics-how-to-setup-wad.md) kogumiseks nende jälgi ja abil nagu [Elastne otsing](service-fabric-diagnostic-how-to-use-elasticsearch.md) lahenduse vaatamiseks ja otsimise jälgi.

![SFX Partition seisund](./media/service-fabric-diagnostics-troubleshoot-common-scenarios/crashNewApp.png)

###<a name="during-service-or-actor-initialization"></a>Teenuse või näitleja lähtestamisel
Mis tahes erandid enne teenuse tüüp on lähtestatud põhjustab krahhi protsess. Seda tüüpi jookseb, kuvatakse rakenduse sündmuselogi tõrge teenusepakkuja.
Need on kõige levinum erandid vaatamiseks enne teenus on lähtestatud.

***System.IO.FileNotFoundException***

Selle tõrke põhjuseks sageli puuduvad komplekti sõltuvused. Märkige ruut CopyLocal atribuut Visual Studio või assemblervahemälust sõlme.

***System.Runtime.InteropServices.COMException***
 *juures System.Fabric.Interop.NativeRuntime+IFabricRuntime.RegisterStatefulServiceFactory (IntPtr, IFabricStatefulServiceFactory)*
 
 See näitab, et registreeritud teenuse tüüp nimi ei vasta teenuse manifest.

[Azure'i diagnostika](service-fabric-diagnostics-how-to-setup-wad.md) saate konfigureerida üles kõik teie sõlmed rakenduse sündmuselogist automaatselt.

###<a name="runasync-or-onactivateasync"></a>RunAsync() või OnActivateAsync()
Kui lähtestamine või töötab teie registreeritud teenuse tüüp või näitleja ajal juhtub krahhi, välja arvatud kohaldatakse tema Azure teenuse struktuuri. Saate vaadata neid EventSource pakkujate jaotises "Järgmised sammud".

## <a name="next-steps"></a>Järgmised sammud

Lisateave teenuse struktuuri esitatud olemasoleva diagnostika.

* [Usaldusväärne osalejate diagnostika](service-fabric-reliable-actors-diagnostics.md)
* [Usaldusväärne Services diagnostika](service-fabric-reliable-services-diagnostics.md)
