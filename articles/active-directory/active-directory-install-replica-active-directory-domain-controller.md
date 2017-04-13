<properties
    pageTitle="Installida Azure Active Directory koopia domeenikontrolleri | Microsoft Azure'i"
    description="Õppeteema, mis selgitab, kuidas installida Azure virtuaalne arvutis on kohapealse Active Directory kogumis domeenikontrolleri."
    services="virtual-network"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="virtual-network"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/30/2016"
    ms.author="curtand"/>


# <a name="install-a-replica-active-directory-domain-controller-in-an-azure-virtual-network"></a>Installige Azure'i virtuaalse võrgu koopia Active Directory domeenikontrolleri

Selles teemas kirjeldatakse kohapealse Active Directory domeeni kohta Azure'i virtuaalmasinates (VM) Azure virtuaalse võrgu jaoks täiendav domeenikontrollerid (nimetatakse ka koopia DCs) installimise kohta.

Teile võib huvi pakkuda ka nende seotud teemade:

-  Soovi korral saate uue Active Directory kogumis installida Azure virtuaalse võrgus. Need juhised leiate teemast [uue Active Directory kogumis Azure virtuaalse võrgus installida](../active-directory/active-directory-new-forest-virtual-machine.md).
-  Kontseptuaalne virtuaalse võrgus Azure Active Directory domeeniteenused (AD DS) installimise kohta vaadake teemast [juurutamine Windows Server Active Directory Azure'i Virtuaalmasinates juhised](https://msdn.microsoft.com/library/azure/jj156090.aspx).


## <a name="scenario-diagram"></a>Stsenaarium skeem

Selle stsenaariumi korral vaja väliste kasutajate juurdepääs rakendusi, mis töötavad domeeni ühendatud serverites. VMs, mis töötavad rakenduse serverid ja koopia DCs installitakse Azure virtuaalse võrgus. Virtuaalse võrgu saab ühendada kohapealse võrgu järgi [- saidilt VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md) -ühendus, nagu on näidatud järgmisel joonisel või kasutage [ExpressRoute](../expressroute/expressroute-locations-providers.md) kiirem ühenduse.

Rakenduste serverid ja DCs on juurutatud eraldi pilveteenustega levitada Arvuta töötlemine ja täiustatud tõrketaluvust [kättesaadavus komplektid](../virtual-machines/virtual-machines-windows-manage-availability.md) .
DCs ise omavahel ja DCs kohapealse Active Directory kopeerimise abil. Sünkroonimise tööriistu on vaja.

![Diagrammi pf koopia Active Directory domeenikontrolleri on Azure vnet][1]

## <a name="create-an-active-directory-site-for-the-azure-virtual-network"></a>Virtuaalse võrgu Azure Active Directory saidi loomine

See on hea mõte saidi loomine Active Directory, mis tähistab vastav võrgu piirkonna virtuaalse võrku. Mis aitab optimeerida autentimine, kopeerimise ja muid näiteks Põhiliselt asukoha toiminguid. Järgmised juhised selgitavad saidi loomine ja rohkem tausta, lugege teemat [uue saidi lisamine](https://technet.microsoft.com/library/cc781496.aspx).

1. Avage Active Directory saidid ja teenused: **Server Manager** > **Tööriistad** > **Active Directory saidid ja teenused**.
2. Piirkond, kuhu olete loonud mõne Azure virtuaalse võrgu tähistada saidi loomine: klõpsake nuppu **saidid** > **toimingu** > **Uus sait** > tippige nimi uue saidi, nt Azure meile Lääne > valige saidi link > **OK**.
3. Mõne alamvõrgu ja sidumiseks uus sait: topeltklõpsake **saidid** > Paremklõpsake **alamvõrku** > **uus alamvõrgu** > tippige IP-aadresside vahemiku virtuaalse võrgu (nt 10.1.0.0/16 stsenaarium diagrammi) > valige uus Azure'i saidi > **OK**.

## <a name="create-an-azure-virtual-network"></a>Azure virtuaalse luua

1. [Azure'i klassikaline portaalis](https://manage.windowsazure.com), klõpsake nuppu **Uus** > **Võrguteenuste** > **Virtuaalse võrgu** > **Kohandatud loomine** ja kasutage järgmist väärtust viisardi lõpuleviimine.

    Selle viisardi lehel...  | Nende väärtuste määramine
    ------------- | -------------
    **Virtuaalse võrgu üksikasjad**  | <p>Nimi: Tippige nimi, nt WestUSVNet virtuaalse võrgu.</p><p>Piirkond: Saate valida lähima regiooni.</p>
    **DNS-i ja VPN Ühenduvus**  | <p>DNS-i serverid: Määrake nimi ja ühe või mitme kohapealse DNS-i serveri IP-aadress.</p><p>Ühenduvus: Valige **VPN saitide konfigureerimine**.</p><p>Kohalikus võrgus: määrake uue kohalikus võrgus.</p><p>Kui kasutate asemel VPN ExpressRoute, vaadake [konfigureerimine on ExpressRoute ühendus mõne Exchange'i pakkuja kaudu](../expressroute/expressroute-locations-providers.md).</p>
    **Saitide Ühenduvus**  | <p>Nimi: Tippige kohapealse võrgu nimi.</p><p>VPN seadme IP-aadress: Määrake seade, mis loob ühenduse, virtuaalse võrku avaliku IP-aadress. VPN-seade ei leita taga mõnda NAT.</p><p>Aadress: Määrake aadresside vahemikud (nt 192.168.0.0/16 stsenaarium diagrammi) kohapealse võrgu jaoks.</p>
    **Virtuaalse võrgu aadress tühikud**  | <p>Aadressiruumi: Määrake IP-aadresside vahemiku vms, mida soovite käivitada Azure virtuaalse võrgu (nt 10.1.0.0/16 stsenaarium diagrammi). See aadress vahemiku ei kattu aadresside vahemikud kohapealse võrgu.</p><p>Alamvõrku: Määrake nimi ja aadress on alamvõrgu rakendus servereid (Frontend, nt 10.1.1.0/24) ja DCs (nt Taustväärtus, 10.1.2.0/24).</p><p>Klõpsake **Lisa lüüsi alamvõrgu**.</p>

2. Järgmiseks tuleb konfigureerida virtuaalse võrgu lüüsi turvaline-saidilt VPN-ühenduse loomiseks. Vaadake juhiseid [virtuaalse võrgu lüüsi konfigureerimine](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) .
3. Saate luua uue virtuaalse võrgu ja seadmes kohapealse VPN-saidilt VPN-ühendus. Vaadake juhiseid [virtuaalse võrgu lüüsi konfigureerimine](../vpn-gateway/vpn-gateway-configure-vpn-gateway-mp.md) .


## <a name="create-azure-vms-for-the-dc-roles"></a>Näiteks Põhiliselt rollide Azure VMs loomine

Korrake luua VMs hostimiseks näiteks Põhiliselt roll vastavalt vajadusele järgmist. Juurutamist peaks olema vähemalt kaks virtuaalse DCs tõrketaluvust ja koondamise. Kui Azure virtuaalse võrgu sisaldab vähemalt kaks DCs sarnaselt konfigureeritud (st need on nii GCs Käivita DNS-i server, ja valimata hoiab FSMO rolli ja jne) viige VMs, töötavad need DCs seadmine täiustatud tõrketaluvust olemasolu.
Windows PowerShelli abil asemel UI VMs loomiseks vaadake teemat [Azure PowerShelli kasutamine loomine ja Windowsi-põhiste Virtuaalmasinates preconfigure](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com), klõpsake nuppu **Uus** > **arvutada** > **virtuaalse masina** > **Galeriist**. Viisardi lõpuleviimine kasutada järgmised väärtused. Aktsepteerige vaikeväärtus sätet juhul, kui mõni muu väärtus on soovitatud või nõutav.

    Selle viisardi lehel...  | Nende väärtuste määramine
    ------------- | -------------
    **Valige pilt**  | Windows Server 2012 R2 andmekeskusega
    **Virtuaalse masina konfigureerimine**  | <p>Virtuaalse masina nimi: Tippige ühe sildi nimi (nt AzureDC1).</p><p>Uue kasutaja nimi: Tippige kasutaja nimi. Selle kasutaja saab VM kohalike administraatorite rühma liige. Peate selle nime VM esimest korda sisse logida. Sisseehitatud kontot nimega administraator ei tööta.</p><p>Parooli kinnitamine: uus parool</p>
    **Virtuaalse masina konfigureerimine**  | <p>Pilveteenuses: Valige <b>luua uue pilveteenuses</b> esimese VM ja selle sama pilvepõhise teenuse nime Rohkem VMs, näiteks Põhiliselt roll majutavad loomisel.</p><p>Pilveteenuse teenuse DNS-i nimi: Määrake globaalselt kordumatu nimi</p><p>Regioon/osaleja rühma/Virtual Network: määrake virtuaalse võrgu nimi (nt WestUSVNet).</p><p>Salvestusruumi konto: Valige <b>konto automaatselt loodud mäluruumi kasutamine</b> esimese VM ja seejärel valige selle sama salvestusruumikonto nimi veel VMs, näiteks Põhiliselt roll majutavad loomisel.</p><p>Kättesaadavus määramine: Valige <b>luua mõne kättesaadavus</b>.</p><p>Kättesaadavus seadmine nimi: tippige nimi kättesaadavus seatud esimese VM loomisel ning valige seejärel, et sama nime, kui loote rohkem VMs.</p>
    **Virtuaalse masina konfigureerimine**  | <p>Valige <b>installida VM Agent</b> ja muud laiendid, mida teil on vaja.</p>
2. Kinnitage kettal iga VM, mis kestab näiteks Põhiliselt serveri roll. Vabastage kettal on vaja AD andmebaasi, logid ja SYSVOL talletamiseks. Määrake ketta (nt 10 GB) suurus ja jätke **Host vahemälu eelistus** **puudub**. Juhised leiate teemast [andmete kettapuhastusriista abil Windowsi virtuaalse masina manustada](../virtual-machines/virtual-machines-windows-classic-attach-disk.md).
3. Kui te esmalt sisse logida VM, avage **Server Manager** > **faili ja salvestusruumi teenuste** loomiseks selle ketta NTFS abil.
4. Saate reserveerida vms, näiteks Põhiliselt roll töötavad staatiline IP-aadress. Reserveerida staatiline IP-aadress, alla laadida Microsoft Web platvormi installiprogramm ja [installige Azure'i PowerShelli](../powershell-install-configure.md) ja käivitage cmdlet-käsk Set-AzureStaticVNetIP. Näiteks:

    "Get-AzureVM - teenuse nimi AzureDC1-nime AzureDC1 | Set-AzureStaticVNetIP - sordib 10.0.0.4 | Update-AzureVM

Staatiline IP-aadress määramise kohta leiate lisateavet teemast [konfigureerimine VM staatiline sisemise IP-aadress](../virtual-network/virtual-networks-reserved-private-ip.md).

## <a name="install-ad-ds-on-azure-vms"></a>Installige Azure'i VMs AD DS

Logige sisse VM ja veenduge, et teil on Ühenduvus üle-saidilt VPN- või ExpressRoute ühenduse ressursid kohapealse võrgus. Installige Azure'i VMs AD DS. Saate kasutada sama protsessi abil saate installida ka täiendavad näiteks Põhiliselt kohapealse võrgu (UI, Windows PowerShelli või mõne vastuse fail). Kui installite AD DS, veenduge, et teie määratud uus draiv AD andmebaasi, logid ja SYSVOL asukohale. Kui vajate põhivalikpäringute AD DS installi, lugege [Installida Active Directory domeeniteenused (tase 100)](https://technet.microsoft.com/library/hh472162.aspx) või [installida koopia Windows Server 2012 domeenikontrolleri olemasoleva domeeni (tase 200)](https://technet.microsoft.com/library/jj574134.aspx).

## <a name="reconfigure-dns-server-for-the-virtual-network"></a>DNS-i serveri virtuaalse võrgu jaoks konfigureerida

1. [Azure'i klassikaline portaalis](https://manage.windowsazure.com), klõpsake virtuaalse võrgu nimi ja seejärel [DNS-i serveri IP-aadresside virtuaalse võrgu jaoks konfigureerida](../virtual-network/virtual-networks-manage-dns-in-vnet.md) staatiline IP-aadressid, mis on määratud koopia DCs IP-aadressid on kohapealse DNS-serverite asemel kasutada vahekaarti **konfigureerimine** .

2. Tagamaks, et kõik koopia näiteks Põhiliselt VMs virtuaalse võrgus on konfigureeritud kasutama DNS-serverid virtuaalse võrgus, klõpsake **Virtuaalmasinates**, klõpsake veerus Olek iga VM ja klõpsake **uuesti**. Oodake, kuni VM näitab **töötab** riigi, enne kui proovite seda sisse logida.

## <a name="create-vms-for-application-servers"></a>Rakenduste serverid VMs loomine

1. Korrake luua VMs rakenduse serverid käivitamiseks tehke järgmist. Aktsepteerige vaikeväärtus sätet juhul, kui mõni muu väärtus on soovitatud või nõutav.

    Selle viisardi lehel...  | Nende väärtuste määramine
    ------------- | -------------
    **Valige pilt**  | Windows Server 2012 R2 andmekeskusega
    **Virtuaalse masina konfigureerimine**  | <p>Virtuaalse masina nimi: Tippige ühe sildi nimi (nt AppServer1).</p><p>Uue kasutaja nimi: Tippige kasutaja nimi. Selle kasutaja saab VM kohalike administraatorite rühma liige. Peate selle nime VM esimest korda sisse logida. Sisseehitatud kontot nimega administraator ei tööta.</p><p>Parooli kinnitamine: uus parool</p>
    **Virtuaalse masina konfigureerimine**  | <p>Pilveteenusesse: Valige **luua uue pilveteenusesse** esimese VM ja selle sama pilvepõhise teenuse nime Rohkem VMs majutavad rakenduse loomisel.</p><p>Pilveteenuse teenuse DNS-i nimi: Määrake globaalselt kordumatu nimi</p><p>Regioon/osaleja rühma/Virtual Network: määrake virtuaalse võrgu nimi (nt WestUSVNet).</p><p>Salvestusruumi konto: Valige **konto automaatselt loodud mäluruumi kasutamine** esimese VM ja seejärel valige selle sama salvestusruumikonto nimi veel VMs majutavad rakenduse loomisel.</p><p>Kättesaadavus määramine: Valige **luua mõne kättesaadavus**.</p><p>Kättesaadavus seadmine nimi: tippige nimi kättesaadavus seatud esimese VM loomisel ning valige seejärel, et sama nime, kui loote rohkem VMs.</p>
    **Virtuaalse masina konfigureerimine**  | <p>Valige <b>installida VM Agent</b> ja muud laiendid, mida teil on vaja.</p>

2. Pärast iga VM on ette valmistatud, logige sisse ja liituda domeeni. Klõpsake **Server Manager** **Kohaliku serveri** > **töörühm** > **muutmine...** ja seejärel valige **Domeen** ja tippige oma kohapealse domeeni nimi. Domeeni kasutaja mandaat, ja seejärel taaskäivitage domeeni Liitu lõpuleviimiseks VM.

Windows PowerShelli abil asemel UI VMs loomiseks vaadake teemat [Azure PowerShelli kasutamine loomine ja Windowsi-põhiste Virtuaalmasinates preconfigure](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

Windows PowerShelli kasutamise kohta leiate lisateavet teemast [Alustamine Azure'i cmdlet-käskude](https://msdn.microsoft.com/library/azure/jj554332.aspx) ja [Azure cmdleti viide](https://msdn.microsoft.com/library/azure/jj554330.aspx).

## <a name="additional-resources"></a>Lisaressursid

-  [Windows Serveri Active Directory Azure'i Virtuaalmasinates juurutamise juhised](https://msdn.microsoft.com/library/azure/jj156090.aspx)
-  [Kuidas üles laadida olemasoleva kohapealse Hyper-V domeenikontrollerid Azure'i Azure PowerShelli abil](http://support.microsoft.com/kb/2904015)
-  [Uue Active Directory kogumis installida Azure virtuaalse võrgus](../active-directory/active-directory-new-forest-virtual-machine.md)
-  [Azure virtuaalse võrgu](../virtual-network/virtual-networks-overview.md)
-  [Microsoft Azure'i IT-spetsialistidele IaaS: (01) virtuaalse masina põhialused](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure'i IT-spetsialistidele IaaS: (05) loomise virtuaalne võrkude ja asukohaga Ühenduvus](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Azure'i PowerShelli](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Azure'i halduse cmdletid](https://msdn.microsoft.com/library/azure/jj152841)

<!--Image references-->
[1]: ./media/active-directory-install-replica-active-directory-domain-controller/ReplicaDCsOnAzureVNet.png
