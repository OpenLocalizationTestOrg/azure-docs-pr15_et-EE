<properties
   pageTitle="Klassikaline juurutamise mudeli VNet-VNet ühenduse konfigureerimine | Microsoft Azure'i"
   description="Kuidas ühendada Azure virtuaalne võrkude koostöö PowerShelli ja Azure klassikaline portaali abil."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>


# <a name="configure-a-vnet-to-vnet-connection-for-the-classic-deployment-model"></a>Klassikaline juurutamise mudeli VNet-VNet ühenduse konfigureerimine

> [AZURE.SELECTOR]
- [Ressursihaldur - Azure'i portaal](vpn-gateway-howto-vnet-vnet-resource-manager-portal.md)
- [Ressursihaldur - PowerShelli](vpn-gateway-vnet-vnet-rm-ps.md)
- [Klassikaline – klassikaline portaal](virtual-networks-configure-vnet-to-vnet-connection.md)



Selles artiklis tutvustatakse juhiseid loomine ja ühenduse, virtuaalse võrgu koos näidise klassikaline juurutamise (tuntud ka kui teenuste haldamine). Järgmiste juhiste abil saate luua VNets ja lüüside ja PowerShelli kes ühenduse konfigureeriks VNet-VNet Azure klassikaline portaali. Ei saa konfigureerida ühenduse portaalis.

![VNet VNet ühenduvuse diagrammile](./media/virtual-networks-configure-vnet-to-vnet-connection/v2vclassic.png)


### <a name="deployment-models-and-methods-for-vnet-to-vnet-connections"></a>Juurutamise mudelite ja VNet-VNet ühendused meetodid

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)]

Järgmises tabelis on praegu saadaval juurutamise mudelid ja meetodid VNet-VNet konfiguratsioone. Kui konfigureerimistoimingute kohanimi on saadaval, me lingi otse sellest tabelist.

[AZURE.INCLUDE [vpn-gateway-table-vnet-vnet](../../includes/vpn-gateway-table-vnet-to-vnet-include.md)]

## <a name="about-vnet-to-vnet-connections"></a>VNet-VNet ühenduste kohta

Virtuaalse võrgu teise virtuaalse võrguga ühenduse loomisel (VNet VNet) on sarnane virtuaalse võrgu ühenduse oma kohapealse saidi asukoha. Mõlemat tüüpi ühenduvuse kasutavad VPN-lüüsi turvaline tunneliga, kasutades IPsec/IKE. 

VNets, loote võib olla erinevad tellimused ja eri piirkondadest. Saate ühendada VNet suhtlemine mitme saidi konfiguratsioone VNet. See võimaldab teil luua võrgu topoloogiatest, mis sisaldavad asutusesiseses Ühenduvus omavahel virtuaalse võrguühendus.


### <a name="why-connect-virtual-networks"></a>Miks ühenduse, virtuaalse võrkudes?

Kui soovite virtuaalne võrkude ühendamiseks järgmistel põhjustel.

- **Piirkonna geo-koondamise ja geo-kohalolek**
    - Saate häälestada oma geo-kopeerimine või sünkroonimise turvaline Ühenduvus ilma läheb üle Interneti-ühendusega lõpp-punktid.
    - Azure'i laadi koormusetasakaalustusteenuse ja Microsofti või muude tootjate klastrite tehnoloogia, saate häälestada tugevalt saadaval töökoormus geo-koondamise mitme Azure'i piirkondade lõikes. Üks oluline näide on SQL alati kohta kättesaadavus levirühmadega levitada mitme Azure'i piirkondade häälestamiseks.

- **Piirkondliku mitmekihilise rakenduste tugev eraldamise piiri**
    - Samas piirkonnas, saate häälestada mitmekihilise rakenduste abil mitme VNets ühendatud koos tugev eraldamise ja muu taseme turvaline side.

- **Tellimuse ettevõtte vaheline side Azure Cross**
    - Kui teil on mitu Azure tellimust, saate ühendada töökoormus kaudu erinevate tellimuste koos turvaliselt virtuaalse võrgu vahel.
    - Ettevõtetele või teenuse pakkujad, saate lubada rist – ettevõtte suhtlemine turvalist virtuaalse Privaatvõrgu tehnoloogia Azure'is.

