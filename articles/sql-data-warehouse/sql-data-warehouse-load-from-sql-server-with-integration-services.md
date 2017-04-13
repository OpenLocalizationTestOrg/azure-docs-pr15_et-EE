<properties
   pageTitle="Andmete laadimine SQL Serverist rakendusse Azure SQL-i andmete ladu (SSIS) | Microsoft Azure'i"
   description="Näitab, kuidas luua SQL serveri Integration Services (SSIS) paketi andmeallikate mitmesuguseid andmete teisaldamiseks SQL-i andmebaas."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="lodipalm"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="08/08/2016"
   ms.author="lodipalm;sonyama;barbkess"/>

# <a name="load-data-from-sql-server-into-azure-sql-data-warehouse-ssis"></a>Andmete laadimine SQL Serverist rakendusse Azure SQL-i andmete ladu (SSIS)

> [AZURE.SELECTOR]
- [SSIS-I](sql-data-warehouse-load-from-sql-server-with-integration-services.md)
- [PolyBase](sql-data-warehouse-load-from-sql-server-with-polybase.md)
- [BCP](sql-data-warehouse-load-from-sql-server-with-bcp.md)


Andmete SQL serveri laadimiseks SQL Azure'i andmebaas SQL serveri Integration Services (SSIS) paketi loomine. Soovi korral saate ümber korraldada, muuta ja puhastada andmeid, sest see läbib SSIS andmevoo.

Selles õppetükis saate küll:

- Looge uus integreerimisteenuste projekt Visual Studios.
- Luua ühenduse andmeallikaga, sh SQL serveri (nagu allikas) ja SQL-i andmebaas (sihtkohana).
- Kujundage SSIS-paketi, mis laaditakse andmete allikas sihtkoht.
- Andmete laadimiseks SSIS-paketi käivitamine

Selle õpetuse kasutab SQL serveri andmeallika. Ruumis või mõne Azure virtuaalse masina võiks SQL Server töötab.

## <a name="basic-concepts"></a>Põhimõtted

Pakett on töö SSIS üksus. Seotud pakettide rühmitatakse projektid. Visual Studio ja SQL Server Data Tools luua projekte ja kujunduse paketid. Kujunduse protsess on visuaalne, kus te lohistage ja kukutage komponendid tööriistakastist kujunduspinna, ühendage need, ja nende atribuutide seadmine. Kui olete lõpetanud oma paketi, saate soovi korral juurutamist SQL serveri täielik haldus, jälgimine ja turvalisus.

## <a name="options-for-loading-data-with-ssis"></a>SSIS-i andmete laadimise suvandid

SQL serveri Integration Services (SSIS) on paindlik komplekt tööriistu, mis pakub mitmesuguseid võimalusi ühenduse loomiseks ja andmete laadimine, SQL-i andmebaas.

1. Ühenduse loomine SQL-i andmebaas on ADO NET sihtkoha abil. Selles õpetuses kasutab mõne ADO NET sihtkoht, kuna see on kõige vähem konfiguratsiooni võimalusi.
2. Ühenduse loomine SQL-i andmebaas on OLE DB sihtkoha abil. See suvand võib anda pisut parema tulemuse kui ADO NET sihtkoht.
3. Azure'i bloobimälu üleslaadimine tööülesande abil andmete Azure'i bloobimälu etapp. Seejärel käivitage Polybase skripti, mis laadib andmed SQL-i andmebaas tööülesande SSIS käivitada SQL-i abil. See suvand annab parimaid tulemusi siin loetletud kolmest suvandist. Azure'i bloobimälu üleslaadimine tööülesande saamiseks laadige alla ühilduvuspakett [Microsoft SQL Server 2016 Integration Services Feature Packi Azure][]. Polybase kohta leiate lisateavet teemast [PolyBase juhend][].

## <a name="before-you-start"></a>Enne alustamist

Samm selle õpetuse kaudu, peate.

