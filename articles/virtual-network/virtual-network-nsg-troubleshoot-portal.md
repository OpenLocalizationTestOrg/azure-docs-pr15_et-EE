<properties 
   pageTitle="Tõrkeotsing võrgu turberühmad – portaali | Microsoft Azure'i"
   description="Saate teada, kuidas tõrkeotsing võrgu turberühmad Azure'i ressursihaldur juurutamise mudeli Azure'i portaalis."
   services="virtual-network"
   documentationCenter="na"
   authors="AnithaAdusumilli"
   manager="narayan"
   editor=""
   tags="azure-resource-manager"
/>
<tags 
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/23/2016"
   ms.author="anithaa" />

# <a name="troubleshoot-network-security-groups-using-the-azure-portal"></a>Võrgu turberühmad Azure'i portaalis tõrkeotsing

> [AZURE.SELECTOR]
- [Azure'i portaal](virtual-network-nsg-troubleshoot-portal.md)
- [PowerShelli](virtual-network-nsg-troubleshoot-powershell.md)

Kui arvuti virtuaalne (VM) konfigureeritud võrgu turberühmad (NSGs) ja esineb VM ühenduvusprobleemide, selles artiklis antakse ülevaade diagnostika võimalused NSGs täpsemaks tõrkeotsinguks.

NSGs võimaldavad juhtida liiklust tüüpi selle voo ja sealt virtuaalmasinates (VMs). NSGs saab rakendada alamvõrku Azure virtuaalse võrgu (VNet), võrgu liidesed (NIC) või mõlemad. Efektiivse kohaldatakse Võrguadapter on liitmise reeglid, mis on olemas NSGs, mis on rakendatud Võrguadapter ja alamvõrku, mis on ühendatud. Reeglite üle nende NSGs vahel saate omavahel konflikti ja mõjutada on VM võrguühendus.  

Tõhus turvalisuse reegleid saate vaadata oma NSGs vastavalt oma VM NICs on rakendatud. Selles artiklis kirjeldatakse järgmiste reeglite kasutamine Azure ressursihaldur juurutamise mudeli VM ühenduvusprobleemide tõ. Kui olete tuttav VNet ja NSG põhimõtet, lugeda artikleid [Virtual võrk](virtual-networks-overview.md) ja [võrgu turberühmad](virtual-networks-nsg.md) ülevaade.

## <a name="using-effective-security-rules-to-troubleshoot-vm-traffic-flow"></a>Tõhus turvalisus reeglite kasutamine VM liiklust tõrkeotsing

Stsenaarium, et järgmine on näide levinud Interneti-ühenduse probleemi:

Nimega *VM1* VM on osa on alamvõrgu *Subnet1* sees on VNet nimega *WestUS-VNet1*nimega. VM, kasutades TCP pordi 3389 RDP ühenduse loomise katse nurjus. NSGs rakendatakse NIC *VM1-NIC1* ja alamvõrgu *Subnet1*. TCP-port 3389 liiklust on lubatud NSG, mis on seotud võrgu liidese *VM1-NIC1*, kuid TCP ping-VM1 isiku porti 3389 nurjub.

Kuigi see näide kasutab TCP port 3389, saab määratlemiseks sissetulevate ja väljaminevate ühenduse tõrkeid, mis tahes Port toimige järgmiselt.

### <a name="view-effective-security-rules-for-a-virtual-machine"></a>Tõhus turvalisuse reeglid virtuaalse masina vaatamine

Järgmiste juhiste VM NSGs tõrkeotsing:

Saate vaadata NIC, VM, ise tõhus turvalisuse reeglite täieliku loendi. Saate lisada, muuta ja kustutada nii NIC ja alamvõrgu NSG reeglid keelest tõhusad kui teil on õigus teha järgmisi toiminguid.

1. Veebisaidil https://portal.azure.com Azure portaali sisse logida.
2. Klõpsake nuppu **rohkem teenuseid**, siis klõpsake kuvatavas loendis **virtuaalmasinates** .
3. Valige loendist, mis kuvatakse tõrkeotsingu VM ja VM abaluuga suvandid kuvatakse.
4. Klõpsake **diagnoosimine & lahendamine** ja seejärel valige üldine probleem. Selle näite puhul **ei saa ühendust oma Windows VM** on märgitud. 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image1.png)

5. Juhised kuvatakse jaotises probleem, nagu on näidatud järgmisel pildil: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image2.png)

    Klõpsake nuppu *tõhus turvalisuse rühma reeglid* loendis soovituslikku toimingut.

