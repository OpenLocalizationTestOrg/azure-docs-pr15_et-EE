<properties
    pageTitle="Azure'i funktsioonide timer päästik | Microsoft Azure'i"
    description="Timer päästikute kasutamine Azure funktsioonide mõistmine."
    services="functions"
    documentationCenter="na"
    authors="christopheranderson"
    manager="erikre"
    editor=""
    tags=""
    keywords="Azure'i töötab, funktsioonide, sündmuse töötlemiseks, dünaamiline Arvuta, serverless arhitektuur"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="reference"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="chrande; glenga"/>

# <a name="azure-functions-timer-trigger"></a>Azure'i funktsioonide timer päästik

[AZURE.INCLUDE [functions-selector-bindings](../../includes/functions-selector-bindings.md)]

Selles artiklis selgitatakse, kuidas konfigureerida timer päästikute Azure'i funktsioonides. Ajastiteenuse käivitab kõne ülesannete ajakava, üks kord või korduv alusel.  

[AZURE.INCLUDE [intro](../../includes/functions-bindings-intro.md)] 

## <a name="functionjson-for-timer-trigger"></a>function.JSON timer päästik jaoks

*Function.json* faili pakub ajakava avaldis. Näiteks järgmise ajakava töötab funktsiooni iga minut:

```json
{
  "bindings": [
    {
      "schedule": "0 * * * * *",
      "name": "myTimer",
      "type": "timerTrigger",
      "direction": "in"
    }
  ],
  "disabled": false
}
```

Ajastiteenuse päästik tegeleb mitme eksemplari skaala välja automaatselt: ainult ühe eksemplari kindla timer funktsioon töötab üle kõik aknad.

## <a name="format-of-schedule-expression"></a>Vormingu ajakava avaldis

Ajakava avaldis on [CRON avaldis](http://en.wikipedia.org/wiki/Cron#CRON_expression) , mis sisaldab 6 välju: `{second} {minute} {hour} {day} {month} {day of the week}`. 

Pange tähele, et paljude cron avaldised, leiate Internetist {teise} välja jätta nii, et kui kopeerite üks neist, tuleb teil kohandada eest välja. 

Siin on mõned muud ajakava avaldis näited:

Iga 5 minuti järel käivitamiseks:

```json
"schedule": "0 */5 * * * *"
```

Käivitamiseks üks kord tunnis ülaosas:

```json
"schedule": "0 0 * * * *",
```

Iga kahe tunni järel käivitamiseks:

```json
"schedule": "0 0 */2 * * *",
```

Käivitamiseks kord tunnis 09: 00-17:

```json
"schedule": "0 0 9-17 * * *",
```

Käivitamiseks kell 9:30 iga päev:

```json
"schedule": "0 30 9 * * *",
```

Käivitamiseks kell 9:30 igal:

```json
"schedule": "0 30 9 * * 1-5",
```

## <a name="timer-trigger-c-code-example"></a>Ajastiteenuse päästik C# koodi näide

C# koodi näites kirjutab ühe Logi iga kord, kui käivitatakse funktsiooni.

```csharp
public static void Run(TimerInfo myTimer, TraceWriter log)
{
    log.Info($"C# Timer trigger function executed at: {DateTime.Now}");    
}
```

## <a name="next-steps"></a>Järgmised sammud

[AZURE.INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)] 
