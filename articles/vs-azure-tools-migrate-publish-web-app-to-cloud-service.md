<properties
   pageTitle="Kuidas migreerida ja avaldada veebirakenduse on Azure pilveteenusesse Visual Studio | Microsoft Azure'i"
   description="Saate teada, kuidas migreerimine ja avaldada oma veebirakenduse on Azure pilveteenusesse Visual Studio abil."
   services="visual-studio-online"
   documentationCenter="na"
   authors="TomArcher"
   manager="douge"
   editor="" />
<tags
   ms.service="multiple"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="multiple"
   ms.date="08/15/2016"
   ms.author="tarcher" />

# <a name="how-to-migrate-and-publish-a-web-application-to-an-azure-cloud-service-from-visual-studio"></a>Kuidas: migreerimine ja Visual Studio on Azure pilveteenusesse veebirakenduse avaldamine

Võtta ära majutusteenuste pakkumiseks ja skaleeritavus Azure, võiksite migreerida ja avaldada oma veebirakenduse on Azure pilveteenusesse. Saate käivitada veebirakenduse Azure minimaalsete muudatustega olemasoleva rakenduse.

>[AZURE.NOTE] See teema on juurutamise pilveteenustega ei veebisaitide abil. Juurutamist veebisaitide kohta leiate teavet teemast [Deploy web appi teenuses Azure rakenduse](./app-service-web/web-sites-deploy.md).

Teatud Mallid, mis on toetatud nii Visual C# ja Visual Basic loendi leiate jaotisest **Toetatud projekti Mallid** selle teema allpool olevatest.

Peate esmalt lubama oma veebirakenduse Azure Visual Studio. Võtme juhiseid avaldada oma olemasoleva veebirakenduse, lisades on Azure projekt juurutamiseks kasutada järgmisel joonisel. Selle protsessi lisatakse teie lahendus on nõutav web rolliga Azure projekt. Sõltuvalt web projekti, et teil on, projekti atribuutide assemblereid värskendatakse ka kui paketi teenus nõuab täiendavate assemblereid juurutamiseks.

![Veebirakenduse Microsft Azure avaldamine](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC748917.png)

>[AZURE.NOTE] Funktsiooni **teisendada**, käsk **teisendamine Azure pilveteenuse teenuse Project** kuvatakse ainult teie lahendus web projekti. Näiteks käsk pole saadaval Silverlighti Projecti ja teie lahendus.
Teenuse paketi loomine või Azure rakenduse avaldamisel, hoiatused või tõrked võivad ilmneda. Need hoiatused ja tõrked aitavad teil enne juurutamist Azure seotud probleemide lahendamine. Näiteks võite saada hoiatust puuduvad komplekti. Lisateavet selle kohta, kuidas hoiatusi käsitleda tõrgete kohta leiate teemast [Azure pilveteenuse teenuse Project Visual Studio konfigureerimine](vs-azure-tools-configuring-an-azure-project.md). Kui rakenduse koostamine, käivitage see kohalikult abil Arvuta emulaator või selle avaldada Azure, võidakse kuvada järgmine tõrketeade aknas **Tõrge loend** : **määratud tee ja faili nimi on liiga pikk**. See tõrge ilmneb täielikult kvalifitseeritud Azure'i projekti nime pikkus on liiga pikk. Täielik tee, sh projekti nime pikkus ei tohi olla rohkem kui 146 märki. See on näiteks täielik projekti nime sh faili tee Azure'i projekti, mis on loodud Silverlighti rakenduse: `c:\users\<user name>\documents\visual studio 2015\Projects\SilverlightApplication4\SilverlightApplication4.Web.Azure.ccproj`. Peate oma lahenduse teisaldamiseks muu kausta, mis on lühem tee vähendada täielikult kvalifitseeritud projekti nime pikkus.

Migreerimine ja avaldada veebirakenduse Azure Visual Studio, toimige järgmiselt.

## <a name="enable-a-web-application-for-deployment-to-azure"></a>Juurutamise Azure veebirakenduse lubamine

### <a name="to-enable-a-web-application-for-deployment-to-azure"></a>Veebirakenduse juurutamiseks Azure lubamiseks