6. **Saada tõhus turvalisuse reeglid** tera kuvatakse, nagu on näidatud järgmisel pildil:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image3.png)

    Järgmistes jaotistes pildi teade:

    - **Ulatus:** Määratud *VM1*, VM valitud sammus 3.
    - **Võrgu liidese:** *VM1-NIC1* oleks märgitud. VM võib olla mitu võrgu liidesed (NIC). Iga NIC võib olla kordumatu tõhus turvalisuse reeglid. Tõrkeotsingu, peate vaadata iga NIC. tõhus turvalisuse reeglid
    - **Seotud NSGs:** NSGs saab rakendada NIC nii alamvõrgu NIC on ühendatud. Järgmisel pildil on NSG rakendatud NIC nii alamvõrku, mis on ühendatud. Võite klõpsata NSG otse muuta soovitud NSGs reeglite nimesid.
    - **VM1-nsg menüü:** Pilt kuvatakse reeglite loendi on NSG, mis on rakendatud NIC. Iga kord, kui mõni NSG on loodud, luuakse Azure'i mitu vaikereeglit. Te ei saa eemaldada vaikimisi reeglite, kuid saate neid alistada, kõrgema prioriteedi reeglite abil. Vaikimisi reeglite kohta lisateabe saamiseks lugege artiklit [NSG ülevaade](virtual-networks-nsg.md#default-rules) .
    - **Sihtkoht veerg:** Mõned reeglid on teksti veerus, samas kui teised on aadress eesliiteid. Tekst on vaikimisi sildid, kui dokument on loodud turvalisuse reegel rakendatud nimi. Sildid on antud süsteemi identifikaatoritest, mis tähistavad mitme eesliidete tähised. Reegli sildiga, nt *AllowInternetOutBound*, valides on loetletud eesliiteid **aadress eesliiteid** tera.
    - **Allalaadimine:** Reeglite loendi võib olla pikk. CSV-faili reeglite ühenduseta analüüsi jaoks saate alla laadida, klõpsates **alla laadida** ja faili salvestamist.
    - **AllowRDP** Sissetulev reegel: see reegel võimaldab RDP ühendused VM.
7. Klõpsake vahekaarti **Subnet1-NSG** vaatamiseks tõhus reegleid: NSG, mis on rakendatud alamvõrku, nagu on näidatud järgmisel pildil: 

    ![](./media/virtual-network-nsg-troubleshoot-portal/image4.png)

    Pange tähele *denyRDP* **Sissetulev** reegel. Enne reeglite rakendada võrgu liidese hinnatakse rakendada alamvõrgu sissetulevad reeglid. Kuna Keela reegel rakendatakse alamvõrgu, taotluse ühenduse TCP 3389 nurjub, kuna hinnatakse kunagi luba reegli juures NIC. 

    *DenyRDP* reegel on põhjus, miks ei suuda RDP-ühendus. Eemaldamise peaks probleemi lahendada.

    >[AZURE.NOTE]Kui seostatud NIC VM pole töötab või NSGs pole rakendatud NIC või alamvõrgu, kuvatakse reeglid.

8. NSG reeglite redigeerimiseks klõpsake jaotises **Seotud NSGs** *Subnet1-NSG* .
   Avatakse **Subnet1-NSG** tera. Saate redigeerida otse reegleid, klõpsates **turvalisuse sissetulevad reeglid**.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image7.png)

9. Pärast **Subnet1-NSG** *denyRDP* Sissetulev reegel eemaldamine ja lisamine reegli *allowRDP* , näeb efektiivse reeglite loendi järgmisel pildil:

    ![](./media/virtual-network-nsg-troubleshoot-portal/image8.png)

    Veenduge, et TCP port 3389 on avatud, avades RDP ühenduse VM või PsPing tööriista abil. Saate selle kohta leiate lisateavet Pspingi abil lugemise [PsPing allalaadimislehele](https://technet.microsoft.com/sysinternals/psping.aspx).

### <a name="view-effective-security-rules-for-a-network-interface"></a>Tõhus turvalisuse reeglid võrgu kasutajaliidese kuvamine

Kui teie VM liiklust mõjutab jaoks teatud NIC, saate vaadata efektiivse reegleid täieliku loendi jaoks NIC kontekstist võrgu liidesed, tehes järgmist:

1. Veebisaidil https://portal.azure.com Azure portaali sisse logida.
2. Klõpsake nuppu **rohkem teenuseid**, siis klõpsake kuvatavas loendis **võrgu liidesed** .
3. Valige võrgu liidese. Järgmisel pildil on valitud soovitud NIC *VM1-NIC1* nimega.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image5.png)

    Pange tähele, et **ulatus** on määratud valitud võrgu liidese. Andmed kohta leiate lisateavet lugege käesoleva artikli jaotise **Tõrkeotsing NSGs VM** juhist 6.

    >[AZURE.NOTE] Kui mõni NSG eemaldatakse võrgu liidese, alamvõrgu NSG on endiselt tõhus antud NIC. Sel juhul ainult näitaks väljund alamvõrgu NSG reeglid. Reeglite kuvatakse ainult siis, kui NIC on lisatud VM.

