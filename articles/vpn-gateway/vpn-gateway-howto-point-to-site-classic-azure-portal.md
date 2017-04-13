<properties
   pageTitle="Lüüsi punkti – saidile VPN-ühenduse Azure'i portaalis Azure virtuaalse võrgu konfigureerimine | Microsoft Azure'i"
   description="Turvaliselt ühendada Azure virtuaalse võrgu Azure'i portaalis punkti saidi VPN-lüüsi ühenduse loomise teel."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/17/2016"
   ms.author="cherylmc"/>

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-azure-portal"></a>Punkti saidi VNet, mis on Azure portaalis ühenduse konfigureerimine

> [AZURE.SELECTOR]
- [Ressursihaldur - Azure'i portaal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Ressursihaldur - PowerShelli](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klassikaline - Azure'i portaal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)

Selles artiklis tutvustatakse loomise on VNet punkti saidi ühendusega klassikaline juurutamise mudeli Azure'i portaalis. SharePointi saidil (P2S) konfiguratsioon võimaldab teil luua turvalist ühendust üksikute klientarvutist virtuaalse võrku. P2S ühendus on kasulik, kui soovite ühendada oma VNet mujalt, nagu kodus või konverentsi. Või kui teil on ainult mõned kliendid, kes on vaja virtuaalse võrguga.

Punkti saidi ühendused, pole vaja VPN seadme või avaliku IP-aadressi töötamiseks. VPN-ühendus on loodud ühendus alates klientarvuti. Punkti saidi ühenduste kohta leiate lisateavet teemast [VPN lüüsi KKK](vpn-gateway-vpn-faq.md#point-to-site-connections) ja [VPN lüüsi](vpn-gateway-about-vpngateways.md#point-to-site).

### <a name="deployment-models-and-methods-for-p2s-connections"></a>Juurutamise mudelite ja P2S ühendused meetodid

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Järgmises tabelis on kaks juurutamise mudelit ja saadaval juurutamise meetodid P2S konfiguratsioone. Kui konfigureerimistoimingute kohanimi on saadaval, me lingi otse sellest tabelist.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 

## <a name="basic-workflow"></a>Tavaline töövoo 

![Punkti-abil – saidile-skeemi] (./media/vpn-gateway-howto-point-to-site-rm-ps/p2srm.png "punkti – saidile")


Järgmistes jaotistes sõelub turvaline punkti saidi virtuaalse võrguga ühenduse loomiseks juhiseid. 

1. Virtuaalse võrgu ja VPN-lüüsi loomine
2. Luua serdid
3. CER-faili üleslaadimine
4. Luua VPN kliendi konfiguratsiooni pakett
5. Klientarvuti konfigureerimine
6. Azure'i ühendamine

### <a name="example-settings"></a>Näide sätted

Saate kasutada näiteks järgmisi sätteid:

- **Nimi: VNet1**
- **Aadress: 192.168.0.0/16**
- **Alamvõrgu nimi: FrontEnd**
- **Alamvõrgu aadress vahemiku: 192.168.1.0/24**
- **Tellimuse:** Kui teil on mitu tellimust, veenduge, et kasutate õige.
- **Ressursirühm: TestRG**
- **Asukoht: Ida-USA**
- **Ühenduse tüüp: punkti – saidile**
- **Kliendi aadressiruumi: 172.16.201.0/24**. VPN-i kliendid, kes ühenduse VNet, kasutamine punkti saidi seoses saadud määratletud kogumi IP-aadress.
- **GatewaySubnet: 192.168.200.0/24**. Lüüsi alamvõrgu peate kasutama nime "GatewaySubnet".
- **Suurus:** Valige lüüsi SKU-ga, mida soovite kasutada.
- **Marsruutimise tüüpi: dünaamiline**

## <a name="vnetvpn"></a>Jaotis 1 - virtuaalse võrgu ja VPN-lüüsi loomine

### <a name="createvnet"></a>Osa 1: Virtuaalse võrgu loomine

Kui teil pole veel virtuaalse võrk, luua. Pildid on toodud näited. Ärge unustage asendage väärtused ise. Loomiseks on VNet Azure portaali abil, siis tehke järgmist: 

1. Brauserist, liikuge [Azure portaali](http://portal.azure.com) ja vajadusel logige sisse oma Azure kontoga.

2. Klõpsake nuppu **Uus**. Tippige väljale **Otsing kuvatakse Marketplace'ist** "Virtuaalse võrgu". Otsige üles **Virtuaalse võrgu** tagastatud loendist ja **Virtuaalse võrgu** tera avamiseks klõpsake seda.

    ![Virtuaalse võrgu blade otsimine] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvnetportal700.png "Virtuaalse võrgu blade otsimine")

3. Virtuaalse võrgu labale loendist **Valige juurutamise mudeli** allosas valige **klassikaline**ja seejärel klõpsake nuppu **Loo**.

    ![Valige juurutamise mudel] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/selectmodel.png "Valige juurutamise mudel")

