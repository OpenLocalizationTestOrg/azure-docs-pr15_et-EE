<properties
   pageTitle="Üldine SQL-i konnektor | Microsoft Azure'i"
   description="Selles artiklis kirjeldatakse, kuidas konfigureerida Microsofti üldise SQL-i konnektor."
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

# <a name="generic-sql-connector-technical-reference"></a>Üldine SQL-i konnektor tehniline teave

Selles artiklis kirjeldatakse üldise SQL-i konnektor. See artikkel kehtib järgmiste toodete:

- Microsofti Identity Manager 2016 (MIM2016)
- Forefront Identity Manager 2010 R2 (FIM2010R2)
    -   Peate kasutama kiirparandus 4.1.3671.0täiendama või uuem versioon [KB3092178](https://support.microsoft.com/kb/3092178).

MIM2016 ja FIM2010R2, konnektor on saadaval [Microsofti allalaadimiskeskusest](http://go.microsoft.com/fwlink/?LinkId=717495)alla laadida.

Selle konnektori tegelikkuses vaatamiseks artiklist [Üldise SQL-i konnektor samm-sammult](active-directory-aadconnectsync-connector-genericsql-step-by-step.md) .

## <a name="overview-of-the-generic-sql-connector"></a>Üldine SQL-i konnektor ülevaade

Üldine SQL-i konnektor võimaldab teil andmebaasi süsteemi, mis pakub ODBC ühenduvuse sünkroonimise teenuse integreerida.  

Praeguse versiooni konnektor toetavad kõrgetasemelisi seisukohalt järgmised funktsioonid:

Funktsioon | Tugi
--- | ---
Ühendatud andmeallikas | Konnektor on toetatud kõik 64-bitine ODBC draiverid. See on testitud järgmist: <li>Microsoft SQL Server ja SQL Azure'i</li><li>IBM DB2 10.x</li><li>IBM DB2 9.x</li><li>Oracle 10 ja 11 g</li><li>MySQL-i 5.x</li>
Stsenaariumid   | <li>Objekti elutsükli haldus</li><li>Parooli haldus</li>
Toimingud | <li>Täielik impordi- ja Delta importimine, eksportimine</li><li>Ekspordiks: Lisamine, kustutamine, värskendamine ja asendamine</li><li>Määra parool, parooli muutmine</li>
Skeemi | <li>Dünaamiliste leitavust objektid ja atribuudid</li>

### <a name="prerequisites"></a>Eeltingimused
Enne konnektor kasutamiseks veenduge, et teil on sünkroonimise server järgmist.

- Microsoft .NET 4.5.2 Frameworki või uuem versioon
- 64-bitine ODBC kliendi draiverid

### <a name="permissions-in-connected-data-source"></a>Ühendatud andmeallika õigused
Luua või toetatud ülesandeid täita üldine SQL-i konnektor, peab teil olema:

- db_datareader
- db_datawriter

### <a name="ports-and-protocols"></a>Pordid ja protokollid
Portide, mis on vaja ODBC-draiver töötamiseks, lugege andmebaasi tootja dokumentatsiooni.

## <a name="create-a-new-connector"></a>Looge uus konnektor
Üldine SQL-i konnektori loomiseks **Sünkroonimise teenuse** valige **Haldamise Agent** ja **loomine**. Valige **Üldine SQL-i (Microsoft)** konnektor.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/createconnector.png)

### <a name="connectivity"></a>Ühenduvus
Konnektor kasutab ODBC DSN-i faili Ühenduvus. **ODBC-andmeallikate** leitud jaotises **Haldusriistad**menüü start kaudu faili DSN-i luua. Haldus tööriista luua **Faili DSN-i** , et see saab esitada konnektor.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericsql/connectivity.png)

Ühenduvus Kuva on esimene, kui loote uue üldise SQL-i konnektori. Peate esmalt järgmist teavet:

- DSN-faili tee
- Autentimine
    - Kasutajanimi
    - Parooli

Andmebaasi toetama üht järgmistest meetoditest autentimist:

