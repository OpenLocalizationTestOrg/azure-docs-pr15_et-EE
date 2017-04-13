<properties 
    pageTitle="Kataloogi integreerimise vahel Mitmikautentimise Azure Active Directory"
    description="See on Azure mitmekordne autentimine leht, mis kirjeldab, kuidas integreerida mitmekordne autentimise Server Azure'i Active Directory nii, et saate sünkroonida kataloogid."
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/04/2016"
    ms.author="kgremban"/>

# <a name="directory-integration-between-azure-mfa-server-and-active-directory"></a>Kataloogi integreerimise Server Azure'i MFA ja Active Directory vahel

Kataloogi integreerimise jaotis võimaldab teil konfigureerida Active Directory või mõne muu Lightweight directory integreerimine.  See võimaldab teil konfigureerida vastavad directory skeemi ja automaatse sünkroonimise kasutajate atribuute.

## <a name="settings"></a>Sätted
Vaikimisi Azure'i mitmekordne autentimine Server on konfigureeritud importida või kasutajate Active Directory sünkroonimine.  Menüü saate alistada vaikekäitumise ja muu Lightweight directory, ADAM kataloogi või kindla Active Directory domeenikontrolleri siduda.  See sisaldab ka LDAP autentimise abil puhverserveri LDAP või LDAP siduda RADIUS eesmärk, eelse autentimise IIS-i autentimist või esmane autentimise kasutaja portaali.  Järgmises tabelis kirjeldatakse üksikute sätted.

![Sätted](./media/multi-factor-authentication-get-started-server-dirint/dirint.png)

| Funktsioon | Kirjeldus |
| ------- | ----------- |
| Active Directory kasutamine | Valige suvand kasutada Active Directory Active Directory kasutamiseks importimine ja sünkroonimine.  See on vaikesäte. <br>Märkus: Arvuti peab olema ühendatud domeeniga ja teil peab allkirjastamist domeeni kontoga Active Directory integreerimiseks töötaks õigesti. |
| Usaldusväärsete domeenide | Märkige ruut kaasa usaldusväärsed domeenid ruut agent proovi ühenduse domeenid on usaldusväärsed praeguse domeeni teine mets domeeni või domeenide kaasatud mets usalda.  Kui ei importimine või sünkroonimine ükskõik millises domeenis usaldusväärsete kasutajate, tühjendage märkeruut jõudluse parandamiseks.  Vaikimisi on märgitud. |
| Teatud LDAP konfigureerimine | Suvand kasutamine Lightweight LDAP sätete määratud importimine ja sünkroonimine. Märkus: Kui valitud on kasutamine Lightweight, kasutajaliidese vahetatakse viited Active Directory LDAP. |
| Redigeerimisnupp | Nupp Redigeeri võimaldab praeguse LDAP konfiguratsiooni sätteid muuta. |
| Atribuut ulatus päringute kasutamine | Näitab, kas atribuut ulatuse päringud tuleks kasutada.  Atribuut ulatus päringute abil tõhusa directory otsingute nõuetele vastavad kirjed, mis põhineb kirjeid mõne muu kirjega atribuut.  Azure'i mitmekordne autentimine Server kasutab atribuut ulatus päringute tõhus päringu, mis on turberühma liige.   <br>Märkus: On mõnel juhul, kui atribuut ulatuse päringud on toetatud, kuid ei tohiks kasutada.  Näiteks saate Active Directory on atribuut ulatus päringute küsimusi, kui turberühma sisaldab rohkem kui ühe domeeni liikmete.  Sel juhul tuleks ruut märkimata. |

Järgmises tabelis kirjeldatakse LDAP konfiguratsiooni sätted.

