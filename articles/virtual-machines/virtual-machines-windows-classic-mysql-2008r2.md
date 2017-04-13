<properties
    pageTitle="Luua töötab MySQL-i VM | Microsoft Azure'i"
    description="Azure virtuaalse masina, mis töötab Windows Server 2012 R2 ja klassikaline juurutamise näidise MySQL-i andmebaasi loomine"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="cynthn"
    manager="timlt"
    editor="tysonn"
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/25/2016"
    ms.author="cynthn"/>


# <a name="install-mysql-on-a-virtual-machine-created-with-the-classic-deployment-model-running-windows-server-2012-r2"></a>Operatsioonisüsteemi Windows Server 2012 R2 klassikaline juurutamise mudeliga loodud virtuaalse masina MySQL-i installimine

[MySQL-i](http://www.mysql.com) on SQL-andmebaasiga, populaarsed avatud allikas. Selle õpetuse näidatakse, kuidas installida ja käitada ühenduse versiooni MySQL-i 5.6.23 MySQL-i server töötab Windows Server 2012 R2 kohta. Linux MySQL-i installimise juhiste saamiseks vaadake: [Kuidas installida Azure MySQL-i](virtual-machines-linux-mysql-install.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="create-a-virtual-machine-running-windows-server-2012-r2"></a>Töötab Windows Server 2012 R2 loomine

Kui te pole veel opsüsteemi Windows Server 2012 R2 VM, saate selle [õpetuse](virtual-machines-windows-classic-tutorial.md) virtuaalse masina loomiseks. 

## <a name="attach-a-data-disk"></a>Andmete kettal manustamine

Virtuaalse masina on loodud, saate soovi korral ka täiendavad andmed kettale manustada. See on soovitatav tootmise töökoormus ja vältida otsa ruumi OS draivile (c), mis sisaldab operatsioonisüsteem.

Vaadake, [Kuidas lisada andmed kettale Windows virtuaalse masina](virtual-machines-windows-classic-attach-disk.md) ja juhiseid manustamise on tühjale kettapuhastusriista. Määrake host vahemälu sätted **pole** või **kirjutuskaitstud**.

## <a name="log-on-to-the-virtual-machine"></a>Virtuaalse masina sisse logida

Järgmisena saate [virtuaalse masina sisse logida](virtual-machines-windows-classic-connect-logon.md) nii, et saate installida MySQL-i.

##<a name="install-and-run-mysql-community-server-on-the-virtual-machine"></a>Installimine ja käivitamine MySQL-i ühenduse Server virtuaalne arvutisse

Järgmiste juhiste abil installida, konfigureerida ja käivitada ühenduse MySQL-i serveri versiooni.

> [AZURE.NOTE] Need juhised kehtivad selle 5.6.23.0 ühenduse MySQL-i ja Windows Server 2012 R2 versiooni. Teie töötamist võib olla erinev erinevate versioonide MySQL-i või Windows Server.

1.  Pärast ühendatud virtuaalse masina kaugtöölaua, klõpsake **Internet Explorer** avakuva kaudu.
2.  Valige paremas ülanurgas (cogged ratta ikoon) nuppu **Tööriistad** ja seejärel käsku **Interneti-suvandid**. Klõpsake vahekaarti **Turvalisus** , klõpsake ikooni **Usaldusväärsed saidid** ja seejärel klõpsake nuppu **saidid** . Lisage http://*. mysql.com usaldusväärsete saitide loendisse. Klõpsake * *Sule**, ja seejärel klõpsake käsku * *OK**.
3.  Rakenduses, Internet Exploreri aadressiribale, tippige http://dev.mysql.com/downloads/mysql/.
4.  Otsige üles ja alla laadida Windows Installer MySQL-i uusima versiooni MySQL-i saidi abil. Valides MySQL-i installiprogrammi, laadige versioon, mis on täielik faili määramine (näiteks selle mysql-installer-ühenduse-5.6.23.0.msi failimahu 282.4 MB), ja salvestage installiprogrammi.
5.  Kui installiprogrammi allalaadimine on lõppenud, klõpsake nuppu **Käivita** Käivita häälestus.
6.  Klõpsake lehel **Litsentsileping** nõustuge litsentsilepinguga ja klõpsake nuppu **edasi**.
7.  Lehe **häälestus tüübi valimine** nuppu häälestus tüüp, mida soovite ja seejärel klõpsake nuppu **edasi**. Järgmiste juhiste korral eeldatakse **ainult Server** häälestamise tüüp valikut.
8.  Klõpsake lehel **Installi** **käivitamine**. Kui installimine on lõpule jõudnud, klõpsake nuppu **edasi**.
9.  Klõpsake lehel **Toote konfiguratsioon** nuppu **edasi**.
10. Määrake lehel **tüüp ja võrgunduse** teie soovitud konfiguratsiooni tüüp ja ühenduvuse võimalusi, sealhulgas TCP pordi vajaduse korral. Valige **Kuva täpsemad suvandid**ja seejärel klõpsake nuppu **edasi**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_TypeNetworking.png)

11. Määrake lehel **kontod ja rollide** MySQL-i juurkausta keerukas parool. Vajaduse korral lisada täiendavad MySQL-i Kasutajakontod ja seejärel klõpsake nuppu **edasi**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AccountsRoles_Filled.png)

12. Lehel **Windowsi teenuse** muudatuste vaikesätete määramine MySQL-i Server töötab teenus Windows vastavalt vajadusele ja seejärel klõpsake nuppu **edasi**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_WindowsService.png)