- **Windowsi autentimine**: autentiva andmebaasi kasutab Windowsi identimisteabe kasutaja kinnitamiseks. Kasutaja nimi või parool määratud kasutatakse autentimiseks andmebaas. Selle konto peab andmebaas õigused.
- **SQL-i autentimise**: autentiva andmebaasi kasutab kasutaja nimi või parool määratletud ühte ühenduvuse ekraani andmebaasiga ühenduse loomiseks. Kui salvestate faili DSN-i kasutaja nimi ja parool, on esitatud Ühenduvus ekraanil identimisteabe järjestuse.
- **Azure'i SQL-andmebaasi autentimine**: lisateabe saamiseks vt [ühenduse loomine SQL-i andmebaasi, kasutades Azure Active Directory autentimise](..\sql-database\sql-database-aad-authentication.md).

**DN on ankur**: kui valite selle suvandi, DN kasutatakse ka ankur atribuuti. Saate lihtsa rakendamiseks kasutada, kuid on ka järgmised piirangud:

-   Konnektor toetab ainult ühe objekti tüüp. Seetõttu ainult viitamiseks mis tahes viide atribuute sama objekti tüüp.

**Ekspordi tüüp: objekti Asenda**: ekspordi ajal kui ainult osa atribuute on muudetud, kogu objekti kõik atribuudid on eksporditud ja asendab olemasoleva objekti.

### <a name="schema-1-detect-object-types"></a>Skeemi 1 (tuvasta objekti tüübid)
Sellel lehel te ei kavatse konfigureerimine, kuidas leida muu objekti tüübid andmebaasi saab konnektor.

Iga objekti tüüp on esitatud sektsiooni ja konfigureerinud **konfigureerimine sektsioonid ja hierarhiad**edasi.

![schema1a](./media/active-directory-aadconnectsync-connector-genericsql/schema1a.png)

**Objekti tüüp tuvastamise meetod**: The konnektor toetab neid objekti tüüp tuvastamise viise.

- **Fikseeritud väärtus**: esitate loendis objekti tüübid komaga eraldatud loend. Näide: `User,Group,Department`.  
![schema1b](./media/active-directory-aadconnectsync-connector-genericsql/schema1b.png)
- **Tabel/vaade/salvestatud protseduur**: tabel/vaade/salvestatud toimingu nime ja seejärel objekti failitüüpide loendit sisaldava veeru nimi. Kui kasutate salvestatud protseduur, siis pakuvad ka parameetrite seda vormingus **[nimi]: [suunas]: [väärtus]**. Sisestage iga parameetri eraldi real (kasutage klahvikombinatsiooni Ctrl + Enter saada uut rida).  
![schema1c](./media/active-directory-aadconnectsync-connector-genericsql/schema1c.png)
- **SQL-päringu**: see suvand võimaldab saata SQL-päringu, mis tagastab ühe veeru koos objekti tüübid, näiteks `SELECT [Column Name] FROM TABLENAME`. Tagastatud veerg peab olema tüüp string (varchar).

### <a name="schema-2-detect-attribute-types"></a>Skeemi 2 (tuvasta atribuut tüüpi)
Sellel lehel te ei kavatse konfigureerimine, kuidas atribuut nimed ja tüübid ei kavatse tuvastada. Konfiguratsiooni suvandid on loetletud iga tuvastanud eelmise lehekülje objekti tüüp.

![schema2a](./media/active-directory-aadconnectsync-connector-genericsql/schema2a.png)

**Atribuut tüüp tuvastamise meetod**: The konnektor toetab neid atribuut tüüp tuvastamise viise iga tuvastatud objekti tüüp skeemi 1 ekraani.

