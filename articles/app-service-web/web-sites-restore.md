<properties 
    pageTitle="Rakenduse Azure taastamine" 
    description="Saate teada, kuidas oma rakenduse taastamine varukoopia põhjal." 
    services="app-service" 
    documentationCenter="" 
    authors="cephalin" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/06/2016" 
    ms.author="cephalin"/>

# <a name="restore-an-app-in-azure"></a>Rakenduse Azure taastamine

Selles artiklis näidatakse, kuidas taastada rakenduse [Azure'i rakenduse teenus](../app-service/app-service-value-prop-what-is.md) , mis varem varundatud (vt [rakenduse Azure varundada](web-sites-backup.md)). Saate rakenduse abil oma lingitud andmebaase (SQL-andmebaasi või MySQL-i) nõudmisel eelmise oleku taastada või luua uue rakenduse ühe algse rakenduse varukoopia põhjal. Luua uue rakenduse, mis töötab samal ajal uusim versioon võib olla kasulik a ja B testimine.

Varukoopiate põhjal taastamine on saadaval rakendust, millel on **Standardne** ja **Premium** astme. Ülespoole rakenduse kohta leiate teavet teemast [rakenduse Azure skaalal](web-sites-scale.md). **Premium** taseme võimaldab suurem arv iga päev varukoopiate teostatav kui **normaliseeritud** taseme.

<a name="PreviousBackup"></a>
## <a name="restore-an-app-from-an-existing-backup"></a>Rakenduse olemasoleva varukoopia põhjal taastamine

1. Rakenduse Azure'i portaalis enne **sätted** , klõpsake **varukoopiate** **varukoopiate** tera kuvamiseks. Klõpsake nuppu **Taasta kohe** käsuriba. 
    
    ![Valige käsk Taasta kohe][ChooseRestoreNow]

3. Esmalt valige labale **taastamine** varukoopia põhjal allikas. 

    ![](./media/web-sites-restore/021ChooseSource.png)
    
    **Rakenduse varundamise** suvand kuvatakse kõik olemasolevad varukoopiad praeguse rakenduse ja valige üks hõlpsasti. 
    **Salvestusruumi** suvandi saate valida mis tahes ZIP varufaili olemasoleva Azure Storage konto ja teie tellimus ümbrises. 
    Kui proovite taastamine varukoopia muus rakenduses, kasutage suvandit **salvestusruumi** .

4. Seejärel määrake rakenduse taastamine sihtkoht **taastamine sihtkoht**.

    ![](./media/web-sites-restore/022ChooseDestination.png)
    
    >[AZURE.WARNING] Kui valite **kirjuta**, kustutatakse kõik olemasolevad andmed oma praeguse rakenduse. Enne nupu **OK**klõpsamist veenduge, et see on täpselt, mida soovite teha.
    
    Saate valida **Olemasoleva rakenduse** muus rakenduses resoure samasse rakenduse varukoopia taastamiseks. Enne selle suvandi kasutamiseks peaksite juba loodud muus rakenduses ressursi rühma peegeldus andmebaasi konfigureerimine ühele määratletud rakenduse varukoopia. 
    
5. Klõpsake nuppu **OK**.

<a name="StorageAccount"></a>
## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Alla laadida või salvestusruumi konto varukoopia kustutamine
    
1. Peamistest **sirvimine** blade Azure portaali, valige **salvestusruumi kontod**.
    
    Olemasoleva salvestusruumi kontode loendit kuvatakse. 
    
2. Valige salvestusruumi konto, mis sisaldab varundamise, mida soovite alla laadida või kustutada.
    
    Tera salvestusruumi konto kuvatakse.

3. Valige salvestusruumi accountn tera, s.o ümbris, mille soovite
    
    ![Ümbriste kuvamine][ViewContainers]

4. Valige varufail, mida soovite alla laadida või kustutada.

    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)

5. Sõltuvalt sellest, mida soovite teha, klõpsake nuppu **alla laadida** või **kustutada** .  

<a name="OperationLogs"></a>
## <a name="monitor-a-restore-operation"></a>Taastamine toimingu jälgimine
    
1. Rakenduse taastetoimingu takistavaid üksikasjade vaatamiseks liikuge **Auditilogi** tera Azure'i portaalis. 
    
    **Auditilogide** tera kuvatakse kõik oma toimingud koos tase, olek, ressursside ja aja andmed.
    
2. Kerige soovitud toimingu taastamine ja klõpsake seda valimiseks.

Tera üksikasjad kuvatakse saadaval taastetoimingu puudutav teave.
    
## <a name="next-steps"></a>Järgmised sammud

Te saate ka varundus ja taaste rakendust Service rakenduste abil REST API-ga (vt [Kasutamine ülejäänud varundamine ja taastamine rakenduse rakendused](websites-csm-backup.md)).

>[AZURE.NOTE] Kui soovite alustada Azure'i rakendust Service enne Azure'i konto kasutajaks, minge [Proovige rakenduse teenus](http://go.microsoft.com/fwlink/?LinkId=523751), kus saate kohe luua lühiajaline starter web app rakenduse teenus. Nõutav; krediitkaardid kohustusi.


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
[StorageAccountFile]: ./media/web-sites-restore/02StorageAccountFile.png
[BrowseCloudStorage]: ./media/web-sites-restore/03BrowseCloudStorage.png
[StorageAccountFileSelected]: ./media/web-sites-restore/04StorageAccountFileSelected.png
[ChooseRestoreSettings]: ./media/web-sites-restore/05ChooseRestoreSettings.png
[ChooseDBServer]: ./media/web-sites-restore/06ChooseDBServer.png
[RestoreToNewSQLDB]: ./media/web-sites-restore/07RestoreToNewSQLDB.png
[NewSQLDBConfig]: ./media/web-sites-restore/08NewSQLDBConfig.png
[RestoredContosoWebSite]: ./media/web-sites-restore/09RestoredContosoWebSite.png
[DashboardOperationLogsLink]: ./media/web-sites-restore/10DashboardOperationLogsLink.png
[ManagementServicesOperationLogsList]: ./media/web-sites-restore/11ManagementServicesOperationLogsList.png
[DetailsButton]: ./media/web-sites-restore/12DetailsButton.png
[OperationDetails]: ./media/web-sites-restore/13OperationDetails.png
 
