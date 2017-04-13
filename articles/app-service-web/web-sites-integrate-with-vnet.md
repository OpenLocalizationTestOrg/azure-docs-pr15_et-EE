<properties 
    pageTitle="Sujuv rakenduse Azure virtuaalse võrguga" 
    description="Näitab, kuidas võrguga rakenduse Azure'i rakendust Service uude või olemasolevasse Azure virtuaalse" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor="cephalin"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/11/2016" 
    ms.author="ccompy"/>

# <a name="integrate-your-app-with-an-azure-virtual-network"></a>Rakenduse integreerimine Azure virtuaalse võrgustik #

Selles dokumendis kirjeldatakse Azure'i rakendust Service virtuaalse võrgu integratsiooni funktsiooni ja näidatakse, kuidas häälestada teenuses [Azure rakenduse](http://go.microsoft.com/fwlink/?LinkId=529714)rakendustega.  Kui olete varem selliseid Azure virtuaalne võrkude (VNETs), see on võimalus, mis võimaldab teil oma Azure ressursse paljude paigutamiseks-internet routeable võrk, mis on juurdepääsu reguleerimine.  Nende võrkude seejärel sees eeldusel võrguga erinevate VPN-tehnoloogia abil saab ühendada.  Lisateavet Azure virtuaalse võrgu alustage siin teabe leiate: [Azure'i virtuaalse võrgu ülevaade][VNETOverview].  

Azure'i rakenduse teenus on kahel viisil.  

1. Mitme rentniku süsteemid, mis toetavad kõiki paketid
1. Rakenduse teenuse keskkonnas (ASE) premium funktsioon, mis kasutab teie VNET sisse.  

Selle dokumendi kontrollimisel VNET integratsioon ja rakenduse teenuse keskkonnas.  Kui soovite lisateavet funktsiooni ASE, siis alustage siin teave: [Sissejuhatus rakenduse teenuse keskkonna][ASEintro].

VNET integreerimine annab teie web appi juurdepääsu ressurssidele virtuaalse võrgu, kuid ei võimalda privaatne juurdepääsu oma web appi, virtuaalse võrgu kaudu.  Privaatne saidile juurdepääsu on saadaval ainult mõne ASE konfigureeritud koos mõne sisemise laadi koormusetasakaalustusteenuse (ILB) abil.  Alustage mõne ILB ASE kasutamise kohta lisateabe saamiseks artiklit: [loomine ja kasutamine on ILB ASE][ILBASE]. 

Levinud stsenaariumi, kus tuleks kasutada VNET integreerimine on andmebaasi või virtuaalse masina Azure virtuaalse võrgu töötavate veebiteenuste web appi juurdepääsu lubamine.  VNET integreerimise ei pea esitamist avaliku lõpp-punkti jaoks rakenduste oma VM, kuid see võib selle asemel kasutada era-internet marsruuditavaid aadressid.  

Funktsiooni VNET integreerimine:

- nõuab Standard- või Premium hinnad kavandamine 
- töötavad koos Classic(V1) või ressursside Manager(V2) VNET 
- toetab TCP- ja UDP
- töötab Web, Mobile ja API rakendustega
- lubab rakenduse ühenduse ainult 1 VNET korraga
- võimaldab kuni 5 VNETs koos integreeritud rakenduse teenuse kavandamine 
- võimaldab sama VNET kasutavad mitu rakendused rakendus teenuse kavandamine
- toetab 99,9% SLA tõttu VNET lüüsi sõltuvus

On mõned asjad, mis ei toeta VNET integreerimine, sh:

- Draivi ühendamine
- AD integreerimine 
- NetBios
- Privaatne saidi juurdepääsuõiguste tühistamine

### <a name="getting-started"></a>Alustamine ###
Siin on mõned asjad, mida enne oma veebirakenduse virtuaalse võrguga ühenduse loomisel pidage meeles:

