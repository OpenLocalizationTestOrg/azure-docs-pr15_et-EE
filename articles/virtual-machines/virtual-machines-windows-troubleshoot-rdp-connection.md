<properties
    pageTitle="Ei saa RDP-Azure'i VM | Microsoft Azure'i"
    description="Kui te ei saa oma Windows Azure'i kaugtöölaua kasutamise virtuaalse masina probleemide tõrkeotsing"
    keywords="Remote'i töölaua viga, kaugtöölaua ühendus viga, ei saa ühendust luua VM Remote'i töölaua tõrkeotsing"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="iainfoulds"
    manager="timlt"
    editor=""
    tags="top-support-issue,azure-service-management,azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="support-article"
    ms.date="10/26/2016"
    ms.author="iainfou"/>

# <a name="troubleshoot-remote-desktop-connections-to-an-azure-virtual-machine"></a>Kaugtöölaua ühendusi on Azure virtuaalse masina tõrkeotsing

Teie Windowsi-põhise Azure arvutiga (VM) Kaugtöölaua protokolli (RDP) ühendus võib nurjuda, jättes te ei pääse juurde teie VM erinevatel põhjustel. Kaugtöölaua teenuse VM, võrguühendus või kaugtöölaua klient hosti arvutis võib olla probleemi. Selles artiklis juhatab teid läbi mõne enamlevinud meetodi RDP ühenduse probleemide lahendamiseks. 