1. Oma veebirakenduse juurutamiseks Azure lubamiseks teie lahendus web projekti kiirmenüü avamine ja valige Azure juurutamise projekti lisada.

    Järgmised toimingud:

    - Mõne Azure'i projekti nimega `<name of the web project>.Azure` lisatakse rakenduse lahendus.

    - Azure'i projekti lisatakse web project web roll.

    - **Kohaliku koopia** atribuut on seatud väärtusele tõene jaoks vajalike MVC MVC 2, MVC 3, mis tahes assemblereid 4 ja Silverlight ärirakenduste. Sellega lisatakse need assemblereid pakett, mida kasutatakse juurutamiseks.

  >[AZURE.IMPORTANT] Kui teil on muud komplektid või faile, mis on selles veebirakenduse jaoks nõutavad, peate määrama käsitsi need failid atribuudid. Neid atribuute seadmise kohta leiate teavet jaotisest **Teenuse faile lisada** selle artikli.

  >[AZURE.NOTE] Teatud web project web roll juba olemas lahendus, **teisendamine**, Azure Projectis **teisendamine Azure pilveteenuse teenuse Project** ei kuvata selle web projekti kiirmenüü.

  Kui teil on mitu web projektide veebirakenduses ja soovite luua iga web project web rollid, peate tegema iga web projekti selle protseduuri käigus pakutakse juhiseid. See loob eraldi Azure projektide iga web roll. Iga web projekti eraldi avaldama. Teise võimalusena saate käsitsi lisada teise web rolli Azure'i projekti oma veebirakenduse. Selle tegemiseks Azure projektis **rollid** kausta kiirmenüü avamine, nuppu **Lisa**ja seejärel **Web rolli projekti lahenduse**, valige project web rolli lisada, ja seejärel valige nupp **OK** .

## <a name="use-an-azure-sql-database-for-your-application"></a>SQL Azure'i andmebaasi rakenduse kasutamine

Kui teil on oma veebirakenduse, mis kasutab SQL Serveri andmebaasiga, mis asub tööruumides ühendusstringi, peate muutma selle ühendusstringi eksemplar SQL-andmebaasi kasutama Azure'i majutatakse asemel.

