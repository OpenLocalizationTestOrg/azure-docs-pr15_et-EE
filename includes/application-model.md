# <a name="compute"></a>Arvuta

Azure'i võimaldab teil juurutada ja kontrollida oma rakenduse koodi töötab Microsofti andmekeskuses sees. Kui kasutate rakenduse loomine ja käivitamine Azure, koodi ja konfiguratsiooni koos nimetatakse ka Azure majutatud teenus. Majutatud teenused on lihtne hallata, skaala üles ja alla, konfigureerida ja värskendada oma rakenduse koodi uue versioonidega. See artikkel keskendub Azure'i, mis on majutatud teenuse mudel.<a id="compare" name="compare"></a>

## Sisukord<a id="_GoBack" name="_GoBack"></a>

-   [Azure'i rakenduse mudeli eelised][]
-   [Majutatud teenuse keskendudes][]
-   [Majutatud teenuse kujundamine][]
-   [Teie taotlus skaala kujundamine][]
-   [Majutatud teenuse määratlus ja konfigureerimine][]
-   [Teenuse definitsioonifail][]
-   [Teenuse konfiguratsioonifail][]
-   [Loomine ja juurutamine majutatud teenus][]
-   [Viited][]

## <a id="benefits"> </a>Azure rakenduse mudeli eelised

Rakenduse majutatud teenuse juurutamisel Azure'i loob ühe või mitme virtuaalmasinates (VM) oma rakenduse koodi sisaldavate ja saapad VMs Azure'i andmekeskuste elavate füüsilise arvutites. Klientide päringutele majutatud rakenduse sisestada andmekeskuse, laadi koormusetasakaalustusteenuse jaotab taotlused võrdselt VMs. Samal ajal, kui teie taotlus on majutatud Azure, läheb kolm peamised eelised:

-   **Kõrge-saadavus.** Kõrge-saadavus tähendab, et Azure'i tagab, et teie rakendus töötab võimalikult palju ja kliendi taotlustele vastata. Kui teie taotlus lõpeb (töötlemata erandi, näiteks), siis Azure tuvastab see ja see automaatselt uuesti alustada rakenduse. Kui teie rakendus töötab seadme kogemusi mingi riistvara nurjumine, siis Azure ka selle avastamiseks ja automaatselt luua uue VM teise töötamise füüsilise masina ja seal oma koodi käivitada. Märkus: Selleks, et saada Microsofti teenuselepingu 99,95% saadaval rakenduse, peab olema vähemalt kaks VMs töötab teie rakenduse koodi. See võimaldab ühe VM töödelda kliendi taotlusi ajal Azure'i liigub oma koodi nurjunud VM uus, hea VM.

-   **Skaleeritavus.** Azure'i saab hõlpsasti ja dünaamiliselt VMs töötab teie rakenduse koodi tegeliku laadi rakenduse asetatakse käsitlema arvu muutmine. See võimaldab teil kohandada oma rakenduse töökoormus, mis kliendid asetavad seda pöörates ainult jaoks VMs, peate, kui neid vajate. Kui soovite muuta VMs arvu, vastab Azure'i võimaldab dünaamiliselt muuta VMs töötab nii tihti kui soovitud arvu minuti jooksul.

-   **Hallatavust.** Kuna Azure on teenus (PaaS), mis pakub platvormi, seda haldab hoida neid seadmetes, kus töötab vajalikku infrastruktuuri (riistvara ise, elektri ja võrk). Azure'i haldab ka platvormi, tagades ajakohast operatsioonisüsteemi kõik on õige plaastrid ja turbevärskendusi, samuti nagu .NET Frameworki ja Internet Information Serveri komponendi värskendused. Kuna kõik VMs arvutis töötab opsüsteem Windows Server 2008, leiate Azure'i lisavõimalusi, nt diagnostika jälgimine, kaugtöölaua tugi, tulemüürid ja sertifikaadi poe konfigureerimine. Kõik need funktsioonid on esitatud ilma lisatasu. Tegelikult rakenduse käivitamisel Azure'i Windows Server 2008 litsentsi operatsioonisüsteem (OS) on kaasatud. Kuna kõik VMs arvutis töötab opsüsteem Windows Server 2008, mis tahes kood, kus töötab Windows Server 2008 töötab hästi Azure käivitamisel.

## <a id="concepts"> </a>Majutatud teenuse keskendudes

Rakenduse juurutamisel hostitud teenuste Azure see töötab ühe või mitme *rollid.* *Roll* viitab lihtsalt rakenduse faile ja konfigureerimine. Saate määratleda ühe või mitu rolli rakenduse, igal versioonil oma komplekti rakenduse faile ja konfigureerimine. Iga rolli oma rakenduse, saate määrata käivitamiseks VMs või *rolli eksemplaride*arv. Joonis kuvada kaks lihtsat näidet rakenduse modelleerida nagu majutatud teenuse kasutamise rollid ja rolli aknad.

##### <a name="figure-1-a-single-role-with-three-instances-vms-running-in-an-azure-data-center"></a>Joonis 1: Kindla rolliga koos töötavate Azure'i andmed keskuses kolm eksemplaride (VM)

![pilt][0]

##### <a name="figure-2-two-roles-each-with-two-instances-vms-running-in-an-azure-data-center"></a>Joonis 2: Kahe rollid, iga kahel (VM), kus töötab keskuses Azure'i andmed

![pilt][1]

Tavaliselt protsessi rolli aknad Interneti-päringud kliendi sisestamise andmekeskuse kaudu, mida nimetatakse *Sisestuskeel lõpp-punkti*. Kindla rolliga võib olla 0 või rohkem Sisestuskeel lõpp-punktid. Iga lõpp-punkti näitab protokolli (HTTP, HTTPS või TCP) ja pordi. On tavaline, et konfigureerida roll, on kaks Sisestuskeel lõpp-punktid: HTTP listening pordi 80 ja pordi 443 kuulamise HTTPS. Joonisel on kujutatud kahe erinevad rollid ja suunavad klientide päringutele neile erinevate Sisestuskeel lõpp-punktid.

![pilt][2]

Azure'i majutatud teenuse loomisel see on määratud kliendid saavad kasutada seda juurdepääsu avalikult adresseeritavad IP-aadress. Majutatud teenuse loomisel peate valima ka URL-i eesliide, mis on vastendatud IP-aadress. See eesliide peab olema kordumatu, kui teil on põhiosas broneerimist *eesliite*. cloudapp.net URL-i, nii et keegi teine võib olla see. Klientide suhelda oma rolli aknad URL-i abil. Tavaliselt, siis ei levitamine või avaldamine Azure *eesliite*. cloudapp.net URL-i. Selle asemel saate osta DNS-i nimi oma DNS-i domeeniregistraatori valik ja oma DNS-i nimi klientide päringutele ümbersuunamiseks Azure'i URL-i konfigureerimine. Lisateavet leiate teemast [konfigureerida kohandatud domeeninime Azure][].

## <a id="considerations"> </a>Majutatud teenuse kujundamine

Rakenduse käivitamiseks cloud keskkonnas kujundamisel on mitu kaalutlused mõelda, nt latentsus, kõrge-saadavus ja skaleeritavus.

Otsustate määrata asukoha rakenduse koodis on oluline, kui installitud majutatud teenuse Azure. See on levinud andmekeskuste, mis on kõige lähemal kliendid vähendada latentsus ja parimaid tulemusi saada võimalik rakenduse juurutamine.
Samas võite andmekeskuse lähemale oma ettevõtte või andmete lähemal kui teil on mõni jurisdiktsiooni või juriidilisi muret andmete ja kus see asub. Kuus andmekeskuste kogu maailmas on võimalik majutada oma rakenduse koodi. Järgmises tabelis antakse ülevaade asukoht saadaolevate asukohtade:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr>
<td style="width: 100px;">
**Riik/regioon**

</td>
<td style="width: 200px;">
**Alamtäpi piirkonnad**

</td>
</tr>
<tr>
<td>
Ameerika Ühendriigid

</td>
<td>
Lõuna-, Kesk- ja Põhja Kesk

</td>
</tr>
<tr>
<td>
Euroopa

</td>
<td>
Põhja- ja Lääne

</td>
</tr>
<tr>
<td>
Asia

</td>
<td>
Kodutee ja Ida

</td>
</tr>
</tbody>
</table>
Majutatud teenuse loomisel valite sub piirkonnas, mis näitab, asukoht, kuhu soovite oma koodi käivitada.

Kõrge-saadavus ja skaleeritavus saavutamiseks on äärmiselt oluline, rakenduse andmeid hoida keskses hoidlas kättesaadavaks rolli mitmes eksemplaris. Azure'i pakub aidata seda, mitu talletamise võimalused, nt plekid, tabelite ja SQL-andmebaasi. Lugege [Andmete salvestusruumi pakkumisi Azure][] mobiilsidevõrgu salvestusruumi kohta lisateavet. Allpool joonisel on näidatud, kuidas laadi koormusetasakaalustusteenuse sees Azure andmekeskuse jaotab kliendi taotluste roll eksemplarid, mis on sama andmesalv juurdepääs.

![pilt][3]

Tavaliselt, kui soovite leida oma rakenduse koodi ja andmete sama andmekeskuse, nagu see võimaldab madal latentsus (parema jõudluse) kui rakenduse koodis kasutab andmeid. Lisaks pole ostmisega läbilaskevõime kui sama andmekeskuse on andmed paigutada.

## <a id="scale"> </a>Teie taotlus skaala kujundamine

Mõnikord, mida soovite teha ühes rakenduses (nt lihtne veebilehel) ja on see majutatud Azure. Vaid sageli, rakenduse võib koosneda mitu rollid, et kõik koos töötada. Näiteks alloleval joonisel on kaks eksemplari veebisaidi roll, kolm eksemplari tellimuse töötlemine roll ja ühe eksemplari aruande generaator roll. Administraatorirollid kõik töötavad koos ja koodi kõik need saab kokku pakitud ja juurutatud kuni Azure'i ühtse üksusena.

![pilt][4]

Tükeldamiseks rakenduse sisse erinevad rollid iga rolli aknad (ehk teisisõnu öeldes VM) oma komplekti töötavate põhjuseks on skaala rollid sõltumatult. Näiteks pühade ajal paljud kliendid võivad saab osta tooted teie ettevõtte töötajad nii, et võiksite suurendamiseks töötavate oma veebisaidi rolli kui ka töötavate oma tellimuse töötlemine rolli rolli eksemplaride arv rolli eksemplaride arv. Pärast pühade, võite saada palju tooteid tagastatud, nii, et peate palju veebisaidi eksemplari, kuid vähem tellimuse töötlemine eksemplarid. Ajal ülejäänud aasta, peate vaid mõned veebisaidi ja tellimuse töötlemine eksemplarid. Kogu kõik, peate ainult ühe aruande generaator eksemplari. Rollipõhine juurutuste Azure paindlikkust võimaldab teil kohandada rakenduse teie ettevõtte vajadustele.

See on levinud roll eksemplaris oma majutatud teenuses omavahel suhelda. Näiteks veebisaidi roll aktsepteerib kliendi tellimuse, kuid seejärel seda offloads tellimuse töötlemise tellimuse töötlemine rolli aknad. Parim viis edasi töö vormi üks kogum rolli eksemplarid eksemplaride teise kogumisse kasutab Azure, mida andmebaasitõrge tehnoloogia järjekorda teenuse või teenuse siini järjekorrad. Järjekorda kasutamine on oluline osa lugu siin. Järjekorda võimaldab majutatud teenuse mastaapimiseks oma rollid sõltumatult võimaldab töökoormus kulu tasakaalustamiseks. Kui järjekorda sõnumite arv suureneb aja jooksul, siis saate skaalal tellimuse töötlemine rolli eksemplaride arv. Kui järjekorda sõnumite arv väheneb aja jooksul, siis te saate skaala alla tellimuse töötlemine rolli eksemplaride arv. Sellisel viisil olete ainult maksab eksemplarid, mis on nõutav käsitlema tegelik töökoormus.

Kuhjuda pakub töökindlus. Kui skaala tellimuse töötlemine rolli eksemplaride arv allapoole Azure'i otsustab millised eksemplarid lõpetada. See võib otsustada lõpetada eksemplari, mis on keskel järjekorda sõnumi töötlemine. Siiski, kuna sõnumi töötlemine lõpule viia, sõnumi muutub nähtavaks uuesti teise tellimuse töötlemine rolli näiteks, et see jätkab ja töötleb. Tõttu kuhjuda sõnumi nähtavus sõnumid on tagatud lõpuks töödeldav. Kuhjuda toimib ka laadi koormusetasakaalustusteenuse mõjuv jagades selle rolli kõik eksemplarid, mis seda taotleda sõnumite sõnumeid.

Veebisaidi rolli aknad, saate jälgida tulevad need liiklust ja soovi mastaapimiseks arvu need üles või alla ka. Järjekorda võimaldab mastaapimiseks veebisaidi rolli aknad sõltumatult tellimuse töötlemine rolli eksemplaride arv. See on väga võimas ja pakub hulgaliselt võimalusi. Muidugi, kui rakenduse koosneb täiendavad rollid, saate lisada täiendavad järjekorrad nimega üksuse suhtlemiseks kasutada sama skaleerimist ja maksumus eeliste nende vahel.

## <a id="defandcfg"> </a>Majutatud teenuse määratlus ja konfigureerimine

Azure'i hostitud teenuste juurutamine peab teil on olemas ka teenuse määratluse faili ja teenuse konfigureerimise faili. Mõlemad failid on XML-failid ja nende abil saab määrata deklaratiivse Juurutussuvandid teie majutatud teenuse jaoks. Teenuse definitsioonifail kirjeldatakse kõik rollid, mis moodustavad majutatud teenust ja kuidas need suhelda. Teenuse konfiguratsioonifail kirjeldatakse iga rolli ja sätete konfigureerimine iga rolli eksemplari eksemplaride arv.

## <a id="def"> </a>Teenuse definitsioonifail

Nagu ma varem mainitud, teenuse määratlus (CSDEF) fail on XML-fail, mis kirjeldab erinevad rollid, mis moodustavad automaatsele. Täieliku skeemi XML-faili saate siit: [[http://msdn.microsoft.com/library/windowsazure/ee758711.aspx]].
CSDEF fail sisaldab WebRole või WorkerRole elemendi iga rolli, mida soovite oma rakenduse. Juurutamine rolli web rolli (abil WebRole elemendi) tähendab, et kood kestab rolli eksemplari, mis sisaldab Windows Server 2008 ja Internet Information Server (IIS).
Juurutamine rollid, nagu töötaja roll (abil WorkerRole elemendi) tähendab, et rolli eksemplari on Windows Server 2008 seda (IIS-i ei installita).

Kindlasti saate luua ja juurutada töötaja rolli, mis kasutab mõnda muud süsteemi kuulata web sissetulevad taotlused (nt oma koodi võib loomine ja kasutamine .NET HttpListener). Kuna rolli aknad kõik opsüsteemi Windows Server 2008, mis tahes rakenduse käivitamine Windows serveris tavaliselt saadaolevaid toiminguid teha oma kood
2008.

Iga rolli näitate esinemisjuhud rolli tuleks kasutada soovitud VM suurusega. Järgmises tabelis antakse ülevaade eri VM suurused saadaval täna ja iga atribuute:

<table border="2" cellspacing="0" cellpadding="5" style="border: 2px solid #000000;">
<tbody>
<tr align="left" valign="top">
<td>
**VM suurus**

</td>
<td>
**CPU**

</td>
<td>
**RAM-I**

</td>
<td>
**Ketas**

</td>
<td>
**Tipp võrgu i/o**

</td>
</tr>
<tr align="left" valign="top">
<td>
**Täiendav väike**

</td>
<td>
1 x 1.0 GHz

</td>
<td>
768 MB

</td>
<td>
20GB

</td>
<td>
\~5 Mbps

</td>
</tr>
<tr align="left" valign="top">
<td>
**Small**

</td>
<td>
1 x 1,6 GHz

</td>
<td>
1,75 GB

</td>
<td>
225GB

</td>
<td>
\~100 Mbps

</td>
</tr>
<tr align="left" valign="top">
<td>
**Keskmine**

</td>
<td>
2 x 1,6 GHz

</td>
<td>
3,5 GB

</td>
<td>
490GB

</td>
<td>
\~200 Mbps

</td>
</tr>
<tr align="left" valign="top">
<td>
**Suure**

</td>
<td>
4 x 1,6 GHz

</td>
<td>
7 GB

</td>
<td>
1TB

</td>
<td>
\~400 Mbps

</td>
</tr>
<tr align="left" valign="top">
<td>
**Suurtele**

</td>
<td>
8 x 1,6 GHz

</td>
<td>
14 GB

</td>
<td>
2TB

</td>
<td>
\~800 Mbps

</td>
</tr>
</tbody>
</table>
Ostmisega tunni iga VM kasutate rolli eksemplar ja ka ostmisega andmeid väljaspool andmekeskuse saata oma rolli aknad. Te ei võeta andmete sisestamise data Centeri kaudu. Lisateavet leiate teemast [Azure'i hinnad] []. Üldiselt on soovitatav kasutada paljude väike roll eksemplari asemel mõne suur juhtudel, et teie taotlus on paindlikumaks tõrge. Ju vähem rolli aknad, mida teil on, rohkem katastroofilised tõrke üks neist on teie üldist rakenduse. Ka nagu enne mainitud, on teil selleks, et saada 99,95% taseme hooldusleppe Microsoft osutab selle juurutamine vähemalt kaks eksemplari iga rolli.

Teenuse definitsioonifail (CSDEF) on ka see, kui määrate palju atribuute iga rolli kohta oma rakenduse. Siin on mõned rohkem kasu üksused saadaval:

-   **Serdid**. Kasutage krüptimiseks andmeid või kui teie veebiteenuse toetab SSL-i serdid. Mis tahes serdid on vaja Azure üles laadida. Lisateabe saamiseks lugege teemat [Azure sertide haldamine][]. Selle XML-i sätte installib eelnevalt üles laaditud serdid rolli eksemplari serdi salvestuskohta, et need oleksid kasutatavaks oma rakenduse koodi.

-   **Otsingukonfiguratsiooni säte nimed**. Jaoks väärtused, mida soovite lugeda rolli eksemplari töötab teie rakendused. Tegelik väärtus konfiguratsioonisätete asub teenuse konfigureerimine (CSCFG) faili, mida saate igal ajal värskendada ilma peaksite Juurutage uuesti oma kood. Tegelikult, saate koodi rakenduste nii tuvastamiseks ilma, et tekiks mis tahes muudetud konfiguratsiooni väärtused.

-   **Sisestusmeetodi lõpp-punktid**. Siin saate määrata mis tahes HTTP, HTTPS või TCP lõpp-punktid (portidega) välise maailma kaudu oma *eesliite*seada soovitud. cloadapp.net URL-i. Kui Azure kasutab oma rolli, see tuleb konfigureerida tulemüüri rolli eksemplari automaatselt.

-   **Sisemise lõpp-punktid**. Siin saate määrata mis tahes HTTP või TCP lõpp-punktid soovitud esitatud muude rolli eksemplaridesse juurutatud rakenduse osana. Sisemise lõpp-punktid luba kõik rolli aknad rakenduse rääkida üksteise sees, kuid ei pääse esinemisvõimaluse rolli, mis on väljaspool teie rakendus.

-   **Impordi moodulid**. Soovi korral installida need kasulik komponendid oma rolli aknad. Komponendid olemas diagnostika järelevalve Remote'i töölaua ja Azure ühenduse (mis lubab juurdepääsu kohapealse ressursid turvalise kanali kaudu oma rolli eksemplari).

-   **Kohalik salvestusruum**. See eraldab alamkausta, kasutada rakenduse eksemplari roll. See on täpsemalt [Andmete salvestusruumi pakkumisi Azure][] artiklis kirjeldatud.

-   **Käivitus tööülesanded**. Käivitus tööülesannete teile võimalus eelnevalt nõutud komponentide installimine rolli eksemplari, nagu see käivitub. Tööülesanded saate käivitada administraatorina vajadusel administraatoriõigustes.

## <a id="cfg"> </a>Teenuse konfiguratsioonifail

Teenuse konfiguratsioonifail (CSCFG) on sätted, mida saate muuta ilma ümbersuunamine rakenduse kirjeldav XML-faili. Täieliku skeemi XML-faili saate siit: [http://msdn.microsoft.com/library/windowsazure/ee758710.aspx][].
CSCFG fail sisaldab rolli elemendi iga rolli oma rakenduse. Siin on toodud mõned üksused, saate määrata CSCFG faili.

-   **Opsüsteemi versioon**. See atribuut võimaldab valida soovitud operatsioonisüsteem (OS) versiooni rolli aknad töötavad rakenduse koodis kasutada. See OS nimetatakse *Külastajate OS*ja iga uus versioon sisaldab selle uusima turvalisus parandused ja värskendused, mis on saadaval Külastajate OS on vabastatud ajal. Kui määrate atribuudi osVersion väärtus "\*", siis Azure automaatselt värskendab Külastajate OS iga rolli eksemplaride uus külaline OS versioon on saadaval. Siiski saate loobuda automaatvärskendused, valides teatud Külastajate Opsüsteemi versioon. Näiteks, milles osVersion atribuudi väärtuseks "WA Külastajate-OS 2,8\_201109-01" põhjustab teie roll läbivalt kogu saada, mida on kirjeldatud selles veebilehele: [http://msdn.microsoft.com/library/hh560567.aspx][]. Külastajate OS versiooni kohta leiate lisateavet teemast [Azure külalistele OS täienduste haldamine].

-   **Eksemplarid**. – See element väärtus näitab rolli aknad, mida soovite ettevalmistatud töötab kindla rolliga kood arvu. Kuna Azure saate uue CSCFG faili üleslaadimine (ilma ümbersuunamine rakenduse), on juba lihtne muuta selle elemendi väärtuse ja dünaamiliselt suurendamiseks või vähendamiseks töötavate rakenduse koodis rolli eksemplaride arv CSCFG uue faili üles laadida. See võimaldab teil hõlpsasti mastaapimiseks rakenduse üles või alla vastavad tegelik töökoormus nõudmistele, samas kui ka kontrolli, kui palju ostmisega töötavad rolli eksemplarid.

-   **Otsingukonfiguratsiooni säte väärtused**. – See element näitab väärtused sätete (CSDEF failis määratletud). Teie roll saate lugeda nende väärtuste töötamise ajal. Ühendusstringi SQL-andmebaasi või Azure Storage kasutatakse tavaliselt väärtused konfiguratsiooni, kuid neid saab kasutada mis tahes eesmärgil, mida soovivad.

## <a id="hostedservices"> </a>Loomine ja juurutamine majutatud teenus

Majutatud teenuse loomiseks peab teil esmalt [Azure'i haldusportaal] ja DNS-i eesliide määramisega majutatud teenuse ettevalmistamise ja andmekeskuse soovite lõpuks oma koodi. Seejärel klõpsake oma arenduskeskkond teenuse määratlus (CSDEF) faili loomine, luua oma rakenduse koodi ja paketti (zip) kõik need failid alla (CSPKG) fail. Samuti saate ette valmistada teenuse konfigureerimine (CSCFG) faili. Oma rolli juurutamiseks saadate CSPKG ja CSCFG failide Azure'i teenuse haldus API-ga. Kui juurutada, Azure, kuvatakse sätte rolli aknad andmekeskuse (pärast konfiguratsiooni andmete põhjal), paketi ekstraktimine rakenduse koodis, kopeerimiseks rolli aknad ja linnanimede käivitada. Nüüd on teie koodi ja töötab.

Alloleval joonisel loote arengu arvutis CSPKG ja CSCFG failid. CSPKG fail sisaldab CSDEF faili- ja kahe rollid kood. Pärast üles CSPKG ja CSCFG failide Azure'i teenuse haldus API-ga, loob Azure'i andmekeskuse rolli aknad. Selles näites CSCFG faili märgitud Azure saate luua kolm eksemplari rolli \#1 ja kaks eksemplari rolli \#2.

![pilt][5]

Lisateavet juurutamine üleminekut ja oma rolli, konfigureerimist artiklist [Azure'i rakenduste värskendamine ja juurutamine][] .<a id="Ref" name="Ref"></a>

## <a id="references"> </a>Viited

-   [Majutatud teenuse Azure loomine][]

-   [Azure hostitud teenuste haldamine][]

-   [Migreerimine rakenduste Azure][]

-   [Azure'i rakenduse konfigureerimine][]

<div style="width: 700px; border-top: solid; margin-top: 5px; padding-top: 5px; border-top-width: 1px;">

<p>Kirjutanud Jeffrey Richter (Wintellect)</p>

</div>

  [Azure'i rakenduse mudeli eelised]: #benefits
  [Majutatud teenuse keskendudes]: #concepts
  [Majutatud teenuse kujundamine]: #considerations
  [Teie taotlus skaala kujundamine]: #scale
  [Majutatud teenuse määratlus ja konfigureerimine]: #defandcfg
  [Teenuse definitsioonifail]: #def
  [Teenuse konfiguratsioonifail]: #cfg
  [Loomine ja juurutamine majutatud teenus]: #hostedservices
  [Viited]: #references
  [0]: ./media/application-model/application-model-3.jpg
  [1]: ./media/application-model/application-model-4.jpg
  [2]: ./media/application-model/application-model-5.jpg
  [Kohandatud domeeninime Azure konfigureerimine]: http://www.windowsazure.com/develop/net/common-tasks/custom-dns/
  [Andmete salvestusruumi pakkumisi Azure]: http://www.windowsazure.com/develop/net/fundamentals/cloud-storage/
  [3]: ./media/application-model/application-model-6.jpg
  [4]: ./media/application-model/application-model-7.jpg
  
  [Azure Pricing]: http://www.windowsazure.com/pricing/calculator/
  [Sertide Azure haldamine]: http://msdn.microsoft.com/library/windowsazure/gg981929.aspx
  [http://MSDN.microsoft.com/library/windowsazure/ee758710.aspx]: http://msdn.microsoft.com/library/windowsazure/ee758710.aspx
  [http://MSDN.microsoft.com/library/hh560567.aspx]: http://msdn.microsoft.com/library/hh560567.aspx
  [Azure'i külalistele OS täienduste haldamine]: http://msdn.microsoft.com/library/ee924680.aspx
  [Azure'i haldusportaal]: http://manage.windowsazure.com/
  [5]: ./media/application-model/application-model-8.jpg
  [Juurutamine ja Azure rakenduste värskendamine]: http://www.windowsazure.com/develop/net/fundamentals/deploying-applications/
  [Majutatud teenuse Azure loomine]: http://msdn.microsoft.com/library/gg432967.aspx
  [Azure majutatud teenuste haldamine]: http://msdn.microsoft.com/library/gg433038.aspx
  [Migreerimine rakenduste Azure]: http://msdn.microsoft.com/library/gg186051.aspx
  [Azure'i rakenduse konfigureerimine]: http://msdn.microsoft.com/library/windowsazure/ee405486.aspx
