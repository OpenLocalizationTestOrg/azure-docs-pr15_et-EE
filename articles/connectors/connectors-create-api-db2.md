<properties
    pageTitle="Loogika rakenduste lisamine DB2 konnektor | Microsoft Azure'i"
    description="DB2 konnektor REST API parameetritega ülevaade"
    services=""
    documentationCenter="" 
    authors="gplarsen"
    manager="erikre"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="09/26/2016"
   ms.author="plarsen"/>


# <a name="get-started-with-the-db2-connector"></a>Alustamine DB2 konnektor
Microsofti konnektor DB2 ühendab loogika rakenduste ressursse, mis on talletatud loomine IBM DB2 andmebaasiga. Selle konnektori sisaldab Microsoft kliendi suhelda DB2 server kaugarvutite TCP/IP võrgus. See hõlmab cloud andmebaasidele, nt IBM Bluemix dashDB või IBM DB2 Windows Azure'i virtualization, töötab ja kohapealse abil kohapealse andmete lüüsi andmebaasid. Vaadake selle [loendi toetatud](connectors-create-api-db2.md#supported-db2-platforms-and-versions) IBM DB2 platvormide ja versioonid (selles teemas).

>[AZURE.NOTE] Selle versiooni see artikkel kehtib loogika rakendused üldiselt kättesaadav (GA). 

DB2 konnektor toetab andmebaasi järgmisi toiminguid:

- Loendi andmebaasitabelid
- Ühe rea valimine abil lugemine
- Lugege kõik read, kasutades valimine
- LISA abil ühe rea lisamine
- Muuta ühe rea UPDATE'I abil
- Kustuta kasutamise ühe rea eemaldamine

See teema näitab konnektor kasutamine loogika rakendus protsessi andmebaasi toiminguid.

Loogika rakenduste kohta leiate lisateavet teemast [loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md).

## <a name="available-actions"></a>Saadaolevad toimingud
DB2 konnektor toetab loogika rakenduse järgmisi toiminguid:

- GetTables
- GetRow
- Täitmisaeg
- InsertRow
- UpdateRow
- DeleteRow


## <a name="list-tables"></a>Loend tabelid
Loogika rakenduse mis tahes toimingu jaoks loomine koosneb palju juhiseid läbi Microsoft Azure portaali kaudu.

Loogika rakenduses, saate lisada toimingu loend tabelid DB2 andmebaasiga. Toimingu juhendab konnektor töödelda DB2 skeemi lause, näiteks `CALL SYSIBM.SQLTABLES`.

### <a name="create-a-logic-app"></a>Loogika rakenduse loomine
1.  **Azure'i käivitamine tahvli**valige **+** (plussmärk) **Web + Mobile**, ja seejärel **Loogika rakendus**.
2.  Sisestage **nimi**, näiteks `Db2getTables`, **tellimus**, **ressursirühm**, **asukoht**ja **Rakenduse teenuse kavandamine**. Valige **armatuurlaud Kinnita**ja seejärel nuppu **Loo**.

### <a name="add-a-trigger-and-action"></a>Päästiku ja toimingu lisamine
1.  **Loogika rakenduste Designer**, valige **Tühi LogicApp** **mallide** loendi.
2.  Valige loendis **päästikute** **Korduvus**. 
3.  **Korduvuse** päästik, valige **Redigeeri**, valige **sagedus** rippmenüüst valida **päeva**ja määrake **intervalli** tippida **7**.  
4.  Valige **+ uus samm** välja ja valige **Lisa toiming**.
5.  Tippige loendis **toimingud** `db2` **Veel toiminguid otsimine** redigeerimine ruut ja seejärel valige **DB2 - saada tabelid (eelvaade)**.

    ![](./media/connectors-create-api-db2/Db2connectorActions.png)  

