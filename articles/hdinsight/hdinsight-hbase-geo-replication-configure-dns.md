<properties 
   pageTitle="DNS-i konfigureerimine kahe Azure virtuaalse võrgu vahel | Microsoft Azure'i" 
   description="Saate teada, kuidas konfigureerida VPN-ühendused ja domeeninimede lahendamine ühest virtuaalse võrgust, ja kuidas konfigureerida HBase geo-dispersioonanalüüs." 
   services="hdinsight,virtual-network" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="06/28/2016"
   ms.author="jgao"/>

# <a name="configure-dns-between-two-azure-virtual-networks"></a>DNS-i vahel kahe Azure virtuaalse võrgu konfigureerimine

> [AZURE.SELECTOR]
- [VPN ühenduvuse konfigureerimine](../hdinsight-hbase-geo-replication-configure-vnets.md)
- [DNS-i konfigureerimine](hdinsight-hbase-geo-replication-configure-dns.md)
- [Konfigureerimine HBase dispersioonanalüüs](hdinsight-hbase-geo-replication.md) 


Saate teada, kuidas lisada ja konfigureerida nimeservereid Azure'i nimelahendus sees ja üle virtuaalse võrkude käsitlema.

Selles õpetuses on teine osa [sarja] [ hdinsight-hbase-geo-replication] HBase geo-dispersioonanalüüs loomise kohta:

- [Virtuaalse Privaatvõrgu ühendus kahe virtuaalse võrgu vahel konfigureerimine][hdinsight-hbase-geo-replication-vnet]
- DNS-i konfigureerimine virtuaalne võrkude (selles õpetuses)
- [Konfigureerimine HBase geo dispersioonanalüüs][hdinsight-hbase-geo-replication]


Järgmine diagramm näitab kahe virtuaalse võrgu, mis on loodud [konfigureerimine virtuaalse Privaatvõrgu ühendus kahe virtuaalse võrgu vahel][hdinsight-hbase-geo-replication-vnet]:

![Hdinsightiga HBase dispersioonanalüüs virtuaalse võrgustikudiagramm][img-vnet-diagram]

##<a name="prerequisites"></a>Eeltingimused
Enne alustamist selles õpetuses, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).