1. **SQL Server Integration Services (SSIS)**. SSIS-i on osa SQL serveri ja jaoks on vaja mõnda hindamise või SQL serveri litsentsiga versioon. Tutvumist võimaldava versiooni SQL Server 2016 Preview kohta leiate teavet [SQL serveri hinnangud][].
2. **Visual Studio**. Tasuta Visual Studio 2015 ühenduse Edition saamiseks vaadake [Visual Studio ühenduse][].
3. **SQL Server Data Tools for Visual Studio (SSDT)**. SQL Server Data Tools for Visual Studio 2015 kohta leiate teavet [Alla laadida SQL serveri andmete tööriistad (SSDT)][].
4. **Näidisandmed**. Selle õpetuse kasutab andmeallikana näidisandmebaasi AdventureWorks SQL serveris talletatavate Näidisandmete laaditakse üheks SQL-i andmebaas. Näidisandmebaasi AdventureWorks kohta leiate teavet [AdventureWorks 2014 valimi andmebaasid][].
5. **A SQL-i andmebaas andmebaasi ja õigused**. Selle õpetuse SQL-i andmebaas eksemplariga ja selle andmete laaditakse. Teil on õigused tabeli loomiseks ja andmete laadimine.
6. **Tulemüüri reegel**. Teil on tulemüüri reegli loomiseks klõpsake SQL-i andmebaas kohaliku arvuti IP-aadressiga enne saate üles laadida andmete SQL-i andmebaas.

## <a name="step-1-create-a-new-integration-services-project"></a>Samm 1: Looge uus integreerimisteenuste projekt

1. Käivitage Visual Studio 2015.
2. Klõpsake menüüs **fail** valige uus **| Projekti**.
3. Liikuge soovitud **installitud | Mallide | Ärianalüüs | Integration Services** projekti tüübid.
4. Valige **Integration Services Project**. Väärtuste ette **nimi** ja **asukoht**ja seejärel klõpsake **nuppu OK**.

Visual Studio avab ja loob uue projekti Integration Services (SSIS). Seejärel avab Visual Studio projekti designer ühe uue SSIS-pakett (Package.dtsx) jaoks. Kuvatakse järgmine Kuva alad.

- Klõpsake vasakul osade SSIS **Toolbox** .
- Keskel, kujunduspinna, mitme sakkidega. Tavaliselt kasutate vähemalt **Control Flow** ja **Data Flow** vahekaarte.
- Paremal, **Solution Exploreris** ja **atribuutide** paanid.

    ![][01]

## <a name="step-2-create-the-basic-data-flow"></a>Samm 2: Looge peamine andmete voogu

1. Lohistage andmeid Flow ülesande tööriistakastist kujunduspinna keskele (vahekaardil **Control Flow** ).

    ![][02]

2. Topeltklõpsake andmete Flow tööülesande Data Flow vahekaardi aktiveerimine.
3. Lohistage tööriistakasti loendist muudest andmeallikatest kujunduspinna ADO.net-i allikas. Koos allika valituks, muuta oma nime **SQL serveri andmeallika** paanil **Atribuudid** .
4. Tööriistakasti loendist Muud sihtkohta lohistage soovitud sihtkohta ADO.net-i kujunduspinna jaotises ADO.net-i allikas. Sihtkoha adapterit valituks, muuta selle nime **SQL-i DW** sihtkohta paanil **Atribuudid** .

    ![][09]

## <a name="step-3-configure-the-source-adapter"></a>Samm 3: Konfigureerige allikas

1. Andmeallika adapterit **ADO.NET lähtekoodi redaktor**avamiseks topeltklõpsake.

    ![][03]

2. **ADO.net-i lähtekoodi redaktor**menüü **Haldur** nuppu **Uus** **Konfigureerimine ADO.net-i haldur** dialoogiboksi avamine ja luua ühenduse sätted, millest selles õpetuses laadib andmed SQL serveri andmebaasi **ADO.net-i haldur** loendi kõrval.

    ![][04]

3. Dialoogiboksis **Konfigureerimine ADO.net-i haldur** nuppu **Uus** **Ühendus halduri** dialoogiboksi avamine ja uue andmeühenduse loomiseks.

    ![][05]

