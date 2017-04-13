<properties
    pageTitle="Võrgu jõudluse kuvari lahenduse OMS | Microsoft Azure'i"
    description="Võrgu jõudluse jälgimine aitab teil jälgida oma võrgu-ja lähedal real-tööaeg – jõudlus tuvastada ja võrgu jõudluse kitsaskohti."
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
    ms.date="07/28/2016"
    ms.author="banders"/>

# <a name="network-performance-monitor-preview-solution-in-oms"></a>Võrgu jõudluse monitori (eelvaade) lahenduse OMS

>[AZURE.NOTE] See on [Eelvaade lahendus](log-analytics-add-solutions.md#log-analytics-preview-solutions-and-features).

Selles dokumendis kirjeldatakse, kuidas häälestamise ja kasutamise võrgu jõudluse kuvari lahendus OMS, mis aitab teil jälgida oma võrgu-ja lähedal real-tööaeg – jõudlus tuvastada ja otsige üles võrgu jõudluse kitsaskohad. Võrgu jõudluse kuvari lahenduse abil saate jälgida kaotsimineku ning latentsuse kahe võrgu, alamvõrku või serverite vahel. Võrgu jõudluse kuvari tuvastab võrguprobleemide nagu liikluse blackholing marsruutimise tõrked ja probleemid, mis tavalise võrgu jälgimise meetodid ei saa tuvastada. Võrgu jõudluse jälgimine teatiste genereeritud ja teavitab, kui võrguühendus on rikutud läve. Piirmäärasid automaatselt õpitut süsteem või saate konfigureerida kohandatud teatiste reegleid kasutada. Võrgu jõudluse jälgimine tagab õigeaegne võrgu jõudlusega seotud probleemide tuvastamine ja lokaliseerib võrku lõigu või seadme probleemi allikaks.

Saate tuvastada võrguprobleemide lahenduse armatuurlaud, mis kuvatakse summeeritud teave teie võrku, sh tehtud võrgu seisund sündmusi, vigane võrgu linkide ja subnetwork linke, mis on ees kõrge paketi kaotus ja latentsusaeg. Te saate süvitsimineku network link praegune seisund oleku subnetwork linke, kui ka sõlm sõlme lingid. Saate vaadata ka ajaloolisi trend ja latentsus võrgu, subnetwork ja sõlm sõlme tase. Saate tuvastada siirdamiseks võrguprobleemide vaatamise paketi kaotus ja latentsuse ajaloolisi trendi diagrammid ja võrgu kitsaskohti topoloogia kaardil. Interaktiivne topoloogia graafik võimaldab sõnumihüppe kohta, pärast mida hop võrgu protsesside visualiseerimine ja määrata probleemi allikas. Muid lahendusi, nt saate luua kohandatud aruandeid võrgu jõudluse kuvari abil kogutud andmete põhjal Log otsingu analytics erinevad nõuded.

Lahendus kasutab sünteetiline tehingute esmane süsteem tuvastamiseks võrgu vead. Jah, saate seda kasutada mõne kindla võrgu seadme tarnija või mudeli jaoks. See töötab kõigis hübriidkeskkonna kohapealse ja pilveteenuse (IaaS). Lahendus leiab automaatselt võrgu topoloogia ja erinevate marsruudib teie võrku.

Tüüpilised võrgu jälgimisega seotud toodete Keskenduge võrgu seade (ruuterid, parameetrid jne) seisundi jälgimine, kuid ei paku teadmisi tegeliku kvaliteedi võrguühendus ühest, ei võrgu jõudluse jälgimine.

### <a name="using-the-solution-standalone"></a>Lahendus autonoomse abil

Kui soovite jälgida oma kriitiliste töökoormus vaheline võrguühendus kvaliteedi, võrkude, andmekeskuste või Office'i saitidel, siis saate võrgu jõudluse kuvari lahendus ise vahelise ühenduvuse seisundi jälgimine:

- mitme andmekeskuste või Office'i saitidel, mis on ühendatud avalik või privaatne võrgu kasutamine
- kriitiline töökoormus, mis töötavad ärirakenduste
- avaliku pilveteenustega, näiteks Microsoft Azure'i või Amazon Web Services (AWS) ja kohapealse võrgu, kui teil on IaaS (VM) saadaval ja teil on konfigureeritud lubama side lüüside kohapealse võrgu cloud võrkude vahel
- Azure'i ja kohapealsete võrkude teekonna kasutamisel

### <a name="using-the-solution-with-other-networking-tools"></a>Lahendus abil teiste võrgu tööriistad

Kui soovite jälgida ärirakendusele rida, saate võrgu jõudluse kuvari lahendus companion lahendusena muud võrgu tööriistad. Aeglase võrguühenduse võib põhjustada aeglane rakenduste ja võrgu jõudluse jälgimine aitab teil on põhjustatud aluseks oleva võrgu probleemid rakenduse jõudlusprobleemide uurimine. Kuna lahendus ei nõua mis tahes juurdepääsu võrguseadmete, ei pea rakenduse administraatori toetuvad võrgu meeskonnatöö teavet selle kohta, kuidas mõjutab võrgu rakenduste.

Ka, kui muud võrgu jälgimise vahendeid juba investeerida siis lahendus saab täiendada nende tööriistade Kuna kõige traditsiooniline võrgu jälgimisega seotud lahendused ei paku teadmisi lõpuni võrgu jõudluse mõõdikute, näiteks kaotsimineku ja latentsusaeg.  Lahendus võrgu jõudluse jälgimine aitab täita selle vahe.


## <a name="installing-and-configuring-agents-for-the-solution"></a>Installimine ja konfigureerimine agentide lahendus

[Log Analytics ühenduse loomine Windows arvutite](log-analytics-windows-agents.md) ja [Ühenduse Toiminguhalduri Log Analytics](log-analytics-om-agents.md)agentide installimiseks kasutada basic protsessid.

>[AZURE.NOTE]
Peate installima vähemalt 2 agentide on piisavalt andmeid leida ja jälgida oma võrgu ressursse. Muul juhul lahendus jääb konfigureerimine olekus seni, kuni olete installinud ja konfigureerinud täiendavad agentide.

### <a name="where-to-install-the-agents"></a>Kuhu installida agentide

Enne installimist agentide, kaaluge topoloogia võrgu ja millised võrgu osad, mida soovite jälgida. Soovitame installida rohkem kui üks agent iga alamvõrku, mida soovite jälgida. Teisisõnu, iga alamvõrku, mida soovite jälgida, valida kahe või enama serverites või VM ja agent installimine.

Kui te ei ole kindel võrgu topoloogia, installige agentide serverite kriitilised töökoormus, kuhu soovite võrgu jõudluse jälgimist. Näiteks võite silma peal võrguühenduse veebiserverisse ja serveris, kus töötab SQL serveri vahel. Selles näites installite agent nii.

Agentide jälgida hosts--pole hosts vaheline võrguühendus (linkide). Nii, et jälgida network link, peate installima agentide selle lingi nii lõpp-punktid.

### <a name="configure-agents"></a>Agentide konfigureerimine

Kui olete installinud agentide, peate tulemüüri portide jaoks nendest arvutitest tagamaks, et agentide saate avada. Peate alla laadima ja seejärel käivitage PowerShelli aknas administraatoriõigustega [EnableRules.ps1 PowerShelli skripti](https://gallery.technet.microsoft.com/OMS-Network-Performance-04a66634) ilma parameetrid

Skripti loob registrivõtmete nõutud võrgu jõudluse jälgimine ja loob Windowsi tulemüüri reeglid lubama agentide omavahel TCP-ühenduste loomine. Loodud skripti registrivõtmed ka määrata, kas logida silumine logid ja logid faili tee. Samuti määratletakse agent TCP pordi edastamiseks kasutada. Need võtmed väärtused määratakse automaatselt skripti, nii, et ei peaks käsitsi muutma need võtmed.

Vaikimisi avatud port on 8084. Saate kasutada kohandatud pordi esitada parameetri `portNumber` skripti. Siiski sama port tuleks kasutada kõigis arvutites, kus käitatakse skripti.

>[AZURE.NOTE] Skripti EnableRules.ps1 konfigureerib vaid arvutis, kus käitatakse skripti Windowsi tulemüüri reeglid. Kui teil on võrgu tulemüür, veenduge, et see lubab liikluse mõeldud TCP pordi, mida kasutavad võrgu jõudluse jälgimine.


## <a name="configuring-the-solution"></a>Lahendus konfigureerimine

Kasutage järgmist teavet, installida ja konfigureerida lahendus.

1. Võrgu jõudluse kuvari lahenduse saab andmete arvutid, kus töötab Windows Server 2008 SP 1 või uuem versioon või Windows 7 SP1 või uuem versioon, mis on sama nõuded nimega Microsoft jälgimise Agent (MMA).
2. Võrgu jõudluse kuvari lahendus lisada oma OMS tööruumi abil [lisada Log Analytics](log-analytics-add-solutions.md)Solutions lahendusegaleriist kirjeldatud.  
  ![Võrgu jõudluse kuvari sümbol](./media/log-analytics-network-performance-monitor/npm-symbol.png)
3. OMS portaalis, kuvatakse uue nimega **Võrgu jõudluse kuvari** *lahendus nõuab täiendavaid konfiguratsiooni*sõnumiga paani. Peate konfigureerimine lahendus lisada võrkude põhjal subnetworks ja sõlmed, mis on avastatud kaudu. **Võrgu jõudluse kuvari** vaikimisi võrgu konfigureerimise alustamiseks klõpsake.  
  ![lahendus nõuab täiendavat konfiguratsioon](./media/log-analytics-network-performance-monitor/npm-config.png)


### <a name="configure-the-solution-with-a-default-network"></a>Lahendus vaikimisi võrguga konfigureerimine

Lehel konfiguratsioon kuvatakse ühe võrgu nimega **vaikimisi**. Pole määratletud ühtegi võrku, paigutatakse kõik automaatselt avastanud alamvõrku vaikimisi võrku.

Iga kord, kui loote võrgus, saate sellele lisada mõne alamvõrgu ja selle alamvõrgu on vaikimisi võrgust eemaldada. Kui kustutate võrgus, tagastatakse kõik selle alamvõrku vaikimisi võrgu automaatselt.

Teisisõnu, on vaikimisi võrgu container alamvõrku, mis sisalduvad mis tahes kasutaja määratletud võrgu jaoks. Saate redigeerida või kustutada vaikimisi võrku. Alati jääb süsteem. Siiski saate luua nii palju võrke, kui teil on vaja.

Enamikul juhtudel alamvõrku teie asutuses on rohkem kui üks võrk korraldada ja loote ühe või mitme võrgu loogiliselt rühmitamiseks teie alamvõrku.

### <a name="create-new-networks"></a>Uue võrgu loomine

Võrgu jõudluse monitoris võrk on alamvõrku ümbris. Saate luua võrgu nimega ja lisage alamvõrku võrku. Näiteks saate luua nimega *Building1* võrku ja seejärel lisage alamvõrku, või saate luua nimega *DMZ* võrku ja seejärel lisage kõik alamvõrku kuuluvate liimesnumber võrguga.

#### <a name="to-create-a-new-network"></a>Uue võrgu loomine

1. Klõpsake nuppu **Lisa võrk** ja seejärel tippige võrgu nimi ja kirjeldus.
2.  Valige üks või mitu alamvõrku, ja seejärel klõpsake nuppu **Lisa**.
3. Klõpsake nuppu **Salvesta** konfiguratsiooni salvestamiseks.  
  ![võrgu lisamine](./media/log-analytics-network-performance-monitor/npm-add-network.png)



### <a name="wait-for-data-aggregation"></a>Oodake, kuni andmete koondamine

Pärast salvestamist konfiguratsiooni esimest korda, käivitab lahendus sõlme, kuhu on installitud agentide vahel võrgu paketi kaotsimineku ja latentsusaeg teabe kogumine. See protsess võib võtta aega, mõnikord 30 minutit. Ajal olekus, paani võrgu jõudluse kuvari ülevaade lehel kuvatakse teade selle kohta, *andmete koondamine käigus*.

![andmete koondamine pooleli.](./media/log-analytics-network-performance-monitor/npm-aggregation.png)


Kui andmed on üles laaditud, kuvatakse teile võrgu jõudluse kuvari värskendatud paan kuvab andmed.

![Võrgu jõudluse kuvari paan](./media/log-analytics-network-performance-monitor/npm-tile.png)

Klõpsake paani kuvamiseks armatuurlaua võrgu jõudluse jälgimine.

![Võrgu jõudluse kuvari armatuurlaud](./media/log-analytics-network-performance-monitor/npm-dash01.png)

### <a name="edit-monitoring-settings-for-subnets"></a>Alamvõrku jälgimisega seotud sätete redigeerimine

Kõik alamvõrku, kuhu on installitud vähemalt üks agent on loetletud lehel konfigureerimine menüü **Subnetworks** .

#### <a name="to-enable-or-disable-monitoring-for-particular-subnetworks"></a>Lubada või keelata teatud subnetworks jälgimine

1. Märkige või tühjendage ruut **subnetwork ID** ja seejärel veenduge, et **jälgimis kasutamiseks** on valitud või tühi, sisestage asjakohane teave. Saate märkige või tühjendage mitu alamvõrku. Kui see on keelatud, ei jälgitakse subnetworks agentide värskendatakse pingida agentide lõpetada.
2. Valige sõlmed, mida soovite jälgida kindla subnetwork valite loendist soovitud subnetwork ja liigub nõutav sõlmed unmonitored ja jälgida sõlmed sisaldavate loendite vahel.
Saate lisada kohandatud **Kirjeldus** subnetwork, kui soovite.
3. Klõpsake nuppu **Salvesta** konfiguratsiooni salvestamiseks.  
  ![alamvõrgu redigeerimine](./media/log-analytics-network-performance-monitor/npm-edit-subnet.png)

### <a name="choose-nodes-to-monitor"></a>Valige sõlmed jälgimiseks

Kõik sõlmed, mis on installitud neid agent on loetletud **sõlmed** menüü.

#### <a name="to-enable-or-disable-monitoring-for-nodes"></a>Lubada või keelata sõlmed jälgimine

1. Märkige või tühjendage sõlmed, mida soovite jälgida või Lõpeta jälgimine.
2. Klõpsake raadionuppu **Kasuta jaoks jälgimis**, või tühjendage see, vastavalt vajadusele.
3. Klõpsake nuppu **Salvesta**.  
  ![luba sõlm jälgimine](./media/log-analytics-network-performance-monitor/npm-enable-node-monitor.png)


### <a name="set-monitoring-rules"></a>Kontrolli reeglid määramine

Võrgu jõudluse kuvari loob seisund sündmuste paari sõlmed või subnetwork või võrgu linke, kui on rikutud piirmäära Ühenduvus. Piirmäärasid automaatselt õpitut süsteem või neid saate konfigureerida kohandatud teatiste reeglid.

Süsteemi *vaikimisi reegel* on loodud ja loob seisund sündmuse iga kord, kui kaotsimineku või mis tahes kahe võrgu või subnetwork vahel latentsus lingid seotud rikkumiste eest süsteemi õpitut lävi. Soovi korral saate keelata vaikimisi reegel ja luua kohandatud kontrolli reeglid

#### <a name="to-create-custom-monitoring-rules"></a>Luua kohandatud kontrolli reeglid

1. Klõpsake nuppu **Lisa reegel** vahekaardil **kuvar** ja sisestage reegli nimi ja kirjeldus.
2. Valige sidumiseks võrgu või subnetwork linkide jälgimine loenditest.
3. Valige esmalt rippmenüüst võrgu network, milles sisaldub esimese subnetwork/s intressi ja valige rippmenüüst vastava subnetwork subnetwork/s.
Valige **Kõik subnetworks** , kui soovite jälgida kõiki subnetworks network link. Valige sarnaselt muude subnetwork/s huvi. Ja, võite klõpsata nuppu **Lisa erand** välistamine tehtud valikust kindla subnetwork linkide jälgimine.
4. Kui te ei soovi luua seisundi sündmuste jaoks valitud üksusi, siis tühjendage **lubamine seisundi jälgimine selle reegli hõlmatud linke**.
5. Valige jälgimise tingimused.
Saate määrata kohandatud lävede seisund sündmuse loomise, tippides läviväärtuse liugureid. Iga kord, kui tingimus väärtus läheb üle selle valitud piiri valitud võrgu/subnetwork paari, luuakse seisund sündmus.
6. Klõpsake nuppu **Salvesta** konfiguratsiooni salvestamiseks.  
  ![kohandatud jälgimisega seotud reegli loomine](./media/log-analytics-network-performance-monitor/npm-monitor-rule.png)


## <a name="data-collection-details"></a>Andmete kogumine üksikasjad

Võrgu jõudluse monitori kasutab TCP SYN-SYNACK-ACK käepigistus paketid kaotsimineku koguda ja latentsuse teave ja jälgimine kasutatakse ka topoloogia teabe saamiseks.

Järgmises tabelis on andmete kogumise meetodite ja muud üksikasjad kohta, kuidas andmeid kogutakse võrgu jõudluse monitori.

| platvorm | Otsest Agent | SCOM agent | Azure'i salvestusruum | SCOM on nõutav? | Rühma kaudu saadetud SCOM agendi andmed | saidikogumi sagedus |
|---|---|---|---|---|---|---|
| Windows |![Jah](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![Jah](./media/log-analytics-network-performance-monitor/oms-bullet-green.png)|![Ei](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|            ![Ei](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)|![Ei](./media/log-analytics-network-performance-monitor/oms-bullet-red.png)| TCP handshakes iga 5 sekundi järel andmeid saata iga 3 minuti järel |

Lahendus kasutab sünteetiline tehingute võrgu seisundi hindamiseks. OMS agentide installitud erinevate hetkel võrgu Exchange'i TCP pakettide üksteisega ja protsessi pendellevi aja ja paketi andmekadu, saate teada, kui mõni. Aeg-ajalt teostab iga agent Jälita marsruutimiseks muude agentide leidmiseks kõik erinevad marsruudib võrku, mida tuleb kontrollida. Agentide abil andmed, on võimalik tuletada võrgu latentsus ja paketi kaotsimineku arvud. Testide korduvad iga viie sekundi ja andmed on kolme minuti jooksul koondatakse enne üleslaadimine OMS abil.

>[AZURE.NOTE] Kuigi agentide omavahel suhelda sageli, nad ei looda palju võrguliikluse ajal testide tegemine. Agentide toetuvad ainult TCP SYN-SYNACK-ACK käepigistus paketid määratlemiseks kaotsimineku ja latentsusaeg--paketid vahetatakse andmed. Selle käigus agentide omavahel ainult siis, kui vaja suhelda ja agent side topoloogia on optimeeritud vähendada võrguliiklust.

## <a name="using-the-solution"></a>Lahendus abil

Selles jaotises selgitatakse kõik armatuurlaua funktsioonid ja kuidas neid kasutada.

### <a name="solution-overview-tile"></a>Lahendus ülevaade paan

Pärast seda, kui olete lubanud võrgu jõudluse kuvari lahenduse, pakub lahenduse paani lehel OMS ülevaade võrgu seisundi kiire ülevaade. Kuvab arvu terve ja vigane subnetwork lingid rõngasdiagrammil. Kui olete klõpsanud, avatakse lahenduse armatuurlaud.

![Võrgu jõudluse kuvari paan](./media/log-analytics-network-performance-monitor/npm-tile.png)


### <a name="network-performance-monitor-solution-dashboard"></a>Võrgu jõudluse kuvari lahenduse armatuurlaud

**Võrgu Kokkuvõte** tera kuvatakse Kokkuvõte võrkude koos suurusele. Sellele järgneb paanid nähtaval koguarv lingid, alamvõrgu linke ja teed süsteemis (tee koosneb kahe hosts agentide ja nende vahel kõik humala IP-aadressid).

**Ülemine võrgu seisund sündmuste** tera pakub loendi viimase seisundi sündmused ja teatised ja aeg alates sündmus on aktiivne. Seisund sündmus või teatise luuakse iga kord, kui paketi kaotsimineku või lingi võrgu või subnetwork latentsuse ületab läve.

Tera **Ülemine vigane lingid** kuvatakse vigane võrgu linkide loend. Need on võrgu linke, mis on ühe või mitme tervist sündmuse neid hetkel.

**Ülemine Subnetwork linkide kõige kaotsimineku** ja **Subnetwork seoste kõige latentsus** labad kuvamine ülemise subnetwork linkide paketi kaotus ja ülemise subnetwork linkide latentsus. Pika latentsusajaga või teatud summa paketi kaotus võib oodata teatud võrgu lingid. Selliste linkide ülemise kümme loendid, mis kuvatakse, kuid pole märgitud vigane.

**Levinud päringute** tera sisaldab otsingupäringuid, mis töötlemata võrgu jälgida andmeid otse toomiseks. Oma päringuid kohandatud aruannete loomiseks saate kasutada lähtepunktina need päringud.

![Võrgu jõudluse kuvari armatuurlaud](./media/log-analytics-network-performance-monitor/npm-dash01.png)


### <a name="drill-down-for-depth"></a>Sügavus süvitsimineku

Võite klõpsata erinevaid linke armatuurlaual lahenduse süvitsimineku süvitsi mõnda üksust väljal Huvialad. Näiteks kui näete teatise või vigane network link, mis kuvatakse armatuurlaual, võite klõpsata neid täiendavalt uurida. Lähete lehele, kus on loetletud subnetwork lingid võrku lingi. On võimalik iga subnetwork lingi kaotsimineku ning latentsuse seisund olekut ja kiiresti teada, millised subnetwork lingid on probleeme põhjustada. Klõpsake **vaate sõlm lingid** sõlm linkide vigane alamvõrgu lingi. Seejärel saate üksikuid sõlm sõlme linke ja vigane sõlm linke.

Klõpsake **vaate topoloogia** vaatamiseks sõnumihüppe kohta, pärast mida hop topoloogia marsruudib lähte-kui ka sõlme vahel. Vigane marsruudib või humal kuvatakse punane nii, et saate kiiresti tuvastada probleemi võrgu kindla osa.

![süvitsimineku andmed](./media/log-analytics-network-performance-monitor/npm-drill.png)


#### <a name="trend-charts"></a>Trendi-diagrammid

Igal tasemel, mida te süvitsimineku, saate vaadata trend ja latentsuse võrguühendus. Trendi diagrammid saadaolevaid Subnetwork ja sõlm lingid. Ajavahemik Graphi diagrammile diagrammi ülaosas aja juhtelemendi abil saate muuta.

Trend kuvatakse teie ajalooliste perspektiivi lingi võrgu jõudlust. Mõned võrguprobleemide on ajutise loomuga ja oleks raske jõuda ainult vaadates võrgu praegune olek. Selle põhjuseks probleemid saate kiiresti kuvada ja kaovad, enne kui kõik teated, vaid uuesti hiljem aega. Siirdamiseks küsimusi ka võib olla keeruline rakenduse administraatoritele, kuna need probleemid sageli pind nimega teadmata põhjusega tõus rakenduse aega, isegi kui kõik komponendid kuvatakse tõrgeteta.

Seda tüüpi küsimusi saab kerge vaevaga tuvastada, vaadates trendi diagrammi, kus probleemi kuvatakse ootamatu kühvli võrgu latentsus või paketi kaotus.

![diagrammi trendi.](./media/log-analytics-network-performance-monitor/npm-trend.png)

#### <a name="hop-by-hop-topology-map"></a>Sõnumihüppe kohta, pärast mida hop topoloogia kaart

Võrgu jõudluse kuvari näete sõnumihüppe kohta, pärast-,-sõnumihüppe kohta, pärast topoloogia marsruudib interaktiivsed topoloogia kaardil kahe sõlme vahel. Saate vaadata, valides lingi sõlm ja seejärel klõpsata käsku **Kuva topoloogia**topoloogia kaarti. Samuti saate vaadata topoloogia kaardi, kui klõpsate **teed** paani armatuurlaual. Kui klõpsate armatuurlaua **teed** , siis on teil valida lähte-kui ka sõlmed vasakpoolse paani ja seejärel klõpsake nuppu **diagrammi** diagrammile marsruudib kahe sõlme vahel.

Topoloogia kaart kuvatakse kaks sõlme ja mis on mitu teed andmed paketid võtta. Võrgu jõudluse kitsaskohad on tähistatud punase topoloogia kaart. Topoloogia kaardil punase värvi elemendid vaadates saate leida vigase võrguühenduse või vigased võrgu seade.

Kui klõpsate sõlm või libistage kursor üle selle topoloogia kaardil, näete atribuutide sõlm nagu FQDN- ja IP-aadress. Klõpsake soovitud sõnumihüppe kohta, pärast see on IP-aadress. Saate teatud marsruudib tühjendada ning seejärel valides ainult marsruudib, mida soovite kaardil esile tõsta esile tõsta. Teie kerimisratta abil saate suurendada või vähendada topoloogia kaardi.

Tähele, et kaardil kujutatud topoloogia on layer 3 topoloogia ja ei sisalda layer 2 seadmed ja ühendused.

![sõnumihüppe kohta, pärast mida hop topoloogia kaart](./media/log-analytics-network-performance-monitor/npm-topology.png)

#### <a name="fault-localization"></a>Vea lokaliseerimine

Võrgu jõudluse monitori on võimalik leida võrgu kitsaskohtade ilma ühenduse võrguseadmed. Mis see kogub võrgust ja rakendades täpsemalt algoritmide graafikul võrgu andmete põhjal võrgu jõudluse jälgimine muudab tõenäosusel hinnanguline võrgu osad, mis on kõige tõenäolisemad probleemi allikaks.

See lähenemine on võrgu kitsaskohtade kindlaks teha, kui juurdepääsu humala pole saadaval, kuna see ei nõuaks võrgu seadmest, nt marsruuteri või parameetrid koguda andmeid. See on kasulik, kui humala kahe sõlme vahel pole teie haldus juhtelemendis. Näiteks võib humala ISP ruuterid.

### <a name="log-analytics-search"></a>Log Analytics otsing

Kõik andmed, mis on graafiliselt exposed võrgu jõudluse kuvari armatuurlaua kaudu ja süvitsimineku lehed on saadaval algupäraselt Log Analytics otsingus. Saate päringu otsing päringukeelt kasutavate andmeid ja luua kohandatud aruandeid andmete eksportimine Exceli või PowerBI. **Levinud päringute** tera armatuurlaual on mõned kasulikud päringud, mis alguspunktiks oma päringute ja aruannete loomiseks saate kasutada.

![otsingupäringu](./media/log-analytics-network-performance-monitor/npm-queries.png)


## <a name="investigate-the-root-cause-of-a-health-alert"></a>Seisund teatise juurkausta põhjuse väljaselgitamiseks

Nüüd, kui olete lugenud võrgu jõudluse kuvar, vaatame lihtsa uurimine põhjus seisund sündmus.

1. Klõpsake lehe Ülevaade saate kiire ülevaate võrgu seisundi jälgides **Võrgu jõudluse kuvari** paani. Pange tähele, et jälgida 80 subnetworks linkide välja 43 on vigane. See nõuab uurimine. Klõpsake paani lahenduse armatuurlaua kuvamiseks.  
  ![Võrgu jõudluse kuvari paan](./media/log-analytics-network-performance-monitor/npm-investigation01.png)

2. Alloleval pildil näide, märkate, on praegu 4 seisund sündmused ja 4 võrgu lingid, mis on vigane. Otsustage, uurida ja klõpsake linki **SharePointi veebi** võrgu välja selgitada probleemi juurkaustas.  
  ![Vigane network link näide](./media/log-analytics-network-performance-monitor/npm-investigation02.png)

3. Süvitsimineku lehel kuvatakse kõik subnetwork lingid **SharePointi** võrgu veebilingi. Märkate, et nii subnetwork lingid ületanud latentsus teha network link vigane läve. Saate vaadata ka latentsus trendide subnetwork lingid. Saate aja valiku juhtelemendi Graphi nõutav ajavahemiku keskenduda. Näete päeva kui latentsus on jõudnud selle tippväärtus aega. Hiljem saate otsida logid selle aja jooksul uurida probleemi. Klõpsake käsku **Kuva sõlm lingid** süvitsimineku edasi.  
  ![Vigane alamvõrgu lingid näide](./media/log-analytics-network-performance-monitor/npm-investigation03.png)

4.  Sarnaselt eelmisele lehele, kindla subnetwork link süvitsimineku leht, mis on loetletud selle komponendi sõlm linkide alla. Saate sarnased toiminguid teha siin tegite eelmises etapis. Klõpsake **vaate topoloogia** vaatamiseks topoloogia 2 sõlme vahel.  
  ![Vigane sõlm lingid näide](./media/log-analytics-network-performance-monitor/npm-investigation04.png)

5. 2 valitud sõlmed teed joonestada topoloogia kaarti. Saate visualiseerida sõnumihüppe kohta, pärast mida hop topoloogia marsruudib topoloogia kaardil kahe sõlme vahel. See annab teile selge ülevaate mitu marsruudib esineda kahte sõlme vahel, mis võtab andmed paketid teed. Võrgu jõudluse kitsaskohad on tähistatud punase värvi. Topoloogia kaardil punase värvi elemendid vaadates saate leida vigase võrguühenduse või vigased võrgu seade.  
  ![Vigane topoloogia vaate näide](./media/log-analytics-network-performance-monitor/npm-investigation05.png)

6. Andmekadu, latentsus ja arv ning iga tee vaadatakse läbi **Tee üksikasjade** paanil. Selle näite puhul saate vaadata, on olemas 3 vigane teed paanil nimetatud. Kerimisriba abil saate vaadata nende vigane teed üksikasjad.  Valige üks teed nii, et ainult üks tee topoloogia diagrammile kandmise märkeruudud abil. Teie kerimisratta abil saate suurendada või vähendada topoloogia kaarti.

  Klõpsake soovitud pildi all selgelt näete kindla osa võrgu alad probleemi põhjuseks-teed ja humala punane värv. Klõpsates sõlm topoloogia kaardil näitab sõlme FQDN, sh atribuutide ja IP-aadress. Klõpsates soovitud sõnumihüppe kohta, pärast kuvatakse selle sõnumihüppe kohta, pärast IP-aadress.  
  ![Vigane topoloogia - tee üksikasjade näide](./media/log-analytics-network-performance-monitor/npm-investigation06.png)

## <a name="next-steps"></a>Järgmised sammud

- [Otsingu logid](log-analytics-log-searches.md) üksikasjalik võrgu jõudluse andmete kirjete vaatamiseks.