- VNET integreerimine töötab ainult rakenduste **Standard** - või **Premium** hinnad leping.  Kui lubate funktsiooni ja siis skaala rakenduse teenusleping pole toetatud hinnakirjad plaani kaotavad rakenduste VNETs, kes kasutavad nende ühendused.  
- Kui target virtuaalse võrgu juba olemas, peab olema punkt saidi VPN lubatud dünaamilise marsruutimise Gateway enne selle rakenduse saab ühendada.  Kui teie lüüsi on konfigureeritud staatiline marsruutimine, ei saa lubada punkti saidi virtuaalse privaatvõrgu (VPN).
- Funktsiooni VNET peab olema sama nimega oma rakenduse teenuse Plan(ASP) tellimus.  
- Rakendused, mis on VNET integreerida kasutab seda VNET määratud DNS-i.
- Vaikimisi integreerimine rakenduste kuvatakse ainult marsruutida liikluse oma VNET põhjal marsruudib, mis on määratletud teie VNET sisse.  


## <a name="enabling-vnet-integration"></a>VNET integreerimise lubamine ##

Selle dokumendi keskendub peamiselt VNET integreerimine Azure portaali kasutamise.  PowerShelli kaudu oma rakendusega VNET integreerimise lubamiseks järgige juhiseid siin: [rakenduse PowerShelli abil virtuaalse võrguga ühenduse loomine][IntPowershell].

Teil on võimalus võrguga ühenduse loomiseks oma rakenduse uus või olemasolev virtuaalse.  Kui loote uue võrgu oma integreerimine siis lisaks lihtsalt luua selle VNET osana, dünaamiline marsruutimine lüüsi eelnevalt konfigureeritud, saate ja valige käsk saidi VPN on lubatud.  

>[AZURE.NOTE] Uue virtuaalse võrgu integreerimise konfigureerimine võib kuluda mitu minutit.  

Lubada VNET integreerimine Ava rakenduse sätted ja seejärel valige võrgunduse.  Kasutajaliides, mis avab pakub kolme võrgu valikuid.  Sellest juhendist ainult läheb üheks VNET integreerimine küll hübriid ühendused ja rakenduse teenuse keskkonnas arutatakse hiljem selles dokumendis.  

Kui teie rakendus pole on õige hinnad leping UI abivalmilt lubada mastaapimiseks oma lepingu suurema hinnakirjad lepingu oma valik.


![][1]
 
###<a name="enabling-vnet-integration-with-a-pre-existing-vnet"></a>Olemasoleva VNET VNET integreerimine lubamine###
VNET integreerimine UI võimaldab valida loendist oma VNETs.  Klassikaline VNETs näitab, et need on näiteks sõna "Classic" sulgudes VNET nime kõrval.  Loend on sorditud nii, et ressursihaldur VNETs on eespool.  Pilt kuvatakse allpool näete, et ainult üks VNET saate valitud.  On mitu põhjust, et on VNET tuhm sh.

- funktsiooni VNET on teine tellimus, mis on teie konto on juurdepääs
- funktsiooni VNET ei ole saidile lubatud punkti
- funktsiooni VNET ei saa dünaamilise marsruutimise lüüsi


![][2]

Lubamiseks integreerimine klõpsake lihtsalt VNET, mida soovite integreerida.  Kui olete valinud soovitud VNET, uuesti oma rakenduse muudatuste jõustumiseks automaatselt.  

##### <a name="enable-point-to-site-in-a-classic-vnet"></a>Valige käsk saidi klassikaline VNET lubamine #####
Kui teie VNET ei saa lüüsi ega punkti on saidile siis tuleb luua esimene.  Toiming klassikaline VNET, minge [Azure portaali] [ AzurePortal] ja tuua virtuaalse Networks(classic) loetelu.  Klõpsake siin võrgu kaudu, mida soovite integreerida ja klõpsake jaotises nimega VPN-ühendused Essentialsi suur kasti.  Siit saate luua oma osutage saidi VPN ja isegi seda lüüsi loomine.  Pärast läbida punkti saidile lüüsi loomine kasutussätteid, mis on 30 minutit enne, kui see on valmis.  

![][8]

##### <a name="enabling-point-to-site-in-a-resource-manager-vnet"></a>Valige käsk saidi ressursihaldur VNET lubamine #####

Ressursihaldur VNET ja lüüsi konfigureerimine ja osutage saidi peate kasutama PowerShelli nagu dokumenteerida siin [punkti saidi virtuaalse võrgu PowerShelli kaudu ühenduse konfigureerimine][V2VNETP2S].  UI teha see funktsioon pole veel saadaval. 

