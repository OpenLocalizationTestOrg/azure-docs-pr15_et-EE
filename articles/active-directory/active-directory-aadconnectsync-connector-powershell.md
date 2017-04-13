<properties
   pageTitle="PowerShelli konnektor | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse, kuidas konfigureerida Microsoft Windows PowerShelli konnektor."
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

# <a name="windows-powershell-connector-technical-reference"></a>Windows PowerShelli konnektor tehniline teave
Selles artiklis kirjeldatakse Windows PowerShelli konnektor. See artikkel kehtib järgmiste toodete:

- Microsofti Identity Manager 2016 (MIM2016)
- Forefront Identity Manager 2010 R2 (FIM2010R2)
    -   Peate kasutama kiirparandus 4.1.3671.0täiendama või uuem versioon [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 ja FIM2010R2, konnektor on saadaval [Microsofti allalaadimiskeskusest](http://go.microsoft.com/fwlink/?LinkId=717495)alla laadida.

## <a name="overview-of-the-powershell-connector"></a>PowerShelli konnektor ülevaade
PowerShelli konnektor võimaldab sünkroonimise teenuse integreerida välised süsteemid, mis pakub Windows PowerShelli vastavalt API-d. Konnektor pakub on seose kõne-põhiste Laiendatav Ühenduvus haldamise agendi võimaluste vahel 2 (ECMA2) raamistik ja Windows PowerShelli. ECMA raames kohta leiate lisateavet teemast [Laiendatav Ühenduvus 2.2 Management Agent viide](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Eeltingimused
Enne konnektor kasutamiseks veenduge, et teil on sünkroonimise server järgmist.

- Microsoft .NET 4.5.2 Frameworki või uuem versioon
- Windows PowerShelli 2.0, 3.0 või 4.0

Täitmise poliitika sünkroonimise teenuse server peab olema konfigureeritud lubama konnektor Windows PowerShelli skriptide käitamiseks. Kui konnektor töötab skriptide digitaalset allkirjastamist konfigureerida selle käsu täitmise poliitika.  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Looge uus konnektor
Windows PowerShelli konnektor sünkroonimise teenuse koostamiseks peab võimaldama sarja Windows PowerShelli skripti, mis käivitada sünkroonimise teenuse taotleja juhiseid. Sõltuvalt andmeallikaga ühenduse loomisel ja funktsioone, vajate, peate rakendama skriptide muutub. Selles jaotises antakse ülevaade iga skriptide, mida saab rakendada, ja kui neid on vaja.

Windows PowerShelli konnektor on mõeldud salvestamiseks iga skriptide sünkroonimise teenuse andmebaasi sees. See on võimalik, mis on talletatud failisüsteemi skriptide käitamiseks, on hõlpsam iga skripti otse soovitud konnektori konfiguratsiooni sisu lisada.

PowerShelli konnektor loomiseks **Sünkroonimise teenuse** valige **Haldamise agendi** ja **loomine**. Valige konnektor **PowerShelli (Microsoft)** .

![Konnektor loomine](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Ühenduvus
Lisage ühenduse loomine remote süsteemi konfiguratsioon parameetrid. Need väärtused on turvaliselt talletatud sünkroonimise teenuse ja kättesaadavaks oma Windows PowerShelli skriptide konnektor käitamisel.

![Ühenduvus](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

Saate konfigureerida järgmisi Ühenduvus.

**Ühenduvus**

Parameetri | Vaikeväärtus | Otstarve
--- | --- | ---
Server | <Blank> | Konnektor peab ühenduse serveri nimi.
Domeeni | <Blank> | Mandaadi talletamiseks kasutamiseks konnektor käitamisel domeenis.
Kasutaja | <Blank> | Kasutajanime mandaadi konnektor käitamisel kasutamiseks salvestada.
Parooli | <Blank> | Mandaadi talletamiseks kasutamiseks konnektor käitamisel parool.
Jäljendada Connectori konto | FALSE | True, kui käivitatakse sünkroonimise teenuse Windows PowerShelli skriptide kontekstis esitatud mandaate. Kui võimalik, on soovitatav, et **$Credentials** parameeter on möödas igale skripti kasutatakse isikustamine asemel. Täiendavad selle suvandi kasutamiseks vajalikud õigused kohta leiate lisateavet teemast [Konfigureerimise pilti](#additional-configuration-for-impersonation).
Kui olete laadi kasutajaprofiili | FALSE | Juhendab Windowsi ajal isikustamine saatja identimisteabe kasutajaprofiili laadimiseks. Kui allalaadimine jäljendatud kasutaja on rändluse profiili, konnektor ei laadita rändluse profiili. Täiendav parameeter kasutamiseks vajalikud õigused kohta leiate lisateavet teemast [Konfigureerimise pilti](#additional-configuration-for-impersonation).
Kui olete sisselogimise tüüp | Ükski | Sisselogimise tüüp isikustamine ajal. Lisateabe saamiseks vaadake [dwLogonType] [ dw] dokumentatsiooni.
Kehtib ainult skriptide | FALSE | Kui väärtus on true, Windows PowerShelli konnektor kinnitab, et iga script on kehtiv digitaalallkiri. Kui väärtus on false, siis veenduge, et sünkroonimise teenuse serveri Windows PowerShelli täitmise poliitika on RemoteSigned või piiranguta.

**Levinud mooduli**  
Konnektor võimaldab salvestada ühiskasutusega Windows PowerShelli mooduli konfiguratsiooni. Konnektor käitamisel skripti Windows PowerShelli mooduli failisüsteemi ekstraktimist nii, et see saab importida iga skript.

Impordi ja ekspordi parooli sünkroonimise skripte, ekstraktimist moodulis saatja MAData kausta. Skeem, valideerimine, hierarhia ja Partition discovery skripte moodulis ekstraktimist kaust % TEMP %. Mõlemal juhul nimega ekstraktitud levinud mooduli skripti vastavalt levinud mooduli skripti nimi.

Nimega FIMPowerShellConnectorModule.psm1 MAData kaustast mooduli laadimiseks kasutada järgmine tekst:`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

Mooduli FIMPowerShellConnectorModule.psm1 nimega kaust % TEMP % laadimiseks kasutada järgmine tekst:`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**Parameetri valideerimine**  
Valideerimise Script on valikuline Windows PowerShelli skripti, mis saab tagada, et konnektori konfiguratsiooni parameetritest administraator on lubatud. Sisseseatud server, ühenduse identimisteabe ja ühenduvuse parameetrid on levinud tavadega valideerimine skripti. Valideerimine skripti nimetatakse pärast järgmisi vahekaarte ja dialoogid on muudetud.

- Ühenduvus
- Globaalne parameetrid
- Sektsiooni konfigureerimine

Valideerimine skripti saab järgmisi konnektor:

Nimi | Andmetüüp | Kirjeldus
--- | --- | ---
ConfigParameterPage | [ConfigParameterPage][cpp] | Konfiguratsiooni menüü või dialoogiboksi päästiksündmuse vallandanud taotluse kinnitamine.
ConfigParameters | [KeyedCollection] [ keyk] [string, [ConfigParameter][cp]] | Tabel konnektori konfiguratsiooni parameetrid.
Mandaadi | [PSCredential][pscred] | Sisaldab mis tahes identimisteabe sisestatud ühenduvuse menüü administraator.

Skripti valideerimine peaks tagasi ParameterValidationResult üksikobjektiks tulemas.

**Skeemi tuvastamine**  
Skeemi Discovery script on kohustuslikud. See skript tagastab objekti tüübid, atribuudid ja atribuut piiranguid kasutav sünkroonimise teenuse konfigureerimisel atribuut meilivoo reeglid. Skeemi Discovery skripti käitatakse konnektor loomise ajal ja kuvab saatja skeemi. Seda kasutatakse ka skeemi värskendamine toimingu sünkroonimise Service Manager.

Skeemi discovery skripti saab järgmisi konnektor:

Nimi | Andmetüüp | Kirjeldus
--- | --- | ---
ConfigParameters | [KeyedCollection] [ keyk] [string, [ConfigParameter][cp]] | Tabel konnektori konfiguratsiooni parameetrid.
Mandaadi | [PSCredential][pscred] | Sisaldab mis tahes identimisteabe sisestatud ühenduvuse menüü administraator.

Skripti peab andma ühe [skeemi] [ schema] tulemas objekti. Skeemi objekti koosneb [SchemaType] [ schemaT] objekte, mis tähistavad objekti tüüp (nt: kasutajad ja rühmad). SchemaType objekt sisaldab [SchemaAttribute] kogumi[ schemaA] objekte, mis tähistavad atribuute (nt: eesnimi, perekonnanimi ja aadress) tüüpi.

**Täiendavad parameetrid**  
Lisaks standard Otsingukonfiguratsiooni sätetest, saate määratleda kohandatud konfigureerimise sätted, mis on omased konnektor eksemplari. Veebisaidil konnektor, partition, saab määrata järgmiste parameetrite või Käivita samm tasemed ja oluline Windows PowerShelli skripti kaudu avatud. Kohandatud Otsingukonfiguratsiooni sätetest võib talletatud sünkroonimise teenuse andmebaasi lihttekstivormingus või võib olla krüptitud. Sünkroonimise teenuse automaatselt krüptib ja dekrüptib turvaline konfiguratsioonisätted, kui see on nõutav.

Kui soovite määrata kohandatud Otsingukonfiguratsiooni sätetest, eraldi iga parameetri nimi semikoolon (;).

Kohandatud Otsingukonfiguratsiooni sätetest skripti kaudu juurdepääsemiseks tuleb järelliide nimi ja allkriips ( \_ ) ja ulatuse parameetri (Global, sektsiooni või RunStep). Juurdepääs parameetri globaalne nimi, kasutage selle koodilõigu.`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Võimalused
Võimaluste vahekaarti haldus Agent Designer määratleb käitumine ja funktsionaalsus konnektor. Sellel vahekaardil tehtud valikud ei saa muuta loomisel konnektor. Selles tabelis on loetletud võimalus sätted.

![Võimalused](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

Võimalus | Kirjeldus |
--- | --- |
[Laadi Eraldusnimi][dnstyle] | Näitab, kui konnektor toetab eristatav nimed ja kui jah, mis laad.
[Ekspordi tüüp][exportT] | Määratleb objekte, mis on esitatud ekspordi skripti tüüp. <li>AttributeReplace – sisaldab mitme väärtusega atribuut väärtused täiskomplekti atribuut muutumisel.</li><li>AttributeUpdate – sisaldab ainult deltad mitme väärtusega atribuut atribuut muutumisel.</li><li>MultivaluedReferenceAttributeUpdate - sisaldab mitme väärtusega viide atribuudid Näita ainult erinevusi ja väärtused-vajalik mitme väärtusega atribuutide kogum.</li><li>ObjectReplace – sisaldab objekti kõik atribuudid, kui mis tahes atribuut muudatused</li>
[Andmete normaliseerimine][DataNorm] | Juhendab sünkroonimise teenust normaliseerimine enne skriptide neile ankur atribuudid.
[Objekti kinnituse][oconf] | Konfigureerib ootel impordi käitumine sünkroonimise teenus. <li>Tavaline – vaikekäitumist, mis eeldab, et kõik eksporditud muudatused kinnitada importimise kaudu</li><li>NoDeleteConfirmation – objekti kustutamisel ei pole loodud ootel impordi.</li><li>NoAddAndDeleteConfirmation – objekti loomisel või kustutanud, ei loodud pole ootel impordi.</li>
Kasuta DN ankur | Kui eristatav nimi laad on määratud LDAP, konnektori ruumi ankur atribuuti on ka eristatav nimi.
Samaaegseid toimingute mitu konnektorid. | Kui märgitud, käitamise korraga mitme Windows PowerShelli konnektorid.
Sektsioonid | Kui märgitud, konnektor toetab mitut sektsioonid ja partition tuvastamine.
Hierarhia | Kui märgitud, toetab konnektor LDAP laadi hierarhilise struktuuri.
Impordi lubamine | Kui märgitud, konnektor imporditavate andmete importimine skripti kaudu.
Delta impordi lubamine | Kui märgitud, saate konnektor impordi skriptide taotleda deltad.
Ekspordi lubamine | Kui märgitud, konnektor ekspordib andmed ekspordi skripti kaudu.
Täielik ekspordi lubamine | Kui märgitud, ekspordi skriptide toetavad eksportimise kogu konnektori ruumi. Kasutage seda suvandit, kui soovite lubada eksportimine tuleb kontrollida ka.
Esimene ekspordi läbimine pole viide väärtusi | Kui märgitud, eksporditakse teise ekspordi pass viide atribuute.
Luba objekti ümbernimetamine | Kui märgitud, saate muuta eristatav nimed.
Kustuta lisada nagu asendamine | Kui märgitud, Kustuta-lisada ühe asendusena eksporditakse toimingud.
Parooli toimingute | Kui märgitud, parooli sünkroonimise skriptide on toetatud.
Ekspordi parooli esimene läbimine lubamine | Kui märgitud, eksporditakse paroolide käigus ettevalmistamise objekti loomisel.

### <a name="global-parameters"></a>Globaalne parameetrid
Globaalne parameetrid Management Agent kujundaja saab konfigureerida Windows PowerShelli skriptide konnektor töötavad. Samuti saate konfigureerida kohandatud konfiguratsioonisätted, mis on määratletud vahekaardil ühenduvuse globaalne väärtused.

**Partition tuvastamine**  
Sektsiooni on eraldi nimeruum ühe ühiskasutusega skeemi sees. Näiteks Active Directory on iga domeeni jooksul ühe mets sektsioon. Sektsiooni on loogiline rühmitamine impordi ja ekspordi toimingud. Importimise ja eksportimise on partition kontekstis ja kõik toimingud, mis juhtub selles kontekstis. Sektsioonid peaksid tähistada LDAP hierarhia. Eraldusnimi sektsiooni kasutatakse impordi kinnitamaks, et kõik tagastatud objektid on sektsiooni piires. Sektsiooni Eraldusnimi kasutatakse ka ajal ettevalmistamise metaversumi konnektori ruumi määratlemiseks sektsiooni objekti peaks olema seostatud ekspordi ajal.

Sektsiooni discovery skripti saab järgmisi konnektor:

Nimi | Andmetüüp | Kirjeldus
--- | --- | ---
ConfigParameters  | [KeyedCollection][keyk][string, [ConfigParameter][cp]] | Tabel konnektori konfiguratsiooni parameetrid.
Mandaadi | [PSCredential][pscred] | Sisaldab mis tahes identimisteabe sisestatud ühenduvuse menüü administraator.

Skripti peab andma on kas üks [Partition] [ part] objekti või tulemas Partition objektide loendi [K].

**Hierarhia tuvastamine**  
Hierarhia discovery skripti kasutatakse ainult siis, kui eristatav nimi laadi võimalus on LDAP. Skripti kasutatakse abil saate sirvida ja valige ümbriste kogum, mida käsitletakse või vähendada ulatus importimiseks ja eksportimiseks toimingud. Skripti peaks sõlmed, mis on otsese laste juurkausta sõlme skripti edastada ainult loetleda.

Hierarhia discovery skripti saab järgmisi konnektor:

Nimi | Andmetüüp | Kirjeldus
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][string, [ConfigParameter][cp]] | Tabel konnektori konfiguratsiooni parameetrid.
Mandaadi | [PSCredential][pscred] | Sisaldab mis tahes identimisteabe sisestatud ühenduvuse menüü administraator.
ParentNode | [HierarchyNode][hn] | Hierarhia, mille skripti peaks tagastama otsese laste juurkausta sõlme.

Skript peab tagastama on kas ühe lapse HierarchyNode objekti või lapse HierarchyNode objektide loendi [K] tulemas.

#### <a name="import"></a>Importimine
Ühendused, mis toetavad imporditoimingute peab rakendada kolme skriptide.

**Alustage importimine**  
Alustage impordi skript käivitatakse on käivitada impordi samm alguses. Ajal selles etapis tuleb saate luua ühendust allika süsteem ja tehke enne andmete importimisest ühendatud süsteemi ettevalmistavaid samme.

Alustage impordi skripti saab järgmisi konnektor:

Nimi | Andmetüüp | Kirjeldus
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][string, [ConfigParameter][cp]] | Tabel konnektori konfiguratsiooni parameetrid.
Mandaadi | [PSCredential][pscred] | Sisaldab mis tahes identimisteabe sisestatud ühenduvuse menüü administraator.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Skripti teavitab tüüpi impordi käivitamine (delta või täisversioon) sektsiooni, hierarhia, vesimärgi ja oodatud lehe suurus.
Tüübid | [Skeemi][schema] | Konnektori ruumi, mis on imporditud skeem.

Skripti peab andma ühe [OpenImportConnectionResults] [ oicres] objekti müügivõimaluste, näiteks:`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Andmete importimine**  
Andmete importimine skripti nimetatakse konnektor enne skripti, et oleks rohkem andmeid importida. Windows PowerShelli konnektor on 9999 objektide lehe suurus. Kui teie script tagastab rohkem kui 9999 objektide importimine, peab toetama Piipar. Konnektor seab nimetatakse kohandatud andmed atribuudi, et saate salvestada vesimärgi nii, et iga kord impordi andmed skripti, skripti uuesti importimisel objektid, kus see pooleli jäi.

Andmete importimine skripti saab järgmisi konnektor:

Nimi | Andmetüüp | Kirjeldus
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][string, [ConfigParameter][cp]] | Tabel konnektori konfiguratsiooni parameetrid.
Mandaadi | [PSCredential][pscred] | Sisaldab mis tahes identimisteabe sisestatud ühenduvuse menüü administraator.
GetImportEntriesRunStep | [ImportRunStep][irs] | Hoiab vesimärgi (CustomData), mida saab kasutada ajal leheküljed impordi ja delta impordi.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Skripti teavitab tüüpi impordi käivitamine (delta või täisversioon) partition, hierarhia, vesimärgi ja oodatud lehe suurus.
Tüübid | [Skeemi][schema] | Konnektori ruumi, mis on imporditud skeem.

Andmete importimine skript tuleb kirjutada loendi [[CSEntryChange][csec]] tulemas objekti. Kogu koosneb CSEntryChange atribuute, mis esindab iga objekti imporditaval. Täielik importimine käitamisel selle saidikogumi peaks olema CSEntryChange objekte, mis on iga objekti kõigi atribuutide kogum. Ajal Delta importida, CSEntryChange objekti peaks sisaldama kas atribuut taseme deltad iga objekti importida, või täieliku kujutis objekte, mis on muutunud (Asenda režiim).

**Lõpeta importimine**  
Käivitage importimine lõppedes käitatakse End importimine skript. See skript peaks tehke Kettapuhastus toiminguid vajalikud (nt Sule ühendused süsteemid) ja vasta ebaõnnestumist.

Lõpeta importimine skripti saab järgmisi konnektor:

Nimi | Andmetüüp | Kirjeldus
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][string, [ConfigParameter][cp]] | Tabel konnektori konfiguratsiooni parameetrid.
Mandaadi | [PSCredential][pscred] | Sisaldab mis tahes identimisteabe sisestatud ühenduvuse menüü administraator.
OpenImportConnectionRunStep | [OpenImportConnectionRunStep][oicrs] | Skripti teavitab tüüpi impordi käivitamine (delta või täisversioon) sektsiooni, hierarhia, vesimärgi ja oodatud lehe suurus.
CloseImportConnectionRunStep | [CloseImportConnectionRunStep][cecrs] | Skripti teavitab importimine on lõppenud põhjuse kohta.

Skripti peab andma ühe [CloseImportConnectionResults] [ cicres] objekti müügivõimaluste, näiteks:`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Eksportimine
Identne impordi arhitektuur konnektor peab ühendused, mis toetavad ekspordi rakendada kolme skriptide.

**Alustage eksportimine**  
Alustage ekspordi skript käivitatakse on ekspordi käivitada samm alguses. Selle toimingu käigus saate andmeallika süsteemi ühendust luua ja läbiviimine mis tahes ettevalmistavaid samme enne andmete eksportimise ühendatud süsteemi.

Alustage ekspordi skripti saab järgmisi konnektor:

Nimi | Andmetüüp | Kirjeldus
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][string, [ConfigParameter][cp]] | Tabel konnektori konfiguratsiooni parameetrid.
Mandaadi | [PSCredential][pscred] | Sisaldab mis tahes identimisteabe sisestatud ühenduvuse menüü administraator.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Skripti teavitab ekspordi käivitada (delta või täisversioon) sektsiooni, hierarhia ja oodatud leheformaadi tüüp.
Tüübid | [Skeemi][schema] | Konnektori ruumi, mis on eksporditud skeem.

Skripti peaks naaske mis tahes väljundi tulemas.

**Andmete eksportimine**  
Sünkroonimise teenuse kõned andmete eksportimine skripti ootel eksportimisel vajalik arv kordi. Kui konnektori ruumi on veel pooleli ekspordi kui saatja lehe suurus, ekspordi andmete skripti võivad kutsuda mitu korda ja võib-olla mitu korda sama objekti.

Andmete eksportimine skripti saab järgmisi konnektor:

Nimi | Andmetüüp | Kirjeldus
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][string, [ConfigParameter][cp]] | Tabel konnektori konfiguratsiooni parameetrid.
Mandaadi | [PSCredential][pscred] | Sisaldab mis tahes identimisteabe sisestatud ühenduvuse menüü administraator.
CSEntries | IListi[CSEntryChange][csec] | Ekspordi töötlemise ajal see pass ootel objektide konnektor ruumi loendis.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Skripti teavitab ekspordi käivitada (delta või täisversioon) sektsiooni, hierarhia ja oodatud leheformaadi tüüp.
Tüübid | [Skeemi][schema] | Konnektori ruumi, mis on eksporditud skeem.

Andmete eksportimine skripti peab andma on [PutExportEntriesResults] [ peeres] tulemas objekti. See objekt pole vaja sisaldavad tulemi teavet iga eksporditud Connectori, kui ilmneb tõrge või muudatuse ankur atribuut. Näiteks tulemas PutExportEntriesResults objekti naasmiseks järgmist.`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**Lõpeta eksportimine**  
Ekspordi käivitada, või End eksportimine skripti lõppedes. See skript peaks tehke Kettapuhastus toiminguid vajalikud (nt Sule ühendused süsteemid) ja vasta ebaõnnestumist.

Lõpeta ekspordi skripti saab järgmisi konnektor:

Nimi | Andmetüüp | Kirjeldus
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][string, [ConfigParameter][cp]] | Tabel konnektori konfiguratsiooni parameetrid.
Mandaadi | [PSCredential][pscred] | Sisaldab mis tahes identimisteabe sisestatud ühenduvuse menüü administraator.
OpenExportConnectionRunStep | [OpenExportConnectionRunStep][oecrs] | Skripti teavitab ekspordi käivitada (delta või täisversioon) sektsiooni, hierarhia ja oodatud leheformaadi tüüp.
CloseExportConnectionRunStep | [CloseExportConnectionRunStep][cecrs] | Skripti teavitab ekspordi lõppes põhjuse kohta.

Skripti peaks naaske mis tahes väljundi tulemas.

#### <a name="password-synchronization"></a>Parooli sünkroonimine
Windows PowerShelli konnektorid saab kasutada siht parooliga muutuste/lähtestatakse.

Skripti parooli saab järgmisi konnektor:

Nimi | Andmetüüp | Kirjeldus
--- | --- | ---
ConfigParameters | [KeyedCollection][keyk][string, [ConfigParameter][cp]] | Tabel konnektori konfiguratsiooni parameetrid.
Mandaadi | [PSCredential][pscred] | Sisaldab mis tahes identimisteabe sisestatud ühenduvuse menüü administraator.
Partition | [Partition][part] | Kataloogi sektsiooni, mis on selle CSEntry.
CSEntry | [CSEntry][cse] | Konnektori ruumi kirje objekti, mis on saanud parooli muutmine või lähtestamine.
ValidateOperationType | String | Näitab, kas toiming on reset (**SetPassword**) või muuta (**parooli muutmine**).
PasswordOptions | [PasswordOptions][pwdopt] | Lipud, mis määravad ettenähtud parooli lähtestada käitumine. See parameeter on saadaval, kui ValidateOperationType on **SetPassword**ainult.
OldPassword | String | Koos objekti vana parool parool muudatuste kasutajanimedega. See parameeter on ainult saadaval, kui ValidateOperationType on **parooli muutmine**.
Uusparool | String | Täidetud objekti uus parool, mis on soovitatav määrata skripti abil.

Skripti parooli ei peaks tulemeid Windows PowerShelli kohaletoimetamisel. Tõrke ilmnemisel skript parooli skript peaks põhjustada ühte teavitada sünkroonimise teenuse probleemi järgmised erandid:

- [PasswordPolicyViolationException] [ pwdex1] – visati, kui parool ei vasta parooli poliitika ühendatud süsteemis.
- [PasswordIllFormedExceptionenam] [ pwdex2] – visati, kui parool ei ole lubatud ühendatud süsteemi.
- [PasswordExtension] [ pwdex3] – kõik muud vead parooli script visati.

## <a name="sample-connectors"></a>Valimi konnektorid
Saadaval valimi konnektorid täieliku ülevaate teemast [Windows PowerShelli konnektor valimi konnektor saidikogumi][samp].

## <a name="other-notes"></a>Muud märkmed

### <a name="additional-configuration-for-impersonation"></a>Täiendavad isikustamine konfigureerimine
Andke kasutaja kehastuda järgmised õigused sünkroonimise teenuse server:

Järgmistes registrivõtmetes lugemisõigus:

- HKEY_USERS\\\Software\Microsoft\PowerShell [SynchronizationServiceServiceAccountSID]
- HKEY_USERS\\\Environment [SynchronizationServiceServiceAccountSID]

Turvalisuse identifikaator (SID) sünkroonimise teenuse teenusekonto määramiseks käivitage PowerShelli järgmised käsud:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Lugemisõigus failisüsteemi järgmised kaustad:

- %ProgramFiles%\Microsoft forefront identiteedi Manager\2010\Synchronization Service\Extensions
- %ProgramFiles%\Microsoft forefront identiteedi Manager\2010\Synchronization Service\ExtensionsCache
- %ProgramFiles%\Microsoft forefront identiteedi Manager\2010\Synchronization Service\MaData\\{ConnectorName}

Asendab Windows PowerShelli konnektor nime kohatäide {ConnectorName}.

## <a name="troubleshooting"></a>Tõrkeotsing

-   Konnektor tõrkeotsingu logimine lubamise kohta leiate artiklist [Kuidas lubada ETW jälitamine konnektorid](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291
