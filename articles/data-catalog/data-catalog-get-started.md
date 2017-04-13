<properties
    pageTitle="Andmekataloogi kasutamise alustamine | Microsoft Azure'i"
    description="Lõpuni õpetuse stsenaariumid ja Azure andmekataloogi võimalusi."
    documentationCenter=""
    services="data-catalog"
    authors="steelanddata"
    manager="jhubbard"
    editor=""
    tags=""/>
<tags
    ms.service="data-catalog"
    ms.devlang="NA"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="NA"
    ms.workload="data-catalog"
    ms.date="09/20/2016"
    ms.author="spelluru"/>

# <a name="get-started-with-azure-data-catalog"></a>Azure'i andmekataloogi kasutamise alustamine
Azure'i andmekataloogi on täielikult hallatud pilveteenuses, mis toimib registreerimise süsteemi ja ettevõtte andmeid varade discovery süsteem. Üksikasjaliku ülevaate leiate teemast [mis on Azure andmekataloogi](data-catalog-what-is-data-catalog.md).

Selle õpetuse aitab teil Azure'i andmekataloogi kasutamise alustamine. Selles õpetuses, tehke järgmist:

| Toiming | Kirjeldus |
| :--- | :---------- |
| [Sätte andmekataloogi](#provision-data-catalog) | Selle toimingu ettevalmistamise või Azure andmekataloogi häälestamine. Mida teha seda toimingut ainult siis, kui kataloogi pole seadistatud enne. Teil võib olla ainult üks andmekataloogi organisatsiooni (Microsoft Azure Active Directory domeeni) kohta isegi juhul, kui teie Azure kontoga seotud mitu tellimust. |
| [Registri andmete varad](#register-data-assets) | Selle toimingu registreerimist andmete varade AdventureWorks2014 näidisandmebaasi andmekataloogi kaudu. Registreerimine on olulised strukturaalset metaandmed, nt nimed, tüübid ja asukohad andmeallikast ja metaandmete kopeerimine kataloogi. Andmeallika ja andmete varad jäävad, kui nad on, kuid kataloogi kasutatav metaandmeid, saate neid hõlpsam leitavad ja arusaadaval viisil. |
| [Andmete varad avastamine](#discover-data-assets) | Seda toimingut saate Azure'i andmekataloogi portaali andmete varad eelmises etapis registreeritud avastamine. Pärast andmeallika on registreeritud Azure'i andmekataloogi, metaandmete on indekseeritud teenuse nii, et kasutajad saavad hõlpsalt neile vajalikule andmeid otsida. |
| [Marginaalide andmete varad](#annotate-data-assets) | Selle toimingu esitate andmete varade marginaalid (nt kirjeldus, sildid, dokumentatsiooni või ekspertide teave). See teave täiendab metaandmete ekstraktimist andmeallikast ja teha andmeallika paremini mõistetav rohkem inimesi. |
| [Ühenduse loomine andmete varad](#connect-to-data-assets) | Selle toimingu avate andmete varasid integreeritud kliendi tööriistad (nt Excel ja SQL Server Data Tools) ja -integreeritud tööriista (SQL Server Management Studio). |
| [Andmete varade haldamine](#manage-data-assets) | Käesolevas näites saate seada turvalisuse oma andmete varad. Andmekataloogi pole kasutajatele juurdepääsu andmine andmed ise. Andmeallika omanik juhtelemendid juurdepääs andmetele. <br/><br/> Andmekataloogi, võite avastada, andmeallikate ja vaate **metaandmete** seotud registreeritud kataloogi allikad. Võib tekkida olukordi, kus andmeallikate peaks olema nähtav ainult kindlatele kasutajatele või rühmadele. Need stsenaariumid, saate andmekataloogi võtta registreeritud andmete varade kataloogi sees ja määrata nähtavuse vara kuulub teile. |
| [Andmete varad eemaldamine](#remove-data-assets) | Käesolevas näites saate teada, kuidas eemaldada andmete varad andmekataloogi. |  

## <a name="tutorial-prerequisites"></a>Õpetus eeltingimused

### <a name="azure-subscription"></a>Azure'i tellimus
Azure'i andmekataloogi häälestada, peate olema omanik või kaasomaniku Azure tellimuse.

Azure'i tellimused aitavad teil korraldada pilvepõhise teenuse ressursid nagu Azure'i andmekataloogi juurdepääs. Nad aitavad saate määrata, kuidas esitatakse ressursikasutus, arveid ja väljaostetud. Iga tellimuse võib olla erinevad arveldus- ja makse häälestamise, siis võite olla erinevad tellimused ja eri lepingud, osakond, project, piirkondlike office ja jne. Iga pilveteenuses kuulub tellimus ja peab teil olema tellimus enne häälestamist Azure'i andmekataloogi. Lisateabe saamiseks vt [haldamine kontod, tellimused ja administraatorirollid](../active-directory/active-directory-how-subscriptions-associated-directory.md).

Kui teil on tellimus, saate luua tasuta prooviversiooni konto vaid paar minutit. Üksikasjad leiate [Tasuta prooviversioon](https://azure.microsoft.com/pricing/free-trial/) .

### <a name="azure-active-directory"></a>Azure Active Directory
Azure'i andmekataloogi häälestada, peate olema logitud Azure Active Directory (Azure AD) kasutaja kontoga. Peate olema omanik või kaasomaniku Azure tellimuse.  

Azure AD abil on lihtne hallata identiteedi ja juurdepääsu nii kohapealse ja pilveteenuse oma ettevõtte jaoks. Saate ühe töö või kooli konto pilve või kohapealse veebirakendusse sisse logida. Azure'i andmekataloogi kasutab Azure AD autentida Logi sisse. Lisateavet leiate teemast [mis on Azure Active Directory](../active-directory/active-directory-whatis.md).

### <a name="azure-active-directory-policy-configuration"></a>Azure Active Directory poliitika konfigureerimine

Võib tekkida olukord, saab Azure'i andmekataloogi portaali sisse logida, kuid kui proovite andmete allikas registreerimise tööriista sisse logida, kuvatakse tõrketeade, mis takistab sisselogimisel ilmneda. See tõrge võib ilmneda juhul, kui teie ettevõtte võrgus või kui te loote väljaspool ettevõtte võrku.

Registreerimise tööriist kasutab valideerimiseks kasutaja sisselogimist vastu Azure Active Directory *vormide autentimist* . Eduka sisselogimine, Azure Active Directory administraator peab lubamiseks vormide autentimist *globaalne autentimise poliitika*.

Globaalne autentimise poliitika, saate lubada autentimise eraldi sise- ja suhtevõrgus ühendused, nagu on näidatud järgmisel pildil. Sisselogimistõrgete võivad ilmneda juhul, kui loote, millest võrgu pole lubatud vormide autentimist.

 ![Azure Active Directory globaalne autentimise poliitika](./media/data-catalog-prerequisites/global-auth-policy.png)

Lisateabe saamiseks vt [poliitikate konfigureerimine autentimist](https://technet.microsoft.com/library/dn486781.aspx).

## <a name="provision-data-catalog"></a>Sätte andmekataloogi
Saate säte ainult üks andmekataloogi organisatsiooni (Azure Active Directory domeeni) kohta. Seetõttu, kui omanik või kaasomaniku Azure tellimuse, kes selle Azure Active Directory domeeni kuulub on juba loonud kataloogiga, te ei saa kataloogiga uuesti luua ka siis, kui teil on mitu Azure tellimust. Testimiseks Azure Active Directory domeeni kasutaja on loodud andmete kataloogi, [Azure'i andmekataloogi avalehel](http://azuredatacatalog.com) ja kontrollige, kas te ei näe kataloogi. Kui kataloogiga on teie eest juba loodud, järgmist vahele jätta ja minge järgmise jaotise juurde.    

1. [Andmekataloogi teenus](https://azure.microsoft.com/services/data-catalog) ja valige **Alustamine**.

    ![Azure'i andmekataloogi--turundus sihtleht](media/data-catalog-get-started/data-catalog-marketing-landing-page.png)
2. Mis on kaasomaniku Azure tellimuse omanik või kasutaja kontoga sisse logima. Järgmisel lehel kuvatakse pärast sisselogimist.

    ![Azure'i andmekataloogi--sätte andmekataloogi](media/data-catalog-get-started/data-catalog-create-azure-data-catalog.png)
3. Määrake andmekataloogi, **tellimuse** , mida soovite kasutada ja **asukoha** kataloogi jaoks **nimi** .
4. Laiendage **hinnakirjad** ja valige Azure andmekataloogi **edition** (tasuta või Standard).
    ![Azure'i andmekataloogi--valige edition](media/data-catalog-get-started/data-catalog-create-catalog-select-edition.png)
5. Laiendage **Kataloogi kasutajad** ja lisamiseks klõpsake nuppu **Lisa** kasutajate andmekataloogi. On lisatakse automaatselt selle rühma.
    ![Azure'i andmekataloogi--kasutajad](media/data-catalog-get-started/data-catalog-add-catalog-user.png)
6. Laiendage **Kataloogi administraatorid** ja lisamiseks klõpsake nuppu **Lisa** täiendavad administraatorite andmekataloogi. On lisatakse automaatselt selle rühma.
    ![Azure'i andmekataloogi--administraatorid](media/data-catalog-get-started/data-catalog-add-catalog-admins.png)
7. Klõpsake **Kataloogi luua** oma ettevõtte andmekataloogi loomiseks. Näete andmekataloogi Avaleht pärast selle loomist.
    ![Azure'i andmekataloogi--loodud](media/data-catalog-get-started/data-catalog-created.png)    

### <a name="find-a-data-catalog-in-the-azure-portal"></a>Azure'i portaalis andmete kataloogi otsimine
1. Klõpsake vahekaardil eraldi veebibrauseri või eraldi brauseriaknas [Azure portaali](https://portal.azure.com) ja logige sisse sama kontoga, mida kasutasite eelmises etapis andmekataloogi loomiseks.
2. Valige **Sirvi** ja seejärel käsku **Andmekataloogi**.

    ![Azure'i andmekataloogi--sirvimine Azure'i](media/data-catalog-get-started/data-catalog-browse-azure-portal.png) näete loodud andmekataloogi.

    ![Azure'i andmekataloogi--loendis Kuva kataloogi](media/data-catalog-get-started/data-catalog-azure-portal-show-catalog.png)
4.  Klõpsake kataloogi, mille olete loonud. Näete **Andmekataloogi** tera portaalis.

    ![Azure'i andmekataloogi--blade portaalis ](media/data-catalog-get-started/data-catalog-blade-azure-portal.png)
5. Saate vaadata andmekataloogi atribuudid ja värskendada. Näiteks klõpsake **hinnakirjad taseme** ja muutke väljaannet.

    ![Azure'i andmekataloogi--taseme hinnad](media/data-catalog-get-started/data-catalog-change-pricing-tier.png)

### <a name="adventure-works-sample-database"></a>Adventure Worksi näidisandmebaasi
Selles õpetuses registreerimist andmete varad (tabelid) AdventureWorks2014 näidisandmebaasi jaoks SQL serveri andmebaasi mootor, kuid saate kasutada mis tahes toetatud andmeallikas, kui soovite andmeid, mis on tuttav ja oma rolli jaoks oluline. Toetatud andmeallikate loendi leiate teemast [toetatud andmeallikad](data-catalog-dsr.md).

### <a name="install-the-adventure-works-2014-oltp-database"></a>Adventure Worksi 2014 OLTP andmebaasi installimine
Adventure Worksi andmebaasi toetab standard online tehingu töötlemise stsenaariumid väljamõeldud jalgratta tootja (Adventure Works tsüklit), mis sisaldab toodete müügi ja ostu. Selles õpetuses registreerimist teabe Azure'i andmekataloogi toodete kohta.

Adventure Worksi näidisandmebaasi installimiseks tehke järgmist.

1. Laadige [Adventure Works 2014 kõiki andmebaasi Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) codeplexis.
2. Teie arvutisse andmebaasi taastamiseks järgige [taastada andmebaasi varukoopia SQL Server Management Studio abil](http://msdn.microsoft.com/library/ms177429.aspx)või järgmist:
    1. Avage SQL Server Management Studio ja ühenduse loomine SQL serveri andmebaasi mootor.
    2. Paremklõpsake **andmebaasid** ja klõpsake nuppu **Taasta andmebaas**.
    3. Jaotises **Andmebaasi taastamine** **Source** suvandit **seade** ja klõpsake nuppu **Sirvi**.
    4. Klõpsake jaotises **Valige varukoopia seadmed**, klõpsake nuppu **Lisa**.
    5. Avage kaust, kuhu on **AdventureWorks2014.bak** faili, valige fail ja klõpsake nuppu **OK** , et sulgeda **Varundamise otsige fail** dialoogiboksis.
    6. Klõpsake nuppu **OK** , et sulgeda dialoogiboks **Valige varukoopia seadmed** .    
    7. Klõpsake nuppu **OK** , et sulgeda dialoogiboks **Andmebaasi taastamine** .

Nüüd saate andmeid varade Adventure Worksi näidisandmebaasi Azure'i andmekataloogi abil registreerida.

## <a name="register-data-assets"></a>Registri andmete varad

Selle ülesande kasutage tööriista registreerimise andmete varad Adventure Worksi andmebaasi registreeruma kataloogi. Registreerimine on olulised strukturaalset metaandmed, nt nimed, tüübid ja asukohad ekstraktimine andmeallika ja see sisaldab varade ja metaandmete kopeerimine kataloogi. Andmeallika ja andmete varad jäävad, kui nad on, kuid kataloogi kasutatav metaandmeid, saate neid hõlpsam leitavad ja arusaadaval viisil.

### <a name="register-a-data-source"></a>Andmeallika registreerimine

1.  [Azure'i andmekataloogi Avaleht](https://azuredatacatalog.com) ja nuppu **Andmete avaldamine**.

    ![Azure'i andmete kataloogi--andmete avaldamine nupp](media/data-catalog-get-started/data-catalog-publish-data.png)

2.  Klõpsake nuppu **Käivita rakendus** alla laadida, installida ja käitada registreerimise teie arvutis.

    ![Azure'i andmekataloogi--nuppu Käivita](media/data-catalog-get-started/data-catalog-launch-application.png)

3. Lehel **Tere tulemast** , klõpsake linki **Logi sisse** ja sisestage oma kasutajanimi ja parool.    

    ![Azure'i andmekataloogi--Tervitusleht](media/data-catalog-get-started/data-catalog-welcome-dialog.png)

4. Klõpsake lehel **Andmekataloogi Microsoft Azure'i** **SQL-serveri** ja **edasi**.

    ![Azure'i andmekataloogi--andmeallikad](media/data-catalog-get-started/data-catalog-data-sources.png)

5.  Sisestage SQL Serveri ühenduse atribuudid **AdventureWorks2014** (vt järgmist näidet) ja klõpsake nuppu **Ühenda**.

    ![Azure'i andmekataloogi--SQL Serveri ühenduse sätted](media/data-catalog-get-started/data-catalog-sql-server-connection.png)

6.  Registreerige oma andmete varade metaandmeid. Selles näites registreerimist **Tootmise ja toodete** objektide AdventureWorks tootmise nimeruumi:

    1. Laiendage **AdventureWorks2014** **Serveri hierarhia** tree, ja klõpsake nuppu **tootmise**.
    2. Valige **toode**, **ProductCategory**, **ProductDescription**ja **ProductPhoto** , kasutades klahvikombinatsiooni Ctrl + hiireklõps.
    3. Klõpsake nuppu **Teisalda valitud noolt** (**>**). See toiming viib kõik valitud objektid **objektide registreerida** loendisse.

        ![Azure'i andmekataloogi õpetuse--sirvida ja objektide valimine](media/data-catalog-get-started/data-catalog-server-hierarchy.png)
    4. Valige **kaasa eelvaade** andmete eelvaateks hetktõmmis. Hetktõmmis sisaldab iga tabeli kuni 20 kirjed, ja see on kopeerida kataloogi.
    5. Valige **Kaasa andmed profiili** kaasamiseks objekti statistika profiili andmete hetktõmmist (nt: miinimum, maksimum ja Keskmine väärtused veerus, ridade arvu).
    6. Sisestage väljale **Lisa sildid** **adventure töötab tsüklit**. See toiming lisab nende andmete varade Otsi silte. Sildid on suurepärane viis aitab kasutajatel leida registreeritud andmete allikas.
    7. Määrake nimi on **ekspert** (valikuline) andmed.

        ![Azure'i andmekataloogi õpetuse--registreerida objektid](media/data-catalog-get-started/data-catalog-objects-register.png)

    8. Klõpsake nuppu **registreeru**. Azure'i andmekataloogi registreerib teie valitud objekte. Selle toiminguga Adventure Worksi valitud objektide on registreeritud. Tööriista registreerimise metaandmete ekstraktib varade andmed ja andmed kopeeritakse Azure andmekataloogi teenusesse. Andmete jääb, kus see asub praegu ja see jääb jaotises juhtelemendi administraatorid ja -poliitikad praeguse süsteemi.

        ![Azure'i andmekataloogi--registreeritud objektid](media/data-catalog-get-started/data-catalog-registered-objects.png)

    9. Registreeritud andmeallikaobjektid kuvamiseks klõpsake käsku **Kuva portaali**. Azure'i andmekataloogi portaalis kinnitada kõik neli tabelid ja andmebaasi ruudustiku vaates kuvatakse.

        ![Objektide Azure'i andmekataloogi portaalis ](media/data-catalog-get-started/data-catalog-view-portal.png)


Selle toiminguga saate registreeritud Adventure Worksi näidisandmebaasi objektid nii, et neid saab hõlpsalt avastada kasutajatele kogu ettevõttes. Järgmise ülesande saate teada, kuidas leida registreeritud andmete varad.

## <a name="discover-data-assets"></a>Andmete varad avastamine
Azure'i andmekataloogis Discovery kasutab kaks esmane menetlustele: otsingu ja filtreerimise.

Otsingu eesmärk on võimas ja intuitiivne. Vaikimisi sobitatakse otsinguterminid vastu mis tahes atribuudi kataloogi, sh kasutaja esitatud marginaalid.

Filtreerimine on mõeldud täiendavad otsimine. Saate valida kindla omadused, nt asjatundjate, andmeallika tüüp, objekti tüüp ja siltide kuvamiseks vastavaid andmeid varad ja piirata otsingutulemite sobitamine varad.

Otsingu ja filtreerimise abil saate kiiresti liikuda andmeallikad, mis on registreeritud Azure'i andmekataloogi avastada, et peate andmed vara.

Selle toiminguga saate Azure'i andmekataloogi portaali avastamine andmete varad registreeritud eelmise ülesanne. [Andmekataloogi otsingu süntaksiviide](https://msdn.microsoft.com/library/azure/mt267594.aspx) kohta vaadake teavet süntaks otsing.

Järgnevalt mõned näited kataloogi varasid andmete otsimise.  

### <a name="discover-data-assets-with-basic-search"></a>Andmete vara lihtotsinguavaldis avastamine
Lihtotsinguavaldis abil saate otsida kataloogiga üks või mitu otsinguterminit abil. Tulemus on mis tahes atribuudi ühe või mitme määratud tingimustele vastavad varad.

1. Klõpsake menüüd **Avaleht** Azure'i andmekataloogi portaalis. Kui olete sulgenud veebibrauseri, minge [Azure'i andmekataloogi avalehele](https://www.azuredatacatalog.com).
2. Sisestage väljale Otsing `cycles` ja vajutage sisestusklahvi **ENTER**.

    ![Azure'i andmekataloogi--Põhitekst otsing](media/data-catalog-get-started/data-catalog-basic-text-search.png)
3. Veenduge, et näete kõigi nelja tabelite ja tulemuste andmebaasi (AdventureWorks2014). Saate vaheldumisi **koordinaatvõrk** ja **loendivaate** , klõpsates tööriistariba, nagu on näidatud järgmisel pildil. Pange tähele, otsingu märksõna esile otsingutulemustes, kuna valik **esiletõstmine** on **sisse LÜLITATUD**. Saate **lehel tulemite** arv otsingutulemites.

    ![Azure'i andmekataloogi--Põhitekst otsingutulemite](media/data-catalog-get-started/data-catalog-basic-text-search-results.png)

    **Otsingute** paneel on vasakul ja paani **Atribuudid** on õigus. **Otsingud** paneeli, saate muuta otsingukriteeriumid ja filtreerida tulemid. Paani **Atribuudid** kuvatakse valitud objekti atribuudid koordinaatvõrgule või loendis.

4. Klõpsake otsingutulemite **toodet** . Klõpsake nuppu **Eelvaade**, **veerud**, **Andmete profiili**ja **dokumentatsiooni** vahekaartide või klõpsake noolenuppu, et laiendada alumise paani.  

    ![Azure'i andmekataloogi--alumise paani](media/data-catalog-get-started/data-catalog-data-asset-preview.png)

    Klõpsake menüü **Eelvaade** kuvatakse tabelis **Product** andmete eelvaade.  
5. Klõpsake vahekaarti **veerud** leida andmete varade üksikasjad veergude (nt **nimi** ja **andmetüüp**).
6. Klõpsake vahekaarti **Andmed profiil** profiili andmete kuvamiseks (nt: arv ridu, suuruse või väikseima väärtuse veerus) andmete vara.
7. Tulemite filtreerimiseks klõpsake vasakus servas **filtrite** abil. Näiteks klõpsake **tabeli** **Objekti tüüp**ja näete ainult neli tabeleid, mitte andmebaas.

    ![Azure'i andmekataloogi--filtreerida Otsingu tulemused](media/data-catalog-get-started/data-catalog-filter-search-results.png)

### <a name="discover-data-assets-with-property-scoping"></a>Tutvumine andmete vara atribuut ulatuse määramine
Atribuut ulatuse määramine aitab leida andmete varad, kus Otsitav termin sobitada määratud atribuudi.

1. Klõpsake jaotises **filtrid** **Objekti tüüp** filtri **Tabel** .  
2. Sisestage väljale Otsing `tags:cycles` ja vajutage sisestusklahvi **ENTER**. Vaadake [andmekataloogi otsingu süntaksiviide](https://msdn.microsoft.com/library/azure/mt267594.aspx) kõik atribuudid, mida saate kasutada andmete kataloogi otsimiseks.
3. Veenduge, et näete kõigi nelja tabelite ja tulemuste andmebaasi (AdventureWorks2014).  

    ![Andmekataloogi--atribuudi otsingutulemite ulatuse määramine](media/data-catalog-get-started/data-catalog-property-scoping-results.png)

### <a name="save-the-search"></a>Otsingu salvestamine
1. **Otsingud** paanil jaotises **Praegune otsing** , sisestage Otsingu nimi ja klõpsake nuppu **Salvesta**.

    ![Azure'i andmekataloogi--Salvesta otsing](media/data-catalog-get-started/data-catalog-save-search.png)
2. Veenduge, et jaotises **Salvestatud otsingud**ilmub salvestatud otsingu.

    ![Azure'i andmekataloogi--salvestatud otsingud](media/data-catalog-get-started/data-catalog-saved-search.png)
3. Valige toimingud, mida saate salvestatud otsingu (**ümbernimetamine**, **kustutamine**, **Salvestage vaikeväärtuseks** otsing).

    ![Azure'i andmekataloogi--salvestatud otsingu suvandid](media/data-catalog-get-started/data-catalog-saved-search-options.png)

### <a name="boolean-operators"></a>Toetatud brauserid
Saate laiendada või otsingut toetatud brauserid.

1. Sisestage väljale Otsing `tags:cycles AND objectType:table`, ja vajutage sisestusklahvi **ENTER**.
2. Veenduge, et näete ainult tabelid (mitte andmebaas) tulemuste.  

    ![Azure'i andmekataloogi--kahendmuutujaga tehtemärk väljale Otsi](media/data-catalog-get-started/data-catalog-search-boolean-operator.png)

### <a name="grouping-with-parentheses"></a>Rühmitamise sulgudega
Rühmitamise sulgudega, mida saate rühmitada osad päringu saavutamiseks loogilise eraldi, eriti koos toetatud brauserid.

1. Sisestage väljale Otsing `name:product AND (tags:cycles AND objectType:table)` ja vajutage sisestusklahvi **ENTER**.
2. Veenduge, et kuvatakse ainult **toote** tabeli otsingutulemites.

    ![Azure'i andmekataloogi--rühmitamise otsing](media/data-catalog-get-started/data-catalog-grouping-search.png)   

### <a name="comparison-operators"></a>Võrdluse tehtemärgid
Võrdluse tehtemärgid, kus saate võrdlemine peale võrdse atribuudid, mis on andmetüüpide arv ja kuupäeva.

1. Sisestage väljale Otsing `lastRegisteredTime:>"06/09/2016"`.
2. Filtri **tabeli** loendist **Objekti tüüp**.
3. Vajutage **Sisestage**.
4. Veenduge, et **toode**, **ProductCategory**, **ProductDescription**ja **ProductPhoto** tabelid ja AdventureWorks2014 andmebaasi registreeritud Otsingu tulemused kuvatakse.

    ![Azure'i andmekataloogi--otsingutulemite võrdlus](media/data-catalog-get-started/data-catalog-comparison-operator-results.png)

Vaadake, [Kuidas leida andmete varad](data-catalog-how-to-discover.md) avastamine andmete varad ja [andmekataloogi otsingu süntaksiviide](https://msdn.microsoft.com/library/azure/mt267594.aspx) Otsi süntaks kohta lisateabe saamiseks.

## <a name="annotate-data-assets"></a>Marginaalide andmete varad
Järgnevas saate kasutada Azure andmekataloogi portaali marginaalide lisamiseks (lisada näiteks kirjelduse, siltide või eksperdid) andmete varad olete varem registreeritud kataloogi. Marginaalid täiendada ja täiustamiseks ekstraktimist andmeallika registreerimise käigus strukturaalset metaandmete ja teeb andmete varad märksa lihtsam üles leida ja mõista.

Selle toiminguga saate marginaale lisada ühe andmete vara (ProductPhoto). Saate lisada ProductPhoto andmete varade sõbralik nimi ja kirjeldus.  

1.  [Azure'i andmekataloogi avalehel](https://www.azuredatacatalog.com) ja otsida `tags:cycles` registreerumist vara andmete otsimiseks.  
2. Klõpsake otsingutulemite **ProductPhoto** .  
3. Sisestage **toote pildid** **Sõbralik nimi** ja **turundusmaterjalide fotosid toote** **Kirjeldus**.

    ![Azure'i andmekataloogi--ProductPhoto kirjeldus](media/data-catalog-get-started/data-catalog-productphoto-description.png)

    **Kirjeldus** aitab teistel leida ja aru, miks ja kuidas kasutada varade valitud andmed. Saate lisada rohkem silte ja veergude kuvamine. Nüüd saate proovida otsingu ja filtreerimise avastada andmete varad kirjeldav metaandmed, kui olete lisanud kataloogi abil.

Saate teha ka järgmist sellel lehel.

- Lisage andmete vara eksperdid. Klõpsake nuppu **Lisa** alal **eksperdid** .
- Andmekomplekti tasemel silte lisada. Klõpsake nuppu **Lisa** **Sildid** ala. Sildi saab kasutaja sildi või sildi sõnastik. Standardse andmekataloogi väljaanne sisaldab business sõnastik, mis aitab määratleda keskse business taksonoomia kataloogi administraatorid. Kataloogi kasutajad saavad marginaale seejärel andmete varad sõnastik tingimuste korral. Lisateabe saamiseks vaadake, [Kuidas häälestada Business sõnastik reguleeritakse sildistamine](data-catalog-how-to-business-glossary.md)
- Veeru tasemel silte lisada. Klõpsake nuppu **Lisa** jaotises **Sildid** veeru soovite märke.
- Lisage kirjeldus veeru tasemel. Sisestage **Kirjeldus** veeru. Saate vaadata ka ekstraktimist andmeallika metaandmete kirjeldus.
- Lisage **juurdepääsutaotluse** teavet, mis näitab kasutajate andmed varade juurdepääsu taotlemiseks tehke järgmist.

    ![Azure'i andmekataloogi--siltide kirjelduste lisamine](media/data-catalog-get-started/data-catalog-add-tags-experts-descriptions.png)

- Menüüd **dokumendid** ja sisestage andmed varade dokumentatsioonist. Azure'i andmekataloogi dokumentatsiooni kohta, kus saate oma andmekataloogi kasutada loomiseks valmis jutustus oma andmete varade sisu hoidla.

    ![Azure'i andmekataloogi--dokumentatsiooni menüü](media/data-catalog-get-started/data-catalog-documentation.png)


Samuti saate lisada marginaali mitme andmete varad. Näiteks saate valida kõik andmed registreeritud ja määrata ekspert neid.

![Azure'i andmekataloogi--marginaalide mitme andmete varad](media/data-catalog-get-started/data-catalog-multi-select-annotate.png)

Azure'i andmekataloogi toetab tõsta hankimine lähenemisviisi marginaalid. Mis tahes andmekataloogi kasutaja saab lisada silte (kasutaja või sõnastik), kirjelduse ja muud metaandmete nii, et iga kasutaja perspektiivi andmed vara ja selle kasutamise kohta on selle perspektiivi jäädvustatud ja teistele kasutajatele saadaval.

Vaadake, [kuidas andmeid varad marginaalide lisamiseks](data-catalog-how-to-annotate.md) üksikasjalikku teavet andmete varad marginaalid lisanud.

## <a name="connect-to-data-assets"></a>Ühenduse loomine andmete varad
Selle toiminguga avate andmete varasid on integreeritud kliendi tööriista (Excel) ja -integreeritud tööriist (SQL Server Management Studio) ühenduse teabe abil.

> [AZURE.NOTE] On oluline meeles pidada, et Azure'i andmekataloogi ei anna teile juurdepääsu tegelik andmeallika – see lihtsalt hõlbustab teil tuvastada ja mõista. Kui loote ühenduse andmeallikaga, valite klientrakendus kasutab teie Windowsi kasutajaandmeid või küsib teilt mandaate vastavalt vajadusele. Kui te pole varem antud andmeallikale juurdepääsu, peate enne ühenduse loomiseks saate anda juurdepääsu.

### <a name="connect-to-a-data-asset-from-excel"></a>Exceli andmete varade ühendamine

1. Valige otsingutulemitest **toode** . Klõpsake tööriistaribal nuppu **Ava:** ja klõpsake nuppu **Excel**.

    ![Azure'i andmekataloogi--varade andmete ühendamine](media/data-catalog-get-started/data-catalog-connect1.png)
2. Klõpsake nuppu **Ava** allalaadimine hüpikaknas. See kogemus võivad erineda sõltuvalt brauseris.

    ![Azure'i andmekataloogi--Exceli ühenduse faili alla laadida](media/data-catalog-get-started/data-catalog-download-open.png)
3. **Microsoft Excel turbeteatis** aknas, klõpsake nuppu **Luba**.

    ![Azure'i andmekataloogi--Exceli turvalisus hüpik](media/data-catalog-get-started/data-catalog-excel-security-popup.png)
4. Säilitamine dialoogiboksis **Andmete importimine** vaikeväärtused ja klõpsake nuppu **OK**.

    ![Azure'i andmekataloogi – Exceli andmete importimine](media/data-catalog-get-started/data-catalog-excel-import-data.png)
5. Saate vaadata andmeallika Excelis.

    ![Azure'i andmekataloogi--Exceli tabel product](media/data-catalog-get-started/data-catalog-connect2.png)

Selle toiminguga ühendatud andmete vara avastanud Azure'i andmekataloogi abil. Azure'i andmekataloogi portaalis saate ühendada otse abil klientrakendustes, integreeritud **Ava** menüü. Samuti saate ühendada mistahes rakendusega, valite kaasatud metaandmete vara asukoht ühendusteabe abil. Näiteks saate SQL Server Management Studio andmete vara selles õpetuses registreeritud andmetele juurdepääsuks AdventureWorks2014 andmebaasiga ühenduse loomiseks.

1. Avage **SQL Server Management Studio**.
2. Sisestage dialoogiboksis **ühenduse Server** Azure'i andmekataloogi portaalis paanil **Atribuudid** serveri nime.
3. Juurdepääs andmete varade sobiv autentimise ja mandaadi abil. Kui teil pole juurdepääsu, kasutage **Juurdepääsutaotluse** väli teabe hankimine.

    ![Azure'i andmekataloogi--juurdepääsutaotluse](media/data-catalog-get-started/data-catalog-request-access.png)

Klõpsake **Vaate ühendusstringi** vaadata ja kasutada rakenduse ADF.NET, ODBC ja OLEDB ühendusstringi lõikelauale.

## <a name="manage-data-assets"></a>Andmete varade haldamine
Selles etapis tuleb näete, kuidas häälestada oma andmete varasid Turve. Andmekataloogi pole kasutajatele juurdepääsu andmine andmed ise. Andmeallika omanik juhtelemendid juurdepääs andmetele.

Saate andmekataloogi andmeallikate ja vaadata registreeritud kataloogi allikad seotud metaandmed. Võib tekkida olukordi, kus andmeallikaid tuleks nähtavad ainult kindlatele kasutajatele või rühmadele liikmetele. Stsenaariumi puhul, andmekataloogi abil saate omandiõigust registreeritud andmete varasid kataloogi ja seejärel juhtelemendile nähtavuse vara kuulub teile.

> [AZURE.NOTE] Selle ülesande kirjeldatud halduse võimaluste on saadaval ainult Azure andmete kataloogi Standard Edition, mitte tasuta Edition.
Azure'i andmekataloogi saate omanikuõiguse andmete varad, lisada kaasomanike andmete varad ja andmete varade nähtavuse seadmine.

### <a name="take-ownership-of-data-assets-and-restrict-visibility"></a>Omanikuõiguse andmete varad ja piiratud nähtavus

1. [Azure'i andmekataloogi avalehele](https://www.azuredatacatalog.com)minek Sisestage väljale **Otsing** tekst `tags:cycles` ja vajutage sisestusklahvi **ENTER**.
2. Klõpsake tulemiloendis üksust ja klõpsake tööriistaribal nuppu **Võtta omandiõiguse** .
3. Klõpsake paani **Atribuudid** jaotises **haldus** **Võtta omandiõiguse**.

    ![Azure'i andmekataloogi--tee omaniku](media/data-catalog-get-started/data-catalog-take-ownership.png)
4. Nähtavuse piiramiseks valige **omanikud ja need kasutajad** , klõpsake jaotises **nähtavus** ja klõpsake nuppu **Lisa**. Sisestage kasutaja meiliaadressid väljale tekst ja vajutage sisestusklahvi **ENTER**.

    ![Azure'i andmekataloogi--juurdepääsu piiramise](media/data-catalog-get-started/data-catalog-ownership.png)

## <a name="remove-data-assets"></a>Andmete varad eemaldamine

Selle ülesande kasutage Azure'i andmekataloogi portaali andmete eelvaade eemaldamine registreeritud andmete varad ja kustutada andmete varad kataloogist.

Azure'i andmekataloogi, saate kustutada üksiku vara või varasid kustutada.

1. [Azure'i andmekataloogi avalehele](https://www.azuredatacatalog.com)minek
2. Sisestage väljale **Otsing** tekst `tags:cycles` ja klõpsake nuppu **Sisesta**.
3. Valige üksus tulemiloendis ja **kustutamiseks** klõpsake tööriistaribal nuppu nagu on näidatud järgmisel pildil.

    ![Azure'i andmekataloogi--Kustuta koordinaatvõrgu üksus](media/data-catalog-get-started/data-catalog-delete-grid-item.png)

    Kui kasutate loendivaate, märkeruut on vasakul üksust, nagu on näidatud järgmisel pildil:

    ![Azure'i andmekataloogi--loendiüksuse kustutamine](media/data-catalog-get-started/data-catalog-delete-list-item.png)

    Saate valida mitme andmete varad ja kustutamiseks, nagu on näidatud järgmisel pildil.

    ![Azure'i andmekataloogi--mitme andmete varad kustutamine](media/data-catalog-get-started/data-catalog-delete-assets.png)


> [AZURE.NOTE] Kataloogi vaikekäitumine on kõik kasutajad, mis tahes andmeallika registreerimiseks ja kõik kasutajad kustutada kõik andmed vara, mis on registreeritud. Azure'i andmete kataloogi Standard väljaanne sisaldab halduse võimaluste pakuvad lisasuvandid varade, piiramine kes varad, võite avastada ja piirata, kes saavad kustutada varad.


## <a name="summary"></a>Kokkuvõte

Selles õpetuses, saate uurida oluline võimaluste Azure'i andmekataloogi, sh registreerimine, marginaalid lisanud, avastamine ja hallata ettevõtte andmeid vara. Nüüd, kui olete õpetusega, on aeg alustada. Saate alustada juba täna registreerimisel sõltuvad teie ja teie meeskond andmeallikad ja kutsudes kolleegide kasutada kataloogi.

## <a name="references"></a>Viited

- [Kuidas registreeruda andmete varad](data-catalog-how-to-register.md)
- [Kuidas leida andmete varad](data-catalog-how-to-discover.md)
- [Kuidas marginaalide andmete varad](data-catalog-how-to-annotate.md)
- [Kuidas dokumendi andmete varad](data-catalog-how-to-documentation.md)
- [Kuidas luua ühendus andmete varad](data-catalog-how-to-connect.md)
- [Kuidas hallata andmeid varad](data-catalog-how-to-manage.md)
