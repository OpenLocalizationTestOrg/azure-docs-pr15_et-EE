<properties 
    pageTitle="Azure'i Redis vahemälu andmete importimine ja eksportimine | Microsoft Azure'i" 
    description="Saate teada, kuidas importida ja eksportida andmeid ja sealt koos premium Azure'i Redis vahemälu eksemplaride bloobimälu" 
    services="redis-cache" 
    documentationCenter="" 
    authors="steved0x" 
    manager="douge" 
    editor=""/>

<tags 
    ms.service="cache" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="cache-redis" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/15/2016" 
    ms.author="sdanie"/>

# <a name="import-and-export-data-in-azure-redis-cache"></a>Azure'i Redis vahemälu andmete importimine ja eksportimine

Import/eksport on Azure Redis vahemälu andmete halduse toiming, mis võimaldab teil Azure'i Redis vahemälu andmete importimine või andmete eksportimine Azure'i Redis vahemälu importimise ja eksportimise hetktõmmise Redis vahemälu andmebaasi (RDB) premium vahemälu lehe bloobimälu Azure'i salvestusruumi kontol. Import/eksport võimaldab migreerida erinevate Azure'i Redis vahemälu eksemplari vahel või vahemälu enne kasutamist andmetega asustada.

Selles artiklis juhendi abil Azure'i Redis vahemälu andmete importimisse ja eksportimisse ja leiate vastused korduma kippuvatele küsimustele.

>[AZURE.IMPORTANT] Import/eksport on eelvaade ja on saadaval ainult [premium taseme](cache-premium-tier-intro.md) vahemälu jaoks.

## <a name="import"></a>Importimine

Impordi saab tuua Redis ühilduvad RDB failid pilve või keskkonnas, sh Linux, Windows või mis tahes pilveteenuse pakkuja nagu Amazon veebiteenuste ja teised Redis ühegi Redis serveriga. Andmete importimine on lihtne viis vahemälu luua juba eelnevalt täidetud andmetega. Importimise käigus Azure'i Redis vahemälu Azure storage RDB failid laaditakse memory ja lisab seejärel klahve vahemälu.