4. Tehke dialoogiboksis **Ühenduse haldur** järgmisi asju.

    1. Valige **pakkuja**, SqlClient andmepakkuja.
    2. Sisestage **Serveri nimi**, SQL serveri nimi.
    3. Jaotises **serveri sisselogimiseks** valige või sisestage autentimise teavet.
    4. Valige jaotises **andmebaasi ühenduse** näidisandmebaasi AdventureWorks.
    5. Klõpsake nuppu **testi ühendust**.
    
        ![][06]
    
    6. Klõpsake kuvatavas dialoogiboksis ühenduse testi tulemused aruannete, **OK** , et naasta dialoogiboksi **Haldur** .
    7. Klõpsake dialoogiboksis **Ühenduse haldur** **OK** , et naasta dialoogiboksi **Konfigureerimine ADO.net-i haldur** .
 
5. **ADO.net-i haldur konfigureerida** dialoogiboksis nuppu **OK** , et naasta **ADO.NET lähtekoodi redaktor**.
6. **ADO.net-i lähtekoodi redaktor** **tabelit või vaadet nime** loendis valige **Sales.SalesOrderDetail** tabel.

    ![][07]

7. Klõpsake nuppu **Eelvaade** , et näha esmalt 200 andmeread lähtetabeli dialoogiboksis **Päringu tulemuste eelvaade** .

    ![][08]

8. Dialoogiboksis **Päringu tulemuste eelvaade** nuppu **Sule** naasmiseks **ADO.NET lähtekoodi redaktor**.
9. **ADO.net-i lähtekoodi redaktor**, klõpsake nuppu **OK** andmeallika konfigureerimise lõpuleviimiseks.

## <a name="step-4-connect-the-source-adapter-to-the-destination-adapter"></a>Samm 4: Allika adapterit ühenduse sihtkoht adapterit

1. Valige allikas adapterit konstruktsioonipinnalt.
2. Valige sinine nool, mis ulatub allika adapterit ja lohistage see sihtkoht redaktori kuni klõpsatusega paika.

    ![][10]

    Tüüpilised SSIS-paketti, kasutage arvu tööriistakastist SSIS vahepeal on lähte- ja muud komponendid ümber korraldada, muuta ja puhastage andmete, sest see läbib SSIS andmevoo. Selles näites võimalikult lihtne säilitamiseks me loote ühenduse allika otse sihtkohta.

## <a name="step-5-configure-the-destination-adapter"></a>Juhis 5: Konfigureerige sihtkoht

1. Sihtkoha adapterit **ADO.NET sihtkoha redaktori**avamiseks topeltklõpsake.

    ![][11]

2. **ADO.net-i sihtkoha redaktori**menüü **Haldur** nuppu **Uus** **Konfigureerimine ADO.net-i haldur** dialoogiboksi avamine ja luua ühenduse sätted, kuhu selle õpetuse laadib andmed SQL Azure'i andmebaas andmebaasi **ühenduse halduri** loendi kõrval.
3. Dialoogiboksis **Konfigureerimine ADO.net-i haldur** nuppu **Uus** **Ühendus halduri** dialoogiboksi avamine ja uue andmeühenduse loomiseks.
4. Tehke dialoogiboksis **Ühenduse haldur** järgmisi asju.
    1. Valige **pakkuja**, SqlClient andmepakkuja.
    2. Sisestage **Serveri nimi**, SQL-i andmebaas nimi.
    3. Jaotise **serverisse sisse logida** , valige **Kasuta SQL serveri autentimine** ja sisestage autentimise teave.
    4. Valige jaotises **andmebaasiga ühenduse loomine** SQL-i andmebaas olemasoleva andmebaasiga.
    5. Klõpsake nuppu **testi ühendust**.
    6. Klõpsake kuvatavas dialoogiboksis ühenduse testi tulemused aruannete, **OK** , et naasta dialoogiboksi **Haldur** .
    7. Klõpsake dialoogiboksis **Ühenduse haldur** **OK** , et naasta dialoogiboksi **Konfigureerimine ADO.net-i haldur** .
5. Klõpsake dialoogiboksis **Konfigureerimine ADO.net-i haldur** **OK** , et naasta **ADO.NET sihtkoha redaktor**.
6. **ADO.net-i sihtkoha redaktori** **kasutamine tabelit või vaadet** loendi avamiseks luua uue sihttabeli veeru loend, mis vastab lähtetabeli dialoogiboksi **Tabeli loomine** kõrval nuppu **Uus** .

    ![][12a]