4. **Loo virtuaalse võrgu** enne, VNet sätete konfigureerimine. Klõpsake selle tera saate lisada oma esimese aadressiruumi ja ühe alamvõrgu aadress vahemiku. Kui olete lõpetanud, kuvatakse VNet loomise, saate minge tagasi ja täiendavad alamvõrku ja aadress tühikute lisamine.

    ![Loo virtuaalse võrgu blade] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vnet125.png "Loo virtuaalse võrgu blade")

5. Veenduge, et **tellimus** on õige. Saate muuta tellimuste drop-down abil.

6. Klõpsake **ressursirühm** ja valige ressursi olemasolevasse rühma või luua uue tippides uue ressursi rühma nime. Kui loote uue rühma, nime ressursirühm vastavalt oma kavandatud väärtused. Ressursi rühmade kohta lisateabe saamiseks külastage [Azure'i ressursihaldur ülevaade](azure-resource-manager/resource-group-overview.md#resource-groups).

7. Seejärel valige oma VNet sätete **kohta** . Asukoht määrab, kus ressursse, mis selles VNet juurutamiseks hakkavad asuma.

8. Valige **armatuurlaud PIN-koodi** , kui soovite saama oma VNet armatuurlaual hõlpsalt leida, ja seejärel klõpsake nuppu **Loo**.
    
    ![PIN-koodi armatuurlaud] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/pintodashboard150.png "PIN-koodi armatuurlaud")


9. Pärast nupu Loo, kuvatakse paan armatuurlauale, mis kajastab oma VNet edenemist. Paan on VNet loomisel.

    ![Paani virtuaalse võrgu loomine] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/deploying150.png "Paani virtuaalse võrgu loomine")

10. Kui olete loonud virtuaalse võrgu, saate lisada nimelahendus käsitlemiseks DNS-i serveri IP-aadress. Avage virtuaalse võrgu sätteid, klõpsake DNS-serverid ja lisage DNS-i server, mida soovite kasutada IP-aadress. See säte ei Loo uus DNS-i server. Lisage DNS-i server, mis oma ressursse ei saa suhelda kindlasti.

Kui virtuaalse võrgu on loodud, kuvatakse **loodud** loendis Azure klassikaline portaalis lehel võrgu **olek** .

### <a name="gateway"></a>Osa 2: Lüüsi alamvõrgu ja dünaamilisi marsruutimise lüüsi loomine

Selles etapis loote lüüsi alamvõrgu ja dünaamilisi marsruutimise lüüsi. Azure'i portaalis klassikaline juurutamise mudeli lüüsi alamvõrgu ja lüüsi loomine saab teha sama konfiguratsiooni labad kaudu.

1. Liikuge portaalis virtuaalse võrk, mille jaoks soovite luua lüüsi.

2. Enne virtuaalse võrgu **Ülevaade** enne, jaotises VPN ühenduste jaoks klõpsake **lüüsi**.

    ![Klõpsake siin, et lüüsi loomine] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/beforegw125.png "Klõpsake siin, et lüüsi loomine")


3. Enne **Uue VPN-ühendus** , valige **SharePointi saidil**.

    ![P2S ühenduse tüüp] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/newvpnconnect.png "P2S ühenduse tüüp")

4. **Kliendi aadressi ruumi**, lisage IP-aadresside vahemiku. See on lahtrivahemik, kust VPN kliendid saavad IP-aadress ühenduse loomise korral. Kustutage automaatselt sisestatud vahemik ja lisage oma.

    ![Kliendi aadressiruumi jaoks] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientaddress.png "Kliendi aadressiruumi jaoks")

5. Märkige ruut **Loo lüüsi kohe** .

    ![Loo lüüsi kohe] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/creategwimm.png "Loo lüüsi kohe")

6. Klõpsake nuppu **Valikuline lüüsi konfigureerimine** **lüüsi konfigureerimine** tera avamiseks.

    ![Klõpsake nuppu Valikuline lüüsi konfigureerimine] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/optsubnet125.png "Klõpsake nuppu Valikuline lüüsi konfigureerimine")

