Azure'i määrab, et teie rakendus kasutab Python, **kui mõlemad järgmised tingimused on täidetud**:

- Requirements.txt faili juurkausta
- mis tahes .py faili juurkausta või runtime.txt, mis määrab python

Kui see on nii, kasutatakse selle Python konkreetse juurutamise skript, mis teeb standard sünkroonimise faile kui ka täiendavad Python toimingud, näiteks:

- Virtuaalse keskkonna automaatne haldamine
- Installimise paketid loendis requirements.txt pip abil
- Valitud Python versiooni korral web.config loomine.
- Staatilise failide Django rakenduste kogumine

Ilma skripti kohandamiseks saate määrata teatud aspektide vaikimisi juurutamise juhiseid.

Kui soovite kõik Python konkreetse juurutamise sammu vahele jätta, saate luua tühja faili:

    \.skipPythonDeployment

Kui soovite saidikogumi staatiliste failide Django rakenduse vahele jätta.

    \.skipDjango 

Juurutamise üle rohkem kontrolli, saate alistada vaikimisi juurutamise skripti, luues järgmised failid:

    \.deployment
    \deploy.cmd

[Azure'i käsurea liides][] abil saate faile luua.  Projekti kaustast, kasutage järgmist käsku:

    azure site deploymentscript --python

Kui need failid pole olemas, Azure'i loob ajutiselt skripti ja see töötab.  See on identne ülaltoodud käsuga loomist.

[Azure'i käsurea liides]: http://azure.microsoft.com/downloads/
