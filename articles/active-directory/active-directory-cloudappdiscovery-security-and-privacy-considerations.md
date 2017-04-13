<properties
    pageTitle="Rakenduse Discovery turvalisus ja privaatsus kaalutlused Cloud | Microsoft Azure'i"
    description="Selles teemas kirjeldatakse pilve rakenduse Discovery seotud turvalisus ja privaatsus kaalutlused."
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="markusvi"/>

# <a name="cloud-app-discovery-security-and-privacy-considerations"></a>Pilveteenuse rakenduse Discovery turvalisus ja privaatsus kaalutlused

Microsoft soovib privaatsuse kaitsmine ja andmete turvamise, samas pakkuda tarkvara ja teenuseid, mis aitavad teil hallata ettevõtte turvalisus. <br>
Me tuvasta mis kui usaldate oma andmed teistele, et usalda nõuab range engineering muud ja teadmiste tagasi.
Microsoft järgib ranges ja turvalisuse juhised turvalise tarkvara arendamise elutsükli tavad teenuste kaudu. <br>
Ning andmed on Microsofti prioriteet.

See teema selgitab, kuidas andmed on kogutud, töödeldud, ja Azure Active Directory pilveteenuse rakenduse Discovery piiridesse




##<a name="overview"></a>Ülevaade

Pilveteenuse rakenduse Discovery on Azure AD ja seda majutab Microsoft Azure. <br>
Pilveteenuse rakenduse Discovery lõpp-punkti agent kasutatakse koguda andmeid selle hallatava masinad rakenduse tuvastamine. <br>
Kogutud andmete saadetakse turvaliselt üle krüptitud kanalit Azure AD pilveteenuse rakenduse Discovery teenus. <br>
Pilveteenuse rakenduse Discovery ettevõtte andmeid on seejärel Azure'i portaalis nähtav. <br>


<center>![Pilveteenuse rakenduse Discovery tööpõhimõte](./media/active-directory-cloudappdiscovery-security-and-privacy-considerations/cad01.png)</center> <br>


Järgmistes jaotistes teabe jälgimine ning kirjeldatakse, kuidas on tagatud, nagu see viib ettevõtte teenus Cloud rakenduse Discovery ja lõpuks pilveteenuse rakenduse Discovery portaali.



## <a name="collecting-data-from-your-organization"></a>Ettevõtte andmete kogumise

Selleks, et saada ülevaadet oma asutuse töötajatele rakendustesse Azure Active Directory pilve rakendus tuvastamise funktsiooni abil, peate esmalt juurutada Azure AD pilveteenuse rakenduse Discovery lõpp-punkti agent masinad teie asutuses.

Administraatorid rentniku Azure Active Directory (või nende volitatud esindaja) allalaadimise agent installipaketi Azure portaalist. Agent saate käsitsi installitud või mitme arvutites ettevõte SCCM või rühmapoliitika abil installitud.

