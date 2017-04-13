<properties
   pageTitle="Lüüsi punkti saidi VPN-ühenduse klassikaline portaalis Azure virtuaalse võrgu konfigureerimine | Microsoft Azure'i"
   description="Turvaliselt ühendada Azure virtuaalse võrgu lüüsi punkti saidi VPN-ühenduse loomise teel."
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

# <a name="configure-a-point-to-site-connection-to-a-vnet-using-the-classic-portal"></a>Punkti saidi on VNet klassikaline portaalis ühenduse konfigureerimine

> [AZURE.SELECTOR]
- [Ressursihaldur - Azure'i portaal](vpn-gateway-howto-point-to-site-resource-manager-portal.md)
- [Ressursihaldur - PowerShelli](vpn-gateway-howto-point-to-site-rm-ps.md)
- [Klassikaline - Azure'i portaal](vpn-gateway-howto-point-to-site-classic-azure-portal.md)
- [Klassikaline – klassikaline portaal](vpn-gateway-point-to-site-create.md)

SharePointi saidil (P2S) konfiguratsioon võimaldab teil luua turvalist ühendust üksikute klientarvutist virtuaalse võrku. P2S ühendus on kasulik, kui soovite ühendada oma VNet mujalt, nagu kodus või konverentsi, või kui teil on vaid mõned kliendid, kes on vaja virtuaalse võrguga.

Selles artiklis tutvustatakse on VNet punkti saidi ühendusega **klassikaline juurutamise mudeli** **klassikaline portaali**loomine.

Punkti saidi ühendused, pole vaja VPN seadme või avaliku IP-aadressi töötamiseks. VPN-ühendus on loodud ühendus alates klientarvuti. Punkti saidi ühenduste kohta leiate lisateavet teemast [VPN lüüsi KKK](vpn-gateway-vpn-faq.md#point-to-site-connections) ja [kavandamisel ja kujundus](vpn-gateway-plan-design.md).


### <a name="deployment-models-and-methods-for-p2s-connections"></a>Juurutamise mudelite ja P2S ühendused meetodid

[AZURE.INCLUDE [deployment models](../../includes/vpn-gateway-deployment-models-include.md)] 

Järgmises tabelis on kaks juurutamise mudelit ja saadaval juurutamise meetodid P2S konfiguratsioone. Kui konfigureerimistoimingute kohanimi on saadaval, me lingi otse sellest tabelist.

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-table-point-to-site-include.md)] 



## <a name="basic-workflow"></a>Tavaline töövoo

![Punkti-abil – saidile-skeemi] (./media/vpn-gateway-point-to-site-create/p2sclassic.png "punkti – saidile")
 
Järgmised toimingud sõelub turvaline punkti saidi virtuaalse võrguga ühenduse loomiseks juhiseid. 

Punkti saidi ühenduse konfiguratsiooni jaotatud nelja jaotistesse. Saate konfigureerida kõigi jaotiste järjestuse on oluline. Ärge jätke või liikumiseks.

- **Jaotis 1** Saate luua virtuaalse võrgu ja VPN-lüüsi.
- **Jaotis 2** Kasutada autentimiseks serdid luua ja üles laadida.
- **Jaotis 3** Eksportida ja installige oma kliendi.
- **Jaotis 4** Seadistage oma VPN-i klient.

## <a name="vnetvpn"></a>Jaotis 1 - virtuaalse võrgu ja VPN-lüüsi loomine

### <a name="part-1-create-a-virtual-network"></a>Osa 1: Virtuaalse võrgu loomine

1. [Azure'i klassikaline portaali](https://manage.windowsazure.com/)sisse logida. Need juhised kasutada klassikaline portaalis, mitte Azure portaali. Praegu ei saa luua ühenduse P2S Azure'i portaalis.

2. Klõpsake ekraani vasakus allnurgas nuppu **Uus**. Klõpsake navigeerimispaanil, klõpsake **Võrguteenuste**ja klõpsake **Virtuaalse võrgu**. Klõpsake nuppu **Kohandatud loomine** konfiguratsiooniviisardi alustada.

3. Klõpsake lehel **Virtuaalse võrgu üksikasjad** sisestage järgmine teave ja klõpsake parempoolses allnurgas noolenuppu järgmine.
    - **Nimi**: virtuaalse võrgu nimi. Näiteks "VNet1". See on nimi, mis kuvatakse selle VNet VMs juurutamisel viitavad.
    - **Asukoht**: asukoht on otseselt seotud, kuhu soovite oma ressursse (VM) asuvad füüsilise asukoha (piirkond). Näiteks, kui soovite selle virtuaalse võrguga asuma füüsilise Ida-USA juurutamist VMs, valige soovitud kohta. Piirkond, mis on seotud virtuaalse võrgu pärast selle loomist ei saa muuta.

4. Lehe **DNS-serverid ja virtuaalse Privaatvõrgu ühendus** sisestage järgmine teave ja seejärel klõpsake parempoolses allnurgas noolenuppu järgmine.
    - **DNS-serverid**: sisestage DNS-i serveri nimi ja IP-aadress või valige otseteemenüü käsk varem registreeritud DNS-i serveri. See säte ei loo DNS-i serveri. See võimaldab teil määrata DNS-serverid, mida soovite kasutada selle virtuaalse võrgu nimelahendus. Kui soovite Azure vaikimisi nime eraldusvõime teenust kasutada, jätke see osa vahele.
    - **Saidi konfigureerimine punkti VPN**: märkige see ruut.

5. Määrake lehel **Punkti saidi ühenduvuse** IP-aadresside vahemiku, millest VPN kliendid saavad kui ühendatud IP-aadress. On mõned reeglid aadresside vahemikud, mida saate määrata kohta. See on oluline veenduge, et teie määratud vahemik ei kattu asuv kohapealse võrgu vahemikud.

6. Sisestage järgmine teave ja seejärel noolenuppu järgmine.
 - **Aadressiruumi**: käivitamine IP ja CIDR (aadressi arv).
 - **Lisa aadressiruumi**: aadressiruumi lisada ainult siis, kui see on vajalik teie võrgu kujundus.

7. Määrake lehel **Virtuaalse võrgu aadress tühikute** aadress vahemiku, mida soovite kasutada virtuaalse võrgu jaoks. Neid dünaamiliseks IP-aadresside (DIPS) VMs ja muud rolli eksemplarid, mis selle virtuaalse võrguga juurutamist määratud.<br><br>See on eriti oluline valige vahemik, mis ei kattuks mis tahes vahemikud, mida kasutatakse kohapealse võrgu jaoks. Te peate koordineerimine võrgu administraatori poole, kes võib leida soovitud vahemiku IP-aadresside kaudu oma kohapealse võrgu aadressiruumi virtuaalse võrgu jaoks kasutamiseks on vaja.

8. Sisestage järgmine teave ja klõpsake virtuaalse võrgu loomise alustamiseks märke.
 - **Aadressiruumi**: lisada sisemise IP-aadresside vahemiku, mida soovite kasutada virtuaalse võrku, alustades ja arv. See on oluline valige vahemik, mis ei kattuks mis tahes vahemikud, mida kasutatakse kohapealse võrgu jaoks. 
 - **Lisa alamvõrgu**: täiendavad alamvõrku pole kohustuslikud, kuid soovite luua eraldi alamvõrgu vms, mis on staatiline DIPS. Või võite olla teie VMs alamvõrku, eraldi oma rolli aknad.
 - **Lisa lüüsi alamvõrgu**: lüüsi alamvõrgu on vaja punkti – saidile VPN-i. Klõpsake lüüsi alamvõrgu lisamiseks. Lüüsi alamvõrgu kasutatakse ainult virtuaalse võrgu lüüsi.

9. Kui virtuaalse võrgu on loodud, kuvatakse **loodud** loendis Azure klassikaline portaalis lehel võrgu **olek** . Kui virtuaalse võrgu on loodud, saate luua dünaamilisi marsruutimise lüüsi.

### <a name="part-2-create-a-dynamic-routing-gateway"></a>Osa 2: Dünaamilise marsruutimise lüüsi loomine

Lüüsi tüüp peab olema konfigureeritud nii dünaamiline. Staatiline marsruutimine lüüside see funktsioon ei tööta.

1. Klõpsake Azure klassikaline portaalis lehel **võrkude** virtuaalse võrk, mille olete loonud ja liikuge **armatuurlaua** leht.

2. Klõpsake **Lüüsi loomiseks**, asub **armatuurlaua** lehe allosas. Ilmub küsimus, **kas soovite luua lüüsi virtuaalse võrgu "VNet1"**. Klõpsake nuppu **Jah** lüüsi loomise alustamiseks. Võib kuluda umbes 15 minutit lüüsi loomiseks.

## <a name="generate"></a>Jaotis 2 - luua ja üles laadida serdid

Serte kasutatakse autentida VPN kliente VPN punkti saidi jaoks. Saate kasutada juurkausta serdi loodud lahenduse ettevõtte sert või kasutage iseallkirjastatud serdi. Saate üles laadida kuni 20 juursertide Azure. Kui CER-fail on üles laaditud, saate Azure'i sisalduva teabe abil autentida klientidele, mis on installitud kliendi serti. Kliendi sert peab tekkinud sama sert, mida tähistab CER-fail.

Selles jaotises saate tehke järgmist.

- Hankige sert juurkausta CER-faili. See võib olla iseallkirjastatud serdi või abil saate oma ettevõtte sert süsteemi.
- Azure'i CER-faili üleslaadimine.
- Saate luua kliendi serdid.

### <a name="root"></a>Osa 1: Saamiseks serdi juurkausta CER-faili

Kui kasutate mõnda ettevõtte sert süsteemi, saada CER-faili jaoks juurkausta sert, mida soovite kasutada. [Osa 3](#createclientcert), saate luua kliendi sertide juurkausta sert.

Kui te ei kasuta lahenduse ettevõtte sert, peate juurkausta iseallkirjastatud serdi loomiseks. Windows 10 juhised, võib viidata [töötamine iseallkirjastatud juursertide punkti saidi jaoks](vpn-gateway-certificates-point-to-site.md). See artikkel juhendab teid makecert abil luua iseallkirjastatud serti ja seejärel eksportige CER-fail.

### <a name="upload"></a>Osa 2: Üles Azure klassikaline portaali juurkausta serdi CER-fail

Azure'i usaldusväärsete serdi lisamine. Azure'i Base64-kodeeringuga X.509 (CER) faili lisamisel te ei räägi Azure'i juurkausta sert, mida tähistab faili usaldada.

1. Azure'i klassikaline portaali, virtuaalse võrgu **serdid** lehel nuppu **juurkausta serdi üleslaadimine**.

2. Lehel **Üles sert** CER juur sertifikaadi sirvida, ja klõpsake märke.

### <a name="createclientcert"></a>Osa 3: Luua kliendi sert

Järgmiseks luua kliendi serdid. Kas saate luua kordumatu sert iga kliendi, mis loob ühenduse või kasutada mitme kliendi serti. Võimalus tühistada ühe serdi vajaduse korral on see eelis, nendele kordumatu kliendi serdid. Muul juhul, kui kõik kasutab sama kliendi serti ja leiate, et peate ühe kliendi serdi tühistada, pead luua ja installige kõik autentida serdi kasutavate klientide uus.

- Kui kasutate lahenduse ettevõtte sert, luua kliendi serti levinud nimi väärtus vorming 'name@yourdomain.com', asemel NetBIOS 'Domeen\kasutajanimi' vormindamine. 

- Kui kasutate iseallkirjastatud serdi, vt [töötamine iseallkirjastatud juursertide punkti saidi jaoks](vpn-gateway-certificates-point-to-site.md) luua kliendi serti.

## <a name="installclientcert"></a>Jaotis 3 – eksportimine ja installige kliendi sert

Installige kliendi sertifikaadi igas arvutis, mida soovite virtuaalse võrguga. Kliendi sert on vaja autentimist. Kliendi serdi installimiseks automatiseerida või käsitsi installida. Järgmised toimingud teile eksportimine ja käsitsi kliendi serdi installimiseks kaudu.

1. Kliendi sertifikaadi eksportida saate *tekst certmgr.msc*. Paremklõpsake kliendi sert, mida soovite eksportida, klõpsake käsku **Kõik tööülesanded**ja seejärel klõpsake nuppu **ekspordi**.
2. Eksportida privaatvõti kliendi sert. See on *pfx-* fail. Veenduge, et salvestada või parool (võti), mida saate seada selle serdi meeles pidada.
3. Kopeerige klientarvuti *pfx* -fail. Klientarvuti, topeltklõpsake seda installida *pfx* -faili. Sisestage parool, kui see on nõutav. Muutke installimise kohta.

## <a name="vpnclientconfig"></a>Jaotis 4 - VPN-klient konfigureerimine

Virtuaalse võrguga ühenduse loomiseks, peate samuti konfigureerimine VPN klient. Kliendi nõuab nii kliendi serti ja proper VPN kliendi konfiguratsiooni, et ühendada. Konfigureerimiseks VPN klient, tehke järgmist, et.

### <a name="part-1-create-the-vpn-client-configuration-package"></a>Osa 1: VPN kliendi konfiguratsiooni paketi loomine

1. Liikuge Azure klassikaline portaalis **armatuurlaualehe virtuaalse võrgu jaoks** kiire ülevaade menüü parempoolses nurgas. Kliendi operatsioonisüsteemid, mis on toetatud loendi leiate teemast [SharePointi saidil ühendused](vpn-gateway-vpn-faq.md#point-to-site-connections) VPN lüüsi KKK osas. VPN kliendi pakett sisaldab konfiguratsiooniteavet VPN klienttarkvara Windowsi sisse ehitatud konfigureerimiseks. Paketi ei saa installida täiendavat tarkvara. Sätted on virtuaalse võrgu, mida soovite ühendada.<br><br>Valige Laadi alla paketi, mis vastab kliendi operatsioonisüsteem, kuhu on installitud.
 - 32-bitine klienti, valige **32-bitise kliendi VPN laadige**.
 - 64-bitine klienti, valige **64-bitise kliendi VPN laadige**.

2. Kulub paar minutit oma kliendi paketi loomiseks. Kui pakett on lõpetatud, saate faili alla laadida. Kohalikus arvutis saab turvaline salvestada *.exe* faili, mille saate alla laadida.

3. Kui olete luua ja Azure klassikaline portaali VPN kliendi alla laadida, saate installida kliendi paketi, millest soovite virtuaalse võrguga klientarvuti. Kui plaanite VPN-kliendi paketi installimiseks mitme klientarvutite, veenduge, et nad iga ka on installitud kliendi serti.

### <a name="part-2-install-the-vpn-configuration-package-on-the-client"></a>Osa 2: Installi VPN kliendi konfiguratsiooni paketi

1. Kopeerige konfiguratsioonifail Kohalik arvuti, mida soovite virtuaalse võrguga ja topeltklõpsake faili .exe. 

2. Kui pakett on installitud, saate alustada VPN-ühendus. Microsoft pole allkirjastatud paketi konfigureerimine. Võib kasutada oma asutuse allkirjastamiseks teenuse allkirjastatav või logige ise [SignTool]( http://go.microsoft.com/fwlink/p/?LinkId=699327)abil. See on OK, et kasutada ilma allkirjastamine. Kui paketi pole sisse logitud, kuvatakse hoiatus, kui installite paketi.

3. Klientarvuti, liikuge **Võrgu sätteid** ja klõpsake **VPN**. Kuvatakse loendis ühendus. See näitab virtuaalse võrgu nimi, et see loob ühenduse ja näeb välja umbes järgmine: 

    ![VPN klient] (./media/vpn-gateway-point-to-site-create/vpn.png "VPN klient")


### <a name="part-3-connect-to-azure"></a>Osa 3: Azure'i ühendamine

1. Ühenduse loomiseks oma VNet klientarvuti, liikuge VPN-ühendused ja otsige üles loodud VPN-ühendus. See on nime virtuaalse võrgu sama nimi. Klõpsake nuppu **Loo ühendus**. Hüpikaknas kuvatakse võidakse kuvada, mis viitab serdi abil. Kui see juhtub, klõpsake nuppu **Jätka** kasutada administraatoriõigusi. 

2. **Ühenduse** oleku lehel käsku **Ühenda** alustamiseks ühendus. Kui kuvatakse **Serdi valimine** Kuva, veenduge, et kliendi sert, kus on üks, mida soovite kasutada ühenduse. Kui see pole õige serdi valimiseks kasutage ripploendi noolt ja seejärel klõpsake nuppu **OK**.

    ![VPN klient 2] (./media/vpn-gateway-point-to-site-create/clientconnect.png "Kliendi VPN-ühendus")

3. Nüüd peaks teie ühendust luua.

    ![VPN klient 3] (./media/vpn-gateway-point-to-site-create/connected.png "VPN-ühendus kliendi 2")

### <a name="part-4-verify-the-vpn-connection"></a>Osa 4: Veenduge, et VPN-ühendus

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

Kui soovite virtuaalne võrkude kohta lisateavet, lugege teemat [Virtuaalse võrgu dokumentatsiooni](https://azure.microsoft.com/documentation/services/virtual-network/) leht.
