<properties
 pageTitle="Esitage töö HPC Pack klaster Azure | Microsoft Azure'i"
 description="Saate teada, kuidas häälestada mõne kohapealse arvuti mõne HPC Pack kobar Azure tööde edastamiseks"
 services="virtual-machines-windows"
 documentationCenter=""
 authors="dlepow"
 manager="timlt"
 editor=""
 tags="azure-resource-manager,azure-service-management,hpc-pack"/>
<tags
ms.service="virtual-machines-windows"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="vm-multiple"
 ms.workload="big-compute"
 ms.date="10/14/2016"
 ms.author="danlep"/>

# <a name="submit-hpc-jobs-from-an-on-premises-computer-to-an-hpc-pack-cluster-deployed-in-azure"></a>Edastab HPC tööd kohapealse arvutist mõne juurutatud Azure HPC Pack kobar

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Konfigureerida kohapealse klientarvuti, [Microsoft HPC Pack](https://technet.microsoft.com/library/cc514029) kobar Azure tööde edastamiseks. Selles artiklis kirjeldatakse, kuidas häälestada kohaliku arvuti, kuhu on klienttööriistade esitada töö https klaster Azure. Nii mitme kobar kasutajad saavad edastada töö pilvepõhist HPC Pack arvutikobaras, kuid ei pea sõlme VM ühenduse või juurdepääs Azure'i tellimuse.

![Töö arvutikobaras Azure edastamine][jobsubmit]

## <a name="prerequisites"></a>Eeltingimused

* **HPC Pack pea sõlme juurutatud on Azure VM** - soovitame kasutada automatiseeritud tööriistad, nt mõni [Azure Kiirjuhend malli](https://azure.microsoft.com/documentation/templates/) või on [Azure PowerShelli skripti](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) pea sõlme ja kobar juurutamiseks. Peate pea sõlme DNS-i nimi ja mandaadi kobar administraatori lõpuleviimiseks selles artiklis toodud juhiseid.

* **Klientarvuti** - teil on vaja Windows või Windows Server kliendi arvutisse, kus saate käivitada HPC Pack kliendi utiliidid (vt [süsteeminõuded](https://technet.microsoft.com/library/dn535781.aspx)). Kui soovite kasutada HPC Pack veebiportaali või REST API esitada töid, saate kasutada mis tahes klientarvuti oma valik.

* **HPC Pack installikandja** - HPC Pack kliendi Utiliidid tasuta installipaketi HPC Pack (HPC Pack 2012 R2) uusima versiooni installimiseks on saadaval [Microsofti allalaadimiskeskuse](http://go.microsoft.com/fwlink/?LinkId=328024)kaudu. Veenduge, et saate alla laadida HPC Pack, kuhu on installitud pea sõlme VM sama versioon.

## <a name="step-1-install-and-configure-the-web-components-on-the-head-node"></a>Samm 1: Installima ja konfigureerima veebikomponentide pea sõlme

ÜLEJÄÄNUD kasutajaliidese esitada töid klaster https lubamiseks tagada pea sõlme HPC Pack konfigureerinud HPC Pack veebikomponentide. Kui need pole juba installitud, esmalt installima veebikomponentide käivitades HpcWebComponents.msi installifail. Seejärel konfigureerida HPC PowerShelli skripti **Set-HPCWebComponents.ps1**komponendid.

Üksikasjalikud juhised leiate teemast [Microsoft HPC Pack Web Components installida](http://technet.microsoft.com/library/hh314627.aspx).

>[AZURE.TIP] Teatud Azure Kiirjuhend malle HPC Pack installima ja konfigureerima veebikomponentide automaatselt. Kui [HPC Pack IaaS juurutamise skripti](virtual-machines-windows-classic-hpcpack-cluster-powershell-script.md) abil saate luua klaster, saate soovi korral installida ja konfigureerida veebikomponentide juurutamise käigus.

**Veebikomponentide installimiseks**

1. Ühenduse pea sõlme VM kobar administraatori identimisteavet kasutades.

2. Käivitage kaustast HPC Keelepaketi häälestamise pea sõlme HpcWebComponents.msi.

3. Järgige viisardi veebikomponentide installimiseks

**Veebikomponentide konfigureerimine**

1. Pea sõlme, käivitage HPC PowerShelli administraatorina.

2. Directory konfigureerimise skripti asukoha muutmiseks tippige järgmine käsk:

    ```powershell
    cd $env:CCP_HOME\bin
    ```
3. ÜLEJÄÄNUD kasutajaliidese konfigureerimine ja HPC veebiteenuse alustada, tippige järgmine käsk:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service REST –enable
    ```

4. Kui teil palutakse valida sert, valige sert, mis vastab pea sõlme avaliku DNS-i nimi. Näiteks kui juurutate pea sõlme VM näidise klassikaline juurutus, serdi nimi näeb CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net. Kui kasutate juurutamise mudeli ressursihaldur, serdi nimi näeb CN =&lt;*HeadNodeDnsName*&gt;. &lt; *piirkond*&gt;. cloudapp.azure.com.

    >[AZURE.NOTE] Saate valida selle serdi hiljem, kui saadate töö pea sõlme kohapealse arvutist. Ärge valige või konfigureerimine sert, mis vastab pea sõlme Active Directory domeeni nime (nt CN =*MyHPCHeadNode.HpcAzure.local*).

5. Konfigureerimiseks töö esitamise veebiportaali tippige järgmine käsk:

    ```powershell
    .\Set-HPCWebComponents.ps1 –Service Portal -enable
    ```
6. Pärast skripti lõpulejõudmist lõpetada ja taaskäivitage HPC töö toiminguajasti teenus, tippides järgmised käsud:

    ```powershell
    net stop hpcscheduler
    net start hpcscheduler
    ```

## <a name="step-2-install-the-hpc-pack-client-utilities-on-an-on-premises-computer"></a>Samm 2: Installida HPC Pack kliendi Utiliidid kohapealse arvutis

Kui soovite oma arvutisse installida HPC Pack kliendi Utiliidid, laadige [Microsofti allalaadimiskeskusest](http://go.microsoft.com/fwlink/?LinkId=328024)HPC Pack installifailid (täielik install). Kui installimise alustamiseks valige suvand setup **HPC Pack kliendi Utiliidid**.

HPC Pack klienttööriistade pea sõlme VM tööde edastamiseks kasutamiseks peate eksportida sert pea sõlme ja kliendi arvutisse installida. Serdi peab olema. CER-vormingus.

**Serdi eksportimine pea sõlme**

1. Pea sõlme lisada serdid lisandmoodul Microsoft Management Console kohaliku arvuti konto. Lisandmooduli lisamine juhised leiate teemast [serdid lisandmooduli MMC soovite lisada](https://technet.microsoft.com/library/cc754431.aspx).

2. Konsoolipuus, laiendage **serdid – kohaliku arvuti** > **isiklik**ja seejärel klõpsake nuppu **serdid**.

3. Otsige üles sert, mida olete konfigureerinud HPC Pack web komponentide [Samm 1: installida ja konfigureerida veebikomponentide pea sõlme](#step-1:-install-and-configure-the-web-components-on-the-head-node) (nt CN =&lt;*HeadNodeDnsName*&gt;. cloudapp.net).

4. Paremklõpsake sert ja klõpsake käsku **Kõik tööülesanded** > **eksportida**.

5. Serdi ekspordiviisardi, klõpsake nuppu **edasi**ja veenduge, et **ei, ekspordi privaatvõti** on valitud.

6. DER kodeeritud binary X.509 serdi eksportimine viisardi ülejäänud juhised (. CER) vormingus.


**Importida serdi klientarvuti**


1. Kopeerige sert, mida saate eksportida pea sõlme klientarvuti kausta.

2. Kliendi arvutisse, käivitage tekst certmgr.msc.

3. Serdi halduri laiendage **Serdid – praegune kasutaja** > **Trusted Root sertimiskeskuste**, paremklõpsake **serdid**, ja seejärel klõpsake **Kõikide tööülesannete** > **impordi**.

4. Serdi importimise viisardis, klõpsake nuppu **edasi** ja järgige importimiseks sert, mida saate eksportida pea sõlme usaldusväärsete Root sertimiskeskuste pood.



>[AZURE.TIP] Võite näha turvalisus hoiatus, kuna klientarvuti ei pea sõlme sertimiskeskus. Katsetamiseks, saate selle hoiatuse ja serdi importimine.

## <a name="step-3-run-test-jobs-on-the-cluster"></a>Samm 3: Käivita testi klaster tööde haldamine

Veendumaks, et teie konfiguratsiooni, proovige töö klaster Azure kohapealse arvutist. Näiteks saate HPC Pack GUI tööriistad või käsurea käsud esitada töid klaster. Samuti saate esitada töid veebipõhine portaal.


**Töö esitamise käskude käivitamiseks klientarvuti**


1. Kliendi arvutis, kuhu on installitud HPC Pack kliendi Utiliidid, käivitage käsuviip.

2. Valimi tippimine. Loetle kõik tööd klaster, tippige sarnane ühte järgmistest, sõltuvalt DNS-i pea sõlme täisnimi käsk:

    ```command
    job list /scheduler:https://<HeadNodeDnsName>.cloudapp.net /all
    ```
    
    või
    
    ```command
    job list /scheduler:https://<HeadNodeDnsName>.<region>.cloudapp.azure.com /all
    ```

    >[AZURE.TIP] Kasutage pea sõlme, mitte IP-aadress täielik DNS-i nimi ajasti URL-i. Kui määrate IP-aadress, tõrge kuvatakse sarnaselt "serveri sertifikaadi vajab on kehtiv ahelas usalda või paigutada usaldusväärsete root poes."

3. Vastava viiba kuvamisel tippige kasutaja nimi (vorm &lt;DomainName&gt;\\&lt;kasutajanimi&gt;) ja parool HPC kobar administraator või mõne muu kobar kasutaja konfigureeritud. Kui soovite, et mandaat kohalikult rohkem töö toimingute jaoks.

    Kuvatakse loend tööde haldamine.


**Klientarvuti HPC töö halduri kasutamine**

1. Kui domeeni kobar kasutaja mandaat salvestate ei varem töö esitamisel, saate lisada identimisteabe Mandaadihaldur.

    lisamine. Käivitage juhtpaneeli klientarvuti Mandaadihaldur.

    b. Klõpsake **Windowsi identimisteabe** > **Lisa üldise mandaati**.

    c. Interneti-aadressi määramiseks (nt https://&lt;HeadNodeDnsName&gt;.cloudapp.net/HpcScheduler või https://&lt;HeadNodeDnsName&gt;.&lt; piirkonna&gt;.cloudapp.azure.com/HpcScheduler), ja kasutajanimi (&lt;DomainName&gt;\\&lt;kasutajanimi&gt;) ja parool kobar administraator või mõne muu kobar kasutaja konfigureeritud.

2. Kliendi arvutisse, käivitage HPC töö haldur.

3. Tippige dialoogiboksis **Valige pea sõlm** pea sõlme Azure URL-i (nt https://&lt;HeadNodeDnsName&gt;. cloudapp.net või https://&lt;HeadNodeDnsName&gt;.&lt; piirkonna&gt;. cloudapp.azure.com).

    HPC töö Manager avaneb ja kuvab pea sõlme tööde loendit.

**Töötab pea sõlme veebiportaali kasutamine**

1. Avage veebibrauser klientarvuti ja sisestada ühe järgmistest aadressi, olenevalt pea sõlme täisnimi DNS-i.

    ```
    https://<HeadNodeDnsName>.cloudapp.net/HpcPortal
    ```
    
    või
    
    ```
    https://<HeadNodeDnsName>.<region>.cloudapp.azure.com/HpcPortal
    ```
2. Tippige kuvatavas dialoogiboksis turvalisus HPC kobar administraatori identimisteabe domeeni. (Teiste kasutajate kobar saate lisada ka erinevad rollid. Vt [kobar kasutajate haldamine](https://technet.microsoft.com/library/ff919335.aspx)).

    Veebiportaali avaneb loendivaate töö.

3. Valimi töö, mis tagastab stringi "Tere, maailm" klaster esitada, klõpsake vasakpoolses navigeerimismenüüs **uus töökoht** .

4. Klõpsake lehel **Uus töökoht** jaotises **esitamise lehtedelt** **HelloWorld**. Töö lehe kuvatakse.

5. Klõpsake **esitada**. Kui kuvatakse vastav viip, sisestage domeeni identimisteabe HPC kobar administraator. Töö esitatakse ja töö ID kuvatakse lehel **Minu töö** .

6. Töö, mille saatsite tulemuste vaatamiseks klõpsake töö ID ja klõpsake **Tööülesanded** käsu väljund (jaotises **väljundi**) kuvamiseks.

## <a name="next-steps"></a>Järgmised sammud

* Samuti saate esitada töö Azure klaster [HPC Pack REST API](http://social.technet.microsoft.com/wiki/contents/articles/7737.creating-and-submitting-jobs-by-using-the-rest-api-in-microsoft-hpc-pack-windows-hpc-server.aspx).

* Kui soovite esitada Linux klientrakenduses klaster töid, lugege teemat Python valimi [HPC Pack 2012 R2 SDK ja proovi kood](https://www.microsoft.com/download/details.aspx?id=41633).


<!--Image references-->
[jobsubmit]: ./media/virtual-machines-windows-hpcpack-cluster-submit-jobs/jobsubmit.png
