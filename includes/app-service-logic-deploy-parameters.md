Koos Azure ressursihaldur, määratlege parameetrite väärtused, mida soovite määrata malli juurutamisel. Mall sisaldab nimega parameetrite jaotis, mis sisaldab kõiki parameetrite väärtused.
Määratlege parameeter on need väärtused, mis erinevad juurutate projekti või keskkond, mis on kasutusele võtavad põhjal. Määratleda parameetrite väärtused, mis jäävad alati samaks. Iga parameetri väärtuse kasutatakse malli määratleda ressursse, mis on juurutamine. 

Parameetrite määratlemisel **allowedValues** välja abil määrata, milliseid väärtusi kasutaja saab sisestada käigus juurutamist. Kasutage **defaultValue** välja määramiseks väärtuse parameeter, kui väärtus puudub käigus juurutamist.

Kirjeldatakse iga parameetri malli.

### <a name="logicappname"></a>logicAppName

Loogika luua rakenduse nimi.

    "logicAppName": {
        "type": "string"
    }