>[AZURE.IMPORTANT] Teie tellimus tuleb lubada SQL-andmebaasi kasutamiseks. Kui teil on juurdepääs teie tellimus [Azure klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885), saate määrata, milliseid teenuseid teie tellimus sisaldab. Järgmised juhised kehtivad vabastatud [Azure klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885). Kui kasutate [Azure portaali](http://portal.microsoft.com), jätkake järgmise toimingu.

### <a name="to-use-a-sql-database-instance-in-your-web-role-for-your-connection-string"></a>SQL-andmebaasi eksemplari kasutada oma web rolli teie ühendusstring

1. Luua eksemplar SQL-andmebaasi [Azure klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885), järgige järgmises artiklis toodud juhiseid: [SQL-andmebaasi serveri loomine](http://go.microsoft.com/fwlink/?LinkId=225109).

    >[AZURE.NOTE] Kui häälestate oma eksemplari SQL-andmebaasi tulemüüri reeglid, valige märkeruut **Luba muude Azure'i teenuste juurde pääseda selles serveris** .

1. Eksemplari SQL-andmebaasi kasutamiseks oma ühendusstring loomiseks järgige järgmises artiklis järgmise jaotise: [SQL-andmebaasi loomine](http://go.microsoft.com/fwlink/?LinkId=225110).

1. Kasutada oma ühendusstring ühendusstringi ADO.net-i kopeerimiseks järgmiste toimingute [Azure klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885).  

  1. Valige nupp **andmebaas** ja avage sõlm tellimus, mida kasutasite oma eksemplari SQL-andmebaasi loomiseks.

  1. Saadaval esinemisjuhud SQL-andmebaasi kuvamiseks valige **SQL andmebaase** sõlm.

  1. Andmebaasi atribuutide kuvamiseks valige andmebaas. Vaate **Atribuudid** kuvatakse.

      >[AZURE.NOTE] Kui **Atribuudid** vaadet ei kuvata, peate seda eraldajat abil avada.

  1. Ühenduse stringid kuvamiseks nuppu vaate kõrval kolmikpunkti (…).

    Kuvatakse dialoogiboks **Ühendusstringi** .

  1. Kopeerige ühendusstring ADO.net-i, tõstke muudetav tekst esile ja valige klahve Ctrl + C.

  1. Dialoogiboksi sulgemiseks nuppu **Sule** .

1. Ühendusstringi eksemplari SQL-andmebaasi kasutama faili web.config asendamiseks avage fail, esile tõsta olemasoleva ühenduse stringi kirje ja valige klahve Ctrl + V. SQL-andmebaasi eksemplari ADO.NET ühendusstringi asendab olemasoleva ühendusstring.

1. Samuti peate lisama parameetri `MultipleActiveResultSets=True` ühendusstringi. Ühendusstringi peaks olema järgmises vormingus:

    ```
    connectionString=”Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"
    ```

1. (Valikuline) Alternatiivset võimalust muutmise ühendusstringi otse fail on jaotise lisada faile web.config teisendus Koosta konfiguratsioonist, mille abil saate luua oma pakett. Avage fail Web.Debug.Config või Web.Release.Config faili. Lisage sellesse faili järgmisest jaotisest.

    ```
    XMLCopy<connectionStrings><addname="DefaultConnection"connectionString="Server=tcp:<database_server>.database.windows.net,1433;Database=<database_name>;User ID=<user_name>@<database_server>;Password=<myPassword>;Trusted_Connection=False;Encrypt=True;MultipleActiveResultSets=True"xdt:Transform="SetAttributes"xdt:Locator="Match(name)"/></connectionStrings>
    ```

1. Salvestage fail, mida saate muuta ja rakenduse uuesti avaldada.

### <a name="to-use-an-instance-of-sql-database-by-using-the-azure-classic-portal"></a>Eksemplari SQL-andmebaasi kasutama Azure'i klassikaline portaali abil

1. [Azure'i klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885)valige sõlm SQL andmebaase.

  - Kui kuvatakse eksemplar SQL-andmebaasi, mida soovite kasutada, valige selle avamiseks.

  - Kui te pole loonud esinemisvõimaluse, valige vastav link ja seejärel looge eksemplari.

1. Kui avate või luua andmebaasi eksemplari, valige **Ühendusstringi** link.

1. Valige lehe allosas linki tulemüüri sätete, konfigureerimine ja aktsepteerimiseks vaikeväärtusi või väärtused, mida teil on vaja konfigureerida.

1. Kopeerige ühendusstring ADO.net-i, kleepige web.config faili üle kohapealsesse andmebaasi vana ühendusstringi ja veenduge, et lisada `MultipleActiveResultSets=True`.

## <a name="publish-a-web-application-to-azure"></a>Veebirakenduse Azure avaldamine

### <a name="to-publish-a-web-application-to-azure"></a>Veebirakenduse Azure avaldada.

1. Testida rakenduse kohalikku keskkonda, kasutades funktsiooni Azure'i arvutada emulaator, Azure project web rolli kohta kiirmenüü avamine ja valige käsk **Määra käivitus projekti**. Valige **silumine**, **Käivitage silumine** (klaviatuuri: **F5**).

    Avaneb dialoogiboks **käivitamine Azure'i silumine keskkonna** ja käivitub brauseris. Üksikasjalikku teavet selle kohta, kuidas alustada igat tüüpi veebirakenduse Arvuta emulaator, teemast selles jaotises Tabel.

1. Häälestada rakenduse avaldamiseks Azure'i teenuseid, peab teil Microsofti kontot ja Azure tellimus. Kasutage juhiseid järgmises teemas, et teenuste häälestamine: [avaldamine või kasutada Azure rakenduse Visual Studio ettevalmistamine](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md).

1. Azure'i veebirakenduse avaldamiseks web project kiirmenüü avamine ja valige käsk **Avalda Azure**.

    Kuvatakse dialoogiboks **Azure teenuserakenduse avaldamine** ja Visual Studio käivitatakse juurutamise protsessi. Lisateavet selle kohta, kuidas avaldada rakenduse jaotisest **avaldada Azure rakenduse Visual Studio** [avaldamise pilveteenus Azure tööriistade abil](vs-azure-tools-publishing-a-cloud-service.md).

    >[AZURE.NOTE] Lisaks saate avaldada Azure projekti veebirakenduse. Selle tegemiseks Azure'i projekti kiirmenüü avamine ja valige käsk **Avalda**.

1. Juurutamise edenemise kuvamiseks saate vaadata akna **Azure'i tegevuste Logi** . Juurutamise protsessi käivitamisel kuvatakse see Logi automaatselt. Saate laiendada reaüksuse tegevuste Logi kuvamiseks üksikasjalikku teavet, nagu on näidatud järgmisel joonisel:

    ![VST_AzureActivityLog](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744149.png)

1. (Valikuline) Juurutamise tühistamiseks rea üksuse kiirmenüü avamine tegevuste Logi ja valige **tühistamine ja eemaldada**. See peatab juurutamise töö ja kustutab Azure juurutamine keskkonnas.

    >[AZURE.NOTE] Selles juurutamise keskkonnas kui see on juurutatud eemaldamiseks peate kasutama [Azure'i klassikaline portaalis](http://go.microsoft.com/fwlink/?LinkID=213885).

1. (Valikuline) Pärast algama oma rolli aknad, kuvatakse Visual Studio juurutamine keskkonnas **Cloud Explorer** või **Server Explorer** **Azure Arvuta** sõlme automaatselt. Siin saate vaadata üksikuid rolli aknad olekut.

    Järgmisel joonisel on kujutatud rolli aknad **Server** Explorer samal ajal, kui need on endiselt Initializing riigis:

    ![VST_DeployComputeNode](./media/vs-azure-tools-migrate-publish-web-app-to-cloud-service/IC744134.png)

1. Pärast juurutuse oma rakenduse avamiseks valige juurutamise kõrval olevat noolt **Azure'i tegevuse log**olek on **lõpule viidud** kuvamisel. Kuvatakse Azure oma veebirakenduse URL-i. Leiate Azure'i mõnda kindlat tüüpi veebirakendus käivitamine üksikasjad järgmisest tabelist.

    Järgmises tabelis on loetletud teavet selle kohta, kuidas alustada teatud veebirakenduste Azure'i või käivitada või silumine kohalikult abil arvutada Azure emulaator veebirakenduse:

  	|Web rakenduse tüüp|Käivita/silumine Arvuta emulaator kohalikult abil|Azure'i töötab?|
  	|---|---|---|
  	|ASP.net-i veebirakendus|Klõpsake menüüribal, valige **silumine**, **Käivitage silumine** (klaviatuuri: Valige klahvi **F5** .).|Valige **juurutamise** vahekaardil **Azure'i tegevuste Logi** laadimiseks avalehe brauseris kuvatud URL-i hüperlink.|
  	|ASP.net-i MVC 2 veebirakenduse|Klõpsake menüüribal, valige **silumine**, **Käivitage silumine** (klaviatuuri: Valige klahvi **F5** .).|Valige **juurutamise** vahekaardil **Azure'i tegevuste Logi** laadimiseks avalehe brauseris kuvatud URL-i hüperlink.|
  	|ASP.net-i MVC 3 veebirakendus|Klõpsake menüüribal, valige **silumine**, **Käivitage silumine** (klaviatuuri: Valige klahvi **F5** .).|Valige **juurutamise** vahekaardil **Azure'i tegevuste Logi** laadimiseks avalehe brauseris kuvatud URL-i hüperlink.|
  	|ASP.net-i MVC 4 veebirakendus|Klõpsake menüüribal, valige **silumine**, **Käivitage silumine** (klaviatuuri: Valige klahvi **F5** .).|Valige **juurutamise** vahekaardil **Azure'i tegevuste Logi** laadimiseks avalehe brauseris kuvatud URL-i hüperlink.|
  	|ASP.net-i Tühjenda veebirakendus|Peate lisama oma rakenduse nimega Avaleht määratavad web projekti aspx-lehe. Klõpsake menüüribal, valige **silumine**, **Käivitage silumine** (klaviatuuri: Valige klahvi **F5** .).|Kui teil on vaikimisi aspx-lehe rakenduse, valige **juurutamise** vahekaardil kuvatud **Azure'i tegevuste Logi** ja selle lehe URL-i hüperlingi laaditakse brauseris. Kui teil on mõne muu aspx-lehe, peate oma URL-i järgmises vormingus seda teatud lehele liikumiseks:`<url for deployment>/<name of page>.aspx`|
  	|Silverlighti rakenduse|Klõpsake menüüribal, valige **silumine**, **Käivitage silumine** (klaviatuuri: Valige klahvi **F5** .).|Peate oma URL-i järgmises vormingus rakenduse teatud lehele liikumiseks:`<url for deployment>/<name of page>.aspx`|
  	|Silverlighti Ärirakendusele|Klõpsake menüüribal, valige **silumine**, **Käivitage silumine** (klaviatuuri: Valige klahvi **F5** .).|Peate oma URL-i järgmises vormingus rakenduse teatud lehele liikumiseks:`<url for deployment>/<name of page>.aspx`|
  	|Silverlighti navigeerimise rakenduse|Klõpsake menüüribal, valige **silumine**, **Käivitage silumine** (klaviatuuri: Valige klahvi **F5** .).|Peate oma URL-i järgmises vormingus rakenduse teatud lehele liikumiseks:`<url for deployment>/<name of page>.aspx`|
  	|WCF-i teenuserakenduse|Peate määrama .svc fail nimega Avaleht WCF-teenus projekti jaoks. Klõpsake menüüribal, valige **silumine**, **Käivitage silumine** (klaviatuuri: Valige klahvi **F5** .).|Peate liikumiseks svc rakenduse abil oma URL-i järgmises vormingus:`<url for deployment>/<name of service file>.svc`|
  	|WCF-i töövoo teenuserakenduse|Peate määrama .svc fail nimega Avaleht WCF-teenus projekti jaoks. Klõpsake menüüribal, valige **silumine**, **Käivitage silumine** (klaviatuuri: Valige klahvi **F5** .).|Peate liikumiseks svc rakenduse abil oma URL-i järgmises vormingus:`<url for deployment>/<name of service file>.svc`|
  	|ASP.net-i dünaamiline üksused|Klõpsake menüüribal, valige **silumine**, **Käivitage silumine** (klaviatuuri: Valige klahvi **F5** .).|Peate värskendama ühendusstringi (vt järgmist jaotist). Samuti peate oma URL-i järgmises vormingus rakenduse teatud lehele liikumiseks:`<url for deployment>/<name of page>.aspx`|
  	|ASP.net-i dünaamiline andmete Linq SQL-i|Klõpsake menüüribal, valige **silumine**, **Käivitage silumine** (klaviatuuri: Valige klahvi **F5** .).|Te peate järgige selle protseduuri käigus pakutakse: SQL Azure'i andmebaasi rakenduse kasutamine (varasema selle teema jaotisest). Samuti peate oma URL-i järgmises vormingus rakenduse teatud lehele liikumiseks:`<url for deployment>/<name of page>.aspx`|

## <a name="update-a-connection-string-for-aspnet-dynamic-entities"></a>ASP.net-i dünaamiline üksuste ühendusstringi värskendus

### <a name="to-update-a-connection-string-for-aspnet-dynamic-entities"></a>ASP.net-i dünaamiline üksuste ühendusstringi värskendamiseks

1. SQL Azure'i andmebaas, mida saab kasutada ASP.net-i dünaamiline üksuste veebirakenduse loomiseks järgige käesolevas teemas **SQL Azure'i andmebaasi rakenduse kasutamiseks** juhiseid.

1. Lisage tabelid ja väljad, et peate selle andmebaasi [Azure klassikaline portaali](http://go.microsoft.com/fwlink/?LinkID=213885).

1. Seda tüüpi rakendus ühendusstringi fail on järgmises vormingus:  

    ```
    <addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;data source=<server name>\SQLEXPRESS;initial catalog=<database name>;integrated security=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

    Värskendage *connectionString* väärtuse SQL Azure'i andmebaasi ADO.NET ühendusstring järgmiselt.

    ```
    XMLCopy<addname="tempdbEntities"connectionString="metadata=res://*/Model1.csdl|res://*/Model1.ssdl|res://*/Model1.msl;provider=System.Data.SqlClient;provider connection string=&quot;Server=tcp:<SQL Azure server name>.database.windows.net,1433;Database=<database name>;User ID=<user name>;Password=<password>;Trusted_Connection=False;Encrypt=True;multipleactiveresultsets=True;App=EntityFramework&quot;"providerName="System.Data.EntityClient"/>
    ```

1. Salvestamiseks valige fail ühendusstringi menüü tehtud muudatustega **faili** **salvestamine web.config**.

## <a name="supported-project-templates"></a>Toetatud projekti Mallid

Veebirakenduse Azure avaldamiseks rakenduse peate kasutama ühte projekti Mallid C# või Visual Basic, mis on loetletud allpool tabelis.

|Projekti malli rühma|Projekti malli|
|---|---|
|Web|ASP.net-i veebirakenduse|
|Web|ASP.net-i MVC 2 veebirakenduse|
|Web|ASP.net-i MVC 3 veebirakenduse|
|Web|ASP.net-i MVC4 veebirakenduse|
|Web|ASP.net-i Tühjenda veebirakendus|
|Web|ASP.net-i MVC 2 tühja veebirakenduse|
|Web|ASP.net-i dünaamiline andmete üksuste veebirakendus|
|Web|ASP.net-i dünaamiline andmete Linq SQL-i veebirakenduse|
|Silverlighti|Silverlighti rakenduse|
|Silverlighti|Silverlighti Ärirakendusele|
|Silverlighti|Silverlighti navigeerimise rakenduse|
|WCF-I|WCF-i teenuserakenduse|
|WCF-I|WCF-i töövoo teenuserakenduse|
|Töövoo|WCF-i töövoo teenuserakenduse|

## <a name="next-steps"></a>Järgmised sammud
Avaldamise kohta leiate lisateavet teemast [Avalda või Deploy Azure'i rakenduse Visual Studio ettevalmistamine](vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md). Lugege lisaks ka [Säte üles nimega autentimiseks mandaadi](vs-azure-tools-setting-up-named-authentication-credentials.md).
