<properties
    pageTitle="Ühenduse loomine SQL-andmebaasi SQL Server Management Studio kasutamine Azure RemoteApp | Microsoft Azure'i"
    description="Selles õppetükis saate teada, kuidas kasutada SQL Server Management Studio Azure RemoteApp turvalisuse ja jõudluse SQL-andmebaasiga ühenduse loomisel abil"
    services="sql-database"
    documentationCenter=""
    authors="adhurwit"
    manager="jhubbard"/>

<tags
    ms.service="sql-database"
    ms.workload="data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/05/2016"
    ms.author="adhurwit"/>

# <a name="use-sql-server-management-studio-in-azure-remoteapp-to-connect-to-sql-database"></a>SQL Server Management Studio kasutamine Azure RemoteApp SQL-i andmebaasiga ühenduse loomiseks

## <a name="introduction"></a>Sissejuhatus  
Selle õpetuse näidatakse, kuidas kasutada SQL Server Management Studio (SSMS) Azure RemoteApp SQL-andmebaasiga ühenduse loomiseks. Juhendab teid protsessi SQL Server Management Studio Azure RemoteApp häälestamiseks, selgitatakse eeliste ja kuvab turbefunktsioonid, mille abil saate Azure Active Directory.

**Hinnanguline aega:** 45 minutit

## <a name="ssms-in-azure-remoteapp"></a>SSMS Azure RemoteApp

Azure RemoteApp on RDS teenus Azure, mis pakub rakendusi. Saate lisateavet selle kohta siin: [mis on RemoteApp?](../remoteapp/remoteapp-whatis.md)

Azure RemoteApp töötab SSMS annab teile samamoodi nagu SSMS kohalikult töötab.

![Kuvatõmmis SSMS Azure RemoteApp töötab?][1]



## <a name="benefits"></a>Eelised

Seal on palju kasu SSMS kasutamine Azure RemoteApp, sh:

- Port 1433 Azure SQL serveris ei ole puutuda väliselt (väljaspool Azure).
- Ei ole vaja säilitada, lisamine ja eemaldamine tulemüüri Azure SQL serveri IP-aadressid.
- Kõik Azure RemoteApp ühendused ilmneda https pordi 443 kaudu krüptitud kaugtöölaua protokoll
- See on mitme kasutaja ja saate skaala.
- On jõudluse kasum SSMS SQL-andmebaasi sama piirkonna.
- Saate audit Azure RemoteApp kasutamine koos Azure Active Directory, mis on kasutaja tegevuste aruannete Premium väljaannet.
- Saate lubada mitmikautentimise (MFA).
- Juurdepääs kõikjal kasutamisel mis tahes toetatud Azure RemoteApp kliendid, mis sisaldab iOS-i, Android, Mac-arvutisse, Windows Phone ja Windows Arvutis SSMS.


## <a name="create-the-azure-remoteapp-collection"></a>Azure RemoteApp saidikogumi loomine

Siin on luua oma Azure RemoteApp saidikogumi SSMS juhiseid.


### <a name="1-create-a-new-windows-vm-from-image"></a>1. luua uue Windows VM pilt
Teie uus VM tegemiseks kasutada galeriist pilt "Windows Server Remote Desktop seansi hosti Windows Server 2012 R2".


### <a name="2-install-ssms-from-sql-express"></a>2 SSMS installima SQL Express

Avage uus VM peale ja selle allalaadimise lehelt: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/en-us/download/details.aspx?id=42299)

On võimalus alla laadida ainult SSMS. Pärast alla laadida, valige installi kataloogi ja SSMS installimiseks installiprogramm.

Samuti peate installima SQL Server 2014 Service Pack 1. Saate alla laadida siit: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/en-us/download/details.aspx?id=46694)

SQL serveri 2014 Service Pack 1 sisaldab olulisi funktsioone töötamiseks Azure'i SQL-andmebaasi.


### <a name="3-run-validate-script-and-sysprep"></a>3. kontrolli skripte ja Sysprep käivitamine

Töölaual VM PowerShelli skripti nimetatakse Valideeri. Käivitage see topeltklõpsates. Kontrollib, kas VM on valmis remote majutusteenuse rakendusi kasutada. Kui kinnitamine on lõpule jõudnud, siis palun käivitage sysprep – seda käivitamiseks valige.

