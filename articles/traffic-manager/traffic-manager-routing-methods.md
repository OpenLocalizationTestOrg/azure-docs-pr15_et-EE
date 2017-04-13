<properties
    pageTitle="Liikluse haldur - liikluse marsruutimise meetodite | Microsoft Azure'i"
    description="See artikkel aitab teil mõista erinevate liikluse marsruutimise meetodid liikluse haldur"
    services="traffic-manager"
    documentationCenter=""
    authors="sdwheeler"
    manager="carmonm"
    editor=""
/>
<tags
    ms.service="traffic-manager"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="infrastructure-services"
    ms.date="10/11/2016"
    ms.author="sewhee"
/>

# <a name="traffic-manager-traffic-routing-methods"></a>Liikluse haldur meetodite liikluse marsruutimine

Azure'i liikluse haldur toetab kolme määrata, kuidas mitmesuguste teenuse lõpp-punktid võrguliikluse marsruutimiseks meetodite liikluse marsruutimist. Liikluse haldur kehtib iga DNS-i päringu saab liikluse marsruutimist meetod. Liikluse marsruutimist meetod võimaldab määrata, milline lõpp-punkti tagastatud vastuseks DNS-i.

Azure'i ressursihaldur liikluse haldurit toetatakse kasutab eri terminoloogia kui klassikaline juurutamise mudel. Järgmine tabel sisaldab sõnu ressursihaldur ja klassikaline erinevused.

| Ressursihaldur termin | Klassikaline termin |
|-----------------------|--------------|
| Liikluse marsruutimist meetod | Koormust tasakaalustavad meetod |
| Priority (prioriteet) meetod | Tõrkesiirde meetod |
| Kaalutud meetod | Round-jaan meetod |
| Jõudluse meetod | Jõudluse meetod |

Klientide tagasiside põhjal oleme muutnud selgust ja vähendamiseks levinud arusaamatusi terminid. Ei erine funktsioone.

Saadaval liikluse haldur on kolm liikluse marsruutimise viisi:

- **Priority (prioriteet):** Valige "Prioriteet", kui soovite kasutada mõnda esmane teenuse lõpp-punkti kõik liikluse ja sisestage varukoopiate juhuks, kui esmast või varukoopia lõpp-punktid pole saadaval.
- **Kaalutud:** Valige "kaalutud" kui soovite jaotada liikluse kogu kogumi lõpp-punktid, kas ühtlaselt või kaalu, mis teile vastavalt määratleda.
- **Jõudluse:** Valige "Jõudlus", kui lõpp-punktid on erinevate geograafiliste asukohtade ja soovite lõppkasutajad kasutada "lähima" lõpp-punkti osas madalam Võrgu latentsusaeg.

Kõik liikluse haldur profiilid sisaldavad lõpp-punkti seisundi ja automaatse lõpp-punkti Tõrkesiirde jälgimist. Lisateavet leiate teemast [Liikluse haldur lõpp-punkti jälgimine](traffic-manager-monitoring.md). Ühe liikluse haldur profiili saate kasutada ainult üks liikluse marsruutimise meetod. Saate valida muu liikluse marsruutimise meetodi profiili igal ajal. Muudatused rakendatakse ühe minuti jooksul ja pole tööseisakute tekib. Pesastatud liikluse haldur profiilid abil saab kombineerida meetodite liikluse marsruutimist. Pesastamine võimaldab keerukaid ja paindlikus liikluse marsruutimist kuvatakse suurem, keerukate rakenduste vajadustele. Lisateabe saamiseks vt [pesastatud liikluse haldur profiilid](traffic-manager-nested-profiles.md).

## <a name="priority-traffic-routing-method"></a>Priority (prioriteet) liikluse marsruutimist meetod

Sageli ettevõtte soovib töökindlus ette oma teenuste juurutamine ühe või mitme teenuse varukoopia juhuks, kui nende esmane teenus läheb alla. "Prioriteet" liikluse marsruutimist meetod võimaldab hõlpsalt rakendada selle Tõrkesiirde mustri Azure kliendid.