| Funktsioon | Kirjeldus |
| ------- | ----------- |
| Server | Sisestage hostname või Lightweight directory töötava serveri IP-aadress.  Varukoopia Serveri ka olema määratud, eraldades need semikooloniga. <br>Märkus: Kui siduda tüüp on SSL-i, on täielikult kvalifitseeritud hostname üldiselt nõutav. |
| Base DN | Sisestage eristatav nimi, kust hakkavad kõik directory päringud base directory objekti.  Näiteks AV = abc, AV = com. |
| Siduda tüüp - päringud | Valige sobiv siduda kasutamiseks kui sidumine Lightweight directory otsida.  Seda kasutatakse impordi, sünkroonimine ja kasutajanimi eraldusvõime. <br><br>  Anonüümne - tehakse oma anonüümse siduda.  Siduda DN ja siduda parool ei kasutata.  See toimib ainult siis, kui Lightweight directory võimaldab anonüümse sidumine ja õigused luba päringute vastavad kirjed ja atribuute.  <br><br> Lihtne – siduda DN ja siduda parooli küsimise sidumiseks Lightweight directory lihttekstina.  See tuleks kasutada ainult katsetamiseks server pääseb ja et siduda konto on sobiv juurdepääs kinnitamiseks.  Soovitatav on SSL-i kasutatakse selle asemel pärast vastav cert on installitud.  <br><br> SSL - siduda DN ja siduda parooliga krüptitud SSL-i kasutamine Lightweight directory siduda.  Selleks, et mõne cert olema installitud kohalikult, et Lightweight directory loodab.  <br><br> Windowsi - siduda kasutajanime ja parooli siduda kasutatakse Active Directory domeenikontrolleri või ADAM directory turvaliselt ühendada.  Kui siduda kasutajanimi on tühi, kasutatakse sisselogitud kasutaja kontot siduda. |
| Tippige - isikutuvastusi sidumine | Valige sobiv siduda kasutamiseks täites LDAP siduda autentimist.  Tippige jaotises siduda tüüp - päringud kirjeldused siduda kuvamine  Näiteks see võimaldab kasutada päringute turvamiseks kasutatakse SSL-i siduda anonüümse siduda LDAP siduda isikutuvastusi. |
| Siduda DN või siduda kasutajanimi | Sisestage kasutaja jaoks konto, mida soovite kasutada, kui sidumine Lightweight directory kirje Eraldusnimi.<br><br>Siduda Eraldusnimi kasutatakse ainult siis, kui siduda tüüp on lihtne või SSL-i.  <br><br>Sisestage Windowsi konto, mida soovite kasutada, kui sidumine Lightweight directory Windows, kui siduda tüüp kasutajanimi.  Kui tühjaks, kasutatakse sisselogitud kasutaja kontot siduda. |
| Parooli sidumine | Sisestage parool siduda siduda DN või kasutatava siduda Lightweight directory kasutajanimi.  Mitme teguri Auth serveri AdSync teenuse parooli konfigureerimiseks peab olema lubatud sünkroonimise ja teenus peab töötama kohalikus arvutis.  Parool salvestatakse konto mitmekordne Auth serveri AdSync teenus töötab nimega all Windowsi talletatud kasutajanimed ja paroolid.  Kasutajaliidese mitmekordne Auth Server töötab nagu konto ja klõpsake jaotises nimega töötab mitme teguri Auth serveri teenuse konto parooli ka salvestatakse.  <br><br> Märkus: Kuna parool on salvestatud ainult kohaliku serveri Windows talletatud kasutajanimed ja paroolid, see toiming on vaja teha iga mitmekordne Auth serveris, mis vajab juurdepääsu parool. |
| Päringu mahupiirang | Määrake mahupiirang directory otsing kasutajate maksimumarv.  Selle piirmäära peaksid olema samad konfiguratsiooni Lightweight directory kohta.  Suure otsingute, kus Piipar ei toetata, proovib importimine ja sünkroonimine pakettidena kasutajate toomiseks.  Kui mahupiirangu määratud siin on suurem kui limiit Lightweight directory konfigureeritud, mõnel kasutajal võib jäänud. |
| Nuppu Test | Klõpsake nuppu testi testimiseks sidumine LDAP serveriga.  <br><br> Märkus: Kasutamine Lightweight suvand ei pea olema märgitud, et testida sidumine.  See võimaldab sidumine katsetada enne LDAP konfiguratsiooni abil. |

## <a name="filters"></a>Filtrid
Filtrite võimaldavad kriteeriumid kirjete saada, kui directory otsingu abil.  Filtri seadmisega saate ulatus objektid, mida soovite sünkroonida.  

![Filtrid](./media/multi-factor-authentication-get-started-server-dirint/dirint2.png)

Azure'i Mitmikautentimise on 3 järgmistest suvanditest.

- **Container filter** - filtri kriteeriumid, millistest container kirjete directory otsingu abil määrata.  Active Directory ja ADAM, (| () tavaliselt kasutatakse objectClass=organizationalUnit)(objectClass=container)).  Muud LDAP kataloogid, tuleks kasutada filtrikriteeriumi, mis igat tüüpi container objekti sõltuvalt directory skeem.  <br>Märkus: Kui tühjaks, ((objectClass=organizationalUnit)(objectClass=container)) kasutatakse vaikimisi.

