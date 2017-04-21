#<a name="how-to-use-the-azure-command-line-tools-for-mac-and-linux"></a>Kuidas kasutada Azure käsurea tööriistad Mac ja Linux

Sellest juhendist kirjeldatakse Mac ja Linux Azure'i käsurea tööriistade abil saate luua ja hallata Azure teenused. Stsenaariumid, mis hõlmab kaasata **installimist tööriistu**, **importida oma avaldamise sätted**, **loomise ja haldamise Azure veebisaitide**ja **loomise ja haldamise Azure'i Virtuaalmasinates**. Täielik dokumentides, leiate [Azure'i käsurea tööriista for Maci ja Linux dokumentatsiooni][reference-docs]. 

##<a name="table-of-contents"></a>Sisukord
* [Mis on Azure käsurea tööriistad Mac ja Linux](#Overview)
* [Kuidas installida Azure käsurea tööriistad Mac ja Linux](#Download)
* [Azure'i konto loomise kohta](#CreateAccount)
* [Kuidas alla laadida ja impordi avaldamine sätted](#Account)
* [Kuidas luua ja hallata ka Azure veebisaidi](#WebSites)
* [Kuidas luua ja hallata ka Azure virtuaalse masina](#VMs)


<h2><a id="Overview"></a>Mis on Azure käsurea tööriistad Mac ja Linux</h2>

Azure'i käsurea tööriistad Mac ja Linux on käsurea tööriistade kasutamise ja haldamise Azure'i teenuste komplekt.
 
Toetatud tööülesanded on järgmised:

* Importida avaldamise sätted.
* Luua ja hallata Azure veebisaitide.
* Luua ja hallata Azure'i Virtuaalmasinates.

Tippige toetatud käsud täieliku loendi `azure -help` käsurida pärast installimist menüü Tööriistad või uurige [dokumentides][reference-docs].

<h2><a id="Download">Kuidas installida Azure käsurea tööriistad Mac ja Linux</a></h2>

Järgmine loend sisaldab teavet installimisel käsurea tööriistu, olenevalt teie operatsioonisüsteemist:

* **Mac-arvutis**: [Azure'i SDK Installeri]allalaadimine[mac-installer]. .Pkg allalaaditud faili avamiseks ja kui teilt küsitakse installimise juhiseid täita.

* **Linux**: [Node.js] uusima versiooni installimiseks[ nodejs-org] (vt [Installida Node.js kaudu Package Manager][install-node-linux]), käivitage järgmine käsk:

        npm install azure-cli -g

    **Märkus**: võimalik, et see käsk Käivita administraatoriõigustega peate:

        sudo npm install azure-cli -g

* **Windowsi**: käivitada Winows installer (MSI-faili), mis on saadaval siin: [Azure'i käsurea tööriistade][windows-installer].


Tippige soovitud installi testimine `azure` käsuviibale. Kui installimine õnnestus, kuvatakse kõigi saadaolevate `azure` käsud.

<h2><a id="CreateAccount"></a>Azure'i konto loomise kohta</h2>

Azure'i käsurea tööriistad Mac ja Linux kasutamiseks peate Azure'i konto.

Avage veebibrauser ja liikuge sirvides [http://www.windowsazure.com] [ windowsazuredotcom] ja **tasuta prooviversioon** paremas ülanurgas nuppu.

![Azure'i veebisait][Azure Web Site]

Järgige konto loomise kohta.

<h2><a id="Account"></a>Kuidas alla laadida ja impordi avaldamine sätted</h2>

Enne alustamist peate esmalt alla laadima ja importida oma avaldamine sätted. See võimaldab teil luua ja hallata Azure teenuste tööriistade abil. Allalaadimiseks oma avaldamine sätted, kasutage funktsiooni `account download` käsk:

    azure account download

See vaikimisi brauseri avamine ja palub teil sisse logida haldusportaali. Pärast sisselogimist, oma `.publishsettings` saavad faili alla laadida. Märkige üles, kuhu fail on salvestatud.

Järgmiseks importimine selle `.publishsettings` fail, käivitage järgmine käsk asendamine `{path to .publishsettings file}` tee koos oma `.publishsettings` faili:

    azure account import {path to .publishsettings file}

Saate eemaldada kõik andmed, mis on talletatud soovitud <code>import</code> käsu abil soovitud <code>account clear</code> käsk:

    azure account clear

Suvandite loendi kuvamiseks `account` käsud, kasutage funktsiooni `-help` suvand:

    azure account -help

Pärast importimist oma avaldamine sätted, peaksite kustutama selle `.publishsettings` faili turvalisuse põhjustel.

> [AZURE.NOTE] Kui impordite avaldamine sätted, on talletatud identimisteabe juurdepääsuks Azure tellimuse oma `user` kausta. Teie `user` operatsioonisüsteem on kaitstud kausta. Kuid on soovitatav, et toiminguid täiendavate krüptimiseks oma `user` kausta. Saate seda teha järgmiselt:    
> 
> - Opsüsteemis Windows, muuta kausta atribuudid või BitLockeri abil.
> - Mac, lülitage sisse FileVault kausta.
> - Ubuntu, kasutage funktsiooni krüptitud Avaleht kataloogi. Muud Linuxi pakuvad võrdväärse funktsioone.

Nüüd olete valmis on loomise ja haldamise Azure veebisaitide ja Azure'i Virtuaalmasinates.  

<h2><a id="WebSites"></a>Kuidas luua ja hallata Azure veebisait</h2>

###<a name="create-a-website"></a>Veebisaidi loomine

Azure'i veebisait esmalt loomiseks tühja directory nimetatakse `MySite` ja selle kataloogi sirvimine.

Käivitage järgmine käsk:

    azure site create MySite --git

See käsk väljund sisaldab vaikimisi äsja loodud veebisaidi URL-i. Funktsiooni `--git` suvand, mis võimaldab kasutada git avaldada oma veebisaidile, luues git hoidlate nii kohaliku rakenduse kataloogi ja teie veebisaidile data Centeri kaudu. Pange tähele, et kui teie kohalikus kaustas pole juba git hoidla, käsk lisab uue serveri osutab hoidla oma veebisaidi andmed keskuses olemasoleva hoidlasse.

Pange tähele, et käivitamiseks on `azure site create` käsu ühte järgmistest suvanditest:

* `--location [location name]`. See suvand võimaldab teil, kus luuakse teie veebisaidi (nt "Lääne meie") andmekeskuse asukoha määramiseks. Kui jätate selle suvandi, siis tuleb promted asukoha valimiseks.
* `--hostname [custom host name]`. Selle suvandi abil saab määrata kohandatud hostname oma veebisaidi jaoks.

Seejärel saate lisada oma veebisaidi kataloogi sisu. Tavaline git voogu kasutamine (`git add`, `git commit`) sisu kinnitamiseks. Kasutada push veebisaidi sisu Azure git järgmine käsk: 

    git push azure master

###<a name="set-up-publishing-from-github"></a>Häälestage GitHub kaudu avaldamine

Pidev avaldamine GitHub hoidla, kasutage funktsiooni `--GitHub` valik saidi loomisel:

    auzre site create MySite --github --githubusername username --githubpassword password --githubrepository githubuser/reponame

Kui teil on kohalik klooni GitHub hoidla või kui teil on ühe remote viitega GitHub hoidla hoidla, see käsk automaatselt avaldada kood GitHub hoidlas saidile. Seejärel lükata GitHub hoidla muudatused automaatselt avaldatakse saidile.

Kui häälestate avaldamise GitHub, on vaikimisi haru, mida kasutatakse juhtslaidi haru. Erinevate haru määramiseks täita teie kohaliku hoidla järgmine käsk:

    azure site repository <branch name>

###<a name="configure-app-settings"></a>Rakenduse sätete konfigureerimine

Võti ja väärtuse paarideks, mis on saadaval rakenduse käitusajal on rakenduse sätted. Kui valitud on Azure veebisaidi jaoks, alistab rakenduse väärtused sätete sama võti, mis on määratletud teie saidi fail. Node.js ja PHP rakenduste, on saadaval keskkonna muutujate rakenduse sätted. Järgmises näites kirjeldatakse, kuidas määrata paari võti-väärtus:

    azure site config add <key>=<value> 

Kõik loendi /-väärtuse paarideks, kasutage järgmist vaatamiseks tehke järgmist.

    azure site config list 

Või kui teate võti ja soovite näha väärtuse, saate kasutada järgmist.

    azure site config get <key> 

Kui soovite muuta mõne olemasoleva võtme väärtus tuleb esmalt tühjendage olemasoleval ja seejärel lisage see uuesti. Nupp Eemalda on:

    azure site config clear <key> 

###<a name="list-and-show-sites"></a>Loendi- ja Kuva saidid

Loendisse veebisaidiga, kasutage järgmist käsku:

    azure site list

Saidi kohta täpsema teabe saamiseks kasutage funktsiooni `site show` käsk. Järgmises näites on kujutatud üksikasjad `MySite`:

    azure site show MySite

###<a name="stop-start-or-restart-a-site"></a>Lõpetada, käivitage või saidi taaskäivitamine

Saate peatada, käivitamisel või taaskäivitage kohas, kus on `site stop`, `site start`, või `site restart` käsud:

    azure site stop MySite
    azure site start MySite
    azure site restart MySite

###<a name="delete-a-site"></a>Saidi kustutamine

Lisaks saate kustutada kohas, kus on `site delete` käsk:

    azure site delete MySite

Pange tähele, et kui teil on mõni Käskude valimiskoht üle kui käivitasite kaustas `site create`, pole vaja määrata saidi nimi `MySite` viimase parameetrina.

Täieliku loendi kuvamiseks `site` käsud, kasutage funktsiooni `-help` suvand:

    azure site -help 

<h2><a id="VMs"></a>Kuidas luua ja hallata ka Azure virtuaalse masina</h2>

mõne Azure virtuaalse masina luuakse virtuaalse masina pildilt (.vhd fail) teie või mida on saadaval Pildigalerii. Lugege teemat Piltide, mis on saadaval, kasutage funktsiooni `vm image list` käsk:

    azure vm image list

Saate ette valmistada ja käivitada ühest saadaval pildid koos selle `vm create` käsk. Järgmises näites kujutatakse luua Linux virtuaalse masina (nimetatakse `myVM`) kaudu pildi Pildigalerii pilt (CentOS 6.2). Juurkausta kasutajanime ja parooli virtuaalse masina on `myusername` ja `Mypassw0rd` vastavalt. (Pange tähele, et selle `--location` , kus on loodud virtuaalse masina andmekeskuse täpsustav parameeter. Kui argument on `--location` parameeter, palutakse teil asukoha valimine.)

    azure vm create myVM OpenLogic__OpenLogic-CentOS-62-20120509-en-us-30GB.vhd myusername --location "West US"

Võite kaaluda läbimise soovitud `--ssh` lipp (Linux) või `--rdp` lipp (Windows) `vm create` vastloodud virtuaalne arvutisse allalaadimise lubamiseks.

Kui olete sooviksite pigem ette virtuaalse masina kohandatud pildilt, saate luua pilt .vhd faili selle `vm image create` käsk ja seejärel kasutage funktsiooni `vm create` käsk ettevalmistamise virtuaalse masina. Järgmises näites kujutatakse loomiseks Linux pilt (nimetatakse `myImage`) kohaliku .vhd faili. (Selle `--location` andmed, kus on salvestatud pildi täpsustav parameeter.)

    azure vm image create myImage /path/to/myImage.vhd --os linux --location "West US"

Asemel kohalik .vhd pildi loomist saate luua .vhd, mis on talletatud Azure'i bloobimälu pilt. Saate seda teha on `blob-url` parameeter:

    azure vm image create myImage --blob-url <url to .vhd in Blob Storage> --os linux

Pärast pildi loomist saate ettevalmistamise virtuaalse masina pildilt, kasutades `vm create`. Käsu alla loob virtuaalse masina nimetatakse `myVM` kohal loodud pilt (`myImage`).

    azure vm create myVM myImage myusername --location "West US"

Pärast on ette valmistatud virtuaalse masina, võite luua virtuaalne arvuti remote juurdepääsu lubamiseks (näiteks) lõpp-punktid. Järgmises näites kasutatakse funktsiooni `vm create endpoint` välise port 22 ja kohalik port 22 avamiseks klõpsake käsu `myVM`:

    azure vm endpoint create myVM 22 22

Üksikasjalikku teavet virtuaalse masina (sh IP-aadress, DNS-i nimi ja lõpp-punkti teave) abil saate soovitud `vm show` käsk:

    azure vm show myVM

Sulgeda, käivitamine, või taaskäivitage virtuaalse masina, kasutage ühte järgmistest käskudest:

    azure vm shutdown myVM
    azure vm start myVM
    azure vm restart myVM

Ja lõpuks VM kustutada, kasutage funktsiooni `vm delete` käsk:

    azure vm delete myVM

Täieliku loendi loomiseks ja haldamiseks virtuaalmasinates käsud, kasutage funktsiooni `-h` suvand:

    azure vm -h

<!-- IMAGES -->
[Azure Web Site]: ./media/crossplat-cmd-tools/freetrial.png

<!-- LINKS -->
[nodejs-org]: http://nodejs.org/
[install-node-linux]: https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager
[mac-installer]: http://go.microsoft.com/fwlink/?LinkId=252249
[windows-installer]: http://go.microsoft.com/fwlink/?LinkID=275464
[reference-docs]: http://go.microsoft.com/fwlink/?LinkId=252246
[windowsazuredotcom]: http://www.windowsazure.com