### <a name="vnet-to-vnet-faq-for-classic-vnets"></a>Klassikaline VNets VNet-VNet KKK

- Virtuaalne võrkude saab samas või mõnes muus tellimused.

- Virtuaalne võrkude võib olla samas või mõnes muus Azure piirkondades (asukohad).

- Pilveteenus või tasakaalustamiseks lõpp-punkti koormus ei saa jaotada üle virtuaalse võrkude, isegi juhul, kui need on ühendatud koos.

- Ühenduse loomine mitme virtuaalse võrgu koos mis tahes VPN seadmed ei vaja.

- VNet-VNet toetab ühendusjooni Azure virtuaalse võrgustikke. See ei toeta virtuaalmasinates ühendusjooni ega pilveteenused, virtuaalse võrgu kasutamist.

- VNet-VNet nõuab dünaamilise marsruutimise lüüsid. Azure'i staatiline marsruutimine lüüsid pole toetatud.

- Virtuaalne võrguühendus saab kasutada korraga mitme saidi VPN. On kuni 10 VPN tunnelite virtuaalse võrgu VPN lüüsi ühenduse muude võrkude virtuaalse või kohapealse saidid.

- Aadress tühikute virtuaalse võrgu ja kohapealsete kohtvõrgu saitide peab ei kattuks. Kattuvate aadress tühikute põhjustab virtuaalne võrkude või netcfg konfigureerimine failide üleslaadimise nurjumise loomine.

- Liigsete tunnelid virtuaalne võrkude paari vahele ei toetata.

- Kõik VPN tunnelites VNet, sh P2S VPN, VPN-lüüsi ja sama VPN lüüsi sees SLA Azure läbilaskevõime ühiskasutusse anda.

- VNet-VNet liikluse läbib Azure nurgakivi üle.


## <a name="step1"></a>Samm 1 - leping IP-aadresside vahemikud

See on oluline otsustada, peate kasutama konfigureerida virtuaalse võrguga vahemike. Selle konfiguratsiooni puhul peab veenduge, et ükski oma VNet vahemike kattu omavahel, või mis tahes kohaliku võrkude loomisel.

Järgmine tabel näitab määratlemine oma VNets näide. Kasutage vahemikke, vaid üldjoontes. Kirjutage virtuaalse võrguga vahemikud. Peate selle teabe hiljem juhised.

**Näide sätted**

|Virtuaalse võrgu  |Aadressiruumi jaoks               |Piirkond      |Loob ühenduse kohtvõrgu saidi|
|:----------------|:---------------------------|:-----------|:-----------------------------|
|VNet1            |VNet1 (10.1.0.0/16)         |USA Lääne     |VNet2Local (10.2.0.0/16)      |
|VNet2            |VNet2 (10.2.0.0/16)         |Jaapan Ida  |VNet1Local (10.1.0.0/16)      |
  
## <a name="step-2---create-vnet1"></a>Samm 2 – VNet1 loomine

Selles etapis tuleb loome VNet1. Kui kasutate mõnda näidet, kindlasti asendada oma väärtused. Kui teie VNet juba olemas, te ei pea selle juhise. Kuid peate kontrollima IP-aadresside vahemikud ei kattuks vahemikud oma teise VNet, või mis tahes muud VNets, millele soovite ühendada.

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com)sisse logida. Selles artiklis kasutame klassikaline portaali, kuna mõni nõutav konfiguratsioon säte ei ole veel Azure'i portaalis.

2. Klõpsake Kuva alumises vasakus nurgas nuppu **Uus** > **Võrguteenuste** > **Virtuaalse võrgu** > **Luua kohandatud** konfiguratsiooniviisardi alustada. Viisardi liikudes lisada iga lehe määratud väärtused.

### <a name="virtual-network-details"></a>Virtuaalse võrgu üksikasjad

Virtuaalse võrgu üksikasjade lehel, sisestage järgmine teave:

  ![Virtuaalse võrgu üksikasjad](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736055.png)

  - **Nimi** – virtuaalse võrgu nimi. Näiteks VNet1.
  - **Asukoht** – virtuaalse võrgu loomisel, saate seostada ka Azure asukoht (piirkond). Näiteks, kui soovite oma VMs, virtuaalse võrgu asuma füüsilise Lääne USA juurutatud, valige soovitud kohta. Pärast selle loomist virtuaalse võrguga seotud asukohta ei saa muuta.