Kui sysprep on lõpule jõudnud, kuvatakse sulgeda VM.

Azure RemoteApp pildi loomise kohta leiate lisateavet artiklist: [Kuidas luua Azure RemoteApp malli pilti](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)


### <a name="4-capture-image"></a>4 pildi jäädvustamiseks

Kui VM on töötamise lõpetanud, otsige see praeguse portaali ja selle jäädvustada.

Jäädvustada kohta leiate lisateavet teemast [Azure Windows virtuaalse masina loodud mudeliga klassikaline juurutamise kuvatõmmist](../virtual-machines/virtual-machines-windows-classic-capture-image.md)


### <a name="5-add-to-azure-remoteapp-template-images"></a>5. Azure RemoteApp mallide piltide lisamine

Azure RemoteApp jaotis praegune portaali, minge menüüsse malli pilti ja klõpsake nuppu Lisa. Hüpikaknas, valige "Import pilt Virtuaalmasinates teegist" ja valige soovitud pilt, mille äsja loodud.



### <a name="6-create-cloud-collection"></a>6. cloud saidikogumi loomine

Praeguse portaalis luua uue Azure RemoteApp pilve. Valige mall pilt mis vast imporditud SSMS installitud.

![Uue cloud saidikogumi loomine][2]


### <a name="7-publish-ssms"></a>7. SSMS avaldamine

Avalda oma uue cloud saidikogumi, valige menüü avalda rakenduse menüüst Start ja seejärel valige SSMS loendist.

![Rakenduse avaldamine][5]

### <a name="8-add-users"></a>8 kasutajate lisamine

Kasutajate juurdepääsu vahekaardil saate valida kasutajad, mis on selles Azure RemoteApp kogum, mis sisaldab ainult SSMS juurdepääs.

![Kasutaja lisamine][6]


### <a name="9-install-the-azure-remoteapp-client-application"></a>9. installida Azure RemoteApp klientrakendus

Saate alla laadida ja installida Azure RemoteApp kliendi siit: [Laadi alla | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)



## <a name="configure-azure-sql-server"></a>Azure SQL serveri konfigureerimine

Tagada, et Azure'i teenused on lubatud tulemüüri on vaja ainult konfiguratsiooni. Kui kasutate seda lahendust, siis pole vaja lisada tulemüüri avamiseks mis tahes IP-aadressid. Võrguliiklust, mis on lubatud SQL serveriga on muude Azure'i teenuste.


![Azure'i lubamine][4]



## <a name="multi-factor-authentication-mfa"></a>Mitmikautentimise (MFA)

MFA saab lubada selle rakenduse jaoks eraldi. Avage menüü rakendused oma Azure Active Directory. Kirje leiate Microsoft Azure RemoteApp. Kui klõpsate selle rakenduse ja konfigureerimist, kuvatakse lehe allpool, kui lubate MFA selle rakenduse jaoks.

![MFA lubamine][3]



## <a name="audit-user-activity-with-azure-active-directory-premium"></a>Auditilogi kasutaja tegevuste Azure Active Directory Premium

Kui teil pole Azure AD Premium, siis peate kataloogi jaotises litsentsid sisselülitamine. Premium lubatud, saate määrata kasutajatele Premium tase.

Kui lähete kasutaja oma Azure Active Directory, saate minna siis tegevuste menüü kuvamiseks Azure RemoteApp sisselogimise andmeid.



## <a name="next-steps"></a>Järgmised sammud

Pärast ülaltoodud juhiseid, saab sisselogimine on määratud kasutaja saab käitada Azure RemoteApp klient ja. Ilmub koos SSMS ühe rakenduste ja käivitage see nii, nagu teeksite, kui see on installitud teie arvutisse juurdepääs Azure SQL Server.

Kuidas teha SQL-andmebaasiga ühenduse kohta leiate lisateavet teemast [ühenduse SQL-andmebaasi SQL Server Management Studio ja teha valimi T-SQL-päringu](sql-database-connect-query-ssms.md).


Mis on nüüd kõik. Nautige!



<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png