6.  Valige paanil **DB2 - saada tabelite** konfigureerimine **ruut** luba **ühenduse loomine kohapealse andmete lüüsi kaudu**. Pange tähele, et sätteid muuta cloud kohapealse.
    - Tippige väärtus **serveri**koolon aadress või pseudonüüm pordinumber kujul. Näiteks tippige `ibmserver01:50000`.
    - Tippige väärtus **andmebaasi**. Näiteks tippige `nwind`.
    - Valige väärtus **autentimiseks**. Näiteks valige **tavaline**.
    - Tippige väärtus **kasutajanimi**. Näiteks tippige `db2admin`.
    - Tippige väärtus **parooli**. Näiteks tippige `Password1`.
    - Valige väärtus **lüüsi**. Näiteks valige **datagateway01**.
7. Valige **Loo**ja seejärel valige **Salvesta**. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

8.  Valige **Db2getTables** labale **Kokkuvõte**, klõpsake jaotises **kõik töötab** loendis esimese loendis üksuse (kõige uuemad Käivita).
9.  **Loogika rakenduse käivitamine** tera, valige **Käivita üksikasjad**. Valige loendis **toiming** sees **Get_tables**. Vt väärtus **olek**, mis peaks olema **õnnestus**. Valige **link sisendina** sisendeid kuvamiseks. Valige **link väljundid**ja vaadata väljundeid; mida peaks sisaldama tabelite loendi.

    ![](./media/connectors-create-api-db2/Db2connectorGetTablesLogicAppRunOutputs.png)

## <a name="create-the-connections"></a>Ühenduste loomine
Selle konnektori toetab ühendusi kohapealse majutatud andmebaasid ja pilves, kasutades järgmist ühenduse atribuudid. 

Atribuut | Kirjeldus
--- | ---
Server | Nõutav. Aktsepteerib stringi väärtus, mis tähistab TCP/IP aadress või pseudonüüm, kas IPv4 või IPv6-vormingus jälgitavad (koolon eraldatud), TCP/IP pordi number. 
andmebaasi | Nõutav. Aktsepteerib stringi väärtus, mis tähistab DRDA relatsiooniline andmebaasi nimi (RDBNAM). DB2 z/OS aktsepteerib 16-baidine string (andmebaasi tuntud IBM DB2 z/OS asukoha jaoks). DB2 i5/OS aktsepteerib mõne 18-baidine string (nimetatakse i IBM DB2 andmebaas relatsiooniandmebaasist). DB2 LUW aktsepteerib on 8-baidine string.
autentimine | Valikuline. Aktsepteerib loendi üksuse väärtuse, kas Basic või Windowsi (kerberos). 
kasutajanimi | Nõutav. Aktsepteerib stringiväärtus. DB2 z/OS aktsepteerib on 8-baidine string. DB2 jaoks ma aktsepteerib 10-baidine string. DB2 Linuxi või UNIX aktsepteerib on 8-baidine string. Windowsi jaoks mõeldud DB2 aktsepteerib 30-baidine string.
parooli | Nõutav. Aktsepteerib stringiväärtus.
lüüsi | Nõutav. Aktsepteerib loendis üksuse väärtus tähistav kohapealse andmete lüüsi määratletud loogika rakenduste salvestusruumi rühmast.  

## <a name="create-the-on-premises-gateway-connection"></a>Asutusesisene lüüsi ühenduse loomine
Selle konnektori pääsevad kohapealse DB2 andmebaasiga kohapealse lüüsi abil. Lüüsi teemadest leiate lisateavet. 

1. Valige paanil **lüüside** konfigureerimine **ruut** luba **lüüsi kaudu ühenduse loomine**. Pöörake tähelepanu sellele, et sätete muutmine cloud kohapealsesse.
2. Tippige väärtus **serveri**koolon aadress või pseudonüüm pordinumber kujul. Näiteks tippige `ibmserver01:50000`.
3. Tippige väärtus **andmebaasi**. Näiteks tippige `nwind`.
4. Valige väärtus **autentimiseks**. Näiteks valige **tavaline**.
5. Tippige väärtus **kasutajanimi**. Näiteks tippige `db2admin`.
6. Tippige väärtus **parooli**. Näiteks tippige `Password1`.
7. Valige väärtus **lüüsi**. Näiteks valige **datagateway01**.
8. Valige **Loo** jätkata. 

    ![](./media/connectors-create-api-db2/Db2connectorOnPremisesDataGatewayConnection.png)

