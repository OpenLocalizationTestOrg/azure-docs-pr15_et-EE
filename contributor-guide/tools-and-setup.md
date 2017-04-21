<properties
pageTitle="Installimine ja häälestamine loome ka GitHub tööriistad"
description="Tööriistad ja saada seadistatud loome Azure sisu GitHub juhiseid."
services="contributor-guide"
documentationCenter=""
authors="tysonn"  
manager="carolz" />

<tags
ms.service="contributor-guide"
 ms.devlang=""
 ms.topic="article"
  ms.tgt_pltfrm=""
  ms.workload=""
  ms.date="01/19/2015"
  ms.author="tysonn" />

#<a name="install-and-set-up-tools-for-authoring-in-github"></a>Installimine ja häälestamine loome ka GitHub tööriistad

Järgige selle artikli tööriistad kaasa Azure dokumentatsioon häälestamiseks. Juhuslik ja aeg-ajalt osaliste tõenäoliselt saate kasutada GitHub UI kirjeldatud etappi 2.

Kui te pole varem selliseid Git, võiksite üle vaadata mõned Git terminoloogia: [https://help.github.com/articles/github-glossary](https://help.github.com/articles/github-glossary). Lisaks selles teemas StackOverflow sisaldab teil tekivad selles teemas kirjeldatud toimingutega Git terminite sõnastik: [http://stackoverflow.com/questions/7076164/terminology-used-by-git](http://stackoverflow.com/questions/7076164/terminology-used-by-git)

## <a name="contents"></a>Sisu

- [Looge konto GitHub ja profiili häälestamine](#create-a-github-account-and-set-up-your-profile)
- [Disqus kasutajaks registreerumine](#sign-up-for-disqus)
- [Määratlemine, kas vaja järgida ülejäänud järgmiselt.](#determine-whether-you-really-need-to-follow-the-rest-of-these-steps)
- [GitHub õigused](#permissions-in-github)
- [Git for Windowsi installimine](#install-git-for-windows)
- [Kahekordne autentimine](#enable-two-factor-authentication)
- [Installige allahindlusest redaktor](#install-a-markdown-editor)
- [Atom konfigureerimine](#configure-atom)
- [Hoidla kahvli ja kopeerige see oma arvutisse](#fork-the-repository-and-copy-it-to-your-computer)
- [Oma kasutajanimi ja e-posti kohalikult konfigureerimine](#configure-your-user-name-and-email-locally)
- [Järgmised sammud](#next-steps)

## <a name="create-a-github-account-and-set-up-your-profile"></a>Looge konto GitHub ja profiili häälestamine

Anda oma panus Azure tehnilise sisu, peate [GitHub](http://www.github.com) konto.

Kui kasutate Microsoft kaasautor, peate GitHub konto häälestamine nii, et te olete selgelt, Microsoft töötaja. Seadistage oma profiili järgmiselt:

- **Profiilipildi**: pildi teha (nõutav)
- **Nimi**: teie ees- ja perekonnanimi (nõutav)
- **E-posti**: oma Microsofti meiliaadressi (valikuline)
- **Ettevõtte**: Microsoft Corporation (nõutav)
- **Asukoht**: asukoha (valikuline) loend

Profiili näeb seda profiili:

<p align="center">
 ![GitHub profiili näide](./media/tools-and-setup/githubprofile.png)

## <a name="sign-up-for-disqus"></a>Disqus kasutajaks registreerumine

Iga avaldatud Azure tehniline artikkel sisaldab kommentaari voo Disqus teenuse.

 ![Kettaheites logo](./media/tools-and-setup/discus.png)

Kui teil on Microsoft töötaja ja kui olete autori või artikkel kaasautor, peate nii, et osalete kommentaari voo artikkel Disqus kasutajaks.

1. [Http://www.disqus.com/](http://www.disqus.com/) konto kasutajaks registreerumine
2. Täitke oma profiili järgmiselt:

 - **Täielik nimi**: oma täisnimi kuvatav oma Microsofti meiliaadressi aadressiraamatu kirjete ja lisaks sulgudes teave, mis on teie pseudonüümi pluss @MSFT. Vorming: *esimene Viimane [alias@MSFT] *
 - **Asukoht**: asukoha
 - **Lühielulugu**: oma tiitel

## <a name="determine-whether-you-really-need-to-follow-the-rest-of-these-steps"></a>Määratlemine, kas soovite tõesti järgige neid juhiseid ülejäänud

Pole peate järgima kõiki selles artiklis toodud juhiseid. See sõltub sellist tüüpi sisu osakaal soovite või on vaja teha.

###<a name="submit-a-text-only-change-to-an-existing-article"></a>Ainult teksti muuta olemasolevaid artikkel edastamine

Kui ainult vaja või soovite teha teksti värskendusi olemasoleva artikkel, ilmselt ei peate täitke ülejäänud juhised. Saate esitada muudatuste GitHub on veebipõhine allahindlusest redaktor. Klõpsake lihtsalt GitHub linki artikli, mida soovite muuta.

 ![GitHub profiili näide](./media/tools-and-setup/contributetogit.png)

 Klõpsake redigeerimisikooni artiklist GitHub versioon

 ![GitHub profiili näide](./media/tools-and-setup/editicon.PNG)

 Mis avaneb lihtne kasutada web editor, mis lihtsustab muudatused esitada. Teil pole vaja järgida selles artiklis toodud juhiseid.

###<a name="all-other-changes"></a>Kõik muud muudatused
GitHub UI ei toeta uute failide ja piltide pukseerimine loomine. UI töötamisel harude haldamine saate siiski selgem seega soovitame tavaliselt installimist tööriistade ja käskude loomise ja haldamise artikleid lugege. Kui soovite kasutada UI, siis lugege:

- [Github failide loomine](https://github.com/blog/1327-creating-files-on-github)
- [Laadige failid üles oma hoidlate](https://github.com/blog/2105-upload-files-to-your-repositories)

Jaoks järgmised sorditakse töö, Soovitame installida ja saate teada, kuidas kasutada tööriistu:

 - Artikli põhjalikumat muutmist
 - Loomise ja avaldamisega uus artikkel
 - Uute piltide lisamine või värskendamine pildid
 - Artikli avaldamise muudatused nende päevade päeva jooksul värskendamine
 - Väljaande, mis sisaldab teatud päeval teatud ajal minna sisu loomine

##<a name="permissions-in-github"></a>GitHub õigused

Igaüks GitHub kontoga saate kaasa Azure tehnilise sisu meie avaliku hoidla veebisaidil [https://github.com/Azure/azure-content](https://github.com/Azure/azure-content)kaudu. Vajalike teisiti õigusi pole.

Kui teil on Microsoft PM või kirjutaja, kes ei tööta Azure sisu, peate töötama meie era sisu hoidla azure-sisu-hind. Külastage [https://repos.opensource.microsoft.com](https://repos.opensource.microsoft.com ) taotlemiseks vt õigusetasemeid, mis annab teile tasuma kaudu privaatne repo - GitHub nupu abil sisse logida > klõpsake Azure > nuppu **Liitu meeskond** või **mõne muu meeskonda**, ja seejärel otsige ja **azure-sisu-Loe** rühmaga liituda.

## <a name="install-git-for-windows"></a>Git for Windowsi installimine

Installige Windowsi Git [http://git-scm.com/download/win](http://git-scm.com/download/win). Selle alla installib Git versiooni kontrolli süsteem ja selle installib Git Bash, käsurea rakendus, mida te kasutate suhelda koos teie kohaliku Git hoidla.

Võite nõustuda vaikesätteid; Kui soovite olla kättesaadav käsurea Windows käsud, valige suvand, mis võimaldab seda.

<p align="center">
 ![GitHub profiili näide](./media/tools-and-setup/gitbashinstall.png)

(Märkus: see pole sama, mis "Github for Windows". "Github Windowsi" on erinevate põhise tööriista, mis töötab ka siis, kui soovite lugeda üles ise. [https://windows.github.com/](https://windows.github.com/))

## <a name="enable-two-factor-authentication"></a>Kahekordne autentimine

Teil on kaks tegurit autentimise (2FA) GitHub kontol kui töötate privaatne sisu hoidla. See on nõutav privaatne hoidla.

Selle lubamiseks järgige nii GitHub teemadest:

- [Teave kahekordne autentimine](https://help.github.com/articles/about-two-factor-authentication/)
- [Juurdepääsu sümboolse käsurea kasutamiseks loomine](https://help.github.com/articles/creating-an-access-token-for-command-line-use/)

Kui loote luba, valige kõik otsinguulatuste, mis on saadaval luba loomise UI ([iga ulatus üksikasjad](https://developer.github.com/v3/oauth/#scopes))

Pärast 2FA lubamiseks peate sisestama juurdepääsu luba parooli GitHub käsuviibale asemel, kui proovite juurdepääs GitHub hoidla käsurea kaudu. Luba juurdepääs pole autentimise koodi, mis kuvatakse tekstsõnum 2FA häälestamisel. See on pikk string, mis näeb välja umbes selline: fdd3b7d3d4f0d2bb2cd3d58dba54bd6bafcd8dee. Mõned märkused selle kohta.

- Kui loote oma juurdepääsu luba, salvestage see tekstifail oleks kergesti juurdepääsetavad, kui neid vajate.

- Hiljem, kui teil on vaja kleepida luba, leida, on kaks võimalust käsureal kleepimiseks:

 - Klõpsake käsurea akna vasakus ülanurgas ikooni > redigeerimine > Kleebi.
 - Paremklõpsake akna vasakus ülanurgas ikooni ja valige Atribuudid > Suvandid > klõpsakeQuickEdit režiim. See konfigureerib käsurea, paremklõpsates käsurea aknas kleepida.

## <a name="install-a-markdown-editor"></a>Installige allahindlusest redaktor

Me Autor sisu, kasutades lihtsat "allahindlusest" esituses pigem failid, kui kompleksarvu "märgistus" (HTML XML,). Peate installima allahindlusest Editori.

- **Atom**: enamik meie GitHub's Atomi allahindlusest redaktori kasutamine: [http://atom.io](http://atom.io). See ei nõua business kasutamiseks litsentsi. See sisaldab õigekirjakontrolli.

- **Notepad**: saate kasutada Notepad väga kerge suvandi.

- **Proosa**: see on kerge, elegantne, Interneti ja avatud lähtekoodi allahindlusest redaktor, mis pakub eelvaade. Külastage [http://prose.io](http://prose.io) ja proosa oma hoidlas autoriseerida.

- **[Visual Studio kood](https://www.visualstudio.com/products/code-vs.aspx)** - Microsofti kirje siia.

## <a name="configure-atom"></a>Atom konfigureerimine

Kui kasutate Atomi, peate häälestamine paari asja.

- Atom on vaikimisi 2 tühikuid vahekaartide abil, kuid allahindlusest eeldab, et 4 tühikuid. Kui jätate kahe vaikeväärtused, oma artiklis näeb välja hea kohaliku eelvaates, aga kui ta on importimata Azure. Nii, et konfigureerida Atomi kasutada 4 tühikud – selle sätte leidmiseks klõpsake menüü Fail > Sätted > redaktori sätted > menüü pikkus.
- Siis ilmselt ka soovite liiga, pehmendatud Murra selles jaotises sisse lülitada mis teeb sama nimega "Murra" Notepad.
- Allahindlusest eelvaade, klõpsake suvandit pakettide > allahindlusest eelvaade > Lülita eelvaade. Saate Ctrl-Shift-M sisse-või väljalülitamiseks eelvaate HTML-i vaade.

## <a name="fork-the-repository-and-copy-it-to-your-computer"></a>Hoidla kahvli ja kopeerige see oma arvutisse

1. Luua hoidla kahvel GitHub – valige lehe paremas ülanurgas ja klõpsake nuppu kahvel. Kui kuvatakse vastav viip, valige asukoht, kuhu kahvel peaks olema teie konto. See loob hoidla kontol Git jaoturi koopia. Üldiselt tehnilise poolt ja programmi juhid vaja kahvel azure-sisu-hind privaatne repo. Ühenduse osaliste vaja avaliku repo kahvel azure-sisu. Teil on vaja ainult kahvli üks kord; pärast esimese häälestust, kui soovite kopeerida mõnda teise arvutisse, toidulauani ainult teil käivitamiseks järgige selle jaotise repo arvutisse kopeerida käsud.  Kui valite kahvlid, nii hoidlate loomiseks, peate kahvel iga hoidla jaoks luua.

2. Kopeerige soovitud isikliku Accessi Turbeloa mis teil [https://github.com/settings/tokens](https://github.com/settings/tokens)kaudu. Võite nõustuda vaikimisi õiguste luba.  Isikliku Accessi Turbeloa tekstifaili hiljem uuesti kasutamiseks salvestada.

3. Seejärel kopeerige hoidla arvutiga manustatud käsk stringi mandaadiga.  Selle tegemiseks avage Git Bash ja käivitage see administraatorina. Käsuviip ja sisestage järgmine käsk.  See käsk loob azure-content(-pr) kausta teie arvutis.  Kui kasutate vaikeasukohta, oleks veebisaidil c:\users<your Windows user name>\azure-content(-pr).

Avaliku repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content.git

Privaatne repo:

        git clone https://[your GitHub user name]:[token]@github.com/<your GitHub user name>/azure-content-pr.git

Näiteks võib selle käsu klooni välja nägema umbes selline:

        git clone https://smithj:b428654321d613773d423ef2f173ddf4a312345@github.com/smithj/azure-content-pr.git  

## <a name="set-remote-repository-connection-and-configure-credentials"></a>Määrake serveri hoidla ühendus ja mandaadi konfigureerimine

Hoidla root viite loomine sisestate need käsud. See loob ühendused hoidlasse ka GitHub, et saaksite teie kohalikku arvutisse viimaste muudatuste kohta ja push muudatused tagasi github. See käsk konfigureerib ka oma luba kohalikult nii, et teil pole vaja iga kord, kui varustava repo ja toidulauani github avamisel sisestage oma nimi ja parool.

Avaliku repo:

        cd azure-content
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content.git
        git fetch upstream

Privaatne repo:

        cd azure-content-pr
        git remote add upstream https://[your GitHub user name]:[token]@github.com/Azure/azure-content-pr.git
        git fetch upstream

Selleks kulub tavaliselt aega. Pärast seda, pole teil kahvli uuesti või sisestage oma kasutajanimi ja parool uuesti. Ainult teil kopeerida forks kohalikku arvutisse uuesti, kui häälestate tööriistade mõnes muus arvutis.


## <a name="configure-your-user-name-and-email-locally"></a>Oma kasutajanimi ja e-posti kohalikult konfigureerimine

Veenduge, et teil on loetletud õigesti kaasautor, peate oma kasutajanimi ja e-posti kohalikult git konfigureerimine.

1. Käivitage Git Bash ja vahetada azure-sisu või azure-sisu-hind:

   ````
   cd azure-content
   ````

 või

   ````
   cd azure-content-pr
   ````

2. Nii, et see sarnaneks oma nimi, kui häälestate seda profiili GitHub, konfigureerida oma kasutajanimi.

    ````
    git config --global user.name "John Doe"
    ````
3. Konfigureerimine oma e-posti, et see sarnaneks esmane e-posti profiili GitHub määratud Kui olete MSFT töötaja, peaks olema MSFT e-posti aadressi:

    ````
    git config --global user.email "alias@example.com"
    ````
4. Tippige `git config -l` ja vaadake üle kohaliku sätteid nii, et tagada kasutajanimi ja e-posti konfiguratsiooni on õiged.

##<a name="next-steps"></a>Järgmised sammud

- Sisutüübi tehnilise sisu repo kuuluva mõistan, ja teada, mis ei kuulu. Vt [sisu kanali juhised](./content-channel-guidance.md)!
- [Järgmiste](./git-commands-for-master.md)juhiste abil luua või muuta artikli ja seejärel esitab avaldamiseks.
- Kopeerige [allahindlusest malli](../markdown templates/markdown-template-for-new-articles.md) alusena uus artikkel.
- Ühendamiseks mõeldud kasutada [kuvatakse vastavad järgmise kontroll-loendi saamiseks kinnitada](./contributor-guide-pr-criteria.md) .


###<a name="contributors-guide-navigation"></a>Osaliste juhend navigeerimine

- [Artikli ülevaade](./../README.md)
- [Juhised artiklite register](./contributor-guide-index.md)



<!--Anchors-->
[Use a customer-friendly voice]: #use-a-customer-friendly-voice
[Consider localization and machine translation]: #consider-localization-and-machine-translation
[other style and voice issues to watch for]: #other-style-and-voice-issues-to-watch-for


[Create a GitHub account and set up your profile]: #create-a-github-account-and-set-up-your-profile
[Determine whether you really need to follow the rest of these steps]: #determine-whether-you-really-need-to-follow-the-rest-of-these-steps
[Permissions in GitHub]: #permissions-in-github
[Install Git for Windows]: #install-git-for-windows
[Enable two-factor authentication]: #enable-two-factor-authentication
[Install a markdown editor]: #install-a-markdown-editor
[Fork the repository and copy it to your computer]: #fork-the-repository-and-copy-it-to-your-computer
[Install git-credential-winstore]: #install-git-credential-winstore
[Sign up for Disqus]: #sign-up-for-disqus
[Configure your user name and email locally]: #configure-your-user-name-and-email-locally
[Next steps]: #next-steps