![Azure'i liikluse haldur "Prioriteet" liikluse marsruutimist meetod][1]

Liikluse haldur profiili sisaldab loendit tähtsuse teenuse lõpp-punktid. Vaikimisi liikluse haldur saadab kogu liikluse esmane (kõrgeim eelistus) lõpp-punkti. Kui see peamine lõpp-punkti pole saadaval, marsruudib liikluse haldur liiklus teise lõpp-punkti. Kui esmaseid ja teiseseid lõpp ei ole saadaval, läheb liiklus kolmanda ja jne. Lõpp-punkti olemasolu põhineb konfigureeritud olek (lubatud või keelatud) ja poolelioleva lõpp-punkti jälgimine.

### <a name="configuring-endpoints"></a>Lõpp-punktid konfigureerimine

Koos Azure ressursihaldur, Konfigureerige lõpp-punkti prioriteet konkreetselt abil atribuudi 'prioriteet' iga lõpp-punkti. Atribuut on väärtus vahemikus 1 – 1000. Madalamad väärtused tähistavad kõrgem prioriteet. Lõpp-punktid ei saa ühiskasutusse anda prioriteet väärtused. Seades atribuuti pole kohustuslik. Kui välja jäetud, kasutatakse vaikimisi prioriteet lõpp-punkti järjestuse põhjal.

Klassikaline kasutajaliidese, lõpp-punkti prioriteet on konfigureeritud peidetult. Prioriteet põhineb järjestuses, kus on loetletud lõpp-punktid profiili määratlus.

## <a name="weighted-traffic-routing-method"></a>Kaalutud liikluse marsruutimist meetod

"Kaalutud" liikluse marsruutimist meetod võimaldab liikluse hajutamine või kasutada eelmääratletud kaalu.

![Azure'i liikluse haldur "Kaalutud" liikluse marsruutimist meetod][2]

Kaalutud liikluse marsruutimist meetodi, saate määrata kaal liikluse haldur profiili konfiguratsiooni iga lõpp-punkti. Kaal on täisarv vahemikus 1 kuni 1000. See parameeter on valikuline. Kui välja jäetud, liikluse haldurid kasutab vaikimisi paksus 1".

Iga DNS-i päringu saanud, valib liikluse haldur CEIP saadaval lõpp. Tõenäosuse, valides lõpp põhineb kaalu, mis on määratud kõik saadaolevad lõpp-punktid. Kasutades sama kaal üle kõik lõpp-punktid tulemusi on paaris liikluse jaotuse. Suurema või vähema kaalu kasutamine kindla lõpp-punktid põhjustab tagastatakse rohkem või vähem sageli DNS-i vastuste nende lõpp-punktid.

Kaalutud meetodi abil mõned kasulikud stsenaariumid:

- Järk-järgult rakenduse täiendamine: eraldada protsendina liikluse marsruutimiseks uue lõpp-punkti ja järk-järgult suurendada liiklust aja jooksul 100%.
- Rakenduse migreerimise Azure: profiili loomine ja Azure ja välise lõpp-punktid. Saate reguleerida paksuse eelistada uue lõpp-punktid lõpp-punktid.
- Pilveteenuse lõhkemist täiendav läbilaskevõime: kiiresti laiendada on kohapealse juurutuse pilve pannes selle taha liikluse haldur profiili. Kui teil on vaja täiendavat võimsus pilves, saate lisada või lubada mitme lõpp-punktid ja määrata, milline osa liikluse läheb iga lõpp-punkti.

Uue Azure portaali toetab konfiguratsiooni kaalutud liikluse marsruutimist. Kaalu ei saa konfigureerida klassikaline portaalis. Samuti saate konfigureerida kaalu ressursihaldur ja Azure PowerShelli, CLI ja REST API-de klassikaline versioonide abil.

See on oluline mõista DNS-i vastuste vahemällu kliendid ja DNS-i nimede kasutavate klientide Rekursiivsed DNS-i serverid. See vahemällu võivad mõjutada kaalutud liikluse jaotuse. Kliendid ja Rekursiivsed DNS-serverid arv on suur, liikluse jaotuse töötab ootuspäraselt. Juhul, kui kliendid või Rekursiivsed DNS-serverid arv on väike, vahemällu saate märkimisväärselt skew liikluse jaotuse.

Kasutage levinud olukorda on järgmised.

- Arendamise ja testimise keskkonnas
- Rakenduse rakenduse side
- Rakenduste suunatud kitsas kasutaja base, jagavad ühist Rekursiivsed DNS-i taristu (nt puhverserveri kaudu ühenduse ettevõtte töötajad)

Need DNS-i vahemälu efektid on kõik DNS-i-põhine liikluse marsruutimise süsteemid, mitte ainult Azure liikluse haldur. Mõnel juhul konkreetselt DNS-i vahemälu tühjendamine võib lahendust. Muudel juhtudel võib alternatiivne liikluse marsruutimist meetod sobivam.

## <a name="performance-traffic-routing-method"></a>Jõudluse liikluse marsruutimist meetod

Lõpp-punktid kahe või enama asukohad juurutamine kogu maailmas saate parandada rakendustele reageerimine marsruutimise liikluse asukohta, mis on teile ' lähima". "Jõudlus" liikluse marsruutimist meetod pakub seda võimalust.

![Azure'i liikluse haldur "Jõudlus" liikluse marsruutimist meetod][3]

'Lähima' lõpp-punkti pole tingimata lähima kauguse mõõdetuna. Selle asemel "Jõudlus" liikluse marsruutimist meetod määratleb lähima lõpp-punkti mõõta võrgu latentsus. Liikluse haldur säilitab Internet latentsus tabeli jälgida IP-aadresside vahemikud ja iga Azure'i andmekeskuse vahelise edasi aeg.

Liikluse haldur otsib sissetulevate DNS-i taotluse Interneti latentsus tabelis allika IP-aadress. Liikluse haldur valib saadaval lõpp Azure andmekeskuses, mis on madalam latentsus see IP-aadress vahemik ja seejärel annab vastuseks DNS-i selle lõpp-punkti.

[Kuidas liikluse haldur töötab](traffic-manager-how-traffic-manager-works.md)kirjeldatud liikluse haldur ei saa DNS-i päringute otse kliendid. Pigem DNS-i päringute on pärit Rekursiivsed DNS-i teenus, et kliendid on konfigureeritud kasutama. Seetõttu, et määratleda 'lähima' lõpp-punkti ei ole kliendi IP-aadress, kuid see on rekursiivse DNS-i teenuse IP-aadress kasutada IP-aadress. Praktikas IP-aadress on hea puhverserveri kliendi jaoks.

Liikluse haldur värskendatakse regulaarselt Interneti latentsus tabeli muudatused globaalne Internet ja uue Azure piirkondades. Siiski rakenduse jõudlus sõltub laadi muudatusi reaalajas Internetis. Jõudluse liikluse marsruutimist jälgida koormus on antud teenuse lõpp-punkti. Juhul, kui lõpp pole saadaval, liikluse Manager see pole DNS-i päringu vastustes.

Pange tähele, et punkte.

- Kui profiili sisaldab mitut lõpp-punktid Azure piirkonna, siis liikluse haldur jaotab liikluse ühtlaselt üle piirkonnas saadaval lõpp-punktid. Kui eelistate erinevate liikluse jaotuse piirkonna, saate kasutada [pesastatud liikluse haldur profiilid](traffic-manager-nested-profiles.md).

- Kui kõik lubatud lõpp-punktid antud Azure'i piirkonnas on halvenenud, jaotab liikluse haldur liikluse üle kõik muud saadaval lõpp-punktid asemel edasi lähima lõpp-punkti. See loogika takistab kaskaadlaadistiku tõrke tekkimist, mitte ülekoormuse edasi lähima lõpp-punkti. Kui soovite määratleda eelistatud Tõrkesiirde, kasutada [pesastatud liikluse haldur profiilid](traffic-manager-nested-profiles.md).

- Väliste lõpp-punktide või pesastatud lõpp-punktid jõudluse liikluse marsruutimise meetodit kasutades peate määrama asukoha nende lõpp-punktid. Saate valida Azure regiooni juurutamise kõige lähemal. Need asukohad on ei toeta Internet latentsus tabeli väärtused.

- Algoritmi, mis valib lõpp-punkti on tarkadeks. Korduvate DNS-i päringute sama kliendi suunatakse sama lõpp-punkti. Tavaliselt kliendid kasutada eri Rekursiivsed DNS-serverid reisil. Klient võib suunatakse eri lõpp-punkti. Marsruutimise võib mõjutada ka värskendusi Internet latentsus tabelisse. Seetõttu ei garanteeri jõudluse liikluse marsruutimist meetodit klient suunatakse alati sama lõpp-punkti.

- Internet latentsus tabeli muutumisel, märkate, et osa kliente suunatakse eri lõpp-punkti. See marsruutimise muudatus on täpsem praeguse latentsus andmete põhjal. Need värskendused on oluline säilitada täpsuse jõudluse liikluse marsruutimist nagu pidevalt areneb Interneti-ühendus.

## <a name="next-steps"></a>Järgmised sammud

Siit saate teada, kuidas arendada kõrge-saadavus rakenduste abil [liikluse haldur lõpp-punkti jälgimine](traffic-manager-monitoring.md)

Siit saate teada, kuidas [liikluse haldur profiili loomine](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png
