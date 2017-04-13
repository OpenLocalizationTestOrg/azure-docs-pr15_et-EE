<properties
   pageTitle="Azure'i VM varundus nurjub: ei saanud suhelda VM agent hetktõmmise olek – hetktõmmis VM sub tööülesande aegus | Microsoft Azure'i"
   description="Sümptomid põhjuseid ja lahendusi Azure VM varukoopia ebaõnnestumist seotud ei saanud suhelda VM agent hetktõmmise olek. Hetktõmmis VM sub tööülesande aegus tõrge"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="cfreeman"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jimpark; markgal;genli"/>

# <a name="azure-vm-backup-fails-could-not-communicate-with-the-vm-agent-for-snapshot-status---snapshot-vm-sub-task-timed-out"></a>Azure'i VM varundus nurjub: ei saanud suhelda VM agent hetktõmmise olek – hetktõmmis VM sub tööülesande aegus

## <a name="summary"></a>Kokkuvõte

Pärast registreerumist ja plaanimine Azure varukoopia virtuaalse masina (VM), Azure varukoopia teenuse alustab Varundustöö määratud ajal suheldes varukoopia laiend VM punkti /-kellaaja hetktõmmis. On tingimused, mis võivad takistada hetktõmmis vallandanud, mis viib varukoopia tõrge. Sellest artiklist leiate Azure'i VM varukoopia ebaõnnestumist seotud hetktõmmise ajalõpp seotud probleemide tõrkeotsingu juhiseid.

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>Sümptom

