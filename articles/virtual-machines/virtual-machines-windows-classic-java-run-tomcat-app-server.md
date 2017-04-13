<properties
    pageTitle="Tomcat virtual arvutisse | Microsoft Azure'i"
    description="Selles õpetuses kasutab ressursse, mis on loodud mudeliga klassikaline juurutamise ja näidatakse, kuidas luua virtuaalse Windowsi arvuti ja konfigureerida nii, et käivitada Apache Tomcat rakenduste serveri."
    services="virtual-machines-windows"
    documentationCenter="java"
    authors="rmcmurray"
    manager="wpickett"
    editor=""
    tags="azure-service-management" />

<tags
    ms.service="virtual-machines-windows"
    ms.workload="web"
    ms.tgt_pltfrm="vm-windows"
    ms.devlang="Java"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>

# <a name="how-to-run-a-java-application-server-on-a-virtual-machine-created-with-the-classic-deployment-model"></a>Kuidas käivitada Java rakenduste serveri klassikaline juurutamise mudeliga loodud virtuaalse masina

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]


Azure, saate virtuaalse masina serveri võimalusi pakkuda. Näiteks saab konfigureerida Azure töötab Java rakenduste serveri, nt Apache Tomcat majutada. Kui olete selle juhendi, on teil mõista, kuidas luua Azure töötab ja konfigureerida nii, et käivitada Java rakenduste serveri.

Saate:

* Kuidas luua virtuaalse masina, mis sisaldab soovitud Java arengu Kit (JDK) juba installitud.
* Kuidas oma virtuaalse masina kaugühenduse teel sisse logida.
* Kuidas installida Java rakenduste serveri virtuaalne arvuti.
* Kuidas luua oma virtuaalse masina lõpp-punkt.
* Kuidas avada port tulemüüri serverisse rakenduse.

Selleks et selles õpetuses, installitakse Apache Tomcat rakenduste serveri virtuaalse masina. Lõplikus installimise tulemuseks Tomcat installi, nt mõni järgmistest.

![Töötab Apache Tomcat][virtual_machine_tomcat]

[AZURE.INCLUDE [create-account-and-vms-note](../../includes/create-account-and-vms-note.md)]

## <a name="to-create-a-virtual-machine"></a>Kui soovite luua virtuaalse masina

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com)sisse logida.
2. Klõpsake nuppu **Uus**, nuppu **Arvuta**, klõpsake **virtuaalse masina**ja valige **Galeriist**.
3. Valige dialoogiboksis **Valige virtuaalse masina pilt** **JDK 7 Windows Server 2012**.
Pange tähele, et **JDK 6 Windows Server 2012** on saadaval, kui teil on pärand rakendusi, mis ei ole JDK 7 käivitamiseks valmis.
4. Klõpsake nuppu **edasi**.
5. **Virtuaalne arvuti konfiguratsiooni** dialoogiboksis järgmist.
    1. Määrake virtuaalse masina nimi.
    2. Määrata virtuaalse masina jaoks kasutada.
    3. Sisestage väljale **Kasutajanimi** administraatori nimi. See nimi ja siis sisestage järgmine parool meeles pidada, kas neid kasutada, kui Eemalt logite virtuaalse masina.
    4. Sisestage **Uus parool** väljale parool ja sisestage see uuesti väljale **Kinnita** . See on administraatori konto parool.
    5. Klõpsake nuppu **edasi**.
6. Järgmise **virtuaalse masina konfiguratsiooni** dialoogiboksis järgmist.
    1. **Pilveteenuses**, kasutage vaikeväärtust **Loo uus pilveteenuses**.
    2. **Pilveteenuse teenuse DNS-i nimi** väärtus peab olema kordumatu cloudapp.net üle. Vajadusel muutke seda väärtust nii, et selle Azure'i näitab, et see on kordumatu.
    2. Piirkond, osaleja rühma või virtuaalse võrgu määramiseks. Selle õpetuse puhul märkida, nt **Lääne USA**piirkonnas.
    2. **Salvestusruumi konto**, valige suvand **Kasuta automaatselt loodud salvestusruumi konto**.
    3. **Kättesaadavus**, valige **(pole)**.
    4. Klõpsake nuppu **edasi**.
