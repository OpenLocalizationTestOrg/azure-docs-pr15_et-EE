<properties
    pageTitle="Jälgi voogu Cloud Services rakenduses Azure'i diagnostika | Microsoft Azure'i"
    description="Azure'i rakenduse aidata silumine jõudluse, jälgimine, liikluse analüüsi ja Lisateavet jälgimise sõnumeid lisada."
    services="cloud-services"
    documentationCenter=".net"
    authors="rboucher"
    manager="jwhit"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="02/20/2016"
    ms.author="robb"/>



# <a name="trace-the-flow-of-a-cloud-services-application-with-azure-diagnostics"></a>Jälita pilveteenustega rakenduse Azure'i diagnostika voo

Jälitamine on viis rakenduse täitmise jälgimiseks, kui see töötab. Saate salvestada teavet tõrgete ja rakenduse täitmine logid, tekstifailidest või muudes seadmetes hilisemaks analüüsiks [System.Diagnostics.Trace](https://msdn.microsoft.com/library/system.diagnostics.trace.aspx), [System.Diagnostics.Debug](https://msdn.microsoft.com/library/system.diagnostics.debug.aspx)ja [System.Diagnostics.TraceSource](https://msdn.microsoft.com/library/system.diagnostics.tracesource.aspx) tunnid. Jälgimise kohta leiate lisateavet teemast [jälgimine ja Instrumenting rakendused](https://msdn.microsoft.com/library/zs6s4h68.aspx).


## <a name="use-trace-statements-and-trace-switches"></a>Jälita laused ja Jälita parameetrid

Lisades [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) rakenduse konfigureerimise ja helistamist System.Diagnostics.Trace või System.Diagnostics.Debug rakenduse koodis rakendada pilveteenustega rakenduse jälgimine. Kasutage konfiguratsiooni faili *app.config* töötaja rollid ja *web.config* web rollid. Kui loote uue majutatud teenuse Visual Studio malli abil, Azure'i diagnostika lisatakse automaatselt projekti ja selle DiagnosticMonitorTraceListener lisatakse konfiguratsioonifail rollid, mille lisate.

Jälita laused viimise kohta leiate teavet teemast [kohta: rakenduse koodi lisamine Jälita laused](https://msdn.microsoft.com/library/zd83saa2.aspx).

[Jälita parameetrid](https://msdn.microsoft.com/library/3at424ac.aspx) oma kood tuleks paigutada, saate määrata, kas esineb jälgimine ja kuidas olulisel on. See võimaldab teil jälgida olekut rakenduse tootmiskeskkonnas. See on eriti oluline business rakendus, mis kasutab mitmes arvutis töötab mitu komponendid. Lisateavet leiate teemast [kohta: konfigureerimine Jälita parameetrid](https://msdn.microsoft.com/library/t06xyy08.aspx).

## <a name="configure-the-trace-listener-in-an-azure-application"></a>Azure'i rakenduses Jälita kuulajale konfigureerimine

Jälita silumine ja TraceSource, on vaja häälestamist "kuulajatele" koguda ja salvestada saadetud sõnumid. Kuulajatele koguda, talletada ja marsruutimine jälgimise sõnumeid. Need otse sihtrühma, nt log, akna või tekstifaili väljund jälgimine. Azure'i diagnostika kasutab [DiagnosticMonitorTraceListener](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.diagnostics.diagnosticmonitortracelistener.aspx) klassi.

Enne järgmiste toimingute tegemiseks peab Azure diagnostika kuvari lähtestamine nurjus. Selle tegemiseks leiate [Microsoft Azure diagnostika lubamine](cloud-services-dotnet-diagnostics.md).

Pange tähele, et Mallid, mis on esitatud Visual Studio kasutamisel kuulajale konfiguratsiooni lisatakse automaatselt teie eest.


### <a name="add-a-trace-listener"></a>Jälita kuulajale lisamine

1. Avage oma rolli web.config või app.config faili.
2. Järgmine kood lisada faili. Muutke versiooni atribuuti kasutada komplekti viidatava versiooninumber. Komplekti versioon ei pruugi muutumisest Azure'i SDK uudiseid ei ole.

    ```
    <system.diagnostics>
        <trace>
            <listeners>
                <add type="Microsoft.WindowsAzure.Diagnostics.DiagnosticMonitorTraceListener,
                  Microsoft.WindowsAzure.Diagnostics,
                  Version=2.8.0.0,
                  Culture=neutral,
                  PublicKeyToken=31bf3856ad364e35"
                  name="AzureDiagnostics">
                  <filter type="" />
                </add>
            </listeners>
        </trace>
    </system.diagnostics>
    ```
    >[AZURE.IMPORTANT] Veenduge, et teil on project viide Microsoft.WindowsAzure.Diagnostics komplekti. Värskendage versiooninumber ülaltoodud XML-soovitatud Microsoft.WindowsAzure.Diagnostics pakett versiooni vastavaks.

3. Otsingukonfiguratsiooni faili salvestada.

Kuulajatele kohta leiate lisateavet teemast [Jälita kuulajatele](https://msdn.microsoft.com/library/4y5y10s7.aspx).

Pärast juhiste kuulajale lisada, saate lisada oma koodi Jälita laused.


### <a name="to-add-trace-statement-to-your-code"></a>Lisada oma koodi jälgimine

1. Avage lähtefail, rakenduse. Näiteks kuvatakse <RoleName>.cs faili töötaja rolli või web roll.
2. Lisage järgmine lause kasutamine, kui see on juba lisatud:
    ```
        using System.Diagnostics;
    ```
3. Lisage Jälita laused, kuhu soovite jäädvustada teavet rakenduse olek. Saate vormindada väljundi Jälita lause mitmesuguseid viise. Lisateavet leiate teemast [kohta: rakenduse koodi lisamine Jälita laused](https://msdn.microsoft.com/library/zd83saa2.aspx).
4. Salvestamine lähtefaili.