- **Turvalisus jaotises filter** - filtri kriteeriumid kirjete turvalisuse rühmitamine millistest directory otsingu abil määrata.  Active Directory ja ADAM, (&(objectCategory=group) (groupType:1.2.840.113556.1.4.804:=-2147483648)) on tavaliselt kasutatakse.  Muud LDAP kataloogid, tuleks kasutada filtrikriteeriumi, mis erinevat tüüpi turvalisus rühma objekti sõltuvalt directory skeem.  <br>Märkus: Kui tühjaks, (&(objectCategory=group) (groupType:1.2.840.113556.1.4.804:=-2147483648)) vaikimisi kasutada.

- **Kasutaja filter** - filtri kriteeriumid, millistest kasutaja kirjete directory otsingu abil määrata.  Active Directory ja ADAM, (ja (objectClass=user)(objectCategory=person)) kasutatakse üldiselt.  Muud LDAP kataloogid, (objektiklassi = inetOrgPerson) või midagi sarnast tuleks kasutada sõltuvalt directory skeem. <br>Märkus: Kui tühjaks, (ja (Vaikimisi kasutatakse objectCategory=person)(objectClass=user)).

## <a name="attributes"></a>Atribuudid
Atribuute võib kohandada vastavalt vajadusele teatud kataloogi.  See võimaldab teil kohandatud atribuutide lisamine ja sünkroonimine ainult atribuute, mida teil on vaja häälestada.  Iga välja atribuudi väärtus peab olema directory skeemi määratletud atribuudi nimi.  Lisateabe saamiseks kasutage järgmises tabelis.

![Atribuudid](./media/multi-factor-authentication-get-started-server-dirint/dirint3.png)

