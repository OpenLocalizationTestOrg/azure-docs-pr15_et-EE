<properties
   pageTitle="Kuidas luua tugi Piletite SQL-i andmebaas | Microsoft Azure'i"
   description="Kuidas luua tugi Piletite SQL Azure'i andmebaas."
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="sonyam"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="09/01/2016"
   ms.author="sonyama;barbkess"/>

# <a name="how-to-create-a-support-ticket-for-sql-data-warehouse"></a>Kuidas luua tugi Piletite SQL-andmebaas
 
Kui te oma SQL-i andmebaas probleeme looge tugi Piletite nii, et teid aidata meie meeskond.

## <a name="create-a-support-ticket"></a>Tugiteenuste Piletite loomine

1. Avage [Azure'i portaalis][].

2. Klõpsake avakuval paan **Spikker + tugi** .

    ![Abi + tugi](./media/sql-data-warehouse-get-started-create-support-ticket/help-support.png)

3. Aidake + tugi tera, klõpsake nuppu **Loo tugiteenuse taotluse**.

    ![Uue tugiteenuse taotluse](./media/sql-data-warehouse-get-started-create-support-ticket/create-support-request.png)
    
    <a name="request-quota-change"></a> 

4. Valige **soovi tüüp**.

    ![Taotluse tüüp](./media/sql-data-warehouse-get-started-create-support-ticket/request-type.png)
    
    >[AZURE.NOTE]  Vaikimisi on iga SQL serveri (nt myserver.database.windows.net) **DTU kvoodi** 45 000. Selle piirmäära on lihtsalt ohutus piirang. Saate suurendada, luues tugi Piletite ja valides *kvoodi* taotluse tüübiks meiliteavitus. Teie DTU arvutamiseks tuleb 7,5 korrutamine [DWU][] vaja kogusumma. Näiteks soovite majutada kaks DW6000s üks SQL Server, siis peate hankima DTU kvoot 90 000.  Saate vaadata oma praeguse DTU tarbimine keelest SQL serveri portaalis. Peatatud- ja un peatatud andmebaasi arvestata DTU kvoodi. 

5. Valige **tellimus** , mis hostib probleemi soovite andmebaasi.

    ![Tellimuse](./media/sql-data-warehouse-get-started-create-support-ticket/subscription.png)

6. Valige **SQL-i andmebaas** ressurss.

    ![Ressurss](./media/sql-data-warehouse-get-started-create-support-ticket/resource.png)

7. Valige oma [Azure tugiteenuste leping][].

    - **Arveldusega seotud kvoodi ja tellimuste haldamise** tugi on saadaval tugi tasanditel.
    - **Probleemide** tugi on esitatud [arendaja][], [Standard][], [Professional][] või [Premier][] tugi. Probleemide on probleemid klientide Azure kasutamise ajal kui seal on mõistlik oodata, Microsoft probleemi põhjustas.
    - **Arendaja juhendamist** ja **nõustamisteenuseid** on saadaval [Professional otsese][] ja [Premier][] toetustasemed. 
    
    Kui teil on Premier-klassi tugi leping, saate teatada ka SQL-i andmebaas seotud probleemid [Microsoft Premier veebiportaali][].  Teemast [Azure tugiteenuste lepingud][Azure tugiteenuste lepingu] Lisateavet erinevate tugiteenuste lepingud, sh ulatus, vastuse ajad, hinnad jne.  Korduma kippuvad küsimused Azure'i toe leiate teemast [Azure toetavad korduma kippuvad küsimused][].  

    ![Toe kavandamine](./media/sql-data-warehouse-get-started-create-support-ticket/support-plan.png)

8. **Probleemi tüüp** ja **kategooria**valimine

    ![Probleemi tüüp kategooria](./media/sql-data-warehouse-get-started-create-support-ticket/problem-type-category.png)

9. Kirjelda ja valige äri tase.

    ![Probleemi kirjeldus](./media/sql-data-warehouse-get-started-create-support-ticket/problem-description.png)

10. Teie **Kontaktteave** selle tugi Piletite on täidetud. Värskendage seda vajaduse korral.

    ![Kontaktteave](./media/sql-data-warehouse-get-started-create-support-ticket/contact-info.png)

11. Klõpsake nuppu **Loo** esitada tugiteenuse taotluse.


## <a name="monitor-a-support-ticket"></a>Kuvari toe Piletite

Kui teil on esitada tugiteenuse taotluse Azure tugimeeskonna teiega ühendust. Teie taotlus olek ja üksikasjad, klõpsake nuppu **Halda tugiteenuse taotlused** armatuurlaual.

![Oleku kontrollimine](./media/sql-data-warehouse-get-started-create-support-ticket/check-status.png)

## <a name="other-resources"></a>Muud ressursid

Lisaks saate ühenduse luua ühenduse SQL-i andmebaas [Virnas ületäitumise][] või [Azure SQL-i andmete ladu MSDN-i Foorum][].

<!--Image references--> 

<!--Article references--> 
[DWU]: ./sql-data-warehouse-overview-what-is.md#data-warehouse-units

<!--MSDN references--> 

<!--Other web references--> 
[Azure'i portaal]: https://portal.azure.com/
[Azure'i toe kavandamine]: https://azure.microsoft.com/support/plans/?WT.mc_id=Support_Plan_510979/  
[Arendaja]: https://azure.microsoft.com/support/plans/developer/  
[Standard]: https://azure.microsoft.com/support/plans/standard/  
[Professionaalne otsesed]: https://azure.microsoft.com/support/plans/prodirect/  
[Premier]: https://azure.microsoft.com/support/plans/premier/  
[Azure'i tugi KKK]: https://azure.microsoft.com/support/faq/
[Microsoft Premier veebiportaali]: https://premier.microsoft.com/
[Virnlintdiagrammil ületäitumine]: https://stackoverflow.com/questions/tagged/azure-sqldw/
[Azure SQL-i andmete ladu MSDN-i Foorum]: https://social.msdn.microsoft.com/Forums/home?forum=AzureSQLDataWarehouse/