7. Klõpsake **lüüsi alamvõrgu**lisamiseks **alamvõrgu konfigureerimine vajalik sätted** . On võimalik luua lüüsi alamvõrgu /29 võimalikult väike, soovitame suuremat alamvõrku, mis sisaldab rohkem aadresse, valides vähemalt /28 või /27 luua. See võimaldab piisavalt aadressid mahutamiseks soovite edaspidi võimalike täiendavate kuvatakse.

    >[AZURE.IMPORTANT] Lüüsi alamvõrku töötamisel vältida võrgu turberühma (NSG) lüüsi alamvõrgu seostada. Turberühma võrgu selle alamvõrgu seostamise võib põhjustada teie VPN-lüüsi enam õigesti töötada. Võrgu turberühmad kohta leiate lisateavet teemast [mis on võrgu turberühma?](../articles/virtual-network/virtual-networks-nsg.md)

    ![GatewaySubnet lisamine] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsubnet125.png "GatewaySubnet lisamine")

8. Lüüsi **suuruse**valimine See on lüüsi virtuaalse võrgu lüüsi loomiseks kasutate SKU-ga. Portaalis SKU vaikimisi on **lihtne**. Lüüsi SKU-de jaoks kohta leiate lisateavet teemast [VPN lüüsi sätete kohta](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).

    ![lüüsi suurus](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/gwsize125.png)

9. Valige oma Gateway **Marsruutimine tüüp** . P2S konfiguratsioone vaja **dünaamilise** marsruutimise tüüp. Kui olete lõpetanud selle tera konfigureerimine, klõpsake nuppu **OK** .

    ![Konfigureerimine marsruutimise tüüp] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/routingtype125.png "Konfigureerimine marsruutimise tüüp")

10. Enne **Uue VPN-ühendus** , klõpsake nuppu **OK** tera allosas virtuaalse võrgu lüüsi loomise alustamiseks. See võib võtta kuni 45 minutit. 


## <a name="generatecerts"></a>Jaotis 2 - luua serdid

Serte kasutatakse Azure autentida VPN kliente VPN punkti saidi jaoks. Eksportimist avaliku serdi andmed (mitte privaatvõti) nii, nagu Base – 64 X.509 CER-fail juurkausta serdi loodud lahenduse ettevõtte sert või juurkausta iseallkirjastatud sert. Seejärel importida andmed avaliku serdi juurkausta serdi Azure. Lisaks on vaja luua kliendi serti juurkausta serdi klientidele. Iga kliendi, mis soovib P2S kaudu virtuaalse võrguga ühenduse loomiseks peab olema kliendi installitud juur sertifikaadi põhjal loodud.

### <a name="cer"></a>Osa 1: Saamiseks serdi juurkausta CER-faili


Kui kasutate lahenduse enterprise, saate oma olemasolev sert ahelas. Kui te ei kasuta ettevõte CA lahenduse, saate luua iseallkirjastatud juurkausta cert. Iseallkirjastatud cert loomise meetod on makecert.

- Kui kasutate mõnda ettevõtte sert süsteemi, saada CER-faili jaoks juurkausta sert, mida soovite kasutada. 

- Kui te ei kasuta lahenduse ettevõtte sert, peate juurkausta iseallkirjastatud serdi loomiseks. Windows 10 juhised, võib viidata [töötamine iseallkirjastatud juursertide punkti saidi jaoks](vpn-gateway-certificates-point-to-site.md).

1. Saada sert CER-fail, avage **tekst certmgr.msc** ja leidke juurkausta sert. Paremklõpsake root iseallkirjastatud serdi, klõpsake käsku **Kõik tööülesanded**ja seejärel klõpsake nuppu **ekspordi**. **Serdi ekspordiviisardi**avaneb.

2. Viisardi, klõpsake nuppu **edasi**, valige **ei, ekspordi privaatvõti**ja seejärel klõpsake nuppu **edasi**.

3. Lehel **Ekspordi failivorming** valige **Base-64-kodeeritud X.509 (. CER).** Klõpsake nuppu **edasi**. 

4. Klõpsake selle **faili eksportida**, **liikuge** asukohta, kuhu soovite eksportida sert. Serdi faili nimi **faili nimi**. Klõpsake nuppu **edasi**.

5. Klõpsake nuppu **valmis** serdi eksportida.


### <a name="genclientcert"></a>Osa 2: Luua kliendi sert