7. Lõplik dialoogiboksi **virtuaalse masina konfigureerimine** :
    1. Nõustuge vaikimisi lõpp-punkti kirjeid.
    2. Klõpsake nuppu **valmis**.

## <a name="to-remotely-sign-in-to-your-virtual-machine"></a>Virtuaalne arvuti eemalt sisse logida

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com)sisse logima.
2. Klõpsake **virtuaalmasinates**.
3. Klõpsake selle nime virtuaalse masina, mida soovite sisse logida.
4. Pärast käivitamist virtuaalse masina hüpikmenüüst lehe allservas võimaldab ühendused.
5. Klõpsake nuppu **Loo ühendus**.
6. Vastata viipasid vastavalt vajadusele ühenduse, virtuaalse masina. See salvestamine või avamine RDP-faili, mis sisaldab ühenduse üksikasju. Peate url: port kopeeritakse Viimane osa RDP-faili esimene rida ja kleepida selle serveri rakenduse Logi sisse.

## <a name="to-install-a-java-application-server-on-your-virtual-machine"></a>Java rakenduste serveri virtual arvutisse installimiseks

Saate kopeerida Java rakenduste serveri virtuaalne arvuti või saate installida Java rakenduste serveri kaudu installer.

Selleks et selles õpetuses, installitakse Tomcat.

