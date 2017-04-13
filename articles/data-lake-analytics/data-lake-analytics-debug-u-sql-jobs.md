<properties 
   pageTitle="Silumine U-SQL-tööde haldamine | Microsoft Azure'i" 
   description="Saate teada, kuidas silumine U-SQL-i nurjunud tipp Visual Studio abil. " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="mumian" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/02/2016"
   ms.author="jgao"/>



#<a name="debug-c-code-in-u-sql-for-data-lake-analytics-jobs"></a>Silumine C# koodi SQL-U Lake andmeanalüüsi tööde haldamine 

Saate teada, kuidas kasutada Azure andmete Lake Visual Studio tööriistad silumine nurjunud U-SQL-i töö vead sees kasutaja kood. 

Visual Studio tööriist võimaldab kompileeritud kood ja vajalikud tipp andmete kobar ja silumine nurjunud tööd alla laadida.

Suur andmete tavaliselt annab laiendatavuse mudeli kaudu keelte nagu Java, C#, Python jne. Palju need annab piiratud runtime silumine teavet, mis on raske silumine käitusaja tõrgete kohandatud koodi. Funktsiooni nimega "Nurjus tipp silumine" on uusim Visual Studio tööriistad. Selle funktsiooni abil saate alla laadida käitusaja andmed: Azure'i kohaliku töökoha nii, et te saate silumine nurjunud kohandatud C# koodi kasutades sama käitusaja ja täpse sisendandmete pilveteenuses.  Pärast probleemid on fikseeritud, saate uuesti käivitada parandatud koodi Azure tööriistu.