### <a name="creating-a-pre-configured-vnet"></a>Eelkonfigureeritud VNET loomine ###
Kui soovite luua uue VNET, mis on konfigureeritud lüüsi ja punkti saidi, Võrgundus kasutajaliides rakenduse teenus on võimalus seda teha, kuid ainult jaoks ressursihaldur VNET.  Kui soovite punkti saidi ja lüüsi klassikaline VNET loomiseks peate teha käsitsi võrgunduse kasutajaliidese kaudu. 

Ressursihaldur VNET VNET integreerimine Kasutajaliidese kaudu loomiseks lihtsalt valige **Luua uue virtuaalse võrgu** ja sisestage soovitud:

- Virtuaalne võrgu nimi
- Virtuaalse võrgu Aadressiplokk
- Alamvõrgu nimi
- Alamvõrgu aadressiplokk
- Lüüsi Aadressiplokk
- Punkti saidi Aadressiplokk

Kui soovite selle ühenduse loomiseks, mis tahes muud võrgu VNET siis tuleks vältida IP-aadress ruumi, mis kattub nende võrkude valimisel.  

>[AZURE.NOTE] Ressursihaldur VNET loomine lüüsi võtab aega umbes 30 minutit ja praegu ei integreerida on VNET oma rakendusega.  Kui teie VNET luuakse peate naaske rakendusse VNET integreerimine Kasutajaliidese ja valige oma uue VNET lüüsi.

![][3]

Azure'i VNETs tavaliselt on loodud privaatvõrgu aadressid.  Vaikimisi VNET integreerimine tee funktsioon mis tahes liikluse mõeldud sisse oma VNET need IP-aadresside vahemikud.  Privaatne IP-aadresside vahemikud on:

- – See on sama, mis 10.0.0.0 - 10.0.0.0/8 10.255.255.255
- – See on sama, mis 172.16.0.0 - 172.16.0.0/12 172.31.255.255 
- 192.168.0.0/16 – see on sama, mis 192.168.0.0 - 192.168.255.255
 
VNET aadressiruumi peab olema määratud CIDR märke.  Kui olete varem selliseid CIDR märke, on meetodi määratlemiseks aadress plokid IP-aadress ja täisarv, mis tähistab võrgu maski abil. Kui kiirülevaade, leiavad, et 10.1.0.0/24 oleks 256 aadressid ja 10.1.0.0/25 oleks 128 aadressid.  Mõne /32 aadressi IPv4 oleks lihtsalt 1 aadress.  

Kui seate DNS-i serveri teave tehke seejärel mis seatakse teie VNET jaoks.  Pärast VNET loomist saate muuta selle teabe VNET kasutuskogemuse.

Kui loote klassikaline VNET, VNET integreerimine Kasutajaliidese abil, loob see on VNET sama ressursi rühma nimega rakenduse. 

## <a name="how-the-system-works"></a>Süsteemi tööpõhimõte ##
All hõlmab see funktsioon koostab punkti saidi VPN tehnoloogia rakenduse ühenduse loomiseks oma VNET peal.   Rakenduste Azure rakenduse teenus on mitme rentniku süsteemi arhitektuur, mis välistab ettevalmistamise rakenduse otse soovitud VNET, nagu on tehtud virtuaalmasinates.  Punkti-saidi tehnoloogia tuginedes me piirata juurdepääsu lihtsalt virtuaalse masina majutusteenuse rakendus.  Täpsemaks piiratud juurdepääsu nende rakenduse hosts nii, et võrgud, mida saate konfigureerida neile juurde pääseb juurde ainult rakenduste.  

![][4]
 
Kui DNS-i server pole konfigureeritud virtuaalse võrguga, peate kasutama IP-aadressid.  Kasutades IP-aadressid, pidage meeles, et suur kasu, on see funktsioon on, et see võimaldab teil kasutada oma isiklike võrgustikus privaatne aadressi.  Kui seate oma rakenduse kasutama avaliku IP-aadresse ühe oma VMs, siis te ei kasuta VNET integratsiooni funktsiooni ja Interneti kaudu suhtlemiseks.


##<a name="managing-the-vnet-integrations"></a>VNET integreerimine funktsioonide haldamine##