1. Kui te oma virtual arvutisse sisse loginud, avage brauseriseansi, et [Apache Tomcat](http://tomcat.apache.org/download-70.cgi).
2. Topeltklõpsake **32-bitine ja 64-bitine Windows teenuse Installer**linki. Selle meetodi abil installitakse Tomcat teenust Windows.
3. Kui küsitakse, valige installiprogrammi käivitamiseks.
4. **Apache Tomcat** häälestusviisardi järgida Tomcat installimiseks viipasid. Selleks et selles õpetuses, aktsepteerige vaikesätted on hea. Kui jõuate dialoogiboksi **Apache Tomcat häälestusviisardi lõpuleviimine** , saate soovi korral **Käivitada Apache Tomcat** on Tomcat Alusta kohe. Klõpsake nuppu **valmis** Tomcat häälestamise lõpuleviimiseks.

## <a name="to-start-tomcat"></a>Tomcat käivitamine
Kui te ei valinud dialoogiboksi **Apache Tomcat häälestusviisardi lõpuleviimine** Tomcat käivitamiseks, käivitamine, avades Käsuviip virtual teie arvutis töötab **net alustada Tomcat7**.

Nüüd näete Tomcat töötab, kui käivitate virtuaalne seadme brauserit ja avada <http://localhost:8080>.

Välise masinad töötab Tomcat kuvamiseks peate lõpp-punkti loomiseks ja pordi avamine.

## <a name="to-create-an-endpoint-for-your-virtual-machine"></a>Virtuaalne arvuti lõpp-punkti loomiseks
1. [Azure'i klassikaline portaali](https://manage.windowsazure.com)sisse logida.
2. Klõpsake **virtuaalmasinates**.
3. Klõpsake virtuaalse masina, kus töötab teie Java rakenduste serveri nime.
4. Klõpsake nuppu **lõpp-punktid**.
5. Klõpsake nuppu **Lisa**.
6. Veenduge dialoogiboksis **Lisa lõpp-punkti** **lisamine autonoomse lõpp-punkti** oleks märgitud, ja klõpsake siis nuppu **edasi**.
7. Dialoogiboksi **Uus lõpp-punkti üksikasjad** :
    1. Määrake nimi lõpp-punkti; näiteks **HttpIn**.
    2. Määrake **TCP** protokolli.
    3. Määrake **80** avaliku pordi.
    4. Määrake **8080** privaatne pordi.
    5. Dialoogiboksi sulgemiseks nuppu **valmis** . Nüüd on loodud oma lõpp-punkti.

## <a name="to-open-a-port-in-the-firewall-for-your-virtual-machine"></a>Arvuti virtuaalne tulemüüris pordi avamine
1. Logige sisse oma virtuaalse masina.
2. Klõpsake **Windowsi avakuva**.
3. Klõpsake käsku **Juhtpaneel**.
4. Klõpsake linki **süsteem ja turve**, klõpsake suvandit **Windowsi tulemüür**ja seejärel nuppu **Täpsemad sätted**.
5. Klõpsake käsku **Sissetulevad reeglid**ja seejärel klõpsake nuppu **Uus reegel**.
 ![Sissetuleva uus reegel][NewIBRule]
6. **Reegli tüüp**valige **Port**ja seejärel klõpsake nuppu **edasi**.
 ![Uue sissetuleva reegli port][NewRulePort]
7. Kuva **protokolli ja pordid** valige **TCP**, määrata **teatud kohaliku port** **8080** ja seejärel klõpsake nuppu **edasi**.
 ![Sissetuleva uus reegel][NewRuleProtocol]
8. **Toimingu** aknas valige **Luba ühenduse**ja klõpsake nuppu **edasi**.
 ![Uus Sissetulev reegel toiming][NewRuleAction]
9. Kuval **profiili** veenduge, et **Domeen**, **privaatse**ja **avaliku** on valitud ja klõpsake nuppu **edasi**.
 ![Sissetulev reegel uus profiil][NewRuleProfile]
10. Kuval **nime** määramiseks nimi reegli, nt **HttpIn** (reegli nimi on nõutav sobitada lõpp-punkti nimi, kuid), ja klõpsake siis nuppu **valmis**.  
 ![Uue sissetuleva reegli nimi][NewRuleName]

Selles etapis veebisaidi Tomcat peaks olema vaadatav välises brauseris URL-i, vormi * *http://*oma\_DNS-i\_nimi*. cloudapp.net**kus ** *oma\_DNS-i\_nimi*** on määratud, kui olete loonud virtuaalse masina DNS-i nimi.

## <a name="application-lifecycle-considerations"></a>Rakenduse elutsükli kaalutlused
* Saanud luua oma rakenduse Veebiarhiiv (sõja) ja lisab selle kausta **veebirakenduste** . Näiteks lihtsa Java teenuse lehe (JSP) dünaamilise projekti loomine ja eksportida faili sõda, kopeerida sõda Apache Tomcat **veebirakenduste** kausta virtual arvutisse, siis käivitage see brauseris.
* Kui Tomcat teenus on installitud, see on vaikimisi käsitsi käivitada. Mida saab vahetada selle automaatselt käivituma, kasutades teenuste lisandmoodul. Käivitage teenuste lisandmoodul, klõpsates **Windowsi avakuva**, **Haldusriistad**ja seejärel **teenused**. Topeltklõpsake **Apache Tomcat** teenuse ja seadke sätte **Käivitustüüp** väärtuseks **Automaatne**.

    ![Teenuse automaatselt käivituma seadmine][service_automatic_startup]

    Kasutada Tomcat start automaatselt on, et see käivitub, kui virtuaalse masina taaskäivitatakse (nt pärast vajavate uuesti installida).

## <a name="next-steps"></a>Järgmised sammud
Lugege lisateavet muude teenuste (nt Azure Storage, teenuse siini ja SQL-andmebaasi), mida soovite kaasata, Java-rakenduste [Java Arenduskeskus](https://azure.microsoft.com/develop/java/)saadaoleva teabe vaatamise kohta.

[virtual_machine_tomcat]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_VirtualMachineRunningApacheTomcat.png

[service_automatic_startup]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/WA_TomcatServiceAutomaticStart.png









[NewIBRule]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewInboundRule.png
[NewRulePort]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRulePort.png
[NewRuleProtocol]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProtocol.png
[NewRuleAction]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleAction.png
[NewRuleName]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleName.png
[NewRuleProfile]: ./media/virtual-machines-windows-classic-java-run-tomcat-app-server/NewRuleProfile.png
