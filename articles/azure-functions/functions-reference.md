<properties
    pageTitle="Azure'i funktsioonide tootearendusmaterjal | Microsoft Azure'i"
    description="Azure'i funktsioonide põhimõtet ja komponendid, mis on kõigi keelte ja sidumiste mõista."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure'i funktsioone, funktsioonide, event töötlus, webhooks, dünaamiline Arvuta, serverless arhitektuur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="05/13/2016"
    ms.author="chrande"/>

# <a name="azure-functions-developer-reference"></a>Azure'i funktsioonide tootearendusmaterjal

Azure'i funktsioonide mõne tehnilise keskendudes ja komponendid, olenemata keele või kasutate sidumine ühiskasutusse andmine. Enne kui hüpata Õppekeskuse teatud keele või sidumine üksikasjad, kindlasti lugege läbi see ülevaade, mis kehtib neile kõigile.

Selles artiklis eeldatakse, et te loetud [Azure'i funktsioonide tutvustus](functions-overview.md) ja tuttavaid [WebJobs SDK põhimõtet nagu päästikute, sidumiste, ja JobHost runtime](../app-service-web/websites-dotnet-webjobs-sdk.md). Azure'i funktsioonide põhineb WebJobs SDK. 


## <a name="function-code"></a>Funktsioon code

*Funktsioon* on esmane mõiste Azure'i funktsioonides. Teie valitud keeles funktsiooni koodi kirjutamine ja koodi failid ja konfigureerimise faili salvestamine samas kaustas. Konfigureerimine on JSON ja faili nimi `function.json`. Erinevate keelte on toetatud ja igale kirjele on veidi teistsugused kogemused töötavad kõige paremini selle keele jaoks optimeeritud. 

Funktsiooni `function.json` fail sisaldab teatud funktsioonile, sh selle sidumiste konfigureerimine. Runtime loeb selle faili määratlemiseks sündmuste käivitamiseks välja, milliseid andmeid helistamisel funktsiooni ja kuhu saata andmed sisaldavad möödunud mööda funktsioon ise. 

```json
{
    "disabled":false,
    "bindings":[
        // ... bindings here
        {
            "type": "bindingType",
            "direction": "in",
            "name": "myParamName",
            // ... more depending on binding
        }
    ]
}
```

Saate takistada runtime töötab funktsiooni, seades selle `disabled` atribuut `true`.

Funktsiooni `bindings` atribuut on konfigureerite sidumiste nii päästikute. Iga sidumine jagab mõned levinud sätted ja teatud sätted, mis on teatud kindlat tüüpi sidumine. Iga sidumine jaoks on vaja järgmisi sätteid:

|Atribuut|Väärtuste/tüübid|Kommentaarid|
|---|-----|------|
|`type`|string|Köitmine tüüp. Näiteks `queueTrigger`.
|`direction`|"", "läbi"| Näitab, kas sidumine on andmete vastuvõtmine funktsiooni või funktsiooni andmete saatmine.
| `name` | string | Nimi, mida kasutatakse funktsiooni seotud andmed. C# see saab argumendi nime; JavaScript oleks Sisestage loendi /-väärtuse.

## <a name="function-app"></a>Funktsioon rakendus

Funktsioon rakenduse hulka kuuluvad ühe või mitme üksikud funktsioonid, mis haldab koos Azure'i rakendust Service. Kõik funktsioonid rakenduse funktsioon hinnakirjad sama lepingu, pidev juurutamise ja käitusaja versioon ühiskasutus. Mitmes keeles kirjutatud funktsioonid võivad kõik kasutada sama funktsioon rakendus. Funktsioon rakenduse mõelda nii, et korraldada ja ühiselt oma funktsioonide haldamine. 

## <a name="runtime-script-host-and-web-host"></a>Käitusaja (skriptihosti ja veebi)

Käitusaja või skriptihosti, on aluseks WebJobs SDK host, mis kuulab sündmused, kogub ja saadab andmed ja lõpuks töötab teie koodi. 

HTTP päästikute hõlbustamiseks on ka veebi, mis on loodud istuda ees skriptihosti peamised sisse. See aitab eristamiseks script host esiosa liiklus hallatavate veebi.

## <a name="folder-structure"></a>Kausta struktuuri

[AZURE.INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

Kui loomine teenuses Azure rakenduse funktsioon rakendusse funktsioonide kasutamise kohta projekti, saate selle kausta struktuuri käsitleda oma saidi kood. Saate kasutada olemasolevaid tööriistu, nagu pidev integratsioon ja juurutamine või kohandatud juurutamise skriptid selleks juurutamine aja paketi installimine või tõrkekood transpilation.

>[AZURE.NOTE] Funktsiooni `wwwroot` on siin kausta, kus failide ei saada juurutatud. Siiski saate ei tohi sisaldada sellesse kausta faile juurutamist, mis peaks koos `wwwroot\wwwroot`. Selle asemel oma `host.json` faili ja funktsioon kaustade peaks olema otse juurtasemel mis juurutamist.

## <a id="fileupdate"></a>Funktsioon rakenduse failide värskendamine

Funktsioon redaktori Azure portaali sisse ehitatud saate värskendada *function.json* faili- ja koodi faili funktsiooni. Üles-või muid faile, nt *package.json* või *project.json* või sõltuvused värskendada, tuleb kasutada muid viise juurutamise.

Funktsioon rakendused on loodud teenuse rakendus, et [Juurutussuvandid standardse veebi rakendustele](../app-service-web/web-sites-deploy.md) on saadaval ka funktsioon rakendused. Siin on mõned meetodid, saate üles laadida või värskendada funktsioon rakenduse failid. 

#### <a name="to-use-app-service-editor"></a>Rakenduse teenuse redaktori kasutamiseks

1. Azure'i funktsioonide portaalis nuppu **funktsioon rakenduse sätted**.

2. Klõpsake jaotises **Täpsemad sätted** **minge Teenusesätted rakendus**.

3. Klõpsake jaotises **arengu tööriistad**nuppu rakenduse menüüd jaotist **Rakenduse teenuse redaktor** .

4.  Klõpsake nuppu **Mine**.

    Pärast rakenduse teenuse redaktor laadib, kuvatakse teile *wwwroot* *host.json* faili ja funktsioon kaustad. 

5. Avage faile redigeerida, või pukseerige faile üles laadida arengu arvutist.

#### <a name="to-use-the-function-apps-scm-kudu-endpoint"></a>Funktsioon rakenduse SCM (Kudu) lõpp-punkti kasutamine

1. Liikuge: `https://<function_app_name>.scm.azurewebsites.net`.

2. Klõpsake **konsooli silumine > CMD**.

3. Liikuge `D:\home\site\wwwroot\` *host.json* värskendamiseks või `D:\home\site\wwwroot\<function_name>` on funktsioon failide värskendamiseks.

4. Hiirega vastav kausta faili ruudustiku üleslaaditava faili. Kus saab pukseerida faili faili ruudustiku on kaks alad. *ZIP* -failid kuvatakse kast sildiga "Üles laadida ja pakkige see lahti, lohistage siia." Muude failitüüpide kukutage ruudustiku faili, kuid väljaspool kasti "pakkige lahti".

#### <a name="to-use-ftp"></a>FTP kasutamine

1. Järgige juhiseid [siin](../app-service-web/web-sites-deploy.md#ftp) saada FTP konfigureeritud.

2. Kui olete ühendatud funktsioon rakendus saidile, kopeerige värskendatud *host.json* faili `/site/wwwroot` või funktsioon failide kopeerimine `/site/wwwroot/<function_name>`.

#### <a name="to-use-continuous-deployment"></a>Pideva kasutamine

Järgige juhiseid teemas [pidev juurutamise Azure'i funktsioonide](functions-continuous-deployment.md).

## <a name="parallel-execution"></a>Paralleelne

Kui mitme sünergilise sündmused kiiremini ühelõimelised funktsioon käitusaja saab töödelda, pikkus võib kasutada mitu korda paralleelselt funktsiooni.  Kui funktsioon rakendus kasutab [Dünaamilise teenuse kavandamine](functions-scale.md#dynamic-service-plan), funktsioon rakendus võib skaala välja automaatselt.  Kas rakendus töötab dünaamilise teenuse leping või mõne tavalise [Rakenduse teenuse leping](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md), iga eksemplari funktsioon rakendus, võib protsessi samaaegseid funktsioon manamine samaaegselt mitu Teemad abil.  Samaaegseid funktsioon manamine iga funktsioon rakenduse eksemplari maksimumarv sõltub funktsioon rakenduse mälu suurust. 

## <a name="azure-functions-pulse"></a>Azure'i funktsioonide Pulse'i  

Pulse'i on reaalajas sündmuse voo, mis kuvab sageduse oma funktsioon töötab ka teatud omadustega elementide ja ebaõnnestumist. Samuti saate jälgida oma täitmisaeg. Me lisame rohkem funktsioone ja kohandamine selle aja jooksul. Pääsete **Pulse'i** lehe **jälgimine** menüü.

## <a name="repositories"></a>Hoidlate

Azure'i funktsioonide kood on avatud allikas ja GitHub hoidlate salvestatud.

* [Azure'i funktsioonide käitusaja](https://github.com/Azure/azure-webjobs-sdk-script/)
* [Azure'i funktsioonide portaal](https://github.com/projectkudu/AzureFunctionsPortal)
* [Azure'i funktsioonide Mallid](https://github.com/Azure/azure-webjobs-sdk-templates/)
* [Azure'i WebJobs SDK](https://github.com/Azure/azure-webjobs-sdk/)
* [Azure'i WebJobs SDK laiendid](https://github.com/Azure/azure-webjobs-sdk-extensions/)

## <a name="bindings"></a>Seosed

Siit leiate kõik toetatud sidumiste tabelisse.

[AZURE.INCLUDE [dynamic compute](../../includes/functions-bindings.md)]

## <a name="reporting-issues"></a>Probleemidest teatamine

[AZURE.INCLUDE [Reporting Issues](../../includes/functions-reporting-issues.md)] 

## <a name="next-steps"></a>Järgmised sammud

Lisateavet järgmistest teemadest:

* [Azure'i funktsioonide C# tootearendusmaterjal](functions-reference-csharp.md)
* [Azure'i funktsioonide F # tootearendusmaterjal](functions-reference-fsharp.md)
* [Azure'i funktsioonide NodeJS tootearendusmaterjal](functions-reference-node.md)
* [Azure'i funktsioonide päästikute ja seosed](functions-triggers-bindings.md)
* [Azure funktsioonid: The reisi](https://blogs.msdn.microsoft.com/appserviceteam/2016/04/27/azure-functions-the-journey/) Azure'i rakendust Service meeskonna ajaveebist. Kuidas Azure'i funktsioonid on välja töötatud ajalugu.