13. Lehel **Täpsemad suvandid** määrata muudatuste logimissuvandite vastavalt vajadusele ja seejärel klõpsake nuppu **edasi**.

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_AdvOptions.png)

14. Klõpsake lehel **Rakendamine konfiguratsiooni** **käivitamine**. Kui konfigureerimine juhiseid lõpule jõudnud, klõpsake nuppu **valmis**.
15. Klõpsake lehel **Toote konfiguratsioon** nuppu **edasi**.
16. Lehel **Installimine lõpule jõudnud** , klõpsake nuppu **Kopeeri logi lõikelauale** kui soovite seda hiljem uurida ja klõpsake siis nuppu **valmis**.
17. Avakuvale ja tippige **MySQL-i**ja klõpsake **MySQL-i 5.6 käsurea kliendi**.
18. Sisestage root parool, mida juhises 11 määratud ja te saate esitada koos viip kus saate väljastada käskude MySQL-i konfigureerimine. Lisateavet käskude ja süntaks, leiate teemast [MySQL-i käsiraamatud](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_CommandPrompt.png)

19. Samuti saate konfigureerida serveri konfiguratsiooni vaikesätted, alus ja andmete kataloogid ja draivid, nt failis C:\Program Files (x86) \MySQL\MySQL serveri 5.6\my-default.ini kirjed. Lisateavet leiate teemast [5.1.2 Server Configuration vaikesätted](http://dev.mysql.com/doc/refman/5.6/en/server-configuration-defaults.html).

## <a name="configure-endpoints"></a>Lõpp-punktid konfigureerimine

Kui soovite MySQL-i serveri teenus kättesaadav MySQL-i klientarvutite Internetis, peate TCP pordi aluseks on listening MySQL-i serveri teenuse lõpp-punkti konfigureerimine ja täiendavad Windowsi tulemüüri reegli loomine. See on TCP port 3306 juhul, kui teie määratud teise porti lehe **tüüp ja Networking** (eelmise toimingu samm 10).


> [AZURE.NOTE] Hoolikalt kaaluge turvalisus mõju sellega, sest see teeb MySQL-i serveri teenuse kättesaadavaks kõik arvutid Interneti. Saate määratleda kogumi allika IP-aadressid, mis on lubatud kasutada lõpp-punkti koos mõne Access Control loend (ACL). Lisateavet leiate teemast [lõpp üles virtuaalse masina](virtual-machines-windows-classic-setup-endpoints.md).


Konfigureerida MySQL-i serveri teenuse lõpp-punkt:

1.  Azure'i klassikaline portaalis, klõpsake **Virtuaalmasinates**, klõpsake oma MySQL-i virtuaalse masina nimi ja klõpsake **lõpp-punktid**.
2.  Klõpsake käsku **Lisa**.
3.  Klõpsake lehel **Lisa virtuaalse masina lõpp-punkti** paremnoolt.
4.  Kui kasutate vaikimisi MySQL-i TCP port 3306, klõpsake **MySQL-i** **nimi**ja klõpsake märkeruutu.
5.  Kui kasutate erinevat TCP porti, tippige **nimi**kordumatu nimi. Valige **TCP** Protocol (protokoll), tippige pordinumber nii **Avalik Port** ja **Privaatne Port**ja klõpsake märkeruutu.

## <a name="add-a-windows-firewall-rule-to-allow-mysql-traffic"></a>MySQL-i liikluse lubamiseks Windowsi tulemüüri reegli lisamine

Windowsi tulemüüri reegel, mis lubab MySQL-i liikluse Interneti kaudu lisamiseks käivitage järgmine käsk administraatoriõigustes Windows PowerShelli käsuviibas MySQL-i server virtuaalne arvutisse.

    New-NetFirewallRule -DisplayName "MySQL56" -Direction Inbound –Protocol TCP –LocalPort 3306 -Action Allow -Profile Public


    
## <a name="test-your-remote-connection"></a>Testige oma kaugühenduse kaudu


Testige oma kaugühenduse MySQL-i serveri teenus, mis töötab Azure virtuaalse masina, peate esmalt määratlema vastav pilveteenusesse, mis sisaldab virtuaalse masina MySQL-i serveri DNS-i nimi.

1.  Azure'i klassikaline portaalis klõpsake **Virtuaalmasinates**, klõpsake oma MySQL-i serveri virtuaalse masina nimi ja klõpsake **armatuurlaua**.
2.  Märkus virtuaalse masina armatuurlaual **Kiirülevaate lühidalt** jaotises **DNS-i nimi** väärtus. Siin on näide:

    ![](./media/virtual-machines-windows-classic-mysql-2008r2/MySQL_DNSName.png)

3.  Kohaliku arvutis, kus töötab MySQL-i või MySQL-i klient, käivitage järgmine käsk MySQL-i kasutajana sisse logida.

        mysql -u <yourMysqlUsername> -p -h <yourDNSname>

    MySQL-i kasutaja nimi dbadmin3 ja DNS-i testmysql.cloudapp.net virtuaalse masina nimi, kasutage järgmist käsku.

        mysql -u dbadmin3 -p -h testmysql.cloudapp.net


## <a name="next-steps"></a>Järgmised sammud

Töötab MySQL-i kohta leiate lisateavet teemast [MySQL-i dokumendid](http://dev.mysql.com/doc/).