Võimalus ühendamine ja ühenduse katkestamine on VNET on rakenduse tasemel.  Toimingud, mis võivad mõjutada VNET integreerimise üle mitme rakendused on ASP tasemel.  Kasutajaliidese, kuvatakse rakenduse tasemel saate andmeid oma VNET.  Enamik sama teave kuvatakse ka ASP tasemel.  

![][5]

Võrgu funktsiooni oleku lehe kaudu saate vaadata, kui teie rakendus on ühendatud teie VNET.  Kui teie VNET lüüs ei tööta jaoks mingil põhjusel siis seda näitab nii pole ühendatud.  

Teave, mida teil on nüüd saadaval rakenduses taseme VNET integreerimine UI on sama mis ASP saate üksikasjalikku teavet.  Siin on need üksused.

- VNET nimi - link avab selle võrgu kasutajaliides
- Asukoht – see näitab teie VNET asukoht.  On võimalik integreerimine on VNET teise asukohta.
- Serdi olek – on kasutada rakendust ja selle VNET vahelise ühenduse VPN serdid.  See näitab test tagada, et need on sünkroonitud.
- Lüüsi olek – peaks teie lüüsid olema alla mingil põhjusel siis rakenduse ei pääse soovitud VNET ressursside.  
- VNET aadressiruumi – see on teie VNET IP-aadress ruumi.  
- Valige käsk saidi aadressiruumi – see on saidi IP aadressiruumi oma VNET punkti.  Rakenduse jäänud side on pärit ühe IP-aadressi aadress siia.  
- Saidi saidi aadressiruumi - saidilt VPN abil saate luua ühendust oma VNET oma sees eeldusel ressursse või muude VNETs.  Kui teil on konfigureeritud, siis IP-aadresside vahemikud määratletud VPN-ühendus näitab siin.
- DNS-serverid – kui olete nimeservereid, siis need on loetletud siin oma VNET konfigureeritud.
- IP-d marsruuditakse VNET - seal on IP-aadressid, et teie VNET on marsruutimine jaoks määratletud loendit.  Nende aadressid näitab siin.  

Saate oma VNET integreerimise rakendus vaate ainult toimingu on rakenduse katkestamine VNET, see on praegu ühendatud.  Tehke seda lihtsalt nuppu Katkesta ühendus ülaosas.  See toiming ei muuda oma VNET.  Funktsiooni VNET ja see on konfiguratsiooni, sh lüüside ei muudeta.  Kui soovite kustutada teie VNET siis peate kustutama ressursside kohta, sh lüüside.  

Rakenduse teenuse leping vaade on mitmeid määratavaid täiendavad toimingud.  See on ka juurde teisiti kui rakendusest.  ASP saavutamiseks Võrgundus kasutajaliides lihtsalt avage oma ASP Kasutajaliidese ja liikuge allapoole.  Kasutajaliidese element, nimetatakse võrgu funktsiooni olek on.  See annab mõne hetke ümber oma VNET integreerimine.  Selles kasutajaliides klõpsamisel avatakse võrgu funktsiooni olek UI.  Kui klõpsate seejärel "Haldamiseks klõpsake siin" saate avada kasutajaliides, kus on loetletud VNET integratsioon selle ASP.

![][6]

ASP asukoht on hea meeles pidada, vaadates VNETs, mida on integreerimise asukohad.  Kui soovitud VNET on mõnes muus asukohas tõenäoliselt palju latentsus probleemid kuvamiseks.  

VNETs, integreeritud on see ASP ja kui palju teil on integreeritud rakenduste kohta, kui palju VNETs meeldetuletus.  

Iga VNET lisatud üksikasjade kuvamiseks klõpsake lihtsalt VNET, mis teid huvitab.  Lisaks üksikasjad mis olid märkida varem kuvatakse see ASP rakendused, mis kasutavad seda VNET loendit.  

Tegevuse osas on kaks esmane toimingud.  Esimene on võimalus lisada protsessid, jättes oma rakenduse sisse oma VNET liiklust.  Teine toiming on võimalus serdid ja võrgu teabe sünkroonimine.

![][7]