Täpsemad juhised Juurutussuvandid, lugege teemat [Pilveteenuse rakenduse Discovery rühma poliitika juurutusjuhend](http://social.technet.microsoft.com/wiki/contents/articles/30965.cloud-app-discovery-group-policy-deployment-guide.aspx).
<br>

### <a name="data-collected-by-the-agent"></a>Kogutud agent

Alltoodud loendis kirjeldatud teavet kogub agent, kui ühendus luuakse veebirakenduses. Ainult nende rakenduste administraator on Discovery konfigureeritud kogutud teave. <br>
Saate redigeerida cloud rakenduste, mis agent jälgib kaudu pilveteenuse rakenduse Discovery tera Microsoft [Azure'i portaal](https://portal.azure.com/), klõpsake jaotises **sätted**loendi->**Andmete kogumine**->**rakenduse saidikogumi loendi**. Täpsema teabe saamiseks vt [Kasutamise alustamine koos pilveteenuse rakenduse Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)
<br>
**Teavet kategooria**: Kasutajateave <br>
**Kirjeldus**: <br>
Taotluse sihtkohta veebirakenduse protsessi Windowsi kasutajanimi (nt: DOMEEN\kasutajanimi) ning Windows turvalisus identifikaator (SID) kasutaja.


**Teavet kategooria**: teabe töötlemine <br>
**Kirjeldus**: <br>
Protsessi taotluse esitanud sihtkohta veebirakenduse nimi (nt: "iexplore.exe")

**Teavet kategooria**: masina teave <br>
**Kirjeldus**: <br>
Kuhu on installitud agent NetBIOS arvutinimi.

**Teavet kategooria**: rakenduse liikluse teave <br>
**Kirjeldus**: <br>

Ühenduse järgmine teave:

- Allikas (kohaliku arvuti) ja sihtkoha IP-aadressid ja pordinumbrid

- Avaliku IP-aadress organisatsiooni kaudu, mis läheb taotluse.

- Taotluse esitamise ajal

- Liiklus saadetud ja vastu võetud

- IP version (4 või 6)

- Ainult TLS-ühendused: target hostinimi Server Name märge pikendamise või Serveri sert.

HTTP järgmine teave:

- Meetod (GET POST)

- Protocol (HTTP/1.1 jne).

- Kasutajaagendi stringi

- Hostname (hostinimi)

- Sihtrakenduse URI (välja arvatud päringustringi)

- Sisutüübi teave

- Viitav URL-i teave (välja arvatud päringustringi)



> [AZURE.NOTE] Ülaltoodud HTTP teave kogutakse krüptimata kõik ühendused.
TLS ühendused, see teave on ainult jäädvustatud kui portaalis on sisse lülitatud 'Sügav kontrolli' säte. Vaikimisi on "ON".
Üksikasjad, vt allpool ja [Kasutamise alustamine koos pilveteenuse rakenduse Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)


Andmed, mida agent kogub võrgu tegevuse kohta, lisaks ka kogub anonüümset teavet tarkvara ja riistvara konfiguratsiooni, tõrketeated ja teave selle kohta, kuidas agent kasutatakse.

<br><br>
### <a name="how-the-agent-works"></a>Agent tööpõhimõtted

Agent installimine sisaldab kahest osast.

- Kasutaja-režiimis komponent

- Tuum-režiimis draiveri komponent (Windowsi filtreerimine platvormi juht)



Kui agent installiti talletab masina usaldusväärne sert arvutisse, mis kasutab seda siis luua turvalist ühendust teenusega pilveteenuse rakenduse Discovery. <br>
Agent toob teenus Cloud rakenduse Discovery poliitika konfiguratsiooni perioodiliselt üle selle turvalist ühendust. <br>
Poliitika sisaldab cloud taotluste jälgida ja seda, kas automaatne uuendamine peaks olema lubatud, muu hulgas teavet.

Nagu Web Liiklus on saadetud ja vastu võetud Internet Explorer ja Chrome'i arvutis, pilveteenuse rakenduse Discovery agent analüüsib liiklus ja ekstraktib oluline metaandmeid (vt ülal jaotist **agent koguda andmeid** ). <br>
Iga minut agent lisatud kogutud metaandmete teenus Cloud rakenduse Discovery üle krüptitud kanalit.

Draiveri komponent intercepts krüptitud liiklus ja lisab krüptitud voo. Järgmises jaotises **pealtkuulamist andmed krüptitud ühendused (sügav kontrolli)** rohkem üksikasju.


### <a name="respecting-user-privacy"></a>Järgides kasutaja Privaatsus

Meie eesmärk on pakkuda administraatorid tööriistad määramiseks saldo vahel üksikasjalik optika rakenduse kasutamise ja kasutajale Privaatsus vastavalt oma asutuse jaoks sisse. Selleks pakume järgmised nupud portaalis lehel sätted:

- **Andmete kogumine**: administraatorid korral saate määrata, millised rakendused või rakenduste kategooriad soovidega arvestamine discovery andmete toomine.

- **Sügav kontrolli**: administraatorid saate valida, kui agent kogub HTTP-liikluse SSL/TLS ühenduste (nimega **"sügav kontrolli"**) määramiseks. Lisateavet selle järgmises jaotises.

- **Nõusolek suvandid**: administraatorid saavad pilveteenuse rakenduse Discovery portaali abil valida, kas soovite kasutajate teavitamis agent andmekogumise ja kas nõua kasutajalt nõusoleku enne selle esindaja algab kasutaja andmete kogumine.

Pilveteenuse rakenduse Discovery lõpp-punkti agent kogub ainult ülaltoodud **andmete kogutud agent** jaotises kirjeldatud teave.


### <a name="intercepting-data-from-encrypted-connections-deep-inspection"></a>Tunnistava andmete krüptitud ühendused (sügav kontrolli)
Nagu varem mainitud, saavad administraatorid konfigureerida agent jälgida andmeid krüptitud ühendused (sügav kontrolli). TLS ([Transpordikihi Turve](https://msdn.microsoft.com/library/windows/desktop/aa380516%28v=vs.85%29.aspx)) on üks kõige levinum Protokollid Kasuta Internetis täna. Suhtlus TLS krüptides luua klient ja privaatsuse tagamine side kanali veebiserverisse; TLS annab oluline kaitse autentimine mandaadi ja vältida tundliku teabe avaldamist.

Kuigi lõpuni – turvaline krüptitud kanali esitatud TLS võimaldab olulised turvalisuse ja privaatsuse kaitsmine, protokoll tihti pahatahtlik või õelate eesmärgil ära. Nii palju, nii tegelikult, et TLS sageli nimetatakse "ühes kohas tulemüüri-meediumierandi protokoll". Probleemi tuum on, et enamik tulemüürid ei saa kontrollida TLS side, kuna rakenduse kiht andmed on krüptitud SSL-i. See teab, ründajad sageli kasutada TLS-ile osutamiseks pahatahtlik lõhkelaengu kindel, et isegi kõige nutikas rakenduse kiht tulemüürid on täielikult pimedad TLS ja tuleb lihtsalt relee TLS suhtlemine hosts kasutajale. Lõppkasutajad sageli kasutada TLS mööda hiilida Accessi juhtelemendid jõustatud oma ettevõtte tulemüür ja puhverserverid, abil ühenduse avaliku puhverserverid ja tunneling-TLS Protokollid läbi tulemüüri, mis muidu võib olla blokeeritud rühmapoliitika abil.

Sügav kontroll võimaldab pilveteenuse rakenduse Discovery agent on usaldusväärne mees-sisse--Lähis tegutseda. Kui kliendi nõude juurdepääsuks on kaitstud HTTPS ressurss, lõpp-punkti Agent draiver intercepts ühendus ja loob uue ühenduse sihtkoha server toob selle SSL-sert, saate kliendi nimel. Agent seejärel kinnitab, et serdi saab usaldada (kontrollimine, et see ei tühistatud ja muud serdi kontrollid läbimiseks), ja kui nende pass, siis lõpp-punkti Agent kopeerib teabe serveri serti ja loob oma serveri serdi--tuntud sertifikaadiga pealtkuulamine--kasutada neid andmeid. Pealtkuulamine sert on allkirjastatud sisse-fly lõpp-punkti agent juurkausta sertifikaadiga, mis on installitud Windowsi poest usaldusväärne sert. Iseallkirjastatud serdi märgitud eksporditav ja on ACL oli administraatorid. See on ette nähtud jäta arvutisse, mis on loodud. Lõppkasutaja klientrakendusega saab pealtkuulamine sert, kuvatakse see usalda seda kuna see valideerib edukalt serdi ahelas kuni juurkausta serdi. See toiming on enamasti läbipaistev-lõppkasutaja seisukohast mõne piirangutega, nagu allpool kirjeldatud.

Võimaldades sügav kontrolli, saate pilveteenuse rakenduse Discovery lõpp-punkti Agent dekrüptimine ja kontrolli TLS krüptitud suhtlus lubamisel teenuse müra ja esitage ülevaateid krüptitud cloud rakenduste kasutamise kohta.

#### <a name="a-word-of-caution"></a>Sõna hoiatused
Enne sisselülitamist sügav kontrolli, tungivalt soovitatav, et teie kavatsused legal ja HR osakondade suhelda ja nende nõusolekut. Lõppkasutaja isikliku krüptitud suhtlemise kontrollimise saab tundliku teema, selge põhjustel. Enne on tootmise välja sügav kontrolli, veenduge, et teie ettevõtte turvalisus ja aktsepteeritav kasutamise põhimõtted värskendatud krüptitud side kontrollitakse näitamiseks. Kui konfigureerite pilveteenuse rakenduse Discovery nende jälgimiseks vajalikud samuti võib kasutaja teatise ja maksuvabastuse saitide loetakse tundliku loomuga (nt panga- ja meditsiiniline saidid). Eespool nimetatud administraatorid saavad kasutada pilveteenuse rakenduse Discovery portaalis valige, kas kasutajate teavitamis agent andmekogumise ja kas soovite nõuda enne esindaja algab kasutaja andmete kogumise kasutaja nõusolekuta.

### <a name="known-issues-and-drawbacks"></a>Teadaolevad probleemid ja puudused
On mõnel juhul, kui TLS pealtkuulamine võib mõjutada lõppkasutaja kogemuse kohta.

- Laiendatud valideerimise (EV) sertide renderdada veebibrauseri aadressiribale roheline visuaalse aimu, et külastatavate veebisaidina tegutseda. TLS kontrolli ei saa dubleerida EV selle probleemid kliendile nii EV serdid kasutavad veebisaidid töötavad tavaliselt märgitud, kuid ei kuvata aadressiriba roheline.  

- Avalik võti kinnitamine (tuntud ka kui serdi kinnitamine) on mõeldud aitab kaitsta kasutajate eest mees-in-Lähis ja petturitest sertimiskeskuste. Kui kinnitatud saidi juursait serti ei sobi ühegi teadaolevad hea CA, brauseri hülgab ühenduse viga. Kuna TLS pealtkuulamine on tegelikult on mees-sisse--Lähis, nurjub nende ühendused.

- Kui kasutajad klõpsake lukuikoon kontrollimiseks saidi teave brauseris brauseri aadressi riba, ei näe nad lõpus on kasutatud veebisait allkirjastamiseks sertimisorgan ahelas, kuid selle asemel, mille nimi lõpeb Windowsi serdi ahelas usaldusväärsed salv.

Järgmiste probleemide korral esinemiskordade vähendamiseks meil ja silma peal hoida pilveteenustega klientrakendustes teada, et kasutada laiendatud valideerimise või avalik võti kinnitamine ja teha lõpp-punkti esindajale vältimiseks pealtkuulamist kannatavas ühendused. Isegi sellisel juhul siiski kuvatakse need cloud rakenduste ja üle andmete maht, kuid kuna nad ei ole sügav kontrollida, rakenduste kasutamise kohta üksikasju on saadaval.


## <a name="sending-data-to-cloud-app-discovery"></a>Pilveteenuse rakenduse Discovery andmete saatmine

Kui metaandmete kogutud agent, see on vahemällu seadmesse üheks minutiks või vahemällu talletatud andmetega jõuab suurus 5 MB. See on tihendatud ja edastatavate pilveteenuse rakenduse Discovery teenusega turvalist ühendust.

Kui agent ei saa suhelda mingil põhjusel teenus Cloud rakenduse Discovery, talletatakse kogutud metaandmete kohaliku faili vahemälu, millele pääsevad juurde ainult õigustega kasutaja arvutis (nt administraatorite rühma). <br>
Agent proovib uuesti vahemällu talletatud metaandmeid, kuni see on edukalt saanud teenus Cloud rakenduse Discovery.



## <a name="receiving-the-data-at-the-service-end"></a>Andmete teenuse lõpus

Pilveteenuse rakenduse Discovery abil arvutisse teatud kliendi autentimissert viidatud kohal ja edastab andmed üle krüptitud kanalit agentide autentida. <br>
Pilveteenuse rakenduse Discovery teenuse analytics müügivõimaluste töötleb metaandmete iga kliendi eraldi, loogiliselt eraldamine see kõigil analytics kohaletoimetamisel.
Analüüsitud metaandmete draivid portaalis mitmesuguseid aruandeid.

Töötlemata metaandmete ja analüüsitud metaandmete salvestatakse kuni 180 päeva. Lisaks saate valida kliendid jäädvustada analüüsitud metaandmete konto Azure'i bloobimälu salvestusruumi nende valitud.
See on abiks ühenduseta analüüs metaandmete samuti kauem andmete säilitamise.

## <a name="accessing-the-data-using-the-azure-portal"></a>Azure'i portaalis andmetele juurdepääsu

Selleks, et hoida turvaline kogutud metaandmeid, vaikimisi ainult globaalse administraatorid rentniku juurdepääs pilveteenuse rakenduse tuvastamise funktsiooni Azure'i portaalis. <br>
Administraatorid saate valida volitatud esindaja sellele juurdepääsu muudele kasutajatele või rühmadele.



> [AZURE.NOTE] Täpsema teabe saamiseks vt [Kasutamise alustamine koos pilveteenuse rakenduse Discovery](http://social.technet.microsoft.com/wiki/contents/articles/30962.getting-started-with-cloud-app-discovery.aspx)

<br>
Mis tahes kasutaja juurdepääs portaalis andmete peavad olema litsentsitud Azure AD Premium litsentsiga.



##<a name="additional-resources"></a>Lisaressursid


* [Kuidas saab oma ettevõttes töötavaid Ülikooli pilve rakendused, mida kasutatakse leida](active-directory-cloudappdiscovery-whatis.md)
* [Artikli registri Rakendusehaldus Azure Active Directory 's](active-directory-apps-index.md)