## <a name="create-the-cloud-connection"></a>Pilveteenuse ühenduse loomine
Selle konnektori pääsevad cloud DB2 andmebaas. 

1. **Lüüside** konfiguratsiooni paanil **ruut** jäta keelatud (mitteklõpsatavate) **ühendamine lüüsi kaudu**. 
2. Tippige **väljale ühenduse**nimi väärtus. Näiteks tippige `hisdemo2`.
3. Tippige väärtus **DB2 serveri nimi**kujul koolon aadress või pseudonüüm pordinumber. Näiteks tippige `hisdemo2.cloudapp.net:50000`.
3. Tippige väärtus **DB2 andmebaasi**nimi. Näiteks tippige `nwind`.
4. Tippige väärtus **kasutajanimi**. Näiteks tippige `db2admin`.
5. Tippige väärtus **parooli**. Näiteks tippige `Password1`.
6. Valige **Loo** jätkata. 

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

## <a name="fetch-all-rows-using-select"></a>Toomiseks abil valige kõik read
Saate määratleda loogiline rakenduse toiming DB2 tabeli kõigi ridade toomiseks. See juhendab konnektor töödelda DB2 valige lause, näiteks `SELECT * FROM AREA`.

### <a name="create-a-logic-app"></a>Loogika rakenduse loomine
1.  **Azure'i käivitamine tahvli**valige **+** (plussmärk) **Web + Mobile**, ja seejärel **Loogika rakendus**.
2.  Sisestage **nimi**, näiteks `Db2getRows`, **tellimus**, **ressursirühm**, **asukoht**ja **Rakenduse teenuse kavandamine**. Valige **armatuurlaud Kinnita**ja seejärel nuppu **Loo**.

### <a name="add-a-trigger-and-action"></a>Päästiku ja toimingu lisamine
1.  **Loogika rakenduste Designer**, valige **Tühi LogicApp** **mallide** loendi.
2.  Valige loendis **päästikute** **Korduvus**. 
3.  **Korduvus** päästik, valige **Redigeeri**, valige **sagedus** ripploend **päeva**valimiseks ja valige **intervall** tippida **7**. 
4.  Valige **+ uus samm** välja ja valige **Lisa toiming**.
5.  Tippige loendis **toimingud** `db2` **Veel toiminguid otsimine** redigeerimine ruut ja seejärel valige **DB2 - saada read (eelvaade)**.
6. Valige toiming **saada read (eelvaade)** **ühenduse muutmine**.
7. Valige paanil **ühendused** konfiguratsiooni **Loo uus**. 

    ![](./media/connectors-create-api-db2/Db2connectorNewConnection.png)
  
8. **Lüüside** konfiguratsiooni paanil **ruut** jäta keelatud (mitteklõpsatavate) **ühendamine lüüsi kaudu**.
    - Tippige **väljale ühenduse**nimi väärtus. Näiteks tippige `HISDEMO2`.
    - Tippige väärtus **DB2 serveri nimi**kujul koolon aadress või pseudonüüm pordinumber. Näiteks tippige `HISDEMO2.cloudapp.net:50000`.
    - Tippige väärtus **DB2 andmebaasi**nimi. Näiteks tippige `NWIND`.
    - Tippige väärtus **kasutajanimi**. Näiteks tippige `db2admin`.
    - Tippige väärtus **parooli**. Näiteks tippige `Password1`.
9. Valige **Loo** jätkata.

    ![](./media/connectors-create-api-db2/Db2connectorCloudConnection.png)

10. **Tabeli nime** loendis Valige olevat **allanoolt**ja valige **ala**.
11. Soovi korral märkige ruut **Kuva täpsemad suvandid** päringu suvandite määramiseks.
12. Valige **Salvesta**. 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsTableName.png)

13. Valige **Db2getRows** labale **Kokkuvõte**, klõpsake jaotises **kõik töötab** loendis esimese loendis üksuse (kõige uuemad Käivita).
14. **Loogika rakenduse käivitamine** tera, valige **Käivita üksikasjad**. Valige loendis **toiming** sees **Get_rows**. Vt väärtus **olek**, mis peaks olema **õnnestus**. Valige **link sisendina** sisendeid kuvamiseks. Valige **link väljundid**ja vaadata väljundeid; mida peaks sisaldama loendi read.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowsOutputs.png)