4. Saate redigeerida otse NSGs Võrguadapter ja on alamvõrgu seotud reeglid. Saate teada, kuidas lugeda juhis 8 käesoleva artikli jaotise **kuvamine tõhus turvalisuse reeglid virtuaalse masina** .

## <a name="view-effective-security-rules-for-a-network-security-group-nsg"></a>Tõhus turvalisuse reeglid võrgu turberühma (NSG) vaatamine

NSG reeglite muutmisel soovite läbi vaadata kindla VM lisamisest reegleid mõju. Saate vaadata tõhus turvalisuse reeglid täieliku loendi NICs, mis on rakendatud antud NSG, ilma konteksti antud NSG keelest aktiveerimine. Tõrkeotsing on NSG efektiivse reegleid, tehke järgmist:

1. Veebisaidil https://portal.azure.com Azure portaali sisse logida.
2. Klõpsake nuppu **rohkem teenuseid**, siis klõpsake kuvatavas loendis **võrgu turberühmad** .
3. Valige soovitud NSG. Järgmisel pildil on NSG nimega VM1-nsg valitud.

    ![](./media/virtual-network-nsg-troubleshoot-portal/image6.png)

    Järgmistes jaotistes eelmise pildi teade:

    - **Ulatus:** Seadke NSG, mis on valitud.
    - **Virtuaalse masina:** Mõne NSG rakendatakse soovitud alamvõrku, rakendatakse see kõigi võrgu liidesed manustatud kõik VMs ühendatud alamvõrku. Selles loendis kuvatakse kõik selle NSG rakendatakse VMs. Saate valida loendist mõni VM.

    >[AZURE.NOTE] Kui mõni NSG rakendatakse ainult mõnda tühja alamvõrku, VMs pole loendis. Kui mõni NSG rakendatakse NIC, mis ei ole seostatud VM, nende NICs ei loetletakse ka. 
    - **Network kasutajaliidese:** VM võib olla mitu võrgu liidesed. Saate valida mõne valitud VM ühendatud võrgu liides.
    - **AssociatedNSGs:** Igal ajal Võrguadapter võib olla kahe efektiivse NSGs, üks NIC ja muud alamvõrgu rakendatud. Kuigi ulatus on valitud VM1 nsg, kui NIC on ka tõhus alamvõrgu NSG, kuvatakse väljund nii NSGs.
4. Saate redigeerida otse NSGs NIC või alamvõrgu seotud reeglid. Saate teada, kuidas lugeda juhis 8 käesoleva artikli jaotise **kuvamine tõhus turvalisuse reeglid virtuaalse masina** .

Andmed kohta lisateabe saamiseks lugege käesoleva artikli jaotises **Vaade tõhus turvalisuse reeglid virtuaalse masina** juhist 6.

>[AZURE.NOTE] Kui soovitud asukoht ja alamvõrgu saate iga on ainult üks NSG rakendatud, on NSG võib olla seotud mitu NICs ja mitu alamvõrku.

## <a name="considerations"></a>Kaalutlused

Kui ühendusprobleemide tõrkeotsing, võtke arvesse järgmist.

- NSG vaikereeglit kuvatakse sissetuleva juurdepääsu Interneti kaudu ja võimaldab üksnes VNet sissetulev liiklus. Reeglite lisatakse selgesõnaliselt sissetuleva juurdepääsu lubamiseks Internet, vastavalt vajadusele.
- Kui NSG turvalisuse reeglid põhjustab mõne VM võrguühenduse nurjumise, probleem võib olla järgmised.
    - Tulemüüri tarkvara on VM operatsioonisüsteemis töötab
    - Marsruudib virtuaalne seadmed või kohapealse liikluse konfigureeritud. Kohapealse sunnitud tunneling kaudu saate Interneti-liikluse ümber. See säte, olenevalt sellest, kuidas käsitleb kohapealse võrgu riistvara see liiklus ei pruugi RDP/SSH ühendus Interneti kaudu oma VM. Lugege artiklit [Tõrkeotsing marsruudib](virtual-network-routes-troubleshoot-powershell.md) teavet, mida võib takistada voo ja sealt VM liikluse marsruutimiseks probleemide väljaselgitamiseks. 
- Kui teil on piilus VNets, vaikimisi, laiendage VIRTUAL_NETWORK sildi automaatselt kaasatav eesliiteid piilus VNets. Saate vaadata nende eesliiteid loendis **ExpandedAddressPrefix** seotud VNet silmitsemine ühenduvuse seotud probleemide tõrkeotsinguks. 
- Tõhus turvalisuse reeglid kuvatakse vaid siis, kui on on seostatud soovitud VM NIC ja või alamvõrgu NSG. 
- Kui tegemist on pole NSGs seostatud NIC alamvõrgu ja teil on määratud oma VM avaliku IP-aadress, avatakse kõik pordid sissetulevate ja väljaminevate juurdepääsu. Kui VM on avaliku IP-aadressi, rakendades NSGs NIC või alamvõrgu tungivalt soovitatav.