<properties
    pageTitle="Installimine on Active Directory kogumis Azure virtuaalse võrgus | Microsoft Azure'i"
    description="Õppeteema, mis selgitab, kuidas luua uus Active Directory kogumis Azure virtuaalse võrgus virtuaalse masina (VM)."
    services="active-directory, virtual-network"
    keywords="Active directory virtuaalse masina, installi active directory kogumis, azure active directory videod "
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    tags=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/10/2016"
    ms.author="markusvi"/>


# <a name="install-a-new-active-directory-forest-on-an-azure-virtual-network"></a>Uue Active Directory kogumis installida Azure virtuaalse võrgus

Selles teemas kirjeldatakse [Azure virtuaalse võrgu](../virtual-network/virtual-networks-overview.md)kuidas luua uus Windows Server Active Directory keskkonnas Azure virtuaalse võrgus virtual arvutis (VM). Sel juhul pole ühendatud Azure virtuaalse võrgu kohapealse võrku.

Teile võib huvi pakkuda ka nende seotud teemade:

- Video, mis näitab neid juhiseid, vaadake [teemat installida uue Active Directory kogumis Azure virtuaalse võrgus](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
- Võite soovi korral [VPN saitide konfigureerimine](../vpn-gateway/vpn-gateway-site-to-site-create.md) ja seejärel ühte installige uus mets või laiendamine on asutusesiseses kogumis Azure virtuaalse võrku. Need juhised leiate teemast [installimine on koopia Active Directory domeenikontrolleri Azure virtuaalse võrgus](../active-directory/active-directory-install-replica-active-directory-domain-controller.md).
-  Kontseptuaalne virtuaalse võrgus Azure Active Directory domeeniteenused (AD DS) installimise kohta vaadake teemast [juurutamine Windows Server Active Directory Azure'i Virtuaalmasinates juhised](https://msdn.microsoft.com/library/azure/jj156090.aspx).

## <a name="scenario-diagram"></a>Stsenaarium skeem

Selle stsenaariumi korral vaja väliste kasutajate juurdepääs rakendusi, mis töötavad domeeni ühendatud serverites. VMs, mis töötavad rakenduse serverid VMs, mis töötavad domeenikontrollerid on installitud ja Azure virtuaalse võrgu oma pilveteenuses installitud. Need sisalduvad ka seadmine täiustatud tõrketaluvust olemasolu.

![Active Directory kogumis virtuaalmasinates Azure virtuaalse võrku sisse ][1] 7
## <a name="how-does-this-differ-from-on-premises"></a>Kuidas see erineb kohapealse?

Pole palju vahet võrreldes kohapealse Azure domeenikontrolleri installimine. Järgmises tabelis on loetletud Peamised erinevused.

Konfigureerida...  | Kohapealse  | Azure virtuaalse võrgu
------------- | -------------  | ------------
**Selle domeenikontrolleri IP-aadress**  | Staatiline IP-aadressi võrgu adapterit atribuutide määramine   | Käivitage cmdlet Set-AzureStaticVNetIP määrata staatiline IP-aadress
**DNS-i kliendi lahendaja**  | Määrata eelistatud ja Alternatiivne DNS-i serveri aadress võrgus domeeni liikmete adapterit atribuudid   | Määratud DNS-i serveri aadress on virtuaalse atribuudid
**Active Directory andmebaasi salvestusruum**  | Soovi korral muutke talletamise vaikeasukohta c:\  | Peate muutma talletamise vaikeasukohta c:\



## <a name="create-an-azure-virtual-network"></a>Azure virtuaalse luua

1. Azure'i klassikaline portaali sisse logida.
2. Saate luua virtuaalse võrgus. Klõpsake **võrkude** > **virtuaalse võrgu loomine**. Järgmises tabelis olevad väärtused abil viisardi lõpuleviimine.

    Selle viisardi lehel...  | Nende väärtuste määramine
    ------------- | -------------
    **Virtuaalse võrgu üksikasjad**  | <p>Nimi: Sisestage virtuaalse võrgu nimi</p><p>Piirkond: Saate valida lähima regiooni</p>
    **DNS-i ja VPN**  | <p>DNS-i serveri tühjaks</p><p>Kas VPN suvandit ei vali</p>
    **Virtuaalse võrgu aadress tühikud**  | <p>Alamvõrgu nimi: sisestage oma alamvõrgu nimi</p><p>Algusslaidi IP: <b>10.0.0.0</b></p><p>CIDR: <b>/24 (256)</b></p>



## <a name="create-vms-to-run-the-domain-controller-and-dns-server-roles"></a>Luua VMs käivitamiseks domeenikontrolleri ja DNS-i serveri rollid

Korrake luua VMs majutada näiteks Põhiliselt roll vastavalt vajadusele järgmist. Juurutamist peaks olema vähemalt kaks virtuaalse DCs tõrketaluvust ja koondamise. Kui Azure virtuaalse võrgu sisaldab vähemalt kaks DCs sarnaselt konfigureeritud (st need on nii GCs Käivita DNS-i server, ja valimata hoiab FSMO rolli ja jne) viige VMs, töötavad need DCs seadmine täiustatud tõrketaluvust olemasolu.

Windows PowerShelli abil asemel UI VMs loomiseks vaadake teemat [Azure PowerShelli kasutamine loomine ja Windowsi-põhiste Virtuaalmasinates preconfigure](../virtual-machines/virtual-machines-windows-classic-create-powershell.md).

1. Klassikaline portaalis, klõpsake nuppu **Uus** > **arvutada** > **virtuaalse masina** > **Galeriist**. Viisardi lõpuleviimine kasutada järgmised väärtused. Aktsepteerige vaikeväärtus sätet juhul, kui mõni muu väärtus on soovitatud või nõutav.

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

## <a name="install-windows-server-active-directory"></a>Installige Windows Server Active Directory

Kasutage sama tavapärase [AD DS installida](https://technet.microsoft.com/library/jj574166.aspx) , et kasutate kohapealse (st saate kasutada UI, vastus failina või Windows PowerShelli). Tuleb sisestada administraatori identimisteave uus mets installimiseks. Active Directory andmebaasi, logid ja SYSVOL asukoha määramine, muutmine talletamise vaikeasukohta operatsioonisüsteemi ketta täiendavate andmete, kuhu te lisatud VM.

Kui näiteks Põhiliselt installimine on lõpule jõudnud, ühenduse VM uuesti ja logige sisse näiteks Põhiliselt. Ärge unustage Määrake domeeni mandaat.

## <a name="reset-the-dns-server-for-the-azure-virtual-network"></a>DNS-i server Azure'i virtuaalse võrgu lähtestamine

1. Lähtestage DNS-i ekspediitor säte uue AV/DNS-i serveri.
  1. Server Manager, klõpsake **menüü Tööriistad** > **DNS-i**.
  2. **DNS-i haldur**paremklõpsake DNS-i serveri nimi ja klõpsake käsku **Atribuudid**.
  3. Klõpsake menüü **Edasisuunamislahendusi** nuppu ekspediitor IP-aadress ja klõpsake nuppu **Redigeeri**.  Valige IP-aadress ja klõpsake nuppu **Kustuta**.
  4. Klõpsake nuppu **OK** , et sulgeda redaktori ja **Ok** sulgemiseks uuesti DNS-i serveri atribuudid.
2. Värskendage DNS-i server, virtuaalse võrgu säte.
  1. Klõpsake **Virtuaalne võrkude** > topeltklõpsake loodud virtuaalse võrgu > **konfigureerimine** > **DNS-serverid**, sisestage nimi ja selle pane ühe VMs, mis töötab AV/DNS-i serveri roll ja klõpsake nuppu **Salvesta**.
  2. Valige VM ja klõpsake **uuesti** käivitamiseks VM DNS-i Resolveri sätete konfigureerimiseks uus DNS-i serveri IP-aadressiga.


## <a name="create-vms-for-domain-members"></a>Domeeni liikmete VMs loomine

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


## <a name="see-also"></a>Vt ka

-  [Kuidas installida Azure virtuaalse võrgus uue Active Directory kogumis](http://channel9.msdn.com/Series/Microsoft-Azure-Tutorials/How-to-install-a-new-Active-Directory-forest-on-an-Azure-virtual-network)
-  [Windows Serveri Active Directory Azure'i Virtuaalmasinates juurutamise juhised](https://msdn.microsoft.com/library/azure/jj156090.aspx)

-  [VPN saitide konfigureerimine](../vpn-gateway/vpn-gateway-site-to-site-create.md)
-  [Installige Azure'i virtuaalse võrgu koopia Active Directory domeenikontrolleri](../active-directory/active-directory-install-replica-active-directory-domain-controller.md)
-  [Microsoft Azure'i IT-spetsialistidele IaaS: (01) virtuaalse masina põhialused](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/01)
-  [Microsoft Azure'i IT-spetsialistidele IaaS: (05) loomise virtuaalne võrkude ja asukohaga Ühenduvus](http://channel9.msdn.com/Series/Windows-Azure-IT-Pro-IaaS/05)
-  [Virtuaalse võrgu ülevaade](../virtual-network/virtual-networks-overview.md)
-  [Kuidas installida ja konfigureerida Azure PowerShell](../powershell-install-configure.md)
-  [Azure'i PowerShelli](https://msdn.microsoft.com/library/azure/jj156055.aspx)
-  [Azure'i cmdleti viide](https://msdn.microsoft.com/library/azure/jj554330.aspx)
-  [Azure'i VM staatiline IP-aadress seadmine](http://windowsitpro.com/windows-azure/set-azure-vm-static-ip-address)
-  [Kuidas määrata staatiline IP Azure VM](http://www.bhargavs.com/index.php/2014/03/13/how-to-assign-static-ip-to-azure-vm/)
-  [Uue Active Directory kogumis installimine](https://technet.microsoft.com/library/jj574166.aspx)
-  [Active Directory domeeni Services (AD DS) Virtualization (tase 100) tutvustus](https://technet.microsoft.com/library/hh831734.aspx)

<!--Image references-->
[1]: ./media/active-directory-new-forest-virtual-machine/AD_Forest.png
