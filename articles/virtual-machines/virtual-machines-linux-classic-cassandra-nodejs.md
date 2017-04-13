<properties
    pageTitle="Käivitage Cassandra Linux Azure | Microsoft Azure'i"
    description="Kuidas kasutada Cassandra kobar Linux Azure'i Virtuaalmasinates Node.js rakenduse kaudu"
    services="virtual-machines-linux"
    documentationCenter="nodejs"
    authors="hanuk"
    manager="wpickett"
    editor=""
    tags="azure-service-management"/>

<tags
    ms.service="virtual-machines-linux"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-linux"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="hanuk;robmcm"/>

# <a name="running-cassandra-with-linux-on-azure-and-accessing-it-from-nodejs"></a>Töötab Cassandra Linux Azure ja juurdepääsemiseks Node.js

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-classic-include.md)]Siit saate teada, kuidas [teha neid juhiseid kasutades ressursihaldur mudel](https://azure.microsoft.com/documentation/templates/datastax-on-ubuntu/).

## <a name="overview"></a>Ülevaade
Microsoft Azure'i on avatud pilve platvorm, mis töötab nii Microsoftile ka nii mitte-Microsofti tarkvara, mis sisaldab operatsioonisüsteemid, rakenduste serverid, sõnumside vahevara kui ka SQL-i ja NoSQL andmebaaside nii trükikojas ja avatud allika mudelite põhjal. Olles teenuste avaliku pilved, sh Azure koostamise vajab hoolikas planeerimine ja tahtlikult arhitektuur nii rakenduste serverites hästi salvestusruumi kihti. Cassandra's jaotatud salvestusruumi arhitektuur loomulikult aitab tugevalt saadaval süsteemide vea salliv jaoks kobar ebaõnnestumist. Cassandra on pilv skaala NoSQL andmebaasi säilitatakse Apache tarkvara Foundation cassandra.apache.org; Cassandra on kirjutatud Java ja seega töötab nii Windows kui ka Linux platvormide jaoks loodud.

Selles artiklis keskendutakse on näidata Cassandra juurutamise Ubuntu Microsoft Azure'i Virtuaalmasinates ja virtuaalne võrkude kasutamine ühe või mitme andmete keskmist klaster. Kobar juurutamise jaoks optimeeritud tootmise töökoormus on selle artikli välja, mis nõuab mitme ketta sõlm konfiguratsioon, asjakohane ring topoloogia kujundus ja andmete modelleerimine vajadusele dispersioonanalüüs, andmete järjepidevuse, läbilaskevõime ja kõrge-saadavus nõuded.

Selles artiklis võtab olulise lähenemine kuvamiseks, mis on seotud koostamise Cassandra kobar võrreldes keskmise suurusega, Chef või nuku, mida saab teha taristu juurutamise palju lihtsam.  

## <a name="the-deployment-models"></a>Juurutamise mudelite
Microsoft Azure'i võrgunduse võimaldab kasutuselevõtu isoleeritakse privaatne kogumite, mille access võib olla piiratud peene tekstuuriga võrgu turvalisuse saavutamiseks.  Kuna see artikkel on nähtaval Cassandra juurutamise olulise tasemel, me ei keskendub järjepidevuse taseme ja läbilaskevõime projekti optimaalse salvestusruumi. Siit leiate loendi networking meie hüpoteetilise kobar nõuded:

- Väliste süsteemide ei pääse Cassandra andmebaasi või väljaspool Azure
- Cassandra kobar peab olema laadi koormusetasakaalustusteenuse ökonoomsus liikluse taha
- Kahe rühmade jaoks täiustatud kobar-saadavus iga andmekeskuse Cassandra sõlmed juurutamine
- Lukustada klaster nii et ainult rakenduse serveripargi on Accessi andmebaasi otse
- Pole avaliku võrgu lõpp-punkte peale SSH
- Iga Cassandra sõlm vajab fikseeritud sisemise IP-aadress

Cassandra saab juurutada Azure alale ühe või mitme piirkonna põhjal jaotatud laadi töökoormus. Mitme piirkonna juurutamise mudeli saate kasutada teenida lõppkasutajal lähemale eriti geograafia sama Cassandra taristu kaudu. Cassandra's sisseehitatud sõlm dispersioonanalüüs hoolitseb mitme juhtslaidi sünkroonimise kirjutab pärinevate mitme andmekeskuste ja ühtse ülevaate rakenduste andmed. Mitme piirkonna juurutamise abiks olla laiem Azure'i teenus katkestuste riski vähendamiseks. Cassandra's Timmitavad järjepidevuse ja dispersioonanalüüs topoloogia aitab mitmesuguse RPO vajaduste rakendused.

### <a name="single-region-deployment"></a>Ühe piirkonna juurutamine
Me alustamine ühe piirkonna juurutuse ja kogutakse õppetunnid mitme piirkonna mudeli loomisel. Azure virtuaalse võrgunduse kasutatakse loomiseks isoleeritakse alamvõrku nii, et ülalnimetatud võrgu turbenõuetele täita.  Kirjeldatud ühe piirkonna juurutamise loomise protsess kasutab Ubuntu 14.04 LTS ja Cassandra 2.08; Siiski saate protsessi vastu hõlpsalt muude Linuxi variantidele. Järgnevalt mõned ühe piirkonna juurutuse süsteemne omadused.  

**Kõrge-Saadavus:** Cassandra sõlmed joonisel 1 on juurutatud kahte kättesaadavus nii, et sõlmed on levinud mitu viga domeeni jaoks kõrge-saadavus vahel. 2 viga domeenide VMs märgitud kättesaadavus igas kogumis on vastendatud.  Microsoft Azure'i kasutab mõiste viga domeeni haldamise planeerimata allapoole (nt või riistvara ebaõnnestumist) aja mõiste täiendamine domeeni (nt hosti või Külastajate OS lappimine/täienduste, rakenduse uuendused) kasutatakse haldamise ajastatud aega. Teemast [Azure rakenduste kõrge kättesaadavus ja](http://msdn.microsoft.com/library/dn251004.aspx) viga roll ja täiendamine domeenide kõrge-saadavus saavutamisel.

![Ühe piirkonna juurutamine](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux1.png)

Joonis 1: Ühe piirkonna juurutamine

Pange tähele, et kirjutamise ajal Azure'i ei luba VMs rühma teatud viga domeeniga konkreetsete kaardistamine Seega on isegi mudeliga juurutamise joonisel 1, statistiliselt tõenäoline kõik virtuaalmasinates vastendada kaks viga domeeni nelja asemel.

**Laadimine tasakaalustamiseks ökonoomsus liikluse:** Ökonoomsus kliendi teegid sees veebiserver ühenduse kobar on sisemine laadi koormusetasakaalustusteenuse kaudu. Selleks on vaja lisamise sisemise koormuse koormusetasakaalustusteenuse "andmed" alamvõrgu protsessi (vt joonisel 1) majutusteenuse Cassandra kobar pilveteenusesse kontekstis. Kui sisemise koormuse koormusetasakaalustusteenuse on määratletud, nõuab iga sõlme koormus tasakaalustatud lõpp-punkti lisatakse eelnevalt määratletud laadi koormusetasakaalustusteenuse nimega komplekti koormus tasakaalustatud marginaalidega. Üksikasjalikumat teavet leiate [Azure'i sisemine Koormusetasakaalustuseks ](../load-balancer/load-balancer-internal-overview.md).

**Klaster seemned:** See on oluline valida enamik väga kättesaadav sõlmed seemneturu nagu uusi sõlmi suhelda seemne sõlmed avastada klaster topoloogia. Ühe sõlme kättesaadavus igas kogumis on määratud seemne sõlmed ühe punkti rikkumise vältimiseks.

**Dispersioonanalüüs tegur ja järjepidevuse tase:** Cassandra's sisseehitatud kõrge-saadavus ja andmete kestvus iseloomustab Dispersioonanalüüs tegur (RF - iga rea, mis on talletatud klaster eksemplaride arv) ja järjepidevuse tase (vastuste olema lugeda/kirjutada tagasi tulemi helistaja arv). Dispersioonanalüüs tegur on määratud KEYSPACE (sarnaselt relatsiooniandmebaasist) loomise ajal järjepidevuse tase on täpsustada ajal emissiooni CRUD päring. [Seadistamine järjepidevuse](http://www.datastax.com/documentation/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html) järjepidevuse üksikasju ja kvoorumi arvutamise valem Cassandra dokumentatsioonist.

Cassandra toetab kahte tüüpi terviklus andmemudelite – järjepidevuse ja lõpuks järjepidevus; Dispersioonanalüüs tegur ja järjepidevuse taseme määrab koos kui andmed on ühtsete niipea, kui kirjutamine toiming on lõpule viidud või oleks lõpuks ühtsete. Näiteks täpsustades KVOORUMI nagu alati järjepidevuse tase tagab ajal mis tahes järjepidevuse tasemel andmete järjepidevuse, alla kirjutatud saavutamiseks vajadusel koopiate arvu KVOORUMI (nt üks), on tulemuseks lõpuks ühtsete andmed.

Ülaltoodud, dispersioonanalüüs teguri 3 ja kvoorum 8 sõlme klaster (2 sõlmed lugeda või kirjutada järjepidevuse) lugemis-ja kirjutamisõigusega järjepidevuse taset, võib jääda teoreetiline andmekadu kõige 1 sõlm dispersioonanalüüs rühma enne märganud selle rakenduse kohta. See eeldab, et kõik võtme tühikud on ka tasakaalustatud lugemis-ja kirjutamisõigusega taotlused.  Kasutame juurutatud kobar parameetrid on järgmised:

Ühe piirkonna Cassandra kobar konfiguratsioon:

| Parameetri kobar | Väärtus | Märkused |
| ----------------- | ----- | ------- |
| Arvu sõlmed (N). | 8   | Koguarvu sõlmed klaster |
| Dispersioonanalüüs tegur (RF) | 3 | Vastuste antud rea arv |
| Vormindusühtsuse tase (kirjutamine) | QUORUM[(RF/2) +1) = 2] valemi tulem ümardatakse allapoole | Kirjutab kõige 2 koopiad enne vastuse saatmist helistaja; 3. koopia kirjutatakse lõpuks ühtse viisil. |
| Vormindusühtsuse tase (vt) | KVOORUMI [(RF/2) + 1 = 2] valemi tulem ümardatakse allapoole | Funktsiooni helistaja vastuse saatmist loeb 2 koopiad. |
| Strateegia dispersioonanalüüs | NetworkTopologyStrategy kuvamiseks Cassandra dokumentatsioonist Lisateavet [Andmete kopeerimine](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) | Juurutamise topoloogia mõistab ja paigutab koopiad sõlmed nii, et kõik selle koopiad ei jõua sama sektsioon |
| Kähveltää | GossipingPropertyFileSnitch näha [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) Cassandra dokumentatsiooni kohta lisateabe saamiseks | NetworkTopologyStrategy kasutab mõiste Kähveltää topoloogia mõista. GossipingPropertyFileSnitch annab parema juhtimise vastendamise iga sõlme andmekeskuse ja sektsioon. Klaster kasutab gossip kajastuma seda teavet. See on lihtsam dünaamiline IP sätte suhtes PropertyFileSnitch |


**Azure kaalutluste kohta Cassandra kobar:** Microsoft Azure'i Virtuaalmasinates võimalus kasutab Azure'i bloobimälu ketta püsimine; Azure'i salvestusruumi salvestab iga ketta jaoks kõrge Kestvus 3 koopiad. Mida tähendab, et iga rea Cassandra tabelisse sisestatud andmed on juba salvestatud 3 koopiad ja seega andmete järjepidevuse on juba tehtud eest isegi juhul, kui Dispersioonanalüüs tegur (RF) on 1. Peamine probleem Dispersioonanalüüs tegur on 1 on, et rakendus kogemus tööseisakute isegi juhul, kui ühe Cassandra sõlm nurjub. Siiski kui sõlm on probleeme (nt riistvara, tarkvara süsteemirikete) tuvasta Azure struktuuri kontrolleril allapoole, see säte uus sõlm selle asemel, kasutades sama salvestusruumi draivid. Uue sõlme asendamiseks vana ettevalmistamise võib kuluda mõni minut.  Samuti kavandatud hooldustööde toiminguteks nagu külaline OS muudatused Cassandra uuendab ja rakenduse muudatused Azure struktuuri kontrolleril sooritab jooksvalt täienduste sõlmed klaster.  Jooksvalt täienduste ka võib kuluda paar sõlmed alla korraga ja seega klaster võib tekkida lühike tööseisakute jaoks mõne sektsioonid. Siiski andmed pole kaotsi sisseehitatud Azure Storage koondamise tõttu.  

Azure'i, mis ei nõua kõrge kättesaadavus (nt ümber 99,9, mis võrdub 8.76 tundi aastas; üksikasjad leiate [Kõrge-saadavus](http://en.wikipedia.org/wiki/High_availability) ) juurutatud süsteemidele võimalik pidamine RF = 1 ja järjepidevuse tase = üks.  Rakenduste RF kõrge-saadavus nõuetele = 3 ja järjepidevuse tase = KVOORUMI ei luba üks sõlmed ühe kujundusmuudatusi alla aega. RF = 1 traditsiooniline juurutuste (nt kohapealne) ei saa kasutada tuleneva probleeme nagu ketta tõrkeid võimalike andmekao tõttu.   

## <a name="multi-region-deployment"></a>Mitme piirkonna juurutamine
Cassandra's andmete keskmist arvestada kopeerimise ja järjepidevuse mudeli eespool kirjeldatud abil välja kasti ilma vajaduseta mis tahes välise instrumentaarium juurutamise mitme piirkond. See on erinevad traditsiooniline relatsiooniandmebaasid, kus saab häälestamise andmebaasi peegeldus mitme juhtslaidi kirjutab jaoks üsna keerukas. Cassandra häälestamine mitme piirkonnas aitab kasutamise stsenaariumid, sh järgmist:

**Lähedus vastavalt juurutamise:** Mitme rentniku rakendusi, Eemalda vastendus rentniku kasutajate-et-piirkond, saate kasu, mitme piirkonna kobar madal latentsused. Näiteks õppe juhtimine haridusasutuste jaoks saate juurutada jaotatud kobar Ida-USA-s ja Lääne USA piirkondades teenida vastav kohalik jaoks samuti analytics selgituseks. Andmeid saab kohalikult ühtsete aja loeb ja kirjutab ja saab lõpuks ühtsete nii piirkondades. On näiteks meediumi jaotuse, pood ja midagi muud näiteid ja kõike, mida pakub geo kontsentreeritud kasutaja base on selle juurutamise mudeli puhul kasutada.

**Kõrge-Saadavus:** Koondamise on oluline saavutamisel kättesaadavuse ja tarkvara; üksikasjad leiate Microsoft Azure Building usaldusväärne Cloud süsteemid. Microsoft Azure, ainult usaldusväärsed viis saavutada tõene koondamise on mitme piirkonna kobar juurutamine. Rakenduste loovad aktiivne aktiivne või passiivne aktiivne režiimis ja kui üks piirkondades on alla, Azure liikluse Manager saate suunata liiklus aktiivne regioon.  Koos ühe piirkonna juurutuse kui kättesaadavus on 99,9, kahe-piirkond juurutamise saavutada saadavus 99,9999 arvutatakse valemiga: (1-(1-0.999) *(1-0,999))*100); Vaadake lisateavet ülaltoodud paberi.

