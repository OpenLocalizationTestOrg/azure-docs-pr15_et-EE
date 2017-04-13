<properties
    pageTitle="Alustamine asjade jaoturi lüüsi SDK | Microsoft Azure'i"
    description="Kasutamise illustreerimiseks tuleks mõista, kui kasutate SDK Azure'i asjade lüüsi võtme põhimõtet Windows Azure'i asjade lüüsi SDK kiirtutvustus."
    services="iot-hub"
    documentationCenter=""
    authors="chipalost"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="cpp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="08/25/2016"
     ms.author="andbuc"/>


# <a name="azure-iot-gateway-sdk-beta---get-started-using-windows"></a>Azure'i asjade lüüsi SDK (beeta) – Windows kasutamise alustamine

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-selector](../../includes/iot-hub-gateway-sdk-getstarted-selector.md)]

## <a name="how-to-build-the-sample"></a>Kuidas koostada valimi

Enne alustamist peate [oma arenduskeskkond häälestamine] [ lnk-setupdevbox] Windows SDK töötamiseks.

1. Avage käsuviip **arendaja Käsuviip VS2015 jaoks** .
2. Liikuge oma kohaliku eksemplari **Azure'i asjade Interneti-lüüsi sdk** hoidla juurkausta.
3. Käivitage selle **Tööriistad\\build.cmd** skripti. See skript loob faili lahenduse Visual Studios, koostab lahendus ja töötab testide. **Koostada** kausta lahenduse Visual Studios leiate **Azure'i asjade Interneti-lüüsi sdk** hoidla kohaliku eksemplari.

## <a name="how-to-run-the-sample"></a>Valimi käivitamise

1. Skripti **build.cmd** loob kausta nimega **koostada** hoidla kohaliku eksemplari. See kaust sisaldab kahte moodulid selles näites kasutatakse.

    Koosta skripti paigutab **logger_hl.dll** sisse selle **koostamine\\moodulid\\puuraidur\\silumine** kaust ja klõpsake **hello_world_hl.dll** soovitud **koostamine\\moodulid\\hello_world\\silumine** kausta. Kasutada need teed **mooduli tee** väärtus, nagu on näidatud allpool JSON sätete fail.

2. Klõpsake faili **hello_world_win.json** soovitud **näidised\\hello_world\\src** kaust on näiteks JSON sätted faili Windowsi, mille abil saate käivitada valimi. Alltoodud näites JSON sätted eeldab, et te kloonitud asjade lüüsi SDK hoidla juurkausta **c** -ketas. Kui laadisite teise asukohta, peate reguleerimiseks **mooduli tee** väärtused on **näidised\\hello_world\\src\\hello_world_win.json** faili vastavalt sellele.

3. **Logger_hl** mooduli, **argumendid** , määrake jaotises **faili nimi** väärtus log andmeid sisaldav faili tee ja nimi.

    See on näide JSON sätted faili Windows, mis kirjutab **c** -ketas juurkaustas **log.txt** fail.

    ```
    {
      "modules" :
      [
        {
          "module name" : "logger_hl",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\modules\\logger\\Debug\\logger_hl.dll",
          "args" : 
          {
            "filename":"C:\\log.txt"
          }
        },
        {
          "module name" : "hello_world",
          "module path" : "C:\\azure-iot-gateway-sdk\\build\\\\modules\\hello_world\\Debug\\hello_world_hl.dll",
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

3. Käsureale, liikuge oma kohaliku eksemplari **Azure'i asjade Interneti-lüüsi sdk** hoidla juurkausta.
4. Käivitage järgmine käsk:
  
  ```
  build\samples\hello_world\Debug\hello_world_sample.exe samples\hello_world\src\hello_world_win.json
  ```

[AZURE.INCLUDE [iot-hub-gateway-sdk-getstarted-code](../../includes/iot-hub-gateway-sdk-getstarted-code.md)]

<!-- Links -->
[lnk-setupdevbox]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/doc/devbox_setup.md