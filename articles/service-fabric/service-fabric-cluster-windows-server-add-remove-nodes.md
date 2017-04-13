<properties
   pageTitle="Lisamine või eemaldamine sõlmed autonoomse teenuse struktuuri arvutikobaras | Microsoft Azure'i"
   description="Saate teada, kuidas lisada või eemaldada sõlmed Azure teenuse struktuuri arvutikobaras, füüsilise või töötab Windows Server, mis võiks olla kohapealse või mis tahes pilveteenuses."
   services="service-fabric"
   documentationCenter=".net"
   authors="dsk-2015"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="09/20/2016"
   ms.author="dkshir;chackdan"/>


# <a name="add-or-remove-nodes-to-a-standalone-service-fabric-cluster-running-on-windows-server"></a>Lisage või eemaldage sõlmed autonoomse teenuse struktuuri kobar, kus töötab Windows Server

Kui olete [loonud klaster autonoomse teenuse struktuuri arvutites Windows Server](service-fabric-cluster-creation-for-windows-server.md), teie ettevõtte vajadustele muuta nii, et peate lisamine või eemaldamine mitme sõlmed klaster. Sellest artiklist leiate üksikasjalikud juhised selle eesmärgi saavutamiseks.


## <a name="add-nodes-to-your-cluster"></a>Klaster sõlmed lisamine

1. Ettevalmistamine lisamine klaster, järgides juhiseid jaotises [ettevalmistamine seotud vasta kobar juurutamiseks](service-fabric-cluster-creation-for-windows-server.md#preparemachines) VM/kohapeal.
2. Viga domeen ja te ei kavatse lisada selle VM/arvuti täiendamine domeeni kavandamine.
3. Kaugtöölaud (RDP) kohale VM/mida soovite lisada klaster.
4. Kopeeri või [teenuse struktuuri Windows Server laadige autonoomse](http://go.microsoft.com/fwlink/?LinkId=730690) selle VM/arvutisse ja pakkige see lahti pakkimine.
5. Käivitage PowerShelli administraatorina ja otsige üles soovitud asukoht mahalaadimist paketi.
6. Käivitage PowerShelli *AddNode.ps1* parameetritega, mis kirjeldab uus sõlm lisamiseks. Järgmises näites lisab uue sõlm nimega VM5, NodeType0, IP-aadress 182.17.34.52 UD1 ja FD1 tüübiga. *ExistingClusterConnectionEndPoint* on juba olemasolevad kobar sõlme ühenduse lõpp-punkti. Selle lõpp-punkti, saate valida klaster *igalt* IP-aadress.

```
.\AddNode.ps1 -NodeName VM5 -NodeType NodeType0 -NodeIPAddressorFQDN 182.17.34.52 -ExistingClientConnectionEndpoint 182.17.34.50:19000 -UpgradeDomain UD1 -FaultDomain FD1 -AcceptEULA

```

## <a name="remove-nodes-from-your-cluster"></a>Klaster sõlmed eemaldamine

1. Sõltuvalt Reliablity valitud klaster, ei saa eemaldada esimese n (3/5/7/9) sõlmed esmane sõlm tüüp
2. Käsu RemoveNode arendaja klaster ei toetata.
2. Kaugtöölaud (RDP) kohale VM/soovitud klaster eemaldamine.
2. Kopeeri või [teenuse struktuuri Windows Server laadige autonoomse](http://go.microsoft.com/fwlink/?LinkId=730690) ja pakkige see lahti paketi selles VM/arvutis.
3. Käivitage PowerShelli administraatorina ja otsige üles soovitud asukoht mahalaadimist paketi.
4. Käivitage PowerShelli *RemoveNode.ps1* . Järgmises näites eemaldab praegusest klaster. *ExistingClientConnectionEndpoint* on kliendi ühenduse näitaja, mis tahes sõlme, mis jäävad klaster. Valige IP-aadress ja *mis tahes* **muud sõlm** lõpp-punkti pordi klaster. Selle **muude sõlm** omakorda update eemaldatud sõlm kobar konfigureerimine. 

```
.\RemoveNode.ps1 -ExistingClientConnectionEndpoint 182.17.34.50:19000
```

Pange tähele, et isegi pärast eemaldamist sõlm, see võib näidata üles on alla ja SFX. See on teada veaks ja kõrvaldatakse on eelseisvad väljaanne. 


## <a name="next-steps"></a>Järgmised sammud
- [Autonoomse Windows kobar sätete konfigureerimine](service-fabric-cluster-manifest.md)
- [Turvaline autonoomse kobar Windows Windows turvalisuse abil](service-fabric-windows-cluster-windows-security.md)
- [Autonoomse kobar abil X509 Windows Secure serdid](service-fabric-windows-cluster-x509-security.md)
- [Luua eraldi teenuse struktuuri kobar opsüsteemi Windows Azure VMs](service-fabric-cluster-creation-with-windows-azure-vms.md)
