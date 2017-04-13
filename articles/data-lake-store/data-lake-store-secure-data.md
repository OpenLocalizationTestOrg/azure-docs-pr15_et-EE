<properties 
   pageTitle="Azure'i andmesalve Lake talletatud andmete turvamise | Microsoft Azure'i" 
   description="Siit saate teada, kuidas tagada Azure Lake andmesalve rühmade kasutamine andmete ja juurdepääsu juhtimine loendid" 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="09/29/2016"
   ms.author="nitinme"/>

# <a name="securing-data-stored-in-azure-data-lake-store"></a>Azure'i andmesalve Lake talletatud andmete turvamise

Azure'i andmesalve Lake andmete turvamise on lähenemine on.

1. Alustuseks loomise turberühmad teenusekomplektis Azure Active Directory (AAD). Rakendada Rollipõhine juurdepääsu reguleerimine (RBAC) Azure portaali kasutatakse neid turberühmad. Lisateavet leiate [Microsoft Azure Rollipõhine juurdepääsu reguleerimine](../active-directory/role-based-access-control-configure.md).

2. Saate määrata Azure'i andmesalve Lake konto AAD turberühmad. Seda määrab juurdepääsu Lake andmesalve konto kaudu portaali ja haldamise toimingute kaudu portaali või API-d.

3. Määrata AAD turberühmad Access juhtelemendi loendid (ACL) Lake andmesalve failisüsteemis.

4. Lisaks saate ka määrata IP-aadresside vahemiku kliendid, kes pääsevad Lake andmesalve andmete jaoks.

Selles artiklis antakse juhiseid kohta, kuidas kasutada Azure portaali ülaltoodud ülesannete täitmiseks. Täpsemat teavet selle kohta, kuidas Lake andmesalve rakendab Turvalisus konto ja andmete tasemel, vaadake teemat [Turvalisus Azure'i Lake poes](data-lake-store-security-overview.md). Suure-dive ACL rakenduspõhimõtteid Azure'i andmesalve Lake kohta leiate teemast [Ülevaade juurdepääsu reguleerimine Lake andmesalve](data-lake-store-access-control.md).

## <a name="prerequisites"></a>Eeltingimused

Enne alustamist selles õpetuses, peab teil olema järgmised:

- **An Azure'i tellimus**. Leiate [Azure'i saada tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/).
- **An Azure'i andmesalve Lake konto**. Juhised, kuidas luua, leiate [Azure'i andmesalve Lake kasutamise alustamine](data-lake-store-get-started-portal.md)

## <a name="create-security-groups-in-azure-active-directory"></a>Turberühmad Azure Active Directory loomine

Juhised selle kohta, kuidas luua AAD turberühmad ja kasutajate lisamine rühma, lugege teemat [Azure Active Directory haldamine turberühmad](../active-directory/active-directory-accessmanagement-manage-groups.md).

## <a name="assign-users-or-security-groups-to-azure-data-lake-store-accounts"></a>Määrata kasutajad või turberühmad Azure'i andmesalve Lake kontod

Kui määrate kasutajad või turberühmad Azure'i andmesalve Lake kontod, siis juurdepääsu haldamise toimingute abil konto Azure portaali ja Azure ressursihaldur API-de kohta juhtida. 

1. Avage Azure'i andmesalve Lake konto. Vasakul paanil klõpsake nuppu **Sirvi**, klõpsake **Lake andmesalve**ja klõpsake keelest Lake andmesalve konto nime, millele soovite määrata kasutaja või turberühma rühma.