## <a name="add-one-row-using-insert"></a>LISA abil ühe rea lisamine
Saate määratleda loogiline rakenduse toiming DB2 tabeli ühe rea lisamiseks. See toiming teeb konnektor töödelda DB2 lisada lause, näiteks `INSERT INTO AREA (AREAID, AREADESC, REGIONID) VALUES ('99999', 'Area 99999', 102)`.

### <a name="create-a-logic-app"></a>Loogika rakenduse loomine
1.  **Azure'i käivitamine tahvli**valige **+** (plussmärk) **Web + Mobile**, ja seejärel **Loogika rakendus**.
2.  Sisestage **nimi**, näiteks `Db2insertRow`, **tellimus**, **ressursirühm**, **asukoht**ja **Rakenduse teenuse kavandamine**. Valige **armatuurlaud Kinnita**ja seejärel nuppu **Loo**.

### <a name="add-a-trigger-and-action"></a>Päästiku ja toimingu lisamine
1.  **Loogika rakenduste Designer**, valige **Tühi LogicApp** **mallide** loendi.
2.  Valige loendis **päästikute** **Korduvus**. 
3.  **Korduvus** päästik, valige **Redigeeri**, valige **sagedus** ripploend **päeva**valimiseks ja valige **intervall** tippida **7**. 
4.  Valige **+ uus samm** välja ja valige **Lisa toiming**.
5.  Klõpsake loendis **toimingud** **db2** Tippige väljale **Otsi veel toiminguid** Redigeeri ja valige **DB2 - lisamisrea (eelvaade)**.
6. Valige toiming **saada read (eelvaade)** **ühenduse muutmine**. 
7. Valige paanil **ühendused** konfiguratsiooni ühenduse. Näiteks valige **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. **Tabeli nime** loendis Valige olevat **allanoolt**ja valige **ala**.
9. Sisestage väärtused kõiki vajalikke veerud (vt punane tärn). Näiteks tippige `99999` **AREAID**, tippige `Area 99999`, ja tippige `102` **REGIONID**jaoks. 
10. Valige **Salvesta**.

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowValues.png)
 
11. Valige **Db2insertRow** labale **Kokkuvõte**, klõpsake jaotises **kõik töötab** loendis esimese loendis üksuse (kõige uuemad Käivita).
12. **Loogika rakenduse käivitamine** tera, valige **Käivita üksikasjad**. Valige loendis **toiming** sees **Get_rows**. Vt väärtus **olek**, mis peaks olema **õnnestus**. Valige **link sisendina** sisendeid kuvamiseks. Valige **link väljundid**ja vaadata väljundeid; mida peaks sisaldama uue rea.

    ![](./media/connectors-create-api-db2/Db2connectorInsertRowOutputs.png)

