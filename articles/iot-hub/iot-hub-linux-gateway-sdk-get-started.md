<properties
    pageTitle="Alustamine asjade jaoturi lüüsi SDK | Microsoft Azure'i"
    description="Azure'i asjade lüüsi SDK jaotistes kasutab Linux illustreerimiseks võti peaks mõista, kui kasutate Azure asjade lüüsi SDK mõisted."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="get-started-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-linux"></a>Azure'i asjade lüüsi SDK (beta) - Linux kasutamise alustamine

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Kuidas koostada valimi

Enne alustamist peate [oma arenduskeskkond häälestamine] [ lnk-setupdevbox] töötamiseks SDK Linux.

1. Avage shell.
2. Liikuge oma kohaliku eksemplari **Azure'i asjade Interneti-lüüsi sdk** hoidla juurkausta.
3. Käivitage **tools/build.sh** skript. See skript kasutab **cmake** kasuliku looge kaust nimega teie kohaliku eksemplari **Azure'i asjade Interneti-lüüsi sdk** hoidla juurkaustas **koostamine** ja lisamine makefile luua. Skripti seejärel koostab lahendus ja käivitab testide.

> [AZURE.NOTE]  Iga kord, kui käivitate **build.sh** skripti, kustutab ja siis recreates **koostada** teie kohaliku eksemplari **Azure'i asjade Interneti-lüüsi sdk** hoidla juurkaustas kaust.

## <a name="how-to-run-the-sample"></a>Valimi käivitamise

1. Skripti **build.sh** loob selle väljundi **Azure'i asjade Interneti-lüüsi sdk** hoidla kohaliku koopia kaustas **koostamine** . See sisaldab selles näites kasutatakse kahte mooduleid.

    Koosta skripti paigutab **liblogger_hl.so** sisse selle **koostamine/moodulid/puuraidur/** kausta ja klõpsake **libhello_world_hl.so** soovitud **koostamine/moodulid/hello_world/** kausta. Kasutada need teed **mooduli tee** väärtus, nagu on näidatud allpool JSON sätete fail.

2. Faili **hello_world_lin.json** kaustas **näidised/hello_world/src** on näiteks JSON sätted faili Linux, mille abil saate käivitada valimi. Alltoodud näites JSON sätted eeldab, et käivitate valimi juurkaustas **Azure'i asjade Interneti-lüüsi sdk** hoidla kohaliku koopia.

3. **Logger_hl** mooduli, **argumendid** , määrake jaotises **faili nimi** väärtus log andmeid sisaldav faili tee ja nimi.

    See on kujutatud JSON sätted faili Linux, mis kirjutab **log.txt** kausta, kui käivitate valimi.

    ```
    {
      "modules" :
      [ 
        {
          "module name" : "logger_hl",
          "module path" : "./build/modules/logger/liblogger_hl.so",
          "args" : 
          {
            "filename":"./log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "./build/modules/hello_world/libhello_world_hl.so",
          "args" : null
        }
      ],
      "links" :
      [
        {
          "source": "hello_world",
          "sink": "logger_hl"
        }
      ]
    }
    ```

3. Liikuge oma Shell **Azure'i asjade Interneti-lüüsi sdk** kausta.
4. Käivitage järgmine käsk:
  
  ```
  ./build/samples/hello_world/hello_world_sample ./samples/hello_world/src/hello_world_lin.json
  ``` 

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md
