<properties
   pageTitle="Lotus Domino Connector | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse, kuidas konfigureerida Microsofti Lotus Domino konnektor."
   services="active-directory"
   documentationCenter=""
   authors="AndKjell"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.workload="identity"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="08/30/2016"
   ms.author="billmath"/>

# <a name="lotus-domino-connector-technical-reference"></a>Lotus Domino Connector tehniline teave
Selles artiklis kirjeldatakse Lotus Domino konnektor. See artikkel kehtib järgmiste toodete:

- Microsofti Identity Manager 2016 (MIM2016)
- Forefront Identity Manager 2010 R2 (FIM2010R2)
    -   Peate kasutama kiirparandus 4.1.3671.0täiendama või uuem versioon [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 ja FIM2010R2, konnektor on saadaval [Microsofti allalaadimiskeskusest](http://go.microsoft.com/fwlink/?LinkId=717495)alla laadida.

## <a name="overview-of-the-lotus-domino-connector"></a>Lotus Domino konnektor ülevaade
Lotus Domino Connector võimaldab sünkroonimise teenuse integreerida IBM Lotus Domino serveri.

Praeguse versiooni konnektor toetavad kõrgetasemelisi seisukohalt järgmised funktsioonid:

Funktsioon | Tugi
--- | ---
Ühendatud andmeallikas | Server: <li>Lotus Domino 8.5.x</li><li>Lotus Domino 9.x</li>Kliendi:<li>Lotus Notesi 9.x</li>
Stsenaariumid | <li>Objekti elutsükli haldus</li><li>Rühma haldamine</li><li>Parooli haldus</li>
Toimingud | <li>Täielik ja Delta importimine</li><li>Eksportimine</li><li>Ja muuta parooli ka HTTP parooli seadmine</li>
Skeemi | <li>Isiku (rändlusteenus kasutaja kontakti (isikud pole serdiga))</li><li>Rühma</li><li>Ressursi (ressursi, et ruumi võrgukoosoleku)</li><li>E-posti andmebaas</li><li>Dünaamiliste leitavust atribuudid toetatud objektid</li>

Lotus Domino connector kasutab Lotus Notesi kliendi Lotus Domino serveriga. Selle sõltuvuse tõttu peab olema installitud toetatud Lotus Notesi kliendi sünkroonimise serveris. Kliendi ja server suhtlemine rakendatakse Lotus märkmete .NET Interop (Interop.domino.dll) kasutajaliidese kaudu. Selle kasutajaliidese hõlbustab Microsoft.NET platvormi ja Lotus Notes kliendi suhtlemine ja juurdepääsu Lotus Domino dokumentide ja vaadete toetab. Delta importida, on ka C++ sisemist kasutajaliidest kasutatakse (olenevalt valitud delta impordi meetod).

### <a name="prerequisites"></a>Eeltingimused
Enne konnektor kasutamiseks veenduge, et teil on sünkroonimise server järgmist.

- Microsoft .NET 4.5.2 Frameworki või uuem versioon
- Lotus Notesi kliendi peab olema installitud serverisse sünkroonimine
- Lotus Domino konnektor nõuab vaikimisi Lotus Domino LDAP skeemi andmebaasi (schema.nsf) võib Domino kataloogi serveris. Kui seda pole olemas, saate selle installida Microsoft või taaskäivitamine Domino serveri LDAP teenuse abil.

### <a name="connected-data-source-permissions"></a>Ühendatud andmeallika õigused
Toetatud ülesandeid täita Lotus Domino konnektor, peate olema järgmised rühma liige:

- Täielik juurdepääs administraatorid
- Administraatorid
- Andmebaasi administraatorid

Järgmises tabelis on loetletud iga toimingu jaoks vajalike õiguste:

Toiming | Juurdepääsuõigused
--- | ---
Importimine | <li>Avaliku dokumentide lugemine</li><li> Täielik juurdepääs administraator (kui olete täielik juurdepääs administraatorite rühma liige, saate automaatselt on tegelik juurdepääs ACL.)</li>
Eksportida ja parooli seadmine | Juurdepääs: <li>Dokumentide loomine</li><li>Dokumentide kustutamine</li><li>Avaliku dokumentide lugemine</li><li>Kirjutage Avalikud dokumendid</li><li>Dokumentide juhendist või kopeerimine</li>Eksporditoimingute, peate ka järgmisi rolle: <li>CreateResource</li><li>GroupCreator</li><li>GroupModifier</li><li>UserCreator</li><li>UserModifier</li>

### <a name="direct-operations-and-adminp"></a>Otsest toimingute ja AdminP
Minge otse Domino kataloogi või AdminP protsessi kaudu tehtavad toimingud. Järgmised tabelid loendi kõigi toetatud objektid, toiminguid ja vajadusel seotud rakendamise viisi:

**Esmane aadressiraamat**

Objekti | Loomine | Värskendamine | Kustutamine
--- | --- | --- | ---
Isiku | AdminP | Otsest | AdminP
Rühma | AdminP | Otsest | AdminP
MailInDB | Otsest | Otsest | Otsest
Ressurss | AdminP | Otsest | AdminP

**Teisene aadressiraamat**

Objekti | Loomine | Värskendamine | Kustutamine
--- | --- | --- | ---
Isiku | N/A | Otsest | Otsest
Rühma | Otsest | Otsest | Otsest
MailInDB | Otsest | Otsest | Otsest
Ressurss | N/A | N/A | N/A

Ressursi loomisel luuakse märkmete dokumendi. Samuti ressursi kustutamisel kustutatakse märkmete dokument.

### <a name="ports-and-protocols"></a>Pordid ja protokollid
IBM Lotus Notes kliendi ja Domino serverite suhtluse abil märkmete Remote toimingu kõne (NRPC) kus NRPC peaks kasutama TCP/IP. Vaikimisi pordi number on 1352, kuid Domino administraator saab muuta.

### <a name="not-supported"></a>Pole toetatud
Praeguse versiooni Lotus Domino konnektor ei toeta järgmisi toiminguid:

- Postkasti meiliserverite vahel liikumine

## <a name="create-a-new-connector"></a>Looge uus konnektor

### <a name="client-software-installation-and-configuration"></a>Kliendi tarkvara installimine ja konfigureerimine
Lotus Notesi peab olema installitud, klõpsake selle serveri **enne** konnektor on installitud.

Kui installite, veenduge, et teha **Üksikkasutaja Install**. Vaikimisi **Installida mitme kasutaja** ei tööta.  
![Notes1](./media/active-directory-aadconnectsync-connector-domino/notes1.png)

Lehel funktsioonid installimiseks vaja Lotus Notes funktsioonid ja **Kliendi ühekordse sisselogimise**. Ühekordse sisselogimise jaoks on vaja konnektor saama Domino serverisse sisse logima.  
![Raamistiku2](./media/active-directory-aadconnectsync-connector-domino/notes2.png)

**Märkus:** Alustage Lotus Notes üks kord kasutaja, mis asub sama Serveri kontot kasutate saatja teenuse konto. Lisaks veenduge, et server kliendi Lotus Notes sulgemiseks. Ei saa töötama samal ajal konnektor proovib Domino serveriga ühendust luua.

### <a name="create-connector"></a>Konnektor loomine
Lotus Domino konnektori loomiseks **Sünkroonimise teenuse** valige **Haldamise Agent** ja **loomine**. Valige konnektor **Lotus Domino (Microsoft)** .  
![CreateConnector](./media/active-directory-aadconnectsync-connector-domino/createconnector.png)

Kui teie versiooni sünkroonimise teenus pakub võimalust konfigureerimine **arhitektuur**, veenduge, et konnektor on seatud väärtusele vaikimisi **protsessi**käivitamiseks.

### <a name="connectivity"></a>Ühenduvus
Ühenduvuse lehe, peate sisestama sisselogimise identimisteave ja Lotus Domino serverinime määramiseks.  
![Ühenduvus](./media/active-directory-aadconnectsync-connector-domino/connectivity.png)

Atribuudi Domino Server toetab kahe serveri nimi.

- Serveri nimi
- Serverinimi/DirectoryName

**Serverinimi/DirectoryName** vorming on eelistatud Vorming atribuudi, kuna see pakub kiiremini vastuse kui konnektor kontaktide Domino serveri.

Esitatud kasutajanimi fail on salvestatud sünkroonimise teenuse andmebaasi konfigureerimine.

**Delta** importimiseks on teil järgmised võimalused.

- **Mitte keegi**. Konnektor ei ole mis tahes delta impordi.
- **Lisage värskendus**. Konnektor teeb delta impordi lisamise ja värskendamise toimingud. Kustuta **Täielik** imporditoimingu on nõutav. See toiming on kasutusel .net interop.
- **Lisamine/värskendamise ja kustutamise**. Konnektor teeb delta impordi lisada, värskendada ja kustutada. See toiming on kohalikke C++ kasutajaliideste kasutamine.

**Skeemi** suvandid on teil järgmised võimalused.

- **Vaikimisi skeemi**. Konnektor tuvastab skeemi Domino serverist. See on vaikimisi.
- **DSML-skeemi**. Kasutada ainult siis, kui Domino server ei jäta skeemiga. Seejärel saate luua DSML faili skeemi ja selle asemel importimiseks. DSML kohta leiate lisateavet teemast [OASIS](https://www.oasis-open.org/committees/tc_home.php?wg_abbrev=dsml).

Kui klõpsate nuppu edasi, konfiguratsiooni parameetrid kasutajanimi ja parool on kinnitatud.

### <a name="global-parameters"></a>Globaalne parameetrid
Lehel globaalne parameetrite konfigureerimine soovitud ajavöönd ja importimine ja eksportimine toiming suvand.  
![Globaalne parameetrid](./media/active-directory-aadconnectsync-connector-domino/globalparameters.png)

Parameetri **Domino serveri ajavööndit** määratleb Domino-serveri asukoht.

Selle konfiguratsiooni valik on vaja **delta importimine** toimingute toetamiseks, kuna see lubab sünkroonimise teenuse kindlaks teha muudatusi kaks viimast impordi vahel.

#### <a name="import-settings-method"></a>Impordisätete, meetod
Selle **Täielik importimine teha,** on need suvandid.

- Otsing
- Vaade (soovitatav)

**Otsing** on Domino indekseerimise abil, kuid see on levinud, et indeksid ei värskendata reaalajas ja serverist tagastatavad andmed ei ole alati õige. Süsteemi palju muudatusi, see suvand tavaliselt ei tööta ka ja pakub false kustutab teatud olukordades. Siiski **Otsing** on kiirem kui **Vaade**.

**Vaade** on soovitatav suvand, sest see pakub andmete õige olek. See on veidi aeglasem kui otsing.

#### <a name="creation-of-virtual-contact-objects"></a>Virtuaalne kontakti objektid loomine
Funktsiooni **loomise lubamiseks \_kontakti objekti** on järgmised suvandid:

- Ükski
- Viide väärtused
- Viide ja muude väärtuste

Domino, viide atribuute võib sisaldada palju erinevaid vorminguid viitamiseks muid objekte. Saama erinevaid versioone, konnektori rakendab moodustavad \_objektid, nimetatakse ka **Virtuaalse kontaktid** (VC) poole. Need objektid on loodud, et nad saaksid liituda olemasoleva MV objektidele või prognoositud uue objektina. Sel viisil saate säilitada viidete atribuut.

See säte võimaldab ja kui viite atribuudi sisu ei ole DN Vorming soovitud \_kontakti objekt on loodud. Näiteks liikme atribuut rühma võib sisaldada SMTP-aadressid. Samuti on võimalik, et shortName ja muud atribuudid, mis on olemas viide atribuute. Selle stsenaariumi korral valige **Mitte-viide väärtused**. Selle konfiguratsiooni on kõige levinum säte Domino jaoks.

Lotus Domino konfigureerimisel on eraldi aadressiraamatud erinevate eristatav nimed, mis tähistab samale objektile on võimalik luua ka \_pöörduge objektide viide kõik väärtused, mis leiduvad aadressiraamat. Selle stsenaariumi korral, valige suvand **viide ja mitte-viide väärtused** .

Kui teil on mitu väärtust atribuudi **nimi** Domino, siis ka soovite lubada virtuaalse kontaktide loomine nii, et viited saab lahendada. Näiteks see atribuut võib olla mitu väärtust või abielu lahutamise pärast. Märkige ruut Luba **... Nimi on mitu väärtust** selle stsenaariumi.

Ühendades õige atribuute, klõpsake selle \_kontakti objekte oleks liitunud MV objektile.

Need objektid on VC =\_kontakt lisatakse tema DN.

#### <a name="import-settings-conflict-object"></a>Importida, vastuollu objekt
**Konflikti objekti välistamine**

Suure Domino rakendamiseks, on võimalik, et mitme objekti on sama DN dispersioonanalüüs probleemide tõttu. Sellisel juhul näeksid konnektor kahe eri UniversalIDs, kuid samas DN objektid. Konflikti põhjustaks siirdamiseks objekti, mis on loodud konnektori ruumis. Konnektor võite ignoreerida objektide kopeerimine ohvriks Domino valitud. Soovitus on märgitud ruut Säilita.

#### <a name="export-settings"></a>Ekspordi sätted
Kui suvand **Kasuta AdminP värskendamise eest** on valimata, siis ekspordi viide atribuute, nagu liikme on otsese kõne ja ei kasuta AdminP protsessi. Ainult kasutage seda suvandit, kui AdminP pole konfigureeritud säilitada viitamistervikluse.

#### <a name="routing-information"></a>Marsruutimise teave
Domino, on võimalik, et viide atribuut on manustatud järelliide, et DN marsruudi teavet. Näiteks rühma liige atribuut võib sisaldada **CN=example/organization@ABC**. Järelliide @ABC on marsruudi teavet. Domino kasutab marsruutimise teavet meilisõnumite saatmiseks õige Domino süsteemi, mis võib olla muus asutuses süsteem. Marsruutimise teave väljal saate määrata marsruutimise järelliited konnektori ulatus teie asutuses kasutatakse. Kui üks neist väärtustest leitakse mõne järelliide viide atribuudile, eemaldatakse marsruudi teavet viide. Kui viite väärtusele marsruutimise järelliide ei saa sobitada ühte neid väärtusi, mis on määratud, on \_kontakti objekt on loodud. Need \_kontakti objektid on loodud ** RO=@ ** DN lisada. Nende \_kontakti objektide lubamiseks liitumist reaal objekti vajaduse korral lisatakse ka järgmisi atribuute: \_routingName, \_kontaktisiku nimi, \_displayName, ja UniversalID.

#### <a name="additional-address-books"></a>Täiendavad
Kui teil on **kataloogi abi** installitud, mis pakub teisene aadressiraamatud nime, siis saate käsitsi sisestada need aadressiraamatuid.

#### <a name="multivalued-transformation"></a>Mitme väärtusega teisendus
Lotus Domino palju atribuudid on mitme väärtusega. Vastavate metaversumi atribuudid on tavaliselt ühe väärtusega. Impordi ja ekspordi toiming suvand konfigureerimisega lubate konnektor hõlbustamiseks vajalik probleemse atribuudid.

**Eksportimine**  
Ekspordi toiming suvand toetab kahel viisil:

- Üksuse lisamine
- Üksuse asendamine

**Üksuse asendamine** – kui valite selle suvandi konnektor alati eemaldada praegusi väärtusi atribuudi Domino ja esitatud väärtuste asendamine. On esitatud hinnatud võib olla ühe väärtusega või mitme väärtusega.

Näide: Isiku objekti sisselogimisabimehe atribuut on järgmised väärtused.

- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Kui uue nimega **David Alexander** abimehe määratakse selle isiku objekti, tulemus on:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf

**Üksuse lisamiseks** – kui valite selle suvandi konnektor säilitab olemasolevate väärtuste atribuudi Domino ja lisa uued väärtused andmete loendi alguses.

Näide: Isiku objekti sisselogimisabimehe atribuut on järgmised väärtused.

- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Kui uue nimega **David Alexander** abimehe määratakse selle isiku objekti, tulemus on:

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

**Importimine**  
Toiming suvandit impordi toetab kahel viisil:

- Vaikimisi
- Mitme väärtusega üks väärtus

**Vaikimisi** – kui valite vaikesuvand, kõik väärtused kõigi nende atribuutide on imporditud.

**Multivalued üksikväärtus** – kui valite selle suvandi, mitme väärtusega atribuut teisendatakse ühe väärtusega atribuut. Mitme väärtuse korral kasutatakse väärtust ülaosas (see väärtus on tavaliselt ka Viimane).

Näide: Isiku objekti sisselogimisabimehe atribuut on järgmised väärtused.

- CN = David Alexander/OU=Contoso/O=Americas,NAB=names.nsf
- CN = Greg Winston/OU=Contoso/O=Americas,NAB=names.nsf
- CN = John Smith/OU=Contoso/O=Americas,NAB=names.nsf

Viimase värskenduse see atribuut on **David Alexander**. Kuna suvandit impordi toiming on seatud Multivalued üks väärtus, impordib konnektor **David Alexander** ainult konnektori ruumi.

Mitme väärtusega atribuutide teisendamiseks ühe väärtusega atribuutide loogika ei kehti rühma liige atribuut ja atribuut isiku nimi.

On võimalik ka konfigureerimine importimine ja eksportimine teisendus reeglite mitme väärtusega atribuut, kohta atribuudid globaalne reeglile erandi. Sisestage selle suvandi konfigureerimiseks [objektitüüp]. [attributename] tekstiväljadele **välistamist atribuut loendi importimine** ja **eksportimine välistamist atribuut loendis** . Näiteks kui sisestate Person.Assistant ja globaalse lipu väärtuseks importida kõik väärtused, ainult esimene väärtus on imporditud abimees.

#### <a name="certifiers"></a>Sertifitseerijate
Konnektor on loetletud kõik ettevõtte/organisatsioonisisene üksused. Saama isiku objektide eksportimine esmane aadressiraamat, on sertifitseerija oma parool on nõutav.

Kui kõik sertifitseerijate pole sama parool, saab kasutada **kõigi Certifers parooli** . Seejärel saate sisestada parool ja määrata ainult sertifitseerija fail.

Kui impordite ainult, siis pole on mis tahes sertifitseerijate määramiseks.

### <a name="configure-provisioning-hierarchy"></a>Ettevalmistamise hierarhia konfigureerimine
Lotus Domino konnektori konfigureerimisel vahele jätta selle dialoogiboksi lehe. Lotus Domino konnektor ei toeta ettevalmistamise hierarhia.  
![Ettevalmistamise hierarhia](./media/active-directory-aadconnectsync-connector-domino/provisioninghierarchy.png)

### <a name="configure-partitions-and-hierarchies"></a>Sektsioonid ja hierarhiad konfigureerimine
Kui konfigureerite sektsioonid ja hierarhiad, valige nimetatakse NAB=names.nsf esmane aadressiraamatust. Lisaks esmane aadressiraamat, saate valida teisene aadressiraamatud, kui need on olemas.  
![Sektsioonid](./media/active-directory-aadconnectsync-connector-domino/partitions.png)

### <a name="select-attributes"></a>Valige Atribuudid
Kui konfigureerite oma atribuute, valige Kõik atribuudid, mis on eesliide ** \_MMS\_**. Neid atribuute on nõutavad, kui olete ette uue objektide Lotus Domino

![Atribuudid](./media/active-directory-aadconnectsync-connector-domino/attributes.png)

## <a name="object-lifecycle-management"></a>Objekti elutsükli haldus
Selles jaotises antakse ülevaade eri objektide Domino.

### <a name="person-objects"></a>Isiku objektid
Isiku objekti tähistab kasutajate ettevõttes ja ettevõtte üksused. Lisaks vaikimisi atribuute, Domino administraator saate lisada kohandatud atribuute isiku objekti. Isiku objekti sisaldama vähemalt kõik kohustuslikud atribuudid. Kohustusliku atribuute täieliku loendi leiate [Lotus märkmete atribuudid](#lotus-notes-properties). Isiku objekti registreerimiseks peavad olema täidetud järgmised eeltingimused:

- Aadressiraamatu (names.nsf) peab olema määratletud ja see peaks olema esmane aadressiraamatust.
- Peab teil olema O/OU sertifitseerija Id ja parool registreerida teatud kasutaja ettevõte / Organisatsiooniüksus.
- Peate määrama määratud Lotus Notes isiku objekti atribuudid. Neid atribuute, mida kasutatakse ettevalmistamise isiku objekti. Lisateavet leiate jaotisest nimega [Lotus märkmete atribuudid](#lotus-notes-properties) hiljem selles dokumendis.
- HTTP algse isikule parooli on atribuut ja Sea ettevalmistamise ajal.
- Isiku objekti peab olema üks järgmistest toetatud kolmeks:
    1. Tavaline kasutaja, kellel on e-posti fail ja seejärel kasutaja ID-d
    2. Rändluse kasutaja (tavaline kasutaja, mis sisaldab kõiki rändluse andmebaas)
    3. Kontaktide (kasutaja id faili)

Isikute (välja arvatud kontaktid) täpsemaks võib rühmitada meile kasutajat ja rahvusvahelise määratletud väärtus on \_MMS\_IDRegType atribuut. Need inimesed kasutavad märkmete kliendi Lotus Domino serverites juurdepääsuks on märkmete Id ja isiku dokumendi. Kui nad kasutavad märkmete e-posti, siis neil on ka e-posti faili. Kasutaja peab olema registreeritud aktiveerimise. Lisateavet leiate teemast:

- [Märkmete kasutajate seadistamine](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_SETTING_UP_NOTES_USERS.html)
- [Kasutajaks registreerumine](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_REGISTERING_USERS.html)
- [Kasutajate haldamine](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_USERS_5151.html)
- [Kasutajate ümbernimetamine](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_USER_AUTOMATICALLY.html)

Kõik need toimingud on tehtud Lotus Domino ja seejärel importida sünkroonimise teenus.

### <a name="resources-and-rooms"></a>Ressursside ja jututubade
Ressursi on muud tüüpi Lotus Domino andmebaasi. Ressursside võib olla erinevat tüüpi seadmed, nt projektor konverentsiruumi. On Lotus Domino konnektor ei toeta ressursse, mis on määratletud atribuudi ressursi Type alamtüüpi.

Ressursi tüüp | Ressursi tüüp atribuut
--- | ---
Ruumi | 1
Ressursi (muud) | 2
Võrgukoosoleku | 3

Ressursi objekti tüübi töötada, on vaja järgmist:

- Ressursi reserveerimine andmebaasi peaks ühendatud Domino serveri juba olemas
- Saidi on juba määratud ressursi

Ressursi reserveerimine andmebaas sisaldab kolme tüüpi dokumendid.

- Saidi profiilile
- Ressurss
- Broneerimiseks

Ressursi reserveerimine andmebaasi häälestamise kohta leiate lisateavet teemast [ressursside broneerimine andmebaasi häälestamiseks](https://www-01.ibm.com/support/knowledgecenter/SSKTMJ_8.0.1/com.ibm.help.domino.admin.doc/DOC/H_SETTING_UP_THE_RESOURCE_RESERVATIONS_DATABASE.html).

**Loomine, värskendamine ja kustutamine ressursid**  
Lotus Domino konnektor ressursside broneerimiseks andmebaasi läbi toimingute loomine, värskendamine ja kustutamine. Ressursid on loodud dokumente Names.nsf (st esmane aadressiraamat). Redigeerimine ja kustutamine ressursside kohta leiate lisateavet teemast [redigeerimine ja kustutamine ressursi dokumendid](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_EDITING_AND_DELETING_RESOURCE_DOCUMENTS.html).

**Impordi- ja eksporditoimingu ressursid**  
Ressursside saate importida ja eksportida sünkroonimise teenuse, nt objekti tüüp. Valige objekti tüüp ressursi konfigureerimise käigus. Eduka eksporditoimingu peaks olema üksikasjad ressursi tüüp, konverentsi andmebaasi ja saidi nimi.

### <a name="mail-in-databases"></a>Postituse andmebaase
E-posti andmebaasi on andmebaas, mis on mõeldud e-kirju. See on Lotus Domino postkasti, mis ei ole seotud mis tahes kindla kasutajakonto Lotus Domino (st see ei ole oma faili ID ja parool). E-posti andmebaas on kordumatu kasutajanimi ("lühike nimi") seotud see ja oma meiliaadress.

Kui on vaja eraldi e-posti aadressi, mida saab jagada eri kasutajatele eraldi postkasti (nt group@contoso.com), e-posti andmebaas on loodud. Sellele postkastile juurdepääsu juhitakse kaudu oma Accessi juhtelemendi loend (ACL), mis sisaldab märkmete lubatud postkasti avamine kasutajate nimed.

Nõutavate atribuutide loendi leiate jaotisest nimega [Kohustuslik atribuute](#mandatory-attributes) selle artikli.

Andmebaasi eesmärk on saada e-post, luuakse e-posti andmebaasi dokumendi Lotus Domino. Selle dokumendi kuuluma Domino kataloogi iga server, mis talletab andmebaasi koopia. E-posti andmebaasi dokumendi loomise kohta üksikasjaliku kirjelduse leiate teemast [e-posti andmebaasi dokumendi loomine](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_A_MAILIN_DATABASE_DOCUMENT_FOR_A_NEW_DATABASE_OVERVIEW.html).

Enne e-posti andmebaasi loomist peaks andmebaasi juba olemas (peaks olema loodud Lotus administraator) Domino server.

### <a name="group-management"></a>Rühma haldamine
Saate üksikasjaliku ülevaate Lotus Domino rühma haldamine järgmistest allikatest:

- [Rühmade kasutamine](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_USING_GROUPS_OVER.html)
- [Rühma loomine](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS_MIDTOPIC_55038956829238418.html)
- [Loovad ja muudavad rühmad](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_CREATING_AND_MODIFYING_GROUPS_STEPS.html)
- [Rühmade haldamine](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_MANAGING_GROUPS_1804.html)
- [Rühma ümbernimetamine](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_RENAMING_A_GROUP_STEPS.html)

### <a name="password-management"></a>Parooli haldus
Lotus Domino kasutajaks, on kahte tüüpi paroole.

1. Kasutaja parooli (salvestatud faili User.id)
2. Internet / HTTP parooli

Lotus Domino konnektor toetab ainult toimingute HTTP parooliga.

Parooli halduse sooritamiseks lubage parooli halduse Designeris Management Agent konnektor. Parooli halduse lubamiseks valige lehel **Konfigureerimine laiendid** dialoogiboksi **parooli haldamise lubamine** .  
![Laiendid konfigureerimine](./media/active-directory-aadconnectsync-connector-domino/configureextensions.png)

Lotus Domino konnektor tugi Internet parooli toiminguid:

- Määra parool: Määra parool määrab kasutaja Domino HTTP/Interneti uus parool. Vaikimisi konto on ka lukustamata. Ava lipp on avatud Sync Engine WMI liides.
- Parooli muutmine: Selle stsenaariumi korral kasutaja võib-olla soovite parooli muutmine või on teil paluda parooli muutmine pärast määratud aja jooksul. Selle toimingu tegemiseks paigutada, nii (vana ja uus parool) on kohustuslikud. Kui muutnud, värskendatakse Lotus Domino uus parool.

Lisateavet leiate teemast:

- [Interneti-blokeeringu funktsiooni abil](http://www.ibm.com/developerworks/lotus/library/domino8-lockout/)
- [Internet paroolide haldamine](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=/com.ibm.help.domino.admin85.doc/H_NOTES_AND_INTERNET_PASSWORD_SYNCHRONIZATION_7570_OVER.html)

## <a name="reference-information"></a>Lisateave
Selles jaotises loetletud nagu atribuut kirjeldused ja atribuut nõuded Lotus Domino konnektor.

### <a name="lotus-notes-properties"></a>Lotus Notesi atribuudid
Kui olete ette isiku objektid teie Lotus Domino kataloogi, teie objektid peab olema määratud atribuudid kindlad väärtused, mis on täidetud. Need väärtused on ainult vaja luua toiminguid.

Järgmises tabelis on loetletud atribuutidest ja kirjeldatakse neid.

Atribuut | Kirjeldus
--- | ---
\_MMS_AltFullName | Alternatiivne kasutaja täielik nimi.
\_MMS_AltFullNameLanguage | Keele määratlemiseks kasutaja alternatiivse täieliku nime kasutamiseks.
\_MMS_CertDaysToExpire | Enne serdi tänasest kuupäevast päevade arv aegub. Kui pole määratud, vaikimisi on kaks aastat praeguse kuupäeva.
\_MMS_Certifier | Atribuut, mis sisaldab soovitud sertifitseerija nime asutuse hierarhial. Näide: OU = OrganizationUnit, O = organisatsiooniskeemi, C = riik.
\_MMS_IDPath | Kui atribuut on tühi, pole kasutaja ID faili luuakse Kohalik sünkroonimine serveriga. Kui atribuut sisaldab faili nimi, luuakse kasutaja ID faili madata kausta. Atribuut võib sisaldada ka täielik tee.
\_MMS_IDRegType | Isikute saab liigitada kontaktid, meile kasutajate ja rahvusvaheline kasutajad. Järgmises tabelis on loetletud võimalikud väärtused: <li>0 - Kontakt</li><li>1 - USA kasutaja</li><li>2 - rahvusvaheline kasutaja</li>
\_MMS_IDStoreType | USA ja rahvusvaheline kasutajate jaoks atribuudi nõutav väärtuseks. Atribuut sisaldab täisarv, mis määrab, kas kasutaja ID on salvestatud märkmete aadressiraamatust või isiku e-posti faili manusena. Kui kasutaja ID fail on aadressiraamatus manuse, võite luua failina \_MMS_IDPath. <li>Tühi - Store'i ID faili ID võlvkelder identifitseerimise faili (kasutatakse kontaktid).</li><li> 1 - manuse märkmete aadressiraamatus. Funktsiooni \_MMS_Password atribuut peab olema seatud kasutaja ID faile, mille manuseid</li><li>2 - hoida ID isiku e-posti fail. Funktsiooni \_MMS_UseAdminP peab väärtuseks false lasta isiku registreerimise käigus luua e-posti fail. Funktsiooni \_MMS_Password atribuut peab olema seatud kasutaja ID failide jaoks.</li>
\_MMS_MailQuotaSizeLimit | Arv e-posti faili andmebaasi jaoks lubatud MB.
\_MMS_MailQuotaWarningThreshold | Arv e-posti faili andmebaasi jaoks lubatud enne hoiatus MB.
\_MMS_MailTemplateName | E-posti malli fail, mille saab luua kasutaja e-posti faili. Kui mall on määratud, on meil faili määratud malli abil loodud. Kui mall pole määratud, kasutatakse vaikimisi mallifail faili loomiseks.
\_MMS_OU | Valikuline atribuut, mis on OU nime all olevat sertifitseerija. Atribuut peaks olema tühi kontaktide.
\_MMS_Password | Kasutajate jaoks atribuudi nõutav väärtuseks. Atribuut sisaldab objekti ID faili parool.
\_MMS_UseAdminP | Atribuut peaks seatud väärtusele tõene, kui e-posti faili tuleb luua AdminP protsessi Domino serveri (asünkroonne eksportimise käigus). Kui atribuudi väärtus on FALSE, e-posti fail on loodud Domino kasutaja (sünkroonse eksportimise käigus).

Kasutaja failiga seostatud tuvastamise funktsiooni \_MMS_Password atribuut peab sisaldama väärtust. E-posti juurdepääsu kliendi Lotus Notes kaudu, MailServer ja MailFile atribuutide kasutaja peab sisaldama väärtust.

E-posti veebibrauseri kaudu juurdepääsemiseks järgmised atribuudid peab sisaldama väärtusi:

- MailFile - atribuudi nõutav väärtuseks, mis sisaldab tee Lotus Domino serveris, kus meil fail on salvestatud.
- MailServer - atribuudi nõutav väärtuseks, mis sisaldab Lotus Domino serveri nimi. See väärtus on nimi, e-posti Lotus Domino serveri faili loomisel kasutada.
- HTTPPassword - valikuline atribuut, mis sisaldab Web access parooli objekti.

E-posti võimalus ilma Domino serverile juurdepääsu HTTPPassword atribuut peab sisaldama väärtust. Atribuudi MailFile ja atribuudi MailServer võib olla tühi.

Koos \_MMS_ IDStoreType = 2 (e-posti faili poe id), NotesRegistrationclass MailSystem atribuudi väärtuseks REG_MAILSYSTEM_INOTES (3).

### <a name="mandatory-attributes"></a>Kohustusliku atribuudid
Lotus Domino konnektor toetab peamiselt selliste objektide (dokumenditüüpide).

- Rühma
- E-posti andmebaas
- Isiku
- Kontakti (kasutaja pole sertifitseerija)
- Ressurss

Selles jaotises loetletud atribuute, mis on kohustuslikud iga toetatud objekti eksportimine Domino serveri.

Objekti tüüp | Kohustusliku atribuudid
--- | ---
Rühma | <li>Loendi nimi ühendus</li>
Põhi-andmebaas | <li>Täisnimi</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li>
Isiku | <li>Perekonnanimi</li><li>MailFile</li><li>ShortName</li><li>\_MMS_Password</li><li>\_MMS_IDStoreType</li><li>\_MMS_Certifier</li><li>\_MMS_IDRegType</li><li>\_MMS_UseAdminP</li>
Kontakti (kasutaja pole sertifitseerija) | <li>\_MMS_IDRegType</li>
Ressurss | <li>Täisnimi</li><li>ResourceType</li><li>ConfDB</li><li>ResourceCapacity</li><li>Saidi</li><li>DisplayName</li><li>MailFile</li><li>MailServer</li><li>MailDomain</li>

## <a name="common-issues-and-questions"></a>Levinud probleemid ja küsimused

### <a name="schema-detection-does-not-work"></a>Skeemi tuvastamise ei tööta
Tuvasta skeemiga saama, on vaja schema.nsf faili esineb Domino serveri. See fail kuvatakse ainult siis, kui server on installitud LDAP. Kui skeemi ei ole nähtav, siis kontrollige järgmist.

- Faili schema.nsf esineb Domino serveri juurkausta
- Kasutajal on õigus näha schema.nsf faili.
- Jõustada LDAP serveri taaskäivitamist. Avage **Lotus Domino konsooli** ja **Kindlaks LDAP ReloadSchema** käsu abil uuesti skeemiga.

### <a name="not-all-secondary-address-books-are-visible"></a>Kõik teisene aadressiraamatud on nähtavad
Domino Connector tugineb funktsiooni leida teisene aadressiraamatud **Directory abi** . Kui teisene aadressiraamatud on puudu, veenduge, et kui [Directory abi](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_DIRECTORY_ASSISTANCE.html) on lubatud ja konfigureeritud Domino serveri.

### <a name="custom-attributes-in-domino"></a>Kohandatud atribuute Domino
Domino skeemiga laiendada, et kohandatud atribuudi tarbitavad konnektor, kuvatakse see on mitu võimalust.

**Lähenemisviisi 1: Laiendamine Lotus Domino skeem**

1. Saate luua koopia Domino Directory malli {PUBNAMES. NTF} (ei peaks kohandada IBM Lotus Domino vaikekataloogi malli) järgmised [järgmist](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) :
2. Avage koopia, Domino directory Mall {CONTOSO. NTF} Mall, mis loodi Domino Designeris ja tehke järgmist:
    - Klõpsake ühiskasutusega elemente ja laiendage alamvormide
    - Topeltklõpsake ${ObjectName} InheritableSchema alamvormi (kus {ObjectName} on nimi vaikimisi strukturaalset objekti klassi, näiteks: isiku).
    - Nime atribuudi skeemi {MyPersonAtrribute} ja vastav lisamiseks soovitud atribuut. Välja loomine, valige käsk **Loo** menüü ja seejärel valige **väli** menüüst.
    - Lisatud välja selle atribuutide seadmine, valides selle tüüpi, laadi, suuruse, fondi ja muude seotud parameetrite välja aken Atribuudid.
    - Säilita atribuut vaikimisi väärtus sama nimega antud selle atribuudi nimi (nt kui atribuudi nimi on MyPersonAttribute, jätta vaikeväärtus sama nimega).
    - Salvestage värskendatud väärtustega ${ObjectName} InheritableSchema alamvormi.
3. Asendage Domino Directory malli {PUBNAMES. NTF} uue kohandatud malli {CONTOSO. NTF}, toimige [järgmiselt](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
4. Sulgege Domino administraator ja Domino konsooli LDAP teenust uuesti ja uuesti laadimiseks LDAP skeemi avada.
    - Domino konsooli, Lisa käsk jaotises **Domino käsu** tekst esitatud LDAP teenust - [Taaskäivitage tööülesande LDAP]( http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)uuesti.
    - LDAP laadimiseks skeemi käsuga LDAP kindlaks teha – kindlaks LDAP ReloadSchema
5. Avatud Domino administraator ja valige inimesed ja rühmad menüü kuvamiseks lisatud atribuut kajastub domino lisada isiku.
6. Avage Schema.nsf menüüd **failid** ja näha lisatud atribuut kajastub dominoPerson LDAP objekti klassi.

**Lähenemisviisi 2: Luua kohandatud atribuut on auxClass ja seostada objekti klassi**

1. Saate luua koopia Domino Directory malli {PUBNAMES. NTF}, toimige [järgmiselt](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_CREATING_A_COPY_OF_THE_DEFAULT_PUBIC_ADDRESS_BOOK_TEMPLATE.html) (mitte kunagi kohandamine IBM Lotus Domino vaikekataloogi Mall):
2. Avage koopia, Domino directory Mall {CONTOSO. NTF} Mall, mis loodi Domino Designer.
3. Valige vasakul paanil jagatud koodi ja alamvorme.
4. Klõpsake uue alamvormi
5. Uue alamvormi atribuutide määramiseks toimige järgmiselt.
    - Uue alamvormi avanud, valige kujundus - alamvormi atribuudid
    - Sisestage väljale atribuudi nimi nimi abistava objekti klassi – näiteks TestSubform.
    - Säilita suvandid atribuudi "Kaasata lisa alamvormi... dialoogiboksi" valitud
    - Tühjendage suvandid atribuudi "Renderda läbida HTML-i märkmetes."
    - Jätke muude atribuutide sama ja sulgege dialoogiboks alamvormi atribuudid.
    - Salvestage ja sulgege uue alamvormi.
6. Määratleda abistava objekti klassi välja lisamiseks tehke järgmist.
    - Avage loodud alamvormi.
    - Valige Loo - välja.
    - Dialoogiboksi väli menüü põhilised nime kõrval määrata mis tahes nimi, nt: {MyPersonTestAttribute}.
    - Lisatud välja, valides selle tüüp, laad, suurus, fondi ja seotud atribuudid selle atribuutide seadmine.
    - Säilita atribuut vaikimisi väärtus sama nimega antud selle atribuudi nimi (nt kui atribuudi nimi on MyPersonTestAttribute, jätta vaikeväärtus sama nimega).
    - Salvestage värskendatud väärtustega alamvormi ja tehke ühte järgmistest:
        - Valige vasakul paanil ühiskasutuses kood ja seejärel alamvormide
        - Valige uue alamvormi ja valige kujundus - atribuudid.
        - Klõpsake vahekaarti vasakult kolmas ja valige **Propagate selle keelamine kujunduse muutmine**.
7. Avage ${ObjectName} ExtensibleSchema alamvormi korral (kus {ObjectName} on vaikimisi strukturaalset objekti klassi, näiteks – inimese nimi).
8. Ressursi lisamine ja valige alamvormi (loodud, näiteks – TestSubform) ja salvestada ${ObjectName} ExtensibleSchema alamvormi.
9. Asendage Domino Directory malli {PUBNAMES. NTF} uue kohandatud malli {CONTOSO. NTF}, toimige [järgmiselt](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_ABOUT_RULES_FOR_CUSTOMIZING_THE_PUBLIC_ADDRESS_BOOK.html).
10. Sulgege Domino administraator ja Domino konsooli LDAP teenust uuesti ja uuesti laadimiseks LDAP skeemi avada.
    - Domino konsooli, Lisa käsk jaotises **Domino käsu** tekst esitatud LDAP teenust - [Taaskäivitage tööülesande LDAP](http://publib.boulder.ibm.com/infocenter/domhelp/v8r0/index.jsp?topic=%2Fcom.ibm.help.domino.admin85.doc%2FH_STARTING_AND_STOPPING_THE_LDAP_SERVER_OVER.html)uuesti.
    - LDAP laadimiseks skeemi käsu kindlaks LDAP **LDAP ReloadSchema kindlaks teha**.
11. Avage Domino administraator ja valige inimesed ja rühmad vahekaarti, et näha lisatud atribuut kajastub domino lisa isiku (jaotises teised tab).
12. Avage Schema.nsf menüüd **failid** ja näha lisatud atribuut kajastub TestSubform LDAP abistava objekti klassi.

**Lähenemisviisi 3: Saate lisada kohandatud atribuuti ExtensibleObject klassi**

1. Avatud {Schema.nsf} fail, juurkaustas
2. Valige vasakpoolsest menüüst **Kõik skeemi** dokumendivaates LDAP objekti tunnid ja klõpsake nuppu **Lisa objekti klassi** :
3. Sisestage LDAP nimi kujul {zzzExtensibleSchema} (kus zzz on vaikimisi strukturaalset objekti klassi, näiteks inimese nimi). Laiendamine skeemiga isiku objekti klassi, sisestage LDAP nimi {PersonExtensibleSchema}.
4. Sisestage Standard objekti tunni nime, mille jaoks soovite pikendada skeemiga. Isiku objekti klassi skeemi laiendamine, sisestage Standard objekti klassinimi {dominoPerson}.
5. Sisestage kehtiv objekti ID, mis vastavad objekti.
6. Valige jaotises kohustuslik või valikuline atribuut tüüpi väljad ühe nõude laiendatud/kohandatud atribuute.
7. Pärast selle ExtensibleObjectClass nõutavate atribuutide lisamine, klõpsake nuppu **Salvesta ja Sule**.
8. Mõne ExtensibleObjectClass luuakse automaatselt vastav vaikimisi objekti klassi laiendatud atribuutidega.

## <a name="troubleshooting"></a>Tõrkeotsing

-   Konnektor tõrkeotsingu logimine lubamise kohta leiate artiklist [Kuidas lubada ETW jälitamine konnektorid](http://go.microsoft.com/fwlink/?LinkId=335731).
