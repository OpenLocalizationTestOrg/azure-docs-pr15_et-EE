<properties
    pageTitle="Kuidas luua kohandatud malli pilti Azure RemoteApp | Microsoft Azure'i"
    description="Siit saate teada, kuidas luua kohandatud malli pilti Azure RemoteApp. Saate selle malli hübriid või pilveteenuse saidikogumi."
    services="remoteapp"
    documentationCenter=""
    authors="lizap"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="remoteapp"
    ms.workload="compute"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016" 
    ms.author="elizapo"/>

# <a name="how-to-create-a-custom-template-image-for-azure-remoteapp"></a>Kuidas luua kohandatud malli pilti Azure RemoteApp

> [AZURE.IMPORTANT]
> Azure RemoteApp katkeb. Lugege lisateavet [teadaanne](https://go.microsoft.com/fwlink/?linkid=821148) .

Azure RemoteApp kasutab Windows Server 2012 R2 malli pilti majutada kõik programmid, mida soovite oma kasutajatega ühiselt kasutada. Luua kohandatud RemoteApp malli pilti, saate alustada olemasoleva pildi või looge uus. 


> [AZURE.TIP] Kas teadsite, et saate luua pilt on Azure VM? True lugu ja selle lõikab aeg, klõpsake selle võtab importimiseks pilt. Vaadake juhiseid [siin](remoteapp-image-on-azurevm.md).

Pilt, mida saab kasutada Azure RemoteAppi üles laadida nõuded on:


- Pildi suurus peaks olema kordne MB. Kui proovite üles pilt, mis ei ole, nurjub üles.
- Pildi suurus peab olema 127 GB või väiksem.
- See peab olema VHD faili (VHDX failide [Hyper-V virtuaalne kõvaketas] ei toeta praegu).
- Funktsiooni VHD ei tohi genereerimine 2 virtuaalse masina.
- Funktsiooni VHD võib olla fikseeritud suurus või dünaamiliselt laiendada. Dünaamiliselt laiendada VHD on soovitatav, kuna kulub vähem aega Azure kui fikseeritud suurus VHD faili üles laadida.
- Ketas tuleb lähtestada abil soovitud juhtslaidi buutimine Record (MBR) eraldamine laad. GUID partition (GPT) partition tabelilaadi ei toetata.
- Funktsiooni VHD peab sisaldama Windows Server 2012 R2 üks install. See võib olla mitu draivi, kuid ainult ühe, mis sisaldab Windowsi installi.
- Kaugtöölaua seansi hosti (RDSH) roll ja Töölauakogemuse funktsioon peab olema installitud.
- Roll Remote'i töölaua ühenduse ta peab *ei* saanud installida.
- Krüptitud (EFS) peab olema keelatud.
- Pilt peab olema SYSPREPed abil parameetrid **/OOBE / generalize /shutdown** (Ärge kasutage parameetrit **/mode:vm** ).
- Üleslaadimise oma VHD hetktõmmise ahelas kaudu ei toetata.


**Enne alustamist**

Peate tegema enne loomine teenuse järgmist:

- RemoteApp [registreeruda](https://azure.microsoft.com/services/remoteapp/) .
- Kasutajakonto loomine Active Directory RemoteApp teenusekonto kasutamiseks. Piira õigusi selle konto nii, et see ainult liituda masinad domeeni. Lisateabe saamiseks vaadake [Konfigureerimine Azure Active Directory jaoks RemoteApp](remoteapp-ad.md) .
- Koguge teavet kohapealse võrgu: IP-aadress ja VPN seadme üksikasju.
- Installige [Azure'i PowerShelli](../powershell-install-configure.md) moodul.
- Koguge teavet kasutajatele, mille soovite anda juurdepääsu. See võib olla Microsofti konto teavet või Active Directory töö kontoteave kasutajate jaoks.



## <a name="create-a-template-image"></a>Luua malli pilti ##

Need on kõrge taseme juhised loomiseks algusest peale ise luua uue malli pilti:

1.  Windows Server 2012 R2 Update DVD või ISO pildini.
2.  VHD faili loomine.
4.  Installige Windows Server 2012 R2.
5.  Installige kaugtöölaua seansi hosti (RDSH) roll ja Töölauakogemuse funktsioon.
6.  Lisafunktsioone nõutud rakenduste installimine.
7.  Installima ja konfigureerima rakenduste. Rakenduste ühiskasutuse hõlbustamiseks lisada kõik rakendused ja programmid, mida soovite ühiskasutusse anda pildi, spetsiaalselt **%systemdrive%\ProgramData\Microsoft\Windows\Start MS menüüsse **Start** .
8.  Mis tahes täiendavaid Windowsi konfiguratsioone nõutud rakenduste teha.
9.  Keelake failisüsteemi (EFS) krüptimine.
10. **Nõutav:** Avage Windows Update ja installige kõik olulised värskendused.
9.  SYSPREP pilti.

Üksikasjalikud juhised uue pildi loomiseks on:

1.  Windows Server 2012 R2 Update DVD või ISO pildini.
2.  Luua VHD faili abil.
    1.  Käivitage ketta Management (diskmgmt.msc).
    2.  Looge dünaamiliselt laiendada VHD 40 GB või rohkem suurus. (Näete, kui palju ruumi, mis on vaja Windows, rakenduste ja kohandusi. RDSH roll ja Töölauakogemuse funktsioon installitud Windows Server nõuab umbes 10 GB mälu).
        1.  Klõpsake **toimingu > loomine VHD**.
        2.  Määrake asukoht, suurus ja VHD vorming. Valige **dünaamiliselt laiendamine**ja seejärel klõpsake nuppu **OK**.

            Kas see töötab mitu sekundit. VHD loomine on lõpule viidud, peaksite nägema uue ketta ilma mis tahes täht ja ketta halduskonsoolis "Lähtestamata" olekus.

        - Paremklõpsake ketas (mitte eraldamata tühik) ja seejärel klõpsake nuppu **Vorminda ketas**. Valige laadi partition **MBR** (juhtslaidi buutimine kirje) ja seejärel klõpsake nuppu **OK**.
        - Looge uus draiv: Paremklõpsake eraldamata ruumi, ja seejärel klõpsake **Lihtne uus draiv**. Saate nõustuda viisardi vaikimisi, kuid veenduge, et teil määrata draivi nime probleemide vältimiseks, kui saadate malli pilti.
        - Paremklõpsake ketas, ja klõpsake **VHD eemaldada**.





1. Installige Windows Server 2012 R2.
    1. Saate luua uue virtuaalse masina. Uue virtuaalse masina viisardi abil Hyper-V halduri või kliendi Hyper-V.
        1. Valige lehel Määrake genereerimine **loomine 1**.
        2. Lehel ühenduse virtuaalse kõvaketta valige **Kasuta olemasolevaid virtuaalse kõvaketta**ja liikuge sirvides soovitud VHD eelmises juhises loodud.
        2. Klõpsake lehel Installisuvandid valige **operatsioonisüsteem on buutimine CD-le/DVD_ROM kaudu installida**ja valige asukoht installikandja Windows Server 2012 R2.
        3. Valige viisardi installida Windowsi ja rakenduste korral muid võimalusi. Viisardi juhiste täitmist.
    2.  Kui viisard on lõpule jõudnud, virtuaalse masina sätete redigeerimine ja muid muudatusi teha vaja installida Windows ja programmid, nt virtuaalse protsessorite, arvu ja seejärel nuppu **OK**.
    4.  Ühenduse loomine virtuaalse masina ja installige Windows Server 2012 R2.
1. Installige kaugtöölaua seansi hosti (RDSH) roll ja Töölauakogemuse funktsioon.
    1. Käivitage Server Manager.
    2. Klõpsake nuppu **rollide ja funktsioonide lisamine** tervituskuval või **haldamine** kaudu.
    3. Klõpsake nuppu **Järgmine** lehe enne alustamist.
    4. Valige **roll või funktsiooni installimise**ja seejärel klõpsake nuppu **edasi**.
    5. Valige loendist kohaliku arvuti ja seejärel klõpsake nuppu **edasi**.
    6. Valige **Kaugtöölauateenuseid**ja seejärel klõpsake nuppu **edasi**.
    7. Laiendage **kasutajaliidese ja taristu** ja valige **Töölauakogemuse**.
    8. Klõpsake nuppu **Lisa funktsioone**ja seejärel klõpsake nuppu **edasi**.
    9. Klõpsake lehel Kaugtöölauateenuseid klõpsake nuppu **edasi**.
    10. Klõpsake **seansil Host**.
    11. Klõpsake nuppu **Lisa funktsioone**ja seejärel klõpsake nuppu **edasi**.
    12. Kinnitamine installimise valikud lehel Valige **uuesti sihtkoha server automaatselt vajadusel**ja seejärel nuppu **Jah** , klõpsake uuesti hoiatus.
    13. Klõpsake nuppu **Installi**. Arvuti uuesti.
1.  Installige lisafunktsioone nõutud rakenduste, nt .NET Framework 3.5. Funktsioonide installimiseks käivitage rollide ja funktsioonide viisard.
7.  Installima ja konfigureerima programmid ja rakendusi soovite avaldada RemoteAppi kaudu.

>[AZURE.IMPORTANT]
>
>Enne installimist tagamaks, mis tahes rakenduste ühilduvuse probleemid avastatakse enne, kui pilt on üles laaditud RemoteApp rakenduste RDSH rolli installida.
>
>Veenduge, et otsetee rakenduse (**LNK** -fail) kuvatakse menüüs **Start** kõigi kasutajate (%systemdrive%\ProgramData\Microsoft\Windows\Start MS). Ka veenduge, et näete menüü **Start** ikoon on, mida soovite kasutajatele kuvada. Kui ei, siis seda muuta. (Te ei *on* lisamiseks rakenduse käivitamine menüü, kuid lihtsam RemoteApp rakenduse avaldada. Muul juhul tuleb rakendamise Installitee ette rakenduse avaldamisel.)


8.  Mis tahes täiendavaid Windowsi konfiguratsioone nõutud rakenduste teha.
9.  Keelake failisüsteemi (EFS) krüptimine. Käivitage käsuaknas järgmine käsk:

        Fsutil behavior set disableencryption 1

    Teise võimalusena saate seada või lisage järgmised DWORD-väärtuse registris:

        HKLM\System\CurrentControlSet\Control\FileSystem\NtfsDisableEncryption = 1
9.  Kui olete oma pildi sees on Azure virtuaalse masina hoone, ümber nimetada ning ** \%windir%\Panther\Unattend.xml** faili, nagu see blokeerib üles skripti kasutada hiljem töötamist. Selle faili nime muuta Unattend.old nii, et teil on veel fail juhuks, kui teil on vaja oma juurutuse taastada.
10. Avage Windows Update ja installige kõik olulised värskendused. Võib juhtuda mitu korda, et saada kõik värskendused Windows Update'i käivitamine. (Mõnikord värskenduse installimiseks ja selle värskenduse ise nõuab värskendust.)
10. SYSPREP pilti. Veebisaidil käsuviipa, käivitage järgmine käsk:

    **C:\Windows\System32\sysprep\sysprep.exe / generalize/OOBE /shutdown.**

    **Märkus:** Ärge kasutage parameetrit **/mode:vm** käsu SYSPREP isegi juhul, kui see on virtuaalse masina.


## <a name="next-steps"></a>Järgmised sammud ##
Nüüd, kui teil on oma kohandatud malli pilti, peate selle pildi üleslaadimine oma RemoteApp kogum. Järgmised artiklid teabe abil oma saidikogumi loomine:


- [Kuidas luua hübriid saidikogumi RemoteApp](remoteapp-create-hybrid-deployment.md)
- [Kuidas luua pilve saidikogumi RemoteApp](remoteapp-create-cloud-deployment.md)
 