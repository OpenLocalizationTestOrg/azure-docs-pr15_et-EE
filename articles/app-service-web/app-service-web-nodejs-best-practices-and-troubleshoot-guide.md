<properties
    pageTitle="Parimate tavade ja Azure veebirakenduste sõlm rakenduste tõrkeotsingu juhend"
    description="Lugege heade tavade ja tõrkeotsingutoiminguid sõlm rakenduste Azure Web Apps."
    services="app-service\web"
    documentationCenter="nodejs"
    authors="ranjithr"
    manager="wadeh"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="06/06/2016"
    ms.author="ranjithr;wadeh"/>
    
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-web-apps"></a>Parimate tavade ja Azure veebirakenduste sõlm rakenduste tõrkeotsingu juhend

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Selles artiklis saate teada, head tavad ja Azure veebirakenduste ( [iisnode](https://github.com/azure/iisnode)) koos töötavad [sõlm rakenduste](app-service-web-nodejs-get-started.md) tõrkeotsingujuhiseid.

>[AZURE.WARNING] Olge ettevaatlik kasutamine oma tootmiskohast tõrkeotsingujuhiseid. Soovitus on tõrkeotsing mitte tekitamiseks häälestamine rakenduse näiteks oma vahekausta pesa ja kui probleem ei lahenenud, vahetada oma vahekausta pesa oma tootmise pesa.

## <a name="iisnode-configuration"></a>IISNODE konfigureerimine

See [skeemifaili](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) kuvatakse kõik sätted, mida saab konfigureerida iisnode. Mõned sätted, mis on kasulik rakenduse on:

* nodeProcessCountPerApplication

    See säte määrab arvu sõlm protsessid, mis on käivitatud IIS-i rakenduse kohta. Vaikeväärtus on 1. Saate oma VM core arvu nimega nii palju node.exe käivitada, seades selle 0. Soovitatav väärtus on 0 enamik rakenduse nii, et kõik selle valdkond saate kasutada oma arvutisse. Node.exe on ühe keermestatud nii ühe node.exe tarbib kuni 1 Core ja saada kuni te soovite kasutada kõigi südamike sõlm rakenduse jõudlust.

* nodeProcessCommandLine

    See säte määrab selle node.exe tee. Saate määrata selle väärtuseks osutage oma node.exe versiooni.

* maxConcurrentRequestsPerProcess

    See säte määrab samaaegseid taotlusi saadetud iisnode iga node.exe maksimaalne arv. Azure'i veebirakenduste, klõpsake selle vaikeväärtus on lõpmatu. Neil pole see säte muretsema. Väljaspool azure veebirakenduste, vaikeväärtus on 1024. Võite seda sõltuvalt sellest, mitu taotleb rakenduse saab ja kui kiiresti rakenduse töötleb iga taotluse konfigureerimine.

* maxNamedPipeConnectionRetry

    See säte määrab maksimaalne arv kordi iisnode proovib tegemisega ühenduse nimega toru üle kutse saatmiseks node.exe. See säte koos namedPipeConnectionRetryDelay määratleb kogu aeg, mille jooksul iisnode iga taotluse. Vaikeväärtus on 200 Azure'i veebirakenduste kohta. Kokku ajalõpp sekundites = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

* namedPipeConnectionRetryDelay

    See säte juhtelementide aeg (millisekundites) iisnode summa ootama proovi uuesti kutse saatmiseks node.exe üle nimega toru vahele. Vaikeväärtus on 250ms.
    Kokku ajalõpp sekundites = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

    Vaikimisi on kogu aeg, mille iisnode azure veebirakenduste kohta rakenduses 200 \* 250ms = 50 sekundit.

* logDirectory

    See säte määrab kataloogi, kus iisnode logib stdout/stderr. Vaikeväärtus on iisnode, mis on sõltuv peamised skripti kataloogi (juhul, kui esineb peamine server.js directory)

* debuggerExtensionDll

    See säte määrab, millist versiooni sõlm-inspektor iisnode kasutab silumine sõlm rakenduse. Praegu iisnode-inspektor-0.7.3.dll ja iisnode-inspector.dll on see säte ainult 2 sobivad väärtused. Vaikeväärtus on iisnode-inspektor-0.7.3.dll. iisnode-inspektor-0.7.3.dll versioon kasutab sõlm-inspektor-0.7.3 ja kasutab websockets, seega tuleb lubada websockets sisse oma azure Web Appis või uuemat versiooni. Vaadake, kuidas konfigureerida kasutama uut sõlm inspektor iisnode <http://www.ranjithr.com/?p=98> rohkem üksikasju.

* flushResponse

    IIS-i vaikekäitumine on, et see puhvrite vastuse andmete üles enne õhetus või vastuse, kuni 4 MB kumb on esimene. iisnode pakub Otsingukonfiguratsiooni sätted alistada seda käitumist: fragment vastuse olemi keha tühjendada kohe, kui iisnode saab seda node.exe peate määrama selle iisnode/@flushResponse atribuut web.config "TRUE":
    
    ```
    <configuration>    
        <system.webServer>    
            <!-- ... -->    
            <iisnode flushResponse="true" />    
        </system.webServer>    
    </configuration>
    ```

    Lubamine õhetus iga fragment vastuse olemi keha lisatakse jõudluse pea kohal, mis vähendab süsteemi läbilaskevõime 5% (alates v0.1.13) nii, et see on parim ulatus seda sätet ainult lõpp-punktid, mis nõuavad vastuse streaming (nt abil soovitud <location> elemendi Web.config)

    Lisaks sellele streaming rakendused, peate oma iisnode sündmuseohjuri, responseBufferLimit ka väärtuseks 0.
    
    ```
    <handlers>    
        <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>    
    </handlers>
    ```

* watchedFiles

    See on faile, mis kuvatakse vaadatud muudatuste semikooloniga eraldatud loend. Faili muutmine põhjustab rakenduse prügikasti. Iga kirje koosneb on valikuline kaustanimi nõutav faili nimi, mis on kataloogi, kus asub rakenduse sisendpunkti suhtes. Metamärgistust on lubatud ainult osa faili nimi. Vaikeväärtus on "\*. js;web.config"

* recycleSignalEnabled

    Vaikeväärtus on false. Kui lubatud, saab ühendada rakenduse sõlm nimega Triibu (muutuja IISNODE\_JUHTELEMENDI\_toru) ja saatke sõnum "prügikast". See võib põhjustada selle leiate w3wp nõtkelt taaskasutada.

* idlePageOutTimePeriod

    Vaikeväärtus on 0, mis tähendab, et see funktsioon on keelatud. Kui valitud mõni väärtus suurem kui 0, iisnode kuvatakse lehel välja oma lapse protsesside iga 'idlePageOutTimePeriod' millisekundit. Mõistmaks, mis tähendab välja leht, vaadake selle [dokumentatsiooni](https://msdn.microsoft.com/library/windows/desktop/ms682606.aspx). See säte on kasu rakendusi, mis peab olema palju mälu ja soovite pageout mälu kettale aeg-ajalt vabastamiseks RAM.

>[AZURE.WARNING] Olge ettevaatlik, kui lubamine mängufilmide konfiguratsiooni järgmised sätted. Soovitus on ei luba neid reaalajas tootmise rakendused.

* debugHeaderEnabled

    Vaikeväärtus on false. Kui on seatud tõene, iisnode tuleb lisada mõne HTTP vastuse päise iisnode-silumine iga HTTP vastus saadetakse iisnode-silumine päise väärtus on URL-i. Üksikute osade diagnostikateave saate gleaned vaadates URL-i fragment, kuid parema visualiseering on saavutada URL-i avamine brauseris.

* loggingEnabled

    See säte määrab logimine stdout ja stderr iisnode järgi. Iisnode kuvatakse stdout/stderr sõlm protsesside See käivitab jäädvustada ja kirjutada 'logDirectory' sätetes määratud kataloogi. Kui see ei luba, rakenduse kirjalikult logid failisüsteemi ning sõltuvalt tehtud rakenduse logimine summa, võib jõudlus mõju.

* devErrorsEnabled

    Vaikeväärtus on false. Kui kuvatakse on seatud tõene, iisnode HTTP olekukoodi ja Win32 tõrkekoodi brauseris. Win32 kood on kasulik silumine teatud tüüpi probleemid.
    
* debuggingEnabled (lubamiseks reaalajas tootmise kohapeal)

    See säte määrab silumine funktsiooni. Iisnode on integreeritud sõlm-inspektor. Selle sätte lubamine lubate silumine sõlm rakenduse. Kui see säte on lubatud, kuvatakse iisnode vajalikud sõlm-inspektor failide 'debuggerVirtualDir' Directorys esimese silumine taotluse sõlm rakenduse paigutus. Võite laadida sõlm-inspektor, saates taotluse http://yoursite/server.js/debug. 'DebuggerPathSegment' sättega saate määrata lõigu silumine URL-i. Poolt vaikimisi debuggerPathSegment = 'silumine. Saate seda GUID näiteks nii, et see oleks raskem avastada teistele.

    Märkige see [link](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) silumine kohta lisateabe saamiseks.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Stsenaariumid ja soovitused/tõrkeotsing

### <a name="my-node-application-is-making-too-many-outbound-calls"></a>Minu sõlm rakenduse on liiga palju väljaminevate kõnede tegemine.

Palju rakendusi soovite muuta nende tavalise tegevuse käigus Väljaminevad ühendused. Näiteks kui taotlus on saadaval, sõlm rakenduse soovite kontakti REST API mujal ja saada taotluse osa teabest. Mida soovite kasutada Hoia elus agent, kui http- või https helistamist. Näiteks võite agentkeepalive mooduli teie Hoia elus agendina kui väljaminevate kõnede. See tagab, et teie azure Web Appis VM klõpsake uuesti kasutada funktsiooni sockets ja vähendada on luua uus sockets iga väljamineva taotlus pea kohal. Ka see tagab, et kasutate vähem arv sockets teha palju väljaminev taotlusi ja seetõttu ei ületa maxSockets, mis on eraldatud VM kohta. Azure'i veebirakenduste soovitus oleks seatud agentKeepAlive maxSockets väärtus kokku 160 sockets VM kohta. See tähendab, et kui teil on 4 node.exe töötavate VM, tuleks soovite seada agentKeepAlive maxSockets 40, mis on 160 kohta VM kokku node.exe kohta.

Näide agentKeepALive konfiguratsioon:

```
var keepaliveAgent = new Agent({    
    maxSockets: 40,    
    maxFreeSockets: 10,    
    timeout: 60000,    
    keepAliveTimeout: 300000    
});
```

Selles näites eeldab, et teil on 4 node.exe töötab teie VM. Kui teil on node.exe töötavate VM erinevat hulka, on teil muuta maxSockets, seada vastavalt sellele.

### <a name="my-node-application-is-consuming-too-much-cpu"></a>Minu sõlm rakenduse tarbib liiga palju CPU.

Tõenäoliselt saate soovitus: Azure'i veebirakenduste oma portaalis suur cpu tarbimine kohta. Saate häälestada ka kuvarite teatud [mõõdikute](web-sites-monitor.md)vaadata. CPU hõivatus [Azure portaali armatuurlaua](../application-insights/app-insights-web-monitor-performance.md)kontrollimisel kontrollige MAX väärtused CPU nii, et te ei jääks märkamata tippväärtus väärtused.
Kui arvate, et teie taotlus tarbib liiga palju CPU ja te ei saa selgitada, miks juhtudel peate profiil sõlm rakenduse.

### 

#### <a name="profiling-your-node-application-on-azure-webapps-with-v8-profiler"></a>Azure'i veebirakenduste abil V8-Profiler sõlm rakendus oma profiili

Näiteks võimaldab Oletagem, et teil on Tere tulemast maailma rakendus, mida soovite profiili, nagu allpool näidatud:

```
var http = require('http');    
function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    WriteConsoleLog();    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Minge oma scm saidi https://yoursite.scm.azurewebsites.net/DebugConsole

Kuvatakse Käsuviip, nagu allpool näidatud. Avage oma sait/wwwroot kataloog

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

Käivitage käsk "npm install v8-profiler"

See peaksite installima v8-profiler sõlme all\_moodulid kataloog ja kõik sõltuvustega.
Nüüd, muuta oma profiili rakenduse server.js.

```
var http = require('http');    
var profiler = require('v8-profiler');    
var fs = require('fs');

function WriteConsoleLog() {    
    for(var i=0;i<99999;++i) {    
        console.log('hello world');    
    }    
}

function HandleRequest() {    
    profiler.startProfiling('HandleRequest');    
    WriteConsoleLog();    
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));    
}

http.createServer(function (req, res) {    
    res.writeHead(200, {'Content-Type': 'text/html'});    
    HandleRequest();    
    res.end('Hello world!');    
}).listen(process.env.PORT);
```

Eespool kirjeldatud muudatused kuvatakse funktsiooni WriteConsoleLog profiil ja seejärel sisestage profiili väljundi "profile.cpuprofile" faili jaotises teie saidi wwwroot. Rakenduse saata. Kuvatakse teie saidi wwwroot loodud "profile.cpuprofile" faili.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Selle faili alla laadida ja peate selle faili avamiseks Chrome'i F12 tööriistu. Tulemus F12 chrome ja seejärel klõpsake vahekaarti"Profiilid". Klõpsake nupu "Laadi". Valige oma profile.cpuprofile lihtsalt allalaaditud faili. Klõpsake lihtsalt laadimist profiili.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Näete, et 95% ajast oli tarbitud WriteConsoleLog funktsioon nagu allpool näidatud. See näitab ka täpse reanumbrite ja allikas faile, mis põhjustavad küsimus.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Minu sõlm rakenduse tarbib liiga palju mälu.

Tõenäoliselt saate soovitus: Azure'i veebirakenduste oma portaalis kõrge mälukasutuse kohta. Saate häälestada ka kuvari jaoks teatud [mõõdikute](web-sites-monitor.md)vaadata. [Azure'i portaalis armatuurlaua](../application-insights/app-insights-web-monitor-performance.md)mälukasutust kontrollimisel Kontrollige mälu MAX väärtusi nii, et te ei jääks märkamata tippväärtus väärtused.

#### <a name="leak-detection-and-heap-diffing-for-nodejs"></a>Lekke leidmise ja hunnik Diffing node.js jaoks 

Võite [sõlm-memwatch](https://github.com/lloyd/node-memwatch) , mis aitavad teil tuvastada mälulekete.
Saate installida memwatch nagu v8-profiler ja redigeerida oma koodi jäädvustada ja erinevuse korrastamine tuvastamiseks mälu lekked oma rakenduse.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Minu node.exe tapetakse CEIP otsimine 

On mõned põhjused, miks see võib juhtuda.

1.  Rakenduse korrutamine tabamatu erandid – palun sisse d:\\home\\LogFiles\\rakenduse\\logimine-errors.txt faili andmed on erand. See fail on virnas Jälita nii, et saate määrata selle rakenduse.

2.  Rakenduse tarbib liiga palju mälu, mis mõjutavad muud protsessid alustamine. Kokku VM mälu on peaaegu 100%, võib teie node.exe tappis protsessi manager lasta muude protsesside saad tegema. Vea parandamiseks veenduge, et teie rakendus ei leki mälu või saate rakenduse väga peab kasutama palju mälu, mastaapimiseks palun palju rohkem RAM suuremat VM.

### <a name="my-node-application-does-not-start"></a>Minu sõlm rakendus ei käivitu

Kui teie taotlus on tagasi 500 tõrked käivitamisel, võib olla mitu põhjust:

1.  Node.exe pole õiges asukohas. Märkige ruut nodeProcessCommandLine säte.

2.  Peamised skriptifail pole õiges asukohas. Märkige ruut web.config ja veenduge, et peamine skriptifail käitleja jaotise nimi vastab peamine skriptifail.

3.  Web.config konfiguratsiooni pole õige – märkige sätete nimed või väärtused.

4.  Külmkäivituse – rakenduse võtab liiga kaua aega käivitamisel. Kui teie taotlus kestab kauem kui (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000 sekundi, tulemuseks iisnode 500 tõrge. Neid sätteid taotluse algusaeg iisnode ajastus ja esitus 500 tõrke vältimiseks väärtuste suurendamine

### <a name="my-node-application-crashed"></a>Minu sõlm rakenduse tabas krahh

Rakenduse korrutamine tabamatu erandid – palun sisse d:\\home\\LogFiles\\rakenduse\\logimine-errors.txt faili andmed on erand. See fail on virnas Jälita nii, et saate määrata selle rakenduse.

### <a name="my-node-application-takes-too-much-time-to-startup-cold-start"></a>Minu sõlm rakenduse võtab liiga palju aega käivitamisel (külm käivitamine)

Kõige levinum põhjus on selles, et rakendus on palju faile, mis on sõlme\_moodulid ja rakendus proovib laadimine käivitamisel enamik need failid. Vaikimisi, kuna teie failid asuvad võrgukettal, klõpsake Azure veebirakenduste, laadimine nii palju faile võib võtta aega.
See kiiremini teha mõned lahendused on:

1.  Veenduge, et teil on tasapinnalise sõltuvus struktuuri ja dubleeritud sõltuvused installimiseks oma moodulid npm3 abil.

2.  Proovi rongile oma sõlme laadimine\_moodulid ja kõik moodulid käivitamisel laadida. See tähendab, et kõne require('module') tuleks teha, kui te tegelikult vajate proovite kasutada mooduli funktsioonis.

3.  Azure'i veebirakenduste pakub funktsiooni nimega kohaliku vahemälu. See funktsioon kopeerib sisu võrgukohast kohalikule kettale VM. Kuna failid on kohalik, laadimise ajal sõlm\_moodulid on palju kiirem. – See [dokumentatsiooni](../app-service/app-service-local-cache.md) selgitatakse, kuidas kasutada kohaliku vahemälu üksikasjalikumalt.

## <a name="iisnode-http-status-and-substatus"></a>IISNODE http oleku ja substatus

[Lähtefail](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) on loetletud kõik võimalikud olek/substatus kombinatsioon iisnode võib tagastada tõrke korral.

Luba FREB rakenduse kuvamiseks win32 tõrkekoodi (Veenduge, et lubate FREB ainult mitte tekitamiseks saitidel jõudluse).

| Http olek | Http SubStatus | Võimalik põhjus?                                                                                                                                                                                            
|-------------|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------
| 500         | 1000           | Esines mõni probleem edasisaatmine taotluse IISNODE – märkige ruut, kui node.exe käivitada. Node.exe võib olla käivitamisel tabas krahh. Kontrollige oma web.config konfiguratsiooni tõrkeid.                                                                                                                                                                                                                                                                                     |
| 500         | 1001           | -Win32Error 0x2 - URL-i rakendus ei reageeri. Otsige URL-i ümberkirjutamine reeglid või kui teie kiire rakendus on õige marsruudib, mis on määratletud. -Win32Error 0x6d – nimega toru on hõivatud – Node.exe ei aktsepteeri taotlusi, kuna toru on hõivatud. Märkige ruut kõrge cpu hõivatus. -Muud vead – märkige ruut, kui node.exe tabas krahh.
| 500         | 1002           | Node.exe tabas krahh – Vaata d:\\home\\LogFiles\\logimine-errors.txt virnas jälgi.                                                                                                                                                                                                                                                                                                                                                                                        |
| 500         | 1003           | Toru konfiguratsiooni probleem – näete seda kunagi, kuid kui te ei tee, nimega Triibu konfiguratsioon on vale.                                                                                                                                                                                                                                                                                                                                                          |
| 500         | 1004-1018      | Kutse saatmine või vastuse node.exe ja sealt töötlemise ajal ilmnes mõni tõrge. Märkige ruut, kui node.exe tabas krahh. Märkige ruut d:\\home\\LogFiles\\logimine-errors.txt virnas jälgi.                                                                                                                                                                                                                                                                                    |
| 503         | 1000           | Pole piisavalt mälu eraldada rohkem nimega toru ühendusi. Miks on rakenduse tarbimine nii palju mälu sisse. Kontrollige maxConcurrentRequestsPerProcess säte väärtust. Kui see ei ole lõpmatu ja teil on palju taotlusi, suurendada seda väärtust tõrke vältimiseks.                                                                                                                                                                                                                                                                                                                  |
| 503         | 1001           | Kutse saanud ei saadeta node.exe kuna rakendus on ringlussevõtu. Kui rakendus on ümber töödelda, taotlused tavaliselt kätte.                                                                                                                                                                                                                                                                                                               |
| 503         | 1002           | Märkige ruut win32 tõrkekoodi tegelik põhjus-kutse saanud ei saadeta on node.exe.                                                                                                                                                                                                                                                                                                                                                                               |
| 503         | 1003           | Nimega toru on hõivatud – märkige ruut, kui palju CPU tarbib sõlm                                                                                                                                                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
Seal on säte sees sõlm nimega NODE.exe\_kuni\_TRIIBU\_EKSEMPLARI. See väärtus on vaikimisi väljaspool azure veebirakenduste 4. See tähendab, et selle node.exe vastu ainult 4 taotlusi nimega toru korraga. Azure'i veebirakenduste, klõpsake selle väärtuseks on seatud 5000 ja see väärtus peab olema piisavalt hea, et enamik töötavad azure veebirakenduste sõlm rakendused. Te ei näeks 503.1003 azure veebirakenduste kohta kuna me on suur väärtus sõlme\_kuni\_TRIIBU\_EKSEMPLARI.  |

## <a name="more-resources"></a>Veel ressursse

Järgige neid linke ja Lisateavet Azure'i rakendust Service node.js rakendusi.

* [Azure'i rakendust Service Node.js veebirakendustega alustamine](app-service-web-nodejs-get-started.md)
* [Kuidas Node.js veebirakenduse teenuses Azure rakenduse silumine](web-sites-nodejs-debug.md)
* [Azure'i rakendustega Node.js moodulid abil](../nodejs-use-node-modules-azure-apps.md)
* [Azure'i rakendust Service veebirakenduste: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js Arenduskeskus](../nodejs-use-node-modules-azure-apps.md)
* [Super salajase Kudu konsooli silumine uurimine](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)