### <a name="dns-servers-and-vpn-connectivity"></a>DNS-serverid ja VPN Ühenduvus

Klõpsake lehel DNS-serverid ja virtuaalse Privaatvõrgu ühendus sisestage järgmine teave ja klõpsake parempoolses allnurgas noolenuppu järgmine.

  ![DNS-serverid ja VPN Ühenduvus](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736056.jpg)  

- **DNS-serverid** - sisestage DNS-i serveri nimi ja IP-aadress või valige rippmenüüst varem registreeritud DNS-i serveri. See säte ei loo DNS-i serveri. See võimaldab teil määrata DNS-serverid, mida soovite kasutada selle virtuaalse võrgu nimelahendus. Kui soovite nimelahendus virtuaalse võrgu vahel, peate oma DNS-i server, mitte abil nimelahendus, mis on esitatud Azure konfigureerimine.
- Ärge valige mis tahes P2S või S2S ühenduvuse märkeruudud. Klõpsake parempoolses allnurgas järgmisele kuvale liikumiseks klõpsake noolt.

### <a name="virtual-network-address-spaces"></a>Virtuaalse võrgu aadress tühikud

Määrake lehel virtuaalse võrgu aadress tühikute aadress vahemiku, mida soovite kasutada virtuaalse võrgu jaoks. Neid dünaamiliseks IP-aadresside (DIPS) VMs ja muud rolli eksemplarid, mis selle virtuaalse võrguga juurutamist määratud. 

Kui loote VNet, mis on ka teie asutusesisese võrguga ühenduse, on eriti oluline valige vahemik, mis ei kattuks mis tahes vahemikud, mida kasutatakse kohapealse võrgu jaoks. Sel juhul peate koordineerimine võrgu administraatori poole. Leida soovitud vahemiku IP-aadresside kaudu oma kohapealse võrgu aadressiruumi jaoks oma VNet kasutamiseks vajalikud võrguadministraatori poole.

  ![Virtuaalse võrgu aadress tühikute leht](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736057.jpg)

  - **Aadressiruumi** – alates ja aadresside arvu. Veenduge, et teie määratud aadressi tühikuid ei kattu aadress tühikuid, et teil on kohapealse võrgu. Selles näites kasutatakse 10.1.0.0/16 VNet1 jaoks.
  - **Lisa alamvõrgu** – alates ja aadresside arvu. Täiendavad alamvõrku pole kohustuslikud, kuid soovite luua eraldi alamvõrgu vms, mis on staatiline DIPS. Või võite olla teie VMs alamvõrku, eraldi oma rolli aknad.
 
Lehe ja virtuaalse võrgu parempoolses allnurgas **nuppu märke** hakkab loomine. Kui see on lõpule jõudnud, kuvatakse "Loodud" loetletud lehel võrgu olek.

## <a name="step-3---create-vnet2"></a>Samm 3 – VNet2 loomine