Kas saate luua kordumatu sert iga kliendi, mis loob ühenduse või kasutada mitme kliendi serti. Võimalus tühistada ühe serdi vajaduse korral on see eelis, nendele kordumatu kliendi serdid. Muul juhul, kui kõik kasutab sama kliendi serti ja leiate, et peate ühe kliendi serdi tühistada, pead luua ja installige kõik kliendid, mida kasutada seda serti autentida uus.

- Kui kasutate lahenduse ettevõtte sert, luua kliendi serti levinud nimi väärtus vorming 'name@yourdomain.com', asemel "domeeni name\username" vorming. 

- Kui kasutate iseallkirjastatud serdi, vt [töötamine iseallkirjastatud juursertide punkti saidi jaoks](vpn-gateway-certificates-point-to-site.md) luua kliendi serti.

### <a name="exportclientcert"></a>Osa 3: Ekspordi kliendi sert

Installige kliendi sertifikaadi igas arvutis, mida soovite virtuaalse võrguga. Kliendi sert on vaja autentimist. Kliendi serdi installimiseks automatiseerida või saate selle käsitsi installida. Järgmised toimingud teile eksportimine ja käsitsi kliendi serdi installimiseks kaudu.

1. Kliendi sertifikaadi eksportida saate *tekst certmgr.msc*. Paremklõpsake kliendi sert, mida soovite eksportida, klõpsake käsku **Kõik tööülesanded**ja seejärel klõpsake nuppu **ekspordi**.
2. Eksportida privaatvõti kliendi sert. See on *pfx-* fail. Veenduge, et salvestada või parool (võti), mida saate seada selle serdi meeles pidada.

## <a name="upload"></a>Jaotis 3 – Laadi juurkausta serdi CER-fail

Pärast lüüsi on loodud, saate üles laadida CER-faili jaoks usaldusväärne juur serdi Azure. Saate üles laadida kuni 20 juursertide failid. Privaatvõti juurkausta serti ei saa Azure üles laadida. Kui CER-fail on üles laaditud, Azure'i kasutab seda autentida klientides virtuaalse võrguga ühenduse luua.

1. Teie VNet höövlitera **VPN-ühendused** jaotises Ava soovitud **punkti saidi VPN-ühenduse **Kliendid** pildi klõpsamisel** tera.

    ![Kliendid] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clients125.png "Kliendid")

2. **SharePointi saidil ühenduse** tera, klõpsake nuppu **Halda serdid** **serdid** tera avamiseks.<br>

    ![Sertide blade](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/ptsmanage.png "Certificates blade")<br><br>


3. Enne **serdid** , klõpsake **üleslaadimine** tera **serdi üleslaadimine** avamiseks.<br>

    ![Sertide blade üleslaadimine](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/uploadcerts.png "Upload certificates blade")<br>


4. Klõpsake kausta pilti, et sirvida CER-fail. Valige fail ja seejärel klõpsake nuppu **OK**. Värskendage lehte üles laaditud serdi kuvamiseks enne **serdid** .

    ![Laadige üles sert](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/upload.png "Upload certificate")<br>
    

## <a name="vpnclientconfig"></a>Jaotis 4 - luua VPN kliendi konfiguratsiooni pakett

Virtuaalse võrguga ühenduse loomiseks, peate samuti konfigureerimine VPN klient. Klientarvuti nõuab kliendi serti nii proper VPN kliendi konfiguratsiooni paketi, et ühendada.

