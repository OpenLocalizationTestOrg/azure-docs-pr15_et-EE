<properties
    pageTitle="Hõlmavad alati kättesaadavus rühmade kohta kohapealse Azure'i | Microsoft Azure'i"
    description="Selle õpetuse kasutab ressursse, mis on loodud mudeliga klassikaline juurutamise ja kirjeldatakse, kuidas lisada koopia viisardi abil rakenduses SQL Server Management Studio (SSMS) lisamine Azure on alati klõpsake kättesaadavus rühma koopia."
    services="virtual-machines-windows"
    documentationCenter="na"
    authors="MikeRayMSFT"
    manager="jhubbard"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-windows"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.workload="infrastructure-services"
    ms.date="07/12/2016"
    ms.author="MikeRayMSFT" />

# <a name="extend-on-premises-always-on-availability-groups-to-azure"></a>Azure'i hõlmavad kohapealse alati kättesaadavus rühmade kohta

Alati klõpsake kättesaadavus rühmad pakkuda kõrge kättesaadavus rühmade andmebaasi, lisades teisene koopiad. Need koopiad luba suuda andmebaaside tõrke korral üle. Lisaks saate neid kasutada et loetuks töökoormus või varukoopia tööülesanded.

Kohapealse kättesaadavus rühmad laiendada Microsoft Azure'i saate ühe või mitme Azure VMs SQL serveri ettevalmistamise ja seejärel lisada need nimega koopiad oma kohapealse kättesaadavus rühmad.

Selle õpetuse eeldab, et teil on järgmine:

- Azure'i aktiivne tellimus. Saate [tasuta prooviversiooni kasutajaks](https://azure.microsoft.com/pricing/free-trial/).

- Olemasoleva alati klõpsake kättesaadavus rühma asutusesisene. Kättesaadavus rühmade kohta leiate lisateavet teemast [Alati klõpsake kättesaadavus rühmad](https://msdn.microsoft.com/library/hh510230.aspx).

- Ühenduvus kohapealse võrgu ja Azure virtuaalse võrgu. Selle virtuaalse võrgu loomise kohta leiate lisateavet teemast [Azure klassikaline portaalis VPN saitide konfigureerimine](../vpn-gateway/vpn-gateway-site-to-site-create.md).

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]

## <a name="add-azure-replica-wizard"></a>Lisage Azure koopia viisard

Selles jaotises kirjeldatakse, kuidas **Lisada Azure koopia viisardi** abil saate lisada Azure koopiad lahendusse alati klõpsake kättesaadavus rühma laiendamine.

1. Laiendage kaudu sees SQL Server Management Studio, **Alati On kõrge kättesaadavus** > **Kättesaadavus rühmad** > **[kättesaadavus rühma nimi]**.

1. Paremklõpsake **Kättesaadavus koopiad**ja seejärel käsku **Lisa koopia**.

1. Vaikimisi kuvatakse **Lisada koopia kättesaadavus rühma viisard** . Klõpsake nuppu **edasi**.  Kui olete valinud **kuvada selle lehe uuesti** suvandi lehe allosas ajal eelmisele käivitada viisardi, Kuva ei kuvata.

    ![SQL-I](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742861.png)

1. Teil palutakse ühenduse kõik olemasolevad teisene koopiad. Klõpsake **ühenduse loomine...** kõrval iga koopia või klõpsata **Ühenduse kõik...** ekraani allservas. Pärast autentimist, klõpsake nuppu **edasi** järgmisele kuvale liikumiseks.

1. Lehel **Määrake koopiad** on loetletud mitu vahekaarti ülaserva: **koopiad**, **lõpp-punktid**, **Varundamise eelistused**ja **kuulajale**. Klõpsake menüü **koopiad** **Lisada Azure koopia...** Lisage Azure koopia viisardi käivitamiseks.

    ![SQL-I](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742863.png)

1. Kui teil on installitud enne, valige olemasolev Azure'i halduse tunnistus sert kohaliku Windowsi poest. Valige või sisestage Azure tellimuse id, kui olete varem kasutanud. Võite klõpsata laadida alla laadida ja installida Azure halduse sertifikaadi ja alla laadida Azure'i konto kaudu tellimuste loend.

    ![SQL-I](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742864.png)

1. Iga välja väärtused, mida kasutatakse Azure virtuaalse masina (VM) majutavad koopia loomiseks lehel on asustada.

  	|Säte|Kirjeldus|