| Funktsioon | Kirjeldus |
| ------- | ----------- |
| Ainuidentifikaator | Sisestage atribuudi nimi on atribuut, mis toimib container, turberühma ja kasutajale kirjete ainuidentifikaator.  Active Directory, see on tavaliselt FederatedIdentity.  Muud LDAP rakendusi, võib olla entryUUID või midagi sarnast.  Vaikimisi on FederatedIdentity. |
| ... (Valige atribuut) nupud | Iga välja atribuudi on "..." nupp selle kõrval kuvatav valige atribuut dialoogiboks, mis võimaldab valida loendist atribuut. <br><br>Valige dialoogiboksi atribuut.<br><br>Märkus: Atribuutide käsitsi sisestatud ja ei pea atribuut loendist atribuut vastavaks. |
| Ainuidentifikaator tüüp | Valige atribuudi kordumatut tunnust.  Active Directory, atribuutiFederatedIdentityDistinguishedNameatribuuti on tippige GUID.  Muud LDAP rakendusi, võib olla ASCII-baidine massiivi- või stringi tüüpi.  Vaikimisi on GUID. <br><br>Märkus: On oluline, et see õigesti seadistatud, kuna sünkroonimine üksused on viidatud, nende kordumatut tunnust ja leidmiseks otse objekti kataloogi kasutatav kordumatu identifikaator tüüp.  Selle määramine stringi kui kataloogi tegelikult talletab väärtus baitide massiivis ASCII märkide aitab vältida sünkroonimise õigesti. |
| Eraldusnimi | Sisestage atribuut, mis sisaldab iga kirje jaoks Eraldusnimi atribuudi nimi.  Active Directory, see on tavaliselt distinguishedName.  Muud LDAP rakendusi, võib olla entryDN või midagi sarnast.  Vaikimisi on distinguishedName. <br><br>Märkus: Kui atribuudile, mis sisaldab ainult eristatav nimi pole olemas, atribuudi adspath atribuut võib kasutada.  Funktsiooni "LDAP: / /<server>/" tee osa kuvatakse automaatselt eemaldamisel lihtsalt objekti Eraldusnimi lahkumata. |
| Container nimi | Sisestage atribuudi nimi on atribuut, mis sisaldab container kirje nimi.  Atribuudi väärtus kuvatakse Container hierarhias Active Directoryst importimisel või sünkroonimise üksuste lisamine.  Vaikimisi on nimi. <br><br>Märkus: Kui erinevate ümbriste kasutada eri atribuudid nende nimed, mitmele ümbrises nimi atribuudid täpsustatud semikooloniga eraldatud.  Container objekti leitud esimese container nime atribuuti kasutatakse selle nime kuvamiseks. |
| Turvalisus rühma nimi | Sisestage atribuudi kirje turvalisuse rühma nime sisaldava atribuudi nimi.  Atribuudi väärtus kuvatakse loendis turberühma Active Directoryst importimisel või sünkroonimise üksuste lisamine.  Vaikimisi on nimi. |
| Kasutajatele | Otsimine, kuvades, importimine ja kasutajateabe kataloogi sünkroonimine kasutatakse järgmisi atribuute. |
| Kasutajanimi | Sisestage atribuut, mis sisaldab kasutajanime kasutaja kirje atribuudi nimi.  Selle atribuudi väärtust kasutatakse mitut tegurit Auth serveri kasutajanimi.  Teine atribuut võib määratud esimese varukoopia.  Teine atribuuti kasutatakse ainult siis, kui esimene atribuut ei sisalda kasutaja väärtuse.  Vaikeväärtused on userPrincipalName ja sAMAccountName. |
| Eesnimi | Sisestage atribuut, mis sisaldab eesnimi kasutaja kirje atribuudi nimi.  Vaikimisi on givenName. |
| Perekonnanimi | Sisestage atribuut, mis sisaldab perekonnanimi kasutaja kirje atribuudi nimi.  Vaikimisi on sn. |
| E-posti aadress | Sisestage meiliaadress kasutaja kirje sisaldava atribuudi atribuudi nimi.  E-posti aadressi kasutatakse saata tervitus ja kasutajale e-kirju värskendamine.  Vaikimisi on meil. |
| Kasutaja rühma | Sisestage atribuut, mis sisaldab kasutaja rühma kasutaja kirje atribuudi nimi.  Kasutaja rühma saab filter agent ja mitmekordne Auth serveri haldusportaal aruannete kasutajad. |
| Kirjeldus | Sisestage atribuudi nimi on atribuut, mis sisaldab kasutaja kirje kirjeldus.  Kirjeldus kasutatakse ainult otsimiseks.  Vaikimisi on kirjeldus. |
| Kõneposti kõne keel | Sisestage atribuudi nimi on atribuut, mis sisaldab tavakõnede kasutaja jaoks kasutatav keel lühike nimi. |
| SMS teksti keel | Sisestage atribuut, mis sisaldab lühike keele jaoks SMS tekstsõnumeid selle kasutaja nime atribuudi nimi. |
| Telefoni rakenduse keel | Sisestage atribuudi nimi on atribuut, mis sisaldab keele kasutamiseks telefonis rakendus tekstsõnumeid kasutaja lühike nimi. |
| VANDE Turbeloa keel | Sisestage lühike keele jaoks VANDE Turbeloa tekstsõnumeid kasutaja nime sisaldava atribuudi atribuudi nimi. |
| Telefonid | Importida või sünkroonida kasutajate telefoninumbrite kasutatakse järgmisi atribuute.  Kui mõni atribuudi nimi telefonitüübi jaoks pole määratud, telefoni tüüp ei ole saadaval järgmistel juhtudel Active Directory importimine või sünkroonimise üksuste lisamine. |
| Business | Sisestage atribuudi nimi on atribuut, mis sisaldab kasutaja kirje ärikontakti telefoninumber.  Vaikimisi on telephoneNumber. |
| Avaleht | Sisestage kasutaja kirje Kodutelefon arv sisaldava atribuudi atribuudi nimi.  Vaikimisi on Kodutelefon. |
| Piipar | Sisestage atribuudi nimi on atribuut, mis sisaldab kasutaja kirje Piipar arv.  Vaikimisi on Piipar. |
| Mobile | Sisestage atribuut, mis sisaldab kasutaja kirje mobiiltelefoninumber atribuudi nimi.  Vaikimisi on Mobiiliversiooni. |
| Faksi | Sisestage atribuut, mis sisaldab faksinumber kasutaja kirje atribuudi nimi.  Vaikimisi on facsimileTelephoneNumber. |
| IP-telefoni | Sisestage kasutaja kirje IP telefoninumbrit sisaldava atribuudi atribuudi nimi.  Vaikimisi on ipPhone. |
| Kohandatud | Atribuudi nimi on atribuut, mis sisaldab kohandatud telefoninumbri sisestamine |
|  | kasutaja kirje.  Vaikimisi on tühi. |
| Laiend | Sisestage atribuut, mis sisaldab telefoni number laiend kasutaja kirje atribuudi nimi.  Pikendamise välja väärtust kasutatakse ainult esmane telefoninumbri laiend.  Vaikimisi on tühi. <br><br>Märkus: Kui laiend atribuut pole määratud, laiendid saab telefoni atribuut kaasatud.  Nii, et ei saa seda sõeluda peaks pikendamise eelne "x".  Näiteks 555-123-4567 x890 tulemuseks 555-123-4567 nimega telefoninumber ja 890 nimega laiendamine. |
| Taasta vaikesätted nupp | Klõpsake nuppu Taasta vaikesätted naasta kõik atribuudid nende vaikeväärtus.  Vaikesätete peaks õigesti töötada tavaline Active Directory või ADAM skeemi. |