**Marsruutimine** Nagu varem on marsruudib, mis on määratletud teie VNET, mida kasutatakse suunavad liikluse oma VNET rakenduste sisse.  Kuigi kui klientide soovite saata täiendavad väljaminev liiklus rakendusest on VNET ja neid see funktsioon on esitatud on mõned kasutab.  Mis juhtub pärast seda, kui see on kuni kuidas kliendi konfigureerib nende VNET liiklust.  

**Serdid** Serdi oleku kajastab läbi valideerimiseks teenuse rakendus, et kasutame VPN-ühenduse serdid on endiselt hea märge.  Kui VNET integreerimine on lubatud, siis kui see on esimene integreerimine selle VNET rakendustel selle ASP on nõutav Exchange'i sertide ühenduse turvalisuse tagamiseks.  Serdid koos saame DNS-i konfiguratsiooni, marsruudib ja muid sarnaseid asju, mis kirjeldavad võrgu.
Kui need serdid või võrgu teave on muutunud siis pead klõpsake "Sünkroonimine Network".  **Märkus**: "Sünkroonimine Network" klõpsamisel ja seejärel kuvatakse põhjustada lühike katkestuste ühenduvuse rakenduse ja teie VNET vahel.  Samal ajal, kui teie rakendus pole taaskäivitatakse ühenduvuse kadu võib põhjustada saidi ei toimi õigesti.  

##<a name="accessing-on-premise-resources"></a>Juurdepääs eeldusel ressursid##

Üks VNET Kliendiintegratsiooni funktsioonide eeliseid on see, kui teie VNET on ühendatud sees eeldusel võrgu saidilt VPN seejärel teie rakendused on juurdepääs teie sees eeldusel ressursid rakenduste.  Seda teha, kui soovite uuendada oma sees eeldusel VPN-lüüsi lennuliini, mille teie saidi IP vahemiku.  Kui esmalt häälestanud saidilt VPN peaks saab konfigureerida selle skriptide marsruudib, sh oma saidi VPN osutage häälestada.  Kui lisate kohani pärast saidi VPN oma luua oma saidilt VPN, siis peate selle marsruudib käsitsi värskendada.  Täpsemat teavet selleks, et lüüsi kohta erinevad ja on kirjeldatud allpool.  

>[AZURE.NOTE] Ajal VNET integreerimise funktsioon töötab koos saidilt VPN juurdepääsu eeldusel ressursid see praegu ei tööta mõne ExpressRoute VPN-kaudu tehke sama.  See on true, kui klassikalise või ressursihaldur VNET integreerimine.  Kui teil on vaja juurdepääsu ressursid ExpressRoute VPN-i kaudu saate kasutada ka ASE, mida saate kasutada oma VNET. 

##<a name="pricing-details"></a>Hinnad üksikasjad##
On mõned hinnad nüansse, mida peaksite teadma, kui funktsiooni VNET integreerimise abil.  3 seotud kulude selle funktsiooni kasutamiseks on:

- ASP hinnad taseme nõuded
- Andmete edastamiseks kulud
- VPN-lüüsi kulud.

Rakenduste saaksid selle funktsiooni kasutamiseks, nad peavad olema Standard või Premium rakenduse teenuse kavandamine.  Saate vaadata nende kulude siin rohkem üksikasju: [Rakenduse teenuse hinnad][ASPricing]. 

Saidi VPN võimalus punkti tõttu on käsitsemise, teil alati olemas tasuta väljaminev liiklus andmete VNET integreerimine-ühenduse kaudu isegi juhul, kui selle VNET on sama andmekeskuse.  Et näha, mida on nende kulude Heitke pilk siin: [Andmete edastamiseks hinnad üksikasjade][DataPricing].  

Viimane üksus on VNET lüüside maksumus.  Kui teil pole vaja midagi muud, nt saidilt VPN lüüside siis olete maksab lüüside VNET integreerimine funktsiooni toetamiseks.  Kulusid siin on üksikasjad: [VPN lüüsi hinnad][VNETPricing].  

##<a name="troubleshooting"></a>Tõrkeotsing##

