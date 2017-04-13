<properties 
    pageTitle="Logide ja konsooli streaming" 
    description="Streaming logid ja konsooli ülevaade" 
    authors="btardif" 
    manager="wpickett" 
    editor="" 
    services="app-service\web" 
    documentationCenter=""/>

<tags 
    ms.service="app-service-web" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="multiple" 
    ms.topic="article" 
    ms.date="10/12/2016" 
    ms.author="byvinyal"/>

# <a name="streaming-logs-and-the-console"></a>Streaming logid ja konsooli

## <a name="streaming-logs"></a>Streaming logid

**Azure'i portaal** pakub integreeritud streaming Logi näitaja, mis võimaldab vaadata **Rakenduse teenuse** rakenduste jälgimine sündmuste reaalajas.  

See funktsioon seadistamiseks on vaja mõned lihtsad juhised:

- Jälgi koodi kirjutamine
- Lubage oma rakenduse rakenduse **Diagnostikalogid**
- Sisseehitatud **Streaming logid** kasutajaliidese voo kuvamine **Azure portaali**.

### <a name="how-to-write-traces-in-your-code"></a>Kuidas kirjutada oma koodi jälgi ###

Jälgi koodi kirjutamine on lihtne.  C# on sama lihtne kui kirjutamine järgmine kood:

`````````````````````````
Trace.TraceInformation("My trace statement");
`````````````````````````

`````````````````````````
Trace.TraceWarning("My warning statement");
`````````````````````````

`````````````````````````
Trace.TraceError("My error statement");
`````````````````````````

Jälita klassi elu System.Diagnostics nimeruumi.

Node.js rakenduse kirjutamise sama tulemuse saavutamiseks järgmine kood:

`````````````````````````
console.log("My trace statement").
`````````````````````````

### <a name="how-to-enable-and-view-the-streaming-logs"></a>Kuidas lubada ja vaadata streaming logib
![][BrowseSitesScreenshot]Diagnostika on lubatud kohta rakenduse alusel. Alustuseks saidile, mida soovite selle funktsiooni lubamiseks klõpsake sirvimine.  
  
![][DiagnosticsLogs]Menüü sätted, liikuge kerides jaotiseni **jälgimine** ja klõpsake **Diagnostikalogid (1)**. Seejärel **(2) luba** **Rakenduse logimine (failisüsteemi)** või **Rakenduse logimine (bloobimälu)** **tase** suvandit võimaldab teil muuta raskusaste jäädvustada jälgi. Kui proovite lihtsalt saate tutvuda funktsiooni, seadmiseks **Verbose** tagamaks, et kõik teie Jälita laused kogutakse tasemele.

Klõpsake nuppu **Salvesta** tera ülaosas ja olete valmis vaatamiseks logid.

>[AZURE.NOTE] Mida suurem toodeti **raskusaste tase** veel ressursse tarbib log ja veel jälgi. Veenduge, et õige Jaarittelu tootmiseks või veebiaadresside sait on konfigureeritud **raskusaste tase** . 

![][StreamingLogsScreenshot]**Logide streaming** : Azure'i portaali kuvamiseks klõpsake nuppu **Log (1) voo** ka menüü sätted jaotises **jälgimine** . Kui teie rakendus on aktiivselt kirjalikult Jälita laused, siis peaksite nägema neid rakenduses soovitud **(2) streaming logib UI** peaaegu reaalajas.

## <a name="console"></a>Konsooli
**Azure'i portaal** pakub konsooli juurdepääsu oma rakenduse. Saate uurida oma rakenduse failisüsteemi ja cmd/PowerShelli skriptide käitamiseks. Teil on seotud samu õigusi määrata oma töötava rakenduse koodi täitmisel konsooli käsud. Juurdepääs kaitstud kataloogid või suurendatud õigusi nõudvad käivitamine on blokeeritud.  

![][ConsoleScreenshot]Klõpsake menüü sätted kerige jaotise **Arengu tööriistad** ja klõpsake **Console (1)** ja **(2) konsooli** UI avaneb paremale.

Saate tutvuda **konsooli**, proovige põhilised käsud, nt:

`````````````````````````
dir
`````````````````````````

`````````````````````````
cd
`````````````````````````

<!-- Images. -->
[DiagnosticsLogs]: ./media/web-sites-streaming-logs-and-console/diagnostic-logs.png
[BrowseSitesScreenshot]: ./media/web-sites-streaming-logs-and-console/browse-sites.png
[StreamingLogsScreenshot]: ./media/web-sites-streaming-logs-and-console/streaming-logs.png
[ConsoleScreenshot]: ./media/web-sites-streaming-logs-and-console/console.png