Atribuutide redigeerimiseks klõpsake lihtsalt nuppu Redigeeri atribuute menüü.  See toob windows, mis võimaldab teil muuta ka selle atribuute.

![Atribuutide redigeerimine](./media/multi-factor-authentication-get-started-server-dirint/dirint4.png)

## <a name="synchronization"></a>Sünkroonimine
Sünkroonimise hoiab Azure'i mitmekordne kasutaja andmebaas sünkroonitud kasutajate Active Directory või mõne muu Lightweight Directory Access Protocol (LDAP) Lightweight Directory Access Protocol kataloogist.  Toiming sarnaneb kasutajate käsitsi importimine Active Directory, kuid perioodiliselt küsitlused Active Directory kasutaja ja turvalisus jaotises muudatuste töötlemine.  Pakub keelamine või eemaldamine kasutajate container või turberühma rühmast eemaldatud ja kasutajate eemaldamine kustutatud Active Directoryst.

Mitme teguri Auth ADSync teenus on Windowsi teenus, mis teeb perioodiliste küsitlused Active Directory.  See ei tohi olla segi Azure AD sünkroonimine või Azure'i AD-ühenduse.  mitme teguri Auth ADSync, kuigi tugineb sarnase kood base, on teatud server Azure'i mitme teguri autentimist.  See on installitud peatatud olekus ja käivitatakse mitmekordne Auth serveri teenus konfigureerimisel käivitamiseks.  Kui teil on mitme serveri mitmekordne Auth serveri konfiguratsiooni, võib mitmekordne Auth ADSync käitada ainult ühes serveris.

Mitme teguri Auth ADSync teenus kasutab Microsofti DirSync LDAP serveri laiend muudatuste tõhus küsitlus.  See DirSync juhtelemendi helistaja peab olema "saada muudatused kausta" paremale ja DS Dispersioonanalüüs-too muudatuste laiendatud juurdepääsu haldamine paremale.  Vaikimisi on need õigused määratud domeenikontrolleritesse kontodele administraator ja paremal.  Mitme teguri Auth AdSync teenus on vaikimisi konfigureeritud käivituma paremal.  Seetõttu on kõige lihtsam domeenikontrolleri teenuse käivitamiseks.  Kui saate konfigureerida nii, et alati käivitada täieliku sünkroonimise käitamise teenuse väiksema õigustega konto.  See on vähem tõhusad, kuid vajab konto vähem õigusi.

Kui konfigureeritud kasutama LDAP ja Lightweight directory toetab DirSync juhtelementi, siis küsitlused kasutaja ja turvalisus jaotises muutused töötavad sama, nagu see Active Directory.  Kui Lightweight directory ei toeta DirSync juhtelementi, klõpsake täieliku sünkroonimise tehakse iga tsükli.

![Sünkroonimine](./media/multi-factor-authentication-get-started-server-dirint/dirint5.png)

Iga üksiku sätted vahekaarti sünkroonimine kohta lisateabe saamiseks kasutage järgmises tabelis.