Kuigi see funktsioon on lihtne häälestamiseks mis ei tähendab, et teie töötamist probleemi tasuta.  Kui teil tekib probleeme, mis on juurdepääs teie soovitud lõpp-punkti seal on mõned Utiliidid abil saate testida Ühenduvus rakenduse konsooli.  On kaks konsooli kogemusi saate kasutada.  Üks on Kudu konsooli ja teine on konsool, mis on Azure portaali.  Rakenduste Kudu konsooli avamiseks minge Tööriistad -> Kudu.  See on sama, mis läheb [sitename]. scm.azurewebsites.net.  Kui avanevas lihtsalt Avage vahekaart konsooli silumine.  Saada Azure portaali majutatud konsooli, siis teie rakenduse-minge Tööriistad > konsooli.  


####<a name="tools"></a>Tööriistad####

Ping tööriistad, nslookup ja tracert ei tööta turvalisus piirangute tõttu konsooli kaudu.  Täitmiseks olnud kehtetu seal kaks eraldi tööriistu, mis on lisatud.  Selleks, et kontrollida DNS-i funktsionaalsus lisasime tööriista nimega nameresolver.exe.  Süntaks on järgmine:

    nameresolver.exe hostname [optional: DNS Server]

Saate kontrollida hostinimed, et teie rakendus sõltub nameresolver.  Nii saate kontrollida, kui teil on midagi valesti konfigureeritud DNS-i või võib-olla ei pääse oma DNS-i serverisse.

Järgmise tööriista, mis võimaldab teil testida TCP Ühenduvus host ja kombinatsiooni.  See tööriist on nn tcpping.exe ja süntaks on:

    tcpping.exe hostname [optional: port]

See tööriist antakse teada, kui kindla hosti ja portide, kuid saate selle ICMP koos sama ülesanne ei toimi vastavalt ping kasuliku.  ICMP ping kasuliku ütleb teile, kui teie host on üles.  Mis tcpping saate teada, kui pääsete kindla pordi hosti.  


####<a name="debugging-access-to-vnet-hosted-resources"></a>Juurdepääs VNET silumine majutatud ressursid####

On mitmeid asju, mida saate takistada rakenduse jõuda kindla hosti ja pordi.  Enamasti on kolm võimalust:

- **On tulemüüri kujul**  Kui teil on tulemüüri nagu siis klõpsamist TCP ajalõpp.  Mis on 21 sekundit sel juhul.  Tcpping tööriista abil saate testida Ühenduvus.  TCP ajalõpud saab paljusid asju, lisaks tulemüürid tõttu kuid alustamiseks seal.  
- **DNS-i on ei pääse juurde**  DNS-i ajalõpu on 3 sekundit DNS-i serveri kohta.  Kui teil on 2 DNS-serverid, mis on 6 sekundit.  Nameresolver abil saate vaadata, kas DNS-i töötab.  Pidage meeles, et te ei saa kasutada nslookup nagu mis ei kasuta teie VNET on konfigureeritud DNS-i.
- **Vigaste P2S IP vahemik** Saidi IP vahemikuks punkti peab olema RFC 1918 privaatne IP-vahemikud (10.0.0.0-10.255.255.255 / 172.16.0.0-172.31.255.255 / 192.168.0.0-192.168.255.255) kui vahemiku kasutab IP-d, mis väljaspool siis asjad, mida ei tööta.  

Kui neid üksusi ei vasta teie probleemi, vaadake esimese lihtsad asjad.  

- Kas lüüsi näitab olevaks portaali?
- Tehke sertide kuvamine on sünkroonitud?
- Kas igaüks võrgu konfigureerimise tegemata "Sünkroonimine võrgu" probleemse turbeteenuste muuta? 

Kui teie lüüs ei tööta seejärel avab selle tagasi.  Kui teie serdid on sünkroonitud minge oma VNET integreerimine ASP vaate ja tabas "Sünkroonimine Network".  Kui kahtlustate, et see on teie VNET konfiguratsiooni, tehtud muudatus ja see ei olnud sünkroonimine oleks koos oma turbeteenuste siis oma VNET integratsiooni vaatesse ASP minna, ja vajutage "Sünkroonimine Network" Just nagu meeldetuletuse, see võib põhjustada lühike katkestuste VNET ühendust ja teie rakendused.  

Kui kõik see on hea peate tõrkeid süvitsi veidi:

- Kas VNET integreerimise abil saavutada sama VNET ressursside rakendustel? 
- Saate rakenduse konsooli minge ja kasutada mis tahes muud ressursid on teie VNET jõuda tcpping?  

