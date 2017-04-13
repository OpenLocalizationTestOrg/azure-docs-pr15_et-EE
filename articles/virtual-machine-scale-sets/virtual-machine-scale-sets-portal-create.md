<properties
    pageTitle="Luua on virtuaalse masina skaala seadmine Azure'i portaalis | Microsoft Azure'i"
    description="Juurutada skaala komplektid Azure'i portaalis."
    keywords="virtuaalse masina skaala komplektid" 
    services="virtual-machine-scale-sets"
    documentationCenter=""
    authors="gatneil"
    manager="madhana"
    editor="tysonn"
    tags="azure-resource-manager" />

<tags
    ms.service="virtual-machine-scale-sets"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/15/2016"
    ms.author="gatneil"/>

# <a name="create-a-virtual-machine-scale-set-using-the-azure-portal"></a>On virtuaalse masina skaala määramine Azure portaali loomine

Selle õpetuse näete, kui lihtne on luua virtuaalse masina skaala määramine vaid paari minutiga, Azure portaali kaudu. Kui teil pole Azure'i tellimus, looge [tasuta konto](https://azure.microsoft.com/free/) enne alustamist.

## <a name="choose-the-vm-image-from-the-marketplace"></a>Valige VM pilt kuvatakse Marketplace'ist

Portaalist saate hõlpsalt juurutada CentOS, CoreOS, Debian, avatud Suse, punane rolli Enterprise Linux, SUSE Linux Enterprise Server, Ubuntu Server või Windows Server piltide seadmine skaala.

Avage esmalt [Azure portaali](https://portal.azure.com) veebibrauseris. Klõpsake `New`, otsida `scale set`, ja seejärel valige soovitud `Virtual machine scale set` kirje:

![ScaleSetPortalOverview](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalOverview.PNG)

## <a name="create-the-linux-virtual-machine"></a>Linux virtuaalse masina loomine

Nüüd saate kasutada vaikesätteid ja kiiresti luua virtuaalse masina.

* Klõpsake soovitud `Basics` tera, sisestage nimi skaala määramine. See nimi muutub laadi koormusetasakaalustusteenuse ette skaala määramine FQDN alusega veenduge, et kõigis Azure on kordumatu nimi.

* Valige oma soovitud OS tippige, sisestage soovitud kasutajanimi ja valige, millist autentimise tüüp, mida eelistate. Kui valite parooli, peab olema vähemalt 12 märki pikk ja kolm välja neli järgmist keerukuse nõuetele vastavad: väiketähti ühe märgi, suurtähed ühe märgi, üks number ja ühe erimärgi. Vt lisateavet [kasutajanime ja parooli nõuete](../virtual-machines/virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm)kohta. Kui valite `SSH public key`, olla kindel, et ainult kleebi oma avalik võti pole teie isiklik võti:

![ScaleSetPortalBasics](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalBasics.PNG)

* Sisestage soovitud ressursirühma nimi ja asukoht ja seejärel klõpsake nuppu `OK`.

* Klõpsake soovitud `Virtual machine scale set service settings` blade: Sisestage soovitud Domeen nimesildi (alus laadi koormusetasakaalustusteenuse ette skaala määramine FQDN). Sildi kõigis Azure'i peab olema kordumatu.

* Valige oma soovitud operatsioonisüsteemi plaat, eksemplari arv ja mõõtmed.

* Lubamine või keelamine autoscale ja konfigureerida, kui lubatud.

![ScaleSetPortalService](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalService.PNG)

* Klõpsake soovitud `Summary` tera, kui kinnitamine on lõpule jõudnud, klõpsake nuppu `OK`.

* Lõpuks klõpsake soovitud `Purchase` tera, klõpsake `Purchase` skaala käivitamiseks määrata juurutamise.

## <a name="connect-to-a-vm-in-the-scale-set"></a>Ühenduse loomine VM skaala määramine

Kui teie skaala määramine on juurutatud, liikuge soovitud `Inbound NAT Rules` menüü laadi koormusetasakaalustusteenuse skaala jaoks:

![ScaleSetPortalNatRules](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalNatRules.PNG)

Saate luua ühenduse iga VM skaala määramine nende NAT reeglite abil. Näiteks Windows skaala komplekti, kui on NAT reegli sissetuleva pordi 50000, võib loote selle seadme kaudu RDP kohta `<load-balancer-ip-address>:50000`. Linux skaala komplekti, oleks ühendada, kasutades käsku `ssh -p 50000 <username>@<load-balancer-ip-address>`.

## <a name="next-steps"></a>Järgmised sammud

Dokumentatsiooni kohta, kuidas juurutada skaala komplektid CLI kaudu, leiate [selle dokumentatsioonist](./virtual-machine-scale-sets-cli-quick-create.md).

Dokumentatsiooni kohta, kuidas juurutada skaala komplektid PowerShelli kaudu, leiate [selle dokumentatsioonist](./virtual-machine-scale-sets-windows-create.md).

Dokumentatsiooni kohta, kuidas juurutamine Visual Studio skaala komplektid, vaadake [selle dokumentatsioonist](./virtual-machine-scale-sets-vs-create.md).

Üldine dokumentatsiooni kohta, vaadake [dokumentatsiooni ülevaate leht skaala määrab](./virtual-machine-scale-sets-overview.md).

Üldist teavet, lugege teemat [skaala komplektide peamised sihtleht](https://azure.microsoft.com/services/virtual-machine-scale-sets/).

