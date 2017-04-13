<properties
    pageTitle="Oma esimese Windows VM IIS-i installimine | Microsoft Azure'i"
    description="Katsetada esimese Windows virtual arvuti, installides IIS-i ja pordi avamine 80 Azure'i portaalis."
    keywords=""
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor=""
    tags="azure-resource-manager"/>
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="cynthn"/>

# <a name="experiment-with-installing-a-role-on-your-windows-vm"></a>Katsetage oma Windows VM rolli installimine
    
Kui olete oma esimese virtuaalse masina (VM) ja töötab, saate liikuda tarkvara installimine ja teenused. Selles õpetuses mõeldud me Server Manager kasutamine Windows Server VM IIS-i installimiseks. Seejärel loome on võrgu turvalisus rühma NSG avamine pordi 80 IIS-i liiklus Azure portaali abil. 

Kui te pole juba loonud oma esimese VM, peate pöörduma uuesti [luua oma esimese Windowsi virtuaalse masina Azure'i portaalis](virtual-machines-windows-hero-tutorial.md) enne jätkamist selles õpetuses.

## <a name="make-sure-the-vm-is-running"></a>Veenduge, et VM töötab

1. Avage [Azure'i portaalis](https://portal.azure.com).
2. Klõpsake menüü jaoturi **Virtuaalmasinates**. Valige loendist virtuaalse masina.
3. Kui olek on **peatatud (Deallocated)**, klõpsake nuppu **Start** , **Essentialsi** enne VM. Kui olek on **töötab**, saate saate liikuda edasi järgmise juhise juurde.

## <a name="connect-to-the-virtual-machine-and-sign-in"></a>Ühenduse loomine virtuaalse masina ja sisselogimine

1.  Klõpsake menüü jaoturi **Virtuaalmasinates**. Valige loendist virtuaalse masina.

3. Enne virtuaalse masina jaoks, klõpsake nuppu **Ühenda**. See loob ja laadib Kaugtöölaua protokolli faili (RDP-faili), mis on nagu otsetee loomiseks arvuti. Võite salvestage fail oma töölauale hõlpsaks juurdepääsuks. **Avage** seda faili ühenduse loomiseks oma VM.

    ![Näitab, kuidas luua ühendus oma VM Azure portaali kuvatõmmis](./media/virtual-machines-windows-hero-tutorial/connect.png)

4. Kuvatakse hoiatus, et selle RDP pärineb tundmatu tootja. See on tavaline. Klõpsake aknas kaugtöölaua **ühendus** jätkata.

    ![Kuvatõmmis hoiatust tundmatu tootja](./media/virtual-machines-windows-hero-tutorial/rdp-warn.png)

5. Tippige aknas Windowsi turvalisus kasutajanime ja parooli loomisel VM loodud kohaliku konto. Kasutajanimi sisestatakse *vmname*& #92; *kasutajanimi*ja seejärel klõpsake nuppu **OK**.

    ![Kuvatõmmis sisestamise VM nimi, kasutajanimi ja parool](./media/virtual-machines-windows-hero-tutorial/credentials.png)
    
6.  Kuvatakse hoiatus, et serti ei saa kontrollida. See on tavaline. Klõpsake nuppu **Jah** virtuaalse masina identiteedi ja lõpuleviimiseks sisselogimist.

    ![Kuvatõmmis sõnumi Lisateavet identiteedi VM](./media/virtual-machines-windows-hero-tutorial/cert-warning.png)


Kui teil tekib probleeme kui proovite ühendust luua, lugege teemat [tõrkeotsing kaugtöölaua ühenduse abil Windowsi-põhiste Azure virtuaalse masina](virtual-machines-windows-troubleshoot-rdp-connection.md).


## <a name="install-iis-on-your-vm"></a>Teie VM IIS-i installimine

Nüüd, kui teil on sisse logitud VM, me serveri roll installida nii, et saate veel proovida.

1. Kui see pole juba avatud, siis avage **Serveri haldur** . Klõpsake menüü **Start** , ja klõpsake **Serveri haldur**.
2. Valige **Kohalik Server** **Server Manager**vasakul paanil. 
3. Valige menüü **Halda** > **lisada rollid ja funktsioonid**.
4. Rollide ja funktsioonide viisardi lehel **Installi tüübi** , valige **rolli või funktsiooni installimise**ja seejärel klõpsake nuppu **edasi**.

    ![Kuvatõmmis vahekaarti lisada rollid ja funktsioonid viisardi installi tüübi jaoks](./media/virtual-machines-windows-hero-tutorial/role-wizard.png)

5. Valige VM kaustast server ja klõpsake nuppu **edasi**.
6. Valige lehel **Serverirollide** **Web Server (IIS)**.

    ![Kuvatõmmis vahekaarti lisada rollid ja funktsioonid viisardi serverirollid](./media/virtual-machines-windows-hero-tutorial/add-iis.png)

7. Klõpsata hüpikaknas olevat puudutava funktsioonid on vaja IIS-i, veenduge, et **kaasata Haldusriistad** oleks märgitud ja seejärel käsku **Lisamine funktsioonid**. Kui hüpikaknas sulgeb, klõpsake nuppu **Järgmine** viisardi juhiseid.

    ![Hüpikakende kinnitamiseks IIS-i roll lisamise kuvatõmmis](./media/virtual-machines-windows-hero-tutorial/confirm-add-feature.png)

8. Klõpsake lehel funktsioonid nuppu **Järgmine**.
9. Klõpsake lehel **Web Server rolli (IIS)** klõpsake nuppu **edasi**. 
10. **Roll teenuste** lehel, klõpsake nuppu **edasi**. 
11. Klõpsake **kinnitamise** lehel nuppu **Installi**. 
12. Kui installimine on lõpule jõudnud, klõpsake **Sule** viisard.



## <a name="open-port-80"></a>Avage port 80 

Et oma VM aktsepteerimiseks sissetulev liiklus port 80, peate lisama sissetulevast võrgu turberühma. 

1. Avage [Azure'i portaalis](https://portal.azure.com).
2. Valige **virtuaalmasinates** loodud VM.
3. Virtuaalmasinates sätteid, valige **võrgu liidesed** ja seejärel valige olemasoleva võrgu liidese.

    ![Kuvatõmmis virtuaalse masina sätet võrgu liidesed](./media/virtual-machines-windows-hero-tutorial/network-interface.png)

4. Klõpsake **Essentialsi** võrgu kasutajaliidese **võrgu turberühma**.

    ![Kuvatõmmis Essentialsi jaotises võrku kasutajaliides](./media/virtual-machines-windows-hero-tutorial/select-nsg.png)

5. Funktsiooni NSG **Essentialsi** labale peaks teil olema üks olemasoleva vaikimisi Sissetulev reegel jaoks **vaikimisi luba rdp** , mis võimaldab teil sisse logida VM. Lisate mõne muu Sissetulev reegel IIS-i liikluse lubamiseks. Klõpsake **sissetuleva turvalisuse reegel**.

    ![Kuvatõmmis jaotise Essentialsi on NSG](./media/virtual-machines-windows-hero-tutorial/inbound.png)

6. **Sissetulevad reeglid turvalisus**, klõpsake nuppu **Lisa**.

    ![Kuvatõmmis, kus on kuvatud nupp Turve reegli lisamine](./media/virtual-machines-windows-hero-tutorial/add-rule.png)

7. **Sissetulevad reeglid turvalisus**, klõpsake nuppu **Lisa**. Tippige vahemikust portide **80** ja veenduge, et oleks valitud suvand **Luba** . Kui olete lõpetanud, klõpsake nuppu **OK**.

    ![Kuvatõmmis, kus on kuvatud nupp Turve reegli lisamine](./media/virtual-machines-windows-hero-tutorial/port-80.png)
 
Lisateavet NSGs sissetulevate ja väljaminevate reegleid, lugege teemat [Luba välise juurdepääsu oma VM Azure portaalis](virtual-machines-windows-nsg-quickstart-portal.md)
 
## <a name="connect-to-the-default-iis-website"></a>Ühenduse loomine IIS-i Vaikeveebisait

1. Azure'i portaalis **virtuaalmasinates** nuppu ja seejärel valige oma VM.
2. Kopeerige **Essentialsi** labale oma **avaliku IP-aadress**.

    ![Kust leida oma VM avaliku IP-aadress kuvatõmmis](./media/virtual-machines-windows-hero-tutorial/ipaddress.png)

2. Avage brauser ja tippige aadressiribale avaliku IP-aadressi umbes järgmine: http://<publicIPaddress> ja klõpsake nuppu **Enter** selle aadressile.
3. Brauseris avatakse vaikimisi IIS-i veebileht. See näeb välja umbes selline:

    ![Kuvatõmmis, mis näitab, milline IIS-i vaikelehe näeb brauseris](./media/virtual-machines-windows-hero-tutorial/iis-default.png)

    

## <a name="next-steps"></a>Järgmised sammud

- Võite ka katsetada [manustamise andmed kettale](virtual-machines-windows-attach-disk-portal.md) teie arvutis virtual. Andmete ketast esitada oma virtuaalse masina rohkem ruumi.