**Avariitaastet:** Mitme piirkonna Cassandra kobar, kui õigesti loodud, saate vastu katastroofiline andmete keskmist katkestuste. Kui ühe piirkonna ei tööta, saate muude piirkondade juurutatud rakendust käivitada serveeritakse lõppkasutajatele. Nagu mis tahes muu business järjepidevuse rakendusi, taotlus on olla jaoks teatud andmete kaotsimineku andmete asünkroonne teel. Kuid Cassandra muudab taastamis palju aega, mida traditsiooniline andmebaasi taastamine protsesside kui kiiremini. Joonis 2 näitab kaheksa sõlmed tüüpilised mitme piirkonna juurutamise mudeli iga piirkonna. Mõlemad piirkonnad on üksteisest sama ühel; tõeline kujundusi sõltuvad töökoormus tüüp (nt selgituseks või analytical), RPO, RTO, andmete järjepidevus ja kättesaadavus nõuded.

![Mitme piirkonna juurutamine](./media/virtual-machines-linux-classic-cassandra-nodejs/cassandra-linux2.png)

Joonis 2: Mitme piirkonna Cassandra juurutamine

### <a name="network-integration"></a>Võrgu integreerimine
Komplekti juurutatud võrgud, mis asub mõlema piirkonna virtuaalmasinates suhtleb üksteisest VPN tunneliga abil. VPN tunneliga ühendab kaks tarkvara lüüside võrgu juurutamise käigus ette valmistatud. Mõlemad piirkonnad on sarnane võrgu arhitektuur osas "web" ja "andmed" alamvõrku; Azure'i võrgunduse võimaldab nii palju alamvõrku vastavalt vajadusele luua ja rakendada ACL vajadusel võrgu turvalisus. Kobar topoloogia kujundamise ajal muu andmete keskmist side latentsus ja economic mõju võrgu liikluse vajadust pidada.