Kui täidetud on üks eespool siis oma VNET integreerimine on hea ja probleem on kuhugi mujale.  See on, kus ta saab olla rohkem probleemiks, sest ei ole näha, miks te ei jõua liides: lihtne viis.  Mõned põhjused on järgmised.

- teil on tulemüüri oma hosti, takistades Accessi rakenduste pordi oma punkti saidi IP vahemikuks.  Ületavate alamvõrku sageli nõuab tutvumist.
- teie target host ei tööta
- Teie taotlus on alla
- teil oli valesti IP- või hostname (hostinimi)
- Kui teie ootustele porti on listening rakenduse.  Võite vaadata seda minnes majutavate ja abil "netstat - aon" käsu viip:.  See näitab teile, mida ID on kuulamise mis port protsess.  
- oma võrgu turberühmad on konfigureeritud nii, et nad juurdepääsu vältimiseks oma rakenduse host ja teie saidi IP vahemiku punktist

Pidage meeles, et te ei tea, mida IP oma punkti saidi IP vahemikule, mida teie rakendus kasutab nii, et peate esmalt lubama juurdepääsu terve lahtrivahemik.  

Järgmised täiendavad silumine juhiseid.

- Logige oma VNET teise VM ja proovige oma ressursi host: port seal saavutamiseks.  Seal on mõned TCP ping Utiliidid, saate selleks kasutada või saate Telneti isegi juhul, kui peate olema.  Eesmärk siin on lihtsalt kindlaks teha, kui on olemas selle muude VM Ühenduvus. 
- avab teise VM- ja testi Accessi rakendus host selle ja konsooli rakenduste  
####<a name="on-premise-resources"></a>Eeldusel ressursid####
Kui teie ei jõua ressursid eeldusel, siis kontrollige esmalt on, kui jõuate ressursi oma VNET.  Kui see töö on üsna lihtne järgmiste juhiste juurde.  A VM oma VNET peate proovida sees eeldusel rakenduse saavutamiseks.  Saate kasutada Telneti või TCP ping kasuliku.  Kui teie VM ei saa teie sees eeldusel ressursi saavutamiseks siis esmalt veenduge, et teie saidilt VPN-ühendus ei tööta.  Kui see ei tööta kontrollige asjad, mida varem märkida, samuti sees eeldusel lüüsi konfigureerimine ja olek.  

Nüüd, kui teie VNET majutatud VM jõuavad teie sees süsteemi eeldusel, kuid teie rakendus ei saa, siis on põhjus tõenäoliselt üks järgmistest:
- teie marsruudib pole konfigureeritud koos oma osutage saidi IP-vahemikke eeldusel lüüsi sees
- oma võrgu turberühmad takistavad juurdepääsu oma punkti saidi IP vahemikuks
- teie sees eeldusel tulemüürid blokeerivad liiklus teie saidi IP vahemikuks
- teil on oma VNET, mis takistab teie saidi vastavalt liikluse jõudmist sees eeldusel võrgu kasutaja määratletud Route(UDR)

## <a name="hybrid-connections-and-app-service-environments"></a>Hübriidjuurutuse ühendusi ja rakenduse teenuse keskkonnas##
On 3 funktsioone, mis võimaldavad juurdepääsu VNET majutatud ressursid.  Need on:

- VNET integreerimine
- Hübriidjuurutuse ühendused
- Rakenduse teenuse keskkonnas

Hübriidjuurutuse ühendused kasutamiseks peate installima relay agent, nimetatakse hübriid ühenduse Manager(HCM) teie võrku.  Funktsiooni HCM peab olema võimalus ühenduse Azure'i ja rakenduse.  See lahendus on eriti suur, nt võrgu sees eeldusel remote võrgust või isegi teise pilve majutatud võrgu, kuna ei nõua internet puuetega inimestele juurdepääsetavate lõpp.  Funktsiooni HCM töötab ainult Windows ja teil võib olla kuni 5 esitada kõrge-saadavus töötavate eksemplaride.  Hübriidjuurutuse ühendused toetab ainult TCP küll ning iga HC lõpp-punkti peab vastama kindla hosti: port kombinatsiooni.  

