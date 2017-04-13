
Kõigi funktsioonide antud funktsiooni rakenduse koodi asub root kausta, mis sisaldab host konfiguratsioonifail ja üks või mitu alamkaustad, mis sisaldavad eraldi funktsioon, nagu järgmises näites kood

```
wwwroot
 | - host.json
 | - mynodefunction
 | | - function.json
 | | - index.js
 | | - node_modules
 | | | - ... packages ...
 | | - package.json
 | - mycsharpfunction
 | | - function.json
 | | - run.csx
```

*Host.json* fail sisaldab teatud käitusaja konfiguratsioon ja funktsioon rakenduse juurkaustas asub. Saadaolevate sätete kohta leiate teavet teemast [host.json](https://github.com/Azure/azure-webjobs-sdk-script/wiki/host.json) WebJobs.Script hoidla viki sisse.

Iga funktsiooni on kaust, mis sisaldab ühte või mitut koodi faili, function.json konfiguratsiooni ja muud sõltuvused.