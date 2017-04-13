<properties
    pageTitle="Andmete Factory Kopeeri viisardi | Microsoft Azure'i"
    description="Lisateavet andmete Factory koopia viisardi abil saate andmeid kopeerida valamud toetatud andmeallikad."
    services="data-factory"
    documentationCenter=""
    authors="spelluru"
    manager="jhubbard"
    editor="monicar"/>

<tags
    ms.service="data-factory"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="spelluru"/>

# <a name="data-factory-copy-wizard"></a>Andmete Factory kopeerimise viisard
Azure'i andmed Factory Kopeeri viisard on söömisega andmeid, mis on tavaliselt esimene samm-lõpuni andmete integreerimise stsenaariumis lihtsustamiseks. Azure'i andmed Factory Kopeeri viisardiga minnes peate aru, mis tahes JSON määratlused lingitud teenused, andmekomplektide ja torujuhtmete. Juhul, kui olete kõik viisardi etapid, viisard loob automaatselt on valitud andmeallika andmete kopeerimine valitud kohaletoimetamisel. Lisaks koopia viisardi abil saate Valideeri andmed on märkimisväärselt ajal loome, mis salvestab palju aega, eriti kui teil on söömisega andmete esimest korda andmeallikast. Kopeeri viisardi käivitamiseks klõpsake oma andmete factory avalehel **andmete kopeerimine** paani.

![Kopeerige viisard](./media/data-factory-copy-wizard/copy-data-wizard.png)


## <a name="an-intuitive-wizard-for-copying-data"></a>Andmete kopeerimine on intuitiivne viisard
Viisardi abil saab hõlpsasti teisaldada andmeid paljudest erinevatest andmeallikatest sihtkohta minutites. Pärast läheb viisardiga, müügivõimaluste Kopeeri tegevus on teie jaoks automaatselt loodud koos sõltuvad andmete Factory üksuste (lingitud teenuste ja andmekomplektide). Loomiseks tulemas on vaja mingeid täiendavaid toiminguid.   

![Andmeallika valimine](./media/data-factory-copy-wizard/select-data-source-page.png)

> [AZURE.NOTE] Vt [Kopeeri viisardi õpetuse](data-factory-copy-data-wizard-tutorial.md) artikkel üksikasjalike juhiste loomiseks valimi kopeerida andmed on Azure Bloobivahemälu Azure'i SQL-andmebaasi tabelisse. 

Viisardi eesmärk on suur andmetega meeles algusest. See on lihtne ja tõhusa Autor andmete Factory torustikud teisaldamine sadu kaustade, failide või tabelite andmete kopeerimine viisardi abil. Viisardi toetab kolme järgmised funktsioonid: automaatne andmete eelvaade, skeemi jäädvustada ja vastendamine ja andmete filtreerimine. 

## <a name="automatic-data-preview"></a>Automaatse andmete eelvaade 
Kopeeri viisardi võimaldab teil läbi vaadata osa andmeid valitud andmeallika saate kontrollida, kas andmed on õiged andmed, mida soovite kopeerida. Lisaks, kui lähteandmete tekstifaili, Kopeeri viisardi sõelub teksti faili teavet rea ja veeru eraldajaid ja skeemi automaatselt. 

![Faili vormingusätted](./media/data-factory-copy-wizard/file-format-settings.png)

## <a name="schema-capture-and-mapping"></a>Skeemi jäädvustada ja kaardistamine 
Sisendandmete skeemiga võib-olla ei vasta skeemile väljundi andmete mõnel juhul. Selle stsenaariumi korral peate Vastendage veerud allika skeemist veergudesse sihtkoha skeemist. 

Kopeeri viisard automaatselt kaardid veergude allika skeemiga sihtkoha skeemi veerud. Saate vastendused alistada, kasutades funktsiooni ripploendite (või) saate määrata, kas veeru peab andmete kopeerimisel vahele jätta.   

![Skeemi vastendamine](./media/data-factory-copy-wizard/schema-mapping.png)