Kui vajate rohkem abi, mis tahes hetkel selle artikli, võtke ühendust [MSDN-i Azure](https://azure.microsoft.com/support/forums/)ja virnas ületäitumise Foorumid Azure eksperdid. Teise võimalusena saate faili Azure'i tugiteenuse taotluse. Minge [Azure tugiteenuste sait](https://azure.microsoft.com/support/options/) ja valige **Saada toetavad**.

<a id="quickfixrdp"></a>

## <a name="quick-troubleshooting-steps"></a>Tõrkeotsingu kiirtoimingud
Tõrkeotsingu iga toimingu järel proovige uuesti, et VM.

1. Kaugtöölaua konfiguratsiooni lähtestada.
2. Kontrollige võrgu turberühma reeglite / Cloud Services lõpp-punktid.
3. Vaadake üle VM konsooli logid.
4. VM ressursi seisundi kontrollimine.
5. VM parool lähtestada.
6. Taaskäivitage oma VM.
7. Juurutage uuesti oma VM.

Kui teil on vaja üksikasjalikud juhised ja selgitused, jätkake lugemist.

> [AZURE.TIP] Kui teie VM nuppu **Loo ühendus** on Hall portaalis ja teil on ühendatud Azure [Teekonna](../expressroute/expressroute-introduction.md) või [- saidilt VPN](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md) -ühenduse kaudu, peate luua ja määrata oma VM avaliku IP-aadressi, enne kui saate kasutada RDP. Lisateavet leiate [Azure'i avaliku IP-aadresside](../virtual-network/virtual-network-ip-addresses-overview-arm.md)kohta.


## <a name="ways-to-troubleshoot-rdp-issues"></a>Viisid RDP probleemide tõrkeotsing
Saate otsida VMs loodud ressursihaldur juurutamise mudeli abil, kasutades ühte järgmistest:

- [Azure'i portaalis](#using-the-azure-portal) - hästi, kui teil on vaja kiiresti lähtestamine RDP konfiguratsiooni või kasutaja mandaat ja te pole installitud Azure tööriistu.
- [Azure'i PowerShelli](#using-azure-powershell) – kui olete rahul PowerShelli käsuviibas, kiiresti lähtestada RDP konfiguratsiooni või kasutaja mandaat Azure PowerShelli cmdlet-käskude abil.

Leiate juhiseid VMs [klassikaline juurutamise mudeli](#troubleshoot-vms-created-using-the-classic-deployment-model)abil loodud tõrkeotsingu kohta.


<a id="fix-common-remote-desktop-errors"></a>
## <a name="troubleshoot-using-the-azure-portal"></a>Tõrkeotsing: Azure'i portaalis
Tõrkeotsingu iga toimingu järel proovige oma VM uuesti ühendust luua. Kui te ikka ei saa ühendust luua, proovige järgmist toimingut.

1. **Lähtesta RDP-ühendus**. See tõrkeotsingu toiming lähtestab RDP konfiguratsiooni allalaadimise on keelatud või Windowsi tulemüüri reeglid blokeerivad RDP, näiteks.

    Valige oma VM Azure portaali. Liikuge kerides allapoole paani sätted jaotisse **tugi + tõrkeotsingu** lähedal loendi lõppu. Klõpsake nuppu **Lähtesta parool** . **Režiimi** lähtestada **ainult konfiguratsiooni** ja seejärel klõpsake nuppu **Värskenda** :

    ![Azure'i portaalis RDP konfiguratsiooni lähtestamine](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-rdp.png)

2. **Kontrollige võrgu turberühma reeglid**. Tõrkeotsingu toimingut kinnitab, et teil on reegli võrgu turberühma lubada RDP liiklust. RDP vaikeport on TCP porti 3389. Reegli lubada RDP liiklust ei saa luua automaatselt teie VM loomisel.

    Valige oma VM Azure portaali. Klõpsake **võrgu liidesed** paanilt sätted.

    ![Kuva võrgu liidesed VM Azure portaalis](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-network-interfaces.png)

    Valige oma võrgu liidese loendist (on tavaliselt ainult üks).

    ![Valige võrgu liidese Azure'i portaalis](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-interface.png)

    Valige **võrgu turberühma** seostatud teie võrgu liidese võrgu turberühma kuvamiseks:

    ![Valige võrgu turberühma Azure'i portaalis](./media/virtual-machines-windows-troubleshoot-rdp-connection/select-nsg.png)

    Veenduge, et sissetulevast olemas, mis lubab TCP-pordi 3389 RDP liiklust. Järgmises näites on kujutatud kehtiv turvalisuse reegli, mis võimaldab RDP liiklust. Saate vaadata `Service` ja `Action` on õigesti konfigureeritud:

    ![Veenduge, et RDP NSG reegli Azure'i portaalis](./media/virtual-machines-windows-troubleshoot-rdp-connection/verify-nsg-rules.png)

    Kui teil on reegel, mis võimaldab RDP liikluse, [võrgu turberühma reegli loomine](virtual-machines-windows-nsg-quickstart-portal.md). Luba TCP porti 3389.

3. **Läbivaatus VM buutimine diagnostika**. Tõrkeotsingu toimingut vaatab läbi VM konsooli logid määratlemiseks, kui VM on teatamine probleemi. Kõik VMs on lubatud, buutimine diagnostika nii, et see tõrkeotsingu samm võib olla valikuline.
    
    Teatud tõrkeotsingujuhiseid ei käsitleta käesoleva artikli, kuid võib viidata laiem probleem, mis mõjutavad RDP Ühenduvus. Konsooli logid ja VM kuvatõmmis läbivaatamise kohta leiate lisateavet teemast [Buutimine diagnostika vms](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

4. **VM ressursi seisundi kontrollimine**. See tõrkeotsingu toiming kinnitab on Azure'i platvormi, mis võivad mõjutada VM Ühenduvus pole teadaolevad probleemid.

    Valige oma VM Azure portaali. Liikuge kerides allapoole paani sätted jaotisse **tugi + tõrkeotsingu** lähedal loendi lõppu. Klõpsake nuppu **ressurss seisund** . Terve VM aruanded on **saadaval**:

    ![VM ressursi seisundi Azure'i portaalis kontrollimine](./media/virtual-machines-windows-troubleshoot-rdp-connection/check-resource-health.png)

5. **Lähtestage kasutaja mandaat**. See tõrkeotsingu toiming lähtestab kohaliku administraatorikonto parooli, kui te pole kindel, või unustanud identimisteabe.

    Valige oma VM Azure portaali. Liikuge kerides allapoole paani sätted jaotisse **tugi + tõrkeotsingu** lähedal loendi lõppu. Klõpsake nuppu **Lähtesta parool** . Veenduge, et **režiim** on seatud **parool** lähtestada, ja seejärel sisestage oma kasutajanimi ja uus parool. Lõpuks nuppu **Värskenda** .

    ![Lähtestage kasutaja mandaat Azure'i portaalis](./media/virtual-machines-windows-troubleshoot-rdp-connection/reset-password.png)

6. **Taaskäivitage oma VM**. Tõrkeotsingu toimingut saate parandada ise VM võttes aluseks probleemidest.

    Valige oma VM Azure portaali ja klõpsake vahekaardil **Ülevaade** . Klõpsake **uuesti** nuppu:

    ![Taaskäivitage VM Azure portaalis](./media/virtual-machines-windows-troubleshoot-rdp-connection/restart-vm.png)

7. **Juurutage uuesti oma VM**. Tõrkeotsingu toimingut redeploys oma VM mõne muu Host Azure'is mis tahes aluseks platvorm või võrgu probleemide lahendamiseks.

    Valige oma VM Azure portaali. Liikuge kerides allapoole paani sätted jaotisse **tugi + tõrkeotsingu** lähedal loendi lõppu. Klõpsake nuppu **Juurutage uuesti** , ja klõpsake **Juurutage uuesti**.

    ![Azure'i portaalis VM ümberkorraldamine](./media/virtual-machines-windows-troubleshoot-rdp-connection/redeploy-vm.png)

    Kui see toiming on lõpule jõudnud, efemeersed ketta andmed lähevad kaotsi ja dünaamiline IP-aadressid, mis on seotud VM on värskendatud.

Kui ilmneb endiselt RDP probleeme, saate [esitada tugiteenuse taotluse avada](https://azure.microsoft.com/support/options/) või lugeda [üksikasjalikumat RDP tõrkeotsingu põhimõtet ja juhiseid](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-using-azure-powershell"></a>Tõrkeotsing: Azure'i PowerShelli abil
Kui te pole seda veel teinud, [installimine ja konfigureerimine uusimate Azure PowerShelli](../powershell-install-configure.md).

Järgmistes näidetes kasutatakse muutujate nagu `myResourceGroup`, `myVM`, ja `myVMAccessExtension`. Oma väärtuste asendamine nende nimed ning asukohad.

> [AZURE.NOTE] [Set-AzureRmVMAccessExtension](https://msdn.microsoft.com/library/mt619447.aspx) PowerShelli cmdleti abil lähtestada kasutaja mandaat ja RDP konfigureerimine. Järgmistes näidetes `myVMAccessExtension` on nimi, mida saate määrata selle käigus. Kui te pole varem kasutanud soovitud VMAccessAgent, saate olemasoleva laiend nimi, kasutades `Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"` VM atribuutide kontrollimiseks. Nime vaatamiseks vaadake väljund jaotises "Laiendid".

Tõrkeotsingu iga toimingu järel proovige oma VM uuesti ühendust luua. Kui te ikka ei saa ühendust luua, proovige järgmist toimingut.

1. **Lähtesta RDP-ühendus**. See tõrkeotsingu toiming lähtestab RDP konfiguratsiooni allalaadimise on keelatud või Windowsi tulemüüri reeglid blokeerivad RDP, näiteks.

    Jälgi näide lähtestab nimega VM RDP-ühendus `myVM` klõpsake soovitud `WestUS` asukoht ja ressursside rühma nimega `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location Westus -Name "myVMAccessExtension"
    ```

2. **Kontrollige võrgu turberühma reeglid**. Tõrkeotsingu toimingut kinnitab, et teil on reegli võrgu turberühma lubada RDP liiklust. RDP vaikeport on TCP porti 3389. Reegli lubada RDP liiklust ei saa luua automaatselt teie VM loomisel.

    Esmalt oma võrgu turberühma, et määrata konfiguratsiooni andmed on `$rules` muutujana. Järgmises näites hangib teavet võrgu turberühma nimega `myNetworkSecurityGroup` ressursirühma nimega `myResourceGroup`:

    ```powershell
    $rules = Get-AzureRmNetworkSecurityGroup -ResourceGroupName "myResourceGroup" `
        -Name "myNetworkSecurityGroup"
    ```

    Nüüd saate vaadata reeglid, mis on konfigureeritud lubama selle võrgu turberühma. Veenduge, et reegli olemas lubamiseks sissetulevate ühenduste TCP porti 3389 järgmiselt:

    ```powershell
    $rules.SecurityRules
    ```

    Järgmises näites on kujutatud kehtiv turvalisuse reegli, mis võimaldab RDP liiklust. Saate vaadata `Protocol`, `DestinationPortRange`, `Access`, ja `Direction` on õigesti konfigureeritud:

    ```powershell
    Name                     : default-allow-rdp
    Id                       : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myNetworkSecurityGroup/securityRules/default-allow-rdp
    Etag                     : 
    ProvisioningState        : Succeeded
    Description              : 
    Protocol                 : TCP
    SourcePortRange          : *
    DestinationPortRange     : 3389
    SourceAddressPrefix      : *
    DestinationAddressPrefix : *
    Access                   : Allow
    Priority                 : 1000
    Direction                : Inbound
    ```

    Kui teil on reegel, mis võimaldab RDP liikluse, [võrgu turberühma reegli loomine](virtual-machines-windows-nsg-quickstart-powershell.md). Luba TCP porti 3389.

3. **Lähtestage kasutaja mandaat**. See tõrkeotsingu toiming lähtestab teie määratud, kui te pole kindel, või unustanud, identimisteabe kohaliku administraatorikonto parooli.

    Esmalt Määrake kasutajanimi ja uus parool mandaat määrates selle `$cred` muutuja järgmiselt:

    ```powershell
    $cred=Get-Credential
    ```

    Nüüd, värskendage oma VM identimisteabe. Järgmises näites värskendab mandaadi kohta nimega VM `myVM` sisse selle `WestUS` asukoht ja ressursside rühma nimega `myResourceGroup`:

    ```powershell
    Set-AzureRmVMAccessExtension -ResourceGroupName "myResourceGroup" `
        -VMName "myVM" -Location WestUS -Name "myVMAccessExtension" `
        -UserName $cred.GetNetworkCredential().Username `
        -Password $cred.GetNetworkCredential().Password
    ```

4. **Taaskäivitage oma VM**. Tõrkeotsingu toimingut saate parandada ise VM võttes aluseks probleemidest.

    Järgmises näites taaskäivitamist nimega VM `myVM` ressursirühma nimega `myResourceGroup`:

    ```powershell
    Restart-AzureRmVM -ResourceGroup "myResourceGroup" -Name "myVM"
    ```

5. **Juurutage uuesti oma VM**. Tõrkeotsingu toimingut redeploys oma VM mõne muu Host Azure'is mis tahes aluseks platvorm või võrgu probleemide lahendamiseks.

    Järgmises näites redeploys nimega VM `myVM` klõpsake soovitud `WestUS` asukoht ja ressursside rühma nimega `myResourceGroup`:

    ```powershell
    Set-AzureRmVM -Redeploy -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

Kui ilmneb endiselt RDP probleeme, saate [esitada tugiteenuse taotluse avada](https://azure.microsoft.com/support/options/) või lugeda [üksikasjalikumat RDP tõrkeotsingu põhimõtet ja juhiseid](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-vms-created-using-the-classic-deployment-model"></a>Klassikaline juurutamise mudeli abil loodud VMs tõrkeotsing

Tõrkeotsingu iga toimingu järel proovige uuesti, et VM.

1. **Lähtesta RDP-ühendus**. See tõrkeotsingu toiming lähtestab RDP konfiguratsiooni allalaadimise on keelatud või Windowsi tulemüüri reeglid blokeerivad RDP, näiteks.

    Valige oma VM Azure portaali. Klõpsake soovitud **... Veel** nuppu ja seejärel klõpsake nuppu **Lähtesta Kaugpöördusteenuse**:

    ![Azure'i portaalis RDP konfiguratsiooni lähtestamine](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-rdp.png)

2.  **Kontrollige pilveteenustega lõpp-punktid**. Tõrkeotsingu toimingut kinnitab, et teil on oma pilveteenustega lubada RDP liiklust lõpp-punktid. RDP vaikeport on TCP porti 3389. Reegli lubada RDP liiklust ei saa luua automaatselt teie VM loomisel.

    Valige oma VM Azure portaali. Lõpp-punktid praegu konfigureeritud oma VM kuvamiseks nuppu **lõpp-punktid** . Veenduge, et lõpp-punktid olemas, mis TCP-pordi 3389 RDP liikluse lubamiseks.
    
    Järgmises näites on kujutatud kehtiv lõpp-punktid, mis võimaldavad RDP liiklust:

    ![Veenduge, et Azure portaali pilveteenustega lõpp-punktid](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-verify-cloud-services-endpoints.png)

    Kui teil on lõpp, mis võimaldab RDP liikluse, [on pilveteenused lõpp-punkti loomine](virtual-machines-windows-classic-setup-endpoints.md). Lubage TCP privaatne porti 3389.

3. **Läbivaatus VM buutimine diagnostika**. Tõrkeotsingu toimingut vaatab läbi VM konsooli logid määratlemiseks, kui VM on teatamine probleemi. Kõik VMs on lubatud, buutimine diagnostika nii, et see tõrkeotsingu samm võib olla valikuline.
    
    Teatud tõrkeotsingujuhiseid ei käsitleta käesoleva artikli, kuid võib viidata laiem probleem, mis mõjutavad RDP Ühenduvus. Konsooli logid ja VM kuvatõmmis läbivaatamise kohta leiate lisateavet teemast [Buutimine diagnostika vms](https://azure.microsoft.com/blog/boot-diagnostics-for-virtual-machines-v2/).

4. **VM ressursi seisundi kontrollimine**. See tõrkeotsingu toiming kinnitab on Azure'i platvormi, mis võivad mõjutada VM Ühenduvus pole teadaolevad probleemid.

    Valige oma VM Azure portaali. Liikuge kerides allapoole paani sätted jaotisse **tugi + tõrkeotsingu** lähedal loendi lõppu. Klõpsake nuppu **Ressurss seisund** . Terve VM aruanded on **saadaval**:

    ![VM ressursi seisundi Azure'i portaalis kontrollimine](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-check-resource-health.png)

5. **Lähtestage kasutaja mandaat**. See tõrkeotsingu toiming lähtestab teie määratud, kui te pole kindel, või unustanud identimisteabe kohaliku administraatorikonto parooli.

    Valige oma VM Azure portaali. Liikuge kerides allapoole paani sätted jaotisse **tugi + tõrkeotsingu** lähedal loendi lõppu. Klõpsake nuppu **Lähtesta parool** . Sisestage oma kasutajanimi ja uus parool. Lõpuks klõpsake nuppu **Salvesta** .

    ![Lähtestage kasutaja mandaat Azure'i portaalis](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-reset-password.png)

6. **Taaskäivitage oma VM**. Tõrkeotsingu toimingut saate parandada ise VM võttes aluseks probleemidest.

    Valige oma VM Azure portaali ja klõpsake vahekaardil **Ülevaade** . Klõpsake **uuesti** nuppu:

    ![Taaskäivitage VM Azure portaalis](./media/virtual-machines-windows-troubleshoot-rdp-connection/classic-restart-vm.png)
    
Kui ilmneb endiselt RDP probleeme, saate [esitada tugiteenuse taotluse avada](https://azure.microsoft.com/support/options/) või lugeda [üksikasjalikumat RDP tõrkeotsingu põhimõtet ja juhiseid](virtual-machines-windows-detailed-troubleshoot-rdp.md).


## <a name="troubleshoot-specific-rdp-errors"></a>RDP tõrgete tõrkeotsing
Võivad ilmneda teatud tõrketeade üritab teie VM RDP kaudu. Järgnevalt on kõige levinumad tõrketeated.

- [Kaugseanss katkestati, kuna puuduvad Remote'i töölaua litsentsi serverid saadaval esitada litsentsi](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdplicense).
- [Kaugtöölaud ei leia arvuti "nimi"](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpname).
- [Autentimise tõrke tõttu. Kohalik turvalisus asutus ei saa ühendust](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpauth).
- [Windowsi turvalisus tõrge: kirjeldamine ei tööta](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#wincred).
- [Kaugarvuti selle arvuti ei saa ühendust luua](virtual-machines-windows-troubleshoot-specific-rdp-errors.md#rdpconnect).


## <a name="additional-resources"></a>Lisaressursid
Kui ükski nende vigade ilmnenud ja te endiselt ei saa ühendust VM kaudu kaugtöölaua, lugege üksikasjalikku [tõrkeotsingujuhendi kaugtöölaua](virtual-machines-windows-detailed-troubleshoot-rdp.md).

- [Azure'i IaaS (Windows) diagnostika pakett](https://home.diagnostics.support.microsoft.com/SelfHelp?knowledgebaseArticleFilter=2976864)
- Tõrkeotsingujuhiseid juurdepääs VM töötavad rakendused, vt [tõrkeotsing Accessi rakendus töötab ka Azure VM](virtual-machines-linux-troubleshoot-app-connection.md).
- Kui teil on probleeme ühendamine Linux VM Azure Secure Shell (SSH) abil, lugege teemat [tõrkeotsing SSH ühenduste Linux VM Azure](virtual-machines-linux-troubleshoot-ssh-connection.md).