Järgmiseks korrake eelmist toimingut teise VNet loomiseks. Hiljem toimingutes ühendate kaks VNets. Saate viidata [näide sätted](#step1) etappi 1. Kui teie VNet juba olemas, te ei pea selle juhise. Siiski peate kontrollima, et IP-aadresside vahemikud ei kattuks mis tahes muu VNets või kohapealse võrgu ühenduse loomiseks soovitud.

## <a name="step-4---add-the-local-network-sites"></a>Juhis 4 – kohtvõrgu saitide lisamine

VNet-VNet konfiguratsiooni loomisel peate konfigureerima kohtvõrgu saidid, mis on näidatud **Kohaliku võrgu** lehe portaali. Azure'i kasutab määrata, kuidas abil saate marsruutida liiklust vahel on VNets määratud iga kohtvõrgu saidi sätted. Saate määratleda, mida soovite kasutada viidata iga kohtvõrgu saidi nimi. See on kõige parem kasutada midagi kirjeldav, kui valite hiljem toimingutes ripploendist väärtus.

Näiteks VNet1 loob ühenduse kohtvõrgu saidi loodud nimega "VNet2Local". Sätete VNet2Local sisaldavad aadress eesliiteid jaoks VNet2 ja VNet2 lüüsi avaliku IP-aadress. VNet2 loob ühenduse loomist kohalikus võrgus saidi nimega "VNet1Local", mis sisaldab aadress eesliiteid VNet1 ja VNet1 lüüsi avaliku IP-aadress.

### <a name="localnet"></a>Kohalikus võrgus saidi VNet1Local lisamine

1. Klõpsake Kuva alumises vasakus nurgas nuppu **Uus** > **Võrguteenuste** > **Virtuaalse võrgu** > **Lisamine kohalikus võrgus**.

2. Sisestage lehel **teie kohalikus võrgus üksikasjad** **nimi**nimi, mida soovite tähistada võrku, millega soovite ühenduse. Selle näite puhul saate kasutada "VNet1Local" IP-aadresside vahemikud ja lüüsi esitada VNet1.

3. Määrake **VPN seadme IP-aadress (valikuline)**, mis tahes kehtiv avaliku IP-aadress. Tavaliselt oleks tegelik välise IP-aadressi kasutatakse VPN seade. Puhul VNet-VNet konfiguratsioone, kasutage on määratud lüüsi oma VNet avaliku IP-aadress. Kuid, võttes arvesse, et olete loonud pole veel lüüsi, saate määrata mis tahes kehtiv avaliku IP-aadressi kohatäitena. Ärge jätke see tühjaks – see pole kohustuslik selle konfiguratsiooni puhul. Hiljem juhises tagasi minna neid sätteid ja konfigureerimine nende vastavate gateway IP-aadressid kui Azure genereeritud seda. Klõpsake noolt järgmisele kuvale liikumiseks.

4. **Määrake lehel aadressi**, sisestage IP address vahemik ja aadress count VNet1 jaoks. See peab vastama täpselt vahemik, mis on konfigureeritud lubama VNet1. Azure'i kasutab abil saate marsruutida liiklust, mis on mõeldud VNet1 määratud IP-aadresside vahemikud. Klõpsake nuppu kohtvõrgu loomiseks märke.

### <a name="add-the-local-network-site-vnet2local"></a>Kohalikus võrgus saidi VNet2Local lisamine

"VNet2Local" kohalikus võrgus saidi loomiseks kasutada ülal esitatud juhiseid. Vajaduse korral võib viidata väärtused samm 1, [näiteks sätted](#step1) .

### <a name="configure-each-vnet-to-point-to-a-local-network"></a>Iga VNet osutamiseks kohtvõrgu konfigureerimine

Iga VNet peavad osutama marsruutida liikluse soovitud vastav kohalikus võrgus. 

#### <a name="for-vnet1"></a>VNet1 jaoks

1. Liikuge lehe virtuaalse võrgu **VNet1** **konfigureerimine** . 
2. Valige jaotises-saidilt Ühenduvus valige "Ühenduse loomine kohaliku võrgu" ja valige rippmenüüst kohtvõrgu **VNet2Local** . 
3. Sätete salvestamine.

#### <a name="for-vnet2"></a>VNet2 jaoks

1. Liikuge lehe virtuaalse võrgu **VNet2** **konfigureerimine** . 
2. Valige jaotises-Saitide Ühenduvus "Ühenduse loomine kohaliku võrgu" valige **VNet1Local** kohtvõrgu rippmenüüst. 
3. Sätete salvestamine.

## <a name="step-5---configure-a-gateway-for-each-vnet"></a>Juhis 5 – iga VNet lüüsi konfigureerimine

Dünaamiline marsruutimine iga virtuaalse võrgu lüüsi konfigureerimine. Selle konfiguratsiooni ei toeta lüüsid staatilise marsruutimist. Kui kasutate VNets, mis olid eelnevalt konfigureeritud ja mis on juba dünaamiline marsruutimine lüüsid, peate pole selle juhise. Kui teie lüüsid on staatiline marsruutimine, peate need kustutada ja neid dünaamiliseks marsruutimist lüüsid nimega uuesti. Lüüsi kustutamisel avaliku IP-aadressi määratud see toob välja ja peate uuesti ja konfigureerima teie kohaliku võrgu ja VPN seadmete uue avaliku IP-aadressi uue lüüsi.

1. Kontrollige lehel **võrkude** , et veerus Olek virtuaalse võrgu jaoks on **loodud**.

2. Klõpsake veerus **Name** virtuaalse võrgu nimi. Selles näites kasutatakse "VNet1".

3. **Armatuurlaualehe,** Pange tähele, et see VNet pole veel konfigureeritud lüüsi. Kuvatakse selle oleku muutmiseks vastavalt läbite lüüsi konfigureerimine.

4. Klõpsake lehe allosas nuppu **Lüüsi loomine** ja **Dünaamiline marsruutimist**. Kui süsteemi palub teil kinnitada lüüsi, mis on loodud, klõpsake nuppu Jah.

    ![Lüüsi tüüp](./media/virtual-networks-configure-vnet-to-vnet-connection/IC717026.png)  

5. Lüüsi loomisel teade lüüsi pilti lehel muutub kollane ja kuvatakse tekst "Lüüsi loomine". Tavaliselt võtab aega umbes 30 minutit lüüsi loomiseks.

6. Korrake samu juhiseid VNet2. Te ei pea esimese VNet lüüsi lõpuleviimiseks enne alustamist oma muude VNet lüüsi loomiseks.

7. Kui lüüsi olek muutub "Ühendamine", iga lüüsi avaliku IP-aadress on nähtav armatuurlaud. Kirjutage IP-aadress, mis vastab iga VNet, jälgides, et mitte segada nende abil. Need on kohatäite IP-aadresside redigeerimisel VPN seadme iga kohtvõrgu jaoks kasutatud IP-aadressid.

## <a name="step-6---edit-the-local-network"></a>Samm 6 – kohalikus võrgus redigeerimine

1. Lehel **Kohaliku võrgu** nime kohaliku võrgu nimi, mida soovite redigeerida, klõpsake käsku **Redigeeri** lehe allosas. Sisestage **VPN seadme IP-aadress**lüüsi, mis vastab olevat VNet IP-aadress. Näiteks VNet1Local, panna määratud VNet1 gateway IP-aadress. Klõpsake lehe allosas olevat noolt.

2. Klõpsake lehe **Määrake aadress ruumi** ilma muudatuste tegemist teha märke klõpsake parempoolses allnurgas.

## <a name="step-7---create-the-vpn-connection"></a>Juhis 7 – saate luua VPN-ühendus

Kui eelnevate juhiste järgimisel on lõpule viidud, määrake IPsec/IKE eelnevalt jagatud võtmed ja ühenduse loomine. Selles teemas kirjeldatud toimingutega kasutab PowerShelli ja ei saa konfigureerida portaalis. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md) installimise Azure PowerShelli cmdlet-käskude kohta lisateavet. Veenduge, et alla laadida uusima versiooni teenuse haldus (SM) cmdlet-käsud. 

1. Avage Windows PowerShelli ja logige sisse.

        Add-AzureAccount

2. Valige tellimus, mis teie VNets asu.

        Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
        Select-AzureSubscription -SubscriptionName "<Subscription Name>"

3. Luua seoseid. Näited, Pange tähele, et ühiskasutusse antud võti on täpselt sama. Ühiskasutusega võti alati peab vastama.


    VNet1 VNet2 ühendus

        Set-AzureVNetGatewayKey -VNetName VNet1 -LocalNetworkSiteName VNet2Local -SharedKey A1b2C3D4

    VNet2 VNet1 ühendus

        Set-AzureVNetGatewayKey -VNetName VNet2 -LocalNetworkSiteName VNet1Local -SharedKey A1b2C3D4

4. Oodake, kuni ühendused lähtestada. Kui lüüsi on lähtestatud, lüüsi näeb välja umbes järgmine pilt.

    ![Lüüsi olek - ühendus](./media/virtual-networks-configure-vnet-to-vnet-connection/IC736059.jpg)  

    [AZURE.INCLUDE [vpn-gateway-no-nsg-include](../../includes/vpn-gateway-no-nsg-include.md)] 

## <a name="next-steps"></a>Järgmised sammud

Saate lisada virtuaalse võrguga virtuaalmasinates. Lugege lisateavet [Virtuaalmasinates dokumentatsiooni](https://azure.microsoft.com/documentation/services/virtual-machines/) .



[1]: ../hdinsight-hbase-geo-replication-configure-vnets.md
[2]: http://channel9.msdn.com/Series/Getting-started-with-Windows-Azure-HDInsight-Service/Configure-the-VPN-connectivity-between-two-Azure-virtual-networks
 