Rakenduse teenuse keskkonna funktsioon võimaldab teil oma VNET eksemplari Azure'i rakendust Service käivitamiseks.  See võimaldab oma rakenduste juurdepääs ressurssidele oma VNET ilma mis tahes täiendavaid toiminguid tegema.  Mõnda muud eelised keskkonnas rakenduse teenus on, mida saate kasutada 8 core spetsiaalne töötajate 14 GB RAM.  Teine kasu on, saate muudate süsteemi teie vajadustele.  Erinevalt mitme rentniku keskkonnas, kus teie ASP maht on piiratud, on ASE saate reguleerida soovite anda süsteemile mitu ressursid.  Võrgu liikumine selle dokumendi küll, üks asju, mida saate koos ASE, mida te ei VNET integreerimise on, et saate töötada ExpressRoute VPN-i.  

Kuigi mõned kasutavad juhul kattumise, pole need funktsioon teistega asendada.  Teab, millist funktsiooni kasutamiseks on seotud teie vajadustele ja kuidas te soovite seda kasutada.  Näiteks:

- Kui olete arendaja ning soovite lihtsalt käivitage saidi Azure ja on see Accessi andmebaasis, klõpsake jaotises töölaua töökoha on kõige lihtsam asi kasutada hübriid ühendused.  
- Kui teil on suur ettevõte, mis soovib sellele suure hulga web atribuutide avalikkusele cloud ja hallata oma võrgu siis, kui soovite rakenduse teenuse keskkond minna.  
- Kui teil on rakenduse teenuse arvu majutatud rakendused ja soovite lihtsalt juurde pääseda oma VNET ressursside, siis VNET integreerimine on tee minna.  

Lisaks kasutamine juhtudel on mõned lihtne seotud aspekte.  Kui teie VNET on juba oma võrguga ühenduses sees eeldusel siis kasutades VNET integreerimise või keskkonnas rakenduse teenus on lihtne viis kasutamine eeldusel ressursid.  Teisalt, kui oma VNET on ühendatud võrku sisse eeldusel, siis on palju elemente, et häälestada saidilt VPN oma VNET võrreldes installimisel kuvatakse HCM.  

Lisaks on Funktsioonierinevused on ka hinnad erinevused.  Funktsiooni rakenduse teenuse keskkond on lisatasu teenuse pakkumine kuid pakub kõige võrgu konfigureerimise võimalusi lisaks muudele võimalustele.  VNET integreerimine saab kasutada Standard- või Premium turbeteenuste ja sobib turvaliselt tarbimine ressursside oma VNET mitme rentniku teenuse rakenduse kaudu.  Hübriidjuurutuse ühendused praegu sõltub BizTalki kontoga, millel on hinnad taset, et alustada tasuta ja seejärel saada järk-järgult kallim põhjal peate summa.  Kui tegemist on küll palju võrkude tööd, ei ole muud funktsioon nagu hübriid ühendused, mis võimaldab teil juurde pääseda ka üle 100 eraldi võrkudes ressursid.    


<!--Image references-->
[1]: ./media/web-sites-integrate-with-vnet/vnetint-upgradeplan.png
[2]: ./media/web-sites-integrate-with-vnet/vnetint-existingvnet.png
[3]: ./media/web-sites-integrate-with-vnet/vnetint-createvnet.png
[4]: ./media/web-sites-integrate-with-vnet/vnetint-howitworks.png
[5]: ./media/web-sites-integrate-with-vnet/vnetint-appmanage.png
[6]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanage.png
[7]: ./media/web-sites-integrate-with-vnet/vnetint-aspmanagedetail.png
[8]: ./media/web-sites-integrate-with-vnet/vnetint-vnetp2s.png

<!--Links-->
[VNETOverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-overview/ 
[AzurePortal]: http://portal.azure.com/
[ASPricing]: http://azure.microsoft.com/pricing/details/app-service/
[VNETPricing]: http://azure.microsoft.com/pricing/details/vpn-gateway/
[DataPricing]: http://azure.microsoft.com/pricing/details/data-transfers/
[V2VNETP2S]: http://azure.microsoft.com/documentation/articles/vpn-gateway-howto-point-to-site-rm-ps/
[IntPowershell]: http://azure.microsoft.com/documentation/articles/app-service-vnet-integration-powershell/
[ASEintro]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
