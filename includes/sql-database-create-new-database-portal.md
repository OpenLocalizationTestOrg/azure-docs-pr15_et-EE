
<!--
includes/sql-database-create-new-database-portal.md

Latest Freshness check:  2016-04-11 , carlrab.

As of circa 2016-04-11, the following topics might include this include:
articles/sql-database/sql-database-get-started-tutorial.md

-->
## <a name="create-a-new-azure-sql-database"></a>Uue Azure SQL-i andmebaasi loomine

Azure portaali järgmiste juhiste abil luua uue Azure SQL-i andmebaasi uude või olemasolevasse Azure'i SQL-andmebaasi loogilise serveris.

1. Kui te pole praegu ühendatud, ühenduse [Azure'i portaalis](http://portal.azure.com).
2. Klõpsake nuppu **Uus**, tippige **SQL-andmebaasi**, ja klõpsake nuppu **SQL-andmebaasi (uus projekt)**.

     ![Uue andmebaasi](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-1.png)

3. Klõpsake nuppu **SQL-andmebaasi (uus projekt)**.

     ![Uue andmebaasi](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-2.png)

4. Klõpsake nuppu **Loo** SQL-andmebaasi teenuse uue andmebaasi loomiseks.

     ![Uue andmebaasi](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-3.png)

5. Väärtuste ette serveri järgmised atribuudid:

 - Andmebaasi nimi
 - Tellimus: See kehtib ainult juhul, kui teil on mitu tellimust.
 - Ressursirühm: kui te alles alustate, kasutage loogika serveri ressursirühma.
 - Valige allikas: saate valida tühja andmebaasi, näidisandmete või Azure'i andmebaasi varukoopia. Mõne asutusesisese SQL serveri andmebaasi migreerimine või BCP käsurea tööriista abil andmete laadimine leiate käesoleva artikli lõpus linke.
 - Server: Uude või olemasolevasse loogilise server.
 - Serveri administraator sisselogimine
 - Parooli
 - Hinnakirjad taseme: kui te alles alustate, kasutage vaikeväärtust, milleks S0.
 - Võrdlemine: See kehtib ainult juhul, kui valiti tühja andmebaasi.

        ![New database](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-4.png)

6.  Klõpsake nuppu **Loo**. Olekualal, näete, et juurutamise käivitas.

     ![Uue andmebaasi](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-5.png)

7. Oodake enne jätkamist järgmise juhise juurde lõpuleviimiseks juurutamiseks.

     ![Uue andmebaasi](./media/sql-database-create-new-database-portal/sql-database-create-new-database-portal-6.png)