7. Tehke dialoogiboksis **Tabeli loomine** järgmisi asju.

    1. Muuta **atribuudi SalesOrderDetail**sihttabeli nimi.
    2. Eemaldage veerg **WGUID** . SQL-i andmebaas ei toeta **ainuidentifikaatori** andmetüüpi.
    3. **Raha** **LineTotal** veeru andmetüüpi muuta. Andmetüüp **decimal** ei toeta SQL-i andmebaas. Toetatud andmetüüpide kohta leiate teemast [Tabeli loomine (SQL Azure'i andmebaas, paralleelselt andmebaas)][].
    
        ![][12b]
    
    4. Klõpsake nuppu **OK** tabeli loomine ja naasmiseks **ADO.NET sihtkoha redaktor**.

8. **ADO.net-i sihtkoha Editor** **vastendused** vahekaardi näha, kuidas veergude allikas on vastendatud sihtkoht veergude valimine

    ![][13]

9. Klõpsake nuppu **OK** andmeallika konfigureerimise lõpuleviimiseks.

## <a name="step-6-run-the-package-to-load-the-data"></a>Samm 6: Käivitage pakett andmete laadimine

Käivitage pakett, klõpsates nuppu **Start** , klõpsake tööriistaribal või valige mõni **käivitada** **silumine** menüü Suvandid.

Kui paketi algab, näete kollane valmistamata rataste näitamaks, samuti töödeldud siiani ridade arv.

![][14]

Kui pakett on töö lõpetanud, kuvatakse roheline märkige ruudud tähistamiseks edu samuti arv andmeridu laaditud lähtekoha sihtkohta.

![][15]

Palju õnne! Kasutasite SQL serveri Integreerimisteenused edukalt andmete laadimiseks SQL Azure'i andmebaas.

## <a name="next-steps"></a>Järgmised sammud

- Lugege lisateavet SSIS andmevoo. Alustage siit: [Data Flow][].
- Saate teada, kuidas silumine ja oma pakettide paremale kujunduskeskkonnast tõrkeotsing. Alustage siit: [Paketi arengu tõrkeotsingu tööriistu][].
- Saate teada, kuidas Juurutage SSIS projekte ja pakettide Integration Services serveris või mõni muu talletuskoht. Alustage siit: [juurutus, projekte ja paketid][].

<!-- Image references -->
[01]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-designer-01.png
[02]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ssis-data-flow-task-02.png
[03]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-03.png
[04]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-manager-04.png
[05]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-connection-05.png
[06]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/test-connection-06.png
[07]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-source-07.png
[08]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/preview-data-08.png
[09]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/source-destination-09.png
[10]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/connect-source-destination-10.png
[11]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/ado-net-destination-11.png
[12a]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-before-12a.png
[12b]: ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/destination-query-after-12b.png
[13]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/column-mapping-13.png
[14]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-running-14.png
[15]:  ./media/sql-data-warehouse-load-from-sql-server-with-integration-services/package-success-15.png

<!-- Article references -->

<!-- MSDN references -->
[PolyBase juhend]: https://msdn.microsoft.com/library/mt143171.aspx
[SQL Server Data Tools (SSDT) allalaadimine]: https://msdn.microsoft.com/library/mt204009.aspx
[Looge tabel (SQL Azure'i andmebaas, Paralleelandmete ladu)]: https://msdn.microsoft.com/library/mt203953.aspx
[Andmevoo]: https://msdn.microsoft.com/library/ms140080.aspx
[Tõrkeotsinguriistad paketi loomine]: https://msdn.microsoft.com/library/ms137625.aspx
[Projektide ja pakettide kasutuselevõtt]: https://msdn.microsoft.com/library/hh213290.aspx

<!--Other Web references-->
[Microsoft SQL Server 2016 Integration Services funktsioonide Azure'i pakett]: http://go.microsoft.com/fwlink/?LinkID=626967
[SQL serveri hindamine]: https://www.microsoft.com/en-us/evalcenter/evaluate-sql-server-2016
[Visual Studio ühenduse]: https://www.visualstudio.com/en-us/products/visual-studio-community-vs.aspx
[AdventureWorks 2014 valimi andmebaasid]: https://msftdbprodsamples.codeplex.com/releases/view/125550