>[AZURE.NOTE] Enne imporditoimingu, tagada teie Redis andmebaasi (RDB) faili või failide üles lehe plekid Azure storage, sama piirkonna ja Azure Redis vahemälu eksemplari oma tellimuse sisse. Lisateavet leiate teemast [Alustamine Azure'i bloobimälu](../storage/storage-dotnet-how-to-use-blobs.md). [Azure'i Redis vahemälu ekspordi](#export) funktsiooni abil RDB faili eksportimisel RDB fail on juba salvestatud lehe bloobimälu ja on importimiseks valmis.

1. Importimiseks üks või mitu eksporditud vahemälu plekid, [liikuge sirvides vahemälu](cache-configure.md#configure-redis-cache-settings) Azure portaali ja klõpsake nuppu **impordi andmed** oma vahemälu eksemplari keelest **sätted** .

    ![Andmete importimine][cache-import-data]

2. Klõpsake **Nuppu Blob(s)** ja valige salvestusruumi konto, mis sisaldab imporditavaid andmeid.

    ![Salvestusruumi konto valimine][cache-import-choose-storage-account]

3. Klõpsake ümbris, mis sisaldab imporditavaid andmeid.

    ![Valige container][cache-import-choose-container]

4. Valige üks või mitu plekid importida, klõpsates ala vasakul bloobimälu nimi ja klõpsake **Valige**.

    ![Valige plekid][cache-import-choose-blobs]

5. Klõpsake nuppu impordi alustamiseks **importida** .

    >[AZURE.IMPORTANT] Vahemälu pole kättesaadav vahemälu kliendid importimise käigus ja vahemälu olemasolevad andmed on kustutatud.

    ![Importimine][cache-import-blobs]

    Pärast teatiste Azure portaali kaudu või vaatate sündmuste [Logi auditilogi](cache-configure.md#support-amp-troubleshooting-settings)saate imporditoimingu edenemist jälgida.

    ![Impordi edenemine][cache-import-data-import-complete] 


## <a name="export"></a>Eksportimine

Ekspordi saate eksportida andmed salvestatakse Azure Redis vahemälu Redis RDB ühilduvad failid. Selle funktsiooni abil saate ühel Azure'i Redis vahemälu andmete teisaldamiseks või mõne muu Redis serveriga. Eksportimise käigus ajutine fail on loodud VM Azure Redis vahemälu serveri eksemplari hostiva ja faili üleslaadimisel kontole määratud salvestusruumi. Eksporditoimingu edu olek või tõrge on lõpule jõudnud, kustutatakse ajutine fail.

1. Praeguse salvestusruumi vahemälu, [liikuge sirvides vahemälu](cache-configure.md#configure-redis-cache-settings) Azure portaali sisu eksportimine ja klõpsake nuppu **Ekspordi andmed** oma vahemälu eksemplari keelest **sätted** .

    ![Valige salvestusruumi container][cache-export-data-choose-storage-container]

2. Klõpsake **Nuppu talletusmahu ümbris** ja valige soovitud salvestusruumi konto. Salvestusruumi konto peab olema sama tellimuse ja regiooni vahemälu.

    >[AZURE.IMPORTANT] Import/eksport lehe plekid, mis toetab nii klassikaline ja ARM salvestusruumi kontod, kuid ei toeta [Bloobivahemälu salvestusruumi kontod](../storage/storage-blob-storage-tiers.md#blob-storage-accounts) praegu töötab.

    ![Salvestusruumi konto][cache-export-data-choose-account]

3. Valige soovitud bloobimälu ümbris ja klõpsake nuppu **Vali**. Uus ümbris kasutamiseks klõpsake **Container lisage** see esmalt lisada, ja valige see loendist.

    ![Valige salvestusruumi container][cache-export-data-container]

4. Tippige **bloobimälu nime eesliide** ja klõpsake nuppu **ekspordi** alustamiseks eksportimise käigus. Bloobimälu nime eesliide kasutatakse eesliide selle eksporditoimingu loodud failide nimed.

    ![Eksportimine][cache-export-data]

    Pärast teatiste Azure portaali kaudu või vaatate sündmuste [Logi auditilogi](cache-configure.md#support-amp-troubleshooting-settings)saate eksporditoimingu edenemist jälgida.

    ![][cache-export-data-export-complete]

    Vahemälu jäävad eksportimise käigus kasutamiseks saadaval.


## <a name="importexport-faq"></a>Import/eksport KKK

See jaotis sisaldab korduma kippuvaid küsimusi funktsiooni impordi/ekspordi.

-   [Millised hinnad astme saate kasutada impordi/ekspordi?](#what-pricing-tiers-can-use-importexport)
-   [Saate importida andmeid mis tahes Redis server?](#can-i-import-data-from-any-redis-server)
-   [Kas minu vahemälu on saadaval impordi-või eksporditoimingu käigus?](#will-my-cache-be-available-during-an-importexport-operation)
-   [Saate kasutada impordi/ekspordi Redis kobar?](#can-i-use-importexport-with-redis-cluster)
-   [Kuidas impordi/ekspordi säte kohandatud andmebaasidega toimib?](#how-does-importexport-work-with-a-custom-databases-setting)
-   [Kuidas erineb impordi/ekspordi Redis püsimine?](#how-is-importexport-different-from-redis-persistence)
-   [Kas impordi/ekspordi PowerShelli, CLI või teiste klientide haldamine saate automaatseks muuta?](#can-i-automate-importexport-using-powershell-cli-or-other-management-clients)
-   [Sain oma impordi-ja eksporditoimingu käigus ajalõpp tõrge. Mida see tähendab?](#i-received-a-timeout-error-during-my-importexport-operation.-what-does-it-mean)
-   [Sain viga Azure'i bloobimälu oma andmete eksportimisel. Mis juhtus?](#i-got-an-error-when-exporting-my-data-to-azure-blob-storage.-what-happened)


### <a name="what-pricing-tiers-can-use-importexport"></a>Millised hinnad astme saate kasutada impordi/ekspordi?

Import/eksport on saadaval ainult lisatasu hinnad taseme.

### <a name="can-i-import-data-from-any-redis-server"></a>Saate importida andmeid mis tahes Redis server?

Jah, lisaks Azure Redis vahemälu eksemplarid eksporditud andmete importimise saate importida RDB faile, mis tahes Redis serverist pilveteenuses või keskkonnas, nt Linux, kus töötab Windows, või cloud pakkujad nagu Amazon Web Services. Selleks soovitud Redis serverist RDB faili üleslaadimise lehe bloobimälu Azure'i salvestusruumi kontol sisse ja seejärel importige need oma premium Azure'i Redis vahemälu eksemplari. Näiteks võite tootmise vahemälu andmete eksportimine ja importimine katsetamiseks või migreerimise osana lavastus keskkonnas kasutatavate vahemälu. 

### <a name="will-my-cache-be-available-during-an-importexport-operation"></a>Kas minu vahemälu on saadaval impordi-või eksporditoimingu käigus?

-   **Ekspordi** - vahemälu endiselt saadaval ja te saate jätkata vahemälu eksporditoimingu käigus.
-   **Importimine** - vahemälu muutuvad kättesaamatuks imporditoimingu käivitamisel ja muutuvad kättesaadavaks, kui imporditoimingu lõpule jõudnud.


### <a name="can-i-use-importexport-with-redis-cluster"></a>Saate kasutada impordi/ekspordi Redis kobar?

Jah, ja te saate import/eksport rühmitatud vahemälu ja -rühmitatud vahemälu vahel. Kuna Redis kobar [toetab ainult andmebaasi 0](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering), andmebaasides pole 0 andmeid ei impordita. Rühmitatud vahemälu andmete importimisel võtmed klaster shards seas edasi. 

### <a name="how-does-importexport-work-with-a-custom-databases-setting"></a>Kuidas impordi/ekspordi säte kohandatud andmebaasidega toimib?

Mõned hinnakirjad astme on erinevate [andmebaaside piirangud](cache-configure.md#databases), nii on mõned kaalutlused importimisel, kui olete konfigureerinud jaoks kohandatud väärtus on `databases` säte vahemälu loomise ajal.

-   Hinnakirjad taseme madalama importimisel `databases` taseme, millest eksporditud kui:
    -   Kui kasutate vaikearvu `databases` pole andmeid, mis on kõik hinnakirjad astme 16, läheb kaotsi.
    -   Kui kasutate kohandatud arvu `databases` piires, mis jääb kiht, mis on importida, ei ole andmed lähevad kaotsi.
    -   Kui andmed andmebaasis, mis ületab uue tasandi eksporditud andmete, need suurema andmebaasid andmeid ei impordita.

### <a name="how-is-importexport-different-from-redis-persistence"></a>Kuidas erineb impordi/ekspordi Redis püsimine?

Azure'i Redis vahemälu püsimine võimaldab Redis Azure'i salvestusruumi talletatavad andmed säilivad. Püsimine konfigureerimisel Azure'i Redis vahemälu püsib hetktõmmise Redis vahemälu Redis kahendvormingus kettale konfigureeritav varukoopia sageduse alusel. Kui esineb katastroofiline sündmus, mis keelab nii põhi-kui ka koopia vahemälu, vahemälu andmed taastatakse automaatselt, kasutades viimase hetktõmmis. Lisateabe saamiseks vaadake, [Kuidas konfigureerida andmete püsimine Premium Azure'i Redis vahemälu](cache-how-to-premium-persistence.md).

Import / eksport võimaldab teil andmeid tuua või eksportimine Azure'i Redis vahemälu. See konfigureerimine varundamine ja taastamine Redis püsimine abil.


### <a name="can-i-automate-importexport-using-powershell-cli-or-other-management-clients"></a>Kas impordi/ekspordi PowerShelli, CLI või teiste klientide haldamine saate automaatseks muuta?

Jah, PowerShelli juhiseid teemast [Redis vahemälu importimiseks](cache-howto-manage-redis-cache-powershell.md#to-import-a-redis-cache) ja [eksportimiseks Redis vahemälu](cache-howto-manage-redis-cache-powershell.md#to-export-a-redis-cache).



### <a name="i-received-a-timeout-error-during-my-importexport-operation-what-does-it-mean"></a>Sain oma impordi-ja eksporditoimingu käigus ajalõpp tõrge. Mida see tähendab?

Kui teil jäävad rohkem kui 15 minutit enne selle toimingu enne **andmete importimine** või **eksportimine andmeid** , kuvatakse järgmine tõrge.

    The request to import data into cache 'contoso55' failed with status 'error' and error 'One of the SAS URIs provided could not be used for the following reason: The SAS token end time (se) must be at least 1 hour from now and the start time (st), if given, must be at least 15 minutes in the past.

Probleemi lahendamiseks, algatada importimine või eksportimine 15 minutit enne möödunud.

### <a name="i-got-an-error-when-exporting-my-data-to-azure-blob-storage-what-happened"></a>Sain viga Azure'i bloobimälu oma andmete eksportimisel. Mis juhtus?

Import/eksport töötab ainult RDB failid talletatakse lehe plekid. Muud tüüpi bloobimälu ei toetata sel ajal, sh bloobimälu salvestusruumi kontod kuum ja lahedad astme.


## <a name="next-steps"></a>Järgmised sammud
Saate teada, kuidas kasutada rohkem vahemälu lisafunktsioonidele.

-   [Azure'i Redis vahemälu Premium taseme tutvustus](cache-premium-tier-intro.md)    

  
<!-- IMAGES -->
[cache-settings-import-export-menu]: ./media/cache-how-to-import-export-data/cache-settings-import-export-menu.png
[cache-export-data-choose-account]: ./media/cache-how-to-import-export-data/cache-export-data-choose-account.png
[cache-export-data-choose-storage-container]: ./media/cache-how-to-import-export-data/cache-export-data-choose-storage-container.png
[cache-export-data-container]: ./media/cache-how-to-import-export-data/cache-export-data-container.png
[cache-export-data-export-complete]: ./media/cache-how-to-import-export-data/cache-export-data-export-complete.png
[cache-export-data]: ./media/cache-how-to-import-export-data/cache-export-data.png
[cache-import-data]: ./media/cache-how-to-import-export-data/cache-import-data.png
[cache-import-choose-storage-account]: ./media/cache-how-to-import-export-data/cache-import-choose-storage-account.png
[cache-import-choose-container]: ./media/cache-how-to-import-export-data/cache-import-choose-container.png
[cache-import-choose-blobs]: ./media/cache-how-to-import-export-data/cache-import-choose-blobs.png
[cache-import-blobs]: ./media/cache-how-to-import-export-data/cache-import-blobs.png
[cache-import-data-import-complete]: ./media/cache-how-to-import-export-data/cache-import-data-import-complete.png