2. Lake andmesalve konto tera, klõpsake nuppu **sätted**. Keelest **sätted** , klõpsake nuppu **Kasutajad**.

    ![Turberühma Azure'i andmesalve Lake konto määramine] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Turberühma Azure'i andmesalve Lake konto määramine")

3. **Kasutaja** blade vaikimisi loetletud **tellimus administraatorite** rühma omaniku. 

    ![Kasutajate lisamine ja rollid] (./media/data-lake-store-secure-data/adl.add.group.roles.png "Kasutajate lisamine ja rollid")
 
    On kaks võimalust rühma lisamine ja määramine oluline rollid.

    * Kasutaja/rühma lisamine kontole ja seejärel määrata rolli, või
    * Lisada rollile ja seejärel kasutajarühmadele rollile määrata.

    Selles jaotises vaatame esimene lähenemine lisamise rühma ja seejärel määramine rollid. Sarnaselt juhiseid esmalt valige roll, ja seejärel määrata rolli rühmad, saate seda teha.
    
4. **Kasutajate** tera, nuppu **Lisa** **lisamine Accessi** tera avamiseks. Labale **lisamine Accessi** nuppu **Valige roll,**ja seejärel valige kasutaja/rühma roll.

     ![Lisa kasutaja rollid] (./media/data-lake-store-secure-data/adl.add.user.1.png "Lisa kasutaja rollid")

    **Omaniku poole** ja **kaasautori** roll pakuvad andmete lake konto juurdepääsu mitmesuguste funktsioonide haldamine. Kasutajatele, kes suhtlevad andmete järves andmeid, saate need lisada **lugeja **roll. Need rollid on piiratud haldus toimingutega Azure'i andmesalve Lake kontoga seotud.

    Andmete jaoks määratleda toimingute üksikute failisüsteemi õigused, saavad kasutajad teha. Seetõttu lugeja rolli kasutaja saab vaadata ainult kontoga seotud haldus sätted, kuid saate potentsiaalselt andmete lugemine ja kirjutamine alusel määratud failisüsteemi õigused. Lake andmesalve failisüsteemi õigused on kirjeldatud aadressil [määramine ACL Azure'i andmesalve Lake failisüsteemi turberühma](#filepermissions).



5. Klõpsake **lisamine Accessi** labale avamine **kasutajate lisamine** tera **kasutajate lisamine** . Otsige see blade lõite Azure Active Directory turberühma. Kui teil on palju rühmi, et otsida, kasutage tekstivälja ülaosas filtreerimiseks klõpsake rühma nime. Klõpsake nuppu **Vali**.

    ![Turberühma lisamine] (./media/data-lake-store-secure-data/adl.add.user.2.png "Turberühma lisamine")

    Kui soovite lisada rühma/kasutaja, mida selles loendis pole, saate neid kutsuda kasutades ikooni **Kutsu** ja täpsustades kasutaja/rühma meiliaadress.

6. Klõpsake nuppu **OK**. Peaksite nägema turberühm, nagu on allpool näidatud.

    ![Lisatud turberühm] (./media/data-lake-store-secure-data/adl.add.user.3.png "Lisatud turberühm")

7. Teie kasutaja-ja turberühma on nüüd juurdepääs Azure'i andmesalve Lake kontole. Kui soovite anda juurdepääsu kindlatele kasutajatele, saate need lisada turberühma. Samamoodi, kui soovite tühistada kasutaja juurdepääsu, saate eemaldada need turberühmast. Saate määrata ka mitu turberühmad konto. 

## <a name="filepermissions"></a>Kasutaja või turberühma määramine Azure andmesalve Lake failisüsteemi nimega ACL-ID

Azure'i andmed Lake failisüsteemi määrate kasutaja/turberühmad, saate määrata Accessi juhtelemendi Azure'i andmesalve Lake talletatud andmed.

1. Klõpsake oma Lake andmesalve konto blade **Andmete Explorer**.

    ![Loo kataloogide Lake andmesalve konto] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Andmete Lake konto loomine kataloogid")

2. **Andmete Explorer** tera, klõpsake faili või kausta, mille soovite konfigureerida ACL, ja siis käsku **juurdepääs**. Faili ACL määramiseks klõpsake **Accessi** **Faili eelvaate** keelest.

    ![Määrake ACL andmete Lake failisüsteemis] (./media/data-lake-store-secure-data/adl.acl.1.png "Määrake ACL andmete Lake failisüsteemis")

3. **Accessi** tera loetletakse standard juurdepääsu ja kohandatud Accessi juba määratud juurkausta. Klõpsake ikooni **Lisa** Kohandatud tase ACL lisamiseks.

    ![Standard- ja kohandatud loend Accessi] (./media/data-lake-store-secure-data/adl.acl.2.png "Standard- ja kohandatud loend Accessi")

    * **Standard juurdepääs** on juurdepääs UNIX-laadi, kus saate lugeda, kirjutada, käivitada (rwx) kolme omaette klassi: omanik, rühma ja teised.
    * **Kohandatud Accessi** vastab POSIX ACL, mis võimaldab teil nimega teatud kasutajate või rühmade ja mitte ainult faili omanik või rühma õiguste määramiseks. 
    
    Lisateavet leiate teemast [HDFS ACL-ID](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html#ACLs_Access_Control_Lists). ACL rakenduspõhimõtteid Lake andmesalve kohta leiate lisateavet teemast [Andmete Lake poes juurdepääsu reguleerimine](data-lake-store-access-control.md).

4. Klõpsake ikooni **Lisa** **Lisada kohandatud Accessi** tera avamiseks. See tera, klõpsake nuppu **Valige kasutaja või rühma**ja seejärel **Valige kasutaja või rühma** tera, otsige lõite Azure Active Directory turberühma. Kui teil on palju rühmi, et otsida, kasutage tekstivälja ülaosas filtreerimiseks klõpsake rühma nime. Klõpsake nuppu Lisa ja seejärel klõpsake nuppu, **Valige**soovitud rühm.

    ![Lisa rühm] (./media/data-lake-store-secure-data/adl.acl.3.png "Lisa rühm")

5. Klõpsake nuppu **Valige õigused**, valige õigused ja kas soovite määrata vaikimisi ACL õigused, kasutada ACL või mõlemad. Klõpsake nuppu **OK**.

    ![Rühmale õiguste määramine] (./media/data-lake-store-secure-data/adl.acl.4.png "Rühmale õiguste määramine")

    Lake andmesalve ja Access vaikimisi ACL õiguste kohta leiate lisateavet teemast [Andmete Lake poes juurdepääsu reguleerimine](data-lake-store-access-control.md).


6. **Lisada kohandatud Accessi** tera, klõpsake nuppu **OK**. Äsja lisatud jaotises seotud õigusi, kus on loetletud kohe **juurdepääsu** tera.

    ![Rühmale õiguste määramine] (./media/data-lake-store-secure-data/adl.acl.5.png "Rühmale õiguste määramine")

    > [AZURE.IMPORTANT] Praeguses versioonis, saate määrata ainult 9 kirjed jaotises **Kohandatud juurdepääs**. Kui soovite lisada rohkem kui 9 kasutajad, peaksite looma turberühmad, lisada kasutajate lisamine, anda juurdepääsu nende turberühmad Lake andmesalve konto.

7. Vajaduse korral saate muuta juurdepääsu õigusi, kui olete lisanud rühma. Tühjendage või märkige ruut iga õiguste tüübi (lugemine, kirjutamine, käivitamine) vastavalt kas soovite eemaldada või turberühma selle õiguste määramine. Klõpsake muudatuste salvestamiseks **Salvesta** või **Hülga** muutused tagasi võtta.

## <a name="set-ip-address-range-for-data-access"></a>Juurdepääs andmetele IP-aadresside vahemiku määramine

Azure'i andmesalve Lake võimaldab edasise juurdepääsu oma võrgu tasemel andmesalve lukustada. Saate lubada tulemüüri, IP-aadressi määramiseks või IP-aadresside vahemiku määratlemine oma usaldusväärsete klientidele. Kui lubatud, saate ainult klientidele, mis on määratletud vahemikus IP-aadresside ühenduse pood.

![Tulemüüri sätted ja IP-juurdepääs] (./media/data-lake-store-secure-data/firewall-ip-access.png "Tulemüüri sätted ja IP-aadress")

## <a name="remove-security-groups-for-an-azure-data-lake-store-account"></a>Turberühmad Azure'i andmesalve Lake konto eemaldamine

Turberühmad eemaldamisel Azure'i andmesalve Lake kontod, muudate ainult Accessi haldamise toimingute abil konto Azure portaali ja Azure ressursihaldur API-de kohta.

1. Lake andmesalve konto tera, klõpsake nuppu **sätted**. Keelest **sätted** , klõpsake nuppu **Kasutajad**.

    ![Turberühma Azure'i andmed Lake konto määramine] (./media/data-lake-store-secure-data/adl.select.user.icon.png "Turberühma Azure'i andmed Lake konto määramine")

2. Klõpsake **kasutajate** tera turberühm, mille soovite eemaldada.

    ![Turberühma eemaldamine] (./media/data-lake-store-secure-data/adl.add.user.3.png "Turberühma eemaldamine")

3. Turberühma labale nuppu **Eemalda**.

    ![Turberühma eemaldatud] (./media/data-lake-store-secure-data/adl.remove.group.png "Turberühma eemaldatud")

## <a name="remove-security-group-acls-from-azure-data-lake-store-file-system"></a>Turberühma ACL-ide eemaldamine Azure'i andmesalve Lake failisüsteemis

Turberühmad ACL eemaldamisel Azure'i andmesalve Lake failisüsteemi muudate Accessi Lake andmesalve andmeid.

1. Klõpsake oma Lake andmesalve konto blade **Andmete Explorer**.

    ![Andmete Lake konto loomine kataloogid] (./media/data-lake-store-secure-data/adl.start.data.explorer.png "Andmete Lake konto loomine kataloogid")

2. **Andmete Explorer** tera, klõpsake faili või kausta, mille soovite eemaldada ACL ja seejärel klõpsake oma konto tera, **Accessi** ikooni. Faili ACL eemaldamiseks peate klõpsama **Accessi** **Faili eelvaate** keelest.

    ![Määrake ACL andmete Lake failisüsteemis] (./media/data-lake-store-secure-data/adl.acl.1.png "Määrake ACL andmete Lake failisüsteemis")

3. Klõpsake **Accessi** labale jaotises **Kohandatud Accessi** turberühm, mille soovite eemaldada. **Kohandatud Accessi** tera, klõpsake nuppu **Eemalda** ja seejärel nuppu **OK**.

    ![Rühmale õiguste määramine] (./media/data-lake-store-secure-data/adl.remove.acl.png "Rühmale õiguste määramine")


## <a name="see-also"></a>Vt ka

- [Azure'i andmesalve Lake ülevaade](data-lake-store-overview.md)
- [Azure'i salvestusruumi plekid andmete kopeerimine andmesalve Lake](data-lake-store-copy-data-azure-storage-blob.md)
- [Azure'i andmeanalüüsi Lake Lake andmesalve kasutamine](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
- [Lake andmesalve Azure Hdinsightiga kasutamine](data-lake-store-hdinsight-hadoop-use-portal.md)
- [Alustamine Lake andmesalve PowerShelli abil](data-lake-store-get-started-powershell.md)
- [Kasutades .NET SDK Lake andmesalve kasutamise alustamine](data-lake-store-get-started-net-sdk.md)
- [Accessi andmete Lake poe diagnostikalogid](data-lake-store-diagnostic-logs.md)