## <a name="filtering-data"></a>Andmete filtreerimine  
Viisardi abil saate filtreerimiseks valige andmed, mis tuleb kopeerida sihtkoha/valamu andmesalve lähteandmetega. Filtreerimine vähendab andmed kopeerida valamu andmesalve mahtu ja seetõttu täiustab kopeerimine jõudlus. Andmete filtreerimine relatsiooniandmebaasist paindlikult pakub abil SQL-i päring keele (või) failide kaustas Azure'i bloobimälu [andmete Factory funktsioonid ja muutujate](data-factory-functions-variables.md)abil.   

### <a name="filtering-of-data-in-a-database"></a>Andmebaasi andmete filtreerimine  
Näites kasutatakse SQL-päringu soovitud `Text.Format` funktsioon ja `WindowStart` muutujana. 

![Kinnitage avaldised](./media/data-factory-copy-wizard/validate-expressions.png)

### <a name="filtering-of-data-in-an-azure-blob-folder"></a>Azure'i bloobimälu kaustas andmete filtreerimine
Kausta tee muutujate abil saate andmete kopeerimine kausta, mis määratakse käitusajal [süsteemi muutujate](data-factory-functions-variables.md#data-factory-system-variables)põhjal. Muutujad toetatud: **{aasta}**, **{kuu}**, **{päeva}**, **{tund}**, **{minutid}**ja **{kohandatud}**. Näide: inputfolder / {aasta} / {kuu} / {päev}.

Oletame, et te sisestate kaustade järgmises vormingus:

    2016/03/01/01
    2016/03/01/02
    2016/03/01/03
    ...

**Faili**või kausta klõpsake nuppu **Sirvi** , otsige üks neid kaustu (nt 2016 -> 03 -> 01 -> 02), ja klõpsake käsku **Vali**. Peaksite nägema `2016/03/01/02` tekstiväljale. Nüüd, Asendage **2016** **03** kuuga **{}**, **{**päev} **01** ja **02** **{**tunnist}, **{aasta}**, ja vajutage klahvi Tab. Peaksite nägema ripploendite valida nende nelja muutujate vorming:

![Süsteemi muutujate abil](./media/data-factory-copy-wizard/blob-standard-variables-in-folder-path.png)   

Nagu on näidatud järgmine pilt, saate kasutada **kohandatud** muutuja ja mis tahes [toetatud vormingustringi](https://msdn.microsoft.com/library/8kb3ddd4.aspx). Valige kaust, mille selle struktuuri, kasutage esmalt nuppu **Sirvi** . Seejärel Asendage väärtus **{kohandatud}**ja vajutage vahekaarti tekstiväli, kuhu saate tippida vormingustring vaatamiseks.     

![Kohandatud muutuja abil](./media/data-factory-copy-wizard/blob-custom-variables-in-folder-path.png)


## <a name="support-for-diverse-data-and-object-types"></a>Tugi mitmesuguse andmed ja objekti tüübid
Kopeeri viisardi abil saate teisaldada tõhus sadu kaustade, failide või tabelid.

![Valige tabelid, millest soovite andmed kopeerida](./media/data-factory-copy-wizard/select-tables-to-copy-data.png)

## <a name="scheduling-options"></a>Plaanimine suvandid
Käivitage kopeerimine üks kord või ajakava (tunni, iga päev, jne). Nende suvandite saab konnektorid laiust üle kohapealse ja pilveteenuse töölaua kohaliku eksemplari.

Ühekordse koopia toiming võimaldab andmete liikumine mõnest muust allikast sihtkohta ainult üks kord. See kehtib andmete mis tahes suurusega ja mis tahes toetatud vormingus. Ajastatud kopeerimine võimaldab kopeerida andmeid ettenähtud Korduvus. Saate konfigureerida Ajastatud kopeerimine rikkaliku sätted (nt proovi uuesti, ajalõpp ja teatised).

![Plaanimine atribuudid](./media/data-factory-copy-wizard/scheduling-properties.png)


## <a name="next-steps"></a>Järgmised sammud
Kiirülevaate ülevaadet loomiseks Kopeeri tegevuse abil andmete Factory koopia viisardi abil, vt [õpetus: müügivõimaluste koopia viisardi abil luua](data-factory-copy-data-wizard-tutorial.md).