Microsoft Azure varukoopia infrastruktuuri teenus (IaaS) VM nurjub ja tagastab töö tõrke üksikasjade [Azure'i portaalis](https://portal.azure.com/)kuvatakse järgmine tõrketeade:

**Ei saanud suhelda VM agent hetktõmmise olek – hetktõmmis VM sub tööülesande aegus.**

## <a name="cause"></a>Põhjus
On neli levinud põhjused.

- VM ei ole Interneti-ühendus.
- Microsoft Azure'i VM VM installitud on aegunud (jaoks Linux VMs).
- Varukoopia laiend värskendada või laadimine nurjub.
- Hetktõmmiste olek ei saa tuua või tõmmised ei saa tagasi võtta.

## <a name="cause-1-the-vm-does-not-have-internet-access"></a>Põhjus 1: VM ei ole Interneti-ühendus
Juurutamise nõue, VM on Interneti-ühendus, või on Azure infrastruktuuri takistavad kohas piirangud.

Varukoopia laiend jaoks on vaja ühenduvust Azure avaliku IP-aadresse töötaks õigesti. Seda sellepärast, et see saadab käske (HTTP URL) haldamiseks tõmmised VM Azure Storage lõpp-punkti. Kui laiendamine ei ole avaliku Interneti-ühendus, varundamine lõpuks nurjub.

### <a name="solution"></a>Lahendus
Selle stsenaariumi korral kasutada üht järgmistest meetoditest probleemi lahendamiseks:

- Valge Azure andmekeskuse IP-vahemikke
- Saate luua HTTP-liikluse meilivoo jaoks tee

### <a name="to-whitelist-the-azure-datacenter-ip-ranges"></a>Nimekirja nimekiri selle Azure'i andmekeskuse IP-vahemikke

1. Saada whitelisted olevat [loendit Azure andmekeskuse IP-d](https://www.microsoft.com/download/details.aspx?id=41653) .
2. Blokeeringu kogumüügist i New-NetRoute cmdleti abil. Käivitada Azure'i VM administraatoriõigustes PowerShelli aknas (Käivita administraatorina).
3. Reeglite lisamine võrgu turvalisus jaotises (NSG) Kui kasutasite funktsiooni IP-d juurdepääsu lubamine.

### <a name="to-create-a-path-for-http-traffic-to-flow"></a>HTTP-liikluse meilivoo jaoks tee loomiseks

1. Kui teil on võrgu piirangute kohta (nt NSG), juurutada abil saate marsruutida liiklust HTTP-puhverserver.
2. Kui teil on võrgu turberühma (NSG), lisage reeglid, mis võimaldab juurdepääsu Interneti HTTP-puhverserver.

Siit saate teada, kuidas [häälestada VM varufailide HTTP-puhverserver](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups).

## <a name="cause-2-the-microsoft-azure-vm-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>Põhjus 2: Microsoft Azure VM VM installitud on aegunud (jaoks Linux VMs)

### <a name="solution"></a>Lahendus
Kõige agendi või laiend seotud tõrgete Linux vms põhjustavad probleemid, mis mõjutavad old VM agent. Kui üldiselt esimesed sammud selle probleemi tõrkeotsinguks on järgmised.

1. [Installige uusim Azure VM agent](https://github.com/Azure/WALinuxAgent).
2. Veenduge, et Azure'i agent töötab VM. Selleks, käivitage järgmine käsk:```ps -e```

    Kui see toiming ei tööta, kasutada järgmisi käske taaskäivitage see.

    Ubuntu:```service walinuxagent start```

    Muud jaotuse jaoks: "' teenuse waagent käivitamine
```

3. [Configure the auto restart agent](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).

4. Run a new test backup. If the failures persist, please collect logs from the following folders for further debugging.

    We require the following logs from the customer’s VM:

    - /var/lib/waagent/*.xml
    - /var/log/waagent.log
    - /var/log/azure/*

If we require verbose logging for waagent, follow these steps to enable this:

1. In the /etc/waagent.conf file, locate the following line:

    Enable verbose logging (y|n)

2. Change the **Logs.Verbose** value from n to y.
3. Save the change, and then restart waagent by following the previous steps in this section.

## Cause 3: The backup extension fails to update or load
If extensions cannot be loaded, then Backup fails because a snapshot cannot be taken.

### Solution
For Windows guests:

1. Verify that the iaasvmprovider service is enabled and has a startup type of automatic.
2. If this is not the configuration, enable the service to determine whether the next backup succeeds.

For Linux guests:

The latest version of VMSnapshot Linux (extension used by backup) is 1.0.91.0

If the backup extension still fails to update or load, you can force the VMSnapshot extension to be reloaded by uninstalling the extension. The next backup attempt will reload the extension.

### To uninstall the extension

1. Go to the [Azure portal](https://portal.azure.com/).
2. Locate the particular VM that has backup problems.
3. Click **Settings**.
4. Click **Extensions**.
5. Click **Vmsnapshot Extension**.
6. Click **uninstall**.

This will cause the extension to be reinstalled during the next backup.

## Cause 4: The snapshots status cannot be retrieved or the snapshots cannot be taken
VM backup relies on issuing snapshot command to underlying storage. The backup can fail because it has no access to storage or because of a delay in snapshot task execution.

### Solution
The following conditions can cause snapshot task failure:

| Cause | Solution |
| ----- | ----- |
| VMs that have Microsoft SQL Server Backup configured. By default, VM Backup runs a VSS Full backup on Windows VMs. On VMs that are running SQL Server-based servers and on which SQL Server Backup is configured, snapshot execution delays may occur. | Set following registry key if you are experiencing backup failures because of snapshot issues.<br><br>[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE" |
| VM status is reported incorrectly because the VM is shut down in RDP. If you shut down the virtual machine in RDP, check the portal to determine whether that VM status is reflected correctly. | If it’s not, shut down the VM in the portal by using the ”Shutdown” option in the VM dashboard. |
| Many VMs from the same cloud service are configured to back up at the same time. | It’s a best practice to spread out the VMs from the same cloud service to have different backup schedules. |
| The VM is running at high CPU or memory usage. | If the VM is running at high CPU usage (more than 90 percent) or high memory usage, the snapshot task is queued and delayed and eventually times out. In this situation, try on-demand backup. |
|The VM cannot get host/fabric address from DHCP.|DHCP must be enabled inside the guest for IaaS VM Backup to work.  If the VM cannot get host/fabric address from DHCP response 245, then it cannot download ir run any extensions. If you need a static private IP, you should configure it through the platform. The DHCP option inside the VM should be left enabled. View more information about [Setting a Static Internal Private IP](../virtual-network/virtual-networks-reserved-private-ip.md).|