- **Tabel/vaade/salvestatud protseduur**: tabel/vaade/salvestatud protseduur, mis tuleks kasutada atribuuti nimede otsimine nimi. Kui kasutate salvestatud protseduur, siis pakuvad ka parameetrite seda vormingus **[nimi]: [suunas]: [väärtus]**. Sisestage iga parameetri eraldi real (kasutage klahvikombinatsiooni Ctrl + Enter saada uut rida). Mitme väärtusega atribuut atribuutide nimed tuvastamiseks pakuvad tabelite või vaadete komaga eraldatud loend. Mitme väärtusega stsenaariumid ei toetata kui ema- ja tütarüksuste tabel on sama veergude nimed.
- **SQL-päringu**: see suvand võimaldab saata SQL-päringu, mis tagastab ühe veeru atribuut nimed, näiteks `SELECT [Column Name] FROM TABLENAME`. Tagastatud veerg peab olema tüüp string (varchar).

### <a name="schema-3-define-anchor-and-dn"></a>Skeemi 3 (ankur Määratle ja DN)
Sellel lehel saate konfigureerida ankur ja DN atribuut iga tuvastatud objekti tüübi. Saate valida mitme atribuudi ankur oleks kordumatu.

![schema3a](./media/active-directory-aadconnectsync-connector-genericsql/schema3a.png)

- Mitme väärtusega ja kahendmuutujaga atribuudid on loetletud.
- Sama atribuuti ei saa kasutada DN ja ankur, kui on valitud **DN pole ankur** ühenduvuse lehe.
- Kui **DN on ankur** ühenduvuse lehel märgitud, on vaja selle lehe DN atribuut. Atribuudi tuleks kasutada ka ankur atribuuti.
![schema3b](./media/active-directory-aadconnectsync-connector-genericsql/schema3b.png)

### <a name="schema-4-define-attribute-type-reference-and-direction"></a>Skeemi 4 (Määratle atribuudi tüüp, viide ja suunas)
Sellel lehel saate konfigureerida atribuudi tüüp, nt täisarv, kahendarvuks, või kahendmuutuja ja iga atribuudi suunas. Lehe **skeemi 2** kõik atribuudid on loetletud, sh mitme väärtusega atribuute.

![schema4a](./media/active-directory-aadconnectsync-connector-genericsql/schema4a.png)

- **Andmetüüp**: vastendada nende tüüpi tuntakse sync engine atribuudi tüüp. Vaikimisi on kasutada sama tüüpi SQL-i skeemi tuvastatud, kuid kuupäeva ja kellaaja ja viide pole kerge vaevaga tuvastatavad. Nende, peate määrama **kuupäeva ja kellaaja** või **viite**.
- **Suund**: saate seada atribuudi suunas importimine, eksportimine või ImportExport. ImportExport on vaikimisi.
![schema4b](./media/active-directory-aadconnectsync-connector-genericsql/schema4b.png)

Märkmed:

- Kui soovitud tüüpi atribuuti ei avastanud konnektor, kasutab ta andmetüüp String.
- **Pesastatud tabelid** saab lugeda ühest veerust andmebaasi tabelid. Oracle'i talletab pesastatud tabeli read ei kindlas järjekorras. Kui pesastatud tabeli toodavate PL/SQL-i muutuja on read esitatud järjestikust allindeksid, alustades 1. Mis annab teile juurdepääsu massiiv nagu eraldi ridadele.
- **VARRYS** ei toeta konnektor.

### <a name="schema-5-define-partition-for-reference-attributes"></a>Skeemi 5 (Määratle sektsioon viide atribuudid)
Sellel lehel saate konfigureerida kõik viide atribuute, millist sektsiooni (objekti tüüp) atribuut viitab.

![schema5](./media/active-directory-aadconnectsync-connector-genericsql/schema5.png)

Kui kasutate **DN on ankur**, peab sama objekti tüüp kui viitate kaudu kasutada. Te ei saa viidata mõne muu objekti tüüp.

### <a name="global-parameters"></a>Globaalne parameetrid
Lehe globaalne parameetrite abil konfigureerida Delta impordi, kuupäev ja kellaaeg vormingus ja parooli meetod.

![globalparameters1](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters1.png)

Üldine SQL-i konnektor toetab Delta importimiseks järgmistest meetoditest.