| Funktsioon | Kirjeldus |
| ------- | ----------- |
| Active Directory sünkroonimise lubamine | Kui märgitud, alustatakse perioodiliselt küsitlus Active Directory muudatuste mitmekordne Auth serveri teenus. <br><br>Märkus: tuleb lisada vähemalt üks sünkroonimise üksus ja Sünkrooni kohe tuleb teha enne mitme teguri Auth Server teenuse hakkavad muudatuste töötlemine. |
| Sünkroonimine iga | Saate määrata ajavahemiku, mitme teguri Auth serveri teenuse ootama küsitlused ja muudatuste töötlemine. <br><br> Märkus: Määratud ajavahemiku on iga algusest vahele jääv aeg.  Kui muudatuste töötlemine aeg ületab intervalli, teenuse kohe uuesti küsitlus. |
| Kasutajate eemaldamine enam Active Directorys | Kui märgitud, mitme teguri Auth serveri teenuse Active Directory kustutatud kasutaja hauakivide töötlemine ja seotud mitme teguri Auth serveri kasutaja eemaldamine. |
| Alati täielik sünkroonimine | Märgitud, täidab mitme teguri Auth serveri teenuse alati täieliku sünkroonimise.  Kui see on märkimata, teha mitme teguri Auth serveri teenuse on astmeline sünkroonimine, ainult päringute kasutajad, mis on muutunud.  Vaikimisi on märkimata. <br><br> Märkus: Kui see on märkimata, on astmeline sünkroonimine saab ainult kui kataloogi toetab DirSync juhtelemendi ja sidumiseks kataloogi kasutatava konto on DirSync suureneva päringute sooritamiseks vajalikud õigused.  Kui soovitud kontol puuduvad vajalikud õigused või sünkroonimine on seotud mitu domeeni, teha täieliku sünkroonimise on soovitatav. |
| Nõua administraatori kinnitust, kui mitu X kasutajad on keelatud või seda eemaldada | Sünkroonimise üksusi saab konfigureerida keelamine või eemaldamine kasutajad, kellel ei ole enam üksuse container või turberühma.  Ehk kinnitamist võib olla nõutav kui keelamine või eemaldamine kasutajate arv ületab läve.  Kui märgitud, nõutakse kinnitamise jaoks määratud piiri.  Vaikimisi on 5 ja on vahemikus 1 – 999. <br><br> Kinnitamise hõlbustavad esimese meiliteatise saatmise administraatoritele. E-posti teatis annab läbivaatamine ja keelamise ja kasutajate eemaldamise juhised.  Kui kasutajaliidese mitmekordne Auth Server on käivitatud, palutakse kinnitamiseks. |

Nupp **Sünkrooni kohe** võimaldab täieliku sünkroonimise jaoks määratud sünkroonimise üksused.  Täielik sünkroonimine on nõutav iga kord, kui sünkroonimise üksused on lisatud, muudetud, eemaldada või ümber rühmitada.  Samuti on vaja enne mitme teguri Auth AdSync teenuse hakkab toimima, kuna seda määrab teenuse suureneva muudatused küsitlus, millest järjepunktist.  Kui on tehtud muudatused sünkroonimise üksused ja täieliku sünkroonimise pole sooritatud, teil palutakse Sünkrooni kohe teise navigeerimisel või kasutajaliidese sulgemisel.

Nupp **Eemalda** võimaldab administraator sünkroonimise ühe või mitme üksuse loendist mitme teguri Auth serveri sünkroonimise üksuse kustutada.

>[AZURE.WARNING]Kui kirje sünkroonimine üksus on eemaldatud, ei saa seda taastada. Peate uuesti sünkroonimise üksuse kirje lisamiseks, kui kustutate kogemata.

Sünkroonimise üksusele või üksuste sünkroonimine on eemaldatud mitme teguri Auth serverist.  Mitme teguri Auth serveri teenus pole enam töötleb sünkroonimise üksused.

Nupud Nihuta üles ja Nihuta alla luba sünkroonimise üksuste järjestuse muutmiseks administraator.  Tellimuse on oluline, kuna sama kasutaja võib olla mitu sünkroonimise üksuse (nt ümbris ja turberühma) liige.  Kasutaja on seotud loendi esimese üksuse sünkroonimise tulevad rakendatud kasutajale sünkroonimisel sätted.  Seetõttu sünkroonimise üksused tuleks kasutada tähtsuse järjekorras.

>[AZURE.TIP]Pärast eemaldamist sünkroonimise üksused tuleks teha täielik sünkroonimine.  Täieliku sünkroonimise teostada pärast tellimist sünkroonimise üksused.  Klõpsake täieliku sünkroonimise sooritamiseks nuppu Sünkrooni kohe.

## <a name="multi-factor-auth-servers"></a>Mitme teguri Auth serverid
Täiendavad mitmekordne Auth serverid luuakse varukoopia RADIUS proxy LDAP puhverserveri, või IIS-i autentimise. Kõigi agentide jagatakse sünkroonimise konfigureerimine. Siiski ainult üks nende võib-olla mitme teguri Auth serveri teenus töötab. Sellel vahekaardil võimaldab valida mitut tegurit Auth Server, mis peaks olema lubatud sünkroonimine.

![Factor-Auth serverid.](./media/multi-factor-authentication-get-started-server-dirint/dirint6.png)
