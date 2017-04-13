<properties
    pageTitle="Ettevalmistused Windows VHD üleslaadimiseks Azure | Microsoft Azure'i"
    description="Soovitatav tavad Windows VHD ettevalmistamine enne Azure'i üleslaadimine"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="genlin"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/18/2016"
    ms.author="glimoli;genli"/>

# <a name="prepare-a-windows-vhd-to-upload-to-azure"></a>Ettevalmistused Windows VHD Azure'i üleslaadimine
Üles laadida Windows VM asutusesisesest Azure, peate õigesti ette virtuaalse kõvaketta (VHD). On mitu soovituslikku toimingut enne on VHD üleslaadimine Azure'i lõpetamiseks. Selles artiklis kirjeldatakse, kuidas valmistada Windowsi VHD Microsoft Azure'i üleslaadimine ja samuti selgitatakse, [Millal ja kuidas kasutada Sysprepi](#step23).

## <a name="prepare-the-virtual-disk"></a>Virtuaalse ketta ettevalmistamine

>[AZURE.NOTE] 
> Ainult toetab Azure'i [virtuaalmasinates genereerimine 1](http://blogs.technet.com/b/ausoemteam/archive/2015/04/21/deciding-when-to-use-generation-1-or-generation-2-virtual-machines-with-hyper-v.aspx) , mis on VHD failivormingus. Azure'i ei toeta VHDX uude vormingusse. 
>
> Funktsiooni VHD peab olema kindlaksmääratud suurusega, mitte dünaamiline. Vajadusel alltoodud juhiseid üksikasjalikult teisendamine VHDX või dünaamiline kettale. Funktsiooni VHD lubatud maksimaalmahu on 1023 GB.


Veenduge, et Windows VHD töötab õigesti kohalikus serveris. Lahendada vigu sees VM ise enne teisendada või Azure üles laadida.

Kui teil on vaja oma virtuaalse ketta teisendamine nõutav vorm Azure, kasutage ühte järgmistes jaotistes toodud meetodid. Enne mis tahes virtuaalse ketta teisendamine või Sysprep VM varundada.

### <a name="convert-using-hyper-v-manager"></a>Teisendamine Hyper-V halduri kasutamine
- Avage Hyper-V halduri ja valige vasakul kohalikus arvutis. Selle kohal menüüs nuppu **toiming** > **Redigeerimine ketas**.
    - **Leidke virtuaalse kõvaketta** ekraanil, otsige sirvides üles ja valige oma virtuaalse ketta.
    - Valige järgmisel kuval **teisendamine**
        - Kui teil on vaja teisendada VHDX, valige **VHD** ja klõpsake nuppu **edasi**
        - Kui teil on vaja teisendada dünaamiline ketas, valige **kindlaksmääratud suurusega** , ja klõpsake nuppu **edasi**

    - Otsige sirvides üles ja valige **Uus VHD faili tee**.
    - Klõpsake nuppu **valmis** sulgemiseks.

### <a name="convert-using-powershell"></a>Teisendamine PowerShelli abil
Virtuaalse ketta [teisendamine-VHD PowerShelli cmdlet-käsu](http://technet.microsoft.com/library/hh848454.aspx)abil saate teisendada. Järgmises näites me on VHD teisendamisel mõne VHDX ja dünaamiline teisendamisel fikseeritud tüüp:

```powershell
Convert-VHD –Path c:\test\MY-VM.vhdx –DestinationPath c:\test\MY-NEW-VM.vhd -VHDType Fixed
```

### <a name="convert-from-vmware-vmdk-disk-format"></a>VMware VMDK kettale vormingust teisendamine
Kui teil on Windows VM pilt [VMDK failivormingus](https://en.wikipedia.org/wiki/VMDK), muuta see on VHD [Microsoft Virtual masina muunduri](https://www.microsoft.com/download/details.aspx?id=42497)abil. Lisateavet [Kuidas teisendada VMware VMDK Hyper-V VHD](http://blogs.msdn.com/b/timomta/archive/2015/06/11/how-to-convert-a-vmware-vmdk-to-hyper-v-vhd.aspx) ajaveebi lugemine.

## <a name="prepare-windows-configuration-for-upload"></a>Ettevalmistused Windows konfiguratsiooni üleslaadimine

> [AZURE.NOTE] Käivitage järgmised käsud [administraatoriõigustega](https://technet.microsoft.com/library/cc947813.aspx).

1. Marsruutimise tabeli mis tahes staatilise püsivate marsruutimiseks eemaldamiseks tehke järgmist.

    - Selleks, et vaadata route tabel, `route print`.
    - Märkige ruut **Püsimine marsruudib** jaotised. Kui püsivate marsruutimiseks, kasutage selle eemaldada [marsruutimiseks kustutamine](https://technet.microsoft.com/library/cc739598.apx) .

2. WinHTTP puhverserveri eemaldamiseks tehke järgmist.

    ```
    netsh winhttp reset proxy
    ```

3. Konfigureerige ketta SAN poliitika [Onlineall](https://technet.microsoft.com/library/gg252636.aspx).

    ```
    diskpart san policy=onlineall
    ```

4. Windowsi jaoks mõeldud koordineeritud maailmaaega (UTC) aja kasutamine ja seatud Windowsi aeg (w32time) teenuse Käivitustüüp **automaatselt**.

    ```
    REG ADD HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
    sc config w32time start= auto
    ```


## <a name="configure-windows-services"></a>Windowsi teenuste konfigureerimine
5. Veenduge, et kõik järgmised Windows teenused on määratud **Windowsi vaikeväärtused**. Need on konfigureeritud Käivitussätted, märkida järgmises loendis. Need käsud lähtestamiseks käivitamise sätted saate kasutada.

    ```
    sc config bfe start= auto

    sc config dcomlaunch start= auto

    sc config dhcp start= auto

    sc config dnscache start= auto

    sc config IKEEXT start= auto

    sc config iphlpsvc start= auto

    sc config PolicyAgent start= demand

    sc config LSM start= auto

    sc config netlogon start= demand

    sc config netman start= demand

    sc config NcaSvc start= demand

    sc config netprofm start= demand

    sc config NlaSvc start= auto

    sc config nsi start= auto

    sc config RpcSs start= auto

    sc config RpcEptMapper start= auto

    sc config termService start= demand

    sc config MpsSvc start= auto

    sc config WinHttpAutoProxySvc start= demand

    sc config LanmanWorkstation start= auto

    sc config RemoteRegistry start= auto
    ```


## <a name="configure-remote-desktop-configuration"></a>Kaugtöölaua konfiguratsiooni konfigureerimine
6. Kui leidub iseallkirjastatud serdid, mis on seotud Kaugtöölaua protokolli (RDP) kuulajale, eemaldage need:

    ```
    REG DELETE "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\SSLCertificateSHA1Hash”
    ```

    Serdid RDP kuulajale konfigureerimise kohta leiate lisateavet teemast [Kuulajale serdi konfiguratsioone Windows Server](https://blogs.technet.microsoft.com/askperf/2014/05/28/listener-certificate-configurations-in-windows-server-2012-2012-r2/)

7. Konfigureerige RDP teenuse [KeepAlive](https://technet.microsoft.com/library/cc957549.aspx) väärtused.

    ```
    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveEnable /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v KeepAliveInterval /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\Winstations\RDP-Tcp" /v KeepAliveTimeout /t REG_DWORD /d 1 /f
    ```

8. Konfigureerige autentimisrežiim RDP teenuse kohta.

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v UserAuthentication /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v SecurityLayer /t REG_DWORD  /d 1 /f

    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v fAllowSecProtocolNegotiation /t REG_DWORD  /d 1 /f
    ```

9. Lisades järgmistest alamvõtmetest registri RDP teenuse lubamiseks tehke järgmist.

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v fDenyTSConnections /t REG_DWORD  /d 0 /f
    ```


## <a name="configure-windows-firewall-rules"></a>Windowsi tulemüüri reeglite konfigureerimine
10. Luba WinRM kuni kolm tulemüüri profiilid (domeeni, Privaatne ja avalikkusele) ja PowerShell Remote teenuse.

    ```
    Enable-PSRemoting -force
    ```

11. Veenduge, et järgmised Külastajate operatsioonisüsteemi tulemüüri reeglid on olemas.

    - Sissetulev

    ```
    netsh advfirewall firewall set rule dir=in name="File and Printer Sharing (Echo Request - ICMPv4-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (LLMNR-UDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Datagram-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (NB-Name-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (Pub-WSD-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (SSDP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (UPnP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Network Discovery (WSD EventsSecure-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes

    netsh advfirewall firewall set rule dir=in name="Windows Remote Management (HTTP-In)" new enable=yes
    ```

    - Sissetulevate ja väljaminevate

    ```
    netsh advfirewall firewall set rule group="Remote Desktop" new enable=yes

    netsh advfirewall firewall set rule group="Core Networking" new enable=yes
    ```

    - Väljamineva meili

    ```
    netsh advfirewall firewall set rule dir=out name="Network Discovery (LLMNR-UDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Datagram-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (NB-Name-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (Pub-WSD-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (SSDP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnPHost-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (UPnP-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD Events-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD EventsSecure-Out)" new enable=yes

    netsh advfirewall firewall set rule dir=out name="Network Discovery (WSD-Out)" new enable=yes
    ```


## <a name="additional-windows-configuration-steps"></a>Windowsi konfiguratsiooni lisatoimingud
12. Käivitage `winmgmt /verifyrepository` Windows Management Instrumentation (WMI) hoidla vastab kinnitamiseks. Kui hoidla on rikutud, vt [ajaveebipostitust](https://blogs.technet.microsoft.com/askperf/2014/08/08/wmi-repository-corruption-or-not).

13. Veenduge, et buutimine konfiguratsiooni andmete (BCD) sätted vastavad järgmist:

    ```
    bcdedit /set {bootmgr} integrityservices enable

    bcdedit /set {default} device partition=C:

    bcdedit /set {default} integrityservices enable

    bcdedit /set {default} recoveryenabled Off

    bcdedit /set {default} osdevice partition=C:

    bcdedit /set {default} bootstatuspolicy IgnoreAllFailures
    ```

14. Eemaldage eest Transport kasutajaliidest filtrid, näiteks tarkvara, mis analüüsib TCP-pakettide sellise.
15. Veenduge, et ketas on terve ja terviklik, käivitage selle `CHKDSK /f` käsk.
16. Desinstallige muude tootjate tarkvara ja draiver seotud füüsilise komponendid või muu virtualization tehnoloogia.
17. Veenduge, et kolmanda osapoole rakendus ei kasuta Port 3389. Selle pordi kasutatakse RDP teenuse Azure. Saate kasutada funktsiooni `netstat -anob` käsk pordid, mida kasutate ning rakendustega.
18. Kui domeenikontrolleri Windows VHD, mille soovite üles laadida, järgige [neid lisatoimingud](https://support.microsoft.com/kb/2904015) ettevalmistamiseks ketas.
19. Taaskäivitage arvuti veenduge, et Windows on endiselt terve VM pääseb RDP ühenduse abil.
20. Praeguse kohaliku administraatori parooli lähtestamine ja veenduge, et saate selle kontoga sisse logima Windows RDP-ühenduse kaudu.  See juurdepääsu õigus kontrollib "Luba Logi sisse – kaugtöölaua teenuste" rühmapoliitika objekti. See objekt asub jaotises "Arvuti Configuration\Windows Settings\Security sätted\Kohalikud Policies\User õiguste määramine."


## <a name="install-windows-updates"></a>Windowsi värskenduste installimine
22. Uusimate värskenduste installimine Windowsi jaoks. Kui see pole võimalik, veenduge, et järgmised värskendused on installitud.

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure'i VMs ei taastamine võrgu katkestuste ja andmete riknemise probleemid ilmnevad

    - [KB3115224](https://support.microsoft.com/kb/3115224) Töökindluse täiustamine vms, kus töötab Windows Server 2012 R2 või Windows Server 2012

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Turvalisus värskendus Microsoft Windowsi õiguste aadress laiendamist: 8 märts 2016

    - [KB3063075](https://support.microsoft.com/kb/3063075) Microsoft Azure'i Windows Server 2012 R2 virtuaalse masina käivitamisel, logitakse palju ID 129 sündmused

    - [KB3137061](https://support.microsoft.com/kb/3137061) Microsoft Azure'i VMs ei taastamine võrgu katkestuste ja andmete riknemise probleemid ilmnevad

    - [KB3114025](https://support.microsoft.com/kb/3114025) Azure'i failide salvestusruumi juurdepääsuks Windows 8.1 või Server 2012 R2 aeglane jõudlus

    - [KB3033930](https://support.microsoft.com/kb/3033930) Kiirparandus suurendab 64K limiit RIO puhvrid protsessi Windows Azure'i teenuse kohta

    - [KB3004545](https://support.microsoft.com/kb/3004545) Te ei pääse läbi VPN-ühenduse Windows Azure'i majutusteenuste majutatud virtuaalmasinates

    - [KB3082343](https://support.microsoft.com/kb/3082343) Rist virtuaalse Privaatvõrgu ühendus läheb kaotsi, kui Azure-saidilt VPN tunnelite kasutada Windows Server 2012 R2 RRAS

    - [KB3140410](https://support.microsoft.com/kb/3140410) MS16-031: Turvalisus värskendus Microsoft Windowsi õiguste aadress laiendamist: 8 märts 2016

    - [KB3146723](https://support.microsoft.com/kb/3146723) MS16-048: CSRSS turvalisuse värskenduse kirjeldus: 12 aprill 2016
    - [KB2904100](https://support.microsoft.com/kb/2904100) Süsteem hangub ajal ketta I/O Windowsis<a id="step23"></a>
23. Kui soovite luua juurutada mitme masinad see pilt, peate üldistada pilt, käitades `sysprep` enne selle VHD üleslaadimine Azure. Teil pole vaja käivitamiseks `sysprep` eriotstarbelisi VHD kasutamise kohta. Üldise pildi loomise kohta lisateabe saamiseks lugege järgmisi artikleid:

    - [Mõne olemasoleva Azure VM ressursihaldur juurutamise näidise loomine VM pilt](virtual-machines-windows-create-vm-generalized.md)
    - [Mõne olemasoleva Azure VM klassikaline juurutamise modemi abil luua VM pilt](virtual-machines-windows-classic-capture-image.md)
    - [Sysprep serverirollide tugi](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)



## <a name="suggested-extra-configurations"></a>Soovitatud eest konfiguratsioone

Järgmised sätted ei mõjuta VHD üles. Siiski soovitame, et teil on neid konfigureeritud.

- Installige [Azure'i Virtuaalmasinates Agent](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409). Pärast installimist agent, saate lubada VM laiendid. VM laiendid rakendada enamik kriitilise tähtsusega funktsioone, mida soovite kasutada koos oma VMs lähtestada paroole, nt konfigureerimise RDP ja paljud teised.

- Dump Logi võib olla kasulik Windows krahhi tõrkeotsing. Dump Logi saidikogumi lubamiseks tehke järgmist.

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\CrashControl" /v CrashDumpEnabled /t REG_DWORD /d 2 /f`

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpFolder /t REG_EXPAND_SZ /d "c:\CrashDumps" /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpCount /t REG_DWORD /d 10 /f

    REG ADD "HKLM\SOFTWARE\Microsoft\Windows\Windows Error Reporting\LocalDumps" /v DumpType /t REG_DWORD /d 2 /f

    sc config wer start= auto
    ```

- Pärast loomist Azure VM drive D: määratletud suurus pagefile konfigureerimine

    ```
    REG ADD "HKLM\SYSTEM\CurrentControlSet\Control\Session Manager\Memory Management" /t REG_MULTI_SZ /v PagingFiles /d "D:\pagefile.sys 0 0" /f
    ```


## <a name="next-steps"></a>Järgmised sammud

- [Ressursihaldur juurutuste Azure'i Windows VM pildi üleslaadimine](virtual-machines-windows-upload-image.md)
