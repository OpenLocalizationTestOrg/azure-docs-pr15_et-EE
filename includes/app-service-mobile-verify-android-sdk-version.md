Poolelioleva arengut, kuna ei kattu Android SDK versioon Android Studios installitud versiooni kood. Selles õpetuses viidatud Androidi SDK on kirjutamise ajal uusim versioon 23. Versiooninumbri võib suurendada, kuvatakse uus versioonides SDK ning soovitame kasutada uusim versioon, mis on saadaval.

Kaks versiooni lahknevuse sümptomid on:

1. Kui ehitada või taastada projekti, saate Gradle tõrketeateid, nt "**ei leitud target Google Inc.:Google APIs:n**".

2. Standardse Androidi objektide kood, mis tuleks lahendada vastavalt `import` laused võib genereerimine tõrketeated.

Kui üks neist, ei kattu versioon Android SDK installitud Androidi Studio SDK target allalaaditud projekti.  Veendumaks, et versiooni, saate teha järgmisi muudatusi:


1. Androidi Studios, klõpsake nuppu **Tööriistad** => **Androidi** => **SDK Manager**. Kui teil on installitud SDK Platform uusim versioon, siis klõpsake selle installimiseks. Kirjutage versiooninumber.

2. Project Exploreri jaotises **Gradle skriptide**, avage fail **build.gradle (modeule: rakenduse)**. Tagama, et **compileSdkVersion** ja **buildToolsVersion** installitud SDK uusim versioon. Silte võib välja nägema selline:
 
            compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
    
3. Androidi Studio Project Explorer paremklõpsake projekti sõlm, valige **Atribuudid**ja valige vasakpoolsest veerust **Androidi**. Veenduge, et **Projekti koostamine Target** väärtuseks on määratud SDK sama versioon nagu **targetSdkVersion**.

4. Androidi Studios, Avaldamisfail enam saab määrata target SDK- ja SDK miinimumversioon, erinevalt Eclipse puhul.