VPN kliendi pakett sisaldab konfiguratsiooniteavet VPN klienttarkvara Windowsi sisse ehitatud konfigureerimiseks. Paketi ei saa installida täiendavat tarkvara. Sätted on virtuaalse võrgu, mida soovite ühendada. Kliendi operatsioonisüsteemid, mis on toetatud loendi leiate teemast [SharePointi saidil ühendused](vpn-gateway-vpn-faq.md#point-to-site-connections) VPN lüüsi KKK osas. 

### <a name="to-generate-the-vpn-client-configuration-package"></a>VPN kliendi konfiguratsiooni paketi loomiseks

1. Azure'i portaalis **Ülevaade** tera jaoks oma VNet **VPN-ühendused**, Ava soovitud **punkti saidil VPN-ühendus kliendi pildi klõpsamisel** tera.
2. Kohal, et **punkti saidi VPN ühenduse** tera, klõpsake nuppu Laadi alla paketi, mis vastab kliendi operatsioonisüsteem, kuhu on installitud:

 - 64-bitine klienti, valige **VPN klient (64-bitine)**.
 - 32-bitine klienti, valige **VPN klient (32-bitine)**.

    ![Laadige VPN kliendi konfiguratsiooni](./media/vpn-gateway-howto-point-to-site-classic-azure-portal/dlclient.png "Download VPN client configuration package")<br>


3. Kuvatakse teade, et Azure'i tekitab VPN kliendi konfiguratsiooni paketi virtuaalse võrgu jaoks. Pärast paar minutit, luuakse paketi ja kuvatakse sõnumi kohalikus arvutis, et paketi on alla laaditud. Konfiguratsiooni paketi faili salvestada. Kas see installida iga klientarvuti loob ühenduse, virtuaalse võrguühendust P2S.


## <a name="clientconfiguration"></a>Jaotis 5 - klientarvuti konfigureerimine

### <a name="part-1-install-the-client-certificate"></a>Osa 1: Installi kliendi sert

Iga klientarvuti peab olema kliendi serti autentimiseks. Kliendi sert installimisel peate kliendi serdi eksportimisel loodud parool.

1. Kopeerige klientarvuti pfx-fail.
2. Topeltklõpsake selle installimiseks pfx-faili. Muutke installimise kohta.

### <a name="part-2-install-the-vpn-client-configuration-package"></a>Osa 2: Installi VPN kliendi konfiguratsiooni pakett

Saate kasutada sama VPN kliendi konfiguratsiooni pakett iga kliendi arvutis, tingimusel, et versioon vastab arhitektuur kliendi jaoks.

1. Kopeerige konfiguratsioonifail Kohalik arvuti, mida soovite virtuaalse võrguga ja topeltklõpsake faili .exe. 

2. Kui pakett on installitud, saate alustada VPN-ühendus. Microsoft pole allkirjastatud paketi konfigureerimine. Võib kasutada oma asutuse allkirjastamiseks teenuse allkirjastatav või logige ise [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327)abil. See on OK, et kasutada ilma allkirjastamine. Kui paketi pole sisse logitud, kuvatakse hoiatus, kui installite paketi.

3. Klientarvuti, liikuge **Võrgu sätteid** ja klõpsake **VPN**. Kuvatakse loendis ühendus. See näitab virtuaalse võrgu nimi, et seda ühendust ja näeb välja umbes järgmine: 

    ![VPN klient] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/vpn.png "VNet VPN klient")

## <a name="connect"></a>Jaotis 6 - ühenduse Azure'i

### <a name="connect-to-your-vnet"></a>Ühenduse loomine oma VNet

1. Ühenduse loomiseks oma VNet klientarvuti, liikuge VPN-ühendused ja otsige üles loodud VPN-ühendus. See on nime virtuaalse võrgu sama nimi. Klõpsake nuppu **Loo ühendus**. Hüpikaknas kuvatakse võidakse kuvada, mis viitab serdi abil. Kui see juhtub, klõpsake nuppu **Jätka** kasutada administraatoriõigusi. 

2. **Ühenduse** oleku lehel käsku **Ühenda** alustamiseks ühendus. Kui kuvatakse **Serdi valimine** Kuva, veenduge, et kliendi sert, kus on üks, mida soovite kasutada ühenduse. Kui see pole õige serdi valimiseks kasutage ripploendi noolt ja seejärel klõpsake nuppu **OK**.

    ![Ühenduse loomine] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/clientconnect.png "Kliendi VPN-ühendus")

3. Nüüd peaks teie ühendust luua.

    ![Asutatud ühendus] (./media/vpn-gateway-howto-point-to-site-classic-azure-portal/connected.png "Ühendust luua")

### <a name="verify-the-vpn-connection"></a>Veenduge, et VPN-ühendus

1. Veenduge, et VPN-ühendus on aktiivne, avage administraatoriõigustes Käsuviip ja käivitage *ipconfig/kõik*.
2. Tulemusi vaadata. Pange tähele, et olete saanud IP-aadress on üks teie VNet loomisel määratud punkti – saidile Ühenduvus aadress vahemikus aadressid. Tulemused peaks olema umbes selline:

Näide:



    PPP adapter VNet1:
        Connection-specific DNS Suffix .:
        Description.....................: VNet1
        Physical Address................:
        DHCP Enabled....................: No
        Autoconfiguration Enabled.......: Yes
        IPv4 Address....................: 192.168.130.2(Preferred)
        Subnet Mask.....................: 255.255.255.255
        Default Gateway.................:
        NetBIOS over Tcpip..............: Enabled

## <a name="next-steps"></a>Järgmised sammud

Saate lisada virtuaalse võrgu virtuaalmasinates. Vaadake, [Kuidas luua kohandatud virtuaalse masina](../virtual-machines/virtual-machines-windows-classic-createportal.md).