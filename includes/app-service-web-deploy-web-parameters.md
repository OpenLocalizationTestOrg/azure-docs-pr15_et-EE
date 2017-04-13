Koos Azure ressursihaldur, määratlege parameetrite väärtused, mida soovite määrata malli juurutamisel. Mall sisaldab nimega parameetrite jaotis, mis sisaldab kõiki parameetrite väärtused.
Määratlege parameeter on need väärtused, mis erinevad juurutate projekti või keskkond, mis on kasutusele võtavad põhjal. Määratleda parameetrite väärtused, mis jäävad alati samaks. Iga parameetri väärtuse kasutatakse määratleda juurutatud ressursside mall. 

Parameetrite määratlemisel **allowedValues** välja abil määrata, milliseid väärtusi kasutaja saab sisestada käigus juurutamist. Kasutage **defaultValue** välja määramiseks väärtuse parameeter, kui väärtus puudub käigus juurutamist.

Kirjeldatakse iga parameetri malli.

### <a name="sitename"></a>siteName

Web appi, mille soovite luua nimi.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName

Rakenduse teenusleping majutusteenuse veebirakenduse jaoks nimi.
    
    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>SKU-ga

Hinnakirjad taseme majutusteenuse lepingu raames.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

Mall määratleb väärtused, mis on lubatud selle parameetri ja määrab vaikeväärtuse (S1), kui pole väärtust määratud.

### <a name="workersize"></a>workerSize

Eksemplari suurus majutusteenuse leping (väike, Keskmine või suur).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }
    
Malli väärtused, mis on lubatud selle parameetri (0, 1 või 2) määratleb ja määrab vaikeväärtuse (0), kui pole väärtust määratud. Väärtused vastavad väike, Keskmine ja suur.