- **Töökoha Azure PowerShelli abil**.

    Enne käivitamist PowerShelli skriptide, veenduge, et olete loonud ühenduse Azure tellimuse abil järgmine cmdlet-käsk:

        Add-AzureAccount

    Kui teil on mitu Azure tellimust, abil järgmine cmdlet-käsk praeguse tellimuse:

        Select-AzureSubscription <AzureSubscriptionName>
        
    [AZURE.INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

- **Kahe Azure virtuaalse võrgu VPN-ühenduse kaudu**.  Juhised leiate teemast [konfigureerimine VPN-ühendus kahe Azure virtuaalse võrgu vahel][hdinsight-hbase-geo-replication-vnet].

>[AZURE.NOTE] Azure'i teenuste nimed ja virtuaalse masina nimed peavad olema kordumatud. Selles õpetuses kasutatav nimi on Contoso-[Azure teenuse/VM nimi]-[EU / U.S.]. Näiteks Contoso-VNet-EL on Azure virtuaalse võrgu Põhja-Euroopa andmekeskuse; Contoso-DNS-USA on DNS-i server VM andmekeskuse Ida-US. Peab tulla oma nimed.
 
 
##<a name="create-azure-virtual-machines-to-be-used-as-dns-servers"></a>Looge DNS-serverid kasutada Azure'i virtuaalmasinates

**Luua virtuaalse masina Liidus Contoso-VNet-ehk Contoso-DNS-EL**

1.  Klõpsake nuppu **Uus**, **ARVUTADA**, **VIRTUAALSE masina**, **galeriist**.
2.  Valige **Windows Server 2012 R2 andmekeskuse**.
3.  Sisestage:
    - **VIRTUAALSE masina nimi**: Contoso-DNS-EL
    - **Uus kasutaja nimi**: 
    - **Uus parool**: 
4.  Sisestage:
    - **PILVETEENUSES**: looge uus pilveteenuses
    - **Piirkonna/osaleja rühma/VIRTUAALSE võrgu**: (valige Contoso-VNet-EL)
    - **Virtuaalne ALAMVÕRKU**: alamvõrgu-1
    - **Salvestusruumi konto**: konto automaatselt loodud mäluruumi kasutamine
    
        Pilveteenuse teenuse nimi on sama mis virtuaalse masina nimi. Selles näites on Contoso-DNS-EL. Jaoks järgmise virtuaalmasinates, saate valida kasutada sama pilveteenuses.  Kõik virtuaalmasinates jaotises sama pilveteenuses ühiskasutusse anda sama virtuaalse võrgu- ja järelliite domeeni.

        Salvestusruumi kontot kasutatakse virtuaalse masina pildifaili talletamiseks. 
    - **Lõpp-punktid**: (liikuge kerides allapoole ja valige **DNS-i**) 

Pärast loomist virtuaalse masina teada sise IP ja välise IP.

1.  Klõpsake nuppu virtuaalse masina nimi, **Contoso-DNS-EL**.
2.  Valige **armatuurlaud**.
3.  Kirjutage:
    - VIRTUAALNE AVALIKU IP-AADRESS
    - SISEMISE IP-AADRESS


**Luua virtuaalse masina sees Contoso VNet-USA-ehk Contoso-DNS-USA** 

- Korrake seda toimingut luua virtuaalse masina järgmised väärtused:
    - VIRTUAALSE masina nimi: Contoso-DNS-USA
    - REGIOON/osaleja rühma/VIRTUAALSE võrgu: Contoso-VNET-USA valimine
    - VIRTUAALNE ALAMVÕRKU: Alamvõrgu-1
    - SALVESTUSRUUMI konto: Automaatselt loodud salvestusruumi konto kasutamine
    - Lõpp-punktid: (valige DNS-i)

##<a name="set-static-ip-addresses-for-the-two-virtual-machines"></a>Staatiline IP-aadresside kaks virtuaalmasinates seadmine

DNS-serverid nõuab staatiline IP-aadressid.  Seda toimingut ei saa teha Azure'i klassikaline portaalist. Kasutage Azure PowerShelli.

**Staatiline IP-aadress kaks virtuaalmasinates konfigureerimine**

1. Avage Windows PowerShell ISE.
2. Käivitage järgmised cmdlet-käsud.  

        Add-AzureAccount
        Select-AzureSubscription [YourAzureSubscriptionName]
        
        Get-AzureVM -ServiceName Contoso-DNS-EU -Name Contoso-DNS-EU | Set-AzureStaticVNetIP -IPAddress 10.1.0.4 | Update-AzureVM
        Get-AzureVM -ServiceName Contoso-DNS-US -Name Contoso-DNS-US | Set-AzureStaticVNetIP -IPAddress 10.2.0.4 | Update-AzureVM 

    Teenuse nimi on pilvepõhise teenuse nimi. Kuna DNS-i server on esimene virtuaalse masina pilvepõhise teenuse, pilvepõhise teenuse nimi on sama mis virtuaalse masina nimi.

    Peate värskendama teenuse nimi ja nimi nimed, mida teil on vastavad.


##<a name="add-the-dns-server-role-the-two-virtual-machines"></a>Lisage DNS-i serveri rolli kahe virtuaalmasinates

**Contoso-DNS-EL jaoks DNS-i serveri roll lisamiseks**

1.  Klassikaline portaalist Azure'i **Virtuaalmasinates** klõpsake vasakul. 
2.  Klõpsake **Contoso-DNS-EL**.
3.  Klõpsake **ARMATUURLAUA** ülaosas.
4.  Klõpsake nuppu **Ühenda** alt ja järgige juhiseid ühenduse, virtuaalse masina RDP kaudu.
2.  Jooksul RDP-seansi, klõpsake alumises vasakus nurgas Avakuva avamiseks klõpsake nuppu Windows.
3.  Klõpsake paani **Server Manager** .
4.  Klõpsake **Lisa rollid ja funktsioonid**.
5.  Klõpsake nuppu **Järgmine**
6.  Valige **roll või funktsiooni installimise**ja seejärel klõpsake nuppu **edasi**.
7.  Valige oma DNS-i virtuaalse masina (see peab olema esile tõstetud juba) ja seejärel klõpsake nuppu **edasi**.
8.  Märkige ruut **DNS-i serveri**.
9.  Klõpsake nuppu **Lisa funktsioone**ja seejärel klõpsake nuppu **Jätka**.
10. Klõpsake nuppu **Järgmine** kolm korda, ja klõpsake nuppu **Installi**. 

**DNS-i serveri roll liitmiseks Contoso-DNS-USA**

- Korrake juhiseid DNS-i roll lisamiseks **Contoso-DNS-US**.

##<a name="assign-dns-servers-to-the-virtual-networks"></a>Virtuaalne võrkude DNS-serverid määramine

**Kahe DNS-serverid registreerimiseks**

1.  Azure'i klassikaline portaalis nuppu **Uus**, **Võrguteenuste**, **VIRTUAALSE võrgu** **Registreerida DNS-i serveri**.
2.  Sisestage:
    - **Nimi**: Contoso-DNS-EL
    - **DNS-i serveri IP-aadress**: 10.1.0.4 – oma IP-aadressi kohustuslik sobitamine DNS-i serveri virtuaalse masina IP-aadress.
     
3.  Korrake toimingut Contoso-DNS-USA registreerima järgmised sätted:
    - **Nimi**: Contoso-DNS-USA
    - **DNS-i serveri IP-aadress**: 10.2.0.4

**Kahe virtuaalse võrgu kaks DNS-serverid määramiseks**

1.  Klõpsake vasakpoolsel paanil klassikaline portaalis **võrkude** .
2.  Klõpsake **Contoso-VNet-EL**.
3.  Klõpsake nuppu **Konfigureeri**.
4.  Valige jaotises **DNS-serverid** **Contoso-DNS-EL** .
5.  Klõpsake lehe allservas **salvestamine** ja klõpsake nuppu **Jah** kinnitage.
6.  Korrake toimingut määramiseks **Contoso-VNet-USA** virtuaalse võrgu **Contoso-USA-DNS-i** DNS-i serveri.

Kõik virtuaalmasinates, virtuaalse võrkudega juurutatud tuleb värskendada DNS-i server configuration taaskäivitama.

**Taaskäivitamiseks on virtuaalmasinates**

1. Klassikaline portaalist Azure'i **Virtuaalmasinates** klõpsake vasakul.
2. Klõpsake **Contoso-DNS-EL**.
3. Klõpsake **armatuurlaua** ülaosas.
4. Klõpsake **uuesti** all.
5. Korrake samu juhiseid taaskäivitamiseks **Contoso-DNS-US**.


##<a name="configure-dns-conditional-forwarders"></a>DNS-i edasisuunamislahendusi tingimusvormingu konfigureerimine

DNS-i server iga virtuaalse võrgus võib lahendada ainult selle virtuaalse võrgustikus DNS-i nimed. Peate tingimusvormingu ekspediitor osutamiseks omavahelistes DNS-serveri nime lahendused omavahelistes virtuaalse võrgu konfigureerimine.

Tingimusvormingu ekspediitor konfigureerimiseks peate teadma kahte DNS-serverite domeeni sufiksid. DNS-sufiksid võivad olla erinevad sõltuvalt kasutasite loomisel kuvatakse virtuaalmasinates pilveteenustega konfiguratsiooni. Iga DNS-i järelliite virtuaalse võrgu kasutada, tuleb teil lisada tingimusvormingu ekspediitor. 

**Domeeni sufiksid kaks DNS-serverite leidmiseks**

1. RDP **Contoso-DNS -**Liitu.
2. Avage Windows PowerShelli konsooli või Käsuviip.
3. Käivitage **ipconfig**ja kirjutage **kindla ühenduse DNS-i järelliite**.
4. Sulgege RDP-seansi, peate endiselt seda. 
5. Korrake samu juhiseid teada **kindla ühenduse DNS-i järelliite** **Contoso-DNS-**USA-s.


**DNS-i edasisuunamislahendusi konfigureerimine**
 
1.  **Contoso-DNS -**Liitu seansiga RDP nuppu Windowsi klahv vasakus alaservas.
2.  Klõpsake **Haldustööriistad**.
3.  Klõpsake **DNS-i**.
4.  Laiendage vasakpoolsel paanil **DSN-i**, **Contoso-DNS-EL**.
5.  Sisestage järgmine teave:
    - **DNS-i Domeen**: sisestage DNS-i järelliite Contoso DNS-i USA. Näide: Contoso-DNS-US.b5.internal.cloudapp.net.
    - **IP-aadresside juhtslaidi serverid**: sisestage 10.2.0.4, mis on Contoso-DNS-USA IP-aadress.
6.  Vajutage sisestusklahvi **ENTER**, ja seejärel klõpsake nuppu **OK**.  Nüüd on teil võimalik Contoso-DNS-EL Contoso-DNS-USA IP-aadressi lahendamiseks.
7.  Korrake juhiseid DNS-i ekspediitor lisamiseks DNS-i teenuse Contoso-DNS-USA virtual arvutisse koos järgmised väärtused:
    - **DNS-i Domeen**: sisestage DNS-i järelliite Contoso DNS-i EL. 
    - **IP-aadresside juhtslaidi serverid**: sisestage 10.2.0.4, mis on Contoso-DNS-Euroopa Liidu IP-aadress.

##<a name="test-the-name-resolution-across-the-virtual-networks"></a>Testida üle virtuaalse võrgu nimi eraldusvõime

Nüüd saate testida hostinimi resolutsioon üle virtuaalse võrkude. Tulemüür blokeerib ping vaikimisi.  Nslookupi abil saate lahendada omavahelistes võrkudes virtuaalmasinates DNS server (tuleb kasutada FQDN).  


##<a name="next-steps"></a>Järgmised sammud

Selles õpetuses olete õppinud, kuidas konfigureerida nimelahendus üle virtuaalse võrkude VPN ühendused. Muud kaks artikleid sarja kuuluvad:

- [Kahe Azure virtuaalse võrgu vahel virtuaalse privaatvõrgu konfigureerimine][hdinsight-hbase-geo-replication-vnet]
- [Konfigureerimine HBase geo dispersioonanalüüs][hdinsight-hbase-geo-replication]



[hdinsight-hbase-geo-replication]: hdinsight-hbase-geo-replication.md
[hdinsight-hbase-geo-replication-vnet]: hdinsight-hbase-geo-replication-configure-vnets.md
[powershell-install]: powershell-install-configure.md

[img-vnet-diagram]: ./media/hdinsight-hbase-geo-replication-configure-DNS/HDInsight.HBase.VPN.diagram.png 
