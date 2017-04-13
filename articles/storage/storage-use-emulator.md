<properties 
    pageTitle="Azure'i salvestusruumi emulaator kasutamiseks katsetamine | Microsoft Azure'i" 
    description="Azure'i salvestusruumi emulaator pakub tasuta kohaliku arenduskeskkond arendamise ja testimise Azure Storage vastu. Lisateavet mäluruumi emulaator, sh kuidas taotluste autentimise, kuidas luua ühendus oma emulaator ja käsurea tööriista kasutamine." 
    services="storage" 
    documentationCenter="" 
    authors="tamram" 
    manager="carmonm" 
    editor="tysonn"/>
<tags 
    ms.service="storage" 
    ms.workload="storage" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/07/2016" 
    ms.author="tamram"/>

# <a name="use-the-azure-storage-emulator-for-development-and-testing"></a>Azure'i salvestusruumi emulaator kasutamiseks katsetamine

## <a name="overview"></a>Ülevaade

Microsoft Azure'i salvestusruumi emulaator pakub kohaliku keskkond, mis jäljendab Azure'i bloobimälu, järjekord ja tabel teenuste arendamiseks. Salvestusruumi emulaator kasutamisel saate testida rakenduse salvestusruumi teenuseid, esitada ilma loomine Azure tellimuse või kannavad kõik kulud. Kui olete rahul, kuidas teie rakendus töötab emulaator, mida saab vahetada pilveteenuses Azure storage konto abil.

