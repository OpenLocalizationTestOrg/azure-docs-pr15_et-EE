<properties
    pageTitle="Töötamine Node.js moodulid"
    description="Saate teada, kuidas töötada Node.js moodulid Azure'i rakendust Service või pilveteenustega kasutamisel."
    services=""
    documentationCenter="nodejs"
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="multiple"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="nodejs"
    ms.topic="article"
    ms.date="08/11/2016"
    ms.author="robmcm"/>


# <a name="using-nodejs-modules-with-azure-applications"></a>Azure'i rakendustega Node.js moodulid abil

Selles dokumendis on toodud juhised Node.js moodulid kasutamisel majutatud Azure'i rakendustega. See annab juhiseid tagada, et teie rakendus kasutab teatud versiooni mooduli samuti kasutades kohalikke moodulid Azure.

Kui olete juba tuttav, kasutades Node.js moodulid, **package.json** ja **npm-shrinkwrap.json** failid, järgmist on kiire ülevaate, mida selles artiklis kirjeldatud:

* Azure'i rakendust Service mõistab **package.json** ja **npm-shrinkwrap.json** failid ja saate installida moodulid alusel kirjete need failid.
* Azure'i pilveteenustega oodata arenduskeskkond, installitakse kõik moodulid ja **sõlm\_moodulid** directory juurutuse paketti lisada. On võimalik toe installimist moodulid abil **package.json** või **npm-shrinkwrap.json** failide pilveteenustega, kuid selleks, et kohandamine vaikimisi skriptid kasutavad pilveteenusesse projekte. Näide sellest, kuidas täita seda leiate [Azure'i käivitus toimingu käivitada npm installi, et vältida sõlm moodulid juurutamine](https://github.com/woloski/nodeonazure-blog/blob/master/articles/startup-task-to-run-npm-in-azure.markdown)

> [AZURE.NOTE] Azure'i Virtuaalmasinates on kirjeldatud selles artiklis nagu juurutamise kogemus VM sõltuvad korraldaja virtuaalse masina operatsioonisüsteem.

##<a name="nodejs-modules"></a>Node.js moodulid

Moodulid on koormatavate JavaScripti pakette, mille rakenduse teatud funktsioone. Tavaliselt on mooduli installitud **npm** käsurea tööriista abil, kuid mõned (nt http moodul) on esitatud core Node.js paketi.

Moodulid installimisel neid talletatakse selle **sõlm\_moodulid** kataloogi root oma rakenduste kataloogi struktuuri. Iga mooduli sees on **sõlm\_moodulid** directory säilitab Omaette **sõlm\_moodulid** kausta, mis sisaldab see sõltub ja see kordab uuesti alla sõltuvus ahelas iga mooduli moodulid. See võimaldab iga mooduli oma versiooninõuete moodulid see sõltub sellest, kuid see võib põhjustada üsna suure kataloogi struktuuri olema installitud.

Juurutamisel on **sõlm\_moodulid** kataloogi rakenduse osana suureneb mahu juurutamise võrreldes **package.json** või **npm-shrinkwrap.json** faili; siiski tagada, et valmistamisel kasutatud moodulid versiooni on samad, mida kasutatakse arengu.

###<a name="native-modules"></a>Kohalikke moodulid

Kuigi enamik moodulid on vaid Lihttekst JavaScripti failide, on mõned moodulid platvormi kohased kahendarvu pilte. Need moodulid on koostatud installi ajal tavaliselt Python ja sõlm-gyp abil. Kuna Azure'i pilveteenustega selle **sõlm\_moodulid** osana rakendus, mis tahes kohalikke mooduli raames installitud moodulid sisalduv sinna kausta peaks töötama pilveteenus, kui see on installitud ja Windows arendamise süsteemi kohta.

Azure'i rakendust Service ei toeta kõik kohalikke moodulid ja võib nurjuda neile, kellel on väga teatud eeltingimused koostamisel. Mõned populaarsed moodulid nagu MongoDB on valikuline kohalikke sõltuvused ja ilma nendeta hästi, kaks lahendused edukust peaaegu kõigi kohalikke moodulid täna saadaval:

* Käivitage **npm installimine** Windowsi arvutisse, mis sisaldab kõiki kohalikke mooduli eeltingimused installitud. Seejärel juurutada loodud **sõlm\_moodulid** kausta Azure'i rakendust Service rakenduse osana.
* Azure'i rakendust Service saab konfigureerida kohandatud bash või shell skripte käivitada juurutamisel, annab teile võimalust käivitada kohandatud käsud ja konfigureerida täpselt viis **npm installida** käitatakse. Video näitab, kuidas seda teha, leiate teemast [Kohandatud veebisaidi juurutamise skriptide abil Kudu].

###<a name="using-a-packagejson-file"></a>Package.json faili abil

**Package.json** fail on võimalus määrata ülataseme sõltuvused teie rakendus nõuab, et hosting platvormi saate installida sõltuvused selle asemel saate lisada soovitud **sõlm\_pakettide** kausta juurutamise käigus. Pärast kasutusele võetud rakendus, kasutatakse **npm installimine** käsk sõeluda **package.json** fail ja installige kõik sõltuvused, mis on loetletud.

Väljatöötamise käigus, saate kasutada funktsiooni **--salvestamine**, **--Salvesta-arendaja**, või **--Salvesta valikuline** parameetrite installimisel moodulid mooduli kirje lisamine **package.json** faili automaatselt. Lisateavet leiate teemast [npm-installi](https://docs.npmjs.com/cli/install).

Üks võimalik probleem **package.json** fail on, et seda määrab ainult kõrgeima taseme sõltuvuste versiooni. Iga mooduli installitud võib-olla või see sõltub moodulid versiooni ei saa määrata ja seega on võimalik, et teil võib lõpuks erinevate sõltuvus ahelas kui arendatakse kasutatud.

> [AZURE.NOTE]
> Azure'i rakendust Service, et kui <b>package.json</b> faili viitab kohalikke mooduli juurutamisel kuvatakse järgmise sisuga tõrge rakenduse abil Git avaldamisel.

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


###<a name="using-a-npm-shrinkwrapjson-file"></a>Npm-shrinkwrap.json faili abil

**Npm-shrinkwrap.json** fail on katse mooduli versioonimise piirangud **package.json** faili aadress. Ajal **package.json** fail sisaldab ainult kõrgeima taseme moodulid versioone, **npm-shrinkwrap.json** fail sisaldab täielik mooduli sõltuvus ahelas versioon nõuded.

Kui teie rakendus on valmis tootmist, saate lukustada-alla versiooni nõuded ja **npm shrinkwrap** käsuga **npm-shrinkwrap.json** faili loomine. See on kasutada versiooni praegu installitud on **sõlm\_moodulid** kausta ja need **npm-shrinkwrap.json** faili salvestada. Pärast rakenduse kasutusele võetud majutusteenuse keskkonnas, kasutatakse **npm installimine** käsk sõeluda **npm-shrinkwrap.json** fail ja installige kõik sõltuvused, mis on loetletud. Lisateavet leiate teemast [npm-shrinkwrap](https://docs.npmjs.com/cli/shrinkwrap).

> [AZURE.NOTE]
>Azure'i rakendust Service, et kui <b>npm-shrinkwrap.json</b> faili viitab kohalikke mooduli juurutamisel kuvatakse järgmise sisuga tõrge rakenduse abil Git avaldamisel.

>       npm ERR! module-name@0.6.0 install: 'node-gyp configure build'

>       npm ERR! 'cmd "/c" "node-gyp configure build"' failed with 1


##<a name="next-steps"></a>Järgmised sammud

Nüüd, kui teil mõista, kuidas kasutada Node.js moodulid Azure, saate teada, kuidas [määrata Node.js versioon], [luua ja juurutada Node.js web appi]ja [Kuidas kasutada Azure käsurea liides Mac ja Linux].

Lisateavet leiate teemast [Node.js Arenduskeskus](/develop/nodejs/).

[Määrake Node.js versioon]: nodejs-specify-node-version-azure-apps.md
[Kuidas kasutada Azure käsurea liides Mac ja Linux]: xplat-cli-install.md
[luua ja juurutada Node.js web app]: web-sites-nodejs-develop-deploy-mac.md
[Node.js Web Application with Storage on MongoDB (MongoLab)]: store-mongolab-web-sites-nodejs-store-data-mongodb.md
[Build and deploy a Node.js application to an Azure Cloud Service]: cloud-services-nodejs-develop-deploy-app.md
[Kohandatud veebisaidi juurutamise skriptide abil Kudu]: /documentation/videos/custom-web-site-deployment-scripts-with-kudu/