### <a name="data-consistency-for-multi-data-center-deployment"></a>Andmete järjepidevuse mitme andmekeskuse juurutamiseks
Jaotatud juurutuste peavad olema teadlik kobar topoloogia mõju läbilaskevõime ja kõrge kättesaadavus. RF ja järjepidevuse tase peavad olema märgitud nii, et kvoorum ei sõltu kõik andmekeskuste kättesaadavus.
Mis tuleb suure süsteemi LOCAL_QUORUM, järjepidevuse tasemeks (loeb ja kirjutab) veenduge, et kohaliku loeb ja kirjutab kohaliku sõlmed on täidetud, samal ajal, kui andmed on kopeeritud asünkroonselt remote andmekeskuste.  Tabel 2 Kokkuvõte mitme piirkonna klaster kirjeldatud hiljem üles kirjutama konfiguratsiooni üksikasjad.

**Kahe-piirkond Cassandra kobar konfigureerimine**


| Parameetri kobar | Väärtus | Märkused |
| ----------------- | ----- | ------- |
| Arvu sõlmed (N). | 8 + 8 | Koguarvu sõlmed klaster |
| Dispersioonanalüüs tegur (RF) | 3 | Vastuste antud rea arv |
| Vormindusühtsuse tase (kirjutamine) | LOCAL_QUORUM [(sum(RF)/2) +1) = 4] valemi tulem ümardatakse allapoole | 2 sõlmed kirjutatakse esimese andmekeskuse sünkroonselt; täiendavad 2 sõlmi kvoorumi jaoks kirjutatakse asünkroonselt 2 data Centeri kaudu. |
| Vormindusühtsuse tase (vt) | LOCAL_QUORUM ((RF/2) + 1) = 2 valemi tulem ümardatakse allapoole | Lugege taotlused on täidetud: ainult ühe piirkonna; 2 sõlmed on lugege enne vastuse saatmist kliendile. |
| Strateegia dispersioonanalüüs | NetworkTopologyStrategy kuvamiseks Cassandra dokumentatsioonist Lisateavet [Andmete kopeerimine](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureDataDistributeReplication_c.html) | Juurutamise topoloogia mõistab ja paigutab koopiad sõlmed nii, et kõik selle koopiad ei jõua sama sektsioon |
| Kähveltää | GossipingPropertyFileSnitch näha [Snitches](http://www.datastax.com/documentation/cassandra/2.0/cassandra/architecture/architectureSnitchesAbout_c.html) Cassandra dokumentatsiooni kohta lisateabe saamiseks | NetworkTopologyStrategy kasutab mõiste Kähveltää topoloogia mõista. GossipingPropertyFileSnitch annab parema juhtimise vastendamise iga sõlme andmekeskuse ja sektsioon. Klaster kasutab gossip kajastuma seda teavet. See on lihtsam dünaamiline IP sätte suhtes PropertyFileSnitch |


##<a name="the-software-configuration"></a>TARKVARA KONFIGUREERIMINE
Juurutamise käigus kasutatakse järgmist tarkvara versioonid:

<table>
<tr><th>Tarkvara</th><th>Allikas</th><th>Versioon</th></tr>
<tr><td>JRE </td><td>[JRE 8](http://www.oracle.com/technetwork/java/javase/downloads/server-jre8-downloads-2133154.html) </td><td>8U5</td></tr>
<tr><td>JNA TEMA </td><td>[JNA TEMA](https://github.com/twall/jna) </td><td> 3.2.7</td></tr>
<tr><td>Cassandra</td><td>[Apache Cassandra 2.0.8](http://www.apache.org/dist/cassandra/2.0.8/apache-cassandra-2.0.8-bin.tar.gz)</td><td> 2.0.8</td></tr>
<tr><td>Ubuntu  </td><td>[Microsoft Azure'i](https://azure.microsoft.com/) </td><td>14.04 LTS</td></tr>
</table>

Kuna allalaadimise JRE, tuleb käsitsi Oracle'i litsentsi, lihtsustamiseks juurutamise, alla laadida vajalik tarkvara hiljem üleslaadimise Ubuntu malli pilti loome kobar juurutusega eelkäijana töölauale.

Ülaltoodud tarkvara tuntud allalaadimine kataloogi (nt Windows %TEMP%/downloads või ~/Downloads enamik Linuxi või Mac-arvutisse) kohalikku arvutisse alla laadida.

### <a name="create-ubuntu-vm"></a>UBUNTU VM LOOMINE
Selles etapis tuleb protsessi loome Ubuntu pilt teegiõiguste tarkvara nii, et pilti saab uuesti kasutada mitu Cassandra sõlme ettevalmistamine.  
####<a name="step-1-generate-ssh-key-pair"></a>Samm 1: Luua SSH paari
Azure'i peab mõne X509 avalik võti, mis on PEM või DER kodeeritud ettevalmistamise ajal. Luua avaliku/era võti paari asub, kuidas kasutada SSH Linux Azure juhiste abil. Kui kavatsete kasutada putty.exe Windowsi või Linuxi SSH klient, olete teisendada PEM, kodeeritud RSA privaatvõti PPK vormingusse abil puttygen.exe; See juhised leiate eeltoodud veebilehele.

####<a name="step-2-create-ubuntu-template-vm"></a>Samm 2: Looge Ubuntu malli VM
VM malli loomiseks Azure klassikaline portaali sisse logida ja kasutada järgmisi: klõpsake nuppu Uus, Arvuta, VIRTUAALSE masina, alates Galerii, UBUNTU, Ubuntu Server 14.04 LTS ja seejärel klõpsake paremnoolt. Õppeteema, mis kirjeldab, kuidas luua Linux VM, vaadake teemat virtuaalse masina töötab Linux.

Sisestage kuval "virtuaalse masina konfiguratsioon" #1 järgmine teave:

<table>
<tr><th>VÄLJA NIMI              </td><td>       VÄLJA VÄÄRTUS               </td><td>         MÄRKUSED                </td><tr>
<tr><td>VÄLJAANDMISAEG VERSIOON    </td><td> Valige soovitud drow kuupäev allapoole</td><td></td><tr>
<tr><td>VIRTUAALSE MASINA NIMI    </td><td> Cass-Mall                </td><td> See on hostname VM </td><tr>
<tr><td>ESIMESE TASEME                     </td><td> STANDARD                        </td><td> Jätke vaikimisi              </td><tr>
<tr><td>SUURUS                     </td><td> A1                              </td><td>Valige VM vastavalt vajadustele IO; Selleks jätke vaikimisi </td><tr>
<tr><td> UUS KASUTAJA NIMI           </td><td> localadmin                      </td><td> "admin" on reserveeritud kasutajanime Ubuntu 12 xx ja pärast</td><tr>
<tr><td> AUTENTIMINE      </td><td> Märkige ruut                 </td><td>Kontrollige, kas soovite secure on SSH võti </td><tr>
<tr><td> SERT             </td><td> avalik võti serdi faili nimi </td><td> Kasutage varem loodud avalik võti</td><tr>
<tr><td> Uus parool   </td><td> keeruka parooli </td><td> </td><tr>
<tr><td> Kinnitage parool   </td><td> keeruka parooli </td><td></td><tr>
</table>

Sisestage kuval "virtuaalse masina konfiguratsioon" #2 järgmine teave:

<table>
<tr><th>VÄLJA NIMI             </th><th> VÄLJA VÄÄRTUS                       </th><th> MÄRKUSED                                 </th></tr>
<tr><td> PILVETEENUSES  </td><td> Looge uus pilveteenuses    </td><td>Pilveteenuses on ümbris arvutada nagu virtuaalmasinates ressursid</td></tr>
<tr><td> PILVETEENUSE TEENUSE DNS-I NIMI </td><td>Ubuntu-template.cloudapp.net   </td><td>Seadme agnostik laadi koormusetasakaalustusteenuse nimi</td></tr>
<tr><td> REGIOON/OSALEJA RÜHMA/VIRTUAALSE VÕRGU </td><td>    Lääne USA. </td><td> Valige piirkond, kust teie veebirakenduste juurdepääs Cassandra kobar</td></tr>
<tr><td>SALVESTUSRUUMI KONTO </td><td>   Vaikimisi kasutada. </td><td>Kindla regiooni salvestusruumi vaikekonto või varem loodud salvestusruumi konto kasutamine</td></tr>
<tr><td>KÄTTESAADAVUS MÄÄRAMINE </td><td>  Ükski </td><td>  Jätke tühjaks</td></tr>
<tr><td>LÕPP-PUNKTID   </td><td>Vaikimisi kasutada. </td><td>  Kasutage vaikimisi SSH konfigureerimine </td></tr>
</table>

Klõpsake paremnoolt, jätke vaikesätted ekraanil #3 ja VM ettevalmistamise protsessi lõpuleviimiseks nuppu "kontrolli". Pärast paar minutit, tuleks VM nime "ubuntu-malli" "töötab" olek.

###<a name="install-the-necessary-software"></a>VAJALIK TARKVARA INSTALLIMINE
####<a name="step-1-upload-tarballs"></a>Samm 1: Laadi pakitud
Kasutades scp või pscp, kopeerige eelnevalt allalaaditud tarkvara ~/downloads kataloogi käsk järgmises vormingus:

#####<a name="pscp-server-jre-8u5-linux-x64targz-localadminhk-cas-templatecloudappnethomelocaladmindownloadsserver-jre-8u5-linux-x64targz"></a>pscp server-jre-8u5-linux-x64.tar.gzlocaladmin@hk-cas-template.cloudapp.net:/home/localadmin/downloads/server-jre-8u5-linux-x64.tar.gz

Korrake eeltoodud käsu JRE samuti nagu Cassandra bittide.

####<a name="step-2-prepare-the-directory-structure-and-extract-the-archives"></a>Samm 2: Ettevalmistamine kataloogi struktuuri ja väljavõte arhiivi
Logige sisse VM ja luua kataloogi struktuuri ja ekstrakti tarkvara on administraatori, kasutades bash skripti allpool:

    #!/bin/bash
    CASS_INSTALL_DIR="/opt/cassandra"
    JRE_INSTALL_DIR="/opt/java"
    CASS_DATA_DIR="/var/lib/cassandra"
    CASS_LOG_DIR="/var/log/cassandra"
    DOWNLOADS_DIR="~/downloads"
    JRE_TARBALL="server-jre-8u5-linux-x64.tar.gz"
    CASS_TARBALL="apache-cassandra-2.0.8-bin.tar.gz"
    SVC_USER="localadmin"

    RESET_ERROR=1
    MKDIR_ERROR=2

    reset_installation ()
    {
       rm -rf $CASS_INSTALL_DIR 2> /dev/null
       rm -rf $JRE_INSTALL_DIR 2> /dev/null
       rm -rf $CASS_DATA_DIR 2> /dev/null
       rm -rf $CASS_LOG_DIR 2> /dev/null
    }
    make_dir ()
    {
       if [ -z "$1" ]
       then
          echo "make_dir: invalid directory name"
          exit $MKDIR_ERROR
       fi

       if [ -d "$1" ]
       then
          echo "make_dir: directory already exists"
          exit $MKDIR_ERROR
       fi

       mkdir $1 2>/dev/null
       if [ $? != 0 ]
       then
          echo "directory creation failed"
          exit $MKDIR_ERROR
       fi
    }

    unzip()
    {
       if [ $# == 2 ]
       then
          tar xzf $1 -C $2
       else
          echo "archive error"
       fi

    }

    if [ -n "$1" ]
    then
       SVC_USER=$1
    fi

    reset_installation
    make_dir $CASS_INSTALL_DIR
    make_dir $JRE_INSTALL_DIR
    make_dir $CASS_DATA_DIR
    make_dir $CASS_LOG_DIR

    #unzip JRE and Cassandra
    unzip $HOME/downloads/$JRE_TARBALL $JRE_INSTALL_DIR
    unzip $HOME/downloads/$CASS_TARBALL $CASS_INSTALL_DIR

    #Change the ownership to the service credentials

    chown -R $SVC_USER:$GROUP $CASS_DATA_DIR
    chown -R $SVC_USER:$GROUP $CASS_LOG_DIR
    echo "edit /etc/profile to add JRE to the PATH"
    echo "installation is complete"


Kui kleebite selle skripti vim aken, veenduge, et eemaldada uuel ('\r ") abil järgmine käsk:

    tr -d '\r' <infile.sh >outfile.sh

####<a name="step-3-edit-etcprofile"></a>Samm 3: Jne/profiili redigeerimine
Lisab lõpus:

    JAVA_HOME=/opt/java/jdk1.8.0_05
    CASS_HOME= /opt/cassandra/apache-cassandra-2.0.8
    PATH=$PATH:$HOME/bin:$JAVA_HOME/bin:$CASS_HOME/bin
    export JAVA_HOME
    export CASS_HOME
    export PATH

####<a name="step-4-install-jna-for-production-systems"></a>Samm 4: Installi JNA tema tootmissüsteemide jaoks
Abil järgmine käsk: järgmine käsk installitakse JNA tema-3.2.7.jar ja JNA tema-platform-3.2.7.jar /usr/share.java kataloogi versiooni installimine libjna-java

Luua viitu $CASS_HOME/teegi directory nii, et Cassandra startup skripti saate otsida nende purgid:

    ln -s /usr/share/java/jna-3.2.7.jar $CASS_HOME/lib/jna.jar

    ln -s /usr/share/java/jna-platform-3.2.7.jar $CASS_HOME/lib/jna-platform.jar

####<a name="step-5-configure-cassandrayaml"></a>Juhis 5: Konfigureerimine cassandra.yaml
Iga VM kajastamiseks konfigureerimine [me saab kohandada efektisuvandite see ajal tegelik ettevalmistamise] virtuaalmasinates nõutud cassandra.yaml redigeerimiseks tehke järgmist.

<table>
<tr><th>Välja nimi   </th><th> Väärtus  </th><th> Märkused </th></tr>
<tr><td>cluster_name </td><td>  "Tegelevate kohalike teenindusettevõtete loendit"   </td><td> Kasutage nime, mis kajastab juurutamise</td></tr>
<tr><td>listen_address  </td><td>[jätke tühjaks]   </td><td> "Localhost" kustutamine </td></tr>
<tr><td>rpc_addres   </td><td>[jätke tühjaks]  </td><td> "Localhost" kustutamine </td></tr>
<tr><td>seemned   </td><td>"10.1.2.4, 10.1.2.6, 10.1.2.8" </td><td>Kõik IP-aadressid, mis on määratud seemned loend.</td></tr>
<tr><td>endpoint_snitch </td><td> org.apache.cassandra.locator.GossipingPropertyFileSnitch </td><td> Seda kasutatakse funktsiooni NetworkTopologyStrateg tuletamise andmekeskuse ja VM raami</td></tr>
</table>

####<a name="step-6-capture-the-vm-image"></a>Samm 6: Jäädvustada VM pilt
Hostname (hk-cas-template.cloudapp.net) ja SSH privaatvõti varem loodud abil virtuaalse masina sisse logida. Lugege teemat Kuidas kasutada SSH Azure kohta, kuidas sisse logida kasutades käsku ssh või putty.exe Linux.

Järgmised toimingud jäädvustada pildi jada täita:
#####<a name="1-deprovision"></a>1. deprovision
Käsu "sudo waagent – deprovision + kasutaja" virtuaalse masina eksemplari kindla teabe eemaldada. [Kuidas jäädvustada Linux virtuaalse masina](virtual-machines-linux-classic-capture-image.md) kasutamine mallina kohta täpsema teabe kuvamiseks pilt kogumise protsessi.

#####<a name="2-shutdown-the-vm"></a>2: VM sulgemine
Veenduge, et virtuaalse masina on esile tõstetud ja käsuribal all linki SULGUMIST.

#####<a name="3-capture-the-image"></a>3: jäädvustada pilt
Veenduge, et virtuaalse masina on esile tõstetud ja käsuribal all linki JÄÄDVUSTADA. Järgmisel kuval andke soovitud pildi nimi (nt hk-cas-2-08-ub-14-04-2014071), sobiv pildi kirjeldus, ja klõpsake käsku "märge" ANDMEHÕIVE protsessi lõpuleviimiseks.

See on mõne hetke aega võtta ja pilt peaks olema Pildigalerii jaotises Minu pildid. Allika VM saab automaatselt delated pärast pildi tegemist edukalt.

##<a name="single-region-deployment-process"></a>Ühe piirkonna juurutuse protsess
**Samm 1: luua virtuaalse võrgu** Azure'i klassikaline portaali sisse logida ja luua virtuaalse võrgu tabeli Kuva atribuudid. Üksikasjalikud juhised protsessi teemast [konfigureerimine Cloud-Only virtuaalse võrgu Azure klassikaline portaalis](../virtual-network/virtual-networks-create-vnet-classic-portal.md) .      

<table>
<tr><th>VM atribuudi nimi</th><th>Väärtus</th><th>Märkused</th></tr>
<tr><td>Nimi</td><td>VNet-cass-Lääne-us</td><td></td></tr>
<tr><td>Piirkond</td><td>Lääne USA.</td><td></td></tr>
<tr><td>DNS-serverid </td><td>Ükski</td><td>See ignoreerida, kuna me ei kasuta DNS-i Server</td></tr>
<tr><td>Punkti saidi VPN-i konfigureerimine</td><td>Ükski</td><td> Selle ignoreerimine</td></tr>
<tr><td>VPN saitide konfigureerimine</td><td>Nnone</td><td> Selle ignoreerimine</td></tr>
<tr><td>Aadressiruumi jaoks</td><td>10.1.0.0/16</td><td></td></tr>
<tr><td>Alates IP</td><td>10.1.0.0</td><td></td></tr>
<tr><td>CIDR </td><td>/ 16 (65531)</td><td></td></tr>
</table>

Järgmised alamvõrku lisamiseks tehke järgmist.

<table>
<tr><th>Nimi</th><th>Alates IP</th><th>CIDR</th><th>Märkused</th></tr>
<tr><td>Web</td><td>10.1.1.0</td><td>/ 24 (251)</td><td>Alamvõrgu web serveripargi</td></tr>
<tr><td>andmete</td><td>10.1.2.0</td><td>/ 24 (251)</td><td>Andmebaasi sõlmed alamvõrgu</td></tr>
</table>

Andmete ja Web alamvõrku kaitset kaudu, mis on väljapoole seda artiklit võrgu turberühmad.  

**Samm 2: ettevalmistamine Virtuaalmasinates** Pilt, mis on varem loodud kasutamisel me loomine järgmised virtuaalmasinates cloud serveri "hk-c-svc-Lääne" ja siduda need vastava alamvõrku, nagu allpool näidatud:

<table>
<tr><th>Arvuti nimi    </th><th>Alamvõrgu </th><th>IP-aadress </th><th>Kättesaadavus määramine</th><th>Näiteks Põhiliselt plaati</th><th>Seemne?</th></tr>
<tr><td>HK-c1-Lääne-us   </td><td>andmete   </td><td>10.1.2.4   </td><td>HK-c-aset-1    </td><td>AV = WESTUS sektsioon = rack1 </td><td>Jah</td></tr>
<tr><td>HK-c2-Lääne-us   </td><td>andmete   </td><td>10.1.2.5   </td><td>HK-c-aset-1    </td><td>AV = WESTUS sektsioon = rack1 </td><td>Ei </td></tr>
<tr><td>HK-c3-Lääne-us   </td><td>andmete   </td><td>10.1.2.6   </td><td>HK-c-aset-1    </td><td>AV = WESTUS sektsioon = rack2 </td><td>Jah</td></tr>
<tr><td>HK-c4-Lääne-us   </td><td>andmete   </td><td>10.1.2.7   </td><td>HK-c-aset-1    </td><td>AV = WESTUS sektsioon = rack2 </td><td>Ei </td></tr>
<tr><td>HK-c5-Lääne-us   </td><td>andmete   </td><td>10.1.2.8   </td><td>HK-c-aset-2    </td><td>AV = WESTUS sektsioon = rack3 </td><td>Jah</td></tr>
<tr><td>HK-c6-Lääne-us   </td><td>andmete   </td><td>10.1.2.9   </td><td>HK-c-aset-2    </td><td>AV = WESTUS sektsioon = rack3 </td><td>Ei </td></tr>
<tr><td>HK-c7-Lääne-us   </td><td>andmete   </td><td>10.1.2.10  </td><td>HK-c-aset-2    </td><td>AV = WESTUS sektsioon = rack4 </td><td>Jah</td></tr>
<tr><td>HK-c8-Lääne-us   </td><td>andmete   </td><td>10.1.2.11  </td><td>HK-c-aset-2    </td><td>AV = WESTUS sektsioon = rack4 </td><td>Ei </td></tr>
<tr><td>HK-w1-Lääne-us   </td><td>Web    </td><td>10.1.1.4   </td><td>HK-w-aset-1    </td><td>                       </td><td>N/A</td></tr>
<tr><td>HK-w2-Lääne-us   </td><td>Web    </td><td>10.1.1.5   </td><td>HK-w-aset-1    </td><td>                       </td><td>N/A</td></tr>
</table>

Ülaltoodud loendis VMs loomine nõuab järgmist protsessi:

1.  Konkreetses piirkonnas on tühi pilveteenuses loomine
2.  Luua VM varem pilti ja lisage see virtuaalse võrgu loodud varem; Korrake seda kõigi VMs
3.  Lisada ka sisemise koormuse koormusetasakaalustusteenuse pilveteenusesse ja manustamiseks alamvõrgu "andmed"
4.  Iga VM varem loodud, lisage ökonoomsus liikluse kaudu ühendatud varem loodud sisemise koormuse koormusetasakaalustusteenuse koormus tasakaalustatud koormus tasakaalustatud lõpp-punkti

Ülaltoodud protsessi saab teostada Azure klassikaline portaalis; Windowsi arvuti (Kasuta VM Azure, kui teil pole juurdepääsu Windowsi arvuti), kasutage järgmist PowerShelli skripti abil ette kõik 8 VMs automaatselt.

**Loendi 1: PowerShelli skripti virtuaalmasinates ettevalmistamine**

        #Tested with Azure Powershell - November 2014
        #This powershell script deployes a number of VMs from an existing image inside an Azure region
        #Import your Azure subscription into the current Powershell session before proceeding
        #The process: 1. create Azure Storage account, 2. create virtual network, 3.create the VM template, 2. crate a list of VMs from the template

        #fundamental variables - change these to reflect your subscription
        $country="us"; $region="west"; $vnetName = "your_vnet_name";$storageAccount="your_storage_account"
        $numVMs=8;$prefix = "hk-cass";$ilbIP="your_ilb_ip"
        $subscriptionName = "Azure_subscription_name";
        $vmSize="ExtraSmall"; $imageName="your_linux_image_name"
        $ilbName="ThriftInternalLB"; $thriftEndPoint="ThriftEndPoint"

        #generated variables
        $serviceName = "$prefix-svc-$region-$country"; $azureRegion = "$region $country"

        $vmNames = @()
        for ($i=0; $i -lt $numVMs; $i++)
        {
           $vmNames+=("$prefix-vm"+($i+1) + "-$region-$country" );
        }

        #select an Azure subscription already imported into Powershell session
        Select-AzureSubscription -SubscriptionName $subscriptionName -Current
        Set-AzureSubscription -SubscriptionName $subscriptionName -CurrentStorageAccountName $storageAccount

        #create an empty cloud service
        New-AzureService -ServiceName $serviceName -Label "hkcass$region" -Location $azureRegion
        Write-Host "Created $serviceName"

        $VMList= @()   # stores the list of azure vm configuration objects
        #create the list of VMs
        foreach($vmName in $vmNames)
        {
           $VMList += New-AzureVMConfig -Name $vmName -InstanceSize ExtraSmall -ImageName $imageName |
           Add-AzureProvisioningConfig -Linux -LinuxUser "localadmin" -Password "Local123" |
           Set-AzureSubnet "data"
        }

        New-AzureVM -ServiceName $serviceName -VNetName $vnetName -VMs $VMList

        #Create internal load balancer
        Add-AzureInternalLoadBalancer -ServiceName $serviceName -InternalLoadBalancerName $ilbName -SubnetName "data" -StaticVNetIPAddress "$ilbIP"
        Write-Host "Created $ilbName"
        #Add add the thrift endpoint to the internal load balancer for all the VMs
        foreach($vmName in $vmNames)
        {
            Get-AzureVM -ServiceName $serviceName -Name $vmName |
                Add-AzureEndpoint -Name $thriftEndPoint -LBSetName "ThriftLBSet" -Protocol tcp -LocalPort 9160 -PublicPort 9160 -ProbePort 9160 -ProbeProtocol tcp -ProbeIntervalInSeconds 10 -InternalLoadBalancerName $ilbName |
                Update-AzureVM

            Write-Host "created $vmName"     
        }

**Samm 3: Konfigureerimine Cassandra iga VM**

Logige sisse VM ja tehke järgmist.

* Redigeerimiseks $CASS_HOME/conf/cassandra-rackdc.properties andmete keskmist ja sektsioon atribuutide määramiseks tehke järgmist.

       dc =EASTUS, rack =rack1

* Cassandra.yaml konfigureerimiseks seemne sõlmed allpool redigeerimiseks tehke järgmist.

       Seeds: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10"

**Samm 4: VMs käivitamine ja testimine klaster**

Logige sisse üks sõlmed (nt hk-c1-Lääne-us) ja käivitage järgmine käsk klaster oleku kuvamine.

       nodetool –h 10.1.2.4 –p 7199 status

Peaksite nägema umbes all on 8 sõlme kobar jaoks kuvatav:

<table>
<tr><th>Olek</th><th>Meiliaadress  </th><th>Laadi   </th><th>Märkide </th><th>Kuulub </th><th>Host ID  </th><th>Sektsioon</th></tr>
<tr><th>UN  </td><td>10.1.2.4   </td><td>87.81 KB   </td><td>256    </td><td>38,0%  </td><td>GUID (eemaldada)</td><td>rack1</td></tr>
<tr><th>UN  </td><td>10.1.2.5   </td><td>41.08 KB   </td><td>256    </td><td>68,9%  </td><td>GUID (eemaldada)</td><td>rack1</td></tr>
<tr><th>UN  </td><td>10.1.2.6   </td><td>55,29 KB   </td><td>256    </td><td>68,8%  </td><td>GUID (eemaldada)</td><td>rack2</td></tr>
<tr><th>UN  </td><td>10.1.2.7   </td><td>55,29 KB   </td><td>256    </td><td>68,8%  </td><td>GUID (eemaldada)</td><td>rack2</td></tr>
<tr><th>UN  </td><td>10.1.2.8   </td><td>55,29 KB   </td><td>256    </td><td>68,8%  </td><td>GUID (eemaldada)</td><td>rack3</td></tr>
<tr><th>UN  </td><td>10.1.2.9   </td><td>55,29 KB   </td><td>256    </td><td>68,8%  </td><td>GUID (eemaldada)</td><td>rack3</td></tr>
<tr><th>UN  </td><td>10.1.2.10  </td><td>55,29 KB   </td><td>256    </td><td>68,8%  </td><td>GUID (eemaldada)</td><td>rack4</td></tr>
<tr><th>UN  </td><td>10.1.2.11  </td><td>55,29 KB   </td><td>256    </td><td>68,8%  </td><td>GUID (eemaldada)</td><td>rack4</td></tr>
</table>

## <a name="test-the-single-region-cluster"></a>Testige ühe piirkonna kobar
Klaster testimiseks tehke järgmist:

1.    Kasutades Powershell käsku Get-AzureInternalLoadbalancer abil, saada sisemise koormuse koormusetasakaalustusteenuse IP-aadress (nt)  10.1.2.101). Käsu süntaks on allpool näidatud: Get-AzureLoadbalancer – teenuse nimi "hk-c-svc-Lääne-us" [kuvab sisemise koormuse koormusetasakaalustusteenuse koos IP-aadressi üksikasjad]
2.  Logige web serveripargi VM (nt hk-w1-Lääne-us) kasutades Putty või ssh
3.  Käivitada $CASS_HOME/alus/cqlsh 10.1.2.101 9160
4.  Järgmised CQL käskude abil saate kontrollida, kas klaster töötab:

        CREATE KEYSPACE customers_ks WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : 3 };
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');

        SELECT * FROM Customers;

Peaksite nägema ekraanita nagu allpool:

<table>
  <tr><th> customer_id </th><th> eesnimi </th><th> perekonnanimi </th></tr>
  <tr><td> 1 </td><td> John </td><td> Mägi </td></tr>
  <tr><td> 2 </td><td> Aile </td><td> Mägi </td></tr>
</table>

Pange tähele, et keyspace, 4 juhises loodud kasutab SimpleStrategy replication_factor 3. SimpleStrategy on soovitatav kasutada ühte keskmist juurutuste tuleks NetworkTopologyStrategy mitme andmete keskele juurutuste. Replication_factor 3 annab hälbe sõlm ebaõnnestumist.

##<a id="tworegion"> </a>Mitme piirkonna juurutamine
Kas kasutada ühe piirkonna juurutamise lõpetatud ja sama toimingut korrata, teises regioonis installimist. Ühe või mitme piirkonna juurutamise võtme vahe on VPN tunneliga setup vaheline piirkond teatis; Me alustamiseks network installimisega, ettevalmistamise VMs ja Cassandra konfigureerimine.

###<a name="step-1-create-the-virtual-network-at-the-2nd-region"></a>Samm 1: Luua virtuaalse võrgu piirkonna 2.
Azure'i klassikaline portaali sisse logida ja luua virtuaalse võrgu tabeli Kuva atribuudid. Üksikasjalikud juhised protsessi teemast [konfigureerimine Cloud-Only virtuaalse võrgu Azure klassikaline portaalis](../virtual-network/virtual-networks-create-vnet-classic-pportal.md) .      

<table>
<tr><th>Atribuudi nimi    </th><th>Väärtus    </th><th>Märkused</th></tr>
<tr><td>Nimi    </td><td>VNet-cass-Ida-us</td><td></td></tr>
<tr><td>Piirkond  </td><td>Ida-USA</td><td></td></tr>
<tr><td>DNS-serverid     </td><td></td><td>See ignoreerida, kuna me ei kasuta DNS-i Server</td></tr>
<tr><td>Punkti saidi VPN-i konfigureerimine</td><td></td><td>     Selle ignoreerimine</td></tr>
<tr><td>VPN saitide konfigureerimine</td><td></td><td>      Selle ignoreerimine</td></tr>
<tr><td>Aadressiruumi jaoks   </td><td>10.2.0.0/16</td><td></td></tr>
<tr><td>Alates IP </td><td>10.2.0.0   </td><td></td></tr>
<tr><td>CIDR    </td><td>/ 16 (65531)</td><td></td></tr>
</table>

Järgmised alamvõrku lisamiseks tehke järgmist.
<table>
<tr><th>Nimi    </th><th>Alates IP    </th><th>CIDR   </th><th>Märkused</th></tr>
<tr><td>Web </td><td>10.2.1.0   </td><td>/ 24 (251)  </td><td>Alamvõrgu web serveripargi</td></tr>
<tr><td>andmete    </td><td>10.2.2.0   </td><td>/ 24 (251)  </td><td>Andmebaasi sõlmed alamvõrgu</td></tr>
</table>


###<a name="step-2-create-local-networks"></a>Samm 2: Looge kohaliku võrgu
Azure virtuaalse võrgunduse kohalikus võrgu on puhverserveri aadressi ruumi, mis on serveri saidi, sh kohaselt või mõne muu Azure piirkonna kaardid. Kaugtöölaua lüüsi marsruutimise võrgu jaoks õige rakendus sihtkohta on seotud puhverserveri aadressi siia. Vaadake [konfigureerimine on VNet VNet ühendus](../vpn-gateway/virtual-networks-configure-vnet-to-vnet-connection.md) VNET-VNET ühenduse loomise juhised.

Looge kaks kohalikku võrkude kohta järgmisi üksikasju.

| Võrgu nimi | VPN lüüsi aadress | Aadressiruumi jaoks | Märkused |
| ------------ | ------------------- | ------------- | ------- |
| HK-lnet-Map-to-East-US | 23.1.1.1  | 10.2.0.0/16   | Kohalikus võrgus loomisel anna kohatäite lüüsi aadress. Reaal lüüsi aadress on täidetud pärast lüüsi loomist. Veenduge, et aadress ruumi vastab täpselt vastav remote VNET; Sel juhul on VNET loodud Ida-USA piirkonnas. |
| HK-lnet-Map-to-West-US | 23.2.2.2  | 10.1.0.0/16   | Kohalikus võrgus loomisel anna kohatäite lüüsi aadress. Reaal lüüsi aadress on täidetud pärast lüüsi loomist. Veenduge, et aadress ruumi vastab täpselt vastav remote VNET; Sel juhul on VNET loodud Lääne USA piirkonnas. |


###<a name="step-3-map-local-network-to-the-respective-vnets"></a>Samm 3: Kaardi "Kohalik" võrgu vastav VNETs
Azure'i klassikaline portaalis, valige iga vnet, "Konfigureerimine", märkige ruut "Ühenduse loomine kohaliku võrgu", ja valige kohaliku võrkude kohta järgmisi üksikasju.


| Virtuaalse võrgu | Kohalikus võrgus |
| --------------- | ------------- |
| HK-vnet-Lääne-us | HK-lnet-Map-to-East-US |
| HK-vnet-Ida-us | HK-lnet-Map-to-West-US |


###<a name="step-4-create-gateways-on-vnet1-and-vnet2"></a>Samm 4: Looge lüüside VNET1 ja VNET2
Klõpsake armatuurlaual mõlema virtuaalse võrgu loomine LÜÜSI, mis käivitab VPN-lüüsi ettevalmistamise protsess. Mõne minuti pärast iga virtuaalse võrgu Armatuurlaud peaks olema kuvatud tegelik lüüsi aadress.

###<a name="step-5-update-local-networks-with-the-respective-gateway-addresses"></a>Juhis 5: Värskendus "Kohalik" võrkude aadressidega vastav "Gateway"###
Saate redigeerida nii kohaliku võrkude kohatäite gateway IP-aadressi asendamiseks lihtsalt ettevalmistatud lüüside real IP-aadress. Kasutage järgmist vastendust.

<table>
<tr><th>Kohalikus võrgus    </th><th>Virtuaalse võrgu lüüsi</th></tr>
<tr><td>HK-lnet-Map-to-East-US </td><td>Lüüsi hk-vnet-Lääne-us.</td></tr>
<tr><td>HK-lnet-Map-to-West-US </td><td>Lüüsi hk-vnet-Ida-us.</td></tr>
</table>

###<a name="step-6-update-the-shared-key"></a>Samm 6: Värskendage ühiskasutuses võti
Kasutage järgmist PowerShelli skripti värskendada iga VPN-lüüsi [kasutamise huvides võti nii lüüside] IPSec võti: Set-AzureVNetGatewayKey - VNetName hk-vnet-Ida-us - LocalNetworkSiteName hk-lnet-map-to-west-us - SharedKey D9E76BKK Set-AzureVNetGatewayKey - VNetName hk-vnet-Lääne-us - LocalNetworkSiteName hk-lnet-map-to-east-us - SharedKey D9E76BKK

###<a name="step-7-establish-the-vnet-to-vnet-connection"></a>Juhis 7: Luua ühendus VNET-VNET
Azure'i klassikaline portaalist kasutada "ARMATUURLAUD" menüüs mõlema virtuaalse võrgu lüüsi gateway ühendust luua. Alumine tööriistariba käsud "CONNECT" kasutamine. Mõne minuti pärast Armatuurlaud peaks olema kuvatud ühenduse üksikasju graafiliselt.

###<a name="step-8-create-the-virtual-machines-in-region-2"></a>Samm 8: Looge selle virtuaalmasinates piirkonna #2
Luua Ubuntu pilt, nagu on kirjeldatud piirkonna #1 juurutamise järgides sama juhiseid või pildifaili VHD Azure storage konto asub piirkonna #2 Kopeeri ja luua pilt. Kasutage selle pildi ja virtuaalmasinates järgmises loendis luua uue kasutusele cloud hk-c-svc-Ida-us.


| Arvuti nimi | Alamvõrgu | IP-aadress | Kättesaadavus määramine | Näiteks Põhiliselt plaati | Seemne? |
| ------------ | ------ | ---------- | ---------------- | ------- | ----- |
| HK-c1-Ida-us | andmete  | 10.2.2.4   | HK-c-aset-1      | AV = EASTUS sektsioon = rack1 | Jah |
| HK-c2-Ida-us | andmete  | 10.2.2.5   | HK-c-aset-1      | AV = EASTUS sektsioon = rack1 | Ei  |
| HK-c3-Ida-us | andmete  | 10.2.2.6   | HK-c-aset-1      | AV = EASTUS sektsioon = rack2 | Jah |
| HK-c5-Ida-us | andmete  | 10.2.2.8   | HK-c-aset-2      | AV = EASTUS sektsioon = rack3 | Jah |
| HK-c6-Ida-us | andmete  | 10.2.2.9   | HK-c-aset-2      | AV = EASTUS sektsioon = rack3 | Ei  |
| HK-c7-Ida-us | andmete  | 10.2.2.10  | HK-c-aset-2      | AV = EASTUS sektsioon = rack4 | Jah |
| HK-c8-Ida-us | andmete  | 10.2.2.11  | HK-c-aset-2      | AV = EASTUS sektsioon = rack4 | Ei  |
| HK-w1-Ida-us | Web   | 10.2.1.4   | HK-w-aset-1      | N/A                    | N/A |
| HK-w2-Ida-us | Web   | 10.2.1.5   | HK-w-aset-1      | N/A                    | N/A |


Järgige sama nimega piirkonna #1, kuid 10.2.xxx.xxx aadressiruumi jaoks kasutada.

###<a name="step-9-configure-cassandra-on-each-vm"></a>Samm 9: Konfigureerimine Cassandra iga VM
Logige sisse VM ja tehke järgmist.

1. $CASS_HOME/conf/cassandra-rackdc.properties andmete keskmist ja sektsioon atribuutide määramiseks vormingus redigeerimine: AV = EASTUS sektsioon = rack1
2. Cassandra.yaml konfigureerida seemne sõlmed redigeerimine: seemned: "10.1.2.4,10.1.2.6,10.1.2.8,10.1.2.10,10.2.2.4,10.2.2.6,10.2.2.8,10.2.2.10"

###<a name="step-10-start-cassandra"></a>Samm 10: Start Cassandra
Logige sisse iga VM ja alustage Cassandra taustal, käivitage järgmine käsk: $ cassandra/CASS_HOME/alus

## <a name="test-the-multi-region-cluster"></a>Testige mitme piirkonna kobar
Nüüd on 16 sõlmed koos 8 sõlmed iga Azure'i piirkonnas kasutusele võetud Cassandra. Sõlmed on sama kobar levinud kobar nimi ja seemne sõlm konfiguratsiooni alusel. Klaster testimiseks kasutatakse järgmist protsessi:

###<a name="step-1-get-the-internal-load-balancer-ip-for-both-the-regions-using-powershell"></a>Samm 1: Saada sisemise koormuse koormusetasakaalustusteenuse IP PowerShelli kaudu piirkondade
- Get-AzureInternalLoadbalancer - teenuse nimi "hk-c-svc-Lääne-us"
- Get-AzureInternalLoadbalancer - teenuse nimi "hk-c-svc-Ida-us"  

    IP-aadresside Märkus (nt Lääne - 10.1.2.101 Ida - 10.2.2.101) kuvatakse.

###<a name="step-2-execute-the-following-in-the-west-region-after-logging-into-hk-w1-west-us"></a>Samm 2: Käivitada järgmine Lääne piirkonna logides hk-w1-Lääne-us
1.    Käivitada $CASS_HOME/alus/cqlsh 10.1.2.101 9160
2.  Käivita CQL järgmised käsud:

        CREATE KEYSPACE customers_ks
        WITH REPLICATION = { 'class' : 'NetworkToplogyStrategy', 'WESTUS' : 3, 'EASTUS' : 3};
        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

Peaksite nägema ekraanita nagu allpool:

| customer_id | eesnimi | Perekonnanimi |
| ----------- | --------- | -------- |
| 1           | John      | Mägi      |
| 2           | Aile      | Mägi      |


###<a name="step-3-execute-the-following-in-the-east-region-after-logging-into-hk-w1-east-us"></a>Samm 3: Käivitada pärast logige hk-w1-Ida-us Ida piirkonna järgmist:
1.    Käivitada $CASS_HOME/alus/cqlsh 10.2.2.101 9160
2.  Käivita CQL järgmised käsud:

        USE customers_ks;
        CREATE TABLE Customers(customer_id int PRIMARY KEY, firstname text, lastname text);
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES(1, 'John', 'Doe');
        INSERT INTO Customers(customer_id, firstname, lastname) VALUES (2, 'Jane', 'Doe');
        SELECT * FROM Customers;

Näha peaks olema sama kuva, nagu näha Lääne regiooni jaoks:


| customer_id | eesnimi | Perekonnanimi   |
|------------ | --------- | ---------- |
| 1           | John      | Mägi        |
| 2           | Aile      | Mägi        |


Veel mõned lisab käivitada ja leiate, et need saada kopeeritud Lääne-us osa klaster.

## <a name="test-cassandra-cluster-from-nodejs"></a>Testi Cassandra kobar Node.js kaudu
Kasutades ühte puuri "Web" Linux VMs taseme varem, me käivitada lihtsa Node.js skripti varem lisatud andmete lugemine

**Samm 1: Node.js ja Cassandra kliendi installimine**

1. Node.js ja npm installimine
2. Installige sõlm paketi "cassandra-klient" npm abil
3. Käivita järgmine skript shell vastav viip, mis kuvab json-stringi allalaaditud andmeid:

        var pooledCon = require('cassandra-client').PooledConnection;
        var ksName = "custsupport_ks";
        var cfName = "customers_cf";
        var hostList = ['internal_loadbalancer_ip:9160'];
        var ksConOptions = { hosts: hostList,
                             keyspace: ksName, use_bigints: false };

        function createKeyspace(callback){
           var cql = 'CREATE KEYSPACE ' + ksName + ' WITH strategy_class=SimpleStrategy AND strategy_options:replication_factor=1';
           var sysConOptions = { hosts: hostList,  
                                 keyspace: 'system', use_bigints: false };
           var con = new pooledCon(sysConOptions);
           con.execute(cql,[],function(err) {
           if (err) {
             console.log("Failed to create Keyspace: " + ksName);
             console.log(err);
           }
           else {
             console.log("Created Keyspace: " + ksName);
             callback(ksConOptions, populateCustomerData);
           }
           });
           con.shutdown();
        }

        function createColumnFamily(ksConOptions, callback){
          var params = ['customers_cf','custid','varint','custname',
                        'text','custaddress','text'];
          var cql = 'CREATE COLUMNFAMILY ? (? ? PRIMARY KEY,? ?, ? ?)';
        var con =  new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) {
                 console.log("Failed to create column family: " + params[0]);
                 console.log(err);
              }
              else {
                 console.log("Created column family: " + params[0]);
                 callback();
              }
          });
          con.shutdown();
        }

        //populate Data
        function populateCustomerData() {
           var params = ['John','Infinity Dr, TX', 1];
           updateCustomer(ksConOptions,params);

           params = ['Tom','Fermat Ln, WA', 2];
           updateCustomer(ksConOptions,params);
        }

        //update will also insert the record if none exists
        function updateCustomer(ksConOptions,params)
        {
          var cql = 'UPDATE customers_cf SET custname=?,custaddress=? where custid=?';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,params,function(err) {
              if (err) console.log(err);
              else console.log("Inserted customer : " + params[0]);
          });
          con.shutdown();
        }

        //read the two rows inserted above
        function readCustomer(ksConOptions)
        {
          var cql = 'SELECT * FROM customers_cf WHERE custid IN (1,2)';
          var con = new pooledCon(ksConOptions);
          con.execute(cql,[],function(err,rows) {
              if (err)
                 console.log(err);
              else
                 for (var i=0; i<rows.length; i++)
                    console.log(JSON.stringify(rows[i]));
            });
           con.shutdown();
        }

        //exectue the code
        createKeyspace(createColumnFamily);
        readCustomer(ksConOptions)


## <a name="conclusion"></a>Kokkuvõte
Microsoft Azure'i on paindlik platvorm, mis võimaldab töötab nii Microsoft kui ka avage, nagu see ülesanne. Väga kättesaadav Cassandra kogumite saab juurutada ühe andmekeskuse levitamine kobar sõlmed viga mitu domeenides kaudu. Cassandra kogumite saab saata ka mitu geograafiliselt kaugemal Azure piirkondade katastroofi tõend süsteemid. Azure'i ja Cassandra koos võimaldab ehitamine väga paindlik, mis on väga saadaval ja katastroofi Taastatavad pilveteenustega vaja tänase Interneti teel mastaapimiseks teenused.  

##<a name="references"></a>Viited##
- [http://Cassandra.Apache.org](http://cassandra.apache.org)
- [http://www.datastax.com](http://www.datastax.com)
- [http://www.nodejs.org](http://www.nodejs.org)