|---|---|
|**Pilt**|Valige soovitud kombinatsiooni OS ja SQL serveris|
|**VM suurus**|Valige soovitud suurus VM, mis sobib kõige paremini teie ettevõtte vajadustele.|
|**VM nimi**|Määrake uus VM kordumatu nimi. Nimi peab sisaldavad 3 ja 15 märkide vahel, saate sisaldada ainult tähti, numbreid ja sidekriipsu, ja peavad algama tähega ja lõpetamine tähe või numbri.|
|**VM kasutajanimi**|Määrake kasutajanimi, mis muutub administraatorikonto VM|
|**VM administraatori parool**|Uue konto parooli määramine|
|**Kinnitage parool**|Kinnitage parool uus konto|
|**Virtuaalse võrgu**|Määrake Azure virtuaalse võrk, mis peaks kasutama uut VM. Virtuaalne võrkude kohta leiate lisateavet teemast [Virtuaalse võrgu ülevaade](../virtual-network/virtual-networks-overview.md).|
|**Virtuaalne alamvõrku**|Määrake virtuaalse alamvõrku, et uus VM kasutama|
|**Domeeni**|Kinnitage Domeen juba eelnevalt täidetud väärtus on õige|
|**Domeeni kasutajanimi**|Määrake konto, mis on kohalike administraatorite rühma sõlmed kohaliku kobar|
|**Parooli**|Domeeninime kasutaja parooli määramine|

1. Klõpsake nuppu **OK** juurutamise sätete kinnitamiseks.

1. Õiguslikult kuvatakse järgmine. Lugege läbi ja kui nõustute nende tingimustega, klõpsake nuppu **OK** .

1. **Määrake koopiad** lehe kuvatakse uuesti. Kontrollige sätteid uue Azure'i koopia **koopiad**, **lõpp-punktid**ja **Varundamise eelistused** vahekaartidele. Muutke sätteid nii, et teie ettevõtte nõuetele.  Need vahekaardid sisaldub parameetrid kohta leiate lisateavet teemast [Määrata koopiad lehe (uus kättesaadavus rühma viisardi/Add koopia viisard)](https://msdn.microsoft.com/library/hh213088.aspx). Pange tähele, et kuulajatele ei saa luua kuulajale menüü kasutamise kättesaadavus rühmad, mis sisaldavad Azure koopiad. Lisaks, kui on kuulajale on juba loodud enne viisardi käivitamist, kuvatakse teade, et see ei toeta Azure. Käsitleme **loomine mõne kättesaadavus rühma kuulajale** jaotises kuulajatele loomise kohta.

    ![SQL-I](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742865.png)

1. Klõpsake nuppu **edasi**.

1. Saate valida andmete sünkroonimise soovite kasutage lehel **Valige algse andmete sünkroonimine** ja klõpsake nuppu **edasi**. Enamik stsenaariumid, valige **Täielik andmete sünkroonimine**. Andmete sünkroonimine meetodite kohta leiate lisateavet teemast [Valige andmete sünkroonimine Avaleht (alati sisse kättesaadavus rühma viisardeid)](https://msdn.microsoft.com/library/hh231021.aspx).

1. Vaadake lehel **valideerimine** tulemusi. Vea tasumata probleemid ja käivitage uuesti valideerimine vajaduse korral. Klõpsake nuppu **edasi**.

    ![SQL-I](./media/virtual-machines-windows-classic-sql-onprem-availability/IC742866.png)

1. Kontrollige sätteid lehel **Kokkuvõte** ja seejärel klõpsake nuppu **valmis**.

1. Ebausaldusväärsete hakkab. Kui viisard on lõpule viidud, klõpsake nuppu **Sule** välja viisardi sulgemiseks.

>[AZURE.NOTE] Lisage Azure koopia viisard loob logifaili Users\User Name\AppData\Local\SQL Server\AddReplicaWizard. Selle faili saab kasutada tõrkeotsinguks nurjunud Azure'i koopia juurutuste. Kui viisard ei käivitamisel midagi, kõik eelmise toimingud on tagasi pöörata, sh kustutamine ettevalmistatud VM.

## <a name="create-an-availability-group-listener"></a>Mõne rühma kättesaadavus kuulajale loomine

Pärast rühma kättesaadavus on loodud, peaksite looma on kuulajale klientidele ühenduse selle koopiad. Kuulajatele otse sissetulevaid ühendusi esmast või kirjutuskaitstud teise koopia. Kuulajatele kohta leiate lisateavet teemast [konfigureerimine on ILB kuulajale alati klõpsake kättesaadavus rühmade Azure](virtual-machines-windows-classic-ps-sql-int-listener.md).

## <a name="next-steps"></a>Järgmised sammud

Lisaks **Lisamine Azure koopia viisardi** abil saate alati sisse kättesaadavus rühma laiendamine Azure, võib ka teisaldate mõne SQL serveri töökoormus täielikult Azure. Alustamiseks, lugege teemat [Azure SQL serveri virtuaalse masina ettevalmistamise](virtual-machines-windows-portal-sql-server-provision.md).

Muud teemad, mis töötab SQL Server Azure'i VMs seotud, vt [SQL Server Azure'i Virtuaalmasinates](virtual-machines-windows-sql-server-iaas-overview.md).