> [AZURE.NOTE] Salvestusruumi emulaator on saadaval [Microsoft Azure'i SDK](https://azure.microsoft.com/downloads/)osana. Saate installida ka salvestusruumi emulaator [autonoomse Installeri](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409)abil. Salvestusruumi emulaator konfigureerimiseks peab teil olema arvutis administraatori õigusi.
> 
> Salvestusruumi emulaator praegu töötab ainult Windows.
>  
> Pange tähele, et andmete salvestusruumi emulaator versioonis loodud on tagatud kättesaadavaks, kui kasutate mõnda muud versiooni. Kui teil on vaja püsida andmete pikaajaliselt, on soovitatav seda andmete talletamiseks konto Azure salvestusruum, mitte salvestusruumi emulaator.

## <a name="how-the-storage-emulator-works"></a>Salvestusruumi emulaator tööpõhimõtted
 
Salvestusruumi emulaator kasutab kohaliku Microsoft SQL serveri eksemplar ja kohaliku failisüsteemi jäljendada Azure storage teenused. Vaikimisi kasutab Microsoft SQL Server 2012 Express LocalDB salvestusruumi emulaator andmebaasi.  Saate konfigureerida emulaator salvestusruumi juurde pääseda SQL serveri kohaliku eksemplari asemel LocalDB eksemplari. Lisateabe saamiseks vaadake [algus- ja lähtestamise salvestusruumi emulaator](#start-and-initialize-the-storage-emulator) all.

Saate installida SQL Server Management Studio Express LocalDB installi haldamine. Salvestusruumi emulaator loob ühenduse SQL serveri või Windowsi autentimist kasutades LocalDB. 

Teatud funktsioonide erinevused salvestusruumi emulaator ja Azure storage teenuste vahel. Erinevuste kohta leiate lisateavet teemast [salvestusruumi emulaator ja Azure Storage erinevused](#differences-between-the-storage-emulator-and-azure-storage).

## <a name="authenticating-requests-against-the-storage-emulator"></a>Taotlusi vastu salvestusruumi emulaator autentimine

Nagu ka Azure Storage pilves, iga taotluse vastu salvestusruumi emulaator tehtud peavad olema kinnitatud, kui see on anonüümse taotluse. Saate autentida taotlusi salvestusruumi emulaator jagatud võtme autentimist kasutades või ühiskasutatava Accessi allkirjastatud (SAS).

### <a name="authentication-with-shared-key-credentials"></a>Ühiskasutusse antud klahvi mandaadiga autentimine

[AZURE.INCLUDE [storage-emulator-connection-string-include](../../includes/storage-emulator-connection-string-include.md)]

Ühendusstringi kohta leiate lisateavet teemast [Konfigureerimine Azure'i salvestusruumi ühendusstringi](storage-configure-connection-string.md). 

### <a name="authentication-with-a-shared-access-signature"></a>Ühiskasutatava Accessi allkirjastatud autentimine 

Mõned Azure salvestusruumi kliendi teegid, nt Xamarin teegi toetavad ainult ühiskasutusega juurdepääsu allkirja (SAS) luba autentimine. Peate selle SAS märgiks tööriista või rakendus, mis toetab jagatud võtme autentimise abil luua. Azure'i PowerShelli kaudu on lihtne viis luua SAS luba:

1. Kui te pole seda veel teinud, installige Azure'i PowerShelli. On soovitatav kasutada Azure PowerShelli cmdletid uusim versioon. Vaadake, [Kuidas installida ja konfigureerida Azure PowerShelli](../powershell-install-configure.md#Install) installimisjuhiseid.

2. Avage Azure'i PowerShelli ja käivitage järgmised käsud. Ärge unustage *ACCOUNT_NAME* ja *ACCOUNT_KEY ==* oma mandaati. Asendage *CONTAINER_NAME* nimi enda valitud Meilikausta.

        $context = New-AzureStorageContext -StorageAccountName "ACCOUNT_NAME" -StorageAccountKey "ACCOUNT_KEY=="
        
        New-AzureStorageContainer CONTAINER_NAME -Permission Off -Context $context
        
        $now = Get-Date 
        
        New-AzureStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri

Tulemuseks ühiskasutusega juurdepääsu signatuur URI uus ümbris peaks olema järgmine:

    https://storageaccount.blob.core.windows.net/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss

Ühiskasutusega juurdepääsu allkirja, mis on loodud selles näites on üks päev. Allkirja annab täielik juurdepääs (st lugemine, kirjutamine, kustutamine, loendi) plekid pakendi sees.

Ühiskasutusega juurdepääsu allkirjade kohta leiate lisateavet teemast [Kaudu ühiskasutusse antud juurdepääs allkirjade (SAS)](storage-dotnet-shared-access-signature-part-1.md).


## <a name="start-and-initialize-the-storage-emulator"></a>Käivitage ja salvestusruumi emulaator lähtestamine

Azure'i salvestusruumi emulaator käivitamiseks valige nupp Start või vajutage Windowsi klahvi. Hakake tippima **Azure salvestusruumi emulaator**ja valige emulaator rakenduste loendist. 

Kui emulaator töötab, kuvatakse Windowsi tegumiriba olekualal ikooni.

Salvestusruumi emulaator käivitamisel käsurea aken. Saate kasutada käsurea akna käivitamine ja peatamine salvestusruumi emulaator samuti andmete kustutamine, saada praegune olek ja emulaator lähtestada. Lisateavet leiate teemast [Salvestusruumi emulaator käsurea tööriista juhend](#storage-emulator-command-line-tool-reference).

Käsurea akna sulgemisel salvestusruumi emulaator jätkuvalt käivitada. Avab käsurea uuesti, järgige eespool antud juhiseid, kui alates salvestusruumi emulaator.

Esimest korda käivitada salvestusruumi emulaator, kohalikku keskkond on lähtestatud teie eest. Lähtestamisel loob andmebaasi LocalDB ja jätab HTTP pordid kohalikku iga teenuse jaoks. 

Salvestusruumi emulaator on installitud vaikimisi C:\Program Files (x86) \Microsoft SDKs\Azure\Storage emulaator kataloogi. 

### <a name="initialize-the-storage-emulator-to-use-a-different-sql-database"></a>Salvestusruumi emulaator erinevate SQL-andmebaasi kasutama lähtestamine

Salvestusruumi emulaator käsurea tööriista abil saate lähtestada salvestusruumi emulaator osutamiseks SQL-i andmebaasi eksemplari peale vaikimisi LocalDB eksemplari. Võtke arvesse, et peate kasutama käsurea tööriista käivitamine on Tagaandmebaasi salvestusruumi emulaatori administraatoriõigustega.

1. Klõpsake nuppu **Start** või vajutage **Windowsi** klahvi. Hakake tippima `Azure Storage Emulator` ja valige see kuvamisel avab salvestusruumi emulaator käsurea tööriist.
2. Tippige järgmine käsk käsuviiba aken, kus `<SQLServerInstance>` on SQL Serveri eksemplari nimi. LocalDb kasutamiseks määrake `(localdb)\v11.0` nimega SQL serveri eksemplar.

        AzureStorageEmulator init /server <SQLServerInstance> 
    
    Samuti saate kasutada vaikimisi SQL serveri eksemplar emulaator suunab järgmine käsk:

        AzureStorageEmulator init /server .\\ 

    Või kasutage uuesti lähtestab andmebaasi vaikimisi LocalDB eksemplari järgmine käsk:

        AzureStorageEmulator init /forceCreate 

Need käsud kohta leiate lisateavet teemast [Salvestusruumi emulaator käsurea tööriista viide](#storage-emulator-command-line-tool-reference).

## <a name="addressing-resources-in-the-storage-emulator"></a>Salvestusruumi emulaator adresseerimise ressursid

Teenuse lõpp-punktide salvestusruumi emulaator erinevad Azure storage konto. Erinevus on, et kohaliku arvuti ei täida domeeninimede lahendamine, nii salvestusruumi emulaator lõpp-punktid nõuda kohalik aadress, mitte domeeni nimi.

Kui te aadressi ressursi Azure storage konto, saate kava, kus konto nimi on osa URI hosti nimi ja ressursside, mis on adresseeritud on osa URI tee.

    <http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>

Näiteks järgmised URI on kehtiv aadress on bloobimälu Azure storage konto jaoks.

    https://myaccount.blob.core.windows.net/mycontainer/myblob.txt

Salvestusruumi emulaator, kuna kohaliku arvuti ei täida domeeninimede lahendamine, konto nimi on osa URI tee asemel hosti nimi. Järgmine skeem töötab salvestusruumi emulaator ressursi kasutamine

    http://<local-machine-address>:<port>/<account-name>/<resource-path>

Näiteks võib kasutada järgmisel aadressil juurdepääsuks on bloobimälu salvestusruumi emulaator:

    http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt

Teenuse lõpp-punktide salvestusruumi emulaator on:

    Blob Service: http://127.0.0.1:10000/<account-name>/<resource-path>
    Queue Service: http://127.0.0.1:10001/<account-name>/<resource-path>
    Table Service: http://127.0.0.1:10002/<account-name>/<resource-path>

### <a name="addressing-the-account-secondary-with-ra-grs"></a>Teisene koos RA-GRS konto tegelemine

Alustades versioon 3,1, salvestusruumi emulaator konto toetab lugemisõigus – geograafilise liigne dispersioonanalüüs (RA GRS). Salvestusruumi ressursid pilveteenuste ja kohalik emulaator, pääsete teisene asukoht näiva lisades - teisene konto nimi. Näiteks võib bloobimälu, mis on kirjutuskaitstud teisese kasutamine salvestusruumi emulaator juurdepääsuks kasutada järgmisel aadressil:

    http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt 

> [AZURE.NOTE] Teisese salvestusruumi emulaator Programmeerimisjuurdepääs, kasutage salvestusruumi kliendi teek .net-i 3,2 või uuem versioon. Lugege teemat [Microsoft Azure'i salvestusruumi kliendi teek .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) üksikasju.

## <a name="storage-emulator-command-line-tool-reference"></a>Salvestusruumi emulaator käsurea tööriista viide

Versiooni 3.0, alates salvestusruumi emulaator käivitamisel, näete mõne käsurea akna ilmub. Käivitamine ja peatamine emulaator ka päringu oleku ja muude toimingute sooritamiseks käsurea akna abil.

> [AZURE.NOTE] Kui teil on Microsoft Azure'i arvutada emulaator installitud, kuvatakse ikooni süsteem salvestusruumi emulaator käivitamisel. Paremklõpsake menüü, mis pakub graafilise käivitamine ja peatamine salvestusruumi emulaator kuvamiseks ikooni.

### <a name="command-line-syntax"></a>Käsurea süntaks

    AzureStorageEmulator [start] [stop] [status] [clear] [init] [help]

### <a name="options"></a>Suvandid

Suvandite loendi kuvamiseks tippige `/help` käsuviibale.

| Suvand | Kirjeldus                                                    | Käsk                                                                                                 | Argumendid                                                                                                         |
|--------|----------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------|
| **Alustamine**  | Käivitub salvestusruumi emulaator.                                | `AzureStorageEmulator start [-inprocess]`                                                                    | *-protsessi käigus*: alustada emulaator praeguse protsessi uue loomise asemel.                          |
| **Stopp**   | Tabelduskohtade salvestusruumi emulaator.                                    | `AzureStorageEmulator stop`                                                                                  |                                                                                                                   |
| **Olek** | Prindib salvestusruumi emulaator olekut.                     | `AzureStorageEmulator status`                                                                                |                                                                                                                   |
| **Eemalda**  | Kustutab määratud käsureal kõigi teenuste andmeid. | `AzureStorageEmulator clear [blob] [table] [queue] [all]                                                    `| *bloobimälu*: puhastab Bloobivahemälu andmed. <br/>*järjekorda*: kustutab järjekorda andmed. <br/>*Tabel*: kustutab tabeli andmeid. <br/>*Kõik*: kustutab kõik andmed kõik teenused. |
| **Käivitamise**   | Teostab ühekordse lähtestamine emulaator häälestamiseks.       | `AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate] [-inprocess]` | *-server serverName\instanceName*: määrab majutava eksemplar SQL serveri. <br/>*-sqlinstance instanceName*: määrab SQL-i eksemplari kasutada vaikimisi serveri eksemplari nimi. <br/>*-forcecreate*: SQL-andmebaasi loomine jõustab isegi juhul, kui see on juba olemas. <br/>*-protsessi käigus*: teostab lähtestamine praeguse protsessi asemel kudemise uue. Praeguse toimingu täitmiseks lähtestamine laiendatud õigustega tuleb käivitada.          |
                                                                                                                  
## <a name="differences-between-the-storage-emulator-and-azure-storage"></a>Salvestusruumi emulaator ja Azure Storage erinevused

Kuna salvestusruumi emulaator on ka Emuleeritud keskkond, mis töötab kohaliku SQL-i eksemplari, on mõned erinevused funktsioonid emulaator ja Azure storage konto pilveteenuses.

- Salvestusruumi emulaator toetab ainult ühe kindla konto ja klahvi tuntud autentimist.

- Salvestusruumi emulaator pole scalable salvestusteenus ja suure hulga samaaegseid kliendid ei toeta.

- [Salvestusruumi emulaator toimetulek ressursside](#addressing-resources-in-the-storage-emulator)kirjeldatud ressursid käsitletakse teisiti salvestusruumi emulaator ja Azure storage konto. See vahe on, et domeeni nimelahendus on saadaval pilves, kuid mitte kohalikus arvutis.

- Alustades versioon 3,1, salvestusruumi emulaator konto toetab lugemisõigus – geograafilise liigne dispersioonanalüüs (RA GRS). Kõik kontod on lubatud RA-GRS emulaator, ja pole kunagi mingi lag vahel esmaseid ja teiseseid koopiad. Saada bloobimälu teenuse statistika, saada järjekorda teenuse statistika ja saada tabeli teenuse argument statistikud on toetatud teisene konto ja tagastab alati väärtus on `LastSyncTime` vastuse elemendi praeguse kellaaja järgi aluseks oleva SQL-andmebaasi nimega.

    Teisese salvestusruumi emulaator Programmeerimisjuurdepääs, kasutage salvestusruumi kliendi teek .net-i 3,2 või uuem versioon. Lugege teemat [Microsoft Azure'i salvestusruumi kliendi teek .NET](https://msdn.microsoft.com/library/azure/dn261237.aspx) üksikasju.

- Faili teenuseid ja SMB Protocol (protokoll) teenuse lõpp-punktid on toetatud praegu salvestusruumi emulaator.

- Kui kasutate versiooni salvestusruumi teenuseid, mis ei toeta veel Office'i versiooni te kasutate emulaator, tagastab salvestusruumi emulaator VersionNotSupportedByEmulator tõrge (HTTP olekukoodi 400 - vigane päring).

### <a name="differences-for-blob-storage"></a>Erinevused bloobimälu 

Järgmised erinevused bloobimälu emulaator rakendamiseks tehke järgmist.

- Salvestusruumi emulaator ainult toetab bloobimälu suurused kuni 2 GB.

- Sellele bloobimälu toiming võib õnnestuda vastu bloobimälu, mis on aktiivne üürilepingu salvestusruumi emulaator on olemas ja isegi juhul, kui üürilepingu ID pole määratud taotluse osana. 

- Lisa bloobimälu ei toetata emulaator. Proovite mõne lisa bloobimälu toiming tagastab FeatureNotSupportedByEmulator tõrge (HTTP olekukoodi 400 - vigane päring).

### <a name="differences-for-table-storage"></a>Tabeli Storage erinevused 

Järgmised erinevused tabelimälu emulaator rakendamiseks tehke järgmist.

- Kuupäevaatribuutide tabeli teenus salvestusruumi emulaator toetab ainult SQL Server 2005 ei toeta vahemikus (*ehk*nad on nõutav hilisem kui 1 Jaanuar 1753). Kõik kuupäevad enne 1 Jaanuar 1753 muudetakse see väärtus. Kuupäevade täpsuse on piiratud täpsuse SQL Server 2005, mis tähendab, et kuupäevad on täpne 1/300th teise.

- Salvestusruumi emulaator toetab sektsiooni võti ja rea võtme kinnisvarahindade on väiksem kui 512 baiti. Lisaks konto nimi, tabeli nimi ja tootenumbri atribuutide nimesid koos kogumaht ei tohi ületada 900 baiti.

- Piiratud väiksem kui 1 MB salvestusruumi emulaator tabeli rea kogumaht.

- Salvestusruumi emulaator, tippige andmed atribuudid `Edm.Guid` või `Edm.Binary` toetab ainult selle `Equal (eq)` ja `NotEqual (ne)` võrdlusmärgid päringus filtreerimine stringide.

### <a name="differences-for-queue-storage"></a>Järjekorda Storage erinevused

Puuduvad erinevused teatud järjekorda talletusruumi emulaator.

## <a name="storage-emulator-release-notes"></a>Salvestusruumi emulaator väljalaskemärkmed

### <a name="version-45"></a>Versioon 4.5

- Fikseeritud vea, mis põhjustas lähtestamine ja installi salvestusruumi emulaator nurjuda, kui varundamise andmebaasi ümber.

### <a name="version-44"></a>Versiooni 4.4

- Salvestusruumi emulaator toetab nüüd salvestusruumi teenuse versioon 2015-12-11 bloobimälu, järjekord ja tabel teenuse lõpp-punktid.

- Suurte arvude plekid praegu salvestusruumi emulaator Prügikoristus bloobimälu andmete tõhusamaks.

- Fikseeritud vea, mis põhjustas container ACL XML-i kaudu soovitud salvestusteenus kuidas see valideeritakse pisut teisiti.

- Fikseeritud viga, mis põhjustas mõnikord max ja min kuupäeva ja kellaaja väärtuste esitatakse vale ajavööndi.

### <a name="version-43"></a>Versiooni 4.3

- Salvestusruumi emulaator toetab nüüd salvestusruumi teenuse versioon 2015-07-08 bloobimälu, järjekord ja tabel teenuse lõpp-punktid.

### <a name="version-42"></a>Versioon 4.2

- Salvestusruumi emulaator toetab nüüd salvestusruumi teenuse versioon 2015-04-05 bloobimälu, järjekord ja tabel teenuse lõpp-punktid.

### <a name="version-41"></a>Versioon 4.1

- Salvestusruumi emulaator toetab nüüd versioon 2015-02-21 salvestusruumi teenust bloobimälu, järjekord ja tabel teenuse lõpp-punktid, välja arvatud bloobimälu lisab uusi funktsioone. 

- Salvestusruumi emulaator nüüd tagastab mõtestatud tõrketeade, kui kasutate versiooni salvestusruumi teenuseid, mis on veel versioon ei toeta seda emulaator. Soovitame kasutada emulaator uusim versioon. Kui teil tekib VersionNotSupportedByEmulator tõrge (HTTP olekukoodi 400 - vigane päring), laadige salvestusruumi emulaator uusim versioon.

- Fikseeritud vea kus võistluse seisund, tabeli üksuse andmed olla valed samaaegseid kirjakooste ajal.

### <a name="version-40"></a>Versiooni 4.0

- Executable salvestusruumi emulaator on ümber *AzureStorageEmulator.exe*abil.

### <a name="version-32"></a>Versiooni 3,2

- Salvestusruumi emulaator toetab nüüd salvestusruumi teenuse versioon 2014-02-14 bloobimälu, järjekord ja tabel teenuse lõpp-punktid. Pange tähele, et faili teenuse lõpp-punktid ei toeta praegu salvestusruumi emulaator. Leiate [Azure'i salvestusruumi teenuste versioonimise](https://msdn.microsoft.com/library/azure/dd894041.aspx) üksikasjade versiooni 2014-02-14.

### <a name="version-31"></a>Versiooni 3,1

- Lugemisõigus – geograafilise liigne salvestusruumi (RA GRS) toetab nüüd salvestusruumi emulaator. Saada bloobimälu teenuse statistika, saada järjekorda teenuse statistika ja saada tabeli teenuse statistika API on toetatud teisene konto ja alati tagasi praeguse kellaaja järgi aluseks oleva SQL-andmebaasi nimega LastSyncTime vastuse elemendi väärtuse. Teisese salvestusruumi emulaator Programmeerimisjuurdepääs, kasutage salvestusruumi kliendi teek .net-i 3,2 või uuem versioon. Üksikasjad leiate Microsoft Azure'i salvestusruumi kliendi teek .net-i viide.

### <a name="version-30"></a>Versiooni 3.0

- Azure'i salvestusruumi emulaator ei ole enam saadetud sama nimega Arvuta emulaator pakkimine.

- Salvestusruumi emulaator graafilist liidest on aegunud kasuks mõne käsurea Skriptattavat liides. Käsurea liides kohta leiate teemast salvestusruumi emulaator käsurea tööriista viide. Graafilist liidest on jätkuvalt Esita versiooni 3.0, kuid see pääseb juurde ainult arvutada emulaator, paremklõpsates süsteemi salve ikooni ja valides Kuva salvestusruumi emulaator UI installimisel.

- Versiooni 2013 – 08-15 Azure storage teenused on nüüd täielikult toetatud. (Varem selles versioonis ainult toetas salvestusruumi emulaator versiooni 2.2.1 Preview.)