## <a name="fetch-one-row-using-select"></a>Toomise ühe rea valimine abil
Saate määratleda loogiline rakenduse toiming toomiseks DB2 tabeli ühe rea. See toiming teeb konnektor töödelda DB2 valida kuhu avaldus, näiteks `SELECT FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Loogika rakenduse loomine
1.  **Azure'i käivitamine tahvli**valige **+** (plussmärk) **Web + Mobile**, ja seejärel **Loogika rakendus**.
2.  Sisestage **nimi** (nt "**Db2getRow**"), **tellimuse**, **ressursirühm**, **asukoht**, ja **Rakenduse teenuse kavandamine**. Valige **armatuurlaud Kinnita**ja seejärel nuppu **Loo**.

### <a name="add-a-trigger-and-action"></a>Päästiku ja toimingu lisamine
1.  **Loogika rakenduste Designer**, valige **Tühi LogicApp** **mallide** loendi. 
2.  Valige loendis **päästikute** **Korduvus**. 
3.  **Korduvus** päästik, valige **Redigeeri**, valige **sagedus** ripploend **päeva**valimiseks ja valige **intervall** tippida **7**. 
4.  Valige **+ uus samm** välja ja valige **Lisa toiming**.
5.  Loendis **toimingud** **db2** Tippige väljale **Otsi veel toiminguid** Redigeeri ja valige **DB2 - saada read (eelvaade)**.
6. Valige toiming **saada read (eelvaade)** **ühenduse muutmine**. 
7. Valige paanil **ühendused** konfiguratsioone olemasoleva ühenduse. Näiteks valige **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. **Tabeli nime** loendis Valige olevat **allanoolt**ja valige **ala**.
9. Sisestage väärtused kõiki vajalikke veerud (vt punane tärn). Näiteks tippige `99999` **AREAID**jaoks. 
10. Soovi korral märkige ruut **Kuva täpsemad suvandid** päringu suvandite määramiseks.
11. Valige **Salvesta**. 

    ![](./media/connectors-create-api-db2/Db2connectorGetRowValues.png)

12. Valige **Db2getRow** labale **Kokkuvõte**, klõpsake jaotises **kõik töötab** loendis esimese loendis üksuse (kõige uuemad Käivita).
13. **Loogika rakenduse käivitamine** tera, valige **Käivita üksikasjad**. Valige loendis **toiming** sees **Get_rows**. Vt väärtus **olek**, mis peaks olema **õnnestus**. Valige **link sisendina** sisendeid kuvamiseks. Valige **link väljundid**ja vaadata väljundeid; mida peaks sisaldama rida.

    ![](./media/connectors-create-api-db2/Db2connectorGetRowOutputs.png)

## <a name="change-one-row-using-update"></a>Ühe rea Update'i muutmine
Saate määratleda loogiline rakenduse toiming DB2 tabeli ühe rea muuta. See toiming teeb konnektor töödelda DB2 UPDATE-lause, näiteks `UPDATE AREA SET AREAID = '99999', AREADESC = 'Area 99999', REGIONID = 102)`.

### <a name="create-a-logic-app"></a>Loogika rakenduse loomine
1.  **Azure'i käivitamine tahvli**valige **+** (plussmärk) **Web + Mobile**, ja seejärel **Loogika rakendus**.
2.  Sisestage **nimi**, näiteks `Db2updateRow`, **tellimus**, **ressursirühm**, **asukoht**ja **Rakenduse teenuse kavandamine**. Valige **armatuurlaud Kinnita**ja seejärel nuppu **Loo**.

### <a name="add-a-trigger-and-action"></a>Päästiku ja toimingu lisamine
1.  **Loogika rakenduste Designer**, valige **Tühi LogicApp** **mallide** loendi.
2.  Valige loendis **päästikute** **Korduvus**. 
3.  **Korduvus** päästik, valige **Redigeeri**, valige **sagedus** ripploend **päeva**valimiseks ja valige **intervall** tippida **7**. 
4.  Valige **+ uus samm** välja ja valige **Lisa toiming**.
5.  Loendis **toimingud** **db2** Tippige väljale **Otsi veel toiminguid** Redigeeri ja valige **DB2 - reale Värskenda (eelvaade)**.
6. Valige toiming **saada read (eelvaade)** **ühenduse muutmine**. 
7. Valige paanil **ühendused** konfiguratsioone olemasoleva ühenduse valimiseks. Näiteks valige **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. **Tabeli nime** loendis Valige olevat **allanoolt**ja valige **ala**.
9. Sisestage väärtused kõiki vajalikke veerud (vt punane tärn). Näiteks tippige `99999` **AREAID**, tippige `Updated 99999`, ja tippige `102` **REGIONID**jaoks. 
10. Valige **Salvesta**. 

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowValues.png)

11. Valige **Db2updateRow** labale **Kokkuvõte**, klõpsake jaotises **kõik töötab** loendis esimese loendis üksuse (kõige uuemad Käivita).
12. **Loogika rakenduse käivitamine** tera, valige **Käivita üksikasjad**. Valige loendis **toiming** sees **Get_rows**. Vt väärtus **olek**, mis peaks olema **õnnestus**. Valige **link sisendina** sisendeid kuvamiseks. Valige **link väljundid**ja vaadata väljundeid; mida peaks sisaldama uue rea.

    ![](./media/connectors-create-api-db2/Db2connectorUpdateRowOutputs.png)

## <a name="remove-one-row-using-delete"></a>Kustuta kasutamise ühe rea eemaldamine
Saate määratleda loogiline rakenduse toiming DB2 tabeli ühe rea eemaldamiseks. See toiming teeb konnektor töödelda DB2 kustutada lause, näiteks `DELETE FROM AREA WHERE AREAID = '99999'`.

### <a name="create-a-logic-app"></a>Loogika rakenduse loomine
1.  **Azure'i käivitamine tahvli**valige **+** (plussmärk) **Web + Mobile**, ja seejärel **Loogika rakendus**.
2.  Sisestage **nimi**, näiteks `Db2deleteRow`, **tellimus**, **ressursirühm**, **asukoht**ja **Rakenduse teenuse kavandamine**. Valige **armatuurlaud Kinnita**ja seejärel nuppu **Loo**.

### <a name="add-a-trigger-and-action"></a>Päästiku ja toimingu lisamine
1.  **Loogika rakenduste Designer**, valige **Tühi LogicApp** **mallide** loendi. 
2.  Valige loendis **päästikute** **Korduvus**. 
3.  **Korduvus** päästik, valige **Redigeeri**, valige **sagedus** ripploend **päeva**valimiseks ja valige **intervall** tippida **7**. 
4.  Valige **+ uus samm** välja ja valige **Lisa toiming**.
5.  **Toimingute** loendist valige **db2** redigeerimine väljale **Otsi veel toiminguid** ja valige **DB2 - Kustuta rida (eelvaade)**.
6. Valige toiming **saada read (eelvaade)** **ühenduse muutmine**. 
7. Valige paanil **ühendused** konfiguratsioone olemasoleva ühenduse. Näiteks valige **hisdemo2**.

    ![](./media/connectors-create-api-db2/Db2connectorChangeConnection.png)

8. **Tabeli nime** loendis Valige olevat **allanoolt**ja valige **ala**.
9. Sisestage väärtused kõiki vajalikke veerud (vt punane tärn). Näiteks tippige `99999` **AREAID**jaoks. 
10. Valige **Salvesta**. 

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowValues.png)

11. Valige **Db2deleteRow** labale **Kokkuvõte**, klõpsake jaotises **kõik töötab** loendis esimese loendis üksuse (kõige uuemad Käivita).
12. **Loogika rakenduse käivitamine** tera, valige **Käivita üksikasjad**. Valige loendis **toiming** sees **Get_rows**. Vt väärtus **olek**, mis peaks olema **õnnestus**. Valige **link sisendina** sisendeid kuvamiseks. Valige **link väljundid**ja vaadata väljundeid; mida peaks sisaldama kustutatud rida.

    ![](./media/connectors-create-api-db2/Db2connectorDeleteRowOutputs.png)

## <a name="technical-details"></a>Tehnilised andmed

## <a name="actions"></a>Toimingud
Toimingu on poolt määratletud loogika rakenduse töövoo toimingu. Konnektor DB2 andmebaas sisaldab järgmisi toiminguid. 

|Toiming|Kirjeldus|
|--- | ---|
|[GetRow](connectors-create-api-db2.md#get-row)|Ühe rea toob DB2 tabelist|
|[Täitmisaeg](connectors-create-api-db2.md#get-rows)|Toob DB2 tabeli read|
|[InsertRow](connectors-create-api-db2.md#insert-row)|Lisab uue rea DB2 tabelisse|
|[DeleteRow](connectors-create-api-db2.md#delete-row)|Kustutab DB2 tabeli rea|
|[GetTables](connectors-create-api-db2.md#get-tables)|Tabelite toob DB2 andmebaasist|
|[UpdateRow](connectors-create-api-db2.md#update-row)|Mõne olemasoleva DB2 tabeli rea värskendused|

### <a name="action-details"></a>Toimingu üksikasjad

Selles jaotises iga toimingu, sh nõutav või valikuline Sisestuskeel atribuudid ja mis tahes vastava väljundi seostatud konnektor teatud üksikasjade kuvamiseks.

#### <a name="get-row"></a>Rea hankimine 
Ühe rea toob DB2 tabelist.  

| Atribuudi nimi| Kuvatav nimi |Kirjeldus|
| ---|---|---|
|tabeli * | Tabeli nimi |DB2 tabeli nimi|
|ID * | Rea id |Rea toomiseks ainuidentifikaator|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
Üksuse

| Atribuudi nimi | Andmetüüp |
|---|---|
|ItemInternalId|string|


#### <a name="get-rows"></a>Ridade hankimine 
Toob DB2 tabeli read.  

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tabeli *|Tabeli nimi|DB2 tabeli nimi|
|$skip|Jäta loendamine|Vahele kirjete arv (vaikimisi = 0)|
|$top|Suurim lubatud Get loendamine|Suurim tuua kirjete arv (vaikimisi = 256)|
|$filter|Päringu filtreerimine|ODATA filtri päringu kirjete arvu piiramine|
|$orderby|Järjestusalus|Päring ODATA orderBy määratlemiseks järjestuse kirjed|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
ItemsList

| Atribuudi nimi | Andmetüüp |
|---|---|
|väärtus|massiiv|


#### <a name="insert-row"></a>Rea lisamine 
Lisab uue rea DB2 tabelisse.  

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tabeli *|Tabeli nimi|DB2 tabeli nimi|
|üksuse *|Rea|Rea DB2 määratud tabeli lisamine|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
Üksuse

| Atribuudi nimi | Andmetüüp |
|---|---|
|ItemInternalId|string|


#### <a name="delete-row"></a>Rea kustutamine 
Kustutab DB2 tabeli rea.  

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tabeli *|Tabeli nimi|DB2 tabeli nimi|
|ID *|Rea id|Rea kustutamine ainuidentifikaator|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad
Mitte keegi.

#### <a name="get-tables"></a>Tabelite hankimine 
Tabelite toob DB2 andmebaasist.  

On selle kõne parameetreid pole. 

##### <a name="output-details"></a>Väljundi üksikasjad 
TablesList

| Atribuudi nimi | Andmetüüp |
|---|---|
|väärtus|massiiv|

#### <a name="update-row"></a>Real Värskenda 
Mõne olemasoleva DB2 tabeli rea värskendused.  

|Atribuudi nimi| Kuvatav nimi|Kirjeldus|
| ---|---|---|
|tabeli *|Tabeli nimi|DB2 tabeli nimi|
|ID *|Rea id|Rea värskendamiseks ainuidentifikaator|
|üksuse *|Rea|Rea väärtustega värskendatud|

Tärn (*) tähendab, et see atribuut on nõutav.

##### <a name="output-details"></a>Väljundi üksikasjad  
Üksuse

| Atribuudi nimi | Andmetüüp |
|---|---|
|ItemInternalId|string|


### <a name="http-responses"></a>HTTP vastused

Tehes kõned erinevaid toiminguid, saate teatud vastuseid. Järgmises tabelis kirjeldatakse vastuseid ja nende kirjeldused:  

|Nimi|Kirjeldus|
|---|---|
|200|Ok|
|202|Aktsepteeritud|
|400|Vigane päring|
|401|Volitused|
|403|Keelatud|
|404|Ei leitud|
|500|Sisemine serveritõrge. Tundmatu tõrge|
|Vaikimisi|Toiming nurjus.|


## <a name="supported-db2-platforms-and-versions"></a>Toetatud platvormid DB2 ja versioonid
Selle konnektori toetab järgmiste IBM DB2 platvormide ja versioonid, samuti IBM DB2 ühilduvad tooted (nt IBM Bluemix dashDB), mis toetavad jaotatud relatsiooniline andmebaas arhitektuur (DRDA) SQL Access Manager (SQLAM) versiooni 10 ja 11.

- IBM DB2 z/OS 11.1
- IBM DB2 z/OS 10,1
- IBM DB2 i 7,3
- IBM DB2 i 7.2
- IBM DB2 i 7.1
- IBM DB2 LUW 11
- IBM DB2 LUW 10.5


## <a name="next-steps"></a>Järgmised sammud

[Loogika rakenduse loomine](../app-service-logic/app-service-logic-create-a-logic-app.md). Tutvuge on saadaval konnektorid loogika rakendustes meie [API-de loendis](apis-list.md).