- **Päästik**: vt [genereerimine Delta vaadete abil](https://technet.microsoft.com/library/cc708665.aspx).
- **Vesimärgi**: üldise lähenemisviisi, mida saab kasutada mis tahes andmebaasi. Vesimärgi päring on juba eelnevalt täidetud põhjal andmebaasi pakkuja poole. Vesimärgi veerg peab olema iga tabeli/vaates kasutada. See veerg peab jälgida lisab ja värskendustega nimega tabelid ja selle sõltuvad (mitme väärtusega või laps) tabelid. Kella sünkroonimise teenuse ja andmebaasiserveri vahel peab olema sünkroonitud. Kui ei, siis võib mõni kirje delta importimine välja jäetud.  
Piirangud:
    - Vesimärgi strateegia ei ole tugi kustutatud objektide.
- **Hetktõmmis**: (töötab ainult Microsoft SQL Server) [Loomisel Delta vaadete abil pilte](https://technet.microsoft.com/library/cc720640.aspx)
- **Muutuste jälitamise**: (töötab ainult Microsoft SQL Server) [Muutuste jälitamise kohta](https://msdn.microsoft.com/library/bb933875.aspx)  
Piirangud:
    - Anchor & DN atribuut peab olema valitud objekti tabeli primaarvõtme osa.
    - SQL-päringu ei toetata impordi ja ekspordi muutuste jälitamise käigus.

**Täiendavad parameetrid**: määrake andmebaasi serveri ajavööndit, mis näitab, kus asub teie andmebaasi server. Seda väärtust kasutatakse toetamiseks vormingusse kuupäev ja kellaaeg atribuudid.

Konnektor alati talletab kuupäeva ja kellaaja-UTC-vormingus. Saama õigesti teisendamine kuupäeva ja kellaajad, andmebaasiserveri ja Vorming soovitud ajavöönd peab olema määratud. Vormingu tuleks väljendada .net-vormingus.

Ekspordi ajal tuleb iga kuupäev aja atribuut edastada konnektor UTC aega vormingus.

![globalparameters2](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters2.png)

**Parooli konfigureerimine**: konnektor pakub parooli sünkroonimise ja toetab ning parooli muutmine.

Konnektor pakub toetamiseks parooli sünkroonimine kaks võimalust:

- **Salvestatud protseduur**: see nõuab kahte salvestatud toimingute toetamiseks määramine ja muutmine parool. Tippige kõik parameetrid lisamine ja muutmiseks parooli **Seada parooli SP** ja **Muuta parooli SP** parameetrid vastavalt inimese kohta näide toiming.
![globalparameters3](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters3.png)
- **Parooli laiend**: see nõuab parooli laiend DLL (peate laiend DLL nimi, mis rakendab [IMAExtensible2Password](https://msdn.microsoft.com/library/microsoft.metadirectoryservices.imaextensible2password.aspx) kasutajaliidese). Parooli laiend komplekti peab olema paigutatud laiend kausta, et laadida konnektor DLL käitusajal.
![globalparameters4](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters4.png)

Samuti peate lubama parooli haldamise lehel **Laiend konfigureerimine** .
![globalparameters5](./media/active-directory-aadconnectsync-connector-genericsql/globalparameters5.png)

### <a name="configure-partitions-and-hierarchies"></a>Sektsioonid ja hierarhiad konfigureerimine
Valige lehel sektsioonid ja hierarhiad kõik objekti tüübid. Iga objekti tüüp on oma sektsioonis.

![partitions1](./media/active-directory-aadconnectsync-connector-genericsql/partitions1.png)

Saate alistada, määratletud lehel **Connectivity** või **Globaalne parameetrite** väärtused.

![partitions2](./media/active-directory-aadconnectsync-connector-genericsql/partitions2.png)

### <a name="configure-anchors"></a>Ankrud konfigureerimine
See leht on kirjutuskaitstud, kuna ankur on juba määratletud. Valitud ankur atribuut on alati lisatud objekti tüüp tagada jääb kordumatu üle objekti tüübid.

![Ankrud](./media/active-directory-aadconnectsync-connector-genericsql/anchors.png)

## <a name="configure-run-step-parameter"></a>Käivita samm parameetri konfigureerimine
Käivita profiilid konnektor on konfigureeritud järgmist. Andmete importimisse ja eksportimisse tegelikku tööd teha neid konfiguratsioone.

### <a name="full-and-delta-import"></a>Täielik ning Delta importimine
Üldise konnektor SQL-i tugiteenuste täielik ja Delta impordi abil järgmised võimalused.

- Tabel
- Vaade
- Salvestatud protseduur
- SQL-päringu

![runstep1](./media/active-directory-aadconnectsync-connector-genericsql/runstep1.png)

**Tabel/vaade**  
Importida mitme väärtusega objekti atribuute, peate anda komaga eraldatud tabel/vaate nimi **nimi, Multi-Valued** tabel/vaadetes ja **Liitu tingimus** tingimustele vastav Liitu ema tabeli.

Näide:, Mida soovite importida töötaja objekti ja selle mitme väärtusega atribuute. On kaks tabelit, nimega töötaja (laua) ja osakonna (mitme väärtusega).
Tehke järgmist.

- Tippige **töötaja** **SP/tabel/vaade**.
- Tippige **nimi, Multi-Valued tabelivaates**osakonna poole.
- Tippige töötaja & osakonna vahel ühenduse tingimus **Liitu tingimus**, näiteks `Employee.DEPTID=Department.DepartmentID`.
![runstep2](./media/active-directory-aadconnectsync-connector-genericsql/runstep2.png)

**Salvestatud toimingute**  
![runstep3](./media/active-directory-aadconnectsync-connector-genericsql/runstep3.png)

- Kui teil on palju andmeid, on soovitatav rakendada lehekülgjaotuse oma salvestatud protseduure.
- Teie salvestatud protseduur lehekülgjaotuse toetama, peate esitama käivitamine ja End. Vaadake teemat: [tõhus Piipar suurte andmehulkade kaudu](https://msdn.microsoft.com/library/bb445504.aspx).
- @StartIndexja @EndIndex vastava lehe suurus väärtuse **Konfigureerimine samm** lehel konfigureeritud täitmise ajal asendada. Näiteks kui konnektor toob Esileht ja leheformaadi on seatud 500, sellisel juhul @StartIndex oleks 1 ja @EndIndex 500. Need väärtused suurendada, kui konnektor toob järgmiste lehtede ja muutke soovitud @StartIndex & @EndIndex väärtus.
- Käivitada parameetritega salvestatud protseduur, sisestage parameetrid `[Name]:[Direction]:[Value]` vorming. Sisestage iga parameetri eraldi reale (Kasuta Ctrl + Enter saada uue rea).
- Üldine SQL-i konnektor toetab imporditoimingu lingitud serverid ka Microsoft SQL Server. Kui teavet tuleks tuua tabelist, mis on lingitud serveri, siis tabeli tuleks esitada vormingus:`[ServerName].[Database].[Schema].[TableName]`
- Üldine SQL-i konnektor toetab ainult objektid, mis on sarnane struktuur (nii alias nime ja andmetüübiga tüüp) vahel juhiseid ja -skeemifailid tuvastamise käivitada. Kui valitud objekti skeemi ja esitatud teavet Käivita toimingu juures on erinev, ei saa toeta seda stsenaariumi SQL-i konnektor.

**SQL-päringu**  
![runstep4](./media/active-directory-aadconnectsync-connector-genericsql/runstep4.png)

![runstep5](./media/active-directory-aadconnectsync-connector-genericsql/runstep5.png)

- Mitme tulemi määrata päringud, mis pole toetatud.
- SQL-päringu toetab lehekülgjaotust ja sisestage algus ja End nagu muutuja lehekülgjaotuse toetamiseks.

### <a name="delta-import"></a>Delta importimine
![runstep6](./media/active-directory-aadconnectsync-connector-genericsql/runstep6.png)

Delta impordi konfiguratsiooni jaoks on vaja veidi rohkem konfiguratsiooni võrreldes täielik Import.

- Kui valite päästik või hetktõmmise lähenemisviisi delta muutuste jälitus, seejärel andke Nurjumiskirjet või hetktõmmise andmebaasi **tabeli või hetktõmmise andmebaasi nimi** väljale.
- Samuti peate pakuvad Liitu tingimuse ajalugu ja ema tabeli, näiteks`Employee.ID=History.EmployeeID`
- Tehingu jälgimiseks ema tabeli tabelist ajalugu, peate määrama veeru nimi, mis sisaldab teavet toiming (lisamine/värskendamise ja kustutamise).
- Kui valite Vesimärgi lisamine delta muutuste jälitus, seejärel sisestage veeru nimi, mis sisaldab toiming teave **Vee märgi veeru nimi**.
- Veeru **muutmiseks tippige atribuut** on vaja tüübi muutmine. Selle veeru kaartide muutmine, mis ilmneb primaartabel või mitme väärtuse tabelis Muuda tüüpi delta vaates. See veerg võib sisaldada Modify_Attribute Muuda tüüpi atribuuti taseme muutmine või lisada, muuta, või Kustuta Muuda tüüpi objekti taseme muutmine jaoks. Kui see on midagi muud kui vaikeväärtus lisamine, muutmine, või Kustuta ja seejärel saate määratleda neid väärtusi, selle suvandi abil.

### <a name="export"></a>Eksportimine
![runstep7](./media/active-directory-aadconnectsync-connector-genericsql/runstep7.png)

Üldine SQL-i konnektor tugi ekspordi kasutamine neli toetatud meetodit, näiteks:

- Tabel
- Vaade
- Salvestatud protseduur
- SQL-päringu

**Tabel/vaade**  
Kui valite suvandi tabel/vaade, loob konnektor vastavate päringute teha ekspordi.

**Salvestatud toimingute**  
![runstep8](./media/active-directory-aadconnectsync-connector-genericsql/runstep8.png)

Kui valite suvandi salvestatud protseduur, ekspordi nõuab kolme eri salvestatud toimingute lisamine/värskendamise ja kustutamise toimingute tegemiseks.

- **Lisage SP nimi**: see SP töötab, kui mõni objekt jõuab konnektor vastava tabeli lisamine.
- **Värskendage SP nimi**: see SP töötab, kui mõni objekt jõuab konnektor värskenduse vastava tabeli.
- **Kustutage SP nimi**: see SP töötab, kui mõni objekt on konnektori kustutamiseks vastava tabeli.
- Atribuut on valitud skeemi kasutada salvestatud protseduur parameetri väärtus. Näiteks `EmployeeName: INPUT: @EmployeeName` (EmployeeName on valitud konnektor skeemi ja konnektor asendab vastav väärtus ekspordi tehes samal ajal)
- Parameetritega salvestatud toimingu käivitamiseks pakuvad parameetrite `[Name]:[Direction]:[Value]` vorming. Sisestage iga parameetri eraldi reale (Kasuta Ctrl + Enter saada uue rea).

**SQL-päringu**  
![runstep9](./media/active-directory-aadconnectsync-connector-genericsql/runstep9.png)

Kui valite suvandi SQL-i päring, ekspordi nõuab kolme eri päringute lisamine/värskendamise ja kustutamise toimingute tegemiseks.

- **Päringu lisamine**: see päring töötab, kui mõni objekt jõuab konnektor vastava tabeli lisamine.
- **Päringu värskendamine**: see päring töötab, kui mõni objekt jõuab konnektor värskenduse vastava tabeli.
- **Kustutage päringu**: see päring töötab, kui mõni objekt on konnektori kustutamiseks vastava tabeli.
- Atribuut valitud skeemi kasutada parameetri väärtus päringusse, näiteks`Insert into Employee (ID, Name) Values (@ID, @EmployeeName)`

## <a name="troubleshooting"></a>Tõrkeotsing

-   Konnektor tõrkeotsingu logimine lubamise kohta leiate artiklist [Kuidas lubada ETW jälitamine konnektorid](http://go.microsoft.com/fwlink/?LinkId=335731).
