<properties
    pageTitle="Arvutites ja seadmetes ühenduse OMS OMS lüüsi abil | Microsoft Azure'i"
    description="Ühendage OMS hallatavate seadmete ja arvutite Toiminguhalduri jälgida OMS lüüsi OMS teenuse andmeid saata, kui neil on Interneti-ühendus."
    services="log-analytics"
    documentationCenter=""
    authors="bandersmsft"
    manager="jwhit"
    editor=""/>
<tags
    ms.service="log-analytics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/26/2016"
    ms.author="banders"/>

# <a name="connect-computers-and-devices-to-oms-using-the-oms-gateway"></a>Ühenduse OMS OMS lüüsi kasutamine arvutites ja seadmetes

Selles dokumendis kirjeldatakse, kuidas OMS hallatavate seadmete ja arvutite System Center toimingute Manager SCOM jälgida saata andmed OMS teenuse kui neil on Interneti-ühendus. OMS lüüsi saate andmete kogumise ja saatke see OMS teenuse enda nimel.

Lüüs on HTTP edasi puhverserveri, mis toetab HTTP läbindusmehhanismid käsuga HTTP ühenduse. Lüüsi võite hakkama kuni 2000 OMS samaaegselt ühendatud seadmete kui käitamist 4-core CPU, 16-GB serveris, kus töötab Windows.

Näiteks oma ettevõtte või suur ettevõte võib olla võrguühenduse serverite aga ei pruugi olla Interneti-ühendus. Teise näite korral võib olla võimalus jälgimine nende otse Müügikoht (POS) seadmete mitme punkti. Ja teise näite korral Toiminguhalduri saate kasutada OMS lüüsi puhverserverit. Nendes näidetes OMS lüüsi saate edastada andmed agentide, need serverid või POS seadmed OMS arvutisse installitud.

Selle asemel, et iga üksiku agent saatmise andmed otse OMS ja nõuavad otsene Interneti-ühendus, selle asemel saadetakse agent kõik andmed kaudu ühes arvutis, kus on Interneti-ühendus. Arvutis on, kus saate installida ja kasutada lüüsi. Selle stsenaariumi korral saate installida agentide mis tahes arvutites, kuhu soovite koguda andmeid. Lüüsi seejärel edastab andmed agentide OMS otse – lüüsi analüüsida andmed, mis on üle.

Lüüsi OMS jälgida ja analüüsida jõudluse või sündmuse andmeid server, kuhu see on installitud, peate installima OMS agent arvutis, kus lüüsi on ka installitud.

Lüüsi peab olema Interneti kaudu üles laadida andmete OMS. Iga agent peab olema võrguühenduse selle lüüsi, et agentide saate automaatselt teisaldada andmed ja sealt lüüsi. Parimate tulemuste saamiseks ei saa installida lüüsi arvutisse, kus on ka domeenikontrolleri.

Skeem, mis näitab andmevoo otsese kaudu OMS on.

![otsest agent skeem](./media/log-analytics-oms-gateway/direct-agent-diagram.png)

Skeem, mis näitab andmevoo Operations Manager OMS on.

![Toimingute halduri skeem](./media/log-analytics-oms-gateway/scom-mgt-server.png)

## <a name="install-the-oms-gateway"></a>OMS lüüsi installimine

Selle lüüsi installimise asendab varasemate versioonide lüüsi on installitud (Log Analytics ekspediitor).

Eeltingimused: .net Framework 4.5, Windows Server 2012 R2 SP1 ja uuemad