See funktsioon video esitluse leiate [oma kohandatud koodi Azure'i andmeanalüüsi Lake silumine](https://mix.office.com/watch/1bt17ibztohcb).

>[AZURE.NOTE] Visual Studio võib hanguda või ootamatult sulguda, kui teil pole järgmised kaks Windowsi uuendused: [Microsoft Visual C++ 2015 Redistributable värskendus 2](https://www.microsoft.com/download/details.aspx?id=51682), [Universal C käitusaja for Windows](https://www.microsoft.com/download/details.aspx?id=50410&wa=wsignin1.0).


##<a name="prerequisites"></a>Eeltingimused
-   Läinud läbi artikkel [Alustamine](data-lake-analytics-data-lake-tools-get-started.md) .

## <a name="create-and-configure-debug-projects"></a>Loomine ja konfigureerimine silumine projektid

Kui avate nurjunud töö andmete Lake Visual Studio tööriista, saate teatise. Tõrke üksikasjalik teave kuvatakse tõrge menüüs ja akna ülaosas teatis kollast riba. 

![Azure'i andmed Lake Analytics U-SQL-i silumine visual studio allalaadimine tipp](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-download-vertex.png)

**Tipp allalaadimiseks ja silumine lahenduse loomine**

1.  Avage Visual Studio nurjunud U-SQL-i töö.
2.  Klõpsake nuppu ressursid ja Sisestuskeel voogu alla laadida **alla laadida** . Klõpsake nuppu **proovi uuesti** , kui allalaadimine nurjus.
3.  Klõpsake nuppu **Ava** , kui allalaadimine on lõpule jõudnud, kohaliku silumine projekti loomise kohta. Luuakse uus Visual Studio lahendus nimega **VertexDebug** tühja projekti nimega **LocalVertexHost** .

Kui kasutaja määratletud tehtemärke kasutatakse U-SQL-i koodide (Script.usql.cs), peab kasutaja määratletud tehtemärgid koodiga klassi teegi C# projekti loomine ja kaasata projekti VertexDebug lahendus.

Kui olete registreerunud dll assemblereid Lake andmeanalüüsi andmebaasi, peate lisama VertexDebug lahenduse lähtekoodi koosolekutel.
 
Kui olete loonud eraldi C# klassi Raamatukogu U-SQL-koodi ja registreeritud DLL assemblereid Lake andmeanalüüsi andmebaasi, peate lisama allikas C# projekti koosolekutel VertexDebug lahenduse kasutamist.

Mõned harva, kasutate kasutaja määratletud tehtemärgid U-SQL-i koodide (Script.usql.cs) faili algse lahendus. Kui soovite töötada, peate sisaldav lähtekoodi C# teegi loomine ja muutmine paketi nimi klaster kasutajaks. Saate komplekti nimi, mis on registreeritud klaster, märkides ruudu skripti, mis kas teil on installitud klaster. Saate selleks avama U-SQL töö ja klõpsake paanil töö "skript". 

**Lahendus konfigureerimine**

1.  Paremklõpsake vastloodud C# projekti Solution Exploreris ja seejärel klõpsake käsku **Atribuudid**.
2.  Määrata väljundi tee LocalVertexHost projekti töötamise kausta tee. Saate LocalVertexHost projekti töötamise kausta tee kaudu LocalVertexHost atribuudid.
3.  Selleks, et LocalVertexHost projekti töötamise Directory .pdb faili salvestasin C# projekti koostamine või saate kopeerida .pdb faili selle kausta käsitsi.
4.  **Erandi sätted**, kontrollige levinud keele Runtime erandid.

![Azure'i andmed Lake Analytics U-SQL-i silumine visual Studios säte](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-clr-exception-setting.png)
 
##<a name="debug-the-job"></a>Töö silumine

Kui olete loonud silumine lahenduse allalaadimine on tipp ja konfigureerinud keskkonna, saate alustada oma A-SQL-koodi silumine.

1.  Lahenduste Explorer, paremklõpsake vastloodud **LocalVertexHost** projekt, osutage **silumine**ja klõpsake nuppu **Alusta uut eksemplari**. Funktsiooni LocalVertexHost peab olema seatud käivitus projekti. Esimest korda, mille võite ignoreerida võidakse kuvada järgmine teade. Võib kuluda kuni üks minut silumine kuvale.
 
    ![Azure'i andmed Lake Analytics U-SQL-i silumine visual Studios hoiatus](./media/data-lake-analytics-debug-u-sql-jobs/data-lake-analytics-visual-studio-u-sql-debug-warning.png)

4.  Kasutage Visual Studio vastavalt silumine kogemus (Vaata, muutujate jne), probleemi tõrkeotsinguks. 
5.  Probleemi olete kindlaks teinud, parandage kood ja seejärel taasloomine C# projekt enne testimist uuesti, kuni kõik probleemid on lahendatud. Pärast silumine on on edukalt lõpule viidud, väljundi aknas, kus on kuvatud järgmine teade 

        The Program ‘LocalVertexHost.exe’ has exited with code 0 (0x0).
 
##<a name="resubmit-the-job"></a>Töö uuesti

Kui olete lõpetanud silumine oma A-SQL-koodi, mida saate uuesti nurjunud töö.

1. Uue andmebaasi ADLA dll komplektide registreerida.

    1.  Server Explorer/Cloud Explorer andmete Lake Visual Studio tööriista, laiendage **andmebaasid** 
    2.  Paremklõpsake assemblereid Register assemblereid. 
    3.  Registreerige oma uus dll komplektide ADLA andmebaas.
 
2.  Või C# koodi kopeerimiseks script.usql.cs--C# koodi taga fail.
3.  Uuesti oma töö.

##<a name="next-steps"></a>Järgmised sammud

- [Õpetus: Alustamine Azure andmete Lake Analytics U-SQL-i keel](data-lake-analytics-u-sql-get-started.md)
- [Õpetus: arendamise U-SQL-i skriptide abil andmete Lake tööriistad Visual Studio](data-lake-analytics-data-lake-tools-get-started.md)
- [Arendada Azure'i andmeanalüüsi Lake tööde U-SQL-i kasutaja määratletud tehtemärgid](data-lake-analytics-u-sql-develop-user-defined-operators.md)