1. [Microsofti allalaadimiskeskusest](http://download.microsoft.com/download/2/5/C/25CF992A-0347-4765-BD7D-D45D5B27F92C/OMS%20Gateway.msi)alla laadida OMS lüüsi uusim versioon.
2. Installimise alustamiseks topeltklõpsake **OMS Gateway.msi**.
3. Lehel Tere tulemast, **edasi**.  
    ![Lüüsi häälestuse viisard](./media/log-analytics-oms-gateway/gateway-wizard01.png)
4. Valige lehel litsentsileping **nõustun litsentsi lepingu tingimustega** nõus EULA ja seejärel **edasi**.
5. Valige lehel pordi ja puhverserveri aadressi:
    1. Tippige kasutatava lüüsi TCP pordi number. Häälestamise avatakse see pordinumber Windowsi tulemüüri kaudu. Vaikeväärtus on 8080.
    Sobiv pordi number on 1 – 65535. Kui siis sisendit ei kuulu sellesse vahemikku, kuvatakse tõrketeade.
    2. Soovi korral, kui server, kuhu on installitud lüüsi peab puhverserveri kasutamine, Tippige meiliaadress kui on vaja ühenduse lüüs. Näiteks `http://myorgname.corp.contoso.com:80` kui tühi, lüüsi proovib otse Interneti-ühenduse. Muul juhul lüüsi loob ühenduse puhverserverist. Kui teie puhverserveri server nõuab autentimist, sisestage oma kasutajanimi ja parool.
        ![Lüüsi viisardi puhverserveri konfigureerimine](./media/log-analytics-oms-gateway/gateway-wizard02.png)  
    3. Klõpsake nuppu **Järgmine**
6. Kui teil pole Microsofti Updates lubatud, kuvatakse Microsoft Update leht, kus saate valida Microsoft Updates lubamiseks. Tehke valik ja klõpsake siis nuppu **edasi**. Muul juhul jätkake järgmise juhisega.
7. Klõpsake lehel kaust, kuhu jätke vaikimisi kausta **%ProgramFiles%\OMS lüüsi** või tippige asukoht, kuhu soovite installida lüüsi ja seejärel klõpsake nuppu **edasi**.
8. Lehe installimiseks valmis, klõpsake nuppu **Installi**. Kasutajakonto kontroll, võidakse kuvada taotlevale õigust installida. Kui jah, klõpsake nuppu **Jah**.
9. Pärast häälestamine on lõpule jõudnud, klõpsake nuppu **valmis**. Saate kontrollida, et teenus töötab, avades services.msc lisandmoodul ja veenduge, et **Lüüsi OMS** kuvatakse loendis teenused.  
    ![Teenuste – OMS lüüsi](./media/log-analytics-oms-gateway/gateway-service.png)

## <a name="install-an-agent-on-devices"></a>Installige agent seadmetes

Vajaduse korral, vt [ühenduse loomine Windows arvutite Log Analytics](log-analytics-windows-agents.md) teavet selle kohta, kuidas installida otse ühendatud agentide. Selles artiklis kirjeldatakse, kuidas saate installida agent häälestamise viisardi abil või käsurea kaudu.

## <a name="configure-oms-agents"></a>OMS agentide konfigureerimine

Vaadake lisateavet konfigureerimise agent kasutada puhverserverit, mis on sel juhul [konfigureerimine puhverserveri ja tulemüüri sätted Microsoft Agenti jälgimine](log-analytics-proxy-firewall.md) on lüüsi.

Toimingute halduri agentide saata andmed, nagu näiteks Toiminguhalduri teatised, konfiguratsiooni hindamise, eksemplari ruumi ja võimsus andmete Management serveri kaudu. Muude suure mahu andmeid, nt IIS-i logid, jõudlus ja turvalisus on saadetud otse OMS lüüsi. Iga kanali kaudu saadetud täieliku loendi leiate [lisamine Log Analytics lahenduste lahendusegaleriist](log-analytics-add-solutions.md) .

>[AZURE.NOTE]
Kui kavatsete kasutada lüüsi võrgu koormusetasakaalustuseks, lugege teemat [soovi konfigureerimine võrgu koormus tasakaalustamiseks](#optionally-configure-network-load-balancing).

## <a name="configure-a-scom-proxy-server"></a>SCOM puhverserveri konfigureerimine

Konfigureerige Toiminguhalduri lisada lüüsi tegutseda puhverserverit. Puhverserveri konfigureerimine värskendamisel automaatselt rakendatakse kõigi agentide, aruandlus Toiminguhalduri puhverserveri konfigureerimine.

Lüüsi toetamiseks Toiminguhalduri kasutamiseks peate olema:

- Microsoft Agenti jälgimine (agenti versiooni – **8.0.10900.0** ja uuemad versioonid) serveris lüüsi installinud ja konfigureerinud OMS tööruumide, kellega soovite suhelda.
- Lüüsi peab olema Interneti-ühendus või puhverserverit, mis teeb ühendada.

### <a name="to-configure-scom-for-the-gateway"></a>Lüüsi SCOM konfigureerimine

1. Avatud Toiminguhalduri konsooli ja klõpsake jaotises **Toimingud Management Suite** **ühendus** ja klõpsake nuppu **Puhverserveri konfigureerimine**:  
    ![Toiminguhalduri – konfigureerimine puhverserver](./media/log-analytics-oms-gateway/scom01.png)
2. Valige **Kasuta puhverserverit Operations Management Suite juurdepääsu** ja seejärel tippige OMS lüüsi serveri IP-aadress. Veenduge, et alustada selle `http://` eesliide:  
    ![Toiminguhalduri – puhverserveri aadress](./media/log-analytics-oms-gateway/scom02.png)
3. Klõpsake nuppu **valmis**. Teie Toiminguhalduri server on ühendatud OMS tööruumis.

## <a name="configure-network-load-balancing"></a>Võrgu koormusetasakaalustuseks konfigureerimine

Lüüsi jaoks kõrge kättesaadavus võrgu koormus tasakaalustamiseks klaster loomisega abil saate konfigureerida. Klaster haldab teie agentide liikluse ümbersuunamise nõutud ühendused Microsoft jälgimise kaudu oma sõlmed üle teel. Kui üks lüüsi server läheb alla, saab liikluse ümber sõlmi.

1. Avage võrgu laadi tasakaalustamiseks halduri ja looge klaster.
2. Paremklõpsake klaster enne lisamist lüüside ja valige **kobar atribuutide.** Konfigureerige oma IP-aadress on kobar.  
    ![Võrgu laadi tasakaalustamiseks halduri – kobar IP-aadressid](./media/log-analytics-oms-gateway/nlb01.png)
3. Ühenduse ettevõtte OMS lüüsi serveris Microsoft Agenti jälgimine installitud, paremklõpsake soovitud klaster IP-aadress ja klõpsake **Kobar Host lisada**.  
    ![Võrgu laadimine tasakaalustavad Manager-hosti Arvutikobaras lisamine](./media/log-analytics-oms-gateway/nlb02.png)
4. Sisestage soovitud lüüsi server, mida soovite ühendada IP-aadress.  
    ![Võrgu laadi tasakaalustamiseks Manager-hosti Arvutikobaras lisamine: ühenduse loomine](./media/log-analytics-oms-gateway/nlb03.png)
5. Arvutites, kus on Interneti-ühendus, kasutage kindlasti klaster IP-aadress kui konfigureerite **Microsoft Agenti atribuudid**:  
    ![Microsoft jälgimise Agent atribuudid – puhverserveri sätted](./media/log-analytics-oms-gateway/nlb04.png)

## <a name="configure-for-automation-hybrid-workers"></a>Automaatika hübriid töötajate konfigureerimine

Kui teil on teie keskkonnas automatiseerimise hübriid töötajate, sisestage järgmist lüüsi toetamiseks konfigureerimiseks käsitsi, ajutised lahendused.

Järgmistes juhistes, peate teadma Azure piirkond, kus asub konto automatiseerimine. Otsige üles asukoht:

1. [Azure'i portaali](https://portal.azure.com/)sisse logida.
2. Valige Azure automatiseerimine teenuse.
3. Valige sobiv konto Azure automatiseerimine.
4. Saate vaadata oma piirkond jaotises **asukoht**.  
    ![Azure'i portaal – automatiseerimise konto asukoht](./media/log-analytics-oms-gateway/location.png)

Järgmistes tabelites abil saate tuvastada iga asukoha URL:

**Töö käitusaja andmete teenuse URL-id**

| **asukoht** | **URL-I** |
| --- | --- |
| Põhja Kesk-USA | ncus-jobruntimedata-tootekataloogi-su1.azure-automation.net |
| Lääne Euroopa | Me-jobruntimedata-tootekataloogi-su1.azure-automation.net |
| Lõuna-, Kesk-USA | scus-jobruntimedata-tootekataloogi-su1.azure-automation.net |
| Ida-USA | ELi jobruntimedata-tootekataloogi-su1.azure-automation.net |
| Keskse Kanada | koopia – jobruntimedata-tootekataloogi-su1.azure-automation.net |
| Põhja-Euroopa | ne-jobruntimedata-tootekataloogi-su1.azure-automation.net |
| Kagu-Aasia | Sea jobruntimedata-tootekataloogi-su1.azure-automation.net |
| Keskse India | CID-jobruntimedata-tootekataloogi-su1.azure-automation.net |
| Jaapan | jpe jobruntimedata-tootekataloogi-su1.azure-automation.net |
| Austraalia | ase-jobruntimedata-tootekataloogi-su1.azure-automation.net |

**Agent teenuse URL-id**

| **asukoht** | **URL-I** |
| --- | --- |
| Põhja Kesk-USA | ncus-agentservice-tootekataloogi-1.azure-automation.net |
| Lääne Euroopa | Me-agentservice-tootekataloogi-1.azure-automation.net |
| Lõuna-, Kesk-USA | scus-agentservice-tootekataloogi-1.azure-automation.net |
| Ida-USA | eus2-agentservice-tootekataloogi-1.azure-automation.net |
| Keskse Kanada | koopia – agentservice-tootekataloogi-1.azure-automation.net |
| Põhja-Euroopa | ne-agentservice-tootekataloogi-1.azure-automation.net |
| Kagu-Aasia | Sea agentservice-tootekataloogi-1.azure-automation.net |
| Keskse India | CID-agentservice-tootekataloogi-1.azure-automation.net |
| Jaapan | jpe agentservice-tootekataloogi-1.azure-automation.net |
| Austraalia | ase-agentservice-tootekataloogi-1.azure-automation.net |

Kui teie arvutis on registreeritud hübriid töötaja automaatselt lappimine abil Update halduse lahenduse, tehke järgmist:

1. Töö käitusaja andmete teenuse URL-ide OMS lüüsi lubatud Host loendisse lisada. Näiteks: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
2. Taaskäivitage OMS lüüsi teenuse abil järgmine PowerShelli cmdlet-käsk:`Restart-Service OMSGatewayService`

Kui teie arvuti on sisse-peatatud abil Azure automatiseerimine hübriid töötaja registreerimise cmdleti abil, tehke järgmist:

1. Host lubatud loendisse OMS lüüsi agent teenuse registreerimise URL-i lisamine. Näiteks:`Add-OMSGatewayAllowedHost ncus-agentservice-prod-1.azure-automation.net`
2. Töö käitusaja andmete teenuse URL-ide OMS lüüsi lubatud Host loendisse lisada. Näiteks: `Add-OMSGatewayAllowedHost we-jobruntimedata-prod-su1.azure-automation.net`
3. Taaskäivitage lüüsi OMS teenus.
    `Restart-Service OMSGatewayService`

## <a name="useful-powershell-cmdlets"></a>Kasulik PowerShelli cmdlet-käsud

Cmdlet-käskude abil saate Sooritage toimingud, mida on vaja värskendada OMS lüüsi konfigureerimise sätted. Enne nende kasutamist kindlasti järgmist.

1. Installige OMS lüüsi (MSI).
2. Avage PowerShelli aken.
3. Mooduli importimiseks tippige see käsk:`Import-Module OMSGateway`
4. Kui eelmises etapis ilmnenud tõrkeid, mooduli on edukalt imporditud ja saate kasutada cmdlet-käsud. Tüüp`Get-Module OMSGateway`
5. Kui muudate cmdlet-käskude abil, veenduge, et lüüsi teenuse taaskäivitamisel.

Kui kuvatakse tõrketeade sammus 3, mooduli ei impordita. Tõrge võib ilmneda PowerShelli ei leia mooduli. Te ei leia seda soovitud lüüsi Installitee: C:\Program File\Microsoft OMS Gateway\PowerShell.

| **Cmdlet-käsk** | **Parameetrid** | **Kirjeldus** | **Näited** |
| --- | --- | --- | --- |
| `Set-OMSGatewayConfig` | Võti (nõutav) <br> Väärtus | Teenuse konfiguratsioon | `Set-OMSGatewayConfig -Name ListenPort -Value 8080` |
| `Get-OMSGatewayConfig` | Klahv | Saab teenuse konfigureerimine | `Get-OMSGatewayConfig` <br> <br> `Get-OMSGatewayConfig -Name ListenPort` |
| `Set-OMSGatewayRelayProxy` | Meiliaadress <br> Kasutajanimi <br> Parooli | Määrab relay (varustava) puhverserveri aadressi (ja mandaadi) | 1. vasta puhverserveri ja mandaadi määramine`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080 -Username user1 -Password 123` <br> <br> 2. määrata vasta puhverserveri, mis ei pea autentimist.`Set-OMSGatewayRelayProxy -Address http://www.myproxy.com:8080` <br> <br> 3. tühjendage ruut vasta puhverserveri säte, seega ei pea vasta puhverserveri:`Set-OMSGatewayRelayProxy -Address ""` |
| `Get-OMSGatewayRelayProxy` |   | Saab relay (varustava) puhverserveri aadressi | `Get-OMSGatewayRelayProxy` |
| `Add-OMSGatewayAllowedHost` | Host (nõutav) | Lisab host on lubatud failide loendisse | `Add-OMSGatewayAllowedHost -Host www.test.com` |
| `Remove-OMSGatewayAllowedHos`t | Host (nõutav) | Eemaldab selle hosti lubatud | `Remove-OMSGatewayAllowedHost -Host www.test.com` |
| `Get-OMSGatewayAllowedHost` |   | Saab praegu lubatud host (ainult kohalikult konfigureeritud lubatud host kaasata automaatselt allalaaditud lubatud hosts) | `Get-OMSGatewayAllowedHost` |
| `Add-OMSGatewayAllowedClientCertificate` | Teema (nõutav) | Lisab lubatud vastavalt kliendi sert | `Add-OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Remove-OMSGatewayAllowedClientCertificate` | Teema (nõutav) | Eemaldab kliendi serdi teema on lubatud failide loendisse lisamine | `Remove- OMSGatewayAllowedClientCertificate -Subject mycert` |
| `Get-OMSGatewayAllowedClientCertificat`e |   | Saab praegu lubatud kliendi serdi teemade (ainult kohalikult konfigureeritud lubatud teemad, ei sisalda automaatselt allalaaditud lubatud Teemad) | `Get-OMSGatewayAllowedClientCertificate` |

## <a name="troubleshoot"></a>Tõrkeotsing

Soovitame installida OMS agent arvutites, kus on installitud lüüsi. Seejärel saate soovitud agendi sündmused, mis on sisse logitud lüüsi koguda.

![Sündmusevaatur – OMS lüüsi Log](./media/log-analytics-oms-gateway/event-viewer.png)

**OMS lüüsi sündmuse ID-d ja kirjeldused**

Järgmises tabelis on sündmuse ID-d ja sündmuste logi OMS lüüsi kirjeldused.

| **ID** | **Kirjeldus** |
| --- | --- |
| 400 | Mis tahes rakenduse viga, mida ei saa teatud ID |
| 401 | Vale konfiguratsioon. Näide: listenPort = "tekst" asemel täisarv |
| 402 | Erandi TLS käepigistus sõnumite sõelumine |
| 403 | Tõrge. Näide: target serveriga ei saa ühendust luua |
| 100 | Üldist teavet |
| 101 | Teenus on hakanud |
| 102 | Teenus on peatatud. |
| 103 | HTTP ühenduse käsu käivitava saadud kliendi |
| 104 | Pole HTTP ühenduse käsu käivitava |
| 105 | Sihtkoha server ei ole lubatud failide loendisse lisamine või selle sihtport pole turvaline port (443) <br> <br> Veenduge, et MMA agent lüüsi serverisse ja suhtlemine lüüsi agentide on ühendatud sama Log Analytics tööruumi.|
| 105 | TÕRGE TcpConnection – sobimatu sert: CN = lüüsi <br><br> Veenduge, et: <br> <br> & #149; Kasutate lüüsi versiooninumber 1.0.395.0 või suurem. <br> & #149; MMA agent lüüsi serverisse ja suhtlemine lüüsi agentide on ühendatud sama Log Analytics tööruumi. |
| 106 | Mis tahes põhjusel, et TLS-i seansi on kahtlane ja hüljatud |
| 107 | TLS-i seansi on kinnitatud |

**Jõudluse hinnale kogumiseks**

Järgmine tabel näitab jõudluse hinnale saadaval OMS lüüsi. Saate lisada hinnale, kasutades jõudluse jälgimine.

| **Nimi** | **Kirjeldus** |
| --- | --- |
| OMS lüüsi/aktiivse kliendi ühendus | Aktiivse kliendi võrguühenduste (TCP) arv |
| OMS lüüsi/tõrgete arv | Vigade arv |
| OMS lüüsi/ühendatud kliendi | Ühendatud klientide arv |
| OMS lüüsi/tagasilükkamise loendamine | Mis tahes TLS-i valideerimine tõrke tõttu tagasilükkamiste arv |

![OMS lüüsi jõudluse hinnale](./media/log-analytics-oms-gateway/counters.png)


## <a name="get-assistance"></a>Abi hankimine

Kui te olete sisse loginud sisse Azure portaali, saate luua abi taotluse OMS lüüsi või teiste Azure'i teenus või teenuse funktsiooni.
Kui soovite abi, klõpsake paremas ülanurgas portaali küsimärki ja seejärel klõpsake nuppu **Uus tugiteenuste taotlus**. Seejärel tehke uus tugiteenuste taotlusvormi.

![Uue tugiteenuse taotluse](./media/log-analytics-oms-gateway/support.png)

Saate ka jätta tagasisidet OMS või Log Analytics [Microsoft Azure'i tagasiside Foorum](https://feedback.azure.com/forums/267889).

## <a name="next-steps"></a>Järgmised sammud

- [Lisa andmeallikate](log-analytics-data-sources.md) oma OMS tööruumis allikad ühendatud andmete kogumine ja salvestada selle OMS hoidla.
